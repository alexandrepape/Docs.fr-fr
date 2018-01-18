---
title: "Création de programmes d’assistance de balise dans ASP.NET Core"
author: rick-anderson
description: "Découvrez comment créer des programmes d’assistance de balise dans ASP.NET Core."
keywords: "ASP.NET Core, les programmes d’assistance de balise"
ms.author: riande
manager: wpickett
ms.date: 06/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cbe46ee1d3cd9f7a30a87d364074f1302f9af7ab
ms.sourcegitcommit: 5834afb87e4262b9b88e60e3fe6c735e61a1e08d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/20/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="f543f-104">Programmes d’assistance de balise dans ASP.NET Core, une procédure pas à pas avec les exemples de création</span><span class="sxs-lookup"><span data-stu-id="f543f-104">Authoring Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="f543f-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f543f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f543f-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f543f-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="getting-started-with-tag-helpers"></a><span data-ttu-id="f543f-107">Prise en main de programmes d’assistance de balise</span><span class="sxs-lookup"><span data-stu-id="f543f-107">Getting started with Tag Helpers</span></span>

<span data-ttu-id="f543f-108">Ce didacticiel fournit une introduction à la programmation des programmes d’assistance de balise.</span><span class="sxs-lookup"><span data-stu-id="f543f-108">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="f543f-109">[Introduction aux applications d’assistance de balise](intro.md) décrit les avantages qui fournissent des programmes d’assistance de balise.</span><span class="sxs-lookup"><span data-stu-id="f543f-109">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="f543f-110">Une application d’assistance de balise est toute classe qui implémente le `ITagHelper` interface.</span><span class="sxs-lookup"><span data-stu-id="f543f-110">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="f543f-111">Toutefois, lorsque vous créez une application d’assistance de balise, vous dérivez généralement `TagHelper`, en procédant ainsi, l’accès à la `Process` (méthode).</span><span class="sxs-lookup"><span data-stu-id="f543f-111">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="f543f-112">Créer un nouveau projet ASP.NET Core nommé **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="f543f-112">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="f543f-113">Vous n’aurez pas d’authentification pour ce projet.</span><span class="sxs-lookup"><span data-stu-id="f543f-113">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="f543f-114">Créez un dossier pour stocker les programmes d’assistance de balise appelée *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="f543f-114">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="f543f-115">Le *TagHelpers* dossier est *pas* requis, mais il s’agit d’une convention raisonnable.</span><span class="sxs-lookup"><span data-stu-id="f543f-115">The *TagHelpers* folder is *not* required, but it is a reasonable convention.</span></span> <span data-ttu-id="f543f-116">Maintenant nous pouvons commencer l’écriture de certains programmes d’assistance de balise simple.</span><span class="sxs-lookup"><span data-stu-id="f543f-116">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="f543f-117">Un programme d’assistance de balise minimale</span><span class="sxs-lookup"><span data-stu-id="f543f-117">A minimal Tag Helper</span></span>

<span data-ttu-id="f543f-118">Dans cette section, vous écrivez une application d’assistance de balise qui met à jour une balise par courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="f543f-118">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="f543f-119">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f543f-119">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="f543f-120">Le serveur utilisera notre programme d’assistance de balise par courrier électronique à convertir ce balisage dans les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="f543f-120">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

<span data-ttu-id="f543f-121">Autrement dit, une balise d’ancrage qui rend cette un lien par courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="f543f-121">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="f543f-122">Vous pouvez souhaiter procéder ainsi si vous écrivez un moteur de blog et que vous avez besoin pour envoyer un courrier électronique pour le marketing, prise en charge et autres contacts, tous au même domaine.</span><span class="sxs-lookup"><span data-stu-id="f543f-122">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="f543f-123">Ajoutez le code suivant `EmailTagHelper` classe le *TagHelpers* dossier.</span><span class="sxs-lookup"><span data-stu-id="f543f-123">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    <span data-ttu-id="f543f-124">**Remarques :**</span><span class="sxs-lookup"><span data-stu-id="f543f-124">**Notes:**</span></span>
    
    * <span data-ttu-id="f543f-125">Programmes d’assistance de la balise utilisent une convention d’affectation de noms qui cible des éléments de la classe racine (moins le *TagHelper* partie du nom de classe).</span><span class="sxs-lookup"><span data-stu-id="f543f-125">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="f543f-126">Dans cet exemple, le nom de racine **messagerie**TagHelper est *messagerie*, donc le `<email>` balise est ciblé.</span><span class="sxs-lookup"><span data-stu-id="f543f-126">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="f543f-127">Cette convention d’affectation de noms doit fonctionner pour la plupart des programmes d’assistance de balise, plus tard, je vais montrer comment substituer.</span><span class="sxs-lookup"><span data-stu-id="f543f-127">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="f543f-128">La classe `EmailTagHelper` dérive de la classe `TagHelper`.</span><span class="sxs-lookup"><span data-stu-id="f543f-128">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="f543f-129">La `TagHelper` classe fournit les méthodes et propriétés pour l’écriture de programmes d’assistance de balise.</span><span class="sxs-lookup"><span data-stu-id="f543f-129">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="f543f-130">Le substituée `Process` méthode contrôle ce que fait l’application d’assistance de balise lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="f543f-130">The  overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="f543f-131">Le `TagHelper` classe fournit également une version asynchrone (`ProcessAsync`) avec les mêmes paramètres.</span><span class="sxs-lookup"><span data-stu-id="f543f-131">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="f543f-132">Le paramètre de contexte à `Process` (et `ProcessAsync`) contient des informations associées à l’exécution de la balise HTML actuelle.</span><span class="sxs-lookup"><span data-stu-id="f543f-132">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="f543f-133">Le paramètre de sortie à `Process` (et `ProcessAsync`) contient un élément HTML avec état représentatif de la source d’origine utilisée pour générer une balise HTML et le contenu.</span><span class="sxs-lookup"><span data-stu-id="f543f-133">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="f543f-134">Le nom de notre classe comporte un suffixe de **TagHelper**, qui est *pas* requis, mais elle est considérée comme une meilleure convention pratique.</span><span class="sxs-lookup"><span data-stu-id="f543f-134">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="f543f-135">Vous pouvez déclarer la classe en tant que :</span><span class="sxs-lookup"><span data-stu-id="f543f-135">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="f543f-136">Pour rendre le `EmailTagHelper` classe disponible pour toutes les vues de notre Razor, ajoutez le `addTagHelper` directive pour le *Views/_ViewImports.cshtml* fichier :[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="f543f-136">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="f543f-137">Le code ci-dessus utilise la syntaxe des caractères génériques pour spécifier que tous les programmes d’assistance de balise dans notre assembly seront disponibles.</span><span class="sxs-lookup"><span data-stu-id="f543f-137">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="f543f-138">La première chaîne après `@addTagHelper` Spécifie l’application d’assistance de balise à charger (utilisez « * » pour tous les programmes d’assistance de balise), et la deuxième chaîne de « AuthoringTagHelpers » Spécifie l’assembly de l’application d’assistance de balise.</span><span class="sxs-lookup"><span data-stu-id="f543f-138">The first string after `@addTagHelper` specifies the tag helper to load (Use "*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="f543f-139">En outre, notez que la deuxième ligne met dans les programmes d’assistance de balise de noyaux de ASP.NET MVC à l’aide de la syntaxe des caractères génériques (ces programmes d’assistance sont décrites dans [Introduction aux programmes d’assistance de balise](intro.md).) Il s’agit du `@addTagHelper` directive qui rend l’application d’assistance de balise disponibles à la vue Razor.</span><span class="sxs-lookup"><span data-stu-id="f543f-139">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="f543f-140">Ou bien, vous pouvez fournir le nom qualifié complet (FQN) d’une application d’assistance de balise comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="f543f-140">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
<span data-ttu-id="f543f-141">Pour ajouter un programme d’assistance de balise à une vue à l’aide d’un FQN, vous ajoutez d’abord le FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) et le nom d’assembly (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="f543f-141">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="f543f-142">La plupart des développeurs préfèrent utiliser la syntaxe des caractères génériques.</span><span class="sxs-lookup"><span data-stu-id="f543f-142">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="f543f-143">[Introduction aux applications d’assistance de balise](intro.md) détaille syntaxe Ajout, suppression, hiérarchie et de caractères génériques des balises d’assistance.</span><span class="sxs-lookup"><span data-stu-id="f543f-143">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="f543f-144">Mettre à jour le balisage dans le *Views/Home/Contact.cshtml* fichier avec ces modifications :</span><span class="sxs-lookup"><span data-stu-id="f543f-144">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  <span data-ttu-id="f543f-145">Exécutez l’application et utiliser votre navigateur favori pour afficher la source HTML afin de vérifier que les balises de courrier électronique sont remplacés par un balisage d’ancrage (par exemple, `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="f543f-145">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="f543f-146">*Prise en charge* et *Marketing* sont rendus sous la forme d’un liens, mais ils n’ont pas un `href` attribut pour les rendre fonctionnel.</span><span class="sxs-lookup"><span data-stu-id="f543f-146">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="f543f-147">Qui sera résolu dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="f543f-147">We'll fix that in the next section.</span></span>

<span data-ttu-id="f543f-148">Remarque : Comme des balises HTML et des attributs, des balises, des noms de classe et des attributs dans Razor et c# ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="f543f-148">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="f543f-149">SetAttribute et SetContent</span><span class="sxs-lookup"><span data-stu-id="f543f-149">SetAttribute and SetContent</span></span>

<span data-ttu-id="f543f-150">Dans cette section, nous allons mettre à jour le `EmailTagHelper` afin qu’il crée une balise d’ancrage valide pour le courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="f543f-150">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="f543f-151">Nous allons mettre à jour pour tirer des informations à partir d’une vue Razor (sous la forme d’un `mail-to` attribut) et l’utiliser lors de la génération de l’ancre.</span><span class="sxs-lookup"><span data-stu-id="f543f-151">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="f543f-152">Mise à jour la `EmailTagHelper` classe par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f543f-152">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

<span data-ttu-id="f543f-153">**Remarques :**</span><span class="sxs-lookup"><span data-stu-id="f543f-153">**Notes:**</span></span>

* <span data-ttu-id="f543f-154">Noms de classe et la propriété casse Pascal pour les programmes d’assistance de balise sont traduites en leur [réduire les cas rapide](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="f543f-154">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="f543f-155">Par conséquent, pour utiliser le `MailTo` attribut, vous allez utiliser `<email mail-to="value"/>` équivalent.</span><span class="sxs-lookup"><span data-stu-id="f543f-155">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="f543f-156">La dernière ligne définit le contenu terminé pour notre programme d’assistance de balise fonctionnel au minimum.</span><span class="sxs-lookup"><span data-stu-id="f543f-156">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="f543f-157">La ligne en surbrillance montre la syntaxe pour l’ajout d’attributs :</span><span class="sxs-lookup"><span data-stu-id="f543f-157">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="f543f-158">Cette approche fonctionne pour l’attribut « href » tant qu’il n’existe pas actuellement dans la collection d’attributs.</span><span class="sxs-lookup"><span data-stu-id="f543f-158">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="f543f-159">Vous pouvez également utiliser le `output.Attributes.Add` méthode pour ajouter un attribut d’assistance de balise à la fin de la collection d’attributs de balise.</span><span class="sxs-lookup"><span data-stu-id="f543f-159">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="f543f-160">Mettre à jour le balisage dans le *Views/Home/Contact.cshtml* fichier avec ces modifications :[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="f543f-160">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="f543f-161">Exécutez l’application et vérifiez qu’il génère les liens corrects.</span><span class="sxs-lookup"><span data-stu-id="f543f-161">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="f543f-162">Si vous deviez écrire la balise de messagerie fermeture automatique (`<email mail-to="Rick" />`), la sortie finale serait également la fermeture automatique.</span><span class="sxs-lookup"><span data-stu-id="f543f-162">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="f543f-163">Pour activer la capacité d’écrire la balise avec uniquement une balise de début (`<email mail-to="Rick">`) vous devez décorer la classe par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f543f-163">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    <span data-ttu-id="f543f-164">Avec une assistance de balise fermeture automatique par courrier électronique, le résultat serait `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="f543f-164">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="f543f-165">Les balises d’ancrage fermeture automatique ne sont pas valide en HTML, afin que vous ne souhaitez pas créer un, mais vous souhaiterez peut-être créer un programme d’assistance de balise se ferme automatiquement.</span><span class="sxs-lookup"><span data-stu-id="f543f-165">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that is self-closing.</span></span> <span data-ttu-id="f543f-166">Programmes d’assistance de balise définir le type de la `TagMode` propriété après la lecture d’une balise.</span><span class="sxs-lookup"><span data-stu-id="f543f-166">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="f543f-167">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="f543f-167">ProcessAsync</span></span>

<span data-ttu-id="f543f-168">Dans cette section, nous allons écrire une application d’assistance de messagerie asynchrone.</span><span class="sxs-lookup"><span data-stu-id="f543f-168">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="f543f-169">Remplacez la `EmailTagHelper` classe par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f543f-169">Replace the `EmailTagHelper` class with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    <span data-ttu-id="f543f-170">**Remarques :**</span><span class="sxs-lookup"><span data-stu-id="f543f-170">**Notes:**</span></span>

    * <span data-ttu-id="f543f-171">Cette version utilise asynchrone `ProcessAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="f543f-171">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="f543f-172">Asynchrone `GetChildContentAsync` retourne un `Task` contenant le `TagHelperContent`.</span><span class="sxs-lookup"><span data-stu-id="f543f-172">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="f543f-173">Utilisez le `output` pour obtenir le contenu de l’élément HTML.</span><span class="sxs-lookup"><span data-stu-id="f543f-173">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="f543f-174">Apportez la modification suivante à la *Views/Home/Contact.cshtml* de fichiers pour permettre l’application d’assistance de balise d’obtenir l’adresse de messagerie cible.</span><span class="sxs-lookup"><span data-stu-id="f543f-174">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  <span data-ttu-id="f543f-175">Exécutez l’application et vérifiez qu’il génère des liens de messagerie valide.</span><span class="sxs-lookup"><span data-stu-id="f543f-175">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="f543f-176">RemoveAll, PreContent.SetHtmlContent et PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="f543f-176">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="f543f-177">Ajoutez le code suivant `BoldTagHelper` classe le *TagHelpers* dossier.</span><span class="sxs-lookup"><span data-stu-id="f543f-177">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    <span data-ttu-id="f543f-178">**Remarques :**</span><span class="sxs-lookup"><span data-stu-id="f543f-178">**Notes:**</span></span>
    
    * <span data-ttu-id="f543f-179">Le `[HtmlTargetElement]` passe un paramètre d’attribut qui spécifie qu’un élément HTML qui contient un attribut HTML nommé « bold » correspondra, d’attribut et la `Process` méthode de substitution dans la classe s’exécute.</span><span class="sxs-lookup"><span data-stu-id="f543f-179">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="f543f-180">Dans notre exemple, le `Process` méthode supprime l’attribut « bold » et entoure le balisage contenant avec `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="f543f-180">In our sample, the `Process`  method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="f543f-181">Car vous ne souhaitez pas remplacer la balise contenue, vous devez écrire l’ouverture `<strong>` balise avec le `PreContent.SetHtmlContent` (méthode) et la clôture `</strong>` la balise avec le `PostContent.SetHtmlContent` (méthode).</span><span class="sxs-lookup"><span data-stu-id="f543f-181">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="f543f-182">Modifier la *About.cshtml* vue contienne un `bold` valeur d’attribut.</span><span class="sxs-lookup"><span data-stu-id="f543f-182">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="f543f-183">Le code complet est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="f543f-183">The completed code is shown below.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  <span data-ttu-id="f543f-184">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="f543f-184">Run the app.</span></span> <span data-ttu-id="f543f-185">Vous pouvez utiliser votre navigateur préféré pour inspecter la source et de vérifier le balisage.</span><span class="sxs-lookup"><span data-stu-id="f543f-185">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="f543f-186">Le `[HtmlTargetElement]` attribut ci-dessus cible uniquement le balisage HTML qui fournit un nom d’attribut de « bold ».</span><span class="sxs-lookup"><span data-stu-id="f543f-186">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="f543f-187">Le `<bold>` élément n’a pas été modifié par l’application d’assistance de balise.</span><span class="sxs-lookup"><span data-stu-id="f543f-187">The `<bold>` element was not modified by the tag helper.</span></span>

4. <span data-ttu-id="f543f-188">Commentez la `[HtmlTargetElement]` ligne d’attribut et il utilisera par défaut ciblant `<bold>` balises, autrement dit, un balisage HTML sous la forme `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="f543f-188">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="f543f-189">N’oubliez pas, la convention d’affectation de noms par défaut correspond au nom de classe **gras**TagHelper à `<bold>` balises.</span><span class="sxs-lookup"><span data-stu-id="f543f-189">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="f543f-190">Exécutez l’application et vérifiez que le `<bold>` la balise est traitée par l’application d’assistance de balise.</span><span class="sxs-lookup"><span data-stu-id="f543f-190">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="f543f-191">Le fait de décorer une classe avec plusieurs `[HtmlTargetElement]` les résultats dans une opération OR logique des cibles d’attributs.</span><span class="sxs-lookup"><span data-stu-id="f543f-191">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="f543f-192">Par exemple, le code ci-dessous, une balise en gras ou un attribut gras est mettre en correspondance.</span><span class="sxs-lookup"><span data-stu-id="f543f-192">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="f543f-193">Lorsque plusieurs attributs sont ajoutés à la même instruction, le runtime les traite comme un AND logique.</span><span class="sxs-lookup"><span data-stu-id="f543f-193">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="f543f-194">Par exemple, dans le code ci-dessous, un élément HTML doit être nommé « bold » avec un attribut nommé « bold » (`<bold bold />`) pour faire correspondre.</span><span class="sxs-lookup"><span data-stu-id="f543f-194">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="f543f-195">Vous pouvez également utiliser le `[HtmlTargetElement]` pour modifier le nom de l’élément ciblé.</span><span class="sxs-lookup"><span data-stu-id="f543f-195">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="f543f-196">Par exemple, si vous souhaitiez le `BoldTagHelper` à cibler `<MyBold>` balises, vous utiliseriez l’attribut suivant :</span><span class="sxs-lookup"><span data-stu-id="f543f-196">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a><span data-ttu-id="f543f-197">Passage d’un modèle dans une application d’assistance de balise</span><span class="sxs-lookup"><span data-stu-id="f543f-197">Passing a model to a Tag Helper</span></span>

1.  <span data-ttu-id="f543f-198">Ajouter un *modèles* dossier.</span><span class="sxs-lookup"><span data-stu-id="f543f-198">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="f543f-199">Ajoutez la classe `WebsiteContext` suivante au dossier *Models* :</span><span class="sxs-lookup"><span data-stu-id="f543f-199">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  <span data-ttu-id="f543f-200">Ajoutez le code suivant `WebsiteInformationTagHelper` classe le *TagHelpers* dossier.</span><span class="sxs-lookup"><span data-stu-id="f543f-200">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    <span data-ttu-id="f543f-201">**Remarques :**</span><span class="sxs-lookup"><span data-stu-id="f543f-201">**Notes:**</span></span>
    
    * <span data-ttu-id="f543f-202">Comme mentionné précédemment, les programmes d’assistance de balise traduit les noms de classe C# casse Pascal et des propriétés pour les programmes d’assistance de balise dans [réduire les cas rapide](http://wiki.c2.com/?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="f543f-202">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="f543f-203">Par conséquent, pour utiliser le `WebsiteInformationTagHelper` dans Razor, vous allez écrire `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="f543f-203">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="f543f-204">Vous ne sont pas identifiant explicitement l’élément cible avec le `[HtmlTargetElement]` attribut, la valeur par défaut de `website-information` sont ciblés.</span><span class="sxs-lookup"><span data-stu-id="f543f-204">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="f543f-205">Si vous avez appliqué l’attribut suivant (Notez qu’il n’est pas rapide cas mais il correspond au nom de la classe) :</span><span class="sxs-lookup"><span data-stu-id="f543f-205">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="f543f-206">La balise de cas rapide inférieure `<website-information />` ne correspondent pas.</span><span class="sxs-lookup"><span data-stu-id="f543f-206">The lower kebab case tag `<website-information />` would not match.</span></span> <span data-ttu-id="f543f-207">Si vous souhaitez utiliser le `[HtmlTargetElement]` attribut, vous utiliseriez cas rapide comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="f543f-207">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="f543f-208">Les éléments qui sont de fermeture automatique ont aucun contenu.</span><span class="sxs-lookup"><span data-stu-id="f543f-208">Elements that are self-closing have no content.</span></span> <span data-ttu-id="f543f-209">Pour cet exemple, le balisage Razor utilise une balise de fermeture automatique, mais l’application d’assistance de balise créeront un [section](http://www.w3.org/TR/html5/sections.html#the-section-element) élément (qui n’est pas une fermeture automatique et que vous écrivez le contenu à l’intérieur du `section` élément).</span><span class="sxs-lookup"><span data-stu-id="f543f-209">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which is not self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="f543f-210">Par conséquent, vous devez définir `TagMode` à `StartTagAndEndTag` pour écrire la sortie.</span><span class="sxs-lookup"><span data-stu-id="f543f-210">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="f543f-211">Ou bien, vous pouvez placer en commentaire le paramètre de ligne `TagMode` et écrire les balises avec une balise de fermeture.</span><span class="sxs-lookup"><span data-stu-id="f543f-211">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="f543f-212">(Le balisage de l’exemple est fourni plus loin dans ce didacticiel.)</span><span class="sxs-lookup"><span data-stu-id="f543f-212">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="f543f-213">Le `$` (signe dollar) dans la ligne suivante utilise une [interpolées chaîne](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="f543f-213">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="f543f-214">Ajoutez le balisage suivant à la *About.cshtml* vue.</span><span class="sxs-lookup"><span data-stu-id="f543f-214">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="f543f-215">La balise en surbrillance affiche les informations de site web.</span><span class="sxs-lookup"><span data-stu-id="f543f-215">The highlighted markup displays the web site information.</span></span>
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > <span data-ttu-id="f543f-216">Dans le balisage de Razor indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="f543f-216">In the Razor markup shown below:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    ><span data-ttu-id="f543f-217">Razor connaît le `info` attribut est une classe, et non une chaîne, et que vous souhaitez écrire du code c#.</span><span class="sxs-lookup"><span data-stu-id="f543f-217">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="f543f-218">N’importe quel attribut d’assistance de balise de chaîne non doit être écrite sans la `@` caractère.</span><span class="sxs-lookup"><span data-stu-id="f543f-218">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="f543f-219">Exécuter l’application et accédez à la vue à propos de pour afficher les informations de site web.</span><span class="sxs-lookup"><span data-stu-id="f543f-219">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="f543f-220">Vous pouvez utiliser le balisage suivant avec une balise de fermeture et supprimez la ligne avec `TagMode.StartTagAndEndTag` dans l’application d’assistance de balise :</span><span class="sxs-lookup"><span data-stu-id="f543f-220">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="f543f-221">Condition d’assistance de balise</span><span class="sxs-lookup"><span data-stu-id="f543f-221">Condition Tag Helper</span></span>

<span data-ttu-id="f543f-222">L’application d’assistance de balise condition restitue la sortie lorsqu’il est passé de la valeur true.</span><span class="sxs-lookup"><span data-stu-id="f543f-222">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="f543f-223">Ajoutez le code suivant `ConditionTagHelper` classe le *TagHelpers* dossier.</span><span class="sxs-lookup"><span data-stu-id="f543f-223">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  <span data-ttu-id="f543f-224">Remplacez le contenu de la *Views/Home/Index.cshtml* fichier par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="f543f-224">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=@Model />
        <div condition="@Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  <span data-ttu-id="f543f-225">Remplacez le `Index` méthode dans le `Home` contrôleur avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f543f-225">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  <span data-ttu-id="f543f-226">Exécuter l’application et accédez à la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="f543f-226">Run the app and browse to the home page.</span></span> <span data-ttu-id="f543f-227">Le balisage de l’attribut conditional `div` ne sont pas rendus.</span><span class="sxs-lookup"><span data-stu-id="f543f-227">The markup in the conditional `div` will not be rendered.</span></span> <span data-ttu-id="f543f-228">Ajouter la chaîne de requête `?approved=true` à l’URL (par exemple, `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="f543f-228">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="f543f-229">`approved`a la valeur true et que l’attribut conditional balisage s’affichera.</span><span class="sxs-lookup"><span data-stu-id="f543f-229">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="f543f-230">Utilisez le [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) opérateur pour spécifier l’attribut cible, plutôt que de spécifier une chaîne comme vous le faisiez avec l’application d’assistance de balise en gras :</span><span class="sxs-lookup"><span data-stu-id="f543f-230">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
><span data-ttu-id="f543f-231">Le [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) opérateur protège le code doit il jamais être refactorisé (nous voulons modifier le nom à `RedCondition`).</span><span class="sxs-lookup"><span data-stu-id="f543f-231">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoiding-tag-helper-conflicts"></a><span data-ttu-id="f543f-232">Prévention des conflits d’application d’assistance de balise</span><span class="sxs-lookup"><span data-stu-id="f543f-232">Avoiding Tag Helper conflicts</span></span>

<span data-ttu-id="f543f-233">Dans cette section, vous écrivez une paire de liaison automatique les programmes d’assistance de balise.</span><span class="sxs-lookup"><span data-stu-id="f543f-233">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="f543f-234">La première remplacera le balisage contenant une URL commençant par HTTP à un HTML d’ancrage balise qui contient la même URL (et produisant ainsi un lien vers l’URL).</span><span class="sxs-lookup"><span data-stu-id="f543f-234">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="f543f-235">Le second sera faites de même pour une URL commençant par WWW.</span><span class="sxs-lookup"><span data-stu-id="f543f-235">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="f543f-236">Étant donné que ces deux programmes d’assistance sont étroitement liés et peuvent les refactoriser à l’avenir, nous allons les conserver dans le même fichier.</span><span class="sxs-lookup"><span data-stu-id="f543f-236">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="f543f-237">Ajoutez le code suivant `AutoLinkerHttpTagHelper` classe le *TagHelpers* dossier.</span><span class="sxs-lookup"><span data-stu-id="f543f-237">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    ><span data-ttu-id="f543f-238">Le `AutoLinkerHttpTagHelper` classe cibles `p` éléments et utilise [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) pour créer le point d’ancrage.</span><span class="sxs-lookup"><span data-stu-id="f543f-238">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2.  <span data-ttu-id="f543f-239">Ajoutez le balisage suivant à la fin de la *Views/Home/Contact.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="f543f-239">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  <span data-ttu-id="f543f-240">Exécutez l’application et vérifiez que l’application d’assistance de balise restitue correctement le point d’ancrage.</span><span class="sxs-lookup"><span data-stu-id="f543f-240">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="f543f-241">Mise à jour la `AutoLinker` classe pour inclure le `AutoLinkerWwwTagHelper` qui convertira www texte à une balise d’ancrage qui contient également le texte www d’origine.</span><span class="sxs-lookup"><span data-stu-id="f543f-241">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="f543f-242">Le code mis à jour est mis en surbrillance ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="f543f-242">The updated code is highlighted below:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  <span data-ttu-id="f543f-243">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="f543f-243">Run the app.</span></span> <span data-ttu-id="f543f-244">Notez que le texte www est affiché sous forme de lien, mais le texte HTTP n’est pas.</span><span class="sxs-lookup"><span data-stu-id="f543f-244">Notice the www text is rendered as a link but the HTTP text is not.</span></span> <span data-ttu-id="f543f-245">Si vous placez un point d’arrêt dans les deux classes, vous pouvez voir que la classe d’assistance de balise HTTP s’exécute en premier.</span><span class="sxs-lookup"><span data-stu-id="f543f-245">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="f543f-246">Le problème est que la sortie d’assistance de balise est mis en cache, et lorsque l’application d’assistance de balise WWW est exécutée, il remplace la sortie mise en cache à partir de l’application d’assistance de balise HTTP.</span><span class="sxs-lookup"><span data-stu-id="f543f-246">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="f543f-247">Plus loin dans ce didacticiel, nous verrons comment contrôler l’ordre dans lequel exécutent des programmes d’assistance de balise dans.</span><span class="sxs-lookup"><span data-stu-id="f543f-247">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="f543f-248">Nous allons corriger le code avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="f543f-248">We'll fix the code with the following:</span></span>

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    ><span data-ttu-id="f543f-249">Dans la première édition des programmes d’assistance des balises de la liaison automatique, que vous avez obtenu le contenu de la cible avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f543f-249">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    ><span data-ttu-id="f543f-250">Autrement dit, vous appelez `GetChildContentAsync` à l’aide de la `TagHelperOutput` passé dans le `ProcessAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="f543f-250">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="f543f-251">Comme mentionné précédemment, étant donné que la sortie est mise en cache, la dernière balise d’assistance pour exécuter wins.</span><span class="sxs-lookup"><span data-stu-id="f543f-251">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="f543f-252">Vous avez résolu ce problème avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f543f-252">You fixed that problem with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    ><span data-ttu-id="f543f-253">Le code ci-dessus vérifie si le contenu a été modifié et s’il a, il obtient le contenu à partir de la mémoire tampon de sortie.</span><span class="sxs-lookup"><span data-stu-id="f543f-253">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="f543f-254">Exécutez l’application et vérifiez que les deux liaisons fonctionnent comme prévu.</span><span class="sxs-lookup"><span data-stu-id="f543f-254">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="f543f-255">Pendant qu’il peut apparaître de que notre programme d’assistance de balise de l’éditeur de liens automatique est correcte et complète, il a un problème subtil.</span><span class="sxs-lookup"><span data-stu-id="f543f-255">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="f543f-256">Si l’application d’assistance de balise WWW s’exécute en premier, il se peut que les liens Web ne seront pas correctes.</span><span class="sxs-lookup"><span data-stu-id="f543f-256">If the WWW tag helper runs first, the www links will not be correct.</span></span> <span data-ttu-id="f543f-257">Mettre à jour le code en ajoutant le `Order` pour contrôler l’ordre de la balise s’exécute dans la surcharge.</span><span class="sxs-lookup"><span data-stu-id="f543f-257">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="f543f-258">Le `Order` propriété détermine l’ordre d’exécution par rapport à d’autres programmes d’assistance de balise ciblant le même élément.</span><span class="sxs-lookup"><span data-stu-id="f543f-258">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="f543f-259">La valeur d’ordre par défaut est égale à zéro et les instances ayant des valeurs inférieures sont exécutées en premier.</span><span class="sxs-lookup"><span data-stu-id="f543f-259">The default order value is zero and instances with lower values are executed first.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    <span data-ttu-id="f543f-260">Le code ci-dessus garantit que l’application d’assistance de balise HTTP s’exécute avant l’application d’assistance de balise WWW.</span><span class="sxs-lookup"><span data-stu-id="f543f-260">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="f543f-261">Modification `Order` à `MaxValue` et vérifiez que le balisage généré pour la balise WWW est incorrect.</span><span class="sxs-lookup"><span data-stu-id="f543f-261">Change `Order` to `MaxValue` and verify that the markup generated for the  WWW tag is incorrect.</span></span>

## <a name="inspecting-and-retrieving-child-content"></a><span data-ttu-id="f543f-262">Inspection et la récupération du contenu enfant</span><span class="sxs-lookup"><span data-stu-id="f543f-262">Inspecting and retrieving child content</span></span>

<span data-ttu-id="f543f-263">Les programmes d’assistance de balise fournissent plusieurs propriétés permettant de récupérer le contenu.</span><span class="sxs-lookup"><span data-stu-id="f543f-263">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="f543f-264">Le résultat de `GetChildContentAsync` peuvent être ajoutés à `output.Content`.</span><span class="sxs-lookup"><span data-stu-id="f543f-264">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="f543f-265">Vous pouvez examiner le résultat de `GetChildContentAsync` avec `GetContent`.</span><span class="sxs-lookup"><span data-stu-id="f543f-265">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="f543f-266">Si vous modifiez `output.Content`, le corps TagHelper ne sera pas exécuté ou restitué, sauf si vous appelez `GetChildContentAsync` comme dans notre exemple auto-éditeur de liens :</span><span class="sxs-lookup"><span data-stu-id="f543f-266">If you modify `output.Content`, the TagHelper body will not be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  <span data-ttu-id="f543f-267">Appels multiples à `GetChildContentAsync` renvoie la même valeur et ne sera pas exécuter de nouveau la `TagHelper` de corps, sauf si vous passez dans un paramètre false indiquant n’utilise pas le résultat mis en cache.</span><span class="sxs-lookup"><span data-stu-id="f543f-267">Multiple calls to `GetChildContentAsync` will return the same value and will not re-execute the `TagHelper` body unless you pass in a false parameter indicating  not use the cached result.</span></span>