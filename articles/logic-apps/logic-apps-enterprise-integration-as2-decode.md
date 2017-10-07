---
title: "zprávy aaaDecode AS2 – Azure Logic Apps | Microsoft Docs"
description: "Jak toouse hello AS2 decoder v hello Enterprise integrační balíček pro Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 2406e5ec68e0906700fad97d60cb83ef0d106cd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="decode-as2-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>Dekódovat AS2 zprávy pro Azure Logic Apps s hello Enterprise integračního balíčku 

tooestablish zabezpečení a spolehlivost při přenášení zpráv pomocí konektoru zpráva dekódovat AS2 hello. Tento konektor poskytuje digitální podpis, dešifrování a potvrzení prostřednictvím zpráv dispozice oznámení (MDN).

## <a name="before-you-start"></a>Než začnete

Tady je hello položky, které budete potřebovat:

* Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
* [Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure. Musíte mít konektoru integrace účet toouse hello dekódovat AS2 zprávy.
* Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace
* [Smlouvy AS2](logic-apps-enterprise-integration-as2.md) , již je definována v účtu integrace

## <a name="decode-as2-messages"></a>Dekódovat AS2 zprávy

1. [Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

2. Hello dekódovat AS2 zpráva konektor nemá aktivačních událostí, je nutné přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku. V hello návrhář aplikace logiky přidejte aktivační události a poté přidejte logiku aplikace tooyour akce.

3.  Hello vyhledávacího pole zadejte "AS2" filtru. Vyberte **AS2 - dekódovat AS2 zpráva**.
   
    ![Vyhledejte "AS2"](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage1.png)

4. Pokud jste nevytvořili dříve všechna připojení tooyour integrace účet, se zobrazí výzva toocreate nyní toto připojení. Název připojení a vyberte účet hello integrace, které chcete tooconnect.
   
    ![Vytvoření připojení integrace](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage2.png)

    Vyžadují se vlastnosti s hvězdičkou.

    | Vlastnost | Podrobnosti |
    | --- | --- |
    | Název připojení * |Zadejte libovolný název pro připojení. |
    | Integrace účet * |Zadejte název pro váš účet integrace. Ujistěte se, že integrace účet a logiku aplikace jsou v hello stejného umístění Azure. |

5.  Když jste hotovi, podrobné informace o připojení by měla vypadat podobně jako příklad toothis. toofinish vytváření připojení, zvolte **vytvořit**.

    ![Podrobné informace o integraci připojení](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage3.png)

6. Po vytvoření připojení, jak je znázorněno v tomto příkladu, vyberte **textu** a **hlavičky** z hello požadavku výstupy.
   
    ![Vytvoření připojení integrace](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage4.png) 

    Například:

    ![Vyberte textu a hlaviček ze žádosti výstupy](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage5.png) 

## <a name="as2-decoder-details"></a>Podrobnosti decoder AS2

konektor dekódovat AS2 Hello provádí tyto úlohy: 

* Zpracuje hlavičky AS2/HTTP
* Ověří podpis hello (Pokud je nakonfigurováno)
* Dešifruje hello zprávy (Pokud je nakonfigurováno)
* Dekomprimuje uvítací zprávu (Pokud je nakonfigurováno)
* Sloučí přijaté MDN s původní odchozí zprávy hello
* Aktualizace a korelaci záznamy v databázi nepopiratelnosti hello
* Zapíše záznamy pro generování sestav o stavu AS2
* Hello výstupní datovou část obsahu jsou kódováním base64
* Určuje, jestli MDN je nutné a jestli hello MDN by měla být synchronní nebo asynchronní na základě konfigurace smlouvy AS2
* Generuje synchronní nebo asynchronní MDN (podle konfigurace smlouvy)
* Nastaví na hello MDN hello korelace tokeny a vlastnosti

## <a name="try-this-sample"></a>Zkuste tuto ukázku

tootry nasazení plně funkční logiku aplikace a ukázkové AS2 scénáři, najdete v části hello [AS2 šablona aplikace logiky a scénář](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="view-hello-swagger"></a>Zobrazení hello swagger
V tématu hello [swagger podrobnosti](/connectors/as2/). 

## <a name="next-steps"></a>Další kroky
[Další informace o hello Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md) 

