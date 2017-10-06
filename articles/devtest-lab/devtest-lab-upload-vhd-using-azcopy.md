---
title: "aaaUpload virtuálního pevného disku soubor pomocí nástroje AzCopy tooAzure DevTest Labs | Microsoft Docs"
description: "Nahrát účtu úložiště toolab souboru virtuálního pevného disku pomocí nástroje AzCopy"
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
ms.openlocfilehash: 14f9e933b0bd27451f6bcb94841ecc381213e578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-azcopy"></a>Nahrát účtu úložiště toolab souboru virtuálního pevného disku pomocí nástroje AzCopy

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

V Azure DevTest Labs může být soubory VHD použité toocreate vlastní Image, které jsou používané tooprovision virtuální počítače. Hello následující kroky vás provedou pomocí tooupload nástroj příkazového řádku AzCopy hello virtuálního pevného disku soubor tooa testovacího prostředí na účet úložiště. Jakmile jste odeslali souboru virtuálního pevného disku, hello [další kroky části](#next-steps) jsou uvedeny některé články, které ilustrují, jak toocreate vlastní image z hello nahrát soubor virtuálního pevného disku. Další informace o disky a virtuální pevné disky v Azure najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](../virtual-machines/linux/about-disks-and-vhds.md)

> [!NOTE] 
>  
> AzCopy je nástroj příkazového řádku pouze pro systém Windows.

## <a name="step-by-step-instructions"></a>Podrobné pokyny

Hello následující kroky procházení vás odesílání virtuálního pevného disku soubor tooAzure DevTest Labs pomocí [AzCopy](http://aka.ms/downloadazcopy). 

1. Získáte hello název účtu úložiště hello prostředí pomocí hello portálu Azure:

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Vyberte **další služby**a potom vyberte **DevTest Labs** hello seznamu.

1. Ze seznamu hello labs vyberte požadované prostředí hello.  

1. V okně prostředí hello vyberte **konfigurace**. 

1. V testovacím hello **konfigurace** vyberte **vlastní Image (VHD)**.

1. Na hello **vlastní image** okně vyberte **+ přidat**. 

1. Na hello **vlastní image** vyberte **virtuálního pevného disku**.

1. Na hello **virtuálního pevného disku** vyberte **nahrát virtuální pevný disk pomocí prostředí PowerShell**.

    ![Nahrání virtuálního pevného disku pomocí prostředí PowerShell](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. Hello **Odeslat bitovou kopii pomocí prostředí PowerShell** zobrazuje volání toohello **přidat AzureVhd** rutiny. první parametr Hello (*cílové*) obsahuje hello identifikátor URI pro kontejner objektů blob (*odešle*) ve formátu hello:

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. Poznamenejte si hello úplný identifikátor URI, jako je tomu v dalších krocích.

1. Nahrajte soubor virtuálního pevného disku hello pomocí AzCopy:
 
1. [Stáhněte a nainstalujte nejnovější verzi AzCopy hello](http://aka.ms/downloadazcopy).

1. Otevřete okno příkazového řádku a přejděte instalační adresář AzCopy toohello. Volitelně můžete přidat hello AzCopy tooyour systému cesta k umístění instalace. AzCopy je ve výchozím nastavení nainstalované toohello následující adresář:

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. Pomocí hello účet klíč a objektů blob kontejneru úložiště identifikátor URI, spusťte následující příkaz na příkazovém řádku hello hello. Hello *vhdFileName* hodnota musí toobe v uvozovkách. Hello proces odesílání soubor virtuálního pevného disku může být náročná v závislosti na velikosti hello hello souboru virtuálního pevného disku a rychlost připojení.   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a>Další kroky

- [Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí hello portálu Azure](devtest-lab-create-template.md)
- [Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí prostředí PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)