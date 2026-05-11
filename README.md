[![Java Version](https://img.shields.io/badge/Java-21-blue.svg)](https://adoptium.net/)
[![Burp Suite](https://img.shields.io/badge/Burp%20Suite-Compatible-orange.svg)](https://portswigger.net/burp)

# 🐍 Snake-burp: Use burp suite as gaming emulator 

## 📋 Table of Contents
- [🎯 Overview](#-overview)
- [✨ Features](#-features)
- [🎯 Why?](#-why)
- [📁 Project Structure](#-project-structure)
- [🚀 Installation](#-installation)
- [🎮 How to Play](#-how-to-play)
- [🏗️ Architecture](#️-architecture)
- [📊 Technical Specifications](#-technical-specifications)
- [Screenshots](#screenshots)
- [Support Development](#support-development)

## 🎯 Overview
**snake-burp** is a fully playable Snake game that runs inside Burp Suite! Take a break from security testing and enjoy some nostalgic gaming directly in your favorite web security tool.

## ✨ Features

- **Classic Snake Gameplay** - Control a snake, eat food, grow longer, avoid colliding with yourself
- **Multiple Difficulty Levels** - Easy (200ms), Medium (130ms), Hard (75ms) base speeds
- **Wrap Mode Toggle** - Screen wrapping for endless movement or wall collision for classic mode
- **Progressive Difficulty** - Game speeds up every 5 food items (minimum 40ms interval)
- **High Score Tracking** - Persists during the session
- **Visual Polish** - Smooth rendering with directional snake eyes, fading body segments, glowing food
- **Keyboard Controls** - Multiple key bindings for convenience
- **Burp Theme Compatible** - Dark color scheme that fits Burp Suite's aesthetic

## 🎯 Why?

Because even security researchers need a break sometimes! Plus, it demonstrates how to build a complete, interactive game as a Burp extension with proper separation of concerns, event handling, and UI rendering.

## 📁 Project Structure

```
snake-burp/
├── pom.xml
└── burp/
    ├── BurpExtender.java                  ← Burp entry point (IBurpExtender)
    ├── engine/
    │   └── GameEngine.java                ← Pure game logic, no Swing
    ├── input/
    │   └── InputHandler.java              ← KeyAdapter → game commands
    ├── model/
    │   ├── Difficulty.java                ← Enum: EASY / MEDIUM / HARD
    │   ├── Direction.java                 ← Enum: UP / DOWN / LEFT / RIGHT
    │   ├── Food.java                      ← Food placement logic
    │   ├── GameState.java                 ← Enum: WAITING / RUNNING / PAUSED / GAME_OVER
    │   ├── Point.java                     ← Immutable grid coordinate
    │   └── Snake.java                     ← Snake body (deque) + movement
    └── ui/
        ├── SnakeGamePanel.java            ← Rendering (Graphics2D, paintComponent)
        └── SnakeTab.java                  ← ITab, Timer, top bar controls
```

## 🚀 Installation

### Prerequisites
- Java 21 or higher
- Burp Suite Community/Professional Edition

### Quick Install (Pre-built JAR)
1. Download the latest `snake-burp-v1.0.0.jar` from the [Releases](https://github.com/VermaOps/snake-burp/releases) page
2. Open Burp Suite → Extender → Extensions
3. Click "Add"
4. Set Extension Type to "Java"
5. Select the downloaded JAR file
6. Click "Next" to load

### Build from Source

#### Using the Build Script (macOS/Linux)
```bash
# Make the script executable
chmod +x build.sh

# Run the build script
./build.sh

# The JAR will be in target/snake-burp-v1.0.0.jar
```

The build script automatically:
- Locates your Burp Suite installation
- Compiles all Java files
- Creates the JAR with proper manifest
- Provides loading instructions

## 🎮 How to Play

### Controls
| Action | Keys |
|--------|------|
| Move Up | ↑ or W |
| Move Down | ↓ or S |
| Move Left | ← or A |
| Move Right | → or D |
| Pause/Resume | P or ESC |
| Restart | R |
| Start/Restart | ENTER (when game not running) |

### Game Rules
- Control the snake to eat the red food pellets
- Each food increases your score and snake length
- Game ends if you collide with yourself
- Toggle "Wrap Mode" to wrap around screen edges instead of dying
- Game speed increases every 5 food items eaten

### Interface
- **Top Bar**: Score, high score, speed level, difficulty selector, wrap mode toggle, support button
- **Game Board**: 40×25 grid with smooth rendering
- **Overlays**: Clear visual feedback for waiting, paused, and game over states

## 🏗️ Architecture

The project follows clean architecture principles:

```
BurpExtender (Entry Point)
    ↓
SnakeTab (UI Container) ←→ GameEngine (Game Logic)
    ↓                          ↓
SnakeGamePanel (Renderer) ←→ Snake/Food/Point (Models)
    ↓
InputHandler (Key Events)
```

### Key Components

| Component | Purpose |
|-----------|---------|
| **BurpExtender** | Burp integration entry point |
| **SnakeTab** | Main UI container implementing Burp's ITab |
| **GameEngine** | Pure game logic with no UI dependencies |
| **SnakeGamePanel** | Custom rendering component |
| **InputHandler** | Keyboard input processing |
| **Model Classes** | Snake, Food, Point, enums for game state |

### Design Highlights
- **Separation of Concerns** - Clear boundaries between logic, UI, and input
- **Event-Driven** - Game loop driven by Swing Timer, UI updates via listener pattern
- **Immutability** - Point class is immutable for safe sharing
- **Input Buffering** - Direction changes queued to prevent illegal moves
- **Performance** - O(1) collision detection using HashSet

## 📊 Technical Specifications

| Parameter | Value |
|-----------|-------|
| Grid Size | 40 columns × 25 rows |
| Cell Size | 24×24 pixels |
| Board Size | 960×600 pixels |
| Speed Progression | +1 level every 5 food items, -10ms interval |
| Minimum Speed | 40ms (capped) |
| Difficulty Levels | Easy (200ms), Medium (130ms), Hard (75ms) |

## Screenshots
| | | 
|:---:|:---:|
| <img width="1470" height="923" alt="Screenshot 2026-02-23 at 9 14 36ΓÇ»AM" src="https://github.com/user-attachments/assets/200a7cc2-51c8-4377-b67f-98cf12f2856c" /> | <img width="1470" height="901" alt="Screenshot 2026-02-23 at 9 16 46ΓÇ»AM" src="https://github.com/user-attachments/assets/aab23932-d373-4126-919f-d165690a0e92" /> |


## Support Development

If snake-burp helps you relieve your tension, consider supporting its development:

**⭐ Star the Repository**: Show your support by starring the project on GitHub!

**Support Links**:
- 💰 **PayPal**: [PayPal](https://www.paypal.com/ncp/payment/7Y3836GETVF94)

Your support helps maintain the project and in creation of new tools.

---

<div align="center">

**Built with ❤️ by [VermaOps](https://github.com/VermaOps)**

[![GitHub Stars](https://img.shields.io/github/stars/VermaOps/snake-burp?style=social)](https://github.com/VermaOps/snake-burp/stargazers)
[![GitHub Issues](https://img.shields.io/github/issues/VermaOps/snake-burp)](https://github.com/VermaOps/snake-burp/issues)
[![GitHub Forks](https://img.shields.io/github/forks/VermaOps/snake-burp?style=social)](https://github.com/VermaOps/snake-burp/network/members)

**⭐ Star this repo if you find it useful for mobile security testing!**

</div>
