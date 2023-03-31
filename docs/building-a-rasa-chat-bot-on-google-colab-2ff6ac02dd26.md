# 在 Google Colab 上构建 Rasa 聊天机器人

> 原文：<https://medium.com/analytics-vidhya/building-a-rasa-chat-bot-on-google-colab-2ff6ac02dd26?source=collection_archive---------3----------------------->

![](img/945d795570b9e895d15b23f10ae22896.png)

鸣谢:istockphoto.com/soberP

Rasa 是一个用于个性化对话的对话式人工智能平台，它是一个使用 python 和自然语言理解来构建人工智能聊天机器人的工具。它提供了一种训练个性化聊天机器人的平滑方法，这些聊天机器人可以在几个平台上在线托管。

对话聊天机器人如今已经变得非常普遍，被公司广泛用于向需要帮助或信息的客户提供即时反馈。他们减少了恼人的等待时间，而这种等待时间曾经是查询得到答复的标准。

我要训练的聊天机器人的目标是回答刚到一个城市的旅行者在火车站会问的问题。Rasa 聊天机器人由两个组件组成，Rasa nlu 和 rasa core。Rasa nlu 是 Rasa 的一个组件，用于分类意图和提取实体(细节)，而 Rasa core 是一个组件，它从 Rasa nlu 获取结构化输入，并使用概率模型预测下一个最佳操作。更多信息可以从官方文档中获得。这个项目是在谷歌 colab 上运行的。

# **库和依赖关系**

安装 rasa_nlu[spacy]，rasa_core，nest_asyncio 和 ipython

```
!pip install nest_asyncio==1.3.3!pip install rasa_nlu[spacy]  #restart runtime after installation and rerun the cell.!pip install rasa_corepip install -U ipython #restart runtime after installation and rerun the cell.
```

导入 rasa_core、rasa_nlu 空间和系统

```
import rasa_coreimport rasa_nluimport spacyimport syspython = sys.executable
```

下载 en_core_web_md，一个中等大小的英文模型，在书面网络文本上训练。

```
!python -m spacy download en_core_web_md
```

# 拉萨·NLU

接下来是意图声明和每个意图的样本数据。之后，我训练一个模型，它能准确预测一个短语或句子的意图。

```
# Writing various intents with examples to nlu.md filenlu_md = """## intent:greet- hey- hello there- hi- hello there- good morning- good evening- how far- hey there- whats up- hey dude- goodmorning- goodevening- good afternoon## intent:goodbye- good by- later- good night- good afternoon- bye- goodbye- have a nice day- see you around- bye bye- see you later## intent:thanks- thanks- thank you- appreciated- gracias## intent:directions- how do i get to- i need to get to- is there a bus to- how i go take reach- I need directions to- how do i get a cab- where can i get a taxi- where can i get a bus-i need to get somewhere-I am going somewhere## intent:choosing_item- 1- 2- 3- 4- 5- 6- 7## intent:accomodation_enquiry- Where is the nearest hotel ?- is there a hotel near by ?- shey hotel they this area ?- Where i fit crash for here ?- i want to get to a hotel- i need to get to a hotel- where can i spend the night ?## intent: restaurant_enquiry- i need to get food- i am hungry- where is the restaurant ?- is there a restaurant ?- where can i get water ?- where i fit buy food for here ?- is there a food court nearby ?- where them they sell food ?- I want chop- where is the nearest shop?- where can i eat ?- i need to eat- are there eatries around ?- where can i buy snacks ?- I want to buy food## intent:toilet_enquiry- i neeed to pee- where is the rest room?- where is the toilet- is there a nearby toilet?- i they find toilet- where is the bathroom- i need to use the bathroom- where can i pee- i want peace- i am pressed"""%store nlu_md >nlu.md
```

为我们的管道和策略设置配置。回退阈值可以调整以适合个人使用。

```
# Adding the NLU components to the pipeline in config.yml fileconfig = """language: "en_core_web_md"pipeline:- name: "nlp_spacy"                   # loads the spacy language model- name: "tokenizer_spacy"             # splits the sentence into tokens- name: "ner_crf"                     # uses the pretrained spacy NER model- name: "intent_featurizer_spacy"     # transform the sentence into a vector representation- name: "intent_classifier_sklearn"   # uses the vector representation to classify using SVM- name: "ner_synonyms"                # trains the synonymspolicies:- name: "RulePolicy"# a confidence >= core_fallback_thresholdcore_fallback_threshold: 0.68                            # Confidence threshold for the `core_fallback_action_name` to apply.core_fallback_action_name: "action_default_fallback"     # The action will apply if no other action was predicted withenable_fallback_prediction: True                         # a confidence >= core_fallback_threshold"""%store config >config.yml
```

# 训练模型

导入“加载数据”和“配置”模块，加载训练数据文件和配置文件，并将其提供给训练器模块以训练模型。

```
# Import modules for trainingfrom rasa_nlu.training_data import load_datafrom rasa_nlu.config import RasaNLUModelConfigfrom rasa_nlu.model import Trainerfrom rasa_nlu import config# loading the nlu training samplestraining_data = load_data("nlu.md")trainer = Trainer(config.load("config.yml"))# training the nluinterpreter = trainer.train(training_data)model_directory = trainer.persist("./models/nlu", fixed_model_name="current")
```

测试模型。
目的是看它能多好地预测意图，并根据预测做出适当的反应。输出应该是意图及其置信度值。

```
# Testing the NLU model with an input messageimport jsondef pprint(o): print(json.dumps(o, indent=2))pprint(interpreter.parse("x"))
```

# Rasa 核心

这是项目的第二部分，我们将处理故事、动作和模板，以便我们的机器人可以适当地响应查询。一个故事是一条路径，包含意图和它们各自要采取的行动。动作是我们希望我们的机器人在识别意图后给出的响应。模板是我们存储所有动作的地方。插槽是我们的机器人的记忆，帮助它跟踪事件。

```
# Writing stories and saving it in the stories.md filestories_md = """##  Greeting path* greet- utter_greet## directions path* directions- utter_menu* choosing_destination- utter_destination_received## Enquires path* accomodation_enquiry- utter_enquiry## thanks path* thanks- utter_thanks## restaurant path* restaurant_enquiry- utter_restaurant_enquiry## toilet path* toilet_enquiry- utter_toilet_location## say goodbye* goodbye- utter_goodbye## thanks* thanks- utter_thanks## fallback rule* bot_fallback- action_default_fallback"""%store stories_md >stories.md
```

这个域是我们为聊天机器人链接所有文件的地方。uttar_default 动作是当所有可能意图的置信度值低于在回退策略下的配置文件中设置的阈值时要采取的动作。

```
# Writing all the intents,slots,entities,actions and templates to domain.ymldomain_yml = """intents:- greet- goodbye- directions- choosing_item- accomodation_enquiry- restaurant_enquiry- toilet_enquiry- thanksslots:group:type: textentities:- groupactions:- utter_greet- utter_menu- utter_destination_received- utter_enquiry- utter_toilet_location- utter_restaurant_enquiry- utter_goodbye- utter_thanks- utter_defaulttemplates:utter_greet:- text: "\n\n NRC BOT: \n\n Hey! Welcome to NRC Kubwa terminal . How may I help you ? \n\n \n\n You:"utter_menu:- text: "\n\n NRC BOT: \n\n There is a bus terminal outside the station, it's just accross the road.  You can board a bus to the following destinations: \n\n 1\. Wuse  2\. Airport 3\. Nyanya  4\. Zuba  5\. Asokoro  6\. Maitama  7\. Area one  8\. Central area  8\. Secretariat. \n\n Enter  the appropraite number to know when the next bus leaves the terminal. \n\n \n\n  You:"utter_destination_received:- text: "\n\n NRC BOT: \n\n The next bus leaves in 15 minutes, do have a safe trip. \n\n \n\n You:"utter_goodbye:- text: "\n\n NRC BOT: \n\n Thank you for choosing to transit with us. Waiting for your next visit. \n\n \n\n You:"utter_enquiry:- text: "\n\n NRC BOT: \n\n Take a right turn from the exit, there are several hotels at your service. \n\n \n\n You:"utter_toilet_location:- text: "\n\n NRC BOT: \n\n There is a toilet symbol on the extreme right from where you stand. \n\n \n\n You:"utter_restaurant_enquiry:- text: "\n\n NRC BOT: \n\n There is a food court filled with a variety of rastaurants at the extreme left after the entrance. \n\n \n\n You: "utter_thanks:- text: "\n\n NRC BOT: \n\n my pleasure \n\n \n\n You:"utter_default:- text: "\n\n NRC BOT: \n\n I'm sorry, I didn't quite understand that. Could you rephrase? \n\n You:""""%store domain_yml >domain.yml
```

在这里，我们导入我们的策略和代理模块。代理模块用于加载文件并传递给“代理”模型。用来自故事文件的数据训练“代理”模型。

```
# Import the policies and agentfrom rasa_core.policies import FallbackPolicy, MemoizationPolicy,KerasPolicyfrom rasa_core.agent import Agent# Initialize the model with `domain.yml`agent = Agent('domain.yml', policies=[MemoizationPolicy(), KerasPolicy(), ])# loading our  training dialogues from `stories.md`training_data = agent.load_data('stories.md')# Training the modelagent.train(training_data)#validation_split=0.0,#epochs=200)agent.persist('models/dialogue')
```

安装 h5py 包的 2.10.0 版，安装后重启运行时，重新运行所有以前的单元。

```
!pip uninstall h5py!pip install h5py==2.10.0
```

加载训练好的模型

```
#Starting the Botfrom rasa_core.agent import Agentagent = Agent.load('models/dialogue', interpreter=model_directory)
```

写一个函数来跟踪聊天机器人的输入，并在输入“停止”时停止输出。键入 stop 结束会话。

```
print("Your bot is ready to talk! Type your messages here or send 'stop'" + "\n\n" + "You:")while True: a = input() if a == 'stop': break responses = agent.handle_message(a) for response in responses: print(response["text"])
```

# 视频演示

您可以在我的 github repo 上获得完整的笔记本:

[](https://github.com/usmanbiu/rasa-chat-bot.git) [## GitHub - usmanbiu/rasa-chat-bot:在 google colab 上开发 rasa 聊天机器人

### 谷歌聊天机器人的开发。通过创建一个帐户，为 usmanbiu/rasa 聊天机器人的开发做出贡献…

github.com](https://github.com/usmanbiu/rasa-chat-bot.git) 

**参考文献:**

**用 Rasa 和 spaCy 构建聊天机器人:**[https://www . machine learning plus . com/NLP/chatbot-with-Rasa-and-spaCy/](https://www.machinelearningplus.com/nlp/chatbot-with-rasa-and-spacy/)

**Rasa 官方文档:**[https://rasa.com/docs/rasa/](https://rasa.com/docs/rasa/)