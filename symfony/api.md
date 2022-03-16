# API

dans AbstractController, une méthode json est déjà codé pour transformer l'objet en Json (Sterealizer component).
`use Symfony\Component\Serializer\SerializerInterface;`


Pour contourner erreur circle, il faut utiliser un contexte dans le Cotnroller : Les groups !

## Utilisation des groups pour rendre en json

1. Dans entity :

- `use Symfony\Component\Serializer\Annotation\Groups;`


- l'annotation sur chaque propriété de l'entité :
```sh
     * @Groups("api_movie_list")
```

2. Dans le Controller :
```sh
return $this->json($movieList, Response::HTTP_OK, [], ['groups' => "api_movie_list"]);
```

## Gérer la sécurité d'une API

- La seule différence que la sécurité dans le code connu dans le navigateur est au niveau de l'authentification. On utilisera un **Token** pour authenfier un user sur une api.

## CONFIGURATION

**1. Installation du bundle**
```sh
composer require lexik/jwt-authentication-bundle
```

**2. Génère les clés SSL**
```sh
# Genere the SSL keys:
bin/console lexik:jwt:generate-keypair
```

**3. Configuration des clés SSL, path & passphrase dans .env**

```sh
JWT_SECRET_KEY=%kernel.project_dir%/config/jwt/private.pem
JWT_PUBLIC_KEY=%kernel.project_dir%/config/jwt/public.pem
JWT_PASSPHRASE=
```

**4. Puis dans `config/packages/lexik_jwt_authentication.yaml`**

```sh
lexik_jwt_authentication:
    secret_key: '%env(resolve:JWT_SECRET_KEY)%' # required for token creation
    public_key: '%env(resolve:JWT_PUBLIC_KEY)%' # required for token verification
    pass_phrase: '%env(JWT_PASSPHRASE)%' # required for token creation
    token_ttl: 3600 # in seconds, default is 3600
```

**5. Configuration de `config/packages/security.yaml`**
```sh
# Symfony avant versions 5.3
security:
    # ...
    
    firewalls:
        login:
            pattern: ^/api/login
            stateless: true
            json_login:
                check_path: /api/login_check # or api_login_check as defined in config/routes.yaml
                success_handler: lexik_jwt_authentication.handler.authentication_success
                failure_handler: lexik_jwt_authentication.handler.authentication_failure

        api:
            pattern:   ^/api
            stateless: true
            guard:
                authenticators:
                    - lexik_jwt_authentication.jwt_token_authenticator

    access_control:
        - { path: ^/api/login, roles: IS_AUTHENTICATED_ANONYMOUSLY }
        - { path: ^/api,       roles: IS_AUTHENTICATED_FULLY }
```

```sh
# Symfony après versions 5.3
security:
    enable_authenticator_manager: true
    # ...
    
    firewalls:
        login:
            pattern: ^/api/login
            stateless: true
            json_login:
                check_path: /api/login_check
                success_handler: lexik_jwt_authentication.handler.authentication_success
                failure_handler: lexik_jwt_authentication.handler.authentication_failure

        api:
            pattern:   ^/api
            stateless: true
            jwt: ~

    access_control:
        - { path: ^/api/login, roles: PUBLIC_ACCESS }
        - { path: ^/api,       roles: IS_AUTHENTICATED_FULLY }
```

**6. Configuration du routing dans `config/routes.yaml`**

```sh
api_login_check:
    path: /api/login_check
```

## UTILISATION

**1. Dans Thunder Clienbt ou Insomnia; créer une route de Login en POST**

**2. Dans body mettre les infos dont on a besoin :**
```sh
{"username":"johndoe","password":"test"}
```
- `Send` et récupérer le Token comme exemple ci-dessous :
```sh
{
   "token" : "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXUyJ9.eyJleHAiOjE0MzQ3Mjc1MzYsInVzZXJuYW1lIjoia29ybGVvbiIsImlhdCI6IjE0MzQ2NDExMzYifQ.nh0L_wuJy6ZKIQWh6OrW5hdLkviTs1_bau2GqYdDCB0Yqy_RplkFghsuqMpsFls8zKEErdX5TYCOR7muX0aQvQxGQ4mpBkvMDhJ4-pE4ct2obeMTr_s4X8nC00rBYPofrOONUOR4utbzvbd4d2xT_tj4TdR_0tsr91Y7VskCRFnoXAnNT-qQb7ci7HIBTbutb9zVStOFejrb4aLbr7Fl4byeIEYgp2Gd7gY"
}
```

3. Transmettre el jeton dans Auth/Bearer/Bearer Token.

Par défaut seul le mode header d'autorisation est activé :Authorization: Bearer {token}

4. Gérer les droits d'authorisation dans les annotations des routes.

## Besoin des Cors

**1. Installation**

```sh
$ composer req cors
# si pas Flex :
composer require nelmio/cors-bundle
```

**2. Configuration**

Dans `config/packages/nelmio_cors.yaml.`

```sh
nelmio_cors:
    defaults:
        origin_regex: true
        allow_origin: ['%env(CORS_ALLOW_ORIGIN)%']
        allow_methods: ['GET', 'OPTIONS', 'POST', 'PUT', 'PATCH', 'DELETE']
        allow_headers: ['Content-Type', 'Authorization']
        expose_headers: ['Link']
        max_age: 3600
    paths:
        '^/api/':
            allow_origin: ['*']
            allow_headers: ['*']
            allow_methods: ['POST', 'PUT', 'GET', 'DELETE']
```