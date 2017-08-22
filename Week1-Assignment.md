# Week 1 Assignment #<br>
## 1. Data Types Of ES6 ##<br>
　　In JavaScript, there are six data types including undefined, null, Boolean, number, string and object. Among these types, object is the complex data type and the other five are basic data type. 
<br>
　　In ES6, there are some newly-added built-in type. Symbol is one of the new basic data types. ES6 introduces Symbol to represent a unique value in order to prevent naming colliding. let s=Symbol(); Symbol can be used to name attributes or constants.
<br>　　ES6 also provides some new data structure. Set is one of them. It is similar to Array, but there is no repeated value in it. const s=new Set().
Map is the set of key-value structure, which is similar to object. The difference is that the type of key is not limited to string. It can be all kinds of value, even an object.
<br>　　However, from my point of view, set and map are just new data structures in strict definition. They are not basic data types.<br>
## 2. Some Common Type Conversion ##<br>
### 1. Number-String ###<br>
　　There are three functions which can convert other data types to number:<br>
　　Number(),Number.parseInt() and Number.parseFloat(). Among these functions, Number() can convert any data type into Number. parseInt() and parseFloat() apply to string especially.<br>
　　To convert a number to string:<br>
　　1. call toString(). This method applies to number, boolean and string.<br>
　　2. String(). This method applies to all the data types.<br>
　　3. If we want to convert a number to a string, we can connect it with a string by + operator. str=''+value;<br>
### 2.Any type to Boolean ###<br>
　　To convert some certain type
 to Boolean, wen can call Boolean(),for example:<br>
　　　let message=”hello world”；<br>
　　　let meesageasBoolean=Boolean(message);<br>
　　　The conversion rule is showed as follows:<br>
　　　　1.    Any non-empty string is converted as true,’’ is false;<br>
　　　　2.	Any number which is not 0 is converted as true, 0 and NaN are false;<br>
　　　　3.	Any object is converted as true<br>
　　　　4.	Null is converted as false;<br>
　　　　5.	Undefined is converted as false;<br>
## 3.Detection Methods Of Data Types ##<br>
### 1. typeof ### <br>
　　typeof is the most common method to detect data types. It can be used to identify string, number, boolean and object.<br>
However,typeof operator has some shortcomings:<br>
　　1.When we apply typeof to null, it returns object.<br>
　　2.When we detect the reference type, it can only return object. But in practical, we wish to know which type of object it is.
typeof cannot realize this.<br>
### 2. instanceof ###<br>
　　instanceof is usually used to detect the type of some certain object. For example:<br>
　　　　typeof new Date() returns object<br>
　　　　var date = new Date();<br>
　　　　date instanceof Date;//true<br>
shortcomings:<br>
1. instanceof cannot detect the basic data types;<br>
2. When we use the same constructor function in two different frames, instanceof will return false when detecting the same function.<br>
### 3. Array.isArray ###<br>
Assuming that there are two or more frames in one window, 
window.onload=function(){
let iframe_arr=new window.frames[0].Array;
alert(iframe_arr instanceof Array); // false
alert(iframe_arr.constructor == Array); // false
}
This occurs because the array instances in diffenrent frames don't share the same prototype.

