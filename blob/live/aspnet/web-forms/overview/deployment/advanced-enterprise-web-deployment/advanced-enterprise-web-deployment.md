---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: "Avancé de déploiement Web d’entreprise | Documents Microsoft"
author: jrjlee
description: "Ce didacticiel vous indiquera comment effectuer diverses tâches requises ou souhaitable dans de nombreux scénarios de déploiement d’entreprise. Pour un translati italien..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: c3cb7f63cf7c0246a0c4da6038a65a6ac43a7b59
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="advanced-enterprise-web-deployment"></a><span data-ttu-id="1c2f2-104">Déploiement de Web avancées d’entreprise</span><span class="sxs-lookup"><span data-stu-id="1c2f2-104">Advanced Enterprise Web Deployment</span></span>
====================
<span data-ttu-id="1c2f2-105">par [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="1c2f2-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="1c2f2-106">Télécharger le PDF</span><span class="sxs-lookup"><span data-stu-id="1c2f2-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="1c2f2-107">Ce didacticiel vous indiquera comment effectuer diverses tâches requises ou souhaitable dans de nombreux scénarios de déploiement d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-107">This tutorial will show you how to perform various tasks that are required or desirable in a lot of enterprise deployment scenarios.</span></span>
> 
> <span data-ttu-id="1c2f2-108">Pour obtenir une traduction italienne de ces didacticiels, visitez [http://www.lucamorelli.it](http://www.lucamorelli.it).</span><span class="sxs-lookup"><span data-stu-id="1c2f2-108">For an Italian translation of these tutorials, visit [http://www.lucamorelli.it](http://www.lucamorelli.it).</span></span>


<span data-ttu-id="1c2f2-109">Cela constitue une partie d’une série de didacticiels basées sur les spécifications de déploiement d’entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un exemple de solution l’a & #x 2014 ; le [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solution & #x 2014 ; pour représenter une application web avec un niveau réaliste de complexité, y compris une application ASP.NET MVC 3, Windows Service de communication Foundation (WCF) et un projet de base de données.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-109">This forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="1c2f2-110">La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet de fractionnement décrite dans [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md), dans lequel le processus de génération est contrôlé par deux fichiers de & projet #x 2014 ; un contenant les instructions qui s’appliquent à chaque environnement de destination et celui qui contient les paramètres de génération et de déploiement spécifiques à l’environnement de génération.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-110">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="1c2f2-111">Au moment de la génération, le fichier de projet spécifique à un environnement est fusionné dans le fichier de projet d’indépendant de l’environnement pour former un ensemble complet d’instructions de génération.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-111">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="1c2f2-112">Vue d’ensemble du scénario</span><span class="sxs-lookup"><span data-stu-id="1c2f2-112">Scenario Overview</span></span>

<span data-ttu-id="1c2f2-113">Le scénario de haut niveau pour ces didacticiels est décrit dans [déploiement Web d’entreprise : vue d’ensemble du scénario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1c2f2-113">The high-level scenario for these tutorials is described in [Enterprise Web Deployment: Scenario Overview](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).</span></span> <span data-ttu-id="1c2f2-114">Nous vous recommandons de consulter cette rubrique avant de commencer ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-114">We recommend that you review this topic before you get started on this tutorial.</span></span>

## <a name="how-to-use-this-tutorial"></a><span data-ttu-id="1c2f2-115">Comment utiliser ce didacticiel</span><span class="sxs-lookup"><span data-stu-id="1c2f2-115">How to Use This Tutorial</span></span>

- <span data-ttu-id="1c2f2-116">Chacune des rubriques de ce didacticiel est autonome et adresse un défi particulier ou un problème qui se produit dans les scénarios de déploiement d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-116">Each of the topics in this tutorial is self-contained and addresses a particular challenge or problem that occurs in enterprise deployment scenarios.</span></span> <span data-ttu-id="1c2f2-117">Vous n’avez pas besoin de parcourir les rubriques suivantes dans un ordre particulier.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-117">You don't need to work through these topics in any particular order.</span></span> <span data-ttu-id="1c2f2-118">Toutefois, ce didacticiel décrit certaines des tâches avancées.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-118">However, this tutorial covers some advanced tasks.</span></span> <span data-ttu-id="1c2f2-119">Par conséquent, vous devez vous familiariser avec les concepts et techniques qui le [déploiement Web de l’entreprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) didacticiel couvre afin d’obtenir le meilleur parti de ce contenu.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-119">As such, you should familiarize yourself with the concepts and techniques that the [Web Deployment in the Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) tutorial covers in order to gain the most benefit from this content.</span></span>
- <span data-ttu-id="1c2f2-120">Ce didacticiel inclut les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="1c2f2-120">This tutorial includes these topics:</span></span>
- <span data-ttu-id="1c2f2-121">[Exécution d’un déploiement « Que se passe-t-il si »](performing-a-what-if-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="1c2f2-121">[Performing a "What If" Deployment](performing-a-what-if-deployment.md).</span></span> <span data-ttu-id="1c2f2-122">Dans de nombreux scénarios, vous souhaiterez déterminer l’impact d’un déploiement proposé sur un environnement cible ou le contenu existant avant de procéder réellement des modifications.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-122">In a lot of scenarios, you'll want to determine the impact of a proposed deployment on a target environment or any existing content before you actually make any changes.</span></span> <span data-ttu-id="1c2f2-123">Cette rubrique décrit comment vous pouvez exécuter un déploiement « que se passe-t-il si » pour générer des fichiers journaux et les scripts de mise à jour de base de données comme si vous aviez déployé contenu à un environnement cible, sans réellement apporter de modifications.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-123">This topic describes how you can run a "what if" deployment to generate log files and database update scripts as if you had deployed content to a target environment, without actually making any changes.</span></span> <span data-ttu-id="1c2f2-124">Analyse de ces ressources peut vous aider à repérer les problèmes potentiels avant un déploiement.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-124">Analyzing these resources can help you to spot any potential problems in advance of a live deployment.</span></span>
- <span data-ttu-id="1c2f2-125">[Personnalisation des déploiements de base de données pour plusieurs environnements](customizing-database-deployments-for-multiple-environments.md).</span><span class="sxs-lookup"><span data-stu-id="1c2f2-125">[Customizing Database Deployments for Multiple Environments](customizing-database-deployments-for-multiple-environments.md).</span></span> <span data-ttu-id="1c2f2-126">Lorsque vous déployez un projet de base de données vers plusieurs destinations, vous souhaiterez souvent personnaliser les propriétés de déploiement pour chaque environnement cible.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-126">When you deploy a database project to multiple destinations, you'll often want to customize the deployment properties for each target environment.</span></span> <span data-ttu-id="1c2f2-127">Par exemple, dans les environnements de test vous serez généralement recréer la base de données sur chaque déploiement, alors que dans les environnements de production ou de vous être beaucoup plus susceptibles d’effectuer des mises à jour incrémentielles pour préserver vos données.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-127">For example, in test environments you'd typically recreate the database on every deployment, whereas in staging or production environments you'd be a lot more likely to make incremental updates to preserve your data.</span></span> <span data-ttu-id="1c2f2-128">Cette rubrique décrit comment vous pouvez incorporer ces modifications de propriété dans votre logique de déploiement en créant un fichier de configuration (.sqldeployment) spécifiques à l’environnement de déploiement pour chaque environnement cible.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-128">This topic describes how you can incorporate these property changes into your deployment logic by creating an environment-specific deployment configuration (.sqldeployment) file for each target environment.</span></span>
- <span data-ttu-id="1c2f2-129">Appartenances aux rôles de base de données à déployer dans des environnements de Test.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-129">Deploying Database Role Memberships to Test Environments.</span></span> <span data-ttu-id="1c2f2-130">Lorsque vous recréez une base de données sur chaque #x 2014 et le déploiement ; par exemple, dans le cadre d’une build d’intégration continue (CI) et que vous déployez sur un environnement de test #x 2014 ; vous devez généralement configurer les appartenances aux rôles de base de données chaque fois.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-130">When you recreate a database on every deployment&#x2014;for example, as part of a continuous integration (CI) build and deploy to a test environment&#x2014;you'll typically need to configure database role memberships every time.</span></span> <span data-ttu-id="1c2f2-131">Par exemple, vous devez généralement accorder des autorisations à l’identité de pool d’applications associé à votre application web.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-131">For example, you'll usually need to grant permissions to the application pool identity associated with your web application.</span></span> <span data-ttu-id="1c2f2-132">Cette rubrique décrit comment vous pouvez automatiser ce processus en ajoutant un script SQL de post-déploiement pour votre logique de déploiement.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-132">This topic describes how you can automate this process by adding a post-deployment SQL script to your deployment logic.</span></span>
- <span data-ttu-id="1c2f2-133">[Déploiement de bases de données d’appartenance pour les environnements d’entreprise](deploying-membership-databases-to-enterprise-environments.md).</span><span class="sxs-lookup"><span data-stu-id="1c2f2-133">[Deploying Membership Databases to Enterprise Environments](deploying-membership-databases-to-enterprise-environments.md).</span></span> <span data-ttu-id="1c2f2-134">Bases de données d’appartenance ASP.NET ont différentes caractéristiques susceptibles de compliquer le processus de déploiement.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-134">ASP.NET membership databases have various characteristics that can complicate the deployment process.</span></span> <span data-ttu-id="1c2f2-135">Par exemple, un déploiement de schéma uniquement sera laisse la base de données dans un état non opérationnel.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-135">For example, a schema-only deployment will leave the database in a non-operational state.</span></span> <span data-ttu-id="1c2f2-136">Dans la plupart des scénarios, il est préférable de créer une base de données d’appartenance directement dans chaque environnement de destination.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-136">In most scenarios, it's preferable to create a membership database directly in each destination environment.</span></span> <span data-ttu-id="1c2f2-137">Toutefois, si vous n’avez pas à déployer une base de données d’appartenance, cette rubrique décrit certaines des méthodes que vous pouvez utiliser pour relever les défis inhérents.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-137">However, if you do have to deploy a membership database, this topic describes some of the approaches you can use to meet the inherent challenges.</span></span>
- <span data-ttu-id="1c2f2-138">[Exclusion de fichiers et dossiers de déploiement](excluding-files-and-folders-from-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="1c2f2-138">[Excluding Files and Folders from Deployment](excluding-files-and-folders-from-deployment.md).</span></span> <span data-ttu-id="1c2f2-139">Dans certains scénarios, que vous souhaitez personnaliser le contenu de votre package web dans les environnements de destination spécifique.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-139">In some scenarios, you'll want to tailor the contents of your web package to specific destination environments.</span></span> <span data-ttu-id="1c2f2-140">Par exemple, vous souhaiterez incluent les versions complètes de bibliothèques JavaScript lorsque vous déployez dans un environnement de test, prennent en charge le débogage côté client, mais d’utiliser des versions réduites des bibliothèques lorsque vous déployez dans un environnement intermédiaire ou de production.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-140">For example, you might want to include full versions of JavaScript libraries when you deploy to a test environment, to support client-side debugging, but use minified versions of the libraries when you deploy to a staging or production environment.</span></span> <span data-ttu-id="1c2f2-141">Cette rubrique décrit comment vous pouvez exclure certains fichiers et dossiers à partir du processus de création de package.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-141">This topic describes how you can exclude specific files and folders from the package creation process.</span></span>
- <span data-ttu-id="1c2f2-142">[Déploiement des Applications Web prise en mode hors connexion avec Web](taking-web-applications-offline-with-web-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="1c2f2-142">[Taking Web Applications Offline with Web Deploy](taking-web-applications-offline-with-web-deploy.md).</span></span> <span data-ttu-id="1c2f2-143">Lorsque vous déployez des solutions dans un environnement intermédiaire ou de production, vous souhaiterez souvent prendre vos applications web en mode hors connexion pendant la durée du processus de déploiement.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-143">When you deploy solutions to a staging or production environment, you'll often want to take your web applications offline for the duration of the deployment process.</span></span> <span data-ttu-id="1c2f2-144">Cette rubrique décrit comment vous pouvez ajouter un *application\_offline.htm* de fichiers à votre application web au début du processus de déploiement et le supprimer à la fin.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-144">This topic describes how you can add an *App\_offline.htm* file to your web application at the start of the deployment process and remove it at the end.</span></span> <span data-ttu-id="1c2f2-145">Alors que le *application\_offline.htm* fichier est en place, les utilisateurs qui accèdent à l’application web sont automatiquement redirigés vers le *application\_offline.htm* fichier.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-145">While the *App\_offline.htm* file is in place, any users who browse to the web application are automatically redirected to the *App\_offline.htm* file.</span></span>
- <span data-ttu-id="1c2f2-146">[En cours d’exécution de Scripts Windows PowerShell à partir de MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md).</span><span class="sxs-lookup"><span data-stu-id="1c2f2-146">[Running Windows PowerShell Scripts from MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md).</span></span> <span data-ttu-id="1c2f2-147">De nombreux scénarios de déploiement requièrent des actions de post-déploiement plus complexes, telles que l’ajout de sources d’événement personnalisés dans le Registre ou la configuration de la réplication entre des instances de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-147">Many deployment scenarios require more complex post-deployment actions, like adding custom event sources to the registry or configuring replication between SQL Server instances.</span></span> <span data-ttu-id="1c2f2-148">Ces actions sont souvent obtenues via des scripts Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-148">These actions are often accomplished through Windows PowerShell scripts.</span></span> <span data-ttu-id="1c2f2-149">Cette rubrique décrit comment exécuter des scripts Windows PowerShell à partir d’un fichier de projet Microsoft Build Engine (MSBuild) dans le cadre du processus de génération et de déploiement.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-149">This topic describes how to run Windows PowerShell scripts from a Microsoft Build Engine (MSBuild) project file as part of the build and deployment process.</span></span>
- <span data-ttu-id="1c2f2-150">[Dépannage du processus d’empaquetage](troubleshooting-the-packaging-process.md).</span><span class="sxs-lookup"><span data-stu-id="1c2f2-150">[Troubleshooting the Packaging Process](troubleshooting-the-packaging-process.md).</span></span> <span data-ttu-id="1c2f2-151">Le Pipeline de publication Web (WPP) définit une propriété MSBuild nommée **EnablePackageProcessLoggingAndAssert** que vous pouvez utiliser pour générer des informations détaillées sur le processus d’empaquetage pour les projets d’application web.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-151">The Web Publishing Pipeline (WPP) defines an MSBuild property named **EnablePackageProcessLoggingAndAssert** that you can use to generate in-depth information about the packaging process for web application projects.</span></span> <span data-ttu-id="1c2f2-152">Cette rubrique décrit ce que fait la propriété et comment l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-152">This topic describes what the property does and how to use it.</span></span>

## <a name="key-technologies"></a><span data-ttu-id="1c2f2-153">Technologies de clé</span><span class="sxs-lookup"><span data-stu-id="1c2f2-153">Key Technologies</span></span>

<span data-ttu-id="1c2f2-154">Ce didacticiel se concentre sur l’utilisation de ces produits et technologies pour prendre en charge la génération automatique et déploiement web :</span><span class="sxs-lookup"><span data-stu-id="1c2f2-154">This tutorial focuses on how to use these products and technologies to support automated build and web deployment:</span></span>

- <span data-ttu-id="1c2f2-155">Visual Studio 2010 et Team Foundation Server (TFS) 2010</span><span class="sxs-lookup"><span data-stu-id="1c2f2-155">Visual Studio 2010 and Team Foundation Server (TFS) 2010</span></span>
- <span data-ttu-id="1c2f2-156">MSBuild et Build d’équipe TFS</span><span class="sxs-lookup"><span data-stu-id="1c2f2-156">MSBuild and TFS Team Build</span></span>
- <span data-ttu-id="1c2f2-157">Internet Information Services (IIS) 7.5</span><span class="sxs-lookup"><span data-stu-id="1c2f2-157">Internet Information Services (IIS) 7.5</span></span>
- <span data-ttu-id="1c2f2-158">Outil de déploiement de Web IIS (Web Deploy) 2.1</span><span class="sxs-lookup"><span data-stu-id="1c2f2-158">IIS Web Deployment Tool (Web Deploy) 2.1</span></span>
- <span data-ttu-id="1c2f2-159">L’utilitaire de déploiement de base de données VSDBCMD.exe</span><span class="sxs-lookup"><span data-stu-id="1c2f2-159">The VSDBCMD.exe database deployment utility</span></span>

## <a name="other-tutorials-in-this-series"></a><span data-ttu-id="1c2f2-160">Autres didacticiels de cette série</span><span class="sxs-lookup"><span data-stu-id="1c2f2-160">Other Tutorials in This Series</span></span>

<span data-ttu-id="1c2f2-161">Cela fait partie d’une série de cinq didacticiels sur le déploiement web de l’échelle de l’entreprise.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-161">This forms part of a series of five tutorials on enterprise-scale web deployment.</span></span> <span data-ttu-id="1c2f2-162">Voici d’autres didacticiels dans la série :</span><span class="sxs-lookup"><span data-stu-id="1c2f2-162">These are other tutorials in the series:</span></span>

- <span data-ttu-id="1c2f2-163">[Déployer des Applications Web dans les scénarios d’entreprise](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="1c2f2-163">[Deploying Web Applications in Enterprise Scenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).</span></span> <span data-ttu-id="1c2f2-164">Ce contenu introduction fournit l’arrière-plan contextuel pour la série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-164">This introductory content provides the contextual background for the tutorial series.</span></span> <span data-ttu-id="1c2f2-165">Il décrit le scénario du didacticiel, et illustre comment les tâches et les procédures pas à pas décrites dans l’ensemble de la série s’intègrent dans un processus d’Application Lifecycle Management (ALM) plus large.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-165">It describes the tutorial scenario, and it illustrates how the tasks and walkthroughs described throughout the series fit into a broader Application Lifecycle Management (ALM) process.</span></span>
- <span data-ttu-id="1c2f2-166">[Le déploiement de l’entreprise Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).</span><span class="sxs-lookup"><span data-stu-id="1c2f2-166">[Web Deployment in the Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).</span></span> <span data-ttu-id="1c2f2-167">Ce didacticiel fournit une présentation conceptuelle pour les fichiers projet MSBuild, les fournisseurs de services, Web Deploy et autres technologies connexes.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-167">This tutorial provides a conceptual introduction to MSBuild project files, the WPP, Web Deploy, and other related technologies.</span></span> <span data-ttu-id="1c2f2-168">Il explique comment vous pouvez utiliser ces outils ensemble pour gérer les processus de déploiement complexe.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-168">It explains how you can use these tools together to manage complex deployment processes.</span></span>
- <span data-ttu-id="1c2f2-169">[Configuration d’environnements de serveur pour le déploiement Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="1c2f2-169">[Configuring Server Environments for Web Deployment](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).</span></span> <span data-ttu-id="1c2f2-170">Ce didacticiel explique comment configurer les serveurs Windows pour prendre en charge différents scénarios de déploiement, y compris le déploiement du package web à distance à l’aide le Service de l’Agent de déploiement Web (l’agent distant) ou le Gestionnaire de déploiement Web et le déploiement de la base de données distante.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-170">This tutorial describes how to configure Windows servers to support various deployment scenarios, including remote web package deployment using the Web Deployment Agent Service (the remote agent) or the Web Deploy Handler and remote database deployment.</span></span> <span data-ttu-id="1c2f2-171">Il fournit des conseils sur le choix de la méthode de déploiement appropriée pour votre propre environnement, et décrit l’utilisation de Web Farm Framework (WFF) pour répliquer les applications web déployées sur tous les serveurs web dans une batterie de serveurs.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-171">It provides guidance on choosing the appropriate deployment method for your own environment, and it describes how to use the Web Farm Framework (WFF) to replicate deployed web applications across all the web servers in a server farm.</span></span>
- <span data-ttu-id="1c2f2-172">[Configuration de Team Foundation Server pour le déploiement Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="1c2f2-172">[Configuring Team Foundation Server for Web Deployment](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).</span></span> <span data-ttu-id="1c2f2-173">Ce didacticiel explique comment configurer TFS pour prendre en charge différents scénarios de déploiement, notamment le déploiement automatisé en tant que partie d’un processus de l’élément de configuration et de déclencher manuellement les déploiements des builds spécifiques.</span><span class="sxs-lookup"><span data-stu-id="1c2f2-173">This tutorial describes how to configure TFS to support various deployment scenarios, including automated deployment as part of a CI process and manually triggered deployments of specific builds.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="1c2f2-174">Next</span><span class="sxs-lookup"><span data-stu-id="1c2f2-174">Next</span></span>](performing-a-what-if-deployment.md)