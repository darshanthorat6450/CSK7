Practical1

import pandas as pd

import numpy as np

df = pd.read\_csv("titanic.csv")

df\['Age'].fillna(df\['Age'].mean(), inplace=True)//-------modify original dataframe directly(inplace=true)

df\['Embarked'].fillna(df\['Embarked'].mode()\[0], inplace=True)

df.drop('Cabin', axis=1, inplace=True)

print(df.describe()) //Prints statistical summary of all numeric columns — count, mean, min, max, standard deviation, etc.

print(df.info())//print metadata

df\['Survived'] = df\['Survived'].astype('category')

df = pd.get\_dummies(df, columns=\['Sex', 'Embarked']) //Applies **One-Hot Encoding** to the Sex and Embarked columns:

print(df.head())











Practical2



import numpy as np

import matplotlib.pyplot as plt //Matplotlib — the base plotting library. Controls figure size, subplots, titles, and rendering graphs.

import seaborn as sns //Seaborn — built on top of Matplotlib. Makes beautiful statistical charts like histograms with just one line.



df = pd.DataFrame({

&#x20;   'Name': \['A', 'B', 'C', 'D', 'E'],

&#x20;   'Marks': \[85, 90, None, 40, 300],

&#x20;   'Attendance': \[90, None, 80, 70, 95]

})





df\['Marks'] = df\['Marks'].fillna(df\['Marks'].mean())

df\['Attendance'] = df\['Attendance'].fillna(df\['Attendance'].median())



Q1 = df\['Marks'].quantile(0.25)

Q3 = df\['Marks'].quantile(0.75)

IQR = Q3 - Q1                    ///IQR (Interquartile Range) = the middle 50% spread of data

lower = Q1 - 1.5 \* IQR

upper = Q3 + 1.5 \* IQR

outliers = df\[(df\['Marks'] < lower) | (df\['Marks'] > upper)]

print("Outliers:\\n", outliers)



df\['Marks\_norm'] = (df\['Marks'] - df\['Marks'].min()) / (df\['Marks'].max() - df\['Marks'].min())

df\['Marks\_log'] = np.log(df\['Marks'])

print(df)



\# --- Graph Representation ---

plt.figure(figsize=(10, 4)) //Creates a blank canvas of size 10 inches wide × 4 inches tall for the plots.



plt.subplot(1, 2, 1)

sns.histplot(df\['Marks'], kde=True)

plt.title("Original Marks Distribution")



plt.subplot(1, 2, 2)

sns.histplot(df\['Marks\_log'], kde=True, color='orange')

plt.title("Log-Transformed Marks Distribution")



plt.tight\_layout()

plt.show()











Practical3

import pandas as pd

df = pd.read\_csv("iris.csv")

print(df.mean(numeric\_only=True))

print(df.median(numeric\_only=True))

print(df.min(numeric\_only=True))

print(df.max(numeric\_only=True))

print(df.var(numeric\_only=True))

print(df.std(numeric\_only=True))

print(df.groupby('species').mean(numeric\_only=True))















Practical4

import pandas as pd

from sklearn.model\_selection import train\_test\_split

from sklearn.linear\_model import LinearRegression

from sklearn.metrics import mean\_squared\_error, r2\_score

df = pd.read\_csv("housing.csv")

X = df.drop('medv', axis=1)

y = df\['medv']

X\_train, X\_test, y\_train, y\_test = train\_test\_split(X, y, test\_size=0.2)

model = LinearRegression()

model.fit(X\_train, y\_train)

y\_pred = model.predict(X\_test)

print("MSE:", mean\_squared\_error(y\_test, y\_pred))

print("R2 Score:", r2\_score(y\_test, y\_pred))













Practical5

import pandas as pd

from sklearn.model\_selection import train\_test\_split

from sklearn.linear\_model import LogisticRegression

from sklearn.metrics import confusion\_matrix, accuracy\_score, precision\_score, recall\_score

df = pd.read\_csv("Social\_Network\_Ads.csv")

X = df\[\['Age', 'EstimatedSalary']]

y = df\['Purchased']

X\_train, X\_test, y\_train, y\_test = train\_test\_split(X, y, test\_size=0.2)

model = LogisticRegression()

model.fit(X\_train, y\_train)

y\_pred = model.predict(X\_test)

print(confusion\_matrix(y\_test, y\_pred))

print("Accuracy:", accuracy\_score(y\_test, y\_pred))

print("Error Rate:", 1 - accuracy\_score(y\_test, y\_pred))

print("Precision:", precision\_score(y\_test, y\_pred))

print("Recall:", recall\_score(y\_test, y\_pred))



















Practical6

import pandas as pd

from sklearn.model\_selection import train\_test\_split

from sklearn.naive\_bayes import GaussianNB

from sklearn.metrics import confusion\_matrix, accuracy\_score, precision\_score, recall\_score

df = pd.read\_csv("iris.csv")

X = df.drop('species', axis=1)

y = df\['species']

X\_train, X\_test, y\_train, y\_test = train\_test\_split(X, y, test\_size=0.2)

model = GaussianNB()

model.fit(X\_train, y\_train)

y\_pred = model.predict(X\_test)

print(confusion\_matrix(y\_test, y\_pred))

print("Accuracy:", accuracy\_score(y\_test, y\_pred))

print("Error Rate:", 1 - accuracy\_score(y\_test, y\_pred))

print("Precision:", precision\_score(y\_test, y\_pred, average='macro'))

print("Recall:", recall\_score(y\_test, y\_pred, average='macro'))





















Practical7

import nltk

from nltk.tokenize import word\_tokenize

from nltk.corpus import stopwords

from nltk.stem import PorterStemmer, WordNetLemmatizer

from sklearn.feature\_extraction.text import TfidfVectorizer

\# Download once (comment after first run)

\# nltk.download('punkt')

\# nltk.download('stopwords')

\# nltk.download('wordnet')



text = "Data science is amazing and powerful"

tokens = word\_tokenize(text)

stop\_words = set(stopwords.words('english'))

filtered = \[w for w in tokens if w.lower() not in stop\_words]

ps = PorterStemmer()

stemmed = \[ps.stem(w) for w in filtered]

lm = WordNetLemmatizer()

lemmatized = \[lm.lemmatize(w) for w in filtered]

vectorizer = TfidfVectorizer()

tfidf = vectorizer.fit\_transform(\[text])

print(tokens)

print(filtered)

print(stemmed)

print(lemmatized)

print(tfidf.toarray())

















Practical8

import pandas as pd

import seaborn as sns

import matplotlib.pyplot as plt

df = pd.read\_csv("titanic.csv")

sns.histplot(df\['Fare'], kde=True)

plt.title("Fare Distribution")

plt.xlabel("Fare")

plt.ylabel("Frequency")

plt.show()

print("Skewness:", df\['Fare'].skew())

















Practical9

import pandas as pd

import seaborn as sns

import matplotlib.pyplot as plt

df = pd.read\_csv("titanic.csv")

sns.boxplot(x='sex', y='age', hue='survived', data=df)

plt.title("Age vs Gender vs Survival")

plt.xlabel("Gender")

plt.ylabel("Age")

plt.show()













Practical10

import pandas as pd

import seaborn as sns

import matplotlib.pyplot as plt

df = pd.read\_csv("iris.csv")

df.hist()

plt.show()

sns.boxplot(data=df)

plt.show()

