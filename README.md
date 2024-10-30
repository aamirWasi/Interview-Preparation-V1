# Interview-Preparation-V1
## 🤔Q1: What is the main difference between the HTTP PUT and PATCH methods?
1. PUT is used to update a resource by replacing the entire resource with a new version. It usually requires sending the full resource in the request.
2. PATCH is used to partially update a resource. It allows you to send only the fields that need to be updated, rather than the entire resource.

**1. PUT Method**:

- Idempotency rule: PUT is idempotent. Making the same PUT request multiple times will always result in the same state on the server.

**Explanation**:

- PUT is used to update or replace a resource on the server. The key point is that it either creates or fully updates the resource in a way that every time you send the same data, the result will be the same.

**Example**: Let's say you're updating a user's profile information with PUT:
```c#
PUT /users/123
{
  "name": "John Doe",
  "email": "john@example.com"
}
```
The first time you send this request, it updates or creates the user profile for user 123 with the given name and email.
If you send the exact same request again, nothing will change. The server will see that the resource is already in that state (the name is still "John Doe" and the email is still "john@example.com"), so it won’t do anything different.

Result: No matter how many times you repeat the request, the final state will be the same.

**2. POST Method**:

- Idempotency rule: POST is NOT idempotent. Making the same POST request multiple times may result in multiple resources being created or multiple actions being taken.

**Explanation**:

- POST is used to create a new resource or perform an action. When you send a POST request, it typically creates a new resource, and sending the same request again may create a duplicate or trigger an additional action.

**Example**: Let's say you're submitting a form to create a new user:
```c#
POST /users
{
  "name": "John Doe",
  "email": "john@example.com"
}
```
The first time you send this request, a new user with the name "John Doe" and the email "john@example.com" is created.
If you send the same request again, it will create another user with the same data, so now you’ll have two users with the same information (if the system allows duplicates).

Result: Repeating the request leads to different outcomes—each POST creates a new resource.

**3. PATCH Method**:

- Idempotency rule: PATCH is idempotent if the same request is applied multiple times. However, the change depends on how you use it.

**Explanation**:

- PATCH is used to partially update a resource. It modifies only the specified fields. Sending the same PATCH request multiple times should lead to the same result as sending it once (if the request doesn’t conflict with the resource's current state).

**Example**: Let's say you're updating just the email of a user:
 ```c#   
PATCH /users/123
{
  "email": "newemail@example.com"
}
```
The first time you send this request, it updates only the email field of user 123 to "newemail@example.com".
If you send the same request again, the server will still update the email to "newemail@example.com", but since it’s already the same, the state won’t change.

Result: If the request is structured properly, sending the same PATCH request multiple times won’t change anything after the first one. The result is idempotent because the resource is left in the same state.

**PATCH**: Generally Idempotent. Repeating the same request leads to the same outcome, assuming the state doesn’t conflict.

## 🤔Q2: When would you choose to use PUT instead of PATCH?
1. Use PUT when you want to completely replace the resource. If you don’t send the full resource with a PUT request, any fields you omit might be reset or lost.
For example, if you have an object with 5 properties and want to change 1 of them, using PUT means you should send all 5 properties. Any missing properties might be deleted.
2. Use PATCH when you only need to update part of a resource and want to avoid sending the entire object.
For example, if you want to update just one field in a large object, PATCH allows you to send a small request with only the modified field(s), which can be more efficient.
## 🤔Q3: Can you use PUT to create a resource?
Yes, PUT can also be used for creation if the resource does not exist yet. If you send a PUT request to a URI that does not exist, it can create a new resource at that URI.
PATCH is typically not used for creating new resources, as it is primarily for modifications.
## 🤔Q4: What is FINALLY?
1. The finally block is used to execute a given set of statements, whether an exception occur or not.
2. Finally block is mostly used to dispose the unwanted objects when they are no more required. This is good for performance, otherwise you have to wait for garbage collector to dispose them.
## 🤔Q5: What is Finalize?
1. Finalize is a method which is automatically called by the garbage collector to dispose the no longer needed objects 
## 🤔Q6: What are the different types of classes in C#?
**👉<ins>Static class</ins>**:

1. In C#, a **static class** is a class that cannot be instantiated, meaning you cannot create an object from it. All members of a static class must also be static. It's typically used to group related methods that don’t act on instance data and are shared across the application.
```c#
string currentDateTime = DateUtils.GetCurrentDateTime(); // Returns current date and time
int daysBetween = DateUtils.CalculateDaysBetween(DateTime.Now, DateTime.Now.AddDays(5)); // Returns 5
Console.WriteLine(daysBetween);
public static class DateUtils
{
    public static string GetCurrentDateTime()
    {
        return DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss");
    }

    public static int CalculateDaysBetween(DateTime startDate, DateTime endDate)
    {
        return (endDate - startDate).Days;
    }
}
```
DateUtils provides utility functions for working with dates and times, which don’t require maintaining any state. The static class can be accessed globally and makes date operations easier.

**👉Key Characteristics of a Static Class**:

*   It **cannot be instantiated**.
*   It **cannot have constructors** (except static constructors).
*   **All members must be static** (methods, fields, properties, etc.).
*   It's sealed, meaning it cannot be inherited.
Static classes are often used to hold utility methods, such as mathematical calculations, logging, or configuration helpers.

**👉Static constructor**: "Static constructor is called once when the class is first used."

**👉<ins>Sealed Class</ins>**

1. Sealed Class: In most cases, we never intend this class to be able to be inherited by other classes, we'll get a significant performance boost by adding sealed keyword in a class not inherited by other classes. By sealing the class, the JIT compiler can directly know the object's actual data type. Meanwhile, for unsealed classes, JIT needs to check whether the class has subclasses or not.
2. A sealed class in C# is a class that cannot be inherited. It is useful when you want to prevent any further modification or extension of a class by subclassing. This can be helpful for security, performance, or to preserve behavior that shouldn't change.
```c#
var logger = new ApplicationLogger();
logger.LogInfo("Application started.");
logger.LogError("An unexpected error occurred.");

public sealed class ApplicationLogger
{
    public void LogInfo(string message)
    {
        Console.WriteLine($"INFO: {message}");
    }

    public void LogError(string message)
    {
        Console.WriteLine($"ERROR: {message}");
    }
}
```
By sealing the ApplicationLogger, you prevent other developers from subclassing and potentially modifying the logging behavior, which could create inconsistent log formats or levels.

**👉<ins>Abstract Classes</ins>**

1. Provide a base class with some implementation and abstract methods that derived classes must implement.
2. Use an **abstract class** when there is shared behavior among subclasses, but use an **interface** when different classes need to follow the same contract, without enforcing shared behavior.
```c#
public abstract class Payment
{
    public void LogTransaction(decimal amount) // Concrete method
    {
        Console.WriteLine($"Logging transaction of {amount:C}.");
    }
    public abstract void ProcessPayment(decimal amount); // Abstract method
}

public class CreditCardPayment : Payment
{
    public override void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing credit card payment of {amount:C}.");
    }
}

public class PayPalPayment : Payment
{
    public override void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing PayPal payment of {amount:C}.");
    }
}
```
**👉<ins>Partial classes</ins>**

1. A partial class in C# allows you to split the definition of a class across multiple files. Each part of the class can be in a separate file, but they’re all combined into a single class at compile time. This feature is particularly useful when working on large, complex classes or when collaborating on projects where different developers might work on different aspects of the same class.

Example: Employee Management System

In an Employee Management System, suppose the Employee class is responsible for handling various operations, including basic employee information, database-related methods, and validation. Each responsibility can be split into a separate file.

Employee.BasicInfo.cs (Handles basic employee properties)

Employee.DatabaseOperations.cs (Handles CRUD operations)

Employee.Validation.cs (Handles validation)
```c#
// Employee.BasicInfo.cs
public partial class Employee
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Department { get; set; }
}

// Employee.DatabaseOperations.cs
public partial class Employee
{
    public void SaveToDatabase()
    {
        // Code to save employee to the database
    }

    public void DeleteFromDatabase()
    {
        // Code to delete employee from the database
    }
}

// Employee.Validation.cs
public partial class Employee
{
    public bool IsValid()
    {
        // Validate employee information (e.g., age should be > 18)
        return Age > 18;
    }
}
```
By splitting these concerns, the Employee class remains manageable, organized, and easier to maintain.

**👉<ins>Generic classes</ins>**
1. Generic classes in C# allow you to create classes that can work with any data type while providing type safety. They are particularly useful for building reusable, flexible components that can operate on various types without sacrificing performance or security. In real-world applications, generic classes are commonly used for data structures, services, repositories, and utility functions.
```c#
var customerRepository = new Repository<Customer>();
customerRepository.Add(new Customer { Name = "John Doe" });

var productRepository = new Repository<Product>();
productRepository.Add(new Product { Name = "Laptop" });
Console.WriteLine(productRepository.GetAll().Count());

public sealed class Customer
{
    public long Id { get; set; }
    public string Name { get; set; } = string.Empty;
}
public sealed class Product
{
    public long Id { get; set; }
    public string Name { get; set; } = string.Empty;
}

public interface IRepository<T> where T : class
{
    void Add(T entity);
    void Update(T entity);
    void Delete(T entity);
    T GetById(int id);
    IEnumerable<T> GetAll();
}

public class Repository<T> : IRepository<T> where T : class
{
    private readonly List<T> _dataStore = new List<T>();

    public void Add(T entity) => _dataStore.Add(entity);
    public void Update(T entity) { }
    public void Delete(T entity) => _dataStore.Remove(entity);
    public T GetById(int id) { return default!; }
    public IEnumerable<T> GetAll() => _dataStore;
}
```
## 🤔Q7: Can Abstract class be Sealed or Static in C#?
1. NO. Abstract class are used for inheritance, but sealed and static both will not allow the class to be inherited.
## 🤔Q8: Can you create an instance of an Abstract class or an Interface?
1. No. Abstract class and interface purpose is to act as base class via inheritance. Their object creation is not possible.
## 🤔Q9: What is an Interface?
1. Define a contract that classes must follow but does not provide any implementation.
2. In C# 8 and later, default methods in interfaces (also known as default interface methods) allow you to provide a default implementation for methods directly in the interface. This was a significant change, as prior to C# 8, interfaces could only contain method signatures (i.e., no implementation).

**👉Backward Compatibility**: If you want to extend an interface by adding new methods without breaking the existing implementations that already implement the interface.
```c#
public interface IPaymentProcessor
{
    void ProcessPayment(decimal amount);
    // New method with a default implementation
    void LogPayment(decimal amount)
    {
        Console.WriteLine($"Logging payment of {amount}");
    }
}

public class CreditCardPayment : IPaymentProcessor
{
    public void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing credit card payment of {amount}");
    }
    // This class doesn't need to implement LogPayment, it can use the default
}

public class PayPalPayment : IPaymentProcessor
{
    public void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing PayPal payment of {amount}");
    }
    // This class can also use the default LogPayment implementation
}
```
**👉Shared Behavior Across Implementations**: If there is some common functionality that multiple implementations of the interface will share, you can provide that behavior in the default method, avoiding code duplication.

1. In C# 8 and later, interface default implementations like ApplySeasonalDiscount are not automatically accessible within implementing classes (like HolidayDiscount and RegularDiscount). Default implementations in interfaces are only available directly from the interface itself, not from the implementing classes.(so cast in classes)
```c#
public interface IDiscountCalculator
{
    decimal CalculateDiscount(decimal totalAmount);
    // Default method to calculate seasonal discounts
    decimal ApplySeasonalDiscount(decimal totalAmount)
    {
        return totalAmount * 0.90m; // 10% off as a default seasonal discount
    }
}

public class HolidayDiscount : IDiscountCalculator
{
    public decimal CalculateDiscount(decimal totalAmount)
    {
        // Specific discount calculation for holidays
        return ((IDiscountCalculator)this).ApplySeasonalDiscount(totalAmount - 20); // Additional $20 off
    }
}

public class RegularDiscount : IDiscountCalculator
{
    public decimal CalculateDiscount(decimal totalAmount)
    {
        // Uses the default seasonal discount provided in the interface
        return ((IDiscountCalculator)this).ApplySeasonalDiscount(totalAmount);
    }
}
```
**👉When to Avoid Default Methods?**
1. Over-complicating interfaces: If you add too many default methods, interfaces can become bloated and harder to understand.
2. Increased coupling: Default methods can sometimes introduce coupling between the interface and its implementations, which can make future changes more difficult.
3. Breaking SOLID principles: If you're not careful, default methods can violate principles like Single Responsibility Principle (SRP) by adding too much behavior to an interface that should focus solely on abstraction.
## 🤔Q9.1: What is the difference between an abstract class and an interface?
In C#, both abstract classes and interfaces are types that enable polymorphism, allowing objects of different classes to be treated as objects of a common super class. However, they serve different purposes and have different rules.
1. **Abstract Classes**: Provide a base class with some implementation and abstract methods that derived classes must implement.
2. **Interfaces**: Define a contract that classes must follow but do not provide any implementation.

**👉Interview Tip**: Use an abstract class when there is shared behavior among subclasses, but use an interface when different classes need to follow the same contract, without enforcing shared behavior.
```c#
public abstract class Payment
{
    public void LogTransaction(decimal amount) // Concrete method
    {
        Console.WriteLine($"Logging transaction of {amount:C}.");
    }
    public abstract void ProcessPayment(decimal amount); // Abstract method
}

public class CreditCardPayment : Payment
{
    public override void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing credit card payment of {amount:C}.");
    }
}

public class PayPalPayment : Payment
{
    public override void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing PayPal payment of {amount:C}.");
    }

}


public interface INotification
{
    void Send(string message);
}

public class EmailNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine($"Sending email notification: {message}");
    }
}

public class SMSNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine($"Sending SMS notification: {message}");
    }
}
```
**👉Abstract Class**:

1. Can contain implementation of methods, properties, fields, or events.
2. Can have access modifiers (public, protected, etc.).
3. A class can inherit from only one abstract class (single inheritance).
4. Can contain constructors.
5. Used when different implementations of objects have common methods or properties that can share a common implementation.

**👉Interface**:

1. Cannot contain implementations, only declarations of methods, properties, events, or indexers.
2. Members of an interface are implicitly public.
3. A class or struct can implement multiple interfaces (multiple inheritance).
4. Cannot contain fields or constructors.
5. Used to define a contract for classes without imposing inheritance hierarchies.
## 🤔Q10: What is an encapsulation?
**👉Encapsulation**:
1. Encapsulation is the concept of restricting direct access to the internal state (fields) of an object and only exposing the necessary methods to interact with it. This ensures controlled access to the data and better security.
2. This is typically achieved through the use of access modifiers such as private, public, protected, and internal. 
```c#
public class BankAccount
{
    // Private fields (encapsulation)
    private decimal balance;
    // Public method to deposit money
    public void Deposit(decimal amount)
    {
        if (amount > 0)
        {
          balance += amount;
          Console.WriteLine($"Deposited: {amount:C}. Current balance: {balance:C}");
        }
        else
        {
          Console.WriteLine("Deposit amount must be positive.");
        }
    }
    // Public method to check balance
    public decimal GetBalance()
    {
        return balance;
    }
}

class Program
{
    static void Main()
    {
      BankAccount account = new BankAccount();
      account.Deposit(500.00m); // Deposit money
      Console.WriteLine($"Balance: {account.GetBalance():C}"); // Check balance
    }
}
```
In a real-world bank account, you can't just take money out of the vault or look at the books yourself. You need to interact with the bank via ATMs or customer service, which controls access to your account.
## 🤔Q11: Explain polymorphism and its types in C#.?
**👉Polymorphism**:
1. **Polymorphism** is a core concept in object-oriented programming (OOP) that allows objects to be treated as instances of their parent class rather than their actual derived class.  
**Polymorphism** means "many forms." It allows a method or function to behave differently based on the object it is acting upon. It can be **compile-time** (method overloading) or **runtime** (method overriding).  
```c#
// Base class
public class Notification
{
    public virtual void Send()
    {
    Console.WriteLine("Sending a generic notification.");
    }
}
// Derived class for email notifications
public class EmailNotification : Notification
{
    public override void Send()
    {
    Console.WriteLine("Sending an email notification.");
    }
}
// Derived class for SMS notifications
public class SMSNotification : Notification
{
    public override void Send()
    {
    Console.WriteLine("Sending an SMS notification.");
    }
}
class Program
{
    static void Main()
    {
        Notification notification = new EmailNotification();
        notification.Send(); // Calls EmailNotification's Send method
        notification = new SMSNotification();
        notification.Send(); // Calls SMSNotification's Send method
    }
}
```
**👉What is Polymorphism, and how does it improve code flexibility?**

1. Polymorphism allows one interface to be used for different types. It can be achieved through method overriding or method overloading.

**How does method overriding support polymorphism in C#?**

1. Method overriding allows a derived class to provide a specific implementation of a method that is already defined in the base class. This is a key feature of runtime polymorphism.
## 🤔Q12: Can you explain the difference between method overloading and method overriding with examples?
**Method Overloading**:

1. Same method name but different parameters (compile-time polymorphism).
```c#
public class Calculator
{
    public int Add(int a, int b) => a + b;
    public int Add(int a, int b, int c) => a + b + c;
}
```
**Method Overriding**:

1. Same method name, and signature, but different implementations in derived classes (runtime polymorphism).
```c#
public class BaseClass
{
    public virtual void ShowMessage()
    {
    Console.WriteLine("Message from base class.");
    }
}

public class DerivedClass : BaseClass
{
    public override void ShowMessage()
    {
    Console.WriteLine("Message from derived class.");
    }
}
```
## 🤔Q13: What is the default behavior of C# language in term of dispatching??
**👉Static Dispatch**

In C#, the default behavior for dispatching (method/property invocation) follows early binding or static dispatch. This means that, by default, C# uses the compile-time type of an object to determine which method or property to call. This is known as static (or early) binding because the decision of which method to call is made at compile time.
```c#
public class Animal
{
    public void Speak()
    {
        Console.WriteLine("Animal speaks.");
    }
}
public class Dog : Animal
{
    public void Speak()
    {
    Console.WriteLine("Dog barks.");
    }
}

Animal animal = new Dog();
animal.Speak(); // Output: "Animal speaks."
```
**👉Explanation**:

Compile-Time Binding: In this example, even though animal holds a Dog object, the method Speak() from the Animal class is invoked. This happens because the declared type of animal is Animal, and the method Speak() is not virtual. Therefore, static dispatch is used, and the decision to call the Animal.Speak() method is made at compile time.

**👉Another Example:**
```c#
User user = new Customer();
Console.WriteLine(user.UserName); // Output: user: aamir

public class User
{
    public string UserName { get; set; } = "user: aamir";
}

public class Customer : User
{
    public string UserName { get; set; } = "customer: aamir";
}
```
**👉Explanation**

It will print **Base User Name**

👉The Problem here is that we are violating liskov principle and base class will hide the child class implementation.

👉In the code provided, you're defining the UserName property in both the base class User and the derived class Customer. This violates the Liskov Substitution Principle (LSP) because the base class User and the derived class Customer don't behave consistently when substituted for one another. Additionally, the derived class's property is hiding the base class property instead of overriding it. This can lead to confusion and unexpected behavior.

**👉Key Concepts**

**Liskov Substitution Principle (LSP)**: The Liskov Substitution Principle states that objects of a derived class must be able to replace objects of the base class without affecting the correctness of the program. In other words, a subclass should behave in a way that is consistent with the expectations from the base class.
Liskov Substitution Principle (LSP), which says that a derived class (Customer) should be able to replace its base class (User) without breaking the behavior.

**👉Inheritance**: The Customer class inherits from the User class, which means it gets all the properties and methods of the User class.

**👉Property Hiding (Shadowing)**: In the Customer class, the UserName property is redefined using the new keyword (implicitly, since it's not marked virtual in the base class), which means it hides the UserName property from the User class. This is not polymorphism; it’s property hiding or shadowing.

1. Here, the variable user is of type User, but it holds an instance of Customer. This means you are creating an object of Customer, but referring to it as a User.
2. **Property Access**: When you access user.UserName, you are accessing the UserName property from the User class, not from the Customer class.
    - Why? This is because property hiding (not overriding) is at play here. Since the type of the variable (user) is User, it will access the UserName property defined in the User class, which has the default       value "Base User Name". The Customer's version of UserName is not considered because it’s hidden, not overridden.
3. **Output**: The value of user.UserName will be "Base User Name", because the UserName property from the User class is accessed.

**👉Why Not "Customer User Name"?**

For the Customer's UserName to be used polymorphically, you would need to use inheritance with polymorphism by marking the base property as virtual and the derived property as override. Here’s how you would do that:

**👉Using Virtual and Override for Polymorphism**

If you want the Customer class to properly override the UserName property in a polymorphic way, you can use the virtual and override keywords:
```c#
User user = new Customer();
Console.WriteLine(user.UserName); // Output: user: aamir

public class User
{
    public string UserName { get; set; } = "user: aamir";
}

public class Customer : User
{
    public string UserName { get; set; } = "customer: aamir";
}
```
**👉Explanation of the Updated Code:**
- virtual in the User class: This allows the property to be overridden in derived classes.
- override in the Customer class: This provides a new implementation of the UserName property in the Customer class.

Now, when you create a Customer object and reference it as a User, the overridden UserName property from the Customer class will be used, and the output will be "Customer User Name".

**👉Virtual Methods (Dynamic Dispatch)**

To enable dynamic dispatch (also known as late binding or runtime polymorphism), you need to mark the method in the base class as virtual and override it in the derived class using the override keyword. This instructs the C# runtime to make method dispatch decisions based on the runtime type of the object, rather than its compile-time type.

**👉Example of Dynamic Dispatch (Late Binding)**
```c#
public class Animal
{
    public virtual void Speak()
    {
        Console.WriteLine("Animal speaks.");
    }
}

public class Dog : Animal
{
    public override void Speak()
    {
        Console.WriteLine("Dog barks.");
    }
}
Animal animal = new Dog();
animal.Speak(); // Output: "Dog barks."
```
**👉Explanation**:

Runtime Binding: In this example, the Speak() method is marked as virtual in the Animal class and overridden in the Dog class. When animal.Speak() is called, the decision is made at runtime, based on the actual type of the object (Dog in this case), not the declared type. This is known as dynamic dispatch or late binding.

**👉Another example:**
```c#
// Base Class
public class SupportPersonnel
{
    // Virtual method that can be overridden by derived classes
    public virtual void HandleRequest(string request)
    {
        Console.WriteLine("Support personnel is handling a general request.");
    }
}

// Derived class for general support
public class GeneralSupport : SupportPersonnel
{
    // Override to provide a specific implementation for general support
    public override void HandleRequest(string request)
    {
        Console.WriteLine("General Support: Handling a customer inquiry.");
    }
}

// Derived class for technical support
public class TechnicalSupport : SupportPersonnel
{
    // Override to provide a specific implementation for technical support
    public override void HandleRequest(string request)
    {
        Console.WriteLine("Technical Support: Resolving a technical issue.");
    }
}

// Usage with Dynamic Dispatch
SupportPersonnel support = new TechnicalSupport(); // Upcast to base class type
support.HandleRequest("Internet connectivity issue");

support = new GeneralSupport(); // Changing the reference to another derived class
support.HandleRequest("Billing inquiry");
```
**👉Output:**
```c#
Technical Support: Resolving a technical issue.
General Support: Handling a customer inquiry.
```
**👉Benefits of Dynamic Dispatch**

**👉Flexibility**: Different classes can provide unique implementations, allowing different behaviors to be triggered based on the actual type.

**👉Extensibility**: New support types (like VIPSupport) can be added without changing the calling code that expects a SupportPersonnel reference.

**👉Runtime Binding**: The method is determined at runtime, meaning code doesn’t need to know the specifics of all possible SupportPersonnel types in advance.

This use of dynamic dispatch or late binding enables the Open/Closed Principle in OOP, where code is open for extension but closed for modification, as we can introduce new SupportPersonnel types without changing the base class or calling code.

**👉**In short, the default behavior in C# is static dispatch, and to enable dynamic dispatch, you use virtual methods.**
## 🤔Q14: Imagine two classes with inheritance hierarchy below.
```c#
public class BaseClass
{
    public BaseClass()
    {
        Print();
    }

    public virtual void Print()
    {
        Console.WriteLine("From Base Class");
    }
}
```
```c#
public class ChildClass : BaseClass
{
    private readonly string _declaration;
    public ChildClass()
    {
        _declaration = "I am a child!";
    }
    public override void Print()
    {
        Console.WriteLine(_declaration.ToLower());
    }
}
```
what will happen if we construct the ChildClass like below?
```c#
var child = new ChildClass();
```
**👉Explanation**:

Gives Exception.

1. When you create an instance of ChildClass, the constructor of the base class (BaseClass) is called first, because the constructor of the base class is always called before the constructor of the derived class in C#.
**👉BaseClass constructor**:

Inside the BaseClass constructor, the Print() method is called:
```c#
public BaseClass()
{
  Print(); // Calls the overridden method in ChildClass
}
```
**👉Virtual method call:**
C# uses dynamic dispatch for virtual methods, meaning the overridden method in ChildClass is called even though it's invoked from the BaseClass constructor. Therefore, Print() in ChildClass is called.

**👉ChildClass Print() method:**
In ChildClass, the Print() method tries to access the _declaration field, but at this point, the ChildClass constructor hasn't been executed yet. The _declaration field is initialized inside the ChildClass constructor:
```c#
public ChildClass()
{
  _declaration = "I am a child!";
}
```
Since the ChildClass constructor hasn't finished running when Print() is called, the _declaration field is still null (its default value for reference types).

**👉Resulting output:**
When Print() in ChildClass is called, it tries to execute this line:
```c#
Console.WriteLine(_declaration.ToLower());
```
- Since _declaration is null, calling .ToLower() on a null string would result in a NullReferenceException. However, to avoid that, let's update the explanation based on the current code logic:
- Since the overridden method is called, but the field _declaration has not been initialized yet (because the child class constructor hasn't run), this would throw a NullReferenceException. The final behavior would be that you'd see "From Base Class" printed first, and then an exception would be thrown.

**👉Fixing the Issue:**
To avoid this problem, it's generally a **bad practice to call virtual methods from constructors**. The base class should avoid calling virtual methods during construction, because the derived class might not be fully initialized at that point.
```c#
ChildClass childClass = new ChildClass();
childClass.Initialize();

public class BaseClass
{
    public BaseClass()
    {
    }

    public void Initialize()
    {
        Print();
    }

    protected virtual void Print()
    {
        Console.WriteLine("From Base Class");
    }
}

public class ChildClass : BaseClass
{
    private readonly string _declarartion;
    public ChildClass()
    {
        _declarartion = "From Child Class";
    }

    protected override void Print()
    {
        Console.WriteLine(_declarartion.ToLower());
    }
}
```
**👉Output**: From Child Class

**note**: if i called Initilaize() from constructor in also throw exception

**👉Explanation of Why It Causes Issues**

**👉Object Construction Process**:
- When you create an instance of a derived class, the base class constructor executes first.
- At this point, the derived class has not yet completed its constructor, meaning that any fields or properties in the derived class may not be initialized.
- If the base class calls a method (even if it's a non-virtual method), and that method uses any members from the derived class, it can lead to a NullReferenceException.
## 🤔Q15: Imagine two classes blew. Customer class is child of User class. What will print in the console output?
```c#
public class User
{
    public string UserName { get; set; } = "Base User Name";
}
```
```c#
public class Customer : User
{
    public string UserName { get; set; } = "Customer User Name";
}
```
```c#
User user = new Customer();
Console.WriteLine(user.UserName); //Output?
```
**👉Explanation**

👉It will print "Base User Name"

👉The Problem here is that we are violating liskov principle and base class will hide the child class implementation.

👉In the code provided, you're defining the UserName property in both the base class User and the derived class Customer. This violates the Liskov Substitution Principle (LSP) because the base class User and the derived class Customer don't behave consistently when substituted for one another. Additionally, the derived class's property is hiding the base class property instead of overriding it. This can lead to confusion and unexpected behavior.

**👉Key Concepts**

**👉Liskov Substitution Principle (LSP)**: The Liskov Substitution Principle states that objects of a derived class must be able to replace objects of the base class without affecting the correctness of the program. In other words, a subclass should behave in a way that is consistent with the expectations from the base class.
Liskov Substitution Principle (LSP), which says that a derived class (Customer) should be able to replace its base class (User) without breaking the behavior.

**👉Inheritance**: The Customer class inherits from the User class, which means it gets all the properties and methods of the User class.

**👉Property Hiding (Shadowing)**: In the Customer class, the UserName property is redefined using the new keyword (implicitly, since it's not marked virtual in the base class), which means it hides the UserName property from the User class. This is not polymorphism; it’s property hiding or shadowing.

1. Here, the variable user is of type User, but it holds an instance of Customer. This means you are creating an object of Customer, but referring to it as a User.
2. **Property Access**: When you access user.UserName, you are accessing the UserName property from the User class, not from the Customer class.
    - Why? This is because property hiding (not overriding) is at play here. Since the type of the variable (user) is User, it will access the UserName property defined in the User class, which has the default       value "Base User Name". The Customer's version of UserName is not considered because it’s hidden, not overridden.
3. **Output**: The value of user.UserName will be "Base User Name", because the UserName property from the User class is accessed.

**👉Why Not "Customer User Name"?**

For the Customer's UserName to be used polymorphically, you would need to use inheritance with polymorphism by marking the base property as virtual and the derived property as override. Here’s how you would do that:

**👉Using Virtual and Override for Polymorphism**

If you want the Customer class to properly override the UserName property in a polymorphic way, you can use the virtual and override keywords:
```c#
User user = new Customer();
Console.WriteLine(user.UserName); // Output: user: aamir

public class User
{
    public string UserName { get; set; } = "user: aamir";
}

public class Customer : User
{
    public string UserName { get; set; } = "customer: aamir";
}
```
**👉Explanation of the Updated Code:**
- virtual in the User class: This allows the property to be overridden in derived classes.
- override in the Customer class: This provides a new implementation of the UserName property in the Customer class.

Now, when you create a Customer object and reference it as a User, the overridden UserName property from the Customer class will be used, and the output will be "Customer User Name".
## Q16: Can you explain OOPS & C# - Inheritance, Abstraction, Encapsulation & Polymorphism?
**Inheritance**

1. In C#, inheritance allows a class (called a derived class) to inherit members (fields, methods, properties) from another class (called a base class). This enables code reuse and polymorphism.

**Real-Life Example**: Employee and Manager
```c#
// Base class
public class Employee
{
    public string Name { get; set; }
    public int EmployeeId { get; set; }

    public void Work()
    {
        Console.WriteLine($"{Name} is working.");
    }
}

// Derived class
public class Manager : Employee
{
    public void ManageTeam()
    {
        Console.WriteLine($"{Name} is managing the team.");
    }
}

class Program
{
    static void Main()
    {
        Manager manager = new Manager { Name = "Alice", EmployeeId = 101 };
        manager.Work();  // Inherited from Employee
        manager.ManageTeam();  // Defined in Manager class
    }
}
```
Think of an "Employee" as a general role. A "Manager" is a specialized role, which can do everything an employee does but also has added responsibilities, like managing a team.

**Abstraction**

1. Abstraction allows hiding the complexity of a system by exposing only the necessary parts. This is typically achieved using abstract classes or interfaces.

**Real-Life Example**: Payment System
```c#
// Abstract class
public abstract class Payment
{
    public abstract void ProcessPayment(decimal amount);  // Abstract method
}

// Derived class for credit card payments
public class CreditCardPayment : Payment
{
    public override void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing credit card payment of {amount:C}.");
    }
}

// Derived class for PayPal payments
public class PayPalPayment : Payment
{
    public override void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing PayPal payment of {amount:C}.");
    }
}

class Program
{
    static void Main()
    {
        Payment payment = new CreditCardPayment();
        payment.ProcessPayment(100.50m);  // Calls CreditCardPayment's method
        
        payment = new PayPalPayment();
        payment.ProcessPayment(75.25m);  // Calls PayPalPayment's method
    }
}
```
Consider the concept of "Payment." Whether you pay using a credit card or PayPal, the user only needs to know how much to pay, not the underlying process. The complexities of how each payment method works are abstracted away.

**Encapsulation**

1. Encapsulation is the concept of restricting direct access to the internal state (fields) of an object and only exposing the necessary methods to interact with it. This ensures controlled access to the data and better security.
2. This is typically achieved through the use of access modifiers such as private, public, protected, and internal. 

**Real-Life Example**: Bank Account
```c#
public class BankAccount
{
    // Private fields (encapsulation)
    private decimal balance;

    // Public method to deposit money
    public void Deposit(decimal amount)
    {
        if (amount > 0)
        {
            balance += amount;
            Console.WriteLine($"Deposited: {amount:C}. Current balance: {balance:C}");
        }
        else
        {
            Console.WriteLine("Deposit amount must be positive.");
        }
    }

    // Public method to check balance
    public decimal GetBalance()
    {
        return balance;
    }
}

class Program
{
    static void Main()
    {
        BankAccount account = new BankAccount();
        account.Deposit(500.00m);  // Deposit money
        Console.WriteLine($"Balance: {account.GetBalance():C}");  // Check balance
    }
}
```
In a real-world bank account, you can't just take money out of the vault or look at the books yourself. You need to interact with the bank via ATMs or customer service, which controls access to your account.

**Polymorphism**:

1. Polymorphism is a core concept in object-oriented programming (OOP) that allows objects to be treated as instances of their parent class rather than their actual derived class.  
**Polymorphism** means "many forms." It allows a method or function to behave differently based on the object it is acting upon. It can be **compile-time** (method overloading) or **runtime** (method overriding).

**Real-Life Example**: Notification System
```c#
// Base class
public class Notification
{
    public virtual void Send()
    {
    Console.WriteLine("Sending a generic notification.");
    }
}
// Derived class for email notifications
public class EmailNotification : Notification
{
    public override void Send()
    {
    Console.WriteLine("Sending an email notification.");
    }
}
// Derived class for SMS notifications
public class SMSNotification : Notification
{
    public override void Send()
    {
    Console.WriteLine("Sending an SMS notification.");
    }
}
class Program
{
    static void Main()
    {
        Notification notification = new EmailNotification();
        notification.Send(); // Calls EmailNotification's Send method
        notification = new SMSNotification();
        notification.Send(); // Calls SMSNotification's Send method
    }
}
```
Consider a notification system. Whether the notification is an email, SMS, or push notification, the system knows how to "send" the message based on the type of notification, even though the method called is the same (Send()).
## Q16: What are the different types of Inheritance?
**Inheritance**

1. In C#, inheritance allows a class (called a derived class) to inherit members (fields, methods, properties) from another class (called a base class). This enables code reuse and polymorphism.

**1. Single Inheritance**

In single inheritance, a class derives from only one base class. This is the simplest and most common form of inheritance.

**Real-World Example**: Employee and Manager
```c#
// Base class
public class Employee
{
    public string Name { get; set; }
    public int EmployeeId { get; set; }

    public void Work()
    {
        Console.WriteLine($"{Name} is working.");
    }
}

// Derived class
public class Manager : Employee
{
    public void Manage()
    {
        Console.WriteLine($"{Name} is managing the team.");
    }
}

class Program
{
    static void Main()
    {
        Manager manager = new Manager { Name = "Alice", EmployeeId = 101 };
        manager.Work();  // Inherited from Employee
        manager.Manage();  // Method in Manager class
    }
}
```
**Explanation**:

Manager class inherits from Employee. Manager can access Work() method from Employee without redefining it.

**2. Multilevel Inheritance**

In multilevel inheritance, a class derives from another derived class, forming a chain of inheritance.

**Real-World Example**: Person → Employee → Manager
```c#
// Base class
public class Person
{
    public string FullName { get; set; }

    public void Introduce()
    {
        Console.WriteLine($"Hello, my name is {FullName}.");
    }
}

// Derived from Person
public class Employee : Person
{
    public int EmployeeId { get; set; }

    public void Work()
    {
        Console.WriteLine($"{FullName} is working.");
    }
}

// Derived from Employee
public class Manager : Employee
{
    public void Manage()
    {
        Console.WriteLine($"{FullName} is managing a team.");
    }
}

class Program
{
    static void Main()
    {
        Manager manager = new Manager { FullName = "Bob", EmployeeId = 102 };
        manager.Introduce();  // Inherited from Person
        manager.Work();  // Inherited from Employee
        manager.Manage();  // Defined in Manager class
    }
}
```
**Explanation**:

Manager inherits from Employee, and Employee inherits from Person. Manager can access members from both Employee and Person.

**3. Hierarchical Inheritance**

In hierarchical inheritance, multiple classes inherit from a single base class.

**Real-World Example**: Employee, Contractor, and Intern inherit from Person
```c#
// Base class
public class Person
{
    public string FullName { get; set; }

    public void Introduce()
    {
        Console.WriteLine($"Hello, my name is {FullName}.");
    }
}

// Derived class
public class Employee : Person
{
    public int EmployeeId { get; set; }

    public void Work()
    {
        Console.WriteLine($"{FullName} is working as an employee.");
    }
}

// Another derived class
public class Contractor : Person
{
    public string ContractId { get; set; }

    public void Work()
    {
        Console.WriteLine($"{FullName} is working as a contractor.");
    }
}

// Another derived class
public class Intern : Person
{
    public string School { get; set; }

    public void Study()
    {
        Console.WriteLine($"{FullName} is studying at {School}.");
    }
}

class Program
{
    static void Main()
    {
        Employee emp = new Employee { FullName = "Charlie", EmployeeId = 103 };
        Contractor contractor = new Contractor { FullName = "Dave", ContractId = "C104" };
        Intern intern = new Intern { FullName = "Eve", School = "Tech University" };

        emp.Introduce();
        emp.Work();
        
        contractor.Introduce();
        contractor.Work();
        
        intern.Introduce();
        intern.Study();
    }
}
```
**Explanation**:

Multiple derived classes (Employee, Contractor, Intern) inherit from the single base class Person. Each subclass can have its own methods but still share common functionality (like Introduce() from Person).

**4. Multiple Inheritance (Not Supported Directly in C#)**

C# does not support multiple inheritance (where a class can inherit from more than one base class) directly. However, multiple inheritance can be achieved using interfaces.

**Real-World Example**: IWorker and IManager
```c#
// Interface 1
public interface IWorker
{
    void Work();
}

// Interface 2
public interface IManager
{
    void Manage();
}

// Class implementing both interfaces
public class Employee : IWorker, IManager
{
    public void Work()
    {
        Console.WriteLine("Employee is working.");
    }

    public void Manage()
    {
        Console.WriteLine("Employee is managing the team.");
    }
}

class Program
{
    static void Main()
    {
        Employee employee = new Employee();
        employee.Work();  // From IWorker interface
        employee.Manage();  // From IManager interface
    }
}
```
**Explanation**:

Employee implements both IWorker and IManager interfaces, achieving the effect of multiple inheritance by inheriting behavior from both.

**5. Hybrid Inheritance**

Hybrid inheritance is a combination of two or more types of inheritance. This typically involves a mix of single, multilevel, hierarchical, or interface-based inheritance. Since C# does not directly support multiple inheritance for classes, hybrid inheritance often relies on interfaces.

**Real-World Example**: Combining Interface and Multilevel Inheritance
```c#
// Interface
public interface IReportable
{
    void GenerateReport();
}

// Base class
public class Employee
{
    public string Name { get; set; }

    public void Work()
    {
        Console.WriteLine($"{Name} is working.");
    }
}

// Derived class from Employee and implementing interface
public class Manager : Employee, IReportable
{
    public void Manage()
    {
        Console.WriteLine($"{Name} is managing the team.");
    }

    public void GenerateReport()
    {
        Console.WriteLine($"{Name} is generating a report.");
    }
}

class Program
{
    static void Main()
    {
        Manager manager = new Manager { Name = "Frank" };
        manager.Work();  // Inherited from Employee
        manager.Manage();  // Defined in Manager
        manager.GenerateReport();  // Defined from IReportable
    }
}
```
**Explanation**:

Manager class inherits from Employee (multilevel inheritance) and also implements IReportable interface. This is a mix of inheritance and interface implementation (hybrid inheritance).
## 🤔What is the difference between single and multilevel inheritance?
**👉Single Inheritance**: A class derives from only one base class.

**👉Multilevel Inheritance**: A class derives from a derived class, forming a chain.

**👉Single Inheritance Example**:
```c#
public class Employee
{
    public string Name { get; set; }
    public int EmployeeId { get; set; }
}

public class Developer : Employee
{
    public void Code()
    {
        Console.WriteLine($"{Name} is writing code.");
    }
}
```
**👉Multilevel Inheritance Example**:
```c#
public class Person
{
    public string FullName { get; set; }
}

public class Employee : Person
{
    public int EmployeeId { get; set; }
}

public class Manager : Employee
{
    public void ManageTeam()
    {
        Console.WriteLine($"{FullName} is managing the team.");
    }
}
```
**🤔16.1 Why c# does not allow multiple inheritance?**

C# does not allow multiple inheritance (i.e., inheriting from more than one class) primarily to avoid complexity and ambiguity that arise with it. Here are the main reasons:

1. **Diamond Problem**: The Diamond Problem is a well-known issue in multiple inheritance scenarios, where a class inherits from two classes that both inherit from the same base class. This can create ambiguity about which path to follow when accessing members from the shared base class. Since C# does not support multiple inheritance (you can inherit from only one class), this problem doesn’t directly arise, but understanding it is helpful for grasping why C# only allows single inheritance.

Visual Representation of the Diamond Problem
Here’s a visual layout of the inheritance chain:
```c#
        Employee
       /         \
   Manager      ProjectLead
       \         /
     SeniorEmployee
```
The SeniorEmployee class gets two copies of the GetBonus() method, one through each parent class (Manager and ProjectLead). This duplication causes the ambiguity.

```c#
using System;

public interface IBonus
{
    void GetBonus();
}

public class Person
{
    public string Name { get; set; }
}

public class Manager : Person, IBonus
{
    public void GetBonus()
    {
        Console.WriteLine("Bonus from Manager class.");
    }
}

public class TeamLead : Person, IBonus
{
    public void GetBonus()
    {
        Console.WriteLine("Bonus from TeamLead class.");
    }
}

public class ManagerLead : Person, IBonus
{
    private readonly IBonus _managerBonus;
    private readonly IBonus _teamLeadBonus;

    public ManagerLead(IBonus managerBonus, IBonus teamLeadBonus)
    {
        _managerBonus = managerBonus;
        _teamLeadBonus = teamLeadBonus;
    }

    public void GetBonus()
    {
        Console.WriteLine("Bonus from ManagerLead class.");
        _managerBonus.GetBonus();
        _teamLeadBonus.GetBonus();
    }
}

// Usage
public class Program
{
    public static void Main()
    {
        IBonus manager = new Manager();
        IBonus teamLead = new TeamLead();
        ManagerLead managerLead = new ManagerLead(manager, teamLead);

        managerLead.GetBonus();
    }
}
```
**Output**:
```c#
Bonus from ManagerLead class.
Bonus from Manager class.
Bonus from TeamLead class.
```
This approach avoids the Diamond Problem by using composition over inheritance: ManagerLead composes both Manager and TeamLead behaviors without inheriting multiple paths, thus sidestepping any ambiguity.

**👉Why This is a Problem?**

In languages that allow multiple inheritance (like C++), the Diamond Problem can create ambiguity and bugs:

- **Ambiguous Methods**: If you call GetBonus() on ManagerLead, it’s unclear which method will run.
- **Redundant Base Calls**: Person might be constructed twice (once through Manager and once through TeamLead), potentially causing wasted resources or inconsistent data.

2. **Simplicity in Design**: C# **favors composition over inheritance**. Instead of inheriting multiple classes, you can use interfaces to achieve polymorphic behavior, and use composition to include the functionality of other classes. This results in more modular, maintainable, and flexible code.

**👉Favor composition over inheritance** means building complex objects by combining simple ones (composition), instead of using rigid hierarchies (inheritance).

In simpler terms:

Composition: Instead of creating a long chain of classes, you create smaller, independent parts (objects) and "compose" them together as needed.
Why? It makes the code more flexible, reusable, and easier to change without breaking other parts.

**👉what is rigid**?

**Rigid** in software design means something that is difficult to change without causing issues or needing many other changes. Rigid code has tight dependencies that make it inflexible, so even a small change can affect multiple parts of the system.

**👉Expanded Real-World Example: Designing a Payment System**

Imagine we’re building a payment system for an e-commerce platform where we can support various payment methods, including Credit Card, PayPal, Gift Card, and others. In addition, we want to add features like Payment Logging and Fraud Detection to some payment methods.

```c#
public abstract class Payment
{
    public abstract void Process();
}
```
```c#
public class CreditCardPayment : Payment
{
    public override void Process() => Console.WriteLine("Processing Credit Card Payment");
}

public class PayPalPayment : Payment
{
    public override void Process() => Console.WriteLine("Processing PayPal Payment");
}

public class GiftCardPayment : Payment
{
    public override void Process() => Console.WriteLine("Processing Gift Card Payment");
}
```
**👉Problem with Inheritance Approach**

If we start by creating a base Payment class and inherit from it, things can quickly become unmanageable, especially when combining multiple payment methods with various features.

**👉The Problems**

Now, let’s say we want to add Logging and Fraud Detection capabilities to different payment types. With inheritance, we might be tempted to create multiple subclasses like:

- LoggedCreditCardPayment, LoggedPayPalPayment, FraudDetectionCreditCardPayment, and so on.
- This leads to a combinatorial explosion of subclasses and tight coupling:

Each new feature requires additional subclasses.
Adding or removing a feature in one class affects other classes.
Testing and maintaining each subclass becomes challenging.

**👉Solution: Favor Composition**

Instead of extending Payment into specific types with every combination, we’ll use composition to build a more flexible, modular solution.

**👉Step-by-Step Solution Using Composition**

Define the IPayment Interface
We’ll create an interface that each payment method will implement.
```c#
public interface IPayment
{
    void Process();
}
```
Implement Payment Method Classes
Each payment method—CreditCardPayment, PayPalPayment, and GiftCardPayment—will implement IPayment individually.
```c#
public class CreditCardPayment : IPayment
{
    public void Process() => Console.WriteLine("Processing Credit Card Payment");
}

public class PayPalPayment : IPayment
{
    public void Process() => Console.WriteLine("Processing PayPal Payment");
}

public class GiftCardPayment : IPayment
{
    public void Process() => Console.WriteLine("Processing Gift Card Payment");
}
```
Define Payment Features as Decorators
Here, we’ll use the Decorator Pattern to add logging and fraud detection features, without modifying the original payment classes.
```c#
public class LoggingDecorator : IPayment
{
    private readonly IPayment _payment;

    public LoggingDecorator(IPayment payment) => _payment = payment;

    public void Process()
    {
        Console.WriteLine("Logging payment process...");
        _payment.Process();
    }
}

public class FraudDetectionDecorator : IPayment
{
    private readonly IPayment _payment;

    public FraudDetectionDecorator(IPayment payment) => _payment = payment;

    public void Process()
    {
        Console.WriteLine("Performing fraud detection...");
        _payment.Process();
    }
}
```
Compose Payment Features Dynamically
Now, instead of creating new subclasses, we can compose our IPayment objects with the necessary decorators at runtime. This approach allows us to dynamically build the functionality based on requirements.
```c#
// Basic credit card payment
IPayment creditCardPayment = new CreditCardPayment();
creditCardPayment.Process();

// Credit card payment with logging
IPayment loggedCreditCardPayment = new LoggingDecorator(new CreditCardPayment());
loggedCreditCardPayment.Process();

// Credit card payment with fraud detection and logging
IPayment secureLoggedCreditCardPayment = new FraudDetectionDecorator(new LoggingDecorator(new CreditCardPayment()));
secureLoggedCreditCardPayment.Process();
```
```c#
// Only using the decorated instance
IPayment loggedCreditCardPayment = new LoggingDecorator(new CreditCardPayment());
loggedCreditCardPayment.Process();
```
To avoid redundant calls, ensure that you call only the decorated instance (loggedCreditCardPayment) where additional behavior is needed, and leave out the original instance if it’s not required in the current context.

Explanation: How Composition Solves the Problems

**👉Modularity and Flexibility**:

We create individual classes (CreditCardPayment, PayPalPayment, etc.) and add features (LoggingDecorator, FraudDetectionDecorator) independently.
Each feature is modular and reusable. We can add logging to any payment method by simply wrapping it in LoggingDecorator, without creating additional subclasses.

**👉Dynamic Feature Composition**:

By composing objects at runtime, we can add or remove features (like logging or fraud detection) without altering any class.
This avoids subclass explosion since we’re not hard-coding combinations.

**👉Open/Closed Principle**:

Composition allows classes to be open for extension but closed for modification. We don’t need to modify CreditCardPayment or PayPalPayment to add logging or fraud detection.

**👉Easier Testing and Maintenance**:

Each component can be tested independently. For instance, LoggingDecorator can be tested with different payment methods.
The single-responsibility principle is better respected, as each class has a focused purpose.
3. **Interfaces as an Alternative**: C# supports multiple interface inheritance, allowing a class to implement multiple interfaces. This provides a way to achieve polymorphism without the downsides of multiple inheritance.

## 🤔Q17: What are access modifiers in C#? How do they relate to encapsulation?
Access modifiers in C# (like public, private, protected, internal) control the visibility of class members, supporting encapsulation by allowing you to hide certain details from external access.

**Real-Life Example**: A Bank Account
```c#
public class BankAccount
{
    private decimal balance;  // Only accessible within the class

    public void Deposit(decimal amount)  // Publicly accessible
    {
        if (amount > 0) balance += amount;
    }

    public decimal GetBalance()  // Publicly accessible method
    {
        return balance;
    }
}
```
**Interview Tip**: Explain how access modifiers help encapsulate the data, ensuring only the required parts are accessible from outside the class.
## 🤔🤔🤔Q18: In a real-life e-commerce application, you have a class called OrderManager responsible for creating, updating, and deleting orders as well as sending confirmation emails and updating inventory. How would you refactor this class to follow the Single Responsibility Principle?
**What is the Single Responsibility Principle? Can you give a real-life example?**

**S — Single Responsibility Principle (SRP)**: The Single Responsibility Principle states that a class should have only one reason to change, meaning it should have only one responsibility or purpose.
To follow SRP, OrderManager should focus on managing order data only. we need to separate each responsibility within OrderManager. Currently, OrderManager is handling multiple responsibilities:
- Managing the lifecycle of orders (creating, updating, and deleting orders).
- Sending confirmation emails.
- Updating inventory.
Here’s a refactored version where each responsibility is isolated into its own class

**Example Refactor:**
```c#
public class OrderManager
{
    private readonly OrderService _orderService;
    private readonly IEmailService _emailService;
    private readonly IInventoryService _inventoryService;

    public OrderManager(OrderService orderService, IEmailService emailService, IInventoryService inventoryService)
    {
        _orderService = orderService;
        _emailService = emailService;
        _inventoryService = inventoryService;
    }

    public async Task CreateOrderAsync(Order order)
    {
        await _orderService.CreateOrderAsync(order);
        await _emailService.SendConfirmationAsync(order);
        await _inventoryService.UpdateStockAsync(order);
    }
}
```
**1. OrderService**
```c#
public class OrderService
{
    public async Task CreateOrderAsync(Order order)
    {
        // Code to save the order to the database (async if I/O bound)
        Console.WriteLine("Order created in the database.");
    }

    public async Task UpdateOrderAsync(Order order)
    {
        // Code to update order in the database
        Console.WriteLine("Order updated in the database.");
    }

    public async Task DeleteOrderAsync(int orderId)
    {
        // Code to delete order from the database
        Console.WriteLine("Order deleted from the database.");
    }
}
```
**2. IEmailService and EmailService**
Interface and implementation for sending emails.
```c#
public interface IEmailService
{
    Task SendConfirmationAsync(Order order);
}

public class EmailService : IEmailService
{
    public async Task SendConfirmationAsync(Order order)
    {
        // Asynchronously send an email confirmation
        Console.WriteLine($"Email confirmation sent for Order ID: {order.Id}");
    }
}
```
**3. IInventoryService and InventoryService**
Interface and implementation for updating inventory.
```c#
public interface IInventoryService
{
    Task UpdateStockAsync(Order order);
}

public class InventoryService : IInventoryService
{
    public async Task UpdateStockAsync(Order order)
    {
        // Asynchronously update the inventory stock
        Console.WriteLine($"Inventory updated for Order ID: {order.Id}");
    }
}
```
Explanation
1. **Single Responsibility**: Each service (OrderService, EmailService, InventoryService) is dedicated to a single responsibility, making the code modular and more maintainable.

2. **Asynchronous Operations**: By making external services asynchronous, we can avoid blocking the main thread during I/O operations, which is ideal in real-world applications.

3. **Loose Coupling**: The OrderManager now depends on abstractions (IEmailService, IInventoryService), so we can swap out implementations easily, which also supports testing and future scaling.

This setup follows the SRP effectively and prepares the application for any additional features without impacting the core OrderManager logic.

**Problem:**
```c#
public async Task CreateOrderAsync(Order order)
{
    await _orderService.CreateOrderAsync(order);
    await _emailService.SendConfirmationAsync(order);
    await _inventoryService.UpdateStockAsync(order);
}
```
in the above code snippet we're using await & the method pauses and waits for this task to complete before moving to the next line. also, it changes the behavior to a sequential, synchronous-like execution.

**How to Improve the Code with Parallel Execution**
If the tasks are truly independent and do not rely on each other's results or have no dependencies, you can run them concurrently to improve performance.

Here’s how to refactor the code to run these tasks in parallel:
```c#
public async Task CreateOrderAsync(Order order)
{
    // Start all tasks without awaiting
    var orderTask = _orderService.CreateOrderAsync(order);
    var emailTask = _emailService.SendConfirmationAsync(order);
    var inventoryTask = _inventoryService.UpdateStockAsync(order);

    // Await all tasks to complete in parallel
    await Task.WhenAll(orderTask, emailTask, inventoryTask);
}
```
**Explanation of Each Line**
Each of these lines looks like they’re being executed sequentially, but in reality, they’re starting each task asynchronously without blocking the execution flow. Here’s a breakdown:
1. var orderTask = _orderService.CreateOrderAsync(order);
    - **What’s Happening**: This line calls the asynchronous method FetchOrdersAsync() and immediately assigns the resulting Task to ordersTask.
    - **Behind the Scenes**: The FetchOrdersAsync method might, for instance, start an HTTP request or database call and return a Task representing this ongoing work. However, because there’s no await here, the method doesn’t wait for the request to complete. Instead,         it immediately moves on to the next line.
    - **Real-World Parallel**: Imagine asking a colleague to go fetch order data for you, and without waiting for them to return, you turn to another colleague and ask them for inventory data.
as well as for 
- var emailTask = _emailService.SendConfirmationAsync(order);
- var inventoryTask = _inventoryService.UpdateStockAsync(order);

**Why It’s Asynchronous, Not Synchronous**
- **No await Means No Waiting**: By not using await immediately, you’re not pausing or blocking execution to wait for each task to complete. Each call to FetchOrdersAsync, FetchInventoryAsync, and FetchCustomersAsync returns a Task that represents the ongoing work,        but the method moves on without waiting.

- **All Tasks Run in Parallel**: The Task objects (ordersTask, inventoryTask, and customersTask) represent operations that are all happening in parallel. They’re “promises” of future results but are not awaited yet, so the application doesn’t stop to wait for any of 
    them at this stage.

**When Does the Method Actually Wait?**

The waiting occurs later with:
```c#
await Task.WhenAll(orderTask, emailTask, inventoryTask);
```
This line **awaits all tasks to complete**, but only at this point does the method pause. Up until this line, each task started running asynchronously and concurrently, allowing other work to be done or other requests to be handled while they’re in progress.

**Real-World Difference**
  - **Sequential Execution with Await per Line**: Each data fetch happens one after another. If each task takes, say, 1 second, the total time to get all data would be approximately 3 seconds (1 second for orders, 1 for inventory, and 1 for customers).
  - **Parallel Execution with Task.WhenAll**: All tasks start at the same time and complete independently. Here, if each fetch still takes 1 second, the total time to get all data is approximately 1 second (since they’re all running concurrently).

**When to Use Sequential vs. Parallel**

  - **Sequential (await one after another)**: Use this if tasks depend on each other’s completion or need results from the previous task.

  - **Parallel (Task.WhenAll)**: Use this if tasks are independent and do not rely on the output of each other.

Using Task.WhenAll here provides a more optimized solution if each service can safely execute without waiting for another.
### Summary
By not using await immediately, we’ve started all three tasks without waiting. The tasks run asynchronously and concurrently, which makes it possible to fetch all three sets of data simultaneously rather than sequentially. Only when we reach await Task.WhenAll(...) does the method wait for all tasks to complete, making the process highly efficient.
## 🤔🤔🤔Q19: Imagine you’re working on a payment processing system with multiple payment methods, such as credit card, PayPal, and bank transfer. How would you design it to be easily extendable for adding new payment methods without modifying existing code?
**What is the Open/Closed Principle? Can you provide a real-life example?**

**O — Open/Closed Principle (OCP)**: The Open/Closed Principle means that classes should be open for extension but closed for modification. You should be able to add new functionality without changing existing code.

**Real-Life Example: Payment Processing System**

In a system that processes payments via different methods (Credit Card, PayPal, etc.), instead of modifying the existing PaymentProcessor class to add new payment methods, we use interfaces or abstract classes and extend the functionality by creating new classes.

**by abstract class**:
```c#
public abstract class PaymentProcessor
{
    public abstract void ProcessPayment(decimal amount);
}

// Existing payment methods
public class CreditCardPayment : PaymentProcessor
{
    public override void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing credit card payment of {amount:C}.");
    }
}

// Adding a new payment method without modifying existing code
public class PayPalPayment : PaymentProcessor
{
    public override void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing PayPal payment of {amount:C}.");
    }
}
```
**by interface**
```c#
public interface IPaymentProcessor
{
    void ProcessPayment(decimal amount);
}

public class CreditCardProcessor : IPaymentProcessor { /* implementation */ }
public class PayPalProcessor : IPaymentProcessor { /* implementation */ }

public class PaymentService
{
    private readonly IPaymentProcessor _paymentProcessor;

    public PaymentService(IPaymentProcessor paymentProcessor)
    {
        _paymentProcessor = paymentProcessor;
    }

    public void ProcessPayment(decimal amount)
    {
        _paymentProcessor.ProcessPayment(amount);
    }
}
```
**Interview Tip**: Demonstrate how adding new functionality (like a new payment method) involves creating a new class (PayPalPayment) rather than modifying the existing PaymentProcessor or CreditCardPayment class. This makes the system easier to maintain and extend.
## 🤔🤔🤔Q19: Imagine you have a payment processing system that handles different types of payment methods (e.g., credit card, PayPal, or bank transfer). You have a base class PaymentProcessor that defines common behavior for all payment types, and specific processors like CreditCardProcessor and PayPalProcessor inherit from this base class.
**What is the Liskov Substitution Principle (LSP)? Can you provide a real-life example?**

**L — Liskov Substitution Principle (LSP)**: The Liskov Substitution Principle (LSP) is one of the five SOLID principles. It states that objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program. In other words, a subclass should be able to stand in for its parent class without breaking the behavior expected from the parent class.

**Real-World Example 1: Payment Processing System**

**Context**:

Imagine you have a payment processing system that handles different types of payment methods (e.g., credit card, PayPal, or bank transfer). You have a base class PaymentProcessor that defines common behavior for all payment types, and specific processors like CreditCardProcessor and PayPalProcessor inherit from this base class.

**Code Example**:
```c#
public abstract class PaymentProcessor
{
    public abstract void ProcessPayment(decimal amount);
}

public class CreditCardProcessor : PaymentProcessor
{
    public override void ProcessPayment(decimal amount)
    {
        // Process credit card payment
        Console.WriteLine($"Processing credit card payment of {amount}");
    }
}

public class PayPalProcessor : PaymentProcessor
{
    public override void ProcessPayment(decimal amount)
    {
        // Process PayPal payment
        Console.WriteLine($"Processing PayPal payment of {amount}");
    }
}
```
**Explanation**:
- CreditCardProcessor and PayPalProcessor are subclasses of PaymentProcessor.
- LSP states that you should be able to substitute any subclass for the parent class without affecting the behavior of the application.

**Usage:**
```c#
public class Checkout
{
    public void ProcessOrder(PaymentProcessor paymentProcessor, decimal amount)
    {
        paymentProcessor.ProcessPayment(amount);
    }
}
```
Here, you can pass either CreditCardProcessor or PayPalProcessor to the ProcessOrder method, and the program will work correctly without modifying the Checkout class. Both subclasses follow LSP because they behave in ways that are consistent with the base class.

**Real-World Example 2: Notification System**

**Context**:

Consider a system that sends notifications via different channels, like email, SMS, and push notifications. You have a base class NotificationSender, and subclasses like EmailSender, SmsSender, and PushNotificationSender.

**Code Example**:
```c#
public abstract class NotificationSender
{
    public abstract void SendNotification(string message);
}

public class EmailSender : NotificationSender
{
    public override void SendNotification(string message)
    {
        // Send email notification
        Console.WriteLine($"Sending email: {message}");
    }
}

public class SmsSender : NotificationSender
{
    public override void SendNotification(string message)
    {
        // Send SMS notification
        Console.WriteLine($"Sending SMS: {message}");
    }
}
```
**Explanation**:
- EmailSender and SmsSender are subclasses of NotificationSender.
- They adhere to LSP because either one can replace the base class NotificationSender without breaking the functionality.

**Usage**:
```c#
public class NotificationService
{
    public void Notify(NotificationSender sender, string message)
    {
        sender.SendNotification(message);
    }
}
```
You can pass either EmailSender or SmsSender to the Notify method, and it will work without issues, thanks to LSP. Each sender behaves according to its own type but maintains the contract defined by the NotificationSender base class.
## 🤔🤔🤔Q20:  You have an IEmployee interface with methods like Work, AttendMeeting, TakeBreak, and ProcessPayroll. Why does this violate ISP, and how can you refactor it?
**What is the Interface Segregation Principle? How would you apply it in a real-life situation?**

**I — Interface Segregation Principle (ISP)**: The Interface Segregation Principle states that no client should be forced to implement methods it does not use. Instead of having large, general-purpose interfaces, it's better to have smaller, specific ones.

Having all these methods in IEmployee forces every implementation of the interface to include methods that may not be relevant to its role (e.g., a contractor who does not need payroll processing). Breaking down the IEmployee interface into more specific interfaces, such as IWorkable, IAttendable, and IPayable, allows each class to implement only what it needs.
```c#
public interface IWorkable { void Work(); }
public interface IAttendable { void AttendMeeting(); }
public interface IPayable { void ProcessPayroll(); }

public class FullTimeEmployee : IWorkable, IAttendable, IPayable { /* implementations */ }
public class Contractor : IWorkable, IAttendable { /* implementations */ }
```
**Real-Life Example: User Management System**

In a user management system, different types of users might require different functionalities. A large IUser interface that includes methods for all user types (like admin, guest, regular user) violates ISP. Instead, you should create smaller interfaces.
```c#
// Too large interface, violates ISP
public interface IUser
{
    void Login();
    void ManageUsers();  // Only admins need this method
    void ViewReports();  // Only specific users need this
}

// Applying ISP by splitting into smaller interfaces
public interface IRegularUser
{
    void Login();
}

public interface IAdminUser : IRegularUser
{
    void ManageUsers();
}

public interface IReportViewer : IRegularUser
{
    void ViewReports();
}

public class Admin : IAdminUser
{
    public void Login() => Console.WriteLine("Admin logged in.");
    public void ManageUsers() => Console.WriteLine("Admin managing users.");
}

public class RegularUser : IRegularUser
{
    public void Login() => Console.WriteLine("Regular user logged in.");
}
```
**Interview Tip**: Discuss how splitting the interface into smaller, more specific interfaces (like IAdminUser, IReportViewer, IRegularUser) ensures that clients (classes) only implement what they need. This reduces unnecessary code and makes the system more modular.
## 🤔🤔🤔Q21:  In a notification system, you have a NotificationService that sends emails and SMS messages directly by instantiating EmailSender and SmsSender classes within it. How can you refactor this to follow the Dependency Inversion Principle?
**What is the Dependency Inversion Principle? Can you explain with a real-world example?**

**D — Dependency Inversion Principle (DIP)**: The Dependency Inversion Principle states that high-level modules should not depend on low-level modules; both should depend on abstractions. Also, abstractions should not depend on details; details should depend on abstractions.

Instead of directly instantiating EmailSender and SmsSender within NotificationService, we should depend on abstractions by introducing an INotificationSender interface that both EmailSender and SmsSender can implement. This allows NotificationService to use any sender that implements INotificationSender, improving flexibility and testability.
```c#
public interface INotificationSender
{
    void Send(string message);
}

public class EmailSender : INotificationSender { /* implementation */ }
public class SmsSender : INotificationSender { /* implementation */ }

public class NotificationService
{
    private readonly INotificationSender _notificationSender;

    public NotificationService(INotificationSender notificationSender)
    {
        _notificationSender = notificationSender;
    }

    public void SendNotification(string message)
    {
        _notificationSender.Send(message);
    }
}
```
**Real-Life Example: Notification System**

In a notification system, high-level modules (like a service sending notifications) should depend on abstractions (interfaces), not on concrete implementations (like email, SMS, etc.).
```c#
// Usage
var emailSender = new EmailNotificationSender();
var notificationService = new NotificationService(emailSender);
notificationService.Notify("Your order has been shipped.");

// High-level module
public class NotificationService
{
    private readonly INotificationSender _notificationSender;

    public NotificationService(INotificationSender notificationSender)
    {
        _notificationSender = notificationSender;
    }

    public void Notify(string message)
    {
        _notificationSender.Send(message);
    }
}

// Abstraction
public interface INotificationSender
{
    void Send(string message);
}

// Low-level modules (implementations)
public class EmailNotificationSender : INotificationSender
{
    public void Send(string message)
    {
        Console.WriteLine($"Sending Email: {message}");
    }
}

public class SMSNotificationSender : INotificationSender
{
    public void Send(string message)
    {
        Console.WriteLine($"Sending SMS: {message}");
    }
}
```
**Interview Tip**: Emphasize that the NotificationService depends on the INotificationSender interface (abstraction), not on specific implementations like EmailNotificationSender or SMSNotificationSender. This makes the system flexible—changing the notification method doesn’t affect the high-level module.
## 🤔🤔🤔Q22:  How do SOLID principles help in writing better code?
Adhering to SOLID principles, especially DIP, makes it easier to mock dependencies in unit tests. For instance, if a service depends on an interface instead of a concrete implementation, you can easily inject a mock or stub to test different scenarios without touching the actual implementation.
## 🤔🤔🤔Q22: What is the Dependency Inversion Principle (DIP), and how does it differ from Dependency Injection?
**DIP**: The Dependency Inversion Principle (DIP) is a design principle in the SOLID principles that says:
1. High-level modules (business logic) should not depend on low-level modules (details). Both should depend on abstractions. Abstractions should not depend on details. Details (implementations) should depend on abstractions.
2. DIP is one of the SOLID principles and states that high-level modules should not depend on low-level modules. Both should depend on abstractions (interfaces or abstract classes). This decouples classes and makes the system more flexible and maintainable. Dependency Injection is a technique used to implement DIP by injecting dependencies (like services) rather than hardcoding them inside classes.

**How is DIP applied in your code?**

**High-Level Module:**
```c#
public class NotificationService
{
	private readonly INotificationSender _notificationSender;

	public NotificationService(INotificationSender notificationSender)
	{
		_notificationSender = notificationSender;
	}

	public void Notify(string message)
	{
		_notificationSender.Send(message);
	}
}
```
NotificationService is the high-level module. It’s responsible for notifying users with messages.

Instead of depending directly on low-level modules (like EmailNotificationSender or SMSNotificationSender), it depends on an abstraction (INotificationSender).

**Low-Level Modules**:
```c#
public class EmailNotificationSender : INotificationSender
{
    public void Send(string message)
    {
        Console.WriteLine($"Sending Email: {message}");
    }
}

public class SMSNotificationSender : INotificationSender
{
    public void Send(string message)
    {
        Console.WriteLine($"Sending SMS: {message}");
    }
}
```
EmailNotificationSender and SMSNotificationSender are low-level modules that implement the INotificationSender abstraction.

The abstraction (INotificationSender) does not depend on the details of how emails or SMS are sent. Instead, these implementations depend on the abstraction.

**What happens if you don't follow DIP in your design?**
- Violating DIP creates tight coupling between classes, making the system harder to maintain or extend. For example, if a reporting system depends directly on SqlReportGenerator, switching to another reporting method (e.g., CsvReportGenerator) requires changes in every place where SqlReportGenerator is used. By depending on an abstraction (like IReportGenerator), the switch is seamless.

**What is Dependency Injection (DI) and what are the types of DI?**

**DI**: DI is a technique used to inject dependencies into a class, rather than the class creating the dependencies itself. This makes the code more flexible, testable, and easier to maintain. This promotes loose coupling. The three types of DI are:
1. **Constructor Injection**: Dependencies are provided through a class's constructor.
2. **Property (Setter) Injection**: Dependencies are assigned via public properties.
3. **Method Injection**: Dependencies are passed through a method when needed.

**Advantages:**

- Loose Coupling: Classes are decoupled, making the code easier to maintain and extend.
- Testability: It’s easier to unit test components since dependencies can be mocked or substituted.
- Flexibility: Components can be reused and swapped out without modifying the consuming code.

**What are the different types of lifetimes in IoC containers, and how do they affect object creation?**

1. **Transient**: A new instance is created every time it's requested.
2. **Scoped**: A new instance is created per request (or scope), making it useful for web applications.
3. **Singleton**: A single instance is created and shared throughout the application’s lifetime.

**What is Inversion of Control (IoC), and how is it related to Dependency Injection?**

**Inversion of Control (IoC)**: IoC is a broader principle that says the control of object creation and the flow of a program should be inverted from traditional procedural code. Rather than a class controlling its dependencies, external code controls them. Dependency Injection is one specific way to implement IoC, by injecting dependencies into classes rather than allowing them to create their own.

**Can you give a real-life example of IoC?**

In a web application, the framework (like ASP.NET Core) manages the lifecycle of controllers and services. You don't manually instantiate these components; instead, the framework injects them where needed using DI containers, inverting the control of object creation.

**What is an IoC container, and why is it useful?**

An IoC container is a framework that manages object creation and lifetime. It resolves dependencies automatically and injects them where needed. Examples include ASP.NET Core's built-in DI container, Autofac, and Ninject.

**Why it's useful**: It simplifies dependency management by handling object creation, lifecycle management (e.g., singleton, scoped), and dependency resolution, making your code cleaner and easier to maintain.

**How does IoC promote loose coupling in a system?**

IoC removes the responsibility of instantiating dependencies from the classes that use them, allowing classes to depend on abstractions (interfaces) rather than specific implementations. This makes it easier to replace dependencies without modifying the dependent classes, promoting flexibility and reducing coupling.

**What is the difference between Service Locator and Dependency Injection?**

Service Locator and Dependency Injection (DI) are two different patterns used to manage dependencies in software systems. Both aim to decouple classes from their dependencies, but they differ in how they achieve this.

In Dependency Injection, the framework or external code injects dependencies, keeping the class unaware of how its dependencies are being provided. In contrast, a Service Locator involves the class itself actively looking up its dependencies from a registry or service container. DI promotes better separation of concerns, while Service Locator can result in hidden dependencies and tighter coupling.

**1. Dependency Injection** is a design pattern where dependencies (objects that a class depends on) are provided to the class from the outside, typically through the constructor, properties, or methods. The class does not create its own dependencies but instead relies on an external source (e.g., an IoC container) to inject them.

**How it Works:**

- Inversion of Control (IoC) is applied, where the responsibility for providing dependencies is shifted from the class itself to an external container.
- Dependencies are passed to the class, making it easier to change or swap them.

**Real-World Example**:

Imagine you’re working on an email notification system where different methods of sending emails (e.g., SMTP or an external API) are needed.

**Code Example (Constructor Injection)**:
```c#
public interface IEmailService
{
    void SendEmail(string message);
}

public class SmtpEmailService : IEmailService
{
    public void SendEmail(string message)
    {
        Console.WriteLine($"Sending email via SMTP: {message}");
    }
}

public class NotificationService
{
    private readonly IEmailService _emailService;

    // Dependency Injection via constructor
    public NotificationService(IEmailService emailService)
    {
        _emailService = emailService;
    }

    public void Notify(string message)
    {
        _emailService.SendEmail(message);
    }
}
```
### Explanation:
- NotificationService does not create its own IEmailService implementation; instead, it's injected via the constructor.
- This allows you to easily switch from SmtpEmailService to another implementation, like ApiEmailService, without modifying the NotificationService.

**Usage**:
```c#
var emailService = new SmtpEmailService();  // Dependency
var notificationService = new NotificationService(emailService);
notificationService.Notify("Hello, Dependency Injection!");
```
**2. Service Locator**: The Service Locator pattern provides a way to get dependencies by querying a central registry (the locator) that holds the mappings between interfaces and their concrete implementations. The class itself actively requests the dependencies from the service locator.

**How it Works**:

- The class calls a global service locator to get its dependencies, instead of having them injected.
- The service locator can be seen as a container that provides the required services when requested.

**Real-World Example**:

- Let's use the same email notification system example but apply the Service Locator pattern instead.

**Code Example (Service Locator)**:
```c#
public interface IEmailService
{
    void SendEmail(string message);
}

public class SmtpEmailService : IEmailService
{
    public void SendEmail(string message)
    {
        Console.WriteLine($"Sending email via SMTP: {message}");
    }
}

// Service Locator class
public static class ServiceLocator
{
    private static Dictionary<Type, object> _services = new Dictionary<Type, object>();

    public static void Register<T>(T service)
    {
        _services[typeof(T)] = service;
    }

    public static T GetService<T>()
    {
        return (T)_services[typeof(T)];
    }
}

public class NotificationService
{
    public void Notify(string message)
    {
        var emailService = ServiceLocator.GetService<IEmailService>();
        emailService.SendEmail(message);
    }
}
```
### Explanation:
- NotificationService now relies on the Service Locator to obtain the IEmailService implementation when needed. It queries the locator for the service at runtime.
- This reduces the need for injecting the dependency directly but hides the class's dependencies, making the system less transparent.

**Usage**:
```c#
ServiceLocator.Register<IEmailService>(new SmtpEmailService());
var notificationService = new NotificationService();
notificationService.Notify("Hello, Service Locator!");
```
### Key Differences
**1. Dependency Injection**:

- Responsibilities: The class does not know or control where its dependencies come from. They are provided from the outside (via the constructor, property, or method).
- Transparency: Dependencies are explicitly listed in the class (e.g., through constructor parameters), making them easier to understand and test.
- Loose Coupling: The class is fully decoupled from its dependencies.
- Unit Testing: Dependencies can easily be mocked for unit testing.
- Example: A notification service receives an email service through its constructor.

**2. Service Locator**:

- Responsibilities: The class actively retrieves its dependencies from a central service locator.
- Transparency: The dependencies are hidden, making it harder to know what the class depends on just by looking at its interface.
- Tight Coupling to Locator: The class becomes dependent on the service locator, which can make testing and maintenance harder.
- Unit Testing: Requires more setup, as the service locator needs to be properly configured or mocked.
- Example: A notification service queries a service locator to obtain an email service.

**In summary,** while both Service Locator and Dependency Injection decouple object creation from their usage, **Dependency Injection** promotes better transparency, maintainability, and testability. **Service Locator**, on the other hand, can hide dependencies and make testing more difficult, but it can simplify things for smaller projects.

## What is Captive dependency injection?

In **Dependency Injection (DI)**, a **captive dependency** problem arises when a transient dependency is inadvertently captured by a longer-lived object, such as a singleton or scoped service. This can cause unexpected behavior, performance issues, or improper resource management.
**The Problem**:

When a **singleton** service depends on a **transient** service (directly or indirectly), it causes the transient service to live longer than expected, as it becomes "captured" by the singleton’s long lifetime. This is problematic because transient services are meant to be short-lived and may not perform well if they are kept alive for longer than intended.

**Example of a Captive Dependency Problem in .NET**

Let’s say you have the following service lifetimes:
- **Singleton**: Created once and lives throughout the application lifetime.
- **Scoped**: Created once per request or scope.
- **Transient**: Created each time it is requested.
Now, consider this setup:
```c#
public class MySingletonService
{
    private readonly ITransientService _transientService;

    public MySingletonService(ITransientService transientService)
    {
        _transientService = transientService;
    }

    public void PerformOperation()
    {
        _transientService.DoWork(); // Calling the transient service
    }
}

public interface ITransientService
{
    void DoWork();
}

public class TransientService : ITransientService
{
    public void DoWork()
    {
        Console.WriteLine("Doing transient work");
    }
}
```
In the above example:

- MySingletonService is a singleton service.
- TransientService is a transient service.

Here’s the DI registration:
```c#
services.AddSingleton<MySingletonService>();
services.AddTransient<ITransientService, TransientService>();
```
**Why is this a Problem?**
- **Lifetime Mismatch**: The transient service is supposed to be created every time it’s requested. However, since it is injected into a singleton service, the transient service will only be created once and will live as long as the singleton does. This breaks the transient’s intended lifecycle, leading to:
  - Resource leakage.
  - Unexpected side effects (e.g., stale data or incorrect state in the transient service).

**Solution**:
To avoid the captive dependency problem, you can inject a factory or service provider to ensure that transient services are created properly every time they are needed.

**Solution 1: Injecting a Factory or Func**

You can inject a factory method to create a new instance of the transient service whenever needed.
```c#
public class MySingletonService
{
    private readonly Func<ITransientService> _transientServiceFactory;

    public MySingletonService(Func<ITransientService> transientServiceFactory)
    {
        _transientServiceFactory = transientServiceFactory;
    }

    public void PerformOperation()
    {
        var transientService = _transientServiceFactory(); // Create a new transient instance
        transientService.DoWork(); 
    }
}
```
**Registration:**
```c#
services.AddSingleton<MySingletonService>();
services.AddTransient<ITransientService, TransientService>();
```
- In this case, the Func<ITransientService> will create a new instance of TransientService every time PerformOperation is called.
- Func<ITransientService> is a delegate (function pointer) that can be invoked to create a new instance of ITransientService. By storing this delegate, we defer the creation of the transient service until it’s actually needed. This is key to solving the captive dependency problem.

**Solution 2: Injecting IServiceProvider**

Another approach is to inject the IServiceProvider, which allows you to resolve transient services as needed.
```c#
public class MySingletonService
{
    private readonly IServiceProvider _serviceProvider;

    public MySingletonService(IServiceProvider serviceProvider)
    {
        _serviceProvider = serviceProvider;
    }

    public void PerformOperation()
    {
        var transientService = _serviceProvider.GetRequiredService<ITransientService>();
        transientService.DoWork(); 
    }
}
```
**Registration**:
```c#
services.AddSingleton<MySingletonService>();
services.AddTransient<ITransientService, TransientService>();
```
Here, the IServiceProvider creates a new instance of TransientService when requested. This avoids the captive dependency problem because the transient service is created with its expected lifecycle every time it’s needed.
### Summary:
- The captive dependency problem occurs when a transient or scoped service is "captured" by a singleton or scoped service, causing unintended behavior.
- To solve this, avoid injecting transient services directly into singletons. Instead, inject a factory (Func) or the IServiceProvider to create transient services as needed, ensuring their proper lifecycle.
- This ensures that the singleton service can still use transient services without violating their expected lifetimes.
By following these patterns, you can avoid lifecycle mismatches and maintain proper dependency management in your ASP.NET Core application.
## 🤔🤔🤔Q23: What are extension methods and where would you use them?
**Extension methods** in C# allow developers to add new methods to existing types without modifying, deriving from, or recompiling the original types. They are static methods defined in a static class, but called as if they were instance methods on the extended type. Extension methods provide a flexible way to extend the functionality of a class or interface.

To use extension methods, the static class containing the extension method must be in scope, which usually requires a using directive for the namespace of the class.

**Advantages of using extension methods include**:

- Enhancing the functionality of third-party libraries or built-in .NET types without access to the original source code.
- Keeping code cleaner and more readable by encapsulating complex operations into methods.
- Facilitating a fluent interface style of coding, which can make code more expressive.
Here is a simple example demonstrating how to define and use an extension method: 
```c#
using System;

namespace ExtensionMethods
{
    public static class StringExtensions
    {
        // Extension method for the String class
        public static string ToPascalCase(this string input)
        {
            if (string.IsNullOrEmpty(input))
                return input;

            string[] words = input.Split(new char[] { ' ', '-' }, StringSplitOptions.RemoveEmptyEntries);
            for (int i = 0; i < words.Length; i++)
            {
                words[i] = char.ToUpper(words[i][0]) + words[i].Substring(1).ToLower();
                //words[i][0]: This accesses the first character of the word at index i. For example, if words[i] is "hello", words[i][0] would be 'h'.
            }
            return string.Join("", words);
        }
    }
}
```
```c#
class Program
{
    static void Main(string[] args)
    {
        string title = "the quick-brown fox";
        string pascalCaseTitle = title.ToPascalCase();
        Console.WriteLine(pascalCaseTitle); // Outputs: TheQuickBrownFox
    }
}
```
In this example, ToPascalCase is an extension method defined for the String class. It converts a given string to Pascal case format. To use this extension method, simply call it on a string instance, as shown in the Main method. This demonstrates how extension methods can add useful functionality to existing types in a clean and natural syntax.

Extension methods are a powerful feature for extending the capabilities of types, especially when direct modifications to the class are not possible or desirable.
```c#
public class Product
{
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```
**Adding Extension Method**

Here's an extension method for the Product class to calculate a discount:
```c#
public static class ProductExtensions
{
    // This method calculates the price after applying the discount.
    public static decimal CalculateDiscount(this Product product, decimal discountPercentage)
    {
        if (discountPercentage < 0 || discountPercentage > 100)
            throw new ArgumentOutOfRangeException("Discount percentage must be between 0 and 100.");
        return product.Price - (product.Price * (discountPercentage / 100));
    }
}
```
```c#
Product product = new Product { Name = "Laptop", Price = 1000 };
decimal discountedPrice = product.CalculateDiscount(10); // 10% discount
Console.WriteLine($"Discounted price: {discountedPrice}");
```
Why Use Extension Methods Here?

- **Cleaner Syntax**: Instead of creating a separate DiscountCalculator class or utility function, you can invoke CalculateDiscount directly on the Product object.
- **Encapsulation**: The method logic is encapsulated, and the Product class remains untouched. You don't have to modify or inherit from Product to add this functionality.

### 🤔🤔🤔the"𝙩𝙝𝙞𝙨" keyword:

However, this keyword is powerful in creating 𝐞𝐱𝐭𝐞𝐧𝐬𝐢𝐨𝐧 𝐦𝐞𝐭𝐡𝐨𝐝𝐬.
𝟐𝟎𝟐𝟐 𝐅𝐈𝐅𝐀 𝐖𝐨𝐫𝐥𝐝 𝐂𝐮𝐩 𝐟𝐢𝐧𝐚𝐥 was a football match, Argentina vs France.

This match was held at the 𝙇𝙪𝙨𝙖𝙞𝙡 𝙎𝙩𝙖𝙙𝙞𝙪𝙢 in Qatar, played in front of 88,966 people.

But technically, more than 1.5 𝘣𝘪𝘭𝘭𝘪𝘰𝘯 𝘱𝘦𝘰𝘱𝘭𝘦 watched this match, and final became one of the most widely watched televised sporting events in history.

How did it happen? To make activities light, easy and interesting(in the 88,966 capacity stadium), 1.5 billiion people were connected to 𝐭𝐡𝐢𝐬(using the this keyword) stadium on 𝐭𝐞𝐥𝐞𝐯𝐢𝐬𝐢𝐨𝐧𝐬(using extension methods).

It's now safe to say that, 1.5 billion crowd were in the stadium on that final.
This is very similar to how extension methods work in C#.
From the analogy above, I can say:

Extension method is a static method from an existing container(class,interface, struct...), without modifying or making changes in the original container.
```c#
//Extension Method
public static void AddPermission(CallConvThiscall AuthorizationOptions options, string permission)
{
    options.AddPolicy(permission, policy =>
    {
        policy.RequireAssertion(context =>
        {
            context.User.HasClaim(c => c.Type == permission && bool.Parse(c.Value) == true));
        })
    });
}

//Service Configuration
builder.Services.AddAuthorization(options =>
{
    Enum.GetNames<Permission>().ForEach(AddPermission =>
    {
        options.AddPermission(permission);
    });
});
```
🤔🤔🤔Why use 𝐞𝐱𝐭𝐞𝐧𝐬𝐢𝐨𝐧 𝐦𝐞𝐭𝐡𝐨𝐝𝐬?

👉Extension methods allow you to add new methods to existing types, including those you don't have control over (like built-in .NET types).

👉They can make your code more intuitive and readable by allowing you to chain method calls in a natural way.

👉You can add methods to interfaces without breaking existing implementations.

👉Instead of creating utility classes with static methods, you can use extension methods for a more object-oriented feel.

## 🤔🤔🤔Q24: What are Value Types and Reference Types in C#?
In C#, data types are divided into two categories: Value Types and Reference Types. This distinction affects how values are stored and manipulated within memory.

👉**Value Types**: Store data directly and are allocated on the stack. This means that when you assign one value type to another, a direct copy of the value is created. Basic data types (int, double, bool, etc.) and structs are examples of value types. Operations on value types are generally faster due to stack allocation.

👉**Reference Types**: Store a reference (or pointer) to the actual data, which is allocated on the heap. When you assign one reference type to another, both refer to the same object in memory; changes made through one reference are reflected in the other. Classes, arrays, delegates, and strings are examples of reference types.

Here's a simple example to illustrate the difference:
```c#
// Value type example
int a = 10;
int b = a;
b = 20;
Console.WriteLine(a); // Output: 10
Console.WriteLine(b); // Output: 20

// Reference type example
var list1 = new List<int> { 1, 2, 3 };
var list2 = list1;
list2.Add(4);
Console.WriteLine(list1.Count); // Output: 4
Console.WriteLine(list2.Count); // Output: 4
```
In the value type example, changing b does not affect a because b is a separate copy. In the reference type example, list2 is not a separate copy; it's another reference to the same list object as list1, so changes made through list2 are visible when accessing list1.

## What are Boxing and Unboxing? Where to use Boxing and Unboxing in real applications?
**Boxing** -Boxing is the process of converting from value type to reference type.
**Unboxing** -Unboxing is the process of converting reference type to value type. Unboxing is an explicit conversion process.
```c#
int num = 10;
object obj = num; // Boxing
int i = (int)obj; //Unboxing
```
We internally use boxing when the item is added to ArrayList. We use unboxing when an item is extracted from ArrayList.
```c#
using System.Collections;

int num = 10;
ArrayList arrayList = new();
arrayList.Add(num); ; // Boxing
int i = (int)arrayList[0]; //Unboxing
```
### Summary
- it is recommended to use boxing and unboxing when it is necessary only.
- Converting from one type to another type is not good from a performance point of view.
## 🤔🤔🤔Q24: What are delegates and how are they used in C#?
**Delegates** in C# are type-safe function pointers or references to methods with a specific parameter list and return type. They allow methods to be passed as parameters, stored in variables, and returned by other methods, which enables flexible and extensible programming designs such as event handling and callback methods. Delegates are particularly useful in implementing the observer pattern and designing frameworks or components that need to notify other objects about events or changes without knowing the specifics of those objects.

There are three main types of delegates in C#:

- Single-cast delegates: Point to a single method at a time.
- Multicast delegates: Can point to multiple methods on a single invocation list. A multicast delegate is a delegate that holds references to more than one method. When invoked, all methods in the invocation list are called sequentially.
- Anonymous methods/Lambda expressions: Allow inline methods or lambda expressions to be used wherever a delegate is expected.

**Multicast Delegate**:

1. A multicast delegate is a delegate that holds references to more than one method. When invoked, all methods in the invocation list are called sequentially.  
      
    **What is the difference between delegates and events in C#?**
    
    **Answer:** Delegates are function pointers, whereas events are special delegates that restrict direct access, allowing only subscription (`+=`) and unsubscription (`-=`).
    
    **Real-World Example:** In a real-world scenario like a stock monitoring system:
    
    *   A delegate allows anyone to invoke a notification method (which could be risky).
        
    *   An event restricts users from invoking notification methods directly, providing better encapsulation.  
          
     **Why Use Delegates?**
    
    *   **Loose Coupling**: The order processing logic doesn’t need to know the details of the notification system (Email or SMS). It only calls the delegate, making the system more flexible.
        
    *   **Extensibility**: In the future, if you want to add another notification method (e.g., a push notification), you can just add it to the delegate without changing the core logic of the order processing.  
```c#
EmailNotifier emailNotifier = new();
SmsNotifier smsNotifier = new();
OrderProcessor orderProcessor = new();

orderProcessor.NotifyDelegate += emailNotifier.SendMail;
orderProcessor.NotifyDelegate += smsNotifier.SendSms;

orderProcessor.ProcessOrder();

Console.ReadLine();

public delegate void Notify(Message message);

public class Message
{
    public string Content { get; set; } = string.Empty;
}

public class EmailMessage : Message
{
    public string Subject { get; set; } = string.Empty;
    public string Attachment { get; set; } = string.Empty;
}


public class EmailNotifier
{
    public void SendMail(Message message)
    {
        EmailMessage? emailMessage = message as EmailMessage;
        if (emailMessage is not null)
            Console.WriteLine($"Email Sent:" +
            $"Subject: {emailMessage.Subject}," +
            $"Body: {emailMessage.Content}," +
            $"Attachment: {emailMessage.Attachment}");
    }
}

public class SmsNotifier
{
    public void SendSms(Message message)
    {
        Console.WriteLine($"Sms Sent: {message.Content}");
    }
}

public class OrderProcessor
{
    public Notify? NotifyDelegate { get; set; }
    public void ProcessOrder()
    {
        Console.WriteLine("Order is being processed...");
        Message smsMessage = new()
        {
            Content = "Your order is processed. Thank you for shopping!"
        };

        EmailMessage emailMessage = new EmailMessage
        {
            Subject = "Order Confirmation",
            Content = "Your order has been successfully processed.",
            Attachment = "Invoice.pdf"
        };

        var delegates = NotifyDelegate!.GetInvocationList();
        if (delegates is not null)
        {
            foreach (var notifier in delegates)
            {
                //The idea is to only invoke the appropriate notifier for each message type rather than invoking all subscribed methods for both messages.
                if (notifier.Target is SmsNotifier)
                    notifier.DynamicInvoke(smsMessage);
                else if (notifier.Target is EmailNotifier)
                    notifier.DynamicInvoke(emailMessage);
            }
        }

        //Here is an issue

        //invoking all subscribed methods for both messages//
        //NotifyV2Delegate?.Invoke(smsMessage);
        //NotifyV2Delegate?.Invoke(emailMessage);
    }
}
```   
👉**Without violating OCP**  
```c#
INotify smsNotifier = new SmsNotifier();
INotify emailNotifier = new EmailNotifier();
INotify pushNotifier = new PushNotifier();

List<INotify> notifiers = new() { smsNotifier, emailNotifier, pushNotifier };

OrderProcessor orderProcessor = new OrderProcessor(notifiers);
orderProcessor.ProcessOrder();

Console.ReadLine();

public interface INotify
{
    void Notify();
}

public class SmsNotifier : INotify
{
    public void Notify()
    {
        Message message = new()
        {
            Content = "Your order is processed. Thank you for shopping"
        };

        Console.WriteLine($"SMS Sent: {message.Content}");
    }
}

public class PushNotifier : INotify
{
    public void Notify()
    {
        PushMessage message = new()
        {
            Content = "(Pushing)Your order is processed. Thank you for shopping"
        };

        Console.WriteLine($"SMS Sent: {message.Content}");
    }
}

public class EmailNotifier : INotify
{
    public void Notify()
    {
        EmailMessage emailMessage = new()
        {
            Subject = "Order Confirmation",
            Content = "Your order has been processed successfully",
            Attachment = "Invoice.pdf"
        };

        Console.WriteLine($"Email Sent: " +
        $"Subject: {emailMessage.Subject}," +
        $"Body: {emailMessage.Content}," +
        $"Attachment: {emailMessage.Attachment}");
    }
}

public class OrderProcessor
{
    private readonly List<INotify> _notifies;
    public OrderProcessor(List<INotify> notifies)
    {
        _notifies = notifies;
    }

    public void ProcessOrder()
    {
        Console.WriteLine("Order is beng processed...");
        foreach (var notifier in _notifies)
        {
            notifier.Notify();
        }
    }
}

//public delegate void Notify(Message message);
public class Message
{
    public string Content { get; set; } = string.Empty;
}

public class PushMessage : Message
{
}

public class EmailMessage : Message
{
    public string Subject { get; set; } = string.Empty;
    public string Attachment { get; set; } = string.Empty;
}
```
👉2. **Event Handling**: Delegates are used to define event signatures, and events allow methods to subscribe to notifications when specific actions occur (e.g., button clicks, download start).  
```c#
public class Program
{
    static void Main(string[] args)
    {
        // Step 1: Create a Button object
        Button button = new Button();
        // Step 2: Subscribe methods ShowMessage and SaveData to the OnClick event
        button.OnClick += ShowMessage; // First method added to the delegate list
        button.OnClick += SaveData; // Second method added to the delegate list
        // Step 3: Simulate button click
        button.Click(); // This will invoke both ShowMessage and SaveData
    }

    public static void ShowMessage()
    {
        // Step 4: This will be executed first
        Console.WriteLine("Button clicked! Showing message...");
    }

    public static void SaveData()
    {
        // Step 5: This will be executed second
        Console.WriteLine("Button clicked! Saving data...");
    }
}
```
👉3. **Callback Methods**: Delegates allow methods to be passed as parameters to handle tasks (e.g., download completion) asynchronously.  
```c#  
public class Program
{
    static void Main()
    {
        // Step 1: Create an instance of FileDownloader
        FileDownloader downloader = new FileDownloader();
        // Step 2: Initiate the file download and pass the OnDownloadComplete as a callback
        downloader.DownloadFile("http://example.com/file", OnDownloadComplete);
        // The Main method finishes, but the download happens asynchronously
    }
    // Step 5: This method will be called once the file download completes (as a callback)

    public static void OnDownloadComplete(string message)
    {
        // Step 6: Print the message to indicate the download is complete
        Console.WriteLine(message);
        // Step 7: Log the download
        LogDownload(message);
    }

    public static void LogDownload(string message)
    {
        // Step 8: Print the log message
        Console.WriteLine($"Logging download: {message}");
    }
}

public class FileDownloader
{
    public void DownloadFile(string fileUrl, DownloadCompletedCallback callback)
    {
        // Step 3: Simulate asynchronous file download with Task.Run
        Task.Run(() =>
        {
            // Simulate a delay to mimic file download
            Thread.Sleep(2000); // Step 4: Wait for 2 seconds
            // Step 5: After the simulated download, invoke the callback
            callback($"File downloaded from {fileUrl} successfully.");
        });
    }
}

public delegate void DownloadCompletedCallback(string message);
```
## 🤔🤔🤔Q25: Can you explain the concept of middleware in ASP.NET Core?
 Middleware in ASP.NET Core is software that's assembled into an application pipeline to handle requests and responses. Each component in the middleware pipeline is responsible for invoking the next component in the sequence or short-circuiting the chain if necessary. Middleware components can perform a variety of tasks, such as authentication, routing, session management, and logging.
 
Middleware enables you to customize the request pipeline in ways that are most suited to your application's needs. They are executed in the order they are added to the pipeline, allowing for precise control over how requests are processed and how responses are constructed.
1. Each middleware component can:
- Process incoming requests.
- Modify the request or response.
- Call the next middleware in the pipeline, or short-circuit the pipeline to return a response immediately.

If a middleware component short-circuits, it prevents further middleware from executing (e.g., if authentication fails, you might return an error immediately).

**Real-World Example: Logging Incoming Requests**

Imagine you're building an **e-commerce application**. You want to log every incoming request to the system so that you can monitor traffic or debug issues.

You can create a custom middleware to handle this.

**Step-by-Step: Implementing a Custom Middleware**
```c#
//1.Creating the Middleware
public class RequestLoggingMiddleware
{
    private readonly RequestDelegate _next;

    public RequestLoggingMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        // Log the incoming request
        Console.WriteLine($"Incoming Request: {context.Request.Method} {context.Request.Path}");
        // Call the next middleware in the pipeline
        await _next(context);
        // Log after the request has been handled
        Console.WriteLine($"Response Status Code: {context.Response.StatusCode}");
    }
}

//2.Registering the Middleware in the Pipeline
app.UseMiddleware<RequestLoggingMiddleware>(); 
// Register custom logging middleware app.UseRouting(); 
// Sets up routing app.UseAuthentication(); 
// Handles authentication app.UseEndpoints(endpoints => { endpoints.MapControllers(); // Maps API controllers });
```
**Testing the Middleware**

Now, whenever you make an HTTP request (e.g., to /api/products), you’ll see the following output in your console:

Incoming Request: GET /api/products

Response Status Code: 200

**Middleware Order**

The order of middleware in the pipeline is critical. For example, authentication middleware must run before authorization middleware because a user must be authenticated before checking if they have the right permissions.
```c#
app.UseAuthentication(); // Authenticate the user
app.UseAuthorization(); // Check if the user is allowed to access the resource
```
**Short-Circut**: In ASP.NET Core, short-circuiting refers to a scenario where a middleware in the request pipeline stops the flow and prevents further middleware components from executing. This can occur deliberately (by design) or unintentionally.

**When Does Short-Circuiting Happen?**

Short-circuiting can happen when:
1. A middleware decides to handle the request completely without calling the next middleware in the pipeline (by not calling await _next(context);).
2. A condition is met that causes a middleware to return a response immediately, such as an authentication failure or a validation error.

**Example of Short-Circuiting**

Consider the following middleware pipeline:
```c#
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.UseMiddleware<RequestLoggingMiddleware>();
        app.UseAuthentication();
        app.UseAuthorization();
        app.UseRouting();
        app.UseEndpoints(endpoints => { endpoints.MapControllers(); });
    }
}
```
**Scenario 1: Short - circuiting with Authentication**

If a request comes in and the user is not authenticated, app.UseAuthentication() middleware might short-circuit the pipeline by returning an unauthorized (401) response, without passing the request further down the pipeline (e.g., app.UseAuthorization() or app.UseEndpoints() won’t execute).

Here's an example where authentication middleware might short-circuit:
```c#
public class AuthenticationMiddleware
{
    private readonly RequestDelegate _next;
    public AuthenticationMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        // Simulate an authentication check
        bool isAuthenticated = false;
        if (!isAuthenticated)
        {
            // Short-circuit the pipeline by returning a 401 Unauthorized response
            context.Response.StatusCode = 401;
            await context.Response.WriteAsync("Unauthorized");
            return; // No call to _next(context), so no further middleware is invoked
        }

        await _next(context); // Pass the request to the next middleware
    }
}
```
In this case:
- If the user is not authenticated, the pipeline is short-circuited. The middleware returns a 401 response and does not call _next(context), preventing further middleware (like routing, authorization, etc.) from running.
- If the user is authenticated, it proceeds to the next middleware by calling _next(context).

**Scenario 2: Short-circuiting with Custom Middleware**

Let’s take another example where a custom logging middleware short-circuits the pipeline for certain requests:
```c#
public class RequestLoggingMiddleware
{
    private readonly RequestDelegate _next;
    public RequestLoggingMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        // Check for a special condition (e.g., a certain request path)
        if (context.Request.Path == "/forbidden")
        {
            // Short-circuit the pipeline by returning a 403 Forbidden response
            context.Response.StatusCode = 403;
            await context.Response.WriteAsync("Forbidden path");
            return; // Prevent further middleware from executing
        }

        await _next(context); // Otherwise, pass the request to the next middleware
    }
}
```
## 🤔🤔🤔Q26: What is CORS(Cross - Origin Resource Sharing) ?
**CORS(Cross - Origin Resource Sharing)** is a security feature implemented by web browsers that controls how web applications interact with resources from different origins (domains). 

**CORS (Cross-Origin Resource Sharing)** is a security feature implemented by browsers to prevent unauthorized cross-origin requests. It is important for security purposes, ensuring that web applications can't freely access resources from different origins without permission.
```c#
// Define a CORS policy 
builder
    .Services
    .AddCors(options => 
    { 
        options.AddPolicy("AllowSpecificOrigins", builder => { 
                builder
                  .WithOrigins("http://example.com")
                  .AllowAnyHeader()
                  .AllowAnyMethod();
        });
    });

// Apply the CORS policy globally
app.UseCors("AllowSpecificOrigins");
```
## 🤔🤔🤔Q27. What are attributes in C# and how can they be used?
**Attributes** in C# are a powerful way to add declarative information to your code. They are used to add metadata, such as compiler instructions, annotations, or custom information, to program elements (classes, methods, properties, etc.). Attributes can influence the behavior of certain components at runtime or compile time, and they can be queried through reflection.

**Attributes** don’t change the functionality of your code directly but can be used to modify how the code behaves at runtime or during compile-time.

**Common Uses of Attributes**:

👉Marking methods as test methods in a unit testing framework (e.g., [TestMethod] in MSTest).

👉Specifying serialization rules (e.g., [Serializable], [DataMember]).

👉Controlling binding and model validation in ASP.NET Core (e.g., [Required], [Bind]).

👉Defining aspects of web service behaviors (e.g., [WebMethod]).

👉Custom attributes for domain-specific purposes.

**Example of Using an Attribute**:

A common use case is data validation in ASP.NET Core models. The [Required] attribute indicates that a property must have a value; if a model is passed to a controller method without this property set, model validation fails.
```c#
using System.ComponentModel.DataAnnotations;

public class Person
{
    [Required]
    public string Name { get; set; }

    [Range(1, 120)]
    public int Age { get; set; }
}
```
**Creating Custom Attributes**:

You can also define custom attributes for specific needs. Here's a simple example:
```c#

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = false)]
public class MyCustomAttribute : Attribute
{
    public string Description { get; set; }

    public MyCustomAttribute(string description)
    {
        Description = description;
    }
}

[MyCustomAttribute("This is a class description.")]
public class MyClass
{
    [MyCustomAttribute("This is a method description.")]
    public void MyMethod() { }
}
```
### 🤔🤔🤔Q28. **What is a race condition, and how can it occur in a multi-threaded .NET application?**
*   **Follow-up**: Imagine you are building an e-commerce application where multiple users can update the stock of products. How would a race condition manifest in this scenario, and how can you avoid it?  
      
    **Answer Guide**:
    
    *   A race condition occurs when multiple threads access shared data simultaneously, and the outcome depends on the timing of thread execution. In an e-commerce scenario, two users could try to update the stock of a product at the same time, potentially causing incorrect data to be stored.
        
    *   Solution: Use locking mechanisms like `lock`, `Mutex`, or `Semaphore` to ensure only one thread can access the shared resource at a time. Alternatively, use high-level concurrency classes like `ConcurrentDictionary` or atomic operations like `Interlocked`.
          
**Example 1: Race Condition Demonstration**

Let's consider an example where two users (threads) are trying to update the stock of a product simultaneously without synchronization.

#### Code Example Without Proper Synchronization:  
```c#
using System;
using System.Threading;
        
class Program
{
    static int stock = 100;
    static void Main()
    {
        Thread user1 = new Thread(new ThreadStart(UpdateStock));
        Thread user2 = new Thread(new ThreadStart(UpdateStock));

        user1.Start();
        user2.Start();

        user1.Join();
        user2.Join();

        Console.WriteLine($"Final stock: {stock}");
    }

    static void UpdateStock()
    {
        int stockBefore = stock;
        Console.WriteLine($"User {Thread.CurrentThread.ManagedThreadId} sees stock: {stockBefore}");

        // Simulate some delay
        Thread.Sleep(100);

        stockBefore -= 10; // User reduces stock by 10

        Console.WriteLine($"User {Thread.CurrentThread.ManagedThreadId} updates stock to: {stockBefore}");

        stock = stockBefore; // Update shared stock variable
    }
}
```
        
          
**Output (Example) — Race Condition:**

User 1 sees stock: 100
User 2 sees stock: 100

User 1 updates stock to: 90
User 2 updates stock to: 90

Final stock: 90  
          
**Explanation of Output:**

*   Both users saw the stock as 100 (before either thread updated the stock).
    
*   Both users tried to reduce it by 10. However, because they both accessed the stock without synchronization, they both ended up setting it to 90.
    
*   **Final stock is incorrect**: It should have been 80, but due to the race condition, it's 90 because both users overwrote the stock value based on the same initial state.  
    

### **Example 2: Fixing the Race Condition with Synchronization (**`lock` **keyword)**

To avoid race conditions, we can use the `lock` keyword to ensure only one thread can access the shared resource (in this case, the `stock` variable) at a time.

#### Code Example With Proper Synchronization:  
```c#
using System;
using System.Threading;
        
class Program
{
    static int stock = 100;
    static readonly object _lockObject = new object ();
    static void Main()
    {
        Thread user1 = new Thread(new ThreadStart(UpdateStock));
        Thread user2 = new Thread(new ThreadStart(UpdateStock));

        user1.Start();
        user2.Start();

        user1.Join();
        user2.Join();

        Console.WriteLine($"Final stock: {stock}");
    }

    static void UpdateStock()
    {
        lock (_lockObject)
        {
            int stockBefore = stock;
            Console.WriteLine($"User {Thread.CurrentThread.ManagedThreadId} sees stock: {stockBefore}");
            // Simulate some delay
            Thread.Sleep(100);
            stockBefore -= 10; // User reduces stock by 10
            Console.WriteLine($"User {Thread.CurrentThread.ManagedThreadId} updates stock to: {stockBefore}");
            stock = stockBefore; // Update shared stock variable
        }
    }
}
```       
**Explanation:**

1.  `lock (_lockObject)`: Ensures that only one thread at a time can enter the critical section (reading and updating the stock).
2.  The `_lockObject` ensures that other threads must wait until the first thread finishes its update before they can access the `stock`.  
      
**Output — With Synchronization:**

User 1 sees stock: 100
User 1 updates stock to: 90

User 2 sees stock: 90
User 2 updates stock to: 80

Final stock: 80
            
### 🤔🤔🤔**2. What is a deadlock, and how can it occur in a .NET application?**
**Follow-up**: Suppose you are developing a banking application where transfers between accounts can happen in parallel. How might a deadlock occur during a funds transfer between two accounts, and how can you prevent it?  
  
**Answer Guide**:

*   A deadlock occurs when two or more threads are blocked indefinitely, each waiting for a resource held by the other.
    
*   In the banking scenario, Thread A may lock Account 1 and wait for Account 2, while Thread B locks Account 2 and waits for Account 1, causing a deadlock.
    
*   Solution: Use a consistent locking order (e.g., always lock Account 1 before Account 2), or use `Monitor.TryEnter` with a timeout to avoid deadlocks.
    

**Example 1: Deadlock Code**  
```c#
using System;
using System.Threading;

class Program
{
    static object account1Lock = new object();
    static object account2Lock = new object();

    static void Main()
    {
        Thread threadA = new Thread(() => TransferMoney("Thread A", account1Lock, account2Lock));
        Thread threadB = new Thread(() => TransferMoney("Thread B", account2Lock, account1Lock));

        threadA.Start();
        threadB.Start();

        threadA.Join();
        threadB.Join();

        Console.WriteLine("Both threads have completed.");
    }

    static void TransferMoney(string threadName, object lockA, object lockB)
    {
        lock (lockA)
        {
            Console.WriteLine($"{threadName} locked first account.");
            // Simulate some work
            Thread.Sleep(100);
            Console.WriteLine($"{threadName} is waiting for the second account...");
            lock (lockB)
            {
                Console.WriteLine($"{threadName} locked second account. Transfer successful!");
            }
        }
    }
}
```
**Explanation:**

1.  **Two threads (**`Thread A` **and** `Thread B`**)**:
    
    *   **Thread A** tries to lock `account1Lock` first, then `account2Lock`.
        
    *   **Thread B** tries to lock `account2Lock` first, then `account1Lock`.
        
2.  **Deadlock scenario**:
    
    *   **Thread A** locks the first account (`account1Lock`), but then waits for the second account (`account2Lock`).
        
    *   **Thread B** locks the second account (`account2Lock`), but then waits for the first account (`account1Lock`).
        
3.  **Thread.Sleep(100)** simulates some delay to increase the likelihood of encountering the deadlock.
    
**Visualizing the Output**:  
Thread A locked first account.
Thread B locked first account.

Thread A is waiting for the second account...
Thread B is waiting for the second account...

**Explanation of Output**:

1.  **Thread A** locks the first account (Account 1) and waits for the second account (Account 2).
    
2.  **Thread B** locks the second account (Account 2) and waits for the first account (Account 1).
    
3.  Both threads are now waiting on each other indefinitely, and the program will hang because of the deadlock.
The program will not print the final line `"Both threads have completed."` because neither thread can proceed. They are both stuck waiting for the resource the other thread holds.

**Example 2: Solution Using Consistent Lock Order**

The deadlock can be avoided by enforcing a **consistent locking order**. Always lock **Account 1** before **Account 2**, regardless of the thread’s operation.

#### Code Example With Consistent Locking Order:  
```c#
using System;
using System.Threading;

class Program
{
    static object account1Lock = new object();
    static object account2Lock = new object();

    static void Main()
    {
        Thread threadA = new Thread(() => TransferMoney("Thread A"));
        Thread threadB = new Thread(() => TransferMoney("Thread B"));

        threadA.Start();
        threadB.Start();

        threadA.Join();
        threadB.Join();

        Console.WriteLine("Both threads have completed.");
    }

    static void TransferMoney(string threadName)
    {
        // Always lock in the same order: account1Lock first, account2Lock second
        lock (account1Lock)
        {
            Console.WriteLine($"{threadName} locked Account 1.");
            // Simulate some work
            Thread.Sleep(100);
            Console.WriteLine($"{threadName} is waiting for Account 2...");
            lock (account2Lock)
            {
                Console.WriteLine($"{threadName} locked Account 2. Transfer successful!");
            }
        }
    }
}
```

### **Explanation**:

1.  Both threads now **always lock Account 1 first** (`account1Lock`), and **then Account 2** (`account2Lock`). This ensures a consistent locking order, preventing deadlocks.
    
2.  **Thread.Sleep(100)** is used again to simulate some delay.
    
### **Visualizing the Output:**

Thread A locked Account 1.  
Thread A is waiting for Account 2...

Thread A locked Account 2. Transfer successful!

Thread B locked Account 1.

Thread B is waiting for Account 2...

Thread B locked Account 2. Transfer successful!

Both threads have completed.

**Explanation of Output**:

1.  **Thread A** locks Account 1 and then Account 2, successfully completing the transfer.
    
2.  **Thread B** waits for Thread A to release both locks and then performs its transfer.
    
3.  The program completes successfully without a deadlock.  
    
### **Example 3: Avoiding Deadlocks with** `Monitor.TryEnter` **and Timeout**

Another approach to avoid deadlocks is to use `Monitor.TryEnter` with a timeout. This allows a thread to attempt acquiring a lock and, if it can't do so within a specified time, it will exit gracefully instead of waiting indefinitely.

#### Code Example with `Monitor.TryEnter`:  
```c#
using System;
using System.Threading;

class Program
{
    static object account1Lock = new object();
    static object account2Lock = new object();

    static void Main()
    {
        Thread threadA = new Thread(() => SafeTransfer("Thread A"));
        Thread threadB = new Thread(() => SafeTransfer("Thread B"));

        threadA.Start();
        threadB.Start();
        
        threadA.Join();
        threadB.Join();

        Console.WriteLine("Both threads have completed.");
    }

    static void SafeTransfer(string threadName)
    {
        bool lockAccount1 = false;
        bool lockAccount2 = false;
        try
        {
            lockAccount1 = Monitor.TryEnter(account1Lock, TimeSpan.FromMilliseconds(500));
            if (lockAccount1)
            {
                Console.WriteLine($"{threadName} locked Account 1.");
                Thread.Sleep(100); // Simulate work
                lockAccount2 = Monitor.TryEnter(account2Lock, TimeSpan.FromMilliseconds(500));
                if (lockAccount2)
                {
                    Console.WriteLine($"{threadName} locked Account 2. Transfer successful!");
                }
                else
                {
                    Console.WriteLine($"{threadName} could not lock Account 2. Transfer failed!");
                }
            }
            else
            {
                Console.WriteLine($"{threadName} could not lock Account 1. Transfer failed!");
            }
        }
        finally
        {
            if (lockAccount2) Monitor.Exit(account2Lock);
            if (lockAccount1) Monitor.Exit(account1Lock);
        }
    }
}
```  
**Explanation**:

1.  `Monitor.TryEnter` tries to acquire the lock on **Account 1** and **Account 2**, but it will only wait for 500 milliseconds. If the lock cannot be acquired within that time, the thread gives up and avoids deadlock.
    
2.  **Locks are always released** in the `finally` block to ensure the program exits cleanly, even if the transfer fails.

**Visualizing the Output**:  
Thread A locked Account 1.

Thread A locked Account 2. Transfer successful!

Thread B locked Account 1.

Thread B could not lock Account 2. Transfer failed!

Both threads have completed.

**Explanation of Output**:

1.  **Thread A** completes the transfer successfully.
    
2.  **Thread B** locks Account 1 but fails to lock Account 2 within the 500ms timeout, so it exits without waiting indefinitely.
    
3.  Both threads finish their work, and the program exits cleanly without deadlock.  
    

### **Takeaways:**

1.  **Deadlock**: Occurs when two or more threads are waiting for each other to release resources they hold.
    
2.  **Consistent Lock Order**: Always acquire locks in the same order across all threads to avoid deadlocks.
    
3.  `Monitor.TryEnter` **with Timeout**: This technique allows threads to avoid waiting indefinitely for locks, breaking potential deadlocks by setting a timeout.
    
4.  **Graceful Exit**: Always ensure that locks are released properly to avoid resource contention or deadlocks.  
      
    

### 🤔🤔🤔**3. How would you avoid deadlocks when using** `async` **and** `await` **in .NET?**

*   **Follow-up**: In an order processing system, you have asynchronous calls to an external payment gateway followed by calls to update the order status in the database. How would you structure your code to avoid deadlocks?
    

**Answer Guide**:

*   Deadlocks in async code often happen when you mix `async` code with blocking calls (e.g., `Task.Wait()` or `.Result`), which can block the UI thread and cause a deadlock.
    
*   Solution: Always await asynchronous calls and avoid blocking the thread with `.Result` or `Wait()`. Ensure that all calls in the chain are asynchronous.
    

4. Can you explain the difference between `lock` and `Monitor` in .NET? When would you use one over the other?  
**Follow-up**: Imagine a financial application where transactions need to be locked before processing. Would you use `lock` or `Monitor`, and why?  
  
**Answer Guide**:

*   The `lock` keyword is a shorthand for `Monitor.Enter` and `Monitor.Exit`. It is simpler to use but less flexible. `Monitor` allows more fine-grained control, such as using `Monitor.TryEnter` to attempt to acquire a lock without blocking indefinitely.
    
*   For a financial application, you might prefer `Monitor.TryEnter` with a timeout to avoid deadlocks and provide better control over how long the system waits for a lock.
    

5. **🤔🤔🤔 Mutex (Mutual Exclusion)**

*   A **mutex** is a locking mechanism that ensures that only one thread can access a shared resource at a time. It is like having a key to a door: only one person (thread) can hold the key (mutex) and access the room (shared resource) at any given time. The key must be released (unlocked) for others to use it.  
      
    Imagine a single bathroom key that two people (threads) are trying to use. Only one person can enter the bathroom at a time (mutual exclusion). The second person waits until the first person finishes and hands over the key (releases the mutex).  
    

6. **🤔🤔🤔 Semaphore**

*   **Semaphore** is used to allow multiple threads to access a limited number of resources concurrently, making it suitable for scenarios where multiple instances of a resource can be shared up to a certain capacity.
    
*   A **semaphore** controls access to a resource by allowing multiple threads to access a limited number of instances of a resource simultaneously.  
      
    **Real-World Example of Semaphore: Parking Lot**
    
    Imagine a parking lot that has 3 parking spaces. Three cars can park at the same time. If all spaces are occupied, new cars have to wait until a spot becomes available.  
      
    **Real-World Analogy:**
    
    *   Think of a parking lot with only 3 spaces (semaphore capacity = 3). As soon as all spots are taken, new cars must wait for one of the cars to leave before parking. Each car that enters reduces the available spaces, and when a car leaves, a new spot opens up for another car.

## 🤔🤔🤔Q29. What is async/await and how does it work?
👉1. **async**: The method is marked as asynchronous, which tells the compiler to generate a state machine.
👉2. **await**: Awaits the result of an asynchronous method without blocking the calling thread.

In real-world applications, await is used for tasks that take time but don't need to block other parts of the system while they’re running. Here are a few examples showing how await allows asynchronous processing without blocking the calling thread:
    
- Here, `Task.Delay(5000)` simulates a 5-second file download. While the download is happening, the main thread (which handles UI) **doesn’t block**—the user can still click on buttons, scroll, or interact with the app.

- Without `await`, the UI would freeze until the file download is complete, which would result in a poor user experience.  
      
Here’s the complete explanation again with a real-world coffee shop example:

1.  **The** `await` **keyword is applied to a task**: Just like ordering coffee and then waiting for it, you’re asking the method to "wait" at a certain point (`await` the task) without blocking the entire system.
2.  **Indicating that the method should pause until the awaited task completes**: The method pauses at the `await`, similar to you waiting for your coffee, but the entire coffee shop (the system) doesn't pause—just that specific task.
3.  **Allowing other operations to run concurrently**: While the method is paused waiting for the task, other methods and tasks can continue running (other customers are getting served while you wait for your coffee).
4.  **Without blocking the main thread**: If this were an app, the user interface (UI) wouldn’t freeze. In the real world, the coffee shop doesn’t shut down while your coffee is being made; it continues working efficiently.  
```c#
public async Task ServeMultipleCustomersAsync()
{
    // Multiple customers ordering coffee concurrently
    var customer1 = ServeCoffeeAsync();
    var customer2 = ServeCoffeeAsync();
    // Await both coffees to be ready concurrently
    await Task.WhenAll(customer1, customer2);
    Console.WriteLine("Both customers served!");
}
```
In this example, **two customers** order coffee at the same time. The `Task.WhenAll` ensures both orders are processed concurrently. No one customer waits for the other to finish. Similarly, in a real-world coffee shop, multiple orders can be handled in parallel, without one customer blocking another’s order.
        
👉3. **State Machine**: A state machine is automatically created when you write asynchronous code using `async` and `await`. It controls how the method moves between different "states" of execution.  

In asynchronous programming, particularly when using `async` and `await` in C#, the compiler transforms the code into a **state machine**. A state machine is a programming construct that controls the flow of execution through a series of states. Each state represents a particular point in the execution of the method, and the state machine keeps track of where to continue when a task resumes after an `await`.

👉4. **Task**: In C#, a Task represents an asynchronous operation. It's part of the Task-based Asynchronous Pattern (TAP) and is used to handle asynchronous programming, allowing code to execute without blocking the main thread.

**Key Task Types**

- **Task**: Represents an asynchronous operation that does not return a value. It is analogous to void in synchronous methods.

- **Task<TResult>**: Represents an asynchronous operation that returns a result (of type TResult). It is analogous to TResult return types in synchronous methods.

**Why Use Task?**

1. **Avoid Blocking the UI**: In applications with a UI (e.g., WPF or Windows Forms), using Task ensures the UI thread remains responsive while long-running tasks (like file I/O or database queries) execute.

2. **Scalability**: In server applications, Task helps in creating more scalable systems by enabling asynchronous I/O operations (e.g., handling multiple HTTP requests simultaneously without blocking threads).

**Task States**

A Task can have different states, including:

👉**Created**: The task is created but not yet running.

👉**Running**: The task is currently running.

👉**Completed**: The task has finished running.

👉**Faulted**: The task encountered an error.

👉**Canceled**: The task was canceled.

**How do you handle exceptions in a method that returns a Task?**

In asynchronous programming with C#, when a method returns a **Task** or **Task<T>**, exceptions should be handled within the task to avoid unhandled exceptions that can crash the application. Exceptions thrown in a task are captured and placed on the returned task object. To handle these exceptions, you can use a try-catch block within the asynchronous method, or you can inspect the task after it has completed for any exceptions.

There are several approaches to handle exceptions in tasks:

👉1. **Inside the Asynchronous Method**: Use a try-catch block inside the async method to catch exceptions directly.
```c#
public async Task PerformOperationAsync()
{
    try
    {
        // Async operation that may throw an exception
    }
    catch (Exception ex)
    {
        // Handle exception
    }
}
```
👉2. **When Awaiting the Task**: Await the task inside a try-catch block to catch exceptions when the task is awaited.
```c#
try
{
    await PerformOperationAsync();
}
catch (Exception ex)
{
    // Handle exception
}
```
👉3. **Using Task.ContinueWith**: Use the ContinueWith method to attach a continuation task that can handle exceptions.
```c#
PerformOperationAsync().ContinueWith(task =>
{
    if (task.Exception != null)
    {
        // Handle exception
        var exception = task.Exception.InnerException;
    }
}, TaskContinuationOptions.OnlyOnFaulted);
```
👉4. **Using Task.WhenAny**: Useful for handling exceptions from multiple tasks.
```c#
var task = PerformOperationAsync();
await Task.WhenAny(task); // Wait for task to complete

if (task.IsFaulted)
{
    // Handle exception
    var exception = task.Exception.InnerException;
}
```
Here is an example demonstrating handling exceptions for a method that returns a Task:
```c#
public async Task<int> DivideAsync(int numerator, int denominator)
{
    return await Task.Run(() =>
    {
        if (denominator == 0)
            throw new DivideByZeroException("Denominator cannot be zero.");

        return numerator / denominator;
    });
}

public async Task ExecuteAsync()
{
    try
    {
        int result = await DivideAsync(10, 0);
        Console.WriteLine($"Result: {result}");
    }
    catch (DivideByZeroException ex)
    {
        Console.WriteLine($"Error: {ex.Message}");
    }
}
```
In this example, DivideAsync performs a division operation asynchronously and may throw a DivideByZeroException. The exception is handled in the ExecuteAsync method, demonstrating how to properly handle exceptions for tasks in asynchronous methods.

Handling exceptions in tasks is crucial for writing robust and error-resistant asynchronous C# applications, ensuring that your application can gracefully recover from errors encountered during asynchronous operations.

**🤔🤔🤔async/await and Task.Wait**:

In C#, both async/await and Task.Wait are used for handling asynchronous operations, but they work differently in terms of their behavior and how they should be used in modern asynchronous programming.

**1.🤔 async/await**

👉**Purpose**: async/await is designed to simplify writing asynchronous code. It makes asynchronous code look like synchronous code, making it easier to read and maintain.

👉**Usage**: await pauses the execution of the method until the awaited task completes. It allows the current thread to continue doing other work while waiting.

👉**Non-blocking**: When you use await, it doesn't block the thread; it frees up the thread to do other work and resumes execution when the awaited operation completes.

👉**Scenarios**: Ideal for I/O-bound operations (e.g., calling APIs, reading files, database operations).

**Example**:
```c#
public async Task FetchDataAsync()
{
    var data = await GetDataFromApiAsync();
    ProcessData(data);
}
```
In this example, await allows the method to be asynchronous without blocking the calling thread.

**2.🤔 Task.Wait**

👉**Purpose**: Task.Wait is used in scenarios where you want to block the calling thread until a task completes.

👉**Usage**: When you call Task.Wait(), it synchronously blocks the current thread until the task is completed.

👉**Blocking**: Unlike await, Task.Wait() blocks the thread, which can lead to thread starvation and reduce performance in asynchronous operations.

👉**Scenarios**: Usually used in console applications or specific scenarios where you need to block until a task is completed. However, it's not recommended for async code because it blocks the calling thread.

**Example**:
```c#
public void FetchData()
{
    Task task = GetDataFromApiAsync();
    task.Wait();  // Blocks the thread until the task completes
    ProcessData(task.Result);
}
```
In this example, Task.Wait() blocks the thread until the task completes, which can be inefficient in UI or web applications because it prevents other tasks from executing.

**Key Differences**:

**Blocking**:

👉async/await is non-blocking and allows the thread to perform other work while waiting.

👉Task.Wait() is blocking and waits for the task to finish before proceeding.

**Error Handling**:

👉With await, exceptions are captured and thrown as part of the task flow, which makes it easier to handle in an async method.

👉With Task.Wait(), exceptions are wrapped in an AggregateException, requiring more effort to unwrap and handle them.

**Best Practice**:

👉Use async/await for asynchronous code because it is non-blocking and helps maintain responsiveness in your applications (especially in UI and web apps).

👉Avoid Task.Wait() in asynchronous code, especially in GUI or web applications, as it blocks the thread and can degrade performance.

**3.🤔 Task.WhenAll**

Processing Multiple Data Fetch Requests Simultaneously
```c#
public async Task<DashboardData> GetDashboardDataAsync()
{
    // Start each data fetch but don't await right away
    var ordersTask = FetchOrdersAsync();
    var inventoryTask = FetchInventoryAsync();
    var customersTask = FetchCustomersAsync();

    // While waiting for the above tasks, the thread can do other work or handle requests

    // Await all tasks to complete before returning the result
    await Task.WhenAll(ordersTask, inventoryTask, customersTask);
    // Combine all results once they complete

    return new DashboardData
    {
        Orders = await ordersTask,
        Inventory = await inventoryTask,
        Customers = await customersTask
    };
}
```
It’s understandable why this might look like a synchronous call, but Each of these lines looks like they’re being executed sequentially, but in reality, they’re **starting each task asynchronously** without blocking the execution flow.
```c#
var ordersTask = FetchOrdersAsync();
```
- **What’s Happening**: This line calls the asynchronous method FetchOrdersAsync() and immediately assigns the resulting Task to ordersTask.

- **Behind the Scenes**: The FetchOrdersAsync method might, for instance, start an HTTP request or database call and return a Task representing this ongoing work. However, because there’s no await here, the method doesn’t wait for the request to complete. Instead, it immediately moves on to the next line.

- **Real-World Parallel**: Imagine asking a colleague to go fetch order data for you, and without waiting for them to return, you turn to another colleague and ask them for inventory data

**Why It’s Asynchronous, Not Synchronous**

- **No await Means No Waiting**: By not using await immediately, you’re not pausing or blocking execution to wait for each task to complete. Each call to FetchOrdersAsync, FetchInventoryAsync, and FetchCustomersAsync returns a Task that represents the ongoing work, but the method moves on without waiting.

- **All Tasks Run in Parallel**: The Task objects (ordersTask, inventoryTask, and customersTask) represent operations that are all happening in parallel. They’re “promises” of future results but are not awaited yet, so the application doesn’t stop to wait for any of them at this stage.

**When Does the Method Actually Wait?**

The waiting occurs later with:
```c#
await Task.WhenAll(ordersTask, inventoryTask, customersTask);
```
**Explanation**: This line uses await with Task.WhenAll, which waits for all three tasks to complete but only pauses here. While waiting, the server thread is freed up to handle other requests.

This line awaits all tasks to complete, but only at this point does the method pause. Up until this line, each task started running asynchronously and concurrently, allowing other work to be done or other requests to be handled while they’re in progress.

**Summary**

By not using await immediately, we’ve started all three tasks **without waiting**. The tasks run asynchronously and concurrently, which makes it possible to fetch all three sets of data simultaneously rather than sequentially. Only when we reach await Task.WhenAll(...) does the method wait for all tasks to complete, making the process highly efficient.

### 🤔🤔🤔What if you add await before each of these method calls, like this:
```c#
var orders = await FetchOrdersAsync();
var inventory = await FetchInventoryAsync();
var customers = await FetchCustomersAsync();
```
and remove **Task.WhenAll()**, it changes the behavior to a **sequential, synchronous-like execution**. Here’s what happens line by line:

1. **First**
	```c#
	await FetchOrdersAsync();
	```
	Completes Before Moving On:
	
	- When you await the result of **FetchOrdersAsync()**, the **method pauses** and **waits for this task to complete** before moving to the next line.
	
	- **Effect**: The program now **waits for FetchOrdersAsync() to finish**, **blocking any further execution** in this method until the order data is fully fetched.

2. **Next** await FetchInventoryAsync() **Waits for Inventory Fetch**:

	- Once **FetchOrdersAsync() completes**, FetchInventoryAsync() is called, and await pauses the method again until inventory data is fetched.
	
	- **Effect**: Now the method is waiting on FetchInventoryAsync(), so nothing else proceeds until inventory data is ready.

3. **Finally**, await FetchCustomersAsync() **Blocks Until Complete**:

	- Once the inventory data is fetched, FetchCustomersAsync() is called. Again, the await keyword blocks the method until this task completes.
	
	- **Effect**: The method waits for customer data, meaning only after this third fetch completes does the method proceed to return data.

**Comparison with Task.WhenAll**

When using await Task.WhenAll(ordersTask, inventoryTask, customersTask);, all three tasks start immediately and run concurrently, making it much faster than sequential awaits.

**Real-World Difference**

- **Sequential Execution with Await per Line**: Each data fetch happens one after another. If each task takes, say, 1 second, the total time to get all data would be approximately 3 seconds (1 second for orders, 1 for inventory, and 1 for customers).

- **Parallel Execution with Task.WhenAll**: All tasks start at the same time and complete independently. Here, if each fetch still takes 1 second, the total time to get all data is approximately 1 second (since they’re all running concurrently).

**Summary**

Using await on each task line by line results in sequential execution (like traditional synchronous programming), making it less efficient for multiple independent asynchronous tasks. Using Task.WhenAll allows them to run concurrently, providing much faster overall performance.

### 🤔🤔🤔Does Task.Run() creates new thread everytime?

No, **Task.Run()** does not necessarily create a new thread every time it is called. Instead, it utilizes the ThreadPool to execute the task, which means it can reuse existing threads from the pool. Here's how it works:

**Understanding Task.Run()**

1. **Thread Pool Usage**:

- **Task.Run()** schedules the provided work (a delegate) to run on a thread pool thread. The .NET ThreadPool manages a pool of worker threads, which can be reused for different tasks.
- If there are idle threads available in the pool, **Task.Run()** will use one of those threads to execute the task, rather than creating a new one.

2. **Thread Creation Logic**:

- If there are no available threads in the pool (because all threads are busy with other tasks), the ThreadPool can create additional threads, but it will do so only up to a certain limit defined by the system configuration. This is done to optimize resource usage and minimize the overhead of thread creation.

3. **Efficiency**:

- Using **Task.Run()** is generally more efficient than **creating a new thread manually with new Thread() for short-lived operations**. The **overhead of creating and destroying threads** is reduced because threads can be reused.

- This makes **Task.Run()** a good choice for offloading work to the background while keeping the main thread responsive.

**Example of Task.Run()**

Here’s a simple example of using Task.Run():
```c#
using System;
using System.Threading.Tasks;

class Program
{
    static void Main()
    {
        Task task = Task.Run(() => DoWork());
        Console.WriteLine("Task started.");
        
        task.Wait(); // Wait for the task to complete
        Console.WriteLine("Task completed.");
    }

    static void DoWork()
    {
        Console.WriteLine("Working on thread: " + System.Threading.Thread.CurrentThread.ManagedThreadId);
        System.Threading.Thread.Sleep(2000); // Simulating work
        Console.WriteLine("Work done.");
    }
}
```
**Key Points**

👉**Reuse of Threads**: Task.Run() is designed to reuse existing threads in the ThreadPool when possible, which improves performance.

👉**Thread Management**: The ThreadPool dynamically manages threads based on workload and system capacity, so you don't have to worry about the underlying thread management details.

👉**Thread Limits**: There are limits to how many threads the ThreadPool can create and manage simultaneously. If your application consistently requires more threads than the pool can provide, you might need to adjust the ThreadPool settings or review the design of your concurrent tasks.

**Summary**

In summary, while Task.Run() may create new threads when necessary, it primarily relies on reusing existing threads from the ThreadPool, making it an efficient way to run tasks asynchronously without the overhead of manual thread management.

### 🤔🤔🤔What is the diffrence between ThreadPool.QueueWorkItem and new Thread().Start()?
The ThreadPool.QueueUserWorkItem and new Thread().Start() methods in .NET are both used to create and manage threads, but they have different purposes, behaviors, and use cases.

👉**ThreadPool.QueueUserWorkItem**: Adds work to a shared ThreadPool of pre-existing threads. Best for short-lived, frequent tasks that don’t need a dedicated thread (e.g., handling requests in a web server).

ThreadPool.QueueUserWorkItem assigns the task to a thread from the shared pool. When the task finishes, the thread returns to the pool, ready for the next task
```c#
using ThreadPool.QueueUserWorkItem;
using System;
using System.Threading;

class Program
{
    static void Main()
    {
        ThreadPool.QueueUserWorkItem(DoWork);
        Console.WriteLine("Task queued on thread pool.");
        Console.ReadLine();
    }

    static void DoWork(object state)
    {
        Console.WriteLine("Working on thread: " + Thread.CurrentThread.ManagedThreadId);
        Thread.Sleep(2000); // Simulating work
        Console.WriteLine("Work completed.");
    }
}
```
👉**new Thread().Start()**: Creates a dedicated thread for each task. Useful for long-running tasks where more control over the thread’s lifecycle is needed (e.g., background data analysis).
```c#
using new Thread().Start()
using System;
using System.Threading;

class Program
{
    static void Main()
    {
        Thread workerThread = new Thread(DoWork);
        workerThread.Start();
        Console.WriteLine("Started new thread.");
        Console.ReadLine();
    }

    static void DoWork()
    {
        Console.WriteLine("Working on thread: " + Thread.CurrentThread.ManagedThreadId);
        Thread.Sleep(2000); // Simulating work
        Console.WriteLine("Work completed.");
    }
}
```
**👉Real-World Example**:

- **ThreadPool.QueueUserWorkItem**: A web server processing incoming HTTP requests quickly without the overhead of creating/destroying threads.

- **new Thread().Start()**: A long-running task that performs real-time data processing, where you may want to control priority or manage its shutdown.

**👉Thread Management**

- **ThreadPool.QueueUserWorkItem**: Managed by the .NET runtime. Threads are reused for efficiency, so creating/destroying threads isn't required.

- **new Thread().Start()**: Manual control over threads, which are created and destroyed as needed. Higher overhead and less efficient for frequent, short tasks.

**👉Example**:

```c#
// ThreadPool - efficient for short tasks
ThreadPool.QueueUserWorkItem(_ => Console.WriteLine("Processing HTTP request"));
```
```c#
// new Thread - better for a dedicated, ongoing task
new Thread(() => Console.WriteLine("Long background task running")).Start();
```
**👉Performance & Overhead**

- **ThreadPool.QueueUserWorkItem**: Lower overhead, as thread creation/destruction is avoided. Ideal for scalable applications.

- **new Thread().Start()**: Higher overhead as each thread is individually created and cleaned up. Use only when task requires dedicated, long-term attention.

**👉Example:**

- **ThreadPool**: A bank’s server handling many user transactions quickly.

- **new Thread**: An analysis tool processing large datasets for hours.
### 🤔🤔🤔What is the purpose of ConfigureAwait(false) method in Task class?
The ConfigureAwait(false) method is used in asynchronous programming in .NET to control how the continuation of an asynchronous task is executed. Here's a detailed explanation of its purpose and benefits:

**Purpose of ConfigureAwait(false)**

1. **Control Context**:

	- By default, when you await a task, the continuation (the code that follows the await) resumes on the original synchronization context. This is particularly important in UI applications (like WPF or WinForms) where you want to update the UI from the main thread.
	Using ConfigureAwait(false) tells the task to continue on any available thread rather than trying to marshal back to the original synchronization context.

2. **Performance Improvement**:

	- When you use ConfigureAwait(false), it can reduce the overhead of context switching, especially in library code or in scenarios where the original context (like a UI thread) is not necessary.
	- This can improve performance by allowing the task to complete without waiting for the original context to be free, reducing latency in your application.

3. **Avoid Deadlocks**:

	- In some scenarios, particularly with deadlocks caused by synchronously waiting on asynchronous code (using .Result or .Wait()), ConfigureAwait(false) can help prevent these issues by not attempting to marshal back to the original context.

**🤔When to Use ConfigureAwait(false)**

- Library Code: When you are writing a library or reusable code, it’s often a good practice to use ConfigureAwait(false) because the calling context might not be relevant, and it prevents the library from unintentionally causing deadlocks or UI thread blocking.

- Non-UI Applications: In console applications or background services, where the original context isn’t needed, using ConfigureAwait(false) is typically advisable.

**Example**

Here’s an example illustrating the use of ConfigureAwait(false):
```c#
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        string result = await FetchDataAsync().ConfigureAwait(false);
        Console.WriteLine(result);
    }

    static async Task<string> FetchDataAsync()
    {
        using (HttpClient client = new HttpClient())
        {
            // Simulate an asynchronous operation
            string data = await client.GetStringAsync("https://jsonplaceholder.typicode.com/posts/1").ConfigureAwait(false);
            return data;
        }
    }
}
```
**👉Summary**

 - Use ConfigureAwait(false) when you don't need to return to the original synchronization context, especially in library code or non-UI applications.
	
 - It can improve performance, reduce deadlocks, and provide more flexible control over task continuations.

By applying ConfigureAwait(false) thoughtfully, you can write more efficient and robust asynchronous code.
### 🤔🤔🤔What is the difference between Task.WaitAll() and Task.WhenAll()?
Task.WaitAll() and Task.WhenAll() are both methods used to handle multiple tasks in .NET, but they have different behaviors and use cases.

**1. Method Type**

- Task.WaitAll()

	- Type: Synchronous
	- Behavior: Blocks the calling thread until all the specified tasks have completed execution. If any of the tasks fail, it throws an AggregateException.
	- Usage: Typically used when you need to ensure that all tasks have completed before continuing execution in the current thread.

- Task.WhenAll()

	- Type: Asynchronous
	- Behavior: Returns a Task that represents the completion of all the specified tasks without blocking the calling thread. It allows the calling code to continue executing while the tasks are running.
	- Usage: Useful when you want to await the completion of multiple tasks without blocking the thread. It can be awaited using the await keyword.

**2. Return Value**

- Task.WaitAll()

	- **Return Value**: Does not return a value. It’s a void method that only indicates when all tasks have completed.

- Task.WhenAll()

	- **Return Value**: Returns a Task, which can be awaited. If the tasks return values (e.g., Task<int>), WhenAll() returns a Task<T[]> containing the results of the completed tasks.

**3. Exception Handling**

- Task.WaitAll()

	- **Exception Handling**: If one or more tasks throw exceptions, WaitAll will throw an AggregateException containing all the exceptions thrown by the individual tasks. The calling thread will be blocked until all tasks are completed.

- Task.WhenAll()

	- **Exception Handling**: If any of the tasks fail, the resulting task will also fail and throw an AggregateException when awaited. However, the calling thread is not blocked, allowing other operations to continue until the awaited task completes.

**4. Example Usage**

Here’s a comparison with example code for both methods:

**Using Task.WaitAll()**
```c#
using System;
using System.Threading.Tasks;

class Program
{
    static void Main()
    {
        Task task1 = Task.Run(() => SimulateWork(1));
        Task task2 = Task.Run(() => SimulateWork(2));
        
        try
        {
            Task.WaitAll(task1, task2); // Blocks the main thread until both tasks complete
            Console.WriteLine("All tasks completed.");
        }
        catch (AggregateException ae)
        {
            foreach (var ex in ae.InnerExceptions)
            {
                Console.WriteLine($"Task failed with exception: {ex.Message}");
            }
        }
    }

    static void SimulateWork(int id)
    {
        if (id == 2) throw new Exception("Error in task 2");
        Console.WriteLine($"Task {id} completed.");
    }
}
```
Using Task.WhenAll()
```c#
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        Task task1 = Task.Run(() => SimulateWork(1));
        Task task2 = Task.Run(() => SimulateWork(2));
        
        try
        {
            await Task.WhenAll(task1, task2); // Does not block; continues executing
            Console.WriteLine("All tasks completed.");
        }
        catch (AggregateException ae)
        {
            foreach (var ex in ae.InnerExceptions)
            {
                Console.WriteLine($"Task failed with exception: {ex.Message}");
            }
        }
    }

    static void SimulateWork(int id)
    {
        if (id == 2) throw new Exception("Error in task 2");
        Console.WriteLine($"Task {id} completed.");
    }
}
```
**👉Summary**

- Use Task.WaitAll() when you want to block the current thread until all tasks are complete and you do not require asynchronous behavior.
- Use Task.WhenAll() when you want to run tasks asynchronously and await their completion without blocking the current thread.
## 🤔🤔🤔Q27. Describe the Entity Framework and its advantages?
👉**1. Explain the difference between `DbContext` and `DbSet`.**

**DbContext**: This is a class that manages the database connection and is used to configure and interact with the database using EF Core. It is responsible for querying and saving data.

**DbSet**: This is a class that represents a collection of entities of a specific type that can be queried from the database. Each entity type that you want to include in your model must have a `DbSet` property in your `DbContext` class.
        
👉**2. What is the purpose of the `OnModelCreating` method in EF Core?**

The **OnModelCreating**` method is used to configure the model using the ModelBuilder API. It is where you can define schema details, relationships, table mappings, and more. This method is overridden in your `DbContext` class to configure entity properties and 
relationships.
    
👉**3. How can you configure a many-to-many relationship in EF Core?**
    
In EF Core, a many-to-many relationship can be configured using a join entity that contains the foreign keys to the two related entities. Here's an example:  
```c#
using Microsoft.EntityFrameworkCore;

public class Student
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;
    public ICollection<StudentCourse> Courses { get; set; }
}

public class Course
{
    public int Id { get; set; }
    public string Title { get; set; } = string.Empty;
    public ICollection<StudentCourse> Students { get; set; }
}

public class StudentCourse
{
    public int StudentId { get; set; }
    public Student Student { get; set; }
    public int CourseId { get; set; }
    public Course Course { get; set; }
}

public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public UserProfile UserProfile { get; set; }
}

public class UserProfile
{
    public int Id { get; set; }
    public string Bio { get; set; }
    public int UserId { get; set; }
    public User User { get; set; }
}

public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;
    public ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; } = string.Empty;
    public string Content { get; set; } = string.Empty;
    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
    {

    }

    public DbSet<Student> Students { get; set; }
    public DbSet<Course> Courses { get; set; }
    public DbSet<User> Users { get; set; }
    public DbSet<UserProfile> UserProfiles { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
	//many to many starts
	modelBuilder.Entity<StudentCourse>()
	.HasKey(sc => new { sc.StudentId, sc.CourseId });

	modelBuilder.Entity<StudentCourse>()
	.HasOne(sc => sc.Student)
	.WithMany(sc => sc.Courses)
	.HasForeignKey(sc => sc.StudentId);

	modelBuilder.Entity<StudentCourse>()
	.HasOne(sc => sc.Course)
	.WithMany(sc => sc.Students)
	.HasForeignKey(sc => sc.CourseId);

	//many to many ends

	//one to one stars
	modelBuilder.Entity<User>()
	.HasOne(u => u.UserProfile)
	.WithOne(up => up.User)
	.HasForeignKey<UserProfile>(up => up.UserId);

	//one to one ends

	//one to many starts
	modelBuilder.Entity<Blog>()
	.HasMany(b => b.Posts)
	.WithOne(p => p.Blog)
	.HasForeignKey(p => p.BlogId);
	//one to many ends
    }
}
```
👉**4. What is the difference between `AsNoTracking` and `AsTracking` in EF Core?**
    
- **AsNoTracking**: This method returns entities without tracking them in the `DbContext`. This is useful for read-only scenarios where the performance can be improved by not tracking the entities.

- **AsTracking**: This is the default behavior. It tracks the entities returned by the query, meaning any changes to these entities are monitored by the `DbContext` and can be persisted to the database during `SaveChanges`.
        
👉**5. What are global query filters in EF Core?**
    
Global query filters are filters applied to all queries executed against a particular entity type. These filters are defined in the `OnModelCreating` method and are useful for scenarios like soft deletes or multi-tenancy. 
Example:
```c#
modelBuilder
    .Entity<Product>()
    .HasQueryFilter(p => !p.IsDeleted);
```
👉**6. How do you handle concurrency conflicts in EF Core?**
    
Concurrency conflicts occur when multiple processes try to update the same data at the same time. EF Core provides optimistic concurrency control. You can configure a property to be a concurrency token:  
```c#
public class Product
{
public int Id { get; set; }
public string Name { get; set; }

[ConcurrencyCheck]
public int Quantity { get; set; }
}
```
//When a conflict is detected during `SaveChanges`, an exception (`DbUpdateConcurrencyException`) is thrown, which can be handled to resolve the conflict:  
```c#
try
{
    context.SaveChanges();
}
catch (DbUpdateConcurrencyException ex)
{
    // Handle the concurrency conflict
}
```
👉**7. What is the difference between `Include` and `ThenInclude` in EF Core?**

`Include`**:** This method is used to include related entities in the query results. It is used for eager loading.
```c#
var orders = context.Orders.Include(o => o.Customer).ToList();
```
`ThenInclude`**:** This method is used to include related entities after the first level of inclusion. It is used for further levels of eager loading.  
```c#
var orders = context.Orders
.Include(o => o.Customer)
.ThenInclude(c => c.Address)
.ToList();
```
👉**7.1. Describe a scenario where you need to use `Include` and `ThenInclude` for eager loading with nested relationships.**
    
Consider a scenario with entities `Order`, `Customer`, and `Address`, where an order has a customer and a customer has an address. To eagerly load the orders along with their customers and the customers' addresses:  
```c#
var orders = context.Orders
.Include(o => o.Customer)
.ThenInclude(c => c.Address)
.ToList();
```
👉**8. How do you handle transactions in EF Core?**
    
EF Core supports transactions implicitly when `SaveChanges` is called. For more complex scenarios requiring explicit transaction control, you can use `DbContext.Database.BeginTransaction`, `Commit`, and `Rollback`:
```c#
using (var transaction = context.Database.BeginTransaction())
{
    try
    {
	// Perform multiple operations
	context.SaveChanges();
	// Another set of operations
	context.SaveChanges();
	transaction.Commit();
    }
    catch (Exception)
    {
	transaction.Rollback();
	throw;
    }
}
``` 
👉**9. How can you optimize performance with EF Core?**

- **Use** `AsNoTracking` **for read-only queries:** This avoids the overhead of change tracking.

- **Batch commands:** Reduce the number of database round-trips by batching multiple commands.

- **Use compiled queries:** For queries that are executed frequently, use compiled queries to avoid query recompilation

- **Avoid lazy loading:** Prefer eager or explicit loading to avoid the "N+1 query problem."

- **Use projections:** Retrieve only the required fields using `Select`  
        
👉**10. How do you handle complex querying scenarios using raw SQL in EF Core?**  

For complex queries that cannot be expressed using LINQ, use raw SQL queries:  
```c#
var customers = context.Customers.FromSqlRaw("SELECT \* FROM Customers WHERE IsActive = 1").ToList();
```
You can also use raw SQL for non-query commands:  
```c#
context.Database.ExecuteSqlRaw("UPDATE Customers SET IsActive = 0 WHERE Id = {0}", customerId);
```
## 🤔🤔🤔Q28. What is .NET?
.NET is a comprehensive development platform used for building a wide variety of applications, including web, mobile, desktop, and gaming. It supports multiple programming languages, such as C#, F#, and Visual Basic. .NET provides a large class library called Framework Class Library (FCL) and runs on a Common Language Runtime (CLR) which offers services like memory management, security, and exception handling.
## 🤔🤔🤔Q29. What is the purpose of the .NET Standard?
1.  The purpose of **.NET Standard** is to create a **common set of APIs** that can be used across different .NET platforms (like .NET Core, .NET Framework, Xamarin, and others). It ensures **code compatibility** between these platforms, so you can write libraries that work everywhere without needing to rewrite code for each platform.  
    - In essence, it helps **unify** the .NET ecosystem and makes code **more portable**.
## 🤔🤔🤔Q30. Explain the differences between .NET Core, .NET Framework, and Xamarin.
.NET Core, .NET Framework, and Xamarin are all part of the .NET ecosystem, but they serve different purposes and are used in different types of projects. Understanding the differences between them can help you choose the right technology for your specific needs.

**.NET Framework**:

The original .NET implementation, launched in 2002.
Primarily runs on Windows and is used for developing Windows desktop applications and ASP.NET web applications.
Provides a vast library of pre-built functionality, including Windows Forms, WPF (Windows Presentation Foundation) for desktop applications, and ASP.NET for web applications.
Moving forward, it will receive only critical security updates and bug fixes.

**.NET Core**:

A cross-platform, open-source reimplementation of .NET that runs on Windows, macOS, and Linux.
Designed to be modular and lightweight, making it suitable for web applications, microservices, and cloud applications.
Supports development of console applications, ASP.NET Core web applications, and libraries.
With the release of .NET 5 and beyond, .NET Core has evolved into simply ".NET," unifying the .NET platform and serving as the future of the .NET ecosystem.

**Xamarin**:

A .NET platform for building mobile applications that can run on iOS, Android, and Windows devices.
Allows developers to write mobile applications using C# and .NET libraries while providing native performance and look-and-feel on each platform.
Integrates with Visual Studio, providing a rich development environment.
Xamarin.Forms allows for the creation of UIs from a single, shared codebase, while Xamarin.iOS and Xamarin.Android provide access to platform-specific APIs.
## 🤔🤔🤔Q31. Can you explain the Common Language Runtime (CLR)?
The CLR is a virtual machine component of the .NET framework that manages the execution of .NET programs. It provides important services such as memory management, type safety, exception handling, garbage collection, and thread management. The CLR converts Intermediate Language (IL) code into native machine code through a process called Just-In-Time (JIT) compilation. This ensures that .NET applications can run on any device or platform that supports the .NET framework.
## 🤔🤔🤔Q32. What is the difference between managed and unmanaged code?
Managed code is executed by the CLR, which provides services like garbage collection, exception handling, and type checking. It's called "managed" because the CLR manages a lot of the functionalities that developers would otherwise need to implement themselves. Unmanaged code, on the other hand, is executed directly by the operating system, and all memory allocation, type safety, and security must be handled by the programmer. Examples of unmanaged code include applications written in C or C++.
## 🤔🤔🤔Q33. Explain the basic structure of a C# program.
A basic C# program consists of the following elements:

**Namespace declaration**: A way to organize code and control the scope of classes and methods in larger projects.

**Class declaration**: Defines a new type with data members (fields, properties) and function members (methods, constructors).

**Main method**: The entry point for the program where execution begins and ends.

**Statements and expressions**: Perform actions with variables, calling methods, looping through collections, etc.
```c#
using System;

namespace HelloWorld
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```
## 🤔🤔🤔Q34. What is garbage collection in .NET?
**Garbage collection (GC)** in .NET is an automatic memory management feature that frees up memory used by objects that are no longer accessible in the program. It eliminates the need for developers to manually release memory, thereby reducing memory leaks and other memory-related errors. The GC operates on a separate thread and works in three phases: marking, relocating, and compacting. During the marking phase, it identifies which objects in the heap are still in use. During the relocating phase, it updates the references to objects that will be compacted. Finally, during the compacting phase, it reclaims the space occupied by the garbage objects and compacts the remaining objects to make memory allocation more efficient.

1. **Finalize** is a method which is automatically called by the garbage collector to dispose the no longer needed objects

2. **FINALLY** The finally block is used to execute a given set of statements, whether an exception occur or not. Finally block is mostly used to dispose the unwanted objects when they are no more required. This is good for performance, otherwise you have to wait for garbage collector to dispose them.
## 🤔🤔🤔Q35. Explain the concept of exception handling in C#?
Exception handling in C# is a mechanism to handle runtime errors, allowing a program to continue running or fail gracefully instead of crashing. It is done using the try, catch, and finally blocks. The try block contains code that might throw an exception, while catch blocks are used to handle the exception. The finally block contains code that is executed whether an exception is thrown or not, often for cleanup purposes.
```c#
try {
    // Code that may cause an exception
    int divide = 10 / 0;
}
catch (DivideByZeroException ex) {
    // Code to handle the exception
    Console.WriteLine("Cannot divide by zero. Please try again.");
}
finally {
    // Code that executes after try/catch, regardless of an exception
    Console.WriteLine("Operation completed.");
}
```
## 🤔🤔🤔Q36. Can you describe what a namespace is and how it is used in C#?
A namespace in C# is used to organize code into a hierarchical structure. It allows the grouping of logically related classes, structs, interfaces, enums, and delegates. Namespaces help avoid naming conflicts by qualifying the uniqueness of each type. For example, the System namespace in .NET includes classes for basic system operations, such as console input/output, file reading/writing, and data manipulation.
```c#
using System;

namespace MyApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello, World!");
        }
    }
}
```
## 🤔🤔🤔Q37. Describe what LINQ is and give an example of where it might be used.
**LINQ (Language Integrated Query)** is a powerful feature in C# that allows developers to write expressive, readable code to query and manipulate data. It provides a uniform way to query various data sources, such as collections in memory, databases (via LINQ to SQL, LINQ to Entities), XML documents (LINQ to XML), and more. LINQ queries offer three main benefits: they are strongly typed, offer compile-time checking, and support IntelliSense, which enhances developer productivity and code maintainability.

LINQ can be used in a variety of scenarios, including filtering, sorting, and grouping data. It supports both method syntax and query syntax, providing flexibility in how queries are expressed.

Here is a simple example demonstrating LINQ with a list of integers:
```c#
using System;
using System.Linq;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

        // Use LINQ to find all even numbers
        var evenNumbers = from num in numbers
                          where num % 2 == 0
                          select num;

        Console.WriteLine("Even numbers:");
        foreach (var num in evenNumbers)
        {
            Console.WriteLine(num);
        }
    }
}
```
## 🤔🤔🤔Q38. How do you manage memory in .NET applications?
**Memory management** in .NET applications is primarily handled automatically by the Garbage Collector (GC), which provides a high-level abstraction for memory allocation and deallocation, ensuring that developers do not need to manually free memory. However, understanding and cooperating with the GC can help improve your application's performance and memory usage. Here are key aspects of memory management in .NET:

- **Garbage Collection**: Automatically reclaims memory occupied by unreachable objects, freeing developers from manually deallocating memory and helping to avoid memory leaks.

- **Dispose Pattern**: Implementing the IDisposable interface and the Dispose method allows for the cleanup of unmanaged resources (such as file handles, database connections, etc.) that the GC cannot reclaim automatically.

- **Finalizers**: Can be defined in classes to perform cleanup operations before the object is collected by the GC. However, overuse of finalizers can negatively impact performance, as it makes objects live longer than necessary.

- **Using Statements**: A syntactic sugar for calling Dispose on IDisposable objects, ensuring that resources are freed as soon as they are no longer needed, even if exceptions are thrown.

- **Large Object Heap (LOH) Management**: Large objects are allocated on a separate heap, and knowing how to manage large object allocations can help reduce memory fragmentation and improve performance.

Here is an example demonstrating the use of the IDisposable interface and using statement for resource management:
## 🤔🤔🤔Q39. Explain the concept of threading in .NET.
**Threading** in .NET allows for the execution of multiple operations simultaneously within the same process. It enables applications to perform background tasks, UI responsiveness, and parallel computations, improving overall application performance and efficiency. The .NET framework provides several ways to create and manage threads:

**System.Threading.Thread**: A low-level approach to create and manage threads directly. This class offers fine-grained control over thread behavior.

**ThreadPool**: A collection of worker threads maintained by the .NET runtime, offering efficient execution of short-lived background tasks without the overhead of creating individual threads for each task.

**Task Parallel Library (TPL)**: Provides a higher-level abstraction over threading, using tasks that represent asynchronous operations. TPL uses the ThreadPool internally and supports features like task chaining, cancellation, and continuation.

**async and await**: Keywords that simplify asynchronous programming, making it easier to write asynchronous code that's as straightforward as synchronous code.
## 🤔🤔🤔Q40. What is reflection in .NET and how would you use it?
**Reflection** in .NET is a powerful feature that allows runtime inspection of assemblies, types, and their members (such as methods, fields, properties, and events). It enables creating instances of types, invoking methods, and accessing fields and properties dynamically, without knowing the types at compile time. Reflection is used for various purposes, including building type browsers, dynamically invoking methods, and reading custom attributes.

Reflection can be particularly useful in scenarios such as:

- Dynamically loading and using assemblies.
- Implementing object browsers or debuggers.
- Creating instances of types for dependency injection frameworks.
- Accessing and manipulating metadata for assemblies and types.
Here's a simple example demonstrating how to use reflection to inspect and invoke methods of a class dynamically:
```c#
using System;
using System.Reflection;

public class MyClass
{
    public void MethodToInvoke()
    {
        Console.WriteLine("Method Invoked.");
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Obtaining the Type object for MyClass
        Type myClassType = typeof(MyClass);
        
        // Creating an instance of MyClass
        object myClassInstance = Activator.CreateInstance(myClassType);
        
        // Getting the MethodInfo object for MethodToInvoke
        MethodInfo methodInfo = myClassType.GetMethod("MethodToInvoke");
        
        // Invoking the method on the instance
        methodInfo.Invoke(myClassInstance, null);
    }
}
```
In this example, reflection is used to obtain the Type object for MyClass, create an instance of MyClass, and then retrieve and invoke the MethodToInvoke method. This demonstrates how reflection allows for dynamic type inspection and invocation, providing flexibility and power in how code interacts with objects.

Using reflection comes with a performance cost, so it should be used judiciously, especially in performance-critical paths of an application.
## 🤔🤔🤔Q41. Can you describe the process of code compilation in .NET?
The process of code compilation in .NET involves converting high-level code (such as C#) into a platform-independent Intermediate Language (IL) and then into platform-specific machine code. This process is facilitated by the .NET Compiler Platform ("Roslyn" for C# and Visual Basic) and the Common Language Runtime (CLR). Here's an overview of the steps involved:

1. **Source Code to Intermediate Language (IL)**:

When you compile a .NET application, the .NET compiler for your language (e.g., csc.exe for C#) compiles the source code into Intermediate Language (IL). IL is a CPU-independent set of instructions that can be efficiently converted into native machine code.
Along with IL, metadata is generated, which includes information about the types, members, references, and other data that the IL code uses.

2. **IL to Native Code**:

At runtime, the .NET application is executed by the CLR. The CLR's Just-In-Time (JIT) compiler converts the IL code into native machine code specific to the architecture of the target machine.
This conversion happens on a need-to-run basis; that is, each method's IL is converted to native code the first time the method is called. The native code is then cached, so subsequent calls to the same method do not require re-compilation.

3. **Execution**:

The native code is executed directly by the hardware of the target machine, leading to the execution of the .NET application.

**Aspects of .NET Compilation**:

- **Assembly**: The compiled code and resources are packed into assemblies (.dll or .exe files), which are the units of deployment and versioning in .NET. Assemblies contain the IL code and metadata.

- **Metadata**: Metadata describes the assembly itself and the types defined within the assembly, including their methods and properties. This information is used by the CLR during execution and supports features like reflection.

- **Strong Naming and GAC**: Assemblies can be strong-named to ensure their uniqueness and integrity. Strong-named assemblies can be placed in the Global Assembly Cache (GAC) for shared use by multiple applications.

**Optimizations and NGEN**:

- The JIT compiler applies various optimizations while converting IL to native code to improve runtime performance. Additionally, developers can use the Native Image Generator (NGEN) to pre-compile assemblies into native images at install time, reducing JIT compilation time at runtime but at the cost of losing some JIT optimizations specific to the end-user's machine.
```c#
// Example C# code
public class Program
{
    public static void Main(string[] args)
    {
        Console.WriteLine("Hello, World!");
    }
}
```
This simple program is compiled to IL when built and then JIT-compiled to native code by the CLR when executed. Understanding the compilation process in .NET is crucial for grasping how .NET applications are built, deployed, and executed across different environments and platforms.
## 🤔🤔🤔Q42. What is the main difference between DateTime and DateTimeOffset?
1. **DateTime**

- **Represents**: A point in time (date and time) without explicitly accounting for time zone information.

- **Time Zones**: DateTime can represent either local time or UTC (Coordinated Universal Time), but it doesn’t store time zone information.

- **Kind Property**: It has a Kind property which can be Unspecified, Utc, or Local, but this doesn’t automatically adjust the time when converting between different time zones.
```c#
DateTime localTime = DateTime.Now;      // Local system time
DateTime utcTime = DateTime.UtcNow;     // UTC time

Console.WriteLine(localTime);           // Example: 2024-10-19 13:45:00 (local)
Console.WriteLine(utcTime);             // Example: 2024-10-19 18:45:00 (UTC)
```
**When to Use DateTime**:

- When working purely with local or UTC times and you don’t care about time zones or offsets.
- For simple date-related calculations like birthdays, scheduling, or timestamps within a single time zone.

**Common Problem with DateTime**:

If you store DateTime.Now in a database as local time and move your server to another time zone, the timestamps might no longer reflect the original time correctly. This is where DateTimeOffset can help.

2. **DateTimeOffset**

- **Represents**: A point in time with an explicit offset (the difference between the time and UTC).

- **Time Zones**: DateTimeOffset explicitly includes the UTC offset (e.g., +02:00 for Eastern European Summer Time), meaning it retains time zone information even when storing or transmitting the value.

- **Offset Property**: The offset ensures that the value is always correct regardless of where it’s used.
```c#
DateTimeOffset localTimeOffset = DateTimeOffset.Now;       // Local time with offset
DateTimeOffset utcTimeOffset = DateTimeOffset.UtcNow;      // UTC time with offset

Console.WriteLine(localTimeOffset);                        // Example: 2024-10-19 13:45:00 +02:00
Console.WriteLine(utcTimeOffset);                          // Example: 2024-10-19 11:45:00 +00:00 (UTC)
```
**When to Use DateTimeOffset**:

- When you need to account for different time zones or store times that are globally relevant (e.g., scheduling across time zones, international event logging).
- For systems where time is passed between different time zones or where historical time zone information must be preserved.

**Real-World Example**:

Imagine you're building a calendar app that allows users to set reminders or schedule meetings.

- DateTime Use Case: If all your users are within the same time zone, you can use DateTime to store and manipulate event times.

- DateTimeOffset Use Case: If users across different time zones are scheduling meetings, you'd use DateTimeOffset to ensure that everyone sees the correct time for their specific location. For instance, a meeting set for 10:00 AM EST (+05:00) will still be 10:00 AM in that zone even if another user views it from UTC or PST.

**Summary**:
- Use DateTime for simple date/time representations where time zone context is not critical.
- Use DateTimeOffset when time zone or UTC offset information is important, especially for globally distributed applications.
## 🤔🤔🤔Q43. Class vs Record in C#?
**Class**

A class in C# is a reference type that can encapsulate data (fields) and behavior (methods). Classes allow you to define objects that contain data and behavior in the form of properties and methods.

**Key Characteristics of a Class**:

- **Mutable**: Class instances can have their values changed after they are created.

- **Reference type**: Class objects are stored on the heap, and variables hold references (or pointers) to the memory location of the object.

- **Inheritance**: Supports inheritance and polymorphism, allowing you to create hierarchies of related objects.

- **Encapsulation**: Supports hiding the internal state through access modifiers (like private and protected).
```c#
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    
    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }
}
```
**Record**

A record in C# is a reference type introduced in C# 9, primarily designed for immutable data and for value equality rather than reference equality. It’s used to represent data-centric classes with a focus on immutability and equality based on the values of its properties.

**Key Characteristics of a Record**:

- **Immutable by default**: Record properties are immutable (can only be set during initialization), though they can be made mutable if needed.

- **Value equality**: Unlike classes, records compare objects based on their values (property values), not their reference (memory location).

- **Concise syntax**: Record types have a shorthand syntax for declaring properties, making the code more concise.

- **Positional records**: Records can be defined using a shorthand positional syntax.
```c#
public record Person(string Name, int Age);
```
**👉What are the differences between a class and a record in C#?**

- Focus on key differences like mutability, equality, and use cases. Mention that records offer value equality and are designed for immutability, whereas classes focus on mutable state and reference equality.

**👉When would you choose a class over a record in C#?**

- Emphasize the need for mutable state, complex behavior, and inheritance when choosing a class over a record.

**👉How does value equality in records differ from reference equality in classes?**

- Explain that with classes, equality compares object references (memory locations), while records compare the actual values of the properties to determine equality.

**👉Can records be mutable in C#?**

- Yes, although records are designed to be immutable by default, you can create mutable records by using the init keyword or explicitly making properties settable.

**👉Can you explain positional records and their advantages?**

- Positional records allow for a concise syntax for declaring records, especially useful for simple data structures.
## 🤔🤔🤔Q44. Explain the Builder Pattern and how it can be used for constructing complex objects like an Order in an e-commerce system.
The **Builder Pattern** helps in constructing complex objects step by step. The builder pattern constructs the order object step by step based on your selections.

**Code Example:**
```c#
// Step 1: Product class (Order)
public class Order
{
    public string Product { get; set; }
    public int Quantity { get; set; }
    public bool HasGiftWrap { get; set; }
    public bool HasExpressShipping { get; set; }
    public override string ToString()
    {
        return $"Product: {Product}, Quantity: {Quantity}, Gift Wrap: {HasGiftWrap}, Express Shipping: {HasExpressShipping}";
    }
}

// Step 2: Builder Interface
public interface IOrderBuilder
{
    IOrderBuilder SetProduct(string product);
    IOrderBuilder SetQuantity(int quantity);
    IOrderBuilder AddGiftWrap();
    IOrderBuilder AddExpressShipping();
    Order Build();
}

// Step 3: Concrete Builder
public class OrderBuilder : IOrderBuilder
{
    private Order _order = new Order();
    public IOrderBuilder SetProduct(string product)
    {
        _order.Product = product;
        return this;
    }
    public IOrderBuilder SetQuantity(int quantity)
    {
        _order.Quantity = quantity;
        return this;
    }
    public IOrderBuilder AddGiftWrap()
    {
        _order.HasGiftWrap = true;
        return this;
    }

    public IOrderBuilder AddExpressShipping()
    {
        _order.HasExpressShipping = true;
        return this;
    }

    public Order Build()
    {
        return _order;
    }
}

// Step 4: Client code

public class Client
{
    public static void Main()
    {
        IOrderBuilder builder = new OrderBuilder();
        // Building an order with various options
        Order order = builder
        .SetProduct("Laptop")
        .SetQuantity(1)
        .AddGiftWrap()
        .AddExpressShipping()
        .Build();

        Console.WriteLine(order);
    }
}
```
In the **Builder Pattern**, the reason for returning the interface (IOrderBuilder) in each method is to implement method chaining. This allows for cleaner, more readable code when constructing complex objects. By returning IOrderBuilder, you can call multiple methods in a single statement, which is a common design choice to enhance fluency in object creation.


Let’s break down the concept with your example to clarify it:

**Why Return IOrderBuilder?**

- Each method in the builder pattern (e.g., SetProduct, SetQuantity) modifies the state of the object being built (in this case, an Order). After each modification, the method returns this (which is the builder instance). This allows the next method to be called on the same instance, forming a chain of method calls.

**Without returning IOrderBuilder**:

- If the builder methods didn’t return IOrderBuilder, you'd need to call each method separately, like this:
```c#
OrderBuilder builder = new OrderBuilder();
builder.SetProduct("Laptop");
builder.SetQuantity(1);
builder.AddGiftWrap();
builder.AddExpressShipping();

Order order = builder.Build();
```

Here, every method call is separate, and the code is less readable when multiple steps are involved.

**With method chaining (returning IOrderBuilder)**:

- By returning IOrderBuilder, you enable a fluent interface, which allows calls to be chained

**Another Example**:

Let’s visualize this with an SQL query builder:
```c#
public interface ISqlBuilder
{
    ISqlBuilder Select(string columns);
    ISqlBuilder From(string table);
    ISqlBuilder Where(string condition);
    string Build();
}

public class SqlBuilder : ISqlBuilder
{
    private string query = string.Empty;
    public ISqlBuilder Select(string columns)
    {
        query += $"SELECT {columns} ";
        return this; // Returning the builder object for method chaining
    }

    public ISqlBuilder From(string table)
    {
        query += $"FROM {table} ";
        return this;
    }

    public ISqlBuilder Where(string condition)
    {
        query += $"WHERE {condition}";
        return this;
    }

    public string Build()
    {
        return query;
    }
}

// Client Code

public class Client
{
    public static void Main()
    {
        ISqlBuilder sqlBuilder = new SqlBuilder();
        string query = sqlBuilder
        .Select("*")
        .From("Products")
        .Where("Price > 1000")
        .Build();
        
        Console.WriteLine(query); // Output: SELECT * FROM Products WHERE Price > 1000
    }
}
```
## 🤔🤔🤔Q45. How would you use the Prototype Pattern in a user authentication system where user profiles need to be cloned and modified for temporary sessions?
The **Prototype Pattern** is used when the cost of creating a new object is expensive or complex. Instead of creating a new object from scratch, you can clone an existing object and make modifications.

**Visualization**:

Think of the Prototype Pattern like making a photocopy of a document:

- The original document remains unchanged.
- The photocopy can be modified (e.g., you can write or highlight on it) without affecting the original. In this case, you clone a user profile for temporary use, modify the clone for that session, and the original user profile stays untouched.

**Why Use the Prototype Pattern in User Authentication?**
1. **Efficient Object Creation**: Cloning a profile is faster than creating a new user profile from scratch, especially when multiple properties need to be copied.
2. **Maintaining Original Data**: You can preserve the integrity of the original user profile while modifying the cloned profile for temporary or session-based operations.
3. **Flexibility**: This allows for flexibility in handling temporary access or guest sessions without altering the user’s core data, making the system more scalable and easier to maintain.

Code Example:
```c#
// Step 1: User class implementing ICloneable (Prototype)
public class User : ICloneable
{
    public string Name { get; set; }
    public string Role { get; set; }
    // Cloning method
    public object Clone()
    {
        return this.MemberwiseClone(); // Shallow copy
    }

    public override string ToString()
    {
        return $"User: {Name}, Role: {Role}";
    }
}

// Step 2: Client code to demonstrate prototype usage

public class Client
{
    public static void Main()
    {
        // Original user (admin profile)
        User adminUser = new User { Name = "Alice", Role = "Admin" };
        // Cloning the admin profile for a temporary session
        User tempUser = (User)adminUser.Clone();
        tempUser.Name = "Temporary User"; // Modify the name
        // Both users have different names but share the same role
        Console.WriteLine(adminUser); // Output: User: Alice, Role: Admin
        Console.WriteLine(tempUser); // Output: User: Temporary User, Role: Admin
    }
}
```
**Another Example**:
```c#
// Step 1: Define the prototype interface (ICloneable for simplicity)
public abstract class UserProfile
{
    public string Username { get; set; }
    public string Role { get; set; }
    public bool IsTemporary { get; set; }
    // Clone method to be implemented by concrete classes
    public abstract UserProfile Clone();
    public override string ToString()
    {
        return $"Username: {Username}, Role: {Role}, IsTemporary: {IsTemporary}";
    }
}

// Step 2: Concrete implementation of UserProfile
public class ConcreteUserProfile : UserProfile
{
    public override UserProfile Clone()
    {
        // Creating a shallow copy using MemberwiseClone
        return (UserProfile)this.MemberwiseClone();
    }
}

// Step 3: Client code that clones and modifies user profiles
public class Client
{
    public static void Main()
    {
        // Original user profile
        UserProfile originalProfile = new ConcreteUserProfile
        {
            Username = "JohnDoe",
            Role = "Admin",
            IsTemporary = false
        };

        Console.WriteLine("Original Profile:");
        Console.WriteLine(originalProfile);
        // Cloning the profile for a temporary session
        UserProfile temporaryProfile = originalProfile.Clone();
        temporaryProfile.IsTemporary = true; // Marking the cloned profile as temporary
        temporaryProfile.Role = "Guest"; // Changing the role to a guest for the session
        Console.WriteLine("\nTemporary Profile:");
        Console.WriteLine(temporaryProfile);
        // Original profile remains unchanged
        Console.WriteLine("\nOriginal Profile After Cloning:");
        Console.WriteLine(originalProfile);
    }
}
```
**Output**:
```c#
Original Profile:

Username: JohnDoe, Role: Admin, IsTemporary: False

Temporary Profile:

Username: JohnDoe, Role: Guest, IsTemporary: True

Original Profile After Cloning:

Username: JohnDoe, Role: Admin, IsTemporary: False
```
## 🤔🤔🤔Q46. Can you explain the Abstract Factory Pattern using a User Authentication System that supports different providers (e.g., Google, Facebook)?
The **Abstract Factory** Pattern provides an interface to create a family of related or dependent objects without specifying their concrete classes. In an authentication system, you may want to support multiple login providers like Google, Facebook, etc., with each provider having its own login and user profile mechanisms.

**Code Example**:
```c#
// Step 1: Abstract product interfaces (Login and Profile)
public interface ILoginProvider
{
    void Authenticate();
}

public interface IUserProfile
{
    void FetchProfile();
}

// Step 2: Concrete products for Google
public class GoogleLogin : ILoginProvider
{
    public void Authenticate()
    {
        Console.WriteLine("Authenticating with Google");
    }
}

public class GoogleUserProfile : IUserProfile
{
    public void FetchProfile()
    {
        Console.WriteLine("Fetching Google user profile");
    }
}

// Step 3: Concrete products for Facebook
public class FacebookLogin : ILoginProvider
{
    public void Authenticate()
    {
        Console.WriteLine("Authenticating with Facebook");
    }
}

public class FacebookUserProfile : IUserProfile
{
    public void FetchProfile()
    {
        Console.WriteLine("Fetching Facebook user profile");
    }
}

// Step 4: Abstract factory interface
public interface IAuthProviderFactory
{
    ILoginProvider CreateLoginProvider();
    IUserProfile CreateUserProfile();
}

// Step 5: Concrete factories for Google and Facebook
public class GoogleAuthProviderFactory : IAuthProviderFactory
{
    public ILoginProvider CreateLoginProvider() => new GoogleLogin();
    public IUserProfile CreateUserProfile() => new GoogleUserProfile();
}

public class FacebookAuthProviderFactory : IAuthProviderFactory
{
    public ILoginProvider CreateLoginProvider() => new FacebookLogin();
    public IUserProfile CreateUserProfile() => new FacebookUserProfile();
}

// Step 6: Client code using the abstract factory
public class Client
{
    public static void Main()
    {
        IAuthProviderFactory googleFactory = new GoogleAuthProviderFactory();
        ILoginProvider googleLogin = googleFactory.CreateLoginProvider();
        IUserProfile googleProfile = googleFactory.CreateUserProfile();
        googleLogin.Authenticate();
        googleProfile.FetchProfile();

        IAuthProviderFactory facebookFactory = new FacebookAuthProviderFactory();
        ILoginProvider facebookLogin = facebookFactory.CreateLoginProvider();
        IUserProfile facebookProfile = facebookFactory.CreateUserProfile();
        facebookLogin.Authenticate();
        facebookProfile.FetchProfile();
    }
}
```
**Line-by-Line Explanation**:
1. ILoginProvider and IUserProfile are abstract products defining login and profile-fetching behaviors.
2. GoogleLogin and GoogleUserProfile are concrete implementations for Google.
3. FacebookLogin and FacebookUserProfile are concrete implementations for Facebook.
4. IAuthProviderFactory is the abstract factory interface for creating related products (login and profile).
5. GoogleAuthProviderFactory and FacebookAuthProviderFactory are concrete factories that create Google and Facebook-related products, respectively.
6. Client code uses the factory to switch between different providers.

**Visualization:**

Imagine an app that supports login with both Google and Facebook. The user selects one provider, and the app uses the correct login and profile-fetching mechanism for the chosen provider without the need to modify the core logic.
## 🤔🤔🤔Q47.  How would you use the Factory Method Pattern to create different types of notifications (email, SMS, push) in a notification system?
The **Factory Method Pattern** is ideal when you need to create different types of objects but want to delegate the object creation logic to subclasses. In a notification system that sends different types of notifications (e.g., email, SMS, push notifications), the Factory Method Pattern helps in cleanly separating the logic for creating these different notifications.

**Scenario**:

You have a notification system where you need to send:
- **Email notifications**
- **SMS notifications**
- **Push notifications**

Each notification has its own implementation, but they share a common interface. Using the Factory Method Pattern, you define a method to create the notification, and the subclasses implement the actual creation logic for each type.

**Step-by-Step Example**:

1. **Notification Interface (Product)**
```c#
// Step 1: Define a common interface for notifications
public interface INotification
{
    void SendNotification(string message);
}
```

2. **Concrete Notification Classes(Concrete Products)**


Now, we implement specific notification types like EmailNotification, SMSNotification, and PushNotification:
```c#
// Step 2: Concrete implementations of notifications
public class EmailNotification : INotification
{
    public void SendNotification(string message)
    {
        Console.WriteLine($"Sending Email Notification: {message}");
    }
}

public class SMSNotification : INotification
{
    public void SendNotification(string message)
    {
        Console.WriteLine($"Sending SMS Notification: {message}");
    }
}

public class PushNotification : INotification
{
    public void SendNotification(string message)
    {
        Console.WriteLine($"Sending Push Notification: {message}");
    }
}
```

Each class implements the INotification interface and defines how to send the specific type of notification (email, SMS, or push).

3. **Notification Creator (Creator)**

Next, we create the Factory Method inside a NotificationFactory class. This class defines a method CreateNotification which will be implemented by the subclasses to create different types of notifications:

```c#
// Step 3: Abstract factory class defining the factory method
public abstract class NotificationFactory
{
    // Factory method to be implemented by subclasses
    public abstract INotification CreateNotification();
    public void Notify(string message)
    {
        // Get the notification instance using the factory method
        INotification notification = CreateNotification();
        // Send the notification
        notification.SendNotification(message);
    }
}
```
Here, CreateNotification is the Factory Method that each subclass will override to return the appropriate notification type.

4. **Concrete Factories for Each Notification Type**

Now, let’s implement concrete factories for each notification type:
```c#
// Step 4: Concrete factories that create specific notifications
public class EmailNotificationFactory : NotificationFactory
{
    public override INotification CreateNotification()
    {
        return new EmailNotification();
    }
}

public class SMSNotificationFactory : NotificationFactory
{
    public override INotification CreateNotification()
    {
        return new SMSNotification();
    }
}

public class PushNotificationFactory : NotificationFactory
{
    public override INotification CreateNotification()
    {
        return new PushNotification();
    }
}
```
Each factory class overrides the CreateNotification method and returns the appropriate notification object (EmailNotification, SMSNotification, or PushNotification).

5. **Client Code**

Finally, the client code uses the factory classes to create and send notifications without worrying about the underlying creation logic.
```c#
// Step 5: Client code
public class Client
{
    public static void Main()
    {
        // Email Notification
        NotificationFactory emailFactory = new EmailNotificationFactory();
        emailFactory.Notify("This is an email message.");
        // SMS Notification
        NotificationFactory smsFactory = new SMSNotificationFactory();
        smsFactory.Notify("This is an SMS message.");
        // Push Notification
        NotificationFactory pushFactory = new PushNotificationFactory();
        pushFactory.Notify("This is a push notification message.");
    }
}
```
**Output**:
```c#
Sending Email Notification: This is an email message.

Sending SMS Notification: This is an SMS message.

Sending Push Notification: This is a push notification message.
```
**Explanation**:

1. Abstract Factory(NotificationFactory): Defines the CreateNotification method, which is the factory method responsible for creating different types of notifications.
2. Concrete Factories (EmailNotificationFactory, SMSNotificationFactory, PushNotificationFactory): Each subclass implements the factory method and provides the specific notification object.
3. Client: The client uses the factory to create the desired type of notification (email, SMS, push) without needing to know the details of how these notifications are created.

**Visualization**:

Think of the Factory Method Pattern as a restaurant menu:

The Menu(the abstract factory) gives you the option to order different types of meals.

Each Kitchen Section (concrete factory) prepares a specific type of meal (e.g., email, SMS, push notifications).

As a Customer (client), you simply place your order from the menu, and the right kitchen section prepares the meal, without you worrying about the preparation details.

**Why Use the Factory Method Pattern for Notifications?**

- **Separation of Concerns**: The client code only knows about the interface (INotification) and not the specific details of each notification type.
- **Scalability**: You can easily add new notification types(e.g., Slack, Webhooks) by creating new concrete factories without modifying existing code.
- **Flexibility**: The Factory Method Pattern allows the system to decide which notification to create at runtime based on dynamic factors

**Comparison: Abstract Factory vs Factory Method**

1. **Object Creation**:
- Factory Method creates one object (e.g., EmailNotification).
- Abstract Factory creates a family of related objects (e.g., both WindowsButton and WindowsCheckbox).

2. **Use Case**:
- Factory Method is used when the system needs to create one type of object but the specific type is determined by subclasses.
- Abstract Factory is used when the system needs to create a group of related objects and ensure they work together.

3. **Flexibility**:
- Factory Method focuses on a single product.
- Abstract Factory focuses on creating related products.

**Example**:

- **Factory Method**: Deciding whether to send an email, SMS, or push notification at runtime.
- **Abstract Factory**: Ensuring that all the UI components in your application (buttons, checkboxes) are compatible with the platform (Windows, macOS).
## 🤔🤔🤔Q48.  Explain the Singleton Pattern. How would you implement it in .NET to manage a database connection pool?
The **Singleton Pattern** ensures that a class has only one instance and provides a global point of access to it. In .NET, this is useful for managing shared resources like a database connection pool where you need to ensure that only one instance of the connection pool is used throughout the application.

**Code Example:**
```c#
public class DbConnectionPool
{
    // Step 1: Private static instance of the class (single instance)
    private static DbConnectionPool _instance;
    // Step 2: Private constructor to prevent instantiation
    private DbConnectionPool()
    {
        Console.WriteLine("Connection pool created.");
    }
    
    // Step 3: Public static method to provide access to the instance
    public static DbConnectionPool GetInstance()
    {
        if (_instance == null)
        {
            _instance = new DbConnectionPool();
        }

        return _instance;
    }

    // Simulate fetching a connection from the pool
    public void GetConnection()
    {
        Console.WriteLine("Fetching a connection from the pool...");
    }
}

// Client code to test Singleton pattern
public class Client
{
    public static void Main()
    {
        DbConnectionPool pool1 = DbConnectionPool.GetInstance();
        pool1.GetConnection();

        DbConnectionPool pool2 = DbConnectionPool.GetInstance();
        pool2.GetConnection();

        // Both pool1 and pool2 refer to the same instance
        Console.WriteLine(pool1 == pool2); // Output: True
    }
}
```
**Line - by - Line Explanation**:

- DbConnectionPool class has a private static field _instance which holds the single instance of the class.
- Private constructor ensures that the class cannot be instantiated from outside.
- GetInstance method returns the single instance of DbConnectionPool, creating it if it doesn’t exist.
- Client code demonstrates how multiple calls to GetInstance() return the same instance.

**Visualization**:

Imagine a customer service desk at a store. There's only one desk and one representative (the Singleton). No matter how many times you visit the desk, you’ll always talk to the same person.
## 🤔🤔🤔Q48. How would you use the Strategy Pattern to implement a payment system with multiple payment methods (Credit Card, PayPal) in .NET?
The **Strategy Pattern** allows you to define a family of algorithms, encapsulate each one, and make them interchangeable. In a payment system, the strategy pattern allows you to switch between different payment methods (credit card, PayPal) at runtime.

**Code Example:**
```c#
// Step 1: Define the Strategy interface
public interface IPaymentStrategy
{
    void Pay(decimal amount);
}

// Step 2: Concrete Strategies
public class CreditCardPayment : IPaymentStrategy
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Paid {amount} using Credit Card");
    }
}

public class PayPalPayment : IPaymentStrategy
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Paid {amount} using PayPal");
    }
}

// Step 3: Context class
public class PaymentContext
{
    private IPaymentStrategy _paymentStrategy;
    // Constructor to set payment strategy
    public PaymentContext(IPaymentStrategy paymentStrategy)
    {
        _paymentStrategy = paymentStrategy;
    }

    public void ProcessPayment(decimal amount)
    {
        _paymentStrategy.Pay(amount);
    }
}

// Client code
public class Client
{
    public static void Main()
    {
        PaymentContext paymentContext;
        // Using credit card strategy
        paymentContext = new PaymentContext(new CreditCardPayment());
        paymentContext.ProcessPayment(100);

        // Switching to PayPal strategy
        paymentContext = new PaymentContext(new PayPalPayment());
        paymentContext.ProcessPayment(200);
    }
}
```
**Line - by - Line Explanation**:

- IPaymentStrategy defines the Pay method, which all payment methods must implement.
- CreditCardPayment and PayPalPayment are concrete strategies that implement the payment logic.
- PaymentContext is the context that interacts with a payment strategy to process payments.
- Client code demonstrates switching between credit card and PayPal payment methods at runtime.

**Visualization**:

Imagine going to a store that accepts both cash and cards. Depending on what you choose (strategy), the cashier processes your payment differently, but the result is the same—you pay for your items.
## 🤔🤔🤔Q48. Can you explain the Observer Pattern with a real-world example of notifying multiple components when a user updates their profile in a .NET application?
The **Observer Pattern** defines a one-to-many dependency between objects so that when one object (subject) changes state, all its dependents (observers) are notified. In a .NET application, this could be used to notify multiple systems (e.g., logging, analytics) when a user updates their profile.

**Code Example**:
```c#
// Step 1: Define the Observer interface
public interface IObserver
{
    void Update(string message);
}

// Step 2: Concrete Observers
public class AnalyticsService : IObserver
{
    public void Update(string message)
    {
        Console.WriteLine("Analytics updated: " + message);
    }
}

public class LoggingService : IObserver
{
    public void Update(string message)
    {
        Console.WriteLine("Log entry created: " + message);
    }
}

// Step 3: Subject class
public class UserProfile
{
    private List<IObserver> _observers = new List<IObserver>();
    // Method to attach observers
    public void Attach(IObserver observer)
    {
        _observers.Add(observer);
    }
    
    // Method to notify observers
    private void Notify(string message)
    {
        foreach (var observer in _observers)
        {
            observer.Update(message);
        }
    }

    // Simulating a profile update
    public void UpdateProfile(string userName)
    {
        Console.WriteLine($"User {userName}'s profile updated.");
        Notify($"Profile updated for {userName}");
    }
}

// Client code
public class Client
{
    public static void Main()
    {
        UserProfile profile = new UserProfile();
        profile.Attach(new AnalyticsService());
        profile.Attach(new LoggingService());
        profile.UpdateProfile("JohnDoe");
    }
}
```
**Line - by - Line Explanation**:

- IObserver defines the Update method, which will be implemented by all observers.
- AnalyticsService and LoggingService are concrete observers that react when the subject (profile) changes.
- UserProfile (the subject) maintains a list of observers. When the profile is updated, all observers are notified.
- Client code attaches multiple observers (analytics and logging) and triggers a profile update, which notifies all attached observers.

**Visualization**:
Think of a YouTube channel. When a new video is uploaded(profile update), all subscribers(observers) are notified.
## 🤔🤔🤔Q49. What is the Decorator Pattern, and how can it be used to extend the functionality of a logging service in .NET?
The **Decorator Pattern** allows you to add additional behavior or functionality to an object dynamically without modifying its core structure. To visualize this in a simple real-world example, let’s imagine a Messaging App where you can send basic messages, but you can decorate them with additional features like encryption or adding emojis, based on the user’s preferences.

**Real-World Example: Messaging App**

In a messaging app, when you send a message, you might want to:
1. Send a plain message.
2. Encrypt the message for security.
3. Add Emojis to make it more expressive.
4. Do both: Encrypt and Add Emojis.

You can use the **Decorator Pattern** to dynamically “decorate” the basic message with these additional features, like encryption or emojis.

**Visualization Using a Messaging App Example**:

1. **Basic Message (Component)**

Let’s assume you have a simple message that can be sent as plain text.
```c#
// Base message interface
public interface IMessage
{
    string Send();
}

// Plain message class (Concrete Component)
public class PlainMessage : IMessage
{
    private string _content;
    public PlainMessage(string content)
    {
        _content = content;
    }

    public string Send()
    {
        return _content;
    }
}
```
Here, the IMessage interface defines a Send method, and PlainMessage sends a basic plain text message.

2. **Message Decorator (Base Decorator)**

Now, we define a base decorator class that implements the IMessage interface and holds a reference to an IMessage object.
```c#
// Base decorator class
public class MessageDecorator : IMessage
{
    protected IMessage _message;
    public MessageDecorator(IMessage message)
    {
        _message = message;
    }

    public virtual string Send()
    {
        return _message.Send();
    }
}
```
The MessageDecorator class doesn’t change the original message behavior but allows additional features to be added by extending it.

3. **Encryption Decorator (Concrete Decorator)**

Let’s add encryption to the message. This decorator will wrap the basic message and encrypt it.
```c#
// Encryption decorator
public class EncryptedMessageDecorator : MessageDecorator
{
    public EncryptedMessageDecorator(IMessage message) : base(message) { }
    public override string Send()
    {
        // Simulate encryption by reversing the string
        string encryptedMessage = Reverse(_message.Send());
        return $"[Encrypted] {encryptedMessage}";
    }

    private string Reverse(string input)
    {
        char[] chars = input.ToCharArray();
        Array.Reverse(chars);
        return new string(chars);
    }
}
```
The EncryptedMessageDecorator encrypts the message by reversing the string (a simple simulation of encryption) and adds an “[Encrypted]” label.

4. **Emoji Decorator (Concrete Decorator)**

Let’s add an emoji to the message.
```c#
// Emoji decorator
public class EmojiMessageDecorator : MessageDecorator
{
    public EmojiMessageDecorator(IMessage message) : base(message) { }
    public override string Send()
    {
        return _message.Send() + " 😊"; // Add a smiley emoji to the message
    }
}
```
The EmojiMessageDecorator adds an emoji to the end of the message.

5. **Client Code: Using the Decorators**

Now, let’s see how the client can use these decorators to add different features to the message dynamically.
```c#
// Client code
public class Client
{
    public static void Main()
    {
        // Basic plain message
        IMessage message = new PlainMessage("Hello, world!");
        // Decorate with encryption
        message = new EncryptedMessageDecorator(message);
        Console.WriteLine(message.Send()); // Output: [Encrypted] !dlrow ,olleH
        // Decorate with both encryption and emoji
        message = new EmojiMessageDecorator(message);
        Console.WriteLine(message.Send()); // Output: [Encrypted] !dlrow ,olleH 😊
    }
}
```
**Output**:
```c#
Encrypted Message:
[Encrypted] !dlrow ,olleH

Encrypted Message with Emoji:
[Encrypted] !dlrow ,olleH 😊
```
**Explanation**:

- Plain Message: You start with a basic message (e.g., “Hello, world!”).
- Add Encryption: The EncryptedMessageDecorator wraps the message and adds encryption (in this case, reversing the message text for simplicity).
- Add Emoji: The EmojiMessageDecorator further decorates the encrypted message by adding an emoji at the end.

**Visualization in Simpler Words**:

- Think of it like sending a text message:
- Plain Message: You send a regular message.
- Encrypt Message: You want to secure the message before sending, so you add encryption (like putting it in a locked envelope).
- Add Emoji: You want to make the message more expressive, so you add a smiley emoji after the message.

Each new feature (encryption, emoji) doesn’t modify the original message system; instead, it wraps the original message with additional functionality. You can combine multiple decorators to achieve the final result.

**Advantages of Using the Decorator Pattern**:
- **Extensibility**: You can easily add new features (like emojis or encryption) without modifying the original message class.
- **Flexibility**: You can add features dynamically at runtime, depending on the user’s preferences.
- **Separation of Concerns**: Each decorator focuses on a specific functionality, making your code easier to maintain.

**Summary**:

The **Decorator Pattern** allows you to dynamically add features like encryption or emojis to a messaging system without changing the core message logic. You can stack or combine decorators to modify the behavior of an object step by step, making your system more flexible and extensible.

The **Decorator Pattern** allows behavior to be added to an individual object, dynamically, without affecting the behavior of other objects from the same class. In .NET, this can be used to add additional functionality to a logging service, such as logging to a file or logging to a database, without modifying the base logging class.

**Another Code Example**:
```c#
// Step 1: Define a base component (ILogger)
public interface ILogger
{
    void Log(string message);
}

// Step 2: Concrete component implementing base component
public class SimpleLogger : ILogger
{
    public void Log(string message)
    {
        Console.WriteLine("Log: " + message);
    }
}

// Step 3: Decorator abstract class implementing the base component
public abstract class LoggerDecorator : ILogger
{
    protected ILogger _logger;
    public LoggerDecorator(ILogger logger)
    {
        _logger = logger;
    }

    public virtual void Log(string message)
    {
        _logger.Log(message);
    }
}

// Step 4: Concrete decorators that extend functionality
public class FileLogger : LoggerDecorator
{
    public FileLogger(ILogger logger) : base(logger) {}
    public override void Log(string message)
    {
        base.Log(message);
        Console.WriteLine("Log written to file: " + message);
    }
}

public class DatabaseLogger : LoggerDecorator
{
    public DatabaseLogger(ILogger logger) : base(logger) {}
    public override void Log(string message)
    {
        base.Log(message);
        Console.WriteLine("Log saved to database: " + message);
    }
}

// Client code
public class Client
{
    public static void Main()
    {
        ILogger logger = new SimpleLogger();
        ILogger fileLogger = new FileLogger(logger);
        ILogger dbLogger = new DatabaseLogger(fileLogger);
        dbLogger.Log("This is a log message");
    }
}
```
**Line-by-Line Explanation**:

- ILogger is the base component, which defines the Log method.
- SimpleLogger is the concrete implementation of ILogger that logs a message to the console.
- LoggerDecorator is an abstract class that implements ILogger and holds a reference to an instance of ILogger. It acts as a wrapper.
- FileLogger and DatabaseLogger are concrete decorators that add additional logging functionality (to a file or database).
- Client code demonstrates chaining decorators to log a message to both a file and a database.

**Visualization**:

Imagine a basic cake. Now, you add icing (decorator). Then, you add sprinkles on top of the icing (another decorator). The cake is still the same, but it now has extra layers of functionality.
## 🤔🤔🤔Q50. ref vs out vs in?
**ref Keyword**:

Purpose: Allows passing parameters by reference, enabling the method to read and modify the caller's variable directly.

Initialization: The variable must be initialized before passing it to the method.

Use Case: Ideal when the method needs to both read and modify a value.
```c#
public class Bank
{
    public void ApplyDiscount(ref decimal balance, decimal discount)
    {
        balance -= discount; // Modify the original balance
    }
}

// Usage
decimal accountBalance = 1000m;

decimal discount = 100m;

var bank = new Bank();

bank.ApplyDiscount(ref accountBalance, discount);
// accountBalance is now 900m
```
**out Keyword**:

Purpose: Similar to ref, but used for scenarios where the method only assigns a value to the variable.

Initialization: The variable does not need to be initialized before passing it to the method; however, it must be assigned a value inside the method.

Use Case: Useful when a method needs to return multiple values.
```c#
public class AuthService
{
    public bool TryLogin(string username, string password, out string authToken)
    {
        // Simulate login logic
        if (username == "user123" && password == "pass123")
        {
            authToken = "ABC123TOKEN"; // Assign a value
            return true;
        }

        authToken = string.Empty; // Must assign before return

        return false;
    }
}

// Usage
string token;
var authService = new AuthService();
if (authService.TryLogin("user123", "pass123", out token))
{
    // token contains "ABC123TOKEN"
}
```
**Usage of in Keyword**:

The in keyword in C# is primarily used to define a parameter that is passed by reference but is intended to be read-only. It allows you to pass large structs to methods without copying them, thereby improving performance, while ensuring that the method cannot modify the data.

Example:

Here’s a simple example demonstrating the use of in with a struct:
```c#
public struct Point
{
    public int X { get; }
    public int Y { get; }
    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }
}

public class Geometry
{
    public double CalculateDistance(in Point p1, in Point p2)
    {
        return Math.Sqrt(Math.Pow(p2.X - p1.X, 2) + Math.Pow(p2.Y - p1.Y, 2));
    }
}

// Usage
class Program
{
    static void Main()
    {
        var point1 = new Point(1, 2);
        var point2 = new Point(4, 6);
        var geometry = new Geometry();
        double distance = geometry.CalculateDistance(in point1, in point2);
        Console.WriteLine($"Distance: {distance}");
    }
}
```
Is It Beneficial to Use in for Reference Types?

Using the in keyword with reference types does not provide significant benefits because reference types are already passed by reference. When you pass a reference type to a method, you are passing a reference to the object, not the object itself.

However, you can still use in with reference types to signal intent that the method will not modify the reference itself or the object it points to:
```c#
public class User
{
    public string Name { get; set; }
}

public class UserService
{
    public void PrintUser(in User user)
    {
        Console.WriteLine(user.Name);
        // Cannot modify user.Name or reassign user to a new User object.
    }
}
```
## 🤔🤔🤔Q51. Can you explain the Common Language Runtime (CLR)?
