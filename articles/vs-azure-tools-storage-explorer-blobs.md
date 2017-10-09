---
title: "aaaManage prostředků Azure Blob Storage pomocí Storage Exploreru (Preview) | Microsoft Docs"
description: "Správa objektů Blob v Azure kontejnerům a objektům BLOB pomocí Storage Exploreru (Preview)"
services: storage
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 2f09e545-ec94-4d89-b96c-14783cc9d7a9
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 503dd061b205875da127378ab48e8d465800fc09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>Správa prostředků Azure Blob Storage pomocí Storage Exploreru (Preview)
## <a name="overview"></a>Přehled
[Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) je služba pro ukládání velkého objemu nestrukturovaných dat, jako je například textu nebo binárních dat, která je přístupná z kdekoli v hello, world pomocí protokolů HTTP nebo HTTPS.
Data tooexpose úložiště objektů Blob můžete použít veřejně toohello svět nebo data aplikací toostore soukromě. V tomto článku se dozvíte jak toouse toowork Storage Explorer (Preview) s kontejnery objektů blob a objekty BLOB.

## <a name="prerequisites"></a>Požadavky
toocomplete hello kroky v tomto článku, budete potřebovat následující hello:

* [Stažení a instalace Storage Exploreru (Preview)](http://www.storageexplorer.com)
* [Připojení účtu úložiště Azure tooa nebo služby](vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Vytvořte kontejner objektů blob
Všechny objekty BLOB se musí nacházet v kontejneru objektů blob, který je jednoduše logické seskupení objektů BLOB. Účet může obsahovat neomezený počet kontejnerů a každý kontejner můžete pojmout neomezený počet objektů BLOB.

Hello následující kroky popisují jak toocreate kontejner objektů blob v rámci Storage Explorer (Preview).

1. Otevřete Storage Explorer (Preview).
2. V levém podokně hello rozbalte účet hello úložiště, ve kterém chcete kontejner objektů blob toocreate hello.
3. Klikněte pravým tlačítkem na **kontejnery objektů Blob**a - hello kontextové nabídce vyberte **vytvořit kontejner objektů Blob**.

   ![Vytvoření objektu blob kontejnery kontextové nabídky][0]
4. V textovém poli se zobrazí pod hello **kontejnery objektů Blob** složky. Zadejte název hello kontejnerech objektů blob. V tématu hello [pravidla pojmenování kontejneru](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) části Seznam pravidel a omezení pro pojmenovávání kontejnerů objektů blob.

   ![Vytvoření textového pole kontejnery objektů Blob][1]
5. Stiskněte klávesu **Enter** po dokončení kontejneru objektu blob hello toocreate nebo **Esc** toocancel. Po úspěšném vytvoření kontejneru objektů blob hello se zobrazí v části hello **kontejnery objektů Blob** složku pro hello vybraný účet úložiště.

   ![Vytvořit kontejner objektů BLOB][2]

## <a name="view-a-blob-containers-contents"></a>Zobrazit obsah kontejner objektů blob
Kontejnery objektů BLOB obsahovat objekty BLOB a složky (které mohou obsahovat také objekty BLOB).

Hello následující kroky popisují jak tooview hello obsah kontejneru objektů blob v rámci Storage Explorer (Preview):

1. Otevřete Storage Explorer (Preview).
2. V levém podokně hello rozbalte účet úložiště hello obsahující chcete tooview kontejner objektů blob hello.
3. Rozbalte účet úložiště hello **kontejnery objektů Blob**.
4. Klikněte pravým tlačítkem na kontejner objektů blob hello je chcete tooview a - hello kontextové nabídce vyberte **otevřete Editor kontejner objektů Blob**.
   Dvakrát klikněte na kontejner objektů blob hello chcete tooview.

   ![Otevřete blob kontejneru editor kontextové nabídky][19]
5. Hello hlavním podokně se zobrazí obsah kontejneru objektů blob hello.

   ![Editor kontejner objektů BLOB][3]

## <a name="delete-a-blob-container"></a>Odstranit kontejner objektů blob
Kontejnery objektů BLOB můžete snadno vytvořit a odstraněn podle potřeby. (toosee jak toodelete jednotlivé objekty BLOB, najdete v části toohello [Správa objektů BLOB v kontejneru objektů blob](#managing-blobs-in-a-blob-container).)

Hello následující kroky popisují jak toodelete kontejner objektů blob v rámci Storage Explorer (Preview):

1. Otevřete Storage Explorer (Preview).
2. V levém podokně hello rozbalte účet úložiště hello obsahující chcete tooview kontejner objektů blob hello.
3. Rozbalte účet úložiště hello **kontejnery objektů Blob**.
4. Klikněte pravým tlačítkem na kontejner objektů blob hello je chcete toodelete a - hello kontextové nabídce vyberte **odstranit**.
   Můžete také stisknout **odstranit** toodelete hello aktuálně vybraného objektu blob kontejneru.

   ![Odstranit objekt blob kontejneru kontextové nabídky][4]
5. Vyberte **Ano** toohello dialogové okno potvrzení.

   ![Odstranit objekt blob kontejneru potvrzení][5]

## <a name="copy-a-blob-container"></a>Zkopírujte kontejner objektů blob
Storage Explorer (Preview) vám umožní toocopy schránky toohello kontejner objektů blob a potom vložení, který kontejner objektů blob do jiný účet úložiště. (toosee jak toocopy jednotlivé objekty BLOB, najdete v části toohello [Správa objektů BLOB v kontejneru objektů blob](#managing-blobs-in-a-blob-container).)

Hello následující kroky ukazují, jak toocopy kontejner objektů blob z jednoho úložiště účet tooanother.

1. Otevřete Storage Explorer (Preview).
2. V levém podokně hello rozbalte účet úložiště hello obsahující chcete toocopy kontejner objektů blob hello.
3. Rozbalte účet úložiště hello **kontejnery objektů Blob**.
4. Klikněte pravým tlačítkem na kontejner objektů blob hello je chcete toocopy a - hello kontextové nabídce vyberte **kontejner objektů Blob kopie**.

   ![Kopírovat objekt blob kontejneru kontextové nabídky][6]
5. Klikněte pravým tlačítkem na požadované hello "target" účet úložiště, do kterého chcete kontejner objektů blob hello toopaste a - hello kontextové nabídce vyberte **kontejner objektů Blob vložení**.

   ![Vložení objektu blob kontejneru kontextové nabídky][7]

## <a name="get-hello-sas-for-a-blob-container"></a>Získat hello SAS pro kontejner objektů blob
A [sdílený přístupový podpis (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md) poskytuje Delegovaný přístup tooresources ve vašem účtu úložiště.
To znamená, že můžete udělit, že klient omezené oprávnění tooobjects ve vašem účtu úložiště v zadaném časovém intervalu a zadanou sadu oprávnění, aniž by museli sdílet klíče pro přístup k účtu.

Hello následující kroky popisují jak toocreate SAS pro kontejner objektů blob:

1. Otevřete Storage Explorer (Preview).
2. V levém podokně hello rozbalte účet úložiště hello obsahující kontejner objektů blob hello, pro kterou chcete tooget SAS.
3. Rozbalte účet úložiště hello **kontejnery objektů Blob**.
4. Klikněte pravým tlačítkem na kontejner objektů blob požadované hello a - hello kontextové nabídce vyberte **sdíleného přístupového podpisu**.

   ![Získat SAS kontextové nabídky][8]
5. V hello **sdíleného přístupového podpisu** dialogové okno, zadejte hello zásad, spuštění a datum vypršení platnosti, časové pásmo a chcete pro prostředek hello úrovně přístupu.

   ![Získat možnosti SAS][9]
6. Jakmile budete hotovi, zadání možností hello SAS, vyberte **vytvořit**.
7. Druhý **sdíleného přístupového podpisu** poté zobrazí dialogové okno, seznamy hello kontejner objektů blob společně s hello adresy URL a QueryStrings můžete použít tooaccess hello prostředků úložiště.
   Vyberte **kopie** další URL toohello chcete toocopy toohello schránky.

   ![Zkopírování adresy URL SAS][10]
8. Až budete hotovi, vyberte **Zavřít**.

## <a name="manage-access-policies-for-a-blob-container"></a>Správa zásad přístupu pro kontejner objektů blob
Hello následující kroky popisují jak toomanage (přidání a odebrání) získat přístup k zásadám pro kontejner objektů blob:

1. Otevřete Storage Explorer (Preview).
2. V levém podokně hello rozbalte účet úložiště hello obsahující kontejner objektů blob hello jejichž zásady přístupu, které chcete toomanage.
3. Rozbalte účet úložiště hello **kontejnery objektů Blob**.
4. Vyberte kontejner objektů blob požadované hello a - z kontextové nabídky hello - vyberte **spravovat zásady přístupu**.

   ![Místní nabídka Spravovat zásady přístupu][11]
5. Hello **zásady přístupu** dialogové okno se zobrazí seznam všechny zásady přístupu, které jsou vytvořeny již pro kontejner objektů blob vybrané hello.

   ![Možnosti zásad přístupu][12]        
6. Pomocí těchto kroků v závislosti na úlohu správy zásad přístupu hello:

   * **Přidání nové zásady přístupu:** Vyberte **Přidat**. Po vygenerování hello **zásady přístupu** dialogové okno se zobrazí hello nově přidaná přístup zásad (s výchozí nastavení).
   * **Upravit zásady přístupu** – proveďte veškeré požadované úpravy a vyberte **Uložit**.
   * **Odebrat zásady přístupu** – vyberte **odebrat** další zásady přístupu toohello chcete tooremove.

## <a name="set-hello-public-access-level-for-a-blob-container"></a>Nastavit hello úroveň veřejný přístup pro kontejner objektů blob
Ve výchozím nastavení, každý kontejner objektů blob je nastaven příliš "Žádné veřejný přístup".

Hello následující kroky popisují toospecify veřejné přístupu úrovně pro kontejner objektů blob.

1. Otevřete Storage Explorer (Preview).
2. V levém podokně hello rozbalte účet úložiště hello obsahující kontejner objektů blob hello jejichž zásady přístupu, které chcete toomanage.
3. Rozbalte účet úložiště hello **kontejnery objektů Blob**.
4. Vyberte kontejner objektů blob požadované hello a - z kontextové nabídky hello - vyberte **nastavit úroveň veřejný přístup**.

   ![Nastavení úrovně kontextovou nabídku veřejného přístupu][13]
5. V hello **nastavit úroveň veřejný přístup kontejneru** dialogové okno, zadejte úroveň přístupu hello potřeby.

   ![Nastavení možností úrovně veřejného přístupu][14]
6. Vyberte **Použít**.

## <a name="managing-blobs-in-a-blob-container"></a>Správa objektů BLOB v kontejneru objektů blob
Po vytvoření kontejneru objektů blob, můžete nahrát objekt blob kontejneru objektů blob toothat, stažení objektů blob tooyour místní počítač, otevřete objekt blob na místním počítači a mnoho dalšího.

Hello následující kroky ukazují, jak toomanage hello objekty BLOB (a složky) v kontejneru objektů blob.

1. Otevřete Storage Explorer (Preview).
2. V levém podokně hello rozbalte účet úložiště hello obsahující chcete toomanage kontejner objektů blob hello.
3. Rozbalte účet úložiště hello **kontejnery objektů Blob**.
4. Dvakrát klikněte na kontejner objektů blob hello chcete tooview.
5. Hello hlavním podokně se zobrazí obsah kontejneru objektů blob hello.

   ![Kontejner objektů blob zobrazení][3]
6. Hello hlavním podokně se zobrazí obsah kontejneru objektů blob hello.
7. Postupujte podle těchto kroků v závislosti na úkolu hello chcete tooperform:

   * **Nahrát kontejner objektů blob tooa soubory**

     1. Na panelu nástrojů hello hlavním podokně vyberte **nahrát**a potom **nahrání souborů** z rozevírací nabídky hello.

        ![Nahrát soubory nabídky][15]
     2. V hello **nahrání souborů** dialogové okno, vyberte hello třemi tečkami (**...** ) na pravé straně hello hello tlačítko **soubory** textového pole tooselect hello soubory chcete tooupload.

        ![Nahrávání souborů možnosti][16]
     3. Zadejte typ hello **Blob typu**. článek Hello [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) hello rozdíly mezi hello vysvětluje různé typy objektů blob.
     4. Volitelně zadejte cílovou složku, do kterého se nahraje hello vybrané soubory. Pokud hello cílové složce neexistuje, bude vytvořen.
     5. Vyberte **Nahrát**.
   * **Nahrát kontejneru objektů blob tooa složky**

     1. Na panelu nástrojů hello hlavním podokně vyberte **nahrát**a potom **nahrát složky** z rozevírací nabídky hello.

        ![Nabídka Nahrát složku][17]
     2. V hello **nahrávání složky** dialogové okno, vyberte hello třemi tečkami (**...** ) na pravé straně hello hello tlačítko **složky** textové pole tooselect hello složku, jejíž obsah, chcete tooupload.

        ![Nahrát Možnosti složky][18]
     3. Zadejte typ hello **Blob typu**. článek Hello [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) hello rozdíly mezi hello vysvětluje různé typy objektů blob.
     4. Volitelně zadejte cílovou složku, do které hello se nahraje obsah vybrané složky. Pokud hello cílové složce neexistuje, bude vytvořen.
     5. Vyberte **Nahrát**.
   * **Stáhněte si místní počítač tooyour objektů blob**

     1. Vyberte objekt blob hello chcete toodownload.
     2. Na panelu nástrojů hello hlavním podokně vyberte **Stáhnout**.
     3. V hello **zadejte, kam toosave hello stáhli blob** dialogové okno, zadejte hello umístění, kam má hello blob stáhli, a hello název chcete toogive ho.  
     4. Vyberte **Uložit**.
   * **Otevřete objekt blob do místního počítače**

     1. Vyberte objekt blob hello chcete tooopen.
     2. Na panelu nástrojů hello hlavním podokně vyberte **otevřete**.
     3. Objekt blob Hello se stáhne a otevřít pomocí aplikace hello spojené s hello blob základní typ souboru.
   * **Kopírování schránky toohello objektů blob**

     1. Vyberte objekt blob hello chcete toocopy.
     2. Na panelu nástrojů hello hlavním podokně vyberte **kopie**.
     3. V levém podokně hello přejděte tooanother kontejner objektů blob a dvojím kliknutím tooview v hlavním podokně hello.
     4. Na panelu nástrojů hello hlavním podokně vyberte **vložení** toocreate kopii hello objektů blob.
   * **Odstranit objekt blob**

     1. Vyberte objekt blob hello chcete toodelete.
     2. Na panelu nástrojů hello hlavním podokně vyberte **odstranit**.
     3. Vyberte **Ano** toohello dialogové okno potvrzení.

## <a name="next-steps"></a>Další kroky
* Zobrazení hello [nejnovější poznámky k verzi Storage Explorer (Preview) a videa](http://www.storageexplorer.com).
* Zjistěte, jak příliš[vytvářet aplikace, které používají Azure BLOB, tabulek, front a soubory](https://azure.microsoft.com/documentation/services/storage/).

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png
