---
title: "Nasazení služby Správce zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak vytvářet a odstraňovat služby StorSimple Manager zařízení na portálu Azure a popisuje, jak spravovat registrační klíč služby."
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
ms.openlocfilehash: 1881a0625b107ae1a90e5b772f5296a4d728973d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-virtual-array"></a><span data-ttu-id="c0354-103">Nasazení služby StorSimple Manager zařízení StorSimple virtuální pole</span><span class="sxs-lookup"><span data-stu-id="c0354-103">Deploy the StorSimple Device Manager service for StorSimple Virtual Array</span></span>
## <a name="overview"></a><span data-ttu-id="c0354-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="c0354-104">Overview</span></span>

<span data-ttu-id="c0354-105">Služba Správce zařízení StorSimple běží v Microsoft Azure a připojí k více zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c0354-105">The StorSimple Device Manager service runs in Microsoft Azure and connects to multiple StorSimple devices.</span></span> <span data-ttu-id="c0354-106">Po vytvoření služby, můžete ke správě zařízení z portálu Microsoft Azure spuštěnému v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c0354-106">After you create the service, you can use it to manage the devices from the Microsoft Azure portal running in a browser.</span></span> <span data-ttu-id="c0354-107">To vám umožňuje monitorovat všechna zařízení, které jsou připojené ke službě StorSimple Manager zařízení z jedné centrální umístění, a současně minimalizujete její související administrativní zátěže.</span><span class="sxs-lookup"><span data-stu-id="c0354-107">This allows you to monitor all the devices that are connected to the StorSimple Device Manager service from a single, central location, thereby minimizing administrative burden.</span></span>

<span data-ttu-id="c0354-108">Běžné úlohy související s služby StorSimple Manager zařízení jsou:</span><span class="sxs-lookup"><span data-stu-id="c0354-108">The common tasks related to a StorSimple Device Manager service are:</span></span>

* <span data-ttu-id="c0354-109">Vytvoření služby</span><span class="sxs-lookup"><span data-stu-id="c0354-109">Create a service</span></span>
* <span data-ttu-id="c0354-110">Odstranění služby</span><span class="sxs-lookup"><span data-stu-id="c0354-110">Delete a service</span></span>
* <span data-ttu-id="c0354-111">Získání registračního klíče služby</span><span class="sxs-lookup"><span data-stu-id="c0354-111">Get the service registration key</span></span>
* <span data-ttu-id="c0354-112">Znovu vygenerovat registrační klíč služby</span><span class="sxs-lookup"><span data-stu-id="c0354-112">Regenerate the service registration key</span></span>

<span data-ttu-id="c0354-113">Tento kurz popisuje, jak provést jednotlivé předchozí úlohy.</span><span class="sxs-lookup"><span data-stu-id="c0354-113">This tutorial describes how to perform each of the preceding tasks.</span></span> <span data-ttu-id="c0354-114">Informace obsažené v tomto článku se vztahuje pouze na pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c0354-114">The information contained in this article is applicable only to StorSimple Virtual Arrays.</span></span> <span data-ttu-id="c0354-115">Další informace o řady StorSimple 8000, přejděte na [nasazení služby StorSimple Manager](storsimple-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="c0354-115">For more information on StorSimple 8000 series, go to [deploy a StorSimple Manager service](storsimple-manage-service.md).</span></span>

## <a name="create-a-service"></a><span data-ttu-id="c0354-116">Vytvoření služby</span><span class="sxs-lookup"><span data-stu-id="c0354-116">Create a service</span></span>

<span data-ttu-id="c0354-117">Pokud chcete vytvořit službu, musíte mít:</span><span class="sxs-lookup"><span data-stu-id="c0354-117">To create a service, you need to have:</span></span>

* <span data-ttu-id="c0354-118">Předplatné s smlouvu Enterprise Agreement</span><span class="sxs-lookup"><span data-stu-id="c0354-118">A subscription with an Enterprise Agreement</span></span>
* <span data-ttu-id="c0354-119">Aktivní účet úložiště Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="c0354-119">An active Microsoft Azure storage account</span></span>
* <span data-ttu-id="c0354-120">Fakturační informace, které se používá pro správu přístupu</span><span class="sxs-lookup"><span data-stu-id="c0354-120">The billing information that is used for access management</span></span>

<span data-ttu-id="c0354-121">Můžete také vygenerovat účet úložiště při vytváření služby.</span><span class="sxs-lookup"><span data-stu-id="c0354-121">You can also choose to generate a storage account when you create the service.</span></span>

<span data-ttu-id="c0354-122">Jeden služby můžete spravovat více zařízení.</span><span class="sxs-lookup"><span data-stu-id="c0354-122">A single service can manage multiple devices.</span></span> <span data-ttu-id="c0354-123">Zařízení, ale nemůžou zahrnovat víc služeb.</span><span class="sxs-lookup"><span data-stu-id="c0354-123">However, a device cannot span multiple services.</span></span> <span data-ttu-id="c0354-124">Velký podnik může mít víc instancí služby pro práci s různých předplatných, organizace nebo i umístění nasazení.</span><span class="sxs-lookup"><span data-stu-id="c0354-124">A large enterprise can have multiple service instances to work with different subscriptions, organizations, or even deployment locations.</span></span>

> [!NOTE]
> <span data-ttu-id="c0354-125">Je třeba samostatné instance služby StorSimple Manager zařízení ke správě zařízení řady StorSimple 8000 a pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c0354-125">You need separate instances of StorSimple Device Manager service to manage StorSimple 8000 series devices and StorSimple Virtual Arrays.</span></span>


<span data-ttu-id="c0354-126">Proveďte následující kroky k vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="c0354-126">Perform the following steps to create a service.</span></span>

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

## <a name="delete-a-service"></a><span data-ttu-id="c0354-127">Odstranění služby</span><span class="sxs-lookup"><span data-stu-id="c0354-127">Delete a service</span></span>

<span data-ttu-id="c0354-128">Před odstraněním služby, ujistěte se, že žádné připojená zařízení ji používají.</span><span class="sxs-lookup"><span data-stu-id="c0354-128">Before you delete a service, make sure that no connected devices are using it.</span></span> <span data-ttu-id="c0354-129">Pokud služba se používá, deaktivujte připojená zařízení.</span><span class="sxs-lookup"><span data-stu-id="c0354-129">If the service is in use, deactivate the connected devices.</span></span> <span data-ttu-id="c0354-130">Operaci deaktivovat severu připojení mezi zařízením a služby, ale zachovat data zařízení v cloudu.</span><span class="sxs-lookup"><span data-stu-id="c0354-130">The deactivate operation will sever the connection between the device and the service, but preserve the device data in the cloud.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c0354-131">Po odstranění služby nelze vrátit operaci zpět.</span><span class="sxs-lookup"><span data-stu-id="c0354-131">After a service is deleted, the operation cannot be reversed.</span></span> <span data-ttu-id="c0354-132">Jakékoli zařízení, které se pomocí služby bude muset být tovární nastavení, než se dá použít s jinou službu.</span><span class="sxs-lookup"><span data-stu-id="c0354-132">Any device that was using the service will need to be factory reset before it can be used with another service.</span></span> <span data-ttu-id="c0354-133">V tomto scénáři místní data na zařízení, jakož i konfigurace, budou ztracena.</span><span class="sxs-lookup"><span data-stu-id="c0354-133">In this scenario, the local data on the device, as well as the configuration, will be lost.</span></span>
 

<span data-ttu-id="c0354-134">Proveďte následující kroky pro odstranění služby.</span><span class="sxs-lookup"><span data-stu-id="c0354-134">Perform the following steps to delete a service.</span></span>

#### <a name="to-delete-a-service"></a><span data-ttu-id="c0354-135">Chcete-li odstranit služby</span><span class="sxs-lookup"><span data-stu-id="c0354-135">To delete a service</span></span>

1. <span data-ttu-id="c0354-136">Přejděte na **všechny prostředky**.</span><span class="sxs-lookup"><span data-stu-id="c0354-136">Go to **All resources**.</span></span> <span data-ttu-id="c0354-137">Vyhledávání služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="c0354-137">Search for your StorSimple Device Manager service.</span></span> <span data-ttu-id="c0354-138">Vyberte službu, kterou chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="c0354-138">Select the service that you wish to delete.</span></span>
   
    ![Vyberte službu odstranit](./media/storsimple-virtual-array-manage-service/deleteservice2.png)
2. <span data-ttu-id="c0354-140">Přejděte na řídicí panel služby zajistit nejsou žádná zařízení připojené ke službě.</span><span class="sxs-lookup"><span data-stu-id="c0354-140">Go to your service dashboard to ensure there are no devices connected to the service.</span></span> <span data-ttu-id="c0354-141">Pokud nejsou žádná zařízení registrovaná ve službě, zobrazí se také zpráva hlavičky v tom smyslu.</span><span class="sxs-lookup"><span data-stu-id="c0354-141">If there are no devices registered with this service, you will also see a banner message to the effect.</span></span> <span data-ttu-id="c0354-142">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c0354-142">Click **Delete**.</span></span>
   
    ![Odstranění služby](./media/storsimple-virtual-array-manage-service/deleteservice3.png)

3. <span data-ttu-id="c0354-144">Po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano** v potvrzení oznámení.</span><span class="sxs-lookup"><span data-stu-id="c0354-144">When prompted for confirmation, click **Yes** in the confirmation notification.</span></span> 
   
    ![Potvrdit odstranění služby](./media/storsimple-virtual-array-manage-service/deleteservice4.png)
4. <span data-ttu-id="c0354-146">To může trvat několik minut, než službu, kterou chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="c0354-146">It may take a few minutes for the service to be deleted.</span></span> <span data-ttu-id="c0354-147">Po odstranění služby je úspěšně, budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="c0354-147">After the service is successfully deleted, you will be notified.</span></span>
   
    ![Odstranění úspěšné služby](./media/storsimple-virtual-array-manage-service/deleteservice6.png)

<span data-ttu-id="c0354-149">Seznam služeb se aktualizují.</span><span class="sxs-lookup"><span data-stu-id="c0354-149">The list of services will be refreshed.</span></span>

 ![Aktualizovaný seznam služeb](./media/storsimple-virtual-array-manage-service/deleteservice7.png)

## <a name="get-the-service-registration-key"></a><span data-ttu-id="c0354-151">Získání registračního klíče služby</span><span class="sxs-lookup"><span data-stu-id="c0354-151">Get the service registration key</span></span>
<span data-ttu-id="c0354-152">Po úspěšném vytvoření služby musíte registrace zařízení StorSimple pomocí služby.</span><span class="sxs-lookup"><span data-stu-id="c0354-152">After you have successfully created a service, you will need to register your StorSimple device with the service.</span></span> <span data-ttu-id="c0354-153">K registraci svého prvního zařízení StorSimple, budete potřebovat registrační klíč služby.</span><span class="sxs-lookup"><span data-stu-id="c0354-153">To register your first StorSimple device, you will need the service registration key.</span></span> <span data-ttu-id="c0354-154">K registraci dalších zařízení s existující službu StorSimple, budete potřebovat registrační klíč a služby datového šifrovacího klíče (což je vygenerovaný na první zařízení během registrace).</span><span class="sxs-lookup"><span data-stu-id="c0354-154">To register additional devices with an existing StorSimple service, you will need both the registration key and the service data encryption key (which is generated on the first device during registration).</span></span> <span data-ttu-id="c0354-155">Další informace o šifrovacího klíče dat služby najdete v tématu [zabezpečení zařízení StorSimple](storsimple-security.md).</span><span class="sxs-lookup"><span data-stu-id="c0354-155">For more information about the service data encryption key, see [StorSimple security](storsimple-security.md).</span></span> <span data-ttu-id="c0354-156">Abyste mohli získat registrační klíč přístup k **klíče** okno pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="c0354-156">You can get the registration key by accessing the **Keys** blade for your service.</span></span>

<span data-ttu-id="c0354-157">Proveďte následující kroky k získání registračního klíče služby.</span><span class="sxs-lookup"><span data-stu-id="c0354-157">Perform the following steps to get the service registration key.</span></span>

#### <a name="to-get-the-service-registration-key"></a><span data-ttu-id="c0354-158">Chcete-li získat registrační klíč služby</span><span class="sxs-lookup"><span data-stu-id="c0354-158">To get the service registration key</span></span>
1. <span data-ttu-id="c0354-159">V **Manager zařízení StorSimple** okno, přejděte na **správy &gt;**  **klíče**.</span><span class="sxs-lookup"><span data-stu-id="c0354-159">In the **StorSimple Device Manager** blade, go to **Management &gt;** **Keys**.</span></span>
   
   ![Okno Klíče](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. <span data-ttu-id="c0354-161">V **klíče** otevře se okno, registrační klíč služby.</span><span class="sxs-lookup"><span data-stu-id="c0354-161">In the **Keys** blade, a service registration key appears.</span></span> <span data-ttu-id="c0354-162">Zkopírujte registrační klíč pomocí ikonu kopírování.</span><span class="sxs-lookup"><span data-stu-id="c0354-162">Copy the registration key using the copy icon.</span></span> 

<span data-ttu-id="c0354-163">Zachovat registrační klíč služby do bezpečného umístění.</span><span class="sxs-lookup"><span data-stu-id="c0354-163">Keep the service registration key in a safe location.</span></span> <span data-ttu-id="c0354-164">Budete potřebovat tento klíč, jakož i služby datový šifrovací klíč k registraci dalších zařízení s touto službou.</span><span class="sxs-lookup"><span data-stu-id="c0354-164">You will need this key, as well as the service data encryption key, to register additional devices with this service.</span></span> <span data-ttu-id="c0354-165">Po získání registračního klíče služby, musíte nakonfigurovat zařízení pomocí Windows Powershellu pro StorSimple rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c0354-165">After obtaining the service registration key, you will need to configure your device through the Windows PowerShell for StorSimple interface.</span></span>

## <a name="regenerate-the-service-registration-key"></a><span data-ttu-id="c0354-166">Znovu vygenerovat registrační klíč služby</span><span class="sxs-lookup"><span data-stu-id="c0354-166">Regenerate the service registration key</span></span>
<span data-ttu-id="c0354-167">Musíte se znova vygenerovat registrační klíč služby, pokud je nutné provést střídání klíče, nebo pokud došlo ke změně seznamu správců služeb.</span><span class="sxs-lookup"><span data-stu-id="c0354-167">You will need to regenerate a service registration key if you are required to perform key rotation or if the list of service administrators has changed.</span></span> <span data-ttu-id="c0354-168">Pokud jste znovu vygenerovat klíč, nový klíč slouží pouze k registraci dalších zařízení.</span><span class="sxs-lookup"><span data-stu-id="c0354-168">When you regenerate the key, the new key is used only for registering subsequent devices.</span></span> <span data-ttu-id="c0354-169">Zařízení, která již byla zaregistrována nejsou ovlivněny tímto procesem.</span><span class="sxs-lookup"><span data-stu-id="c0354-169">The devices that were already registered are unaffected by this process.</span></span>

<span data-ttu-id="c0354-170">Proveďte následující kroky k opětovnému vytvoření registrační klíč služby.</span><span class="sxs-lookup"><span data-stu-id="c0354-170">Perform the following steps to regenerate a service registration key.</span></span>

#### <a name="to-regenerate-the-service-registration-key"></a><span data-ttu-id="c0354-171">Chcete-li znovu vygenerovat registrační klíč služby</span><span class="sxs-lookup"><span data-stu-id="c0354-171">To regenerate the service registration key</span></span>
1. <span data-ttu-id="c0354-172">V **Manager zařízení StorSimple** okno, přejděte na **správy &gt;**  **klíče**.</span><span class="sxs-lookup"><span data-stu-id="c0354-172">In the **StorSimple Device Manager** blade, go to **Management &gt;** **Keys**.</span></span>
   
   ![Okno Klíče](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. <span data-ttu-id="c0354-174">V **klíče** okně klikněte na tlačítko **znovu vygenerovat**.</span><span class="sxs-lookup"><span data-stu-id="c0354-174">In the **Keys** blade, click **Regenerate**.</span></span>
   
   ![Klikněte na tlačítko znovu vygenerovat](./media/storsimple-virtual-array-manage-service/getregkey5.png)
3. <span data-ttu-id="c0354-176">V **registrační klíč znovu generovat služby** okno, zkontrolujte akce vyžadována, je-li se znovu vygenerovat klíče.</span><span class="sxs-lookup"><span data-stu-id="c0354-176">In the **Regenerate service registration key** blade, review the action required when the keys are regenerated.</span></span> <span data-ttu-id="c0354-177">Další zařízení, které jsou registrované s touto službou použije nový registrační klíč.</span><span class="sxs-lookup"><span data-stu-id="c0354-177">All the subsequent devices that are registered with this service will use the new registration key.</span></span> <span data-ttu-id="c0354-178">Klikněte na tlačítko **znovu vygenerovat** k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="c0354-178">Click **Regenerate** to confirm.</span></span> <span data-ttu-id="c0354-179">Po dokončení registrace, budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="c0354-179">You will be notified after the registration is complete.</span></span>
   
   ![Zkontrolujte znovu generovat klíče](./media/storsimple-virtual-array-manage-service/getregkey3.png)
4. <span data-ttu-id="c0354-181">Zobrazí se nový registrační klíč služby.</span><span class="sxs-lookup"><span data-stu-id="c0354-181">A new service registration key will appear.</span></span>
   
    ![Zkontrolujte znovu generovat klíče](./media/storsimple-virtual-array-manage-service/getregkey4.png)
   
   <span data-ttu-id="c0354-183">Zkopírujte tento klíč a uložit ho pro registraci nové zařízení s touto službou.</span><span class="sxs-lookup"><span data-stu-id="c0354-183">Copy this key and save it for registering any new devices with this service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c0354-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c0354-184">Next steps</span></span>
* <span data-ttu-id="c0354-185">Zjistěte, jak [Začínáme](storsimple-virtual-array-deploy1-portal-prep.md) s polem virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c0354-185">Learn how to [get started](storsimple-virtual-array-deploy1-portal-prep.md) with a StorSimple Virtual Array.</span></span>
* <span data-ttu-id="c0354-186">Zjistěte, jak [spravovat zařízení StorSimple](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="c0354-186">Learn how to [administer your StorSimple device](storsimple-ova-web-ui-admin.md).</span></span>

