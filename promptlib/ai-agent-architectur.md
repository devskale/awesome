# AI Agent Architecture Guide: Building Tool-Enabled Agents with AI SDK

## Overview

This guide outlines a comprehensive architecture for building AI agents with tools using the Vercel AI SDK. The architecture emphasizes modularity, type safety, streaming responses, and clean separation of concerns between agent logic, tool execution, and UI integration.

## Core Architecture Principles

### 1. **DRY (Don't Repeat Yourself) Design**

- Centralize constants, schemas, and configurations
- Use TypeScript enums and strict validation
- Generate descriptions and prompts dynamically from constants
- Maintain single source of truth for action types and parameters

### 2. **Type Safety First**

- Leverage TypeScript for compile-time error detection
- Use Zod for runtime validation at boundaries
- Implement context-aware validation based on action types
- Create type guards for safe runtime type checking

### 3. **Tool-Centric Architecture**

- Tools are the primary interface between AI agents and external systems
- Each tool should be self-contained with minimal processing logic
- Schema-first design with comprehensive validation
- Tools can be other LLM calls (sub-agents) for complex operations

## System Architecture Components

### 1. API Route Layer (`/api/agents/[agentName]/route.ts`)

**Purpose**: Server-side endpoint for agent interactions

**Key Responsibilities**:

- Model selection and provider management (OpenAI, Anthropic, Google)
- System prompt composition (core + custom instructions)
- Request validation and error handling
- Streaming response coordination
- Tool registration and configuration

**Architecture Pattern**:

```typescript
export async function POST(req: Request) {
  // 1. Parse and validate request
  const { messages, modelId, customInstructions } = await req.json();

  // 2. Configure LLM provider
  const llm = selectProvider(modelId);

  // 3. Compose system prompt
  const systemPrompt = buildSystemPrompt(corePrompt, customInstructions);

  // 4. Execute streaming agent
  const result = await streamText({
    model: llm,
    system: systemPrompt,
    messages,
    tools: registeredTools,
    maxSteps: 15,
    toolChoice: "auto",
  });

  return result.toDataStreamResponse();
}
```

### 2. Tool System Architecture

#### Tool Definition (`/lib/tools/`)

**Structure**:

- Individual tool files (`toolName.ts`)
- Centralized type definitions (`types.ts`)
- Barrel exports (`index.ts`)
- Consistent schema validation

**Tool Implementation Pattern**:

```typescript
// types.ts - Centralized constants
export const ACTION_TYPES = {
  ACTION_ONE: "actionOne",
  ACTION_TWO: "actionTwo",
} as const;

export const ToolSchema = z
  .object({
    type: z.enum([ACTION_TYPES.ACTION_ONE, ACTION_TYPES.ACTION_TWO]),
    // ... other fields
  })
  .refine((data) => {
    // Context-aware validation
  });

// toolName.ts - Tool implementation
export const myTool = tool({
  description: generateToolDescription(),
  parameters: ToolSchema,
  execute: async (params) => {
    // Minimal processing, primarily validation and pass-through
    return { result: processedAction };
  },
});
```

#### Tool Design Principles:

- **Minimal Processing**: Tools validate and pass data, avoid complex logic
- **Schema-First**: Let Zod handle validation
- **Boundary Validation**: Verify ranges, positions, numeric inputs
- **Meaningful Errors**: Provide actionable error messages

### 3. Streaming Response Architecture

#### Stream Handler Pattern:

```typescript
const handleStreamingResponse = async (response, config, messageId) => {
const reader = response.body.getReader();
const decoder = new TextDecoder();

while (!done) {
const { value, done: readerDone } = await http://reader.read();
const chunk = decoder.decode(value, { stream: true });
const lines = chunk.split("\n").filter((line) => line.trim());

for (const line of lines) {
// Parse different stream prefixes:
// 0: - Text content
// 9: - Tool call invocation
// a: - Tool result
// 3: - Errors
// 4: - Tool errors
await handleStreamLine(line, messageId);
}
}
};
```

#### Stream Processing Types:

- **Text Streams (`0:`)**: Incremental text content
- **Tool Invocations (`9:`)**: Tool call initiation
- **Tool Results (`a:`)**: Tool execution results
- **Error Handling (`3:`, `4:`)**: Various error types
- **Metadata (`d:`, `f:`)**: Additional context data

### 4. State Management Architecture

#### Chat Logic Hook Pattern:

```typescript
export function useChatLogic() {
  // Core state
  const [messages, setMessages] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [selectedModel, setSelectedModel] = useState();

  // Agent configuration
  const [mode, setMode] = useState("default");
  const [customInstructions, setCustomInstructions] = useState();

  // Submission handler
  const handleSubmit = useCallback(
    async (input, context) => {
      // Prepare request with context
      // Execute streaming request
      // Handle responses and update UI state
    },
    [dependencies]
  );

  return {
    // State
    messages,
    isLoading,
    selectedModel,
    // Actions
    handleSubmit,
    setMode,
    setSelectedModel,
    // Configuration
    availableModels,
    availableInstructions,
  };
}
```

#### State Separation Concerns:

- **Chat Logic**: Message flow, model selection, submission handling
- **Input Management**: Rich input, command detection, context extraction
- **Streaming**: Response processing, tool result handling
- **UI State**: Loading states, error handling, display logic

### 5. Prompt Engineering Architecture

#### System Prompt Composition:

```typescript
// Core agent prompt (always first)
const CORE_AGENT_PROMPT = `Your primary role and capabilities...`;

// Dynamic instruction augmentation
const buildSystemPrompt = (core, customInstructions, dynamicContext) => {
  let prompt = core;

  if (customInstructions) {
    prompt += `\n\n--- Custom Guidelines ---\n${customInstructions}\n---`;
  }

  if (dynamicContext) {
    prompt += `\n\n--- Context ---\n${dynamicContext}\n---`;
  }

  return prompt;
};
```

#### Prompt Strategy Patterns:

- **Tool-First Instructions**: Always lead with tool capabilities
- **Dynamic Generation**: Generate descriptions from constants
- **Context Injection**: Append user-specific context
- **Template Support**: Dynamic document templates
- **Multi-Step Guidance**: Clear action sequence instructions

### 6. UI Integration Architecture

#### Component Separation:

- **Agent Components** (`/components/agent/`): Agent-specific UI
- **Tool Renderers** (`/components/agent/tool-renderers/`): Tool-specific displays
- **Chat Interface**: Generic conversation UI
- **Input Components**: Rich input with command support

#### Tool Invocation Rendering:

```typescript
// Tool-specific renderer component
export function ToolRenderer({ toolInvocation }) {
const { toolName, args, result, state } = toolInvocation;

switch (state) {
case 'call':
return <ToolCallDisplay args={args} />;
case 'result':
return <ToolResultDisplay result={result} />;
case 'error':
return <ToolErrorDisplay error={result} />;
}
}

// Main chat component uses renderers
export function ChatInterface() {
return http://messages.map(message => {
if (message.toolInvocations) {
return http://message.toolInvocations.map(invocation => (
<ToolRenderer key={invocation.toolCallId} toolInvocation={invocation} />
));
}
return <MessageDisplay message={message} />;
});
}
```

## Advanced Patterns

### 1. Multi-Agent Systems

#### Orchestrator-Worker Pattern:

- **Orchestrator Agent**: Routes requests to specialized workers
- **Worker Agents**: Specialized for specific domains/tasks
- **Tool-as-Agent**: Complex tools implemented as sub-agents

#### Implementation:

```typescript
// Orchestrator routes to specialized agents
const routingTool = tool({
  description: "Route request to appropriate specialist",
  parameters: z.object({
    specialistType: z.enum(["content", "analysis", "code"]),
    request: z.string(),
  }),
  execute: async ({ specialistType, request }) => {
    const specialist = getSpecialistAgent(specialistType);
    return await specialist.process(request);
  },
});
```

### 2. Multi-Step Tool Usage

#### Pattern:

```typescript
const result = await streamText({
  model: llm,
  system: systemPrompt,
  messages,
  tools: registeredTools,
  maxSteps: 15, // Allow iterative tool usage
  toolChoice: "auto",
});
```

#### Use Cases:

- Complex workflows requiring multiple tool calls
- Iterative refinement processes
- Decision trees with conditional tool usage

### 3. Context Management

#### Document Context Pattern:

```typescript
// Context extraction from user input
const extractContext = (userInput) => {
  const documentRefs = extractDocumentReferences(userInput);
  const contextualizedInput = buildContextualizedPrompt(
    userInput,
    documentRefs
  );
  return contextualizedInput;
};

// Context injection in system prompt
const systemPromptWithContext = `
${basePrompt}

CONTEXT PROVIDED:
${contextData}

USER QUERY REGARDING CONTEXT:
${userQuery}
`;
```

### 4. Error Handling & Recovery

#### Layered Error Strategy:

1. **Schema Validation**: Catch parameter errors early
2. **Tool Execution**: Handle operational failures gracefully
3. **Stream Processing**: Manage connection/parsing issues
4. **UI Error States**: Present user-friendly error messages
5. **Fallback Strategies**: Provide alternative approaches

### 5. External Service Integration

#### MCP (Model Context Protocol) Pattern:

```typescript
// MCP client management
const mcpClient = experimental_createMCPClient({
  name: "external-service",
  version: "1.0.0",
});

// Use MCP tools in agent
const tools = {
  ...localTools,
  ...mcpClient.tools(), // External service tools
};
```

## Implementation Checklist

### Phase 1: Foundation

- [ ] Set up API route structure
- [ ] Define core tool types and schemas
- [ ] Implement basic streaming handler
- [ ] Create base system prompts

### Phase 2: Tool System

- [ ] Build tool registry and validation
- [ ] Implement tool execution framework
- [ ] Add error handling and recovery
- [ ] Create tool-specific UI renderers

### Phase 3: Advanced Features

- [ ] Add multi-step tool usage
- [ ] Implement custom instruction system
- [ ] Build context management
- [ ] Add multi-agent capabilities

### Phase 4: Polish

- [ ] Optimize streaming performance
- [ ] Enhance error messages
- [ ] Add comprehensive testing
- [ ] Document API contracts

## Best Practices

1. **Start Simple**: Begin with basic tool usage, add complexity incrementally
2. **Type Safety**: Use TypeScript and Zod for all boundaries
3. **Streaming First**: Design for streaming from the beginning
4. **Tool Isolation**: Keep tools focused and independent
5. **Error Transparency**: Provide clear error messages and recovery paths
6. **Performance**: Monitor token usage and response times
7. **Testing**: Test tool execution independently from agent logic
8. **Documentation**: Maintain clear API contracts and examples

This architecture provides a robust foundation for building sophisticated AI agents while maintaining clean separation of concerns and enabling future extensibility.
