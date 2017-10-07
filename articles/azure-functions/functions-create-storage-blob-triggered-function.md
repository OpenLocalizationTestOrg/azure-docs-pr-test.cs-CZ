---
title: "aaaCreate funkce v aktivovány úložiště objektů Blob v Azure | Microsoft Docs"
description: "Použití Azure Functions toocreate bez serveru funkce, která je volána, podle položek přidat tooAzure úložiště objektů Blob."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: d6bff41c-a624-40c1-bbc7-80590df29ded
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: acb7d29abb07a22da11d0e65d2ed54591f8e3f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-azure-blob-storage"></a>Vytvoření funkce aktivované službou Azure Blob Storage

Zjistěte, jak toocreate funkci aktivuje, když jsou soubory nahrát tooor aktualizovat v Azure Blob storage.

![Zobrazení zpráv v protokolech hello.](./media/functions-create-storage-blob-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Požadavky

+ Stáhněte a nainstalujte hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).
+ Předplatné Azure. Pokud ho nemáte, než začnete, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Vytvoření aplikace Azure Function App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

Dál vytvořte funkci v nové funkce aplikace hello.

<a name="create-function"></a>

## <a name="create-a-blob-storage-triggered-function"></a>Vytvoření funkce aktivované službou Blob Storage

1. Rozšířit funkce aplikace a klikněte na tlačítko hello  **+**  tlačítko vedle příliš**funkce**. Pokud je to první funkce hello ve vaší aplikaci funkce, vyberte **vlastní funkce**. Zobrazí se hello kompletní sada šablon funkcí.

    ![Funkce Rychlé spuštění stránku hello portálu Azure](./media/functions-create-storage-blob-triggered-function/add-first-function.png)

2. Vyberte hello **BlobTrigger** šablonu pro požadovaný jazyk a použít hello nastavení uvedeného v tabulce hello.

    ![Vytvoření funkce aktivované úložiště objektů Blob hello.](./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal.png)

    | Nastavení | Navrhovaná hodnota | Popis |
    |---|---|---|
    | **Cesta**   | mycontainer/{name}    | Monitorované umístění ve službě Blob Storage. Název souboru Hello objektu hello blob je předán hello vazbu jako hello _název_ parametr.  |
    | **Připojení k účtu úložiště** | AzureWebJobStorage | Můžete použít připojení účtu úložiště hello již používá aplikace funkce nebo vytvořte novou.  |
    | **Pojmenujte svoji funkci** | Jedinečný název v rámci aplikace Function App | Název této funkce aktivované objektem blob. |

3. Klikněte na tlačítko **vytvořit** toocreate funkce.

V dalším kroku připojit tooyour účet úložiště Azure a vytvořit hello **můj_kontejner** kontejneru.

## <a name="create-hello-container"></a>Vytvoření kontejneru hello

1. Ve funkci klikněte na **Integrace**, rozbalte položku **Dokumentace**a zkopírujte údaje **Název účtu** a **Klíč účtu**. Používáte účet úložiště toohello tooconnect tyto přihlašovací údaje. Pokud už jste připojení účtu úložiště, přeskočte toostep 4.

    ![Účet hello úložiště získáte přihlašovací údaje pro připojení.](./media/functions-create-storage-blob-triggered-function/functions-storage-account-connection.png)

1. Spustit hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/) nástroje, klikněte na tlačítko hello připojit ikonu na levé straně hello, zvolte **použít název účtu úložiště a klíč**a klikněte na tlačítko **Další**.

    ![Spusťte nástroj hello Průzkumníka účet úložiště.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-1.png)

1. Zadejte hello **název účtu** a **klíč účtu** z kroku 1, klikněte na tlačítko **Další** a potom **Connect**. 

    ![Zadejte přihlašovací údaje storage hello a připojení.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-2.png)

1. Rozbalte účet úložiště hello připojit, klikněte pravým tlačítkem na **Blob kontejnery**, klikněte na tlačítko **vytvořit kontejner objektů blob**, typ `mycontainer`, a potom stiskněte klávesu enter.

    ![Vytvořte frontu úložiště.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-create-blob-container.png)

Teď, když máte kontejner objektů blob, můžete otestovat hello funkce tím, že nahrajete soubor toohello kontejner.

## <a name="test-hello-function"></a>Testování funkce hello

1. Zpět v hello portálu Azure, rozbalte procházet tooyour funkce hello **protokoly** dolnímu hello hello stránky a ujistěte se, že není pozastavená této protokolů streamování.

1. V Průzkumníku úložišť rozbalte účet úložiště, položku **Kontejnery objektů blob** a potom položku **mycontainer**. Klikněte na **Odeslat** a potom na **Nahrát soubory…**.

    ![Nahrajte soubor toohello kontejneru objektů blob.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-upload-file-blob.png)

1. V hello **nahrání souborů** dialogovém okně klikněte na hello **soubory** pole. Vyhledejte soubor tooa v místním počítači, jako je soubor bitové kopie, vyberte ho a klikněte na tlačítko **otevřete** a potom **nahrát**.

1. Přejděte zpět tooyour funkce protokoly a ověřte, zda že byl načten tomuto objektu blob hello.

   ![Zobrazení zpráv v protokolech hello.](./media/functions-create-storage-blob-triggered-function/functions-blob-storage-trigger-view-logs.png)

    >[!NOTE]
    > Když vaše aplikace funkce běží v plánu spotřeby výchozí hello, mohou být zpoždění až tooseveral minut mezi hello objekt blob se přidán nebo aktualizován a hello funkce se aktivuje. Pokud u funkcí aktivovaných objekty blob potřebujete nízkou latenci, zvažte spuštění aplikace Function App v rámci plánu služby App Service.

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Vytvořili jste funkci, která se spustí v případě, že objekt blob je přidaný do úložiště objektů Blob tooor aktualizovat. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace o aktivačních událostech služby Blob Storage najdete v tématu [Vazby služby Azure Functions Blob Storage](functions-bindings-storage-blob.md).
