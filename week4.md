# Week 4 Outline

## Questions from last week?

- Git workflows?
(Dr\.|Mr\.|Ms\.|\Mrs\.).*.*\h(\p{L}).*.*\h([A-Z]).*
- Troubles with the assignment?

## Marine Biology in Alaska

#### See Moodle for more details!

```
2019 Marine Biology in Alaska Program will be offered from July 12 to August 6,
2018. Students will have the opportunity to earn 7 credit hours of upper division
courses from among:

Undergraduate Research (Biol. 3999, 3 credit hours)
Neurobiology (Biol. 4177, 3 credit hours)
Principles of Ecology ( Biol. 4253, 3 credit hours)
Marine Communities (Biol. 4262, 3 credit hours)
Marine Communities Lab (Biol. 4263, 1 credit hour)

The program fee is $3950.

273 students have participated in the first 11 years of the Program.

There will be several Marine Biology in Alaska Travel fund scholarships available.

We will hold an informational meeting for students who may be interested in the
Program on Wednesday September 19th at 7PM in Williams Hall 202.
```


## Regular Expressions

- [ ] What are Regular Expressions?
  - Editing text files is one of the most common tasks in computational biology
  - Often these files contain datasets whose format needs to be changed or from which some information needs to be extracted
  - Accomplishing these tasks by hand is tedious at best, and impossible in many cases
  - Regular expressions are an advanced form of find-and-replace that can be used to manipulate complex patterns of text
  - Regular expressions are often abbreviated as `regex`
  - Regular expression syntax can be used in a variety of different programs and contexts, including Atom, `grep`, and `sed`.


- [ ] Basic Regex Synatx (in Atom)
  - To begin, go to the `Find` menu and select `Find in Buffer`. This will conduct a search on the file that you currently have open.
  - By default, Atom does __not__ use regular expression syntax. It will interpret all text strings literally. To illustrate this, look for `^` in this file. Atom should immediately highlight the character at this point in the file.
  - Now turn on regex by clicking the option illustrated by `.*` in the Find option menu (upper right). Now what happens when you search with that same character? Atom should select the beginning of every line in the file. In regex syntax, `^` has a special meaning as the beginning of a line.
  - Now try searching with $. What happens? Both `^` and `$` are special characters for regex.
  - Now try searching these other special character combinations: `\n`, `\s`, `\w`, `.`, and `\d`.
    - Each of these is a type of wildcard. What do they match?


- [ ] Find and Replace
  - One of the most powerful uses of regex is to reformat files. This requires finding the text patterns that you're interested in and replacing them (or adding to them).
  - In Atom, you can type literal text strings into the `Replace in current buffer` field.

```
To start, try these combinations in the Find and Replace fields, then run
Replace All (You might need to click Find first). Undo the changes after each
example. What

(1) Find: ^
    Replace: NNN

(2) Find: $
    Replace: NNN

(3) Find: \n
    Replace: \s

(3a) Repeat (3), but with " " (a single space - no quotes) as the replacement
     string. How is this different than (3)? Why?

(4) Find: .
    Replace: Nothing, leave the field empty.
    What happens? What's left?
```

- [ ] Escaping special characters
  - What if you wanted to search for a literal '$' in your file? How would you do it?
  - The \ is used to "escape" these characters with special meanings. So, if you search for \$, it will match any literal dollar signs in your file.
  - Note that this is essentially the opposite of the way \ is used with regular letters (like \n). In that case, it actually _gives_ the letter meaning. But when escaping, it _takes away_ the special meaning.

```
To explore the behavior of escaping characters, try the following:

(1) Try searching for \$ and $? What does Find with regex match in each case?

(2) Try searching for \\n? What does Find with regex match in this case? Why?
```


- [ ] Customized wildcards
  - Sometimes (often, actually) the exact wildcard you need won't be built in. Thankfully, regex includes an easy way to set up any custom wildcard. Simply put the list of characters you want to be included in your wildcard inside square brackets, like this:
    - `[AB]`
    - `[12345]`
    - `[_A56]`
  - Note that, in Atom, you can choose whether or not your search is case sensitive by toggling the button `Aa` next to the regex (`.*`) button. However, most uses of regex, especially on the command line, __are__ case sensitive. To be consistent with other implementations, go ahead and __turn on case sensitivity__.
  - Ranges of letters or numbers can be included in custom sets, to avoid having to type each one individually. For instance,
    - `[0-9]` matches all individual digits
    - `[A-Z]` matches all capital letters
    - `[d-h]` matches lowercase letters between d and h
    - Be careful about specifying a range that includes both uppercase and lowercase letters. You always have to start with uppercase, but notice what happens when you end with lowercase, like `[A-z]`. What unexpected characters are included? Why? Hint: check Practical Computing.
    - You can also create wildcards that match anything _except_ what you specify by adding ^ to the beginning of your wildcard descript. Note that this is different than the use of ^ to specify the beginning of a line when it's not inside square brackets. So, to find all characters that are not lower-case letters, you could use `[^a-z]`

```
Try creating a wildcard that will match any of the punctuation characters *,
-, [, ], and _. Remember what you just learned about escaping.
```

- [ ] Repeating characters
  - Often, you'll want to search for more than one instance of a particular type of character in a row. To search for one or more instances of that character, add a +.
    - `t+`
  - To look for a specific number of instances, you can follow the character with braces and the precise number of instances you're looking for.
    - `t{2}`
  - To look for zero or more instances (when you're not sure if the character will be present in any amount, but you want to match it if so), use *.
    - `A*` - This will match 0-... As. But what happens if you use only this as your search?

```
    Here's a more useful application. Let's say you want to find any cases
    where a . is followed by a -, but there may be white space (symbolized
    by the wildcard \s, which includes spaces, tabs, and line ends) in
    between. Note that we should escape both the . and -, since they are
    symbols and might have special meaning. How would you write this search
    as a regular expression? What do you find?
```


- [ ] Capturing text in search fields
  - It's very useful to capture some portion of the text matched by a search in order to use it as part of the replacement.
  - To capture part of the search text, simply surround it with parentheses, ( and ).

```
\[\s\]\s+(\w+)\s+(\w+)\s+(\w+)

What would be captured with this search? Hint: add spaces between the individual elements of the regex string to get a better sense for the pattern.
```

  - To use the captured text in the replacement, you'll need to indicate which of the captured fields you'd like to use. Regex numbers them based on the order of the parentheses used in your search. To indicate the first captured field, use `$1`, to indicate the second captured field, use `$2`, etc.

```
What would happen if the previous search string was combined with this
replacement string?

$2\t$1
```


- [ ] sed
  - `sed` is a Unix program whose name stands for "stream editor".
  - `sed` is extremely powerful. Here, we're only going to take advantage of its search and replace functionality.
  - Fair warning: there can be a number of small differences to regex syntax between different systems (e.g., Mac v Linux) and different programs (e.g., Atom v sed). You might have to experiment sometimes.
  - To use `sed` for find and replace, you'll need to give it instructions that look like this:
    - `'s/<FIND_TEXT>/<REPLACE_TEXT>/g'`
  - To start, try giving `sed` input using a pipe:
    - `echo "Here is some text." | sed 's/some/a little/g'`
    - What does the output look like? Where does `sed` send the output?
  - `sed` can also use the text of a file as input. Here's an example:
    - `touch inputFile.txt` - Create new file.
    - `echo "She sells seashells down by the seashore." >> inputFile.txt` - Add text.
    - `sed 's/seashells/dinosaur bones/g' inputFile.txt` - Run find and replace.
    - `cat inputFile.txt` - What's in file?
    - Have you heard of [Mary Anning](https://en.wikipedia.org/wiki/Mary_Anning)?
  - Perhaps most often, you'll want to just do a find and replace on text in the file directly, and not have the output sent to the screen. To do that, use this syntax:
    - `sed -i"_backup" 's/She/Darwin/g' inputFile.txt`
    - Note that this syntax will run the replacement and


## Week 4 Assignment (Due Tuesday, Sept. 18th by 1:30)

For this assignment, we are going to use temperature data from some Swedish sub-arctic lakes (https://bolin.su.se/data/Crill-2015). We'll focus on 3 files that include temperature measurements from June 11th, 2009 until June 2nd, 2010.
  - `Strdln_Twater_090611-090828_corrd.csv`
  - `Strdln_Twater_090829-091012_corrd.csv`
  - `Strdln_Twater_091015-100602_corrd.csv`

Fork the week 4 repository (containing the data files) from the class GitHub page and clone it to your VM. In your clone, create a new branch with your name. Use Atom and any Terminal commands as needed to complete the steps below. Commit your changes and new files to your branch and push this back to your fork of the repository on GitHub. Then submit a pull request to the class page.

__For all of these steps, save your answers to questions, as well as a description of what you did (including what program you used, as well as any regex expressions and Terminal commands you used) in a file called `Week4Answers.txt`. Please keep them organized!__

1. The first thing to do when working with data files for the first time, especially those created by someone else, is to look through them. Do you notice any inconsistencies across files in how data are recorded?

2. Let's say we are preparing to analyze these data in a program that has certain formatting requirements. For each of these requirements, write one or more series of Find and Replace statements using regex syntax.
  - The data fields are currently delimited by semicolons, but our program requires tab-delimited files.
  - Our program does not handle blank fields. Empty fields should be replaced with NaN (which stands for "Not a Number").
    - Double check that you've done this properly. Do you still see empty fields? If so, why? Turn on invisible characters to help.
  - Our program does not like whitespace on the end of lines. Remove all whitespace (spaces and tabs) from line ends.
  - Our program does not like spaces in any of its headers or fields. Replace spaces with underscores (_).
  - Our program does not like lines with all missing information (only NaNs). Delete all characters from these lines.
  - Our program can only handle one digit after the decimal place for each number. Replace all numbers with trimmed versions.


3. We also need one file that contains all measurements (from all input files) with samples from 2009. What is a series of Terminal commands (should only take 2 or 3) to create a new file with all 2009 measurements. This file should still have one header line at the top (and none elsewhere in the file). Save the commands you use to do

4. Use regex and grep to extract all lines from all files that are complete (have no NaN values) and save them in a new file called `completeLakeTemps.txt`.

5. The full dataset is pretty large and we want to run some analyses that only have the measurements recorded on the 1st and 15th days of each month. Use grep with regex to create one combined file with just these measurements and save it as `SemiMonthlyLakeTemps.txt`.

6. We'd like to do an analysis that only looks at nighttime temperatures. Use grep and regex to create a file that contains samples taken between 8PM (20:00) and 6AM (06:00). Save this file as `NightTimeLakeTemps.txt`.

7. Lastly, we'd like to do separate analyses for the temperatures taken at different depths. Create separate files that contain measurements for each depth (but across all lakes). Save these as `depth_0.1m.txt`, `depth_0.3m.txt`, etc.
