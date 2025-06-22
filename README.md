# Neo AI Chat

![Platform](https://img.shields.io/badge/platform-windows%20%7C%20macOS%20%7C%20linux-blue)
![Node Version](https://img.shields.io/badge/node.js-16%2B-brightgreen)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

A floating, always-on-top desktop chat app powered by Google Gemini AI and n8n automation. Stay productive with a modern AI assistant that understands text and voice, handles automation, and looks good while doing it.

[![Neo AI Chat Demo](https://img.youtube.com/vi/lN9plS6gY2I/maxresdefault.jpg)](https://youtu.be/lN9plS6gY2I?feature=shared)
*Click the image above to watch the demo video*


---

## 🚀 Quick Start

1. **Install Node.js 16+** ([Download](https://nodejs.org/))
2. **Clone the repository:**
   ```bash
   git clone https://github.com/Rakesh-ada/Neo.git
   cd Neo
   ```
3. **Install dependencies and launch:**
   ```bash
   npm install
   npm start
   ```
   The app will:
   - Install required dependencies
   - Check and install n8n (if not already present)
   - Start n8n in the background
   - Launch the floating AI chat interface

---

## 💡 How It Works

- **Integrated n8n:** App auto-starts n8n and connects, or uses your running n8n instance.
- **Dual AI Engine:** Switches between n8n (automation) and Google Gemini (natural language) for best results.
- **No manual n8n setup required!**

---


## ✨ Features

### 🪟 Floating Chat Interface
- Always-on-top, resizable, and minimizable
- Modern, animated UI
- Message history with one-click copy

### 🤖 Dual AI Engine
- **n8n AI:** Automation and custom workflows
- **Google Gemini AI:** Natural language understanding (fallback)
- **Smart Switching:** Automatic failover and status lights (🔵 n8n, 🟣 Gemini)

### 🎤 Voice Capabilities
- Google Speech-to-Text for voice input
- Text-to-Speech with male/female voices
- Visual mic controls and auto-timeout

### ⚙️ Configuration
- Launch the app and click the gear icon (⚙️) to open settings.
- Enter your n8n Webhook URL (optional, for custom workflows).
- The app will use Gemini AI if n8n is unavailable.

### 🛠️ Build & Distribution
- Cross-platform: Windows, macOS, Linux
- Build installer or portable executable (`npm run build`)

### 🧑‍💻 Developer Tools
- Real-time logs, modular code, IPC
- Auto repair for dependencies

### 🔐 Security & Privacy
- No telemetry or tracking
- Local voice fallback
- Visual network indicators

### 🔑 API Keys

### Getting API Keys
To use all features of Neo AI Chat, you'll need to obtain the following API keys:

#### Google Gemini API
1. Visit the [Google AI Studio](https://aistudio.google.com/) and sign in with your Google account
2. Navigate to the API keys section (usually found in settings or dashboard)
3. Create a new API key and copy it
4. For more details, visit the [Google AI documentation](https://ai.google.dev/tutorials/setup)

#### ElevenLabs API (for enhanced Text-to-Speech)
1. Create an account at [ElevenLabs](https://elevenlabs.io/)
2. Navigate to your profile settings
3. Find your API key in the "API" section
4. Copy your API key
5. Free tier includes limited usage; paid plans offer more voice options and usage

### Updating API Keys in the App
1. Launch Neo AI Chat
2. Click the gear icon (⚙️) in the top-right corner to open settings
3. Paste your Google Gemini API key in the "Gemini API Key" field
4. Paste your ElevenLabs API key in the "ElevenLabs API Key" field
5. Click "Save Settings"
6. The app will immediately begin using your API keys for enhanced functionality

**Note:** If you don't provide API keys, some features may be limited or unavailable. The app will attempt to use default services where possible.

---

## 🔌 Workflow Templates

### Complete AI Assistant: Demo Template
This is our main demo template that showcases the integration of multiple tools with an AI agent. It includes Google Gemini, OpenWeatherMap, Twitter/X, GitHub, Wikipedia, and Gmail integration.

**Tags:** AI Assistant, Multi-tool, Demo

```json
{
  "name": "chat",
  "nodes": [
    {
      "parameters": {
        "promptType": "define",
        "text": "= {{ $json.body.message }}",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 1.8,
      "position": [
        580,
        -140
      ],
      "id": "503d020e-9e6a-472e-9d38-a4cbd4a684cd",
      "name": "AI Agent"
    },
    {
      "parameters": {
        "modelName": "models/gemini-2.0-flash",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatGoogleGemini",
      "typeVersion": 1,
      "position": [
        400,
        300
      ],
      "id": "3797ca99-e3e3-4503-b035-a39ba84e945e",
      "name": "Google Gemini Chat Model",
      "credentials": {
        "googlePalmApi": {
          "id": "0CAYgjYDI7R8xwcE",
          "name": "Google Gemini(PaLM) Api account"
        }
      }
    },
    {
      "parameters": {
        "sessionIdType": "customKey",
        "sessionKey": "123456"
      },
      "type": "@n8n/n8n-nodes-langchain.memoryBufferWindow",
      "typeVersion": 1.3,
      "position": [
        560,
        300
      ],
      "id": "f5f01fec-f394-4043-9256-119f766481f3",
      "name": "Simple Memory"
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "n8n-nodes-base.respondToWebhook",
      "typeVersion": 1.1,
      "position": [
        1400,
        -340
      ],
      "id": "8705af19-5756-4886-952c-ba028be78215",
      "name": "Respond to Webhook",
      "alwaysOutputData": true
    },
    {
      "parameters": {
        "httpMethod": "POST",
        "path": "rakesh",
        "responseMode": "responseNode",
        "options": {}
      },
      "type": "n8n-nodes-base.webhook",
      "typeVersion": 2,
      "position": [
        400,
        -140
      ],
      "id": "efb6c70d-7702-44c7-8dd7-b539b1c46ace",
      "name": "Webhook",
      "webhookId": "ec6698cf-6781-4147-ba0d-bcf3df2fa0f5"
    },
    {
      "parameters": {
        "jsCode": "// Example for JavaScript-based code node\nreturn {\n  message: $input.first().json.output || \"No response from AI\"\n};\n"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        1100,
        -260
      ],
      "id": "2d65e293-4962-44ce-a8f6-6759d2e4b1ed",
      "name": "Code"
    },
    {
      "parameters": {
        "cityName": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('City', ``, 'string') }}",
        "language": "en"
      },
      "type": "n8n-nodes-base.openWeatherMapTool",
      "typeVersion": 1,
      "position": [
        960,
        420
      ],
      "id": "78022d93-14d1-43f1-8bee-4d5bad839857",
      "name": "OpenWeatherMap",
      "credentials": {
        "openWeatherMapApi": {
          "id": "CMGUGjGVEwEETFiT",
          "name": "OpenWeatherMap account"
        }
      }
    },
    {
      "parameters": {
        "text": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('Text', ``, 'string') }}",
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.twitterTool",
      "typeVersion": 2,
      "position": [
        1240,
        400
      ],
      "id": "a36623c2-e2b2-4ccd-aba9-6145c666db8f",
      "name": "X",
      "credentials": {
        "twitterOAuth2Api": {
          "id": "UHYI7WwMAW7kSHc2",
          "name": "X account"
        }
      }
    },
    {
      "parameters": {
        "resource": "repository",
        "operation": "get",
        "owner": {
          "__rl": true,
          "mode": "name",
          "value": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('Repository_Owner', ``, 'string') }}"
        },
        "repository": {
          "__rl": true,
          "value": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('Repository_Name', ``, 'string') }}",
          "mode": "url",
          "__regex": "https:\\/\\/github.com\\/(?:[-_0-9a-zA-Z]+)\\/([-_.0-9a-zA-Z]+)"
        }
      },
      "type": "n8n-nodes-base.githubTool",
      "typeVersion": 1.1,
      "position": [
        1480,
        340
      ],
      "id": "1bfa294a-029a-4b4c-8d5e-4fae27dc80e8",
      "name": "GitHub",
      "webhookId": "8eb9a97a-bcda-448b-ab86-e360e7f493c6",
      "credentials": {
        "githubApi": {
          "id": "RFWlN43uETbNO6K9",
          "name": "GitHub account"
        }
      }
    },
    {
      "parameters": {},
      "type": "@n8n/n8n-nodes-langchain.toolWikipedia",
      "typeVersion": 1,
      "position": [
        1680,
        240
      ],
      "id": "ff9c0ba1-610f-42b2-b978-e44650e4a14a",
      "name": "Wikipedia"
    },
    {
      "parameters": {
        "sendTo": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('To', ``, 'string') }}",
        "subject": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('Subject', ``, 'string') }}",
        "emailType": "text",
        "message": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('Message', ``, 'string') }}",
        "options": {}
      },
      "type": "n8n-nodes-base.gmailTool",
      "typeVersion": 2.1,
      "position": [
        1720,
        0
      ],
      "id": "92feeaee-39db-491b-b1be-14471e9d17c5",
      "name": "Gmail",
      "webhookId": "7e9d910f-8d4e-41ea-b36c-46760388b0b9",
      "credentials": {
        "gmailOAuth2": {
          "id": "caqCEnsCiejZYas0",
          "name": "Gmail account"
        }
      }
    }
  ],
  "pinData": {},
  "connections": {
    "Google Gemini Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Simple Memory": {
      "ai_memory": [
        [
          {
            "node": "AI Agent",
            "type": "ai_memory",
            "index": 0
          }
        ]
      ]
    },
    "AI Agent": {
      "main": [
        [
          {
            "node": "Code",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Webhook": {
      "main": [
        [
          {
            "node": "AI Agent",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Code": {
      "main": [
        [
          {
            "node": "Respond to Webhook",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "OpenWeatherMap": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "X": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "GitHub": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "Wikipedia": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "Gmail": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": true,
  "settings": {
    "executionOrder": "v1",
    "timezone": "Asia/Kolkata",
    "callerPolicy": "workflowsFromSameOwner"
  },
  "versionId": "565d4222-d5bc-4b7a-b8db-0f52e2a577ec",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "c120cfab186612b4e1cc5fa3db0b0db5cf70e67d23ee72e9a63de20c8a268d5a"
  },
  "id": "uv5ENuAEWpgPrIAy",
  "tags": []
}
```

#### Complete AI Assistant Workflow Diagram
This comprehensive workflow connects a webhook to an AI Agent powered by Google Gemini, with multiple tool integrations:
- Google Gemini Chat Model for natural language processing
- Simple Memory for conversation context
- OpenWeatherMap for weather data
- Twitter/X for social media posting
- GitHub for repository information
- Wikipedia for knowledge lookup
- Gmail for sending emails

1. Click the Copy JSON button above.
2. Open your n8n instance and go to the workflow canvas.
3. In your n8n workflow canvas, press Ctrl+V (or Cmd+V on Mac) to paste the JSON directly.
4. Set up your API credentials for each service.
5. Save and activate your workflow.

### Gmail: Send Email Workflow
Send emails via Gmail automatically. Great for notifications, alerts, or automated messages.

**Tags:** Email, Gmail, Automation

#### Gmail Workflow Diagram
This workflow connects a webhook to an AI Agent powered by Google Gemini, with Gmail integration for sending emails.

1. Click Copy JSON above.
2. Open your n8n instance and go to the workflow canvas.
3. In your n8n workflow canvas, simply press Ctrl+V (or Cmd+V on Mac) to paste the JSON directly onto the canvas.
4. Set up your Gmail API credentials in the node (replace YOUR_GMAIL_CREDENTIAL).
5. Save and activate your workflow.

Need help? Watch the video guide provided. If you face any issues, refer to the video for troubleshooting steps.

### Weather: Get Weather Data Workflow
Fetch current weather data for a specified city using the OpenWeatherMap API. Extend to send notifications or trigger other actions.

**Tags:** Weather, API, Automation

#### Weather Workflow Diagram
This workflow connects a webhook to an AI Agent with OpenWeatherMap, Wikipedia, and YouTube tools for comprehensive responses.

1. Click Copy JSON above.
2. Open your n8n instance and go to the workflow canvas.
3. In your n8n workflow canvas, simply press Ctrl+V (or Cmd+V on Mac) to paste the JSON directly onto the canvas.
4. Set your OpenWeatherMap API key in the HTTP Request node (replace YOUR_OPENWEATHER_API_KEY).
5. Save and activate your workflow.

Need help? Watch the video guide provided. If you face any issues, refer to the video for troubleshooting steps.

### How to Import Workflows

#### Method 1: Using JSON Code
1. Copy the workflow JSON for the template you want to use by clicking the Copy JSON button.
2. In your n8n workflow canvas, simply press Ctrl+V (or Cmd+V on Mac) to paste the JSON directly onto the canvas.
3. Set up any required API credentials or keys as described in the workflow instructions.
4. Save and activate your workflow.

#### Method 2: Within n8n Interface
1. Locate the template you want to use in the n8n template library.
2. Select the nodes you want to copy by dragging a selection box around them.
3. Use keyboard shortcut Ctrl+C (or ⌘+C on Mac) to copy the selected nodes.
4. Create a new workflow or open an existing one where you want to paste the nodes.
5. Use keyboard shortcut Ctrl+V (or ⌘+V on Mac) to paste the nodes.
6. Adjust node positions and connections as needed.

#### Pro Tips
- When importing JSON, make sure to replace any placeholder API keys or credentials with your own.
- After importing, test each node individually to ensure proper configuration.
- You can modify the imported workflow to suit your specific needs by adding or removing nodes.
- Save your workflow frequently while making changes to avoid losing your work.
- Still have trouble? Watch the provided video guides for step-by-step troubleshooting and setup.

### Video Tutorials

#### n8n Workflow Tutorial
**Get Started With Google OAuth**  
Learn how to set up and configure Google OAuth credentials for your n8n workflows.

#### n8n Webhook URL Tutorial
**What is n8n webhook URL and how to connect**  
Learn how to find and use webhook URLs in n8n to connect your workflows with external applications.

#### N8N Webhooks Tutorial
**Step-by-Step: N8N Webhooks (From Beginner to Pro)**  
Comprehensive guide to understanding and implementing webhooks in n8n, covering everything from basic setup to advanced usage.

#### Advanced n8n AI Integration
**Get Your API Key for OpenWeather**  
Step-by-step guide on how to obtain and configure an API key for the OpenWeather service in n8n.

---

1. Open your terminal in this project directory.
2. Run the following commands:
   ```sh
   npm install
   npm start
   ```
3. The application will launch.

## 🖥️ Installation

### Requirements
- [Node.js 16+](https://nodejs.org/)
- npm (bundled with Node.js)

### Automatic Setup
```bash
git clone https://github.com/Rakesh-ada/Neo.git
cd Neo
npm install
npm start
```

### Manual Setup (if auto-install fails)
```bash
node install-electron.js          # Downloads Electron (~250MB)
node install-dependencies.js      # Installs other dependencies
npm start
```

---

## 🏗️ Build for Distribution

```bash
npm run build
```
Creates an installer and a portable executable in the `dist/` folder.

---

## 🩺 Troubleshooting
| Problem                  | Solution                                               |
|--------------------------|--------------------------------------------------------|
| n8n not connecting       | Check if webhook URL is correct in settings            |
| App won't start          | Re-run `node install-electron.js` and `install-dependencies.js` |
| Voice not working        | Ensure microphone permissions are enabled              |
| Build fails              | Delete `dist/` and try `npm run build` again          |
| API features not working | Check that you've entered valid API keys in settings   |
| Rate limit errors        | You may have exceeded your API quota; consider upgrading your plan |

---

## 📦 Dependencies
- Electron (UI framework)
- n8n (workflow automation)
- Google Gemini (Generative AI)
- Google Cloud Speech API (voice recognition)
- ElevenLabs (Text-to-Speech)
- SQLite3 (local storage)

---

## 🙌 Credits
Built with ❤️ using [n8n](https://n8n.io/) and Google AI.

- [n8n GitHub](https://github.com/n8n-io/n8n)
- [n8n Official Website](https://n8n.io/)
- [Google AI](https://ai.google.dev/)
- [ElevenLabs](https://elevenlabs.io/)

We thank the n8n team and community for their outstanding work in workflow automation!

[![Neo AI Chat Features](https://img.youtube.com/vi/lN9plS6gY2I/maxresdefault.jpg)](https://youtu.be/lN9plS6gY2I?feature=shared)
*Click the image above to watch the features video*