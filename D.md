# Dependency Inversion Principle (DIP)

High-level modules should not depend on low-level modules. Both should depend on abstractions 


##Concepts you should know 

### coupling : 
Coupling measures how dependent two modules/classes are on each other.

- loose coupling : 
 Modules know as little as possible about each other’s internal details.
 They interact through interfaces, abstractions, or events.
 Changes in one module have minimal impact on others.


- tight coupling :
-- One module directly depends on the concrete implementation of another.
-- Changes in one module force changes in others.


### cohesion : 
Cohesion measures how closely related the responsibilities of a single module/class are.

- High Cohesion
- A class/module does one well-defined thing (it is also related to SRP mentioned before )

- Example:
-- A UserRepository class only handles user database operations (no email sending and no order processing)

- Low Cohesion :
- A class/module does many unrelated things as a result it Hard to maintain and test.

Our concept is called Dependency Inversion, but it is done with Dependency Injection. You inject your interface or abstract and use this injected object to call any method you want 


Dependency Injection: DI is a technique (often used to implement DIP). 
### It is the way we provide the dependency (the interface implementation) to the class, we usually use 

- Constructor injection will be shown later in an example  


## example (before DIP)

We’ll simulate a payment processing system where initially the code is tightly coupled (DIP violation), and then we’ll fix it with Dependency Inversion Principle.


The Problem – DIP Violation
We have an EcommerceService that directly depends on concrete payment providers like PayPalPaymentProcessor and StripePaymentProcessor.
This means if we want to add any other method or even a small change in the payment API, we must edit EcommerceService — because of high coupling.  

here is the code 

```csharp
public class PayPalPaymentProcessor
{
    public void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing {amount} via PayPal.");
    }
}

public class StripePaymentProcessor
{
    public void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing {amount} via Stripe.");
    }
}

public class EcommerceService
{
    private PayPalPaymentProcessor _paypalProcessor;
    private StripePaymentProcessor _stripeProcessor;

    public EcommerceService()
    {
        _paypalProcessor = new PayPalPaymentProcessor();
        _stripeProcessor = new StripePaymentProcessor();
    }

    public void Checkout(decimal amount, string method)
    {
        if (method == "paypal")
            _paypalProcessor.ProcessPayment(amount);
        else if (method == "stripe")
            _stripeProcessor.ProcessPayment(amount);
    }
}

```

-the problems without DI
-- EcommerceService knows about every payment processor (tight coupling ).
-- also this is an OCP violation: Adding any new payment method we should modify EcommerceService.
-- Hard to test: Can’t mock payment services easily.
-- Not reusable in different contexts.



The Solution – Applying DIP
We:

Create an abstraction (IPaymentProcessor) high-level module will depend on .

Make all payment processors implement this abstraction.

Inject the processor into EcommerceService via Dependency Injection.

Now EcommerceService depends on abstractions, not concretions .
so we can use it normally without every where we want 

Code After DIP 


```csharp
// Abstraction
public interface IPaymentProcessor
{
    void ProcessPayment(decimal amount);
}

// Implementations
public class PayPalPaymentProcessor : IPaymentProcessor
{
    public void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing {amount} via PayPal.");
    }
}

public class StripePaymentProcessor : IPaymentProcessor
{
    public void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing {amount} via Stripe.");
    }
}

public class ApplePayPaymentProcessor : IPaymentProcessor
{
    public void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing {amount} via Apple Pay.");
    }
}

// High-level module
public class EcommerceService
{
    private readonly IPaymentProcessor _paymentProcessor;

    // Constructor Injection
    public EcommerceService(IPaymentProcessor paymentProcessor)
    {
        _paymentProcessor = paymentProcessor;
    }

    public void Checkout(decimal amount)
    {
        _paymentProcessor.ProcessPayment(amount);
    }
}

// Composition root
class Program
{
    static void Main()
    {
        // Choose which payment processor to use (could come from config/DI container)
        IPaymentProcessor paymentProcessor = new ApplePayPaymentProcessor();

        EcommerceService ecommerceService = new EcommerceService(paymentProcessor);
        ecommerceService.Checkout(250.00m);
    }
}

```
now with DIP if we want to add another payment method we will not touch ECommerceService we will only create another class that implement our abstract or interface 
