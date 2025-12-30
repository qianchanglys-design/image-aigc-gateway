#Image AIGC Gateway
A production‑ready Image Generation Gateway compatible with the OpenAI Images API, designed for multi‑provider routing, protocol‑level compatibility, and long‑term maintainability.

✨ Features
✅ OpenAI Images API compatible

✅ Supports size, n, response_format

✅ Supports url and b64_json response formats

✅ Provider‑based architecture (OpenAI / Midjourney / Mock)

✅ Model → Provider routing

✅ Unified OpenAI‑style error handling

✅ Structured JSON logging

✅ Stateless & concurrency‑safe

## 📦 Project Structure

```text
src/
├── api/
│   └── images.js          # OpenAI-compatible HTTP API
├── engine/
│   └── imageEngine.js     # Provider routing & dispatch
├── providers/
│   ├── base.js            # Provider interface
│   ├── mock.js            # Mock provider (default)
│   └── openai.js          # OpenAI Images API provider
├── errors/
│   └── openaiError.js     # Unified OpenAI-style error
├── utils/
│   └── logger.js          # Structured JSON logger
└── server.js              # Express bootstrap
```

🚀 Getting Started
1️⃣ Install dependencies
```bash
npm install
```
2️⃣ Start the server
```bash
npm start
```
Server will run on:
```代码
http://localhost:3000
```
🖼 Image Generation API
Endpoint
```代码
POST /v1/images/generations
```
Request Body
```json
{
  "model": "gpt-image-1",
  "prompt": "a futuristic city",
  "size": "1024x1024",
  "n": 1,
  "response_format": "url"
}
```
Parameters
```Text
Field	Type	Required	Description
model	string	❌	Model name (used for provider routing)
prompt	string	✅	Image generation prompt
size	string	❌	Image size (default: 1024x1024)
n	number	❌	Number of images (default: 1)
response_format	string	❌	url or b64_json (default: url)
```
📤 Response Format
response_format: "url"
```json
{
  "created": 1767091282,
  "data": [
    { "url": "https://via.placeholder.com/1024" }
  ]
}
```
response_format: "b64_json"
```json
{
  "created": 1767091282,
  "data": [
    { "b64_json": "bW9jayBpbWFnZSBjb250ZW50" }
  ]
}
```
🔀 Model → Provider Routing
Routing is handled in:
```代码
src/engine/imageEngine.js
```
Example:
```js
const providerMap = {
  'gpt-image-1': openaiProvider,
  'mj-v6': mockProvider,
  'default': mockProvider
};
```
>Unmatched models automatically fall back to default
>Providers are fully interchangeable
>API layer remains unchanged

🔌 Enabling OpenAI Provider
1️⃣ Set environment variable
```Bash
export OPENAI_API_KEY=your_api_key_here
```
2️⃣ Enable provider mapping
```Js
const openaiProvider = require('../providers/openai');

'gpt-image-1': openaiProvider,
```
No other code changes are required.

❌ Error Handling
All errors follow OpenAI‑style error format:
```json
{
  "error": {
    "message": "prompt is required",
    "type": "invalid_request_error",
    "param": "prompt",
    "code": null
  }
}
```
Supported error types:
```Text
invalid_request_error
authentication_error
api_error
internal_error
```

Errors are:

Thrown by providers with clear semantics

Unified and formatted at API layer

Safe for production exposure

📜 Logging
Structured JSON logs are emitted for:

Request Entry
json
{
  "level": "info",
  "message": "Image generation request",
  "model": "test-model",
  "prompt_length": 17,
  "n": 2,
  "size": "1024x1024",
  "response_format": "url"
}
Provider Dispatch
json
{
  "level": "info",
  "message": "Dispatching image generation",
  "model": "test-model",
  "provider": "MockImageProvider"
}
Errors
json
{
  "level": "error",
  "message": "Image generation failed",
  "type": "authentication_error"
}
Logger implementation is intentionally minimal and can be replaced with winston or pino.

🧩 Provider Interface
All providers implement:

js
generateImage({ prompt, model, size, n, response_format })
Providers:

Do not handle HTTP

Do not format responses

Only throw semantic errors

🛡 Design Principles
Protocol‑first compatibility

Strict separation of concerns

Stateless request handling

Provider‑agnostic architecture

Production‑safe error exposure

📄 License
MIT

🏁 Status
Production‑ready core.  
Ready for:

Real OpenAI integration

Additional providers

Deployment & scaling
