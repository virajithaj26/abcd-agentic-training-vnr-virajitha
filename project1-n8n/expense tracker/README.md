# Telegram Expense Tracking Agent  
**(n8n + Telegram + Google Sheets + Google Gemini)**  

Production-ready **n8n automation** that turns a Telegram bot into a **smart AI-powered expense tracker**.  
Users can log expenses via **text messages or voice notes**, and the system **records the expense in Google Sheets** and replies with a confirmation.

---

## 📁 Files and Assets

| Item | Description |
|------|-------------|
| **Workflow file** | `expense tracker.json` |
| **Snapshot** | `workflow.png` |
| **Demo reply** | `telegram reply.png` |
| **Video for text input** | `execution text.mp4` |
| **Video for audio input** | `execution audio.mp4` |

---

## 💡 What It Does

- Receives messages from users via **Telegram Bot API**.  
- Supports **text messages**:  
  Example: `today I spent 2000 rupees on clothes`  
- Supports **voice messages**:  
  Users can send a voice note describing their expense. The system transcribes it automatically.  
- Extracts structured expense information: **Date, Amount, Category**.  
- Persists minimal chat context per user (optional, using Buffer Window memory).  
- Updates **Google Sheets** automatically:  
  - Logs new expenses with the three fields: **Date, Amount, Category**.  
- Sends a confirmation reply in Telegram:  
  > “I have recorded that you spent 2000 rupees on clothes today, 2025-10-21”  
- Supports multiple users simultaneously.

---

## 🧱 Architecture

### Core Workflow Components

- **Telegram Trigger** → receives incoming messages (text or voice).  
- **AI Agent (LangChain)** → parses natural language or transcribed voice to extract **Date, Amount, Category**.  
- **Memory** → simple Buffer Window keyed by Telegram user ID (optional).  
- **Google Sheets Nodes** → append new rows in the **Expense Log** sheet.  
- **Telegram Send Message** → sends confirmation reply to the user.  

---

### 🔄 High-Level Flow

1. User sends a message or voice note to the Telegram bot.  
   **Example Text:** `today I spent 2000 rupees on clothes`  
   **Example Voice:** user records the same sentence.  
2. Telegram Trigger receives the message in n8n.  
3. If it’s a voice message, the system transcribes it to text.  
4. AI Agent extracts **Date, Amount, Category** from the text.  
5. Google Sheets node appends a new row in **Expense Log**.  
6. Telegram bot sends a confirmation back:  
   > “I have recorded that you spent 2000 rupees on clothes today, 2025-10-21”  
7. Optional: Chat is logged to **Chat DB** for analytics.

---

## 📊 Data Model (Google Sheets)

| Sheet | Columns | Description |
|-------|---------|-------------|
| **Expense Log** | Date, Amount, Category | Logs all expenses submitted via Telegram (text or voice). |
| **Chat DB (optional)** | Timestamp, User ID, Chat Message | Stores raw chat or transcribed messages for context and debugging. |

> Ensure column headers match exactly. Google Sheets nodes rely on these names to append data correctly.

