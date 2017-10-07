---
title: aaaLearn jak toouse hello konektor FTP v aplikace logiky | Microsoft Docs
description: "Vytvoření aplikace logiky službou Azure App service. Připojte tooFTP server toomanage vaše soubory. Můžete provádět různé akce, jako je například nahrávání, aktualizovat, získání a odstranit soubory v serveru FTP."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: d83c55fe-eb59-4b7b-a5ec-afac5c772616
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/22/2016
ms.author: mandia; ladocs
ms.openlocfilehash: a7020df2005ebb34fc569627ae0516b8528cc7a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-ftp-connector"></a>Začínáme s konektor FTP hello
Pomocí konektoru toomonitor hello FTP, spravovat a vytvoření souborů na serveru FTP. 

toouse [všechny konektory](apis-list.md), musíte nejprve toocreate aplikace logiky. Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-tooftp"></a>Připojit tooFTP
Než se aplikace logiky k jakékoli služby, musíte nejprve toocreate *připojení* toohello služby. A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.  

### <a name="create-a-connection-tooftp"></a>Vytvoření připojení tooFTP
> [!INCLUDE [Steps toocreate a connection tooFTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a>Použít aktivační událost FTP
Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky. [Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!IMPORTANT]
> konektor FTP Hello vyžaduje serveru FTP, který je dostupný z Internetu hello a je nakonfigurované toooperate pasivní režim. Je taky hello FTP konektor **není kompatibilní s implicitní FTPS (FTP přes protokol SSL)**. konektor FTP Hello podporuje pouze explicitní FTPS (FTP přes protokol SSL).  
> 
> 

V tomto příkladu I vám ukáže, jak toouse hello **FTP – Pokud je soubor přidat ani upravit** aktivují tooinitiate pracovní postup aplikace logiky, když je přidán do, nebo změny souboru na serveru FTP. V příklad enterprise můžete použít tento aktivační událost toomonitor složky FTP pro nové soubory, které představují objednávek zákazníků.  Poté můžete použít akci konektor FTP, jako **získat obsah souboru** tooget hello obsah hello pořadí pro další zpracování a úložiště v databázi objednávky.

1. Zadejte *ftp* hello vyhledávacího pole v designeru aplikace logiky hello zvolte hello **FTP – Pokud je soubor přidat ani upravit** aktivační události   
   ![Obrázek aktivační událost FTP 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
   Hello **je přidána nebo upravena souboru** otevře se ovládací prvek  
   ![Obrázek aktivační událost FTP 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
2. Vyberte hello **...**  nachází na pravé straně hello hello řízení. Otevře se ovládací prvek pro výběr složky hello  
   ![Obrázek aktivační událost FTP 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
3. Vyberte hello  **>**  (šipka doprava) a vyhledejte složku hello toofind má toomonitor pro nové nebo upravení soubory. Vyberte složku hello a Všimněte si hello složky se nyní zobrazí v hello **složky** ovládacího prvku.  
   ![FTP aktivační událost obrázek 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)   

Aplikace logiky v tomto okamžiku je nakonfigurovaná s aktivační událost, která bude zahájena spuštění hello jiných triggery a akce v pracovním postupu hello po upravit nebo vytvořit v konkrétní složce FTP hello souboru. 

> [!NOTE]
> Pro logiku aplikace toobe funkční musí obsahovat nejméně jedna aktivační událost a jedna akce. Postupujte podle kroků hello v další části tooadd hello akce.  
> 
> 

## <a name="use-a-ftp-action"></a>Pomocí akce FTP
Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky. [Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

Teď, když jste přidali aktivační událost, postupujte podle těchto kroků tooadd akci, která bude načíst obsah hello hello nové nebo upravené souboru nalezen hello aktivační procedura.    

1. Vyberte **+ nový krok** tooadd hello hello akce tooget hello obsah souboru hello na serveru hello FTP  
2. Vyberte hello **přidat akci** odkaz.  
   ![Obrázek akce FTP 1](./media/connectors-create-api-ftp/ftp-action-1.png)  
3. Zadejte *FTP* toosearch pro všechny akce související s tooFTP.
4. Vyberte **FTP – získat obsah souboru** jako hello tootake akce při nové nebo upravené soubor naleznete ve složce hello FTP.      
   ![Obrázek akce FTP 2](./media/connectors-create-api-ftp/ftp-action-2.png)  
   Hello **získat obsah souboru** řízení otevře. **Poznámka:**: vám bude výzvami tooauthorize vaše tooaccess aplikace logiky váš server FTP účtu, pokud jste tak dosud neučinili dříve.  
   ![Obrázek akce FTP 3](./media/connectors-create-api-ftp/ftp-action-3.png)   
5. Vyberte hello **soubor** ovládací prvek (hello mezer nacházel pod **souboru***). Tady můžete použít všechny hello různé vlastnosti z hello nový nebo změněný soubor nalezen na serveru FTP hello.  
6. Vyberte hello **souboru obsahu** možnost.  
   ![Obrázek akce FTP 4](./media/connectors-create-api-ftp/ftp-action-4.png)   
7. ovládací prvek Hello aktualizován, která určuje, že hello **FTP – získat obsah souboru** akce získají hello *souboru obsahu* hello nové nebo upravené souboru na serveru hello FTP.      
   ![Obrázek akce FTP 5](./media/connectors-create-api-ftp/ftp-action-5.png)     
8. Uložte si práci, pak přidejte souboru toohello FTP složky tootest pracovního postupu.    

V tomto okamžiku aplikace logiky hello bylo nakonfigurované toomonitor aktivační události do složky na serveru FTP a pracovní postup inicializace hello Pokud najde soubor nové nebo upravené soubor na serveru hello FTP. 

aplikace logiky Hello také má nakonfigurované akci tooget hello obsah souboru hello nové nebo upravené.

Nyní můžete přidat další akci, jako je například hello [SQL Server – vložit řádek](connectors-create-api-sqlazure.md) akce tooinsert hello obsah hello nové nebo upravené souboru do tabulky databáze SQL.  

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/ftpconnector/). 

## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md)

