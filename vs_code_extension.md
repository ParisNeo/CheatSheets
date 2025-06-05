
## VS Code Extension: Build and Publishing Guide

This document outlines the steps to build your VS Code extension and publish it to the Visual Studio Marketplace.

**Table of Contents:**
1.  Prerequisites
2.  Understanding Key Files
3.  Part 1: Building Your Extension
    *   3.1. Setting up Your `package.json`
    *   3.2. Compiling/Transpiling (if applicable)
    *   3.3. Packaging with `vsce`
    *   3.4. Local Testing of the `.vsix`
4.  Part 2: Publishing to the VS Code Marketplace
    *   4.1. Create a Publisher
    *   4.2. Get a Personal Access Token (PAT)
    *   4.3. Login with `vsce`
    *   4.4. Publishing Your Extension
    *   4.5. Updating Your Extension
5.  Best Practices
6.  Troubleshooting Common Issues

---

### 1. Prerequisites

Before you begin, ensure you have the following installed:

*   **Node.js and npm (or Yarn):** VS Code extensions are typically JavaScript/TypeScript based. Download from [nodejs.org](https://nodejs.org/).
*   **VS Code:** For testing your extension.
*   **`vsce` (Visual Studio Code Extensions):** The command-line tool for packaging, publishing, and managing VS Code extensions. Install it globally:
    ```bash
    npm install -g @vscode/vsce
    # or
    yarn global add @vscode/vsce
    ```
*   **Git:** Highly recommended for version control.
*   **An Azure DevOps Organization:** The VS Code Marketplace is backed by Azure DevOps. You'll need an account. If you don't have one, you can create one for free at [dev.azure.com](https://dev.azure.com).

---

### 2. Understanding Key Files

Your project will have several important files:

*   `package.json`:
    *   Defines metadata (name, version, description, publisher, icon, etc.).
    *   Lists dependencies.
    *   Contains scripts for building, linting, testing.
    *   Crucial for `vsce`.
*   `README.md`: This will be the main content for your extension's Marketplace page. Make it informative and appealing.
*   `CHANGELOG.md`: Tracks changes between versions. Good practice for users.
*   `LICENSE`: Specify the license for your extension (e.g., MIT).
*   `.vscodeignore`: Similar to `.gitignore`, this file specifies which files and folders should *not* be included in your packaged extension (`.vsix` file). This helps keep the extension size small.
    *   **Common entries:** `.vscode`, `.git`, `node_modules/` (if bundling), test files, source maps (unless needed for debugging published extensions).
*   `icon.png`: A 128x128px icon for your extension. Referenced in `package.json`.

---

### 3. Part 1: Building Your Extension

"Building" typically means preparing your code and then packaging it into a `.vsix` file, which is the distributable format for VS Code extensions.

#### 3.1. Setting up Your `package.json`

Ensure your `package.json` has the following essential fields correctly filled:

```json
{
  "name": "your-extension-name", // kebab-case, unique
  "displayName": "Your Extension Display Name",
  "description": "A brief but clear description of what your extension does.",
  "version": "0.0.1", // Use Semantic Versioning (Major.Minor.Patch)
  "publisher": "your-publisher-name", // This MUST match your Marketplace publisher ID
  "engines": {
    "vscode": "^1.70.0" // Minimum VS Code version your extension supports
  },
  "categories": [
    "Linters",
    "Programming Languages",
    "Themes",
    "Other"
    // Choose relevant categories from the allowed list
  ],
  "activationEvents": [
    // When your extension activates, e.g.,
    "onLanguage:python",
    "onCommand:your-extension-name.start",
    "workspaceContains:your_config_file.json",
    "*" // Not recommended for performance unless necessary
  ],
  "main": "./out/extension.js", // Entry point of your extension (JS file)
  "icon": "images/icon.png", // Path to your extension's icon (128x128px)
  "contributes": {
    // Define commands, settings, keybindings, languages, themes, etc.
    "commands": [
      {
        "command": "your-extension-name.helloWorld",
        "title": "Hello World"
      }
    ]
  },
  "scripts": {
    "vscode:prepublish": "npm run compile", // Script run before packaging by vsce
    "compile": "tsc -p ./", // If using TypeScript
    "watch": "tsc -watch -p ./", // If using TypeScript
    "lint": "eslint src --ext ts",
    "test": "node ./out/test/runTest.js"
  },
  "devDependencies": {
    "@types/vscode": "^1.70.0",
    "@types/glob": "^8.0.0",
    "@types/mocha": "^10.0.0",
    "@types/node": "16.x",
    "eslint": "^8.20.0",
    "glob": "^8.0.3",
    "mocha": "^10.0.0",
    "typescript": "^4.8.0",
    "@vscode/test-electron": "^2.1.5"
  }
  // "dependencies": { ... } // Runtime dependencies
}
```

**Important Notes for `package.json`:**
*   **`publisher`**: This field is critical and must match the publisher ID you create on the Marketplace.
*   **`name`**: Should be unique. The full identifier of your extension will be `publisher.name`.
*   **`version`**: Increment this for every new release. `vsce` will prevent publishing if the version in `package.json` matches an already published version.
*   **`icon`**: Must be a 128x128 PNG.
*   **`vscode:prepublish` script**: `vsce` automatically runs this script before packaging. Use it for compilation, bundling, or any other build steps.
*   **`repository`**: Add a `repository` field if your code is on GitHub/GitLab for easy linking from the Marketplace:
    ```json
      "repository": {
        "type": "git",
        "url": "https://github.com/your-username/your-repo.git"
      }
    ```
*   **`homepage`**: Link to your project's homepage (often the GitHub repo).
*   **`bugs`**: Link to your issue tracker.

#### 3.2. Compiling/Transpiling (if applicable)

If you're using TypeScript (common for VS Code extensions), you need to compile it to JavaScript.
Typically, you'll have a `tsconfig.json` and a compile script in your `package.json`:

```json
// package.json (scripts section)
"scripts": {
  "compile": "tsc -p ./",
  "watch": "tsc -watch -p ./",
  "vscode:prepublish": "npm run compile" // Or just "tsc -p ./" if no other prepublish steps
}
```
Run the compile script:
```bash
npm run compile
# or
yarn compile
```
This will usually output JavaScript files to an `out` directory (as configured in `tsconfig.json` and referenced in `package.json`'s `main` field).

#### 3.3. Packaging with `vsce`

Once your code is ready (compiled, assets in place, `package.json` updated), you can package it:

1.  **Open your terminal in the root of your extension project.**
2.  **Run the package command:**
    ```bash
    vsce package
    ```
    This command will:
    *   Run the `vscode:prepublish` script from your `package.json` (if it exists).
    *   Validate your `package.json` and `README.md`.
    *   Bundle files according to `.vscodeignore`.
    *   Create a `.vsix` file (e.g., `your-extension-name-0.0.1.vsix`).

    If there are errors, `vsce` will report them. Common issues include missing fields in `package.json`, a missing `README.md`, or an invalid icon.

#### 3.4. Local Testing of the `.vsix`

Before publishing, it's crucial to test the packaged extension:

1.  Open VS Code.
2.  Go to the Extensions view (Ctrl+Shift+X or Cmd+Shift+X).
3.  Click the "..." (More Actions) menu at the top of the Extensions view.
4.  Select "Install from VSIX..."
5.  Browse to and select the `.vsix` file you just created.
6.  VS Code will install it. You might need to reload VS Code.
7.  Thoroughly test your extension's functionality.

---

### 4. Part 2: Publishing to the VS Code Marketplace

#### 4.1. Create a Publisher

A Publisher is an identity that can publish extensions to the Marketplace. You only need to do this once.

1.  Go to the [VS Code Marketplace publisher management page](https://marketplace.visualstudio.com/manage/publishers/).
2.  Sign in with the same Microsoft account associated with your Azure DevOps organization.
3.  Click on "**Create publisher**".
4.  Fill in the details:
    *   **Name:** A human-readable name for your publisher (e.g., "Your Company Name" or "Your Name").
    *   **ID:** A unique identifier for your publisher (e.g., `yourcompany` or `yourusername`). **This ID is what you put in your `package.json`'s `publisher` field.** Choose it carefully; it cannot be easily changed.
    *   Provide other requested details.
5.  Click **Create**.

#### 4.2. Get a Personal Access Token (PAT)

To allow `vsce` to publish on your behalf, you need a Personal Access Token (PAT) from Azure DevOps.

1.  Go to your Azure DevOps organization: `https://dev.azure.com/{yourorganization}`.
2.  Click on **User settings** (icon usually at the top right) and select **Personal access tokens**.
3.  Click **+ New Token**.
4.  Configure the token:
    *   **Name:** Give it a descriptive name (e.g., `VSCode Marketplace Publish`).
    *   **Organization:** Select "All accessible organizations" or ensure the correct one is chosen.
    *   **Expiration:** Choose an expiration date (max 1 year). You'll need to renew it later.
    *   **Scopes:** Select **Custom defined**.
    *   Click **Show all scopes** (at the bottom).
    *   Find and check **Marketplace** > **Manage** (or **Publish** depending on UI version). This is the *only* scope required.
5.  Click **Create**.
6.  **IMPORTANT:** Azure DevOps will show you the token **ONCE**. Copy it immediately and store it in a secure place (like a password manager). You won't be able to see it again.

#### 4.3. Login with `vsce`

Now, link `vsce` to your publisher using the PAT.

1.  In your terminal, run:
    ```bash
    vsce login <your-publisher-id>
    ```
    Replace `<your-publisher-id>` with the Publisher ID you created (e.g., `yourusername`).
2.  `vsce` will prompt you for your Personal Access Token. Paste the PAT you copied.
3.  If successful, `vsce` will store the PAT locally for future use with this publisher.

Alternatively, you can pass the PAT directly to the publish command using the `-p` flag, which is useful for CI/CD environments (see below).

#### 4.4. Publishing Your Extension

Make sure:
*   You have incremented the `version` in `package.json` if you're updating an existing extension.
*   Your `publisher` field in `package.json` matches your publisher ID.
*   You have tested the `.vsix` locally.
*   Your `README.md` is complete and looks good.

To publish:

1.  Open your terminal in the root of your extension project.
2.  Run:
    ```bash
    vsce publish
    ```

    If you haven't logged in (or for CI), you can use the PAT directly:
    ```bash
    vsce publish -p <your-pat>
    ```

    `vsce` will perform the packaging steps (including running `vscode:prepublish`), then upload the `.vsix` to the Marketplace.

    You can also use version increment helpers:
    ```bash
    vsce publish minor  # Increments minor version (e.g., 0.1.0 -> 0.2.0) and publishes
    vsce publish major  # Increments major version (e.g., 0.1.0 -> 1.0.0) and publishes
    vsce publish patch  # Increments patch version (e.g., 0.0.1 -> 0.0.2) and publishes
    # Or provide a specific version
    vsce publish 2.0.1
    ```
    These commands update `package.json`, commit the change (if in a git repo), tag it, and then publish.

3.  After a successful publish, your extension will appear on the [VS Code Marketplace](https://marketplace.visualstudio.com/vscode). It might take a few minutes to show up or update. You can manage it via your publisher page.

#### 4.5. Updating Your Extension

To publish an update:

1.  Make your code changes.
2.  **Increment the `version` number in `package.json`**. This is mandatory.
3.  Update your `CHANGELOG.md`.
4.  Commit your changes to Git (good practice).
5.  Run `vsce publish` (or `vsce publish patch/minor/major`).

---

### 5. Best Practices

*   **Semantic Versioning:** Use `MAJOR.MINOR.PATCH` (e.g., `1.2.3`).
*   **Thorough `README.md`:** This is your store page. Include:
    *   What the extension does.
    *   Features list.
    *   Screenshots/GIFs demonstrating functionality.
    *   How to use it (settings, commands).
    *   Requirements or dependencies.
    *   Known issues.
    *   Release notes (or link to `CHANGELOG.md`).
*   **Meaningful `CHANGELOG.md`:** Keep users informed of what's new.
*   **Icon:** A clear, recognizable icon helps your extension stand out.
*   **Small `.vsix` Size:** Use `.vscodeignore` effectively. Only include necessary files. Consider using a bundler like Webpack or esbuild if you have many small files or large dependencies to reduce file count and potentially size.
*   **Testing:** Write unit tests and integration tests for your extension.
*   **Error Handling:** Implement robust error handling and provide useful feedback to users.
*   **Performance:** Pay attention to activation events to avoid activating your extension unnecessarily, which can slow down VS Code startup.
*   **CI/CD:** Automate your build and publish process using GitHub Actions, Azure Pipelines, etc. Store your PAT as a secret in your CI system.
    *   Example GitHub Action step:
      ```yaml
      - name: Publish to VS Code Marketplace
        run: npx @vscode/vsce publish -p ${{ secrets.VSCE_PAT }}
      ```

---

### 6. Troubleshooting Common Issues

*   **`ERROR publisher <name> not found`**:
    *   Ensure the `publisher` name in `package.json` exactly matches the ID of the publisher you created on the Marketplace.
    *   You might not have created the publisher yet, or there's a typo.
*   **`ERROR Version <x.y.z> already exists.`**:
    *   You must increment the `version` in `package.json` before publishing an update.
*   **PAT issues (401 Unauthorized)**:
    *   The PAT might have expired. Generate a new one.
    *   The PAT might not have the correct "Marketplace (Manage)" scope.
    *   You might have copied the PAT incorrectly.
*   **`Missing field: <field_name>` (e.g., `description`, `version`)**:
    *   `vsce` validates `package.json` strictly. Ensure all required fields are present.
*   **`File or folder not found (icon.png)`**:
    *   The path to `icon` in `package.json` is incorrect or the file is missing.
*   **`README.md not found`**:
    *   Ensure you have a `README.md` in the root of your project.
*   **Large `.vsix` file**:
    *   Review your `.vscodeignore`. Are you including `node_modules` unnecessarily? Are there large assets or build artifacts being included?
    *   Consider using a JavaScript bundler (Webpack, esbuild, Rollup) to tree-shake and minify your code, especially if you have many dependencies. `vsce package --yarn` or `vsce package --webpack` can sometimes help if your project is structured for it.
