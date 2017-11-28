---
title: "Zřídit pole virtuální zařízení StorSimple technologie Hyper-v | Microsoft Docs"
description: "Tento druhý kurz v nasazení pole virtuální zařízení StorSimple zahrnuje zřizování virtuální pole Hyper-v."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 4354963c-e09d-41ac-9c8b-f21abeae9913
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bad431c8958f7d381bb9c0410caa3a57c6e75c19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-hyper-v"></a><span data-ttu-id="14f73-103">Nasazení zařízení StorSimple virtuální pole - zřízení technologie Hyper-v</span><span class="sxs-lookup"><span data-stu-id="14f73-103">Deploy StorSimple Virtual Array - Provision in Hyper-V</span></span>
![](./media/storsimple-virtual-array-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a><span data-ttu-id="14f73-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="14f73-104">Overview</span></span>
<span data-ttu-id="14f73-105">Tento kurz popisuje, jak zřídit o pole virtuální zařízení StorSimple v systému hostitele s technologií Hyper-V v systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="14f73-105">This tutorial describes how to provision a StorSimple Virtual Array on a host system running Hyper-V on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2.</span></span> <span data-ttu-id="14f73-106">Tento článek se týká nasazení pole virtuální zařízení StorSimple v portálu Azure a cloudu Microsoft Azure Government.</span><span class="sxs-lookup"><span data-stu-id="14f73-106">This article applies to the deployment of StorSimple Virtual Arrays in Azure portal and Microsoft Azure Government Cloud.</span></span>

<span data-ttu-id="14f73-107">Můžete potřebovat oprávnění správce zřídit a nakonfigurovat virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="14f73-107">You need administrator privileges to provision and configure a virtual array.</span></span> <span data-ttu-id="14f73-108">Zřizování a počáteční instalace může trvat přibližně 10 minut.</span><span class="sxs-lookup"><span data-stu-id="14f73-108">The provisioning and initial setup can take around 10 minutes to complete.</span></span>

## <a name="provisioning-prerequisites"></a><span data-ttu-id="14f73-109">Zřizování požadavky</span><span class="sxs-lookup"><span data-stu-id="14f73-109">Provisioning prerequisites</span></span>
<span data-ttu-id="14f73-110">Tady najdete požadavky zřídit virtuální pole v systému hostitele s technologií Hyper-V v systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="14f73-110">Here you will find the prerequisites to provision a virtual array on a host system running Hyper-V on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2.</span></span>

### <a name="for-the-storsimple-device-manager-service"></a><span data-ttu-id="14f73-111">Služba Správce zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="14f73-111">For the StorSimple Device Manager service</span></span>
<span data-ttu-id="14f73-112">Než začnete, ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="14f73-112">Before you begin, make sure that:</span></span>

* <span data-ttu-id="14f73-113">Jste dokončili všechny kroky v [připravit na portálu pro pole virtuální zařízení StorSimple](storsimple-virtual-array-deploy1-portal-prep.md).</span><span class="sxs-lookup"><span data-stu-id="14f73-113">You have completed all the steps in [Prepare the portal for StorSimple Virtual Array](storsimple-virtual-array-deploy1-portal-prep.md).</span></span>
* <span data-ttu-id="14f73-114">Obrázek virtuální pole pro Hyper-V jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="14f73-114">You have downloaded the virtual array image for Hyper-V from the Azure portal.</span></span> <span data-ttu-id="14f73-115">Další informace najdete v tématu **krok 3: stáhnout bitovou kopii virtuálního pole** z [připravit na portálu pro Průvodce pole virtuální zařízení StorSimple](storsimple-virtual-array-deploy1-portal-prep.md).</span><span class="sxs-lookup"><span data-stu-id="14f73-115">For more information, see **Step 3: Download the virtual array image** of [Prepare the portal for StorSimple Virtual Array guide](storsimple-virtual-array-deploy1-portal-prep.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="14f73-116">Software spuštěný na poli virtuální zařízení StorSimple lze použít pouze se službou Správce zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="14f73-116">The software running on the StorSimple Virtual Array may only be used with the StorSimple Device Manager service.</span></span>
  >
  >

### <a name="for-the-storsimple-virtual-array"></a><span data-ttu-id="14f73-117">Pro pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="14f73-117">For the StorSimple Virtual Array</span></span>
<span data-ttu-id="14f73-118">Před nasazením virtuální pole, ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="14f73-118">Before you deploy a virtual array, make sure that:</span></span>

* <span data-ttu-id="14f73-119">Máte přístup k systému hostitele, který používá technologie Hyper-V v systému Windows Server 2008 R2 nebo novější, lze použít k zřízení zařízení.</span><span class="sxs-lookup"><span data-stu-id="14f73-119">You have access to a host system running Hyper-V on Windows Server 2008 R2 or later that can be used to a provision a device.</span></span>
* <span data-ttu-id="14f73-120">Systém hostitele je možné vyhradit následující prostředky pro zřízení virtuální pole:</span><span class="sxs-lookup"><span data-stu-id="14f73-120">The host system is able to dedicate the following resources to provision your virtual array:</span></span>

  * <span data-ttu-id="14f73-121">Minimálně 4 jádra.</span><span class="sxs-lookup"><span data-stu-id="14f73-121">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="14f73-122">Alespoň 8 GB paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="14f73-122">At least 8 GB of RAM.</span></span> <span data-ttu-id="14f73-123">Pokud chcete konfigurovat virtuální pole jako souborový server, 8 GB podporuje menší než 2 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="14f73-123">If you plan to configure the virtual array as file server, 8 GB supports less than 2 million files.</span></span> <span data-ttu-id="14f73-124">Je nutné 16 GB paměti RAM pro podporu 2 – 4 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="14f73-124">You need 16 GB RAM to support 2 - 4 million files.</span></span>
  * <span data-ttu-id="14f73-125">Jedno síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="14f73-125">One network interface.</span></span>
  * <span data-ttu-id="14f73-126">500 GB virtuální disk pro data.</span><span class="sxs-lookup"><span data-stu-id="14f73-126">A 500 GB virtual disk for data.</span></span>

### <a name="for-the-network-in-the-datacenter"></a><span data-ttu-id="14f73-127">Síť v datovém centru</span><span class="sxs-lookup"><span data-stu-id="14f73-127">For the network in the datacenter</span></span>
<span data-ttu-id="14f73-128">Než začnete, zkontrolujte požadavky na sítě k nasazení pole virtuální zařízení StorSimple a odpovídajícím způsobem nakonfigurovat síti datového centra.</span><span class="sxs-lookup"><span data-stu-id="14f73-128">Before you begin, review the networking requirements to deploy a StorSimple Virtual Array and configure the datacenter network appropriately.</span></span> <span data-ttu-id="14f73-129">Další informace najdete v tématu [pole virtuální zařízení StorSimple sítě požadavky](storsimple-ova-system-requirements.md#networking-requirements).</span><span class="sxs-lookup"><span data-stu-id="14f73-129">For more information, see [StorSimple Virtual Array networking requirements](storsimple-ova-system-requirements.md#networking-requirements).</span></span>

## <a name="step-by-step-provisioning"></a><span data-ttu-id="14f73-130">Krok za krokem zřizování</span><span class="sxs-lookup"><span data-stu-id="14f73-130">Step-by-step provisioning</span></span>
<span data-ttu-id="14f73-131">Pokud chcete zřídit a připojit k virtuální poli, musíte provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="14f73-131">To provision and connect to a virtual array, you need to perform the following steps:</span></span>

1. <span data-ttu-id="14f73-132">Zajistěte, aby hostitelský systém dostatek prostředků ke splňují požadavky na minimální virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="14f73-132">Ensure that the host system has sufficient resources to meet the minimum virtual array requirements.</span></span>
2. <span data-ttu-id="14f73-133">Zřídit virtuální pole ve vašem hypervisoru.</span><span class="sxs-lookup"><span data-stu-id="14f73-133">Provision a virtual array in your hypervisor.</span></span>
3. <span data-ttu-id="14f73-134">Spusťte virtuální pole a získat IP adresu.</span><span class="sxs-lookup"><span data-stu-id="14f73-134">Start the virtual array and get the IP address.</span></span>

<span data-ttu-id="14f73-135">Každý z těchto kroků je vysvětlené v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="14f73-135">Each of these steps is explained in the following sections.</span></span>

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-array-requirements"></a><span data-ttu-id="14f73-136">Krok 1: Ujistěte se, že hostitelský systém splňuje požadavky na minimální virtuální pole</span><span class="sxs-lookup"><span data-stu-id="14f73-136">Step 1: Ensure that the host system meets minimum virtual array requirements</span></span>
<span data-ttu-id="14f73-137">Chcete-li vytvořit virtuální pole, je třeba:</span><span class="sxs-lookup"><span data-stu-id="14f73-137">To create a virtual array, you need:</span></span>

* <span data-ttu-id="14f73-138">Role Hyper-V nainstalovaná v systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="14f73-138">The Hyper-V role installed on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2 SP1.</span></span>
* <span data-ttu-id="14f73-139">Microsoft Hyper-V Manager v klientském počítači Microsoft Windows, který je připojen k hostiteli.</span><span class="sxs-lookup"><span data-stu-id="14f73-139">Microsoft Hyper-V Manager on a Microsoft Windows client connected to the host.</span></span>

<span data-ttu-id="14f73-140">Ujistěte se, že je možné vyhradit v následujících zdrojích informací na vaše virtuální pole základní hardware (hostitelský systém), na kterém vytváříte virtuální pole:</span><span class="sxs-lookup"><span data-stu-id="14f73-140">Make sure that the underlying hardware (host system) on which you are creating the virtual array is able to dedicate the following resources to your virtual array:</span></span>

* <span data-ttu-id="14f73-141">Minimálně 4 jádra.</span><span class="sxs-lookup"><span data-stu-id="14f73-141">A minimum of 4 cores.</span></span>
* <span data-ttu-id="14f73-142">Alespoň 8 GB paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="14f73-142">At least 8 GB of RAM.</span></span> <span data-ttu-id="14f73-143">Pokud chcete konfigurovat virtuální pole jako souborový server, 8 GB podporuje menší než 2 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="14f73-143">If you plan to configure the virtual array as file server, 8 GB supports less than 2 million files.</span></span> <span data-ttu-id="14f73-144">Je nutné 16 GB paměti RAM pro podporu 2 – 4 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="14f73-144">You need 16 GB RAM to support 2 - 4 million files.</span></span>
* <span data-ttu-id="14f73-145">Jedno síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="14f73-145">One network interface.</span></span>
* <span data-ttu-id="14f73-146">500 GB virtuální disk pro data systému.</span><span class="sxs-lookup"><span data-stu-id="14f73-146">A 500 GB virtual disk for system data.</span></span>

## <a name="step-2-provision-a-virtual-array-in-hypervisor"></a><span data-ttu-id="14f73-147">Krok 2: Zřídit virtuální pole v hypervisoru</span><span class="sxs-lookup"><span data-stu-id="14f73-147">Step 2: Provision a virtual array in hypervisor</span></span>
<span data-ttu-id="14f73-148">Proveďte následující kroky pro zřízení zařízení ve vaší hypervisoru.</span><span class="sxs-lookup"><span data-stu-id="14f73-148">Perform the following steps to provision a device in your hypervisor.</span></span>

#### <a name="to-provision-a-virtual-array"></a><span data-ttu-id="14f73-149">Zřizování virtuální pole</span><span class="sxs-lookup"><span data-stu-id="14f73-149">To provision a virtual array</span></span>
1. <span data-ttu-id="14f73-150">Na hostiteli s Windows Server zkopírujte bitovou kopii virtuálního pole na místní disk.</span><span class="sxs-lookup"><span data-stu-id="14f73-150">On your Windows Server host, copy the virtual array image to a local drive.</span></span> <span data-ttu-id="14f73-151">Jste si stáhli tuto bitovou kopii (VHD nebo VHDX) prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="14f73-151">You downloaded this image (VHD or VHDX) through the Azure portal.</span></span> <span data-ttu-id="14f73-152">Poznamenejte si umístění, kam můžete zkopírovat bitovou kopii jako používáte tuto bitovou kopii později v postupu.</span><span class="sxs-lookup"><span data-stu-id="14f73-152">Make a note of the location where you copied the image as you are using this image later in the procedure.</span></span>
2. <span data-ttu-id="14f73-153">Otevřete **správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="14f73-153">Open **Server Manager**.</span></span> <span data-ttu-id="14f73-154">V pravém horním rohu, klikněte na **nástroje** a vyberte **Správce technologie Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="14f73-154">In the top right corner, click **Tools** and select **Hyper-V Manager**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image1.png)  

   <span data-ttu-id="14f73-155">Pokud používáte Windows Server 2008 R2, otevřete Správce technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="14f73-155">If you are running Windows Server 2008 R2, open the Hyper-V Manager.</span></span> <span data-ttu-id="14f73-156">Ve Správci serveru klikněte na tlačítko **role > Hyper-V > Správce technologie Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="14f73-156">In Server Manager, click **Roles > Hyper-V > Hyper-V Manager**.</span></span>
3. <span data-ttu-id="14f73-157">V **Správce technologie Hyper-V**, v podokně oboru klikněte pravým tlačítkem na uzel váš systém otevřete kontextu nabídku a pak klikněte na tlačítko **nový** > **virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="14f73-157">In **Hyper-V Manager**, in the scope pane, right-click your system node to open the context menu, and then click **New** > **Virtual Machine**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image2.png)
4. <span data-ttu-id="14f73-158">Na **před zahájením** stránky Průvodce novým virtuálním počítačem, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="14f73-158">On the **Before you begin** page of the New Virtual Machine Wizard, click **Next**.</span></span>
5. <span data-ttu-id="14f73-159">Na **zadejte název a umístění** zadejte **název** pro vaše virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="14f73-159">On the **Specify name and location** page, provide a **Name** for your virtual array.</span></span> <span data-ttu-id="14f73-160">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="14f73-160">Click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image4.png)
6. <span data-ttu-id="14f73-161">Na **určit generaci** stránky, vyberte typ obrázku zařízení a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="14f73-161">On the **Specify generation** page, choose the device image type, and then click **Next**.</span></span> <span data-ttu-id="14f73-162">Tato stránka se nezobrazí, pokud používáte Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="14f73-162">This page doesn't appear if you're using Windows Server 2008 R2.</span></span>

   * <span data-ttu-id="14f73-163">Zvolte **generace 2** Pokud jste stáhli bitovou kopii .vhdx pro Windows Server 2012 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="14f73-163">Choose **Generation 2** if you downloaded a .vhdx image for Windows Server 2012 or later.</span></span>
   * <span data-ttu-id="14f73-164">Zvolte **generace 1** Pokud jste stáhli VHD image pro Windows Server 2008 R2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="14f73-164">Choose **Generation 1** if you downloaded a .vhd image for Windows Server 2008 R2 or later.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image5.png)
7. <span data-ttu-id="14f73-165">Na **přiřazení paměti** stránky, zadejte **spouštěcí paměť** z alespoň **8192 MB**, nemáte povolte dynamickou paměť a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="14f73-165">On the **Assign memory** page, specify a **Startup memory** of at least **8192 MB**, don't enable dynamic memory, and then click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image6.png)  
8. <span data-ttu-id="14f73-166">Na **konfigurace sítě** zadejte virtuální přepínač, který je připojený k Internetu a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="14f73-166">On the **Configure networking** page, specify the virtual switch that is connected to the Internet and then click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image7.png)
9. <span data-ttu-id="14f73-167">Na **připojit virtuální pevný disk** vyberte **použít existující virtuální pevný disk**, zadejte umístění bitové kopie virtuálních pole (vhdx nebo VHD) a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="14f73-167">On the **Connect virtual hard disk** page, choose **Use an existing virtual hard disk**, specify the location of the virtual array image (.vhdx or .vhd), and then click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image8m.png)
10. <span data-ttu-id="14f73-168">Zkontrolujte **Souhrn** a pak klikněte na **Dokončit** k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="14f73-168">Review the **Summary** and then click **Finish** to create the virtual machine.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image9.png)
11. <span data-ttu-id="14f73-169">Chcete-li splňují minimální požadavky, je nutné 4 jádra.</span><span class="sxs-lookup"><span data-stu-id="14f73-169">To meet the minimum requirements, you need 4 cores.</span></span> <span data-ttu-id="14f73-170">Chcete-li přidat 4 virtuální procesory, vyberte vašeho hostitelského systému v **Správce technologie Hyper-V** okno.</span><span class="sxs-lookup"><span data-stu-id="14f73-170">To add 4 virtual processors, select your host system in the **Hyper-V Manager** window.</span></span> <span data-ttu-id="14f73-171">V pravém podokně v části seznamu **virtuální počítače**, najít virtuálního počítače, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="14f73-171">In the right-pane under the list of **Virtual Machines**, locate the virtual machine you just created.</span></span> <span data-ttu-id="14f73-172">Vyberte a klikněte pravým tlačítkem na název počítače a vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="14f73-172">Select and right-click the machine name and select **Settings**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image10.png)
12. <span data-ttu-id="14f73-173">Na **nastavení** stránky, v levém podokně klikněte na tlačítko **procesoru**.</span><span class="sxs-lookup"><span data-stu-id="14f73-173">On the **Settings** page, in the left-pane, click **Processor**.</span></span> <span data-ttu-id="14f73-174">V pravém podokně, nastavte **počet virtuálních procesorů** 4 (nebo více).</span><span class="sxs-lookup"><span data-stu-id="14f73-174">In the right-pane, set **number of virtual processors** to 4 (or more).</span></span> <span data-ttu-id="14f73-175">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="14f73-175">Click **Apply**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image11.png)
13. <span data-ttu-id="14f73-176">Aby splňoval minimální požadavky, musíte taky přidat 500 GB virtuální datový disk.</span><span class="sxs-lookup"><span data-stu-id="14f73-176">To meet the minimum requirements, you also need to add a 500 GB virtual data disk.</span></span> <span data-ttu-id="14f73-177">V **nastavení** stránky:</span><span class="sxs-lookup"><span data-stu-id="14f73-177">In the **Settings** page:</span></span>

    1. <span data-ttu-id="14f73-178">V levém podokně vyberte **řadič SCSI**.</span><span class="sxs-lookup"><span data-stu-id="14f73-178">In the left pane, select **SCSI Controller**.</span></span>
    2. <span data-ttu-id="14f73-179">V pravém podokně vyberte **pevný disk,** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="14f73-179">In the right pane, select **Hard Drive,** and click **Add**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image12.png)
14. <span data-ttu-id="14f73-180">Na **pevný disk** vyberte **virtuální pevný disk** možnost a klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="14f73-180">On the **Hard drive** page, select the **Virtual hard disk** option and click **New**.</span></span> <span data-ttu-id="14f73-181">**Průvodce vytvořením virtuálního pevného disku** spustí.</span><span class="sxs-lookup"><span data-stu-id="14f73-181">The **New Virtual Hard Disk Wizard** starts.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image13.png)
15. <span data-ttu-id="14f73-182">Na **před zahájením** stránky Průvodce vytvořením virtuálního pevného disku, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="14f73-182">On the **Before you begin** page of the New Virtual Hard Disk Wizard, click **Next**.</span></span>
16. <span data-ttu-id="14f73-183">Na **stránky výběr formátu disku**, přijměte výchozí možnost **VHDX** formátu.</span><span class="sxs-lookup"><span data-stu-id="14f73-183">On the **Choose Disk Format page**, accept the default option of **VHDX** format.</span></span> <span data-ttu-id="14f73-184">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="14f73-184">Click **Next**.</span></span> <span data-ttu-id="14f73-185">Tato obrazovka není uveden, pokud systém Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="14f73-185">This screen is not presented if running Windows Server 2008 R2.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image15.png)
17. <span data-ttu-id="14f73-186">Na **zvolit typ disku stránky**, nastavte typ virtuálního pevného disku jako **dynamicky se zvětšující** (doporučeno).</span><span class="sxs-lookup"><span data-stu-id="14f73-186">On the **Choose Disk Type page**, set virtual hard disk type as **Dynamically expanding** (recommended).</span></span> <span data-ttu-id="14f73-187">**Pevná velikost** disk by pracovat, ale budete muset počkat dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="14f73-187">**Fixed size** disk would work but you may need to wait a long time.</span></span> <span data-ttu-id="14f73-188">Doporučujeme vám, že nepoužíváte **Differencing** možnost.</span><span class="sxs-lookup"><span data-stu-id="14f73-188">We recommend that you do not use the **Differencing** option.</span></span> <span data-ttu-id="14f73-189">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="14f73-189">Click **Next**.</span></span> <span data-ttu-id="14f73-190">V systému Windows Server 2012 R2 a Windows Server 2012 **dynamicky se zvětšující** je výchozí možnost, zatímco ve Windows serveru 2008 R2, výchozí hodnota je **pevnou velikost**.</span><span class="sxs-lookup"><span data-stu-id="14f73-190">In Windows Server 2012 R2 and Windows Server 2012, **Dynamically expanding** is the default option whereas in Windows Server 2008 R2, the default is **Fixed size**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image16.png)
18. <span data-ttu-id="14f73-191">Na **zadat název a umístění** zadejte **název** a také **umístění** (můžete vyhledat jednoho) pro datový disk.</span><span class="sxs-lookup"><span data-stu-id="14f73-191">On the **Specify Name and Location** page, provide a **name** as well as **location** (you can browse to one) for the data disk.</span></span> <span data-ttu-id="14f73-192">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="14f73-192">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image17.png)
19. <span data-ttu-id="14f73-193">Na **konfigurovat Disk** vyberte možnost **vytvořit nový prázdný virtuální pevný disk** a zadejte velikost jako **500 GB** (nebo více).</span><span class="sxs-lookup"><span data-stu-id="14f73-193">On the **Configure Disk** page, select the option **Create a new blank virtual hard disk** and specify the size as **500 GB** (or more).</span></span> <span data-ttu-id="14f73-194">Požadavek na minimální při 500 GB můžete zřídit vždy větší disku.</span><span class="sxs-lookup"><span data-stu-id="14f73-194">While 500 GB is the minimum requirement, you can always provision a larger disk.</span></span> <span data-ttu-id="14f73-195">Všimněte si, že nelze rozbalit nebo zmenšení disku jednou zřízený.</span><span class="sxs-lookup"><span data-stu-id="14f73-195">Note that you cannot expand or shrink the disk once provisioned.</span></span> <span data-ttu-id="14f73-196">Další informace o velikosti disku a zajišťují, projděte si část nastavení velikosti v [osvědčené postupy dokumentu](storsimple-ova-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="14f73-196">For more information on the size of disk to provision, review the sizing section in the [best practices document](storsimple-ova-best-practices.md).</span></span> <span data-ttu-id="14f73-197">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="14f73-197">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image18.png)
20. <span data-ttu-id="14f73-198">Na **Souhrn** , zkontrolujte podrobnosti virtuální datový disk a pokud splnit, klikněte na tlačítko **Dokončit** disk vytvoříte tak.</span><span class="sxs-lookup"><span data-stu-id="14f73-198">On the **Summary** page, review the details of your virtual data disk and if satisfied, click **Finish** to create the disk.</span></span> <span data-ttu-id="14f73-199">Průvodce se zavře a virtuální pevný disk se přidá do vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="14f73-199">The wizard closes and a virtual hard disk is added to your machine.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image19.png)
21. <span data-ttu-id="14f73-200">Vraťte se na **nastavení** stránky.</span><span class="sxs-lookup"><span data-stu-id="14f73-200">Return to the **Settings** page.</span></span> <span data-ttu-id="14f73-201">Klikněte na tlačítko **OK** zavřete **nastavení** stránky a vraťte se do Správce technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="14f73-201">Click **OK** to close the **Settings** page and return to Hyper-V Manager window.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-array-and-get-the-ip"></a><span data-ttu-id="14f73-202">Krok 3: Spuštění virtuální pole a získat IP adresu</span><span class="sxs-lookup"><span data-stu-id="14f73-202">Step 3: Start the virtual array and get the IP</span></span>
<span data-ttu-id="14f73-203">Proveďte následující postup spusťte virtuální pole a připojte se k němu.</span><span class="sxs-lookup"><span data-stu-id="14f73-203">Perform the following steps to start your virtual array and connect to it.</span></span>

#### <a name="to-start-the-virtual-array"></a><span data-ttu-id="14f73-204">Spustit virtuální pole</span><span class="sxs-lookup"><span data-stu-id="14f73-204">To start the virtual array</span></span>
1. <span data-ttu-id="14f73-205">Spusťte virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="14f73-205">Start the virtual array.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image21.png)
2. <span data-ttu-id="14f73-206">Po spuštění zařízení vyberte zařízení, klikněte pravým tlačítkem a vyberte **Connect**.</span><span class="sxs-lookup"><span data-stu-id="14f73-206">After the device is running, select the device, right click, and select **Connect**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image22.png)
3. <span data-ttu-id="14f73-207">Možná budete muset počkat 5 až 10 minut, než se zařízení bude připravená.</span><span class="sxs-lookup"><span data-stu-id="14f73-207">You may have to wait 5-10 minutes for the device to be ready.</span></span> <span data-ttu-id="14f73-208">Stavová zpráva se zobrazí v konzole informace o průběhu.</span><span class="sxs-lookup"><span data-stu-id="14f73-208">A status message is displayed on the console to indicate the progress.</span></span> <span data-ttu-id="14f73-209">Když jsou připravena, přejít na **akce**.</span><span class="sxs-lookup"><span data-stu-id="14f73-209">After the device is ready, go to **Action**.</span></span> <span data-ttu-id="14f73-210">Stiskněte klávesu `Ctrl + Alt + Delete` k přihlášení do virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="14f73-210">Press `Ctrl + Alt + Delete` to log in to the virtual array.</span></span> <span data-ttu-id="14f73-211">Výchozí uživatel se *StorSimpleAdmin* a výchozí heslo je *Heslo1*.</span><span class="sxs-lookup"><span data-stu-id="14f73-211">The default user is *StorSimpleAdmin* and the default password is *Password1*.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image23.png)
4. <span data-ttu-id="14f73-212">Z bezpečnostních důvodů vyprší platnost hesla správce zařízení při prvním přihlášení.</span><span class="sxs-lookup"><span data-stu-id="14f73-212">For security reasons, the device administrator password expires at the first logon.</span></span> <span data-ttu-id="14f73-213">Zobrazí se výzva ke změně hesla.</span><span class="sxs-lookup"><span data-stu-id="14f73-213">You are prompted to change the password.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image24.png)

   <span data-ttu-id="14f73-214">Zadejte heslo obsahující alespoň 8 znaků.</span><span class="sxs-lookup"><span data-stu-id="14f73-214">Enter a password that contains at least 8 characters.</span></span> <span data-ttu-id="14f73-215">Heslo musí splňovat minimálně 3 z následujících požadavků 4: velká písmena, malá písmena, číselné a speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="14f73-215">The password must satisfy at least 3 out of the following 4 requirements: uppercase, lowercase, numeric, and special characters.</span></span> <span data-ttu-id="14f73-216">Zadejte znovu heslo k potvrzení této akce.</span><span class="sxs-lookup"><span data-stu-id="14f73-216">Reenter the password to confirm it.</span></span> <span data-ttu-id="14f73-217">Zobrazí upozornění, že došlo ke změně hesla.</span><span class="sxs-lookup"><span data-stu-id="14f73-217">You are notified that the password has changed.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image25.png)
5. <span data-ttu-id="14f73-218">Po heslo úspěšně změněno, může restartovat virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="14f73-218">After the password is successfully changed, the virtual array may restart.</span></span> <span data-ttu-id="14f73-219">Počkejte, než zařízení spustit.</span><span class="sxs-lookup"><span data-stu-id="14f73-219">Wait for the device to start.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image26.png)

    <span data-ttu-id="14f73-220">Konzole prostředí Windows PowerShell pro zařízení se zobrazí spolu s indikátor průběhu.</span><span class="sxs-lookup"><span data-stu-id="14f73-220">The Windows PowerShell console of the device is displayed along with a progress bar.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image27.png)
6. <span data-ttu-id="14f73-221">Kroky 6 až 8 se projeví pouze při dalším spuštění v prostředí bez služby DHCP.</span><span class="sxs-lookup"><span data-stu-id="14f73-221">Steps 6-8 only apply when booting up in a non-DHCP environment.</span></span> <span data-ttu-id="14f73-222">Pokud jste v prostředí s DHCP, tyto kroky přeskočte a přejděte ke kroku 9.</span><span class="sxs-lookup"><span data-stu-id="14f73-222">If you are in a DHCP environment, then skip these steps and go to step 9.</span></span> <span data-ttu-id="14f73-223">Pokud je spouštěn nahoru zařízení v prostředí bez služby DHCP, zobrazí se následující obrazovka.</span><span class="sxs-lookup"><span data-stu-id="14f73-223">If you booted up your device in non-DHCP environment, you will see the following screen.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image28m.png)

    <span data-ttu-id="14f73-224">V dalším kroku Nakonfigurujte síť.</span><span class="sxs-lookup"><span data-stu-id="14f73-224">Next, configure the network.</span></span>
7. <span data-ttu-id="14f73-225">Použití `Get-HcsIpAddress` seznam rozhraní sítě povolené na vaše virtuální pole příkazu.</span><span class="sxs-lookup"><span data-stu-id="14f73-225">Use the `Get-HcsIpAddress` command to list the network interfaces enabled on your virtual array.</span></span> <span data-ttu-id="14f73-226">Pokud vaše zařízení má jedno síťové rozhraní povolena, je výchozí název, který je přiřazen k tomuto rozhraní `Ethernet`.</span><span class="sxs-lookup"><span data-stu-id="14f73-226">If your device has a single network interface enabled, the default name assigned to this interface is `Ethernet`.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image29m.png)
8. <span data-ttu-id="14f73-227">Použití `Set-HcsIpAddress` rutiny pro konfiguraci sítě.</span><span class="sxs-lookup"><span data-stu-id="14f73-227">Use the `Set-HcsIpAddress` cmdlet to configure the network.</span></span> <span data-ttu-id="14f73-228">Podívejte se na následující příklad:</span><span class="sxs-lookup"><span data-stu-id="14f73-228">See the following example:</span></span>

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image30.png)
9. <span data-ttu-id="14f73-229">Po dokončení počáteční nastavení a zařízení má spuštění nahoru, zobrazí se text hlavičky zařízení.</span><span class="sxs-lookup"><span data-stu-id="14f73-229">After the initial setup is complete and the device has booted up, you will see the device banner text.</span></span> <span data-ttu-id="14f73-230">Poznamenejte si IP adresu a adresu URL zobrazené v text hlavičky ke správě zařízení.</span><span class="sxs-lookup"><span data-stu-id="14f73-230">Make a note of the IP address and the URL displayed in the banner text to manage the device.</span></span> <span data-ttu-id="14f73-231">Použijte tuto IP adresu připojit k serveru webového uživatelského rozhraní vaše virtuální pole a dokončete místní instalaci a registraci.</span><span class="sxs-lookup"><span data-stu-id="14f73-231">Use this IP address to connect to the web UI of your virtual array and complete the local setup and registration.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image31m.png)
10. <span data-ttu-id="14f73-232">(Volitelné) Tento krok proveďte jenom v případě, že nasazujete zařízení v Cloud vlády.</span><span class="sxs-lookup"><span data-stu-id="14f73-232">(Optional) Perform this step only if you are deploying your device in the Government Cloud.</span></span> <span data-ttu-id="14f73-233">Nyní povolíte režimu Spojených států informace o zpracování Standard FIPS (Federal) ve vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="14f73-233">You will now enable the United States Federal Information Processing Standard (FIPS) mode on your device.</span></span> <span data-ttu-id="14f73-234">Standard FIPS 140 definuje kryptografické algoritmy pro použití schváleno nám Federal government počítačových systémů pro ochranu citlivá data.</span><span class="sxs-lookup"><span data-stu-id="14f73-234">The FIPS 140 standard defines cryptographic algorithms approved for use by US Federal government computer systems for the protection of sensitive data.</span></span>

    1. <span data-ttu-id="14f73-235">Pokud chcete povolit režim FIPS, spusťte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="14f73-235">To enable the FIPS mode, run the following cmdlet:</span></span>

        `Enable-HcsFIPSMode`
    2. <span data-ttu-id="14f73-236">Restartování zařízení po povolení režimu FIPS tak, aby kryptografických ověření vstoupí v platnost.</span><span class="sxs-lookup"><span data-stu-id="14f73-236">Reboot your device after you have enabled the FIPS mode so that the cryptographic validations take effect.</span></span>

       > [!NOTE]
       > <span data-ttu-id="14f73-237">Můžete povolit nebo zakázat režim FIPS ve vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="14f73-237">You can either enable or disable FIPS mode on your device.</span></span> <span data-ttu-id="14f73-238">Střídání zařízení mezi režim FIPS a standardu FIPS není podporována.</span><span class="sxs-lookup"><span data-stu-id="14f73-238">Alternating the device between FIPS and non-FIPS mode is not supported.</span></span>
       >
       >

<span data-ttu-id="14f73-239">Pokud zařízení nesplňuje minimální požadavky na konfiguraci, zobrazí následující chyba v text hlavičky (zobrazené dole).</span><span class="sxs-lookup"><span data-stu-id="14f73-239">If your device does not meet the minimum configuration requirements, you see the following error in the banner text (shown below).</span></span> <span data-ttu-id="14f73-240">Upravte konfiguraci zařízení tak, aby tento počítač nemá odpovídající prostředky pro splňují minimální požadavky.</span><span class="sxs-lookup"><span data-stu-id="14f73-240">Modify the device configuration so that the machine has adequate resources to meet the minimum requirements.</span></span> <span data-ttu-id="14f73-241">Potom můžete restartovat a připojte k zařízení.</span><span class="sxs-lookup"><span data-stu-id="14f73-241">You can then restart and connect to the device.</span></span> <span data-ttu-id="14f73-242">Přečtěte si požadavky minimální konfigurace v [krok 1: Ujistěte se, že hostitelský systém splňuje požadavky na minimální virtuální pole](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).</span><span class="sxs-lookup"><span data-stu-id="14f73-242">Refer to the minimum configuration requirements in [Step 1: Ensure that the host system meets minimum virtual array requirements](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).</span></span>

![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image32.png)

<span data-ttu-id="14f73-243">Pokud jste čelí chybě během počáteční konfigurace provedená prostřednictvím místního webového uživatelského rozhraní, podívejte se na následující pracovních postupů:</span><span class="sxs-lookup"><span data-stu-id="14f73-243">If you face any other error during the initial configuration using the local web UI, refer to the following workflows:</span></span>

* <span data-ttu-id="14f73-244">Spustit diagnostické testy na [řešení potíží s instalací webového uživatelského rozhraní](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).</span><span class="sxs-lookup"><span data-stu-id="14f73-244">Run diagnostic tests to [troubleshoot web UI setup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).</span></span>
* <span data-ttu-id="14f73-245">[Generovat balíček protokolu a prohlížení soubory protokolů](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span><span class="sxs-lookup"><span data-stu-id="14f73-245">[Generate log package and view log files](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span></span>

## <a name="next-steps"></a><span data-ttu-id="14f73-246">Další kroky</span><span class="sxs-lookup"><span data-stu-id="14f73-246">Next steps</span></span>
* [<span data-ttu-id="14f73-247">Nastavení pole virtuální zařízení StorSimple jako souborový server</span><span class="sxs-lookup"><span data-stu-id="14f73-247">Set up your StorSimple Virtual Array as a file server</span></span>](storsimple-virtual-array-deploy3-fs-setup.md)
* [<span data-ttu-id="14f73-248">Nastavení pole virtuální zařízení StorSimple jako serveru iSCSI</span><span class="sxs-lookup"><span data-stu-id="14f73-248">Set up your StorSimple Virtual Array as an iSCSI server</span></span>](storsimple-virtual-array-deploy3-iscsi-setup.md)
