# CONFLICTS

This document explains why certain conflicts were added to `composer.json` and
references related issues.

 - `symfony/cache": "^6.0`, "symfony/amqp-messenger": "^6.0", "symfony/doctrine-messenger": "^6.0", 
"symfony/error-handler": "^6.0", "symfony/redis-messenger": "^6.0", "symfony/stopwatch": "^6.0", "symfony/twig-bridge": "^6.0", 
"symfony/var-dumper": "^6.0", "symfony/var-exporter": "^6.0",:

   These libraries still happen to be installed with Sylius if no flex is used. As we don't support Sf6 yet they are conflicted. Installation of symfony/cache v6.0 results with following error:
   ```
   Uncaught Error: Class "Symfony\Component\Cache\DoctrineProvider" not found
   ```
   
 - `symfony/password-hasher": "^6.0`:

   Symfony in version 5.3 change password hashing logic, and in version 6.0 they removed BC layer
   
   References: https://github.com/Sylius/Sylius/pull/13358

 - `doctrine/doctrine-bundle:2.3.0`:

   This version makes Gedmo Doctrine Extensions fail (tree and position behaviour mostly).

   References: https://github.com/doctrine/DoctrineBundle/issues/1305

 - `jms/serializer-bundle:4.1.0`:

   This version contains service with a wrong constructor arguments:
   `Invalid definition for service ".container.private.profiler": argument 4 of "JMS\SerializerBundle\Debug\DataCollector::__construct()" accepts "JMS\SerializerBundle\Debug\TraceableDriver", "JMS\SerializerBundle\Debug\TraceableMetadataFactory" passed.`

   References: https://github.com/schmittjoh/JMSSerializerBundle/issues/902
 
 - `symfony/dependency-injection:5.4.5`:
   
   This version is causing a problem with mink session:
  `InvalidArgumentException: Specify session name to get in vendor/friends-of-behat/mink/src/Mink.php:198`,
   Psalm error: 
   `UndefinedDocblockClass: Docblock-defined class, interface or enum named UnitEnum does not exist`.

 - `symfony/framework-bundle:^5.4.5`:

   This version is causing a problem with returning null as token from `Symfony\Component\Security\Core\Authentication\Token\Storage\TokenStorage`
   which leads to wrong solving path prefix by `Sylius\Bundle\ApiBundle\Provider\PathPrefixProvider` in API scenarios

 - `api-platform/core:2.7.0`:

   The FQCN of `ApiPlatform\Core\Metadata\Resource\ResourceNameCollection` has changed to:
   `ApiPlatform\Metadata\Resource\ResourceNameCollection` and due to this fact
   `Sylius\Bundle\ApiBundle\Swagger\AcceptLanguageHeaderDocumentationNormalizer` 
   references this class throws an exception
  `Class "ApiPlatform\Core\Metadata\Resource\ResourceNameCollection" not found`
