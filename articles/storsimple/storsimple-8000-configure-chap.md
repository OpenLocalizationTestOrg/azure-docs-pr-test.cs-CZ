---
title: "aaaConfigure CHAP pro zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje, jak tooconfigure hello Challenge Handshake Authentication Protocol (CHAP) na zařízení StorSimple."
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
ms.openlocfilehash: 3351184b0317da7e3deae398bc0d63c3e5bd930f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a><span data-ttu-id="2ad85-103">Konfigurace protokolů CHAP pro vaše zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="2ad85-103">Configure CHAP for your StorSimple device</span></span>

<span data-ttu-id="2ad85-104">Tento kurz popisuje, jak tooconfigure CHAP pro vaše zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="2ad85-104">This tutorial explains how tooconfigure CHAP for your StorSimple device.</span></span> <span data-ttu-id="2ad85-105">Hello postup popsaný v tomto článku platí tooStorSimple 8000 řady zařízení.</span><span class="sxs-lookup"><span data-stu-id="2ad85-105">hello procedure detailed in this article applies tooStorSimple 8000 series devices.</span></span>

<span data-ttu-id="2ad85-106">CHAP znamená Challenge Handshake Authentication Protocol.</span><span class="sxs-lookup"><span data-stu-id="2ad85-106">CHAP stands for Challenge Handshake Authentication Protocol.</span></span> <span data-ttu-id="2ad85-107">Je příslušné schéma ověřování používané servery toovalidate hello identitu vzdálených klientů.</span><span class="sxs-lookup"><span data-stu-id="2ad85-107">It is an authentication scheme used by servers toovalidate hello identity of remote clients.</span></span> <span data-ttu-id="2ad85-108">ověření Hello je založena na sdílené heslo nebo tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="2ad85-108">hello verification is based on a shared password or secret.</span></span> <span data-ttu-id="2ad85-109">CHAP může být jeden ze způsobů (Jednosměrný) nebo vzájemné (obousměrné).</span><span class="sxs-lookup"><span data-stu-id="2ad85-109">CHAP can be one way (unidirectional) or mutual (bidirectional).</span></span> <span data-ttu-id="2ad85-110">Jedním ze způsobů CHAP je při hello cíl ověření iniciátor.</span><span class="sxs-lookup"><span data-stu-id="2ad85-110">One way CHAP is when hello target authenticates an initiator.</span></span> <span data-ttu-id="2ad85-111">V vzájemné nebo zpětného CHAP cílový hello ověřuje hello iniciátor a pak hello iniciátor ověřuje hello cíl.</span><span class="sxs-lookup"><span data-stu-id="2ad85-111">In mutual or reverse CHAP, hello target authenticates hello initiator and then hello initiator authenticates hello target.</span></span> <span data-ttu-id="2ad85-112">Iniciátor ověřování se dá implementovat bez cíl ověřování.</span><span class="sxs-lookup"><span data-stu-id="2ad85-112">Initiator authentication can be implemented without target authentication.</span></span> <span data-ttu-id="2ad85-113">Cíl ověřování však můžete implementovat, pouze v případě, že se také implementuje iniciátor ověřování.</span><span class="sxs-lookup"><span data-stu-id="2ad85-113">However, target authentication can be implemented only if initiator authentication is also implemented.</span></span>

<span data-ttu-id="2ad85-114">Jako osvědčený postup doporučujeme použít CHAP tooenhance iSCSI zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2ad85-114">As a best practice, we recommend that you use CHAP tooenhance iSCSI security.</span></span>

> [!NOTE]
> <span data-ttu-id="2ad85-115">Uvědomte si, že protokol IPSEC není aktuálně podporován zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="2ad85-115">Keep in mind that IPSEC is not currently supported on StorSimple devices.</span></span>

<span data-ttu-id="2ad85-116">Hello CHAP nastavení v zařízení StorSimple hello se dá nakonfigurovat v hello následující způsoby:</span><span class="sxs-lookup"><span data-stu-id="2ad85-116">hello CHAP settings on hello StorSimple device can be configured in hello following ways:</span></span>

* <span data-ttu-id="2ad85-117">Jednosměrný nebo jednosměrné ověřování</span><span class="sxs-lookup"><span data-stu-id="2ad85-117">Unidirectional or one-way authentication</span></span>
* <span data-ttu-id="2ad85-118">Obousměrné nebo vzájemné nebo zpětného ověřování</span><span class="sxs-lookup"><span data-stu-id="2ad85-118">Bidirectional or mutual or reverse authentication</span></span>

<span data-ttu-id="2ad85-119">V každé z těchto případů musí hello portál pro hello zařízení a softwarem iniciátoru iSCSI server hello toobe nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="2ad85-119">In each of these cases, hello portal for hello device and hello server iSCSI initiator software needs toobe configured.</span></span> <span data-ttu-id="2ad85-120">Hello podrobné pokyny pro tuto konfiguraci jsou popsány v následujícím kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="2ad85-120">hello detailed steps for this configuration are described in hello following tutorial.</span></span>

## <a name="unidirectional-or-one-way-authentication"></a><span data-ttu-id="2ad85-121">Jednosměrný nebo jednosměrné ověřování</span><span class="sxs-lookup"><span data-stu-id="2ad85-121">Unidirectional or one-way authentication</span></span>

<span data-ttu-id="2ad85-122">V jednosměrný ověřování ověřuje hello cíl hello iniciátor.</span><span class="sxs-lookup"><span data-stu-id="2ad85-122">In unidirectional authentication, hello target authenticates hello initiator.</span></span> <span data-ttu-id="2ad85-123">Toto ověřování vyžaduje, konfigurovat nastavení iniciátoru CHAP hello v zařízení StorSimple hello a hello iSCSI software Initiator na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="2ad85-123">This authentication requires that you configure hello CHAP initiator settings on hello StorSimple device and hello iSCSI Initiator software on hello host.</span></span> <span data-ttu-id="2ad85-124">Hello podrobné postupy pro zařízení StorSimple a hostitele Windows jsou popsané dále.</span><span class="sxs-lookup"><span data-stu-id="2ad85-124">hello detailed procedures for your StorSimple device and Windows host are described next.</span></span>

#### <a name="tooconfigure-your-device-for-one-way-authentication"></a><span data-ttu-id="2ad85-125">tooconfigure zařízení pro jednosměrné ověřování</span><span class="sxs-lookup"><span data-stu-id="2ad85-125">tooconfigure your device for one-way authentication</span></span>

1. <span data-ttu-id="2ad85-126">V hello portálu Azure přejděte služby StorSimple Manager zařízení tooyour.</span><span class="sxs-lookup"><span data-stu-id="2ad85-126">In hello Azure portal, go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="2ad85-127">Klikněte na tlačítko **zařízení** a vyberte a klikněte na zařízení, které chcete tooconfigure CHAP pro.</span><span class="sxs-lookup"><span data-stu-id="2ad85-127">Click **Devices** and select and click a device you wish tooconfigure CHAP for.</span></span> <span data-ttu-id="2ad85-128">Přejděte příliš**nastavení zařízení > zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-128">Go too**Device settings > Security**.</span></span> <span data-ttu-id="2ad85-129">V hello **nastavení zabezpečení** okně klikněte na tlačítko **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-129">In hello **Security settings** blade, click **CHAP**.</span></span>
   
    ![Iniciátoru CHAP](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. <span data-ttu-id="2ad85-131">V hello **CHAP** okno a v hello **iniciátoru CHAP** části:</span><span class="sxs-lookup"><span data-stu-id="2ad85-131">In hello **CHAP** blade, and in hello **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="2ad85-132">Zadejte uživatelské jméno pro vaše iniciátoru CHAP.</span><span class="sxs-lookup"><span data-stu-id="2ad85-132">Provide a user name for your CHAP initiator.</span></span>
   2. <span data-ttu-id="2ad85-133">Zadejte heslo pro vaše iniciátoru CHAP.</span><span class="sxs-lookup"><span data-stu-id="2ad85-133">Supply a password for your CHAP initiator.</span></span>
      
    > [!IMPORTANT]
    > <span data-ttu-id="2ad85-134">Hello CHAP uživatelské jméno musí obsahovat méně než 233 znaků.</span><span class="sxs-lookup"><span data-stu-id="2ad85-134">hello CHAP user name must contain fewer than 233 characters.</span></span> <span data-ttu-id="2ad85-135">Hello CHAP heslo musí být 12 až 16 znaků.</span><span class="sxs-lookup"><span data-stu-id="2ad85-135">hello CHAP password must be between 12 and 16 characters.</span></span> <span data-ttu-id="2ad85-136">Delší uživatelské jméno nebo heslo vede k chybě ověřování na hostitele Windows hello.</span><span class="sxs-lookup"><span data-stu-id="2ad85-136">A longer user name or password results in an authentication failure on hello Windows host.</span></span>
   
   3. <span data-ttu-id="2ad85-137">Potvrzení hesla hello.</span><span class="sxs-lookup"><span data-stu-id="2ad85-137">Confirm hello password.</span></span>

       ![Iniciátoru CHAP](./media/storsimple-8000-configure-chap/configure-chap6.png)
3. <span data-ttu-id="2ad85-139">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-139">Click **Save**.</span></span> <span data-ttu-id="2ad85-140">Se zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="2ad85-140">A confirmation message is displayed.</span></span> <span data-ttu-id="2ad85-141">Klikněte na tlačítko **OK** toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="2ad85-141">Click **OK** toosave hello changes.</span></span>

#### <a name="tooconfigure-one-way-authentication-on-hello-windows-host-server"></a><span data-ttu-id="2ad85-142">tooconfigure jednosměrné ověřování v systému Windows hello hostitelském serveru</span><span class="sxs-lookup"><span data-stu-id="2ad85-142">tooconfigure one-way authentication on hello Windows host server</span></span>
1. <span data-ttu-id="2ad85-143">Na serveru hostitele Windows hello spusťte iniciátor iSCSI hello.</span><span class="sxs-lookup"><span data-stu-id="2ad85-143">On hello Windows host server, start hello iSCSI Initiator.</span></span>
2. <span data-ttu-id="2ad85-144">V hello **vlastnosti iniciátoru iSCSI** okně provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2ad85-144">In hello **iSCSI Initiator Properties** window, perform hello following steps:</span></span>
   
   1. <span data-ttu-id="2ad85-145">Klikněte na tlačítko hello **zjišťování** kartě.</span><span class="sxs-lookup"><span data-stu-id="2ad85-145">Click hello **Discovery** tab.</span></span>
      
       ![Vlastnosti iniciátoru iSCSI](./media/storsimple-configure-chap/IC740944.png)
   2. <span data-ttu-id="2ad85-147">Klikněte na tlačítko **zjistit portál**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-147">Click **Discover Portal**.</span></span>
3. <span data-ttu-id="2ad85-148">V hello **zjistit cílový portál** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="2ad85-148">In hello **Discover Target Portal** dialog box:</span></span>
   
   1. <span data-ttu-id="2ad85-149">Zadejte IP adresu hello vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="2ad85-149">Specify hello IP address of your device.</span></span>
   2. <span data-ttu-id="2ad85-150">Klikněte na tlačítko **rozšířené**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-150">Click **Advanced**.</span></span>
      
       ![Zjistit cílový portál](./media/storsimple-configure-chap/IC740945.png)
4. <span data-ttu-id="2ad85-152">V hello **Upřesnit nastavení** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="2ad85-152">In hello **Advanced Settings** dialog box:</span></span>
   
   1. <span data-ttu-id="2ad85-153">Vyberte hello **povolit CHAP přihlásit** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="2ad85-153">Select hello **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="2ad85-154">V hello **název** pole, zadejte hello uživatelské jméno, které jste zadali pro hello iniciátoru CHAP portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="2ad85-154">In hello **Name** field, supply hello user name that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   3. <span data-ttu-id="2ad85-155">V hello **tajný klíč cíle** pole, zadejte hello heslo, které jste zadali pro hello iniciátoru CHAP portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="2ad85-155">In hello **Target secret** field, supply hello password that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   4. <span data-ttu-id="2ad85-156">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-156">Click **OK**.</span></span>
      
       ![Upřesňující nastavení, obecné](./media/storsimple-configure-chap/IC740946.png)
5. <span data-ttu-id="2ad85-158">Na hello **cíle** kartě hello **vlastnosti iniciátoru iSCSI** okně hello stav zařízení mají zobrazit jako **připojeno**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-158">On hello **Targets** tab of hello **iSCSI Initiator Properties** window, hello device status should appear as **Connected**.</span></span> <span data-ttu-id="2ad85-159">Pokud používáte zařízení StorSimple 1200, každý svazek připojit jako cíle iSCSI.</span><span class="sxs-lookup"><span data-stu-id="2ad85-159">If you are using a StorSimple 1200 device, then each volume is mounted as an iSCSI target.</span></span> <span data-ttu-id="2ad85-160">Proto nutné kroky 3 a 4 toobe opakuje pro každý svazek.</span><span class="sxs-lookup"><span data-stu-id="2ad85-160">Hence, steps 3-4 will need toobe repeated for each volume.</span></span>
   
    ![Svazky připojené jako samostatné cíle](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="2ad85-162">Pokud změníte název iSCSI hello, hello nový název se používá pro nové relace iSCSI.</span><span class="sxs-lookup"><span data-stu-id="2ad85-162">If you change hello iSCSI name, hello new name is used for new iSCSI sessions.</span></span> <span data-ttu-id="2ad85-163">Nové nastavení nejsou znovu použít pro existující až do odhlášení a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="2ad85-163">New settings are not used for existing sessions until you log off and log on again.</span></span>

<span data-ttu-id="2ad85-164">Další informace o konfiguraci CHAP na hostitelském serveru Windows hello přejděte příliš[další aspekty](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="2ad85-164">For more information about configuring CHAP on hello Windows host server, go too[Additional considerations](#additional-considerations).</span></span>

## <a name="bidirectional-or-mutual-authentication"></a><span data-ttu-id="2ad85-165">Obousměrné nebo vzájemné ověřování</span><span class="sxs-lookup"><span data-stu-id="2ad85-165">Bidirectional or mutual authentication</span></span>

<span data-ttu-id="2ad85-166">V obousměrné ověřování hello cíl ověřuje hello iniciátor a pak hello iniciátor ověřuje hello cíl.</span><span class="sxs-lookup"><span data-stu-id="2ad85-166">In bidirectional authentication, hello target authenticates hello initiator and then hello initiator authenticates hello target.</span></span> <span data-ttu-id="2ad85-167">Tento postup vyžaduje nastavení iniciátoru CHAP tooconfigure hello aplikace hello uživatele, reverse CHAP nastavení na zařízení hello a služby iSCSI software Initiator na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="2ad85-167">This procedure requires hello user tooconfigure hello CHAP initiator settings, reverse CHAP settings on hello device, and iSCSI Initiator software on hello host.</span></span> <span data-ttu-id="2ad85-168">Hello následující postupy popisují hello kroky tooconfigure vzájemné ověřování na hello zařízení a na hostitele Windows hello.</span><span class="sxs-lookup"><span data-stu-id="2ad85-168">hello following procedures describe hello steps tooconfigure mutual authentication on hello device and on hello Windows host.</span></span>

#### <a name="tooconfigure-your-device-for-mutual-authentication"></a><span data-ttu-id="2ad85-169">tooconfigure zařízení pro vzájemné ověřování</span><span class="sxs-lookup"><span data-stu-id="2ad85-169">tooconfigure your device for mutual authentication</span></span>

1. <span data-ttu-id="2ad85-170">V hello portálu Azure přejděte služby StorSimple Manager zařízení tooyour.</span><span class="sxs-lookup"><span data-stu-id="2ad85-170">In hello Azure portal, go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="2ad85-171">Klikněte na tlačítko **zařízení** a vyberte a klikněte na zařízení, které chcete tooconfigure CHAP pro.</span><span class="sxs-lookup"><span data-stu-id="2ad85-171">Click **Devices** and select and click a device you wish tooconfigure CHAP for.</span></span> <span data-ttu-id="2ad85-172">Přejděte příliš**nastavení zařízení > zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-172">Go too**Device settings > Security**.</span></span> <span data-ttu-id="2ad85-173">V hello **nastavení zabezpečení** okně klikněte na tlačítko **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-173">In hello **Security settings** blade, click **CHAP**.</span></span>
   
    ![Cíle CHAP](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. <span data-ttu-id="2ad85-175">Posuňte se dolů na této stránce a hello **CHAP cíl** části:</span><span class="sxs-lookup"><span data-stu-id="2ad85-175">Scroll down on this page, and in hello **CHAP Target** section:</span></span>
   
   1. <span data-ttu-id="2ad85-176">Zadejte **Reverse CHAP uživatelské jméno** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="2ad85-176">Provide a **Reverse CHAP user name** for your device.</span></span>
   2. <span data-ttu-id="2ad85-177">Zadejte **hesla Reverse CHAP** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="2ad85-177">Supply a **Reverse CHAP password** for your device.</span></span>
   3. <span data-ttu-id="2ad85-178">Potvrzení hesla hello.</span><span class="sxs-lookup"><span data-stu-id="2ad85-178">Confirm hello password.</span></span>
3. <span data-ttu-id="2ad85-179">V hello **iniciátoru CHAP** části:</span><span class="sxs-lookup"><span data-stu-id="2ad85-179">In hello **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="2ad85-180">Zadejte **uživatelské jméno** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="2ad85-180">Provide a **user name** for your device.</span></span>
   2. <span data-ttu-id="2ad85-181">Zadejte **heslo** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="2ad85-181">Provide a **password** for your device.</span></span>
   3. <span data-ttu-id="2ad85-182">Potvrzení hesla hello.</span><span class="sxs-lookup"><span data-stu-id="2ad85-182">Confirm hello password.</span></span>

       ![Iniciátoru CHAP](./media/storsimple-8000-configure-chap/configure-chap11.png)
4. <span data-ttu-id="2ad85-184">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-184">Click **Save**.</span></span> <span data-ttu-id="2ad85-185">Se zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="2ad85-185">A confirmation message is displayed.</span></span> <span data-ttu-id="2ad85-186">Klikněte na tlačítko **OK** toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="2ad85-186">Click **OK** toosave hello changes.</span></span>

#### <a name="tooconfigure-bidirectional-authentication-on-hello-windows-host-server"></a><span data-ttu-id="2ad85-187">tooconfigure obousměrné ověřování v systému Windows hello hostitelském serveru</span><span class="sxs-lookup"><span data-stu-id="2ad85-187">tooconfigure bidirectional authentication on hello Windows host server</span></span>

1. <span data-ttu-id="2ad85-188">Na serveru hostitele Windows hello spusťte iniciátor iSCSI hello.</span><span class="sxs-lookup"><span data-stu-id="2ad85-188">On hello Windows host server, start hello iSCSI Initiator.</span></span>
2. <span data-ttu-id="2ad85-189">V hello **vlastnosti iniciátoru iSCSI** okně klikněte na tlačítko hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="2ad85-189">In hello **iSCSI Initiator Properties** window, click hello **Configuration** tab.</span></span>
3. <span data-ttu-id="2ad85-190">Klikněte na tlačítko **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-190">Click **CHAP**.</span></span>
4. <span data-ttu-id="2ad85-191">V hello **tajný klíč pro vzájemné CHAP iniciátor iSCSI** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="2ad85-191">In hello **iSCSI Initiator Mutual CHAP Secret** dialog box:</span></span>
   
   1. <span data-ttu-id="2ad85-192">Typ hello **hesla Reverse CHAP** který jste nakonfigurovali v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2ad85-192">Type hello **Reverse CHAP Password** that you configured in hello Azure portal.</span></span>
   2. <span data-ttu-id="2ad85-193">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-193">Click **OK**.</span></span>
      
       ![iSCSI initiator vzájemné CHAP tajný klíč](./media/storsimple-configure-chap/IC740949.png)
5. <span data-ttu-id="2ad85-195">Klikněte na tlačítko hello **cíle** kartě.</span><span class="sxs-lookup"><span data-stu-id="2ad85-195">Click hello **Targets** tab.</span></span>
6. <span data-ttu-id="2ad85-196">Klikněte na tlačítko hello **Connect** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2ad85-196">Click hello **Connect** button.</span></span> 
7. <span data-ttu-id="2ad85-197">V hello **připojit tooTarget** dialogové okno, klikněte na tlačítko **Upřesnit**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-197">In hello **Connect tooTarget** dialog box, click **Advanced**.</span></span>
8. <span data-ttu-id="2ad85-198">V hello **Upřesnit vlastnosti** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="2ad85-198">In hello **Advanced Properties** dialog box:</span></span>
   
   1. <span data-ttu-id="2ad85-199">Vyberte hello **povolit CHAP přihlásit** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="2ad85-199">Select hello **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="2ad85-200">V hello **název** pole, zadejte hello uživatelské jméno, které jste zadali pro hello iniciátoru CHAP portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="2ad85-200">In hello **Name** field, supply hello user name that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   3. <span data-ttu-id="2ad85-201">V hello **tajný klíč cíle** pole, zadejte hello heslo, které jste zadali pro hello iniciátoru CHAP portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="2ad85-201">In hello **Target secret** field, supply hello password that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   4. <span data-ttu-id="2ad85-202">Vyberte hello **provést vzájemné ověřování** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="2ad85-202">Select hello **Perform mutual authentication** check box.</span></span>
      
       ![Upřesnit nastavení vzájemného ověření](./media/storsimple-configure-chap/IC740950.png)
   5. <span data-ttu-id="2ad85-204">Klikněte na tlačítko **OK** toocomplete hello CHAP konfigurace</span><span class="sxs-lookup"><span data-stu-id="2ad85-204">Click **OK** toocomplete hello CHAP configuration</span></span>

<span data-ttu-id="2ad85-205">Další informace o konfiguraci CHAP na hostitelském serveru Windows hello přejděte příliš[další aspekty](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="2ad85-205">For more information about configuring CHAP on hello Windows host server, go too[Additional considerations](#additional-considerations).</span></span>

## <a name="additional-considerations"></a><span data-ttu-id="2ad85-206">Další aspekty</span><span class="sxs-lookup"><span data-stu-id="2ad85-206">Additional considerations</span></span>

<span data-ttu-id="2ad85-207">Hello **rychlé připojení** funkce nepodporuje připojení, které mají povolené CHAP.</span><span class="sxs-lookup"><span data-stu-id="2ad85-207">hello **Quick Connect** feature does not support connections that have CHAP enabled.</span></span> <span data-ttu-id="2ad85-208">Pokud je povoleno CHAP, ujistěte se, že používáte hello **připojit** tlačítko, která je dostupná na hello **cíle** kartě tooconnect tooa cíl.</span><span class="sxs-lookup"><span data-stu-id="2ad85-208">When CHAP is enabled, make sure that you use hello **Connect** button that is available on hello **Targets** tab tooconnect tooa target.</span></span>

![Připojit tootarget](./media/storsimple-configure-chap/IC740947.png)

<span data-ttu-id="2ad85-210">V hello **připojit tooTarget** dialogu, který je vidění, vyberte hello **přidat tato připojení toohello seznam oblíbených cíle** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="2ad85-210">In hello **Connect tooTarget** dialog box that is presented, select hello **Add this connection toohello list of Favorite Targets** check box.</span></span> <span data-ttu-id="2ad85-211">Tato volba zajistí, že při každém restartování počítače hello, je proveden pokus o toorestore hello připojení toohello iSCSI Oblíbené cíle.</span><span class="sxs-lookup"><span data-stu-id="2ad85-211">This selection ensures that every time hello computer restarts, an attempt is made toorestore hello connection toohello iSCSI favorite targets.</span></span>

## <a name="errors-during-configuration"></a><span data-ttu-id="2ad85-212">Chyby během konfigurace</span><span class="sxs-lookup"><span data-stu-id="2ad85-212">Errors during configuration</span></span>

<span data-ttu-id="2ad85-213">Pokud vaše konfigurace CHAP je nesprávná, pak se pravděpodobně toosee **selhání ověření** chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="2ad85-213">If your CHAP configuration is incorrect, then you are likely toosee an **Authentication failure** error message.</span></span>

## <a name="verification-of-chap-configuration"></a><span data-ttu-id="2ad85-214">Ověření konfigurace CHAP</span><span class="sxs-lookup"><span data-stu-id="2ad85-214">Verification of CHAP configuration</span></span>

<span data-ttu-id="2ad85-215">Můžete ověřit, jestli se používá protokol CHAP provedením následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="2ad85-215">You can verify that CHAP is being used by completing hello following steps.</span></span>

#### <a name="tooverify-your-chap-configuration"></a><span data-ttu-id="2ad85-216">tooverify konfiguraci CHAP</span><span class="sxs-lookup"><span data-stu-id="2ad85-216">tooverify your CHAP configuration</span></span>
1. <span data-ttu-id="2ad85-217">Klikněte na tlačítko **Oblíbené cíle**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-217">Click **Favorite Targets**.</span></span>
2. <span data-ttu-id="2ad85-218">Vyberte cíl hello, pro které jste povolili ověřování.</span><span class="sxs-lookup"><span data-stu-id="2ad85-218">Select hello target for which you enabled authentication.</span></span>
3. <span data-ttu-id="2ad85-219">Klikněte na tlačítko **podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-219">Click **Details**.</span></span>
   
    ![Oblíbené cíle vlastnosti iniciátoru iSCSI](./media/storsimple-configure-chap/IC740951.png)
4. <span data-ttu-id="2ad85-221">V hello **Oblíbené podrobnosti cíl** dialogové okno, Poznámka hello položku v hello **ověřování** pole.</span><span class="sxs-lookup"><span data-stu-id="2ad85-221">In hello **Favorite Target Details** dialog box, note hello entry in hello **Authentication** field.</span></span> <span data-ttu-id="2ad85-222">Pokud hello konfigurace proběhla úspěšně, by mělo být uvedeno **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="2ad85-222">If hello configuration was successful, it should say **CHAP**.</span></span>
   
    ![Podrobnosti o oblíbeného cíle](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a><span data-ttu-id="2ad85-224">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2ad85-224">Next steps</span></span>

* <span data-ttu-id="2ad85-225">Další informace o [zabezpečení zařízení StorSimple](storsimple-8000-security.md).</span><span class="sxs-lookup"><span data-stu-id="2ad85-225">Learn more about [StorSimple security](storsimple-8000-security.md).</span></span>
* <span data-ttu-id="2ad85-226">Další informace o [pomocí hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="2ad85-226">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

