---
title: "Vytvoření funkce aktivované zprávami ve frontě v Azure | Dokumentace Microsoftu"
description: "Pomocí služby Azure Functions vytvoříte funkci bez serveru, kterou vyvolávají zprávy odeslané do fronty služby Azure Storage."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/08/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 77c8c8dbe6228d80062f34f4bb7fc93a1871e8c4
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a>Vytvoření funkce aktivované službou Azure Queue Storage

Zjistíte, jak vytvořit funkci, která se aktivuje při odeslání zpráv do fronty služby Azure Storage.

![Zobrazte si zprávy v protokolech.](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Požadavky

- Stáhnout a nainstalovat [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

- Předplatné Azure. Pokud ho nemáte, než začnete, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Vytvoření aplikace Azure Function App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

Dál vytvoříte v nové aplikaci Function App funkci.

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a>Vytvoření funkce aktivované frontou

1. Rozbalte aplikaci Function App a klikněte na tlačítko **+** vedle položky **Funkce**. Pokud jde o první funkci ve vaší aplikaci Function App, vyberte možnost **Vlastní funkce**. Zobrazí se kompletní sada šablon funkcí.

    ![Stručný úvod do služby Functions na webu Azure Portal](./media/functions-create-storage-queue-triggered-function/add-first-function.png)

2. Do vyhledávacího pole zadejte `queue` a zvolte jazyk požadovaný pro šablonu funkce aktivované frontou úložiště.

    ![Vyberte šablonu funkce aktivované frontou úložiště.](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)

3. Použijte nastavení uvedená v tabulce pod obrázkem.
    ![Konfigurujte funkci aktivovanou frontou úložiště.](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal-2.png)
    
    | Nastavení | Navrhovaná hodnota | Popis |
    |---|---|---|
    | **Název** | Jedinečný název v rámci aplikace Function App | Název této funkce aktivované frontou. |
    | **Název fronty**   | myqueue-items    | Název fronty, ke které se připojíte ve svém účtu úložiště. |
    | **Připojení k účtu úložiště** | AzureWebJobStorage | Můžete použít připojení k účtu úložiště, které už používá vaše aplikace Function App, nebo můžete vytvořit nové.  |    

3. Funkci vytvoříte kliknutím na **Vytvořit**.

Teď se připojíte ke svému účtu služby Azure Storage a vytvoříte frontu úložiště **myqueue-items**.

## <a name="create-the-queue"></a>Vytvoření fronty

1. Ve funkci klikněte na **Integrace**, rozbalte položku **Dokumentace**a zkopírujte údaje **Název účtu** a **Klíč účtu**. Tyto přihlašovací údaje použijete k připojení k účtu úložiště v Průzkumníku služby Azure Storage. Pokud jste se už ke svému účtu úložiště připojili, přejděte ke kroku 4.

    ![Získejte přihlašovací údaje účtu úložiště.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)

1. Spusťte nástroj [Microsoft Azure Storage Explorer](http://storageexplorer.com/), vlevo klikněte na ikonu připojení, zvolte **Use a storage account name and key** (Použít název a klíč účtu úložiště) a klikněte na **Next** (Další).

    ![Spusťte nástroj Průzkumník účtu úložiště.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)

1. Zadejte **Název účtu** a **Klíč účtu** z kroku 1, klikněte na **Další** a potom klikněte na **Připojit**.

    ![Zadejte přihlašovací údaje úložiště a připojte se.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

1. Rozbalte připojený účet úložiště, klikněte pravým tlačítkem na **Fronty**, klikněte na **Vytvořit frontu**, zadejte`myqueue-items` a potom stiskněte Enter.

    ![Vytvořte frontu úložiště.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

Teď máte frontu úložiště a můžete funkci otestovat přidáním zprávy do fronty.

## <a name="test-the-function"></a>Testování funkce

1. Zpátky na webu Azure Portal přejděte ke svojí funkci, ve spodní části stránky rozbalte **Protokoly** a ujistěte se, že není pozastavené streamování protokolů.

1. V Storage Exploreru rozbalte svůj účet úložiště, možnosti **Queues** (Fornty), a **myqueue-items** a potom klikněte na **Add message** (Přidat zprávu).

    ![Přidejte do fronty zprávu.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

1. Zadejte zprávu „Hello World!“ do pole **Text zprávy** a klikněte na **OK**.

1. Několik sekund počkejte, potom se vraťte k protokolům funkce a zkontrolujte, jestli se nová zpráva z fronty přečetla.

    ![Zobrazte si zprávy v protokolech.](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

1. Zpátky v Storage Exploreru klikněte na **Refresh** (Aktualizovat) a zkontrolujte, jestli proběhlo zpracování zprávy a jestli zpráva zmizela z fronty.

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Vytvořili jste funkci, která se spustí při přidání zprávy do fronty úložiště.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace o aktivačních událostech fronty úložiště najdete v tématu [Vazby front úložiště služby Azure Functions](functions-bindings-storage-queue.md).
