# How to Create an MCP Server Using .NET

This guide will help you set up a basic MCP (Model Context Protocol) server in .NET, configure it in VS Code, and interact with it using Copilot Chat.

[![Follow me on GitHub](https://img.shields.io/github/followers/nisalgunawardhana?label=Follow%20me%20on%20GitHub&style=social)](https://github.com/nisalgunawardhana)


## How to Clone This Repo and Start Working

1. **Clone the repository from GitHub:**
   ```zsh
   git clone https://github.com/nisalgunawardhana/How-To-create-MCP-Server
   cd How-To-create-MCP-Server
   ```
   > Replace `<your-repo-url>` with your repository's URL and `<repo-folder>` with the cloned folder name.

2. **Follow the steps below to set up and run the MCP server.**

---


---

## 1. Install .NET

Download and install the [.NET SDK](https://dotnet.microsoft.com/download) for your OS.
> **Note:** This guide uses **.NET SDK 8**. Make sure you download the correct version for your operating system.

If you're using **VS Code**, install the [C# Dev Kit extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit) for the best development experience.

Verify installation in your terminal:
```zsh
dotnet --version
```

---

## 2. Create a New Project

Open your terminal and run:
```zsh
dotnet new console -o HelloSriLankaServer
cd HelloSriLankaServer
```

---

## 3. Install Required Packages

Install the necessary NuGet packages:
```zsh
dotnet add package ModelContextProtocol.Server
dotnet add package Microsoft.Extensions.Hosting
```

---

## 4. Update `Program.cs`

Clear the contents of `Program.cs` and replace with:

```csharp
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Logging.Console;
using Microsoft.Extensions.Options;
using ModelContextProtocol.Server;
using System.ComponentModel;

var builder = Host.CreateApplicationBuilder(args);

builder.Logging.AddConsole(options => {
    options.LogToStandardErrorThreshold = LogLevel.Trace;
});

builder.Services.AddMcpServer()
    .WithStdioServerTransport()
    .WithToolsFromAssembly();

var app = builder.Build();

await app.RunAsync();

[McpServerToolType]
public static class HelloTool
{
    [McpServerTool(Name = "HelloTool"), Description("Say hello from the server")]
    public static void SayHello()
    {
        Console.WriteLine("Hello Sri Lanka!");
    }
}
```

---

## 5. Configure MCP in VS Code

1. In your project root, create a `.vscode` directory:
    ```zsh
    mkdir -p .vscode
    ```

2. Inside `.vscode`, create a file named `mcp.json` and add:

    ```json
    {
    "servers": {
        "LocationServer": {
            "type": "stdio",
            "command": "dotnet",
            "args": [
                "run",
                "--project",
                "${workspaceFolder}/HelloSriLankaServer.csproj"
            ]
        }
    }
    }
    ```
    > **Note:** ${workspaceFolder} automatically replace your correct path if not work replace the path with the actual path to your `.csproj` file (right-click the file in VS Code and select "Copy Path").

---

## 6. Run the MCP Server(test if it's work)

From the `HelloSriLankaServer` directory, start the server:
```zsh
dotnet run
```

---

## 7. Add the Tool in Copilot Chat

1. Open Copilot Chat in VS Code.
2. Click the **gear icon** (⚙️) or the **"Add Tool"** button.
3. Select your MCP server (`Hello-Server`) from the list.

**Image Example:(here show Location service but acutally show MCP :HelloSriLankaServer)**
![Add Tool in Copilot Chat](images/image1.png)
![Add Tool in Copilot Chat](images/image2.png)


---

## 8. Send a Message Using Copilot Chat

1. In Copilot Chat, select the `HelloSriLankaServer` tool.
2. Type a message to invoke your tool, for example:
    ```
    Can you Say hello from the tool
    ```
3. You should see a reply from your MCP server in the chat.
    ```
    Hello Sri Lanka !
    
    ```

---

## 9. Get a Reply from the MCP Server

You will receive a response from your MCP server in the Copilot Chat window.

---

## 10. Advanced Demo: Try the MCP Location Server

For a more advanced example, you can explore the [Try-mcp-location-server-demo](https://github.com/nisalgunawardhana/Try-mcp-location-server-demo) repository. This demo showcases how to build and interact with a location-based MCP server using .NET.

---

## Additional Learning Resources

For a deeper understanding of MCP and more hands-on examples, check out the [Introduction to MCP](https://github.com/nisalgunawardhana/introduction-to-mcp) repository. This resource provides tutorials, sample projects, and further guidance on working with MCP in .NET.

---
## Connect with Me

Follow me on social media for more sessions, tech tips, and giveaways:

- [LinkedIn](https://www.linkedin.com/in/nisalgunawardhana/) — Professional updates and networking
- [Twitter (X)](https://x.com/thenisals) — Insights and announcements
- [Instagram](https://www.instagram.com/thenisals) — Behind-the-scenes and daily tips
- [GitHub](https://github.com/nisalgunawardhana) — Repositories and project updates
- [YouTube](https://www.youtube.com/channel/UCNP5-zR4mN6zkiJ9pVCM-1w) — Video tutorials and sessions

Feel free to connect and stay updated!

---

## License

This project is licensed under the [MIT License](LICENSE).

---

**Tip:**  
You can add more tools to your MCP server by extending the `HelloTool` class in `Program.cs`.
