# CHATBOT WITH RULE-BASED RESPONSES
A simple yet functional **rule-based chatbot** built using Python. This project demonstrates how basic conversational AI can be implemented using hardcoded rules, conditional logic, and keyword detection. The chatbot can handle greetings, identity questions, weather queries, jokes, and more — making it a fun and educational beginner-level project.

---

## 📌 Table of Contents

- [📖 Overview](#-overview)  
- [✨ Features](#-features)  
- [🛠️ Technologies Used](#️-technologies-used)
- [🚀 Code ](#code )  
- [💬 Example Usage](#-example-usage)  
- [🔧 Future Enhancements](#-future-enhancements)  

---

## 📖 Overview

This chatbot is a **command-line based conversational assistant** that responds to user queries using a rule-based approach. It recognizes specific keywords and phrases to generate relevant responses. The chatbot continues the conversation until the user types `"bye"`.

It's a beginner-friendly project to explore how chatbots work and lays the foundation for more advanced concepts like NLP and machine learning.

---

## ✨ Features

- Responds to:
  - Greetings (`hello`, `hi`, etc.)
  - Identity questions (`who are you?`)
  - Weather inquiries
  - Joke requests
  - Farewells (`bye`)
- Keyword-based detection
- Simple and clean Python logic
- Easy to customize and extend

---

## 🛠️ Technologies Used

- **Language**: Python 3.x  
- **Concepts**:
  - Conditional statements
  - String manipulation
  - Functions and loops
  - Basic keyword detection logic
- **IDE/Editor**: Visual Studio Code / Any Python-supported IDE

---
## 🚀 Code 
- **Objectives**:
  - In this code we are going to create a Rule Based AI Chatbot.
  - Our main Goal is to Create a simple rule-based chatbot that responds to predefined user inputs.
  - Key Requirements are Handle greetings and exit commands, Use if-else logic for responses, Run in a continuous loop.
  - In this code we learn the concept of Control flow, decisionmaking logic, basic AI concepts
- **Explanation**:
  - Firstly, we created a dictionary with predefined keys as user input and response is defined as output.
  - After that a method is defined that takes input as parameter ,then convert them in lower case and remove whitespaces.
  - Then a conditional if statement to handle exit condition.
  - In case of any input that is not present in dictionary this will be handle by a built-in .get function, this condition is required because we have limited chat-bot.
  - This function will return the reply on th bases of condition.
  - After that a method is defined that run the chat-bot.
  - Display user interface with some messages. Then, a while statement that takes input from user.
  - Call the get_response function and pass the input as an argument, it run untill the exit condition will not true.
  - At the end of code we have starting point of program that call the other functions.

## 💬 Example Usage

```bash
You: hello
Bot: Hey there! 👋 I'm DecoBot. How can I help you today?

You: tell me a joke?
Bot: Why do programmers prefer dark mode? Because light attracts bugs! 🐛😂

You: how are you?
Bot: I'm just code, but I'm running perfectly! 😄 How about you?

You: bye
Bot: Goodbye! Keep coding and stay curious. 👋
```

---

## 🔧 Future Enhancements

- Add support for:
  - More diverse queries and topics
  - NLP-based understanding using NLTK or spaCy
  - GUI integration using Tkinter or a web-based frontend
- Use a JSON or YAML file for storing responses dynamically
- Integrate with APIs (e.g., weather, jokes, news)
- Add logging and history tracking

---
