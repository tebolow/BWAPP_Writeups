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
<img src="Screenshot 2023-04-05 054623.png">
Now let's open the source code of the page to see how it works.<br>
We can see that the first bracket < is considered as the closing bracket for <b> a tag </b>.<br>
<img src="Screenshot 2023-04-05 054719.png">
So we can modify our payload to be <em><b>><script> alert("Attention ... Cyberus") </script></b></em> instead of <em><b><script> alert("Attention ... Cyberus") </script></b></em>
<img src="Screenshot 2023-04-05 054831.png">

<h2>A3 Cross-Site-Scripting Stored (SQLiteManager)</h2>
At first, When we open the page it's going to display 3 links. Let's try to open the first one.<br>
<img src="Screenshot 2023-04-08 020237.png">
Now we are going to try to put our payload in the first input field. Oops it appears that every field must be filled.<br>
<img src="Screenshot 2023-04-08 020503.png">
After filling all fields our payload and a white space, let's try agian.<br>
Well done it worked. Now this alert will show up every time we visit this link.
<img src="Screenshot 2023-04-08 020531.png">

<h2>A3 SQL Injection (POST/Select)</h2>
In this challenge when you open you will find a combo-box to choose the movie you want, then after pressing "go" the website will display all the movie's data.<br>
<img src="Screenshot 2023-04-08 040313.png">
There are no passed parameters in the URL and no input fields. So, we are going to use burp suit to analyse the request being sent.<br>
<img src="Screenshot 2023-04-08 040340.png">
We find that the website selects the movie depending on a number. let's try to mess it up by adding <b> ' </b>.<br>
It now shows us the syntax error of the database and that's how we know that the website is vulnerable to SQL injection.<br>
<img src="Screenshot 2023-04-08 040405.png">
Now you can try multiple payloads.

<h2>A3 SQL Injection (Captcha)</h2>
When openning this challenge we are going to find an input field to fill a captcha. let's try to test it. It doesn't fill any URL parameters or reflect what we typed.<br>
<img src="Screenshot 2023-04-08 043933.png">
So, let's fill it correctly and see what happens.<br>
It will open a second page in which we can search for movies. I can notice that whatever we type is affecting the URL "title" parameter.<br>
<img src="Screenshot 2023-04-08 044349.png">
Let's try to add <b> ' </b> in a try to corrupt the database. Guess what? it worked and the syntax error has shown up. <br>
<img src="Screenshot 2023-04-08 044420.png">
Now you can try different payloads. The one I tried was <b><em> test ' union select 1,2,3,4,5,6,7 from users # </em></b>

<h2>A3 SQL Injection (SQLite)</h2>
OK, let's try to test that "search for a movie" input field. It does nothing. but as usual the title URL parameter is changing.<br>
Now try to add <b> ' </b>. It returns an error HY000. and if we search for it we get.
<img src="Screenshot 2023-04-08 050938.png">
Which refers to a missing of semicolon right here.<br>
That means that it's vulnerable to SQLi. so let's try to use our payload <b> test ' union select null, name, null, null, null, null from sqlite_master where type='table';</b> this payload is expected to get you the names of the tables stored. And now you can do your own payloads.
<img src="Screenshot 2023-04-08 052158.png">


<h2>A3 SQL Injection Stored Blind Time-based</h2>
In this section there is a single input field that does NOT reflect anything. But, the URL parameter (title) is taking whatever we type.<br>
So, let's try to test it. We are going to type in "test <b>'</b>" in order to corrupt the query and see what happens. Nothing? dissapointing. But don't worry<br>
Ok, Now, let's try something a little diiferent. let's try <b>sleep()</b> to check if it was vulnerable to SQLi or not. let's try this payload <b> test ' or 1=1 and sleep(1) </b> we can find that the page took too much time to take an action than usual which means that our payload was executed successfully. 
