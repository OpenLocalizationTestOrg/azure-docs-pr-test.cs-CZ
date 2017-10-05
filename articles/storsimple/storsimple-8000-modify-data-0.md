---
title: "Upravit DATA 0 nastavení na zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Další informace o použití prostředí Windows PowerShell pro StorSimple překonfigurovat síťového rozhraní DATA 0 v zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: 90df43e22f17fd32fe642514df098b72700e77af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="modify-the-data-0-network-interface-settings-on-your-storsimple-8000-series-device"></a><span data-ttu-id="43dc9-103">Upravit nastavení 0 síťového rozhraní DATA ve vašem zařízení řady StorSimple 8000</span><span class="sxs-lookup"><span data-stu-id="43dc9-103">Modify the DATA 0 network interface settings on your StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="43dc9-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="43dc9-104">Overview</span></span>

<span data-ttu-id="43dc9-105">Zařízení s Microsoft Azure StorSimple má šest síťových rozhraní, z DATA 0 až DATA 5.</span><span class="sxs-lookup"><span data-stu-id="43dc9-105">Your Microsoft Azure StorSimple device has six network interfaces, from DATA 0 to DATA 5.</span></span> <span data-ttu-id="43dc9-106">DATA 0 rozhraní vždycky nakonfigurovaný pomocí rozhraní Windows PowerShell nebo konzole sériového portu a je automaticky povolenou podporu cloudu.</span><span class="sxs-lookup"><span data-stu-id="43dc9-106">The DATA 0 interface is always configured through the Windows PowerShell interface or the serial console, and is automatically cloud-enabled.</span></span> <span data-ttu-id="43dc9-107">Všimněte si, že nemůžete nakonfigurovat síťového rozhraní DATA 0 prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="43dc9-107">Note that you cannot configure DATA 0 network interface through the Azure portal.</span></span>

<span data-ttu-id="43dc9-108">DATA 0 rozhraní nejprve nakonfigurovaný pomocí Průvodce instalací během počáteční nasazení zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="43dc9-108">The DATA 0 interface is first configured through the setup wizard during initial deployment of the StorSimple device.</span></span> <span data-ttu-id="43dc9-109">Když je zařízení v provozním režimu, bude pravděpodobně třeba překonfigurovat DATA 0 nastavení.</span><span class="sxs-lookup"><span data-stu-id="43dc9-109">When the device is in an operational mode, you may need to reconfigure DATA 0 settings.</span></span> <span data-ttu-id="43dc9-110">V tomto kurzu nabízí dvě metody k úpravě DATA 0 síťová nastavení, jak pomocí prostředí Windows PowerShell pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="43dc9-110">This tutorial provides two methods to modify DATA 0 network settings, both through Windows PowerShell for StorSimple.</span></span>

<span data-ttu-id="43dc9-111">Po přečtení tohoto kurzu, budete moci:</span><span class="sxs-lookup"><span data-stu-id="43dc9-111">After reading this tutorial, you will be able to:</span></span>

* <span data-ttu-id="43dc9-112">Upravit DATA 0 síťová nastavení prostřednictvím Průvodce instalací</span><span class="sxs-lookup"><span data-stu-id="43dc9-112">Modify DATA 0 network setting through the setup wizard</span></span>
* <span data-ttu-id="43dc9-113">Upravit nastavení DATA 0 sítě prostřednictvím `Set-HcsNetInterface` rutiny</span><span class="sxs-lookup"><span data-stu-id="43dc9-113">Modify DATA 0 network settings through the `Set-HcsNetInterface` cmdlet</span></span>

## <a name="modify-data-0-network-settings-through-setup-wizard"></a><span data-ttu-id="43dc9-114">Upravit nastavení DATA 0 sítě prostřednictvím Průvodce instalací</span><span class="sxs-lookup"><span data-stu-id="43dc9-114">Modify DATA 0 network settings through setup wizard</span></span>
<span data-ttu-id="43dc9-115">Můžete změnit konfiguraci dat nastavení 0 sítě pomocí připojení k rozhraní Windows PowerShell zařízení StorSimple a spuštění relace Průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="43dc9-115">You can reconfigure DATA 0 network settings by connecting to the Windows PowerShell interface of your StorSimple device and launching a setup wizard session.</span></span> <span data-ttu-id="43dc9-116">Proveďte následující kroky k úpravě DATA 0 nastavení:</span><span class="sxs-lookup"><span data-stu-id="43dc9-116">Perform the following steps to modify DATA 0 settings:</span></span>

#### <a name="to-modify-data-0-network-settings-through-setup-wizard"></a><span data-ttu-id="43dc9-117">K úpravě nastavení DATA 0 sítě prostřednictvím Průvodce instalací</span><span class="sxs-lookup"><span data-stu-id="43dc9-117">To modify DATA 0 network settings through setup wizard</span></span>
1. <span data-ttu-id="43dc9-118">V nabídce konzoly sériového portu, zvolte možnost 1, **přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="43dc9-118">In the serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="43dc9-119">Po zobrazení výzvy zadejte **hesla správce zařízení**.</span><span class="sxs-lookup"><span data-stu-id="43dc9-119">When prompted provide the **device administrator password**.</span></span> <span data-ttu-id="43dc9-120">Výchozí heslo je `Password1`.</span><span class="sxs-lookup"><span data-stu-id="43dc9-120">The default password is `Password1`.</span></span>
2. <span data-ttu-id="43dc9-121">Na příkazovém řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="43dc9-121">At the command prompt, type:</span></span>
   
    `Invoke-HcsSetupWizard`
3. <span data-ttu-id="43dc9-122">Zobrazí se Průvodce instalací vám pomohou při konfiguraci DATA 0 rozhraní vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="43dc9-122">A setup wizard appears to help configure the DATA 0 interface of your device.</span></span> <span data-ttu-id="43dc9-123">Zadejte nové hodnoty pro adresu IP, brány a síťovou masku.</span><span class="sxs-lookup"><span data-stu-id="43dc9-123">Provide new values for the IP address, gateway, and netmask.</span></span>

> [!NOTE]
> <span data-ttu-id="43dc9-124">Opravené řadiče IP adres bude třeba překonfigurovat pomocí **nastavení síťového** okno zařízení StorSimple na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="43dc9-124">The fixed controllers IPs will need to be reconfigured through the **Network settings** blade of the StorSimple device in the Azure portal.</span></span> <span data-ttu-id="43dc9-125">Další informace, přejděte na [upravit síťových rozhraní](storsimple-8000-modify-device-config.md#modify-network-interfaces).</span><span class="sxs-lookup"><span data-stu-id="43dc9-125">For more information, go to [Modify network interfaces](storsimple-8000-modify-device-config.md#modify-network-interfaces).</span></span>

## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a><span data-ttu-id="43dc9-126">Úprava nastavení DATA 0 sítě pomocí rutiny Set-HcsNetInterface</span><span class="sxs-lookup"><span data-stu-id="43dc9-126">Modify DATA 0 network settings through Set-HcsNetInterface cmdlet</span></span>
<span data-ttu-id="43dc9-127">Alternativní způsob překonfigurovat DATA 0 síťové rozhraní je prostřednictvím `Set-HcsNetInterface` rutiny.</span><span class="sxs-lookup"><span data-stu-id="43dc9-127">An alternate way to reconfigure DATA 0 network interface is through the use of the `Set-HcsNetInterface` cmdlet.</span></span> <span data-ttu-id="43dc9-128">Rutina se spustí z rozhraní Windows PowerShell zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="43dc9-128">The cmdlet is executed from the Windows PowerShell interface of your StorSimple device.</span></span> <span data-ttu-id="43dc9-129">Při použití tohoto postupu, pevné IP adresy řadiče také zde mohou být konfigurovány.</span><span class="sxs-lookup"><span data-stu-id="43dc9-129">When using this procedure, the controller fixed IPs can also be configured here.</span></span> <span data-ttu-id="43dc9-130">Proveďte následující kroky k úpravě DATA 0 nastavení:</span><span class="sxs-lookup"><span data-stu-id="43dc9-130">Perform the following steps to modify the DATA 0 settings:</span></span> 

#### <a name="to-modify-data-0-network-settings-through-the-set-hcsnetinterface-cmdlet"></a><span data-ttu-id="43dc9-131">Úprava nastavení DATA 0 sítě pomocí rutiny Set-HcsNetInterface</span><span class="sxs-lookup"><span data-stu-id="43dc9-131">To modify DATA 0 network settings through the Set-HcsNetInterface cmdlet</span></span>
1. <span data-ttu-id="43dc9-132">V nabídce konzoly sériového portu, zvolte možnost 1, **přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="43dc9-132">In the serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="43dc9-133">Po zobrazení výzvy zadejte heslo správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="43dc9-133">When prompted provide the device administrator password.</span></span> <span data-ttu-id="43dc9-134">Výchozí heslo je `Password1`.</span><span class="sxs-lookup"><span data-stu-id="43dc9-134">The default password is `Password1`.</span></span>
2. <span data-ttu-id="43dc9-135">Na příkazovém řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="43dc9-135">At the command prompt, type:</span></span>
   
    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
   
    <span data-ttu-id="43dc9-136">V lomené závorky zadejte následující hodnoty pro DATA 0:</span><span class="sxs-lookup"><span data-stu-id="43dc9-136">In the angled brackets, type the following values for DATA 0:</span></span>
   
   * <span data-ttu-id="43dc9-137">Adresa IPv4</span><span class="sxs-lookup"><span data-stu-id="43dc9-137">IPv4 address</span></span>
   * <span data-ttu-id="43dc9-138">Brána IPv4</span><span class="sxs-lookup"><span data-stu-id="43dc9-138">IPv4 gateway</span></span>
   * <span data-ttu-id="43dc9-139">Maska podsítě IPv4</span><span class="sxs-lookup"><span data-stu-id="43dc9-139">IPv4 subnet mask</span></span>
   * <span data-ttu-id="43dc9-140">Opravené adresu IPv4 pro řadič 0</span><span class="sxs-lookup"><span data-stu-id="43dc9-140">Fixed IPv4 address for Controller 0</span></span>
   * <span data-ttu-id="43dc9-141">Opravené adresu IPv4 pro řadič 1</span><span class="sxs-lookup"><span data-stu-id="43dc9-141">Fixed IPv4 address for Controller 1</span></span>
     
     <span data-ttu-id="43dc9-142">Další informace o použití této rutiny, přejděte na [prostředí Windows PowerShell pro referenční informace o rutinách StorSimple](https://technet.microsoft.com/library/dn688161.aspx).</span><span class="sxs-lookup"><span data-stu-id="43dc9-142">For more information on the use of this cmdlet, go to [Windows PowerShell for StorSimple cmdlet reference](https://technet.microsoft.com/library/dn688161.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="43dc9-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="43dc9-143">Next steps</span></span>
* <span data-ttu-id="43dc9-144">Ke konfiguraci síťových rozhraní než DATA 0, můžete použít [konfigurace nastavení sítě na portálu Azure](storsimple-8000-modify-device-config.md).</span><span class="sxs-lookup"><span data-stu-id="43dc9-144">To configure network interfaces other than DATA 0, you can use the [Configure network settings in the Azure portal](storsimple-8000-modify-device-config.md).</span></span> 
* <span data-ttu-id="43dc9-145">Pokud máte problémy při konfiguraci síťových rozhraní, podívejte se na [potíží s nasazením](storsimple-troubleshoot-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="43dc9-145">If you experience any issues when configuring your network interfaces, refer to [Troubleshoot deployment issues](storsimple-troubleshoot-deployment.md).</span></span>

