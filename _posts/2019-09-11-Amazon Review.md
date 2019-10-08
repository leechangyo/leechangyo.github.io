---
layout: post
title: 11. Amazon Review
category: Python
tag: Python
---

# DATA OVERVIEW
- Dataset consists of 3000 Amazon customer reviews, star ratings, date of review, variant and feedback of various amazon Alexa products like Alexa Echo, Echo dots
- The objective is to discover insights into consumer reviews and perfrom sentiment analysis on the data.
- Dataset: www.kaggle.com/sid321axn/amazon-alexa-reviews
- PLEASE MAKE SURE TO INSTALL WORDCLOUD: pip install wordcloud

```python
# import libraries
import pandas as pd # Import Pandas for data manipulation(조정) using dataframes
import numpy as np # Import Numpy for data statistical analysis
import matplotlib.pyplot as plt # Import matplotlib for data visualisation(시각화)
import seaborn as sns # Statistical data visualization library.
%matplotlib inline
# matplotlib inline is that for jupiter note to prompht seeing plot
```

```python
df_alexa = pd.read_csv('amazon_alexa.tsv', sep='\t') #"\t" seperator information of file

#、<- \t tab, \n new line, \s - blank space
# to read '.tsv' or csv function is 'pd.read_csv'
df_alexa.head()
```

<a href="https://postimg.cc/qg8ydH91"><img src="https://i.postimg.cc/mZnwCbVW/41231232.png" width="400px" title="source: imgur.com" /><a>

```python
df_alexa.keys()
# to read column '0'
# Index(['rating', 'date', 'variation', 'verified_reviews', 'feedback'], dtype='object')
df_alexa.tail()
```
<a href="https://postimg.cc/7bdHX5Bc"><img src="https://i.postimg.cc/X77rjF9V/4123212.png" width="400px" title="source: imgur.com" /><a>


```python
df_alexa['verified_reviews']
```

```
0                                           Love my Echo!
1                                               Loved it!
2       Sometimes while playing a game, you can answer...
3       I have had a lot of fun with this thing. My 4 ...
4                                                   Music
5       I received the echo as a gift. I needed anothe...
6       Without having a cellphone, I cannot use many ...
7       I think this is the 5th one I've purchased. I'...
8                                             looks great
9       Love it! I’ve listened to songs I haven’t hear...
10      I sent it to my 85 year old Dad, and he talks ...
11      I love it! Learning knew things with it eveyda...
12      I purchased this for my mother who is having k...
13                                     Love, Love, Love!!
14                               Just what I expected....
15                              I love it, wife hates it.
16      Really happy with this purchase.  Great speake...
17      We have only been using Alexa for a couple of ...
18      We love the size of the 2nd generation echo. S...
19      I liked the original Echo. This is the same bu...
20      Love the Echo and how good the music sounds pl...
21      We love Alexa! We use her to play music, play ...
22      Have only had it set up for a few days. Still ...
23      I love it. It plays my sleep sounds immediatel...
24      I got a second unit for the bedroom, I was exp...
25                                        Amazing product
26      I love my Echo. It's easy to operate, loads of...
27                              Sounds great!! Love them!
28      Fun item to play with and get used to using.  ...
29                                Just like the other one
                              ...                        
3120                                                     
3121    I like the hands free operation vs the Tap. We...
3122    I dislike that it confuses my requests all the...
3123                                                     
3124    Love my Alexa! Actually have 3 throughout the ...
3125    This product is easy to use and very entertain...
3126                                                     
3127    works great but speaker is not the good for mu...
3128      Outstanding product - easy to use.  works great
3129    We have six of these throughout our home and t...
3130            Use the product for music and it’s great!
3131                           Easy to set-up and to use.
3132                                     It works great!!
3133    I like having more Alexa devices in my house a...
3134                                           PHENOMENAL
3135                 I loved it does exactly what it says
3136    I used it to control my smart home devices. Wo...
3137                                      Very convenient
3138    Este producto llegó y a la semana se quedó sin...
3139              Easy to set up Ready to use in minutes.
3140                                                Barry
3141                                                     
3142    My three year old loves it.  Good for doing ba...
3143           Awesome device wish I bought one ages ago.
3144                                              love it
3145    Perfect for kids, adults and everyone in betwe...
3146    Listening to music, searching locations, check...
3147    I do love these things, i have them running my...
3148    Only complaint I have is that the sound qualit...
3149                                                 Good
Name: verified_reviews, Length: 3150, dtype: object
```
```python
df_alexa.info() #  non-null means there is no missing information.
```

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3150 entries, 0 to 3149
Data columns (total 5 columns):
rating              3150 non-null int64
date                3150 non-null object
variation           3150 non-null object
verified_reviews    3150 non-null object
feedback            3150 non-null int64
dtypes: int64(2), object(3)
memory usage: 123.1+ KB

```

```python
df_alexa.describe() # count is how many people rating.
# 4.46 is good thing like the rate peoplse rated by means like it is high
# feed bace is 0~1
# standard devation.(표준편차)
```
```

       	rating 	feedback
count 3150.000  3150.000
mean 	4.463175 	0.918413
std 	1.068506 	0.273778
min 	1.000000 	0.000000
25% 	4.000000 	1.000000
50% 	5.000000 	1.000000
75% 	5.000000 	1.000000
max 	5.000000 	1.000000
```
# VISUALIZING THE DATA

```python
positive = df_alexa[df_alexa['feedback']==1] # 1 is possitive feedback
# only take a raw which is value is one in feedback colum
negative = df_alexa[df_alexa['feedback']==0] # 0 is negative feedback
# only take a raw which is value is zero in feedback colum
negative
```

```
rating 	date 	variation 	verified_reviews 	 feedback
46 	2 	30-Jul-18 	Charcoal Fabric 	It's like Siri, in fact, Siri answers more acc... 	0
111 	2 	30-Jul-18 	Charcoal Fabric 	Sound is terrible if u want good music too get... 	0
141 	1 	30-Jul-18 	Charcoal Fabric 	Not much features. 	0
162 	1 	30-Jul-18 	Sandstone Fabric 	Stopped working after 2 weeks ,didn't follow c... 	0
176 	2 	30-Jul-18 	Heather Gray Fabric 	Sad joke. Worthless. 	0
187 	2 	29-Jul-18 	Charcoal Fabric 	Really disappointed Alexa has to be plug-in to... 	0
205 	2 	29-Jul-18 	Sandstone Fabric 	It's got great sound and bass but it doesn't w... 	0
233 	2 	29-Jul-18 	Sandstone Fabric 	I am not super impressed with Alexa. When my P... 	0
299 	2 	29-Jul-18 	Charcoal Fabric 	Too difficult to set up. It keeps timing out ... 	0
341 	1 	28-Jul-18 	Charcoal Fabric 	Alexa hardly came on.. 	0
350 	1 	31-Jul-18 	Black 	Item no longer works after just 5 months of us... 	0
361 	1 	29-Jul-18 	Black 	This thing barely works. You have to select 3r... 	0
368 	1 	28-Jul-18 	Black 	I returned 2 Echo Dots & am only getting refun... 	0
369 	1 	28-Jul-18 	Black 	not working 	0
373 	1 	27-Jul-18 	Black 	I'm an Echo fan but this one did not work 	0
374 	1 	26-Jul-18 	Black 		0
376 	2 	26-Jul-18 	Black 	Doesn't always respond when spoken to with pro... 	0
381 	1 	25-Jul-18 	White 	It worked for a month or so then it stopped. I... 	0
382 	2 	25-Jul-18 	Black 	Poor quality. Gave it away. 	0
388 	1 	24-Jul-18 	Black 	Never could get it to work. A techie friend lo... 	0
394 	2 	22-Jul-18 	White 	Initially, this echo dot worked very well. Ove... 	0
396 	1 	20-Jul-18 	Black 	I bought an echo dot that had been refurbished... 	0
398 	1 	19-Jul-18 	Black 	Dont trust this.... 	0
406 	1 	16-Jul-18 	White 		0
418 	1 	13-Jul-18 	Black 	I wanted to use these as a radio and intercom ... 	0
420 	1 	12-Jul-18 	Black 	Item has never worked. Out of box it is broken... 	0
424 	1 	11-Jul-18 	Black 	Great product but returning for new Alexa Dot.... 	0
434 	1 	9-Jul-18 	Black 	&#34;NEVER BUY CERTIFIED AND REFURBISHED ECHO ... 	0
470 	1 	1-Jul-18 	White 	This item did not work. Certified refurbished ... 	0
473 	2 	29-Jun-18 	White 	None 	0
... 	... 	... 	... 	... 	...
2688 	2 	30-Jul-18 	Black Dot 	Weak sound. Compared to the Google Home Mini t... 	0
2696 	1 	30-Jul-18 	Black Dot 	Echo Dot responds to us when we aren't even ta... 	0
2697 	1 	30-Jul-18 	White Dot 	NOT CONNECTED TO MY PHONE PLAYLIST :( 	0
2716 	2 	30-Jul-18 	Black Dot 	The only negative we have on this product is t... 	0
2740 	1 	30-Jul-18 	Black Dot 	I didn’t order it 	0
2745 	1 	30-Jul-18 	White Dot 	The product sounded the same as the emoji spea... 	0
2812 	1 	30-Jul-18 	Black Dot 	I am quite disappointed by this product.There ... 	0
2823 	2 	30-Jul-18 	Black Dot 	Nope. Still a lot to be improved. For most of ... 	0
2842 	1 	30-Jul-18 	Black Dot 	I reached out to Amazon, because the device wa... 	0
2851 	1 	30-Jul-18 	Black Dot 	I didn't like that almost everytime i asked Al... 	0
2866 	1 	30-Jul-18 	Black Dot 	The volume is very low 	0
2876 	1 	30-Jul-18 	Black Dot 		0
2892 	1 	30-Jul-18 	Black Dot 	Cheap and cheap sound. 	0
2909 	2 	30-Jul-18 	Black Dot 	For the price, the product is nice quality and... 	0
2922 	1 	30-Jul-18 	White Dot 	Used twice not working!!!!!!! 	0
2932 	1 	30-Jul-18 	Black Dot 	This device does not interact with my home fil... 	0
2940 	2 	30-Jul-18 	White Dot 	Not all that happy. The speaker isn’t great an... 	0
2945 	2 	30-Jul-18 	Black Dot 	When you think about it this really doesn’t do... 	0
2962 	1 	30-Jul-18 	Black Dot 	This worked well for about 6 months but then s... 	0
2964 	2 	30-Jul-18 	Black Dot 	Ask it to play Motown radio on Pandora and it ... 	0
2979 	1 	30-Jul-18 	White Dot 		0
3010 	2 	30-Jul-18 	Black Dot 	Sound is terrible. Cannot pair with echo to pl... 	0
3016 	1 	30-Jul-18 	White Dot 	I am having real difficulty working with the E... 	0
3024 	1 	30-Jul-18 	Black Dot 	I was really happy with my original echo so i ... 	0
3039 	2 	30-Jul-18 	Black Dot 	Weak sound. Compared to the Google Home Mini t... 	0
3047 	1 	30-Jul-18 	Black Dot 	Echo Dot responds to us when we aren't even ta... 	0
3048 	1 	30-Jul-18 	White Dot 	NOT CONNECTED TO MY PHONE PLAYLIST :( 	0
3067 	2 	30-Jul-18 	Black Dot 	The only negative we have on this product is t... 	0
3091 	1 	30-Jul-18 	Black Dot 	I didn’t order it 	0
3096 	1 	30-Jul-18 	White Dot 	The product sounded the same as the emoji spea... 	0
```
```python
sns.countplot(df_alexa['feedback'], label = "Count")
# how many possitive and negative
# label is the what we pointg on.
```
<a href="https://postimg.cc/LhD9Ky6c"><img src="https://i.postimg.cc/HsDVMRGk/42132123.png" width="400px" title="source: imgur.com" /><a>

```python
sns.countplot(x = 'rating', data = df_alexa)
#countplot is that just count for
```
<a href="https://postimg.cc/8j3VNx80"><img src="https://i.postimg.cc/2SjjFf0r/4232132.png" width="400px" title="source: imgur.com" /><a>


```python
df_alexa['rating'].hist(bins = 5)
#bin is how many bar we will display on the plot
```
<a href="https://postimg.cc/Xp9gYyt6"><img src="https://i.postimg.cc/hP5ZMLrv/4213222.png" width="400px" title="source: imgur.com" /><a>

```python
plt.figure(figsize = (40,15))
sns.barplot(x = 'variation', y='rating', data=df_alexa, palette = 'deep')
# this is showing all of the data they got
#variation : 변화
```

<a href="https://postimg.cc/tsSnqqRH"><img src="https://i.postimg.cc/LsRtV5rh/4213221.png" width="700px" title="source: imgur.com" /><a>

# WORD CLOUD
- we can know how many the word repeated and we can know what is the word used by
```python
words = df_alexa['verified_reviews'].tolist()# this is the column just we interested in
# like 생략 인덱스 와 라벨
#we elminate the blank
#tolist <- it is transform all the word into the list of the word
#because we dont need to have blank
#with `,`
# like we dont maek a list make it one into list
len(words)
# 3150
print(words)
```

```
['Love my Echo!', 'Loved it!', 'Sometimes while playing a game, you can answer a question correctly but Alexa says you got it wrong and answers the same as you.  I like being able to turn lights on and off while away from home.', 'I have had a lot of fun with this thing. My 4 yr old learns about dinosaurs, i control the lights and play games like categories. Has nice sound when playing music as well.', 'Music', 'I received the echo as a gift. I needed another Bluetooth or something to play music easily accessible, and found this smart speaker. Can’t wait to see what else it can do.', 'Without having a cellphone, I cannot use many of her features. I have an iPad but do not see that of any use.  It IS a great alarm.  If u r almost deaf, you can hear her alarm in the bedroom from out in the living room, so that is reason enough to keep her.It is fun to ask random questions to hear her response.  She does not seem to be very smartbon politics yet.', "I think this is the 5th one I've purchased. I'm working on getting one in every room of my house. I really like what features they offer specifily playing music on all Echos and controlling the lights throughout my house.", 'looks great', 'Love it! I’ve listened to songs I haven’t heard since childhood! I get the news, weather, information! It’s great!', 'I sent it to my 85 year old Dad, and he talks to it constantly.', "I love it! Learning knew things with it eveyday! Still figuring out how everything works but so far it's been easy to use and understand. She does make me laugh at times", 'I purchased this for my mother who is having knee problems now, to give her something to do while trying to over come not getting around so fast like she did.She enjoys all the little and big things it can do...Alexa play this song, What time is it and where, and how to cook this and that!', 'Love, Love, Love!!', 'Just what I expected....', 'I love it, wife hates it.', 'Really happy with this purchase.  Great speaker and easy to set up.', 'We have only been using Alexa for a couple of days and are having a lot of fun with our new toy. It like having a new household member! We are trying to learn all the different featues and benefits that come with it.', 'We love the size of the 2nd generation echo. Still needs a little improvement on sound', "I liked the original Echo. This is the same but shorter and with greate']
```

# WaterMark to PDF
```python
words_as_one_string =" ".join(words) # this " " <- it is technical function to put all the word into together
# join the `word` into ` ` inside. like just fill word out in ` ` blank space
# we need to make one single string out of it.
# so we need to merge word all into together
# it remove []<- list branket
words_as_one_string
```
```
'Love my Echo! Loved it! Sometimes while playing a game, you can answer a question correctly but Alexa says you got it wrong and answers the same as you.  I like being able to turn lights on and off while away from home. I have had a lot of fun with this thing. My 4 yr old learns about dinosaurs, i control the lights and play games like categories. Has nice sound when playing music as well. Music I received the echo as a gift. I needed another Bluetooth or something to play music easily accessible, and found this smart speaker. Can’t wait to see what else it can do. Without having a cellphone, I cannot use many of her features. I have an iPad but do not see that of any use.  It IS a great alarm.  If u r almost deaf, you can hear her alarm in the bedroom from out in the living room, so that is reason enough to keep her.It is fun to ask random questions to hear her response.  She does not seem to be very smartbon politics yet. I think this is the 5th one I\'ve purchased. I\'m working on getting one in every room of my house. I really like what features they offer specifily playing music on all Echos and controlling the lights throughout my house. looks great Love it! I’ve listened to songs I haven’t heard since childhood! I get the news, weather, information! It’s great! I sent it to my 85 year old Dad, and he talks to it constantly. I love it! Learning knew things with it eveyday! Still figuring out how everything works but so far it\'s been easy to use and understand. She does make me laugh at times I purchased this for my mother who is having knee problems now, to give her something to do while trying to over come not getting around so fast like she did.She enjoys all the little and big things it can do...Alexa play this song, What time is it and where, and how to cook this and that! Love, Love, Love!! Just what I expected.... I love it, wife hates it. Really happy with this purchase.  Great speaker and easy to set up. We have only been using Alexa for a couple of days and are having a lot of fun with our new toy. It like having a new household member! We are trying to learn all the different featues and benefits that come with it. We love the size of the 2nd generation echo. Still needs a little improvement on sound I liked the original Echo. This is the same but shorter and with greater fabric/color choices. I miss the volume ring on top, now it\'s just the plus/minus buttons. Not a big deal but the ring w as comforting. :) Other than that, well I do like the use of a standard USB charger /port instead of the previous round pin. Other than that, I guess it sounds the same, seems to work the same, still answers to Alexa/Echo/Computer. So what\'s not to like? :) Love the Echo and how good the music sounds playing off it. Alexa understands most commands but it is difficult at times for her to find specific playlists or songs on Spotify. She is good with Amazon Music but is lacking in other major programs. We love Alexa! We use her to play music, play radio through iTunes, play podcasts through Anypod, and set reminders. We listen to our flash briefing of news and weather every morning. We rely on our custom lists. We like being able to voice control the volume. We\'re sure we\'ll continue to find new uses.Sometimes it\'s a bit frustrating when Alexa doesn\'t understand what we\'re saying. Have only had it set up for a few days. Still adding smart home devices to it. The speaker is great for playing music. I like the size, we have it stationed on the kitchen counter and it’s not intrusive to look at. I love it. It plays my sleep sounds immediately when I ask I got a second unit for the bedroom, I was expecting the sounds to be improved but I didnt really see a difference at all.  Overall, not a big improvement over the 1st generation. Amazing product I love my Echo. It\'s easy to operate, loads of fun.It is everything as advertised. I use it mainly to play my favorite tunes and test Alexa\'s knowledge. Sounds great!! Love them! Fun item to play with and get used to using.  Sometimes has hard time answering the questions you ask, but I think it will be better. Just like the other one Still learning all the capabilities...but so far pretty pretty pretty good I like it She works well. Needs a learning command  for unique, owners and users like. Alexa “learn” Tasha’s birthday.  Or Alexa “learn” my definition of Fine. Etc. other than that she is great The speakers sound pretty good for being so small and setup is pretty easy.  I bought two and the reason I only rate it a 3 is I have followed the instructions
```

```python
len(words_as_one_string)
# 419105
from wordcloud import WordCloud

plt.figure(figsize=(20,20))
plt.imshow(WordCloud().generate(words_as_one_string))
# this is the function to show the most used word in the string

```

# DATA CLEANING/FEATURE ENGINEERING

```python
from sklearn.feature_extraction.text import CountVectorizer
# sklearn.feature _extraction.txt import Countvectorizer <- how many time 'love' mension in the review
# we need to know from the big data. so we call countvetotizer function.
vectorizer = CountVectorizer()
alexa_countvectorizer = vectorizer.fit_transform(df_alexa['verified_reviews'])
#vectorizer.fit_transform<- translate my verified the reviews into the count vectorizor of word
#transform our review into count of vectorizer
alexa_countvectorizer.shape
# almost 4000 column means that how many word we have
# (3150, 4044)
print(alexa_countvectorizer.toarray()) # convert it to array
```

```
[[0 0 0 ... 0 0 0]
 [0 0 0 ... 0 0 0]
 [0 0 0 ... 0 0 0]
 ...
 [0 0 0 ... 0 0 0]
 [0 0 0 ... 0 0 0]
 [0 0 0 ... 0 0 0]]

```

```python
type(alexa_countvectorizer)
#scipy.sparse.csr.csr_matrix
print(vectorizer.get_feature_names())
#this is simply every single word that have been repeated somehow within the reviews
#can see every signle word
```
```
['00', '000', '07', '10', '100', '100x', '11', '1100sf', '12', '129', '12am', '15', '150', '18', '19', '1964', '1990', '1gb', '1rst', '1st', '20', '200', '2000', '2017', '229', '23', '24', '25', '29', '2nd', '2package', '30', '300', '30pm', '34', '360', '39', '3rd', '3x', '3xs', '40', '45', '48', '4am', '4ghz', '4k', '4th', '50', '54', '5am', '5ghz', '5th', '600', '62', '672', '6th', '70', '75', '79', '80', '80s', '81', '83', '85', '88', '888', '8gb', '90', '91', '911', '99', '_specifically_', 'a1', 'a19', 'abay', 'abc', 'abd', 'abilities', 'ability', 'able', 'abode', 'about', 'above', 'absolutely', 'absolutly', 'ac', 'accent', 'acceptable', 'accepting', 'access', 'accessable', 'accessible', 'accessing', 'accessories', 'accesss', 'accident', 'accidentally', 'accompanying', 'accomplish', 'accomplished', 'according', 'accordingly', 'account', 'accounts', 'accuracy', 'accurate', 'accurately', 'accustom', 'acknowledge', 'acoustical', 'across', 'act', 'acting', 'action', 'actions', 'activate', 'activated', 'activates', 'activating', 'activation', 'actively', 'activities', 'acts', 'actually', 'ad', 'adapted', 'adapter', 'adapting', 'add', 'added', 'addict', 'addicted', 'addicts', 'adding', 'addition', 'additional', 'additionally', 'addons', 'addressed', 'addresses', 'adds', 'adept', 'adequate', 'adjacent', 'adjust', 'adjusting', 'adjustment', 'adjusts', 'admit', 'adopters', 'adorable', 'ads', 'adults', 'advance', 'advanced', 'advantage', 'advantages', 'advertise', 'advertised', 'advertisement', 'advertising', 'advice', 'advise', 'advised', 'aesthetic', 'af', 'affirm', 'affirmations', 'afford', 'affordable', 'afraid', 'after', 'afternoon', 'afterwards', 'again', 'age', 'agent', 'ages', 'ago', 'agree', 'agreement', 'ahead', 'ai', 'aide', 'aint', 'air', 'aka', 'al', 'alabama', 'alarm', 'alarms', 'albeit', 'alcohol', 'alert', 'alerts', 'alex', 'alexa', 'alexas', 'alexi', 'alexia', 'alexis', 'alexus', 'algo', 'alive', 'all', 'alleviate', 'allow', 'allowed', 'allowing', 'allows', 'allrecipes', 'almost', 'alone', 'along', 'alongside', 'alot', 'alots', 'aloud', 'alread', 'already', 'alright', 'also', 'altering', 'alternative', 'alternatives', 'although', 'always', 'am', 'amaonmazing', 'amaxing', 'amaze', 'amazed', 'amazin', 'amazing', 'amazingly', 'amazon', 'amazonia', 'amazons', 'ambient', 'american', 'americans', 'among', 'amount', 'amounts', 'amozon', 'amplifier', 'amused', 'amusing', 'an', 'analog', 'and', 'android', 'ands', 'angle', 'annoying', 'another', 'answer', 'answered', 'answering', 'answers', 'ant', 'anti', 'anticipate', 'anticipated', 'any', 'anybody', 'anyhow', 'anylist', 'anymore', 'anyone', 'anypod', 'anything', 'anytime', 'anyway', 'anyways', 'anywhere', 'apartment', 'app', 'apparent', 'apparently', 'appealing', 'appear', 'appears', 'apple', 'appliance', 'appliances', 'application', 'applications', 'appointments', 'appreciated', 'apprehensive', 'approaching', 'appropriate', 'approximately', 'apps', 'are', 'area', 'areas', 'aren', 'arent', 'argue', 'argument', 'arguments', 'arises', 'arlo', 'arm', 'around', 'array', 'arrive', 'arrived', 'arriving', 'articles', 'artist', 'artists', 'as', 'asap', 'ase', 'ask', 'asked', 'askes', 'asking', 'asleep', 'aspect', 'aspects', 'ass', 'assigned', 'assist', 'assistance', 'assistant', 'assume', 'assumed', 'assuming', 'assumption', 'at', 'atención', 'atmosphere', 'atrás', 'attach', 'attached', 'attachment', 'attempt', 'attempted', 'attempting', 'attention', 'attractive', 'audible', 'audibles', 'audio', 'audioapple', 'audiobook', 'audiobooks', 'audiophile', 'august', 'aunt', 'auto', 'automatic', 'automatically', 'automation', 'aux', 'auxiliary', 'av', 'avail', 'availability', 'available', 'avoid', 'awake', 'aware', 'away', 'awesome', 'awful', 'awhile', 'awkward', 'awsome', 'b073sqyxtw', 'baby', 'back', 'background', 'backgrounds', 'backyard', 'bad', 'baffle', 'baffled', 'ball', 'ban', 'band', 'bandwagon', 'bandwidth', 'bang', 'bar', 'bare', 'barely', 'bargain', 'bark', 'barn', 'barret', 'barry', 'base', 'baseball', 'based', 'basement', 'basic', 'basically', 'bass', 'bathroom', 'bathrooms', 'batman', 'batteries', 'battery', 'bc', 'be', 'beam', 'beat', 'beautiful', 'beautifully', 'beauty', 'became', 'because', 'becausse', 'become', 'becomes', 'becoming', 'bed', 'bedroom', 'bedrooms', 'bedside', 'bedtime', 'beefy', 'been', 'before', 'begin', 'beginners', 'beginning', 'begun', 'behaved', 'behind', 'being', 'believe', 'believer', 'bells', 'belong', 'below', 'benefit', 'benefits', 'beside', 'besides', 'best', 'bet', 'beta', 'better', 'bettter', 'between', 'beyond', 'bezel', 'bezos', 'bf', 'bff', 'bible', 'big', 'bigger', 'biggest', 'bill', 'billboard', 'bills', 'bing', 'birth', 'birthday', 'bit', 'bizarre', 'black', 'blanket', 'blast', 'blasting', 'blessing', 'blind', 'blink', 'blinks', 'blocking', 'bloods', 'bloomberg', 'blown', 'blows', 'blue', 'blueprints', 'bluetooth', 'blurring', 'board', 'boat', 'bob', 'body', 'bolt', 'bonkers', 'bonus', 'book', 'books', 'boom', 'boombox', 'bo'....
```

```python
word_count_array = alexa_countvectorizer.toarray()
word_count_array.shape
#(3150, 4044)
word_count_array[0,:] # we grab first raw all the column
# array([0, 0, 0, ..., 0, 0, 0], dtype=int64)
```

```python
index = 13
plt.plot(word_count_array[0, :])# the showing that how many word mostly repeated the specified row
df_alexa['verified_reviews'][0]
# we can see mostly using word in the review '0' column
# 'Love my Echo!'
```

<a href="https://postimg.cc/JsL3S467"><img src="https://i.postimg.cc/sgBnQM5W/333333.png" width="400px" title="source: imgur.com" /><a>

```python
df_alexa['length'] = df_alexa['verified_reviews'].apply(len)
#create a new kind of feature which our data frame that has a length into it
#and apply len simply, means that count how many words in 'verified_reviews'
# and added in a new data fram new elemant in the frames
df_alexa.head()
```
<a href="https://postimg.cc/Q9GjJwNz"><img src="https://i.postimg.cc/FR11NXMh/4232123.png" width="400px" title="source: imgur.com" /><a>

```python
df_alexa['length'].hist(bins=100)
```
<a href="https://postimg.cc/m1hBSH1d"><img src="https://i.postimg.cc/gjHjmyzP/3333.png" width="400px" title="source: imgur.com" /><a>


```python
min_char = df_alexa['length'].min()
df_alexa[df_alexa['length'] == min_char]['verified_reviews'].iloc[0]
# this is one of minimum review
#extract 'verified_reviews'
#previously iloc'0' . iloc is primart integer position based (옆으로 글씨가 나오게 하는거)
```
```
'😍'
```

```python
max_char = df_alexa['length'].max()
df_alexa[df_alexa['length'] == max_char]['verified_reviews'].iloc[0]
```

```
"Incredible piece of technology.I have this right center of my living room on an island kitchen counter. The mic and speaker goes in every direction and the quality of the sound is quite good. I connected the Echo via Bluetooth to my Sony soundbar on my TV but find the Echo placement and 360 sound more appealing. It's no audiophile equipment but there is good range and decent bass. The sound is more than adequate for any indoor entertaining and loud enough to bother neighbors in my building. The knob on the top works great for adjusting volume. This is my first Echo device and I would imagine having to press volume buttons (on the Echo 2) a large inconvenience and not as precise. For that alone I would recommend this over the regular Echo (2nd generation).The piece looks quality and is quite sturdy with some weight on it. The rubber material on the bottom has a good grip on the granite counter-- my cat can even rub her scent on it without tipping it over.This order came with a free Philips Hue Bulb which I installed along with an extra one I bought. I put the 2 bulbs into my living room floor lamp, turned on the light, and all I had to do was say &#34;Alexa, connect my devices&#34;. The default names for each bulb was assigned as &#34;First light&#34; and &#34;Second light&#34;, so I can have a dimmer floor lamp if I just turned on/off one of the lights by saying &#34;Alexa, turn off the second light&#34;. In the Alexa app, I created a 'Group' with &#34;First light&#34; and &#34;Second light&#34; and named the group &#34;The light&#34;, so to turn on the lamp with both bulbs shining I just say &#34;Alexa, turn on The light&#34;.I was surprised how easily the bulbs connected to the Echo Plus with its built in hub. I thought I would have to buy a hub bridge to connect to my floor lamp power plug. Apparently there is some technology built directly inside the bulb! I was surprised by that. Awesome.You will feel like Tony Stark on this device. I added quite a few &#34;Skills&#34; like 'Thunderstorm sounds' and 'Quote of the day' . Alexa always loads them up quickly. Adding songs that you hear to specific playlists on Amazon Music is also a great feature.I can go on and on and this is only my second day of ownership.I was lucky to buy this for $100 on Prime Day, but I think for $150 is it pretty expensive considering the Echo 2 is only $100. In my opinion, you will be paying a premium for the Echo Plus and you have to decide if the value is there for you:1) Taller and 360 sound unit.2) Volume knob on top that you spin (I think this is a huge benefit over buttons)3) Built in hub for Hue bulbs. After researching more, there are some cons to this setup if you plan on having more advanced light setups. For me and my floor lamp, it's just perfect.I highly recommend it and will buy an Echo dot for my bedroom now."
```
