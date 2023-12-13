pnpm install ai
llllll

# Recaptcha
RECAPTCHA_SITE_KEY=<recaptcha site key>
RECAPTCHA_SECRET_KEY=<recaptcha secret key>

# Microsoft
MS_AZURE_COGNITIVE_SERVICES_API_KEY=<MS Azure Cognitive Services API Key>
MS_AZURE_COG_SERVICES_ENTITY_LINKING_API_KEY=<MS Azure Entity Linking API Key>
MS_AZURE_COMPUTER_VISION_KEY=<key>

# IBM Watson/Alchemy/etc (note that different IBM services have different auth requirements)
IBM_WATSON_TONE_USERNAME=<IBM Tone API Username>
IBM_WATSON_TONE_PASSWORD=<IBM Tone API Password>
IBM_ALCHEMY_API_KEY=<IBM Alchemy API Key>

# Google NLP
GCLOUD_PROJECT=<Google Cloud Platform Project ID>
GOOGLE_NLP_API_KEY=<API KEY>
GOOGLE_CLOUD_PRIVATE_KEY="<key>"
GOOGLE_CLOUD_EMAIL="888888888-something@developer.gserviceaccount.com"

# Clarifai
CLARIFAI_CLIENT_ID=<token>
CLARIFAI_CLIENT_SECRET=<secret>

# Recast.ai
RECAST_AI_TOKEN=<token>

# API.ai
API_AI_TOKEN=<token>

# Baidu
BAIDU_TRANSLATION_APP_ID=<token>
BAIDU_TRANSLATION_KEY=<token>

# this is enabled for rate limiting on our production environment
REDIS_URL=redis://<Redis location>
RATE_LIMITING_ENABLED=true
RATE_LIMITING_INTERVAL=<interval>
RATE_LIMITING_REQUESTS=<limit>


// ./app/api/chat/route.js
import OpenAI from 'openai';
import { OpenAIStream, StreamingTextResponse } from 'ai';

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

export const runtime = 'edge';

export async function POST(req) {
  const { messages } = await req.json();
  const response = await openai.chat.completions.create({
    model: 'gpt-4',
    stream: true,
    messages,
  });
  const stream = OpenAIStream(response);
  return new StreamingTextResponse(stream);
}
lllllll
// ./app/page.js
'use client';

import { useChat } from 'ai/react';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit } = useChat();

  return (
    <div>
      {messages.map(m => (
        <div key={m.id}>
          {m.role}: {m.content}
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input
          value={input}
          placeholder="Say something..."
          onChange={handleInputChange}
        />
      </form>
    </div>
  );
}
