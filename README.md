Laravel 5 Eloquent Hashids
==========================
Laravel 5 Eloquent Model trait for automatically generating **and persistently saving** [Hashids](http://hashids.org) for new models.  
Uses [@vinkla's Laravel 5 Hashids package](https://github.com/vinkla/hashids).

[![Latest Stable Version](http://img.shields.io/packagist/v/naabster/laravel-eloquent-hashids.svg?style=flat)](https://packagist.org/packages/naabster/laravel-eloquent-hashids)
[![License](https://img.shields.io/packagist/l/naabster/laravel-eloquent-hashids.svg?style=flat)](https://packagist.org/packages/naabster/laravel-eloquent-hashids)


## Installation
Require this package, with [Composer](https://getcomposer.org/), in the root directory of your project.

```bash
composer require naabster/laravel-eloquent-hashids:~1.0
```

You should first install [@vinkla's Laravel 5 Hashids package](https://github.com/vinkla/hashids) according to their installation instructions.

## Usage

Use the `EloquentHashids` trait on your model:

```php

use Illuminate\Database\Eloquent\Model;
use Naabster\EloquentHashids\EloquentHashids;

class Book extends Model
{

  use EloquentHashids;

  // Rest of your eloquent model
}
```


## Configuration

By default the following configuration is used:

#### Hashids Connection

The default Hashids Connection is `table.<tablename>`, so for example for the `Book` model it would be `table.books`.
 You can set a different connection for your model with `static::getHashidConnection()`:

```php

use Illuminate\Database\Eloquent\Model;
use Naabster\EloquentHashids\EloquentHashids;

class Book extends Model
{

  use EloquentHashids;

  public static function getHashidConnection(Model $model)
	{
	  return 'someconnection';
	}
}
```

Make sure you set your connections in your `config/hashids.php` config file (comes with @vinkla's Laravel 5 Hashids package)!

#### Column Name

By default the Hashid column is named `uid`. You can set a different column name for your model with `static::getHashidColumn()`:

```php

use Illuminate\Database\Eloquent\Model;
use Naabster\EloquentHashids\EloquentHashids;

class Book extends Model
{

  use EloquentHashids;

  public static function getHashidColumn(Model $model)
	{
	  return 'hashid';
	}
}
```

**Attention**

This package generates Hashids for new models and saves them to a column in your database table! To be able to do that, this column must exist in your table.  
You should add this column to your model with a [database migration](https://laravel.com/docs/5.3/migrations).

#### Encoding Value

By default the Value/Number for your Hashid is the `primary Key` attribute of your model (defaults to `id`). You can adjust that any way you 
want, just make sure it is `Hashids` compatible. Just return another value from `static::getHashidEncodingValue()`:

```php

use Illuminate\Database\Eloquent\Model;
use Naabster\EloquentHashids\EloquentHashids;

class Book extends Model
{

  use EloquentHashids;

  public static function getHashidEncodingValue(Model $model)
	{
	  return $model->myId;
	}
}
```


## Example

In this example we generate `Hashids` with the connection **bookHashids**, for column **hashid** and with an encoding 
value of an **array**:

```php

use Illuminate\Database\Eloquent\Model;
use Naabster\EloquentHashids\EloquentHashids;

class Book extends Model
{

  use EloquentHashids;

  public static function getHashidConnection(Model $model)
	{
	  return 'bookHashids';
	}
	
  public static function getHashidColumn(Model $model)
	{
	  return 'hashid';
	}

  public static function getHashidEncodingValue(Model $model)
	{
	  return [$model->id, $model->foreign_key];
	}
}
```

## License

The Laravel 5 Eloquent Hashids package is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT).
