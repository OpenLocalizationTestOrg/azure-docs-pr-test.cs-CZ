---
title: "aaaManage svazky na pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje hello Správce zařízení StorSimple a vysvětluje, jak toouse ho toomanage svazky na pole virtuální zařízení StorSimple."
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: caa6a26b-b7ba-4a05-b092-1a79450225cf
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 46aa6d7508b3e62f75a3b78ed73302b88320a0f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-service-toomanage-volumes-on-hello-storsimple-virtual-array"></a><span data-ttu-id="b5708-103">Pomocí Správce zařízení StorSimple služby toomanage svazky na hello pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="b5708-103">Use StorSimple Device Manager service toomanage volumes on hello StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="b5708-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b5708-104">Overview</span></span>

<span data-ttu-id="b5708-105">Tento kurz vysvětluje, jak toouse hello toocreate service Manager zařízení StorSimple a spravovat svazky na pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="b5708-105">This tutorial explains how toouse hello StorSimple Device Manager service toocreate and manage volumes on your StorSimple Virtual Array.</span></span>

<span data-ttu-id="b5708-106">Hello služby StorSimple Manager zařízení je rozšíření hello portálu Azure, která umožňuje spravovat z jedné webové rozhraní vašeho řešení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="b5708-106">hello StorSimple Device Manager service is an extension in hello Azure portal that lets you manage your StorSimple solution from a single web interface.</span></span> <span data-ttu-id="b5708-107">V přidání toomanaging složkami a svazky můžete použít tooview služby StorSimple Manager zařízení hello a spravovat zařízení, zobrazovat výstrahy a zobrazení a Správa zásady zálohování a zálohování katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="b5708-107">In addition toomanaging shares and volumes, you can use hello StorSimple Device Manager service tooview and manage devices, view alerts, and view and manage backup policies and hello backup catalog.</span></span>

## <a name="volume-types"></a><span data-ttu-id="b5708-108">Typy svazku</span><span class="sxs-lookup"><span data-stu-id="b5708-108">Volume Types</span></span>

<span data-ttu-id="b5708-109">Svazky zařízení StorSimple může být:</span><span class="sxs-lookup"><span data-stu-id="b5708-109">StorSimple volumes can be:</span></span>

* <span data-ttu-id="b5708-110">**Místně vázaný**: Data v těchto svazcích zůstává v poli hello za všech okolností a nebudou distribuována toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="b5708-110">**Locally pinned**: Data in these volumes stays on hello array at all times and does not spill toohello cloud.</span></span>
* <span data-ttu-id="b5708-111">**Víceúrovňová**: Data v těchto svazků můžete distribuována toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="b5708-111">**Tiered**: Data in these volumes can spill toohello cloud.</span></span> <span data-ttu-id="b5708-112">Když vytvoříte vrstvený svazek, na místní úrovni hello je zřízený přibližně 10 % hello místa a 90 % prostoru hello se zřizuje v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="b5708-112">When you create a tiered volume, approximately 10 % of hello space is provisioned on hello local tier and 90 % of hello space is provisioned in hello cloud.</span></span> <span data-ttu-id="b5708-113">Například pokud jste zřídili svazku 1 TB, 100 GB by nacházet v místním prostoru hello a 900 GB se použije v cloudu hello při hello datové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="b5708-113">For example, if you provisioned a 1 TB volume, 100 GB would reside in hello local space and 900 GB would be used in hello cloud when hello data tiers.</span></span> <span data-ttu-id="b5708-114">Pak z toho vyplývá, že pokud spustíte mimo všechny hello volné místo na hello zařízení, nejde zřídit vrstvený svazek (protože hello 10 % požadované na hello místní vrstvy nebudete mít k dispozici).</span><span class="sxs-lookup"><span data-stu-id="b5708-114">This in turn implies that if you run out of all hello local space on hello device, you cannot provision a tiered volume (because hello 10 % required on hello local tier will not be available).</span></span>

### <a name="provisioned-capacity"></a><span data-ttu-id="b5708-115">Zřízená kapacita</span><span class="sxs-lookup"><span data-stu-id="b5708-115">Provisioned capacity</span></span>
<span data-ttu-id="b5708-116">Naleznete toohello následující tabulka pro maximální zřízená kapacita pro každý typ svazku.</span><span class="sxs-lookup"><span data-stu-id="b5708-116">Refer toohello following table for maximum provisioned capacity for each volume type.</span></span>

| <span data-ttu-id="b5708-117">**Identifikátor limit**</span><span class="sxs-lookup"><span data-stu-id="b5708-117">**Limit identifier**</span></span>                                       | <span data-ttu-id="b5708-118">**Limit**</span><span class="sxs-lookup"><span data-stu-id="b5708-118">**Limit**</span></span>     |
|------------------------------------------------------------|---------------|
| <span data-ttu-id="b5708-119">Minimální velikost vrstvený svazek</span><span class="sxs-lookup"><span data-stu-id="b5708-119">Minimum size of a tiered volume</span></span>                            | <span data-ttu-id="b5708-120">500 GB</span><span class="sxs-lookup"><span data-stu-id="b5708-120">500 GB</span></span>        |
| <span data-ttu-id="b5708-121">Maximální velikost vrstvený svazek</span><span class="sxs-lookup"><span data-stu-id="b5708-121">Maximum size of a tiered volume</span></span>                            | <span data-ttu-id="b5708-122">5 TB</span><span class="sxs-lookup"><span data-stu-id="b5708-122">5 TB</span></span>          |
| <span data-ttu-id="b5708-123">Minimální velikost místně vázaný svazek</span><span class="sxs-lookup"><span data-stu-id="b5708-123">Minimum size of a locally pinned volume</span></span>                    | <span data-ttu-id="b5708-124">50 GB</span><span class="sxs-lookup"><span data-stu-id="b5708-124">50 GB</span></span>         |
| <span data-ttu-id="b5708-125">Maximální velikost místně vázaný svazek</span><span class="sxs-lookup"><span data-stu-id="b5708-125">Maximum size of a locally pinned volume</span></span>                    | <span data-ttu-id="b5708-126">500 GB</span><span class="sxs-lookup"><span data-stu-id="b5708-126">500 GB</span></span>        |

## <a name="hello-volumes-blade"></a><span data-ttu-id="b5708-127">okno svazky Hello</span><span class="sxs-lookup"><span data-stu-id="b5708-127">hello Volumes blade</span></span>
<span data-ttu-id="b5708-128">Hello **svazky** nabídky na souhrnné okně vaší StorSimple služby zobrazí hello seznam svazků úložiště v daném poli StorSimple a toomanage, můžete je.</span><span class="sxs-lookup"><span data-stu-id="b5708-128">hello **Volumes** menu on your StorSimple service summary blade displays hello list of storage volumes on a given StorSimple array and allows you toomanage them.</span></span>

![Okno svazky](./media/storsimple-virtual-array-manage-volumes/volumes-blade.png)

<span data-ttu-id="b5708-130">Svazek se skládá z řady atributy:</span><span class="sxs-lookup"><span data-stu-id="b5708-130">A volume consists of a series of attributes:</span></span>

* <span data-ttu-id="b5708-131">**Název svazku** – popisný název, který musí být jedinečný a pomáhá identifikovat hello svazku.</span><span class="sxs-lookup"><span data-stu-id="b5708-131">**Volume Name** – A descriptive name that must be unique and helps identify hello volume.</span></span>
* <span data-ttu-id="b5708-132">**Stav** – může být online nebo offline.</span><span class="sxs-lookup"><span data-stu-id="b5708-132">**Status** – Can be online or offline.</span></span> <span data-ttu-id="b5708-133">Pokud je svazek offline, není viditelné tooinitiators (servery) povolený přístup toouse hello svazku.</span><span class="sxs-lookup"><span data-stu-id="b5708-133">If a volume is offline, it is not visible tooinitiators (servers) that are allowed access toouse hello volume.</span></span>
* <span data-ttu-id="b5708-134">**Typ** – Určuje, zda je svazek hello **nastavování** (hello výchozí) nebo **místně vázaný**.</span><span class="sxs-lookup"><span data-stu-id="b5708-134">**Type** – Indicates whether hello volume is **Tiered** (hello default) or **Locally pinned**.</span></span>
* <span data-ttu-id="b5708-135">**Kapacita** – určuje hello množství dat použít jako porovnání toohello celkové množství dat, které můžou být uložené ve iniciátor hello (server).</span><span class="sxs-lookup"><span data-stu-id="b5708-135">**Capacity** – specifies hello amount of data used as compared toohello total amount of data that can be stored by hello initiator (server).</span></span>
* <span data-ttu-id="b5708-136">**Zálohování** – v případě, že z hello pole virtuální zařízení StorSimple, jsou všechny svazky automaticky povolené pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="b5708-136">**Backup** – In case of hello StorSimple Virtual Array, all volumes are automatically enabled for backup.</span></span>
* <span data-ttu-id="b5708-137">**Připojení hostitele** – určuje hello iniciátorů (serverů), které jsou povoleny přístup toothis svazku.</span><span class="sxs-lookup"><span data-stu-id="b5708-137">**Connected hosts** – Specifies hello initiators (servers) that are allowed access toothis volume.</span></span>

![Podrobnosti o svazky](./media/storsimple-virtual-array-manage-volumes/volume-details.png)

<span data-ttu-id="b5708-139">Používejte hello pokyny v této kurz tooperform hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="b5708-139">Use hello instructions in this tutorial tooperform hello following tasks:</span></span>

* <span data-ttu-id="b5708-140">Přidání svazku</span><span class="sxs-lookup"><span data-stu-id="b5708-140">Add a volume</span></span>
* <span data-ttu-id="b5708-141">Upravit svazku</span><span class="sxs-lookup"><span data-stu-id="b5708-141">Modify a volume</span></span>
* <span data-ttu-id="b5708-142">Svazek převést do režimu offline</span><span class="sxs-lookup"><span data-stu-id="b5708-142">Take a volume offline</span></span>
* <span data-ttu-id="b5708-143">Odstranění svazku</span><span class="sxs-lookup"><span data-stu-id="b5708-143">Delete a volume</span></span>

## <a name="add-a-volume"></a><span data-ttu-id="b5708-144">Přidání svazku</span><span class="sxs-lookup"><span data-stu-id="b5708-144">Add a volume</span></span>

1. <span data-ttu-id="b5708-145">Z hello StorSimple služby souhrnu okna, klikněte na tlačítko **+ přidat svazek** z příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="b5708-145">From hello StorSimple service summary blade, click **+ Add volume** from hello command bar.</span></span> <span data-ttu-id="b5708-146">Otevře se hello **Přidání svazku** okno.</span><span class="sxs-lookup"><span data-stu-id="b5708-146">This opens up hello **Add volume** blade.</span></span>
   
    ![Přidání svazku](./media/storsimple-virtual-array-manage-volumes/add-volume.png)
2. <span data-ttu-id="b5708-148">V hello **Přidání svazku** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="b5708-148">In hello **Add volume** blade, do hello following:</span></span>
   
   * <span data-ttu-id="b5708-149">V hello **název svazku** pole, zadejte jedinečný název pro svazek.</span><span class="sxs-lookup"><span data-stu-id="b5708-149">In hello **Volume name** field, enter a unique name for your volume.</span></span> <span data-ttu-id="b5708-150">Název Hello musí být řetězec, který obsahuje 3 too127 znaky.</span><span class="sxs-lookup"><span data-stu-id="b5708-150">hello name must be a string that contains 3 too127 characters.</span></span>
   * <span data-ttu-id="b5708-151">V hello **typ** rozevíracího seznamu, zadejte zda toocreate **nastavování** nebo **místně vázaný** svazku.</span><span class="sxs-lookup"><span data-stu-id="b5708-151">In hello **Type** dropdown list, specify whether toocreate a **Tiered** or **Locally pinned** volume.</span></span> <span data-ttu-id="b5708-152">Pro úlohy, které vyžadují místní záruky, nízkou latenci a vyšší výkon, vyberte **místně vázaný svazek**.</span><span class="sxs-lookup"><span data-stu-id="b5708-152">For workloads that require local guarantees, low latencies, and higher performance, select **Locally pinned volume**.</span></span> <span data-ttu-id="b5708-153">Všechna ostatní data, vyberte **nastavování** svazku.</span><span class="sxs-lookup"><span data-stu-id="b5708-153">For all other data, select **Tiered** volume.</span></span>
   * <span data-ttu-id="b5708-154">V hello **kapacity** pole, zadejte hello velikost svazku hello.</span><span class="sxs-lookup"><span data-stu-id="b5708-154">In hello **Capacity** field, specify hello size of hello volume.</span></span> <span data-ttu-id="b5708-155">Vrstvený svazek musí být mezi 500 GB a 5 TB a místně vázaný svazek musí být mezi 50 GB a 500 GB.</span><span class="sxs-lookup"><span data-stu-id="b5708-155">A tiered volume must be between 500 GB and 5 TB and a locally pinned volume must be between 50 GB and 500 GB.</span></span>
   * * <span data-ttu-id="b5708-156">Klikněte na tlačítko **připojení hostitele**, vyberte přístup k řízení záznamu (ACR) odpovídající toohello iniciátor iSCSI má tooconnect toothis svazek a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="b5708-156">Click **Connected hosts**, select an access control record (ACR) corresponding toohello iSCSI initiator that you want tooconnect toothis volume, and then click **Select**.</span></span>
3. <span data-ttu-id="b5708-157">Klikněte na tlačítko tooadd na nového hostitele připojené **přidat nový**, zadejte název hostitele hello a jeho iSCSI kvalifikovaný název IQN () a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b5708-157">tooadd a new connected host, click **Add new**, enter a name for hello host and its iSCSI Qualified Name (IQN), and then click **Add**.</span></span>
   
    ![Přidání svazku](./media/storsimple-virtual-array-manage-volumes/volume-add-acr.png)
4. <span data-ttu-id="b5708-159">Po dokončení konfigurace svazku, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b5708-159">When you've finished configuring your volume, click **Create**.</span></span> <span data-ttu-id="b5708-160">Vytvoří se svazek s hello zadaná nastavení a můžete se zobrazí oznámení o úspěšném vytvoření hello hello stejné.</span><span class="sxs-lookup"><span data-stu-id="b5708-160">A volume will be created with hello specified settings and you will see a notification on hello successful creation of hello same.</span></span> <span data-ttu-id="b5708-161">Ve výchozím nastavení zálohování budou povolené pro svazek hello.</span><span class="sxs-lookup"><span data-stu-id="b5708-161">By default backup will be enabled for hello volume.</span></span>
5. <span data-ttu-id="b5708-162">tooconfirm, který hello svazek byl úspěšně vytvořil, přejděte toohello **svazky** okno.</span><span class="sxs-lookup"><span data-stu-id="b5708-162">tooconfirm that hello volume was successfully created, go toohello **Volumes** blade.</span></span> <span data-ttu-id="b5708-163">Měli byste vidět hello svazku uvedené.</span><span class="sxs-lookup"><span data-stu-id="b5708-163">You should see hello volume listed.</span></span>
   
    ![Úspěšné vytvoření svazku](./media/storsimple-virtual-array-manage-volumes/volume-success.png)

## <a name="modify-a-volume"></a><span data-ttu-id="b5708-165">Upravit svazku</span><span class="sxs-lookup"><span data-stu-id="b5708-165">Modify a volume</span></span>

<span data-ttu-id="b5708-166">Upravte svazek, když potřebujete toochange hello hostitele, kteří přístup hello svazku.</span><span class="sxs-lookup"><span data-stu-id="b5708-166">Modify a volume when you need toochange hello hosts that access hello volume.</span></span> <span data-ttu-id="b5708-167">Hello ostatní atributy svazek nelze změnit po vytvoření svazku hello.</span><span class="sxs-lookup"><span data-stu-id="b5708-167">hello other attributes of a volume cannot be modified once hello volume has been created.</span></span>

#### <a name="toomodify-a-volume"></a><span data-ttu-id="b5708-168">toomodify svazku</span><span class="sxs-lookup"><span data-stu-id="b5708-168">toomodify a volume</span></span>

1. <span data-ttu-id="b5708-169">Z hello **svazky** nastavit hello StorSimple služby souhrnu okna, vyberte hello virtuální pole, na které hello svazku, které chcete toomodify nachází.</span><span class="sxs-lookup"><span data-stu-id="b5708-169">From hello **Volumes** setting on hello StorSimple service summary blade, select hello virtual array on which hello volume you wish you toomodify resides.</span></span>
2. <span data-ttu-id="b5708-170">**Vyberte** hello svazku a klikněte na tlačítko **připojení hostitele** tooview hello aktuálně připojené hostitele a upravit ho tooa jiný server.</span><span class="sxs-lookup"><span data-stu-id="b5708-170">**Select** hello volume and click **Connected hosts** tooview hello currently connected host and modify it tooa different server.</span></span>
   
    ![Upravit svazku](./media/storsimple-virtual-array-manage-volumes/volume-edit-acr.png)
3. <span data-ttu-id="b5708-172">Uložte změny kliknutím hello **Uložit** panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="b5708-172">Save your changes by clicking hello **Save** command bar.</span></span> <span data-ttu-id="b5708-173">Zadané nastavení uplatní a zobrazí se oznámení.</span><span class="sxs-lookup"><span data-stu-id="b5708-173">Your specified settings will be applied and you will see a notification.</span></span>

## <a name="take-a-volume-offline"></a><span data-ttu-id="b5708-174">Svazek převést do režimu offline</span><span class="sxs-lookup"><span data-stu-id="b5708-174">Take a volume offline</span></span>

<span data-ttu-id="b5708-175">Můžete potřebovat tootake svazek offline při plánování toomodify ho nebo odstraňte ji.</span><span class="sxs-lookup"><span data-stu-id="b5708-175">You may need tootake a volume offline when you are planning toomodify it or delete it.</span></span> <span data-ttu-id="b5708-176">Pokud je svazek offline, není k dispozici pro přístup pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="b5708-176">When a volume is offline, it is not available for read-write access.</span></span> <span data-ttu-id="b5708-177">Budete potřebovat tootake hello svazku do offline režimu na hostiteli hello i hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="b5708-177">You will need tootake hello volume offline on hello host as well as on hello device.</span></span>

#### <a name="tootake-a-volume-offline"></a><span data-ttu-id="b5708-178">tootake svazek offline</span><span class="sxs-lookup"><span data-stu-id="b5708-178">tootake a volume offline</span></span>

1. <span data-ttu-id="b5708-179">Ujistěte se, že svazek hello dotyčném není používán před přepnutím do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="b5708-179">Make sure that hello volume in question is not in use before taking it offline.</span></span>
2. <span data-ttu-id="b5708-180">Trvat hello svazku do offline režimu na hostiteli hello první.</span><span class="sxs-lookup"><span data-stu-id="b5708-180">Take hello volume offline on hello host first.</span></span> <span data-ttu-id="b5708-181">Tím se eliminuje všechny potenciální riziko poškození dat na svazku hello.</span><span class="sxs-lookup"><span data-stu-id="b5708-181">This eliminates any potential risk of data corruption on hello volume.</span></span> <span data-ttu-id="b5708-182">Konkrétní kroky najdete v části toohello pokyny pro operační systém hostitele.</span><span class="sxs-lookup"><span data-stu-id="b5708-182">For specific steps, refer toohello instructions for your host operating system.</span></span>
3. <span data-ttu-id="b5708-183">Po hello svazek na hostiteli hello je offline, trvat hello svazek v poli hello offline provedením hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b5708-183">After hello volume on hello host is offline, take hello volume on hello array  offline by performing hello following steps:</span></span>
   
   * <span data-ttu-id="b5708-184">Z hello **svazky** nastavit hello StorSimple služby souhrnu okna, vyberte hello virtuální pole, na které hello nachází svazku můžete chcete tootake do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="b5708-184">From hello **Volumes** setting on hello StorSimple service summary blade, select hello virtual array on which hello volume you wish you tootake offline resides.</span></span>
   * <span data-ttu-id="b5708-185">**Vyberte** hello svazku a klikněte na tlačítko **...**  (případně klikněte pravým tlačítkem na tento řádek) a z hello kontextové nabídky, vyberte **převést do režimu offline**.</span><span class="sxs-lookup"><span data-stu-id="b5708-185">**Select** hello volume and click **...** (alternately right-click in this row) and from hello context menu, select **Take offline**.</span></span>
     
        ![Offline svazku](./media/storsimple-virtual-array-manage-volumes/volume-offline.png)
   * <span data-ttu-id="b5708-187">Zkontrolujte informace hello v hello **převést do režimu offline** okno a potvrďte, že přijímáte hello operace.</span><span class="sxs-lookup"><span data-stu-id="b5708-187">Review hello information in hello **Take offline** blade and confirm your acceptance of hello operation.</span></span> <span data-ttu-id="b5708-188">Klikněte na tlačítko **převést do režimu offline** tootake hello svazek do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="b5708-188">Click **Take offline** tootake hello volume offline.</span></span> <span data-ttu-id="b5708-189">Zobrazí se oznámení hello operace probíhá.</span><span class="sxs-lookup"><span data-stu-id="b5708-189">You will see a notification of hello operation in progress.</span></span>
   * <span data-ttu-id="b5708-190">tooconfirm, který hello svazek byl úspěšně do režimu offline, přejděte toohello **svazky** okno.</span><span class="sxs-lookup"><span data-stu-id="b5708-190">tooconfirm that hello volume was successfully taken offline, go toohello **Volumes** blade.</span></span> <span data-ttu-id="b5708-191">Měli byste vidět hello stav svazku hello jako offline.</span><span class="sxs-lookup"><span data-stu-id="b5708-191">You should see hello status of hello volume as offline.</span></span>
     
       ![Potvrzení offline svazku](./media/storsimple-virtual-array-manage-volumes/volume-offline-confirm.png)

## <a name="delete-a-volume"></a><span data-ttu-id="b5708-193">Odstranění svazku</span><span class="sxs-lookup"><span data-stu-id="b5708-193">Delete a volume</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b5708-194">Svazek můžete odstranit pouze v případě, že je offline.</span><span class="sxs-lookup"><span data-stu-id="b5708-194">You can delete a volume only if it is offline.</span></span>
> 
> 

<span data-ttu-id="b5708-195">Proveďte následující kroky toodelete svazek hello.</span><span class="sxs-lookup"><span data-stu-id="b5708-195">Complete hello following steps toodelete a volume.</span></span>

#### <a name="toodelete-a-volume"></a><span data-ttu-id="b5708-196">toodelete svazku</span><span class="sxs-lookup"><span data-stu-id="b5708-196">toodelete a volume</span></span>

1. <span data-ttu-id="b5708-197">Z hello **svazky** nastavit hello StorSimple služby souhrnu okna, vyberte hello virtuální pole, na které hello svazku, které chcete toodelete nachází.</span><span class="sxs-lookup"><span data-stu-id="b5708-197">From hello **Volumes** setting on hello StorSimple service summary blade, select hello virtual array on which hello volume you wish you toodelete resides.</span></span>
2. <span data-ttu-id="b5708-198">**Vyberte** hello svazku a klikněte na tlačítko **...**  (případně klikněte pravým tlačítkem na tento řádek) a z hello kontextové nabídky, vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="b5708-198">**Select** hello volume and click **...** (alternately right-click in this row) and from hello context menu, select **Delete**.</span></span>
   
    ![Odstranění svazku](./media/storsimple-virtual-array-manage-volumes/volume-delete.png)
3. <span data-ttu-id="b5708-200">Zkontrolujte stav hello hello svazku chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="b5708-200">Check hello status of hello volume you want toodelete.</span></span> <span data-ttu-id="b5708-201">Pokud chcete toodelete svazku hello není v režimu offline, proveďte ho offline hello první, následující kroky [do offline režimu svazku](#take-a-volume-offline).</span><span class="sxs-lookup"><span data-stu-id="b5708-201">If hello volume you want toodelete is not offline, take it offline first, following hello steps in [Take a volume offline](#take-a-volume-offline).</span></span>
4. <span data-ttu-id="b5708-202">Po zobrazení výzvy k potvrzení v hello **odstranit** přijměte potvrzení hello a klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="b5708-202">When prompted for confirmation in hello **Delete** blade, accept hello confirmation and click **Delete**.</span></span> <span data-ttu-id="b5708-203">Hello svazek bude odstraněno a hello **svazky** okno se zobrazí hello aktualizovat seznam svazků v rámci pole virtuálním hello.</span><span class="sxs-lookup"><span data-stu-id="b5708-203">hello volume will now be deleted and hello **Volumes** blade will show hello updated list of volumes within hello virtual array.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5708-204">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5708-204">Next steps</span></span>

<span data-ttu-id="b5708-205">Zjistěte, jak příliš[klonovat svazek StorSimple](storsimple-virtual-array-clone.md).</span><span class="sxs-lookup"><span data-stu-id="b5708-205">Learn how too[clone a StorSimple volume](storsimple-virtual-array-clone.md).</span></span>

