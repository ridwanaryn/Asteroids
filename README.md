# Asteroids

> A small, keyboard-controlled arcade game built with Python and Pygame.

Pilot a triangular spaceship through an endless field of incoming asteroids. Rotate
and thrust around the screen, fire shots, split larger asteroids into smaller ones,
and avoid collisions for as long as possible.

## Features

- Player ship with rotation and forward/reverse movement
- Keyboard-controlled shooting with a short firing cooldown
- Asteroids that spawn from the edges of the screen
- Constant-velocity asteroid movement
- Larger asteroids split into two faster, smaller asteroids when shot
- Circle-based collision detection for the player, asteroids, and shots
- Game-over handling with event and state logging in JSON Lines format
- Sprite groups for coordinated updates and rendering

## Tech stack

| Technology | Purpose |
| --- | --- |
| Python 3.13+ | Application language |
| [Pygame](https://www.pygame.org/) 2.6.1 | Window management, input, timing, drawing, and sprites |
| `pygame.sprite.Group` | Update and render lifecycle management |
| `pygame.Vector2` | Position, velocity, rotation, and collision math |
| `uv` or `pip` | Dependency and virtual-environment management |

## How the game works

The game uses a simple frame-based loop:

1. Pygame processes window events.
2. Every object in the `updatable` group receives the current frame delta time
   (`dt`).
3. The player, asteroids, and shots update their positions or rotation.
4. Collision checks run:
   - Player + asteroid: logs `player_hit`, prints **Game over!**, and exits.
   - Shot + asteroid: logs `asteroid_shot`, removes the shot, and splits the asteroid.
5. Every object in the `drawable` group is rendered.

The player and all circular game objects share the `CircleShape` base class.
Collision detection compares the distance between two centers with the sum of
their radii. Asteroids larger than the minimum radius split into two smaller
asteroids with diverging, slightly faster velocities.

## Controls

| Key | Action |
| --- | --- |
| `W` | Move forward |
| `S` | Move backward |
| `A` | Rotate left |
| `D` | Rotate right |
| `Space` | Fire |
| Close window | Quit |

## Requirements

- Python **3.13 or newer**
- A desktop environment capable of opening a Pygame window
- Internet access the first time dependencies are installed

## Run on another computer

### Option A: Using `uv` (recommended)

Install `uv` by following the instructions in the
[uv documentation](https://docs.astral.sh/uv/getting-started/installation/).
Then clone the project and run:

```bash
git clone https://github.com/ridwanaryn/Asteroids.git
cd Asteroids
uv sync
uv run python main.py
```

`uv sync` creates or updates the project environment and installs the exact
Pygame version declared in `pyproject.toml`.

### Option B: Using Python and `pip`

Clone the project, create a virtual environment, install the dependency, and
start the game:

```bash
git clone https://github.com/ridwanaryn/Asteroids.git
cd Asteroids

python3 -m venv .venv
source .venv/bin/activate       # macOS/Linux
# .venv\Scripts\activate        # Windows PowerShell

python -m pip install --upgrade pip
python -m pip install pygame==2.6.1
python main.py
```

On Windows, use the `py` launcher instead of `python3` if that is how Python is
installed:

```powershell
py -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install pygame==2.6.1
python main.py
```

## Project structure

```text
.
├── main.py          # Pygame initialization and main game loop
├── player.py        # Player movement, rotation, shooting, and rendering
├── asteroid.py      # Asteroid movement, rendering, and splitting
├── asteroidfield.py # Timed asteroid spawning from screen edges
├── shot.py          # Projectile movement and rendering
├── circleshape.py   # Shared sprite and collision behavior
├── constants.py     # Gameplay and screen configuration
└── logger.py        # JSONL state and event logging
```

## Runtime logs

The game may generate these local files while running:

- `game_state.jsonl` — periodic snapshots of screen and sprite state
- `game_events.jsonl` — gameplay events such as shots and collisions

They are intentionally ignored by Git and can be deleted safely between runs.

## Troubleshooting

- **`pygame` cannot be imported:** activate the virtual environment or run the
  command through `uv run`.
- **The window does not open:** run the game in a graphical desktop session;
  headless environments need a configured SDL video driver.
- **Controls feel too fast or slow:** adjust the player and asteroid constants in
  `constants.py`.

## License

This project is a learning-oriented game prototype. Add a license file before
redistributing it if you intend to publish or reuse the code.