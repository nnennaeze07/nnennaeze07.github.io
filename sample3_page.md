## Kaggle Competition: Predicting Housing Prices Project Page

**Project description:**
This machine learning competitionn gives the user a dataset of house and prices, along with specific features of the houses that include number of beds and baths, lot size, yard size, neighborhood information, year built, and so on. The goal of the competition is to build and train a model that will accurately predict the sale price of houses in a new dataset. For my model, I use a Random Forest classifier, which is a model that takes the mean value of multiple Decision Trees. I used this model, along with specific features that I tested based on which feature combination would provide the lowest mean absolute error with the validation data used.

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
