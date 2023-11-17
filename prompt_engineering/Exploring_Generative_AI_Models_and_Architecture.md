# Exploring Generative AI Models and Architecture

## Table of contents
- [Smart Aiding System for Visually Impaired](#smart-aiding-system-for-visually-impaired)
  - [Tesseract OCR](#tesseract-ocr)
    - [Using Tesseract OCR:](#using-tesseract-ocr)
      - [Command Line:](#command-line)
      - [Python (using pytesseract wrapper):](#python-using-pytesseract-wrapper)
  - [Open CV](#open-cv)
  - [You Only Look Once (YOLO)](#you-only-look-once-yolo)
    - [Using YOLO:](#using-yolo)
  - [CMU Sphinx](#cmu-sphinx)
  - [TensorFlow](#tensorflow)
    - [TensorFlow Key Features:](#tensorflow-key-features)
    - [TensorFlow Components:](#tensorflow-components)
    - [Using TensorFlow:](#using-tensorflow)
  - [Raspberry Pi](#raspberry-pi)
    - [Key Features:](#key-features)
    - [Use Cases and Projects:](#use-cases-and-projects)
    - [Getting Started:](#getting-started)
- [Generative AI Course Notebook](#generative-ai-course-notebook)
  - [Description](#description)
  - [Courses](#courses)
- [ChatGPT and Generative AI: The Big Picture](#chatgpt-and-generative-ai-the-big-picture)
  - [Table of Contents](#table-of-contents)
  - [Generative AI](#generative-ai)
  - [Discriminative AI](#discriminative-ai)
- [Exploring Generative AI Models and Architecture](#exploring-generative-ai-models-and-architecture)
  - [Table of contents](#table-of-contents-1)
  - [Generative AI](#generative-ai-1)
    - [Types of Generative AI models](#types-of-generative-ai-models)
    - [Variational Auto Encoders](#variational-auto-encoders)
    - [Transformers](#transformers)
    - [Generative Adversarial Networks (GANs)](#generative-adversarial-networks-gans)
    - [Combination Models](#combination-models)
  - [Chatbots](#chatbots)
    - [Architecture of Chatbots](#architecture-of-chatbots)
    - [Additional Architectural components](#additional-architectural-components)
  - [Large Language Models (LLMs)](#large-language-models-llms)
- [Getting Started on Prompt Engineering with Generative AI](#getting-started-on-prompt-engineering-with-generative-ai)
  - [Table of Contents](#table-of-contents-2)
  - [Prompt Engineering](#prompt-engineering)
    - [Prompt Engineering: Common Challenges with regular prompts](#prompt-engineering-common-challenges-with-regular-prompts)
    - [Anatomy of a Prompt](#anatomy-of-a-prompt)
  - [Usecases of prompts](#usecases-of-prompts)
  - [Interfaces for prompt](#interfaces-for-prompt)
    - [OpenAI: Playground vs API](#openai-playground-vs-api)
    - [OpenAI: Playground - Chat vs Assistant](#openai-playground---chat-vs-assistant)
  - [OpenAI GPT3 Parameters](#openai-gpt3-parameters)
  - [Top-k and Top-p Sampling](#top-k-and-top-p-sampling)
    - [Top-k Sampling:](#top-k-sampling)
    - [Nucleus Sampling:](#nucleus-sampling)
    - [Comparison:](#comparison)
  - [Prompting - types](#prompting---types)
    - [Factual Responses](#factual-responses)
    - [Summarization](#summarization)
    - [Text extraction](#text-extraction)
    - [Text Classification](#text-classification)
    - [Conversation](#conversation)
    - [Code generation](#code-generation)
    - [Math and Reasoning](#math-and-reasoning)
  - [Evaluating Prompt Performance](#evaluating-prompt-performance)
    - [Objective Metrics](#objective-metrics)
    - [Subjective Metrics](#subjective-metrics)
    - [Evaluation Techniques](#evaluation-techniques)
      - [Objective Evaluation Techniques:](#objective-evaluation-techniques)
      - [Subjective Evaluation Techniques:](#subjective-evaluation-techniques)
  - [Advanced Prompting Techniques](#advanced-prompting-techniques)
    - [Zero Shot](#zero-shot)
    - [Few Shot](#few-shot)
    - [Chain of thought](#chain-of-thought)
    - [Least to Most](#least-to-most)
    - [Generated Knowledge](#generated-knowledge)
    - [Ethical Considerations](#ethical-considerations)
  - [Best Practices of prompt engineering](#best-practices-of-prompt-engineering)
  - [Table of contents](#table-of-contents-3)
- [Navigating Generative AI Hurdles: Prototyping, Evaluation, and Ethics](#navigating-generative-ai-hurdles-prototyping-evaluation-and-ethics)
  - [Prototyping](#prototyping)
    - [Prototyping Significance](#prototyping-significance)
    - [Prototyping Steps](#prototyping-steps)
    - [Prototyping Techniques](#prototyping-techniques)
    - [Prototyping Effectiveness](#prototyping-effectiveness)
  - [Evaluating Generative AI Applications](#evaluating-generative-ai-applications)
    - [Evaluation Importance](#evaluation-importance)
    - [Different Evaluation Methods](#different-evaluation-methods)
    - [Applying Evaluation Methods](#applying-evaluation-methods)
    - [Understand Evaluation Results](#understand-evaluation-results)
  - [Customizing Generative AI](#customizing-generative-ai)
    - [Customization Importance](#customization-importance)
    - [Customizations Factors](#customizations-factors)
    - [Customization Techniques](#customization-techniques)
  - [Ethical Considerations in Generative AI](#ethical-considerations-in-generative-ai)
    - [Ethical Issues](#ethical-issues)
    - [Ethical Principles](#ethical-principles)
    - [Ethical Implications](#ethical-implications)


## Generative AI
It is a type of AI that generates new content based on what it has learnt from lots and lots of data. Data like text, audio, images and videos.
It learns the pattern of the data and then use those patterns to generate new content. It creates human like content.

### Types of Generative AI models
- Variational autoencoders (VAEs)
- Generative adversarial networks (GANs)
- Transformers
- Combination model

### Variational Auto Encoders

1. **Auto Encoders**
- **`Auto Encoders`** are a type of neural network that learns the most important features from the input data and then stores the captured information in a compressed form as numbers. The features and patterns it learns or captures are called latent representations of data, and the captured information is called latent data space.
The process of capturing important features and patterns of the input data and then storing them in the compressed form is called **`encoding`**. Now, after this stage of encoding, the auto encoder tries to reconstruct the same picture you provided as input to it, and this is called **`decoding`**. But auto encoders do not generate new data, they simply encode and decode the data that you input and try to minimize the difference between the input and output.

2. **Variational Auto Encoders**
- **`Variational Auto Encoders`** are a type of auto encoders that are used to generate new data, which may be similar but not the same, and the output is always different each time. VAEs do this by capturing and saving the patterns and features of the input data, to a distribution over the latent space where patterns and features are captured and stored separately according to different segments. After capturing and storing the patterns and features of the input data, VAEs are able to generate new images by sampling the data randomly, and due to this random sampling, variation and probability comes into picture. Instead of the same output with single value to describe features, patterns and attributes all the time, the encoder gives a probability distribution for each feature or latent attribute. One random selection from hundreds or thousands of attributes for the same feature. They then pass it through the decoder. The decoder tehn generates a new image that corresponds to the latent space point or the captured patterns and features.

3. **Use cases of VAEs**
   1) Image generation
   2) Video Prediction
   3) Music generation
   4) Detecting Anomalies


### Transformers
1. **Transformers Overview**
- There has been a need for a more powerful and effecient way to process sequential data like text streams, audio clips and video clips; and transformer models address this problem for us.
- Transformers are deep learning architecture used in different generative AI models that's been making waves in the AI world, especially in the field of Natural Language Processing.
- They learn context and understanding through sequential data analysis, data that is ordered and correlated, like what we use in our daily conversation, where you know the sentence has sequential words and context, which is the relationship between the words.
- They breakdown the text into smaller chunks called tokens and analyze the relationships and dependencies between them.
- Example: **`BERT (Bidirectional Encoder Representations from Transformers by Google), GPT (Generative Pre-trained Transformer by OpenAI)`**
- In pre-training, they learn language structures from large dataset. For example, BERT can predict missing words both ways in a sentence and GPT predicts next word during its pre-training. After pre-training, they fine-tune label data for a specific task. This saves time and uses less data and leverages their language understanding for task-like sentiment analysis and text summarization.

2. **Transfomer model: Architecture**
- Transformer models are also based on encoder-decoder model and consist of multiple layers of neural network. Each layer focuses on specific aspects, like keywords or context.
- The encoder processes input, capturing important features using self-attention and feed forward neural network.
- The decoder generates output, understanding the input using self-attention.
  
3. **Transformer model: Architecture components**
- **`Self Attention:`** It is the key mechanism of the transformer model that allows understanding of different parts of the input sequence. For example, in the sentence "The cat sat on the mat", the model learns word relationships like cat and sat, sat and mat.
- **`Multi Headed Attention:`** It performs self attention multiple times in parallel, capturing various relationships. For example, BERT has 12 heads, while GPT3 has 96, making the model more robust.
- **`Positional Encoding:`**  It helps to understand word order and their position, enabling faster processing. It contains positional information relative to other words, like vector numbers.
- `**Feed Forward Neural Networks:**` It transforms self-attention outputs into context-aware representation of tasks, like translation and answering questions. It has multiple layers with non-linear functions.

3. **Use Cases of Transformers**
- 1) Natural Language Processing (NLP)
- 2) Computer Vision
- 3) Audio and multi-modal processing

### Generative Adversarial Networks (GANs)
1. **Overview of GAN**
- GANs are a challenging, but rewarding machine learning technique.
- They are used to generate synthetic, but realistic data that is indistinguishable from real data. However, they can be difficult to train and can be unstable sometimes.
- They are a type of Neural network that can create realistic images, text and more.

2. **GAN archhitecture**
- The GAN has 2 networks. The Genrator and Discriminator.
- **`Generator`** generates new data that should look real while the **`Discriminator`** tried to find real data from fake data.
- During training the generator improves its creation to fool the discriminator, and the discriminator gets better at distinguishing real from fake.
- The process continues until the generator produces data that are indistinguishable from real data.

3. **GAN Challenges**
- Tricky to train
- Sensitive to settings
- Unlike traditional ML, GANs use adversarial loss, both the generator and discriminator measure the loss, and their weights are updated through back propogation.

4. **GAN Applications**
- 1) Fashion Industry: Automate Outfit designs
- 2) Enhance Product Description
- 3) Personalized recommendations

### Combination Models
1. **Overview of Combination Models**
- They are powerful tools that can be used to improve the accuracy and effectiveness of decision-making in many different areas.
- Also called Ensemble models, are a group of models working together to provide best possible outcome. They combine the predictions of multiple individual models to make more accurate and robust decisions.

2. **Combination Models - Architecture**
- **`Base Models:`** These are individual models that form a group of other Generative Models, like two GANs, trained on different data and using different algorithms.
- **`Combination Methods:`** It determines how the predictions of different base models are combined, using techniques like averaging, stacking or voting.
- **`Meta-Learner:` (Optional)** It learns how to best combine different base models to produce optimal results.

3. **Disadvantages of using combination models**
- Complexity: to train and interpret
- Data requirement: more than individual models
- Computational cost

## Chatbots
Chatbots are computer programs that simulate conversations with human users. They can be powered by AI and become even more robust when powered by Generative AI.
**Examples:** 
- 1) Google cloud offers conversational AI on Gen App Builder, a product that makes it easy to build AI-powered chatbots for conversational experiences.
- 2) Amazon connect is a popular AI contact center solution with an AI chatbot feature as well that uses Amazon Lex, Amazon Polly and Amazon Rekognition to provide a natural language conversational experience for customers.

### Architecture of Chatbots
- **`NLP:`** This is the core process of chatbots and is used to understand natural human language. NLP breakdowns the user inputs into smaller units, such as words and phrases, and identifies the intent of the user's request.
- **`Knowledge Base:`** This is a repository of information that the chatbot can access to answer user's questions. It can range from a simple list of FAQs to a complex database of structured data. 
- **`Dialog Management:`** Used to manage the chat between the users and chatbot. It keeps track of the conversation history, generates responses, and routes the user to the appropriate agent, if required.
- **`User Interface:`**  It can be a simple text based interface or GUI.

### Additional Architectural components
- **`Machine Learning:`** To improve performance over time
- **`Sentiment Analysis:`** to personalize responses 
- **`Data Analytics`** to collect and analyze data about the interactions with users. 

## Large Language Models (LLMs)
1. **Overview of LLM**
- LLMs are typically built using deep learning techniques, particularly Neural Netwroks, which are trained on massive amounts of text data which allows them to generate creative and realistic text, translate languages, generate codes and  answer questions in a way that is indistinguishable from human-generated text.
2. **LLM Architecture**
- As for the architecture these LLMs use **`transformers`** as their base model and are built using powerful **`deep-learning frameworks`**, like TensorFlow and PyTorch. The basic idea behind the architecture of LLM is that it's made up of a bunch of smaller models that work together. These models are called layers, and each layer is responsible for a different task.
- For example, one layer might be responsible for *understanding the meaning of words*, while another layer might be responsible for generating text.
- One of the successful architecture for LLM is Transformer architecture, which has been utilized in various iterations, such as Generative pre-trained Transformer, GPT4 etc.,
3. **Challenges of LLM**
- 1) Inaccurate information and hallucination
- 2) Biased behavior or harmful and malicious content
- 3) Data Piracy
- 4) High computing power and energy consumption


