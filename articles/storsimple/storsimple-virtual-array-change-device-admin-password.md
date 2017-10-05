---
title: "Heslo správce zařízení změnu pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak změnit heslo správce zařízení pomocí portálu Azure nebo pole virtuální zařízení StorSimple webového uživatelského rozhraní."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 11490814-d9fd-4dc7-9c3b-55dd2c23eaf1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 260a23003d705e6598da8c51bb5a96f2539a0014
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-storsimple-virtual-array-device-administrator-password-via-storsimple-device-manager"></a><span data-ttu-id="63a01-103">Změna hesla správce zařízení pole virtuální zařízení StorSimple pomocí Správce zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="63a01-103">Change the StorSimple Virtual Array device administrator password via StorSimple Device Manager</span></span>

## <a name="overview"></a><span data-ttu-id="63a01-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="63a01-104">Overview</span></span>

<span data-ttu-id="63a01-105">Pokud použijete rozhraní Windows PowerShell pro přístup k poli virtuální zařízení StorSimple, musíte k zadání hesla správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="63a01-105">When you use the Windows PowerShell interface to access the StorSimple Virtual Array, you are required to enter a device administrator password.</span></span> <span data-ttu-id="63a01-106">Když zařízení StorSimple je nejdřív zřízený a spuštěna, je výchozí heslo *Heslo1*.</span><span class="sxs-lookup"><span data-stu-id="63a01-106">When the StorSimple device is first provisioned and started, the default password is *Password1*.</span></span> <span data-ttu-id="63a01-107">Pro zabezpečení vašich dat výchozí heslo vyprší platnost při prvním přihlášení a je nutné toto heslo změnit.</span><span class="sxs-lookup"><span data-stu-id="63a01-107">For the security of your data, the default password expires the first time that you sign in and you are required to change this password.</span></span>

<span data-ttu-id="63a01-108">Změna hesla správce zařízení kdykoli po nasazení zařízení v provozním prostředí můžete také použít místní webového uživatelského rozhraní nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="63a01-108">You can also use either the local web UI or the Azure portal to change the device administrator password at any time after the device is deployed in your production environment.</span></span> <span data-ttu-id="63a01-109">Každá z těchto postupů je popsána v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="63a01-109">Each of these procedures is described in this article.</span></span>

 ![Okno zařízení](./media/storsimple-virtual-array-change-device-admin-password/ova-devices-blade.png)

## <a name="use-the-azure-portal-to-change-the-password"></a><span data-ttu-id="63a01-111">Použijte portál Azure. Chcete-li změnit heslo</span><span class="sxs-lookup"><span data-stu-id="63a01-111">Use the Azure portal to change the password</span></span>

<span data-ttu-id="63a01-112">Proveďte následující kroky, chcete-li změnit heslo správce zařízení prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="63a01-112">Perform the following steps to change the device administrator password through the Azure portal.</span></span>

#### <a name="to-change-the-device-administrator-password-via-the-azure-portal"></a><span data-ttu-id="63a01-113">Chcete-li změnit heslo správce zařízení prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="63a01-113">To change the device administrator password via the Azure portal</span></span>

1. <span data-ttu-id="63a01-114">Na stránce cílové služby, vyberte svoji službu, dvakrát klikněte na název služby a pak v rámci **správy** klikněte na tlačítko **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="63a01-114">On the service landing page, select your service, double-click the service name, and then within the **Management** section, click **Devices**.</span></span> <span data-ttu-id="63a01-115">Tím se otevře **zařízení** okno, které obsahuje seznamy všech zařízení StorSimple virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="63a01-115">This opens the **Devices** blade that lists all your StorSimple Virtual Array devices.</span></span>

2. <span data-ttu-id="63a01-116">V **zařízení** okno, dvakrát klikněte na zařízení, která vyžaduje změnu hesla.</span><span class="sxs-lookup"><span data-stu-id="63a01-116">In the **Devices** blade, double-click the device that requires a change of password.</span></span>

3. <span data-ttu-id="63a01-117">V **nastavení** pro zařízení, klikněte na **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="63a01-117">In the **Settings** blade for your device, click **Security**.</span></span>

4. <span data-ttu-id="63a01-118">V **nastavení zabezpečení** okno, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="63a01-118">In the **Security Settings** blade, do the following:</span></span>
   
   1. <span data-ttu-id="63a01-119">Přejděte dolů k položce **heslo správce zařízení** části.</span><span class="sxs-lookup"><span data-stu-id="63a01-119">Scroll down to the **Device Administrator Password** section.</span></span> <span data-ttu-id="63a01-120">Zadejte heslo správce, který obsahuje 8 až 15 znaků.</span><span class="sxs-lookup"><span data-stu-id="63a01-120">Provide an administrator password that contains from 8 to 15 characters.</span></span>
   2. <span data-ttu-id="63a01-121">Potvrzení hesla.</span><span class="sxs-lookup"><span data-stu-id="63a01-121">Confirm the password.</span></span>
   3. <span data-ttu-id="63a01-122">Klikněte na tlačítko **Uložit** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="63a01-122">Click **Save** at the top of the blade.</span></span>

<span data-ttu-id="63a01-123">Heslo správce zařízení se nyní aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="63a01-123">The device administrator password is now updated.</span></span> <span data-ttu-id="63a01-124">Toto upravené heslo slouží k přístupu k zařízení místně.</span><span class="sxs-lookup"><span data-stu-id="63a01-124">You can use this modified password to access the device locally.</span></span>

![Okno nastavení zabezpečení](./media/storsimple-virtual-array-change-device-admin-password/ova-change-device-pwd.png)

## <a name="use-the-local-web-ui-to-change-the-password"></a><span data-ttu-id="63a01-126">Chcete-li změnit heslo použít místní webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="63a01-126">Use the local web UI to change the password</span></span>

<span data-ttu-id="63a01-127">Proveďte následující kroky, chcete-li změnit heslo správce zařízení prostřednictvím místního webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="63a01-127">Perform the following steps to change the device administrator password through the local web UI.</span></span>

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a><span data-ttu-id="63a01-128">Chcete-li změnit heslo správce zařízení prostřednictvím místního webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="63a01-128">To change the device administrator password via the local web UI</span></span>

1. <span data-ttu-id="63a01-129">V místní webového uživatelského rozhraní, klikněte na **údržby** > **Změna hesla** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="63a01-129">In the local web UI, click **Maintenance** > **Password change** for your device.</span></span>
   
    ![změnit Heslo1](./media/storsimple-virtual-array-change-device-admin-password/image40.png)
2. <span data-ttu-id="63a01-131">Zadejte **aktuální heslo**.</span><span class="sxs-lookup"><span data-stu-id="63a01-131">Enter the **Current password**.</span></span>
3. <span data-ttu-id="63a01-132">Zadejte **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="63a01-132">Provide a **New Password**.</span></span> <span data-ttu-id="63a01-133">Heslo musí být dlouhé alespoň 8 znaků.</span><span class="sxs-lookup"><span data-stu-id="63a01-133">The password must be at least 8 characters long.</span></span> <span data-ttu-id="63a01-134">Musí obsahovat 3 z následujících 4: velká písmena, malá písmena, číselné a speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="63a01-134">It must contain 3 of 4 of the following: uppercase, lowercase, numeric, and special characters.</span></span>
   
    <span data-ttu-id="63a01-135">Všimněte si, že vaše heslo nemůže být stejný jako poslední 24 hesla.</span><span class="sxs-lookup"><span data-stu-id="63a01-135">Note that your password cannot be the same as the last 24 passwords.</span></span>
4. <span data-ttu-id="63a01-136">Zadejte znovu heslo k potvrzení této akce.</span><span class="sxs-lookup"><span data-stu-id="63a01-136">Reenter the password to confirm it.</span></span>
   
    ![změnit Heslo2](./media/storsimple-virtual-array-change-device-admin-password/image41.png)
5. <span data-ttu-id="63a01-138">V dolní části stránky klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="63a01-138">At the bottom of the page, click **Apply**.</span></span> <span data-ttu-id="63a01-139">Nové heslo se teď použijí.</span><span class="sxs-lookup"><span data-stu-id="63a01-139">The new password is now applied.</span></span> <span data-ttu-id="63a01-140">Pokud změna hesla se nepovedlo úspěšně dokončit, zobrazí se následující chyba:</span><span class="sxs-lookup"><span data-stu-id="63a01-140">If the password change is not successful, you see the following error:</span></span>
   
    ![Chyba hesla](./media/storsimple-virtual-array-change-device-admin-password/image42.png)
   
    <span data-ttu-id="63a01-142">Po úspěšné aktualizaci heslo, budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="63a01-142">After the password is successfully updated, you are notified.</span></span> <span data-ttu-id="63a01-143">Pak můžete toto upravené heslo pro přístup k zařízení místně.</span><span class="sxs-lookup"><span data-stu-id="63a01-143">You can then use this modified password to access the device locally.</span></span>


## <a name="next-steps"></a><span data-ttu-id="63a01-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="63a01-144">Next steps</span></span>
<span data-ttu-id="63a01-145">Zjistěte, jak [spravovat vaše pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="63a01-145">Learn how to [administer your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

