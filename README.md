# **DSA Final Project:** 

##                                               Abstract
As primarily a JavaScript and Python developer, I want to be able to easily convert snippets of code written in Python to JavaScript and vice versa. This will enable programmers to avoid rewriting boilerplate logic that has been implemented in Python or JavaScript when they are switching languages and also to make it easy to migrate codebases from Python to JavaScript and vice versa almost enirely automating the process. 

##                                       Problem Importance
  Particularly in the context of web development, it is common to see both languages used together either entirely on the server-side(e.g multiple containers running with instances of node.js and Django/flask ) or the client-side typically written in Javascript and the server side in Python or any of its derivative web frameworks. Being able to convert between the two languages could potentially save programmers time spent on rewriting same program across the two languages. With the continuous evolution of new programming languages and sometimes cessation of support for older ones, programmers have to learn new languages, rewrite codebases in a newer language, and handle many other quirks that emanate from changing the language a program is originally written in. Solving this problem would promote convergence around how programming languages are used.


##                                           Problem Background

According to Wikipedia, there are over 700 programming languages with Javascript and Python being the two most popular. Javascript versatility in the web and python's versatility across multiple domains have made them a popular choice of language for programmers. There exists some similarities between both languages. Both JavaScript and Python are interpreted, high-level languages, and support multiple modern paradigms of programming like object-oriented and functional programming. Since 1995 when Sun Microsystems first championed the "Write once, run anywhere" philosophy, many other programming languages have evolved to allow the same source code to be compiled different target machines. software kits like google's flutter allow the same source to be used for android, iOS, Windows, Mac, Linux, Google Fuchsia, and the web. This philosophy is the main driver behind the quest to solve this problem, a sort of "Write Once, Translate, and Use Anywhere". 


##                                   Solution Requirement
At the barest minimum, the solution to this problem should satisfy the following constraints:
* Near accurate conversion from Python to JavaScript.
* Handle syntactic edge cases like regular function declaration and arrow functions in JavaScript, regular functions and function expressions(lambda functions) in Python, etc
* Parsing should be achieved in the worst-case scenario of *O(nlogn)* time. This is particularly important because we expect to be dealing with an input(sourcecode string) of arbitrary size. The size might vary depending on the amount of source code the user is parsing. 
* Parsing should be done in *O(n2)* space complexity. The rationale behind this is that since source code is fundamentally a text file, the input size is expected to be small, and in the case of large input size, space can be easily compensated for in comparison to time. 


##                                                   Solution Approach                      
Considering the constraints outlined above, we can approach solving this problem in the following ways:

### Search and replace

This is the most brute-force approach that comes to mind when trying to solve this problem. Since both Python and Javascript have similar constructs, we can simply search through the source code of one language and output the equivalent construct of the later. We loop through the source of the first language and generate the equivalent of construct based on the values we encounter when searching. We first store what a construct translates to in the other language. For example, a function declared with the "def" keyword in Python is the same as a function declared with the "function" keyword in JavaScript. When searching through the Python source code, if we encounter the string "def", we output the string "function" in the new source code we are generating for JavaScript.  
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

We could write some more custom logic in the conditionals here, but it already begins to become clear that logic is convoluted, and its almost impossible to account for all the edge cases. Seeing that search text matching is bound to fail here, we then turn to a more powerful way of matching text - regular expressions. Regex is an excellent tool for working with complex strings. 
```
<tag[^>]*>(.*?)</tag>
```

Trying to parse the sourcedoe with regular expressions was quite initimidating and a quick google search shows a well documented pattern of people failing to parse strings with complex and recursive structures(see the appendix for references to this). As the name implies, regular expressions are designed for parsing regular languages. A regular language is something that can be interpreted with a finite-state machine(Olsson, 2019). Programming languages and mark up languages are not regular languages and this cannot be parsed with regular expressions. Seeing how is it impossible for us to insert a wide range of custom conditionals depending on the scenario we encounter while parsing the source code, it becomes clear that regex is insufficiently sophisticated to accomplish this task and also handle it at scale.   


Looking back at the approach so far, we can still make some optimizations to how we store and access data(what a construct translates to) rather than hardcoding it directly into our logic making it easier to work with. Considering that we might be working with a source code of arbitrary size, we need a way to store how the different constructs map to each other in the two programming languages to enable us to search faster. A data structure suitable for this task should allow us to easily search for the equivalent of a construct. The most ideal option in this scenario would be a hash table-based data structure. This can be a map, dictionary, or object. We can then map the equivalent construct in a way similar to this below.
 
```javascript
{
  'def': 'function'
  'for': 'for',
  'while': 'while'
  '==': '===',
  'type': 'typeof',
  '{}': ' ',
  '': 'var',
  '': 'const',
  '':'let'
}
```
When parsing the source code of the first language, in this case, Python. While searching, we simply check our map to see what a text translates as to. This is easily achieved in O(n) time, since searching for the equivalent of a construct takes O(1) time. While this has made it easy for to able to quickly access and translate from the sourcecode, it fails to address the main problem.  
**N/B:** Some assumptions were made here like mapping the equality operator in python to strict equality in javascript, braces to tabs indentation, etc. 


**Additional Insight from Approach:** Through this problem-solving process, another constraint that wasn't originally taken into consideration becomes obvious. The same piece of code might represent something else depending on the surrounding context. How then do we handle an expression, block of code in relation to its context? Search and replace so far has not been a good approach to solving this. 

#### Limitations of search and replace:
**Handling Scope:** Say a we have two nested functions like this

```javascript
function foo(){
  //outer scope
  
  function bar(){
    //inner scope
  }
}
```
Search and replace blindly transverses the sourcecode without context and is bound to error. No doubt some clever conditioning can be done here to handle this but it not only increases the complexity but introduces newer problems. 

## Encoding:
What if we could somehow come up with a standard way to encode the source code in a way the makes it easier to parse and convert considering the difficulties we have experienced so far with the source code in its raw form. Ideally, this format should be independent of any of the two languages making it easy to parse data back and forth. Taking a cue from JSON(JavaScript Object Notation), when an application resource is exposed via an API and data is transmitted via JSON, any other applications irrespective of the programming language being used can consume the API and utilize its resources. 

*At the core of this approach is a representation problem. How best do we best represent and model our data to enable us to transform it from one form to another while preserving the original information the data represents? What is the most accurate data structure that suits this task? Do existing data structures suits this task and satisfy the constraints? do we need to make modifications to enable existing data structures properly model our data ?*

Take an expression like this below
```javascript
3 + 1 * y;
```
While this looks like a simple expression, a number of factors have to be taken into consideration to effectively parse this expression. Operator precedence rule is followed in almost any modern programming language today when evaluating expressions, thus the multiplication side of this expression is first evaluated then followed by the addition part. 
A common way to represent expressions like this is by using a tree. The expression above can easily be represented as a tree this way. 
```javascript
3 + 1 * y
\   \   /
 \   \ /
  \   *(Multiplication)
   \ /
    +(Addition)
```
This effectively captures the context needed to accurately parse and evaluate this expression. THe source code ofr a prgram is a recursive structure and trees are one of the best data structures for representing recursive objects. Scaling this up we can effectively represent an entire source code as some sort of implicit tree structure. Take the code example below.
```javascript
function myfunc(){
    var y = 4
    var z = 3 + 1 + y
}
```
The parent node here would be a module. Both JavaScript and Python treats each file as a module, thats why its possible to simply import a file as a dependency in Python using import statments and in JavaScript using import, require, etc depending on the module sytem being used(e.g Common JS, ESM, etc). The function declaration is a child node of the module node, the first variable declaration is a child node of the function, the second variable decalaration is a child nod of the function and the expression in Z is a child of the Z variable declaration. Representing this grapically would look something like this.
```javascript
Programe
    |
  body
    \
     \
    function declaration(myfunc) 
           \
            \
          function body
                 /\
               /    \
              /      \
             /        \
            /          \
           /            \
Variable Declaration    Variable Declaration
(name:x, val: int 4)    (name:z) (val: Expression)
                                  3 + 1 * y
                                  \   \   /
                                  \   \ /
                                  \   *(Multiplication)
                                   \ /
                                    +(Addition)

```

### Final Solution: Abtract Syntax Trees ###
Wikipedia defines abstract syntax trees (AST) as a tree representation of the abstract syntactic structure of source code written in a programming language. Each node of the tree denotes a construct occurring in the source code. Abstract Syntax trees are useful data structures for representing the abstract structure of source code irrespective of the language. All modern programming languages have a large similarities in terms of the code structure they express e.g variable declaration/assignment, conditionals, loops, logic branching, etc. Abstact syntax trees is a standadized way for appraoching how we represent sourcecode as a tree data structure. To generate the sourcecode -- original or converted, we simply apply the right kind of traversal on the abstract syntax tree visiting each node and analysing it. There are multiple conventions for ASTs in JavaScript(e.g esprima. estree,ascorn,etc) an accompanying parsers to process the AST. Python has an inbuilt module for working with AST and JavaScript has a several libraries for parsing ASTs. A simple AST in Python would look like this.
```python
import ast 
sourcecode = "3 + 1 * y"
tree = ast.parse(sourcecode)
print(ast.dump(tree))
```
The result tree would look like this
```python
Module(body=[Expr(value=BinOp(left=Num(n=3), op=Add(), right=BinOp(left=Num(n=1), op=Mult(), right=Name(id='y', ctx=Load()))))])
```
We can format this to make it easier to intepret.
```python
Module(
        body=[
              Expr(value=BinOp(
              left=Num(n=3), 
              op=Add(), 
              right=BinOp(
                            left=Num(n=1), 
                            op=Mult(), 
                            right=Name(id='y',ctx=Load()))))]
)

```
The equivalent AST for the same code in JavaScript using Esprima standards would look this this
```javascript
{
  "type": "Program",
  "body": [
    {
      "type": "ExpressionStatement",
      "expression": {
        "type": "BinaryExpression",
        "operator": "+",
        "left": {
          "type": "Literal",
          "value": 3,
          "raw": "3"
        },
        "right": {
          "type": "BinaryExpression",
          "operator": "*",
          "left": {
            "type": "Literal",
            "value": 1,
            "raw": "1"
          },
          "right": {
            "type": "Identifier",
            "name": "y"
          },
        },
    }
  ],
  "sourceType": "module"
}
```
**NB:**  Some properties were removed from the AST for clarity purposes.  

### Why ASTs? ###

* Taking the constraint of near accurate conversion we How we handle accuracy? we can now use a set of predefined syntactic rules on how the AST should be processed and we decide what to do when we encounter certain kinds of node on numerous circumstances. Using the semi colon delimeter is optional in Javascript and the usage of this is entirely based on preference. AST allows us the sophiscation of defining how we approach the source code. Babel a JavaScript tool for transpiling and polyfilling sourcecode takes advantage of this. linters also take advantage of this allowing programmers to enforce code styling and conventions. 

    Take for example below
    
```python
4 + "30"
```
   In Python, this would throw a TypeError while in JavaScript, both values in the expression would be treated as strings      and concatenated together because of implicit type coercion. Some of the somewhat quirky behaviors in JavaScript like        this can be eliminated by running the source in "strict mode" or using TypeScript(a typed superscript of javascript).        Parsing source code this way allows us to make decisions on how we want the AST to be transformed, especially                syntactic and context decisions which we have seen can easily mar the conversion process. 

* AST handles scoping and context limitations of search and replaces by storing meta-data while parsing the sourcecode allows context-specific decisions. This meta data can include the location of a contruct within the sourcecode, etc. 

### Taking Abstract Syntax Trees One Step Further
Having figured out to accurately represent and convert sourcecode.There are many routes to achieve the goal this project  the most obvious/direct being to make the AST, make transform rules and convert the python tree to a JavaScript tree and vice versa. This is the approach used in the code for this project. However, since the overall philisophy behind this project to promote convergence around program languages. This solution can be taken one step further.
**Universal ASTs:** Currently ASTs are very language specific, as seen in the snippet above, AST for the same piece of code varies across both languages and in the case of JavaScript, the AST vaaries depending the parser engine used. 

A modular approach to designing parser based on a plugin system. Programmers can simply 
