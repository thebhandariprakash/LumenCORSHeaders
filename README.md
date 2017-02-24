# Lumen with CORS and OPTIONS requests

Additional middleware can be written to perform a variety of tasks besides authentication. 
A CORS middleware might be responsible for adding the proper headers to all responses leaving your application. 
Working with the APIs you may face the error like "No 'Access-Control-Allow-Origin' header is present on the requested resource".
This type of problem cab be solve by creating the additional middleware. This can be done as follows.

**1. Creating `CrosMiddleware` class**

All middleware should be stored in the **app/Http/Middleware** directory. Lets create **CrosMiddleware** in app/Http/Middleware directory.

```php 
<?php
/**
 * User: bhprakash
 * Date: 2/24/17
 * Time: 10:34 AM
 */

namespace App\Http\Middleware;

use Closure;

class CrosMiddleware
{
    public function handle($request, Closure $next)
    {
        $headers = [
            'Access-Control-Allow-Origin'      => '*',
            'Access-Control-Allow-Methods'     => 'POST, GET, OPTIONS, PUT, DELETE',
            'Access-Control-Allow-Headers'     => 'Content-Type, Authorization, X-Requested-With'
        ];
        
        if ($request->isMethod('OPTIONS'))
        {
            return response()->json('{"method":"OPTIONS"}', 200, $headers);
        }

        $response = $next($request);

        foreach($headers as $key => $value)
        {
            $response->header($key, $value);
        }

        return $response;
    }
}

```

**2. Registering Middleware**

 Simply list the middleware class in the call to the **$app->middleware()** method in your **bootstrap/app.php** file:
 
```php
 $app->middleware([
    App\Http\Middleware\OldMiddleware::class
 ])
 ```