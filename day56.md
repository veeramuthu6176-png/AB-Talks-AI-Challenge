from flask import Flask, request, jsonify
from flask_cors import CORS
from dotenv import load_dotenv
import os
import json
import google.generativeai as genai
from prompts import PROMPTS, BASE_SYSTEM_PROMPT

load_dotenv()

app = Flask(__name__)
CORS(app)

# Configure Gemini
genai.configure(api_key=os.getenv("GEMINI_API_KEY"))
model = genai.GenerativeModel('gemini-1.5-flash')

@app.route('/health', methods=['GET'])
def health():
    return jsonify({"status": "ok"})

@app.route('/debug', methods=['POST'])
def debug():
    try:
        data = request.get_json()
        code = data.get('code')
        error = data.get('error')
        language = data.get('language', 'python')
        eli5 = data.get('eli5', False)

        if not code or not error:
            return jsonify({"error": "code and error are required"}), 400

        if language not in PROMPTS:
            return jsonify({"error": f"Language '{language}' not supported yet"}), 400

        # Build the full prompt
        system_prompt = BASE_SYSTEM_PROMPT + "\n" + PROMPTS[language]
        if eli5:
            system_prompt += "\n8. IMPORTANT: Explain like I am 5 years old."

        user_prompt = f"""Language: {language}
Error: {error}
Code:
```{language}
{code}
```"""

        response = model.generate_content([system_prompt, user_prompt])
        response_text = response.text.strip()

        # Gemini sometimes adds ```json ``` so we clean it
        if response_text.startswith("```json"):
            response_text = response_text[7:-3]
        if response_text.startswith("```"):
            response_text = response_text[3:-3]

        ai_json = json.loads(response_text)
        return jsonify(ai_json), 200

    except json.JSONDecodeError:
        return jsonify({"error": "AI returned invalid JSON. Please try again."}), 500
    except Exception as e:
        return jsonify({"error": str(e)}), 500

if __name__ == '__main__':
    app.run(debug=True, port=5000)
