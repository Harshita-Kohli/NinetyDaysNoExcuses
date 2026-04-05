## Immutable vs Mutable Objects in Java [(Read More @ BaelDung)](http://baeldung.com/java-mutable-vs-immutable-objects)
- Immutable objects:
  - state cannot be changed after creation. Better reliability, consistency, better caching, thread safety.
  - Once an immutable object is instantiated, its values and properties remain constant throughout its lifetime.
  - We can create our own custom immutable object by creating a class with **final keyword** and **no setter methods** allowing changes to its state after instantiation.
- Mutable Objects:
- Mutable objects in Java are entities whose state can be modified after their creation. 
- Better flexibility and dynamic state changes.
- While immutability provides thread safety, predictability, and other advantages, mutability offers flexibility and dynamic state changes.


## Concurrent Hashmap:
- It is a thread-safe alternative to a hashmap. It performs fine-grained locking ie locking only a part of the table.
- Read operations are completely lock-free, write operations require partial locking i.e. CAS + bucket-level locking.
- CAS - Compare and Swap
- Bucket-level locking in case of collision and resizing.
- In Java 7, we only locked the segment we were writing into. Segment is a smaller part of the hashtable. Each segment has its own lock.
- Post java 7, no segmentation is done. SInce 16 is the no of segments allowed. For larger hashtables, we would have larger segments and lot of waiting in each segment. If 2 or more concurrent writes ahve to be done in the same segment, lot of delay since entire segment will be locked.
- In java 8 and above, we have CAS+Bucket level locking, except for colision and resizing. In collision, a linked list is dealt with, hence we got to lock the entire linked list at the bucket.

  
## How put() happens in ConcurrentHashmap?
1. If key/value = null -> exception since null keys and values not allowed.
2. Compute hashcode -> then index from hashcode
3. If bucket[index] is empty -> insert using CAS
4. Else collision has occured -> SO bucket level locking and then insert new node in the linkedList
5. else if resizing is needed -> create new bucket -> bucket-level locking onto it.

## equals and hashcode contract:
If you override equals() and not hashcode(), then hashcode() will generate diff hashcodes for "equal" objects. Diff hashcodes means diff buckets, and equal objects are put in diff buckets. As a result, Hashmap behaves incorrectly.

## Compare HashMap vs ConcurrentHashMap in 5 bullets.
- Thread Safety: HashMap is not synchronized and therefore not thread-safe, making it unsuitable for multi-threaded environments without external locking. In contrast, ConcurrentHashMap is thread-safe and allows concurrent access and modification by multiple threads without needing external synchronization.

- Performance & Contention: HashMap is faster in single-threaded scenarios because it lacks synchronization overhead. ConcurrentHashMap is optimized for high-concurrency environments by using fine-grained locking (or bucket-level locking) to minimize contention, making it significantly faster when accessed by multiple threads.
  
- Null Values: HashMap allows one null key and multiple null values. ConcurrentHashMap does not allow null keys or null values, and it will throw a NullPointerException if you attempt to add them.

- Iteration Behavior: HashMap uses a fail-fast iterator, which throws ConcurrentModificationException if the map is structurally modified during iteration. ConcurrentHashMap uses a weakly consistent (fail-safe) iterator, which does not throw this exception and permits safe modifications while iterating.

- Use Cases: Use HashMap for simple, single-threaded applications or read-only maps. Use ConcurrentHashMap for high-throughput, multi-threaded scenarios, such as caching, web servers, or when shared data is frequently modified by multiple threads.
