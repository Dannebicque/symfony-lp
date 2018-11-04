# Services et Injection de dépendances

* [Notion de service](#services)
* [Injection de dépendance](#services-et-injection-de-dpendances)


Fetching Services¶
Symfony comes packed with a lot of useful objects, called services. These are used for rendering templates, sending emails, querying the database and any other "work" you can think of.

If you need a service in a controller, type-hint an argument with its class (or interface) name. Symfony will automatically pass you the service you need:

 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
use Psr\Log\LoggerInterface
// ...

/**
 * @Route("/lucky/number/{max}")
 */
public function number($max, LoggerInterface $logger)
{
    $logger->info('We are logging!');
    // ...
}
Awesome!

