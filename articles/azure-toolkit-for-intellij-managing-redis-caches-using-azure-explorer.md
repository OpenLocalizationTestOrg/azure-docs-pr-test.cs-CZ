---
title: "Správa mezipaměti Redis Cache pomocí Průzkumníka Azure pro IntelliJ | Microsoft Docs"
description: "Naučte se spravovat své mezipaměti redis systému Azure pomocí Průzkumníka Azure pro IntelliJ."
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
ms.openlocfilehash: 9ab8ae17ee2a92b5b16d2210366c00b5b8023fa8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="managing-redis-caches-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="87bbb-103">Správa mezipaměti Redis Cache pomocí Průzkumníka Azure pro IntelliJ</span><span class="sxs-lookup"><span data-stu-id="87bbb-103">Managing Redis Caches using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="87bbb-104">Průzkumník Azure, který je součástí sady nástrojů Azure pro IntelliJ, poskytuje možnosti pro vývojáře v jazyce Java s řešením pro správu pro snadné použití redis mezipaměti v účtu Azure z uvnitř IntelliJ IDE.</span><span class="sxs-lookup"><span data-stu-id="87bbb-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing redis caches in their Azure account from inside the IntelliJ IDE.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-intellij"></a><span data-ttu-id="87bbb-105">Vytvoření mezipaměti Redis pomocí IntelliJ</span><span class="sxs-lookup"><span data-stu-id="87bbb-105">Create a Redis Cache by using IntelliJ</span></span>

<span data-ttu-id="87bbb-106">Následující postup vás provede kroky k vytvoření mezipaměti redis pomocí Průzkumníka Azure.</span><span class="sxs-lookup"><span data-stu-id="87bbb-106">The following steps walk you through the steps to create a redis cache using the Azure Explorer.</span></span>

1. <span data-ttu-id="87bbb-107">Přihlaste se k účtu Azure, pomocí kroků v [přihlášení v pokyny pro Azure nástrojů pro IntelliJ] článku.</span><span class="sxs-lookup"><span data-stu-id="87bbb-107">Sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ] article.</span></span>

1. <span data-ttu-id="87bbb-108">V **Průzkumník Azure** nástroj okno, rozbalte **Azure** uzel, klikněte pravým tlačítkem na **mezipaměti Redis cache**a potom klikněte na **vytvořit Redis Cache**.</span><span class="sxs-lookup"><span data-stu-id="87bbb-108">In the **Azure Explorer** tool window, expand the **Azure** node, right-click **Redis Caches**, and then click **Create Redis Cache**.</span></span>

   ![Vytvořit nabídku Redis Cache][CR01]

1. <span data-ttu-id="87bbb-110">Když **nová mezipaměť Redis** se zobrazí dialogové okno, určete následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="87bbb-110">When the **New Redis Cache** dialog box appears, specify the following options:</span></span>

   ![Nová mezipaměť Redis dialogové okno vytvořit][CR02]

   <span data-ttu-id="87bbb-112">a.</span><span class="sxs-lookup"><span data-stu-id="87bbb-112">a.</span></span> <span data-ttu-id="87bbb-113">**Název DNS**: Určuje subdoméně DNS pro novou mezipaměť redis systému, který se přidá jako předpona k ". redis.cache.windows.net"; například: *wingtiptoys.redis.cache.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="87bbb-113">**DNS Name**: Specifies the DNS subdomain for the new redis cache, which are prepended to ".redis.cache.windows.net"; for example: *wingtiptoys.redis.cache.windows.net*.</span></span>

   <span data-ttu-id="87bbb-114">b.</span><span class="sxs-lookup"><span data-stu-id="87bbb-114">b.</span></span> <span data-ttu-id="87bbb-115">**Předplatné**: Určuje předplatné Azure, kterou chcete použít pro nové mezipaměti redis.</span><span class="sxs-lookup"><span data-stu-id="87bbb-115">**Subscription**: Specifies the Azure subscription you want to use for the new redis cache.</span></span>

   <span data-ttu-id="87bbb-116">c.</span><span class="sxs-lookup"><span data-stu-id="87bbb-116">c.</span></span> <span data-ttu-id="87bbb-117">**Skupina prostředků**: Určuje skupinu prostředků pro vaše mezipaměť redis, je potřeba vyberte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="87bbb-117">**Resource Group**: Specifies the resource group for your redis cache; you need to choose one of the following options:</span></span>
      * <span data-ttu-id="87bbb-118">**Vytvořit nový**: Určuje, že chcete vytvořit novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="87bbb-118">**Create New**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="87bbb-119">**Použít existující**: Určuje, že zvolíte ze seznamu skupin prostředků, které jsou přidružené k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="87bbb-119">**Use Existing**: Specifies that you will choose from a list of resource groups associated with your Azure account.</span></span>

   <span data-ttu-id="87bbb-120">d.</span><span class="sxs-lookup"><span data-stu-id="87bbb-120">d.</span></span> <span data-ttu-id="87bbb-121">**Umístění**: Určuje umístění, kde se má vytvořit vaše mezipaměť redis; například *západní USA*.</span><span class="sxs-lookup"><span data-stu-id="87bbb-121">**Location**: Specifies the location where your redis cache is created; for example, *West US*.</span></span>

   <span data-ttu-id="87bbb-122">e.</span><span class="sxs-lookup"><span data-stu-id="87bbb-122">e.</span></span> <span data-ttu-id="87bbb-123">**Cenová úroveň**: Určuje, které cenovou úroveň, které používá vaše mezipaměť redis; toto nastavení určuje počet připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="87bbb-123">**Pricing Tier**: Specifies which pricing tier your redis cache uses; this setting determines the number of client connections.</span></span> <span data-ttu-id="87bbb-124">(Další informace najdete v tématu [Redis Cache ceny].)</span><span class="sxs-lookup"><span data-stu-id="87bbb-124">(For more information, see [Redis Cache Pricing].)</span></span>

   <span data-ttu-id="87bbb-125">f.</span><span class="sxs-lookup"><span data-stu-id="87bbb-125">f.</span></span> <span data-ttu-id="87bbb-126">**Port bez SSL**: Určuje, jestli vaše mezipaměť redis umožňuje připojení bez SSL; ve výchozím nastavení, jsou povoleny pouze připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="87bbb-126">**Non-SSL port**: Specifies whether your redis cache allows non-SSL connections; by default, only SSL connections are allowed.</span></span>

1. <span data-ttu-id="87bbb-127">Pokud jste zadali všechna nastavení mezipaměti redis, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="87bbb-127">When you have specified all your redis cache settings, click **OK**.</span></span>

<span data-ttu-id="87bbb-128">Po vytvoření mezipaměti redis, zobrazí se v Průzkumníkovi Azure.</span><span class="sxs-lookup"><span data-stu-id="87bbb-128">After your redis cache has been created, it will be displayed in the Azure Explorer.</span></span>

   ![Redis Cache v Azure Průzkumníku][CR03]

> [!NOTE]
>
> <span data-ttu-id="87bbb-130">Další informace o konfiguraci vašeho Azure redis cache nastavení najdete v tématu [postup konfigurace Azure Redis Cache].</span><span class="sxs-lookup"><span data-stu-id="87bbb-130">For more information about configuring your Azure redis cache settings, see [How to configure Azure Redis Cache].</span></span>
>

## <a name="display-the-properties-for-your-redis-cache-in-intellij"></a><span data-ttu-id="87bbb-131">Zobrazí vlastnosti pro mezipaměť Redis v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="87bbb-131">Display the properties for your Redis Cache in IntelliJ</span></span>

1. <span data-ttu-id="87bbb-132">V Průzkumníku Azure, klikněte pravým tlačítkem na vaše mezipaměť redis a klikněte na tlačítko **zobrazit vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="87bbb-132">In the Azure Explorer, right-click your redis cache and click **Show properties**.</span></span>

   ![Místní nabídku Azure Průzkumníka zobrazíte vlastnosti mezipaměti redis][SP01]

1. <span data-ttu-id="87bbb-134">Průzkumník Azure zobrazí vlastnosti pro vaše mezipaměť redis.</span><span class="sxs-lookup"><span data-stu-id="87bbb-134">The Azure Explorer displays the properties for your redis cache.</span></span>

   ![Vlastnosti mezipaměti Redis][SP02]

## <a name="delete-your-redis-cache-by-using-intellij"></a><span data-ttu-id="87bbb-136">Odstranění mezipaměti Redis pomocí IntelliJ</span><span class="sxs-lookup"><span data-stu-id="87bbb-136">Delete your Redis Cache by using IntelliJ</span></span>

1. <span data-ttu-id="87bbb-137">V Průzkumníku Azure, klikněte pravým tlačítkem na vaše mezipaměť redis a klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="87bbb-137">In the Azure Explorer, right-click your redis cache and click **Delete**.</span></span>

   ![Místní nabídku Azure Průzkumníka odstranit mezipaměti redis][DE01]

1. <span data-ttu-id="87bbb-139">Klikněte na tlačítko **Ano** při zobrazení výzvy k odstranění vaše mezipaměť redis.</span><span class="sxs-lookup"><span data-stu-id="87bbb-139">Click **Yes** when prompted to delete your redis cache.</span></span>

   ![Odstranění řádku mezipaměti redis][DE02]

## <a name="next-steps"></a><span data-ttu-id="87bbb-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87bbb-141">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="87bbb-142">Další informace o mezipaměti redis systému Azure, nastavení konfigurace a cenách naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="87bbb-142">For more information about Azure redis caches, configuration settings and pricing, see the following links:</span></span>

* <span data-ttu-id="87bbb-143">[Azure Redis Cache]</span><span class="sxs-lookup"><span data-stu-id="87bbb-143">[Azure Redis Cache]</span></span>
* <span data-ttu-id="87bbb-144">[Dokumentaci k mezipaměti redis]</span><span class="sxs-lookup"><span data-stu-id="87bbb-144">[Redis Cache Documentation]</span></span>
* <span data-ttu-id="87bbb-145">[Redis Cache ceny]</span><span class="sxs-lookup"><span data-stu-id="87bbb-145">[Redis Cache Pricing]</span></span>
* <span data-ttu-id="87bbb-146">[postup konfigurace Azure Redis Cache]</span><span class="sxs-lookup"><span data-stu-id="87bbb-146">[How to configure Azure Redis Cache]</span></span>

<!-- URL List -->

<span data-ttu-id="87bbb-147">[Redis Cache ceny]: https://azure.microsoft.com/pricing/details/cache/</span><span class="sxs-lookup"><span data-stu-id="87bbb-147">[Redis Cache Pricing]: https://azure.microsoft.com/pricing/details/cache/</span></span>
<span data-ttu-id="87bbb-148">[Azure Redis Cache]: https://azure.microsoft.com/services/cache/</span><span class="sxs-lookup"><span data-stu-id="87bbb-148">[Azure Redis Cache]: https://azure.microsoft.com/services/cache/</span></span>
<span data-ttu-id="87bbb-149">[Dokumentaci k mezipaměti redis]: ./redis-cache/index.md</span><span class="sxs-lookup"><span data-stu-id="87bbb-149">[Redis Cache Documentation]: ./redis-cache/index.md</span></span>
<span data-ttu-id="87bbb-150">[postup konfigurace Azure Redis Cache]: ./redis-cache/cache-configure.md</span><span class="sxs-lookup"><span data-stu-id="87bbb-150">[How to configure Azure Redis Cache]: ./redis-cache/cache-configure.md</span></span>
<span data-ttu-id="87bbb-151">[přihlášení v pokyny pro Azure nástrojů pro IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="87bbb-151">[Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>

<!-- IMG List -->

[CR01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE02.png
