# Corelink UE5 Character Movement Component 


### **Corelink Integration Overview**
Corelink is a high-performance networking library designed for real-time data exchange. To integrate it into Unreal Engine for seamless use within game projects, we need to:
1. Replace Unreal’s default netcode with Corelink for specific functionalities (e.g., character movement).
2. Use Corelink’s sender/receiver streams to transmit and receive data.
3. Ensure compatibility with Unreal’s gameplay framework (e.g., `UCharacterMovementComponent`).

---



### **Replace Unreal’s Netcode with Corelink**
#### **Override `UCharacterMovementComponent`**
Create a custom movement component that uses Corelink for networking.

```cpp
UCLASS()
class CORELINKPLUGIN_API UCorelinkCharacterMovementComponent : public UCharacterMovementComponent
{
    GENERATED_BODY()

public:
    virtual void ServerMove_PerformMovement(const FCharacterNetworkMoveData& MoveData) override;
    virtual void ClientAdjustPosition_Implementation(...) override;

private:
    void SendMovementData(const FCharacterNetworkMoveData& MoveData);
    void OnMovementDataReceived(const TArray<uint8>& Data);
};
```

#### **Serialize and Send Movement Data**
```cpp
void UCorelinkCharacterMovementComponent::SendMovementData(const FCharacterNetworkMoveData& MoveData)
{
    // Serialize movement data into a TArray<uint8>
    TArray<uint8> Payload;
    FMemoryWriter Writer(Payload);
    MoveData.NetworkSerialize(Writer);

    // Send via Corelink
    UCorelinkSubsystem* CorelinkSubsystem = GetWorld()->GetSubsystem<UCorelinkSubsystem>();
    CorelinkSubsystem->SendMovementData(Payload, /*ChannelId*/);
}
```

#### **Handle Received Movement Data**
```cpp
void UCorelinkCharacterMovementComponent::OnMovementDataReceived(const TArray<uint8>& Data)
{
    // Deserialize movement data
    FCharacterNetworkMoveData MoveData;
    FMemoryReader Reader(Data);
    MoveData.NetworkSerialize(Reader);

    // Apply movement data
    ClientAdjustPosition_Implementation(
        MoveData.TimeStamp,
        MoveData.Accel,
        MoveData.ClientLoc,
        MoveData.ClientVel,
        MoveData.ClientBase,
        MoveData.ClientBaseBoneName,
        MoveData.ClientMovementMode
    );
}
```

---

### **Corelink Data Channels**
#### **Create a Movement Data Channel**
```cpp
void UCorelinkSubsystem::CreateMovementDataChannel()
{
    auto CreateSenderRequest = MakeShared<corelink::client::request_response::requests::modify_sender_stream_request>(
        corelink::core::network::constants::protocols::udp
    );
    CreateSenderRequest->workspace = "GameMovement";
    CreateSenderRequest->stream_type = "CharacterMovement";
    CreateSenderRequest->on_init = [this](corelink::core::network::channel_id_type ChannelId)
    {
        MovementDataChannelId = ChannelId;
        UE_LOG(LogTemp, Log, TEXT("Movement data channel created: %d"), ChannelId);
    };

    CorelinkClient.request(
        ControlChannelId,
        corelink::client::corelink_functions::create_sender,
        CreateSenderRequest,
        [](corelink::core::network::channel_id_type ChannelId, const std::string& Message,
           TSharedPtr<corelink::client::request_response::responses::corelink_server_response_base> Response)
        {
            UE_LOG(LogTemp, Log, TEXT("Sender stream created."));
        }
    );
}
```

#### **Send and Receive Data**
```cpp
void UCorelinkSubsystem::SendMovementData(const TArray<uint8>& Data, int32 ChannelId)
{
    CorelinkClient.send_data(ChannelId, Data, corelink::utils::json());
}

void UCorelinkSubsystem::OnCorelinkDataReceived(const TArray<uint8>& Data, int32 ChannelId)
{
    // Forward received data to the movement component
    UCorelinkCharacterMovementComponent* MovementComponent = /* Get the component */;
    MovementComponent->OnMovementDataReceived(Data);
}
```

---

### **Testing and Debugging**
- **Simulate Network Conditions**: Use tools like `NetworkEmulation` to test latency and packet loss.
- **Validate Data Sync**: Ensure movement data is correctly synchronized between client and server.
- **Profile Performance**: Monitor bandwidth and CPU usage to ensure Corelink handles real-time demands.


