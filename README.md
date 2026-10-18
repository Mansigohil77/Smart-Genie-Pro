# Smart-Genie-Pro
A local-first AI workspace powered by Ollama, Node.js, and Express.js, featuring AI chat, image intelligence with OCR, Python and JavaScript code execution, conversation history, and local file management.
# 🤖 Smart Genie Pro

### A Local-First AI Workspace Built with Node.js, Express & Ollama

**Smart Genie Pro is a locally hosted AI workspace that brings conversational assistance, persistent chat history, OCR-based image intelligence, Python and JavaScript execution, and local file management into one application.**

The project explores how multiple AI-assisted workflows can be integrated into a single application using a locally running language model.

Instead of depending on a cloud AI API for its core language-model inference, Smart Genie Pro connects to [Ollama](https://ollama.com/) and communicates with the model through its local HTTP API.

---

## ✨ Features

### 💬 1. AI Assistant

* Ask questions and receive AI-generated responses.
* Connect to a locally hosted Ollama model.
* Support regular and streaming response endpoints.
* Handle model connectivity issues and request timeouts.
* Preserve conversations in local JSON storage.
* Revisit and delete saved conversations.

### 🧠 2. Persistent Conversation History

* Store conversation sessions locally.
* Generate conversation identifiers.
* Maintain conversation titles and timestamps.
* Retrieve previous conversations through API endpoints.
* Limit retained sessions and messages to help control storage growth.

### 🖼️ 3. Image Intelligence & OCR

* Upload supported images for text extraction.
* Extract text using optional Tesseract.js integration.
* Optionally preprocess images using Sharp.
* Normalize extracted text.
* Evaluate OCR confidence and identify potentially unreliable results.
* Ask questions about extracted text using the configured local AI model.
* Return warnings when OCR quality may affect answer reliability.

**Important:** This implementation primarily analyzes extracted image text. It is not a general-purpose visual recognition system for understanding every object, scene, or visual relationship in an image.

### 💻 4. Python & JavaScript Execution

* Submit Python or JavaScript code for execution.
* Execute code through child processes.
* Return standard output and standard error.
* Report exit codes and execution duration.
* Apply a configurable execution timeout.
* Limit the size of submitted code.

Code execution is intended for trusted local use. The current implementation does not provide a complete security sandbox for arbitrary untrusted code.

### 📁 5. Local File Management

* Inspect files and folders within the application's data directory.
* Create folders.
* Rename files and folders.
* Delete supported files and folders.
* Open the application's data directory.

Path validation and protected-folder checks are included, but further security testing is necessary before treating the application as safe for hostile or multi-user environments.

### 🔌 6. Ollama Integration

* Configure the Ollama endpoint.
* Select the configured model through environment variables.
* Check whether Ollama is responding.
* Inspect installed model information.
* Handle unavailable models and failed requests.

The default configuration uses `phi3:latest`.

---

## 🛠️ Technology Stack

| Technology      | Purpose                          |
| --------------- | -------------------------------- |
| Node.js         | Backend runtime                  |
| Express.js      | HTTP server and API routing      |
| Ollama          | Local language-model integration |
| JavaScript      | Application logic                |
| Multer          | Image upload handling            |
| JSON            | Local conversation persistence   |
| Tesseract.js    | Optional OCR                     |
| Sharp           | Optional image preprocessing     |
| Child processes | Python and JavaScript execution  |

---

## 🏗️ Architecture

The application follows a straightforward local-first architecture:

1. **User interface:** Sends questions, images, code, and file-management requests to the backend.
2. **Express API:** Validates incoming requests and routes them to the appropriate handler.
3. **Local AI integration:** Sends prompts to the configured Ollama model.
4. **Conversation storage:** Persists conversation sessions in a local JSON file.
5. **Image processing:** Optionally preprocesses uploaded images before OCR.
6. **Execution engine:** Starts Python or Node.js child processes and captures their results.
7. **File-management layer:** Performs supported operations inside designated application directories.

### Request flow

`User → Frontend → Express API → Local Processing / Ollama → Response`

Core language-model inference is designed to run locally. Optional dependencies must be installed for their associated features to work.

---

## 🚀 Getting Started

### Prerequisites

Install:

* Node.js
* npm
* Ollama

Download Node.js from https://nodejs.org/ and Ollama from https://ollama.com/.

### 1. Clone the repository

```bash
git clone YOUR_GITHUB_REPOSITORY_URL
cd YOUR_REPOSITORY_FOLDER
```

Replace the placeholders with your actual repository URL and folder name.

### 2. Install dependencies

```bash
npm install express multer
```

Optional image-processing dependencies:

```bash
npm install tesseract.js sharp
```

### 3. Download the local model

```bash
ollama pull phi3:latest
```

Ensure Ollama is running before starting the application.

### 4. Start the server

```bash
node server.js
```

The default local application address is:

http://127.0.0.1:5000

The frontend is served from the `public` directory. Ensure that `public/index.html` exists.

### 5. Configure the environment

The application supports the following environment variables:

| Variable       | Default                  | Description         |
| -------------- | ------------------------ | ------------------- |
| `PORT`         | `5000`                   | Server port         |
| `HOST`         | `127.0.0.1`              | Server bind address |
| `OLLAMA_URL`   | `http://127.0.0.1:11434` | Ollama API endpoint |
| `OLLAMA_MODEL` | `phi3:latest`            | Configured model    |

For example, in PowerShell:

```powershell
$env:PORT = "5000"
$env:HOST = "127.0.0.1"
$env:OLLAMA_MODEL = "phi3:latest"
node server.js
```

Keep the server bound to localhost unless you have deliberately configured and tested the additional security controls required for remote access.

---

## 🔍 API Overview

| Endpoint              | Method | Purpose                                     |
| --------------------- | ------ | ------------------------------------------- |
| `/health`             | GET    | Application and Ollama status               |
| `/api/models`         | GET    | Configured and installed model information  |
| `/history`            | GET    | Retrieve conversation history               |
| `/history/new`        | POST   | Create a conversation                       |
| `/history/:sessionId` | GET    | Retrieve a conversation                     |
| `/history/:sessionId` | DELETE | Delete a conversation                       |
| `/ask`                | POST   | Generate an AI response                     |
| `/ask-stream`         | POST   | Stream an AI response                       |
| `/ocr`                | POST   | Extract text from an uploaded image         |
| `/ask-image`          | POST   | Answer questions about extracted OCR text   |
| `/run-code`           | POST   | Execute supported Python or JavaScript code |
| `/organize-files`     | GET    | Inspect application data files              |
| `/os-automation`      | POST   | Perform supported local file operations     |

---

## 🔐 Privacy & Security

Smart Genie Pro is designed for local use.

* Core language-model requests are sent to the configured Ollama endpoint.
* Conversation history is stored in local JSON storage.
* Uploaded images are processed by the application and removed after the OCR request completes.
* Code execution occurs through local child processes.

These design choices do not guarantee complete privacy or security. The application has not been established as a hardened security environment.

**Before using it with sensitive information or exposing it beyond localhost:**

* Add authentication and authorization where needed.
* Implement strict execution isolation for untrusted code.
* Review filesystem permissions and deletion behavior.
* Validate uploaded file types and content.
* Add request-rate limits and appropriate resource limits.
* Review data retention and backup practices.
* Test error handling and concurrent requests.
* Never expose the code-execution endpoint to an untrusted network without robust isolation.

---

## 🧪 Testing

Test the application feature by feature:

* Confirm Ollama is reachable and the selected model is installed.
* Submit an AI question and verify the response.
* Create a new conversation and reload previous history.
* Upload a clear image and check the extracted text.
* Ask a question whose answer is explicitly present in the extracted text.
* Run a simple Python or JavaScript program.
* Verify that syntax errors and timeouts are reported correctly.
* Test file operations within the designated data directory.

Successful startup alone does not establish that every feature works correctly. Validate each workflow in your own environment.

---

## 🎯 Project Goals

Smart Genie Pro explores the practical engineering challenges involved in building an AI workspace that integrates:

* Local language models
* Persistent conversational workflows
* OCR-based document and image-text analysis
* Code execution
* Local file operations
* API-driven architecture

The broader goal is to develop a reliable, useful, and maintainable local AI application through iterative testing and improvement.

---

## 👩‍💻 Author

**Mansi Gohil**

GitHub: https://github.com/Mansigohil77

LinkedIn: https://www.linkedin.com/in/mansigohildm/

---

## 📄 License

Add a `LICENSE` file before declaring a license for this project. If you intend to publish it as open source, choose a license that fits your distribution and contribution goals.

---

**Smart Genie Pro — Explore AI locally. Build workflows that do more.**
