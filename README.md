# Solana Agent Kit Telegram Agent

## Overview
Create a Telegram bot powered by Solana Agent Kit to interact with the Solana blockchain through natural language conversations. This implementation provides a seamless way to execute blockchain operations via Telegram.

---

## Quick Start

### 1. Agent Setup
```sh
# Clone the repository
npm install -g degit
degit sendaifun/solana-agent-kit/examples/tg-bot-starter tg-agent
cd tg-agent

# Install dependencies
pnpm install
```

### 2. Environment Configuration
Create a `.env.local` file:
```sh
OPENAI_API_KEY=your_openai_key
RPC_URL=your_solana_rpc_url
SOLANA_PRIVATE_KEY=your_wallet_private_key_base58
TELEGRAM_BOT_TOKEN=your_telegram_bot_token
```

### 3. Create Telegram Bot
1. Message `@BotFather` on Telegram
2. Use `/newbot` command
3. Follow instructions to create the bot
4. Save the bot token

---

## Project Structure
```
├── app/
│   ├── api/
│   │   └── bot/
│   │       └── route.ts    # Telegram webhook handler
│   ├── lib/
│   │   ├── agent.ts       # Solana Agent setup
│   │   └── telegram.ts    # Telegram utilities
│   └── config.ts          # Configuration
├── public/
└── package.json
```

---

## Implementation Steps

### 1. Set Up Webhook
```sh
# Using curl
curl https://api.telegram.org/bot<TELEGRAM_BOT_TOKEN>/setWebhook?url=https://<your-domain>/api/bot

# Or visit in browser
https://api.telegram.org/bot<TELEGRAM_BOT_TOKEN>/setWebhook?url=https://<your-domain>/api/bot
```

### 2. Local Development
```sh
# Start development server
pnpm dev

# Start ngrok tunnel
ngrok http 3000
```

---

## Core Components

### Webhook Handler
```ts
// app/api/bot/route.ts
export async function POST(req: Request) {
  const body = await req.json();
  
  // Initialize agent
  const agent = new SolanaAgentKit(
    process.env.SOLANA_PRIVATE_KEY!,
    process.env.RPC_URL!,
    process.env.OPENAI_API_KEY
  );

  // Process message
  const response = await processMessage(body, agent);
  
  return new Response(JSON.stringify(response));
}
```

### Message Processing
```ts
async function processMessage(message: any, agent: SolanaAgentKit) {
  const { text, chat } = message;
  
  // Generate response using agent
  const response = await agent.chat(text);
  
  // Send response to Telegram
  await sendTelegramMessage(chat.id, response);
}
```

---

## Features
### Chat Interaction
- Natural language processing
- Command handling
- Error responses
- Message formatting

### Blockchain Operations
- Transaction execution
- Balance checking
- Token transfers
- Price queries

### Bot Management
- User session handling
- Rate limiting
- Error handling
- Logging

---

## Deployment
### Deploy to Vercel
1. Push code to GitHub
2. Import project in Vercel
3. Configure environment variables
4. Deploy

### Set Up Webhook After Deployment
1. Get your deployment URL
2. Set webhook using the URL
3. Verify webhook status
4. Test bot functionality

---

## Security Considerations
### Token Security
- Secure storage of bot token
- Environment variable protection
- Access control
- Rate limiting

### Request Validation
- Validate Telegram updates
- Check message format
- Verify user permissions
- Monitor activity

### Error Handling
- Graceful error messages
- Transaction failures
- Network issues
- API limits

---

## Example Bot Commands
```sh
/balance - Check wallet balance
/price SOL - Get SOL price
/transfer 1 SOL address - Transfer SOL
/help - Show available commands
```

---

## Development Tips
### Local Testing
- Use ngrok for local development
- Test all commands thoroughly
- Monitor error logs
- Check response times

### Message Handling
- Parse commands correctly
- Format responses properly
- Handle long messages
- Implement retry logic

### User Experience
- Clear error messages
- Progress indicators
- Command suggestions
- Help documentation

---

## Common Issues
### Webhook Setup
- Invalid URL format
- SSL certificate issues
- Port configuration
- Domain verification

### Bot Responses
- Slow response times
- Message formatting
- Command parsing
- Error handling

### Deployment
- Environment variables
- Webhook configuration
- API access
- Rate limits

---

## Testing
### Local Testing
```sh
# Start local server
pnpm dev

# Start ngrok
ngrok http 3000

# Set webhook to ngrok URL
curl https://api.telegram.org/bot<token>/setWebhook?url=<ngrok-url>/api/bot
```

### Production Testing
- Send test messages
- Check transaction execution
- Verify responses
- Monitor errors

---

## Resources
- [Telegram Bot API](https://core.telegram.org/bots/api)
- [Solana Agent Kit Docs](https://solana-agent-kit-docs)
- [Vercel Deployment](https://vercel.com/docs)
- [ngrok Documentation](https://ngrok.com/docs)

---

## Support
For support and questions:
- Create a GitHub issue
- Join the Telegram support group
- Check the documentation
- Contact maintainers

