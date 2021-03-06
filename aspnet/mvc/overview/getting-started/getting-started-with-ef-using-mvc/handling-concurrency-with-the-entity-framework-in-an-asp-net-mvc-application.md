---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Gestion des accès concurrentiels avec Entity Framework 6 dans une Application ASP.NET MVC 5 (10 sur 12) | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 Code First et Visual Studio...
ms.author: aspnetcontent
ms.date: 12/08/2014
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b971e4ca16ecbf79f47e0aab8303a05452279b68
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837229"
---
<a name="handling-concurrency-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-10-of-12"></a>Gestion des accès concurrentiels avec Entity Framework 6 dans une Application ASP.NET MVC 5 (10 sur 12)
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [télécharger le PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 Code First et Visual Studio 2013. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Dans les didacticiels précédents, vous avez appris comment mettre à jour des données. Ce didacticiel montre comment gérer les conflits quand plusieurs utilisateurs mettent à jour la même entité en même temps.

Vous allez modifier les pages web qui fonctionnent avec le `Department` entité afin qu’ils gèrent les erreurs d’accès concurrentiel. Les illustrations suivantes montrent les pages d’Index et Delete, notamment certains messages sont affichés si un conflit d’accès concurrentiel se produit.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Conflits d’accès concurrentiel

Un conflit d’accès concurrentiel se produit quand un utilisateur affiche les données d’une entité pour la modifier, puis qu’un autre utilisateur met à jour les données de la même entité avant que les modifications du premier utilisateur soient écrites dans la base de données. Si vous n’activez pas la détection de ces conflits, la personne qui met à jour la base de données en dernier remplace les modifications de l’autre utilisateur. Dans de nombreuses applications, ce risque est acceptable : s’il n’y a que quelques utilisateurs ou quelques mises à jour, ou s’il n’est pas réellement critique que des modifications soient remplacées, le coût de la programmation nécessaire à la gestion des accès concurrentiels peut être supérieur au bénéfice qu’elle apporte. Dans ce cas, vous ne devez pas configurer l’application pour gérer les conflits d’accès concurrentiel.

### <a name="pessimistic-concurrency-locking"></a>Accès concurrentiel pessimiste (verrouillage)

Si votre application doit éviter la perte accidentelle de données dans des scénarios d’accès concurrentiel, une manière de le faire consiste à utiliser des verrous de base de données. Il s’agit *d’accès concurrentiel pessimiste*. Par exemple, avant de lire une ligne d’une base de données, vous demandez un verrou pour lecture seule ou pour accès avec mise à jour. Si vous verrouillez une ligne pour accès avec mise à jour, aucun autre utilisateur n’est autorisé à verrouiller la ligne pour lecture seule ou pour accès avec mise à jour, car ils obtiendraient ainsi une copie de données qui sont en cours de modification. Si vous verrouillez une ligne pour accès en lecture seule, d’autres utilisateurs peuvent également la verrouiller pour accès en lecture seule, mais pas pour accès avec mise à jour.

La gestion des verrous présente des inconvénients. Elle peut être complexe à programmer. Elle nécessite des ressources de gestion de base de données importantes, et peut provoquer des problèmes de performances au fil de l’augmentation du nombre d’utilisateurs d’une application. Pour ces raisons, certains systèmes de gestion de base de données ne prennent pas en charge l’accès concurrentiel pessimiste. Entity Framework ne fournit aucune prise en charge intégrée pour celle-ci, et ce didacticiel ne vous montre comment l’implémenter.

### <a name="optimistic-concurrency"></a>Accès concurrentiel optimiste

L’alternative à l’accès concurrentiel pessimiste est *d’accès concurrentiel optimiste*. L’accès concurrentiel optimiste signifie autoriser la survenance des conflits d’accès concurrentiel, puis de réagir correctement quand ils surviennent. Par exemple, John exécute la page de modification des départements, modifications le **Budget** montant pour le département « English » à partir de $ 350 000,00 à $0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Avant de Jean clique sur **enregistrer**, Jane exécute la même page et modifications le **Start Date** champ à partir du 1/9/2007 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John clique sur **enregistrer** premier et voit sa modification quand le navigateur revient à la page d’Index, puis Jane clique sur **enregistrer**. Ce qui se passe ensuite est déterminé par la façon dont vous gérez les conflits d’accès concurrentiel. Voici quelques-unes des options :

- Vous pouvez effectuer le suivi des propriétés modifiées par un utilisateur et mettre à jour seulement les colonnes correspondantes dans la base de données. Dans l’exemple de scénario, aucune donnée ne serait perdue, car des propriétés différentes ont été mises à jour par chacun des deux utilisateurs. La prochaine fois que quelqu'un consultera le département « English », ils voient les modifications de John et Jane, une date de début de 8/8/2013 et un budget de zéro dollars.

    Cette méthode de mise à jour peut réduire le nombre de conflits qui peuvent entraîner des pertes de données, mais elle ne peut pas éviter la perte de données si des modifications concurrentes sont apportées à la même propriété d’une entité. Un tel fonctionnement d’Entity Framework dépend de la façon dont vous implémentez votre code de mise à jour. Il n’est pas souvent pratique dans une application web, car il peut nécessiter la gestion de grandes quantités d’états pour effectuer le suivi de toutes les valeurs de propriété d’origine d’une entité, ainsi que des nouvelles valeurs. La gestion de grandes quantités d’états peut affecter les performances de l’application, car elle nécessite des ressources serveur, ou doit être incluse dans la page web elle-même (par exemple dans des champs masqués) ou dans un cookie.
- Vous pouvez laisser les modifications de Jane remplacer les modifications de John. La prochaine fois que quelqu'un consultera le département « English », ils voient 8/8/2013 et la valeur de $ 350 000,00 restaurée. Ceci s’appelle un scénario *Priorité au client* ou *Priorité au dernier entré* (Last in Wins). (Toutes les valeurs provenant du client sont prioritaires par rapport à ce qui se trouve dans le magasin de données.) Comme indiqué dans l’introduction de cette section, si vous ne codez rien pour la gestion des accès concurrentiels, ceci se produit automatiquement.
- Vous pouvez empêcher des modifications de Jane de mise à jour dans la base de données. En règle générale, vous affiche un message d’erreur, démontrer l’état actuel des données et lui permet de réappliquer ses modifications si elle souhaite toujours pour les rendre. Il s’agit alors d’un scénario *Priorité au magasin*. (Les valeurs du magasin de données sont prioritaires par rapport à celles soumises par le client.) Dans ce didacticiel, vous allez implémenter le scénario Priorité au magasin. Cette méthode garantit qu’aucune modification n’est remplacée sans qu’un utilisateur soit averti de ce qui se passe.

### <a name="detecting-concurrency-conflicts"></a>Détection des conflits d’accès concurrentiel

Vous pouvez résoudre les conflits en gérant [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) exceptions Entity Framework levées par. Pour savoir quand lever ces exceptions, Entity Framework doit être en mesure de détecter les conflits. Par conséquent, vous devez configurer de façon appropriée la base de données et le modèle de données. Voici quelques options pour l’activation de la détection des conflits :

- Dans la table de base de données, incluez une colonne de suivi qui peut être utilisée pour déterminer quand une ligne a été modifiée. Vous pouvez ensuite configurer Entity Framework pour inclure cette colonne dans la `Where` clause SQL `Update` ou `Delete` commandes.

    Le type de données de la colonne de suivi est généralement [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). Le [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) valeur est un nombre séquentiel qui est incrémenté chaque fois que la ligne est mise à jour. Dans un `Update` ou `Delete` commande, le `Where` clause inclut la valeur d’origine de la colonne de suivi (la version de ligne d’origine). Si la ligne en cours de mise à jour a été modifiée par un autre utilisateur, la valeur dans le `rowversion` colonne est différente de la valeur d’origine, afin que la `Update` ou `Delete` instruction ne peut pas trouver la ligne mise à jour en raison de la `Where` clause. Quand Entity Framework trouve qu’aucune ligne n’ont été mis à jour par le `Update` ou `Delete` commande (autrement dit, lorsque le nombre de lignes affectées est égal à zéro), il interprète ceci comme un conflit d’accès concurrentiel.
- Configurer Entity Framework pour inclure les valeurs d’origine de chaque colonne dans la table dans le `Where` clause de `Update` et `Delete` commandes.

    Comme dans la première option, si quelque chose dans la ligne a changé depuis la ligne a été lu tout d’abord, le `Where` clause ne retourne pas une ligne à mettre à jour, que l’Entity Framework interprète comme un conflit d’accès concurrentiel. Pour les tables de base de données qui ont beaucoup de colonnes, cette approche peut provoquer très volumineux `Where` clauses et peut nécessiter la gestion de grandes quantités d’état. Comme indiqué précédemment, la gestion de grandes quantités d’états peut affecter les performances de l’application. Par conséquent, cette approche n’est généralement pas recommandée et n’est pas la méthode utilisée dans ce didacticiel.

    Si vous ne souhaitez pas implémenter cette approche à l’accès concurrentiel, vous devez marquer toutes les propriétés de clé non primaire dans l’entité que vous souhaitez suivre les accès concurrentiels en ajoutant le [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) attribut. Que modification permet à Entity Framework à inclure toutes les colonnes dans le code SQL `WHERE` clause de `UPDATE` instructions.

Dans le reste de ce didacticiel, vous ajouterez un [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) suivi de propriété pour le `Department` entité, créer un contrôleur et des vues et de test pour vérifier que tout fonctionne correctement.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Ajouter une propriété de l’accès concurrentiel optimiste à l’entité Department

Dans *Models\Department.cs*, ajoutez une propriété de suivi nommée `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

Le [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) attribut spécifie que cette colonne doit être incluse dans le `Where` clause de `Update` et `Delete` commandes envoyées à la base de données. L’attribut est appelé [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) , car les versions précédentes de SQL Server utilisaient SQL [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) type de données avant le SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) remplacé. Le type .net pour [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) est un tableau d’octets.

Si vous préférez utiliser l’API fluent, vous pouvez utiliser la [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) méthode pour spécifier la propriété de suivi, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

En ajoutant une propriété, vous avez changé le modèle de base de données et vous devez donc effectuer une autre migration. Dans la console du Gestionnaire de package, entrez les commandes suivantes :

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-the-department-controller"></a>Modifier le contrôleur de service

Dans *Controllers\DepartmentController.cs*, ajoutez un `using` instruction :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Dans le *DepartmentController.cs* , modifiez les quatre occurrences de « LastName » en « FullName » afin que les listes déroulantes d’administrateur service contienne le nom complet de l’instructeur plutôt que simplement le nom de famille.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Remplacez le code existant pour le `HttpPost` `Edit` méthode avec le code suivant :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Si la méthode `FindAsync` retourne null, c’est que le département a été supprimé par un autre utilisateur. Le code illustré utilise les valeurs de formulaire publiées pour créer une entité de service afin que la page Edit puisse être réaffichée avec un message d’erreur. Vous pouvez aussi ne pas recréer l’entité Department si vous affichez seulement un message d’erreur sans réafficher les champs du département.

La vue stocke la version d’origine `RowVersion` valeur dans un champ masqué et la méthode reçoit dans le `rowVersion` paramètre. Avant d’appeler `SaveChanges`, vous devez placer la valeur d’origine de la propriété `RowVersion` dans la collection `OriginalValues` pour l’entité. Quand Entity Framework crée ensuite une instance SQL `UPDATE` commande, que la commande doit contenir un `WHERE` clause qui recherche une ligne qui a l’original `RowVersion` valeur.

Si aucune ligne n’est affectée par le `UPDATE` commande (aucune ligne ne contient la version d’origine `RowVersion` valeur), Entity Framework lève une `DbUpdateConcurrencyException` exception et le code dans le `catch` bloc obtient affecté `Department` entité à partir de l’exception objet.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Cet objet a les nouvelles valeurs entrées par l’utilisateur dans son `Entity` propriété et vous pouvez obtenir les valeurs lues à partir de la base de données en appelant le `GetDatabaseValues` (méthode).

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Le `GetDatabaseValues` méthode retourne null si quelqu'un a supprimé la ligne de la base de données ; sinon, vous devez effectuer un cast de l’objet retourné à la `Department` classe afin d’accéder à la `Department` propriétés. (Étant donné que vous avez déjà activé pour la suppression, `databaseEntry` aura la valeur null uniquement si le service a été supprimé après `FindAsync` s’exécute et avant `SaveChanges` s’exécute.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Ensuite, le code ajoute un message d’erreur personnalisé pour chaque colonne qui comporte des valeurs de la base de données différents de ce que l’utilisateur a entré sur la page de modification :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Un message d’erreur plu explique ce qui est arrivé et ce qu’il faut faire :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Enfin, le code définit le `RowVersion` valeur de la `Department` objet à la nouvelle valeur récupérée à partir de la base de données. Cette nouvelle valeur de `RowVersion` est stockée dans le champ masqué quand la page Edit est réaffichée et, la prochaine fois que l’utilisateur clique sur **Save**, seules les erreurs d’accès concurrentiel qui se produisent depuis le réaffichage de la page Edit sont interceptées.

Dans *Views\Department\Edit.cshtml*, ajoutez un champ masqué pour enregistrer le `RowVersion` valeur de propriété, immédiatement après le champ masqué pour la `DepartmentID` propriété :

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="testing-optimistic-concurrency-handling"></a>Test de gestion d’accès concurrentiel optimiste

Exécutez le site et cliquez sur **départements**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Bouton droit sur le **modifier** lien hypertexte pour le département « English » et sélectionnez **ouvert dans un nouvel onglet,** puis cliquez sur le **modifier** lien hypertexte pour le département « English ». Les deux onglets affichent les mêmes informations.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Changez un champ sous le premier onglet du navigateur, puis cliquez sur **Save**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Le navigateur affiche la page Index avec la valeur modifiée.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Modifier un champ dans le deuxième onglet du navigateur, cliquez sur **enregistrer**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Cliquez sur **enregistrer** dans le deuxième onglet du navigateur. Vous voyez un message d’erreur :

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Cliquez à nouveau sur **Save**. La valeur que vous avez entré dans le deuxième onglet du navigateur est enregistrée en même temps que la valeur d’origine des données que vous avez modifié dans le navigateur en premier. Vous voyez les valeurs enregistrées quand la page Index apparaît.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>La mise à jour de la Page Delete

Pour la page Delete, Entity Framework détecte les conflits d’accès concurrentiel provoqués par un autre utilisateur qui modifie le service de façon similaire. Lorsque le `HttpGet` `Delete` méthode affiche la vue de confirmation, la vue inclut la version d’origine `RowVersion` valeur dans un champ masqué. Cette valeur est ensuite disponible pour le `HttpPost` `Delete` méthode est appelée lorsque l’utilisateur confirme la suppression. Quand Entity Framework crée le SQL `DELETE` commande, il inclut un `WHERE` clause avec l’original `RowVersion` valeur. Si les résultats de la commande dans des lignes nulles affectés (c'est-à-dire la ligne a été modifiée après l’affichage de la page de confirmation de suppression), une exception d’accès concurrentiel est levée et le `HttpGet Delete` méthode est appelée avec un indicateur d’erreur défini sur `true` pour réafficher le page de confirmation avec un message d’erreur. Il est également possible qu’aucune ligne ont été affectés, car la ligne a été supprimée par un autre utilisateur, donc dans ce cas, un message d’erreur différent est affiché.

Dans *DepartmentController.cs*, remplacez le `HttpGet` `Delete` méthode avec le code suivant :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

La méthode accepte un paramètre facultatif qui indique si la page est réaffichée après une erreur d’accès concurrentiel. Si cet indicateur est `true`, un message d’erreur est envoyé à la vue en utilisant un `ViewBag` propriété.

Remplacez le code dans le `HttpPost` `Delete` (méthode) (nommé `DeleteConfirmed`) avec le code suivant :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Dans le code du modèle généré automatiquement que vous venez de remplacer, cette méthode n’acceptait qu’un seul ID d’enregistrement :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Vous avez modifié ce paramètre pour un `Department` instance d’entité créé par le binder de modèle. Cela vous donne accès à la `RowVersion` valeur de propriété en plus de la clé d’enregistrement.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Vous avez également changé le nom de la méthode d’action de `DeleteConfirmed` en `Delete`. Le code structuré nommé le `HttpPost` `Delete` méthode `DeleteConfirmed` afin de donner la `HttpPost` méthode une signature unique. (Le CLR nécessite des méthodes surchargées doivent avoir des paramètres de méthode différents.) Maintenant que les signatures sont uniques, vous pouvez cantonner à la convention MVC et utiliser le même nom pour le `HttpPost` et `HttpGet` supprimer les méthodes.

Si une erreur d’accès concurrentiel est interceptée, le code réaffiche la page de confirmation de suppression et fournit un indicateur indiquant qu’elle doit afficher un message d’erreur d’accès concurrentiel.

Dans *Views\Department\Delete.cshtml*, remplacez le code généré automatiquement par le code suivant qui ajoute un champ de message d’erreur et les champs masqués pour les propriétés DepartmentID et RowVersion. Les modifications apparaissent en surbrillance.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

Ce code ajoute un message d’erreur entre la `h2` et `h3` des en-têtes :

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Il remplace `LastName` avec `FullName` dans le `Administrator` champ :

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Enfin, il ajoute les champs masqués pour les `DepartmentID` et `RowVersion` propriétés après la `Html.BeginForm` instruction :

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

Exécutez la page Index des départements. Bouton droit sur le **supprimer** lien hypertexte pour le département « English » et sélectionnez **ouvert dans un nouvel onglet,** puis, dans le premier onglet, cliquez sur le **modifier** lien hypertexte pour le département « English ».

Dans la première fenêtre, modifiez l’une des valeurs, puis cliquez sur **enregistrer** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

La page d’Index a confirmé la modification.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Sous le deuxième onglet, cliquez sur **Delete**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Vous voyez le message d’erreur d’accès concurrentiel et les valeurs du département sont actualisées avec ce qui est actuellement dans la base de données.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Si vous recliquez sur **Delete**, vous êtes redirigé vers la page Index, qui montre que le département a été supprimé.

## <a name="summary"></a>Récapitulatif

Ceci termine l’introduction à la gestion des conflits d’accès concurrentiel. Pour plus d’informations sur les autres méthodes pour gérer les différents scénarios de concurrence, consultez [des modèles d’accès concurrentiel optimiste](https://msdn.microsoft.com/data/jj592904) et [travaillez avec des valeurs de propriété](https://msdn.microsoft.com/data/jj592677) sur MSDN. Le didacticiel suivant montre comment implémenter l’héritage table par hiérarchie pour le `Instructor` et `Student` entités.

Vous trouverez des liens vers d’autres ressources Entity Framework dans le [accès aux données ASP.NET - ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Suivant](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
