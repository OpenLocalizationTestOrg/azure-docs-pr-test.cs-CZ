---
title: "Naučte se používat konektor FTP v aplikace logiky | Microsoft Docs"
description: "Vytvoření aplikace logiky službou Azure App service. Připojte k serveru FTP spravovat soubory. Můžete provádět různé akce, jako je například nahrávání, aktualizovat, získání a odstranit soubory v serveru FTP."
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
ms.openlocfilehash: 61bfbedfd4f1e84b6976099323a32f3a720634c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-ftp-connector"></a>Začínáme s konektor FTP
Pomocí konektoru serveru FTP monitorovat, spravovat a vytvoření souborů na serveru FTP. 

Chcete-li použít [všechny konektory](apis-list.md), musíte nejprve vytvořit aplikaci logiky. Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-ftp"></a>Připojení k serveru FTP
Než se aplikace logiky k jakékoli služby, musíte nejprve vytvořit *připojení* ke službě. A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.  

### <a name="create-a-connection-to-ftp"></a>Umožňuje vytvořit připojení k serveru FTP
> [!INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a>Použít aktivační událost FTP
Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu definované v aplikaci logiky. [Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!IMPORTANT]
> Konektor FTP vyžaduje serveru FTP, který je dostupný z Internetu a je nakonfigurován na provoz s pasivní režim. Konektor FTP je taky **není kompatibilní s implicitní FTPS (FTP přes protokol SSL)**. Konektor FTP podporuje pouze explicitní FTPS (FTP přes protokol SSL).  
> 
> 

V tomto příkladu I vám ukáže, jak používat **FTP – Pokud je soubor přidat ani upravit** aktivovat Pokud chcete spustit pracovní postup aplikace logiky, pokud je přidán do, nebo změny souboru na serveru FTP. V příklad enterprise můžete použít této aktivační události k monitorování složky FTP pro nové soubory, které představují objednávek zákazníků.  Poté můžete použít akci konektor FTP, jako **získat obsah souboru** získat obsah pořadí pro další zpracování a úložiště v databázi objednávky.

1. Zadejte *ftp* do vyhledávacího pole v designeru aplikace logiky zvolte **FTP – Pokud je soubor přidat ani upravit** aktivační události   
   ![Obrázek aktivační událost FTP 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
   **Je přidána nebo upravena souboru** otevře se ovládací prvek  
   ![Obrázek aktivační událost FTP 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
2. Vyberte **...**  nachází na pravé straně ovládacího prvku. Otevře se ovládací prvek pro výběr složky  
   ![Obrázek aktivační událost FTP 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
3. Vyberte  **>**  (šipka doprava) a procházet a najít složku, kterou chcete sledovat pro nové nebo upravení soubory. Vyberte složku a složku se nyní zobrazí v oznámení **složky** ovládacího prvku.  
   ![FTP aktivační událost obrázek 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)   

Aplikace logiky v tomto okamžiku je nakonfigurovaná s aktivační událost, která zahájí spuštění ostatních triggery a akce v pracovním postupu po upravit nebo vytvořit ve složce konkrétní FTP souboru. 

> [!NOTE]
> Pro aplikace logiky být funkční musí obsahovat nejméně jedna aktivační událost a jedna akce. Postupujte podle kroků v další části a přidejte akci.  
> 
> 

## <a name="use-a-ftp-action"></a>Pomocí akce FTP
Akce je operace prováděné definované v aplikaci logiky pracovního postupu. [Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

Teď, když jste přidali aktivační událost, použijte následující postup přidání akce, která bude získán obsah souboru nové nebo upravené nalezena. aktivační událost.    

1. Vyberte **+ nový krok** přidat akce, která má být získán obsah souboru na serveru FTP  
2. Vyberte **přidat akci** odkaz.  
   ![Obrázek akce FTP 1](./media/connectors-create-api-ftp/ftp-action-1.png)  
3. Zadejte *FTP* k vyhledání všech akcí souvisejících s FTP.
4. Vyberte **FTP – získat obsah souboru** jako akce provést v případě nové nebo upravené soubor naleznete ve složce serveru FTP.      
   ![Obrázek akce FTP 2](./media/connectors-create-api-ftp/ftp-action-2.png)  
   **Získat obsah souboru** řízení otevře. **Poznámka:**: Zobrazí se výzva k autorizaci aplikace logiky k přístupu k účtu serveru FTP, pokud jste tak dosud neučinili dříve.  
   ![Obrázek akce FTP 3](./media/connectors-create-api-ftp/ftp-action-3.png)   
5. Vyberte **soubor** ovládací prvek (mezeru nacházel pod **souboru***). Zde můžete vytvořit různé vlastnosti ze souboru nové nebo upravené nalezena na serveru FTP.  
6. Vyberte **souboru obsahu** možnost.  
   ![Obrázek akce FTP 4](./media/connectors-create-api-ftp/ftp-action-4.png)   
7. Ovládací prvek se aktualizuje, což indikuje, že **FTP – získat obsah souboru** akce získáte *souboru obsahu* nové nebo upravené souboru na serveru FTP.      
   ![Obrázek akce FTP 5](./media/connectors-create-api-ftp/ftp-action-5.png)     
8. Uložte si práci, pak přidá soubor do složky FTP k testování pracovního postupu.    

Aplikace logiky v tomto okamžiku je nakonfigurovaná s aktivační události k monitorování složky na serveru FTP a zahájit pracovního postupu, pokud najde soubor nové nebo upravené soubor na serveru FTP. 

Aplikace logiky také nakonfigurovaný pomocí akce má být získán obsah souboru nové nebo upravené.

Nyní můžete přidat další akci, jako [SQL Server – vložit řádek](connectors-create-api-sqlazure.md) akci vložení obsah souboru nové nebo upravené do tabulky databáze SQL.  

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/ftpconnector/). 

## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md)

