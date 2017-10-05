---
title: "Spravovat zařízení s Snapshot Manager zařízení StorSimple | Microsoft Docs"
description: "Popisuje způsob použití modulu snap-in konzoly MMC StorSimple Snapshot Manager k připojení a správě zařízení StorSimple."
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 966ecbe3-a7fa-4752-825f-6694dd949946
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: f5e3186a4271e0be781f367fa75ada195c58c960
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a><span data-ttu-id="215c7-103">Použít k připojení a správě zařízení StorSimple Snapshot Manager zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="215c7-103">Use StorSimple Snapshot Manager to connect and manage StorSimple devices</span></span>
## <a name="overview"></a><span data-ttu-id="215c7-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="215c7-104">Overview</span></span>
<span data-ttu-id="215c7-105">Uzly můžete použít v Snapshot Manager zařízení StorSimple **oboru** podokně ověřte importovaných dat zařízení StorSimple a aktualizujte připojené paměťové zařízení.</span><span class="sxs-lookup"><span data-stu-id="215c7-105">You can use nodes in the StorSimple Snapshot Manager **Scope** pane to verify imported StorSimple device data and refresh connected storage devices.</span></span> <span data-ttu-id="215c7-106">Kromě toho, když kliknete **zařízení** uzlu, zobrazí se seznam připojených zařízení a odpovídající informace o stavu v **výsledky** podokně.</span><span class="sxs-lookup"><span data-stu-id="215c7-106">Additionally, when you click the **Devices** node, you can see a list of connected devices and corresponding status information in the **Results** pane.</span></span>

![Připojená zařízení](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

<span data-ttu-id="215c7-108">**Obrázek 1: StorSimple Snapshot Manager připojeného zařízení**</span><span class="sxs-lookup"><span data-stu-id="215c7-108">**Figure 1: StorSimple Snapshot Manager connected device**</span></span> 

<span data-ttu-id="215c7-109">V závislosti na vaší **zobrazení** výběr **výsledky** podokně se zobrazují následující informace o každé zařízení.</span><span class="sxs-lookup"><span data-stu-id="215c7-109">Depending on your **View** selections, the **Results** pane shows the following information about each device.</span></span> <span data-ttu-id="215c7-110">(Další informace o konfiguraci zobrazení, přejděte na [nabídka Zobrazit](storsimple-use-snapshot-manager.md#view-menu).</span><span class="sxs-lookup"><span data-stu-id="215c7-110">(For more information about configuring a view, go to [View menu](storsimple-use-snapshot-manager.md#view-menu).</span></span>

| <span data-ttu-id="215c7-111">Sloupec výsledků</span><span class="sxs-lookup"><span data-stu-id="215c7-111">Results column</span></span> | <span data-ttu-id="215c7-112">Popis</span><span class="sxs-lookup"><span data-stu-id="215c7-112">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="215c7-113">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="215c7-113">Name</span></span> |<span data-ttu-id="215c7-114">Název zařízení podle konfigurace v portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="215c7-114">The name of the device as configured in the Azure classic portal</span></span> |
| <span data-ttu-id="215c7-115">Model</span><span class="sxs-lookup"><span data-stu-id="215c7-115">Model</span></span> |<span data-ttu-id="215c7-116">Číslo modelu zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-116">The model number of the device</span></span> |
| <span data-ttu-id="215c7-117">Verze</span><span class="sxs-lookup"><span data-stu-id="215c7-117">Version</span></span> |<span data-ttu-id="215c7-118">Verze softwaru na zařízení nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="215c7-118">The version of the software installed on the device</span></span> |
| <span data-ttu-id="215c7-119">Status</span><span class="sxs-lookup"><span data-stu-id="215c7-119">Status</span></span> |<span data-ttu-id="215c7-120">Zda je k dispozici zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-120">Whether the device is available</span></span> |
| <span data-ttu-id="215c7-121">Poslední synchronizace proběhla</span><span class="sxs-lookup"><span data-stu-id="215c7-121">Last Synced</span></span> |<span data-ttu-id="215c7-122">Datum a čas, kdy zařízení poslední synchronizace</span><span class="sxs-lookup"><span data-stu-id="215c7-122">Date and time when the device was last synchronized</span></span> |
| <span data-ttu-id="215c7-123">Sériové číslo.</span><span class="sxs-lookup"><span data-stu-id="215c7-123">Serial No.</span></span> |<span data-ttu-id="215c7-124">Sériové číslo zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-124">The serial number for the device</span></span> |

<span data-ttu-id="215c7-125">Pokud kliknete pravým tlačítkem na **zařízení** uzlu **oboru** podokně, můžete vybrat z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="215c7-125">If you right-click the **Devices** node in the **Scope** pane, you can select from the following actions:</span></span>

* <span data-ttu-id="215c7-126">Přidat nebo nahradit zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-126">Add or replace a device</span></span>
* <span data-ttu-id="215c7-127">Připojte zařízení a ověřte importy</span><span class="sxs-lookup"><span data-stu-id="215c7-127">Connect a device and verify imports</span></span>
* <span data-ttu-id="215c7-128">Aktualizujte připojená zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-128">Refresh connected devices</span></span>

<span data-ttu-id="215c7-129">Pokud kliknete **zařízení** uzel a potom klikněte pravým tlačítkem na zařízení názvu **výsledky** podokně, můžete vybrat z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="215c7-129">If you click the **Devices** node and then right-click a device name in the **Results** pane, you can select from the following actions:</span></span>

* <span data-ttu-id="215c7-130">Ověřování zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-130">Authenticate a device</span></span>
* <span data-ttu-id="215c7-131">Zobrazení podrobností o zařízeních</span><span class="sxs-lookup"><span data-stu-id="215c7-131">View device details</span></span>
* <span data-ttu-id="215c7-132">Aktualizace zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-132">Refresh a device</span></span>
* <span data-ttu-id="215c7-133">Odstranění konfigurace zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-133">Delete a device configuration</span></span>
* <span data-ttu-id="215c7-134">Změnit heslo k zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-134">Change a device password</span></span>

> [!NOTE]
> <span data-ttu-id="215c7-135">Všechny tyto akce jsou také k dispozici v **akce** podokně.</span><span class="sxs-lookup"><span data-stu-id="215c7-135">All of these actions are also available in the **Actions** pane.</span></span>


<span data-ttu-id="215c7-136">Tento kurz vysvětluje, jak použít StorSimple Snapshot Manager k připojení a správě zařízení a provádět následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="215c7-136">This tutorial explains how to use StorSimple Snapshot Manager to connect and manage devices and perform the following tasks:</span></span>

* <span data-ttu-id="215c7-137">Přidat nebo nahradit zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-137">Add or replace a device</span></span>
* <span data-ttu-id="215c7-138">Připojte zařízení a ověřte importy</span><span class="sxs-lookup"><span data-stu-id="215c7-138">Connect a device and verify imports</span></span>
* <span data-ttu-id="215c7-139">Aktualizujte připojená zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-139">Refresh connected devices</span></span>
* <span data-ttu-id="215c7-140">Ověřování zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-140">Authenticate a device</span></span>
* <span data-ttu-id="215c7-141">Zobrazení podrobností o zařízeních</span><span class="sxs-lookup"><span data-stu-id="215c7-141">View device details</span></span>
* <span data-ttu-id="215c7-142">Aktualizace k jednotlivým zařízením</span><span class="sxs-lookup"><span data-stu-id="215c7-142">Refresh an individual device</span></span>
* <span data-ttu-id="215c7-143">Odstranění konfigurace zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-143">Delete a device configuration</span></span>
* <span data-ttu-id="215c7-144">Změnit heslo k zařízení s vypršenou platností</span><span class="sxs-lookup"><span data-stu-id="215c7-144">Change an expired device password</span></span>
* <span data-ttu-id="215c7-145">Nahraďte zařízení se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="215c7-145">Replace a failed device</span></span>

> [!NOTE]
> <span data-ttu-id="215c7-146">Obecné informace o používání rozhraní Snapshot Manager zařízení StorSimple, přejděte na [StorSimple Snapshot Manager uživatelské rozhraní](storsimple-use-snapshot-manager.md).</span><span class="sxs-lookup"><span data-stu-id="215c7-146">For general information about using the StorSimple Snapshot Manager interface, go to [StorSimple Snapshot Manager user interface](storsimple-use-snapshot-manager.md).</span></span>


## <a name="add-or-replace-a-device"></a><span data-ttu-id="215c7-147">Přidat nebo nahradit zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-147">Add or replace a device</span></span>
<span data-ttu-id="215c7-148">Pomocí následujícího postupu můžete přidat nebo nahradit zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="215c7-148">Use the following procedure to add or replace a StorSimple device.</span></span>

#### <a name="to-add-or-replace-a-device"></a><span data-ttu-id="215c7-149">Chcete přidat nebo nahradit zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-149">To add or replace a device</span></span>
1. <span data-ttu-id="215c7-150">Klikněte na ikonu plochy spusťte StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="215c7-150">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="215c7-151">V **oboru** podokně klikněte pravým tlačítkem myši **zařízení** uzel a pak klikněte na tlačítko **nakonfigurovat zařízení**.</span><span class="sxs-lookup"><span data-stu-id="215c7-151">In the **Scope** pane, right-click the **Devices** node, and then click **Configure a device**.</span></span> <span data-ttu-id="215c7-152">**Nakonfigurovat zařízení** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="215c7-152">The **Configure a Device** dialog box appears.</span></span>
   
    ![Konfigurace zařízení StorSimple](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 
3. <span data-ttu-id="215c7-154">V **zařízení** rozevíracího seznamu vyberte adresu IP virtuálního zařízení nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="215c7-154">In the **Device** drop-down box, select the IP address of the device or virtual device.</span></span> 
4. <span data-ttu-id="215c7-155">V **heslo** textové pole, zadejte heslo Snapshot Manager zařízení StorSimple, kterou jste vytvořili pro zařízení na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="215c7-155">In the **Password** text box, type the StorSimple Snapshot Manager password that you created for the device in the Azure classic portal.</span></span> <span data-ttu-id="215c7-156">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="215c7-156">Click **OK**.</span></span> <span data-ttu-id="215c7-157">StorSimple Snapshot Manager hledá zařízení, které jste určili.</span><span class="sxs-lookup"><span data-stu-id="215c7-157">StorSimple Snapshot Manager searches for the device that you identified.</span></span> 
   
   * <span data-ttu-id="215c7-158">Pokud zařízení není k dispozici, StorSimple Snapshot Manager přidá připojení.</span><span class="sxs-lookup"><span data-stu-id="215c7-158">If the device is available, StorSimple Snapshot Manager adds a connection.</span></span>
   * <span data-ttu-id="215c7-159">Pokud zařízení není k dispozici z jakéhokoli důvodu, StorSimple Snapshot Manager vrátí chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="215c7-159">If the device is unavailable for any reason, StorSimple Snapshot Manager returns an error message.</span></span> <span data-ttu-id="215c7-160">Klikněte na tlačítko **OK** zavřete chybovou zprávu, a pak klikněte na **zrušit** zavřete **nakonfigurovat zařízení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="215c7-160">Click **OK** to close the error message, and then click **Cancel** to close the **Configure a Device** dialog box.</span></span>

## <a name="connect-a-device-and-verify-imports"></a><span data-ttu-id="215c7-161">Připojte zařízení a ověřte importy</span><span class="sxs-lookup"><span data-stu-id="215c7-161">Connect a device and verify imports</span></span>
<span data-ttu-id="215c7-162">Následující postup použijte k připojení zařízení StorSimple a ověřit, zda jsou naimportovány jakékoli existující svazek skupiny, které mají přidružené zálohy.</span><span class="sxs-lookup"><span data-stu-id="215c7-162">Use the following procedure to connect a StorSimple device and verify that any existing volume groups that have associated backups are imported.</span></span>

#### <a name="to-connect-a-device-and-verify-imports"></a><span data-ttu-id="215c7-163">K připojení zařízení a ověřte importy</span><span class="sxs-lookup"><span data-stu-id="215c7-163">To connect a device and verify imports</span></span>
1. <span data-ttu-id="215c7-164">K připojení k systému StorSimple Snapshot Manager zařízení, postupujte podle pokynů v přidat nebo nahradit zařízení.</span><span class="sxs-lookup"><span data-stu-id="215c7-164">To connect a device to StorSimple Snapshot Manager, follow the instructions in Add or replace a device.</span></span> <span data-ttu-id="215c7-165">Při připojení zařízení StorSimple Snapshot Manager zareaguje takto:</span><span class="sxs-lookup"><span data-stu-id="215c7-165">When it connects to a device, StorSimple Snapshot Manager responds as follows:</span></span>
   
   * <span data-ttu-id="215c7-166">Pokud zařízení není k dispozici z jakéhokoli důvodu, StorSimple Snapshot Manager vrátí chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="215c7-166">If the device is unavailable for any reason, StorSimple Snapshot Manager returns an error message.</span></span> 
   
   * <span data-ttu-id="215c7-167">Pokud zařízení není k dispozici, StorSimple Snapshot Manager přidá připojení.</span><span class="sxs-lookup"><span data-stu-id="215c7-167">If the device is available, StorSimple Snapshot Manager adds a connection.</span></span> <span data-ttu-id="215c7-168">Pokud vyberete zařízení, zobrazí se v **výsledky** podokno a stav pole označuje, že zařízení je **dostupné**.</span><span class="sxs-lookup"><span data-stu-id="215c7-168">When you select the device, it appears in the **Results** pane, and the status field indicates that the device is **Available**.</span></span> <span data-ttu-id="215c7-169">StorSimple Snapshot Manager importuje všechny svazku skupiny nakonfigurované pro zařízení, za předpokladu, že svazek skupiny mají přidružené zálohy.</span><span class="sxs-lookup"><span data-stu-id="215c7-169">StorSimple Snapshot Manager imports any volume groups configured for the device, provided that the volume groups have associated backups.</span></span> <span data-ttu-id="215c7-170">Zásady zálohování nebyly naimportovány.</span><span class="sxs-lookup"><span data-stu-id="215c7-170">Backup policies are not imported.</span></span> <span data-ttu-id="215c7-171">Svazek skupin, které nemají přidružených záloh nebyly naimportovány.</span><span class="sxs-lookup"><span data-stu-id="215c7-171">Volume groups that do not have associated backups are not imported.</span></span>
2. <span data-ttu-id="215c7-172">Klikněte na ikonu plochy spusťte StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="215c7-172">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
3. <span data-ttu-id="215c7-173">Klikněte pravým tlačítkem myši na nejvyšší uzel v **oboru** podokně a pak klikněte na tlačítko **přepnout importů zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="215c7-173">Right-click the top node in the **Scope** pane, and then click **Toggle Imports Display**.</span></span>
   
    ![Vyberte možnost přepnout importů zobrazení](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 
4. <span data-ttu-id="215c7-175">**Přepnout importů zobrazení** se zobrazí dialogové okno, zobrazením stavu skupiny importované svazku a zálohování.</span><span class="sxs-lookup"><span data-stu-id="215c7-175">The **Toggle Imports Display** dialog box appears, showing the status of the imported volume groups and backups.</span></span> <span data-ttu-id="215c7-176">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="215c7-176">Click **OK**.</span></span>

<span data-ttu-id="215c7-177">Jakmile skupiny svazku a zálohování jsou úspěšně importovány, můžete použít StorSimple Snapshot Manager spravovat, stejně, jako by spravovat skupiny svazku a zálohování, které vytvoříte a nakonfigurujete s StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="215c7-177">After the volume groups and backups are successfully imported, you can use StorSimple Snapshot Manager to manage them, just as you would manage volume groups and backups that you created and configured with StorSimple Snapshot Manager.</span></span> 

## <a name="refresh-connected-devices"></a><span data-ttu-id="215c7-178">Aktualizujte připojená zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-178">Refresh connected devices</span></span>
<span data-ttu-id="215c7-179">Použijte následující postup k synchronizaci s Snapshot Manager zařízení StorSimple připojené zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="215c7-179">Use the following procedure to synchronize the connected StorSimple devices with StorSimple Snapshot Manager.</span></span>

#### <a name="to-refresh-connected-devices"></a><span data-ttu-id="215c7-180">K aktualizaci připojeného zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-180">To refresh connected devices</span></span>
1. <span data-ttu-id="215c7-181">Klikněte na ikonu plochy spusťte StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="215c7-181">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="215c7-182">V **oboru** podokně klikněte pravým tlačítkem na **zařízení**a potom klikněte na **aktualizace zařízení**.</span><span class="sxs-lookup"><span data-stu-id="215c7-182">In the **Scope** pane, right-click **Devices**, and then click **Refresh Devices**.</span></span> <span data-ttu-id="215c7-183">Provádí synchronizaci připojená zařízení s StorSimple Snapshot Manager tak, aby můžete zobrazit skupiny svazku a zálohování, včetně všech nedávno přidány.</span><span class="sxs-lookup"><span data-stu-id="215c7-183">This synchronizes the connected devices with StorSimple Snapshot Manager so that you can view the volume groups and backups, including any recent additions.</span></span> 
   
    ![Aktualizace zařízení StorSimple](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)

<span data-ttu-id="215c7-185">**Aktualizace zařízení** akce načte všechny nové skupiny svazek a všechny přidružené zálohy z připojených zařízení.</span><span class="sxs-lookup"><span data-stu-id="215c7-185">The **Refresh Devices** action retrieves any new volume groups and any associated backups from connected devices.</span></span> <span data-ttu-id="215c7-186">Na rozdíl od **Prohledat znovu svazky** akce, které jsou k dispozici pro **svazky** uzlu, **aktualizujte zařízení** neobnoví zálohování registru.</span><span class="sxs-lookup"><span data-stu-id="215c7-186">Unlike the **Rescan volumes** action available for the **Volumes** node, **Refresh Devices** does not restore the backup registry.</span></span>

## <a name="authenticate-a-device"></a><span data-ttu-id="215c7-187">Ověřování zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-187">Authenticate a device</span></span>
<span data-ttu-id="215c7-188">Použijte následující postup k ověření s StorSimple Snapshot Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="215c7-188">Use the following procedure to authenticate a StorSimple device with StorSimple Snapshot Manager.</span></span>

#### <a name="to-authenticate-a-device"></a><span data-ttu-id="215c7-189">K ověření zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-189">To authenticate a device</span></span>
1. <span data-ttu-id="215c7-190">Klikněte na ikonu plochy spusťte StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="215c7-190">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="215c7-191">V **oboru** podokně klikněte na tlačítko **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="215c7-191">In the **Scope** pane, click **Devices**.</span></span>
3. <span data-ttu-id="215c7-192">V **výsledky** podokně klikněte pravým tlačítkem na název zařízení a pak klikněte na tlačítko **ověřit**.</span><span class="sxs-lookup"><span data-stu-id="215c7-192">In the **Results** pane, right-click the name of the device, and then click **Authenticate**.</span></span>
4. <span data-ttu-id="215c7-193">**Ověřit** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="215c7-193">The **Authenticate** dialog box appears.</span></span> <span data-ttu-id="215c7-194">Zadejte heslo k zařízení a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="215c7-194">Type the device password, and then click **OK**.</span></span>
   
    ![Ověření dialogové okno](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 

## <a name="view-device-details"></a><span data-ttu-id="215c7-196">Zobrazení podrobností o zařízeních</span><span class="sxs-lookup"><span data-stu-id="215c7-196">View device details</span></span>
<span data-ttu-id="215c7-197">Následující postup použijte k zobrazení podrobností o zařízení StorSimple a v případě potřeby znovu spusťte synchronizaci s StorSimple Snapshot Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="215c7-197">Use the following procedure to view the details of a StorSimple device and, if necessary, resynchronize the device with StorSimple Snapshot Manager.</span></span>

#### <a name="to-view-and-resynchronize-device-details"></a><span data-ttu-id="215c7-198">K zobrazení a Opětovná synchronizace podrobnosti o zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-198">To view and resynchronize device details</span></span>
1. <span data-ttu-id="215c7-199">Klikněte na ikonu plochy spusťte StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="215c7-199">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="215c7-200">V **oboru** podokně klikněte na tlačítko **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="215c7-200">In the **Scope** pane, click **Devices**.</span></span>
3. <span data-ttu-id="215c7-201">V **výsledky** podokně klikněte pravým tlačítkem na název zařízení a pak klikněte na tlačítko **podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="215c7-201">In the **Results** pane, right-click the name of the device, and then click **Details**.</span></span>

<span data-ttu-id="215c7-202">4 na **podrobnosti o zařízení** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="215c7-202">4.The **Device Details** dialog box appears.</span></span> <span data-ttu-id="215c7-203">Toto pole ukazuje název, modelu, verzi, sériové číslo, stav, cíl iSCSI kvalifikovaný název IQN () a poslední synchronizace datum a čas.</span><span class="sxs-lookup"><span data-stu-id="215c7-203">This box shows the name, model, version, serial number, status, target iSCSI Qualified Name (IQN), and last synchronization date and time.</span></span>

* <span data-ttu-id="215c7-204">Klikněte na tlačítko **nové synchronizace** synchronizovat zařízení.</span><span class="sxs-lookup"><span data-stu-id="215c7-204">Click **Resync** to synchronize the device.</span></span>
* <span data-ttu-id="215c7-205">Klikněte na tlačítko **OK** nebo **zrušit** zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="215c7-205">Click **OK** or **Cancel** to close the dialog box.</span></span>
  
  ![Podrobnosti o zařízení](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 

## <a name="refresh-an-individual-device"></a><span data-ttu-id="215c7-207">Aktualizace k jednotlivým zařízením</span><span class="sxs-lookup"><span data-stu-id="215c7-207">Refresh an individual device</span></span>
<span data-ttu-id="215c7-208">Opětovná synchronizace jednotlivých s StorSimple Snapshot Manager zařízení StorSimple, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="215c7-208">Use the following procedure to resynchronize an individual StorSimple device with StorSimple Snapshot Manager.</span></span>

#### <a name="to-refresh-a-device"></a><span data-ttu-id="215c7-209">Aktualizace zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-209">To refresh a device</span></span>
1. <span data-ttu-id="215c7-210">Klikněte na ikonu plochy spusťte StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="215c7-210">Click the desktop icon to start StorSimple Snapshot Manager.</span></span> 
2. <span data-ttu-id="215c7-211">V **oboru** podokně klikněte na tlačítko **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="215c7-211">In the **Scope** pane, click **Devices**.</span></span> 
3. <span data-ttu-id="215c7-212">V **výsledky** podokně klikněte pravým tlačítkem na název zařízení a pak klikněte na tlačítko **aktualizace zařízení**.</span><span class="sxs-lookup"><span data-stu-id="215c7-212">In the **Results** pane, right-click the name of the device, and then click **Refresh Device**.</span></span> <span data-ttu-id="215c7-213">Provádí synchronizaci zařízení s StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="215c7-213">This synchronizes the device with StorSimple Snapshot Manager.</span></span>

## <a name="delete-a-device-configuration"></a><span data-ttu-id="215c7-214">Odstranění konfigurace zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-214">Delete a device configuration</span></span>
<span data-ttu-id="215c7-215">Pomocí následujícího postupu můžete odstranit z Snapshot Manager zařízení StorSimple jednotlivé konfigurace zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="215c7-215">Use the following procedure to delete an individual StorSimple device configuration from StorSimple Snapshot Manager.</span></span>

#### <a name="to-delete-a-device-configuration"></a><span data-ttu-id="215c7-216">Chcete-li odstranit konfigurace zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-216">To delete a device configuration</span></span>
1. <span data-ttu-id="215c7-217">Klikněte na ikonu plochy spusťte StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="215c7-217">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="215c7-218">V **oboru** podokně klikněte na tlačítko **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="215c7-218">In the **Scope** pane, click **Devices**.</span></span> 
3. <span data-ttu-id="215c7-219">V **výsledky** podokně klikněte pravým tlačítkem na název zařízení a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="215c7-219">In the **Results** pane, right-click the name of the device, and then click **Delete**.</span></span> 
4. <span data-ttu-id="215c7-220">Zobrazí se následující zpráva.</span><span class="sxs-lookup"><span data-stu-id="215c7-220">The following message appears.</span></span> <span data-ttu-id="215c7-221">Klikněte na tlačítko **Ano** odstranit konfiguraci nebo kliknutím na tlačítko **ne** následované.</span><span class="sxs-lookup"><span data-stu-id="215c7-221">Click **Yes** to delete the configuration or click **No** to cancel the deletion.</span></span>
   
    ![Odstranit konfigurace zařízení](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a><span data-ttu-id="215c7-223">Změnit heslo k zařízení s vypršenou platností</span><span class="sxs-lookup"><span data-stu-id="215c7-223">Change an expired device password</span></span>
<span data-ttu-id="215c7-224">Musíte zadat heslo k ověření s StorSimple Snapshot Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="215c7-224">You must enter a password to authenticate a StorSimple device with StorSimple Snapshot Manager.</span></span> <span data-ttu-id="215c7-225">Konfigurujete toto heslo použít rozhraní Windows PowerShell k nastavení zařízení.</span><span class="sxs-lookup"><span data-stu-id="215c7-225">You configure this password when you use the Windows PowerShell interface to set up the device.</span></span> <span data-ttu-id="215c7-226">Však můžete vypršení platnosti hesla.</span><span class="sxs-lookup"><span data-stu-id="215c7-226">However, the password can expire.</span></span> <span data-ttu-id="215c7-227">Pokud k tomu dojde, můžete změnit heslo portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="215c7-227">If this happens, you can use the Azure classic portal to change the password.</span></span> <span data-ttu-id="215c7-228">Pak, protože zařízení byl nakonfigurován v Snapshot Manager zařízení StorSimple, než vyprší platnost hesla, je třeba znovu ověřit zařízení ve Snapshot Manageru zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="215c7-228">Then, because the device was configured in StorSimple Snapshot Manager before the password expired, you must re-authenticate the device in StorSimple Snapshot Manager.</span></span>

#### <a name="to-change-the-expired-password"></a><span data-ttu-id="215c7-229">Chcete-li změnit vypršela platnost hesla</span><span class="sxs-lookup"><span data-stu-id="215c7-229">To change the expired password</span></span>
1. <span data-ttu-id="215c7-230">Na portálu Azure classic spusťte službu StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="215c7-230">In the Azure classic portal, start the StorSimple Manager service.</span></span>
2. <span data-ttu-id="215c7-231">Klikněte na tlačítko **zařízení** > **konfigurace** pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="215c7-231">Click **Devices** > **Configure** for the device.</span></span>
3. <span data-ttu-id="215c7-232">Přejděte do části StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="215c7-232">Scroll down to the StorSimple Snapshot Manager section.</span></span> <span data-ttu-id="215c7-233">Zadejte heslo, které je 14 až 15 znaků.</span><span class="sxs-lookup"><span data-stu-id="215c7-233">Enter a password that is 14-15 characters.</span></span> <span data-ttu-id="215c7-234">Ujistěte se, zda heslo obsahuje směs velká písmena, malá písmena, číselné a speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="215c7-234">Make sure that the password contains a mix of uppercase, lowercase, numeric, and special characters.</span></span>
4. <span data-ttu-id="215c7-235">Znovu zadejte heslo a potvrďte ho.</span><span class="sxs-lookup"><span data-stu-id="215c7-235">Re-enter the password to confirm it.</span></span>
5. <span data-ttu-id="215c7-236">V dolní části stránky klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="215c7-236">Click **Save** at the bottom of the page.</span></span>

#### <a name="to-re-authenticate-the-device"></a><span data-ttu-id="215c7-237">Opakované ověření zařízení</span><span class="sxs-lookup"><span data-stu-id="215c7-237">To re-authenticate the device</span></span>
1. <span data-ttu-id="215c7-238">Spusťte Snapshot Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="215c7-238">Start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="215c7-239">V **oboru** podokně klikněte na tlačítko **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="215c7-239">In the **Scope** pane, click **Devices**.</span></span> <span data-ttu-id="215c7-240">Seznam zařízení, nakonfigurovaného se zobrazí v **výsledky** podokně.</span><span class="sxs-lookup"><span data-stu-id="215c7-240">A list of configured devices appears in the **Results** pane.</span></span>
3. <span data-ttu-id="215c7-241">Vyberte zařízení, klikněte pravým tlačítkem a pak klikněte na tlačítko **ověřit**.</span><span class="sxs-lookup"><span data-stu-id="215c7-241">Select the device, right-click, and then click **Authenticate**.</span></span>
4. <span data-ttu-id="215c7-242">V **ověřit** okno, zadejte nové heslo.</span><span class="sxs-lookup"><span data-stu-id="215c7-242">In the **Authenticate** window, enter the new password.</span></span>
5. <span data-ttu-id="215c7-243">Vyberte zařízení, klikněte pravým tlačítkem a vyberte **aktualizace zařízení**.</span><span class="sxs-lookup"><span data-stu-id="215c7-243">Select the device, right-click, and select **Refresh device**.</span></span> <span data-ttu-id="215c7-244">Provádí synchronizaci zařízení s StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="215c7-244">This synchronizes the device with StorSimple Snapshot Manager.</span></span>

## <a name="replace-a-failed-device"></a><span data-ttu-id="215c7-245">Nahraďte zařízení se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="215c7-245">Replace a failed device</span></span>
<span data-ttu-id="215c7-246">Pokud zařízení StorSimple selže a je nahrazena zařízení úsporném režimu (převzetí služeb při selhání), použijte následující postup k připojení k nové zařízení a zobrazení přidružených záloh.</span><span class="sxs-lookup"><span data-stu-id="215c7-246">If a StorSimple device fails and is replaced by a standby (failover) device, use the following steps to connect to the new device and view the associated backups.</span></span>

#### <a name="to-connect-to-a-new-device-after-failover"></a><span data-ttu-id="215c7-247">Pro připojení k nové zařízení po převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="215c7-247">To connect to a new device after failover</span></span>
1. <span data-ttu-id="215c7-248">Překonfigurujte iSCSI připojení k nové zařízení.</span><span class="sxs-lookup"><span data-stu-id="215c7-248">Reconfigure the iSCSI connection to the new device.</span></span> <span data-ttu-id="215c7-249">Pokyny najdete v tématu "krok 7: připojení, inicializace a formát svazek" v [nasazení místního zařízení StorSimple](storsimple-8000-deployment-walkthrough-u2.md).</span><span class="sxs-lookup"><span data-stu-id="215c7-249">For instructions, go to "Step 7: Mount, initialize, and format a volume" in [Deploy your on-premises StorSimple device](storsimple-8000-deployment-walkthrough-u2.md).</span></span>

> [!NOTE]
> <span data-ttu-id="215c7-250">Pokud nové zařízení StorSimple má stejnou IP adresu jako ten starý, může být schopni připojit původní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="215c7-250">If the new StorSimple device has the same IP address as the old one, you might be able to connect the old configuration.</span></span>


1. <span data-ttu-id="215c7-251">Zastavte službu Microsoft StorSimple správy:</span><span class="sxs-lookup"><span data-stu-id="215c7-251">Stop the Microsoft StorSimple Management Service:</span></span>
   
   1. <span data-ttu-id="215c7-252">Spusťte správce serveru.</span><span class="sxs-lookup"><span data-stu-id="215c7-252">Start Server Manager.</span></span>
   2. <span data-ttu-id="215c7-253">Na řídicím panelu Správce serveru na **nástroje** nabídce vyberte možnost **služby**.</span><span class="sxs-lookup"><span data-stu-id="215c7-253">On the Server Manager Dashboard, on the **Tools** menu, select **Services**.</span></span>
   3. <span data-ttu-id="215c7-254">Na **služby** vyberte **služba správy zařízení StorSimple Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="215c7-254">On the **Services** window, select the **Microsoft StorSimple Management Service**.</span></span>
   4. <span data-ttu-id="215c7-255">V pravém podokně v části **služba správy zařízení StorSimple Microsoft**, klikněte na tlačítko **zastavit službu**.</span><span class="sxs-lookup"><span data-stu-id="215c7-255">In the right pane, under **Microsoft StorSimple Management Service**, click **Stop the service**.</span></span>
2. <span data-ttu-id="215c7-256">Odeberte informace o konfiguraci související s původním zařízení:</span><span class="sxs-lookup"><span data-stu-id="215c7-256">Remove the configuration information related to the old device:</span></span>
   
   1. <span data-ttu-id="215c7-257">V Průzkumníku souborů přejděte do C:\ProgramData\Microsoft\StorSimple\BACatalog.</span><span class="sxs-lookup"><span data-stu-id="215c7-257">In File Explorer, browse to C:\ProgramData\Microsoft\StorSimple\BACatalog.</span></span>
   2. <span data-ttu-id="215c7-258">Odstraňte soubory ve složce BACatalog.</span><span class="sxs-lookup"><span data-stu-id="215c7-258">Delete the files in the BACatalog folder.</span></span>
3. <span data-ttu-id="215c7-259">Restartujte službu Microsoft StorSimple Management:</span><span class="sxs-lookup"><span data-stu-id="215c7-259">Restart the Microsoft StorSimple Management Service:</span></span>
   
   1. <span data-ttu-id="215c7-260">Na řídicím panelu Správce serveru na **nástroje** nabídce vyberte možnost **služby**.</span><span class="sxs-lookup"><span data-stu-id="215c7-260">On the Server Manager Dashboard, on the **Tools** menu, select **Services**.</span></span>
   2. <span data-ttu-id="215c7-261">Na **služby** vyberte **služba správy zařízení StorSimple Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="215c7-261">On the **Services** window, select the **Microsoft StorSimple Management Service**.</span></span>
   3. <span data-ttu-id="215c7-262">V pravém podokně v části **služba správy zařízení StorSimple Microsoft**, klikněte na tlačítko **restartujte službu**.</span><span class="sxs-lookup"><span data-stu-id="215c7-262">In the right pane, under **Microsoft StorSimple Management Service**, click **Restart the service**.</span></span>
4. <span data-ttu-id="215c7-263">Spusťte Snapshot Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="215c7-263">Start StorSimple Snapshot Manager.</span></span>
5. <span data-ttu-id="215c7-264">Pokud chcete konfigurovat nové zařízení StorSimple, proveďte kroky v kroku 2: připojení zařízení StorSimple v [nasazení zařízení StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="215c7-264">To configure the new StorSimple device, complete the steps in Step 2: Connect a StorSimple device in [Deploy StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).</span></span>
6. <span data-ttu-id="215c7-265">Klikněte pravým tlačítkem na uzel na nejvyšší úrovni v **oboru** panelu (Snapshot Manager zařízení StorSimple v příkladu) a pak klikněte na tlačítko **přepnout importů zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="215c7-265">Right-click the top-level node in the **Scope** pane (StorSimple Snapshot Manager in the example), and then click **Toggle Imports Display**.</span></span> 
7. <span data-ttu-id="215c7-266">Zpráva se zobrazí, když se zobrazí v zařízení StorSimple Snapshot Manager skupiny importované svazku a zálohování.</span><span class="sxs-lookup"><span data-stu-id="215c7-266">A message appears when the imported volume groups and backups are visible in StorSimple Snapshot Manager.</span></span> <span data-ttu-id="215c7-267">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="215c7-267">Click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="215c7-268">Další kroky</span><span class="sxs-lookup"><span data-stu-id="215c7-268">Next steps</span></span>
* <span data-ttu-id="215c7-269">Zjistěte, jak [použít ke správě vašeho řešení StorSimple Snapshot Manager zařízení StorSimple](storsimple-snapshot-manager-admin.md).</span><span class="sxs-lookup"><span data-stu-id="215c7-269">Learn how to [use StorSimple Snapshot Manager to administer your StorSimple solution](storsimple-snapshot-manager-admin.md).</span></span>
* <span data-ttu-id="215c7-270">Zjistěte, jak [pomocí StorSimple Snapshot Manager můžete zobrazit a spravovat svazky](storsimple-snapshot-manager-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="215c7-270">Learn how to [use StorSimple Snapshot Manager to view and manage volumes](storsimple-snapshot-manager-manage-volumes.md).</span></span>

