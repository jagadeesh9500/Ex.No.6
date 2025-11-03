# Ex.No.6: Development of Python Code Compatible with Multiple AI Tools

**Date:** 3-11-2025  
**Register No.:** 212223230083 

---

## Aim
To write and implement Python code that integrates with multiple AI tools (such as OpenAI GPT, Hugging Face, and Google Gemini) to automate API interactions, compare outputs, and generate actionable insights.

---

## AI Tools & Services Utilized
- **OpenAI GPT-3.5/4:** Used for text processing and summarization.  
- **Hugging Face Transformers:** Used for sentiment analysis.  
- **Amazon Warehouse API (Simulated):** Provides source data (robot alerts).  

---

## Python Implementation
The solution is structured into modular functions for data fetching, AI processing, and insight generation.  
**Note:** The complete, runnable script is formed by combining all the code sections below.

---

### 1. Imports and Setup
import random  
import openai  
from transformers import pipeline  
import warnings  
warnings.filterwarnings("ignore", message="No model was supplied")  

---

### 2. API Data Fetching (Simulated)
A mock function simulates fetching real-time data from a warehouse API.

def fetch_warehouse_data():  
 """Simulates fetching alerts from a warehouse API."""  
 return {  
  "robot_alerts": [  
   "Collision alert in Aisle B3",  
   "Battery low on Robot #AX-12",  
   "Package sorted in Zone 5"  
  ],  
  "status": random.choice(["Operational", "Maintenance_Needed"])  
 }  

---

### 3. AI Tools Integration

#### Hugging Face Sentiment Analysis
def analyze_sentiment(texts):  
 """Analyzes the sentiment of a list of alert texts."""  
 analyzer = pipeline("sentiment-analysis")  
 return [analyzer(text)[0] for text in texts]  

#### OpenAI Summarization
def generate_summary(text, api_key):  
 """Generates a concise summary of all alerts using OpenAI."""  
 openai.api_key = api_key  
 try:  
  response = openai.ChatCompletion.create(  
   model="gpt-3.5-turbo",  
   messages=[  
    {"role": "system", "content": "You are a helpful assistant that summarizes warehouse alerts."},  
    {"role": "user", "content": f"Summarize these alerts concisely: {text}"}  
   ]  
  )  
  return response.choices[0].message['content']  
 except Exception as e:  
  print(f"Error calling OpenAI API: {e}")  
  return "Summary generation failed."  

---

### 4. Comparison & Insight Generation
This function synthesizes the data from all sources into an actionable recommendation.

def generate_insights(data, sentiments, summary):  
 """Generates actionable insights based on AI analysis."""  
 critical = sum(1 for s in sentiments if s['label'] == 'NEGATIVE')  
 if data["status"] == "Maintenance_Needed":  
  recommendation = "Immediate maintenance required (System status abnormal)."  
 elif critical > 1:  
  recommendation = "Immediate maintenance recommended (Multiple critical alerts)."  
 else:  
  recommendation = "Routine check. System is operational."  
 return {  
  "critical_alerts_count": critical,  
  "operational_status": data["status"],  
  "ai_summary": summary,  
  "recommendation": recommendation  
 }  

---

### 5. Complete Workflow (Main)
The main function ties all components together, executes the workflow, and prints a formatted report.

def main():  
 OPENAI_API_KEY = "YOUR_OPENAI_API_KEY_HERE"  

 if OPENAI_API_KEY == "YOUR_OPENAI_API_KEY_HERE":  
  print("="*40)  
  print("!!! WARNING !!!")  
  print("Please set your OpenAI API key in the main() function to run.")  
  print("="*40)  
  return  

 print("Fetching warehouse data...")  
 data = fetch_warehouse_data()  
 alerts_text = "\n".join(data["robot_alerts"])  

 print("Analyzing alert sentiments...")  
 sentiments = analyze_sentiment(data["robot_alerts"])  

 print("Generating AI summary...")  
 summary = generate_summary(alerts_text, OPENAI_API_KEY)  

 insights = generate_insights(data, sentiments, summary)  

 print("\n" + "="*30)  
 print("   Warehouse AI Report   ")  
 print("="*30)  

 print("\n[Raw Alerts & Sentiment]")  
 for alert, sentiment in zip(data["robot_alerts"], sentiments):  
  print(f"  - {alert} ({sentiment['label']} | Score: {sentiment['score']:.2f})")  

 print(f"\n[Operational Status]\n  - {insights['operational_status']}")  
 print(f"\n[AI Summary]\n  - {insights['ai_summary']}")  
 print(f"\n[Actionable Recommendation]\n  - {insights['recommendation']}")  
 print("="*30)  

if __name__ == "__main__":  
 main()  

---

## Setup & Execution

### 1. Install Dependencies
Install the required libraries before running the script:  
`pip install transformers openai torch`

### 2. Set OpenAI API Key
Open the Python file (e.g., `warehouse_ai.py`) and replace the placeholder API key inside the `main()` function.

### 3. Run the Script
Save the file and execute it using:  
`python warehouse_ai.py`

---

## Experiment Details
**Input Prompt:**  
Write a Python program that fetches simulated warehouse robot alert data, uses Hugging Face Transformers to analyze sentiment for each alert, and uses OpenAI GPT (via API) to summarize the alerts. Based on sentiment and operational status, generate maintenance recommendations. The output should display individual alerts with sentiment scores, a summary of events, and a final recommendation. Use a modular structure with clearly defined functions for data fetching, sentiment analysis, OpenAI summarization, and insight generation.

---

## Example Output
*(Note: Output may vary slightly due to randomness and AI response variability.)*  

Fetching warehouse data...  
Analyzing alert sentiments...  
Generating AI summary...  

==============================  
 Warehouse AI Report  
==============================  

[Raw Alerts & Sentiment]  
 - Collision alert in Aisle B3 (NEGATIVE | Score: 0.99)  
 - Battery low on Robot #AX-12 (NEGATIVE | Score: 0.98)  
 - Package sorted in Zone 5 (POSITIVE | Score: 0.99)  

[Operational Status]  
 - Operational  

[AI Summary]  
 - The system has issued two critical alerts for a collision in Aisle B3 and a low battery on Robot #AX-12. A successful package sort in Zone 5 was also recorded.  

[Actionable Recommendation]  
 - Immediate maintenance recommended (Multiple critical alerts).  

==============================  

---

## Conclusion
The system successfully demonstrates the following:

- **Multi-AI Integration:**  
 Hugging Face was used for real-time sentiment analysis, while OpenAI provided contextual summarization.  

- **Automated Decision Making:**  
 The system classifies alerts by sentiment and produces automated maintenance recommendations.  

- **Scalable Architecture:**  
 The modular design supports easy expansion to include additional AI tools or data sources.  

**Result:** The corresponding prompt was executed successfully.
