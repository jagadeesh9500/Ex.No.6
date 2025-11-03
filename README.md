Ex.No.6 Development of Python Code Compatible with Multiple AI ToolsDate: 3-11-2025Register no.: 212223230083Aim:To write and implement Python code that integrates with multiple AI tools (such as OpenAI GPT, Hugging Face, and Google Gemini) to automate API interactions, compare outputs, and generate actionable insights.AI Tools Required:OpenAI GPT-3.5/4 (Text Processing)Hugging Face Transformers (Sentiment Analysis)Amazon Warehouse API (Simulated)Algorithm & ImplementationThe implementation is broken into four distinct functional parts.1. API Data Fetching (Simulated)A mock function simulates an API call to a warehouse management system, returning a list of robot alerts and a general system status.import random

def fetch_warehouse_data():
    """
    Simulates fetching data from a warehouse API.
    """
    return {
        "robot_alerts": [
            "Collision alert in Aisle B3",
            "Battery low on Robot #AX-12",
            "Package sorted in Zone 5"
        ],
        "status": random.choice(["Operational", "Maintenance_Needed"])
    }
2. AI Tools IntegrationTwo different AI models are used for processing:Hugging Face Transformers: A pre-trained sentiment analysis model is used to classify each alert (e.g., as NEGATIVE or POSITIVE).OpenAI GPT: A large language model is used to provide a high-level summary of all alerts combined.from transformers import pipeline
import openai

# Hugging Face Sentiment Analysis
def analyze_sentiment(texts):
    """
    Analyzes the sentiment of a list of texts using Hugging Face.
    """
    # Using a specific model to ensure it's available
    analyzer = pipeline("sentiment-analysis", model="distilbert-base-uncased-finetuned-sst-2-english")
    return [analyzer(text)[0] for text in texts]

# OpenAI Summarization
def generate_summary(text, api_key):
    """
    Generates a summary of the text using the OpenAI API.
    """
    # Note: Ensure you are using the correct client for your 'openai' library version.
    # This example assumes version < 1.0.0
    # For versions 1.0.0+, the client is initialized differently:
    # client = openai.OpenAI(api_key=api_key)
    # response = client.chat.completions.create(...)
    
    openai.api_key = api_key
    try:
        response = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",
            messages=[{"role":"user", "content":f"Summarize these alerts: {text}"}]
        )
        return response.choices[0].message['content']
    except Exception as e:
        print(f"Error calling OpenAI: {e}")
        return "Summary generation failed."
3. Comparison & Insight GenerationA simple logic function aggregates the AI-processed data to generate a final, actionable recommendation.def generate_insights(data, sentiments, summary):
    """
    Generates actionable insights based on data and AI analysis.
    """
    critical = sum(1 for s in sentiments if s['label']=='NEGATIVE')
    
    recommendation = "Routine check"
    if critical > 1 or data["status"] == "Maintenance_Needed":
        recommendation = "Immediate maintenance required"
        
    return {
        "critical_alerts": critical,
        "operational_status": data["status"],
        "ai_summary": summary,
        "recommendation": recommendation
    }
4. Complete WorkflowThe main function orchestrates the entire process: fetch data, process with AI, generate insights, and print the final report.def main():
    # 1. Fetch Data
    data = fetch_warehouse_data()
    alerts = data["robot_alerts"]
    
    # 2. AI Processing
    print("Analyzing sentiments...")
    sentiments = analyze_sentiment(alerts)
    
    print("Generating summary...")
    # --- IMPORTANT ---
    # Replace "your_openai_key" with your actual OpenAI API key
    # It's safer to use an environment variable (os.environ.get("OPENAI_API_KEY"))
    summary = generate_summary("\n".join(alerts), "your_openai_key")
    
    # 3. Generate Insights
    insights = generate_insights(data, sentiments, summary)
    
    # 4. Display Results
    print("\n" + "="*30)
    print("=== Warehouse AI Report ===")
    print("="*30 + "\n")
    print(f"Operational Status: {insights['operational_status']}\n")
    
    print("--- Individual Alerts ---")
    for alert, sentiment in zip(alerts, sentiments):
        print(f"  [{sentiment['label']}] {alert} (Score: {sentiment['score']:.2f})")
    
    print("\n--- AI Summary ---")
    print(f"  {insights['ai_summary']}")
    
    print("\n--- Recommendation ---")
    print(f"  >>> {insights['recommendation']} <<<")

if __name__ == "__main__":
    main()
Execution Steps:Install dependencies:pip install transformers torch openai
(Note: torch is required by Hugging Face Transformers)Set OpenAI API key:Find the line summary = generate_summary(...) in the script and replace "your_openai_key" with your valid OpenAI API key.Run script:python warehouse_ai.py
Input Prompt:"Write a Python program that fetches simulated warehouse robot alert data, uses Hugging Face Transformers to analyze sentiment for each alert, and uses OpenAI GPT (via API) to summarize the alerts. Based on sentiment and operational status, generate maintenance recommendations. The output should display individual alerts with sentiment scores, a summary of events, and a final recommendation. Use a modular structure with clearly defined functions for data fetching, sentiment analysis, OpenAI summarization, and insight generation."Example Output:Analyzing sentiments...
Generating summary...

==============================
=== Warehouse AI Report ===
==============================

Operational Status: Maintenance_Needed

--- Individual Alerts ---
  [NEGATIVE] Collision alert in Aisle B3 (Score: 0.99)
  [NEGATIVE] Battery low on Robot #AX-12 (Score: 0.99)
  [POSITIVE] Package sorted in Zone 5 (Score: 0.99)

--- AI Summary ---
  The alerts indicate two critical issues: a collision in Aisle B3 and a low battery on Robot #AX-12. A separate operation, package sorting in Zone 5, was completed successfully.

--- Recommendation ---
  >>> Immediate maintenance required <<<
Conclusion:The system successfully demonstrates:Multi-AI IntegrationHugging Face for real-time, local sentiment analysis.OpenAI for high-level, contextual summarization.Automated Decision MakingPriority-based alert classification using sentiment.Actionable maintenance recommendations derived from combined data.Scalable ArchitectureModular design allows for easy addition or replacement of AI models or API integrations.The pattern is adaptable to other industrial IoT or data processing scenarios.Result: The corresponding Prompt is executed successfully.
