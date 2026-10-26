Model
=====

[![Build Status](http://img.shields.io/travis/jenssegers/model.svg)](https://travis-ci.org/jenssegers/model) [![Coverage Status](http://img.shields.io/coveralls/jenssegers/model.svg)](https://coveralls.io/r/jenssegers/model)

This model provides an Laravel eloquent-like base class that can be used to build custom models in Laravel or other frameworks.

Features
--------

 - Accessors and mutators
 - Model to Array and JSON conversion
 - Hidden attributes in Array/JSON conversion
 - Guarded and fillable attributes
 - Appending accessors and mutators to Array/JSON conversion
 - Attribute casting

You can read more about these features and the original Eloquent model on http://laravel.com/docs/eloquent

Installation
------------

This is AryeoHQ's fork of `jenssegers/model`, with Laravel 13 support. It is published to the
Aryeo Composer repository in Artifactory, not Packagist, so add that repository first:

```bash
composer config repositories.aryeo '{"type": "composer", "url": "https://artifactory.zgtools.net/artifactory/api/composer/listing-and-vendor-management-composer/", "only": ["aryeo/*", "zillow/*"]}'
composer require aryeo/model
```

The package replaces `jenssegers/model`, so packages that require the upstream name get this
fork instead.

Example
-------

```php

use Jenssegers\Model\Model;

class User extends Model {

    protected $hidden = ['password'];

    protected $guarded = ['password'];

    protected $casts = ['age' => 'integer'];

    public function save()
    {
        return API::post('/items', $this->attributes);
    }

    public function setBirthdayAttribute($value)
    {
        $this->attributes['birthday'] = strtotime($value);
    }

    public function getBirthdayAttribute($value)
    {
        return new DateTime("@$value");
    }

    public function getAgeAttribute($value)
    {
        return $this->birthday->diff(new DateTime('now'))->y;
    }
}

$item = new User(array('name' => 'john'));
$item->password = 'bar';

echo $item; // {"name":"john"}
```
