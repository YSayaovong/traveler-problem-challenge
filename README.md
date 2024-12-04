using System;
using System.Collections.Generic;
using System.Linq;

class Program
{
    static void Main(string[] args)
    {
        // Step 1: Define the city network as a dictionary (Graph representation)
        var cityGraph = new Dictionary<string, List<string>>
        {
            { "Arizona", new List<string> { "Utah", "New Mexico" } },
            { "Utah", new List<string> { "Colorado" } },
            { "New Mexico", new List<string> { "Texas" } },
            { "Colorado", new List<string> { "Kansas" } },
            { "Texas", new List<string> { "LA" } },
            { "Kansas", new List<string> { "LA" } },
            { "LA", new List<string>() } // LA is a terminal city
        };

        // Step 2: Call the recursive function to find the shortest path
        var shortestPath = FindShortestPath(cityGraph, "Arizona", "LA", new List<string>());

        // Step 3: Display the shortest path
        Console.WriteLine("Shortest Path: " + string.Join(" -> ", shortestPath));
    }

    /// <summary>
    /// Finds the shortest path between two cities using recursion and LINQ.
    /// </summary>
    /// <param name="graph">Dictionary representing the city network</param>
    /// <param name="start">Starting city</param>
    /// <param name="destination">Destination city</param>
    /// <param name="visited">List of visited cities to avoid loops</param>
    /// <returns>List representing the shortest path</returns>
    static List<string> FindShortestPath(Dictionary<string, List<string>> graph, string start, string destination, List<string> visited)
    {
        // Add the current city to the visited list
        visited.Add(start);

        // Base Case: If the start equals the destination, return the current path
        if (start == destination)
            return new List<string> { start };

        // Recursive Case: Explore neighboring cities
        var paths = new List<List<string>>();

        foreach (var neighbor in graph[start].Where(city => !visited.Contains(city)))
        {
            var path = FindShortestPath(graph, neighbor, destination, new List<string>(visited));
            if (path.Any()) // Only add non-empty paths
                paths.Add(new List<string> { start }.Concat(path).ToList());
        }

        // Use LINQ to find the shortest path based on the number of cities
        return paths.OrderBy(path => path.Count).FirstOrDefault() ?? new List<string>();
    }
}
