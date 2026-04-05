## Diff between Abstract classes and Interface:

<img width="735" height="445" alt="image" src="https://github.com/user-attachments/assets/775021a8-8d98-4821-a247-71169db32d37" />

- The private methods in interfaces help in extracting common logic from multiple default or static methods without exposing that logic to implementing classes.

### When to Use Which?
Use an Abstract Class when:
- You want to **share code** among **several closely related classes** (e.g., Dog and Cat extending Animal).
- You need to maintain a common state (non-static/non-final fields) across subclasses.
- You want to provide a base implementation that **subclasses can inherit** and **optionally override.**

Use an Interface when:
- You want to define a **contract for unrelated classes** (e.g., Car and Bird both implementing Flyable).
- You need to take advantage of **multiple inheritance of type.**
- You want to decouple the "what" (behavior) from the "how" (implementation) to ensure loose coupling. 

Summary:
- Abstract classes represent **what an object is (Inheritance/Structure).**
- Interfaces represent **what an object can do (Capability/Contract).** 

## Default Methods vs Static Methods in Java 8:
- Default methods allow you to add new methods to an interface without breaking the existing classes that implement it. 
- Syntax: Defined using the **default keyword followed by the method body.**
- Inheritance: Implementing classes automatically inherit these methods. They can use the default implementation as-is or override them to provide specific behavior.
- **Backward Compatibility**: They were primarily added to support new features like the Stream API in the java.util.Collection interface without requiring every third-party collection library to be rewritten.
- Diamond Problem: If a class implements two interfaces that have the same default method signature, the compiler will throw an error. The class must resolve this by overriding the method and optionally calling a specific parent via InterfaceName.super.methodName()

Static Methods:
- Static methods in **interfaces belong to the interface class itself** rather than any implementing object. 
- Syntax: Defined using the **static keyword.**
- Access: They must be called **using the interface name** (e.g., MyInterface.myStaticMethod()). They cannot be called via an instance of an implementing class.
- No Overriding: **Unlike default methods, static methods cannot be overridden by implementing classes.**
- Purpose: They are used for utility or helper methods that are relevant to the interface's domain, reducing the need for separate utility classes like Collections or Path
