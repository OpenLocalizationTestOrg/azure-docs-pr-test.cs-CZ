---
title: "Příprava nasazení cloudové služby (Node.js) | Microsoft Docs"
description: "Zjistěte, jak nasadit aplikaci Azure k testovacím prostředí, a potom nasazení v produkčním prostředí pomocí prohození virtuální IP (VIP)."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d65d26a6-b424-49cd-a88c-7ef46bb112a8
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: b3000ed769e8c60eccb21e26f53ce7ccb7e68d7f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="staging-an-application-in-azure"></a><span data-ttu-id="fcd04-103">Pracovní aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="fcd04-103">Staging an Application in Azure</span></span>
<span data-ttu-id="fcd04-104">Zabalené aplikace lze nasadit do fázovacího prostředí v Azure má být testována, před přesunutím do produkčního prostředí, ve kterém je aplikace přístupné z Internetu.</span><span class="sxs-lookup"><span data-stu-id="fcd04-104">A packaged application can be deployed to the staging environment in Azure to be tested before you move it to the production environment in which the application is accessible on the Internet.</span></span> <span data-ttu-id="fcd04-105">Je pracovní prostředí je úplně stejně jako provozní prostředí, ale jenom přístup k dvoufázové instalace aplikace pomocí zkomolené adresy URL, který je generovaný Azure.</span><span class="sxs-lookup"><span data-stu-id="fcd04-105">The staging environment is exactly like the production environment, except that you can only access the staged application with an obfuscated URL that is generated by Azure.</span></span> <span data-ttu-id="fcd04-106">Po ověření, že aplikace funguje správně, dá se nasadit do produkčního prostředí provedením prohození virtuální IP (VIP).</span><span class="sxs-lookup"><span data-stu-id="fcd04-106">After you have verified that your application is working correctly, it can be deployed to the production environment by performing a Virtual IP (VIP) swap.</span></span>

> [!NOTE]
> <span data-ttu-id="fcd04-107">Kroky v tomto článku se uplatní jenom na uzlu aplikace hostované jako cloudová služba Azure.</span><span class="sxs-lookup"><span data-stu-id="fcd04-107">The steps in this article only apply to node applications hosted as an Azure Cloud Service.</span></span>
> 
> 

## <a name="step-1-stage-an-application"></a><span data-ttu-id="fcd04-108">Krok 1: Příprava aplikace</span><span class="sxs-lookup"><span data-stu-id="fcd04-108">Step 1: Stage an Application</span></span>
<span data-ttu-id="fcd04-109">Tato úloha vysvětluje postup dvoufázové instalace aplikace pomocí **Microsoft Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="fcd04-109">This task covers how to stage an application by using the **Microsoft Azure PowerShell**.</span></span>

1. <span data-ttu-id="fcd04-110">Při publikování služby, stačí předat **-slotu** parametru **Publish-AzureServiceProject** rutiny.</span><span class="sxs-lookup"><span data-stu-id="fcd04-110">When publishing a service, simply pass the **-Slot** parameter to the **Publish-AzureServiceProject** cmdlet.</span></span>
   
   ```powershell
   Publish-AzureServiceProject -Slot staging
   ```
2. <span data-ttu-id="fcd04-111">Přihlaste se na [portál Azure classic] a vyberte **cloudové služby**.</span><span class="sxs-lookup"><span data-stu-id="fcd04-111">Log on to the [Azure classic portal] and select **Cloud Services**.</span></span> <span data-ttu-id="fcd04-112">Po cloudu vytvoření služby a **pracovní** stav sloupce byl aktualizovaný, aby **systémem**, klikněte na název služby.</span><span class="sxs-lookup"><span data-stu-id="fcd04-112">After the cloud service is created and the **Staging** column status has been updated to **Running**, click on the service name.</span></span>
   
   ![portál zobrazení spuštěnou službu][cloud-service]
3. <span data-ttu-id="fcd04-114">Vyberte **řídicí panel**a potom vyberte **pracovní**.</span><span class="sxs-lookup"><span data-stu-id="fcd04-114">Select the **Dashboard**, and then select **Staging**.</span></span>
   
   ![řídicí panel cloud service][cloud-service-dashboard]
4. <span data-ttu-id="fcd04-116">Poznamenejte si hodnotu **adresa URL webu** položka vpravo.</span><span class="sxs-lookup"><span data-stu-id="fcd04-116">Note the value in the **Site URL** entry to the right.</span></span> <span data-ttu-id="fcd04-117">Název DNS je zkomolené interní ID, které generuje Azure.</span><span class="sxs-lookup"><span data-stu-id="fcd04-117">The DNS name is an obfuscated internal ID that Azure generated.</span></span>
   
    ![Adresa url webu][cloud-service-staging-url]

<span data-ttu-id="fcd04-119">Nyní můžete ověřit, zda aplikace pracuje správně v testovacím prostředí pomocí pracovní adresa URL webu.</span><span class="sxs-lookup"><span data-stu-id="fcd04-119">Now you can verify that the application is working correctly in the staging environment by using the staging site URL.</span></span>

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a><span data-ttu-id="fcd04-120">Krok 2: Upgrade aplikace v produkčním prostředí pomocí Prohazuje virtuální IP adresy</span><span class="sxs-lookup"><span data-stu-id="fcd04-120">Step 2: Upgrade an Application in Production by Swapping VIPs</span></span>
<span data-ttu-id="fcd04-121">Po ověření upgradovanou verzi aplikace v testovacím prostředí, můžete rychle zpřístupnit jej v produkčním prostředí pomocí vzájemná záměna virtuálních IP adres (VIP), pracovní a provozní prostředí.</span><span class="sxs-lookup"><span data-stu-id="fcd04-121">After you have verified the upgraded version of an application in the staging environment, you can quickly make it available in production by swapping the virtual IPs (VIPs) of the staging and production environments.</span></span>

> [!NOTE]
> <span data-ttu-id="fcd04-122">Tento krok předpokládá, že jste už nasadit aplikace do produkčního prostředí a připraví upgradovanou verzi aplikace.</span><span class="sxs-lookup"><span data-stu-id="fcd04-122">This step assumes that you have already deployed an application to production and staged the upgraded version of the application.</span></span>
> 
> 

1. <span data-ttu-id="fcd04-123">Přihlaste se [portál Azure classic], klikněte na tlačítko **cloudové služby** a pak vyberte název služby.</span><span class="sxs-lookup"><span data-stu-id="fcd04-123">Log into the [Azure classic portal], click **Cloud Services** and then select the service name.</span></span>
2. <span data-ttu-id="fcd04-124">Z **řídicí panel**, vyberte **pracovní**a potom klikněte na **odkládacího souboru** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="fcd04-124">From the **Dashboard**, select **Staging**, and then click **Swap** at the bottom of the page.</span></span> <span data-ttu-id="fcd04-125">Otevře se dialogové okno prohození virtuálních IP adres.</span><span class="sxs-lookup"><span data-stu-id="fcd04-125">This opens the VIP Swap dialog.</span></span>
   
   ![Dialogové okno prohození virtuální IP adresy][vip-swap-dialog]
3. <span data-ttu-id="fcd04-127">Zkontrolujte informace a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="fcd04-127">Review the information, and then click **OK**.</span></span> <span data-ttu-id="fcd04-128">Dvě nasazení začněte aktualizace jako pracovní nasazení se přepne do produkčního prostředí a produkční nasazení se přepne do přípravy.</span><span class="sxs-lookup"><span data-stu-id="fcd04-128">The two deployments begin updating as the staging deployment switches to production and the production deployment switches to staging.</span></span>

<span data-ttu-id="fcd04-129">Úspěšně jste připravený nasazení a upgradovat produkční nasazení přes vzájemná záměna virtuální IP adresy s nasazením v pracovní.</span><span class="sxs-lookup"><span data-stu-id="fcd04-129">You have successfully staged a deployment and upgraded a production deployment by swapping VIPs with the deployment in staging.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fcd04-130">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="fcd04-130">Additional Resources</span></span>
* <span data-ttu-id="fcd04-131">[Vzájemná záměna virtuální IP adresy v Azure nasazení upgradu služby do produkčního prostředí]</span><span class="sxs-lookup"><span data-stu-id="fcd04-131">[How to Deploy a Service Upgrade to Production by Swapping VIPs in Azure]</span></span>

<span data-ttu-id="fcd04-132">[portál Azure classic]: http://manage.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="fcd04-132">[Azure classic portal]: http://manage.windowsazure.com</span></span>
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
<span data-ttu-id="fcd04-133">[Vzájemná záměna virtuální IP adresy v Azure nasazení upgradu služby do produkčního prostředí]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production</span><span class="sxs-lookup"><span data-stu-id="fcd04-133">[How to Deploy a Service Upgrade to Production by Swapping VIPs in Azure]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production</span></span>