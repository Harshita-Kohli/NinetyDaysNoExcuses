[Interview Questions](https://neesri.medium.com/top-15-tricky-hashmap-interview-questions-with-smart-answers-813f2bdbc108)
## Internal Working of HashMap in Java
- HashMap is a Collection in Java that stores data in key-value pairs.
- O(1) insertion, deletion, retrieval
- It is an Array of nodes. Each node has Key, Value, Hashcode of key, reference to next node

### Insertion in Hashmap(map.put())
1. Compute hashvalue from the key. hashCode(key), where n=16 [the by default no of buckets in hashmap]
2. Compute the index ```index = hashCode(key) & (n-1)``` [The bitwise AND operation ensures even distribution of entries across buckets]
3. If index wali bucket is empty, create a Node object with the value, place the Node at the calculated index .
4. If another node already exists, Collision has occured.
   - If both keys have same hashCode() and equals(), replace the previous existing value - Key replacement happens
     ```
     map.put("fruit", "apple");
     map.put("fruit", "mango");
     System.out.println(map.get("fruit")); // Output: mango
     ```
   - if not equal, then create a linkedList and append the new node to the existing node.
  
### Retrieval in Hashmap(map.get())
1. Compute hashvalue from the key. hashCode(key)
2. Compute the index ```index = hashCode(key) & (n-1)```
3. Check if the first node's key is same as the given key. If yes, return the value. Else, check the next node in the Linked List

## Collision Handling 
- A collision occurs when two different keys produce the same index in the HashMap.
- Java handles collisions using:
**Linked List (Java 7)
Red-Black Tree (Java 8+)**

1. Before Java 8: Collisions are managed using a **linked list** — new nodes are added at the same index, linked via the next reference. But the lookup will take O(n) in the worst case.
2. Since Java 8: If a bucket contains more than 8 nodes, the linked list is converted into a **balanced tree (TreeNode) for faster lookup** (O(log n) instead of O(n)).

## What is Treeification?
- If one bucket has more than 8 entries and capacity ≥ 64, Java 8 and above converts the bucket into a **Red-Black Tree** → improves lookup to **O(log n)**.
- Treeification is a performance optimization for HashMap (and ConcurrentHashMap) introduced in Java 8.
- It transforms a bucket's data structure from a Singly Linked List into a Red-Black Tree when a high number of hash collisions occur.
-  Thresholds for Conversion
- The transition is governed by specific static constants in the HashMap source code:
  1. TREEIFY_THRESHOLD (8): A bucket is converted to a tree when it reaches 8 entries.
  2. UNTREEIFY_THRESHOLD (6): If removals or resizing reduce a bucket's size to 6 entries, it is converted back into a linked list to save memory.
  3. MIN_TREEIFY_CAPACITY (64): Even if a bucket has 8 nodes, treeification will not occur unless the total map capacity is at least 64.
  4. If the map is smaller, it will Resize (Rehash) instead to spread out entrie
 
## How treeification Works Internally?
- When a new entry is added to a bucket that already has 8 nodes:
1. Node Upgrade: The standard Node objects (which only have a next pointer) are replaced by TreeNode objects. TreeNode is roughly twice the size of a regular Node because it must store references to parent, left, right, and a red/black boolean.
2. Sorting Logic: To maintain the Red-Black Tree, keys must be ordered.
  - Java first compares keys by their hash codes.
  - If hash codes are identical, it checks if the keys implement the Comparable interface to use compareTo().
  - As a final fallback, it uses a system identity hash code via tieBreakOrder() to ensure a consistent, deterministic tree structure.
