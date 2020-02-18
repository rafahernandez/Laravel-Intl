# Laravel Intl

[![Build Status](https://api.travis-ci.com/rafahernandez/Laravel-Intl.svg?branch=master)](https://travis-ci.com/rafahernandez/Laravel-Intl)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/rafahernandez/Laravel-Intl/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/rafahernandez/Laravel-Intl/?branch=master)
[![Code Coverage](https://scrutinizer-ci.com/g/rafahernandez/Laravel-Intl/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/rafahernandez/Laravel-Intl/?branch=master)
[![Latest Stable Version](https://poser.pugx.org/rafahernandez/laravel-intl/v/stable)](https://packagist.org/packages/rafahernandez/laravel-intl)
[![Total Downloads](https://poser.pugx.org/rafahernandez/laravel-intl/downloads)](https://packagist.org/packages/rafahernandez/laravel-intl)
[![License](https://poser.pugx.org/rafahernandez/laravel-intl/license)](https://packagist.org/packages/rafahernandez/laravel-intl)

> This is a fork of the archived package by Propaganistas 

Easy to use internationalization functions for Laravel 5 and Lumen based on various libraries for easy retrieval of
localized values and formatting of numeric values into their localized patterns.

### Overview

* [Installation](#installation)
* [Usage](#usage)
    * [Country](#country)
    * [Currency](#currency)
    * [Date](#date)
    * [Language](#language)
    * [Number](#number)
* [Changing locales](#changing-locales)
    
### Installation

Run the following command to install the latest version of the package

```bash
composer require rafahernandez/laravel-intl
```

#### Laravel
If you don't use auto-discovery, open up your app config and add the Service Provider to the `$providers` array:

 ```php
'providers' => [
    ...
    rafahernandez\LaravelIntl\IntlServiceProvider::class,
],
```

#### Lumen
In `bootstrap/app.php`, register the Service Provider

 ```php
$app->register(RafaHernandez\LaravelIntl\IntlServiceProvider::class);
```

### Usage

> Note: **always** use the helper functions or Facades, or make use of dependency injection.

#### Country

Output localized country names.
```php
use RafaHernandez\LaravelIntl\Facades\Country;

// Application locale: nl
Country::name('US'); // Verenigde Staten
Country::all(); // ['US' => 'Verenigde Staten', 'BE' => 'België', ...]
```

```php
// Application locale: en
country('US'); // United States
country()->all(); // ['US' => 'United States', 'BE' => 'Belgium', ...]
```

#### Currency

Output localized currency names and format currency amounts into their localized pattern.

```php
use RafaHernandez\LaravelIntl\Facades\Currency;

// Application locale: nl
Currency::name('USD'); // Amerikaanse dollar
Currency::symbol('USD'); // $
Currency::format(1000, 'USD'); // $ 1.000,00
Currency::formatAccounting(-1234, 'USD'); // (US$ 1.234,00)
Currency::all(); // ['USD' => 'Amerikaanse dollar', 'EUR' => 'Euro', ...]
```

```php
// Application locale: en
currency('USD'); // US Dollar
currency()->symbol('USD'); // $
currency(1000, 'USD'); // $1,000.00
currency()->all(); // ['USD' => 'US Dollar', 'EUR' => 'Euro', ...]
```

Parse localized values into native PHP numbers.

```php
use RafaHernandez\LaravelIntl\Facades\Currency;

// Application locale: nl
Currency::parse('€ 1.234,50'); // 1234.5
```

```php
// Application locale: nl
currency()->parse('€ 1.234,50'); // 1234.5
```

#### Date

Just use `Illuminate\Support\Facades\Date`.

Additional methods are also available to output localized common date formats. E.g. `toShortDateString()`:

* Locale "en": 2/18/20
* Locale "es": 18/2/20

````php
$date = Carbon\Carbon::parse("2020-02-18T19:13:24+00:00");

$date->toShortDateString();  // es: "18/2/20" - en: "2/18/20"
$date->toMediumDateString(); // es: "18 feb. 2020" - en: "Feb 18, 2020"
$date->toLongDateString();   // es: "18 de febrero de 2020"  - en: "February 18, 2020"
$date->toFullDateString();   // es: "martes, 18 de febrero de 2020" - en: "Tuesday, February 18, 2020"

$date->toShortTimeString();  // es: "19:13" - en: "7:13 PM"
$date->toMediumTimeString(); // es: "19:13:24" - en: "7:13:24 PM"
$date->toLongTimeString();   // es: "19:13:24 GMT+0"  - en: "7:13:24 PM GMT+0"
$date->toFullTimeString();   // es: "19:13:24 (GMT+00:00)" - en: "7:13:24 PM GMT+00:00"

$date->toShortDatetimeString();  // es: "18/2/20 19:13" - en: "2/18/20, 7:13 PM"
$date->toMediumDatetimeString(); // es: "18 feb. 2020 19:13:24" - en: "Feb 18, 2020, 7:13:24 PM"
$date->toLongDatetimeString();   // es: "18 de febrero de 2020, 19:13:24 GMT+0" - en: "February 18, 2020 at 7:13:24 PM GMT+0"
$date->toFullDatetimeString();   // es: "martes, 18 de febrero de 2020, 19:13:24 (GMT+00:00)" - en: "Tuesday, February 18, 2020 at 7:13:24 PM GMT+00:00"
````

#### Language

Output localized language names.

```php
use RafaHernandez\LaravelIntl\Facades\Language;

// Application locale: nl
Language::name('en'); // Engels
Language::all(); // ['en' => 'Engels', 'nl' => 'Nederlands', ...]
```

```php
// Application locale: en
language('en'); // English
language()->all(); // ['en' => 'English', 'nl' => 'Dutch', ...]
```

#### Number

Output localized numeric values into their localized pattern.

```php
use RafaHernandez\LaravelIntl\Facades\Number;

// Application locale: en
Number::format(1000); // '1,000'
Number::percent('0.75'); // '75%'
```

```php
// Application locale: fr
number(1000); // '1 000'
number()->percent('0.75'); // '75 %'
```

Parse localized values into native PHP numbers.

```php
use RafaHernandez\LaravelIntl\Facades\Number;

// Application locale: fr
Number::parse('1 000'); // 1000
number()->parse('1,5'); // 1.5
```

### Changing locales

Ever feel the need to use a locale other than the current application locale? You can temporarily use another locale by using the `usingLocale()` method.

```php
country()->name('US'); // United States

country()->usingLocale('nl', function($country) {
    return $country->name('US');
}); // Verenigde Staten

country()->name('US'); // United States
```

Alternatively, you can force each component individually to the preferred locale for the remainder of the application by calling the `setLocale()` on the helper function or Facade.
Usually you'd set this in the `boot()` method of a *ServiceProvider*.
