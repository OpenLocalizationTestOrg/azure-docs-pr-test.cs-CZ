---
title: "Příprava aaaPortal pro pole virtuální zařízení StorSimple | Microsoft Docs"
description: "První kurz toodeploy pole virtuální zařízení StorSimple spočívá v přípravě hello portálu Azure"
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
ms.openlocfilehash: 5332b235e7296a9274f2e7dafcdf72f4b9cdadf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---prepare-hello-azure-portal"></a><span data-ttu-id="5db4c-103">Nasazení pole virtuálního zařízení StorSimple – Příprava hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5db4c-103">Deploy StorSimple Virtual Array - Prepare hello Azure portal</span></span>

![](./media/storsimple-virtual-array-deploy1-portal-prep/getstarted4.png)
## <a name="overview"></a><span data-ttu-id="5db4c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="5db4c-104">Overview</span></span>

<span data-ttu-id="5db4c-105">Toto je první článek hello v řadě hello kurzy nasazení vyžaduje toocompletely nasadit virtuální pole jako souborový server nebo server se službou iSCSI pomocí modelu Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="5db4c-105">This is hello first article in hello series of deployment tutorials required toocompletely deploy your virtual array as a file server or an iSCSI server using hello Resource Manager model.</span></span> <span data-ttu-id="5db4c-106">Tento článek popisuje hello přípravy požadované toocreate a konfigurace vašeho správce zařízení StorSimple služby předchozí tooprovisioning virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="5db4c-106">This article describes hello preparation required toocreate and configure your StorSimple Device Manager service prior tooprovisioning a virtual array.</span></span> <span data-ttu-id="5db4c-107">Tento článek taky obsahuje odkazy na požadavky konfigurace a kontrolní seznam nasazení konfigurace tooa.</span><span class="sxs-lookup"><span data-stu-id="5db4c-107">This article also links out tooa deployment configuration checklist and configuration prerequisites.</span></span>

<span data-ttu-id="5db4c-108">Je třeba správce oprávnění toocomplete hello procesu instalace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5db4c-108">You need administrator privileges toocomplete hello setup and configuration process.</span></span> <span data-ttu-id="5db4c-109">Doporučujeme, abyste si prošli kontrolní seznam konfigurace nasazení hello před zahájením.</span><span class="sxs-lookup"><span data-stu-id="5db4c-109">We recommend that you review hello deployment configuration checklist before you begin.</span></span> <span data-ttu-id="5db4c-110">Příprava portálu Hello trvá méně než 10 minut.</span><span class="sxs-lookup"><span data-stu-id="5db4c-110">hello portal preparation takes less than 10 minutes.</span></span>

<span data-ttu-id="5db4c-111">informace o Hello publikované v tomto článku se vztahuje toohello nasazení pole virtuální zařízení StorSimple v hello portál Azure a cloudu Microsoft Azure Government.</span><span class="sxs-lookup"><span data-stu-id="5db4c-111">hello information published in this article applies toohello deployment of StorSimple Virtual Arrays in hello Azure portal and Microsoft Azure Government Cloud.</span></span>

### <a name="get-started"></a><span data-ttu-id="5db4c-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="5db4c-112">Get started</span></span>
<span data-ttu-id="5db4c-113">pracovní postup nasazení Hello se skládá z přípravy hello portál, zřizování virtuální pole ve virtualizovaném prostředí a dokončení instalace hello.</span><span class="sxs-lookup"><span data-stu-id="5db4c-113">hello deployment workflow consists of preparing hello portal, provisioning a virtual array in your virtualized environment, and completing hello setup.</span></span> <span data-ttu-id="5db4c-114">tooget začít s hello pole virtuální zařízení StorSimple nasazení jako souborový server nebo server se službou iSCSI, je nutné toorefer toohello následující tabulce prostředky.</span><span class="sxs-lookup"><span data-stu-id="5db4c-114">tooget started with hello StorSimple Virtual Array deployment as a file server or an iSCSI server, you need toorefer toohello following tabulated resources.</span></span>

#### <a name="deployment-articles"></a><span data-ttu-id="5db4c-115">Články nasazení</span><span class="sxs-lookup"><span data-stu-id="5db4c-115">Deployment articles</span></span>

<span data-ttu-id="5db4c-116">toodeploy pole virtuální zařízení StorSimple, najdete v toohello následující články v pořadí určeném hello.</span><span class="sxs-lookup"><span data-stu-id="5db4c-116">toodeploy your StorSimple Virtual Array, refer toohello following articles in hello prescribed sequence.</span></span>

| **#** | <span data-ttu-id="5db4c-117">**V tomto kroku**</span><span class="sxs-lookup"><span data-stu-id="5db4c-117">**In this step**</span></span> | <span data-ttu-id="5db4c-118">**To uděláte...**</span><span class="sxs-lookup"><span data-stu-id="5db4c-118">**You do this …**</span></span> | <span data-ttu-id="5db4c-119">**A použijte tyto dokumenty.**</span><span class="sxs-lookup"><span data-stu-id="5db4c-119">**And use these documents.**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5db4c-120">1.</span><span class="sxs-lookup"><span data-stu-id="5db4c-120">1.</span></span> |<span data-ttu-id="5db4c-121">**Nastavit hello portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="5db4c-121">**Set up hello Azure portal**</span></span> |<span data-ttu-id="5db4c-122">Vytvoření a konfigurace vašeho správce zařízení StorSimple služby předchozí tooprovisioning o pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5db4c-122">Create and configure your StorSimple Device Manager service prior tooprovisioning a StorSimple Virtual Array.</span></span> |[<span data-ttu-id="5db4c-123">Příprava portálu hello</span><span class="sxs-lookup"><span data-stu-id="5db4c-123">Prepare hello portal</span></span>](storsimple-virtual-array-deploy1-portal-prep.md) |
| <span data-ttu-id="5db4c-124">2.</span><span class="sxs-lookup"><span data-stu-id="5db4c-124">2.</span></span> |<span data-ttu-id="5db4c-125">**Zřídit hello virtuální pole**</span><span class="sxs-lookup"><span data-stu-id="5db4c-125">**Provision hello Virtual Array**</span></span> |<span data-ttu-id="5db4c-126">Pro Hyper-V zřídíte a připojit tooa pole virtuální zařízení StorSimple v systému hostitele s technologií Hyper-V v systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="5db4c-126">For Hyper-V, provision and connect tooa StorSimple Virtual Array on a host system running Hyper-V on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2.</span></span> <br></br> <br></br> <span data-ttu-id="5db4c-127">Pro VMware zřídíte a připojit tooa pole virtuální zařízení StorSimple v hostitelském systému, systémem VMware ESXi 5.5 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="5db4c-127">For VMware, provision and connect tooa StorSimple Virtual Array on a host system running VMware ESXi 5.5 and above.</span></span><br></br> |[<span data-ttu-id="5db4c-128">Zřídit virtuální pole Hyper-v</span><span class="sxs-lookup"><span data-stu-id="5db4c-128">Provision a virtual array in Hyper-V</span></span>](storsimple-virtual-array-deploy2-provision-hyperv.md) <br></br> <br></br> [<span data-ttu-id="5db4c-129">Zřídit virtuální pole v prostředí VMware</span><span class="sxs-lookup"><span data-stu-id="5db4c-129">Provision a virtual array in VMware</span></span>](storsimple-virtual-array-deploy2-provision-vmware.md) |
| <span data-ttu-id="5db4c-130">3.</span><span class="sxs-lookup"><span data-stu-id="5db4c-130">3.</span></span> |<span data-ttu-id="5db4c-131">**Nastavit hello virtuální pole**</span><span class="sxs-lookup"><span data-stu-id="5db4c-131">**Set up hello Virtual Array**</span></span> |<span data-ttu-id="5db4c-132">Pro souborový server provedení počáteční instalace, zaregistrujte zařízení StorSimple souborového serveru a dokončení instalace zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="5db4c-132">For your file server, perform initial setup, register your StorSimple file server, and complete hello device setup.</span></span> <span data-ttu-id="5db4c-133">Potom můžete zřídit sdílené složky protokolu SMB.</span><span class="sxs-lookup"><span data-stu-id="5db4c-133">You can then provision SMB shares.</span></span> <br></br> <br></br> <span data-ttu-id="5db4c-134">Pro váš server iSCSI provedení počáteční instalace, zaregistrujte serveru iSCSI StorSimple a dokončení instalace zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="5db4c-134">For your iSCSI server, perform initial setup, register your StorSimple iSCSI server, and complete hello device setup.</span></span> <span data-ttu-id="5db4c-135">Potom můžete zřídit svazky iSCSI.</span><span class="sxs-lookup"><span data-stu-id="5db4c-135">You can then provision iSCSI volumes.</span></span> |[<span data-ttu-id="5db4c-136">Nastavit virtuální pole jako souborový server</span><span class="sxs-lookup"><span data-stu-id="5db4c-136">Set up virtual array as file server</span></span>](storsimple-virtual-array-deploy3-fs-setup.md)<br></br> <br></br>[<span data-ttu-id="5db4c-137">Nastavit virtuální pole jako iSCSI server</span><span class="sxs-lookup"><span data-stu-id="5db4c-137">Set up virtual array as iSCSI server</span></span>](storsimple-virtual-array-deploy3-iscsi-setup.md) |

<span data-ttu-id="5db4c-138">Teď můžete začít tooset až hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5db4c-138">You can now begin tooset up hello Azure portal.</span></span>

## <a name="configuration-checklist"></a><span data-ttu-id="5db4c-139">Kontrolní seznam konfigurace</span><span class="sxs-lookup"><span data-stu-id="5db4c-139">Configuration checklist</span></span>

<span data-ttu-id="5db4c-140">kontrolní seznam konfigurace Hello popisuje hello informace, je nutné toocollect před konfigurací softwaru hello na pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5db4c-140">hello configuration checklist describes hello information that you need toocollect before you configure hello software on your StorSimple Virtual Array.</span></span> <span data-ttu-id="5db4c-141">Příprava tyto informace před čas pomáhá zjednodušit hello proces nasazení zařízení StorSimple hello ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="5db4c-141">Preparing this information ahead of time helps streamline hello process of deploying hello StorSimple device in your environment.</span></span> <span data-ttu-id="5db4c-142">V závislosti na tom, jestli je vaše pole virtuální zařízení StorSimple nasazený jako souborový server nebo server se službou iSCSI, budete potřebovat hello následující kontrolní seznamy.</span><span class="sxs-lookup"><span data-stu-id="5db4c-142">Depending upon whether your StorSimple Virtual Array is deployed as a file server or an iSCSI server, you need one of hello following checklists.</span></span>

* <span data-ttu-id="5db4c-143">Stáhnout hello [StorSimple virtuální pole souboru serveru kontrolní seznam konfigurace](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).</span><span class="sxs-lookup"><span data-stu-id="5db4c-143">Download hello [StorSimple Virtual Array File Server Configuration Checklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).</span></span>
* <span data-ttu-id="5db4c-144">Stáhnout hello [pole virtuální zařízení StorSimple iSCSI kontrolní seznam konfigurace serveru](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).</span><span class="sxs-lookup"><span data-stu-id="5db4c-144">Download hello [StorSimple Virtual Array iSCSI Server Configuration Checklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5db4c-145">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5db4c-145">Prerequisites</span></span>

<span data-ttu-id="5db4c-146">Zde zjistíte hello požadavky konfigurace služby StorSimple Manager zařízení, pole virtuální zařízení StorSimple a hello síti datového centra.</span><span class="sxs-lookup"><span data-stu-id="5db4c-146">Here you find hello configuration prerequisites for your StorSimple Device Manager service, your StorSimple Virtual Array, and hello datacenter network.</span></span>

### <a name="for-hello-storsimple-device-manager-service"></a><span data-ttu-id="5db4c-147">Pro hello služby StorSimple Manager zařízení</span><span class="sxs-lookup"><span data-stu-id="5db4c-147">For hello StorSimple Device Manager service</span></span>

<span data-ttu-id="5db4c-148">Než začnete, ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="5db4c-148">Before you begin, make sure that:</span></span>

* <span data-ttu-id="5db4c-149">Máte účet Microsoft a přihlašovací údaje účtu.</span><span class="sxs-lookup"><span data-stu-id="5db4c-149">You have your Microsoft account with access credentials.</span></span>
* <span data-ttu-id="5db4c-150">Máte účet služby Microsoft Azure Storage a přihlašovací údaje účtu.</span><span class="sxs-lookup"><span data-stu-id="5db4c-150">You have your Microsoft Azure storage account with access credentials.</span></span>
* <span data-ttu-id="5db4c-151">Vaše předplatné Microsoft Azure by měl povolit pro službu StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="5db4c-151">Your Microsoft Azure subscription should be enabled for StorSimple Device Manager service.</span></span>

### <a name="for-hello-storsimple-virtual-array"></a><span data-ttu-id="5db4c-152">Pro hello pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="5db4c-152">For hello StorSimple Virtual Array</span></span>

<span data-ttu-id="5db4c-153">Před nasazením virtuální pole, ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="5db4c-153">Before you deploy a virtual array, make sure that:</span></span>

* <span data-ttu-id="5db4c-154">Máte přístup tooa hostitelský systém s technologií Hyper-V v systému Windows Server 2008 R2 nebo novější nebo VMware (ESXi 5.5 nebo novější), které můžou být použité tooa zřízení zařízení.</span><span class="sxs-lookup"><span data-stu-id="5db4c-154">You have access tooa host system running Hyper-V on Windows Server 2008 R2 or later or VMware (ESXi 5.5 or later) that can be used tooa provision a device.</span></span>
* <span data-ttu-id="5db4c-155">Hello systém hostitele je možné toodedicate hello následující prostředky tooprovision virtuální pole:</span><span class="sxs-lookup"><span data-stu-id="5db4c-155">hello host system is able toodedicate hello following resources tooprovision your virtual array:</span></span>
  
  * <span data-ttu-id="5db4c-156">Minimálně 4 jádra.</span><span class="sxs-lookup"><span data-stu-id="5db4c-156">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="5db4c-157">Alespoň 8 GB paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="5db4c-157">At least 8 GB of RAM.</span></span> <span data-ttu-id="5db4c-158">Pokud máte v plánu tooconfigure hello virtuální pole jako souborový server, 8 GB podporuje 2 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="5db4c-158">If you plan tooconfigure hello virtual array as file server, 8 GB supports 2 million files.</span></span> <span data-ttu-id="5db4c-159">Je nutné 16 GB paměti RAM toosupport 2 – 4 miliony souborů.</span><span class="sxs-lookup"><span data-stu-id="5db4c-159">You need 16 GB RAM toosupport 2 - 4 million files.</span></span>
  * <span data-ttu-id="5db4c-160">Jedno síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5db4c-160">One network interface.</span></span>
  * <span data-ttu-id="5db4c-161">500 GB virtuální disk pro data systému.</span><span class="sxs-lookup"><span data-stu-id="5db4c-161">A 500 GB virtual disk for system data.</span></span>

### <a name="for-hello-datacenter-network"></a><span data-ttu-id="5db4c-162">Pro síť datového centra hello</span><span class="sxs-lookup"><span data-stu-id="5db4c-162">For hello datacenter network</span></span>

<span data-ttu-id="5db4c-163">Než začnete, ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="5db4c-163">Before you begin, make sure that:</span></span>

* <span data-ttu-id="5db4c-164">podle hello síťové požadavky pro zařízení StorSimple je nakonfigurován Hello sítě ve vašem datovém centru.</span><span class="sxs-lookup"><span data-stu-id="5db4c-164">hello network in your datacenter is configured as per hello networking requirements for your StorSimple device.</span></span> <span data-ttu-id="5db4c-165">Další informace najdete v tématu hello [StorSimple virtuální požadavky na systém pole](storsimple-ova-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="5db4c-165">For more information, see hello [StorSimple Virtual Array System Requirements](storsimple-ova-system-requirements.md).</span></span>
* <span data-ttu-id="5db4c-166">Pole virtuální zařízení StorSimple nemá vyhrazené 5 MB/s šířky pásma Internetu (nebo má více) vždy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="5db4c-166">Your StorSimple Virtual Array has a dedicated 5 Mbps Internet bandwidth (or more) available at all times.</span></span> <span data-ttu-id="5db4c-167">Tato šířky pásma nesmí sdílet s jinými aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="5db4c-167">This bandwidth should not be shared with any other applications.</span></span>

## <a name="step-by-step-preparation"></a><span data-ttu-id="5db4c-168">Krok za krokem přípravy</span><span class="sxs-lookup"><span data-stu-id="5db4c-168">Step-by-step preparation</span></span>

<span data-ttu-id="5db4c-169">Pomocí portálu hello následující tooprepare podrobné pokyny pro hello služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="5db4c-169">Use hello following step-by-step instructions tooprepare your portal for hello StorSimple Device Manager service.</span></span>

## <a name="step-1-create-a-new-service"></a><span data-ttu-id="5db4c-170">Krok 1: Vytvoření nové služby</span><span class="sxs-lookup"><span data-stu-id="5db4c-170">Step 1: Create a new service</span></span>

<span data-ttu-id="5db4c-171">Jedna instance služby StorSimple Manager zařízení hello můžete spravovat několik polí virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5db4c-171">A single instance of hello StorSimple Device Manager service can manage multiple StorSimple Virtual Arrays.</span></span> <span data-ttu-id="5db4c-172">Proveďte následující kroky toocreate instanci služby StorSimple Manager zařízení hello hello.</span><span class="sxs-lookup"><span data-stu-id="5db4c-172">Perform hello following steps toocreate an instance of hello StorSimple Device Manager service.</span></span> <span data-ttu-id="5db4c-173">Pokud máte vaše virtuální pole stávající služby toomanage Správce zařízení StorSimple, tento krok přeskočte a přejděte příliš[krok 2: registrační klíč služby hello Get](#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="5db4c-173">If you have an existing StorSimple Device Manager service toomanage your virtual arrays, skip this step and go too[Step 2: Get hello service registration key](#step-2-get-the-service-registration-key).</span></span>

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

> [!IMPORTANT]
> <span data-ttu-id="5db4c-174">Pokud jste nepovolili automatické vytvoření účtu úložiště hello s službou, budete potřebovat toocreate alespoň jeden účet úložiště po úspěšném vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="5db4c-174">If you did not enable hello automatic creation of a storage account with your service, you will need toocreate at least one storage account after you have successfully created a service.</span></span>
> 
> * <span data-ttu-id="5db4c-175">Pokud nebyl vytvořen účet úložiště automaticky, přejděte příliš[konfigurace nového účtu úložiště pro službu hello](#optional-step-configure-a-new-storage-account-for-the-service) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="5db4c-175">If you did not create a storage account automatically, go too[Configure a new storage account for hello service](#optional-step-configure-a-new-storage-account-for-the-service) for detailed instructions.</span></span>
> * <span data-ttu-id="5db4c-176">Pokud jste povolili hello automatické vytvoření účtu úložiště, pokračujte příliš[krok 2: registrační klíč služby hello Get](#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="5db4c-176">If you enabled hello automatic creation of a storage account, go too[Step 2: Get hello service registration key](#step-2-get-the-service-registration-key).</span></span>
> 
> 

## <a name="step-2-get-hello-service-registration-key"></a><span data-ttu-id="5db4c-177">Krok 2: Získání registračního klíče služby hello</span><span class="sxs-lookup"><span data-stu-id="5db4c-177">Step 2: Get hello service registration key</span></span>

<span data-ttu-id="5db4c-178">Po hello služby StorSimple Manager zařízení je spuštěný a funkční, budete potřebovat registrační klíč služby hello tooget.</span><span class="sxs-lookup"><span data-stu-id="5db4c-178">After hello StorSimple Device Manager service is up and running, you will need tooget hello service registration key.</span></span> <span data-ttu-id="5db4c-179">Tento klíč je použité tooregister a připojení zařízení StorSimple službou hello.</span><span class="sxs-lookup"><span data-stu-id="5db4c-179">This key is used tooregister and connect your StorSimple device with hello service.</span></span>

<span data-ttu-id="5db4c-180">Proveďte následující kroky v hello hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5db4c-180">Perform hello following steps in hello [Azure portal](https://portal.azure.com/).</span></span>

[!INCLUDE [storsimple-virtual-array-get-service-registration-key](../../includes/storsimple-virtual-array-get-service-registration-key.md)]

> [!NOTE]
> <span data-ttu-id="5db4c-181">Hello registrační klíč služby je použité tooregister všechny hello zařízení StorSimple Manager zařízení, která vyžadují tooregister s služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="5db4c-181">hello service registration key is used tooregister all hello StorSimple Device Manager devices that need tooregister with your StorSimple Device Manager service.</span></span>
> 
> 

## <a name="step-3-download-hello-virtual-array-image"></a><span data-ttu-id="5db4c-182">Krok 3: Stáhnout bitovou kopii virtuálního pole hello</span><span class="sxs-lookup"><span data-stu-id="5db4c-182">Step 3: Download hello virtual array image</span></span>

<span data-ttu-id="5db4c-183">Až budete mít registrační klíč služby hello, budete potřebovat toodownload hello příslušné virtuální pole image tooprovision virtuální pole v systému hostitele.</span><span class="sxs-lookup"><span data-stu-id="5db4c-183">After you have hello service registration key, you will need toodownload hello appropriate virtual array image tooprovision a virtual array on your host system.</span></span> <span data-ttu-id="5db4c-184">Hello virtuální pole Image jsou specifické pro operační systém a lze ji stáhnout ze stránky rychlý Start hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5db4c-184">hello virtual array images are operating system specific and can be downloaded from hello Quick Start page in hello Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5db4c-185">Hello software spuštěný na hello pole virtuální zařízení StorSimple lze použít pouze s hello služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="5db4c-185">hello software running on hello StorSimple Virtual Array may only be used with hello StorSimple Device Manager service.</span></span>
> 
> 

<span data-ttu-id="5db4c-186">Proveďte následující kroky v hello hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5db4c-186">Perform hello following steps in hello [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="tooget-hello-virtual-array-image"></a><span data-ttu-id="5db4c-187">Obrázek virtuální pole tooget hello</span><span class="sxs-lookup"><span data-stu-id="5db4c-187">tooget hello virtual array image</span></span>

1. <span data-ttu-id="5db4c-188">Přihlaste se k hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5db4c-188">Sign into hello [Azure portal](https://portal.azure.com/).</span></span> 
2. <span data-ttu-id="5db4c-189">V hello portálu Azure, klikněte na **procházet > Správci zařízení StorSimple**.</span><span class="sxs-lookup"><span data-stu-id="5db4c-189">In hello Azure portal, click **Browse > StorSimple Device Managers**.</span></span>
3. <span data-ttu-id="5db4c-190">Vyberte existující službu StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="5db4c-190">Select an existing StorSimple Device Manager service.</span></span> <span data-ttu-id="5db4c-191">V hello **Manager zařízení StorSimple** okně klikněte na tlačítko **rychlý Start**.</span><span class="sxs-lookup"><span data-stu-id="5db4c-191">In hello **StorSimple Device Manager** blade, click **Quick Start**.</span></span> 
4. <span data-ttu-id="5db4c-192">Klikněte na tlačítko hello odkaz odpovídající toohello bitové kopie, které chcete toodownload z hello Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="5db4c-192">Click hello link corresponding toohello image that you want toodownload from hello Microsoft Download Center.</span></span> <span data-ttu-id="5db4c-193">soubory Hello image jsou přibližně 4,8 GB.</span><span class="sxs-lookup"><span data-stu-id="5db4c-193">hello image files are approximately 4.8 GB.</span></span>
   
   * <span data-ttu-id="5db4c-194">VHDX pro Hyper-V v systému Windows Server 2012 a novější</span><span class="sxs-lookup"><span data-stu-id="5db4c-194">VHDX for Hyper-V on Windows Server 2012 and later</span></span>
   * <span data-ttu-id="5db4c-195">Virtuální pevný disk pro technologii Hyper-V v systému Windows Server 2008 R2 a novější</span><span class="sxs-lookup"><span data-stu-id="5db4c-195">VHD for Hyper-V on Windows Server 2008 R2 and later</span></span>
   * <span data-ttu-id="5db4c-196">VMDK pro VMWare ESXi 5.5 a novější</span><span class="sxs-lookup"><span data-stu-id="5db4c-196">VMDK for VMWare ESXi 5.5 and later</span></span>
5. <span data-ttu-id="5db4c-197">Stáhněte a rozbalte hello souboru tooa místní disk, provádění poznamenejte si kde je umístěn soubor rozbalené hello.</span><span class="sxs-lookup"><span data-stu-id="5db4c-197">Download and unzip hello file tooa local drive, making a note of where hello unzipped file is located.</span></span>

## <a name="optional-step-configure-a-new-storage-account-for-hello-service"></a><span data-ttu-id="5db4c-198">Volitelný krok: Konfigurace nového účtu úložiště pro službu hello</span><span class="sxs-lookup"><span data-stu-id="5db4c-198">Optional step: Configure a new storage account for hello service</span></span>

<span data-ttu-id="5db4c-199">Tento krok je volitelný a musí provést pouze v případě, že jste nepovolili automatické vytvoření účtu úložiště hello s vaší služby.</span><span class="sxs-lookup"><span data-stu-id="5db4c-199">This step is optional and should be performed only if you did not enable hello automatic creation of a storage account with your service.</span></span>

<span data-ttu-id="5db4c-200">Pokud potřebujete toocreate účet úložiště Azure v jiné oblasti, přečtěte si [jak toocreate účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="5db4c-200">If you need toocreate an Azure storage account in a different region, see [How toocreate a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) for step-by-step instructions.</span></span>

<span data-ttu-id="5db4c-201">Proveďte následující kroky v hello hello [portál Azure](https://ms.portal.azure.com/) na hello Správce zařízení StorSimple služby stránky tooadd stávající účet úložiště Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="5db4c-201">Perform hello following steps in hello [Azure portal](https://ms.portal.azure.com/) on hello StorSimple Device Manager service page tooadd an existing Microsoft Azure storage account.</span></span>

#### <a name="tooadd-a-storage-account-credential-that-has-hello-same-azure-subscription-as-hello-device-manager-service"></a><span data-ttu-id="5db4c-202">hello tooadd pověření účtu úložiště, který má stejné předplatné jako služba hello Správce zařízení</span><span class="sxs-lookup"><span data-stu-id="5db4c-202">tooadd a storage account credential that has hello same Azure subscription as hello Device Manager service</span></span>

1. <span data-ttu-id="5db4c-203">Přejděte tooyour služby Správce zařízení, vyberte a dvakrát na ni klikněte.</span><span class="sxs-lookup"><span data-stu-id="5db4c-203">Navigate tooyour Device Manager service, select and double-click it.</span></span> <span data-ttu-id="5db4c-204">Tím se otevře hello **přehled** okno.</span><span class="sxs-lookup"><span data-stu-id="5db4c-204">This opens hello **Overview** blade.</span></span>
2. <span data-ttu-id="5db4c-205">Vyberte **přihlašovacích údajů účtu úložiště** v rámci hello **konfigurace** části.</span><span class="sxs-lookup"><span data-stu-id="5db4c-205">Select **Storage account credentials** within hello **Configuration** section.</span></span>
3. <span data-ttu-id="5db4c-206">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="5db4c-206">Click **Add**.</span></span>
4. <span data-ttu-id="5db4c-207">V hello **přidání účtu úložiště** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="5db4c-207">In hello **Add a storage account** blade, do hello following:</span></span>
   
    1. <span data-ttu-id="5db4c-208">Pro **předplatné**, vyberte **aktuální**.</span><span class="sxs-lookup"><span data-stu-id="5db4c-208">For **Subscription**, select **Current**.</span></span>
   
    2. <span data-ttu-id="5db4c-209">Zadejte název hello svého účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="5db4c-209">Provide hello name of your Azure storage account.</span></span>
   
    3. <span data-ttu-id="5db4c-210">Vyberte **povolit** toocreate zabezpečený kanál pro síťovou komunikaci mezi StorSimple zařízení a hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="5db4c-210">Select **Enable** toocreate a secure channel for network communication between your StorSimple device and hello cloud.</span></span> <span data-ttu-id="5db4c-211">Vyberte **zakázat** pouze v případě, že pracujete v privátním cloudu.</span><span class="sxs-lookup"><span data-stu-id="5db4c-211">Select **Disable** only if you are operating within a private cloud.</span></span>
   
    4. <span data-ttu-id="5db4c-212">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="5db4c-212">Click **Add**.</span></span> <span data-ttu-id="5db4c-213">Upozornění se zobrazí po úspěšném vytvoření účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="5db4c-213">You are notified after hello storage account is successfully created.</span></span><br></br>
   
     ![Přidat existující přihlašovací údaje účtu úložiště](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

## <a name="next-step"></a><span data-ttu-id="5db4c-215">Další krok</span><span class="sxs-lookup"><span data-stu-id="5db4c-215">Next step</span></span>

<span data-ttu-id="5db4c-216">dalším krokem Hello je tooprovision virtuálního počítače pro pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5db4c-216">hello next step is tooprovision a virtual machine for your StorSimple Virtual Array.</span></span> <span data-ttu-id="5db4c-217">V závislosti na operačním systému hostitele najdete podrobné pokyny v hello:</span><span class="sxs-lookup"><span data-stu-id="5db4c-217">Depending on your host operating system, see hello detailed instructions in:</span></span>

* [<span data-ttu-id="5db4c-218">Zřídit o virtuální zařízení StorSimple pole technologie Hyper-v</span><span class="sxs-lookup"><span data-stu-id="5db4c-218">Provision a StorSimple Virtual Array in Hyper-V</span></span>](storsimple-virtual-array-deploy2-provision-hyperv.md)
* [<span data-ttu-id="5db4c-219">Zřídit o virtuální zařízení StorSimple pole v prostředí VMware</span><span class="sxs-lookup"><span data-stu-id="5db4c-219">Provision a StorSimple Virtual Array in VMware</span></span>](storsimple-virtual-array-deploy2-provision-vmware.md)

