# CKEditor

CKEditor est un éditeur WYSIWYG qui peut facilement être intégré à tout site web dans les formulaires divers.
Il apporte de nombreuses fonctionnalités d'édition et de nombreux plugins.

Il est disponible en téléchargement pour tout projet sur le site officiel, et adapté à Symfony par le groupe "Friends of Symfony", sous le nom FOSCkeditor.

## installation du bundle

```sh
composer require friendsofsymfony/ckeditor-bundle
```

## Télechargement et installations les ressources nécessaires (HTML, CSS, javascript) dans le dossier `public

```sh
bin/console ckeditor:install
```

## Copier les sources dans `public`

```sh
bin/console assets:install public
```

## Configuration CKEditor

Une fois installé, CKEditor est configurable directement depuis le fichier "config/packages/fos_ckeditor.yaml" qui a été créé automatiquement.

voici la configuration customs qui comprendra la liste des options qui seront proposées aux utilisateurs.

```sh
# Read the documentation: https://symfony.com/doc/current/bundles/FOSCKEditorBundle/index.html

twig:
    form_themes:
        - '@FOSCKEditor/Form/ckeditor_widget.html.twig'

fos_ck_editor:
    configs:
        main_config:
            toolbar:
                - { name: "styles", items: ['Bold', 'Italic', 'Underline', 'Strike', 'Blockquote', '-', 'Link', '-', 'RemoveFormat', '-', 'NumberedList', 'BulletedList', '-', 'Outdent', 'Indent', '-', '-', 'JustifyLeft', 'JustifyCenter', 'JustifyRight', 'JustifyBlock', '-', 'Image', 'Table', '-', 'Styles', 'Format','Font','FontSize', '-', 'TextColor', 'BGColor', 'Source'] }
```

## Utilisation CKEditor


Une fois CKEditor installé et configuré, il nous suffit de l'appeler depuis un formulaire Symfony pour l'intégrer à celui-ci.

N'oubliez pas le Use :

```sh
use FOS\CKEditorBundle\Form\Type\CKEditorType;
```

### Si utilisation dans Form de Symfony :

```sh
   ->add('contenu', CKEditorType::class) // Ce champ sera remplacé par un éditeur WYSIWIG
```

### Si utilisation dans Bundle EasyAdmin :

```php
   TextEditorField::new('content')->hideOnIndex()->setFormType(CKEditorType::class),
```

Sous la function `configureFields()` du CrudController, ajoutez :

```php
public function configureCrud(Crud $crud): Crud
{
  return $crud
      ->addFormTheme('@FOSCKEditor/Form/ckeditor_widget.html.twig');
}
```

Et voilà : CKEDitor est activé dans EasyAdmin !

## Mettre en place l'upload d'images dans éditeur de texte

Si nous souhaitons uploader des images et les afficher dans notre article, une interface d'ajout d'images sera nécessaire.
Pour ajouter l'envoi d'images, un bundle appelé "elfinder" est très utile.

1. installation du bundle

```sh
composer require helios-ag/fm-elfinder-bundle
```

2. Installation du javascript du bundle dans bibliothèque Symfony

```sh
bin/console elfinder:install

# Message de réussite :
 [OK] elFinder assets successfully installed 
 ```

3. Configurer le bundle ElFinder

Aller dans `confif/packages.fm_elfinder.yaml`

```yaml
fm_elfinder:
    assets_path: /assets # chemin des fichiers JS
    instances:
        default:
            locale: fr # Langue
            editor: ckeditor # Editeur utilisé
            fullscreen: true # Taille d'affichage
            theme: smoothness # Thème à utiliser
            connector:
                debug: false # Désactive le débug
                roots:
                    uploads:
                        show_hidden: false # Masque les fichiers cachés
                        driver: LocalFileSystem # Pilote des fichiers
                        path: uploads/images # Chemin d'upload
                        upload_allow: ['image/png', 'image/jpg', 'image/jpeg'] # Fichiers autorisés
                        upload_deny: ['all'] # Fichiers interdits
                        upload_max_size: 2M # Taille maximum
```

4. Ajouter elfinder à CKEditor

Une fois elfinder installé, il faut déclarer son existence à CKEditor.

Nous allons donc modifier le fichier `fos_ckeditor.yaml` et y ajouter les 2 dernières lignes ci-dessous dans toutes les configurations qui nous intéressent (pour nous main_config)

```yaml
main_config:
    toolbar:
        - { name: "styles", items: ['Bold', 'Italic', 'Underline', 'Strike', 'Blockquote', '-', 'Link', '-', 'RemoveFormat', '-', 'NumberedList', 'BulletedList', '-', 'Outdent', 'Indent', '-', '-', 'JustifyLeft', 'JustifyCenter', 'JustifyRight', 'JustifyBlock', '-', 'Image', 'Table', '-', 'Styles', 'Format','Font','FontSize', '-', 'TextColor', 'BGColor', 'Source'] }
    filebrowserBrowseRoute: elfinder
    filebrowserBrowseRouteParameters: []
```
