# Graphify: The Absolute Beginner's Guide

Welcome to Graphify! If you have zero experience with programming, AI, or command lines, you are in the right place. This guide will take you from absolute scratch to becoming a power user of Graphify.

## Table of Contents
1. [What is Graphify?](#1-what-is-graphify)
2. [Prerequisites: Getting Your Computer Ready](#2-prerequisites-getting-your-computer-ready)
3. [Installing Graphify](#3-installing-graphify)
4. [Your First Graph: How to Use Graphify](#4-your-first-graph-how-to-use-graphify)
5. [How Graphify Works (Behind the Scenes)](#5-how-graphify-works-behind-the-scenes)
6. [Integrating Graphify with Your AI Assistant](#6-integrating-graphify-with-your-ai-assistant)
7. [Advanced: Integrating Graphify with Other Programs (MCP & Docker)](#7-advanced-integrating-graphify-with-other-programs-mcp--docker)
8. [Advanced: Configuration and Environment Variables](#8-advanced-configuration-and-environment-variables)

---

## 1. What is Graphify?

Imagine you have a giant box of puzzle pieces (your files, code, documents, PDFs, images). Trying to find how one piece connects to another by looking at them one by one is slow and frustrating.

**Graphify** is a tool that looks at all your puzzle pieces and builds a **Knowledge Graph**—a map that shows exactly how everything is connected.

Instead of an AI (like ChatGPT or Claude) reading hundreds of files over and over to answer a question, it can just look at this map. This makes your AI coding assistants incredibly fast, smart, and cheap to run because they instantly know how your entire project fits together.

---

## 2. Prerequisites: Getting Your Computer Ready

Before we install Graphify, we need to make sure your computer has the right tools. You will be using a **Terminal** (Mac/Linux) or **Command Prompt / PowerShell** (Windows). This is just a text window where you type commands for your computer to run.

### Step 1: Install Python
Graphify is built on a programming language called Python.
- Go to [Python.org](https://www.python.org/downloads/) and download the latest version (it must be 3.10 or higher).
- **Important for Windows users:** When installing, make sure to check the box that says **"Add Python to PATH"** at the bottom of the installer window.

To check if Python is installed, open your Terminal/PowerShell and type:
```bash
python --version
```
If it prints a version number (like `Python 3.12.0`), you are good to go!

### Step 2: Install `uv` (The Recommended Way)
`uv` is a tool that makes installing Python programs like Graphify incredibly fast and easy without messing up your computer.

- **Mac/Linux:** Open your terminal and paste this command, then press Enter:
  ```bash
  curl -LsSf https://astral.sh/uv/install.sh | sh
  ```
- **Windows:** Open PowerShell and paste this command, then press Enter:
  ```powershell
  winget install astral-sh.uv
  ```
To check if it worked, close your terminal, open a new one, and type:
```bash
uv --version
```

---

## 3. Installing Graphify

Now that your computer is ready, installing Graphify is a breeze. Open your terminal and type:

```bash
uv tool install graphifyy
```
*(Note: Yes, there are two "y"s in `graphifyy` for the installation package!)*

If your terminal says `graphify: command not found` after this, type `uv tool update-shell`, press Enter, and then close and reopen your terminal.

That's it! Graphify is installed.

---

## 4. Your First Graph: How to Use Graphify

Let's build a map of a folder!

1. Create a folder on your computer and put some text files, documents, or code files in it.
2. Open your terminal and **navigate** to that folder. (You can type `cd ` and then drag and drop the folder into the terminal window, then press Enter).
3. Type the following command and press Enter:

```bash
graphify .
```
*(The `.` means "this current folder")*

### What just happened?
Graphify read all your files and created a new folder called `graphify-out/`. Inside, you will find:
- **`graph.html`**: Double-click this to open it in your web browser. It's a visual, clickable map of your project!
- **`GRAPH_REPORT.md`**: A text file summarizing the most important parts of your project (the "God nodes" or main hubs).
- **`graph.json`**: The raw data file that AI assistants read.

You can now ask questions about your map directly in the terminal:
```bash
graphify query "What are the main files in this project?"
```

---

## 5. How Graphify Works (Behind the Scenes)

*(Consolidated from `how-it-works.md`)*

You don't need to know this to use Graphify, but if you're curious, here is how Graphify processes your files in three passes:

1. **Pass 1 — Code structure (Free, no AI needed):** Graphify reads your code files locally. It finds functions, classes, and how they call each other. This happens instantly on your computer.
2. **Pass 2 — Video and Audio:** If you have media files, Graphify transcribes them locally on your machine.
3. **Pass 3 — Docs, papers, images (Uses AI, costs a tiny bit of money):** For human text (like PDFs or Markdown), Graphify sends them to an AI (like Claude or OpenAI) to figure out what they mean and how they connect.

### How it finds Communities
Graphify groups similar files together into "Communities" (like neighborhoods on a map). It does this by seeing which files talk to each other the most.

### Confidence Tags
When you look at the graph, every connection has a label:
- **`EXTRACTED`**: Graphify is 100% sure because it saw the files directly linked.
- **`INFERRED`**: The AI guessed they are related, with a score (e.g., 0.95 means very sure).
- **`AMBIGUOUS`**: Graphify isn't sure and wants a human to check.

---

## 6. Integrating Graphify with Your AI Assistant

Graphify's true superpower is plugging into AI coding assistants (like Claude Code, Cursor, GitHub Copilot, or Aider).

Instead of typing in the terminal, you can install Graphify directly into your AI assistant.

### Step 1: Install the Skill
Open your terminal in your project folder and run the command for your specific AI:

- **Claude Code:** `graphify claude install`
- **Cursor:** `graphify cursor install`
- **GitHub Copilot Chat:** `graphify copilot install`
- **VS Code Copilot Chat:** `graphify vscode install`
- **Aider:** `graphify aider install`
- **Gemini CLI:** `graphify gemini install`
- **Kimi Code:** `graphify kimi install`

### Step 2: Use It!
Now, open your AI assistant and just talk to it! Because you ran the install command, the AI now knows to look at the `graph.json` map before trying to read all your files. It makes the AI incredibly smart about your specific project.

---

## 7. Advanced: Integrating Graphify with Other Programs (MCP & Docker)

*(Consolidated from `docker-mcp-sqlite.md` and Advanced usages)*

If you are a developer and want to integrate Graphify into custom setups, Graphify supports **MCP (Model Context Protocol)**. MCP is a standard way for AI models to talk to tools.

### Running the Graphify MCP Server
You can expose your graph as an MCP server so a whole team or a custom AI script can access it.

1. Install the MCP extra:
   ```bash
   uv tool install "graphifyy[mcp]"
   ```
2. Start the server (pointing it to your generated graph):
   ```bash
   python -m graphify.serve graphify-out/graph.json
   ```
This gives connected AI assistants structured tools like `query_graph`, `get_node`, and `shortest_path`.

### Shared Team Server (HTTP)
You can run it so your whole team can use one map:
```bash
python -m graphify.serve graphify-out/graph.json --transport http --port 8080
```

### Docker, MCP, and SQLite (Bonus Recipe)
If you use Docker (a tool for running apps in isolated containers) and want a persistent SQL database for your AI to take notes in alongside Graphify, you can install the SQLite MCP server into Docker Desktop.

1. Add the working SQLite server to your Docker MCP profile:
   ```bash
   docker mcp profile server add default --server catalog://mcp/docker-mcp-catalog/SQLite
   ```
2. Pull the image:
   ```bash
   docker pull mcp/sqlite:latest
   ```
3. Connect your client (e.g., Claude Code):
   ```bash
   docker mcp client connect claude-code
   ```
This gives your AI the ability to read and write database queries (`read_query`, `write_query`) seamlessly.

---

## 8. Advanced: Configuration and Environment Variables

If you are using Graphify purely for code, everything runs locally and is 100% free.

However, if you want Graphify to read PDFs, images, or documentation, it needs to use an AI brain (like OpenAI or Claude). To do this, you have to give Graphify a "key" (a password) to use those services.

You do this using **Environment Variables** (hidden settings in your terminal).

For example, if you have an OpenAI key, you would type this before running Graphify:

- **Mac/Linux:**
  ```bash
  export OPENAI_API_KEY="your-secret-key-here"
  ```
- **Windows (PowerShell):**
  ```powershell
  $env:OPENAI_API_KEY="your-secret-key-here"
  ```

Then you tell Graphify to use it:
```bash
graphify extract . --backend openai
```

Other backends you can use include `claude` (Anthropic), `gemini` (Google), and even `ollama` (for running models completely offline and free on a powerful computer).

---

## You Did It!

You now know everything from what Graphify is, to how to install it, use it with AI, and even set up advanced servers. Welcome to the future of understanding your files!
