---
title: "Připojení ke službě Azure Analysis Services s Power BI | Microsoft Docs"
description: "Zjistěte, jak se připojit k serveru Azure Analysis Services pomocí Power BI."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 3a2af043feddb4a1d6d63f50e838c8a39035449f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-with-power-bi"></a><span data-ttu-id="72309-103">Připojit k Power BI</span><span class="sxs-lookup"><span data-stu-id="72309-103">Connect with Power BI</span></span>

<span data-ttu-id="72309-104">Jakmile jste vytvořili serveru v Azure a nasadili do ní tabulkový model, jste připravení připojit a začít zkoumat data uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="72309-104">Once you've created a server in Azure, and deployed a tabular model to it, users in your organization are ready to connect and begin exploring data.</span></span> 

> [!TIP]
> <span data-ttu-id="72309-105">Nezapomeňte použít nejnovější verzi [Power BI Desktop](https://powerbi.microsoft.com/desktop/).</span><span class="sxs-lookup"><span data-stu-id="72309-105">Be sure to use the latest version of [Power BI Desktop](https://powerbi.microsoft.com/desktop/).</span></span>
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a><span data-ttu-id="72309-106">Připojení v Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="72309-106">Connect in Power BI Desktop</span></span>

1. <span data-ttu-id="72309-107">V Power BI Desktop, klikněte na **načíst Data** > **Azure** > **databáze Azure Analysis Services**.</span><span class="sxs-lookup"><span data-stu-id="72309-107">In Power BI Desktop, click **Get Data** > **Azure** > **Azure Analysis Services database**.</span></span>

2. <span data-ttu-id="72309-108">V **Server**, zadejte název serveru.</span><span class="sxs-lookup"><span data-stu-id="72309-108">In **Server**, enter the server name.</span></span> 
    
    <span data-ttu-id="72309-109">Nezapomeňte zahrnout úplnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="72309-109">Be sure to include the full URL.</span></span> <span data-ttu-id="72309-110">Například asazure://westcentralus.asazure.windows.net/advworks.</span><span class="sxs-lookup"><span data-stu-id="72309-110">For example, asazure://westcentralus.asazure.windows.net/advworks.</span></span>

3. <span data-ttu-id="72309-111">V **databáze**, pokud znáte název databázi tabulkového modelu nebo perspektivy, kterou chcete připojit, vložte jej zde.</span><span class="sxs-lookup"><span data-stu-id="72309-111">In **Database**, if you know the name of the tabular model database or perspective you want to connect to, paste it here.</span></span> <span data-ttu-id="72309-112">Jinak můžete ponechat toto pole prázdné a vybrat databázi nebo perspektivy později.</span><span class="sxs-lookup"><span data-stu-id="72309-112">Otherwise, you can leave this field blank and select a database or perspective later.</span></span>

4. <span data-ttu-id="72309-113">Ponechte výchozí nastavení **připojit za provozu** možnost a potom stiskněte klávesu **Connect**.</span><span class="sxs-lookup"><span data-stu-id="72309-113">Leave the default **Connect live** option, then press **Connect**.</span></span> 

5. <span data-ttu-id="72309-114">Po zobrazení výzvy zadejte své přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="72309-114">If prompted, enter your login credentials.</span></span> 

6. <span data-ttu-id="72309-115">V **Navigátor**, rozbalte server a pak vyberte modelu nebo perspektivy chcete připojit, pak klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="72309-115">In **Navigator**, expand the server, then select the model or perspective you want to connect to, then click **Connect**.</span></span> <span data-ttu-id="72309-116">Klikněte na modelu nebo perspektivy zobrazíte všechny objekty pro toto zobrazení.</span><span class="sxs-lookup"><span data-stu-id="72309-116">Click  a model or perspective to show all the objects for that view.</span></span>

    <span data-ttu-id="72309-117">Model se otevře v Power BI Desktop s prázdnou sestavou v zobrazení sestavy.</span><span class="sxs-lookup"><span data-stu-id="72309-117">The model opens in Power BI Desktop with a blank report in Report view.</span></span> <span data-ttu-id="72309-118">V seznamu polí zobrazí všechny objekty modelu nebyla skrytá.</span><span class="sxs-lookup"><span data-stu-id="72309-118">The Fields list displays all non-hidden model objects.</span></span> <span data-ttu-id="72309-119">Stav připojení je zobrazen v pravém dolním rohu.</span><span class="sxs-lookup"><span data-stu-id="72309-119">Connection status is displayed in the lower-right corner.</span></span>

## <a name="connect-in-power-bi-service"></a><span data-ttu-id="72309-120">Připojení v Power BI (služba)</span><span class="sxs-lookup"><span data-stu-id="72309-120">Connect in Power BI (service)</span></span>

1. <span data-ttu-id="72309-121">Vytvořte soubor Power BI Desktop, která má aktivní připojení k vašemu modelu na vašem serveru.</span><span class="sxs-lookup"><span data-stu-id="72309-121">Create a Power BI Desktop file that has a live connection to your model on your server.</span></span>
2. <span data-ttu-id="72309-122">V [Power BI](https://powerbi.microsoft.com), klikněte na tlačítko **načíst Data** > **soubory**.</span><span class="sxs-lookup"><span data-stu-id="72309-122">In [Power BI](https://powerbi.microsoft.com), click **Get Data** > **Files**.</span></span> <span data-ttu-id="72309-123">Vyhledejte a vyberte soubor.</span><span class="sxs-lookup"><span data-stu-id="72309-123">Locate and select your file.</span></span>



## <a name="see-also"></a><span data-ttu-id="72309-124">Viz také</span><span class="sxs-lookup"><span data-stu-id="72309-124">See also</span></span>
<span data-ttu-id="72309-125">[Připojení ke službě Azure Analysis Services](analysis-services-connect.md) </span><span class="sxs-lookup"><span data-stu-id="72309-125">[Connect to Azure Analysis Services](analysis-services-connect.md) </span></span>  
[<span data-ttu-id="72309-126">Knihovny klienta</span><span class="sxs-lookup"><span data-stu-id="72309-126">Client libraries</span></span>](analysis-services-data-providers.md)

