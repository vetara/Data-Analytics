# Profitable App Profiles For The App Store and Google Play Markets

- Our goal for this project is to analyze data to help our developers understand what type of apps are likely to attract more users.
- Our main source of revenue consists of in-app ads.
- This means our revenue for any given app is mostly influenced by the number of users who use our app — the more users that see and engage with the ads, the better.

# Official Documentation

- For more information on the Apple Store dataset, click [here](https://www.kaggle.com/ramamet4/app-store-apple-data-set-10k-apps).
- For more information on the Google Play dataset, click [here](https://www.kaggle.com/lava18/google-play-store-apps).

# The `explore_data()` Function:

- Function that prints the rows of a data set in a readable way
- Takes in four parameters:
  - **`dataset`**, which is expected to be a list of lists
  - **`start`** and **`end`**, which are both expected to be integers and represent the starting and the ending indices of a slice from the data set.
  - **`rows_and_columns`**, which is expected to be a Boolean and has **`False`** as a default argument
  
- Firstly, the function slices the data set using **`dataset[start:end]`**
- Then loops through the slice, and for each iteration, prints a row and adds a new line after that row using **`print('\n')`**
- It then prints the number of rows and columns if **`rows_and_columns`** is **`True`**


```python
# function to print rows of a data set in a readable way

def explore_data(dataset, start, end, rows_and_columns=False):
    dataset_slice = dataset[start:end]    
    for row in dataset_slice:
        print(row)
        print('\n') # adds a new (empty) line after each row

    if rows_and_columns:
        print('Number of rows:', len(dataset))
        print('Number of columns:', len(dataset[0]))
```


```python
from csv import reader

# Openning Google Play dataset and saving it as a list of lists
google_opened = open('googleplaystore.csv')
google_read = reader(google_opened)
googleplay_dataset = list(google_read)
# Seperating the header row from the rest of the dataset
google_header = googleplay_dataset[0]
google = googleplay_dataset[1:]

# Openning Apple Store dataset and saving it as a list of lists
apple_opened = open('AppleStore.csv')
apple_read = reader(apple_opened)
applestore_dataset = list(apple_read)
# Separating the header row from the rest of the dataset
apple_header = applestore_dataset[0]
apple = applestore_dataset[1:]
```


```python
# Printing the first few rows of each data set
explore_data(apple, 0, 3, 'True')
print('\n')
explore_data(google, 0, 3, 'True')

# Printing column names
print(apple_header)
print('\n')
print(google_header)
```

    ['284882215', 'Facebook', '389879808', 'USD', '0.0', '2974676', '212', '3.5', '3.5', '95.0', '4+', 'Social Networking', '37', '1', '29', '1']
    
    
    ['389801252', 'Instagram', '113954816', 'USD', '0.0', '2161558', '1289', '4.5', '4.0', '10.23', '12+', 'Photo & Video', '37', '0', '29', '1']
    
    
    ['529479190', 'Clash of Clans', '116476928', 'USD', '0.0', '2130805', '579', '4.5', '4.5', '9.24.12', '9+', 'Games', '38', '5', '18', '1']
    
    
    Number of rows: 7197
    Number of columns: 16
    
    
    ['Photo Editor & Candy Camera & Grid & ScrapBook', 'ART_AND_DESIGN', '4.1', '159', '19M', '10,000+', 'Free', '0', 'Everyone', 'Art & Design', 'January 7, 2018', '1.0.0', '4.0.3 and up']
    
    
    ['Coloring book moana', 'ART_AND_DESIGN', '3.9', '967', '14M', '500,000+', 'Free', '0', 'Everyone', 'Art & Design;Pretend Play', 'January 15, 2018', '2.0.0', '4.0.3 and up']
    
    
    ['U Launcher Lite – FREE Live Cool Themes, Hide Apps', 'ART_AND_DESIGN', '4.7', '87510', '8.7M', '5,000,000+', 'Free', '0', 'Everyone', 'Art & Design', 'August 1, 2018', '1.2.4', '4.0.3 and up']
    
    
    Number of rows: 10841
    Number of columns: 13
    ['id', 'track_name', 'size_bytes', 'currency', 'price', 'rating_count_tot', 'rating_count_ver', 'user_rating', 'user_rating_ver', 'ver', 'cont_rating', 'prime_genre', 'sup_devices.num', 'ipadSc_urls.num', 'lang.num', 'vpp_lic']
    
    
    ['App', 'Category', 'Rating', 'Reviews', 'Size', 'Installs', 'Type', 'Price', 'Content Rating', 'Genres', 'Last Updated', 'Current Ver', 'Android Ver']

# Data Cleaning

Before beginning our analysis, we need to make sure the data we analyze is accurate. This means that we need to:
 - Detect inaccurate data, and correct or remove it.
 - Detect duplicate data, and remove the duplicates.
 
Recall that at our company, we only build apps that are free to download and install, and that are directed toward an English-speaking audience. This means that we'll need to:
 - Remove all non-English apps
 - Remove apps that are not free
 
The Google Play data set has a dedicated discussion section, and we can see that one of the discussions describes an error for a certain row (row number **10472**). 


```python
print(google[10472])
```

    ['Life Made WI-Fi Touchscreen Photo Frame', '1.9', '19', '3.0M', '1,000+', 'Free', '0', 'Everyone', '', 'February 11, 2018', '1.0.19', '4.0 and up']


The row indeed has an error. It has a missing **'Rating'** column and thus,  a column shift happened for next columns. We are going to remove this row using the **`del`** command. Running this command more than once will result in more than one row being deleted


```python
del google[10472] # Run this command only once
```

The discussions section of the Google Play data set also mentions that some apps have **duplicate entries**, which you will notice upon further inspection of the data set. For example, **`Instagram`** has four entries:


```python
for app in google:
    name = app[0]
    if name == 'Instagram':
        print(app)
```

    ['Instagram', 'SOCIAL', '4.5', '66577313', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']
    ['Instagram', 'SOCIAL', '4.5', '66577446', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']
    ['Instagram', 'SOCIAL', '4.5', '66577313', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']
    ['Instagram', 'SOCIAL', '4.5', '66509917', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']


In total, there are **1181** cases where an app occurs more than once. How did we determine this?


```python
duplicate_apps = []
unique_apps = []

for app in google:
    name = app[0]
    if name in unique_apps:
        duplicate_apps.append(name)
    else:
        unique_apps.append(name)
        
print('Number Of Duplicate Apps: ',len(duplicate_apps))
print('\n')
print('Examples Of Duplicate Apps: ',duplicate_apps[:5])
```

    Number Of Duplicate Apps:  1181
    
    
    Examples Of Duplicate Apps:  ['Quick PDF Scanner + OCR FREE', 'Box', 'Google My Business', 'ZOOM Cloud Meetings', 'join.me - Simple Meetings']


Above, we:

 - Created two lists: One for storing the name of duplicate apps, and one for storing the name of unique apps
 - Looped through the **`google`** data set (Google Play data set), and for each iteration:
     - We saved the app name to a variable named **`name`**
     - If **`name`** was already in the **`unique_apps`** list, we appended **`name`** to the **`duplicate_apps`** list
     - Else (if **`name`** wasn't already in the **`unique_apps`** list), we appended **`name`** to the **`unique_apps`** list.

# Criterion For Removing Duplicate Apps

- We don't want to count certain apps more than once when we analyze data, so we need to remove the duplicate entries and keep only one entry per app.
- One thing we could do is remove the duplicate rows randomly, but we could probably find a better, more precise option.
- For example, if you examine the rows we printed for the Instagram app, the main difference happens on the fourth position of each row, which corresponds to the **number of reviews**.
- The different numbers show the data was collected at different times. The **higher** the **number of reviews**, the **more recent** the data should be.

Rather than removing duplicates randomly, we'll only keep the row with the highest number of reviews and remove the other entries for any given app.

Keep in mind, we looped through the Google Play data set and found that there are 1,181 duplicates. After we remove the duplicates, we should be left with **9,659 rows**:


```python
print('Expected Length: ', len(google) - 1181)
```

    Expected Length:  9659


To remove the duplicates, we will:
 - Create a **`dictionary`**, where each dictionary key is a **unique app name** and the corresponding dictionary value is the **highest number of reviews** of that app.
 - Use the information stored in the dictionary and create a new data set (`google_clean`), which will have only one entry per app (and for each app, we'll only select the entry with the highest number of reviews).


```python
reviews_max = {}

for app in google:
    name = app[0]
    n_reviews = float(app[3])
    
    if name in reviews_max and reviews_max[name] < n_reviews:
        reviews_max[name] = n_reviews
    elif name not in reviews_max:
        reviews_max[name] = n_reviews
```

Now, we will use the dictionary we created above to remove the duplicate rows:

 - We'll start by creating two empty lists: **`google_clean`** (which will store our new cleaned data set) and **`already_added`** (which will just store app names).
 - Loop through the **`google`** data set, and for each iteration:
   - Isolate the name of the app and the number of reviews
   - We add the current row (app) to the **`android_clean`** list, and
   the app name (name) to the already_cleaned list if:
    - The number of reviews of the current app matches the number
    of reviews of that app as described in the reviews_max
    dictionary; and
    - The name of the app is not already in the already_added
    list. 


```python
google_clean = []
already_added = []

for app in google:
    name = app[0]
    n_reviews = float(app[3])
    
    if (reviews_max[name] == n_reviews) and (name not in already_added):
        google_clean.append(app)
        already_added.append(name)        
```

The **`google_clean`** data set should have **9659** rows. We can confirm this:


```python
explore_data(google_clean, 0, 3, True)
```

    ['Photo Editor & Candy Camera & Grid & ScrapBook', 'ART_AND_DESIGN', '4.1', '159', '19M', '10,000+', 'Free', '0', 'Everyone', 'Art & Design', 'January 7, 2018', '1.0.0', '4.0.3 and up']
    
    
    ['U Launcher Lite – FREE Live Cool Themes, Hide Apps', 'ART_AND_DESIGN', '4.7', '87510', '8.7M', '5,000,000+', 'Free', '0', 'Everyone', 'Art & Design', 'August 1, 2018', '1.2.4', '4.0.3 and up']
    
    
    ['Sketch - Draw & Paint', 'ART_AND_DESIGN', '4.5', '215644', '25M', '50,000,000+', 'Free', '0', 'Teen', 'Art & Design', 'June 8, 2018', 'Varies with device', '4.2 and up']
    
    
    Number of rows: 9659
    Number of columns: 13


# Removing Non-Engish Apps

-  If we explore the data long enough, we'll find that both data sets have apps with names that suggest they are not directed toward an English-speaking audience. We'll need to remove these.

- One way to go about this is to remove each app with a name containing a symbol that is not commonly used in English text — English text usually includes letters from the English alphabet, numbers composed of digits from 0 to 9, punctuation marks (., !, ?, ;), and other symbols (+, *, /).

- Each character we use in a string has a corresponding number associated with it. For instance, the corresponding number for character **'a'** is **97**, character **'A'** is **65**, and character **'爱'** is **29,233**. We can get the corresponding number of each character using the **`ord()`** built-in function.


```python
# Examples on the use of the ord() function
print(ord('a'))
print(ord('z'))
print(ord('%'))
```

    97
    122
    37


- The numbers corresponding to the characters we commonly use in an English text are all in the range 0 to 127, according to the ASCII (American Standard Code for Information Interchange) system.

- Based on this number range, we can build a function that detects whether a character belongs to the set of common English characters or not.

- If the number is **equal to or less than 127**, then the character belongs to the set of common English characters.

- If an app name contains a character that is **greater than 127**, then it probably means that the app has a non-English name.

- In Python, strings are indexable and iterable, which means we can use indexing to select an individual character, and we can also iterate on the string using a for loop.


```python
# Example of using indexing to select individual characters
string = 'abc'
print(string[0])
print(string[1])
print(string[2])

print('\n')

# Example of iterating on a string using a for loop
for every_letter in string:
    print(every_letter)
```

    a
    b
    c
    
    
    a
    b
    c


Now let's create this function:


```python
def is_english(string):
    for every_letter in string:
        if ord(every_letter) > 127:
            return False
        
    return True
```

We'll test the above function to check whether the following app names are detected as English or non-English:
 - **Instagram**
 - **爱奇艺PPS -《欢乐颂2》电视剧热播**
 - **Docs To Go™ Free Office Suite**
 - **Instachat 😜**


```python
print(is_english('Instagram'))
print('\n')

print(is_english('爱奇艺PPS -《欢乐颂2》电视剧热播'))
print('\n')

print(is_english('Docs To Go™ Free Office Suite'))
print('\n')

print(is_english('Instachat 😜'))
```

    True
    
    
    False
    
    
    False
    
    
    False


- The function works only to a certain extent, because it may not be able to correctly identify certain English app names like 'Docs To Go™ Free Office Suite' and 'Instachat 😜'.

- This is because emojis and characters like ™ fall outside the ASCII range and have corresponding numbers over 127.

- If we're going to use the function we've created, we'll lose useful data since many English apps will be incorrectly labeled as non-English.

- To minimize the impact of data loss, we'll only remove an app if its name has **more than three characters with corresponding numbers falling outside the ASCII range**.

- This means all English apps with up to three emoji or other special characters will still be labeled as English. Our filter function is still not perfect, but it should be fairly effective.

The function is edited below, and we will subsequently use it to filter out the non-English apps:


```python
def is_english(string):
    non_ascii_counter = 0
    
    for every_letter in string:
        if ord(every_letter) > 127:
            non_ascii_counter += 1
            
    if non_ascii_counter > 3:
        return False
    else:
        return True
```


```python
# lists to store apps directed to an English audience
google_english = []
apple_english = []

for app in google_clean:
    name = app[0]
    if is_english(name):
        google_english.append(app)

for app in apple:
    name = app[1]
    if is_english(name):
        apple_english.append(app)

# Exploring the data sets to see how many rows are in each data set
explore_data(google_english, 0, 3, 'True')
print('\n')
explore_data(apple_english, 0, 3, 'True')
```

    ['Photo Editor & Candy Camera & Grid & ScrapBook', 'ART_AND_DESIGN', '4.1', '159', '19M', '10,000+', 'Free', '0', 'Everyone', 'Art & Design', 'January 7, 2018', '1.0.0', '4.0.3 and up']
    
    
    ['U Launcher Lite – FREE Live Cool Themes, Hide Apps', 'ART_AND_DESIGN', '4.7', '87510', '8.7M', '5,000,000+', 'Free', '0', 'Everyone', 'Art & Design', 'August 1, 2018', '1.2.4', '4.0.3 and up']
    
    
    ['Sketch - Draw & Paint', 'ART_AND_DESIGN', '4.5', '215644', '25M', '50,000,000+', 'Free', '0', 'Teen', 'Art & Design', 'June 8, 2018', 'Varies with device', '4.2 and up']
    
    
    Number of rows: 9614
    Number of columns: 13
    
    
    ['284882215', 'Facebook', '389879808', 'USD', '0.0', '2974676', '212', '3.5', '3.5', '95.0', '4+', 'Social Networking', '37', '1', '29', '1']
    
    
    ['389801252', 'Instagram', '113954816', 'USD', '0.0', '2161558', '1289', '4.5', '4.0', '10.23', '12+', 'Photo & Video', '37', '0', '29', '1']
    
    
    ['529479190', 'Clash of Clans', '116476928', 'USD', '0.0', '2130805', '579', '4.5', '4.5', '9.24.12', '9+', 'Games', '38', '5', '18', '1']
    
    
    Number of rows: 6183
    Number of columns: 16


We can see that we're left with 9614 Android apps and 6183 iOS apps.

# Isolating Free Apps

As we mentioned in the introduction, we only build apps that are free to download and install, and our main source of revenue consists of in-app ads. Our data sets contain both free and non-free apps; we'll need to isolate only the free apps for our analysis.

- Loop through each data set to isolate the free apps in separate lists, Making sure to identify the columns describing the app price correctly.

- After isolating the free apps, we"ll check the length of each data set to see how many apps are remaining.

Isolating the free apps will be our last step in the data cleaning process.


```python
# Isolating free apps in the Google Play data set
google_final = []

for apps in google_english:
    price = apps[7]
    if price == '0':
        google_final.append(apps)
        
# Isolating free apps in the Apple Store data set
apple_final = []

for apps in apple_english:
    price = apps[4]
    if price == '0.0':
        apple_final.append(apps)
        
# Checking the length of each data set
print('Number Of Android Apps Left: ',len(google_final))
print('Number Of iOS Apps Left: ',len(apple_final))
```

    Number Of Android Apps Left:  8864
    Number Of iOS Apps Left:  3222


So far, we spent a good amount of time on cleaning data, and:

- Removed inaccurate data
- Removed duplicate app entries
- Removed non-English apps
- Isolated the free apps

Now we are going to start analysing the data

As we mentioned in the introduction, our aim is to determine the kinds of apps that are likely to attract more users because our revenue is highly influenced by the number of people using our apps.

To minimize risks and overhead, our validation strategy for an app idea is comprised of three steps:

 1. Build a minimal Android version of the app, and add it to Google Play.
 2. If the app has a good response from users, we develop it further.
 3. If the app is profitable after six months, we build an iOS version of the app and add it to the App Store.
 
Because our end goal is to add the app on both Google Play and the App Store, we need to find app profiles that are successful on both markets. For instance, a profile that works well for both markets might be a productivity app that makes use of gamification.

Let's begin the analysis by getting a sense of what are the most common genres for each market. For this, we'll need to build frequency tables for a few columns in our data sets.

Upon inspection of the data sets, we conclude that the columns that will be useful for finding out what the most common genres in each market are, will be:

- **`prime_genre`** column of the App Store data set
- **`Genres`** and **`Category`** columns of the Google Play Data set

We'll build two functions we can use to analyze the frequency tables:

 - One function to generate frequency tables that show percentages

 - Another function we can use to display the percentages in a descending order, because we'll be generating these frequency tables using dictionaries, which have no order. 
 
 To do that, we'll need to make use of the built-in `sorted()` function. This function takes in an iterable data type (like a list, dictionary, tuple, etc.), and returns a list of the elements of that iterable sorted in ascending or descending order.
 
 Lets test this function on dictionaries:


```python
freq_table = {'Genre_1': 50, 'Genre_3': 20, 'Genre_2': 100}
sorted(freq_table)
```




    ['Genre_1', 'Genre_2', 'Genre_3']



The **`sorted()`** function doesn't work too well with dictionaries because it only considers and returns the **dictionary keys**, and not the **values**.

However, the **`sorted()`** function works well if we transform the dictionary into a **list of tuples**, where each tuple contains a **dictionary key** along with its corresponding **dictionary value**.

To ensure the sorting works right, the dictionary value **comes first**, and the dictionary key **comes second**:


```python
# Example of sorting dictionaries by first converting them to tuples
freq_table = {'Genre_1': 50, 'Genre_3': 20, 'Genre_2': 100}
freq_table_as_tuple = [(50, 'Genre_1'), (20, 'Genre_3'), (100, 'Genre_2')]
sorted(freq_table_as_tuple)
```




    [(20, 'Genre_3'), (50, 'Genre_1'), (100, 'Genre_2')]



Now we'll create a function that returns a **frequency table** and that will take in two inputs: 

 - **`dataset`**, which is expected to be a list of lists, and
 - **`index`**, which is expected to be an integer
 
 The function will return a frequency table **(as a dictionary)** for any column we want. The frequencies should also be expressed as **percentages**.


```python
def freq_table(dataset, index):
    table = {}
    total = 0
    
    for row in dataset:
        total += 1
        value = row[index]
        if value in table:
            table[value] += 1
        else:
            table[value] = 1
    
    table_percentages = {}
    for key in table:
        percentage = (table[key] / total) * 100
        table_percentages[key] = percentage 
    
    return table_percentages


def display_table(dataset, index):
    table = freq_table(dataset, index)
    table_display = []
    for key in table:
        key_val_as_tuple = (table[key], key)
        table_display.append(key_val_as_tuple)
        
    table_sorted = sorted(table_display, reverse = True)
    for entry in table_sorted:
        print(entry[1], ':', entry[0])
```

We'll start by examining the **`prime_genre`** column of the Apple Store data set


```python
display_table(apple_final, -5)
```

    Games : 58.16263190564867
    Entertainment : 7.883302296710118
    Photo & Video : 4.9658597144630665
    Education : 3.662321539416512
    Social Networking : 3.2898820608317814
    Shopping : 2.60707635009311
    Utilities : 2.5139664804469275
    Sports : 2.1415270018621975
    Music : 2.0484171322160147
    Health & Fitness : 2.0173805090006205
    Productivity : 1.7380509000620732
    Lifestyle : 1.5828677839851024
    News : 1.3345747982619491
    Travel : 1.2414649286157666
    Finance : 1.1173184357541899
    Weather : 0.8690254500310366
    Food & Drink : 0.8069522036002483
    Reference : 0.5586592178770949
    Business : 0.5276225946617008
    Book : 0.4345127250155183
    Navigation : 0.186219739292365
    Medical : 0.186219739292365
    Catalogs : 0.12414649286157665


We can see that among the free English apps, more than a half **(58.16%)** are **games**. **Entertainment** apps are close to **8%**, followed by **photo and video** apps, which are close to **5%**. Only **3.66%** of the apps are designed for **education**, followed by **social networking** apps which amount for **3.29%** of the apps in our data set.

The general impression is that App Store (at least the part containing free English apps) is dominated by apps that are designed for fun (games, entertainment, photo and video, social networking, sports, music, etc.), while apps with practical purposes (education, shopping, utilities, productivity, lifestyle, etc.) are more rare. However, the fact that fun apps are the most numerous doesn't also imply that they also have the greatest number of users — the demand might not be the same as the offer.

Let's continue by examining the Genres and Category columns of the Google Play data set (two columns which seem to be related).


```python
display_table(google_clean, 1) # Category Column
```

    FAMILY : 19.40159436794699
    GAME : 9.793974531525002
    TOOLS : 8.582669013355419
    BUSINESS : 4.348276219070297
    MEDICAL : 4.089450253649446
    PERSONALIZATION : 3.892742519929599
    PRODUCTIVITY : 3.8720364426959315
    LIFESTYLE : 3.820271249611761
    FINANCE : 3.571798322807744
    SPORTS : 3.3647375504710633
    COMMUNICATION : 3.2612071643027227
    HEALTH_AND_FITNESS : 2.9816751216482036
    PHOTOGRAPHY : 2.9092038513303655
    NEWS_AND_MAGAZINES : 2.6296718086758464
    SOCIAL : 2.474376229423336
    BOOKS_AND_REFERENCE : 2.298374572937157
    TRAVEL_AND_LOCAL : 2.2673154570866547
    SHOPPING : 2.0913138006004766
    DATING : 1.760016564861787
    VIDEO_PLAYERS : 1.6978983331607829
    MAPS_AND_NAVIGATION : 1.3562480588052592
    FOOD_AND_DRINK : 1.1595403250854126
    EDUCATION : 1.1077751320012423
    ENTERTAINMENT : 0.9007143596645615
    AUTO_AND_VEHICLES : 0.8800082824308935
    LIBRARIES_AND_DEMO : 0.8696552438140595
    WEATHER : 0.8178900507298892
    HOUSE_AND_HOME : 0.7557718190288849
    EVENTS : 0.6625944714773786
    ART_AND_DESIGN : 0.6315353556268765
    PARENTING : 0.6211823170100425
    COMICS : 0.5797701625427063
    BEAUTY : 0.5487110466922042


The landscape seems significantly different on Google Play: there are not that many apps designed for fun, and it seems that a good number of apps are designed for practical purposes (family, tools, business, lifestyle, productivity, etc.). However, if we investigate this further, we can see that the family category (which accounts for almost 19% of the apps) means mostly games for kids.

Even so, practical apps seem to have a better representation on Google Play compared to App Store. This picture is also confirmed by the frequency table we see for the Genres column:


```python
display_table(google_final, -4)
```

    Tools : 8.449909747292418
    Entertainment : 6.069494584837545
    Education : 5.347472924187725
    Business : 4.591606498194946
    Productivity : 3.892148014440433
    Lifestyle : 3.892148014440433
    Finance : 3.7003610108303246
    Medical : 3.531137184115524
    Sports : 3.463447653429603
    Personalization : 3.3167870036101084
    Communication : 3.2378158844765346
    Action : 3.1024368231046933
    Health & Fitness : 3.0798736462093865
    Photography : 2.944494584837545
    News & Magazines : 2.7978339350180503
    Social : 2.6624548736462095
    Travel & Local : 2.3240072202166067
    Shopping : 2.2450361010830324
    Books & Reference : 2.1435018050541514
    Simulation : 2.0419675090252705
    Dating : 1.861462093862816
    Arcade : 1.8501805054151623
    Video Players & Editors : 1.7712093862815883
    Casual : 1.7599277978339352
    Maps & Navigation : 1.3989169675090252
    Food & Drink : 1.2409747292418771
    Puzzle : 1.128158844765343
    Racing : 0.9927797833935018
    Role Playing : 0.9363718411552346
    Libraries & Demo : 0.9363718411552346
    Auto & Vehicles : 0.9250902527075812
    Strategy : 0.9138086642599278
    House & Home : 0.8235559566787004
    Weather : 0.8009927797833934
    Events : 0.7107400722021661
    Adventure : 0.6768953068592057
    Comics : 0.6092057761732852
    Beauty : 0.5979241877256317
    Art & Design : 0.5979241877256317
    Parenting : 0.4963898916967509
    Card : 0.45126353790613716
    Casino : 0.42870036101083037
    Trivia : 0.41741877256317694
    Educational;Education : 0.39485559566787
    Board : 0.3835740072202166
    Educational : 0.3722924187725632
    Education;Education : 0.33844765342960287
    Word : 0.2594765342960289
    Casual;Pretend Play : 0.236913357400722
    Music : 0.2030685920577617
    Racing;Action & Adventure : 0.16922382671480143
    Puzzle;Brain Games : 0.16922382671480143
    Entertainment;Music & Video : 0.16922382671480143
    Casual;Brain Games : 0.13537906137184114
    Casual;Action & Adventure : 0.13537906137184114
    Arcade;Action & Adventure : 0.12409747292418773
    Action;Action & Adventure : 0.10153429602888085
    Educational;Pretend Play : 0.09025270758122744
    Simulation;Action & Adventure : 0.078971119133574
    Parenting;Education : 0.078971119133574
    Entertainment;Brain Games : 0.078971119133574
    Board;Brain Games : 0.078971119133574
    Parenting;Music & Video : 0.06768953068592057
    Educational;Brain Games : 0.06768953068592057
    Casual;Creativity : 0.06768953068592057
    Art & Design;Creativity : 0.06768953068592057
    Education;Pretend Play : 0.056407942238267145
    Role Playing;Pretend Play : 0.04512635379061372
    Education;Creativity : 0.04512635379061372
    Role Playing;Action & Adventure : 0.033844765342960284
    Puzzle;Action & Adventure : 0.033844765342960284
    Entertainment;Creativity : 0.033844765342960284
    Entertainment;Action & Adventure : 0.033844765342960284
    Educational;Creativity : 0.033844765342960284
    Educational;Action & Adventure : 0.033844765342960284
    Education;Music & Video : 0.033844765342960284
    Education;Brain Games : 0.033844765342960284
    Education;Action & Adventure : 0.033844765342960284
    Adventure;Action & Adventure : 0.033844765342960284
    Video Players & Editors;Music & Video : 0.02256317689530686
    Sports;Action & Adventure : 0.02256317689530686
    Simulation;Pretend Play : 0.02256317689530686
    Puzzle;Creativity : 0.02256317689530686
    Music;Music & Video : 0.02256317689530686
    Entertainment;Pretend Play : 0.02256317689530686
    Casual;Education : 0.02256317689530686
    Board;Action & Adventure : 0.02256317689530686
    Video Players & Editors;Creativity : 0.01128158844765343
    Trivia;Education : 0.01128158844765343
    Travel & Local;Action & Adventure : 0.01128158844765343
    Tools;Education : 0.01128158844765343
    Strategy;Education : 0.01128158844765343
    Strategy;Creativity : 0.01128158844765343
    Strategy;Action & Adventure : 0.01128158844765343
    Simulation;Education : 0.01128158844765343
    Role Playing;Brain Games : 0.01128158844765343
    Racing;Pretend Play : 0.01128158844765343
    Puzzle;Education : 0.01128158844765343
    Parenting;Brain Games : 0.01128158844765343
    Music & Audio;Music & Video : 0.01128158844765343
    Lifestyle;Pretend Play : 0.01128158844765343
    Lifestyle;Education : 0.01128158844765343
    Health & Fitness;Education : 0.01128158844765343
    Health & Fitness;Action & Adventure : 0.01128158844765343
    Entertainment;Education : 0.01128158844765343
    Communication;Creativity : 0.01128158844765343
    Comics;Creativity : 0.01128158844765343
    Casual;Music & Video : 0.01128158844765343
    Card;Action & Adventure : 0.01128158844765343
    Books & Reference;Education : 0.01128158844765343
    Art & Design;Pretend Play : 0.01128158844765343
    Art & Design;Action & Adventure : 0.01128158844765343
    Arcade;Pretend Play : 0.01128158844765343
    Adventure;Education : 0.01128158844765343


The difference between the Genres and the Category columns is not crystal clear, but one thing we can notice is that the Genres column is much more granular (it has more categories). We're only looking for the bigger picture at the moment, so we'll only work with the Category column moving forward.

Up to this point, we found that the App Store is dominated by apps designed for fun, while Google Play shows a more balanced landscape of both practical and for-fun apps. Now we'd like to get an idea about the kind of apps that have most users.

One way to find out what genres are the most popular (have the most users) is to calculate the **average number of installs** for each app genre. For the Google Play data set, we can find this information in the **`Installs`** column, but this information is missing for the App Store data set. As a workaround, we'll take the total number of user ratings as a proxy, which we can find in the **`rating_count_tot app`**.

Let's start with calculating the average number of user ratings per app genre on the App Store. To do that, we'll need to:

 - **Isolate the apps of each genre.**
 - **Sum up the user ratings for the apps of that genre.**
 - **Divide the sum by the number of apps belonging to that genre** (not by the total number of apps).
 
To calculate the average number of user ratings for each genre, we'll use a **for loop inside of another for loop** (nested loop). This is an example of a nested loop:


```python
some_strings = ['FIRST', 'SECOND']
some_integers = [1, 2, 3, 4, 5]

for every_string in some_strings:
    print(every_string)
    
    for every_integer in some_integers:
        print(every_integer)
```

    FIRST
    1
    2
    3
    4
    5
    SECOND
    1
    2
    3
    4
    5


 Notice that all the elements of the list **`some_integers`** are printed for each of the two iterations over the list **`some_strings`**.


```python
iOS_genres = freq_table(apple_final, -5)

for genre in iOS_genres:
    total = 0  # store the sum of user ratings specific to each genre.
    len_genre = 0 # store the number of apps specific to each genre.
    
    for app in apple_final:
        genre_app = app[-5]
        if genre_app == genre:
            n_ratings = float(app[5])
            total += n_ratings
            len_genre += 1
    avg_n_ratings = total / len_genre
    print(genre, ':', avg_n_ratings)
```

    Reference : 74942.11111111111
    Lifestyle : 16485.764705882353
    Catalogs : 4004.0
    Games : 22788.6696905016
    Navigation : 86090.33333333333
    Utilities : 18684.456790123455
    Travel : 28243.8
    Education : 7003.983050847458
    Sports : 23008.898550724636
    Entertainment : 14029.830708661417
    Shopping : 26919.690476190477
    Book : 39758.5
    Health & Fitness : 23298.015384615384
    Productivity : 21028.410714285714
    Business : 7491.117647058823
    Photo & Video : 28441.54375
    Medical : 612.0
    News : 21248.023255813954
    Food & Drink : 33333.92307692308
    Finance : 31467.944444444445
    Music : 57326.530303030304
    Social Networking : 71548.34905660378
    Weather : 52279.892857142855


On average, navigation apps have the highest number of user reviews, but this figure is heavily influenced by Waze and Google Maps, which have close to half a million user reviews together:


```python
for app in apple_final:
    name = app[1]
    genre = app[-5]
    u_rating = app[5]
    
    if genre == 'Navigation':
        print(name, ' : ', u_rating)   
```

    Waze - GPS Navigation, Maps & Real-time Traffic  :  345046
    Google Maps - Navigation & Transit  :  154911
    Geocaching®  :  12811
    CoPilot GPS – Car Navigation & Offline Maps  :  3582
    ImmobilienScout24: Real Estate Search in Germany  :  187
    Railway Route Search  :  5


The same pattern applies to social networking apps, where the average number is heavily influenced by a few giants like Facebook, Pinterest, Skype, etc. Same applies to music apps, where a few big players like Pandora, Spotify, and Shazam heavily influence the average number.

Our aim is to find popular genres, but navigation, social networking or music apps might seem more popular than they really are. The average number of ratings seem to be skewed by very few apps which have hundreds of thousands of user ratings, while the other apps may struggle to get past the 10,000 threshold. We could get a better picture by removing these extremely popular apps for each genre and then rework the averages, but we'll leave this level of detail for later.

Reference apps have 74,942 user ratings on average, but it's actually the Bible and Dictionary.com which skew up the average rating:


```python
for app in apple_final:
    name = app[1]
    genre = app[-5]
    u_rating = app[5]
    
    if genre == 'Reference':
        print(name, ' : ', u_rating)   
```

    Bible  :  985920
    Dictionary.com Dictionary & Thesaurus  :  200047
    Dictionary.com Dictionary & Thesaurus for iPad  :  54175
    Google Translate  :  26786
    Muslim Pro: Ramadan 2017 Prayer Times, Azan, Quran  :  18418
    New Furniture Mods - Pocket Wiki & Game Tools for Minecraft PC Edition  :  17588
    Merriam-Webster Dictionary  :  16849
    Night Sky  :  12122
    City Maps for Minecraft PE - The Best Maps for Minecraft Pocket Edition (MCPE)  :  8535
    LUCKY BLOCK MOD ™ for Minecraft PC Edition - The Best Pocket Wiki & Mods Installer Tools  :  4693
    GUNS MODS for Minecraft PC Edition - Mods Tools  :  1497
    Guides for Pokémon GO - Pokemon GO News and Cheats  :  826
    WWDC  :  762
    Horror Maps for Minecraft PE - Download The Scariest Maps for Minecraft Pocket Edition (MCPE) Free  :  718
    VPN Express  :  14
    Real Bike Traffic Rider Virtual Reality Glasses  :  8
    教えて!goo  :  0
    Jishokun-Japanese English Dictionary & Translator  :  0


However, this niche seems to show some potential. One thing we could do is take another popular book and turn it into an app where we could add different features besides the raw version of the book. This might include daily quotes from the book, an audio version of the book, quizzes about the book, etc. On top of that, we could also embed a dictionary within the app, so users don't need to exit our app to look up words in an external app.

This idea seems to fit well with the fact that the App Store is dominated by for-fun apps. This suggests the market might be a bit saturated with for-fun apps, which means a practical app might have more of a chance to stand out among the huge number of apps on the App Store.

Other genres that seem popular include weather, book, food and drink, or finance. The book genre seem to overlap a bit with the app idea we described above, but the other genres don't seem too interesting to us:
 
 - **Weather apps** — people generally don't spend too much time in-app, and the chances of making profit from in-app adds are low. Also, getting reliable live weather data may require us to connect our apps to non-free APIs.

 - **Food and drink** — examples here include Starbucks, Dunkin' Donuts, McDonald's, etc. So making a popular food and drink app requires actual cooking and a delivery service, which is outside the scope of our company.

 - **Finance apps** — these apps involve banking, paying bills, money transfer, etc. Building a finance app requires domain knowledge, and we don't want to hire a finance expert just to build an app.
 
 Now let's analyze the Google Play market a bit.


```python
display_table(google_final, 5)
```

    1,000,000+ : 15.726534296028879
    100,000+ : 11.552346570397113
    10,000,000+ : 10.548285198555957
    10,000+ : 10.198555956678701
    1,000+ : 8.393501805054152
    100+ : 6.915613718411552
    5,000,000+ : 6.825361010830325
    500,000+ : 5.561823104693141
    50,000+ : 4.7721119133574
    5,000+ : 4.512635379061372
    10+ : 3.5424187725631766
    500+ : 3.2490974729241873
    50,000,000+ : 2.3014440433213
    100,000,000+ : 2.1322202166064983
    50+ : 1.917870036101083
    5+ : 0.78971119133574
    1+ : 0.5076714801444043
    500,000,000+ : 0.2707581227436823
    1,000,000,000+ : 0.22563176895306858
    0+ : 0.04512635379061372
    0 : 0.01128158844765343


We have data about the number of installs for the Google Play market, so we should be able to get a clearer picture about genre popularity. However, the install numbers don't seem precise enough — we can see that most values are open-ended (100+, 1,000+, 5,000+, etc.). 

For instance, we don't know whether an app with 100,000+ installs has 100,000 installs, 200,000, or 350,000. However, we don't need very precise data for our purposes — we only want to find out which app genres attract the most users, and we don't need perfect precision with respect to the number of users.

We're going to leave the numbers as they are, which means that we'll consider that an app with 100,000+ installs has 100,000 installs, and an app with 1,000,000+ installs has 1,000,000 installs, and so on. To perform computations, however, we'll need to convert each install number from string to float. This means we need to remove the commas and the plus characters, otherwise the conversion will fail and raise an error.

To remove characters from strings, we can use **`str.replace(old, new)`** method, which takes in two parameters, **`old`** and **`new`**, and replaces all occurrences of `old` within a string with `new`:


```python
n_installs = '100,000+'
print(n_installs.replace('+', 'plus'))

# Removing characters by replacing them with the empty string
print(n_installs.replace(',', ''))
```

    100,000plus
    100000+



```python
google_categories = freq_table(google_final, 1)

for category in google_categories:
    total = 0 # store the sum of installs specific to each genre.
    len_category = 0 #  store the number of apps specific to each genre.
    
    for app in google_final:
        category_app = app[1]
        if category_app == category:
            n_installs = app[5]
            n_installs = n_installs.replace('+', '')
            n_installs = n_installs.replace(',', '')
            n_installs = float(n_installs)
            total += n_installs
            len_category += 1
            
    avg_installs = total / len_category
    print(category, ':', avg_installs)
```

    PRODUCTIVITY : 16787331.344927534
    ART_AND_DESIGN : 1986335.0877192982
    MAPS_AND_NAVIGATION : 4056941.7741935486
    VIDEO_PLAYERS : 24727872.452830188
    EDUCATION : 1833495.145631068
    BUSINESS : 1712290.1474201474
    BEAUTY : 513151.88679245283
    COMMUNICATION : 38456119.167247385
    COMICS : 817657.2727272727
    FINANCE : 1387692.475609756
    ENTERTAINMENT : 11640705.88235294
    NEWS_AND_MAGAZINES : 9549178.467741935
    SPORTS : 3638640.1428571427
    FOOD_AND_DRINK : 1924897.7363636363
    PERSONALIZATION : 5201482.6122448975
    AUTO_AND_VEHICLES : 647317.8170731707
    HOUSE_AND_HOME : 1331540.5616438356
    FAMILY : 3695641.8198090694
    MEDICAL : 120550.61980830671
    SOCIAL : 23253652.127118643
    HEALTH_AND_FITNESS : 4188821.9853479853
    PHOTOGRAPHY : 17840110.40229885
    EVENTS : 253542.22222222222
    GAME : 15588015.603248259
    TRAVEL_AND_LOCAL : 13984077.710144928
    SHOPPING : 7036877.311557789
    LIFESTYLE : 1437816.2687861272
    DATING : 854028.8303030303
    BOOKS_AND_REFERENCE : 8767811.894736841
    PARENTING : 542603.6206896552
    LIBRARIES_AND_DEMO : 638503.734939759
    TOOLS : 10801391.298666667
    WEATHER : 5074486.197183099


On average, communication apps have the most installs: 38,456,119. This number is heavily skewed up by a few apps that have over one billion installs (WhatsApp, Facebook Messenger, Skype, Google Chrome, Gmail, and Hangouts), and a few others with over 100 and 500 million installs:


```python
for app in google_final:
    name = app[0]
    category = app[1]
    installs = app[5]
    if category == 'COMMUNICATION' and (installs == '1,000,000,000+' or installs == '500,000,000+' or installs == '100,000,000+'):
        print(name, ':', installs)
```

    WhatsApp Messenger : 1,000,000,000+
    imo beta free calls and text : 100,000,000+
    Android Messages : 100,000,000+
    Google Duo - High Quality Video Calls : 500,000,000+
    Messenger – Text and Video Chat for Free : 1,000,000,000+
    imo free video calls and chat : 500,000,000+
    Skype - free IM & video calls : 1,000,000,000+
    Who : 100,000,000+
    GO SMS Pro - Messenger, Free Themes, Emoji : 100,000,000+
    LINE: Free Calls & Messages : 500,000,000+
    Google Chrome: Fast & Secure : 1,000,000,000+
    Firefox Browser fast & private : 100,000,000+
    UC Browser - Fast Download Private & Secure : 500,000,000+
    Gmail : 1,000,000,000+
    Hangouts : 1,000,000,000+
    Messenger Lite: Free Calls & Messages : 100,000,000+
    Kik : 100,000,000+
    KakaoTalk: Free Calls & Text : 100,000,000+
    Opera Mini - fast web browser : 100,000,000+
    Opera Browser: Fast and Secure : 100,000,000+
    Telegram : 100,000,000+
    Truecaller: Caller ID, SMS spam blocking & Dialer : 100,000,000+
    UC Browser Mini -Tiny Fast Private & Secure : 100,000,000+
    Viber Messenger : 500,000,000+
    WeChat : 100,000,000+
    Yahoo Mail – Stay Organized : 100,000,000+
    BBM - Free Calls & Messages : 100,000,000+


If we removed all the communication apps that have over 100 million installs, the average would be reduced roughly ten times:


```python
under_100_m = []

for app in google_final:
    n_installs = app[5]
    n_installs = n_installs.replace(',', '')
    n_installs = n_installs.replace('+', '')
    if (app[1] == 'COMMUNICATION') and (float(n_installs) < 100000000):
        under_100_m.append(float(n_installs))
        
sum(under_100_m) / len(under_100_m)
```




    3603485.3884615386



We see the same pattern for the video players category, which is the runner-up with 24,727,872 installs. The market is dominated by apps like Youtube, Google Play Movies & TV, or MX Player. The pattern is repeated for social apps (where we have giants like Facebook, Instagram, Google+, etc.), photography apps (Google Photos and other popular photo editors), or productivity apps (Microsoft Word, Dropbox, Google Calendar, Evernote, etc.).

Again, the main concern is that these app genres might seem more popular than they really are. Moreover, these niches seem to be dominated by a few giants who are hard to compete against.

The game genre seems pretty popular, but previously we found out this part of the market seems a bit saturated, so we'd like to come up with a different app recommendation if possible.

The books and reference genre looks fairly popular as well, with an average number of installs of 8,767,811. It's interesting to explore this in more depth, since we found this genre has some potential to work well on the App Store, and our aim is to recommend an app genre that shows potential for being profitable on both the App Store and Google Play.

Let's take a look at some of the apps from this genre and their number of installs:


```python
for app in google_final:
    if app[1] == 'BOOKS_AND_REFERENCE':
        print(app[0], ':', app[5])
```

    E-Book Read - Read Book for free : 50,000+
    Download free book with green book : 100,000+
    Wikipedia : 10,000,000+
    Cool Reader : 10,000,000+
    Free Panda Radio Music : 100,000+
    Book store : 1,000,000+
    FBReader: Favorite Book Reader : 10,000,000+
    English Grammar Complete Handbook : 500,000+
    Free Books - Spirit Fanfiction and Stories : 1,000,000+
    Google Play Books : 1,000,000,000+
    AlReader -any text book reader : 5,000,000+
    Offline English Dictionary : 100,000+
    Offline: English to Tagalog Dictionary : 500,000+
    FamilySearch Tree : 1,000,000+
    Cloud of Books : 1,000,000+
    Recipes of Prophetic Medicine for free : 500,000+
    ReadEra – free ebook reader : 1,000,000+
    Anonymous caller detection : 10,000+
    Ebook Reader : 5,000,000+
    Litnet - E-books : 100,000+
    Read books online : 5,000,000+
    English to Urdu Dictionary : 500,000+
    eBoox: book reader fb2 epub zip : 1,000,000+
    English Persian Dictionary : 500,000+
    Flybook : 500,000+
    All Maths Formulas : 1,000,000+
    Ancestry : 5,000,000+
    HTC Help : 10,000,000+
    English translation from Bengali : 100,000+
    Pdf Book Download - Read Pdf Book : 100,000+
    Free Book Reader : 100,000+
    eBoox new: Reader for fb2 epub zip books : 50,000+
    Only 30 days in English, the guideline is guaranteed : 500,000+
    Moon+ Reader : 10,000,000+
    SH-02J Owner's Manual (Android 8.0) : 50,000+
    English-Myanmar Dictionary : 1,000,000+
    Golden Dictionary (EN-AR) : 1,000,000+
    All Language Translator Free : 1,000,000+
    Azpen eReader : 500,000+
    URBANO V 02 instruction manual : 100,000+
    Bible : 100,000,000+
    C Programs and Reference : 50,000+
    C Offline Tutorial : 1,000+
    C Programs Handbook : 50,000+
    Amazon Kindle : 100,000,000+
    Aab e Hayat Full Novel : 100,000+
    Aldiko Book Reader : 10,000,000+
    Google I/O 2018 : 500,000+
    R Language Reference Guide : 10,000+
    Learn R Programming Full : 5,000+
    R Programing Offline Tutorial : 1,000+
    Guide for R Programming : 5+
    Learn R Programming : 10+
    R Quick Reference Big Data : 1,000+
    V Made : 100,000+
    Wattpad 📖 Free Books : 100,000,000+
    Dictionary - WordWeb : 5,000,000+
    Guide (for X-MEN) : 100,000+
    AC Air condition Troubleshoot,Repair,Maintenance : 5,000+
    AE Bulletins : 1,000+
    Ae Allah na Dai (Rasa) : 10,000+
    50000 Free eBooks & Free AudioBooks : 5,000,000+
    Ag PhD Field Guide : 10,000+
    Ag PhD Deficiencies : 10,000+
    Ag PhD Planting Population Calculator : 1,000+
    Ag PhD Soybean Diseases : 1,000+
    Fertilizer Removal By Crop : 50,000+
    A-J Media Vault : 50+
    Al-Quran (Free) : 10,000,000+
    Al Quran (Tafsir & by Word) : 500,000+
    Al Quran Indonesia : 10,000,000+
    Al'Quran Bahasa Indonesia : 10,000,000+
    Al Quran Al karim : 1,000,000+
    Al-Muhaffiz : 50,000+
    Al Quran : EAlim - Translations & MP3 Offline : 5,000,000+
    Al-Quran 30 Juz free copies : 500,000+
    Koran Read &MP3 30 Juz Offline : 1,000,000+
    Hafizi Quran 15 lines per page : 1,000,000+
    Quran for Android : 10,000,000+
    Surah Al-Waqiah : 100,000+
    Hisnul Al Muslim - Hisn Invocations & Adhkaar : 100,000+
    Satellite AR : 1,000,000+
    Audiobooks from Audible : 100,000,000+
    Kinot & Eichah for Tisha B'Av : 10,000+
    AW Tozer Devotionals - Daily : 5,000+
    Tozer Devotional -Series 1 : 1,000+
    The Pursuit of God : 1,000+
    AY Sing : 5,000+
    Ay Hasnain k Nana Milad Naat : 10,000+
    Ay Mohabbat Teri Khatir Novel : 10,000+
    Arizona Statutes, ARS (AZ Law) : 1,000+
    Oxford A-Z of English Usage : 1,000,000+
    BD Fishpedia : 1,000+
    BD All Sim Offer : 10,000+
    Youboox - Livres, BD et magazines : 500,000+
    B&H Kids AR : 10,000+
    B y H Niños ES : 5,000+
    Dictionary.com: Find Definitions for English Words : 10,000,000+
    English Dictionary - Offline : 10,000,000+
    Bible KJV : 5,000,000+
    Borneo Bible, BM Bible : 10,000+
    MOD Black for BM : 100+
    BM Box : 1,000+
    Anime Mod for BM : 100+
    NOOK: Read eBooks & Magazines : 10,000,000+
    NOOK Audiobooks : 500,000+
    NOOK App for NOOK Devices : 500,000+
    Browsery by Barnes & Noble : 5,000+
    bp e-store : 1,000+
    Brilliant Quotes: Life, Love, Family & Motivation : 1,000,000+
    BR Ambedkar Biography & Quotes : 10,000+
    BU Alsace : 100+
    Catholic La Bu Zo Kam : 500+
    Khrifa Hla Bu (Solfa) : 10+
    Kristian Hla Bu : 10,000+
    SA HLA BU : 1,000+
    Learn SAP BW : 500+
    Learn SAP BW on HANA : 500+
    CA Laws 2018 (California Laws and Codes) : 5,000+
    Bootable Methods(USB-CD-DVD) : 10,000+
    cloudLibrary : 100,000+
    SDA Collegiate Quarterly : 500+
    Sabbath School : 100,000+
    Cypress College Library : 100+
    Stats Royale for Clash Royale : 1,000,000+
    GATE 21 years CS Papers(2011-2018 Solved) : 50+
    Learn CT Scan Of Head : 5,000+
    Easy Cv maker 2018 : 10,000+
    How to Write CV : 100,000+
    CW Nuclear : 1,000+
    CY Spray nozzle : 10+
    BibleRead En Cy Zh Yue : 5+
    CZ-Help : 5+
    Modlitební knížka CZ : 500+
    Guide for DB Xenoverse : 10,000+
    Guide for DB Xenoverse 2 : 10,000+
    Guide for IMS DB : 10+
    DC HSEMA : 5,000+
    DC Public Library : 1,000+
    Painting Lulu DC Super Friends : 1,000+
    Dictionary : 10,000,000+
    Fix Error Google Playstore : 1,000+
    D. H. Lawrence Poems FREE : 1,000+
    Bilingual Dictionary Audio App : 5,000+
    DM Screen : 10,000+
    wikiHow: how to do anything : 1,000,000+
    Dr. Doug's Tips : 1,000+
    Bible du Semeur-BDS (French) : 50,000+
    La citadelle du musulman : 50,000+
    DV 2019 Entry Guide : 10,000+
    DV 2019 - EDV Photo & Form : 50,000+
    DV 2018 Winners Guide : 1,000+
    EB Annual Meetings : 1,000+
    EC - AP & Telangana : 5,000+
    TN Patta Citta & EC : 10,000+
    AP Stamps and Registration : 10,000+
    CompactiMa EC pH Calibration : 100+
    EGW Writings 2 : 100,000+
    EGW Writings : 1,000,000+
    Bible with EGW Comments : 100,000+
    My Little Pony AR Guide : 1,000,000+
    SDA Sabbath School Quarterly : 500,000+
    Duaa Ek Ibaadat : 5,000+
    Spanish English Translator : 10,000,000+
    Dictionary - Merriam-Webster : 10,000,000+
    JW Library : 10,000,000+
    Oxford Dictionary of English : Free : 10,000,000+
    English Hindi Dictionary : 10,000,000+
    English to Hindi Dictionary : 5,000,000+
    EP Research Service : 1,000+
    Hymnes et Louanges : 100,000+
    EU Charter : 1,000+
    EU Data Protection : 1,000+
    EU IP Codes : 100+
    EW PDF : 5+
    BakaReader EX : 100,000+
    EZ Quran : 50,000+
    FA Part 1 & 2 Past Papers Solved Free – Offline : 5,000+
    La Fe de Jesus : 1,000+
    La Fe de Jesús : 500+
    Le Fe de Jesus : 500+
    Florida - Pocket Brainbook : 1,000+
    Florida Statutes (FL Code) : 1,000+
    English To Shona Dictionary : 10,000+
    Greek Bible FP (Audio) : 1,000+
    Golden Dictionary (FR-AR) : 500,000+
    Fanfic-FR : 5,000+
    Bulgarian French Dictionary Fr : 10,000+
    Chemin (fr) : 1,000+
    The SCP Foundation DB fr nn5n : 1,000+


The book and reference genre includes a variety of apps: software for processing and reading ebooks, various collections of libraries, dictionaries, tutorials on programming or languages, etc. It seems there's still a small number of extremely popular apps that skew the average:


```python
for app in google_final:
    if app[1] == 'BOOKS_AND_REFERENCE' and (app[5] == '1,000,000,000+' or app[5] == '500,000,000+' or app[5] == '100,000,000+'):
        print(app[0], ':', app[5])
```

    Google Play Books : 1,000,000,000+
    Bible : 100,000,000+
    Amazon Kindle : 100,000,000+
    Wattpad 📖 Free Books : 100,000,000+
    Audiobooks from Audible : 100,000,000+


However, it looks like there are only a few very popular apps, so this market still shows potential. Let's try to get some app ideas based on the kind of apps that are somewhere in the middle in terms of popularity (between 1,000,000 and 100,000,000 downloads):


```python
for app in google_final:
    if app[1] == 'BOOKS_AND_REFERENCE' and (app[5] == '1,000,000+' or app[5] == '5,000,000+' or app[5] == '10,000,000+' or app[5] == '50,000,000+'):
        print(app[0], ':', app[5])
```

    Wikipedia : 10,000,000+
    Cool Reader : 10,000,000+
    Book store : 1,000,000+
    FBReader: Favorite Book Reader : 10,000,000+
    Free Books - Spirit Fanfiction and Stories : 1,000,000+
    AlReader -any text book reader : 5,000,000+
    FamilySearch Tree : 1,000,000+
    Cloud of Books : 1,000,000+
    ReadEra – free ebook reader : 1,000,000+
    Ebook Reader : 5,000,000+
    Read books online : 5,000,000+
    eBoox: book reader fb2 epub zip : 1,000,000+
    All Maths Formulas : 1,000,000+
    Ancestry : 5,000,000+
    HTC Help : 10,000,000+
    Moon+ Reader : 10,000,000+
    English-Myanmar Dictionary : 1,000,000+
    Golden Dictionary (EN-AR) : 1,000,000+
    All Language Translator Free : 1,000,000+
    Aldiko Book Reader : 10,000,000+
    Dictionary - WordWeb : 5,000,000+
    50000 Free eBooks & Free AudioBooks : 5,000,000+
    Al-Quran (Free) : 10,000,000+
    Al Quran Indonesia : 10,000,000+
    Al'Quran Bahasa Indonesia : 10,000,000+
    Al Quran Al karim : 1,000,000+
    Al Quran : EAlim - Translations & MP3 Offline : 5,000,000+
    Koran Read &MP3 30 Juz Offline : 1,000,000+
    Hafizi Quran 15 lines per page : 1,000,000+
    Quran for Android : 10,000,000+
    Satellite AR : 1,000,000+
    Oxford A-Z of English Usage : 1,000,000+
    Dictionary.com: Find Definitions for English Words : 10,000,000+
    English Dictionary - Offline : 10,000,000+
    Bible KJV : 5,000,000+
    NOOK: Read eBooks & Magazines : 10,000,000+
    Brilliant Quotes: Life, Love, Family & Motivation : 1,000,000+
    Stats Royale for Clash Royale : 1,000,000+
    Dictionary : 10,000,000+
    wikiHow: how to do anything : 1,000,000+
    EGW Writings : 1,000,000+
    My Little Pony AR Guide : 1,000,000+
    Spanish English Translator : 10,000,000+
    Dictionary - Merriam-Webster : 10,000,000+
    JW Library : 10,000,000+
    Oxford Dictionary of English : Free : 10,000,000+
    English Hindi Dictionary : 10,000,000+
    English to Hindi Dictionary : 5,000,000+


This niche seems to be dominated by software for processing and reading ebooks, as well as various collections of libraries and dictionaries, so it's probably not a good idea to build similar apps since there'll be some significant competition.

We also notice there are quite a few apps built around the book Quran, which suggests that building an app around a popular book can be profitable. It seems that taking a popular book (perhaps a more recent book) and turning it into an app could be profitable for both the Google Play and the App Store markets.

However, it looks like the market is already full of libraries, so we need to add some special features besides the raw version of the book. This might include daily quotes from the book, an audio version of the book, quizzes on the book, a forum where people can discuss the book, etc.

# Conclusions

In this project, we analyzed data about the App Store and Google Play mobile apps with the goal of recommending an app profile that can be profitable for both markets.

We concluded that taking a popular book (perhaps a more recent book) and turning it into an app could be profitable for both the Google Play and the App Store markets. The markets are already full of libraries, so we need to add some special features besides the raw version of the book. This might include daily quotes from the book, an audio version of the book, quizzes on the book, a forum where people can discuss the book, etc.
