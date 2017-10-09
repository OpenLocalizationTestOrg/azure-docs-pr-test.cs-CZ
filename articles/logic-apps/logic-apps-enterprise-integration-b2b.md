---
title: "aaaCreate B2B solutions – Azure Logic Apps | Microsoft Docs"
description: "Přijímat data v aplikacích logiky pomocí funkcí hello B2B aplikace hello Enterprise integračního balíčku"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 20fc3722-6f8b-402f-b391-b84e9df6fcff
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 8f01318a0415d81c37b216f9b991c060edec2053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="receive-data-in-logic-apps-with-hello-b2b-features-in-hello-enterprise-integration-pack"></a>Přijímat data v aplikacích logiky s funkcemi B2B hello v hello Enterprise integračního balíčku

Po vytvoření účtu integrace s partnery a smlouvy, jsou připravené toocreate pracovního postupu obchodní toobusiness (B2B) pro svou aplikaci logiky s hello [Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md).

## <a name="prerequisites"></a>Požadavky

toouse hello AS2 a X12 akce, musí mít účet integrace podnikové sítě. Další informace [jak toocreate integrační Enterprise účet](../logic-apps/logic-apps-enterprise-integration-accounts.md).

## <a name="create-a-logic-app-with-b2b-connectors"></a>Vytvoření aplikace logiky s konektory B2B

Postupujte podle těchto kroků toocreate B2B logiku aplikaci, která používá hello AS2 a X12 akce tooreceive data z obchodního partnera:

1. Vytvoření aplikace logiky, pak [propojte si účet tooyour integrace aplikace](../logic-apps/logic-apps-enterprise-integration-accounts.md).

2. Přidat **požadavku - obdrží žádost HTTP při** aplikace logiky tooyour aktivační události.

    ![](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)

3. tooadd hello **dekódovat AS2** akce, vyberte **přidat akci**.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-2.png)

4. toofilter všechny toohello akce, který chcete, zadejte slovo hello **as2** hello vyhledávacího pole.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-5.png)

5. Vyberte hello **AS2 – zpráva dekódovat AS2** akce.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-6.png)

6. Přidat hello **textu** , které chcete toouse jako vstup. V tomto příkladu vyberte hello textu hello požadavku protokolu HTTP, aktivační události hello aplikace logiky. Nebo zadejte výraz, který vstup hello hlavičky v hello **HLAVIČKY** pole:

    @triggerOutputs() [záhlaví]

7. Přidejte požadované hello **hlavičky** pro AS2, které můžete najít v hlavičkách žádosti hello protokolu HTTP. V tomto příkladu vyberte hello hlavičky požadavku HTTP hello aplikaci logiky hello aktivační události.

8. Nyní přidejte hello dekódování X12 zpráva akce. Vyberte **přidat akci**.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-9.png)

9. toofilter všechny toohello akce, který chcete, zadejte slovo hello **x12** hello vyhledávacího pole.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-10.png)

10. Vyberte hello **X12-dekódovat X12 zpráva** akce.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-as2message.png)

11. Nyní je nutné zadat akci vstupní toothis hello. Tento vstup je výstup hello z předchozí akce AS2 hello.

    obsah zprávy skutečné Hello je v objektu JSON a s kódováním base64, musíte zadat výraz jako vstup hello. 
    Zadejte následující výraz v hello hello **X12 s PLOCHOU zpráva souboru tooDECODE** vstupní pole:
    
    @base64ToString(body('Decode_AS2_message')? ['AS2Message']? ['Content'])

    Nyní přidejte kroky toodecode hello X12 data přijata z hello obchodního partnera a výstupní položek v objektu JSON. 
    byla přijata toonotify hello partnera, který hello data, můžete odeslat zpět odpověď obsahující hello AS2 zpráva dispozice oznámení (MDN) v akce odpovědi HTTP.

12. tooadd hello **odpovědi** akce, zvolte **přidat akci**.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-14.png)

13. toofilter všechny toohello akce, který chcete, zadejte slovo hello **odpovědi** hello vyhledávacího pole.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-15.png)

14. Vyberte hello **odpovědi** akce.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-16.png)

15. tooaccess hello MDN z hello výstup hello **zpráva dekódování X12** akce, sada hello odpovědi **textu** pole s tímto výrazem:

    @base64ToString(body('Decode_AS2_message')? ['OutgoingMdn']? ['Content'])

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-17.png)  

16. Uložte práci.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-5.png)  

Nyní dokončení nastavení aplikace logiky B2B. V reálné aplikaci, můžete toostore hello dekódovat X12 dat v úložišti obchodní (LOB) aplikace nebo data. tooconnect vlastní obchodní aplikace a použijte tato rozhraní API v aplikaci logiky, můžete přidat další akce nebo můžete zapsat vlastní rozhraní API.

## <a name="features-and-use-cases"></a>Funkce a případy použití

* Hello AS2 a X12 dekódovat a kódování akce umožňují výměnu dat mezi obchodních partnerů pomocí standardních protokolů v aplikacích logiky.
* tooexchange data s obchodními partnery, můžete použít AS2 a X12 s nebo bez navzájem.
* Hello B2B akce můžete jednoduše vytvořit partnery a smluv ve vašem účtu integrace a využívat v aplikaci logiky.
* Když rozšíříte svou aplikaci logiky s další akce, můžete odesílat a přijímat data mezi jiným aplikacím a službám, jako je SalesForce.

## <a name="learn-more"></a>Další informace
[Další informace o hello Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md)
