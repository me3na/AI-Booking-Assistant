# AI Consulting Booking Assistant (n8n Workflow)

This workflow creates an intelligent, conversational booking assistant using LangChain nodes in n8n and Google Gemini. The agent handles user queries, verifies appointment availability, and automatically logs booked slots into Google Sheets.

## 🚀 Features
*   **Conversational Chat Interface:** Triggered immediately when a user sends a chat message.
*   **AI Agent (LangChain):** Equipped with custom system instructions to guide the user naturally through a booking funnel.
*   **Google Gemini Integration:** Uses Gemini as the underlying LLM to handle reasoning and tools.
*   **Memory Retention:** Utilizes a `Window Buffer Memory` node so the agent remembers conversation context.
*   **Tool Calling:** The agent can dynamically decide when to call the Google Sheets tools to check availability or write data.

## 🛠️ Workflow Structure
1.  **Chat Trigger:** Intercepts incoming chat messages.
2.  **AI Agent:** The brain of the operation, given specific guardrails (e.g., *Never say a slot is available unless confirmed*).
3.  **CheckAvailability (Tool):** Reads from Google Sheets to verify free slots.
4.  **BookSlots (Tool):** Appends confirmed bookings (Name, Date, Time, and Need) to the connected spreadsheet.

## ⚙️ Setup Requirements
To get this workflow running in your own n8n instance, you need to configure the following:

### 1. Google Gemini API
*   Select the **Google Gemini Chat Model** node.
*   Add your Google Gemini API credentials.

### 2. Google Sheets
*   **CheckAvailability Node:** Connect your Google account and specify the Spreadsheet ID and Sheet name where your availability schedule lives.
*   **BookSlots Node:** Connect your Google account. The workflow currently targets an existing sheet configuration mapping fields to columns (`Name`, `Date`, `Time`, `Need`). You may update the `documentId` if you want to use your own sheet.

## 📝 Agent Instructions
The agent is explicitly instructed to:
1. Ask for the visitor's name and consulting needs.
2. Request their preferred date and time.
3. Check availability using the `CheckAvailability` tool.
4. Append data using the `BookSlot` tool only when verified.
5. Provide the closest alternative if a slot is taken.
