# Asynchronous SOAP client

[![codecov.io](https://codecov.io/github/meng-tian/async-soap-guzzle/coverage.svg?branch=master)](https://codecov.io/github/meng-tian/async-soap-guzzle?branch=master) ![workflow](https://github.com/meng-tian/async-soap-guzzle/actions/workflows/main.yaml/badge.svg)

An asynchronous SOAP client build on top of Guzzle. The `SoapClient` implements [meng-tian/php-async-soap](https://github.com/meng-tian/php-async-soap).

## Requirement

PHP 8.2 or newer with `ext-soap`, and Guzzle 7. PHP 7.x and PHP 8.0-8.1 remain
available only through older releases.

## Install
```
composer require meng-tian/async-soap-guzzle:^0.4.2
```

## Usage
An instance of `Psr\Http\Message\RequestFactoryInterface` and an instance of
`Psr\Http\Message\StreamFactoryInterface` are injected into
`Meng\AsyncSoap\Guzzle\Factory`. These interfaces are defined by
[PSR-17](https://www.php-fig.org/psr/psr-17/) and create
[PSR-7](https://www.php-fig.org/psr/psr-7/) HTTP messages. Choose any PSR-17
implementation, such as `laminas/laminas-diactoros`.

1. Install a PSR-17 implementation if your application does not already provide one:
```json
...
    "require": {
        "php": "^8.2",
        "meng-tian/async-soap-guzzle": "^0.4.2",
        "laminas/laminas-diactoros": "^3.8"
    },
...
```
`laminas/laminas-diactoros` can be replaced by another PSR-17
implementation.
2. Run `composer install`

3. Create your async SOAP client and call your SOAP messages:
```php
use GuzzleHttp\Client;
use Meng\AsyncSoap\Guzzle\Factory;
use Laminas\Diactoros\RequestFactory;
use Laminas\Diactoros\StreamFactory;

$factory = new Factory();
$client = $factory->create(new Client(), new StreamFactory(), new RequestFactory(), 'http://www.webservicex.net/Statistics.asmx?WSDL');

// async call
$promise = $client->callAsync('GetStatistics', [['X' => [1,2,3]]]);
$result = $promise->wait();

// sync call
$result = $client->call('GetStatistics', [['X' => [1,2,3]]]);

// magic method
$promise = $client->GetStatistics(['X' => [1,2,3]]);
$result = $promise->wait();
```

## Testing

```sh
composer install
vendor/bin/phpunit
```

## License
This library is released under [MIT](https://github.com/meng-tian/async-soap-guzzle/blob/master/LICENSE) license.
