---
title: "aaaDownload Windows virtuálního pevného disku z Azure | Microsoft Docs"
description: "Stáhněte si Windows virtuální pevný disk pomocí hello portálu Azure."
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
ms.openlocfilehash: d0ca8842db98f22751f01648c0ba4e5cde090043
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="download-a-windows-vhd-from-azure"></a>Stáhněte si Windows virtuálního pevného disku z Azure

V tomto článku se dozvíte, jak toodownload [Windows virtuální pevný disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hello soubor z Azure pomocí portálu Azure. 

Virtuální počítače (VM) Azure používá [disky](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) jako místní toostore operačního systému, aplikace a data. Všechny virtuální počítače Azure mít aspoň dva disky – disk operačního systému Windows a dočasný disk. disk s operačním systémem Hello je původně vytvořili z bitové kopie a disku operačního systému hello i hello image jsou virtuální pevné disky uložené v účtu úložiště Azure. Virtuální počítače také může mít jeden nebo více datových disků, které jsou také uloženy jako virtuální pevné disky.

## <a name="stop-hello-vm"></a>Zastavit hello virtuálních počítačů

Virtuální pevný disk nelze stáhnout ze služby Azure, pokud je připojen tooa spuštění virtuálního počítače. Je nutné toostop hello virtuálních počítačů toodownload virtuální pevný disk. Pokud chcete toouse virtuální pevný disk jako [image](tutorial-custom-images.md) toocreate ostatních virtuálních počítačů s nové disky použijete [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) toogeneralize hello obsažené v souboru hello operačního systému a poté jej zastavit hello virtuálních počítačů. toouse hello virtuální pevný disk jako disk pro novou instanci třídy na existující virtuální počítač nebo datový disk, pouze potřebujete toostop a navrátit hello virtuálních počítačů.

toouse hello virtuálního pevného disku jako image toocreate ostatní virtuální počítače, proveďte následující kroky:

1.  Pokud jste tak již neučinili, přihlaste se toohello [portál Azure](https://portal.azure.com/).
2.  [Připojit virtuální počítač toohello](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
3.  Na hello virtuálních počítačů otevřete okno příkazového řádku hello jako správce.
4.  Změnit adresář, hello příliš*%windir%\system32\sysprep* a spusťte sysprep.exe.
5.  V dialogovém okně Nástroj pro přípravu systému hello, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)**a ujistěte se, že **generalizace** je vybrána.
6.  V možnosti vypnutí, vyberte **vypnutí**a potom klikněte na **OK**. 

toouse hello virtuální pevný disk jako disk pro novou instanci třídy na existující virtuální počítač nebo datový disk, proveďte tyto kroky:

1.  V nabídce centra hello v hello portálu Azure, klikněte na tlačítko **virtuální počítače**.
2.  Vyberte ze seznamu hello hello virtuálních počítačů.
3.  V okně hello hello virtuálních počítačů, klikněte na tlačítko **Zastavit**.

    ![Zastavit virtuální počítač](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>Generování adresy URL SAS

soubor VHD hello toodownload, je nutné toogenerate [sdílený přístupový podpis (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) adresy URL. Generování adresy URL hello čas vypršení platnosti je přiřazena toohello adresy URL.

1.  V nabídce hello hello okna pro hello virtuálních počítačů, klikněte na tlačítko **disky**.
2.  Vyberte hello disk operačního systému pro hello virtuálních počítačů a pak klikněte na tlačítko **exportovat**.
3.  Nastavit čas vypršení platnosti hello adresy URL hello příliš*36000*.
4.  Klikněte na tlačítko **generování adresy URL**.

    ![Generování adresy URL](./media/download-vhd/export-generate.png)

> [!NOTE]
> čas vypršení platnosti Hello vzroste z hello výchozí tooprovide dostatek času toodownload hello velkých souborů virtuálního pevného disku pro operační systém Windows Server. Můžete očekávat, že soubor virtuálního pevného disku, který obsahuje tootake operačního systému Windows Server hello toodownload několik hodin v závislosti na připojení. Pokud stahujete virtuální pevný disk pro datový disk, hello výchozí doba je dostačující. 
> 
> 

## <a name="download-vhd"></a>Stáhnout virtuálního pevného disku

1.  V části hello adresu URL, která byla vygenerována klikněte na soubor VHD hello stahování.

    ![Stáhnout virtuálního pevného disku](./media/download-vhd/export-download.png)

2.  Může být nutné tooclick **Uložit** v hello prohlížeče toostart hello stahování. Hello výchozí název souboru virtuálního pevného disku hello je *abcd*.

    ![Kliknutím na Uložit v prohlížeči hello](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak příliš[nahrát tooAzure souboru virtuálního pevného disku](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
- [Vytvoření spravované disky z nespravovaných disků v účtu úložiště](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- [Správa Azure disky pomocí prostředí PowerShell](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

