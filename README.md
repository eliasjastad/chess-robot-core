# Bachelor Thesis: Automatic Chess Robot (Core Classes)

This repository contains selected **core classes** from my bachelor’s thesis project, where we developed an **automatic chess-playing robot** capable of playing against a human opponent with adjustable difficulty.  

The robot system combines **computer vision, artificial intelligence, robotics, and system integration**.  
This repo does not include the full project (≈500MB with binaries and datasets, and over 3000 lines of code spread across 20 classes), but instead highlights the most important and representative components.  

---

## Overview
The system works as follows:  
1. A mounted camera captures the chessboard.  
2. A **CNN model** classifies each square (piece type or empty).  
3. The current position is translated into FEN notation.  
4. **Stockfish engine** is used for move generation and evaluation.  
5. **Robot waypoints** are generated and sent to a UR5e robotic arm.  
6. A **C# GUI** allows the player to interact with the system and manage games.  

---

## Technologies
- **C# / WinForms** – game loop, GUI, Stockfish communication, robot integration  
- **Python (PyTorch)** – CNN training for board-square classification and robot communication 
- **Stockfish** – chess engine for move generation and evaluation  
- **UR5e robot** – robotic arm for physical piece manipulation  
- **Database (PostgreSQL)** – storing game history (not included here)  

---

## Core Components

### `FormBotPlayPage.cs`
- Implements the main game loop in a C# WinForms application.  
- Handles **player vs. bot turns**, **FEN updates**, **Stockfish moves**, and **database logging**.  
- Integrates robot control by sending moves through `RobotWaypointsFinder`.  

### `RobotWaypointsFinder.cs`
- Converts **UCI chess moves** into **UR5e robot waypoints**.  
- Supports all special moves: **castling, en passant, promotions, captures**.  
- Exports waypoints as JSON and executes them through a Python bridge.  

### `CombinedStockfishHandler.cs`
- Unified class for communicating with the **Stockfish chess engine**.  
- Supports move generation, applying moves to FEN, evaluation, and stalemate detection.  

### `Training_loop_colab.py`
- PyTorch script for training a CNN to classify chessboard squares (empty or containing a piece).  
- Uses EfficientNet-B0 modified for grayscale 130x130 input.  
- Implements **weighted sampling, early stopping, ReduceLROnPlateau scheduler**, and test evaluation.
- Trained on over 12000 self-made pictures (no augmentation), with 100% test accuracy on 1700 pictures (easy but realistic variations in light and angles)

---

## Results
- The CNN achieved accurate classification of board squares, enabling reliable board state reconstruction.  
- The UR5e robot was able to execute both normal and special chess moves.  
- All games could be played fully from start to finish against Stockfish at adjustable difficulty.  

---

## Notes
- This repo is a **showcase of core code** only.  
- Full dataset, binaries, and GUI assets are not included due to size (>500MB).  
- Paths to Stockfish executable and datasets are hardcoded in places, as this is extracted from the original bachelor project, and not generalized due to time limitations.  
