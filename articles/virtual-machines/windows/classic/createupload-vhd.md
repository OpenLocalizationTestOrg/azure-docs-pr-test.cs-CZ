---
title: "aaaCreate a nahrání virtuálního počítače bitové kopie pomocí prostředí Powershell | Microsoft Docs"
description: "Další informace toocreate a nahrajte zobecněný Windows serverovou bitovou kopii (VHD) pomocí modelu nasazení classic hello a prostředí Azure Powershell."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8c4a08fe-7714-4bf0-be87-c728a7806d3f
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: 093b57c9157cea5f348c8ba02b5700c917adbcdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-a-windows-server-vhd-tooazure"></a>Vytvoření a nahrání virtuálního pevného disku serveru Windows tooAzure
Tento článek ukazuje, jak tooupload zobecněný virtuální počítač bitové kopie jako virtuální pevný disk (VHD), můžete ho toocreate virtuálních počítačů. Další podrobnosti o disky a virtuální pevné disky ve službě Microsoft Azure najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Můžete také [nahrát](../upload-generalized-managed.md) virtuálního počítače pomocí modelu Resource Manager hello.

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že máte:

* **Předplatné Azure** – pokud ji nemáte, můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
* **[Microsoft Azure PowerShell](/powershell/azure/overview)**  -máte hello Microsoft Azure PowerShell modul nainstalovaný a nakonfigurovaný toouse vašeho předplatného.
* **A. Soubor VHD** – podporované Windows operačního systému uložené v souboru VHD a připojené tooa virtuálního počítače. Toosee zkontrolujte, zda role serveru hello systémem hello virtuálního pevného disku jsou podporovány nástrojem Sysprep. Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

    > [!IMPORTANT]
    > Formát VHDX Hello není podporovaný v Microsoft Azure. Můžete převést na formát tooVHD hello disku pomocí Správce technologie Hyper-V nebo hello [rutiny Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx). Podrobnosti najdete v tématu to [blogový příspěvek](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-hello-vhd"></a>Krok 1: Příprava aplikace hello virtuálního pevného disku
Před nahráním tooAzure hello virtuální pevný disk, musí toobe zobecněn pomocí nástroje Sysprep hello. To připraví toobe hello virtuálního pevného disku použít jako obrázek. Podrobnosti o nástroji Sysprep najdete v tématu [jak tooUse nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx). Zálohujte hello virtuálních počítačů před spuštěním nástroje Sysprep.

Z hello virtuální počítač, který hello operačního systému je nainstalovaná, aby dokončení hello následující postup:

1. Přihlaste se toohello operačního systému.
2. Otevřete okno příkazového řádku jako správce. Změnit adresář, hello příliš**%windir%\system32\sysprep**a poté spusťte `sysprep.exe`.

    ![Otevřete okno příkazového řádku](./media/createupload-vhd/sysprep_commandprompt.png)
3. Hello **nástroj pro přípravu systému** zobrazí se dialogové okno.

   ![Spusťte nástroj Sysprep](./media/createupload-vhd/sysprepgeneral.png)
4. V hello **nástroj pro přípravu systému**, vyberte **zadejte systému mimo pole prostředí Spouštěného** a ujistěte se, že **generalizace** je zaškrtnuté.
5. V **možnosti vypnutí**, vyberte **vypnutí**.
6. Klikněte na **OK**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Krok 2: Vytvoření účtu úložiště a kontejner
Účet úložiště v Azure je nutné, abyste získali soubor VHD místní tooupload hello. Tento krok ukazuje, jak toocreate účet nebo get hello informace je třeba z existující účet. Nahraďte hello proměnné v &lsaquo; závorky &rsaquo; nahraďte svými vlastními informacemi.

1. Přihlásit

    ```powershell
    Add-AzureAccount
    ```

2. Nastavte vašeho předplatného Azure.

    ```powershell
    Select-AzureSubscription -SubscriptionName <SubscriptionName>
    ```

3. Vytvořte nový účet úložiště. název účtu úložiště hello Hello by měl být jedinečný, 3 až 24 znaků. Název Hello může být jakoukoli kombinací písmen a číslic. Musíte taky toospecify umístění jako "Východ USA"

    ```powershell
    New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>
    ```

4. Nastavit jako výchozí hello hello nový účet úložiště.

    ```powershell
    Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>
    ```

5. Vytvořte nový kontejner.

    ```powershell
    New-AzureStorageContainer -Name <ContainerName> -Permission Off
    ```

## <a name="step-3-upload-hello-vhd-file"></a>Krok 3: Odeslání souboru VHD hello
Použití hello [přidat AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) tooupload hello virtuálního pevného disku.

Z jste použili v předchozím kroku hello okno Azure PowerShell text hello, typ hello následující příkaz a nahraďte hello proměnné v &lsaquo; závorky &rsaquo; nahraďte svými vlastními informacemi.

```powershell
Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>
```

## <a name="step-4-add-hello-image-tooyour-list-of-custom-images"></a>Krok 4: Přidejte hello image tooyour seznamu vlastních bitových kopií
Použití hello [přidat AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) rutiny tooadd hello image toohello seznam vlastních bitových kopií.

```powershell
Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"
```

## <a name="next-steps"></a>Další kroky
Teď můžete [vytvořit vlastní virtuální](createportal.md) pomocí hello image nahrát.
