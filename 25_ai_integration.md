# Phase 13.5 — AI Integration with Node.js

# Step 1 — LLM SDK in Node.js

Now we begin the next phase from your original roadmap:

```text
[ ] LLM SDK in Node (OpenAI/Anthropic/Groq)
[ ] Streaming responses (SSE) to React frontend
[ ] RAG basics: embeddings, vector DB
[ ] LangChain.js or raw SDK calls
[ ] AI agents in Node
[ ] Rate limiting + cost control for LLM calls
[ ] Prompt caching, error handling for LLM APIs
```

---

# 1. What Is an LLM?

LLM means:

```text
Large Language Model
```

Examples include models provided by:

* [OpenAI](https://openai.com?utm_source=chatgpt.com)
* [Anthropic](https://www.anthropic.com?utm_source=chatgpt.com)
* [Groq](https://groq.com?utm_source=chatgpt.com)

An LLM can process text and generate responses.

For example:

```text
User:
"Summarize this document"

        ↓

LLM

        ↓

AI-generated summary
```

---

# 2. What Is an SDK?

SDK means:

```text
Software Development Kit
```

Instead of manually making complicated HTTP requests, providers offer libraries for interacting with their APIs.

Conceptually:

```text
Node.js Application
       ↓
Provider SDK
       ↓
LLM API
       ↓
AI Model
       ↓
Response
```

---

# 3. Why Use an LLM SDK?

Without an SDK, conceptually:

```text
Node.js
   ↓
Manually construct HTTP request
   ↓
Authentication headers
   ↓
Request body
   ↓
API endpoint
   ↓
Parse response
```

With an SDK:

```text
Node.js
   ↓
SDK
   ↓
Model
   ↓
Response
```

The SDK simplifies communication with the AI provider.

---

# 4. Where Does the API Key Go?

An API key authenticates your backend with the AI provider.

It should be stored in:

```text
.env
```

Example:

```text
OPENAI_API_KEY=your_secret_key
```

or:

```text
GROQ_API_KEY=your_secret_key
```

Important:

> Never expose API keys directly in the React frontend.

Correct architecture:

```text
React Frontend
      ↓
Express Backend
      ↓
AI SDK
      ↓
LLM Provider
```

Not:

```text
React
  ↓
Secret API Key ❌
  ↓
LLM Provider
```

---

# 5. Basic AI Request Flow

Imagine the user sends:

```text
"What should I do today?"
```

The flow:

```text
React
  ↓
POST /chat
  ↓
Express
  ↓
AI SDK
  ↓
LLM
  ↓
Generate response
  ↓
Express
  ↓
React
```

---

# 6. Provider APIs Are Similar Conceptually

Whether using OpenAI, Anthropic, or Groq, the general architecture is similar:

```text
Your Node.js Backend
        ↓
SDK
        ↓
API Key
        ↓
AI Provider
        ↓
LLM Model
        ↓
Generated Response
```

The exact SDK syntax and API structure differ between providers.

---

# 7. What Does the Backend Send to the LLM?

Usually some combination of:

```text
System instructions
User message
Conversation history
```

Conceptually:

```text
System:
"You are a helpful assistant."

User:
"Explain JavaScript closures."
```

The model processes the input and returns generated text.

---

# 8. Chat Messages

Modern LLM applications usually work with messages.

For example:

```text
System
  ↓
Defines behavior

User
  ↓
Provides question

Assistant
  ↓
Previous AI response
```

Conversation:

```text
User: What is Node.js?

Assistant: Node.js is...

User: Give me an example.
```

The backend may send conversation context so the model understands what "me" refers to.

---

# 9. Why the Backend Is Important

Your Express backend acts as the secure middle layer.

```text
Frontend
   ↓
Express Backend
   ├── Authentication
   ├── Rate limiting
   ├── Input validation
   ├── API key protection
   └── AI request handling
          ↓
        LLM Provider
```

This is why AI integration belongs naturally after learning backend development.

---

# 10. Basic AI Endpoint Architecture

Our backend could have:

```text
POST /api/chat
```

Conceptually:

```text
Client sends message
       ↓
Validate message
       ↓
Check authenticated user
       ↓
Apply rate limit
       ↓
Call LLM API
       ↓
Receive AI response
       ↓
Return response
```

This combines many things you've already learned.

---

# 11. Different AI Providers

You may use different providers depending on:

```text
Model quality
Speed
Cost
Available models
Features
Rate limits
```

For example:

| Provider  | General Role                     |
| --------- | -------------------------------- |
| OpenAI    | AI models and APIs               |
| Anthropic | Claude models                    |
| Groq      | Fast AI inference infrastructure |

The important skill is not memorizing one provider's SDK.

It's understanding the architecture:

```text
Application
    ↓
SDK/API
    ↓
LLM Provider
    ↓
Model
```

---

# 12. Error Handling with AI APIs

AI requests can fail.

For example:

```text
Invalid API key
Rate limit exceeded
Network failure
Provider unavailable
Invalid model
Request timeout
```

So:

```text
Client
  ↓
Express
  ↓
try
  ↓
LLM API
  ↓
Success → Response

Error → catch → Error handler
```

This connects directly with the backend error handling you learned earlier.

---

# 13. AI Request Is Asynchronous

Calling an AI model takes time.

So Node.js uses:

```text
async
await
```

Conceptually:

```text
Request
   ↓
Express async controller
   ↓
await LLM response
   ↓
Return AI result
```

The architecture is similar to calling:

```text
Database
External API
Cloud service
```

An LLM API is essentially another external service your backend communicates with.

---

# 14. Complete Mental Model

```text
USER
  ↓
REACT FRONTEND
  ↓
POST /api/chat
  ↓
EXPRESS BACKEND
  ├── Authentication
  ├── Validation
  
  ├── Rate Limiting
  └── Error Handling
          ↓
       LLM SDK
          ↓
       AI PROVIDER
          ↓
       AI MODEL
          ↓
       GENERATED RESPONSE
          ↓
EXPRESS
  ↓
REACT
  ↓
USER
```

---

# Step 1 Complete

The most important things to understand are:

```text
LLM
= Large Language Model

SDK
= Library used to communicate with an AI provider

API Key
= Secret credential stored on the backend

Express
= Secure middle layer between frontend and AI provider
```

----------------------------------------------------------------------------------------------------------------------------------------

# Step 2 — Streaming Responses Server-Sent Events (SSE) to React


## 1. Normal AI Response

Without streaming, the flow looks like:

```text
React
  ↓
Express
  ↓
LLM
  ↓
Wait...
  ↓
Complete response
  ↓
Express
  ↓
React
```

The user may see nothing while the model is generating.

For a long response, this can feel slow.

---

# 2. What Is Streaming?

Streaming means sending the response **piece by piece as it is generated**.

Instead of:

```text
Wait → complete response → display
```

we get:

```text
Generate → send
Generate → send
Generate → send
Generate → send
```

The user can see the answer appearing progressively.

This is the familiar ChatGPT-style experience.

---

# 3. Example

Suppose the model generates:

```text
Node.js is a JavaScript runtime...
```

Without streaming:

```text
[wait................]
Node.js is a JavaScript runtime...
```

With streaming:

```text
Node
Node.js is
Node.js is a
Node.js is a JavaScript
Node.js is a JavaScript runtime...
```

The frontend keeps receiving new pieces and displays them.

---

# 4. What Is SSE?

SSE means:

**Server-Sent Events**

It allows a server to continuously send events to a browser over an HTTP connection.

Basic flow:

```text
React
  ↓
HTTP Request
  ↓
Express
  ↓
LLM
  ↓
Chunk 1 ──→ React
Chunk 2 ──→ React
Chunk 3 ──→ React
Chunk 4 ──→ React
  ↓
Done
```

The connection stays open while the server sends data.

---

# 5. Why SSE Is Useful for AI

AI responses are naturally generated over time.

So SSE fits this pattern well:

```text
LLM
 ↓
Token/chunk
 ↓
Backend
 ↓
SSE
 ↓
Browser
 ↓
Display immediately
```

The user doesn't have to wait for the entire response.

---

# 6. Normal API vs Streaming API

### Normal

```text
POST /api/chat
       ↓
Complete response
       ↓
JSON
```

Example:

```json
{
  "message": "Here is the complete AI response..."
}
```

### Streaming

```text
POST /api/chat
       ↓
Stream starts
       ↓
Chunk
Chunk
Chunk
Chunk
       ↓
Stream ends
```

---

# 7. Express's Role

Your Express backend becomes the bridge between the frontend and LLM provider.

```text
React
  ↓
Express
  ↓
LLM API
  ↓
LLM generates response
  ↓
Express receives chunks
  ↓
SSE
  ↓
React
```

This is important because your backend can still handle:

```text
Authentication
Validation
Rate limiting
API keys
Logging
Error handling
```

---

# 8. Why Not Call the LLM Directly From React?

Because your API key is secret.

Bad architecture:

```text
React
  ↓
LLM API
  ↓
API KEY exposed
```

Better:

```text
React
  ↓
Your Express API
  ↓
LLM API
```

The provider API key stays on the server.

---

# 9. SSE vs WebSockets

You will encounter both.

### SSE

```text
Server → Client
```

The server continuously sends updates to the browser.

Excellent for:

```text
AI text streaming
Notifications
Progress updates
Live status
```

### WebSocket

```text
Client ↔ Server
```

Both sides can continuously communicate.

Excellent for:

```text
Chat applications
Multiplayer games
Real-time collaboration
Live communication
```

For AI response streaming, **SSE is often simpler because the main requirement is server → browser streaming**.

---

# 10. The `EventSource` API

Browsers provide an API called:

```text
EventSource
```

It is designed for SSE connections.

Conceptually:

```text
React
  ↓
EventSource
  ↓
GET /api/chat/stream
  ↓
Express
  ↓
SSE events
```

The browser receives events as the server sends them.

However, traditional `EventSource` is primarily designed around GET requests, while AI chat APIs often need POST bodies. In those cases, frontend applications may use the `fetch()` API with a streamed response instead of `EventSource`.

The important concept is **HTTP response streaming**, not memorizing one particular browser API.

---

# 11. What Is a Chunk?

A chunk is a small piece of the generated response.

For example:

```text
Chunk 1 → "Node"
Chunk 2 → ".js"
Chunk 3 → " is"
Chunk 4 → " a"
Chunk 5 → " runtime"
```

The frontend combines them:

```text
Node.js is a runtime
```

The exact chunk boundaries depend on the AI provider and streaming protocol.

---

# 12. Complete AI Streaming Flow

```text
                 User
                   ↓
              React UI
                   ↓
             POST /chat
                   ↓
             Express API
                   ↓
        Authentication
                   ↓
          Input Validation
                   ↓
            LLM Provider
                   ↓
             AI generates
                   ↓
        ┌──────────┼──────────┐
        ↓          ↓          ↓
      Chunk 1    Chunk 2    Chunk 3
        ↓          ↓          ↓
        └──────── SSE/Stream ─┘
                   ↓
                React
                   ↓
             Update UI
```

---

# 13. Streaming Errors

Streaming introduces another consideration.

The request can fail:

```text
Before stream starts
```

or:

```text
While stream is running
```

For example:

```text
React
 ↓
Express
 ↓
LLM
 ↓
Chunk 1
 ↓
Chunk 2
 ↓
Provider error
```

The backend needs appropriate error handling so the frontend knows the stream ended unexpectedly.

This connects directly to the **error handling and logging** concepts from Phase 13.

---

# 14. Streaming + Rate Limiting

AI streaming can also be expensive.

So our architecture can combine:

```text
Request
  ↓
Rate Limit
  ↓
Authentication
  ↓
Validation
  ↓
LLM
  ↓
Streaming
```

This prevents unrestricted users from repeatedly starting expensive AI requests.

We'll cover **AI-specific rate limiting and cost control** later in this phase.

---

# 15. Why Streaming Improves UX

Consider a response that takes 10 seconds to generate.

### Without streaming

```text
0 sec  → waiting
5 sec  → waiting
10 sec → entire response appears
```

### With streaming

```text
0 sec → first text
1 sec → more text
2 sec → more text
3 sec → more text
...
10 sec → complete
```

The total generation time might not change dramatically, but the application **feels much faster and more responsive**.

---

# Step 2 Complete

Remember the core idea:

```text
Normal API:

Request
  ↓
Wait
  ↓
Complete response


Streaming:

Request
  ↓
Chunk
  ↓
Chunk
  ↓
Chunk
  ↓
Complete
```

And:

> **SSE allows the server to continuously send generated data to the browser, making it well suited for streaming AI responses.**

----------------------------------------------------------------------------------------------------------------------------------------

# Step 3 — RAG Basics: Embeddings and Vector Databases


# 1. What Problem Does RAG Solve?

An LLM does not automatically know your private or custom data.

For example:

```text
Company documents
Product information
Internal PDFs
User's todos
Knowledge base
Database records
```

Suppose you ask an AI:

```text
"What is our company's leave policy?"
```

The LLM may not know your company's internal policy.

You need to provide the relevant information.

This is where RAG comes in.

---

# 2. What Is RAG?

RAG means:

```text
Retrieval-Augmented Generation
```

Break it down:

### Retrieval

Find relevant information.

### Augmented

Add that information to the AI's context.

### Generation

The LLM generates an answer using that information.

Flow:

```text
User Question
     ↓
Retrieve Relevant Data
     ↓
Add Data to Prompt
     ↓
LLM
     ↓
Generate Answer
```

---

# 3. Simple Example

Imagine your application contains these documents:

```text
Document A → Leave policy
Document B → Salary policy
Document C → Work-from-home policy
```

The user asks:

```text
"How many leave days do employees get?"
```

The system should not send every document to the LLM.

Instead:

```text
Question
   ↓
Search documents
   ↓
Find Leave Policy
   ↓
Send relevant information to LLM
   ↓
Generate answer
```

That is the basic RAG idea.

---

# 4. Why Not Just Put Everything in the Prompt?

Imagine:

```text
10,000 documents
```

Sending everything to the LLM would cause problems:

```text
Expensive
Slow
Context limits
Too much irrelevant information
```

Instead:

```text
All Documents
      ↓
Find only relevant pieces
      ↓
Send relevant pieces to LLM
```

This makes the system more efficient.

---

# 5. What Are Embeddings?

This is the core concept behind RAG.

An embedding converts text into numbers that represent its meaning.

For example:

```text
"I love dogs"
```

becomes conceptually:

```text
[0.21, -0.54, 0.88, 0.13, ...]
```

Another sentence:

```text
"Dogs are wonderful pets"
```

also becomes numbers:

```text
[0.19, -0.49, 0.91, 0.10, ...]
```

Because these sentences have similar meanings, their embeddings are mathematically close.

---

# 6. Why Convert Text Into Numbers?

Computers can compare numerical vectors efficiently.

For example:

```text
Text A
  ↓
Embedding

Text B
  ↓
Embedding

Compare similarity
```

The system can find:

```text
Which text has the closest meaning?
```

Not just:

```text
Which text contains exactly the same words?
```

---

# 7. Keyword Search vs Semantic Search

Traditional keyword search:

```text
Search:
"dog"

Find:
Documents containing "dog"
```

Embedding-based semantic search:

```text
Search:
"good pets"

Can potentially find:
"Dogs are wonderful companions"
```

Even though the exact words are different.

This is called:

```text
Semantic similarity
```

The search focuses on meaning.

---

# 8. What Is a Vector?

An embedding is essentially a vector.

For example:

```text
[0.21, -0.54, 0.88]
```

Real embeddings usually contain many more numbers.

Conceptually:

```text
Text
 ↓
Embedding Model
 ↓
Vector
```

---

# 9. What Is a Vector Database?

A vector database stores and searches embeddings.

Examples include:

* [Pinecone](https://www.pinecone.io?utm_source=chatgpt.com)
* [Qdrant](https://qdrant.tech?utm_source=chatgpt.com)
* [pgvector](https://github.com/pgvector/pgvector?utm_source=chatgpt.com)

Conceptually:

```text
Documents
    ↓
Create Embeddings
    ↓
Vector Database
```

Each document or text chunk is stored with its embedding.

---

# 10. Why Do We Need a Vector Database?

Imagine:

```text
1 million documents
```

When the user asks a question, you need to find the most relevant documents quickly.

The vector database performs similarity search:

```text
User Question
      ↓
Create Question Embedding
      ↓
Search Vector Database
      ↓
Find Similar Vectors
      ↓
Return Relevant Documents
```

---

# 11. Complete RAG Flow

Let's look at the complete architecture.

## Step A — Prepare the Knowledge

```text
Documents
    ↓
Split into smaller chunks
    ↓
Create embeddings
    ↓
Store in Vector Database
```

This happens when preparing your data.

---

## Step B — User Asks a Question

```text
User:
"What is the leave policy?"
```

The application:

```text
Question
   ↓
Create embedding
   ↓
Search Vector Database
   ↓
Find relevant document chunks
```

---

## Step C — Give Context to the LLM

The system combines:

```text
User Question
+
Retrieved Documents
```

Conceptually:

```text
Context:
"Employees receive 20 annual leave days..."

Question:
"How many leave days do employees get?"
```

Then:

```text
LLM
 ↓
Generate answer using provided context
```

---

# 12. Complete RAG Architecture

```text
                    DATA PREPARATION

Documents
   ↓
Split into Chunks
   ↓
Embedding Model
   ↓
Vectors
   ↓
Vector Database
```

Then during a user request:

```text
User Question
      ↓
Embedding Model
      ↓
Question Vector
      ↓
Vector Database Search
      ↓
Relevant Chunks
      ↓
Add to LLM Context
      ↓
LLM
      ↓
Answer
```

---

# 13. What Is Chunking?

Documents are usually split into smaller pieces.

For example:

```text
100-page PDF
```

You don't create one giant embedding for the entire document.

Instead:

```text
Document
   ↓
Chunk 1
Chunk 2
Chunk 3
Chunk 4
...
```

Each chunk gets its own embedding.

Example:

```text
Employee Handbook

Chunk 1 → Company introduction
Chunk 2 → Leave policy
Chunk 3 → Salary policy
Chunk 4 → Remote work policy
```

This allows the vector search to retrieve only relevant chunks.

---

# 14. Why Chunking Is Important

Bad approach:

```text
Entire 100-page document
       ↓
One embedding
```

Problems:

```text
Too broad
Less precise
Relevant information mixed together
```

Better:

```text
Document
   ↓
Small meaningful chunks
   ↓
Individual embeddings
```

Now the system can retrieve:

```text
Exactly the relevant section
```

---

# 15. RAG vs Fine-Tuning

These are different concepts.

### RAG

```text
External knowledge
      ↓
Retrieve relevant data
      ↓
Give to model during request
```

Useful for:

```text
Documents
Company data
Knowledge bases
Frequently changing information
```

### Fine-Tuning

```text
Train/adjust model behavior using training data
```

Useful for changing:

```text
Style
Behavior
Specific task performance
```

Simple distinction:

```text
RAG = Give the model knowledge

Fine-tuning = Change/adapt model behavior
```

RAG is usually preferred when your information changes frequently.

---

# 16. Where RAG Fits in Node.js

Our backend architecture:

```text
React
  ↓
POST /chat
  ↓
Express
  ↓
User Question
  ↓
Create Embedding
  ↓
Vector Database Search
  ↓
Relevant Context
  ↓
LLM
  ↓
Response
  ↓
React
```

Your Express backend coordinates the entire process.

---

# 17. Example: "Chat With Your Todos"

This directly connects to the project idea in your roadmap.

Suppose the user has:

```text
Learn React
Finish backend project
Buy groceries
Prepare presentation
```

User asks:

```text
"What programming tasks do I have?"
```

RAG flow:

```text
Question
   ↓
Search user's todo data
   ↓
Find relevant todos
   ↓
Provide context to LLM
   ↓
Answer:
"You have two programming-related tasks..."
```

---

# 18. Important Terms to Remember

### RAG

```text
Retrieve information
+
Give it to LLM
+
Generate answer
```

### Embedding

```text
Text converted into numerical representation of meaning
```

### Vector

```text
The numerical representation produced by an embedding
```

### Vector Database

```text
Stores vectors and performs similarity search
```

### Chunking

```text
Split large documents into smaller pieces
```

### Semantic Search

```text
Search based on meaning/similarity
```

---

# Final Mental Model

```text
KNOWLEDGE BASE

Documents
   ↓
Chunks
   ↓
Embeddings
   ↓
Vector Database


USER REQUEST

Question
   ↓
Embedding
   ↓
Similarity Search
   ↓
Relevant Chunks
   ↓
LLM Context
   ↓
Generated Answer
```

The single most important idea is:

> **RAG does not make the LLM permanently learn your documents. It retrieves relevant information at request time and gives that information to the LLM as context.**

------------------------------------------------------------------------------------------------------------------------------------------

# Step 4 — LangChain.js vs Raw SDK Calls


# 1. The Main Question

When building an AI application in Node.js, you generally have two approaches:

```text
Option 1 → Use the provider's SDK directly

Option 2 → Use a framework like LangChain.js
```

For example:

```text
Node.js
   ↓
Raw SDK
   ↓
OpenAI / Anthropic / Groq
```

or:

```text
Node.js
   ↓
LangChain.js
   ↓
LLM Provider
```

---

# 2. What Are Raw SDK Calls?

Raw SDK means directly using the official SDK provided by an AI company.

Conceptually:

```text
Your Express App
       ↓
Official AI SDK
       ↓
LLM Provider API
```

For example, if using OpenAI:

```text
Your App
   ↓
OpenAI SDK
   ↓
OpenAI API
```

If using Anthropic:

```text
Your App
   ↓
Anthropic SDK
   ↓
Anthropic API
```

---

# 3. Why Use Raw SDK Calls?

Raw SDK calls give you direct control.

Your application handles:

```text
Prompt construction
API requests
Responses
Streaming
Error handling
Conversation history
Tool calling
```

Architecture:

```text
Express
   ↓
Controller
   ↓
Provider SDK
   ↓
LLM
```

This is usually easier to understand when the application is relatively simple.

---

# 4. Example Mental Model

Suppose a user sends:

```text
"Explain closures in JavaScript"
```

With a raw SDK:

```text
Express Route
      ↓
Get user message
      ↓
Build request
      ↓
Call LLM SDK
      ↓
Receive response
      ↓
Return response
```

Everything is handled directly by your application.

---

# 5. What Is LangChain?

[LangChain.js](https://js.langchain.com?utm_source=chatgpt.com) is a framework designed to help build applications using LLMs.

It provides abstractions and tools for common AI application tasks.

For example:

```text
Prompt management
LLM integration
Chains/workflows
RAG
Vector databases
Document loading
Tool calling
Agents
Memory
```

Conceptually:

```text
Your Application
       ↓
LangChain
       ↓
LLM / Vector DB / Tools
```

---

# 6. Why Does LangChain Exist?

Imagine building a complex AI application manually.

You may need to connect:

```text
LLM
+
Vector Database
+
Embeddings
+
Documents
+
Tools
+
Conversation History
```

Without a framework:

```text
Your code manually connects everything
```

With LangChain:

```text
LangChain provides abstractions and integrations
```

Conceptually:

```text
Documents
    ↓
LangChain
    ↓
Embeddings
    ↓
Vector DB
    ↓
Retriever
    ↓
LLM
    ↓
Response
```

---

# 7. Raw SDK vs LangChain

| Raw SDK                       | LangChain                       |
| ----------------------------- | ------------------------------- |
| Direct provider communication | Framework/abstraction layer     |
| More control                  | More built-in abstractions      |
| Fewer dependencies            | More dependencies               |
| Simple for small applications | Useful for complex AI workflows |
| Provider-specific code        | Can support multiple providers  |
| You build more yourself       | Provides reusable components    |

---

# 8. Simple Application: Raw SDK

Suppose you only need:

```text
User message
    ↓
LLM
    ↓
Response
```

Raw SDK is often enough.

```text
React
  ↓
Express
  ↓
Official SDK
  ↓
LLM
```

Adding LangChain may be unnecessary.

---

# 9. Complex Application: LangChain

Suppose you need:

```text
User Question
      ↓
Load PDF documents
      ↓
Split documents
      ↓
Create embeddings
      ↓
Store/search vector DB
      ↓
Retrieve relevant context
      ↓
LLM
      ↓
Tool calling
      ↓
Final response
```

A framework like LangChain can help organize this workflow.

---

# 10. Important: LangChain Is Not an LLM

This distinction is important.

```text
OpenAI / Claude / Llama / Qwen
            ↓
          LLMs
```

But:

```text
LangChain
    ↓
Framework for working with LLMs
```

So:

```text
LangChain ≠ AI Model
```

Instead:

```text
LangChain
    ↓
Can connect to
    ↓
Different AI Models
```

---

# 11. LangChain in the Architecture

Without LangChain:

```text
Express
   ↓
OpenAI SDK
   ↓
LLM
```

With LangChain:

```text
Express
   ↓
LangChain
   ├── LLM
   ├── Vector Database
   ├── Embeddings
   ├── Documents
   └── Tools
```

LangChain acts as an orchestration layer.

---

# 12. Provider Lock-In

Raw SDK code is usually more tightly connected to one provider.

For example:

```text
Your App
   ↓
OpenAI SDK
```

If you switch providers, some code may need to change.

With an abstraction framework:

```text
Your App
   ↓
LangChain
   ↓
Provider A / Provider B
```

Switching providers can potentially be easier.

However, provider differences still exist, so frameworks do not magically make every provider interchangeable.

---

# 13. The Trade-Off

### Raw SDK

```text
Advantages
✓ Simple
✓ Direct
✓ Full control
✓ Easier debugging
✓ Fewer abstractions
```

```text
Disadvantages
✗ You build more infrastructure yourself
✗ Complex workflows require more custom code
```

### LangChain

```text
Advantages
✓ Built-in AI application components
✓ Useful integrations
✓ Helps organize complex workflows
✓ Supports RAG-related workflows
```

```text
Disadvantages
✗ More abstraction
✗ More concepts to learn
✗ Can make debugging harder
✗ Sometimes unnecessary for simple projects
```

---

# 14. Which One Should You Learn First?

For your learning roadmap:

```text
First → Raw SDK
Then → Understand LangChain
```

Why?

Because you should understand what is happening underneath.

Learn:

```text
Request
↓
SDK
↓
LLM
↓
Response
```

Then understand what LangChain helps organize:

```text
Documents
↓
Embeddings
↓
Vector DB
↓
Retriever
↓
LLM
↓
Tools
↓
Response
```

If you start directly with LangChain, some underlying concepts can feel like magic.

---

# 15. Real-World Perspective

You do not always need LangChain.

A simple AI endpoint:

```text
POST /chat
```

may only require:

```text
Express
+
Official SDK
+
LLM
```

A complex RAG application might use:

```text
Express
+
LangChain
+
Embeddings
+
Vector Database
+
Document Processing
+
LLM
```

Choose the tool based on complexity.

---

# 16. Complete Mental Model

```text
                    AI APPLICATION

                         Node.js
                           │
             ┌─────────────┴─────────────┐
             ↓                           ↓
          Raw SDK                    LangChain
             ↓                           ↓
       Direct control              Abstraction layer
             ↓                           ↓
           LLM                  LLM + RAG + Tools
```

---

# The Most Important Thing to Remember

```text
Raw SDK
=
Direct communication with the AI provider.


LangChain
=
A framework that helps organize and build
more complex LLM applications.
```

Neither is automatically better.

```text
Simple application
      ↓
Raw SDK is often enough

Complex AI workflow
      ↓
LangChain may help
```

-----------------------------------------------------------------------------------------------------------------------------------------


# Step 5 — AI Agents in Node


# 1. What Is an AI Agent?

A normal LLM application works like this:

```text
User
 ↓
Question
 ↓
LLM
 ↓
Answer
```

An AI agent can do more.

```text
User
 ↓
Question
 ↓
LLM decides what to do
 ↓
Uses a Tool
 ↓
Gets Result
 ↓
LLM processes result
 ↓
Final Answer
```

The important difference:

> A normal LLM mainly generates text. An agent can decide to use external tools to complete a task.

---

# 2. Simple Example

User asks:

```text
"What is the weather in Chennai?"
```

A normal LLM without current data might only answer based on its existing knowledge.

An AI agent can:

```text
User Question
      ↓
Agent understands:
"I need current weather data"
      ↓
Use Weather Tool/API
      ↓
Get Current Weather
      ↓
LLM generates final answer
```

---

# 3. What Is a Tool?

A tool is a function or external capability available to the AI.

Examples:

```text
Weather API
Database search
Web search
Calculator
Send email
Check calendar
Create support ticket
Search documents
```

Conceptually:

```text
AI Agent
   ↓
Can choose
   ↓
┌────────┬────────┬────────┐
↓        ↓        ↓        ↓
Search  Database Calculator API
```

---

# 4. Function Calling

Function calling is one of the core concepts behind modern AI agents.

Suppose your application provides this function:

```text
getUserTodos(userId)
```

The AI doesn't directly execute JavaScript itself.

Instead:

```text
LLM
 ↓
"I need the user's todos"
 ↓
Requests function call
 ↓
Your Node.js application executes function
 ↓
Result returned to LLM
 ↓
LLM generates final response
```

This is important.

The model:

```text
Decides WHAT tool to use
```

Your application:

```text
Actually executes the tool
```

---

# 5. Example: Todo AI Agent

User asks:

```text
"What tasks do I have for today?"
```

Flow:

```text
User
 ↓
Express Backend
 ↓
LLM
 ↓
LLM decides:
"I need todo data"
 ↓
Calls getTodos()
 ↓
Node.js executes database query
 ↓
MongoDB
 ↓
Returns todos
 ↓
LLM receives todo data
 ↓
Generates answer
```

Architecture:

```text
                USER
                  ↓
               REACT
                  ↓
              EXPRESS
                  ↓
                 LLM
                  ↓
          "Need todo information"
                  ↓
            TOOL CALL REQUEST
                  ↓
               NODE.JS
                  ↓
              DATABASE
                  ↓
             TOOL RESULT
                  ↓
                 LLM
                  ↓
            FINAL RESPONSE
```

---

# 6. Agent vs Regular Chatbot

### Regular Chatbot

```text
User
 ↓
LLM
 ↓
Response
```

The LLM only uses the information provided in the prompt.

### AI Agent

```text
User
 ↓
LLM
 ↓
Reason about task
 ↓
Choose Tool
 ↓
Execute Tool
 ↓
Observe Result
 ↓
Generate Answer
```

The agent can interact with your application's capabilities.

---

# 7. Multiple Tools

An agent may have access to multiple tools.

For example:

```text
AI Agent
   │
   ├── searchTodos()
   │
   ├── createTodo()
   │
   ├── deleteTodo()
   │
   ├── searchDocuments()
   │
   └── getWeather()
```

The model decides which tool is appropriate.

User:

```text
"Add buy groceries to my todos."
```

Possible flow:

```text
LLM
 ↓
Select createTodo()
 ↓
Node.js executes function
 ↓
Database updated
 ↓
Result returned
 ↓
LLM:
"Your todo was created."
```

---

# 8. Agent Loop

Agents can sometimes perform multiple steps.

Example:

```text
User:
"What tasks should I prioritize today?"
```

Possible agent flow:

```text
Step 1
 ↓
Get user's todos

Step 2
 ↓
Analyze deadlines/priorities

Step 3
 ↓
Possibly get calendar information

Step 4
 ↓
Generate recommendation
```

This is often called an:

```text
Agent Loop
```

Conceptually:

```text
LLM
 ↓
Need information?
 ↓
YES → Call Tool
 ↓
Get Result
 ↓
Need another tool?
 ↓
YES → Call Tool
 ↓
Get Result
 ↓
Enough information?
 ↓
Final Answer
```

---

# 9. Important: Agents Are Not Magic

An agent is still based on:

```text
LLM
+
Tools
+
Instructions
+
Application Logic
```

The LLM does not automatically have unlimited access to everything.

You explicitly define which tools are available.

For example:

```text
Agent Tools:

✓ Search todos
✓ Create todos
✓ Get user profile

Not Available:

✗ Delete database
✗ Access server files
✗ Execute arbitrary commands
```

This is important for security.

---

# 10. Tool Permissions

Never give an AI agent unrestricted access.

Bad architecture:

```text
LLM
 ↓
Full database access ❌
```

Better:

```text
LLM
 ↓
Controlled Tool
 ↓
Your Application
 ↓
Validation
 ↓
Database
```

Example:

```text
LLM requests:
deleteTodo(todoId)
```

Your backend should still verify:

```text
Is user authenticated?
Does this todo belong to this user?
Is this action allowed?
```

The AI should not bypass normal backend security.

---

# 11. AI Agent Architecture in Node.js

A typical architecture:

```text
React Frontend
      ↓
Express API
      ↓
Authentication
      ↓
Validation
      ↓
AI Agent / LLM
      ↓
Decides Tool
      ↓
Tool Execution Layer
      ↓
Database / APIs / Services
      ↓
Tool Result
      ↓
LLM
      ↓
Final Response
```

---

# 12. RAG vs AI Agents

These are related but different.

### RAG

Purpose:

```text
Find relevant information
```

Flow:

```text
Question
 ↓
Vector Search
 ↓
Relevant Documents
 ↓
LLM
 ↓
Answer
```

### AI Agent

Purpose:

```text
Perform actions or use tools
```

Flow:

```text
Question
 ↓
LLM
 ↓
Choose Tool
 ↓
Execute Tool
 ↓
Result
 ↓
Answer
```

Simple distinction:

```text
RAG
=
Retrieve knowledge


Agent
=
Use tools to perform tasks
```

They can also work together.

---

# 13. RAG + Agent Together

Example:

```text
User:
"Based on our company policy, can I request leave next week?"
```

The agent could:

```text
Step 1
 ↓
Search company documents using RAG

Step 2
 ↓
Retrieve leave policy

Step 3
 ↓
Check user's available leave balance

Step 4
 ↓
Generate answer
```

Architecture:

```text
                 AI AGENT
                    ↓
        ┌───────────┴───────────┐
        ↓                       ↓
      RAG Tool              Database Tool
        ↓                       ↓
 Company Documents        User Leave Data
        └───────────┬───────────┘
                    ↓
                  LLM
                    ↓
                Response
```

---

# 14. Real-World Examples of AI Agents

AI agents can be used for:

```text
Customer Support
    ↓
Search knowledge base
Check order status
Create support ticket


Developer Assistant
    ↓
Search documentation
Call APIs
Analyze code


Personal Productivity App
    ↓
Read todos
Create tasks
Update tasks
Check calendar


Business Assistant
    ↓
Query database
Generate reports
Analyze information
```

---

# 15. The Most Important Concept: Tool Calling

Remember this flow:

```text
USER REQUEST
     ↓
LLM ANALYZES REQUEST
     ↓
DOES IT NEED A TOOL?
     ↓
YES
     ↓
SELECT TOOL
     ↓
YOUR NODE.JS APP EXECUTES TOOL
     ↓
RESULT RETURNS TO LLM
     ↓
FINAL RESPONSE
```

The LLM does not directly execute your backend functions.

Instead:

```text
LLM
 ↓
Requests tool call

Node.js
 ↓
Executes controlled function

LLM
 ↓
Uses result
```

---

# 16. Agent vs Traditional Backend

Traditional backend:

```text
User
 ↓
Specific API endpoint
 ↓
Specific backend logic
 ↓
Response
```

Example:

```text
POST /todos
```

The developer explicitly defines exactly what happens.

Agent-based application:

```text
User
 ↓
Natural language request
 ↓
LLM interprets request
 ↓
Selects available tool
 ↓
Backend executes controlled action
 ↓
Response
```

Example:

```text
"Add a meeting tomorrow at 3 PM"
```

The agent interprets the request and may select the appropriate calendar tool.

---

# Final Mental Model

```text
                    USER
                      ↓
              Natural Language
                      ↓
                     LLM
                      ↓
              Understand Request
                      ↓
             Need External Action?
                  ↙       ↘
                NO         YES
                ↓           ↓
             Answer      Select Tool
                            ↓
                       Node.js executes
                            ↓
                        Get Result
                            ↓
                           LLM
                            ↓
                      Final Answer
```

---

# Step 5 Complete

The key ideas:

```text
AI Agent
=
LLM + Tools + Decision Making


Tool Calling
=
LLM requests a function/tool


Node.js
=
Actually executes the tool


Security
=
AI tools must have controlled permissions
```

-----------------------------------------------------------------------------------------------------------------------------------------


# Step 6 — Rate Limiting + Cost Control for LLM APIs


# 1. Why Is This Important?

Traditional APIs might be relatively cheap to run.

For example:

```text
GET /todos
```

Usually involves:

```text
Request
↓
Database query
↓
Response
```

But an LLM request can cost money based on usage.

```text
User
↓
AI Request
↓
LLM Provider
↓
Tokens processed
↓
Cost
```

If 1,000 users continuously send requests, costs can increase quickly.

So AI applications need:

```text
Rate Limiting
+
Usage Control
+
Cost Monitoring
```

---

# 2. What Is Rate Limiting?

Rate limiting restricts how many requests a user can make within a specific period.

For example:

```text
10 requests per minute
```

Flow:

```text
User
 ↓
Request 1 ✓
Request 2 ✓
Request 3 ✓
...
Request 10 ✓

Request 11 ✗
Rate limit exceeded
```

---

# 3. Why AI APIs Need Rate Limiting

Without rate limiting:

```text
User
 ↓
AI Request
 ↓
AI Request
 ↓
AI Request
 ↓
AI Request
 ↓
AI Request
 ↓
...
```

Problems:

```text
High API costs
Server overload
Abuse
Spam
Provider rate limits
```

With rate limiting:

```text
User
 ↓
Rate Limiter
 ↓
Allowed?
 ├── No → Reject request
 │
 └── Yes → Call LLM
```

---

# 4. Where Should Rate Limiting Happen?

The rate limiter should run **before** calling the LLM.

Correct:

```text
Request
   ↓
Authentication
   ↓
Rate Limiter
   ↓
Validation
   ↓
LLM API
```

Bad:

```text
Request
   ↓
LLM API
   ↓
Rate Limiter ❌
```

Once the LLM request is already made, the cost may already exist.

---

# 5. Different Types of Limits

You don't always need one global limit.

You can have different limits.

### Per User

```text
User A → 100 requests/day
User B → 100 requests/day
```

### Per IP Address

```text
IP Address → 20 requests/minute
```

### Per API Key

```text
API Key → 1,000 requests/day
```

### Per Endpoint

```text
/api/chat → 10 requests/minute
/api/embedding → 50 requests/minute
```

---

# 6. Authentication + Rate Limiting

For authenticated applications, limiting by user is often better than only limiting by IP.

Example:

```text
User logs in
     ↓
JWT identifies user
     ↓
req.user.id
     ↓
Rate limiter checks usage
```

Architecture:

```text
Request
   ↓
Authentication Middleware
   ↓
User Identified
   ↓
Rate Limiter
   ↓
AI Endpoint
```

This prevents users from simply changing networks to bypass a basic IP-based limit.

---

# 7. What Happens When the Limit Is Reached?

The server commonly returns:

```text
429 Too Many Requests
```

Example:

```text
Request
   ↓
Rate limit exceeded
   ↓
429
```

Response conceptually:

```json
{
  "message": "Too many requests. Please try again later."
}
```

---

# 8. Request Limits Are Not Enough

This is especially important for LLM applications.

Imagine:

### User A

```text
10 requests
Each request = 100 tokens
```

### User B

```text
10 requests
Each request = 100,000 tokens
```

Both made:

```text
10 requests
```

But their costs may be very different.

So we also need to think about:

```text
Token usage
```

---

# 9. What Are Tokens?

LLMs process text as tokens.

Very roughly:

```text
Input tokens
+
Output tokens
=
Usage
```

Example:

```text
User prompt
    ↓
500 input tokens

AI response
    ↓
1,000 output tokens
```

Total usage:

```text
1,500 tokens
```

AI providers commonly calculate pricing based partly on token usage.

---

# 10. Input and Output Tokens

There are usually two major categories:

### Input Tokens

What you send to the model.

```text
System prompt
User message
Conversation history
RAG context
```

### Output Tokens

What the model generates.

```text
AI response
```

Architecture:

```text
INPUT

System Instructions
        +
User Message
        +
Conversation History
        +
RAG Documents
        ↓
      LLM
        ↓
OUTPUT

Generated Response
```

Both sides can affect cost.

---

# 11. Cost Control Strategy #1 — Limit Request Frequency

Example:

```text
10 AI requests per minute
```

This controls abuse frequency.

```text
Rate Limiting
=
How often users can call the AI
```

---

# 12. Cost Control Strategy #2 — Limit Input Size

Suppose a user sends:

```text
A 500-page document
```

directly to the LLM.

That could be expensive.

Instead, limit:

```text
Maximum message length
Maximum file size
Maximum context size
```

Flow:

```text
User Input
   ↓
Too Large?
 ├── Yes → Reject
 │
 └── No → Continue
```

---

# 13. Cost Control Strategy #3 — Limit Output Tokens

You can restrict how much the AI is allowed to generate.

For example:

```text
Maximum output:

500 tokens
```

Instead of:

```text
Unlimited response generation
```

This prevents unnecessarily long responses.

Conceptually:

```text
User Request
    ↓
LLM
    ↓
Maximum Output Limit
    ↓
Response
```

---

# 14. Cost Control Strategy #4 — Limit Conversation History

A common mistake in AI chat applications:

```text
User Message 1
User Message 2
User Message 3
...
User Message 1,000
```

Then every new request sends the entire conversation:

```text
Entire History
     ↓
LLM
```

Problems:

```text
Expensive
Slow
Context window issues
```

Better:

```text
Recent Messages
+
Conversation Summary
```

Architecture:

```text
Old conversation
       ↓
Summarize
       ↓
Short summary

Recent conversation
       ↓
Keep directly

Summary + Recent Messages
       ↓
LLM
```

---

# 15. Cost Control Strategy #5 — RAG Instead of Sending Everything

Suppose you have:

```text
10,000 company documents
```

Bad:

```text
All documents
     ↓
LLM ❌
```

Better:

```text
User Question
     ↓
Vector Search
     ↓
Relevant Documents Only
     ↓
LLM
```

This is one major advantage of RAG.

It improves:

```text
Cost
Speed
Relevance
```

---

# 16. Cost Control Strategy #6 — Choose the Right Model

Not every request needs the most powerful model.

Example conceptually:

```text
Simple task
   ↓
Smaller / cheaper model

Complex reasoning
   ↓
More capable model
```

For example:

```text
Categorize a todo
        ↓
Small model may be enough


Complex document analysis
        ↓
More capable model
```

Architecture:

```text
Request
   ↓
Task Type
   ↓
Choose Appropriate Model
```

This is often called:

```text
Model Routing
```

---

# 17. Usage Tracking

A production AI application should track usage.

For example:

```text
User ID
Request Count
Input Tokens
Output Tokens
Model Used
Estimated Cost
Timestamp
```

Conceptually:

```text
AI Request
   ↓
LLM Response
   ↓
Usage Information
   ↓
Store Metrics
```

Then you can answer:

```text
Which users consume the most?
Which endpoints are expensive?
Which models cost the most?
```

---

# 18. User Quotas

You can create usage quotas.

For example:

```text
Free User

50 AI requests/day
```

```text
Premium User

500 AI requests/day
```

Or token-based:

```text
Free → 100,000 tokens/month
Premium → 1,000,000 tokens/month
```

Architecture:

```text
Request
   ↓
Check User Plan
   ↓
Check Usage
   ↓
Quota Available?
   ├── No → Reject
   │
   └── Yes → Call LLM
```

---

# 19. Complete Production Flow

A more realistic AI endpoint:

```text
User Request
      ↓
Authentication
      ↓
Rate Limiting
      ↓
Input Validation
      ↓
Check User Quota
      ↓
Limit Input Size
      ↓
Call Appropriate Model
      ↓
Limit Output Tokens
      ↓
Receive Response
      ↓
Track Token Usage
      ↓
Return Response
```

---

# 20. Rate Limiting vs Cost Control

These are related but different.

| Rate Limiting              | Cost Control                          |
| -------------------------- | ------------------------------------- |
| Controls request frequency | Controls AI resource usage            |
| Example: 10 requests/min   | Example: 100K tokens/month            |
| Prevents spam              | Prevents excessive cost               |
| Based on request count     | Can be based on tokens/models/context |

Simple mental model:

```text
Rate Limiting
=
How often can you ask?


Cost Control
=
How much AI resource can you consume?
```

---

# 21. Real-World AI Architecture

```text
                    USER
                      ↓
                 React App
                      ↓
                Express API
                      ↓
              Authentication
                      ↓
               Rate Limiter
                      ↓
             Input Validation
                      ↓
                Usage Quota
                      ↓
              Context Control
                      ↓
                    LLM
                      ↓
              Token Usage Data
                      ↓
              Usage Monitoring
                      ↓
                 Response
```

---

# Most Important Things to Remember

### Rate Limiting

```text
Limit how frequently users call the API.
```

### 429

```text
Too Many Requests
```

### Token Control

```text
Limit input and output size.
```

### Usage Tracking

```text
Track requests and token consumption.
```

### Model Selection

```text
Use the cheapest suitable model for the task.
```

### RAG

```text
Retrieve only relevant information instead of sending everything.
```

---

# Final Mental Model

```text
AI APPLICATION COST CONTROL

User
 ↓
Authentication
 ↓
Rate Limit
 ↓
Quota Check
 ↓
Input Size Limit
 ↓
LLM
 ↓
Output Token Limit
 ↓
Track Usage
 ↓
Response
```

> **Rate limiting protects how often the AI can be called. Cost control protects how much AI usage each request and user can consume.**

------------------------------------------------------------------------------------------------------------------------------------------

# Step 7 — Prompt Caching + Error Handling for LLM APIs


# Part 1 — Prompt Caching

## 1. What Is Prompt Caching?

AI applications often send the same information repeatedly.

For example:

```text
System Prompt:
"You are a helpful Todo assistant.
You help users organize and manage tasks.
Always provide concise answers."

+
User's conversation history
+
User's new question
```

If the system prompt is large and sent with every request:

```text
Request 1 → Same large prompt
Request 2 → Same large prompt
Request 3 → Same large prompt
Request 4 → Same large prompt
```

This can increase:

```text
Cost
Latency
Processing
```

Prompt caching helps reuse repeated prompt content.

---

# 2. Basic Idea

Without caching:

```text
Request 1
↓
Process System Prompt

Request 2
↓
Process Same System Prompt Again

Request 3
↓
Process Same System Prompt Again
```

With prompt caching:

```text
Request 1
↓
Process Prompt
↓
Cache It

Request 2
↓
Reuse Cached Prompt

Request 3
↓
Reuse Cached Prompt
```

The exact implementation depends on the AI provider.

---

# 3. What Usually Gets Cached?

Typically, repeated content.

For example:

```text
System Instructions
Large Documentation
Long Context
Repeated Examples
Tool Definitions
```

Imagine:

```text
STATIC CONTENT
──────────────

System Instructions
Company Information
Product Documentation

DYNAMIC CONTENT
───────────────

User's Current Question
```

Caching is most useful for:

```text
STATIC CONTENT
```

because it stays the same across multiple requests.

---

# 4. Example Architecture

Without prompt caching:

```text
Request 1
↓
System Prompt (5,000 tokens)
↓
User Message

Request 2
↓
System Prompt (5,000 tokens)
↓
User Message

Request 3
↓
System Prompt (5,000 tokens)
↓
User Message
```

With caching:

```text
Request 1
↓
System Prompt
↓
Provider Cache

Request 2
↓
Cached System Prompt
+
New User Message

Request 3
↓
Cached System Prompt
+
New User Message
```

Potential benefits:

```text
Lower cost
Faster requests
Less repeated processing
```

---

# 5. Important: Prompt Caching Is Provider-Specific

Different providers implement prompt caching differently.

The general concept is the same:

```text
Repeated Input
      ↓
Cache
      ↓
Reuse
```

But:

```text
Cache duration
Minimum prompt size
Pricing
Configuration
```

can differ between providers.

So when implementing it, check the documentation for the specific LLM provider you're using.

---

# 6. Prompt Caching vs Normal Application Caching

These are different concepts.

### Normal Caching

Example:

```text
GET /products
↓
Redis Cache
↓
Return API Data
```

### Prompt Caching

Example:

```text
Large System Prompt
↓
LLM Provider Cache
↓
Reuse Previously Processed Input
```

Comparison:

| Normal Cache              | Prompt Cache                       |
| ------------------------- | ---------------------------------- |
| Caches application data   | Caches/reuses LLM input processing |
| Often Redis/database      | Often managed by LLM provider      |
| Example: product data     | Example: system instructions       |
| Reduces database/API work | Reduces repeated LLM processing    |

---

# Part 2 — Error Handling for LLM APIs

LLM API calls can fail just like any other external API.

For example:

```text
Network failure
Invalid API key
Rate limit exceeded
Provider temporarily unavailable
Timeout
Invalid request
```

Your backend must handle these properly.

---

# 7. Basic LLM Error Flow

Bad:

```text
User
 ↓
AI Request
 ↓
Provider Error
 ↓
Application crashes ❌
```

Better:

```text
User
 ↓
AI Request
 ↓
Provider Error
 ↓
Catch Error
 ↓
Log Error
 ↓
Send Proper Response
```

---

# 8. Basic Error Handling Pattern

Conceptually:

```js
try {
    const response = await callLLM();
} catch (error) {
    // Handle error
}
```

This is the same `try/catch` pattern you already learned in Express.

---

# 9. Different Types of LLM Errors

## Invalid API Key

```text
Your Application
      ↓
Invalid API Key
      ↓
Provider Rejects Request
```

Possible response:

```text
401
Authentication Error
```

---

## Rate Limit Error

Too many requests:

```text
Your Application
      ↓
Too Many Requests
      ↓
Provider
      ↓
429 Error
```

Your application should handle this gracefully.

---

## Provider Server Error

The AI provider may have temporary problems:

```text
Your Application
      ↓
LLM Provider
      ↓
Server Error
```

Examples:

```text
500
502
503
```

---

## Timeout

Sometimes the AI request takes too long.

```text
Your Server
      ↓
Wait...
      ↓
Wait...
      ↓
Timeout
```

You should avoid allowing requests to wait forever.

---

# 10. Don't Expose Internal Provider Errors

Bad:

```text
Provider API error:
Invalid internal configuration...
API request details...
Server information...
```

Don't directly expose internal details to users.

Better:

```text
Client:

"AI service is temporarily unavailable. Please try again later."
```

Meanwhile, your server logs the actual error.

```text
Client
↓
Safe Error Message

Server Logs
↓
Detailed Error Information
```

---

# 11. Error Categories

A useful mental model:

| Error Type           | Example                |
| -------------------- | ---------------------- |
| Client Error         | Invalid request        |
| Authentication Error | Invalid API key        |
| Rate Limit           | Too many requests      |
| Provider Error       | AI service unavailable |
| Timeout              | Request took too long  |
| Network Error        | Connection failed      |

You can handle them differently.

---

# 12. Retry Logic

Some errors are temporary.

Example:

```text
Request
↓
Provider temporarily unavailable
↓
Wait
↓
Retry
```

But you should not retry everything.

### Usually reasonable to retry:

```text
Temporary network failure
502
503
Temporary provider errors
```

### Usually don't retry:

```text
Invalid API key
Invalid request
Authentication failure
```

Because retrying won't fix them.

---

# 13. Exponential Backoff

Instead of retrying immediately:

```text
Retry
Retry
Retry
```

Use increasing delays.

Example:

```text
Attempt 1 → Wait 1 second
Attempt 2 → Wait 2 seconds
Attempt 3 → Wait 4 seconds
```

Conceptually:

```text
Request Failed
     ↓
Wait
     ↓
Retry
     ↓
Failed?
     ↓
Wait Longer
     ↓
Retry
```

This is called:

> Exponential backoff.

It prevents repeatedly hammering a failing service.

---

# 14. Timeout Handling

AI responses can sometimes take longer than normal APIs.

You should define a reasonable timeout.

```text
Request
↓
LLM API
↓
Too long?
├── No → Continue
│
└── Yes → Cancel/Fail Gracefully
```

This prevents requests from hanging indefinitely.

---

# 15. Fallback Models

A more advanced production strategy:

```text
Primary Model
      ↓
Fails?
      ↓
Fallback Model
```

Example architecture:

```text
User Request
      ↓
Primary Provider
      │
      ├── Success → Response
      │
      └── Failure
             ↓
       Secondary Provider
             ↓
          Response
```

This can improve reliability.

However:

```text
Fallback Model
```

may behave differently in:

```text
Quality
Speed
Cost
Response format
```

So fallback systems need careful testing.

---

# 16. Logging LLM Errors

You should log useful information.

For example:

```text
Timestamp
User ID
Model Name
Provider
Error Type
Status Code
Request Duration
```

But be careful with sensitive information.

Avoid unnecessarily logging:

```text
Passwords
API keys
Private user data
Sensitive prompts
```

Good logging helps answer:

```text
Why did the request fail?
Which provider failed?
How often does it happen?
Which model has problems?
```

---

# 17. Complete Production Error Flow

```text
User Request
      ↓
Validate Input
      ↓
Call LLM
      ↓
   Success?
   ├── YES
   │     ↓
   │   Response
   │
   └── NO
         ↓
     Identify Error
         ↓
   ┌─────┼──────────┐
   ↓     ↓          ↓
429   Timeout    Provider Error
   ↓     ↓          ↓
Handle  Retry?    Retry?
   ↓     ↓          ↓
Safe Error Response
         ↓
       Log Error
```

---

# 18. Combining Everything

A production AI endpoint may look conceptually like this:

```text
CLIENT REQUEST
      ↓
Authentication
      ↓
Rate Limiting
      ↓
Input Validation
      ↓
Usage / Cost Check
      ↓
Prompt Construction
      ↓
Prompt Cache
      ↓
LLM API Request
      ↓
    ┌───────────┐
    │ Success?  │
    └─────┬─────┘
      YES │ NO
       ↓  │
   Response│
            ↓
      Error Handling
            ↓
      Retry if appropriate
            ↓
       Log Error
            ↓
      Safe Error Response
```

---

# 19. Prompt Caching + Cost Control

These two concepts connect.

```text
Cost Control
    ↓
Reduce unnecessary AI usage

Prompt Caching
    ↓
Reduce repeated prompt processing
```

Together:

```text
Rate Limiting
+
Token Limits
+
Model Selection
+
RAG
+
Prompt Caching
```

help build more efficient AI applications.

---

# Most Important Things to Remember

### Prompt Caching

```text
Reuse repeated LLM input processing.
```

Useful for:

```text
System prompts
Large repeated context
Documentation
Tool definitions
```

### Error Handling

```text
LLM APIs can fail.
```

Handle:

```text
401 → Authentication problems
429 → Rate limits
500/502/503 → Provider/server issues
Timeout → Request took too long
Network → Connection problems
```

### Retry

```text
Retry temporary failures.

Do not retry permanent errors.
```

### Logging

```text
Log useful debugging information.

Never expose secrets unnecessarily.
```

---

# Final Mental Model

```text
AI REQUEST

User
 ↓
Validate
 ↓
Rate Limit
 ↓
Cost Check
 ↓
Prompt Cache
 ↓
LLM API
 ↓
Success?
 ├── YES → Response
 │
 └── NO
      ↓
   Handle Error
      ↓
   Retry if temporary
      ↓
   Log Error
      ↓
 Safe Client Response
```

---

# Phase 13.5 — AI Integration Complete

```text
[x] Step 1 — LLM SDK in Node
[x] Step 2 — Streaming Responses (SSE) to React Frontend
[x] Step 3 — RAG Basics: Embeddings + Vector Database
[x] Step 4 — LangChain.js or Raw SDK Calls
[x] Step 5 — AI Agents in Node
[x] Step 6 — Rate Limiting + Cost Control for LLM Calls
[x] Step 7 — Prompt Caching + Error Handling for LLM APIs
```
---------------------------------------------------------------------------------------------------------------------------------------