---
title: "aaaMove tooand dat z úložiště objektů Blob s Azure Storage Explorer | Microsoft Docs"
description: "Přesun dat tooand z Azure Blob Storage pomocí Průzkumníka úložiště Azure"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 10bd283f-0875-4c67-af63-6492270b7656
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 38d3bc009950c97d8474b0acceaf74814638dac0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-blob-storage-using-azure-storage-explorer"></a>Přesun dat tooand z Azure Blob Storage pomocí Průzkumníka úložiště Azure
Azure Storage Explorer je bezplatný nástroj od společnosti Microsoft, který vám umožní toowork s daty Azure Storage ve Windows, systému macOS a Linux. Toto téma popisuje, jak toouse ho tooupload a stahování dat z Azure blob storage. Hello nástroj lze stáhnout z [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Pokud používáte virtuální počítač, který byl nastaven pomocí skriptů hello poskytované [datové vědy virtuálních počítačů v Azure](machine-learning-data-science-virtual-machines.md), pak Azure Storage Explorer je již nainstalován na hello virtuálních počítačů.
> 
> [!NOTE]
> Úložiště objektů blob tooAzure dokončení úvod, najdete v části příliš[základy objektu Blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md) a [služby objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).   
> 
> 

## <a name="prerequisites"></a>Požadavky
Tento dokument předpokládá, že máte předplatné Azure, účet úložiště a hello odpovídající klíč úložiště pro tento účet. Před nahrávání nebo stahování dat, musíte znát klíč účet a název účtu úložiště Azure. 

* tooset si předplatné Azure, najdete v části [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).
* Pokyny pro vytvoření účtu úložiště a získávání účet a klíčové informace naleznete v tématu [účty Azure storage](../storage/common/storage-create-storage-account.md). Poznámka: hello přístupový klíč účtu úložiště Ujistěte se, jak je nutné tento účet toohello klíče tooconnect s nástrojem Azure Storage Explorer hello.
* Nástroj Azure Storage Explorer Hello lze stáhnout z [Microsoft Azure Storage Explorer](http://storageexplorer.com/). Přijměte výchozí hodnoty hello během instalace.

<a id="explorer"></a>

## <a name="use-azure-storage-explorer"></a>Pomocí Průzkumníka úložiště Azure
Dobrý den, jak následující kroky dokumentu tooupload nebo stahování dat pomocí Průzkumníka úložiště Azure. 

1. Spusťte Průzkumníka úložiště Microsoft Azure.
2. toobring až hello **přihlašovací účet tooyour...**  průvodci vyberte **nastavení účtu Azure** ikonu, pak **přidat účet** a zadat přihlašovací údaje. ![Přidat účet úložiště Azure](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3. toobring až hello **připojit tooAzure úložiště** průvodce, vyberte hello **připojení úložiště tooAzure** ikonu. ![Připojit tooAzure úložiště](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Na hello zadejte hello přístupový klíč účtu úložiště Azure **připojit tooAzure úložiště** průvodce a potom **Další**. ![Připojit tooAzure úložiště](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Zadejte název účtu úložiště v hello **název účtu** pole a pak vyberte **Další**. ![Připojit externí úložiště](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Přidat účet úložiště Hello by měl být nyní uvedený. toocreate kontejner objektů blob v účtu úložiště, klikněte pravým tlačítkem na hello **kontejnery objektů Blob** uzlu v daném účtu, vyberte **vytvořit kontejner objektů Blob**a zadejte název.
7. kontejner tooa tooupload dat, vyberte hello cílový kontejner a klikněte na tlačítko hello **nahrát** tlačítko.![ Účty úložiště](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Klikněte na hello **...**  toohello napravo od hello **soubory** pole, vyberte jeden nebo více souborů tooupload hello systém souborů a klikněte na **nahrát** toobegin nahrávání souborů hello.![ Nahrání souborů](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
9. toodownload dat, výběrem hello objektů blob v kontejneru toodownload odpovídající hello a klikněte na tlačítko **Stáhnout**. ![Stažení souborů](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)

