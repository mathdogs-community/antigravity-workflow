# Complete Setup Guide: Git, GitHub, and MCP

This guide covers everything you need to set up a new computer (Windows or Mac) from scratch to work with Git, GitHub, and the Model Context Protocol (MCP).

## Phase 1: Installation

### 1. Install Node.js (Required for MCP)
MCP servers often run on Node.js.
*   **Windows/Mac**: Go to [nodejs.org](https://nodejs.org/) and download the **LTS (Long Term Support)** version.
*   Run the installer and accept the defaults.
*   **Verify**: Open a terminal (Command Prompt or Terminal) and run:
    ```bash
    node -v
    npm -v
    ```

### 2. Install Git
*   **Windows**:
    *   Download the installer from [git-scm.com](https://git-scm.com/download/win).
    *   Run the installer. You can mostly accept the defaults, but we recommend selecting "Override the default branch name for new repositories" and setting it to `main`.
*   **Mac**:
    *   Open the Terminal app.
    *   Type `git --version`. If it's not installed, macOS will prompt you to install the Xcode Command Line Tools. Accept this.
    *   Alternatively, if you use Homebrew: `brew install git`.

### 3. Install GitHub CLI (`gh`)
This tool makes authenticating and managing GitHub much easier.
*   **Windows**:
    *   Open PowerShell or Command Prompt and run:
        ```powershell
        winget install --id GitHub.cli
        ```
    *   *Or download the installer from [cli.github.com](https://cli.github.com/)*.
*   **Mac**:
    *   If you have Homebrew: `brew install gh`.
    *   *Or download the macOS installer from [cli.github.com](https://cli.github.com/)*.

---

## Phase 2: Configuration & Authentication

### 1. Configure Git Identity
Tell Git who you are. This name/email will appear on your commits.
Open your terminal and run:
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### 2. Authenticate with GitHub
This links your computer to your GitHub account.
1.  Run this command in your terminal:
    ```bash
    gh auth login
    ```
2.  Follow the interactive prompts:
    *   **Account**: GitHub.com
    *   **Protocol**: HTTPS
    *   **Authenticate**: Login with a web browser
3.  Copy the one-time code shown in the terminal.
4.  Press Enter to open your browser, paste the code, and authorize.

---

## Phase 3: Getting a Token for MCP

While `gh auth login` connects your terminal, the MCP server usually needs a specific **Personal Access Token (PAT)** to read/write to your repositories programmatically.

1.  Go to GitHub.com: [Settings > Developer settings > Personal access tokens](https://github.com/settings/tokens).
2.  Select **Fine-grained tokens** (Recommended) or **Tokens (classic)**.
3.  Click **Generate new token**.
4.  **Name**: Give it a clear name (e.g., "MCP-Antigravity-PC").
5.  **Expiration**: Choose an expiration (e.g., 30 days or 90 days).
6.  **Repository access**: Select "All repositories" or specific ones you want the agent to access.
7.  **Permissions**:
    *   **Contents**: Read and Write (to read code and save changes)
    *   **Pull Requests**: Read and Write
    *   **Issues**: Read and Write
    *   *(Add others if you need specific features)*
8.  **Generate** and **COPY** the token immediately. You won't see it again.

---

## Phase 4: Setting up MCP (The Safe Way)

Now we will set up the folder where your AI agent will work, ensuring your token stays safe.

### 1. Create a Workspace
Create a folder on your computer for your project.
```bash
mkdir MyProject
cd MyProject
```

### 2. Create the `.env` File (For Secrets)
This file will store your token. **Never** share this file.
1.  Create a file named `.env`.
2.  Add your token inside:
    ```env
    GITHUB_PERSONAL_ACCESS_TOKEN=your_token_starts_with_github_pat_...
    ```

### 3. Create the `.gitignore` File (For Safety)
This tells Git to ignore your secrets file.
1.  Create a file named `.gitignore`.
2.  Add this line:
    ```text
    .env
    ```

### 4. Create `mcp_config.json`
This tells the AI agent how to run the GitHub MCP server.
1.  Create a file named `mcp_config.json`.
2.  Paste this configuration:
    ```json
    {
      "mcpServers": {
        "github": {
          "command": "npx",
          "args": [
            "-y",
            "@modelcontextprotocol/server-github"
          ],
          "env": {
            "GITHUB_PERSONAL_ACCESS_TOKEN": "YOUR_TOKEN_HERE_OR_USE_ENV_VAR"
          }
        }
      }
    }
    ```
    *> **Pro Tip**: If your system supports reading from `.env` automatically (like Antigravity often does), you don't need to hardcode the token in the JSON. You can just ensure the environment variable is loaded.*

---

## Phase 5: Verification

To verify everything is working:

1.  **Check Git**: `git status` (should say "not a git repository" or show status).
2.  **Check GitHub Auth**: `gh auth status` (should show you are logged in).
3.  **Check Node**: `npx -v` (should show a version number).

You are now ready to let Antigravity or any MCP client connect to this folder!
