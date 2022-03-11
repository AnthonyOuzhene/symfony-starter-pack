# Sécurité dans Symfony

3 Grandes étapes :

- Enregistrement

- Authentification

- Autorisation


## Installation du composant

```sh
# Permet de configurer notre user et ses propriétés.
# Une classe Repository est créée automatiquement.
composer require symfony/security-bundle
```

### Ce composant installe un fichier security.yaml : la configuration de sécurité de Symfony
- Password hashers
- Firewalls : règles à appliquer pour la sécurité
- Access Control List (ACL) : Gestion des autorisations

## 1. Fournir des users

- Créer des entités avec le maker.

[Voir le fichier Maker](symfony/maker.md)

```sh
bin/console make:user
```
## 2. Appliquer les migrations

```sh
bin/console make:migration

bin/console doctrine:migrations:migrate
```

## 3. Hasher les mot de passer

```sh
php bin/console security:hash-password
```

## 4. Créer le formulaire de Login
 
 - POSSIBILITE N°1

```sh
bin/console make:controller Login
```
1. puis rajouter dans sécurity.yaml dans firewalls/main
```sh
            form_login:
                # "login" is the name of the route created previously
                login_path: login
                check_path: login
```
2. Puis faire la route manuellement pour logout en suivant la doc

- POSSIBILITE N°2 (attention, vérifier dans doc si methode dépréciée)

```
bin/console make:auth
```
1. Puis dans src/Controller/Security/LoginAuthentificator (dans la méthode onAuthenticationSuccess):

- Rediriger vers une route opérationnelle 
- et commente "throw exception"

```sh
        // For example:
        return new RedirectResponse($this->urlGenerator->generate('backoffice_movie_browse'));
        // throw new \Exception('TODO: provide a valid redirect inside '.__FILE__);
```
2. pas de numero 2

## 5. Paramétrer les autorisations & hierarchisation des rôles

Chaque role doit commencer par ROLE_

Créer une base de role dans le role de base pour que les suivants héritent des responsabilités données.

Dans [security.yaml] :
```sh
    role_hierarchy:
        ROLE_ADMIN:   ROLE_MANAGER
        ROLE_MANAGER: ROLE_USER
```

On peut récupérer l'objet l'utilisateur pour sécurité les accès dans le code de 4 manières différentes :

1. Dans Twig avec :

```sh
{% if is_granted('ROLE_USER') %}
{% endif %}
```

2. Dans la méthode du Controller avec la fonction `denyAccessUnlessGranted()`:
```sh
$this->denyAccessUnlessGranted('ROLE_USER');
```

3. Dans les annotations du Controller :
```sh
#* @IsGranted("ROLE_USER")
```
4. dans ACL du security.yaml :

```sh
access_control:
# Sécuriser toutes les routes du backoffice /add /edit /delete pour les admin
- { path: ^/backoffice/.*/(add|new|edit|delete), roles: ROLE_ADMIN }
```

Est-ce que tu as vu que j'avais fait une pull Request sur GitHub du repo symfo-recap ? Car je ne suis pas sur qu'elle soit bien passée...