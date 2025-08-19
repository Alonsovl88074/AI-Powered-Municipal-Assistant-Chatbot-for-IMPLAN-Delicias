# AI-Powered-Municipal-Assistant-Chatbot-for-IMPLAN-Delicias
This repository documents the development of an intelligent virtual assistant for the Municipal Planning Institute (IMPLAN) of Delicias, Chihuahua. This project is more than a chatbot; it is a full-fledged Business Intelligence solution designed to automate public information access, enhance citizen engagement, and optimize internal resources.

![Implani Bot](ImplaniBot)

![Chatbot Demo GIF](Inicio-IMPLAN-Personal_-Microsoft_-Edge-2025-08-15-15-20-35.gif)
> *A quick demonstration of the chatbot in action on the official IMPLAN website.*

---

## Project Vision: Beyond Code, a BI Solution

As a Senior Business Intelligence professional, my primary goal was not merely to build an application, but to solve a core business problem: **bridging the gap between the vast amount of municipal planning data and its accessibility to the average citizen.**

The challenge was to transform complex documents—such as development plans, official gazettes, and risk analyses—into instant, comprehensible answers available 24/7. This chatbot serves as a direct data pipeline from IMPLAN's knowledge base to the community's questions.

---

## Technical Architecture

To deliver a robust, scalable, and real-time solution, I designed a three-tiered architecture that decouples presentation, logic, and intelligence. This modular approach ensures easy maintenance and future scalability.

mermaid
graph TD
    A[Frontend: Vanilla JS on IMPLAN Website] -- "User Query (POST Request)" --> B{Backend: Python/Flask API};
    B -- "Enrich Query + Context" --> C[Intelligence Layer: Google Gemini API];
    C -- "Generated Response" --> B;
    B -- "Formatted Answer (JSON)" --> A;

### Frontend (UI): A clean, responsive chat interface built with HTML5, CSS3, and Vanilla JavaScript. Asynchronous communication with the backend is handled via the Fetch API, ensuring a seamless user experience without page reloads.
### Backend (Logic Core): The heart of the operation. I developed a lightweight yet powerful microservice using Python and the Flask framework. This API endpoint is responsible for:
Securely receiving user queries.
Orchestrating communication with the external AI API.
Processing and returning the final response to the frontend.
### Intelligence Layer (External API): I integrated the Google Gemini Pro model via its API. My approach was based on a Retrieval-Augmented Generation (RAG) architecture. Instead of relying on its general knowledge, the model is instructed to base its answers exclusively on a curated knowledge base provided in real-time. This technique ensures factual accuracy and prevents AI "hallucinations."

## My Role & Development Process: A Full-Stack Approach
Throughout this project, I took on an end-to-end role, from data engineering to final deployment, allowing me to apply and strengthen a broad spectrum of technical skills.

### 1. Knowledge Base Engineering (The BI Perspective)
The first phase was a classic ETL exercise. I extracted unstructured data from various sources (the IMPLAN website, PDF documents, HTML pages) and transformed it into a structured JSON context. This curated Knowledge Base is the cornerstone that guarantees the relevance and veracity of the chatbot's answers.

### 2. Backend & API Development
I built the /chat endpoint using Flask, efficiently handling POST requests. The Python logic does more than just proxy requests; it enriches the user's query with the Knowledge Base context before dispatching it to the AI, showcasing my ability to design and build purposeful, robust APIs.

### 3. Frontend Implementation
I developed the chatbot's UI from scratch, focusing on usability and seamless integration with the existing IMPLAN website design. The client-server implementation using asynchronous JavaScript highlights my competency in frontend development and my understanding of user experience principles.

### 4. Deployment & Systems Administration
Deploying the solution on a physical on-premise server presented real-world challenges that I successfully navigated:
Network Configuration: I engineered a resilient solution for a dynamic public IP address by setting up a DDNS service (FreeDNS) and configuring Port Forwarding rules on the network router.
Server Hardening: I secured the server by configuring the Windows Firewall to allow traffic exclusively on necessary ports (e.g., SSH and the application port), ensuring both accessibility and security.
Python Environment Management: I resolved dependency conflicts and managed the Python environment to ensure the script runs consistently and reliably on the production server.
Core Competencies & Tech Stack

### This project is a practical demonstration of my expertise in the following areas:
Languages: Python, JavaScript (ES6+), HTML5, CSS3
Backend: Flask, RESTful API Design
Frontend: Vanilla JS, DOM Manipulation, Fetch API (AJAX)
Artificial Intelligence: API Integration (Google Gemini), Prompt Engineering, RAG Architecture
Systems & Networking: Windows Server, Firewall Configuration, Port Forwarding, Dynamic DNS (DDNS), SSH
Methodologies: Problem Solving, Solution Architecture, Full-Stack Development, Data Engineering (ETL)

### Contact
This chatbot is a tangible example of how technology, when applied with a Business Intelligence strategy, can transform data into direct value for an organization and its community. I am proud of the result and eager to apply these skills to new and complex challenges.
Feel free to reach out if you'd like to learn more about this project or discuss potential collaborations.
