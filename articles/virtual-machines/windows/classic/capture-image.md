---
title: "Vytvořte bitovou kopii virtuálního počítače Windows Azure | Microsoft Docs"
description: "Zachycení image virtuálního počítače Azure s Windows vytvořeného pomocí modelu klasického nasazení"
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
ms.openlocfilehash: 6032263848c469ce2f416306e5c91c29f4cb30e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a><span data-ttu-id="5ab02-103">Zachycení image virtuálního počítače Azure s Windows vytvořeného pomocí modelu klasického nasazení</span><span class="sxs-lookup"><span data-stu-id="5ab02-103">Capture an image of an Azure Windows virtual machine created with the classic deployment model.</span></span>
> [!IMPORTANT]
> <span data-ttu-id="5ab02-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5ab02-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="5ab02-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="5ab02-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="5ab02-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5ab02-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="5ab02-107">Informace o modelu Resource Manager, najdete v části [zaznamenat bitovou kopii spravované zobecněný virtuálního počítače v Azure](../capture-image-resource.md).</span><span class="sxs-lookup"><span data-stu-id="5ab02-107">For Resource Manager model information, see [Capture a managed image of a generalized VM in Azure](../capture-image-resource.md).</span></span>

<span data-ttu-id="5ab02-108">Tento článek ukazuje, jak zachytit virtuální počítač Azure se systémem Windows, můžete ho použít jako obrázek pro vytvoření dalších virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5ab02-108">This article shows you how to capture an Azure virtual machine running Windows so you can use it as an image to create other virtual machines.</span></span> <span data-ttu-id="5ab02-109">Tento image obsahuje disk operačního systému a datové disky, které jsou připojené k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="5ab02-109">This image includes the operating system disk and any data disks that are attached to the virtual machine.</span></span> <span data-ttu-id="5ab02-110">Neobsahuje síťových konfigurací, takže budete muset nastavit konfiguraci sítě, když vytvoříte další virtuální počítače, které použít bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="5ab02-110">It doesn't include networking configurations, so you'll need to set up network configurations when you create the other virtual machines that use the image.</span></span>

<span data-ttu-id="5ab02-111">Obrázek v Azure uloží **Image virtuálních počítačů (klasické)**, **výpočetní** služba, která je uvedena při zobrazení všech služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="5ab02-111">Azure stores the image under **VM images (classic)**, a **Compute** service that is listed when you view all the Azure services.</span></span> <span data-ttu-id="5ab02-112">Toto je na stejném místě, kde jsou uložené všechny Image, které jste odeslali.</span><span class="sxs-lookup"><span data-stu-id="5ab02-112">This is the same place where any images you've uploaded are stored.</span></span> <span data-ttu-id="5ab02-113">Podrobnosti o bitových kopiích naleznete v tématu [o bitové kopie u virtuálních počítačů](about-images.md?toc=%2fazure%2fvirtual-machines%2fWindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5ab02-113">For details about images, see [About images for virtual machines](about-images.md?toc=%2fazure%2fvirtual-machines%2fWindows%2fclassic%2ftoc.json).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="5ab02-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="5ab02-114">Before you begin</span></span>
<span data-ttu-id="5ab02-115">Tento postup předpokládá, že jste vytvořili virtuální počítač Azure a nakonfigurovat operačního systému, včetně připojení jakýchkoli datových disků.</span><span class="sxs-lookup"><span data-stu-id="5ab02-115">These steps assume that you've already created an Azure virtual machine and configured the operating system, including attaching any data disks.</span></span> <span data-ttu-id="5ab02-116">Pokud jste to ještě neudělali, najdete v následujících článcích informace o vytváření a příprava virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="5ab02-116">If you haven't done this yet, see the following articles for information on creating and preparing the virtual machine:</span></span>

* [<span data-ttu-id="5ab02-117">Vytvořit virtuální počítač z bitové kopie</span><span class="sxs-lookup"><span data-stu-id="5ab02-117">Create a virtual machine from an image</span></span>](createportal.md)
* [<span data-ttu-id="5ab02-118">Tom, jak připojit datový disk k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="5ab02-118">How to attach a data disk to a virtual machine</span></span>](attach-disk.md)
* <span data-ttu-id="5ab02-119">Ujistěte se, že role serveru jsou podporované pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="5ab02-119">Make sure the server roles are supported with Sysprep.</span></span> <span data-ttu-id="5ab02-120">Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span><span class="sxs-lookup"><span data-stu-id="5ab02-120">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span></span>

> [!WARNING]
> <span data-ttu-id="5ab02-121">Tento proces odstraní původní virtuální počítač po jejím zaznamenání.</span><span class="sxs-lookup"><span data-stu-id="5ab02-121">This process deletes the original virtual machine after it's captured.</span></span>
>
>

<span data-ttu-id="5ab02-122">Před zachycením bitové kopie virtuálního počítače Azure, se doporučuje zálohovat cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5ab02-122">Prior to capturing an image of an Azure virtual machine, it is recommended the target virtual machine be backed up.</span></span> <span data-ttu-id="5ab02-123">Virtuální počítače Azure můžete zálohovat pomocí služby Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="5ab02-123">Azure virtual machines can be backed up using Azure Backup.</span></span> <span data-ttu-id="5ab02-124">Podrobnosti najdete v článku [Zálohování virtuálních počítačů Azure](../../../backup/backup-azure-vms.md).</span><span class="sxs-lookup"><span data-stu-id="5ab02-124">For details, see [Back up Azure virtual machines](../../../backup/backup-azure-vms.md).</span></span> <span data-ttu-id="5ab02-125">Další řešení jsou k dispozici od certifikovaných partnerů.</span><span class="sxs-lookup"><span data-stu-id="5ab02-125">Other solutions are available from certified partners.</span></span> <span data-ttu-id="5ab02-126">Pokud chcete zjistit, co je právě k dispozici, prohledejte web Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="5ab02-126">To find out what’s currently available, search the Azure Marketplace.</span></span>

## <a name="capture-the-virtual-machine"></a><span data-ttu-id="5ab02-127">Zachytit virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="5ab02-127">Capture the virtual machine</span></span>
1. <span data-ttu-id="5ab02-128">V [portál Azure](http://portal.azure.com), **Connect** k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="5ab02-128">In the [Azure portal](http://portal.azure.com), **Connect** to the virtual machine.</span></span> <span data-ttu-id="5ab02-129">Pokyny najdete v tématu [jak se přihlásit k virtuálnímu počítači s Windows serverem][How to sign in to a virtual machine running Windows Server].</span><span class="sxs-lookup"><span data-stu-id="5ab02-129">For instructions, see [How to sign in to a virtual machine running Windows Server][How to sign in to a virtual machine running Windows Server].</span></span>
2. <span data-ttu-id="5ab02-130">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="5ab02-130">Open a Command Prompt window as an administrator.</span></span>
3. <span data-ttu-id="5ab02-131">Změňte adresář na `%windir%\system32\sysprep`, a poté spusťte sysprep.exe.</span><span class="sxs-lookup"><span data-stu-id="5ab02-131">Change the directory to `%windir%\system32\sysprep`, and then run sysprep.exe.</span></span>
4. <span data-ttu-id="5ab02-132">**Nástroj pro přípravu systému** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5ab02-132">The **System Preparation Tool** dialog box appears.</span></span> <span data-ttu-id="5ab02-133">Udělejte toto:</span><span class="sxs-lookup"><span data-stu-id="5ab02-133">Do the following:</span></span>

   * <span data-ttu-id="5ab02-134">V **Akce čištění systému**, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)** a ujistěte se, že **generalizace** je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="5ab02-134">In **System Cleanup Action**, select **Enter System Out-of-Box Experience (OOBE)** and make sure that **Generalize** is checked.</span></span> <span data-ttu-id="5ab02-135">Další informace o používání nástroje Sysprep najdete v tématu [postup použití nástroje Sysprep: Úvod][How to Use Sysprep: An Introduction].</span><span class="sxs-lookup"><span data-stu-id="5ab02-135">For more information about using Sysprep, see [How to Use Sysprep: An Introduction][How to Use Sysprep: An Introduction].</span></span>
   * <span data-ttu-id="5ab02-136">V **možnosti vypnutí**, vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="5ab02-136">In **Shutdown Options**, select **Shutdown**.</span></span>
   * <span data-ttu-id="5ab02-137">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5ab02-137">Click **OK**.</span></span>

   ![Spusťte nástroj Sysprep](./media/capture-image/SysprepGeneral.png)
5. <span data-ttu-id="5ab02-139">Nástroj Sysprep vypne virtuální počítač, který změní stav virtuálního počítače na portálu Azure **Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="5ab02-139">Sysprep shuts down the virtual machine, which changes the status of the virtual machine in the Azure portal to **Stopped**.</span></span>
6. <span data-ttu-id="5ab02-140">Na portálu Azure klikněte na tlačítko **virtuálních počítačů (klasické)** a vyberte virtuální počítač, který chcete zaznamenat.</span><span class="sxs-lookup"><span data-stu-id="5ab02-140">In the Azure portal, click **Virtual Machines (classic)** and select the virtual machine you want to capture.</span></span> <span data-ttu-id="5ab02-141">**Image virtuálních počítačů (klasické)** skupina je uvedena v části **výpočetní** při prohlížení **další služby**.</span><span class="sxs-lookup"><span data-stu-id="5ab02-141">The **VM images (classic)** group is listed under **Compute** when you view **More services**.</span></span>

7. <span data-ttu-id="5ab02-142">Na panelu příkazů klikněte na tlačítko **zaznamenat**.</span><span class="sxs-lookup"><span data-stu-id="5ab02-142">On the command bar, click **Capture**.</span></span>

   ![Zachytit virtuální počítač](./media/capture-image/CaptureVM.png)

   <span data-ttu-id="5ab02-144">**Zachytit virtuální počítač** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5ab02-144">The **Capture the Virtual Machine** dialog box appears.</span></span>

8. <span data-ttu-id="5ab02-145">V **název bitové kopie**, zadejte název pro novou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="5ab02-145">In **Image name**, type a name for the new image.</span></span> <span data-ttu-id="5ab02-146">V **popisek bitové kopie**, zadejte popisek pro novou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="5ab02-146">In **Image label**, type a label for the new image.</span></span>

9. <span data-ttu-id="5ab02-147">Klikněte na tlačítko **na virtuální počítač jste spustit program Sysprep**.</span><span class="sxs-lookup"><span data-stu-id="5ab02-147">Click **I've run Sysprep on the virtual machine**.</span></span> <span data-ttu-id="5ab02-148">Toto políčko odkazuje na akce pomocí nástroje Sysprep v krocích 3 až 5.</span><span class="sxs-lookup"><span data-stu-id="5ab02-148">This checkbox refers to the actions with Sysprep in steps 3-5.</span></span> <span data-ttu-id="5ab02-149">Obrázek _musí_ zobecněny spuštěním nástroje Sysprep předtím, než přidáte bitovou kopii systému Windows Server do sady vlastních bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="5ab02-149">An image _must_ be generalized by running Sysprep before you add a Windows Server image to your set of custom images.</span></span>

10. <span data-ttu-id="5ab02-150">Po dokončení zachytávání, bude k dispozici v novou bitovou kopii **Marketplace**v **výpočetní**, **Image virtuálních počítačů (klasické)** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5ab02-150">Once the capture completes, the new image becomes available in the **Marketplace**, in the **Compute**, **VM images (classic)** container.</span></span>

    ![Zachycení Image úspěšné](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a><span data-ttu-id="5ab02-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5ab02-152">Next steps</span></span>
<span data-ttu-id="5ab02-153">Obrázek je připravený k použití pro vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5ab02-153">The image is ready to be used to create virtual machines.</span></span> <span data-ttu-id="5ab02-154">To provedete tak, že vyberete vytvoříte virtuální počítač **další služby** položky nabídky v dolní části nabídky služeb, pak **Image virtuálních počítačů (klasické)** v **výpočetní** skupiny.</span><span class="sxs-lookup"><span data-stu-id="5ab02-154">To do this, you'll create a virtual machine by selecting the **More services** menu item at the bottom of the services menu, then **VM images (classic)** in the **Compute** group.</span></span> <span data-ttu-id="5ab02-155">Pokyny najdete v tématu [vytvořit virtuální počítač z bitové kopie](createportal.md).</span><span class="sxs-lookup"><span data-stu-id="5ab02-155">For instructions, see [Create a virtual machine from an image](createportal.md).</span></span>

[How to sign in to a virtual machine running Windows Server]:connect-logon.md
[How to Use Sysprep: An Introduction]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
