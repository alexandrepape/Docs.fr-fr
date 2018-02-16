---
title: "Ajouter des fonctionnalités d’application à l’aide d’une configuration spécifique à la plateforme dans ASP.NET Core"
author: guardrex
description: "Découvrez comment ajouter des fonctionnalités à une application ASP.NET Core à partir d’un assembly externe à l’aide d’une implémentation IHostingStartup."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/platform-specific-configuration
ms.openlocfilehash: 2663cd1e05be9e8695966df959082e6e574d0b4a
ms.sourcegitcommit: 809ee4baf8bf7b4cae9e366ecae29de1037d2bbb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/15/2018
---
# <a name="add-app-features-using-a-platform-specific-configuration-in-aspnet-core"></a><span data-ttu-id="76e57-103">Ajouter des fonctionnalités d’application à l’aide d’une configuration spécifique à la plateforme dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="76e57-103">Add app features using a platform-specific configuration in ASP.NET Core</span></span>

<span data-ttu-id="76e57-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="76e57-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="76e57-105">Un [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implémentation permet d’ajouter des fonctionnalités à une application au démarrage à partir d’un assembly externe en dehors de l’application `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="76e57-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding features to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="76e57-106">Par exemple, une bibliothèque d’outils externes permettre utiliser un `IHostingStartup` l’implémentation pour fournir des services à une application ou fournisseurs de configuration supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="76e57-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="76e57-107">`IHostingStartup` *est disponible dans ASP.NET Core 2.0 et versions ultérieures.*</span><span class="sxs-lookup"><span data-stu-id="76e57-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="76e57-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="76e57-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="76e57-109">Détecter les assemblys de démarrage hébergement chargés</span><span class="sxs-lookup"><span data-stu-id="76e57-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="76e57-110">Pour découvrir les assemblys de démarrage d’hébergement chargé par l’application ou par des bibliothèques, activez la journalisation, puis vérifiez les journaux des applications.</span><span class="sxs-lookup"><span data-stu-id="76e57-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="76e57-111">Les erreurs qui se produisent lors du chargement d’assemblys sont enregistrées.</span><span class="sxs-lookup"><span data-stu-id="76e57-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="76e57-112">Assemblys de démarrage hébergement chargés sont enregistrées au niveau du débogage, et toutes les erreurs sont journalisées.</span><span class="sxs-lookup"><span data-stu-id="76e57-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="76e57-113">Les lectures d’application exemple le [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) dans un `string` de tableau et affiche le résultat dans la page d’Index de l’application :</span><span class="sxs-lookup"><span data-stu-id="76e57-113">The sample app reads the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[Main](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="76e57-114">Désactiver le chargement automatique des assemblys de démarrage d’hébergement</span><span class="sxs-lookup"><span data-stu-id="76e57-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="76e57-115">Il existe deux méthodes pour désactiver le chargement automatique des assemblys de démarrage d’hébergement :</span><span class="sxs-lookup"><span data-stu-id="76e57-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="76e57-116">Définir le [empêcher le démarrage d’hébergement](xref:fundamentals/hosting#prevent-hosting-startup) héberger le paramètre de configuration.</span><span class="sxs-lookup"><span data-stu-id="76e57-116">Set the [Prevent Hosting Startup](xref:fundamentals/hosting#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="76e57-117">Définir le `ASPNETCORE_preventHostingStartup` variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="76e57-117">Set the `ASPNETCORE_preventHostingStartup` environment variable.</span></span>

<span data-ttu-id="76e57-118">Lorsque le paramètre d’hôte ou de la variable d’environnement est définie sur `true` ou `1`, l’hébergement des assemblys de démarrage ne sont pas chargés automatiquement.</span><span class="sxs-lookup"><span data-stu-id="76e57-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="76e57-119">Si les deux sont définis, le paramètre de l’hôte détermine le comportement.</span><span class="sxs-lookup"><span data-stu-id="76e57-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="76e57-120">Désactivation des assemblys de démarrage hébergement à l’aide de la variable d’environnement ou le paramètre hôte les désactive globalement et peut désactiver plusieurs fonctionnalités d’une application.</span><span class="sxs-lookup"><span data-stu-id="76e57-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several features of an app.</span></span> <span data-ttu-id="76e57-121">Il n’est pas encore possible de désactiver de manière sélective un hébergement assembly de démarrage ajouté par une bibliothèque, à moins que la bibliothèque propose sa propre option de configuration.</span><span class="sxs-lookup"><span data-stu-id="76e57-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="76e57-122">Une version ultérieure propose la possibilité de désactiver de manière sélective les assemblys de démarrage hébergement (consultez [GitHub émettre aspnet/hébergement # entré 1243](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="76e57-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup-features"></a><span data-ttu-id="76e57-123">Implémenter des fonctionnalités de IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="76e57-123">Implement IHostingStartup features</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="76e57-124">Créer l’assembly</span><span class="sxs-lookup"><span data-stu-id="76e57-124">Create the assembly</span></span>

<span data-ttu-id="76e57-125">Un `IHostingStartup` déployée en tant qu’assembly basé sur une application console sans point d’entrée.</span><span class="sxs-lookup"><span data-stu-id="76e57-125">An `IHostingStartup` feature is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="76e57-126">Les références d’assembly le [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package :</span><span class="sxs-lookup"><span data-stu-id="76e57-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[Main](platform-specific-configuration/snapshot_sample/StartupFeature.csproj)]

<span data-ttu-id="76e57-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribut identifie une classe en tant qu’implémentation de `IHostingStartup` pour le chargement et l’exécution lors de la génération du [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="76e57-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="76e57-128">Dans l’exemple suivant, l’espace de noms est `StartupFeature`, et la classe est `StartupFeatureHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="76e57-128">In the following example, the namespace is `StartupFeature`, and the class is `StartupFeatureHostingStartup`:</span></span>

[!code-csharp[Main](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet1)]

<span data-ttu-id="76e57-129">Une classe implémente `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="76e57-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="76e57-130">La classe [configurer](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) utilise un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) pour ajouter des fonctionnalités à une application :</span><span class="sxs-lookup"><span data-stu-id="76e57-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add features to an app:</span></span>

[!code-csharp[Main](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="76e57-131">Lors de la génération un `IHostingStartup` de projet, le fichier de dépendances (*\*. deps.json*) définit les `runtime` l’emplacement de l’assembly à la *bin* dossier :</span><span class="sxs-lookup"><span data-stu-id="76e57-131">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[Main](platform-specific-configuration/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="76e57-132">Seule une partie du fichier est affichée.</span><span class="sxs-lookup"><span data-stu-id="76e57-132">Only part of the file is shown.</span></span> <span data-ttu-id="76e57-133">Le nom de l’assembly dans l’exemple est `StartupFeature`.</span><span class="sxs-lookup"><span data-stu-id="76e57-133">The assembly name in the example is `StartupFeature`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="76e57-134">Mettre à jour le fichier de dépendances</span><span class="sxs-lookup"><span data-stu-id="76e57-134">Update the dependencies file</span></span>

<span data-ttu-id="76e57-135">L’emplacement d’exécution est spécifié dans le  *\*. deps.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="76e57-135">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="76e57-136">Pour active la fonctionnalité, le `runtime` élément doit spécifier l’emplacement de l’assembly du runtime de la fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="76e57-136">To active the feature, the `runtime` element must specify the location of the feature's runtime assembly.</span></span> <span data-ttu-id="76e57-137">Préfixe la `runtime` emplacement avec `lib/netcoreapp2.0/`:</span><span class="sxs-lookup"><span data-stu-id="76e57-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[Main](platform-specific-configuration/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="76e57-138">Dans l’exemple d’application, la modification de la  *\*. deps.json* fichier est effectué par un [PowerShell](/powershell/scripting/powershell-scripting) script.</span><span class="sxs-lookup"><span data-stu-id="76e57-138">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="76e57-139">Le script PowerShell est automatiquement déclenché par une cible de génération dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="76e57-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="feature-activation"></a><span data-ttu-id="76e57-140">Activation de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="76e57-140">Feature activation</span></span>

<span data-ttu-id="76e57-141">**Placez le fichier d’assembly**</span><span class="sxs-lookup"><span data-stu-id="76e57-141">**Place the assembly file**</span></span>

<span data-ttu-id="76e57-142">Le `IHostingStartup` fichier d’assembly de l’implémentation doit être *bin*-déployés dans l’application ou placé dans le [magasin runtime](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="76e57-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="76e57-143">Pour une utilisation par l’utilisateur, placez l’assembly dans le magasin de runtime du profil utilisateur à :</span><span class="sxs-lookup"><span data-stu-id="76e57-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="76e57-144">Dans le monde entier, placez l’assembly dans le magasin d’exécution de l’installation de .NET Core :</span><span class="sxs-lookup"><span data-stu-id="76e57-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="76e57-145">Lors du déploiement de l’assembly dans le magasin de l’exécution, le fichier de symboles peut-être être déployé également, mais n’est pas nécessaire pour la fonctionnalité soit opérationnelle.</span><span class="sxs-lookup"><span data-stu-id="76e57-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the feature to work.</span></span>

<span data-ttu-id="76e57-146">**Placez le fichier de dépendances**</span><span class="sxs-lookup"><span data-stu-id="76e57-146">**Place the dependencies file**</span></span>

<span data-ttu-id="76e57-147">La mise en oeuvre  *\*. deps.json* fichier doit être dans un emplacement accessible.</span><span class="sxs-lookup"><span data-stu-id="76e57-147">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="76e57-148">Pour une utilisation par l’utilisateur, placez le fichier dans le `additonalDeps` dossier du profil utilisateur `.dotnet` paramètres :</span><span class="sxs-lookup"><span data-stu-id="76e57-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="76e57-149">Dans le monde entier, placez le fichier dans le `additonalDeps` dossier de l’installation de .NET Core :</span><span class="sxs-lookup"><span data-stu-id="76e57-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="76e57-150">Notez le numéro de version, `2.0.0`, reflète la version du runtime partagé utilisé par l’application cible.</span><span class="sxs-lookup"><span data-stu-id="76e57-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="76e57-151">Le runtime partagé est indiqué dans le  *\*. runtimeconfig.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="76e57-151">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="76e57-152">Dans l’exemple d’application, le runtime partagé est spécifié dans le *HostingStartupSample.runtimeconfig.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="76e57-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="76e57-153">**Jeu de variables d’environnement**</span><span class="sxs-lookup"><span data-stu-id="76e57-153">**Set environment variables**</span></span>

<span data-ttu-id="76e57-154">Définissez les variables d’environnement suivantes dans le contexte de l’application qui utilise la fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="76e57-154">Set the following environment variables in the context of the app that uses the feature.</span></span>

<span data-ttu-id="76e57-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="76e57-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="76e57-156">Seuls les assemblys démarrage hébergement sont analysés pour la `HostingStartupAttribute`.</span><span class="sxs-lookup"><span data-stu-id="76e57-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="76e57-157">Le nom de l’assembly de l’implémentation est fourni dans cette variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="76e57-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="76e57-158">L’exemple d’application définit cette valeur sur `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="76e57-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="76e57-159">La valeur peut également être définie à l’aide de la [hébergeant les assemblys de démarrage](xref:fundamentals/hosting#hosting-startup-assemblies) héberger le paramètre de configuration.</span><span class="sxs-lookup"><span data-stu-id="76e57-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/hosting#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="76e57-160">DOTNET\_SUPPLÉMENTAIRES\_DÉP.</span><span class="sxs-lookup"><span data-stu-id="76e57-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="76e57-161">L’emplacement de la mise en oeuvre  *\*. deps.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="76e57-161">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="76e57-162">Si le fichier est placé dans le profil d’utilisateur *.dotnet* dossier pour une utilisation de l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="76e57-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="76e57-163">Si le fichier est placé dans l’installation de .NET Core dans le monde entier, fournissez le chemin d’accès complet au fichier :</span><span class="sxs-lookup"><span data-stu-id="76e57-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="76e57-164">L’exemple d’application définit cette valeur sur :</span><span class="sxs-lookup"><span data-stu-id="76e57-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="76e57-165">Pour obtenir des exemples montrant comment définir des variables d’environnement pour les différents systèmes d’exploitation, consultez [fonctionne avec plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="76e57-165">For examples of how to set environment variables for various operating systems, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="76e57-166">Exemple d’application</span><span class="sxs-lookup"><span data-stu-id="76e57-166">Sample app</span></span>

<span data-ttu-id="76e57-167">Le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)) utilise `IHostingStartup` pour créer un outil de diagnostic.</span><span class="sxs-lookup"><span data-stu-id="76e57-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="76e57-168">L’outil ajoute deux middlewares à l’application au démarrage, qui fournissent des informations de diagnostic :</span><span class="sxs-lookup"><span data-stu-id="76e57-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="76e57-169">Services enregistrés</span><span class="sxs-lookup"><span data-stu-id="76e57-169">Registered services</span></span>
* <span data-ttu-id="76e57-170">Adresse : schéma, hôte, chemin d’accès de base, chemin d’accès, chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="76e57-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="76e57-171">Connexion : adresse IP distante, port distant, adresse IP locale, port local, le certificat client</span><span class="sxs-lookup"><span data-stu-id="76e57-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="76e57-172">En-têtes de demande</span><span class="sxs-lookup"><span data-stu-id="76e57-172">Request headers</span></span>
* <span data-ttu-id="76e57-173">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="76e57-173">Environment variables</span></span>

<span data-ttu-id="76e57-174">Pour exécuter l’exemple :</span><span class="sxs-lookup"><span data-stu-id="76e57-174">To run the sample:</span></span>

1. <span data-ttu-id="76e57-175">Le projet de démarrage du Diagnostic utilise [PowerShell](/powershell/scripting/powershell-scripting) pour modifier son *StartupDiagnostics.deps.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="76e57-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="76e57-176">PowerShell est installé par défaut sur le système d’exploitation Windows depuis Windows 7 SP1 et Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="76e57-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="76e57-177">Pour obtenir de PowerShell sur d’autres plateformes, consultez [installation de Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="76e57-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="76e57-178">Générez le projet de démarrage du Diagnostic.</span><span class="sxs-lookup"><span data-stu-id="76e57-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="76e57-179">Une cible de génération dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="76e57-179">A build target in the project file:</span></span>
   * <span data-ttu-id="76e57-180">Déplace l’assembly et des symboles des fichiers au magasin de runtime du profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="76e57-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="76e57-181">Déclenche le script PowerShell pour modifier la *StartupDiagnostics.deps.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="76e57-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="76e57-182">Déplace le *StartupDiagnostics.deps.json* fichier pour le profil d’utilisateur `additionalDeps` dossier.</span><span class="sxs-lookup"><span data-stu-id="76e57-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="76e57-183">Définissez les variables d’environnement :</span><span class="sxs-lookup"><span data-stu-id="76e57-183">Set the environment variables:</span></span>
    * <span data-ttu-id="76e57-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="76e57-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="76e57-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="76e57-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="76e57-186">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="76e57-186">Run the sample app.</span></span>
5. <span data-ttu-id="76e57-187">Demander le `/services` point de terminaison pour voir l’application inscrit des services.</span><span class="sxs-lookup"><span data-stu-id="76e57-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="76e57-188">Demander le `/diag` point de terminaison pour afficher les informations de diagnostic.</span><span class="sxs-lookup"><span data-stu-id="76e57-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>