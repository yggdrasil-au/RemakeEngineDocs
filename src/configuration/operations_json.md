# Game Configuration (`operations.json`)

Each game module contains an `operations.json` file describing the actions available for that title. The top-level object maps the game display name to a list of operation objects.

```json
{
  "Example Game (PS3)": [
    { "Name": "Extract Archives", "script": "Scripts/extract_archives.py" }
  ]
}
```
