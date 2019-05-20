# Clean Code PHP
Traducción al español de [Clean Code PHP](https://github.com/jupeter/clean-code-php) realizado por [Jupeter](https://github.com/jupeter/).
Si encuentras un error de ortografía, de redacción o de traducción; no dudes en hacer un PR.

## Tabla de contenidos

  1. [Introducción](#introducción)
  2. [Variables](#variables)
     * [Usar variables que tengan significado y sean pronunciables](#usar-variables-que-tengan-significado-y-sean-pronunciables)
     * [Usar el mismo vocabulario para el mismo tipo de variable](#usar-el-mismo-vocabulario-para-el-mismo-tipo-de-variable)
     * [Usar nombres que puedan ser buscados (parte 1)](#usar-nombres-que-puedan-ser-buscados-parte-1)
     * [Usar nombres que puedan ser buscados (parte 2)](#usar-nombres-que-puedan-ser-buscados-parte-2)
     * [Usar variables explicativas](#usar-variables-explicativas)
     * [Evitar anidación profunda usando return tempranamente (parte 1)](#evitar-anidación-profunda-usando-return-tempranamente-parte-1)
     * [Evitar anidación profunda usando return tempranamente (parte 2)](#evitar-anidación-profunda-usando-return-tempranamente-parte-2)
     * [Evitar mapas mentales](#evitar-mapas-mentales)
     * [No agregar contexto innecesario](#no-agregar-contexto-innecesario)
     * [Usar argumentos por defecto en lugar de cortocircuitos o condicionales](#usar-argumentos-por-defecto-en-lugar-de-cortocircuitos-o-condicionales)
  3. [Funciones](#funciones)
     * [Argumentos de la función (idealmente 2 o menos)](#argumentos-de-la-función-idealmente-2-o-menos)
     * [Las funciones deben hacer una cosa](#las-funciones-deben-hacer-una-cosa)
     * [Los nombres de las funciones deben indicar lo que hacen](#los-nombres-de-las-funciones-deben-indicar-lo-que-hacen)
     * [Las funciones deben tener sólo un nivel de abstracción](#las-funciones-deben-tener-sólo-un-nivel-de-abstracción)
     * [No usar banderas como parámetros de funciones](#no-usar-banderas-como-parámetros-de-funciones)
     * [Evitar efectos secundarios](#evitar-efectos-secundarios)
     * [No escribir funciones globales](#no-escribir-funciones-globales)
     * [No usar el patrón Singleton](#no-usar-el-patrón-singleton)
     * [Encapsular condicionales](#encapsular-condicionales)
     * [Evitar condicionales negativos](#evitar-condicionales-negativos)
     * [Evitar condicionales](#evitar-condicionales)
     * [Evitar revisión de tipo (parte 1)](#evitar-revisión-de-tipo-parte-1)
     * [Evitar revisión de tipo (parte 2)](#evitar-revisión-de-tipo-parte-2)
     * [Quitar código muerto](#quitar-código-muerto)
  4. [Objetos y estructuras de datos](#objetos-y-estructuras-de-datos)
     * [Usar encapsulación de objetos](#usar-encapsulación-de-objetos)
     * [Hacer que los objetos tengan partes private/protected](#hacer-que-los-objetos-tengan-partes-privateprotected)
  5. [Clases](#clases)
     * [Preferir composición antes que herencia](#preferir-composición-antes-que-herencia)
     * [Evitar interfaces fluidas](#evitar-interfaces-fluidas)
  6. [SOLID](#solid)
     * [Principio de responsabilidad única](#principio-de-responsabilidad-única)
     * [Principio de abierto/cerrado](#principio-de-abiertocerrado)
     * [Principio de la sustitución de Liskov](#principio-de-la-sustitución-de-liskov)
     * [Principio de segregación de la interfaz](#principio-de-segregación-de-la-interfaz)
     * [Principio de la inversión de dependencia](#principio-de-la-inversión-de-dependencia)
  7. [No te repitas](#no-te-repitas)
  8. [Traducciones](#traducciones)

## Introducción

Adaptación para PHP de los principios de ingeniería de software descritos por Robert C. Martin en su libro 
[*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882). Esta no es una guía de estilo. Es una guía para producir software que sea legible, reutilizable y refactorizable en PHP.

No todos los principios deben ser seguidos estrictamente, e incluso unos pocos serán aceptados totalmente. Estos son una referencia y nada más, pero han sido desarrollados tras los años de experiencia del autor de *Clean Code*.

Inspirado por [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)

A pesar de que muchos desarrolladores aún utilizan PHP 5, la mayoría de los ejemplos en este artículo funcionan sólo en PHP 7.1+.

## Variables

### Usar variables que tengan significado y sean pronunciables

**Mal:**

```php
$ymdstr = $moment->format('y-m-d');
```

**Bien:**

```php
$currentDate = $moment->format('y-m-d');
```

**[⬆ volver](#tabla-de-contenidos)**

### Usar el mismo vocabulario para el mismo tipo de variable

**Mal:**

```php
getUserInfo();
getUserData();
getUserRecord();
getUserProfile();
```

**Bien:**

```php
getUser();
```

**[⬆ volver](#tabla-de-contenidos)**

### Usar nombres que puedan ser buscados (parte 1)

Leerás más código del que puedas escribir. Es importante que el código que escribimos sea legible y que pueda ser buscado. Dañamos a nuestros lectores al *no* escribir nombres de variables que tengan significado para entender nuestro programa. Crea nombres que puedan ser buscados.

**Mal:**

```php
// ¿Qué diablos significa 448?
$result = $serializer->serialize($data, 448);
```

**Bien:**

```php
$json = $serializer->serialize($data, JSON_UNESCAPED_SLASHES | JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE);
```

### Usar nombres que puedan ser buscados (parte 2)

**Mal:**

```php
// ¿Qué diablos significa 4?
if ($user->access & 4) {
    // ...
}
```

**Bien:**

```php
class User
{
    const ACCESS_READ = 1;
    const ACCESS_CREATE = 2;
    const ACCESS_UPDATE = 4;
    const ACCESS_DELETE = 8;
}

if ($user->access & User::ACCESS_UPDATE) {
    // Editar ...
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Usar variables explicativas

**Mal:**

```php
$address = 'One Infinite Loop, Cupertino 95014';
$cityZipCodeRegex = '/^[^,]+,\s*(.+?)\s*(\d{5})$/';
preg_match($cityZipCodeRegex, $address, $matches);

saveCityZipCode($matches[1], $matches[2]);
```

**Nada mal:**

Está mejor, pero todavía es muy dependiente de la expresión regular.

```php
$address = 'One Infinite Loop, Cupertino 95014';
$cityZipCodeRegex = '/^[^,]+,\s*(.+?)\s*(\d{5})$/';
preg_match($cityZipCodeRegex, $address, $matches);

[, $city, $zipCode] = $matches;
saveCityZipCode($city, $zipCode);
```

**Bien:**

Disminuye la dependencia a la expresión regular al nombrar subpatrones.

```php
$address = 'One Infinite Loop, Cupertino 95014';
$cityZipCodeRegex = '/^[^,]+,\s*(?<city>.+?)\s*(?<zipCode>\d{5})$/';
preg_match($cityZipCodeRegex, $address, $matches);

saveCityZipCode($matches['city'], $matches['zipCode']);
```

**[⬆ volver](#tabla-de-contenidos)**

### Evitar anidación profunda usando return tempranamente (parte 1)

Muchas declaraciones if else pueden hacer tu código difícil de seguir. Explicito es mejor que implícito.

**Mal:**

```php
function isShopOpen($day): bool
{
    if ($day) {
        if (is_string($day)) {
            $day = strtolower($day);
            if ($day === 'friday') {
                return true;
            } elseif ($day === 'saturday') {
                return true;
            } elseif ($day === 'sunday') {
                return true;
            } else {
                return false;
            }
        } else {
            return false;
        }
    } else {
        return false;
    }
}
```

**Bien:**

```php
function isShopOpen(string $day): bool
{
    if (empty($day)) {
        return false;
    }

    $openingDays = [
        'friday', 'saturday', 'sunday'
    ];

    return in_array(strtolower($day), $openingDays, true);
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Evitar anidación profunda usando return tempranamente (parte 2)

**Mal:**

```php
function fibonacci(int $n)
{
    if ($n < 50) {
        if ($n !== 0) {
            if ($n !== 1) {
                return fibonacci($n - 1) + fibonacci($n - 2);
            } else {
                return 1;
            }
        } else {
            return 0;
        }
    } else {
        return 'Not supported';
    }
}
```

**Bien:**

```php
function fibonacci(int $n): int
{
    if ($n === 0 || $n === 1) {
        return $n;
    }

    if ($n > 50) {
        throw new \Exception('Not supported');
    }

    return fibonacci($n - 1) + fibonacci($n - 2);
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Evitar mapas mentales

No fuerces al lector de tu código a traducir lo que significa una variable.
Explicito es mejor que implícito.

**Mal:**

```php
$l = ['Austin', 'New York', 'San Francisco'];

for ($i = 0; $i < count($l); $i++) {
    $li = $l[$i];
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    // Espera, ¿Para qué era `$li`?
    dispatch($li);
}
```

**Bien:**

```php
$locations = ['Austin', 'New York', 'San Francisco'];

foreach ($locations as $location) {
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    dispatch($location);
}
```

**[⬆ volver](#tabla-de-contenidos)**

### No agregar contexto innecesario

Si el nombre de tu clase/objeto te dice algo, no lo repitas en el nombre del atributo.


**Mal:**

```php
class Car
{
    public $carMake;
    public $carModel;
    public $carColor;

    //...
}
```

**Bien:**

```php
class Car
{
    public $make;
    public $model;
    public $color;

    //...
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Usar argumentos por defecto en lugar de cortocircuitos o condicionales

**Nada bien:**

No está bien porque `$breweryName` puede ser `NULL`.

```php
function createMicrobrewery($breweryName = 'Hipster Brew Co.'): void
{
    // ...
}
```

**Nada mal:**

Está opción es más entendible que la versión anterior, pero es mejor controlar el valor de la variable.

```php
function createMicrobrewery($name = null): void
{
    $breweryName = $name ?: 'Hipster Brew Co.';
    // ...
}
```

**Bien:**

Si tienes instalado PHP 7+, entonces puedas usar [implicación de tipos](http://php.net/manual/en/functions.arguments.php#functions.arguments.type-declaration) y así asegurarte de que `$breweryName` no será `NULL`.

```php
function createMicrobrewery(string $breweryName = 'Hipster Brew Co.'): void
{
    // ...
}
```

**[⬆ volver](#tabla-de-contenidos)**

## Funciones

### Argumentos de la función (idealmente 2 o menos)

Limitar la cantidad de parámetros de una función es increíblemente importante porque la hace más fácil de probar. Teniendo más de tres lleva a una explosión de combinaciones que tendrás que probar, argumento por argumento.

El caso ideal es cero argumentos. Uno o dos argumentos están bien, pero tres deben ser evitados. Algo más debe ser dicho. Usualmente, si tienes más de dos argumentos significa que estás intentando hacer demasiado en la función. En los casos en que no, la mayoría del tiempo un objeto de alto nivel será suficiente como argumento.

**Mal:**

```php
function createMenu(string $title, string $body, string $buttonText, bool $cancellable): void
{
    // ...
}
```

**Bien:**

```php
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

function createMenu(MenuConfig $config): void
{
    // ...
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Las funciones deben hacer una cosa

Esta es por lejos una de las más importantes reglas en ingeniería de software. Cuando las funciones hacen más de una cosa, se vuelven difíciles de hacer, probar y razonar sobre ellas. Cuando puedes aislar una función en una sola acción, ellas pueden ser refactorizadas con facilidad y tu código será mucho más limpio de leer. Si esta es la única regla que sigas de esta guía, estarás por sobre muchos desarrolladores.

**Mal:**
```php
function emailClients(array $clients): void
{
    foreach ($clients as $client) {
        $clientRecord = $db->find($client);
        if ($clientRecord->isActive()) {
            email($client);
        }
    }
}
```

**Bien:**

```php
function emailClients(array $clients): void
{
    $activeClients = activeClients($clients);
    array_walk($activeClients, 'email');
}

function activeClients(array $clients): array
{
    return array_filter($clients, 'isClientActive');
}

function isClientActive(int $client): bool
{
    $clientRecord = $db->find($client);

    return $clientRecord->isActive();
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Los nombres de las funciones deben indicar lo que hacen

**Mal:**

```php
class Email
{
    //...

    public function handle(): void
    {
        mail($this->to, $this->subject, $this->body);
    }
}

$message = new Email(...);
// ¿Qué es esto? ¿Un manejador para los mensajes? ¿Ahora escribimos en un archivo?
$message->handle();
```

**Bien:**

```php
class Email 
{
    //...

    public function send(): void
    {
        mail($this->to, $this->subject, $this->body);
    }
}

$message = new Email(...);
// Limpio y obvio
$message->send();
```

**[⬆ volver](#tabla-de-contenidos)**

### Las funciones deben tener sólo un nivel de abstracción

Cuando tienes más de un nivel de abstracción usualmente es porque tu función está haciendo demasiado. Separarlas en funciones lleva a la reutilización y facilita las pruebas.

**Mal:**

```php
function parseBetterJSAlternative(string $code): void
{
    $regexes = [
        // ...
    ];

    $statements = explode(' ', $code);
    $tokens = [];
    foreach ($regexes as $regex) {
        foreach ($statements as $statement) {
            // ...
        }
    }

    $ast = [];
    foreach ($tokens as $token) {
        // lex...
    }

    foreach ($ast as $node) {
        // convertir...
    }
}
```

**También mal:**

Hemos separado algunas de las funcionalidades, pero la función `parseBetterJSAlternative()` todavía es muy compleja e imposible de probar.

```php
function tokenize(string $code): array
{
    $regexes = [
        // ...
    ];

    $statements = explode(' ', $code);
    $tokens = [];
    foreach ($regexes as $regex) {
        foreach ($statements as $statement) {
            $tokens[] = /* ... */;
        }
    }

    return $tokens;
}

function lexer(array $tokens): array
{
    $ast = [];
    foreach ($tokens as $token) {
        $ast[] = /* ... */;
    }

    return $ast;
}

function parseBetterJSAlternative(string $code): void
{
    $tokens = tokenize($code);
    $ast = lexer($tokens);
    foreach ($ast as $node) {
        // convertir...
    }
}
```

**Bien:**

Lo mejor es sacar las dependencias de la función `parseBetterJSAlternative()`.

```php
class Tokenizer
{
    public function tokenize(string $code): array
    {
        $regexes = [
            // ...
        ];

        $statements = explode(' ', $code);
        $tokens = [];
        foreach ($regexes as $regex) {
            foreach ($statements as $statement) {
                $tokens[] = /* ... */;
            }
        }

        return $tokens;
    }
}

class Lexer
{
    public function lexify(array $tokens): array
    {
        $ast = [];
        foreach ($tokens as $token) {
            $ast[] = /* ... */;
        }

        return $ast;
    }
}

class BetterJSAlternative
{
    private $tokenizer;
    private $lexer;

    public function __construct(Tokenizer $tokenizer, Lexer $lexer)
    {
        $this->tokenizer = $tokenizer;
        $this->lexer = $lexer;
    }

    public function parse(string $code): void
    {
        $tokens = $this->tokenizer->tokenize($code);
        $ast = $this->lexer->lexify($tokens);
        foreach ($ast as $node) {
            // convertir...
        }
    }
}
```

**[⬆ volver](#tabla-de-contenidos)**

### No usar banderas como parámetros de funciones

Las banderas le dicen al usuario que la función hace más de una cosa. Las funciones deben hacer sólo una. Divide tus funciones si ellas siguen diferentes caminos basados en un valor booleano.

**Mal:**

```php
function createFile(string $name, bool $temp = false): void
{
    if ($temp) {
        touch('./temp/'.$name);
    } else {
        touch($name);
    }
}
```

**Bien:**

```php
function createFile(string $name): void
{
    touch($name);
}

function createTempFile(string $name): void
{
    touch('./temp/'.$name);
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Evitar efectos secundarios

Una función produce un efecto secundario si hace algo más que tomar un valor y devolver otros. Un efecto secundario puede ser escribir en un archivo, modificar alguna variable global, o accidentalmente darle todo tu dinero a un extraño.

Ahora, ocasionalmente necesitaras los efectos secundarios en un programa. Como los ejemplos anteriores, necesitarás escribir en un archivo. Lo que quieres hacer en esos casos es centralizar donde realizarlos. No tengas muchas funciones y clases que escriban un archivo en particular. Ten un servicio que lo haga. Uno y sólo uno.

El punto principal es evitar trampas comunes como compartir estados entre objetos sin alguna estructura, usar tipos de datos mutables que puedan ser escritos por cualquiera, y no centralizar donde el efecto paralelo ocurre. Si puedes hacerlo, serás más feliz que la vasta mayoría de los demás programadores.

**Mal:**

```php
// Variable global referenciada por la siguiente función.
// Si tenemos otra función que use el mismo nombre, ahora será un arreglo y podría romperla.
$name = 'Ryan McDermott';

function splitIntoFirstAndLastName(): void
{
    global $name;

    $name = explode(' ', $name);
}

splitIntoFirstAndLastName();

var_dump($name); // ['Ryan', 'McDermott'];
```

**Bien:**

```php
function splitIntoFirstAndLastName(string $name): array
{
    return explode(' ', $name);
}

$name = 'Ryan McDermott';
$newName = splitIntoFirstAndLastName($name);

var_dump($name); // 'Ryan McDermott';
var_dump($newName); // ['Ryan', 'McDermott'];
```

**[⬆ volver](#tabla-de-contenidos)**

### No escribir funciones globales

Contaminar los globales es una mala práctica en muchos lenguajes porque puedes chocar con otra librería y el usuario de tu API podría no enterarse hasta obtener una excepción en producción. Pensemos en un ejemplo: qué pasaría si esperabas tener un arreglo de configuración. Podrías escribir una función global como `config()`, pero podría chocar con otra librería que haya intentado hacer lo mismo.

**Mal:**

```php
function config(): array
{
    return  [
        'foo' => 'bar',
    ]
}
```

**Bien:**

```php
class Configuration
{
    private $configuration = [];

    public function __construct(array $configuration)
    {
        $this->configuration = $configuration;
    }

    public function get(string $key): ?string
    {
        return isset($this->configuration[$key]) ? $this->configuration[$key] : null;
    }
}
```

Crea la variable `$configuration` con una instancia de la clase `Configuration` 

```php
$configuration = new Configuration([
    'foo' => 'bar',
]);
```

Y ahora puedes usar una instancia de la clase `Configuration` en tu aplicación.

**[⬆ volver](#tabla-de-contenidos)**

### No usar el patrón Singleton

Singleton es un [anti-patrón](https://en.wikipedia.org/wiki/Singleton_pattern). Citando a Brian Button:
 1. Son usados generalmente como una **instancia global**, ¿Por qué eso es malo? Porque **escondes las dependencias** de tu aplicación en tu código, en lugar de exponerlas mediante interfaces. Hacer algo global para evitar pasarlo es una [hediondez de código](https://en.wikipedia.org/wiki/Code_smell).
 2. Violan el [principio de la responsabilidad única](#principio-de-responsabilidad-única): en virtud del hecho de que **ellos controlan su propia creación y ciclo de vida**.
 3. Inherentemente causan que el código esté estrechamente [acoplado](https://en.wikipedia.org/wiki/Coupling_%28computer_programming%29). Esto hace que muchas veces sean **difíciles de probar**.
 4. Llevan su estado al ciclo de vida de la aplicación. Otro golpe a las pruebas porque **puedes terminar con una situación donde las pruebas necesitan ser ordenadas** lo cual es un gran no para las pruebas unitarias. ¿Por qué? Porque cada prueba unitaria debe hacerse independiente de la otra.

[Misko Hevery](http://misko.hevery.com/about/) ha realizado unas reflexiones interesantes sobre el [origen del problema](http://misko.hevery.com/2008/08/25/root-cause-of-singletons/).

**Mal:**

```php
class DBConnection
{
    private static $instance;

    private function __construct(string $dsn)
    {
        // ...
    }

    public static function getInstance(): DBConnection
    {
        if (self::$instance === null) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    // ...
}

$singleton = DBConnection::getInstance();
```

**Bien:**

```php
class DBConnection
{
    public function __construct(string $dsn)
    {
        // ...
    }

     // ...
}
```
Crea una instancia de la clase `DBConnection` y configúrala con [DSN](http://php.net/manual/en/pdo.construct.php#refsect1-pdo.construct-parameters).

```php
$connection = new DBConnection($dsn);
```

Y ahora debes usar la instancia de `DBConnection` en tu aplicación.

**[⬆ volver](#tabla-de-contenidos)**

### Encapsular condicionales

**Mal:**

```php
if ($article->state === 'published') {
    // ...
}
```

**Bien:**

```php
if ($article->isPublished()) {
    // ...
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Evitar condicionales negativos

**Mal:**

```php
function isDOMNodeNotPresent(\DOMNode $node): bool
{
    // ...
}

if (!isDOMNodeNotPresent($node))
{
    // ...
}
```

**Bien:**

```php
function isDOMNodePresent(\DOMNode $node): bool
{
    // ...
}

if (isDOMNodePresent($node)) {
    // ...
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Evitar condicionales

Esta parece ser una tarea imposible. Al escuchar esto por primera vez, la mayoría de la gente dice, "¿cómo se supone que haré algo sin una declaración `if`?" La respuesta es que la mayoría de las veces puedes usar polimorfismo para lograr el mismo resultado.

La segunda pregunta usualmente es, "bien, eso es genial, ¿pero cómo puedo hacerlo?" La respuesta es un concepto de código limpio que ya hemos aprendido: una función debe hacer sólo una cosa. Cuando tienes clases y funciones que usan declaraciones `if`, estás diciéndole al usuario que tu función hace más de una cosa. Recuerda, hacer sólo una cosa.

**Mal:**

```php
class Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        switch ($this->type) {
            case '777':
                return $this->getMaxAltitude() - $this->getPassengerCount();
            case 'Air Force One':
                return $this->getMaxAltitude();
            case 'Cessna':
                return $this->getMaxAltitude() - $this->getFuelExpenditure();
        }
    }
}
```

**Bien:**

```php
interface Airplane
{
    // ...

    public function getCruisingAltitude(): int;
}

class Boeing777 implements Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        return $this->getMaxAltitude() - $this->getPassengerCount();
    }
}

class AirForceOne implements Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        return $this->getMaxAltitude();
    }
}

class Cessna implements Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        return $this->getMaxAltitude() - $this->getFuelExpenditure();
    }
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Evitar revisión de tipo (parte 1)

PHP es un lenguaje no tipado, lo que quiere decir que tus funciones pueden tener cualquier tipo de argumento.
Algunas veces habrás sentido esta libertad y te habrás tentado a hacer revisión de tipo en tus funciones. Hay muchas maneras de evitar tener que hacerlo.
Lo primero es considerar la consistencia de las APIs.

**Mal:**

```php
function travelToTexas($vehicle): void
{
    if ($vehicle instanceof Bicycle) {
        $vehicle->pedalTo(new Location('texas'));
    } elseif ($vehicle instanceof Car) {
        $vehicle->driveTo(new Location('texas'));
    }
}
```

**Bien:**

```php
function travelToTexas(Traveler $vehicle): void
{
    $vehicle->travelTo(new Location('texas'));
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Evitar revisión de tipo (parte 2)

Si estás trabajando con valores básicos primitivos como cadenas, enteros y arreglos; y estás usando PHP 7+ y no puedes usar polimorfismo pero aún necesitas realizar una revisión de tipo entonces puedes considerar [declaración de tipo](http://php.net/manual/en/functions.arguments.php#functions.arguments.type-declaration) o modo estricto. Esto provee tipado estático sobre la sintaxis estándar de PHP.
El problema con hacer la revisión de tipo manualmente es que requerirá escribir texto adicional que dará una falsa "seguridad de tipado" que no compensará la perdida de legibilidad. Mantén tu código PHP limpio, escribe buenas pruebas, y realiza buenas revisiones.
Dicho de otra forma, realiza todo lo que se ha recomendado pero usando la declaración de tipo estricto en PHP o el modo estricto.

**Mal:**

```php
function combine($val1, $val2): int
{
    if (!is_numeric($val1) || !is_numeric($val2)) {
        throw new \Exception('Must be of type Number');
    }

    return $val1 + $val2;
}
```

**Bien:**

```php
function combine(int $val1, int $val2): int
{
    return $val1 + $val2;
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Quitar código muerto

El código muerto es tan malo como el código duplicado. No hay motivos para mantenerlo en tu código fuente. Si no está siendo llamado, ¡deshazte del! Siempre estará a salvo en tu versión histórica si aún lo necesitas.

**Mal:**

```php
function oldRequestModule(string $url): void
{
    // ...
}

function newRequestModule(string $url): void
{
    // ...
}

$request = newRequestModule($requestUrl);
inventoryTracker('apples', $request, 'www.inventory-awesome.io');
```

**Bien:**

```php
function requestModule(string $url): void
{
    // ...
}

$request = requestModule($requestUrl);
inventoryTracker('apples', $request, 'www.inventory-awesome.io');
```

**[⬆ volver](#tabla-de-contenidos)**


## Objetos y estructuras de datos

### Usar encapsulación de objetos

En PHP puedes establecer métodos como `public`, `protected` y `private`.
Al utilizarlos, puedes controlar las modificaciones a las propiedades de un objeto.

* Cuando quieras hacer algo más que obtener una propiedad de un objeto, no tienes que revisar y cambiar cada método de acceso en tu código fuente.
* Agrega una validación simple cuando haces `set`.
* Encapsula la representación interna.
* Facilita agregar registro y manejo de errores al obtener y colocar.
* Puedes sobrescribir la funcionalidad por defecto al heredar la clase.
* Puedes hacer carga diferida de las propiedades del objeto, por ejemplo, al obtenerlos desde un servidor.

Adicionalmente, esto es parte del [principio abierto/cerrado](#principio-de-abiertocerrado).

**Mal:**

```php
class BankAccount
{
    public $balance = 1000;
}

$bankAccount = new BankAccount();

// Comprar zapatos...
$bankAccount->balance -= 100;
```

**Bien:**

```php
class BankAccount
{
    private $balance;

    public function __construct(int $balance = 1000)
    {
      $this->balance = $balance;
    }

    public function withdraw(int $amount): void
    {
        if ($amount > $this->balance) {
            throw new \Exception('Amount greater than available balance.');
        }

        $this->balance -= $amount;
    }

    public function deposit(int $amount): void
    {
        $this->balance += $amount;
    }

    public function getBalance(): int
    {
        return $this->balance;
    }
}

$bankAccount = new BankAccount();

// Comprar zapatos...
$bankAccount->withdraw($shoesPrice);

// Obtener saldo
$balance = $bankAccount->getBalance();
```

**[⬆ volver](#tabla-de-contenidos)**

### Hacer que los objetos tengan partes private/protected

* Los métodos y propiedades configuradas como `public` son las más expuestas a los cambios, porque algún código externo puede fácilmente modificarlos pero no tienes control sobre qué es lo que se está modificando en verdad. **Modificaciones en clases son peligrosas para usuarios de una clase.**
* El modificador `protected` es tan peligroso como el `public`, porque están disponible en el ámbito de cualquier clase hija. Efectivamente esto significa que la diferencia entre public y protected es el mecanismo de acceso, pero la encapsulación garantiza que siga siendo la misma. **Modificaciones en clases son peligrosas para todas las clases descendientes.**
* El modificador `private` garantiza que el código es **peligroso de modificar sólo en los límites de una clase en particular** (estarás asegurado ante modificaciones y no tendrás un [efecto Jenga](http://www.urbandictionary.com/define.php?term=Jengaphobia&defid=2494196)).

Por lo tanto, usar `private` por defecto y `public/protected` cuando quieras proveer acceso a clases externas.

Para más información, puedes leer el [siguiente articulo](http://fabien.potencier.org/pragmatism-over-theory-protected-vs-private.html) escrito por [Fabien Potencier](https://github.com/fabpot).

**Mal:**

```php
class Employee
{
    public $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }
}

$employee = new Employee('John Doe');
echo 'Employee name: '.$employee->name; // Nombre del empleado: John Doe
```

**Bien:**

```php
class Employee
{
    private $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function getName(): string
    {
        return $this->name;
    }
}

$employee = new Employee('John Doe');
echo 'Employee name: '.$employee->getName(); // Nombre del empleado: John Doe
```

**[⬆ volver](#tabla-de-contenidos)**

## Clases

### Preferir composición antes que herencia

Como se ha dicho en [*Design Patterns*](https://en.wikipedia.org/wiki/Design_Patterns) de the Gang of Four, debes preferir composición sobre herencia cuando puedas hacerlo. Hay muchas buenas razones para usar herencia y muchas buenas razones para usar composición. El punto principal de esta máxima es que si tu mente instintivamente va hacia herencia, trata de pensar si la composición modela tu problema de mejor manera. En algunos casos si es posible.

Debes preguntarte entonces, "¿cuándo debo usar herencia?" Eso depende del problema que tengas entre manos, pero hay una lista decente que indica cuando tiene más sentido usar herencia en lugar de composición.

1. Herencia representa una relación "es-un" y no una relación "tiene-un". (Humano->Animal vs. Usuario->DetallesDeUsuario).
2. Puedes reutilizar código desde las clases padres. (Humanos pueden moverse como todos los animales).
3. Puedes querer hacer cambios globales en las clases derivadas al cambiar una clase padre. (Cambiar el consumo de calorías de todos los animales cuando se mueven)

**Mal:**

```php
class Employee 
{
    private $name;
    private $email;

    public function __construct(string $name, string $email)
    {
        $this->name = $name;
        $this->email = $email;
    }

    // ...
}

// Está mal porque los empleados "tienen" información de impuestos.
// EmployeeTaxData no es un tipo de Employee

class EmployeeTaxData extends Employee 
{
    private $ssn;
    private $salary;
    
    public function __construct(string $name, string $email, string $ssn, string $salary)
    {
        parent::__construct($name, $email);

        $this->ssn = $ssn;
        $this->salary = $salary;
    }

    // ...
}
```

**Bien:**

```php
class EmployeeTaxData 
{
    private $ssn;
    private $salary;

    public function __construct(string $ssn, string $salary)
    {
        $this->ssn = $ssn;
        $this->salary = $salary;
    }

    // ...
}

class Employee 
{
    private $name;
    private $email;
    private $taxData;

    public function __construct(string $name, string $email)
    {
        $this->name = $name;
        $this->email = $email;
    }

    public function setTaxData(string $ssn, string $salary)
    {
        $this->taxData = new EmployeeTaxData($ssn, $salary);
    }

    // ...
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Evitar interfaces fluidas

Una [interfaz fluida](https://en.wikipedia.org/wiki/Fluent_interface) es una API orientada a objetos que ayuda a mejorar la legibilidad del código fuente al usar [encadenamientos de métodos](https://en.wikipedia.org/wiki/Method_chaining).

Pueden haber algunos contextos, frecuentemente los constructores de objetos, donde este patrón reduce la cantidad de texto del código (por ejemplo el [Constructor de Simulación de PHPUnit](https://phpunit.de/manual/current/en/test-doubles.html) o [Constructor de Consultas Doctrine](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/query-builder.html)), pero a menudo esto viene con algunos costos:

1. Rompe la [encapsulación](https://en.wikipedia.org/wiki/Encapsulation_%28object-oriented_programming%29)
2. Rompe los [decoradores](https://en.wikipedia.org/wiki/Decorator_pattern)
3. Es difícil de [simular](https://en.wikipedia.org/wiki/Mock_object) en un conjunto de pruebas.
4. Dificulta la lectura de los diffs de commits.

Para más información, puedes leer el [articulo completo](https://ocramius.github.io/blog/fluent-interfaces-are-evil/) escrito por [Marco Pivetta](https://github.com/Ocramius).

**Mal:**

```php
class Car
{
    private $make = 'Honda';
    private $model = 'Accord';
    private $color = 'white';

    public function setMake(string $make): self
    {
        $this->make = $make;

        // Observación: Retornará esto para encadenar.
        return $this;
    }

    public function setModel(string $model): self
    {
        $this->model = $model;

        // Observación: Retornará esto para encadenar.
        return $this;
    }

    public function setColor(string $color): self
    {
        $this->color = $color;

        // Observación: Retornará esto para encadenar.
        return $this;
    }

    public function dump(): void
    {
        var_dump($this->make, $this->model, $this->color);
    }
}

$car = (new Car())
  ->setColor('pink')
  ->setMake('Ford')
  ->setModel('F-150')
  ->dump();
```

**Bien:**

```php
class Car
{
    private $make = 'Honda';
    private $model = 'Accord';
    private $color = 'white';

    public function setMake(string $make): void
    {
        $this->make = $make;
    }

    public function setModel(string $model): void
    {
        $this->model = $model;
    }

    public function setColor(string $color): void
    {
        $this->color = $color;
    }

    public function dump(): void
    {
        var_dump($this->make, $this->model, $this->color);
    }
}

$car = new Car();
$car->setColor('pink');
$car->setMake('Ford');
$car->setModel('F-150');
$car->dump();
```

**[⬆ volver](#tabla-de-contenidos)**

## SOLID

**SOLID** es un acrónimo mnemotécnico creado por Michael Feathers para los primeros cinco principios nombrados por Robert Martin, lo que significa los cinco principios básicos de la programación y diseño orientado a objetos.

 * [S: Principio de responsabilidad única (SRP)](#principio-de-responsabilidad-única)
 * [O: Principio de abierto/cerrado (OCP)](#principio-de-abiertocerrado)
 * [L: Principio de la sustitución de Liskov (LSP)](#principio-de-la-sustitución-de-liskov)
 * [I: Principio de la segregación de la interfaz (ISP)](#principio-de-segregación-de-la-interfaz)
 * [D: Principio de la inversión de dependencia (DIP)](#principio-de-la-inversión-de-dependencia)

### Principio de responsabilidad única

Como se ha dicho en Clean Code, "No debe haber nunca más de un motivo para que una clase cambie". Es tentador empaquetar una clase con muchas funcionalidades, como si sólo pudieras llevar una maleta en tu vuelo. El problema es que esa clase no será conceptualmente cohesiva y te dará muchas razones para cambiarla. Minimizar la cantidad de veces que necesitas realizar cambios a una clase es importante. Es importante porque hay demasiadas funcionalidades en una sola clase y modificas una parte de ella, será difícil entender cómo afectará a otros módulos que dependan de ella en tu código fuente.

**Mal:**

```php
class UserSettings
{
    private $user;

    public function __construct(User $user)
    {
        $this->user = $user;
    }

    public function changeSettings(array $settings): void
    {
        if ($this->verifyCredentials()) {
            // ...
        }
    }

    private function verifyCredentials(): bool
    {
        // ...
    }
}
```

**Bien:**

```php
class UserAuth 
{
    private $user;

    public function __construct(User $user)
    {
        $this->user = $user;
    }
    
    public function verifyCredentials(): bool
    {
        // ...
    }
}

class UserSettings 
{
    private $user;
    private $auth;

    public function __construct(User $user) 
    {
        $this->user = $user;
        $this->auth = new UserAuth($user);
    }

    public function changeSettings(array $settings): void
    {
        if ($this->auth->verifyCredentials()) {
            // ...
        }
    }
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Principio de abierto/cerrado

Como dijo Bertrand Meyer, "las entidades de software (clases, módulos, funciones, etc) deben ser abiertas para ser extendidas, pero cerradas para modificarlas." ¿Qué significa esto? Este principio establece básicamente que puedes permitir a los usuarios agregar nuevas funcionalidades pero sin cambiar el código existente.

**Mal:**

```php
abstract class Adapter
{
    protected $name;

    public function getName(): string
    {
        return $this->name;
    }
}

class AjaxAdapter extends Adapter
{
    public function __construct()
    {
        parent::__construct();

        $this->name = 'ajaxAdapter';
    }
}

class NodeAdapter extends Adapter
{
    public function __construct()
    {
        parent::__construct();

        $this->name = 'nodeAdapter';
    }
}

class HttpRequester
{
    private $adapter;

    public function __construct(Adapter $adapter)
    {
        $this->adapter = $adapter;
    }

    public function fetch(string $url): Promise
    {
        $adapterName = $this->adapter->getName();

        if ($adapterName === 'ajaxAdapter') {
            return $this->makeAjaxCall($url);
        } elseif ($adapterName === 'httpNodeAdapter') {
            return $this->makeHttpCall($url);
        }
    }

    private function makeAjaxCall(string $url): Promise
    {
        // Solicitar y retornar una promesa
    }

    private function makeHttpCall(string $url): Promise
    {
        // Solicitar y retornar una promesa
    }
}
```

**Bien:**

```php
interface Adapter
{
    public function request(string $url): Promise;
}

class AjaxAdapter implements Adapter
{
    public function request(string $url): Promise
    {
        // Solicitar y retornar una promesa
    }
}

class NodeAdapter implements Adapter
{
    public function request(string $url): Promise
    {
        // Solicitar y retornar una promesa
    }
}

class HttpRequester
{
    private $adapter;

    public function __construct(Adapter $adapter)
    {
        $this->adapter = $adapter;
    }

    public function fetch(string $url): Promise
    {
        return $this->adapter->request($url);
    }
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Principio de la sustitución de Liskov

Este es un término aterrador para un concepto muy simple. Se define formalmente como "Si S es un subtipo de T, entonces los objetos de tipo T pueden ser reemplazados con objetos de tipo S (es decir, objetos de tipo S pueden sustituir objetos de tipo T) sin alterar alguna propiedad deseable de ese programa (exactitud, tarea realizada, etc)." Esa es una definición aún más aterradora.

La mejor explicación para esto es que si tienes una clase padre y una clase hija, entonces la clase padre y la clase hija pueden intercambiarse sin obtener resultados incorrectos. Esto puede ser confuso, así que veamos el clásico ejemplo del Cuadrado-Rectángulo. Matemáticamente, un cuadrado es un Rectángulo, pero si tu modelo está usando la relación "es-un" por herencia, rápidamente estarás en problemas.

**Mal:**

```php
class Rectangle
{
    protected $width = 0;
    protected $height = 0;

    public function render(int $area): void
    {
        // ...
    }

    public function setWidth(int $width): void
    {
        $this->width = $width;
    }

    public function setHeight(int $height): void
    {
        $this->height = $height;
    }

    public function getArea(): int
    {
        return $this->width * $this->height;
    }
}

class Square extends Rectangle
{
    public function setWidth(int $width): void
    {
        $this->width = $this->height = $width;
    }

    public function setHeight(int $height): void
    {
        $this->width = $this->height = $height;
    }
}

/**
 * @param Rectangle[] $rectangles
 */
function renderLargeRectangles(array $rectangles): void
{
    foreach ($rectangles as $rectangle) {
        $rectangle->setWidth(4);
        $rectangle->setHeight(5);
        $area = $rectangle->getArea(); // MAL: Retornará 25 para cuadrados y debería ser 20
        $rectangle->render($area);
    }
}

$rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles($rectangles);
```

**Bien:**

```php
abstract class Shape
{
    protected $width = 0;
    protected $height = 0;

    abstract public function getArea(): int;

    public function render(int $area): void
    {
        // ...
    }
}

class Rectangle extends Shape
{
    public function setWidth(int $width): void
    {
        $this->width = $width;
    }

    public function setHeight(int $height): void
    {
        $this->height = $height;
    }

    public function getArea(): int
    {
        return $this->width * $this->height;
    }
}

class Square extends Shape
{
    private $length = 0;

    public function setLength(int $length): void
    {
        $this->length = $length;
    }

    public function getArea(): int
    {
        return pow($this->length, 2);
    }
}

/**
 * @param Rectangle[] $rectangles
 */
function renderLargeRectangles(array $rectangles): void
{
    foreach ($rectangles as $rectangle) {
        if ($rectangle instanceof Square) {
            $rectangle->setLength(5);
        } elseif ($rectangle instanceof Rectangle) {
            $rectangle->setWidth(4);
            $rectangle->setHeight(5);
        }

        $area = $rectangle->getArea(); 
        $rectangle->render($area);
    }
}

$shapes = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles($shapes);
```

**[⬆ volver](#tabla-de-contenidos)**

### Principio de segregación de la interfaz

Este principio establece que "Los clientes no deben forzar la dependencia sobre interfaces que no utilizan".

Un buen ejemplo a considerar para demostrar este principio son las clases que requieren objetos de configuración grandes. No requerir clientes con una gran cantidad de opciones de configuración es beneficioso, porque la mayoría del tiempo no necesitará todas las configuraciones. Hacerlas opcional ayuda a prevenir una "interfaz pesada".

**Mal:**

```php
interface Employee
{
    public function work(): void;

    public function eat(): void;
}

class Human implements Employee
{
    public function work(): void
    {
        // ....trabajando
    }

    public function eat(): void
    {
        // ...... comiendo en la hora de almuerzo
    }
}

class Robot implements Employee
{
    public function work(): void
    {
        //.... trabajando mucho más
    }

    public function eat(): void
    {
        //.... los robots no pueden comer, pero debe implementarse este método
    }
}
```

**Bien:**

No todos los trabajadores son empleados, pero cada empleado es un trabajador.

```php
interface Workable
{
    public function work(): void;
}

interface Feedable
{
    public function eat(): void;
}

interface Employee extends Feedable, Workable
{
}

class Human implements Employee
{
    public function work(): void
    {
        // ....trabajando
    }

    public function eat(): void
    {
        //.... comiendo en la hora de almuerzo
    }
}

// Los robots solo pueden trabajar
class Robot implements Workable
{
    public function work(): void
    {
        // ....trabajando
    }
}
```

**[⬆ volver](#tabla-de-contenidos)**

### Principio de la inversión de dependencia

Este principio establece dos cosas esenciales:
1. Los módulos de alto nivel no deben depender de los de bajo nivel. Ambos deben depender de abstracciones.
2. Las abstracciones no deben depender de detalles. Los detalles deben depender de abstracciones.

Esto puede ser difícil de entender al comienzo, pero si has trabajado con frameworks de PHP (como Symfony), habrás visto alguna implementación de este principio en forma de Inyección de Dependencia (DI). Cuando no hay conceptos idénticos, este principio mantiene en conocimiento a los módulos de alto nivel sobre los módulos de bajo nivel y los configura.
Esto se logra mediante la inyección de dependencia. Un gran beneficio de esto es la reducción del acoplamiento entre módulos. El acoplamiento es un patrón de desarrollo muy malo porque hace que el código sea difícil de refactorizar.

**Mal:**

```php
class Employee
{
    public function work(): void
    {
        // ....trabajando
    }
}

class Robot extends Employee
{
    public function work(): void
    {
        //.... trabajando mucho más
    }
}

class Manager
{
    private $employee;

    public function __construct(Employee $employee)
    {
        $this->employee = $employee;
    }

    public function manage(): void
    {
        $this->employee->work();
    }
}
```

**Bien:**

```php
interface Employee
{
    public function work(): void;
}

class Human implements Employee
{
    public function work(): void
    {
        // ....trabajando
    }
}

class Robot implements Employee
{
    public function work(): void
    {
        //.... trabajando mucho más
    }
}

class Manager
{
    private $employee;

    public function __construct(Employee $employee)
    {
        $this->employee = $employee;
    }

    public function manage(): void
    {
        $this->employee->work();
    }
}
```

**[⬆ volver](#tabla-de-contenidos)**

## No te repitas

Intenta observar el principio [no te repitas.](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)

Haz tu mejor esfuerzo en evitar código duplicado. Duplicar código está mal porque significa que hay más de un lugar para modificar algo si necesitas cambiar alguna lógica.

Imagina si tienes un restaurant y mantienes seguimiento de tu inventario: todos tus tomates, cebollas, ajos, especias, etc. Si tienes múltiples listas que mantienen esto, entonces tienes que actualizarlas cuando sirves un plato con tomates en él. Si solo tienes una lista, ¡Hay un solo lugar que actualizar!

A menudo tienes código duplicado porque tienes dos o más cosas ligeramente diferentes, que comparten mucho en común, pero sus diferencias te fuerzan a tener dos o más funciones separadas haciendo mucho de lo mismo. Remover código duplicado significa crear una abstracción que puedan manejar diferentes conjuntos de cosas en una función/modulo/clase.

Lograr una correcta abstracción es crítico, esto porque deberás seguir los principios SOLID dispuestos en la sección [clases](#clases). Malas abstracciones pueden ser peor que el código duplicado, ¡así que ten cuidado! Dicho esto, si puedes hacer buenas abstracciones, ¡Hazlo! No te repitas, de otra manera te encontrarás actualizando muchos lugares cuando necesites cambiar una cosa.

**Mal:**

```php
function showDeveloperList(array $developers): void
{
    foreach ($developers as $developer) {
        $expectedSalary = $developer->calculateExpectedSalary();
        $experience = $developer->getExperience();
        $githubLink = $developer->getGithubLink();
        $data = [
            $expectedSalary,
            $experience,
            $githubLink
        ];

        render($data);
    }
}

function showManagerList(array $managers): void
{
    foreach ($managers as $manager) {
        $expectedSalary = $manager->calculateExpectedSalary();
        $experience = $manager->getExperience();
        $githubLink = $manager->getGithubLink();
        $data = [
            $expectedSalary,
            $experience,
            $githubLink
        ];

        render($data);
    }
}
```

**Bien:**

```php
function showList(array $employees): void
{
    foreach ($employees as $employee) {
        $expectedSalary = $employee->calculateExpectedSalary();
        $experience = $employee->getExperience();
        $githubLink = $employee->getGithubLink();
        $data = [
            $expectedSalary,
            $experience,
            $githubLink
        ];

        render($data);
    }
}
```

**Muy bien:**

Es mejor usar una versión compacta del código.

```php
function showList(array $employees): void
{
    foreach ($employees as $employee) {
        render([
            $employee->calculateExpectedSalary(),
            $employee->getExperience(),
            $employee->getGithubLink()
        ]);
    }
}
```

**[⬆ volver](#tabla-de-contenidos)**

## Traducciones

Disponible en muchos otros idiomas:

*  :cn: **Chino:**
   * [php-cpm/clean-code-php](https://github.com/php-cpm/clean-code-php)
* :ru: **Ruso:**
   * [peter-gribanov/clean-code-php](https://github.com/peter-gribanov/clean-code-php)
* :brazil: **Portugues:**
   * [fabioars/clean-code-php](https://github.com/fabioars/clean-code-php)
   * [jeanjar/clean-code-php](https://github.com/jeanjar/clean-code-php/tree/pt-br)
* :thailand: **Tailandes:**
   * [panuwizzle/clean-code-php](https://github.com/panuwizzle/clean-code-php)

**[⬆ volver](#tabla-de-contenidos)**
