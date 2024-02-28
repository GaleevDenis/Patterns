# 1. Использование порождающего шаблона проектирования 

#### Фабричный метод (Factory method)
----

#### Задание

В базе данных системы электронного документооборота обрабатываются документы 3-х типов: письма, приказы и распоряжения о командировке. Каждый документ имеет номер, дату и краткую информацию о содержании. Кроме того, в письме указывается тип (входящее/исходящее) и корреспондент, в приказе –подразделение, срок выполнения и ответственный исполнитель, в распоряжении о командировке – сотрудник, период и место назначения.
Необходимо вывести:
1. Полный перечень документов.
2. Содержание выбранного документа (по номеру документа).

#### Описание шаблона проектирования

Фабричный метод (виртуальный конструктор, factory method) — это порождающий паттерн проектирования, который определяет общий интерфейс для создания объектов в суперклассе, позволяя подклассам изменять тип создаваемых объектов.

#### Интерпретация шаблона проектирования для решения задачи.

Для решения задачи мы будем использовать порождающий паттерн проектирования - Фабричный метод (Factory method). В качестве суперкласса будет использоваться класс Document который будет реализовывать инициализацию “Общих” атрибутов создаваемых объектов, таких как, номер, дата, информация (в конкретном случае документов), в качестве подклассов будут созданы три класса DocumentPismo, DocumentPrikaz, DocumentRasporyazhenie, которые будут создавать соответствующие типы документов (письмо, приказ, распоряжение) при создании объекта через пользовательский интерфейс,  добавляя свои уникальные атрибуты в объект и наследуя “Общие” атрибуты от суперкласса.

```python
from enum import Enum


class DataBase(Enum):
    """
    Перечисление текущих типов документов в базе данных
    """
    PISMO = 0,
    PRIKAZ = 1,
    RASPORYAZHENIE = 2


class Document:
    """
    Базовый класс для документов, которые можно
    создать в базе данных
    """
    def __init__(self, number, date, inf):
        self.number = number # Номер
        self.date = date # Дата
        self.inf = inf # Информация

    def get_number(self):
        return self.number
    
    def get_date(self):
        return self.date
    
    def get_inf(self):
        return self.inf
    


class DocumentPismo(Document):
    def __init__(self, type="Входящее", people = "Иванов.А.А"):
        super().__init__(number = 1, date = "10.03.23", inf = "Письмо от кого-то")
        self.type = type
        self.people = people
        
    def get_type(self):
        return self.type
    
    def get_people(self):
        return self.people    
    
    def __str__(self):
        return f'\nСодержание документа номер: {my_document.get_number()}\nТип: {my_document.get_type()}\nИнформация: {my_document.get_people()}\n'


class DocumentPrikaz(Document):
    def __init__(self, podrazdelenie = "Бухгалтерия", srok_vypolneniya = "До 11.12.24", ispolnitel = "Малиновских И.А"):
        super().__init__(number=2, date = "14.08.23", inf = "Приказ выполнить что-то")
        self.podrazdelenie = podrazdelenie
        self.srok_vypolneniya = srok_vypolneniya
        self.ispolnitel = ispolnitel
        
    def get_podrazdelenie(self):
        return self.podrazdelenie
    
    def get_srok_vypolneniya(self):
        return self.srok_vypolneniya
    
    def get_ispolnitel(self):
        return self.ispolnitel      
    
    def __str__(self):
        return f'\nСодержание документа номер: {my_document.get_number()}\nПодразделение: {my_document.get_podrazdelenie()}\nСрок выполнения: {my_document.get_srok_vypolneniya()}\nИсполнитель: {my_document.get_ispolnitel()}\n'


class DocumentRasporyazhenie(Document):
    def __init__(self, sotrudnik = "Шапошников И.Ф", period = "22.06.23-15.09.25", mesto = "Чукотка"):
        super().__init__(number=3, date="27.12.23", inf="Распоряжение о командировке")
        self.sotrudnik = sotrudnik
        self.period = period
        self.mesto = mesto
        
    def get_sotrudnik(self):
        return self.sotrudnik
    
    def get_period(self):
        return self.period

    def get_mesto(self):
        return self.mesto
    
    def __str__(self):
        return f'\nСодержание документа номер: {my_document.get_number()}\nСотрудник: {my_document.get_sotrudnik()}\nПериод: {my_document.get_period()}\nМесто назначения: {my_document.get_mesto()}\n'


def create_document(document_type: DataBase) -> Document:
    """
    Factory Method
    """
    factory_dict = {
        DataBase.PISMO: DocumentPismo,
        DataBase.PRIKAZ: DocumentPrikaz,
        DataBase.RASPORYAZHENIE: DocumentRasporyazhenie
    }
    return factory_dict[document_type]()


if __name__ == '__main__':
    
    for document_type in DataBase:
        my_document = create_document(document_type)
        print(f'\nТип документа: {document_type}\nНомер: {my_document.get_number()}\nДата: {my_document.get_date()}\nИнформация: {my_document.get_inf()}')
    check = int(input("Выберите номер документа (Выйти: 0): "))
    while check != 0:
        proverka = False
        for document_type in DataBase:
            my_document = create_document(document_type)
            if my_document.get_number() == int(check):
                if document_type == DataBase.PISMO:
                    print(my_document)
                elif document_type == DataBase.PRIKAZ:
                    print(my_document)
                elif document_type == DataBase.RASPORYAZHENIE:
                    print(my_document)
                check = int(input("Выберите номер документа (Выйти: 0): "))
                proverka = True
        if proverka is False:
            print("Не найден такой номер. Выберите номер документа (Выйти: 0):")
            check = int(input("Выберите номер документа (Выйти: 0): "))
```
# 2. Использование структурного шаблона проектирования 

#### Фасад (Facade pattern)
----

#### Задание

Реализовать систему управления домашним кинотеатром, включающим следующие электронные компоненты: широкоформатный телевизор, Blu-Ray проигрыватель, ресивер с акустической системой. Каждый из компонентов имеет собственный интерфейс управления и ряд настроек (например, телевизор – формат отображения, режим яркости, 3D; ресивер – режим объемного звучания, громкость).
Реализовать общий интерфейс управления домашним кинотеатром, предусматривающий включение и комплекс настроек для нескольких типовых режимов просмотра / прослушивания (например, мультфильмы, классическая музыка и т.п.).

#### Описание шаблона проектирования

Фасад (facade) – это структурный паттерн проектирования, который предоставляет простой интерфейс к сложной системе классов, библиотеке или фреймворку.
Клиент – использует фасад вместо прямой работы с объектами сложной подсистемы.
Фасад – предоставляет быстрый доступ к определённой функциональности подсистемы. Он «знает», каким классам нужно переадресовать запрос, и какие данные для этого нужны.
Сложная подсистема – состоит из множества разнообразных классов. Для того, чтобы заставить их что-то делать, нужно знать подробности устройства подсистемы, порядок инициализации объектов и так далее.
Дополнительный фасад – можно ввести, чтобы не «захламлять» единственный фасад разнородной функциональностью. Он может использоваться как клиентом, так и другими фасадами.

#### Интерпретация шаблона проектирования для решения задачи.

Для решения задачи мы будем использовать структурный паттерн проектирования - Фасад (Facade). В данном случае компонентами системы будет класс Tv (Телевизор), BluRay (BluRay проигрыватель), Resiver (ресивер с акустической системой), в роли клиента выступает специальный метод __name__,  фасадом класс HomeTheater (Домашний кинотеатр), который через свои методы делегирует выполнение работы компонентам и их методам. При этом надо учитывать, что клиент может при необходимости обращаться напрямую к компонентам, например, отдельно от других компонентов использовать текстовый редактор. Но в виду сложности процесса создания приложения лучше использовать фасад. Также это не единственный возможный фасад для работы с данными компонентами. При необходимости можно создавать альтернативные фасады.

```python
class Tv:
    
    def __init__(self, pitanie = False, format = "low", mode="2D"):
        self.pitanie = pitanie
        self.format = format
        self.mode = mode
        
    def start_tv(self):
        if not self.pitanie:
            self.pitanie = True
            print("Телевизор включен.")
        else:
            print("Телевизор уже был включен.")
    
    def stop_tv(self):
        if self.pitanie:
            self.pitanie = False
            print("Телевизор выключен.")
        else:
            print("Телевизор уже был выключен.")
        
    def start_hd(self):
        if self.format == "low":
            self.format = 'HD'
            print("У телевизора включен режим HD") 
        else:
            print("У телевизора уже был включен режим HD")
    
    def stop_hd(self):
        if self.format == "HD":
            self.format = 'low'
            print("У телевизора включен режим low" )
        else:
            print("У телевизора уже был включен режим low")
        
    def start_3D(self):
        if self.mode == "2D":
            self.mode = '3D'
            print("У телевизора включен режим 3D" )
        else:
            print("У телевизора уже был включен режим 3D")
    
    def stop_3D(self):
        if self.mode == "3D":
            self.mode = '2D'
            print("У телевизора включен режим 2D" )
        else:
            print("У телевизора уже был включен режим 2D")
    
class BluRay:
    
    def __init__(self, pitanie = False, bright = "low"):
        self.pitanie = pitanie
        self.bright = bright
        
    def start_bluray(self):
        if not self.pitanie:
            self.pitanie = True
            print("BluRay проигрыватель включен.")
        else:
            print("BluRay проигрыватель уже был включен.")
    
    def stop_bluray(self):
        if self.pitanie:
            self.pitanie = False
            print("BluRay проигрыватель выключен.")
        else:
            print("BluRay проигрыватель уже был выключен.")
    
    def bright_low(self):
        if self.bright == "high":
            self.bright = 'low'
            print("У BluRay проигрывателя включена низкая яркость") 
        else:
            print("У BluRay проигрывателя уже была включена низкая яркость")
    
    def bright_high(self):
        if self.bright == "low":
            self.bright = 'high'
            print("У BluRay проигрывателя включена высокая яркость" )
        else:
            print("У BluRay проигрывателя уже была включена высокая яркость")

class Resiver:

    def __init__(self, pitanie = False, volume = "low", mode="2D"):
        self.pitanie = pitanie
        self.volume = volume
        self.mode = mode
        
    def start_res(self):
        if not self.pitanie:
            self.pitanie = True
            print("Рессивер включен.")
        else:
            print("Рессивер уже был включен.")
    
    def stop_res(self):
        if self.pitanie:
            self.pitanie = False
            print("Рессивер выключен.")
        else:
            print("Рессивер уже был выключен.")
        
    def volume_low(self):
        if self.volume != "low":
            self.volume = 'low'
            print("У рессивера включена низкая громкость") 
        else:
            print("У рессивера уже включена низкая громкость")
    
    def volume_mid(self):
        if self.volume != "mid":
            self.volume = 'mid'
            print("У рессивера включена средняя громкость") 
        else:
            print("У рессивера уже включена средняя громкость")
    
    def volume_high(self):
        if self.volume != "high":
            self.volume = 'high'
            print("У рессивера включена высокая громкость" )
        else:
            print("У рессивера уже включена высокая громкость")
        
    def start_music_3D(self):
        if self.mode == "2D":
            self.mode = '3D'
            print("У рессивера включен режим 3D-звука" )
        else:
            print("У рессивера уже был включен режим 3D-звука")
    
    def stop_music_3D(self):
        if self.mode == "3D":
            self.mode = '2D'
            print("У рессивера включен режим 2D-звука" )
        else:
            print("У рессивера уже был включен режим 2D-звука")

# Фасад (Facade)
class HomeTheater:
    
    def __init__(self):
        self.TV = Tv()
        self.BluRay = BluRay()
        self.Resiver = Resiver()
        
    def start_system(self):
        print("Включается домашний кинотеатр:\n")
        self.TV.start_tv()
        self.BluRay.start_bluray()
        self.Resiver.start_res()
        print("\n")
         
    def mode_cartoon(self):
        print("Включается режим мультики:\n")
        self.TV.start_hd()
        self.TV.start_3D()
        self.BluRay.bright_high()
        self.Resiver.start_music_3D()
        self.Resiver.volume_mid()
        print("\n")
        
    def mode_classic(self):
        print("Включается режим классической музыки:\n")
        self.TV.start_hd()
        self.TV.stop_3D()
        self.BluRay.bright_low()
        self.Resiver.start_music_3D()
        self.Resiver.volume_high()
        print("\n")
    
    def stop_system(self):
        print("Выключается домашний кинотеатр:\n")
        self.TV.stop_tv()
        self.BluRay.stop_bluray()
        self.Resiver.stop_res()
        print("\n")
    
# Тестовый стенд
if __name__ == '__main__':
    homeTheater = HomeTheater()
    homeTheater.start_system()
    homeTheater.mode_cartoon()
    homeTheater.mode_classic()
    homeTheater.stop_system()
```

# 3. Использование поведенческого шаблона проектирования 

#### Интерпретатор (Interpreter)
----

#### Задание

Реализовать библиотеку для задания образцов поиска строк (регулярных выражений). Должны поддерживаться:
•	поиск строки по точному совпадению;
•	поиск строки по неточному совпадению;
•	применение оператора AND (и);
•	применение оператора OR (или).


#### Описание шаблона проектирования

"Интерпретатор" (Interpreter) — это поведенческий шаблон проектирования, который определяет способ интерпретации грамматики определенного языка. Этот шаблон предоставляет способ интерпретации заданных выражений, что обеспечивает гибкость при реализации правил в соответствии с определенным языком.

#### Интерпретация шаблона проектирования для решения задачи.

Для решения задачи мы будем использовать поведенческий паттерн проектирования - "Интерпретатор" (Interpreter). В нашей задачи Пользователь в начале инициализирует текст обращаясь к классу Context (это сделано для того, чтобы каждый раз пользователю не приходилось вводить текст заново) создается атрибут context который будет хранить этот текст, после этого он может обратиться к одному из четырех классов передавая при инициализации этот объект:
1. Класс ExactMatchExpression (Поиск предложений по точному совпадению) принимает при инициализации объект (текст) созданный пользователем, при вызове метода interpret запрашивает слово и возвращает все предложения, содержащие это слово (регистр при этом имеет значение, слово не может быть частью другого слова)
2. Класс InexactMatchExpression (Поиск предложений по неточному совпадению) принимает при инициализации объект (текст) созданный пользователем, при вызове метода interpret запрашивает слово и возвращает все предложения, содержащие это слово по неточному совпадению (регистр при этом не имеет значение, слово может быть частью другого слова)
3. Класс AndExpression (Оператор И) принимает при инициализации объект (текст) созданный пользователем, при вызове метода interpret запрашивает слово с точным и не точным совпадением и возвращает все предложения удовлетворяющие одновременно двум условиям поиска (без дублирования) (для слова с точным совпадением (регистр имеет значение, слово не может быть частью другого слова), для слова с неточным совпадением (регистр не имеет значение, слово может быть частью другого слова))
4. Класс OrExpression (Оператор Или) принимает при инициализации объект (текст) созданный пользователем, при вызове метода interpret запрашивает слово с точным и не точным совпадением и возвращает все предложения удовлетворяющие двум условиям поиска (без дублирования) (для слова с точным совпадением (регистр имеет значение, слово не может быть частью другого слова), для слова с неточным совпадением (регистр не имеет значение, слово может быть частью другого слова))


```python
import re


# Контекст - предложения
class Context:
    def __init__(self):
        self.context = input("Введите предложения: ", ).split(".")


# Точное совпадение
class ExactMatchExpression():

    def __init__(self, client):
        self.client = client
        self.exact = fr"\b{(input('Введите слово для точного совпадения: '))}\b"

    def interpret(self):
        matches = []
        for sentence in self.client.context:
            if re.search(self.exact, sentence):
                matches.append(sentence)
        if matches:
            return matches
        return ["Предложения с этим словом не найдены"]


# Неточное совпадение
class InexactMatchExpression():

    def __init__(self, client):
        self.client = client
        self.inexact = fr'{input("Введите слово для неточного совпадения: ")}'

    def interpret(self):
        matches = []
        for sentence in self.client.context:
            if re.search(self.inexact, sentence, flags=re.IGNORECASE):
                matches.append(sentence)
        if matches:
            return matches
        return ["Предложения с этой частью слова не найдены"]


# Оператор AND
class AndExpression():
    def __init__(self, client):
        self.client = client
        self.expr1 = ExactMatchExpression(self.client)
        self.expr2 = InexactMatchExpression(self.client)

    def interpret(self):
        matches1 = self.expr1.interpret()
        matches2 = self.expr2.interpret()
        result = [sentence for sentence in self.client.context if sentence in matches1 and sentence in matches2]
        if result:
            return result
        return ["Предложения содержащие эти слова не найдены"]


# Оператор OR
class OrExpression():
    def __init__(self, client):
        self.client = client
        self.expr1 = ExactMatchExpression(self.client)
        self.expr2 = InexactMatchExpression(self.client)

    def interpret(self):
        matches1 = self.expr1.interpret()
        matches2 = self.expr2.interpret()
        result = [sentence for sentence in self.client.context if sentence in matches1 or sentence in matches2]
        if result:
            return result
        return ["Предложения содержащие одно из введённых слов не найдены"]
    

# Тестовый стенд
if __name__ == "__main__":
    client = Context() # Быстрая бурая лиса прыгает через ленивого пса. Лиса млекопитающее. Бурые кошки такие милые.
    exact_match = ExactMatchExpression(client).interpret() # Бурые
    inexact_match = InexactMatchExpression(client).interpret() # лис
    and_expr = AndExpression(client).interpret() # Бурые # ко
    or_expr = OrExpression(client).interpret() # Бу # лис

    print("Точное совпадение:", exact_match)
    print("Неточное совпадение:", inexact_match)
    print("И Оператор:", and_expr)
    print("Или оператор:", or_expr)
```


#### Цепочка ответственности (Chain of responsibility)
----

#### Задание

Есть четыре приемника монет, каждый из которых принимает монеты только одного номинала (1, 5, 10 и 25 центов). Необходимо вывести на экран общую внесенную сумму и количество внесенных монет каждого номинала.


#### Описание шаблона проектирования

“Цепочка Обязанностей” (Chain of responsibility) - поведенческий шаблон проектирования, который позволяет избежать жесткой привязки отправителя запроса к получателю. Все возможные обработчики запроса образуют цепочку, а сам запрос перемещается по этой цепочке. Каждый объект в этой цепочке при получении запроса выбирает, либо закончить обработку запроса, либо передать запрос на обработку следующему по цепочке объекту.

#### Интерпретация шаблона проектирования для решения задачи.

Для решения задачи мы будем использовать поведенческий паттерн проектирования “Цепочка обязанностей” (Chain of responsibility). На вход мы будем получать данные типа строка (последовательность номиналов монет которые будет считать наш приемник) они будут передаваться в класс CoinMachine, в этом классе будет создана цепочка классов CoinReceiver которые будут обрабатывать разные номиналы монет в таком порядке 1 -> 5-> 10 -> 25, если номинал может быть обработан классом CoinReceiver он сохраняет значение в атрибут и не передает значение дальше по цепочке обязанностей, если класс CoinReceiver дошел до последнего номинала монеты (25) и не смог обработать значение, то он возвращает строку “Монета номиналом … не принимается этим приемником”, цикл повторяется до тех пор пока не будут обработаны все монеты.


```python
class CoinReceiver:
    def __init__(self, denomination):
        self.denomination = denomination
        self.count = 0
        self.successor = None  # Поле для хранения следующего приемника монет

    def set_successor(self, successor):
        self.successor = successor

    def receive_coin(self, coin_value):
        if coin_value == self.denomination:
            self.count += 1
        elif self.successor:
            self.successor.receive_coin(coin_value)

class CoinMachine:
    def __init__(self):
        self.receiver1 = CoinReceiver(1)
        self.receiver2 = CoinReceiver(5)
        self.receiver3 = CoinReceiver(10)
        self.receiver4 = CoinReceiver(25)

        self.receiver1.set_successor(self.receiver2)
        self.receiver2.set_successor(self.receiver3)
        self.receiver3.set_successor(self.receiver4)
        
        self.receivers = [1, 5, 10, 25]

    def receive_coin(self, coin_value):
        for i in coin_value.split():
            if int(i) in self.receivers:
                self.receiver1.receive_coin(int(i))
            else:
                print(f"Монета номиналом {int(i)} не принимается этим приемником.\n")
                
    def total_amount(self):
        total = 0
        receivers = [self.receiver1, self.receiver2, self.receiver3, self.receiver4]
        for receiver in receivers:
            total += receiver.denomination * receiver.count
            print(f"Количество монет номиналом {receiver.denomination} центов: {receiver.count}")
        print(f"Общая сумма внесенных монет: {total} центов")

# Тестовый стенд
machine = CoinMachine()
machine.receive_coin(str(input())) #  10 5 25 5 1 25 5 25 1 25
machine.total_amount()
```