# Complete Guide: Claude Desktop MCP Setup for WhatsApp Voice Messages with Cloned Voices

## Overview

This comprehensive guide documents the complete process of setting up Claude Desktop with MCP (Model Context Protocol) servers for WhatsApp and Minimax integration. The goal is to enable sending personalized voice messages using cloned voices directly through WhatsApp from Claude Desktop.

## What You'll Achieve

- ✅ Configure Claude Desktop with WhatsApp MCP server
- ✅ Integrate Minimax MCP server for AI voice generation
- ✅ Clone your voice and use it for message generation
- ✅ Send custom audio messages directly to WhatsApp contacts
- ✅ Handle common setup issues and troubleshooting

## Prerequisites

- macOS or Linux system
- Claude Desktop installed
- Python 3.8+ installed
- Git installed
- WhatsApp account
- Minimax account with API access

## Required Resources

- **WhatsApp MCP Server**: [https://github.com/lharries/whatsapp-mcp/tree/main](https://github.com/lharries/whatsapp-mcp/tree/main)
- **Minimax MCP Server**: [https://github.com/MiniMax-AI/MiniMax-MCP](https://github.com/MiniMax-AI/MiniMax-MCP)
- **Video example**: [https://www.youtube.com/watch?v=1xwAl284fJ8](https://www.youtube.com/watch?v=1xwAl284fJ8)

## Step 1: Install uv Package Manager

First, install the `uv` package manager which will be used to run the MCP servers:

```bash
# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Add to PATH (add to your ~/.bashrc or ~/.zshrc)
export PATH="$HOME/.local/bin:$PATH"

# Reload shell or run:
source ~/.bashrc  # or ~/.zshrc
```

**Common Issue**: If `uv` is not found, ensure the PATH is correctly set and restart your terminal.

## Step 2: Setup WhatsApp MCP Server

### 2.1 Clone and Setup Repository

```bash
# Clone the repository
cd ~/
mkdir -p github_com_lharries_whatsapp-mcp
cd github_com_lharries_whatsapp-mcp
git clone https://github.com/lharries/whatsapp-mcp.git whatsapp-mcp-server
cd whatsapp-mcp-server
```

### 2.2 Install Dependencies

```bash
# Install Python dependencies using uv
uv sync
```

**Troubleshooting**: If you encounter dependency issues:
- Ensure Python 3.8+ is installed
- Try: `uv pip install -r requirements.txt`
- Check that all system dependencies are met

### 2.3 WhatsApp Authentication Setup

The WhatsApp MCP server requires authentication through WhatsApp Web. Follow these steps:

1. Run the server initially to generate QR code:
```bash
uv run main.py
```

2. Scan the QR code with your WhatsApp mobile app
3. Wait for successful authentication
4. The session will be saved for future use

**Common Issues**:
- **QR Code not appearing**: Check firewall settings and ensure port access
- **Authentication fails**: Try clearing browser cache and regenerating QR code
- **Session expires**: You may need to re-authenticate periodically

## Step 3: Setup Minimax MCP Server

### 3.1 Get Minimax API Key

1. Sign up at [Minimax AI platform](https://api.minimaxi.chat)
2. Navigate to API settings
3. Generate your API key
4. Save the key securely (you'll need it for configuration)

### 3.2 Install Minimax MCP Server

```bash
# Install using uvx
uvx install minimax-mcp
```

**Alternative installation if uvx fails**:
```bash
# Clone repository method
git clone https://github.com/MiniMax-AI/MiniMax-MCP.git
cd MiniMax-MCP
uv sync
```

## Step 4: Configure Claude Desktop

### 4.1 Locate Claude Desktop Configuration

Find your Claude Desktop configuration file:
- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Linux**: `~/.config/claude/claude_desktop_config.json`

### 4.2 Create/Update Configuration File

Create or update the configuration file with the following content:

```json
{
  "mcpServers": {
    "whatsapp": {
      "command": "/Users/[YOUR_USERNAME]/.local/bin/uv",
      "args": [
        "--directory",
        "/Users/[YOUR_USERNAME]/github_com_lharries_whatsapp-mcp/whatsapp-mcp-server",
        "run",
        "main.py"
      ]
    },
    "MiniMax": {
      "command": "/Users/[YOUR_USERNAME]/.local/bin/uvx",
      "args": [
        "minimax-mcp",
        "-y"
      ],
      "env": {
        "MINIMAX_API_KEY": "YOUR_MINIMAX_API_KEY_HERE",
        "MINIMAX_MCP_BASE_PATH": "/tmp",
        "MINIMAX_API_HOST": "https://api.minimaxi.chat",
        "MINIMAX_API_RESOURCE_MODE": "local"
      }
    }
  }
}
```

**Important**: Replace `[YOUR_USERNAME]` with your actual username and `YOUR_MINIMAX_API_KEY_HERE` with your actual Minimax API key.

### 4.3 Configuration Troubleshooting

**Common Issues**:

1. **Path Issues**: 
   - Ensure all paths use absolute paths
   - Verify `uv` and `uvx` are installed in `~/.local/bin/`
   - Check that directory structure matches exactly

2. **JSON Syntax Errors**:
   - Validate JSON syntax using online validators
   - Ensure all quotes are properly escaped
   - Check for trailing commas

3. **Environment Variables**:
   - Verify API key is correct and active
   - Ensure base path `/tmp` exists and is writable
   - Check API host URL is accessible

## Step 5: Voice Cloning Setup (Optional but Recommended)

### 5.1 Prepare Voice Sample

1. Record a clear audio sample of your voice (10-30 seconds)
2. Use good quality microphone
3. Speak clearly and naturally
4. Save as MP3 or WAV format

### 5.2 Clone Your Voice

Once Claude Desktop is running with MCP servers:

1. Ask Claude to list available voices:
   ```
   Can you list all available voices in Minimax?
   ```

2. Clone your voice:
   ```
   Can you clone my voice using this audio file? [upload your voice sample]
   Use voice ID: my_custom_voice_[your_name]
   ```

3. Test the cloned voice:
   ```
   Generate a test audio message with my cloned voice saying "Hello, this is a test message"
   ```

## Step 6: Restart Claude Desktop

1. Completely close Claude Desktop
2. Restart the application
3. Wait for MCP servers to initialize
4. Check that both servers are connected (you should see MCP indicators in Claude)

## Step 7: Test the Integration

### 7.1 Test WhatsApp Connection

```
Can you search for contacts in my WhatsApp?
```

### 7.2 Test Minimax Voice Generation

```
Can you generate an audio message saying "Hello, this is a test" using my cloned voice?
```

### 7.3 Send Test Voice Message

```
Can you send a voice message to [phone number] saying "Hello, this is a test message from Claude" using my cloned voice?
```

## Complete Working Example

Here's the exact process we followed that resulted in success:

### Initial Request
```
User: "Can you send an audio message with my cloned voice to +34959217658 with the message 'Hemos llegado a los 140.000 seguidores en instagram. Que chimba!! Gracias a la divinidad'"
```

### Step-by-Step Execution

1. **Voice Discovery**:
   ```
   Claude listed available voices and found the cloned voice: moss_audio_17d8fcf2-4abf-11f0-b862-46ba4da2d9df
   ```

2. **Audio Generation**:
   ```
   Parameters used:
   - Text: "Hemos llegado a los 140.000 seguidores en instagram. Que chimba!! Gracias a la divinidad"
   - Voice ID: moss_audio_17d8fcf2-4abf-11f0-b862-46ba4da2d9df
   - Emotion: happy
   - Language: Spanish
   ```

3. **WhatsApp Delivery**:
   ```
   Sent to: 34959217658 (without + prefix)
   Format: Audio message
   Result: Success
   ```

4. **Content Correction**:
   ```
   User requested change from "140.000 seguidores" to "140 mil seguidores"
   Regenerated audio with corrected text
   Sent updated message successfully
   ```

## Common Issues and Solutions

### Issue 1: MCP Servers Not Connecting

**Symptoms**: Claude Desktop shows no MCP servers connected

**Solutions**:
1. Check configuration file syntax
2. Verify all paths are absolute and correct
3. Ensure `uv` and `uvx` are properly installed
4. Restart Claude Desktop completely
5. Check system logs for error messages

### Issue 2: WhatsApp Authentication Fails

**Symptoms**: Cannot connect to WhatsApp, QR code issues

**Solutions**:
1. Clear WhatsApp Web sessions in your phone
2. Ensure good internet connection
3. Try running WhatsApp MCP server manually first
4. Check firewall/antivirus settings

### Issue 3: Minimax API Errors

**Symptoms**: Voice generation fails, API errors

**Solutions**:
1. Verify API key is correct and active
2. Check account credit balance
3. Ensure API host URL is accessible
4. Try different base path if permission issues

### Issue 4: Voice Cloning Not Working

**Symptoms**: Cannot find cloned voice, generation fails

**Solutions**:
1. Ensure voice was properly cloned in Minimax platform
2. Check voice ID format
3. Verify audio file format compatibility
4. Try listing voices to confirm availability

### Issue 5: Audio Message Delivery Fails

**Symptoms**: Audio generates but WhatsApp sending fails

**Solutions**:
1. Check phone number format (no + prefix)
2. Ensure WhatsApp session is active
3. Verify audio file format compatibility
4. Check file permissions and paths

## Best Practices

1. **Regular Backups**: Save your Claude Desktop configuration
2. **API Key Security**: Never share your Minimax API key
3. **Voice Quality**: Use high-quality audio for voice cloning
4. **Testing**: Always test with yourself first before sending to others
5. **Updates**: Keep MCP servers updated regularly

## Troubleshooting Commands

### Check uv installation:
```bash
which uv
uv --version
```

### Check uvx installation:
```bash
which uvx
uvx --version
```

### Test WhatsApp MCP manually:
```bash
cd ~/github_com_lharries_whatsapp-mcp/whatsapp-mcp-server
uv run main.py
```

### Test Minimax MCP manually:
```bash
uvx minimax-mcp -y
```

### Validate Claude config:
```bash
cat ~/Library/Application\ Support/Claude/claude_desktop_config.json | python -m json.tool
```

## Success Metrics

You'll know everything is working when:
- ✅ Claude Desktop shows both MCP servers connected
- ✅ You can search WhatsApp contacts
- ✅ You can list available voices including your cloned voice
- ✅ You can generate audio with your voice
- ✅ You can send audio messages to WhatsApp successfully

## Conclusion

This setup enables powerful automation capabilities by combining Claude's intelligence with WhatsApp's reach and Minimax's voice cloning technology. The key to success is careful attention to configuration details and systematic troubleshooting of any issues that arise.

The complete integration allows for natural language commands to Claude that result in personalized voice messages being sent directly to WhatsApp contacts, opening up possibilities for automated customer service, personal assistants, and creative content distribution.
