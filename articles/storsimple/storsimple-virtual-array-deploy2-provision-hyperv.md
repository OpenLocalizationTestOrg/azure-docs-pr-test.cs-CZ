---
title: "aaaProvision pole virtuální zařízení StorSimple v Hyper-V | Microsoft Docs"
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
ms.openlocfilehash: f47d642f740827ae1440b819e07067c6a183527f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-hyper-v"></a><span data-ttu-id="a2eea-103">Nasazení zařízení StorSimple virtuální pole - zřízení technologie Hyper-v</span><span class="sxs-lookup"><span data-stu-id="a2eea-103">Deploy StorSimple Virtual Array - Provision in Hyper-V</span></span>
![](./media/storsimple-virtual-array-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a><span data-ttu-id="a2eea-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="a2eea-104">Overview</span></span>
<span data-ttu-id="a2eea-105">Tento kurz popisuje, jak tooprovision virtuální zařízení StorSimple pole v systému hostitele s technologií Hyper-V v systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="a2eea-105">This tutorial describes how tooprovision a StorSimple Virtual Array on a host system running Hyper-V on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2.</span></span> <span data-ttu-id="a2eea-106">Tento článek se týká nasazení toohello polí virtuální zařízení StorSimple v portálu Azure a cloudu Microsoft Azure Government.</span><span class="sxs-lookup"><span data-stu-id="a2eea-106">This article applies toohello deployment of StorSimple Virtual Arrays in Azure portal and Microsoft Azure Government Cloud.</span></span>

<span data-ttu-id="a2eea-107">Je třeba tooprovision oprávnění správce a nakonfigurovat virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="a2eea-107">You need administrator privileges tooprovision and configure a virtual array.</span></span> <span data-ttu-id="a2eea-108">Hello zřizování a počáteční instalace může trvat přibližně 10 minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="a2eea-108">hello provisioning and initial setup can take around 10 minutes toocomplete.</span></span>

## <a name="provisioning-prerequisites"></a><span data-ttu-id="a2eea-109">Zřizování požadavky</span><span class="sxs-lookup"><span data-stu-id="a2eea-109">Provisioning prerequisites</span></span>
<span data-ttu-id="a2eea-110">Zde najdete hello požadavky tooprovision virtuální pole v systému hostitele s technologií Hyper-V v systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="a2eea-110">Here you will find hello prerequisites tooprovision a virtual array on a host system running Hyper-V on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2.</span></span>

### <a name="for-hello-storsimple-device-manager-service"></a><span data-ttu-id="a2eea-111">Pro hello služby StorSimple Manager zařízení</span><span class="sxs-lookup"><span data-stu-id="a2eea-111">For hello StorSimple Device Manager service</span></span>
<span data-ttu-id="a2eea-112">Než začnete, ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="a2eea-112">Before you begin, make sure that:</span></span>

* <span data-ttu-id="a2eea-113">Jste dokončili všechny kroky hello v [Příprava hello portál pro pole virtuální zařízení StorSimple](storsimple-virtual-array-deploy1-portal-prep.md).</span><span class="sxs-lookup"><span data-stu-id="a2eea-113">You have completed all hello steps in [Prepare hello portal for StorSimple Virtual Array](storsimple-virtual-array-deploy1-portal-prep.md).</span></span>
* <span data-ttu-id="a2eea-114">Jste si stáhli hello virtuální pole image pro Hyper-V z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a2eea-114">You have downloaded hello virtual array image for Hyper-V from hello Azure portal.</span></span> <span data-ttu-id="a2eea-115">Další informace najdete v tématu **krok 3: stažení hello virtuální pole image** z [portál hello Příprava pro Průvodce pole virtuální zařízení StorSimple](storsimple-virtual-array-deploy1-portal-prep.md).</span><span class="sxs-lookup"><span data-stu-id="a2eea-115">For more information, see **Step 3: Download hello virtual array image** of [Prepare hello portal for StorSimple Virtual Array guide](storsimple-virtual-array-deploy1-portal-prep.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="a2eea-116">Hello software spuštěný na hello pole virtuální zařízení StorSimple lze použít pouze s hello služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="a2eea-116">hello software running on hello StorSimple Virtual Array may only be used with hello StorSimple Device Manager service.</span></span>
  >
  >

### <a name="for-hello-storsimple-virtual-array"></a><span data-ttu-id="a2eea-117">Pro hello pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="a2eea-117">For hello StorSimple Virtual Array</span></span>
<span data-ttu-id="a2eea-118">Před nasazením virtuální pole, ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="a2eea-118">Before you deploy a virtual array, make sure that:</span></span>

* <span data-ttu-id="a2eea-119">Máte přístup tooa hostitelský systém s Hyper-V v systému Windows Server 2008 R2 nebo novější, můžou být použité tooa zřídit zařízení.</span><span class="sxs-lookup"><span data-stu-id="a2eea-119">You have access tooa host system running Hyper-V on Windows Server 2008 R2 or later that can be used tooa provision a device.</span></span>
* <span data-ttu-id="a2eea-120">Hello systém hostitele je možné toodedicate hello následující prostředky tooprovision virtuální pole:</span><span class="sxs-lookup"><span data-stu-id="a2eea-120">hello host system is able toodedicate hello following resources tooprovision your virtual array:</span></span>

  * <span data-ttu-id="a2eea-121">Minimálně 4 jádra.</span><span class="sxs-lookup"><span data-stu-id="a2eea-121">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="a2eea-122">Alespoň 8 GB paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="a2eea-122">At least 8 GB of RAM.</span></span> <span data-ttu-id="a2eea-123">Pokud máte v plánu tooconfigure hello virtuální pole jako souborový server, 8 GB podporuje menší než 2 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="a2eea-123">If you plan tooconfigure hello virtual array as file server, 8 GB supports less than 2 million files.</span></span> <span data-ttu-id="a2eea-124">Je nutné 16 GB paměti RAM toosupport 2 – 4 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="a2eea-124">You need 16 GB RAM toosupport 2 - 4 million files.</span></span>
  * <span data-ttu-id="a2eea-125">Jedno síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a2eea-125">One network interface.</span></span>
  * <span data-ttu-id="a2eea-126">500 GB virtuální disk pro data.</span><span class="sxs-lookup"><span data-stu-id="a2eea-126">A 500 GB virtual disk for data.</span></span>

### <a name="for-hello-network-in-hello-datacenter"></a><span data-ttu-id="a2eea-127">Pro síť hello v datovém centru hello</span><span class="sxs-lookup"><span data-stu-id="a2eea-127">For hello network in hello datacenter</span></span>
<span data-ttu-id="a2eea-128">Než začnete, zkontrolujte hello sítě toodeploy požadavky pole virtuální zařízení StorSimple a nakonfigurujte síť datového centra hello správně.</span><span class="sxs-lookup"><span data-stu-id="a2eea-128">Before you begin, review hello networking requirements toodeploy a StorSimple Virtual Array and configure hello datacenter network appropriately.</span></span> <span data-ttu-id="a2eea-129">Další informace najdete v tématu [pole virtuální zařízení StorSimple sítě požadavky](storsimple-ova-system-requirements.md#networking-requirements).</span><span class="sxs-lookup"><span data-stu-id="a2eea-129">For more information, see [StorSimple Virtual Array networking requirements](storsimple-ova-system-requirements.md#networking-requirements).</span></span>

## <a name="step-by-step-provisioning"></a><span data-ttu-id="a2eea-130">Krok za krokem zřizování</span><span class="sxs-lookup"><span data-stu-id="a2eea-130">Step-by-step provisioning</span></span>
<span data-ttu-id="a2eea-131">tooprovision a připojit virtuální pole tooa, musíte tooperform hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a2eea-131">tooprovision and connect tooa virtual array, you need tooperform hello following steps:</span></span>

1. <span data-ttu-id="a2eea-132">Zajistěte, aby systém hostitele hello dostatek prostředků toomeet hello virtuální pole minimální požadavky.</span><span class="sxs-lookup"><span data-stu-id="a2eea-132">Ensure that hello host system has sufficient resources toomeet hello minimum virtual array requirements.</span></span>
2. <span data-ttu-id="a2eea-133">Zřídit virtuální pole ve vašem hypervisoru.</span><span class="sxs-lookup"><span data-stu-id="a2eea-133">Provision a virtual array in your hypervisor.</span></span>
3. <span data-ttu-id="a2eea-134">Spusťte virtuální pole hello a získat hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="a2eea-134">Start hello virtual array and get hello IP address.</span></span>

<span data-ttu-id="a2eea-135">Každý z těchto kroků je vysvětleno v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="a2eea-135">Each of these steps is explained in hello following sections.</span></span>

## <a name="step-1-ensure-that-hello-host-system-meets-minimum-virtual-array-requirements"></a><span data-ttu-id="a2eea-136">Krok 1: Zkontrolujte, zda hello hostitelský systém splňuje požadavky na minimální virtuální pole</span><span class="sxs-lookup"><span data-stu-id="a2eea-136">Step 1: Ensure that hello host system meets minimum virtual array requirements</span></span>
<span data-ttu-id="a2eea-137">toocreate virtuální pole, je třeba:</span><span class="sxs-lookup"><span data-stu-id="a2eea-137">toocreate a virtual array, you need:</span></span>

* <span data-ttu-id="a2eea-138">role Hello technologie Hyper-V nainstalovaná v systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="a2eea-138">hello Hyper-V role installed on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2 SP1.</span></span>
* <span data-ttu-id="a2eea-139">Microsoft Hyper-V Manager na klienta se systémem Microsoft Windows připojené toohello hostitele.</span><span class="sxs-lookup"><span data-stu-id="a2eea-139">Microsoft Hyper-V Manager on a Microsoft Windows client connected toohello host.</span></span>

<span data-ttu-id="a2eea-140">Zajistěte, aby byl tento hello základní hardware (hostitelský systém), na kterém vytváříte virtuální pole hello možné toodedicate hello následující prostředky tooyour virtuální pole:</span><span class="sxs-lookup"><span data-stu-id="a2eea-140">Make sure that hello underlying hardware (host system) on which you are creating hello virtual array is able toodedicate hello following resources tooyour virtual array:</span></span>

* <span data-ttu-id="a2eea-141">Minimálně 4 jádra.</span><span class="sxs-lookup"><span data-stu-id="a2eea-141">A minimum of 4 cores.</span></span>
* <span data-ttu-id="a2eea-142">Alespoň 8 GB paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="a2eea-142">At least 8 GB of RAM.</span></span> <span data-ttu-id="a2eea-143">Pokud máte v plánu tooconfigure hello virtuální pole jako souborový server, 8 GB podporuje menší než 2 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="a2eea-143">If you plan tooconfigure hello virtual array as file server, 8 GB supports less than 2 million files.</span></span> <span data-ttu-id="a2eea-144">Je nutné 16 GB paměti RAM toosupport 2 – 4 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="a2eea-144">You need 16 GB RAM toosupport 2 - 4 million files.</span></span>
* <span data-ttu-id="a2eea-145">Jedno síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a2eea-145">One network interface.</span></span>
* <span data-ttu-id="a2eea-146">500 GB virtuální disk pro data systému.</span><span class="sxs-lookup"><span data-stu-id="a2eea-146">A 500 GB virtual disk for system data.</span></span>

## <a name="step-2-provision-a-virtual-array-in-hypervisor"></a><span data-ttu-id="a2eea-147">Krok 2: Zřídit virtuální pole v hypervisoru</span><span class="sxs-lookup"><span data-stu-id="a2eea-147">Step 2: Provision a virtual array in hypervisor</span></span>
<span data-ttu-id="a2eea-148">Proveďte následující kroky tooprovision zařízení ve vaší hypervisoru hello.</span><span class="sxs-lookup"><span data-stu-id="a2eea-148">Perform hello following steps tooprovision a device in your hypervisor.</span></span>

#### <a name="tooprovision-a-virtual-array"></a><span data-ttu-id="a2eea-149">tooprovision virtuální pole</span><span class="sxs-lookup"><span data-stu-id="a2eea-149">tooprovision a virtual array</span></span>
1. <span data-ttu-id="a2eea-150">Na hostiteli s Windows Server zkopírujte hello virtuální pole image tooa místní disk.</span><span class="sxs-lookup"><span data-stu-id="a2eea-150">On your Windows Server host, copy hello virtual array image tooa local drive.</span></span> <span data-ttu-id="a2eea-151">Jste si stáhli tuto bitovou kopii (VHD nebo VHDX) prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a2eea-151">You downloaded this image (VHD or VHDX) through hello Azure portal.</span></span> <span data-ttu-id="a2eea-152">Poznamenejte si hello umístění, kam jste zkopírovali hello image jako později v postupu hello používáte tuto bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="a2eea-152">Make a note of hello location where you copied hello image as you are using this image later in hello procedure.</span></span>
2. <span data-ttu-id="a2eea-153">Otevřete **správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-153">Open **Server Manager**.</span></span> <span data-ttu-id="a2eea-154">V hello pravém horním rohu klikněte na **nástroje** a vyberte **Správce technologie Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-154">In hello top right corner, click **Tools** and select **Hyper-V Manager**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image1.png)  

   <span data-ttu-id="a2eea-155">Pokud používáte Windows Server 2008 R2, otevřete hello Správce technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="a2eea-155">If you are running Windows Server 2008 R2, open hello Hyper-V Manager.</span></span> <span data-ttu-id="a2eea-156">Ve Správci serveru klikněte na tlačítko **role > Hyper-V > Správce technologie Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-156">In Server Manager, click **Roles > Hyper-V > Hyper-V Manager**.</span></span>
3. <span data-ttu-id="a2eea-157">V **Správce technologie Hyper-V**, v podokně hello oboru, klikněte pravým tlačítkem na váš systém uzlu tooopen hello kontextové nabídky a pak klikněte na tlačítko **nový** > **virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-157">In **Hyper-V Manager**, in hello scope pane, right-click your system node tooopen hello context menu, and then click **New** > **Virtual Machine**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image2.png)
4. <span data-ttu-id="a2eea-158">Na hello **před zahájením** stránku hello Průvodce novým virtuálním počítačem, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-158">On hello **Before you begin** page of hello New Virtual Machine Wizard, click **Next**.</span></span>
5. <span data-ttu-id="a2eea-159">Na hello **zadejte název a umístění** zadejte **název** pro vaše virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="a2eea-159">On hello **Specify name and location** page, provide a **Name** for your virtual array.</span></span> <span data-ttu-id="a2eea-160">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-160">Click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image4.png)
6. <span data-ttu-id="a2eea-161">Na hello **určit generaci** stránky, vyberte typ image hello zařízení a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-161">On hello **Specify generation** page, choose hello device image type, and then click **Next**.</span></span> <span data-ttu-id="a2eea-162">Tato stránka se nezobrazí, pokud používáte Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="a2eea-162">This page doesn't appear if you're using Windows Server 2008 R2.</span></span>

   * <span data-ttu-id="a2eea-163">Zvolte **generace 2** Pokud jste stáhli bitovou kopii .vhdx pro Windows Server 2012 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a2eea-163">Choose **Generation 2** if you downloaded a .vhdx image for Windows Server 2012 or later.</span></span>
   * <span data-ttu-id="a2eea-164">Zvolte **generace 1** Pokud jste stáhli VHD image pro Windows Server 2008 R2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a2eea-164">Choose **Generation 1** if you downloaded a .vhd image for Windows Server 2008 R2 or later.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image5.png)
7. <span data-ttu-id="a2eea-165">Na hello **přiřazení paměti** stránky, zadejte **spouštěcí paměť** z alespoň **8192 MB**, nemáte povolte dynamickou paměť a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-165">On hello **Assign memory** page, specify a **Startup memory** of at least **8192 MB**, don't enable dynamic memory, and then click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image6.png)  
8. <span data-ttu-id="a2eea-166">Na hello **konfigurace sítě** zadejte hello virtuální přepínač, který je připojený toohello Internetu a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-166">On hello **Configure networking** page, specify hello virtual switch that is connected toohello Internet and then click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image7.png)
9. <span data-ttu-id="a2eea-167">Na hello **připojit virtuální pevný disk** vyberte **použít existující virtuální pevný disk**, zadejte umístění hello hello virtuální pole obrázku (vhdx nebo VHD) a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-167">On hello **Connect virtual hard disk** page, choose **Use an existing virtual hard disk**, specify hello location of hello virtual array image (.vhdx or .vhd), and then click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image8m.png)
10. <span data-ttu-id="a2eea-168">Zkontrolujte hello **Souhrn** a pak klikněte na **Dokončit** toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2eea-168">Review hello **Summary** and then click **Finish** toocreate hello virtual machine.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image9.png)
11. <span data-ttu-id="a2eea-169">toomeet hello minimální požadavky, je nutné 4 jádra.</span><span class="sxs-lookup"><span data-stu-id="a2eea-169">toomeet hello minimum requirements, you need 4 cores.</span></span> <span data-ttu-id="a2eea-170">tooadd 4 virtuální procesory, vyberte hostitelský systém v hello **Správce technologie Hyper-V** okno.</span><span class="sxs-lookup"><span data-stu-id="a2eea-170">tooadd 4 virtual processors, select your host system in hello **Hyper-V Manager** window.</span></span> <span data-ttu-id="a2eea-171">V pravém podokně hello pod hello seznam **virtuální počítače**, vyhledejte hello virtuálního počítače, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a2eea-171">In hello right-pane under hello list of **Virtual Machines**, locate hello virtual machine you just created.</span></span> <span data-ttu-id="a2eea-172">Vyberte a klikněte pravým tlačítkem na název počítače hello a vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-172">Select and right-click hello machine name and select **Settings**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image10.png)
12. <span data-ttu-id="a2eea-173">Na hello **nastavení** klepněte ve hello levém podokně, **procesoru**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-173">On hello **Settings** page, in hello left-pane, click **Processor**.</span></span> <span data-ttu-id="a2eea-174">V pravém hello podokně nastavte **počet virtuálních procesorů** too4 (nebo více).</span><span class="sxs-lookup"><span data-stu-id="a2eea-174">In hello right-pane, set **number of virtual processors** too4 (or more).</span></span> <span data-ttu-id="a2eea-175">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-175">Click **Apply**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image11.png)
13. <span data-ttu-id="a2eea-176">toomeet hello minimální požadavky, musíte taky tooadd 500 GB virtuální datový disk.</span><span class="sxs-lookup"><span data-stu-id="a2eea-176">toomeet hello minimum requirements, you also need tooadd a 500 GB virtual data disk.</span></span> <span data-ttu-id="a2eea-177">V hello **nastavení** stránky:</span><span class="sxs-lookup"><span data-stu-id="a2eea-177">In hello **Settings** page:</span></span>

    1. <span data-ttu-id="a2eea-178">V levém podokně hello vyberte **řadič SCSI**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-178">In hello left pane, select **SCSI Controller**.</span></span>
    2. <span data-ttu-id="a2eea-179">V pravém podokně hello vyberte **pevný disk,** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-179">In hello right pane, select **Hard Drive,** and click **Add**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image12.png)
14. <span data-ttu-id="a2eea-180">Na hello **pevný disk** stránky, vyberte hello **virtuální pevný disk** možnost a klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-180">On hello **Hard drive** page, select hello **Virtual hard disk** option and click **New**.</span></span> <span data-ttu-id="a2eea-181">Hello **Průvodce vytvořením virtuálního pevného disku** spustí.</span><span class="sxs-lookup"><span data-stu-id="a2eea-181">hello **New Virtual Hard Disk Wizard** starts.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image13.png)
15. <span data-ttu-id="a2eea-182">Na hello **před zahájením** stránku hello Průvodce vytvořením virtuálního pevného disku, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-182">On hello **Before you begin** page of hello New Virtual Hard Disk Wizard, click **Next**.</span></span>
16. <span data-ttu-id="a2eea-183">Na hello **stránky výběr formátu disku**, přijměte výchozí možnost hello **VHDX** formátu.</span><span class="sxs-lookup"><span data-stu-id="a2eea-183">On hello **Choose Disk Format page**, accept hello default option of **VHDX** format.</span></span> <span data-ttu-id="a2eea-184">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-184">Click **Next**.</span></span> <span data-ttu-id="a2eea-185">Tato obrazovka není uveden, pokud systém Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="a2eea-185">This screen is not presented if running Windows Server 2008 R2.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image15.png)
17. <span data-ttu-id="a2eea-186">Na hello **zvolit typ disku stránky**, nastavte typ virtuálního pevného disku jako **dynamicky se zvětšující** (doporučeno).</span><span class="sxs-lookup"><span data-stu-id="a2eea-186">On hello **Choose Disk Type page**, set virtual hard disk type as **Dynamically expanding** (recommended).</span></span> <span data-ttu-id="a2eea-187">**Pevná velikost** disk by pracovat, ale může být nutné toowait dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="a2eea-187">**Fixed size** disk would work but you may need toowait a long time.</span></span> <span data-ttu-id="a2eea-188">Doporučujeme, abyste nepoužívejte hello **Differencing** možnost.</span><span class="sxs-lookup"><span data-stu-id="a2eea-188">We recommend that you do not use hello **Differencing** option.</span></span> <span data-ttu-id="a2eea-189">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-189">Click **Next**.</span></span> <span data-ttu-id="a2eea-190">V systému Windows Server 2012 R2 a Windows Server 2012 **dynamicky se zvětšující** je hello výchozí možnost, zatímco ve Windows serveru 2008 R2, je výchozí hello **pevnou velikost**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-190">In Windows Server 2012 R2 and Windows Server 2012, **Dynamically expanding** is hello default option whereas in Windows Server 2008 R2, hello default is **Fixed size**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image16.png)
18. <span data-ttu-id="a2eea-191">Na hello **zadat název a umístění** zadejte **název** a také **umístění** (můžete procházet tooone) pro hello datový disk.</span><span class="sxs-lookup"><span data-stu-id="a2eea-191">On hello **Specify Name and Location** page, provide a **name** as well as **location** (you can browse tooone) for hello data disk.</span></span> <span data-ttu-id="a2eea-192">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-192">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image17.png)
19. <span data-ttu-id="a2eea-193">Na hello **konfigurovat Disk** stránky, vyberte hello možnost **vytvořit nový prázdný virtuální pevný disk** a zadejte velikost hello jako **500 GB** (nebo více).</span><span class="sxs-lookup"><span data-stu-id="a2eea-193">On hello **Configure Disk** page, select hello option **Create a new blank virtual hard disk** and specify hello size as **500 GB** (or more).</span></span> <span data-ttu-id="a2eea-194">Zatímco 500 GB je minimální požadavek hello, můžete zřídit vždy větší disku.</span><span class="sxs-lookup"><span data-stu-id="a2eea-194">While 500 GB is hello minimum requirement, you can always provision a larger disk.</span></span> <span data-ttu-id="a2eea-195">Všimněte si, že nelze rozbalit nebo zmenšení disku hello jednou zřízený.</span><span class="sxs-lookup"><span data-stu-id="a2eea-195">Note that you cannot expand or shrink hello disk once provisioned.</span></span> <span data-ttu-id="a2eea-196">Další informace o hello velikost disku tooprovision, projděte si část nastavení velikosti hello v hello [osvědčené postupy dokumentu](storsimple-ova-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="a2eea-196">For more information on hello size of disk tooprovision, review hello sizing section in hello [best practices document](storsimple-ova-best-practices.md).</span></span> <span data-ttu-id="a2eea-197">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-197">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image18.png)
20. <span data-ttu-id="a2eea-198">Na hello **Souhrn** , zkontrolujte podrobnosti hello data virtuálního disku a pokud splnit, klikněte na tlačítko **Dokončit** toocreate hello disku.</span><span class="sxs-lookup"><span data-stu-id="a2eea-198">On hello **Summary** page, review hello details of your virtual data disk and if satisfied, click **Finish** toocreate hello disk.</span></span> <span data-ttu-id="a2eea-199">Hello Průvodce zavře, virtuální pevný disk je přidat tooyour počítače.</span><span class="sxs-lookup"><span data-stu-id="a2eea-199">hello wizard closes and a virtual hard disk is added tooyour machine.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image19.png)
21. <span data-ttu-id="a2eea-200">Vrátí toohello **nastavení** stránky.</span><span class="sxs-lookup"><span data-stu-id="a2eea-200">Return toohello **Settings** page.</span></span> <span data-ttu-id="a2eea-201">Klikněte na tlačítko **OK** tooclose hello **nastavení** stránku a vrátit okno Správce tooHyper-V.</span><span class="sxs-lookup"><span data-stu-id="a2eea-201">Click **OK** tooclose hello **Settings** page and return tooHyper-V Manager window.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-hello-virtual-array-and-get-hello-ip"></a><span data-ttu-id="a2eea-202">Krok 3: Spuštění virtuální pole hello a získat hello IP</span><span class="sxs-lookup"><span data-stu-id="a2eea-202">Step 3: Start hello virtual array and get hello IP</span></span>
<span data-ttu-id="a2eea-203">Proveďte následující kroky toostart hello virtuální pole a připojit tooit.</span><span class="sxs-lookup"><span data-stu-id="a2eea-203">Perform hello following steps toostart your virtual array and connect tooit.</span></span>

#### <a name="toostart-hello-virtual-array"></a><span data-ttu-id="a2eea-204">toostart hello virtuální pole</span><span class="sxs-lookup"><span data-stu-id="a2eea-204">toostart hello virtual array</span></span>
1. <span data-ttu-id="a2eea-205">Spusťte virtuální pole hello.</span><span class="sxs-lookup"><span data-stu-id="a2eea-205">Start hello virtual array.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image21.png)
2. <span data-ttu-id="a2eea-206">Po hello zařízení běží, vyberte hello zařízení, klikněte pravým tlačítkem a vyberte **Connect**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-206">After hello device is running, select hello device, right click, and select **Connect**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image22.png)
3. <span data-ttu-id="a2eea-207">Můžete mít toowait 5 až 10 minut pro hello zařízení toobe připraven.</span><span class="sxs-lookup"><span data-stu-id="a2eea-207">You may have toowait 5-10 minutes for hello device toobe ready.</span></span> <span data-ttu-id="a2eea-208">Stavová zpráva se zobrazí na hello konzoly tooindicate hello průběh.</span><span class="sxs-lookup"><span data-stu-id="a2eea-208">A status message is displayed on hello console tooindicate hello progress.</span></span> <span data-ttu-id="a2eea-209">Poté, co je připraven hello zařízení, se příliš**akce**.</span><span class="sxs-lookup"><span data-stu-id="a2eea-209">After hello device is ready, go too**Action**.</span></span> <span data-ttu-id="a2eea-210">Stiskněte klávesu `Ctrl + Alt + Delete` toolog v toohello virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="a2eea-210">Press `Ctrl + Alt + Delete` toolog in toohello virtual array.</span></span> <span data-ttu-id="a2eea-211">Výchozí uživatel Hello *StorSimpleAdmin* a hello výchozí heslo je *Heslo1*.</span><span class="sxs-lookup"><span data-stu-id="a2eea-211">hello default user is *StorSimpleAdmin* and hello default password is *Password1*.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image23.png)
4. <span data-ttu-id="a2eea-212">Z bezpečnostních důvodů vyprší platnost hesla správce zařízení hello při prvním přihlášení hello.</span><span class="sxs-lookup"><span data-stu-id="a2eea-212">For security reasons, hello device administrator password expires at hello first logon.</span></span> <span data-ttu-id="a2eea-213">Jste výzvami toochange hello heslo.</span><span class="sxs-lookup"><span data-stu-id="a2eea-213">You are prompted toochange hello password.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image24.png)

   <span data-ttu-id="a2eea-214">Zadejte heslo obsahující alespoň 8 znaků.</span><span class="sxs-lookup"><span data-stu-id="a2eea-214">Enter a password that contains at least 8 characters.</span></span> <span data-ttu-id="a2eea-215">Hello heslo musí splňovat minimálně 3 z následujících 4 požadavky hello: velká písmena, malá písmena, číselné a speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="a2eea-215">hello password must satisfy at least 3 out of hello following 4 requirements: uppercase, lowercase, numeric, and special characters.</span></span> <span data-ttu-id="a2eea-216">Zadejte znovu heslo tooconfirm hello ho.</span><span class="sxs-lookup"><span data-stu-id="a2eea-216">Reenter hello password tooconfirm it.</span></span> <span data-ttu-id="a2eea-217">Zobrazí se upozornění, že došlo ke změně toto heslo hello.</span><span class="sxs-lookup"><span data-stu-id="a2eea-217">You are notified that hello password has changed.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image25.png)
5. <span data-ttu-id="a2eea-218">Po hello heslo je úspěšně změněno, může restartovat virtuální pole hello.</span><span class="sxs-lookup"><span data-stu-id="a2eea-218">After hello password is successfully changed, hello virtual array may restart.</span></span> <span data-ttu-id="a2eea-219">Počkejte toostart hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="a2eea-219">Wait for hello device toostart.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image26.png)

    <span data-ttu-id="a2eea-220">konzole Windows PowerShell Hello hello zařízení se zobrazí spolu s indikátor průběhu.</span><span class="sxs-lookup"><span data-stu-id="a2eea-220">hello Windows PowerShell console of hello device is displayed along with a progress bar.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image27.png)
6. <span data-ttu-id="a2eea-221">Kroky 6 až 8 se projeví pouze při dalším spuštění v prostředí bez služby DHCP.</span><span class="sxs-lookup"><span data-stu-id="a2eea-221">Steps 6-8 only apply when booting up in a non-DHCP environment.</span></span> <span data-ttu-id="a2eea-222">Pokud jste v prostředí s DHCP, pak Přeskočit tyto kroky a přejít toostep 9.</span><span class="sxs-lookup"><span data-stu-id="a2eea-222">If you are in a DHCP environment, then skip these steps and go toostep 9.</span></span> <span data-ttu-id="a2eea-223">Pokud je spouštěn nahoru zařízení v prostředí bez služby DHCP, zobrazí se následující obrazovka hello.</span><span class="sxs-lookup"><span data-stu-id="a2eea-223">If you booted up your device in non-DHCP environment, you will see hello following screen.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image28m.png)

    <span data-ttu-id="a2eea-224">V dalším kroku nakonfigurujte hello sítě.</span><span class="sxs-lookup"><span data-stu-id="a2eea-224">Next, configure hello network.</span></span>
7. <span data-ttu-id="a2eea-225">Použití hello `Get-HcsIpAddress` příkaz toolist hello síťových rozhraní na vaše virtuální pole povolené.</span><span class="sxs-lookup"><span data-stu-id="a2eea-225">Use hello `Get-HcsIpAddress` command toolist hello network interfaces enabled on your virtual array.</span></span> <span data-ttu-id="a2eea-226">Pokud vaše zařízení má jedno síťové rozhraní povolena, je hello výchozí název přiřazený toothis rozhraní `Ethernet`.</span><span class="sxs-lookup"><span data-stu-id="a2eea-226">If your device has a single network interface enabled, hello default name assigned toothis interface is `Ethernet`.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image29m.png)
8. <span data-ttu-id="a2eea-227">Použití hello `Set-HcsIpAddress` rutiny tooconfigure hello sítě.</span><span class="sxs-lookup"><span data-stu-id="a2eea-227">Use hello `Set-HcsIpAddress` cmdlet tooconfigure hello network.</span></span> <span data-ttu-id="a2eea-228">Viz následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="a2eea-228">See hello following example:</span></span>

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image30.png)
9. <span data-ttu-id="a2eea-229">Po dokončení počáteční nastavení hello a hello zařízení má spuštění nahoru, zobrazí se text hlavičky hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="a2eea-229">After hello initial setup is complete and hello device has booted up, you will see hello device banner text.</span></span> <span data-ttu-id="a2eea-230">Poznamenejte si hello IP adresu a adresu URL hello zobrazené v hello banner text toomanage hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="a2eea-230">Make a note of hello IP address and hello URL displayed in hello banner text toomanage hello device.</span></span> <span data-ttu-id="a2eea-231">Použijte tuto IP adresu tooconnect toohello webového uživatelského rozhraní virtuální pole a dokončení hello místní instalaci a registraci.</span><span class="sxs-lookup"><span data-stu-id="a2eea-231">Use this IP address tooconnect toohello web UI of your virtual array and complete hello local setup and registration.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image31m.png)
10. <span data-ttu-id="a2eea-232">(Volitelné) Tento krok proveďte jenom v případě, že nasazujete zařízení v hello Cloud vlády.</span><span class="sxs-lookup"><span data-stu-id="a2eea-232">(Optional) Perform this step only if you are deploying your device in hello Government Cloud.</span></span> <span data-ttu-id="a2eea-233">Nyní povolíte hello Spojených států informace o zpracování Standard FIPS (Federal) režim na zařízení.</span><span class="sxs-lookup"><span data-stu-id="a2eea-233">You will now enable hello United States Federal Information Processing Standard (FIPS) mode on your device.</span></span> <span data-ttu-id="a2eea-234">standard Hello FIPS 140 definuje kryptografické algoritmy pro použití schváleno nám Federal government počítačových systémů hello ochranu citlivá data.</span><span class="sxs-lookup"><span data-stu-id="a2eea-234">hello FIPS 140 standard defines cryptographic algorithms approved for use by US Federal government computer systems for hello protection of sensitive data.</span></span>

    1. <span data-ttu-id="a2eea-235">tooenable hello režimu FIPS, spusťte následující rutinu hello:</span><span class="sxs-lookup"><span data-stu-id="a2eea-235">tooenable hello FIPS mode, run hello following cmdlet:</span></span>

        `Enable-HcsFIPSMode`
    2. <span data-ttu-id="a2eea-236">Restartování zařízení po povolení režimu FIPS hello tak, aby ověření kryptografických hello vstoupí v platnost.</span><span class="sxs-lookup"><span data-stu-id="a2eea-236">Reboot your device after you have enabled hello FIPS mode so that hello cryptographic validations take effect.</span></span>

       > [!NOTE]
       > <span data-ttu-id="a2eea-237">Můžete povolit nebo zakázat režim FIPS ve vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="a2eea-237">You can either enable or disable FIPS mode on your device.</span></span> <span data-ttu-id="a2eea-238">Střídání hello zařízení mezi režim FIPS a standardu FIPS není podporováno.</span><span class="sxs-lookup"><span data-stu-id="a2eea-238">Alternating hello device between FIPS and non-FIPS mode is not supported.</span></span>
       >
       >

<span data-ttu-id="a2eea-239">Pokud zařízení nesplňuje hello minimální požadavky na konfiguraci, zobrazí hello následující došlo k chybě v text hlavičky hello (zobrazené dole).</span><span class="sxs-lookup"><span data-stu-id="a2eea-239">If your device does not meet hello minimum configuration requirements, you see hello following error in hello banner text (shown below).</span></span> <span data-ttu-id="a2eea-240">Upravte konfiguraci zařízení hello tak, aby hello počítač má odpovídající zdroje toomeet hello minimální požadavky.</span><span class="sxs-lookup"><span data-stu-id="a2eea-240">Modify hello device configuration so that hello machine has adequate resources toomeet hello minimum requirements.</span></span> <span data-ttu-id="a2eea-241">Potom můžete restartovat a toohello zařízení připojit.</span><span class="sxs-lookup"><span data-stu-id="a2eea-241">You can then restart and connect toohello device.</span></span> <span data-ttu-id="a2eea-242">Odkazovat toohello minimální požadavky na konfiguraci v [krok 1: Zkontrolujte, zda hello hostitelský systém splňuje požadavky na minimální virtuální pole](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).</span><span class="sxs-lookup"><span data-stu-id="a2eea-242">Refer toohello minimum configuration requirements in [Step 1: Ensure that hello host system meets minimum virtual array requirements](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).</span></span>

![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image32.png)

<span data-ttu-id="a2eea-243">Pokud potýkáte chybě během počáteční konfigurace hello pomocí hello místního webového uživatelského rozhraní, podívejte se na toohello následující pracovních postupů:</span><span class="sxs-lookup"><span data-stu-id="a2eea-243">If you face any other error during hello initial configuration using hello local web UI, refer toohello following workflows:</span></span>

* <span data-ttu-id="a2eea-244">Spustit diagnostické testy příliš[řešení potíží s instalací webového uživatelského rozhraní](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).</span><span class="sxs-lookup"><span data-stu-id="a2eea-244">Run diagnostic tests too[troubleshoot web UI setup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).</span></span>
* <span data-ttu-id="a2eea-245">[Generovat balíček protokolu a prohlížení soubory protokolů](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span><span class="sxs-lookup"><span data-stu-id="a2eea-245">[Generate log package and view log files](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a2eea-246">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a2eea-246">Next steps</span></span>
* [<span data-ttu-id="a2eea-247">Nastavení pole virtuální zařízení StorSimple jako souborový server</span><span class="sxs-lookup"><span data-stu-id="a2eea-247">Set up your StorSimple Virtual Array as a file server</span></span>](storsimple-virtual-array-deploy3-fs-setup.md)
* [<span data-ttu-id="a2eea-248">Nastavení pole virtuální zařízení StorSimple jako serveru iSCSI</span><span class="sxs-lookup"><span data-stu-id="a2eea-248">Set up your StorSimple Virtual Array as an iSCSI server</span></span>](storsimple-virtual-array-deploy3-iscsi-setup.md)
