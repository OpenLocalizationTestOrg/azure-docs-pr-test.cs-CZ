---
title: "aaaModify DATA 0 nastavení na zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Zjistěte, jak toouse Windows Powershellu pro StorSimple tooreconfigure hello síťového rozhraní DATA 0 v zařízení StorSimple."
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
ms.openlocfilehash: 2d3bdd3675017be86def45a81d94b6c03fc61da6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="modify-hello-data-0-network-interface-settings-on-your-storsimple-8000-series-device"></a><span data-ttu-id="53353-103">Upravit nastavení rozhraní 0 sítě hello DATA ve vašem zařízení řady StorSimple 8000</span><span class="sxs-lookup"><span data-stu-id="53353-103">Modify hello DATA 0 network interface settings on your StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="53353-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="53353-104">Overview</span></span>

<span data-ttu-id="53353-105">Zařízení s Microsoft Azure StorSimple má šest síťových rozhraní, z dat 0 tooDATA 5.</span><span class="sxs-lookup"><span data-stu-id="53353-105">Your Microsoft Azure StorSimple device has six network interfaces, from DATA 0 tooDATA 5.</span></span> <span data-ttu-id="53353-106">Hello DATA 0 rozhraní vždycky nakonfigurovaný pomocí rozhraní Windows PowerShell hello nebo hello konzoly sériového portu a je automaticky povolenou podporu cloudu.</span><span class="sxs-lookup"><span data-stu-id="53353-106">hello DATA 0 interface is always configured through hello Windows PowerShell interface or hello serial console, and is automatically cloud-enabled.</span></span> <span data-ttu-id="53353-107">Všimněte si, že nemůžete nakonfigurovat síťového rozhraní DATA 0 prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="53353-107">Note that you cannot configure DATA 0 network interface through hello Azure portal.</span></span>

<span data-ttu-id="53353-108">Hello rozhraní DATA 0 je nejdřív nakonfigurovali prostřednictvím Průvodce instalací hello během počátečního nasazení zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="53353-108">hello DATA 0 interface is first configured through hello setup wizard during initial deployment of hello StorSimple device.</span></span> <span data-ttu-id="53353-109">Když je zařízení hello v provozní režim, musíte tooreconfigure DATA 0 nastavení.</span><span class="sxs-lookup"><span data-stu-id="53353-109">When hello device is in an operational mode, you may need tooreconfigure DATA 0 settings.</span></span> <span data-ttu-id="53353-110">V tomto kurzu nabízí dvě metody toomodify DATA 0 síťová nastavení, jak pomocí prostředí Windows PowerShell pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="53353-110">This tutorial provides two methods toomodify DATA 0 network settings, both through Windows PowerShell for StorSimple.</span></span>

<span data-ttu-id="53353-111">Po přečtení tohoto kurzu, budete moci:</span><span class="sxs-lookup"><span data-stu-id="53353-111">After reading this tutorial, you will be able to:</span></span>

* <span data-ttu-id="53353-112">Upravit DATA 0 síťová nastavení prostřednictvím Průvodce instalací hello</span><span class="sxs-lookup"><span data-stu-id="53353-112">Modify DATA 0 network setting through hello setup wizard</span></span>
* <span data-ttu-id="53353-113">Upravit nastavení DATA 0 sítě prostřednictvím hello `Set-HcsNetInterface` rutiny</span><span class="sxs-lookup"><span data-stu-id="53353-113">Modify DATA 0 network settings through hello `Set-HcsNetInterface` cmdlet</span></span>

## <a name="modify-data-0-network-settings-through-setup-wizard"></a><span data-ttu-id="53353-114">Upravit nastavení DATA 0 sítě prostřednictvím Průvodce instalací</span><span class="sxs-lookup"><span data-stu-id="53353-114">Modify DATA 0 network settings through setup wizard</span></span>
<span data-ttu-id="53353-115">Nastavení DATA 0 sítě můžete znovu nakonfigurovat pomocí rozhraní Windows PowerShell toohello zařízení StorSimple připojení a spuštění relace Průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="53353-115">You can reconfigure DATA 0 network settings by connecting toohello Windows PowerShell interface of your StorSimple device and launching a setup wizard session.</span></span> <span data-ttu-id="53353-116">Proveďte následující kroky toomodify DATA 0 hello nastavení:</span><span class="sxs-lookup"><span data-stu-id="53353-116">Perform hello following steps toomodify DATA 0 settings:</span></span>

#### <a name="toomodify-data-0-network-settings-through-setup-wizard"></a><span data-ttu-id="53353-117">nastavení 0 sítě toomodify DATA prostřednictvím Průvodce instalací</span><span class="sxs-lookup"><span data-stu-id="53353-117">toomodify DATA 0 network settings through setup wizard</span></span>
1. <span data-ttu-id="53353-118">V nabídce konzoly sériového portu hello, zvolte možnost 1, **přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="53353-118">In hello serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="53353-119">Po zobrazení výzvy zadejte hello **hesla správce zařízení**.</span><span class="sxs-lookup"><span data-stu-id="53353-119">When prompted provide hello **device administrator password**.</span></span> <span data-ttu-id="53353-120">výchozí heslo Hello je `Password1`.</span><span class="sxs-lookup"><span data-stu-id="53353-120">hello default password is `Password1`.</span></span>
2. <span data-ttu-id="53353-121">Hello příkazového řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="53353-121">At hello command prompt, type:</span></span>
   
    `Invoke-HcsSetupWizard`
3. <span data-ttu-id="53353-122">Průvodce instalací se zobrazí toohelp konfigurace hello DATA 0 rozhraní vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="53353-122">A setup wizard appears toohelp configure hello DATA 0 interface of your device.</span></span> <span data-ttu-id="53353-123">Zadejte nové hodnoty pro hello IP adresu, brány a síťovou masku.</span><span class="sxs-lookup"><span data-stu-id="53353-123">Provide new values for hello IP address, gateway, and netmask.</span></span>

> [!NOTE]
> <span data-ttu-id="53353-124">pevné IP adresy, bude nutné překonfigurovat prostřednictvím hello toobe řadiče Hello **nastavení síťového** okno hello zařízení StorSimple v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="53353-124">hello fixed controllers IPs will need toobe reconfigured through hello **Network settings** blade of hello StorSimple device in hello Azure portal.</span></span> <span data-ttu-id="53353-125">Další informace, přejděte příliš[upravit síťových rozhraní](storsimple-8000-modify-device-config.md#modify-network-interfaces).</span><span class="sxs-lookup"><span data-stu-id="53353-125">For more information, go too[Modify network interfaces](storsimple-8000-modify-device-config.md#modify-network-interfaces).</span></span>

## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a><span data-ttu-id="53353-126">Úprava nastavení DATA 0 sítě pomocí rutiny Set-HcsNetInterface</span><span class="sxs-lookup"><span data-stu-id="53353-126">Modify DATA 0 network settings through Set-HcsNetInterface cmdlet</span></span>
<span data-ttu-id="53353-127">Jiný způsob, jak tooreconfigure DATA 0 síťové rozhraní je prostřednictvím použití hello hello `Set-HcsNetInterface` rutiny.</span><span class="sxs-lookup"><span data-stu-id="53353-127">An alternate way tooreconfigure DATA 0 network interface is through hello use of hello `Set-HcsNetInterface` cmdlet.</span></span> <span data-ttu-id="53353-128">Hello rutina se spustí z rozhraní Windows PowerShell hello zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="53353-128">hello cmdlet is executed from hello Windows PowerShell interface of your StorSimple device.</span></span> <span data-ttu-id="53353-129">Při použití tohoto postupu, pevné IP adresy řadiče hello také zde mohou být konfigurovány.</span><span class="sxs-lookup"><span data-stu-id="53353-129">When using this procedure, hello controller fixed IPs can also be configured here.</span></span> <span data-ttu-id="53353-130">Proveďte následující kroky toomodify hello DATA 0 hello nastavení:</span><span class="sxs-lookup"><span data-stu-id="53353-130">Perform hello following steps toomodify hello DATA 0 settings:</span></span> 

#### <a name="toomodify-data-0-network-settings-through-hello-set-hcsnetinterface-cmdlet"></a><span data-ttu-id="53353-131">nastavení 0 sítě toomodify dat pomocí rutiny Set-HcsNetInterface hello</span><span class="sxs-lookup"><span data-stu-id="53353-131">toomodify DATA 0 network settings through hello Set-HcsNetInterface cmdlet</span></span>
1. <span data-ttu-id="53353-132">V nabídce konzoly sériového portu hello, zvolte možnost 1, **přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="53353-132">In hello serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="53353-133">Po zobrazení výzvy zadejte heslo správce zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="53353-133">When prompted provide hello device administrator password.</span></span> <span data-ttu-id="53353-134">výchozí heslo Hello je `Password1`.</span><span class="sxs-lookup"><span data-stu-id="53353-134">hello default password is `Password1`.</span></span>
2. <span data-ttu-id="53353-135">Hello příkazového řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="53353-135">At hello command prompt, type:</span></span>
   
    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
   
    <span data-ttu-id="53353-136">V závorkách hello šikmo zadejte následující hodnoty pro DATA 0 hello:</span><span class="sxs-lookup"><span data-stu-id="53353-136">In hello angled brackets, type hello following values for DATA 0:</span></span>
   
   * <span data-ttu-id="53353-137">Adresa IPv4</span><span class="sxs-lookup"><span data-stu-id="53353-137">IPv4 address</span></span>
   * <span data-ttu-id="53353-138">Brána IPv4</span><span class="sxs-lookup"><span data-stu-id="53353-138">IPv4 gateway</span></span>
   * <span data-ttu-id="53353-139">Maska podsítě IPv4</span><span class="sxs-lookup"><span data-stu-id="53353-139">IPv4 subnet mask</span></span>
   * <span data-ttu-id="53353-140">Opravené adresu IPv4 pro řadič 0</span><span class="sxs-lookup"><span data-stu-id="53353-140">Fixed IPv4 address for Controller 0</span></span>
   * <span data-ttu-id="53353-141">Opravené adresu IPv4 pro řadič 1</span><span class="sxs-lookup"><span data-stu-id="53353-141">Fixed IPv4 address for Controller 1</span></span>
     
     <span data-ttu-id="53353-142">Další informace o hello použití této rutiny naleznete příliš[prostředí Windows PowerShell pro referenční informace o rutinách StorSimple](https://technet.microsoft.com/library/dn688161.aspx).</span><span class="sxs-lookup"><span data-stu-id="53353-142">For more information on hello use of this cmdlet, go too[Windows PowerShell for StorSimple cmdlet reference](https://technet.microsoft.com/library/dn688161.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="53353-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="53353-143">Next steps</span></span>
* <span data-ttu-id="53353-144">tooconfigure síťová rozhraní kromě rozhraní DATA 0, můžete použít hello [konfigurace nastavení sítě v portálu Azure hello](storsimple-8000-modify-device-config.md).</span><span class="sxs-lookup"><span data-stu-id="53353-144">tooconfigure network interfaces other than DATA 0, you can use hello [Configure network settings in hello Azure portal](storsimple-8000-modify-device-config.md).</span></span> 
* <span data-ttu-id="53353-145">Pokud máte problémy při konfiguraci síťových rozhraní, podívejte se příliš[potíží s nasazením](storsimple-troubleshoot-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="53353-145">If you experience any issues when configuring your network interfaces, refer too[Troubleshoot deployment issues](storsimple-troubleshoot-deployment.md).</span></span>

