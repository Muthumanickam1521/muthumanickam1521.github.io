---
title: "RAG QA Assistant with LlamaIndex and Gemini 2.0 Flash"
description: ""
pubDate: 2025-04-20
heroImage: "/project1_banner.png"
tags: ["LlamaIndex", "LangChain", "Vector DB", "Pinecone", "LLM", "Gemini Models", "Streamlit"]
---


# Problem

Many people struggle to understand and verify their financial documents (credit card bills) due to complex formats and manual effort. This project aims to build a RAG-based QA assistant that lets users upload financial documents and query them naturally for instant clarity.


#### Demo
<video width="600" controls>
  <source src="/project1_demo.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

#### Document 
<object data="/project1_doc.pdf" type="application/pdf" width="100%" height="600">
  <p>PDF cannot be displayed. <a href="/project1_doc.pdf">Download</a></p>
</object>

**GitHub**: [View Repository](https://github.com/Muthumanickam1521/RAG-QA-Assistant) 

**Technical Blog**: [View Article](https://medium.com/data-and-beyond/from-data-chaos-to-calm-a-rag-qa-assistant-with-llamaindex-gemini-2-0-flash-ac0a709ba2eb)


---

# Result

1. Everyday usable chatbot assistant that takes care questions about your financial documents.
2. Improved the question-answer time from **30 to 20 seconds** by adopting strategies of indexing and structuring code logic.


---

# Challenges

- Use of LLM’s intrinsic knowledge
    - **Challenge:** Observed that LLM hallucinated confidently when I asked a query.
    - **Solution:** Performed the following
        - increased **break_point_threshold** parameter from **0.80** to **0.9** to make split more rigid.
        - increased **top-k** value from **3** to **20** to increase the chance of retrieving relevant information
- Response tone
    - **Challenge**: Generated text tone was likely needed improvement.
    - **Solution**: Adjusted the prompt instructions to get response like a finance professional.  Sometimes simple prompting helps in obtaining desired text.
- Token usage
    - **Challenge**: Standard responses like “Based on the information...” are indeed not necessary at least in this QA chatbot.
    - **Solution**: Provided prompt instructions on token usage. Now it provides bullet-tip answers by cutting out extra tokens. By doing this, we get short and crisp answers with less input tokens which cuts cost.


---

# Future Version

1. Scale upto 100s of docs (refer this implemention ideas from LlamaIndex: https://docs.llamaindex.ai/en/stable/optimizing/production_rag/)
2. More documents (**>2**)
3. Reduction in response latency

---