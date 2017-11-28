---
title: "Vytvoření webové aplikace Azure systémem Linux | Microsoft Docs"
description: "Webové aplikace Tvorba pracovního postupu pro webové aplikace Azure v systému Linux."
keywords: "služby Azure app service, webové aplikace, linux, operačních systémů"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: 3a71d10a-a0fe-4d28-af95-03b2860057d5
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 49091d4a85bed23927850f9c0bbc5ea8b6e8c9e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-web-app-running-on-linux"></a><span data-ttu-id="40043-104">Vytvoření webové aplikace Azure systémem Linux</span><span class="sxs-lookup"><span data-stu-id="40043-104">Create an Azure web app running on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


## <a name="use-the-azure-portal-to-create-your-web-app"></a><span data-ttu-id="40043-105">Pomocí portálu Azure k vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="40043-105">Use the Azure portal to create your web app</span></span>
<span data-ttu-id="40043-106">Můžete začít s vytvářením webové aplikace v systému Linux z [portál Azure](https://portal.azure.com) jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="40043-106">You can start creating your web app on Linux from the [Azure portal](https://portal.azure.com) as shown in the following image:</span></span>

![Zahájení vytváření webové aplikace na portálu Azure][1]

<span data-ttu-id="40043-108">Dále **vytvořit okno** otevře, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="40043-108">Next, the **Create blade** opens as shown in the following image:</span></span>

![V okně Vytvořit][2]

1. <span data-ttu-id="40043-110">Zadejte název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="40043-110">Give your web app a name.</span></span>
2. <span data-ttu-id="40043-111">Vyberte existující skupinu prostředků nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="40043-111">Choose an existing resource group or create a new one.</span></span> <span data-ttu-id="40043-112">(Viz dostupné oblasti v [omezení části](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="40043-112">(See available regions in the [limitations section](app-service-linux-intro.md).)</span></span>
3. <span data-ttu-id="40043-113">Vyberte existující plán služby Azure App Service nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="40043-113">Choose an existing Azure App Service plan or create a new one.</span></span> <span data-ttu-id="40043-114">(Naleznete v poznámkách k plánu služby App Service v [omezení části](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="40043-114">(See App Service plan notes in the [limitations section](app-service-linux-intro.md).)</span></span>
4. <span data-ttu-id="40043-115">Zvolte zásobník aplikací, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="40043-115">Choose the application stack that you intend to use.</span></span> <span data-ttu-id="40043-116">Můžete si vybrat mezi několika verzích rozhraní Node.js, PHP, .net Core a Ruby.</span><span class="sxs-lookup"><span data-stu-id="40043-116">You can choose between several versions of Node.js, PHP, .Net Core, and Ruby.</span></span>

<span data-ttu-id="40043-117">Po vytvoření aplikace můžete změnit zásobník aplikací z nastavení aplikace jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="40043-117">Once you have created the app, you can change the application stack from the application settings as shown in the following image:</span></span>

![Nastavení aplikace][3]

## <a name="deploy-your-web-app"></a><span data-ttu-id="40043-119">Nasazení webové aplikace</span><span class="sxs-lookup"><span data-stu-id="40043-119">Deploy your web app</span></span>
<span data-ttu-id="40043-120">Výběr **možnosti nasazení** ze správy portál vám dává možnost použít místní úložiště Git nebo GitHub pro nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="40043-120">Choosing **deployment options** from the management portal gives you the option to use local Git or GitHub repository to deploy your application.</span></span> <span data-ttu-id="40043-121">Zbývající pokyny jsou podobné jako u jiných Linux webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="40043-121">The rest of the instructions are similar to those for a non-Linux web app.</span></span> <span data-ttu-id="40043-122">Můžete postupovat podle pokynů v [místní nasazení Git](app-service-deploy-local-git.md) nebo [průběžné nasazování](app-service-continuous-deployment.md) k nasazení své aplikace.</span><span class="sxs-lookup"><span data-stu-id="40043-122">You can follow the instructions in [local Git deployment](app-service-deploy-local-git.md) or [continuous deployment](app-service-continuous-deployment.md) to deploy your app.</span></span>

<span data-ttu-id="40043-123">FTP můžete použít taky k nahrání aplikace do vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="40043-123">You can also use FTP to upload your application to your site.</span></span> <span data-ttu-id="40043-124">Koncový bod FTP pro vaši webovou aplikaci můžete získat z části protokolů diagnostiky, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="40043-124">You can get the FTP endpoint for your web app from the diagnostics logs section as shown in the following image:</span></span>

![Diagnostické protokoly][4]

## <a name="next-steps"></a><span data-ttu-id="40043-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="40043-126">Next steps</span></span>
* [<span data-ttu-id="40043-127">Co je Azure webové aplikace v systému Linux?</span><span class="sxs-lookup"><span data-stu-id="40043-127">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="40043-128">Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="40043-128">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="40043-129">Použití Ruby v webové aplikace Azure App Service v systému Linux</span><span class="sxs-lookup"><span data-stu-id="40043-129">Using Ruby in Azure App Service Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="40043-130">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="40043-130">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
