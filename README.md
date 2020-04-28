# **DSA Final Project:** 

##                                               Abstract
As primarily a JavaScript and Python developer, I want to be able to easily convert snippets of code written in Python to JavaScript and vice versa. This is enable programmers avoid rewritting boiler plate logic that has been implemented in Python or JavaScript when they are switching languages and also to make it easy to migrate a codebases from Python to JavaScript and vice versa. 

##                                       Problem Importance
  Particularly in the context of web development, it is common to see both languages used together either entirely on the server side(e.g multiple containers running with instances of node.js and django/flask ) or the client side typically written in Javascript and the server side in Python or any of its deriavative web frameworks. Being able to convert between the two languages could potentially save programmers time spent on rewritting same program accross the two languages. With the continous evolving of new programming languages and sometimes ceasation of support for older ones, programmers have to learn new languages, rewrite codebases in a newer language and handle many other quirks that emanate from changing the langauge a program is written in. Solving this problem would promote a convegence around how programming languages are used.


##                                           Problem Background

According to wikipedia, there are over 700 pogramming languages with Javascript and Python being the two most popular. Javascript versality in the web and python's versality across multiple domains have made them a popular choice of language for programmers. There exists some similarities between both languages. Both JavaScript and Python are interpreted, high level languages and support multiple modern paradigms of programming like object oriented and functional programming. Since 1995 when Sun Microsystems first championed the "Write once, run anywhere" philosophy, many other programming languages have evolved to allow the same sourcecode to be compiled different target machines. software kits like google's flutter allow the same source to be used for android, iOS, Windows, Mac, Linux, Google Fuchsia and the web. This philisophy is the main driver behind the quest to solve this problem. 


##                                   Solution Requirement
At the barest minimum, the solution to this problem should satisfy the following constraints:
* Near accurate conversion from Python to JavaScript.
* Handle syntatic edge cases like regular function declaration and arrow functions in JavaScript, regular functions and function expressions(lambda functions) in Python, etc
* Parsing should be achieved in worst case scenerio of ** O(nlogn) ** time. This is  particularly important because we are dealing with an input(sourcecode string) of arbitraty size which might vary depending on the amount of sourcecode we are parsing. 
* Parsing should be done in ** O(n2) ** time. The rational behind this is that since sourcecode is fundamentally a text file, the input size is expected to be small and in the case of large input size, space can be easily compensated for in  comparison to time. 


##                                                   Solution Approach                      
Considering the constraints outlined above, we can aproach solving this problem in the following ways:


**Search and replace:** This is the most bruteforce approach that comes to mind when trying to solve this problem. Since both Python and Javascript have similar constucts, we can simply search through the sourcecode of one language and output the equivalent construct of the later. We loop through the source of the first language and generate the equivalent of construct based on the values we encounter when searching.  
```python
//string containing sourcecode to be converted
source_code = "function myfunc(){
               }"
//splitting the string into its individual constructs based on the space delimiter
source_code_list = source_code.split(" ")

//Now we have an array of every construct in the source code and can easily

output_code_list = []



for construct in source_code_list:
    if construct == "def":
        output_code_list.append(construct)
    elif construct == 


```

we could write some more custom logic in the conditionals here, but it aready begins to become clear that logic is convulated and its almost imposible to account for all the egde cases. 
Regex already begins to fail here, in the process of converting, we have to make 


We first store what a construct translates to in the other language. For example, a function declared with the "def" keyword in Python is the same as a function declared with the "function" keyword in JavaScript. When searching through the Python sourcecode, if we ecounter the string "def", we output the string "function" in the new sourcecode we are generating for JavaScript. Considering that we might be working with a sourcecode of arbitraty size, we need a way to store how the different constructs map to each other in the two programming languages to enable us search faster. A data structure suitable for this task should allow us to easily search for the equivalent of a construct. The most ideal option in this scenerio would be a hast table based data structure. This can be a map, dictionary or object. We can then map the equivalent construct in way similar to this below.

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
When parsing the sourcecode of the first first language, in this case Python. While searching, we simply check our map to see what a text translates as to. This is easily achieved in O(n) time, since searching for the equivalent of a construct takes O(1) time.  
N/B: Some assumptions were made here like mapping the equality operator in python to strict equality in javascript, braces to  tabs indentation, etc. 

#### Limitations of search and replace: 



*At the core of this approach is a representation problem. How best do we best represent and model our data to be enable us to transform it from one form to another while preserving the original information the data represents? What is the most accurate data struture that suits this task? Do existing data structures suits this task and saisfy the constaints? do we need to make modifications to enable existing data structures properly model our data ?*









Take an expression like this below
```javascript
const x = 3 + 1 * 7;
```
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
Parsng this into valid syntax in another language would be extremely difficult. Both javascri

```
