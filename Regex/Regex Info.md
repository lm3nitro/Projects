## Charsets

You can specify a range of characters by using a charset. range of characters that you want to match. A charset is defined by using square brackets ==[]==. 

When using charsets, you have to be specific which includes specifying uppercase and lowercase. 

Within a charset, you can also use a dash ==-== to signify a range or letters or numbers. You can also exclude a character within a charset using the ==^== symbol. 

**Examples:** 

Exact Match:
1. Match a,b,c: `[abc]`
2. Match mat, rat, sat: `[mrs]at`

Specific lowercase/uppercase:
3. Match Mat, mat, Rat, rat: `[MmRr]at `

Range:
4. Match Test1, Test2, test3, test4, Test5: `[Tt]est[1-5]`
5. Match same as above except Test5: `[Tt]est[^5]`

## Wildcards and Optional Characters

In regex, a ==.== is used as a wildcard to match any single character, except line breaks (spaces). 

If there is an optional character that may or not be needed, than a ==?== can be used. 

Note, if you want to search for a literal dot, then you will need to escape it with a reverse slash. 

**Examples:**

Match Mat, rat, sat, Cat: `.at`
Match Mat, mats, sat, Cats: `[MmsC]ats?`
Match google.com: `google\.com`
Match the domains cat.com, rats.com, mat.com: `[crm]ats?\.com`
Match a 5 letter string that doesn't end in a-f: `...[^a-f]`
Match bat, bats, hat, hats, but not rat or rats: `[^r]ats?`

## Metacharacters and Multiples

Metacharacters are used as an easier way to match bigger charsets. 

`\d` matches a digit, like `9`  
`\D` matches a non-digit, like `A` or `@`  
`\w` matches an alphanumeric character, like `a` or `3` 
Note: `\w` Also includes underscores __
`\W` matches a non-alphanumeric character, like `!` or `#`  
`\s` matches a whitespace character (spaces, tabs, and line breaks)  
`\S` matches everything else (alphanumeric characters and symbols)


`{5}` - **exactly 5 times.  
`{1,7}` - 1 to 5 times.  
`{3,}` - 3 or more** times.  
`*` - **0 or more** times.  
`+` - **1 or more** times.

**Examples:**

1. Match ratssssss: `rats{6}`
2. Match Mat, mats, matsss: `[Mm]ats*`
3. Match both I like regex, I like regexxxxx: `I like regex+`
4. Match ab0001, bb0000, abc1000, cba0110, c0000 (don't use a metacharacter):
`[abc]{1,3}[01]{4}`
5. Match Test81, test72, Test4, test1: `[Te]est\d{1,2}`
6. Match king pawn, king     pawn: `king\s+pawn`
7. Match pawn~, love@, boat#, gate!: `\w{5}\W`
8. Match  4h6u@o5p0%!     a)h43kl2!k8l7: `\S*\s*\S*`
9. Match a 9-character string that doesn't end with #: `\S{8}[^#]`
10. Match the following file names .bash_rc, .chess_book, and chess101: `\.?\w+`

## Starts, Ens, Groups, Either/Or

`^` - starts with  
`$` - ends with
Note: When specifying a character in at the start the symbol `^` needs to be used outside of brackets`[]`. When excluding a character, it needs to be enclosed inside the brackets `[^]`. 

Parenthesis `()`are used to define groups. To say "or" in Regex, the pipe  `|` is used. 

**Examples:**

1. Match all strings that start with "Password:" followed by any 16 characters excluding "9": `^Password:[^9]{16}`
2. Match "username: " in the beginning of a line (note the space!): `^username:\s`
3.  Match every line that doesn't start with a digit: `^\D`
4. Match this string at the end of a line CHESS!: `CHESS\!$`
5. Match all of the following sentences
- I like nachos
- I use tacos
  `I like (nachos|tacos)`
6. Match all lines that start with $, followed by any single digit,  
followed by $, followed by one or more non-whitespace characters
`\$\d\$\S+`
7. Match every possible IPv4 IP address: `(\d{1,3}\.){3}\d{1,3}`
8. Match all of these emails while also adding the username and the domain name (not the TLD) in separate groups (use \w): login@domain.com, test@xyz.com, lm3nitro@portfolio.com
`(\w+)@(\w+)\.com`

