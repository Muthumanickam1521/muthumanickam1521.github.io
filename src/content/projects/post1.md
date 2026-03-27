---
title: "Demo Post 1"
description: "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
pubDate: "Sep 10 2022"
heroImage: "/post_img.webp"
tags: ["tokio"]
---
# Problem

Many people struggle to understand and verify their financial documents (credit card bills) due to complex formats and manual effort. This project aims to build a RAG-based QA assistant that lets users upload financial documents and query them naturally for instant clarity.

---

# Experience the Assistant

[Note: The Streamlit app is currently working as intended. However, please be aware that occasional errors may occur due to factors like limited resources, connectivity, or package-dependency. To have an idea about how this project helps I have attached a demo video. Thanks for understanding. ](https://rag-app-gemini2-flash.streamlit.app/?embed=true)

Note: The Streamlit app is currently working as intended. However, please be aware that occasional errors may occur due to factors like limited resources, connectivity, or package-dependency. To have an idea about how this project helps I have attached a demo video. Thanks for understanding. 

**GitHub**: https://github.com/Muthumanickam1521/RAG-QA-Assistant

---

# Technical Blog

> **Do checkout this blog to know more about**:
> 
> 
> [From Data Chaos to Calm: A RAG QA Assistant with LlamaIndex + Gemini 2.0 Flash](https://medium.com/@pearlrubymv/from-data-chaos-to-calm-a-rag-qa-assistant-with-llamaindex-gemini-2-0-flash-ac0a709ba2eb)
> 

---

# Demo

[attachment:bbb4c13c-d1e6-40cc-9204-7451bb2f5f43:Streamlit.mp4](attachment:bbb4c13c-d1e6-40cc-9204-7451bb2f5f43:Streamlit.mp4)

**Document used:** 

[Sample-Credit-Card-Document.pdf](attachment:598f0551-7c7b-47a2-a4d0-830e142e131c:Sample-Credit-Card-Document.pdf)

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