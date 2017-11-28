---
title: Smluv pro komunikace B2B - Azure Logic Apps | Microsoft Docs
description: "Vytvoření smlouvy, partneři může komunikovat v scénáře B2B Azure Logic Apps a Enterprise integračního balíčku"
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
ms.openlocfilehash: 7ce0860272901f3b4e4cf3d63f7361d539f64741
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="partner-agreements-for-b2b-communication-with-azure-logic-apps-and-enterprise-integration-pack"></a><span data-ttu-id="85d0c-103">Partner smluv pro B2B komunikace se službou Azure Logic Apps a Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="85d0c-103">Partner agreements for B2B communication with Azure Logic Apps and Enterprise Integration Pack</span></span>

<span data-ttu-id="85d0c-104">Smlouvy umožňují obchodní entity komunikovat bez problémů pomocí standardních protokolů a jsou základním kamenem pro komunikaci business-to-business (B2B).</span><span class="sxs-lookup"><span data-stu-id="85d0c-104">Agreements let business entities communicate seamlessly using industry standard protocols and are the cornerstone for business-to-business (B2B) communication.</span></span> <span data-ttu-id="85d0c-105">Při povolování scénáře B2B pro logic apps s integračního balíčku Enterprise, smlouva je uspořádání komunikace mezi obchodními partnery B2B.</span><span class="sxs-lookup"><span data-stu-id="85d0c-105">When enabling B2B scenarios for logic apps with the Enterprise Integration Pack, an agreement is a communications arrangement between B2B trading partners.</span></span> <span data-ttu-id="85d0c-106">Tato smlouva je podle komunikace, partnery, které chcete vytvořit a je protokol nebo přenos specifické.</span><span class="sxs-lookup"><span data-stu-id="85d0c-106">This agreement is based on the communications that the partners want to establish and is protocol or transport-specific.</span></span>

<span data-ttu-id="85d0c-107">Integrace Enterprise podporuje těchto standardů protokol nebo přenos:</span><span class="sxs-lookup"><span data-stu-id="85d0c-107">Enterprise integration supports these protocol or transport standards:</span></span>

* [<span data-ttu-id="85d0c-108">AS2</span><span class="sxs-lookup"><span data-stu-id="85d0c-108">AS2</span></span>](logic-apps-enterprise-integration-as2.md)
* [<span data-ttu-id="85d0c-109">X12</span><span class="sxs-lookup"><span data-stu-id="85d0c-109">X12</span></span>](logic-apps-enterprise-integration-x12.md)
* [<span data-ttu-id="85d0c-110">EDIFACT</span><span class="sxs-lookup"><span data-stu-id="85d0c-110">EDIFACT</span></span>](logic-apps-enterprise-integration-edifact.md)

## <a name="why-use-agreements"></a><span data-ttu-id="85d0c-111">Proč používat smlouvy</span><span class="sxs-lookup"><span data-stu-id="85d0c-111">Why use agreements</span></span>

<span data-ttu-id="85d0c-112">Zde jsou některé běžné výhody při použití smlouvy:</span><span class="sxs-lookup"><span data-stu-id="85d0c-112">Here are some common benefits when using agreements:</span></span>

* <span data-ttu-id="85d0c-113">Umožňuje různým organizacím a podnikům k výměně informací o ve formátu, dobře známé.</span><span class="sxs-lookup"><span data-stu-id="85d0c-113">Enables different organizations and businesses to exchange information in a well-known format.</span></span>
* <span data-ttu-id="85d0c-114">Zlepšení efektivity při provádění transakcí B2B</span><span class="sxs-lookup"><span data-stu-id="85d0c-114">Improves efficiency when conducting B2B transactions</span></span>
* <span data-ttu-id="85d0c-115">Usnadňují vytváření, správu a použít při vytváření enterprise integrace aplikací</span><span class="sxs-lookup"><span data-stu-id="85d0c-115">Easy to create, manage, and use when creating enterprise integration apps</span></span>

## <a name="how-to-create-agreements"></a><span data-ttu-id="85d0c-116">Postup vytvoření smluv</span><span class="sxs-lookup"><span data-stu-id="85d0c-116">How to create agreements</span></span>

* [<span data-ttu-id="85d0c-117">Vytvoření smlouvy AS2</span><span class="sxs-lookup"><span data-stu-id="85d0c-117">Create an AS2 agreement</span></span>](logic-apps-enterprise-integration-as2.md)
* [<span data-ttu-id="85d0c-118">Vytvoření X12 smlouvy</span><span class="sxs-lookup"><span data-stu-id="85d0c-118">Create an X12 agreement</span></span>](logic-apps-enterprise-integration-x12.md)
* [<span data-ttu-id="85d0c-119">Vytvoření smlouvy EDIFACT</span><span class="sxs-lookup"><span data-stu-id="85d0c-119">Create an EDIFACT agreement</span></span>](logic-apps-enterprise-integration-edifact.md)

## <a name="how-to-use-an-agreement"></a><span data-ttu-id="85d0c-120">Jak používat smlouvu</span><span class="sxs-lookup"><span data-stu-id="85d0c-120">How to use an agreement</span></span>

<span data-ttu-id="85d0c-121">Můžete vytvořit [aplikace logiky](logic-apps-what-are-logic-apps.md "Další informace o Logic apps") s možnostmi B2B pomocí smlouvu, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="85d0c-121">You can create [logic apps](logic-apps-what-are-logic-apps.md "Learn about Logic apps") with B2B capabilities by using an agreement that you created.</span></span>

## <a name="how-to-edit-an-agreement"></a><span data-ttu-id="85d0c-122">Jak upravit smlouvu</span><span class="sxs-lookup"><span data-stu-id="85d0c-122">How to edit an agreement</span></span>

<span data-ttu-id="85d0c-123">Jakékoli smlouvy, můžete upravit pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="85d0c-123">You can edit any agreement by following these steps:</span></span>

1. <span data-ttu-id="85d0c-124">Vyberte integrační účet, který má smlouvy, kterou chcete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="85d0c-124">Select the integration account that has the agreement you want to update.</span></span>

2. <span data-ttu-id="85d0c-125">Vyberte **smlouvy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="85d0c-125">Choose the **Agreements** tile.</span></span>

3. <span data-ttu-id="85d0c-126">Na **smlouvy** okně vyberte smlouvu.</span><span class="sxs-lookup"><span data-stu-id="85d0c-126">On the **Agreements** blade, select the agreement.</span></span>

4. <span data-ttu-id="85d0c-127">Zvolte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="85d0c-127">Choose **Edit**.</span></span> <span data-ttu-id="85d0c-128">Provedené změny.</span><span class="sxs-lookup"><span data-stu-id="85d0c-128">Make your changes.</span></span>

5. <span data-ttu-id="85d0c-129">Pokud chcete uložit provedené změny, zvolte **OK**.</span><span class="sxs-lookup"><span data-stu-id="85d0c-129">To save your changes, choose **OK**.</span></span>

## <a name="how-to-delete-an-agreement"></a><span data-ttu-id="85d0c-130">Postup odstranění smlouvy</span><span class="sxs-lookup"><span data-stu-id="85d0c-130">How to delete an agreement</span></span>

<span data-ttu-id="85d0c-131">Jakékoli smlouvy, můžete odstranit pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="85d0c-131">You can delete any agreement by following these steps:</span></span>

1. <span data-ttu-id="85d0c-132">Vyberte integrační účet, který má smlouvy, kterou chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="85d0c-132">Select the integration account that has the agreement you want to delete.</span></span>
2. <span data-ttu-id="85d0c-133">Vyberte **smlouvy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="85d0c-133">Choose the **Agreements** tile.</span></span>
3. <span data-ttu-id="85d0c-134">Na **smlouvy** okně vyberte smlouvu.</span><span class="sxs-lookup"><span data-stu-id="85d0c-134">On the **Agreements** blade, select the agreement.</span></span>
4. <span data-ttu-id="85d0c-135">Zvolte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="85d0c-135">Choose **Delete**.</span></span>
5. <span data-ttu-id="85d0c-136">Potvrďte, že chcete vybranou smlouvu odstranit.</span><span class="sxs-lookup"><span data-stu-id="85d0c-136">Confirm that you want to delete the selected agreement.</span></span>

    <span data-ttu-id="85d0c-137">V okně smlouvy přestane zobrazovat odstraněné smlouvy.</span><span class="sxs-lookup"><span data-stu-id="85d0c-137">The Agreements blade no longer shows the deleted agreement.</span></span>

## <a name="next-steps"></a><span data-ttu-id="85d0c-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="85d0c-138">Next steps</span></span>
* [<span data-ttu-id="85d0c-139">Vytvoření smlouvy AS2</span><span class="sxs-lookup"><span data-stu-id="85d0c-139">Create an AS2 agreement</span></span>](logic-apps-enterprise-integration-as2.md)
