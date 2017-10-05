---
title: "Konfigurace protokolů CHAP pro zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje, jak nakonfigurovat výzvu ověřování protokol CHAP (Handshake) na zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 61e0877187759d76b6f7efcef0a5ed8bec8500fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a><span data-ttu-id="4556b-103">Konfigurace protokolů CHAP pro vaše zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="4556b-103">Configure CHAP for your StorSimple device</span></span>

<span data-ttu-id="4556b-104">Tento kurz vysvětluje postup konfigurace CHAP pro vaše zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="4556b-104">This tutorial explains how to configure CHAP for your StorSimple device.</span></span> <span data-ttu-id="4556b-105">Tento postup podrobně popsané v tomto článku se vztahuje na řadu zařízení StorSimple 8000.</span><span class="sxs-lookup"><span data-stu-id="4556b-105">The procedure detailed in this article applies to StorSimple 8000 series devices.</span></span>

<span data-ttu-id="4556b-106">CHAP znamená Challenge Handshake Authentication Protocol.</span><span class="sxs-lookup"><span data-stu-id="4556b-106">CHAP stands for Challenge Handshake Authentication Protocol.</span></span> <span data-ttu-id="4556b-107">Je příslušné schéma ověřování používají servery pro ověření identity vzdálených klientů.</span><span class="sxs-lookup"><span data-stu-id="4556b-107">It is an authentication scheme used by servers to validate the identity of remote clients.</span></span> <span data-ttu-id="4556b-108">Ověření je založena na sdílené heslo nebo tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="4556b-108">The verification is based on a shared password or secret.</span></span> <span data-ttu-id="4556b-109">CHAP může být jeden ze způsobů (Jednosměrný) nebo vzájemné (obousměrné).</span><span class="sxs-lookup"><span data-stu-id="4556b-109">CHAP can be one way (unidirectional) or mutual (bidirectional).</span></span> <span data-ttu-id="4556b-110">Jedním ze způsobů CHAP je při cíl ověření iniciátor.</span><span class="sxs-lookup"><span data-stu-id="4556b-110">One way CHAP is when the target authenticates an initiator.</span></span> <span data-ttu-id="4556b-111">Ve vzájemné nebo zpětného CHAP cíl ověřuje iniciátoru a pak iniciátoru ověřuje cíl.</span><span class="sxs-lookup"><span data-stu-id="4556b-111">In mutual or reverse CHAP, the target authenticates the initiator and then the initiator authenticates the target.</span></span> <span data-ttu-id="4556b-112">Iniciátor ověřování se dá implementovat bez cíl ověřování.</span><span class="sxs-lookup"><span data-stu-id="4556b-112">Initiator authentication can be implemented without target authentication.</span></span> <span data-ttu-id="4556b-113">Cíl ověřování však můžete implementovat, pouze v případě, že se také implementuje iniciátor ověřování.</span><span class="sxs-lookup"><span data-stu-id="4556b-113">However, target authentication can be implemented only if initiator authentication is also implemented.</span></span>

<span data-ttu-id="4556b-114">Jako osvědčený postup doporučujeme použít CHAP pro zvýšení zabezpečení iSCSI.</span><span class="sxs-lookup"><span data-stu-id="4556b-114">As a best practice, we recommend that you use CHAP to enhance iSCSI security.</span></span>

> [!NOTE]
> <span data-ttu-id="4556b-115">Uvědomte si, že protokol IPSEC není aktuálně podporován zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="4556b-115">Keep in mind that IPSEC is not currently supported on StorSimple devices.</span></span>

<span data-ttu-id="4556b-116">CHAP nastavení v zařízení StorSimple můžete nakonfigurovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4556b-116">The CHAP settings on the StorSimple device can be configured in the following ways:</span></span>

* <span data-ttu-id="4556b-117">Jednosměrný nebo jednosměrné ověřování</span><span class="sxs-lookup"><span data-stu-id="4556b-117">Unidirectional or one-way authentication</span></span>
* <span data-ttu-id="4556b-118">Obousměrné nebo vzájemné nebo zpětného ověřování</span><span class="sxs-lookup"><span data-stu-id="4556b-118">Bidirectional or mutual or reverse authentication</span></span>

<span data-ttu-id="4556b-119">V každé z těchto případech je potřeba nakonfigurovat na portálu pro zařízení a serverového softwaru iniciátoru iSCSI.</span><span class="sxs-lookup"><span data-stu-id="4556b-119">In each of these cases, the portal for the device and the server iSCSI initiator software needs to be configured.</span></span> <span data-ttu-id="4556b-120">Podrobné pokyny pro tuto konfiguraci jsou popsané v následujícím kurzu.</span><span class="sxs-lookup"><span data-stu-id="4556b-120">The detailed steps for this configuration are described in the following tutorial.</span></span>

## <a name="unidirectional-or-one-way-authentication"></a><span data-ttu-id="4556b-121">Jednosměrný nebo jednosměrné ověřování</span><span class="sxs-lookup"><span data-stu-id="4556b-121">Unidirectional or one-way authentication</span></span>

<span data-ttu-id="4556b-122">Cíl v jednosměrný ověřování ověřuje iniciátor.</span><span class="sxs-lookup"><span data-stu-id="4556b-122">In unidirectional authentication, the target authenticates the initiator.</span></span> <span data-ttu-id="4556b-123">Toto ověřování vyžaduje, že nakonfigurujete nastavení iniciátoru CHAP na zařízení StorSimple a iSCSI software Initiator na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="4556b-123">This authentication requires that you configure the CHAP initiator settings on the StorSimple device and the iSCSI Initiator software on the host.</span></span> <span data-ttu-id="4556b-124">Podrobné pokyny k zařízení StorSimple a hostitele Windows jsou popsané dále.</span><span class="sxs-lookup"><span data-stu-id="4556b-124">The detailed procedures for your StorSimple device and Windows host are described next.</span></span>

#### <a name="to-configure-your-device-for-one-way-authentication"></a><span data-ttu-id="4556b-125">Pro konfiguraci zařízení pro jednosměrné ověřování</span><span class="sxs-lookup"><span data-stu-id="4556b-125">To configure your device for one-way authentication</span></span>

1. <span data-ttu-id="4556b-126">V portálu Azure přejděte do služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="4556b-126">In the Azure portal, go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="4556b-127">Klikněte na tlačítko **zařízení** a vyberte a klikněte na zařízení, které chcete nakonfigurovat CHAP pro.</span><span class="sxs-lookup"><span data-stu-id="4556b-127">Click **Devices** and select and click a device you wish to configure CHAP for.</span></span> <span data-ttu-id="4556b-128">Přejděte na **nastavení zařízení > zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="4556b-128">Go to **Device settings > Security**.</span></span> <span data-ttu-id="4556b-129">V **nastavení zabezpečení** okně klikněte na tlačítko **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="4556b-129">In the **Security settings** blade, click **CHAP**.</span></span>
   
    ![Iniciátoru CHAP](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. <span data-ttu-id="4556b-131">V **CHAP** okno a v **iniciátoru CHAP** části:</span><span class="sxs-lookup"><span data-stu-id="4556b-131">In the **CHAP** blade, and in the **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="4556b-132">Zadejte uživatelské jméno pro vaše iniciátoru CHAP.</span><span class="sxs-lookup"><span data-stu-id="4556b-132">Provide a user name for your CHAP initiator.</span></span>
   2. <span data-ttu-id="4556b-133">Zadejte heslo pro vaše iniciátoru CHAP.</span><span class="sxs-lookup"><span data-stu-id="4556b-133">Supply a password for your CHAP initiator.</span></span>
      
    > [!IMPORTANT]
    > <span data-ttu-id="4556b-134">Uživatelské jméno CHAP musí obsahovat méně než 233 znaků.</span><span class="sxs-lookup"><span data-stu-id="4556b-134">The CHAP user name must contain fewer than 233 characters.</span></span> <span data-ttu-id="4556b-135">CHAP heslo musí být 12 až 16 znaků.</span><span class="sxs-lookup"><span data-stu-id="4556b-135">The CHAP password must be between 12 and 16 characters.</span></span> <span data-ttu-id="4556b-136">Delší uživatelské jméno nebo heslo vede k chybě ověřování na hostiteli systému Windows.</span><span class="sxs-lookup"><span data-stu-id="4556b-136">A longer user name or password results in an authentication failure on the Windows host.</span></span>
   
   3. <span data-ttu-id="4556b-137">Potvrzení hesla.</span><span class="sxs-lookup"><span data-stu-id="4556b-137">Confirm the password.</span></span>

       ![Iniciátoru CHAP](./media/storsimple-8000-configure-chap/configure-chap6.png)
3. <span data-ttu-id="4556b-139">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4556b-139">Click **Save**.</span></span> <span data-ttu-id="4556b-140">Se zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="4556b-140">A confirmation message is displayed.</span></span> <span data-ttu-id="4556b-141">Klikněte na tlačítko **OK** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="4556b-141">Click **OK** to save the changes.</span></span>

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a><span data-ttu-id="4556b-142">Jednosměrné ověřování nakonfigurujete na hostitelském serveru Windows</span><span class="sxs-lookup"><span data-stu-id="4556b-142">To configure one-way authentication on the Windows host server</span></span>
1. <span data-ttu-id="4556b-143">Na hostitelském serveru systému Windows spusťte iniciátor iSCSI.</span><span class="sxs-lookup"><span data-stu-id="4556b-143">On the Windows host server, start the iSCSI Initiator.</span></span>
2. <span data-ttu-id="4556b-144">V **vlastnosti iniciátoru iSCSI** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4556b-144">In the **iSCSI Initiator Properties** window, perform the following steps:</span></span>
   
   1. <span data-ttu-id="4556b-145">Klikněte **zjišťování** kartě.</span><span class="sxs-lookup"><span data-stu-id="4556b-145">Click the **Discovery** tab.</span></span>
      
       ![Vlastnosti iniciátoru iSCSI](./media/storsimple-configure-chap/IC740944.png)
   2. <span data-ttu-id="4556b-147">Klikněte na tlačítko **zjistit portál**.</span><span class="sxs-lookup"><span data-stu-id="4556b-147">Click **Discover Portal**.</span></span>
3. <span data-ttu-id="4556b-148">V **zjistit cílový portál** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="4556b-148">In the **Discover Target Portal** dialog box:</span></span>
   
   1. <span data-ttu-id="4556b-149">Zadejte IP adresu vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="4556b-149">Specify the IP address of your device.</span></span>
   2. <span data-ttu-id="4556b-150">Klikněte na tlačítko **rozšířené**.</span><span class="sxs-lookup"><span data-stu-id="4556b-150">Click **Advanced**.</span></span>
      
       ![Zjistit cílový portál](./media/storsimple-configure-chap/IC740945.png)
4. <span data-ttu-id="4556b-152">V **Upřesnit nastavení** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="4556b-152">In the **Advanced Settings** dialog box:</span></span>
   
   1. <span data-ttu-id="4556b-153">Vyberte **povolit CHAP přihlásit** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="4556b-153">Select the **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="4556b-154">V **název** pole, zadejte uživatelské jméno, které jste zadali pro iniciátoru CHAP portálu classic.</span><span class="sxs-lookup"><span data-stu-id="4556b-154">In the **Name** field, supply the user name that you specified for the CHAP Initiator in the classic portal.</span></span>
   3. <span data-ttu-id="4556b-155">V **tajný klíč cíle** pole, zadejte heslo, které jste zadali pro iniciátoru CHAP portálu classic.</span><span class="sxs-lookup"><span data-stu-id="4556b-155">In the **Target secret** field, supply the password that you specified for the CHAP Initiator in the classic portal.</span></span>
   4. <span data-ttu-id="4556b-156">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4556b-156">Click **OK**.</span></span>
      
       ![Upřesňující nastavení, obecné](./media/storsimple-configure-chap/IC740946.png)
5. <span data-ttu-id="4556b-158">Na **cíle** kartě **vlastnosti iniciátoru iSCSI** okně Stav zařízení mají zobrazit jako **připojeno**.</span><span class="sxs-lookup"><span data-stu-id="4556b-158">On the **Targets** tab of the **iSCSI Initiator Properties** window, the device status should appear as **Connected**.</span></span> <span data-ttu-id="4556b-159">Pokud používáte zařízení StorSimple 1200, každý svazek připojit jako cíle iSCSI.</span><span class="sxs-lookup"><span data-stu-id="4556b-159">If you are using a StorSimple 1200 device, then each volume is mounted as an iSCSI target.</span></span> <span data-ttu-id="4556b-160">Proto kroky 3 a 4 muset opakovat pro každý svazek.</span><span class="sxs-lookup"><span data-stu-id="4556b-160">Hence, steps 3-4 will need to be repeated for each volume.</span></span>
   
    ![Svazky připojené jako samostatné cíle](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="4556b-162">Pokud změníte název iSCSI, nový název se používá pro nové relace iSCSI.</span><span class="sxs-lookup"><span data-stu-id="4556b-162">If you change the iSCSI name, the new name is used for new iSCSI sessions.</span></span> <span data-ttu-id="4556b-163">Nové nastavení nejsou znovu použít pro existující až do odhlášení a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4556b-163">New settings are not used for existing sessions until you log off and log on again.</span></span>

<span data-ttu-id="4556b-164">Další informace o konfiguraci CHAP na hostitelském serveru Windows, přejděte na [další aspekty](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="4556b-164">For more information about configuring CHAP on the Windows host server, go to [Additional considerations](#additional-considerations).</span></span>

## <a name="bidirectional-or-mutual-authentication"></a><span data-ttu-id="4556b-165">Obousměrné nebo vzájemné ověřování</span><span class="sxs-lookup"><span data-stu-id="4556b-165">Bidirectional or mutual authentication</span></span>

<span data-ttu-id="4556b-166">V obousměrné ověřování cíl ověřuje iniciátoru a pak iniciátoru ověřuje cíl.</span><span class="sxs-lookup"><span data-stu-id="4556b-166">In bidirectional authentication, the target authenticates the initiator and then the initiator authenticates the target.</span></span> <span data-ttu-id="4556b-167">Tento postup vyžaduje, aby uživatel k konfigurace nastavení iniciátoru CHAP a reverse CHAP nastavení na zařízení a služby iSCSI software Initiator na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="4556b-167">This procedure requires the user to configure the CHAP initiator settings, reverse CHAP settings on the device, and iSCSI Initiator software on the host.</span></span> <span data-ttu-id="4556b-168">Následující postupy popisují postup konfigurace vzájemného ověřování v zařízení a na hostiteli systému Windows.</span><span class="sxs-lookup"><span data-stu-id="4556b-168">The following procedures describe the steps to configure mutual authentication on the device and on the Windows host.</span></span>

#### <a name="to-configure-your-device-for-mutual-authentication"></a><span data-ttu-id="4556b-169">Pro konfiguraci zařízení pro vzájemné ověřování</span><span class="sxs-lookup"><span data-stu-id="4556b-169">To configure your device for mutual authentication</span></span>

1. <span data-ttu-id="4556b-170">V portálu Azure přejděte do služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="4556b-170">In the Azure portal, go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="4556b-171">Klikněte na tlačítko **zařízení** a vyberte a klikněte na zařízení, které chcete nakonfigurovat CHAP pro.</span><span class="sxs-lookup"><span data-stu-id="4556b-171">Click **Devices** and select and click a device you wish to configure CHAP for.</span></span> <span data-ttu-id="4556b-172">Přejděte na **nastavení zařízení > zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="4556b-172">Go to **Device settings > Security**.</span></span> <span data-ttu-id="4556b-173">V **nastavení zabezpečení** okně klikněte na tlačítko **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="4556b-173">In the **Security settings** blade, click **CHAP**.</span></span>
   
    ![Cíle CHAP](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. <span data-ttu-id="4556b-175">Posuňte se dolů na této stránce a v **CHAP cíl** části:</span><span class="sxs-lookup"><span data-stu-id="4556b-175">Scroll down on this page, and in the **CHAP Target** section:</span></span>
   
   1. <span data-ttu-id="4556b-176">Zadejte **Reverse CHAP uživatelské jméno** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="4556b-176">Provide a **Reverse CHAP user name** for your device.</span></span>
   2. <span data-ttu-id="4556b-177">Zadejte **hesla Reverse CHAP** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="4556b-177">Supply a **Reverse CHAP password** for your device.</span></span>
   3. <span data-ttu-id="4556b-178">Potvrzení hesla.</span><span class="sxs-lookup"><span data-stu-id="4556b-178">Confirm the password.</span></span>
3. <span data-ttu-id="4556b-179">V **iniciátoru CHAP** části:</span><span class="sxs-lookup"><span data-stu-id="4556b-179">In the **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="4556b-180">Zadejte **uživatelské jméno** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="4556b-180">Provide a **user name** for your device.</span></span>
   2. <span data-ttu-id="4556b-181">Zadejte **heslo** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="4556b-181">Provide a **password** for your device.</span></span>
   3. <span data-ttu-id="4556b-182">Potvrzení hesla.</span><span class="sxs-lookup"><span data-stu-id="4556b-182">Confirm the password.</span></span>

       ![Iniciátoru CHAP](./media/storsimple-8000-configure-chap/configure-chap11.png)
4. <span data-ttu-id="4556b-184">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4556b-184">Click **Save**.</span></span> <span data-ttu-id="4556b-185">Se zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="4556b-185">A confirmation message is displayed.</span></span> <span data-ttu-id="4556b-186">Klikněte na tlačítko **OK** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="4556b-186">Click **OK** to save the changes.</span></span>

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a><span data-ttu-id="4556b-187">Postup konfigurace obousměrné ověřování na hostitelském serveru Windows</span><span class="sxs-lookup"><span data-stu-id="4556b-187">To configure bidirectional authentication on the Windows host server</span></span>

1. <span data-ttu-id="4556b-188">Na hostitelském serveru systému Windows spusťte iniciátor iSCSI.</span><span class="sxs-lookup"><span data-stu-id="4556b-188">On the Windows host server, start the iSCSI Initiator.</span></span>
2. <span data-ttu-id="4556b-189">V **vlastnosti iniciátoru iSCSI** okně klikněte na tlačítko **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="4556b-189">In the **iSCSI Initiator Properties** window, click the **Configuration** tab.</span></span>
3. <span data-ttu-id="4556b-190">Klikněte na tlačítko **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="4556b-190">Click **CHAP**.</span></span>
4. <span data-ttu-id="4556b-191">V **tajný klíč pro vzájemné CHAP iniciátor iSCSI** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="4556b-191">In the **iSCSI Initiator Mutual CHAP Secret** dialog box:</span></span>
   
   1. <span data-ttu-id="4556b-192">Typ **hesla Reverse CHAP** nakonfigurovaný na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4556b-192">Type the **Reverse CHAP Password** that you configured in the Azure portal.</span></span>
   2. <span data-ttu-id="4556b-193">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4556b-193">Click **OK**.</span></span>
      
       ![iSCSI initiator vzájemné CHAP tajný klíč](./media/storsimple-configure-chap/IC740949.png)
5. <span data-ttu-id="4556b-195">Klikněte **cíle** kartě.</span><span class="sxs-lookup"><span data-stu-id="4556b-195">Click the **Targets** tab.</span></span>
6. <span data-ttu-id="4556b-196">Klikněte **Connect** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4556b-196">Click the **Connect** button.</span></span> 
7. <span data-ttu-id="4556b-197">V **připojení k cílové** dialogové okno, klikněte na tlačítko **Upřesnit**.</span><span class="sxs-lookup"><span data-stu-id="4556b-197">In the **Connect To Target** dialog box, click **Advanced**.</span></span>
8. <span data-ttu-id="4556b-198">V **Upřesnit vlastnosti** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="4556b-198">In the **Advanced Properties** dialog box:</span></span>
   
   1. <span data-ttu-id="4556b-199">Vyberte **povolit CHAP přihlásit** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="4556b-199">Select the **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="4556b-200">V **název** pole, zadejte uživatelské jméno, které jste zadali pro iniciátoru CHAP portálu classic.</span><span class="sxs-lookup"><span data-stu-id="4556b-200">In the **Name** field, supply the user name that you specified for the CHAP Initiator in the classic portal.</span></span>
   3. <span data-ttu-id="4556b-201">V **tajný klíč cíle** pole, zadejte heslo, které jste zadali pro iniciátoru CHAP portálu classic.</span><span class="sxs-lookup"><span data-stu-id="4556b-201">In the **Target secret** field, supply the password that you specified for the CHAP Initiator in the classic portal.</span></span>
   4. <span data-ttu-id="4556b-202">Vyberte **provést vzájemné ověřování** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="4556b-202">Select the **Perform mutual authentication** check box.</span></span>
      
       ![Upřesnit nastavení vzájemného ověření](./media/storsimple-configure-chap/IC740950.png)
   5. <span data-ttu-id="4556b-204">Klikněte na tlačítko **OK** k dokončení konfigurace CHAP</span><span class="sxs-lookup"><span data-stu-id="4556b-204">Click **OK** to complete the CHAP configuration</span></span>

<span data-ttu-id="4556b-205">Další informace o konfiguraci CHAP na hostitelském serveru Windows, přejděte na [další aspekty](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="4556b-205">For more information about configuring CHAP on the Windows host server, go to [Additional considerations](#additional-considerations).</span></span>

## <a name="additional-considerations"></a><span data-ttu-id="4556b-206">Další aspekty</span><span class="sxs-lookup"><span data-stu-id="4556b-206">Additional considerations</span></span>

<span data-ttu-id="4556b-207">**Rychlé připojení** funkce nepodporuje připojení, které mají povolené CHAP.</span><span class="sxs-lookup"><span data-stu-id="4556b-207">The **Quick Connect** feature does not support connections that have CHAP enabled.</span></span> <span data-ttu-id="4556b-208">Pokud je povoleno CHAP, ujistěte se, že používáte **připojit** tlačítko, která je dostupná na **cíle** kartě připojit k cíli.</span><span class="sxs-lookup"><span data-stu-id="4556b-208">When CHAP is enabled, make sure that you use the **Connect** button that is available on the **Targets** tab to connect to a target.</span></span>

![Připojit k cíli](./media/storsimple-configure-chap/IC740947.png)

<span data-ttu-id="4556b-210">V **připojit k cíli** dialogu, který se zobrazí, vyberte **přidejte toto připojení do seznamu oblíbených cílů** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="4556b-210">In the **Connect to Target** dialog box that is presented, select the **Add this connection to the list of Favorite Targets** check box.</span></span> <span data-ttu-id="4556b-211">Tato volba zajistí, že pokaždé, když se počítač restartuje, je proveden pokus o obnovení připojení k cílům iSCSI oblíbených položek.</span><span class="sxs-lookup"><span data-stu-id="4556b-211">This selection ensures that every time the computer restarts, an attempt is made to restore the connection to the iSCSI favorite targets.</span></span>

## <a name="errors-during-configuration"></a><span data-ttu-id="4556b-212">Chyby během konfigurace</span><span class="sxs-lookup"><span data-stu-id="4556b-212">Errors during configuration</span></span>

<span data-ttu-id="4556b-213">Pokud vaše konfigurace CHAP je nesprávná, pak budete chtít nejspíš najdete v článku **selhání ověření** chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="4556b-213">If your CHAP configuration is incorrect, then you are likely to see an **Authentication failure** error message.</span></span>

## <a name="verification-of-chap-configuration"></a><span data-ttu-id="4556b-214">Ověření konfigurace CHAP</span><span class="sxs-lookup"><span data-stu-id="4556b-214">Verification of CHAP configuration</span></span>

<span data-ttu-id="4556b-215">Můžete ověřit, jestli se používá protokol CHAP podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="4556b-215">You can verify that CHAP is being used by completing the following steps.</span></span>

#### <a name="to-verify-your-chap-configuration"></a><span data-ttu-id="4556b-216">Chcete-li ověřit konfiguraci CHAP</span><span class="sxs-lookup"><span data-stu-id="4556b-216">To verify your CHAP configuration</span></span>
1. <span data-ttu-id="4556b-217">Klikněte na tlačítko **Oblíbené cíle**.</span><span class="sxs-lookup"><span data-stu-id="4556b-217">Click **Favorite Targets**.</span></span>
2. <span data-ttu-id="4556b-218">Vyberte cíl, pro které jste povolili ověřování.</span><span class="sxs-lookup"><span data-stu-id="4556b-218">Select the target for which you enabled authentication.</span></span>
3. <span data-ttu-id="4556b-219">Klikněte na tlačítko **podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="4556b-219">Click **Details**.</span></span>
   
    ![Oblíbené cíle vlastnosti iniciátoru iSCSI](./media/storsimple-configure-chap/IC740951.png)
4. <span data-ttu-id="4556b-221">V **Oblíbené podrobnosti cíl** dialogové okno pole si všimněte, položky v **ověřování** pole.</span><span class="sxs-lookup"><span data-stu-id="4556b-221">In the **Favorite Target Details** dialog box, note the entry in the **Authentication** field.</span></span> <span data-ttu-id="4556b-222">Pokud konfigurace proběhla úspěšně, by mělo být uvedeno **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="4556b-222">If the configuration was successful, it should say **CHAP**.</span></span>
   
    ![Podrobnosti o oblíbeného cíle](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a><span data-ttu-id="4556b-224">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4556b-224">Next steps</span></span>

* <span data-ttu-id="4556b-225">Další informace o [zabezpečení zařízení StorSimple](storsimple-8000-security.md).</span><span class="sxs-lookup"><span data-stu-id="4556b-225">Learn more about [StorSimple security](storsimple-8000-security.md).</span></span>
* <span data-ttu-id="4556b-226">Další informace o [pomocí služby StorSimple Manager zařízení ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="4556b-226">Learn more about [using the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

