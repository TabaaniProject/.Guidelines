![Image](https://github.com/user-attachments/assets/a1a69c0a-bdd8-48df-87fb-cc8ff8ec2142)
# Tabaani Ecosystem - Central README

## Overview
Tabaani is an AI-driven travel technology company focused on redefining the way travelers plan and experience trips. Our ecosystem is built on a **seamless integration of AI models, travel booking platforms, and centralized management tools** to create an efficient and data-driven travel experience.

This repository serves as the central hub for all Tabaani projects, detailing how each component contributes to the broader ecosystem.

## Products in the Tabaani Ecosystem
![Image](https://github.com/user-attachments/assets/f7929f54-a862-4fc9-8d43-1364ff9ac369)
### 1. **Tripease** (Social Travel Booking Platform)
**Tripease** is a web platform where travelers can plan and book trips collaboratively. It provides:
- **User Roles:**
  - **Travelers:** End users who plan and book trips.
  - **TripLeaders:** Organizers who create and manage group trips.
  - **Local Hosts:** Service providers offering accommodations and experiences.
  - **Local Experts:** Travel professionals who provide insights and validate AI recommendations.

- **Core Features:**
  - Create **private trips** with friends.
  - Join **public trips** created by verified travel agents.
  - Book **accommodations, activities, and transportation**.
  - **AI-generated personalized itineraries** powered by **Basir**.
  - Secure **payments and booking management**.

- **Monetization Model:**
  - **10% commission** on trip bookings.
  - **Premium subscriptions** for travel agents to post public trips.

---
![Image](https://github.com/user-attachments/assets/9aa6ec13-d469-48bf-b494-c9035587caeb)
### 2. **Assil** (Small Language Model - SLM for Travel AI)
**Assil** is our proprietary **AI model** fine-tuned for travel planning, integrating NLP and recommendation algorithms. It:
- Powers **AI-driven travel recommendations** within **Tripease**.
- Generates personalized trip itineraries based on **user preferences, real-time travel trends, and external data sources**.
- Provides an **API for enterprise customers** to integrate AI-powered travel assistants into their platforms.
- Uses a **human-in-the-loop** feedback system, where **Local Experts** refine AI responses when needed.
- Built using a **fine-tuned LLaMA-based model**, with data pipelines sourced from our structured travel database.

**Key AI Capabilities:**
- **Natural Language Understanding (NLU)** for personalized travel conversations.
- **Retrieval-Augmented Generation (RAG)** to pull real-time information from external sources.
- **Contextual Memory** to enhance user interactions.
- **Continuous Learning** through reinforcement from **Local Experts**.

---
![Image](https://github.com/user-attachments/assets/cb2d140f-21a2-4058-b531-1742e1207d4b)
### 3. **Basir** (Consumer-Facing AI Travel Assistant)
**Basir** is an AI travel assistant designed for **end consumers**. Unlike **Assil**, which is an enterprise-level API, **Basir**:
- Generates **personalized trip plans** directly for users.
- Integrates into **Tripease** to provide AI-powered suggestions.
- Uses data from **Assil’s structured travel dataset** for enhanced recommendations.
- Continuously refines responses based on user interactions and Local Expert feedback.

**Technical Architecture:**
- **Front-end:** Embedded into **Tripease** via API calls.
- **Back-end:** Powered by **Assil’s AI model**, running inference with optimized query handling.
- **Reinforcement Learning:** Local Experts validate **Basir’s recommendations** to improve accuracy.

---

### 4. **TripMaster** (Centralized Admin Panel)
**TripMaster** is our **admin dashboard** that manages all aspects of the Tabaani ecosystem. It includes:

#### **User & Role Management:**
- Handles **Travelers, TripLeaders, Local Hosts, and Local Experts**.
- Assigns permissions and access control for each role.
- Logs user activity for security and monitoring.

#### **AI Feedback Management:**
- **Local Experts** provide feedback on **Assil’s AI-generated responses**.
- Tracks improvement metrics for AI response accuracy.
- Stores expert feedback for **continuous AI fine-tuning**.

#### **Booking & Payment Management:**
- Processes **payments, cancellations, and refunds**.
- Supports **multiple currencies and payment gateways**.
- Logs transactions with fraud detection mechanisms.

#### **Analytics & Reporting:**
- Tracks **user behavior, booking trends, and platform growth**.
- Provides **real-time insights** on AI performance and customer interactions.
- Enables **business intelligence reporting** for decision-making.

**Tech Stack:**
- **Front-end:** React.js
- **Back-end:** Node.js, Express, PostgreSQL
- **AI Monitoring:** Integrated with Assil’s feedback loop.

---

### 5. **Assil’s Travel Dataset & Centralized Travel Database**
We maintain a structured travel dataset that fuels both **Assil** and **Basir**. This dataset includes:
- **Activities** (e.g., city-based attractions, duration, budget, traveler types).
- **Restaurants** (e.g., cuisine type, meal prices, recommendations).
- **Accommodations** (e.g., hotels, hostels, apartments, pricing, location-based preferences).
- **Transportation** (e.g., routes, modes of transport, price ranges, schedules).
- **Visa & Seasonal Information** (e.g., entry requirements, peak seasons).
- **Scam Alerts** (e.g., common tourist scams and prevention tips).

**Database Architecture:**
- **PostgreSQL** as the primary relational database.
- **Data Warehouse** for historical trends and AI training.
- **GraphQL API** for dynamic querying and integration with Assil & Basir.

---

## How These Products Interconnect
### **Creating a Seamless Travel Ecosystem**
1. **Tripease** serves as the **primary user interface** where travelers and agents interact.
2. **Basir** (AI Assistant) integrates into **Tripease** to provide AI-driven trip planning.
3. **Assil** acts as the **back-end AI engine**, generating recommendations for **Basir** and offering API services to external businesses.
4. **Local Experts** (managed via **TripMaster**) provide **human reinforcement learning** to **improve Assil’s recommendations**.
5. **TripMaster** ensures smooth platform operations, booking transactions, and data insights.
6. The **centralized travel dataset** powers all AI-driven decisions within **Assil** and **Basir**.

---

## Future Enhancements
- **Improved AI Training:** Expanding **Assil** with more real-world travel insights.
- **Enhanced Analytics:** Deepening data visualization in **TripMaster**.
- **Expanded Enterprise Integrations:** Providing more API features for businesses using **Assil**.
- **Incorporation of LLM Fine-Tuning Pipelines** for Assil’s continuous improvement.

---

## Contribution Guidelines
- Follow **our coding guidelines** (see `CODE_ARCHITECTURE.md`).
- Use **ClickUp** for task tracking and progress updates.
- Submit all changes via **pull requests** and follow the review process.

---

### **Tabaani - Innovating the Future of Travel with AI** 🚀
