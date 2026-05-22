# AI-CHATBOT-IMPLEMENTATION
## KOLLURU PUJITHA
## 212223240074

## An-Intelligent-Enterprise-Assistant
An AI-powered virtual assistant designed to help the public sector work smarter. It automates tasks like document search, policy answering, leave requests, and helpdesk support — all through natural conversation.

## Overview
The Intelligent Enterprise Assistant acts as a smart FAQ chatbot for organizational use. It uses predefined knowledge to respond to queries and can be easily extended to support more workflows. This prototype demonstrates how AI-powered chat support can: 1)Answer repeated employee/citizen queries instantly 2)Reduce helpdesk workload 3)Improve access to organizational knowledge 4)Serve as a foundation for integrating automation and data lookup

## Problem Statement
Public sector organizations handle large volumes of repetitive queries related to rules, HR policies, procedures, and internal services. Traditional systems require manual lookup, causing delays and inefficiency.

## Features
Conversational Chat Interface • Predefined knowledge responses • Web-based UI • Accessible using any browser • Easy to Customize • Responds to common policy and helpdesk queries • Users can ask questions naturally

## Project Structure
<img width="483" height="353" alt="image" src="https://github.com/user-attachments/assets/cd7bf4c8-c46b-4ce1-8959-1b4aad7380a4" />

## Implementation Code:
app.py
```
from flask import Flask, render_template, request, jsonify
from difflib import get_close_matches # Import for fuzzy matching

app = Flask(__name__)
#The Knowledge Base (Key = Exact Query, Value = Response)
responses = {
    "hi": "Hello! How can I assist you today?",
    "hello": "Hi there! How can I help?",
    "how are you": "I'm working perfectly! 😊",
    "bye": "Goodbye! Have a great day!",
    "what is an intelligent enterprise": "An Intelligent Enterprise uses AI, automation, and data to improve efficiency and decision making.",
    "what is your purpose": "My purpose is to assist employees and citizens by providing quick and accurate information.",
    "how do you help employees": "I help employees by answering questions instantly and reducing manual work.",
    "how to apply for leave": "You can apply leave using the HR Portal under Employee Self Service.",
    "what are office working hours": "Office hours are Monday to Friday, 9 AM to 5 PM.",
    "how to register a complaint": "You can register a complaint through the Citizen Grievance Portal.",
    "how to reset my password": "You can reset your password through the IT Helpdesk Support System.",
    "where to get service forms": "Service forms are available on the official e-Governance portal.",
    "what documents are needed for id card": "You need Aadhaar Card, Employee ID, and a passport-size photograph."
}

def get_fuzzy_response(user_msg):
    """
    Finds the best matching response key using fuzzy matching (difflib).
    """
    # Find the single best match with a low cutoff (0.4) for better flexibility
    matches = get_close_matches(user_msg, responses.keys(), n=1, cutoff=0.4) 
    
    if matches:
        # Return the response for the best match
        return responses[matches[0]]
    else:
        # Default response if no suitable match is found
        return "Sorry, I didn't understand. Can you try again?"


@app.route("/")
def index():
    return render_template("index.html")

@app.route("/get", methods=["POST"])
def chatbot_response():
    # Convert user input to lowercase for comparison
    user_msg = request.form["msg"].lower() 
    
    # Get the fuzzy matched response
    reply = get_fuzzy_response(user_msg) 
    
    return jsonify({"response": reply})

if __name__ == "__main__":
    app.run(debug=True)
```

index.html
```
<!DOCTYPE html>
<html>
<head>
    <title>Simple Chatbot</title>
    <link rel="stylesheet" href="/static/style.css"> 
</head>
<body>

<h2>💬 Simple AI Chatbot</h2>

<div id="chatbox"></div> 

<div class="input-area">
    <input type="text" id="userInput" placeholder="Type your message...">
    <button onclick="sendMessage()">Send</button>
</div>

<script src="/static/script.js"></script>
</body>
</html>
```
style.css
```
body {
    /* Updated: Light Blue background */
    background: #e0b4d6; 
    text-align: center;
    font-family: Arial, sans-serif;
}

h2 {
    color: #333;
}

#chatbox {
    width: 60%;
    height: 350px;
    background: #e0b4d6(177, 75, 146); /* Chatbox background remains white for readability */
    border-radius: 6px;
    padding: 10px;
    margin: auto;
    margin-bottom: 10px;
    overflow-y: auto;
    border: 1px solid #ccc;
}

.input-area {
    display: flex;
    justify-content: center;
    gap: 10px;
}

#userInput {
    width: 50%;
    padding: 10px;
    border-radius: 4px;
    border: 1px solid #555;
}

button {
    padding: 10px 20px;
    border: none;
    background: #007bff;
    color: rgb(188, 18, 18);
    cursor: pointer;
    border-radius: 4px;
}

button:hover {
    background: #0056b3;
}

/* Styling for conversation messages for clarity */
.chat-message {
    padding: 5px 0;
    text-align: left;
    border-bottom: 1px dotted #eee;
}
.chat-message b {
    color: #007bff;
}
```
script.js
```
function sendMessage() {
    // Get the value from the input field
    let msg = document.getElementById("userInput").value;
    // Don't send empty messages
    if (msg.trim() === "") return; 

    // Add user's message to the chatbox
    document.getElementById("chatbox").innerHTML += `<p class="chat-message"><b>You:</b> ${msg}</p>`;
    
    // Use the Fetch API to send the message to the Flask endpoint /get
    fetch("/get", {
        method: "POST",
        // The header tells the server the format of the data being sent
        headers: {"Content-Type": "application/x-www-form-urlencoded"}, 
        // Send the message as form data
        body: `msg=${msg}` 
    })
    .then(response => response.json()) // Parse the JSON response from Flask
    .then(data => {
        // Add the bot's response to the chatbox
        document.getElementById("chatbox").innerHTML += `<p class="chat-message"><b>Bot:</b> ${data.response}</p>`;
        
        // Scroll the chatbox to the bottom to show the newest message
        let chatbox = document.getElementById("chatbox");
        chatbox.scrollTop = chatbox.scrollHeight;
    });

    // Clear the input field after sending
    document.getElementById("userInput").value = "";
}+

// Add functionality to send message by hitting the Enter key
document.addEventListener('DOMContentLoaded', () => {
    document.getElementById('userInput').addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            sendMessage();
            e.preventDefault(); // Prevents the default action 
        }
    });
});
```
## How to Run Everything

Step 1: Open Terminal in Project Folder Navigate to your project's root directory (intelligent-enterprise-assistant-v2) using your terminal or command prompt.

cd intelligent-enterprise-assistant-v2
Step 2: Install Dependencies This project only requires the Flask web framework, as fuzzy matching is handled by Python's standard library (difflib).

pip install flask
Step 3: Start the Flask Server Run the main application file to start the web server.

python app.py You will see output in your terminal similar to this, indicating the server is active:

Running on http://127.0.0.1:5000/

Step 4: Access the Chatbot Open your web browser and navigate directly to the local server address:

http://127.0.0.1:5000/ You can now interact with the chatbot and test the fuzzy-matching feature!

## OUTPUT
<img width="1261" height="668" alt="image" src="https://github.com/user-attachments/assets/cd5944b7-c8af-459b-8184-b4154f48fda8" />

## RESULT:
Thus the Chatbot is executed Successfully.
