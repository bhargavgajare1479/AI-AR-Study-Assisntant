# AI-Powered AR Study Assistant

A hands-on workshop project demonstrating how **Augmented Reality (AR)** and **Multimodal AI** can be combined to create an interactive learning experience.

The application allows a student to place or draw an educational diagram on a physical surface, capture it using the AR camera, send the image to Google's Gemini multimodal API, and display the AI-generated interpretation as a spatially anchored AR information card.

> **Workshop project:** This repository is designed for educational prototyping, not production deployment.

---

## Project Overview

The project demonstrates a simple but complete pipeline:

```text
Physical World
      │
      ▼
AR Plane Detection
      │
      ▼
Student Places / Draws Diagram
      │
      ▼
AR Camera Capture
      │
      ▼
Gemini Multimodal API
      │
      ▼
Structured AI Response
      │
      ▼
Unity JSON Parsing
      │
      ▼
Spatially Anchored AR UI
```

The central idea is:

> **AR tells the application where something is. AI tells the application what it means.**

Unity acts as the application layer connecting the physical environment, AR interaction, AI service, and user interface.

---

## What Does the Application Do?

The student can:

1. Open the application.
2. Allow AR plane detection to identify a physical surface.
3. Select a suitable study area.
4. Place or draw an educational diagram on that surface.
5. Capture the diagram using the AR camera.
6. Send the captured image to Gemini.
7. Receive an AI-generated interpretation.
8. Display the interpretation in an AR information card.

The diagram does **not** need to exist before plane detection.

Plane detection is used only to establish the application's understanding of the physical environment and provide a location for the AR experience.

After the study area has been established, the student can place or draw the diagram.

---

## Example

A student draws a rough neural-network diagram on paper:

```text
Input Layer
    │
    ▼
Hidden Layer
    │
    ▼
Output Layer
```

The application captures the diagram and sends it to Gemini.

Gemini may identify it as a feed-forward neural network and return information such as:

```json
{
  "diagram_type": "neural_network",
  "title": "Feed-Forward Neural Network",
  "explanation": "The diagram represents a neural network in which information moves from the input layer through one or more hidden layers to the output layer.",
  "potential_issues": []
}
```

Unity parses this response and displays the result as an AR information card beside the physical diagram.

---

# Features

### AR / Spatial Computing

- AR plane detection
- Surface raycasting
- Physical surface selection
- Spatial positioning
- AR-anchored UI
- Camera-based image capture

### Multimodal AI

- Image-based Gemini input
- Custom prompts
- Educational diagram interpretation
- Structured AI responses
- Identification of obvious conceptual issues

### Application Engineering

- REST API communication
- JSON request/response handling
- C# integration
- Dynamic Unity UI
- Separation between AR, AI, and UI components

---

# Technology Stack

| Technology | Purpose |
|---|---|
| Unity 2022.3.62f3 | Application development |
| C# | Application logic |
| AR Foundation | Cross-platform AR functionality |
| ARCore | Android AR support |
| ARKit | iOS AR support |
| Gemini API | Multimodal AI interpretation |
| REST API | Communication with Gemini |
| JSON | Structured AI response |
| Git / GitHub | Version control |

---

# Requirements

## Software

Install the following before opening the project:

- Unity Hub
- Unity **2022.3.62f3**
- Android Build Support and/or iOS Build Support
- Git

The required Unity packages are included/configured in the project.

## Hardware

For the complete experience, use a physical mobile device with AR support.

Recommended:

- Android device supporting ARCore
- iPhone/iPad supporting ARKit

A desktop computer can be used for development, but the AR experience should ultimately be tested on a physical device.

---

# Getting Started

## 1. Clone the Repository

```bash
git clone <REPOSITORY_URL>
```

Then open the project using:

```text
Unity Hub → Add → Select project folder
```

Use:

```text
Unity 2022.3.62f3
```

---

## 2. Open the Project

After opening the project, allow Unity to import and compile the project.

The first import may take some time.

Do not modify or delete the `Library` folder while Unity is importing the project.

---

# Gemini API Setup

This project uses the Gemini API to interpret captured diagrams.

Each student should use their **own Gemini API key**.

## 1. Create an API Key

Create a Gemini API key through Google's AI development platform.

Do not commit the API key to GitHub.

## 2. Add the API Key

Enter the key in the API configuration location provided by the workshop project.

For example:

```text
GeminiConfig
    └── API Key
```

The exact location may depend on the implementation provided in the workshop.

---

## Security Warning

This workshop uses direct API access from the Unity application for simplicity.

That is intentional.

This is **not a production security architecture**.

Never:

- Commit your API key to Git.
- Push your API key to GitHub.
- Share your API key with other students.
- Put your API key in screenshots.
- Upload your API key to public repositories.

For production applications, API requests should generally be routed through a secure backend rather than exposing credentials inside the client application.

---

# Application Flow

The application follows this sequence:

### Step 1 — Start AR

The application starts an AR session.

```text
AR Session
    ↓
Camera
    ↓
Plane Detection
```

AR Foundation searches for suitable physical surfaces.

---

### Step 2 — Select Study Area

A reticle indicates where an AR object can be placed.

The student taps a detected surface.

This establishes the location of the study area in physical space.

---

### Step 3 — Prepare the Diagram

The student places or draws an educational diagram on the selected physical surface.

Examples:

- Neural networks
- Flowcharts
- Database schemas
- CPU architecture
- ML pipelines
- Data-processing diagrams
- Basic system architectures
- Educational block diagrams

The diagram can be **hand-drawn or printed**.

---

### Step 4 — Capture

The student selects:

```text
Analyze Diagram
```

The application captures the relevant camera image.

---

### Step 5 — Send to Gemini

Unity creates an HTTP request containing:

```text
Image
+
Prompt
```

The request is sent to the Gemini API.

---

### Step 6 — AI Interpretation

Gemini analyzes the visual input.

The prompt asks the model to determine:

- What the diagram represents
- The likely diagram type
- Important labels/components
- Relationships or flow
- A concise explanation
- Obvious conceptual issues

The response is requested in a structured format.

---

### Step 7 — Parse the Response

Unity receives the JSON response.

The application converts the response into C# data structures.

Conceptually:

```text
Gemini Response
      ↓
JSON
      ↓
C# Object
      ↓
UI Data
```

---

### Step 8 — Display in AR

The resulting information is displayed in an AR UI card.

Example:

```text
┌──────────────────────────────┐
│        AI ANALYSIS           │
│                              │
│ Feed-Forward Neural Network  │
│                              │
│ This diagram represents...   │
│                              │
│ Potential Issues             │
│ None identified              │
└──────────────────────────────┘
```

The card is positioned in physical space rather than being a conventional screen-only interface.

---

# Architecture

```text
                 ┌─────────────────────┐
                 │   Physical World    │
                 │                     │
                 │  Diagram / Surface  │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │    AR Foundation    │
                 │                     │
                 │ Plane Detection     │
                 │ Raycasting          │
                 │ Spatial Positioning │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │    Camera Capture   │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │    Unity C# Layer   │
                 │                     │
                 │ Prompt + Image      │
                 │ HTTP Request        │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │    Gemini API       │
                 │                     │
                 │ Multimodal Analysis │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │    JSON Response    │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │   Unity JSON Layer  │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │     AR UI Card      │
                 │                     │
                 │ AI Explanation      │
                 │ Issues / Feedback   │
                 └─────────────────────┘
```

---

# Project Structure

The exact folder structure may evolve during development, but the project is organized around the major responsibilities of the application.

```text
Assets/
│
├── Scenes/
│   └── MainScene
│
├── Scripts/
│   ├── AR/
│   │   ├── PlaneDetection
│   │   ├── Raycasting
│   │   └── Placement
│   │
│   ├── AI/
│   │   ├── GeminiAPI
│   │   └── GeminiConfig
│   │
│   ├── Data/
│   │   └── DiagramResponse
│   │
│   └── UI/
│       ├── AnalysisCard
│       └── UIManager
│
├── Prefabs/
│
├── Materials/
│
├── UI/
│
└── Resources/
```

The provided boilerplate may contain additional folders or scripts required by the workshop.

---

# Gemini Response Format

The AI response is designed to be consumed by software rather than simply displayed as unstructured text.

A simplified response can look like:

```json
{
  "diagram_type": "string",
  "title": "string",
  "explanation": "string",
  "potential_issues": [
    "string"
  ]
}
```

For example:

```json
{
  "diagram_type": "database_schema",
  "title": "Student Database Schema",
  "explanation": "The diagram shows a relational database containing student-related entities and relationships between them.",
  "potential_issues": [
    "The relationship between Course and Enrollment is not clearly labelled."
  ]
}
```

This allows Unity to use AI output as application data.

---

# Prompt Design

The Gemini prompt should account for the fact that the image may be:

- Hand-drawn
- Rough
- Incomplete
- Photographed at an angle
- Poorly aligned
- Contain handwritten labels
- Contain arrows and symbols
- Have imperfect handwriting

The model should focus on understanding the intended educational structure rather than judging the visual quality.

A suitable prompt should request:

```text
1. Identify what the diagram represents.
2. Determine the likely diagram type.
3. Identify important components and relationships.
4. Provide a concise undergraduate-level explanation.
5. Identify obvious conceptual issues if present.
6. Return the result in the required JSON structure.
```

---

# Workshop Learning Objectives

After completing the project, students should understand how to:

- Build a basic AR application using Unity.
- Detect physical surfaces using AR Foundation.
- Convert physical interaction into digital coordinates.
- Place content in physical space.
- Capture visual information from an AR application.
- Communicate with an external AI service using REST.
- Send images to a multimodal AI model.
- Design prompts for visual reasoning.
- Consume structured JSON from an AI API.
- Connect AI output to a Unity application.
- Create spatially anchored user interfaces.

More importantly, students should understand that AR and AI can be combined into a single application architecture.

---

# What This Project Is Not

This is a **prototype for learning and demonstration**.

It is not intended to be:

- A production educational platform
- A medical or scientific diagnostic system
- A guaranteed-correct diagram checker
- A replacement for teachers
- A production-ready AI architecture
- A secure API deployment
- A custom computer-vision model
- A fully autonomous diagram-understanding system

Gemini's interpretation can be incorrect, particularly when diagrams are ambiguous, poorly drawn, poorly lit, or technically complex.

---

# Known Limitations

The AI may struggle with:

- Very small text
- Extremely messy handwriting
- Poor lighting
- Motion blur
- Heavy perspective distortion
- Overlapping labels
- Ambiguous arrows
- Highly specialized technical diagrams
- Incomplete diagrams
- Diagrams with unclear relationships

The application should therefore treat AI output as **assistance**, not guaranteed ground truth.

---

# Scope

The workshop intentionally keeps the application small.

### Included

- AR plane detection
- Surface selection
- Diagram placement/drawing
- Image capture
- Gemini multimodal analysis
- Structured response
- AR information card

### Not included

- Quiz functionality
- Interactive component exploration
- Voice commands
- Hand tracking
- Gesture recognition
- Persistent world mapping
- Multiplayer
- Complex 3D reconstruction
- Custom computer-vision model training
- Production backend infrastructure

These can be explored as future projects.

---

# Future Extensions

Once the basic prototype works, the same architecture could be extended toward:

- Interactive AR textbooks
- AR laboratory assistants
- AI-powered technical documentation
- AR museum guides
- Industrial inspection
- AI-assisted engineering diagrams
- Computer vision applications
- VR learning environments
- HCI research
- Spatial AI applications

The purpose of the workshop is to demonstrate the foundation from which these systems can be developed.

---

# Development Philosophy

This project deliberately follows a **prototype-first approach**.

The objective is not to build every layer from scratch.

Instead:

```text
Pre-built Infrastructure
          +
Hands-on AR Development
          +
Live Gemini Integration
          +
AI-powered Spatial UI
          =
Complete Working Prototype
```

The boilerplate removes repetitive setup and infrastructure work so that workshop time can be spent understanding the important concepts.

---

# Contributing

This repository is primarily intended for workshop participants.

Students are encouraged to experiment with:

- Different diagram types
- Different prompts
- UI layouts
- AR positioning
- AI response formats
- Additional educational use cases

Keep experimental changes isolated from the core workshop implementation where possible.

---

## Workshop Project

**AI-Powered AR Study Assistant**

**Built with Unity + AR Foundation + Gemini Multimodal AI**

```text
Physical World
      ↓
      AR
      ↓
 Visual Input
      ↓
      AI
      ↓
Structured Data
      ↓
   Unity UI
      ↓
Spatial Experience
```