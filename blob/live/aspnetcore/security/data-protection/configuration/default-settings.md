---
title: "Gestion de clés de Protection des données et la durée de vie dans ASP.NET Core"
author: rick-anderson
description: "En savoir plus sur la gestion de clés de Protection des données et la durée de vie dans ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 0993c68e37944a3aad863b98f92fe0140cfb746d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a><span data-ttu-id="32181-103">Gestion de clés de Protection des données et la durée de vie dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="32181-103">Data Protection key management and lifetime in ASP.NET Core</span></span>

<span data-ttu-id="32181-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="32181-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="key-management"></a><span data-ttu-id="32181-105">Gestion de clés</span><span class="sxs-lookup"><span data-stu-id="32181-105">Key management</span></span>

<span data-ttu-id="32181-106">L’application tente de détecter son environnement d’exploitation et de gérer la configuration de la clé sur son propre.</span><span class="sxs-lookup"><span data-stu-id="32181-106">The app attempts to detect its operational environment and handle key configuration on its own.</span></span>

1. <span data-ttu-id="32181-107">Si l’application est hébergée dans [applications Azure](https://azure.microsoft.com/services/app-service/), les clés sont rendues persistantes dans le *%HOME%\ASP.NET\DataProtection-Keys* dossier.</span><span class="sxs-lookup"><span data-stu-id="32181-107">If the app is hosted in [Azure Apps](https://azure.microsoft.com/services/app-service/), keys are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="32181-108">Ce dossier est sauvegardé par le stockage réseau et est synchronisé sur tous les ordinateurs hébergeant l’application.</span><span class="sxs-lookup"><span data-stu-id="32181-108">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span>
   * <span data-ttu-id="32181-109">Les clés ne sont pas protégées au repos.</span><span class="sxs-lookup"><span data-stu-id="32181-109">Keys aren't protected at rest.</span></span>
   * <span data-ttu-id="32181-110">Le *DataProtection-clés* dossier fournit l’anneau de clé à toutes les instances d’une application dans un emplacement de déploiement.</span><span class="sxs-lookup"><span data-stu-id="32181-110">The *DataProtection-Keys* folder supplies the key ring to all instances of an app in a single deployment slot.</span></span>
   * <span data-ttu-id="32181-111">Les emplacements de déploiement distinct, telles que les intermédiaires et de Production, ne partagent pas un anneau de clé.</span><span class="sxs-lookup"><span data-stu-id="32181-111">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span> <span data-ttu-id="32181-112">Lorsque vous échangez entre les emplacements de déploiement, par exemple le remplacement intermédiaire en Production ou à l’aide de / B test, n’importe quelle application à l’aide de la Protection des données ne pourra plus être déchiffrer les données stockées à l’aide de l’anneau de clé à l’intérieur de l’emplacement précédent.</span><span class="sxs-lookup"><span data-stu-id="32181-112">When you swap between deployment slots, for example swapping Staging to Production or using A/B testing, any app using Data Protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="32181-113">Cela conduit aux utilisateurs est enregistrés en dehors d’une application qui utilise l’authentification de cookie ASP.NET Core standard, car elle utilise la Protection des données à protéger ses cookies.</span><span class="sxs-lookup"><span data-stu-id="32181-113">This leads to users being logged out of an app that uses the standard ASP.NET Core cookie authentication, as it uses Data Protection to protect its cookies.</span></span> <span data-ttu-id="32181-114">Si vous le souhaitez, indépendant de l’emplacement de clé anneaux utilisent un fournisseur de l’anneau de clé externe, telles que le stockage Blob Azure, Azure Key Vault, un magasin SQL, ou le cache Redis.</span><span class="sxs-lookup"><span data-stu-id="32181-114">If you desire slot-independent key rings, use an external key ring provider, such as Azure Blob Storage, Azure Key Vault, a SQL store, or Redis cache.</span></span>

1. <span data-ttu-id="32181-115">Si le profil utilisateur est disponible, les clés sont rendues persistantes dans le *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* dossier.</span><span class="sxs-lookup"><span data-stu-id="32181-115">If the user profile is available, keys are persisted to the *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="32181-116">Si le système d’exploitation est Windows, les clés sont chiffrées au repos à l’aide de DPAPI.</span><span class="sxs-lookup"><span data-stu-id="32181-116">If the operating system is Windows, the keys are encrypted at rest using DPAPI.</span></span>

1. <span data-ttu-id="32181-117">Si l’application est hébergée dans IIS, les clés sont conservées dans le Registre HKLM dans une clé de Registre spéciale qui est l’ACL sur uniquement pour le compte de processus de travail.</span><span class="sxs-lookup"><span data-stu-id="32181-117">If the app is hosted in IIS, keys are persisted to the HKLM registry in a special registry key that is ACLed only to the worker process account.</span></span> <span data-ttu-id="32181-118">Les clés sont chiffrées au repos à l’aide de DPAPI.</span><span class="sxs-lookup"><span data-stu-id="32181-118">Keys are encrypted at rest using DPAPI.</span></span>

1. <span data-ttu-id="32181-119">Si aucune de ces conditions, les clés ne sont pas rendues persistantes en dehors du processus en cours.</span><span class="sxs-lookup"><span data-stu-id="32181-119">If none of these conditions match, keys aren't persisted outside of the current process.</span></span> <span data-ttu-id="32181-120">Lorsque le processus s’arrête, tous les généré les clés sont perdues.</span><span class="sxs-lookup"><span data-stu-id="32181-120">When the process shuts down, all generated keys are lost.</span></span>

<span data-ttu-id="32181-121">Le développeur est toujours dans un contrôle total et peut remplacer comment et où les clés sont stockées.</span><span class="sxs-lookup"><span data-stu-id="32181-121">The developer is always in full control and can override how and where keys are stored.</span></span> <span data-ttu-id="32181-122">Les trois premières options ci-dessus doivent fournir des valeurs par défaut corrects pour la plupart des applications similaires à la manière dont ASP.NET  **\<machineKey >** les routines de la génération automatique a fonctionné dans le passé.</span><span class="sxs-lookup"><span data-stu-id="32181-122">The first three options above should provide good defaults for most apps similar to how the ASP.NET **\<machineKey>** auto-generation routines worked in the past.</span></span> <span data-ttu-id="32181-123">L’option de secours, finale est le seul scénario qui nécessite le développeur à spécifier [configuration](xref:security/data-protection/configuration/overview) initial s’ils veulent persistance des clés, mais ce basculement se produit uniquement dans les rares cas.</span><span class="sxs-lookup"><span data-stu-id="32181-123">The final, fallback option is the only scenario that requires the developer to specify [configuration](xref:security/data-protection/configuration/overview) upfront if they want key persistence, but this fallback only occurs in rare situations.</span></span>

<span data-ttu-id="32181-124">Lorsque vous hébergez dans un conteneur Docker, clés doivent être persistante dans un dossier qui est un volume Docker (un volume partagé ou un volume monté hôte qui persiste au-delà de la durée de vie du conteneur) ou dans un fournisseur externe, tel que [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ou [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="32181-124">When hosting in a Docker container, keys should be persisted in a folder that's a Docker volume (a shared volume or a host-mounted volume that persists beyond the container's lifetime) or in an external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span> <span data-ttu-id="32181-125">Un fournisseur externe est également utile dans les scénarios de batterie de serveurs web si les applications ne peut pas accéder à un volume partagé de réseau (voir [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) pour plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="32181-125">An external provider is also useful in web farm scenarios if apps can't access a shared network volume (see [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) for more information).</span></span>

> [!WARNING]
> <span data-ttu-id="32181-126">Si le développeur remplace les règles décrites ci-dessus et le système de Protection des données dans un dépôt de clé spécifique de points, un chiffrement de clés à l’arrêt automatique est désactivé.</span><span class="sxs-lookup"><span data-stu-id="32181-126">If the developer overrides the rules outlined above and points the Data Protection system at a specific key repository, automatic encryption of keys at rest is disabled.</span></span> <span data-ttu-id="32181-127">Une protection à l’arrêt peut être réactivée via [configuration](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="32181-127">At-rest protection can be re-enabled via [configuration](xref:security/data-protection/configuration/overview).</span></span>

## <a name="key-lifetime"></a><span data-ttu-id="32181-128">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="32181-128">Key lifetime</span></span>

<span data-ttu-id="32181-129">Par défaut, les clés ont une durée de vie de 90 jours.</span><span class="sxs-lookup"><span data-stu-id="32181-129">Keys have a 90-day lifetime by default.</span></span> <span data-ttu-id="32181-130">Lorsqu’une clé expire, l’application génère une nouvelle clé et définit la nouvelle clé comme clé active automatiquement.</span><span class="sxs-lookup"><span data-stu-id="32181-130">When a key expires, the app automatically generates a new key and sets the new key as the active key.</span></span> <span data-ttu-id="32181-131">Tant que clés supprimées restent sur le système, votre application peut déchiffrer les données protégées avec eux.</span><span class="sxs-lookup"><span data-stu-id="32181-131">As long as retired keys remain on the system, your app can decrypt any data protected with them.</span></span> <span data-ttu-id="32181-132">Consultez [gestion de clés](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="32181-132">See [key management](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) for more information.</span></span>

## <a name="default-algorithms"></a><span data-ttu-id="32181-133">Algorithmes par défaut</span><span class="sxs-lookup"><span data-stu-id="32181-133">Default algorithms</span></span>

<span data-ttu-id="32181-134">L’algorithme de protection de charge utile par défaut utilisé est AES-256-CBC pour la confidentialité et HMACSHA256 authenticité.</span><span class="sxs-lookup"><span data-stu-id="32181-134">The default payload protection algorithm used is AES-256-CBC for confidentiality and HMACSHA256 for authenticity.</span></span> <span data-ttu-id="32181-135">Une clé principale de 512 bits, modifiée tous les 90 jours, est utilisée pour dériver les deux sous-clés utilisés pour ces algorithmes sur une base par charge.</span><span class="sxs-lookup"><span data-stu-id="32181-135">A 512-bit master key, changed every 90 days, is used to derive the two sub-keys used for these algorithms on a per-payload basis.</span></span> <span data-ttu-id="32181-136">Consultez [des sous-clés de dérivation](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="32181-136">See [subkey derivation](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) for more information.</span></span>

## <a name="see-also"></a><span data-ttu-id="32181-137">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="32181-137">See also</span></span>

* [<span data-ttu-id="32181-138">Extensibilité de la gestion de clés</span><span class="sxs-lookup"><span data-stu-id="32181-138">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)