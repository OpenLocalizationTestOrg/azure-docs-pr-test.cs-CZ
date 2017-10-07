---
title: "Průzkumník Azure pro Eclipse aaaManaging mezipaměti Redis Cache pomocí hello | Microsoft Docs"
description: "Zjistěte, jak toomanage Azure redis ukládá do mezipaměti pomocí hello Průzkumník Azure pro Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 06/14/2017
ms.author: robmcm
ms.openlocfilehash: aa0c38862bda7919a3fc6c53c2fdaf555dd64bff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-redis-caches-using-hello-azure-explorer-for-eclipse"></a><span data-ttu-id="268ee-103">Správa mezipaměti Redis Cache pomocí hello Průzkumník Azure pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="268ee-103">Managing Redis Caches using hello Azure Explorer for Eclipse</span></span>

<span data-ttu-id="268ee-104">Průzkumník Azure, který je součástí hello nástrojů Azure pro prostředí Eclipse, poskytuje možnosti pro vývojáře v jazyce Java s řešením snadno použitelné pro správu redis mezipaměti v účtu Azure z uvnitř hello Eclipse IDE Hello.</span><span class="sxs-lookup"><span data-stu-id="268ee-104">hello Azure Explorer, which is part of hello Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing redis caches in their Azure account from inside hello Eclipse IDE.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-eclipse"></a><span data-ttu-id="268ee-105">Vytvoření mezipaměti Redis pomocí Eclipse</span><span class="sxs-lookup"><span data-stu-id="268ee-105">Create a Redis Cache by using Eclipse</span></span>

<span data-ttu-id="268ee-106">Hello následující kroky vás provedou kroky toocreate hello mezipaměti redis pomocí hello Průzkumník Azure.</span><span class="sxs-lookup"><span data-stu-id="268ee-106">hello following steps walk you through hello steps toocreate a redis cache using hello Azure Explorer.</span></span>

1. <span data-ttu-id="268ee-107">Přihlaste se tooyour účet Azure postupem hello hello [přihlášení v pokyny pro hello nástrojů Azure pro Eclipse] článku.</span><span class="sxs-lookup"><span data-stu-id="268ee-107">Sign in tooyour Azure account using hello steps in hello [Sign In Instructions for hello Azure Toolkit for Eclipse] article.</span></span>

1. <span data-ttu-id="268ee-108">V hello **Azure Explorer** nástroj okno, rozbalte položku hello **Azure** uzel, klikněte pravým tlačítkem na **mezipaměti Redis cache**a potom klikněte na **vytvořit Redis Cache**.</span><span class="sxs-lookup"><span data-stu-id="268ee-108">In hello **Azure Explorer** tool window, expand hello **Azure** node, right-click **Redis Caches**, and then click **Create Redis Cache**.</span></span>

   ![Vytvořit nabídku mezipaměti Redis][CR01]

1. <span data-ttu-id="268ee-110">Když hello **nová mezipaměť Redis** se zobrazí dialogové okno, zadejte hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="268ee-110">When hello **New Redis Cache** dialog box appears, specify hello following options:</span></span>

   ![Vytvořit nový Redis Cache dialogové okno][CR02]

   <span data-ttu-id="268ee-112">a.</span><span class="sxs-lookup"><span data-stu-id="268ee-112">a.</span></span> <span data-ttu-id="268ee-113">**Název DNS**: Určuje hello DNS subdomény pro hello novou mezipaměť redis systému, který se přidá jako předpona příliš ". redis.cache.windows .net"; například: *wingtiptoys.redis.cache.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="268ee-113">**DNS Name**: Specifies hello DNS subdomain for hello new redis cache, which is prepended too".redis.cache.windows.net"; for example: *wingtiptoys.redis.cache.windows.net*.</span></span>

   <span data-ttu-id="268ee-114">b.</span><span class="sxs-lookup"><span data-stu-id="268ee-114">b.</span></span> <span data-ttu-id="268ee-115">**Předplatné**: Určuje hello předplatné Azure, které chcete použít pro nová mezipaměť redis hello toouse.</span><span class="sxs-lookup"><span data-stu-id="268ee-115">**Subscription**: Specifies hello Azure subscription you want toouse for hello new redis cache.</span></span>

   <span data-ttu-id="268ee-116">c.</span><span class="sxs-lookup"><span data-stu-id="268ee-116">c.</span></span> <span data-ttu-id="268ee-117">**Skupina prostředků**: Určuje hello skupinu prostředků pro vaše mezipaměť redis, je třeba toochoose jeden hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="268ee-117">**Resource Group**: Specifies hello resource group for your redis cache; you need toochoose one of hello following options:</span></span>
      * <span data-ttu-id="268ee-118">**Vytvořit nový**: Určuje, že chcete toocreate novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="268ee-118">**Create New**: Specifies that you want toocreate a new resource group.</span></span>
      * <span data-ttu-id="268ee-119">**Použít existující**: Určuje, že zvolíte ze seznamu skupin prostředků, které jsou přidružené k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="268ee-119">**Use Existing**: Specifies that you will choose from a list of resource groups associated with your Azure account.</span></span>

   <span data-ttu-id="268ee-120">d.</span><span class="sxs-lookup"><span data-stu-id="268ee-120">d.</span></span> <span data-ttu-id="268ee-121">**Umístění**: Určuje hello umístění, kde se má vytvořit vaše mezipaměť redis; například *západní USA*.</span><span class="sxs-lookup"><span data-stu-id="268ee-121">**Location**: Specifies hello location where your redis cache is created; for example, *West US*.</span></span>

   <span data-ttu-id="268ee-122">e.</span><span class="sxs-lookup"><span data-stu-id="268ee-122">e.</span></span> <span data-ttu-id="268ee-123">**Cenová úroveň**: Určuje, které cenovou úroveň, které používá vaše mezipaměť redis; toto nastavení určuje hello počtu připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="268ee-123">**Pricing Tier**: Specifies which pricing tier your redis cache uses; this setting determines hello number of client connections.</span></span> <span data-ttu-id="268ee-124">(Další informace najdete v tématu [Redis Cache ceny].)</span><span class="sxs-lookup"><span data-stu-id="268ee-124">(For more information, see [Redis Cache Pricing].)</span></span>

   <span data-ttu-id="268ee-125">f.</span><span class="sxs-lookup"><span data-stu-id="268ee-125">f.</span></span> <span data-ttu-id="268ee-126">**Port bez SSL**: Určuje, jestli vaše mezipaměť redis umožňuje připojení bez SSL; ve výchozím nastavení, jsou povoleny pouze připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="268ee-126">**Non-SSL port**: Specifies whether your redis cache allows non-SSL connections; by default, only SSL connections are allowed.</span></span>

1. <span data-ttu-id="268ee-127">Pokud jste zadali všechna nastavení mezipaměti redis, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="268ee-127">When you have specified all your redis cache settings, click **OK**.</span></span>

<span data-ttu-id="268ee-128">Po vytvoření mezipaměti redis, zobrazí se v hello Průzkumník Azure.</span><span class="sxs-lookup"><span data-stu-id="268ee-128">After your redis cache has been created, it will be displayed in hello Azure Explorer.</span></span>

   ![Redis Cache v Azure Průzkumníku][CR03]

> [!NOTE]
>
> <span data-ttu-id="268ee-130">Další informace o konfiguraci vašeho Azure redis cache nastavení najdete v tématu [jak tooconfigure Azure Redis Cache].</span><span class="sxs-lookup"><span data-stu-id="268ee-130">For more information about configuring your Azure redis cache settings, see [How tooconfigure Azure Redis Cache].</span></span>
>

## <a name="display-hello-properties-for-your-redis-cache-in-eclipse"></a><span data-ttu-id="268ee-131">Zobrazí vlastnosti hello ke svojí mezipaměti Redis v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="268ee-131">Display hello properties for your Redis Cache in Eclipse</span></span>

1. <span data-ttu-id="268ee-132">V hello Azure Explorer, klikněte pravým tlačítkem na vaše mezipaměť redis a klikněte na **zobrazit vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="268ee-132">In hello Azure Explorer, right-click your redis cache and click **Show properties**.</span></span>

   ![Azure Explorer kontextové nabídky toodisplay vlastnosti mezipaměti redis][SP01]

1. <span data-ttu-id="268ee-134">Hello Azure Explorer zobrazí hello vlastnosti mezipaměti redis.</span><span class="sxs-lookup"><span data-stu-id="268ee-134">hello Azure Explorer displays hello properties for your redis cache.</span></span>

   ![Vlastnosti mezipaměti Redis][SP02]

## <a name="delete-your-redis-cache-by-using-eclipse"></a><span data-ttu-id="268ee-136">Odstranění mezipaměti Redis pomocí Eclipse</span><span class="sxs-lookup"><span data-stu-id="268ee-136">Delete your Redis Cache by using Eclipse</span></span>

1. <span data-ttu-id="268ee-137">V hello Azure Explorer, klikněte pravým tlačítkem na vaše mezipaměť redis a klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="268ee-137">In hello Azure Explorer, right-click your redis cache and click **Delete**.</span></span>

   ![Azure Explorer kontextové nabídky toodelete mezipaměti redis][DE01]

1. <span data-ttu-id="268ee-139">Klikněte na tlačítko **OK** při zobrazení výzvy toodelete vaše mezipaměť redis.</span><span class="sxs-lookup"><span data-stu-id="268ee-139">Click **OK** when prompted toodelete your redis cache.</span></span>

   ![Odstranění řádku mezipaměti redis][DE02]

## <a name="next-steps"></a><span data-ttu-id="268ee-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="268ee-141">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="268ee-142">Další informace o mezipamětí Azure redis, nastavení konfigurace a cenách najdete v tématu hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="268ee-142">For more information about Azure redis caches, configuration settings and pricing, see hello following links:</span></span>

* <span data-ttu-id="268ee-143">[Azure Redis Cache]</span><span class="sxs-lookup"><span data-stu-id="268ee-143">[Azure Redis Cache]</span></span>
* <span data-ttu-id="268ee-144">[Dokumentaci k mezipaměti redis]</span><span class="sxs-lookup"><span data-stu-id="268ee-144">[Redis Cache Documentation]</span></span>
* <span data-ttu-id="268ee-145">[Redis Cache ceny]</span><span class="sxs-lookup"><span data-stu-id="268ee-145">[Redis Cache Pricing]</span></span>
* <span data-ttu-id="268ee-146">[jak tooconfigure Azure Redis Cache]</span><span class="sxs-lookup"><span data-stu-id="268ee-146">[How tooconfigure Azure Redis Cache]</span></span>

<!-- URL List -->

[Redis Cache ceny]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis Cache]: https://azure.microsoft.com/services/cache/
[Dokumentaci k mezipaměti redis]: ./redis-cache/index.md
[jak tooconfigure Azure Redis Cache]: ./redis-cache/cache-configure.md
[přihlášení v pokyny pro hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md

<!-- IMG List -->

[CR01]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/DE02.png
