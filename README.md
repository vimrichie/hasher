# Storing user passwords in plain text?

Keeping your passwords secure is important. Even more if you are storing passwords for others. You won't believe how
many companies out there have stored user information in plain text. I'll point you to some articles and see if you remember
any of these.


[Krebs on Security - Facebook Stored Hundreds of Millions of User Passwords in Plain Text for Years
](https://krebsonsecurity.com/2019/03/facebook-stored-hundreds-of-millions-of-user-passwords-in-plain-text-for-years/)

[RockYou2021 - largest password compilation of all time leaked online with 8.4 billion entries
](https://cybernews.com/security/rockyou2021-alltime-largest-password-compilation-leaked/)

[SolarWinds hack: the mystery of one of the biggest cyberattacks ever
](https://cybernews.com/security/solarwinds-hack-the-mystery-of-one-of-the-biggest-cyberattacks-ever/)

I believe when a new app or website is created, security should be one of the first things that needs
to be implemented. I'm going to show you a simple way of hashing passwords using Python. We will use SHA-2(512) to create
a long string out of the password. 

### Requirements
- Python 3. 
- Passlib 
  - https://passlib.readthedocs.io/en/stable/

### TLDR
![alt text](https://github.com/applericky/hasher/blob/main/images/codeisnittygritty.jpeg?raw=true)

## Let's code!

I'm going to write out some code and explain what is happening.
I'll start off by importing our two libraries Passlib and Getpass.
```python
1 |  from passlib.handlers.sha2_crypt import sha512_crypt as hasher
2 |  from getpass import getpass
```

So what do we have here? 

Line 1: Passlib is imported and from the ```handlers.sha2_crypt``` module we import ```sha512_crypt```
as ```hasher```.   

Line 2: From the library ```getpass```  we import ```getpass```.


Now let's create our function that will ask for the username and password. In this function we can also 
include the hash method.

```python
5 | def account_creation():
6 |     username = input("Please enter a username: ")
7 |     password = getpass("Please enter a password: ")
8 |     hashed_password = hasher.hash(password, rounds=200_000)
9 |     database_create(username, hashed_password)
```

What just happened?

Line 5: we defined our function and we named it ```account_creation()```

Line 6: asks the user for the username and sets it as a variable named ```username```.

Line 7: prompts the user for the password and sets it as variable named ```password```.

Line 8: hashes our ```password``` using ```hasher.hash``` and sets the rounds to 200 thousand. By default passlib
iterates the hash 29,000 times but we went above and beyond with 200k!

Line 9: we call a function named ```database_create()``` and give it two parameters named ```username``` and ```password```.


Let's create our ```database_create()``` definition that we just passed the username and password through. 

```python
12 | def database_create(username, password):
13 |    file = open(f"{username}.txt", "w")
14 |    file.write(password)
15 |    file.close()
16 |    print("Account created.")
```

Line 12: we define our function ```database_create()``` and pass the ```username``` and ```password``` arguments.

Line 13: we create a variable named ```file``` and assign the Python built in function ```open()```. 
This built in function is used to open or create files. Inside the ```open()``` function we pass an f-string with the 
argument ```username```. As you can see that it is creating a .txt file with the ```username``` that is provided.

Line 14: Our variable ```file``` is used to write the password using the ```write()``` function.

Line 15: We use the ```close()``` function to make sure code and the OS has closed the session.

Line 16 And we print something out to confirm our code completed.

Finally we add our main method. This makes us 10x developers!
```python
19 | if __name__ == "__main__":
20 |     account_creation()
```


And that's it! 

![alt text](https://github.com/applericky/hasher/blob/main/images/what%3F.jpeg?raw=true)

Yep, it is that easy!

Let's test the code in our terminal
```shell
➜ python3 main.py
Please enter a username: elmo
Please enter a password:
Account created.
````
I entered the username ```elmo``` and the password ```elmoloveshisgoldfishandcrayon123```.

Now we check to see if the file was created using ls.


```shell
➜ ls 
elmo.txt main.py
````
As you can see there is a file named ```elmo.txt```. I'll cat the file to check the contents.

```shell
cat elmo.txt
$6$rounds=200000$gYlAWMks4mttsMxS$xDjy0WP5d/zTL0hF/Y.ATqv1J5Yrji4pyerh6znnie01qKdMmAXrzqg7FV.9XtKHiGN/5p3aHsglCg2mA3kVY0%
```

Whoa. Check out that long hashed password!

Want to learn more about SHA-2?

https://en.wikipedia.org/wiki/SHA-2




# The Code

```python
from passlib.handlers.sha2_crypt import sha512_crypt as hasher
from getpass import getpass


def database_create(username, password):
    file = open(f"{username}.txt", "w")
    file.write(password)
    file.close()
    print("Account created.")


def account_creation():
    username = input("Please enter a username: ")
    password = getpass("Please enter a password: ")
    hashed_password = hasher.hash(password, rounds=200_000)
    database_create(username, hashed_password)


if __name__ == "__main__":
    account_creation()
```