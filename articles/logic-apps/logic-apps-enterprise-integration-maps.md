---
title: aaaTransform XML s XSLT mapy - Azure Logic Apps | Microsoft Docs
description: "Přidat, že XSLT mapuje tootransform data XML s Azure Logic Apps a hello Enterprise integračního balíčku"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 720e6f988b8542136dfcc402c3c463fcfb2f23cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-maps-for-xml-data-transform"></a><span data-ttu-id="8499c-103">Přidat mapy pro transformaci dat XML</span><span class="sxs-lookup"><span data-stu-id="8499c-103">Add maps for XML data transform</span></span>

<span data-ttu-id="8499c-104">Používá integrace Enterprise mapuje data XML tootransform mezi formáty.</span><span class="sxs-lookup"><span data-stu-id="8499c-104">Enterprise integration uses maps tootransform XML data between formats.</span></span> <span data-ttu-id="8499c-105">Mapu je dokument XML, který definuje hello data v dokumentu, který by měl být převedeny do jiného formátu.</span><span class="sxs-lookup"><span data-stu-id="8499c-105">A map is an XML document that defines hello data in a document that should be transformed into another format.</span></span> 

## <a name="why-use-maps"></a><span data-ttu-id="8499c-106">Proč používat mapy?</span><span class="sxs-lookup"><span data-stu-id="8499c-106">Why use maps?</span></span>

<span data-ttu-id="8499c-107">Předpokládejme, že pravidelně zobrazit B2B objednávky nebo faktury z zákazník, který používá formát YYYMMDD hello mezi daty.</span><span class="sxs-lookup"><span data-stu-id="8499c-107">Suppose that you regularly receive B2B orders or invoices from a customer who uses hello YYYMMDD format for dates.</span></span> <span data-ttu-id="8499c-108">Ale ve vaší organizaci, ukládat data ve formátu MMDDYYY hello.</span><span class="sxs-lookup"><span data-stu-id="8499c-108">However, in your organization, you store dates in hello MMDDYYY format.</span></span> <span data-ttu-id="8499c-109">Můžete použít mapování příliš*transformace* hello YYYMMDD formát data do hello MMDDYYY před uložením hello objednávky nebo faktury podrobnosti v databázi zákazníků aktivity.</span><span class="sxs-lookup"><span data-stu-id="8499c-109">You can use a map too*transform* hello YYYMMDD date format into hello MMDDYYY before storing hello order or invoice details in your customer activity database.</span></span>

## <a name="how-do-i-create-a-map"></a><span data-ttu-id="8499c-110">Vytvoření mapy</span><span class="sxs-lookup"><span data-stu-id="8499c-110">How do I create a map?</span></span>

<span data-ttu-id="8499c-111">Můžete vytvořit projekty BizTalk integrace s hello [Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md "Další informace o hello enterprise integračního balíčku") pro Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="8499c-111">You can create BizTalk Integration projects with hello [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Learn about hello enterprise integration pack") for Visual Studio 2015.</span></span> <span data-ttu-id="8499c-112">Potom můžete vytvořit soubor mapa integrace pro, který umožňuje vizuálně položky mezi dva soubory schématu XML mapy.</span><span class="sxs-lookup"><span data-stu-id="8499c-112">You can then create an Integration Map file that lets you visually map items between two XML schema files.</span></span> <span data-ttu-id="8499c-113">Po sestavení tohoto projektu, budete mít k dokumentu XSLT.</span><span class="sxs-lookup"><span data-stu-id="8499c-113">After you build this project, you will have an XSLT document.</span></span>

## <a name="how-do-i-add-a-map"></a><span data-ttu-id="8499c-114">Jak přidat mapu?</span><span class="sxs-lookup"><span data-stu-id="8499c-114">How do I add a map?</span></span>

1. <span data-ttu-id="8499c-115">V hello portálu Azure, vyberte **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="8499c-115">In hello Azure portal, select **Browse**.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. <span data-ttu-id="8499c-116">Hello filtru vyhledávacího pole zadejte **integrace**, pak vyberte **účty pro integraci** ze seznamu výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="8499c-116">In hello filter search box, enter **integration**, then select **Integration Accounts** from hello results list.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. <span data-ttu-id="8499c-117">Vyberte účet integrace hello místo tooadd hello mapy.</span><span class="sxs-lookup"><span data-stu-id="8499c-117">Select hello integration account where you want tooadd hello map.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. <span data-ttu-id="8499c-118">Vyberte hello **mapy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="8499c-118">Select hello **Maps** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. <span data-ttu-id="8499c-119">Po otevření okna mapy hello zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="8499c-119">After hello Maps blade opens, choose **Add**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. <span data-ttu-id="8499c-120">Zadejte **název** pro mapu.</span><span class="sxs-lookup"><span data-stu-id="8499c-120">Enter a **Name** for your map.</span></span> <span data-ttu-id="8499c-121">Mapa hello tooupload souboru, vyberte ikonu složky hello na pravé straně hello hello **mapy** textové pole.</span><span class="sxs-lookup"><span data-stu-id="8499c-121">tooupload hello map file, choose hello folder icon on hello right side of hello **Map** text box.</span></span> <span data-ttu-id="8499c-122">Po dokončení procesu nahrávání hello zvolte **OK**.</span><span class="sxs-lookup"><span data-stu-id="8499c-122">After hello upload process completes, choose **OK**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. <span data-ttu-id="8499c-123">Po Azure přidá hello mapy tooyour integrace účet, obdržíte zprávu na obrazovce, která uvádí, zda byl přidán souboru mapy, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="8499c-123">After Azure adds hello map tooyour integration account, you get an onscreen message that shows whether your map file was added or not.</span></span> <span data-ttu-id="8499c-124">Poté, co se vám tato zpráva, zvolte hello **mapy** dlaždice, aby se dala zobrazit hello nově přidány mapy.</span><span class="sxs-lookup"><span data-stu-id="8499c-124">After you get this message, choose hello **Maps** tile so you can view hello newly added map.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)

## <a name="how-do-i-edit-a-map"></a><span data-ttu-id="8499c-125">Jak upravit mapu?</span><span class="sxs-lookup"><span data-stu-id="8499c-125">How do I edit a map?</span></span>

<span data-ttu-id="8499c-126">Musíte nahrát nový soubor s hello změny, které chcete mapy.</span><span class="sxs-lookup"><span data-stu-id="8499c-126">You must upload a new map file with hello changes that you want.</span></span> <span data-ttu-id="8499c-127">Nejprve si můžete stáhnout hello mapy pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="8499c-127">You can first download hello map for editing.</span></span>

<span data-ttu-id="8499c-128">tooupload nové mapu, která nahradí existující mapu hello, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="8499c-128">tooupload a new map that replaces hello existing map, follow these steps.</span></span>

1. <span data-ttu-id="8499c-129">Zvolte hello **mapy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="8499c-129">Choose hello **Maps** tile.</span></span>

2. <span data-ttu-id="8499c-130">Po otevření okna mapy hello vyberte hello mapy, které chcete tooedit.</span><span class="sxs-lookup"><span data-stu-id="8499c-130">After hello Maps blade opens, select hello map that you want tooedit.</span></span>

3. <span data-ttu-id="8499c-131">Na hello **mapy** okně zvolte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="8499c-131">On hello **Maps** blade, choose **Update**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. <span data-ttu-id="8499c-132">V hello pro výběr souborů, vyberte soubor s mapováním hello má tooupload a pak vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="8499c-132">In hello file picker, select hello map file that you want tooupload, then select **Open**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-toodelete-a-map"></a><span data-ttu-id="8499c-133">Jak toodelete mapu?</span><span class="sxs-lookup"><span data-stu-id="8499c-133">How toodelete a map?</span></span>

1. <span data-ttu-id="8499c-134">Zvolte hello **mapy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="8499c-134">Choose hello **Maps** tile.</span></span>

2. <span data-ttu-id="8499c-135">Po otevření okna mapy hello vyberte mapu hello chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="8499c-135">After hello Maps blade opens, select hello map you want toodelete.</span></span>

3. <span data-ttu-id="8499c-136">Zvolte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="8499c-136">Choose **Delete**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. <span data-ttu-id="8499c-137">Potvrďte, že chcete toodelete hello mapy.</span><span class="sxs-lookup"><span data-stu-id="8499c-137">Confirm that you want toodelete hello map.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a><span data-ttu-id="8499c-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8499c-138">Next Steps</span></span>
* [<span data-ttu-id="8499c-139">Další informace o hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="8499c-139">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")  
* [<span data-ttu-id="8499c-140">Další informace o smlouvy</span><span class="sxs-lookup"><span data-stu-id="8499c-140">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  
* [<span data-ttu-id="8499c-141">Další informace o transformací</span><span class="sxs-lookup"><span data-stu-id="8499c-141">Learn more about transforms</span></span>](logic-apps-enterprise-integration-transform.md "Další informace o integraci transformací enterprise")  

