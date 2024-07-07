# Regex Tutorial

A regular expressions (regex) is a way to search through a string of text, and group searches together. Regex allows get pieces of text and perform a number of actions with the text, such as validation or find and replace. There are many actions you can do with the text you are targeting.

2 commmon use cases of regex are:
 1. Validating user generated input
 2. Search through a large body of text
 3. Find and replace

## Summary

Briefly summarize the regex you will be describing and what you will explain. Include a code snippet of the regex. Replace this text with your summary.

To use regex, you have a body of text you're searching through, and you write your regular expression at the top. This is what you are searching for. The regular expression almost always starts and ends with a /. Between the slashes, you have your search pattern. After the closing /, you have flags. Flags are the different search filters that target text using different parameters. 

The default settings are to find the first exact match of whatever is between the slashes. To change this behavior, flags are necessary.

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

## Regex Components



### Anchors

Anchors don't match any characters. They tell the regex engine where matches can begin and end, to ensure that a regex matches a string at a specific place. Some places that can be specified are the beginning or end of a string or line, a word, or non-word boundary. The ^ and $ symbols are anchors. They match the beginning or end of a string.

### Quantifiers

### OR Operator

The | (or) Operator is used if you are looking for a character OR another character, and you want either to match. In the expression /(t|T)he/g , you want either a lowercase t or an uppercase t to match. 

### Character Classes

Character classes are ways to search using special characters. The special characters have different search filters and can be used to specify your search. If you want to use a special character without it being a character class, and you are searching for, say, a period, then you can use the \ to escape the character. By using \. you are letting it know that you are actually searching for a period, not using the period as a character.

Here are some commonly used character classes: 


+ character class: 1 or more
    A common use case of character classes is a scenario where you want to search for 1 or more letter. Let's say you want to find all instances of the letter e, or ee, or eee, or eeee, and so on. If you want to find all instances of one or more e's, you would use the + character after the letter e. In this case, the + is a character class.


? character class: optional
    Another commonly used character class is the ? character. This means optional. if you're searching for 
    /ea?/g, this means that you want to find all instances of e, AND if there is any instances of ea, you also want those. Think "Whatever is before it is optional"


* character class: 0 or more
    This is a combination of the + and the ?. It is saying it will accept a match for the character it's placed after, and it will still run the search for what came before that character if there is no match. Also, if there are multiple characters matching the one being searched with * that are the same next to each other, it will find ALL of those consecutive characters. If you search /re*/g, it will find instances of just r, but also ree, or reee.


. character class: Anything except new line
    The . character class matches everything, but the rest of the regex expression will be adhered to. For example, if you search /.at/g, then you'll get fat, cat, and eat matching. This regex expression means "Give me any instances of any character + at. The only thing this won't match is a new line. If you wanted to search for any character that comes before a period, as well as a period, your regex expression would look like /.\./g


\w character class: Match any word
    This is exactly what it sounds like. Every word is matched.

\W character class: Negative (opposite) version of match any word. Match anything but a word.


\s character class: Match any white space
    This is exactly what it sounds like. Every space is matched.

\S character class: Negative (opposite) version of match any white space. Match anything but a white space.


{1, 2} character class: Set minimum and maximum.
    With {} you can specify a minimum and maximum character length you'd like a word to be to fit the match. This is used with \w. For example, if your regex expression was /\w{4,5}/g, this means you are looking for all words that are 4 or 5 characters. You can also do a search such as /\w{4,}/g. By not specifying a maximum, there is no maximum. Therefore this search would give you all words that are 4 characters or more.


^ (carrot) Character class: Match the beginning of a group of text
    This searches for a match if the character is at the beginning of a group of text. You can also use the multiline flag, which will give you a match if the character you're searching for matches the first character of any line.

$ Character class: Match the end of a group of text
    This is the opposite of the ^ class.


\d characrer class: any digit
    This grabs any numbers


### Flags

Flags filter through search results each in their own unique ways. To use a flag, add the corresponding letter of the flag after the closing / in the regular expression. You can use multiple flags at once. For example, if you used the global flag and the case insensitive flag simultaneously, your regex would expression would look like /something/gi.


There are several main flags that are important to be familiar with:


global: '/g'
    The global flag allows us to match anywhere in the string. If you search /at/g as your regex expression, your results will show ALL instances of 'at'. If you take the global flag off of this search, it will only find the FIRST instance of the search. 
    This is the most commonly used flag


case insensitive: '/i'
    The case insensitive flag matches text regardless of its case (uppercase/lowercase). The default is to find an exact match to the casing in the search of the regex expression, unless you are using this flag.


multiline: '/m'
    The multiline flag searches any line in a body of text. This is especially relevant when using the ^ and $ characters, because you can search all lines, instead of just the first line. This makes the ^ and $ boundary tokens match the beginning and end of each line.

unicode: '/u'
    Enables unicode support. This makes the regex expression treat characters as code points, rather than code units. This is only necessary when using characters not found in the UTF-16 character set. 

dot all (AKA single-line mode): '/s'
    Makes the wild character . match everything on any line, not just the first line in which the regex expression is found.

sticky: '/y'
    Makes the regex expression start the search from the index in its lastIndex property. Without changing the lastIndex property on an expression that uses the y flag, nothing changes. It is also necessary to change the lastIndex, so that your search can match the string from the custom index. 


### Grouping and Capturing

Anything inside of () is going to be its own group, and only act upon themselves. If you wanted to search for either lowercase 'the' or uppercase 'The', then your regex expression would look like /(t|T)he/g.
The parentheses are needed, because without them, your expression would be /t|The/g, which means something different. The parentheses are needed to accomplish our goal in this instance, by keeping the t and T in a group. The | is the or symbol, and it means look for this OR that (t or T in this case). Both The and the will match.

    If you searched /(t|e|r){2,3}/g, this will search for instances of between 2 and 3 letter combinations of either t, e, or r.

    By putting things in groups, you are saying you only want to do special things to the characters in those groupings. If you have the expression /re{2,3}/g, the {2,3} is only acting on the e, not the r. But if you have an expression /(re){2,3}, now the {2,3} is acting on both the r and the e.


### Bracket Expressions


[]: Match anything inside of the []
    Whatever is inside the [] is matched, and it can be any character within the [], you don't need an entire match. If you have /[fc]at/g then you will match cat and fat.
    Inside of [] you can also do ranges


[a-z] range: Match anything within the range specified
    Whatever range specified in the [] is searched in the same fashion as described in the [] character set. This is case sensitive, so if you wanted all lowercase and uppercase letters, your regex expression would look like /[a-z][A-Z]at/g. Now you'll get any letter + at.
    You can also do this with numbers (e.g. [0-9])


### Greedy and Lazy Match

### Boundaries

### Back-references

### Look-ahead and Look-behind


Look-aheads and Look-behinds help you look ahead of and behind target characters. If you want to match everything that comes before or after a specific character or word, these will be very helpful.


Positive Look-behind: (?<=)
    To use a positive look-behind, you need to put it inside of a group. The group will start with ?<= . The < symbol means we want to 'look behind' and the = symbol means it's positive, so it must match what's in the expression. Let's say you want to get every first character that is preceeded by the word 'the'. You can use a positive look-behind and type: /(?<=[tT]he)./g . This means that you want the first character after any instance of lowercase or uppercase 'the'. The '.' is targeting any character. The actual word 'the' will NOT be selected, ONLY the character that comes directly after it.


Negative Look-behind: (?<!)
    Negative look-behind is saying we want to get anything that does not have the expression before it. If you had the expression /(?<![tT]he)./g every single character that is not preceeded by the word the will match.


Positive Look-ahead: (?=)
    This is how you look for characters that come before your search. Let's say you wanted to match any character that comes before the characters a and t. You would write this as /.(?=at)/g . Now if you had the words fat, cat, or eat, the f in fat, the c in cat, and the e in eat would match.


Negative Look-ahead: (?!)
    This looks for anything except what's in front of what you specify, the opposite of the positive look-ahead. The opposite for the example given for the positive look-ahead would be /.(?!at)/g, and this would give you everything except characters that come before at.


## Author

Written by Adam Hussain, a future web developer bootcamp grad from the EdX UC Berkeley bootcamp. Adam is a talented developer who creates intuitive user interfaces, and is passionate about simplifying the user experience on webpages. Adam's github can be found at: https://github.com/adamh1223