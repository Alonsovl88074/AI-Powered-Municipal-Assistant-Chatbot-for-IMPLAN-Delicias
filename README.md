# AI-Powered Municipal Assistant: Chatbot for IMPLAN Delicias

<p align="center">
  <img src="ImplaniBot.png" alt="Implani Bot Logo" width="200"/>
</p>

This repository contains the source code and documentation for an intelligent virtual assistant developed for the Municipal Planning Institute (IMPLAN) of Delicias, Chihuahua. This project is more than a chatbot; it is a full-fledged Business Intelligence solution designed to automate public information access, enhance citizen engagement, and optimize internal resources.

<p align="center">
  <img src="Inicio-IMPLAN-Personal_-Microsoft_-Edge-2025-08-15-15-20-35.gif" alt="Chatbot Demo GIF" width="80%"/>
</p>

> *A live demonstration of the chatbot in action on the official IMPLAN website. For try the ChatBot visit [(http://implandeliciaschih.ddns.net/)]*

---

## Project Vision: Beyond Code, a BI Solution

As a Senior Business Intelligence professional, my primary goal was not merely to build an application, but to solve a core business problem: **bridging the gap between the vast amount of municipal planning data and its accessibility to the average citizen.**

The challenge was to transform complex, unstructured documents—such as development plans, official gazettes, and risk analyses—into instant, comprehensible answers available 24/7. This chatbot serves as a direct data pipeline from IMPLAN's knowledge base to the community's questions, turning static information into a dynamic, interactive service.

---

## Technical Architecture

To deliver a robust, scalable, and real-time solution, I designed and implemented a three-tiered architecture that decouples presentation, logic, and intelligence. This modular approach ensures easy maintenance and future scalability.

graph TD
    A[Frontend: Vanilla JS on IMPLAN Website] -- "User Query (POST Request)" --> B{Backend: Python/Flask API};
    B -- "Enrich Query + Context" --> C[Intelligence Layer: Google Gemini API];
    C -- "Generated Response" --> B;
    B -- "Formatted Answer (JSON)" --> A;
    
**Frontend (UI):** A clean, responsive chat interface built with HTML5, CSS3, and Vanilla JavaScript. Asynchronous communication with the backend is handled via the Fetch API, ensuring a seamless user experience.

**Backend (Logic Core):** A lightweight yet powerful microservice developed in Python with the Flask framework. This API is the operational heart, orchestrating the entire request-response cycle.

**Intelligence Layer (External API):** The Google Gemini Pro model, integrated via its API. I employed a Retrieval-Augmented Generation (RAG) architecture to ensure all responses are grounded in factual, provided data, mitigating AI "hallucinations."

## My Role & Development Process: A Full-Stack Approach
I managed this project end-to-end, from data engineering to final deployment, applying a wide spectrum of technical skills.

1. Knowledge Base Engineering (The BI Perspective)
The foundation of any intelligent system is its data. I executed a classic ETL process to build the chatbot's brain:

**Extract:** Pulled unstructured text from the IMPLAN website, internal HTML pages, and linked PDF documents.

**Transform:** Structured the extracted information into a coherent JSON format, creating a domain-specific knowledge base.

**Load:** This JSON context is loaded at runtime and injected into the AI's prompt for every query.

<details>
## <summary>► Click to see an example of the structured Knowledge Base</summary>
code
{
  "about": "El Instituto Municipal de Planeación (IMPLAN) de Delicias tiene por objeto fortalecer la planeación participativa estratégica...",
  
  "contact": {
  
    "address": "Calle Cuarta Norte N°8, Colonia Centro, Delicias, Chihuahua...",
    "email": "implan@delicias.gob.mx",
    "phone": "639 6882650"
  },
  "documents_and_services": [
  
    {
      "title": "Plan Municipal de Desarrollo de Delicias 24-27",
      "href": "https://chihuahua.gob.mx/...",
      "summary": "Este es el documento que rige el desarrollo municipal para el periodo 2024-2027..."
    }
  ]
}
</details>

## 2. Backend & API Development
I built the /chat endpoint using Flask, focusing on robustness and clarity. The Python logic does more than just proxy requests; it enriches the user's query with the knowledge base context, demonstrating my ability to design and build purposeful APIs.
<details>
  
<summary>► Click to see a snippet of the Flask backend code</summary>
code
Python
# chatbot_backend.py
import google.generativeai as genai
from flask import Flask, request, jsonify

app = Flask(__name__)

The system prompt instructs the AI on its personality and constraints
system_prompt = f"""

Eres un asistente virtual amable y servicial del IMPLAN de Delicias, Chihuahua.
Tu nombre es 'Planito'. Responde únicamente basándote en la siguiente información:
{knowledge_base_json}
"""
model = genai.GenerativeModel(model_name="gemini-2.5-pro")

@app.route('/chat', methods=['POST'])
def chat():
    try:
        data = request.get_json()
        user_question = data.get("question")

        if not user_question:
            return jsonify({"error": "No question provided."}), 400
        
        # Start a chat session with the system instructions and user's query
        convo = model.start_chat(history=[...]) # History includes the system_prompt
        convo.send_message(user_question)
        
        return jsonify({"answer": convo.last.text})

    except Exception as e:
        print(f"Error in /chat endpoint: {e}")
        return jsonify({"error": "Internal server error."}), 500

if __name__ == '__main__':
    # Listens on all available network interfaces
    app.run(host='10.10.5.0', port=5000, debug=True)
</details>

## 3. Frontend Implementation
I developed the chatbot's UI from scratch for seamless integration. The client-server logic, written in clean Vanilla JavaScript, highlights my competency in frontend development and asynchronous programming.
<details>
<summary>► Click to see a snippet of the frontend JavaScript</summary>
code
JavaScript
// Part of the sendMessage function in the main HTML file

const sendMessage = async () => {
    const question = chatInput.value.trim();
    if (question === '') return;

    // Display user message immediately for a responsive feel
    displayMessage(question, 'user-message');
    chatInput.value = '';

    try {
        // Asynchronously call the backend API endpoint
        const response = await fetch('http://implan-chat.strangled.net:5000/chat', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ question: question })
        });

        if (!response.ok) throw new Error(`Server error: ${response.status}`);

        const data = await response.json();
        // Display the AI's answer received from the backend
        displayMessage(data.answer, 'bot-message');

    } catch (error) {
        console.error("Error contacting the bot:", error);
        displayMessage('Sorry, I am having trouble connecting. Please try again later.', 'bot-message');
    }
};
</details>

## 4. Deployment & Systems Administration
Deploying this solution on a physical, on-premise server presented real-world challenges that I successfully navigated:
Network Configuration: Engineered a resilient solution for a dynamic public IP address by setting up a DDNS service (FreeDNS) and configuring Port Forwarding rules on the network router.
Server Hardening: Secured the server by configuring the Windows Firewall to allow traffic exclusively on the application's port (5000/TCP), ensuring both accessibility and security.
Environment Management: Resolved dependency conflicts and managed the Python environment to ensure the script runs consistently and reliably as a service.
Core Competencies & Tech Stack

## 5. Advanced Architecture: From Monolithic Context to a Vector Database

The initial prototype confirmed the viability of the project but quickly revealed a critical bottleneck: providing the entire knowledge base as context for every API call was inefficient, costly, and severely limited by the API's token quota. To evolve this solution into a scalable, production-ready system, I re-architected the data pipeline using a sophisticated **Vector Search** methodology.

This approach transforms the chatbot from a simple Q&A tool into a powerful semantic search engine.

### 1. Data Ingestion and Chunking Strategy

Raw documents (PDF, CSV, JSON) are too large and unstructured to be efficiently processed by an LLM. The first step was to engineer an automated ingestion pipeline using a dedicated Python script (`indexer.py`).

-   **Multi-Format Document Loading:** The pipeline uses libraries like `PyPDF2` and `Pandas` to extract raw text from a variety of file formats dropped into a designated `knowledge_base` folder.
-   **Intelligent Text Chunking:** I implemented a text splitting strategy using the `langchain` library's `RecursiveCharacterTextSplitter`. Instead of processing entire documents, the text is segmented into smaller, semantically coherent chunks (e.g., paragraphs of 1000 characters with a 150-character overlap). This overlap ensures that the contextual relationship between chunks is preserved.

<details>
<summary>► Click to see a snippet of the Indexing & Chunking logic</summary>

### indexer.py
from langchain.text_splitter import RecursiveCharacterTextSplitter

### Initialize a smart text splitter
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=150,
    length_function=len,
)

### Process each document and fragment its content
def process_and_store_documents(folder_path='knowledge_base'):
    all_chunks = []
    all_metadatas = []
    
    for file in os.listdir(folder_path):
        # ... logic to load raw text from PDF, CSV, etc. ...
        
        if content:
            # The core of the strategy: split large text into small, meaningful chunks
            chunks = text_splitter.split_text(content)
            all_chunks.extend(chunks)
            all_metadatas.extend([{"source": file}] * len(chunks))
    
    # ... logic to add chunks to the vector database ...
</details>
## 6. Embedding Generation and Vector Storage

Once the knowledge is fragmented, it needs to be converted into a machine-readable format. This is achieved through embeddings.
Embedding Model: I leveraged Google's embedding-001 model via the GoogleGenerativeAiEmbeddingFunction. Each text chunk is sent to this model, which returns a high-dimensional vector (a list of numbers) representing its semantic meaning.
Vector Database: The generated vectors, along with their original text content and metadata (like the source document), are stored in a local ChromaDB instance. ChromaDB is a lightweight but powerful open-source vector database that allows for incredibly fast similarity searches. This entire indexing process is executed offline and only needs to be re-run when the source documents are updated.

## 7. Retrieval-Augmented Generation (RAG) in Action
With the knowledge base indexed, the real-time query process becomes highly efficient:
User Query Embedding: When a user sends a question, the backend first uses the same Google embedding model to convert the user's question into a vector.
Semantic Search: This query vector is used to perform a similarity search against the millions of vectors stored in ChromaDB. The database instantly returns the k most semantically relevant text chunks (e.g., the top 7 chunks that are conceptually closest to the user's question).
Dynamic Prompt Engineering: Instead of sending the entire library of documents, I dynamically construct a highly focused prompt. This prompt includes the original user question and is augmented only with the relevant text chunks retrieved from the vector search.
Final Answer Generation: This concise, context-rich prompt is sent to the gemini-1.5-flash model, which then generates a precise and accurate answer based only on the provided information, dramatically improving response quality and speed while staying within API limits.

## 8. My Role & Development Process: Architecting an End-to-End AI Solution

I drove this project from concept to production, acting as the sole architect and full-stack developer. My process was iterative, evolving from a simple prototype to a sophisticated RAG-based system, demonstrating my ability to adapt and scale solutions based on technical requirements and performance feedback.

### 1. Data Engineering for AI

My primary role was to architect the data pipeline that fuels the AI. This went beyond simple ETL:
-   **Strategy:** I identified the limitations of a static JSON context and pivoted to a dynamic, multi-format ingestion pipeline. This strategic shift made the system scalable and easy for non-technical users to update.
-   **Implementation:** I developed a robust Python script (`indexer.py`) that automates the entire data preparation process: reading diverse file types, performing intelligent text chunking with `Langchain`, and handling failures gracefully through batch processing.

### 2. Backend & RAG Orchestration

I designed the Flask backend to be more than a simple API; it is the central orchestrator of the RAG pipeline. For every user query, the backend executes:
-   **Query Embedding:** Converts the user's natural language question into a vector.
-   **Vector Search:** Performs a high-speed semantic search in ChromaDB to retrieve the most relevant context.
-   **Dynamic Prompting:** Constructs a precise, context-aware prompt on the fly, feeding the LLM only the information it needs to formulate an accurate answer.

### 3. Frontend Development

I built the user-facing chat interface from the ground up using **Vanilla JavaScript, HTML5, and CSS3**. The focus was on creating a responsive, intuitive user experience that integrates seamlessly into the existing IMPLAN website. The asynchronous communication via the `Fetch API` ensures that the interface remains fast and interactive while the backend performs the complex AI processing.

### 4. Full-Cycle Deployment & Systems Administration

A key differentiator of this project was its deployment on a physical, on-premise server, which required a comprehensive DevOps skill set:
-   **Infrastructure Management:** Configured the server environment, managed Python dependencies, and set up the application to run as a persistent service.
-   **Network Engineering:** Architected a solution for a dynamic public IP by implementing a **DDNS** service and configuring **Port Forwarding** and **Firewall rules (Windows Defender)** to securely expose the application to the internet.
-   **Maintenance & Automation:** Created an automated workflow where updating the chatbot's knowledge is as simple as adding a file to a folder and re-running the indexing script.
  
## This project is a practical demonstration of my expertise across the full data and development lifecycle.

## Category	Technologies & Skills

**Backend Development**	Python, Flask, RESTful API Design

**Frontend Development**	JavaScript (ES6+), HTML5, CSS3, DOM Manipulation, Fetch API (AJAX)

**Artificial Intelligence**	Google Gemini API, Prompt Engineering, Retrieval-Augmented Generation (RAG)

**Systems & Networking**	Windows Server, Firewall Configuration, Port Forwarding, DDNS, SSH

**BI & Data Engineering**	ETL Processes, Data Structuring (JSON), Solution Architecture, Problem Solving

## Contact
This chatbot is a tangible example of how technology, when applied with a Business Intelligence strategy, can transform data into direct value for an organization and its community. 
I am proud of the result and eager to apply these skills to new and complex challenges.
Feel free to reach out if you'd like to learn more about this project or discuss potential collaborations.

LinkedIn: [https://www.linkedin.com/in/alonso-villalobos-lara-7297641b/](https://www.linkedin.com/in/alonso-villalobos-lara-7297641b/)]
