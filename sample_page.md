## Choose Your Own Adventure Chatbot Project Page

**Project description:**
For this project, I create a “Choose Your Own Adventure” chabot through openrouter API with the mistrel 7b model: a model (compatible with ChatGPTs OpenAI package). The chatbot assumes the role of a DND dungeon master that sets up an environment and creates a story that develops based on user interative choices. Gradio is used to create an interface for the chatbot.

### 1. OpenRouter API Call

I start with a simple API call through OpenRouter, which is free and compatible with ChatGPTs OpenAI package. I have created a .env file to store the openrouter api key for security purposes.

```python
client = OpenAI(
base_url = "https://openrouter.ai/api/v1",
api_key = os.getenv("OPENAI_KEY"),
)
```

### 2. Assign System Role

The next step is to assign the system role. The system role can be specified by the developer so that the chatbot assumes this role throughout the conversation. For example, if you would like a chatbot that acts as a financial assistant, you can describe this in the system message

```python
system_msg = "Act as a financial expert assistant to the user"
```

The system message can be as descriptive as the developer would like, and a more advanced chatbot may be need more information from the system message to act accordingly. For the Choose Your Own Adventure chatbot, for example, the system message is about 2-3 sentences, and breifly explains the concept of choose-your-own-adventures. After the message is written, we assign it to the content key in a messages variable:

```python
messages = [{"role": "system", "content": system_msg}]
```

### 3. Responses to the User

The chatbot of course allows for input from the user, which then triggers a new response  by the chatbot. By appending inputs to our messages array, the chatbot will remember previous responses to create continued conversation. 

```python
def CustomChatGPT(user_input, history):
    messages.append({"role": "user", "content": user_input})
    response = client.chat.completions.create(
        model = "mistralai/mistral-7b-instruct:free",
        messages = messages
    )
    ChatGPT_reply = response.choices[0].message.content
    messages.append({"role": "assistant", "content": ChatGPT_reply})
    return ChatGPT_reply
```

Using Gradio's ChatInterface, we are able to get a nice interface to interact with our chatbot!

<img src="images/cyoa_chatsc.png?raw=true"/>

Citations:

https://www.gradio.app/docs/gradio/chatinterface

https://www.youtube.com/watch?v=pGOyw_M1mNE&ab_channel=TheAIAdvantage

https://openrouter.ai/docs/requests

https://platform.openai.com/docs/api-reference/introduction

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).
