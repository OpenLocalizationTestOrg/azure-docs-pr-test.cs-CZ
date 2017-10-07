---
title: "aaaAdd hello úložiště objektů blob Azure konektor ve vašich Logic Apps | Microsoft Docs"
description: "Začínáme a nakonfigurovat konektor úložiště objektů blob v Azure hello v aplikaci logiky"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b5dc3f75-6bea-420b-b250-183668d2848d
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/02/2017
ms.author: mandia; ladocs
ms.openlocfilehash: add61287ef1b2228ef9d3f54ce082807bad6858b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-blob-storage-connector-in-a-logic-app"></a>Použití konektoru úložiště objektů blob v Azure hello v aplikaci logiky
Použití hello Azure Blob storage konektor tooupload, aktualizace, získání a odstranění objektů BLOB v účtu úložiště, vše v rámci aplikace logiky.  

S úložištěm Azure blob můžete:

* Vytvořte pracovní postup odesílání nových projektů nebo získávání souborů, které byly nedávno aktualizovány.
* Pomocí akce tooget soubor metadat, odstraňte soubor, kopírovat soubory a další. Například při aktualizaci nástroj v Azure webový server (aktivační událost), aktualizujte soubor v úložišti objektů blob (akce). 

Toto téma ukazuje, jak toouse hello objektu blob úložiště konektor v aplikaci logiky.

toolearn Další informace o Logic Apps, najdete v části [co jsou logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

toolearn Další informace o Logic Apps, najdete v části [co jsou logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-tooazure-blob-storage"></a>Připojit tooAzure úložiště objektů blob
Než se aplikace logiky k jakékoli služby, nejprve vytvořit *připojení* toohello služby. Připojení poskytuje připojení mezi aplikace logiky a jiné služby. Například účet úložiště tooa tooconnect, nejprve vytvoříte úložiště objektů blob *připojení*. toocreate připojení, zadejte přihlašovací údaje hello normálně používat tooaccess hello služby, ke kterému se připojujete. Proto s Azure storage, zadejte přihlašovací údaje hello tooyour úložiště účet toocreate hello připojení. 

#### <a name="create-hello-connection"></a>Vytvoření připojení hello
> [!INCLUDE [Create a connection tooAzure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="use-a-trigger"></a>Použít aktivační událost
Tento konektor nemá žádné aktivační události. Použijte jiné aktivační události toostart hello aplikace logiky, jako jsou aktivační událost opakování, aktivační události Webhooku protokolu HTTP, aktivační události, které jsou k dispozici ostatní konektory a další. [Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) poskytuje příklad.

## <a name="use-an-action"></a>Použít akci.
Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky.

1. Vyberte znaménko plus hello. Zobrazí několik možností: **přidat akci**, **přidat podmínku**, nebo jeden z hello **Další** možnosti.
   
    ![](./media/connectors-create-api-azureblobstorage/add-action.png)
2. Zvolte **přidat akci**.
3. Hello textového pole zadejte "blob" tooget seznam všechny dostupné akce hello.
   
    ![](./media/connectors-create-api-azureblobstorage/actions.png) 
4. V našem příkladu zvolte **AzureBlob - Get metadata souboru pomocí cesty**. Pokud připojení již existuje, pak vyberte hello **...** Tlačítko tooselect (Zobrazit výběr) soubor.
   
    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)
   
    Pokud budete vyzváni k hello informace o připojení, zadejte hello podrobnosti toocreate hello připojení. [Vytvoření připojení hello](connectors-create-api-azureblobstorage.md#create-the-connection) v tomto tématu popisuje tyto vlastnosti. 
   
   > [!NOTE]
   > V tomto příkladu se nám získat hello metadata souboru. toosee hello metadata, přidejte další akci, která vytvoří nový soubor pomocí jiný konektor. Například přidejte OneDrive akci, která vytvoří nový soubor "test" na základě metadat hello. 


5. **Uložit** změny (levém horním rohu hello panelu nástrojů). Aplikace logiky je uloženo a automaticky povolí.

> [!TIP]
> [Storage Explorer](http://storageexplorer.com/) je skvělý nástroj příliš spravovat více účtů úložiště.

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/azureblobconnector/). 

## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md). Prozkoumejte hello dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).

