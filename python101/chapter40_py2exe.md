---
title: 40. py2exe
description: Python 101
toc: true
authors:
tags:
categories:
series:
featuredImage:
date: "2022-06-28"
lastmod: "2022-06-28"
draft: false
weight: 40
---

Проект py2exe раньше был основным способом создания исполняемых файлов Windows из ваших приложений Python. На [PyPI](https://pypi.python.org/pypi/py2exe/) лежит версия, которая будет работать и с Python 2 и 3.

У вас есть несколько вариантов для приложения. Вы можете создать программу, которая будет работать только в терминале, вы можете создать графический интерфейс пользователя (GUI) для рабочего стола или создать веб-приложение. Мы создадим очень простой настольный интерфейс, который ничего не делает, кроме отображения формы, которую пользователь может заполнить. Мы будем использовать инструментарий wxPython GUI, чтобы продемонстрировать, как py2exe может подбирать пакеты без нашего указания.

## Создание простого графического интерфейса

Вам нужно перейти на сайт wxPython (www.wxpython.org) и загрузить копию, соответствующую вашей версии Python. Если у вас 32-битный Python, убедитесь, что вы скачали 32-битный wxPython. Вы не сможете использовать easy_install или pip для установки wxPython, если только вы не получите самую современную версию wxPython от Phoenix, поэтому вам придется взять копию, предварительно собранную для вашей системы, либо с сайта wxPython, либо из менеджера пакетов вашей системы. Я рекомендую использовать по крайней мере wxPython 2.9 или выше.

Давайте напишем немного кода!

```python
import wx

class DemoPanel(wx.Panel):
    """"""

    def __init__(self, parent):
        """Constructor"""
        wx.Panel.__init__(self, parent)

        labels = ["Name", "Address", "City", "State", "Zip",
                  "Phone", "Email", "Notes"]

        mainSizer = wx.BoxSizer(wx.VERTICAL)
        lbl = wx.StaticText(self, label="Please enter your information here:")
        lbl.SetFont(wx.Font(12, wx.SWISS, wx.NORMAL, wx.BOLD))
        mainSizer.Add(lbl, 0, wx.ALL, 5)
        for lbl in labels:
            sizer = self.buildControls(lbl)
            mainSizer.Add(sizer, 1, wx.EXPAND)
        self.SetSizer(mainSizer)
        mainSizer.Layout()

    def buildControls(self, label):
        """
        Put the widgets together
        """
        sizer = wx.BoxSizer(wx.HORIZONTAL)
        size = (80,40)
        font = wx.Font(12, wx.SWISS, wx.NORMAL, wx.BOLD)

        lbl = wx.StaticText(self, label=label, size=size)
        lbl.SetFont(font)
        sizer.Add(lbl, 0, wx.ALL|wx.CENTER, 5)
        if label != "Notes":
            txt = wx.TextCtrl(self, name=label)
        else:
            txt = wx.TextCtrl(self, style=wx.TE_MULTILINE, name=label)
        sizer.Add(txt, 1, wx.ALL, 5)
        return sizer

class DemoFrame(wx.Frame):
    """
    Frame that holds all other widgets
    """

    def __init__(self):
        """Constructor"""
        wx.Frame.__init__(self, None, wx.ID_ANY,
                          "Py2Exe Tutorial",
                          size=(600,400)
                          )
        panel = DemoPanel(self)
        self.Show()

if __name__ == "__main__":
    app = wx.App(False)
    frame = DemoFrame()
    app.MainLoop()
```

Если вы выполните приведенный выше код, вы должны увидеть что-то вроде следующего:

![](../img/py2exe_wx.jpg)

Давайте немного разложим это по полочкам. Мы создаем два класса, **DemoPanel** и **DemoFrame**. В **wxPython** объект wx.Frame используется для создания реального "окна", которое вы видите в большинстве случаев. Вы добавляете **wx.Panel**, чтобы придать вашему приложению соответствующий вид и ощущение, а также добавить табуляцию между полями. Родителем объекта панели является фрейм. Фрейм, будучи виджетом верхнего уровня, не имеет родителя. Панель содержит все остальные виджеты в этом примере. Для компоновки виджетов мы используем сайзеры. Сайзеры позволяют разработчику создавать виджеты, размер которых будет изменяться соответствующим образом при изменении размера самого окна. Вы также можете разместить виджеты на панели с помощью абсолютного позиционирования, что не рекомендуется. В конце мы вызываем метод **MainLoop** объекта **wx.App**, чтобы запустить цикл событий, который позволяет wxPython реагировать на события мыши и клавиатуры (такие как щелчок, ввод текста и т.д.).

Теперь мы готовы узнать, как упаковать это приложение в исполняемый файл!

## Файл py2exe setup.py

Ключевым элементом любого скрипта py2exe является файл **setup.py**. Этот файл определяет, что будет включено или исключено, как сильно мы будем сжимать и упаковывать, и многое другое! Вот простейшая установка, которую мы можем использовать с приведенным выше скриптом wx:

```python
from distutils.core import setup
import py2exe

setup(windows=['sampleApp.py'])
```

Как вы видите, мы импортируем метод **setup** из **distutils.core**, а затем импортируем **py2exe**. Далее мы вызываем setup с параметром ключевого слова **windows** и передаем ему имя главного файла внутри объекта python list. Если бы вы создавали проект без графического интерфейса, то вместо **windows** вы бы использовали клавишу **console**. Чтобы запустить этот фрагмент, сохраните его в той же папке, что и ваш скрипт wxPython, откройте командную строку и перейдите в то место, где вы сохранили эти два файла. Затем введите **python setup.py py2exe**, чтобы запустить его. Если все идет хорошо, вы увидите много вывода, заканчивающегося примерно так:

![](../img/py2exe_output.jpg)

Если вы используете Python 2.6, вы можете получить ошибку **MSVCP90.dll** не найден. Если вы увидите эту ошибку, вам, вероятно, придется найти **Microsoft Visual C++ 2008 Redistributable Package** и установить его, чтобы DLL стала доступна в вашей системе. Иногда бывает так, что вы создаете исполняемый файл, а затем, когда вы его запускаете, он просто не загружается правильно. Обычно при этом создается файл журнала, который можно использовать для выяснения причины. Я также нашел инструмент под названием **Dependency Walker**, который можно запустить против вашего исполняемого файла, и он может рассказать вам о недостающих элементах, не относящихся к Python (например, DLL и т.д.).

Я хотел бы отметить, что файл **setup.py** не включает wxPython в явном виде. Это означает, что py2exe был достаточно умен, чтобы включить пакет wxPython автоматически. Давайте потратим немного времени, чтобы узнать немного больше о включении и исключении пакетов.

## Создание расширенного файла setup.py

Давайте посмотрим, какие еще возможности дает нам py2exe для создания двоичных файлов, создав более сложный файл **setup.py**.

```python
from distutils.core import setup
import py2exe

includes = []
excludes = ['_gtkagg', '_tkagg', 'bsddb', 'curses', 'email', 'pywin.debugger',
            'pywin.debugger.dbgcon', 'pywin.dialogs', 'tcl',
            'Tkconstants', 'Tkinter']
packages = []
dll_excludes = ['libgdk-win32-2.0-0.dll', 'libgobject-2.0-0.dll', 'tcl84.dll',
                'tk84.dll']

setup(
    options = {"py2exe": {"compressed": 2,
                          "optimize": 2,
                          "includes": includes,
                          "excludes": excludes,
                          "packages": packages,
                          "dll_excludes": dll_excludes,
                          "bundle_files": 3,
                          "dist_dir": "dist",
                          "xref": False,
                          "skip_archive": False,
                          "ascii": False,
                          "custom_boot_script": '',
                         }
              },
    windows=['sampleApp.py']
    )
```
Это довольно понятно, но все же давайте разберемся. Сначала мы создадим несколько списков, которые мы передадим в параметр options функции setup.

-  Список **includes** предназначен для специальных модулей, которые вам нужно включить. Иногда py2exe не может найти определенные модули, поэтому их нужно указать вручную.
-  Список **excludes** - это список модулей, которые нужно исключить из вашей программы. В данном случае нам не нужен Tkinter, так как мы используем wxPython. Этот список исключений GUI2Exe будет исключать по умолчанию.
-  Список **packages** - это список конкретных пакетов для включения. Опять же, иногда py2exe просто не может что-то найти. Мне уже приходилось включать сюда email, PyCrypto или lxml. Обратите внимание, что если список excludes содержит что-то, что вы пытаетесь включить в списки packages или includes, py2exe может продолжать исключать это.
-  **dll_excludes** - исключает dll, которые не нужны в нашем проекте.

В словаре **options** у нас есть еще несколько опций, на которые стоит обратить внимание. Ключ **compressed** указывает py2exe, сжимать или нет zip-файл, если он установлен. Ключ optimize задает уровень оптимизации. Ноль - это отсутствие оптимизации, а 2 - самый высокий уровень. Установив **optimize** на 2, мы можем уменьшить размер папки примерно на один мегабайт. Ключ **bundle_files** связывает dlls в zip-файл или exe. Допустимыми значениями для bundle_files являются:

-  1 = упаковывать все, включая интерпретатор Python.
-  2 = упаковывать все, кроме интерпретатора Python.
-  3 = не упаковывать (по умолчанию).

Несколько лет назад, когда я только начинал изучать py2exe, я спросил в их списке рассылки, какой вариант лучше, потому что у меня были проблемы с вариантом bundle 1. Мне ответили, что вариант 3, вероятно, самый стабильный. Я перешел на него и перестал испытывать случайные проблемы, поэтому сейчас я рекомендую именно его. Если вам не нравится распространять более одного файла, заархивируйте их или создайте программу установки. Единственный вариант, который я использую в этом списке, это **dist_dir**. Я использую его для экспериментов с различными вариантами сборки или для создания пользовательских сборок, когда я не хочу перезаписывать свою основную хорошую сборку. Обо всех остальных опциях вы можете прочитать на сайте py2exe.

Пакет py2exe не поддерживает включение eggs Python в свои двоичные файлы, поэтому если вы установили пакет, от которого зависит ваше приложение, как egg, то при создании исполняемого файла он не будет работать. Вам придется убедиться, что ваши зависимости установлены нормально.

## Подведение итогов

На данном этапе вы должны знать достаточно, чтобы начать использовать py2exe самостоятельно. Вы можете приступить к работе и начать распространять свое последнее творение прямо сейчас. Следует отметить, что существует несколько альтернатив py2exe, таких как **bbfreeze, cx_freeze** и **PyInstaller**. Вам стоит попробовать хотя бы пару из них, чтобы понять, насколько они лучше. Создание исполняемых файлов может быть нелегким делом, но наберитесь терпения и настойчиво пройдите через это. Сообщество разработчиков упаковки Python готово помочь. Все, что вам нужно сделать, это спросить.
