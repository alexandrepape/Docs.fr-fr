---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: "Tests unitaires d’Applications de SignalR | Documents Microsoft"
author: pfletcher
description: "Cet article décrit comment utiliser les fonctionnalités de test unitaire de SignalR 2.0."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: e55efd644dd4b6fb57061ffb89a5c041136c7b5e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-signalr-applications"></a><span data-ttu-id="d3b20-103">Unité de test des Applications SignalR</span><span class="sxs-lookup"><span data-stu-id="d3b20-103">Unit Testing SignalR Applications</span></span>
====================
<span data-ttu-id="d3b20-104">par [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d3b20-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="d3b20-105">Cet article décrit les fonctionnalités des tests unitaires de SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="d3b20-105">This article describes using the Unit Testing features of SignalR 2.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="d3b20-106">Versions du logiciel utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="d3b20-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="d3b20-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d3b20-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="d3b20-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d3b20-108">.NET 4.5</span></span>
> - <span data-ttu-id="d3b20-109">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="d3b20-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="d3b20-110">Questions et des commentaires</span><span class="sxs-lookup"><span data-stu-id="d3b20-110">Questions and comments</span></span>
> 
> <span data-ttu-id="d3b20-111">Veuillez laisser des commentaires sur la façon dont vous avez aimé ce didacticiel et nous pouvons améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="d3b20-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="d3b20-112">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les valider pour le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="d3b20-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="d3b20-113">Les applications SignalR de tests unitaires</span><span class="sxs-lookup"><span data-stu-id="d3b20-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="d3b20-114">Vous pouvez utiliser les fonctionnalités de test unitaire dans SignalR 2 pour créer des tests unitaires pour votre application SignalR.</span><span class="sxs-lookup"><span data-stu-id="d3b20-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="d3b20-115">SignalR 2 inclut les [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, ce qui peut être utilisé pour créer un objet factice pour simuler vos méthodes de concentrateur de test.</span><span class="sxs-lookup"><span data-stu-id="d3b20-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="d3b20-116">Dans cette section, vous allez ajouter des tests unitaires pour l’application créée dans le [didacticiel de mise en route](../getting-started/tutorial-getting-started-with-signalr.md) à l’aide de [XUnit.net](https://github.com/xunit/xunit) et [Moq](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="d3b20-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="d3b20-117">XUnit.net permet de contrôler le test ; Moq permet de créer un [mocker](http://en.wikipedia.org/wiki/Mock_object) objet pour le test.</span><span class="sxs-lookup"><span data-stu-id="d3b20-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="d3b20-118">Autres infrastructures factices peuvent être utilisés si vous le souhaitez ; [NSubstitute](http://nsubstitute.github.io/) est également un bon choix.</span><span class="sxs-lookup"><span data-stu-id="d3b20-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="d3b20-119">Ce didacticiel montre comment définir l’objet factice de deux manières : en premier lieu, à l’aide un `dynamic` objet (introduite dans .NET Framework 4) et, ensuite, à l’aide d’une interface.</span><span class="sxs-lookup"><span data-stu-id="d3b20-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="d3b20-120">Sommaire</span><span class="sxs-lookup"><span data-stu-id="d3b20-120">Contents</span></span>

<span data-ttu-id="d3b20-121">Ce didacticiel contient les sections suivantes.</span><span class="sxs-lookup"><span data-stu-id="d3b20-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="d3b20-122">Tests unitaires avec dynamique</span><span class="sxs-lookup"><span data-stu-id="d3b20-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="d3b20-123">Tests unitaires en type</span><span class="sxs-lookup"><span data-stu-id="d3b20-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="d3b20-124">Tests unitaires avec dynamique</span><span class="sxs-lookup"><span data-stu-id="d3b20-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="d3b20-125">Dans cette section, vous allez ajouter un test unitaire pour l’application créée dans le [didacticiel de mise en route](../getting-started/tutorial-getting-started-with-signalr.md) à l’aide d’un objet dynamique.</span><span class="sxs-lookup"><span data-stu-id="d3b20-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="d3b20-126">Installer le [extension de XUnit Runner](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) pour Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="d3b20-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="d3b20-127">Terminer la [didacticiel de mise en route](../getting-started/tutorial-getting-started-with-signalr.md), ou téléchargez l’application terminée à partir de [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="d3b20-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="d3b20-128">Si vous utilisez la version de téléchargement de l’application de mise en route, ouvrez **Package Manager Console** et cliquez sur **restaurer** pour ajouter le package SignalR au projet.</span><span class="sxs-lookup"><span data-stu-id="d3b20-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Restaurer les Packages](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="d3b20-130">Ajoutez un projet à la solution pour le test unitaire.</span><span class="sxs-lookup"><span data-stu-id="d3b20-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="d3b20-131">Avec le bouton droit de votre solution dans **l’Explorateur de solutions** et sélectionnez **ajouter**, **nouveau projet...** . Sous le **c#** nœud, sélectionnez le **Windows** nœud.</span><span class="sxs-lookup"><span data-stu-id="d3b20-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="d3b20-132">Sélectionnez **bibliothèque de classes**.</span><span class="sxs-lookup"><span data-stu-id="d3b20-132">Select **Class Library**.</span></span> <span data-ttu-id="d3b20-133">Nommez le nouveau projet **TestLibrary** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3b20-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Créer la bibliothèque de tests](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="d3b20-135">Ajoutez une référence dans le projet de bibliothèque de test au projet SignalRChat.</span><span class="sxs-lookup"><span data-stu-id="d3b20-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="d3b20-136">Avec le bouton droit le **TestLibrary** de projet et sélectionnez **ajouter**, **référence...** . Sélectionnez le **projets** nœud sous la **Solution** nœud et cocher **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="d3b20-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="d3b20-137">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3b20-137">Click **OK**.</span></span>

    ![Ajouter une référence de projet](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="d3b20-139">Ajouter les packages SignalR, Moq et XUnit le **TestLibrary** projet.</span><span class="sxs-lookup"><span data-stu-id="d3b20-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="d3b20-140">Dans le **Package Manager Console**, définissez le **projet par défaut** menu déroulant pour **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="d3b20-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="d3b20-141">Exécutez les commandes suivantes dans la fenêtre de console :</span><span class="sxs-lookup"><span data-stu-id="d3b20-141">Run the following commands in the console window:</span></span>

    - `Install-Package Microsoft.AspNet.SignalR`
    - `Install-Package Moq`
    - `Install-Package XUnit`

    ![Installer des Packages](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="d3b20-143">Créer le fichier de test.</span><span class="sxs-lookup"><span data-stu-id="d3b20-143">Create the test file.</span></span> <span data-ttu-id="d3b20-144">Cliquez sur le **TestLibrary** projet puis cliquez sur **ajouter...** , **Classe**.</span><span class="sxs-lookup"><span data-stu-id="d3b20-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="d3b20-145">Nommez la nouvelle classe **Tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="d3b20-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="d3b20-146">Remplacez le contenu de Tests.cs par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="d3b20-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="d3b20-147">Dans le code ci-dessus, un client de test est créé à l’aide de la `Mock` à partir de l’objet le [Moq](https://github.com/Moq/moq4) bibliothèque, de type [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1, affecter `dynamic` pour le type paramètre.) Le `IHubCallerConnectionContext` interface est l’objet proxy avec lequel vous appelez des méthodes sur le client.</span><span class="sxs-lookup"><span data-stu-id="d3b20-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="d3b20-148">Le `broadcastMessage` fonction est alors définie pour le client fictif afin qu’il peut être appelé le `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="d3b20-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="d3b20-149">Le moteur de test appelle ensuite la `Send` méthode de la `ChatHub` (classe), qui à son tour appelle la factices `broadcastMessage` (fonction).</span><span class="sxs-lookup"><span data-stu-id="d3b20-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="d3b20-150">Générez la solution en appuyant sur **F6**.</span><span class="sxs-lookup"><span data-stu-id="d3b20-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="d3b20-151">Exécutez le test unitaire.</span><span class="sxs-lookup"><span data-stu-id="d3b20-151">Run the unit test.</span></span> <span data-ttu-id="d3b20-152">Dans Visual Studio, sélectionnez **Test**, **Windows**, **l’Explorateur de tests**.</span><span class="sxs-lookup"><span data-stu-id="d3b20-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="d3b20-153">Dans la fenêtre de l’Explorateur de tests, cliquez sur **HubsAreMockableViaDynamic** et sélectionnez **exécuter les Tests sélectionnés**.</span><span class="sxs-lookup"><span data-stu-id="d3b20-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Explorateur de tests](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="d3b20-155">Vérifiez que le test a réussi en vérifiant le volet inférieur de la fenêtre de l’Explorateur de tests.</span><span class="sxs-lookup"><span data-stu-id="d3b20-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="d3b20-156">La fenêtre indique que le test a réussi.</span><span class="sxs-lookup"><span data-stu-id="d3b20-156">The window will show that the test passed.</span></span>

    ![Test réussi](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="d3b20-158">Tests unitaires en type</span><span class="sxs-lookup"><span data-stu-id="d3b20-158">Unit testing by type</span></span>

<span data-ttu-id="d3b20-159">Dans cette section, vous allez ajouter un test pour l’application créée dans le [didacticiel de mise en route](../getting-started/tutorial-getting-started-with-signalr.md) à l’aide d’une interface qui contient la méthode à tester.</span><span class="sxs-lookup"><span data-stu-id="d3b20-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="d3b20-160">Effectuez les étapes 1 à 7 dans le [tests unitaires avec dynamique](#dynamic) didacticiel ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="d3b20-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="d3b20-161">Remplacez le contenu de Tests.cs par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="d3b20-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="d3b20-162">Dans le code ci-dessus, une interface est créée en définissant la signature de la `broadcastMessage` méthode pour laquelle le moteur de test créera un client fictif.</span><span class="sxs-lookup"><span data-stu-id="d3b20-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="d3b20-163">Un client fictif est ensuite créé à l’aide de la `Mock` objet de type [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1, affecter `dynamic` pour le paramètre de type.) Le `IHubCallerConnectionContext` interface est l’objet proxy avec lequel vous appelez des méthodes sur le client.</span><span class="sxs-lookup"><span data-stu-id="d3b20-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="d3b20-164">Le test crée ensuite une instance de `ChatHub`, puis crée une version fictive de la `broadcastMessage` (méthode), qui à son tour, est appelé en appelant le `Send` méthode du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="d3b20-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="d3b20-165">Générez la solution en appuyant sur **F6**.</span><span class="sxs-lookup"><span data-stu-id="d3b20-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="d3b20-166">Exécutez le test unitaire.</span><span class="sxs-lookup"><span data-stu-id="d3b20-166">Run the unit test.</span></span> <span data-ttu-id="d3b20-167">Dans Visual Studio, sélectionnez **Test**, **Windows**, **l’Explorateur de tests**.</span><span class="sxs-lookup"><span data-stu-id="d3b20-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="d3b20-168">Dans la fenêtre de l’Explorateur de tests, cliquez sur **HubsAreMockableViaDynamic** et sélectionnez **exécuter les Tests sélectionnés**.</span><span class="sxs-lookup"><span data-stu-id="d3b20-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Explorateur de tests](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="d3b20-170">Vérifiez que le test a réussi en vérifiant le volet inférieur de la fenêtre de l’Explorateur de tests.</span><span class="sxs-lookup"><span data-stu-id="d3b20-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="d3b20-171">La fenêtre indique que le test a réussi.</span><span class="sxs-lookup"><span data-stu-id="d3b20-171">The window will show that the test passed.</span></span>

    ![Test réussi](unit-testing-signalr-applications/_static/image8.png)