---
title: "Zřídit pole virtuální zařízení StorSimple ve službě VMware | Microsoft Docs"
description: "V tomto kurzu druhé pole virtuální zařízení StorSimple řady nasazení zahrnuje zřizování virtuálního zařízení ve službě VMware."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 0425b2a9-d36f-433d-8131-ee0cacef95f8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 118521a127b2e4b765efabdbdde71605440d81c7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-vmware"></a><span data-ttu-id="47a8f-103">Nasazení zařízení StorSimple virtuální pole - zřídit ve službě VMware</span><span class="sxs-lookup"><span data-stu-id="47a8f-103">Deploy StorSimple Virtual Array - Provision in VMware</span></span>
![](./media/storsimple-virtual-array-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a><span data-ttu-id="47a8f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="47a8f-104">Overview</span></span>
<span data-ttu-id="47a8f-105">Tento kurz popisuje, jak zřídit a připojte se k poli virtuální zařízení StorSimple v hostitelském systému, systémem VMware ESXi 5.5 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="47a8f-105">This tutorial describes how to provision and connect to a StorSimple Virtual Array on a host system running VMware ESXi 5.5 and above.</span></span> <span data-ttu-id="47a8f-106">Tento článek se týká nasazení pole virtuální zařízení StorSimple v portálu Azure a cloudu Microsoft Azure Government.</span><span class="sxs-lookup"><span data-stu-id="47a8f-106">This article applies to the deployment of StorSimple Virtual Arrays in Azure portal and the Microsoft Azure Government Cloud.</span></span>

<span data-ttu-id="47a8f-107">Potřebovat oprávnění správce k poskytování a připojte se k virtuálnímu zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a8f-107">You need administrator privileges to provision and connect to a virtual device.</span></span> <span data-ttu-id="47a8f-108">Zřizování a počáteční instalace může trvat přibližně 10 minut.</span><span class="sxs-lookup"><span data-stu-id="47a8f-108">The provisioning and initial setup can take around 10 minutes to complete.</span></span>

## <a name="provisioning-prerequisites"></a><span data-ttu-id="47a8f-109">Zřizování požadavky</span><span class="sxs-lookup"><span data-stu-id="47a8f-109">Provisioning prerequisites</span></span>
<span data-ttu-id="47a8f-110">Požadavky pro zřízení virtuálního zařízení v hostitelském systému, systémem VMware ESXi 5.5 a vyšší, jsou následující.</span><span class="sxs-lookup"><span data-stu-id="47a8f-110">The prerequisites to provision a virtual device on a host system running VMware ESXi 5.5 and above, are as follows.</span></span>

### <a name="for-the-storsimple-device-manager-service"></a><span data-ttu-id="47a8f-111">Služba Správce zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="47a8f-111">For the StorSimple Device Manager service</span></span>
<span data-ttu-id="47a8f-112">Než začnete, ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="47a8f-112">Before you begin, make sure that:</span></span>

* <span data-ttu-id="47a8f-113">Jste dokončili všechny kroky v [připravit na portálu pro pole virtuální zařízení StorSimple](storsimple-virtual-array-deploy1-portal-prep.md).</span><span class="sxs-lookup"><span data-stu-id="47a8f-113">You have completed all the steps in [Prepare the portal for StorSimple Virtual Array](storsimple-virtual-array-deploy1-portal-prep.md).</span></span>
* <span data-ttu-id="47a8f-114">Bitovou kopii virtuálního zařízení pro VMware jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="47a8f-114">You have downloaded the virtual device image for VMware from the Azure portal.</span></span> <span data-ttu-id="47a8f-115">Další informace najdete v tématu **krok 3: stáhnout bitovou kopii virtuálního zařízení** z [připravit na portálu pro Průvodce pole virtuální zařízení StorSimple](storsimple-virtual-array-deploy1-portal-prep.md).</span><span class="sxs-lookup"><span data-stu-id="47a8f-115">For more information, see **Step 3: Download the virtual device image** of [Prepare the portal for StorSimple Virtual Array guide](storsimple-virtual-array-deploy1-portal-prep.md).</span></span>

### <a name="for-the-storsimple-virtual-device"></a><span data-ttu-id="47a8f-116">Pro virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="47a8f-116">For the StorSimple virtual device</span></span>
<span data-ttu-id="47a8f-117">Před nasazením virtuálního zařízení, ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="47a8f-117">Before you deploy a virtual device, make sure that:</span></span>

* <span data-ttu-id="47a8f-118">Máte přístup k systému hostitele s technologií Hyper-V (2008 R2 nebo novější), lze použít k zřízení zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a8f-118">You have access to a host system running Hyper-V (2008 R2 or later) that can be used to a provision a device.</span></span>
* <span data-ttu-id="47a8f-119">Systém hostitele je možné vyhradit následující prostředky pro zřízení virtuálního zařízení:</span><span class="sxs-lookup"><span data-stu-id="47a8f-119">The host system is able to dedicate the following resources to provision your virtual device:</span></span>

  * <span data-ttu-id="47a8f-120">Minimálně 4 jádra.</span><span class="sxs-lookup"><span data-stu-id="47a8f-120">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="47a8f-121">Alespoň 8 GB paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="47a8f-121">At least 8 GB of RAM.</span></span> <span data-ttu-id="47a8f-122">Pokud chcete konfigurovat virtuální pole jako souborový server, 8 GB podporuje menší než 2 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="47a8f-122">If you plan to configure the virtual array as file server, 8 GB supports less than 2 million files.</span></span> <span data-ttu-id="47a8f-123">Je nutné 16 GB paměti RAM pro podporu 2 – 4 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="47a8f-123">You need 16 GB RAM to support 2 - 4 million files.</span></span>
  * <span data-ttu-id="47a8f-124">Jedno síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="47a8f-124">One network interface.</span></span>
  * <span data-ttu-id="47a8f-125">500 GB virtuální disk pro data systému.</span><span class="sxs-lookup"><span data-stu-id="47a8f-125">A 500 GB virtual disk for system data.</span></span>

### <a name="for-the-network-in-datacenter"></a><span data-ttu-id="47a8f-126">Pro síť datového centra.</span><span class="sxs-lookup"><span data-stu-id="47a8f-126">For the network in datacenter</span></span>
<span data-ttu-id="47a8f-127">Než začnete, ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="47a8f-127">Before you begin, make sure that:</span></span>

* <span data-ttu-id="47a8f-128">Zkontrolovat požadavky na sítě k nasazení virtuálního zařízení StorSimple a nakonfigurovaný podle požadavků na síti datového centra.</span><span class="sxs-lookup"><span data-stu-id="47a8f-128">You have reviewed the networking requirements to deploy a StorSimple virtual device and configured the datacenter network as per the requirements.</span></span> 

## <a name="step-by-step-provisioning"></a><span data-ttu-id="47a8f-129">Krok za krokem zřizování</span><span class="sxs-lookup"><span data-stu-id="47a8f-129">Step-by-step provisioning</span></span>
<span data-ttu-id="47a8f-130">Pokud chcete zřídit a připojení k virtuálnímu zařízení, musíte provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="47a8f-130">To provision and connect to a virtual device, you need to perform the following steps:</span></span>

1. <span data-ttu-id="47a8f-131">Zajistěte, aby hostitelský systém dostatek prostředků ke splňují požadavky na minimální virtuální zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a8f-131">Ensure that the host system has sufficient resources to meet the minimum virtual device requirements.</span></span>
2. <span data-ttu-id="47a8f-132">Zřídit virtuální zařízení ve vaší hypervisoru.</span><span class="sxs-lookup"><span data-stu-id="47a8f-132">Provision a virtual device in your hypervisor.</span></span>
3. <span data-ttu-id="47a8f-133">Spusťte virtuální zařízení a získat IP adresu.</span><span class="sxs-lookup"><span data-stu-id="47a8f-133">Start the virtual device and get the IP address.</span></span>

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a><span data-ttu-id="47a8f-134">Krok 1: Ujistěte se, že hostitelský systém splňuje požadavky na minimální virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="47a8f-134">Step 1: Ensure host system meets minimum virtual device requirements</span></span>
<span data-ttu-id="47a8f-135">Pokud chcete vytvořit virtuální zařízení, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="47a8f-135">To create a virtual device, you will need:</span></span>

* <span data-ttu-id="47a8f-136">Přístup k systému hostitele, který používá VMware ESXi Server 5.5 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="47a8f-136">Access to a host system running VMware ESXi Server 5.5 and above.</span></span>
* <span data-ttu-id="47a8f-137">VMware vSphere klienta v systému pro správu hostitele ESXi.</span><span class="sxs-lookup"><span data-stu-id="47a8f-137">VMware vSphere client on your system to manage the ESXi host.</span></span>

  * <span data-ttu-id="47a8f-138">Minimálně 4 jádra.</span><span class="sxs-lookup"><span data-stu-id="47a8f-138">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="47a8f-139">Alespoň 8 GB paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="47a8f-139">At least 8 GB of RAM.</span></span> <span data-ttu-id="47a8f-140">Pokud chcete konfigurovat virtuální pole jako souborový server, 8 GB podporuje menší než 2 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="47a8f-140">If you plan to configure the virtual array as file server, 8 GB supports less than 2 million files.</span></span> <span data-ttu-id="47a8f-141">Je nutné 16 GB paměti RAM pro podporu 2 – 4 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="47a8f-141">You need 16 GB RAM to support 2 - 4 million files.</span></span>
  * <span data-ttu-id="47a8f-142">Jedno síťové rozhraní připojené k síti podporující směrování provoz do Internetu.</span><span class="sxs-lookup"><span data-stu-id="47a8f-142">One network interface connected to the network capable of routing traffic to Internet.</span></span> <span data-ttu-id="47a8f-143">Minimální šířky pásma Internetu by měl být 5 MB/s na povolit pro optimální fungování zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a8f-143">The minimum Internet bandwidth should be 5 Mbps to allow for optimal working of the device.</span></span>
  * <span data-ttu-id="47a8f-144">500 GB virtuální disk pro data.</span><span class="sxs-lookup"><span data-stu-id="47a8f-144">A 500 GB virtual disk for data.</span></span>

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a><span data-ttu-id="47a8f-145">Krok 2: Zřídit virtuální zařízení v hypervisoru</span><span class="sxs-lookup"><span data-stu-id="47a8f-145">Step 2: Provision a virtual device in hypervisor</span></span>
<span data-ttu-id="47a8f-146">Proveďte následující kroky pro zřízení virtuálního zařízení ve vaší hypervisoru.</span><span class="sxs-lookup"><span data-stu-id="47a8f-146">Perform the following steps to provision a virtual device in your hypervisor.</span></span>

1. <span data-ttu-id="47a8f-147">Zkopírujte bitovou kopii virtuálního zařízení ve vašem systému.</span><span class="sxs-lookup"><span data-stu-id="47a8f-147">Copy the virtual device image on your system.</span></span> <span data-ttu-id="47a8f-148">Jste si stáhli tuto bitovou kopii virtuálního prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="47a8f-148">You downloaded this virtual image through the Azure portal.</span></span>

   1. <span data-ttu-id="47a8f-149">Ujistěte se, že jste si stáhli nejnovější soubor bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="47a8f-149">Ensure that you have downloaded the latest image file.</span></span> <span data-ttu-id="47a8f-150">Pokud jste předtím stáhli bitovou kopii, stáhněte ho znovu a ujistěte se, že máte nejnovější bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="47a8f-150">If you downloaded the image earlier, download it again to ensure you have the latest image.</span></span> <span data-ttu-id="47a8f-151">Nejnovější bitové kopie má dva soubory (místo toho).</span><span class="sxs-lookup"><span data-stu-id="47a8f-151">The latest image has two files (instead of one).</span></span>
   2. <span data-ttu-id="47a8f-152">Poznamenejte si umístění, kam můžete zkopírovat bitovou kopii jako používáte tuto bitovou kopii později v postupu.</span><span class="sxs-lookup"><span data-stu-id="47a8f-152">Make a note of the location where you copied the image as you are using this image later in the procedure.</span></span>

2. <span data-ttu-id="47a8f-153">Přihlaste se k serveru ESXi pomocí vSphere klienta.</span><span class="sxs-lookup"><span data-stu-id="47a8f-153">Log in to the ESXi server using the vSphere client.</span></span> <span data-ttu-id="47a8f-154">Musíte mít oprávnění správce k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="47a8f-154">You need to have administrator privileges to create a virtual machine.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image1.png)
3. <span data-ttu-id="47a8f-155">V klientovi vSphere v sekci inventáře v levém podokně vyberte Server, ESXi.</span><span class="sxs-lookup"><span data-stu-id="47a8f-155">In the vSphere client, in the inventory section in the left pane, select the ESXi Server.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image2.png)
4. <span data-ttu-id="47a8f-156">Nahrajte na server ESXi VMDK.</span><span class="sxs-lookup"><span data-stu-id="47a8f-156">Upload the VMDK to the ESXi server.</span></span> <span data-ttu-id="47a8f-157">Přejděte na **konfigurace** kartě v pravém podokně.</span><span class="sxs-lookup"><span data-stu-id="47a8f-157">Navigate to the **Configuration** tab in the right pane.</span></span> <span data-ttu-id="47a8f-158">V části **hardwaru**, vyberte **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-158">Under **Hardware**, select **Storage**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image3.png)
5. <span data-ttu-id="47a8f-159">V pravém podokně v části **Datastores**, vyberte úložiště dat, kam chcete nahrát VMDK.</span><span class="sxs-lookup"><span data-stu-id="47a8f-159">In the right pane, under **Datastores**, select the datastore where you want to upload the VMDK.</span></span> <span data-ttu-id="47a8f-160">S úložištěm, musí mít dostatek volného místa pro disky operačního systému a data.</span><span class="sxs-lookup"><span data-stu-id="47a8f-160">The datastore must have enough free space for the OS and data disks.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image4.png)
6. <span data-ttu-id="47a8f-161">Klikněte pravým tlačítkem a vyberte **procházet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-161">Right-click and select **Browse Datastore**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image5.png)
7. <span data-ttu-id="47a8f-162">A **úložiště prohlížeče** se zobrazí v okně.</span><span class="sxs-lookup"><span data-stu-id="47a8f-162">A **Datastore Browser** window appears.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image6.png)
8. <span data-ttu-id="47a8f-163">Na panelu nástrojů klikněte na tlačítko ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png) ikonu vytvořte novou složku.</span><span class="sxs-lookup"><span data-stu-id="47a8f-163">In the tool bar, click ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png) icon to create a new folder.</span></span> <span data-ttu-id="47a8f-164">Zadejte název složky a poznamenejte si ho.</span><span class="sxs-lookup"><span data-stu-id="47a8f-164">Specify the folder name and make a note of it.</span></span> <span data-ttu-id="47a8f-165">Tento název složky bude používat později, při vytváření virtuálního počítače (doporučeno osvědčený postup).</span><span class="sxs-lookup"><span data-stu-id="47a8f-165">You will use this folder name later when creating a virtual machine (recommended best practice).</span></span> <span data-ttu-id="47a8f-166">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-166">Click **OK**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image8.png)
9. <span data-ttu-id="47a8f-167">Do nové složky se zobrazí v levém podokně **úložiště prohlížeče**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-167">The new folder appears in the left pane of the **Datastore Browser**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image9.png)
10. <span data-ttu-id="47a8f-168">Klikněte na ikonu nahrávání ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png) a vyberte **nahrát soubor**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-168">Click the Upload icon ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png) and select **Upload File**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image11.png)
11. <span data-ttu-id="47a8f-169">Procházet a přejděte na soubory VMDK, které jste stáhli.</span><span class="sxs-lookup"><span data-stu-id="47a8f-169">Browse and point to the VMDK files that you downloaded.</span></span> <span data-ttu-id="47a8f-170">Existují dva soubory.</span><span class="sxs-lookup"><span data-stu-id="47a8f-170">There are two files.</span></span> <span data-ttu-id="47a8f-171">Vyberte soubor k odeslání.</span><span class="sxs-lookup"><span data-stu-id="47a8f-171">Select a file to upload.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image12m.png)
12. <span data-ttu-id="47a8f-172">Klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-172">Click **Open**.</span></span> <span data-ttu-id="47a8f-173">Spustí se nahrávání souboru VMDK zadaný úložištěm.</span><span class="sxs-lookup"><span data-stu-id="47a8f-173">The upload of the VMDK file to the specified datastore starts.</span></span> <span data-ttu-id="47a8f-174">To může trvat několik minut, než soubor k odeslání.</span><span class="sxs-lookup"><span data-stu-id="47a8f-174">It may take several minutes for the file to upload.</span></span>
13. <span data-ttu-id="47a8f-175">Po dokončení nahrávání se zobrazí souboru v úložišti dat ve složce, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="47a8f-175">After the upload is complete, you see the file in the datastore in the folder you created.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image14.png)

    <span data-ttu-id="47a8f-176">Teď nahrajte druhý soubor VMDK na stejné úložiště.</span><span class="sxs-lookup"><span data-stu-id="47a8f-176">Now upload the second VMDK file to the same datastore.</span></span>
14. <span data-ttu-id="47a8f-177">Vraťte se do okna vSphere klienta.</span><span class="sxs-lookup"><span data-stu-id="47a8f-177">Return to the vSphere client window.</span></span> <span data-ttu-id="47a8f-178">S ESXi server vybraný, klikněte pravým tlačítkem a vyberte **nový virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-178">With ESXi server selected, right-click and select **New Virtual Machine**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image15.png)
15. <span data-ttu-id="47a8f-179">A **vytvořit nový virtuální počítač** se okno.</span><span class="sxs-lookup"><span data-stu-id="47a8f-179">A **Create New Virtual Machine** window will appear.</span></span> <span data-ttu-id="47a8f-180">Na **konfigurace** vyberte **vlastní** možnost.</span><span class="sxs-lookup"><span data-stu-id="47a8f-180">On the **Configuration** page, select the **Custom** option.</span></span> <span data-ttu-id="47a8f-181">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-181">Click **Next**.</span></span>
    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image16.png)
16. <span data-ttu-id="47a8f-182">Na **název a umístění** stránky, zadejte název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="47a8f-182">On the **Name and Location** page, specify the name of your virtual machine.</span></span> <span data-ttu-id="47a8f-183">Tento název by měl odpovídat názvu složky (osvědčeného postupu), který jste zadali v kroku 8.</span><span class="sxs-lookup"><span data-stu-id="47a8f-183">This name should match the folder name (recommended best practice) you specified earlier in Step 8.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image17.png)
17. <span data-ttu-id="47a8f-184">Na **úložiště** vyberte úložiště, kterou chcete použít ke zřízení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="47a8f-184">On the **Storage** page, select a datastore you want to use to provision your VM.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image18.png)
18. <span data-ttu-id="47a8f-185">Na **verze virtuálního počítače** vyberte **verze virtuálního počítače: 8**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-185">On the **Virtual Machine Version** page, select **Virtual Machine Version: 8**.</span></span> <span data-ttu-id="47a8f-186">Verze 8 až 11 všechny podporované.</span><span class="sxs-lookup"><span data-stu-id="47a8f-186">Versions 8 to 11 are all supported.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image19.png)
19. <span data-ttu-id="47a8f-187">Na **hostovaný operační systém** vyberte **hostovaný operační systém** jako **Windows**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-187">On the **Guest Operating System** page, select the **Guest Operating System** as **Windows**.</span></span> <span data-ttu-id="47a8f-188">Pro **verze**, z rozevíracího seznamu vyberte **Microsoft Windows Server 2012 (64 bitů)**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-188">For **Version**, from the dropdown list, select **Microsoft Windows Server 2012 (64-bit)**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image20.png)
20. <span data-ttu-id="47a8f-189">Na **procesory** stránky, upravte **virtuální soketů** a **počet jader na virtuální soketu** tak, aby **celkový počet jader** je 4 (nebo více).</span><span class="sxs-lookup"><span data-stu-id="47a8f-189">On the **CPUs** page, adjust the **Number of virtual sockets** and **Number of cores per virtual socket** so that the **Total number of cores** is 4 (or more).</span></span> <span data-ttu-id="47a8f-190">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-190">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image21.png)
21. <span data-ttu-id="47a8f-191">Na **paměti** zadejte 8 GB (nebo více) paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="47a8f-191">On the **Memory** page, specify 8 GB (or more) of RAM.</span></span> <span data-ttu-id="47a8f-192">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-192">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image22.png)
22. <span data-ttu-id="47a8f-193">Na **sítě** stránky, zadejte počet síťových rozhraní.</span><span class="sxs-lookup"><span data-stu-id="47a8f-193">On the **Network** page, specify the number of the network interfaces.</span></span> <span data-ttu-id="47a8f-194">Minimální požadavek je jedno síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="47a8f-194">The minimum requirement is one network interface.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image23.png)
23. <span data-ttu-id="47a8f-195">Na **řadič SCSI** přijměte výchozí **řadič LSI Logic SAS**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-195">On the **SCSI Controller** page, accept the default **LSI Logic SAS controller**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image24.png)
24. <span data-ttu-id="47a8f-196">Na **vyberte Disk** vyberte **použijte existující virtuální disk**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-196">On the **Select a Disk** page, choose **Use an existing virtual disk**.</span></span> <span data-ttu-id="47a8f-197">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-197">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image25.png)
25. <span data-ttu-id="47a8f-198">Na **vyberte stávající Disk** v části **cesta k souboru disku**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-198">On the **Select Existing Disk** page, under **Disk File Path**, click **Browse**.</span></span> <span data-ttu-id="47a8f-199">Tím se otevře **procházet Datastores** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="47a8f-199">This opens a **Browse Datastores** dialog.</span></span> <span data-ttu-id="47a8f-200">Přejděte do umístění, kde jste nahráli VMDK.</span><span class="sxs-lookup"><span data-stu-id="47a8f-200">Navigate to the location where you uploaded the VMDK.</span></span> <span data-ttu-id="47a8f-201">Nyní uvidíte pouze jeden soubor v úložišti dat jako sloučeným dva soubory, které jste původně nahráli.</span><span class="sxs-lookup"><span data-stu-id="47a8f-201">You now see only one file in the datastore as the two files that you initially uploaded have been merged.</span></span> <span data-ttu-id="47a8f-202">Vyberte soubor a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-202">Select the file and click **OK**.</span></span> <span data-ttu-id="47a8f-203">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-203">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image26.png)
26. <span data-ttu-id="47a8f-204">Na **pokročilé možnosti** , přijměte výchozí nastavení a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-204">On the **Advanced Options** page, accept the default and click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image27.png)
27. <span data-ttu-id="47a8f-205">Na **připravení Dokončit** zkontrolujte všechna nastavení, které jsou přidružené k novému virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="47a8f-205">On the **Ready to Complete** page, review all the settings associated with the new virtual machine.</span></span> <span data-ttu-id="47a8f-206">Zkontrolujte **upravit nastavení virtuálního počítače před dokončením**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-206">Check **Edit the virtual machine settings before completion**.</span></span> <span data-ttu-id="47a8f-207">Klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-207">Click **Continue**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image28.png)
28. <span data-ttu-id="47a8f-208">Na **vlastnosti virtuálního počítače** stránky v **hardwaru** najděte hardwarové zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a8f-208">On the **Virtual Machines Properties** page, in the **Hardware** tab, locate the device hardware.</span></span> <span data-ttu-id="47a8f-209">Vyberte **nový pevný Disk**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-209">Select **New Hard Disk**.</span></span> <span data-ttu-id="47a8f-210">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-210">Click **Add**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image29.png)
29. <span data-ttu-id="47a8f-211">Zobrazí **přidat Hardware** okno.</span><span class="sxs-lookup"><span data-stu-id="47a8f-211">You see a **Add Hardware** window.</span></span> <span data-ttu-id="47a8f-212">Na **typ zařízení** v části **vyberte typ zařízení, které chcete přidat**, vyberte **pevný Disk**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-212">On the **Device Type** page, under **Choose the type of device you wish to add**, select **Hard Disk**, and click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image30.png)
30. <span data-ttu-id="47a8f-213">Na **vyberte Disk** vyberte **vytvořit nový virtuální disk**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-213">On the **Select a Disk** page, choose **Create a new virtual disk**.</span></span> <span data-ttu-id="47a8f-214">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-214">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image31.png)
31. <span data-ttu-id="47a8f-215">Na **vytvoření disku** změňte **velikost disku** 500 GB (nebo více).</span><span class="sxs-lookup"><span data-stu-id="47a8f-215">On the **Create a Disk** page, change the **Disk Size** to 500 GB (or more).</span></span> <span data-ttu-id="47a8f-216">Požadavek na minimální při 500 GB můžete zřídit vždy větší disku.</span><span class="sxs-lookup"><span data-stu-id="47a8f-216">While 500 GB is the minimum requirement, you can always provision a larger disk.</span></span> <span data-ttu-id="47a8f-217">Všimněte si, že nelze rozbalit nebo zmenšení disku jednou zřízený.</span><span class="sxs-lookup"><span data-stu-id="47a8f-217">Note that you cannot expand or shrink the disk once provisioned.</span></span> <span data-ttu-id="47a8f-218">Další informace o velikosti disku a zajišťují, projděte si část nastavení velikosti v [osvědčené postupy dokumentu](storsimple-ova-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="47a8f-218">For more information on the size of disk to provision, review the sizing section in the [best practices document](storsimple-ova-best-practices.md).</span></span> <span data-ttu-id="47a8f-219">V části **disku zřizování**, vyberte **dynamickým zajišťováním**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-219">Under **Disk Provisioning**, select **Thin Provision**.</span></span> <span data-ttu-id="47a8f-220">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-220">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image32.png)
32. <span data-ttu-id="47a8f-221">Na **pokročilé možnosti** přijměte výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="47a8f-221">On the **Advanced Options** page, accept the default.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image33.png)
33. <span data-ttu-id="47a8f-222">Na **připravení Dokončit** stránky, projděte si možnosti disku.</span><span class="sxs-lookup"><span data-stu-id="47a8f-222">On the **Ready to Complete** page, review the disk options.</span></span> <span data-ttu-id="47a8f-223">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-223">Click **Finish**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image34.png)
34. <span data-ttu-id="47a8f-224">Návrat na stránku vlastností virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="47a8f-224">Return to the Virtual Machine Properties page.</span></span> <span data-ttu-id="47a8f-225">K virtuálnímu počítači se přidá nový pevný disk.</span><span class="sxs-lookup"><span data-stu-id="47a8f-225">A new hard disk is added to your virtual machine.</span></span> <span data-ttu-id="47a8f-226">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-226">Click **Finish**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image35.png)
35. <span data-ttu-id="47a8f-227">S virtuální počítač vybrali v pravém podokně přejděte do **Souhrn** kartě.</span><span class="sxs-lookup"><span data-stu-id="47a8f-227">With your virtual machine selected in the right pane, navigate to the **Summary** tab.</span></span> <span data-ttu-id="47a8f-228">Zkontrolujte nastavení pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="47a8f-228">Review the settings for your virtual machine.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image36.png)

<span data-ttu-id="47a8f-229">Virtuální počítač je nyní zajištěna.</span><span class="sxs-lookup"><span data-stu-id="47a8f-229">Your virtual machine is now provisioned.</span></span> <span data-ttu-id="47a8f-230">Dalším krokem je spotřeby na tomto počítači a získat IP adresu.</span><span class="sxs-lookup"><span data-stu-id="47a8f-230">The next step is to power on this machine and get the IP address.</span></span>

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a><span data-ttu-id="47a8f-231">Krok 3: Spuštění virtuální zařízení a získat IP adresu</span><span class="sxs-lookup"><span data-stu-id="47a8f-231">Step 3: Start the virtual device and get the IP</span></span>
<span data-ttu-id="47a8f-232">Proveďte následující kroky ke spuštění virtuálního zařízení a k nim připojit.</span><span class="sxs-lookup"><span data-stu-id="47a8f-232">Perform the following steps to start your virtual device and connect to it.</span></span>

#### <a name="to-start-the-virtual-device"></a><span data-ttu-id="47a8f-233">Při spuštění virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="47a8f-233">To start the virtual device</span></span>
1. <span data-ttu-id="47a8f-234">Spuštění virtuálního zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a8f-234">Start the virtual device.</span></span> <span data-ttu-id="47a8f-235">V vSphere nástroje Configuration Manager, v levém podokně vyberte zařízení a klikněte pravým tlačítkem na zprovoznit v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="47a8f-235">In the vSphere Configuration Manager, in the left pane, select your device and right-click to bring up the context menu.</span></span> <span data-ttu-id="47a8f-236">Vyberte **Power** a pak vyberte **zapnout**.</span><span class="sxs-lookup"><span data-stu-id="47a8f-236">Select **Power** and then select **Power on**.</span></span> <span data-ttu-id="47a8f-237">To by měl power ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="47a8f-237">This should power on your virtual machine.</span></span> <span data-ttu-id="47a8f-238">Můžete zobrazit stav v dolní části **poslední úkoly** podokně vSphere klienta.</span><span class="sxs-lookup"><span data-stu-id="47a8f-238">You can view the status in the bottom **Recent Tasks** pane of the vSphere client.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image37.png)
2. <span data-ttu-id="47a8f-239">Instalační program úlohy bude trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="47a8f-239">The setup tasks will take a few minutes to complete.</span></span> <span data-ttu-id="47a8f-240">Jakmile je na zařízení spuštěný, přejděte na **konzoly** kartě.</span><span class="sxs-lookup"><span data-stu-id="47a8f-240">Once the device is running, navigate to the **Console** tab.</span></span> <span data-ttu-id="47a8f-241">Odešlete Ctrl + Alt + Delete pro přihlášení k zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a8f-241">Send Ctrl+Alt+Delete to log in to the device.</span></span> <span data-ttu-id="47a8f-242">Alternativně můžete bodu kurzoru v okně konzoly a stiskněte klávesu Ctrl + Alt + Insert.</span><span class="sxs-lookup"><span data-stu-id="47a8f-242">Alternatively, you can point the cursor on the console window and press Ctrl+Alt+Insert.</span></span> <span data-ttu-id="47a8f-243">Výchozí uživatel se *StorSimpleAdmin* a výchozí heslo je *Heslo1*.</span><span class="sxs-lookup"><span data-stu-id="47a8f-243">The default user is *StorSimpleAdmin* and the default password is *Password1*.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image38.png)
3. <span data-ttu-id="47a8f-244">Z bezpečnostních důvodů vyprší platnost hesla správce zařízení při prvním přihlášení.</span><span class="sxs-lookup"><span data-stu-id="47a8f-244">For security reasons, the device administrator password expires at the first logon.</span></span> <span data-ttu-id="47a8f-245">Zobrazí se výzva ke změně hesla.</span><span class="sxs-lookup"><span data-stu-id="47a8f-245">You are prompted to change the password.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image39.png)
4. <span data-ttu-id="47a8f-246">Zadejte heslo obsahující alespoň 8 znaků.</span><span class="sxs-lookup"><span data-stu-id="47a8f-246">Enter a password that contains at least 8 characters.</span></span> <span data-ttu-id="47a8f-247">Heslo musí obsahovat 3 z 4 z těchto požadavků: velká písmena, malá písmena, číselné a speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="47a8f-247">The password must contain 3 out of 4 of these requirements: uppercase, lowercase, numeric, and special characters.</span></span> <span data-ttu-id="47a8f-248">Zadejte znovu heslo k potvrzení této akce.</span><span class="sxs-lookup"><span data-stu-id="47a8f-248">Reenter the password to confirm it.</span></span> <span data-ttu-id="47a8f-249">Budete informováni, že došlo ke změně hesla.</span><span class="sxs-lookup"><span data-stu-id="47a8f-249">You will be notified that the password has changed.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image40.png)
5. <span data-ttu-id="47a8f-250">Po heslo úspěšně změněno, může restartování virtuálního zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a8f-250">After the password is successfully changed, the virtual device may reboot.</span></span> <span data-ttu-id="47a8f-251">Počkejte na dokončení restartování.</span><span class="sxs-lookup"><span data-stu-id="47a8f-251">Wait for the reboot to complete.</span></span> <span data-ttu-id="47a8f-252">Konzole prostředí Windows PowerShell pro zařízení, může se zobrazit společně s indikátor průběhu.</span><span class="sxs-lookup"><span data-stu-id="47a8f-252">The Windows PowerShell console of the device may be displayed along with a progress bar.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image41.png)
6. <span data-ttu-id="47a8f-253">Kroky 6 až 8 se projeví pouze při dalším spuštění v prostředí bez služby DHCP.</span><span class="sxs-lookup"><span data-stu-id="47a8f-253">Steps 6-8 only apply when booting up in a non-DHCP environment.</span></span> <span data-ttu-id="47a8f-254">Pokud jste v prostředí s DHCP, tyto kroky přeskočte a přejděte ke kroku 9.</span><span class="sxs-lookup"><span data-stu-id="47a8f-254">If you are in a DHCP environment, then skip these steps and go to step 9.</span></span> <span data-ttu-id="47a8f-255">Pokud je spouštěn nahoru zařízení v prostředí bez služby DHCP, zobrazí se následující obrazovka.</span><span class="sxs-lookup"><span data-stu-id="47a8f-255">If you booted up your device in non-DHCP environment, you will see the following screen.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image42m.png)

   <span data-ttu-id="47a8f-256">V dalším kroku Nakonfigurujte síť.</span><span class="sxs-lookup"><span data-stu-id="47a8f-256">Next, configure the network.</span></span>
7. <span data-ttu-id="47a8f-257">Použití `Get-HcsIpAddress` seznam rozhraní sítě povolené na virtuálním zařízení příkazu.</span><span class="sxs-lookup"><span data-stu-id="47a8f-257">Use the `Get-HcsIpAddress` command to list the network interfaces enabled on your virtual device.</span></span> <span data-ttu-id="47a8f-258">Pokud vaše zařízení má jedno síťové rozhraní povolena, je výchozí název, který je přiřazen k tomuto rozhraní `Ethernet`.</span><span class="sxs-lookup"><span data-stu-id="47a8f-258">If your device has a single network interface enabled, the default name assigned to this interface is `Ethernet`.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image43m.png)
8. <span data-ttu-id="47a8f-259">Použití `Set-HcsIpAddress` rutiny pro konfiguraci sítě.</span><span class="sxs-lookup"><span data-stu-id="47a8f-259">Use the `Set-HcsIpAddress` cmdlet to configure the network.</span></span> <span data-ttu-id="47a8f-260">Níže je uveden příklad:</span><span class="sxs-lookup"><span data-stu-id="47a8f-260">An example is shown below:</span></span>

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image44.png)
9. <span data-ttu-id="47a8f-261">Po dokončení počáteční nastavení a zařízení má spuštění nahoru, zobrazí se text hlavičky zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a8f-261">After the initial setup is complete and the device has booted up, you will see the device banner text.</span></span> <span data-ttu-id="47a8f-262">Poznamenejte si IP adresu a adresu URL zobrazené v text hlavičky ke správě zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a8f-262">Make a note of the IP address and the URL displayed in the banner text to manage the device.</span></span> <span data-ttu-id="47a8f-263">Tuto IP adresu budete používat pro připojení k webového uživatelského rozhraní vašeho virtuální zařízení a místní instalaci a registraci dokončit.</span><span class="sxs-lookup"><span data-stu-id="47a8f-263">You will use this IP address to connect to the web UI of your virtual device and complete the local setup and registration.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image45.png)
10. <span data-ttu-id="47a8f-264">(Volitelné) Tento krok proveďte jenom v případě, že nasazujete zařízení v Cloud vlády.</span><span class="sxs-lookup"><span data-stu-id="47a8f-264">(Optional) Perform this step only if you are deploying your device in the Government Cloud.</span></span> <span data-ttu-id="47a8f-265">Nyní povolíte režimu Spojených států informace o zpracování Standard FIPS (Federal) ve vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a8f-265">You will now enable the United States Federal Information Processing Standard (FIPS) mode on your device.</span></span> <span data-ttu-id="47a8f-266">Standard FIPS 140 definuje kryptografické algoritmy pro použití schváleno nám Federal government počítačových systémů pro ochranu citlivá data.</span><span class="sxs-lookup"><span data-stu-id="47a8f-266">The FIPS 140 standard defines cryptographic algorithms approved for use by US Federal government computer systems for the protection of sensitive data.</span></span>

    1. <span data-ttu-id="47a8f-267">Pokud chcete povolit režim FIPS, spusťte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="47a8f-267">To enable the FIPS mode, run the following cmdlet:</span></span>

        `Enable-HcsFIPSMode`
    2. <span data-ttu-id="47a8f-268">Restartování zařízení po povolení režimu FIPS tak, aby kryptografických ověření vstoupí v platnost.</span><span class="sxs-lookup"><span data-stu-id="47a8f-268">Reboot your device after you have enabled the FIPS mode so that the cryptographic validations take effect.</span></span>

       > [!NOTE]
       > <span data-ttu-id="47a8f-269">Můžete povolit nebo zakázat režim FIPS ve vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a8f-269">You can either enable or disable FIPS mode on your device.</span></span> <span data-ttu-id="47a8f-270">Střídání zařízení mezi režim FIPS a standardu FIPS není podporována.</span><span class="sxs-lookup"><span data-stu-id="47a8f-270">Alternating the device between FIPS and non-FIPS mode is not supported.</span></span>
       >
       >

<span data-ttu-id="47a8f-271">Pokud zařízení nesplňuje minimální požadavky na konfiguraci, zobrazí se chyba v text hlavičky (zobrazené dole).</span><span class="sxs-lookup"><span data-stu-id="47a8f-271">If your device does not meet the minimum configuration requirements, you will see an error in the banner text (shown below).</span></span> <span data-ttu-id="47a8f-272">Potřebujete upravit konfiguraci zařízení tak, aby ho nemá odpovídající prostředky pro splňují minimální požadavky.</span><span class="sxs-lookup"><span data-stu-id="47a8f-272">You will need to modify the device configuration so that it has adequate resources to meet the minimum requirements.</span></span> <span data-ttu-id="47a8f-273">Potom můžete restartovat a připojte k zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a8f-273">You can then restart and connect to the device.</span></span> <span data-ttu-id="47a8f-274">Přečtěte si požadavky minimální konfigurace v [krok 1: Ujistěte se, že hostitelský systém splňuje požadavky na minimální virtuální zařízení](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).</span><span class="sxs-lookup"><span data-stu-id="47a8f-274">Refer to the minimum configuration requirements in [Step 1: Ensure that the host system meets minimum virtual device requirements](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).</span></span>

![](./media/storsimple-virtual-array-deploy2-provision-vmware/image46.png)

<span data-ttu-id="47a8f-275">Pokud jste čelí chybě během počáteční konfigurace provedená prostřednictvím místního webového uživatelského rozhraní, podívejte se na následující pracovních postupů:</span><span class="sxs-lookup"><span data-stu-id="47a8f-275">If you face any other error during the initial configuration using the local web UI, refer to the following workflows:</span></span>

* <span data-ttu-id="47a8f-276">Spustit diagnostické testy na [řešení potíží s instalací webového uživatelského rozhraní](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).</span><span class="sxs-lookup"><span data-stu-id="47a8f-276">Run diagnostic tests to [troubleshoot web UI setup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).</span></span>
* <span data-ttu-id="47a8f-277">[Generovat balíček protokolu a prohlížení soubory protokolů](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span><span class="sxs-lookup"><span data-stu-id="47a8f-277">[Generate log package and view log files](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span></span>

## <a name="next-steps"></a><span data-ttu-id="47a8f-278">Další kroky</span><span class="sxs-lookup"><span data-stu-id="47a8f-278">Next steps</span></span>
* [<span data-ttu-id="47a8f-279">Nastavení pole virtuální zařízení StorSimple jako souborový server</span><span class="sxs-lookup"><span data-stu-id="47a8f-279">Set up your StorSimple Virtual Array as a file server</span></span>](storsimple-virtual-array-deploy3-fs-setup.md)
* [<span data-ttu-id="47a8f-280">Nastavení pole virtuální zařízení StorSimple jako serveru iSCSI</span><span class="sxs-lookup"><span data-stu-id="47a8f-280">Set up your StorSimple Virtual Array as an iSCSI server</span></span>](storsimple-virtual-array-deploy3-iscsi-setup.md)
