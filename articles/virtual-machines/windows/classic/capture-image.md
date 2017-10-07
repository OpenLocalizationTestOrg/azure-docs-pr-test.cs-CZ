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
# <a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-hello-classic-deployment-model"></a><span data-ttu-id="b3114-103">Zaznamenejte bitovou kopii virtuálního počítače Azure Windows vytvořené pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="b3114-103">Capture an image of an Azure Windows virtual machine created with hello classic deployment model.</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b3114-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b3114-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="b3114-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="b3114-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="b3114-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="b3114-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="b3114-107">Informace o modelu Resource Manager, najdete v části [zaznamenat bitovou kopii spravované zobecněný virtuálního počítače v Azure](../capture-image-resource.md).</span><span class="sxs-lookup"><span data-stu-id="b3114-107">For Resource Manager model information, see [Capture a managed image of a generalized VM in Azure](../capture-image-resource.md).</span></span>

<span data-ttu-id="b3114-108">Tento článek ukazuje, jak toocapture virtuální počítač Azure se systémem Windows, můžete ji použít jako image toocreate jiné virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="b3114-108">This article shows you how toocapture an Azure virtual machine running Windows so you can use it as an image toocreate other virtual machines.</span></span> <span data-ttu-id="b3114-109">Tento image obsahuje hello disk operačního systému a datové disky, které jsou připojené toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b3114-109">This image includes hello operating system disk and any data disks that are attached toohello virtual machine.</span></span> <span data-ttu-id="b3114-110">Neobsahuje síťových konfigurací, takže musíte tooset až konfigurace sítě při vytváření hello jiné virtuální počítače, které použít bitovou kopii hello.</span><span class="sxs-lookup"><span data-stu-id="b3114-110">It doesn't include networking configurations, so you'll need tooset up network configurations when you create hello other virtual machines that use hello image.</span></span>

<span data-ttu-id="b3114-111">Úložiště Azure hello bitové kopie v rámci **Image virtuálních počítačů (klasické)**, **výpočetní** hello služba, která je uvedena při zobrazení všech služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="b3114-111">Azure stores hello image under **VM images (classic)**, a **Compute** service that is listed when you view all hello Azure services.</span></span> <span data-ttu-id="b3114-112">Toto je hello jednom místě, kde jsou uložené všechny Image, které jste odeslali.</span><span class="sxs-lookup"><span data-stu-id="b3114-112">This is hello same place where any images you've uploaded are stored.</span></span> <span data-ttu-id="b3114-113">Podrobnosti o bitových kopiích naleznete v tématu [o bitové kopie u virtuálních počítačů](about-images.md?toc=%2fazure%2fvirtual-machines%2fWindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b3114-113">For details about images, see [About images for virtual machines](about-images.md?toc=%2fazure%2fvirtual-machines%2fWindows%2fclassic%2ftoc.json).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b3114-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="b3114-114">Before you begin</span></span>
<span data-ttu-id="b3114-115">Tento postup předpokládá, že jste vytvořili virtuální počítač Azure a nakonfigurovat hello operačního systému, včetně připojení jakýchkoli datových disků.</span><span class="sxs-lookup"><span data-stu-id="b3114-115">These steps assume that you've already created an Azure virtual machine and configured hello operating system, including attaching any data disks.</span></span> <span data-ttu-id="b3114-116">Pokud jste to ještě neudělali, přečtěte si téma hello následující články pro informace o vytváření a příprava hello virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="b3114-116">If you haven't done this yet, see hello following articles for information on creating and preparing hello virtual machine:</span></span>

* [<span data-ttu-id="b3114-117">Vytvořit virtuální počítač z bitové kopie</span><span class="sxs-lookup"><span data-stu-id="b3114-117">Create a virtual machine from an image</span></span>](createportal.md)
* [<span data-ttu-id="b3114-118">Jak tooattach datový disk tooa virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b3114-118">How tooattach a data disk tooa virtual machine</span></span>](attach-disk.md)
* <span data-ttu-id="b3114-119">Ujistěte se, že role serveru hello jsou podporované pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="b3114-119">Make sure hello server roles are supported with Sysprep.</span></span> <span data-ttu-id="b3114-120">Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span><span class="sxs-lookup"><span data-stu-id="b3114-120">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span></span>

> [!WARNING]
> <span data-ttu-id="b3114-121">Tento proces odstraní hello původní virtuální počítač po jejím zaznamenání.</span><span class="sxs-lookup"><span data-stu-id="b3114-121">This process deletes hello original virtual machine after it's captured.</span></span>
>
>

<span data-ttu-id="b3114-122">Předchozí toocapturing na bitovou kopii virtuálního počítače Azure, se doporučuje zálohovat hello cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b3114-122">Prior toocapturing an image of an Azure virtual machine, it is recommended hello target virtual machine be backed up.</span></span> <span data-ttu-id="b3114-123">Virtuální počítače Azure můžete zálohovat pomocí služby Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="b3114-123">Azure virtual machines can be backed up using Azure Backup.</span></span> <span data-ttu-id="b3114-124">Podrobnosti najdete v článku [Zálohování virtuálních počítačů Azure](../../../backup/backup-azure-vms.md).</span><span class="sxs-lookup"><span data-stu-id="b3114-124">For details, see [Back up Azure virtual machines](../../../backup/backup-azure-vms.md).</span></span> <span data-ttu-id="b3114-125">Další řešení jsou k dispozici od certifikovaných partnerů.</span><span class="sxs-lookup"><span data-stu-id="b3114-125">Other solutions are available from certified partners.</span></span> <span data-ttu-id="b3114-126">toofind, co je aktuálně k dispozici, vyhledejte hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b3114-126">toofind out what’s currently available, search hello Azure Marketplace.</span></span>

## <a name="capture-hello-virtual-machine"></a><span data-ttu-id="b3114-127">Zachytit virtuální počítač hello</span><span class="sxs-lookup"><span data-stu-id="b3114-127">Capture hello virtual machine</span></span>
1. <span data-ttu-id="b3114-128">V hello [portál Azure](http://portal.azure.com), **připojit** toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b3114-128">In hello [Azure portal](http://portal.azure.com), **Connect** toohello virtual machine.</span></span> <span data-ttu-id="b3114-129">Pokyny najdete v tématu [jak toosign v tooa virtuální počítač se systémem Windows Server][How toosign in tooa virtual machine running Windows Server].</span><span class="sxs-lookup"><span data-stu-id="b3114-129">For instructions, see [How toosign in tooa virtual machine running Windows Server][How toosign in tooa virtual machine running Windows Server].</span></span>
2. <span data-ttu-id="b3114-130">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="b3114-130">Open a Command Prompt window as an administrator.</span></span>
3. <span data-ttu-id="b3114-131">Změnit adresář, hello příliš`%windir%\system32\sysprep`, a poté spusťte sysprep.exe.</span><span class="sxs-lookup"><span data-stu-id="b3114-131">Change hello directory too`%windir%\system32\sysprep`, and then run sysprep.exe.</span></span>
4. <span data-ttu-id="b3114-132">Hello **nástroj pro přípravu systému** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b3114-132">hello **System Preparation Tool** dialog box appears.</span></span> <span data-ttu-id="b3114-133">Hello následující:</span><span class="sxs-lookup"><span data-stu-id="b3114-133">Do hello following:</span></span>

   * <span data-ttu-id="b3114-134">V **Akce čištění systému**, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)** a ujistěte se, že **generalizace** je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="b3114-134">In **System Cleanup Action**, select **Enter System Out-of-Box Experience (OOBE)** and make sure that **Generalize** is checked.</span></span> <span data-ttu-id="b3114-135">Další informace o používání nástroje Sysprep najdete v tématu [jak tooUse nástroje Sysprep: Úvod][How tooUse Sysprep: An Introduction].</span><span class="sxs-lookup"><span data-stu-id="b3114-135">For more information about using Sysprep, see [How tooUse Sysprep: An Introduction][How tooUse Sysprep: An Introduction].</span></span>
   * <span data-ttu-id="b3114-136">V **možnosti vypnutí**, vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="b3114-136">In **Shutdown Options**, select **Shutdown**.</span></span>
   * <span data-ttu-id="b3114-137">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3114-137">Click **OK**.</span></span>

   ![Spusťte nástroj Sysprep](./media/capture-image/SysprepGeneral.png)
5. <span data-ttu-id="b3114-139">Nástroj Sysprep ukončí hello virtuálního počítače, které změní stav hello hello virtuálního počítače v hello portál Azure příliš**Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="b3114-139">Sysprep shuts down hello virtual machine, which changes hello status of hello virtual machine in hello Azure portal too**Stopped**.</span></span>
6. <span data-ttu-id="b3114-140">V hello portálu Azure, klikněte na **virtuálních počítačů (klasické)** a vyberte hello chcete toocapture virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b3114-140">In hello Azure portal, click **Virtual Machines (classic)** and select hello virtual machine you want toocapture.</span></span> <span data-ttu-id="b3114-141">Hello **Image virtuálních počítačů (klasické)** skupina je uvedena v části **výpočetní** při prohlížení **další služby**.</span><span class="sxs-lookup"><span data-stu-id="b3114-141">hello **VM images (classic)** group is listed under **Compute** when you view **More services**.</span></span>

7. <span data-ttu-id="b3114-142">Na panelu příkazů hello, klikněte na tlačítko **zaznamenat**.</span><span class="sxs-lookup"><span data-stu-id="b3114-142">On hello command bar, click **Capture**.</span></span>

   ![Zachytit virtuální počítač](./media/capture-image/CaptureVM.png)

   <span data-ttu-id="b3114-144">Hello **hello zachycení virtuálního počítače** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b3114-144">hello **Capture hello Virtual Machine** dialog box appears.</span></span>

8. <span data-ttu-id="b3114-145">V **název bitové kopie**, zadejte název pro novou bitovou kopii hello.</span><span class="sxs-lookup"><span data-stu-id="b3114-145">In **Image name**, type a name for hello new image.</span></span> <span data-ttu-id="b3114-146">V **popisek bitové kopie**, zadejte popisek hello novou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="b3114-146">In **Image label**, type a label for hello new image.</span></span>

9. <span data-ttu-id="b3114-147">Klikněte na tlačítko **na hello virtuálního počítače jste spustit program Sysprep**.</span><span class="sxs-lookup"><span data-stu-id="b3114-147">Click **I've run Sysprep on hello virtual machine**.</span></span> <span data-ttu-id="b3114-148">Toto políčko odkazuje toohello akce pomocí nástroje Sysprep v krocích 3 až 5.</span><span class="sxs-lookup"><span data-stu-id="b3114-148">This checkbox refers toohello actions with Sysprep in steps 3-5.</span></span> <span data-ttu-id="b3114-149">Obrázek _musí_ zobecněny spuštěním nástroje Sysprep předtím, než přidáte Windows Server image tooyour sadu vlastních bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="b3114-149">An image _must_ be generalized by running Sysprep before you add a Windows Server image tooyour set of custom images.</span></span>

10. <span data-ttu-id="b3114-150">Po dokončení hello zachycení hello novou bitovou kopii k dispozici v hello **Marketplace**, v hello **výpočetní**, **Image virtuálních počítačů (klasické)** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="b3114-150">Once hello capture completes, hello new image becomes available in hello **Marketplace**, in hello **Compute**, **VM images (classic)** container.</span></span>

    ![Zachycení Image úspěšné](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a><span data-ttu-id="b3114-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b3114-152">Next steps</span></span>
<span data-ttu-id="b3114-153">Hello bitová kopie je připravená toobe používá toocreate virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b3114-153">hello image is ready toobe used toocreate virtual machines.</span></span> <span data-ttu-id="b3114-154">toodo, vytvoříte virtuální počítač tak, že vyberete hello **další služby** položky nabídky v dolní části hello hello nabídky služeb, pak **Image virtuálních počítačů (klasické)** v hello **výpočetní**skupiny.</span><span class="sxs-lookup"><span data-stu-id="b3114-154">toodo this, you'll create a virtual machine by selecting hello **More services** menu item at hello bottom of hello services menu, then **VM images (classic)** in hello **Compute** group.</span></span> <span data-ttu-id="b3114-155">Pokyny najdete v tématu [vytvořit virtuální počítač z bitové kopie](createportal.md).</span><span class="sxs-lookup"><span data-stu-id="b3114-155">For instructions, see [Create a virtual machine from an image](createportal.md).</span></span>

[How toosign in tooa virtual machine running Windows Server]:connect-logon.md
[How tooUse Sysprep: An Introduction]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/capture-image/SysprepGeneral.png
[hello virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of hello virtual machine]: ./media/capture-image/CaptureVM.png
[Enter hello image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use hello captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
