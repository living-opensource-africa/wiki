---
title: Contrbuting to LOS wiki
# summary: 
date: 2021-09-07
authors:
  - Muse Sisay
---

We are glad that you have decided to contribute to **LOS Africa Wiki**.

!!! warning 
    This is just an outline


1. Fork the repo on github
2. Clone the repo on local machine
3. check out a branch
4. Work on the branch
5. Commit your works
6. push your work github
7. create a pull request


---
## Setting up your Enviroment 

In this section I will be covering how to install `mkdocs`, the wiki app. (# I will be using python virtual enviroment )

1. Install the latest version of `python3` and `pip`.
```console
sudo apt install python3 python3-pip 
```

2. Create a virtual enviroment.
```bash
mkdir ~/python-virtual-environments && cd ~/python-virtual-environments
```
Let's create a new python virtual enviroment and call it mkdocs 
```bash
python3 -m venv mkdocs
```

3. Activate your python env and install `mkdocs-material`.
```bash
source ~/python-virtual-environments/mkdocs/bin/activate
```
If you are using fish shell the file is `activate.fish` instead of `activate`
```bash
python3 -m pip install mkdocs-material
```
Every time you want to work with mkdocs you need to activate the virtual env.
 
Read more on python virtual enviroments [here](https://realpython.com/python-virtual-environments-a-primer/)

--- 

- section: ssh keypair

- section : Writing is based on markdown 
    - Write new document
    - Metadata
    - setting up vscode
