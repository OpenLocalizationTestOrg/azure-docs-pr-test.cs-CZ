---
title: "Použití Storage Exploreru (Preview) se službou Azure File Storage | Dokumentace Microsoftu"
description: "Naučíte se, jak pomocí Storage Exploreru (Preview) pracovat se sdílenými složkami a soubory."
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
ms.openlocfilehash: 964691758254531cb92a5b1cbe055ef61d25dba8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="using-storage-explorer-preview-with-azure-file-storage"></a>Použití Storage Exploreru (Preview) se službou Azure File Storage

Azure File Storage je služba, která nabízí sdílené složky v cloudu přes standardní protokol SMB (Server Message Block). Podporují se SMB 2.1 i SMB 3.0. S Azure File Storage můžete rychle a bez nákladných přepisů migrovat starší aplikace, které spoléhají na sdílené složky, do Azure. Službu File Storage můžete používat ke zveřejňování dat pro celý svět, nebo k soukromému ukládání dat aplikací. V tomto článku se dozvíte, jak pomocí Storage Exploreru (Preview) pracovat se sdílenými složkami a soubory.

## <a name="prerequisites"></a>Požadavky

K dokončení kroků v tomto článku budete potřebovat následující:

- [Stažení a instalace Storage Exploreru (Preview)](http://www.storageexplorer.com/)

- [Připojení k účtu úložiště nebo službě Azure](https://docs.microsoft.com//azure/vs-azure-tools-storage-manage-with-storage-explorer#connect-to-a-storage-account-or-service)

## <a name="create-a-file-share"></a>Vytvoření sdílené složky

Všechny soubory se musí nacházet ve sdílené složce, což je jednoduše logické seskupení souborů. Účet může obsahovat neomezený počet sdílených složek a v každé sdílené složce může být uložen neomezený počet souborů.

Následující postup ukazuje vytvoření sdílené složky v Storage Exploreru (Preview).

1. Otevřete Storage Explorer (Preview).

2. V levém podokně rozbalte účet úložiště, v rámci kterého chcete vytvořit sdílenou složku.

3. Klikněte pravým tlačítkem na **Sdílené složky** a v místní nabídce vyberte **Vytvořit sdílenou složku**.

    ![Vytvoření sdílené složky](media/vs-azure-tools-storage-explorer-files/image1.png)

4. Pod složkou **Sdílené složky** se zobrazí textové pole. Zadejte název sdílené složky. V části popisující [pravidla pojmenování sdílených složek](https://docs.microsoft.com//azure/storage/storage-dotnet-how-to-use-blobs#create-a-container) najdete seznam pravidel a omezení pro pojmenování sdílených složek.

    ![Pojmenování sdílené složky](media/vs-azure-tools-storage-explorer-files/image2.png)

5. Jakmile budete hotovi s vytvářením sdílené složky, stiskněte **Enter**, nebo vytváření zrušte stisknutím klávesy **Esc**. Sdílená složka se po úspěšném vytvoření zobrazí v rámci složky **Sdílené složky** pro vybraný účet úložiště.

    ![Nová sdílená složka](media/vs-azure-tools-storage-explorer-files/image3.png)

## <a name="view-a-file-shares-contents"></a>Zobrazení obsahu sdílené složky

Sdílené složky obsahují soubory a složky (ty také můžou obsahovat soubory).

Následující postup ukazuje, jak zobrazit obsah sdílené složky v Storage Exploreru (Preview):

1. Otevřete Storage Explorer (Preview).

2. V levém podokně rozbalte účet úložiště obsahující sdílenou složku, kterou chcete zobrazit.

3. Rozbalte **Sdílené složky** účtu úložiště.

4. Klikněte pravým tlačítkem na sdílenou složku, kterou chcete zobrazit, a v místní nabídce vyberte **Otevřít**. Na sdílenou složku, kterou chcete zobrazit, můžete také dvakrát kliknout.

    ![Otevření sdílené složky](media/vs-azure-tools-storage-explorer-files/image4.png)

5. V hlavním podokně se zobrazí obsah sdílené složky.
    
    ![Obsah sdílené složky](media/vs-azure-tools-storage-explorer-files/image5.png)

## <a name="delete-a-file-share"></a>Odstranění sdílené složky

Sdílené složky můžete podle potřeby snadno vytvářet a odstraňovat. (Chcete-li odstranit jednotlivé soubory, přečtěte si část popisující [správu souborů ve sdílené složce](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)

Následující postup ukazuje odstranění sdílené složky v Storage Exploreru (Preview):

1. Otevřete Storage Explorer (Preview).

2. V levém podokně rozbalte účet úložiště obsahující sdílenou složku, kterou chcete zobrazit.

3. Rozbalte **Sdílené složky** účtu úložiště.

4. Klikněte pravým tlačítkem na sdílenou složku, kterou chcete odstranit, a v místní nabídce vyberte **Odstranit**. Můžete také stisknout klávesu **Delete** a odstranit tak aktuálně vybranou sdílenou složku.

    ![Odstranění](media/vs-azure-tools-storage-explorer-files/image6.png)

5. V potvrzovacím dialogovém okně klikněte na **Ano**.
    
    ![Potvrzovací dialogové okno](media/vs-azure-tools-storage-explorer-files/image7.png)

## <a name="copy-a-file-share"></a>Kopírování sdílené složky

Storage Explorer (Preview) umožňuje zkopírování sdílené složky do schránky a následné vložení této sdílené složky do jiného účtu úložiště. (Chcete-li kopírovat jednotlivé soubory, přečtěte si část popisující [správu souborů ve sdílené složce](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)

Následující postup ukazuje kopírování sdílené složky z jednoho účtu úložiště do jiného.

1. Otevřete Storage Explorer (Preview).

2. V levém podokně rozbalte účet úložiště obsahující sdílenou složku, kterou chcete kopírovat.

3. Rozbalte **Sdílené složky** účtu úložiště.

4. Klikněte pravým tlačítkem na sdílenou složku, kterou chcete kopírovat, a v místní nabídce vyberte **Kopírovat sdílenou složku**.

    ![Kopírování sdílené složky](media/vs-azure-tools-storage-explorer-files/image8.png)

5. Klikněte pravým tlačítkem na požadovaný cílový účet úložiště, do kterého chcete sdílenou složku vložit, a v místní nabídce vyberte **Vložit sdílenou složku**.

    ![Vložení sdílené složky](media/vs-azure-tools-storage-explorer-files/image9.png)

## <a name="get-the-sas-for-a-file-share"></a>Získání sdíleného přístupového podpisu (SAS) pro sdílenou složku

[Sdílený přístupový podpis (SAS)](https://docs.microsoft.com//azure/storage/storage-dotnet-shared-access-signature-part-1) poskytuje delegovaný přístup k prostředkům ve vašem účtu úložiště. To znamená, že můžete klientovi udělit omezená oprávnění k objektům ve vašem účtu úložiště po stanovené časové období a s konkrétní sadou oprávnění, aniž byste museli sdílet přístupové klíče vašeho účtu.

Následující postup ukazuje vytvoření sdíleného přístupového podpisu pro sdílenou složku:

1. Otevřete Storage Explorer (Preview).

2. V levém podokně rozbalte účet úložiště obsahující sdílenou složku, pro kterou chcete získat sdílený přístupový podpis.

3. Rozbalte **Sdílené složky** účtu úložiště.

4. Klikněte pravým tlačítkem na požadovanou sdílenou složku a v místní nabídce vyberte **Získat sdílený přístupový podpis**.

    ![Získání sdíleného přístupového podpisu](media/vs-azure-tools-storage-explorer-files/image10.png)

5. V dialogovém okně **Sdílený přístupový podpis** zadejte zásadu, počáteční datum a datum vypršení platnosti, časové pásmo a požadované úrovně přístupu k prostředku.

    ![Dialogové okno Sdílený přístupový podpis](media/vs-azure-tools-storage-explorer-files/image11.png)

6. Jakmile budete hotovi se zadáváním možností sdíleného přístupového podpisu, vyberte **Vytvořit**.

7. Zobrazí se druhé dialogové okno **Sdílený přístupový podpis** obsahující výpis příslušné sdílené složky společně s adresou URL a řetězci dotazu, které můžete použít k přístupu k prostředku úložiště. Vyberte **Kopírovat** vedle adresy URL, kterou chcete zkopírovat do schránky.
    
    ![Druhé dialogové okno Sdílený přístupový podpis](media/vs-azure-tools-storage-explorer-files/image12.png)

8. Až budete hotovi, vyberte **Zavřít**.

## <a name="manage-access-policies-for-a-file-share"></a>Správa zásad přístupu pro sdílenou složku

Následující postup ukazuje jak spravovat (přidávat a odebírat) zásady přístupu pro sdílenou složku. Zásady přístupu slouží k vytvoření adres URL sdíleného přístupového podpisu, pomocí kterých můžou lidé během definovaného časového období přistupovat k prostředkům služby File Storage.

1. Otevřete Storage Explorer (Preview).

2. V levém podokně rozbalte účet úložiště obsahující sdílenou složku, pro kterou chcete spravovat zásady přístupu.

3. Rozbalte **Sdílené složky** účtu úložiště.

4. Vyberte požadovanou sdílenou složku a v místní nabídce vyberte **Spravovat zásady přístupu**.

    ![Místní nabídka Spravovat zásady přístupu](media/vs-azure-tools-storage-explorer-files/image13.png)

5. V dialogovém okně **Zásady přístupu** se zobrazí výpis již vytvořených zásad přístupu pro vybranou sdílenou složku.
    
    ![Zásady přístupu](media/vs-azure-tools-storage-explorer-files/image14.png)

6. V závislosti na úloze správy zásad přístupu postupujte podle těchto kroků:
    
    - **Přidání nové zásady přístupu:** Vyberte **Přidat**. Po vygenerování se nová zásada přístupu (s výchozím nastavením) zobrazí v dialogovém okně **Zásady přístupu**.

    - **Úprava zásady přístupu:** Proveďte požadované úpravy a vyberte **Uložit**.

    - **Odebrání zásady přístupu:** Vyberte **Odebrat** vedle zásady přístupu, kterou chcete odebrat.

7. Vytvořte novou adresu URL sdíleného přístupového podpisu pomocí zásady přístupu, kterou jste vytvořili dříve:
    
    ![Získání sdíleného přístupového podpisu](media/vs-azure-tools-storage-explorer-files/image15.png)
    
    ![Název a vlastnosti sdíleného přístupového podpisu](media/vs-azure-tools-storage-explorer-files/image16.png)

## <a name="managing-files-in-a-file-share"></a>Správa souborů ve sdílené složce

Po vytvoření sdílené složky do ní můžete nahrát soubor, stáhnout soubor na místní počítač, otevřít soubor na místním počítači a mnoho dalšího.

Následující postup ukazuje, jak spravovat soubory (a složky) v rámci sdílené složky.

1.  Otevřete Storage Explorer (Preview).

2.  V levém podokně rozbalte účet úložiště obsahující sdílenou složku, kterou chcete spravovat.

3.  Rozbalte **Sdílené složky** účtu úložiště.

4.  Dvakrát klikněte na sdílenou složku, kterou chcete zobrazit.

5.  V hlavním podokně se zobrazí obsah sdílené složky.

    ![Obsah sdílené složky](media/vs-azure-tools-storage-explorer-files/image17.png)

6.  V hlavním podokně se zobrazí obsah sdílené složky.

7.  V závislosti na úloze, kterou chcete provést, postupujte podle těchto kroků:

    - **Nahrání souborů do sdílené složky**

        a.  Na panelu nástrojů v hlavním podokně vyberte **Nahrát** a pak v rozevírací nabídce vyberte **Nahrát soubory**.

        ![Nahrání souborů](media/vs-azure-tools-storage-explorer-files/image18.png)
        
        b. V dialogovém okně **Nahrát soubory** vyberte tlačítko se třemi tečkami (**...**) na pravé straně textového pole **Soubory** a vyberte soubory, které chcete nahrát.

        ![Přidání souborů](media/vs-azure-tools-storage-explorer-files/image19.png)

        c. Vyberte **Nahrát**.

    - **Nahrání složky do sdílené složky**
        
        a. Na panelu nástrojů v hlavním podokně vyberte **Nahrát** a pak v rozevírací nabídce vyberte **Nahrát složku**.

        ![Nabídka Nahrát složku](media/vs-azure-tools-storage-explorer-files/image20.png)

        b. V dialogovém okně **Nahrát složku** vyberte tlačítko se třemi tečkami (**...**) na pravé straně textového pole **Složka** a vyberte složku, jejíž obsah chcete nahrát.

        c. Volitelně můžete zadat cílovou složku, do které se obsah vybrané složky nahraje. Pokud cílová složka neexistuje, vytvoří se.

        d. Vyberte **Nahrát**.

    - **Stažení souboru na místní počítač**
        
        a. Vyberte soubor, který chcete stáhnout.
        
        b. Na panelu nástrojů v hlavním podokně vyberte **Stáhnout**.
        
        c. V dialogovém okně **Zadejte, kam se má stahovaný soubor uložit** zadejte umístění, kam chcete soubor stáhnout, a případně nový název souboru.

        d. Vyberte **Uložit**.

    - **Otevření souboru na místním počítači**
        
        a.  Vyberte soubor, který chcete otevřít.
        
        b.  Na panelu nástrojů v hlavním podokně vyberte **Otevřít**.
        
        c.  Soubor se stáhne a otevře pomocí aplikace přidružené k základnímu typu tohoto souboru.

    - **Zkopírování souboru do schránky**

        a. Vyberte soubor, který chcete kopírovat.

        b. Na panelu nástrojů v hlavním podokně vyberte **Kopírovat**.

        c. V levém podokně přejděte do jiné sdílené složky a dvojím kliknutím ji zobrazte v hlavním podokně.

        d. Na panelu nástrojů v hlavním podokně vyberte **Vložit**. Vytvoří se kopie souboru.

    - **Odstranění souboru**

        a. Vyberte soubor, který chcete odstranit.

        b. Na panelu nástrojů v hlavním podokně vyberte **Odstranit**.

        c. V potvrzovacím dialogovém okně klikněte na **Ano**.

## <a name="next-steps"></a>Další kroky

- Podívejte se na [nejnovější poznámky k verzi a videa pro Storage Explorer (Preview)](http://www.storageexplorer.com/).

- Zjistěte, jak [vytvářet aplikace pomocí objektů blob, tabulek, dotazů a souborů Azure](https://azure.microsoft.com/documentation/services/storage/).
