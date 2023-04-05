# BWAPP_Writeups

<h1>What is bWAPP?</h1>
  bWAPP, or a buggy web application, is a free and open source deliberately insecure web application. bWAPP helps security enthusiasts, developers and students to discover and to prevent web vulnerabilities. We are going to go through 8 vulnerabilites along this writeup.

<h2>A3 Cross-Site-Scripting Reflected (Post)</h2>
In this section we are going to try to exploit the vulnerability of XSS Post.<br>
<b>First step:</b> we are going to try the fields. We find that the website writes <em>welcome + "what we entered in the two fields"</em>.<br>
<img src="Screenshot%202023-04-05%20054239.png">
Now we are going to try to deploy a payload that shows an alert of "Attention ... Cyberus". after placing our payload we find that it does NOT work as we left the second field empty.<br>
Our payload is going to be <em><b><script> alert("Attention ... Cyberus") </script></b></em>, and we will put a white space in the second field. Now, It's working.
<img src="Screenshot%202023-04-05%20054239.png">

<h2>A3 Cross-Site-Scripting Reflected (HREF)</h2>
In this section the website is asking us to enter the name in order to vote for a certain movie. We are going to enter a name then press go to see what is going to happen.<br>
Amazing! our name reflected in the page. that means we can do as we did in the previous task.<br>
<img src="Screenshot 2023-04-05 054447.png">
Now, let's enter the usual payload <em><b><script> alert("Attention ... Cyberus") </script></b></em> and see what happens. Oops, It does NOT do anything. but there is something weird, The payload is passed instead of <b>vote</b> word.<br>
Now let's open the source code of the page to see how it works.<br>
We can see that the first bracket < is considered as the closing bracket for <b> a tag </b>.<br>
So we can modify our payload to be <em><b>><script> alert("Attention ... Cyberus") </script></b></em> instead of <em><b><script> alert("Attention ... Cyberus") </script></b></em>

<h2>A3 Cross-Site-Scripting Reflected (Post)</h2>
