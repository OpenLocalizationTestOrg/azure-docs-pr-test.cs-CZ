---
title: "aaaDeploy služby StorSimple Manager zařízení | Microsoft Docs"
description: "Popisuje, jak toocreate a odstranění hello služby StorSimple Manager zařízení v hello portálu Azure a popisuje, jak toomanage hello registrační klíč služby."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 28499494-8c4d-4a7f-9d44-94b2b8a93c93
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: alkohli
ms.openlocfilehash: 9064053addc7b3dfcce08b47e81b38c2e0e1b559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-storsimple-device-manager-service-for-storsimple-virtual-array"></a><span data-ttu-id="79126-103">Nasazení služby StorSimple Manager zařízení hello pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="79126-103">Deploy hello StorSimple Device Manager service for StorSimple Virtual Array</span></span>
## <a name="overview"></a><span data-ttu-id="79126-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="79126-104">Overview</span></span>

<span data-ttu-id="79126-105">Hello služby StorSimple Manager zařízení běží ve službě Microsoft Azure a připojí zařízení StorSimple toomultiple.</span><span class="sxs-lookup"><span data-stu-id="79126-105">hello StorSimple Device Manager service runs in Microsoft Azure and connects toomultiple StorSimple devices.</span></span> <span data-ttu-id="79126-106">Po vytvoření služby hello, můžete ji použít toomanage hello zařízení z hello Microsoft Azure portálu spuštěnému v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="79126-106">After you create hello service, you can use it toomanage hello devices from hello Microsoft Azure portal running in a browser.</span></span> <span data-ttu-id="79126-107">To vám umožní toomonitor všechna hello zařízení, které jsou připojené toohello Správce zařízení StorSimple služby z jedné centrální umístění, a současně minimalizujete její související administrativní zátěže.</span><span class="sxs-lookup"><span data-stu-id="79126-107">This allows you toomonitor all hello devices that are connected toohello StorSimple Device Manager service from a single, central location, thereby minimizing administrative burden.</span></span>

<span data-ttu-id="79126-108">Hello, běžné úkoly, související, tooa služby StorSimple Manager zařízení jsou:</span><span class="sxs-lookup"><span data-stu-id="79126-108">hello common tasks related tooa StorSimple Device Manager service are:</span></span>

* <span data-ttu-id="79126-109">Vytvoření služby</span><span class="sxs-lookup"><span data-stu-id="79126-109">Create a service</span></span>
* <span data-ttu-id="79126-110">Odstranění služby</span><span class="sxs-lookup"><span data-stu-id="79126-110">Delete a service</span></span>
* <span data-ttu-id="79126-111">Získat registrační klíč služby hello</span><span class="sxs-lookup"><span data-stu-id="79126-111">Get hello service registration key</span></span>
* <span data-ttu-id="79126-112">Znovu vygenerovat registrační klíč služby hello</span><span class="sxs-lookup"><span data-stu-id="79126-112">Regenerate hello service registration key</span></span>

<span data-ttu-id="79126-113">Tento kurz popisuje, jak tooperform z hello předchozí úlohy.</span><span class="sxs-lookup"><span data-stu-id="79126-113">This tutorial describes how tooperform each of hello preceding tasks.</span></span> <span data-ttu-id="79126-114">Hello informace obsažené v tomto článku je použít jenom tooStorSimple virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="79126-114">hello information contained in this article is applicable only tooStorSimple Virtual Arrays.</span></span> <span data-ttu-id="79126-115">Další informace o řady StorSimple 8000 přejděte příliš[nasazení služby StorSimple Manager](storsimple-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="79126-115">For more information on StorSimple 8000 series, go too[deploy a StorSimple Manager service](storsimple-manage-service.md).</span></span>

## <a name="create-a-service"></a><span data-ttu-id="79126-116">Vytvoření služby</span><span class="sxs-lookup"><span data-stu-id="79126-116">Create a service</span></span>

<span data-ttu-id="79126-117">toocreate služby, je třeba toohave:</span><span class="sxs-lookup"><span data-stu-id="79126-117">toocreate a service, you need toohave:</span></span>

* <span data-ttu-id="79126-118">Předplatné s smlouvu Enterprise Agreement</span><span class="sxs-lookup"><span data-stu-id="79126-118">A subscription with an Enterprise Agreement</span></span>
* <span data-ttu-id="79126-119">Aktivní účet úložiště Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="79126-119">An active Microsoft Azure storage account</span></span>
* <span data-ttu-id="79126-120">fakturační informace, které se používá pro správu přístupu Hello</span><span class="sxs-lookup"><span data-stu-id="79126-120">hello billing information that is used for access management</span></span>

<span data-ttu-id="79126-121">Můžete také toogenerate účet úložiště při vytváření služby hello.</span><span class="sxs-lookup"><span data-stu-id="79126-121">You can also choose toogenerate a storage account when you create hello service.</span></span>

<span data-ttu-id="79126-122">Jeden služby můžete spravovat více zařízení.</span><span class="sxs-lookup"><span data-stu-id="79126-122">A single service can manage multiple devices.</span></span> <span data-ttu-id="79126-123">Zařízení, ale nemůžou zahrnovat víc služeb.</span><span class="sxs-lookup"><span data-stu-id="79126-123">However, a device cannot span multiple services.</span></span> <span data-ttu-id="79126-124">Velký podnik může mít více instancí toowork služby pomocí různých předplatných, organizace nebo i umístění nasazení.</span><span class="sxs-lookup"><span data-stu-id="79126-124">A large enterprise can have multiple service instances toowork with different subscriptions, organizations, or even deployment locations.</span></span>

> [!NOTE]
> <span data-ttu-id="79126-125">Je třeba samostatné instance řadu zařízení StorSimple Manager zařízení služby toomanage StorSimple 8000 a pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="79126-125">You need separate instances of StorSimple Device Manager service toomanage StorSimple 8000 series devices and StorSimple Virtual Arrays.</span></span>


<span data-ttu-id="79126-126">Proveďte následující kroky toocreate služby hello.</span><span class="sxs-lookup"><span data-stu-id="79126-126">Perform hello following steps toocreate a service.</span></span>

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

## <a name="delete-a-service"></a><span data-ttu-id="79126-127">Odstranění služby</span><span class="sxs-lookup"><span data-stu-id="79126-127">Delete a service</span></span>

<span data-ttu-id="79126-128">Před odstraněním služby, ujistěte se, že žádné připojená zařízení ji používají.</span><span class="sxs-lookup"><span data-stu-id="79126-128">Before you delete a service, make sure that no connected devices are using it.</span></span> <span data-ttu-id="79126-129">Pokud služba hello se používá, deaktivujte hello připojené zařízení.</span><span class="sxs-lookup"><span data-stu-id="79126-129">If hello service is in use, deactivate hello connected devices.</span></span> <span data-ttu-id="79126-130">Hello deaktivovat operaci severu hello připojení mezi hello zařízení a hello služby, ale zachovat data zařízení hello v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="79126-130">hello deactivate operation will sever hello connection between hello device and hello service, but preserve hello device data in hello cloud.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="79126-131">Po odstranění služby hello operace se nedá vrátit.</span><span class="sxs-lookup"><span data-stu-id="79126-131">After a service is deleted, hello operation cannot be reversed.</span></span> <span data-ttu-id="79126-132">Jakékoli zařízení, které se pomocí služby hello potřebovat toobe tovární nastavení, než se dá použít s jinou službu.</span><span class="sxs-lookup"><span data-stu-id="79126-132">Any device that was using hello service will need toobe factory reset before it can be used with another service.</span></span> <span data-ttu-id="79126-133">V tomto scénáři hello místní data na zařízení hello, jakož i hello konfigurace budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="79126-133">In this scenario, hello local data on hello device, as well as hello configuration, will be lost.</span></span>
 

<span data-ttu-id="79126-134">Proveďte následující kroky toodelete služby hello.</span><span class="sxs-lookup"><span data-stu-id="79126-134">Perform hello following steps toodelete a service.</span></span>

#### <a name="toodelete-a-service"></a><span data-ttu-id="79126-135">toodelete služby</span><span class="sxs-lookup"><span data-stu-id="79126-135">toodelete a service</span></span>

1. <span data-ttu-id="79126-136">Přejděte příliš**všechny prostředky**.</span><span class="sxs-lookup"><span data-stu-id="79126-136">Go too**All resources**.</span></span> <span data-ttu-id="79126-137">Vyhledávání služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="79126-137">Search for your StorSimple Device Manager service.</span></span> <span data-ttu-id="79126-138">Vyberte službu hello chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="79126-138">Select hello service that you wish toodelete.</span></span>
   
    ![Vyberte toodelete služby](./media/storsimple-virtual-array-manage-service/deleteservice2.png)
2. <span data-ttu-id="79126-140">Přejděte tooensure řídicí panel služby tooyour neexistují žádné zařízení připojených toohello služby.</span><span class="sxs-lookup"><span data-stu-id="79126-140">Go tooyour service dashboard tooensure there are no devices connected toohello service.</span></span> <span data-ttu-id="79126-141">Pokud nejsou žádná zařízení registrovaná ve službě, zobrazí se také vliv toohello zprávy informační zpráva.</span><span class="sxs-lookup"><span data-stu-id="79126-141">If there are no devices registered with this service, you will also see a banner message toohello effect.</span></span> <span data-ttu-id="79126-142">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="79126-142">Click **Delete**.</span></span>
   
    ![Odstranění služby](./media/storsimple-virtual-array-manage-service/deleteservice3.png)

3. <span data-ttu-id="79126-144">Po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano** v hello potvrzení oznámení.</span><span class="sxs-lookup"><span data-stu-id="79126-144">When prompted for confirmation, click **Yes** in hello confirmation notification.</span></span> 
   
    ![Potvrdit odstranění služby](./media/storsimple-virtual-array-manage-service/deleteservice4.png)
4. <span data-ttu-id="79126-146">To může trvat několik minut, než toobe služby hello odstranit.</span><span class="sxs-lookup"><span data-stu-id="79126-146">It may take a few minutes for hello service toobe deleted.</span></span> <span data-ttu-id="79126-147">Po hello služba se úspěšně odstranila, budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="79126-147">After hello service is successfully deleted, you will be notified.</span></span>
   
    ![Odstranění úspěšné služby](./media/storsimple-virtual-array-manage-service/deleteservice6.png)

<span data-ttu-id="79126-149">Hello seznam služeb se aktualizují.</span><span class="sxs-lookup"><span data-stu-id="79126-149">hello list of services will be refreshed.</span></span>

 ![Aktualizovaný seznam služeb](./media/storsimple-virtual-array-manage-service/deleteservice7.png)

## <a name="get-hello-service-registration-key"></a><span data-ttu-id="79126-151">Získat registrační klíč služby hello</span><span class="sxs-lookup"><span data-stu-id="79126-151">Get hello service registration key</span></span>
<span data-ttu-id="79126-152">Po úspěšném vytvoření služby budete potřebovat tooregister zařízení StorSimple službou hello.</span><span class="sxs-lookup"><span data-stu-id="79126-152">After you have successfully created a service, you will need tooregister your StorSimple device with hello service.</span></span> <span data-ttu-id="79126-153">tooregister prvního zařízení StorSimple, bude nutné hello registrační klíč služby.</span><span class="sxs-lookup"><span data-stu-id="79126-153">tooregister your first StorSimple device, you will need hello service registration key.</span></span> <span data-ttu-id="79126-154">tooregister další zařízení s existující službu StorSimple, budete potřebovat hello registrační klíč a hello šifrovacího klíče dat služby (který je generován během registrace na první zařízení hello).</span><span class="sxs-lookup"><span data-stu-id="79126-154">tooregister additional devices with an existing StorSimple service, you will need both hello registration key and hello service data encryption key (which is generated on hello first device during registration).</span></span> <span data-ttu-id="79126-155">Další informace o hello šifrovacího klíče dat služby najdete v tématu [zabezpečení zařízení StorSimple](storsimple-security.md).</span><span class="sxs-lookup"><span data-stu-id="79126-155">For more information about hello service data encryption key, see [StorSimple security](storsimple-security.md).</span></span> <span data-ttu-id="79126-156">Hello registrační klíč můžete získat přístup k hello **klíče** okno pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="79126-156">You can get hello registration key by accessing hello **Keys** blade for your service.</span></span>

<span data-ttu-id="79126-157">Proveďte následující kroky tooget hello služby registrační klíč hello.</span><span class="sxs-lookup"><span data-stu-id="79126-157">Perform hello following steps tooget hello service registration key.</span></span>

#### <a name="tooget-hello-service-registration-key"></a><span data-ttu-id="79126-158">registrační klíč služby hello tooget</span><span class="sxs-lookup"><span data-stu-id="79126-158">tooget hello service registration key</span></span>
1. <span data-ttu-id="79126-159">V hello **Manager zařízení StorSimple** okně přejděte příliš**správy &gt;**  **klíče**.</span><span class="sxs-lookup"><span data-stu-id="79126-159">In hello **StorSimple Device Manager** blade, go too**Management &gt;** **Keys**.</span></span>
   
   ![Okno Klíče](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. <span data-ttu-id="79126-161">V hello **klíče** otevře se okno, registrační klíč služby.</span><span class="sxs-lookup"><span data-stu-id="79126-161">In hello **Keys** blade, a service registration key appears.</span></span> <span data-ttu-id="79126-162">Zkopírujte hello registrační klíč pomocí ikona kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="79126-162">Copy hello registration key using hello copy icon.</span></span> 

<span data-ttu-id="79126-163">Registrační klíč služby hello mějte do bezpečného umístění.</span><span class="sxs-lookup"><span data-stu-id="79126-163">Keep hello service registration key in a safe location.</span></span> <span data-ttu-id="79126-164">Budete potřebovat tento klíč, a také šifrovacího klíče dat služby hello, tooregister další zařízení s touto službou.</span><span class="sxs-lookup"><span data-stu-id="79126-164">You will need this key, as well as hello service data encryption key, tooregister additional devices with this service.</span></span> <span data-ttu-id="79126-165">Po získání registračního klíče služby hello, budete potřebovat tooconfigure zařízení prostřednictvím hello Windows Powershellu pro StorSimple rozhraní.</span><span class="sxs-lookup"><span data-stu-id="79126-165">After obtaining hello service registration key, you will need tooconfigure your device through hello Windows PowerShell for StorSimple interface.</span></span>

## <a name="regenerate-hello-service-registration-key"></a><span data-ttu-id="79126-166">Znovu vygenerovat registrační klíč služby hello</span><span class="sxs-lookup"><span data-stu-id="79126-166">Regenerate hello service registration key</span></span>
<span data-ttu-id="79126-167">Je nutné tooregenerate registrační klíč služby, pokud jsou požadované tooperform střídání klíče nebo pokud došlo ke změně hello seznamu správců služeb.</span><span class="sxs-lookup"><span data-stu-id="79126-167">You will need tooregenerate a service registration key if you are required tooperform key rotation or if hello list of service administrators has changed.</span></span> <span data-ttu-id="79126-168">Pokud jste znovu vygenerovat klíč hello, hello nový klíč slouží pouze k registraci dalších zařízení.</span><span class="sxs-lookup"><span data-stu-id="79126-168">When you regenerate hello key, hello new key is used only for registering subsequent devices.</span></span> <span data-ttu-id="79126-169">Hello zařízení, které již byly zaregistrovány nejsou ovlivněny tímto procesem.</span><span class="sxs-lookup"><span data-stu-id="79126-169">hello devices that were already registered are unaffected by this process.</span></span>

<span data-ttu-id="79126-170">Proveďte následující kroky tooregenerate registrační klíč služby hello.</span><span class="sxs-lookup"><span data-stu-id="79126-170">Perform hello following steps tooregenerate a service registration key.</span></span>

#### <a name="tooregenerate-hello-service-registration-key"></a><span data-ttu-id="79126-171">registrační klíč služby hello tooregenerate</span><span class="sxs-lookup"><span data-stu-id="79126-171">tooregenerate hello service registration key</span></span>
1. <span data-ttu-id="79126-172">V hello **Manager zařízení StorSimple** okně přejděte příliš**správy &gt;**  **klíče**.</span><span class="sxs-lookup"><span data-stu-id="79126-172">In hello **StorSimple Device Manager** blade, go too**Management &gt;** **Keys**.</span></span>
   
   ![Okno Klíče](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. <span data-ttu-id="79126-174">V hello **klíče** okně klikněte na tlačítko **znovu vygenerovat**.</span><span class="sxs-lookup"><span data-stu-id="79126-174">In hello **Keys** blade, click **Regenerate**.</span></span>
   
   ![Klikněte na tlačítko znovu vygenerovat](./media/storsimple-virtual-array-manage-service/getregkey5.png)
3. <span data-ttu-id="79126-176">V hello **registrační klíč znovu generovat služby** okně zkontrolujte hello akce při vyžaduje hello klíče se generují.</span><span class="sxs-lookup"><span data-stu-id="79126-176">In hello **Regenerate service registration key** blade, review hello action required when hello keys are regenerated.</span></span> <span data-ttu-id="79126-177">Všechny následné zařízení hello, které jsou registrované s touto službou použije hello nový registrační klíč.</span><span class="sxs-lookup"><span data-stu-id="79126-177">All hello subsequent devices that are registered with this service will use hello new registration key.</span></span> <span data-ttu-id="79126-178">Klikněte na tlačítko **znovu vygenerovat** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="79126-178">Click **Regenerate** tooconfirm.</span></span> <span data-ttu-id="79126-179">Po dokončení registrace hello, budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="79126-179">You will be notified after hello registration is complete.</span></span>
   
   ![Zkontrolujte znovu generovat klíče](./media/storsimple-virtual-array-manage-service/getregkey3.png)
4. <span data-ttu-id="79126-181">Zobrazí se nový registrační klíč služby.</span><span class="sxs-lookup"><span data-stu-id="79126-181">A new service registration key will appear.</span></span>
   
    ![Zkontrolujte znovu generovat klíče](./media/storsimple-virtual-array-manage-service/getregkey4.png)
   
   <span data-ttu-id="79126-183">Zkopírujte tento klíč a uložit ho pro registraci nové zařízení s touto službou.</span><span class="sxs-lookup"><span data-stu-id="79126-183">Copy this key and save it for registering any new devices with this service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="79126-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79126-184">Next steps</span></span>
* <span data-ttu-id="79126-185">Zjistěte, jak příliš[Začínáme](storsimple-virtual-array-deploy1-portal-prep.md) s polem virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="79126-185">Learn how too[get started](storsimple-virtual-array-deploy1-portal-prep.md) with a StorSimple Virtual Array.</span></span>
* <span data-ttu-id="79126-186">Zjistěte, jak příliš[spravovat zařízení StorSimple](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="79126-186">Learn how too[administer your StorSimple device](storsimple-ova-web-ui-admin.md).</span></span>

