---
title: "From Data Chaos to Calm: A RAG QA Assistant with LlamaIndex + Gemini 2.0 Flash"
description: ""
pubDate: 2025-04-25
heroImage: "/project1_banner.png"
tags: ["Natural Language Processing 🔡", "LLM 🧠", "Retrieval Augmented Generation 📄⛏️"]
---

Inthis modern, people tend to work for longer hours. Sometimes they may not get time to go through all bills that received in the previous months. As in my case, whenever I get my monthly credit card bill statements, I think like “Oh! again, how many transactions have i made last month in food? How much cashback I received for this month from swiggy?”. This is where one can leverage the power of LLMs like ChatGPT-4o, Gemini-2.0, etc.

Imagine, you have a smart virtual assistant that helps with understanding about your monthly statements. It's a sound a bit of relieve, right? yes? Then this blog post is just written for you.

Heard about fine-tuning an LLM? If yes, then we are not going to be fine-tuning any LLM because of the following genuine reasons:

1) Our bill statements change every month. And it is not feasible idea to finetune an LLM every month.

2) If you are okay with yearly statements, still if you wish to update LLM’s knowledge with those documents, think about the fine-tuning resource cost that is billed.

![image1.png](/blog1_image1.png)

By looking at the above illustration, you might understand how does fine-tuning affect text it generated. Text generated from pre-trained model (also known as base model) does not help in generating meaningful and relevant response.

Alternative to LLM fine-tuning is RAG (Retrieval Augmented Generation) based approach such as VectorRAG, GraphRAG, etc. It is not only pocket friendly but does the job too. If in case, cost is not issue to you and you may need to use in longer run, fine-tuning an LLM is the ideal choice as it completely understands your document and can answer questions related to it with accurate response.

#### Motivation
Have you ever struggled to make sense of your credit card bill? I have — I used to manually organize every transaction in Excel just to verify that the incurred charges are correct or not. Often, I wished I had a friend who’s a chartered accountant to help decode these statements. Since I didn’t have one, I developed a RAG-based QA assistant using LlamaIndex and Gemini 2.0 Flash, enabling users to query their documents effortlessly.​

In simple terms, this RAG application lets you upload a document (TXT, Excel, CSV, etc.) and ask questions related to the document uploaded. Behind the scenes, it parses the document, retrieves the most relevant context, and generates accurate, concise answers using a powerful large language model.

![image2.png](/blog1_image2.png)

#### Technical Implementation

The following are the technical stack I used in developing the application for my personal use cases. Reminder, this is a hobby project which is not production-ready, but this system can be scaled given enough time and resources.

* Frontend: Streamlit
* Backend: Python
* Frameworks and others: LlamaIndex, Gemini 2.0 Flash
* Deployment: Steamlit Cloud


#### Architecture
To address the pain point, here’s the high-level workflow of my RAG QA:

![image3.png](/blog1_image3.png)

`Data Loading`: Data is seen in various file formats from multiple sources such as book, document, webpage, excel sheet, image, etc. User uploads data manually. As part of indexing, all documents are converted to pure text pieces as text is the input to our LLM.

`Data Indexing`: You might think like “why can’t we just give all text from the document as input to my LLM?”. It’s absolutely to think this way. The problem is LLM might miss important information that is relevant to the query. Here’s what do: 1) make big text into smaller ones by using appropriate chunking strategies 2) Convert text pieces into number using appropriate embedding model 3) Store them into a vector database (vDB). Now, information from the document is ready to be used whenever needed. Remember this process (indexing) is done one-time only.

`Data Retrieval`: Now we have our information from the document stored in a vector database such as Pinecone, FAISS, ChromaDB, etc. In my project, I utilised Pinecone DB for indexing and retrieving. This phase in RAG involves obtaining information that is relevant to the query from DB. I used top-k retrieving strategy to collect top matching text pieces. If k is lower, we might loose some important information. Higher value to k, leads to getting noisy information. It’s highly experimental to find the optimal k.


`Text Generation`: The final phase in RAG where one has to structure their query and external information along with LLM instructions in a prompt template. So that LLM generates highly meaningful and useful response.


#### Have a Question About Your File?
To use the application, you’ll need:
* A document (.txt, .csv, .xlsx, .docx, .pdf)
* A question

Instructions:
* Access the Streamlit app.
* Upload your file (please wait a few seconds for processing).
* Ask your question!

#### Improvement Made
* I noticed that the assistant bot hallucinates by providing confident irrelevant response. I decreased bad relevant response by improving prompt template, reducing break-point percentile threshold in indexing (from 0.95 to 0.9), and increasing k value in retrieving.


#### Limitations
Having built a core, here are what still need care:
1. Currently handles only one file at a time.
2. Complex document parsing such as document containing images, tables, footer, header, and diagram.
3. Does not process unstructured data beyond supported file types.
4. Generates text-based responses only.
5. PDF parsing accuracy may vary.
Note: The Streamlit app is currently working as intended. However, please be aware that occasional errors may occur due to factors like limited resources, connectivity, or package-dependency.

#### Future Versions

I plan to address these limitations as I get less time to invest on hobby projects and develop a more robust RAG application with the following enhancements:

1. Agent Orchestration: Implement a team of specialized agents working collaboratively to accomplish complex tasks and goals.
2. Multi-Document Handling: Enable the system to process and answer questions from multiple 
uploaded documents simultaneously and seamlessly.
3. Enhanced Indexing and Retrieval: Integrate semantic-based indexing and retrieval mechanisms for improved accuracy and efficiency.
4. Multimedia Support: Expand the system to handle and generate responses based on images and other multimedia content.
5. Improved PDF Parsing: implement a more robust and accurate PDF parser.
6. User Authentication: Add user authentication to improve data security.
7. API Integration: Create an API for integration into other applications.

#### Final Note
This project exemplifies my ability to integrate advanced NLP frameworks to solve real-world problems. I’m eager to apply these skills in a professional setting, contributing to innovative solutions in the field of AI and machine learning.

Thank you reading till the end. Have a bright day ahead.
