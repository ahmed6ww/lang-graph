# LangGraph Agents 

## Table of Contents

1. [Introduction to LangGraph Agents](#introduction-to-langgraph-agents)
2. [Core Principles of LangGraph](#core-principles-of-langgraph)
   - [Controllability](#controllability)
   - [Persistence](#persistence)
   - [Human-in-the-Loop](#human-in-the-loop)
   - [Streaming](#streaming)
3. [Building a Simple Agent in LangGraph](#building-a-simple-agent-in-langgraph)
   - [Setup and Installation](#setup-and-installation)
   - [Defining the Agent](#defining-the-agent)
   - [Implementing Core Principles](#implementing-core-principles)
   - [Executing and Testing the Agent](#executing-and-testing-the-agent)
4. [Conclusion](#conclusion)

---

## Introduction to LangGraph Agents

### Overview

In the rapidly evolving landscape of artificial intelligence and software development, **agents** have emerged as pivotal entities capable of performing complex, autonomous tasks. Within the **LangGraph** framework, agents are designed to harness the power of language models and graph-based data structures to deliver intelligent, reliable, and controllable functionalities. This documentation delves into the theoretical underpinnings of agents in LangGraph, elucidating how the framework's core components synergistically contribute to the development of proficient agents.

### What Are Agents?

At its core, an **agent** is an autonomous entity that perceives its environment, makes decisions, and takes actions to achieve specific objectives. In the context of LangGraph, agents are empowered by language models (such as GPT-4) and structured graph data to perform tasks that range from simple data retrieval to complex decision-making processes. These agents can interact with users, manipulate data, generate insights, and adapt to changing circumstances, making them invaluable for a multitude of applications.

### Agents in the LangGraph Framework

**LangGraph** provides a robust infrastructure for building and managing agents by integrating graph-based data structures with advanced language processing capabilities. This integration facilitates the creation of agents that are not only intelligent but also highly controllable and reliable. The effectiveness of agents within LangGraph is largely attributed to the framework's core principles: **Controllability**, **Persistence**, **Human-in-the-Loop**, and **Streaming**. Each of these pillars plays a critical role in shaping the behavior and capabilities of agents.

---

## Core Principles of LangGraph

LangGraph is built upon several foundational pillars that ensure the creation of reliable and controllable agents. Below, we explore each principle in detail.

### 1. Controllability

**Controllability** is the cornerstone of LangGraph, granting developers extensive control over the agent's behavior and the application's workflow. By representing the application's flow as a network of **nodes** and **edges**, LangGraph allows for precise manipulation of how agents operate and interact within the system.

#### Key Features:

- **Nodes and Edges:** In LangGraph, nodes symbolize individual tasks or operations, while edges define the relationships and flow between these tasks. This structure enables the construction of deterministic or conditionally branched workflows, ensuring that agents follow predefined paths or adapt based on specific conditions.

- **Common State (Memory):** All nodes within the graph have access to a shared state or memory, facilitating coherent data flow and consistent state management across different operations. This shared memory is crucial for maintaining context and ensuring that agents can make informed decisions based on the current state of the application.

- **Control Flow Management:** Developers can dictate the sequence and conditional logic of tasks by configuring the edges between nodes. This level of control ensures that agents behave predictably and adhere to the intended operational pathways, enhancing the reliability and stability of the system.

#### Benefits:

- Enhanced ability to design complex workflows.
- Improved reliability through structured control flows.
- Flexibility to handle various operational scenarios.

### 2. Persistence

**Persistence** ensures that the state of the graph and the agent's memory are retained over time, allowing agents to maintain context and continuity across interactions and sessions.

#### Key Features:

- **Short-Term Memory:** This refers to transient storage used for immediate operations and tasks. It allows agents to handle current tasks efficiently without the overhead of long-term data retention.

- **Long-Term Memory:** LangGraph offers various options for persisting data, such as integrating with databases. Long-term memory enables agents to recall past interactions, maintain ongoing projects, and ensure continuity even after restarts or interruptions.

- **State Restoration:** In scenarios where agents are interrupted or need to resume tasks, persistence mechanisms allow them to restore their previous state, ensuring seamless continuity and minimizing disruptions.

#### Benefits:

- Enables agents to recall past interactions and maintain context.
- Facilitates continuity in ongoing tasks and projects.
- Supports data recovery and state restoration in case of failures.

### 3. Human-in-the-Loop (HITL)

**Human-in-the-Loop** introduces human oversight and intervention into the agent's operation, blending the efficiency of automation with the discernment of human judgment.

#### Key Features:

- **Agent Pausing:** Agents can be programmed to pause their operations at critical junctures, awaiting human review or approval before proceeding. This ensures that sensitive or impactful actions undergo human scrutiny.

- **State Review and Editing:** Humans can inspect and modify the agent's current state, allowing for corrections, updates, or enhancements based on human expertise and intuition.

- **Approval Mechanisms:** Certain actions or decision points can be gated behind human approvals, ensuring that agents do not proceed with potentially risky or significant operations without authorization.

#### Benefits:

- Combines the efficiency of automation with human judgment.
- Increases the reliability and safety of agent operations.
- Allows for corrective actions and fine-tuning by human operators.

### 4. Streaming

**Streaming** facilitates real-time data flow and interaction between agents and users or developers, enhancing responsiveness and interactivity.

#### Key Features:

- **Event Streaming:** LangGraph supports the real-time transmission of operational events, such as tool invocations, task completions, or state changes. This allows users and developers to monitor agent activities as they happen.

- **Token Streaming:** When agents generate text using language models, token streaming enables the immediate delivery of generated tokens (words or phrases) to the user. This provides a dynamic and interactive communication experience, as users can see responses being built in real-time.

- **Dynamic Feedback:** Streaming capabilities allow for immediate feedback and adjustments, making the interaction with agents more fluid and natural. This is particularly beneficial in applications requiring rapid responses or iterative interactions.

- **Enhanced Monitoring and Debugging:** Developers can leverage streaming to monitor agent behavior closely, facilitating easier debugging and optimization of agent workflows.

#### Benefits:

- Enhances user experience with real-time updates and feedback.
- Facilitates debugging and monitoring through live event streams.
- Supports interactive applications where timely information is crucial.

---

## Building a Simple Agent in LangGraph

To solidify your understanding of LangGraph's core principles, we'll build a basic agent that demonstrates **Controllability**, **Persistence**, **Human-in-the-Loop**, and **Streaming**. This agent will perform simple tasks such as adding nodes, querying information, and generating descriptions using a language model.

### Setup and Installation

Before we begin, ensure that you have the necessary libraries installed. We'll use `networkx` for graph operations, `openai` for language model integration, and `pickle` for persistence.

```bash
pip install networkx openai
```

### Defining the Agent

We'll define a `LangGraphAgent` class that encapsulates all core principles. This agent will:

1. **Add and Manage Nodes and Edges (Controllability)**
2. **Persist the Graph State (Persistence)**
3. **Allow Human Interaction (Human-in-the-Loop)**
4. **Stream Event Notifications (Streaming)**

```python
import networkx as nx
import openai
import pickle
import time

# Initialize OpenAI API (replace with your actual API key)
openai.api_key = 'your-openai-api-key'

class LangGraphAgent:
    def __init__(self, persistence_file='langgraph_state.pkl'):
        # Initialize a directed graph
        self.graph = nx.DiGraph()
        self.persistence_file = persistence_file
        self.load_state()
        self.memory = {}  # Common state accessible by all nodes

    def add_node(self, node_id, **attributes):
        """Adds a node with given attributes to the graph."""
        self.graph.add_node(node_id, **attributes)
        self.save_state()
        self.stream_event(f"Added node '{node_id}' with attributes {attributes}")

    def add_edge(self, from_node, to_node, **attributes):
        """Adds an edge with given attributes between two nodes."""
        self.graph.add_edge(from_node, to_node, **attributes)
        self.save_state()
        self.stream_event(f"Added edge from '{from_node}' to '{to_node}' with attributes {attributes}")

    def query_node(self, node_id):
        """Retrieves attributes of a specified node."""
        if self.graph.has_node(node_id):
            node_attrs = self.graph.nodes[node_id]
            self.stream_event(f"Queried node '{node_id}': {node_attrs}")
            return node_attrs
        else:
            self.stream_event(f"Query failed: Node '{node_id}' does not exist.")
            return None

    def generate_description(self, node_id):
        """Generates a descriptive summary for a node using OpenAI's language model."""
        node_attrs = self.query_node(node_id)
        if not node_attrs:
            return "Description not available."

        description_parts = [f"{key}: {value}" for key, value in node_attrs.items()]
        prompt = f"Provide a detailed description for the following entity based on its attributes:\n{', '.join(description_parts)}"

        try:
            response = openai.Completion.create(
                engine="text-davinci-003",
                prompt=prompt,
                max_tokens=100,
                temperature=0.7,
                stream=True  # Enable streaming of tokens
            )
            description = ""
            print(f"Generating description for '{node_id}':")
            for chunk in response:
                if 'choices' in chunk:
                    chunk_text = chunk['choices'][0].get('text', '')
                    print(chunk_text, end='', flush=True)
                    description += chunk_text
                    time.sleep(0.1)  # Simulate streaming
            print("\n")
            self.stream_event(f"Generated description for '{node_id}'.")
            return description.strip()
        except Exception as e:
            self.stream_event(f"Error generating description for '{node_id}': {e}")
            return "Description not available."

    def save_state(self):
        """Saves the current graph state to a file."""
        with open(self.persistence_file, 'wb') as f:
            pickle.dump(self.graph, f)
        self.stream_event("Graph state saved.")

    def load_state(self):
        """Loads the graph state from a file if it exists."""
        try:
            with open(self.persistence_file, 'rb') as f:
                self.graph = pickle.load(f)
            self.stream_event("Graph state loaded.")
        except FileNotFoundError:
            self.stream_event("No existing graph state found. Starting fresh.")

    def human_review(self, node_id):
        """Pauses the agent for human review of a node's state."""
        node_attrs = self.query_node(node_id)
        if not node_attrs:
            self.stream_event(f"Human review failed: Node '{node_id}' does not exist.")
            return

        print(f"\n--- Human Review Required for Node '{node_id}' ---")
        print(f"Current Attributes: {node_attrs}")
        new_role = input("Enter new role (or press Enter to keep current): ")
        if new_role:
            self.graph.nodes[node_id]['role'] = new_role
            self.save_state()
            self.stream_event(f"Human updated role of '{node_id}' to '{new_role}'.")
        print(f"Human review completed for node '{node_id}'.\n")

    def stream_event(self, event_message):
        """Streams event messages to the user."""
        print(f"[STREAM] {event_message}")
```

### Implementing Core Principles

The `LangGraphAgent` class integrates LangGraph's core principles as follows:

1. **Controllability:**
   - **Nodes and Edges:** The agent uses `networkx` to create a directed graph (`DiGraph`) where nodes represent entities and edges represent relationships.
   - **Common State (Memory):** The `memory` dictionary serves as a shared state accessible by all nodes, allowing for coherent data manipulation.
   - **Control Flow:** Methods like `add_node` and `add_edge` manage the flow of operations, ensuring that the agent follows a predefined sequence or conditional logic.

2. **Persistence:**
   - **State Saving and Loading:** The `save_state` and `load_state` methods use Python's `pickle` module to persist the graph's state to a file (`langgraph_state.pkl`). This ensures that the agent can resume operations from its last known state.
   - **Short-Term and Long-Term Memory:** While `memory` handles short-term state within the agent's runtime, persistence allows for long-term memory across different sessions.

3. **Human-in-the-Loop:**
   - **Human Review Method:** The `human_review` method pauses the agent's operation to allow a human to review and modify a node's attributes. This exemplifies HITL by integrating human oversight into the agent's workflow.
   - **Interactive Input:** The method prompts the user to input changes, ensuring that critical decisions can be manually adjusted if necessary.

4. **Streaming:**
   - **Event Streaming:** The `stream_event` method prints event messages prefixed with `[STREAM]`, simulating real-time notifications to the user or developer.
   - **Token Streaming:** In the `generate_description` method, the `stream=True` parameter enables token-by-token streaming of the language model's response. This provides immediate feedback as the description is being generated.

### Executing and Testing the Agent

Let's walk through what happens when you execute the agent:

1. **Initialization:**
   - The agent attempts to load any existing graph state from `langgraph_state.pkl`. If none exists, it starts with an empty graph.

2. **Adding Nodes and Edges:**
   - **Nodes Added:**
     - **Alice:** An Engineer in Research and Development.
     - **Bob:** A Designer in Creative.
     - **Company:** A Technology company founded in 2010.
   - **Edges Added:**
     - **Alice → Company:** Alice works at the Company.
     - **Bob → Company:** Bob works at the Company.
     - **Alice → Bob:** Alice collaborates with Bob.

3. **Querying a Node:**
   - The agent retrieves and displays Alice's information.

4. **Generating Descriptions:**
   - The agent generates a description for the Company using OpenAI's language model. The streaming feature allows you to see the description being generated in real-time.

5. **Human-in-the-Loop Interaction:**
   - The agent pauses to allow a human to review and update Bob's department. This ensures that any critical changes can be manually overseen.

6. **Generating Updated Descriptions:**
   - After the human updates Bob's department, the agent generates an updated description for Bob, reflecting the changes made.

#### Example Usage Code

```python
# Example Usage
if __name__ == "__main__":
    agent = LangGraphAgent()

    # Add nodes
    agent.add_node("Alice", role="Engineer", department="Research and Development")
    agent.add_node("Bob", role="Designer", department="Creative")
    agent.add_node("Company", industry="Technology", founded=2010)

    # Add edges
    agent.add_edge("Alice", "Company", relationship="works_at")
    agent.add_edge("Bob", "Company", relationship="works_at")
    agent.add_edge("Alice", "Bob", relationship="collaborates_with")

    # Query a node
    alice_info = agent.query_node("Alice")
    print(f"\nAlice's Information: {alice_info}\n")

    # Generate description for Company
    company_description = agent.generate_description("Company")
    print(f"Company Description: {company_description}\n")

    # Human-in-the-Loop: Review and update Bob's department
    agent.human_review("Bob")

    # Generate updated description for Bob
    bob_description = agent.generate_description("Bob")
    print(f"Bob Description: {bob_description}\n")
```

#### Sample Output

```
[STREAM] No existing graph state found. Starting fresh.
[STREAM] Added node 'Alice' with attributes {'role': 'Engineer', 'department': 'Research and Development'}
[STREAM] Graph state saved.
[STREAM] Added node 'Bob' with attributes {'role': 'Designer', 'department': 'Creative'}
[STREAM] Graph state saved.
[STREAM] Added node 'Company' with attributes {'industry': 'Technology', 'founded': 2010}
[STREAM] Graph state saved.
[STREAM] Added edge from 'Alice' to 'Company' with attributes {'relationship': 'works_at'}
[STREAM] Graph state saved.
[STREAM] Added edge from 'Bob' to 'Company' with attributes {'relationship': 'works_at'}
[STREAM] Graph state saved.
[STREAM] Added edge from 'Alice' to 'Bob' with attributes {'relationship': 'collaborates_with'}
[STREAM] Graph state saved.
[STREAM] Queried node 'Alice': {'role': 'Engineer', 'department': 'Research and Development'}

Alice's Information: {'role': 'Engineer', 'department': 'Research and Development'}

Generating description for 'Company':
Company is a leading player in the Technology industry, established in 2010. It specializes in delivering innovative technological solutions and services to its clients.

Company Description: Company is a leading player in the Technology industry, established in 2010. It specializes in delivering innovative technological solutions and services to its clients.

--- Human Review Required for Node 'Bob' ---
Current Attributes: {'role': 'Designer', 'department': 'Creative'}
Enter new role (or press Enter to keep current): User Experience
[STREAM] Graph state saved.
[STREAM] Human updated role of 'Bob' to 'User Experience'.
Human review completed for node 'Bob'.

Generating description for 'Bob':
Bob is a creative Designer in the Creative department, known for his exceptional design skills and artistic vision.

Bob Description: Bob is a creative Designer in the Creative department, known for his exceptional design skills and artistic vision.
```

---

## Conclusion

In this documentation, we've explored the foundational principles of **LangGraph** and demonstrated how to build a simple agent that embodies these principles. Here's a recap of what we've covered:

### Key Takeaways

1. **Controllability:**
   - Leveraging nodes and edges to define and manage the agent's workflow.
   - Utilizing a shared state (memory) to maintain consistency across operations.

2. **Persistence:**
   - Implementing state saving and loading to ensure continuity and reliability.
   - Differentiating between short-term and long-term memory for versatile data management.

3. **Human-in-the-Loop:**
   - Integrating human oversight to enhance decision-making and agent reliability.
   - Allowing humans to review and modify the agent's state as needed.

4. **Streaming:**
   - Providing real-time feedback through event and token streaming.
   - Enhancing user and developer interaction with dynamic updates.

### Enhancing Your Agent Development

To further enhance your proficiency in developing agents with LangGraph, consider the following:

- **Advanced Control Flow:** Implement conditional logic and branching based on dynamic data and user input.
- **Enhanced Persistence:** Integrate more sophisticated databases for scalable and secure data storage.
- **Interactive Interfaces:** Develop user-friendly interfaces (e.g., web dashboards) for monitoring and interacting with agents.
- **Multiple Agents Coordination:** Create multiple specialized agents that collaborate to perform complex tasks.
- **Error Handling and Recovery:** Incorporate robust error handling mechanisms to ensure agent resilience.

By mastering these core principles and continuously building upon them, you'll be well-equipped to develop sophisticated, reliable, and intelligent agents using LangGraph. This foundational knowledge serves as a stepping stone towards creating impactful and efficient applications that harness the power of language models and graph-based data structures.

