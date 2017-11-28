---
title: "heslo správce zařízení aaaChange pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak toouse buď hello portálu Azure nebo hesla správce zařízení toochange pole virtuální zařízení StorSimple webového uživatelského rozhraní hello."
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
ms.openlocfilehash: 531b395df7aeade0a909360797c6b0f0abd9fd1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-storsimple-virtual-array-device-administrator-password-via-storsimple-device-manager"></a><span data-ttu-id="4d66d-103">Změna hesla správce zařízení StorSimple virtuální pole hello pomocí Správce zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="4d66d-103">Change hello StorSimple Virtual Array device administrator password via StorSimple Device Manager</span></span>

## <a name="overview"></a><span data-ttu-id="4d66d-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="4d66d-104">Overview</span></span>

<span data-ttu-id="4d66d-105">Při použití hello prostředí Windows PowerShell rozhraní tooaccess hello pole virtuální zařízení StorSimple, jsou požadované tooenter hesla správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="4d66d-105">When you use hello Windows PowerShell interface tooaccess hello StorSimple Virtual Array, you are required tooenter a device administrator password.</span></span> <span data-ttu-id="4d66d-106">Pokud zařízení StorSimple hello je nejdřív zřízený a spuštěna, hello výchozí heslo je *Heslo1*.</span><span class="sxs-lookup"><span data-stu-id="4d66d-106">When hello StorSimple device is first provisioned and started, hello default password is *Password1*.</span></span> <span data-ttu-id="4d66d-107">Hello zabezpečení vašich dat, hello výchozí heslo vyprší hello při prvním přihlášení a jsou požadované toochange toto heslo.</span><span class="sxs-lookup"><span data-stu-id="4d66d-107">For hello security of your data, hello default password expires hello first time that you sign in and you are required toochange this password.</span></span>

<span data-ttu-id="4d66d-108">Buď hello místního webového uživatelského rozhraní nebo hello Azure portálu toochange hello hesla správce zařízení můžete taky kdykoli po nasazení hello zařízení v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4d66d-108">You can also use either hello local web UI or hello Azure portal toochange hello device administrator password at any time after hello device is deployed in your production environment.</span></span> <span data-ttu-id="4d66d-109">Každá z těchto postupů je popsána v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="4d66d-109">Each of these procedures is described in this article.</span></span>

 ![Okno zařízení](./media/storsimple-virtual-array-change-device-admin-password/ova-devices-blade.png)

## <a name="use-hello-azure-portal-toochange-hello-password"></a><span data-ttu-id="4d66d-111">Použít heslo hello Azure portálu toochange hello</span><span class="sxs-lookup"><span data-stu-id="4d66d-111">Use hello Azure portal toochange hello password</span></span>

<span data-ttu-id="4d66d-112">Proveďte následující kroky správce heslo zařízení hello toochange prostřednictvím portálu Azure hello hello.</span><span class="sxs-lookup"><span data-stu-id="4d66d-112">Perform hello following steps toochange hello device administrator password through hello Azure portal.</span></span>

#### <a name="toochange-hello-device-administrator-password-via-hello-azure-portal"></a><span data-ttu-id="4d66d-113">heslo správce zařízení toochange hello prostřednictvím hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4d66d-113">toochange hello device administrator password via hello Azure portal</span></span>

1. <span data-ttu-id="4d66d-114">Na cílovou stránku hello služby, vyberte svoji službu, klikněte dvakrát na hello název služby a potom v rámci hello **správy** klikněte na tlačítko **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="4d66d-114">On hello service landing page, select your service, double-click hello service name, and then within hello **Management** section, click **Devices**.</span></span> <span data-ttu-id="4d66d-115">Tím se otevře hello **zařízení** okno, které obsahuje seznamy všech zařízení StorSimple virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="4d66d-115">This opens hello **Devices** blade that lists all your StorSimple Virtual Array devices.</span></span>

2. <span data-ttu-id="4d66d-116">V hello **zařízení** okno, klikněte dvakrát na hello zařízení, která vyžaduje změnu hesla.</span><span class="sxs-lookup"><span data-stu-id="4d66d-116">In hello **Devices** blade, double-click hello device that requires a change of password.</span></span>

3. <span data-ttu-id="4d66d-117">V hello **nastavení** pro zařízení, klikněte na **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="4d66d-117">In hello **Settings** blade for your device, click **Security**.</span></span>

4. <span data-ttu-id="4d66d-118">V hello **nastavení zabezpečení** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="4d66d-118">In hello **Security Settings** blade, do hello following:</span></span>
   
   1. <span data-ttu-id="4d66d-119">Projděte dolů toohello **heslo správce zařízení** části.</span><span class="sxs-lookup"><span data-stu-id="4d66d-119">Scroll down toohello **Device Administrator Password** section.</span></span> <span data-ttu-id="4d66d-120">Zadejte heslo správce, který obsahuje z 8 znaků too15.</span><span class="sxs-lookup"><span data-stu-id="4d66d-120">Provide an administrator password that contains from 8 too15 characters.</span></span>
   2. <span data-ttu-id="4d66d-121">Potvrzení hesla hello.</span><span class="sxs-lookup"><span data-stu-id="4d66d-121">Confirm hello password.</span></span>
   3. <span data-ttu-id="4d66d-122">Klikněte na tlačítko **Uložit** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="4d66d-122">Click **Save** at hello top of hello blade.</span></span>

<span data-ttu-id="4d66d-123">heslo správce zařízení Hello se nyní aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="4d66d-123">hello device administrator password is now updated.</span></span> <span data-ttu-id="4d66d-124">Toto zařízení hello tooaccess upravené heslo můžete místně.</span><span class="sxs-lookup"><span data-stu-id="4d66d-124">You can use this modified password tooaccess hello device locally.</span></span>

![Okno nastavení zabezpečení](./media/storsimple-virtual-array-change-device-admin-password/ova-change-device-pwd.png)

## <a name="use-hello-local-web-ui-toochange-hello-password"></a><span data-ttu-id="4d66d-126">Použijte hello místního webového uživatelského rozhraní toochange hello heslo</span><span class="sxs-lookup"><span data-stu-id="4d66d-126">Use hello local web UI toochange hello password</span></span>

<span data-ttu-id="4d66d-127">Proveďte následující kroky správce heslo zařízení hello toochange hello místního webového uživatelského rozhraní hello.</span><span class="sxs-lookup"><span data-stu-id="4d66d-127">Perform hello following steps toochange hello device administrator password through hello local web UI.</span></span>

#### <a name="toochange-hello-device-administrator-password-via-hello-local-web-ui"></a><span data-ttu-id="4d66d-128">heslo správce zařízení toochange hello prostřednictvím hello místního webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="4d66d-128">toochange hello device administrator password via hello local web UI</span></span>

1. <span data-ttu-id="4d66d-129">V hello místního webového uživatelského rozhraní, klikněte na **údržby** > **Změna hesla** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="4d66d-129">In hello local web UI, click **Maintenance** > **Password change** for your device.</span></span>
   
    ![změnit Heslo1](./media/storsimple-virtual-array-change-device-admin-password/image40.png)
2. <span data-ttu-id="4d66d-131">Zadejte hello **aktuální heslo**.</span><span class="sxs-lookup"><span data-stu-id="4d66d-131">Enter hello **Current password**.</span></span>
3. <span data-ttu-id="4d66d-132">Zadejte **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="4d66d-132">Provide a **New Password**.</span></span> <span data-ttu-id="4d66d-133">Hello heslo musí být dlouhé alespoň 8 znaků.</span><span class="sxs-lookup"><span data-stu-id="4d66d-133">hello password must be at least 8 characters long.</span></span> <span data-ttu-id="4d66d-134">Musí obsahovat 3 4 následující hello: velká písmena, malá písmena, číselné a speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="4d66d-134">It must contain 3 of 4 of hello following: uppercase, lowercase, numeric, and special characters.</span></span>
   
    <span data-ttu-id="4d66d-135">Všimněte si, že vaše heslo nemůže být hello stejná hodnota jako hello posledních 24 hesla.</span><span class="sxs-lookup"><span data-stu-id="4d66d-135">Note that your password cannot be hello same as hello last 24 passwords.</span></span>
4. <span data-ttu-id="4d66d-136">Zadejte znovu heslo tooconfirm hello ho.</span><span class="sxs-lookup"><span data-stu-id="4d66d-136">Reenter hello password tooconfirm it.</span></span>
   
    ![změnit Heslo2](./media/storsimple-virtual-array-change-device-admin-password/image41.png)
5. <span data-ttu-id="4d66d-138">V dolní části hello hello stránky, klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="4d66d-138">At hello bottom of hello page, click **Apply**.</span></span> <span data-ttu-id="4d66d-139">nové heslo Hello se teď použijí.</span><span class="sxs-lookup"><span data-stu-id="4d66d-139">hello new password is now applied.</span></span> <span data-ttu-id="4d66d-140">Změna hesla hello neproběhne úspěšně, zobrazí hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="4d66d-140">If hello password change is not successful, you see hello following error:</span></span>
   
    ![Chyba hesla](./media/storsimple-virtual-array-change-device-admin-password/image42.png)
   
    <span data-ttu-id="4d66d-142">Po úspěšné aktualizaci hello heslo, budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="4d66d-142">After hello password is successfully updated, you are notified.</span></span> <span data-ttu-id="4d66d-143">Pak můžete toto zařízení hello tooaccess upravené heslo místně.</span><span class="sxs-lookup"><span data-stu-id="4d66d-143">You can then use this modified password tooaccess hello device locally.</span></span>


## <a name="next-steps"></a><span data-ttu-id="4d66d-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4d66d-144">Next steps</span></span>
<span data-ttu-id="4d66d-145">Zjistěte, jak příliš[spravovat vaše pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="4d66d-145">Learn how too[administer your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

