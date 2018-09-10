# Route Middleware

Middleware allow you to modify the request and/or response before and/or after it reaches the route handler. Middleware can be any of the following:
1. the class name of a class that implements `MiddlewareInterface` (see `src/Middleware/MiddlewareInterface`). This is the recommended way of defining middleware.
1. any anonymous function
1. an instance of a class  that implements `MiddlewareInterface`

A common example for middleware usage is protecting certain routes to be accessible by logged in users only:

```php
class AuthenticationMiddleware implements \WPEmerge\Middleware\MiddlewareInterface {
    public function handle( $request, Closure $next ) {
        if ( ! is_user_logged_in() ) {
            $return_url = $request->getUrl();
            $login_url = wp_login_url( $return_url );
            return app_redirect()->to( $login_url );
        }
        return $next( $request );
    }
}

Router::get( '/protected-url/')
    ->add( AuthenticationMiddleware::class );
```

You can also define global middleware which is applied to all defined routes when booting the framework:

!> Global middleware is only applied on defined routes - normal WordPress requests that do not match any route 
will NOT have middleware applied. To apply global middleware to all requests you have to match all requests with 
routes. Take a look at [Handling all requests](framework/routing/methods.md#handling-all-requests) for an easy way to 
achieve this.

```php
WPEmerge::boot( [
    'global_middleware' => [
        AuthenticationMiddleware::class,
    ],
] );
```

## Order of execution

!> Middleware without a specified priority will be sorted and run in an undefined order (see `Note` on http://php
.net/manual/en/function.sort.php). In most cases this will work perfectly fine, however, if you have middleware that 
depends on another middleware being run before it you may want to specify a priority to ensure a specific order of 
execution (priority is in ascending order).

!> Middleware defined with an anonymous function will use the default priority as specified in your [configuration](framework/configuration.md).

```php
WPEmerge::boot( [
    'middleware_priority' => [
        // CLASS_NAME => DESIRED_PRIORITY,
        \App\Middleware\YourMiddleware::class => 30,
    ],
] );
```
