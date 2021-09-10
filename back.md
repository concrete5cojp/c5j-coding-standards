# Concrete5 Japan Coding Standards

> A brief guideline to the c5j developers to follow the coding standards.

## Clean Code Concepts for PHP

Clean Code PHP ([jupeter/clean-code-php](https://github.com/jupeter/clean-code-php)), is a guide based on the book Clean Code: A Handbook of Agile Software Craftmanship, a classic programming book about writing maintainable code by [Uncle Bob Martin](https://twitter.com/unclebobmartin).

The clean-code-php guide is inspired by a JavaScript adaptation, [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript) with PHP-specific features.

Here are a few of my favorite adaptations from the clean-code-php repository:

### Don’t add unneeded context

#### Bad

```php
<?php
class Car
{
    public $carMake;
    public $carModel;
    public $carColor;
    //...
}
```

#### Good

```php
<?php
class Car
{
    public $make;
    public $model;
    public $color;
    //...
}
```

View [Don’t add unneeded context](https://github.com/jupeter/clean-code-php#dont-add-unneeded-context) in the guide.

### Function Arguments (2 or fewer ideally)

#### Bad

```php
<?php
function createMenu($title, $body, $buttonText, $cancellable) {
    // ...
}
```

#### Good

```php
<?php
class MenuConfig
{
    public $title;
    public $body;
    public $buttonText;
    public $cancellable = false;
}
$config = new MenuConfig();
$config->title = 'Foo';
$config->body = 'Bar';
$config->buttonText = 'Baz';
$config->cancellable = true;
function createMenu(MenuConfig $config) {
    // ...
}
```

View [Function Arguments](https://github.com/jupeter/clean-code-php#functions-should-do-one-thing) in the guide.


### Functions Should Do One Thing

#### Bad

```php
<?php
function emailClients($clients) {
    foreach ($clients as $client) {
        $clientRecord = $db->find($client);
        if ($clientRecord->isActive()) {
            email($client);
        }
    }
}
```

#### Good

```php
function emailClients($clients) {
    $activeClients = activeClients($clients);
    array_walk($activeClients, 'email');
}

function activeClients($clients) {
    return array_filter($clients, 'isClientActive');
}

function isClientActive($client) {
    $clientRecord = $db->find($client);
    return $clientRecord->isActive();
}
```

View [View Functions Should Do One Thing](https://github.com/jupeter/clean-code-php#function-arguments-2-or-fewer-ideally) in the guide.