# Directory Structure

A typical repository layout:

```
RemakeEngine/
├── Engine/                # core engine modules
├── RemakeRegistry/        # game modules
│   └── Games/
│       └── MyGame/
│           ├── operations.json
│           └── Scripts/
├── Tools/                 # third-party utilities
├── main.py
└── download.py
```

Game modules may include additional assets or data files required for operations.
