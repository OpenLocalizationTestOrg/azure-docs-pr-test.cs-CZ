---
title: "aaaCreate Azure webová aplikace spuštěna v systému Linux | Microsoft Docs"
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
ms.openlocfilehash: de1bd030345d5e2a8024012067b5bcaa2cca09dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-web-app-running-on-linux"></a><span data-ttu-id="28b59-104">Vytvoření webové aplikace Azure systémem Linux</span><span class="sxs-lookup"><span data-stu-id="28b59-104">Create an Azure web app running on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


## <a name="use-hello-azure-portal-toocreate-your-web-app"></a><span data-ttu-id="28b59-105">Použít hello Azure portálu toocreate vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="28b59-105">Use hello Azure portal toocreate your web app</span></span>
<span data-ttu-id="28b59-106">Můžete začít s vytvářením webové aplikace v systému Linux z hello [portál Azure](https://portal.azure.com) jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="28b59-106">You can start creating your web app on Linux from hello [Azure portal](https://portal.azure.com) as shown in hello following image:</span></span>

![Zahájení vytváření webové aplikace na hello portálu Azure][1]

<span data-ttu-id="28b59-108">V dalším kroku hello **vytvořit okno** otevře, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="28b59-108">Next, hello **Create blade** opens as shown in hello following image:</span></span>

![okno Vytvořit Hello][2]

1. <span data-ttu-id="28b59-110">Zadejte název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="28b59-110">Give your web app a name.</span></span>
2. <span data-ttu-id="28b59-111">Vyberte existující skupinu prostředků nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="28b59-111">Choose an existing resource group or create a new one.</span></span> <span data-ttu-id="28b59-112">(Viz dostupné oblasti v hello [omezení části](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="28b59-112">(See available regions in hello [limitations section](app-service-linux-intro.md).)</span></span>
3. <span data-ttu-id="28b59-113">Vyberte existující plán služby Azure App Service nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="28b59-113">Choose an existing Azure App Service plan or create a new one.</span></span> <span data-ttu-id="28b59-114">(Naleznete v poznámkách k plánu služby App Service v hello [omezení části](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="28b59-114">(See App Service plan notes in hello [limitations section](app-service-linux-intro.md).)</span></span>
4. <span data-ttu-id="28b59-115">Vyberte aplikace hello zásobníku, že máte v úmyslu toouse.</span><span class="sxs-lookup"><span data-stu-id="28b59-115">Choose hello application stack that you intend toouse.</span></span> <span data-ttu-id="28b59-116">Můžete si vybrat mezi několika verzích rozhraní Node.js, PHP, .net Core a Ruby.</span><span class="sxs-lookup"><span data-stu-id="28b59-116">You can choose between several versions of Node.js, PHP, .Net Core, and Ruby.</span></span>

<span data-ttu-id="28b59-117">Po vytvoření aplikace hello, můžete změnit zásobník aplikací hello z nastavení aplikace hello jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="28b59-117">Once you have created hello app, you can change hello application stack from hello application settings as shown in hello following image:</span></span>

![Nastavení aplikace][3]

## <a name="deploy-your-web-app"></a><span data-ttu-id="28b59-119">Nasazení webové aplikace</span><span class="sxs-lookup"><span data-stu-id="28b59-119">Deploy your web app</span></span>
<span data-ttu-id="28b59-120">Výběr **možnosti nasazení** z portálu poskytuje správu hello je hello možnost toouse místní Git nebo Githubu úložiště toodeploy vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="28b59-120">Choosing **deployment options** from hello management portal gives you hello option toouse local Git or GitHub repository toodeploy your application.</span></span> <span data-ttu-id="28b59-121">Hello zbytek hello pokyny jsou podobné toothose pro webovou aplikaci systému Linux.</span><span class="sxs-lookup"><span data-stu-id="28b59-121">hello rest of hello instructions are similar toothose for a non-Linux web app.</span></span> <span data-ttu-id="28b59-122">Můžete postupovat podle pokynů hello v [místní nasazení Git](app-service-deploy-local-git.md) nebo [průběžné nasazování](app-service-continuous-deployment.md) toodeploy vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="28b59-122">You can follow hello instructions in [local Git deployment](app-service-deploy-local-git.md) or [continuous deployment](app-service-continuous-deployment.md) toodeploy your app.</span></span>

<span data-ttu-id="28b59-123">Můžete také pomocí FTP tooupload webu tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="28b59-123">You can also use FTP tooupload your application tooyour site.</span></span> <span data-ttu-id="28b59-124">Můžete získat koncový bod hello FTP pro vaši webovou aplikaci z hello diagnostiky protokoly části Jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="28b59-124">You can get hello FTP endpoint for your web app from hello diagnostics logs section as shown in hello following image:</span></span>

![Diagnostické protokoly][4]

## <a name="next-steps"></a><span data-ttu-id="28b59-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="28b59-126">Next steps</span></span>
* [<span data-ttu-id="28b59-127">Co je Azure webové aplikace v systému Linux?</span><span class="sxs-lookup"><span data-stu-id="28b59-127">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="28b59-128">Pomocí PM2 konfigurace pro Node.js v Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="28b59-128">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="28b59-129">Použití Ruby v webové aplikace Azure App Service v systému Linux</span><span class="sxs-lookup"><span data-stu-id="28b59-129">Using Ruby in Azure App Service Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="28b59-130">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="28b59-130">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
