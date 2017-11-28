---
title: "aaaIntroduction tooAzure webové aplikace v systému Linux | Microsoft Docs"
description: "Další informace o Azure webové aplikace v systému Linux."
keywords: "služby Azure app service, linux, operačních systémů"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 43b9865ade251909a77429eb3e18fe0bcaac3bde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-web-app-on-linux"></a><span data-ttu-id="c350c-104">Úvod tooAzure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="c350c-104">Introduction tooAzure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a><span data-ttu-id="c350c-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="c350c-105">Overview</span></span>
<span data-ttu-id="c350c-106">Zákazníci mohou používat webové aplikace ve webových aplikacích toohost Linux nativně v systému Linux pro zásobníky podporovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="c350c-106">Customers can use Web App on Linux toohost web apps natively on Linux for supported application stacks.</span></span> <span data-ttu-id="c350c-107">Hello následující části jsou uvedeny zásobníky hello aplikace, které jsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="c350c-107">hello following section lists hello application stacks that are currently supported.</span></span> 

## <a name="features"></a><span data-ttu-id="c350c-108">Funkce</span><span class="sxs-lookup"><span data-stu-id="c350c-108">Features</span></span>
<span data-ttu-id="c350c-109">Webové aplikace v systému Linux aktuálně podporuje následující zásobníky aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="c350c-109">Web App on Linux currently supports hello following application stacks:</span></span>

* <span data-ttu-id="c350c-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="c350c-110">Node.js</span></span>
    * <span data-ttu-id="c350c-111">4.4</span><span class="sxs-lookup"><span data-stu-id="c350c-111">4.4</span></span>
    * <span data-ttu-id="c350c-112">4.5</span><span class="sxs-lookup"><span data-stu-id="c350c-112">4.5</span></span>
    * <span data-ttu-id="c350c-113">6.2</span><span class="sxs-lookup"><span data-stu-id="c350c-113">6.2</span></span>
    * <span data-ttu-id="c350c-114">6.6</span><span class="sxs-lookup"><span data-stu-id="c350c-114">6.6</span></span>
    * <span data-ttu-id="c350c-115">6.9</span><span class="sxs-lookup"><span data-stu-id="c350c-115">6.9</span></span>
    * <span data-ttu-id="c350c-116">6.10</span><span class="sxs-lookup"><span data-stu-id="c350c-116">6.10</span></span>
    * <span data-ttu-id="c350c-117">6.11</span><span class="sxs-lookup"><span data-stu-id="c350c-117">6.11</span></span>
    * <span data-ttu-id="c350c-118">8.0</span><span class="sxs-lookup"><span data-stu-id="c350c-118">8.0</span></span>
    * <span data-ttu-id="c350c-119">8.1</span><span class="sxs-lookup"><span data-stu-id="c350c-119">8.1</span></span>
* <span data-ttu-id="c350c-120">PHP</span><span class="sxs-lookup"><span data-stu-id="c350c-120">PHP</span></span>
    * <span data-ttu-id="c350c-121">5.6</span><span class="sxs-lookup"><span data-stu-id="c350c-121">5.6</span></span>
    * <span data-ttu-id="c350c-122">7.0</span><span class="sxs-lookup"><span data-stu-id="c350c-122">7.0</span></span>
* <span data-ttu-id="c350c-123">.NET core</span><span class="sxs-lookup"><span data-stu-id="c350c-123">.Net Core</span></span>
    * <span data-ttu-id="c350c-124">1.0</span><span class="sxs-lookup"><span data-stu-id="c350c-124">1.0</span></span>
    * <span data-ttu-id="c350c-125">1.1</span><span class="sxs-lookup"><span data-stu-id="c350c-125">1.1</span></span>
* <span data-ttu-id="c350c-126">Ruby</span><span class="sxs-lookup"><span data-stu-id="c350c-126">Ruby</span></span>
    * <span data-ttu-id="c350c-127">2.3</span><span class="sxs-lookup"><span data-stu-id="c350c-127">2.3</span></span>

<span data-ttu-id="c350c-128">Zákazníci můžou nasazovat svých aplikacích pomocí:</span><span class="sxs-lookup"><span data-stu-id="c350c-128">Customers can deploy their applications by using:</span></span>

* <span data-ttu-id="c350c-129">FTP</span><span class="sxs-lookup"><span data-stu-id="c350c-129">FTP</span></span>
* <span data-ttu-id="c350c-130">Místní Git</span><span class="sxs-lookup"><span data-stu-id="c350c-130">Local Git</span></span>
* <span data-ttu-id="c350c-131">GitHubu</span><span class="sxs-lookup"><span data-stu-id="c350c-131">GitHub</span></span>
* <span data-ttu-id="c350c-132">Bitbucket</span><span class="sxs-lookup"><span data-stu-id="c350c-132">Bitbucket</span></span>

<span data-ttu-id="c350c-133">Pro škálování aplikací:</span><span class="sxs-lookup"><span data-stu-id="c350c-133">For application scaling:</span></span>

* <span data-ttu-id="c350c-134">Zákazníci mohou nahoru a dolů škálování webové aplikace tak, že změníte úroveň hello jejich plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="c350c-134">Customers can scale web apps up and down by changing hello tier of their App Service plan</span></span>
* <span data-ttu-id="c350c-135">Zákazníci můžete škálovat aplikace a spustit více instancí aplikace v rámci hello rámec jejich SKU</span><span class="sxs-lookup"><span data-stu-id="c350c-135">Customers can scale out applications and run multiple app instances within hello confines of their SKU</span></span>

<span data-ttu-id="c350c-136">Pro Kudu, některé základní funkce hello:</span><span class="sxs-lookup"><span data-stu-id="c350c-136">For Kudu, some of hello basic functionality:</span></span>

* <span data-ttu-id="c350c-137">Prostředí</span><span class="sxs-lookup"><span data-stu-id="c350c-137">Environments</span></span>
* <span data-ttu-id="c350c-138">Nasazení</span><span class="sxs-lookup"><span data-stu-id="c350c-138">Deployments</span></span>
* <span data-ttu-id="c350c-139">Základní konzoly</span><span class="sxs-lookup"><span data-stu-id="c350c-139">Basic console</span></span>
* <span data-ttu-id="c350c-140">SSH</span><span class="sxs-lookup"><span data-stu-id="c350c-140">SSH</span></span>

<span data-ttu-id="c350c-141">Pro devops:</span><span class="sxs-lookup"><span data-stu-id="c350c-141">For devops:</span></span>

* <span data-ttu-id="c350c-142">Přípravná prostředí</span><span class="sxs-lookup"><span data-stu-id="c350c-142">Staging environments</span></span>
* <span data-ttu-id="c350c-143">ACR a DockerHub CI/CD</span><span class="sxs-lookup"><span data-stu-id="c350c-143">ACR and DockerHub CI/CD</span></span>

## <a name="limitations"></a><span data-ttu-id="c350c-144">Omezení</span><span class="sxs-lookup"><span data-stu-id="c350c-144">Limitations</span></span>
<span data-ttu-id="c350c-145">Hello portálu Azure jsou pouze funkce, které aktuálně fungují pro webovou aplikaci v systému Linux a skryje hello rest.</span><span class="sxs-lookup"><span data-stu-id="c350c-145">hello Azure portal shows only features that currently work for Web App on Linux and hides hello rest.</span></span> <span data-ttu-id="c350c-146">Jak jsme povolit další funkce, bude zobrazovat na portálu hello.</span><span class="sxs-lookup"><span data-stu-id="c350c-146">As we enable more features, they will be visible on hello portal.</span></span>

<span data-ttu-id="c350c-147">Některé funkce, například integrace virtuální sítě, ověřování Azure Active Directory nebo třetích stran nebo rozšíření lokality Kudu, ještě nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c350c-147">Some features, such as virtual network integration, Azure Active Directory/third-party authentication, or Kudu site extensions, are not available yet.</span></span> <span data-ttu-id="c350c-148">Jakmile se tyto funkce jsou k dispozici, budeme aktualizovat naší dokumentaci a blog o změnách hello.</span><span class="sxs-lookup"><span data-stu-id="c350c-148">Once these features are available, we will update our documentation and blog about hello changes.</span></span>

<span data-ttu-id="c350c-149">Tuto verzi public preview je aktuálně k dispozici pouze v hello následující oblasti:</span><span class="sxs-lookup"><span data-stu-id="c350c-149">This public preview is currently only available in hello following regions:</span></span>

* <span data-ttu-id="c350c-150">Západní USA</span><span class="sxs-lookup"><span data-stu-id="c350c-150">West US</span></span>
* <span data-ttu-id="c350c-151">Východ USA</span><span class="sxs-lookup"><span data-stu-id="c350c-151">East US</span></span>
* <span data-ttu-id="c350c-152">Západní Evropa</span><span class="sxs-lookup"><span data-stu-id="c350c-152">West Europe</span></span>
* <span data-ttu-id="c350c-153">Severní Evropa</span><span class="sxs-lookup"><span data-stu-id="c350c-153">North Europe</span></span>
* <span data-ttu-id="c350c-154">Střed USA – jih</span><span class="sxs-lookup"><span data-stu-id="c350c-154">South Central US</span></span>
* <span data-ttu-id="c350c-155">Střed USA – sever</span><span class="sxs-lookup"><span data-stu-id="c350c-155">North Central US</span></span>
* <span data-ttu-id="c350c-156">Jihovýchodní Asie</span><span class="sxs-lookup"><span data-stu-id="c350c-156">Southeast Asia</span></span>
* <span data-ttu-id="c350c-157">Východní Asie</span><span class="sxs-lookup"><span data-stu-id="c350c-157">East Asia</span></span>
* <span data-ttu-id="c350c-158">Austrálie – východ</span><span class="sxs-lookup"><span data-stu-id="c350c-158">Australia East</span></span>
* <span data-ttu-id="c350c-159">Japonsko – východ</span><span class="sxs-lookup"><span data-stu-id="c350c-159">Japan East</span></span>
* <span data-ttu-id="c350c-160">Brazílie – jih</span><span class="sxs-lookup"><span data-stu-id="c350c-160">Brazil South</span></span>
* <span data-ttu-id="c350c-161">Indie – jih</span><span class="sxs-lookup"><span data-stu-id="c350c-161">South India</span></span>

<span data-ttu-id="c350c-162">Webové aplikace v systému Linux je podporována pouze v rámci plánů služby vyhrazené aplikace hello a nemá úroveň Free nebo sdílené.</span><span class="sxs-lookup"><span data-stu-id="c350c-162">Web Apps on Linux is only supported in hello Dedicated app service plans and does not have a Free or Shared tier.</span></span> <span data-ttu-id="c350c-163">Také plány služby App Service pro regulární a Linux webové aplikace se vzájemně vylučují, proto nelze vytvořit webovou aplikaci Linux v plánu služby app-systému Linux.</span><span class="sxs-lookup"><span data-stu-id="c350c-163">Also, App Service plans for regular and Linux web apps are mutually exclusive, so you cannot create a Linux web app in a non-Linux app service plan.</span></span>

<span data-ttu-id="c350c-164">Webové aplikace v systému Linux musí být vytvořeny ve skupině prostředků, který neobsahuje jiné Linux webové aplikace v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="c350c-164">Web Apps on Linux must be created in a resource group that does not contain non-Linux web apps in hello same region.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c350c-165">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="c350c-165">Troubleshooting</span></span> ##

<span data-ttu-id="c350c-166">Když toostart selhání aplikace nebo chcete toocheck hello protokolování z vaší aplikace, zkontrolujte hello Docker protokolů v adresáři LogFiles hello.</span><span class="sxs-lookup"><span data-stu-id="c350c-166">When your application fails toostart or you want toocheck hello logging from your app, check hello Docker logs in hello LogFiles directory.</span></span> <span data-ttu-id="c350c-167">Buď prostřednictvím webu SCM nebo FTP, můžete přístup k tomuto adresáři.</span><span class="sxs-lookup"><span data-stu-id="c350c-167">You can access this directory either through your SCM site or via FTP.</span></span>
<span data-ttu-id="c350c-168">toolog hello `stdout` a `stderr` z kontejneru, je nutné tooenable **kontejner Docker protokolování** pod **protokolů diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="c350c-168">toolog hello `stdout` and `stderr` from your container, you need tooenable **Docker Container logging** under **Diagnostics Logs**.</span></span>

![Povolení protokolování][2]

![Pomocí modulu Kudu tooview Docker protokolů][1]

<span data-ttu-id="c350c-171">Dostanete hello SCM lokality z **Rozšířené nástroje** v hello **nástroje pro vývoj** nabídky.</span><span class="sxs-lookup"><span data-stu-id="c350c-171">You can access hello SCM site from **Advanced Tools** in hello **Development Tools** menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c350c-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c350c-172">Next steps</span></span>
<span data-ttu-id="c350c-173">V tématu hello následující odkazy tooget spuštění službou App Service v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="c350c-173">See hello following links tooget started with App Service on Linux.</span></span> <span data-ttu-id="c350c-174">Otázky a aspekty můžete zveřejnit na [našem fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="c350c-174">You can post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>

* [<span data-ttu-id="c350c-175">Jak toouse vlastní Docker obrázků pro webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="c350c-175">How toouse a custom Docker image for Azure Web App on Linux</span></span>](app-service-linux-using-custom-docker-image.md)
* [<span data-ttu-id="c350c-176">Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="c350c-176">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="c350c-177">Pomocí .NET Core v webové aplikace Azure App Service v systému Linux</span><span class="sxs-lookup"><span data-stu-id="c350c-177">Using .NET Core in Azure App Service Web App on Linux</span></span>](app-service-linux-using-dotnetcore.md)
* [<span data-ttu-id="c350c-178">Použití Ruby v webové aplikace Azure App Service v systému Linux</span><span class="sxs-lookup"><span data-stu-id="c350c-178">Using Ruby in Azure App Service Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="c350c-179">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="c350c-179">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)
* [<span data-ttu-id="c350c-180">Podpora SSH pro webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="c350c-180">SSH support for Azure Web App on Linux</span></span>](./app-service-linux-ssh-support.md)
* [<span data-ttu-id="c350c-181">Nastavení přípravných prostředí v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c350c-181">Set up staging environments in Azure App Service</span></span>](./web-sites-staged-publishing.md)
* [<span data-ttu-id="c350c-182">Docker Hub průběžné nasazování pomocí Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="c350c-182">Docker Hub Continuous Deployment with Azure Web App on Linux</span></span>](./app-service-linux-ci-cd.md)

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png