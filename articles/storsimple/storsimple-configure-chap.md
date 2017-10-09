---
title: "aaaConfigure CHAP pro zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje, jak tooconfigure hello Challenge Handshake Authentication Protocol (CHAP) na zařízení StorSimple."
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
ms.openlocfilehash: 272ef2c184f56ad262e55410357494c72e45cf83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a><span data-ttu-id="35199-103">Konfigurace protokolů CHAP pro vaše zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="35199-103">Configure CHAP for your StorSimple device</span></span>
<span data-ttu-id="35199-104">Tento kurz popisuje, jak tooconfigure CHAP pro vaše zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="35199-104">This tutorial explains how tooconfigure CHAP for your StorSimple device.</span></span> <span data-ttu-id="35199-105">Hello postup popsaný v tomto článku se vztahuje tooStorSimple řady 8000 a také zařízení StorSimple 1200.</span><span class="sxs-lookup"><span data-stu-id="35199-105">hello procedure detailed in this article applies tooStorSimple 8000 series as well as StorSimple 1200 devices.</span></span>

<span data-ttu-id="35199-106">CHAP znamená Challenge Handshake Authentication Protocol.</span><span class="sxs-lookup"><span data-stu-id="35199-106">CHAP stands for Challenge Handshake Authentication Protocol.</span></span> <span data-ttu-id="35199-107">Je příslušné schéma ověřování používané servery toovalidate hello identitu vzdálených klientů.</span><span class="sxs-lookup"><span data-stu-id="35199-107">It is an authentication scheme used by servers toovalidate hello identity of remote clients.</span></span> <span data-ttu-id="35199-108">ověření Hello je založena na sdílené heslo nebo tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="35199-108">hello verification is based on a shared password or secret.</span></span> <span data-ttu-id="35199-109">CHAP může být jednosměrná (Jednosměrný) nebo vzájemné (obousměrné).</span><span class="sxs-lookup"><span data-stu-id="35199-109">CHAP can be one-way (unidirectional) or mutual (bidirectional).</span></span> <span data-ttu-id="35199-110">Jednosměrné CHAP je při hello cíl ověření iniciátor.</span><span class="sxs-lookup"><span data-stu-id="35199-110">One-way CHAP is when hello target authenticates an initiator.</span></span> <span data-ttu-id="35199-111">Vzájemné nebo zpětného CHAP na hello druhé straně, vyžaduje, aby hello cíl ověření hello iniciátor a pak hello iniciátor ověření hello cíl.</span><span class="sxs-lookup"><span data-stu-id="35199-111">Mutual or reverse CHAP, on hello other hand, requires that hello target authenticate hello initiator and then hello initiator authenticate hello target.</span></span> <span data-ttu-id="35199-112">Iniciátor ověřování se dá implementovat bez cíl ověřování.</span><span class="sxs-lookup"><span data-stu-id="35199-112">Initiator authentication can be implemented without target authentication.</span></span> <span data-ttu-id="35199-113">Cíl ověřování však můžete implementovat, pouze v případě, že se také implementuje iniciátor ověřování.</span><span class="sxs-lookup"><span data-stu-id="35199-113">However, target authentication can be implemented only if initiator authentication is also implemented.</span></span> 

<span data-ttu-id="35199-114">Jako osvědčený postup doporučujeme použít CHAP tooenhance iSCSI zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="35199-114">As a best practice, we recommend that you use CHAP tooenhance iSCSI security.</span></span>

> [!NOTE]
> <span data-ttu-id="35199-115">Uvědomte si, že protokol IPSEC není aktuálně podporován zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="35199-115">Keep in mind that IPSEC is not currently supported on StorSimple devices.</span></span>
> 
> 

<span data-ttu-id="35199-116">Hello CHAP nastavení v zařízení StorSimple hello se dá nakonfigurovat v hello následující způsoby:</span><span class="sxs-lookup"><span data-stu-id="35199-116">hello CHAP settings on hello StorSimple device can be configured in hello following ways:</span></span>

* <span data-ttu-id="35199-117">Jednosměrný nebo jednosměrné ověřování</span><span class="sxs-lookup"><span data-stu-id="35199-117">Unidirectional or one-way authentication</span></span>
* <span data-ttu-id="35199-118">Obousměrné nebo vzájemné nebo zpětného ověřování</span><span class="sxs-lookup"><span data-stu-id="35199-118">Bidirectional or mutual or reverse authentication</span></span>

<span data-ttu-id="35199-119">V každé z těchto případů musí hello portál pro hello zařízení a softwarem iniciátoru iSCSI server hello toobe nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="35199-119">In each of these cases, hello portal for hello device and hello server iSCSI initiator software needs toobe configured.</span></span> <span data-ttu-id="35199-120">Hello podrobné pokyny pro tuto konfiguraci jsou popsány v následujícím kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="35199-120">hello detailed steps for this configuration are described in hello following tutorial.</span></span>

## <a name="unidirectional-or-one-way-authentication"></a><span data-ttu-id="35199-121">Jednosměrný nebo jednosměrné ověřování</span><span class="sxs-lookup"><span data-stu-id="35199-121">Unidirectional or one-way authentication</span></span>
<span data-ttu-id="35199-122">V jednosměrný ověřování ověřuje hello cíl hello iniciátor.</span><span class="sxs-lookup"><span data-stu-id="35199-122">In unidirectional authentication, hello target authenticates hello initiator.</span></span> <span data-ttu-id="35199-123">Toto ověřování vyžaduje, konfigurovat nastavení iniciátoru CHAP hello v zařízení StorSimple hello a hello iSCSI software Initiator na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="35199-123">This authentication requires that you configure hello CHAP initiator settings on hello StorSimple device and hello iSCSI Initiator software on hello host.</span></span> <span data-ttu-id="35199-124">Hello podrobné postupy pro zařízení StorSimple a hostitele Windows jsou popsané dále.</span><span class="sxs-lookup"><span data-stu-id="35199-124">hello detailed procedures for your StorSimple device and Windows host are described next.</span></span>

#### <a name="tooconfigure-your-device-for-one-way-authentication"></a><span data-ttu-id="35199-125">tooconfigure zařízení pro jednosměrné ověřování</span><span class="sxs-lookup"><span data-stu-id="35199-125">tooconfigure your device for one-way authentication</span></span>
1. <span data-ttu-id="35199-126">V portálu Azure classic, na hello hello **zařízení** klikněte na tlačítko hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="35199-126">In hello Azure classic portal, on hello **Devices** page, click hello **Configure** tab.</span></span>
   
    ![Iniciátoru CHAP](./media/storsimple-configure-chap/IC740943.png)
2. <span data-ttu-id="35199-128">Posuňte se dolů na této stránce a hello **iniciátoru CHAP** části:</span><span class="sxs-lookup"><span data-stu-id="35199-128">Scroll down on this page, and in hello **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="35199-129">Zadejte uživatelské jméno pro vaše iniciátoru CHAP.</span><span class="sxs-lookup"><span data-stu-id="35199-129">Provide a user name for your CHAP initiator.</span></span>
   2. <span data-ttu-id="35199-130">Zadejte heslo pro vaše iniciátoru CHAP.</span><span class="sxs-lookup"><span data-stu-id="35199-130">Supply a password for your CHAP initiator.</span></span>
      
    > [!IMPORTANT]
    > <span data-ttu-id="35199-131">Hello CHAP uživatelské jméno musí obsahovat méně než 233 znaků.</span><span class="sxs-lookup"><span data-stu-id="35199-131">hello CHAP user name must contain fewer than 233 characters.</span></span> <span data-ttu-id="35199-132">Hello CHAP heslo musí být 12 až 16 znaků.</span><span class="sxs-lookup"><span data-stu-id="35199-132">hello CHAP password must be between 12 and 16 characters.</span></span> <span data-ttu-id="35199-133">Delší uživatelské jméno nebo heslo bude mít za následek selhání ověřování na hostitele Windows hello.</span><span class="sxs-lookup"><span data-stu-id="35199-133">A longer user name or password will result in an authentication failure on hello Windows host.</span></span>
   
   3. <span data-ttu-id="35199-134">Potvrzení hesla hello.</span><span class="sxs-lookup"><span data-stu-id="35199-134">Confirm hello password.</span></span>
3. <span data-ttu-id="35199-135">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="35199-135">Click **Save**.</span></span> <span data-ttu-id="35199-136">Zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="35199-136">A confirmation message will be displayed.</span></span> <span data-ttu-id="35199-137">Klikněte na tlačítko **OK** toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="35199-137">Click **OK** toosave hello changes.</span></span>

#### <a name="tooconfigure-one-way-authentication-on-hello-windows-host-server"></a><span data-ttu-id="35199-138">tooconfigure jednosměrné ověřování v systému Windows hello hostitelském serveru</span><span class="sxs-lookup"><span data-stu-id="35199-138">tooconfigure one-way authentication on hello Windows host server</span></span>
1. <span data-ttu-id="35199-139">Na serveru hostitele Windows hello spusťte iniciátor iSCSI hello.</span><span class="sxs-lookup"><span data-stu-id="35199-139">On hello Windows host server, start hello iSCSI Initiator.</span></span>
2. <span data-ttu-id="35199-140">V hello **vlastnosti iniciátoru iSCSI** okně provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="35199-140">In hello **iSCSI Initiator Properties** window, perform hello following steps:</span></span>
   
   1. <span data-ttu-id="35199-141">Klikněte na tlačítko hello **zjišťování** kartě.</span><span class="sxs-lookup"><span data-stu-id="35199-141">Click hello **Discovery** tab.</span></span>
      
       ![Vlastnosti iniciátoru iSCSI](./media/storsimple-configure-chap/IC740944.png)
   2. <span data-ttu-id="35199-143">Klikněte na tlačítko **zjistit portál**.</span><span class="sxs-lookup"><span data-stu-id="35199-143">Click **Discover Portal**.</span></span>
3. <span data-ttu-id="35199-144">V hello **zjistit cílový portál** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="35199-144">In hello **Discover Target Portal** dialog box:</span></span>
   
   1. <span data-ttu-id="35199-145">Zadejte IP adresu hello vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="35199-145">Specify hello IP address of your device.</span></span>
   2. <span data-ttu-id="35199-146">Klikněte na tlačítko **rozšířené**.</span><span class="sxs-lookup"><span data-stu-id="35199-146">Click **Advanced**.</span></span>
      
       ![Zjistit cílový portál](./media/storsimple-configure-chap/IC740945.png)
4. <span data-ttu-id="35199-148">V hello **Upřesnit nastavení** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="35199-148">In hello **Advanced Settings** dialog box:</span></span>
   
   1. <span data-ttu-id="35199-149">Vyberte hello **povolit CHAP přihlásit** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="35199-149">Select hello **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="35199-150">V hello **název** pole, zadejte hello uživatelské jméno, které jste zadali pro hello iniciátoru CHAP portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="35199-150">In hello **Name** field, supply hello user name that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   3. <span data-ttu-id="35199-151">V hello **tajný klíč cíle** pole, zadejte hello heslo, které jste zadali pro hello iniciátoru CHAP portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="35199-151">In hello **Target secret** field, supply hello password that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   4. <span data-ttu-id="35199-152">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="35199-152">Click **OK**.</span></span>
      
       ![Upřesňující nastavení, obecné](./media/storsimple-configure-chap/IC740946.png)
5. <span data-ttu-id="35199-154">Na hello **cíle** kartě hello **vlastnosti iniciátoru iSCSI** okně hello stav zařízení mají zobrazit jako **připojeno**.</span><span class="sxs-lookup"><span data-stu-id="35199-154">On hello **Targets** tab of hello **iSCSI Initiator Properties** window, hello device status should appear as **Connected**.</span></span> <span data-ttu-id="35199-155">Pokud používáte zařízení StorSimple 1200, pak každý svazek se připojí jako cíl iSCSI jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="35199-155">If you are using a StorSimple 1200 device, then each volume will be mounted as an iSCSI target as shown below.</span></span> <span data-ttu-id="35199-156">Proto nutné kroky 3 a 4 toobe opakuje pro každý svazek.</span><span class="sxs-lookup"><span data-stu-id="35199-156">Hence, steps  3-4 will need toobe repeated for each volume.</span></span>
   
    ![Svazky připojené jako samostatné cíle](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="35199-158">Pokud změníte název iSCSI hello, hello nový název se použije pro nové relace iSCSI.</span><span class="sxs-lookup"><span data-stu-id="35199-158">If you change hello iSCSI name, hello new name will be used for new iSCSI sessions.</span></span> <span data-ttu-id="35199-159">Nové nastavení nejsou znovu použít pro existující až do odhlášení a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="35199-159">New settings are not used for existing sessions until you log off and log on again.</span></span>
   > 
   > 

<span data-ttu-id="35199-160">Další informace o konfiguraci CHAP na hostitelském serveru Windows hello přejděte příliš[další aspekty](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="35199-160">For more information about configuring CHAP on hello Windows host server, go too[Additional considerations](#additional-considerations).</span></span>

## <a name="bidirectional-or-mutual-authentication"></a><span data-ttu-id="35199-161">Obousměrné nebo vzájemné ověřování</span><span class="sxs-lookup"><span data-stu-id="35199-161">Bidirectional or mutual authentication</span></span>
<span data-ttu-id="35199-162">V obousměrné ověřování hello cíl ověřuje hello iniciátor a pak hello iniciátor ověřuje hello cíl.</span><span class="sxs-lookup"><span data-stu-id="35199-162">In bidirectional authentication, hello target authenticates hello initiator and then hello initiator authenticates hello target.</span></span> <span data-ttu-id="35199-163">To vyžaduje nastavení iniciátoru CHAP tooconfigure hello aplikace hello uživatele, jakož i hello reverse CHAP nastavení na zařízení hello a iSCSI software Initiator na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="35199-163">This requires hello user tooconfigure hello CHAP initiator settings, as well as hello reverse CHAP settings on hello device and iSCSI Initiator software on hello host.</span></span> <span data-ttu-id="35199-164">Hello následující postupy popisují hello kroky tooconfigure vzájemné ověřování na hello zařízení a na hostitele Windows hello.</span><span class="sxs-lookup"><span data-stu-id="35199-164">hello following procedures describe hello steps tooconfigure mutual authentication on hello device and on hello Windows host.</span></span>

#### <a name="tooconfigure-your-device-for-mutual-authentication"></a><span data-ttu-id="35199-165">tooconfigure zařízení pro vzájemné ověřování</span><span class="sxs-lookup"><span data-stu-id="35199-165">tooconfigure your device for mutual authentication</span></span>
1. <span data-ttu-id="35199-166">V portálu Azure classic, na hello hello **zařízení** klikněte na tlačítko hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="35199-166">In hello Azure classic portal, on hello **Devices** page, click hello **Configure** tab.</span></span>
   
    ![Cíle CHAP](./media/storsimple-configure-chap/IC740948.png)
2. <span data-ttu-id="35199-168">Posuňte se dolů na této stránce a hello **CHAP cíl** části:</span><span class="sxs-lookup"><span data-stu-id="35199-168">Scroll down on this page, and in hello **CHAP Target** section:</span></span>
   
   1. <span data-ttu-id="35199-169">Zadejte **Reverse CHAP uživatelské jméno** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="35199-169">Provide a **Reverse CHAP user name** for your device.</span></span>
   2. <span data-ttu-id="35199-170">Zadejte **hesla Reverse CHAP** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="35199-170">Supply a **Reverse CHAP password** for your device.</span></span>
   3. <span data-ttu-id="35199-171">Potvrzení hesla hello.</span><span class="sxs-lookup"><span data-stu-id="35199-171">Confirm hello password.</span></span>
3. <span data-ttu-id="35199-172">V hello **iniciátoru CHAP** části:</span><span class="sxs-lookup"><span data-stu-id="35199-172">In hello **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="35199-173">Zadejte **uživatelské jméno** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="35199-173">Provide a **user name** for your device.</span></span>
   2. <span data-ttu-id="35199-174">Zadejte **heslo** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="35199-174">Provide a **password** for your device.</span></span>
   3. <span data-ttu-id="35199-175">Potvrzení hesla hello.</span><span class="sxs-lookup"><span data-stu-id="35199-175">Confirm hello password.</span></span>
4. <span data-ttu-id="35199-176">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="35199-176">Click **Save**.</span></span> <span data-ttu-id="35199-177">Zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="35199-177">A confirmation message will be displayed.</span></span> <span data-ttu-id="35199-178">Klikněte na tlačítko **OK** toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="35199-178">Click **OK** toosave hello changes.</span></span>

#### <a name="tooconfigure-bidirectional-authentication-on-hello-windows-host-server"></a><span data-ttu-id="35199-179">tooconfigure obousměrné ověřování v systému Windows hello hostitelském serveru</span><span class="sxs-lookup"><span data-stu-id="35199-179">tooconfigure bidirectional authentication on hello Windows host server</span></span>
1. <span data-ttu-id="35199-180">Na serveru hostitele Windows hello spusťte iniciátor iSCSI hello.</span><span class="sxs-lookup"><span data-stu-id="35199-180">On hello Windows host server, start hello iSCSI Initiator.</span></span>
2. <span data-ttu-id="35199-181">V hello **vlastnosti iniciátoru iSCSI** okně klikněte na tlačítko hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="35199-181">In hello **iSCSI Initiator Properties** window, click hello **Configuration** tab.</span></span>
3. <span data-ttu-id="35199-182">Klikněte na tlačítko **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="35199-182">Click **CHAP**.</span></span>
4. <span data-ttu-id="35199-183">V hello **tajný klíč pro vzájemné CHAP iniciátor iSCSI** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="35199-183">In hello **iSCSI Initiator Mutual CHAP Secret** dialog box:</span></span>
   
   1. <span data-ttu-id="35199-184">Typ hello **hesla Reverse CHAP** který jste nakonfigurovali v hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="35199-184">Type hello **Reverse CHAP Password** that you configured in hello Azure classic portal.</span></span>
   2. <span data-ttu-id="35199-185">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="35199-185">Click **OK**.</span></span>
      
       ![iSCSI initiator vzájemné CHAP tajný klíč](./media/storsimple-configure-chap/IC740949.png)
5. <span data-ttu-id="35199-187">Klikněte na tlačítko hello **cíle** kartě.</span><span class="sxs-lookup"><span data-stu-id="35199-187">Click hello **Targets** tab.</span></span>
6. <span data-ttu-id="35199-188">Klikněte na tlačítko hello **Connect** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="35199-188">Click hello **Connect** button.</span></span> 
7. <span data-ttu-id="35199-189">V hello **připojit tooTarget** dialogové okno, klikněte na tlačítko **Upřesnit**.</span><span class="sxs-lookup"><span data-stu-id="35199-189">In hello **Connect tooTarget** dialog box, click **Advanced**.</span></span>
8. <span data-ttu-id="35199-190">V hello **Upřesnit vlastnosti** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="35199-190">In hello **Advanced Properties** dialog box:</span></span>
   
   1. <span data-ttu-id="35199-191">Vyberte hello **povolit CHAP přihlásit** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="35199-191">Select hello **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="35199-192">V hello **název** pole, zadejte hello uživatelské jméno, které jste zadali pro hello iniciátoru CHAP portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="35199-192">In hello **Name** field, supply hello user name that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   3. <span data-ttu-id="35199-193">V hello **tajný klíč cíle** pole, zadejte hello heslo, které jste zadali pro hello iniciátoru CHAP portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="35199-193">In hello **Target secret** field, supply hello password that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   4. <span data-ttu-id="35199-194">Vyberte hello **provést vzájemné ověřování** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="35199-194">Select hello **Perform mutual authentication** check box.</span></span>
      
       ![Upřesnit nastavení vzájemného ověření](./media/storsimple-configure-chap/IC740950.png)
   5. <span data-ttu-id="35199-196">Klikněte na tlačítko **OK** toocomplete hello CHAP konfigurace</span><span class="sxs-lookup"><span data-stu-id="35199-196">Click **OK** toocomplete hello CHAP configuration</span></span>

<span data-ttu-id="35199-197">Další informace o konfiguraci CHAP na hostitelském serveru Windows hello přejděte příliš[další aspekty](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="35199-197">For more information about configuring CHAP on hello Windows host server, go too[Additional considerations](#additional-considerations).</span></span>

## <a name="additional-considerations"></a><span data-ttu-id="35199-198">Další aspekty</span><span class="sxs-lookup"><span data-stu-id="35199-198">Additional considerations</span></span>
<span data-ttu-id="35199-199">Hello **rychlé připojení** funkce nepodporuje připojení, které mají povolené CHAP.</span><span class="sxs-lookup"><span data-stu-id="35199-199">hello **Quick Connect** feature does not support connections that have CHAP enabled.</span></span> <span data-ttu-id="35199-200">Pokud je povoleno CHAP, ujistěte se, že používáte hello **připojit** tlačítko, která je dostupná na hello **cíle** kartě tooconnect tooa cíl.</span><span class="sxs-lookup"><span data-stu-id="35199-200">When CHAP is enabled, make sure that you use hello **Connect** button that is available on hello **Targets** tab tooconnect tooa target.</span></span>

![Připojit tootarget](./media/storsimple-configure-chap/IC740947.png)

<span data-ttu-id="35199-202">V hello **připojit tooTarget** dialogu, který je vidění, vyberte hello **přidat tato připojení toohello seznam oblíbených cíle** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="35199-202">In hello **Connect tooTarget** dialog box that is presented, select hello **Add this connection toohello list of Favorite Targets** check box.</span></span> <span data-ttu-id="35199-203">Tím se zajistí, že při každém restartování počítače hello, je proveden pokus o toorestore hello připojení toohello iSCSI Oblíbené cíle.</span><span class="sxs-lookup"><span data-stu-id="35199-203">This ensures that every time hello computer restarts, an attempt is made toorestore hello connection toohello iSCSI favorite targets.</span></span>

## <a name="errors-during-configuration"></a><span data-ttu-id="35199-204">Chyby během konfigurace</span><span class="sxs-lookup"><span data-stu-id="35199-204">Errors during configuration</span></span>
<span data-ttu-id="35199-205">Pokud vaše konfigurace CHAP je nesprávná, pak se pravděpodobně toosee **selhání ověření** chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="35199-205">If your CHAP configuration is incorrect, then you are likely toosee an **Authentication failure** error message.</span></span>

## <a name="verification-of-chap-configuration"></a><span data-ttu-id="35199-206">Ověření konfigurace CHAP</span><span class="sxs-lookup"><span data-stu-id="35199-206">Verification of CHAP configuration</span></span>
<span data-ttu-id="35199-207">Můžete ověřit, jestli se používá protokol CHAP provedením následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="35199-207">You can verify that CHAP is being used by completing hello following steps.</span></span>

#### <a name="tooverify-your-chap-configuration"></a><span data-ttu-id="35199-208">tooverify konfiguraci CHAP</span><span class="sxs-lookup"><span data-stu-id="35199-208">tooverify your CHAP configuration</span></span>
1. <span data-ttu-id="35199-209">Klikněte na tlačítko **Oblíbené cíle**.</span><span class="sxs-lookup"><span data-stu-id="35199-209">Click **Favorite Targets**.</span></span>
2. <span data-ttu-id="35199-210">Vyberte cíl hello, pro které jste povolili ověřování.</span><span class="sxs-lookup"><span data-stu-id="35199-210">Select hello target for which you enabled authentication.</span></span>
3. <span data-ttu-id="35199-211">Klikněte na tlačítko **podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="35199-211">Click **Details**.</span></span>
   
    ![Oblíbené cíle vlastnosti iniciátoru iSCSI](./media/storsimple-configure-chap/IC740951.png)
4. <span data-ttu-id="35199-213">V hello **Oblíbené podrobnosti cíl** dialogové okno, Poznámka hello položku v hello **ověřování** pole.</span><span class="sxs-lookup"><span data-stu-id="35199-213">In hello **Favorite Target Details** dialog box, note hello entry in hello **Authentication** field.</span></span> <span data-ttu-id="35199-214">Pokud hello konfigurace proběhla úspěšně, by mělo být uvedeno **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="35199-214">If hello configuration was successful, it should say **CHAP**.</span></span>
   
    ![Podrobnosti o oblíbeného cíle](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a><span data-ttu-id="35199-216">Další kroky</span><span class="sxs-lookup"><span data-stu-id="35199-216">Next steps</span></span>
* <span data-ttu-id="35199-217">Další informace o [zabezpečení zařízení StorSimple](storsimple-security.md).</span><span class="sxs-lookup"><span data-stu-id="35199-217">Learn more about [StorSimple security](storsimple-security.md).</span></span>
* <span data-ttu-id="35199-218">Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="35199-218">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

