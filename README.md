# Machine Learning based translator with Sign Language to text conversion and vice-a-versa for hearing-impaired individuals.
## Introduction
Human society is plagued with many physical and mental disabilities, one such disability is the loss of hearing. India is reported to have sixty three million Individuals are hearing-impaired. 

This study focus on developing a deep neural network model to bridge the communication gap between the general society and the hearing impaired people. 
  * The first part of the project focuses on developing a classification algorithm to identify the English words when the input is a sign pose video.
  * The second part of the project involves conversion of English word to it's respective sign pose using an Animation software.

Thereby developing a complete translation software that can identify human pose gestures and translate it to English using Machine Learning techniques, and convert selected English words into human pose representation. This experiment is performed using the Indian Sign Language as the dataset.

## Data Collection and Exploratory Data Analysis
The data is sourced from  Indian Lexicon Sign Language Dataset - INCLUDE - an ISL dataset that contains 0.27 million frames across 4,287 videos over 263 word signs from 15 different word categories. INCLUDE is recorded with the help of experienced signers to provide close resemblance to natural conditions. 
 
The INCLUDE dataset has 4292 videos, each video is a recording of 1 ISL sign, signed by deaf students from St. Louis School for the Deaf, Adyar, Chennai.

The dataset Includes
  * **Adjectives**: Pleased, Cold, Warm, Quiet, Loud, Clean, Attack, Healthy, Peace, Wide, Cheap, Dry, Happy, Young, Fast, Alright, Short, Sick, Curved, Beautiful, Hard, Long, Heavy, Big, Small, Old, Wet, Low, Narrow, Slow, Hot, Poor, High, Adult, Famous, Sad, Shallow, Week, Cool, Deep, Tight, Good, Bad, Light, Expensive, Alive, New, Strong, Tall, Loose, Rich, Deaf, Thin, Dirty, Ugly, Dead, Nice, Thick, Soft, Blind, Mean, Flat
  * **Animals**: Cat, Cow, Animal, Mouse, Dog, Horse, Fish, Bird
  * **Clothes**: Pant, Clothing, Skirt, Hat, Shoes, Shirt, Pocket, Suit, T Shirt, Dress, Ring
  * **Colors**: Colour, Black, Yellow, Green, White, Orange, Blue, Grey, Brown, Pink, Red
  * **Days and Time**: Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday, Evening, Morning, Second, Night, Year, Weak, Time, Month, Minute, Today, Hour, Afternoon, Yesterday, Tomorrow
  * **Electronic devices**: Television, Telephone, Laptop, Screen, Camera, Energy, Computer, Radio, Phone
  * **Greetings**: Hello, Good Morning, Good Afternoon, Good Night, You all, Thank You, How are you, Good evening
  * **Household Items**: Lock, Book, Medicine, Lamp, Photograph, Newspaper, Bathroom, Kitchen, Pen, Bed, Table, Paint, Box, Train ticket, Bill, Key, Bedroom, Letter, Ball, Chair, Fan, Bag, Door, Paper, Pencil, Tool, Soap, Page, Card, Window, Clock
  * **Various Professions**: Police, Actor, Secretary, Student, Patient, Player, Waiter, Manager, Soldier, Lawyer, Doctor, President, Artist, Priest, Author, Reporter
  * **Means of Transportation**: Car, Bus, Bicycle, Sign, Plane, Boat, Transportation, Truck, Train
  * **People**: Child, Baby, Girl, Mother, Woman, Grandmother, Daughter, Neighbour, Friend, Husband, Sister, Wife, Son, Brother, Man, King, Queen, Father, Teacher, Boy, Parent, Family, Grandfather
  * **Pronouns**: Male, She, Me, You, We, It, They, He, Female
  * **Seasons**: Monsoon, Winter, Fall, Summer, Spring, Season
  * **Society**: Religion, Science, Marriage, God, War, Job, Election, Sport, Ethnicity, Death, Team, Crowd, Price, Exercise, Money, Dream, Technology

## Data Visualization and Prepossessing

### Architecure 
The project is developed by capturing videos of the signed poses enacted by the humans. Each video is split into it’s constituent frames, having 25 frames per video. The pose landmarks are identified for body and hand to recognize the hand gesture and the location of hand at the time of making the pose. These pose co-ordinates are passed to ML models to identify the poses made by the user and classify it to an English word.
![Method](https://github.com/Kaushik-yh/Indian_sign_language_pose_identification/assets/138836652/c04528e3-6c59-441e-8040-885a330bbe65)

### Identifying Human Pose
The pose co-ordinates are extracted using MediaPipe library. This API gives the X and Y co-ordinates of 33 Body Landmark and 21 hand landmarks each for Left hand and Right Hand. Z co-ordinate to show the depth of the point, i.e. how for is the given X and Y values from the median point of the Body and Hand respectively. A Visibility field which provides information about if the Body/Hand Landmark are visible, and what percent visibility is observed. In total, there are 301 columns/Features representing X, Y, Z and Visibility for each frame of the video. The extraction of pose landmarks can be viewed from the figure:
<img width="983" alt="Pose_Landmark_Identification" src="https://github.com/Kaushik-yh/Indian_sign_language_pose_identification/assets/138836652/266d0790-5972-48da-8b00-8a77dc6f8c3b">
fig:Pose_Landmark_Identification

### Data Prepossessing
**Visible Landmarks**: The Landmarks which representing Knee points and the Feet points are invisible and do not contribute any information to the pose representation done, hence, it can be excluded from model training.

**Visibility Feature**: The Feature that represents Visiblity precentage of the point in the given frame also does not contribute any value addition to the model, hence it can be removed.

**Depth Feature**: Since the variance between the Depth and the Y co-ordinates of the pose were similar, it is beneficial to remove the Z coordinates, thereby reducing the model complexity.

## Models Comparison and Results
The output of various models were compared and the best results were selected from each model to have the final model selection. The output of the models are described in the table:

| Model Type | Frames per video | Training accuracy | Testing Accuracy |
| --- | --- | --- | --- |
| Logistic Regression | 180 | 1 | 0.3932 | 
| Decision Tree | 180 | 1 | 0.5799 | 
| Random Forest | 180 | 1 | 0.8345 | 
| Ada Boost | 180 | 1 | 0.8203 | 
| Deep Neural Network | 180 | 0.8921 | 0.7397 | 
| LSTM | 180 | 0.587 | 0.1019 | 
| GRU | 180 | 1 | 0.3992 | 
| Self-Attention NN | 180 | 0.9694 | 0.9056| 
| Multihead Self-Attention NN | 180 | 0.9645 | 0.8992 | 

## Sign pose generation using Unity

The pose co-ordinates extracted by the Mediapipe API, which were used to train the ML models are being used again to recreate the videos using the animation and game development software - Unity.
The CSVs files that contain the pose-coordinates are loaded into Unity software, the pose animation is recreated by conneting GameObjects and animating them using the C# script for the joints and bones connecting the joints.\
![Good Morning_ISL](https://github.com/Kaushik-yh/Indian_sign_language_pose_identification/assets/138836652/30cd3d5d-d6ee-4b5d-a9c2-c46daceb45fc)

gif: The animation representing the sign of "Good Morning" in Indian Sign Language

![Actor_ISL](https://github.com/Kaushik-yh/Indian_sign_language_pose_identification/assets/138836652/1c2e92a4-e2ef-49f7-a9ac-07ad284842db)

gif: The animation representing the sign of "Actor" in Indian Sign Language

The Animation can be re-created for all the bellow signed words from Indian Sign Language
  * **Adjectives**: Pleased, Cold, Warm, Quiet, Loud, Clean, Attack, Healthy, Peace, Wide, Cheap, Dry, Happy, Young, Fast, Alright, Short, Sick, Curved, Beautiful, Hard, Long, Heavy, Big, Small, Old, Wet, Low, Narrow, Slow, Hot, Poor, High, Adult, Famous, Sad, Shallow, Week, Cool, Deep, Tight, Good, Bad, Light, Expensive, Alive, New, Strong, Tall, Loose, Rich, Deaf, Thin, Dirty, Ugly, Dead, Nice, Thick, Soft, Blind, Mean, Flat
  * **Animals**: Cat, Cow, Animal, Mouse, Dog, Horse, Fish, Bird
  * **Clothes**: Pant, Clothing, Skirt, Hat, Shoes, Shirt, Pocket, Suit, T Shirt, Dress, Ring
  * **Colors**: Colour, Black, Yellow, Green, White, Orange, Blue, Grey, Brown, Pink, Red
  * **Days and Time**: Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday, Evening, Morning, Second, Night, Year, Weak, Time, Month, Minute, Today, Hour, Afternoon, Yesterday, Tomorrow
  * **Electronic devices**: Television, Telephone, Laptop, Screen, Camera, Energy, Computer, Radio, Phone
  * **Greetings**: Hello, Good Morning, Good Afternoon, Good Night, You all, Thank You, How are you, Good evening
  * **Household Items**: Lock, Book, Medicine, Lamp, Photograph, Newspaper, Bathroom, Kitchen, Pen, Bed, Table, Paint, Box, Train ticket, Bill, Key, Bedroom, Letter, Ball, Chair, Fan, Bag, Door, Paper, Pencil, Tool, Soap, Page, Card, Window, Clock
  * **Various Professions**: Police, Actor, Secretary, Student, Patient, Player, Waiter, Manager, Soldier, Lawyer, Doctor, President, Artist, Priest, Author, Reporter
  * **Means of Transportation**: Car, Bus, Bicycle, Sign, Plane, Boat, Transportation, Truck, Train
  * **People**: Child, Baby, Girl, Mother, Woman, Grandmother, Daughter, Neighbour, Friend, Husband, Sister, Wife, Son, Brother, Man, King, Queen, Father, Teacher, Boy, Parent, Family, Grandfather
  * **Pronouns**: Male, She, Me, You, We, It, They, He, Female
  * **Seasons**: Monsoon, Winter, Fall, Summer, Spring, Season
  * **Society**: Religion, Science, Marriage, God, War, Job, Election, Sport, Ethnicity, Death, Team, Crowd, Price, Exercise, Money, Dream, Technology
