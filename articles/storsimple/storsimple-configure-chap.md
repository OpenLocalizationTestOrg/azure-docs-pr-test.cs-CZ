---
title: "Konfigurace protokolů CHAP pro zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje, jak nakonfigurovat výzvu ověřování protokol CHAP (Handshake) na zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 467044d7-7885-4382-90bd-3148dbbd341f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 36b4e73d0336deb9560d44163fc5330d1c9d775c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a><span data-ttu-id="b3bc0-103">Konfigurace protokolů CHAP pro vaše zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="b3bc0-103">Configure CHAP for your StorSimple device</span></span>
<span data-ttu-id="b3bc0-104">Tento kurz vysvětluje postup konfigurace CHAP pro vaše zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-104">This tutorial explains how to configure CHAP for your StorSimple device.</span></span> <span data-ttu-id="b3bc0-105">Postup popsaný v tomto článku platí pro řady StorSimple 8000 a také zařízení StorSimple 1200.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-105">The procedure detailed in this article applies to StorSimple 8000 series as well as StorSimple 1200 devices.</span></span>

<span data-ttu-id="b3bc0-106">CHAP znamená Challenge Handshake Authentication Protocol.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-106">CHAP stands for Challenge Handshake Authentication Protocol.</span></span> <span data-ttu-id="b3bc0-107">Je příslušné schéma ověřování používají servery pro ověření identity vzdálených klientů.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-107">It is an authentication scheme used by servers to validate the identity of remote clients.</span></span> <span data-ttu-id="b3bc0-108">Ověření je založena na sdílené heslo nebo tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-108">The verification is based on a shared password or secret.</span></span> <span data-ttu-id="b3bc0-109">CHAP může být jednosměrná (Jednosměrný) nebo vzájemné (obousměrné).</span><span class="sxs-lookup"><span data-stu-id="b3bc0-109">CHAP can be one-way (unidirectional) or mutual (bidirectional).</span></span> <span data-ttu-id="b3bc0-110">Jednosměrné CHAP je při cíl ověření iniciátor.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-110">One-way CHAP is when the target authenticates an initiator.</span></span> <span data-ttu-id="b3bc0-111">Vzájemné nebo zpětného CHAP, na druhé straně vyžaduje, aby cíl ověření iniciátoru a pak iniciátoru ověření cíl.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-111">Mutual or reverse CHAP, on the other hand, requires that the target authenticate the initiator and then the initiator authenticate the target.</span></span> <span data-ttu-id="b3bc0-112">Iniciátor ověřování se dá implementovat bez cíl ověřování.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-112">Initiator authentication can be implemented without target authentication.</span></span> <span data-ttu-id="b3bc0-113">Cíl ověřování však můžete implementovat, pouze v případě, že se také implementuje iniciátor ověřování.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-113">However, target authentication can be implemented only if initiator authentication is also implemented.</span></span> 

<span data-ttu-id="b3bc0-114">Jako osvědčený postup doporučujeme použít CHAP pro zvýšení zabezpečení iSCSI.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-114">As a best practice, we recommend that you use CHAP to enhance iSCSI security.</span></span>

> [!NOTE]
> <span data-ttu-id="b3bc0-115">Uvědomte si, že protokol IPSEC není aktuálně podporován zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-115">Keep in mind that IPSEC is not currently supported on StorSimple devices.</span></span>
> 
> 

<span data-ttu-id="b3bc0-116">CHAP nastavení v zařízení StorSimple můžete nakonfigurovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b3bc0-116">The CHAP settings on the StorSimple device can be configured in the following ways:</span></span>

* <span data-ttu-id="b3bc0-117">Jednosměrný nebo jednosměrné ověřování</span><span class="sxs-lookup"><span data-stu-id="b3bc0-117">Unidirectional or one-way authentication</span></span>
* <span data-ttu-id="b3bc0-118">Obousměrné nebo vzájemné nebo zpětného ověřování</span><span class="sxs-lookup"><span data-stu-id="b3bc0-118">Bidirectional or mutual or reverse authentication</span></span>

<span data-ttu-id="b3bc0-119">V každé z těchto případech je potřeba nakonfigurovat na portálu pro zařízení a serverového softwaru iniciátoru iSCSI.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-119">In each of these cases, the portal for the device and the server iSCSI initiator software needs to be configured.</span></span> <span data-ttu-id="b3bc0-120">Podrobné pokyny pro tuto konfiguraci jsou popsané v následujícím kurzu.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-120">The detailed steps for this configuration are described in the following tutorial.</span></span>

## <a name="unidirectional-or-one-way-authentication"></a><span data-ttu-id="b3bc0-121">Jednosměrný nebo jednosměrné ověřování</span><span class="sxs-lookup"><span data-stu-id="b3bc0-121">Unidirectional or one-way authentication</span></span>
<span data-ttu-id="b3bc0-122">Cíl v jednosměrný ověřování ověřuje iniciátor.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-122">In unidirectional authentication, the target authenticates the initiator.</span></span> <span data-ttu-id="b3bc0-123">Toto ověřování vyžaduje, že nakonfigurujete nastavení iniciátoru CHAP na zařízení StorSimple a iSCSI software Initiator na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-123">This authentication requires that you configure the CHAP initiator settings on the StorSimple device and the iSCSI Initiator software on the host.</span></span> <span data-ttu-id="b3bc0-124">Podrobné pokyny k zařízení StorSimple a hostitele Windows jsou popsané dále.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-124">The detailed procedures for your StorSimple device and Windows host are described next.</span></span>

#### <a name="to-configure-your-device-for-one-way-authentication"></a><span data-ttu-id="b3bc0-125">Pro konfiguraci zařízení pro jednosměrné ověřování</span><span class="sxs-lookup"><span data-stu-id="b3bc0-125">To configure your device for one-way authentication</span></span>
1. <span data-ttu-id="b3bc0-126">Na portálu Azure classic na **zařízení** klikněte na tlačítko **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-126">In the Azure classic portal, on the **Devices** page, click the **Configure** tab.</span></span>
   
    ![Iniciátoru CHAP](./media/storsimple-configure-chap/IC740943.png)
2. <span data-ttu-id="b3bc0-128">Posuňte se dolů na této stránce a v **iniciátoru CHAP** části:</span><span class="sxs-lookup"><span data-stu-id="b3bc0-128">Scroll down on this page, and in the **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="b3bc0-129">Zadejte uživatelské jméno pro vaše iniciátoru CHAP.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-129">Provide a user name for your CHAP initiator.</span></span>
   2. <span data-ttu-id="b3bc0-130">Zadejte heslo pro vaše iniciátoru CHAP.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-130">Supply a password for your CHAP initiator.</span></span>
      
    > [!IMPORTANT]
    > <span data-ttu-id="b3bc0-131">Uživatelské jméno CHAP musí obsahovat méně než 233 znaků.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-131">The CHAP user name must contain fewer than 233 characters.</span></span> <span data-ttu-id="b3bc0-132">CHAP heslo musí být 12 až 16 znaků.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-132">The CHAP password must be between 12 and 16 characters.</span></span> <span data-ttu-id="b3bc0-133">Delší uživatelské jméno nebo heslo povede k chybě ověřování na hostiteli systému Windows.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-133">A longer user name or password will result in an authentication failure on the Windows host.</span></span>
   
   3. <span data-ttu-id="b3bc0-134">Potvrzení hesla.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-134">Confirm the password.</span></span>
3. <span data-ttu-id="b3bc0-135">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-135">Click **Save**.</span></span> <span data-ttu-id="b3bc0-136">Zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-136">A confirmation message will be displayed.</span></span> <span data-ttu-id="b3bc0-137">Klikněte na tlačítko **OK** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-137">Click **OK** to save the changes.</span></span>

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a><span data-ttu-id="b3bc0-138">Jednosměrné ověřování nakonfigurujete na hostitelském serveru Windows</span><span class="sxs-lookup"><span data-stu-id="b3bc0-138">To configure one-way authentication on the Windows host server</span></span>
1. <span data-ttu-id="b3bc0-139">Na hostitelském serveru systému Windows spusťte iniciátor iSCSI.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-139">On the Windows host server, start the iSCSI Initiator.</span></span>
2. <span data-ttu-id="b3bc0-140">V **vlastnosti iniciátoru iSCSI** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b3bc0-140">In the **iSCSI Initiator Properties** window, perform the following steps:</span></span>
   
   1. <span data-ttu-id="b3bc0-141">Klikněte **zjišťování** kartě.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-141">Click the **Discovery** tab.</span></span>
      
       ![Vlastnosti iniciátoru iSCSI](./media/storsimple-configure-chap/IC740944.png)
   2. <span data-ttu-id="b3bc0-143">Klikněte na tlačítko **zjistit portál**.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-143">Click **Discover Portal**.</span></span>
3. <span data-ttu-id="b3bc0-144">V **zjistit cílový portál** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="b3bc0-144">In the **Discover Target Portal** dialog box:</span></span>
   
   1. <span data-ttu-id="b3bc0-145">Zadejte IP adresu vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-145">Specify the IP address of your device.</span></span>
   2. <span data-ttu-id="b3bc0-146">Klikněte na tlačítko **rozšířené**.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-146">Click **Advanced**.</span></span>
      
       ![Zjistit cílový portál](./media/storsimple-configure-chap/IC740945.png)
4. <span data-ttu-id="b3bc0-148">V **Upřesnit nastavení** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="b3bc0-148">In the **Advanced Settings** dialog box:</span></span>
   
   1. <span data-ttu-id="b3bc0-149">Vyberte **povolit CHAP přihlásit** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-149">Select the **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="b3bc0-150">V **název** pole, zadejte uživatelské jméno, které jste zadali pro iniciátoru CHAP portálu classic.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-150">In the **Name** field, supply the user name that you specified for the CHAP Initiator in the classic portal.</span></span>
   3. <span data-ttu-id="b3bc0-151">V **tajný klíč cíle** pole, zadejte heslo, které jste zadali pro iniciátoru CHAP portálu classic.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-151">In the **Target secret** field, supply the password that you specified for the CHAP Initiator in the classic portal.</span></span>
   4. <span data-ttu-id="b3bc0-152">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-152">Click **OK**.</span></span>
      
       ![Upřesňující nastavení, obecné](./media/storsimple-configure-chap/IC740946.png)
5. <span data-ttu-id="b3bc0-154">Na **cíle** kartě **vlastnosti iniciátoru iSCSI** okně Stav zařízení mají zobrazit jako **připojeno**.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-154">On the **Targets** tab of the **iSCSI Initiator Properties** window, the device status should appear as **Connected**.</span></span> <span data-ttu-id="b3bc0-155">Pokud používáte zařízení StorSimple 1200, pak každý svazek se připojí jako cíl iSCSI jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-155">If you are using a StorSimple 1200 device, then each volume will be mounted as an iSCSI target as shown below.</span></span> <span data-ttu-id="b3bc0-156">Proto kroky 3 a 4 muset opakovat pro každý svazek.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-156">Hence, steps  3-4 will need to be repeated for each volume.</span></span>
   
    ![Svazky připojené jako samostatné cíle](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="b3bc0-158">Pokud změníte název iSCSI, nový název se použije pro nové relace iSCSI.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-158">If you change the iSCSI name, the new name will be used for new iSCSI sessions.</span></span> <span data-ttu-id="b3bc0-159">Nové nastavení nejsou znovu použít pro existující až do odhlášení a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-159">New settings are not used for existing sessions until you log off and log on again.</span></span>
   > 
   > 

<span data-ttu-id="b3bc0-160">Další informace o konfiguraci CHAP na hostitelském serveru Windows, přejděte na [další aspekty](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="b3bc0-160">For more information about configuring CHAP on the Windows host server, go to [Additional considerations](#additional-considerations).</span></span>

## <a name="bidirectional-or-mutual-authentication"></a><span data-ttu-id="b3bc0-161">Obousměrné nebo vzájemné ověřování</span><span class="sxs-lookup"><span data-stu-id="b3bc0-161">Bidirectional or mutual authentication</span></span>
<span data-ttu-id="b3bc0-162">V obousměrné ověřování cíl ověřuje iniciátoru a pak iniciátoru ověřuje cíl.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-162">In bidirectional authentication, the target authenticates the initiator and then the initiator authenticates the target.</span></span> <span data-ttu-id="b3bc0-163">To vyžaduje, aby uživatel ke konfiguraci nastavení iniciátoru CHAP, jakož i zpětné CHAP nastavení na zařízení a iSCSI Initiator software na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-163">This requires the user to configure the CHAP initiator settings, as well as the reverse CHAP settings on the device and iSCSI Initiator software on the host.</span></span> <span data-ttu-id="b3bc0-164">Následující postupy popisují postup konfigurace vzájemného ověřování v zařízení a na hostiteli systému Windows.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-164">The following procedures describe the steps to configure mutual authentication on the device and on the Windows host.</span></span>

#### <a name="to-configure-your-device-for-mutual-authentication"></a><span data-ttu-id="b3bc0-165">Pro konfiguraci zařízení pro vzájemné ověřování</span><span class="sxs-lookup"><span data-stu-id="b3bc0-165">To configure your device for mutual authentication</span></span>
1. <span data-ttu-id="b3bc0-166">Na portálu Azure classic na **zařízení** klikněte na tlačítko **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-166">In the Azure classic portal, on the **Devices** page, click the **Configure** tab.</span></span>
   
    ![Cíle CHAP](./media/storsimple-configure-chap/IC740948.png)
2. <span data-ttu-id="b3bc0-168">Posuňte se dolů na této stránce a v **CHAP cíl** části:</span><span class="sxs-lookup"><span data-stu-id="b3bc0-168">Scroll down on this page, and in the **CHAP Target** section:</span></span>
   
   1. <span data-ttu-id="b3bc0-169">Zadejte **Reverse CHAP uživatelské jméno** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-169">Provide a **Reverse CHAP user name** for your device.</span></span>
   2. <span data-ttu-id="b3bc0-170">Zadejte **hesla Reverse CHAP** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-170">Supply a **Reverse CHAP password** for your device.</span></span>
   3. <span data-ttu-id="b3bc0-171">Potvrzení hesla.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-171">Confirm the password.</span></span>
3. <span data-ttu-id="b3bc0-172">V **iniciátoru CHAP** části:</span><span class="sxs-lookup"><span data-stu-id="b3bc0-172">In the **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="b3bc0-173">Zadejte **uživatelské jméno** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-173">Provide a **user name** for your device.</span></span>
   2. <span data-ttu-id="b3bc0-174">Zadejte **heslo** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-174">Provide a **password** for your device.</span></span>
   3. <span data-ttu-id="b3bc0-175">Potvrzení hesla.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-175">Confirm the password.</span></span>
4. <span data-ttu-id="b3bc0-176">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-176">Click **Save**.</span></span> <span data-ttu-id="b3bc0-177">Zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-177">A confirmation message will be displayed.</span></span> <span data-ttu-id="b3bc0-178">Klikněte na tlačítko **OK** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-178">Click **OK** to save the changes.</span></span>

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a><span data-ttu-id="b3bc0-179">Postup konfigurace obousměrné ověřování na hostitelském serveru Windows</span><span class="sxs-lookup"><span data-stu-id="b3bc0-179">To configure bidirectional authentication on the Windows host server</span></span>
1. <span data-ttu-id="b3bc0-180">Na hostitelském serveru systému Windows spusťte iniciátor iSCSI.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-180">On the Windows host server, start the iSCSI Initiator.</span></span>
2. <span data-ttu-id="b3bc0-181">V **vlastnosti iniciátoru iSCSI** okně klikněte na tlačítko **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-181">In the **iSCSI Initiator Properties** window, click the **Configuration** tab.</span></span>
3. <span data-ttu-id="b3bc0-182">Klikněte na tlačítko **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-182">Click **CHAP**.</span></span>
4. <span data-ttu-id="b3bc0-183">V **tajný klíč pro vzájemné CHAP iniciátor iSCSI** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="b3bc0-183">In the **iSCSI Initiator Mutual CHAP Secret** dialog box:</span></span>
   
   1. <span data-ttu-id="b3bc0-184">Typ **hesla Reverse CHAP** nakonfigurovaný na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-184">Type the **Reverse CHAP Password** that you configured in the Azure classic portal.</span></span>
   2. <span data-ttu-id="b3bc0-185">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-185">Click **OK**.</span></span>
      
       ![iSCSI initiator vzájemné CHAP tajný klíč](./media/storsimple-configure-chap/IC740949.png)
5. <span data-ttu-id="b3bc0-187">Klikněte **cíle** kartě.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-187">Click the **Targets** tab.</span></span>
6. <span data-ttu-id="b3bc0-188">Klikněte **Connect** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-188">Click the **Connect** button.</span></span> 
7. <span data-ttu-id="b3bc0-189">V **připojení k cílové** dialogové okno, klikněte na tlačítko **Upřesnit**.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-189">In the **Connect To Target** dialog box, click **Advanced**.</span></span>
8. <span data-ttu-id="b3bc0-190">V **Upřesnit vlastnosti** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="b3bc0-190">In the **Advanced Properties** dialog box:</span></span>
   
   1. <span data-ttu-id="b3bc0-191">Vyberte **povolit CHAP přihlásit** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-191">Select the **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="b3bc0-192">V **název** pole, zadejte uživatelské jméno, které jste zadali pro iniciátoru CHAP portálu classic.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-192">In the **Name** field, supply the user name that you specified for the CHAP Initiator in the classic portal.</span></span>
   3. <span data-ttu-id="b3bc0-193">V **tajný klíč cíle** pole, zadejte heslo, které jste zadali pro iniciátoru CHAP portálu classic.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-193">In the **Target secret** field, supply the password that you specified for the CHAP Initiator in the classic portal.</span></span>
   4. <span data-ttu-id="b3bc0-194">Vyberte **provést vzájemné ověřování** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-194">Select the **Perform mutual authentication** check box.</span></span>
      
       ![Upřesnit nastavení vzájemného ověření](./media/storsimple-configure-chap/IC740950.png)
   5. <span data-ttu-id="b3bc0-196">Klikněte na tlačítko **OK** k dokončení konfigurace CHAP</span><span class="sxs-lookup"><span data-stu-id="b3bc0-196">Click **OK** to complete the CHAP configuration</span></span>

<span data-ttu-id="b3bc0-197">Další informace o konfiguraci CHAP na hostitelském serveru Windows, přejděte na [další aspekty](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="b3bc0-197">For more information about configuring CHAP on the Windows host server, go to [Additional considerations](#additional-considerations).</span></span>

## <a name="additional-considerations"></a><span data-ttu-id="b3bc0-198">Další aspekty</span><span class="sxs-lookup"><span data-stu-id="b3bc0-198">Additional considerations</span></span>
<span data-ttu-id="b3bc0-199">**Rychlé připojení** funkce nepodporuje připojení, které mají povolené CHAP.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-199">The **Quick Connect** feature does not support connections that have CHAP enabled.</span></span> <span data-ttu-id="b3bc0-200">Pokud je povoleno CHAP, ujistěte se, že používáte **připojit** tlačítko, která je dostupná na **cíle** kartě připojit k cíli.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-200">When CHAP is enabled, make sure that you use the **Connect** button that is available on the **Targets** tab to connect to a target.</span></span>

![Připojit k cíli](./media/storsimple-configure-chap/IC740947.png)

<span data-ttu-id="b3bc0-202">V **připojit k cíli** dialogu, který se zobrazí, vyberte **přidejte toto připojení do seznamu oblíbených cílů** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-202">In the **Connect to Target** dialog box that is presented, select the **Add this connection to the list of Favorite Targets** check box.</span></span> <span data-ttu-id="b3bc0-203">Tím se zajistí, že pokaždé, když se počítač restartuje, je proveden pokus o obnovení připojení k cílům iSCSI oblíbených položek.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-203">This ensures that every time the computer restarts, an attempt is made to restore the connection to the iSCSI favorite targets.</span></span>

## <a name="errors-during-configuration"></a><span data-ttu-id="b3bc0-204">Chyby během konfigurace</span><span class="sxs-lookup"><span data-stu-id="b3bc0-204">Errors during configuration</span></span>
<span data-ttu-id="b3bc0-205">Pokud vaše konfigurace CHAP je nesprávná, pak budete chtít nejspíš najdete v článku **selhání ověření** chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-205">If your CHAP configuration is incorrect, then you are likely to see an **Authentication failure** error message.</span></span>

## <a name="verification-of-chap-configuration"></a><span data-ttu-id="b3bc0-206">Ověření konfigurace CHAP</span><span class="sxs-lookup"><span data-stu-id="b3bc0-206">Verification of CHAP configuration</span></span>
<span data-ttu-id="b3bc0-207">Můžete ověřit, jestli se používá protokol CHAP podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-207">You can verify that CHAP is being used by completing the following steps.</span></span>

#### <a name="to-verify-your-chap-configuration"></a><span data-ttu-id="b3bc0-208">Chcete-li ověřit konfiguraci CHAP</span><span class="sxs-lookup"><span data-stu-id="b3bc0-208">To verify your CHAP configuration</span></span>
1. <span data-ttu-id="b3bc0-209">Klikněte na tlačítko **Oblíbené cíle**.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-209">Click **Favorite Targets**.</span></span>
2. <span data-ttu-id="b3bc0-210">Vyberte cíl, pro které jste povolili ověřování.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-210">Select the target for which you enabled authentication.</span></span>
3. <span data-ttu-id="b3bc0-211">Klikněte na tlačítko **podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-211">Click **Details**.</span></span>
   
    ![Oblíbené cíle vlastnosti iniciátoru iSCSI](./media/storsimple-configure-chap/IC740951.png)
4. <span data-ttu-id="b3bc0-213">V **Oblíbené podrobnosti cíl** dialogové okno pole si všimněte, položky v **ověřování** pole.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-213">In the **Favorite Target Details** dialog box, note the entry in the **Authentication** field.</span></span> <span data-ttu-id="b3bc0-214">Pokud konfigurace proběhla úspěšně, by mělo být uvedeno **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="b3bc0-214">If the configuration was successful, it should say **CHAP**.</span></span>
   
    ![Podrobnosti o oblíbeného cíle](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a><span data-ttu-id="b3bc0-216">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b3bc0-216">Next steps</span></span>
* <span data-ttu-id="b3bc0-217">Další informace o [zabezpečení zařízení StorSimple](storsimple-security.md).</span><span class="sxs-lookup"><span data-stu-id="b3bc0-217">Learn more about [StorSimple security](storsimple-security.md).</span></span>
* <span data-ttu-id="b3bc0-218">Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="b3bc0-218">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

