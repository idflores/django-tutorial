<h1 align="center">Django Tutorial</h1>

### My personal repository to learn Django from [DjangoProject.com](https://docs.djangoproject.com/en/1.11/intro/)

<br>

<h2 align="center">Using <code>virtualenv</code></h2>

(note: used as a "container" environment for the project)

### Install

```bash
sudo pip install virtualenv
```

### Start a new `virtualenv` "container"

```bash
virtualenv directory/name
```

To use Python3 instead:

```bash
virtualenv -p Python3 directory/name
```

### Usage

#### activate

```bash
source bin/activate
```

#### deactivate

```bash
deactivate
```

#### example

notice how, while activated, python and pip point to the packages defined inside the environment:

![](./readme-assets/virtualenv-activate-example.png)


<h2 align="center">Install Django</h2>

With `virtualenv` [activated](#usage), run the following command:

```bash
pip install Django
```

#### example

![](./readme-assets/install-django-example.png)


<h2 align="center">The Django Project Tutorial</h2>

Tutorial Reference: [https://docs.djangoproject.com/en/1.11/intro/tutorial01/](https://docs.djangoproject.com/en/1.11/intro/tutorial01/)
