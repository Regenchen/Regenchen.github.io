---
layout: post
title: CI with Travis and Coveralls
tags: Dev
---
## Continuous integration

Continuous integration(CI) is the practice to efficiently merge developer working copies with a shared mainline. Automated CI is a must in the industry nowadays, and it's also free and ready-to-go without any sophisticated configurations.

## Travis-ci

[Travis-ci](travis-ci.org) is a CI platform to build packages on-the-fly. `.travis.yml` is required in the repository to trigger builds on Travis, provided that you have already registered on the website.

A sample `.travis.yml` for python packages is as below:

```
  language: python
  python:
    - "2.7"
    - "3.4"
    - "3.5"
    - "3.6"
  branches:
    only:
      - master
  install:
    - pip install coveralls
    - pip install coverage
  script:
    - python setup.py install
    - coverage run test.py -v
  after_success:
    - coveralls
```

Here you need to specify the python version and branches you want to build. Travis will execute code brackets in the order of `install`, `script` and `after_success`.

If your package works, Travis will build it and give it a `passing` badge.

![travis](https://raw.githubusercontent.com/Jiaxigu/Jiaxigu.github.io/master/assets/images/2018-01-30-travis.jpeg){: .center-image}


## Coveralls.io

As you can see in the `.travis.yml`, [Coveralls.io](https://coveralls.io/) is included for [code coverage analysis](https://en.wikipedia.org/wiki/Code_coverage).

You don't even have to run the code coverage analysis by yourself; Travis will execute the code and submit automatically the coverage report to Coveralls. If everything works it should look like this:

![coveralls](https://raw.githubusercontent.com/Jiaxigu/Jiaxigu.github.io/master/assets/images/2018-01-30-coveralls.jpeg){: .center-image}

However, if you insist on submitting coverage report by yourself (or from a private CI), you need to do this:

- Get repository token from Coveralls (as SECRET_TOKEN)
- add `.coveralls.yml` to repository. The file should include the SECRET_TOKEN:

	```
		repo_token: SECRET_TOKEN
	```
- generate report with the following code (provided that all packages have been installed):

	```
		coverage run test.py -v
		coveralls
	```

The reports should have been submitted to Coveralls by now. It's a good practice to add `.coveralls.yml` to `.gitignore` since 1) the YML file is only usefull locally and 2) you don't want anyone to see your SECRET_TOKEN...