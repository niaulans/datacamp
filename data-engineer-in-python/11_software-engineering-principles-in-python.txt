## [Software Engineering Principles in Python](https://app.datacamp.com/learn/courses/software-engineering-principles-in-python)


---------------------------------------------------------------------------
SOFTWARE ENGINEERING PRINCIPLES IN PYTHON                                 |
---------------------------------------------------------------------------
Software engineering concepts:
- Modularity
- Documenetation
- Testing
- Version control & git

Benefits of modularity:
- Improve readibility
- Improve maintainability
- Solve problem only once

Benefits of documentation:
- Show users how to use your project
- Prevent confusion from your collaborators
- Prevent frustation from future you

Benefits of automated testing:
- Save time over manual testing
- Find and fix more bugs
- Run tests anytime/anywhere

---------------------------------------------------------------------------
# load the Counter function into our environment
from collections import Counter

# View the documentation for Counter.most_common
help(Counter.most_common)

# use Counter to find the top 5 most common words
top_5_words = Counter(words).most_common(5)

# display the top 5 most common words
print(top_5_words)

---------------------------------------------------------------------------
Conventions and PEP 8

- Convention: a set of agreed-upon rules followed by a group of developers
- PEP 8: Style Guide for Python Code

pycode_style -> check code for PEP 8 compliance

# Import needed package
import pycodestyle

# Create a StyleGuide instance
style_checker = pycodestyle.StyleGuide()

# Run PEP 8 check on multiple files
result = style_checker.check_files(['nay_pep8.py', 'yay_pep8.py'])

# Print result of PEP 8 style check
print(result.messages)
---------------------------------------------------------------------------
# Import local packages
import MY_PACKAGE
import py_package

# View the help for each package
help(MY_PACKAGE)
help(py_package)


- Minimal required for make a package: __init__.py file

---------------------------------------------------------------------------
- working in work_dir/my_package/utils.py

def we_need_to_talk(break_up=False):
  """Helper for communicating with significant others"""
  if break_up:
    print("It's not you, it's me...")
  else:
    print("I <3 U!")

- working in work_dir/my_script.py

import my_package.utils as utils

utils.we_need_to_talk(break_up=True)

- working in work_dir/my_package/__init__.py
from .utils import we_need_to_talk

---------------------------------------------------------------------------
# Import needed functionality
from collections import Counter

def plot_counter(counter, n_most_common=5):
  # Subset the n_most_common items from the input counter
  top_items = counter.most_common(n_most_common)
  # Plot `top_items`
  plot_counter_most_common(top_items)

# Import needed functionality
from collections import Counter

def sum_counters(counters):
  # Sum the inputted counters
  return sum(counters, Counter())


from .counter_utils import plot_counter, sum_counters
---------------------------------------------------------------------------
# Import local package
import text_analyzer 

# Sum word_counts using sum_counters from text_analyzer
word_count_totals = text_analyzer.sum_counters(word_counts)

# Plot word_count_totals using plot_counter from text_analyzer
text_analyzer.plot_counter(word_count_totals)

---------------------------------------------------------------------------
requirements.txt

numpy==1.15.4
pandas==0.23.4
matplotlib==3.0.2

pip install -r requirements.txt

setup.py

from setuptools import setup

setup(name='my_package',
      version='0.0.1',
      description='An example package for Datacamp.',
      author='Adam Spannbauer',
      author_email='spannbaueradam@gmail.com',
      packages=['my_package'],
      install_requires=['numpy', 'pandas', 'matplotlib'])

python setup.py install
pip install .

---------------------------------------------------------------------------
# Specify where to istall requirements from
--index-url https://pypi.python.org/simple/

requirements = """
matplotlib>=3.0.0
numpy==1.15.4
pandas<=0.22.0
pycodestyle
"""

---------------------------------------------------------------------------
# Import needed function from setuptools
from setuptools import setup

# Create proper setup to be used by pip
setup(name='text_analyzer',
      version='0.0.1',
      description='Perform and visualize a text analysis.',
      author='Nia Ulan Sari',
      packages=['text_analyzer'])

---------------------------------------------------------------------------
# Import needed function from setuptools
from setuptools import setup

# Create proper setup to be used by pip
setup(name='text_analyzer',
      version='0.0.1',
      description='Perform and visualize a text analysis.',
      author='Nia Ulan Sari',
      packages=['text_analyzer'],
      install_requires=['matplotlib>=3.0.0'])

---------------------------------------------------------------------------
Class in python:

working in work_dir/my_package/my_class.py

class MyClass:
  """ A minimal example class

  """

  # Method to create a new instance of MyClass
  def __init__(self, value):
    #  Define attribute with the contents of value parameter
    self.attribute = value


working in work_dir/my_package/__init__.py

from .my_class import MyClass

my_instance = my_package.MyClass(value='class attribute value')
print(my_instance.attribute)

---------------------------------------------------------------------------
# Define Document class
class Document:
    """A class for text analysis
    
    :param text: string of text to be analyzed
    :ivar text: string of text to be analyzed; set by `text` parameter
    """
    # Method to create a new instance of Document
    def __init__(self, text):
        # Store text parameter to the text attribute
        self.text = text

# Import custom text_analyzer package
import text_analyzer

# Create an instance of Document with datacamp_tweet
my_document = text_analyzer.Document(text=datacamp_tweet)

# Print the text attribute of the Document instance
print(my_document.text)
---------------------------------------------------------------------------
from .token_utils import tokenize

class Document:
  def __init__(self, text, token_regex=r'[a-zA-Z+']):
    self.text = text
    self.tokens = self._tokenize() # Attribute to store tokens

  def _tokenize(self): # non-public method
    return tokenize(self.text)

doc = Document('test doc)
print(doc.tokens)

---------------------------------------------------------------------------
class Document:
  def __init__(self, text):
    self.text = text
    # pre tokenize the document with non-public tokenize method
    self.tokens = self._tokenize()
    # pre tokenize the document with non-public count_words
    self.word_counts = self._count_words()

  def _tokenize(self):
    return tokenize(self.text)
	
  # non-public method to tally document's word counts with Counter
  def _count_words(self):
    return Counter(self.tokens)

# create a new document instance from datacamp_tweets
datacamp_doc = Document(datacamp_tweets)

# print the first 5 tokens from datacamp_doc
print(datacamp_doc.tokens[:5])

# print the top 5 most used words in datacamp_doc
print(datacamp_doc.word_counts.most_common(5))
---------------------------------------------------------------------------
Class and the DRY principle

DRY -> Don't Repeat Yourself

Inheritance in Python

from .parent_class import ParentClass

#  Create a child class with inheritance
class ChildClass(ParentClass):
  def __init__(self):
    # Call parent's __init__ method
    ParentClass.__init__(self)
    self.child_attribute = "I'm a child class attribute!

child_class = ChildClass()
print(child_class.child_attribute)
print(child_class.parent_attribute)

---------------------------------------------------------------------------
# Parent Class
class Document:
    # Initialize a new Document instance
    def __init__(self, text):
        self.text = text
        # Pre tokenize the document with non-public tokenize method
        self.tokens = self._tokenize()
        # Pre tokenize the document with non-public count_words
        self.word_counts = self._count_words()

    def _tokenize(self):
        return tokenize(self.text)

    # Non-public method to tally document's word counts
    def _count_words(self):
        # Use collections.Counter to count the document's tokens
        return Counter(self.tokens)

# Child Class
# Define a SocialMedia class that is a child of the `Document class`
class SocialMedia(Document):
    def __init__(self, text):
        Document.__init__(self, text)
        self.hashtag_counts = self._count_hashtags()
        self.mention_counts = self._count_mentions()
        
    def _count_hashtags(self):
        # Filter attribute so only words starting with '#' remain
        return filter_word_counts(self.word_counts, first_char='#')      
    
    def _count_mentions(self):
        # Filter attribute so only words starting with '@' remain
        return filter_word_counts(self.word_counts, first_char='@')

# Import custom text_analyzer package
import text_analyzer

# Create a SocialMedia instance with datacamp_tweets
dc_tweets = text_analyzer.SocialMedia(text=datacamp_tweets)

# Print the top five most mentioned users
print(dc_tweets.mention_counts.most_common(5))

# Plot the most used hashtags
text_analyzer.plot_counter(dc_tweets.hashtag_counts)

---------------------------------------------------------------------------
Multilevel inheritance

class Parent:
  def __init__(self):
    print("I'm a parent")

class Child(Parent):
  def__init__(self):
    Parent.__init__(self)
    print("I'm a child!")

class SuperChild(Child):
  def __init__(self):
  super().__init__()
  print("I'm a super child!")

---------------------------------------------------------------------------
# Import needed package
import text_analyzer

# Create instance of document
my_doc = text_analyzer.Document(datacamp_tweets)

# Run help on my_doc's plot method
help(my_doc.plot_counts)

# Plot the word_counts of my_doc
my_doc.plot_counts()
---------------------------------------------------------------------------
# Define a Tweet class that inherits from SocialMedia
class Tweets(SocialMedia):
    def __init__(self, text):
        # Call parent's __init__ with super()
        super().__init__()
        # Define retweets attribute with non-public method
        self.retweets = self._process_retweets()

    def _process_retweets(self):
        # Filter tweet text to only include retweets
        retweet_text = filter_lines(self.text, first_chars='RT')
        # Return retweet_text as a SocialMedia object
        return SocialMedia(retweet_text)

---------------------------------------------------------------------------
# Import needed package
import text_analyzer

# Create instance of Tweets
my_tweets = text_analyzer.Tweets(datacamp_tweets)

# Import needed package
import text_analyzer

# Create instance of Tweets
my_tweets = text_analyzer.Tweets(datacamp_tweets)

# Plot the most used hashtags in the tweets
my_tweets.plot_counts('hashtag_counts')

# Import needed package
import text_analyzer

# Create instance of Tweets
my_tweets = text_analyzer.Tweets(datacamp_tweets)

# Plot the most used hashtags in the retweets
my_tweets.retweets.plot_counts('hashtag_counts')


---------------------------------------------------------------------------
inheritance (pewarisan), polymorphism, encapsulation, dan abstraction.

- Encapsulation adalah konsep di mana atribut atau metode dalam class dibatasi aksesnya. Ini dapat dilakukan dengan menentukan tingkat akses:
Public (self.atribut) → Bisa diakses dari mana saja.
Protected (self._atribut) → Bisa diakses dari dalam class dan turunannya.
Private (self.__atribut) → Hanya bisa diakses dari dalam class itu sendiri.
class Bank:
    def __init__(self, saldo):
        self.__saldo = saldo  # Private attribute

    def cek_saldo(self):
        print(f"Saldo: {self.__saldo}")

    def tambah_saldo(self, jumlah):
        self.__saldo += jumlah
        print(f"Saldo bertambah menjadi: {self.__saldo}")

# Membuat objek
akun = Bank(1000000)
akun.cek_saldo()  # Output: Saldo: 1000000

# akun.__saldo = 5000  # Ini akan error karena atribut bersifat private

- Polymorphism memungkinkan penggunaan metode yang sama dengan implementasi yang berbeda di class yang berbeda.
class Burung:
    def bersuara(self):
        print("Cuit cuit")

class Ayam:
    def bersuara(self):
        print("Kukuruyuk")

# Contoh Polymorphism
def buat_suara(hewan):
    hewan.bersuara()

burung1 = Burung()
ayam1 = Ayam()

buat_suara(burung1)  # Output: Cuit cuit
buat_suara(ayam1)    # Output: Kukuruyuk

- Abstraksi digunakan untuk menyembunyikan detail implementasi dan hanya menampilkan fungsi yang relevan. Di Python, kita bisa menggunakan modul ABC (Abstract Base Class) untuk membuat kelas abstrak.
from abc import ABC, abstractmethod

class Kendaraan(ABC):
    @abstractmethod
    def bergerak(self):
        pass

class Mobil(Kendaraan):
    def bergerak(self):
        print("Mobil berjalan di jalan.")

class Pesawat(Kendaraan):
    def bergerak(self):
        print("Pesawat terbang di udara.")

mobil = Mobil()
pesawat = Pesawat()

mobil.bergerak()   # Output: Mobil berjalan di jalan.
pesawat.bergerak() # Output: Pesawat terbang di udara.

---------------------------------------------------------------------------
Konsep utama dalam OOP adalah:

Class dan Object → Blueprint dan instansiasi objek.
Inheritance → Pewarisan atribut dan metode.
Encapsulation → Mengatur akses data.
Polymorphism → Metode dengan nama sama tetapi perilaku berbeda.
Abstraction → Menyembunyikan detail implementasi.
---------------------------------------------------------------------------
Maintainability

Documentation in python:
- Comments -> #
- Docstrings -> """ """

Docstrings

def function(x):
  """High level description of function

  Additional details on function 

  :param x: description of parameter x
  :return: description of return value

  >>> # Example function usage 
  Expected output of example function usage
  """

# Example
def square(x):
  """Square the number xlabel

  :param x: number to square 
  :return: x squared

  >>> square(2)
  4

  # 'x * x' is faster than 'x ** 2'
  # reference: https://stackoverflow.com/a/2596215
  """
  return x * x

---------------------------------------------------------------------------
# Complete the function's docstring
def tokenize(text, regex=r'[a-zA-z]+'):
  """Split text into tokens using a regular expression

  :param text: text to be tokenized
  :param regex: regular expression used to match tokens using re.findall 
  :return: a list of resulting tokens

  >>> tokenize('the rain in spain')
  ['the', 'rain', 'in', 'spain']
  """
  return re.findall(regex, text, flags=re.IGNORECASE)

# Print the docstring
help(tokenize)

---------------------------------------------------------------------------
def hypotenuse_length(leg_a, leg_b):
    """Find the length of a right triangle's hypotenuse

    :param leg_a: length of one leg of triangle
    :param leg_b: length of other leg of triangle
    :return: length of hypotenuse
    
    >>> hypotenuse_length(3, 4)
    5
    """
    return math.sqrt(leg_a**2 + leg_b**2)


# Print the length of the hypotenuse with legs 6 & 8
print(hypotenuse_length(6, 8))
---------------------------------------------------------------------------
def polygon_perimeter(n_sides, side_len):
    return n_sides * side_len


def polygon_apothem(n_sides, side_len):
    denominator = 2 * math.tan(math.pi / n_sides)
    return side_len / denominator

def polygon_area(n_sides, side_len):
    perimeter = polygon_perimeter(n_sides, side_len)
    apothem = polygon_apothem(n_sides, side_len)

    return perimeter * apothem / 2

# Print the area of a hexagon with legs of size 10
print(polygon_area(n_sides=6, side_len=10))

---------------------------------------------------------------------------
Unit testing

Testing in python:
- doctest
- pytest


- doctest
def square(x):
  """Square the number xlabel

  :param x: number to square 
  :return: x squared

  >>> square(2)
  4

  # 'x * x' is faster than 'x ** 2'
  # reference: https://stackoverflow.com/a/2596215
  """
  return x * x

import doctest
doctest.testmod()

---------------------------------------------------------------------------
work in workdir/tests/test_document.py

from text_analyzer import Document

# Test tokens attribue on Document object
def test_document_tokens():
  doc = Document('a e i o u')
  assert doc.tokens == ['a', 'e', 'i', 'o', 'u']

# Test edge case of black document
def test_document_empty():
  doc = Document('')

  assert doc.tokens == []
  assert doc.word_counts == Counter()

# Create 2 identical Document objects
doc_a = Document('a e i o u')
doc_b = Document('a e i o u')

# Check if objects are equal
print(doc_a == doc_b)

---------------------------------------------------------------------------
def sum_counters(counters):
    """Aggregate collections.Counter objects by summing counts

    :param counters: list/tuple of counters to sum
    :return: aggregated counters with counts summed

    >>> d1 = text_analyzer.Document('1 2 fizz 4 buzz fizz 7 8')
    >>> d2 = text_analyzer.Document('fizz buzz 11 fizz 13 14')
    >>> sum_counters([d1.word_counts, d2.word_counts])
    Counter({'fizz': 4, 'buzz': 2})
    """
    return sum(counters, Counter())

doctest.testmod()

---------------------------------------------------------------------------
test_social_media.py

from collections import Counter
from text_analyzer import SocialMedia

# Create an instance of SocialMedia for testing
test_post = 'learning #python & #rstats is awesome! thanks @datacamp!'
sm_post = SocialMedia(test_post)

# Test hashtag counts are created properly
def test_social_media_hashtags():
    expected_hashtag_counts = Counter({'#python': 1, '#rstats': 1})
    assert sm_post.hashtag_counts == expected_hashtag_counts
---------------------------------------------------------------------------
Sphinx -> documentation generator
Travis Ci -> continuous integration and test code
Git and GitHub -> version control and collaboration
Codecov -> code coverage
Code climate -> code quality
---------------------------------------------------------------------------
from text_analyzer import Document

class SocialMedia(Document):
    """Analyze text data from social media
    
    :param text: social media text to analyze

    :ivar hashtag_counts: Counter object containing counts of hashtags used in text
    :ivar mention_counts: Counter object containing counts of @mentions used in text
    """
    def __init__(self, text):
        Document.__init__(self, text)
        self.hashtag_counts = self._count_hashtags()
        self.mention_counts = self._count_mentions()