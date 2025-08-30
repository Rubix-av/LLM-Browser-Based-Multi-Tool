# 🤖 LLM Agent POC - Browser-Based Multi-Tool Reasoning

A powerful proof-of-concept demonstrating how Large Language Models can orchestrate multiple tools through iterative reasoning loops to accomplish complex tasks—all running directly in your browser.

## 🚀 Features

- **🔄 Agent Reasoning Loop**: Implements the core agent logic with continuous tool orchestration
- **🔍 Web Search Integration**: Real-time Google Custom Search API with fallback mock results
- **💻 Code Execution**: Safe JavaScript sandbox execution within the browser
- **🤖 AI Pipeline Processing**: Extensible AI workflow integration with mock services
- **🎯 Multi-LLM Support**: OpenAI, Anthropic Claude, and Google Gemini compatibility
- **🔧 Function Calling**: OpenAI-style tool/function calling interface
- **⚡ Real-time Processing**: Live conversation with typing indicators and progress tracking
- **🛡️ Secure**: Client-side only processing with no data persistence

## 🧠 How It Works

The agent implements a continuous reasoning loop that mirrors advanced AI systems:

```javascript
// Core Agent Loop (simplified)
while (true) {
    const response = await llm(messages, tools);
    
    if (response.tool_calls) {
        // Execute tools and add results to conversation
        const results = await executeTools(response.tool_calls);
        messages.push(...results);
    } else {
        // No more tools needed, task complete
        break;
    }
}
```

### 🛠 Available Tools

1. **Web Search** (`web_search`)
   - Real-time Google Custom Search API integration
   - Fallback to mock results for testing
   - Structured result formatting with titles, snippets, and links

2. **Code Execution** (`execute_code`)
   - Safe JavaScript sandbox environment
   - Console output capture and display
   - Error handling and result visualization

3. **AI Pipeline** (`ai_pipe`)
   - Extensible AI workflow processing
   - Mock implementation for analysis, summarization, translation
   - Ready for real AI service integration

## 📋 Requirements

### Essential
- Modern web browser (Chrome 80+, Firefox 75+, Safari 13+, Edge 80+)
- LLM API key (OpenAI, Anthropic, or Google Gemini)

### Optional (for enhanced functionality)
- Google Search API key and Custom Search Engine ID
- AI Pipeline service credentials (for production use)

## 🚀 Quick Start

### Method 1: GitHub Pages (Recommended)
1. Visit the [live demo](https://your-username.github.io/llm-agent-poc)
2. Select your LLM provider (OpenAI, Anthropic, or Gemini)
3. Enter your API key
4. Start chatting with the agent!

### Method 2: Local Development
1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/llm-agent-poc.git
   cd llm-agent-poc
   ```

2. Serve the files locally:
   ```bash
   # Using Python
   python -m http.server 8000
   
   # Using Node.js
   npx http-server
   
   # Using PHP
   php -S localhost:8000
   ```

3. Open `http://localhost:8000` in your browser

## 🔧 Setup & Configuration

### LLM Provider Setup

#### OpenAI
1. Visit [OpenAI API Keys](https://platform.openai.com/api-keys)
2. Create a new secret key
3. Ensure sufficient credits in your account
4. Supports: GPT-4, GPT-4 Turbo, GPT-3.5 Turbo

#### Anthropic (Claude)
1. Visit [Anthropic Console](https://console.anthropic.com/)
2. Generate an API key
3. Check usage limits and billing
4. Supports: Claude-3 Sonnet, Claude-3 Opus

#### Google Gemini
1. Visit [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Create an API key
3. Enable Generative AI API
4. Supports: Gemini Pro, Gemini Pro Vision

### Optional: Google Search Integration
1. **Create Google Cloud Project**: [Google Cloud Console](https://console.cloud.google.com/)
2. **Enable Custom Search API**: APIs & Services → Enable APIs
3. **Create API Key**: Credentials → Create Credentials
4. **Setup Custom Search Engine**: [Google CSE](https://cse.google.com/cse/)
5. **Get Search Engine ID**: From your custom search engine settings

## 💡 Usage Examples

### Example 1: Research Assistant
```
User: Research the latest developments in quantum computing
Agent: Let me search for recent quantum computing news...
[Tool: web_search("quantum computing 2024 developments")]
Agent: Based on my search, here are the key developments...
User: Can you analyze which companies are leading?
[Tool: ai_pipe("analyze", "quantum computing company data")]
```

### Example 2: Code Helper
```
User: Help me create a sorting algorithm
Agent: I'll create a bubble sort implementation for you...
[Tool: execute_code("function bubbleSort(arr) { ... }")]
Agent: Here's the working code. Let me test it with sample data...
[Tool: execute_code("console.log(bubbleSort([64, 34, 25, 12]))")]
```

### Example 3: Content Creation
```
User: Interview me to create a blog post about remote work
Agent: Great! Let's start with some questions...
What industry do you work in?
User: Software development
Agent: Let me research current remote work trends in tech...
[Tool: web_search("remote work software development 2024")]
```

## 🏗 Architecture

### Frontend Components
- **Agent Controller**: Main reasoning loop and conversation management
- **Tool Orchestrator**: Function calling and tool execution coordination
- **LLM Integrator**: Multi-provider API communication layer
- **UI Manager**: Real-time chat interface with typing indicators
- **Safety Sandbox**: Secure code execution environment

### Technology Stack
- **Pure JavaScript**: No framework dependencies for maximum hackability
- **Bootstrap 5**: Responsive UI components and styling
- **Prism.js**: Syntax highlighting for code blocks
- **Native APIs**: Fetch, Blob, eval for core functionality

### Security Features
- **Client-side only**: No backend servers or data storage
- **API key isolation**: Keys never logged or persisted
- **Sandboxed execution**: Safe code running environment
- **CORS compliance**: Direct API communication with providers

## 🔍 Debugging & Troubleshooting

### Common Issues

**"API call failed"**
```javascript
// Check API key format
OpenAI: sk-...
Anthropic: sk-ant-...
Gemini: (alphanumeric string)
```

**"Tool execution error"**
- Check browser console for detailed error messages
- Ensure JavaScript execution permissions are enabled
- Verify API quotas and rate limits

**"Search results not loading"**
- Google Search API requires valid credentials
- Falls back to mock results automatically
- Check Custom Search Engine configuration

### Debug Mode
Open browser DevTools and check:
```javascript
// Access the agent instance
window.agent

// View current conversation
window.agent.messages

// Check tool configurations
window.agent.tools
```

## 🛠 Extending the Agent

### Adding New Tools
```javascript
// Define new tool in defineLLMTools()
{
    type: "function",
    function: {
        name: "your_tool_name",
        description: "What your tool does",
        parameters: {
            type: "object",
            properties: {
                param1: { type: "string", description: "Parameter description" }
            },
            required: ["param1"]
        }
    }
}

// Implement tool execution
async executeTool(toolCall) {
    switch (toolCall.function.name) {
        case 'your_tool_name':
            return await this.yourToolImplementation(args);
        // ... existing cases
    }
}
```

### Custom LLM Providers
```javascript
// Add to callLLM() method
case 'your_provider':
    endpoint = 'https://api.yourprovider.com/chat';
    headers = { 'Authorization': `Bearer ${apiKey}` };
    body = { /* your provider's format */ };
    break;
```

## 🧪 Testing

### Manual Testing Scenarios
1. **Basic Conversation**: Simple Q&A without tools
2. **Single Tool Use**: Search, code execution, or AI pipeline
3. **Multi-tool Chains**: Complex tasks requiring multiple tools
4. **Error Handling**: Invalid API keys, network issues, malformed responses
5. **Edge Cases**: Empty responses, timeout scenarios

### Automated Testing (Future)
```bash
# Install testing framework
npm install --save-dev cypress

# Run integration tests
npm run test:e2e
```
