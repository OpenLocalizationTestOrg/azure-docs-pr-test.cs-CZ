---
title: "aaaSchemas pro ověření XML - Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 87cf92741e10ff7cccd260f27442909e34928903
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="validate-xml-with-schemas-for-azure-logic-apps-and-hello-enterprise-integration-pack"></a><span data-ttu-id="60f4e-103">Ověření XML s schémata pro Azure Logic Apps a hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="60f4e-103">Validate XML with schemas for Azure Logic Apps and hello Enterprise Integration Pack</span></span>

<span data-ttu-id="60f4e-104">Schémata potvrdit, že jsou platné hello XML – dokumenty, které vám a obsahují hello očekávané data v předdefinovaném formátu.</span><span class="sxs-lookup"><span data-stu-id="60f4e-104">Schemas confirm that hello XML documents you receive are valid and have hello expected data in a predefined format.</span></span> <span data-ttu-id="60f4e-105">Schémata také pomůže ověřit zprávy, které se vyměňují ve scénáři B2B.</span><span class="sxs-lookup"><span data-stu-id="60f4e-105">Schemas also help validate messages that are exchanged in a B2B scenario.</span></span>

## <a name="add-a-schema"></a><span data-ttu-id="60f4e-106">Přidat schéma.</span><span class="sxs-lookup"><span data-stu-id="60f4e-106">Add a schema</span></span>

1. <span data-ttu-id="60f4e-107">V hello portálu Azure, vyberte **další služby**.</span><span class="sxs-lookup"><span data-stu-id="60f4e-107">In hello Azure portal, select **More services**.</span></span>

    ![Portál Azure, "Další služby"](media/logic-apps-enterprise-integration-schemas/overview-11.png)

2. <span data-ttu-id="60f4e-109">Hello filtru vyhledávacího pole zadejte **integrace**a vyberte **účty pro integraci** ze seznamu výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="60f4e-109">In hello filter search box, enter **integration**, and select **Integration Accounts** from hello results list.</span></span>

    ![Pole filtru hledání](media/logic-apps-enterprise-integration-schemas/overview-21.png)

3. <span data-ttu-id="60f4e-111">Vyberte hello **integrace účet** místo tooadd hello schématu.</span><span class="sxs-lookup"><span data-stu-id="60f4e-111">Select hello **integration account** where you want tooadd hello schema.</span></span>

    ![Seznam účtů, integrace](media/logic-apps-enterprise-integration-schemas/overview-31.png)

4. <span data-ttu-id="60f4e-113">Zvolte hello **schémata** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="60f4e-113">Choose hello **Schemas** tile.</span></span>

    ![Příklad integrace účet "Schémata"](media/logic-apps-enterprise-integration-schemas/schema-11.png)

### <a name="add-a-schema-file-smaller-than-2-mb"></a><span data-ttu-id="60f4e-115">Přidejte soubor schématu, která je menší než 2 MB</span><span class="sxs-lookup"><span data-stu-id="60f4e-115">Add a schema file smaller than 2 MB</span></span>

1. <span data-ttu-id="60f4e-116">V hello **schémata** okno, které se otevře (z hello předchozích kroků), zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="60f4e-116">In hello **Schemas** blade that opens (from hello preceding steps), choose **Add**.</span></span>

    ![Schémata okně "Přidat"](media/logic-apps-enterprise-integration-schemas/schema-21.png)

2. <span data-ttu-id="60f4e-118">Zadejte název pro schéma.</span><span class="sxs-lookup"><span data-stu-id="60f4e-118">Enter a name for your schema.</span></span> <span data-ttu-id="60f4e-119">Nahrát soubor schématu hello výběrem hello složky ikonu další toohello **schématu** pole.</span><span class="sxs-lookup"><span data-stu-id="60f4e-119">Upload hello schema file by selecting hello folder icon next toohello **Schema** box.</span></span> <span data-ttu-id="60f4e-120">Po dokončení procesu nahrávání hello vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="60f4e-120">After hello upload process completes, select **OK**.</span></span>

    ![Snímek obrazovky "Přidat schéma", se zvýrazněnou "malý soubor"](media/logic-apps-enterprise-integration-schemas/schema-31.png)

### <a name="add-a-schema-file-larger-than-2-mb-up-too8-mb-maximum"></a><span data-ttu-id="60f4e-122">Přidejte soubor schématu, která je větší než 2 MB (až too8 MB maximum)</span><span class="sxs-lookup"><span data-stu-id="60f4e-122">Add a schema file larger than 2 MB (up too8 MB maximum)</span></span>

<span data-ttu-id="60f4e-123">Tyto kroky se liší v závislosti na úroveň přístupu kontejneru objektů blob hello: **veřejné** nebo **žádné anonymní přístup**.</span><span class="sxs-lookup"><span data-stu-id="60f4e-123">These steps differ based on hello blob container access level: **Public** or **No anonymous access**.</span></span>

<span data-ttu-id="60f4e-124">**toodetermine tato úroveň přístupu**</span><span class="sxs-lookup"><span data-stu-id="60f4e-124">**toodetermine this access level**</span></span>

1.  <span data-ttu-id="60f4e-125">Otevřete **Azure Storage Explorer**.</span><span class="sxs-lookup"><span data-stu-id="60f4e-125">Open **Azure Storage Explorer**.</span></span> 

2.  <span data-ttu-id="60f4e-126">V části **kontejnery objektů Blob**, vyberte možnost kontejner objektů blob hello chcete.</span><span class="sxs-lookup"><span data-stu-id="60f4e-126">Under **Blob Containers**, select hello blob container you want.</span></span> 

3.  <span data-ttu-id="60f4e-127">Vyberte **zabezpečení**, **úroveň přístupu**.</span><span class="sxs-lookup"><span data-stu-id="60f4e-127">Select **Security**, **Access Level**.</span></span>

<span data-ttu-id="60f4e-128">Pokud je úroveň přístupu objektu blob hello **veřejné**, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="60f4e-128">If hello blob security access level is **Public**, follow these steps.</span></span>

![Azure Storage Explorer "Kontejnery objektů Blob", "Zabezpečení" a "Veřejná" zvýrazněná](media/logic-apps-enterprise-integration-schemas/blob-public.png)

1. <span data-ttu-id="60f4e-130">Nahrát účet úložiště tooyour hello schématu a zkopírujte hello identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="60f4e-130">Upload hello schema tooyour storage account, and copy hello URI.</span></span>

    ![Účet úložiště se zvýrazněnou identifikátor URI](media/logic-apps-enterprise-integration-schemas/schema-blob.png)

2. <span data-ttu-id="60f4e-132">V **přidat schéma**, vyberte **velkých souborů**a zadejte hello URI v hello **obsahu URI** textové pole.</span><span class="sxs-lookup"><span data-stu-id="60f4e-132">In **Add Schema**, select **Large file**, and provide hello URI in hello **Content URI** text box.</span></span>

    ![Schémata s tlačítko "Přidat" a "Velkých souborů" zvýrazněná](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

<span data-ttu-id="60f4e-134">Pokud je úroveň přístupu objektu blob hello **žádné anonymní přístup**, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="60f4e-134">If hello blob security access level is **No anonymous access**, follow these steps.</span></span>

![Azure Storage Explorer "Kontejnery objektů Blob", "Zabezpečení" a "Žádná anonymní přístup" zvýrazněná](media/logic-apps-enterprise-integration-schemas/blob-1.png)

1. <span data-ttu-id="60f4e-136">Nahrajte účet úložiště tooyour hello schématu.</span><span class="sxs-lookup"><span data-stu-id="60f4e-136">Upload hello schema tooyour storage account.</span></span>

    ![Účet úložiště](media/logic-apps-enterprise-integration-schemas/blob-3.png)

2. <span data-ttu-id="60f4e-138">Vygenerujte sdílený přístupový podpis pro hello schématu.</span><span class="sxs-lookup"><span data-stu-id="60f4e-138">Generate a shared access signature for hello schema.</span></span>

    ![Účet úložiště se zvýrazněnou kartu podpisy sdíleného přístupu](media/logic-apps-enterprise-integration-schemas/blob-2.png)

3. <span data-ttu-id="60f4e-140">V **přidat schéma**, vyberte **velkých souborů**a zadejte hello sdílený přístupový podpis URI v hello **obsahu URI** textové pole.</span><span class="sxs-lookup"><span data-stu-id="60f4e-140">In **Add Schema**, select **Large file**, and provide hello shared access signature URI in hello **Content URI** text box.</span></span>

    ![Schémata s tlačítko "Přidat" a "Velkých souborů" zvýrazněná](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

4. <span data-ttu-id="60f4e-142">V hello **schémata** okno účtu integrace, by se měla zobrazit vaše nově přidané schéma.</span><span class="sxs-lookup"><span data-stu-id="60f4e-142">In hello **Schemas** blade of your integration account, your newly added schema should appear.</span></span>

    ![Účtu integrace s "Schémat" a zvýrazní nové schéma hello](media/logic-apps-enterprise-integration-schemas/schema-41.png)

## <a name="edit-schemas"></a><span data-ttu-id="60f4e-144">Upravit schémat.</span><span class="sxs-lookup"><span data-stu-id="60f4e-144">Edit schemas</span></span>

1. <span data-ttu-id="60f4e-145">Zvolte hello **schémata** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="60f4e-145">Choose hello **Schemas** tile.</span></span>

2. <span data-ttu-id="60f4e-146">Po hello **schémata** otevře se okno, vyberte hello schéma, které chcete tooedit.</span><span class="sxs-lookup"><span data-stu-id="60f4e-146">After hello **Schemas** blade opens, select hello schema that you want tooedit.</span></span>

3. <span data-ttu-id="60f4e-147">Na hello **schémata** okně zvolte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="60f4e-147">On hello **Schemas** blade, choose **Edit**.</span></span>

    ![Okno schémat.](media/logic-apps-enterprise-integration-schemas/edit-12.png)

4. <span data-ttu-id="60f4e-149">Vyberte hello souboru schématu, které tooedit, a potom vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="60f4e-149">Select hello schema file that you want tooedit, then select **Open**.</span></span>

    ![Tooedit soubor otevřete schématu](media/logic-apps-enterprise-integration-schemas/edit-31.png)

<span data-ttu-id="60f4e-151">Azure ukazuje a zprávy, které hello schéma se úspěšně nahrál.</span><span class="sxs-lookup"><span data-stu-id="60f4e-151">Azure shows a message that hello schema uploaded successfully.</span></span>

## <a name="delete-schemas"></a><span data-ttu-id="60f4e-152">Odstranit schémat.</span><span class="sxs-lookup"><span data-stu-id="60f4e-152">Delete schemas</span></span>

1. <span data-ttu-id="60f4e-153">Zvolte hello **schémata** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="60f4e-153">Choose hello **Schemas** tile.</span></span>

2. <span data-ttu-id="60f4e-154">Po hello **schémata** otevře se okno, vyberte hello schématu chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="60f4e-154">After hello **Schemas** blade opens, select hello schema you want toodelete.</span></span>

3. <span data-ttu-id="60f4e-155">Na hello **schémata** okně zvolte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="60f4e-155">On hello **Schemas** blade, choose **Delete**.</span></span>

    ![Okno schémat.](media/logic-apps-enterprise-integration-schemas/delete-12.png)

4. <span data-ttu-id="60f4e-157">tooconfirm, které chcete toodelete hello vybraná schématu, zvolte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="60f4e-157">tooconfirm that you want toodelete hello selected schema, choose **Yes**.</span></span>

    ![Potvrzovací zpráva "Odstranit schématu"](media/logic-apps-enterprise-integration-schemas/delete-21.png)

    <span data-ttu-id="60f4e-159">V hello **schémata** okně hello schématu seznam aktualizuje a už obsahuje hello schéma, které jste odstranili.</span><span class="sxs-lookup"><span data-stu-id="60f4e-159">In hello **Schemas** blade, hello schema list refreshes  and no longer includes hello schema that you deleted.</span></span>

    ![Svoji integraci účet s "Schémata" zvýrazněná](media/logic-apps-enterprise-integration-schemas/delete-31.png)

## <a name="next-steps"></a><span data-ttu-id="60f4e-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60f4e-161">Next steps</span></span>
* <span data-ttu-id="60f4e-162">[Další informace o hello Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md "Další informace o hello enterprise integračního balíčku").</span><span class="sxs-lookup"><span data-stu-id="60f4e-162">[Learn more about hello Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Learn about hello enterprise integration pack").</span></span>  

