# Defining Operations

Operations describe discrete workflow steps such as extraction, conversion or cleanup. Combine placeholders with prompts to create flexible actions.

Example:
```json
{
  "Name": "Convert Textures",
  "script": "Scripts/convert_textures.py",
  "args": ["--input", "{{Game.RootPath}}/TXD", "--output", "{{outputDir}}"],
  "prompts": [
    {"Name": "outputDir", "type": "text", "message": "Output folder"}
  ]
}
```
