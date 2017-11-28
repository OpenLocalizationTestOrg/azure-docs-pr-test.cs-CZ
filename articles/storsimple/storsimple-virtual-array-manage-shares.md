---
title: "sdílí aaaManage pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje hello Správce zařízení StorSimple a vysvětluje, jak toouse ho toomanage sdílených složek na pole virtuální zařízení StorSimple."
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: 0a799c83-fde5-4f3f-af0e-67535d1882b6
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 9b57d7ec7c0b7de5a22e1b816daa8852d0f32a48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-shares-on-hello-storsimple-virtual-array"></a><span data-ttu-id="6ca2f-103">Používat hello sdílené složky toomanage service Manager zařízení StorSimple na hello pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="6ca2f-103">Use hello StorSimple Device Manager service toomanage shares on hello StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="6ca2f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="6ca2f-104">Overview</span></span>

<span data-ttu-id="6ca2f-105">Tento kurz vysvětluje, jak toouse hello toocreate service Manager zařízení StorSimple a správu sdílených složek na pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-105">This tutorial explains how toouse hello StorSimple Device Manager service toocreate and manage shares on your StorSimple Virtual Array.</span></span>

<span data-ttu-id="6ca2f-106">Hello služby StorSimple Manager zařízení je rozšíření hello portálu Azure, která umožňuje spravovat z jedné webové rozhraní vašeho řešení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-106">hello StorSimple Device Manager service is an extension in hello Azure portal that lets you manage your StorSimple solution from a single web interface.</span></span> <span data-ttu-id="6ca2f-107">V přidání toomanaging složkami a svazky můžete použít tooview služby StorSimple Manager zařízení hello a spravovat zařízení, Zobrazit výstrahy, spravovat zásady zálohování a správa hello zálohování katalogu.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-107">In addition toomanaging shares and volumes, you can use hello StorSimple Device Manager service tooview and manage devices, view alerts, manage backup policies, and manage hello backup catalog.</span></span>

## <a name="share-types"></a><span data-ttu-id="6ca2f-108">Typy sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-108">Share Types</span></span>

<span data-ttu-id="6ca2f-109">Sdílené složky StorSimple může být:</span><span class="sxs-lookup"><span data-stu-id="6ca2f-109">StorSimple shares can be:</span></span>

* <span data-ttu-id="6ca2f-110">**Místně vázaný**: Data v těchto sdílených složek v poli hello zůstává po celou dobu a nebudou distribuována toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-110">**Locally pinned**: Data in these shares stays on hello array at all times and does not spill toohello cloud.</span></span>
* <span data-ttu-id="6ca2f-111">**Víceúrovňová**: Data v těchto sdílených složek můžete distribuována toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-111">**Tiered**: Data in these shares can spill toohello cloud.</span></span> <span data-ttu-id="6ca2f-112">Když vytvoříte sdílenou složku vrstvené, přibližně 10 % prostoru hello je zřízený na místní úrovni hello a 90 % prostoru hello se zřizuje v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-112">When you create a tiered share, approximately 10 % of hello space is provisioned on hello local tier and 90 % of hello space is provisioned in hello cloud.</span></span> <span data-ttu-id="6ca2f-113">Například pokud jste zřídili sdílenou složku 1 TB, 100 GB by nacházet v místním prostoru hello a 900 GB se použije v cloudu hello při hello datové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-113">For example, if you provisioned a 1 TB share, 100 GB would reside in hello local space and 900 GB would be used in hello cloud when hello data tiers.</span></span> <span data-ttu-id="6ca2f-114">Pak z toho vyplývá, že pokud spustíte mimo všechny hello volné místo na hello zařízení, nejde zřídit vrstvené sdílené složky (protože hello 10 % požadované na hello místní vrstvy nebudete mít k dispozici).</span><span class="sxs-lookup"><span data-stu-id="6ca2f-114">This in turn implies that if you run out of all hello local space on hello device, you cannot provision a tiered share (because hello 10 % required on hello local tier will not be available).</span></span>

### <a name="provisioned-capacity"></a><span data-ttu-id="6ca2f-115">Zřízená kapacita</span><span class="sxs-lookup"><span data-stu-id="6ca2f-115">Provisioned capacity</span></span>

<span data-ttu-id="6ca2f-116">Naleznete toohello následující tabulka pro maximální zřízená kapacita pro každý typ sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-116">Refer toohello following table for maximum provisioned capacity for each share type.</span></span>

| <span data-ttu-id="6ca2f-117">**Identifikátor limit**</span><span class="sxs-lookup"><span data-stu-id="6ca2f-117">**Limit identifier**</span></span> | <span data-ttu-id="6ca2f-118">**Limit**</span><span class="sxs-lookup"><span data-stu-id="6ca2f-118">**Limit**</span></span> |
| --- | --- |
| <span data-ttu-id="6ca2f-119">Minimální velikost vrstvené sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-119">Minimum size of a tiered share</span></span> |<span data-ttu-id="6ca2f-120">500 GB</span><span class="sxs-lookup"><span data-stu-id="6ca2f-120">500 GB</span></span> |
| <span data-ttu-id="6ca2f-121">Maximální velikost vrstvené sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-121">Maximum size of a tiered share</span></span> |<span data-ttu-id="6ca2f-122">20 TB</span><span class="sxs-lookup"><span data-stu-id="6ca2f-122">20 TB</span></span> |
| <span data-ttu-id="6ca2f-123">Minimální velikost místně vázaný sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-123">Minimum size of a locally pinned share</span></span> |<span data-ttu-id="6ca2f-124">50 GB</span><span class="sxs-lookup"><span data-stu-id="6ca2f-124">50 GB</span></span> |
| <span data-ttu-id="6ca2f-125">Maximální velikost místně vázaný sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-125">Maximum size of a locally pinned share</span></span> |<span data-ttu-id="6ca2f-126">2 TB</span><span class="sxs-lookup"><span data-stu-id="6ca2f-126">2 TB</span></span> |

## <a name="hello-shares-blade"></a><span data-ttu-id="6ca2f-127">okno Hello sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-127">hello Shares blade</span></span>

<span data-ttu-id="6ca2f-128">Hello **sdílené složky** nabídky na souhrnné okně vaší StorSimple služby zobrazí hello seznamu sdílených složek úložiště v daném poli StorSimple a toomanage, můžete je.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-128">hello **Shares** menu on your StorSimple service summary blade displays hello list of storage shares on a given StorSimple array and allows you toomanage them.</span></span>

![Okno sdílené složky](./media/storsimple-virtual-array-manage-shares/shares-blade.png)

<span data-ttu-id="6ca2f-130">Sdílenou složku se skládá z řady atributy:</span><span class="sxs-lookup"><span data-stu-id="6ca2f-130">A share consists of a series of attributes:</span></span>

* <span data-ttu-id="6ca2f-131">**Název sdílené složky** – popisný název, který musí být jedinečný a pomáhá identifikovat hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-131">**Share Name** – A descriptive name that must be unique and helps identify hello share.</span></span>
* <span data-ttu-id="6ca2f-132">**Stav** – může být online nebo offline.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-132">**Status** – Can be online or offline.</span></span> <span data-ttu-id="6ca2f-133">Pokud sdílenou složku offline, uživatelé hello sdílené složky nebudou moct tooaccess ho.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-133">If a share is offline, users of hello share will not be able tooaccess it.</span></span>
* <span data-ttu-id="6ca2f-134">**Typ** – Určuje, zda je sdílená složka hello **nastavování** (hello výchozí) nebo **místně vázaný**.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-134">**Type** – Indicates whether hello share is **Tiered** (hello default) or **Locally pinned**.</span></span>
* <span data-ttu-id="6ca2f-135">**Kapacita** – určuje hello množství dat použít jako porovnání toohello celkové množství dat, které můžou být uložené ve sdílené složce hello.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-135">**Capacity** – specifies hello amount of data used as compared toohello total amount of data that can be stored on hello share.</span></span>
* <span data-ttu-id="6ca2f-136">**Popis** – volitelné nastavení, které pomáhá popisují hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-136">**Description** – An optional setting that helps describe hello share.</span></span>
* <span data-ttu-id="6ca2f-137">**Oprávnění** -hello systému souborů NTFS oprávnění toohello složky, kterou je možné spravovat pomocí Průzkumníka Windows.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-137">**Permissions** - hello NTFS permissions toohello share that can be managed through Windows Explorer.</span></span>
* <span data-ttu-id="6ca2f-138">**Zálohování** – v případě, že z hello pole virtuální zařízení StorSimple, se automaticky povolí všechny sdílené složky pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-138">**Backup** – In case of hello StorSimple Virtual Array, all shares are automatically enabled for backup.</span></span>

![Podrobnosti o sdílených složek](./media/storsimple-virtual-array-manage-shares/share-details.png)

<span data-ttu-id="6ca2f-140">Používejte hello pokyny v této kurz tooperform hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="6ca2f-140">Use hello instructions in this tutorial tooperform hello following tasks:</span></span>

* <span data-ttu-id="6ca2f-141">Přidání sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-141">Add a share</span></span>
* <span data-ttu-id="6ca2f-142">Upravit sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-142">Modify a share</span></span>
* <span data-ttu-id="6ca2f-143">Sdílenou složku do offline</span><span class="sxs-lookup"><span data-stu-id="6ca2f-143">Take a share offline</span></span>
* <span data-ttu-id="6ca2f-144">Odstranění sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-144">Delete a share</span></span>

## <a name="add-a-share"></a><span data-ttu-id="6ca2f-145">Přidání sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-145">Add a share</span></span>

1. <span data-ttu-id="6ca2f-146">Z hello StorSimple služby souhrnu okna, klikněte na tlačítko **+ přidat sdílenou složku** z příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-146">From hello StorSimple service summary blade, click **+ Add share** from hello command bar.</span></span> <span data-ttu-id="6ca2f-147">Otevře se hello **přidat sdílenou složku** okno.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-147">This opens up hello **Add share** blade.</span></span>

    ![Přidejte sdílenou složku](./media/storsimple-virtual-array-manage-shares/add-share.png)

2. <span data-ttu-id="6ca2f-149">V hello **přidat sdílenou složku** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="6ca2f-149">In hello **Add share** blade, do hello following:</span></span>
   
    1. <span data-ttu-id="6ca2f-150">V hello **název sdílené složky** pole, zadejte jedinečný název pro vaši sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-150">In hello **Share name** field, enter a unique name for your share.</span></span> <span data-ttu-id="6ca2f-151">Název Hello musí být řetězec, který obsahuje 3 too127 znaky.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-151">hello name must be a string that contains 3 too127 characters.</span></span>

    2. <span data-ttu-id="6ca2f-152">Volitelný **popis** hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-152">An optional **Description** for hello share.</span></span> <span data-ttu-id="6ca2f-153">Popis Hello pomůže identifikovat hello vlastníkům sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-153">hello description will help identify hello share owners.</span></span>

    3. <span data-ttu-id="6ca2f-154">V hello **typ** rozevíracího seznamu, zadejte zda toocreate **nastavování** nebo **místně vázaný** sdílet.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-154">In hello **Type** dropdown list, specify whether toocreate a **Tiered** or **Locally pinned** share.</span></span> <span data-ttu-id="6ca2f-155">Pro úlohy, které vyžadují místní záruky, nízkou latenci a vyšší výkon, vyberte **místně vázaný sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-155">For workloads that require local guarantees, low latencies, and higher performance, select **Locally pinned share**.</span></span> <span data-ttu-id="6ca2f-156">Všechna ostatní data, vyberte **nastavování** sdílet.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-156">For all other data, select **Tiered** share.</span></span>

    4. <span data-ttu-id="6ca2f-157">V hello **kapacity** pole, zadejte velikost hello hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-157">In hello **Capacity** field, specify hello size of hello share.</span></span> <span data-ttu-id="6ca2f-158">Vrstvený sdílené složky musí být mezi 500 GB a 20 TB a místně vázaný sdílené složky musí být mezi 50 GB a 2 TB.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-158">A tiered share must be between 500 GB and 20 TB and a locally pinned share must be between 50 GB and 2 TB.</span></span>

    5. <span data-ttu-id="6ca2f-159">V hello **nastavit výchozí úplná oprávnění** pole, přiřadit hello oprávnění toohello uživatele nebo skupinu hello, který přistupuje k této sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-159">In hello **Set default full permissions to** field, assign hello permissions toohello user, or hello group that is accessing this share.</span></span> <span data-ttu-id="6ca2f-160">Zadejte název hello hello uživatele nebo skupiny uživatelů hello v  _john@contoso.com_  formátu.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-160">Specify hello name of hello user or hello user group in _john@contoso.com_ format.</span></span> <span data-ttu-id="6ca2f-161">Doporučujeme používat tooaccess oprávnění správce uživatele skupiny (ne jenom jednoho konkrétního uživatele) tooallow tyto sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-161">We recommend that you use a user group (instead of a single user) tooallow admin privileges tooaccess these shares.</span></span> <span data-ttu-id="6ca2f-162">Po přiřazení oprávnění hello sem, pak můžete Průzkumníka souborů toomodify tato oprávnění.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-162">After you have assigned hello permissions here, you can then use File Explorer toomodify these permissions.</span></span>
3. <span data-ttu-id="6ca2f-163">Po dokončení konfigurace vaší sdílené složky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-163">When you've finished configuring your share, click **Create**.</span></span> <span data-ttu-id="6ca2f-164">Vytvoří se sdílenou složku s hello zadaná nastavení a můžete se zobrazí oznámení.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-164">A share will be created with hello specified settings and you will see a notification.</span></span> <span data-ttu-id="6ca2f-165">Ve výchozím nastavení zálohování budou povolené pro sdílenou složku hello.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-165">By default, backup will be enabled for hello share.</span></span>
4. <span data-ttu-id="6ca2f-166">tooconfirm, který hello sdílená složka byla úspěšně vytvořil, přejděte toohello **sdílené složky** okno.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-166">tooconfirm that hello share was successfully created, go toohello **Shares** blade.</span></span> <span data-ttu-id="6ca2f-167">Měli byste vidět hello sdílené složky uvedené.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-167">You should see hello share listed.</span></span>
   
    ![Úspěšné vytvoření sdílené složky](./media/storsimple-virtual-array-manage-shares/share-success.png)

## <a name="modify-a-share"></a><span data-ttu-id="6ca2f-169">Upravit sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-169">Modify a share</span></span>

<span data-ttu-id="6ca2f-170">Upravte sdílenou složku, pokud budete potřebovat toochange hello popis hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-170">Modify a share when you need toochange hello description of hello share.</span></span> <span data-ttu-id="6ca2f-171">Žádné jiné vlastnosti sdílené složky můžete změnit po vytvoření sdílené složky hello.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-171">No other share properties can be modified once hello share is created.</span></span>

#### <a name="toomodify-a-share"></a><span data-ttu-id="6ca2f-172">toomodify sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-172">toomodify a share</span></span>

1. <span data-ttu-id="6ca2f-173">Z hello **sdílené složky** nastavit hello StorSimple služby souhrnu okna, vyberte hello virtuální pole, na které hello sdílené složky, které chcete toomodify nachází.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-173">From hello **Shares** setting on hello StorSimple service summary blade, select hello virtual array on which hello share you wish you toomodify resides.</span></span>
2. <span data-ttu-id="6ca2f-174">**Vyberte** hello sdílenou složku tooview hello aktuální popis a upravit ho.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-174">**Select** hello share tooview hello current description and modify it.</span></span>
3. <span data-ttu-id="6ca2f-175">Uložte změny kliknutím hello **Uložit** panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-175">Save your changes by clicking hello **Save** command bar.</span></span> <span data-ttu-id="6ca2f-176">Zadané nastavení uplatní a zobrazí se oznámení.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-176">Your specified settings will be applied and you will see a notification.</span></span>
   
    ![ <span data-ttu-id="6ca2f-177">Upravit sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-177">Edit share</span></span>](./media/storsimple-virtual-array-manage-shares/share-edit.png)

## <a name="take-a-share-offline"></a><span data-ttu-id="6ca2f-178">Sdílenou složku do offline</span><span class="sxs-lookup"><span data-stu-id="6ca2f-178">Take a share offline</span></span>

<span data-ttu-id="6ca2f-179">Můžete potřebovat tootake sdílenou složku do offline režimu při plánování toomodify ho nebo odstraňte ji.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-179">You may need tootake a share offline when you are planning toomodify it or delete it.</span></span> <span data-ttu-id="6ca2f-180">Když sdílenou složku do režimu offline, není k dispozici pro přístup pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-180">When a share is offline, it is not available for read-write access.</span></span> <span data-ttu-id="6ca2f-181">Budete potřebovat tootake hello sdílenou složku do offline režimu na hostiteli hello i hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-181">You will need tootake hello share offline on hello host as well as on hello device.</span></span>

#### <a name="tootake-a-share-offline"></a><span data-ttu-id="6ca2f-182">tootake sdílenou složku do offline režimu</span><span class="sxs-lookup"><span data-stu-id="6ca2f-182">tootake a share offline</span></span>

1. <span data-ttu-id="6ca2f-183">Zajistěte, aby že se v této sdílené složce hello nepoužívá před přepnutím do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-183">Make sure that hello share in question is not in use before taking it offline.</span></span>
2. <span data-ttu-id="6ca2f-184">Provedením následujících kroků hello trvat hello sdílené složky v poli hello offline:</span><span class="sxs-lookup"><span data-stu-id="6ca2f-184">Take hello share on hello array offline by performing hello following steps:</span></span>
   
    1. <span data-ttu-id="6ca2f-185">Z hello **sdílené složky** nastavit hello StorSimple služby souhrnu okna, vyberte hello virtuální pole, na které hello se nachází sdílená složka je chcete tootake do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-185">From hello **Shares** setting on hello StorSimple service summary blade, select hello virtual array on which hello share you wish you tootake offline resides.</span></span>

    2. <span data-ttu-id="6ca2f-186">**Vyberte** hello sdílené složky a klikněte na tlačítko **...**  (případně klikněte pravým tlačítkem na tento řádek) a z hello kontextové nabídky, vyberte **převést do režimu offline**.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-186">**Select** hello share and Click **...** (alternately right-click in this row) and from hello context menu, select **Take offline**.</span></span>
     
        ![Offline sdílené složky](./media/storsimple-virtual-array-manage-shares/shares-offline.png)

    3. <span data-ttu-id="6ca2f-188">Zkontrolujte informace hello v hello **převést do režimu offline** okno a potvrďte, že přijímáte hello operace.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-188">Review hello information in hello **Take offline** blade and confirm your acceptance of hello operation.</span></span> <span data-ttu-id="6ca2f-189">Klikněte na tlačítko **převést do režimu offline** sdílené složky hello tootake do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-189">Click **Take offline** tootake hello share offline.</span></span> <span data-ttu-id="6ca2f-190">Zobrazí se oznámení hello operace probíhá.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-190">You will see a notification of hello operation in progress.</span></span>

    4. <span data-ttu-id="6ca2f-191">tooconfirm, který hello sdílené složky byla úspěšně provedena toohello offline, přejděte **sdílené složky** okno.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-191">tooconfirm that hello share was successfully taken offline, go toohello **Shares** blade.</span></span> <span data-ttu-id="6ca2f-192">Měli byste vidět stav hello sdílené složky hello jako v režimu offline.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-192">You should see hello status of hello share as offline.</span></span>

## <a name="delete-a-share"></a><span data-ttu-id="6ca2f-193">Odstranění sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-193">Delete a share</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6ca2f-194">Sdílené složky lze odstranit pouze v případě, že je offline.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-194">You can delete a share only if it is offline.</span></span>


<span data-ttu-id="6ca2f-195">Proveďte následující kroky toodelete sdílenou složku hello.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-195">Complete hello following steps toodelete a share.</span></span>

#### <a name="toodelete-a-share"></a><span data-ttu-id="6ca2f-196">toodelete sdílené složky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-196">toodelete a share</span></span>

1. <span data-ttu-id="6ca2f-197">Z hello **sdílené složky** nastavit hello StorSimple služby souhrnu okna, vyberte hello virtuální pole, ve které složce hello chcete toodelete nachází.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-197">From hello **Shares** setting on hello StorSimple service summary blade, select hello virtual array on which hello share you wish toodelete resides.</span></span>
2. <span data-ttu-id="6ca2f-198">**Vyberte** hello sdílené složky a klikněte na tlačítko **...**  (případně klikněte pravým tlačítkem na tento řádek) a z hello kontextové nabídky, vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-198">**Select** hello share and Click **...** (alternately right-click in this row) and from hello context menu, select **Delete**.</span></span>
   
    ![Odstranit sdílenou složku](./media/storsimple-virtual-array-manage-shares/share-delete.png)
3. <span data-ttu-id="6ca2f-200">Zkontrolujte stav hello hello sdílenou složku, kterou chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-200">Check hello status of hello share you want toodelete.</span></span> <span data-ttu-id="6ca2f-201">Pokud není sdílená složka hello chcete toodelete offline, odpojte jej nejprve.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-201">If hello share you want toodelete is not offline, take it offline first.</span></span> <span data-ttu-id="6ca2f-202">Postupujte podle kroků hello v [do offline režimu sdílenou složku](#take-a-share-offline).</span><span class="sxs-lookup"><span data-stu-id="6ca2f-202">Follow hello steps in [Take a share offline](#take-a-share-offline).</span></span>
4. <span data-ttu-id="6ca2f-203">Po zobrazení výzvy k potvrzení v hello **odstranit** přijměte potvrzení hello a klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-203">When prompted for confirmation in hello **Delete** blade, accept hello confirmation and click **Delete**.</span></span> <span data-ttu-id="6ca2f-204">sdílené složky Hello bude odstraněno a hello **sdílené složky** okno zobrazuje hello aktualizovat seznam sdílených složek v rámci pole virtuálním hello.</span><span class="sxs-lookup"><span data-stu-id="6ca2f-204">hello share will now be deleted and hello **Shares** blade shows hello updated list of shares within hello virtual array.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ca2f-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6ca2f-205">Next steps</span></span>
<span data-ttu-id="6ca2f-206">Zjistěte, jak příliš[klonovat sdílenou složku StorSimple](storsimple-virtual-array-clone.md).</span><span class="sxs-lookup"><span data-stu-id="6ca2f-206">Learn how too[clone a StorSimple share](storsimple-virtual-array-clone.md).</span></span>

