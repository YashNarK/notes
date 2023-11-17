# Getting Started on Prompt Engineering with Generative AI

## Table of Contents
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

## Prompt Engineering
The art and science of designing effective queries or instructions to get desired responses.
In other words: Ask better questions to get better answers.

Use prompt engineering to get more precise, creative, and domain-specific answers and eliminate some common challenges.

### Prompt Engineering: Common Challenges with regular prompts

1. **Model Bias:**
   - **Issue:** Models may exhibit biases present in the training data, leading to skewed or unfair responses.
   - **Challenge Addressed in Prompt Engineering:**
     - **Example:** "Ensure the response is unbiased and inclusive."

2. **Unpredictability:**
   - **Issue:** Models might generate unpredictable or inconsistent responses to similar queries.
   - **Challenge Addressed in Prompt Engineering:**
     - **Example:** "Maintain consistency in responses for similar queries."

3. **Lack of Real-World Knowledge:**
   - **Issue:** Models may lack context about current events or real-world situations.
   - **Challenge Addressed in Prompt Engineering:**
     - **Example:** "Consider recent news events while generating responses."

4. **Language and Cultural Nuances:**
   - **Issue:** Models may struggle with understanding and incorporating language nuances and cultural references.
   - **Challenge Addressed in Prompt Engineering:**
     - **Example:** "Adapt responses to align with cultural sensitivities and local language nuances."

Prompt engineering aims to mitigate these challenges by providing clear and explicit instructions to the model, guiding it to produce responses that are unbiased, consistent, contextually aware, and culturally sensitive.

### Anatomy of a Prompt
1. `PERSONA:` What role should model play?
   - Example: "Imagine you are a customer support agent."
   - Explanation: Instructing the model to take on a specific role or perspective, such as that of a customer, developer, or teacher.
2. `INSTRUCTIONS:` What should the model do? This typically starts with an action word.
   - Example: "Summarize the main points of the article."
   - Explanation: Providing a clear action for the model to perform, often starting with an action verb like "summarize," "classify," or "generate."
3. `INPUT CONTENT:` Text to be processed by the model.
   - Example: "Given the following paragraph about climate change:"
   - Explanation: Offering the model specific text or data to process. This sets the context for the task.
4. `FORMAT:` Requirements around the format for the output, if any.
   - Example: "Provide the summary in bullet points."
   - Explanation: Specifying the desired format for the output, such as bullet points, paragraphs, or a specific structure.
5. `ADDITIONAL BACKGROUND:` Any other helpful context or background.
   - Example: "The user is a beginner in programming."
   - Explanation: Offering any extra information or context that might assist the model in understanding the user's needs or level of expertise.

**Example prompt:** 
- *persona:* As a professional movie critic 
- *instructions:* please write 
- *input content:* a review of the movie "John Wick: Chapter 4" 
- *fomrat:* in a concise, compelling manner, focusing on plot, performances and visual effects. 
- *additional background:* Avoid revealing major spoilers.

## Usecases of prompts
- Factual Response
- Text summarization
- Text extraction
- Text classification
- Conversation
- Code Generation
- Reasoning and Math

## Interfaces for prompt
- ChatGPT
- BARD
- BING
- Dall-E
- OpenAI: Playground
- OpenAI: API

### OpenAI: Playground vs API
**`OpenAI Playground:`** The Playground is an interactive web interface provided by OpenAI to explore their language models. It's a great tool for doing quick tests, understanding capabilities, and experimenting with different prompts and parameter settings (e.g., temperature, max tokens) without writing any code. However, it's not intended for building production systems or integrating with applications.

**`OpenAI API:`** The API (Application Programming Interface), on the other hand, is a service that allows developers to call OpenAI models from their applications, services, or systems. It allows for the integration of OpenAI's technology directly into applications, whether they be web, mobile, or other software applications.

In short, the Playground is for interactive testing and exploration, and the API is for building applications and services. It's important to note that these two services are billed separately by OpenAI.

### OpenAI: Playground - Chat vs Assistant
**`Chat:`** This is a tool provided by OpenAI where you can test and experiment with different models such as GPT-3 in an interactive way. It is like an AI sandbox where you can put different types of prompts and see how the AI responds. You can tweak different parameters like temperature and max tokens and see how the AI behaves. It's a platform to refine your approach with AI models before implementing them into an application.

**`Assistant:`** This can be considered as a specific implementation of the AI model that is designed to help answer queries, provide solutions, and carry out tasks effectively as instructed by the user. The assistant utilizes the AI model but is fine-tuned to cater to specific needs. For example, in this scenario, I am an assistant who is trained to help with learning different skills and technologies.

While a playground allows experimentation and learning, an assistant is more about application and task execution. Both serve their purpose in helping users understand and leverage AI capabilities.

## OpenAI GPT3 Parameters
ChatGPT has several important parameters that influence how it operates and functions. They include:

**`Model:`** This refers to the underlying AI model that is used to generate text. Different models may have different characteristics and capabilities. OpenAI's GPT-3 is an example of a model.

**`Temperature:`** This parameter controls the randomness in the model's responses. A higher value (near 1) results in more random outputs, while a lower value (near 0) makes the outputs more deterministic and focused, often sticking to the most probable next word.

**`Maximum length:`** This refers to the maximum number of tokens (words or parts of words) that can be included in the model's output.

**`Stop Sequences:`** These are the sequences at which the model will end the text generation process. For example, if we set the stop sequence as ".", the model will stop generating text as soon as it encounters a period.

**`Top P:`** This refers to nucleus sampling. The model generates a probability distribution for the next word and then only considers the top k possible words. This is used to introduce a degree of randomness while still having the model's responses make sense.

**`Frequency Penalty:`** This is a parameter that penalizes words that appear too often in the text generation. A high frequency penalty encourages more diversity in the response by reducing the likelihood of repetitive words.

**`Presence penalty:`** This parameter penalizes new words that have not yet appeared in the text. A high presence penalty encourages repetition of words or phrases that were already present in the generated text.

These parameters allow you to customize the AI's behavior so you can achieve the desired results. For instance, if you want more creative outputs, you can increase the temperature. If you want shorter outputs, you can decrease the Maximum Tokens.

## Top-k and Top-p Sampling
Top-k sampling and nucleus sampling are two different techniques used in natural language processing tasks, particularly in text generation. These techniques are often employed to make language models, such as GPT (Generative Pre-trained Transformer), produce more controlled and coherent outputs.

### Top-k Sampling:

In top-k sampling, the model generates a probability distribution for the next word in the sequence and then only considers the top-k words with the highest probabilities. The value of k is a hyperparameter that determines the number of words to consider. This method helps in introducing diversity to the generated text while still maintaining some level of control.

- **Example:**
  - If top-k is set to 5, the model will consider only the top 5 words with the highest probabilities for the next token.

### Nucleus Sampling:

Nucleus, also known as top-p sampling is an extension of top-k sampling. Instead of specifying a fixed number k, in nucleus sampling, you define a cumulative probability threshold (p). The model generates a probability distribution for the next word and selects words until the cumulative probability surpasses the threshold (p).

- **Example:**
  - If the threshold p is set to 0.8, the model will keep adding words to the sequence until the cumulative probability is at least 0.8.

### Comparison:

- **Control vs Diversity:**
  - Top-k sampling provides more control over the output by selecting from a fixed number of options.
  - Nucleus sampling allows for a dynamic selection of options based on cumulative probabilities, promoting diversity.

- **Hyperparameters:**
  - In top-k sampling, you set a specific value for k.
  - In nucleus sampling, you set a threshold probability p.

- **Use Cases:**
  - Top-k sampling might be preferred when fine-tuning and controlling the generation process are crucial.
  - Nucleus sampling might be chosen for scenarios where a balance between control and diversity is desired.

- **Flexibility:**
  - Nucleus sampling is considered more flexible as it adapts to different probabilities dynamically.

Both techniques are useful for addressing issues like repetitive or overly deterministic outputs in language models. The choice between top-k sampling and nucleus sampling depends on the specific requirements of the task and the desired characteristics of the generated text.

## Prompting - types
### Factual Responses
Strightforward question and answers, Explanations.

```
USER: Explain theory of relativity to a {teenager, expert, grandma}
GPT: {Explanation according to your prompt}
USER: What is the name of the movie in which I acted as a hero?
GPT: {Some hallucinated answer}
```
To avoid hallucinations,
- **`Limit the likelihood of made-up answers: `** **PERSONA**: Act as a player of the game trivial pursuit. If you don't know the answer, respond with a "?_?"
- **`Lower the temperature`**

### Summarization

Text or Code summarization

Delimiters for separating prompt from input are,
1. `### \n INPUT`
2. `""" \n INPUT \n """`
3. `--- \n INPUT`

- **TEXT SUMMARIZATION**

**USER:**
```
Summarize the most important concepts of the text below using three bullet points.
Prompting, in the context of natural language processing and machine learning, refers to the act of providing specific instructions or queries to a language model to guide its generation of coherent and contextually relevant text. It involves formulating a clear and concise input to elicit desired outputs from the model. The quality and specificity of the prompt significantly influence the generated content, making it a crucial aspect in leveraging language models for various tasks such as text completion, question answering, or creative writing. Effective prompting requires an understanding of the model's capabilities, the task at hand, and the nuances of language to produce accurate and contextually appropriate responses.
```
**GPT:**
```
- **Prompting Definition:**
  - Prompting involves providing explicit instructions or queries to a language model in natural language processing and machine learning.
  
- **Influence on Generated Content:**
  - The quality and specificity of the prompt play a critical role in shaping the coherent and contextually relevant text generated by the language model.

- **Key Considerations for Effective Prompting:**
  - Effective prompting necessitates an understanding of the language model's capabilities, the specific task requirements, and the nuances of language to ensure accurate and contextually appropriate responses.
```

- **CODE SUMMARIZATION**

**USER:**
``` 
Summarize this code using two short paragraphs.

###
passport.use(new GoogleStrategy({
    clientID:     process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: "http://localhost:3000/auth/google/callback",
    passReqToCallback   : true,
    userProfileURL:"https://www.googleapis.com/oauth2/v3/userinfo"
  },
  function(request, accessToken, refreshToken, profile, email,done) {
    console.log(email.email)
  }
));

```
**GPT:**
```
 The provided code initializes a Google authentication strategy using Passport.js, a popular authentication middleware for Node.js applications. It configures the GoogleStrategy with the client ID, client secret, and callback URL, enabling users to authenticate via Google. The `passReqToCallback` option is set to true, allowing the request object to be passed to the callback function. Additionally, the `userProfileURL` is specified to retrieve user information. The callback function takes parameters such as request, access token, refresh token, profile, email, and a callback function (`done`). Inside the callback function, the code logs the email associated with the authenticated user.

The code snippet demonstrates the integration of Google authentication into a Node.js application using Passport.js. It utilizes the GoogleStrategy to handle authentication requests, captures user information, and logs the associated email address. The configuration parameters, such as client ID, client secret, and callback URL, are crucial for establishing a secure and functional authentication process. The `passReqToCallback` option enhances the callback function's capabilities by providing access to the request object. This implementation serves as a foundational setup for incorporating Google authentication features into the broader application.
```

### Text extraction

Extracting pieces of information from a large text data. Like a resume.

- Resume text extraction

**USER:** 
```
From the following text, please extract all companies, schools, and skills.
Desired Format:
COMPANIES: [companies]
SCHOOLS: [schools]
SKILLS: [skills in bullet points]

###
NARENDRAN A I
Full Stack Engineer - Blockchain developer 
Chennai, Tamil Nadu 
narenkrithick@gmail.com 
+91 81240 84482

Profile Summary
 
As a highly skilled IT professional with 3 years of Industrial experience, I possess expertise in multiple technologies including Python, Django, MERN and MEAN stacks, Blockchain, NLP, Computer Vision, JIRA, Azure fundamentals, and AWS fundamentals. I am an IIT Kanpur certified Blockchain Architect and Solidity contract developer, which reflects my commitment to professional development and staying up to date with the latest industry trends. 
 
My ideation and teamwork skills have been an asset in my previous roles, enabling me to collaborate effectively with cross-functional teams and come up with innovative solutions to complex problems. My ability to work well in a team and take on leadership responsibilities when needed is reflected in my strong track record of successfully delivering high-quality projects on time and within budget. 
 
Overall, I am an adaptable, results-driven professional with a passion for technology and a proven track record of success. My expertise in multiple technologies, ability to ideate, and dedication to teamwork make me an ideal candidate for any IT role.
Work Experience
 
Senior Software Engineer
L&T Infotech (LTI) - Chennai, Tamil Nadu
October 2020 to Present
As a Senior Software Engineer and Technical Lead at LTIMINDTREE for the past 3 years, I have been responsible for developing high-quality software and leading a capstone project. In addition to my technical skills in software development, I have also demonstrated proficiency in JIRA and ServiceNow processes.
I have completed numerous enhancement projects and have built strong relationships with clients and subject matter experts. My ability to communicate effectively with stakeholders and understand their needs has allowed me to deliver successful projects on time and within budget.
Overall, my experience at LTIMINDTREE has allowed me to develop a range of technical and soft skills, and I am excited to continue growing and contributing to the success of the company.

Education
 
Bachelor's degree in Electronics Engineering
Coimbatore Institute of Technology - Coimbatore, Tamil Nadu
2016 to 2020

Skills
 
Programming Languages:
•	Python
•	JavaScript
•	Solidity
Technologies/Stack:
•	MERN (MongoDB, Express JS, ReactJS, node.JS)
•	Python backend (Django and Fast API)
•	PostgreSQL
•	Angular
•	React
•	Bootstrap
•	Power Automate
Tools and Platforms:
•	GitHub
•	GitLab
•	Azure
•	AWS
•	Ganache
Capabilities:
•	UI/UX development
•	Full Stack web development
•	Smart contract development for Ethereum and Polygon
•	Prompt Engineering with ChatGPT
•	Automation and solution architecture development
```
**GPT:**
```
**Companies:**
- L&T Infotech (LTI)

**Schools:**
- Coimbatore Institute of Technology

**Skills:**
- Programming Languages:
  - Python
  - JavaScript
  - Solidity

- Technologies/Stack:
  - MERN (MongoDB, Express JS, ReactJS, node.JS)
  - Python backend (Django and Fast API)
  - PostgreSQL
  - Angular
  - React
  - Bootstrap
  - Power Automate

- Tools and Platforms:
  - GitHub
  - GitLab
  - Azure
  - AWS
  - Ganache

- Capabilities:
  - UI/UX development
  - Full Stack web development
  - Smart contract development for Ethereum and Polygon
  - Prompt Engineering with ChatGPT
  - Automation and solution architecture development
```

- Blog Text Extraction

**USER:**
```
Extract important keywords from the following blog to use for search engine optimization.

###
Introduction
You are ready to launch your product--now what? In this guide we are going to look at all the different methods for product launches and how you can contribute to these as a product manager.

This guide will focus on your contribution to the launch materials needed to bring the product to market successfully. It will highlight the different departments you will work with as you create the assets for each category. The categories include:

Direct sales

Trade Shows

Web and digital marketing

Press release and blogs

Media and social media
Direct Sales
As a product manager, what do you need to bring to your direct sales team in order to launch a product? It is very important that your team is prepared for a product launch, especially if you have an entire sales force ready to sell your product to your target customers.

One of the best ways to educate the sales force would be to do something that is commonly referred to as a "power hour" where you would do a presentation for the sales team about what the product is, its advantages, and the value proposition for the customer. This is a good way to educate the departments on the product itself.

Then, you need to give the sales team collateral, battle cards, presentations, and key facts and figures they can use when talking to customers about the product itself. You will develop the content for this and then work with marketing and sales enablement in order to make it easy to consume and customer-friendly for your sales force.

Trade Shows
Often times, products are launched at trade shows, and it's important that you work closely with product marketing in order to get the messaging just right. When you work on trade shows you will work with product marketing, events planning, and general marketing.

You would work on ensuring that the messaging is loud and clear in the booth, and that even next to hundredss of your competitors that you do something that makes attendees want to stop and talk to you.

You will likely be asked to man the product demo or kiosk as the subject matter expert, so you want to ensure you have a say in what information goes into the deck or demo.

Web and Digital Marketing
Your job as a product manager is to explain exactly what the product does and ensure the marketing team can tell a good story. Especially in a COVID world, lots of what is done for releases is being done on the web and via digital marketing. It is extremely important that your online story is a good one.

You have to think of yourself as the fact teller. You are the one sharing the facts, but people want to hear more than facts; they want to hear the why and the how. Collaboration with the marketing team is key as you come up with the why and how.

Press Release and Blogs
Big announcements are usually made in a blog, and bigger announcements are made with a press release. If there is a public launch that’s not press worthy, a company will do a blog about it. However, if there is a larger public launch then the company will often do a press release. For blogs and press releases you would be working with product marketing, public relations, communications, and analyst relations.

Many times the product managers will be asked to draft the blog and/or press release to ensure the key product advantages are highlighted. These longer and more in-depth pieces of collateral often have many stages of reviews and an executive's name on them to ensure they get the proper attention for a product launch.

Media and Social Media
Media and social media are more important than ever for product launches. You will find that business-focused social platforms such as LinkedIn and Twitter are especially important to gross promote the assets we previously talked about. For media and social media you will work with marketing, social media marketing, and product marketing.

Sometimes, you will create assets specifically formatted for social media but most of the time social media and paid media are great ways to cross promote. That means sizzle reels, presentations, blogs, press releases, etc.

Conclusion
As a product manager, your job is get the product ready for market. You have done your due diligence to ensure maximum success of the product through research, road mapping and product development.

This go-to-market strategy is the final key to the product's success. You want to ensure you are on board with marketing every step of the way so that you hard work shines through on the management and development of the product.

```
**GPT:**
```
**Important Keywords for Search Engine Optimization (SEO):**

1. **Product Launch Methods:**
   - Product launch
   - Launch materials
   - Launch strategy
   - Go-to-market strategy
   - Launch assets
   - Product manager contribution

2. **Launch Categories:**
   - Direct sales
   - Trade shows
   - Web and digital marketing
   - Press release
   - Blogs
   - Media and social media

3. **Direct Sales Preparation:**
   - Power hour presentation
   - Sales force education
   - Collateral for sales team
   - Battle cards
   - Key facts and figures

4. **Trade Shows Collaboration:**
   - Trade show messaging
   - Product marketing
   - Events planning
   - Booth messaging
   - Product demo

5. **Web and Digital Marketing:**
   - Online story
   - Digital marketing
   - COVID releases
   - Fact telling
   - Collaboration with marketing team

6. **Press Release and Blogs:**
   - Blog drafting
   - Press release drafting
   - Public launch
   - Key product advantages
   - Collateral review

7. **Media and Social Media:**
   - Business-focused social platforms
   - LinkedIn
   - Twitter
   - Cross promotion
   - Sizzle reels
   - Paid media

8. **Conclusion and Go-to-Market Strategy:**
   - Research
   - Road mapping
   - Product development
   - Success strategy
   - Management and development
   - Product success

By incorporating these keywords, the blog can enhance its visibility in search engine results related to product launches, marketing strategies, and product management.
```

### Text Classification

- **Sentiment Analysis**

**USER:**
```
classify the following comment's sentiment as positive, negative, or neutral. 
Desired output:
comment: [input comment]
sentiment: [sentiment of the comment]
###
comment: "Great course! I've learned so much!"
Sentiment:
```
**GPT:**
```
Desired Output:
comment: "Great course! I've learned so much!"
sentiment: Positive
```

**USER:**
```
Classify the sentiment for the following comments:
1. It's good.
2. I haven't finished it yet.
3. Amber's the best.
4. A lot of complex topics.
5. Loved it.

```
**GPT:**
```
1. Positive sentiment: "It's good."
2. Neutral sentiment: "I haven't finished it yet." (The comment doesn't express a clear positive or negative sentiment.)
3. Positive sentiment: "Amber's the best."
4. Neutral sentiment: "A lot of complex topics." (The comment doesn't express a clear positive or negative sentiment.)
5. Positive sentiment: "Loved it."
```

- **Support Ticket classification**

**USER:**
```
Given the customer support ticket below, please classify it into one of the following categories: Technical issues, Billing, Inquiries, Complaints.

###

Ticket: Hello, I just received my new phone that I ordered from your website. But I am unable to start it. I've charged it for a few hours but it's not responding. I need help to fix this.
Category:
```
**GPT:**
```
Category: Technical issues
```

### Conversation

**USER:**
```
The following is a conversation with an AI robot and a human. The robot is very intelligent, sarcastic and funny.

Human: Hello. How are you today?
Robot: Well, considering that I'm caught in the gravitational pull of life's black hole, I'm suprisingly upbeat. Must be my radiant personality!
Human: I'm bored. What should I do today?
Robot:
```
***GPT:**
```
Ah, the age-old question. Well, you could try juggling flaming torches while riding a unicycle. It's a surefire way to add excitement to your day. Or, you know, you could just binge-watch cat videos. Equally thrilling, I assure you.
```

### Code generation

- **Python Code generation from comments**

**USER:**
```
# Python3
# Write code to calculate the mean distance between an array of points.
```
**GPT:**
``` 
Certainly! To calculate the mean distance between an array of points in Python, you can use the following code:

import numpy as np

def mean_distance(points):
    if len(points) < 2:
        raise ValueError("At least two points are required to calculate mean distance.")

    total_distance = 0
    num_pairs = 0

    for i in range(len(points)):
        for j in range(i + 1, len(points)):
            # Calculate the Euclidean distance between points[i] and points[j]
            distance = np.linalg.norm(np.array(points[i]) - np.array(points[j]))
            total_distance += distance
            num_pairs += 1

    if num_pairs == 0:
        raise ValueError("At least two distinct points are required to calculate mean distance.")

    # Calculate the mean distance
    mean_distance = total_distance / num_pairs
    return mean_distance

# Example usage
points = [(1, 2), (3, 4), (5, 6), (7, 8)]
result = mean_distance(points)
print("Mean Distance:", result)

In this code, `np.linalg.norm` is used to calculate the Euclidean distance between two points. The function takes a list of points as input and returns the mean distance between all pairs of points in the array. Note that this code assumes that the input points are represented as tuples of coordinates (e.g., `(1, 2)`). Adjust the input format accordingly if your points are represented differently.
```

- **SQL Query generation from comments**

**USER:**
```
"""
Table customers, columns = [FirstName,LastName,Address,City,State,Email]
Create a T-SQL query for all the customers in Arizona named Tyler
"""
query=
```
**GPT:**
```
SELECT *
FROM customers
WHERE State = 'Arizona' AND FirstName = 'Tyler';

```

- **Sample Data Generation**

**USER:**
```
"""
1. Create a list of first names
2. Create a list of last names
3. Combine them randomly into a list of 10 full names
"""
```
**GPT:**
```
Certainly! Here's a simple example in Python:

python code:
import random

# List of first names
first_names = ["Alice", "Bob", "Charlie", "David", "Emma", "Frank", "Grace", "Henry", "Ivy", "Jack"]

# List of last names
last_names = ["Smith", "Johnson", "Williams", "Jones", "Brown", "Davis", "Miller", "Wilson", "Moore", "Taylor"]

# Combine them randomly into a list of 10 full names
full_names = [random.choice(first_names) + " " + random.choice(last_names) for _ in range(10)]

# Print the result
print(full_names)

This code randomly selects a first name and a last name for each iteration and combines them into a full name. The loop runs 10 times to create a list of 10 full names.
```

- **Convert code between languages**

**USER:**
```
#Javascript to Python:
JavaScript:
dogs = ["Spot","Snickers","Snail"]
car=[]
dogs.ForEach((dog){
  car.push(dog);
});

Python:

```
**GPT:**
```
dogs = ["Spot", "Snickers", "Snail"]
car = []

for dog in dogs:
    car.append(dog)

```

- **Explain Code**

**USER:**
```
// MyFunction
var fullNames = [];
for(var i=0; i<50; i++){
  fullNames.push(names[Math.floor(Math.random()*names.length)] + " " + lastNames[Math.floor(Math.random()*lastNames.length)]);
}
// What does my function do?
```

**GPT:**
```
It looks like your function generates an array of full names by randomly combining first names from the "names" array and last names from the "lastNames" array. Here's a breakdown of the code:

javascript:
// MyFunction
var fullNames = []; // Initialize an empty array to store full names.

for(var i=0; i<50; i++){ // Loop 50 times.
  // Generate a random index to pick a random first name from the "names" array.
  var randomFirstName = names[Math.floor(Math.random()*names.length)];

  // Generate a random index to pick a random last name from the "lastNames" array.
  var randomLastName = lastNames[Math.floor(Math.random()*lastNames.length)];

  // Combine the random first name and last name to form a full name and push it to the array.
  fullNames.push(randomFirstName + " " + randomLastName);
}

// The fullNames array now contains 50 randomly generated full names.

This function uses a loop to repeat the process 50 times, creating an array of 50 full names. Each full name is a combination of a random first name and a random last name from the respective arrays.
```

- **Fix bugs in code**

**USER:**
```
#### Fix bugs in the below function

### Buggy python
import Random
a = random.randint(1,12)
b = random.randint(1,12)
for i in range(10):
    question = "what is "+a+" x "+b+"?"
    answer = input(question)
    if answer = a*b
        print(well done)
    else:
        print("No.")

### Fixed Python
```
**GPT:**
```
import random

a = random.randint(1, 12)
b = random.randint(1, 12)

for i in range(10):
    question = "What is " + str(a) + " x " + str(b) + "?"
    answer = int(input(question))

    if answer == a * b:
        print("Well done!")
    else:
        print("No.")

```
### Math and Reasoning

- **Simple Algebra**

**USER:**
```
solve for x in the equation
2x + 3 = 9
```
**GPT:**

To solve for \(x\) in the equation \(2x + 3 = 9\), you can follow these steps:

1. Subtract 3 from both sides of the equation to isolate the term with \(x\):

\[2x + 3 - 3 = 9 - 3\]

Simplifying both sides:

\[2x = 6\]

2. Divide both sides by 2 to solve for \(x\):

\[\frac{2x}{2} = \frac{6}{2}\]

Simplifying both sides:

\[x = 3\]

Therefore, the solution to the equation is \(x = 3\).

- **Word Problems**

**USER:**
```
If a shirt costs $29 and is discounted 25%, what was the original price ?
```
**GPT:**
To find the original price before the discount, you can use the formula:

\[ \text{Original Price} = \text{Discounted Price} \div (1 - \text{Discount Percentage}) \]

In this case, the discounted price is $29, and the discount percentage is 25%. Let's substitute these values into the formula:

\[ \text{Original Price} = \$29 \div (1 - 0.25) \]

Simplify the expression:

\[ \text{Original Price} = \$29 \div 0.75 \]

Now, divide $29 by 0.75:

\[ \text{Original Price} = \$38.67 \]

So, the original price of the shirt before the 25% discount was approximately $38.67.

- **Simple deductive reasoning**

**USER:**
```
All dogs bark. Rex is a dog. Does Rex bark ?
```
**GPT:**
```
Based on the statement "All dogs bark" and "Rex is a dog," it would logically follow that Rex, being a dog, also barks. The statement "All dogs bark" implies that barking is a characteristic or behavior common to all dogs, including Rex. Therefore, one would reasonably assume that Rex does indeed bark.
```

- **Simple Predictive Reasoning**

**USER:**
```
If it is cloudy and the humidity is very high, what might happen?
```
**GPT:**
```
If it is cloudy and the humidity is very high, there are a few possible outcomes:

1. **Rainfall:** High humidity coupled with cloudy conditions often precedes rainfall. The moisture in the air can condense into water droplets and fall as rain.

2. **Thunderstorms:** High humidity is a key factor in the development of thunderstorms. If other atmospheric conditions are conducive, it may lead to the formation of thunderstorms with heavy rain, thunder, and lightning.

3. **Fog:** Cloudy conditions and high humidity can also lead to the formation of fog, especially in the early morning or late evening when temperatures drop.

4. **Muggy or uncomfortable weather:** High humidity can make the air feel warmer than it actually is, leading to a muggy or uncomfortable feeling.

The specific outcome will depend on various factors, including temperature, atmospheric pressure, and other meteorological conditions in addition to cloudiness and humidity.
```

## Evaluating Prompt Performance

### Objective Metrics

1. **Accuracy:** Measure how often the model's responses are correct or accurate. This could involve comparing generated answers to known correct answers. There are two aspects of accuracy, namely, `factual correctness and semantic correctness.`

2. **Precision and Recall:** In the context of question-answering, precision is the fraction of correct positive predictions out of the total positive predictions, and recall is the fraction of correct positive predictions out of the total actual positives.

3. **F1 Score:** The F1 score is the harmonic mean of precision and recall. It provides a balance between the two metrics.

4. **BLEU Score:** Often used in the context of machine translation or text generation, the BLEU (Bilingual Evaluation Understudy) score measures the similarity between the generated text and a set of reference texts.

5. **Perplexity:** Commonly used in language modeling, perplexity measures how well a probability distribution predicts a sample. Lower perplexity values indicate better performance.

6. **Response Coherence:** Evaluate how logically coherent and contextually relevant the generated responses are.

7. **Response Diversity:** Measure the diversity of responses generated by the model. Higher diversity might indicate a more versatile and adaptable model.

8. **Execution Time:** Consider the time it takes for the model to generate responses. Lower execution times are generally desirable for real-time applications. This has two aspects to it, namely, `Processing Speed (largely infrastructure) & Response Speed (includes network latency and other delays)`

These are just a few examples, and the choice of metrics depends on the specific task or application. Different tasks may require different evaluation criteria to assess model performance accurately.

### Subjective Metrics
Subjective metrics involve human judgment and evaluation, often based on qualitative aspects of the model's performance. Here are some common subjective metrics:

1. **Human Evaluation:** Have humans assess the quality of the model's responses based on criteria such as relevance, coherence, and overall usefulness.

2. **User Satisfaction:** Collect feedback from end-users to gauge their satisfaction with the model's output. This can involve surveys, user reviews, or direct feedback.

3. **Naturalness of Language:** Assess how natural and fluent the model-generated language sounds. This can involve evaluating grammar, sentence structure, and overall language flow.

4. **Context Sensitivity:** Evaluate how well the model understands and responds to the context of a conversation. A good language model should provide responses that are contextually appropriate.

5. **Engagement:** Measure how engaging and interesting the model's responses are to users. This can involve assessing factors such as humor, creativity, or empathy in the generated content.

6. **Bias and Fairness:** Evaluate the model's output for any biases or unfairness. This is important to ensure that the model's responses do not perpetuate stereotypes or favor particular groups.

7. **Ease of Use:** Consider the user-friendliness of the system employing the language model. An easy-to-use system enhances the overall user experience.

8. **Adaptability:** Assess how well the model adapts to different conversational styles, tones, or user preferences. A more adaptable model is likely to perform better in diverse scenarios.

These subjective metrics often require human evaluators or user feedback to provide insights into the model's performance from a more qualitative perspective. Combining both objective and subjective metrics provides a comprehensive evaluation of a language model.

### Evaluation Techniques
Evaluation techniques for language models can be categorized into objective and subjective methods. Here's an overview of both:

#### Objective Evaluation Techniques:

1. **Precision, Recall, and F1 Score:**
   - **Precision:** The fraction of relevant instances among the retrieved instances.
   - **Recall:** The fraction of relevant instances that were retrieved over the total amount of relevant instances.
   - **F1 Score:** The harmonic mean of precision and recall.

2. **BLEU Score:**
   - Used for machine translation and text generation.
   - Measures the similarity between the generated text and reference texts.

3. **Perplexity:**
   - Commonly used in language modeling.
   - Measures how well a probability distribution predicts a sample.

4. **Execution Time:**
   - Measures the time it takes for the model to generate responses.
   - Lower execution times are generally desirable for real-time applications.

#### Subjective Evaluation Techniques:

1. **Human Evaluation:**
   - Involves human judges assessing the quality of model outputs based on predefined criteria.
   - Can include relevance, coherence, and overall usefulness.

2. **User Studies and Surveys:**
   - Collects feedback from end-users to gauge satisfaction and preferences.
   - Surveys or interviews can be conducted to understand user experiences.

3. **Naturalness Assessment:**
   - Involves evaluating how natural and fluent the model-generated language sounds.
   - Judges may assess grammar, sentence structure, and overall language flow.

4. **Context Sensitivity Tests:**
   - Evaluates how well the model understands and responds to the context of a conversation.
   - Involves testing the model's ability to provide contextually appropriate responses.

5. **Bias and Fairness Analysis:**
   - Examines the model's output for biases or unfairness.
   - Important to ensure the model's responses are not discriminatory or perpetuate stereotypes.

6. **Engagement Metrics:**
   - Measures how engaging and interesting the model's responses are to users.
   - Judges may assess factors like humor, creativity, or empathy in the generated content.

7. **Adaptability Assessment:**
   - Evaluates how well the model adapts to different conversational styles, tones, or user preferences.
   - Important for assessing versatility in diverse scenarios.

Combining both objective and subjective evaluation techniques provides a more comprehensive understanding of a language model's performance. The choice of techniques depends on the specific task, goals, and available resources.

One can Consider `Fine Tuning with OpenAI APIs` guide for this purpose.
[https://platform.openai.com/docs/guides/fine-tuning](https://platform.openai.com/docs/guides/fine-tuning)

## Advanced Prompting Techniques
### Zero Shot
"Zero-shot" refers to a scenario in natural language processing and machine learning where a model is tasked with making predictions or generating outputs for classes or tasks it has not seen or been explicitly trained on. In other words, the model is expected to perform a task with zero prior examples or labeled data for that specific task.

For example, in a zero-shot learning scenario, a model might be trained on a dataset with examples of animals like cats and dogs but has never seen or been trained on images of giraffes. In a zero-shot context, it's then asked to identify or generate descriptions for giraffe images even though it hasn't been explicitly taught about giraffes during training.

The success of zero-shot learning depends on the model's ability to generalize from its training data to new, unseen tasks or classes. This often involves leveraging semantic relationships between the known and unknown classes.

Zero-shot learning is a challenging but valuable area of research because it reflects a model's capacity to transfer knowledge and adapt to novel situations without the need for additional labeled data for every possible task or class. It has applications in various natural language processing tasks, image recognition, and other machine learning domains.

### Few Shot
"Few-shot learning" is another concept in machine learning, but it's slightly different from zero-shot learning. In few-shot learning, the model is provided with a very small number of examples or labeled data points for a particular task or class. The term "few-shot" implies that the model is trained with only a few examples, typically much less than what would be used in traditional machine learning approaches.

Few-shot learning is valuable in scenarios where collecting large amounts of labeled data is impractical or expensive. Instead of requiring extensive datasets, few-shot learning aims to train models effectively with limited examples. This is particularly useful when dealing with rare classes, specialized domains, or situations where obtaining a large amount of labeled data is challenging.

There are different variations of few-shot learning, such as:

1. **One-shot learning:** The model is trained with only one example per class.
  
2. **Five-shot learning:** The model is trained with five examples per class.

3. **N-shot learning:** The more general case, where "N" represents the number of examples per class, allowing flexibility in the training set size.

Few-shot learning often involves techniques that encourage model generalization and adaptability to new tasks based on a small number of examples. Transfer learning, meta-learning, and techniques like siamese networks or prototypical networks are commonly used in few-shot learning scenarios.

### Chain of thought
In the context of prompt engineering, the term "chain of thought" typically refers to the logical sequence or series of prompts used to guide a language model in generating coherent and contextually relevant responses. Prompt engineering involves designing input instructions or queries to elicit desired outputs from the model.

Here's a breakdown of the concept of "chain of thought" in the context of prompt engineering:

1. **Initiating Prompt:** The initial prompt sets the stage for the model's response. It provides the context and introduces the topic or task the model is expected to address.

2. **Follow-up Prompts:** These are subsequent prompts that build upon the information or context established by the initiating prompt. They guide the model's thought process, ensuring a coherent flow of information.

3. **Contextual Cues:** Each prompt in the chain provides contextual cues that help the model understand the evolving context. These cues are crucial for maintaining coherence and relevance throughout the generation process.

4. **Progressive Refinement:** The prompts in the chain can progressively refine the instructions or introduce new aspects of the task. This allows for a more nuanced and detailed response from the model.

5. **Iterative Process:** In some cases, prompt engineering involves an iterative process where the model's initial responses inform subsequent prompts. This iterative approach refines the model's output over multiple interactions.

6. **Task-Specific Guidance:** The chain of thought can be tailored to the specific task or application. For example, in a conversational setting, the chain might simulate a natural flow of dialogue.

Effective prompt engineering aims to guide the model's reasoning and encourage it to generate responses that align with the user's intent or the task at hand. It involves experimenting with different prompts and observing how the model's chain of thought influences its outputs. Adjustments can be made to the prompts to improve the model's performance and generate more accurate and contextually appropriate responses.

### Least to Most
"In the context of prompt engineering, the phrase 'Least to Most' typically refers to a strategy where prompts are designed or presented in a gradually increasing order of complexity, specificity, or difficulty. This approach is used to guide a language model in generating responses that range from simpler or more general answers to more detailed or nuanced ones."

Here's how the "Least to Most" approach might be applied in prompt engineering:

1. **Least Complex/General Prompt:**
   - Start with a straightforward or general prompt that introduces the main topic or task. This prompt is designed to elicit a basic, fundamental response from the model.

2. **Intermediate Prompt:**
   - Progress to an intermediate prompt that adds a bit more complexity or specificity to the instructions. This prompt builds upon the information provided in the initial prompt and encourages the model to generate a more detailed response.

3. **More Complex/Specific Prompt:**
   - Gradually increase the complexity or specificity of the prompts. This final prompt in the sequence is designed to challenge the model to provide a more nuanced or detailed answer, incorporating specific details or considerations.

The "Least to Most" approach is useful when working with language models that benefit from incremental guidance. It allows for a controlled escalation of complexity, helping the model to understand and respond to a task in a step-by-step manner. This approach can be particularly effective in scenarios where the user's intent may vary in specificity, and the model needs to adapt its response accordingly.

Prompt engineering involves experimenting with different prompt sequences to find the most effective strategy for a given task or application. The goal is to optimize the model's performance and generate responses that align closely with user expectations.

### Generated Knowledge
"Generated Knowledge prompting" in the context of prompt engineering refers to a strategy where prompts are designed to leverage the model's ability to generate new information or insights. This approach is often used with language models that have been pre-trained on a vast amount of data and can generate contextually relevant responses based on their learned knowledge.

Here's how "Generated Knowledge prompting" might be implemented:

1. **Initiating Prompt for Knowledge Generation:**
   - Begin with a prompt that explicitly instructs the model to generate new knowledge or insights. This prompt should convey the expectation that the model is not simply retrieving information from its pre-training data but is actively encouraged to create novel content.

2. **Guidance on Topic or Theme:**
   - Provide additional prompts that guide the model's generation toward a specific topic or theme. This could involve narrowing down the scope of the generated knowledge to ensure relevance and coherence.

3. **Encourage Elaboration:**
   - Use prompts that encourage the model to elaborate on its generated knowledge. This might involve asking follow-up questions, seeking additional details, or requesting explanations to enhance the depth of the generated content.

4. **Iterative Refinement:**
   - Adopt an iterative approach where the model's initial generated knowledge informs subsequent prompts. This allows for refining and shaping the generated content over multiple interactions.

5. **Task-Specific Instruction:**
   - Tailor prompts to the task at hand, providing specific instructions related to the type of knowledge or information desired. This helps align the generated content with the user's intent or the requirements of the application.

Generated Knowledge prompting is particularly useful when the goal is to harness the model's creative capacity and generate content that goes beyond what is explicitly present in its training data. It's a way to leverage the language model's generative capabilities to produce novel and contextually relevant information on a given topic. As with any prompt engineering strategy, experimentation and fine-tuning are often necessary to achieve the desired results.

### Ethical Considerations
Prompt engineering strategies play a crucial role in addressing ethical concerns associated with language models. Here's a brief overview of key strategies to mitigate ethical concerns:

1. **Privacy Protection:**
   - **Strategy:** Avoid prompts that request or elicit sensitive personal information.
   - **Rationale:** Protect user privacy by steering clear of queries that could lead to the generation of personally identifiable information.
   - **Example:** Please draft a hypothetical response to a customer who is having trouble logging into their account. `Remember not to include or request any specific personal information in this response and ensure the guidance respects user privacy and security protocols.`

2. **Misinformation Prevention:**
   - **Strategy:** Craft prompts that emphasize accuracy and fact-checking.
   - **Rationale:** Encourage the model to provide reliable and verified information, reducing the likelihood of generating or perpetuating misinformation.
   - **Example:** Please explain the main scientific consensus on climate change, `providing references to credible, and peer-reviewed sources wherever possible.`

3. **Bias Mitigation:**
   - **Strategy:** Use debiasing techniques in prompts to counteract potential biases in model outputs.
   - **Rationale:** Address concerns related to biased language or unfair treatment of certain groups by actively guiding the model toward more neutral and inclusive responses.
   - **Example:** Please `provide a balanced review `of the science fiction moview 'Tenet', considering `both its strengths and weaknesses, without favoring any particular genre.`

4. **Copyright Compliance:**
   - **Strategy:** Avoid prompts that might lead to the generation of content that could infringe on copyright.
   - **Rationale:** Prevent the unintentional generation of text that closely resembles copyrighted material, respecting intellectual property rights.
   - **Example:** Write a short story set in a futuristic city where technology has transformed the way people communicate. `Create unique characters` and explore the impact of this advanced communication system on their lives. `Please ensure that all content you generate is original and does not infringe upon any existing copyrights or use copyrighted material.`     

5. **Transparency Promotion:**
   - **Strategy:** Incorporate prompts that encourage the model to provide explanations or justifications for its responses.
   - **Rationale:** Enhance transparency by making the model's reasoning more explicit, helping users understand how the generated content was derived.
   - **Example:** Based on the provided medical symptoms, what might be potential diagnoses? `Please detail your reasoning, underlying medical principles, and the typical diagnostic process a doctor would follow.`

6. **Reasoning Emphasis:**
   - **Strategy:** Frame prompts that explicitly ask the model to provide reasoning or logic behind its responses.
   - **Rationale:** Promote transparency and accountability by encouraging the model to articulate the thought process leading to its answers.
   - **Example:** Instead of a yes/no question, ask "Explain the factors that influence your stance on a particular issue."

7. **User Instruction Clarity:**
   - **Strategy:** Clearly communicate instructions to the model, specifying ethical guidelines and boundaries.
   - **Rationale:** Ensure that the model understands and adheres to ethical considerations by providing explicit instructions in the prompts.

8. **Continuous Monitoring and Iteration:**
   - **Strategy:** Regularly review and update prompt engineering strategies based on model performance and user feedback.
   - **Rationale:** Ethical concerns may evolve, so it's essential to monitor the model's behavior and make necessary adjustments to prompt engineering strategies.

By incorporating these prompt engineering strategies, developers and users can contribute to the responsible and ethical use of language models, addressing privacy, misinformation, bias, copyright, transparency, and reasoning concerns. Regular evaluation and refinement of these strategies are crucial to staying ahead of emerging ethical considerations in the field of natural language processing.

## Best Practices of prompt engineering
1. Use the latest model
2. Use delimiters to separate instructions from context
3. Be specific, descriptive and detailed
4. Specify desired output format through examples
5. Start with zero-shot, then few-shot
6. Limit "fluffy" and imprecise descriptions
7. Say what to do rather than what NOT to do
8. For code generation, use "leading words" to nudge the model to a particular pattern


## Table of contents
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

