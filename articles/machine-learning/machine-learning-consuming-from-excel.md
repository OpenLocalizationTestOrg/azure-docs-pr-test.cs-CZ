---
title: "Webová služba Machine Learning z Excelu aaaConsume | Microsoft Docs"
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
ms.openlocfilehash: e2e8bbf7ba75b6618a0285539555ce175ec03c1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="consuming-an-azure-machine-learning-web-service-from-excel"></a><span data-ttu-id="8f6c5-103">Využívání webové služby Azure Machine Learning z Excelu</span><span class="sxs-lookup"><span data-stu-id="8f6c5-103">Consuming an Azure Machine Learning Web Service from Excel</span></span>
 <span data-ttu-id="8f6c5-104">Azure Machine Learning Studio umožňuje snadno toocall webové služby přímo z aplikace Excel bez hello potřebovat toowrite žádný kód.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-104">Azure Machine Learning Studio makes it easy toocall web services directly from Excel without hello need toowrite any code.</span></span>

<span data-ttu-id="8f6c5-105">Pokud používáte aplikace Excel 2013 (nebo novější) nebo Excel Online, pak doporučujeme použít hello Excel [doplněk aplikace Excel](machine-learning-excel-add-in-for-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="8f6c5-105">If you are using Excel 2013 (or later) or Excel Online, then we recommend that you use hello Excel [Excel add-in](machine-learning-excel-add-in-for-web-services.md).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="steps"></a><span data-ttu-id="8f6c5-106">Kroky</span><span class="sxs-lookup"><span data-stu-id="8f6c5-106">Steps</span></span>
<span data-ttu-id="8f6c5-107">Publikování webové služby.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-107">Publish a web service.</span></span> <span data-ttu-id="8f6c5-108">[Tato stránka](machine-learning-walkthrough-5-publish-web-service.md) vysvětluje, jak toodo ho.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-108">[This page](machine-learning-walkthrough-5-publish-web-service.md) explains how toodo it.</span></span> <span data-ttu-id="8f6c5-109">Aktuálně funkce sešitu aplikace Excel hello je podporována pouze pro služby požadavků a odpovědí, které mají jediného výstupu (tedy jediný vyhodnocování štítek).</span><span class="sxs-lookup"><span data-stu-id="8f6c5-109">Currently hello Excel workbook feature is only supported for Request/Response services that have a single output (that is, a single scoring label).</span></span> 

<span data-ttu-id="8f6c5-110">Až budete mít webovou službu, klikněte na hello **webové služby** části na hello nalevo od hello studio a potom vyberte hello webové služby tooconsume z Excelu.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-110">Once you have a web service, click on hello **WEB SERVICES** section on hello left of hello studio, and then select hello web service tooconsume from Excel.</span></span>

<span data-ttu-id="8f6c5-111">**Classic webové služby**</span><span class="sxs-lookup"><span data-stu-id="8f6c5-111">**Classic Web Service**</span></span>

1. <span data-ttu-id="8f6c5-112">Na hello **řídicí panel** kartě hello webové služby je řádek pro hello **požadavků a odpovědí** služby.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-112">On hello **DASHBOARD** tab for hello web service is a row for hello **REQUEST/RESPONSE** service.</span></span> <span data-ttu-id="8f6c5-113">Pokud tato služba měli jediného výstupu, měli byste vidět hello **stáhnout sešitu aplikace Excel** odkaz v daném řádku.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-113">If this service had a single output, you should see hello **Download Excel Workbook** link in that row.</span></span>
   
    ![][1]
2. <span data-ttu-id="8f6c5-114">Klikněte na **stáhnout sešitu aplikace Excel**.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-114">Click on **Download Excel Workbook**.</span></span>

<span data-ttu-id="8f6c5-115">**Novou webovou službu**</span><span class="sxs-lookup"><span data-stu-id="8f6c5-115">**New Web Service**</span></span>

1. <span data-ttu-id="8f6c5-116">Webová služba Azure Machine Learning portálu hello, vyberte **spotřebě**.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-116">In hello Azure Machine Learning Web Service portal, select **Consume**.</span></span>
2. <span data-ttu-id="8f6c5-117">Na stránce spotřebě hello, hello **webové služby spotřeba možnosti** klikněte ikonu hello aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-117">On hello Consume page, in hello **Web service consumption options** section, click hello Excel icon.</span></span>

<span data-ttu-id="8f6c5-118">**Sešitem hello**</span><span class="sxs-lookup"><span data-stu-id="8f6c5-118">**Using hello workbook**</span></span>

1. <span data-ttu-id="8f6c5-119">Sešit otevřít hello.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-119">Open hello workbook.</span></span>
2. <span data-ttu-id="8f6c5-120">Zobrazí se upozornění zabezpečení; Klikněte na hello **povolit úpravy** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-120">A Security Warning appears; click on hello **Enable Editing** button.</span></span>
   
    ![][2]
3. <span data-ttu-id="8f6c5-121">Zobrazí se upozornění zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-121">A Security Warning appears.</span></span> <span data-ttu-id="8f6c5-122">Klikněte na hello **povolit obsah** tlačítko toorun makra tabulky.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-122">Click on hello **Enable Content** button toorun macros on your spreadsheet.</span></span>
   
    ![][3]
4. <span data-ttu-id="8f6c5-123">Jakmile jsou povolené makra, je generována tabulku.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-123">Once macros are enabled, a table is generated.</span></span> <span data-ttu-id="8f6c5-124">Sloupce v modré jsou požadované jako vstup do hello RRS webové služby, nebo **parametry**.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-124">Columns in blue are required as input into hello RRS web service, or **PARAMETERS**.</span></span> <span data-ttu-id="8f6c5-125">Všimněte si výstup hello hello RRS služby **PŘEDPOVĚDĚT hodnoty** zeleně.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-125">Note hello output of hello RRS service, **PREDICTED VALUES** in green.</span></span> <span data-ttu-id="8f6c5-126">Když jsou vyplněny všechny sloupce pro daný řádek, hello sešitu automaticky volá hello vyhodnocování rozhraní API a zobrazí hello skóre pro magnitudu výsledky.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-126">When all columns for a given row are filled, hello workbook automatically calls hello scoring API, and displays hello scored results.</span></span>
   
    ![][4]
5. <span data-ttu-id="8f6c5-127">tooscore více než jeden řádek, výplň hello druhý řádek s daty a hello předpovědět, že vytváří hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-127">tooscore more than one row, fill hello second row with data and hello predicted values are produced.</span></span> <span data-ttu-id="8f6c5-128">Několik řádků můžete vložit i najednou.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-128">You can even paste several rows at once.</span></span>

<span data-ttu-id="8f6c5-129">Můžete použít některou z funkcí aplikace Excel hello (grafů, power map, podmíněného formátování, atd.) s hello předpovědět hodnoty toohelp vizualizovat hello data.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-129">You can use any of hello Excel features (graphs, power map, conditional formatting, etc.) with hello predicted values toohelp visualize hello data.</span></span>    

## <a name="sharing-your-workbook"></a><span data-ttu-id="8f6c5-130">Sdílení sešitu</span><span class="sxs-lookup"><span data-stu-id="8f6c5-130">Sharing your workbook</span></span>
<span data-ttu-id="8f6c5-131">Klíč rozhraní API toowork hello makra, musí být součástí hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-131">For hello macros toowork, your API Key must be part of hello spreadsheet.</span></span> <span data-ttu-id="8f6c5-132">To znamená, že by měly sdílet hello sešitu pouze s entit nebo jednotlivce, kterým důvěřujete.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-132">That means that you should share hello workbook only with entities/individuals you trust.</span></span>

## <a name="automatic-updates"></a><span data-ttu-id="8f6c5-133">Automatické aktualizace</span><span class="sxs-lookup"><span data-stu-id="8f6c5-133">Automatic updates</span></span>
<span data-ttu-id="8f6c5-134">Volání na záznamy o prostředku se provádí v těchto dvou situacích:</span><span class="sxs-lookup"><span data-stu-id="8f6c5-134">An RRS call is made in these two situations:</span></span>

1. <span data-ttu-id="8f6c5-135">Hello poprvé řádek má obsah ve všech jeho **parametry**</span><span class="sxs-lookup"><span data-stu-id="8f6c5-135">hello first time a row has content in all of its **PARAMETERS**</span></span>
2. <span data-ttu-id="8f6c5-136">Kdykoli se některé z hello **parametry** změny v řádku, který měl všechny jeho **parametry** zadali.</span><span class="sxs-lookup"><span data-stu-id="8f6c5-136">Any time any of hello **PARAMETERS** changes in a row that had all of its **PARAMETERS** entered.</span></span>

[1]: ./media/machine-learning-consuming-from-excel/excellink.png
[2]: ./media/machine-learning-consuming-from-excel/enableeditting.png
[3]: ./media/machine-learning-consuming-from-excel/enablecontent.png
[4]: ./media/machine-learning-consuming-from-excel/sampletable.png
