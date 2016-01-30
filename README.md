# Authorization header fix

Using a Symfony application on Apache, you get `null` when trying to get Authorization header with `$request->headers->get('Authorization');`.

The why is explained in [this Stackoverflow question](http://stackoverflow.com/questions/19443718/symfony-2-3-getrequest-headers-not-showing-authorization-bearer-token).

So if you don't want to patch your .htaccess, this library, inspired by [fschmengler's answer in this another SO question](http://stackoverflow.com/questions/11990388/request-headers-bag-is-missing-authorization-header-in-symfony-2),
provides a listener which adds the Authorization header to a Symfony Request instance.


## Installation

### Download

Using Composer:

``` js
{
    "require": {
        "alcalyn/authorization-header-fix": "1.0.x"
    }
}
```


### Register listener

To fix all Requests in a full stack Symfony app, register a listener like this:

``` yml
# app/config/services.yml
services:
    acme.listeners.authorization_header_fix:
        class: Alcalyn\AuthorizationHeaderFix\AuthorizationHeaderFixListener
        tags:
            - { name: kernel.event_listener, event: kernel.request, priority: 10 }
```

Or using Silex:

``` php
$this->on('kernel.request', array(
    new Alcalyn\AuthorizationHeaderFix\AuthorizationHeaderFixListener(),
    'onKernelRequest'
), 10);
```

> Note:
>
> An higher priority is recommended to have the Authorization header
> available in others listeners.


## License

This library is under [MIT License](LICENSE).
