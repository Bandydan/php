# Dependency Injection, Traits сказ о том как иметь зависимости и получать от этого удовольствие!

`— Так это и есть твоя тайна? Твое грандиозное приключение? Ты три дня валялся на пляже и хлебал ром?
— Добро пожаловать на Карибы, моя любовь.`

![fallout_addiction](images/fallout_dependency.jpg)

## Traits

Трейт - это механизм обеспечения повторного использования кода в языках с поддержкой только одиночного наследования, таких как PHP. 
Трейт предназначен для уменьшения некоторых ограничений одиночного наследования, позволяя разработчику повторно использовать наборы методов свободно, в нескольких независимых классах и реализованных с использованием разных архитектур построения классов. 
Семантика комбинации трейтов и классов определена таким образом, чтобы снизить уровень сложности, а также избежать типичных проблем, связанных с множественным наследованием и смешиванием (mixins).

![trait_scheme](images/trait_model.png)

Трейт очень похож на класс, но предназначен для группирования функционала хорошо структурированым и последовательным образом. 
Невозможно создать самостоятельный экземпляр трейта. 
Это дополнение к обычному наследованию и позволяет сделать горизонтальную композицию поведения, то есть применение членов класса без необходимости наследования.

![trait_module_phone](images/traite_module_phone.jpg)

[Больше про трейты](https://www.php.net/manual/ru/language.oop5.traits.php)

### Правило хорошего тона.  

Задача. 
В некоторых классах мы используем логирование. Давайте создаим трейт.

```php
<?php

use Psr\Log\LoggerInterface;

trait LoggerAwareTrait
{
    /**
     * @var LoggerInterface 
     */
    private $logger;
    
    public function setLogger(LoggerInterface $logger)
    {
        $this->logger = $logger;
        
        return $this;
    }

    public function getLogger() : LoggerInterface
    {
        return $this->logger;
    }
}
```
Пока все хорошо, теперь мы должны подключить жанный трейт к нашему классу.

```php
<?php

class Rick
{
    use LoggerAwareTrait;

}
```

Теперь класс Rick умеет логировать данные используя сторонрий класс. Но как нам это понять?
В случае если это наш код  мы знаем что модем вызвать еужные методы, но как про это может знать клеинсткий код?

Рашение проблемы. Давайте создадим интерфейс.

```php
<?php

use Psr\Log\LoggerInterface;

interface LoggerAwareInterface
{
    public function setLogger(LoggerInterface $logger);
    
    public function getLogger() : LoggerInterface;
}

```

Теперь немного модифицируем наш класс.

```php
<?php

class Rick implements LoggerAwareInterface 
{
    use LoggerAwareTrait;

}
```
 
Все теперь клиентский код без проблем узнает что наш класс умеет логировать.

```php
$rick = new Rick();

if ($rick instanceof LoggerAwareInterface)
{
    $rick->getLogger()->notice("Hello I'm Rick");
}
```
![rick](images/rick.jpg)

## Dependency Injection

