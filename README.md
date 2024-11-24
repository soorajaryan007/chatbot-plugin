## chatbot-plugin
a chatbot with a plugin-based architecture where each plugin adds specific functionality (e.g., weather updates, jokes, math calculations). The chatbot should handle basic interactions and dynamically load plugins.

Let's solve **Project 5: Chatbot with Plugin Architecture**. Here's a step-by-step breakdown:

---

### **Goal:**
Create a chatbot with a plugin-based architecture where each plugin adds specific functionality (e.g., weather updates, jokes, math calculations). The chatbot should handle basic interactions and dynamically load plugins.

---

### **1. Define the Chatbot Core**

Start by defining the main chatbot class that supports adding plugins dynamically.

```python
class Chatbot:
    def __init__(self):
        self.plugins = {}  # Dictionary to hold plugins

    def register_plugin(self, name, plugin):
        """Register a new plugin."""
        self.plugins[name] = plugin

    def get_response(self, user_input):
        """Generate response using plugins."""
        for name, plugin in self.plugins.items():
            if plugin.can_handle(user_input):
                return plugin.handle(user_input)
        return "I'm sorry, I don't understand that."

# Interface for plugins
class Plugin:
    def can_handle(self, user_input):
        """Determine if this plugin can handle the input."""
        raise NotImplementedError

    def handle(self, user_input):
        """Handle the input and return a response."""
        raise NotImplementedError
```

---

### **2. Implement Plugins**

Create specific plugins for features like jokes, weather, and math calculations.

#### **Plugin 1: Joke Plugin**
```python
import random

class JokePlugin(Plugin):
    def __init__(self):
        self.jokes = [
            "Why did the scarecrow win an award? Because he was outstanding in his field!",
            "Why don't skeletons fight each other? They don't have the guts.",
            "What do you call fake spaghetti? An impasta!"
        ]

    def can_handle(self, user_input):
        return "joke" in user_input.lower()

    def handle(self, user_input):
        return random.choice(self.jokes)
```

#### **Plugin 2: Math Plugin**
```python
class MathPlugin(Plugin):
    def can_handle(self, user_input):
        return any(op in user_input for op in ["+", "-", "*", "/"])

    def handle(self, user_input):
        try:
            # Evaluate simple math expressions
            result = eval(user_input)
            return f"The result is: {result}"
        except Exception:
            return "I couldn't calculate that."
```

#### **Plugin 3: Weather Plugin**
(For simplicity, this plugin returns static responses, but it can be extended using APIs like OpenWeatherMap.)

```python
class WeatherPlugin(Plugin):
    def can_handle(self, user_input):
        return "weather" in user_input.lower()

    def handle(self, user_input):
        return "The weather is sunny with a chance of code!"
```

---

### **3. Integrate Plugins into Chatbot**

Register the plugins into the chatbot and test its functionality.

```python
if __name__ == "__main__":
    # Create chatbot instance
    chatbot = Chatbot()

    # Register plugins
    chatbot.register_plugin("joke", JokePlugin())
    chatbot.register_plugin("math", MathPlugin())
    chatbot.register_plugin("weather", WeatherPlugin())

    # Main loop for user interaction
    print("Chatbot: Hi! How can I help you? (Type 'exit' to quit)")
    while True:
        user_input = input("You: ")
        if user_input.lower() == "exit":
            print("Chatbot: Goodbye!")
            break
        response = chatbot.get_response(user_input)
        print(f"Chatbot: {response}")
```

---

### **4. Expected Output**

#### Interaction 1: Ask for a Joke
```
You: Tell me a joke.
Chatbot: Why did the scarecrow win an award? Because he was outstanding in his field!
```

#### Interaction 2: Perform a Math Calculation
```
You: What is 5 + 3 * 2?
Chatbot: The result is: 11
```

#### Interaction 3: Ask About Weather
```
You: What's the weather like?
Chatbot: The weather is sunny with a chance of code!
```

---

### **5. Extending the Project**

- **Add More Plugins:** Create plugins for news, reminders, or even natural language processing using AI models like OpenAI's GPT.
- **Persist User Sessions:** Store user interactions and plugin states.
- **Use APIs:** Integrate external services for dynamic data like real-time weather or currency exchange rates.
- **GUI Interface:** Build a front-end using Tkinter or a web-based UI using Flask.

Let me know if you'd like to explore any of these extensions or need further clarification!
