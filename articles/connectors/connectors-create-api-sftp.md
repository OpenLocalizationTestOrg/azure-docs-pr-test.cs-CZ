---
title: "aaaLearn jak toouse hello SFTP konektor ve vašich logic apps | Microsoft Docs"
description: "Vytvoření aplikace logiky službou Azure App service. Připojte toosend tooSFTP rozhraní API a přijímat soubory. Můžete provádět různé operace, jako je vytváření, aktualizace, získání nebo odstraňte některé soubory."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 697eb8b0-4a66-40c7-be7b-6aa6b131c7ad
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/20/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 3f50570191c9b9339fe6584b9056b2549512b789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-sftp-connector"></a>Začínáme s konektorem SFTP hello
Použijte hello SFTP konektor tooaccess toosend účet protokolu SFTP a přijímat soubory. Můžete provádět různé operace, jako je vytváření, aktualizace, získání nebo odstraňte některé soubory.  

toouse [všechny konektory](apis-list.md), musíte nejprve toocreate aplikace logiky. Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-toosftp"></a>Připojit tooSFTP
Než se aplikace logiky k jakékoli služby, musíte nejprve toocreate *připojení* toohello služby. A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.  

### <a name="create-a-connection-toosftp"></a>Vytvoření připojení tooSFTP
> [!INCLUDE [Steps toocreate a connection tooSFTP](../../includes/connectors-create-api-sftp.md)]
> 
> 

## <a name="use-an-sftp-trigger"></a>Použít aktivační procedury protokolu SFTP
Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky. [Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

V tomto příkladu hello **SFTP – Pokud je soubor přidat ani upravit** aktivační událost není použité tooinitiate pracovní postup aplikace logiky při je přidat do souboru, nebo úpravě na serveru pomocí protokolu SFTP. Můžete také přidat podmínku, která kontroluje hello obsah hello nové nebo upravené souboru a vytvoří soubor rozhodnutí tooextract hello, pokud jeho obsah označuje, že by měla být rozbalena před použitím hello obsah. Nakonec přidejte akci tooextract hello obsah souboru a umístit do složky na serveru pomocí protokolu SFTP hello hello extrahovat obsah. 

V příklad enterprise můžete použít tuto aktivační událost toomonitor na složku SFTP pro nové soubory, které představují objednávek zákazníků.  Poté můžete použít konektor akce SFTP, jako například **získat obsah souboru**, tooget hello obsah hello pořadí pro další zpracování a úložiště v databázi objednávky.

> [!INCLUDE [Steps toocreate an SFTP trigger](../../includes/connectors-create-api-sftp-trigger.md)]
> 
> 

## <a name="add-a-condition"></a>Přidat podmínku
> [!INCLUDE [Steps tooadd a condition](../../includes/connectors-create-api-sftp-condition.md)]
> 
> 

## <a name="use-an-sftp-action"></a>Použít akci protokolu SFTP
Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky. [Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!INCLUDE [Steps toocreate an SFTP action](../../includes/connectors-create-api-sftp-action.md)]
> 
> 

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/sftpconnector/).

## <a name="more-connectors"></a>Více konektorů
Přejděte zpět toohello [rozhraní API seznamu](apis-list.md).
