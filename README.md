# AI Voice-Controlled Personal Assistant

A GitHub-ready Python starter project for a voice/text-controlled desktop assistant. It interprets supported commands, searches files in a chosen workspace, organizes files into folders, and processes comment/task lists until each item is marked complete.

## What it does

- Accepts typed commands; voice input is an optional extension point.
- Searches for files by name inside a configured workspace.
- Lists files and folders.
- Creates folders and moves files with confirmation.
- Processes a CSV task/comment queue and marks entries complete.
- Uses a command allowlist instead of running arbitrary shell commands.
- Includes unit tests and GitHub Actions CI.

**Safety:** This project does not control every application or drive automatically. It only operates inside the workspace you configure. It does not delete files or execute arbitrary commands. Review actions before enabling write operations. Back up important data.

## Project layout

```text
ai-voice-controlled-assistant/
├── data/
│   └── tasks.csv
├── src/voice_assistant/
│   ├── __init__.py
│   ├── assistant.py
│   └── cli.py
├── tests/
│   └── test_assistant.py
├── .github/workflows/tests.yml
├── .gitignore
├── requirements.txt
└── README.md
```

## Requirements

- Python 3.10+
- No third-party packages required for the core text-command version
- Optional voice input can be added with a speech-recognition library and microphone support

## Run on Windows

Open Command Prompt in the extracted project directory:

```bat
py -m venv .venv
.venv\Scripts\activate
set PYTHONPATH=src
python -m voice_assistant.cli --workspace demo_workspace
```

The program creates the workspace folder if needed. Type `help` to see commands, and `exit` to quit.

## Supported commands

```text
help
list
find <filename fragment>
mkdir <folder name>
move <filename> <destination folder>
process-tasks <csv path>
exit
```

Examples:

```text
find report
mkdir Notes
move todo.txt Notes
process-tasks data/tasks.csv
```

`move` asks for confirmation before changing files. The destination must stay within the workspace. File operations are deliberately limited to the configured workspace.

## Task/comment processing

`data/tasks.csv` contains `id,comment,status` fields. The processor shows each pending item and asks whether it is complete. Answer `y` to mark it complete, `n` to leave it pending, or `q` to stop. It does not post comments to external websites or submit forms automatically.

## Run tests

```bat
set PYTHONPATH=src
python -m unittest discover -s tests -v
```

## GitHub upload

1. Extract the ZIP file.
2. Create a repository on GitHub, e.g. `ai-voice-controlled-assistant`.
3. In CMD, navigate to the extracted folder.
4. Run:

```bat
git init
git add .
git commit -m "Add AI voice-controlled assistant"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/ai-voice-controlled-assistant.git
git push -u origin main
```

Replace `YOUR-USERNAME` with your GitHub username. Install Git if CMD does not recognize `git`. Do not upload passwords, private files, tokens, or your `.venv` directory.

## Roadmap

- Add speech-to-text input using a local or opt-in speech engine.
- Add a GUI with an explicit workspace picker.
- Add undo/operation logs for file changes.
- Add an optional LLM intent parser, with strict validation and human confirmation.
- Add integrations only through official APIs and user-authorized accounts.

## Scope note

This is a safe starter assistant, not unrestricted remote control of the whole computer or every drive. It does not claim to understand every possible command. New actions should be added as explicit, tested handlers.
