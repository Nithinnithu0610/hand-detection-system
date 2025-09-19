# Task 2 – Motion Detection with smolVLM

## 1. Overview

This project implements **real-time hand motion detection** using the SmolVLM model via webcam feed.  
It detects:

- Hand presence  
- Hand side (Left/Right)  
- Finger count (0–5)  
- Simple gestures (`Thumbs Up` / `Thumbs Down`)  

It can be **integrated with an attendance system** for gesture-based presence confirmation.

Implementation uses **Python or C++** for frame capture and SmolVLM for inference.

---

## 2. Core Components

| Component | Responsibility |
|-----------|----------------|
| `llama-server` | Hosts the SmolVLM model and performs inference on incoming frames. |
| SmolVLM Model | Performs visual-language understanding and gesture recognition. |
| Frame Capture Module (Python/C++) | Captures live webcam frames and sends them to the server. |
| Hand Detection Module | Detects hand bounding boxes and landmarks. |
| Hand Side Identification | Determines Left/Right hand from landmarks. |
| Finger Counting | Counts the number of extended fingers. |
| Gesture Recognition | Recognizes predefined gestures such as Thumbs Up / Thumbs Down. |
| Logger | Records detection events and system performance metrics. |

**Design Pattern:** Client-server architecture with modular hand detection and inference components.

---

## 3. Motion Detection Workflow

### 3.1 Hand Detection
- Capture frames from the webcam.
- Detect hand bounding boxes and landmarks using OpenCV / MediaPipe.
- Output: `"Hand Detected"` + coordinates.

### 3.2 Hand Side Identification
- Analyze landmarks to classify as Left or Right hand.

### 3.3 Finger Counting
- Determine which fingers are extended.
- Output: `"Finger Count: N"`

### 3.4 Gesture Recognition
- Thumbs Up: Thumb extended, others folded.
- Thumbs Down: Thumb down, others folded.
- Output: `"Gesture: Thumbs Up / Thumbs Down / Other"`

### 3.5 Logging
- Record gesture events and system metrics (CPU, memory).

---

## 4. Component Interaction Diagram

```mermaid
graph TD
    A[Webcam Frame] --> B[Frame Capture Module (Python/C++)]
    B --> C[llama-server: SmolVLM]
    C --> D[Hand Detection Module]
    D --> E[Hand Side Identification]
    D --> F[Finger Counting]
    E --> F
    F --> G[Gesture Recognition]
    G --> H[Logger / Storage]
```

---

## 5. Deployment Architecture

- **Dependencies:** Python 3.x / C++ compiler, OpenCV, MediaPipe, llama.cpp, SmolVLM 500M model.
- **Build Steps:**
  1. Clone `llama.cpp` repository and build.
  2. Start `llama-server` with SmolVLM model.
- **Server Command Example:**
```bash
llama-server -hf ggml-org/SmolVLM-500M-Instruct-GGUF -ngl 99
```
- **Frontend / Client:** Python or C++ module captures frames and sends to the server.
- **Environment:** Supports dev, staging, prod. Can be run on GPU for real-time performance.

---

## 6. Runtime Behavior

- Application initializes `llama-server` and loads the SmolVLM model.
- Python/C++ client streams frames continuously.
- Server performs inference: hand detection, side identification, finger counting, gesture recognition.
- Detection results are returned to the client and optionally logged.
- Errors (missing frames, server errors) are handled gracefully. Performance monitoring runs in background.

---

## 7. Integration with Attendance Repo

- **Integration Point:** Replace or complement face recognition frames with hand gesture frames.
- **Use Cases:**
  - Hand gestures as attendance confirmation.
  - Combine face + gesture for dual-factor presence validation.
- **Implementation:**
  - Add Python/C++ module in attendance repo to query SmolVLM server.
  - Log gesture events alongside face recognition.
  - Use streaming or batch inference as needed.

---

## 8. Inputs & Outputs

| Input | Output |
|-------|--------|
| Live webcam frames | Hand Detected / No Hand |
| Hand landmarks | Left Hand / Right Hand |
| Finger positions | Finger Count (0–5) |
| Gesture detection | Thumbs Up / Thumbs Down / Other |

---

*End of Task 2 Documentation*

