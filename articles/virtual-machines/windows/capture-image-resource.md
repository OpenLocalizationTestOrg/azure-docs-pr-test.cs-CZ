---
title: "aaaCreate spravované bitové kopie v Azure | Microsoft Docs"
description: "Vytvořte bitovou kopii spravované zobecněný virtuální počítač nebo virtuální pevný disk v Azure. Bitové kopie může být použité toocreate víc virtuálních počítačů, které používají spravovaný disky."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: cynthn
ms.openlocfilehash: d8cd6c2ce8c5d704de2c845abced85139944d682
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-image-of-a-generalized-vm-in-azure"></a>Vytvořte bitovou kopii spravované zobecněný virtuálního počítače v Azure

Z zobecněný virtuální počítač, který je uložený jako spravovaných disků nebo diskem nespravované v účtu úložiště můžete vytvořit prostředek spravované bitové kopie. Hello image může pak možné použít toocreate víc virtuálních počítačů. 


## <a name="generalize-hello-windows-vm-using-sysprep"></a>Generalize hello virtuální počítač s Windows pomocí nástroje Sysprep

Nástroj Sysprep odstraní všechny vaše osobní informace o účtu, mimo jiné a připraví toobe počítač hello použít jako obrázek. Podrobnosti o nástroji Sysprep najdete v tématu [jak tooUse nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).

Ujistěte se, že role serveru hello spuštěné na počítači hello jsou podporovány nástrojem Sysprep. Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Pokud používáte nástroj Sysprep před nahráním vaší tooAzure virtuálního pevného disku pro hello poprvé, ujistěte se, máte [připravit virtuální počítač](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před spuštěním nástroje Sysprep. 
> 
> 

1. Přihlaste se toohello virtuálního počítače Windows.
2. Otevřete okno příkazového řádku hello jako správce. Změnit adresář, hello příliš**%windir%\system32\sysprep**a poté spusťte `sysprep.exe`.
3. V hello **nástroj pro přípravu systému** dialogové okno, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)**a ujistěte se, že hello **generalizace** je zaškrtnuté políčko.
4. V **možnosti vypnutí**, vyberte **vypnutí**.
5. Klikněte na **OK**.
   
    ![Spusťte nástroj Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. Po dokončení nástroj Sysprep vypne hello virtuálního počítače. Nerestartovat hello virtuálních počítačů.


## <a name="create-a-managed-image-in-hello-portal"></a>Vytvoření bitové kopie spravované hello portálu 

1. Otevřete hello [portál](https://portal.azure.com).
2. Klikněte na tlačítko hello znaménko plus toocreate nový prostředek.
3. Hello filtru hledání, zadejte **Image**.
4. Vyberte **Image** z výsledků hello.
5. V hello **Image** okně klikněte na tlačítko **vytvořit**.
6. V **název**, zadejte název bitové kopie hello.
7. Pokud máte více než jedno předplatné, vyberte hello správné jeden z hello **předplatné** rozevíracího seznamu.
7. V **skupiny prostředků** vyberte buď **vytvořit nový** a zadejte název, nebo vyberte **z existujícího** a vyberte z rozevíracího seznamu hello toouse skupiny prostředků.
8. V **umístění**, vyberte umístění hello vaší skupiny prostředků.
9. V **typ operačního systému** vyberte hello typ operačního systému Windows nebo Linux.
11. V **objektu blob Storage**, klikněte na tlačítko **Procházet** toolook pro hello virtuální pevný disk ve službě Azure storage.
12. V **typ účtu** zvolte Standard_LRS nebo Premium_LRS. Jednotky pevného disku používá standard a Premium jednotky SSD. Jak používat místně redundantní úložiště.
13. V **ukládání do mezipaměti disku** vyberte hello příslušný disk možnost ukládání do mezipaměti. Možnosti Hello jsou **žádné**, **jen pro čtení** a **čtení/zápis**.
14. Volitelné: Můžete také přidat stávající image disku toohello dat kliknutím **+ přidat datový disk**.  
15. Po dokončení provedení výběru klikněte na tlačítko **vytvořit**.
16. Po vytvoření bitové kopie hello se bude zobrazovat jako **Image** prostředku v seznamu hello prostředků ve skupině prostředků hello jste zvolili.



## <a name="create-a-managed-image-of-a-vm-using-powershell"></a>Vytvoření spravované image virtuálního počítače pomocí prostředí Powershell

Vytvoření bitové kopie přímo z virtuálních počítačů zajistí této bitové kopie hello hello zahrnuje všechny disky hello přidružené hello virtuálních počítačů, včetně hello Disk operačního systému a všech datových disků.


Než začnete, ujistěte se, že máte nejnovější verzi hello modulu AzureRM.Compute PowerShell hello. Spustit hello následující příkaz tooinstall.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).


1. Vytvořte některé proměnné.

    ```powershell
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```
2. Ujistěte se, zda text hello, které bylo zrušeno přidělení virtuálního počítače.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. Nastavte stav hello hello virtuálního počítače příliš**zobecněno**. 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized
    ```
    
4. Získáte hello virtuální počítač. 

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```

5. Vytvořte konfiguraci hello bitové kopie.

    ```powershell
    $image = New-AzureRmImageConfig -Location $location -SourceVirtualMachineId $vm.ID 
    ```
6. Vytvoření bitové kopie hello.

    ```powershell
    New-AzureRmImage -Image $image -ImageName $imageName -ResourceGroupName $rgName
    ``` 



## <a name="create-a-managed-image-of-a-vhd-in-powershell"></a>Vytvoříte spravované bitovou kopii virtuálního pevného disku v prostředí PowerShell

Vytvoření spravované image pomocí vaší zobecněný virtuální pevný disk operačního systému.


1.  Nastavte nejprve, hello společné parametry:

    ```powershell
    $rgName = "myResourceGroupName"
    $vmName = "myVM"
    $location = "West Central US" 
    $imageName = "yourImageName"
    $osVhdUri = "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd"
    ```
2. Step\deallocate hello virtuálních počítačů.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. Označte hello virtuálního počítače jako zobecněn.

    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized 
    ```
4.  Vytvoření image hello pomocí vaší zobecněný virtuální pevný disk operačního systému.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```


## <a name="create-a-managed-image-from-a-snapshot-using-powershell"></a>Vytvoření bitové kopie spravované ze snímku pomocí prostředí Powershell

Můžete také vytvořit bitovou kopii spravované z snímek hello virtuálního pevného disku z zobecněný virtuální počítač.

    
1. Vytvořte některé proměnné. 

    ```powershell
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. Získáte hello snímku.

   ```powershell
   $snapshot = Get-AzureRmSnapshot -ResourceGroupName $rgName -SnapshotName $snapshotName
   ```
   
3. Vytvořte konfiguraci hello bitové kopie.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -SnapshotId $snapshot.Id
    ```
4. Vytvoření bitové kopie hello.

    ```powershell
    New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 
    

## <a name="next-steps"></a>Další kroky
- Teď můžete [vytvořte virtuální počítač z bitové kopie spravované hello zobecněn](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).    

