---
title: Créer des API web avec ASP.NET Core
author: scottaddie
description: Découvrez les fonctionnalités disponibles pour la création d’une API web dans ASP.NET Core et quand il convient d’utiliser chaque fonctionnalité.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: web-api/index
ms.openlocfilehash: ccee4f7bae0abe1b36088d58e5c1e1362d8de9f0
ms.sourcegitcommit: 5338b1ed9e2ef225ab565d6cba072b474fd9324d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39243095"
---
# <a name="build-web-apis-with-aspnet-core"></a>Créer des API web avec ASP.NET Core

Par [Scott Addie](https://github.com/scottaddie)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

Ce document explique comment créer une API web ASP.NET Core et quand il convient le plus d’utiliser chaque fonctionnalité.

## <a name="derive-class-from-controllerbase"></a>Dériver la classe de ControllerBase

Héritez de la classe [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) dans un contrôleur destiné à servir d’API web. Exemple :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

La classe `ControllerBase` fournit l’accès à de plusieurs propriétés et méthodes. Dans le code précédent, les exemples incluent [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) et [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). Ces méthodes sont appelées dans les méthodes d’action pour retourner les codes d’état HTTP 400 et 201, respectivement. La propriété [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), également fournie par `ControllerBase`, permet de gérer la validation des modèles de demande.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>Annoter la classe avec ApiControllerAttribute

ASP.NET Core 2.1 introduit l’attribut [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) pour désigner une classe de contrôleur d’API web. Exemple :

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Une version de compatibilité 2.1 ou ultérieure, définie via [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), est obligatoire pour utiliser cet attribut. Par exemple, le code en surbrillance dans *Startup.ConfigureServices* définit l’indicateur de compatibilité 2.1 :

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

L’attribut `[ApiController]` est généralement associé à `ControllerBase` pour donner aux contrôleurs un comportement spécifique à REST. `ControllerBase` permet d’accéder aux méthodes telles que [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) et [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).

Une autre approche consiste à créer une classe de contrôleur de base personnalisée annotée avec l’attribut `[ApiController]` :

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

Les sections suivantes décrivent les fonctionnalités pratiques ajoutées par l’attribut.

### <a name="automatic-http-400-responses"></a>Réponses HTTP 400 automatiques

Les erreurs de validation déclenchent automatiquement une réponse HTTP 400. Le code suivant devient inutile dans vos actions :

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

Le comportement par défaut est désactivé lorsque la propriété [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) est définie avec la valeur `true`. Ajoutez le code suivant dans *Startup.ConfigureServices* après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>Inférence de paramètre de source de liaison

Un attribut de source de liaison définit l’emplacement auquel se trouve la valeur d’un paramètre d’action. Les attributs de source de liaison suivants existent :

|Attribut|Source de liaison |
|---------|---------|
|**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**     | Corps de la requête |
|**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**     | Données de formulaire dans le corps de la demande |
|**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)** | En-tête de demande |
|**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**   | Paramètre de la chaîne de requête de la demande |
|**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**   | Données d’itinéraire à partir de la demande actuelle |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Service de demande injecté comme paramètre d’action |

> [!WARNING]
> N’utilisez pas `[FromRoute]` lorsque les valeurs risquent de contenir `%2f` (c’est-à-dire `/`). `%2f` ne sera pas sans la séquence d’échappement `/`. Utilisez `[FromQuery]` si la valeur peut contenir `%2f`.

Sans l’attribut `[ApiController]`, les attributs de source de liaison sont définis explicitement. Dans l’exemple suivant, l’attribut `[FromQuery]` indique que la valeur du paramètre `discontinuedOnly` est fournie dans la chaîne de requête de l’URL de demande :

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Des règles d’inférence sont appliquées pour les sources de données par défaut des paramètres d’action. Ces règles configurent les sources de liaison que vous seriez susceptible d’appliquer manuellement aux paramètres d’action. Les attributs de source de liaison se comportent comme suit :

* **[FromBody]** est déduit des paramètres de type complexe. Une exception à cette règle est tout type complexe intégré ayant une signification spéciale, comme [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) et [CancellationToken](/dotnet/api/system.threading.cancellationtoken). Le code de l’inférence de la source de liaison ignore ces types spéciaux. Quand une action a plusieurs paramètres explicitement spécifiés (par le biais de `[FromBody]`) ou déduits comme étant liés à partir du corps de la demande, une exception est levée. Par exemple, les signatures d’action suivantes génèrent une exception :

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]** est déduit pour les paramètres d’action de type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) et [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection). Il n’est pas déduit pour les types simples ou définis par l’utilisateur.
* **[FromRoute]** est déduit pour tout nom de paramètre d’action correspondant à un paramètre dans le modèle d’itinéraire. Quand plus d’un itinéraire correspond à un paramètre d’action, toute valeur d’itinéraire est considérée comme `[FromRoute]`.
* **[FromQuery]** est déduit pour tous les autres paramètres d’action.

Les règles d’inférence par défaut sont désactivées quand la propriété [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) est définie avec la valeur `true`. Ajoutez le code suivant dans *Startup.ConfigureServices* après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Inférence de demande multipart/form-data

Quand un paramètre d’action est annoté avec l’attribut [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute), le type de contenu de demande `multipart/form-data` est déduit.

Le comportement par défaut est désactivé quand la propriété [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) est définie avec la valeur `true`. Ajoutez le code suivant dans *Startup.ConfigureServices* après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Exigence du routage d’attribut

Le routage d’attribut devient une exigence. Exemple :

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Les actions sont inaccessibles par le biais des [itinéraires conventionnels](xref:mvc/controllers/routing#conventional-routing) définis dans [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) ou par [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dans *Startup.Configure*.

::: moniker-end

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
