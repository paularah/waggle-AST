# **DSA Final Project:** 

##                                               Abstract
As primarily a JavaScript and Python developer, I want to be able to easily convert snippets of code written in Python to JavaScript and vice versa. This is enable programmers avoid rewritting boiler plate logic that have been implemented in Python or JavaScript when they are switching languages and also to make it easy to migrate a codebases from Python to JavaScript and vice versa. 

##                                       Problem Importance
Javascript and Python are the two most popular programming languages. Javascript versality in the web and python's versality across multiple domains have made them a popular choice of language for programmers. There exists some similarities between both languages. Both JavaScript and Python are interpreted languages low level languages and the support multiple modern paradigms of programming like object oriented and functional programming. At a first glance some of the most noticable difference between both language is in namespaces, ndentation, implementation of object oriented programming and the 




##                                           Problem Background

According to wikipedia, there are over 700 pogramming languages. With the evolving of new programming languages and sometimes ceasation of support for older ones, programmers have to learn new languages, rewrite codebases in a newer language and handle many other quirks that emanate from changing the langauge a program is written in. The aim of this    


##                                   Solution Requirement
At the barest minimum, the solution to this problem should satisfy the following constraints:
* Accurate conversion from Python to JavaScript
* Handle syntatic edge cases like regular function declaration and arrow functions in JavaScript, regular functions and function expressions(lambda functions) in Python, etc
* Parsing should be achieved in worst case scenerio of ** *O(nlogn)* ** time. This is  particularly important because we are dealing with an input(sourcecode string) of arbitraty size which might vary depending on the amount of sourcecode we are parsing. 
* Parsing should be done in ** *O(n2)* ** time. The rational behind this is that since sourcecode is fundamentally a text file, the input size is expected to be small and in the case of large input size, space can be easily compensated for in  comparison to time. 



##                                                   Solution Approach                      
Considering the constraints outlined above, we can aproach solving this problem in the following ways:


**Search and replace:** This is the most bruteforce approach that comes to mind when trying to solve this problem. Since both Python and Javascript have similar constucts, we can simply search through the sourcecode of one language and output the equivalent construct of the later. We loop through the source of the first language and generate the equivalent of construct based on the values we encounter. We first store what a construct translates to in the other language. For example, a function declared with the "def" keyword in Python is the same as a function declared with the "function" keyword in JavaScript. When searching through the Python sourcecode, if we ecounter the string "def", we output the string "function" in the new sourcecode we are generating for JavaScript. Considering that we might be working with a sourcecode of arbitraty size, we need a way to store how the different constructs map to each other in the two programming languages to enable us search faster. A data structure suitable for this task should allow us to easily search for the equivalent of a construct. The most ideal option in this scenerio would be a hast table based data structure. This can be a map, dictionary or object. We can then map the equivalent construct in way similar to this below.

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
