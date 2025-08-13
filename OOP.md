# OOP 

 ## OOP Fundamentals (The Big Four) 


## Encapsulation 

Encapsulation Hides the internal state and functionality of an object and only allows access through a public set of functions

What is Data Hiding in C#?
Data hiding or Information Hiding is a Process in which we hide internal data from outside the world. The purpose of data hiding is to protect the data from misuse by the outside world. 
Data hiding is also known as Data Encapsulation. Without the Encapsulation Principle, we cannot achieve data hiding.

so encapsulation is a mechanism to achieve data hiding 

Why use Encapsulation ?

- Data protection
- Achieving Data Hiding
- Security


## Abstraction 

The process of representing the essential features without including the background details
a process of defining a class by providing the necessary details to call the object operations (i.e., methods) by hiding its implementation details


To take a real-time example, when we log in to any social networking site like Facebook, Twitter, LinkedIn, etc.,
we enter our user ID and password, and then we get logged in. Here, we don’t know how they are processing the data or what logic or algorithm they are using for login.
This information is abstracted/hidden from us since they are not essential to us. This is basically what abstraction is.




here is an example to show how is abstraction is important 

first we violate abstraction to see the result 

```csharp 

using System;
namespace GarbageCollectionDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Transaction doing SBI Bank");
            SBI sbi = new SBI();
            sbi.ValidateCard();
            sbi.WithdrawMoney();
            sbi.CheckBalanace();
            sbi.BankTransfer();
            sbi.MiniStatement();

            Console.WriteLine("\nTransaction doing AXIX Bank");
            AXIX AXIX = new AXIX();
            AXIX.ValidateCard();
            AXIX.WithdrawMoney();
            AXIX.CheckBalanace();
            AXIX.BankTransfer();
            AXIX.MiniStatement();

            Console.Read();
        }
    }
    
    public class SBI 
    {
        public void BankTransfer()
        {
            Console.WriteLine("SBI Bank Bank Transfer");
        }

        public void CheckBalanace()
        {
            Console.WriteLine("SBI Bank Check Balanace");
        }

        public void MiniStatement()
        {
            Console.WriteLine("SBI Bank Mini Statement");
        }

        public void ValidateCard()
        {
            Console.WriteLine("SBI Bank Validate Card");
        }

        public void WithdrawMoney()
        {
            Console.WriteLine("SBI Bank Withdraw Money");
        }
    }

    public class AXIX 
    {
        public void BankTransfer()
        {
            Console.WriteLine("AXIX Bank Bank Transfer");
        }

        public void CheckBalanace()
        {
            Console.WriteLine("AXIX Bank Check Balanace");
        }

        public void MiniStatement()
        {
            Console.WriteLine("AXIX Bank Mini Statement");
        }

        public void ValidateCard()
        {
            Console.WriteLine("AXIX Bank Validate Card");
        }

        public void WithdrawMoney()
        {
            Console.WriteLine("AXIX Bank Withdraw Money");
        }
    }
}
```


The problem is the user of our application accesses the SBI and AXIX classes directly.
Directly means they can go to the class definition and see the implementation details of the methods. That is, the user will come to know how the services or methods are implemented.
This might cause security issues. We should not expose our implementation details to the outside.


here are the ways that we can use for applying this principle 
- Using Interface
- Using Abstract Classes and Abstract Methods


now to provide a good solution we can use interfaces  to achieve the abstraction principle


```csharp

using System;
namespace GarbageCollectionDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Transaction doing SBI Bank");
            IBank sbi = BankFactory.GetBankObject("SBI");
            sbi.ValidateCard();
            sbi.WithdrawMoney();
            sbi.CheckBalanace();
            sbi.BankTransfer();
            sbi.MiniStatement();

            Console.WriteLine("\nTransaction doing AXIX Bank");
            IBank AXIX = BankFactory.GetBankObject("AXIX");
            AXIX.ValidateCard();
            AXIX.WithdrawMoney();
            AXIX.CheckBalanace();
            AXIX.BankTransfer();
            AXIX.MiniStatement();

            Console.Read();
        }
    }

    public interface IBank
    {
        void ValidateCard();
        void WithdrawMoney();
        void CheckBalanace();
        void BankTransfer();
        void MiniStatement();
    }

    public class BankFactory
    {
        public static IBank GetBankObject(string bankType)
        {
            IBank BankObject = null;
            if (bankType == "SBI")
            {
                BankObject = new SBI();
            }
            else if (bankType == "AXIX")
            {
                BankObject = new AXIX();
            }
            return BankObject;
        }
    }

    public class SBI : IBank
    {
        public void BankTransfer()
        {
            Console.WriteLine("SBI Bank Bank Transfer");
        }

        public void CheckBalanace()
        {
            Console.WriteLine("SBI Bank Check Balanace");
        }

        public void MiniStatement()
        {
            Console.WriteLine("SBI Bank Mini Statement");
        }

        public void ValidateCard()
        {
            Console.WriteLine("SBI Bank Validate Card");
        }

        public void WithdrawMoney()
        {
            Console.WriteLine("SBI Bank Withdraw Money");
        }
    }

    public class AXIX : IBank
    {
        public void BankTransfer()
        {
            Console.WriteLine("AXIX Bank Bank Transfer");
        }

        public void CheckBalanace()
        {
            Console.WriteLine("AXIX Bank Check Balanace");
        }

        public void MiniStatement()
        {
            Console.WriteLine("AXIX Bank Mini Statement");
        }

        public void ValidateCard()
        {
            Console.WriteLine("AXIX Bank Validate Card");
        }

        public void WithdrawMoney()
        {
            Console.WriteLine("AXIX Bank Withdraw Money");
        }
    }
}
```

Using the interface, we can achieve 100% abstraction. Now, the user will only know the services that are defined in the interface, but how the services are implemented, the user will never know.
This is how we can implement abstraction . by hiding the implementation details from the user.
Here, the user will only know about IBank, but the user will not know about the SBI and AXIX Classes.


now we want to compare Encapsulation vs Abstraction 

- The Encapsulation Principle is all about data hiding (or information hiding). On the other hand, the Abstraction Principle is all about detailed hiding (implementation hiding). 

- We can implement Encapsulation by declaring the data members as private and exposing the data members only through publicly exposed methods and properties with proper validation.
On the other hand, we can implement abstraction through abstract classes and interfaces.





## Inheritance 

The members defined in one class can be consumed by another class by establishing a parent/child relationship between the classes.




Reusability:
Avoids code duplication by reusing parent class logic in child classes.

- Types of Inheritance in C#:
Single → One parent, one child.
Multilevel → Child becomes a parent for another child.
Hierarchical → Multiple children share the same parent.
Multiple (via interfaces only, not classes).

- Access Modifiers in Inheritance:
public → Accessible everywhere.
protected → Accessible in the same class and its derived classes.
private → Not inherited (accessible only in the same class).

- Constructors:
Parent class constructors are called implicitly before child class constructors.
Use base() to explicitly call a specific parent constructor.

- Method Overriding:
Use virtual in parent and override in child to change behavior.
Use sealed to prevent further overriding.

- Upcasting & Downcasting:
Upcasting → Child reference stored in a parent variable (can only access parent members).
Downcasting → Parent reference cast back to child (can access child-specific members).

- is and as Keywords:
is → Checks type compatibility.
as → Safe casting, returns null if the cast fails.

-new Keyword:
-- Hides a parent method instead of overriding it.

- Polymorphism in Inheritance:

-- Achieved through method overriding (runtime polymorphism).






# Polymorphism in C#

Polymorphism is one of the **four pillars of OOP** and means **"many forms"**.  
It allows the same method, operator, or object to behave differently depending on the context.

In programming, polymorphism means that **a single interface can be used for different underlying forms (data types or classes)**.

---

## Why Polymorphism is Useful
- Increases **flexibility** and **reusability** of code.
- Supports **extensibility** — new classes can be added without changing existing code.
- Reduces **code duplication**.

---

## Types of Polymorphism in C#

1. **Compile-Time Polymorphism** (Static / Early Binding)  
   Method call is bound to the method body **at compile time**.  
   Achieved through:
   - **Method Overloading**
   - **Operator Overloading**

2. **Run-Time Polymorphism** (Dynamic / Late Binding)  
   Method call is resolved **at runtime** by the CLR.  
   Achieved through:
   - **Method Overriding** (with `virtual` / `override`)
   - **Method Hiding** (with `new` keyword — not true overriding)

---

## Compile-Time Polymorphism Example — Method Overloading

```csharp
using System;

namespace MethodOverloading
{
    class Program
    {
        public void Add(int a, int b)
        {
            Console.WriteLine(a + b);
        }
        public void Add(float x, float y)
        {
            Console.WriteLine(x + y);
        }
        public void Add(string s1, string s2)
        {
            Console.WriteLine(s1 + " " + s2);
        }

        static void Main()
        {
            Program obj = new Program();
            obj.Add(10, 20);              // Calls int version
            obj.Add(10.5f, 20.5f);        // Calls float version
            obj.Add("Pranaya", "Rout");   // Calls string version
            Console.ReadKey();
        }
    }
}

```



## Run-Time Polymorphism Example — Method Overriding

```csharp
using System;

namespace PolymorphismDemo
{
    class Class1
    {
        // Virtual method
        public virtual void Show()
        {
            Console.WriteLine("Parent Class Show Method");
        }
    }

    class Class2 : Class1
    {
        // Overridden method
        public override void Show()
        {
            Console.WriteLine("Child Class Show Method");
        }
    }

    class Program
    {
        static void Main()
        {
            Class1 obj1 = new Class2(); // Upcasting
            obj1.Show(); // Decided at runtime — Child's method is called
            Console.ReadKey();
        }
    }
}


Explanation:

Upcasting (Class1 obj1 = new Class2();) allows calling overridden methods.

The decision on which method to call is made at runtime.

Requires:

virtual in base class

override in derived class


Key Points to Remember
Overloading → Same name, different signature → Compile time.

Overriding → Same name, same signature, different implementation → Runtime.

Hiding → Same name, hides base method, not true overriding.

Always use virtual and override for runtime polymorphism.

Upcasting enables polymorphic behavior.
