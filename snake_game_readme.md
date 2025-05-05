
#  Console Snake Game in C++

A lightweight, cross-platform implementation of the classic Snake game in C++. This version runs entirely in the terminal and adapts input and rendering for both Windows and Linux environments using conditional compilation.

---

## 📋 Overview

This is a simple Snake game where the player controls a snake to collect fruits and grow in length while avoiding collisions with the snake's own body. It works on both **Windows** and **Linux** platforms with screen wrapping at the edges of the game area.

-  **Cross-Platform**: Works on both Windows and Linux.
-  **Real-time Input Handling** using `conio.h` (Windows) and `ncurses` (Linux).
- **Fruit Collection**: Eating fruit i.e the letter 'F' grows the snake and increases score.
- **Self-collision**: The game ends when the snake collides with itself.
-  **Edge Wrapping**: The snake reappears on the opposite side if it crosses the boundary.

---

## 🛠️ Requirements

### Windows
- C++ compiler (MSVC, MinGW, or similar)
- Standard Windows headers: `conio.h`, `windows.h`

### Linux
- C++ compiler (e.g., `g++`)
- `ncurses` library for handling terminal output and input:
  ```bash
  sudo apt install libncurses5-dev libncursesw5-dev
  ```

---

## ⚙️ Compilation

### On Windows:
To compile on Windows using `g++` (MinGW or similar):
```bash
g++ snake.cpp -o snake.exe
```

### On Linux:
To compile on Linux using `g++` with `ncurses` support:
```bash
g++ snake.cpp -o snake -lncurses
```

---

## 🚀 Controls

| Key | Action     |
|-----|------------|
| `W` | Move Up    |
| `A` | Move Left  |
| `S` | Move Down  |
| `D` | Move Right |
| `X` | Exit Game  |

---

## 📂 Code Structure

### Global Variables

```cpp
bool gameOver;
const int width = 40, height = 25;
int x, y, fruitX, fruitY, score;
int tailX[200], tailY[200];
int nTail;
enum eDirecton { STOP = 0, LEFT, RIGHT, UP, DOWN };
eDirecton dir;
```

- `x`, `y`: Coordinates of the snake's head.
- `fruitX`, `fruitY`: Coordinates of the fruit.
- `score`: Player's score.
- `tailX`, `tailY`: Arrays for tail segment coordinates.
- `nTail`: Number of tail segments.
- `dir`: Current direction (enum: `STOP`, `LEFT`, `RIGHT`, `UP`, `DOWN`).

---

## 🔧 Functions

### `void Setup()`
Initializes the game state:
- Snake's initial position in the center of the screen.
- Random fruit position.
- Score and tail length are set to 0.

```cpp
void Setup() {
    gameOver = false;
    dir = STOP;
    x = width / 2;
    y = height / 2;
    fruitX = rand() % width;
    fruitY = rand() % height;
    score = 0;
}
```

---

### `void Draw()`
Draws the game screen, including:
- Border using `#` symbols.
- Snake head (`O`), fruit (`F`), and tail (`o`).
- Current score displayed below the game area.

```cpp
void Draw() {
    system(CLEAR_SCREEN);
    for (int i = 0; i < width + 2; i++) cout << "#";
    cout << endl;

    for (int i = 0; i < height; i++) {
        for (int j = 0; j < width; j++) {
            if (j == 0) cout << "#";
            if (i == y && j == x) cout << "O";
            else if (i == fruitY && j == fruitX) cout << "F";
            else {
                bool print = false;
                for (int k = 0; k < nTail; k++) {
                    if (tailX[k] == j && tailY[k] == i) {
                        cout << "o";
                        print = true;
                    }
                }
                if (!print) cout << " ";
            }
            if (j == width - 1) cout << "#";
        }
        cout << endl;
    }

    for (int i = 0; i < width + 2; i++) cout << "#";
    cout << endl;
    cout << "Score: " << score << endl;
}
```

---

### `void Input()`
Handles keyboard input:
- On Windows: Uses `_kbhit()` and `_getch()`.
- On Linux: Uses `ncurses`'s `getch()` (non-blocking).

```cpp
void Input() {
#ifdef _WIN32
    if (_kbhit()) {
        switch (_getch()) {
#else
    int ch = getch();
    if (ch != ERR) {
        switch (ch) {
#endif
            case 'a': dir = LEFT; break;
            case 'd': dir = RIGHT; break;
            case 'w': dir = UP; break;
            case 's': dir = DOWN; break;
            case 'x': gameOver = true; break;
        }
    }
}
```

---

### `void Logic()`
Handles the game's logic:
- Moves the snake's head and tail.
- Checks for fruit collection, score update, and snake growth.
- Ends the game if the snake collides with its tail.
- Implements screen wrapping at the borders.

```cpp
void Logic() {
    int prevX = tailX[0], prevY = tailY[0];
    int prev2X, prev2Y;
    tailX[0] = x;
    tailY[0] = y;
    for (int i = 1; i < nTail; i++) {
        prev2X = tailX[i];
        prev2Y = tailY[i];
        tailX[i] = prevX;
        tailY[i] = prevY;
        prevX = prev2X;
        prevY = prev2Y;
    }

    switch (dir) {
        case LEFT: x--; break;
        case RIGHT: x++; break;
        case UP: y--; break;
        case DOWN: y++; break;
        default: break;
    }

    if (x >= width) x = 0; else if (x < 0) x = width - 1;
    if (y >= height) y = 0; else if (y < 0) y = height - 1;

    for (int i = 0; i < nTail; i++) {
        if (tailX[i] == x && tailY[i] == y) gameOver = true;
    }

    if (x == fruitX && y == fruitY) {
        score += 10;
        fruitX = rand() % width;
        fruitY = rand() % height;
        nTail++;
    }
}
```

---

## 🌐 Platform Handling

```cpp
#ifdef _WIN32
    #include <conio.h>
    #include <windows.h>
    #define CLEAR_SCREEN "cls"
    void sleep_ms(int ms) { Sleep(ms); }
#else
    #include <ncurses.h>
    #include <unistd.h>
    #define CLEAR_SCREEN "clear"
    void sleep_ms(int ms) { usleep(ms * 1000); }
#endif
```

---

## 🚧 Future Improvements

- Add multiple difficulty levels.
- Introduce obstacles or moving walls.
- Enable sound effects using platform-specific libraries.
- Save high scores to a file.
- Add restart menu.

---

## 📜 License

Licensed under the **MIT License**.

---

## Project By 
Farzan bhalara (202301248)
---
