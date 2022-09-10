<h1 align="center"> Software module for answering questions on processes </h1>

<div align="center">

[![](https://img.shields.io/badge/Open%20in%20collab-gray?style=for-the-badge&logo=google%20colab)](https://colab.research.google.com/drive/1kk07RGPRDV3LaoGbzccBKV6YowgcrYNm?usp=sharing)
[![](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)](https://www.python.org/)
[![](https://img.shields.io/badge/%F0%9F%A4%97%20HuggingFace-white?style=for-the-badge)](https://huggingface.co/)

</div>

<br />

## **:information_source: About :information_source:**

This is a software module developed to answer questions about linear business processes using sophisticated [machine learning](https://en.wikipedia.org/wiki/Machine_learning) techniques.

This module represents a final thesis written in the form of a program project for the [Faculty of Informatics in Pula](https://fipu.unipu.hr/) under the mentorship of [Nikola Tanković](https://www.tankovic.me/). The topic is based on the [program engineering](https://ntankovic.unipu.hr/pi) course.

It was developed using [Google Colaboratory](https://colab.research.google.com/) running [Python 3.7](https://www.python.org/downloads/release/python-370/) notebook. It uses different [HuggingFace models](https://huggingface.co/models), frameworks like [TensorFlow](https://www.tensorflow.org/) and [PyTorch](https://pytorch.org/) and datasets like

<br />

## **:scroll: Features :scroll:**

For a given JSON object that represents a set of business processes (all being linear in the nature) and training data that is oriented around these processes, the module is capable to answer the following question (for a specific business process):

-   P1: What are all the tasks of the process
-   P2: What is the duration of the process?
-   P3: What is the description of an individial task?
-   P4: What is the duration of a certain task?
-   P5: What task comes after the specific task?
-   P6: Out of domain (none of the above)

<br />

## **:robot: How does it work :robot:**

For a certain question, the module goes through few logic units.

First, the user intent recognition module. This module was designed to categorize a question into one of the 6 predetermined question domains (6th being an "empty domain" for questions that do not belong to any other domain). The module was developed by [Aldo Ferlatti](https://github.com/AldoF95) ([source](https://github.com/AldoF95/intent_recognition_masters_thesis)).

There exist 2 types of domains: _simple_ and _complex_. After the domain of the question is figured out, main module checks whether the domain is a _simple_ or a _complex_ one. If the domain is a _simple_ domain, then all the data is already available via JSON model. After extracting the data, the main module prints out a message with the data. On the other hand, if the domain is a _complex_ domain, further logic is required. _Complex_ domains require some additional extraction from the question itself, so the next logic module is data extraction module.

The data extraction module is composed of few different submodules, one being named entity recognition module (NER module). NER module is finetuned to recognise the names of the tasks and mark them as an "Organisation" entity. NER module was built by [Deepak moonat](https://github.com/dmoonat) ([source](https://github.com/dmoonat/Named-Entity-Recognition/blob/main/Fine_tune_NER.ipynb)). By doing this, the module "zooms into" the original question to find the most matching part of the question to one of the task names. Once "zoomed in" (or extracted), next submodule transforms the result of the previous submodule into multidimenstional vector space and performs a cosine similarity among all the task names to find which of the task names the extracted data matches the most. For this task, [all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) sentence transformer was used. This module yields a number, indicating what is the list index of the task being referenced in the original question (assuming that a _complex_ domain always contains a task name in the question).

Finally, after the additional data extraction (specifically the task number), the rest of the data is available via JSON model, which is then printed out by the main module.

This process is repeated until a user inputs a letter "q" indicating the termination of the interaction.

<br />

### Example of the JSON model:

```py
json = [
    {
        "name": "Foo Bar",
        "phases": [
            {
                "name": "Foo",
                "alias": ["<other>", "<names>", "<for>", "<foo>"]
                "description": "<your description goes here>",
                "duration": "<Foo duration goes here>",
            },
            {
                "name": "Bar",
                "alias": ["<alias are optional>"]
                "description": "...",
                "duration": "...",
            },
        ],
        "duration": "<duration of the Foo Bar>",
    },
    {
        "name": "Fry the eggs",
        "phases": [
            {
                "name": "Set up the oven",
                "alias": ["Start", "First step", "Oven"],
                "description": "Turn on your oven...",
                "duration": "few seconds",
            },
            ...
            {
                "name": "Bon appetite!",
                "alias": ["Final step", "ready meal"],
                "description": "Your meal is now ready...",
                "duration": "None",
            },
        ],
        "duration": "5-10 minutes",
    },
]
```

### Demo sample (translated):

```
To end the conversation, enter 'q'

>>> how long does the practice last
The process 'practice' takes 2 month

>>> I need to do the practice
The process stages for the 'Practice' process are: selecting preferences, filling out the application form and submitting the practice log

>>> how long does it take to select preferences
The 'preference selection' process takes 1 month

>>> Who is Joe?
Unfortunately, I do not understand your question

>>> q
Good bye! ( ^_^)/
```

<br />

## **:computer: Running it on local machine :computer:**

You can run the code on Google Colaboratory platform ([click me](https://colab.research.google.com/drive/1kk07RGPRDV3LaoGbzccBKV6YowgcrYNm?usp=sharing)). Initial execution of the code should last for ~30 min on CPU and ~5 min on [GPU mode](https://www.tutorialspoint.com/google_colab/google_colab_using_free_gpu.htm).

If you however want to download the notebook and run it localy, there is a quick [4 minute video tutorial on YouTube](https://www.youtube.com/watch?v=h1sAzPojKMg) to help you set up Jupyter Notebooks and all the necessary environment. Clone the repository (or [download ZIP](https://codeload.github.com/rkrstacic/Software-module-for-answering-questions-on-processes/zip/refs/heads/main)), and run the notebook. You may need to `pip install` some libraries since the working notebook was developed in Google Colaboratory, which has some common libraries preinstalled.
