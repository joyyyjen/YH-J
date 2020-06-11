---
title: "[2019]Chatbot"
date: 2019-03-01T21:09:30-06:00
---
It's now running on my [Facebook Fan Page](https://www.facebook.com/beingbarbarianistough/) Send it a message and get started!

In this project, we are using **Dexter**, which is a platform people can create automated conversation. I will talk about data preparation, discussion, and challenge. 
The goal is to create a facebook fan page Chatbot in both Mandarin Chinese and English that help to manage fan page private message for Taiwanese Student Association (TSA).  
It should respond to messages related to:

1. sponsorship/ public relation /marketing cooperation 
2. New Student Orientation Event Info
3. frequent asked questions from new incoming students  
        
### 1. Data Preparation

Before I started writing the content of TSA bot, I thought it might be helpful to dig into the messages we received in the past. Therefore, I decided to download the facebook page message history. This can be done using Facebook Developers Graph API. And then we can use a GitHub python package ["FB-page-chat-download"](https://github.com/eisenjulian/fb-page-chat-download) to extract the data. Its output is a CSV file.

It follows three steps: data pre-processing, tokenization, and analysis

- data pre-preprocessing divides the chat into 4 categories: user questions in Chinese, TSA response in Chinese, users questions in English, and TSA responses in English. 
- Tokenization splits the sentences into words and saves nouns into a dictionary.
- Analyzation return questions and answering contain the 100 most frequent words.

To make the process more interesting, I draw a word cloud based on the most frequent word from the chat.
```python
from PIL import Image
import matplotlib.pyplot as plt
from wordcloud import WordCloud, ImageColorGenerator

def graph(freq):
    font = "/System/Library/Fonts/STHeiti Medium.ttc"
    wordcloud = WordCloud(font_path=font,width=2400, height=2400, margin=2).generate_from_frequencies(frequencies=freq)
    plt.figure(figsize=(20,10),dpi=300)
    plt.imshow(wordcloud, interpolation='bilinear')
    plt.axis("off")
    plt.show()
```
![img1](/TSA.english.png)
![img2](/user_eng.png)
![img3](/TSA_message.png)
![img4](/user_chat.png)

### 2. Discussion & Challenge
Dexter makes rule-based Chatbot easier to implement by using Regular Expression. 
```
+ Users Question
- Chatbot Response
```
We can deploy it on Facebook, Slack, Website Embedded, Twitter and etc. They have several functions such as insert buttons, links, media, HTTP get,  HTTP post and etc. 
One interesting basic function I want to talk about is **"topics"**. Topics allow us to organize the dialogue into subsections. In my case, I have mainly three topics "Information Sharing", "Upcoming Events", and "New Students Questions"
The default page is used as a menu page and people should go to the corresponding channel. The issue is, people don't like this idea. They rather spit out the question and stuck in the "out of vocab" loop. I started to think it's not a good idea to have topics.
One good thing about topics is that it is domain specific with the restricted environment and can do follow up answering. Some keywords might also be used in other topics.

```
+ (english|英文)
- What brings you here today? <send> I am not able to chat. I can "ONLY" take care of mainly three topics. Information Sharing for students, school administration, sponsors who are looking for promotion. TSA might help you advertise your event, research, job information. <send> Upcoming Events will tell you the information about our most recent event. <send> New Student Questions are frequent questions asked by the new incoming student. <send> You can also reply "help" to learn how to communicate with me =( ^buttons("Information Sharing", "Upcoming Events", "New Student Questions") If you want to see the buttons again, reply "menu".

+ information sharing
- What would you like to share with us?{topic=sharing}

+ upcoming events
- TSA New Student Orientation @ Taiwan is coming soon. \n Time and location\n is 5/25(Sat) at GIS NTU Convention Center from 9am-12pm. Feel free to ask me any question about it!{topic=coming-events}

+ new student questions
- Go ahead! I am listening =) I can help to answer questions about housing, dorm, transportation, environment, bank, entertainment, and other general information.{topic=faq}

```

I have some users who asked questions about transportation at "Information-Sharing" channel. And ended up getting "out of vocabulary again". The answer is the in new student question, but they just don't go there.
Therefore, I gave clear instruction on the purpose of each channel. There was one user who didn't even read and started asking questions.
Thus, I am thinking about removing the topics now... 

Another function we can use is to **capture the last message with a percentage sign**.
```
+ ([*]open[*]bank[*]|[*]bank[*])
- There are mainly 3 banks in Urbana Champaign. Busey Bank, PNC Bank, and Chase bank. <send> PNC is a partner of U of I. (And also a partner of TSA). Check their info here ^link("https://www.pnc.com/en/personal-banking/banking/student-banking/university-of-illinois.html") <send> We also recommend Chase, because it's a national bank. <send> Are you familiar with the types of Bank Account? (reply "yep" or "nah")

+ yep
- Do you need TSA taking you to the PNC bank

+ yes
% do you need tsa taking you to the pnc bank 
- We will post a sign-up link in late July or early August. Follow our facebook page~! 

+ no
% do you need tsa taking you to the pnc bank 
- When you go to open your account, take your passport and immigration documents(i-20) with you to serve as identification. 😊

#% are you familiar with the types of bank account
+ nah
- There are two basic types of bank accounts: checking accounts and savings accounts. <send> With a checking account you can deposit your money in the account and access those funds with a debit card

```
The reason I wrote "reply yep or nah" is that I did't want to conflict it with the following question "do you need tsa taking you to the pnc bank"
 % did not capture "Are you familiar with the types of Bank Account?" Is it because the message is a long paragraph?
 It also did not work if the questions contain buttons. 
 It's a very confusing function that I actually want to use more in the future.
 

 p.s. The free version of Dexter bot is limited to 10 ppl person. I asked a couple of new students and friends to help me test it. And it exceeds the limits. Now I am paying 😂.
 **Users are impatient**, they asked only 1-2 questions. If they didn't receive the answer they like, they left. 
 **Users are unreasonable**, (or I am unreasonable). Apparently, it's a bot for new students question and answering, why they asked questions like "When should we have dinner", "what should I eat for dinner" 
 
 **Do I think data preparation is helpful?**
To be honest, I think people attitude and their wording is really different when they talk to a chatbot; less complete sentence, less polite. I knew about it. But its a different case to see the truth. 
It is still helpful in case of finding keywords.

 
### Final Thought:
**To write a chatbot is easy, but to make users like it is difficult.**
 
If you are interested in the Ethical and Privacy issues I found during data preprocessing, you can find it here: https://www.ajoy.me/note/chatbot_ethical_concern

Thanks to [Dexter](https://rundexter.com/)
