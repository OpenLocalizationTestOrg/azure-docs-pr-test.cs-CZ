---
title: "Úvod do Azure webové aplikace v systému Linux | Microsoft Docs"
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
ms.openlocfilehash: 0870f811845ec7c705da13f08abdfa762d25b209
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="introduction-to-azure-web-app-on-linux"></a><span data-ttu-id="2d052-104">Úvod do Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="2d052-104">Introduction to Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a><span data-ttu-id="2d052-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="2d052-105">Overview</span></span>
<span data-ttu-id="2d052-106">Zákazníci mohou pomocí webové aplikace v systému Linux do hostitele webové aplikace v systému Linux nativně pro zásobníky podporovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="2d052-106">Customers can use Web App on Linux to host web apps natively on Linux for supported application stacks.</span></span> <span data-ttu-id="2d052-107">V následující části jsou uvedeny zásobníky aplikace, které jsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="2d052-107">The following section lists the application stacks that are currently supported.</span></span> 

## <a name="features"></a><span data-ttu-id="2d052-108">Funkce</span><span class="sxs-lookup"><span data-stu-id="2d052-108">Features</span></span>
<span data-ttu-id="2d052-109">Webové aplikace v systému Linux aktuálně podporuje následující zásobníky aplikace:</span><span class="sxs-lookup"><span data-stu-id="2d052-109">Web App on Linux currently supports the following application stacks:</span></span>

* <span data-ttu-id="2d052-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="2d052-110">Node.js</span></span>
    * <span data-ttu-id="2d052-111">4.4</span><span class="sxs-lookup"><span data-stu-id="2d052-111">4.4</span></span>
    * <span data-ttu-id="2d052-112">4.5</span><span class="sxs-lookup"><span data-stu-id="2d052-112">4.5</span></span>
    * <span data-ttu-id="2d052-113">6.2</span><span class="sxs-lookup"><span data-stu-id="2d052-113">6.2</span></span>
    * <span data-ttu-id="2d052-114">6.6</span><span class="sxs-lookup"><span data-stu-id="2d052-114">6.6</span></span>
    * <span data-ttu-id="2d052-115">6.9</span><span class="sxs-lookup"><span data-stu-id="2d052-115">6.9</span></span>
    * <span data-ttu-id="2d052-116">6.10</span><span class="sxs-lookup"><span data-stu-id="2d052-116">6.10</span></span>
    * <span data-ttu-id="2d052-117">6.11</span><span class="sxs-lookup"><span data-stu-id="2d052-117">6.11</span></span>
    * <span data-ttu-id="2d052-118">8.0</span><span class="sxs-lookup"><span data-stu-id="2d052-118">8.0</span></span>
    * <span data-ttu-id="2d052-119">8.1</span><span class="sxs-lookup"><span data-stu-id="2d052-119">8.1</span></span>
* <span data-ttu-id="2d052-120">PHP</span><span class="sxs-lookup"><span data-stu-id="2d052-120">PHP</span></span>
    * <span data-ttu-id="2d052-121">5.6</span><span class="sxs-lookup"><span data-stu-id="2d052-121">5.6</span></span>
    * <span data-ttu-id="2d052-122">7.0</span><span class="sxs-lookup"><span data-stu-id="2d052-122">7.0</span></span>
* <span data-ttu-id="2d052-123">.NET core</span><span class="sxs-lookup"><span data-stu-id="2d052-123">.Net Core</span></span>
    * <span data-ttu-id="2d052-124">1.0</span><span class="sxs-lookup"><span data-stu-id="2d052-124">1.0</span></span>
    * <span data-ttu-id="2d052-125">1.1</span><span class="sxs-lookup"><span data-stu-id="2d052-125">1.1</span></span>
* <span data-ttu-id="2d052-126">Ruby</span><span class="sxs-lookup"><span data-stu-id="2d052-126">Ruby</span></span>
    * <span data-ttu-id="2d052-127">2.3</span><span class="sxs-lookup"><span data-stu-id="2d052-127">2.3</span></span>

<span data-ttu-id="2d052-128">Zákazníci můžou nasazovat svých aplikacích pomocí:</span><span class="sxs-lookup"><span data-stu-id="2d052-128">Customers can deploy their applications by using:</span></span>

* <span data-ttu-id="2d052-129">FTP</span><span class="sxs-lookup"><span data-stu-id="2d052-129">FTP</span></span>
* <span data-ttu-id="2d052-130">Místní Git</span><span class="sxs-lookup"><span data-stu-id="2d052-130">Local Git</span></span>
* <span data-ttu-id="2d052-131">GitHubu</span><span class="sxs-lookup"><span data-stu-id="2d052-131">GitHub</span></span>
* <span data-ttu-id="2d052-132">Bitbucket</span><span class="sxs-lookup"><span data-stu-id="2d052-132">Bitbucket</span></span>

<span data-ttu-id="2d052-133">Pro škálování aplikací:</span><span class="sxs-lookup"><span data-stu-id="2d052-133">For application scaling:</span></span>

* <span data-ttu-id="2d052-134">Zákazníci mohou nahoru a dolů škálování webové aplikace tak, že změníte úroveň jejich plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="2d052-134">Customers can scale web apps up and down by changing the tier of their App Service plan</span></span>
* <span data-ttu-id="2d052-135">Zákazníci můžete škálovat aplikace a spustit více instancí aplikace v hranicích jejich SKU</span><span class="sxs-lookup"><span data-stu-id="2d052-135">Customers can scale out applications and run multiple app instances within the confines of their SKU</span></span>

<span data-ttu-id="2d052-136">Pro Kudu, některé základní funkce:</span><span class="sxs-lookup"><span data-stu-id="2d052-136">For Kudu, some of the basic functionality:</span></span>

* <span data-ttu-id="2d052-137">Prostředí</span><span class="sxs-lookup"><span data-stu-id="2d052-137">Environments</span></span>
* <span data-ttu-id="2d052-138">Nasazení</span><span class="sxs-lookup"><span data-stu-id="2d052-138">Deployments</span></span>
* <span data-ttu-id="2d052-139">Základní konzoly</span><span class="sxs-lookup"><span data-stu-id="2d052-139">Basic console</span></span>
* <span data-ttu-id="2d052-140">SSH</span><span class="sxs-lookup"><span data-stu-id="2d052-140">SSH</span></span>

<span data-ttu-id="2d052-141">Pro devops:</span><span class="sxs-lookup"><span data-stu-id="2d052-141">For devops:</span></span>

* <span data-ttu-id="2d052-142">Přípravná prostředí</span><span class="sxs-lookup"><span data-stu-id="2d052-142">Staging environments</span></span>
* <span data-ttu-id="2d052-143">ACR a DockerHub CI/CD</span><span class="sxs-lookup"><span data-stu-id="2d052-143">ACR and DockerHub CI/CD</span></span>

## <a name="limitations"></a><span data-ttu-id="2d052-144">Omezení</span><span class="sxs-lookup"><span data-stu-id="2d052-144">Limitations</span></span>
<span data-ttu-id="2d052-145">Portál Azure jsou pouze funkce, které aktuálně fungují pro webovou aplikaci v systému Linux a zbytek skryje.</span><span class="sxs-lookup"><span data-stu-id="2d052-145">The Azure portal shows only features that currently work for Web App on Linux and hides the rest.</span></span> <span data-ttu-id="2d052-146">Jak jsme povolit další funkce, se bude zobrazovat na portálu.</span><span class="sxs-lookup"><span data-stu-id="2d052-146">As we enable more features, they will be visible on the portal.</span></span>

<span data-ttu-id="2d052-147">Některé funkce, například integrace virtuální sítě, ověřování Azure Active Directory nebo třetích stran nebo rozšíření lokality Kudu, ještě nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2d052-147">Some features, such as virtual network integration, Azure Active Directory/third-party authentication, or Kudu site extensions, are not available yet.</span></span> <span data-ttu-id="2d052-148">Jakmile se tyto funkce jsou k dispozici, budeme aktualizovat naší dokumentaci a blog o změnách.</span><span class="sxs-lookup"><span data-stu-id="2d052-148">Once these features are available, we will update our documentation and blog about the changes.</span></span>

<span data-ttu-id="2d052-149">Tato verze public preview je aktuálně k dispozici pouze v následujících oblastech:</span><span class="sxs-lookup"><span data-stu-id="2d052-149">This public preview is currently only available in the following regions:</span></span>

* <span data-ttu-id="2d052-150">Západní USA</span><span class="sxs-lookup"><span data-stu-id="2d052-150">West US</span></span>
* <span data-ttu-id="2d052-151">Východ USA</span><span class="sxs-lookup"><span data-stu-id="2d052-151">East US</span></span>
* <span data-ttu-id="2d052-152">Západní Evropa</span><span class="sxs-lookup"><span data-stu-id="2d052-152">West Europe</span></span>
* <span data-ttu-id="2d052-153">Severní Evropa</span><span class="sxs-lookup"><span data-stu-id="2d052-153">North Europe</span></span>
* <span data-ttu-id="2d052-154">Střed USA – jih</span><span class="sxs-lookup"><span data-stu-id="2d052-154">South Central US</span></span>
* <span data-ttu-id="2d052-155">Střed USA – sever</span><span class="sxs-lookup"><span data-stu-id="2d052-155">North Central US</span></span>
* <span data-ttu-id="2d052-156">Jihovýchodní Asie</span><span class="sxs-lookup"><span data-stu-id="2d052-156">Southeast Asia</span></span>
* <span data-ttu-id="2d052-157">Východní Asie</span><span class="sxs-lookup"><span data-stu-id="2d052-157">East Asia</span></span>
* <span data-ttu-id="2d052-158">Austrálie – východ</span><span class="sxs-lookup"><span data-stu-id="2d052-158">Australia East</span></span>
* <span data-ttu-id="2d052-159">Japonsko – východ</span><span class="sxs-lookup"><span data-stu-id="2d052-159">Japan East</span></span>
* <span data-ttu-id="2d052-160">Brazílie – jih</span><span class="sxs-lookup"><span data-stu-id="2d052-160">Brazil South</span></span>
* <span data-ttu-id="2d052-161">Indie – jih</span><span class="sxs-lookup"><span data-stu-id="2d052-161">South India</span></span>

<span data-ttu-id="2d052-162">Webové aplikace v systému Linux je podporována pouze v rámci plánů služby vyhrazené aplikace a nemá úroveň Free nebo sdílené.</span><span class="sxs-lookup"><span data-stu-id="2d052-162">Web Apps on Linux is only supported in the Dedicated app service plans and does not have a Free or Shared tier.</span></span> <span data-ttu-id="2d052-163">Také plány služby App Service pro regulární a Linux webové aplikace se vzájemně vylučují, proto nelze vytvořit webovou aplikaci Linux v plánu služby app-systému Linux.</span><span class="sxs-lookup"><span data-stu-id="2d052-163">Also, App Service plans for regular and Linux web apps are mutually exclusive, so you cannot create a Linux web app in a non-Linux app service plan.</span></span>

<span data-ttu-id="2d052-164">Webové aplikace v systému Linux musí být vytvořeny ve skupině prostředků, který neobsahuje jiné Linux webové aplikace ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="2d052-164">Web Apps on Linux must be created in a resource group that does not contain non-Linux web apps in the same region.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="2d052-165">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="2d052-165">Troubleshooting</span></span> ##

<span data-ttu-id="2d052-166">Pokud vaše aplikace se nepodaří spustit nebo chcete provést kontrolu protokolování z vaší aplikace, zkontrolujte, že že docker protokolů v adresáři LogFiles.</span><span class="sxs-lookup"><span data-stu-id="2d052-166">When your application fails to start or you want to check the logging from your app, check the Docker logs in the LogFiles directory.</span></span> <span data-ttu-id="2d052-167">Buď prostřednictvím webu SCM nebo FTP, můžete přístup k tomuto adresáři.</span><span class="sxs-lookup"><span data-stu-id="2d052-167">You can access this directory either through your SCM site or via FTP.</span></span>
<span data-ttu-id="2d052-168">Do protokolu `stdout` a `stderr` z kontejneru, je nutné povolit **kontejner Docker protokolování** pod **protokolů diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="2d052-168">To log the `stdout` and `stderr` from your container, you need to enable **Docker Container logging** under **Diagnostics Logs**.</span></span>

![Povolení protokolování][2]

![Pokud chcete zobrazit protokoly Docker pomocí modulu Kudu][1]

<span data-ttu-id="2d052-171">Můžete získat přístup k webu SCM z **Rozšířené nástroje** v **nástroje pro vývoj** nabídky.</span><span class="sxs-lookup"><span data-stu-id="2d052-171">You can access the SCM site from **Advanced Tools** in the **Development Tools** menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2d052-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2d052-172">Next steps</span></span>
<span data-ttu-id="2d052-173">V následujících tématech začít pracovat s App Service v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="2d052-173">See the following links to get started with App Service on Linux.</span></span> <span data-ttu-id="2d052-174">Otázky a aspekty můžete zveřejnit na [našem fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="2d052-174">You can post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>

* [<span data-ttu-id="2d052-175">Jak používat vlastní image Docker pro webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="2d052-175">How to use a custom Docker image for Azure Web App on Linux</span></span>](app-service-linux-using-custom-docker-image.md)
* [<span data-ttu-id="2d052-176">Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="2d052-176">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="2d052-177">Pomocí .NET Core v webové aplikace Azure App Service v systému Linux</span><span class="sxs-lookup"><span data-stu-id="2d052-177">Using .NET Core in Azure App Service Web App on Linux</span></span>](app-service-linux-using-dotnetcore.md)
* [<span data-ttu-id="2d052-178">Použití Ruby v webové aplikace Azure App Service v systému Linux</span><span class="sxs-lookup"><span data-stu-id="2d052-178">Using Ruby in Azure App Service Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="2d052-179">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="2d052-179">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)
* [<span data-ttu-id="2d052-180">Podpora SSH pro webové aplikace Azure v systému Linux</span><span class="sxs-lookup"><span data-stu-id="2d052-180">SSH support for Azure Web App on Linux</span></span>](./app-service-linux-ssh-support.md)
* [<span data-ttu-id="2d052-181">Nastavení přípravných prostředí v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2d052-181">Set up staging environments in Azure App Service</span></span>](./web-sites-staged-publishing.md)
* [<span data-ttu-id="2d052-182">Docker Hub průběžné nasazování pomocí Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="2d052-182">Docker Hub Continuous Deployment with Azure Web App on Linux</span></span>](./app-service-linux-ci-cd.md)

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png