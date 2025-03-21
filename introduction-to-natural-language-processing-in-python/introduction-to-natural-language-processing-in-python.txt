## [Introduction to Natural Language Processing in Python](https://app.datacamp.com/learn/courses/introduction-to-natural-language-processing-in-python)

# %% [markdown]
# ## 1. Regular expressions & word tokenization

# %% [markdown]
# ### Introduction to regular expressions

# %% [markdown]
# **What is Natural Language Processing?**
# - Filed of study on making sense of language 
# - Using statistics and computers
# - Learn the basics of NLP
# - Topic indentification
# - Text classification
# - NLP applications: 
#     - Chatbots
#     - Translation
#     - Sentiment analysis
#     - etc
# 
# ---
# **What exactly are regular expressions/ regex?**
# - Strings with a special syntax
# - Allow us to match patterns in other strings
# - Applications of regular expressions: 
# - Find all web links in a document
# - Parse email addresses
# - Remove/replace unwanted characters
# 
# ---
# **Common regex patterns**
# pattern | matches | example
# :-: | :-: | :-: 
# \\w+ | word | 'Magic'
# \\d | digit | 9
# \\s | space | ''
# .* | wildcard | 'username78' match semua letter dan symbol
# tanda + or * | greedy match | 'aaa'
# \\S | not space | 'no_space'
# [a-z] | lowercase group | 'abcdefg'
# 
# ---
# **Python's re module**
# - re module
# - split : split a string on regex
# - findall : find all patterns in a string
# - search : search for a pattern
# - match : match an entire string or substring based on a pattern
# - Pattern first and the string second
# - May return an iterator, string, or match object

# %%
import re
re.match('abc', 'abcdef')

# %%
word_regex = '\w+' # Match kata pertama yang ditemukan
re.match(word_regex, 'hi there!')

# %%
re.split('\s+', 'Split on spaces')

# %%
my_string = "Let's write Regex!"
PATTERN = r"\w+"
re.findall(PATTERN, my_string)

# %% [markdown]
# #### re.split() and re.findall()

# %%
import re

my_string = "Let's write RegEx!  Won't that be fun?  I sure think so.  Can you find 4 sentences?  Or perhaps, all 19 words?"
sentence_endings = r"[.?!]"
print("Split by .?!: \n" , re.split(sentence_endings, my_string))

capitalized_words = r"[A-Z]\w+"
print("Find all capitalized words: \n" , re.findall(capitalized_words, my_string))

spaces = r"\s+"
print("Split by spaces: \n" , re.split(spaces, my_string))

digits = r"\d+"
print("Find digits: \n" , re.findall(digits, my_string))


# %% [markdown]
# ### Introduction to tokenization

# %% [markdown]
# **What is tokenization?**
# - Turning a string or document into tokens (smaller chunks)
# - One step in preparing a text for nlp
# - Many different theories and rules 
# - can create your own rules using regular expressions
# - Some examples: 
#     - Breaking out words or sentences
#     - Separating punctuation
#     - Separating all hashtag in a tweet
# 
# ---
# **nltk library**
# - nltk: naural language toolkit
# 
# --- 
# **Why tokenize?** 
# - Easier to map part of speech
# - Matching common words
# - Removing unwanted tokens
# 
# ---
# **Other nltk tokenizers**
# - sent_tokenize: tokenize a document into sentences
# - regexp_tokenize: tokenize a string or document based on a regex pattern
# - TweetTokenizer: special class just for tweet tokenization, separate hashtags, mentions and lots of exclamation points
# 
# ---
# **More regex practice** 
# - Difference between re.search() and re.match()
# - match mencoba mencocokkan string sampai tidak cocok lagi
# - search akan lihat seluruh string untuk mencocokkan

# %%
from nltk.tokenize import word_tokenize
word_tokenize("Hi there!")

# %%
file = r'D:\Pemrograman\Data-Science\Datacamp\Code\Natural Language Processing in Python\Introduction to NLP in Python\scene1.txt'
with open(file, 'r') as f:
    text = f.read()
    print(text)

# %%
from nltk.tokenize import sent_tokenize, word_tokenize

sentences = sent_tokenize(text)
tokenized_sent = word_tokenize(sentences[3])
unique_tokens = set(word_tokenize(text))
print(unique_tokens)

# %%
# Search for the first occurencne of "accounts" in scene_one
match = re.search("coconuts", text)
print(match.start(), match.end())

# %%
pattern1 = r"\[.*\]"
print(re.search(pattern1, text))

# %%
pattern2 = r"[\w\s]+:"
print(re.match(pattern2, sentences[3]))

# %% [markdown]
# ### Advances tokenization with NLTK and regex

# %% [markdown]
# **Regex groups using or "|"**
# - OR is represented using |
# - can define a group using ()
# - can define explicit character range using []

# %%
import re

match_digits_and_words = ('(\d+|\w+)')
re.findall(match_digits_and_words, "Nia Ulan Sari 10.")


# %% [markdown]
# **Regex ranges and groups**
# 
# pattern | matches | example
# :-| :-| :-
# [A-Za-z]+ | upper and lowercase English alphabet | 'ABCDEFghijk'|
# [0-9] | numbers from 0 to 9 | 9
# [A-Za-z\\-\\.]+ | upper and lowercase English alphabet, - and . | 'My-website.com'
# (a-z) | a, - and z | 'a-z'
# (\s+ or ,) | spaces or a comma | ','

# %%
import re

my_str = 'match lowercase spaces nums like 12, but no commas'
re.match('[a-z0-9 ]+', my_str)

# %%
from nltk.tokenize import regexp_tokenize

my_string = "SOLDIER #1: Found them? In Mercea? The coconut's tropical!"
pattern = r"(\w+|#\d|\?\!)"
x = regexp_tokenize(my_string, pattern)
x

# %%
from nltk.tokenize import word_tokenize, regexp_tokenize, TweetTokenizer

text = ['This is the best #nlp exercise ive found online! #python', '#NLP is super fun! <3 #learning', 'Thanks @datacamp :) #nlp #python']

pattern1 = r"#\w+"
# pattern2 = r"(#\w+|@\w+)"
pattern2 = r"([@#]\w+)"
hashtags = regexp_tokenize(text[0], pattern1)
mention_hashtags = regexp_tokenize(text[-1], pattern2)
print(hashtags)
print(mention_hashtags)

# %%
tknzr = TweetTokenizer()
all_tokens = [tknzr.tokenize(t) for t in text]
print(all_tokens)

# %%
text = 'Wann gehen wir Pizza essen? 🍕 Und fährst du mit Über? 🚕'

# %%
# Tokenize and print all words in german_text
all_words = word_tokenize(text)
print(all_words)

# Tokenize and print only capital words
capital_words = r"[A-ZÜ]\w+"
print(regexp_tokenize(text, capital_words))

# Tokenize and print only emoji
emoji = "['\U0001F300-\U0001F5FF'|'\U0001F600-\U0001F64F'|'\U0001F680-\U0001F6FF'|'\u2600-\u26FF\u2700-\u27BF']"
print(regexp_tokenize(text, emoji))

# %% [markdown]
# ### Charting word length with NLTK

# %% [markdown]
# **Getting started with matplotlib**
# - Charting library used by many open source Python projects
# - Straightforward functionality with lots of options
#     - Histograms
#     - Bar charts
#     - Line charts
#     - Scatter plots
# - advanced functionality like 3D graphs and animations

# %%
from matplotlib import pyplot as plt

plt.hist([1, 5, 5, 7, 7, 9])
plt.show()

# %% [markdown]
# #### Combining NLP data extraction with plotting

# %%
from matplotlib import pyplot as plt
from nltk.tokenize import word_tokenize

words = word_tokenize("This is pretty cool tool!")
words_lengths = [len(w) for w in words]
plt.hist(words_lengths)

# %%
file = r"D:\Pemrograman\Data-Science\Datacamp\Code\Natural Language Processing in Python\Introduction to NLP in Python\grail.txt"
with open(file, 'r') as f:
    text = f.read()
    print(text)

# %%
import re

# Split the script into lines: lines
lines = text.split('\n')

# Replace all script lines for speaker
pattern = "[A-Z]{2,}(\s)?(#\d)?(A-Z]{2,})?:"
lines = [re.sub(pattern, '', l) for l in lines]

# Tokenize each line: tokenized_lines
tokenized_lines = [regexp_tokenize(s, "\w+") for s in lines]

# Make a frequency list of lengths: line_num_words
line_num_words = [len(t_line) for t_line in tokenized_lines]

# Plot a histogram of the line lengths
plt.hist(line_num_words)

# Show the plot
plt.show()

# %% [markdown]
# ## 2. Simple topic identification

# %% [markdown]
# ### Word counts with bag-of-words

# %% [markdown]
# **Bag of words**
# - Basics method for finding topics in a text
# - Need to first create tokens using tokenization
# - .. count up all the tokens
# - The more frequent a word, the more important it might be
# - Can be a great way to determine the significant words in a text
# 
# **Bad-of-words example**
# - Text : "The cat is in the bos. The cat likes the box. The box is over the cat."
# - Bag of words (strip punct):
#     - "The": 3, "box":3
#     - "cat":3, "the":3
#     - "is":2
#     - "in":1, "likes":1, "over":1

# %%
from nltk.tokenize import word_tokenize
from collections import Counter

Counter(word_tokenize("The cat is in the box. The cat box."))

# %%
counter.most_common(2)

# %%
from collections import Counter

file = r'News articles\articles.txt'
with open(file, 'r', encoding="utf-8") as f:
    text = f.read()
    # print(text)

tokens = word_tokenize(text)
lower_tokens = [t.lower() for t in tokens]
bow_simple = Counter(lower_tokens)
bow_simple.most_common(10)

# %% [markdown]
# ### Simple text preprocessing

# %% [markdown]
# **Why preprocess?**
# - Helps make for better input data
#     - When performing ml for other statistical methods
# - Examples: 
#     - Tokenization to create a bag of words
#     - Lowercasing words
# - Lemmatization/stemming
#     - Shorten words to their root stems
# - Removing stopwords, punctuation, or unwanted tokens
# - Good to experiment with different approaches
# 
# **Preprocessing example**
# - Imput text: Cats, dogs, and birds are common pets. So are fish.
# - Output tokens: cat, dog, bird, common, pet, fish

# %%
from nltk.corpus import stopwords
text = """The cat is in the box. The cat likes the box. 
The box is over the cat."""

tokens = [w for w in word_tokenize(text.lower()) if w.isalpha()]
no_stops = [t for t in tokens if t not in stopwords.words('english')]
no_stops
Counter(no_stops).most_common(2)

# %%
import nltk
nltk.download('omw-1.4')

# %%
from nltk.stem import WordNetLemmatizer
from collections import Counter

file = r'News articles\articles.txt'
with open(file, 'r', encoding="utf-8") as f:
    text = f.read()
    # print(text)

tokens = word_tokenize(text)
lower_tokens = [t.lower() for t in tokens]
alpha_only = [t for t in lower_tokens if t.isalpha()]

no_stops = [t for t in alpha_only if t not in stopwords.words('english')]
wordnet_lemmatizer = WordNetLemmatizer()
lemmatized = [wordnet_lemmatizer.lemmatize(t) for t in no_stops]
bow = Counter(lemmatized)
print(bow.most_common(10))

# %% [markdown]
# ### Introduction to gensim

# %% [markdown]
# **What is gensim?**
# - Popular open -source NLP library
# - Uses top academic models to perform complex tasks
#     - Building document or word vectors
#     - Performing topic identification and document comparison
# - A word embedding or vector is trained from a larger corpus and is a multi-dimensional representation of a word or document
# - LDA (latent dirichlet allocation) is a statistical model we can apply to text using Gensim for topic analysis and modelling
# - Gensim allows you to buils corpora and dictionaries using simple classes and functions
# - Gensim models can be easily saved, updated, and reused
# - Our dictionary can also be updated

# %%
from gensim.corpora.dictionary import Dictionary
from nltk.tokenize import word_tokenize

my_documents = ['The movie was about a spaceship and aliends.',
                'I really liked the movie!',
                'Awesome action scenes, but boring characters.',
                'The movie was awful! I hate alien films',
                'Space is cool! I liked the movie.',
                'More space films, please!']

# %%
tokenized_docs = [word_tokenize(doc.lower()) for doc in my_documents]
dictionary = Dictionary(tokenized_docs)
dictionary.token2id
corpus = [dictionary.doc2bow(doc) for doc in tokenized_docs]
corpus 

# %%
from gensim.corpora.dictionary import Dictionary

file = r'Wikipedia articles/wiki_text_computer.txt'
with open(file, 'r', encoding="utf-8") as f:
    text = f.read()
    # print(text)

tokens = word_tokenize(text.lower())
no_stops = [t for t in tokens if t not in stopwords.words('english')]
no_punch = [t for t in no_stops if t.isalpha()]

# Create a Dictionary from the articles: dictionary
dictionary = Dictionary(no_punch)

# Select the id for "computer": computer_id
computer_id = dictionary.token2id.get("computer")

# Use computer_id with the dictionary to print the word
print(dictionary.get(computer_id))

# Create a MmCorpus: corpus
corpus = [dictionary.doc2bow(article) for article in articles]

# Print the first 10 word ids with their frequency counts from the fifth document
print(corpus[4][:10])

# %%
# Save the fifth document: doc
doc = corpus[4]

# Sort the doc for frequency: bow_doc
bow_doc = sorted(doc, key=lambda w: w[1], reverse=True)

# Print the top 5 words of the document alongside the count
for word_id, word_count in bow_doc[:5]:
    print(dictionary.get(word_id), word_count)
    
# Create the defaultdict: total_word_count
total_word_count = defaultdict(int)
for word_id, word_count in itertools.chain.from_iterable(corpus):
    total_word_count[word_id] += word_count
    
# Create a sorted list from the defaultdict: sorted_word_count
sorted_word_count = sorted(total_word_count.items(), key=lambda w: w[1], reverse=True) 

# Print the top 5 words across all documents alongside the count
for word_id, word_count in sorted_word_count[:5]:
    print(dictionary.get(word_id), word_count)

# %% [markdown]
# ### Tf-idf with gensim

# %% [markdown]
# **What is tf-idf?**
# - Term frequency - inverse document frequency
# - Allows you to determine the most important words in each document
# - Each corpus may have shared words beyond just stopwords
# - These words should be down-weighted in importance
# - Example from astronomy: "Sky"
# - Keeps document spesific frequent words weighted high

# %%
from gensim.models.tfidfmodel import TfidfModel
tfidf = TfidfModel(corpus)
tfidf[corpus[0]]

# %%
from gensim.models.tfidfmodel import TfidfModel

# Create a new TfidfModel using the corpus: tfidf
tfidf = TfidfModel(corpus)
doc = corpus[4]

# Calculate the tfidf weights of doc: tfidf_weights
tfidf_weights = tfidf[doc]

# Print the first five weights
print(tfidf_weights[:5])

# Sort the weights from highest to lowest: sorted_tfidf_weights
sorted_tfidf_weights = sorted(tfidf_weights, key=lambda w: w[1], reverse=True)

# Print the top 5 weighted words
for term_id, weight in sorted_tfidf_weights[:5]:
    print(dictionary.get(term_id), weight)


# %% [markdown]
# ## Named entity recognition

# %% [markdown]
# **What is named entity recognition?**
# - NLP tasks to identify important names entities in the text
# - Examples: 
#     - People names
#     - Places
#     - Organizations
#     - Dates
#     - States
#     - Works of art
#     - etc
# - Can be used alongside topic identification
# - Who? What? Where? When? Why?
# 
# **nltk and the Stanford CoreNLp Library**
# - The Stanford CoreNLP library: 
#     - Integrated into Python via nltk
#     - Java based 
#     - Support for NER as well as coreference and dependency parsing

# %%
import nltk

sentence = '''In New York, I loke to ride the Metro to visit MOMA
            and some restaurants rated well by Ruth Reichl.'''
tokenized_sent = nltk.word_tokenize(sentence)
tagged_sent = nltk.pos_tag(tokenized_sent)
tagged_sent[:3]

# %%
nltk.ne_chunk(tagged_sent)

# %%
from nltk.tokenize import sent_tokenize, word_tokenize

with open('News articles/uber_apple.txt', 'r', encoding="utf-8") as f:
    text = f.read()
    # print(text)

# %%
sentences = sent_tokenize(text)
token_sentences = [word_tokenize(sentence) for sentence in sentences]
pos_sentences = [nltk.pos_tag(sentence) for sentence in token_sentences]
chunked_sentences = nltk.ne_chunk_sents(pos_sentences, binary=True)

for sent in chunked_sentences:
    for chunck in sent:
        if hasattr(chunk, "label") and chunck.label() == "NE":
            print(chunk)

# %%
# Create the defaultdict: ner_categories
ner_categories = defaultdict(int)

# Create the nested for loop
for sent in chunked_sentences:
    for chunk in sent:
        if hasattr(chunk, 'label'):
            ner_categories[chunk.label()] += 1
            
# Create a list from the dictionary keys for the chart labels: labels
labels = list(ner_categories.keys())

# Create a list of the values: values
values = [ner_categories.get(v) for v in labels]

# Create the pie chart
plt.pie(values, labels=labels, autopct='%1.1f%%', startangle=140)

# Display the chart
plt.show()

# %% [markdown]
# ### Introduction to SpaCy

# %% [markdown]
# **What is SpaCy?**
# - NLP library similar to genim, with different implementations
# - Focus on creating NLP pipelines to generate models and corpora
# - Open-source, with extra libraries and tools
#     - Displacy -> Displacy entity recognition visualizer
#     

# %%
import spacy

nlp = spacy.load('en_core_web_sm')

# %%
doc = nlp("""Berlin is the capital of Germany;
            and the residence of Chancellor Angela Merkel.""")
doc.ents

# %%
for ent in doc.ents:
    print(ent.text, ent.label_)

# %%
print(doc.ents[0], doc.ents[0].label_)

# %% [markdown]
# **Why use SpaCy for NER**
# - Easy pipeline creation
# - Different entity type compared to nltk
# - Informal language corpora
#     - Easily find entities in Tweets and chat messages
# - Quickly growing!

# %%
with open('News articles/uber_apple.txt', 'r', encoding="utf-8") as f:
    text = f.read()
    # print(text)

# Import spacy
import spacy

# Instantiate the English model: nlp
nlp = spacy.load('en_core_web_sm', disable=['parser', 'tagger', 'matcher'])

# Create a new document: doc
doc = nlp(text)

# Print all of the found entities and their labels
for ent in doc.ents:
    print(ent.text, ent.label_)


# %% [markdown]
# ### Multilingual NER with polyglot

# %% [markdown]
# **What is polyglot?**
# - 


