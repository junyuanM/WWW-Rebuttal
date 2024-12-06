## Reviewer rsvN

Dear Reviewer rsvN：

We sincerely appreciate the time and effort that the reviewers have put into evaluating our manuscript. Their insightful comments and suggestions have greatly helped to improve the quality of our work.Below, we provide detailed responses to each comment, highlighting the revisions we have made to address the reviewers' concerns.

---

><font color=FireBrick>**Question 1**</font>: The overall quality of the manuscript is suboptimal, due to the numerous typos and weak conceptual clarity. A thorough revision is recommended to refine its structure, coherence, and professionalism.

- **Typos**: We have carefully reviewed and corrected all spelling errors to ensure accuracy throughout the manuscript.
- **Conceptual Clarity**: We have strengthened the explanation of key concepts, simplifying any ambiguous sections to make the arguments clearer and more coherent.
- **Structure and Coherence**: We have reorganized parts of the manuscript to improve the overall structure, ensuring better logical flow and readability.

> <font color=FireBrick>**Question 2**</font>: The authors formalize several typical interaction topologies, however, what are the actual corresponding scenarios for these interactions? Moreover, the Agent interaction scenarios proposed by the authors are not supported by concrete related work. i.e., are the authors' abstract Agent topologies representative of existing applications?

1. **Early Stage of MAS Development**  
   The development of Multi-Agent Systems (MAS) is still in its early stages. Currently, most practical interaction scenarios are centered around large-scale AI projects, such as CAMEL [1], Multi-Agent Debate [2], and LangChain, among others.

2. **Emerging Applications of MAS**  
   In addition, MAS applications are emerging across various industries. For example, ChatDev [3] simulates a technology company to develop code, and Agent Hospital [4] simulates a fully AI-driven hospital to assist patients. In these applications, agents perform specific roles and collaborate to accomplish tasks.

3. **Lack of Layered Information Management**  
   However, these systems lack a layered information management framework. This means that sensitive information from higher-level decision-makers, such as company executives or hospital administrators, may inadvertently leak through lower-level agents.

4. **Security Vulnerabilities**  
   Attackers could exploit this vulnerability, injecting harmful information into critical agents, such as those in charge of diagnosing patients in a hospital, leading to serious errors in diagnosis.

> <font color=FireBrick>**Question 3**</font>: The network topology proposed by the authors appears overly simplistic and abstract, as reflected in the relationship diagram presented in the introduction and the methodology framework(Figure 1), which involves at most a system with three interacting agents. However, in their experiments for RQ3, the authors set up systems with up to 10 agents. What was the specific MAS configuration used in these experiments?

As research on multi-agent systems (MAS) is still in its early stages, most high-citation papers involve fewer than 10 agents (cite). This is primarily due to two reasons:  

1. **Practical Requirements**: In existing MAS applications, fewer than 10 agents are typically sufficient to complete the desired tasks effectively.  
2. **Token Consumption**: Increasing the number of agents significantly results in a sharp rise in token consumption, leading to high computational overhead.

**Agent Connectivity and Topology**. In our experiments, the connectivity between agents is random, which includes various topologies such as chains, rings, and complete graphs. We hypothesize that our defense mechanism should perform consistently across different topologies, so we did not differentiate between them in our experiments. Instead, we used a random structure to represent the connections.

**Impact of Topology on Defense Performance**. We address your concern about topology in a follow-up experiment, which will show that topology does not have a significant impact on defense performance.

**Diversity in Agent Connectivity**. The diversity in agent connectivity accurately reflects the defense performance across different MAS architectures, such as Came [1]l and AutoGPT. The agents in our experiments use APIs like Llama, GPT-4, and others.

In fact, the existing Chain and Complete Graph topologies are a realistic exploration of the security of the structures used in influential MAS research, such as Camel [1] and Multi-agent Debate [2]. The only difference lies in the system prompt used for the agents.

> <font color=FireBrick>**Question 4**</font>: The abstracts are quite lengthy (especially the section introducing the TBA and MBA), could the language be further optimized to further emphasize the core motivation and core contribution of the study?

We acknowledge that the abstracts could be more concise. In response to your comment, we will revise the language to streamline the introduction of **TBA** and **MBA**, ensuring that the **core motivation** and **key contributions** of the study are highlighted more effectively and succinctly.

> <font color=FireBrick>**Question 5**</font>: There are problems with the formatting of section headings and space allocation in the author's manuscript, where the author presents extensively on Attacks in MAS, but lacks space on AgentSafe.

Thank you for your feedback.

Proposing attacks in MAS is one of our main contributions. However, as you suggested, we will revise the structure to allocate more space to the **AgentSafe** section, including more implementation details of **ThreatSieve** and **HierarCache**.

> <font color=FireBrick>**Question 6**</font>: In Table 1, the table caption refers to ‘over 10 interaction turns’, but the table content shows data from Turn 5 to Turn 50, not "over 10 interaction turns". Therefore, there is a real contradiction between the description of the title and the table content.

We appreciate your attention to this detail.The caption for **Table 1** has been revised to more accurately reflect the data presented. Instead of stating “over 10 interaction turns,” we now specify “from Turn 5 to Turn 50,” which aligns with the data shown in the table.

> <font color=FireBrick>**Question 7**</font>: The authors have designed possible attack scenarios against the four topologies (IABT, AM, II, IM), rather than attack methods, and the authors should have listed the corresponding attack methods in these four scenarios.
> <font color=FireBrick>**Question 8**</font>: The authors use abbreviations to represent attack scenarios without first defining them (IABT, AM, II, IM).

Thank you for your valuable feedback. We apologize for the confusion regarding the terminology. To clarify, the terms "IABT," "AM," "II," and "IM" **indeed refer to specific attack methods**, not attack scenarios. We mistakenly used these terms without providing their full definitions. In the revised manuscript, we will explicitly define each of these attack methods at their first occurrence, as follows:

- **IABT** stands for Information Acquisition Based on Topology attack,
- **AM** refers to Authorization Mixup attack,
- **II** denotes Information Interference attack,
- **IM** is Identity Manipulation attack.

Furthermore, we will ensure that the relationship between the attack methods and their corresponding scenarios is clearly explained, in order to avoid any misunderstandings. Thank you for bringing this to our attention.

> <font color=FireBrick>**Question 9**</font>: In Figure 4, why did the authors choose to use a radar chart instead of a line chart to represent the relationship between the Cosine Similarity Rate (CSR) and the number of agents? A line chart would be more suitable for illustrating the continuous nature of these relations.

Thank you for your suggestion.

1. **Choice of Radar Chart**: We initially chose the radar chart to represent CSR and the number of agents because it effectively highlights variations across multiple conditions. It allows us to capture different perspectives in a multi-agent system.
2. **Line Chart Consideration**: We agree that a line chart would better show the continuous nature of the relationship between CSR and the number of agents. It would clearly illustrate the trend.
3. **Revision Plan**: Based on your feedback, we will consider adding a line chart or adjusting the figure to better reflect the continuous trend in the revised manuscript.

Thank you for helping improve the clarity of our presentation.

> <font color=FireBrick>**Question 10**</font>: In Figure 5, the author did not provide the main caption for the picture.
>
> <font color=FireBrick>**Question 11**</font>: The author did not handle the image link correctly in Obs①, citing ‘Figure ??’ (page 7).

Thank you for pointing this out. We apologize for the missing main caption in Figure 5 and the incorrect reference to "Figure ??" in Obs①. We will **add the correct caption and fix the figure reference** in the revised version to ensure accuracy.We appreciate your valuable feedback and will make the necessary corrections.

> <font color=FireBrick>**Question 12**</font>: The **Case Study** is somewhat confusing and lacks representativeness. The statement, *“Compared to systems without the AgentSafe, the token count in the hierarchical memory under AgentSafe is significantly reduced,”* raises the question: Is there any direct comparison being made here?

We apologize for any confusion caused by this section. Below is a clarification:

1. **Figure Explanation**:
   The left and right figures represent token consumption under TBA and MBA attacks, respectively, both with and without AgentSafe. The comparisons are conducted within each figure.
2. **Token Consumption Reduction**:
   In the left figure, we observe that with AgentSafe, token consumption at each level is significantly reduced. This occurs because junk memory data is excluded from the conversation history provided to the API, thereby lowering the overall token usage.
3. **Improved LLM Performance**:
   By removing irrelevant or harmful information from the conversation history, the accuracy and performance of the LLM’s responses are also enhanced.

---

[1] CamelL: Communicative Agents for "Mind" Exploration of Large Language Model Society

[2] ChatEval: Towards Better LLM-based Evaluators through Multi-Agent Debate

[3] ChatDev: Communicative Agents for Software Development

[4] Agent Hospital: A Simulacrum of Hospital with Evolvable Medical Agents

Sincerely,

Authors

