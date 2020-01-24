
# <a name="create-an-aspnet-core-web-app-in-azure"></a>Cr�er et d�ployer une application web ASP.NET Core dans Azure Web Apps


[Azure App Service](https://docs.microsoft.com/fr-ca/azure/app-service/) vous permet de cr�er et d�h�berger des applications web, des back-ends mobiles et des API RESTful 
dans le langage de programmation de votre choix sans g�rer l�infrastructure. Il offre une mise � l��chelle automatique et une haute disponibilit�, 
prend en charge � la fois Windows et Linux et permet des d�ploiements automatis�s � partir de GitHub, Azure DevOps ou n�importe quel r�f�rentiel Git.

##<a name="goal"></a> Objectif

Pour cette premi�re partie du laboratoire, les participants vont cr�er une application Web ASP.NET Core dans Visual Studio. Ils vont ajouter un formulaire pour collecter les commentaires
des utilisateurs. L'application sera ensuite d�ploy�e sur Azure App Service.

## <a name="prerequisites"></a>Pr�requis

Pour effectuer ce laboratoire, vous devez installer <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio�2019</a> avec la charge de travail **D�veloppement web et ASP.NET**.

Si vous avez d�j� install� Visual Studio�2019�:

- Installez les derni�res mises � jour dans Visual Studio en s�lectionnant **Aide** > **Rechercher les mises � jour**.
- Ajoutez la charge de travail en s�lectionnant **Outils** > **Obtenir des outils et des fonctionnalit�s**.

## <a name="open-website"></a> Ouvrir le projet de demarrage

Cr�ez une application web ASP.NET Core en effectuant les �tapes suivantes�:

1. Ouvrez Visual Studio, puis s�lectionnez **Ouvrir un projet ou une solution**.

 ![Ouverture projet](./media/open-project.PNG)

2. Ouvrez la solution de l'�tape 1 (\Etape 1 - Deployer une Web App sur Azure\Workshop\Workshop.sln).

3. Dans le menu Visual Studio, s�lectionnez **D�boguer** > **D�marrer le d�bogage** pour ex�cuter l�application web localement.

![Ex�cuter l�application localement](./media/debug-locally.PNG)

## <a name="publish-y"></a>Modifier l'application Web


### Le mod�le

Dans le dossier **Models**, ajoutez une nouvelle classe **Commentaire.cs**, avec le code suivant :

```cs
using System;


namespace WebApp.Models
{
    public class Commentaire
    {
        public int Id { get; set; }

        public string Nom { get; set; }

        public string Email { get; set; }

        public DateTime DateCommentaire { get; set; }
    }
}

```
### Le contr�leur

1. Faites un clic droit sur le dossier Controllers. Selectionnez **Ajouter -> Contr�leur**.

2. Dans la fen�tre qui va s'afficher, selectionnez **Contr�leur MVC - Vide**.

![Ajout controleur](./media/add-controller.PNG)

3. Dans la fen�tre suivante, donnez le nom **CommentairesController**.

4. Remplacez le code dans ce fichier par ce qui suit :

```cs
using System;
using System.Collections.Generic;
using Microsoft.AspNetCore.Mvc;
using WebApp.Models;

namespace WebApp.Controllers
{
    public class CommentairesController : Controller
    {
        public IActionResult Index()
        {
            var commentaires = new List<Commentaire>()
            {
                new Commentaire(){Id=1,Nom="Thomas", Email="thomas@test.com", Texte="Belle initiative", DateCommentaire=DateTime.Now},
                new Commentaire(){Id=1,Nom="Daniel", Email="daniel@test.com", Texte="Int�ressant pour d�couvrir Azure", DateCommentaire=DateTime.Now}
            };

            return View(commentaires);
        }
    }
}
```

### La vue

1. Faites un clic droit sur le nom de la m�thode **Index()** du contr�leur **CommentairesController**. 

1. S�lectionner Ajouter une Vue 

![Ajout controleur](./media/scalflod-view.png)

La fen�tre d'ajout de la vue va s'afficher.

Dans le champ Mod�le, d�roulez et s�lectionnez 'List'.

Dans le champ Classe de  mod�le, d�roulez et s�lectionnez 'Commentaire'.

Cliquez sur ajouter.

![Ajout controleur](./media/add-view.PNG)

Un fichier Index.cshtml sera ajout� dans le dossier 'Views/Commentaires'.

### Le layout

Ouvrez le fichier /Views/Shared/_Layout.cshtml.

Apr�s les lignes de code :

```cs
<li class="nav-item">
                            <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Index">Accueil</a>
                        </li>
```

Ajoutez le code suivant 

```cs
<li class="nav-item">
                            <a class="nav-link text-dark" asp-area="" asp-controller="Commentaires" asp-action="Index">Commentaires</a>
                        </li>
```

### Ex�cuter l'application

Appuyez F5 pour ex�cuter l'application.

## <a name="publish-your-web-app"></a>D�ployer sur Azure

Le moyen le plus simple de d�ployer votre application est d'utiliser la fonction Web Deploy de Visual Studio.

1. Dans l�**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **WebApp**, puis s�lectionnez **Publier**.

2. Choisissez **App Service**, puis cliquez sur **Cr�er un profil**.

   ![Publier � partir de la page de pr�sentation du projet](./media/publish-app-vs2019.PNG)

Dans **Cr�er un App Service**, vos options varient si vous �tes d�j� connect� � Azure et si vous avez un compte Visual Studio li� � un compte Azure. 
 
3. S�lectionnez **Ajouter un compte** ou **Connexion** pour vous connecter � votre abonnement Azure. Si vous �tes d�j� connect�, s�lectionnez le compte souhait�.

   ![Connexion � Azure](./media/sign-in-azure-vs2019.PNG)

<a herf="https://docs.microsoft.com/fr-ca/azure/azure-resource-manager/management/overview#terminology">Un groupe de ressources</a> est un conteneur logique dans lequel les ressources Azure comme les applications web, les bases de donn�es et les comptes de stockage sont d�ploy�s et g�r�s. Par exemple, vous pouvez choisir de supprimer le groupe de ressources complet ult�rieurement en une seule �tape.

4. Pour **Groupe de ressources**, s�lectionnez **Nouveau**.

5. Dans **Nouveau nom du groupe de ressources**, entrez *CEMLabRG*, puis s�lectionnez **OK**.

<a href="https://docs.microsoft.com/fr-ca/azure/app-service/overview-hosting-plans">Un plan App Service</a> sp�cifie l�emplacement, la taille et les fonctionnalit�s de la batterie de serveurs web qui h�berge votre application

6. Pour le **Plan d�h�bergement**, s�lectionnez **Nouveau**.

7. Dans la bo�te de dialogue **Configurer le plan d�h�bergement**, entrez les valeurs du tableau suivant, puis s�lectionnez **OK**.

   | Param�tre | Valeur sugg�r�e | Description |
   |-|-|-|
   |Plan App Service| MonPremierWebAppPlan | Nom du plan App Service. |
   | Location | Canada East | Centre de donn�es dans lequel l�application web est h�berg�e. |
   | Size | Gratuit | Le [niveau tarifaire](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) d�termine les fonctionnalit�s d�h�bergement. |

   ![Cr�er un plan App Service](./media/app-service-plan-vs2019.PNG)

8. Dans **Nom**, entrez un nom d�application unique qui inclut uniquement les caract�res valides `a-z`, `A-Z`, `0-9` et `-`. Vous pouvez accepter le nom unique g�n�r� automatiquement. L�URL de l�application web est `http://<app_name>.azurewebsites.net`, o� `<app_name>` est le nom de votre application.

   ![Configurer le nom de l�application](./media/web-app-name-vs2019.PNG)

9. S�lectionnez **Cr�er** pour commencer � cr�er les ressources Azure.

Une fois les ressources Azure cr��es, une page r�capitulative va s'afficher.

 ![Configurer le nom de l�application](./media/publis-webapp.PNG)

10. Dans la page r�capitulative intitul�e **Publier**, s�lectionnez **Publier**.

Une fois la publication termin�e, Visual Studio lance le navigateur avec votre application en cours d'ex�cution.

![Application web ASP.NET publi�e dans Azure](./media/web-app-running-live.PNG)

Le nom d�application sp�cifi� dans la page **Cr�er un App Service** est utilis� en tant que pr�fixe d�URL au format `http://<app_name>.azurewebsites.net`.


**F�licitations�!** Votre application web ASP.NET Core s�ex�cute en temps r�el dans Azure App Service.





## <a name="manage-the-azure-app"></a>G�rer l�application Azure

Pour g�rer l�application web, acc�dez au [Portail Azure](https://portal.azure.com), puis recherchez et s�lectionnez **App Services**.

![S�lectionner App Services](./media/app-service-web-get-started-dotnet/app-services.png)

Dans la page **App Services**, s�lectionnez le nom de votre application web.

![Navigation au sein du portail pour acc�der � l�application Azure](./media/app-service-web-get-started-dotnet/access-portal-vs2019.png)

Vous voyez appara�tre la page Vue d�ensemble de votre application web. Ici, vous pouvez effectuer des t�ches de gestion de base, par exemple parcourir, arr�ter, d�marrer, red�marrer et supprimer.

![App Service dans le portail Azure](./media/app-service-web-get-started-dotnet/web-app-general-vs2019.png)

Le menu de gauche fournit diff�rentes pages vous permettant de configurer votre application.

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>�tapes suivantes

> [!div class="nextstepaction"]
> [Build a .NET Core and SQL Database web app in Azure App Service](app-service-web-tutorial-dotnetcore-sqldb.md) (Cr�er une application web .NET Core et SQL Database dans Azure App Service)

