---
title: "aaaConnect tooAzure služby Analysis Services s Power BI | Microsoft Docs"
description: "Zjistěte, jak tooconnect tooan Azure Analysis Services serveru pomocí Power BI."
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
ms.openlocfilehash: f6c4cdec6edb92900ad2e552e23a4d9172ba9b84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-with-power-bi"></a><span data-ttu-id="31c4c-103">Připojit k Power BI</span><span class="sxs-lookup"><span data-stu-id="31c4c-103">Connect with Power BI</span></span>

<span data-ttu-id="31c4c-104">Jakmile jste vytvořili serveru v Azure a nasadí tooit tabulkový model, uživatelé ve vaší organizaci jsou připravené tooconnect a začít zkoumat data.</span><span class="sxs-lookup"><span data-stu-id="31c4c-104">Once you've created a server in Azure, and deployed a tabular model tooit, users in your organization are ready tooconnect and begin exploring data.</span></span> 

> [!TIP]
> <span data-ttu-id="31c4c-105">Být jisti toouse hello nejnovější verzi [Power BI Desktop](https://powerbi.microsoft.com/desktop/).</span><span class="sxs-lookup"><span data-stu-id="31c4c-105">Be sure toouse hello latest version of [Power BI Desktop](https://powerbi.microsoft.com/desktop/).</span></span>
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a><span data-ttu-id="31c4c-106">Připojení v Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="31c4c-106">Connect in Power BI Desktop</span></span>

1. <span data-ttu-id="31c4c-107">V Power BI Desktop, klikněte na **načíst Data** > **Azure** > **databáze Azure Analysis Services**.</span><span class="sxs-lookup"><span data-stu-id="31c4c-107">In Power BI Desktop, click **Get Data** > **Azure** > **Azure Analysis Services database**.</span></span>

2. <span data-ttu-id="31c4c-108">V **Server**, zadejte název serveru hello.</span><span class="sxs-lookup"><span data-stu-id="31c4c-108">In **Server**, enter hello server name.</span></span> 
    
    <span data-ttu-id="31c4c-109">Být jisti tooinclude hello úplnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="31c4c-109">Be sure tooinclude hello full URL.</span></span> <span data-ttu-id="31c4c-110">Například asazure://westcentralus.asazure.windows.net/advworks.</span><span class="sxs-lookup"><span data-stu-id="31c4c-110">For example, asazure://westcentralus.asazure.windows.net/advworks.</span></span>

3. <span data-ttu-id="31c4c-111">V **databáze**, pokud znáte název hello hello databázi tabulkového modelu nebo perspektivy chcete tooconnect k, vložte jej zde.</span><span class="sxs-lookup"><span data-stu-id="31c4c-111">In **Database**, if you know hello name of hello tabular model database or perspective you want tooconnect to, paste it here.</span></span> <span data-ttu-id="31c4c-112">Jinak můžete ponechat toto pole prázdné a vybrat databázi nebo perspektivy později.</span><span class="sxs-lookup"><span data-stu-id="31c4c-112">Otherwise, you can leave this field blank and select a database or perspective later.</span></span>

4. <span data-ttu-id="31c4c-113">Ponechte výchozí hello **připojit za provozu** možnost a potom stiskněte klávesu **Connect**.</span><span class="sxs-lookup"><span data-stu-id="31c4c-113">Leave hello default **Connect live** option, then press **Connect**.</span></span> 

5. <span data-ttu-id="31c4c-114">Po zobrazení výzvy zadejte své přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="31c4c-114">If prompted, enter your login credentials.</span></span> 

6. <span data-ttu-id="31c4c-115">V **Navigátor**, rozbalte hello server a pak vyberte hello modelu nebo perspektivy chcete tooconnect k, pak klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="31c4c-115">In **Navigator**, expand hello server, then select hello model or perspective you want tooconnect to, then click **Connect**.</span></span> <span data-ttu-id="31c4c-116">Klikněte na tlačítko modelu nebo perspektivy tooshow všechny objekty hello tohoto zobrazení.</span><span class="sxs-lookup"><span data-stu-id="31c4c-116">Click  a model or perspective tooshow all hello objects for that view.</span></span>

    <span data-ttu-id="31c4c-117">Hello model se otevře v Power BI Desktop s prázdnou sestavou v zobrazení sestavy.</span><span class="sxs-lookup"><span data-stu-id="31c4c-117">hello model opens in Power BI Desktop with a blank report in Report view.</span></span> <span data-ttu-id="31c4c-118">Zobrazí se seznam polí Hello všechny objekty modelu nebyla skrytá.</span><span class="sxs-lookup"><span data-stu-id="31c4c-118">hello Fields list displays all non-hidden model objects.</span></span> <span data-ttu-id="31c4c-119">Stav připojení se zobrazí v pravém dolním rohu, hello.</span><span class="sxs-lookup"><span data-stu-id="31c4c-119">Connection status is displayed in hello lower-right corner.</span></span>

## <a name="connect-in-power-bi-service"></a><span data-ttu-id="31c4c-120">Připojení v Power BI (služba)</span><span class="sxs-lookup"><span data-stu-id="31c4c-120">Connect in Power BI (service)</span></span>

1. <span data-ttu-id="31c4c-121">Vytvořte soubor Power BI Desktop, který má model tooyour živé připojení na serveru.</span><span class="sxs-lookup"><span data-stu-id="31c4c-121">Create a Power BI Desktop file that has a live connection tooyour model on your server.</span></span>
2. <span data-ttu-id="31c4c-122">V [Power BI](https://powerbi.microsoft.com), klikněte na tlačítko **načíst Data** > **soubory**.</span><span class="sxs-lookup"><span data-stu-id="31c4c-122">In [Power BI](https://powerbi.microsoft.com), click **Get Data** > **Files**.</span></span> <span data-ttu-id="31c4c-123">Vyhledejte a vyberte soubor.</span><span class="sxs-lookup"><span data-stu-id="31c4c-123">Locate and select your file.</span></span>



## <a name="see-also"></a><span data-ttu-id="31c4c-124">Viz také</span><span class="sxs-lookup"><span data-stu-id="31c4c-124">See also</span></span>
<span data-ttu-id="31c4c-125">[Připojit tooAzure Analysis Services](analysis-services-connect.md) </span><span class="sxs-lookup"><span data-stu-id="31c4c-125">[Connect tooAzure Analysis Services](analysis-services-connect.md) </span></span>  
[<span data-ttu-id="31c4c-126">Knihovny klienta</span><span class="sxs-lookup"><span data-stu-id="31c4c-126">Client libraries</span></span>](analysis-services-data-providers.md)

