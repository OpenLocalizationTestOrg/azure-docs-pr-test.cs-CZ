---
title: aaaAgreements pro komunikace B2B - Azure Logic Apps | Microsoft Docs
description: "Vytvoření smlouvy, partneři může komunikovat v scénáře B2B Azure Logic Apps a hello Enterprise integračního balíčku"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 447ffb8e-3e91-4403-872b-2f496495899d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2016
ms.author: LADocs
ms.openlocfilehash: 499edcbab1cd67fbc169e393c3cad7b81658a250
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="partner-agreements-for-b2b-communication-with-azure-logic-apps-and-enterprise-integration-pack"></a><span data-ttu-id="eac4f-103">Partner smluv pro B2B komunikace se službou Azure Logic Apps a Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="eac4f-103">Partner agreements for B2B communication with Azure Logic Apps and Enterprise Integration Pack</span></span>

<span data-ttu-id="eac4f-104">Smlouvy umožňují obchodní entity komunikovat bez problémů pomocí standardních protokolů a jsou hello základním kamenem pro komunikaci business-to-business (B2B).</span><span class="sxs-lookup"><span data-stu-id="eac4f-104">Agreements let business entities communicate seamlessly using industry standard protocols and are hello cornerstone for business-to-business (B2B) communication.</span></span> <span data-ttu-id="eac4f-105">Při povolování scénáře B2B pro logic apps s hello Enterprise integračního balíčku, je smlouvu uspořádání komunikace mezi obchodními partnery B2B.</span><span class="sxs-lookup"><span data-stu-id="eac4f-105">When enabling B2B scenarios for logic apps with hello Enterprise Integration Pack, an agreement is a communications arrangement between B2B trading partners.</span></span> <span data-ttu-id="eac4f-106">Tato smlouva je založena na hello komunikace, které mají hello partnery příliš stanovit a protokol nebo přenos specifické.</span><span class="sxs-lookup"><span data-stu-id="eac4f-106">This agreement is based on hello communications that hello partners want too establish and is protocol or transport-specific.</span></span>

<span data-ttu-id="eac4f-107">Integrace Enterprise podporuje těchto standardů protokol nebo přenos:</span><span class="sxs-lookup"><span data-stu-id="eac4f-107">Enterprise integration supports these protocol or transport standards:</span></span>

* [<span data-ttu-id="eac4f-108">AS2</span><span class="sxs-lookup"><span data-stu-id="eac4f-108">AS2</span></span>](logic-apps-enterprise-integration-as2.md)
* [<span data-ttu-id="eac4f-109">X12</span><span class="sxs-lookup"><span data-stu-id="eac4f-109">X12</span></span>](logic-apps-enterprise-integration-x12.md)
* [<span data-ttu-id="eac4f-110">EDIFACT</span><span class="sxs-lookup"><span data-stu-id="eac4f-110">EDIFACT</span></span>](logic-apps-enterprise-integration-edifact.md)

## <a name="why-use-agreements"></a><span data-ttu-id="eac4f-111">Proč používat smlouvy</span><span class="sxs-lookup"><span data-stu-id="eac4f-111">Why use agreements</span></span>

<span data-ttu-id="eac4f-112">Zde jsou některé běžné výhody při použití smlouvy:</span><span class="sxs-lookup"><span data-stu-id="eac4f-112">Here are some common benefits when using agreements:</span></span>

* <span data-ttu-id="eac4f-113">Umožňuje různé organizace a firmy tooexchange informace ve formátu, dobře známé.</span><span class="sxs-lookup"><span data-stu-id="eac4f-113">Enables different organizations and businesses tooexchange information in a well-known format.</span></span>
* <span data-ttu-id="eac4f-114">Zlepšení efektivity při provádění transakcí B2B</span><span class="sxs-lookup"><span data-stu-id="eac4f-114">Improves efficiency when conducting B2B transactions</span></span>
* <span data-ttu-id="eac4f-115">Snadno toocreate, spravovat a použít při vytváření enterprise integrace aplikací</span><span class="sxs-lookup"><span data-stu-id="eac4f-115">Easy toocreate, manage, and use when creating enterprise integration apps</span></span>

## <a name="how-toocreate-agreements"></a><span data-ttu-id="eac4f-116">Jak toocreate smlouvy</span><span class="sxs-lookup"><span data-stu-id="eac4f-116">How toocreate agreements</span></span>

* [<span data-ttu-id="eac4f-117">Vytvoření smlouvy AS2</span><span class="sxs-lookup"><span data-stu-id="eac4f-117">Create an AS2 agreement</span></span>](logic-apps-enterprise-integration-as2.md)
* [<span data-ttu-id="eac4f-118">Vytvoření X12 smlouvy</span><span class="sxs-lookup"><span data-stu-id="eac4f-118">Create an X12 agreement</span></span>](logic-apps-enterprise-integration-x12.md)
* [<span data-ttu-id="eac4f-119">Vytvoření smlouvy EDIFACT</span><span class="sxs-lookup"><span data-stu-id="eac4f-119">Create an EDIFACT agreement</span></span>](logic-apps-enterprise-integration-edifact.md)

## <a name="how-toouse-an-agreement"></a><span data-ttu-id="eac4f-120">Jak toouse smlouvu</span><span class="sxs-lookup"><span data-stu-id="eac4f-120">How toouse an agreement</span></span>

<span data-ttu-id="eac4f-121">Můžete vytvořit [aplikace logiky](logic-apps-what-are-logic-apps.md "Další informace o Logic apps") s možnostmi B2B pomocí smlouvu, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="eac4f-121">You can create [logic apps](logic-apps-what-are-logic-apps.md "Learn about Logic apps") with B2B capabilities by using an agreement that you created.</span></span>

## <a name="how-tooedit-an-agreement"></a><span data-ttu-id="eac4f-122">Jak tooedit smlouvu</span><span class="sxs-lookup"><span data-stu-id="eac4f-122">How tooedit an agreement</span></span>

<span data-ttu-id="eac4f-123">Jakékoli smlouvy, můžete upravit pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="eac4f-123">You can edit any agreement by following these steps:</span></span>

1. <span data-ttu-id="eac4f-124">Vyberte účet hello integrace, který má hello smlouvy, že chcete tooupdate.</span><span class="sxs-lookup"><span data-stu-id="eac4f-124">Select hello integration account that has hello agreement you want tooupdate.</span></span>

2. <span data-ttu-id="eac4f-125">Zvolte hello **smlouvy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="eac4f-125">Choose hello **Agreements** tile.</span></span>

3. <span data-ttu-id="eac4f-126">Na hello **smlouvy** okně, vyberte hello smlouvy.</span><span class="sxs-lookup"><span data-stu-id="eac4f-126">On hello **Agreements** blade, select hello agreement.</span></span>

4. <span data-ttu-id="eac4f-127">Zvolte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="eac4f-127">Choose **Edit**.</span></span> <span data-ttu-id="eac4f-128">Provedené změny.</span><span class="sxs-lookup"><span data-stu-id="eac4f-128">Make your changes.</span></span>

5. <span data-ttu-id="eac4f-129">Zvolte změny, toosave **OK**.</span><span class="sxs-lookup"><span data-stu-id="eac4f-129">toosave your changes, choose **OK**.</span></span>

## <a name="how-toodelete-an-agreement"></a><span data-ttu-id="eac4f-130">Jak toodelete smlouvu</span><span class="sxs-lookup"><span data-stu-id="eac4f-130">How toodelete an agreement</span></span>

<span data-ttu-id="eac4f-131">Jakékoli smlouvy, můžete odstranit pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="eac4f-131">You can delete any agreement by following these steps:</span></span>

1. <span data-ttu-id="eac4f-132">Vyberte účet hello integrace, který má hello smlouvy, že chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="eac4f-132">Select hello integration account that has hello agreement you want toodelete.</span></span>
2. <span data-ttu-id="eac4f-133">Zvolte hello **smlouvy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="eac4f-133">Choose hello **Agreements** tile.</span></span>
3. <span data-ttu-id="eac4f-134">Na hello **smlouvy** okně, vyberte hello smlouvy.</span><span class="sxs-lookup"><span data-stu-id="eac4f-134">On hello **Agreements** blade, select hello agreement.</span></span>
4. <span data-ttu-id="eac4f-135">Zvolte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="eac4f-135">Choose **Delete**.</span></span>
5. <span data-ttu-id="eac4f-136">Potvrďte, že chcete toodelete hello vybrané smlouvy.</span><span class="sxs-lookup"><span data-stu-id="eac4f-136">Confirm that you want toodelete hello selected agreement.</span></span>

    <span data-ttu-id="eac4f-137">Hello smlouvy okno přestane zobrazovat hello odstranit smlouvy.</span><span class="sxs-lookup"><span data-stu-id="eac4f-137">hello Agreements blade no longer shows hello deleted agreement.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eac4f-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eac4f-138">Next steps</span></span>
* [<span data-ttu-id="eac4f-139">Vytvoření smlouvy AS2</span><span class="sxs-lookup"><span data-stu-id="eac4f-139">Create an AS2 agreement</span></span>](logic-apps-enterprise-integration-as2.md)
