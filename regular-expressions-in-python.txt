## [Regular Expressions in Python](https://app.datacamp.com/learn/courses/regular-expressions-in-python)

# Length
len(str)

# Convert to str
str()

# Concatenate
str1 + str2

# Indexing and Slicing
str[0]
str[0:3] 0-> inclusive 3-> exclusive
str[0:3:2] 2-> step

------------------------------------------------------------------
# Find characters in movie variable
length_string = len(movie)

# Convert to string
to_string = str(length_string)

# Predefined variable
statement = "Number of characters in this review:"

# Concatenate strings and print result
print(statement + " " + to_string)

------------------------------------------------------------------
# Select the first 32 characters of movie1
first_part = movie1[:32]

# Select from 43rd character to the end of movie1
last_part = movie1[42:]

# Select from 33rd to the 42nd character
middle_part = movie2[32:42]

# Print concatenation and movie2 variable
print(first_part+middle_part+last_part) 
print(movie2)

------------------------------------------------------------------
# Get the word
movie_title = movie[11:30]

# Obtain the palindrome
palindrome = movie_title[::-1]

# Print the word if it's a palindrome
if movie_title == palindrome:
	print(movie_title)

------------------------------------------------------------------
# Convert to lowercase
str.lower()

# Convert to uppercase
str.upper()

# Convert to capitalize
str.capitalize()

# Split into a list of substrings
str.split(sep=' ', maxsplit=2)

# Split from the right
str.rsplit(sep=' ', maxsplit=2)

# Breaking at line boundaries
str.splitlines()

# Joining strings
str.join(iterable)
print(" ".join(movie))

# Strip leading and trailing white spaces
str.strip()

# Remove character from the right
str.rstrip()

# Remove character from the left
str.lstrip()

------------------------------------------------------------------
# Convert to lowercase and print the result
movie_lower = movie.lower()
print(movie_lower)

# Remove specified character and print the result
movie_no_sign = movie_lower.strip("$")
print(movie_no_sign)

# Split the string into substrings and print the result
movie_split = movie_no_sign.split()
print(movie_split)

# Select root word and print the result
word_root = movie_split[1][:-1]
print(word_root)

------------------------------------------------------------------
# Remove tags happening at the end and print results
movie_tag = movie.rstrip("<\i>")
print(movie_tag)

# Split the string using commas and print results
movie_no_comma = movie_tag.split(",")
print(movie_no_comma)

# Join back together and print results
movie_join = " ".join(movie_no_comma)
print(movie_join)

------------------------------------------------------------------
# Split string at line boundaries
file_split = file.splitlines()

# Print file_split
print(file_split)

# Complete for-loop to split by commas
for substring in file_split:
    substring_split = substring.split(",")
    print(substring_split)

------------------------------------------------------------------
# Finding a substring
string.find(substring, start, end)

# Find using index
string.index(substring, start, end)

# Counting occurrences
string.count(substring, start, end)

# Replace a substring
string.replace(old, new, count)

------------------------------------------------------------------
for movie in movies:
  	# If actor is not found between character 37 and 41 inclusive
    # Print word not found
    if movie.find("actor", 37, 42) == -1:
        print("Word not found")
    # Count occurrences and replace two with one
    elif movie.count("actor") == 2:  
        print(movie.replace("actor actor", "actor"))
    else:
        # Replace three occurrences with one
        print(movie.replace("actor actor actor", "actor"))

------------------------------------------------------------------
for movie in movies:
  # Find the first occurrence of word
  print(movie.find("money", 12, 51))

for movie in movies:
  try:
    # Find the first occurrence of word
  	print(movie.index("money", 12, 51))
  except ValueError:
    print("substring not found")

------------------------------------------------------------------
# Replace negations 
movies_no_negation = movies.replace("isn't", "is")

# Replace important
movies_antonym = movies_no_negation.replace("important", "insignificant")

# Print out
print(movies_antonym)

------------------------------------------------------------------
Formatting Strings

custom_string = "String formatting"
print(f"{custom_string} is a powerful technique")

Usage:
- Title in a graph
- Show message or error
- Pass a statement to a function 

# Positional Formatting
- Placeholder replace by value 
'text {}' .format(value)
print("Machine learning provides {} the ability to learn {}".format("systems", "automatically"))

# Index Formatting
print("{2} {1} {0}".format("Python", "is", "cool"))
-> cool is Python

# Named Formatting
my_methods = {"tool": "Unsupervised algorithms", "goal": "patterns"}
print('{data[tool]} try to find {data[goal]} in the dataset'.format(data=my_methods))

# Format Specifiers
- :d -> integer
- :f -> float
- :s -> string
- :.2f -> float with 2 decimal places
- :.2% -> percentage with 2 decimal places
- :.2e -> scientific notation with 2 decimal places
- :.2g -> general format with 2 decimal places

print("Ony {0:f}% of the data is useful".format(12.3456))
print("Ony {:.2f}% of the data is useful".format(12.3456))

# Padding
print("The area of the circle is {0:0.2f} cm\u00b2".format(78.5))
-> The area of the circle is 78.50 cm²

# Formatting datetime 
from datteime import datetime 
print(datetime.now())

print("Today's date is {:%Y-%m-%d %H:%M}".format(datetime.now()))
-> Today's date is 2020-07-01 12:00
------------------------------------------------------------------
# Assign the substrings to the variables
first_pos = wikipedia_article[3:19].lower()
second_pos = wikipedia_article[21:44].lower()

# Define string with placeholders 
my_list.append("The tool {} is used in {}")

# Define string with rearranged placeholders
my_list.append("The tool {1} is used in {0}")

# Use format to print strings
for my_string in my_list:
  	print(my_string.format(first_pos, second_pos))

------------------------------------------------------------------
# Create a dictionary
plan = {
  	"field": courses[0],
        "tool": courses[1]
        }

# Complete the placeholders accessing elements of field and tool keys in the data dictionary
my_message = "If you are interested in {data[field]}, you can take the course related to {data[tool]}"

# Use the plan dictionary to replace placeholders
print(my_message.format(data=plan))

------------------------------------------------------------------
# Import datetime 
from datetime import datetime

# Assign date to get_date
get_date = datetime.now()

# Add named placeholders with format specifiers
message = "Good morning. Today is {today:%B %d, %Y}. It's {today:%H:%M} ... time to work!"

# Use the format method replacing the placeholder with get_date
print(message.format(today=get_date))

%d -> day
%B -> month name
%Y -> year
%H -> hour
%M -> minute
%b -> month abbreviation
%a -> day of the week abbreviation

------------------------------------------------------------------
# Formattes string literal
f"literal|string {expression}"

custom_string = "tool"
print(f"Machine learning is a {custom_string}")

Type conversion
- !s -> string
- !r -> repr, quote
- !a -> same as !r but escape non-ASCII characters

name = "Python"
print(f"Python is called {name!r} due to a comedy series")
-> Python is called 'Python' due to a comedy series

Format specifiers
- :d -> integer
- :f -> float
- :s -> string
- :e -> scientific notation

number = 90.351371
print(f"In the last 2 years, {number:.2f}% of the data was generated")
-> In the last 2 years, 90.35% of the data was generated

Index lookup
family = {"dad": "John", "mom": "Jane", "son": "Jack"}
print(f"{family['dad']} is a father")
-> John is a father

Escape characters using backslash
print(f"Machine learning is a powerful technique\nIt's used in data science")
-> Machine learning is a powerful technique
   It's used in data science

# Calling a function
def capitalize_word(word):
    return word.capitalize()

print(f"{capitalize_word('python')} is a programming language")
------------------------------------------------------------------
# Complete the f-string
print(f"Data science is considered {field1!r} in the {fact1}st century")

# Complete the f-string
print(f"{field3} create around {fact3:.2f}% of the data but only {fact4:.1f}% is analyzed")

# Complete the f-string
print(f"About {fact2:e} of {field2} in the world")

# Include both variables and the result of dividing them 
print(f"{number1} tweets were downloaded in {number2} minutes indicating a speed of {number1/number2:.1f} tweets per min")

# Replace the substring https by an empty string
print(f"{string1.replace('https', '')}")

# Access values of date and price in east dictionary
print(f"The price for a house in the east neighborhood was ${east['price']} in {east['date']:%m-%d-%Y}")
------------------------------------------------------------------
from string import Template
my_string = Template("Data science has been calles $identifier')
my_string.substitute(identifier="sexiest job of the 21st century)

- Use $$ to escape dollar sign

# Import Template
from string import Template

# Create a template
wikipedia = Template("$tool is a $description")

# Import Template
from string import Template

# Create a template
wikipedia = Template("$tool is a $description")

# Substitute variables in template
print(wikipedia.substitute(tool=tool1, description=description1))
print(wikipedia.substitute(tool=tool2, description=description2))
print(wikipedia.substitute(tool=tool3, description=description3))
------------------------------------------------------------------
# Import template
from string import Template

# Select variables
our_tool = tools[0]
our_fee = tools[1]
our_pay = tools[2]

# Create template
course = Template("We are offering a 3-month beginner course on $tool just for $$ $fee ${pay}ly")

# Substitute identifiers with three variables
print(course.substitute(tool=our_tool, fee=our_fee, pay=our_pay))
------------------------------------------------------------------
# Import template
from string import Template

# Complete template string using identifiers
the_answers = Template("Check your answer 1: $answer1, and your answer 2: $answer2")
------------------------------------------------------------------
# Import template
from string import Template

# Complete template string using identifiers
the_answers = Template("Check your answer 1: $answer1, and your answer 2: $answer2")

# Use substitute to replace identifiers
try:
    print(the_answers.substitute(answers))
except KeyError:
    print("Missing information")
------------------------------------------------------------------

# Import template
from string import Template

# Complete template string using identifiers
the_answers = Template("Check your answer 1: $answer1, and your answer 2: $answer2")

# Use safe_substitute to replace identifiers
try:
    print(the_answers.safe_substitute(answers))
except KeyError:
    print("Missing information")
------------------------------------------------------------------
Regex
r'sr\d\s\w{3,10'}
st = normal characters match themselves
\d = digit
\s = whitespace
\w = word
3, 10 = 

re module -> handle regex
import re
re.findall(r"regex", string)
re.findall(r"#movies", "Love #movies! I had fun yesterday going to the #movies")
--> ['#movies', '#movies']

------------------------------------------------------------------
re.split(r"regex", string)
re.split(r"!", "Nice place to eat! I'll come back! Excellent meat!")
-> menjadi list yg dipisah 

------------------------------------------------------------------
re.sub(r"regex", new, string)
re.sub(r"yellow", "nice", "I have a yellow car and yellow house in a yellow neighborhood")

\d -> digit
re.findall(r"lisar\d", "The winner are lisar9")
-> ["lisar9"]

\D -> non-digit
re.findall(r"lisar\D", "The winner are lisarN")
-> ["lisarN"]

\w -> word
\W -> non-word
re.findall(r"\W\d", "The winner got $9")
-> ['$9']

\s -> whitespace
\S -> non-whitespace

# Import the re module
import re

# Write the regex
regex = r"@robot\d\W"

# Find all matches of regex
print(re.findall(regex, sentiment_analysis))

# Write a regex to obtain user mentions
print(re.findall(r"User_mentions:\d", sentiment_analysis))

# Write a regex to obtain number of likes
print(re.findall(r"likes:\s\d", sentiment_analysis))

# Write a regex to obtain number of retweets
print(re.findall(r"number\sof\sretweets:\s\d", sentiment_analysis))

# Write a regex to match pattern separating sentences
regex_sentence = r"\W\dbreak\W"

# Replace the regex_sentence with a space
sentiment_sub = re.sub(regex_sentence, " ", sentiment_analysis)
------------------------------------------------------------------
# Write a regex to match pattern separating sentences
regex_sentence = r"\W\dbreak\W"

# Replace the regex_sentence with a space
sentiment_sub = re.sub(regex_sentence, " ", sentiment_analysis)

# Write a regex to match pattern separating words
regex_words = r"\Wnew\w"

# Replace the regex_words and print the result
sentiment_final = re.sub(regex_words, " ", sentiment_sub)
print(sentiment_final)

------------------------------------------------------------------
Repeated characters:

quantifiers:
A metacharacter that tells the regex eninge how many times to match a character immediately to its left

import re
password = "password1234"
re.search(r"\w{8}\d{4}", password)

- Once or more: +
text = "Date of start: 4-3. Date of registration: 10-04"
re.findall(r"\d+-\d+", text)
-> ['4-3', '10-04']

- Zero or more: *
my_string = "The concert was amazing! @amelia!a @joh&&n @mary90"
re.findall(r"@\w+\W*\w+", my_string)
-> ['@amelia!a', '@joh&&n', '@mary90']

- Zero or one: ?
my_string = "The color of this image is amazing. However, the colour blue could be brighter."
re.findall(r"colou?r", my_string)
-> ['color', 'colour']

- n times at least, m times at most: {n, m}
phone_number = "John: 1-966-847-3131 Michelle: 54-908-42-42424"
re.findall(r"\{1,2}-\d{3}-\d{2,3}-\d{4,5}", phone_number)
-> ['1-966-847-3131', '54-908-42-42424']

------------------------------------------------------------------
# Import re module
import re

for tweet in sentiment_analysis:
	# Write regex to match http links and print out result
	print(re.findall(r"https://www.\S+", tweet))

	# Write regex to match user mentions and print out result
	print(re.findall(r"@\w+\d+", tweet))

-> ['https://www.tellyourstory.com']
-> ['@blueKnight39']

------------------------------------------------------------------
# Complete the for loop with a regex to find dates
for date in sentiment_analysis:
	print(re.findall(r"\d{1,2}\s\w+\sago", date))

-> ['5 days ago']

# Complete the for loop with a regex to find dates
for date in sentiment_analysis:
	print(re.findall(r"\d{1,2}\w+\s\w+\s\d{4}", date))

-> ['23rd June 2018']

# Complete the for loop with a regex to find dates
for date in sentiment_analysis:
	print(re.findall(r"\d{1,2}\w+\s\w+\s\d{4}\s\d{1,2}:\d{2}", date))

-> ['23rd June 2018 17:54']

------------------------------------------------------------------
# Write a regex matching the hashtag pattern
regex = r"#\w+"

# Replace the regex by an empty string
no_hashtag = re.sub(regex, "", sentiment_analysis)

# Get tokens by splitting text
print(re.split(r"\s+", no_hashtag))
-> ['I', 'feel', 'so', 'happy', 'this', 'weekend']

------------------------------------------------------------------
Regex metacharacter

- Looking for patterns
re.search(r"regex", string)
re.search(r"@\w+", "The user @match is a great player")
-> <re.Match object; span=(9, 15), match='@match'>

re.match(r"regex", string)
re.match(r"@\w+", "The user @match is a great player")
-> None , because it looks at the beginning of the string

- Special characters
. -> Match any character except newline
my_links = "Just check out this link: www.amazingpics.com. It has amazing photos!"
re.findall(r"www.+com", my_links)
-> ['www.amazingpics.com']

^ -> Match the start of the string
my_string ="the 80s music was much better than the 90s"
re.findall(r"the\s\d+s", my_string)
-> ['the 80s', 'the 90s']

re.findall(r"^the\s\d+s", my_string)
-> ['the 80s'] -> only the first one

$ -> Match the end of the string
my_string = "the 80s music was much better than the 90s"
re.findall(r"the\s\d+s$", my_string)
-> ['the 90s']

\ -> Escape special characters
my_string = "I love the music of Mr.Go."
re.findall(r"Mr\.\s\w+", my_string)
-> ['Mr.Go']

| -> Alternation
my_string = "Elephants are the world's largest land animal! I would love to see an elephant one day"
re.findall(r"Elephant|elephant", my_string)
-> ['Elephant', 'elephant']

[] -> Match a set of characters
my_links = "Yesterday I spent my afternoon with my friend: MaryJohn2 Clary3"
re.findall(r"[a-zA-Z]+\d", my_links)
-> ['MaryJohn2', 'Clary3']

my_string = "My&name&is#John Smith. I%live$in#Miami"
re.sub(r"[#$%&]", " ", my_string)
-> 'My name is John Smith. I live in Miami'

^ -> Negation
my_links = "Bad website: www.99.com. Favorite site: www.hola.com"
re.findall(r"www[^0-9]+com", my_links)
-> ['www.hola.com']
------------------------------------------------------------------
# Write a regex to match text file name
regex = r"^[aeiouAEIOU]{2,3}\w+.txt"

for text in sentiment_analysis:
	# Find all matches of the regex
	print(re.findall(regex, text))
    
	# Replace all matches with empty string
	print(re.sub(regex, "", text))

-> ['ouMYTAXES.txt']

------------------------------------------------------------------
# Write a regex to match a valid email address
regex = r"[a-zA-Z0-9!#%&*\$\.]+@\w+\.com"

for example in emails:  
  	# Match the regex to the string
    if re.findall(regex, example):
        # Complete the format method to print out the result
      	print("The email {email_example} is a valid email".format(email_example=example))
    else:
      	print("The email {email_example} is invalid".format(email_example=example))   

-> The email n.john.smith@gmail.com is a valid email
   The email 87victory@hotmail.com is a valid email
   The email !#mary-=@msca.net is invalid

\$ -> dollar sign
\. -> dot
kenapa harus di-escape? karena merupakan special character
------------------------------------------------------------------
# Write a regex to check if the password is valid
regex = r"[a-zA-Z0-9*#\$%!&\.]{8,20}"

for example in passwords:
  	# Scan the strings to find a match
    if re.findall(regex, example):
        # Complete the format method to print out the result
      	print("The password {pass_example} is a valid password".format(pass_example=example))
    else:
      	print("The password {pass_example} is invalid".format(pass_example=example))   

->  The password Apple34!rose is a valid password
    The password My87hou#4$ is a valid password
    The password abc123 is invalid

------------------------------------------------------------------
Greedy vs Non-Greedy Matching
- Standard quantifiers are greedy by default: * + {n, m} ?

Greedy matching: 
- Match as much as possible-
- Backtrack when too many characters are matched
- Gives up characters one at a time

import re
re.match(r"\d+", "12345abc")
-> <re.Match object; span=(0, 5), match='12345'>

import re 
re.match(r".*hello", "xhelloxxxxxx")
-> <re.Match object; span=(0, 12), match='xhelloxxxxxx'>

Non-Greedy matching:
- Match as little as possible
- Add ? after quantifier
- Gives up characters one at a time

import re
re.match(r"\d+?", "12345abc")
-> <re.Match object; span=(0, 1), match='1'>

import re
re.match(r".*?hello", "xhelloxxxxxx")
-> <re.Match object; span=(0, 6), match='xhello'>

------------------------------------------------------------------
Non Greedy Matching/Lazy Matching:

# Import re
import re

# Write a regex to eliminate tags
string_notags = re.sub(r"<.+?>", "", string)

# Print out the result
print(string_notags)

------------------------------------------------------------------
# Write a lazy regex expression 
numbers_found_lazy = re.findall(r"\d+?", sentiment_analysis)

# Print out the result
print(numbers_found_lazy)
-> ['1', '9', '2', '2]

# Write a greedy regex expression 
numbers_found_greedy = re.findall(r"\d+", sentiment_analysis)

# Print out the result
print(numbers_found_greedy)
-> ['19', '22']

------------------------------------------------------------------
# Write a greedy regex expression to match 
sentences_found_greedy = re.findall(r"\(.*\)", sentiment_analysis)

# Print out the result
print(sentences_found_greedy)
-> ['(I thought the movie was incredible)', '(I thought the movie was incredible)']

# Write a lazy regex expression
sentences_found_lazy = re.findall(r"\(.*?\)", sentiment_analysis)

# Print out the results
print(sentences_found_lazy)
-> ['(I thought the movie was incredible)', '(It was the worst movie I've ever seen)']

------------------------------------------------------------------
Capturing Groups
- Parentheses are used to group regex tokens
- Capturing groups are saved in the match object

import re
re.search(r"(\d+).(\d+)", "128.345").group(1)
-> '128'

re.search(r"([A-Za-z]+)\s\w+\s(\d+)\s(\w+), "John Smith is 31 years old")
-> <re.Match object; span=(0, 25), match='John Smith is 31 years old'>

------------------------------------------------------------------
Capture a repeated group (\d+) vs. repeat a capture group (\d)+
- (\d+) -> Capture a repeated group
- (\d)+ -> Repeat a capture group

------------------------------------------------------------------
# Write a regex that matches email
regex_email = r"([A-Za-z0-9]+)@\S+"

for tweet in sentiment_analysis:
    # Find all matches of regex in each tweet
    email_matched = re.findall(regex_email, tweet)

    # Complete the format method to print the results
    print("Lists of users found in this tweet: {}".format(email_matched))

Lists of users found in this tweet: ['statravelAU', 'statravelpo']
Lists of users found in this tweet: ['Hollywoodheat34']
Lists of users found in this tweet: ['msdrama098']

------------------------------------------------------------------
# Import re
import re

# Write regex to capture information of the flight
regex = r"([A-Z]{2})(\d{4})\s([A-Z]{3})-([A-Z]{3})\s(\d{2}[A-Z]{3})"
-> LA4214 AER-CDB 06NOV

# Find all matches of the flight information
flight_matches = re.findall(regex, flight)
    
#Print the matches
print("Airline: {} Flight number: {}".format(flight_matches[0][0], flight_matches[0][1]))
print("Departure: {} Destination: {}".format(flight_matches[0][2], flight_matches[0][3]))
print("Date: {}".format(flight_matches[0][4]))

------------------------------------------------------------------
Alternating and non-capturing groups

Pipe |
my_string = "I want to have a pet. But I don't know if I want a cat, a dog, or a bird."
re.findall(r"cat|dog|bird", my_string)
-> ['cat', 'dog', 'bird']

my_string = "I want to have a pet. But I don't know if I want 2 cats, a dog, or a bird."
re.findall(r"\d+\scat|dog|bird", my_string)
-> ['2 cat', 'dog', 'bird']

------------------------------------------------------------------
Alternation
\d+\s(cat|dog|bird)
my_string = "I want to have a pet. But I don't know if I want 2 cats, a dog, or a bird."
re.findall(r"\d+\s(cat|dog|bird)", my_string)
-> ['cat', 'dog']

(\d+)\s(cat|dog|bird)
my_string = "I want to have a pet. But I don't know if I want 2 cats, a dog, or a bird."
re.findall(r"(\d+)\scat|dog|bird", my_string)
-> [('2', 'cat'), ('1', 'dog')]

------------------------------------------------------------------
Non-capturing groups:
- Match but not capture a group
- (?:\d{2}-){3}(\d{3}-\d{3})
042-900

-Use non-capturing groups for alternation
my_data = "Today is 23rd May 2019. Tomorrow is 26th May 19."
re.findall(r"\d)(?:th|rd)", my_data)
-> ['23', '26']

# Write a regex that matches sentences with the optional words
regex_positive = r"(love|like|enjoy).+?(movie|concert)\s(.+?)\."

for tweet in sentiment_analysis:
	# Find all matches of regex in tweet
    positive_matches = re.findall(regex_positive, tweet)
    
    # Complete format to print out the results
    print("Positive comments found {}".format(positive_matches))

# Write a regex that matches sentences with the optional words
regex_negative = r"(dislike|hate|disapprove).+?(?:movie|concert)\s(.+?)\."

for tweet in sentiment_analysis:
	# Find all matches of regex in tweet
    negative_matches = re.findall(regex_negative, tweet)
    
    # Complete format to print out the results
    print("Negative comments found {}".format(negative_matches))

------------------------------------------------------------------
Backreferences
text = Python 3.0 was release 0n 13-03-2008
information = re.search('(\d{1,2})-(\d{1,2})-(\d{4})', text)
information.group(3)
-> 2008

Named groups
(?P<name>regex)
text = "Austin, 70701"
cities = re.search(r"(?P<city>[A-Za-z]+).*?(?P<zipcode>\d{5})", text)
cities.group("city")
-> ('Austin')

- Using capturing groups to reference back to a group
(\d{1,2})-(\d{1,2})-(\d{4})\
\1-\2-\3

sentence = "I wish you a happy happy birthday!"
re.findall(r"(\w+)\s\1", sentence)
-> ['happy']

sentence = "I wish you a happy happy birthday!"
re.sub("r(\w+)\s\1", "\1", sentence)
-> 'I wish you a happy birthday!'

- Using named capturing groups to reference back 
(?P<name>regex)
senntence = "Your new code number is 23434. Please, enter 23434 to open the door."
re.findall(r"(?P<code>\d{5}).*?(?P=code)", sentence)
-> ['23434']

- Using named capturing groups to reference back 
(?P<name>regex)
sentence = "Your new code number is 23434. Please, enter 23434 to open the door."
re.sub(r"(?P<code>\d{5}).*?(?P=code)", "\g<code>", sentence)
-> 'Your new code number is 23434. Please, enter 23434 to open the door.'

------------------------------------------------------------------
Character that need to be escaped:
. ^ $ * + ? { } [ ] ( ) | \

------------------------------------------------------------------
re.match(pattern, string) → Mencari pola hanya di awal string.
re.search(pattern, string) → Mencari pola di mana saja dalam string (pertama yang cocok saja).
re.findall(pattern, string) → Mengembalikan semua kecocokan dalam bentuk list.

------------------------------------------------------------------
# Write regex and scan contract to capture the dates described
regex_dates = r"Signed\son\s(\d{2})/(\d{2})/(\d{4})"
dates = re.search(regex_dates, contract)

# Assign to each key the corresponding match
signature = {
	"day": dates.group(2),
	"month": dates.group(1),
	"year": dates.group(3)
}
# Complete the format method to print-out
print("Our first contract is dated back to {data[year]}. Particularly, the day {data[day]} of the month {data[month]}.".format(data=signature))
-> Our first contract is dated back to 2001. Particularly, the day 25 of the month 03.

------------------------------------------------------------------
for string in html_tags:
    # Complete the regex and find if it matches a closed HTML tags
    match_tag =  re.match(r"<(\w+)>.*?</\1>", string)
 
    if match_tag:
        # If it matches print the first group capture
        print("Your tag {} is closed".format(match_tag.group(1))) 
    else:
        # If it doesn't match capture only the tag 
        notmatch_tag = re.match(r"<(\w+)>", string)
        # Print the first group capture
        print("Close your {} tag!".format(notmatch_tag.group(1)))

------------------------------------------------------------------
# Complete the regex to match an elongated word
regex_elongated = r"\w*(\w)\1\w*"

for tweet in sentiment_analysis:
	# Find if there is a match in each tweet 
	match_elongated = re.search(regex_elongated, tweet)
    
	if match_elongated:
		# Assign the captured group zero 
		elongated_word = match_elongated.group(0)
        
		# Complete the format method to print the word
		print("Elongated word found: {word}".format(word=elongated_word))
	else:
		print("No elongated word found")     	

------------------------------------------------------------------
Look around 

- Look ahead
(?=regex) -> Positive look ahead
(?!regex) -> Negative look ahead

- Look behind
(?<=regex) -> Positive look behind
(?<!regex) -> Negative look behind

my_string = "I love cherries, apples, and strawberries."
re.findall(r"\w+(?=,)", my_string)
-> ['cherries', 'apples']

my_string = "I love cherries, apples, and strawberries."
re.findall(r"\w+(?<!es)\b", my_string)
-> ['cherries', 'apples', 'strawberries']

- Positive look ahead
my_text = "tweets.txt transferred, tweets2.txt transferred, tweets3.txt error"
re.findall(r"\w+\.txt(?=\stransferred)", my_text)
-> ['tweets.txt', 'tweets2.txt']

- Negative look ahead
my_text = "tweets.txt transferred, tweets2.txt transferred, tweets3.txt error"
re.findall(r"\w+\.txt(?!\stransferred)", my_text)
-> ['tweets3.txt']

- Positive look behind
my_text = "tweets.txt transferred, tweets2.txt transferred, tweets3.txt error"
re.findall(r"(?<=transferred\s)\w+\.txt", my_text)
-> ['tweets.txt', 'tweets2.txt']

- Negative look behind
my_text = "tweets.txt transferred, tweets2.txt transferred, tweets3.txt error"
re.findall(r"(?<!transferred\s)\w+\.txt", my_text)
-> ['tweets3.txt']

------------------------------------------------------------------
# Positive lookahead
sentiment_analysis = 'You need excellent python skills to be a data scientist. Must be! Excellent python'
look_ahead = re.findall(r"\w+(?=\spython)", sentiment_analysis)

# Print out
print(look_ahead)
-> ['excellent', 'Excellent']

# Positive lookbehind
look_behind = re.findall(r"(?<=[Pp]ython\s)\w+", sentiment_analysis)

# Print out
print(look_behind)
-> ['skills']

lookahead => followed by -> dibelakang regex 
lookbehind => preceded by -> didepan regex
------------------------------------------------------------------
for phone in cellphones:
	# Get all phone numbers not preceded by area code
	number = re.findall(r"(?<!\d{3}-)\d{4}-\d{6}-\d{2}", phone)
	print(number)

for phone in cellphones:
	# Get all phone numbers not followed by optional extension
	number = re.findall(r"\d{3}-\d{4}-\d{6}(?!-\d{2})", phone)
	print(number)
