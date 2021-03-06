---
title: Authentification à l’aide de fournisseurs externes (Facebook, Google et autres) dans ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre comment générer une application ASP.NET Core 2.x à l’aide d’OAuth 2.0 avec des fournisseurs d’authentification externes.
ms.author: riande
ms.date: 11/01/2016
uid: security/authentication/social/index
ms.openlocfilehash: b2176ef40faaee02c5ef53dbb92a802212e58e8b
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063323"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a>Authentification à l’aide de fournisseurs externes (Facebook, Google et autres) dans ASP.NET Core

Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce didacticiel montre comment créer une application ASP.NET Core 2.x qui permet aux utilisateurs de se connecter à l’aide d’OAuth 2.0 avec des informations d’identification provenant de fournisseurs d’authentification externes.

Les fournisseurs [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins) et [Microsoft](xref:security/authentication/microsoft-logins) sont traités dans les sections qui suivent. D’autres fournisseurs sont disponibles dans des packages tiers, comme [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) et [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Icônes de réseau social pour Facebook, Twitter, Google plus et Windows](index/_static/social.png)

Permettre aux utilisateurs de se connecter avec leurs informations d’identification existantes est pratique pour eux et transfère une grande partie de la complexité de la gestion du processus de connexion sur un tiers. Pour obtenir des exemples de la façon dont les connexions des réseaux sociaux peuvent amener du trafic et des conversions de clients, consultez les études de cas par [Facebook](https://www.facebook.com/unsupportedbrowser) et [Twitter](https://dev.twitter.com/resources/case-studies).

Remarque : Les packages présentés font abstraction d’une grande partie de la complexité du flux d’authentification OAuth, mais en comprendre les détails peut devenir nécessaire pour la résolution des problèmes. De nombreuses ressources sont disponibles. Vous pouvez par exemple consulter [Introduction à OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) ou [Présentation d’OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/). Certains problèmes peuvent être résolus en examinant le [code source d’ASP.NET Core pour les packages de fournisseur](https://github.com/aspnet/Security/tree/master/src).

## <a name="create-a-new-aspnet-core-project"></a>Créer un projet ASP.NET Core

* Dans Visual Studio 2017, créez un projet à partir de la page de démarrage, ou via **Fichier > Nouveau > Projet**.

* Sélectionnez le modèle **Application web ASP.NET Core** disponible dans la catégorie **Visual C# > .NET Core** :

![Boîte de dialogue Nouveau projet](index/_static/new-project.png)

* Appuyez sur **Application Web** et vérifiez que **Authentification** est défini sur **Comptes d’utilisateur individuels** :

![Boîte de dialogue Nouvelle application web](index/_static/select-project.png)

Remarque : Ce didacticiel s’applique à la version ASP.NET Core 2.0 SDK, qui peut être sélectionnée en haut de l’Assistant.

## <a name="apply-migrations"></a>Appliquer des migrations

* Exécutez l’application et sélectionnez le lien **Se connecter**.
* Sélectionnez le lien **S’inscrire comme nouvel utilisateur**.
* Entrez l’adresse e-mail et le mot de passe du nouveau compte, puis sélectionnez **S’inscrire**.
* Suivez les instructions pour appliquer des migrations.

## <a name="require-ssl"></a>Exiger SSL

OAuth 2.0 nécessite l’utilisation de SSL pour l’authentification avec le protocole HTTPS.

Remarque : Les projets créés à partir des modèles de projet **Application web** ou **API web** pour ASP.NET Core 2.x sont configurés automatiquement pour activer SSL, et pour se lancer avec des URL HTTPS si l’option **Comptes d’utilisateur individuels** a été sélectionnée dans la boîte de dialogue **Modifier l’authentification** dans l’Assistant Projet, comme indiqué ci-dessus.

* Imposez l’utilisation de SSL sur votre site en suivant les étapes de la rubrique [Imposer SSL dans une application ASP.NET Core](xref:security/enforcing-ssl).

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>Utilisez SecretManager pour stocker les jetons affectés par les fournisseurs de connexion

Les fournisseurs de connexion de réseaux sociaux affectent des jetons **ID d’application** et **Secret de l’application** lors du processus d’inscription. Les noms de jeton exacts varient selon le fournisseur. Ces jetons représentent les informations d’identification que votre application utilise pour accéder à son API. Les jetons constituent les « secrets » qui peuvent être liés à la configuration de votre application à l’aide de [Secret Manager](xref:security/app-secrets#secret-manager). Secret Manager est une alternative plus sécurisée pour stocker les jetons dans un fichier de configuration, tel que *appsettings.json*.

> [!IMPORTANT]
> Secret Manager est uniquement réservé au développement. Vous pouvez stocker et protéger les secrets de test et de production Azure avec le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration).

Suivez les étapes de la rubrique [Stockage sécurisé des secrets d’application lors du développement dans ASP.NET Core](xref:security/app-secrets) pour stocker les jetons affectés par chaque fournisseur de connexion ci-dessous.

## <a name="setup-login-providers-required-by-your-application"></a>Configurer les fournisseurs de connexion nécessaires à votre application

Utilisez les rubriques suivantes pour configurer votre application pour utiliser ces différents fournisseurs :

* Instructions pour [Facebook](xref:security/authentication/facebook-logins)
* Instructions pour [Twitter](xref:security/authentication/twitter-logins)
* Instructions pour [Google](xref:security/authentication/google-logins)
* Instructions pour [Microsoft](xref:security/authentication/microsoft-logins)
* Instructions pour les [autres fournisseurs](xref:security/authentication/otherlogins)

[!INCLUDE[](~/includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a>Définition facultative d’un mot de passe

Quand vous vous inscrivez auprès d’un fournisseur de connexion externe, vous n’avez pas de mot de passe inscrit auprès de l’application. Ceci vous évite de devoir créer et mémoriser un mot de passe pour le site, mais vous rend aussi dépendant du fournisseur de connexion externe. Si le fournisseur de connexion externe est indisponible, vous ne pouvez pas vous connecter au site web.

Pour créer un mot de passe et vous connecter à l’aide de l’e-mail que vous avez défini lors du processus de connexion avec des fournisseurs externes :

* Appuyez sur le lien **Bonjour &lt;alias d’e-mail&gt;** en haut à droite pour accéder à la vue **Gérer**.

![Vue Gérer de l’application web](index/_static/pass1a.png)

* Appuyez sur **Créer**.

![Page Définir votre mot de passe](index/_static/pass2a.png)

* Définissez un mot de passe valide à utiliser pour vous connecter avec votre e-mail.

## <a name="next-steps"></a>Étapes suivantes

* Cet article a présenté l’authentification externe et expliqué les prérequis nécessaires pour ajouter des connexions externes à votre application ASP.NET Core.

* Référencez les pages spécifiques au fournisseur pour configurer les connexions pour les fournisseurs nécessaires à votre application.
