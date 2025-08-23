# Example: Extract Archives

```json
{
  "Name": "Extract Game Archives",
  "script": "Scripts/extract_archives.py",
  "args": ["--source", "{{Game.RootPath}}/archives", "--dest", "Extracted"],
  "prompts": [
    {
      "Name": "confirmExtract",
      "type": "confirm",
      "message": "Proceed with extraction?",
      "default": true
    }
  ]
}
```
This operation runs a script to unpack archive files after asking the user for confirmation.
