<h1>XML external entity (XXE) injection</h1> 

<h2>What is XML?</h2>
XML stands for eXtensible Markup Language. 

What does it do?
Well.......It doesn't really do anything. It is just used to store and share data. The data is stored in a text format which makes it easier to store and share.

XML entities are a way of representing an item within an XML document. You can use inbuilt entities of XML language or you can define your own. Think of it like a variable that you refer to instead of the data that the variable contains. 
The start of the XML document contains a document type definiation (DTD), which defines what type of document it is. DTD is often declared with the element <i>DOCTYPE</i>. The document type definition can refer to data to some internal entity which is data in the document itself or it can refer to some external entity which is data that can be loaded from some other location.

XML external entities would look something like this
{% highlight HTML %}
<!DOCTYPE foo [ <!ENTITY ext SYSTEM "file:///path/to/file" > ]>
{% end highlight %}

#sample XML code here

What is XML external entity injection?




<h2>Exploiting XXE using externa entities to retrieve files</h2>


<h2>Exploiting XXE to perform SSRF attacks<h2>

  <!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://internal.vulnerable-website.com/"> ]>
  
 

<h2>Blind XXE vulnerabilities</h2>
Blind XXE injection occours when the attacker has no way to analyzing the output/response of the server. Sometimes the server does not return back any useful response. So in order to find  












Resources: 
https://portswigger.net/web-security/xxe
