# Prompt Engineering Quizzes for AI Enthusiasts
## Quiz 1: GPT-4.1 Prompting Guide

**Instructions**  
This quiz tests your understanding of the *GPT-4.1 Prompting Guide*, focusing on agentic workflows, long context, chain-of-thought, instruction following, and diff generation.  
- Questions include multiple-choice, scenario-based, and short-answer formats.  
- Provide concise, accurate responses.  
- **Total: 100 points**

### Section 1: Agentic Workflows (20 points)

**Multiple Choice (5 points)**  
What are the three key types of reminders recommended for agentic prompts in GPT-4.1 to maximize its capabilities?  
a) Persistence, Tool-calling, Output formatting  
b) Persistence, Tool-calling, Planning  
c) Planning, Debugging, Testing  
d) Tool-calling, Output formatting, Reflection  
**Answer**: b) Persistence, Tool-calling, Planning  

**Scenario-Based (10 points)**  
You’re building an agentic workflow for GPT-4.1 to resolve a bug in a Python codebase. The model keeps prematurely yielding control back to the user before fully resolving the issue. Based on the guide, how would you modify the system prompt to address this issue? Provide a specific prompt component and explain why it helps.  
**Answer**:  
Add to the system prompt:  
```
You are an agent - please keep going until the user’s query is completely resolved, before ending your turn and yielding back to the user. Only terminate your turn when you are sure that the problem is solved.
```  
**Explanation**: This persistence reminder ensures the model continues working autonomously until the bug is fully resolved, preventing premature termination. The guide notes a 20% increase in SWE-bench Verified score with such instructions.

**Short Answer (5 points)**  
Why does the guide recommend using API-parsed tool descriptions instead of manually injecting tool schemas into the system prompt?  
**Answer**: API-parsed tool descriptions minimize errors and keep the model in distribution during tool-calling, improving reliability. The guide cites a 2% increase in SWE-bench Verified pass rate.

### Section 2: Long Context (20 points)

**Multiple Choice (5 points)**  
What is the recommended placement for instructions when working with long context in GPT-4.1 prompts?  
a) Only at the beginning of the context  
b) Only at the end of the context  
c) Both at the beginning and end of the context  
d) Within the context, interspersed with documents  
**Answer**: c) Both at the beginning and end of the context  

**Scenario-Based (10 points)**  
You’re using GPT-4.1 to parse a 1M token context containing a mix of relevant and irrelevant code files to answer a query about a specific function’s behavior. The model struggles to focus on the relevant files. How would you structure the prompt to improve performance? Provide a specific approach and explain its benefit.  
**Answer**:  
Structure the prompt:  
```
# Instructions
Only use the provided documents to answer the query. Analyze each document for relevance before responding. If no relevant information is found, respond with "I don't have the information needed to answer that."
# External Context
[1M token context with code files]
# Final Instructions
Follow the instructions above: analyze documents for relevance, use only provided context, and respond concisely.
```  
**Explanation**: Placing instructions at both ends improves performance for long contexts, as per the guide. Explicit relevance analysis optimizes recall and accuracy.

**Short Answer (5 points)**  
Why might GPT-4.1’s performance degrade in long context tasks requiring complex reasoning, such as graph search?  
**Answer**: Performance may degrade because complex reasoning tasks require maintaining state across the entire 1M token context, which is challenging despite strong needle-in-a-haystack performance.

### Section 3: Chain of Thought (20 points)

**Multiple Choice (5 points)**  
How does the guide suggest inducing chain-of-thought (CoT) reasoning in GPT-4.1, which is not a reasoning model?  
a) Rely on the model’s internal reasoning capabilities  
b) Prompt the model to think step by step explicitly  
c) Use multiple parallel tool calls  
d) Provide only the final answer without intermediate steps  
**Answer**: b) Prompt the model to think step by step explicitly  

**Scenario-Based (10 points)**  
You’re designing a prompt for GPT-4.1 to select relevant documents from a large set for answering a user query. The model sometimes misses key documents. Create a chain-of-thought prompt to improve document selection and explain how it addresses the issue.  
**Answer**:  
Prompt:  
```
# Reasoning Strategy
1. Query Analysis: Break down the user query to identify key topics and requirements.
2. Context Analysis: Review all provided documents. For each:
   a. Analyze how it relates to the query.
   b. Assign a relevance rating: [high, medium, low, none].
3. Synthesis: Summarize which documents are most relevant (medium or higher) and why.
# User Question
{user_question}
# External Context
{external_context}
First, think carefully step by step about what documents are needed, following the Reasoning Strategy. Then, list the TITLE and ID of relevant documents and format their IDs into a list.
```  
**Explanation**: This CoT prompt forces systematic query and document analysis, improving recall by ensuring relevant documents are identified, as per the guide’s recommendation.

**Short Answer (5 points)**  
How much did inducing explicit planning improve the SWE-bench Verified pass rate in the guide’s experiments?  
**Answer**: Inducing explicit planning increased the SWE-bench Verified pass rate by 4%.

### Section 4: Instruction Following (20 points)

**Multiple Choice (5 points)**  
What is a common failure mode when instructing GPT-4.1 to always call a tool before responding?  
a) The model refuses to call tools  
b) The model hallucinates tool inputs or calls with null values  
c) The model ignores the instruction entirely  
d) The model repeats sample phrases verbatim  
**Answer**: b) The model hallucinates tool inputs or calls with null values  

**Scenario-Based (10 points)**  
You’re developing a customer service agent with GPT-4.1, but the model keeps discussing prohibited topics (e.g., politics). How would you modify the prompt to prevent this? Provide a specific prompt component and explain its impact.  
**Answer**:  
Add to the system prompt:  
```
# Instructions
- Do not discuss prohibited topics (politics, religion, controversial current events, medical, legal, or financial advice, personal conversations, internal company operations, or criticism of any people or company).
- If a user asks about a prohibited topic, respond with: "I'm sorry, but I'm unable to discuss that topic. Is there something else I can help you with?"
```  
**Explanation**: The guide emphasizes literal instruction following. Explicitly listing prohibited topics and providing a deflection phrase ensures the model avoids these topics and redirects appropriately.

**Short Answer (5 points)**  
Why might existing prompts optimized for other models fail with GPT-4.1?  
**Answer**: Existing prompts may fail because GPT-4.1 follows instructions more literally and does not infer implicit rules as strongly, requiring explicit and precise instructions.

### Section 5: Generating and Applying File Diffs (20 points)

**Multiple Choice (5 points)**  
What is a key characteristic of the V4A diff format recommended for GPT-4.1?  
a) Uses line numbers to identify code changes  
b) Relies on context (e.g., 3 lines before/after) instead of line numbers  
c) Requires absolute file paths  
d) Does not support multiple changes in a single file  
**Answer**: b) Relies on context (e.g., 3 lines before/after) instead of line numbers  

**Scenario-Based (10 points)**  
You’re tasked with generating a diff to fix a bug in `search.py` where a function incorrectly returns `None`. The correct implementation should raise a `NotImplementedError`. Write a V4A diff to apply this change and explain why the format is effective.  
**Answer**:  
Diff:  
```
%%bash
apply_patch <<"EOF"
*** Begin Patch
*** Update File: search.py
@@ def search():
-        return None
+        raise NotImplementedError()
*** End Patch
EOF
```  
**Explanation**: The V4A diff format uses context (e.g., `@@ def search():`) to identify code without line numbers, ensuring reliability if the codebase changes. Clear `+` and `-` syntax ensures precise replacement, and training alignment improves accuracy.

**Short Answer (5 points)**  
What are the two key aspects shared by effective diff formats like V4A, SEARCH/REPLACE, and pseudo-XML?  
**Answer**: Effective diff formats (1) do not use line numbers and (2) provide both the exact code to be replaced and the replacement code with clear delimiters.

### Section 6: General Advice (10 points)

**Multiple Choice (5 points)**  
Which delimiter format does the guide recommend starting with for structuring prompts?  
a) JSON  
b) Markdown  
c) XML  
d) Plain text  
**Answer**: b) Markdown  

**Short Answer (5 points)**  
Why does the guide caution against using JSON for long context document delimiters?  
**Answer**: JSON performs poorly in long context testing due to its verbosity and need for character escaping, which adds overhead and reduces clarity.

## Quiz 2: ReAct & Automatic Prompt Engineering

**Instructions**  
This quiz tests your understanding of ReAct (Reason and Act) prompting and Automatic Prompt Engineering (APE) from the *Prompt Engineering Whitepaper* (pages 37-41).  
- Questions cover ReAct’s thought-action loop, integration with external tools, APE’s process, and real-world applications.  
- **Total: 100 points**

### Section 1: ReAct (Reason and Act) Prompting (50 points)

**Multiple Choice (10 points)**  
What is the primary mechanism by which ReAct prompting enables LLMs to solve complex tasks?  
a) Generating multiple prompts simultaneously  
b) Combining reasoning and acting in a thought-action loop  
c) Using high-temperature settings to increase randomness  
d) Relying solely on internal model knowledge  
**Answer**: b) Combining reasoning and acting in a thought-action loop  

**Scenario-Based (20 points)**  
You’re building a ReAct agent using the LangChain framework with Gemini-pro to answer customer queries about a band merchandise store. A customer asks: “How many band members are in Metallica, and how many kids do they have in total?” The agent fails to use external tools and provides an incorrect answer. How would you modify the setup to ensure the agent uses external tools (e.g., SerpAPI) correctly? Provide a specific code snippet or prompt adjustment and explain why it helps.  
**Answer**:  
Modify the agent setup:  
```python
from langchain.agents import load_tools, initialize_agent, AgentType
from langchain.llms import VertexAI

prompt = """
You are a ReAct agent tasked with answering queries about band merchandise. For factual questions requiring external data (e.g., band member details), use the SerpAPI tool to search for accurate information. Reason step-by-step and perform actions to retrieve data before answering.
Query: How many band members are in Metallica, and how many kids do they have in total?
"""
llm = VertexAI(temperature=0.1)
tools = load_tools(["serpapi"], llm=llm)
agent = initialize_agent(tools, llm, agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, verbose=True)
agent.run(prompt)
```  
**Explanation**: The whitepaper highlights ReAct’s integration with external tools like SerpAPI. Explicitly loading the tool and instructing the model to use it ensures accurate data retrieval, as shown in Snippet 2. Verbose mode aids debugging.

**Short Answer (10 points)**  
How does ReAct prompting mimic human problem-solving, according to the whitepaper?  
**Answer**: ReAct mimics human problem-solving by combining verbal reasoning with actions (e.g., querying APIs like SerpAPI), generating a plan, performing actions, observing results, and updating reasoning iteratively.

**Short Answer (10 points)**  
Why does ReAct prompting require resending previous prompts/responses and trimming extra content?  
**Answer**: ReAct requires resending prompts/responses to maintain context in the thought-action loop. Trimming extra content prevents irrelevant tokens from inflating costs and ensures focus on relevant information.

### Section 2: Automatic Prompt Engineering (APE) (40 points)

**Multiple Choice (10 points)**  
What is the primary benefit of Automatic Prompt Engineering (APE) as described in the whitepaper?  
a) It eliminates the need for external tools  
b) It reduces the need for human input and enhances model performance  
c) It guarantees deterministic outputs  
d) It simplifies model training  
**Answer**: b) It reduces the need for human input and enhances model performance  

**Scenario-Based (20 points)**  
You’re developing a chatbot for a band merchandise t-shirt webshop and want to use APE to generate diverse customer order phrasings (e.g., “One Metallica t-shirt size S”). The generated prompts are inconsistent in structure, causing misinterpretation. How would you improve the APE process to ensure consistency? Provide a specific prompt or process adjustment and explain its benefit.  
**Answer**:  
Adjust the APE prompt:  
```
Prompt:
We have a band merchandise t-shirt webshop, and to train a chatbot, we need various ways to order: "One Metallica t-shirt size S". Generate 10 variants with the same semantics, formatted as a JSON list.
Schema:
```json
[
  {"order": "string", "size": "S", "item": "Metallica t-shirt", "quantity": 1}
]
```
Example Output:
```json
[
  {"order": "I’d like to purchase a Metallica t-shirt in size small", "size": "S", "item": "Metallica t-shirt", "quantity": 1},
  {"order": "Can I order a small-sized Metallica t-shirt?", "size": "S", "item": "Metallica t-shirt", "quantity": 1},
  ...
]
```
```  
**Explanation**: The whitepaper emphasizes APE’s generation, evaluation, and refinement steps. A JSON schema ensures consistent structure, reducing misinterpretation. Evaluating with BLEU or ROUGE ensures high-quality prompts.

**Short Answer (10 points)**  
What are the three steps of the APE process outlined in the whitepaper?  
**Answer**:  
1. Prompt the model to generate multiple prompt variants.  
2. Evaluate candidates using a metric (e.g., BLEU or ROUGE).  
3. Select the highest-scoring prompt, optionally tweaking and re-evaluating.

### Section 3: General Application (10 points)

**Short Answer (10 points)**  
How can the ReAct prompting example in the whitepaper (Snippet 1 and 2) be adapted for checking stock availability for a merchandise store? Provide a brief description of the adaptation.  
**Answer**:  
Adapt the ReAct agent to query a stock database API:  
```
You are a ReAct agent for a band merchandise store. For queries about stock availability (e.g., 'Is a Metallica t-shirt size S in stock?'), reason step-by-step and use the stock_check tool to query the database. Return the stock status.
```  
Replace `load_tools(["serpapi"])` with a `stock_check` tool. The agent reasons, calls the tool, and returns results (e.g., “In stock: 5 Metallica t-shirts size S”), leveraging ReAct’s thought-action loop.

## Quiz Notes

- **Conceptual Focus**: Questions bridge theory and practice, covering agentic workflows, long context, chain-of-thought, ReAct, and APE, with real-world applications like debugging and e-commerce.  
- **Real-World Scenarios**: Scenario-based questions simulate tasks like fixing bugs or building chatbots, aligning with the source documents’ examples.  
- **Sources**:  
  - *GPT-4.1 Prompting Guide*  
  - *Prompt Engineering Whitepaper*  

**Engagement**: Try the quizzes, share your scores, and discuss your favorite prompt engineering techniques in the comments!

