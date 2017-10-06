---
title: "aaaCreating obrázek místní virtuální počítač pro hello Azure Marketplace | Microsoft Docs"
description: "Pochopit a provedení kroků toocreate hello bitovou kopii virtuálního počítače místní a nasadit toohello Azure Marketplace pro ostatní toopurchase."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 26dfbd5a-8685-4b19-987e-c20ca60540ec
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: c7a265330f1e494db8d0e981a38ee00d85746bb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-an-on-premises-virtual-machine-image-for-hello-azure-marketplace"></a>Vývoj bitové kopie virtuálního počítače místní pro hello Azure Marketplace
Důrazně doporučujeme vývoj Azure virtuální pevné disky (VHD) přímo v cloudu hello pomocí protokolu RDP. Pokud je to nutné, ale je možné toodownload virtuální pevný disk a vývoj pomocí místní infrastruktury.  

Pro místní vývoj, je nutné stáhnout hello operačního systému hello vytvořit virtuální pevný disk virtuálního počítače. Tyto kroky by proběhnout jako součást kroku 3.3, výše.  

## <a name="download-a-vhd-image"></a>Stažení bitové kopie virtuálního pevného disku
### <a name="locate-a-blob-url"></a>Vyhledejte adresu URL objektu blob
V pořadí toodownload hello virtuálního pevného disku nejprve vyhledejte adresy URL objektu blob hello hello disk operačního systému.

Vyhledejte nové adresy URL objektu blob hello z hello [portálu Microsoft Azure](https://portal.azure.com):

1. Přejděte příliš**Procházet** > **virtuální počítače**, a pak vyberte hello nasazení virtuálních počítačů.
2. V části **konfigurace**, vyberte hello **disky** dlaždici, což otevře okno disky hello.
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)
3. Vyberte hello **Disk s operačním systémem**, což otevře další okno, které zobrazuje vlastnosti disku, včetně umístění virtuálního pevného disku hello.
4. Zkopírujte tuto adresu URL objektu blob.
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)
5. Nyní, odstranit hello nasazení virtuálních počítačů bez odstranění hello základní disky. Neodstraňovat, ale můžete také zastavit hello virtuálních počítačů. Nestahovat hello operačního systému virtuálního pevného disku, když běží hello virtuálních počítačů.
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### <a name="download-a-vhd"></a>Stažení virtuálního pevného disku
Po zjištění adresy URL objektu blob hello si můžete stáhnout hello virtuální pevný disk pomocí hello [portál Azure](http://manage.windowsazure.com/) nebo prostředí PowerShell.  

> [!NOTE]
> V době hello Tato příručka vytvoření hello funkce toodownload virtuální pevný disk ještě není součástí hello nového portálu Microsoft Azure.  
> 
> 

**Stáhnout hello operačního systému virtuálního pevného disku prostřednictvím hello aktuální [portálu Azure](http://manage.windowsazure.com/)**

1. Pokud jste tak již neučinili, přihlaste toohello portálu Azure.
2. Klikněte na tlačítko hello **úložiště** kartě.
3. Vyberte účet úložiště hello v rámci které hello je uložený virtuální pevný disk.
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)
4. Zobrazí vlastnosti účtu úložiště. Vyberte hello **kontejnery** kartě.
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)
5. Vyberte hello kontejner, ve které hello se k uložení virtuálního pevného disku. Ve výchozím nastavení při vytváření z portálu hello hello virtuální pevný disk uložený v kontejner virtuálních pevných disků.
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)
6. Vyberte hello správný operační systém virtuálního pevného disku tak, že porovnáte toohello hello adresu URL, jeden, který jste uložili.
7. Klikněte na **Stáhnout**.
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### <a name="download-a-vhd-by-using-powershell"></a>Stáhnout virtuální pevný disk pomocí prostředí PowerShell
Kromě toho toousing hello portálu Azure, můžete použít hello [uložit AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) rutiny toodownload hello operačního systému virtuálního pevného disku.

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
Například uložit AzureVhd-zdroje "https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd" - LocalFilePath "C:\Users\Administrator\Desktop\baseimagevm.vhd" - klíč úložiště<String>

> [!NOTE]
> **Uložit AzureVhd** má také **NumberOfThreads** možnost, která může být použit tooincrease paralelismus toomake hello nejlepší využití šířky pásma k dispozici ke stažení hello.
> 
> 

## <a name="upload-vhds-tooan-azure-storage-account"></a>Nahrát účtu úložiště Azure tooan virtuálních pevných disků
Pokud jste připravili virtuální pevné disky místní, musíte tooupload je do úložiště účtu v Azure. Tento krok se provádí po vytvoření svůj disk VHD místní ale před jeho získání certifikační pro bitové kopie virtuálního počítače.

### <a name="create-a-storage-account-and-container"></a>Vytvoření účtu úložiště a kontejneru
Doporučujeme vám, že virtuální pevné disky se nahraje do účtu úložiště v oblasti ve Spojených státech amerických hello. Všechny virtuální pevné disky pro jednu SKU musí být umístěny v jednom kontejneru v rámci účtu jedno úložiště.

toocreate účet úložiště, můžete použít hello [portálu Microsoft Azure](https://portal.azure.com/), prostředí PowerShell nebo nástroj příkazového řádku Linux hello.  

**Vytvořit účet úložiště z portálu Microsoft Azure hello**

1. Klikněte na možnost **Nové**.
2. Vyberte **úložiště**.
3. Zadejte název účtu úložiště hello a pak vyberte umístění.
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)
4. Klikněte na možnost **Vytvořit**.
5. by se mělo otevřít okno Hello hello vytvořit účet úložiště. Pokud ne, vyberte **Procházet** > **účty úložiště**. Na hello úložiště účet okně, vyberte účet úložiště hello vytvořili.
6. Vyberte **kontejnery**.
   
   ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 
7. V okně hello kontejnery, vyberte **přidat**a potom zadejte oprávnění pro kontejner název a hello kontejneru. Vyberte **privátní** kontejneru oprávnění.

> [!TIP]
> Doporučujeme, abyste vytvořili jednoho kontejneru typu jedné plánování toopublish SKU.
> 
> 

  ![Kreslení](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### <a name="create-a-storage-account-by-using-powershell"></a>Vytvořit účet úložiště pomocí prostředí PowerShell
Pomocí prostředí PowerShell vytvořit účet úložiště pomocí hello [New-AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) rutiny.

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

Potom můžete vytvořit kontejner v rámci tohoto účtu úložiště pomocí hello [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) rutiny.

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [!NOTE]
> Těchto příkazů se předpokládá, že tento kontext účtu úložiště aktuální hello již byla nastavena v prostředí PowerShell.   Odkazovat příliš[nastavení prostředí Azure PowerShell](marketplace-publishing-powershell-setup.md) Další informace o instalaci prostředí PowerShell.  
> 
> ### <a name="create-a-storage-account-by-using-hello-command-line-tool-for-mac-and-linux"></a>Vytvořit účet úložiště pomocí nástroje příkazového řádku hello pro Mac a Linux
> Z [nástroj příkazového řádku Linux](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), vytvoření účtu úložiště následujícím způsobem.
> 
> 

        azure storage account create mystorageaccount --location "West US"

Vytvořte kontejner následujícím způsobem.

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## <a name="upload-a-vhd"></a>Nahrání virtuálního pevného disku
Po vytvoření účtu úložiště hello a kontejneru, můžete nahrát připravit virtuální pevné disky. Můžete použít PowerShell, nástroje příkazového řádku hello Linux nebo jiné nástroje pro správu Azure Storage.

### <a name="upload-a-vhd-via-powershell"></a>Nahrání virtuálního pevného disku pomocí prostředí PowerShell
Použití hello [přidat AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) rutiny.

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### <a name="upload-a-vhd-by-using-hello-command-line-tool-for-mac-and-linux"></a>Nahrát VHD pomocí nástroje příkazového řádku hello pro Mac a Linux
S hello [nástroj příkazového řádku Linux](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), použijte hello: vytvoření bitové kopie virtuálního počítače azure <image name> – umístění <Location of hello data center> – operačního systému Linux<LocationOfLocalVHD>

## <a name="see-also"></a>Viz také
* [Vytváření bitové kopie virtuálního počítače pro hello Marketplace.](marketplace-publishing-vm-image-creation.md)
* [Nastavení prostředí Azure PowerShell](marketplace-publishing-powershell-setup.md)

