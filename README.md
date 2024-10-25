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
## Q7: Can Abstract class be Sealed or Static in C#?
1. NO. Abstract class are used for inheritance, but sealed and static both will not allow the class to be inherited.
## Q8: Can you create an instance of an Abstract class or an Interface?
1. No. Abstract class and interface purpose is to act as base class via inheritance. Their object creation is not possible.
## Q9: What is an Interface?
1. Define a contract that classes must follow but does not provide any implementation.
## Q10: What is an encapsulation?
### Encapsulation:
1. Encapsulation is the concept of restricting direct access to the internal state (fields) of an object and only exposing the necessary methods to interact with it. This ensures controlled access to the data and better security.  
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
## Q12: How does method overriding support polymorphism in C#??
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





> 12. What are delegates and how are they used in C#?

1.  A multicast delegate is a delegate that holds references to more than one method. When invoked, all methods in the invocation list are called sequentially.  
      
    **What is the difference between delegates and events in C#?**
    
    **Answer:** Delegates are function pointers, whereas events are special delegates that restrict direct access, allowing only subscription (`+=`) and unsubscription (`-=`).
    
    **Real-World Example:** In a real-world scenario like a stock monitoring system:
    
    *   A delegate allows anyone to invoke a notification method (which could be risky).
        
    *   An event restricts users from invoking notification methods directly, providing better encapsulation.  
          
        Why Use Delegates?
        
    
    *   **Loose Coupling**: The order processing logic doesn’t need to know the details of the notification system (Email or SMS). It only calls the delegate, making the system more flexible.
        
    *   **Extensibility**: In the future, if you want to add another notification method (e.g., a push notification), you can just add it to the delegate without changing the core logic of the order processing.  
          
        EmailNotifierV2 emailNotifierV2 = new();
        
    
    SmsNotifierV2 smsNotifierV2 = new();
    
    OrderProcessorV2 orderProcessorV2 = new();
    
    orderProcessorV2.NotifyV2Delegate += emailNotifierV2.SendMail;
    
    orderProcessorV2.NotifyV2Delegate += smsNotifierV2.SendSms;
    
    orderProcessorV2.ProcessOrder();
    
    Console.ReadLine();  
    public class Message
    
    {
    
    public string Content { get; set; } = string.Empty;
    
    }
    
    public class EmailMessage : Message
    
    {
    
    public string Subject { get; set; } = string.Empty;
    
    public string Attachment { get; set; } = string.Empty;
    
    }
    
    public delegate void NotifyV2(Message message);
    
    public class EmailNotifierV2
    
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
    
    public class SmsNotifierV2
    
    {
    
    public void SendSms(Message message)
    
    {
    
    Console.WriteLine($"Sms Sent: {message.Content}");
    
    }
    
    }
    
    public class OrderProcessorV2
    
    {
    
    public NotifyV2? NotifyV2Delegate { get; set; }
    
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
    
    var delegates = NotifyV2Delegate!.GetInvocationList();
    
    if (delegates is not null)
    
    {
    
    foreach (var notifier in delegates)
    
    {
    
    //The idea is to only invoke the appropriate notifier for each message type rather than invoking all subscribed methods for both messages.
    
    if (notifier.Target is SmsNotifierV2)
    
    notifier.DynamicInvoke(smsMessage);
    
    else if (notifier.Target is EmailNotifierV2)
    
    notifier.DynamicInvoke(emailMessage);
    
    }
    
    }
    
    //Here is an issue
    
    /\*invoking all subscribed methods for both messages\*/
    
    /\*NotifyV2Delegate?.Invoke(smsMessage);
    
    NotifyV2Delegate?.Invoke(emailMessage);\*/
    
    }
    
    }  
    
2.  Without violating OCP  
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
    
    private readonly List<INotify> \_notifies;
    
    public OrderProcessor(List<INotify> notifies)
    
    {
    
    \_notifies = notifies;
    
    }
    
    public void ProcessOrder()
    
    {
    
    Console.WriteLine("Order is beng processed...");
    
    foreach (var notifier in \_notifies)
    
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
    
3.  **Event Handling**: Delegates are used to define event signatures, and events allow methods to subscribe to notifications when specific actions occur (e.g., button clicks, download start).  
      
    public class Program
    
    {
    
    static void Main(string\[\] args)
    
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
    
      
    
4.  **Callback Methods**: Delegates allow methods to be passed as parameters to handle tasks (e.g., download completion) asynchronously.  
      
    public class Program
    
    {
    
    static void Main(string\[\] args)
    
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
