## Runtime Assumptions

- `require` functions as designed in Luau RFC <!-- Insert Number Here -->
- The standard Luau libraries are provided in their entirety.
- The standard Luau globals are provided in their entirety.
- No modification has been made to the operators for `string`,
 `boolean`, or `number`.

## Environmental Assumptions

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

## External Library Assumptions


## Cost Requirements

- Automatic Migration cannot exceed 10% of the DataStore Get/Set request
 limitations.
- Automatic Migration cannot exceed 1% of the DataStore storage limitation.
- Automatic Migration cannot exceed 20% of the MemoryStore request limitations.
- Automatic Migration cannot exceed 25% of the MemoryStore storage limitation.