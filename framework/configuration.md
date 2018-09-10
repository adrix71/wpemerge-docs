# Configuration

When booting WP Emerge you can specify a number of configuration options:
```php
WPEmerge::boot( [
    // Array of classes that implement \WPEmerge\ServiceProviders\ServiceProviderInterface.
    'providers' => [
        // Examples:
        MyServiceProviders::class,
        // Blade example for htmlburger/wpemerge-blade:
        WPEmergeBlade\View\ServiceProvider::class,
    ],
    
    // Default middleware priority. Defaults to 100.
    'middleware_default_priority' => 100,
    
    // Array of middleware priority to change execution order.
    'middleware_priority' => [
        // Examples:
        MyMiddleware::class => 90,
        MySecondMiddleware::class => 110,
    ],

    // Array of middleware to apply to all routes.
    'global_middleware' => [
        // Examples:
        MyMiddleware::class,
        function( $request, $next ) {
            $response = $next( $request );
            // minify response here, for example.
            return $response;
        },
    ],
    
    // Array of debug settings.
    'debug' => [
        // Enable the use of filp/whoops for an enhanced error interface. Defaults to true.
        'pretty_errors' => true,
    ],
] )
```
