---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Utilisation de plusieurs contrôles de fenêtre contextuelle (VB) | Microsoft Docs
author: wenz
description: L’extendeur PopupControl dans les outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé. Il est également possible d’utiliser m...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 5a2be36ecf17a95d53e5e912ba0a90113f70f2fb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807633"
---
<a name="using-multiple-popup-controls-vb"></a>Utilisation de plusieurs contrôles de fenêtre contextuelle (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> L’extendeur PopupControl dans les outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé. Il est également possible d’utiliser plus d’un contrôle popup sur une seule page.


## <a name="overview"></a>Vue d'ensemble

L’extendeur PopupControl dans les outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé. Il est également possible d’utiliser plus d’un contrôle popup sur une seule page.

## <a name="steps"></a>Étapes

Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

Ensuite, ajoutez un panneau qui sert de la fenêtre contextuelle. Dans le scénario en cours, le panneau contient un `Calendar` contrôle. Afin d’éviter les actualisations de la page a provoqué par des publications (postback) du calendrier, le panneau est placé dans un `UpdatePanel` contrôle :

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

La page contient également deux zones de texte. Chaque zone de texte, la fenêtre de calendrier apparaît une fois que la zone de texte est activée.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

À présent étendre les deux zones de texte avec un `PopupControlExtender`. Le `TargetControlID` attribut fournit l’ID du contrôle lié à l’extendeur. Le `PopupControlID` attribut contient l’ID du Panneau de menu contextuel. Dans ce cas, les deux extendeurs affichent le même panneau, mais différents panneaux est également possibles.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

Chaque fois que vous cliquez dans un champ de texte, un calendrier apparaît désormais sous le champ, ce qui vous permet de sélectionner une date. (Retour de la date sélectionnée dans les zones de texte sera développée dans un autre didacticiel.)


[![Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte ([cliquez pour afficher l’image en taille réelle](using-multiple-popup-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Suivant](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
