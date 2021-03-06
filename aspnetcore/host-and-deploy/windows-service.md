---
title: Héberger ASP.NET Core dans un service Windows
author: guardrex
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 4aded0b87ca14a5c09844cc378efb1ac0c12a289
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342154"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Héberger ASP.NET Core dans un service Windows

Par [Luke Latham](https://github.com/guardrex) et [Tom Dykstra](https://github.com/tdykstra)

Une application ASP.NET Core peut être hébergée sur Windows sans utiliser IIS en tant que [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Quand elle est hébergée en tant que service Windows, l’application peut automatiquement démarrer après les redémarrages et les plantages sans nécessiter une intervention humaine.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="get-started"></a>Prise en main

Les modifications minimales suivantes sont requises pour configurer un projet ASP.NET Core existant afin qu’il s’exécute dans un service :

1. Dans le fichier projet :

   1. Vérifiez la présence de l’identificateur de runtime ou ajoutez-le à la section **\<PropertyGroup>** qui contient la version cible du .Net Framework :

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

   1. Ajoutez une référence de package pour [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

1. Dans `Program.Main`, effectuez les changements suivants :

   * Appelez [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) au lieu de `host.Run`.

   * Appelez [UseContentRoot](xref:fundamentals/host/web-host#content-root) et utilisez un chemin vers l’emplacement publié de l’application au lieu de `Directory.GetCurrentDirectory()`.

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,12)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. Publiez l’application. Utilisez [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) ou un [profil de publication Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles). Si vous utilisez Visual Studio, sélectionnez **FolderProfile**.

   Pour publier l’exemple d’application à partir de la ligne de commande, exécutez la commande suivante dans une fenêtre de console à partir du dossier de projet :

   ```console
   dotnet publish --configuration Release
   ```

1. Utilisez l’outil en ligne de commande [sc.exe](https://technet.microsoft.com/library/bb490995) pour créer le service. La valeur `binPath` est le chemin du fichier exécutable de l’application, qui inclut le nom du fichier exécutable. **L’espace entre le signe égal et le guillemet au début du chemin est obligatoire.**

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   Pour un service publié dans le dossier du projet, utilisez le chemin du dossier *publish* pour créer le service. Dans l’exemple suivant :

   * Le projet se trouve dans le dossier `c:\my_services\AspNetCoreService`.
   * Le projet est publié dans la configuration `Release`.
   * Le moniker de framework cible (TFM) est `netcoreapp2.1`.
   * L’identificateur du runtime (RID) est `win7-x64`.
   * L’exécutable de l’application s’appelle *AspNetCoreService.exe*.
   * Le service s’appelle **MyService**.

   Exemple :

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```
   
   > [!IMPORTANT]
   > Vérifiez que l’espace est présent entre l’argument `binPath=` et sa valeur.
   
   Pour publier et démarrer le service à partir d’un autre dossier :
   
      1. Utilisez l’option [--output &lt;RÉPERTOIRE_SORTIE&gt;](/dotnet/core/tools/dotnet-publish#options) dans la commande `dotnet publish`. Si vous utilisez Visual Studio, configurez l’**Emplacement cible** dans la page de propriétés de publication **FolderProfile** avant de sélectionner le bouton **Publier**.
   1. Créez le service avec la commande `sc.exe` en utilisant le chemin du dossier de sortie. Ajoutez le nom de l’exécutable du service dans le chemin fourni dans `binPath`.

1. Démarrez le service avec la commande `sc start <SERVICE_NAME>`.

   Pour démarrer l’exemple de service d’application, utilisez la commande suivante :

   ```console
   sc start MyService
   ```

   La commande prend quelques secondes pour démarrer le service.

1. La commande `sc query <SERVICE_NAME>` permet de déterminer l’état du service :

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   Utilisez la commande suivante pour vérifier l’état de l’exemple de service d’application :

   ```console
   sc query MyService
   ```

1. Quand le service est une application web et que son état est `RUNNING`, accédez au chemin de l'application (par défaut, `http://localhost:5000`, qui redirige vers `https://localhost:5001` si le [middleware (intergiciel) de redirection HTTPS](xref:security/enforcing-ssl) est utilisé).

   Pour l’exemple de service d’application, accédez au chemin de l'application sur `http://localhost:5000`.

1. Arrêtez le service avec la commande `sc stop <SERVICE_NAME>`.

   La commande suivante arrête l’exemple de service d’application :

   ```console
   sc stop MyService
   ```

1. Après un court délai pour arrêter un service, désinstallez le service avec la commande `sc delete <SERVICE_NAME>`.

   Vérifiez l’état de l’exemple de service d’application :

   ```console
   sc query MyService
   ```

   Quand l’exemple de service d’application est dans l’état `STOPPED`, utilisez la commande suivante pour le désinstaller :

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Fournir un moyen d’exécution en dehors d’un service

Comme il est plus facile d’effectuer des tests et des débogages pendant une exécution en dehors d’un service, il est habituel d’ajouter du code qui appelle `RunAsService` uniquement sous certaines conditions. Par exemple, l’application peut s’exécuter en tant qu’application console avec un argument de ligne de commande `--console` ou si le débogueur est attaché :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

Étant donné que la configuration d’ASP.NET Core nécessite des paires nom/valeur pour les arguments de ligne de commande, le commutateur `--console` est supprimé avant que les arguments ne soient passés à [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

> [!NOTE]
> `isService` n’est pas passé de `Main` vers `CreateWebHostBuilder`, car la signature de `CreateWebHostBuilder` doit être `CreateWebHostBuilder(string[])` afin que les [tests d’intégration](xref:test/integration-tests) fonctionnent correctement.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a>Gérer les événements d’arrêt et de démarrage

Pour gérer les événements [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) et [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), apportez les modifications supplémentaires suivantes :

1. Créez une classe qui dérive de [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) :

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. Créez une méthode d’extension pour [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) qui transmet le `WebHostService` personnalisé à [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) :

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. Dans `Program.Main`, appelez la nouvelle méthode d’extension, `RunAsCustomService`, au lieu de [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) :

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=14)]

   > [!NOTE]
   > `isService` n’est pas passé de `Main` vers `CreateWebHostBuilder`, car la signature de `CreateWebHostBuilder` doit être `CreateWebHostBuilder(string[])` afin que les [tests d’intégration](xref:test/integration-tests) fonctionnent correctement.

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

Si le code `WebHostService` personnalisé nécessite un service à partir d’une injection de dépendances (par exemple, un enregistreur d’événements), obtenez-le à partir de la propriété [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) :

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scénarios avec un serveur proxy et un équilibreur de charge

Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire. Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)
* <xref:fundamentals/host/web-host>
