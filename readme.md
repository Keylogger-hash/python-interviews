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
* Как работают декораторы`<br>`
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

Output`<br>`

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
  В python бывают следующие типы данных:

  Неиммутабельные: число(number), строка(string),кортеж(tuple)

  Иммутабельные: список(list), множество(set), словарь(dict)
* В чем отличие list от tuple?

  List отличается от tuple тем что лист иммутабельный, а tuple нет
* Как реализован type checking python?
* Библиотеки для type checking

  typing, pydanticч>я>
* Как хранятся в памяти list, tuple, dict, set

  List - хранится как динамический массив в котором хранятся указатели на объекты

  Получение по индексу происходит по константному времени, так же как и append, все остальные операции за O(n)

  Tuple - хранится как глобально уникальный объект. Таким образом, все пустые кортежи — это один и тот же объект, а значит и адрес в памяти у таких кортежей один.

  Dict - хранится как хеш таблица(разреженный массив), каждая единица называется сегментом, и каждый сегмент состоит из двух частей, первая это ссылка на ключ, вторая объект значения. Поскольку все сегменты имеют одинаковую структуру и размер, мы можем указать местоположение сегмента по смещению. Условие объекта который может использоваться как ключ это его хэшируемость(число, строка кортеж). Внутри объекта есть методы __hash__ и __eq__.

  Set - использует хеш функцию

  https://wiki.python.org/moin/TimeComplexity

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
  Сигналы это реализация паттерна observer.В данном случае один или несколько объектов подписываются на объект и наблюдают за событиями которые с ним происходят . С помощью диспетчера сигналов django может распределять сигналы в несвязанном наборе на различные приемники в различных компонентах системы. Сигналы регистрируют всякий раз когда в функции или методе на который подписались произошло определенное событие его получает приемник вместе с некой констектуальной нагрузкой нужной для функциональности приемника. Приемником может быть любая функция или метод в python. Нужно когда в save определять функциональность избыточно и приведет к кодовой несвязности.
* Как в request появляется атрибут user?
  Request user появляется через промежуточный middleware(Authentication middleware). process_request
* Что такое дата-миграция?
  Обычные миграции меняют структуру таблиц в базе данных: добавляют поля, удаляют поля и так далее. Дата миграции позволяют перенсти данные вместе с таблицей
  Пример:
  `class Customer(models.Model): name = models.CharField(max_length=200) ... class Seller(models.Model): name = models.CharField(max_length=200) ...`
  У нас есть две таблицы Customer и Seller. Допустим мы хотим добавить третью таблицу user и скопировать все имена из seller и customer в эту таблицу и затем убрать таблицу customer и seller. Если мы так сделаем то мы все данные потеряем. С дата миграцией такого не произойдет. Нам надо прописать команду:
  `python manage.py makemigrations --empty appname1`
  Далее у нас создается миграция

  from django.db import migrations  `class Migration(migrations.Migration): dependencies = ['appname','009_auto'_2923_129) operations = []`

  Затем на надо добавить:

  ```python
  operations = [
          migrations.RunPython(copy_customers_to_users),
          migrations.RunPython(copy_sellers_to_users),
      ]
  ```

  ```python
  def copy_customers_to_users(apps, schema_editor):
      User = apps.get_model('appname', 'User')
      Customer = apps.get_model('appname', 'Customer')
      for customer in Customer.objects.all():
          User.objects.get_or_create(name=customer.name)
  ```

  Затем пишем обычную команду
  `python manage.py migrate`

  Вуаля данные скопировались.
* Продвинутые методы orm

  exists() - существует ли строка в базе данных

  count() - количество объектов

  values() - выбранные колонки возращает словарь

  values_list() - выбранные колонки возращает кортеж

  exclude() - исключить строки по критериям

  filter() - отфильтровать по значениям

  select_related() - делает выборку по ключу

  prefetch_related() - делает заранее выборку по many to many field

  order_by() - сортировка по значению

  last,first,earliest - первое значение, последнее значение, ранне по времени

  annotate - аннотирует значение и добавляет его к запросу. Выражение может быть аггрегированной функцией, простым значением, полем в модели, ссылкой значением которое будет вычислено для объектов связанных с значением

  alias - то же что и annotate, но только его сохраняет. Это полезно, когда результат самого выражения не нужен, но используется для фильтрации, упорядочивания или как часть сложного выражения

  distinct - уникальные значения

  dates - возращает список всех объектов похожих на datetime.datetime

  all - все значения

  union - используется для объединения двух или более querysets

  intersection - используется для пересечения двух или более queryset

  difference - нужен для того чтобы сохранить элементы только присутствующие в queryset

  extra - для внедрения sql

  where - добавить  значения в where

  defer - для отложенной загрузки в память

  only - для немедленной загрузки в память

  using - с какой базы данных будет приниматься запрос

  raw - принимает необработанный SQL запрос

  select_for_update - Возвращает набор запросов, который будет блокировать строки до конца транзакции, создавая оператор SQL для поддерживаемых баз данных.SELECT ... FOR UPDATE

  get,create,update - получить, создать, обновить

  bulk_create - эффективно вставляет множество объектов

  bulk_update - Этот метод эффективно обновляет указанные поля в предоставленных экземплярах модели, как правило, с помощью одного запроса и возвращает количество обновленных объектов.

  bulk_in - сопоставляет с id

  aggregate - Возвращает словарь совокупных значений (средние значения, суммы и т. д.), рассчитанных по QuerySet. Каждый аргумент aggregate()указывает значение, которое будет включено в возвращаемый словарь. Функции агрегации, предоставляемые Django, описаны в разделе « Функции агрегации » ниже. Поскольку агрегаты также являются выражениями запросов , вы можете комбинировать агрегаты с другими агрегатами или значениями для создания сложных агрегатов. Агрегаты, указанные с помощью аргументов ключевого слова, будут использовать ключевое слово в качестве имени для аннотации. Для анонимных аргументов будет создано имя, основанное на имени агрегатной функции и агрегируемом поле модели. Сложные агрегаты не могут использовать анонимные аргументы и должны указывать аргумент ключевого слова в качестве псевдонима.

  Explain - Возращает строку queryset плана выполнения в которой подробно будет описано как база данных будет выполнять запрос. Не поддерживается oracle

  ...
* Как в django откатить транзакцию
  Импортировать `from django.db import transaction`
  `transcation.atomic(). `
  Можно перехватывать исключения и делать rollback транзакции или сначала делать transcation.savepoint() и затем при возникновении ошибки делать метод transcation_rollback().

  ```


  https://django.fun/docs/django/ru/4.0/topics/db/transactions/#:~:text=Django%20%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B5%D1%82%20%D1%82%D1%80%D0%B0%D0%BD%D0%B7%D0%B0%D0%BA%D1%86%D0%B8%D0%B8%20%D0%B8%D0%BB%D0%B8%20%D1%82%D0%BE%D1%87%D0%BA%D0%B8,%D0%B2%20%D1%82%D1%80%D0%B0%D0%BD%D0%B7%D0%B0%D0%BA%D1%86%D0%B8%D0%B8%20%D0%BF%D0%BE%20%D1%81%D0%BE%D0%BE%D0%B1%D1%80%D0%B0%D0%B6%D0%B5%D0%BD%D0%B8%D1%8F%D0%BC%20%D0%BF%D1%80%D0%BE%D0%B8%D0%B7%D0%B2%D0%BE%D0%B4%D0%B8%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D0%BE%D1%81%D1%82%D0%B8.
  ```
* Асинхронщина в django. Как это работает?

  Работает через asgi. Для функций которые не работают используется sync to async
* ORM асинхронный?

  C версии django 4.1 - ДА

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
* Как устроен стек TCP/IP
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
* Что такое и зачем нужен docker volumes?class Customer(models.Model):

    name = models.CharField(max_length=200)
    ...

class Seller(models.Model):
    name = models.CharField(max_length=200)
    ...
