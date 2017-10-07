---
title: "aaaUpload virtuálního pevného disku soubor pomocí Microsoft Azure Storage Explorer tooAzure DevTest Labs | Microsoft Docs"
description: "Nahrání virtuálního pevného disku soubor toolab účtu úložiště pomocí Microsoft Azure Storage Explorer"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 686691e3676cea4b2d7cd8bf045bc43a792c667e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-microsoft-azure-storage-explorer"></a>Nahrání virtuálního pevného disku soubor toolab účtu úložiště pomocí Microsoft Azure Storage Explorer

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

V Azure DevTest Labs může být soubory VHD použité toocreate vlastní Image, které jsou používané tooprovision virtuální počítače. Tento článek ukazuje, jak toouse [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) účet úložiště tooa testovacím souborů tooupload virtuální pevný disk. Jakmile jste odeslali souboru virtuálního pevného disku, hello [další kroky části](#next-steps) jsou uvedeny některé články, které ilustrují, jak toocreate vlastní image z hello nahrát soubor virtuálního pevného disku. Další informace o disky a virtuální pevné disky v Azure najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](../virtual-machines/linux/about-disks-and-vhds.md)

## <a name="step-by-step-instructions"></a>Podrobné pokyny

Hello následující kroky procházení vás odesílání virtuálního pevného disku soubor tooDevTest Labs pomocí [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).

1. [Stáhněte a nainstalujte nejnovější verzi hello Microsoft Azure Storage Explorer hello](http://www.storageexplorer.com).

1. Získáte hello název účtu úložiště hello prostředí pomocí hello portálu Azure:

    1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
    
    1. Vyberte **další služby**a potom vyberte **DevTest Labs** hello seznamu.
    
    1. Ze seznamu hello labs vyberte požadované prostředí hello.  
    
    1. V okně prostředí hello vyberte **konfigurace**. 
    
    1. V testovacím hello **konfigurace** vyberte **vlastní Image (VHD)**.
    
    1. Na hello **vlastní image** okně vyberte **+ přidat**. 
    
    1. Na hello **vlastní image** vyberte **virtuálního pevného disku**.
    
    1. Na hello **virtuálního pevného disku** vyberte **nahrát virtuální pevný disk pomocí prostředí PowerShell**.
    
        ![Nahrání virtuálního pevného disku pomocí prostředí PowerShell][0]
    
    1. Hello **Odeslat bitovou kopii pomocí prostředí PowerShell** zobrazuje volání toohello **přidat AzureVhd** rutiny. první parametr Hello (*cílové*) obsahuje název účtu úložiště hello pro testovací prostředí hello v hello následující formát:
    
        https://<STORAGE-ACCOUNT-name>.BLOB.Core.Windows.NET/uploads/... 

    1. Poznamenejte si název účtu úložiště hello jako se používá v dalších krocích.
    
1. Připojte tooan účtu předplatného Azure pomocí Průzkumníka úložiště.

    > [!TIP] 
    > 
    > Storage Explorer podporuje několik možností připojení. Tato část ukazuje připojující účet úložiště tooa spojené s předplatným Azure. toosee hello další možnosti připojení nepodporuje Storage Explorer, najdete v článku toohello [Začínáme se Storage Explorerem](../vs-azure-tools-storage-manage-with-storage-explorer.md).
 
    1. Otevřete Storage Explorer.
    
    1. V Průzkumníku úložiště vyberte **nastavení účtu Azure**. 
    
        ![Nastavení účtu Azure][1]
    
    1. Hello levý panel zobrazuje účty Microsoft hello, pod kterým jste přihlášení. tooconnect tooanother účtu, vyberte možnost **přidat účet**a postupujte podle pokynů hello dialogová okna toosign pomocí účtu Microsoft, který je přidružen alespoň jeden aktivní předplatné Azure.
    
        ![Přidání účtu][2]
    
    1. Jakmile se úspěšně přihlásíte účtem Microsoft, levém podokně hello naplní s hello předplatná Azure, které jsou přidružené k tomuto účtu. Vyberte hello předplatná Azure, se kterými chcete toowork a potom vyberte **použít**. (Výběr **všechny odběry** přepínačů hello výběr všech nebo žádná z hello uvedených předplatných Azure.)
    
        ![Výběr předplatných Azure][3]
    
    1. Hello levém podokně zobrazí hello účty úložiště přidružené ke hello vybraná předplatná Azure.
    
        ![Vybraná předplatná Azure][4]

1. Vyhledejte účet úložiště hello laboratoře:

    1. V levém podokně Storage Exploreru hello vyhledejte a rozbalte uzel hello pro hello předplatného Azure, které vlastní hello testovacího prostředí.
    
    1. V uzlu hello předplatné, rozbalte položku **účty úložiště**.

    1. Rozbalte uzly protokolů tooreveal uzlu účet úložiště hello laboratoř pro **kontejnery objektů Blob**, **sdílené složky**, **fronty**, a **tabulky**.
    
    1. Rozbalte hello **kontejnery objektů Blob** uzlu.
    
    1. Vyberte toodisplay kontejner objektů blob nahrávání hello jeho obsah v pravém podokně hello.
        
        ![Nahrát adresáře][5]

1. Nahrajte soubor virtuálního pevného disku hello pomocí Průzkumníka úložiště:

    1. V pravém podokně Storage Exploreru hello byste měli vidět v seznamu objektů BLOB hello v hello **odešle** kontejneru objektů blob s testovacím hello účtu úložiště. Na panelu nástrojů editoru objektů blob hello, vyberte **nahrát** 
        
        ![Tlačítko Odeslat][6]
    
    1. Z hello **nahrát** rozevírací nabídky vyberte **nahrání souborů...** .
    
    1. Na hello **nahrání souborů** dialogové okno, vyberte hello třemi tečkami.
        
        ![Vyberte soubor][8]  

    1. Na hello **vyberte soubory tooupload** dialogu procházení toohello požadovaného souboru virtuálního pevného disku, vyberte ho a potom vyberte **otevřete**.
    
    1. Při vrátil toohello **nahrání souborů** dialogové okno, změna **Blob typ** příliš**objekt Blob stránky**.
    
    1. Vyberte **Nahrát**.

        ![Vyberte soubor][9]  
    
    1. Hello Storage Explorer **protokol aktivit** podokně se zobrazují stav stahování hello (spolu s odkazy toocancel hello nahrávání). Hello proces odesílání soubor virtuálního pevného disku může být náročná v závislosti na velikosti hello hello souboru virtuálního pevného disku a rychlost připojení. 

        ![Stav nahrávání souboru][10]  

## <a name="next-steps"></a>Další kroky

- [Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí hello portálu Azure](devtest-lab-create-template.md)
- [Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí prostředí PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)

[0]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-image-using-psh.png
[1]: ./media/devtest-lab-upload-vhd-using-storage-explorer/settings-icon.png
[2]: ./media/devtest-lab-upload-vhd-using-storage-explorer/add-account-link.png
[3]: ./media/devtest-lab-upload-vhd-using-storage-explorer/subscriptions-list.png
[4]: ./media/devtest-lab-upload-vhd-using-storage-explorer/storage-accounts-list.png
[5]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-dir.png
[6]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-button.png
[7]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-files.png
[8]: ./media/devtest-lab-upload-vhd-using-storage-explorer/select-file.png
[9]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-file.png
[10]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-status.png
