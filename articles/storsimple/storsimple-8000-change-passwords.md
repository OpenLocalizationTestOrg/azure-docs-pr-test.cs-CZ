---
title: "aaaChange hesla služby StorSimple | Microsoft Docs"
description: "Popisuje, jak toouse hello toochange služby StorSimple Manager zařízení StorSimple Snapshot Manager a zařízení hesla správce."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: cf884be31b4bbf9e372c0aa11b9da2eadcda35dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toochange-your-storsimple-passwords"></a><span data-ttu-id="0ec2f-103">Použít toochange služby StorSimple Manager zařízení hello hesla služby StorSimple</span><span class="sxs-lookup"><span data-stu-id="0ec2f-103">Use hello StorSimple Device Manager service toochange your StorSimple passwords</span></span>

## <a name="overview"></a><span data-ttu-id="0ec2f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="0ec2f-104">Overview</span></span>
<span data-ttu-id="0ec2f-105">portál Azure Hello **nastavení zařízení** možnost obsahuje všechny parametry hello zařízení, které můžete změnit konfiguraci na zařízení StorSimple, který je spravovaný nástrojem service Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-105">hello Azure portal **Device settings** option contains all hello device parameters that you can reconfigure on a StorSimple device that is managed by a StorSimple Device Manager service.</span></span> <span data-ttu-id="0ec2f-106">Tento kurz vysvětluje, jak je možné používat hello **zabezpečení** možnost pod **nastavení zařízení** toochange Správce zařízení nebo heslo Snapshot Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-106">This tutorial explains how you can use hello **Security** option under **Device settings** toochange your device administrator or StorSimple Snapshot Manager password.</span></span>

## <a name="change-hello-device-administrator-password"></a><span data-ttu-id="0ec2f-107">Heslo správce zařízení hello změn</span><span class="sxs-lookup"><span data-stu-id="0ec2f-107">Change hello device administrator password</span></span>
<span data-ttu-id="0ec2f-108">Pokud používáte zařízení StorSimple hello tooaccess rozhraní Windows PowerShell, se vyžaduje tooenter hesla správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-108">When you use Windows PowerShell interface tooaccess hello StorSimple device, you are required tooenter a device administrator password.</span></span> <span data-ttu-id="0ec2f-109">Pokud služba není zaregistrována hello prvního zařízení StorSimple, hello výchozí heslo pro toto rozhraní je *Heslo1*.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-109">When hello first StorSimple device is registered with a service, hello default password for this interface is *Password1*.</span></span> <span data-ttu-id="0ec2f-110">Hello zabezpečení vašich dat, se vyžaduje toochange toto heslo v hello konci procesu registrace hello.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-110">For hello security of your data, you are required toochange this password at hello end of hello registration process.</span></span> <span data-ttu-id="0ec2f-111">Nelze ukončit proces registrace hello beze změny toto heslo.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-111">You cannot exit from hello registration process without changing this password.</span></span> <span data-ttu-id="0ec2f-112">Další informace najdete v tématu [krok 3: Konfigurace a registrace zařízení hello pomocí Windows Powershellu pro StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="0ec2f-112">For more information, see [Step 3: Configure and register hello device through Windows PowerShell for StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).</span></span>

<span data-ttu-id="0ec2f-113">Hello heslo, které se nejprve nastavit pomocí rozhraní Windows PowerShell hello během registrace může prostřednictvím portálu Azure hello později změnit.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-113">hello password that was first set through hello Windows PowerShell interface during registration can be changed later via hello Azure portal.</span></span> <span data-ttu-id="0ec2f-114">Proveďte následující heslo správce zařízení hello toochange kroky hello.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-114">Perform hello following steps toochange hello device administrator password.</span></span>

#### <a name="toochange-hello-device-administrator-password"></a><span data-ttu-id="0ec2f-115">heslo správce zařízení toochange hello</span><span class="sxs-lookup"><span data-stu-id="0ec2f-115">toochange hello device administrator password</span></span>
1. <span data-ttu-id="0ec2f-116">Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-116">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span>

2. <span data-ttu-id="0ec2f-117">Hello tabulkové seznam všech zařízení, vyberte a klikněte na hello zařízení, jehož heslo chcete toochange.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-117">From hello tabular listing of devices, select and click hello device whose password you intend toochange.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. <span data-ttu-id="0ec2f-118">V hello **nastavení** okně přejděte příliš**nastavení zařízení > zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-118">In hello **Settings** blade, go too**Device settings > Security**.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. <span data-ttu-id="0ec2f-119">V hello **nastavení zabezpečení** okně klikněte na tlačítko **heslo** hesla správce zařízení toochange hello.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-119">In hello **Security settings** blade, click **Password** toochange hello device administrator password.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd3.png)

5. <span data-ttu-id="0ec2f-120">V hello **heslo** okno, zadejte heslo správce, který obsahuje z 8 znaků too15.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-120">In hello **Password** blade, provide an administrator password that contains from 8 too15 characters.</span></span> <span data-ttu-id="0ec2f-121">Hello heslo musí být kombinací 3 nebo více velká písmena, malá písmena, číselné a speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-121">hello password must be a combination of 3 or more of uppercase, lowercase, numeric, and special characters.</span></span>

6. <span data-ttu-id="0ec2f-122">Potvrzení hesla hello.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-122">Confirm hello password.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd4.png)

7. <span data-ttu-id="0ec2f-123">Klikněte na tlačítko **Uložit** a po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-123">Click **Save** and when prompted for confirmation, click **Yes**.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd6.png)

<span data-ttu-id="0ec2f-124">Nyní je třeba aktualizovat heslo správce zařízení Hello.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-124">hello device administrator password should now be updated.</span></span> <span data-ttu-id="0ec2f-125">Můžete použít rozhraní Windows PowerShell hello tooaccess této změny hesla.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-125">You can use this modified password tooaccess hello Windows PowerShell interface.</span></span>

## <a name="set-hello-storsimple-snapshot-manager-password"></a><span data-ttu-id="0ec2f-126">Nastavení hesla Snapshot Manageru zařízení StorSimple hello</span><span class="sxs-lookup"><span data-stu-id="0ec2f-126">Set hello StorSimple Snapshot Manager password</span></span>
<span data-ttu-id="0ec2f-127">Software Snapshot Manager zařízení StorSimple se nachází na hostiteli s Windows a umožňuje správci toomanage zálohy zařízení StorSimple ve formě hello místních a cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-127">StorSimple Snapshot Manager software resides on your Windows host and allows administrators toomanage backups of your StorSimple device in hello form of local and cloud snapshots.</span></span>

<span data-ttu-id="0ec2f-128">Při konfiguraci zařízení ve Snapshot Manageru zařízení StorSimple, zobrazí se výzva tooprovide hello zařízení IP adresu a heslo tooauthenticate zařízení úložiště.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-128">When configuring a device in StorSimple Snapshot Manager, you will be prompted tooprovide hello device IP address and password tooauthenticate your storage device.</span></span>

<span data-ttu-id="0ec2f-129">Můžete nastavit nebo změnit hello heslo pro StorSimple Snapshot Manager prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-129">You can set or change hello password for StorSimple Snapshot Manager via hello Azure portal.</span></span> <span data-ttu-id="0ec2f-130">Proveďte následující kroky tooset hello nebo změnit heslo Snapshot Manager zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-130">Perform hello following steps tooset or change hello StorSimple Snapshot Manager password.</span></span>

#### <a name="tooset-hello-storsimple-snapshot-manager-password"></a><span data-ttu-id="0ec2f-131">heslo Snapshot Manager zařízení StorSimple tooset hello</span><span class="sxs-lookup"><span data-stu-id="0ec2f-131">tooset hello StorSimple Snapshot Manager password</span></span>
1. <span data-ttu-id="0ec2f-132">Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-132">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span>

2. <span data-ttu-id="0ec2f-133">Hello tabulkové seznam všech zařízení, vyberte a klikněte na zařízení hello jehož heslo Snapshot Manager zařízení StorSimple úmyslu tooset nebo změnit.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-133">From hello tabular listing of devices, select and click hello device whose StorSimple Snapshot Manager password you intend tooset or change.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. <span data-ttu-id="0ec2f-134">V hello **nastavení** okně přejděte příliš**nastavení zařízení > zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-134">In hello **Settings** blade, go too**Device settings > Security**.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. <span data-ttu-id="0ec2f-135">V hello **nastavení zabezpečení** okně klikněte na tlačítko **heslo** tooset nebo změňte heslo Snapshot Manager zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-135">In hello **Security settings** blade, click **Password** tooset or change hello StorSimple Snapshot Manager password.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd3.png) 

5. <span data-ttu-id="0ec2f-136">V hello **heslo** okno, zadejte heslo, které je tvořeno 14 až 15 znaků.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-136">In hello **Password** blade, enter a password that is 14 or 15 characters.</span></span> <span data-ttu-id="0ec2f-137">Zajistěte, aby že toto heslo hello obsahuje kombinaci 3 nebo více velká písmena, malá písmena, číselné a speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-137">Make sure that hello password contains a combination of 3 or more of uppercase, lowercase, numeric, and special characters.</span></span>

6. <span data-ttu-id="0ec2f-138">Potvrzení hesla hello.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-138">Confirm hello password.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd5.png)

7. <span data-ttu-id="0ec2f-139">Klikněte na tlačítko **Uložit** a po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-139">Click **Save** and when prompted for confirmation, click **Yes**.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd6.png)

<span data-ttu-id="0ec2f-140">Nyní je třeba aktualizovat heslo Snapshot Manager zařízení StorSimple Hello.</span><span class="sxs-lookup"><span data-stu-id="0ec2f-140">hello StorSimple Snapshot Manager password should now be updated.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ec2f-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0ec2f-141">Next steps</span></span>
* <span data-ttu-id="0ec2f-142">Další informace o [zabezpečení zařízení StorSimple](storsimple-8000-security.md).</span><span class="sxs-lookup"><span data-stu-id="0ec2f-142">Learn more about [StorSimple security](storsimple-8000-security.md).</span></span>
* <span data-ttu-id="0ec2f-143">Další informace o [úprava konfiguraci zařízení](storsimple-8000-modify-device-config.md).</span><span class="sxs-lookup"><span data-stu-id="0ec2f-143">Learn more about [modifying your device configuration](storsimple-8000-modify-device-config.md).</span></span>
* <span data-ttu-id="0ec2f-144">Další informace o [pomocí hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="0ec2f-144">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

