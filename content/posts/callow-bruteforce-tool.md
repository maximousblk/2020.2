---
date: 2020-03-20
title: Brute-forcing websites with Callow
Cover: covers/callow-bruteforce-tool.png
tags:
  - python
  - security
---

[Callow](https://callow.now.sh/) makes it stupidly simple to brute-force website login pages. It has been made with beginners in mind and is super intuitive. Callow is available free of charge under the [GPL-3.0 license](https://www.gnu.org/licenses/gpl-3.0.en.html) and can be used for both, commercial and non-commercial purposes.

# What is a brute force attack?

A brute force attack is a trial-and-error method of trying many passwords to discover the correct one. More targeted brute-force attacks use a list of common passwords to speed this up.

Brute force attacks are relatively simple to perform and, given enough time and the lack of mitigation strategy of the target, they always work. Every password based security system can be cracked by brute force attacks.

# Where Callow enters the game

Brute force attacks can be fairly easy for services like SSH or Telnet but for something like a website login page, we need much more information to execute the attack. Callow simplifies this process to a point that even beginners can do it. Callow automates a Chrome instance to host a brute force attack against the login form of (nearly) any website with a visible login forum.

Upon launching Callow, it asks for what site you want to brute-force, it will check to see if the page exists and is accessible. If it is, Callow asks for the CSS selectors for the username and password fields, so that it knows where to enter the username and password. Then it requests the target username and a list of passwords to try during the attack.

After Callow has the information it needs, it will open a browser window and begin automating the attack. In the terminal, you can watch each password attempt as it progresses down the list.

# Installation

Before installing Callow, you’ll need to install a few requirements, including a driver, to be able to interact with Chrome.

#### Requirements

First, we’ll need to install a few things for this to work.

- Python 3.5+

- Google Chrome

- [Chrome Driver](https://chromedriver.chromium.org/)

**Install Python**

Python is an ideal language for automating these kinds of attacks. It also makes it easier for beginners to understand how it works.

You’ll need to install Python version 3.5 or above. You can download Python from [python.org/downloads](https://www.python.org/downloads/).

To check if Python is installed correctly, open up a terminal and type _python -V_ (Uppercase V).

```sh
python -V
```

![python version](/images/callow-bruteforce-tool/python-version.png)

**Download Callow**

To install Callow, you have to clone the [GitHub repo](https://github.com/maximousblk/callow/). It is frequently updated and bug fixes are easy to apply. You can also download the zipped files from the [releases](https://github.com/maximousblk/callow/releases/) if you don’t want to keep the full source code. To clone the repository, run the `git clone` command.

```sh
git clone https://github.com/maximousblk/callow.git
```

![git clone callow repo](/images/callow-bruteforce-tool/git-clone.png)

**Install Dependencies**

Fire up a terminal and change the directory to cloned repo.

```sh
cd callow
```

![change directory to callow](/images/callow-bruteforce-tool/cd-callow.png)

Install the python modules required for Callow to work. You’ll need:

- selenium

- requests

You can install them using the following command.

```sh
python -m pip install --user -r requirements.txt
```

![install dependencies](/images/callow-bruteforce-tool/pip-install.png)

**Install Chrome Driver**

Next, you need to install the driver that allows us to control Chrome from the Python program. You need to get the version corresponding to the version of Google Chrome that you currently have. Callow currently ships with the Chrome Driver binaries for Chrome version 80, which should _(in theory)_ work with any future versions of Chrome too. You can download Chrome Driver from [chromedriver.chromium.org/downloads](http://chromedriver.chromium.org/downloads/). Then put `chromedriver.exe` in the installation folder or a location that is in your `PATH` system variable.

# Getting started

Now that we have Callow and all its dependencies installed on our system, it’s time to check out how it works. You can test this safely on our sandbox so that no one gets in trouble. Head over to [callow.now.sh/sandbox](https://callow.now.sh/sandbox/) to follow along.

![login page](/images/callow-bruteforce-tool/login-page.png)

**Identifying the login form elements**

We need some more information about the page in order to do this. Run Callow using the following command. Make sure you are in the same directory where Callow is installed.

```sh
python callow.py
```

![run callow](/images/callow-bruteforce-tool/run-callow.png)

Enter the URL for the target website’s login page into the first prompt. It will check to make sure the website exists and can be accessed. Make sure you have `http://` or `https://` in the URL.

![check website status](/images/callow-bruteforce-tool/check-website.png)

Next, we’ll need to enter the CSS selectors of the login and password input elements of the target website. On the login page, right-click on the _username_ field, then click on _inspect_.

![inspect input feild](/images/callow-bruteforce-tool/inspect.png)

Next, right click on the element’s code, and a menu will appear. Make sure it is an `<input>` tag. Now click on _Copy > Copy selector_ to copy the CSS selector of the user input field, which Callow will use to interact with this element.

![copy css selector](/images/callow-bruteforce-tool/copy-selector.png)

Enter the username selector into Callow, and then repeat the process with the password input field.

```txt
#username
#password
```

![enter css selectors](/images/callow-bruteforce-tool/enter-selector.png)

Now that we have the elements selected, we need to set the username that we’re trying to brute-force. In this case, we will use the username for sandbox.

```txt
testuser
```

![enter target username](/images/callow-bruteforce-tool/target-username.png)

The final step will be to select the password list. This can be any plaintext file with one password in each line. You can use any wordlist that you think can have the password. You can find many great password lists in the [berzerk0/probable-wordlists](https://github.com/berzerk0/Probable-Wordlists/) Github repository. After downloading a password list of your choice, you can add it to the installation directory, and select it instead of the default list. Callow comes with a tiny example list which contains the password for the sandbox which we will use here.

```txt
pass.txt
```

![enter dictionary location](/images/callow-bruteforce-tool/dictionary-file.png)

Press _Enter_, and a new Google Chrome window will open and begin trying the passwords in the dictionary.

![start attack](/images/callow-bruteforce-tool/start-attack.png)

Let this run for as long as it takes and eventually, if the list has the password, it will find the correct password. Once Callow detects a successful login, it will output the password that succeeded, close the chrome window and exit. That’s how simple it is.

You can also pass those options in the form of arguments. Let’s look at the help menu using the following command. You can see the options for Callow here.

```sh
python callow.py -h
```

![help menu](/images/callow-bruteforce-tool/help-menu.png)

You can do all the previous steps in just one line and it will start the attack right away. you can do it as such:

```sh
py callow.py --site=https://callow.now.sh/sandbox --usel=#username --psel=#password --user=testuser --pass=pass.txt
```

![](/images/callow-bruteforce-tool/one-liner.png)

# Where this all goes down

Websites have the best ability to defend against these attacks by making sure to implement common safeguards for dictionary and other types of attacks. Some websites have hidden login forms that require you to scroll or click to show the form.

Many services now do some kind of rate-limiting, which detects too many failed login attempts and blocks further attempts for a period of time, which can substantially slow down a brute force attack.

The biggest downside to a dictionary attack is that if the password does not exist in the dictionary, the attack will fail. If the password used by the target is strong, brute-force attacks can be too time and resource expensive to use as we start having to try every possible combination of characters.

# Disclaimer

The author of this article, under any circumstances, does not support or take responsibility for any form of unethical acts. This article is purely for educational purposes and is not intended to cause any harm.
