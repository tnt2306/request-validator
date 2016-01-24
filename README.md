# PHP Request Validator

Easy PHP validation

## Install
Coming soon on composer :)

## Available rules
- [x]  accepted
- [x]  alpha
- [x]  between
- [x]  boolean
- [x]  dateFormat
- [x]  email
- [x]  json
- [x]  image
- [x]  ip
- [x]  max
- [x]  min
- [x]  numeric
- [x]  required
- [x]  same
- [x]  unique (db connection required)
- [x]  url

### Examples
```php
# Create new Validator, pass data, define rules and custom messages
# Also has errors messages by default
$validator = Validator::make($_POST, [
    'firstname, lastname' => 'required|alpha|min:2',
    'lastname'            => 'max:18',
    'email'               => 'email|unique:users', //table in db
    'age'                 => 'min:16|numeric',
    'date'                => 'dateFormat:(m-Y.d H:i)', //any format you set
    'rule'                => 'accepted',
    'randNum'             => 'between:1, 100',
    'ip'                  => 'ip',
    'password'            => 'required|min:6',
    'password_repeat'     => 'same:password',
    'json'                => 'json',
    'site'                => 'url',
], [
   'email.required'     => 'Email is required',
   'email.email'        => 'Email has bad format',
   'email.unique'       => 'Email is not unique',
]);
```


### Advanced Usage
----

#### Setup ORM system and use (only for unique rule)
```php
use Progsmile\Validator\DbProviders\PhalconORM;

Validator::setupDbProvider(PhalconORM::class); // Phalcon ORM comes from the box

$validator = Validator::make($this->request->getPost(), [
    'password'        => 'min:6',
    'password_repeat' => 'same:password',
    'json'            => 'json'
]);

```


#### Formatting - the best way to auto-reformat the returned array into your own style

The `$validator->format()`, by default, the messages will be formatted to html `<ul><li></li>...</ul>` element.

You can create your own class to format the array `$validator->messages()` into a well formed result.

```php
use Progsmile\Validator\Contracts\Format\FormatInterface;

class MarkdownFormatter implements FormatInterface
{
    public function reformat($messages)
    {
        $ret = '#### Error Found';

        foreach ($messages as $field => $message) {

            foreach ($message as $content) {

                $ret .= "- [x] ".$content."**\n"
            }
        }

        return $ret;
    }
}
```

Then in to use this call, you must do this way:

```php
$validator = Validator::make(
    # ... some code here...
);

echo $validator->format(MarkdownFormatter::class);
```

----

Project is just started and it is not stable yet, we love to have your fork requests.
