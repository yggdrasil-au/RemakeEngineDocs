# Writing Game-Specific Scripts

Scripts executed by operations should be small and focused. Use `argparse` to handle arguments and exit with non‑zero status on failure.

Scripts can rely on placeholders expanded by the engine, allowing paths and options to be passed in dynamically.
