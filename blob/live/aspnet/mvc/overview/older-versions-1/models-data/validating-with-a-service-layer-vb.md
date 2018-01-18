---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: Validation avec une couche de Service (VB) | Documents Microsoft
author: StephenWalther
description: "Découvrez comment déplacer votre logique de validation en dehors de vos actions de contrôleur et dans une couche de service distinct. Dans ce didacticiel, Stephen Walther explique comment vous..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 5a8f1dd888c7fa6a3353b7b748a0ffa30b94149c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="validating-with-a-service-layer-vb"></a><span data-ttu-id="7a592-104">Validation avec une couche de Service (VB)</span><span class="sxs-lookup"><span data-stu-id="7a592-104">Validating with a Service Layer (VB)</span></span>
====================
<span data-ttu-id="7a592-105">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="7a592-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="7a592-106">Découvrez comment déplacer votre logique de validation en dehors de vos actions de contrôleur et dans une couche de service distinct.</span><span class="sxs-lookup"><span data-stu-id="7a592-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="7a592-107">Dans ce didacticiel, Stephen Walther explique comment vous pouvez maintenir une séparation AIGUE des problèmes en isolant votre couche de service à partir de votre couche de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7a592-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="7a592-108">L’objectif de ce didacticiel est de décrire une méthode d’effectuer la validation dans une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7a592-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="7a592-109">Dans ce didacticiel, vous allez apprendre à déplacer votre logique de validation en dehors de vos contrôleurs et dans une couche de service distinct.</span><span class="sxs-lookup"><span data-stu-id="7a592-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="7a592-110">Séparation des problèmes</span><span class="sxs-lookup"><span data-stu-id="7a592-110">Separating Concerns</span></span>

<span data-ttu-id="7a592-111">Lorsque vous générez une application ASP.NET MVC, vous ne devez pas placer votre logique de la base de données à l’intérieur de vos actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7a592-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="7a592-112">Mélange de votre logique de la base de données et de contrôleur rend votre application plus difficile à gérer au fil du temps.</span><span class="sxs-lookup"><span data-stu-id="7a592-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="7a592-113">Il est recommandé que vous placez toute logique de votre base de données dans une couche distincte de référentiel.</span><span class="sxs-lookup"><span data-stu-id="7a592-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="7a592-114">Par exemple, la liste 1 contient un référentiel simple nommé le ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="7a592-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="7a592-115">Le référentiel de produit contient toutes les le code d’accès aux données pour l’application.</span><span class="sxs-lookup"><span data-stu-id="7a592-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="7a592-116">La liste inclut également l’interface IProductRepository implémentant le référentiel de produit.</span><span class="sxs-lookup"><span data-stu-id="7a592-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="7a592-117">**La liste 1 - Models\ProductRepository.vb**</span><span class="sxs-lookup"><span data-stu-id="7a592-117">**Listing 1 - Models\ProductRepository.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

<span data-ttu-id="7a592-118">Le contrôleur dans la liste 2 utilise la couche de référentiels dans son Index() et Create() actions.</span><span class="sxs-lookup"><span data-stu-id="7a592-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="7a592-119">Notez que ce contrôleur ne contient pas la logique de base de données.</span><span class="sxs-lookup"><span data-stu-id="7a592-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="7a592-120">Création d’une couche de référentiel permet d’établir une séparation claire des préoccupations.</span><span class="sxs-lookup"><span data-stu-id="7a592-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="7a592-121">Les contrôleurs sont responsables de la logique de contrôle de flux application et le référentiel est responsable de la logique d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="7a592-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="7a592-122">**Listing 2 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="7a592-122">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="7a592-123">Création d’une couche de Service</span><span class="sxs-lookup"><span data-stu-id="7a592-123">Creating a Service Layer</span></span>

<span data-ttu-id="7a592-124">Par conséquent, la logique de contrôle de flux application appartienne à un contrôleur d’et appartienne de logique d’accès aux données dans un référentiel.</span><span class="sxs-lookup"><span data-stu-id="7a592-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="7a592-125">Dans ce cas, où placer votre logique de validation ?</span><span class="sxs-lookup"><span data-stu-id="7a592-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="7a592-126">Une option consiste à placer votre logique de validation dans un *couche de service*.</span><span class="sxs-lookup"><span data-stu-id="7a592-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="7a592-127">Une couche de service est une couche supplémentaire dans une application ASP.NET MVC qui sert d’intermédiaire pour la communication entre un contrôleur et une couche de référentiel.</span><span class="sxs-lookup"><span data-stu-id="7a592-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="7a592-128">La couche de service contient la logique métier.</span><span class="sxs-lookup"><span data-stu-id="7a592-128">The service layer contains business logic.</span></span> <span data-ttu-id="7a592-129">En particulier, il contient la logique de validation.</span><span class="sxs-lookup"><span data-stu-id="7a592-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="7a592-130">Par exemple, la couche de service de produit dans la liste 3 a une méthode CreateProduct().</span><span class="sxs-lookup"><span data-stu-id="7a592-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="7a592-131">La méthode CreateProduct() appelle la méthode de ValidateProduct() pour valider un nouveau produit avant de passer le produit dans le référentiel de produit.</span><span class="sxs-lookup"><span data-stu-id="7a592-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="7a592-132">**La liste 3 - Models\ProductService.vb**</span><span class="sxs-lookup"><span data-stu-id="7a592-132">**Listing 3 - Models\ProductService.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

<span data-ttu-id="7a592-133">Le contrôleur de produit a été mis à jour dans la liste de 4 à utiliser la couche de service au lieu de la couche de référentiels.</span><span class="sxs-lookup"><span data-stu-id="7a592-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="7a592-134">La couche de contrôleur communique avec la couche de service.</span><span class="sxs-lookup"><span data-stu-id="7a592-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="7a592-135">La couche de service communique avec la couche de référentiels.</span><span class="sxs-lookup"><span data-stu-id="7a592-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="7a592-136">Chaque couche possède une responsabilité distincte.</span><span class="sxs-lookup"><span data-stu-id="7a592-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="7a592-137">**La liste 4 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="7a592-137">**Listing 4 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

<span data-ttu-id="7a592-138">Notez que le service de produit est créé dans le constructeur de contrôleur du produit.</span><span class="sxs-lookup"><span data-stu-id="7a592-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="7a592-139">Lorsque le service de produit est créé, le dictionnaire d’états de modèles est passé au service.</span><span class="sxs-lookup"><span data-stu-id="7a592-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="7a592-140">Le service de produit utilise l’état du modèle pour transmettre les messages d’erreur de la validation vers le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7a592-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="7a592-141">Dissociation de la couche de Service</span><span class="sxs-lookup"><span data-stu-id="7a592-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="7a592-142">Nous avons a échoué isoler le contrôleur et les couches de service dans un point.</span><span class="sxs-lookup"><span data-stu-id="7a592-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="7a592-143">Le contrôleur et les couches de service communiquent à l’état de modèle.</span><span class="sxs-lookup"><span data-stu-id="7a592-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="7a592-144">En d’autres termes, la couche de service a une dépendance sur une fonctionnalité particulière de l’infrastructure ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7a592-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="7a592-145">Vous souhaitez isoler la couche de service à partir de notre couche contrôleur autant que possible.</span><span class="sxs-lookup"><span data-stu-id="7a592-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="7a592-146">En théorie, nous devons être en mesure d’utiliser la couche de service avec n’importe quel type d’application et non seulement une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7a592-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="7a592-147">Par exemple, à l’avenir, nous souhaitions générer un frontal pour notre application de WPF.</span><span class="sxs-lookup"><span data-stu-id="7a592-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="7a592-148">Nous étudions trouver un moyen de supprimer la dépendance sur ASP.NET MVC état du modèle à partir de la couche de service.</span><span class="sxs-lookup"><span data-stu-id="7a592-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="7a592-149">Dans la liste 5, la couche de service a été mise à jour afin qu’il n’utilise plus le modèle d’état.</span><span class="sxs-lookup"><span data-stu-id="7a592-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="7a592-150">Au lieu de cela, il utilise n’importe quelle classe qui implémente l’interface IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="7a592-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="7a592-151">**La liste 5 - Models\ProductService.vb (dissocié)**</span><span class="sxs-lookup"><span data-stu-id="7a592-151">**Listing 5 - Models\ProductService.vb (decoupled)**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

<span data-ttu-id="7a592-152">L’interface IValidationDictionary est défini dans la liste 6.</span><span class="sxs-lookup"><span data-stu-id="7a592-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="7a592-153">Cette interface simple possède une méthode unique et une seule propriété.</span><span class="sxs-lookup"><span data-stu-id="7a592-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="7a592-154">**La liste 6 - Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="7a592-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

<span data-ttu-id="7a592-155">La classe dans la liste 7, la classe ModelStateWrapper, nommée implémente l’interface IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="7a592-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="7a592-156">Vous pouvez instancier la classe ModelStateWrapper en passant un dictionnaire d’états de modèle au constructeur.</span><span class="sxs-lookup"><span data-stu-id="7a592-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="7a592-157">**La liste 7 - Models\ModelStateWrapper.vb**</span><span class="sxs-lookup"><span data-stu-id="7a592-157">**Listing 7 - Models\ModelStateWrapper.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

<span data-ttu-id="7a592-158">Enfin, le contrôleur de mise à jour dans la liste 8 utilise le ModelStateWrapper lors de la création de la couche de service dans son constructeur.</span><span class="sxs-lookup"><span data-stu-id="7a592-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="7a592-159">**Liste 8 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="7a592-159">**Listing 8 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

<span data-ttu-id="7a592-160">À l’aide de la IValidationDictionary interface et la classe ModelStateWrapper permet d’isoler totalement notre couche de service à partir de la couche de notre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7a592-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="7a592-161">La couche de service n’est plus dépendante de l’état du modèle.</span><span class="sxs-lookup"><span data-stu-id="7a592-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="7a592-162">Vous pouvez passer n’importe quelle classe qui implémente l’interface IValidationDictionary à la couche de service.</span><span class="sxs-lookup"><span data-stu-id="7a592-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="7a592-163">Par exemple, une application WPF peut implémenter l’interface IValidationDictionary avec une classe de collection simple.</span><span class="sxs-lookup"><span data-stu-id="7a592-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="7a592-164">Résumé</span><span class="sxs-lookup"><span data-stu-id="7a592-164">Summary</span></span>

<span data-ttu-id="7a592-165">L’objectif de ce didacticiel a pour présenter une approche pour effectuer la validation dans une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7a592-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="7a592-166">Dans ce didacticiel, vous avez appris comment déplacer tout votre logique de validation en dehors de vos contrôleurs et dans une couche de service distinct.</span><span class="sxs-lookup"><span data-stu-id="7a592-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="7a592-167">Vous avez également appris comment isoler votre couche de service à partir de votre couche de contrôleur en créant une classe ModelStateWrapper.</span><span class="sxs-lookup"><span data-stu-id="7a592-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7a592-168">[Précédent](validating-with-the-idataerrorinfo-interface-vb.md)
[Suivant](validation-with-the-data-annotation-validators-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7a592-168">[Previous](validating-with-the-idataerrorinfo-interface-vb.md)
[Next](validation-with-the-data-annotation-validators-vb.md)</span></span>