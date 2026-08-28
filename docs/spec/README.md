

## Assumptions

- `N`-Servers can be active at any given time.
- A server has no guaranteed lifetime.
- A server is culled when there are no clients attached.
- A server is created when a client requests one.
 Note: Not all experiences permit this behaviour, but
 users can abuse the blocking functionality to create
 individualized servers. 
- All DataStore requests have limitations on frequency and size.
- All MemoryStore requests have limitations on frequency and size.
- All requests yield until a response or error has been received.

