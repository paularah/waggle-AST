# **DSA Final Project:** 

##                                               Abstract
As primarily a JavaScript and Python developer, I want to be able to easily convert snippets of code written in Python to JavaScript and vice versa. This wil enables programmers to avoid rewriting boilerplate logic that has been implemented in Python or JavaScript when they are switching languages and also to make it easy to migrate codebases from Python to JavaScript and vice versa almost enirely automating the process. 

##                                       Problem Importance
  Particularly in the context of web development, it is common to see both languages used together either entirely on the server-side(e.g multiple containers running with instances of node.js and Django/flask ) or the client-side typically written in Javascript and the server side in Python or any of its derivative web frameworks. Being able to convert between the two languages could potentially save programmers time spent on rewriting same program across the two languages. With the continuous evolution of new programming languages and sometimes cessation of support for older ones, programmers have to learn new languages, rewrite codebases in a newer language, and handle many other quirks that emanate from changing the language a program is written in. Solving this problem would promote convergence around how programming languages are used.


##                                           Problem Background

According to Wikipedia, there are over 700 programming languages with Javascript and Python being the two most popular. Javascript versatility in the web and python's versatility across multiple domains have made them a popular choice of language for programmers. There exists some similarities between both languages. Both JavaScript and Python are interpreted, high-level languages, and support multiple modern paradigms of programming like object-oriented and functional programming. Since 1995 when Sun Microsystems first championed the "Write once, run anywhere" philosophy, many other programming languages have evolved to allow the same source code to be compiled different target machines. software kits like google's flutter allow the same source to be used for android, iOS, Windows, Mac, Linux, Google Fuchsia, and the web. This philosophy is the main driver behind the quest to solve this problem, a sort of "Write once, translate, and use anywhere. 


##                                   Solution Requirement
At the barest minimum, the solution to this problem should satisfy the following constraints:
* Near accurate conversion from Python to JavaScript.
* Handle syntactic edge cases like regular function declaration and arrow functions in JavaScript, regular functions and function expressions(lambda functions) in Python, etc
* Parsing should be achieved in the worst-case scenario of ** O(nlogn) ** time. This is particularly important because we are dealing with an input(sourcecode string) of arbitrary size which might vary depending on the amount of source code we are parsing. 
* Parsing should be done in ** O(n2) ** space complexity. The rationale behind this is that since source code is fundamentally a text file, the input size is expected to be small, and in the case of large input size, space can be easily compensated for in comparison to time. 


##                                                   Solution Approach                      
Considering the constraints outlined above, we can approach solving this problem in the following ways:


**Search and replace:** This is the most brute-force approach that comes to mind when trying to solve this problem. Since both Python and Javascript have similar constructs, we can simply search through the source code of one language and output the equivalent construct of the later. We loop through the source of the first language and generate the equivalent of construct based on the values we encounter when searching. We first store what a construct translates to in the other language. For example, a function declared with the "def" keyword in Python is the same as a function declared with the "function" keyword in JavaScript. When searching through the Python source code, if we encounter the string "def", we output the string "function" in the new source code we are generating for JavaScript.  
```python
#string containing sourcecode to be converted

source_code = "function myfunc(){}"
               
#splitting the string into its individual constructs based on the space delimiter
source_code_list = source_code.split(" ")

#Now we have an array of every construct in the source code and can easily
output_code_list = []


for construct in source_code_list:
    if construct == "function":
        output_code_list.append("def ")      
    elif construct == "{}":
        output_code_list.append("   ")
    else:
    output_code_list.append(construct)
```

We could write some more custom logic in the conditionals here, but it already begins to become clear that logic is convoluted, and its almost impossible to account for all the edge cases. Seeing that replace text matching is bound to fail here, we then turn to a more powerful way of matching text - regular expressions.   Regex is an excellent tool for working with complex strings. 
```
```
Regex already begins to fail here, in the process of converting, we have to make 

Seeing how is it impossible for us to insert a wide range of custom conditionals depending on the scenario we encounter while parsing the source code, it becomes clear that regex is insufficient to accomplish this task and the complexities it involves and also handle it at scale. Looking at the approach so far, we can still make some optimizations to how we store and access data(what a construct translates to) rather than hardcoding it directly into our logic. 

Considering that we might be working with a source code of arbitrary size, we need a way to store how the different constructs map to each other in the two programming languages to enable us to search faster. A data structure suitable for this task should allow us to easily search for the equivalent of a construct. The most ideal option in this scenario would be a hast table-based data structure. This can be a map, dictionary, or object. We can then map the equivalent construct in a way similar to this below.

```javascript
{
  'def': 'function'
  'for': 'for',
  'while': 'while'
  '==': '===',
  'type': 'typeof',
  '{}': ' '
  
}
```
When parsing the source code of the first language, in this case, Python. While searching, we simply check our map to see what a text translates as to. This is easily achieved in O(n) time, since searching for the equivalent of a construct takes O(1) time.  
N/B: Some assumptions were made here like mapping the equality operator in python to strict equality in javascript, braces to tabs indentation, etc. 

#### Limitations of search and replace: 
Through this problem-solving process, another constraint that wasn't originally taken into consideration becomes obvious. The same piece of code might represent something else depending on the surrounding context. How then do we handle an expression, block of code in relation to its context? Search and replace so far has not been a good approach to solving this. 




*At the core of this approach is a representation problem. How best do we best represent and model our data to be enable us to transform it from one form to another while preserving the original information the data represents? What is the most accurate data structure that suits this task? Do existing data structures suits this task and satisfy the constraints? do we need to make modifications to enable existing data structures properly model our data ?*









Take an expression like this below
```javascript
const x = 3 + 1 * 7;
```
A common way to represent this  


Handling scope
Say a we have two nested functions like this

```javascript
function foo(){
  //outer scope
  
  function bar(){
    //inner scope
  }
}
```
Parsng this into valid syntax in another language would be extremely difficult. Both javasc

How we handle accuracy? we can now use a set of predefined syntactic rules on how the AST should be processed and us decide what to do when we encounter certain kinds of node on numerous circumstances. Take for example below
```python
4 + "30"
```
In python, this would throw a TypeError while in JavaScript, both values in the expression would be treated as strings and concatenated together because of implicit type coercion. Some of the somewhat quirky behaviors in JavaScript like this can be eliminated by running the source in "strict mode" or using TypeScript(a typed superscript of javascript). Parsing source code this way allows us to make decisions on how we want the AST to be transformed, especially syntactic and context decisions which we have seen can easily mar the conversion process. 
