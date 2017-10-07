---
title: "aaaDownload Linux virtuálního pevného disku z Azure | Microsoft Docs"
description: "Stáhněte si Linux virtuální pevný disk pomocí hello rozhraní příkazového řádku Azure a hello portálu Azure."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: davidmu
ms.openlocfilehash: 7e08e985a64a6be581b8f5eedcce60fbd314eaf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="download-a-linux-vhd-from-azure"></a>Stáhnout Linux virtuální pevný disk z Azure

V tomto článku se dozvíte, jak toodownload [Linux virtuální pevný disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) hello soubor z Azure pomocí rozhraní příkazového řádku Azure a portálu Azure. 

Virtuální počítače (VM) Azure používá [disky](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) jako místní toostore operačního systému, aplikace a data. Všechny virtuální počítače Azure mít aspoň dva disky – disk operačního systému Windows a dočasný disk. disk s operačním systémem Hello je původně vytvořili z bitové kopie a disku operačního systému hello i hello image jsou virtuální pevné disky uložené v účtu úložiště Azure. Virtuální počítače také může mít jeden nebo více datových disků, které jsou také uloženy jako virtuální pevné disky.

Pokud jste tak již neučinili, nainstalujte [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="stop-hello-vm"></a>Zastavit hello virtuálních počítačů

Virtuální pevný disk nelze stáhnout ze služby Azure, pokud je připojen tooa spuštění virtuálního počítače. Je nutné toostop hello virtuálních počítačů toodownload virtuální pevný disk. Pokud chcete toouse virtuální pevný disk jako [image](tutorial-custom-images.md) toocreate ostatních virtuálních počítačů s nové disky, potřebujete toodeprovision a generalize hello operačního systému, která je součástí hello souboru a zastavit hello virtuálních počítačů. toouse hello virtuální pevný disk jako disk pro novou instanci třídy na existující virtuální počítač nebo datový disk, pouze potřebujete toostop a navrátit hello virtuálních počítačů.

toouse hello virtuálního pevného disku jako image toocreate ostatní virtuální počítače, proveďte následující kroky:

1. Použití SSH, název účtu hello a hello veřejné IP adresy tooit tooconnect hello virtuálních počítačů a zrušit jejich zřízení ho. Vítejte + uživatele parametr také odebere poslední účet zřízení uživatele hello. Pokud jsou pečení přihlašovací údaje toohello virtuálních počítačů, nechte si to + parametr uživatele. Hello následující příklad odebere poslední účet zřízení uživatele hello:

    ```bash
    ssh azureuser@40.118.249.235
    sudo waagent -deprovision+user -force
    exit 
    ```

2. Přihlaste se tooyour účet Azure s [az přihlášení](https://docs.microsoft.com/cli/azure/#login).
3. Zastavte a navrátit hello virtuálních počítačů.

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. Generalize hello virtuálních počítačů. 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

toouse hello virtuální pevný disk jako disk pro novou instanci třídy na existující virtuální počítač nebo datový disk, proveďte tyto kroky:

1.  Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2.  V nabídce centra hello, klikněte na tlačítko **virtuální počítače**.
3.  Vyberte ze seznamu hello hello virtuálních počítačů.
4.  V okně hello hello virtuálních počítačů, klikněte na tlačítko **Zastavit**.

    ![Zastavit virtuální počítač](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>Generování adresy URL SAS

soubor VHD hello toodownload, je nutné toogenerate [sdílený přístupový podpis (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) adresy URL. Generování adresy URL hello čas vypršení platnosti je přiřazena toohello adresy URL.

1.  V nabídce hello hello okna pro hello virtuálních počítačů, klikněte na tlačítko **disky**.
2.  Vyberte hello disk operačního systému pro hello virtuálních počítačů a pak klikněte na tlačítko **exportovat**.
3.  Klikněte na tlačítko **generování adresy URL**.

    ![Generování adresy URL](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a>Stáhnout virtuálního pevného disku

1.  V části hello adresu URL, která byla vygenerována klikněte na soubor VHD hello stahování.

    ![Stáhnout virtuálního pevného disku](./media/download-vhd/export-download.png)

2.  Může být nutné tooclick **Uložit** v hello prohlížeče toostart hello stahování. Hello výchozí název souboru virtuálního pevného disku hello je *abcd*.

    ![Kliknutím na Uložit v prohlížeči hello](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak příliš[odesílání a vytvoření virtuálního počítače s Linuxem z vlastní disk s hello Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- [Správa Azure disky hello rozhraní příkazového řádku Azure](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

