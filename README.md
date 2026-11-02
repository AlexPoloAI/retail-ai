# AI Product Description Generator (Module 06 — AI in Retail)

A Flask web application that uses a **real large language model (LLM)** to write an English product description from a product name, user-provided features, a target audience, and a selected writing tone. It demonstrates personalized content generation for retail and e-commerce.

## Main features

- Product name and comma-separated product features input
- Personalized descriptions for a specified target audience
- **Stretch goal:** six writing tones (neutral, formal, casual, humorous, young adults, professionals)
- Choice of **Ollama** (default), **LM Studio**, or **OpenAI API** through `.env`
- Input validation and readable error messages when the LLM is unavailable
- Clear display of which model actually generated the result
- Basic word/phrase checks labeled as **heuristics, not factual verification**
- Three sample products for repeatable experiments

**Important:** This app does not generate a substitute template description when the LLM is unavailable. If the model/server is not running, a visible error will be shown instead. GitHub hosts the source code, not a live instance of this local Flask application.

## Project structure

```text
retail-ai/
├── app.py                # Flask app, prompt engineering, LLM API calls, validation
├── requirements.txt      # Python dependencies
├── .env.example          # Example provider/model configuration; no secrets
├── .gitignore            # Prevents committing .env and local files
├── README.md             # Documentation and short project report
├── screenshot.png        # Add a real screenshot after successfully running an LLM
├── templates/
│   └── index.html        # Input form and model-generated output
└── tests/
    └── test_app.py       # Automated tests with a mocked model
```

## Install and run (Windows PowerShell)

1. Install Python and [Ollama](https://ollama.com/download) if you plan to use the local LLM option.
2. In a terminal opened inside the project folder, run:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
Copy-Item .env.example .env
ollama pull gemma3:1b
python app.py
```

If PowerShell blocks environment activation, use `.\.venv\Scripts\python.exe -m pip install -r requirements.txt` and `.\.venv\Scripts\python.exe app.py` instead of activating it. Keep Ollama running. Open **http://127.0.0.1:5000/** and click **Generate Description**. An Ollama response should show a model label, for example `Ollama / gemma3:1b (real LLM)`.

To use **LM Studio**, load a model, start the local server in its Developer tab, and set `LLM_PROVIDER=lmstudio` and `LMSTUDIO_MODEL=` to the model ID shown in LM Studio. To use **OpenAI**, set `LLM_PROVIDER=openai` and add your own `OPENAI_API_KEY` in `.env`; this option may have API costs. Never commit `.env`.

## Run tests

```powershell
python -m pip install pytest
python -m pytest -q
```

The automated tests mock model responses. **Passing these tests confirms app logic, not that a real LLM connection works.** Run the app with an actual model and keep a screenshot of the output to demonstrate that requirement.

## Short project report (model choice, challenges, future improvements, ethics)

I chose **Option C, a local language model through Ollama (Gemma 3)**, as the main approach because it avoids the need to share an API key and supports experiments with different product descriptions on my computer. The application also supports LM Studio and the optional OpenAI API. One important design challenge was separating successful AI generation from a non-AI fallback: a fixed text template can create sentences, but it does not satisfy the assignment's LLM requirement. I therefore made generation fail visibly if the selected model is unavailable. I also used prompt instructions to discourage invented product claims and checked the form inputs before sending them to the model.

In a real retail store, the application could take product specifications from a catalog database, apply brand guidelines, and help staff draft versions for different customer segments. However, AI can invent claims, exaggerate benefits, or use unverified marketing language. For that reason, the current word and exact-phrase checks are only quick hints; they are **not** an automated factual accuracy rating. A human reviewer must compare each generated description with verified product data before publication. A useful next step would be integration with a trusted inventory system and a review/approval workflow.

> **Review before submitting:** This report describes the intended implementation. Confirm the model you actually used and revise any statements that do not reflect what you personally tested.

## Controlled experiments (fill in with your actual observations)

Test the same boots product with three tones so that other inputs stay constant. Then try a different product with another audience. Do not insert invented scores or claim that the LLM was tested until you actually run it.

| Experiment | Product | Audience | Tone | Observation to record after testing |
|---|---|---|---|---|
| A1 | TrailRunner Leather Boots | Hikers and outdoor enthusiasts | Casual | **Record actual output and one strength/weakness** |
| A2 | TrailRunner Leather Boots | Hikers and outdoor enthusiasts | Formal | **Compare wording against A1** |
| A3 | TrailRunner Leather Boots | Hikers and outdoor enthusiasts | Humorous | **Check whether humor stays grounded in product facts** |
| B1 | BaristaPro Espresso Grinder | Home baristas and coffee lovers | Professionals | **Check audience fit and any unsupported claims** |

**Suggested manual rubric:** score factual accuracy, audience fit, tone consistency, readability, keyword relevance, and usefulness from 1 to 5 **after reading each real output**. Explain one reason for each score. This manual rubric is separate from the on-screen keyword checks.

## Screenshot

After a successful run against Ollama, LM Studio, or OpenAI, take a screenshot showing the input, generated description, and model label. Save it as `screenshot.png` and upload it to the repository. Remove or replace any old screenshot showing template-only generation before submitting. The screenshot is evidence of your own run; it is not auto-generated by this repository.

## Submission

Required project files: `app.py`, `templates/index.html`, and the short written report in this README. The screenshot and experiments are additional supporting evidence. Submit the public GitHub repository URL through Canvas **Website URL** if that submission option is enabled.
