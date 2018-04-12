---
title: 正确方法创建一个Python项目（译）
tags:
  - python
permalink: starting-a-python-project-the-right-way
categories:
  - Programming
date: 2014-02-10 00:00:00
---


原文： [Starting A Python Project The Right Way](http://www.jeffknupp.com/blog/2014/02/04/starting-a-python-project-the-right-way/)

译者： [mapleray](https://github.com/mapleray)


如果你像大多数新手Python程序员，你可能能够在你的头脑里构思整个应用程序，但是，当开始编写代码代码时，一个空白编辑器窗口总盯着你脸，你会感到失落和不知所措。在今天的文章中，*我*将讨论当从头开始一个项目时我自己使用的方法。到文章的末尾，你应该对有开发任何应用程序有一个很好的计划。
        
##设置

在写一行代码之前,我做的第一件事是创建一个虚拟环境。什么是虚拟环境呢?这是一个与系统的其余部分完全隔离的Python安装(包括系统默认的Python安装)。为什么这个有用?设想你有两个项目在本地工作。如果使用相同的库(比如`requests`),但第一个使用旧版本(由于其他库依旧版本的`requests`而不能升级),你如何在你的新项目中成功的使用最新版本的`requests`? 答案是虚拟环境。

首先,安装`virtualenvwrapper`(对包virtualenv封装)。添加一行`source /usr/local/bin/virtualenvwrapper.sh` 到你的`.bashrc`文件，然后通过`source 你刚才编辑的文件`来重新加载文件。你现在应该有一个`mkvirtualenv`命令，可以通过tab补全。如果你正在使用Python 3.3 +,同样支持虚拟环境,所以不需要安装其他包。命令 `mkvirtualenv < my_project >` 将创建一个名为`my_project`的新虚拟环境,同时`pip`和`setuptools`也已经安装好了(对于 Python3,`python - m venv < my_project >`然后执行`source < my_project >/bin/activate`就可以了)。

现在，你已经创建好你的虚拟环境了，该初始化你选择的源代码控制工具了。假设它是git（因为，来吧......），这意味着用`git init .`命令。添加一个`.gitignore`文件来忽略python编译文件和`__pycache__`目录也是很有必要的。要做到这一点，创建一个名为`.gitignore`文件，包含以下内容：

    ::text
    *.pyc
    __pycache__


现在，添加一个`README`到你的项目中。即使你是唯一一个能看到代码的，这却对你组织自己的想法是个很好的锻炼。`README`应该描述项目是做什么的，它的要求以及如何使用它。我使用`markdown`语法书写`README`，一方面是Github能自动格式化`README.md`文件，另一方面是我所有的文档全部用的`markdown`写的。

最后，用过命令`git add .gitignore README.md`和`git commit -m "initial commit"`来第一次提交你刚才创建的两个文件。


##结构(骨架)！

我几乎都是以相同的方式开始一个应用：创建一个由包含文档字符串但是没有实现的函数和类组成的应用程序“骨架”。我发现，当强迫自己写一个需要的函数的文档字符串时，如果我没有充分想过这个问题，我将不能写得很简洁。

作为一个示例应用程序，我将使用一个最近在我们会话中辅导客户创建的一个脚本。该脚本的目的是创建一个包含去年最卖座的电影（从IMDB）和字IMDB上与他们相关的关键字的csv文件。

首先，创建一个主文件，作为你的应用程序的进入点。我的取名为`imdb.py`。接下来，复制并粘贴以下代码到你的编辑器（更改适当的文档字符串）：

    :::python
    """Script to gather IMDB keywords from 2013's top grossing movies."""
    import sys

    def main():
        """Main entry point for the script."""
        pass

    if __name__ == '__main__':
        sys.exit(main())


虽然它可能貌不惊人，但这是一个完整功能的Python程序。你可以直接运行它，并得到正确的返回代码（0，但老实说，运行一个空文件也将返回正确的代码）。接下来，我将为我认为我需要的函数或类创建一个主体：

    :::python
    """Script to gather IMDB keywords from 2013's top grossing movies."""
    import sys

    URL = "http://www.imdb.com/search/title?at=0&sort=boxoffice_gross_us,desc&start=1&year=2013,2013"

    def main():
        """Main entry point for the script."""
        pass

    def get_top_grossing_movie_links(url):
        """Return a list of tuples containing the top grossing movies of 2013 and link to their IMDB page."""
        pass

    def get_keywords_for_movie(url):
        """Return a list of keywords associated with *movie*."""
        pass

    if __name__ == '__main__':
        sys.exit(main())


看起来很合理。注意函数都带有参数（比如`get_keywords_for_movie`有`movie_url`参数）。当实现这些时可能很看上去很奇怪。为什么包括这些参数？这其实和预先编写文档字符串是一样的：如果我不知道函数接受什么参数，说明我还没考虑好这个问题。

此时，我很可能提交到`git`,因为我不想丢失我已经完成的一些工作。在这之后，该完成主体了。我总是从`main`开始，因为它是连接其他函数的”枢纽“。下面是 `imdb.py`里`main`的实现：
        
    :::python
    import csv

    def main():
        """Main entry point for the script."""
        movies = get_top_grossing_movie_links(URL)
        with open('output.csv', 'w') as output:
            csvwriter = csv.writer(output)
            for title, url in movies:
            keywords = get_keywords_for_movie('http://www.imdb.com{}keywords/'.format(url))
                csvwriter.writerow([title, keywords])


尽管`get_top_grossing_movie_links`和`get_keywords_for_movie`都还尚未实现，我有足够的了解去使用它们。`main`已经实现了我们在一开始的讨论：得到最卖座的电影，并输出其关键字到csv文件。

现在全部都是有不完整的函数实现的。有趣的是，即使我们知道`get_keywords_for_movie`将在`get_top_grossing_movie_links`之后被调用，但我们可以以我们喜欢的任何顺序去编写。这和你从头开始编写脚本，然后一步一步添加函数完全不同。你必须写完第一个函数才能继续编写第二个函数。从我们可以以任意顺序编写函数表明它们都是松耦合。

让我们先实现`get_keywords_for_movie`：

    :::python
    def get_keywords_for_movie(url):
        """Return a list of keywords associated with *movie*."""
        keywords = []
        response = requests.get(url)
        soup = BeautifulSoup(response.text)
        tables = soup.find_all('table', class_='dataTable')
        table = tables[0]
        return [td.text for tr in table.find_all('tr') for td in tr.find_all('td')]

我们将使用的`requests`和`BeautifulSoup`，所以我们需要先用`pip`安装它们。现在用`pip freeze requirements.txt`列出项目的依赖然后提交。这样一来，我们就总能创建一个虚拟环境，并安装运行我们的应用程序所需要的依赖和相应的版本。

返回的列表解析可能看起来很奇怪，但它只是简单的返回两层嵌套迭代的第一个结果。只要你愿意，你可以用列表解析嵌套更多的语句。

最后一步是`get_top_grossing_movie_links`的实现：

    :::python
    def get_top_grossing_movie_links(url):
        """Return a list of tuples containing the top grossing movies of 2013 and link to their IMDB
        page."""
        response = requests.get(url)
        movies_list = []
        for each_url in BeautifulSoup(response.text).select('.title a[href*="title"]'):
            movie_title = each_url.text 
            if movie_title != 'X':
                movies_list.append((movie_title, each_url['href']))
        return movies_list

相当的简单。`if movie_title != 'X' `是因为我的选择相对宽松。而不是刚好得到正确的结果，我只是使用`if`语句简单的过滤掉伪造的链接。

这是`imdb.py`的全部内容：

    :::python
    """Script to gather IMDB keywords from 2013's top grossing movies."""
    import sys
    import requests
    from bs4 import BeautifulSoup
    import csv

    URL = "http://www.imdb.com/search/title?at=0&sort=boxoffice_gross_us,desc&start=1&year=2013,2013"
    
    def get_top_grossing_movie_links(url):
        """Return a list of tuples containing the top grossing movies of 2013 and link to their IMDB page."""
        response = requests.get(url)
        movies_list = []
        for each_url in BeautifulSoup(response.text).select('.title a[href*="title"]'):
            movie_title = each_url.text 
            if movie_title != 'X':
                movies_list.append((movie_title, each_url['href']))
        return movies_list



    def get_keywords_for_movie(url):
        """Return a list of keywords associated with *movie*."""
        keywords = []
        response = requests.get(url)
        soup = BeautifulSoup(response.text)
        tables = soup.find_all('table', class_='dataTable')
        table = tables[0]
        return [td.text for tr in table.find_all('tr') for td in tr.find_all('td')]
    

    def main():
        """Main entry point for the script."""
        movies = get_top_grossing_movie_links(URL)
        with open('output.csv', 'w') as output:
            csvwriter = csv.writer(output)
            for title, url in movies:
                keywords = get_keywords_for_movie('http://www.imdb.com{}keywords/'.format(url))
                csvwriter.writerow([title, keywords])


    if __name__ == '__main__':
        sys.exit(main())

从开始一个空白的编辑器窗口的应用程序到现在已经完成了。运行后将生成`output.csv`,包含了我们所希望得到的。鉴于这个脚本的长度,我不会写这个脚本的输出测试。然而,它肯定是可能的(因为我们的函数是松耦合的)来测试每个函数都是相对孤立的。

##结束

