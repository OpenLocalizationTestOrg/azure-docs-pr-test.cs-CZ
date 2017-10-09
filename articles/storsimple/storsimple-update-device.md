---
title: "aaaUpdate zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak aktualizovat toouse hello StorSimple funkce tooinstall regulární údržby režim aktualizace a opravy hotfix."
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
ms.openlocfilehash: 05acf05c8fc89bbb4343f67ad103235bbe3dba0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-your-storsimple-8000-series-device"></a><span data-ttu-id="9c803-103">Aktualizace zařízení StorSimple řady 8000</span><span class="sxs-lookup"><span data-stu-id="9c803-103">Update your StorSimple 8000 Series device</span></span>
## <a name="overview"></a><span data-ttu-id="9c803-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="9c803-104">Overview</span></span>
<span data-ttu-id="9c803-105">Hello StorSimple aktualizace funkcí umožňují tooeasily aktualizaci zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="9c803-105">hello StorSimple updates features allow you tooeasily keep your StorSimple device up-to-date.</span></span> <span data-ttu-id="9c803-106">V závislosti na typu hello aktualizace můžete použít aktualizace toohello zařízení prostřednictvím hello portál Azure classic nebo prostřednictvím rozhraní Windows PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="9c803-106">Depending on hello update type, you can apply updates toohello device via hello Azure classic portal or via hello Windows PowerShell interface.</span></span> <span data-ttu-id="9c803-107">Tento kurz popisuje typy aktualizací hello a jak tooinstall každý z nich.</span><span class="sxs-lookup"><span data-stu-id="9c803-107">This tutorial describes hello update types and how tooinstall each of them.</span></span>

<span data-ttu-id="9c803-108">Můžete použít dva typy aktualizace zařízení:</span><span class="sxs-lookup"><span data-stu-id="9c803-108">You can apply two types of device updates:</span></span> 

* <span data-ttu-id="9c803-109">Regulární (nebo v normálním režimu) aktualizací</span><span class="sxs-lookup"><span data-stu-id="9c803-109">Regular (or Normal mode) updates</span></span>
* <span data-ttu-id="9c803-110">Aktualizace režimu údržby</span><span class="sxs-lookup"><span data-stu-id="9c803-110">Maintenance mode updates</span></span>

<span data-ttu-id="9c803-111">Můžete nainstalovat pravidelné aktualizace prostřednictvím hello portál Azure classic nebo prostředí Windows PowerShell; však musíte použít prostředí Windows PowerShell tooinstall údržby režim aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9c803-111">You can install regular updates via hello Azure classic portal or Windows PowerShell; however, you must use Windows PowerShell tooinstall Maintenance mode updates.</span></span> 

<span data-ttu-id="9c803-112">Každý typ aktualizace je popsána samostatně, níže.</span><span class="sxs-lookup"><span data-stu-id="9c803-112">Each update type is described separately, below.</span></span>

### <a name="regular-updates"></a><span data-ttu-id="9c803-113">Pravidelné aktualizace</span><span class="sxs-lookup"><span data-stu-id="9c803-113">Regular updates</span></span>
<span data-ttu-id="9c803-114">Pravidelné aktualizace jsou omezovaly aktualizace, které lze nainstalovat, když je zařízení hello v normálním režimu.</span><span class="sxs-lookup"><span data-stu-id="9c803-114">Regular updates are non-disruptive updates that can be installed when hello device is in Normal mode.</span></span> <span data-ttu-id="9c803-115">Tyto aktualizace se uplatňuje skrze hello Microsoft Update webu tooeach zařízení řadiče.</span><span class="sxs-lookup"><span data-stu-id="9c803-115">These updates are applied through hello Microsoft Update website tooeach device controller.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="9c803-116">Během procesu aktualizace hello může dojít k selhání řadiče.</span><span class="sxs-lookup"><span data-stu-id="9c803-116">A controller failover may occur during hello update process.</span></span> <span data-ttu-id="9c803-117">Ale to nebude mít vliv na dostupnost systému nebo operace.</span><span class="sxs-lookup"><span data-stu-id="9c803-117">However, this will not affect system availability or operation.</span></span>
> 
> 

* <span data-ttu-id="9c803-118">Podrobnosti o tom, jak tooinstall regular aktualizací prostřednictvím hello portál Azure classic, najdete v části [nainstalovat pravidelné aktualizace prostřednictvím portálu Azure classic hello](#install-regular-updates-via-the-azure-classic-portal).</span><span class="sxs-lookup"><span data-stu-id="9c803-118">For details on how tooinstall regular updates via hello Azure classic portal, see [Install regular updates via hello Azure classic portal](#install-regular-updates-via-the-azure-classic-portal).</span></span>
* <span data-ttu-id="9c803-119">Můžete také nainstalovat pravidelné aktualizace pomocí prostředí Windows PowerShell pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="9c803-119">You can also install regular updates via Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="9c803-120">Podrobnosti najdete v tématu [instalace pravidelné aktualizace prostřednictvím Windows Powershellu pro StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="9c803-120">For details, see [Install regular updates via Windows PowerShell for StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).</span></span>

### <a name="maintenance-mode-updates"></a><span data-ttu-id="9c803-121">Aktualizace režimu údržby</span><span class="sxs-lookup"><span data-stu-id="9c803-121">Maintenance mode updates</span></span>
<span data-ttu-id="9c803-122">Aktualizace režimu údržby jsou rušivý aktualizace, jako je například upgrade firmwaru disku.</span><span class="sxs-lookup"><span data-stu-id="9c803-122">Maintenance Mode updates are disruptive updates such as disk firmware upgrades.</span></span> <span data-ttu-id="9c803-123">Tyto aktualizace vyžadují toobe hello zařízení uvést do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="9c803-123">These updates require hello device toobe put into Maintenance mode.</span></span> <span data-ttu-id="9c803-124">Podrobnosti najdete v tématu [krok 2: Zadejte údržby režimu](#step2).</span><span class="sxs-lookup"><span data-stu-id="9c803-124">For details, see [Step 2: Enter Maintenance mode](#step2).</span></span> <span data-ttu-id="9c803-125">Hello Azure classic portálu tooinstall údržby režim aktualizace nejde použít.</span><span class="sxs-lookup"><span data-stu-id="9c803-125">You cannot use hello Azure classic portal tooinstall Maintenance mode updates.</span></span> <span data-ttu-id="9c803-126">Místo toho musíte použít Windows PowerShell pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="9c803-126">Instead, you must use Windows PowerShell for StorSimple.</span></span> 

<span data-ttu-id="9c803-127">Podrobnosti o způsobu režimu údržby tooinstall aktualizace, najdete v části [aktualizace režimu údržby nainstalovat prostřednictvím Windows Powershellu pro StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="9c803-127">For details on how tooinstall Maintenance mode updates, see [Install Maintenance mode updates via Windows PowerShell for StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9c803-128">Režim údržby, které aktualizace musí být použity samostatně tooeach řadiče.</span><span class="sxs-lookup"><span data-stu-id="9c803-128">Maintenance mode updates must be applied separately tooeach controller.</span></span> 
> 
> 

## <a name="install-regular-updates-via-hello-azure-classic-portal"></a><span data-ttu-id="9c803-129">Nainstalujte pravidelné aktualizace prostřednictvím hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="9c803-129">Install regular updates via hello Azure classic portal</span></span>
<span data-ttu-id="9c803-130">Můžete použít zařízení StorSimple tooyour aktualizace Azure classic portálu tooapply hello.</span><span class="sxs-lookup"><span data-stu-id="9c803-130">You can use hello Azure classic portal tooapply updates tooyour StorSimple device.</span></span>

[!INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a><span data-ttu-id="9c803-131">Instalace pravidelné aktualizace prostřednictvím Windows Powershellu pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="9c803-131">Install regular updates via Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="9c803-132">Alternativně můžete použít prostředí Windows PowerShell pro StorSimple tooapply regular (normální režim) aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9c803-132">Alternatively, you can use Windows PowerShell for StorSimple tooapply regular (Normal mode) updates.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9c803-133">I když můžete nainstalovat pravidelné aktualizace pomocí Windows Powershellu pro StorSimple, důrazně doporučujeme nainstalovat pravidelné aktualizace prostřednictvím hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="9c803-133">Although you can install regular updates using Windows PowerShell for StorSimple, we strongly recommend that you install regular updates through hello Azure classic portal.</span></span> <span data-ttu-id="9c803-134">Počínaje Update 1, bude předběžné kontroly aktualizací provádí předchozí tooinstalling z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="9c803-134">Beginning with Update 1, pre-checks will be performed prior tooinstalling updates from hello portal.</span></span> <span data-ttu-id="9c803-135">Tyto předběžné kontroly se mají prioritu před selhání a zajistěte hladší prostředí.</span><span class="sxs-lookup"><span data-stu-id="9c803-135">These pre-checks will preempt failures and ensure a smoother experience.</span></span> 
> 
> 

[!INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a><span data-ttu-id="9c803-136">Instalovat aktualizace režimu údržby pomocí prostředí Windows PowerShell pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="9c803-136">Install Maintenance mode updates via Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="9c803-137">Pomocí Windows Powershellu pro StorSimple tooapply údržby režim aktualizace tooyour zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="9c803-137">You use Windows PowerShell for StorSimple tooapply Maintenance mode updates tooyour StorSimple device.</span></span> <span data-ttu-id="9c803-138">V tomto režimu jsou pozastavena všechny vstupně-výstupní požadavky.</span><span class="sxs-lookup"><span data-stu-id="9c803-138">All I/O requests are paused in this mode.</span></span> <span data-ttu-id="9c803-139">Také se zastaví službám, jako je paměť s náhodným přístupem stálé (paměti NVRAM) nebo hello Clusterová služba.</span><span class="sxs-lookup"><span data-stu-id="9c803-139">Services such as non-volatile random access memory (NVRAM) or hello clustering service are also stopped.</span></span> <span data-ttu-id="9c803-140">Oba řadiče se restartují, když zadáte nebo ukončit tento režim.</span><span class="sxs-lookup"><span data-stu-id="9c803-140">Both controllers are rebooted when you enter or exit this mode.</span></span> <span data-ttu-id="9c803-141">Při ukončení tohoto režimu se všechny služby hello bude pokračovat a musí být v pořádku.</span><span class="sxs-lookup"><span data-stu-id="9c803-141">When you exit this mode, all hello services will resume and should be healthy.</span></span> <span data-ttu-id="9c803-142">(To může trvat několik minut.)</span><span class="sxs-lookup"><span data-stu-id="9c803-142">(This may take a few minutes.)</span></span>

<span data-ttu-id="9c803-143">Pokud budete potřebovat aktualizace režimu údržby tooapply, obdržíte výstrahu prostřednictvím hello portál Azure classic, abyste měli aktualizace, které musí být nainstalován.</span><span class="sxs-lookup"><span data-stu-id="9c803-143">If you need tooapply Maintenance mode updates, you will receive an alert through hello Azure classic portal that you have updates that must be installed.</span></span> <span data-ttu-id="9c803-144">Tato výstraha bude obsahovat pokyny pro používání prostředí Windows PowerShell pro StorSimple tooinstall hello aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9c803-144">This alert will include instructions for using Windows PowerShell for StorSimple tooinstall hello updates.</span></span> <span data-ttu-id="9c803-145">Po aktualizaci zařízení hello použijte stejný postup toochange hello zařízení tooRegular režimu.</span><span class="sxs-lookup"><span data-stu-id="9c803-145">After you update your device, use hello same procedure toochange hello device tooRegular mode.</span></span> <span data-ttu-id="9c803-146">Podrobné pokyny najdete v tématu [krok 4: režim údržby ukončení](#step4).</span><span class="sxs-lookup"><span data-stu-id="9c803-146">For step-by-step instructions, see [Step 4: Exit Maintenance mode](#step4).</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="9c803-147">Před přechodem do režimu údržby, ověřte, zda jsou oba řadiče zařízení v pořádku kontrolou hello **stavu hardwaru** na hello **údržby** stránku hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="9c803-147">Before entering Maintenance mode, verify that both device controllers are healthy by checking hello **Hardware Status** on hello **Maintenance** page in hello Azure classic portal.</span></span> <span data-ttu-id="9c803-148">Pokud řadič hello není v pořádku, požádejte o další kroky hello Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="9c803-148">If hello controller is not healthy, contact Microsoft Support for hello next steps.</span></span> <span data-ttu-id="9c803-149">Další informace přejděte tooContact Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="9c803-149">For more information, go tooContact Microsoft Support.</span></span> 
> * <span data-ttu-id="9c803-150">Pokud jste v režimu údržby, je nutné tooapply hello aktualizace nejdřív na jednom řadiči a pak na hello jiný řadič.</span><span class="sxs-lookup"><span data-stu-id="9c803-150">When you are in Maintenance mode, you need tooapply hello update first on one controller and then on hello other controller.</span></span>
> 
> 

### <a name="step-1-connect-toohello-serial-console-a-namestep1"></a><span data-ttu-id="9c803-151">Krok 1: Připojte toohello konzoly sériového portu<a name="step1"></span><span class="sxs-lookup"><span data-stu-id="9c803-151">Step 1: Connect toohello serial console <a name="step1"></span></span>
<span data-ttu-id="9c803-152">Nejprve pomocí aplikace, jako je PuTTY tooaccess hello konzoly sériového portu.</span><span class="sxs-lookup"><span data-stu-id="9c803-152">First, use an application such as PuTTY tooaccess hello serial console.</span></span> <span data-ttu-id="9c803-153">Hello následující postup vysvětluje, jak toouse PuTTY tooconnect toohello konzoly sériového portu.</span><span class="sxs-lookup"><span data-stu-id="9c803-153">hello following procedure explains how toouse PuTTY tooconnect toohello serial console.</span></span>

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a><span data-ttu-id="9c803-154">Krok 2: Zadejte režimu údržby<a name="step2"></span><span class="sxs-lookup"><span data-stu-id="9c803-154">Step 2: Enter Maintenance mode <a name="step2"></span></span>
<span data-ttu-id="9c803-155">Po připojení konzoly toohello určit, jestli jsou aktualizace tooinstall a zadejte tooinstall režimu údržby je.</span><span class="sxs-lookup"><span data-stu-id="9c803-155">After you connect toohello console, determine whether there are updates tooinstall, and enter Maintenance mode tooinstall them.</span></span>

[!INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a><span data-ttu-id="9c803-156">Krok 3: Instalace aktualizace<a name="step3"></span><span class="sxs-lookup"><span data-stu-id="9c803-156">Step 3: Install your updates <a name="step3"></span></span>
<span data-ttu-id="9c803-157">Dále nainstalujte vaše aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9c803-157">Next, install your updates.</span></span>

[!INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]

### <a name="step-4-exit-maintenance-mode-a-namestep4"></a><span data-ttu-id="9c803-158">Krok 4: Režim údržby ukončení<a name="step4"></span><span class="sxs-lookup"><span data-stu-id="9c803-158">Step 4: Exit Maintenance mode <a name="step4"></span></span>
<span data-ttu-id="9c803-159">Nakonec ukončení režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="9c803-159">Finally, exit Maintenance mode.</span></span>

[!INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="9c803-160">Instalaci oprav hotfix prostřednictvím Windows Powershellu pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="9c803-160">Install hotfixes via Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="9c803-161">Na rozdíl od aktualizace pro Microsoft Azure StorSimple jsou nainstalované opravy hotfix ze sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="9c803-161">Unlike updates for Microsoft Azure StorSimple, hotfixes are installed from a shared folder.</span></span> <span data-ttu-id="9c803-162">Jako s aktualizacemi, existují dva typy opravy hotfix:</span><span class="sxs-lookup"><span data-stu-id="9c803-162">As with updates, there are two types of hotfixes:</span></span> 

* <span data-ttu-id="9c803-163">Regulární opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="9c803-163">Regular hotfixes</span></span> 
* <span data-ttu-id="9c803-164">Opravy hotfix režimu údržby</span><span class="sxs-lookup"><span data-stu-id="9c803-164">Maintenance mode hotfixes</span></span>  

<span data-ttu-id="9c803-165">Hello následující postupy popisují, jak toouse Windows Powershellu pro StorSimple tooinstall regulární a opravy hotfix režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="9c803-165">hello following procedures explain how toouse Windows PowerShell for StorSimple tooinstall regular and Maintenance mode hotfixes.</span></span>

[!INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[!INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-tooupdates-if-you-perform-a-factory-reset-of-hello-device"></a><span data-ttu-id="9c803-166">Co se stane, že tooupdates Pokud provádíte objekt pro vytváření resetovat hello zařízení?</span><span class="sxs-lookup"><span data-stu-id="9c803-166">What happens tooupdates if you perform a factory reset of hello device?</span></span>
<span data-ttu-id="9c803-167">Pokud se zařízení resetovat toofactory nastavení, všechny hello aktualizace budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="9c803-167">If a device is reset toofactory settings, then all hello updates are lost.</span></span> <span data-ttu-id="9c803-168">Po obnovení továrního nastavení zařízení hello registraci a konfiguraci, je nutné nainstalovat aktualizace toomanually prostřednictvím hello portál Azure classic nebo prostředí Windows PowerShell pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="9c803-168">After hello factory-reset device is registered and configured, you will need toomanually install updates through hello Azure classic portal and/or Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="9c803-169">Další informace o obnovení továrního nastavení najdete v tématu [resetovat hello zařízení toofactory výchozí nastavení](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span><span class="sxs-lookup"><span data-stu-id="9c803-169">For more information about factory reset, see [Reset hello device toofactory default settings](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9c803-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9c803-170">Next steps</span></span>
* <span data-ttu-id="9c803-171">Další informace o [pomocí Windows Powershellu pro StorSimple tooadminister zařízení StorSimple](storsimple-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="9c803-171">Learn more about [using Windows PowerShell for StorSimple tooadminister your StorSimple device](storsimple-windows-powershell-administration.md).</span></span>
* <span data-ttu-id="9c803-172">Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="9c803-172">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

