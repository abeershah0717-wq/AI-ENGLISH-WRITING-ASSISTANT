# AI-ENGLISH-WRITING-ASSISTANT# ✍️ AI English Writing Assistant

A simple, beginner-friendly web app that helps you write better English. Paste your text, choose a task, and the app (powered by Google's Gemini Flash model) will:

- correct grammar, spelling and punctuation mistakes
- explain every mistake in **simple English**
- suggest better vocabulary and sentence structures
- show a **before-and-after** comparison with highlighted changes
- let you **download** the improved text as a `.txt` file

Built with **Python**, **Streamlit** and the official **`google-genai`** SDK. No database and no login.

## Features

| Feature | Where you see it |
| --- | --- |
| Text box for your writing (up to 5,000 characters) | Top left |
| Task selector: Grammar Correction, Improve Writing, Make Text More Professional, Simplify English, Vocabulary Suggestions | Top right |
| "Analyze and improve" button and "Try an example text" button | Top right |
| Improved text + download button | Results section |
| Before & After comparison (red = removed, green = new) | Results tab 1 |
| Mistakes with simple explanations | Results tab 2 |
| Vocabulary suggestions | Results tab 3 |
| Sentence structure tips | Results tab 4 |
| Friendly messages for empty input, missing key, quota limits, network errors and unreadable AI answers | Anywhere |

## Project structure

```
your-project/
├── app.py                  # the whole application
├── requirements.txt        # Python packages
├── README.md               # this file
├── .gitignore              # keeps your secret key out of GitHub (you create this)
└── .streamlit/
    └── secrets.toml        # your API key, local only (you create this, NEVER upload it)
```

## Run it on your computer (Windows)

1. **Install Python 3.10 or newer** from <https://www.python.org/downloads/>. During setup, tick **"Add python.exe to PATH"**.
2. **Open PowerShell** in your project folder (open the folder in File Explorer, click the address bar, type `powershell`, press Enter).
3. **Create a virtual environment** and activate it:
   ```powershell
   python -m venv venv
   venv\Scripts\Activate.ps1
   ```
   If PowerShell says scripts are disabled, run `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`, answer `Y`, and try again. (Or use Command Prompt and run `venv\Scripts\activate.bat`.)
4. **Install the packages:**
   ```powershell
   pip install -r requirements.txt
   ```
5. **Add your API key.** Create a folder named `.streamlit` and, inside it, a file named `secrets.toml` with exactly this content (use your own key):
   ```toml
   GEMINI_API_KEY = "paste-your-key-here"
   ```
6. **Start the app:**
   ```powershell
   streamlit run app.py
   ```
   Your browser opens at <http://localhost:8501>.

### Optional setting: choose a different model

The app uses `gemini-3.5-flash` by default. Google retires old models from time to time. If you see a "model not found" message, add one more line to your secrets (no code change needed):

```toml
GEMINI_MODEL = "name-of-another-gemini-flash-model"
```

You can find current model names in the [Gemini API model list](https://ai.google.dev/gemini-api/docs/models).

## Keep your API key safe

- The key is read in `app.py` with `st.secrets["GEMINI_API_KEY"]`. It is **never** written in the code.
- Create a file named `.gitignore` in the project folder with this content so Git ignores your secrets:
  ```
  .streamlit/secrets.toml
  venv/
  __pycache__/
  ```
- If you ever upload a key by accident, delete it in Google AI Studio and create a new one.

## Deploy on Streamlit Community Cloud (free)

1. Push the project to a **GitHub repository** (see the steps in the guide below). Make sure `secrets.toml` is **not** uploaded.
2. Go to <https://share.streamlit.io> and sign in with GitHub.
3. Click **Create app** (or **New app**) and choose your repository, the branch (usually `main`) and the main file path `app.py`.
4. Open **Advanced settings**, choose Python 3.11 or 3.12, and paste your secret into the **Secrets** box:
   ```toml
   GEMINI_API_KEY = "paste-your-key-here"
   ```
5. Click **Deploy**. After a minute or two you get a public link like `https://your-app-name.streamlit.app`.
6. To change the key later: open your app, click **Manage app** → **Settings** → **Secrets**, edit it and save.

## Optional: change the colors

Create `.streamlit/config.toml` (this file is safe to upload) to change the theme:

```toml
[theme]
primaryColor = "#0F766E"
```

## Troubleshooting

| Message | What to do |
| --- | --- |
| "API key not found" | Check that `.streamlit/secrets.toml` exists and contains `GEMINI_API_KEY = "..."`. On Cloud, check the app Secrets. |
| "Your API key was not accepted" | Copy the key again from Google AI Studio. No spaces, and keep the quotation marks. |
| "The AI model was not found" | Add `GEMINI_MODEL` to your secrets (see above). |
| "Too many requests" | You hit the free usage limit. Wait a minute (or until tomorrow) and try again. |
| App works locally but not on Cloud | Make sure `requirements.txt` is in the repository root and the Secrets box is filled in. |

## How the code is organized (`app.py`)

1. **Settings**: model name, limits, the list of writing tasks.
2. **Helper functions**: read secrets, make text safe for display, find differences between two texts, friendly error messages.
3. **Talking to Gemini**: build the request, ask for a JSON answer, check the answer, retry once if it is unreadable.
4. **Screen helpers**: functions that draw each part of the results.
5. **`main()`**: the page itself, from input box to results.

## Notes

- Your text is sent to Google's Gemini API to get the answer. Do not paste private or sensitive information.
- AI can make mistakes. Always read the result before you use it.
- Free-tier limits and model names change over time. Check the [Gemini API documentation](https://ai.google.dev/gemini-api/docs) for the latest details.
