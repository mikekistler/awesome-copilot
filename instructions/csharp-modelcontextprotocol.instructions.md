---
description: 'Guidelines for building Model Context Protocol Servers in C#'
applyTo: '**/*.cs'
---

# Building Model Context Protocol Servers in C #

Model Context Protocol (MCP) is an open protocol that standardizes how applications connect
AI models to different data sources and tools.

## 1. Understanding MCP Concepts (Server–Client Architecture)

- **MCP Servers** expose three possible capability types:

  - **Tools**: callable functions (your main focus here)
  - **Resources**: like file-like data sources
  - **Prompts**: reusable templates for client bots

## 2. Code Style and Structure

- Use the [Official MCP C# SDK](https://www.nuget.org/packages/ModelContextProtocol/) for building your server.
- Write idiomatic and efficient C# code.
- Always use the latest version C#, currently C# 13.
- Write clear and concise comments for each function.
- Put classes for MCP Tools in a directory called `Tools`.
- Put classes for MCP Resources in a directory called `Resources`.
- Put classes for MCP Prompts in a directory called `Prompts`.

## 4. Scaffold a Minimal C# MCP Server

Use this prompt with Copilot to generate your `Program.cs`:

```csharp
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using ModelContextProtocol.Server;

var builder = Host.CreateApplicationBuilder(args);

builder.Services
    .AddMcpServer()
    .WithStdioServerTransport()
    .WithToolsFromAssembly();

await builder.Build().RunAsync();
```

This sets up a basic MCP server that:

- Runs inside a .NET Host
- Uses **stdio transport** for communication
- Automatically discovers tools defined in your assembly ([Microsoft for Developers][3], [Medium][4])

---

## 5. Define Your Tools

Ask Copilot to create a tool. For example:

```csharp
using ModelContextProtocol.Server;
using ModelContextProtocol.Server.Attributes;

public static class WeatherTools
{
    [McpTool("get_forecast")]
    public static ForecastResult GetForecast(string location)
    {
        // Insert logic to fetch or mock forecast data
    }

    [McpTool("get_alerts")]
    public static AlertsResult GetAlerts(string location)
    {
        // Insert logic for severe weather alerts
    }
}
```

Copilot can help fill in the method logic, data models (`ForecastResult`, `AlertsResult`), or stub implementations.

---

## 6. Testing with GitHub Copilot

1. Run your server (`dotnet run`)
2. In VS Code, activate GitHub Copilot **Chat Mode**
3. Use the “Select tools” button to confirm your server and tools are recognized ([Microsoft Learn][2])
4. Send a prompt like:

   ```
   What’s the weather forecast for Portland, Oregon?
   ```

5. Approve the tool execution when prompted by Copilot, then inspect the response ([Microsoft Learn][2]).

---

## 7. Further Enhancements & Best Practices

Use Copilot to help with ongoing development tasks, such as:

- **Add configuration via environment variable** (e.g., use an API key for weather data):

  ```csharp
  var apiKey = Environment.GetEnvironmentVariable("WEATHER_API_KEY");
  ```

- **Implement logging**, error handling, or custom serialization inside the tool methods.

- Explore advanced features like **Resources** or **Prompts** if you want to provide file templates or reusable prompts to clients like Claude ([Model Context Protocol][1]).
