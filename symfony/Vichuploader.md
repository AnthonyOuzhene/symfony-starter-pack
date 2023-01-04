# Vich uploader

bundle Symfony de téléchargement de fichiers et d'images.

## installation du bundle

```sh
composer require vich/uploader-bundle
```

## Configurer les mappings dans les fichiers .yaml

Ces "mappages" indiquent au bundle où les fichiers doivent être téléchargés et quels chemins doivent être utilisés pour les afficher dans l'application.

```yaml
# config/services.yaml
parameters:
    gallery_images: /uploads/gallery
    # ...

# config/packages/vich_uploader.yaml
vich_uploader:
    db_driver: orm

    mappings:
        gallery_images:
            uri_prefix: '%gallery_images%'
            upload_destination: '%kernel.project_dir%/public%gallery_images%'
            namer: Vich\UploaderBundle\Naming\SmartUniqueNamer
            delete_on_update: false
            delete_on_remove: false
```

## Préparer les entités pour les images

1. Ajouter l'annotation à la racine de la classe souhaité avec les `use` correspondants

```php
use Symfony\Component\HttpFoundation\File\File;
use Vich\UploaderBundle\Mapping\Annotation as Vich;

/**
 * @ORM\Entity
 * @Vich\Uploadable
 */
class Product
{
    // ...
}
```

2. Ajouter une nouvelle propriété `File` pour l'image correspondante (ici c'est main_pictureFile) ET un propriété `UpdatedAt`.

* Si `updatedAt` n'est pas définie dans l'entité, utilisez une autre propriété existante équivalente.

* Bien relier dans annotation l'image à l'imageFile

```php
/**
* @ORM\Column(type="string", length=255)
*/
private $main_picture;

/**
* @Vich\UploadableField(mapping="gallery_images", fileNameProperty="main_picture")
* @var File
*/
private $main_pictureFile;

/**
* @ORM\Column(type="datetime")
* @var \DateTime
*/
    private $updatedAt;
```

3. Ajouter les getter et les setter

```php
public function setImageFile(File $image = null)
    {
        $this->imageFile = $image;

        // VERY IMPORTANT:
        // It is required that at least one field changes if you are using Doctrine,
        // otherwise the event listeners won't be called and the file is lost
        if ($image) {
            // if 'updatedAt' is not defined in your entity, use another property
            $this->updatedAt = new \DateTime('now');
        }
    }

    public function getImageFile()
    {
        return $this->imageFile;
    }
```

La propriété main_picture stocke uniquement le nom de l'image téléchargée et elle est conservée dans la base de données. La propriété main_pictureFile stocke le contenu binaire du fichier image et il n'est pas conservé dans la base de données (c'est pourquoi il ne définit pas d' @ORM annotation).

## Configurer le CrudController d'easyAdmin

1. Utilisez un textField pour uploader l'url de l'image
2. Utiliser ImageField pour stocker le File de l'image et le chemin du repertoire de stockage

```php
    public function configureFields(string $pageName): iterable
    {
        return [
            IdField::new('id')->hideOnForm(),
            TextField::new('name', 'Titre de l\'image'),
            
            TextField::new('main_pictureFile', 'Image principale')
            ->setFormType(VichImageType::class)
            ->onlyOnForms(),

            ImageField::new('main_picture', 'Image principale')
            ->setBasePath('/uploads/gallery')
            ->onlyOnIndex(),
        ];
    }
```
