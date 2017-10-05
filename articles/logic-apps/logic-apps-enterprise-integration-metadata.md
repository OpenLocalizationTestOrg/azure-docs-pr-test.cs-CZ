---
title: "Správa integrace účet artefaktu metadat - Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 28bb8296ddd820ec5aa9793dc0928b4b1e67bf6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-artifact-metadata-in-integration-accounts-for-logic-apps"></a><span data-ttu-id="3d790-103">Spravovat artefaktu metadat v účtech integrace pro logic apps</span><span class="sxs-lookup"><span data-stu-id="3d790-103">Manage artifact metadata in integration accounts for logic apps</span></span>

<span data-ttu-id="3d790-104">Můžete definovat vlastní metadata pro artefakty v účty pro integraci a načtení tohoto metadat při běhu pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="3d790-104">You can define custom metadata for artifacts in integration accounts and retrieve that metadata during runtime for your logic app.</span></span> <span data-ttu-id="3d790-105">Například můžete zadat metadata pro artefakty jako partnery, smlouvy, schémat a mapy – všechny ukládání metadat pomocí páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="3d790-105">For example, you can specify metadata for artifacts like partners, agreements, schemas, and maps - all store metadata using key-value pairs.</span></span> <span data-ttu-id="3d790-106">V současné době artefakty nelze vytvořit metadat prostřednictvím uživatelského rozhraní, ale rozhraní REST API můžete použít k vytvoření metadat.</span><span class="sxs-lookup"><span data-stu-id="3d790-106">Currently, artifacts can't create metadata through UI, but you can use REST APIs to create metadata.</span></span> <span data-ttu-id="3d790-107">Chcete-li přidat metadata při vytváření nebo vyberte partnera, smlouvy nebo schématu na portálu Azure, zvolte **upravit jako JSON**.</span><span class="sxs-lookup"><span data-stu-id="3d790-107">To add metadata when you create or select a partner, agreement, or schema in the Azure portal, choose **Edit as JSON**.</span></span> <span data-ttu-id="3d790-108">K načtení metadat artefaktů v aplikace logiky, můžete použít funkci integrace vyhledávání artefaktů účtu.</span><span class="sxs-lookup"><span data-stu-id="3d790-108">To retrieve artifact metadata in logic apps, you can use the Integration Account Artifact Lookup feature.</span></span>

## <a name="add-metadata-to-artifacts-in-integration-accounts"></a><span data-ttu-id="3d790-109">Přidat metadata do artefakty na účty pro integraci</span><span class="sxs-lookup"><span data-stu-id="3d790-109">Add metadata to artifacts in integration accounts</span></span>

1. <span data-ttu-id="3d790-110">Vytvoření [integrace účet](logic-apps-enterprise-integration-create-integration-account.md).</span><span class="sxs-lookup"><span data-stu-id="3d790-110">Create an [integration account](logic-apps-enterprise-integration-create-integration-account.md).</span></span>

2. <span data-ttu-id="3d790-111">Například přidejte artefakt ke svému účtu integrace, [partnera](logic-apps-enterprise-integration-partners.md#how-to-create-a-partner), [smlouvy](logic-apps-enterprise-integration-agreements.md#how-to-create-agreements), nebo [schématu](logic-apps-enterprise-integration-schemas.md).</span><span class="sxs-lookup"><span data-stu-id="3d790-111">Add an artifact to your integration account, for example, a [partner](logic-apps-enterprise-integration-partners.md#how-to-create-a-partner), [agreement](logic-apps-enterprise-integration-agreements.md#how-to-create-agreements), or [schema](logic-apps-enterprise-integration-schemas.md).</span></span>

3.  <span data-ttu-id="3d790-112">Vyberte artefaktu, zvolte **upravit jako JSON**a zadejte podrobnosti o metadata.</span><span class="sxs-lookup"><span data-stu-id="3d790-112">Select the artifact, choose **Edit as JSON**, and enter metadata details.</span></span>

    ![Zadejte metadat](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a><span data-ttu-id="3d790-114">Načtení metadat z artefaktů pro logic apps</span><span class="sxs-lookup"><span data-stu-id="3d790-114">Retrieve metadata from artifacts for logic apps</span></span>

1. <span data-ttu-id="3d790-115">Vytvoření [aplikace logiky](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="3d790-115">Create a [logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="3d790-116">Vytvoření [odkaz z aplikace logiky k vašemu účtu integrace](logic-apps-enterprise-integration-create-integration-account.md#link-an-integration-account-to-a-logic-app).</span><span class="sxs-lookup"><span data-stu-id="3d790-116">Create a [link from your logic app to your integration account](logic-apps-enterprise-integration-create-integration-account.md#link-an-integration-account-to-a-logic-app).</span></span> 

3. <span data-ttu-id="3d790-117">Návrhář aplikace na základě logiky, přidejte aktivační událost jako *požadavku* nebo *HTTP* do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="3d790-117">In Logic App Designer, add a trigger like *Request* or *HTTP* to your logic app.</span></span>

4.  <span data-ttu-id="3d790-118">Zvolte **další krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="3d790-118">Choose **Next Step** > **Add an action**.</span></span> <span data-ttu-id="3d790-119">Vyhledejte *integrace* , můžete najít a pak vyberte **integrace účet – integrace vyhledávání artefaktů účtu**.</span><span class="sxs-lookup"><span data-stu-id="3d790-119">Search for *integration* so you can find and then select **Integration Account - Integration Account Artifact Lookup**.</span></span>

    ![Vyberte integrační účet artefaktů vyhledávání](media/logic-apps-enterprise-integration-metadata/image2.png)

5. <span data-ttu-id="3d790-121">Vyberte **typu artefaktu**a zadejte **artefaktů název**.</span><span class="sxs-lookup"><span data-stu-id="3d790-121">Select the **Artifact Type**, and provide the **Artifact Name**.</span></span>

    ![Vyberte typ artefaktů a zadejte název artefaktů](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a><span data-ttu-id="3d790-123">Příklad: Načtení partnera metadat</span><span class="sxs-lookup"><span data-stu-id="3d790-123">Example: Retrieve partner metadata</span></span>

<span data-ttu-id="3d790-124">Metadata partnera má tyto `routingUrl` podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="3d790-124">Partner metadata has these `routingUrl` details:</span></span>

![Vyhledejte "routingURL" metadata partnera.](media/logic-apps-enterprise-integration-metadata/image6.png)

1. <span data-ttu-id="3d790-126">V aplikaci logiky přidat aktivační událost, **integrace účet – integrace vyhledávání artefaktů účtu** akce pro váš partner a **HTTP**.</span><span class="sxs-lookup"><span data-stu-id="3d790-126">In your logic app, add your trigger, an **Integration Account - Integration Account Artifact Lookup** action for your partner, and an **HTTP**.</span></span>

    ![Přidání do aplikace logiky aktivační událost, artefaktů vyhledávání a "HTTP"](media/logic-apps-enterprise-integration-metadata/image4.png)

2. <span data-ttu-id="3d790-128">Chcete-li získat identifikátor URI, přejděte do zobrazení kódu pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="3d790-128">To retrieve the URI, go to Code View for your logic app.</span></span> <span data-ttu-id="3d790-129">Vaše definici aplikace logiky by měl vypadat jako tento ukázkový:</span><span class="sxs-lookup"><span data-stu-id="3d790-129">Your logic app definition should look like this example:</span></span>

    ![Hledání vyhledávání](media/logic-apps-enterprise-integration-metadata/image5.png)


## <a name="next-steps"></a><span data-ttu-id="3d790-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3d790-131">Next steps</span></span>
* [<span data-ttu-id="3d790-132">Další informace o smlouvy</span><span class="sxs-lookup"><span data-stu-id="3d790-132">Learn more about agreements</span></span>](logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  
