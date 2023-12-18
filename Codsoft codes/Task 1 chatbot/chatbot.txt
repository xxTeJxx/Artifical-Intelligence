import re

def simple_chatbot(user_input):
    user_input = user_input.lower()

    greetings = ['hello', 'hi', 'hey', 'greetings', 'howdy']
    farewells = ['bye', 'goodbye', 'see you', 'take care']
    questions = ['how are you', 'what is your name', 'who are you']
    default_response = "I'm a simple chatbot. Ask me anything!"

    if any(word in user_input for word in greetings):
        return "Hello! How can I help you today?"

    elif any(word in user_input for word in farewells):
        return "Goodbye! Have a great day."

    elif any(word in user_input for word in questions):
        return "I'm just a chatbot. You can call me ChatGPT."

    else:
        return default_response

while True:
    user_input = input("You: ")
    if user_input.lower() == 'exit':
        print("Chatbot: Goodbye!")
        break
    response = simple_chatbot(user_input)
    print("Chatbot:", response)
