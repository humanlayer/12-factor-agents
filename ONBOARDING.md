## How to Add Output Schemas to Your Tool

### Why Output Schemas Matter

Output schemas enable clients—whether LLMs, frameworks, or developers—to know exactly what data to expect from a tool. This supports robust planning, validation, token efficiency, type safety, and reliable tool contracts for agentic systems.

### Stepwise Instructions

**Step 1:** Structure your tool’s output as data (e.g., a Python dict or JSON object).

**Step 2:** Write a JSON Schema describing your output’s structure (fields, types, required properties).

**Step 3:** Make the output schema discoverable—either by extending your tool’s discovery endpoint or by adding a `get_output_schema()` method.

**Step 4:** (If applicable) Register or document your output schema so it is visible to the framework and other contributors.

### Minimal Example

```python
# Example tool output
output = {
    "user": {
        "id": 123,
        "name": "Alice"
    }
}

# Example JSON Schema
output_schema = {
    "type": "object",
    "properties": {
        "user": {
            "type": "object",
            "properties": {
                "id": {"type": "integer"},
                "name": {"type": "string"}
            },
            "required": ["id", "name"]
        }
    },
    "required": ["user"]
}
```

To expose this schema, add a `get_output_schema()` method to your tool, or include the schema in your tool’s registration/discovery metadata.

### Guidance for Maintainers and Contributors

- Pair every new tool with an explicit, discoverable output schema.
- Review schemas during code review and documentation updates.
- Questions or feedback? Open an issue or pull request—help us improve clarity for all contributors.
