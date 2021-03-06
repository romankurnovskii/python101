---
title: 11. Классы
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
weight: 11
---



Все в Python является объектом. Это очень расплывчатое утверждение, если только вы не посещали пару занятий по компьютерному программированию. Это означает, что каждая вещь в Python имеет методы и значения. Причина в том, что в основе всего лежит класс. Класс - это проект объекта. Давайте посмотрим, что я имею в виду:

```sh
>>> x = "Mike"
>>> dir(x)
['__add__', '__class__', '__contains__', '__delattr__', '__doc__', '__eq__',
'__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__',
'__getslice__', '__gt__', '__hash__', '__init__', '__le__', '__len__', '__lt__',
'__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__',
'__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__',
'_formatter_field_name_split', '_formatter_parser', 'capitalize', 'center', 'count',
'decode', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'index', 'isalnum',
'isalpha', 'isdigit', 'islower', 'isspace', 'istitle', 'isupper', 'join', 'ljust',
'lower', 'lstrip', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition',
'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title',
'translate', 'upper', 'zfill']
```

Здесь у нас есть строка, присвоенная переменной **x**. Может показаться, что это не так много, но у этой строки есть много методов. Если вы используете ключевое слово **dir** в Python, то сможете получить список всех методов, которые можно вызвать для вашей строки. Здесь 71 метод! Технически мы не должны напрямую вызывать методы, начинающиеся с символов подчеркивания, поэтому общее количество методов сокращается до 38, но это все равно очень много! Что это значит? Это значит, что строка основана на классе, а **x**- это **экземпляр** этого класса!

В Python мы можем создавать свои собственные классы. Давайте начнем!

## Создание класса

Создать класс в Python очень просто. Вот очень простой пример:

```sh
class Vehicle(object):
    """docstring"""

    def __init__(self):
        """Constructor"""
        pass
```

Этот класс не делает ничего особенного, однако он является очень хорошим инструментом обучения. Например, чтобы создать класс, нам нужно использовать ключевое слово Python **class**, за которым следует имя класса. В Python принято, что в имени класса первая буква должна быть заглавной. Далее следует открытая скобка, за которой следует слово **object** и закрытая скобка. **object** - это то, на чем основан класс или от чего он наследуется. Он известен как базовый класс или родительский класс. Большинство классов в Python основаны на **объекте**. У классов есть специальный метод **__init__** (для инициализации). Этот метод вызывается всякий раз, когда вы создаете (или инстанцируете) объект на основе данного класса. Метод **__init__** вызывается только один раз и не должен вызываться повторно внутри программы. Другим термином для __init__ является **конструктор**, хотя этот термин не часто используется в Python.

Вам может быть интересно, почему я все время говорю **метод**, а не **функция**. Функция меняет свое название на "метод", когда она находится внутри класса. Вы также заметите, что каждый метод должен иметь хотя бы один аргумент (т.е. self), чего нельзя сказать об обычной функции.

В Python 3 нам не нужно прямо указывать, что мы наследуем у **объекта**. Вместо этого мы могли бы написать вышеописанное следующим образом:

```sh
# Python 3.x syntax
class Vehicle:
    """docstring"""

    def __init__(self):
        """Constructor"""
        pass
```

Вы заметите, что единственное различие заключается в том, что нам больше не нужны круглые скобки, если мы основываем наш класс на **объекте**. Давайте немного расширим определение нашего класса и наделим его некоторыми атрибутами и методами.

```sh
class Vehicle(object):
    """docstring"""

    def __init__(self, color, doors, tires):
        """Constructor"""
        self.color = color
        self.doors = doors
        self.tires = tires

    def brake(self):
        """
        Stop the car
        """
        return "Braking"

    def drive(self):
        """
        Drive the car
        """
        return "I'm driving!"
```

Приведенный выше код добавил три атрибута и два метода. Тремя атрибутами являются:

```sh
self.color = color
self.doors = doors
self.tires = tires
```

Атрибуты описывают транспортное средство. Так, автомобиль имеет цвет, некоторое количество дверей и некоторое количество шин. У него также есть два метода. Метод описывает, что делает класс. В данном случае автомобиль может **тормозить** и **ездить**. Возможно, вы заметили, что все методы, включая первый, имеют забавный аргумент **self**. Давайте поговорим об этом!

## Что такое self?

Классы Python нуждаются в способе обращения к самим себе. Это не какое-то самовлюбленное созерцание класса. Напротив, это способ отличить один экземпляр от другого. Слово **"self"** - это способ самоописания любого объекта, в буквальном смысле. Давайте рассмотрим пример, поскольку я всегда нахожу это полезным, когда изучаю что-то новое и непонятное:

Добавьте следующий код в конец класса, который вы написали выше, и сохраните его:

```sh
if __name__ == "__main__":
    car = Vehicle("blue", 5, 4)
    print(car.color)

    truck = Vehicle("red", 3, 6)
    print(truck.color)
```

Условия оператора if в данном примере это стандартный способ указать Пайтону на то, что вы хотите запустить код, если он выполняется как автономный файл. Если вы импортировали свой модуль в другой скрипт, то код, расположенный ниже проверки if не заработает. В любом случае, если вы выполните этот код, вы создадите два экземпляра класса Vehicle: экземпляр легкового автомобиля и экземпляр грузовика. У каждого экземпляра будут свои атрибуты и методы. Вот почему, когда мы выводим цвет каждого экземпляра, они отличаются. Причина в том, что класс использует аргумент **self**, чтобы сообщить себе, кто из них кто. Давайте немного изменим класс, чтобы сделать методы более уникальными:

```sh
class Vehicle(object):
    """docstring"""

    def __init__(self, color, doors, tires, vtype):
        """Constructor"""
        self.color = color
        self.doors = doors
        self.tires = tires
        self.vtype = vtype

    def brake(self):
        """
        Stop the car
        """
        return "%s braking" % self.vtype

    def drive(self):
        """
        Drive the car
        """
        return "I'm driving a %s %s!" % (self.color, self.vtype)

if __name__ == "__main__":
    car = Vehicle("blue", 5, 4, "car")
    print(car.brake())
    print(car.drive())

    truck = Vehicle("red", 3, 6, "truck")
    print(truck.drive())
    print(truck.brake())

```

В этом примере мы передаем еще один параметр, чтобы сообщить классу, какой тип автомобиля мы создаем. Затем мы вызываем каждый метод для каждого экземпляра. Если вы запустите этот код, вы должны увидеть следующий результат:

```sh
car braking
I'm driving a blue car!
I'm driving a red truck!
truck braking

```

Это демонстрирует, как экземпляр отслеживает свой аргумент "self". Вы также могли заметить, что мы можем получать значения атрибутов из метода **__init__** в другие методы. Причина в том, что все эти атрибуты снабжены предлогом **self**. Если бы мы этого не сделали, переменные вышли бы из области видимости в конце метода **__init__**.

## Подклассы

Настоящая сила классов становится очевидной, когда вы переходите к подклассам. Возможно, вы не осознаете этого, но мы уже создали подкласс, когда создавали класс на основе **объекта**. Другими словами, мы создали подкласс **object**. Поскольку **объект** не очень интересен, предыдущие примеры не демонстрируют возможности подклассификации. Так что давайте создадим подкласс нашего класса Vehicle и выясним, как все это работает.

```sh
class Car(Vehicle):
    """
    The Car class
    """

    def brake(self):
        """
        Override brake method
        """
        return "The car class is breaking slowly!"

if __name__ == "__main__":
    car = Car("yellow", 2, 4, "car")
    car.brake()
    'The car class is breaking slowly!'
    car.drive()
    "I'm driving a yellow car!"
```

Для этого примера мы создали подкласс нашего класса **Vehicle**. Вы заметите, что мы не включили метод **__init__** или метод **drive**. Причина в том, что при создании подкласса Vehicle вы получаете все его атрибуты и методы, если только вы не переопределите их. Таким образом, вы заметите, что мы переопределили метод **brake** и заставили его делать что-то другое. Остальные методы мы оставили прежними. Поэтому, когда вы говорите машине ехать, она использует оригинальный метод, и мы узнаем, что едем на желтой машине. Разве это не здорово?

Использование значений по умолчанию родительского класса известно как **наследование**. Это большая тема в объектно-ориентированном программировании (ООП). Это также простой пример **полиморфизма**. Полиморфные классы обычно имеют одинаковые интерфейсы (т.е. методы, атрибуты), но они не контактируют друг с другом. В языке Python полиморфизм не является очень жестким в плане обеспечения точного совпадения интерфейсов. Вместо этого он следует концепции **утиной типизации**. Идея **утиной типизации** заключается в том, что если объект ходит как утка и говорит как утка, то он должен быть уткой. Поэтому в Python, пока класс имеет одинаковые имена методов, не имеет значения, если реализация методов разная.

В любом случае, чтобы использовать классы в Python, вам не нужно знать все эти тонкости. Вам просто нужно знать терминологию, чтобы, если вы захотите копнуть глубже, у вас была такая возможность. Вы можете найти много хороших примеров полиморфизма в Python, которые помогут вам понять, как и зачем вы можете использовать этот концепт в собственных приложениях.

Теперь, когда вы создаете подкласс, вы можете переопределить как можно больше или меньше функций родительского класса. Если вы полностью переопределите его, то вам, вероятно, будет лучше просто создать новый класс.

## Подведение итогов

Классы немного сложны, но они очень мощные. Они позволяют использовать переменные в разных методах, что может сделать повторное использование кода еще проще. Я рекомендую заглянуть в исходники Python, чтобы найти несколько отличных примеров того, как определяются и используются классы.

Мы подошли к концу первой части. Поздравляю, что вы дошли до этого! Теперь у вас есть инструменты, необходимые для создания практически всего на Python! В части II мы проведём время время в изучении использования некоторых замечательных модулей, которые входят в дистрибутив Python. Это поможет вам лучше понять возможности Python и познакомиться с использованием Стандартной библиотеки. Часть II будет представлять собой набор уроков, которые помогут вам стать отличным программистом на Python!
