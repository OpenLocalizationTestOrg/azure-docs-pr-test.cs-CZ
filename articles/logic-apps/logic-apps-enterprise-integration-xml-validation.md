---
title: aaaValidate XML - Azure Logic Apps | Microsoft Docs
description: "Ověření XML s schémata pro scénáře Azure Logic Apps a B2B pomocí hello Enterprise integračního balíčku"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: d700588f-2d8a-4c92-93eb-e1e6e250e760
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 81f662d0ddf908657b54de8af0a75fff55782ef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="validate-xml-for-enterprise-integration"></a><span data-ttu-id="4faaf-103">Ověření XML pro integraci enterprise</span><span class="sxs-lookup"><span data-stu-id="4faaf-103">Validate XML for enterprise integration</span></span>

<span data-ttu-id="4faaf-104">Často v scénáře B2B hello partnery v smlouvu musí Ujistěte se, že hello zprávy, které si vyměňují jsou platné, než můžete začít zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="4faaf-104">Often in B2B scenarios, hello partners in an agreement must make sure that hello messages they exchange are valid before data processing can start.</span></span> <span data-ttu-id="4faaf-105">Dokumenty pro předdefinované schéma můžete ověřit pomocí hello použití hello ověření XML konektoru v hello Enterprise integračního balíčku.</span><span class="sxs-lookup"><span data-stu-id="4faaf-105">You can validate documents against a predefined schema by using hello use hello XML Validation connector in hello Enterprise Integration Pack.</span></span>

## <a name="validate-a-document-with-hello-xml-validation-connector"></a><span data-ttu-id="4faaf-106">Ověřit dokument s konektorem hello ověření XML</span><span class="sxs-lookup"><span data-stu-id="4faaf-106">Validate a document with hello XML Validation connector</span></span>

1. <span data-ttu-id="4faaf-107">Vytvoření aplikace logiky, a [propojit hello aplikace toohello integrace účet](../logic-apps/logic-apps-enterprise-integration-accounts.md "další toolink aplikace logiky integrace účet tooa") má hello schématu chcete toouse pro ověření dat XML.</span><span class="sxs-lookup"><span data-stu-id="4faaf-107">Create a logic app, and [link hello app toohello integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md "Learn toolink an integration account tooa Logic app") that has hello schema you want toouse for validating XML data.</span></span>

2. <span data-ttu-id="4faaf-108">Přidat **požadavku - obdrží žádost HTTP při** aplikace logiky tooyour aktivační události.</span><span class="sxs-lookup"><span data-stu-id="4faaf-108">Add a **Request - When an HTTP request is received** trigger tooyour logic app.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-1.png)

3. <span data-ttu-id="4faaf-109">tooadd hello **ověření XML** akce, zvolte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="4faaf-109">tooadd hello **XML Validation** action, choose **Add an action**.</span></span>

4. <span data-ttu-id="4faaf-110">Zadejte všechny hello toohello akce, který chcete, toofilter *xml* hello vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="4faaf-110">toofilter all hello actions toohello one that you want, enter *xml* in hello search box.</span></span> <span data-ttu-id="4faaf-111">Zvolte **ověření XML**.</span><span class="sxs-lookup"><span data-stu-id="4faaf-111">Choose **XML Validation**.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-2.png)

5. <span data-ttu-id="4faaf-112">Vyberte toospecify hello obsah XML, které chcete toovalidate, **obsah**.</span><span class="sxs-lookup"><span data-stu-id="4faaf-112">toospecify hello XML content that you want toovalidate, select **CONTENT**.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-1-5.png)

6. <span data-ttu-id="4faaf-113">Vyberte značku textu hello jako obsahu, který má toovalidate hello.</span><span class="sxs-lookup"><span data-stu-id="4faaf-113">Select hello body tag as hello content that you want toovalidate.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-3.png)

7. <span data-ttu-id="4faaf-114">schéma hello toospecify chcete toouse pro ověření hello předchozí *obsah* vstup, zvolte **název schématu**.</span><span class="sxs-lookup"><span data-stu-id="4faaf-114">toospecify hello schema you want toouse for validating hello previous *content* input, choose **SCHEMA NAME**.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-4.png)

8. <span data-ttu-id="4faaf-115">Uložte si práci</span><span class="sxs-lookup"><span data-stu-id="4faaf-115">Save your work</span></span>  

    ![](./media/logic-apps-enterprise-integration-xml/xml-5.png)

<span data-ttu-id="4faaf-116">Nyní jste hotovi s nastavením vašeho konektoru ověření.</span><span class="sxs-lookup"><span data-stu-id="4faaf-116">You are now done with setting up your validation connector.</span></span> <span data-ttu-id="4faaf-117">V reálné aplikaci můžete chtít toostore hello ověřit data v aplikaci obchodní (LOB) jako SalesForce.</span><span class="sxs-lookup"><span data-stu-id="4faaf-117">In a real world application, you might want toostore hello validated data in a line-of-business (LOB) app like SalesForce.</span></span> <span data-ttu-id="4faaf-118">toosend hello tooSalesforce ověřené výstup, přidat akci.</span><span class="sxs-lookup"><span data-stu-id="4faaf-118">toosend hello validated output tooSalesforce, add an action.</span></span>

<span data-ttu-id="4faaf-119">tootest ověření akci, ujistěte se, koncový bod toohello HTTP žádosti.</span><span class="sxs-lookup"><span data-stu-id="4faaf-119">tootest your validation action, make a request toohello HTTP endpoint.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4faaf-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4faaf-120">Next steps</span></span>
[<span data-ttu-id="4faaf-121">Další informace o hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="4faaf-121">Learn more about hello Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")   

