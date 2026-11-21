# Dukstra Nova Phone Field

A Laravel Nova 5 phone field with country-specific input masks and optional
custom formats. This is a Nova 5 / Vue 3 upgrade of `bissolli/nova-phone-field`.

## Requirements

- PHP 8.3+
- Laravel 13
- Laravel Nova 5

## Install

```sh
composer require dukstra/nova-phone-field
```

## Use

```php
use Dukstra\NovaPhoneField\PhoneNumber;

PhoneNumber::make('Phone Number', 'phone_number')
    ->onlyCountries('ZA', 'ZW', 'US')
    ->withCustomFormats('+27 ## ### ####', '+263 ## ### ####');
```

`onlyCountries()` limits the built-in masks presented by the input. If omitted,
all bundled country masks are available. `withCustomFormats()` adds the given
mask strings, using `#` for a digit. To use only those custom masks, chain
`onlyCustomFormats()` as well. The field keeps the submitted value in the same
format the user entered; it does not convert values to E.164.

The index and detail views link non-empty values using a `tel:` URL with
non-digit characters removed. The visible phone value remains unchanged.

## Build assets

Nova 5's field assets use Vue 3 and Laravel Nova Devtool. After installing the
Composer dependencies (including dev dependencies) and NPM dependencies, run:

```sh
npm ci
npm run production
```

## License

MIT. See [LICENSE](LICENSE).
