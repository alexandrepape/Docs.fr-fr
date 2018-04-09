---
title: Résoudre les problèmes pour ASP.NET Core
author: Rick-Anderson
description: Comprendre et résoudre les avertissements et erreurs avec les projets ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: 98077081409949db14b19c7934bc162990ffc302
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="fdb48-103">Résoudre les problèmes des projets ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fdb48-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="fdb48-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fdb48-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fdb48-105">Les liens suivants fournissent des conseils de dépannage :</span><span class="sxs-lookup"><span data-stu-id="fdb48-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="fdb48-106">Résoudre les problèmes liés à ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="fdb48-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="fdb48-107">Résoudre les problèmes liés à ASP.NET Core sur IIS</span><span class="sxs-lookup"><span data-stu-id="fdb48-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="fdb48-108">Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fdb48-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="fdb48-109">Avertissements du Kit de développement .NET core</span><span class="sxs-lookup"><span data-stu-id="fdb48-109">.NET Core SDK warnings</span></span>

### <a name="both-the-32-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="fdb48-110">Les deux les 32 et 64 bits versions du Kit de développement .NET Core sont installées</span><span class="sxs-lookup"><span data-stu-id="fdb48-110">Both the 32 and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="fdb48-111">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant apparaît en haut :</span><span class="sxs-lookup"><span data-stu-id="fdb48-111">In the **New Project** dialog for ASP.NET Core, you may see the following warning appear at the top:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="fdb48-113">Cet avertissement s’affiche lorsque les (x86) 32 bits et les versions 64 bits (x 64) de la [le Kit de développement .NET Core](https://www.microsoft.com/net/download/all) sont installés.</span><span class="sxs-lookup"><span data-stu-id="fdb48-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="fdb48-114">Les deux versions peuvent être installées causes courantes :</span><span class="sxs-lookup"><span data-stu-id="fdb48-114">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="fdb48-115">Vous initialement téléchargé le programme d’installation du Kit de développement .NET Core à l’aide d’un ordinateur 32 bits, mais puis copié sur et installez sur un ordinateur 64 bits.</span><span class="sxs-lookup"><span data-stu-id="fdb48-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="fdb48-116">Le Kit de développement 32 bits .NET Core a été installée par une autre application.</span><span class="sxs-lookup"><span data-stu-id="fdb48-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="fdb48-117">Version incorrecte a été téléchargée et installée.</span><span class="sxs-lookup"><span data-stu-id="fdb48-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="fdb48-118">Désinstallez le Kit de développement 32 bits .NET Core pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="fdb48-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="fdb48-119">Désinstallation à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="fdb48-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="fdb48-120">Si vous comprenez la raison pour laquelle l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="fdb48-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="fdb48-121">Le Kit de développement .NET Core est installé dans plusieurs emplacements</span><span class="sxs-lookup"><span data-stu-id="fdb48-121">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="fdb48-122">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant apparaît en haut :</span><span class="sxs-lookup"><span data-stu-id="fdb48-122">In the **New Project** dialog for ASP.NET Core you may see the following warning appear at the top:</span></span> 

 <span data-ttu-id="fdb48-123">Le Kit de développement .NET Core est installé dans plusieurs emplacements.</span><span class="sxs-lookup"><span data-stu-id="fdb48-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="fdb48-124">Seuls les modèles à partir de la SDK(s) installé sur « C:\Program Files\dotnet\sdk\' s’affiche.</span><span class="sxs-lookup"><span data-stu-id="fdb48-124">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="fdb48-126">Vous voyez ce message, car vous avez au moins une installation du Kit de développement .NET Core dans un répertoire en dehors de * C:\Program Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="fdb48-126">You are seeing this message because you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="fdb48-127">Cela se produit généralement lorsque le Kit de développement .NET Core a été déployée sur un ordinateur à l’aide de copier/coller au lieu du programme d’installation MSI.</span><span class="sxs-lookup"><span data-stu-id="fdb48-127">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="fdb48-128">Désinstallez le Kit de développement 32 bits .NET Core pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="fdb48-128">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="fdb48-129">Désinstallation à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="fdb48-129">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="fdb48-130">Si vous comprenez la raison pour laquelle l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="fdb48-130">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>