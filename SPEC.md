# AI Agent Command Center — SPEC.md

## Concept & Vision

A sleek, cyberpunk-inspired command center for managing multiple AI agents. The interface feels like piloting a starship bridge — each agent is a "ship" you can command. Dark, atmospheric, with glowing accents that pulse with activity. The settings panel slides in like a ship manifest, and the chat interface feels like a direct neural link to your AI crew.

**Personality**: Futuristic, powerful, professional. The kind of interface you'd see in a high-tech control room.

---

## Design Language

### Aesthetic Direction
**Cyberpunk Command Center** — Deep space blacks, electric cyan accents, subtle scan-line effects, holographic glass panels. Think Blade Runner meets mission control.

### Color Palette
```css
--bg-deep: #0a0a0f;
--bg-panel: #12121a;
--bg-card: #1a1a24;
--bg-elevated: #22222e;
--border-glow: #00f0ff;
--border-subtle: #2a2a3a;
--accent-primary: #00f0ff;    /* Electric cyan */
--accent-secondary: #ff006e;  /* Hot pink */
--accent-tertiary: #8b5cf6;   /* Electric purple */
--text-primary: #e8e8f0;
--text-secondary: #8888a0;
--text-muted: #55556a;
--success: #00ff88;
--error: #ff4466;
```

### Typography
- **Display/Headers**: "Orbitron" (geometric, futuristic) — for titles, agent names, headings
- **Body/UI**: "JetBrains Mono" (clean monospace) — for chat, inputs, labels
- **Fallback**: system-ui, monospace

### Spatial System
- Base unit: 8px
- Panel padding: 24px
- Card padding: 16px
- Gap between elements: 12px
- Border radius: 8px (panels), 4px (inputs/buttons), 50% (avatars)

### Motion Philosophy
- **Panel transitions**: Slide + fade, 300ms cubic-bezier(0.4, 0, 0.2, 1)
- **Hover states**: Subtle glow expansion, 150ms
- **Message appearance**: Slide up + fade, staggered 50ms
- **Pulse effects**: Subtle breathing animation on active elements
- **Scan-line overlay**: Very subtle, 0.02 opacity, animated

### Visual Assets
- **Icons**: Lucide icons (line style, 1.5px stroke)
- **Agent avatars**: Gradient circles with initials
- **Decorative**: Corner brackets on panels, subtle grid pattern background, glow effects

---

## Layout & Structure

### Overall Architecture
```
┌─────────────────────────────────────────────────────────────┐
│  HEADER: Logo + Title + Settings Toggle                     │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   ┌─────────────────┐  ┌─────────────────────────────────┐  │
│   │   AGENT LIST    │  │         CHAT INTERFACE          │  │
│   │   (Sidebar)      │  │                                  │  │
│   │                  │  │  ┌────────────────────────────┐ │  │
│   │  [+ Add Agent]   │  │  │     Message bubbles         │ │  │
│   │                  │  │  │     with timestamps         │ │  │
│   │  ○ Agent 1       │  │  └────────────────────────────┘ │  │
│   │  ● Agent 2       │  │                                  │  │
│   │  ○ Agent 3       │  │  ┌────────────────────────────┐ │  │
│   │                  │  │  │  [Input] [Send]            │ │  │
│   └─────────────────┘  └─────────────────────────────────┘  │
│                                                              │
└─────────────────────────────────────────────────────────────┘

SETTINGS PANEL (Overlay/Slide-in):
┌─────────────────────────────────────────┐
│  ⚙️ AGENT CONFIGURATION                  │
├─────────────────────────────────────────┤
│  [Agent Cards with Name + API inputs]   │
│                                          │
│  [+ Add New Agent]                       │
│                                          │
│  [Save] [Cancel]                         │
└─────────────────────────────────────────┘
```

### Responsive Strategy
- Desktop (>1024px): Side-by-side agent list + chat
- Tablet (768-1024px): Collapsible sidebar
- Mobile (<768px): Tab-based navigation between agents and chat

### Visual Pacing
- Header: Compact, 60px height, always visible
- Sidebar: 280px width, scrollable agent list
- Chat area: Flexible, takes remaining space
- Settings overlay: Full-screen on mobile, centered modal (500px) on desktop

---

## Features & Interactions

### Core Features

#### 1. Agent Management (Settings)
- **Add Agent**: Click "+ Add Agent" → New card appears with empty fields
- **Edit Agent**: Click agent card → Fields become editable
- **Delete Agent**: Trash icon → Confirmation tooltip → Remove with fade
- **Agent Fields**:
  - Name (required, 1-30 chars)
  - API Key (required, masked by default, show/hide toggle)
  - API Endpoint URL (optional, defaults to common endpoints)
- **Validation**: Real-time validation, red border + error message on invalid fields
- **Persistence**: LocalStorage for agent configurations

#### 2. Agent Selection
- **Visual indicator**: Selected agent has glowing border + filled indicator
- **Hover state**: Slight elevation + border glow
- **Click behavior**: Select agent, load its conversation, highlight in list

#### 3. Chat Interface
- **Message display**: 
  - User messages: Right-aligned, accent gradient background
  - Agent messages: Left-aligned, panel background, avatar + name
  - Timestamps below each message
  - Loading indicator (animated dots) while waiting for response
- **Input area**:
  - Auto-expanding textarea (max 200px height)
  - Send on Enter (Shift+Enter for newline)
  - Disabled when no agent selected
- **Error handling**:
  - API error: Red banner with error message
  - Network error: Retry button
  - Invalid API: Prompt to check settings

#### 4. Conversation Management
- **Per-agent conversations**: Each agent maintains separate chat history
- **Clear chat**: Button to clear current conversation
- **Message count badge**: Show unread/total messages per agent

### Edge Cases & Error Handling
- **No agents configured**: Welcome screen with "Add your first agent" CTA
- **No agent selected**: Placeholder in chat area with instructions
- **Empty message**: Prevent sending, subtle shake animation
- **API timeout**: 60s timeout, then error state with retry
- **Long messages**: Proper word-wrap, scrollable message containers
- **Special characters**: Proper escaping and display

---

## Component Inventory

### Header
- **Default**: Dark panel, logo (stylized "AI" text), title, settings gear icon
- **Settings open**: Gear icon rotates, highlighted state
- **States**: Default only (always visible)

### Agent Card (in sidebar)
- **Default**: Dark card, agent avatar, name, subtle border
- **Hover**: Elevated shadow, border glow intensifies
- **Selected**: Cyan border glow, filled indicator dot
- **Disabled**: N/A (all agents always available if configured)
- **Loading**: Pulsing glow when agent is responding

### Agent Card (in settings)
- **Default**: Editable fields, delete button (muted)
- **Hover**: Delete button highlighted
- **Editing**: Active input borders
- **Invalid**: Red border, error message below field
- **Empty state**: Placeholder text in fields

### Message Bubble (User)
- **Default**: Right-aligned, gradient background (cyan→purple), white text
- **Sending**: Opacity 0.7, spinner overlay
- **Sent**: Full opacity
- **Error**: Red background, error icon

### Message Bubble (Agent)
- **Default**: Left-aligned, panel background, avatar + name header
- **Loading**: Typing indicator (3 animated dots)
- **Error**: Red tinted background

### Input Area
- **Default**: Dark input, subtle border, placeholder text
- **Focus**: Cyan border glow, placeholder fades
- **Filled**: White text
- **Disabled**: Muted colors, "Select an agent" placeholder
- **Sending**: Input disabled, send button shows spinner

### Buttons
- **Primary** (Send): Cyan gradient, white text, glow on hover
- **Secondary** (Cancel, Clear): Ghost style, border only
- **Danger** (Delete): Red tint, appears on hover of agent card
- **Icon buttons**: Circular, subtle background on hover
- **Disabled**: 50% opacity, no interactions

### Settings Modal
- **Default**: Centered panel, dark background, blur backdrop
- **Open animation**: Scale 0.95→1, opacity 0→1, 300ms
- **Close animation**: Reverse of open

---

## Technical Approach

### Stack
- **Single HTML file** with embedded CSS and JavaScript
- **No framework** — vanilla JS for simplicity and portability
- **LocalStorage** for persistence
- **Fetch API** for AI communication

### Architecture
```
├── index.html
│   ├── <style> — All CSS including animations
│   └── <script>
│       ├── State management (agents, messages, UI state)
│       ├── LocalStorage sync
│       ├── DOM rendering functions
│       ├── Event handlers
│       └── API communication
```

### Data Model
```javascript
// Agent Configuration (stored in localStorage)
{
  agents: [
    {
      id: "uuid",
      name: "Agent Name",
      apiKey: "sk-...",
      apiEndpoint: "https://api.openai.com/v1/chat/completions",
      createdAt: timestamp
    }
  ],
  selectedAgentId: "uuid" | null,
  conversations: {
    "agent-uuid": [
      {
        id: "msg-uuid",
        role: "user" | "assistant",
        content: "message text",
        timestamp: timestamp
      }
    ]
  }
}
```

### API Integration
- Support for OpenAI-compatible APIs
- Configurable endpoint per agent
- Request format:
```json
{
  "model": "gpt-4",
  "messages": [...],
  "stream": false
}
```
- Response parsing from standard chat completion format

### Accessibility
- Keyboard navigation for all interactive elements
- ARIA labels on buttons and inputs
- Focus management in modal
- High contrast maintained in all states
