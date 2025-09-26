# Corelink UE5 Netcode Replication


## Steps to Implement

- Implement `UCorelinkSubsystem` (`UWorldSubsystem`)
	- Create a **`UWorldSubsystem`** to manage Corelink globally.
	- Handle Movement Data in `UCorelinkSubsystem`
- Create `UCorelinkNetDriver` to Replace Unreal’s netcode
	- Subclass `UIpNetDriver` to create `UCorelinkNetDriver`.
	- Replace Unreal’s default `NetDriver` with Corelink in `DefaultEngine.ini`.
- Expose to Blueprints
	- Provide `UCorelinkReplicationComponent` as a Blueprint-enabled component.
	- Developers can attach it to actors without writing C++.


## Subsystem vs NetDriver


While both `UCorelinkSubsystem::Initialize` and `UCorelinkNetDriver::InitBase` connect to Corelink, their **purposes** differ:

1. `UCorelinkSubsystem::Initialize`
    - Initializes Corelink globally.
    - Manages **control channels**.
    - Handles **authentication**.
    - Sends specific **game-related** messages.
      
2. `UCorelinkNetDriver::InitBase`
    - Sets up Corelink **for networking only**.
    - Manages **network channels**.
    - Sends and receives **Unreal network packets**.
    - Does **not** handle authentication (relies on the subsystem).

This separation allows **Corelink to work globally** while still **integrating with Unreal’s networking stack**.


## Corelink Subsystem
#### **Create a Corelink Subsystem**
A subsystem ensures Corelink is initialized and managed globally within the game.

```cpp
UCLASS()
class CORELINKPLUGIN_API UCorelinkSubsystem : public UWorldSubsystem
{
    GENERATED_BODY()

public:
    virtual void Initialize(FSubsystemCollectionBase& Collection) override;
    virtual void Deinitialize() override;

    void SendMovementData(const TArray<uint8>& Data, int32 ChannelId);
    void OnCorelinkDataReceived(const TArray<uint8>& Data, int32 ChannelId);

private:
    corelink::client::corelink_classic_client CorelinkClient;
    corelink::core::network::channel_id_type ControlChannelId;
    corelink::core::network::channel_id_type MovementDataChannelId;

    void AuthenticateWithCorelink();
    void CreateMovementDataChannel();
};
```

#### **Initialize Corelink**
In `Initialize`, set up the Corelink client and authenticate.

```cpp
void UCorelinkSubsystem::Initialize(FSubsystemCollectionBase& Collection)
{
    Super::Initialize(Collection);

    // Initialize Corelink protocols
    if (!CorelinkClient.init_protocols())
    {
        UE_LOG(LogTemp, Error, TEXT("Failed to initialize Corelink protocols."));
        return;
    }

    // Set up control channel
    corelink::client::corelink_client_connection_info ConnectionInfo(
        corelink::core::network::constants::protocols::tcp
    );
    ConnectionInfo
        .set_endpoint("corelink.hsrn.nyu.edu")
        .set_port_number(20012)
        .set_username("default_username")
        .set_password("default_password");

    ControlChannelId = CorelinkClient.add_control_channel(
        ConnectionInfo,
        [](corelink::core::network::channel_id_type ChannelId, const std::string& Message)
        {
            UE_LOG(LogTemp, Error, TEXT("Control channel error: %s"), *FString(Message.c_str()));
        },
        [](corelink::core::network::channel_id_type ChannelId)
        {
            UE_LOG(LogTemp, Log, TEXT("Control channel connected: %d"), ChannelId);
        },
        [](corelink::core::network::channel_id_type ChannelId)
        {
            UE_LOG(LogTemp, Log, TEXT("Control channel disconnected: %d"), ChannelId);
        }
    );

    // Authenticate with Corelink
    AuthenticateWithCorelink();
}
```

#### **Authenticate with Corelink**
```cpp
void UCorelinkSubsystem::AuthenticateWithCorelink()
{
    CorelinkClient.request(
        ControlChannelId,
        corelink::client::corelink_functions::authenticate,
        MakeShared<corelink::client::request_response::requests::authenticate_client_request>(
            "default_username", "default_password"),
        [](corelink::core::network::channel_id_type ChannelId, const std::string& Message,
           TSharedPtr<corelink::client::request_response::responses::corelink_server_response_base> Response)
        {
            if (Response->status_code != 0)
            {
                UE_LOG(LogTemp, Error, TEXT("Authentication failed: %s"), *FString(Response->message.c_str()));
                return;
            }
            UE_LOG(LogTemp, Log, TEXT("Authentication successful."));
        }
    );
}
```


## Corelink NetDriver
#### **Create a Corelink NetDriver**

The `UCorelinkNetDriver` replaces Unreal’s default `UIpNetDriver` to route network traffic through Corelink.

```cpp
UCLASS()
class CORELINKPLUGIN_API UCorelinkNetDriver : public UIpNetDriver
{
    GENERATED_BODY()

public:
    virtual bool InitBase(UNetDriver::FNetworkNotify* InNotify, FSocketSubsystem* InSocketSubsystem, const FURL& InURL, bool bReuseAddressAndPort, FString& Error) override;
    virtual void LowLevelSend(FSocket* Socket, uint8* Data, int32 Count) override;
    virtual void TickDispatch(float DeltaTime) override;

private:
    corelink::client::corelink_classic_client CorelinkClient;
    corelink::core::network::channel_id_type NetChannelId;

    void ConnectToCorelink();
    void HandleIncomingData(const TArray<uint8>& Data);
};
```

---

#### **Initialize Corelink NetDriver**

In `InitBase`, initialize the Corelink client and set up the network channel.

```cpp
bool UCorelinkNetDriver::InitBase(UNetDriver::FNetworkNotify* InNotify, FSocketSubsystem* InSocketSubsystem, const FURL& InURL, bool bReuseAddressAndPort, FString& Error)
{
    if (!Super::InitBase(InNotify, InSocketSubsystem, InURL, bReuseAddressAndPort, Error))
    {
        return false;
    }

    // Initialize Corelink client
    if (!CorelinkClient.init_protocols())
    {
        Error = TEXT("Failed to initialize Corelink protocols.");
        return false;
    }

    // Connect to Corelink
    ConnectToCorelink();

    return true;
}
```

---

#### **Connect to Corelink**

Establishes a Corelink connection and creates a network channel.

```cpp
void UCorelinkNetDriver::ConnectToCorelink()
{
    corelink::client::corelink_client_connection_info ConnectionInfo(
        corelink::core::network::constants::protocols::tcp
    );

    ConnectionInfo
        .set_endpoint("corelink.hsrn.nyu.edu")
        .set_port_number(20012)
        .set_username("default_username")
        .set_password("default_password");

    NetChannelId = CorelinkClient.add_control_channel(
        ConnectionInfo,
        [](corelink::core::network::channel_id_type ChannelId, const std::string& Message)
        {
            UE_LOG(LogTemp, Error, TEXT("Corelink network error: %s"), *FString(Message.c_str()));
        },
        [](corelink::core::network::channel_id_type ChannelId)
        {
            UE_LOG(LogTemp, Log, TEXT("Corelink network connected: %d"), ChannelId);
        },
        [](corelink::core::network::channel_id_type ChannelId)
        {
            UE_LOG(LogTemp, Log, TEXT("Corelink network disconnected: %d"), ChannelId);
        }
    );
}
```

---

### **Send Network Data via Corelink**

In `LowLevelSend`, override Unreal’s default send function to route packets through Corelink.

```cpp
void UCorelinkNetDriver::LowLevelSend(FSocket* Socket, uint8* Data, int32 Count)
{
    if (NetChannelId == corelink::core::network::constants::invalid_channel_id)
    {
        UE_LOG(LogTemp, Warning, TEXT("CorelinkNetDriver: No active network channel."));
        return;
    }

    TArray<uint8> PacketData;
    PacketData.Append(Data, Count);

    CorelinkClient.send(NetChannelId, PacketData.GetData(), PacketData.Num());
}
```

---

#### **Handle Incoming Data from Corelink**

Process received data and forward it to Unreal’s networking stack.

```cpp
void UCorelinkNetDriver::HandleIncomingData(const TArray<uint8>& Data)
{
    if (Data.Num() == 0)
    {
        return;
    }

    FReceivedPacket Packet;
    Packet.Data.Append(Data);
    Packet.Address = FInternetAddrCorelink::Create(); // Simulated address

    // Notify Unreal networking layer of incoming data
    Notify->NotifyReceiveData(this, Packet.Address, Packet.Data.GetData(), Packet.Data.Num());
}
```

---

#### **Poll Corelink for Incoming Packets**

In `TickDispatch`, poll for network events and process received packets.

```cpp
void UCorelinkNetDriver::TickDispatch(float DeltaTime)
{
    Super::TickDispatch(DeltaTime);

    CorelinkClient.poll([this](corelink::core::network::channel_id_type ChannelId, const void* Data, std::size_t Size)
    {
        if (ChannelId == NetChannelId)
        {
            TArray<uint8> ReceivedData;
            ReceivedData.Append(static_cast<const uint8*>(Data), Size);
            HandleIncomingData(ReceivedData);
        }
    });
}
```



