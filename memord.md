# ElevenLabs Twilio Agent Setup Guide (memord.md)

## Overview
This guide provides step-by-step instructions to set up the ElevenLabs Conversational AI Agent with Twilio for handling both inbound and outbound calls.

## Prerequisites
- Node.js (v16 or higher)
- npm package manager
- ElevenLabs account with Conversational AI Agent
- Twilio account with phone number
- Public domain/ngrok for webhook URLs

## Step 1: Environment Setup

### 1.1 Clone and Navigate to Repository
```bash
cd /path/to/elevenlabs-twilio-agent-v2
```

### 1.2 Install Dependencies
```bash
npm install
```
**Status**: ✅ Completed - All dependencies installed and vulnerabilities fixed

### 1.3 Create Environment Variables
Copy the example environment file:
```bash
cp .env.example .env
```

Edit `.env` file with your actual credentials:
```env
ELEVENLABS_AGENT_ID=your-elevenlabs-agent-id
ELEVENLABS_API_KEY=your-elevenlabs-api-key
TWILIO_ACCOUNT_SID=your-twilio-account-sid
TWILIO_AUTH_TOKEN=your-twilio-auth-token
TWILIO_PHONE_NUMBER=your-twilio-phone-number
```

**Status**: ⏳ Pending - User needs to add API keys

## Step 2: ElevenLabs Configuration

### 2.1 Create Conversational AI Agent
1. Go to [ElevenLabs Conversational AI](https://elevenlabs.io/conversational-ai)
2. Create a new agent
3. Note down your `ELEVENLABS_AGENT_ID`
4. Get your API key from account settings

### 2.2 Configure Agent for Twilio
Follow the official guide: [ElevenLabs Twilio Integration](https://elevenlabs.io/docs/conversational-ai/guides/conversational-ai-twilio)

Key settings:
- Enable Twilio integration
- Set audio format to `mulaw` (8kHz)
- Configure webhook URLs

### 2.3 Enable Authentication (For Custom Parameters)
Follow: [Conversation Configuration](https://elevenlabs.io/docs/conversational-ai/customization/conversation-configuration)

**Important**: Turn on "Enable Authentication" in your agent settings to use custom parameters.

## Step 3: Twilio Configuration

### 3.1 Get Twilio Credentials
1. Sign up at [Twilio Console](https://console.twilio.com/)
2. Get your Account SID and Auth Token
3. Purchase a phone number
4. Note down the phone number in E.164 format (e.g., +1234567890)

### 3.2 Configure Webhooks
Set up webhooks in Twilio Console:
- **Inbound calls**: `https://yourdomain.com/incoming-call-eleven`
- **Outbound calls**: Handled programmatically via API

## Step 4: Server Deployment

### 4.1 Local Development
Start the server locally:
```bash
npm start
```
Server runs on port 8000 by default.

### 4.2 Public Access (Required for Twilio)
Use ngrok for local testing:
```bash
# Install ngrok if not already installed
npm install -g ngrok

# Expose local server
ngrok http 8000
```

Use the ngrok HTTPS URL for Twilio webhook configuration.

### 4.3 Production Deployment
Deploy to platforms like:
- Heroku
- Railway
- Vercel
- AWS/GCP/Azure

## Step 5: Testing

### 5.1 Test Inbound Calls
1. Call your Twilio phone number
2. Verify connection to ElevenLabs agent
3. Check server logs for any errors

### 5.2 Test Outbound Calls
Make a POST request to initiate outbound calls:
```bash
curl -X POST https://yourdomain.com/outbound-call \
  -H "Content-Type: application/json" \
  -d '{
    "number": "+1234567890",
    "prompt": "You are a helpful assistant calling to check in."
  }'
```

## Step 6: Available Scripts and Features

### 6.1 Main Scripts
- `index.js` - Main server entry point
- `inbound-calls.js` - Handles incoming calls with authentication
- `outbound-calls.js` - Handles outbound calls with custom parameters

### 6.2 Additional Scripts (in forTheLegends folder)
**Inbound Call Variations**:
- `inbound-normal.js` - Basic inbound calls
- `inbound-authenticated.js` - Secure inbound calls
- `inbound-custom-prompt.js` - Inbound with custom parameters

**Outbound Call Variations**:
- `outbound-normal.js` - Basic outbound calls
- `outbound-authenticated.js` - Secure outbound calls
- `outbound-custom-make.js` - Integration with Make.com
- `outbound-custom-prompt.js` - Outbound with custom parameters

### 6.3 API Endpoints
- `GET /` - Health check
- `POST /incoming-call-eleven` - Twilio inbound webhook
- `POST /outbound-call` - Initiate outbound calls
- `GET /outbound-call-twiml` - TwiML for outbound calls
- `WS /media-stream` - WebSocket for inbound media
- `WS /outbound-media-stream` - WebSocket for outbound media

## Step 7: Advanced Features

### 7.1 Custom Parameters
Pass custom data to personalize conversations:
```json
{
  "number": "+1234567890",
  "prompt": "You are calling John Smith about his recent order #12345"
}
```

### 7.2 Make.com Integration
Use the `outbound-custom-make.js` script to integrate with Make.com for:
- Automated calls from Google Sheets
- CRM-triggered calls
- Scheduled call campaigns

### 7.3 Authentication
For secure operations, ensure:
- ElevenLabs agent has authentication enabled
- Use signed URLs for WebSocket connections
- Validate incoming requests

## Step 8: Troubleshooting

### 8.1 Common Issues
- **WebSocket connection fails**: Check if ngrok/domain is accessible
- **Audio quality issues**: Verify Twilio audio format settings
- **Authentication errors**: Ensure ElevenLabs agent has auth enabled
- **Environment variables**: Double-check all required vars are set

### 8.2 Debugging
Enable detailed logging by checking server console output:
```bash
npm start
```

### 8.3 Resources
- [ElevenLabs Documentation](https://elevenlabs.io/docs/conversational-ai)
- [Twilio Media Streams](https://www.twilio.com/docs/voice/media-streams)
- [Tutorial Video](https://youtu.be/_BxzbGh9uvk)

## Step 9: Security Considerations

### 9.1 Environment Variables
- Never commit `.env` file to version control
- Use secure environment variable management in production
- Rotate API keys regularly

### 9.2 Webhook Security
- Validate Twilio webhook signatures
- Use HTTPS for all webhook URLs
- Implement rate limiting

### 9.3 Authentication
- Enable ElevenLabs authentication for custom parameters
- Validate all incoming requests
- Use secure WebSocket connections

## Completion Checklist

- [ ] Dependencies installed (`npm install`)
- [ ] Environment variables configured (`.env` file)
- [ ] ElevenLabs agent created and configured
- [ ] Twilio account set up with phone number
- [ ] Webhooks configured in Twilio Console
- [ ] Server deployed and accessible via HTTPS
- [ ] Inbound calls tested successfully
- [ ] Outbound calls tested successfully
- [ ] Custom parameters working (if needed)
- [ ] Production deployment completed

## Next Steps

1. **Add your API keys** to the `.env` file
2. **Deploy the server** to a public domain
3. **Configure Twilio webhooks** with your domain
4. **Test both inbound and outbound calls**
5. **Customize agent prompts** as needed
6. **Set up monitoring and logging** for production

---

**Note**: This setup enables both basic and advanced call handling scenarios. Choose the appropriate scripts from the `forTheLegends` folder based on your specific requirements.
