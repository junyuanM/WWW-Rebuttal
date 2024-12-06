Dear Reviewer G9px:

Thank you for your thoughtful and constructive feedback. We greatly appreciate the time and effort you have invested in reviewing our work. Below, we provide responses to your concerns and suggestions, along with the revisions we have made to address them.

> <font color=FireBrick>**Weakness 1**</font>: The relevance of LLMs and MAS to Web security and privacy is unclear to me.

We believe that MAS and the Web have a clear correlation, because each node in MAS can be understood as an entity in the Web. The security challenges in multi-agent systems are similar to those in Web systems, but with the addition of collaboration and information sharing between agents. The introduction of LLM may change the form of security threats.

- **LLMs in Web Security and Privacy**: LLMs can analyze large amounts of data to detect vulnerabilities, identify threats, and provide real-time responses, making them valuable tools for enhancing Web security.
- **MAS in Web Security and Privacy**: In a Multi-Agent System, multiple agents (including LLMs) can collaborate to model attack scenarios, detect adversarial behavior, and coordinate defense mechanisms, offering new ways to manage security and privacy in decentralized Web environments.

> <font color=FireBrick>**Weakness 2**</font>: Authors omitted any discussions about the details of their system with regards to implementation. 

To make it easier for readers to understand, we have included specific details in the article including:

- For a **clearer formulation**, we use the term threatsieve and hiarcache in this article to represent... At the same time, we optimized the... (formula) and added the... **notation table** in the appendix.
- The **technical details are clearer** (the first work to consider MAS layering; the first systematic consideration of memory security issues and security frameworks; threatsieve uses..., junk memory...)

> <font color=FireBrick>**Weakness 3**</font>: I didn't find the value in most of the formal definitions in the paper.

We found it **necessary to give formula definitions** in the introduction. These concepts are common in the graph(LTH [1], Snowflake [2], GPTSwarm [3], OpenGraph [4], DHGR [5]).We believe that the expression of formulas is more convenient for subsequent work and helps readers follow our research. At the same time, we agree that the formulas in the introduction are too bloated. We have deleted the formulas, but the formulas in the main text are meaningful.


> <font color=FireBrick>**Weakness 4**</font>: The space in the paper could have been used for topics such as details of the system, or the discussion of system components that is currently in the Appendix

We will add ... in the appendix

> <font color=FireBrick>**Weakness 5**</font>: Is ThreatSieve simply authenticating the communication channel between the agents? What does its cryptographic verification and authority validation look in practice? Is it based on an existing protocol? 

1. **Identity Verification in MAS**  
   In our framework, agents communicate in natural language, and the exchanged content is structured, such as in JSON files. Currently, research in Multi-Agent Systems (MAS) is still in its early stages, and experiments typically assume that identity spoofing does not occur. Each agent takes on a specific role, and communication follows a pre-designed process.

2. **Identity Verification Process**  
   To verify identity, we call APIs and extract information from the source agent to determine its identity. However, because the content is in natural language, even if we inform the target agent of an impostor, there remains a risk of misjudgment during task processing by the LLM.

3. **Handling Incorrect Identity**  
   In **AgentSafe**, if an incorrect identity is detected, the related information is immediately treated as junk information. This helps prevent attacks that may succeed due to the non-deterministic behavior of LLMs when processing tasks.

4. **Future Considerations for Server-Based Agents**  
   If each agent is placed on a different server in the future, then the first thing to do for agent information confirmation is to confirm the information between servers, and this is something that needs to be considered at a lower level than MAS.

> <font color=FireBrick>**Weakness 6**</font>: How does Agentsafe characterize junk memory? How does Agentsafe identify this in practice and how are these terms defined in different contexts?

We appreciate your feedback and would like to clarify the process:

1. **Definition of Junk Information**:  
   Junk information refers to harmful content that is irrelevant to the agent’s tasks, including misleading, erroneous, biased, or contradictory information.

2. **Before Information Enters Memory**:  
   We use two methods to assess information before it is stored in memory:

   * **API Call**: A designed prompt, which includes descriptions of different permission levels, helps the LLM determine the correct level of access.
   * **Vector Comparison**: We compare the information against predefined permission levels using vector mappings.

3. **For Information Stored in Memory**:  
   For stored information, we ensure that each interaction is contextually designed for retrieval. Then, we apply the same method of using a prompt to assess the permission level. The main difference here is that this step is more reflective, considering the context of the information (see Camel [6], Reflexion [7]).

> <font color=FireBrick>**Question 1**</font>: What is the effort involved in building Agentsafe and marking data and agent access hierarchy?

- The focus of building agentsafe is the design of the overall architecture and experiments to ensure that each part can run well. We have enriched the expression of methodology in the article.

- The construction of the dataset first determines the task. For example, the task of agentsafe includes basic information between agents and various attack statements. Then, like many articles in the existing LLM agent field [], the generation of the dataset is to first generate a large amount of information or questions through LLM, and then manually review to generate the dataset.

- Regarding the construction of the agent access hierarchy, we built the hierarchical datasets RIOH and WCEI for mas in simulating human interaction [camel] and company scenarios [chatdev]. MAS applications are emerging across various industries. For example, ChatDev [3] simulates a technology company to develop code, and Agent Hospital [4] simulates a fully AI-driven hospital to assist patients. In these applications, agents perform specific roles and collaborate to accomplish tasks. However, these systems lack a layered information management framework. This means that sensitive information from higher-level decision-makers, such as company executives or hospital administrators, may inadvertently leak through lower-level agents. Therefore, constructing a dataset that is of practical significance is crucial. Our experiments also prove that AgentSafe can provide effective defense in different fields.


[1] A Unified Lottery Ticket Hypothesis for Graph Neural Networks

[2] The Snowflake Hypothesis: Training and Powering GNN with One Node One Receptive Field

[3] GPTSwarm: Language Agents as Optimizable Graphs

[4] OpenGraph: Towards Open Graph Foundation Models

[5] Make Heterophilic Graphs Better Fit GNN: A Graph Rewiring Approach

[6] CAMEL: Communicative Agents for “Mind” Exploration of Large Language Model Society

[7] Reflexion: Language Agents with Verbal Reinforcement Learning

Warm regards,

Authors

