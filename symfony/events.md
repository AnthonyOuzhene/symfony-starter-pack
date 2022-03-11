# Les events

fenêtres ouverte par l'ORM pour exécuter du code.
On utilise les Events avant ou après des péthodes d'éxécution du code (`persist`, `flush`, `remove`, etc.)

## Liste des events

|Event|Dispatched|by|Lifecycle|Callback|Passed Argument

|---    |:-:    |:-:    |:-:    |--:    |

preRemove	$em->remove()	Yes	`_LifecycleEventArgs`_
postRemove	$em->flush()	Yes	`_LifecycleEventArgs`_
prePersist	$em->persist() on initial persist	Yes	`_LifecycleEventArgs`_
postPersist	$em->flush()	Yes	`_LifecycleEventArgs`_
preUpdate	$em->flush()	Yes	`_PreUpdateEventArgs`_
postUpdate	$em->flush()	Yes	`_LifecycleEventArgs`_
postLoad	Loading from database	Yes	`_LifecycleEventArgs`_
loadClassMetadata	Loading of mapping metadata	No	`_LoadClassMetadataEventArgs`
onClassMetadataNotFound	MappingException	No	`_OnClassMetadataNotFoundEventArgs`
preFlush	$em->flush()	Yes	`_PreFlushEventArgs`_
onFlush	$em->flush()	No	`_OnFlushEventArgs`
postFlush	$em->flush()	No	`_PostFlushEventArgs`
onClear	$em->clear()	No	`_OnClearEventArgs`




## Les cycles de vie : emoji_tiger

Tous les différents états par lesquels passent un élement du code. 
On se demande alors quels sont les évènements qui me disent si je peux exécuter du code.

Différents cycles de vie selon environnement :
-  exemple :
Cycles de vie d'ORM (new, persist, flush, etc.)

## Event Listeners

Nous avons 2 possibilités

### 1ère facon : EVENT LISTENER

Exemple dans un formulaire :

```sh
    ->addEventListener(FormEvents::PRE_SET_DATA, [$this, 'preSetData'])
        ;
    }

    public function preSetData(FormEvent $event)
    {
        /** @var User $user */
        $user = $event->getData();

        $user->setUsername(uniqid('toto-'));
        // dd($user);
    }
```

### 2ème facon : SUBSCRIBER

Avec Subscriber, aucune différence si ce n'est que le subscriber est géré directement dans une classe avec l'implémentation EventSubscriberInterface contrairement au listener

```sh
bin/console make:subscriber
```
puis 

```sh
  What event do you want to subscribe to?:
 > console.command```
 ```