# Study
GOD


https://codeshare.io/tr9_new    This is the Link of The Class Codeshare

 
https://github.com/AshuMItter/MLA238NetAM.git    Tier9GOD
 
https://github.com/pcankur73/MLA238NetPC         Tier 10GOD


https://learn.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/overview   C#






Project 3



using System;
using System.Collections.Generic;
using System.Linq;

namespace MiniProject2
{
    internal class Program
    {
        class Expenses
        {
            public float Amount { get; set; }
            public string Description { get; set; }
            public string Category { get; set; }
            public DateTime Date { get; set; }
        }

        static void Main(string[] args)
        {
            var expenses = new List<Expenses>();
            var budgetLimits = new Dictionary<string, float>();
            string addMore = "yes";

            while (addMore == "yes")
            {
              // Collect expense details from the user and Check if the budget limit is exceeded
                var expense = CollectExpenseDetails(budgetLimits);
                expenses.Add(expense);

                 CheckBudgetLimit(expenses, budgetLimits, expense);
             addMore = GetStringInput("Add another expense? (yes/no)").ToLower();
            }

             // Display all expenses and budget summary
            DisplayExpenses(expenses);

             
            DisplayBudgetSummary(expenses, budgetLimits);

            Console.WriteLine("\nThank you for using the application. Press any key to exit.");
            Console.ReadKey();
        }

        // Method to collect expense details from the user
        static Expenses CollectExpenseDetails(Dictionary<string, float> budgetLimits)
        {
            float amount = GetFloatInput("Enter Amount:");
            string description = GetStringInput("Enter Description:");
            string category = GetStringInput("Enter Category:");

            // Set budget limit for new categories
            if (!budgetLimits.ContainsKey(category))
            {
                float budgetLimit = GetFloatInput($"Enter Budget Limit for {category}:");
                budgetLimits[category] = budgetLimit;
            }

            DateTime date = GetDateInput("Enter Date (yyyy-mm-dd):");

            return new Expenses { Amount = amount, Description = description, Category = category, Date = date };
        }

        // Method to check if the budget limit is exceeded
        static void CheckBudgetLimit(List<Expenses> expenses, Dictionary<string, float> budgetLimits, Expenses expense)
        {
            float currentTotal = expenses.Where(e => e.Category == expense.Category && e.Date.Month == expense.Date.Month && e.Date.Year == expense.Date.Year).Sum(e => e.Amount);
            if (currentTotal > budgetLimits[expense.Category])
            {
                Console.WriteLine($"Warning: Budget exceeded for {expense.Category} this month!");
            }
        }

        // Method to get float input from the user and string input from the user
        static float GetFloatInput(string prompt)
        {
            Console.WriteLine(prompt);
            return float.Parse(Console.ReadLine());
        }

        static string GetStringInput(string prompt)
        {
            Console.WriteLine(prompt);
            return Console.ReadLine();
        }

        // Method to get date input from the user
        static DateTime GetDateInput(string prompt)
        {
            Console.WriteLine(prompt);
            return DateTime.Parse(Console.ReadLine());
        }

        // Method to display all expenses
        static void DisplayExpenses(List<Expenses> expenses)
        {
            Console.WriteLine("\nExpenses:");
            foreach (var expense in expenses)
            {
                Console.WriteLine($"Amount: {expense.Amount}, Description: {expense.Description}, Category: {expense.Category}, Date: {expense.Date.ToShortDateString()}");
            }
        }

        // Method to display budget summary
        static void DisplayBudgetSummary(List<Expenses> expenses, Dictionary<string, float> budgetLimits)
        {
            Console.WriteLine("\nBudget Summary:");
            foreach (var category in budgetLimits.Keys)
            {
                float totalSpent = expenses.Where(e => e.Category == category).Sum(e => e.Amount);
                float remainingBudget = budgetLimits[category] - totalSpent;
                Console.WriteLine($"{category}: Spent {totalSpent}, Remaining Budget: {remainingBudget}");
            }
        }
    }
}
