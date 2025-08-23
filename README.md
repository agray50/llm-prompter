# llm-prompter

An interactive Jupyter Notebook UI for chatting with OpenAI models (GPT‑4o) using ipywidgets, with simple conversation history saved to disk.

## Features

- **Interactive chat UI** built with `ipywidgets` inside Jupyter
- **System instructions** box to steer the assistant
- **Conversation persistence**: messages saved/loaded as JSON under `./tmp/`
- **Multiple conversations**: create and switch between conversations via dropdown

## Project layout

- `interactive_chat.ipynb` — the main notebook (UI + logic)
- `requirements.txt` — Python dependencies
- `tmp/` — runtime directory for saved conversations (auto-created)

## Requirements

- Python 3.11+
- An OpenAI API key

## Setup

```bash
# (Optional) Create and activate a virtual environment
python3 -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Configure your API key
Create a `.env` file in the project root with:

```env
OPENAI_API_KEY=sk-...your-key-here...
```

The notebook calls `load_dotenv()` so environment variables from `.env` are available.

> Note: In `interactive_chat.ipynb` the client is initialized as `OpenAI(api_key='')`. You can:
> - Either paste your key into that argument, or
> - Replace that line with `client = OpenAI()` to automatically use `OPENAI_API_KEY` from your environment.

## Run

```bash
jupyter notebook
# Then open interactive_chat.ipynb and run all cells
```

## Using the UI

- **Instructions**: Write or adjust the system prompt (defaults to “You are a helpful assistant.”)
- **Prompt**: Enter your message
- **Send**: Submits your message, calls GPT‑4o, and appends the reply
- **New Conversation**: Starts a fresh `convo_N`
- **Load**: Switch between saved conversations
- **Chat Output**: Shows the running transcript

## Data & persistence

- Save directory: `./tmp/` (auto-created)
- Format: one JSON file per conversation (e.g., `convo_0.json`)
- Each entry: `{ "role": "user"|"assistant", "content": "..." }`

## Troubleshooting

- **Widgets don’t render**:
  - Ensure `ipywidgets>=8` is installed (provided via `requirements.txt`)
  - Use Jupyter Notebook 7+ (also included in requirements)
- **Authentication errors**:
  - Verify `OPENAI_API_KEY` is set (e.g., `import os; os.getenv('OPENAI_API_KEY')`)
  - Ensure `client = OpenAI(api_key='...')` or `client = OpenAI()` is configured properly
- **Permission errors writing to ./tmp**:
  - Ensure you have write access or adjust `SAVE_DIR` in the notebook

## Notes

- Conversation JSON files may contain sensitive data. Avoid committing `tmp/` to version control.
- The notebook currently targets `gpt-4o` via `client.responses.create`. Adjust the model if needed.