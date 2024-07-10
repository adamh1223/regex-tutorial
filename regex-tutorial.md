# Regex Tutorial

A regular expression (regex) is a way to search through a string of text and group searches together. Regex allows you to get pieces of text and perform several actions with the text, such as validation or find and replace. There are many actions you can do with the text you are targeting.

Regex is used differently for different languages, such as JavaScript, Ruby, Python, and more. Today we will be taking a look at how regex works within JavaScript.

3 common use cases of regex in JavaScript are:
 1. Validating user-generated input
 2. Searching through a large body of text
 3. Find and replace

## Summary

To use regex, you have a body of text you're searching through, and you write your regular expression at the top. This is what you are searching for. The regular expression almost always starts and ends with a **`/`**. Between the slashes, you have your search pattern. After the closing **`/`**, you have flags. Flags are the different search filters that target text using different parameters. 

The default settings are to find the first exact match of whatever is between the slashes. To change this behavior, flags are necessary.

In the following example of the flags syntax, the global flag is used on a search of the word 'cat': **`/cat/g`**. 
We will get to flags later in this lesson.

There are two methods of using regex in JavaScript:
1. Use a string as a regex expression
2. Use a regex object

Let's take a closer look at each of these methods.

Let's say you want to search through a website's URL parameters to grab specific information. For this example, we'll use wix.com. This is the link we'll use: `https://www.wix.com/lp-en/website-builder?utm_source=google&utm_medium=cpc&utm_campaign=18723764133^141603088966^search%20-%20us^&experiment_id=website^e^631152329166^&gad_source=1&gclid=CjwKCAjwnK60BhA9EiwAmpHZwzrkMLBTkedJKcQH1es7sEhfc-OsxveEBqDdfOUGcJvFmqMYQmpgnBoCzc8QAvD_BwE`

1. Open the console, and let's create our first regex expression. Enter the following line:

    ```javascript
    var regex = /\w+/gi
    ```

    Notice the syntax of the regex expression. The `\` denotes that we are not actually trying to match the letter w, but rather 'words'. The `+` denotes that we are looking for all words. After the `/` are the `g` (global) flag and the `i` (case insensitive) flag. This means we are looking for all instances on any line, and regardless of its capitalization. As of now, this will return undefined because we have not told the browser what to search through. What this search means in plain English is "Give me every word that appears in the text I am searching through."

2. Let's grab the URL so we can search through it using regex. Use this command:

    ```javascript
    var url = window.location.href
    ```

    and console.log the URL to make sure it's being retrieved.

3. Now that we have the URL, let's run a regex expression against the URL.

4. Give the console the command:

    ```javascript
    window.location.href.match(/\w+/gi)
    ```

    Here we are using the match method to run our regex against the URL. We'll get back every word in the URL.

5. Now let's give the command:

    ```javascript
    window.location.href.match(/\w+/gi)[0]
    ```

    What this means is "give me the first index (or instance) of a word in the URL." Recall that 0 index means 'first'.

6. Let's try and grab digits instead of words. Try the command:

    ```javascript
    window.location.href.match(/\d+/gi)
    ```

    Notice that this returns all of the digits within the URL.

7. Let's say we want to grab a value within the URL. Recall that URL parameters are comprised of key-value pairs. Let's say we want to grab 'cpc' from the part of the URL `utm_medium=cpc`. In this instance, `utm_medium` is a key, and `cpc` is the value. 

    Try the command:

    ```javascript
    window.location.href.match(/utm_medium=(\w+)/i)
    ```

    Notice that we get back an array with 2 indices:
    - Index 0: "utm_medium=cpc"
    - Index 1: "cpc"

    To retrieve the value "cpc", simply add a `[1]` to the previous command. Notice that there are parentheses around the `\w+`. We have turned the search into a capture group. Stay tuned for more on grouping later in the lesson.

### 2. Use a regex object

1. Try the command:

    ```javascript
    var pattern = new RegExp(/\w+/, "gi")
    ```

    followed by:

    ```javascript
    pattern.test("hello")
    ```

    This should return true. Notice the syntax change. We are using the `RegExp` object, and passing in our regex as the first parameter, and flags as the second parameter. In plain English, this means "Tell me if the string 'hello' has words in it, wherever it appears in the string and whether or not it's capitalized." The response is of course 'true'.

For the majority of instances in this lesson, we will use strategy 1: Use a string as a regex expression.

Now we will take a look at some of the individual components within regex.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)
- [Hex example](#hex-example)
- [Author](#author)

## Regex Components

### Anchors

Anchors don't match any characters. They tell the regex engine where matches can begin and end, to ensure that a regex matches a string at a specific place. Some places that can be specified are the beginning or end of a string or line, a word, or non-word boundary. 

The `^` and `$` symbols are anchors. They match the beginning or end of a string.

**`^` carrot:**
- Beginning of a string (or line)
- This means that `^a` matches an `a` at the beginning of a string.

**`$` dollar sign:**
- End of a string (or line)
- This means that `e$` matches an `e` at the end of a string (or line).

### Quantifiers

Quantifiers are special characters that quantify a given pattern. More specifically, they quantify the pattern immediately preceding them.

**`+`**
- The `+` quantifier takes its preceding pattern and quantifies it 1 or more times. This means the regex `/q+/` will match 'q', 'qq', 'qqq', etc.
- The regex `/ca+t/` will match 'cat', 'caat', 'caaat', etc. This is because the `+` only acts on what immediately precedes it. In this case, that is the letter `a`. If you wanted it to act on more than just the letter `a`, you could group characters together immediately before the `+` symbol.

**`*`**
- The `*` quantifier looks for the preceding pattern 0 or more times. It matches strings even where the pattern before the `*` never appears. Now if you had the example of `/ca*t/`, you will still get the same matches as `/ca+t/`, but now you will also match 'ct'.

**`?`**
- The `?` quantifier looks for its preceding pattern 0 or 1 times. If you had the regex `/ca?t/`, it would only match 'ct', and 'cat'.

**Custom Ranges:**

**`{count}`**
- Matches its preceding pattern exactly the specified number of times.

**`{min,max}`**
- Matches its preceding pattern from the value of min to the value of max number of times.

**`{min,}`**
- Matches its preceding pattern for the value of min or more times.

### OR Operator

The **`|`** (or) Operator is used if you are looking for a character OR another character, and you want either to match. In the expression `/(t|T)he/g`, you want either a lowercase `t` or an uppercase `t` to match.

The basic syntax of the `|` operator is:
`pattern1|pattern2`

Let's take a look at an example use case for the or operator:

```javascript
let text = "The cat sat on the mat."
let regex = /cat|dog/
console.log(text.match(regex)) // ["cat"]
```


This regex matches either "cat" or "dog". In the given text, it matches "cat".

The or operator also comes in handy when looking for any of a group of patterns. Here is an example demonstrating this:

```javascript
let text = "I like apples and oranges.";
let regex = /I like (apples|bananas|oranges)/;
console.log(text.match(regex)); // ["I like apples", "apples"]
```

Here, the regex matches the phrase "I like " followed by either "apples", "bananas", or "oranges". In the text, it matches "I like apples".
    




### Character Classes

Character classes are ways to search using special characters. The special characters have different search filters and can be used to specify your search. If you want to use a special character without it being a character class, and you are searching for, say, a period, then you can use the \ to escape the character. By using \. you are letting it know that you are actually searching for a period, not using the period as a character.

Here are some commonly used character classes: 


**`+`** character class: 1 or more
    A common use case of character classes is a scenario where you want to search for 1 or more letter. Let's say you want to find all instances of the letter e, or ee, or eee, or eeee, and so on. If you want to find all instances of one or more e's, you would use the + character after the letter e. In this case, the + is a character class.


**`?`** character class: optional
    Another commonly used character class is the ? character. This means optional. if you're searching for 
    /ea?/g, this means that you want to find all instances of e, AND if there is any instances of ea, you also want those. Think "Whatever is before it is optional"


**`*`** character class: 0 or more
    This is a combination of the + and the ?. It is saying it will accept a match for the character it's placed after, and it will still run the search for what came before that character if there is no match. Also, if there are multiple characters matching the one being searched with * that are the same next to each other, it will find ALL of those consecutive characters. If you search /re*/g, it will find instances of just r, but also ree, or reee.


**`.`** character class: Anything except new line
    The . character class matches everything, but the rest of the regex expression will be adhered to. For example, if you search /.at/g, then you'll get fat, cat, and eat matching. This regex expression means "Give me any instances of any character + at. The only thing this won't match is a new line. If you wanted to search for any character that comes before a period, as well as a period, your regex expression would look like /.\./g


**`\w`** character class: Match any word
    This is exactly what it sounds like. Every word is matched.

**`\W`** character class: Negative (opposite) version of match any word. Match anything but a word.


**`\s`** character class: Match any white space
    This is exactly what it sounds like. Every space is matched.

**`\S`** character class: Negative (opposite) version of match any white space. Match anything but a white space.


**`{1, 2}`** character class: Set minimum and maximum.
    With {} you can specify a minimum and maximum character length you'd like a word to be to fit the match. This is used with \w. For example, if your regex expression was /\w{4,5}/g, this means you are looking for all words that are 4 or 5 characters. You can also do a search such as /\w{4,}/g. By not specifying a maximum, there is no maximum. Therefore this search would give you all words that are 4 characters or more.


**`^`** (carrot) Character class: Match the beginning of a group of text
    This searches for a match if the character is at the beginning of a group of text. You can also use the multiline flag, which will give you a match if the character you're searching for matches the first character of any line.

**`$`** Character class: Match the end of a group of text
    This is the opposite of the ^ class.


**`\d`** character class: any digit
    This grabs any digits.


### Flags

Flags filter through search results each in their own unique ways. To use a flag, add the corresponding letter of the flag after the closing / in the regular expression. You can use multiple flags at once. For example, if you used the global flag and the case insensitive flag simultaneously, your regex would expression would look like /something/gi.


There are several main flags that are important to be familiar with:


global: **`/g`**
    The global flag allows us to match anywhere in the string. If you search /at/g as your regex expression, your results will show ALL instances of 'at'. If you take the global flag off of this search, it will only find the FIRST instance of the search. 
    This is the most commonly used flag


case insensitive: **`/i`**
    The case insensitive flag matches text regardless of its case (uppercase/lowercase). The default is to find an exact match to the casing in the search of the regex expression, unless you are using this flag.


multiline: **`/m`**
    The multiline flag searches any line in a body of text. This is especially relevant when using the ^ and $ characters, because you can search all lines, instead of just the first line. This makes the ^ and $ boundary tokens match the beginning and end of each line.

unicode: **`/u`**
    Enables unicode support. This makes the regex expression treat characters as code points, rather than code units. This is only necessary when using characters not found in the UTF-16 character set. 

dot all (AKA single-line mode): **`/s`**
    Makes the wild character . match everything on any line, not just the first line in which the regex expression is found.

sticky: **`/y`**
    Makes the regex expression start the search from the index in its lastIndex property. Without changing the lastIndex property on an expression that uses the y flag, nothing changes. It is also necessary to change the lastIndex, so that your search can match the string from the custom index. 


Example: 

```javascript
let text = "The quick brown fox jumps over the lazy dog.";
let regex = /the/gi;
console.log(text.match(regex)); // ["The", "the"]
```

Here, This regex matches all instances of "the", regardless of case. This is because the g and i flags are present in the regex expression.


### Grouping and Capturing

Anything inside of () is going to be its own group, and only act upon themselves. If you wanted to search for either lowercase 'the' or uppercase 'The', then your regex expression would look like /(t|T)he/g.
The parentheses are needed, because without them, your expression would be /t|The/g, which means something different. The parentheses are needed to accomplish our goal in this instance, by keeping the t and T in a group. The | is the or symbol, and it means look for this OR that (t or T in this case). Both The and the will match.

If you searched /(t|e|r){2,3}/g, this will search for instances of between 2 and 3 letter combinations of either t, e, or r.

By putting things in groups, you are saying you only want to do special things to the characters in those groupings. If you have the expression /re{2,3}/g, the {2,3} is only acting on the e, not the r. But if you have an expression /(re){2,3}, now the {2,3} is acting on both the r and the e.

Example: 

```javascript
let text = "apple pie and banana pie";
let regex = /(apple|banana) pie/g;
console.log(text.match(regex)); // ["apple pie", "banana pie"]

```

Here, This regex matches either "apple pie" or "banana pie".


### Bracket Expressions

Bracket expressions define a set of characters to match.

**[abc]**
Matches any character in the set (a, b, or c).

**[^abc]**
Matches any character not in the set.

**[]** Match anything inside of the []
Whatever is inside the [] is matched, and it can be any character within the [], you don't need an entire match. If you have /[fc]at/g then you will match cat and fat.
Inside of [] you can also do ranges.


**[a-z]** range: Match anything within the range specified
Whatever range specified in the [] is searched in the same fashion as described in the [] character set. This is case sensitive, so if you wanted all lowercase and uppercase letters, your regex expression would look like /[a-z][A-Z]at/g. Now you'll get any letter + at.
You can also do this with numbers (e.g. [0-9])


### Greedy and Lazy Match

There are 'greedy' and 'lazy' matches in regex.

1. Greedy matching:
A greedy match tries to match as much of the input string as possible. By default, regex quantifiers (*, +, ?, {n,m}) are greedy.

Consider the following string: 
```javascript
const text = "The quick brown fox jumps over the lazy dog.";
```

If you use a greedy regex to match everything between the first and last quotation marks, it would look like this:

```javascript
const regex = /".*"/;
const result = text.match(regex);
console.log(result); // ["The quick brown fox jumps over the lazy dog."]
```
    
Here, .* is a greedy quantifier. It tries to match as many characters as possible, consuming the entire string between the first and last quotation marks.

2. Lazy matching:
A lazy match tries to match as little of the input string as possible. You can make a quantifier lazy by appending a ? after it.

Example of Lazy Matching
Using the same string:
```javascript
const text = 'He said, "The quick brown fox jumps over the lazy dog."';
```

If you want to match everything between each pair of quotation marks lazily, you would use:

```javascript
const regex = /".*?"/;
const result = text.match(regex);
console.log(result); // ["The quick brown fox jumps over the lazy dog."]
```

Here, .*? is a lazy quantifier. It tries to match as few characters as possible, stopping at the first closing quotation mark.


Example: 

```javascript
let text = "The quick brown fox jumps over the lazy dog.";
let regex = /q.*k/;
console.log(text.match(regex)); // ["quick brown fox jumps over the lazy dog."]

let regexLazy = /q.*?k/;
console.log(text.match(regexLazy)); // ["quick"]

```

The greedy regex matches from "q" to the last "k", while the lazy regex matches from "q" to the first "k".

### Boundaries

In JavaScript, boundaries in regex are used to specify positions in the text where certain conditions must hold true. These boundaries don't match actual characters but rather positions in the text that meet certain criteria.

Types of Boundaries:
**Word Boundaries (\b and \B):**

**`\b`** 
Matches a position where a word character (usually a-z, A-Z, 0-9, or underscore) is not followed or preceded by another word character.

**`\B`** 
Matches a position where \b does not match, i.e., a position between two word characters or two non-word characters.

Practical Examples:
**Word Boundaries (\b)**
Example 1: Matching Whole Words

String: "Hello, world! Hello again, world."

Regex: /\bHello\b/

```javascript
    const text = "Hello, world! Hello again, world.";
    const regex = /\bHello\b/g;
    const result = text.match(regex);
    console.log(result); // ["Hello", "Hello"]
```

We can see that the result matches "Hello" as a whole word in 2 instances.

Example 2: Finding Word Positions

String: "start middle end"

Regex: /\b\w+\b/g

Explanation:

\b: Match a word boundary
\w+: Match one or more word characters
\b: Match a word boundary


```javascript
    const text = "start middle end";
    const regex = /\b\w+\b/g;
    const result = text.match(regex);
    console.log(result); // ["start", "middle", "end"]
```

We can see that the result matches all of the words in the string within the text variable.
**Non-word Boundaries (\B)**

A non-word boundary is the opposite of /b (word boundary). 

```javascript
const text = "The quick brown fox jumps over the lazy dog.";
const regex = /\Bfox/B\;
console.log(text.match(regex));
```
    


### Back-references

Back-references in regular expressions allow you to refer to previously captured groups within the same regex pattern. This is useful for finding repeated patterns or for reusing portions of the match. In JavaScript, back-references are denoted by a backslash followed by the number of the capturing group. Capturing groups are numbered by the order of their opening parentheses `(`

Let's take a look at an example of a basic back-reference in regex:

```javascript
let text = "He said, 'hello' and then 'hello' again.";
let regex = /'(hello)' and then '\1'/;
console.log(text.match(regex)); //RESULT: ["'hello' and then 'hello'", "hello"]
```

In this example, '(hello)' is the first capturing group, and \1 is the back-reference to this group. The regex matches the pattern where "hello" is repeated.

Back-references are typically used to find repeated patterns. For example, to find a word that repeats in a sentence, you might use:

```javascript
let text = "The cat chases the cat.";
let regex = /\b(\w+)\b.*\b\1\b/;
console.log(text.match(regex)); // ["The cat chases the cat", "cat"]
```

Here, (\w+) captures a word, and \1 matches the same word later in the string.


### Look-ahead and Look-behind


Look-aheads and Look-behinds help you look ahead of and behind target characters. If you want to match everything that comes before or after a specific character or word, these will be very helpful.


Positive Look-behind: **`(?<=)`**
To use a positive look-behind, you need to put it inside of a group. The group will start with ?<= . The < symbol means we want to 'look behind' and the = symbol means it's positive, so it must match what's in the expression. Let's say you want to get every first character that is preceeded by the word 'the'. You can use a positive look-behind and type: /(?<=[tT]he)./g . This means that you want the first character after any instance of lowercase or uppercase 'the'. The '.' is targeting any character. The actual word 'the' will NOT be selected, ONLY the character that comes directly after it.


Negative Look-behind: **`(?<!)`**
Negative look-behind is saying we want to get anything that does not have the expression before it. If you had the expression /(?<![tT]he)./g every single character that is not preceeded by the word the will match.


Positive Look-ahead: **`(?=)`**
This is how you look for characters that come before your search. Let's say you wanted to match any character that comes before the characters a and t. You would write this as /.(?=at)/g . Now if you had the words fat, cat, or eat, the f in fat, the c in cat, and the e in eat would match.


Negative Look-ahead: **`(?!)`**
This looks for anything except what's in front of what you specify, the opposite of the positive look-ahead. The opposite for the example given for the positive look-ahead would be /.(?!at)/g, and this would give you everything except characters that come before at.


Example: 

```javascript
let text = "apple pie and banana pie";
let regex = /apple(?= pie)/;
console.log(text.match(regex)); // ["apple"]

let regexNegative = /apple(?! pie)/;
console.log(text.match(regexNegative)); // null

```

The positive look-ahead regex matches "apple" only if it's followed by "pie". The negative look-ahead regex does not match "apple" if it's followed by "pie".

### Hex Example

Now let's combine some of these concepts to break down a practical example of a regex expression.

The following regex is designed to match a valid hexadecimal color value, either in the 6-digit format (e.g., #a3c113) or the 3-digit shorthand format (e.g., #a3c).

This is a very common use case for regex, so let's take a look.

Regex: **`/^#?([a-f0-9]{6}|[a-f0-9]{3})$/`**

Now at first this may seem intimidating, but let's break this down using concepts we've covered so far. 


Overview of the Expression:

    ^ asserts the start of the string.
    #? matches an optional hash symbol.
    ([a-f0-9]{6}|[a-f0-9]{3}) is a capturing group that matches either 6 or 3 hexadecimal digits.
    $ asserts the end of the string.

Breakdown of the Regex Pattern:
Start of String **`(^)`**

^: Asserts the position at the start of the string.
Optional Hash Symbol **`(#?)`**

#?: Matches an optional hash symbol (#). The question mark (?) means that the preceding character (in this case, #) is optional and can appear 0 or 1 time.

**Capturing Group** ([a-f0-9]{6}|[a-f0-9]{3}):

([a-f0-9]{6}|[a-f0-9]{3}): This is a capturing group that matches either a 6-digit or 3-digit hexadecimal number.

Hex Digits:

[a-f0-9]: Matches any single character that is a hexadecimal digit, which can be any lowercase letter from a to f or any digit from 0 to 9.
Six Hex Digits **`({6})`**

[a-f0-9]{6}: Matches exactly six hexadecimal digits.
Three Hex Digits **`({3})`**

[a-f0-9]{3}: Matches exactly three hexadecimal digits.
Alternation **`(|)`**`

|: The alternation operator allows for matching either the pattern on its left side or its right side. So, [a-f0-9]{6}|[a-f0-9]{3} matches either six hex digits or three hex digits.
End of String **`($)`**

**`$`** Asserts the position at the end of the string.

Testing the expression with strings

Here are a few test cases we can try with this regex:

Matching with a 6-digit hexadecimal color:

```javascript
let regex = /^#?([a-f0-9]{6}|[a-f0-9]{3})$/;
console.log(regex.test("#a3c113")); // true
console.log(regex.test("a3c113"));  // true
```


#a3c113 and a3c113 both match because they contain exactly six hexadecimal digits, with the hash symbol being optional.

Matching a 3-digit hexadecimal color:

```javascript
let regex = /^#?([a-f0-9]{6}|[a-f0-9]{3})$/;
console.log(regex.test("#a3c")); // true
console.log(regex.test("a3c"));  // true
```

#a3c and a3c both match because they contain exactly three hexadecimal digits, with the hash symbol being optional.

Non-Matching Examples:

```javascript
let regex = /^#?([a-f0-9]{6}|[a-f0-9]{3})$/;
console.log(regex.test("#a3c11")); // false (only 5 digits)
console.log(regex.test("a3c1134")); // false (7 digits)
console.log(regex.test("#g3c113")); // false (contains 'g', which is not a hex digit)
```

#a3c11 does not match because it has only 5 digits.
a3c1134 does not match because it has 7 digits.
#g3c113 does not match because it contains 'g', which is not a valid hexadecimal digit.



## Author

Written by Adam Hussain, a future web developer bootcamp grad from the EdX UC Berkeley bootcamp. Adam is a talented developer who creates intuitive user interfaces, and is passionate about simplifying the user experience on webpages. Adam's github can be found at: https://github.com/adamh1223