# Interview-Preparation-V1
## Q1: What is the main difference between the HTTP PUT and PATCH methods?
1. PUT is used to update a resource by replacing the entire resource with a new version. It usually requires sending the full resource in the request.
2. PATCH is used to partially update a resource. It allows you to send only the fields that need to be updated, rather than the entire resource.
### 1. PUT Method:
- Idempotency rule: PUT is idempotent. Making the same PUT request multiple times will always result in the same state on the server.
### Explanation:
- PUT is used to update or replace a resource on the server. The key point is that it either creates or fully updates the resource in a way that every time you send the same data, the result will be the same.
### Example: Let's say you're updating a user's profile information with PUT:
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
### 2. POST Method:
- Idempotency rule: POST is NOT idempotent. Making the same POST request multiple times may result in multiple resources being created or multiple actions being taken.
### Explanation:
- POST is used to create a new resource or perform an action. When you send a POST request, it typically creates a new resource, and sending the same request again may create a duplicate or trigger an additional action.
### Example: Let's say you're submitting a form to create a new user:
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

### 3. PATCH Method:
- Idempotency rule: PATCH is idempotent if the same request is applied multiple times. However, the change depends on how you use it.
### Explanation:
- PATCH is used to partially update a resource. It modifies only the specified fields. Sending the same PATCH request multiple times should lead to the same result as sending it once (if the request doesn’t conflict with the resource's current state).
### Example: Let's say you're updating just the email of a user:
 ```c#   
PATCH /users/123
{
  "email": "newemail@example.com"
}
```
The first time you send this request, it updates only the email field of user 123 to "newemail@example.com".
If you send the same request again, the server will still update the email to "newemail@example.com", but since it’s already the same, the state won’t change.

Result: If the request is structured properly, sending the same PATCH request multiple times won’t change anything after the first one. The result is idempotent because the resource is left in the same state.
#### PATCH: Generally Idempotent. Repeating the same request leads to the same outcome, assuming the state doesn’t conflict.

## Q2: When would you choose to use PUT instead of PATCH?
1. Use PUT when you want to completely replace the resource. If you don’t send the full resource with a PUT request, any fields you omit might be reset or lost.
For example, if you have an object with 5 properties and want to change 1 of them, using PUT means you should send all 5 properties. Any missing properties might be deleted.
2. Use PATCH when you only need to update part of a resource and want to avoid sending the entire object.
For example, if you want to update just one field in a large object, PATCH allows you to send a small request with only the modified field(s), which can be more efficient.
## Q3: Can you use PUT to create a resource?
Yes, PUT can also be used for creation if the resource does not exist yet. If you send a PUT request to a URI that does not exist, it can create a new resource at that URI.
PATCH is typically not used for creating new resources, as it is primarily for modifications.
## Q4: What is FINALLY?
1. The finally block is used to execute a given set of statements, whether an exception occur or not.
2. Finally block is mostly used to dispose the unwanted objects when they are no more required. This is good for performance, otherwise you have to wait for garbage collector to dispose them.
## Q5: What is Finalize?
1. Finalize is a method which is automatically called by the garbage collector to dispose the no longer needed objects 
## Q6: What are the different types of classes in C#?
### Static class:
1. In C#, a **static class** is a class that cannot be instantiated, meaning you cannot create an object from it. All members of a static class must also be static. It's typically used to group related methods that don’t act on instance data and are shared across the application.
### Key Characteristics of a Static Class:
*   It **cannot be instantiated**.
*   It **cannot have constructors** (except static constructors).
*   **All members must be static** (methods, fields, properties, etc.).
*   It's sealed, meaning it cannot be inherited.
Static classes are often used to hold utility methods, such as mathematical calculations, logging, or configuration helpers.
### Static constructor: "Static constructor is called once when the class is first used."
### Sealed Class
1. Sealed Class: In most cases, we never intend this class to be able to be inherited by other classes, we'll get a significant performance boost by adding sealed keyword in a class not inherited by other classes. By sealing the class, the JIT compiler can directly know the object's actual data type. Meanwhile, for unsealed classes, JIT needs to check whether the class has subclasses or not.
### Abstract Classes  
1. Provide a base class with some implementation and abstract methods that derived classes must implement.
2. Use an **abstract class** when there is shared behavior among subclasses, but use an **interface** when different classes need to follow the same contract, without enforcing shared behavior.
### Partial classes
1. Allow the splitting of a class definition across multiple files.
### Generic classes
1. Allow the definition of classes with placeholders for the type of its fields, methods, parameters, etc.
## Q7: Can Abstract class be Sealed or Static in C#?
1. NO. Abstract class are used for inheritance, but sealed and static both will not allow the class to be inherited.
## Q8: Can you create an instance of an Abstract class or an Interface?
1. No. Abstract class and interface purpose is to act as base class via inheritance. Their object creation is not possible.
## Q9: What is an Interface?
1. Define a contract that classes must follow but does not provide any implementation.
2. In C# 8 and later, default methods in interfaces (also known as default interface methods) allow you to provide a default implementation for methods directly in the interface. This was a significant change, as prior to C# 8, interfaces could only contain method signatures (i.e., no implementation).

**Backward Compatibility**: If you want to extend an interface by adding new methods without breaking the existing implementations that already implement the interface.
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
**Shared Behavior Across Implementations**: If there is some common functionality that multiple implementations of the interface will share, you can provide that behavior in the default method, avoiding code duplication.
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
**When to Avoid Default Methods?**
1. Over-complicating interfaces: If you add too many default methods, interfaces can become bloated and harder to understand.
2. Increased coupling: Default methods can sometimes introduce coupling between the interface and its implementations, which can make future changes more difficult.
3. Breaking SOLID principles: If you're not careful, default methods can violate principles like Single Responsibility Principle (SRP) by adding too much behavior to an interface that should focus solely on abstraction.
## Q9.1: What is the difference between an abstract class and an interface?
In C#, both abstract classes and interfaces are types that enable polymorphism, allowing objects of different classes to be treated as objects of a common super class. However, they serve different purposes and have different rules.
1. **Abstract Classes**: Provide a base class with some implementation and abstract methods that derived classes must implement.
2. **Interfaces**: Define a contract that classes must follow but do not provide any implementation.

**Interview Tip**: Use an abstract class when there is shared behavior among subclasses, but use an interface when different classes need to follow the same contract, without enforcing shared behavior.
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
### Abstract Class:
1. Can contain implementation of methods, properties, fields, or events.
2. Can have access modifiers (public, protected, etc.).
3. A class can inherit from only one abstract class (single inheritance).
4. Can contain constructors.
5. Used when different implementations of objects have common methods or properties that can share a common implementation.
### Interface:
1. Cannot contain implementations, only declarations of methods, properties, events, or indexers.
2. Members of an interface are implicitly public.
3. A class or struct can implement multiple interfaces (multiple inheritance).
4. Cannot contain fields or constructors.
5. Used to define a contract for classes without imposing inheritance hierarchies.
## Q10: What is an encapsulation?
### Encapsulation:
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
## Q11: Explain polymorphism and its types in C#.?
### Polymorphism:
1. Polymorphism is a core concept in object-oriented programming (OOP) that allows objects to be treated as instances of their parent class rather than their actual derived class.  
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
**What is Polymorphism, and how does it improve code flexibility?**
1. Polymorphism allows one interface to be used for different types. It can be achieved through method overriding or method overloading.

**How does method overriding support polymorphism in C#?**
1. Method overriding allows a derived class to provide a specific implementation of a method that is already defined in the base class. This is a key feature of runtime polymorphism.
## Q12: Can you explain the difference between method overloading and method overriding with examples?
### Method Overloading: 
1. Same method name but different parameters (compile-time polymorphism).
```c#
public class Calculator
{
    public int Add(int a, int b) => a + b;
    public int Add(int a, int b, int c) => a + b + c;
}
```
### Method Overriding: 
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
## Q13: What is the default behavior of C# language in term of dispatching??
### Static Dispatch
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
### Explanation:
Compile-Time Binding: In this example, even though animal holds a Dog object, the method Speak() from the Animal class is invoked. This happens because the declared type of animal is Animal, and the method Speak() is not virtual. Therefore, static dispatch is used, and the decision to call the Animal.Speak() method is made at compile time.

### Virtual Methods (Dynamic Dispatch)
To enable dynamic dispatch (also known as late binding or runtime polymorphism), you need to mark the method in the base class as virtual and override it in the derived class using the override keyword. This instructs the C# runtime to make method dispatch decisions based on the runtime type of the object, rather than its compile-time type.
#### Example of Dynamic Dispatch (Late Binding)
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
### Explanation:
Runtime Binding: In this example, the Speak() method is marked as virtual in the Animal class and overridden in the Dog class. When animal.Speak() is called, the decision is made at runtime, based on the actual type of the object (Dog in this case), not the declared type. This is known as dynamic dispatch or late binding.
##### In short, the default behavior in C# is static dispatch, and to enable dynamic dispatch, you use virtual methods.
## Q14: Imagine two classes with inheritence hierarchy below.
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
### Explanation:
Gives Exception.

1. When you create an instance of ChildClass, the constructor of the base class (BaseClass) is called first, because the constructor of the base class is always called before the constructor of the derived class in C#.
**BaseClass constructor**:
Inside the BaseClass constructor, the Print() method is called:
```c#
public BaseClass()
{
  Print(); // Calls the overridden method in ChildClass
}
```
**Virtual method call:**
C# uses dynamic dispatch for virtual methods, meaning the overridden method in ChildClass is called even though it's invoked from the BaseClass constructor. Therefore, Print() in ChildClass is called.

**ChildClass Print() method:**
In ChildClass, the Print() method tries to access the _declaration field, but at this point, the ChildClass constructor hasn't been executed yet. The _declaration field is initialized inside the ChildClass constructor:
```c#
public ChildClass()
{
  _declaration = "I am a child!";
}
```
Since the ChildClass constructor hasn't finished running when Print() is called, the _declaration field is still null (its default value for reference types).

**Resulting output:**
When Print() in ChildClass is called, it tries to execute this line:
```c#
Console.WriteLine(_declaration.ToLower());
```
- Since _declaration is null, calling .ToLower() on a null string would result in a NullReferenceException. However, to avoid that, let's update the explanation based on the current code logic:
- Since the overridden method is called, but the field _declaration has not been initialized yet (because the child class constructor hasn't run), this would throw a NullReferenceException. The final behavior would be that you'd see "From Base Class" printed first, and then an exception would be thrown.

**Fixing the Issue:**
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
**o/p**: From Child Class

**note**: if i called Initilaize() from constructor in also throw exception
### Explanation of Why It Causes Issues
Object Construction Process:
- When you create an instance of a derived class, the base class constructor executes first.
- At this point, the derived class has not yet completed its constructor, meaning that any fields or properties in the derived class may not be initialized.
- If the base class calls a method (even if it's a non-virtual method), and that method uses any members from the derived class, it can lead to a NullReferenceException.
## Q15: Imagine two classes blew. Customer class is child of User class. What will print in the console output?
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
### Explanation
It will print "Base User Name"
The Problem here is that we are violating liskov principle and base class will hide the child class implementation.
In the code provided, you're defining the UserName property in both the base class User and the derived class Customer. This violates the Liskov Substitution Principle (LSP) because the base class User and the derived class Customer don't behave consistently when substituted for one another. Additionally, the derived class's property is hiding the base class property instead of overriding it. This can lead to confusion and unexpected behavior.
### Key Concepts
**Liskov Substitution Principle (LSP)**: The Liskov Substitution Principle states that objects of a derived class must be able to replace objects of the base class without affecting the correctness of the program. In other words, a subclass should behave in a way that is consistent with the expectations from the base class.
Liskov Substitution Principle (LSP), which says that a derived class (Customer) should be able to replace its base class (User) without breaking the behavior.

**Inheritance**: The Customer class inherits from the User class, which means it gets all the properties and methods of the User class.

**Property Hiding (Shadowing)**: In the Customer class, the UserName property is redefined using the new keyword (implicitly, since it's not marked virtual in the base class), which means it hides the UserName property from the User class. This is not polymorphism; it’s property hiding or shadowing.

1. Here, the variable user is of type User, but it holds an instance of Customer. This means you are creating an object of Customer, but referring to it as a User.
2. **Property Access**: When you access user.UserName, you are accessing the UserName property from the User class, not from the Customer class.
    - Why? This is because property hiding (not overriding) is at play here. Since the type of the variable (user) is User, it will access the UserName property defined in the User class, which has the default       value "Base User Name". The Customer's version of UserName is not considered because it’s hidden, not overridden.
3. **Output**: The value of user.UserName will be "Base User Name", because the UserName property from the User class is accessed.

**Why Not "Customer User Name"?**

For the Customer's UserName to be used polymorphically, you would need to use inheritance with polymorphism by marking the base property as virtual and the derived property as override. Here’s how you would do that:
**Using Virtual and Override for Polymorphism**

If you want the Customer class to properly override the UserName property in a polymorphic way, you can use the virtual and override keywords:
```c#
User user = new Customer();
Console.WriteLine(user.UserName); // Output: Customer User Name

public class User
{
    public string UserName { get; set; } = "user: aamir";
}

public class Customer : User
{
    public string UserName { get; set; } = "customer: aamir";
}
```
**Explanation of the Updated Code:**
- virtual in the User class: This allows the property to be overridden in derived classes.
- override in the Customer class: This provides a new implementation of the UserName property in the Customer class.

Now, when you create a Customer object and reference it as a User, the overridden UserName property from the Customer class will be used, and the output will be "Customer User Name".
## Q16: Can you explain OOPS & C# - Inheritance, Abstraction, Encapsulation & Polymorphism?
### Inheritance
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
### Abstraction
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
### Encapsulation
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
### Polymorphism:
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
### Inheritance
1. In C#, inheritance allows a class (called a derived class) to inherit members (fields, methods, properties) from another class (called a base class). This enables code reuse and polymorphism.
### 1. Single Inheritance
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
### Explanation:
Manager class inherits from Employee. Manager can access Work() method from Employee without redefining it.
### 2. Multilevel Inheritance
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
### Explanation:
Manager inherits from Employee, and Employee inherits from Person. Manager can access members from both Employee and Person.
### 3. Hierarchical Inheritance
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
### Explanation:
Multiple derived classes (Employee, Contractor, Intern) inherit from the single base class Person. Each subclass can have its own methods but still share common functionality (like Introduce() from Person).
### 4. Multiple Inheritance (Not Supported Directly in C#)
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
### Explanation:
Employee implements both IWorker and IManager interfaces, achieving the effect of multiple inheritance by inheriting behavior from both.
### 5. Hybrid Inheritance
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
### Explanation:
Manager class inherits from Employee (multilevel inheritance) and also implements IReportable interface. This is a mix of inheritance and interface implementation (hybrid inheritance).
### What is the difference between single and multilevel inheritance?
#### Single Inheritance: A class derives from only one base class.
#### Multilevel Inheritance: A class derives from a derived class, forming a chain.
**Single Inheritance Example**:
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
**Multilevel Inheritance Example**:
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
## Q17: What are access modifiers in C#? How do they relate to encapsulation?
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
## Q18: In a real-life e-commerce application, you have a class called OrderManager responsible for creating, updating, and deleting orders as well as sending confirmation emails and updating inventory. How would you refactor this class to follow the Single Responsibility Principle?
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
## Q19: Imagine you’re working on a payment processing system with multiple payment methods, such as credit card, PayPal, and bank transfer. How would you design it to be easily extendable for adding new payment methods without modifying existing code?
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
## Q19: Imagine you have a payment processing system that handles different types of payment methods (e.g., credit card, PayPal, or bank transfer). You have a base class PaymentProcessor that defines common behavior for all payment types, and specific processors like CreditCardProcessor and PayPalProcessor inherit from this base class.
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
## Q20:  You have an IEmployee interface with methods like Work, AttendMeeting, TakeBreak, and ProcessPayroll. Why does this violate ISP, and how can you refactor it?
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
## Q21:  In a notification system, you have a NotificationService that sends emails and SMS messages directly by instantiating EmailSender and SmsSender classes within it. How can you refactor this to follow the Dependency Inversion Principle?
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

// Usage
var emailSender = new EmailNotificationSender();
var notificationService = new NotificationService(emailSender);
notificationService.Notify("Your order has been shipped.");
```
**Interview Tip**: Emphasize that the NotificationService depends on the INotificationSender interface (abstraction), not on specific implementations like EmailNotificationSender or SMSNotificationSender. This makes the system flexible—changing the notification method doesn’t affect the high-level module.
## Q22:  How do SOLID principles help in writing better code?
Adhering to SOLID principles, especially DIP, makes it easier to mock dependencies in unit tests. For instance, if a service depends on an interface instead of a concrete implementation, you can easily inject a mock or stub to test different scenarios without touching the actual implementation.
## Q22: What is the Dependency Inversion Principle (DIP), and how does it differ from Dependency Injection?
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
## Q23: What are extension methods and where would you use them?
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

### the"𝙩𝙝𝙞𝙨" keyword:

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
Why use 𝐞𝐱𝐭𝐞𝐧𝐬𝐢𝐨𝐧 𝐦𝐞𝐭𝐡𝐨𝐝𝐬?

👉Extension methods allow you to add new methods to existing types, including those you don't have control over (like built-in .NET types).

👉They can make your code more intuitive and readable by allowing you to chain method calls in a natural way.

👉You can add methods to interfaces without breaking existing implementations.

👉Instead of creating utility classes with static methods, you can use extension methods for a more object-oriented feel.

## Q24: What are Value Types and Reference Types in C#?
In C#, data types are divided into two categories: Value Types and Reference Types. This distinction affects how values are stored and manipulated within memory.

**Value Types**: Store data directly and are allocated on the stack. This means that when you assign one value type to another, a direct copy of the value is created. Basic data types (int, double, bool, etc.) and structs are examples of value types. Operations on value types are generally faster due to stack allocation.

**Reference Types**: Store a reference (or pointer) to the actual data, which is allocated on the heap. When you assign one reference type to another, both refer to the same object in memory; changes made through one reference are reflected in the other. Classes, arrays, delegates, and strings are examples of reference types.

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
## Q24: What are delegates and how are they used in C#?
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
**Without violating OCP**  
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
2. **Event Handling**: Delegates are used to define event signatures, and events allow methods to subscribe to notifications when specific actions occur (e.g., button clicks, download start).  
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
4. **Callback Methods**: Delegates allow methods to be passed as parameters to handle tasks (e.g., download completion) asynchronously.  
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

> 13. Describe what LINQ is and give an example of where it might be used.

> 14. What is the difference between an abstract class and an interface?

1.**Abstract Classes**: Provide a base class with some implementation and abstract methods that derived classes must implement.

**Interfaces**: Define a contract that classes must follow but do not provide any implementation.  
  
**Interview Tip**: Use an **abstract class** when there is shared behavior among subclasses, but use an **interface** when different classes need to follow the same contract, without enforcing shared behavior.  
  
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

2.  Q: Only abstract methods (unless default methods in C# 8+) when it recommended to use default methods in interfaces with examples?  
      
    In C# 8 and later, **default methods in interfaces** (also known as **default interface methods**) allow you to provide a **default implementation** for methods directly in the interface. This was a significant change, as prior to C# 8, interfaces could only contain method signatures (i.e., no implementation).  
      
    **Backward Compatibility**: If you want to extend an interface by adding new methods without breaking the existing implementations that already implement the interface.  
      
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
    
      
    Shared Behavior Across Implementations: If there is some common functionality that multiple implementations of the interface will share, you can provide that behavior in the default method, avoiding code duplication.  
      
    public interface IDiscountCalculator
    
    {
    
    decimal CalculateDiscount(decimal totalAmount);
    
    // Default method to calculate seasonal discounts
    
    decimal ApplySeasonalDiscount(decimal totalAmount)
    
    {
    
    return totalAmount \* 0.90m; // 10% off as a default seasonal discount
    
    }
    
    }
    
    public class HolidayDiscount : IDiscountCalculator
    
    {
    
    public decimal CalculateDiscount(decimal totalAmount)
    
    {
    
    // Specific discount calculation for holidays
    
    return ApplySeasonalDiscount(totalAmount - 20); // Additional $20 off
    
    }
    
    }
    
    public class RegularDiscount : IDiscountCalculator
    
    {
    
    public decimal CalculateDiscount(decimal totalAmount)
    
    {
    
    // Uses the default seasonal discount provided in the interface
    
    return ApplySeasonalDiscount(totalAmount);
    
    }
    
    }
    
3.  When to Avoid Default Methods?  
    **Over-complicating interfaces**: If you add too many default methods, interfaces can become bloated and harder to understand.
    
    **Increased coupling**: Default methods can sometimes introduce coupling between the interface and its implementations, which can make future changes more difficult.
    
    **Breaking SOLID principles**: If you're not careful, default methods can violate principles like **Single Responsibility Principle (SRP)** by adding too much behavior to an interface that should focus solely on abstraction.

> 15. How do you manage memory in .NET applications?

> 16. Explain the concept of threading in .NET.

### 1\. **What is a race condition, and how can it occur in a multi-threaded .NET application?**

*   **Follow-up**: Imagine you are building an e-commerce application where multiple users can update the stock of products. How would a race condition manifest in this scenario, and how can you avoid it?  
      
    **Answer Guide**:
    
    *   A race condition occurs when multiple threads access shared data simultaneously, and the outcome depends on the timing of thread execution. In an e-commerce scenario, two users could try to update the stock of a product at the same time, potentially causing incorrect data to be stored.
        
    *   Solution: Use locking mechanisms like `lock`, `Mutex`, or `Semaphore` to ensure only one thread can access the shared resource at a time. Alternatively, use high-level concurrency classes like `ConcurrentDictionary` or atomic operations like `Interlocked`.  
          
          
        **Example 1: Race Condition Demonstration**
        
        Let's consider an example where two users (threads) are trying to update the stock of a product simultaneously without synchronization.
        
        #### Code Example Without Proper Synchronization:  
          
        using System;
        
        using System.Threading;
        
        class Program
        
        {
        
        static int stock = 100;
        
        static void Main(string\[\] args)
        
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
        
          
        Output (Example) — Race Condition:  
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
        
        using System;
        
        using System.Threading;
        
        class Program
        
        {
        
        static int stock = 100;
        
        static readonly object \_lockObject = new object();
        
        static void Main(string\[\] args)
        
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
        
        lock (\_lockObject)
        
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
        
          
        **Explanation:**
        
        1.  `lock (_lockObject)`: Ensures that only one thread at a time can enter the critical section (reading and updating the stock).
            
        2.  The `_lockObject` ensures that other threads must wait until the first thread finishes its update before they can access the `stock`.  
              
            Output — With Synchronization:  
            User 1 sees stock: 100
            
            User 1 updates stock to: 90
            
            User 2 sees stock: 90
            
            User 2 updates stock to: 80
            
            Final stock: 80
            

### **2\. What is a deadlock, and how can it occur in a .NET application?**

**Follow-up**: Suppose you are developing a banking application where transfers between accounts can happen in parallel. How might a deadlock occur during a funds transfer between two accounts, and how can you prevent it?  
  
**Answer Guide**:

*   A deadlock occurs when two or more threads are blocked indefinitely, each waiting for a resource held by the other.
    
*   In the banking scenario, Thread A may lock Account 1 and wait for Account 2, while Thread B locks Account 2 and waits for Account 1, causing a deadlock.
    
*   Solution: Use a consistent locking order (e.g., always lock Account 1 before Account 2), or use `Monitor.TryEnter` with a timeout to avoid deadlocks.
    

Example 1: Deadlock Code  
using System;

using System.Threading;

class Program

{

static object account1Lock = new object();

static object account2Lock = new object();

static void Main(string\[\] args)

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

  
**Explanation:**

1.  **Two threads (**`Thread A` **and** `Thread B`**)**:
    
    *   **Thread A** tries to lock `account1Lock` first, then `account2Lock`.
        
    *   **Thread B** tries to lock `account2Lock` first, then `account1Lock`.
        
2.  **Deadlock scenario**:
    
    *   **Thread A** locks the first account (`account1Lock`), but then waits for the second account (`account2Lock`).
        
    *   **Thread B** locks the second account (`account2Lock`), but then waits for the first account (`account1Lock`).
        
3.  **Thread.Sleep(100)** simulates some delay to increase the likelihood of encountering the deadlock.
    

Visualizing the Output:  
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
  
using System;

using System.Threading;

class Program

{

static object account1Lock = new object();

static object account2Lock = new object();

static void Main(string\[\] args)

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
  
using System;

using System.Threading;

class Program

{

static object account1Lock = new object();

static object account2Lock = new object();

static void Main(string\[\] args)

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
      
    

### **3\. How would you avoid deadlocks when using** `async` **and** `await` **in .NET?**

*   **Follow-up**: In an order processing system, you have asynchronous calls to an external payment gateway followed by calls to update the order status in the database. How would you structure your code to avoid deadlocks?
    

**Answer Guide**:

*   Deadlocks in async code often happen when you mix `async` code with blocking calls (e.g., `Task.Wait()` or `.Result`), which can block the UI thread and cause a deadlock.
    
*   Solution: Always await asynchronous calls and avoid blocking the thread with `.Result` or `Wait()`. Ensure that all calls in the chain are asynchronous.
    

4\. Can you explain the difference between `lock` and `Monitor` in .NET? When would you use one over the other?  
**Follow-up**: Imagine a financial application where transactions need to be locked before processing. Would you use `lock` or `Monitor`, and why?  
  
**Answer Guide**:

*   The `lock` keyword is a shorthand for `Monitor.Enter` and `Monitor.Exit`. It is simpler to use but less flexible. `Monitor` allows more fine-grained control, such as using `Monitor.TryEnter` to attempt to acquire a lock without blocking indefinitely.
    
*   For a financial application, you might prefer `Monitor.TryEnter` with a timeout to avoid deadlocks and provide better control over how long the system waits for a lock.
    

5\. **Mutex (Mutual Exclusion)**

*   A **mutex** is a locking mechanism that ensures that only one thread can access a shared resource at a time. It is like having a key to a door: only one person (thread) can hold the key (mutex) and access the room (shared resource) at any given time. The key must be released (unlocked) for others to use it.  
      
    Imagine a single bathroom key that two people (threads) are trying to use. Only one person can enter the bathroom at a time (mutual exclusion). The second person waits until the first person finishes and hands over the key (releases the mutex).  
    

6\. **Semaphore**

*   **Semaphore** is used to allow multiple threads to access a limited number of resources concurrently, making it suitable for scenarios where multiple instances of a resource can be shared up to a certain capacity.
    
*   A **semaphore** controls access to a resource by allowing multiple threads to access a limited number of instances of a resource simultaneously.  
      
    **Real-World Example of Semaphore: Parking Lot**
    
    Imagine a parking lot that has 3 parking spaces. Three cars can park at the same time. If all spaces are occupied, new cars have to wait until a spot becomes available.  
      
    **Real-World Analogy:**
    
    *   Think of a parking lot with only 3 spaces (semaphore capacity = 3). As soon as all spots are taken, new cars must wait for one of the cars to leave before parking. Each car that enters reduces the available spaces, and when a car leaves, a new spot opens up for another car.

> 17. What is async/await and how does it work?

> 17. What is async/await and how does it work?

1.  `async`: The method is marked as asynchronous, which tells the compiler to generate a state machine.  
    
2.  Here, `Task.Delay(5000)` simulates a 5-second file download. While the download is happening, the main thread (which handles UI) **doesn’t block**—the user can still click on buttons, scroll, or interact with the app.
    
    Without `await`, the UI would freeze until the file download is complete, which would result in a poor user experience.  
      
    Here’s the complete explanation again with a real-world coffee shop example:
    
    1.  **The** `await` **keyword is applied to a task**: Just like ordering coffee and then waiting for it, you’re asking the method to "wait" at a certain point (`await` the task) without blocking the entire system.
        
    2.  **Indicating that the method should pause until the awaited task completes**: The method pauses at the `await`, similar to you waiting for your coffee, but the entire coffee shop (the system) doesn't pause—just that specific task.
        
    3.  **Allowing other operations to run concurrently**: While the method is paused waiting for the task, other methods and tasks can continue running (other customers are getting served while you wait for your coffee).
        
    4.  **Without blocking the main thread**: If this were an app, the user interface (UI) wouldn’t freeze. In the real world, the coffee shop doesn’t shut down while your coffee is being made; it continues working efficiently.  
          
        public async Task ServeMultipleCustomersAsync()
        
        {
        
        // Multiple customers ordering coffee concurrently
        
        var customer1 = ServeCoffeeAsync();
        
        var customer2 = ServeCoffeeAsync();
        
        // Await both coffees to be ready concurrently
        
        await Task.WhenAll(customer1, customer2);
        
        Console.WriteLine("Both customers served!");
        
        }
        
        In this example, **two customers** order coffee at the same time. The `Task.WhenAll` ensures both orders are processed concurrently. No one customer waits for the other to finish. Similarly, in a real-world coffee shop, multiple orders can be handled in parallel, without one customer blocking another’s order.
        
3.  **State Machine**: A state machine is automatically created when you write asynchronous code using `async` and `await`. It controls how the method moves between different "states" of execution.  
      
    In asynchronous programming, particularly when using `async` and `await` in C#, the compiler transforms the code into a **state machine**. A state machine is a programming construct that controls the flow of execution through a series of states. Each state represents a particular point in the execution of the method, and the state machine keeps track of where to continue when a task resumes after an `await`.

> 18. Describe the Entity Framework and its advantages.

1.  Explain the difference between `DbContext` and `DbSet`.  
      
    **Answer:**
    
    *   **DbContext:** This is a class that manages the database connection and is used to configure and interact with the database using EF Core. It is responsible for querying and saving data.
        
    *   **DbSet:** This is a class that represents a collection of entities of a specific type that can be queried from the database. Each entity type that you want to include in your model must have a `DbSet` property in your `DbContext` class.
        
2.  What is the purpose of the `OnModelCreating` method in EF Core?
    
    **Answer:**  
    The `OnModelCreating` method is used to configure the model using the ModelBuilder API. It is where you can define schema details, relationships, table mappings, and more. This method is overridden in your `DbContext` class to configure entity properties and relationships.
    
3.  How can you configure a many-to-many relationship in EF Core?
    
    **Answer:**  
    In EF Core, a many-to-many relationship can be configured using a join entity that contains the foreign keys to the two related entities. Here's an example:  
    
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
    
4.  What is the difference between `AsNoTracking` and `AsTracking` in EF Core?
    
    **Answer:**
    
    *   `AsNoTracking`**:** This method returns entities without tracking them in the `DbContext`. This is useful for read-only scenarios where the performance can be improved by not tracking the entities.
        
    *   `AsTracking`**:** This is the default behavior. It tracks the entities returned by the query, meaning any changes to these entities are monitored by the `DbContext` and can be persisted to the database during `SaveChanges`.
        
5.  What are global query filters in EF Core?
    
    **Answer:**  
    Global query filters are filters applied to all queries executed against a particular entity type. These filters are defined in the `OnModelCreating` method and are useful for scenarios like soft deletes or multi-tenancy. Example:  
    modelBuilder.Entity<Product>().HasQueryFilter(p => !p.IsDeleted);
    
6.  How do you handle concurrency conflicts in EF Core?
    
    **Answer:**  
    Concurrency conflicts occur when multiple processes try to update the same data at the same time. EF Core provides optimistic concurrency control. You can configure a property to be a concurrency token:  
      
    public class Product
    
    {
    
    public int Id { get; set; }
    
    public string Name { get; set; }
    
    \[ConcurrencyCheck\]
    
    public int Quantity { get; set; }
    
    }
    
    When a conflict is detected during `SaveChanges`, an exception (`DbUpdateConcurrencyException`) is thrown, which can be handled to resolve the conflict:  
      
    try
    
    {
    
    context.SaveChanges();
    
    }
    
    catch (DbUpdateConcurrencyException ex)
    
    {
    
    // Handle the concurrency conflict
    
    }  
    
7.  What is the difference between `Include` and `ThenInclude` in EF Core?  
    `Include`**:** This method is used to include related entities in the query results. It is used for eager loading.  
      
    var orders = context.Orders.Include(o => o.Customer).ToList();
    
      
    `ThenInclude`**:** This method is used to include related entities after the first level of inclusion. It is used for further levels of eager loading.  
      
    var orders = context.Orders
    
    .Include(o => o.Customer)
    
    .ThenInclude(c => c.Address)
    
    .ToList();
    
8.  Describe a scenario where you need to use `Include` and `ThenInclude` for eager loading with nested relationships.
    
    **Answer:**  
    Consider a scenario with entities `Order`, `Customer`, and `Address`, where an order has a customer and a customer has an address. To eagerly load the orders along with their customers and the customers' addresses:  
      
    var orders = context.Orders
    
    .Include(o => o.Customer)
    
    .ThenInclude(c => c.Address)
    
    .ToList();
    
9.  How do you handle transactions in EF Core?
    
    **Answer:**  
    EF Core supports transactions implicitly when `SaveChanges` is called. For more complex scenarios requiring explicit transaction control, you can use `DbContext.Database.BeginTransaction`, `Commit`, and `Rollback`:  
      
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
    
10.  How can you optimize performance with EF Core?  
    **Answer:**
    
    *   **Use** `AsNoTracking` **for read-only queries:** This avoids the overhead of change tracking.
        
    *   **Batch commands:** Reduce the number of database round-trips by batching multiple commands.
        
    *   **Use compiled queries:** For queries that are executed frequently, use compiled queries to avoid query recompilation
        
    *   **Avoid lazy loading:** Prefer eager or explicit loading to avoid the "N+1 query problem."
        
    *   **Use projections:** Retrieve only the required fields using `Select`  
        
11.  How do you handle complex querying scenarios using raw SQL in EF Core?  
    **Answer:**  
    For complex queries that cannot be expressed using LINQ, use raw SQL queries:  
      
    var customers = context.Customers.FromSqlRaw("SELECT \* FROM Customers WHERE IsActive = 1").ToList();
    
    You can also use raw SQL for non-query commands:  
      
    context.Database.ExecuteSqlRaw("UPDATE Customers SET IsActive = 0 WHERE Id = {0}", customerId);

> 19. What are extension methods and where would you use them?

1.  `words[i][0]`: This accesses the **first character** of the word at index `i`. For example, if `words[i]` is `"hello"`, `words[i][0]` would be `'h'`.
    
2.    
    public class Product
    
    {
    
    public string Name { get; set; }
    
    public decimal Price { get; set; }
    
    }
    
    ### Adding Extension Method
    
    Here's an extension method for the `Product` class to calculate a discount:  
      
    public static class ProductExtensions
    
    {
    
    // This method calculates the price after applying the discount.
    
    public static decimal CalculateDiscount(this Product product, decimal discountPercentage)
    
    {
    
    if (discountPercentage < 0 || discountPercentage > 100)
    
    throw new ArgumentOutOfRangeException("Discount percentage must be between 0 and 100.");
    
    return product.Price - (product.Price \* (discountPercentage / 100));
    
    }
    
    }
    
    Product product = new Product { Name = "Laptop", Price = 1000 };
    
    decimal discountedPrice = product.CalculateDiscount(10); // 10% discount
    
    Console.WriteLine($"Discounted price: {discountedPrice}");
    
      
    Why Use Extension Methods Here?
    
    *   **Cleaner Syntax**: Instead of creating a separate `DiscountCalculator` class or utility function, you can invoke `CalculateDiscount` directly on the `Product` object.
        
    *   **Encapsulation**: The method logic is encapsulated, and the `Product` class remains untouched. You don't have to modify or inherit from `Product` to add this functionality.

> 20. How do you handle exceptions in a method that returns a Task?

1.  Task: In C#, a `Task` represents an asynchronous operation. It's part of the **Task-based Asynchronous Pattern (TAP)** and is used to handle asynchronous programming, allowing code to execute without blocking the main thread.  
      
    Key Task Types
    
    *   `Task`: Represents an asynchronous operation that **does not return a value**. It is analogous to `void` in synchronous methods.
        
    *   `Task<TResult>`: Represents an asynchronous operation that **returns a result** (of type `TResult`). It is analogous to `TResult` return types in synchronous methods.
        
    
    ### Why Use `Task`?
    
    1.  **Avoid Blocking the UI**: In applications with a UI (e.g., WPF or Windows Forms), using `Task` ensures the UI thread remains responsive while long-running tasks (like file I/O or database queries) execute.
        
    2.  **Scalability**: In server applications, `Task` helps in creating more scalable systems by enabling asynchronous I/O operations (e.g., handling multiple HTTP requests simultaneously without blocking threads).  
        
2.  Task States
    
    A `Task` can have different states, including:
    
    *   **Created**: The task is created but not yet running.
        
    *   **Running**: The task is currently running.
        
    *   **Completed**: The task has finished running.
        
    *   **Faulted**: The task encountered an error.
        
    *   **Canceled**: The task was canceled.

> 21. What is reflection in .NET and how would you use it?

> 22. Can you explain the concept of middleware in ASP.NET Core?

1.  Each middleware component can:
    
    1.  **Process incoming requests**.
        
    2.  **Modify the request or response**.
        
    3.  **Call the next middleware in the pipeline**, or short-circuit the pipeline to return a response immediately.
        
        If a middleware component short-circuits, it prevents further middleware from executing (e.g., if authentication fails, you might return an error immediately).
        
2.  Real-World Example: Logging Incoming Requests
    
    Imagine you're building an **e-commerce application**. You want to log every incoming request to the system so that you can monitor traffic or debug issues.
    
    You can create a custom middleware to handle this.
    
    ### Step-by-Step: Implementing a Custom Middleware
    
    #### 1\. Creating the Middleware  
      
    public class RequestLoggingMiddleware
    
    {
    
    private readonly RequestDelegate \_next;
    
    public RequestLoggingMiddleware(RequestDelegate next)
    
    {
    
    \_next = next;
    
    }
    
    public async Task InvokeAsync(HttpContext context)
    
    {
    
    // Log the incoming request
    
    Console.WriteLine($"Incoming Request: {context.Request.Method} {context.Request.Path}");
    
    // Call the next middleware in the pipeline
    
    await \_next(context);
    
    // Log after the request has been handled
    
    Console.WriteLine($"Response Status Code: {context.Response.StatusCode}");
    
    }
    
    }
    
    2\. Registering the Middleware in the Pipeline  
      
    app.UseMiddleware<RequestLoggingMiddleware>(); // Register custom logging middleware app.UseRouting(); // Sets up routing app.UseAuthentication(); // Handles authentication app.UseEndpoints(endpoints => { endpoints.MapControllers(); // Maps API controllers });
    
    #### 3\. Testing the Middleware
    
    Now, whenever you make an HTTP request (e.g., to `/api/products`), you’ll see the following output in your console:  
      
    Incoming Request: GET /api/products
    
    Response Status Code: 200
    
3.    
    Middleware Order
    
    The order of middleware in the pipeline is critical. For example, authentication middleware must run before authorization middleware because a user must be authenticated before checking if they have the right permissions.  
      
    app.UseAuthentication(); // Authenticate the user
    
    app.UseAuthorization(); // Check if the user is allowed to access the resource
    
      
    
4.  Sgort-Circut: In **ASP.NET Core**, **short-circuiting** refers to a scenario where a middleware in the request pipeline **stops the flow** and prevents further middleware components from executing. This can occur deliberately (by design) or unintentionally.  
      
    When Does Short-Circuiting Happen?
    
    Short-circuiting can happen when:
    
    1.  **A middleware decides to handle the request completely** without calling the next middleware in the pipeline (by **not** calling `await _next(context);`).
        
    2.  **A condition is met** that causes a middleware to return a response immediately, such as an authentication failure or a validation error.
        
    
    ### Example of Short-Circuiting
    
    Consider the following middleware pipeline:  
      
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
    
      
    Scenario 1: Short-circuiting with Authentication
    
    If a request comes in and the user is not authenticated, `app.UseAuthentication()` middleware might **short-circuit** the pipeline by returning an unauthorized (401) response, without passing the request further down the pipeline (e.g., `app.UseAuthorization()` or `app.UseEndpoints()` won’t execute).
    
    Here's an example where authentication middleware might short-circuit:  
      
    public class AuthenticationMiddleware
    
    {
    
    private readonly RequestDelegate \_next;
    
    public AuthenticationMiddleware(RequestDelegate next)
    
    {
    
    \_next = next;
    
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
    
    return; // No call to \_next(context), so no further middleware is invoked
    
    }
    
    await \_next(context); // Pass the request to the next middleware
    
    }
    
    }
    
      
    In this case:
    
    *   If the user is not authenticated, the pipeline is **short-circuited**. The middleware returns a 401 response and **does not call** `_next(context)`, preventing further middleware (like routing, authorization, etc.) from running.
        
    *   If the user is authenticated, it proceeds to the next middleware by calling `_next(context)`.
        
    
    #### Scenario 2: Short-circuiting with Custom Middleware
    
    Let’s take another example where a custom logging middleware short-circuits the pipeline for certain requests:  
      
    public class RequestLoggingMiddleware
    
    {
    
    private readonly RequestDelegate \_next;
    
    public RequestLoggingMiddleware(RequestDelegate next)
    
    {
    
    \_next = next;
    
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
    
    await \_next(context); // Otherwise, pass the request to the next middleware
    
    }
    
    }  
      
    
5.  **What is CORS (Cross-Origin Resource Sharing)?**
    
    CORS (Cross-Origin Resource Sharing) is a security feature implemented by web browsers that **controls how web applications interact with resources from different origins** (domains).  
      
    CORS (Cross-Origin Resource Sharing) is a security feature implemented by browsers to prevent unauthorized cross-origin requests. It is important for security purposes, ensuring that web applications can't freely access resources from different origins without permission.
    
      
    // Define a CORS policy  
    services.AddCors(options => { options.AddPolicy("AllowSpecificOrigins", builder => { builder.WithOrigins("http://example.com") .AllowAnyHeader() .AllowAnyMethod(); }); });  
      
    // Apply the CORS policy globally app.UseCors("AllowSpecificOrigins");

> 23. Describe the Dependency Injection (DI) pattern and how it's implemented in .NET Core.

### **1\. Dependency Inversion Principle (DIP)**

#### **What is DIP?**

The **Dependency Inversion Principle (DIP)** is a design principle in the **SOLID** principles that says:

*   **High-level modules** (business logic) should not depend on **low-level modules** (details). Both should depend on **abstractions**.
    
*   **Abstractions should not depend on details**. Details (implementations) should depend on abstractions.
    

### **How is DIP applied in your code?**

#### **High-Level Module**:  
  
public class NotificationService

{

private readonly INotificationSender \_notificationSender;

public NotificationService(INotificationSender notificationSender)

{

\_notificationSender = notificationSender;

}

public void Notify(string message)

{

\_notificationSender.Send(message);

}

}

*   `NotificationService` is the **high-level module**. It’s responsible for notifying users with messages.
    
*   Instead of depending directly on low-level modules (like `EmailNotificationSender` or `SMSNotificationSender`), it depends on an **abstraction** (`INotificationSender`).
    

#### **Low-Level Modules**:  
  
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

*   `EmailNotificationSender` and `SMSNotificationSender` are **low-level modules** that implement the `INotificationSender` abstraction.
    
*   The **abstraction (**`INotificationSender`**) does not depend on the details** of how emails or SMS are sent. Instead, these implementations depend on the abstraction.
    

### **2\. Dependency Injection (DI)**

#### **What is DI?**

**Dependency Injection (DI)** is a design pattern where an object’s dependencies are provided to it from the outside, rather than the object creating its own dependencies. This makes the code more flexible, testable, and easier to maintain. This promotes loose coupling. The three types of DI are:

1.  **Constructor Injection**: Dependencies are provided through a class's constructor.
    
2.  **Property (Setter) Injection**: Dependencies are assigned via public properties.
    
3.  **Method Injection**: Dependencies are passed through a method when needed.
    

### **How is DI applied in your code?**

Look at the constructor in the **NotificationService** class:  
  
public NotificationService(INotificationSender notificationSender)

{

\_notificationSender = notificationSender;

}

  
  
Here, `NotificationService` **does not create an instance** of `EmailNotificationSender` or `SMSNotificationSender` by itself. Instead, **it receives an instance** (injected) from the outside.

#### **Usage Example**:  
  
var emailSender = new EmailNotificationSender(); // Create the dependency

var notificationService = new NotificationService(emailSender); // Inject the dependency

notificationService.Notify("Your order has been shipped.");

*   In this example, the `NotificationService` **receives** `emailSender` as a dependency when it’s created. This is **constructor-based dependency injection**.
    
*   You could easily swap out the dependency with an `SMSNotificationSender` instead of `EmailNotificationSender` without changing the `NotificationService` logic.
    

3.  **What is Inversion of Control (IoC), and how is it related to Dependency Injection?**
    
    *   **Answer**: IoC is a broader principle that says the control of object creation and the flow of a program should be inverted from traditional procedural code. Rather than a class controlling its dependencies, external code controls them. **Dependency Injection** is one specific way to implement IoC, by injecting dependencies into classes rather than allowing them to create their own.
        
4.  **Can you give a real-life example of IoC?**
    
    *   **Example**: In a web application, the framework (like ASP.NET Core) manages the lifecycle of controllers and services. You don't manually instantiate these components; instead, the framework injects them where needed using DI containers, inverting the control of object creation.
        
5.  **What is an IoC container, and why is it useful?**
    
    *   **Answer**: An IoC container is a framework that manages object creation and lifetime. It resolves dependencies automatically and injects them where needed. Examples include ASP.NET Core's built-in DI container, Autofac, and Ninject.
        
        *   **Why it's useful**: It simplifies dependency management by handling object creation, lifecycle management (e.g., singleton, scoped), and dependency resolution, making your code cleaner and easier to maintain.
            
6.  **What are the different types of lifetimes in IoC containers, and how do they affect object creation?**
    
    *   **Answer**: Common lifetimes in IoC containers include:
        
        1.  **Transient**: A new instance is created every time it's requested.
            
        2.  **Scoped**: A new instance is created per request (or scope), making it useful for web applications.
            
        3.  **Singleton**: A single instance is created and shared throughout the application’s lifetime.

> 24. What is the purpose of the .NET Standard?

1.  The purpose of **.NET Standard** is to create a **common set of APIs** that can be used across different .NET platforms (like .NET Core, .NET Framework, Xamarin, and others). It ensures **code compatibility** between these platforms, so you can write libraries that work everywhere without needing to rewrite code for each platform.  
      
    In essence, it helps **unify** the .NET ecosystem and makes code **more portable**.

> 25. Explain the differences between .NET Core, .NET Framework, and Xamarin.

> 30. How would you secure a web application in ASP.NET Core?

> 27. What are attributes in C# and how can they be used?

1.  Attributes don’t change the functionality of your code directly but can be used to modify how the code behaves at **runtime** or during **compile-time**.

> What is MVC (Model-View-Controller)?

> How do you perform validations in ASP.NET Core?

> How do you implement Web API versioning in ASP.NET Core?

> How do you mock dependencies in unit tests using .NET?

> Can you explain SOLID principles?

> What is Continuous Integration/Continuous Deployment (CI/CD) and how does it apply to .NET development?

> How do you ensure your C# code is secure?

> What are some common performance issues in .NET applications and how do you address them?

> Describe the Repository pattern and its benefits.

> How do you handle database migrations in Entity Framework?

> In C#, both abstract classes and interfaces are types that enable polymorphism, allowing objects of different classes to be treated as objects of a common super class. However, they serve different purposes and have different rules:


Answer:
Partial updates can introduce complexity in terms of validation and conflict resolution. With PATCH, the server needs to handle merging the changes into the existing resource, which can lead to issues like conflicting changes or data inconsistency.
Also, using PATCH requires more attention to how changes are applied, especially in concurrent environments where multiple users may update the resource at the same time.

# What is the difference between ThreadPool.QueueWorkItem and new Thread().Start()?

## Answer:
The ThreadPool.QueueUserWorkItem and new Thread().Start() methods in .NET are both used to create and manage threads, but they have different purposes, behaviors, and use cases.
- ThreadPool.QueueUserWorkItem: Adds work to a shared ThreadPool of pre-existing threads. Best for short-lived, frequent tasks that don’t need a dedicated thread (e.g., handling requests in a web server).
    ThreadPool.QueueUserWorkItem assigns the task to a thread from the shared pool.
    When the task finishes, the thread returns to the pool, ready for the next task

## Using ThreadPool.QueueUserWorkItem
```c#
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
- new Thread().Start(): Creates a dedicated thread for each task. Useful for long-running tasks where more control over the thread’s lifecycle is needed (e.g., background data analysis).

## Using new Thread().Start()
```c#
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
## Real-World Example:

- ThreadPool.QueueUserWorkItem: A web server processing incoming HTTP requests quickly without the overhead of creating/destroying threads.

- new Thread().Start(): A long-running task that performs real-time data processing, where you may want to control priority or manage its shutdown.

## Thread Management

- ThreadPool.QueueUserWorkItem: Managed by the .NET runtime. Threads are reused for efficiency, so creating/destroying threads isn't required.

- new Thread().Start(): Manual control over threads, which are created and destroyed as needed. Higher overhead and less efficient for frequent, short tasks.

## Example:
```c#
// ThreadPool - efficient for short tasks
ThreadPool.QueueUserWorkItem(_ => Console.WriteLine("Processing HTTP request"));
// new Thread - better for a dedicated, ongoing task
new Thread(() => Console.WriteLine("Long background task running")).Start();
```
## Performance & Overhead

- ThreadPool.QueueUserWorkItem: Lower overhead, as thread creation/destruction is avoided. Ideal for scalable applications.

- new Thread().Start(): Higher overhead as each thread is individually created and cleaned up. Use only when task requires dedicated, long-term attention.

## Example:

- ThreadPool: A bank’s server handling many user transactions quickly.

- new Thread: An analysis tool processing large datasets for hours.
