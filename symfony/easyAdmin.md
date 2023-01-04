# Easy Admin en backoffice

EasyAdmin nécessite les éléments suivants :

- PHP 8.0.2 ou supérieur
- Symfony 5.4 ou supérieur
- Entités Doctrine ORM

## Installationn du bundle

```sh
composer require easycorp/easyadmin-bundle
```

## Créer le 1er Dashboard grâce au maker

````sh
php bin/console make:admin:dashboard
````

## Rendez-vous sur localhost/admin

Une interface graphique permet de gérer facilement les entités du backoffice.

### Les toutes sont générées dans les annotations mais il estpossible de gérer ça dans le fichier `config/routes.yaml`

```sh
# config/routes.yaml
dashboard:
    path: /admin
    controller: App\Controller\Admin\DashboardController::index
```

## Configuration du tableau de bord

La configuration du tableau de bord est définie dans la méthode `configureDashboard()`

```sh
   public function configureDashboard(): Dashboard
    {
        return Dashboard::new()
            ->setTitle("Titre de votre backoffice");
    }
```

## Configuration du Menu

La configuration du tableau de bord est définie dans la méthode `configureMenuItems()`

```sh
    public function configureMenuItems(): iterable
    {
        # linkToDashboard sert à utiliser un autre Crud Controller en guise de page d'accueil.
        yield MenuItem::linkToDashboard('Salon de tatouage', 'fa fa-home');
        yield MenuItem::linkToCrud('Activités', 'fa fa-briefcase', Activity::class);
        yield MenuItem::linkToCrud('Categories', 'fa fa-list', Category::class);
        yield MenuItem::linkToCrud('Livre dor', 'fa fa-comment', Comment::class);
        yield MenuItem::linkToCrud('Gallerie', 'fa fa-image', Gallery::class);
        yield MenuItem::linkToCrud('Blog', 'fa fa-blog', Post::class);
        yield MenuItem::linkToCrud('Prestations', 'fa fa-pen', Service::class);
        yield MenuItem::linkToCrud('Utilisateurs', 'fa fa-users', User::class);
        #yield MenuItem::linkToCrud('The Label', 'fas fa-list', EntityClass::class);
    }
```

### Les champs des entités

Les champs sont paramétrable dans la méthode `configureFileds()`

`````sh
    public function configureFields(string $pageName): iterable
    {
        return [
            #hideOnForm() permet de cacher l'ID sur le formulaire d'ajout et de modification
            IdField::new('id')->hideOnForm(),
            TextField::new('name', "Nom du service"),
            IntegerField::new('price', "Prix"),
            TextareaField::new('description', "Description"),
            #hideOnIndex() permet de cacher la photo sur la liste des services
            TextareaField::new('picture', "Photo du service")->hideOnIndex(),
            AssociationField::new('activity_name'),
            AssociationField::new('category_name'),
        ];
    }

````