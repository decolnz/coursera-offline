# Coursera Offline

[![PyPi version](https://img.shields.io/pypi/v/coursera_offline.svg)](http://pypi.python.org/pypi/coursera_offline)
[![PyPi downloads](https://img.shields.io/pypi/dm/coursera_offline.svg)](http://pypi.python.org/pypi/coursera_offline)

Download and save the video lectures of your favorite courses for offline viewing.

# Changes

- When `-d` option is given and the given directory does not exist, exiting with error on -S option and creating the directory on -s option.
- Extracting and normalizing the titles of files from urls.
- Now supports download of excel files.
- Deprecated -f option in favor of -S

# Contents

* [Installation](#installation)
  * [Proxy Settings](#for-those-behind-proxy)
  * [Requires](#requires)
* [Running](#running)
* [Features](#features)
* [Full Usage](#full-usage)
* [Some sample invocations](#some-sample-invocations)
  * [First time download](#first-time-download)
  * [Obtaining the shortname](#obtaining-the-shortname)
  * [Synching](#synching)
  * [Auto Synch](#auto-synch)
  * [Fetch using file](#fetch-using-file)

## <a name="installation"></a>Installation

* Make sure you have python version 2.7 installed. If you don't have python, get it from [here](https://www.python.org/download/releases/2.7/)
* If you have python and are not sure of the version, type `python -V` in the terminal. If it says 2.7.x+ then you may proceed to the next instruction. Otherwise, go to the link provided above.
* Install `pip` using `sudo apt-get install python-pip`. Install the application using `sudo pip install coursera_offline`
* Before proceeding to the next step, make sure you have `setuptools` module installed. If it isn't, you can find the installation instructions [here](https://pypi.python.org/pypi/setuptools#installation-instructions).
* If you don't want to install pip, you can download the tar.gz from [PyPi](https://pypi.python.org/packages/source/c/coursera_offline/coursera_offline-0.1.0.tar.gz) or zip from [Github](https://github.com/sanketh95/coursera-offline/archive/master.zip), extract the archive file and follow the installation instructions in the README.txt file.
* You may also clone the repo onto your local workstation and follow the instructions in the README.txt file
`git clone https://github.com/sanketh95/coursera-offline`

#### <a name="for-those-behind-proxy"></a>For those behind proxy

You just need to set `HTTP_PROXY` and `HTTPS_PROXY` environment variables and python automatically sends all requests through proxy.
Here's the way to set proxy in windows and linux 

##### Windows
Run `set HTTP_PROXY=http://user:password@address:port` and `set HTTPS_PROXY=https://user:password@address:port`

##### Linux
Run `export HTTP_PROXY=http://user:password@address:port` and `export HTTPS_PROXY=https://user:password@address:port`


### <a name="requires"></a>REQUIRES

* Python2.7
* pyquery 1.2.9
* crontab 1.8.1

**Note:** You need not install the requirements manually, the setup script takes care of installing them for you.

## <a name="running"></a>Running

### Windows
* Open command prompt and change the `cd` into the directory containing **coursera-offline** and run `python coursera_offline -h`

### Linux
* Open terminal and run `coursera_offline -h`.

## <a name="features"></a>Features

* All the videos are downloaded according to the folder structure and you don't need to take care of sorting the videos into separate folders manually.
* You need not track the order of the videos/weeks as the script intentionally renames them so that the week and video order maintained.
* The downloads are lightning fast as the videos are downloaded parallely.
* The script creates a crontab entry automatically to fetch any newly added videos in the course. You can also force synch with Coursera just by running a simple command.
* The script also downloads the subtitles automatically and saves them in *Subs* folder. So when you play the videos using VLC, the subs are automatically loaded.
* The script saves a *data.json* file in the course directory. This has all the information required to fetch the videos. So even if you lose some videos or if you forget the course name, as long as you have the *data.json* file, you can always re-download the lost videos.
* The script saves the session cookies so you don't have to login everytime you run it.
* The script fetches the lecture slides (pdfs and ppts) and saves them in 'Other Files' folder.


## <a name="full-usage"></a>Full Usage
```
usage: coursera_offline.py [-h] [-s SHORTNAME] [-e EMAIL] [-p PASSWORD] [-S]
                           [-d DIR] [-a [AUTO]]

optional arguments:
  -h, --help            show this help message and exit
  -s SHORTNAME, --shortname SHORTNAME
                        Short name of the course being downloaded. This option
                        is required when running the script for the first time
  -e EMAIL, --email EMAIL
                        Email id registered with Coursera
  -p PASSWORD, --password PASSWORD
                        Coursera Password
  -S, --synch           Give this flag to synch with Coursera
  -d DIR, --dir DIR     Give this option to save the videos in the path
                        specified as argument. Defaults to Present Working
                        Directory (PWD).
  -a [AUTO], --auto [AUTO]
                        Give this option to create a crontab entry inorder to
                        automatically synch with Coursera. Argument must one
                        among 'MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT', 'SUN'.
                        The argument is optional and defaults to 'SUN'
```

## <a name="some-sample-invocations"></a>Some sample invocations

**Note:** In order to download some courses, you need to register to the course and accept the honor code in Coursera site before running the script.

Let's say you're downloading a course called **Introduction to Logic** and you want to download it to the `~/Logic`
 directory.


#### <a name="first-time-download"></a>First time download

```
coursera_offline -d ~/Logic -s intrologic-005 -e <email> -p <password>
```

Do this to download the video lectures arranged as per weeks. This creates the `Logic` directory in the home directory (if it doesn't exist) and downloads all the videos into this directory. This also creates a `data.json` file that contains all the information requried to download the videos.

The argument **intrologic-005** is called the **shortname** and is unique for every course. It can be obtained from the class url. For example for the course under discussion, the url is https://class.coursera.org/intrologic-005.


##### <a name="obtaining-the-shortname"></a>Obtaining the shortname

* Signin to Coursera from [here](https://accounts.coursera.org/signin).
* You'll be redirected to a page containing all your registered courses.
* Choose the course you want to download and click the **Go to class** button.
* You'll be redirected to the class page whose url looks like 
`https://class.coursera.org/<short name>`.
* Copy the short name.

**Note:** If the `-d` options is not given, the videos will be downloaded to the current working directory.


#### <a name="synching"></a>Synching

```
coursera_offline -d ~/Logic -S -e <email> -p <password>
```

Okay, so you've been a few weeks into the course, now u need to fetch the updated videos. Don't worry, there's a way to do it !
The above command takes care of fetching the updated video content and synching it with your local directories.


#### <a name="auto-synch"></a>Auto Synch

```
coursera_offline -a <Day of the week> -s intrologic-005 -e <email> -p <password>
```

You're downloading the course for the first time, and you don't want to keep using the 'Synch' command whenever you want to update the video content, you can just pass the `-a` flag which creates a crontab entry for the Synch command whch will be run at 11:59:59 pm on that particular day of the week. If no day is specified, it is defaulted to **Sunday**. 

**Note:** You can still force the script to synch with Coursera by using the Synch command described above.