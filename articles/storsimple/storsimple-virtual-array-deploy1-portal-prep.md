---
title: "Příprava portálu aplikace pro pole virtuální zařízení StorSimple | Microsoft Docs"
description: "První kurz k nasazení pole virtuální zařízení StorSimple spočívá v přípravě portálu Azure"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 68a4cfd3-94c9-46cb-805c-46217290ce02
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3d0801053721f98ce7a2b0fcbe3c65da8dbdd8d3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-storsimple-virtual-array---prepare-the-azure-portal"></a><span data-ttu-id="09105-103">Nasazení pole virtuálního zařízení StorSimple – Příprava portálu Azure</span><span class="sxs-lookup"><span data-stu-id="09105-103">Deploy StorSimple Virtual Array - Prepare the Azure portal</span></span>

![](./media/storsimple-virtual-array-deploy1-portal-prep/getstarted4.png)
## <a name="overview"></a><span data-ttu-id="09105-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="09105-104">Overview</span></span>

<span data-ttu-id="09105-105">Toto je první článku řady kurzy nasazení, které jsou potřebné k nasazení zcela virtuální pole jako souborový server nebo server se službou iSCSI pomocí modelu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="09105-105">This is the first article in the series of deployment tutorials required to completely deploy your virtual array as a file server or an iSCSI server using the Resource Manager model.</span></span> <span data-ttu-id="09105-106">Tento článek popisuje přípravu potřebné k vytvoření a konfiguraci služby StorSimple Manager zařízení před zřizování virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="09105-106">This article describes the preparation required to create and configure your StorSimple Device Manager service prior to provisioning a virtual array.</span></span> <span data-ttu-id="09105-107">Tento článek taky obsahuje odkazy na kontrolní seznam konfigurace nasazení a konfigurace požadované součásti.</span><span class="sxs-lookup"><span data-stu-id="09105-107">This article also links out to a deployment configuration checklist and configuration prerequisites.</span></span>

<span data-ttu-id="09105-108">K dokončení této instalace a procesu konfigurace potřebujete oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="09105-108">You need administrator privileges to complete the setup and configuration process.</span></span> <span data-ttu-id="09105-109">Doporučujeme, abyste si prošli kontrolní seznam konfigurace nasazení před zahájením.</span><span class="sxs-lookup"><span data-stu-id="09105-109">We recommend that you review the deployment configuration checklist before you begin.</span></span> <span data-ttu-id="09105-110">Příprava portálu trvá méně než 10 minut.</span><span class="sxs-lookup"><span data-stu-id="09105-110">The portal preparation takes less than 10 minutes.</span></span>

<span data-ttu-id="09105-111">Informace zveřejněné v tomto článku se vztahují na nasazení pole virtuální zařízení StorSimple v portálu Azure a cloudu Microsoft Azure Government.</span><span class="sxs-lookup"><span data-stu-id="09105-111">The information published in this article applies to the deployment of StorSimple Virtual Arrays in the Azure portal and Microsoft Azure Government Cloud.</span></span>

### <a name="get-started"></a><span data-ttu-id="09105-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="09105-112">Get started</span></span>
<span data-ttu-id="09105-113">Pracovní postup nasazení se skládá z přípravy na portálu, zřizování virtuální pole ve virtualizovaném prostředí a dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="09105-113">The deployment workflow consists of preparing the portal, provisioning a virtual array in your virtualized environment, and completing the setup.</span></span> <span data-ttu-id="09105-114">Chcete-li začít s nasazením pole virtuální zařízení StorSimple jako souborový server nebo server se službou iSCSI, naleznete v následující tabulce materiálech.</span><span class="sxs-lookup"><span data-stu-id="09105-114">To get started with the StorSimple Virtual Array deployment as a file server or an iSCSI server, you need to refer to the following tabulated resources.</span></span>

#### <a name="deployment-articles"></a><span data-ttu-id="09105-115">Články nasazení</span><span class="sxs-lookup"><span data-stu-id="09105-115">Deployment articles</span></span>

<span data-ttu-id="09105-116">Nasadit pole virtuální zařízení StorSimple, naleznete v následujících článcích v předepsaných pořadí.</span><span class="sxs-lookup"><span data-stu-id="09105-116">To deploy your StorSimple Virtual Array, refer to the following articles in the prescribed sequence.</span></span>

| **#** | <span data-ttu-id="09105-117">**V tomto kroku**</span><span class="sxs-lookup"><span data-stu-id="09105-117">**In this step**</span></span> | <span data-ttu-id="09105-118">**To uděláte...**</span><span class="sxs-lookup"><span data-stu-id="09105-118">**You do this …**</span></span> | <span data-ttu-id="09105-119">**A použijte tyto dokumenty.**</span><span class="sxs-lookup"><span data-stu-id="09105-119">**And use these documents.**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="09105-120">1.</span><span class="sxs-lookup"><span data-stu-id="09105-120">1.</span></span> |<span data-ttu-id="09105-121">**Nastavení portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="09105-121">**Set up the Azure portal**</span></span> |<span data-ttu-id="09105-122">Vytvoření a konfigurace služby StorSimple Manager zařízení před zřizování pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="09105-122">Create and configure your StorSimple Device Manager service prior to provisioning a StorSimple Virtual Array.</span></span> |[<span data-ttu-id="09105-123">Příprava na portálu</span><span class="sxs-lookup"><span data-stu-id="09105-123">Prepare the portal</span></span>](storsimple-virtual-array-deploy1-portal-prep.md) |
| <span data-ttu-id="09105-124">2.</span><span class="sxs-lookup"><span data-stu-id="09105-124">2.</span></span> |<span data-ttu-id="09105-125">**Zřizování virtuální pole**</span><span class="sxs-lookup"><span data-stu-id="09105-125">**Provision the Virtual Array**</span></span> |<span data-ttu-id="09105-126">Pro Hyper-V zřizování a připojte se k poli virtuální zařízení StorSimple v systému hostitele s technologií Hyper-V v systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="09105-126">For Hyper-V, provision and connect to a StorSimple Virtual Array on a host system running Hyper-V on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2.</span></span> <br></br> <br></br> <span data-ttu-id="09105-127">Pro VMware zřizování a připojte se k poli virtuální zařízení StorSimple v hostitelském systému, systémem VMware ESXi 5.5 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="09105-127">For VMware, provision and connect to a StorSimple Virtual Array on a host system running VMware ESXi 5.5 and above.</span></span><br></br> |[<span data-ttu-id="09105-128">Zřídit virtuální pole Hyper-v</span><span class="sxs-lookup"><span data-stu-id="09105-128">Provision a virtual array in Hyper-V</span></span>](storsimple-virtual-array-deploy2-provision-hyperv.md) <br></br> <br></br> [<span data-ttu-id="09105-129">Zřídit virtuální pole v prostředí VMware</span><span class="sxs-lookup"><span data-stu-id="09105-129">Provision a virtual array in VMware</span></span>](storsimple-virtual-array-deploy2-provision-vmware.md) |
| <span data-ttu-id="09105-130">3.</span><span class="sxs-lookup"><span data-stu-id="09105-130">3.</span></span> |<span data-ttu-id="09105-131">**Nastavit virtuální pole**</span><span class="sxs-lookup"><span data-stu-id="09105-131">**Set up the Virtual Array**</span></span> |<span data-ttu-id="09105-132">Pro souborový server provedení počáteční instalace, zaregistrujte zařízení StorSimple souborového serveru a dokončení instalace zařízení.</span><span class="sxs-lookup"><span data-stu-id="09105-132">For your file server, perform initial setup, register your StorSimple file server, and complete the device setup.</span></span> <span data-ttu-id="09105-133">Potom můžete zřídit sdílené složky protokolu SMB.</span><span class="sxs-lookup"><span data-stu-id="09105-133">You can then provision SMB shares.</span></span> <br></br> <br></br> <span data-ttu-id="09105-134">Pro váš server iSCSI provedení počáteční instalace, zaregistrujte serveru iSCSI StorSimple a dokončení instalace zařízení.</span><span class="sxs-lookup"><span data-stu-id="09105-134">For your iSCSI server, perform initial setup, register your StorSimple iSCSI server, and complete the device setup.</span></span> <span data-ttu-id="09105-135">Potom můžete zřídit svazky iSCSI.</span><span class="sxs-lookup"><span data-stu-id="09105-135">You can then provision iSCSI volumes.</span></span> |[<span data-ttu-id="09105-136">Nastavit virtuální pole jako souborový server</span><span class="sxs-lookup"><span data-stu-id="09105-136">Set up virtual array as file server</span></span>](storsimple-virtual-array-deploy3-fs-setup.md)<br></br> <br></br>[<span data-ttu-id="09105-137">Nastavit virtuální pole jako iSCSI server</span><span class="sxs-lookup"><span data-stu-id="09105-137">Set up virtual array as iSCSI server</span></span>](storsimple-virtual-array-deploy3-iscsi-setup.md) |

<span data-ttu-id="09105-138">Teď můžete začít nastavení portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="09105-138">You can now begin to set up the Azure portal.</span></span>

## <a name="configuration-checklist"></a><span data-ttu-id="09105-139">Kontrolní seznam konfigurace</span><span class="sxs-lookup"><span data-stu-id="09105-139">Configuration checklist</span></span>

<span data-ttu-id="09105-140">Kontrolní seznam konfigurace popisuje informace, které je třeba shromáždit před konfigurací softwaru na pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="09105-140">The configuration checklist describes the information that you need to collect before you configure the software on your StorSimple Virtual Array.</span></span> <span data-ttu-id="09105-141">Příprava předem tyto informace pomáhají zjednodušit proces nasazení zařízení StorSimple ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="09105-141">Preparing this information ahead of time helps streamline the process of deploying the StorSimple device in your environment.</span></span> <span data-ttu-id="09105-142">V závislosti na tom, jestli je vaše pole virtuální zařízení StorSimple nasazený jako souborový server nebo server se službou iSCSI, budete potřebovat následující kontrolní seznamy.</span><span class="sxs-lookup"><span data-stu-id="09105-142">Depending upon whether your StorSimple Virtual Array is deployed as a file server or an iSCSI server, you need one of the following checklists.</span></span>

* <span data-ttu-id="09105-143">Stažení [StorSimple kontrolní seznam konfigurace virtuální pole souboru](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).</span><span class="sxs-lookup"><span data-stu-id="09105-143">Download the [StorSimple Virtual Array File Server Configuration Checklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).</span></span>
* <span data-ttu-id="09105-144">Stažení [pole virtuální zařízení StorSimple iSCSI kontrolní seznam konfigurace serveru](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).</span><span class="sxs-lookup"><span data-stu-id="09105-144">Download the [StorSimple Virtual Array iSCSI Server Configuration Checklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="09105-145">Požadavky</span><span class="sxs-lookup"><span data-stu-id="09105-145">Prerequisites</span></span>

<span data-ttu-id="09105-146">Zde zjistíte požadavky konfigurace služby StorSimple Manager zařízení StorSimple virtuální pole a síti datového centra.</span><span class="sxs-lookup"><span data-stu-id="09105-146">Here you find the configuration prerequisites for your StorSimple Device Manager service, your StorSimple Virtual Array, and the datacenter network.</span></span>

### <a name="for-the-storsimple-device-manager-service"></a><span data-ttu-id="09105-147">Služba Správce zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="09105-147">For the StorSimple Device Manager service</span></span>

<span data-ttu-id="09105-148">Než začnete, ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="09105-148">Before you begin, make sure that:</span></span>

* <span data-ttu-id="09105-149">Máte účet Microsoft a přihlašovací údaje účtu.</span><span class="sxs-lookup"><span data-stu-id="09105-149">You have your Microsoft account with access credentials.</span></span>
* <span data-ttu-id="09105-150">Máte účet služby Microsoft Azure Storage a přihlašovací údaje účtu.</span><span class="sxs-lookup"><span data-stu-id="09105-150">You have your Microsoft Azure storage account with access credentials.</span></span>
* <span data-ttu-id="09105-151">Vaše předplatné Microsoft Azure by měl povolit pro službu StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="09105-151">Your Microsoft Azure subscription should be enabled for StorSimple Device Manager service.</span></span>

### <a name="for-the-storsimple-virtual-array"></a><span data-ttu-id="09105-152">Pro pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="09105-152">For the StorSimple Virtual Array</span></span>

<span data-ttu-id="09105-153">Před nasazením virtuální pole, ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="09105-153">Before you deploy a virtual array, make sure that:</span></span>

* <span data-ttu-id="09105-154">Máte přístup k systému hostitele s technologií Hyper-V v systému Windows Server 2008 R2 nebo novější nebo VMware (ESXi 5.5 nebo novější), kterou lze použít k zřízení zařízení.</span><span class="sxs-lookup"><span data-stu-id="09105-154">You have access to a host system running Hyper-V on Windows Server 2008 R2 or later or VMware (ESXi 5.5 or later) that can be used to a provision a device.</span></span>
* <span data-ttu-id="09105-155">Systém hostitele je možné vyhradit následující prostředky pro zřízení virtuální pole:</span><span class="sxs-lookup"><span data-stu-id="09105-155">The host system is able to dedicate the following resources to provision your virtual array:</span></span>
  
  * <span data-ttu-id="09105-156">Minimálně 4 jádra.</span><span class="sxs-lookup"><span data-stu-id="09105-156">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="09105-157">Alespoň 8 GB paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="09105-157">At least 8 GB of RAM.</span></span> <span data-ttu-id="09105-158">Pokud chcete konfigurovat virtuální pole jako souborový server, 8 GB podporuje 2 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="09105-158">If you plan to configure the virtual array as file server, 8 GB supports 2 million files.</span></span> <span data-ttu-id="09105-159">Je nutné 16 GB paměti RAM pro podporu 2 – 4 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="09105-159">You need 16 GB RAM to support 2 - 4 million files.</span></span>
  * <span data-ttu-id="09105-160">Jedno síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="09105-160">One network interface.</span></span>
  * <span data-ttu-id="09105-161">500 GB virtuální disk pro data systému.</span><span class="sxs-lookup"><span data-stu-id="09105-161">A 500 GB virtual disk for system data.</span></span>

### <a name="for-the-datacenter-network"></a><span data-ttu-id="09105-162">Pro síť datového centra</span><span class="sxs-lookup"><span data-stu-id="09105-162">For the datacenter network</span></span>

<span data-ttu-id="09105-163">Než začnete, ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="09105-163">Before you begin, make sure that:</span></span>

* <span data-ttu-id="09105-164">Síť ve vašem datovém centru je nakonfigurovaná podle síťové požadavky pro zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="09105-164">The network in your datacenter is configured as per the networking requirements for your StorSimple device.</span></span> <span data-ttu-id="09105-165">Další informace najdete v tématu [StorSimple virtuální požadavky na systém pole](storsimple-ova-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="09105-165">For more information, see the [StorSimple Virtual Array System Requirements](storsimple-ova-system-requirements.md).</span></span>
* <span data-ttu-id="09105-166">Pole virtuální zařízení StorSimple nemá vyhrazené 5 MB/s šířky pásma Internetu (nebo má více) vždy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="09105-166">Your StorSimple Virtual Array has a dedicated 5 Mbps Internet bandwidth (or more) available at all times.</span></span> <span data-ttu-id="09105-167">Tato šířky pásma nesmí sdílet s jinými aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="09105-167">This bandwidth should not be shared with any other applications.</span></span>

## <a name="step-by-step-preparation"></a><span data-ttu-id="09105-168">Krok za krokem přípravy</span><span class="sxs-lookup"><span data-stu-id="09105-168">Step-by-step preparation</span></span>

<span data-ttu-id="09105-169">Použijte následující podrobné pokyny k přípravě portálu pro službu Správce zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="09105-169">Use the following step-by-step instructions to prepare your portal for the StorSimple Device Manager service.</span></span>

## <a name="step-1-create-a-new-service"></a><span data-ttu-id="09105-170">Krok 1: Vytvoření nové služby</span><span class="sxs-lookup"><span data-stu-id="09105-170">Step 1: Create a new service</span></span>

<span data-ttu-id="09105-171">Jednu instanci služby StorSimple Manager zařízení můžete spravovat několik polí virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="09105-171">A single instance of the StorSimple Device Manager service can manage multiple StorSimple Virtual Arrays.</span></span> <span data-ttu-id="09105-172">Pomocí následujících kroků vytvořte instanci služby Správce zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="09105-172">Perform the following steps to create an instance of the StorSimple Device Manager service.</span></span> <span data-ttu-id="09105-173">Pokud máte existující službu StorSimple Manager zařízení spravovat vaše virtuální pole, přeskočte tento krok a přejděte k [krok 2: získání registračního klíče služby](#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="09105-173">If you have an existing StorSimple Device Manager service to manage your virtual arrays, skip this step and go to [Step 2: Get the service registration key](#step-2-get-the-service-registration-key).</span></span>

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

> [!IMPORTANT]
> <span data-ttu-id="09105-174">Pokud jste nepovolili automatické vytvoření účtu úložiště při vytvoření služby, po úspěšném vytvoření služby bude nutné vytvořit alespoň jeden účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="09105-174">If you did not enable the automatic creation of a storage account with your service, you will need to create at least one storage account after you have successfully created a service.</span></span>
> 
> * <span data-ttu-id="09105-175">Pokud jste nevytvořili účet úložiště automaticky, najdete podrobné pokyny k vytvoření účtu v tématu [Konfigurace nového účtu úložiště pro službu](#optional-step-configure-a-new-storage-account-for-the-service).</span><span class="sxs-lookup"><span data-stu-id="09105-175">If you did not create a storage account automatically, go to [Configure a new storage account for the service](#optional-step-configure-a-new-storage-account-for-the-service) for detailed instructions.</span></span>
> * <span data-ttu-id="09105-176">Pokud jste automatické vytvoření účtu úložiště povolili, pokračujte na [krok 2: Získání registračního klíče služby](#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="09105-176">If you enabled the automatic creation of a storage account, go to [Step 2: Get the service registration key](#step-2-get-the-service-registration-key).</span></span>
> 
> 

## <a name="step-2-get-the-service-registration-key"></a><span data-ttu-id="09105-177">Krok 2: Získání registračního klíče služby</span><span class="sxs-lookup"><span data-stu-id="09105-177">Step 2: Get the service registration key</span></span>

<span data-ttu-id="09105-178">Po vytvoření a spuštění služby Správce zařízení StorSimple je nutné získat registrační klíč služby.</span><span class="sxs-lookup"><span data-stu-id="09105-178">After the StorSimple Device Manager service is up and running, you will need to get the service registration key.</span></span> <span data-ttu-id="09105-179">Tento klíč se používá k registraci a připojení zařízení StorSimple ke službě.</span><span class="sxs-lookup"><span data-stu-id="09105-179">This key is used to register and connect your StorSimple device with the service.</span></span>

<span data-ttu-id="09105-180">Proveďte následující kroky v [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="09105-180">Perform the following steps in the [Azure portal](https://portal.azure.com/).</span></span>

[!INCLUDE [storsimple-virtual-array-get-service-registration-key](../../includes/storsimple-virtual-array-get-service-registration-key.md)]

> [!NOTE]
> <span data-ttu-id="09105-181">Registrační klíč služby slouží k registraci všech zařízení StorSimple Manager zařízení, které potřebují registrovat ve službě StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="09105-181">The service registration key is used to register all the StorSimple Device Manager devices that need to register with your StorSimple Device Manager service.</span></span>
> 
> 

## <a name="step-3-download-the-virtual-array-image"></a><span data-ttu-id="09105-182">Krok 3: Stáhnout bitovou kopii virtuálního pole</span><span class="sxs-lookup"><span data-stu-id="09105-182">Step 3: Download the virtual array image</span></span>

<span data-ttu-id="09105-183">Až budete mít registrační klíč služby, musíte stáhnout bitovou kopii odpovídající virtuální pole ke zřízení virtuálních pole v systému hostitele.</span><span class="sxs-lookup"><span data-stu-id="09105-183">After you have the service registration key, you will need to download the appropriate virtual array image to provision a virtual array on your host system.</span></span> <span data-ttu-id="09105-184">Image virtuálního pole jsou specifické pro operační systém a lze ji stáhnout ze stránky rychlý Start na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="09105-184">The virtual array images are operating system specific and can be downloaded from the Quick Start page in the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="09105-185">Software spuštěný na poli virtuální zařízení StorSimple lze použít pouze se službou Správce zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="09105-185">The software running on the StorSimple Virtual Array may only be used with the StorSimple Device Manager service.</span></span>
> 
> 

<span data-ttu-id="09105-186">Proveďte následující kroky v [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="09105-186">Perform the following steps in the [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="to-get-the-virtual-array-image"></a><span data-ttu-id="09105-187">Chcete-li získat bitovou kopii virtuálního pole</span><span class="sxs-lookup"><span data-stu-id="09105-187">To get the virtual array image</span></span>

1. <span data-ttu-id="09105-188">Přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="09105-188">Sign into the [Azure portal](https://portal.azure.com/).</span></span> 
2. <span data-ttu-id="09105-189">Na portálu Azure klikněte na tlačítko **procházet > Správci zařízení StorSimple**.</span><span class="sxs-lookup"><span data-stu-id="09105-189">In the Azure portal, click **Browse > StorSimple Device Managers**.</span></span>
3. <span data-ttu-id="09105-190">Vyberte existující službu StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="09105-190">Select an existing StorSimple Device Manager service.</span></span> <span data-ttu-id="09105-191">V **Manager zařízení StorSimple** okně klikněte na tlačítko **rychlý Start**.</span><span class="sxs-lookup"><span data-stu-id="09105-191">In the **StorSimple Device Manager** blade, click **Quick Start**.</span></span> 
4. <span data-ttu-id="09105-192">Kliknutím na odkaz odpovídající bitovou kopii, kterou chcete stáhnout z webu Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="09105-192">Click the link corresponding to the image that you want to download from the Microsoft Download Center.</span></span> <span data-ttu-id="09105-193">Soubory image jsou přibližně 4,8 GB.</span><span class="sxs-lookup"><span data-stu-id="09105-193">The image files are approximately 4.8 GB.</span></span>
   
   * <span data-ttu-id="09105-194">VHDX pro Hyper-V v systému Windows Server 2012 a novější</span><span class="sxs-lookup"><span data-stu-id="09105-194">VHDX for Hyper-V on Windows Server 2012 and later</span></span>
   * <span data-ttu-id="09105-195">Virtuální pevný disk pro technologii Hyper-V v systému Windows Server 2008 R2 a novější</span><span class="sxs-lookup"><span data-stu-id="09105-195">VHD for Hyper-V on Windows Server 2008 R2 and later</span></span>
   * <span data-ttu-id="09105-196">VMDK pro VMWare ESXi 5.5 a novější</span><span class="sxs-lookup"><span data-stu-id="09105-196">VMDK for VMWare ESXi 5.5 and later</span></span>
5. <span data-ttu-id="09105-197">Stáhněte a rozbalte soubor na místní disk, což poznámku o kterém se nachází soubor rozbalené.</span><span class="sxs-lookup"><span data-stu-id="09105-197">Download and unzip the file to a local drive, making a note of where the unzipped file is located.</span></span>

## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a><span data-ttu-id="09105-198">Volitelný krok: Konfigurace nového účtu úložiště pro službu</span><span class="sxs-lookup"><span data-stu-id="09105-198">Optional step: Configure a new storage account for the service</span></span>

<span data-ttu-id="09105-199">Tento krok je volitelný a musí provést pouze v případě, že jste nepovolili automatické vytvoření účtu úložiště s vaší služby.</span><span class="sxs-lookup"><span data-stu-id="09105-199">This step is optional and should be performed only if you did not enable the automatic creation of a storage account with your service.</span></span>

<span data-ttu-id="09105-200">Pokud potřebujete vytvořit účet úložiště Azure v jiné oblasti, přečtěte si téma [postup vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="09105-200">If you need to create an Azure storage account in a different region, see [How to create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) for step-by-step instructions.</span></span>

<span data-ttu-id="09105-201">Proveďte následující kroky v [portál Azure](https://ms.portal.azure.com/) na stránce služby StorSimple Manager zařízení přidat existující účet úložiště Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="09105-201">Perform the following steps in the [Azure portal](https://ms.portal.azure.com/) on the StorSimple Device Manager service page to add an existing Microsoft Azure storage account.</span></span>

#### <a name="to-add-a-storage-account-credential-that-has-the-same-azure-subscription-as-the-device-manager-service"></a><span data-ttu-id="09105-202">Chcete-li přidat přihlašovací údaje účtu úložiště, který má stejné předplatné jako služba Správce zařízení</span><span class="sxs-lookup"><span data-stu-id="09105-202">To add a storage account credential that has the same Azure subscription as the Device Manager service</span></span>

1. <span data-ttu-id="09105-203">Přejděte do vaší služby Správce zařízení, vyberte a dvakrát na ni klikněte.</span><span class="sxs-lookup"><span data-stu-id="09105-203">Navigate to your Device Manager service, select and double-click it.</span></span> <span data-ttu-id="09105-204">Tím se otevře **přehled** okno.</span><span class="sxs-lookup"><span data-stu-id="09105-204">This opens the **Overview** blade.</span></span>
2. <span data-ttu-id="09105-205">Vyberte **přihlašovacích údajů účtu úložiště** v rámci **konfigurace** části.</span><span class="sxs-lookup"><span data-stu-id="09105-205">Select **Storage account credentials** within the **Configuration** section.</span></span>
3. <span data-ttu-id="09105-206">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="09105-206">Click **Add**.</span></span>
4. <span data-ttu-id="09105-207">V **přidání účtu úložiště** okno, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="09105-207">In the **Add a storage account** blade, do the following:</span></span>
   
    1. <span data-ttu-id="09105-208">Pro **předplatné**, vyberte **aktuální**.</span><span class="sxs-lookup"><span data-stu-id="09105-208">For **Subscription**, select **Current**.</span></span>
   
    2. <span data-ttu-id="09105-209">Zadejte název účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="09105-209">Provide the name of your Azure storage account.</span></span>
   
    3. <span data-ttu-id="09105-210">Vyberte **povolit** vytvořit zabezpečený kanál pro síťovou komunikaci mezi zařízení StorSimple a cloudem.</span><span class="sxs-lookup"><span data-stu-id="09105-210">Select **Enable** to create a secure channel for network communication between your StorSimple device and the cloud.</span></span> <span data-ttu-id="09105-211">Vyberte **zakázat** pouze v případě, že pracujete v privátním cloudu.</span><span class="sxs-lookup"><span data-stu-id="09105-211">Select **Disable** only if you are operating within a private cloud.</span></span>
   
    4. <span data-ttu-id="09105-212">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="09105-212">Click **Add**.</span></span> <span data-ttu-id="09105-213">Upozornění se zobrazí po úspěšném vytvoření účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="09105-213">You are notified after the storage account is successfully created.</span></span><br></br>
   
     ![Přidat existující přihlašovací údaje účtu úložiště](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

## <a name="next-step"></a><span data-ttu-id="09105-215">Další krok</span><span class="sxs-lookup"><span data-stu-id="09105-215">Next step</span></span>

<span data-ttu-id="09105-216">Dalším krokem je zřízení virtuálního počítače pro pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="09105-216">The next step is to provision a virtual machine for your StorSimple Virtual Array.</span></span> <span data-ttu-id="09105-217">V závislosti na operačním systému hostitele najdete podrobné pokyny v:</span><span class="sxs-lookup"><span data-stu-id="09105-217">Depending on your host operating system, see the detailed instructions in:</span></span>

* [<span data-ttu-id="09105-218">Zřídit o virtuální zařízení StorSimple pole technologie Hyper-v</span><span class="sxs-lookup"><span data-stu-id="09105-218">Provision a StorSimple Virtual Array in Hyper-V</span></span>](storsimple-virtual-array-deploy2-provision-hyperv.md)
* [<span data-ttu-id="09105-219">Zřídit o virtuální zařízení StorSimple pole v prostředí VMware</span><span class="sxs-lookup"><span data-stu-id="09105-219">Provision a StorSimple Virtual Array in VMware</span></span>](storsimple-virtual-array-deploy2-provision-vmware.md)

