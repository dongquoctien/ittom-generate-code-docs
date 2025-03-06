# IT-Tom Generate Code

A powerful VSCode extension that streamlines code generation through customizable templates and JSON-driven content generation. Perfect for developers looking to automate repetitive coding tasks and maintain consistent code structure across projects.

## ✨ Features

- 🎯 **Template Creation**: Build reusable templates from existing code structures
- 🔄 **Code Generation**: Generate code from templates using JSON data input
- 📝 **Smart Text Formatting**: Support multiple text cases (kebab, camel, pascal, snake)
- 🎨 **Dynamic Content Generation**: Auto-generate model properties and HTML elements based on JSON data types
- 🔍 **Targeted Updates**: Insert code at specific locations using markers
- 📁 **Structure Preservation**: Maintains directory structure when creating templates

## 🚀 Getting Started

### Installation

1. Launch Visual Studio Code
2. Open the Extensions view (`Ctrl+Shift+X` or `Cmd+Shift+X`)
3. Search for "IT-Tom Generate Code"
4. Click Install

### Basic Usage

#### Creating Templates

1. **New Template**:
   - Select file(s)/folder(s) in the explorer
   - Right-click → "IT-Tom Build Template"
   - Enter project name
   - Template is created in `_projectname_template` folder

2. **Update Existing Template**:
   - Select text to use as marker
   - Right-click → "IT-Tom Set Key For Update"
   - Choose template to update

#### Generating Code

1. **Using JSON**:
   - Select JSON text (optional)
   - Right-click → "IT-Tom Generate Code"
   - Enter JSON if not selected
   - Select template
   - Fill in placeholders

## 📖 Template Configuration

### Key Configuration Files

#### 1. `_pathFiles.json`
Defines file handling rules:

```json
[
  {
    "pathTemplate": "src/models/user.ts",
    "pathGenerate": "src/models/${camel-area}/${camel-model}.ts",
    "actionType": "create"
  },
  {
    "pathUpdate": "src/app.module.ts",
    "actionType": "update",
    "updateConfigs": [{
      "type": "before",
      "key": "// <insert-modules-here>",
      "contentInsert": "import { ${pascal-model}Module } from './${kebab-model}/${kebab-model}.module';",
      "isCheckExistNotUpdate": true
    }]
  }
]
```

#### 2. `_displaySetting.json`
Defines code generation patterns:

```json
{
  "display": {
    "model": [
      { "dataType": "String", "convert": "public @{fieldName}: string;" },
      { "dataType": "Number", "convert": "public @{fieldName}: number;" }
    ],
    "html": [
      { "dataType": "String", "convert": "<input name=\"@{fieldName}\" type=\"text\">" },
      { "dataType": "Number", "convert": "<input name=\"@{fieldName}\" type=\"number\">" }
    ]
  }
}
```

## 🎯 Template Tags

### Display Tags
Use in templates for dynamic code generation:

```typescript
export class ${pascal-model} {
    @{display.model-for}  // Generates properties from JSON
    
    constructor() {
        // Constructor logic
    }
}
```

### Text Format Tags

| Format Tag | Input Example | Output |
|------------|---------------|---------|
| `${kebab-text}` | user profile | `user-profile` |
| `${camel-text}` | user profile | `userProfile` |
| `${pascal-text}` | user profile | `UserProfile` |
| `${snake-text}` | user profile | `user_profile` |
| `${upper-text}` | user profile | `USER_PROFILE` |
| `${lower-text}` | user profile | `user_profile` |

## 🔧 Action Types

### Create Mode
- Purpose: Generate new files
- Configuration: Uses `pathTemplate` and `pathGenerate`
- Features: Supports placeholder substitution

### Update Mode
- Purpose: Modify existing files
- Configuration: Uses `pathUpdate` and `updateConfigs`
- Features: 
  - Insert before/after markers
  - Optional duplicate check (`isCheckExistNotUpdate`)

## 📋 Requirements

- Visual Studio Code 1.97.0 or higher
- Workspace with write permissions

## 🐛 Issue Reporting

Found a bug or have a suggestion? Please report it on our [GitHub repository](https://github.com/dongquoctien/ittom-generate-code-docs).

## 📝 Release Notes

### 0.0.1
- Initial release
- Template generation system
- JSON-based code generation
- Multiple text format support
- Dynamic content generation

## 🤝 Contributing

We welcome contributions! Please feel free to submit pull requests.

## 📄 License

This extension is licensed under the [MIT License](LICENSE).
