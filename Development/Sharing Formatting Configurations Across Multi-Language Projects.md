- [x] Sharing Formatting Configurations Across Multi-Language Projects ➕ 2025-06-26 ✅ 2025-07-01

### How can I share formatting across developers for a React and Spring project using the Zed editor?

To share formatting configurations in Zed, you can use project-specific settings by creating a `.zed` folder in your project's root directory.

#### Project-Level Configuration

Create a `.zed/settings.json` file in each project (React and Spring) to define shared formatting rules. This allows team members to use the same configuration while maintaining their personal user preferences.

#### React Project Configuration (`.zed/settings.json`)

This configuration sets up Prettier for formatting and ESLint for linting, with format-on-save enabled.

```JSON
{
  "languages": {
    "JavaScript": {
      "formatter": {
        "external": {
          "command": "prettier",
          "arguments": ["--stdin-filepath", "{buffer_path}"]
        }
      },
      "format_on_save": "on",
      "tab_size": 2,
      "code_actions_on_format": {
        "source.fixAll.eslint": true
      }
    },
    "TypeScript": {
      "formatter": {
        "external": {
          "command": "prettier",
          "arguments": ["--stdin-filepath", "{buffer_path}"]
        }
      },
      "format_on_save": "on",
      "tab_size": 2,
      "code_actions_on_format": {
        "source.fixAll.eslint": true
      }
    },
    "TSX": {
      "formatter": {
        "external": {
          "command": "prettier",
          "arguments": ["--stdin-filepath", "{buffer_path}"]
        }
      },
      "format_on_save": "on",
      "tab_size": 2
    }
  },
  "lsp": {
    "eslint": {
      "settings": {
        "codeActionOnSave": {
          "rules": ["import/order"]
        }
      }
    }
  }
}
```

#### Spring Project Configuration (`.zed/settings.json`)

This configuration uses the built-in language server for formatting Java, XML, and YAML files.

```JSON
{
  "languages": {
    "Java": {
      "formatter": "language_server",
      "format_on_save": "on",
      "tab_size": 4,
      "preferred_line_length": 120
    },
    "XML": {
      "formatter": "language_server",
      "format_on_save": "on",
      "tab_size": 2
    },
    "YAML": {
      "formatter": "language_server",
      "format_on_save": "on",
      "tab_size": 2
    }
  }
}
```

#### Additional Project Files

Ensure consistency by including these configuration files in your version control:

- **React:** `.prettierrc`, `.eslintrc.js`, and `package.json` with dev dependencies.
- **Spring:** `checkstyle.xml` (if used), `.editorconfig`.

#### Team Setup

1. Commit the `.zed` folder and all linter/formatter config files to version control.
2. Document that team members need to install required tools (e.g., Prettier, ESLint).
3. Ensure external formatters are available in the system's `PATH`.

### I heard about a formatter named Biome that is much faster than Prettier. Can I use it for a project with TypeScript, React, Java, and Kotlin?

Yes, you can use Biome, but its language support is currently focused on web technologies.

#### What is Biome?

Biome is an all-in-one toolchain for the web, integrating formatting, linting, and import sorting. It is written in Rust and is significantly faster (25-35x) than traditional tools like Prettier and ESLint.

#### Language Support

- **Fully Supported:** TypeScript, React (JSX/TSX), JavaScript, JSON/JSONC.
- **Not Supported:** Java, Kotlin.

Since Biome does not support Java or Kotlin, you will need to use other formatters for your backend code.

#### Recommended Hybrid Approach

For a multi-language project, a hybrid approach is best:

- **Frontend (React/TypeScript):** Use Biome to take advantage of its superior performance.
- **Backend (Java/Kotlin):** Continue using language-specific formatters.

This allows you to gain performance benefits where Biome is supported while maintaining consistent formatting across your entire backend stack.

### Can I use Biome for TypeScript/React and Prettier for Java/Kotlin in the same project?

Yes, this is the optimal approach for a multi-language project. You can configure your editor to use different formatters based on the file's language.

- **Biome's Strengths:** Provides high-performance formatting and linting for TypeScript, React/JSX, JavaScript, and JSON, with 97% Prettier compatibility.
- **Prettier's Strengths:** Through plugins like `prettier-plugin-java` and `prettier-plugin-kotlin`, it offers reliable formatting for backend languages.

This hybrid setup gives you the best of both worlds: Biome's speed for frontend development and Prettier's extensive language support for the backend.

### How do I set up Prettier and Biome for my backend and frontend projects across Cursor, Zed, and IntelliJ?

Here is a guide to setting up a hybrid formatting environment (Biome for frontend, Prettier for backend) in each editor.

#### Frontend Project Setup (React/TypeScript with Biome)

1. **Install & Initialize Biome:**
    
    ```
    npm i -D --save-exact @biomejs/biome
    npx @biomejs/biome init
    ```
    
2. **Configure `biome.json`:**

	```JSON
    {
      "$schema": "https://biomejs.dev/schemas/1.8.3/schema.json",
      "organizeImports": { "enabled": true },
      "formatter": {
        "enabled": true,
        "indentStyle": "space",
        "lineWidth": 120
      },
      "javascript": {
        "formatter": {
          "quoteStyle": "single",
          "trailingComma": "es5"
        }
      },
      "linter": {
        "enabled": true,
        "rules": { "recommended": true }
      }
    }
    ```
    

#### Backend Project Setup (Java/Kotlin with Prettier)

1. **Install Prettier & Plugins:**
    
    ```Bash
    npm i -D prettier prettier-plugin-java prettier-plugin-kotlin
    ```
    
2. **Configure `.prettierrc`:**
    
    ```JSON
    {
      "tabWidth": 4,
      "printWidth": 120,
      "plugins": ["prettier-plugin-java", "prettier-plugin-kotlin"]
    }
    ```
    

#### Editor Configurations

##### Cursor (VS Code)

- **Frontend (`.vscode/settings.json`):**
    
    ```JSON
    {
      "editor.formatOnSave": true,
      "editor.defaultFormatter": "biomejs.biome"
    }
    ```
    
- **Backend (`.vscode/settings.json`):**
    
    ```JSON
    {
      "editor.formatOnSave": true,
      "editor.defaultFormatter": "esbenp.prettier-vscode",
      "[java]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
      },
      "[kotlin]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
      }
    }
    ```
    

##### Zed

- **Frontend (`.zed/settings.json`):**
    
    ```JSON
    {
      "languages": {
        "TypeScript": { "formatter": "biome", "format_on_save": "on" },
        "TSX": { "formatter": "biome", "format_on_save": "on" },
        "JavaScript": { "formatter": "biome", "format_on_save": "on" },
        "JSX": { "formatter": "biome", "format_on_save": "on" }
      }
    }
    ```
    
- **Backend (`.zed/settings.json`):**
    
    ```JSON
    {
      "languages": {
        "Java": {
          "formatter": { "external": { "command": "prettier", "arguments": ["--stdin-filepath", "{buffer_path}"] } },
          "format_on_save": "on"
        },
        "Kotlin": {
          "formatter": { "external": { "command": "prettier", "arguments": ["--stdin-filepath", "{buffer_path}"] } },
          "format_on_save": "on"
        }
      }
    }
    ```
    

##### IntelliJ

- **Frontend (React/TypeScript):**
    
    1. Install the **Biome** plugin from the marketplace.
    2. Navigate to `Settings → Languages & Frameworks → Biome Settings`.
    3. Enable **LSP-based Code Formatting**.
    4. Enable format on save via `Settings → Actions on Save → Reformat Code`.
- **Backend (Java/Kotlin):**
    
    1. Install the **Prettier** plugin.
    2. Go to `Settings → Languages & Frameworks → JavaScript → Prettier`.
    3. Select **Automatic Prettier configuration**.
    4. Check the boxes for **On code reformatting** and **On save**. IntelliJ will automatically detect your `.prettierrc` and its plugins.

---

### Summary

#### Formatting Strategy

- **Project-Specific Settings**: Use a `.zed/settings.json` file in each project's root to enforce shared formatting rules.
- **Version Control**: Commit editor configurations (`.zed`, `.vscode`), formatter configurations (`.prettierrc`, `biome.json`), and linter rules (`.eslintrc.js`) to your repository.

#### Tooling: Biome vs. Prettier

- **Biome**: An extremely fast, all-in-one tool (formatter, linter) for web technologies.
    - **Supported**: JavaScript, TypeScript, JSX/TSX, JSON.
    - **Not Supported**: Java, Kotlin.
- **Prettier**: A widely-used formatter with extensive language support via plugins.
    - **Supported**: Java (via `prettier-plugin-java`), Kotlin (via `prettier-plugin-kotlin`), and many others.

#### Recommended Hybrid Implementation

- **Frontend (React/TypeScript)**: Use **Biome** for maximum performance in formatting and linting.
- **Backend (Java/Kotlin)**: Use **Prettier** with its language-specific plugins for reliable formatting.

#### Editor Setup

- **Cursor / VS Code**: Set the default formatter per workspace (`"editor.defaultFormatter"` in `settings.json`) and install the `Biome` and `Prettier` extensions.
- **Zed**: Configure formatters per language in `.zed/settings.json`, using the built-in `biome` formatter and an `external` command for Prettier.
- **IntelliJ**: Install the `Biome` and `Prettier` plugins from the marketplace and configure them in settings to format on save automatically.