---
title: "Použití Ruby v webové aplikace Azure App Service v systému Linux | Microsoft Docs"
description: "Použití Ruby ve webové aplikace Azure App Service v systému Linux."
keywords: "služby Azure app service, webové aplikace, – nejčastější dotazy, linux, operačních systémů, ruby"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: 56105d1bc153e552e12c0c408c8f6075e4eff9d0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-ruby-in-web-app-on-linux"></a><span data-ttu-id="08a81-104">Použití Ruby v webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="08a81-104">Using Ruby in Web App on Linux</span></span> #

<span data-ttu-id="08a81-105">Pomocí nejnovější aktualizace na našem back-end zavedli jsme podporu pro poznámky Ruby v.2.3.</span><span class="sxs-lookup"><span data-stu-id="08a81-105">With the latest update to our backend, we introduced support for Ruby v.2.3.</span></span> <span data-ttu-id="08a81-106">Nastavením konfigurace webové aplikace v systému Linux, můžete změnit v zásobníku aplikace.</span><span class="sxs-lookup"><span data-stu-id="08a81-106">By setting the configuration of your Linux web app, you can change the application stack.</span></span>

## <a name="using-the-azure-portal"></a><span data-ttu-id="08a81-107">Použití webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="08a81-107">Using the Azure portal</span></span> ##

<span data-ttu-id="08a81-108">Z nové nabídky v [portál Azure](https://portal.azure.com), můžete k vytvoření webové aplikace v systému Linux z Web + mobilní možnosti, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="08a81-108">From the new menu in the [Azure portal](https://portal.azure.com), you can choose to create a Web App on Linux from the Web + Mobile option as shown in the following image:</span></span>

![Zahájení vytváření webové aplikace na portálu Azure][1]

<span data-ttu-id="08a81-110">Dále **vytvořit okno** otevře, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="08a81-110">Next, the **Create blade** opens as shown in the following image:</span></span>

![V okně Vytvořit][2]

1. <span data-ttu-id="08a81-112">Zadejte název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="08a81-112">Give your web app a name.</span></span>
2. <span data-ttu-id="08a81-113">Vyberte existující skupinu prostředků nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="08a81-113">Choose an existing resource group or create a new one.</span></span> <span data-ttu-id="08a81-114">(Viz dostupné oblasti v [omezení části](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="08a81-114">(See available regions in the [limitations section](app-service-linux-intro.md).)</span></span>
3. <span data-ttu-id="08a81-115">Vyberte existující plán služby Azure App Service nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="08a81-115">Choose an existing Azure App Service plan or create a new one.</span></span> <span data-ttu-id="08a81-116">(Naleznete v poznámkách k plánu služby App Service v [omezení části](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="08a81-116">(See App Service plan notes in the [limitations section](app-service-linux-intro.md).)</span></span>
4. <span data-ttu-id="08a81-117">Její vyberte z předdefinovaných Runtime zásobníky.</span><span class="sxs-lookup"><span data-stu-id="08a81-117">Choose the Ruby from the Built-in Runtime stacks.</span></span>

<span data-ttu-id="08a81-118">Po vytvoření získá Ruby webové aplikace, můžete nasadit se pomocí Git a FTP.</span><span class="sxs-lookup"><span data-stu-id="08a81-118">After your Ruby web app gets created, you can deploy to it using Git or FTP.</span></span>

<span data-ttu-id="08a81-119">Další informace o vytváření Ruby aplikace, zkontrolujte [Příručka Začínáme get](app-service-linux-ruby-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="08a81-119">To learn more about creating a Ruby app, check the [get started guide](app-service-linux-ruby-get-started.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="08a81-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="08a81-120">Next steps</span></span>
* [<span data-ttu-id="08a81-121">Co je webová aplikace v systému Linux?</span><span class="sxs-lookup"><span data-stu-id="08a81-121">What is Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="08a81-122">Místní nasazení z Gitu do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="08a81-122">Local Git Deployment to Azure App Service</span></span>](app-service-deploy-local-git.md)
* [<span data-ttu-id="08a81-123">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="08a81-123">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)
* [<span data-ttu-id="08a81-124">Vytvoření aplikace pro poznámky Ruby pomocí Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="08a81-124">Create a Ruby App with Azure Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-ruby/New-Linux.png
[2]: ./media/app-service-linux-using-ruby/Ruby-UX.png