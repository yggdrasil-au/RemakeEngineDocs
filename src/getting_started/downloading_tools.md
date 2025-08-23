# Downloading Tools (`download.py`)

Some game modules can fetch required utilities automatically. Run:
```bash
python download.py
```
This script reads the same configuration as `main.py` and downloads any tools defined with a `download` operation type. It is safe to run multiple times; existing files will be skipped unless `force` is enabled in the operation definition.
