---
title: "aaaHow toomanage Azure File storage z hello portálu Azure | Microsoft Docs"
description: "Přečtěte si toouse hello Azure toomanage portálu Azure File storage."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: 385b99ac1c3d97ca79059ae8229afb53f1e825cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-file-storage-from-hello-azure-portal"></a>Jak hello toouse Azure File storage z portálu Azure
Hello [portál Azure](https://portal.azure.com) poskytuje uživatelské rozhraní pro správu Azure File storage. Můžete provádět následující akce z webového prohlížeče hello:

* Vytvoření sdílené složky
* Nahrávání a stahování souborů tooand ze sdílené složky.
* Sledování hello skutečné využití každé sdílené složky.
* Upravte kvótu velikosti sdílené složky souboru hello.
* Zkopírujte hello připojení příkazy toouse toomount sdílené složky souboru z klienta Windows nebo Linux.

## <a name="create-file-share"></a>Vytvoření sdílené složky
1. Přihlaste se toohello portálu Azure.
2. V navigační nabídce hello, klikněte na tlačítko **účty úložiště** nebo **účty úložiště (klasické)**.
    
    ![Snímek obrazovky, který ukazuje, jak toocreate sdílená hello portálu](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share1.png)

3. Vyberte svůj účet úložiště

    ![Snímek obrazovky, který ukazuje, jak toocreate sdílená hello portálu](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share2.png)

4. Vyberte službu „Files“.

    ![Snímek obrazovky, který ukazuje, jak toocreate sdílená hello portálu](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share3.png)

5. Klikněte na tlačítko "Sdílené složky" a postupujte podle toocreate odkaz hello první soubor sdílet.

    ![Snímek obrazovky, který ukazuje, jak toocreate sdílená hello portálu](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share4.png)

6. Zadejte název sdílené složky souborů hello a hello velikost hello soubor sdílené složky (až too5120 GB) toocreate svou první sdílenou. Po vytvoření hello sdílené složky ji můžete připojit z jakéhokoli souborové systému, který podporuje SMB 2.1 nebo SMB 3.0. Uživatel může klepnout na **kvóty** na hello velikost sdílené složky toochange hello hello souboru až too5120 GB. Naleznete příliš[cenové kalkulačce pro Azure](https://azure.microsoft.com/pricing/calculator/) tooestimate hello náklady na úložiště používání Azure File storage.

    ![Snímek obrazovky, který ukazuje, jak toocreate sdílená hello portálu](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share5.png)

## <a name="upload-and-download-files"></a>Ukládání a stahování souborů
1. Vyberte jednu sdílenou složku, kterou jste už vytvořili.

    ![Snímek obrazovky, který ukazuje, jak hello tooupload a stahování souborů z portálu](media/storage-file-how-to-use-files-portal/use-files-portal-upload-file1.png)

2. Klikněte na tlačítko **nahrát** otevřete hello uživatelské rozhraní pro ukládání souborů.

    ![Snímek obrazovky, který ukazuje, jak tooupload souborů z portálu hello](media/storage-file-how-to-use-files-portal/use-files-portal-upload-file2.png)

## <a name="connect-toofile-share"></a>Připojení sdílené složky toofile
-  Klikněte na tlačítko **Connect** získat hello příkazového řádku pro připojování hello sdílené složky z Windows a Linux. Pro uživatele, Linux, můžete se také podívat příliš[jak toouse Azure File storage s Linuxem](storage-how-to-use-files-linux.md) další připojení pokyny pro ostatní distribuce systému Linux.

    ![Snímek obrazovky, který ukazuje, jak toomount hello sdílené složky](media/storage-file-how-to-use-files-portal/use-files-portal-connect.png)
-  Můžete zkopírovat hello příkazů pro připojení souboru sdílenou složku v systému Windows nebo Linux a spusťte jej z jste virtuální počítač Azure nebo na místní počítač.

    ![Snímek obrazovky, který zobrazuje hello připojení příkazy pro systém Windows a Linux](media/storage-file-how-to-use-files-portal/use-files-portal-show-mount-commands.png)

**Tip:**  
toofind přístupový klíč účtu úložiště hello pro připojení, klikněte na **zobrazení přístupových klíčů pro tento účet úložiště** dole hello hello připojit stránky.

## <a name="see-also"></a>Viz také
Další informace o úložišti Azure File jsou dostupné na těchto odkazech.

* [Nejčastější dotazy](storage-files-faq.md)
* [Řešení potíží](storage-troubleshoot-file-connection-problems.md)