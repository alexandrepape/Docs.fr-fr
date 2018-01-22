---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: "L’exécution des Animations à l’aide du Code côté Client (c#) | Documents Microsoft"
author: wenz
description: "Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’exécution de l’animation en cours..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 6b1911686a79aa692ef193430cd0746a2511105a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="executing-animations-using-client-side-code-c"></a><span data-ttu-id="a11cf-104">Animations en cours d’exécution à l’aide de Code côté Client (c#)</span><span class="sxs-lookup"><span data-stu-id="a11cf-104">Executing Animations Using Client-Side Code (C#)</span></span>
====================
<span data-ttu-id="a11cf-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a11cf-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a11cf-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a11cf-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span></span>

> <span data-ttu-id="a11cf-107">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="a11cf-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a11cf-108">L’exécution de l’animation peut également être déclenchée à l’aide du code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="a11cf-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="a11cf-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="a11cf-109">Overview</span></span>

<span data-ttu-id="a11cf-110">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="a11cf-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a11cf-111">L’exécution de l’animation peut également être déclenchée à l’aide du code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="a11cf-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="a11cf-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="a11cf-112">Steps</span></span>

<span data-ttu-id="a11cf-113">Tout d’abord, incluez le `ScriptManager` dans la page ; ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="a11cf-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="a11cf-114">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="a11cf-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="a11cf-115">Dans la classe CSS associée pour le panneau de configuration, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau de configuration :</span><span class="sxs-lookup"><span data-stu-id="a11cf-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="a11cf-116">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant une `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="a11cf-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="a11cf-117">Dans le `<Animations>` nœud, utilisez `<OnClick>` pour exécuter les animations une fois l’utilisateur clique sur le panneau de configuration.</span><span class="sxs-lookup"><span data-stu-id="a11cf-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="a11cf-118">Ajoutez deux animations doit être exécuté en :</span><span class="sxs-lookup"><span data-stu-id="a11cf-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

<span data-ttu-id="a11cf-119">Pour des raisons de démonstration, cette animation (et toute autre animation créé à l’aide de la boîte à outils de contrôle) sont exécutées à l’aide de code JavaScript, une fois que la page s’exécute.</span><span class="sxs-lookup"><span data-stu-id="a11cf-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="a11cf-120">Tout d’abord nous avons besoin d’accéder à la `AnimationExtender` contrôle.</span><span class="sxs-lookup"><span data-stu-id="a11cf-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="a11cf-121">La bibliothèque ASP.NET AJAX fournit le `$find()` fonction pour cette tâche :</span><span class="sxs-lookup"><span data-stu-id="a11cf-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

<span data-ttu-id="a11cf-122">Le `AnimationExtender` contrôle expose une API riche, y compris les méthodes avec des noms identiques aux gestionnaires d’événements utilisés dans le balisage XML : `OnClick()`, `OnLoad()`, et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="a11cf-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="a11cf-123">Par exemple, un appel de la `OnClick()` méthode s’exécute l’animation dans le `<OnClick>` élément de la `AnimationExtender` contrôle :</span><span class="sxs-lookup"><span data-stu-id="a11cf-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

<span data-ttu-id="a11cf-124">Voici le code JavaScript côté client complet qui émule le, cliquez sur le panneau de configuration une fois que la page a été entièrement chargée Notez que le `pageLoad()` nom de la fonction est utilisée, qui est appelée par ASP.NET AJAX une fois la page et tous les inclus JavaScript bibliothèques ont été chargé.</span><span class="sxs-lookup"><span data-stu-id="a11cf-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


<span data-ttu-id="a11cf-125">[![L’animation s’exécute immédiatement, sans un clic de souris](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a11cf-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="a11cf-126">L’animation s’exécute immédiatement, sans un clic de souris ([cliquez pour afficher l’image en taille réelle](executing-animations-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a11cf-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a11cf-127">[Précédent](modifying-animations-from-the-server-side-cs.md)
[Suivant](changing-an-animation-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="a11cf-127">[Previous](modifying-animations-from-the-server-side-cs.md)
[Next](changing-an-animation-using-client-side-code-cs.md)</span></span>