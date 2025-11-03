# 🧠 Automated Text Analysis with n8n & Gemini API

This project demonstrates a **full-stack AI automation system** that performs **text summarization or sentiment analysis** using **Google’s Gemini API**, orchestrated through **n8n**, and presented in a clean **Tailwind CSS web interface**.

Users can input any paragraph, click **Analyze**, and receive an AI-generated summary or sentiment report — all in one click.

---

## 🚀 Project Overview

### 🔧 Core Idea
The system automates text processing through an n8n workflow connected to Gemini.  
It’s fully integrated with a front-end interface that triggers the automation and displays the LLM output.

**Flow:**  
User Input → n8n Webhook → Gemini API → n8n Set Node → Webhook Response → Front-End Display

---

## 🧩 Components

| Layer | Technology | Purpose |
|-------|-------------|----------|
| **Automation Backend** | [n8n Cloud](https://app.n8n.cloud) | Handles request flow, connects Webhook to Gemini |
| **AI Model** | [Gemini API (Google Generative AI)](https://ai.google.dev/) | Performs text summarization or sentiment analysis |
| **Front-End** | HTML, Tailwind CSS, JavaScript | User-facing interface for text input/output |
| **Networking** | Webhook (POST) | Links browser → n8n workflow |
| **Configuration** | `config.json` | Stores your Webhook URL securely |

---

## 🛠️ Project Setup

### 1️⃣ n8n Workflow Configuration

Your uploaded workflow file  
📄 `Automated Text Analysis with LLM Integration.json`  
already includes the required node structure:

#### 🧱 Node Sequence
1. **Webhook Trigger (Start Node)**  
   - Method: `POST`  
   - Response Mode: *Respond immediately*  
   - Used to receive text input from front-end.

2. **Gemini API Request (HTTP Node)**  
   - Endpoint: `https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent`  
   - Method: `POST`  
   - Headers:  
     ```
     Authorization: Bearer YOUR_GEMINI_API_KEY
     Content-Type: application/json
     ```
   - JSON Body:
     ```json
     {
       "contents": [
         {
           "parts": [
             {
               "text": "Perform sentiment analysis or summarization on: {{$json['text_to_analyze']}}"
             }
           ]
         }
       ]
     }
     ```

3. **Set Node**  
   - Extracts the LLM response and standardizes output.
   - Expression:
     ```js
     {
       "final_result": "{{$json['candidates'][0]['content']['parts'][0]['text']}}"
     }
     ```

4. **Webhook Response Node**  
   - Responds with JSON:
     ```json
     {
       "final_result": "The sentiment of the text is Positive."
     }
     ```

✅ **Activate the workflow** in n8n to make your **Production Webhook URL** live.

---

### 2️⃣ Front-End Setup

#### 📁 Folder Structure
```
/Text-Analysis
├── index.html              # Styled front-end UI
├── config.json             # Contains n8n Webhook URL
├── Automated Text Analysis with LLM Integration.json
└── README.md
```

#### ⚙️ `config.json`
Create this file in the same directory as your `index.html`:

```json
{
  "WEBHOOK_URL": "https://your-subdomain.n8n.cloud/webhook/automated-text-analysis"
}
```
> Replace the URL above with your actual **n8n Production Webhook URL** from the **Webhook Trigger** node.

---

## 🎨 Front-End (index.html)

The front-end uses **Tailwind CSS** for styling and dynamically loads your webhook URL from `config.json`.

### 💡 Features:
- Clean, card-style layout  
- Responsive design (mobile-friendly)  
- Dynamic API response capture  
- Debug info and Copy-to-Clipboard button  
- Real-time loading feedback  

### 🧠 User Flow:
1. Enter or paste text in the input box.  
2. Click **Analyze**.  
3. The front-end sends a JSON request:
   ```json
   { "text_to_analyze": "Your paragraph here" }
   ```
4. The n8n workflow runs → calls Gemini → formats output.  
5. The response appears instantly in the result card.

---

## 🧾 Example Run

**Input:**  
> “The product is well-designed, but the delivery experience was poor.”

**n8n Response:**  
```json
{ "final_result": "The sentiment is mixed — good product but poor delivery." }
```

**Displayed Output (UI):**  
> 🟡 *The sentiment is mixed — good product but poor delivery.*

---

## 💻 Tech Stack

| Component | Tool | Purpose |
|------------|------|----------|
| **Automation** | n8n Cloud | Workflow orchestration |
| **AI Model** | Gemini API | Natural language processing |
| **Front-End** | HTML, JS, Tailwind | UI & user interaction |
| **Hosting (Optional)** | GitHub Pages / Local | Static site deployment |

---

## 🧱 Project Flow Diagram

```
┌────────────┐
│   Browser  │
│ (index.html)│
└──────┬─────┘
       │ JSON POST
       ▼
┌────────────┐
│   n8n Webhook │
└──────┬─────┘
       │
       ▼
┌───────────────┐
│ Gemini API     │
│ (Text Analysis)│
└──────┬────────┘
       │
       ▼
┌────────────┐
│ n8n Set Node │
│ Format JSON  │
└──────┬─────┘
       │
       ▼
┌────────────┐
│ Webhook Response │
└──────┬─────┘
       │
       ▼
┌────────────┐
│ Front-End UI │
│ Show Result  │
└────────────┘
```

---

## 🔍 Debugging & Notes

- **Error 404:** Usually means your n8n Webhook URL is incorrect or not activated.  
- **Empty Output:** Ensure your `Set Node` expression matches your Gemini response path.  
- **Local Testing:** Open `index.html` in a browser — no server required.  
- **Config Not Found:** Make sure `config.json` is in the same directory.

---

## 🧩 Future Enhancements

- Add dropdown for **mode selection** (Summary / Sentiment / Keywords).  
- Display **response time** and request logs.  
- Integrate **OpenAI API** as an alternative LLM.  
- Include **light/dark mode toggle** for UI.  

---

## 👨‍💻 Author

**Kowshik Tukuntla**  
AI/ML Engineer | Automation Developer  
📧 Email: [your.email@example.com]  
🌐 Portfolio: [your-portfolio-link.com]

---

## 📜 License

This project is open source and distributed under the **MIT License**.

---

## 🙌 Acknowledgments

- [n8n Documentation](https://docs.n8n.io/)
- [Gemini API Docs](https://ai.google.dev/)
- [Tailwind CSS](https://tailwindcss.com/)
- [MDN Web Docs](https://developer.mozilla.org/)
