# Chapito (chat-to-API)

Free API access using your web-based chatbots ![logo](https://github.com/user-attachments/assets/c21a077e-860b-4c84-ab7e-7b65b3e68379)

## Current version 0.1.4 (2025-03-22)

- [NEW] Add DeepSeek.

## Presentation

The primary goal of *Chapito* is to enable developers to use web-based chatbots —such as Grok, Mistral, ...— with tools that rely on language model (LLM) APIs —like [AIder](https://aider.chat/), [Cline](https://github.com/cline/cline), or [Roo-Code](https://github.com/RooVetGit/Roo-Code)— without needing to pay for additional API subscriptions.  
It achieves this by bridging the gap between premium chatbot accounts and applications that require API access, even for chatbots like Grok that do not offer a native API.  
  
**Chapito** addresses two key challenges:  
  
🔌 **Access to chatbots without APIs:** Many web-based chatbots, such as Grok, lack a direct API, making it difficult for developers to integrate them into their workflows. *Chapito* provides a workaround by simulating API access to these chatbots.  
💸 **Cost efficiency:** Developers with premium chatbot accounts (e.g., Grok, Mistral, OpenAI) can leverage their existing subscriptions instead of purchasing separate API access, saving money while utilizing the full capabilities of their accounts.  

### Key functionalities

*Chapito* acts as a local proxy server that connects web-based chatbots to API-dependent applications. Its core features include:  

- 💻 **Local server operation:** runs on your machine, intercepting API requests from tools and relaying them to a web-based chatbot.
- 🌐 **Browser integration:** uses a web browser to load the chatbot's webpage, automating the process of sending queries and retrieving responses.
- 🧩 **OpenAI API compatibility:** formats all responses to match the OpenAI API standard, ensuring seamless integration with compatible tools.
- 🔗 **Relay mechanism:** handles the back-and-forth communication between the application and the chatbot, making the web-based interface appear as a fully functional API.

## Supported chatbots

- [x] [Grok](https://grok.com/)
- [x] [Mistral](https://chat.mistral.ai/chat)
- [x] [OpenAI](https://chatgpt.com/)
- [x] [DeepSeek](https://chat.deepseek.com/)
- [ ] Gemini
- [ ] Anthropic
- [ ] Perplexity
- [ ] ...

## Workflow

![Chapito workflow](https://github.com/user-attachments/assets/afde0d72-3e43-4d1f-8ffc-b675897a33af)

## Demo

<https://github.com/user-attachments/assets/869963bc-48cf-43f8-9a2c-9bd4a41c30d9>

Explanation:  

- Top left window: runs `Chapito`. Once launched, user don't have to interact with it.
- Top right window: browser used par *Chapito*. User don't have to interact with it.
- Bottom left: the AI tool (in this demo: *AIder*). User only interacts with this window.
- Bottom right: the result generated by the AI tool.

## Disclamer

*Chapito* is provided "as is" for lawful use only. Users are solely responsible for ensuring their use of the program complies with all applicable terms of service and laws. The author of this program cannot be held responsible for any misuse, abuse, or consequences arising from the use of this program.

## Usage

### 1. Run the proxy

```bash
uv run main.py
```

It opens a Chromium browser and waits for the chatbot page to become available; if there’s a captcha or authentication required, the user must handle it manually in the browser.  
The server is ready when the console shows a message like `INFO: Uvicorn running on http://127.0.0.1:5001 (Press CTRL+C to quit)`  

#### Configuration

Application will use parameters from command-line args or `config.ini` file.

**Parameters**  
`cli parameter` / `config file parameter`: description  

- `--chatbot <NAME>`: name of the chat service to use. Possible values: `grok`, `mistral`.
- `--use-browser-profile` / `use_browser_profile`: when used, a profile will be saved and reused. Usefull when you don't want to authenticate everytime.
- `--profile-path <PATH>` / `browser_profile_path`: when `--use-browser-profile`, provides the `PATH` where the profile is stored.
- `--user-agent <VALUE>` / `browser_user_agent`: the user-agent to use.
- `--verbosity <VALUE>` / `verbosity`: the verbosity to use. Possible values: `0` = ERROR, `1` = WARNING, `2` = INFO, `3` = DEBUG.

Exemple:  

```bash
uv run main.py --chatbot grok --verbosity 1
```

### 2. Run your AI tool

Run your favorite AI tool using the following configuration:  

- `API Type`: use something like "OpenAI compatible".
- `Base URL` : use the value shown by the proxy (eg. `http://127.0.0.1:5001`).
- If your AI tool requires an `API_KEY` : use anything you want (eg. `fake_key`).
- If your AI tool requires a model name, you can use anything corresponding to a real OpenAI model (eg. `gpt-3.5-turbo`).
- **Tell your AI tool that this API is not streamable.**

Exemple with `AIder`:

```bash
aider --openai-api-base http://127.0.0.1:5001 --openai-api-key fake_key --model gpt-3.5-turbo --no-stream
```

## Installation

### Prerequisite

- Python 3.12+
- `uv` (see. [official website](https://docs.astral.sh/uv/getting-started/installation/) for more informations)

### 1. Clone the repository

```bash
git clone https://github.com/Yajusta/Chapito.git
cd Chapito
```

### 2. Install dependencies

(only if you plan to not use `uv` to run application)

```bash
uv venv
uv sync
```

## F.A.Q

**Q: The chat bot always gives code inside `<![CDATA[` and `]]>` tags.**  
**A:** This is because the chat bot is designed to generate code that can be used in XML documents. The `<![CDATA[` and `]]>` tags are used to indicate that the text inside them should be treated as plain text, not as XML markup. If you want to remove these tags, you can explicitly tell the chat bot that you don't want to use them.
Eg. with `Roo-Code`:
Add the following custom instruction: `Do not use <![CDATA[ and ]]> tags in your responses.`
