---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: L’activation des requêtes Cross-Origin dans ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Montre comment prendre en charge le partage des ressources Cross-Origin (CORS) dans l’API Web ASP.NET.
ms.author: aspnetcontent
ms.date: 07/15/2014
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7ac6158c2365aa324cefe97db044f568a1a43795
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805410"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a>L’activation des requêtes Cross-Origin dans ASP.NET Web API 2
====================
par [Mike Wasson](https://github.com/MikeWasson)

> La sécurité du navigateur empêche une page web d’effectuer des demandes AJAX vers un autre domaine. Cette restriction est appelée la *stratégie de même origine* et empêche un site malveillant de lire des données sensibles à partir d’un autre site. Toutefois, vous pouvez parfois laisser les autres sites à appeler votre API web.
> 
> [Cross-origine partage de ressources](http://www.w3.org/TR/cors/) (CORS) est une norme W3C qui permet à un serveur d’abaisser la stratégie de même origine. À l’aide de CORS, un serveur peut autoriser explicitement certaines demandes cross-origin lors du refus d’autres. CORS est plus sûre et plus flexible que des techniques antérieures telles que [JSONP](http://en.wikipedia.org/wiki/JSONP). Ce didacticiel montre comment activer CORS dans votre application API Web.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - [Visual Studio 2013 Update 2](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2.2


<a id="intro"></a>
## <a name="introduction"></a>Introduction

Ce didacticiel illustre la que prise en charge de CORS dans l’API Web ASP.NET. Nous allons commencer en créant deux projets ASP.NET : une appelée « WebService », qui héberge un contrôleur d’API Web, et l’autre appelée « WebClient », qui appelle le service Web. Étant donné que les deux applications sont hébergées sur des domaines différents, une requête AJAX à partir de WebClient pour le service Web est une demande de cross-origin.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>Quelle est la « Même origine » ?

Deux URL ont la même origine que s’ils ont des ports, des hôtes et des schémas identiques. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Ces deux URL ayant la même origine :

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Ces URL ont des origines différentes aux deux précédentes :

- `http://example.net` -Autre domaine
- `http://example.com:9000/foo.html` -Autre port
- `https://example.com/foo.html` -Schéma différent
- `http://www.example.com/foo.html` -Autre sous-domaine

> [!NOTE]
> Internet Explorer ne considère pas le port lors de la comparaison des origines.


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a>Créer le projet WebService

> [!NOTE]
> Cette section suppose que vous savez déjà comment créer des projets d’API Web. Dans le cas contraire, consultez [mise en route avec ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).


Démarrez Visual Studio et créez un nouveau **Application Web ASP.NET** projet. Sélectionnez le **vide** modèle de projet. Sous « Ajouter des dossiers et les références principales pour », sélectionnez le **API Web** case à cocher. Si vous le souhaitez, sélectionnez l’option « Hôte dans le Cloud » pour déployer l’application sur Microsoft Azure. Microsoft propose d’hébergement web gratuit pour jusqu'à 10 sites Web dans un [compte d’essai Azure gratuit](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

Ajouter un contrôleur d’API Web nommé `TestController` avec le code suivant :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

Vous pouvez exécuter l’application localement ou déployez-la dans Azure. (Pour les captures d’écran dans ce didacticiel, j’ai déployé sur Azure App Service Web Apps). Pour vérifier l’utilisation de l’API web, accédez à `http://hostname/api/test/`, où *nom d’hôte* est le domaine où vous avez déployé l’application. Vous devez voir le texte de réponse, &quot;obtenir : Message de Test&quot;.

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a>Créer le projet de WebClient

Créez un autre projet d’Application Web ASP.NET et sélectionnez le **MVC** modèle de projet. Si vous le souhaitez, sélectionnez **modifier l’authentification** > **aucune authentification**. Vous n’avez pas besoin d’authentification pour ce didacticiel.

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

Dans l’Explorateur de solutions, ouvrez le fichier Views/Home/Index.cshtml. Remplacez le code dans ce fichier avec les éléments suivants :

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

Pour le *serviceUrl* variable, utilisez l’URI de l’application de service Web. Maintenant exécuter l’application de WebClient localement ou la publier sur un autre site Web.

En cliquant sur le bouton « essayer » envoie une requête AJAX à l’application de service Web, à l’aide de la méthode HTTP répertoriée dans la liste déroulante (GET, POST ou PUT). Cela nous permet d’examiner les différentes demandes cross-origin. À ce stade, l’application de service Web ne prend pas en charge CORS, donc si vous cliquez sur le bouton, vous obtiendrez une erreur.

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Si vous regardez le trafic HTTP dans un outil tel que [Fiddler](http://www.telerik.com/fiddler), vous verrez que le navigateur envoie la demande GET et la demande réussit, mais l’appel AJAX retourne une erreur. Il est important de comprendre que stratégie de même origine n’empêche pas le navigateur à partir de *envoi* la demande. Au lieu de cela, elle empêche l’application de voir les *réponse*.


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a>Activer CORS

Maintenant nous allons activer CORS dans l’application de service Web. Tout d’abord, ajoutez le package NuGet de CORS. Dans Visual Studio, à partir de la **outils** menu, sélectionnez **Library Package Manager**, puis sélectionnez **Console du Gestionnaire de Package**. Dans la fenêtre de Console du Gestionnaire de Package, tapez la commande suivante :

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Cette commande installe le dernier package et met à jour toutes les dépendances, y compris les bibliothèques d’API Web core. Utilisateur - indicateur de Version pour cibler une version spécifique. Le package CORS requiert Web API 2.0 ou version ultérieure.

Ouvrez le fichier App\_Start/WebApiConfig.cs. Ajoutez le code suivant à la **WebApiConfig.Register** (méthode).

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Ensuite, ajoutez le **[EnableCors]** attribut le `TestController` classe :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Pour le *origines* paramètre, utilisez l’URI où vous avez déployé l’application de WebClient. Cela permet des requêtes cross-origin à partir de WebClient, tout toujours interdit toutes les autres demandes inter-domaines. Plus tard, je vais décrire les paramètres pour **[EnableCors]** plus en détail.

N’incluez pas une barre oblique à la fin de la *origines* URL.

Redéployer l’application de service Web mis à jour. Vous n’avez pas besoin de mettre à jour de WebClient. La requête AJAX à partir de WebClient doit réussir. Toutes les méthodes GET, PUT et POST sont autorisées.

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a>Fonctionnement de CORS

Cette section décrit ce qui se passe dans une demande CORS, au niveau des messages HTTP. Il est important de comprendre le fonctionnement de CORS, afin que vous puissiez configurer le **[EnableCors]** attribut correctement et résoudre les problèmes si les choses ne fonctionnent pas comme prévu.

La spécification CORS introduit plusieurs nouveaux en-têtes HTTP qui permettent les demandes cross-origin. Si un navigateur prend en charge CORS, il définit ces en-têtes automatiquement pour les demandes cross-origin ; vous n’avez pas besoin de rien de spécial dans votre code JavaScript.

Voici un exemple d’une demande de cross-origin. L’en-tête « Origin » donne le domaine du site qui effectue la demande.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Si le serveur autorise la demande, il définit l’en-tête Access-Control-Allow-Origin. La valeur de cet en-tête correspond à l’en-tête d’origine, soit est la valeur de caractère générique «\*», ce qui signifie que toute origine est autorisée.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Si la réponse n’inclut pas l’en-tête Access-Control-Allow-Origin, la requête AJAX échoue. Plus précisément, le navigateur n’autorise pas la demande. Même si le serveur retourne une réponse correcte, le navigateur ne rend pas la réponse disponibles à l’application cliente.

**Demandes préliminaires**

Pour certaines requêtes CORS, le navigateur envoie une demande supplémentaire, appelée « demande préliminaire, » avant d’envoyer la demande réelle de la ressource.

Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :

- La méthode de demande est GET, HEAD ou POST, *et*
- L’application ne définit pas les en-têtes de demande autre que Accept, Accept-Language, Content-Language, Content-Type ou dernière--ID d’événement, *et*
- L’en-tête `Content-Type` (si défini) est une des opérations suivantes : 

    - application/x-www-form-urlencoded
    - multipart/form-data
    - texte/brut

La règle sur les en-têtes de demande s’applique aux en-têtes de l’application définit en appelant **setRequestHeader** sur le **XMLHttpRequest** objet. (La spécification CORS les appelle « en-têtes de demande auteur ».) La règle ne s’applique pas aux en-têtes de la *navigateur* pouvez définir, telles que l’Agent utilisateur, hôte ou Content-Length.

Voici un exemple d’une requête préliminaire :

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

La demande de contrôle préliminaire utilise la méthode HTTP OPTIONS. Il comprend deux en-têtes spéciaux :

- Access-Control-Request-Method : Méthode HTTP qui sera utilisée pour la demande réelle.
- Access-Control-Request-Headers : Une liste d’en-têtes de demande qui les *application* définie sur la demande réelle. (Là encore, cela n’inclut pas les en-têtes qui définit le navigateur.)

Voici un exemple de réponse, en supposant que le serveur autorise la demande :

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

La réponse inclut un en-tête `Access-Control-Allow-Methods` qui répertorie les méthodes autorisées et éventuellement un en-tête `Access-Control-Allow-Headers`, qui répertorie les en-têtes autorisés. Si la demande préliminaire réussit, le navigateur envoie la demande réelle, comme décrit précédemment.

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a>Règles de portée pour [EnableCors]

Vous pouvez activer CORS par action, par contrôleur, ou globalement pour tous les contrôleurs d’API Web dans votre application.

**Par Action**

Pour activer CORS pour une seule action, définissez la **[EnableCors]** attribut sur la méthode d’action. L’exemple suivant active CORS pour le `GetItem` méthode uniquement.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Par contrôleur**

Si vous définissez **[EnableCors]** sur la classe de contrôleur, il s’applique à toutes les actions sur le contrôleur. Pour désactiver CORS pour une action, ajoutez le **[DisableCors]** d’attribut à l’action. L’exemple suivant active CORS pour chaque méthode, à l’exception `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Dans le monde entier**

Pour activer CORS pour tous les contrôleurs d’API Web dans votre application, passez un **EnableCorsAttribute** l’instance à la **EnableCors** méthode :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Si vous définissez l’attribut à plusieurs étendues, l’ordre de priorité est :

1. Action
2. Contrôleur
3. Global

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a>Définir les origines autorisées

Le *origines* paramètre de la **[EnableCors]** attribut spécifie les origines autorisées à accéder à la ressource. La valeur est une liste séparée par des virgules des origines autorisées.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

Vous pouvez également utiliser la valeur de caractère générique «\*» pour autoriser les demandes à partir de n’importe quel origines.

Réfléchissez bien avant d’autoriser des demandes à partir de n’importe quelle origine. Cela signifie que littéralement n’importe quel site Web peut effectuer les appels AJAX à votre API web.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a>Définir les méthodes HTTP autorisées

Le *méthodes* paramètre de la **[EnableCors]** attribut spécifie les méthodes HTTP sont autorisés à accéder à la ressource. Pour autoriser toutes les méthodes, utilisez la valeur de caractère générique «\*». L’exemple suivant autorise uniquement les requêtes GET et POST.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a>Définir les en-têtes de requête

J’ai décrit précédemment comment une requête préliminaire peut inclure un en-tête Access-Control-Request-Headers, répertoriant les en-têtes HTTP définis par l’application (la soi-disant « author des en-têtes de demande »). Le *en-têtes* paramètre de la **[EnableCors]** attribut spécifie les en-têtes de requête d’auteur sont autorisés. Pour autoriser tous les en-têtes, définissez *en-têtes* à «\*». À la liste verte des en-têtes spécifiques, définissez *en-têtes* à une liste séparée par des virgules d’en-têtes autorisés :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Toutefois, les navigateurs ne sont pas entièrement cohérents dans la façon dont elles définies Access-Control-Request-Headers. Par exemple, Chrome inclut actuellement « origin » ; Bien que FireFox n’inclut pas les en-têtes standard tels que « Accepter », même lorsque l’application les définit dans le script.

Si vous définissez *en-têtes* sur n’importe quelle autre que «\*», vous devez inclure au moins « accepter », « content-type » et « origin », ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a>Définir les en-têtes de réponse autorisés

Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application. Les en-têtes de réponse sont disponibles par défaut sont :

- Cache-Control
- Content-Language
- Content-Type
- Arrive à expiration
- Dernière modification
- Pragma

La spécification CORS appelle ces en-têtes : [les en-têtes de réponse simple](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Pour que les autres en-têtes accessibles à l’application, définissez la *exposedHeaders* paramètre de **[EnableCors]**.

Dans de l’exemple suivant, le contrôleur `Get` méthode définit un en-tête personnalisé nommé « X-Custom-Header ». Par défaut, le navigateur exposera pas cet en-tête dans une demande de cross-origin. Pour libérer de l’en-tête, inclure « X-Custom-Header » dans *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a>Informations d’identification de passage dans les demandes Cross-Origin

Les informations d’identification nécessitent un traitement particulier dans une demande CORS. Par défaut, le navigateur n’envoie pas d’informations d’identification avec une demande de cross-origin. Les informations d’identification incluent les cookies, ainsi que des schémas d’authentification HTTP. Pour envoyer des informations d’identification avec une demande de cross-origin, le client doit définir **XMLHttpRequest.withCredentials** sur true.

À l’aide de **XMLHttpRequest** directement :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

Dans jQuery :

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

En outre, le serveur doit autoriser les informations d’identification. Pour autoriser les informations d’identification de cross-origine dans l’API Web, définissez la **SupportsCredentials** propriété sur true sur le **[EnableCors]** attribut :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Si cette propriété est true, la réponse HTTP inclut un en-tête Access-Control-Allow-Credentials. Cet en-tête indique au navigateur que le serveur autorise les informations d’identification pour une demande de cross-origin.

Si le navigateur envoie les informations d’identification, mais la réponse n’inclut pas un en-tête Access-Control-Allow-Credentials valid, le navigateur n’expose la réponse à l’application, et la requête AJAX échoue.

Soyez très prudent sur la configuration **SupportsCredentials** sur true, car cela signifie que d’un site Web à un autre domaine peut envoyer connecté de l’utilisateur à votre API Web sur l’utilisateur, sans que l’utilisateur en cours prenant en charge. La spécification CORS indique également ce paramètre *origines* à &quot; \* &quot; n’est pas valide si **SupportsCredentials** a la valeur true.

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a>Fournisseurs de stratégie CORS personnalisé

Le **[EnableCors]** attribut implémente le **ICorsPolicyProvider** interface. Vous pouvez fournir votre propre implémentation en créant une classe qui dérive de **attribut** et implémente **ICorsProlicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Vous pouvez maintenant appliquer l’attribut partout, que vous devez placer **[EnableCors]**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Par exemple, un fournisseur de stratégie CORS personnalisé peut lire les paramètres à partir d’un fichier de configuration.

Comme alternative à l’aide d’attributs, vous pouvez inscrire un **ICorsPolicyProviderFactory** objet crée **ICorsPolicyProvider** objets.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Pour définir le **ICorsPolicyProviderFactory**, appelez le **SetCorsPolicyProviderFactory** méthode d’extension au démarrage, comme suit :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a>Prise en charge d'un navigateur

Le package de l’API Web CORS est une technologie côté serveur. Navigateur de l’utilisateur doit également prendre en charge de CORS. Heureusement, les versions actuelles de tous les principaux navigateurs incluent [prise en charge de CORS](http://caniuse.com/cors).

Internet Explorer 8 et Internet Explorer 9 ont une prise en charge partielle de CORS, à l’aide de l’objet XDomainRequest qui hérité au lieu de XMLHttpRequest. Pour plus d’informations, consultez [XDomainRequest qui - Restrictions, les Limitations et les solutions de contournement](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).
