The RemakeEngine Main Menu Tool is a Python-based, interactive command-line interface (CLI) designed to streamline the setup, validation, and processing of game assets for reverse engineering and modding purposes. It supports multiple games through a dynamic and modular structure, enabling efficient handling of diverse file types and game-specific configurations.

🔧 Key Features:
🕹️ Game Selection
Detects and lists all available games from a central registry.

Prompts the user to select a game to work with.

Supports multiple platforms (e.g., PS2, PS3) and handles each game’s unique data layout.

⚙️ Initialization Workflow
Loads configuration from project.json.

Verifies or prompts for the source game directory.

Offers safe handling options:

Copy (recommended)

Move (destructive)

Direct use (modifies original files)

Validates essential subdirectories to ensure the data structure is compatible.

Updates project metadata dynamically based on user input and validation results.

📂 Operations Menu (Game-Specific)
After initialization, the user is presented with a tailored list of operations for the selected game. Examples include:

For The Simpsons Game (PS3):
Rename base folders

Validate game files

Extract archives (.STR)

Extract textures (.txd → .dds)

Validate extracted files

Convert models (.preinstanced → .blend)

Convert videos (.vp6 → .ogv)

Reorganize audio files

Convert audio (.snu → .wav)

Flatten folder structure

For The Simpsons Game (PS2):
Rename base folders

Extract archives (.ASR)

Extract archives (.EN)

Extract textures (.txd → .dds)

Convert models (.preinstanced → .blend)

Convert videos (.vp6 → .ogv)

Convert audio (AGV.wav → .wav)

Flatten folder structure

Game Switching
Easily switch between games without restarting the tool using the Change Game option.

Safe Exit
Use the Exit Tool option to terminate cleanly.

