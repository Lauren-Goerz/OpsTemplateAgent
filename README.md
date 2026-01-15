# Rasa Helpdesk Agent

A Rasa Pro 3.15 conversational agent for the Rasa Helpdesk, built to help users with company information and support.

## Installation

This project uses **Rasa Pro 3.15** (Developer Edition). Follow these steps to set up your environment:

### Prerequisites

- Python 3.10, 3.11, 3.12, or 3.13 (Python 3.11 recommended)
- A Rasa Developer Edition License (obtain from [Rasa License Request Page](https://rasa.com/docs/pro/installation/licensing))

### Set Up Your Python Environment

1. **Check your Python version:**
   ```bash
   python --version
   pip --version
   ```

2. **Install the `uv` package manager** (recommended for faster installation):
   
   **macOS and Linux:**
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```
   
   **Windows (PowerShell):**
   ```powershell
   powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
   ```

### Create Virtual Environment

1. **Create and activate a virtual environment:**
   ```bash
   # Create virtual environment with Python 3.11
   uv venv --python 3.11
   
   # Activate the virtual environment
   # macOS and Linux:
   source .venv/bin/activate
   
   # Windows:
   # .venv\Scripts\activate
   ```

   > **Note:** Virtual environments only need to be created once per project, but you must activate them every time you open a new terminal session.

### Install Rasa Pro

1. **Install Rasa Pro:**
   ```bash
   uv pip install rasa-pro
   ```
   
   Alternatively, you can use `pip` (though it will take longer):
   ```bash
   pip install rasa-pro
   ```

2. **Set your Rasa license:**
   
   **macOS and Linux:**
   ```bash
   export RASA_LICENSE=YOUR_LICENSE_KEY
   ```
   
   **Windows:**
   ```cmd
   set RASA_LICENSE=YOUR_LICENSE_KEY
   ```

3. **Verify installation:**
   ```bash
   rasa --version
   ```

### Install Project Dependencies

Install the required language data package:
```bash
pip install "langcodes[data]"
```

### Train the Model

After making any changes to the domain, flows, or patterns, train the model:
```bash
rasa train
```

### Run the Agent

Start the Rasa development inspector for the full testing experience:
```bash
rasa inspect
```

This will start the Rasa server with the development inspector, providing a comprehensive testing interface. Visit `http://localhost:5005/webhooks/socketio/inspect.html` in your browser to interact with the agent.

Alternatively, you can use the simple shell interface:
```bash
rasa shell
```

## Project Structure

```
OpsTemplateAgent/
├── actions/                 # Custom action server code
│   ├── __init__.py
│   └── actions.py          # Custom action implementations
├── data/                    # Training data and flows
│   ├── flows.yml           # User-defined flows
│   └── patterns.yml        # Pattern flows (session_start, chitchat, etc.)
├── models/                  # Trained model files (gitignored)
│   └── *.tar.gz            # Trained Rasa models
├── config.yml               # Rasa configuration (pipeline, policies)
├── credentials.yml          # Channel credentials
├── domain.yml              # Domain definition (responses, slots, intents)
├── endpoints.yml           # Action server and tracker store endpoints
├── .gitignore              # Git ignore rules
└── README.md               # This file
```

### Key Files Explained

- **`domain.yml`**: Defines the agent's responses, slots, and domain configuration
- **`data/flows.yml`**: Contains user-defined conversation flows
- **`data/patterns.yml`**: Contains pattern flows (system flows like session_start, chitchat)
- **`config.yml`**: Configures the NLU pipeline and dialogue policies
- **`endpoints.yml`**: Defines endpoints for action servers and external services

## Adding a New Flow

This guide shows you how to add a new flow to answer questions about the Rasa Vacation Policy.

### Step 1: Add the Response to Domain

Edit `domain.yml` and add a new response in the `responses:` section:

```yaml
  utter_vacation_policy:
    - text: "Rasa employees receive 25 days of paid vacation per year, plus public holidays. Vacation requests should be submitted through our HR portal at least 2 weeks in advance for approval."
```

### Step 2: Create the Flow

Edit `data/flows.yml` and add a new flow:

```yaml
flows:
  vacation_policy:
    description: Provide information about the Rasa vacation policy
    steps:
      - action: utter_vacation_policy
```

### Step 3: Train the Model

After adding the flow, train the model:

```bash
rasa train
```

### Step 4: Test the Flow

Start the Rasa development UI to test your new flow with the full testing experience:

```bash
rasa inspect
```

Visit `http://localhost:5005/webhooks/socketio/inspect.html` in your browser, then try asking questions like:
- "What is the vacation policy?"
- "How many vacation days do we get?"
- "Tell me about vacation at Rasa"

The `CompactLLMCommandGenerator` in Rasa Pro 3.15 will automatically route these questions to your `vacation_policy` flow.

## Current Flows

- **`pattern_session_start`**: Greets users when they start a conversation
- **`company_location`**: Provides the company address (Schönhauser Allee 175, 10119 Berlin)
- **`pattern_chitchat`**: Handles off-topic conversations
- **`pattern_search`**: Handles knowledge-based questions

## Resources

- [Rasa Pro Documentation](https://rasa.com/docs/pro/)
- [Rasa Pro Installation Guide](https://rasa.com/docs/pro/installation/python)
- [Rasa License Request](https://rasa.com/docs/pro/installation/licensing)
- [Rasa Community Forum](https://forum.rasa.com/)

## License

See [LICENSE](LICENSE) file for details.
