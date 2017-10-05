---
title: "Webová služba Machine Learning z Excelu využívat | Microsoft Docs"
description: "Využívají webové služby Azure Machine Learning z Excelu"
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: cgronlun
ms.assetid: 3f3cdd2f-1816-487e-ab78-530e01e9788f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/13/2017
ms.author: tedway
ms.openlocfilehash: 9f1aac04d54221888ee9374317be339400dcf085
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="consuming-an-azure-machine-learning-web-service-from-excel"></a><span data-ttu-id="c9101-103">Využívání webové služby Azure Machine Learning z Excelu</span><span class="sxs-lookup"><span data-stu-id="c9101-103">Consuming an Azure Machine Learning Web Service from Excel</span></span>
 <span data-ttu-id="c9101-104">Azure Machine Learning Studio usnadňuje volání webové služby přímo z aplikace Excel bez nutnosti psaní jakéhokoli kódu.</span><span class="sxs-lookup"><span data-stu-id="c9101-104">Azure Machine Learning Studio makes it easy to call web services directly from Excel without the need to write any code.</span></span>

<span data-ttu-id="c9101-105">Pokud používáte aplikace Excel 2013 (nebo novější) nebo Excel Online, pak doporučujeme použít v aplikaci Excel [doplněk aplikace Excel](machine-learning-excel-add-in-for-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="c9101-105">If you are using Excel 2013 (or later) or Excel Online, then we recommend that you use the Excel [Excel add-in](machine-learning-excel-add-in-for-web-services.md).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="steps"></a><span data-ttu-id="c9101-106">Kroky</span><span class="sxs-lookup"><span data-stu-id="c9101-106">Steps</span></span>
<span data-ttu-id="c9101-107">Publikování webové služby.</span><span class="sxs-lookup"><span data-stu-id="c9101-107">Publish a web service.</span></span> <span data-ttu-id="c9101-108">[Tato stránka](machine-learning-walkthrough-5-publish-web-service.md) vysvětluje, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="c9101-108">[This page](machine-learning-walkthrough-5-publish-web-service.md) explains how to do it.</span></span> <span data-ttu-id="c9101-109">Funkce sešitu aplikace Excel se aktuálně podporuje jen pro služby požadavků a odpovědí, které mají jediného výstupu (tedy jediný vyhodnocování štítek).</span><span class="sxs-lookup"><span data-stu-id="c9101-109">Currently the Excel workbook feature is only supported for Request/Response services that have a single output (that is, a single scoring label).</span></span> 

<span data-ttu-id="c9101-110">Až budete mít webovou službu, klikněte na **webové služby** části na levé straně nástroje studio a potom vyberte webovou službu využívat z Excelu.</span><span class="sxs-lookup"><span data-stu-id="c9101-110">Once you have a web service, click on the **WEB SERVICES** section on the left of the studio, and then select the web service to consume from Excel.</span></span>

<span data-ttu-id="c9101-111">**Classic webové služby**</span><span class="sxs-lookup"><span data-stu-id="c9101-111">**Classic Web Service**</span></span>

1. <span data-ttu-id="c9101-112">Na **řídicí panel** kartě pro webovou službu je řádek pro **požadavků a odpovědí** služby.</span><span class="sxs-lookup"><span data-stu-id="c9101-112">On the **DASHBOARD** tab for the web service is a row for the **REQUEST/RESPONSE** service.</span></span> <span data-ttu-id="c9101-113">Pokud tato služba měli jediného výstupu, měli byste vidět **stáhnout sešitu aplikace Excel** odkaz v daném řádku.</span><span class="sxs-lookup"><span data-stu-id="c9101-113">If this service had a single output, you should see the **Download Excel Workbook** link in that row.</span></span>
   
    ![][1]
2. <span data-ttu-id="c9101-114">Klikněte na **stáhnout sešitu aplikace Excel**.</span><span class="sxs-lookup"><span data-stu-id="c9101-114">Click on **Download Excel Workbook**.</span></span>

<span data-ttu-id="c9101-115">**Novou webovou službu**</span><span class="sxs-lookup"><span data-stu-id="c9101-115">**New Web Service**</span></span>

1. <span data-ttu-id="c9101-116">Na portálu Azure Machine Learning webové služby, vyberte **spotřebě**.</span><span class="sxs-lookup"><span data-stu-id="c9101-116">In the Azure Machine Learning Web Service portal, select **Consume**.</span></span>
2. <span data-ttu-id="c9101-117">Na stránce spotřebě v **webové služby spotřeba možnosti** části, klikněte na ikonu aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="c9101-117">On the Consume page, in the **Web service consumption options** section, click the Excel icon.</span></span>

<span data-ttu-id="c9101-118">**V práci se sešitem**</span><span class="sxs-lookup"><span data-stu-id="c9101-118">**Using the workbook**</span></span>

1. <span data-ttu-id="c9101-119">Otevřete sešit.</span><span class="sxs-lookup"><span data-stu-id="c9101-119">Open the workbook.</span></span>
2. <span data-ttu-id="c9101-120">Zobrazí se upozornění zabezpečení; Klikněte na **povolit úpravy** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c9101-120">A Security Warning appears; click on the **Enable Editing** button.</span></span>
   
    ![][2]
3. <span data-ttu-id="c9101-121">Zobrazí se upozornění zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c9101-121">A Security Warning appears.</span></span> <span data-ttu-id="c9101-122">Klikněte na **povolit obsah** tlačítko spustit makra tabulky.</span><span class="sxs-lookup"><span data-stu-id="c9101-122">Click on the **Enable Content** button to run macros on your spreadsheet.</span></span>
   
    ![][3]
4. <span data-ttu-id="c9101-123">Jakmile jsou povolené makra, je generována tabulku.</span><span class="sxs-lookup"><span data-stu-id="c9101-123">Once macros are enabled, a table is generated.</span></span> <span data-ttu-id="c9101-124">Sloupce v modré jsou požadované jako vstup do webovou službu RRS nebo **parametry**.</span><span class="sxs-lookup"><span data-stu-id="c9101-124">Columns in blue are required as input into the RRS web service, or **PARAMETERS**.</span></span> <span data-ttu-id="c9101-125">Všimněte si výstup RRS služby **PŘEDPOVĚDĚT hodnoty** zeleně.</span><span class="sxs-lookup"><span data-stu-id="c9101-125">Note the output of the RRS service, **PREDICTED VALUES** in green.</span></span> <span data-ttu-id="c9101-126">Když jsou vyplněny všechny sloupce pro daný řádek, sešit automaticky zavolá rozhraní API vyhodnocování a zobrazí scored výsledky.</span><span class="sxs-lookup"><span data-stu-id="c9101-126">When all columns for a given row are filled, the workbook automatically calls the scoring API, and displays the scored results.</span></span>
   
    ![][4]
5. <span data-ttu-id="c9101-127">Ke stanovení skóre více než jeden řádek, vznikají výplně druhý řádek s daty a předpovězené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c9101-127">To score more than one row, fill the second row with data and the predicted values are produced.</span></span> <span data-ttu-id="c9101-128">Několik řádků můžete vložit i najednou.</span><span class="sxs-lookup"><span data-stu-id="c9101-128">You can even paste several rows at once.</span></span>

<span data-ttu-id="c9101-129">Žádnou z funkcí aplikace Excel (grafů, map napájení, podmíněného formátování atd.) můžete použít s předpovězené hodnoty můžete vizualizovat data.</span><span class="sxs-lookup"><span data-stu-id="c9101-129">You can use any of the Excel features (graphs, power map, conditional formatting, etc.) with the predicted values to help visualize the data.</span></span>    

## <a name="sharing-your-workbook"></a><span data-ttu-id="c9101-130">Sdílení sešitu</span><span class="sxs-lookup"><span data-stu-id="c9101-130">Sharing your workbook</span></span>
<span data-ttu-id="c9101-131">Makra pro práci musí být klíč rozhraní API součást tabulky.</span><span class="sxs-lookup"><span data-stu-id="c9101-131">For the macros to work, your API Key must be part of the spreadsheet.</span></span> <span data-ttu-id="c9101-132">To znamená, že by měly sdílet sešit pouze s entit nebo jednotlivce, kterým důvěřujete.</span><span class="sxs-lookup"><span data-stu-id="c9101-132">That means that you should share the workbook only with entities/individuals you trust.</span></span>

## <a name="automatic-updates"></a><span data-ttu-id="c9101-133">Automatické aktualizace</span><span class="sxs-lookup"><span data-stu-id="c9101-133">Automatic updates</span></span>
<span data-ttu-id="c9101-134">Volání na záznamy o prostředku se provádí v těchto dvou situacích:</span><span class="sxs-lookup"><span data-stu-id="c9101-134">An RRS call is made in these two situations:</span></span>

1. <span data-ttu-id="c9101-135">První řádek má obsah ve všech jeho **parametry**</span><span class="sxs-lookup"><span data-stu-id="c9101-135">The first time a row has content in all of its **PARAMETERS**</span></span>
2. <span data-ttu-id="c9101-136">Kdykoli se některé z **parametry** změny v řádku, který měl všechny jeho **parametry** zadali.</span><span class="sxs-lookup"><span data-stu-id="c9101-136">Any time any of the **PARAMETERS** changes in a row that had all of its **PARAMETERS** entered.</span></span>

[1]: ./media/machine-learning-consuming-from-excel/excellink.png
[2]: ./media/machine-learning-consuming-from-excel/enableeditting.png
[3]: ./media/machine-learning-consuming-from-excel/enablecontent.png
[4]: ./media/machine-learning-consuming-from-excel/sampletable.png
