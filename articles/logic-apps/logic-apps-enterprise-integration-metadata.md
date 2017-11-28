---
title: "integrace aaaManage účet artefaktu metadat - Azure Logic Apps | Microsoft Docs"
description: "Přidat nebo načíst metadata artefaktů z účtů integrace pro Azure Logic Apps"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 11/21/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 8de71bffa9f9975d5409716b2208fa6c3a9545d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-artifact-metadata-in-integration-accounts-for-logic-apps"></a><span data-ttu-id="1a51f-103">Spravovat artefaktu metadat v účtech integrace pro logic apps</span><span class="sxs-lookup"><span data-stu-id="1a51f-103">Manage artifact metadata in integration accounts for logic apps</span></span>

<span data-ttu-id="1a51f-104">Můžete definovat vlastní metadata pro artefakty v účty pro integraci a načtení tohoto metadat při běhu pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="1a51f-104">You can define custom metadata for artifacts in integration accounts and retrieve that metadata during runtime for your logic app.</span></span> <span data-ttu-id="1a51f-105">Například můžete zadat metadata pro artefakty jako partnery, smlouvy, schémat a mapy – všechny ukládání metadat pomocí páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="1a51f-105">For example, you can specify metadata for artifacts like partners, agreements, schemas, and maps - all store metadata using key-value pairs.</span></span> <span data-ttu-id="1a51f-106">V současné době artefakty nelze vytvořit metadat prostřednictvím uživatelského rozhraní, ale můžete použít rozhraní REST API toocreate metadat.</span><span class="sxs-lookup"><span data-stu-id="1a51f-106">Currently, artifacts can't create metadata through UI, but you can use REST APIs toocreate metadata.</span></span> <span data-ttu-id="1a51f-107">metadata tooadd při vytváření nebo vyberte partnera, smlouvy nebo schématu v hello portál Azure, zvolte **upravit jako JSON**.</span><span class="sxs-lookup"><span data-stu-id="1a51f-107">tooadd metadata when you create or select a partner, agreement, or schema in hello Azure portal, choose **Edit as JSON**.</span></span> <span data-ttu-id="1a51f-108">tooretrieve artefaktu metadat v aplikace logiky, můžete použít funkce integrace vyhledávání artefaktů účtu hello.</span><span class="sxs-lookup"><span data-stu-id="1a51f-108">tooretrieve artifact metadata in logic apps, you can use hello Integration Account Artifact Lookup feature.</span></span>

## <a name="add-metadata-tooartifacts-in-integration-accounts"></a><span data-ttu-id="1a51f-109">Přidání metadat tooartifacts v účty pro integraci</span><span class="sxs-lookup"><span data-stu-id="1a51f-109">Add metadata tooartifacts in integration accounts</span></span>

1. <span data-ttu-id="1a51f-110">Vytvoření [integrace účet](logic-apps-enterprise-integration-create-integration-account.md).</span><span class="sxs-lookup"><span data-stu-id="1a51f-110">Create an [integration account](logic-apps-enterprise-integration-create-integration-account.md).</span></span>

2. <span data-ttu-id="1a51f-111">Přidat účet integrace tooyour artefaktů, například, [partnera](logic-apps-enterprise-integration-partners.md#how-to-create-a-partner), [smlouvy](logic-apps-enterprise-integration-agreements.md#how-to-create-agreements), nebo [schématu](logic-apps-enterprise-integration-schemas.md).</span><span class="sxs-lookup"><span data-stu-id="1a51f-111">Add an artifact tooyour integration account, for example, a [partner](logic-apps-enterprise-integration-partners.md#how-to-create-a-partner), [agreement](logic-apps-enterprise-integration-agreements.md#how-to-create-agreements), or [schema](logic-apps-enterprise-integration-schemas.md).</span></span>

3.  <span data-ttu-id="1a51f-112">Vyberte hello artefaktů, zvolte **upravit jako JSON**a zadejte podrobnosti o metadata.</span><span class="sxs-lookup"><span data-stu-id="1a51f-112">Select hello artifact, choose **Edit as JSON**, and enter metadata details.</span></span>

    ![Zadejte metadat](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a><span data-ttu-id="1a51f-114">Načtení metadat z artefaktů pro logic apps</span><span class="sxs-lookup"><span data-stu-id="1a51f-114">Retrieve metadata from artifacts for logic apps</span></span>

1. <span data-ttu-id="1a51f-115">Vytvoření [aplikace logiky](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1a51f-115">Create a [logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="1a51f-116">Vytvoření [odkaz z vašeho účtu logiku aplikace tooyour integrace](logic-apps-enterprise-integration-create-integration-account.md#link-an-integration-account-to-a-logic-app).</span><span class="sxs-lookup"><span data-stu-id="1a51f-116">Create a [link from your logic app tooyour integration account](logic-apps-enterprise-integration-create-integration-account.md#link-an-integration-account-to-a-logic-app).</span></span> 

3. <span data-ttu-id="1a51f-117">Návrhář aplikace na základě logiky, přidejte aktivační událost jako *požadavku* nebo *HTTP* tooyour aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="1a51f-117">In Logic App Designer, add a trigger like *Request* or *HTTP* tooyour logic app.</span></span>

4.  <span data-ttu-id="1a51f-118">Zvolte **další krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="1a51f-118">Choose **Next Step** > **Add an action**.</span></span> <span data-ttu-id="1a51f-119">Vyhledejte *integrace* , můžete najít a pak vyberte **integrace účet – integrace vyhledávání artefaktů účtu**.</span><span class="sxs-lookup"><span data-stu-id="1a51f-119">Search for *integration* so you can find and then select **Integration Account - Integration Account Artifact Lookup**.</span></span>

    ![Vyberte integrační účet artefaktů vyhledávání](media/logic-apps-enterprise-integration-metadata/image2.png)

5. <span data-ttu-id="1a51f-121">Vyberte hello **typu artefaktu**a zadejte hello **artefaktů název**.</span><span class="sxs-lookup"><span data-stu-id="1a51f-121">Select hello **Artifact Type**, and provide hello **Artifact Name**.</span></span>

    ![Vyberte typ artefaktů a zadejte název artefaktů](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a><span data-ttu-id="1a51f-123">Příklad: Načtení partnera metadat</span><span class="sxs-lookup"><span data-stu-id="1a51f-123">Example: Retrieve partner metadata</span></span>

<span data-ttu-id="1a51f-124">Metadata partnera má tyto `routingUrl` podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="1a51f-124">Partner metadata has these `routingUrl` details:</span></span>

![Vyhledejte "routingURL" metadata partnera.](media/logic-apps-enterprise-integration-metadata/image6.png)

1. <span data-ttu-id="1a51f-126">V aplikaci logiky přidat aktivační událost, **integrace účet – integrace vyhledávání artefaktů účtu** akce pro váš partner a **HTTP**.</span><span class="sxs-lookup"><span data-stu-id="1a51f-126">In your logic app, add your trigger, an **Integration Account - Integration Account Artifact Lookup** action for your partner, and an **HTTP**.</span></span>

    ![Přidání aktivační událost, artefaktů vyhledávání a aplikace logiky tooyour "HTTP"](media/logic-apps-enterprise-integration-metadata/image4.png)

2. <span data-ttu-id="1a51f-128">tooretrieve hello identifikátor URI, přejděte tooCode zobrazení pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="1a51f-128">tooretrieve hello URI, go tooCode View for your logic app.</span></span> <span data-ttu-id="1a51f-129">Vaše definici aplikace logiky by měl vypadat jako tento ukázkový:</span><span class="sxs-lookup"><span data-stu-id="1a51f-129">Your logic app definition should look like this example:</span></span>

    ![Hledání vyhledávání](media/logic-apps-enterprise-integration-metadata/image5.png)


## <a name="next-steps"></a><span data-ttu-id="1a51f-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a51f-131">Next steps</span></span>
* [<span data-ttu-id="1a51f-132">Další informace o smlouvy</span><span class="sxs-lookup"><span data-stu-id="1a51f-132">Learn more about agreements</span></span>](logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  
