# Corelink Concurrency
#cs #gamedev 

## Threads

Thread initialized in `corelink_data_xchg_raw_socket_protocol_context_manager.cc`:

- On `context_manager` initialization, we add threads to the pool and initialize them
- These threads are then essentially **daemonised**
	- This means that it won't prevent the program from exiting and will run in the background until all non-daemon threads are complete.
- Threads are initialized using `std::thread`, which is redefined under the `corelink_thread` alias

```cpp
start_context_manager()
{
// if there is at least one thread running, don't restart threads or add more
	if (m_context_runner_thread_running)
		return;
	for (size_t count = 0; count < m_concurrency_level; ++count)
	{
		 // add a thread to the pool
		corelink_thread th(
				[&]()
				{
					// thread is running. mark it
					m_context_runner_thread_running = true;
			// this call will block till ALL the work in the ASIO queue is done.
					try
					{
						m_context.run();
					}
					catch (asio::error_code &error)
					{
// okay so m_context.run(error); returned. Which means the context was either freed
// or there was an error
						m_context_runner_thread_running = false;
			// check if there is a valid error and if the context handler is init
						if (context_error_handler)
						{
							// log the error
							std::stringstream error_str;
							error_str << "Context runner error: Code [" << error.value()
									  << "] Name[" << error.category().name()
									  << "] Message: "
									  << error.message() << "\n";
							context_error_handler(error_str.str());
						}
					}
				}
		);
		m_context_runner_threads.emplace_back(MoveTemp(th));
// detach yourself from the context thread. what this allows is that after the function call ends, the threads won't be bound to the original thread. This is equivalent to daemon-ising the threads
		m_context_runner_threads.at(count).detach();
	}
}
```

Key variables:

```cpp
/**
 * @brief context runner thread pool
 */
std::vector<corelink_thread> m_context_runner_threads;
/**
 * @brief Flag: true if the context manager threads are running. False if they shut down.
 */
bool m_context_runner_thread_running;
/**
 * @brief Concurrency level. this is essentially a user parameter to decide how many threads should be pooled to
 * handle requests.
 */
size_t m_concurrency_level;
```