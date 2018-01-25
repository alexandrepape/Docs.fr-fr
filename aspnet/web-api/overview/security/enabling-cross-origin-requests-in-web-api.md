---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: "L’activation de demandes Cross-Origin dans ASP.NET Web API 2 | Documents Microsoft"
author: MikeWasson
description: "Montre comment prendre en charge le partage de ressources Cross-Origin (CORS) dans l’API Web ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="0321d-103">L’activation de demandes Cross-Origin dans ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="0321d-103">Enabling Cross-Origin Requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="0321d-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0321d-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="0321d-105">Sécurité du navigateur empêche une page web d’effectuer des demandes AJAX vers un autre domaine.</span><span class="sxs-lookup"><span data-stu-id="0321d-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="0321d-106">Cette restriction est appelée le *stratégie de même origine*et un site malveillant empêche de lire des données sensibles à partir d’un autre site.</span><span class="sxs-lookup"><span data-stu-id="0321d-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="0321d-107">Toutefois, vous pouvez parfois permettent d’autres sites appeler votre API web.</span><span class="sxs-lookup"><span data-stu-id="0321d-107">However, sometimes you might want to let other sites call your web API.</span></span>
> 
> <span data-ttu-id="0321d-108">[Cross-origine partage des ressources](http://www.w3.org/TR/cors/) (CORS) est une norme W3C qui permet à un serveur d’abaisser la stratégie de même origine.</span><span class="sxs-lookup"><span data-stu-id="0321d-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="0321d-109">À l’aide de CORS, un serveur peut autoriser explicitement les certaines demandes cross-origin lors du refus d’autres.</span><span class="sxs-lookup"><span data-stu-id="0321d-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="0321d-110">CORS est plus sûre et plus flexible qu’antérieures techniques telles que [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="0321d-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="0321d-111">Ce didacticiel montre comment activer les CORS dans votre application d’API Web.</span><span class="sxs-lookup"><span data-stu-id="0321d-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0321d-112">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="0321d-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="0321d-113">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="0321d-113">Visual Studio 2013 Update 2</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="0321d-114">2.2 d’API Web</span><span class="sxs-lookup"><span data-stu-id="0321d-114">Web API 2.2</span></span>


<a id="intro"></a>
## <a name="introduction"></a><span data-ttu-id="0321d-115">Introduction</span><span class="sxs-lookup"><span data-stu-id="0321d-115">Introduction</span></span>

<span data-ttu-id="0321d-116">Ce didacticiel montre que Cors prennent en charge dans l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0321d-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="0321d-117">Nous allons commencer en créant deux projets ASP.NET : une appelée « WebService », qui héberge le contrôleur d’API Web, et l’autres appelée « WebClient », qui appelle le service Web.</span><span class="sxs-lookup"><span data-stu-id="0321d-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="0321d-118">Étant donné que les deux applications sont hébergées sur des domaines différents, une requête AJAX à partir de WebClient au service Web est une demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0321d-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="0321d-119">Quelle est la « Même origine » ?</span><span class="sxs-lookup"><span data-stu-id="0321d-119">What is "Same Origin"?</span></span>

<span data-ttu-id="0321d-120">Deux URL ayant la même origine, s’ils ont des ports, des hôtes et des schémas identiques.</span><span class="sxs-lookup"><span data-stu-id="0321d-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="0321d-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="0321d-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="0321d-122">Ces deux URL ayant la même origine :</span><span class="sxs-lookup"><span data-stu-id="0321d-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="0321d-123">Ces URL ont des origines différentes à la précédente deux :</span><span class="sxs-lookup"><span data-stu-id="0321d-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="0321d-124">`http://example.net`-Autre domaine</span><span class="sxs-lookup"><span data-stu-id="0321d-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="0321d-125">`http://example.com:9000/foo.html`-Autre port</span><span class="sxs-lookup"><span data-stu-id="0321d-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="0321d-126">`https://example.com/foo.html`-Autre schéma</span><span class="sxs-lookup"><span data-stu-id="0321d-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="0321d-127">`http://www.example.com/foo.html`-Autre sous-domaine</span><span class="sxs-lookup"><span data-stu-id="0321d-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="0321d-128">Internet Explorer ne considère pas le port lors de la comparaison des origines.</span><span class="sxs-lookup"><span data-stu-id="0321d-128">Internet Explorer does not consider the port when comparing origins.</span></span>


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a><span data-ttu-id="0321d-129">Créer le projet de service Web</span><span class="sxs-lookup"><span data-stu-id="0321d-129">Create the WebService Project</span></span>

> [!NOTE]
> <span data-ttu-id="0321d-130">Cette section suppose que vous savez déjà comment créer des projets d’API Web.</span><span class="sxs-lookup"><span data-stu-id="0321d-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="0321d-131">Dans le cas contraire, consultez [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="0321d-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>


<span data-ttu-id="0321d-132">Démarrez Visual Studio et créez un nouveau **Application Web ASP.NET** projet.</span><span class="sxs-lookup"><span data-stu-id="0321d-132">Start Visual Studio and create a new **ASP.NET Web Application** project.</span></span> <span data-ttu-id="0321d-133">Sélectionnez le **vide** modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="0321d-133">Select the **Empty** project template.</span></span> <span data-ttu-id="0321d-134">Sous « Ajouter des dossiers et pour les références de base », sélectionnez le **API Web** case à cocher.</span><span class="sxs-lookup"><span data-stu-id="0321d-134">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="0321d-135">Si vous le souhaitez, sélectionnez l’option « Hôte dans le Cloud » pour déployer l’application sur Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="0321d-135">Optionally, select the "Host in Cloud" option to deploy the app to Mircosoft Azure.</span></span> <span data-ttu-id="0321d-136">Microsoft propose d’hébergement web gratuit pour jusqu'à 10 sites Web dans un [libérer le compte d’évaluation Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="0321d-136">Microsoft offers free web hosting for up to 10 websites in a [free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

<span data-ttu-id="0321d-137">Ajouter un contrôleur d’API Web nommé `TestController` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0321d-137">Add a Web API controller named `TestController` with the following code:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

<span data-ttu-id="0321d-138">Vous pouvez exécuter l’application localement ou déployer sur Azure.</span><span class="sxs-lookup"><span data-stu-id="0321d-138">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="0321d-139">(Pour les captures d’écran de ce didacticiel, j’ai déployé sur Azure App Service Web Apps). Pour vérifier l’utilisation de l’API web, accédez à `http://hostname/api/test/`, où *nom d’hôte* est le domaine où vous avez déployé l’application.</span><span class="sxs-lookup"><span data-stu-id="0321d-139">(For the screenshots in this tutorial, I deployed to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="0321d-140">Vous devez voir le texte de réponse, &quot;obtenir : Message de Test&quot;.</span><span class="sxs-lookup"><span data-stu-id="0321d-140">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a><span data-ttu-id="0321d-141">Créer le projet de WebClient</span><span class="sxs-lookup"><span data-stu-id="0321d-141">Create the WebClient Project</span></span>

<span data-ttu-id="0321d-142">Créez un autre projet d’Application Web ASP.NET et sélectionnez le **MVC** modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="0321d-142">Create another ASP.NET Web Application project and select the **MVC** project template.</span></span> <span data-ttu-id="0321d-143">Si vous le souhaitez, sélectionnez **modifier l’authentification** > **aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="0321d-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="0321d-144">Vous n’avez pas besoin d’authentification pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="0321d-144">You don't need authentication for this tutorial.</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

<span data-ttu-id="0321d-145">Dans l’Explorateur de solutions, ouvrez le fichier Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="0321d-145">In Solution Explorer, open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="0321d-146">Remplacez le code dans ce fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="0321d-146">Replace the code in this file with the following:</span></span>

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

<span data-ttu-id="0321d-147">Pour le *serviceUrl* variable, utilisez l’URI de l’application de service Web.</span><span class="sxs-lookup"><span data-stu-id="0321d-147">For the *serviceUrl* variable, use the URI of the WebService app.</span></span> <span data-ttu-id="0321d-148">Maintenant, exécutez l’application de WebClient localement ou le publier sur un autre site Web.</span><span class="sxs-lookup"><span data-stu-id="0321d-148">Now run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="0321d-149">En cliquant sur le bouton « essayez » soumet une requête AJAX à l’application de service Web, à l’aide de la méthode HTTP répertoriée dans la liste déroulante (GET, POST ou PUT).</span><span class="sxs-lookup"><span data-stu-id="0321d-149">Clicking the "Try It" button submits an AJAX request to the WebService app, using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="0321d-150">Cela nous permet d’examiner les différentes demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0321d-150">This lets us examine different cross-origin requests.</span></span> <span data-ttu-id="0321d-151">Maintenant, l’application de service Web ne prend pas en charge les CORS, donc si vous cliquez sur le bouton, vous obtiendrez une erreur.</span><span class="sxs-lookup"><span data-stu-id="0321d-151">Right now, the WebService app does not support CORS, so if you click the button, you will get an error.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="0321d-152">Si vous regardez le trafic HTTP dans un outil tel que [Fiddler](http://www.telerik.com/fiddler), vous verrez que le navigateur envoie la demande GET et la demande réussit, mais l’appel AJAX renvoie une erreur.</span><span class="sxs-lookup"><span data-stu-id="0321d-152">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you will see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="0321d-153">Il est important de comprendre que la stratégie de même origine n’empêche pas le navigateur à partir de *envoi* la demande.</span><span class="sxs-lookup"><span data-stu-id="0321d-153">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="0321d-154">Au lieu de cela, elle empêche l’application de voir les *réponse*.</span><span class="sxs-lookup"><span data-stu-id="0321d-154">Instead, it prevents the application from seeing the *response*.</span></span>


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a><span data-ttu-id="0321d-155">Activer CORS</span><span class="sxs-lookup"><span data-stu-id="0321d-155">Enable CORS</span></span>

<span data-ttu-id="0321d-156">Maintenant nous allons activer CORS dans l’application de service Web.</span><span class="sxs-lookup"><span data-stu-id="0321d-156">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="0321d-157">Tout d’abord, ajoutez le package NuGet de CORS.</span><span class="sxs-lookup"><span data-stu-id="0321d-157">First, add the CORS NuGet package.</span></span> <span data-ttu-id="0321d-158">Dans Visual Studio, à partir de la **outils** menu, sélectionnez **Gestionnaire de Package de bibliothèque**, puis sélectionnez **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="0321d-158">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="0321d-159">Dans la fenêtre de Console du Gestionnaire de Package, tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="0321d-159">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="0321d-160">Cette commande installe le dernier package et met à jour toutes les dépendances, y compris les bibliothèques d’API Web core.</span><span class="sxs-lookup"><span data-stu-id="0321d-160">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="0321d-161">Utilisateur-indicateur de Version pour cibler une version spécifique.</span><span class="sxs-lookup"><span data-stu-id="0321d-161">User the -Version flag to target a specific version.</span></span> <span data-ttu-id="0321d-162">Le package CORS requiert Web API 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="0321d-162">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="0321d-163">Ouvrez le fichier App\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="0321d-163">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="0321d-164">Ajoutez le code suivant à la **WebApiConfig.Register** (méthode).</span><span class="sxs-lookup"><span data-stu-id="0321d-164">Add the following code to the **WebApiConfig.Register** method.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="0321d-165">Ensuite, ajoutez le **[EnableCors]** d’attribut à la `TestController` classe :</span><span class="sxs-lookup"><span data-stu-id="0321d-165">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="0321d-166">Pour le *origines* paramètre, utilisez l’URI où vous avez déployé l’application WebClient.</span><span class="sxs-lookup"><span data-stu-id="0321d-166">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="0321d-167">Cela permet au demandes cross-origin de WebClient, tout toujours interdire toutes les autres demandes inter-domaines.</span><span class="sxs-lookup"><span data-stu-id="0321d-167">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="0321d-168">Une version ultérieure, je vais décrire les paramètres pour **[EnableCors]** plus en détail.</span><span class="sxs-lookup"><span data-stu-id="0321d-168">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="0321d-169">N’incluez pas une barre oblique à la fin de la *origines* URL.</span><span class="sxs-lookup"><span data-stu-id="0321d-169">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="0321d-170">Redéployez l’application de service Web mis à jour.</span><span class="sxs-lookup"><span data-stu-id="0321d-170">Redeploy the updated WebService application.</span></span> <span data-ttu-id="0321d-171">Vous n’avez pas besoin de mettre à jour le service WebClient.</span><span class="sxs-lookup"><span data-stu-id="0321d-171">You don't need to update WebClient.</span></span> <span data-ttu-id="0321d-172">La requête AJAX à partir de WebClient doit à présent réussir.</span><span class="sxs-lookup"><span data-stu-id="0321d-172">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="0321d-173">Toutes les méthodes GET, PUT et POST sont autorisées.</span><span class="sxs-lookup"><span data-stu-id="0321d-173">The GET, PUT, and POST methods are all allowed.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a><span data-ttu-id="0321d-174">Fonctionnement des règles CORS</span><span class="sxs-lookup"><span data-stu-id="0321d-174">How CORS Works</span></span>

<span data-ttu-id="0321d-175">Cette section décrit ce qui se passe dans une demande CORS, au niveau des messages HTTP.</span><span class="sxs-lookup"><span data-stu-id="0321d-175">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="0321d-176">Il est important de comprendre le fonctionnement des règles CORS, afin que vous puissiez configurer le **[EnableCors]** attribut correctement et résoudre les problèmes si les éléments ne fonctionnent pas comme prévu.</span><span class="sxs-lookup"><span data-stu-id="0321d-176">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly, and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="0321d-177">La spécification CORS introduit plusieurs nouveaux en-têtes HTTP qui permettent les demandes cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0321d-177">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="0321d-178">Si un navigateur prend en charge CORS, il définit ces en-têtes automatiquement pour les demandes cross-origin ; vous n’avez pas besoin de faire quelque chose de spécial dans votre code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0321d-178">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="0321d-179">Voici un exemple de demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0321d-179">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="0321d-180">L’en-tête « Origine » donne le domaine du site qui effectue la demande.</span><span class="sxs-lookup"><span data-stu-id="0321d-180">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="0321d-181">Si le serveur autorise la demande, il définit l’en-tête Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="0321d-181">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="0321d-182">La valeur de cet en-tête correspond à l’en-tête d’origine, ou est la valeur de caractère générique «\*», ce qui signifie que toute origine est autorisée.</span><span class="sxs-lookup"><span data-stu-id="0321d-182">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="0321d-183">Si la réponse n’inclut pas l’en-tête Access-Control-Allow-Origin, la requête AJAX échoue.</span><span class="sxs-lookup"><span data-stu-id="0321d-183">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="0321d-184">Plus précisément, le navigateur n’autorise pas la demande.</span><span class="sxs-lookup"><span data-stu-id="0321d-184">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="0321d-185">Même si le serveur retourne une réponse correcte, le navigateur ne rend pas la réponse disponible à l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="0321d-185">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="0321d-186">**Demandes préliminaires**</span><span class="sxs-lookup"><span data-stu-id="0321d-186">**Preflight Requests**</span></span>

<span data-ttu-id="0321d-187">Pour certaines requêtes CORS, le navigateur envoie une demande supplémentaire, appelée « demande préliminaire, » avant d’envoyer la demande réelle de la ressource.</span><span class="sxs-lookup"><span data-stu-id="0321d-187">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="0321d-188">Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="0321d-188">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="0321d-189">La méthode de demande est GET, HEAD ou POST, *et*</span><span class="sxs-lookup"><span data-stu-id="0321d-189">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="0321d-190">L’application ne définit pas de tous les en-têtes de demande que Accept, Accept-Language, Content-Language, Content-Type ou dernier-ID d’événement, *et*</span><span class="sxs-lookup"><span data-stu-id="0321d-190">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="0321d-191">L’en-tête Content-Type (si défini) est une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="0321d-191">The Content-Type header (if set) is one of the following:</span></span> 

    - <span data-ttu-id="0321d-192">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="0321d-192">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="0321d-193">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="0321d-193">multipart/form-data</span></span>
    - <span data-ttu-id="0321d-194">texte brut</span><span class="sxs-lookup"><span data-stu-id="0321d-194">text/plain</span></span>

<span data-ttu-id="0321d-195">La règle sur les en-têtes de demande s’applique aux en-têtes de l’application définit en appelant **setRequestHeader** sur la **XMLHttpRequest** objet.</span><span class="sxs-lookup"><span data-stu-id="0321d-195">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="0321d-196">(La spécification CORS appelle ces « en-têtes de demande auteur ».) La règle ne s’applique pas aux en-têtes de la *navigateur* pouvez définir, telles que l’Agent utilisateur, ordinateur hôte ou Content-Length.</span><span class="sxs-lookup"><span data-stu-id="0321d-196">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="0321d-197">Voici un exemple d’une requête préliminaire :</span><span class="sxs-lookup"><span data-stu-id="0321d-197">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="0321d-198">La demande préliminaire utilise la méthode HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="0321d-198">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="0321d-199">Elle inclut deux en-têtes spéciales :</span><span class="sxs-lookup"><span data-stu-id="0321d-199">It includes two special headers:</span></span>

- <span data-ttu-id="0321d-200">Access-Control-Request-Method : Méthode HTTP qui sera utilisée pour la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="0321d-200">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="0321d-201">Access-Control-Request-Headers : Une liste des en-têtes de demande qui les *application* définie sur la demande réelle.</span><span class="sxs-lookup"><span data-stu-id="0321d-201">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="0321d-202">(Là encore, cela n’inclut pas les en-têtes qui définit par le navigateur.)</span><span class="sxs-lookup"><span data-stu-id="0321d-202">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="0321d-203">Voici un exemple de réponse, en supposant que le serveur autorise la demande :</span><span class="sxs-lookup"><span data-stu-id="0321d-203">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="0321d-204">La réponse inclut un en-tête Access-contrôle-Allow-Methods qui répertorie les méthodes autorisées et éventuellement un en-tête Access-Control-autoriser-Headers, qui répertorie les en-têtes autorisés.</span><span class="sxs-lookup"><span data-stu-id="0321d-204">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="0321d-205">Si la demande préliminaire réussit, le navigateur envoie la demande réelle, comme décrit précédemment.</span><span class="sxs-lookup"><span data-stu-id="0321d-205">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="0321d-206">Règles de portée pour [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="0321d-206">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="0321d-207">Vous pouvez activer CORS par action, par contrôleur, ou pour tous les contrôleurs de l’API Web dans votre application.</span><span class="sxs-lookup"><span data-stu-id="0321d-207">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="0321d-208">**Par Action**</span><span class="sxs-lookup"><span data-stu-id="0321d-208">**Per Action**</span></span>

<span data-ttu-id="0321d-209">Pour activer CORS pour une action unique, définissez la **[EnableCors]** attribut sur la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="0321d-209">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="0321d-210">L’exemple suivant active des règles CORS pour le `GetItem` méthode uniquement.</span><span class="sxs-lookup"><span data-stu-id="0321d-210">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="0321d-211">**Par contrôleur**</span><span class="sxs-lookup"><span data-stu-id="0321d-211">**Per Controller**</span></span>

<span data-ttu-id="0321d-212">Si vous définissez **[EnableCors]** sur la classe de contrôleur, il s’applique à toutes les actions sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="0321d-212">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="0321d-213">Pour désactiver les CORS pour une action, vous devez ajouter le **[DisableCors]** d’attribut à l’action.</span><span class="sxs-lookup"><span data-stu-id="0321d-213">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="0321d-214">L’exemple suivant active les CORS pour chaque méthode, à l’exception `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="0321d-214">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="0321d-215">**Global**</span><span class="sxs-lookup"><span data-stu-id="0321d-215">**Globally**</span></span>

<span data-ttu-id="0321d-216">Pour activer CORS pour tous les contrôleurs d’API Web dans votre application, passez un **EnableCorsAttribute** d’instance pour le **EnableCors** méthode :</span><span class="sxs-lookup"><span data-stu-id="0321d-216">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="0321d-217">Si vous définissez l’attribut à plusieurs étendues, l’ordre de priorité est :</span><span class="sxs-lookup"><span data-stu-id="0321d-217">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="0321d-218">Action</span><span class="sxs-lookup"><span data-stu-id="0321d-218">Action</span></span>
2. <span data-ttu-id="0321d-219">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="0321d-219">Controller</span></span>
3. <span data-ttu-id="0321d-220">Global</span><span class="sxs-lookup"><span data-stu-id="0321d-220">Global</span></span>

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a><span data-ttu-id="0321d-221">Définir les origines autorisées</span><span class="sxs-lookup"><span data-stu-id="0321d-221">Set the Allowed Origins</span></span>

<span data-ttu-id="0321d-222">Le *origines* paramètre de la **[EnableCors]** attribut spécifie les origines sont autorisées à accéder à la ressource.</span><span class="sxs-lookup"><span data-stu-id="0321d-222">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="0321d-223">La valeur est une liste séparée par des virgules des origines autorisées.</span><span class="sxs-lookup"><span data-stu-id="0321d-223">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="0321d-224">Vous pouvez également utiliser la valeur de caractère générique «\*» pour autoriser les demandes à partir de toutes les origines.</span><span class="sxs-lookup"><span data-stu-id="0321d-224">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="0321d-225">Réfléchissez bien avant d’autoriser des demandes à partir de n’importe quel origine.</span><span class="sxs-lookup"><span data-stu-id="0321d-225">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="0321d-226">Cela signifie que tout site Web peut effectuer des appels d’AJAX à votre API web.</span><span class="sxs-lookup"><span data-stu-id="0321d-226">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="0321d-227">Définir les méthodes HTTP autorisées</span><span class="sxs-lookup"><span data-stu-id="0321d-227">Set the Allowed HTTP Methods</span></span>

<span data-ttu-id="0321d-228">Le *méthodes* paramètre de la **[EnableCors]** attribut spécifie les méthodes HTTP sont autorisés à accéder à la ressource.</span><span class="sxs-lookup"><span data-stu-id="0321d-228">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="0321d-229">Pour autoriser toutes les méthodes, utilisez la valeur de caractère générique «\*».</span><span class="sxs-lookup"><span data-stu-id="0321d-229">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="0321d-230">L’exemple suivant autorise uniquement les requêtes GET et POST.</span><span class="sxs-lookup"><span data-stu-id="0321d-230">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="0321d-231">Définir les en-têtes de requête autorisée</span><span class="sxs-lookup"><span data-stu-id="0321d-231">Set the Allowed Request Headers</span></span>

<span data-ttu-id="0321d-232">Précédemment décrit comment une demande préliminaire peut inclure un en-tête Access-Control-Request-Headers, qui répertorie les en-têtes HTTP définis par l’application (appelé « author les en-têtes de demande »).</span><span class="sxs-lookup"><span data-stu-id="0321d-232">Earlier I described how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="0321d-233">Le *en-têtes* paramètre de la **[EnableCors]** attribut spécifie les en-têtes de requête d’auteur sont autorisés.</span><span class="sxs-lookup"><span data-stu-id="0321d-233">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="0321d-234">Pour autoriser tous les en-têtes, affectez *en-têtes* à «\*».</span><span class="sxs-lookup"><span data-stu-id="0321d-234">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="0321d-235">Pour les en-têtes spécifiques de la liste d’autorisation, jeu de *en-têtes* à une liste séparée par des virgules des en-têtes autorisés :</span><span class="sxs-lookup"><span data-stu-id="0321d-235">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="0321d-236">Toutefois, les navigateurs ne sont pas entièrement cohérents dans leur définition Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="0321d-236">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="0321d-237">Par exemple, Chrome inclut actuellement « origine » ; alors que FireFox n’inclut pas les en-têtes standard tels que « Accepter », même lorsque l’application les définit dans le script.</span><span class="sxs-lookup"><span data-stu-id="0321d-237">For example, Chrome currently includes "origin"; while FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="0321d-238">Si vous définissez *en-têtes* autre que «\*», vous devez inclure au moins « accepter », « content-type » et « origine », ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="0321d-238">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="0321d-239">Définir les en-têtes de réponse autorisées</span><span class="sxs-lookup"><span data-stu-id="0321d-239">Set the Allowed Response Headers</span></span>

<span data-ttu-id="0321d-240">Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application.</span><span class="sxs-lookup"><span data-stu-id="0321d-240">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="0321d-241">Les en-têtes de réponse qui sont disponibles par défaut sont :</span><span class="sxs-lookup"><span data-stu-id="0321d-241">The response headers that are available by default are:</span></span>

- <span data-ttu-id="0321d-242">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="0321d-242">Cache-Control</span></span>
- <span data-ttu-id="0321d-243">Content-Language</span><span class="sxs-lookup"><span data-stu-id="0321d-243">Content-Language</span></span>
- <span data-ttu-id="0321d-244">Type de contenu</span><span class="sxs-lookup"><span data-stu-id="0321d-244">Content-Type</span></span>
- <span data-ttu-id="0321d-245">Arrive à expiration</span><span class="sxs-lookup"><span data-stu-id="0321d-245">Expires</span></span>
- <span data-ttu-id="0321d-246">Dernière modification</span><span class="sxs-lookup"><span data-stu-id="0321d-246">Last-Modified</span></span>
- <span data-ttu-id="0321d-247">Pragma</span><span class="sxs-lookup"><span data-stu-id="0321d-247">Pragma</span></span>

<span data-ttu-id="0321d-248">La spécification CORS appelle ces [les en-têtes de réponse simple](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="0321d-248">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="0321d-249">Pour rendre les autres en-têtes accessibles à l’application, définissez la *exposedHeaders* paramètre de **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="0321d-249">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="0321d-250">Dans de l’exemple suivant, le contrôleur `Get` méthode définit un en-tête personnalisé nommé « En-tête X-personnalisé ».</span><span class="sxs-lookup"><span data-stu-id="0321d-250">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="0321d-251">Par défaut, le navigateur expose pas cet en-tête dans une demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0321d-251">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="0321d-252">Pour libérer de l’en-tête, inclure 'En-tête X-personnalisé' dans *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="0321d-252">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a><span data-ttu-id="0321d-253">Informations d’identification de passage dans les demandes Cross-Origin</span><span class="sxs-lookup"><span data-stu-id="0321d-253">Passing Credentials in Cross-Origin Requests</span></span>

<span data-ttu-id="0321d-254">Informations d’identification nécessitent un traitement particulier dans une demande CORS.</span><span class="sxs-lookup"><span data-stu-id="0321d-254">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="0321d-255">Par défaut, le navigateur n’envoie pas d’informations d’identification avec une demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0321d-255">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="0321d-256">Informations d’identification incluent les cookies, ainsi que des schémas d’authentification HTTP.</span><span class="sxs-lookup"><span data-stu-id="0321d-256">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="0321d-257">Pour envoyer des informations d’identification avec une demande cross-origin, le client doit définir **XMLHttpRequest.withCredentials** sur true.</span><span class="sxs-lookup"><span data-stu-id="0321d-257">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="0321d-258">À l’aide de **XMLHttpRequest** directement :</span><span class="sxs-lookup"><span data-stu-id="0321d-258">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="0321d-259">Dans jQuery :</span><span class="sxs-lookup"><span data-stu-id="0321d-259">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="0321d-260">En outre, le serveur doit autoriser les informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="0321d-260">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="0321d-261">Pour autoriser les informations d’identification cross-origine dans l’API Web, définissez la **SupportsCredentials** propriété la valeur true sur la **[EnableCors]** attribut :</span><span class="sxs-lookup"><span data-stu-id="0321d-261">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="0321d-262">Si cette propriété est true, la réponse HTTP inclut un en-tête Access-contrôle-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="0321d-262">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="0321d-263">Cet en-tête indique au navigateur que le serveur autorise les informations d’identification pour une demande cross-origin.</span><span class="sxs-lookup"><span data-stu-id="0321d-263">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="0321d-264">Si le navigateur envoie des informations d’identification, mais la réponse n’inclut pas un en-tête Access-contrôle-Allow-Credentials valid, le navigateur ne doit pas exposer la réponse à l’application et la requête AJAX échoue.</span><span class="sxs-lookup"><span data-stu-id="0321d-264">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="0321d-265">Soyez très prudent paramètre **SupportsCredentials** sur « true », car cela signifie qu’un site Web à un autre domaine peut envoyer des informations d’identification d’un utilisateur de connecté à votre API Web, le nom de l’utilisateur, sans que l’utilisateur en cours prenant en charge.</span><span class="sxs-lookup"><span data-stu-id="0321d-265">Be very careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="0321d-266">La spécification CORS indique également ce paramètre *origines* à &quot; \* &quot; n’est pas valide si **SupportsCredentials** a la valeur true.</span><span class="sxs-lookup"><span data-stu-id="0321d-266">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a><span data-ttu-id="0321d-267">Fournisseurs de stratégie CORS personnalisé</span><span class="sxs-lookup"><span data-stu-id="0321d-267">Custom CORS Policy Providers</span></span>

<span data-ttu-id="0321d-268">Le **[EnableCors]** attribut implémente la **ICorsPolicyProvider** interface.</span><span class="sxs-lookup"><span data-stu-id="0321d-268">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="0321d-269">Vous pouvez fournir votre propre implémentation en créant une classe qui dérive de **attribut** et implémente **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="0321d-269">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="0321d-270">Maintenant vous pouvez appliquer l’attribut de tout endroit que vous devez placer **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="0321d-270">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="0321d-271">Par exemple, un fournisseur de stratégie CORS personnalisé peut lire les paramètres à partir d’un fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="0321d-271">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="0321d-272">Comme alternative à l’utilisation d’attributs, vous pouvez inscrire un **ICorsPolicyProviderFactory** objet crée **ICorsPolicyProvider** objets.</span><span class="sxs-lookup"><span data-stu-id="0321d-272">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="0321d-273">Pour définir le **ICorsPolicyProviderFactory**, appelez le **SetCorsPolicyProviderFactory** méthode d’extension au démarrage, comme suit :</span><span class="sxs-lookup"><span data-stu-id="0321d-273">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a><span data-ttu-id="0321d-274">Prise en charge d'un navigateur</span><span class="sxs-lookup"><span data-stu-id="0321d-274">Browser Support</span></span>

<span data-ttu-id="0321d-275">Le package Web API CORS est une technologie côté serveur.</span><span class="sxs-lookup"><span data-stu-id="0321d-275">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="0321d-276">Navigateur de l’utilisateur doit également prendre en charge de CORS.</span><span class="sxs-lookup"><span data-stu-id="0321d-276">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="0321d-277">Heureusement, les versions actuelles de tous les principaux navigateurs incluent [prise en charge de CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="0321d-277">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>

<span data-ttu-id="0321d-278">Internet Explorer 8 et Internet Explorer 9 ont une prise en charge partielle CORS, à l’aide de l’objet XDomainRequest qui hérité au lieu de XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="0321d-278">Internet Explorer 8 and Internet Explorer 9 have partial support for CORS, using the legacy XDomainRequest object instead of XMLHttpRequest.</span></span> <span data-ttu-id="0321d-279">Pour plus d’informations, consultez [XDomainRequest qui - solutions de contournement, Limitations et Restrictions](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span><span class="sxs-lookup"><span data-stu-id="0321d-279">For more information, see [XDomainRequest - Restrictions, Limitations and Workarounds](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span></span>