---
title: "aaaCreate funkce v Azure aktivovány zprávy fronty | Microsoft Docs"
description: "Použití Azure Functions toocreate bez serveru funkce, které je vyvolána zprávy odeslané tooan fronty Azure Storage."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 0b609bc0-c264-4092-8e3e-0784dcc23b5d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/17/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 44db90fa80bf77e31bf53dddabd7136de5800b11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-messages-tooan-azure-storage-queue-using-functions"></a>Přidat fronty Azure Storage tooan zprávy pomocí funkce

V Azure Functions poskytují vstupní a výstupní vazby dat deklarativní způsob tooconnect tooexternal služby z funkce. V tomto tématu zjistěte, jak tooupdate existující funkce přidáním výstup vazby, která odesílá zprávy tooAzure Queue storage.  

![Zobrazení zpráv v protokolech hello.](./media/functions-integrate-storage-queue-output-binding/functions-integrate-storage-binding-in-portal.png)

## <a name="prerequisites"></a>Požadavky 

[!INCLUDE [Previous topics](../../includes/functions-quickstart-previous-topics.md)]

* Nainstalujte hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

## <a name="add-binding"></a>Přidání výstupní vazby
 
1. Rozbalte aplikaci Function App i funkci.

2. Vyberte **integrací** a **+ nový výstupní**, zvolte **Azure Queue storage** a zvolte **vyberte**.
    
    ![Přidejte funkci tooa vazby výstupní fronty úložiště v hello portálu Azure.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding.png)

3. Použijte hello nastavení uvedeného v tabulce hello: 

    ![Přidejte funkci tooa vazby výstupní fronty úložiště v hello portálu Azure.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding-2.png)

    | Nastavení      |  Navrhovaná hodnota   | Popis                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Název fronty**   | myqueue-items    | Název Hello hello fronty tooconnect tooin účtu úložiště. |
    | **Připojení k účtu úložiště** | AzureWebJobStorage | Můžete použít připojení účtu úložiště hello již používá aplikace funkce nebo vytvořte novou.  |
    | **Název parametru zprávy** | outputQueueItem | Název Hello hello výstupní parametr vazby. | 

4. Klikněte na tlačítko **Uložit** tooadd hello vazby.
 
Teď, když máte vazbu výstup, který je definován, je nutné tooupdate hello kód toouse hello vazby tooadd tooa fronty zpráv.  

## <a name="update-hello-function-code"></a>Aktualizovat kód funkce hello

1. Vyberte funkce toodisplay hello funkce kódu v editoru hello. 

2. Pro C# funkci, aktualizovat svou definici funkce takto tooadd hello **outputQueueItem** úložiště parametr vazby. V případě funkce v jazyce JavaScript tento krok přeskočte.

    ```cs   
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, 
        ICollector<string> outputQueueItem, TraceWriter log)
    {
        ....
    }
    ```

3. Přidejte následující kód toohello funkce těsně před hello metoda vrátí hello. Použití hello odpovídající fragment kódu pro jazyk hello funkce.

    ```javascript
    context.bindings.outputQueueItem = "Name passed toohello function: " + 
                (req.query.name || req.body.name);
    ```

    ```cs
    outputQueueItem.Add("Name passed toohello function: " + name);     
    ```

4. Vyberte **Uložit** toosave změny.

Aktivace protokolu HTTP toohello předaná hodnota Hello je součástí přidané toohello front zpráv.
 
## <a name="test-hello-function"></a>Testování funkce hello 

1. Po uložení změn kódu hello vyberte **spustit**. 

    ![Přidejte funkci tooa vazby výstupní fronty úložiště v hello portálu Azure.](./media/functions-integrate-storage-queue-output-binding/functions-test-run-function.png)

2. Zkontrolujte toomake protokoly hello se, zda text hello funkce proběhla úspěšně. Novou frontu s názvem **outqueue** hello runtime Functions při hello výstup vazby se používá nejprve vytvoří ve vašem účtu úložiště.

V dalším kroku se můžete připojit tooyour úložiště účet tooverify hello novou frontu a uvítací zprávu, že jste přidali tooit. 

## <a name="connect-toohello-queue"></a>Připojit toohello fronty

Přeskočit hello první tři kroky, pokud je již nainstalován Storage Explorer a připojení se účet úložiště tooyour.    

1. Ve funkci, zvolte **integrací** a hello nové **Azure Queue storage** výstup vazby, pak rozbalte **dokumentaci**. Zkopírujte nastavení **Název účtu** i **Klíč účtu**. Používáte účet úložiště toohello tooconnect tyto přihlašovací údaje.
 
    ![Účet hello úložiště získáte přihlašovací údaje pro připojení.](./media/functions-integrate-storage-queue-output-binding/function-get-storage-account-credentials.png)

2. Spustit hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/) nástroj, vyberte hello připojit ikonu na levé straně hello, zvolte **použít název účtu úložiště a klíč**a vyberte **Další**.

    ![Spusťte nástroj hello Průzkumníka účet úložiště.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-1.png)
    
3. Vložení hello **název účtu** a **klíč účtu** z kroku 1 do jejich odpovídající pole a pak vyberte **Další**, a **Connect**. 
  
    ![Vložte hello úložiště přihlašovací údaje a připojení.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-2.png)

4. Rozbalte účet úložiště hello připojené, rozbalte položku **fronty** a ověřte, že frontu s názvem **Moje_fronta položky** existuje. Měli byste taky vidět zprávu již ve frontě hello.  
 
    ![Vytvořte frontu úložiště.](./media/functions-integrate-storage-queue-output-binding/function-queue-storage-output-view-queue.png)
 

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Jste přidali tooan vazby výstup pro existující funkce. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace o vazby tooQueue úložiště najdete v tématu [vazby fronty Azure Storage funkce](functions-bindings-storage-queue.md). 



