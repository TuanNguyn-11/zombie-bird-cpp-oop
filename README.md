# Zombie Bird

A 2D physics-based slingshot puzzle game written in **C++17** with **SFML 2.6**. Launch birds with special abilities to knock down wooden fortresses and wipe out every zombie in the level.

> Object-Oriented Programming (OOP) course project — **HCMC University of Technology and Education (HCM-UTE)**.

## Features

- **Custom 2D physics engine** (no Box2D): gravity, mass & inertia, restitution, friction, rotation, and an impulse-based contact solver with warm starting.
- **Accurate collision detection**: circle vs circle, circle vs rotated box (OBB), and box vs box using the Separating Axis Theorem.
- **Damage from impact force**: blocks and zombies lose health based on collision impulse.
- **Four bird types**, each with its own mid-air ability.
- **Data-driven levels** loaded from JSON files — add or edit levels without recompiling.
- **Trajectory preview** while aiming the slingshot.
- Main menu, level select, win and lose screens.

## Birds

| Bird | Color | Ability (Left Click / Space mid-flight) |
|------|-------|------------------------------------------|
| Basic Bird | Red | None — the classic, reliable bird |
| Dash Bird | Blue | Dashes forward at high speed |
| Explosive Bird | Black | Explodes, dealing area damage |
| Split Bird | Yellow | Splits into three smaller birds |

## Controls

| Action | Input |
|--------|-------|
| Aim | Left click & drag on the slingshot |
| Launch | Release left mouse button |
| Use ability | Left click or `Space` while the bird is flying |
| Navigate menus | Left click or `Enter` |

**Win** by destroying all zombies. **Lose** if you run out of birds first.

## OOP Design

```
Entity (abstract: update, draw)
├── Bird (abstract: onAbility)
│   ├── BasicBird
│   ├── DashBird
│   ├── ExplosiveBird
│   └── SplitBird
├── Pig    (zombie)
└── Block

IState (interface: onEnter, onExit, handleEvent, update, draw)
├── MainMenuState
├── LevelSelectState
├── GameplayState
├── WinState
└── LoseState
```

- **Inheritance & polymorphism** — `GameplayState` treats every bird as a `Bird*` and calls `onAbility()` without knowing the concrete type.
- **Abstraction & encapsulation** — physics data lives in `PhysicsBody`; `PhysicsWorld` and `CollisionSystem` hide all the math from gameplay code.
- **State pattern** — `StateManager` keeps a stack of `IState` screens.
- **Singleton** — `ResourceManager` loads textures and fonts once and shares them.

## Project Structure

```
.
├── CMakeLists.txt
├── run_game.bat          # One-click build & run (MSYS2 / UCRT64)
├── Data/
│   ├── Fonts/            # Open Sans
│   ├── Levels/           # level1.json, level2.json, level3.json
│   └── object/           # Sprites and backgrounds
└── src/
    ├── main.cpp
    ├── core/             # PhysicsBody, PhysicsWorld, CollisionSystem, ResourceManager
    ├── entities/         # Entity, Block, Pig, birds/
    ├── level/            # LevelLoader (+ nlohmann/json)
    ├── states/           # IState, StateManager, game screens
    └── ui/               # Slingshot, TrajectoryLine
```

## Getting Started

### Play (Windows)

Download the latest `.zip` from the [Releases](../../releases) page, extract it, and run `ZombieBirds.exe`. Keep the `Data/` folder next to the executable.

### Build from source

**Requirements:** a C++17 compiler, CMake ≥ 3.10, SFML 2.5+.

**Windows (MSYS2 UCRT64)** — just double-click `run_game.bat`. It installs GCC, CMake, Make and SFML through `pacman`, builds the project, and launches the game.

**Manual build:**

```bash
cmake -S . -B build
cmake --build build
./ZombieBirds        # the executable is placed in the project root
```

On Windows with MinGW, use `cmake -S . -B build -G "MinGW Makefiles"`, and copy the SFML / MinGW runtime DLLs next to `ZombieBirds.exe` if they are not on your `PATH`.

## Level Format

Levels are JSON files in `Data/Levels/`:

```json
{
  "name": "Easy Meadow",
  "background": "lvl1",
  "birds": ["basic", "dash", "explosive", "split"],
  "pigs":   [{ "x": 750, "y": 672.5 }],
  "blocks": [{ "type": "wood", "x": 722.5, "y": 655, "w": 17.5, "h": 70 }]
}
```

## Team

- Trọng Nhân
- Tuấn Nguyên
- An Nam
- Quang Minh
- Trung Nguyên

## Acknowledgements

- [SFML](https://www.sfml-dev.org/) — graphics and windowing
- [nlohmann/json](https://github.com/nlohmann/json) — JSON parsing
- [Open Sans](https://fonts.google.com/specimen/Open+Sans) — font (Apache License 2.0)

## License

This project is licensed under the [MIT License](LICENSE).
