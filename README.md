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
<img width="847" height="1129" alt="image" src="https://github.com/user-attachments/assets/800c3ba3-3cb7-4d2f-80c4-f8c02dc96692" />
<img width="1448" height="900" alt="image" src="https://github.com/user-attachments/assets/3cc971b9-6596-460a-ad95-ec26c7fce429" />

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
<img width="487" height="424" alt="image" src="https://github.com/user-attachments/assets/fbf993b2-6511-48d4-a036-f814b07eb63c" /><img width="1120" height="679" alt="image" src="https://github.com/user-attachments/assets/ac565588-833a-41ff-87d8-bde19d772c68" />


---

## Results
- The CNN achieved accurate classification of board squares, enabling reliable board state reconstruction.  
- The UR5e robot was able to execute both normal and special chess moves.  
- All games could be played fully from start to finish against Stockfish at adjustable difficulty.
  
Display of the system during early testing:
- The archives page: https://www.youtube.com/shorts/mkMYbiD1WSw
- Some gameplay: https://www.youtube.com/watch?v=vP3UgIXu7uM (The CNN misclassifies a bit at this stage because when changed to black backround as in the video, our cheap web camera distorted the board to blue. We couldn't fix it digitally, and the background was changed back to white at a later stage.)

---

## Notes
- This repo is a **showcase of core code** only.
- Full dataset, binaries, and GUI assets are not included due to size (>500MB).  
- Paths to Stockfish executable and datasets are hardcoded in places, as this is extracted from the original bachelor project, and not generalized due to time limitations.
- The code has not been edited since the thesis' deadline 05.22.2025, except for a merge of the stockfish classes for display purposes.
