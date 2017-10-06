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
# <a name="validate-xml-for-enterprise-integration"></a>Ověření XML pro integraci enterprise

Často v scénáře B2B hello partnery v smlouvu musí Ujistěte se, že hello zprávy, které si vyměňují jsou platné, než můžete začít zpracování dat. Dokumenty pro předdefinované schéma můžete ověřit pomocí hello použití hello ověření XML konektoru v hello Enterprise integračního balíčku.

## <a name="validate-a-document-with-hello-xml-validation-connector"></a>Ověřit dokument s konektorem hello ověření XML

1. Vytvoření aplikace logiky, a [propojit hello aplikace toohello integrace účet](../logic-apps/logic-apps-enterprise-integration-accounts.md "další toolink aplikace logiky integrace účet tooa") má hello schématu chcete toouse pro ověření dat XML.

2. Přidat **požadavku - obdrží žádost HTTP při** aplikace logiky tooyour aktivační události.

    ![](./media/logic-apps-enterprise-integration-xml/xml-1.png)

3. tooadd hello **ověření XML** akce, zvolte **přidat akci**.

4. Zadejte všechny hello toohello akce, který chcete, toofilter *xml* hello vyhledávacího pole. Zvolte **ověření XML**.

    ![](./media/logic-apps-enterprise-integration-xml/xml-2.png)

5. Vyberte toospecify hello obsah XML, které chcete toovalidate, **obsah**.

    ![](./media/logic-apps-enterprise-integration-xml/xml-1-5.png)

6. Vyberte značku textu hello jako obsahu, který má toovalidate hello.

    ![](./media/logic-apps-enterprise-integration-xml/xml-3.png)

7. schéma hello toospecify chcete toouse pro ověření hello předchozí *obsah* vstup, zvolte **název schématu**.

    ![](./media/logic-apps-enterprise-integration-xml/xml-4.png)

8. Uložte si práci  

    ![](./media/logic-apps-enterprise-integration-xml/xml-5.png)

Nyní jste hotovi s nastavením vašeho konektoru ověření. V reálné aplikaci můžete chtít toostore hello ověřit data v aplikaci obchodní (LOB) jako SalesForce. toosend hello tooSalesforce ověřené výstup, přidat akci.

tootest ověření akci, ujistěte se, koncový bod toohello HTTP žádosti.

## <a name="next-steps"></a>Další kroky
[Další informace o hello Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")   

