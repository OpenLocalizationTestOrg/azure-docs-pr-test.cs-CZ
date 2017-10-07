---
title: "zprávy aaaEncode AS2 – Azure Logic Apps | Microsoft Docs"
description: "Jak toouse hello AS2 kodér v hello Enterprise integrační balíček pro Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 332fb9e3-576c-4683-bd10-d177a0ebe9a3
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 2b372c416512ffa9ea5dc50ce0f767bfd8aefbc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encode-as2-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>Kódování zprávy AS2 pro Azure Logic Apps s hello Enterprise integračního balíčku

tooestablish zabezpečení a spolehlivost při přenášení zpráv, pomocí konektoru serveru hello AS2 kódování zprávy. Tento konektor poskytuje digitální podpis, šifrování a potvrzení prostřednictvím zpráv dispozice oznámení (MDN), což také vede toosupport pro Non-Odvolatelnost.

## <a name="before-you-start"></a>Než začnete

Tady je hello položky, které budete potřebovat:

* Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
* [Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure. Musíte mít konektoru integrace účet toouse hello AS2 kódování zprávy.
* Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace
* [Smlouvy AS2](logic-apps-enterprise-integration-as2.md) , již je definována v účtu integrace

## <a name="encode-as2-messages"></a>Kódování zprávy AS2

1. [Vytvoření aplikace logiky](logic-apps-create-a-logic-app.md).

2. Hello AS2 kódování zpráv konektor nemá, aktivační události, je nutné přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku. V hello návrhář aplikace logiky přidejte aktivační události a poté přidejte logiku aplikace tooyour akce.

3.  Hello vyhledávacího pole zadejte "AS2" filtru. Vyberte **AS2 - AS2 kódování zpráv**.
   
    ![Vyhledejte "AS2"](./media/logic-apps-enterprise-integration-as2-encode/as2decodeimage1.png)

4. Pokud jste nevytvořili dříve všechna připojení tooyour integrace účet, se zobrazí výzva toocreate nyní toto připojení. Název připojení a vyberte účet hello integrace, které chcete tooconnect. 
   
    ![Vytvořte účet toointegration připojení](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage1.png)  

    Vyžadují se vlastnosti s hvězdičkou.

    | Vlastnost | Podrobnosti |
    | --- | --- |
    | Název připojení * |Zadejte libovolný název pro připojení. |
    | Integrace účet * |Zadejte název pro váš účet integrace. Ujistěte se, že integrace účet a logiku aplikace jsou v hello stejného umístění Azure. |

5.  Když jste hotovi, podrobné informace o připojení by měla vypadat podobně jako příklad toothis. toofinish vytváření připojení, zvolte **vytvořit**.
   
    ![Podrobné informace o integraci připojení](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage2.png)

6. Po vytvoření připojení, jak je znázorněno v tomto příkladu, zadejte podrobnosti pro **AS2-z**, **AS2 tooidentifiers** podle konfigurace v smlouvě, a **textu**, což je Hello datovou část zprávy.
   
    ![Zadejte povinné pole](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage3.png)

## <a name="as2-encoder-details"></a>Podrobnosti kodér AS2

konektor kódování AS2 Hello provádí tyto úlohy: 

* Platí hlavičky AS2/HTTP
* Přihlásí odchozí zprávy (Pokud je nakonfigurováno)
* Šifruje odchozích zpráv (Pokud je nakonfigurováno)
* Komprimuje uvítací zprávu (Pokud je nakonfigurováno)

## <a name="try-this-sample"></a>Zkuste tuto ukázku

tootry nasazení plně funkční logiku aplikace a ukázkové AS2 scénáři, najdete v části hello [AS2 šablona aplikace logiky a scénář](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="view-hello-swagger"></a>Zobrazení hello swagger
V tématu hello [swagger podrobnosti](/connectors/as2/). 

## <a name="next-steps"></a>Další kroky
[Další informace o hello Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku") 

