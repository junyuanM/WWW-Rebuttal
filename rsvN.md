

discuss阶段

additional实验结果对于Q1

我们提供这些实验结果，我们在尽全力补充实验，如果有最新进展，我们会在discuss阶段补充



Dear Reviewer rsvN：

We sincerely appreciate the time and effort that the reviewers have put into evaluating our manuscript. Their insightful comments and suggestions have greatly helped to improve the quality of our work.Below, we provide detailed responses to each comment, highlighting the revisions we have made to address the reviewers' concerns.

---

><strong>Q1</strong>: The overall quality of the manuscript is suboptimal, due to the numerous typos and weak conceptual clarity. A thorough revision is recommended to refine its structure, coherence, and professionalism.

- **Typos**: We have carefully reviewed and corrected all spelling errors to ensure accuracy throughout the manuscript.
- **Conceptual Clarity**: We have strengthened the explanation of key concepts, simplifying any ambiguous sections to make the arguments clearer and more coherent.
- **Structure and Coherence**: We have reorganized parts of the manuscript to improve the overall structure, ensuring better logical flow and readability.

> <strong>Q2</strong>: The authors formalize several typical interaction topologies, however, what are the actual corresponding scenarios for these interactions? Moreover, the Agent interaction scenarios proposed by the authors are not supported by concrete related work. i.e., are the authors' abstract Agent topologies representative of existing applications?



### [Evaluating multi-agent coordination abilities in large language models](https://arxiv.org/abs/2310.03903)

### [Psysafe: A comprehensive framework for psychological-based attack, defense, and evaluation of multi-agent system safety](https://arxiv.org/abs/2401.11880)

### [Large language models empowered agent-based modeling and simulation: A survey and perspectives](https://www.nature.com/articles/s41599-024-03611-3)

### [SpeechAgents: Human-Communication Simulation with Multi-Modal Multi-Agent Systems](https://arxiv.org/abs/2401.03945)

### [Large language models empowered agent-based modeling and simulation: A survey and perspectives](https://www.nature.com/articles/s41599-024-03611-3)





> <strong>Q3</strong>: The network topology proposed by the authors appears overly simplistic and abstract, as reflected in the relationship diagram presented in the introduction and the methodology framework(Figure 1), which involves at most a system with three interacting agents. However, in their experiments for RQ3, the authors set up systems with up to 10 agents. What was the specific MAS configuration used in these experiments?



> 由于现阶段mulit-agent system的研究处于早期，现有的高引用量的文章涉及的agent数量均不超过10个(cite)。因为其一，现有的multi-agent的现实应用中只需要不超过10个agent就可以很好的完成目标任务；其二是太多的agent数量会导致token消耗急剧增加，从而开销非常大。
>
> 我们新增了实验关于链状、环状、完全图等等拓扑架构....实验结果如下：
>
> 我们的agent使用的api包括Llama、gpt-4o等等。prompt
>
> 节点数，模型样式，拓扑结构，在原文中新增了这个章节

实际上现有的Chain和Complete Graph的拓扑结构是对MAS领域较有影响力的Camel[1]和Multi-agent Debate[2]所用的结构的安全性realistic的探究(区别仅在于是否存在信息分层和memory数据结构的探索)

> 

We acknowledge that the abstracts could be more concise. In response to your comment, we will revise the language to streamline the introduction of **TBA** and **MBA**, ensuring that the **core motivation** and **key contributions** of the study are highlighted more effectively and succinctly.

We appreciate your suggestions and will ensure that the abstract better reflects the main focus of our work.

> <strong>Q5</strong>:There are problems with the formatting of section headings and space allocation in the author's manuscript, where the author presents extensively on Attacks in MAS, but lacks space on AgentSafe.

Thank you for your feedback.

Proposing attacks in MAS is one of our main contributions. However, as you suggested, we will revise the structure to allocate more space to the **AgentSafe** section, including more implementation details of **ThreatSieve** and **HierarCache**.

> <strong>Q6</strong>:In Table 1, the table caption refers to ‘over 10 interaction turns’, but the table content shows data from Turn 5 to Turn 50, not "over 10 interaction turns". Therefore, there is a real contradiction between the description of the title and the table content.

We appreciate your attention to this detail.The caption for **Table 1** has been revised to more accurately reflect the data presented. Instead of stating “over 10 interaction turns,” we now specify “from Turn 5 to Turn 50,” which aligns with the data shown in the table.

> <strong>Q7</strong>:The authors have designed possible attack scenarios against the four topologies (IABT, AM, II, IM), rather than attack methods, and the authors should have listed the corresponding attack methods in these four scenarios.
> <strong>Q8</strong>:The authors use abbreviations to represent attack scenarios without first defining them (IABT, AM, II, IM).

Thank you for your valuable feedback. We apologize for the confusion regarding the terminology. To clarify, the terms "IABT," "AM," "II," and "IM" indeed refer to specific attack methods, not attack scenarios. We mistakenly used these terms without providing their full definitions. In the revised manuscript, we will explicitly define each of these attack methods at their first occurrence, as follows:

- **IABT** stands for Information Acquisition Based on Topology,
- **AM** refers to Authorization Mixup,
- **II** denotes Information Interference,
- **IM** is Identity Manipulation.

Furthermore, we will ensure that the relationship between the attack methods and their corresponding scenarios is clearly explained, in order to avoid any misunderstandings. Thank you for bringing this to our attention.

> <strong>Q9</strong>:In Figure 4, why did the authors choose to use a radar chart instead of a line chart to represent the relationship between the Cosine Similarity Rate (CSR) and the number of agents? A line chart would be more suitable for illustrating the continuous nature of these relations.

Thank you for your suggestion.

1. **Choice of Radar Chart**: We initially chose the radar chart to represent CSR and the number of agents because it effectively highlights variations across multiple conditions. It allows us to capture different perspectives in a multi-agent system.
2. **Line Chart Consideration**: We agree that a line chart would better show the continuous nature of the relationship between CSR and the number of agents. It would clearly illustrate the trend.
3. **Revision Plan**: Based on your feedback, we will consider adding a line chart or adjusting the figure to better reflect the continuous trend in the revised manuscript.

Thank you for helping improve the clarity of our presentation.

> <strong>Q10</strong>:In Figure 5, the author did not provide the main caption for the picture.
>
> <strong>Q11</strong>The author did not handle the image link correctly in Obs①, citing ‘Figure ??’ (page 7).

Thank you for pointing this out. We apologize for the missing main caption in Figure 5 and the incorrect reference to "Figure ??" in Obs①. We will **add the correct caption and fix the figure reference** in the revised version to ensure accuracy.We appreciate your valuable feedback and will make the necessary corrections.

> <strong>Q12</strong>:The **Case Study** is somewhat confusing and lacks representativeness. The statement, *“Compared to systems without the AgentSafe, the token count in the hierarchical memory under AgentSafe is significantly reduced,”* raises the question: Is there any direct comparison being made here?



抱歉这段的内容让您感到困惑，左边和右边的图分别表示在TBA和MBA攻击下有无agentsafe时token的消耗数。因此comparison分别发生在两张图中，例如在左图中，我们发现有了agentsafe之后，各个层级的token数量明显减少，又因为junk memory的数据不会出现在提供给api的对话历史中，这样对于api token的消耗数就会明显的减少。基于此，由于对话历史中少了很多无关或者有害的信息，LLM回答内容的准确性和性能也因此提高。

