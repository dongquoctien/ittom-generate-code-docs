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

### **Structure:**
- `pathTemplate`: Source file path in template directory
- `pathGenerate`: Target path for new file generation (supports placeholders)
- `pathUpdate`: Path to existing file to modify
- `actionType`: "create" or "update"
- `updateConfigs`: Array of update rules containing:
  - `type`: Position to insert ("before"/"after" the key)
  - `key`: Marker text in file
  - `contentInsert`: Content to insert (supports placeholders)
  - `isCheckExistNotUpdate`: Prevents duplicate content if true


Defines file handling rules. Each entry can be either a "create" or "update" configuration:

##### Create Configuration
```json
{
  "pathTemplate": "src/models/user.ts",     // Source template file path
  "pathGenerate": "src/models/@{camel-area}/@{camel-model}.ts",  // Target generation path
  "actionType": "create"    // Specifies file creation
}
```

##### Update Configuration
```json
{
  "pathUpdate": "src/app.module.ts",   // Target file to update
  "actionType": "update",              // Specifies file update
  "updateConfigs": [{
    "type": "before",                  // Insert position: "before" or "after"
    "key": "// <insert-modules-here>", // Marker for insertion point
    "contentInsert": "import { @{pascal-model}Module } from './@{kebab-model}/@{kebab-model}.module';",
    "isCheckExistNotUpdate": true      // Skip if content exists
  }]
}
```

#### 2. `_displaySetting.json`

### **Structure:**
- `@fieldName`: The field name of the previously entered JSON object model.  
- `display`: A mandatory branch that must be declared, containing sub-branches used for rendering.  
  - `display.DisplayModel`: As shown in the example, `DisplayModel` can be (formHtmlInteger, form, formInteger, etc.).  

- **Structure of `DisplayModel`:**  
  - `nameUnique`: A unique name within the `DisplayModel[]` array, used when `convert` is declared as **@{display.DisplayModel}**.  
  - `dataType`: Supports `Integer`, `Float`, `String`, `Boolean`, `Date`, and `DateTime`.  
  - `convert`: The content to be converted to or selected (**@{display.DisplayModel}**).  
  - `followBySelect`: Used to track the selection of **@{display.DisplayModel}** as desired.


Example: Defines code generation patterns for different data types:

```json
{
  "__comment": "Key example @{<<fieldName>>}",
  "display": {
    "formHtml": [
      {
        "dataType": "String",
        "convert": "@{fieldName} render for string"
      },
      {
        "dataType": "Integer",
        "convert": "@{display.formHtmlInteger}"
      },
      {
        "dataType": "Boolean",
        "convert": ""
      },
      {
        "dataType": "DateTime",
        "convert": "public @{fieldName}: Date;"
      }
    ],
    "formHtmlInteger": [
      {
        "nameUnique": "SelectList",
        "dataType": "Integer",
        "convert": "          <tr>\n            <th scope=\"row\" class=\"th-bg\">@{lable-fieldName}</th>\n            <td>\n              <app-selectbox useAllOptionName=\"Select\" [options]=\"@{camel-fieldName}List\" dataKey=\"@{camel-fieldName}Value\" displayKey=\"@{camel-fieldName}Text\"  formControlName=\"@{camel-fieldName}\" class=\"flex-fill\" [class.valid-error]=\"checkIsInvalid('@{camel-fieldName}')\"></app-selectbox>\n            </td>\n          </tr>"
      },
      {
        "nameUnique": "Numberic",
        "dataType": "Integer",
        "convert": "      <tr>\n        <th scope=\"row\" class=\"th-bg\">@{lable-fieldName}</th>\n        <td>\n          <app-input-text formControlName=\"@{camel-fieldName}\" [isInvalid]=\"checkIsInvalid('@{camel-fieldName}')\" inputFormat=\"numberOnly\"></app-input-text>\n        </td>\n      </tr>"
      }
    ],
    "form": [
      {
        "dataType": "String",
        "convert": "public @{fieldName}: string;"
      },
      {
        "dataType": "Integer",
        "convert": "@{display.formInteger}",
        "followBySelect": "formHtml"
      },
      {
        "dataType": "Boolean",
        "convert": "public @{fieldName}: boolean;"
      },
      {
        "dataType": "Object",
        "convert": "public @{fieldName}: any;"
      },
      {
        "dataType": "Array",
        "convert": "public @{fieldName}: any[];"
      },
      {
        "dataType": "DateTime",
        "convert": "public @{fieldName}: Date;"
      }
    ],
    "formInteger": [
      {
        "nameUnique": "SelectList",
        "dataType": "Integer",
        "convert": "  @Input() select@{pascal-fieldName}: number;\n  @Input() @{camel-fieldName}List: any[];"
      }
    ]
  }
}
```


## 🎯 Template Tags

### Display Tags
Use in templates for dynamic code generation:

```typescript
export class @{pascal-model} {
    @{display.model-for}  // Generates properties from JSON
    
    constructor() {
        // Constructor logic
    }
}
```

### Text Format Tags

Input text supports the following formats: **"user profile"**, **"user-profile"**, **"user_profile"**, and **"userProfile"**.
| Format Tag | Input Text Example | Output | Description |
|------------|---------------|---------|-------------|
| `@{kebab-text}` | userProfile | `user-profile` | Lowercase with hyphens |
| `@{camel-text}` | user profile | `userProfile` | camelCase formatting |
| `@{pascal-text}` | user-profile | `UserProfile` | PascalCase formatting |
| `@{snake-text}` | user profile | `user_profile` | Lowercase with underscores |
| `@{upper-text}` | user profile | `USER_PROFILE` | Uppercase without separators |
| `@{lower-text}` | user profile | `user_profile` | Lowercase without separators |
| `@{label-text}` | user profile | `User Profile` | Lable case formatting |

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

Found a bug or have a suggestion? Please report it on our [GitHub repository](https://github.com/dongquoctien/ittom-generate-code-docs/issues).

## 📝 Release Notes

### 0.0.8
- Initial release
- Template generation system
- JSON-based code generation
- Multiple text format support
- Dynamic content generation

## 🤝 Contributing

We welcome contributions! Please feel free to submit pull requests.

## 📄 License

This extension is licensed under the [MIT License](LICENSE).
