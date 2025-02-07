# OpenHands Repository Understanding

This document describes how OpenHands actually works based on the code structure and implementation.

## Core Architecture

OpenHands is built with a microagent architecture, consisting of:

1. **Core Runtime System** (`openhands/core/`):
   - Main application loop (`loop.py`)
   - Configuration management (`config/`)
   - Message handling and formatting (`message.py`, `message_format.md`)
   - Schema definitions for actions, agents, and observations (`schema/`)

2. **Agent System** (`openhands/agenthub/`):
   - Multiple specialized agents:
     - `browsing_agent`: Handles web browsing tasks
     - `codeact_agent`: Performs code-related actions
     - `delegator_agent`: Manages task delegation
     - `visualbrowsing_agent`: Visual web interaction
     - Microagents (`micro/`): Small, specialized agents for specific tasks like:
       - Code writing (`coder/`)
       - Commit writing (`commit_writer/`)
       - Repository exploration (`repo_explorer/`)
       - Task study (`study_repo_for_task/`)
       - Math operations (`math_agent/`)
       - PostgreSQL operations (`postgres_agent/`)
       - Code verification (`verifier/`)

3. **Event System** (`openhands/events/`):
   - Action handling (`action/`): Browse, file operations, messaging
   - Observation handling (`observation/`): Agent responses, errors, success states
   - Event serialization (`serialization/`)
   - Tool integration (`tool.py`)

4. **Memory Management** (`openhands/memory/`):
   - Conversation memory handling
   - Memory condensers for managing conversation history:
     - Amortized forgetting
     - LLM attention-based
     - LLM summarizing
     - Observation masking
     - Recent events tracking

5. **Runtime Environment** (`openhands/runtime/`):
   - Multiple runtime implementations:
     - Docker-based (`impl/docker/`)
     - E2B sandbox (`impl/e2b/`)
     - Modal cloud (`impl/modal/`)
     - Remote execution (`impl/remote/`)
   - Browser environment (`browser/`)
   - Plugin system (`plugins/`) with:
     - Agent skills
     - Jupyter integration
     - VSCode integration

6. **Security System** (`openhands/security/`):
   - Security analysis (`analyzer.py`)
   - Invariant checking (`invariant/`)
   - Policy enforcement (`invariant/policies.py`)

7. **Server & API** (`openhands/server/`):
   - FastAPI-based server (`app.py`)
   - Authentication (`auth.py`)
   - Conversation management (`conversation_manager/`)
   - WebSocket support (`listen.py`, `listen_socket.py`)
   - API routes (`routes/`)

## Frontend Architecture (`frontend/src/`)

1. **Core Components**:
   - API integration (`api/`)
   - State management (`state/`)
   - Routing system (`routes/`)
   - WebSocket client (`context/ws-client-provider.tsx`)

2. **Main Features**:
   - Chat interface (`components/features/chat/`)
   - Browser integration (`components/features/browser/`)
   - File explorer (`components/features/file-explorer/`)
   - Terminal emulation (`components/features/terminal/`)
   - Jupyter notebook integration (`components/features/jupyter/`)
   - GitHub integration (`components/features/github/`)

3. **State Management**:
   - Redux slices for:
     - Agent state (`state/agent-slice.tsx`)
     - Browser state (`state/browser-slice.ts`)
     - Chat state (`state/chat-slice.ts`)
     - Code state (`state/code-slice.ts`)
     - File system state (`state/file-state-slice.ts`)
     - Security analyzer state (`state/security-analyzer-slice.ts`)

4. **User Interface**:
   - Shared components (`components/shared/`)
   - Layout components (`components/layout/`)
   - Modal dialogs (`components/shared/modals/`)
   - Custom hooks (`hooks/`)
   - Internationalization support (`i18n/`)

## Storage and Persistence

1. **Storage System** (`openhands/storage/`):
   - Multiple storage backends:
     - Local storage
     - Google Cloud Storage
     - Amazon S3
   - Conversation storage
   - Settings storage
   - File management

## Key Workflows

1. **Task Execution**:
   - User input is processed through the chat interface
   - The delegator agent determines which specialized agent should handle the task
   - The chosen agent executes the task using available tools and runtime environment
   - Results are observed, processed, and returned to the user

2. **Security Checks**:
   - Code and actions are analyzed by the security system
   - Invariant checking ensures safe execution
   - Policy enforcement prevents unauthorized actions

3. **Memory Management**:
   - Conversations are stored and managed
   - Memory condensers optimize conversation history
   - Different condensing strategies based on context and needs

4. **Development Workflow**:
   - File system operations through the file explorer
   - Code editing and execution
   - Terminal access for system commands
   - GitHub integration for repository management

This understanding is based on the actual code structure and implementation, not theoretical design principles.