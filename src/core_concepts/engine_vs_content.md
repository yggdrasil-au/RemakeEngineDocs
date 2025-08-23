# Engine vs. Content Model

The engine provides generic functionality: menu handling, placeholder expansion and operation execution. Game-specific logic lives in **modules** under `RemakeRegistry/Games/`.

Separating engine and content ensures that new games can be added without modifying core code. Modules define operations and scripts while the engine remains stable.
