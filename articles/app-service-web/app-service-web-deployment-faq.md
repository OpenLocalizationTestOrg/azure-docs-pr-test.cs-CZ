---
title: "Nejčastější dotazy k nasazení pro webové aplikace Azure | Microsoft Docs"
description: "Získejte odpovědi na nejčastější dotazy týkající se nasazení pro funkce Web Apps služby Azure App Service."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 69f8c50f7f5889b75544deca19c54268fbf5eca7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="deployment-faqs-for-web-apps-in-azure"></a><span data-ttu-id="83862-103">Nejčastější dotazy k nasazení pro webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="83862-103">Deployment FAQs for Web Apps in Azure</span></span>

<span data-ttu-id="83862-104">Tento článek obsahuje odpovědi na nejčastější dotazy (FAQ) o problémy při nasazení pro [funkce Web Apps služby Azure App Service](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="83862-104">This article has answers to frequently asked questions (FAQs) about deployment issues for the [Web Apps feature of Azure App Service](https://azure.microsoft.com/services/app-service/web/).</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-am-just-getting-started-with-app-service-web-apps-how-do-i-publish-my-code"></a><span data-ttu-id="83862-105">I se právě Začínáme s webovými aplikacemi App Service.</span><span class="sxs-lookup"><span data-stu-id="83862-105">I am just getting started with App Service web apps.</span></span> <span data-ttu-id="83862-106">Jak lze publikovat vlastní kód?</span><span class="sxs-lookup"><span data-stu-id="83862-106">How do I publish my code?</span></span>

<span data-ttu-id="83862-107">Zde jsou některé možnosti pro publikování kódu webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="83862-107">Here are some options for publishing your web app code:</span></span>

*   <span data-ttu-id="83862-108">Nasazení pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83862-108">Deploy by using Visual Studio.</span></span> <span data-ttu-id="83862-109">Pokud máte řešení sady Visual Studio, klikněte pravým tlačítkem na projekt webové aplikace a pak vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="83862-109">If you have the Visual Studio solution, right-click the web application project, and then select **Publish**.</span></span>
*   <span data-ttu-id="83862-110">Nasazení pomocí klienta FTP.</span><span class="sxs-lookup"><span data-stu-id="83862-110">Deploy by using an FTP client.</span></span> <span data-ttu-id="83862-111">Na portálu Azure stáhnete profil publikování pro webové aplikace, kterou chcete nasadit svůj kód.</span><span class="sxs-lookup"><span data-stu-id="83862-111">In the Azure portal, download the publish profile for the web app that you want to deploy your code to.</span></span> <span data-ttu-id="83862-112">Pak odešlete soubory do \site\wwwroot pomocí stejných přihlašovacích údajů Publikovat profil FTP.</span><span class="sxs-lookup"><span data-stu-id="83862-112">Then, upload the files to \site\wwwroot by using the same publish profile FTP credentials.</span></span>

<span data-ttu-id="83862-113">Další informace najdete v tématu [nasazení vaší aplikace do služby App Service](web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="83862-113">For more information, see [Deploy your app to App Service](web-sites-deploy.md).</span></span>

## <a name="i-see-an-error-message-when-i-try-to-deploy-from-visual-studio-how-do-i-resolve-this"></a><span data-ttu-id="83862-114">I při pokusu o nasazení ze sady Visual Studio zobrazí chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="83862-114">I see an error message when I try to deploy from Visual Studio.</span></span> <span data-ttu-id="83862-115">Jak to lze vyřešit?</span><span class="sxs-lookup"><span data-stu-id="83862-115">How do I resolve this?</span></span>

<span data-ttu-id="83862-116">Pokud se zobrazí následující zprávu, pravděpodobně používáte starší verze sady SDK: "při nasazení pro prostředek"YourResourceName"ve skupině prostředků 'YourResourceGroup' došlo k chybě: MissingRegistrationForLocation: předplatné není zaregistrované pro typ prostředku"součástmi"v umístění, střed USA".</span><span class="sxs-lookup"><span data-stu-id="83862-116">If you see the following message, you might be using an older version of the SDK: “Error during deployment for resource 'YourResourceName' in resource group 'YourResourceGroup': MissingRegistrationForLocation: The subscription is not registered for the resource type 'components' in the location 'Central US'.</span></span> <span data-ttu-id="83862-117">Zkuste se znovu zaregistrovat pro tohoto zprostředkovatele pro přístup do tohoto umístění."</span><span class="sxs-lookup"><span data-stu-id="83862-117">Please re-register for this provider in order to have access to this location.”</span></span> 

<span data-ttu-id="83862-118">Chcete-li tuto chybu vyřešit, upgradujte na [nejnovější SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="83862-118">To resolve this error, upgrade to the [latest SDK](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="83862-119">Pokud se zobrazí tato zpráva a máte na nejnovější SDK, odešlete žádost o podporu.</span><span class="sxs-lookup"><span data-stu-id="83862-119">If you see this message and you have the latest SDK, submit a support request.</span></span>

## <a name="how-do-i-deploy-an-aspnet-application-from-visual-studio-to-app-service"></a><span data-ttu-id="83862-120">Nasazení aplikace ASP.NET v sadě Visual Studio do služby App Service</span><span class="sxs-lookup"><span data-stu-id="83862-120">How do I deploy an ASP.NET application from Visual Studio to App Service?</span></span>
<a id="deployasp"></a>

<span data-ttu-id="83862-121">Tento kurz [vytvořte první webové aplikace ASP.NET v Azure v pěti minutách](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-get-started/) ukazuje, jak nasadit webovou aplikaci ASP.NET do webové aplikace ve službě App Service pomocí sady Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="83862-121">The tutorial [Create your first ASP.NET web app in Azure in five minutes](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-get-started/) shows you how to deploy an ASP.NET web application to a web app in App Service by using Visual Studio 2015.</span></span>

## <a name="what-are-the-different-types-of-deployment-credentials"></a><span data-ttu-id="83862-122">Jaké jsou různé typy přihlašovací údaje pro nasazení?</span><span class="sxs-lookup"><span data-stu-id="83862-122">What are the different types of deployment credentials?</span></span>

<span data-ttu-id="83862-123">Služby App Service podporuje dva typy přihlašovací údaje pro místní nasazením Git a FTP/S.</span><span class="sxs-lookup"><span data-stu-id="83862-123">App Service supports two types of credentials for local Git deployment and FTP/S deployment.</span></span> <span data-ttu-id="83862-124">Další informace o tom, jak nakonfigurovat přihlašovací údaje pro nasazení najdete v tématu [nakonfigurovat přihlašovací údaje nasazení pro službu App Service](app-service-deployment-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="83862-124">For more information about how to configure deployment credentials, see [Configure deployment credentials for App Service](app-service-deployment-credentials.md).</span></span>

## <a name="what-is-the-file-or-directory-structure-of-my-app-service-web-app"></a><span data-ttu-id="83862-125">Co je soubor nebo adresář strukturu webová aplikace služby App Service?</span><span class="sxs-lookup"><span data-stu-id="83862-125">What is the file or directory structure of my App Service web app?</span></span>

<span data-ttu-id="83862-126">Informace o struktuře souborů aplikace služby App Service najdete v tématu [struktura souboru v Azure](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure).</span><span class="sxs-lookup"><span data-stu-id="83862-126">For information about the file structure of your App Service app, see [File structure in Azure](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure).</span></span>

## <a name="how-do-i-resolve-ftp-error-550---there-is-not-enough-space-on-the-disk-when-i-try-to-ftp-my-files"></a><span data-ttu-id="83862-127">Jak lze vyřešit, "Chyba FTP 550 - zde není dostatek místa na disku" při Moje soubory FTP?</span><span class="sxs-lookup"><span data-stu-id="83862-127">How do I resolve "FTP Error 550 - There is not enough space on the disk" when I try to FTP my files?</span></span>

<span data-ttu-id="83862-128">Pokud se zobrazí tato zpráva, je pravděpodobné, že používáte do kvóty disku v plánu služby pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="83862-128">If you see this message, it's likely that you are running into a disk quota in the service plan for your web app.</span></span> <span data-ttu-id="83862-129">Možná budete muset vertikálně navýšit kapacitu na vyšší úroveň služby, na základě potřeb místa na disku.</span><span class="sxs-lookup"><span data-stu-id="83862-129">You might need to scale up to a higher service tier based on your disk space needs.</span></span> <span data-ttu-id="83862-130">Další informace o cenách plány a omezení prostředků najdete v tématu [služby App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="83862-130">For more information about pricing plans and resource limits, see [App Service pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>

## <a name="how-do-i-set-up-continuous-deployment-for-my-app-service-web-app"></a><span data-ttu-id="83862-131">Jak nastavit průběžné nasazování pro webovou aplikaci my služby App Service?</span><span class="sxs-lookup"><span data-stu-id="83862-131">How do I set up continuous deployment for my App Service web app?</span></span>

<span data-ttu-id="83862-132">Můžete nastavit průběžné nasazování z několika zdrojů, včetně Visual Studio Team Services, OneDrive, Githubu, Bitbucket, Dropbox a další úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="83862-132">You can set up continuous deployment from several resources, including Visual Studio Team Services, OneDrive, GitHub, Bitbucket, Dropbox, and other Git repositories.</span></span> <span data-ttu-id="83862-133">Tyto možnosti jsou dostupné na portálu.</span><span class="sxs-lookup"><span data-stu-id="83862-133">These options are available in the portal.</span></span> <span data-ttu-id="83862-134">[Průběžné nasazování do služby App Service](app-service-continuous-deployment.md) je užitečné kurz, který vysvětluje, jak nastavit průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="83862-134">[Continuous deployment to App Service](app-service-continuous-deployment.md) is a helpful tutorial that explains how to set up continuous deployment.</span></span>

## <a name="how-do-i-troubleshoot-issues-with-continuous-deployment-from-github-and-bitbucket"></a><span data-ttu-id="83862-135">Jak odstranit problémy s průběžné nasazování z Githubu a Bitbucket?</span><span class="sxs-lookup"><span data-stu-id="83862-135">How do I troubleshoot issues with continuous deployment from GitHub and Bitbucket?</span></span>

<span data-ttu-id="83862-136">Pomoc příčin problémů s průběžné nasazování z webu GitHub nebo Bitbucket, najdete v tématu [příčin průběžné nasazování](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment).</span><span class="sxs-lookup"><span data-stu-id="83862-136">For help investigating issues with continuous deployment from GitHub or Bitbucket, see [Investigating continuous deployment](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment).</span></span>

## <a name="i-cant-ftp-to-my-site-and-publish-my-code-how-do-i-resolve-this"></a><span data-ttu-id="83862-137">I nelze FTP na mé lokality a publikování vlastní kód.</span><span class="sxs-lookup"><span data-stu-id="83862-137">I can't FTP to my site and publish my code.</span></span> <span data-ttu-id="83862-138">Jak to lze vyřešit?</span><span class="sxs-lookup"><span data-stu-id="83862-138">How do I resolve this?</span></span>

<span data-ttu-id="83862-139">Chcete-li vyřešit problémy FTP:</span><span class="sxs-lookup"><span data-stu-id="83862-139">To resolve FTP issues:</span></span>

1. <span data-ttu-id="83862-140">Ověřte, že jste zadali správný hostitele název a pověření.</span><span class="sxs-lookup"><span data-stu-id="83862-140">Verify that you are entering the correct host name and credentials.</span></span> <span data-ttu-id="83862-141">Podrobné informace o různých typech přihlašovací údaje a jejich použití najdete v tématu [přihlašovací údaje nasazení](https://github.com/projectkudu/kudu/wiki/Deployment-credentials).</span><span class="sxs-lookup"><span data-stu-id="83862-141">For detailed information about different types of credentials and how to use them, see [Deployment credentials](https://github.com/projectkudu/kudu/wiki/Deployment-credentials).</span></span>
2. <span data-ttu-id="83862-142">Ověřte, že porty serveru FTP nejsou blokována bránou firewall.</span><span class="sxs-lookup"><span data-stu-id="83862-142">Verify that the FTP ports are not blocked by a firewall.</span></span> <span data-ttu-id="83862-143">Porty musí mít tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="83862-143">The ports should have these settings:</span></span>
    * <span data-ttu-id="83862-144">Port připojení řízení FTP: 21</span><span class="sxs-lookup"><span data-stu-id="83862-144">FTP control connection port: 21</span></span>
    * <span data-ttu-id="83862-145">Port pro připojení FTP data: 989, 10001-10300</span><span class="sxs-lookup"><span data-stu-id="83862-145">FTP data connection port: 989, 10001-10300</span></span>

## <a name="how-do-i-publish-my-code-to-app-service"></a><span data-ttu-id="83862-146">Jak publikovat vlastní kód do služby App Service?</span><span class="sxs-lookup"><span data-stu-id="83862-146">How do I publish my code to App Service?</span></span>

<span data-ttu-id="83862-147">Rychlý start Azure slouží k vám pomůžou nasadit aplikace pomocí nasazení zásobníku a metoda podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="83862-147">The Azure Quickstart is designed to help you deploy your app by using the deployment stack and method of your choice.</span></span> <span data-ttu-id="83862-148">Chcete-li použít startu na portálu Azure, přejděte na **nastavení** > **nasazení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="83862-148">To use the Quickstart, in the Azure portal, go to **Settings** > **App Deployment**.</span></span>

## <a name="why-does-my-app-sometimes-restart-after-deployment-to-app-service"></a><span data-ttu-id="83862-149">Proč aplikace my někdy restartovat po nasazení do služby App Service?</span><span class="sxs-lookup"><span data-stu-id="83862-149">Why does my app sometimes restart after deployment to App Service?</span></span>

<span data-ttu-id="83862-150">Další informace o podmínek, za kterých nasazení aplikace může způsobit restart najdete v tématu [nasazení oproti runtime problémy](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues#deployments-and-web-app-restarts").</span><span class="sxs-lookup"><span data-stu-id="83862-150">To learn about the circumstances under which an application deployment might result in a restart, see [Deployment vs. runtime issues](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues#deployments-and-web-app-restarts").</span></span> <span data-ttu-id="83862-151">Jak popisuje článek, služby App Service nasadí soubory do složky wwwroot.</span><span class="sxs-lookup"><span data-stu-id="83862-151">As the article describes, App Service deploys files to the wwwroot folder.</span></span> <span data-ttu-id="83862-152">Nikdy přímo restartuje vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="83862-152">It never directly restarts your app.</span></span>

## <a name="how-do-i-integrate-visual-studio-team-services-code-with-app-service"></a><span data-ttu-id="83862-153">Jak integrovat kódu Visual Studio Team Services službou App Service?</span><span class="sxs-lookup"><span data-stu-id="83862-153">How do I integrate Visual Studio Team Services code with App Service?</span></span>

<span data-ttu-id="83862-154">Máte dvě možnosti pro průběžné nasazování pomocí Visual Studio Team Services:</span><span class="sxs-lookup"><span data-stu-id="83862-154">You have two options for using continuous deployment with Visual Studio Team Services:</span></span>

*   <span data-ttu-id="83862-155">Pomocí Git projektu.</span><span class="sxs-lookup"><span data-stu-id="83862-155">Use a Git project.</span></span> <span data-ttu-id="83862-156">Připojte prostřednictvím služby App Service pomocí možnosti nasazení pro tento úložišti.</span><span class="sxs-lookup"><span data-stu-id="83862-156">Connect via App Service by using the deployment options for that repo.</span></span>
*   <span data-ttu-id="83862-157">Pomocí projektu Team Foundation verze ovládacího prvku (TFVC).</span><span class="sxs-lookup"><span data-stu-id="83862-157">Use a Team Foundation Version Control (TFVC) project.</span></span> <span data-ttu-id="83862-158">Nasazení pomocí agenta sestavení pro službu App Service.</span><span class="sxs-lookup"><span data-stu-id="83862-158">Deploy by using the build agent for App Service.</span></span>

<span data-ttu-id="83862-159">Nasazení průběžné kód pro obě tyto možnosti závisí na stávajících pracovními postupy developer a postupy vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="83862-159">Continuous code deployment for both these options depends on existing developer workflows and check-in procedures.</span></span> <span data-ttu-id="83862-160">Další informace najdete v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="83862-160">For more information, see these articles:</span></span> 

*   [<span data-ttu-id="83862-161">Implementace průběžné nasazování vaší aplikace na web Azure</span><span class="sxs-lookup"><span data-stu-id="83862-161">Implement continuous deployment of your app to an Azure website</span></span>](https://www.visualstudio.com/docs/release/examples/azure/azure-web-apps-from-build-and-release-hubs)
*   [<span data-ttu-id="83862-162">Nastavit účet Visual Studio Team Services, můžete nasadit do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="83862-162">Set up a Visual Studio Team Services account so it can deploy to a web app</span></span>](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App)

## <a name="how-do-i-use-ftp-or-ftps-to-deploy-my-app-to-app-service"></a><span data-ttu-id="83862-163">Jak používat protokol FTP nebo FTPS k nasazení aplikace do služby App Service?</span><span class="sxs-lookup"><span data-stu-id="83862-163">How do I use FTP or FTPS to deploy my app to App Service?</span></span>

<span data-ttu-id="83862-164">Informace o použití FTP a FTPS k nasazení vaší webové aplikace do služby App Service najdete v tématu [nasazení vaší aplikace do služby App Service pomocí FTP nebo S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="83862-164">For information about using FTP or FTPS to deploy your web app to App Service, see [Deploy your app to App Service by using FTP/S](app-service-deploy-ftp.md).</span></span>
