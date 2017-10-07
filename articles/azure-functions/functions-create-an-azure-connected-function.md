---
title: "aaaCreate funkce, která se připojuje tooAzure služby | Microsoft Docs"
description: "Azure Functions toocreate aplikaci bez serveru, který se připojuje tooother Azure pomocí služby."
services: functions
documentationcenter: dev-center-name
author: yochay
manager: manager-alias
editor: 
tags: 
keywords: "funkce azure, funkce, zpracování událostí, webhook, dynamické výpočty, architektura bez serverů"
ms.assetid: ab86065d-6050-46c9-a336-1bfc1fa4b5a1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 9d1f7d3b236f8d2c1a404c76aee410f6d458fb7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-functions-toocreate-a-function-that-connects-tooother-azure-services"></a>Azure Functions toocreate funkce, která se připojuje tooother Azure pomocí služby

Toto téma ukazuje, jak toocreate funkce v Azure Functions, která naslouchá toomessages na Azure Storage fronty a kopie hello zprávy toorows v tabulce Azure Storage. Funkce spustí časovač je použité tooload zprávy do fronty hello. Druhý funkce přečte z fronty hello a zapíše zprávy toohello tabulky. Hello fronty a tabulky hello jsou vytvořené pro vás Azure Functions podle definice vazby hello. 

věcí toomake zajímavějšího, jednu funkci je napsána v jazyce JavaScript a hello jiných je napsán v C# skript. To ukazuje, jak aplikace Function App může obsahovat funkce v různých jazycích. 

Předvedení tohoto scénáře zobrazuje [video na webu Channel 9](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player).

## <a name="create-a-function-that-writes-toohello-queue"></a>Vytvoří funkci, která zapisuje toohello fronty

Před připojením tooa fronty úložiště, musíte toocreate funkci, která načte fronta zpráv hello. Tato funkce JavaScript, která používá aktivaci časovačem, který zapíše zprávu fronty toohello každých 10 sekund. Pokud nemáte účet Azure, podívejte se na hello [zkuste Azure Functions](https://functions.azure.com/try) prostředí, nebo [vytvořit účet Azure zdarma](https://azure.microsoft.com/free/).

1. Přejděte toohello portál Azure a vyhledejte aplikaci funkce.

2. Klikněte na **Nová funkce** > **TimerTrigger-JavaScript**. 

3. Název funkce hello **FunctionsBindingsDemo1**, zadejte hodnotu výrazu cron `0/10 * * * * *` pro **plán**a potom klikněte na **vytvořit**.
   
    ![Přidání funkce aktivované časovačem](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)

    Nyní jste vytvořili funkci aktivovanou časovačem, která se spouští každých 10 sekund.

5. Na hello **vývoj** , klikněte na **protokoly** a zobrazit hello aktivity v protokolu hello. Jak vidíte, každých 10 sekund se zapíše položka protokolu.
   
    ![Zobrazení hello protokolu tooverify hello funkce funguje](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

## <a name="add-a-message-queue-output-binding"></a>Přidání výstupní vazby fronty zpráv

1. Na hello **integrací** , zvolte **nový výstupní** > **Azure Queue Storage** > **vyberte**.

    ![Přidání funkce časovače triggeru](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

2. Zadejte `myQueueItem` pro **název parametru zpráva** a `functions-bindings` pro **název fronty**, vyberte existující **připojení účtu úložiště** nebo klikněte na tlačítko **nové** toocreate úložiště účtu připojení a pak klikněte na **Uložit**.  

    ![Vytvoření fronty úložiště toohello vazby výstup hello](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab2.png)

1. Zpět v hello **vývoj** kartě, doplňovací hello následující funkce toohello kódu:
   
    ```javascript
   
    function myQueueItem() 
    {
        return {
            msg: "some message goes here",
            time: "time goes here"
        }
    }
   
    ```
2. Vyhledejte hello *Pokud* příkaz přibližně řádek 9 hello funkce a vložení hello následující kód po tento příkaz.
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueueItem = toBeQed;
   
    ```  
   
    Tento kód vytvoří **myQueueItem** a nastaví její **čas** vlastnost toohello aktuální časové razítko. Potom přidá hello nové fronty položky toohello kontextu **myQueueItem** vazby.

3. Klikněte na **Uložit a spustit**.

## <a name="view-storage-updates-by-using-storage-explorer"></a>Zobrazení aktualizací úložiště pomocí Storage Exploreru
Můžete ověřit, že funkce funguje zobrazením zprávy ve frontě hello, kterou jste vytvořili.  Tooyour fronty úložiště můžete připojit pomocí Průzkumníku cloudu v sadě Visual Studio. Ale hello portál je účet úložiště snadno tooconnect tooyour pomocí Microsoft Azure Storage Explorer.

1. V hello **integrací** , klikněte na frontu výstup vazby > **dokumentace**, odkrýt hello připojovací řetězec pro váš účet úložiště a zkopírujte hodnotu hello. Můžete použít tento účet úložiště tooyour tooconnect hodnotu.

    ![Stáhnutí Azure Storage Exploreru](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab3.png)


2. Pokud jste tak již neučinili, stáhněte a nainstalujte [Microsoft Azure Storage Explorer](http://storageexplorer.com). 
 
3. Ve Storage Exploreru, klikněte na tlačítko hello připojení ikona úložiště tooAzure, vložte hello připojovací řetězec v poli hello a dokončete průvodce hello.

    ![Přidání připojení ve Storage Exploreru](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-storage-explorer.png)

4. V části **místní a připojené**, rozbalte položku **účty úložiště** > váš účet úložiště > **fronty** > **funkce vazby**a ověřte, zda jsou zprávy zapisovány toohello fronty.

    ![Zobrazení zpráv ve frontě hello](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer.png)

    Pokud hello fronta neexistuje nebo je prázdný, je pravděpodobně problém s vaší vazba funkce nebo kódu.

## <a name="create-a-function-that-reads-from-hello-queue"></a>Vytvoří funkci, která čte z fronty hello

Teď, když máte zprávy přidávané toohello fronty, můžete vytvořit další funkce, která čte z fronty hello a zápisy hello zprávy trvale tooan tabulky Azure Storage.

1. Klikněte na **Nová funkce** > **QueueTrigger-CSharp**. 
 
2. Název funkce hello `FunctionsBindingsDemo2`, zadejte **funkce vazby** v hello **název fronty** pole, vybrat existující účet úložiště nebo vytvořit a pak klikněte na tlačítko **vytvořit**.

    ![Přidání funkce časovače výstupní fronty](./media/functions-create-an-azure-connected-function/function-demo2-new-function.png) 

3. (Volitelné) Můžete ověřit, že nové funkce hello funguje zobrazením hello novou frontu na Storage Explorer jako před. Můžete také použít Průzkumník cloudu v sadě Visual Studio.  

4. (Volitelné) Aktualizujte hello **funkce vazby** ve frontě a Všimněte si, že položky byly odebrány z fronty hello. Hello odebrání dochází, je funkce hello vázané toohello **funkce vazby** jako vstupní funkce aktivační události a hello čte hello fronty ve frontě. 
 
## <a name="add-a-table-output-binding"></a>Přidání výstupní vazby tabulky

1. V aplikaci FunctionsBindingsDemo2 klikněte na **Integrace** > **Nový výstup** > **Azure Table Storage** > **Vybrat**.

    ![Přidat tabulku vazby tooan Azure Storage](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png) 

2. Zadejte `functionbindings` jako **Název tabulky** a `myTable` jako **Název parametru tabulky**, zvolte **Připojení účtu úložiště** nebo vytvořte nové, a potom klikněte na **Uložit**.

    ![Nakonfigurujte vazbu tabulky úložiště hello](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab2.png)
   
3. V hello **vývoj** kartě, nahraďte existující kód funkce hello hello následující:
   
    ```cs
    
    using System;
    
    public static void Run(QItem myQueueItem, ICollector<TableItem> myTable, TraceWriter log)
    {    
        TableItem myItem = new TableItem
        {
            PartitionKey = "key",
            RowKey = Guid.NewGuid().ToString(),
            Time = DateTime.Now.ToString("hh.mm.ss.ffffff"),
            Msg = myQueueItem.Msg,
            OriginalTime = myQueueItem.Time    
        };
        
        // Add hello item toohello table binding collection.
        myTable.Add(myItem);
    
        log.Verbose($"C# Queue trigger function processed: {myItem.RowKey} | {myItem.Msg} | {myItem.Time}");
    }
    
    public class TableItem
    {
        public string PartitionKey {get; set;}
        public string RowKey {get; set;}
        public string Time {get; set;}
        public string Msg {get; set;}
        public string OriginalTime {get; set;}
    }
    
    public class QItem
    {
        public string Msg { get; set;}
        public string Time { get; set;}
    }
    ```
    Hello **TableItem** třída reprezentuje řádek v tabulce hello úložiště a přidejte položku toohello hello `myTable` kolekce **TableItem** objekty. Je nutné nastavit hello **PartitionKey** a **RowKey** vlastnosti toobe možné tooinsert do tabulky hello.

4. Klikněte na **Uložit**.  Nakonec můžete ověřit hello funkce funguje zobrazením hello tabulky v Průzkumníka úložiště nebo cloudu Průzkumníka Visual Studio.

5. (Volitelné) Ve vašem účtu úložiště v Storage Explorer rozbalte **tabulky** > **functionsbindings** a ověřte, zda jsou řádky přidán toohello tabulky. Můžete provést stejný hello v Průzkumníku cloudu v sadě Visual Studio.

    ![Zobrazení řádků v tabulce hello](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer2.png)

    Pokud hello tabulka neexistuje nebo je prázdný, je pravděpodobně problém s vaší vazba funkce nebo kódu. 
 
[!INCLUDE [More binding information](../../includes/functions-bindings-next-steps.md)]

## <a name="next-steps"></a>Další kroky
Další informace o Azure Functions najdete v tématu hello následující témata:

* [Referenční informace pro vývojáře Azure Functions](functions-reference.md)  
  Referenční informace pro programátory týkající se kódování funkcí a definování triggerů a vazeb.
* [Testování Azure Functions](functions-test-a-function.md)  
  Toto téma popisuje různé nástroje a techniky pro testování funkcí.
* [Jak tooscale Azure Functions](functions-scale.md)  
  Popisuje plány služby, které jsou dostupné s Azure Functions, včetně hello spotřeba hostingový plán a jak toochoose hello správného plánu. 

[!INCLUDE [Getting help note](../../includes/functions-get-help.md)]

