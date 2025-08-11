# Liskov Substitution Principle (LSP)
We can say that when we have Parent-Child relationships, i.e., Inheritance Relationships between two classes,
then if we successfully replace the object/instance of a parent class with an object/instance of the child class without affecting the behavior of the base class instance,
it is said to be in Liskov Substitution Principle.
If you are not getting this point properly, don’t worry; we will see some real-time examples to understand this concept.

Violating LSP

```csharp
public class BankAccount
    {
        protected double balance;
        public virtual void Deposit(double amount)
        {
            balance += amount;
        }
        public virtual void Withdraw(double amount)
        {
            if (balance >= amount)
            {
                balance -= amount;
            }
            else
            {
                throw new InvalidOperationException("Insufficient funds");
            }
        }
        public double GetBalance()
        {
            return balance;
        }
    }
    public class FixedTermDepositAccount : BankAccount
    {
        public override void Withdraw(double amount)
        {
            throw new InvalidOperationException("Cannot withdraw from a fixed term deposit account until term ends");
        }
    }

```
now here if you noticed if we replaced a FixedTermDepositAccount object With a BankAccount object in a context that expects withdrawals to be possible, the program will break, violating the Liskov Substitution Principle (LSP).


So how can we solve this problem
first we depend on abstracts or interfaces and try to abstract all shared methods 

so here is the solution 



Following LSP:


```csharp
public abstract class BankAccount
    {
        protected double balance;
        public virtual void Deposit(double amount)
        {
            balance += amount;
            Console.WriteLine($"Deposit: {amount}, Total Amount: {balance}");
        }
        public abstract void Withdraw(double amount);
        public double GetBalance()
        {
            return balance;
        }
    }
    public class RegularAccount : BankAccount
    {
        public override void Withdraw(double amount)
        {
            if (balance >= amount)
            {
                balance -= amount;
                Console.WriteLine($"Withdraw: {amount}, Balance: {balance}");
            }
            else
            {
                Console.WriteLine($"Trying to Withdraw: {amount}, Insufficient Funds, Available Funds: {balance}");
            }
        }
    }
    public class FixedTermDepositAccount : BankAccount
    {
        private bool termEnded = false; // simplification for the example
        public override void Withdraw(double amount)
        {
            if (!termEnded)
            {
                Console.WriteLine("Cannot withdraw from a fixed term deposit account until term ends");
            }
            else if (balance >= amount)
            {
                balance -= amount;
                Console.WriteLine($"Withdraw: {amount}, Balance: {balance}");
            }
            else
            {
                Console.WriteLine($"Trying to Withdraw: {amount}, Insufficient Funds, Available Funds: {balance}");
            }
        }
    }
    
    //Testing the Liskov Substitution Principle
    public class Program
    {
        public static void Main()
        {
            Console.WriteLine("RegularAccount:");
            var RegularBankAccount = new RegularAccount();
            RegularBankAccount.Deposit(1000);
            RegularBankAccount.Deposit(500);
            RegularBankAccount.Withdraw(900);
            RegularBankAccount.Withdraw(800);
            Console.WriteLine("\nFixedTermDepositAccount:");
            var FixedTermDepositBankAccount = new FixedTermDepositAccount();
            FixedTermDepositBankAccount.Deposit(1000);
            FixedTermDepositBankAccount.Withdraw(500);
            
            Console.ReadKey();
        }
    }
 ```

so if you noticed here we did a back acocunt this is our main or super class inherets and override its common methods the subclasses each class implemetns the functions as it want 
