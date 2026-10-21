---
title: "Event 2"
date: 2026-08-08
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Report: "AWS FCAJ Agent Forge - Deep Dive Day 2"

### Purpose of the Event

- Overview of Agent Core
- Core components of an Agent: Memory, Observability, Evaluation, Policy & Security, Tools & Capability
- Supporting services such as Browser (web browsing/search), Code Interpreter (code execution environment), and Payment (payment integration)
- Hands-on Lab practice

### Speaker List

- **Nghia Tran** – Core components of an Agent
- **Anh Pham** – Hands-on Lab

### Key Highlights

#### AI Agent Knowledge

- How context window sizing works, and Agent Core memory: understanding the context window limits of models (e.g., Claude can go up to 1 million tokens) to see why Memory is essential for storing and personalizing the user experience.
- Storage and retrieval mechanisms of Short-term & Long-term memory: Short-term memory stores raw messages synchronously to keep conversations coherent, while Long-term memory asynchronously extracts important information to store as vectors, using 4 main strategies: Summary, User Preference, Semantic, and Episodic.
- Memory partitioning via Namespace and access-control security: Using a Namespace structure (folder-like) to partition data for faster search and token savings. Security is established through IAM policies and fine-grained access-scoping mechanisms between agents.

#### Pillars of Monitoring and Evaluation for Agentic AI Systems

- **Observability**: Operates on the philosophy "You cannot fix what you cannot see." Integrates OpenTelemetry to collect data across 3 pillars: Logs (errors/system state), Traces (conversation flow), and Metrics (latency, token cost).
- **Evaluation**: Helps detect hallucinations and faulty reasoning that lead to incorrect tool calls. Provides 13 built-in evaluators running either On-demand (during development) or Online (real-time in production).

#### Powerful Supporting Services (Capabilities)

- **Cedar Policy & Security**: Controls Agent behavior using the Cedar policy language (e.g., setting rules to auto-approve refunds under $100). Supports human-in-the-loop approval when thresholds are exceeded.
- **Browser & Code Interpreter (Sandbox)**: Creates a safe simulated environment for the Agent to automatically browse the web/search for information and write/test code without affecting the user's real system.
- **Payment & Registry**: Direct payment integration via Stripe/Coinbase. Sets up a Registry to register and reuse tools/APIs across departments within an organization.
- **Agent Harness & Knowledge Base**: Quickly bootstraps a minimal agent with just a model, system prompt, and tools. Connects RAG to an organization's internal data (S3, SharePoint, OneDrive).

### Key Takeaways

#### Design Thinking

- **System Engineering mindset**: Realizing that when building an AI Agent for a real enterprise, the core AI technology accounts for only 6%, while the remaining 93% is software engineering — memory handling, tool integration, monitoring, and security.
- **Personalization & Responsibility**: The importance of personalizing the user experience through long-term memory, while complying with sensitive customer data security rules and using AI responsibly.

#### Technical Architecture

- **Combining Memory sensibly**: To optimize cost and avoid overflowing the context window, one needs to skillfully combine partial retrieval of short-term memory with key insights from long-term memory.
- **Monitoring via AI Judge**: Understanding how a mechanism that uses another intelligent AI model as a "referee" (LLM Judge) operates to evaluate each of the Agent's reasoning steps against industry standards.

### Applications to My Work

- **Applying Bedrock Agent Core Memory**: Implementing a chat-history storage mechanism for the current project with retention configuration (e.g., keeping data for 7 days), partitioning customer data by Namespace to optimize token cost and improve search accuracy.
- **Integrating AWS CloudWatch / Observability**: Setting up monitoring metrics for latency and token consumption cost of each model to promptly adjust the system prompt.
- **Experimenting with the Nova Micro model**: Configuring simple tasks to switch from Claude to AWS's new-generation Amazon Nova Micro model line to increase response speed and save on testing budget.
- **Setting up a secure sandbox**: Experimenting with integrating a simulated Code Interpreter to let the Agent automatically perform complex calculations or data analysis in isolation.

### Event Experience

Attending the **"AWS FCAJ Agent Forge - Deep Dive Day 2"** workshop was a very rewarding experience, providing an extremely large amount of knowledge at an advanced level (L300 - Advanced). The speakers' theoretical lectures helped the audience easily visualize how the system operates under the hood before the Agent Core tools automate and simplify it.

#### Learning from Highly Skilled Speakers

- The speakers from AWS shared their experience using the Kiro IDE with its file-steering mechanism to guide context, using UV to manage environments, and deploying AWS resources directly via the Agent Core CLI / CDK.

#### Hands-on Technical Experience

- Explored the **modular architecture of Amazon Bedrock Agent Core**, which helped visualize how an Agentic AI system is decomposed into independent services for easier scaling and management.
- Learned how to set up and operate a multi-layer **Memory** system. Clearly distinguished between **Short-term memory**, which operates synchronously to maintain conversation flow, and **Long-term memory**, which operates asynchronously to extract long-term knowledge in vector form.
- Gained a deeper understanding of the **Namespace** structure (tree/folder format) used in long-term memory. This mechanism helps isolate the storage space of each actor/session and narrows the scope of similarity search, saving tokens and optimizing cost.
- Learned the system-monitoring approach through the **3 pillars of Observability (Logs, Traces, Metrics)** based on the **OpenTelemetry** protocol. This helps visually measure latency, cost, and token consumption of each model.

#### Applying Modern Tools

- Got hands-on with the AI-assisted code editor **Kiro IDE**, learning about the steering-document mechanism to quickly and accurately guide project context.
- Practiced using UV to manage and install dependencies in an isolated environment, along with **Agent Core CLI and CDK** to automate the packaging process and deploy resources to AWS via CloudFormation.
- Learned how to configure and switch the default model to the new-generation **Amazon Nova Micro** model line. This significantly increases response speed and maximizes cost savings during testing.

#### Networking and Exchange

- An opportunity to connect and learn directly within the **First Cloud and AI (FCAJ)** community, gaining access to real-world deployment scenarios shared by experienced AWS Solution Architects in Vietnam.
- Gained a valuable real-world perspective from experts: in a real enterprise-grade Agentic AI system, the core AI technology accounts for only 6%, while the remaining 93% belongs to software engineering — memory handling, tool integration, monitoring, and security.

#### Lessons Learned

- Building an AI Agent for an enterprise requires a rigorous multi-layered security mindset through detailed **Access Scoping** policies and the **Cedar Policy** language, in order to prevent sensitive data leakage between Agents.
- The Agent capability verification process (Evaluation), using 13 built-in evaluators split across 3 levels (Session, Trace, Span) combined with the LLM Judge mechanism, is a core solution for systematically detecting hallucinations before deploying a system to production.
- Migrating Agentic AI systems to serverless cloud platforms like Bedrock Agent Core helps organizations achieve high reliability in both **Security** and **Scalability**, at a cost optimized for actual usage volume.

#### Photos from the Event
* Add your photos here
> Overall, this event provided not only technical knowledge but also helped me shift my mindset on application design, system modernization, and more effective cross-team collaboration.