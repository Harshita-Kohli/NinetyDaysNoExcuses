## Types of Association [Baeldung](https://www.baeldung.com/cs/inheritance-aggregation)
Association in Java can be further classified into the following types:

1. **Aggregation (Weak Association)**
- Aggregation represents a **“has-a” relationship** where one class contains a reference to another class, but both can exist independently.
- Unlike inheritance, where objects are related through a parent-child relationship, objects in an aggregation relationship are **loosely coupled** and
have **NO inherent hierarchical relationship**.
- So, aggregation allows objects to share information and operations, but **they can still be used independently**
- It is a weak relationship
- Objects have independent lifecycles
- One object can exist without the other
Example: A Department has Employees, but employees can exist even if the department is removed

2. **Composition (Strong Association)**
- Tightly bound type of "HAS-A" relationship
- Composition is a strong form of association where one class owns another class. If the parent object is destroyed, the child object also gets destroyed.
- It is a strong relationship
- Objects have dependent lifecycles, where **Child object cannot exist without the parent**
Example: A Car has an Engine

3. **Inheritance:**
- It is an IS-A rleationship, where child class is a child of Parent class, and inherits the properties of the parent class.
- Inheritance promotes code reusability and reduces redundancy.
- Any changes done in parent class need to be done in the child class as well, so the classes are tightly coupled.


## String & Immutability
- Strings in Java are immutable to maintain Thread-safety, Efficient Caching, Consistent Hashvalue production, Better memory usage.
- Whenever we manipulate a string in java, it does not change the existing object, instead creates a new object.
- EG: Concatenation creates a NEW String
  ```
  // Both s1 and s2 point to the same object in the String Pool because JVM reuses string literals.
  String s1 = "Hello";
  String s2 = "Hello"; 
  s1 = s1.concat(" World"); //this RHS creates a new object in Heap(not in string pool) and as comes the LHS, s1 will now point to this object.
  ```
```
Here is where immutability comes in:
"Hello" cannot be modified.
concat(" World") returns a new String → "Hello World".
s1 now refers to this new object in the heap.
s2 still refers to "Hello" in the pool.
```

- Using new always creates a heap object
```
String s3 = new String("Hello");
s2 == s3   // false
```
- This creates a new object in heap, even though "Hello" exists in the pool. So s2 (pool) and s3 (heap) are different references.

## Why Strings Are Made Immutable?
Immutability gives major advantages:
- **Security**: Used in ClassLoader, hash keys, network paths → safe from modification.
- **String Pool Optimization**: JVM can reuse string literals, reducing memory usage.
- **Thread Safety**: No synchronization needed because String objects cannot change.
- **Caching hash code**: String’s hash code is cached → faster HashMap key lookup.

## Immutability is about the String object, not the reference.
- You CAN reassign s1, You can change what the variable reference s1 points to.
- ❌ But you CANNOT modify the String object itself.
- The object’s internal char[] never changes. **So immutability applies to the object, not the variable pointing to it.**
- Also the string class is final, so we can't override the functions given in the class

## How does this maintain hashcode consistency?
- Hashcode belongs to the String object, not the reference
When you do: String s1 = "Hello";
You have: Variable s1  --->  String Object("Hello")  (hashCode = 69609650)

If you do: s1 = s1.concat(" World");

Java does NOT change the original "Hello" objectIt creates a new object:
```
Old object: "Hello"          (hashCode stable forever)
New object: "Hello World"    (Has its own hashCode)
```
So now:
s1 ---> "Hello World"
s2 ---> "Hello"
🔥 This is why hashcode consistency is guaranteed:
"Hello" will always have the same hashcode
"Hello World" will always have its own hashcode
Reassigning a reference has no impact on previous objects in memory


String pool
Why String is final
Mutable vs Immutable
🔥 Hard: ConcurrentHashMap
Segmented locking (older versions)
CAS + synchronized buckets (Java 8)
Why threads don’t block

Task: Compare HashMap vs ConcurrentHashMap in 5 bullets.
