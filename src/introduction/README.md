# 📘 Introduction

Welcome to the **RemakeEngine** documentation!

This section introduces the core ideas behind the tool, explains what makes it powerful and flexible, and shares the philosophy that guides its development.

---

## 📦 1.1 What is RemakeEngine?

**RemakeEngine** is a modular reverse-engineering and asset processing tool built to assist in the extraction, analysis, and transformation of data from console games — specifically PlayStation 2 and PlayStation 3 titles.

It provides a structured way to work with proprietary game formats and assets, supporting workflows like:
- Game modding and custom asset replacement
- File format analysis and reverse engineering
- Game archival and preservation
- Prototype remakes or fan restoration projects

RemakeEngine combines Python scripting, configurable operations, and clean project structure to give users the power to dive deep into game internals without sacrificing safety or clarity.

---

## 🧩 1.2 Key Features

RemakeEngine offers a variety of powerful and user-friendly features:

- **Multi-game support:** Switch between supported games with dedicated initialization and validation logic per title.
- **Safe file handling:** Choose between copying, moving, or working in-place with your source files.
- **Extensible architecture:** Easily add new extractors, converters, and operations without changing the core.
- **Asset conversion tools:** Supports unpacking and converting textures, models, audio, video, and archives.
- **Config persistence:** Automatically tracks selected game and paths via `project.json`.
- **Modder-friendly operations:** Rename folders, flatten structures, and clean up assets for easy editing.

---

## ⚙️ 1.3 How It Works

At its core, RemakeEngine is a **command-line toolkit** built around:

- A main menu interface (`main.py`) that allows game selection and operation execution.
- A `project.json` file to manage paths, game state, and validation status.
- Per-game **initialization scripts** to validate and prepare source data.
- A flexible `operations.json` that defines available actions for each supported title.
- A collection of Python modules for extracting and converting proprietary formats.

The system is **plug-and-play**: add a new game registry entry with scripts and configs, and the engine will dynamically support it.

Each operation runs as a modular script, meaning contributors can expand capabilities without editing core logic.

---

## 🤝 1.4 Community Focus

RemakeEngine was designed with **modders**, **developers**, and **preservationists** in mind. The goals are:

- **Transparency**: Clear, readable code and modular structure.
- **Respect for game content**: No piracy, no redistribution of proprietary files.
- **Empowerment**: Help communities document and improve classic games.
- **Collaboration**: Encourage contribution of new scripts, formats, and tools.

Whether you're digging through `.txd` texture bundles, rebuilding `.vp6` videos, or creating an HD asset mod, RemakeEngine provides a foundation to build on.

If you're part of a fan project or reverse engineering group, this tool was made for you.

---

🎉 Let’s get started!
