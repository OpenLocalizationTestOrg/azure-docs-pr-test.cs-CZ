---
title: "Schémata pro ověření XML - Azure Logic Apps | Microsoft Docs"
description: "Ověření XML – dokumenty s schémata pro Azure Logic Apps a Enterprise integračního balíčku"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 56c5846c-5d8c-4ad4-9652-60b07aa8fc3b
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 4f58a587c1f10aea1cee89e46fa9ec340e0d21c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="validate-xml-with-schemas-for-azure-logic-apps-and-the-enterprise-integration-pack"></a><span data-ttu-id="74694-103">Ověření XML s schémata pro Azure Logic Apps a Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="74694-103">Validate XML with schemas for Azure Logic Apps and the Enterprise Integration Pack</span></span>

<span data-ttu-id="74694-104">Schémata potvrdit, že dokumenty XML, které se zobrazí jsou platné a mít očekávaná data v předdefinovaném formátu.</span><span class="sxs-lookup"><span data-stu-id="74694-104">Schemas confirm that the XML documents you receive are valid and have the expected data in a predefined format.</span></span> <span data-ttu-id="74694-105">Schémata také pomůže ověřit zprávy, které se vyměňují ve scénáři B2B.</span><span class="sxs-lookup"><span data-stu-id="74694-105">Schemas also help validate messages that are exchanged in a B2B scenario.</span></span>

## <a name="add-a-schema"></a><span data-ttu-id="74694-106">Přidat schéma.</span><span class="sxs-lookup"><span data-stu-id="74694-106">Add a schema</span></span>

1. <span data-ttu-id="74694-107">Na portálu Azure vyberte **další služby**.</span><span class="sxs-lookup"><span data-stu-id="74694-107">In the Azure portal, select **More services**.</span></span>

    ![Portál Azure, "Další služby"](media/logic-apps-enterprise-integration-schemas/overview-11.png)

2. <span data-ttu-id="74694-109">Do vyhledávacího pole filtru zadejte **integrace**a vyberte **účty pro integraci** ze seznamu výsledků.</span><span class="sxs-lookup"><span data-stu-id="74694-109">In the filter search box, enter **integration**, and select **Integration Accounts** from the results list.</span></span>

    ![Pole filtru hledání](media/logic-apps-enterprise-integration-schemas/overview-21.png)

3. <span data-ttu-id="74694-111">Vyberte **integrace účet** ve které chcete přidat schéma.</span><span class="sxs-lookup"><span data-stu-id="74694-111">Select the **integration account** where you want to add the schema.</span></span>

    ![Seznam účtů, integrace](media/logic-apps-enterprise-integration-schemas/overview-31.png)

4. <span data-ttu-id="74694-113">Vyberte **schémata** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="74694-113">Choose the **Schemas** tile.</span></span>

    ![Příklad integrace účet "Schémata"](media/logic-apps-enterprise-integration-schemas/schema-11.png)

### <a name="add-a-schema-file-smaller-than-2-mb"></a><span data-ttu-id="74694-115">Přidejte soubor schématu, která je menší než 2 MB</span><span class="sxs-lookup"><span data-stu-id="74694-115">Add a schema file smaller than 2 MB</span></span>

1. <span data-ttu-id="74694-116">V **schémata** okno, které se otevře (z předchozího postupu), zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="74694-116">In the **Schemas** blade that opens (from the preceding steps), choose **Add**.</span></span>

    ![Schémata okně "Přidat"](media/logic-apps-enterprise-integration-schemas/schema-21.png)

2. <span data-ttu-id="74694-118">Zadejte název pro schéma.</span><span class="sxs-lookup"><span data-stu-id="74694-118">Enter a name for your schema.</span></span> <span data-ttu-id="74694-119">Nahrání souboru schématu výběrem ikony složku do **schématu** pole.</span><span class="sxs-lookup"><span data-stu-id="74694-119">Upload the schema file by selecting the folder icon next to the **Schema** box.</span></span> <span data-ttu-id="74694-120">Po dokončení procesu nahrávání, vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="74694-120">After the upload process completes, select **OK**.</span></span>

    ![Snímek obrazovky "Přidat schéma", se zvýrazněnou "malý soubor"](media/logic-apps-enterprise-integration-schemas/schema-31.png)

### <a name="add-a-schema-file-larger-than-2-mb-up-to-8-mb-maximum"></a><span data-ttu-id="74694-122">Přidejte soubor schématu, která je větší než 2 MB (maximálně 8 MB)</span><span class="sxs-lookup"><span data-stu-id="74694-122">Add a schema file larger than 2 MB (up to 8 MB maximum)</span></span>

<span data-ttu-id="74694-123">Na úrovni přístupu kontejneru objektu blob se liší podle těchto kroků: **veřejné** nebo **žádné anonymní přístup**.</span><span class="sxs-lookup"><span data-stu-id="74694-123">These steps differ based on the blob container access level: **Public** or **No anonymous access**.</span></span>

<span data-ttu-id="74694-124">**Chcete-li zjistit tato úroveň přístupu**</span><span class="sxs-lookup"><span data-stu-id="74694-124">**To determine this access level**</span></span>

1.  <span data-ttu-id="74694-125">Otevřete **Azure Storage Explorer**.</span><span class="sxs-lookup"><span data-stu-id="74694-125">Open **Azure Storage Explorer**.</span></span> 

2.  <span data-ttu-id="74694-126">V části **kontejnery objektů Blob**, vyberte kontejner objektů blob, který chcete.</span><span class="sxs-lookup"><span data-stu-id="74694-126">Under **Blob Containers**, select the blob container you want.</span></span> 

3.  <span data-ttu-id="74694-127">Vyberte **zabezpečení**, **úroveň přístupu**.</span><span class="sxs-lookup"><span data-stu-id="74694-127">Select **Security**, **Access Level**.</span></span>

<span data-ttu-id="74694-128">Pokud je úroveň zabezpečení přístupu k objektu blob **veřejné**, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="74694-128">If the blob security access level is **Public**, follow these steps.</span></span>

![Azure Storage Explorer "Kontejnery objektů Blob", "Zabezpečení" a "Veřejná" zvýrazněná](media/logic-apps-enterprise-integration-schemas/blob-public.png)

1. <span data-ttu-id="74694-130">Odeslat schématu do svého účtu úložiště a zkopírujte identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="74694-130">Upload the schema to your storage account, and copy the URI.</span></span>

    ![Účet úložiště se zvýrazněnou identifikátor URI](media/logic-apps-enterprise-integration-schemas/schema-blob.png)

2. <span data-ttu-id="74694-132">V **přidat schéma**, vyberte **velkých souborů**a zadejte identifikátor URI v **obsahu URI** textové pole.</span><span class="sxs-lookup"><span data-stu-id="74694-132">In **Add Schema**, select **Large file**, and provide the URI in the **Content URI** text box.</span></span>

    ![Schémata s tlačítko "Přidat" a "Velkých souborů" zvýrazněná](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

<span data-ttu-id="74694-134">Pokud je úroveň zabezpečení přístupu k objektu blob **žádné anonymní přístup**, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="74694-134">If the blob security access level is **No anonymous access**, follow these steps.</span></span>

![Azure Storage Explorer "Kontejnery objektů Blob", "Zabezpečení" a "Žádná anonymní přístup" zvýrazněná](media/logic-apps-enterprise-integration-schemas/blob-1.png)

1. <span data-ttu-id="74694-136">Schéma odešlete do svého účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="74694-136">Upload the schema to your storage account.</span></span>

    ![Účet úložiště](media/logic-apps-enterprise-integration-schemas/blob-3.png)

2. <span data-ttu-id="74694-138">Vygenerujte sdílený přístupový podpis pro schéma.</span><span class="sxs-lookup"><span data-stu-id="74694-138">Generate a shared access signature for the schema.</span></span>

    ![Účet úložiště se zvýrazněnou kartu podpisy sdíleného přístupu](media/logic-apps-enterprise-integration-schemas/blob-2.png)

3. <span data-ttu-id="74694-140">V **přidat schéma**, vyberte **velkých souborů**a zadejte sdílený přístupový podpis URI v **obsahu URI** textové pole.</span><span class="sxs-lookup"><span data-stu-id="74694-140">In **Add Schema**, select **Large file**, and provide the shared access signature URI in the **Content URI** text box.</span></span>

    ![Schémata s tlačítko "Přidat" a "Velkých souborů" zvýrazněná](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

4. <span data-ttu-id="74694-142">V **schémata** okno účtu integrace, by se měla zobrazit vaše nově přidané schéma.</span><span class="sxs-lookup"><span data-stu-id="74694-142">In the **Schemas** blade of your integration account, your newly added schema should appear.</span></span>

    ![Účtu integrace s "Schémat" a nové schéma zvýrazněná](media/logic-apps-enterprise-integration-schemas/schema-41.png)

## <a name="edit-schemas"></a><span data-ttu-id="74694-144">Upravit schémat.</span><span class="sxs-lookup"><span data-stu-id="74694-144">Edit schemas</span></span>

1. <span data-ttu-id="74694-145">Vyberte **schémata** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="74694-145">Choose the **Schemas** tile.</span></span>

2. <span data-ttu-id="74694-146">Po **schémata** otevře se okno, vyberte schéma, které chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="74694-146">After the **Schemas** blade opens, select the schema that you want to edit.</span></span>

3. <span data-ttu-id="74694-147">Na **schémata** okně zvolte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="74694-147">On the **Schemas** blade, choose **Edit**.</span></span>

    ![Okno schémat.](media/logic-apps-enterprise-integration-schemas/edit-12.png)

4. <span data-ttu-id="74694-149">Vyberte soubor schématu, který chcete upravit a pak vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="74694-149">Select the schema file that you want to edit, then select **Open**.</span></span>

    ![Soubor otevřete schématu upravit](media/logic-apps-enterprise-integration-schemas/edit-31.png)

<span data-ttu-id="74694-151">Azure zobrazí zpráva, že schéma se úspěšně nahrál.</span><span class="sxs-lookup"><span data-stu-id="74694-151">Azure shows a message that the schema uploaded successfully.</span></span>

## <a name="delete-schemas"></a><span data-ttu-id="74694-152">Odstranit schémat.</span><span class="sxs-lookup"><span data-stu-id="74694-152">Delete schemas</span></span>

1. <span data-ttu-id="74694-153">Vyberte **schémata** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="74694-153">Choose the **Schemas** tile.</span></span>

2. <span data-ttu-id="74694-154">Po **schémata** otevře se okno, vyberte schéma, které chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="74694-154">After the **Schemas** blade opens, select the schema you want to delete.</span></span>

3. <span data-ttu-id="74694-155">Na **schémata** okně zvolte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="74694-155">On the **Schemas** blade, choose **Delete**.</span></span>

    ![Okno schémat.](media/logic-apps-enterprise-integration-schemas/delete-12.png)

4. <span data-ttu-id="74694-157">Pokud chcete potvrdit, že chcete odstranit vybrané schéma, zvolte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="74694-157">To confirm that you want to delete the selected schema, choose **Yes**.</span></span>

    ![Potvrzovací zpráva "Odstranit schématu"](media/logic-apps-enterprise-integration-schemas/delete-21.png)

    <span data-ttu-id="74694-159">V **schémata** okně seznamu schématu aktualizuje a už obsahuje schéma, které jste odstranili.</span><span class="sxs-lookup"><span data-stu-id="74694-159">In the **Schemas** blade, the schema list refreshes  and no longer includes the schema that you deleted.</span></span>

    ![Svoji integraci účet s "Schémata" zvýrazněná](media/logic-apps-enterprise-integration-schemas/delete-31.png)

## <a name="next-steps"></a><span data-ttu-id="74694-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74694-161">Next steps</span></span>
* <span data-ttu-id="74694-162">[Další informace o integračního balíčku Enterprise](logic-apps-enterprise-integration-overview.md "Další informace o integračního balíčku enterprise").</span><span class="sxs-lookup"><span data-stu-id="74694-162">[Learn more about the Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Learn about the enterprise integration pack").</span></span>  

