import json
import os

class SimpleAI:
    def __init__(self, data_file='responses.json'):
        self.data_file = data_file
        self.responses = self.load_responses()

    def load_responses(self):
        # Load responses from JSON file if it exists
        if os.path.exists(self.data_file):
            with open(self.data_file, 'r') as file:
                return json.load(file)
        return {
            "hello": "Hello! How can I assist you?",
            "how are you?": "I'm doing great, and you?",
            "bye": "Goodbye! Talk to you later."
        }

    def save_responses(self):
        # Save responses to JSON file
        with open(self.data_file, 'w') as file:
            json.dump(self.responses, file)

    def respond(self, message):
        # Respond based on user input or default message
        return self.responses.get(message.lower(), "Sorry, I don't understand your request.")

    def learn(self, question, answer):
        # Add new question-answer pair to responses and save to file
        self.responses[question.lower()] = answer
        self.save_responses()
        return "Got it! I've learned a new response."

# Example usage
if __name__ == "__main__":
    ai_agent = SimpleAI()
    while True:
        user_input = input("You: ")
        
        if user_input.lower() == "bye":
            print(f"AI Agent: {ai_agent.respond(user_input)}")
            break
        elif user_input.lower().startswith("learn:"):
            # Example: "learn: question; answer"
            try:
                _, data = user_input.split(":", 1)
                question, answer = map(str.strip, data.split(";"))
                print(f"AI Agent: {ai_agent.learn(question, answer)}")
            except ValueError:
                print("AI Agent: Please provide the learning data in the format 'learn: question; answer'")
        else:
            print(f"AI Agent: {ai_agent.respond(user_input)}")
