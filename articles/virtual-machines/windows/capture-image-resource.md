---
title: "Vytvořte bitovou kopii spravovaných v Azure | Microsoft Docs"
description: "Vytvořte bitovou kopii spravované zobecněný virtuální počítač nebo virtuální pevný disk v Azure. Obrázky lze použít k vytvoření více virtuálních počítačů, které používají spravovaný disky."
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
ms.openlocfilehash: f64b81489ab426b50ec89af369e1581ac71848be
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-managed-image-of-a-generalized-vm-in-azure"></a>Vytvořte bitovou kopii spravované zobecněný virtuálního počítače v Azure

Z zobecněný virtuální počítač, který je uložený jako spravovaných disků nebo diskem nespravované v účtu úložiště můžete vytvořit prostředek spravované bitové kopie. Obrázek pak lze vytvořit víc virtuálních počítačů. 


## <a name="generalize-the-windows-vm-using-sysprep"></a>Generalize virtuální počítač s Windows pomocí nástroje Sysprep

Nástroj Sysprep odstraní všechny vaše osobní informace o účtu, mimo jiné a připraví počítač, který se má použít jako obrázek. Podrobnosti o nástroji Sysprep najdete v tématu [postup použití nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).

Ujistěte se, že role serveru spuštěná na tomto počítači jsou podporovány nástrojem Sysprep. Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Pokud používáte nástroj Sysprep před nahráním svůj disk VHD do Azure poprvé, ujistěte se, máte [připravit virtuální počítač](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před spuštěním nástroje Sysprep. 
> 
> 

1. Přihlaste se k virtuálnímu počítači Windows.
2. Otevřete okno příkazového řádku jako správce. Změňte adresář na **%windir%\system32\sysprep**a poté spusťte `sysprep.exe`.
3. V **nástroj pro přípravu systému** dialogové okno, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)**a ujistěte se, že **generalizace** je zaškrtnuté políčko.
4. V **možnosti vypnutí**, vyberte **vypnutí**.
5. Klikněte na **OK**.
   
    ![Spusťte nástroj Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. Po dokončení nástroj Sysprep vypne virtuální počítač. Virtuální počítač nerestartuje.


## <a name="create-a-managed-image-in-the-portal"></a>Vytvoření bitové kopie spravované na portálu 

1. Otevřete [portál](https://portal.azure.com).
2. Klikněte na symbol plus k vytvoření nového prostředku.
3. Filtr hledání, zadejte **Image**.
4. Vyberte **Image** z výsledků.
5. V **Image** okně klikněte na tlačítko **vytvořit**.
6. V **název**, zadejte název bitové kopie.
7. Pokud máte více než jedno předplatné, vyberte tu správnou z **předplatné** rozevíracího seznamu.
7. V **skupiny prostředků** vyberte buď **vytvořit nový** a zadejte název, nebo vyberte **z existujícího** a vyberte skupinu prostředků z rozevíracího seznamu.
8. V **umístění**, vyberte umístění skupiny prostředků.
9. V **typ operačního systému** vyberte typ operačního systému Windows nebo Linux.
11. V **objektu blob Storage**, klikněte na tlačítko **Procházet** hledání virtuální pevný disk ve službě Azure storage.
12. V **typ účtu** zvolte Standard_LRS nebo Premium_LRS. Jednotky pevného disku používá standard a Premium jednotky SSD. Jak používat místně redundantní úložiště.
13. V **ukládání do mezipaměti disku** vyberte příslušný disk možnost ukládání do mezipaměti. Možnosti jsou **žádné**, **jen pro čtení** a **čtení/zápis**.
14. Volitelné: Také přidáním stávající datový disk do bitové kopie kliknutím **+ přidat datový disk**.  
15. Po dokončení provedení výběru klikněte na tlačítko **vytvořit**.
16. Po vytvoření image se bude zobrazovat jako **Image** prostředku v seznamu prostředků ve skupině prostředků, který jste zvolili.



## <a name="create-a-managed-image-of-a-vm-using-powershell"></a>Vytvoření spravované image virtuálního počítače pomocí prostředí Powershell

Vytvoření bitové kopie přímo z virtuálního počítače zajistí, že image obsahuje všechny disky, které jsou spojené s virtuálním Počítačem, včetně disku operačního systému a všech datových disků.


Než začnete, ujistěte se, že máte nejnovější verzi modulu AzureRM.Compute prostředí PowerShell. Spusťte následující příkaz k její instalaci.

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
2. Zajistěte, aby že bylo zrušeno přidělení virtuálního počítače.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. Nastavte stav virtuálního počítače na **zobecněno**. 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized
    ```
    
4. Získáte virtuální počítač. 

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```

5. Vytvořte konfiguraci bitové kopie.

    ```powershell
    $image = New-AzureRmImageConfig -Location $location -SourceVirtualMachineId $vm.ID 
    ```
6. Vytvoření bitové kopie.

    ```powershell
    New-AzureRmImage -Image $image -ImageName $imageName -ResourceGroupName $rgName
    ``` 



## <a name="create-a-managed-image-of-a-vhd-in-powershell"></a>Vytvoříte spravované bitovou kopii virtuálního pevného disku v prostředí PowerShell

Vytvoření spravované image pomocí vaší zobecněný virtuální pevný disk operačního systému.


1.  Nastavte nejprve, běžné parametry:

    ```powershell
    $rgName = "myResourceGroupName"
    $vmName = "myVM"
    $location = "West Central US" 
    $imageName = "yourImageName"
    $osVhdUri = "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd"
    ```
2. Step\deallocate virtuálního počítače.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. Virtuální počítač označte jako zobecněn.

    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized 
    ```
4.  Vytvořte bitovou kopii pomocí vaší zobecněný virtuální pevný disk operačního systému.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```


## <a name="create-a-managed-image-from-a-snapshot-using-powershell"></a>Vytvoření bitové kopie spravované ze snímku pomocí prostředí Powershell

Můžete také vytvořit bitovou kopii spravované z snímek virtuálního pevného disku z zobecněný virtuální počítač.

    
1. Vytvořte některé proměnné. 

    ```powershell
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. Pořízení snímku.

   ```powershell
   $snapshot = Get-AzureRmSnapshot -ResourceGroupName $rgName -SnapshotName $snapshotName
   ```
   
3. Vytvořte konfiguraci bitové kopie.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -SnapshotId $snapshot.Id
    ```
4. Vytvoření bitové kopie.

    ```powershell
    New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 
    

## <a name="next-steps"></a>Další kroky
- Teď můžete [vytvořte virtuální počítač z bitové kopie zobecněný spravované](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).  

