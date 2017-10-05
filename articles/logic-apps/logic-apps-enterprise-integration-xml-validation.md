---
title: "Ověření XML - Azure Logic Apps | Microsoft Docs"
description: "Ověření XML s schémata pro scénáře Azure Logic Apps a B2B pomocí Enterprise integračního balíčku"
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
ms.openlocfilehash: 8558efffa354cc4bb93820c837077ee997924c95
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="validate-xml-for-enterprise-integration"></a><span data-ttu-id="f13b9-103">Ověření XML pro integraci enterprise</span><span class="sxs-lookup"><span data-stu-id="f13b9-103">Validate XML for enterprise integration</span></span>

<span data-ttu-id="f13b9-104">Často v scénáře B2B partnery v smlouvu musí Ujistěte se, že zprávy, které si vyměňují jsou platné, než můžete začít zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="f13b9-104">Often in B2B scenarios, the partners in an agreement must make sure that the messages they exchange are valid before data processing can start.</span></span> <span data-ttu-id="f13b9-105">Dokumenty pro předdefinované schéma můžete ověřit pomocí použití konektoru ověření XML v podniku integračního balíčku.</span><span class="sxs-lookup"><span data-stu-id="f13b9-105">You can validate documents against a predefined schema by using the use the XML Validation connector in the Enterprise Integration Pack.</span></span>

## <a name="validate-a-document-with-the-xml-validation-connector"></a><span data-ttu-id="f13b9-106">Ověřit dokument s konektorem ověření XML</span><span class="sxs-lookup"><span data-stu-id="f13b9-106">Validate a document with the XML Validation connector</span></span>

1. <span data-ttu-id="f13b9-107">Vytvoření aplikace logiky, a [propojit aplikaci se účtem integrace](../logic-apps/logic-apps-enterprise-integration-accounts.md "zjistěte, jak lze propojit účet integrace aplikace logiky") má schéma, kterou chcete použít pro ověření dat XML.</span><span class="sxs-lookup"><span data-stu-id="f13b9-107">Create a logic app, and [link the app to the integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md "Learn to link an integration account to a Logic app") that has the schema you want to use for validating XML data.</span></span>

2. <span data-ttu-id="f13b9-108">Přidat **požadavku - obdrží žádost HTTP při** aktivační svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="f13b9-108">Add a **Request - When an HTTP request is received** trigger to your logic app.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-1.png)

3. <span data-ttu-id="f13b9-109">Chcete-li přidat **ověření XML** akce, zvolte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="f13b9-109">To add the **XML Validation** action, choose **Add an action**.</span></span>

4. <span data-ttu-id="f13b9-110">Chcete-li všechny akce, které ten, který chcete filtrovat, zadejte *xml* do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="f13b9-110">To filter all the actions to the one that you want, enter *xml* in the search box.</span></span> <span data-ttu-id="f13b9-111">Zvolte **ověření XML**.</span><span class="sxs-lookup"><span data-stu-id="f13b9-111">Choose **XML Validation**.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-2.png)

5. <span data-ttu-id="f13b9-112">Chcete-li určit obsah XML, který chcete ověřit, vyberte **obsah**.</span><span class="sxs-lookup"><span data-stu-id="f13b9-112">To specify the XML content that you want to validate, select **CONTENT**.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-1-5.png)

6. <span data-ttu-id="f13b9-113">Vyberte značky body jako obsah, který chcete ověřit.</span><span class="sxs-lookup"><span data-stu-id="f13b9-113">Select the body tag as the content that you want to validate.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-3.png)

7. <span data-ttu-id="f13b9-114">K určení schématu, kterou chcete použít pro ověření předchozí *obsah* vstup, zvolte **název schématu**.</span><span class="sxs-lookup"><span data-stu-id="f13b9-114">To specify the schema you want to use for validating the previous *content* input, choose **SCHEMA NAME**.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-4.png)

8. <span data-ttu-id="f13b9-115">Uložte si práci</span><span class="sxs-lookup"><span data-stu-id="f13b9-115">Save your work</span></span>  

    ![](./media/logic-apps-enterprise-integration-xml/xml-5.png)

<span data-ttu-id="f13b9-116">Nyní jste hotovi s nastavením vašeho konektoru ověření.</span><span class="sxs-lookup"><span data-stu-id="f13b9-116">You are now done with setting up your validation connector.</span></span> <span data-ttu-id="f13b9-117">V reálné aplikaci můžete chtít uložit ověřená data v aplikaci obchodní (LOB) jako SalesForce.</span><span class="sxs-lookup"><span data-stu-id="f13b9-117">In a real world application, you might want to store the validated data in a line-of-business (LOB) app like SalesForce.</span></span> <span data-ttu-id="f13b9-118">Chcete-li odeslat ověřené výstup do služby Salesforce, přidáte akci.</span><span class="sxs-lookup"><span data-stu-id="f13b9-118">To send the validated output to Salesforce, add an action.</span></span>

<span data-ttu-id="f13b9-119">Pokud chcete otestovat ověření akci, vytvořte žádost na koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="f13b9-119">To test your validation action, make a request to the HTTP endpoint.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f13b9-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f13b9-120">Next steps</span></span>
[<span data-ttu-id="f13b9-121">Další informace o integračního balíčku Enterprise</span><span class="sxs-lookup"><span data-stu-id="f13b9-121">Learn more about the Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")   

