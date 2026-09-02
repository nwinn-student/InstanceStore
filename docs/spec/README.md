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
- Upon a server close, all operations are halted except for the
 closing remarks (functions explicitly stated to run upon close).
- All DataStore requests have limitations on frequency and size.
- All MemoryStore requests have limitations on frequency and size.
- All requests yield until a response or error has been received.

## External Library Assumptions

- There exists a DataStore library that **TODO**.
- There exists a MemoryStore library that **TODO**.

## Cost Requirements

- Automatic Migration cannot exceed 10% of the DataStore Get/Set request
 limitations.
- Automatic Migration cannot exceed 1% of the DataStore storage limitation.
- Automatic Migration cannot exceed 20% of the MemoryStore request limitations.
- Automatic Migration cannot exceed 25% of the MemoryStore storage limitation.

## History Path

### Reducing Migration Effort

The base approach is having a set of migration
 functions that the user must create to go from
 `vN` to `vM`.  Where all versions between have
 some migration function.

- Approach 1: Instead of storing the name of
   an item, store a unique identifier that
   is never taken by another and never changes.
   Inability to provide custom migration paths.
  - Intent: No need to migrate when an item's
     name changes.
  - Flaw: Migrating across multiple versions leads
     to inconsistencies.  <!-- Provide Example -->
  - Result: Declined
- Approach 2: Allow for default values to be specified
   and remove them from the saved data.
  - Intent: Reduce the size of the saved data while
     allowing for optional fields.
  - Flaw: Upon a default value changing and an entry
     holding prior default data, their data holds a different
     meaning.  Such changes may be abused.
    - Example: `User1` has data `{ fruit = nil }` on version `v1`,
     which signified that their `fruit` was a `"pear"`.
     The next version `v2` modifies the default value of `fruit`
     to be `"apple"`.  Because `User1` did not have `"pear"` saved,
     their data's meaning changes.
    - Takeaway: Do **not** modify a default value. 
  - Result: Declined
- Approach 3: Allow for custom migrations on Approach 1.
  - Intent: Support complex migrations while allowing simple
     ones to not require effort.
  - Flaw: There becomes a need to know when to add migration
     functions.  Regardless the flaw from Approach 1 still
     persists, but in a different form.
  - Result: Declined
- Approach 4: Allow for custom migrations, but have an identifier
   system where all data has a `tag` and should the specific `tag`
   get moved, on load the tag will be moved to the corresponding
   location.
  - Different from Approach 3 in that `tag`s are global
     and not local based on different scopes.
  - Intent: **TODO**
  - Flaw: Migrating across multiple versions causes
     inconsistencies.

- Approach 5: Do nothing.
  - Intent: Minimize migration concerns by forcing the developer
     to create them.
  - Flaw: Inconvient to the developer, pushing them to
     try to create another declined Approach or variant
     thereof that has migration issues.
- Approach 5: Do nothing and provide an external program
   to take the data format from `vN` and generate the
   migration function for it to go to the next version.
  - Requirements: An external storage of the previous
     version's format.  The ability to modify code
     in-place with a program capable of reading the code.
     A provided location where the current version's format lies.
  - Intent: Automatically generate migration functions to
     improve user experience drastically.
  - Flaw: There is no way to generate moves.