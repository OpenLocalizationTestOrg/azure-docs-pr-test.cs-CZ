---
title: aaaUsing Storage Explorer (Preview) s Azure File storage | Microsoft Docs
description: "Zjistěte, jak zjistěte, jak toouse toowork Storage Explorer (Preview) se souborem sdílené složky a soubory."
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/09/2017
ms.author: cawa
ms.openlocfilehash: 98eb3cde711ae3dbfdb6ffaec23ae24f822370e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-storage-explorer-preview-with-azure-file-storage"></a>Použití Storage Exploreru (Preview) se službou Azure File Storage

Azure File storage je služba, která nabízí sdílené složky v cloudu pomocí hello hello standardní protokol Server Message Block (SMB). Podporují se SMB 2.1 i SMB 3.0. S Azure File storage můžete migrovat starší aplikace, které spoléhají na sdílené složky tooAzure souboru rychle a bez nákladných přepisů. Můžete použít soubor úložiště tooexpose data veřejně toohello svět nebo data aplikací toostore soukromě. V tomto článku se dozvíte, jak toouse toowork Storage Explorer (Preview) se souborem sdílené složky a soubory.

## <a name="prerequisites"></a>Požadavky

toocomplete hello kroky v tomto článku, budete potřebovat následující hello:

- [Stažení a instalace Storage Exploreru (Preview)](http://www.storageexplorer.com/)

- [Připojení účtu úložiště Azure tooa nebo služby](https://docs.microsoft.com//azure/vs-azure-tools-storage-manage-with-storage-explorer#connect-to-a-storage-account-or-service)

## <a name="create-a-file-share"></a>Vytvoření sdílené složky

Všechny soubory se musí nacházet ve sdílené složce, což je jednoduše logické seskupení souborů. Účet může obsahovat neomezený počet sdílených složek a v každé sdílené složce může být uložen neomezený počet souborů.

Hello následující kroky ukazují, jak toocreate soubor sdílet v rámci Storage Explorer (Preview).

1. Otevřete Storage Explorer (Preview).

2. V levém podokně hello rozbalte účet úložiště hello v rámci kterého chcete toocreate hello sdílené složky

3. Klikněte pravým tlačítkem na **sdílené složky**a - hello kontextové nabídce vyberte **vytvoření sdílené složky**.

    ![Vytvoření sdílené složky](media/vs-azure-tools-storage-explorer-files/image1.png)

4. V textovém poli se zobrazí pod hello **sdílené složky** složky. Zadejte název sdílené složky hello. V tématu hello [sdílet pravidla pojmenování](https://docs.microsoft.com//azure/storage/storage-dotnet-how-to-use-blobs#create-a-container) části Seznam pravidel a omezení pro pojmenovávání sdílených složek.

    ![Pojmenování hello sdílené složky](media/vs-azure-tools-storage-explorer-files/image2.png)

5. Stiskněte klávesu **Enter** při done toocreate hello sdílené složky, nebo **Esc** toocancel. Po úspěšném vytvoření hello sdílené složky se zobrazí v části hello **sdílené složky** složku pro hello vybraný účet úložiště.

    ![Nová sdílená složka Hello](media/vs-azure-tools-storage-explorer-files/image3.png)

## <a name="view-a-file-shares-contents"></a>Zobrazení obsahu sdílené složky

Sdílené složky obsahují soubory a složky (ty také můžou obsahovat soubory).

Hello následující kroky popisují jak sdílet obsah hello tooview souboru v rámci Storage Explorer (Preview): +

1. Otevřete Storage Explorer (Preview).

2. V levém podokně hello rozbalte účet úložiště hello obsahující chcete tooview hello sdílené složky.

3. Rozbalte účet úložiště hello **sdílené složky**.

4. Klikněte pravým tlačítkem na hello sdílené složky můžete chcete tooview a - hello kontextové nabídce vyberte **otevřete**. Dvakrát klikněte na sdílenou složku hello chcete tooview.

    ![Otevření sdílené složky](media/vs-azure-tools-storage-explorer-files/image4.png)

5. Hello hlavním podokně se zobrazí obsah hello sdílené složky.
    
    ![Hello obsah sdílené složky](media/vs-azure-tools-storage-explorer-files/image5.png)

## <a name="delete-a-file-share"></a>Odstranění sdílené složky

Sdílené složky můžete podle potřeby snadno vytvářet a odstraňovat. (toosee jak toodelete jednotlivé soubory, naleznete v části toohello [správu souborů ve sdílené složce](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)

Hello následující kroky popisují, jak toodelete soubor sdílet v rámci Storage Explorer (Preview):

1. Otevřete Storage Explorer (Preview).

2. V levém podokně hello rozbalte účet úložiště hello obsahující chcete tooview hello sdílené složky.

3. Rozbalte účet úložiště hello **sdílené složky**.

4. Klikněte pravým tlačítkem na hello sdílené složky můžete chcete toodelete a - hello kontextové nabídce vyberte **odstranit**. Můžete také stisknout **odstranit** toodelete hello aktuálně vybrané sdílené složky.

    ![Odstranění](media/vs-azure-tools-storage-explorer-files/image6.png)

5. Vyberte **Ano** toohello dialogové okno potvrzení.
    
    ![Potvrzovací dialogové okno](media/vs-azure-tools-storage-explorer-files/image7.png)

## <a name="copy-a-file-share"></a>Kopírování sdílené složky

Storage Explorer (Preview) vám umožní toocopy schránky toohello sdílenou složku soubor a vložte tuto sdílenou složku souborů do jiný účet úložiště. (toosee jak toocopy jednotlivé soubory, naleznete v části toohello [správu souborů ve sdílené složce](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)

Hello následující kroky ukazují, jak sdílet toocopy soubor z jednoho tooanother účet úložiště.

1. Otevřete Storage Explorer (Preview).

2. V levém podokně hello rozbalte účet úložiště hello obsahující chcete toocopy hello sdílené složky.

3. Rozbalte účet úložiště hello **sdílené složky**.

4. Klikněte pravým tlačítkem na hello sdílené složky můžete chcete toocopy a - hello kontextové nabídce vyberte **sdílená složka kopie**.

    ![Kopírování sdílené složky](media/vs-azure-tools-storage-explorer-files/image8.png)

5. Klikněte pravým tlačítkem na požadovaný hello "target" účet úložiště, do kterého chcete toopaste hello sdílené složky a - hello kontextové nabídce vyberte **sdílená složka vložení**.

    ![Vložení sdílené složky](media/vs-azure-tools-storage-explorer-files/image9.png)

## <a name="get-hello-sas-for-a-file-share"></a>Získat hello SAS pro sdílené složky

A [sdílený přístupový podpis (SAS)](https://docs.microsoft.com//azure/storage/storage-dotnet-shared-access-signature-part-1) poskytuje Delegovaný přístup tooresources ve vašem účtu úložiště. To znamená, že můžete udělit, že klient omezené oprávnění tooobjects ve vašem účtu úložiště v zadaném časovém intervalu a zadanou sadu oprávnění, bez nutnosti tooshare klíče pro přístup k účtu.

Hello následující kroky popisují jak toocreate SAS pro soubor sdílet: +

1. Otevřete Storage Explorer (Preview).

2. V levém podokně hello rozbalte účet úložiště hello obsahující hello sdílené složky, pro kterou chcete tooget SAS.

3. Rozbalte účet úložiště hello **sdílené složky**.

4. Klikněte pravým tlačítkem na hello požadované sdílené složky a z kontextové nabídky hello - vyberte **sdíleného přístupového podpisu**.

    ![Získání sdíleného přístupového podpisu](media/vs-azure-tools-storage-explorer-files/image10.png)

5. V hello **sdíleného přístupového podpisu** dialogové okno, zadejte hello zásad, spuštění a datum vypršení platnosti, časové pásmo a chcete pro prostředek hello úrovně přístupu.

    ![Dialogové okno Sdílený přístupový podpis](media/vs-azure-tools-storage-explorer-files/image11.png)

6. Jakmile budete hotovi, zadání možností hello SAS, vyberte **vytvořit**.

7. Druhý **sdíleného přístupového podpisu** poté zobrazí dialogové okno, seznamy hello sdílené složky společně s hello adresy URL a QueryStrings můžete použít tooaccess hello prostředků úložiště. Vyberte **kopie** další URL toohello chcete toocopy toohello schránky.
    
    ![Druhé dialogové okno Sdílený přístupový podpis](media/vs-azure-tools-storage-explorer-files/image12.png)

8. Až budete hotovi, vyberte **Zavřít**.

## <a name="manage-access-policies-for-a-file-share"></a>Správa zásad přístupu pro sdílenou složku

Hello následující kroky popisují jak toomanage (přidání a odebrání) získat přístup k zásadám pro sdílené složky: +. Zásady přístupu Hello se používá pro vytvoření adresy URL SAS, pomocí kterého se uživatelé mohou používat tooaccess hello soubor úložiště prostředků v definované období.

1. Otevřete Storage Explorer (Preview).

2. V levém podokně hello rozbalte účet úložiště hello obsahující hello sdílené složky jejichž zásady přístupu, které chcete toomanage.

3. Rozbalte účet úložiště hello **sdílené složky**.

4. Vyberte hello požadované sdílené složky a - z kontextové nabídky hello - vyberte **spravovat zásady přístupu**.

    ![Místní nabídka Spravovat zásady přístupu](media/vs-azure-tools-storage-explorer-files/image13.png)

5. Hello **zásady přístupu** dialogové okno se zobrazí seznam všechny zásady přístupu, které jsou vytvořeny již pro hello vybrané sdílené složky.
    
    ![Zásady přístupu](media/vs-azure-tools-storage-explorer-files/image14.png)

6. Pomocí těchto kroků v závislosti na úlohu správy zásad přístupu hello:
    
    - **Přidání nové zásady přístupu:** Vyberte **Přidat**. Po vygenerování hello **zásady přístupu** dialogové okno se zobrazí hello nově přidaná přístup zásad (s výchozí nastavení).

    - **Úprava zásady přístupu:** Proveďte požadované úpravy a vyberte **Uložit**.

    - **Odebrat zásady přístupu** – vyberte **odebrat** další zásady přístupu toohello chcete tooremove.

7. Vytvořte novou adresu SAS URL pomocí hello zásady přístupu, které jste vytvořili dříve:
    
    ![Získání sdíleného přístupového podpisu](media/vs-azure-tools-storage-explorer-files/image15.png)
    
    ![Název a vlastnosti sdíleného přístupového podpisu](media/vs-azure-tools-storage-explorer-files/image16.png)

## <a name="managing-files-in-a-file-share"></a>Správa souborů ve sdílené složce

Po vytvoření sdílené složky, můžete nahrát soubor toothat sdílené složky, stáhněte soubor tooyour místní počítač, otevřete soubor na místním počítači a mnoho dalšího.

Hello následující kroky znázorňují, jak sdílet toomanage hello soubory (a složky) v souboru.

1.  Otevřete Storage Explorer (Preview).

2.  V levém podokně hello rozbalte účet úložiště hello obsahující chcete toomanage hello sdílené složky.

3.  Rozbalte účet úložiště hello **sdílené složky**.

4.  Dvakrát klikněte na sdílenou složku hello chcete tooview.

5.  Hello hlavním podokně se zobrazí obsah hello sdílené složky.

    ![Hello obsah sdílené složky](media/vs-azure-tools-storage-explorer-files/image17.png)

6.  Hello hlavním podokně se zobrazí obsah hello sdílené složky.

7.  Postupujte podle těchto kroků v závislosti na úkolu hello chcete tooperform:

    - **Nahrání souborů tooa sdílené složky**

        a.  Na panelu nástrojů hello hlavním podokně vyberte **nahrát**a potom **nahrání souborů** z rozevírací nabídky hello.

        ![Nahrání souborů](media/vs-azure-tools-storage-explorer-files/image18.png)
        
        b. V hello **nahrání souborů** dialogové okno, vyberte hello třemi tečkami (**...** ) na pravé straně hello hello tlačítko **soubory** textového pole tooselect hello soubory chcete tooupload.

        ![Přidání souborů](media/vs-azure-tools-storage-explorer-files/image19.png)

        c. Vyberte **Nahrát**.

    - **Nahrát tooa sdílené složky**
        
        a. Na panelu nástrojů hello hlavním podokně vyberte **nahrát**a potom **nahrát složky** z rozevírací nabídky hello.

        ![Nabídka Nahrát složku](media/vs-azure-tools-storage-explorer-files/image20.png)

        b. V hello **nahrávání složky** dialogové okno, vyberte hello třemi tečkami (**...** ) na pravé straně hello hello tlačítko **složky** textové pole tooselect hello složku, jejíž obsah, chcete tooupload.

        c. Volitelně zadejte cílovou složku, do které hello se nahraje obsah vybrané složky. Pokud hello cílové složce neexistuje, bude vytvořen.

        d. Vyberte **Nahrát**.

    - **Stáhněte si soubor tooyour místní počítač**
        
        a. Vyberte soubor hello chcete toodownload.
        
        b. Na panelu nástrojů hello hlavním podokně vyberte **Stáhnout**.
        
        c. V hello **zadejte, kde toosave hello stáhli soubor** dialogové okno, zadejte hello umístění, kam chcete soubor hello stažený a hello název chcete toogive ho.

        d. Vyberte **Uložit**.

    - **Otevření souboru na místním počítači**
        
        a.  Vyberte soubor hello chcete tooopen.
        
        b.  Na panelu nástrojů hello hlavním podokně vyberte **otevřete**.
        
        c.  Hello soubor se stáhne a otevřít pomocí aplikace hello spojené s hello základní typ souboru.

    - **Zkopírujte soubor toohello schránky**

        a. Vyberte soubor hello chcete toocopy.

        b. Na panelu nástrojů hello hlavním podokně vyberte **kopie**.

        c. V levém podokně hello přejděte tooanother sdílené složky a klikněte dvakrát na jeho tooview v hlavním podokně hello.

        d. Na panelu nástrojů hello hlavním podokně vyberte **vložení** toocreate kopii souboru hello.

    - **Odstranění souboru**

        a. Vyberte soubor hello chcete toodelete.

        b. Na panelu nástrojů hello hlavním podokně vyberte **odstranit**.

        c. Vyberte **Ano** toohello dialogové okno potvrzení.

## <a name="next-steps"></a>Další kroky

- Zobrazení hello [nejnovější poznámky k verzi Storage Explorer (Preview) a videa](http://www.storageexplorer.com/).

- Zjistěte, jak příliš[vytvářet aplikace, které používají Azure BLOB, tabulek, front a soubory](https://azure.microsoft.com/documentation/services/storage/).
