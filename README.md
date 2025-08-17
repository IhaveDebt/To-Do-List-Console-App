using System;
using System.Collections.Generic;
using System.IO;
using System.Text.Json;

class Program
{
    static string filePath = "tasks.json";
    static List<string> tasks = new();

    static void Main()
    {
        LoadTasks();

        while (true)
        {
            Console.WriteLine("\n=== To-Do List ===");
            Console.WriteLine("1) View Tasks");
            Console.WriteLine("2) Add Task");
            Console.WriteLine("3) Remove Task");
            Console.WriteLine("4) Exit");
            Console.Write("Choose an option: ");

            var choice = Console.ReadLine();
            switch (choice)
            {
                case "1": ViewTasks(); break;
                case "2": AddTask(); break;
                case "3": RemoveTask(); break;
                case "4": SaveTasks(); return;
                default: Console.WriteLine("Invalid option."); break;
            }
        }
    }

    static void ViewTasks()
    {
        if (tasks.Count == 0)
        {
            Console.WriteLine("No tasks yet!");
            return;
        }

        Console.WriteLine("\nYour tasks:");
        for (int i = 0; i < tasks.Count; i++)
            Console.WriteLine($"{i + 1}. {tasks[i]}");
    }

    static void AddTask()
    {
        Console.Write("Enter a new task: ");
        string? task = Console.ReadLine();
        if (!string.IsNullOrWhiteSpace(task))
        {
            tasks.Add(task.Trim());
            SaveTasks();
            Console.WriteLine("Task added!");
        }
    }

    static void RemoveTask()
    {
        ViewTasks();
        Console.Write("Enter task number to remove: ");
        if (int.TryParse(Console.ReadLine(), out int index) &&
            index > 0 && index <= tasks.Count)
        {
            Console.WriteLine($"Removed: {tasks[index - 1]}");
            tasks.RemoveAt(index - 1);
            SaveTasks();
        }
        else
        {
            Console.WriteLine("Invalid number.");
        }
    }

    static void SaveTasks()
    {
        File.WriteAllText(filePath, JsonSerializer.Serialize(tasks));
    }

    static void LoadTasks()
    {
        if (File.Exists(filePath))
        {
            string json = File.ReadAllText(filePath);
            tasks = JsonSerializer.Deserialize<List<string>>(json) ?? new();
        }
    }
}
