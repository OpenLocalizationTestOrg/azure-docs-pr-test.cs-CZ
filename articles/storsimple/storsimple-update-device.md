---
title: "Aktualizace zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak používat funkci aktualizace zařízení StorSimple k instalaci aktualizací režimu regular a údržbu a opravy hotfix."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 786059f5-2a38-4105-941d-0860ce4ac515
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/18/2016
ms.author: v-sharos
ms.openlocfilehash: 8490110942741b049b6d44ac93697303cef40e8a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="update-your-storsimple-8000-series-device"></a><span data-ttu-id="37f05-103">Aktualizace zařízení StorSimple řady 8000</span><span class="sxs-lookup"><span data-stu-id="37f05-103">Update your StorSimple 8000 Series device</span></span>
## <a name="overview"></a><span data-ttu-id="37f05-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="37f05-104">Overview</span></span>
<span data-ttu-id="37f05-105">Funkce aktualizace zařízení StorSimple umožňují snadno zachovat aktuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="37f05-105">The StorSimple updates features allow you to easily keep your StorSimple device up-to-date.</span></span> <span data-ttu-id="37f05-106">V závislosti na daný typ aktualizace můžete použít aktualizace na zařízení prostřednictvím portálu Azure classic nebo prostřednictvím rozhraní Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="37f05-106">Depending on the update type, you can apply updates to the device via the Azure classic portal or via the Windows PowerShell interface.</span></span> <span data-ttu-id="37f05-107">Tento kurz popisuje typy aktualizace a postup instalace, každý z nich.</span><span class="sxs-lookup"><span data-stu-id="37f05-107">This tutorial describes the update types and how to install each of them.</span></span>

<span data-ttu-id="37f05-108">Můžete použít dva typy aktualizace zařízení:</span><span class="sxs-lookup"><span data-stu-id="37f05-108">You can apply two types of device updates:</span></span> 

* <span data-ttu-id="37f05-109">Regulární (nebo v normálním režimu) aktualizací</span><span class="sxs-lookup"><span data-stu-id="37f05-109">Regular (or Normal mode) updates</span></span>
* <span data-ttu-id="37f05-110">Aktualizace režimu údržby</span><span class="sxs-lookup"><span data-stu-id="37f05-110">Maintenance mode updates</span></span>

<span data-ttu-id="37f05-111">Můžete nainstalovat pravidelné aktualizace prostřednictvím portálu Azure classic nebo prostředí Windows PowerShell; však musíte použít prostředí Windows PowerShell k instalaci aktualizací režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="37f05-111">You can install regular updates via the Azure classic portal or Windows PowerShell; however, you must use Windows PowerShell to install Maintenance mode updates.</span></span> 

<span data-ttu-id="37f05-112">Každý typ aktualizace je popsána samostatně, níže.</span><span class="sxs-lookup"><span data-stu-id="37f05-112">Each update type is described separately, below.</span></span>

### <a name="regular-updates"></a><span data-ttu-id="37f05-113">Pravidelné aktualizace</span><span class="sxs-lookup"><span data-stu-id="37f05-113">Regular updates</span></span>
<span data-ttu-id="37f05-114">Pravidelné aktualizace jsou omezovaly aktualizace, které je možné nainstalovat, když je zařízení v normálním režimu.</span><span class="sxs-lookup"><span data-stu-id="37f05-114">Regular updates are non-disruptive updates that can be installed when the device is in Normal mode.</span></span> <span data-ttu-id="37f05-115">Tyto aktualizace se použijí prostřednictvím webu Microsoft Update pro každý řadič zařízení.</span><span class="sxs-lookup"><span data-stu-id="37f05-115">These updates are applied through the Microsoft Update website to each device controller.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="37f05-116">Během procesu aktualizace může dojít k selhání řadiče.</span><span class="sxs-lookup"><span data-stu-id="37f05-116">A controller failover may occur during the update process.</span></span> <span data-ttu-id="37f05-117">Ale to nebude mít vliv na dostupnost systému nebo operace.</span><span class="sxs-lookup"><span data-stu-id="37f05-117">However, this will not affect system availability or operation.</span></span>
> 
> 

* <span data-ttu-id="37f05-118">Podrobnosti o tom, jak nainstalovat pravidelné aktualizace prostřednictvím portálu Azure classic najdete v tématu [nainstalovat pravidelné aktualizace prostřednictvím portálu Azure classic](#install-regular-updates-via-the-azure-classic-portal).</span><span class="sxs-lookup"><span data-stu-id="37f05-118">For details on how to install regular updates via the Azure classic portal, see [Install regular updates via the Azure classic portal](#install-regular-updates-via-the-azure-classic-portal).</span></span>
* <span data-ttu-id="37f05-119">Můžete také nainstalovat pravidelné aktualizace pomocí prostředí Windows PowerShell pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="37f05-119">You can also install regular updates via Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="37f05-120">Podrobnosti najdete v tématu [instalace pravidelné aktualizace prostřednictvím Windows Powershellu pro StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="37f05-120">For details, see [Install regular updates via Windows PowerShell for StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).</span></span>

### <a name="maintenance-mode-updates"></a><span data-ttu-id="37f05-121">Aktualizace režimu údržby</span><span class="sxs-lookup"><span data-stu-id="37f05-121">Maintenance mode updates</span></span>
<span data-ttu-id="37f05-122">Aktualizace režimu údržby jsou rušivý aktualizace, jako je například upgrade firmwaru disku.</span><span class="sxs-lookup"><span data-stu-id="37f05-122">Maintenance Mode updates are disruptive updates such as disk firmware upgrades.</span></span> <span data-ttu-id="37f05-123">Tyto aktualizace vyžaduje zařízení uvést do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="37f05-123">These updates require the device to be put into Maintenance mode.</span></span> <span data-ttu-id="37f05-124">Podrobnosti najdete v tématu [krok 2: Zadejte údržby režimu](#step2).</span><span class="sxs-lookup"><span data-stu-id="37f05-124">For details, see [Step 2: Enter Maintenance mode](#step2).</span></span> <span data-ttu-id="37f05-125">Portál Azure classic nelze použít k instalaci aktualizací režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="37f05-125">You cannot use the Azure classic portal to install Maintenance mode updates.</span></span> <span data-ttu-id="37f05-126">Místo toho musíte použít Windows PowerShell pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="37f05-126">Instead, you must use Windows PowerShell for StorSimple.</span></span> 

<span data-ttu-id="37f05-127">Podrobnosti o tom, jak nainstalovat aktualizace režimu údržby najdete v tématu [aktualizace režimu údržby nainstalovat prostřednictvím Windows Powershellu pro StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="37f05-127">For details on how to install Maintenance mode updates, see [Install Maintenance mode updates via Windows PowerShell for StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="37f05-128">Aktualizace režimu údržby, je nutné použít samostatně na každém řadiči.</span><span class="sxs-lookup"><span data-stu-id="37f05-128">Maintenance mode updates must be applied separately to each controller.</span></span> 
> 
> 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a><span data-ttu-id="37f05-129">Nainstalujte pravidelné aktualizace prostřednictvím portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="37f05-129">Install regular updates via the Azure classic portal</span></span>
<span data-ttu-id="37f05-130">Portál Azure classic můžete použít aktualizace na zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="37f05-130">You can use the Azure classic portal to apply updates to your StorSimple device.</span></span>

[!INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a><span data-ttu-id="37f05-131">Instalace pravidelné aktualizace prostřednictvím Windows Powershellu pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="37f05-131">Install regular updates via Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="37f05-132">Alternativně můžete použít prostředí Windows PowerShell pro StorSimple na aktualizace regular (normální režim).</span><span class="sxs-lookup"><span data-stu-id="37f05-132">Alternatively, you can use Windows PowerShell for StorSimple to apply regular (Normal mode) updates.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="37f05-133">I když můžete nainstalovat pravidelné aktualizace pomocí Windows Powershellu pro StorSimple, důrazně doporučujeme nainstalovat pravidelné aktualizace prostřednictvím portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="37f05-133">Although you can install regular updates using Windows PowerShell for StorSimple, we strongly recommend that you install regular updates through the Azure classic portal.</span></span> <span data-ttu-id="37f05-134">Počínaje Update 1, bude předběžné kontroly provést před instalací aktualizací z portálu.</span><span class="sxs-lookup"><span data-stu-id="37f05-134">Beginning with Update 1, pre-checks will be performed prior to installing updates from the portal.</span></span> <span data-ttu-id="37f05-135">Tyto předběžné kontroly se mají prioritu před selhání a zajistěte hladší prostředí.</span><span class="sxs-lookup"><span data-stu-id="37f05-135">These pre-checks will preempt failures and ensure a smoother experience.</span></span> 
> 
> 

[!INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a><span data-ttu-id="37f05-136">Instalovat aktualizace režimu údržby pomocí prostředí Windows PowerShell pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="37f05-136">Install Maintenance mode updates via Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="37f05-137">Pomocí Windows Powershellu pro StorSimple na aktualizace režimu údržby pro zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="37f05-137">You use Windows PowerShell for StorSimple to apply Maintenance mode updates to your StorSimple device.</span></span> <span data-ttu-id="37f05-138">V tomto režimu jsou pozastavena všechny vstupně-výstupní požadavky.</span><span class="sxs-lookup"><span data-stu-id="37f05-138">All I/O requests are paused in this mode.</span></span> <span data-ttu-id="37f05-139">Také se zastaví službám, jako je paměť s náhodným přístupem stálé (paměti NVRAM) nebo službu clusteringu.</span><span class="sxs-lookup"><span data-stu-id="37f05-139">Services such as non-volatile random access memory (NVRAM) or the clustering service are also stopped.</span></span> <span data-ttu-id="37f05-140">Oba řadiče se restartují, když zadáte nebo ukončit tento režim.</span><span class="sxs-lookup"><span data-stu-id="37f05-140">Both controllers are rebooted when you enter or exit this mode.</span></span> <span data-ttu-id="37f05-141">Při ukončení tohoto režimu se všechny služby bude pokračovat a musí být v pořádku.</span><span class="sxs-lookup"><span data-stu-id="37f05-141">When you exit this mode, all the services will resume and should be healthy.</span></span> <span data-ttu-id="37f05-142">(To může trvat několik minut.)</span><span class="sxs-lookup"><span data-stu-id="37f05-142">(This may take a few minutes.)</span></span>

<span data-ttu-id="37f05-143">Pokud potřebujete aktualizace režimu údržby, zobrazí se výstraha prostřednictvím portálu Azure classic, která obsahuje aktualizace, které je třeba nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="37f05-143">If you need to apply Maintenance mode updates, you will receive an alert through the Azure classic portal that you have updates that must be installed.</span></span> <span data-ttu-id="37f05-144">Tato výstraha bude obsahovat pokyny pro instalaci aktualizací pomocí Windows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="37f05-144">This alert will include instructions for using Windows PowerShell for StorSimple to install the updates.</span></span> <span data-ttu-id="37f05-145">Po aktualizaci zařízení, použijte stejný postup a změňte zařízení do režimu regulární.</span><span class="sxs-lookup"><span data-stu-id="37f05-145">After you update your device, use the same procedure to change the device to Regular mode.</span></span> <span data-ttu-id="37f05-146">Podrobné pokyny najdete v tématu [krok 4: režim údržby ukončení](#step4).</span><span class="sxs-lookup"><span data-stu-id="37f05-146">For step-by-step instructions, see [Step 4: Exit Maintenance mode](#step4).</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="37f05-147">Před přechodem do režimu údržby, ověřte, zda jsou oba řadiče zařízení v pořádku kontrolou **stavu hardwaru** na **údržby** na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="37f05-147">Before entering Maintenance mode, verify that both device controllers are healthy by checking the **Hardware Status** on the **Maintenance** page in the Azure classic portal.</span></span> <span data-ttu-id="37f05-148">Pokud je řadič není v pořádku, požádejte o další kroky Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="37f05-148">If the controller is not healthy, contact Microsoft Support for the next steps.</span></span> <span data-ttu-id="37f05-149">Další informace najdete v tématu kontaktujte podporu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="37f05-149">For more information, go to Contact Microsoft Support.</span></span> 
> * <span data-ttu-id="37f05-150">Pokud jste v režimu údržby, musíte nejprve použít aktualizaci na jednom řadiči a poté na jiný řadič.</span><span class="sxs-lookup"><span data-stu-id="37f05-150">When you are in Maintenance mode, you need to apply the update first on one controller and then on the other controller.</span></span>
> 
> 

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a><span data-ttu-id="37f05-151">Krok 1: Připojení ke konzole sériového portu<a name="step1"></span><span class="sxs-lookup"><span data-stu-id="37f05-151">Step 1: Connect to the serial console <a name="step1"></span></span>
<span data-ttu-id="37f05-152">Nejprve pomocí aplikace, jako je například PuTTY přístup ke konzole sériového portu.</span><span class="sxs-lookup"><span data-stu-id="37f05-152">First, use an application such as PuTTY to access the serial console.</span></span> <span data-ttu-id="37f05-153">Následující postup vysvětluje, jak použití klienta PuTTY k připojení ke konzole sériového portu.</span><span class="sxs-lookup"><span data-stu-id="37f05-153">The following procedure explains how to use PuTTY to connect to the serial console.</span></span>

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a><span data-ttu-id="37f05-154">Krok 2: Zadejte režimu údržby<a name="step2"></span><span class="sxs-lookup"><span data-stu-id="37f05-154">Step 2: Enter Maintenance mode <a name="step2"></span></span>
<span data-ttu-id="37f05-155">Po připojení ke konzole, zjistěte, jestli jsou aktualizace nainstalovat, a zadejte režim údržby k instalaci.</span><span class="sxs-lookup"><span data-stu-id="37f05-155">After you connect to the console, determine whether there are updates to install, and enter Maintenance mode to install them.</span></span>

[!INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a><span data-ttu-id="37f05-156">Krok 3: Instalace aktualizace<a name="step3"></span><span class="sxs-lookup"><span data-stu-id="37f05-156">Step 3: Install your updates <a name="step3"></span></span>
<span data-ttu-id="37f05-157">Dále nainstalujte vaše aktualizace.</span><span class="sxs-lookup"><span data-stu-id="37f05-157">Next, install your updates.</span></span>

[!INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]

### <a name="step-4-exit-maintenance-mode-a-namestep4"></a><span data-ttu-id="37f05-158">Krok 4: Režim údržby ukončení<a name="step4"></span><span class="sxs-lookup"><span data-stu-id="37f05-158">Step 4: Exit Maintenance mode <a name="step4"></span></span>
<span data-ttu-id="37f05-159">Nakonec ukončení režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="37f05-159">Finally, exit Maintenance mode.</span></span>

[!INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="37f05-160">Instalaci oprav hotfix prostřednictvím Windows Powershellu pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="37f05-160">Install hotfixes via Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="37f05-161">Na rozdíl od aktualizace pro Microsoft Azure StorSimple jsou nainstalované opravy hotfix ze sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="37f05-161">Unlike updates for Microsoft Azure StorSimple, hotfixes are installed from a shared folder.</span></span> <span data-ttu-id="37f05-162">Jako s aktualizacemi, existují dva typy opravy hotfix:</span><span class="sxs-lookup"><span data-stu-id="37f05-162">As with updates, there are two types of hotfixes:</span></span> 

* <span data-ttu-id="37f05-163">Regulární opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="37f05-163">Regular hotfixes</span></span> 
* <span data-ttu-id="37f05-164">Opravy hotfix režimu údržby</span><span class="sxs-lookup"><span data-stu-id="37f05-164">Maintenance mode hotfixes</span></span>  

<span data-ttu-id="37f05-165">Následující postupy popisují, jak pomocí prostředí Windows PowerShell pro StorSimple nainstalovat regular a opravy hotfix režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="37f05-165">The following procedures explain how to use Windows PowerShell for StorSimple to install regular and Maintenance mode hotfixes.</span></span>

[!INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[!INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a><span data-ttu-id="37f05-166">Co se stane aktualizace, pokud provádíte obnovení továrního nastavení zařízení?</span><span class="sxs-lookup"><span data-stu-id="37f05-166">What happens to updates if you perform a factory reset of the device?</span></span>
<span data-ttu-id="37f05-167">Pokud je zařízení obnovit tovární nastavení, všechny aktualizace budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="37f05-167">If a device is reset to factory settings, then all the updates are lost.</span></span> <span data-ttu-id="37f05-168">Po obnovení továrního nastavení zařízení je registrované a konfiguraci, musíte ručně nainstalovat aktualizace prostřednictvím portálu Azure classic nebo prostředí Windows PowerShell pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="37f05-168">After the factory-reset device is registered and configured, you will need to manually install updates through the Azure classic portal and/or Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="37f05-169">Další informace o obnovení továrního nastavení najdete v tématu [zařízení resetovat výchozí tovární nastavení](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span><span class="sxs-lookup"><span data-stu-id="37f05-169">For more information about factory reset, see [Reset the device to factory default settings](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span></span>

## <a name="next-steps"></a><span data-ttu-id="37f05-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="37f05-170">Next steps</span></span>
* <span data-ttu-id="37f05-171">Další informace o [pomocí Windows Powershellu pro StorSimple ke správě zařízení StorSimple](storsimple-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="37f05-171">Learn more about [using Windows PowerShell for StorSimple to administer your StorSimple device](storsimple-windows-powershell-administration.md).</span></span>
* <span data-ttu-id="37f05-172">Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="37f05-172">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

