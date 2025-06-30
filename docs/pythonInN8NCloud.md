# Python Code Node in n8n Workflow JSON Guide

This comprehensive guide provides thorough instructions on embedding Python code snippet nodes into JSON files for n8n workflow imports.

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Understanding n8n Workflow JSON Structure](#understanding-n8n-workflow-json-structure)
4. [Python Code Node Configuration](#python-code-node-configuration)
5. [Step-by-Step Implementation](#step-by-step-implementation)
6. [Code Examples](#code-examples)
7. [Best Practices](#best-practices)
8. [Troubleshooting](#troubleshooting)
9. [Advanced Configurations](#advanced-configurations)
10. [Importing and Testing](#importing-and-testing)

## Overview

n8n is a powerful, open-source workflow automation platform that supports Python code execution through its Code node. Starting from version 1.0, n8n introduced native Python support using Pyodide (a WebAssembly port of CPython), allowing you to embed Python code directly into workflows without requiring a separate Python installation.

### Key Features:
- **Native Python Support**: Execute Python code directly within n8n workflows
- **Pyodide Integration**: Uses WebAssembly for Python execution
- **Data Processing**: Access and manipulate workflow data using Python
- **Built-in Variables**: Pre-defined variables and methods for data access
- **JSON Workflow Export/Import**: Easy sharing and deployment of workflows

## Prerequisites

Before creating Python code nodes in n8n workflow JSON files, ensure you have:

- n8n version 1.0 or higher
- Basic understanding of JSON structure
- Python programming knowledge
- Familiarity with n8n workflow concepts
- Understanding of n8n data structure (array of objects)

## Understanding n8n Workflow JSON Structure

An n8n workflow JSON file contains the following main components:

### Basic Structure
```json
{
  "name": "Workflow Name",
  "nodes": [
    // Array of node objects
  ],
  "connections": {
    // Node connection definitions
  },
  "active": false,
  "settings": {},
  "versionId": "unique-version-id",
  "meta": {
    // Metadata
  },
  "id": "workflow-id",
  "tags": []
}
```

### Node Structure
Each node in the workflow has the following structure:
```json
{
  "parameters": {},
  "id": "unique-node-id",
  "name": "Node Display Name",
  "type": "node-type",
  "typeVersion": 1,
  "position": [x, y]
}
```

## Python Code Node Configuration

The Python Code node (`n8n-nodes-base.code`) has specific parameters for Python execution:

### Core Parameters
- **mode**: Execution mode (`runOnceForAllItems` or `runOnceForEachItem`)
- **language**: Set to `"python"` for Python code execution
- **code**: The actual Python code as a string

### Python Code Node JSON Structure
```json
{
  "parameters": {
    "mode": "runOnceForAllItems",
    "language": "python",
    "code": "# Your Python code here\nreturn _input.all()"
  },
  "id": "unique-node-id",
  "name": "Python Code Node",
  "type": "n8n-nodes-base.code",
  "typeVersion": 2,
  "position": [340, 300]
}
```

## Step-by-Step Implementation

### Step 1: Create Base Workflow Structure

Start with a basic workflow JSON structure:

```json
{
  "name": "Python Workflow Example",
  "nodes": [],
  "connections": {},
  "active": false,
  "settings": {},
  "versionId": "12345678-1234-1234-1234-123456789abc",
  "meta": {
    "templateCredsSetupCompleted": true
  },
  "id": "workflow-id",
  "tags": []
}
```

### Step 2: Add Trigger Node

Add a manual trigger node to start the workflow:

```json
{
  "parameters": {},
  "id": "trigger-node-id",
  "name": "Manual Trigger",
  "type": "n8n-nodes-base.manualTrigger",
  "typeVersion": 1,
  "position": [120, 300]
}
```

### Step 3: Configure Python Code Node

Add the Python code node with your specific code:

```json
{
  "parameters": {
    "mode": "runOnceForAllItems",
    "language": "python",
    "code": "# Python code implementation\nimport json\n\n# Access input data\ninput_data = _input.all()\n\n# Process data\nresult = []\nfor item in input_data:\n    processed_item = {\n        'json': {\n            'original_data': item['json'],\n            'processed_timestamp': str(_now),\n            'status': 'processed'\n        }\n    }\n    result.append(processed_item)\n\nreturn result"
  },
  "id": "python-code-node-id",
  "name": "Python Data Processor",
  "type": "n8n-nodes-base.code",
  "typeVersion": 2,
  "position": [340, 300]
}
```

### Step 4: Add Output Node

Include an output node to display results:

```json
{
  "parameters": {
    "options": {}
  },
  "id": "output-node-id",
  "name": "Display Results",
  "type": "n8n-nodes-base.noOp",
  "typeVersion": 1,
  "position": [560, 300]
}
```

### Step 5: Define Node Connections

Connect the nodes in the connections object:

```json
"connections": {
  "Manual Trigger": {
    "main": [
      [
        {
          "node": "Python Data Processor",
          "type": "main",
          "index": 0
        }
      ]
    ]
  },
  "Python Data Processor": {
    "main": [
      [
        {
          "node": "Display Results",
          "type": "main",
          "index": 0
        }
      ]
    ]
  }
}
```

## Code Examples

### Example 1: Basic Data Processing

```python
# Access input data
input_items = _input.all()

# Initialize result array
processed_items = []

# Process each item
for item in input_items:
    new_item = {
        'json': {
            'original': item['json'],
            'processed_at': str(_now),
            'item_count': len(input_items)
        }
    }
    processed_items.append(new_item)

return processed_items
```

### Example 2: Mathematical Operations

```python
# Mathematical processing example
import math

input_data = _input.all()
results = []

for item in input_data:
    if 'number' in item['json']:
        value = item['json']['number']
        processed = {
            'json': {
                'original_number': value,
                'square': value ** 2,
                'square_root': math.sqrt(abs(value)),
                'sine': math.sin(value),
                'is_even': value % 2 == 0
            }
        }
        results.append(processed)

return results
```

### Example 3: String Manipulation

```python
# String processing example
input_items = _input.all()
processed = []

for item in input_items:
    if 'text' in item['json']:
        text = item['json']['text']
        result = {
            'json': {
                'original_text': text,
                'uppercase': text.upper(),
                'lowercase': text.lower(),
                'word_count': len(text.split()),
                'character_count': len(text),
                'reversed': text[::-1]
            }
        }
        processed.append(result)

return processed
```

## Best Practices

### 1. Code Structure
- Always return an array of objects with 'json' key
- Use proper Python indentation
- Include error handling when necessary
- Comment your code for maintainability

### 2. Data Access
- Use `_input.all()` for all items (runOnceForAllItems mode)
- Use `_input.item` for single item processing (runOnceForEachItem mode)
- Access item data via `item['json']` notation

### 3. Built-in Variables
- `_input`: Access to input data
- `_now`: Current timestamp
- `_today`: Current date
- `_environment`: Environment variables

### 4. Error Handling
```python
try:
    # Your processing code
    result = process_data(_input.all())
    return result
except Exception as e:
    # Return error information
    return [{'json': {'error': str(e), 'success': False}}]
```

### 5. JSON Structure Compliance
Always ensure your Python code returns data in n8n's expected format:
```python
# Correct format
return [
    {'json': {'key': 'value'}},
    {'json': {'key': 'value2'}}
]

# Incorrect - will cause errors
return {'key': 'value'}  # Missing array and json wrapper
```

## Troubleshooting

### Common Issues and Solutions

#### 1. "JSON parameter need to be valid JSON"
- **Cause**: Malformed JSON in the code parameter
- **Solution**: Validate JSON syntax, especially escaping quotes and newlines

#### 2. Python Package Import Errors
- **Cause**: Trying to import packages not available in Pyodide
- **Solution**: Use only packages included with Pyodide or built-in Python modules

#### 3. Code Execution Timeouts
- **Cause**: Code taking too long to execute
- **Solution**: Optimize code, reduce data processing complexity

#### 4. Data Access Errors
- **Cause**: Incorrect data access patterns
- **Solution**: Use proper `_input` variable syntax

### Debugging Tips

1. **Use Print Statements**: Add `print()` statements for debugging (outputs to browser console)
2. **Start Simple**: Begin with basic data access before adding complex logic
3. **Test Incrementally**: Test each part of your code separately
4. **Validate Data Structure**: Ensure input data matches expected format

## Advanced Configurations

### Mode Selection

#### Run Once for All Items (Default)
```json
{
  "parameters": {
    "mode": "runOnceForAllItems",
    "language": "python",
    "code": "# Process all items at once\nreturn _input.all()"
  }
}
```

#### Run Once for Each Item
```json
{
  "parameters": {
    "mode": "runOnceForEachItem",
    "language": "python",
    "code": "# Process each item individually\nitem = _input.item\nreturn {'json': {'processed': True, 'data': item.json}}"
  }
}
```

### Advanced Python Examples

#### Data Aggregation
```python
# Aggregate data example
items = _input.all()
total = 0
count = 0

for item in items:
    if 'value' in item['json'] and isinstance(item['json']['value'], (int, float)):
        total += item['json']['value']
        count += 1

average = total / count if count > 0 else 0

return [{
    'json': {
        'total': total,
        'count': count,
        'average': average,
        'processed_at': str(_now)
    }
}]
```

#### Data Filtering
```python
# Filter data based on conditions
input_data = _input.all()
filtered_results = []

for item in input_data:
    data = item['json']
    
    # Apply filtering logic
    if data.get('status') == 'active' and data.get('score', 0) > 50:
        filtered_item = {
            'json': {
                'id': data.get('id'),
                'name': data.get('name'),
                'score': data.get('score'),
                'filtered_at': str(_now)
            }
        }
        filtered_results.append(filtered_item)

return filtered_results
```

## Importing and Testing

### Import Process

1. **Create JSON File**: Save your workflow JSON with a `.json` extension
2. **Open n8n**: Navigate to your n8n instance
3. **Import Workflow**: 
   - Use Ctrl+V to paste JSON directly into canvas, OR
   - Click menu (⋯) → "Import from File" → Select your JSON file
4. **Verify Connections**: Ensure all nodes are properly connected
5. **Test Execution**: Click "Test workflow" to verify functionality

### Validation Checklist

- [ ] JSON syntax is valid
- [ ] All node IDs are unique
- [ ] Node connections are properly defined
- [ ] Python code follows n8n data structure requirements
- [ ] Required credentials are configured (if applicable)
- [ ] Code handles edge cases and errors appropriately

### Testing Strategies

1. **Start with Sample Data**: Use manual trigger with test data
2. **Incremental Testing**: Test each node individually
3. **Error Scenarios**: Test with invalid/missing data
4. **Performance Testing**: Test with larger datasets
5. **Integration Testing**: Test full workflow end-to-end

## Conclusion

Embedding Python code nodes in n8n workflow JSON files enables powerful data processing capabilities within your automation workflows. By following the structured approach outlined in this guide, you can create robust, maintainable workflows that leverage Python's extensive capabilities while adhering to n8n's data structure requirements.

Remember to:
- Follow n8n's data structure conventions
- Use proper error handling
- Test thoroughly before deployment
- Document your code for future maintenance
- Leverage n8n's built-in variables and methods

This guide provides the foundation for creating sophisticated Python-powered n8n workflows that can be easily shared, version-controlled, and deployed across different environments.