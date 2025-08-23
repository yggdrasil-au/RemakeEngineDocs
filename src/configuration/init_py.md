# Using `init.py` Scripts

When a game is selected for the first time the engine looks for an `init.py` file in the game's directory. If present it is executed to validate data, build caches or perform one-time setup.

An `init.py` script should exit with `0` on success. Non-zero exit codes will prevent the game from being marked as initialised.
