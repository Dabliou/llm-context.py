# LLM Context

LLM Context is a tool designed to help developers efficiently copy and paste relevant context from code repositories or text-based document collections into Large Language Model (LLM) chats. It leverages `.gitignore` patterns for smart file selection and uses the clipboard for seamless integration with LLM interfaces.

> **Note on AI Assistance**: This project was developed in collaboration with Claude-3.5-Sonnet. LLM Context itself was used during development to share code context with Claude in project mode. All of the code that makes it into the repo is human curated (by me 😇, @restlessronin).

## Key Features

- **Intelligent File Selection**: Respects `.gitignore` rules and additional custom ignore patterns to exclude irrelevant files.
- **Clipboard Integration**: Automatically copies the generated context to your clipboard for easy pasting into LLM chats.
- **Customizable Ignore Patterns**: Allows additional ignore patterns to be specified, giving you fine-grained control over what's included in the context.
- **Versatile Content Support**: Works with both code repositories and collections of text-based documents.

## Current Usage Patterns

1. **LLM Integration**: LLM Context has been primarily used to provide Project Knowledge in Claude and GPT Knowledge in OpenAI. It can also be used with vanilla chat interfaces, though this use case has been less explored.

2. **Project Types**: The tool has been successfully used with both code repositories and collections of text/markdown documents, making it versatile for various types of projects.

3. **Project Size**: LLM Context has been mainly used for projects where all files can be comfortably loaded into the LLM's context. Its usage for larger projects, where not all files would fit within the LLM's context window, has been limited and the workflow for such cases is less optimized.

We welcome feedback on how to improve the workflow for larger projects and other use cases.

## Installation

Use [pipx](https://pypa.github.io/pipx/) to install LLM Context:

```
pipx install llm-context
```

## Usage

### Quick Start and Typical Workflow

1. Navigate to your project's root directory.
2. (Optional) Edit `.llm-context/config.json` to [add custom ignore patterns](#customizing-ignore-patterns).
3. Run `lc-sel-full` to select files for full content inclusion.
4. (Optional) Edit the selected file list. See the [Usage Guide](#reviewing-and-editing-selected-files) for instructions.
5. Run `lc-context` to generate and copy the context to your clipboard.
6. Paste the generated context into your Claude Project Knowledge or GPT Knowledge.
7. Start your conversation with the LLM about your project.

### Handling LLM File Requests

When the LLM requests specific files:

1. Copy the LLM's file request (typically in a markdown block) to your clipboard.
2. Run `lc-clipfiles` to generate the content of the requested files.
3. Paste the generated file contents back into your chat with the LLM.

This process allows the LLM to access the full content of the requested files for a more comprehensive analysis, without modifying the original context.

For more detailed usage instructions, please refer to our [Usage Guide](docs/usage.md).

### Configuration

#### Customizing Ignore Patterns

You can add custom ignore patterns to exclude specific files or directories from being processed by LLM Context. This is particularly useful for ignoring files that are typically large, frequently changing, or not directly relevant to the code logic, but are still version controlled.

1. Create a `.llm-context/config.json` file in your project root if it doesn't exist.
2. Add or modify the `gitignores` key in the JSON file.

The custom ignore patterns should focus on files that are not already ignored by your project's top-level .gitignore but may not be useful for code context, such as large generated files, detailed changelogs, or environment-specific configuration files.

Example:

```json
{
  "gitignores": {
    "full_files": [
      ".git",
      ".gitignore",
      ".llm-context/",
      "*.lock",
      "CHANGELOG.md",
      "*.log",
      "*.svg",
      "*.png",
      "*.jpg",
      "*.jpeg",
      "*.gif",
      "*.ico",
      "*.woff",
      "*.woff2",
      "*.eot",
      "*.ttf",
      "*.map"
    ]
  }
}
```

#### Reviewing and Editing Selected Files

You can review and manually edit the list of selected files to fine-tune the context provided to the LLM. The `.llm-context/curr_ctx.json` file in your project root contains the current selection. This can be useful for checking what's included in the context or for debugging context overflow issues.

## Experimental: Handling Larger Repositories

For larger repositories, we're exploring a workflow that combines full file content and file outlines to provide a more comprehensive yet manageable context:

- Full content would be included for key files that require detailed analysis.
- Outlines could be provided for less critical files or those that are too large for full inclusion.

This approach might allow you to provide context for more files without exceeding the LLM's context window limit.

If you wish to experiment with this approach, you can use the `lc-sel-outline` command after `lc-sel-full` to select files for outline inclusion.

When using this feature, if the AI requests to see the full content of an outlined file, you should use the `lc-clipfiles` command in conjunction with the outline. This allows you to provide the complete file content when needed, while still benefiting from the more compact outline in the initial context.

**Note:** The outlining feature currently supports the following programming languages:
C, C++, C#, Elisp, Elixir, Elm, Go, Java, JavaScript, OCaml, PHP, Python, QL, Ruby, Rust, and TypeScript. Files in unsupported languages will not be outlined and will be excluded from the outline selection.

We welcome feedback on this experimental feature and how it might be improved to better handle larger projects.

### Feedback and Contributions

If you encounter any issues, have suggestions for improvements, or want to share your experience using the tool, please open an issue on our GitHub repository or submit a pull request with proposed changes.

## Acknowledgments

LLM Context has evolved from several projects and influences:

- This project is a successor to [LLM Code Highlighter](https://github.com/restlessronin/llm-code-highlighter), a TypeScript library developed for use in IDEs like VS Code.
- LLM Code Highlighter was inspired by [Aider Chat](https://github.com/paul-gauthier/aider), particularly its [RepoMap](https://aider.chat/docs/repomap.html) functionality.
- The original concept grew out of a project for [RubberDuck](https://github.com/rubberduck-ai/rubberduck-vscode) and was later used for [Continue](https://github.com/continuedev/continuedev).
- LLM Code Highlighter included functionality for ranking and highlighting tags, based on a translation of Aider Chat's Python code to TypeScript (with the help of Chat-GPT-4). This functionality is not yet implemented in LLM Context.
- The outlining functionality, independently developed in LLM Code Highlighter, has been moved to this project.
- Parts of the code in LLM Context were translated from TypeScript to Python with Claude-3.5-Sonnet's help, bringing the project full circle (Python -> TypeScript -> Python).
- This project currently uses the tree-sitter [tag query files](src/llm_context/highlighter/tag-qry/) from Aider Chat.

We are grateful for the open-source community and the innovations that have influenced this project's development.

I am grateful for the help of Claude-3.5-Sonnet in the development of this project.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the Apache License, Version 2.0. See the [LICENSE](LICENSE) file for details.