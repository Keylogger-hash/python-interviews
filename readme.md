1. Python

* Что такое list comprehension?
  Короткий синтаксис для создания нового листа на основе предыдущего.
  Пример:
  Обычный синтаксис
  ```
  x = [1,2,3,4,5,6,7,8,9]
  for i in x:
    x[i] = x[i]*2
  ```
  List comprehension
  ```
  x = [1,2,3,4,5,6,7,8,9,10]
  x = [i * i for i in x]
  ```
* Как работают декораторы<br>
  Code
```
def decorator_is_authenticated(func): # передаем функцию callback как аргумент
  def wrapper(request): # функция в которой происходит работа со списком
    if request.is_authenticated():
      func(request)
      request.code = 200
    else:
      request.code = 401
  return wrapper # возвращаем функцию inner как значение

def response(code):
  return code

class Request(object):
  
  token = None
  user = None
  method = None
  code = None
  def __init__(self, token=None, user=None,method=None):
    self.token = token
    self.user = user
    self.method = method

  def is_authenticated(self):
    if self.user is  None:
      return False # юзер не авторизован
    else:
      return True

def render(request, template_path):
  return request.code,template_path
  
def redirect(request,redirect_url):
  return request.code, redirect_url
  
@decorator_is_authenticated # вызываем декоратор
def callback(request):
  if request.method == 'GET':
    template_path = 'templates/index.html'
    return render(request,template_path)
  elif request.method == 'POST':
    redirect_url = 'form/'
    return redirect(redirect_url)  # производим вычисления

if __name__ == "__main__":
  r = Request('123','Mohamed',method='GET')
  r1 = Request(None,None, method='POST')
  # первый запрос
  callback(r)
  print(r.code)
  print(r.method)
  
  # второй запрос
  callback(r1)
  print(r1.code)
  print(r1.method)
```
<br>
Output

```
200
GET
401
POST
```
* Что такое __slots__?
__slots__ нужны для того чтобы сэкономить место в памяти при создании каждого инстанса и для более быстрого обращения к атрибутам.
Если указать __slots__, то __dict__ больше не будет использоваться
Code

```
 class BaseSlots(object):
  __slots__ = ('a','b')

  def foo(self):
    if self.a is None or self.b is None:
      print("Foo: ")
    else:
      print(f"Foo: {self.a}, {self.b}")

  def bar(self):
    print(f"Bar: {self.a}, {self.b}")

class BaseDict(object):
  
  a = None
  b = None
  
  def foo(self):
    pass

if __name__ == "__main__":
  # Атрибуты как словарь
  bd = BaseDict()
  bd.a = 1
  bd.b = 2
  print(bd.__dict__)
  
  # Для использования атрибутов используется __slots__
  bs = BaseSlots()

  #bs.foo()
  bs.a = 1
  bs.b = 2
  print(bs.a)
  print(bs.b)
  bs.bar()

```
Output<br>

```
{'a': 1, 'b': 2}
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__',
 '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', 
 '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__',
  '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 
  'a', 'b', 'foo']
1
2
Bar: 1, 2
['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', 
'__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', 
'__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', 
'__repr__', '__setattr__', '__sizeof__', '__slots__', '__str__', '__subclasshook__', 
'a', 'b', 'bar', 'foo']
```
* Как во множественном наследовании отрабатывается поиск по атрибуту
Поиск по атрибуту выполняется через алгоритм поиска разрешения методов(MRO)

2. Data types

* Какие типы данных бывают в python?(Иммутабельные неиммутабельные)
* В чем отличие list от tuple?
* Как реализован type checking python?
* Библиотеки для type checking
* Как хранятся в памяти list, tuple, dict, set

3. Testing

* Что такое TDD?
* Какие тесты приходилось писать
* Что такое мок-объекты?
* Приходилось ли использовать?

4. Asynchrounos code

* Способы выполнять код асинхронно/параллельно?
* В чем отличие асинхронности от многопоточности?
* В каких задачах использовать асинхронность необходимо?
* В каких бесмысленно?
* Как реализовать асинхронность в python

5. Django

* Как работают сигналы?
* Как в request появляется атрибут user?
* Что такое дата-миграция?
* Асинхронщина в django. Как это работает?
* ORM асинхронный?

6. Algorithms
 
* О-большое
* Бинарный поиск
* Обход дерева
* Дерево
* Лист


6. Code patterns

* Что такое паттерны проектирования?
* Какие есть?
* Как устроен паттерн Pub/Sub?
* Где в django применяется паттерн мост?

7. Database

* Что такое транзакция? 
* Как работают JOIN-ы? 
* Зачем нужны и как работают индексы? 
* Напишите запрос с GROUP BY

* Как дебажить медленный запрос? (в ответе ожидаем услышать про EXPLAIN и план запроса). 
* Как обслуживать PostgreSQL-базу? 
* Как настраивать репликацию?

8. NoSQL

* С какими NoSQL работали?
* Как работает Redis?
* Зачем нужен ElasticSearch?
* Что будет, если размер данных превысит размер ОЗУ при использовании Redis?

9. Administration
* Как устроен DNS? 
* Что такое nginx? 
* Как установить nginx на linux?

* В чём отличие http от https?
* Чем обеспечить ротацию логов?

10. Tools

* Что делает git cherry pick?
* Как устроен CI/CD в gitlab или github, либо любой знакомый вам? 
* Что такое gitflow? 
* Что делают команды rebase, fixup, stash, revert?

* Приходилось ли использовать docker/docker-compose?	
* Что такое и зачем нужен docker volumes?
