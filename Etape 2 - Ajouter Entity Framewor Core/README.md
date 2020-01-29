# <a name="crud-avec-ef-core">CRUD avec Entity Framework Core</a>

<a href="https://docs.microsoft.com/fr-ca/ef/core/">Entity Framework (EF) Core</a> est une version l�g�re, extensible, open source et multiplateforme de la tr�s connue technologie d�acc�s aux donn�es Entity Framework.
EF Core peut servir de mappeur relationnel/objet (O/RM), permettant aux d�veloppeurs .NET de travailler avec une base de donn�es � l�aide d�objets .NET, et �liminant la n�cessit� de la plupart du code d�acc�s aux donn�es qu�ils doivent g�n�ralement �crire. EF Core prend en charge de nombreux moteurs de base de donn�es.

## <a name="objectif"></a> Objectif

Pour cette deuxi�me partie du laboratoire, les participants vont moodifier leur application pour int�grer la prise en charge de Entity Framework. Ils utiliseront ensuite 
les outils de Visual Studio pour mettre en place des formulaires permettant la consulation, l'ajout, la modification et la suppression des donn�es dans une banque SQLite
en utilisant Entity Framework Core.

## <a name="open-web-site">Ouvrir le projet de demarrage</a>

1. Ouvrez Visual Studio, puis s�lectionnez **Ouvrir un projet ou une solution**.

 ![Ouverture projet](./media/open-project.PNG)

2. Ouvrez la solution de l'�tape 2 (\Etape 2 - Ajouter Entity Framewor Core\Workshop\Workshop.sln).

3. Supprimer le fichier **CommentairesController.cs** dans le dossier **Controllers**.

4. Supprimer le dossier **Commentaires** dans le dossier **Views**

## <a name="validation"></a> Validation avec les DataAnnotations

Les DataAnnotations sont utilis�s pour personnaliser le mod�le de donn�es en utilisant des attributs qui sp�cifient des r�gles de mise en forme, de validation et de mappage de base de donn�es.

Remplacez le contenu du fichier **Models\Commentaire.cs** par le code qui suit : 

```cs
using System;
using System.ComponentModel.DataAnnotations;

namespace WebApp.Models
{
    public class Commentaire
    {
        public int Id { get; set; }

        public string Nom { get; set; }

        [Required]
        [Display(Name = "Adresse Mail")]
        public string Email { get; set; }

        [Required]
        [Display(Name = "Commemtaire")]
        public string Texte { get; set; }

        [Required]
        [Display(Name = "Date")]
        [DataType(DataType.Date)]
        [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
        public DateTime DateCommentaire { get; set; }
    }
}


```

L'attribut **Required** rend le champ obligatoire. 

L'attribut **Display** permet de d�finir le nom d'affichage.

L'attribut **DataType** permet de sp�cifier un type de donn�es qui est plus sp�cifique que le type intrins�que de la base de donn�es.

L'attribut **DisplayFormat** permet de sp�cifier explicitement le format de la date.


## <a name="">Le DBContext</a>

Le **Database Context (DBContext)** est un �l�ment important d'Entity Framework Core. C'est le pont entre votre domaine (ou vos classes d'entit�) et la base de donn�es.

1. Vous allez cr�er un nouveau dossier **Data**.

2. Dans ce dossier, vous allez ajouter un nouveau fichier WebAppContext.cs.

3. Remplacez le code dans ce fichier par ce qui suit :

```cs
using Microsoft.EntityFrameworkCore;
using WebApp.Models;

namespace WebApp.Data
{
    public class WebAppContext : DbContext
    {

        public WebAppContext(DbContextOptions<WebAppContext> options)
                    : base(options)
        {
        }

        public DbSet<Commentaire> Commentaires { get; set; }

    }
}
```


## <a name="connectionstring">La chaine de connexion</a>

La chaine de connexion (ConnectionString) founit les informations pour se connecter � la base de donn�es, dont le nom de la base de donn�es, le nom d'utilisateur, 
le mot de passe ou encore le serveur de base de donn�es.

Dans notre, nous allons dans un premier temps utiliser une base de donn�es locale SQLite. 

Editez le fichier appsettings.json, et ajoutez la chaine de la connexion :

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "ConnectionStrings": {
    "LocalConnection": "Data Source=localdb.db"
  },
  "AllowedHosts": "*"
}
```

## <a name="install-efcore-sqlite">Installer le package Entity Framework Core pour SQLite</a>

A ce stade, nous allons utiliser la console du gestionnaire de package NuGet pour installer le package **Microsoft.EntityFrameworkCore.Sqlite**.


1. Cliquez sur le menu **Outils**, puis sur **Gestionnaire de package NuGet** et enfin sur **Console du gestionnaire de package**.

 ![Ouverture projet](./media/add-package.png)

 2. Saisisez la commande **Install-Package Microsoft.EntityFrameworkCore.Sqlite** dans la console du gestionnaire de package.

  ![Console NuGet](./media/nuget-console.PNG)


## Enregistrer le DBContext avec l'injection de d�pendances

Pour utiliser votre classe DBContext, elle doit �tre enregistr�e dans le conteneur d'injection de d�pendances de ASP.NET Core. Pour le faire, vous devez utiliser
la m�thode d'extenssion **AddDbContext**.

Ouvrez le fichier Startup.cs, et ajoutez la ligne de code suivante dans la m�thode **ConfigureServices** :


```cs
 services.AddDbContext<WebAppContext>(options =>
                    options.UseSqlite(Configuration.GetConnectionString("LocalConnection")));
```

Le code complet de cette m�thode est le suivant :

```cs
 public void ConfigureServices(IServiceCollection services)
        {
            services.AddControllersWithViews();

            services.AddDbContext<WebAppContext>(options =>
                    options.UseSqlite(Configuration.GetConnectionString("LocalConnection")));

        }

```
Vous devez ajoutez ces r�f�rences. Copiez et collez les lignes ci-dessus dans la section **Using** du fichier Startup.cs :

```cs
using Microsoft.Extensions.Hosting;
using WebApp.Data;
```


## Utiliser la migration pour cr�er et mettre � jour la base de donn�es


```cs


```


```cs


```