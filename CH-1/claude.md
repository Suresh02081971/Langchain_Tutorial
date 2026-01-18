Building an Agent with Tools
from langchain.agents import create_agent
from langchain.tools import tool

# Define a tool
@tool
def get_weather(city: str) -> str:
    """Get weather for a given city."""
    return f"It's always sunny in {city}!"

# Create the agent
agent = create_agent(
    model="claude-sonnet-4-5-20250929",
    tools=[get_weather],
    system_prompt="You are a helpful assistant",
)

# Run the agent
response = agent.invoke(
    {"messages": [{"role": "user", "content": "what is the weather in San Francisco?"}]}
)

Adding Memory

from langgraph.checkpoint.memory import InMemorySaver

# Add memory checkpointer
checkpointer = InMemorySaver()

agent = create_agent(
    model="claude-sonnet-4-5-20250929",
    tools=[get_weather],
    system_prompt="You are a helpful assistant",
    checkpointer=checkpointer
)

# Use thread_id to maintain conversation state
config = {"configurable": {"thread_id": "conversation-1"}}

response1 = agent.invoke(
    {"messages": [{"role": "user", "content": "My name is Alice"}]},
    config=config
)

response2 = agent.invoke(
    {"messages": [{"role": "user", "content": "What's my name?"}]},
    config=config  # Same thread_id remembers previous context
)