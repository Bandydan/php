# Полиморфизм, абстракция, SOLID, практика

## Абстракция

На прошлом уроке мы обсудили, попытались понять и даже немного попрактиковали написание классов, создание объектов и применение принципов наследования и инкапсуляции.

В последнее время принято называть четыре принципа ООП, и принцип, который появился в этом списке последним - принцип абстракции. 

Принцип абстракции очень прост: необходимо включать в класс, а соответственно и в объекты этого класса, только те свойства объекта, которые исользуются, абстрагируясь от лишней информации.

![Обстругируйся](http://s61.radikal.ru/i174/1302/21/d93342cd94da.jpg)

К примеру, в нашем классе Person мы описываем человека именем, фамилией, возрастом и полом, но не работаем с ростом, весом, цветом кожи, волос и глаз. У каждого человека есть такие свойства, но они нерелевантны и несущественны для нас в данный момент, и скорее всего не будут в ближайшем будущем. Если нам и понадобится собрать статистику, к примеру, покупателей интернет-магазина, вряд-ли мы будем искать закономерности в весе, росте или цвете глаз, скорее - возраст, пол, профессия и уровень дохода будут иметь значение.

Тем не менее, каждый собирает ту информацию, которую считает важной для себя. Facebook собирает о своих пользователей достаточно много информации. На 2017 год их интересовало [98 параметров](http://madcats.ru/smm/98-facebook-targetings/).

## Полиморфизм

![Туть](http://memesmix.net/media/created/vx1pfl.jpg)

Полиморфизм - один из самых сложных для понимания принципов ООП. Полиморфизм подразумевает правильную организацию классов и методов в них и требует хорошего понимания наследования, инкапсуляции и абстракции.

К примеру, человек умеет писать карандашом. Давайте попробуем реализовать это умение в нашем классe Person:

```php
<?php

class Pencil {
	function write(){
		echo "I am writing with my pencil \n";
	}
}

class Person {

	public $age, $name, $surname, $gender;
	private $pencil;

	function take_pencil(Pencil $pencil) {
		$this->pencil = $pencil;
	}

	function get_info(){
		return $this->name . ' ' . $this->surname . "\n";
	}

	function write(){
		if(is_object($this->pencil)) {
			$this->pencil->write();
		} else echo "Nothing to use";
	}

}

$blackPencil = new Pencil();
$Johnny = new Person();
$Johnny->take_pencil($blackPencil);
$Johnny->write();

```

![](https://cs5.pikabu.ru/images/big_size_comm/2015-10_3/1444922663135957975.jpg)

Этот метод что-то делает, но он абсолютно не универсален и не удобен. Следующим шагом мы добавим еще несколько инструментов, которыми можно писать, и попросим объект Johnny провзаимодействовать с ними:

```php
<?php

class Pencil {
    function write(){
        echo "I am writing with my pencil \n";
    }
}

class Feather
{
    function write(){
        echo "I am writing with my feather \n";
    }
}

class Pen{
    function write()
    {
        echo "I am writing with my pen.\n";
    }
}

class Knife
{
    function write()
    {
        echo "I am writing with my knife HAHAHAH.\n";
    }
}


class Person {

    public $age, $name, $surname, $gender;
    private $thing;

    function take_something($thing) {
        if(!is_object($thing)) return;
        $this->thing = $thing;
    }

    function get_info(){
        return $this->name . ' ' . $this->surname . "\n";
    }

    function write(){
        if(is_object($this->thing)) {
            $this->thing->write();
        } else echo "Nothing to use".PHP_EOL;
    }

}

$blackPencil = new Pencil();
$Johnny = new Person();
$Johnny->take_something($blackPencil);
$Johnny->write();

$feather = new Feather();
$pen = new Pen();
$knife = new Knife();
$Johnny->take_something($feather);
$Johnny->write();

$Johnny->take_something($pen);
$Johnny->write();

$Johnny->take_something($knife);
$Johnny->write();
```

Для того, чтобы все работало и было правильно логически, наша персона теперь вместо взятия карандаша реализует взятие какого-то предмета с проверкой, что взять можно только объект, а потом пишет им.

Но писать можно далеко не каждым объектом, а только таким, у которого есть метод write. Необходимо добавить механизм проверки наличия этого метода.

## Абстрактные классы и интерфейсы

![Now speak](https://cdn-images-1.medium.com/max/1600/1*vLM_tTuVTgMi1qpMn3Eu9g.gif)

ООП предлагает нам несколько интересных инструментов для обспечения подобных проверок: абстрактные классы и интерфейсы.

**Абстрактным классом** называется класс, объект которого нельзя создать. Такой класс может во всем остальном напоминать обычный класс В php он отличается словом **abstract** перед определением класса:

```php
<?php

abstract class WritingThing
{
    function write()
    {
        echo "I am writing with my ".strtolower(get_class($this))." \n";
    }
}
```

Абстрактные классы добавляют в нескольких случаях:
1. Вынести общую функциональность из нескольких классов выше по уровню наследования.
2. Сделать общую функциональность универсальной.
3. Логически правильно все организовать в структуре наследования.

Добавим такой класс к нашему коду:

```php
<?php

abstract class WritingThing
{
    function write()
    {
        echo "I am writing with my ".strtolower(get_class($this))." \n";
    }
}

class Pencil extends WritingThing {

}

class Feather extends WritingThing
{

}

class Pen extends WritingThing
{

}

class Knife extends WritingThing
{
    function write()
    {
        echo "I am writing with my knife HAHAHAH.\n";
    }
}


class Person {

    public $age, $name, $surname, $gender;
    private $thing;

    function take_something($thing) {
        if(!is_object($thing)) return;
        $this->thing = $thing;
    }

    function get_info(){
        return $this->name . ' ' . $this->surname . "\n";
    }

    function write(){
        if(is_object($this->thing)) {
            $this->thing->write();
        } else echo "Nothing to use".PHP_EOL;
    }

}

$blackPencil = new Pencil();
$Johnny = new Person();
$Johnny->take_something($blackPencil);
$Johnny->write();

$feather = new Feather();
$pen = new Pen();
$knife = new Knife();
$Johnny->take_something($feather);
$Johnny->write();

$Johnny->take_something($pen);
$Johnny->write();

$Johnny->take_something($knife);
$Johnny->write();
```

После применения парочки функций и абстрактного класса код стал выглядеть намного изящнее.

Попробуем внедрить так же использование абстрактных методов. 

**Абстрактным методом** называется метод без тела, перед которым так же стоит ключевое слово **abstract**:

```php
abstract method print();
```

Абстрактный метод может быть объявлен только в абстрактном классе или интерфейсе, в обычном классе его написать невозможно, язык будет выдавать ошибку.

Основное назначение абстрактного метода - заставить класс, наследующий абстрактный класс с этим методом или имплементирующий интерфейс с этим методом - реализовать этот метод, т.е. заполнить его тело и сделать этот метод реальным.

Есть только один способ каким-то образом унаследовать абстрактные методы, но не реализовывать их, это объявить наследующий класс абстрактным и оставить все пришедшие методы абстрактными.

![](http://memesmix.net/media/created/hrl2jg.jpg)

А что же такое интерфейс и в чем его ообенности?

**Интерфейсом** называется особая, максимально абстрактная сущность, которая имеет некоторые сходства с абстрактным классом. Маленький пример интерфейса:

```php
<?php

interface canWrite
{
    function write();
}

abstract class WritingThing implements canWrite
{
...
```

В отличие от абстрактного класса, интерфейсы:

- не позволяют включать в себя ничего кроме абстрактных методов и констант, тогда как в абстрактном классе могут быть и абстрактные и обычные методы;
- не требуют ключевого слова `abstract` перед методами, поскольку все методы в интерфейсе всегда абстрактны;
- не наследуются, а имплементируется (не extends, а implements);
- могут имплементироваться в класс в любом количестве, в отличие от наследования, которое в PHP позволяет унаследоваться лишь от одного класса.

Применяются интерфейсы в основном для того, чтобы создать правило и передать это правило всем классам, которые его имплементируют. По-сути, интерфейс - способ организовать несколько абстрактных методов и передать их классу или группе классов, чтобы те были обязаны его реализовать.

Применим интерфейс в наш код и немного поменяем код:

```php
<?php
interface canWrite
{
    function write();
}

abstract class WritingThing implements canWrite
{

    function write()
    {
        echo "I am writing with my ".strtolower(get_class($this))." \n";
        echo $this->additionalInfo() ."\n";
    }

    abstract protected function additionalInfo();

}


class Pencil extends WritingThing {
    protected function additionalInfo(){
        return "";
    }
}

class Feather extends WritingThing
{
    protected function additionalInfo(){
        return "";
    }
}

class Pen extends WritingThing
{
    protected function additionalInfo(){
        return "";
    }
}

class Knife extends WritingThing
{
    protected function additionalInfo(){
        return "HAHAHAH";
    }

}


class Person {

    public $age, $name, $surname, $gender;
    private $thing;

    function take_something($thing) {
        if(!is_object($thing)) return false;
        $this->thing = $thing;
        return $this;
    }

    function get_info(){
        return $this->name . ' ' . $this->surname . "\n";
    }

    function write(){
        if(method_exists($this->thing, 'write')
            && is_subclass_of($this->thing, "WritingThing"))
        {
            $this->thing->write();
        } else echo "Nothing to use".PHP_EOL;
    }

}

$blackPencil = new Pencil();
$feather = new Feather();
$pen = new Pen();
$knife = new Knife();

$Johnny = new Person();

$things = [$blackPencil, $feather, $pen, $knife];

foreach ($things as $thing)
{
    $Johnny->take_something($thing)->write();

}
```


[Домашнее задание](24_homework.md)

[25-й урок](24_Magic_methods.md)

[SOLID](https://habr.com/ru/post/348286/)

