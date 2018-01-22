---
title: Travailler avec des fichiers statiques dans ASP.NET Core
author: rick-anderson
description: "Découvrez comment traiter et sécuriser les fichiers statiques et configurer fichier statique qui héberge les comportements d’intergiciel (middleware) dans une application de web ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 60b205bf0a45e2965f9dab46f46956947ae513fd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a><span data-ttu-id="c8c82-103">Travailler avec des fichiers statiques dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8c82-103">Work with static files in ASP.NET Core</span></span>

<span data-ttu-id="c8c82-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="c8c82-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="c8c82-105">Fichiers statiques, tels que HTML, CSS, images et JavaScript, sont des ressources de qu'une application ASP.NET Core sert directement aux clients.</span><span class="sxs-lookup"><span data-stu-id="c8c82-105">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="c8c82-106">Une configuration est requise pour permettre au service de ces fichiers.</span><span class="sxs-lookup"><span data-stu-id="c8c82-106">Some configuration is required to enable to serving of these files.</span></span>

<span data-ttu-id="c8c82-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c8c82-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="c8c82-108">Les fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="c8c82-108">Serve static files</span></span>

<span data-ttu-id="c8c82-109">Fichiers statiques sont stockés dans le répertoire racine de votre projet web.</span><span class="sxs-lookup"><span data-stu-id="c8c82-109">Static files are stored within your project's web root directory.</span></span> <span data-ttu-id="c8c82-110">Le répertoire par défaut est  *\<content_root > / wwwroot*, mais il peut être modifié le [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) (méthode).</span><span class="sxs-lookup"><span data-stu-id="c8c82-110">The default directory is *\<content_root>/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="c8c82-111">Consultez [racine du contenu](xref:fundamentals/index#content-root) et [racine Web](xref:fundamentals/index#web-root) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="c8c82-111">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="c8c82-112">Hôte de l’application web doit être informé du répertoire racine du contenu.</span><span class="sxs-lookup"><span data-stu-id="c8c82-112">The app's web host must be made aware of the content root directory.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c8c82-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c8c82-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c8c82-114">Le `WebHost.CreateDefaultBuilder` méthode définit la racine du contenu dans le répertoire actif :</span><span class="sxs-lookup"><span data-stu-id="c8c82-114">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c8c82-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c8c82-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c8c82-116">Définissez comme racine contenue dans le répertoire en cours en appelant [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) à l’intérieur de `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="c8c82-116">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

<span data-ttu-id="c8c82-117">Fichiers statiques sont accessibles via un chemin d’accès relatif à la racine web.</span><span class="sxs-lookup"><span data-stu-id="c8c82-117">Static files are accessible via a path relative to the web root.</span></span> <span data-ttu-id="c8c82-118">Par exemple, le **Application Web** modèle de projet contient plusieurs dossiers dans le *wwwroot* dossier :</span><span class="sxs-lookup"><span data-stu-id="c8c82-118">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="c8c82-119">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="c8c82-119">**wwwroot**</span></span>
  * <span data-ttu-id="c8c82-120">**css**</span><span class="sxs-lookup"><span data-stu-id="c8c82-120">**css**</span></span>
  * <span data-ttu-id="c8c82-121">**images**</span><span class="sxs-lookup"><span data-stu-id="c8c82-121">**images**</span></span>
  * <span data-ttu-id="c8c82-122">**js**</span><span class="sxs-lookup"><span data-stu-id="c8c82-122">**js**</span></span>

<span data-ttu-id="c8c82-123">Le format d’URI pour accéder à un fichier dans le *images* sous-dossier est *http://\<server_address > /images/\<nom_fichier_image >*.</span><span class="sxs-lookup"><span data-stu-id="c8c82-123">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="c8c82-124">Par exemple, *http://localhost:9189/images/banner3.svg*.</span><span class="sxs-lookup"><span data-stu-id="c8c82-124">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c8c82-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c8c82-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c8c82-126">Si vous ciblez .NET Framework, ajoutez le [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package pour votre projet.</span><span class="sxs-lookup"><span data-stu-id="c8c82-126">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span> <span data-ttu-id="c8c82-127">Si le ciblage de .NET Core, la [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) inclut ce package.</span><span class="sxs-lookup"><span data-stu-id="c8c82-127">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c8c82-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c8c82-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c8c82-129">Ajouter le [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package pour votre projet.</span><span class="sxs-lookup"><span data-stu-id="c8c82-129">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span>

---

<span data-ttu-id="c8c82-130">Configurer le [intergiciel (middleware)](xref:fundamentals/middleware) qui permet les serveurs de fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="c8c82-130">Configure the [middleware](xref:fundamentals/middleware) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="c8c82-131">Les fichiers au sein de la racine web</span><span class="sxs-lookup"><span data-stu-id="c8c82-131">Serve files inside of web root</span></span>

<span data-ttu-id="c8c82-132">Appeler le [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) méthode `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="c8c82-132">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="c8c82-133">Sans paramètre `UseStaticFiles` surcharge de méthode marque les fichiers dans la racine web en tant que servable.</span><span class="sxs-lookup"><span data-stu-id="c8c82-133">The parameterless `UseStaticFiles` method overload marks the files in web root as servable.</span></span> <span data-ttu-id="c8c82-134">Les références suivantes de balisage *wwwroot/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="c8c82-134">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="c8c82-135">Les fichiers en dehors de la racine web</span><span class="sxs-lookup"><span data-stu-id="c8c82-135">Serve files outside of web root</span></span>

<span data-ttu-id="c8c82-136">Considérez une hiérarchie de répertoires dans lesquels les fichiers statiques pour être pris en charge résident en dehors de la racine web :</span><span class="sxs-lookup"><span data-stu-id="c8c82-136">Consider a directory hierarchy in which the static files to be served reside outside of the web root:</span></span>

* <span data-ttu-id="c8c82-137">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="c8c82-137">**wwwroot**</span></span>
  * <span data-ttu-id="c8c82-138">**css**</span><span class="sxs-lookup"><span data-stu-id="c8c82-138">**css**</span></span>
  * <span data-ttu-id="c8c82-139">**images**</span><span class="sxs-lookup"><span data-stu-id="c8c82-139">**images**</span></span>
  * <span data-ttu-id="c8c82-140">**js**</span><span class="sxs-lookup"><span data-stu-id="c8c82-140">**js**</span></span>
* <span data-ttu-id="c8c82-141">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="c8c82-141">**MyStaticFiles**</span></span>
  * <span data-ttu-id="c8c82-142">**images**</span><span class="sxs-lookup"><span data-stu-id="c8c82-142">**images**</span></span>
      * <span data-ttu-id="c8c82-143">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="c8c82-143">*banner1.svg*</span></span>

<span data-ttu-id="c8c82-144">Une demande peut accéder à la *banner1.svg* fichier en configurant l’intergiciel (middleware) de fichiers statiques comme suit :</span><span class="sxs-lookup"><span data-stu-id="c8c82-144">A request can access the *banner1.svg* file by configuring the static file middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="c8c82-145">Dans le code précédent, le *MyStaticFiles* hiérarchie de répertoires est exposée publiquement via la *StaticFiles* segment d’URI.</span><span class="sxs-lookup"><span data-stu-id="c8c82-145">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="c8c82-146">Une demande de *http://\<server_address > /StaticFiles/images/banner1.svg* sert le *banner1.svg* fichier.</span><span class="sxs-lookup"><span data-stu-id="c8c82-146">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="c8c82-147">Les références suivantes de balisage *MyStaticFiles/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="c8c82-147">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="c8c82-148">Définir les en-têtes de réponse HTTP</span><span class="sxs-lookup"><span data-stu-id="c8c82-148">Set HTTP response headers</span></span>

<span data-ttu-id="c8c82-149">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) objet peut être utilisé pour définir les en-têtes de réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="c8c82-149">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="c8c82-150">Outre la configuration des services de fichiers statiques à partir de la racine web, le code suivant définit le `Cache-Control` en-tête :</span><span class="sxs-lookup"><span data-stu-id="c8c82-150">In addition to configuring static file serving from the web root, the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="c8c82-151">Le [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) méthode existe dans le [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span><span class="sxs-lookup"><span data-stu-id="c8c82-151">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="c8c82-152">Les fichiers ont été apportées à la mise en cache publiquement pendant 10 minutes (600 secondes) :</span><span class="sxs-lookup"><span data-stu-id="c8c82-152">The files have been made publicly cacheable for 10 minutes (600 seconds):</span></span>

![En-têtes de réponse indiquant l’en-tête Cache-Control a été ajouté.](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="c8c82-154">Autorisation de fichier statique</span><span class="sxs-lookup"><span data-stu-id="c8c82-154">Static file authorization</span></span>

<span data-ttu-id="c8c82-155">L’intergiciel (middleware) de fichier statique ne fournit pas les vérifications d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="c8c82-155">The static file middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="c8c82-156">Tous les fichiers pris en charge par, notamment ceux de *wwwroot*, sont accessibles publiquement.</span><span class="sxs-lookup"><span data-stu-id="c8c82-156">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="c8c82-157">Pour traiter les fichiers en fonction de l’autorisation :</span><span class="sxs-lookup"><span data-stu-id="c8c82-157">To serve files based on authorization:</span></span>

* <span data-ttu-id="c8c82-158">Les stocker en dehors de *wwwroot* et n’importe quel répertoire accessible à l’intergiciel (middleware) de fichiers statiques **et**</span><span class="sxs-lookup"><span data-stu-id="c8c82-158">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>
* <span data-ttu-id="c8c82-159">Traiter les via une méthode d’action à laquelle l’autorisation est appliquée.</span><span class="sxs-lookup"><span data-stu-id="c8c82-159">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="c8c82-160">Retourner un [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) objet :</span><span class="sxs-lookup"><span data-stu-id="c8c82-160">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="c8c82-161">Activer l’exploration des répertoires</span><span class="sxs-lookup"><span data-stu-id="c8c82-161">Enable directory browsing</span></span>

<span data-ttu-id="c8c82-162">Exploration des répertoires permet aux utilisateurs de votre application web afficher une liste de répertoires et fichiers contenus dans un répertoire spécifié.</span><span class="sxs-lookup"><span data-stu-id="c8c82-162">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="c8c82-163">Exploration des répertoires est désactivée par défaut pour des raisons de sécurité (voir [considérations](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="c8c82-163">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="c8c82-164">Répertoire Enable navigation en appelant le [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) méthode dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="c8c82-164">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="c8c82-165">Ajouter des services requis en appelant le [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) méthode à partir de `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c8c82-165">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="c8c82-166">Le code précédent permet l’exploration des répertoires de la *wwwroot/images* dossier à l’aide de l’URL *http://\<server_address > / MyImages*, avec des liens vers chaque fichier et dossier :</span><span class="sxs-lookup"><span data-stu-id="c8c82-166">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![exploration des répertoires](static-files/_static/dir-browse.png)

<span data-ttu-id="c8c82-168">Consultez [considérations](#considerations) sur les risques de sécurité lors de l’exploration.</span><span class="sxs-lookup"><span data-stu-id="c8c82-168">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="c8c82-169">Notez les deux `UseStaticFiles` appelle dans l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="c8c82-169">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="c8c82-170">Le premier appel permet les serveurs de fichiers statiques dans le *wwwroot* dossier.</span><span class="sxs-lookup"><span data-stu-id="c8c82-170">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="c8c82-171">Le deuxième appel permet l’exploration des répertoires de la *wwwroot/images* dossier à l’aide de l’URL *http://\<server_address > / MyImages*:</span><span class="sxs-lookup"><span data-stu-id="c8c82-171">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="c8c82-172">Prendre en charge un document par défaut</span><span class="sxs-lookup"><span data-stu-id="c8c82-172">Serve a default document</span></span>

<span data-ttu-id="c8c82-173">Définition d’une page d’accueil par défaut fournit des visiteurs un point de départ logique lors de la visite de votre site.</span><span class="sxs-lookup"><span data-stu-id="c8c82-173">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="c8c82-174">Pour prendre en charge une page par défaut sans que l’utilisateur qualifié de l’URI, appelez le [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) méthode à partir de `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="c8c82-174">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="c8c82-175">`UseDefaultFiles`doit être appelé avant `UseStaticFiles` pour traiter le fichier par défaut.</span><span class="sxs-lookup"><span data-stu-id="c8c82-175">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="c8c82-176">`UseDefaultFiles`est un module de réécriture d’URL qui ne servent réellement le fichier.</span><span class="sxs-lookup"><span data-stu-id="c8c82-176">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="c8c82-177">Activer l’intergiciel (middleware) de fichiers statiques via `UseStaticFiles` pour traiter le fichier.</span><span class="sxs-lookup"><span data-stu-id="c8c82-177">Enable the static file middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="c8c82-178">Avec `UseDefaultFiles`, les requêtes pour rechercher un dossier :</span><span class="sxs-lookup"><span data-stu-id="c8c82-178">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="c8c82-179">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="c8c82-179">*default.htm*</span></span>
* <span data-ttu-id="c8c82-180">*default.html*</span><span class="sxs-lookup"><span data-stu-id="c8c82-180">*default.html*</span></span>
* <span data-ttu-id="c8c82-181">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="c8c82-181">*index.htm*</span></span>
* <span data-ttu-id="c8c82-182">*index.html*</span><span class="sxs-lookup"><span data-stu-id="c8c82-182">*index.html*</span></span>

<span data-ttu-id="c8c82-183">Le premier fichier trouvé dans la liste est traitée comme si la demande était l’URI complet.</span><span class="sxs-lookup"><span data-stu-id="c8c82-183">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="c8c82-184">L’URL de navigateur continue refléter l’URI demandé.</span><span class="sxs-lookup"><span data-stu-id="c8c82-184">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="c8c82-185">Le code suivant modifie le nom de fichier par défaut à *mydefault.html*:</span><span class="sxs-lookup"><span data-stu-id="c8c82-185">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="c8c82-186">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="c8c82-186">UseFileServer</span></span>

<span data-ttu-id="c8c82-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combine les fonctionnalités de `UseStaticFiles`, `UseDefaultFiles`, et `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="c8c82-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="c8c82-188">Le code suivant active les serveurs de fichiers statiques et le fichier par défaut.</span><span class="sxs-lookup"><span data-stu-id="c8c82-188">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="c8c82-189">Exploration des répertoires n’est pas activée.</span><span class="sxs-lookup"><span data-stu-id="c8c82-189">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="c8c82-190">Le code suivant s’appuie sur la surcharge sans paramètre en activant l’exploration des répertoires :</span><span class="sxs-lookup"><span data-stu-id="c8c82-190">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="c8c82-191">Tenez compte de la hiérarchie de répertoires suivants :</span><span class="sxs-lookup"><span data-stu-id="c8c82-191">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="c8c82-192">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="c8c82-192">**wwwroot**</span></span>
  * <span data-ttu-id="c8c82-193">**css**</span><span class="sxs-lookup"><span data-stu-id="c8c82-193">**css**</span></span>
  * <span data-ttu-id="c8c82-194">**images**</span><span class="sxs-lookup"><span data-stu-id="c8c82-194">**images**</span></span>
  * <span data-ttu-id="c8c82-195">**js**</span><span class="sxs-lookup"><span data-stu-id="c8c82-195">**js**</span></span>
* <span data-ttu-id="c8c82-196">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="c8c82-196">**MyStaticFiles**</span></span>
  * <span data-ttu-id="c8c82-197">**images**</span><span class="sxs-lookup"><span data-stu-id="c8c82-197">**images**</span></span>
      * <span data-ttu-id="c8c82-198">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="c8c82-198">*banner1.svg*</span></span>
  * <span data-ttu-id="c8c82-199">*default.html*</span><span class="sxs-lookup"><span data-stu-id="c8c82-199">*default.html*</span></span>

<span data-ttu-id="c8c82-200">Le code suivant permet de fichiers statiques, fichiers et par défaut, exploration des répertoires de `MyStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="c8c82-200">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="c8c82-201">`AddDirectoryBrowser`doit être appelé lorsque le `EnableDirectoryBrowsing` valeur de propriété est `true`:</span><span class="sxs-lookup"><span data-stu-id="c8c82-201">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="c8c82-202">URL à l’aide de la hiérarchie des fichiers et qui précède le code, résoudre comme suit :</span><span class="sxs-lookup"><span data-stu-id="c8c82-202">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="c8c82-203">URI</span><span class="sxs-lookup"><span data-stu-id="c8c82-203">URI</span></span>            |                             <span data-ttu-id="c8c82-204">Réponse</span><span class="sxs-lookup"><span data-stu-id="c8c82-204">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="c8c82-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="c8c82-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="c8c82-206">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="c8c82-206">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="c8c82-207">*http://\<server_address>/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="c8c82-207">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="c8c82-208">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="c8c82-208">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="c8c82-209">Si aucun fichier de la valeur par défaut nommée existe dans le *MyStaticFiles* active, *http://\<server_address > / StaticFiles* renvoie la liste avec des liens interactifs des répertoires :</span><span class="sxs-lookup"><span data-stu-id="c8c82-209">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![Liste de fichiers statiques](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="c8c82-211">`UseDefaultFiles`et `UseDirectoryBrowser` utiliser l’URL *http://\<server_address > / StaticFiles* sans la barre oblique pour déclencher un côté client rediriger vers *http://\<server_address > / StaticFiles /*.</span><span class="sxs-lookup"><span data-stu-id="c8c82-211">`UseDefaultFiles` and `UseDirectoryBrowser` use the URL *http://\<server_address>/StaticFiles* without the trailing slash to trigger a client-side redirect to *http://\<server_address>/StaticFiles/*.</span></span> <span data-ttu-id="c8c82-212">Notez l’ajout de la barre oblique.</span><span class="sxs-lookup"><span data-stu-id="c8c82-212">Notice the addition of the trailing slash.</span></span> <span data-ttu-id="c8c82-213">Les URL relatives dans les documents sont considérés comme non valides sans barre oblique de fin.</span><span class="sxs-lookup"><span data-stu-id="c8c82-213">Relative URLs within the documents are deemed invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="c8c82-214">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="c8c82-214">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="c8c82-215">Le [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) classe contient un `Mappings` propriété agissant comme un mappage des extensions de fichier pour les types de contenu MIME.</span><span class="sxs-lookup"><span data-stu-id="c8c82-215">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="c8c82-216">Dans l’exemple suivant, plusieurs extensions de fichiers sont inscrits pour les types MIME connus.</span><span class="sxs-lookup"><span data-stu-id="c8c82-216">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="c8c82-217">Le *.rtf* extension est remplacée, et *.mp4* est supprimé.</span><span class="sxs-lookup"><span data-stu-id="c8c82-217">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="c8c82-218">Consultez [les types de contenu MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="c8c82-218">See [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="c8c82-219">Types de contenu non standard</span><span class="sxs-lookup"><span data-stu-id="c8c82-219">Non-standard content types</span></span>

<span data-ttu-id="c8c82-220">L’intergiciel (middleware) de fichiers statiques comprend des types de contenu est connu presque 400.</span><span class="sxs-lookup"><span data-stu-id="c8c82-220">The static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="c8c82-221">Si l’utilisateur demande un fichier d’un type de fichier inconnu, l’intergiciel (middleware) fichier statique retourne une réponse HTTP 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="c8c82-221">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not Found) response.</span></span> <span data-ttu-id="c8c82-222">Si l’exploration des répertoires est activée, un lien vers le fichier s’affiche.</span><span class="sxs-lookup"><span data-stu-id="c8c82-222">If directory browsing is enabled, a link to the file is displayed.</span></span> <span data-ttu-id="c8c82-223">L’URI retourne une erreur HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="c8c82-223">The URI returns an HTTP 404 error.</span></span>

<span data-ttu-id="c8c82-224">Le code suivant permet de servir des types inconnus et rend le fichier inconnu en tant qu’image :</span><span class="sxs-lookup"><span data-stu-id="c8c82-224">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="c8c82-225">Avec le code précédent, une demande pour un fichier avec un type de contenu inconnu est retournée en tant qu’image.</span><span class="sxs-lookup"><span data-stu-id="c8c82-225">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="c8c82-226">L’activation de [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) est un risque de sécurité.</span><span class="sxs-lookup"><span data-stu-id="c8c82-226">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="c8c82-227">Il est désactivé par défaut, et son utilisation est déconseillée.</span><span class="sxs-lookup"><span data-stu-id="c8c82-227">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="c8c82-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) fournit une alternative plus sûre pour servir des fichiers avec les extensions non standard.</span><span class="sxs-lookup"><span data-stu-id="c8c82-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="c8c82-229">Éléments à prendre en considération</span><span class="sxs-lookup"><span data-stu-id="c8c82-229">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="c8c82-230">`UseDirectoryBrowser`et `UseStaticFiles` peut entraîner une fuite secrets.</span><span class="sxs-lookup"><span data-stu-id="c8c82-230">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="c8c82-231">Pour désactiver l’exploration de répertoire production est fortement recommandé.</span><span class="sxs-lookup"><span data-stu-id="c8c82-231">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="c8c82-232">Lisez attentivement les répertoires qui sont activés via `UseStaticFiles` ou `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="c8c82-232">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="c8c82-233">L’ensemble du répertoire et ses sous-répertoires accessibles publiquement.</span><span class="sxs-lookup"><span data-stu-id="c8c82-233">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="c8c82-234">Stockage de fichiers approprié pour le service au public dans un dossier dédié, tel que  *\<content_root > / wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="c8c82-234">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="c8c82-235">À partir de vues MVC, les Pages Razor (2.x uniquement), les fichiers de configuration, etc., séparez ces fichiers.</span><span class="sxs-lookup"><span data-stu-id="c8c82-235">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="c8c82-236">Les URL pour le contenu exposé avec `UseDirectoryBrowser` et `UseStaticFiles` sont soumis aux restrictions de caractères du système de fichiers sous-jacent et respect de la casse.</span><span class="sxs-lookup"><span data-stu-id="c8c82-236">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="c8c82-237">Par exemple, Windows respecte la casse&mdash;Mac et Linux ne sont pas.</span><span class="sxs-lookup"><span data-stu-id="c8c82-237">For example, Windows is case insensitive&mdash;Mac and Linux aren't.</span></span>

* <span data-ttu-id="c8c82-238">Les applications ASP.NET Core hébergées dans IIS utilisation le [ASP.NET Core Module (ANCM)](xref:fundamentals/servers/aspnet-core-module) pour transférer toutes les demandes à l’application, y compris les demandes de fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="c8c82-238">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module (ANCM)](xref:fundamentals/servers/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="c8c82-239">Le Gestionnaire de fichiers statiques d’IIS n’est pas utilisé.</span><span class="sxs-lookup"><span data-stu-id="c8c82-239">The IIS static file handler isn't used.</span></span> <span data-ttu-id="c8c82-240">Il a sans possibilité de traiter les demandes avant qu’ils sont gérés par le ANCM.</span><span class="sxs-lookup"><span data-stu-id="c8c82-240">It has no chance to handle requests before they're handled by the ANCM.</span></span>

* <span data-ttu-id="c8c82-241">Effectuez les étapes suivantes dans le Gestionnaire des services Internet pour supprimer le Gestionnaire de fichiers statiques d’IIS au niveau du serveur ou le site Web :</span><span class="sxs-lookup"><span data-stu-id="c8c82-241">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="c8c82-242">Accédez à la **Modules** fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="c8c82-242">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="c8c82-243">Sélectionnez **StaticFileModule** dans la liste.</span><span class="sxs-lookup"><span data-stu-id="c8c82-243">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="c8c82-244">Cliquez sur **supprimer** dans les **Actions** barre latérale.</span><span class="sxs-lookup"><span data-stu-id="c8c82-244">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="c8c82-245">Si le Gestionnaire de fichiers statiques d’IIS est activé **et** le ANCM est incorrectement configurée, les fichiers statiques sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="c8c82-245">If the IIS static file handler is enabled **and** the ANCM is configured incorrectly, static files are served.</span></span> <span data-ttu-id="c8c82-246">Cela se produit, par exemple, si le *web.config* fichier n’est pas déployé.</span><span class="sxs-lookup"><span data-stu-id="c8c82-246">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="c8c82-247">Placez les fichiers de code (y compris *.cs* et *.cshtml*) en dehors de l’application racine du projet web.</span><span class="sxs-lookup"><span data-stu-id="c8c82-247">Place code files (including *.cs* and *.cshtml*) outside of the app project's web root.</span></span> <span data-ttu-id="c8c82-248">Par conséquent, création d’une séparation logique entre contenu côté client et le code basé sur le serveur de l’application.</span><span class="sxs-lookup"><span data-stu-id="c8c82-248">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="c8c82-249">Cela empêche le code côté serveur à partir de la fuite.</span><span class="sxs-lookup"><span data-stu-id="c8c82-249">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c8c82-250">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="c8c82-250">Additional resources</span></span>

* [<span data-ttu-id="c8c82-251">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="c8c82-251">Middleware</span></span>](xref:fundamentals/middleware)

* [<span data-ttu-id="c8c82-252">Présentation d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8c82-252">Introduction to ASP.NET Core</span></span>](xref:index)