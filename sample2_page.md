## Natural Language Processing Chatbot Project Page

**Project description:**
This project focuses primarily on NLP concepts, using NLTK (Natural Language Toolkit), Tensorflow, and Keras to prepare and train a chatbot to understand basic English language phrases such as greetings, goodbyes, and pleasantries. This is the main focus of this project, although the chatbot is also trained to be developed into a bot that can help users with navigating their file explorer. For example, if a user is looking for a specific file and cannot recall where it is saved, if they want to open a specific folder, or if they would like their explorer to be more organized, the chabot could be developed further to help with these tasks.

### 1. Creating Data

The data being worked with is stored in a json file. It contains tags, patterns, responses, and context. The tag acts as grouping - there are tags with the values "greeting", "goodbye", and "thanks", among others. The pattern is an example of what a user might say to the chatbot, and the response is an example of how the bot should respond. For example, a pattern might be "How are you?", and a response could be "Good, thanks for asking". Finally, the context value includes tasks the chatbot may need to complete as a result of the pattern recorded. Below is an example, which is included in the json file used for this project.

```json
{"tag": "thanks",
 "patterns": ["Thanks", "Thank you", "That's helpful", "Awesome, thanks", "Thanks for helping me"],
 "responses": ["Happy to help!", "Any time!", "My pleasure"],
 "context": [""]
}
```

Here is another example that includes a context value: the user provides input related to the Documents folder, so the chatbot will likely have task relating to the folder.

```json
{"tag": "documents",
 "patterns": ["Open the document title Resume", "Tell me what's in my document folder", "Where can I find my last saved document?"],
 "responses": ["Searching...", "Performing task...one moment", "Opening relevant folder..."],
 "context": ["open_docs_folder"]
}
```

### 2. Preparing the Data for Training

In order for the data to be used for training by the model, there are several preprocessing steps that need to be completed. First, each sentence will be tokenized, meaning they are broken up into their core words. In this case, punctuation is ignored.

```python
for intent in intents['intents']:
    for pattern in intent['patterns']:
        #tokenize each wordd
        word = nltk.word_tokenize(pattern, language='english', preserve_line=True)
        words.extend(word)
        #add documents in corpus
        documents.append((word, intent['tag']))
        #add to classes list
        if intent['tag'] not in classes:
            classes.append(intent['tag'])
```

Following tokenization, each word is added to a document and sorted into classes, formatted to the lowercase, and then lemmatized - that is, to be shortened to its root form.

```python
words = [lemmatizer.lemmatize(w.lower()) for w in words if w not in ignore_letters]
``` 

### 3. Creating Training Data

The data is then sorted into training data and validation data. The patterns and words need to be converted into numbers so the model can understand the data. We create a list of zeroes the same length as the total number of words, and then set a value of 1 to words actually found in the patterns.

```python
for doc in documents:
    bag = []
    #tokenized words
    word_patterns = doc[0]
    word_patterns = [lemmatizer.lemmatize(word.lower()) for word in word_patterns]
    for word in words:
        bag.append(1) if word in word_patterns else bag.append(0)
    
    output_row = list(output_empty)
    output_row[classes.index(doc[1])] = 1
    training.append([bag, output_row])

#shuffle features and make np array
random.shuffle(training)
training = np.array(training, dtype=np.ndarray)

xtrain = list(training[:,0])
ytrain = list(training[:,1])
```

### 4. Creating and Compiling the Model

An SGD (Stochastic Gradient Descent) model is used to train the data. In an SGD model, instead of using the entire dataset for each iteration, only a small batch is selected to calculate gradient and update the model's parameters. The model parameters are then slightly updated in the direction of the negative gradient until the most optimized parametes are found. The model used contains 3 layers, and has an input shape equal to the length of the training data size. It also uses ReLu (Rectifier Linear Unit) activation, which means positive values pass through the layer unchanged, while negative values are set to zero. It can described simply with the mathematical function f(x) = max(0,x)

```python
model = Sequential([
    Dense(128, input_shape=(len(xtrain[0]),), activation='relu'),
    Dropout(0.5),
    Dense(64, activation='relu'),
    Dropout(0.5),
    Dense(len(ytrain[0]), activation='softmax')
])

sgd = SGD(learning_rate=0.01, decay=1e-6, momentum=0.9, nesterov=True) 
#can tweak
model.compile(
    loss = 'categorical_crossentropy',
    optimizer = sgd,
    metrics=['accuracy']
)
```

As the model is trained, it goes through 200 epochs (or complete cycles), with a batch size of 5. As you can see from the images below, the accuracy and loss is recorded after each cycle. The accuracy tends to increase as we go through more epochs.

<img src="images/nltk_chatsc4.png?raw=true"/> <img src="images/nltk_chatsc.png?raw=true"/>

### 5. GUI for ChatBot Using Python Tkinter

Finally, a graphic user interface is programmed in order to interact with the chatbot. Python Tkinter library is used to create this GUI. It is a simple window with a textbox and "Send" button for the user to talk to the bot, and then records the conversation in the space above the button and textbox. 

<img src="images/nltk_chatsc2.png?raw=true"/> <img src="images/nltk_chatsc5.png?raw=true"/>


Citations:

https://dzone.com/articles/python-chatbot-project-build-your-first-python-pro

https://docs.python.org/3/library/tkinter.html

more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).
