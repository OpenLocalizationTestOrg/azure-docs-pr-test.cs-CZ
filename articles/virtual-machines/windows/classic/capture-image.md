---
title: "aaaCapture bitové kopie virtuálního počítače Windows Azure | Microsoft Docs"
description: "Zaznamenejte bitovou kopii virtuálního počítače Azure Windows vytvořené pomocí modelu nasazení classic hello."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: a5986eac-4cf3-40bd-9b79-7c811806b880
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: b9bbc437012aa44295f90941c9d72e39509df28f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-hello-classic-deployment-model"></a>Zaznamenejte bitovou kopii virtuálního počítače Azure Windows vytvořené pomocí modelu nasazení classic hello.
> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Informace o modelu Resource Manager, najdete v části [zaznamenat bitovou kopii spravované zobecněný virtuálního počítače v Azure](../capture-image-resource.md).

Tento článek ukazuje, jak toocapture virtuální počítač Azure se systémem Windows, můžete ji použít jako image toocreate jiné virtuální počítače. Tento image obsahuje hello disk operačního systému a datové disky, které jsou připojené toohello virtuálního počítače. Neobsahuje síťových konfigurací, takže musíte tooset až konfigurace sítě při vytváření hello jiné virtuální počítače, které použít bitovou kopii hello.

Úložiště Azure hello bitové kopie v rámci **Image virtuálních počítačů (klasické)**, **výpočetní** hello služba, která je uvedena při zobrazení všech služeb Azure. Toto je hello jednom místě, kde jsou uložené všechny Image, které jste odeslali. Podrobnosti o bitových kopiích naleznete v tématu [o bitové kopie u virtuálních počítačů](about-images.md?toc=%2fazure%2fvirtual-machines%2fWindows%2fclassic%2ftoc.json).

## <a name="before-you-begin"></a>Než začnete
Tento postup předpokládá, že jste vytvořili virtuální počítač Azure a nakonfigurovat hello operačního systému, včetně připojení jakýchkoli datových disků. Pokud jste to ještě neudělali, přečtěte si téma hello následující články pro informace o vytváření a příprava hello virtuálního počítače:

* [Vytvořit virtuální počítač z bitové kopie](createportal.md)
* [Jak tooattach datový disk tooa virtuálního počítače](attach-disk.md)
* Ujistěte se, že role serveru hello jsou podporované pomocí nástroje Sysprep. Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [!WARNING]
> Tento proces odstraní hello původní virtuální počítač po jejím zaznamenání.
>
>

Předchozí toocapturing na bitovou kopii virtuálního počítače Azure, se doporučuje zálohovat hello cílového virtuálního počítače. Virtuální počítače Azure můžete zálohovat pomocí služby Azure Backup. Podrobnosti najdete v článku [Zálohování virtuálních počítačů Azure](../../../backup/backup-azure-vms.md). Další řešení jsou k dispozici od certifikovaných partnerů. toofind, co je aktuálně k dispozici, vyhledejte hello Azure Marketplace.

## <a name="capture-hello-virtual-machine"></a>Zachytit virtuální počítač hello
1. V hello [portál Azure](http://portal.azure.com), **připojit** toohello virtuálního počítače. Pokyny najdete v tématu [jak toosign v tooa virtuální počítač se systémem Windows Server][How toosign in tooa virtual machine running Windows Server].
2. Otevřete okno příkazového řádku jako správce.
3. Změnit adresář, hello příliš`%windir%\system32\sysprep`, a poté spusťte sysprep.exe.
4. Hello **nástroj pro přípravu systému** zobrazí se dialogové okno. Hello následující:

   * V **Akce čištění systému**, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)** a ujistěte se, že **generalizace** je zaškrtnuté. Další informace o používání nástroje Sysprep najdete v tématu [jak tooUse nástroje Sysprep: Úvod][How tooUse Sysprep: An Introduction].
   * V **možnosti vypnutí**, vyberte **vypnutí**.
   * Klikněte na **OK**.

   ![Spusťte nástroj Sysprep](./media/capture-image/SysprepGeneral.png)
5. Nástroj Sysprep ukončí hello virtuálního počítače, které změní stav hello hello virtuálního počítače v hello portál Azure příliš**Zastaveno**.
6. V hello portálu Azure, klikněte na **virtuálních počítačů (klasické)** a vyberte hello chcete toocapture virtuálního počítače. Hello **Image virtuálních počítačů (klasické)** skupina je uvedena v části **výpočetní** při prohlížení **další služby**.

7. Na panelu příkazů hello, klikněte na tlačítko **zaznamenat**.

   ![Zachytit virtuální počítač](./media/capture-image/CaptureVM.png)

   Hello **hello zachycení virtuálního počítače** zobrazí se dialogové okno.

8. V **název bitové kopie**, zadejte název pro novou bitovou kopii hello. V **popisek bitové kopie**, zadejte popisek hello novou bitovou kopii.

9. Klikněte na tlačítko **na hello virtuálního počítače jste spustit program Sysprep**. Toto políčko odkazuje toohello akce pomocí nástroje Sysprep v krocích 3 až 5. Obrázek _musí_ zobecněny spuštěním nástroje Sysprep předtím, než přidáte Windows Server image tooyour sadu vlastních bitových kopií.

10. Po dokončení hello zachycení hello novou bitovou kopii k dispozici v hello **Marketplace**, v hello **výpočetní**, **Image virtuálních počítačů (klasické)** kontejneru.

    ![Zachycení Image úspěšné](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Další kroky
Hello bitová kopie je připravená toobe používá toocreate virtuálních počítačů. toodo, vytvoříte virtuální počítač tak, že vyberete hello **další služby** položky nabídky v dolní části hello hello nabídky služeb, pak **Image virtuálních počítačů (klasické)** v hello **výpočetní**skupiny. Pokyny najdete v tématu [vytvořit virtuální počítač z bitové kopie](createportal.md).

[How toosign in tooa virtual machine running Windows Server]:connect-logon.md
[How tooUse Sysprep: An Introduction]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/capture-image/SysprepGeneral.png
[hello virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of hello virtual machine]: ./media/capture-image/CaptureVM.png
[Enter hello image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use hello captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
