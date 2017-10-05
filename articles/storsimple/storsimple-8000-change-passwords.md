---
title: "Změna hesla služby StorSimple | Microsoft Docs"
description: "Popisuje, jak změnit správce hesla služby StorSimple Snapshot Manager a zařízení pomocí služby StorSimple Manager zařízení."
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
ms.openlocfilehash: 7762f8499c67672f0a2ffed99e98baea4c940fa0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-change-your-storsimple-passwords"></a><span data-ttu-id="3b6a7-103">Chcete-li změnit hesla služby StorSimple používat službu Správce zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="3b6a7-103">Use the StorSimple Device Manager service to change your StorSimple passwords</span></span>

## <a name="overview"></a><span data-ttu-id="3b6a7-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="3b6a7-104">Overview</span></span>
<span data-ttu-id="3b6a7-105">Portál Azure **nastavení zařízení** možnost obsahuje všechny parametry zařízení, které můžete změnit konfiguraci na zařízení StorSimple, který je spravovaný nástrojem service Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-105">The Azure portal **Device settings** option contains all the device parameters that you can reconfigure on a StorSimple device that is managed by a StorSimple Device Manager service.</span></span> <span data-ttu-id="3b6a7-106">Tento kurz popisuje, jak můžete použít **zabezpečení** možnost pod **nastavení zařízení** změnit správce zařízení nebo heslo Snapshot Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-106">This tutorial explains how you can use the **Security** option under **Device settings** to change your device administrator or StorSimple Snapshot Manager password.</span></span>

## <a name="change-the-device-administrator-password"></a><span data-ttu-id="3b6a7-107">Změna hesla správce zařízení</span><span class="sxs-lookup"><span data-stu-id="3b6a7-107">Change the device administrator password</span></span>
<span data-ttu-id="3b6a7-108">Pokud použijete rozhraní Windows PowerShell k přístupu k zařízení StorSimple, musíte k zadání hesla správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-108">When you use Windows PowerShell interface to access the StorSimple device, you are required to enter a device administrator password.</span></span> <span data-ttu-id="3b6a7-109">Pokud služba není zaregistrována prvního zařízení StorSimple, výchozí heslo pro toto rozhraní je *Heslo1*.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-109">When the first StorSimple device is registered with a service, the default password for this interface is *Password1*.</span></span> <span data-ttu-id="3b6a7-110">Pro zabezpečení vašich dat musíte změnit toto heslo na konci procesu registrace.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-110">For the security of your data, you are required to change this password at the end of the registration process.</span></span> <span data-ttu-id="3b6a7-111">Nelze ukončit proces registrace beze změny toto heslo.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-111">You cannot exit from the registration process without changing this password.</span></span> <span data-ttu-id="3b6a7-112">Další informace najdete v tématu [krok 3: Konfigurace a registrace zařízení pomocí Windows Powershellu pro StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="3b6a7-112">For more information, see [Step 3: Configure and register the device through Windows PowerShell for StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).</span></span>

<span data-ttu-id="3b6a7-113">Heslo, které se nejdřív nastavit pomocí rozhraní Windows PowerShell během registrace lze později změnit prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-113">The password that was first set through the Windows PowerShell interface during registration can be changed later via the Azure portal.</span></span> <span data-ttu-id="3b6a7-114">Proveďte následující kroky, chcete-li změnit heslo správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-114">Perform the following steps to change the device administrator password.</span></span>

#### <a name="to-change-the-device-administrator-password"></a><span data-ttu-id="3b6a7-115">Chcete-li změnit heslo správce zařízení</span><span class="sxs-lookup"><span data-stu-id="3b6a7-115">To change the device administrator password</span></span>
1. <span data-ttu-id="3b6a7-116">Přejděte do služby Správce zařízení StorSimple a klikněte na **Zařízení**.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-116">Go to your StorSimple Device Manager service and click **Devices**.</span></span>

2. <span data-ttu-id="3b6a7-117">Tabulkový seznam zařízení, vyberte a klikněte na zařízení, jehož heslo chcete změnit.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-117">From the tabular listing of devices, select and click the device whose password you intend to change.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. <span data-ttu-id="3b6a7-118">V **nastavení** okno, přejděte na **nastavení zařízení > zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-118">In the **Settings** blade, go to **Device settings > Security**.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. <span data-ttu-id="3b6a7-119">V **nastavení zabezpečení** okně klikněte na tlačítko **heslo** Změna hesla správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-119">In the **Security settings** blade, click **Password** to change the device administrator password.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd3.png)

5. <span data-ttu-id="3b6a7-120">V **heslo** okno, zadejte heslo správce, který obsahuje 8 až 15 znaků.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-120">In the **Password** blade, provide an administrator password that contains from 8 to 15 characters.</span></span> <span data-ttu-id="3b6a7-121">Heslo musí být kombinací 3 nebo více velká písmena, malá písmena, číselné a speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-121">The password must be a combination of 3 or more of uppercase, lowercase, numeric, and special characters.</span></span>

6. <span data-ttu-id="3b6a7-122">Potvrzení hesla.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-122">Confirm the password.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd4.png)

7. <span data-ttu-id="3b6a7-123">Klikněte na tlačítko **Uložit** a po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-123">Click **Save** and when prompted for confirmation, click **Yes**.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd6.png)

<span data-ttu-id="3b6a7-124">Nyní je třeba aktualizovat heslo správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-124">The device administrator password should now be updated.</span></span> <span data-ttu-id="3b6a7-125">Můžete toto upravené heslo pro přístup k rozhraní Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-125">You can use this modified password to access the Windows PowerShell interface.</span></span>

## <a name="set-the-storsimple-snapshot-manager-password"></a><span data-ttu-id="3b6a7-126">Nastavení hesla Snapshot Manageru zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="3b6a7-126">Set the StorSimple Snapshot Manager password</span></span>
<span data-ttu-id="3b6a7-127">Software Snapshot Manager zařízení StorSimple se nachází na hostiteli systému Windows a umožňuje správcům spravovat zálohy zařízení StorSimple ve formě místních a cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-127">StorSimple Snapshot Manager software resides on your Windows host and allows administrators to manage backups of your StorSimple device in the form of local and cloud snapshots.</span></span>

<span data-ttu-id="3b6a7-128">Při konfiguraci zařízení ve Snapshot Manageru zařízení StorSimple, budete vyzváni k zadání zařízení IP adresu a heslo k ověření zařízení úložiště.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-128">When configuring a device in StorSimple Snapshot Manager, you will be prompted to provide the device IP address and password to authenticate your storage device.</span></span>

<span data-ttu-id="3b6a7-129">Můžete nastavit nebo změnit heslo pro StorSimple Snapshot Manager prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-129">You can set or change the password for StorSimple Snapshot Manager via the Azure portal.</span></span> <span data-ttu-id="3b6a7-130">Proveďte následující kroky pro nastavení nebo změna hesla Snapshot Manageru zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-130">Perform the following steps to set or change the StorSimple Snapshot Manager password.</span></span>

#### <a name="to-set-the-storsimple-snapshot-manager-password"></a><span data-ttu-id="3b6a7-131">Chcete-li nastavit heslo Snapshot Manager zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="3b6a7-131">To set the StorSimple Snapshot Manager password</span></span>
1. <span data-ttu-id="3b6a7-132">Přejděte do služby Správce zařízení StorSimple a klikněte na **Zařízení**.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-132">Go to your StorSimple Device Manager service and click **Devices**.</span></span>

2. <span data-ttu-id="3b6a7-133">Tabulkový seznam zařízení, vyberte a klikněte na zařízení, jehož heslo Snapshot Manager zařízení StorSimple chcete nastavit nebo změnit.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-133">From the tabular listing of devices, select and click the device whose StorSimple Snapshot Manager password you intend to set or change.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. <span data-ttu-id="3b6a7-134">V **nastavení** okno, přejděte na **nastavení zařízení > zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-134">In the **Settings** blade, go to **Device settings > Security**.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. <span data-ttu-id="3b6a7-135">V **nastavení zabezpečení** okně klikněte na tlačítko **heslo** nastavit nebo změnit heslo Snapshot Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-135">In the **Security settings** blade, click **Password** to set or change the StorSimple Snapshot Manager password.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd3.png) 

5. <span data-ttu-id="3b6a7-136">V **heslo** okno, zadejte heslo, které je tvořeno 14 až 15 znaků.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-136">In the **Password** blade, enter a password that is 14 or 15 characters.</span></span> <span data-ttu-id="3b6a7-137">Ujistěte se, zda heslo obsahuje kombinaci 3 nebo více velká písmena, malá písmena, číselné a speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-137">Make sure that the password contains a combination of 3 or more of uppercase, lowercase, numeric, and special characters.</span></span>

6. <span data-ttu-id="3b6a7-138">Potvrzení hesla.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-138">Confirm the password.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd5.png)

7. <span data-ttu-id="3b6a7-139">Klikněte na tlačítko **Uložit** a po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-139">Click **Save** and when prompted for confirmation, click **Yes**.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd6.png)

<span data-ttu-id="3b6a7-140">Nyní je třeba aktualizovat heslo Snapshot Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3b6a7-140">The StorSimple Snapshot Manager password should now be updated.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3b6a7-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3b6a7-141">Next steps</span></span>
* <span data-ttu-id="3b6a7-142">Další informace o [zabezpečení zařízení StorSimple](storsimple-8000-security.md).</span><span class="sxs-lookup"><span data-stu-id="3b6a7-142">Learn more about [StorSimple security](storsimple-8000-security.md).</span></span>
* <span data-ttu-id="3b6a7-143">Další informace o [úprava konfiguraci zařízení](storsimple-8000-modify-device-config.md).</span><span class="sxs-lookup"><span data-stu-id="3b6a7-143">Learn more about [modifying your device configuration](storsimple-8000-modify-device-config.md).</span></span>
* <span data-ttu-id="3b6a7-144">Další informace o [pomocí služby StorSimple Manager zařízení ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="3b6a7-144">Learn more about [using the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

