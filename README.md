# Phase-2
using System;

public class CalculatorApp
{
    public static void Main(string[] args)
    {
        RunCalculator();
    }

    static void RunCalculator()
    {
        bool keepRunning = true;
        Console.WriteLine("Console Calculator – Software Design and Control Flow");
        Console.WriteLine("Prepared by: Morgan Moore");
        Console.WriteLine("Welcome to the Console Calculator!\n");

        while (keepRunning)
        {
            DisplayMenu();
            keepRunning = HandleMenuSelection();
        }

        Console.WriteLine("Thank you for using the calculator. Goodbye!");
    }

    static void DisplayMenu()
    {
        Console.WriteLine("\n--- Calculator Menu ---");
        Console.WriteLine("1. Add");
        Console.WriteLine("2. Subtract");
        Console.WriteLine("3. Multiply");
        Console.WriteLine("4. Divide");
        Console.WriteLine("5. Evaluate Formula");
        Console.WriteLine("6. Quit");
        Console.Write("Choose an option (1-6): ");
    }

    static bool HandleMenuSelection()
    {
        string choice = Console.ReadLine();
        double a, b;

        switch (choice)
        {
            case "1":
                GetTwoNumbers(out a, out b);
                Console.WriteLine($"Result: {MathOperations.Add(a, b):F2}");
                break;
            case "2":
                GetTwoNumbers(out a, out b);
                Console.WriteLine($"Result: {MathOperations.Subtract(a, b):F2}");
                break;
            case "3":
                GetTwoNumbers(out a, out b);
                Console.WriteLine($"Result: {MathOperations.Multiply(a, b):F2}");
                break;
            case "4":
                GetTwoNumbers(out a, out b);
                var result = MathOperations.Divide(a, b);
                Console.WriteLine(result.HasValue ? $"Result: {result.Value:F2}" : "Error: Division by zero.");
                break;
            case "5":
                Console.Write("Enter a formula (e.g., 5 + 2): ");
                string formula = Console.ReadLine();
                double? eval = MathOperations.EvaluateFormula(formula);
                Console.WriteLine(eval.HasValue ? $"Result: {eval.Value:F2}" : "Invalid formula.");
                break;
            case "6":
                return false;
            default:
                Console.WriteLine("Invalid choice. Please try again.");
                break;
        }

        return true;
    }

    static void GetTwoNumbers(out double a, out double b)
    {
        Console.Write("Enter first number: ");
        while (!double.TryParse(Console.ReadLine(), out a))
            Console.Write("Invalid input. Enter a valid number: ");

        Console.Write("Enter second number: ");
        while (!double.TryParse(Console.ReadLine(), out b))
            Console.Write("Invalid input. Enter a valid number: ");
    }
}

public static class MathOperations
{
    public static double Add(double a, double b) => a + b;
    public static double Subtract(double a, double b) => a - b;
    public static double Multiply(double a, double b) => a * b;

    public static double? Divide(double a, double b)
    {
        return b != 0 ? (double?) (a / b) : null;
    }

    public static double? EvaluateFormula(string formula)
    {
        try
        {
            var dataTable = new System.Data.DataTable();
            object result = dataTable.Compute(formula, "");
            return Convert.ToDouble(result);
        }
        catch
        {
            return null;
        }
    }
}
