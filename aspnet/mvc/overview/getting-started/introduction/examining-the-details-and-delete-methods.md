---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: "Examiner les détails et les méthodes de suppression | Documents Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 6d7d0fe5bd2f6a6bd7f9c7ca04a8f142223ccf8e
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2017
---
<a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="ec2c8-102">Examiner les détails et les méthodes de suppression</span><span class="sxs-lookup"><span data-stu-id="ec2c8-102">Examining the Details and Delete Methods</span></span>
====================
<span data-ttu-id="ec2c8-103">Par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="ec2c8-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="ec2c8-104">Dans cette partie du didacticiel, vous allez examiner générées automatiquement `Details` et `Delete` méthodes.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-104">In this part of the tutorial, you'll examine the automatically generated `Details` and `Delete` methods.</span></span>

## <a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="ec2c8-105">Examiner les détails et les méthodes de suppression</span><span class="sxs-lookup"><span data-stu-id="ec2c8-105">Examining the Details and Delete Methods</span></span>

<span data-ttu-id="ec2c8-106">Ouvrez le `Movie` contrôleur et examiner les `Details` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ec2c8-106">Open the `Movie` controller and examine the `Details` method.</span></span>

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

<span data-ttu-id="ec2c8-107">Le moteur de génération de modèles automatique de MVC qui a créé cette méthode d’action ajoute un commentaire indiquant une requête HTTP qui appelle la méthode.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-107">The MVC scaffolding engine that created this action method adds a comment showing a HTTP request that invokes the method.</span></span> <span data-ttu-id="ec2c8-108">Dans ce cas, il est un `GET` demande avec trois segments d’URL, le `Movies` contrôleur, le `Details` (méthode) et un `ID` valeur.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-108">In this case it's a `GET` request with three URL segments, the `Movies` controller, the `Details` method and a `ID` value.</span></span>

<span data-ttu-id="ec2c8-109">Code tout d’abord vous permettent de rechercher des données à l’aide du `Find` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ec2c8-109">Code First makes it easy to search for data using the `Find` method.</span></span> <span data-ttu-id="ec2c8-110">Une fonctionnalité de sécurité importante intégrée à la méthode est que le code vérifie que le `Find` méthode a trouvé un film avant que le code tente de faire quelque chose avec lui.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-110">An important security feature built into the method is that the code verifies that the `Find` method has found a movie before the code tries to do anything with it.</span></span> <span data-ttu-id="ec2c8-111">Par exemple, un pirate pourrait induire des erreurs dans le site en modifiant l’URL créée par les liens à partir de `http://localhost:xxxx/Movies/Details/1` à quelque chose comme `http://localhost:xxxx/Movies/Details/12345` (ou une autre valeur qui ne représente pas un film réel).</span><span class="sxs-lookup"><span data-stu-id="ec2c8-111">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="ec2c8-112">Si vous n’avez pas coché pour un film null, un film null entraînerait une erreur de base de données.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-112">If you did not check for a null movie, a null movie would result in a database error.</span></span>

<span data-ttu-id="ec2c8-113">Examinez les méthodes `Delete` et `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-113">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

<span data-ttu-id="ec2c8-114">Notez que la `HTTP Get``Delete` méthode ne supprime pas le film spécifié, il retourne une vue de la séquence dans laquelle vous pouvez envoyer (`HttpPost`) la suppression...</span><span class="sxs-lookup"><span data-stu-id="ec2c8-114">Note that the `HTTP Get``Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (`HttpPost`) the deletion..</span></span> <span data-ttu-id="ec2c8-115">L’exécution d’une opération de suppression en réponse à une requête GET (ou encore l’exécution d’une opération de modification, d’une opération de création ou de toute autre opération qui modifie des données) génère une faille de sécurité.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-115">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span> <span data-ttu-id="ec2c8-116">Pour plus d’informations, consultez le billet de blog de Stephen Walther [ASP.NET MVC Conseil #46 : n’utilisez pas supprimer les liens parce qu’elles créent des failles de sécurité](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec2c8-116">For more information about this, see Stephen Walther's blog entry [ASP.NET MVC Tip #46 — Don't use Delete Links because they create Security Holes](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).</span></span>

<span data-ttu-id="ec2c8-117">La méthode `HttpPost` qui supprime les données est nommée `DeleteConfirmed` pour donner à la méthode HTTP POST une signature ou un nom unique.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-117">The `HttpPost` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="ec2c8-118">Les signatures des deux méthodes sont illustrées ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="ec2c8-118">The two method signatures are shown below:</span></span>

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

<span data-ttu-id="ec2c8-119">Le Common Language Runtime (CLR) nécessite des méthodes surchargées pour avoir une signature à paramètre unique (même nom de méthode, mais liste de paramètres différentes).</span><span class="sxs-lookup"><span data-stu-id="ec2c8-119">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="ec2c8-120">Toutefois, vous devez ici deux méthodes de suppression : un pour GET--et un pour valider que les deux ont la même signature de paramètre.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-120">However, here you need two Delete methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="ec2c8-121">(Elles doivent toutes les deux accepter un entier unique comme paramètre.)</span><span class="sxs-lookup"><span data-stu-id="ec2c8-121">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="ec2c8-122">Pour trier sur ce point, vous pouvez effectuer deux choses.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-122">To sort this out, you can do a couple of things.</span></span> <span data-ttu-id="ec2c8-123">Une consiste à attribuer aux méthodes des noms différents.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-123">One is to give the methods different names.</span></span> <span data-ttu-id="ec2c8-124">C’est ce qu’a fait le mécanisme de génération de modèles automatique dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-124">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="ec2c8-125">Toutefois, elle présente un petit problème : ASP.NET mappe des segments d’une URL à des méthodes d’action par nom. Si vous renommez une méthode, il est probable que le routage ne pourra pas trouver cette méthode.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-125">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="ec2c8-126">La solution consiste à faire ce que vous voyez dans l’exemple, c’est-à-dire à ajouter l’attribut `ActionName("Delete")` à la méthode `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-126">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="ec2c8-127">Cela exécute efficacement mappage pour le système de routage afin qu’une URL qui inclut */Delete/* un billet demande trouveront le `DeleteConfirmed` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ec2c8-127">This effectively performs mapping for the routing system so that a URL that includes */Delete/* for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="ec2c8-128">Une autre méthode pour éviter tout problème avec les méthodes qui ont des signatures et des noms identiques est artificiellement modifier la signature de la méthode POST pour inclure un paramètre inutilisé.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-128">Another common way to avoid a problem with methods that have identical names and signatures is to artificially change the signature of the POST method to include an unused parameter.</span></span> <span data-ttu-id="ec2c8-129">Par exemple, certains développeurs ajoutent un type de paramètre `FormCollection` qui est passé à la méthode POST et puis simplement n’utilisez pas le paramètre :</span><span class="sxs-lookup"><span data-stu-id="ec2c8-129">For example, some developers add a parameter type `FormCollection` that is passed to the POST method, and then simply don't use the parameter:</span></span>

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a><span data-ttu-id="ec2c8-130">Résumé</span><span class="sxs-lookup"><span data-stu-id="ec2c8-130">Summary</span></span>

<span data-ttu-id="ec2c8-131">Vous avez maintenant une application ASP.NET MVC complète qui stocke les données dans une base de données de base de données locale.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-131">You now have a complete ASP.NET MVC application that stores data in a local DB database.</span></span> <span data-ttu-id="ec2c8-132">Vous pouvez créer, lire, mettre à jour, supprimer et rechercher des films.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-132">You can create, read, update, delete, and search for movies.</span></span>

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a><span data-ttu-id="ec2c8-133">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="ec2c8-133">Next Steps</span></span>

<span data-ttu-id="ec2c8-134">Après avoir généré et testé une application web, l’étape suivante consiste à rendre disponible à d’autres personnes à utiliser sur Internet.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-134">After you have built and tested a web application, the next step is to make it available to other people to use over the Internet.</span></span> <span data-ttu-id="ec2c8-135">Pour ce faire, vous devez déployer vers un fournisseur d’hébergement web.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-135">To do that, you have to deploy it to a web hosting provider.</span></span> <span data-ttu-id="ec2c8-136">Microsoft propose d’hébergement web gratuit pour jusqu'à 10 sites web dans un [libérer le compte d’évaluation Azure](https://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="ec2c8-136">Microsoft offers free web hosting for up to 10 web sites in a [free Azure trial account](https://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="ec2c8-137">Je vous suggère ensuite suivre mon didacticiel [déployer une application Secure ASP.NET MVC avec l’appartenance, OAuth et base de données SQL Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="ec2c8-137">I suggest you next follow my tutorial [Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="ec2c8-138">Un excellent didacticiel est de niveau intermédiaire de Tom Dykstra [création d’un modèle de données Entity Framework pour une Application ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="ec2c8-138">An excellent tutorial is Tom Dykstra's intermediate-level [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="ec2c8-139">[StackOverflow](http://stackoverflow.com/help) et [forums d’ASP.NET MVC](https://forums.asp.net/1146.aspx) sont une bonne place pour poser des questions.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-139">[Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are a great places to ask questions.</span></span> <span data-ttu-id="ec2c8-140">Suivez [me](https://twitter.com/RickAndMSFT) sur twitter, vous pouvez obtenir les mises à jour sur mon didacticiels plus récente.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-140">Follow [me](https://twitter.com/RickAndMSFT) on twitter so you can get updates on my latest tutorials.</span></span>

<span data-ttu-id="ec2c8-141">Commentaires sont Bienvenue.</span><span class="sxs-lookup"><span data-stu-id="ec2c8-141">Feedback is welcome.</span></span>

<span data-ttu-id="ec2c8-142">— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter :[@RickAndMSFT](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ec2c8-142">— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)</span></span>  
<span data-ttu-id="ec2c8-143">— [Scott Hanselman](http://www.hanselman.com/blog/) twitter :[@shanselman](https://twitter.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="ec2c8-143">— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="ec2c8-144">Précédent</span><span class="sxs-lookup"><span data-stu-id="ec2c8-144">Previous</span></span>](adding-validation.md)