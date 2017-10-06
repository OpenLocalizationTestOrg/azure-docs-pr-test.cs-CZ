---
title: "aaaUsing Ruby v Azure App Service webové aplikace v systému Linux | Microsoft Docs"
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
ms.openlocfilehash: 45692cb3bf1da9ff65b9466055029bfaef8b7d8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-ruby-in-web-app-on-linux"></a><span data-ttu-id="25272-104">Použití Ruby v webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="25272-104">Using Ruby in Web App on Linux</span></span> #

<span data-ttu-id="25272-105">U hello nejnovější aktualizace tooour back-end zavedli jsme podporu pro poznámky Ruby v.2.3.</span><span class="sxs-lookup"><span data-stu-id="25272-105">With hello latest update tooour backend, we introduced support for Ruby v.2.3.</span></span> <span data-ttu-id="25272-106">Nastavení konfigurace hello Linux webové aplikace můžete změnit zásobník aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="25272-106">By setting hello configuration of your Linux web app, you can change hello application stack.</span></span>

## <a name="using-hello-azure-portal"></a><span data-ttu-id="25272-107">Pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="25272-107">Using hello Azure portal</span></span> ##

<span data-ttu-id="25272-108">Z nové nabídky hello v hello [portál Azure](https://portal.azure.com), můžete zvolit toocreate webové aplikace v systému Linux hello Web + mobilní možnost jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="25272-108">From hello new menu in hello [Azure portal](https://portal.azure.com), you can choose toocreate a Web App on Linux from hello Web + Mobile option as shown in hello following image:</span></span>

![Zahájení vytváření webové aplikace na hello portálu Azure][1]

<span data-ttu-id="25272-110">V dalším kroku hello **vytvořit okno** otevře, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="25272-110">Next, hello **Create blade** opens as shown in hello following image:</span></span>

![okno Vytvořit Hello][2]

1. <span data-ttu-id="25272-112">Zadejte název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="25272-112">Give your web app a name.</span></span>
2. <span data-ttu-id="25272-113">Vyberte existující skupinu prostředků nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="25272-113">Choose an existing resource group or create a new one.</span></span> <span data-ttu-id="25272-114">(Viz dostupné oblasti v hello [omezení části](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="25272-114">(See available regions in hello [limitations section](app-service-linux-intro.md).)</span></span>
3. <span data-ttu-id="25272-115">Vyberte existující plán služby Azure App Service nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="25272-115">Choose an existing Azure App Service plan or create a new one.</span></span> <span data-ttu-id="25272-116">(Naleznete v poznámkách k plánu služby App Service v hello [omezení části](app-service-linux-intro.md).)</span><span class="sxs-lookup"><span data-stu-id="25272-116">(See App Service plan notes in hello [limitations section](app-service-linux-intro.md).)</span></span>
4. <span data-ttu-id="25272-117">Hello Ruby vyberte z předdefinovaných Runtime zásobníky hello.</span><span class="sxs-lookup"><span data-stu-id="25272-117">Choose hello Ruby from hello Built-in Runtime stacks.</span></span>

<span data-ttu-id="25272-118">Po vytvoření získá Ruby webové aplikace, můžete nasadit tooit pomocí Git a FTP.</span><span class="sxs-lookup"><span data-stu-id="25272-118">After your Ruby web app gets created, you can deploy tooit using Git or FTP.</span></span>

<span data-ttu-id="25272-119">Další informace o toolearn vytvořit aplikaci, Ruby, zkontrolujte hello [Příručka Začínáme get](app-service-linux-ruby-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="25272-119">toolearn more about creating a Ruby app, check hello [get started guide](app-service-linux-ruby-get-started.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="25272-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25272-120">Next steps</span></span>
* [<span data-ttu-id="25272-121">Co je webová aplikace v systému Linux?</span><span class="sxs-lookup"><span data-stu-id="25272-121">What is Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="25272-122">Místní nasazení Git tooAzure služby App Service</span><span class="sxs-lookup"><span data-stu-id="25272-122">Local Git Deployment tooAzure App Service</span></span>](app-service-deploy-local-git.md)
* [<span data-ttu-id="25272-123">Webové aplikace Azure App Service v systému Linux – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="25272-123">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)
* [<span data-ttu-id="25272-124">Vytvoření aplikace pro poznámky Ruby pomocí Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="25272-124">Create a Ruby App with Azure Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-ruby/New-Linux.png
[2]: ./media/app-service-linux-using-ruby/Ruby-UX.png