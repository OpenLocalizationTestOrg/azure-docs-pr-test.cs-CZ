---
title: "Spravovat sdílené složky pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje Správce zařízení StorSimple a vysvětluje, jak použít ho ke správě sdílených složek na pole virtuální zařízení StorSimple."
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
ms.openlocfilehash: e5c62689de36baa175001f5f4f70d87568876ef0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-shares-on-the-storsimple-virtual-array"></a><span data-ttu-id="91743-103">Použít službu StorSimple Manager zařízení ke správě sdílených složek na poli virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="91743-103">Use the StorSimple Device Manager service to manage shares on the StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="91743-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="91743-104">Overview</span></span>

<span data-ttu-id="91743-105">Tento kurz vysvětluje, jak používat službu StorSimple Manager zařízení k vytváření a správě sdílených složek na pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="91743-105">This tutorial explains how to use the StorSimple Device Manager service to create and manage shares on your StorSimple Virtual Array.</span></span>

<span data-ttu-id="91743-106">Služba Správce zařízení StorSimple je rozšíření na portálu Azure, která umožňuje spravovat z jedné webové rozhraní vašeho řešení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="91743-106">The StorSimple Device Manager service is an extension in the Azure portal that lets you manage your StorSimple solution from a single web interface.</span></span> <span data-ttu-id="91743-107">Kromě správy sdílené složky a svazky, můžete použít službu StorSimple Manager zařízení zobrazit a spravovat zařízení, zobrazení výstrah, spravovat zásady zálohování a Správa katalogu zálohování.</span><span class="sxs-lookup"><span data-stu-id="91743-107">In addition to managing shares and volumes, you can use the StorSimple Device Manager service to view and manage devices, view alerts, manage backup policies, and manage the backup catalog.</span></span>

## <a name="share-types"></a><span data-ttu-id="91743-108">Typy sdílené složky</span><span class="sxs-lookup"><span data-stu-id="91743-108">Share Types</span></span>

<span data-ttu-id="91743-109">Sdílené složky StorSimple může být:</span><span class="sxs-lookup"><span data-stu-id="91743-109">StorSimple shares can be:</span></span>

* <span data-ttu-id="91743-110">**Místně vázaný**: Data v těchto sdílených složek zůstává v poli za všech okolností a nebudou distribuována do cloudu.</span><span class="sxs-lookup"><span data-stu-id="91743-110">**Locally pinned**: Data in these shares stays on the array at all times and does not spill to the cloud.</span></span>
* <span data-ttu-id="91743-111">**Víceúrovňová**: Data v těchto sdílených složek můžete distribuována do cloudu.</span><span class="sxs-lookup"><span data-stu-id="91743-111">**Tiered**: Data in these shares can spill to the cloud.</span></span> <span data-ttu-id="91743-112">Když vytvoříte sdílenou složku vrstvené, přibližně 10 % prostoru je zřízený na místní úrovni a 90 % prostoru se zřizuje v cloudu.</span><span class="sxs-lookup"><span data-stu-id="91743-112">When you create a tiered share, approximately 10 % of the space is provisioned on the local tier and 90 % of the space is provisioned in the cloud.</span></span> <span data-ttu-id="91743-113">Například pokud jste zřídili sdílenou složku 1 TB, 100 GB by nacházet v místním prostoru a 900 GB se použije v cloudu při datové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="91743-113">For example, if you provisioned a 1 TB share, 100 GB would reside in the local space and 900 GB would be used in the cloud when the data tiers.</span></span> <span data-ttu-id="91743-114">Pak z toho vyplývá, že pokud spustíte mimo veškeré volné místo na zařízení, nejde zřídit vrstvené sdílené složky (protože 10 % požadované na místní úrovni nebudete mít k dispozici).</span><span class="sxs-lookup"><span data-stu-id="91743-114">This in turn implies that if you run out of all the local space on the device, you cannot provision a tiered share (because the 10 % required on the local tier will not be available).</span></span>

### <a name="provisioned-capacity"></a><span data-ttu-id="91743-115">Zřízená kapacita</span><span class="sxs-lookup"><span data-stu-id="91743-115">Provisioned capacity</span></span>

<span data-ttu-id="91743-116">Naleznete v následující tabulce maximální zřízená kapacita pro každý typ sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="91743-116">Refer to the following table for maximum provisioned capacity for each share type.</span></span>

| <span data-ttu-id="91743-117">**Identifikátor limit**</span><span class="sxs-lookup"><span data-stu-id="91743-117">**Limit identifier**</span></span> | <span data-ttu-id="91743-118">**Limit**</span><span class="sxs-lookup"><span data-stu-id="91743-118">**Limit**</span></span> |
| --- | --- |
| <span data-ttu-id="91743-119">Minimální velikost vrstvené sdílené složky</span><span class="sxs-lookup"><span data-stu-id="91743-119">Minimum size of a tiered share</span></span> |<span data-ttu-id="91743-120">500 GB</span><span class="sxs-lookup"><span data-stu-id="91743-120">500 GB</span></span> |
| <span data-ttu-id="91743-121">Maximální velikost vrstvené sdílené složky</span><span class="sxs-lookup"><span data-stu-id="91743-121">Maximum size of a tiered share</span></span> |<span data-ttu-id="91743-122">20 TB</span><span class="sxs-lookup"><span data-stu-id="91743-122">20 TB</span></span> |
| <span data-ttu-id="91743-123">Minimální velikost místně vázaný sdílené složky</span><span class="sxs-lookup"><span data-stu-id="91743-123">Minimum size of a locally pinned share</span></span> |<span data-ttu-id="91743-124">50 GB</span><span class="sxs-lookup"><span data-stu-id="91743-124">50 GB</span></span> |
| <span data-ttu-id="91743-125">Maximální velikost místně vázaný sdílené složky</span><span class="sxs-lookup"><span data-stu-id="91743-125">Maximum size of a locally pinned share</span></span> |<span data-ttu-id="91743-126">2 TB</span><span class="sxs-lookup"><span data-stu-id="91743-126">2 TB</span></span> |

## <a name="the-shares-blade"></a><span data-ttu-id="91743-127">V okně sdílené složky</span><span class="sxs-lookup"><span data-stu-id="91743-127">The Shares blade</span></span>

<span data-ttu-id="91743-128">**Sdílené složky** nabídky na souhrnné okně vaší StorSimple služby zobrazí seznamu sdílených složek úložiště v daném poli StorSimple a umožňuje je spravovat.</span><span class="sxs-lookup"><span data-stu-id="91743-128">The **Shares** menu on your StorSimple service summary blade displays the list of storage shares on a given StorSimple array and allows you to manage them.</span></span>

![Okno sdílené složky](./media/storsimple-virtual-array-manage-shares/shares-blade.png)

<span data-ttu-id="91743-130">Sdílenou složku se skládá z řady atributy:</span><span class="sxs-lookup"><span data-stu-id="91743-130">A share consists of a series of attributes:</span></span>

* <span data-ttu-id="91743-131">**Název sdílené složky** – popisný název, který musí být jedinečný a pomáhá identifikovat sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="91743-131">**Share Name** – A descriptive name that must be unique and helps identify the share.</span></span>
* <span data-ttu-id="91743-132">**Stav** – může být online nebo offline.</span><span class="sxs-lookup"><span data-stu-id="91743-132">**Status** – Can be online or offline.</span></span> <span data-ttu-id="91743-133">Pokud sdílenou složku offline, uživatelé sdílené složky nebudou moci k němu přístup.</span><span class="sxs-lookup"><span data-stu-id="91743-133">If a share is offline, users of the share will not be able to access it.</span></span>
* <span data-ttu-id="91743-134">**Typ** – Určuje, zda sdílená složka je **nastavování** (výchozí) nebo **místně vázaný**.</span><span class="sxs-lookup"><span data-stu-id="91743-134">**Type** – Indicates whether the share is **Tiered** (the default) or **Locally pinned**.</span></span>
* <span data-ttu-id="91743-135">**Kapacita** – Určuje velikost dat používaných ve srovnání se celkové množství dat, které můžou být uložené ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="91743-135">**Capacity** – specifies the amount of data used as compared to the total amount of data that can be stored on the share.</span></span>
* <span data-ttu-id="91743-136">**Popis** – volitelné nastavení, která pomáhá popisují sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="91743-136">**Description** – An optional setting that helps describe the share.</span></span>
* <span data-ttu-id="91743-137">**Oprávnění** – systému souborů NTFS oprávnění ke sdílené složce, kterou je možné spravovat pomocí Průzkumníka Windows.</span><span class="sxs-lookup"><span data-stu-id="91743-137">**Permissions** - The NTFS permissions to the share that can be managed through Windows Explorer.</span></span>
* <span data-ttu-id="91743-138">**Zálohování** – v případě, že pole virtuální zařízení StorSimple se automaticky povolí všechny sdílené složky pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="91743-138">**Backup** – In case of the StorSimple Virtual Array, all shares are automatically enabled for backup.</span></span>

![Podrobnosti o sdílených složek](./media/storsimple-virtual-array-manage-shares/share-details.png)

<span data-ttu-id="91743-140">Postupujte podle pokynů v tomto kurzu k provádění následujících úloh:</span><span class="sxs-lookup"><span data-stu-id="91743-140">Use the instructions in this tutorial to perform the following tasks:</span></span>

* <span data-ttu-id="91743-141">Přidání sdílené složky</span><span class="sxs-lookup"><span data-stu-id="91743-141">Add a share</span></span>
* <span data-ttu-id="91743-142">Upravit sdílené složky</span><span class="sxs-lookup"><span data-stu-id="91743-142">Modify a share</span></span>
* <span data-ttu-id="91743-143">Sdílenou složku do offline</span><span class="sxs-lookup"><span data-stu-id="91743-143">Take a share offline</span></span>
* <span data-ttu-id="91743-144">Odstranění sdílené složky</span><span class="sxs-lookup"><span data-stu-id="91743-144">Delete a share</span></span>

## <a name="add-a-share"></a><span data-ttu-id="91743-145">Přidání sdílené složky</span><span class="sxs-lookup"><span data-stu-id="91743-145">Add a share</span></span>

1. <span data-ttu-id="91743-146">Ze služby StorSimple souhrnu okna, klikněte na tlačítko **+ přidat sdílenou složku** z řádku nabídek.</span><span class="sxs-lookup"><span data-stu-id="91743-146">From the StorSimple service summary blade, click **+ Add share** from the command bar.</span></span> <span data-ttu-id="91743-147">Otevře **přidat sdílenou složku** okno.</span><span class="sxs-lookup"><span data-stu-id="91743-147">This opens up the **Add share** blade.</span></span>

    ![Přidejte sdílenou složku](./media/storsimple-virtual-array-manage-shares/add-share.png)

2. <span data-ttu-id="91743-149">V **přidat sdílenou složku** okno, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="91743-149">In the **Add share** blade, do the following:</span></span>
   
    1. <span data-ttu-id="91743-150">V **název sdílené složky** pole, zadejte jedinečný název pro vaši sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="91743-150">In the **Share name** field, enter a unique name for your share.</span></span> <span data-ttu-id="91743-151">Název musí být řetězec, který obsahuje 3 až 127 znaků.</span><span class="sxs-lookup"><span data-stu-id="91743-151">The name must be a string that contains 3 to 127 characters.</span></span>

    2. <span data-ttu-id="91743-152">Volitelný **popis** pro sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="91743-152">An optional **Description** for the share.</span></span> <span data-ttu-id="91743-153">Popis pomůže identifikovat vlastníkům sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="91743-153">The description will help identify the share owners.</span></span>

    3. <span data-ttu-id="91743-154">V **typ** rozevíracího seznamu, určete, zda chcete vytvořit **nastavování** nebo **místně vázaný** sdílet.</span><span class="sxs-lookup"><span data-stu-id="91743-154">In the **Type** dropdown list, specify whether to create a **Tiered** or **Locally pinned** share.</span></span> <span data-ttu-id="91743-155">Pro úlohy, které vyžadují místní záruky, nízkou latenci a vyšší výkon, vyberte **místně vázaný sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="91743-155">For workloads that require local guarantees, low latencies, and higher performance, select **Locally pinned share**.</span></span> <span data-ttu-id="91743-156">Všechna ostatní data, vyberte **nastavování** sdílet.</span><span class="sxs-lookup"><span data-stu-id="91743-156">For all other data, select **Tiered** share.</span></span>

    4. <span data-ttu-id="91743-157">V **kapacity** pole, zadejte velikost sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="91743-157">In the **Capacity** field, specify the size of the share.</span></span> <span data-ttu-id="91743-158">Vrstvený sdílené složky musí být mezi 500 GB a 20 TB a místně vázaný sdílené složky musí být mezi 50 GB a 2 TB.</span><span class="sxs-lookup"><span data-stu-id="91743-158">A tiered share must be between 500 GB and 20 TB and a locally pinned share must be between 50 GB and 2 TB.</span></span>

    5. <span data-ttu-id="91743-159">V **nastavit výchozí úplná oprávnění** pole, přiřadit oprávnění uživatele nebo skupiny, který přistupuje k této sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="91743-159">In the **Set default full permissions to** field, assign the permissions to the user, or the group that is accessing this share.</span></span> <span data-ttu-id="91743-160">Zadejte název tohoto uživatele nebo skupiny uživatelů v  _john@contoso.com_  formátu.</span><span class="sxs-lookup"><span data-stu-id="91743-160">Specify the name of the user or the user group in _john@contoso.com_ format.</span></span> <span data-ttu-id="91743-161">Doporučujeme vám, že používáte skupinu uživatelů (ne jenom jednoho konkrétního uživatele) umožňuje správu oprávnění k přístupu k tyto sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="91743-161">We recommend that you use a user group (instead of a single user) to allow admin privileges to access these shares.</span></span> <span data-ttu-id="91743-162">Po přiřazení oprávnění tady, můžete pomocí Průzkumníka souborů upravit tato oprávnění.</span><span class="sxs-lookup"><span data-stu-id="91743-162">After you have assigned the permissions here, you can then use File Explorer to modify these permissions.</span></span>
3. <span data-ttu-id="91743-163">Po dokončení konfigurace vaší sdílené složky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="91743-163">When you've finished configuring your share, click **Create**.</span></span> <span data-ttu-id="91743-164">Vytvoří sdílené složky se zadaným nastavením a zobrazí se oznámení.</span><span class="sxs-lookup"><span data-stu-id="91743-164">A share will be created with the specified settings and you will see a notification.</span></span> <span data-ttu-id="91743-165">Ve výchozím nastavení zálohování budou povolené pro sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="91743-165">By default, backup will be enabled for the share.</span></span>
4. <span data-ttu-id="91743-166">Chcete-li potvrdit, že byl úspěšně vytvořen sdílenou složku, přejděte na **sdílené složky** okno.</span><span class="sxs-lookup"><span data-stu-id="91743-166">To confirm that the share was successfully created, go to the **Shares** blade.</span></span> <span data-ttu-id="91743-167">Měli byste vidět sdílené složky uvedené.</span><span class="sxs-lookup"><span data-stu-id="91743-167">You should see the share listed.</span></span>
   
    ![Úspěšné vytvoření sdílené složky](./media/storsimple-virtual-array-manage-shares/share-success.png)

## <a name="modify-a-share"></a><span data-ttu-id="91743-169">Upravit sdílené složky</span><span class="sxs-lookup"><span data-stu-id="91743-169">Modify a share</span></span>

<span data-ttu-id="91743-170">Upravte sdílenou složku, pokud potřebujete změnit popis sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="91743-170">Modify a share when you need to change the description of the share.</span></span> <span data-ttu-id="91743-171">Žádné jiné vlastnosti sdílené složky můžete změnit po vytvoření sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="91743-171">No other share properties can be modified once the share is created.</span></span>

#### <a name="to-modify-a-share"></a><span data-ttu-id="91743-172">Chcete-li upravit sdílené složky</span><span class="sxs-lookup"><span data-stu-id="91743-172">To modify a share</span></span>

1. <span data-ttu-id="91743-173">Z **sdílené složky** nastavení v okně Souhrn služby StorSimple, vyberte virtuální pole, v němž jsou uložena vám upravit chcete sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="91743-173">From the **Shares** setting on the StorSimple service summary blade, select the virtual array on which the share you wish you to modify resides.</span></span>
2. <span data-ttu-id="91743-174">**Vyberte** sdílenou složku na aktuální popis zobrazit a upravit ho.</span><span class="sxs-lookup"><span data-stu-id="91743-174">**Select** the share to view the current description and modify it.</span></span>
3. <span data-ttu-id="91743-175">Uložte změny kliknutím **Uložit** panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="91743-175">Save your changes by clicking the **Save** command bar.</span></span> <span data-ttu-id="91743-176">Zadané nastavení uplatní a zobrazí se oznámení.</span><span class="sxs-lookup"><span data-stu-id="91743-176">Your specified settings will be applied and you will see a notification.</span></span>
   
    ![ <span data-ttu-id="91743-177">Upravit sdílené složky</span><span class="sxs-lookup"><span data-stu-id="91743-177">Edit share</span></span>](./media/storsimple-virtual-array-manage-shares/share-edit.png)

## <a name="take-a-share-offline"></a><span data-ttu-id="91743-178">Sdílenou složku do offline</span><span class="sxs-lookup"><span data-stu-id="91743-178">Take a share offline</span></span>

<span data-ttu-id="91743-179">Musíte uvedení sdílenou složku do režimu offline, když máte v úmyslu upravit nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="91743-179">You may need to take a share offline when you are planning to modify it or delete it.</span></span> <span data-ttu-id="91743-180">Když sdílenou složku do režimu offline, není k dispozici pro přístup pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="91743-180">When a share is offline, it is not available for read-write access.</span></span> <span data-ttu-id="91743-181">Musíte provést offline sdílenou složku na hostiteli, a také na zařízení.</span><span class="sxs-lookup"><span data-stu-id="91743-181">You will need to take the share offline on the host as well as on the device.</span></span>

#### <a name="to-take-a-share-offline"></a><span data-ttu-id="91743-182">Uvedení sdílenou složku do režimu offline</span><span class="sxs-lookup"><span data-stu-id="91743-182">To take a share offline</span></span>

1. <span data-ttu-id="91743-183">Ujistěte se, že není sdílená složka dotyčném používá před přepnutím do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="91743-183">Make sure that the share in question is not in use before taking it offline.</span></span>
2. <span data-ttu-id="91743-184">Využít sdílené složky v poli do offline režimu provedením následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="91743-184">Take the share on the array offline by performing the following steps:</span></span>
   
    1. <span data-ttu-id="91743-185">Z **sdílené složky** nastavení v okně Souhrn služby StorSimple, vyberte pole virtuální, na kterém se nachází sdílená složka chcete přepnout do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="91743-185">From the **Shares** setting on the StorSimple service summary blade, select the virtual array on which the share you wish you to take offline resides.</span></span>

    2. <span data-ttu-id="91743-186">**Vyberte** do sdílené složky a klikněte na tlačítko **...**  (případně klikněte pravým tlačítkem na tento řádek) a v místní nabídce vyberte **převést do režimu offline**.</span><span class="sxs-lookup"><span data-stu-id="91743-186">**Select** the share and Click **...** (alternately right-click in this row) and from the context menu, select **Take offline**.</span></span>
     
        ![Offline sdílené složky](./media/storsimple-virtual-array-manage-shares/shares-offline.png)

    3. <span data-ttu-id="91743-188">Informace v **převést do režimu offline** okno a potvrďte, že přijímáte operace.</span><span class="sxs-lookup"><span data-stu-id="91743-188">Review the information in the **Take offline** blade and confirm your acceptance of the operation.</span></span> <span data-ttu-id="91743-189">Klikněte na tlačítko **převést do režimu offline** uvedení sdílenou složku do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="91743-189">Click **Take offline** to take the share offline.</span></span> <span data-ttu-id="91743-190">Zobrazí se oznámení o probíhající operaci.</span><span class="sxs-lookup"><span data-stu-id="91743-190">You will see a notification of the operation in progress.</span></span>

    4. <span data-ttu-id="91743-191">Chcete-li potvrdit, že sdílené složky byla úspěšně převedeno do režimu offline, přejděte na **sdílené složky** okno.</span><span class="sxs-lookup"><span data-stu-id="91743-191">To confirm that the share was successfully taken offline, go to the **Shares** blade.</span></span> <span data-ttu-id="91743-192">Měli byste vidět stav sdílené složky jako offline.</span><span class="sxs-lookup"><span data-stu-id="91743-192">You should see the status of the share as offline.</span></span>

## <a name="delete-a-share"></a><span data-ttu-id="91743-193">Odstranění sdílené složky</span><span class="sxs-lookup"><span data-stu-id="91743-193">Delete a share</span></span>

> [!IMPORTANT]
> <span data-ttu-id="91743-194">Sdílené složky lze odstranit pouze v případě, že je offline.</span><span class="sxs-lookup"><span data-stu-id="91743-194">You can delete a share only if it is offline.</span></span>


<span data-ttu-id="91743-195">Dokončete následující postup odstranění sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="91743-195">Complete the following steps to delete a share.</span></span>

#### <a name="to-delete-a-share"></a><span data-ttu-id="91743-196">Chcete-li odstranit sdílenou složku</span><span class="sxs-lookup"><span data-stu-id="91743-196">To delete a share</span></span>

1. <span data-ttu-id="91743-197">Z **sdílené složky** nastavení v okně Souhrn služby StorSimple, vyberte virtuální pole, v němž jsou uložena chcete odstranit sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="91743-197">From the **Shares** setting on the StorSimple service summary blade, select the virtual array on which the share you wish to delete resides.</span></span>
2. <span data-ttu-id="91743-198">**Vyberte** do sdílené složky a klikněte na tlačítko **...**  (případně klikněte pravým tlačítkem na tento řádek) a v místní nabídce vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="91743-198">**Select** the share and Click **...** (alternately right-click in this row) and from the context menu, select **Delete**.</span></span>
   
    ![Odstranit sdílenou složku](./media/storsimple-virtual-array-manage-shares/share-delete.png)
3. <span data-ttu-id="91743-200">Zkontrolujte stav sdílené složky, které chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="91743-200">Check the status of the share you want to delete.</span></span> <span data-ttu-id="91743-201">Pokud není sdílená složka, kterou chcete odstranit offline, odpojte jej nejprve.</span><span class="sxs-lookup"><span data-stu-id="91743-201">If the share you want to delete is not offline, take it offline first.</span></span> <span data-ttu-id="91743-202">Postupujte podle kroků v [do offline režimu sdílenou složku](#take-a-share-offline).</span><span class="sxs-lookup"><span data-stu-id="91743-202">Follow the steps in [Take a share offline](#take-a-share-offline).</span></span>
4. <span data-ttu-id="91743-203">Po zobrazení výzvy k potvrzení v **odstranit** okně přijměte potvrzení a klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="91743-203">When prompted for confirmation in the **Delete** blade, accept the confirmation and click **Delete**.</span></span> <span data-ttu-id="91743-204">Sdílenou složku bude odstraněno a **sdílené složky** aktualizovaný seznam sdílených složek v rámci virtuální pole se zobrazí okno.</span><span class="sxs-lookup"><span data-stu-id="91743-204">The share will now be deleted and the **Shares** blade shows the updated list of shares within the virtual array.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91743-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="91743-205">Next steps</span></span>
<span data-ttu-id="91743-206">Zjistěte, jak [klonovat sdílenou složku StorSimple](storsimple-virtual-array-clone.md).</span><span class="sxs-lookup"><span data-stu-id="91743-206">Learn how to [clone a StorSimple share](storsimple-virtual-array-clone.md).</span></span>

