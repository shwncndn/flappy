# Flappy Bird

## Overview
- **Game Engine**: bracket_lib
- **Game Type**: Flappy Bird clone
- **Display**: 80x50 character terminal

## Code Structure

### GameMode Enum
There are three states that the game can occupy:

```rust
enum GameMode {
    Menu,    // Start screen
    Playing, // Active gameplay
    End,     // Game over screen
}

Playing:
- stub version instantly ends the game by setting program node to End
-

Main Menu:
- starts by clearing the screen and printing menu options
    - print_centered - an extended version of print()

### State
`self` is the game state (`State`)
- contains the game data (current mode, player position, etc)

### BTerm
the terminal/graphics interface from bracket-bracket_lib
- gives you access to the terminal and screen operators:
- provides methods to draw, get input, manage the screen, etc
- handles "how to display" for you

combined with the State of a current game (`self`) we can call the BTerm object's methods via the `ctx` reference:

- ctx.cls() - clears the screen
- ctx.print() - print text at coordinates
- ctx.print_centered() - printe centered text
etc.

#### Key
a variable included in the BTerm object that holds keyboard input state
- a player may (or may not) be pressing a key
- represented with an `Option`  type

#### if let shorthand
syntax for matching against a single case. Here, that case is `Some`
```
        if let Some(key) = ctx.key {
            match key {
                VirtualKeyCode::P => self.restart(),
                VirtualKeyCode::Q => ctx.quitting = true,
                _ -> {}
            }
```

- If user presses `P`, restart the game by calling the `restart()`
- If user presses `Q`, set `ctx.quitting == true` to terminate
* The `Option` type in Rust can only contain `Some(data)` or `None`

## Player

player is battling gravity and trying to avoid obstacles

### Player Struct
requires storing of the current game attributes of the bird:

```
struct Player {
x:i32,
y:i32,
velocity: f32
}
```

using float for velocity allows for fractional velocity
- allows for more precise falling

### Constructor

impl Player {
fn new(x: i32, y: i32) -> Self {
Player {
x,
y,
velocity: 0.0,
 }
}

```
   fn render(&mut self, ctx: &mut BTerm) {
        ctx.set(0, self.y, YELLOW, BLACK, to_cp437('@'));
    }
}
```

- set() - `bracket-lib` function that sets a single character on the screen
- x and y coordinates on the screen
- bracket-lib colors
- `to_cp347('@') converts unicode from source code to the matching Codepage 437 character number

* NOTE - 0 is the top of the screen



I. Setup

II. State Management

III. Player Entity

IV. Game Loop

V. Movement

VI. Lifecycle

graph TD
    A[Game Starts] --> B[Main Menu]
    B --> |Press P| C[Playing Game]
    B --> |Press Q| D[Quit Game]

    C --> E{Player Action}
    E --> |Press SPACE| F[Player Flaps Up]
    E --> |No Input| G[Player Falls Down]

    F --> H[Check Game State]
    G --> H

    H --> |Player Y > Screen Height| I[Game Over Screen]
    H --> |Player Still Alive| C

    I --> |Press P| J[Restart Game]
    I --> |Press Q| D

    J --> C

    style A fill:#e1f5fe
    style B fill:#fff3e0
    style C fill:#e8f5e8
    style I fill:#ffebee
    style D fill:#f3e5f5
