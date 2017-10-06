---
title: "aaaCreate funkce v Azure aktivovány zprávy fronty | Microsoft Docs"
description: "Použití Azure Functions toocreate bez serveru funkce, které je vyvolána zprávy odeslané tooan fronty Azure Storage."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: e9501ed336b502eaeee3fa62ec4ae085c76de0ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a>Vytvoření funkce aktivované službou Azure Queue Storage

Zjistěte, jak toocreate funkci aktivuje, když se zprávy odeslané tooan fronty Azure Storage.

![Zobrazení zpráv v protokolech hello.](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Požadavky

- Stáhněte a nainstalujte hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

- Předplatné Azure. Pokud ho nemáte, než začnete, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Vytvoření aplikace Azure Function App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

Dál vytvořte funkci v nové funkce aplikace hello.

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a>Vytvoření funkce aktivované frontou

1. Rozšířit funkce aplikace a klikněte na tlačítko hello  **+**  tlačítko vedle příliš**funkce**. Pokud je to první funkce hello ve vaší aplikaci funkce, vyberte **vlastní funkce**. Zobrazí se hello kompletní sada šablon funkcí.

    ![Funkce Rychlé spuštění stránku hello portálu Azure](./media/functions-create-storage-queue-triggered-function/add-first-function.png)

2. Vyberte hello **QueueTrigger** šablonu pro požadovaný jazyk a použít hello nastavení uvedeného v tabulce hello.

    ![Vytvoření funkce aktivované fronty úložiště hello.](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)
    
    | Nastavení | Navrhovaná hodnota | Popis |
    |---|---|---|
    | **Název fronty**   | myqueue-items    | Název hello fronty tooconnect tooin účtu úložiště. |
    | **Připojení k účtu úložiště** | AzureWebJobStorage | Můžete použít připojení účtu úložiště hello již používá aplikace funkce nebo vytvořte novou.  |
    | **Pojmenujte svoji funkci** | Jedinečný název v rámci aplikace Function App | Název této funkce aktivované frontou. |

3. Klikněte na tlačítko **vytvořit** toocreate funkce.

V dalším kroku připojit tooyour účet úložiště Azure a vytvořit hello **Moje_fronta položky** fronty úložiště.

## <a name="create-hello-queue"></a>Vytvořit frontu hello

1. Ve funkci klikněte na **Integrace**, rozbalte položku **Dokumentace**a zkopírujte údaje **Název účtu** a **Klíč účtu**. Používáte účet úložiště toohello tooconnect tyto přihlašovací údaje. Pokud už jste připojení účtu úložiště, přeskočte toostep 4.

    ![Účet hello úložiště získáte přihlašovací údaje pro připojení.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)v

1. Spustit hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/) nástroje, klikněte na tlačítko hello připojit ikonu na levé straně hello, zvolte **použít název účtu úložiště a klíč**a klikněte na tlačítko **Další**.

    ![Spusťte nástroj hello Průzkumníka účet úložiště.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)

1. Zadejte hello **název účtu** a **klíč účtu** z kroku 1, klikněte na tlačítko **Další** a potom **Connect**.

    ![Zadejte přihlašovací údaje storage hello a připojení.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

1. Rozbalte účet úložiště hello připojit, klikněte pravým tlačítkem na **fronty**, klikněte na tlačítko **fronty vytvořit**, typ `myqueue-items`, a potom stiskněte klávesu enter.

    ![Vytvořte frontu úložiště.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

Teď, když máte fronty úložiště, můžete otestovat hello funkce přidáním toohello front zpráv.

## <a name="test-hello-function"></a>Testování funkce hello

1. Zpět v hello portálu Azure, rozbalte procházet tooyour funkce hello **protokoly** dolnímu hello hello stránky a ujistěte se, že není pozastavená této protokolů streamování.

1. V Storage Exploreru rozbalte svůj účet úložiště, možnosti **Queues** (Fornty), a **myqueue-items** a potom klikněte na **Add message** (Přidat zprávu).

    ![Přidejte toohello front zpráv.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

1. Zadejte zprávu „Hello World!“ do pole **Text zprávy** a klikněte na **OK**.

1. Počkejte několik sekund poté přejděte zpět tooyour funkce protokoly a ověřte, že tuto novou zprávu hello byl načten z fronty hello.

    ![Zobrazení zpráv v protokolech hello.](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

1. Zpět v Storage Explorer, klikněte na **aktualizovat** a ověřte, že uvítací zprávu zpracovává a již není ve frontě hello.

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Vytvořili jste funkci, která spustí při přidání fronty úložiště tooa zprávu.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace o aktivačních událostech fronty úložiště najdete v tématu [Vazby front úložiště služby Azure Functions](functions-bindings-storage-queue.md).