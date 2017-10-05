---
title: "Transformace XML pomocí map XSLT - Azure Logic Apps | Microsoft Docs"
description: "Přidat, že XSLT mapuje transformace dat XML s Azure Logic Apps a Enterprise integračního balíčku"
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
ms.openlocfilehash: 4445a84a6c6425110e7d705019a28b5cc5447046
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-maps-for-xml-data-transform"></a><span data-ttu-id="3bd4d-103">Přidat mapy pro transformaci dat XML</span><span class="sxs-lookup"><span data-stu-id="3bd4d-103">Add maps for XML data transform</span></span>

<span data-ttu-id="3bd4d-104">Integrace organizace používá pro transformaci dat XML mezi formáty mapy.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-104">Enterprise integration uses maps to transform XML data between formats.</span></span> <span data-ttu-id="3bd4d-105">Mapu je dokument XML, který definuje data v dokumentu, který by měl být převedeny do jiného formátu.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-105">A map is an XML document that defines the data in a document that should be transformed into another format.</span></span> 

## <a name="why-use-maps"></a><span data-ttu-id="3bd4d-106">Proč používat mapy?</span><span class="sxs-lookup"><span data-stu-id="3bd4d-106">Why use maps?</span></span>

<span data-ttu-id="3bd4d-107">Předpokládejme, že pravidelně zobrazit B2B objednávky nebo faktury z zákazník, který používá formát YYYMMDD mezi daty.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-107">Suppose that you regularly receive B2B orders or invoices from a customer who uses the YYYMMDD format for dates.</span></span> <span data-ttu-id="3bd4d-108">Ale ve vaší organizaci, ukládat data ve formátu MMDDYYY.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-108">However, in your organization, you store dates in the MMDDYYY format.</span></span> <span data-ttu-id="3bd4d-109">Můžete použít mapu, která *transformace* formát data YYYMMDD do MMDDYYY před uložením podrobnosti objednávky nebo faktury v databázi zákazníků aktivity.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-109">You can use a map to *transform* the YYYMMDD date format into the MMDDYYY before storing the order or invoice details in your customer activity database.</span></span>

## <a name="how-do-i-create-a-map"></a><span data-ttu-id="3bd4d-110">Vytvoření mapy</span><span class="sxs-lookup"><span data-stu-id="3bd4d-110">How do I create a map?</span></span>

<span data-ttu-id="3bd4d-111">Můžete vytvořit projekty BizTalk integrace s [Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md "Další informace o integračního balíčku enterprise") pro Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-111">You can create BizTalk Integration projects with the [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Learn about the enterprise integration pack") for Visual Studio 2015.</span></span> <span data-ttu-id="3bd4d-112">Potom můžete vytvořit soubor mapa integrace pro, který umožňuje vizuálně položky mezi dva soubory schématu XML mapy.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-112">You can then create an Integration Map file that lets you visually map items between two XML schema files.</span></span> <span data-ttu-id="3bd4d-113">Po sestavení tohoto projektu, budete mít k dokumentu XSLT.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-113">After you build this project, you will have an XSLT document.</span></span>

## <a name="how-do-i-add-a-map"></a><span data-ttu-id="3bd4d-114">Jak přidat mapu?</span><span class="sxs-lookup"><span data-stu-id="3bd4d-114">How do I add a map?</span></span>

1. <span data-ttu-id="3bd4d-115">Na portálu Azure vyberte **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-115">In the Azure portal, select **Browse**.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. <span data-ttu-id="3bd4d-116">Do vyhledávacího pole filtru zadejte **integrace**, pak vyberte **účty pro integraci** ze seznamu výsledků.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-116">In the filter search box, enter **integration**, then select **Integration Accounts** from the results list.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. <span data-ttu-id="3bd4d-117">Vyberte účet integrace, ve které chcete přidat mapy.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-117">Select the integration account where you want to add the map.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. <span data-ttu-id="3bd4d-118">Vyberte **mapy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-118">Select the **Maps** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. <span data-ttu-id="3bd4d-119">Po otevření okna mapy, zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-119">After the Maps blade opens, choose **Add**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. <span data-ttu-id="3bd4d-120">Zadejte **název** pro mapu.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-120">Enter a **Name** for your map.</span></span> <span data-ttu-id="3bd4d-121">Pro nahrání souboru s mapováním, zvolte ikonu složky na pravé straně **mapy** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-121">To upload the map file, choose the folder icon on the right side of the **Map** text box.</span></span> <span data-ttu-id="3bd4d-122">Po dokončení procesu nahrávání, zvolte **OK**.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-122">After the upload process completes, choose **OK**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. <span data-ttu-id="3bd4d-123">Po Azure přidá ke svému účtu integrace mapy, obdržíte zprávu na obrazovce, která uvádí, zda byl přidán souboru mapy, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-123">After Azure adds the map to your integration account, you get an onscreen message that shows whether your map file was added or not.</span></span> <span data-ttu-id="3bd4d-124">Poté, co se vám tato zpráva, vyberte **mapy** dlaždici, aby se dala zobrazit nově přidané mapy.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-124">After you get this message, choose the **Maps** tile so you can view the newly added map.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)

## <a name="how-do-i-edit-a-map"></a><span data-ttu-id="3bd4d-125">Jak upravit mapu?</span><span class="sxs-lookup"><span data-stu-id="3bd4d-125">How do I edit a map?</span></span>

<span data-ttu-id="3bd4d-126">Musíte nahrát nový soubor mapy s změny, které chcete.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-126">You must upload a new map file with the changes that you want.</span></span> <span data-ttu-id="3bd4d-127">Nejprve si můžete stáhnout mapy pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-127">You can first download the map for editing.</span></span>

<span data-ttu-id="3bd4d-128">Pokud chcete nahrát nový mapu, která nahradí existující mapu, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-128">To upload a new map that replaces the existing map, follow these steps.</span></span>

1. <span data-ttu-id="3bd4d-129">Vyberte **mapy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-129">Choose the **Maps** tile.</span></span>

2. <span data-ttu-id="3bd4d-130">Po otevření okna mapy, vyberte, kterou chcete upravit mapování.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-130">After the Maps blade opens, select the map that you want to edit.</span></span>

3. <span data-ttu-id="3bd4d-131">Na **mapy** okně zvolte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-131">On the **Maps** blade, choose **Update**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. <span data-ttu-id="3bd4d-132">V dialogu pro výběr souborů vyberte soubor mapy, který chcete nahrát a pak vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-132">In the file picker, select the map file that you want to upload, then select **Open**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-to-delete-a-map"></a><span data-ttu-id="3bd4d-133">Postup odstranění mapy?</span><span class="sxs-lookup"><span data-stu-id="3bd4d-133">How to delete a map?</span></span>

1. <span data-ttu-id="3bd4d-134">Vyberte **mapy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-134">Choose the **Maps** tile.</span></span>

2. <span data-ttu-id="3bd4d-135">Po otevření okna mapy, vyberte mapování, které chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-135">After the Maps blade opens, select the map you want to delete.</span></span>

3. <span data-ttu-id="3bd4d-136">Zvolte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-136">Choose **Delete**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. <span data-ttu-id="3bd4d-137">Potvrďte, že chcete odstranit mapy.</span><span class="sxs-lookup"><span data-stu-id="3bd4d-137">Confirm that you want to delete the map.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a><span data-ttu-id="3bd4d-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3bd4d-138">Next Steps</span></span>
* [<span data-ttu-id="3bd4d-139">Další informace o integračního balíčku Enterprise</span><span class="sxs-lookup"><span data-stu-id="3bd4d-139">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")  
* [<span data-ttu-id="3bd4d-140">Další informace o smlouvy</span><span class="sxs-lookup"><span data-stu-id="3bd4d-140">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  
* [<span data-ttu-id="3bd4d-141">Další informace o transformací</span><span class="sxs-lookup"><span data-stu-id="3bd4d-141">Learn more about transforms</span></span>](logic-apps-enterprise-integration-transform.md "Další informace o integraci transformací enterprise")  

