---
title: "aaaConnect vzdáleně tooyour zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak tooconfigure zařízení pro vzdálenou správu a jak tooconnect tooWindows Powershellu pro StorSimple prostřednictvím protokolu HTTP nebo HTTPS."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 38b6a6350891b9f6f8fdfc55880b2f47105d947c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-remotely-tooyour-storsimple-8000-series-device"></a><span data-ttu-id="96419-103">Vzdálené připojení zařízení řady StorSimple 8000 tooyour</span><span class="sxs-lookup"><span data-stu-id="96419-103">Connect remotely tooyour StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="96419-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="96419-104">Overview</span></span>

<span data-ttu-id="96419-105">Můžete vzdáleně připojit zařízení tooyour pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="96419-105">You can remotely connect tooyour device via Windows PowerShell.</span></span> <span data-ttu-id="96419-106">Když připojíte tímto způsobem, neuvidíte žádné nabídky.</span><span class="sxs-lookup"><span data-stu-id="96419-106">When you connect this way, you do not see a menu.</span></span> <span data-ttu-id="96419-107">(Zobrazí nabídky pouze v případě, že používáte hello konzoly sériového portu v zařízení tooconnect hello.) Díky vzdálené komunikace Windows Powershellu připojit tooa konkrétní prostředí runspace.</span><span class="sxs-lookup"><span data-stu-id="96419-107">(You see a menu only if you use hello serial console on hello device tooconnect.) With Windows PowerShell remoting, you connect tooa specific runspace.</span></span> <span data-ttu-id="96419-108">Můžete také zadat hello jazyk zobrazení.</span><span class="sxs-lookup"><span data-stu-id="96419-108">You can also specify hello display language.</span></span>

<span data-ttu-id="96419-109">Další informace o použití toomanage vzdálenou komunikaci prostředí Windows PowerShell zařízení, přejděte příliš[pomocí Windows Powershellu pro StorSimple tooadminister zařízení StorSimple](storsimple-8000-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="96419-109">For more information about using Windows PowerShell remoting toomanage your device, go too[Use Windows PowerShell for StorSimple tooadminister your StorSimple device](storsimple-8000-windows-powershell-administration.md).</span></span>

<span data-ttu-id="96419-110">Tento kurz vysvětluje, jak tooconfigure zařízení pro vzdálenou správu a pak tooconnect tooWindows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="96419-110">This tutorial explains how tooconfigure your device for remote management and then how tooconnect tooWindows PowerShell for StorSimple.</span></span> <span data-ttu-id="96419-111">Můžete použít protokol HTTP nebo HTTPS tooremotely připojit prostřednictvím Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="96419-111">You can use HTTP or HTTPS tooremotely connect via Windows PowerShell.</span></span> <span data-ttu-id="96419-112">Ale při rozhodování jak tooconnect tooWindows Powershellu pro StorSimple, zvažte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="96419-112">However, when you are deciding how tooconnect tooWindows PowerShell for StorSimple, consider hello following information:</span></span>

* <span data-ttu-id="96419-113">Připojení přímo toohello konzoly sériového portu zařízení je bezpečné, ale není připojování konzoly sériového portu toohello přes síťové přepínače.</span><span class="sxs-lookup"><span data-stu-id="96419-113">Connecting directly toohello device serial console is secure, but connecting toohello serial console over network switches is not.</span></span> <span data-ttu-id="96419-114">Buďte opatrní hello rizika zabezpečení při připojování konzoly sériového portu zařízení toohello přes síťové přepínače.</span><span class="sxs-lookup"><span data-stu-id="96419-114">Be cautious of hello security risk when connecting toohello device serial console over network switches.</span></span>
* <span data-ttu-id="96419-115">Připojení přes relaci protokolu HTTP může nabízí lepší zabezpečení než připojení prostřednictvím sériové konzoly hello přes síť hello.</span><span class="sxs-lookup"><span data-stu-id="96419-115">Connecting through an HTTP session might offer more security than connecting through hello serial console over hello network.</span></span> <span data-ttu-id="96419-116">I když to není hello nejbezpečnější metodou, je přijatelné v důvěryhodných sítích.</span><span class="sxs-lookup"><span data-stu-id="96419-116">Although this is not hello most secure method, it is acceptable on trusted networks.</span></span>
* <span data-ttu-id="96419-117">Připojení prostřednictvím relace HTTPS se certifikát podepsaný svým držitelem je hello nejbezpečnější a doporučená možnost hello.</span><span class="sxs-lookup"><span data-stu-id="96419-117">Connecting through an HTTPS session with a self-signed certificate is hello most secure and hello recommended option.</span></span>

<span data-ttu-id="96419-118">Můžete vzdáleně připojit toohello rozhraní Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="96419-118">You can connect remotely toohello Windows PowerShell interface.</span></span> <span data-ttu-id="96419-119">Zařízení StorSimple tooyour vzdáleného přístupu prostřednictvím rozhraní Windows PowerShell hello však není povoleno ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="96419-119">However, remote access tooyour StorSimple device via hello Windows PowerShell interface is not enabled by default.</span></span> <span data-ttu-id="96419-120">Musíte nejdřív povolit vzdálenou správu na hello zařízení a potom na hello klienta, který je použité tooaccess zařízení.</span><span class="sxs-lookup"><span data-stu-id="96419-120">You must enable remote management on hello device first, and then on hello client that is used tooaccess your device.</span></span>

<span data-ttu-id="96419-121">Hello kroky popsané v tomto článku byly provedeny v hostitelském systému, systémem Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="96419-121">hello steps described in this article were performed on a host system running Windows Server 2012 R2.</span></span>

## <a name="connect-through-http"></a><span data-ttu-id="96419-122">Připojení prostřednictvím protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="96419-122">Connect through HTTP</span></span>

<span data-ttu-id="96419-123">Připojení tooWindows Powershellu pro StorSimple přes relaci protokolu HTTP poskytuje lepší zabezpečení než připojení prostřednictvím sériové konzoly hello zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="96419-123">Connecting tooWindows PowerShell for StorSimple through an HTTP session offers more security than connecting through hello serial console of your StorSimple device.</span></span> <span data-ttu-id="96419-124">I když to není hello nejbezpečnější metodou, je přijatelné v důvěryhodných sítích.</span><span class="sxs-lookup"><span data-stu-id="96419-124">Although this is not hello most secure method, it is acceptable on trusted networks.</span></span>

<span data-ttu-id="96419-125">Můžete použít buď hello portálu nebo hello konzoly sériového portu tooconfigure vzdálenou správu Azure.</span><span class="sxs-lookup"><span data-stu-id="96419-125">You can use either hello Azure portal or hello serial console tooconfigure remote management.</span></span> <span data-ttu-id="96419-126">Vyberte z následujících postupů hello:</span><span class="sxs-lookup"><span data-stu-id="96419-126">Select from hello following procedures:</span></span>

* [<span data-ttu-id="96419-127">Používání hello Azure portálu tooenable vzdálené správy přes protokol HTTP</span><span class="sxs-lookup"><span data-stu-id="96419-127">Use hello Azure portal tooenable remote management over HTTP</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [<span data-ttu-id="96419-128">Používání hello konzoly sériového portu tooenable vzdálené správy přes protokol HTTP</span><span class="sxs-lookup"><span data-stu-id="96419-128">Use hello serial console tooenable remote management over HTTP</span></span>](#use-the-serial-console-to-enable-remote-management-over-http)

<span data-ttu-id="96419-129">Po povolení vzdálené správy, použijte následující postup tooprepare hello klienta pro připojení ke vzdálené hello.</span><span class="sxs-lookup"><span data-stu-id="96419-129">After you enable remote management, use hello following procedure tooprepare hello client for a remote connection.</span></span>

* [<span data-ttu-id="96419-130">Připravit hello klienta pro připojení ke vzdálené</span><span class="sxs-lookup"><span data-stu-id="96419-130">Prepare hello client for remote connection</span></span>](#prepare-the-client-for-remote-connection)

### <a name="use-hello-azure-portal-tooenable-remote-management-over-http"></a><span data-ttu-id="96419-131">Používání hello Azure portálu tooenable vzdálené správy přes protokol HTTP</span><span class="sxs-lookup"><span data-stu-id="96419-131">Use hello Azure portal tooenable remote management over HTTP</span></span>

<span data-ttu-id="96419-132">Proveďte následující kroky v hello Azure portálu tooenable vzdálené správy přes protokol HTTP hello.</span><span class="sxs-lookup"><span data-stu-id="96419-132">Perform hello following steps in hello Azure portal tooenable remote management over HTTP.</span></span>

#### <a name="tooenable-remote-management-through-hello-azure-portal"></a><span data-ttu-id="96419-133">tooenable vzdálenou správu prostřednictvím hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="96419-133">tooenable remote management through hello Azure portal</span></span>

1. <span data-ttu-id="96419-134">Přejděte služby StorSimple Manager zařízení tooyour.</span><span class="sxs-lookup"><span data-stu-id="96419-134">Go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="96419-135">Vyberte **zařízení** a pak vyberte a klikněte na zařízení hello chcete tooconfigure pro vzdálenou správu.</span><span class="sxs-lookup"><span data-stu-id="96419-135">Select **Devices** and then select and click hello device you want tooconfigure for remote management.</span></span> <span data-ttu-id="96419-136">Přejděte příliš**nastavení zařízení > zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="96419-136">Go too**Device settings > Security**.</span></span>
2. <span data-ttu-id="96419-137">V hello **nastavení zabezpečení** okně klikněte na tlačítko **vzdálenou správu**.</span><span class="sxs-lookup"><span data-stu-id="96419-137">In hello **Security settings** blade, click **Remote Management**.</span></span>
3. <span data-ttu-id="96419-138">V hello **vzdálenou správu** okně nastavit **povolit vzdálenou správu** příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="96419-138">In hello **Remote management** blade, set **Enable Remote Management** too**Yes**.</span></span>
4. <span data-ttu-id="96419-139">Teď můžete zvolit tooconnect pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="96419-139">You can now choose tooconnect using HTTP.</span></span> <span data-ttu-id="96419-140">(výchozí hello je tooconnect přes protokol HTTPS.) Ujistěte se, že je vybraný HTTP.</span><span class="sxs-lookup"><span data-stu-id="96419-140">(hello default is tooconnect over HTTPS.) Make sure that HTTP is selected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="96419-141">Připojení pomocí protokolu HTTP je přijatelné jenom v důvěryhodných sítích.</span><span class="sxs-lookup"><span data-stu-id="96419-141">Connecting over HTTP is acceptable only on trusted networks.</span></span>
   
5. <span data-ttu-id="96419-142">Klikněte na tlačítko **Uložit** a po zobrazení výzvy k potvrzení, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="96419-142">Click **Save** and when prompted for confirmation, select **Yes**.</span></span>

### <a name="use-hello-serial-console-tooenable-remote-management-over-http"></a><span data-ttu-id="96419-143">Používání hello konzoly sériového portu tooenable vzdálené správy přes protokol HTTP</span><span class="sxs-lookup"><span data-stu-id="96419-143">Use hello serial console tooenable remote management over HTTP</span></span>
<span data-ttu-id="96419-144">Proveďte následující kroky na hello zařízení konzoly sériového portu tooenable vzdálenou správu hello.</span><span class="sxs-lookup"><span data-stu-id="96419-144">Perform hello following steps on hello device serial console tooenable remote management.</span></span>

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a><span data-ttu-id="96419-145">tooenable vzdálenou správu prostřednictvím konzoly sériového portu zařízení hello</span><span class="sxs-lookup"><span data-stu-id="96419-145">tooenable remote management through hello device serial console</span></span>
1. <span data-ttu-id="96419-146">V nabídce konzoly sériového portu hello vyberte možnost 1.</span><span class="sxs-lookup"><span data-stu-id="96419-146">On hello serial console menu, select option 1.</span></span> <span data-ttu-id="96419-147">Další informace o používání konzoly sériového portu hello na hello zařízení, přejděte příliš[připojení tooWindows Powershellu pro StorSimple prostřednictvím konzoly sériového portu zařízení](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="96419-147">For more information about using hello serial console on hello device, go too[Connect tooWindows PowerShell for StorSimple via device serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="96419-148">Hello řádku zadejte:`Enable-HcsRemoteManagement –AllowHttp`</span><span class="sxs-lookup"><span data-stu-id="96419-148">At hello prompt, type: `Enable-HcsRemoteManagement –AllowHttp`</span></span>
3. <span data-ttu-id="96419-149">Jsou oznámení o ohrožení zabezpečení hello pomocí protokolu HTTP tooconnect toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="96419-149">You are notified about hello security vulnerabilities of using HTTP tooconnect toohello device.</span></span> <span data-ttu-id="96419-150">Po zobrazení výzvy potvrďte zadáním **Y**.</span><span class="sxs-lookup"><span data-stu-id="96419-150">When prompted, confirm by typing **Y**.</span></span>
4. <span data-ttu-id="96419-151">Ověřte, že je povolený protokol HTTP zadáním:`Get-HcsSystem`</span><span class="sxs-lookup"><span data-stu-id="96419-151">Verify that HTTP is enabled by typing: `Get-HcsSystem`</span></span>
5. <span data-ttu-id="96419-152">Ověřte, že hello **RemoteManagementMode** pole ukazuje **HttpsAndHttpEnabled**.hello následující obrázek ukazuje tato nastavení v PuTTY.</span><span class="sxs-lookup"><span data-stu-id="96419-152">Verify that hello **RemoteManagementMode** field shows **HttpsAndHttpEnabled**.hello following illustration shows these settings in PuTTY.</span></span>
   
     ![Sériového portu HTTPS a HTTP povoleno](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-hello-client-for-remote-connection"></a><span data-ttu-id="96419-154">Připravit hello klienta pro připojení ke vzdálené</span><span class="sxs-lookup"><span data-stu-id="96419-154">Prepare hello client for remote connection</span></span>
<span data-ttu-id="96419-155">Proveďte následující kroky na vzdálenou správu serveru signálu hello klienta tooenable hello.</span><span class="sxs-lookup"><span data-stu-id="96419-155">Perform hello following steps on hello client tooenable remote management.</span></span>

#### <a name="tooprepare-hello-client-for-remote-connection"></a><span data-ttu-id="96419-156">tooprepare hello klienta pro připojení ke vzdálené</span><span class="sxs-lookup"><span data-stu-id="96419-156">tooprepare hello client for remote connection</span></span>
1. <span data-ttu-id="96419-157">Spusťte relaci prostředí Windows PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="96419-157">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="96419-158">Zadejte následující příkaz tooadd hello IP adresu ze seznamu důvěryhodných hostitelů hello StorSimple zařízení toohello klienta hello:</span><span class="sxs-lookup"><span data-stu-id="96419-158">Type hello following command tooadd hello IP address of hello StorSimple device toohello client’s trusted hosts list:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     <span data-ttu-id="96419-159">Nahraďte <*device_ip*> s hello IP adresa vašeho zařízení; například:</span><span class="sxs-lookup"><span data-stu-id="96419-159">Replace <*device_ip*> with hello IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="96419-160">Zadejte následující příkaz toosave hello přihlašovací údaje zařízení v proměnné hello:</span><span class="sxs-lookup"><span data-stu-id="96419-160">Type hello following command toosave hello device credentials in a variable:</span></span> 
   
    ```
    $cred = Get-Credential
    ```
    
4. <span data-ttu-id="96419-161">V dialogovém okně hello které se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="96419-161">In hello dialog box that appears:</span></span>
   
   1. <span data-ttu-id="96419-162">Zadejte jméno uživatele hello v tomto formátu: *device_ip\SSAdmin*.</span><span class="sxs-lookup"><span data-stu-id="96419-162">Type hello user name in this format: *device_ip\SSAdmin*.</span></span>
   2. <span data-ttu-id="96419-163">Zadejte heslo správce zařízení hello, který byl nastaven při konfiguraci zařízení hello pomocí Průvodce instalací hello.</span><span class="sxs-lookup"><span data-stu-id="96419-163">Type hello device administrator password that was set when hello device was configured with hello setup wizard.</span></span> <span data-ttu-id="96419-164">výchozí heslo Hello je *Heslo1*.</span><span class="sxs-lookup"><span data-stu-id="96419-164">hello default password is *Password1*.</span></span>
5. <span data-ttu-id="96419-165">Spusťte relaci prostředí Windows PowerShell na zařízení hello zadáním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="96419-165">Start a Windows PowerShell session on hello device by typing this command:</span></span>
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > <span data-ttu-id="96419-166">toocreate relaci prostředí Windows PowerShell pro použití s hello virtuálního zařízení StorSimple, připojit hello `–Port` parametr a zadejte hello veřejný port, který jste nakonfigurovali v vzdálenou komunikaci pro virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="96419-166">toocreate a Windows PowerShell session for use with hello StorSimple virtual device, append hello `–Port` parameter and specify hello public port that you configured in Remoting for StorSimple Virtual Appliance.</span></span>
   
   
<span data-ttu-id="96419-167">V tomto okamžiku by měl mít zařízení aktivní toohello vzdálené relace prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="96419-167">At this point, you should have an active remote Windows PowerShell session toohello device.</span></span>
   
![Vzdálená komunikace prostředí PowerShell pomocí protokolu HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a><span data-ttu-id="96419-169">Připojení přes protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="96419-169">Connect through HTTPS</span></span>

<span data-ttu-id="96419-170">Připojení tooWindows Powershellu pro StorSimple prostřednictvím relace HTTPS je hello nejbezpečnější a doporučená metoda zařízení Microsoft Azure StorSimple pro vzdálené připojení tooyour.</span><span class="sxs-lookup"><span data-stu-id="96419-170">Connecting tooWindows PowerShell for StorSimple through an HTTPS session is hello most secure and recommended method of remotely connecting tooyour Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="96419-171">Hello následující postupy popisují, jak tooset až hello sériové konzoly a klientské počítače tak, aby HTTPS tooconnect tooWindows prostředí PowerShell můžete použít pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="96419-171">hello following procedures explain how tooset up hello serial console and client computers so that you can use HTTPS tooconnect tooWindows PowerShell for StorSimple.</span></span>

<span data-ttu-id="96419-172">Můžete použít buď hello portálu nebo hello konzoly sériového portu tooconfigure vzdálenou správu Azure.</span><span class="sxs-lookup"><span data-stu-id="96419-172">You can use either hello Azure portal or hello serial console tooconfigure remote management.</span></span> <span data-ttu-id="96419-173">Vyberte z následujících postupů hello:</span><span class="sxs-lookup"><span data-stu-id="96419-173">Select from hello following procedures:</span></span>

* [<span data-ttu-id="96419-174">Použít hello Azure portálu tooenable vzdálené správy přes protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="96419-174">Use hello Azure portal tooenable remote management over HTTPS</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [<span data-ttu-id="96419-175">Použít hello konzoly sériového portu tooenable vzdálené správy přes protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="96419-175">Use hello serial console tooenable remote management over HTTPS</span></span>](#use-the-serial-console-to-enable-remote-management-over-https)

<span data-ttu-id="96419-176">Po povolení vzdálené správy, použijte následující postupy tooprepare hello hostitele pro vzdálenou správu hello a připojení zařízení toohello hello vzdáleného hostitele.</span><span class="sxs-lookup"><span data-stu-id="96419-176">After you enable remote management, use hello following procedures tooprepare hello host for a remote management and connect toohello device from hello remote host.</span></span>

* [<span data-ttu-id="96419-177">Příprava pro vzdálenou správu hostitele hello</span><span class="sxs-lookup"><span data-stu-id="96419-177">Prepare hello host for remote management</span></span>](#prepare-the-host-for-remote-management)
* [<span data-ttu-id="96419-178">Připojte zařízení toohello hello vzdáleného hostitele</span><span class="sxs-lookup"><span data-stu-id="96419-178">Connect toohello device from hello remote host</span></span>](#connect-to-the-device-from-the-remote-host)

### <a name="use-hello-azure-portal-tooenable-remote-management-over-https"></a><span data-ttu-id="96419-179">Použít hello Azure portálu tooenable vzdálené správy přes protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="96419-179">Use hello Azure portal tooenable remote management over HTTPS</span></span>

<span data-ttu-id="96419-180">Proveďte následující kroky v hello Azure portálu tooenable vzdálené správy přes protokol HTTPS hello.</span><span class="sxs-lookup"><span data-stu-id="96419-180">Perform hello following steps in hello Azure portal tooenable remote management over HTTPS.</span></span>

#### <a name="tooenable-remote-management-over-https-from-hello-azure-portal"></a><span data-ttu-id="96419-181">tooenable vzdálené správy přes protokol HTTPS z hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="96419-181">tooenable remote management over HTTPS from hello Azure portal</span></span>

1. <span data-ttu-id="96419-182">Přejděte služby StorSimple Manager zařízení tooyour.</span><span class="sxs-lookup"><span data-stu-id="96419-182">Go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="96419-183">Vyberte **zařízení** a pak vyberte a klikněte na zařízení hello chcete tooconfigure pro vzdálenou správu.</span><span class="sxs-lookup"><span data-stu-id="96419-183">Select **Devices** and then select and click hello device you want tooconfigure for remote management.</span></span> <span data-ttu-id="96419-184">Přejděte příliš**nastavení zařízení > zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="96419-184">Go too**Device settings > Security**.</span></span>
2. <span data-ttu-id="96419-185">V hello **nastavení zabezpečení** okně klikněte na tlačítko **vzdálenou správu**.</span><span class="sxs-lookup"><span data-stu-id="96419-185">In hello **Security settings** blade, click **Remote Management**.</span></span>
3. <span data-ttu-id="96419-186">Nastavit **povolit vzdálenou správu** příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="96419-186">Set **Enable Remote Management** too**Yes**.</span></span>
4. <span data-ttu-id="96419-187">Teď můžete zvolit tooconnect pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="96419-187">You can now choose tooconnect using HTTPS.</span></span> <span data-ttu-id="96419-188">(výchozí hello je tooconnect přes protokol HTTPS.) Ujistěte se, že je vybraný protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="96419-188">(hello default is tooconnect over HTTPS.) Make sure that HTTPS is selected.</span></span>
5. <span data-ttu-id="96419-189">Klikněte na tlačítko... a pak klikněte na **Download Remote Management Certificate**.</span><span class="sxs-lookup"><span data-stu-id="96419-189">Click ... and then click **Download Remote Management Certificate**.</span></span> <span data-ttu-id="96419-190">Zadejte umístění toosave tento soubor.</span><span class="sxs-lookup"><span data-stu-id="96419-190">Specify a location toosave this file.</span></span> <span data-ttu-id="96419-191">Je nutné tento certifikát hello klientský nebo hostitelský počítač, že používáte zařízení toohello tooconnect tooinstall.</span><span class="sxs-lookup"><span data-stu-id="96419-191">You need tooinstall this certificate on hello client or host computer that you will use tooconnect toohello device.</span></span>
6. <span data-ttu-id="96419-192">Klikněte na tlačítko **Uložit** a pak klikněte na **Ano** po zobrazení výzvy k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="96419-192">Click **Save** and then click **Yes** when prompted for confirmation.</span></span>

### <a name="use-hello-serial-console-tooenable-remote-management-over-https"></a><span data-ttu-id="96419-193">Použít hello konzoly sériového portu tooenable vzdálené správy přes protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="96419-193">Use hello serial console tooenable remote management over HTTPS</span></span>

<span data-ttu-id="96419-194">Proveďte následující kroky na hello zařízení konzoly sériového portu tooenable vzdálenou správu hello.</span><span class="sxs-lookup"><span data-stu-id="96419-194">Perform hello following steps on hello device serial console tooenable remote management.</span></span>

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a><span data-ttu-id="96419-195">tooenable vzdálenou správu prostřednictvím konzoly sériového portu zařízení hello</span><span class="sxs-lookup"><span data-stu-id="96419-195">tooenable remote management through hello device serial console</span></span>
1. <span data-ttu-id="96419-196">V nabídce konzoly sériového portu hello vyberte možnost 1.</span><span class="sxs-lookup"><span data-stu-id="96419-196">On hello serial console menu, select option 1.</span></span> <span data-ttu-id="96419-197">Další informace o používání konzoly sériového portu hello na hello zařízení, přejděte příliš[připojení tooWindows Powershellu pro StorSimple prostřednictvím konzoly sériového portu zařízení](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="96419-197">For more information about using hello serial console on hello device, go too[Connect tooWindows PowerShell for StorSimple via device serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="96419-198">Hello řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="96419-198">At hello prompt, type:</span></span>
   
     `Enable-HcsRemoteManagement`
   
    <span data-ttu-id="96419-199">To by měl povolit protokol HTTPS na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="96419-199">This should enable HTTPS on your device.</span></span>
3. <span data-ttu-id="96419-200">Ověřte, že je povoleno HTTPS zadáním:</span><span class="sxs-lookup"><span data-stu-id="96419-200">Verify that HTTPS has been enabled by typing:</span></span> 
   
     `Get-HcsSystem`
   
    <span data-ttu-id="96419-201">Ujistěte se, že hello **RemoteManagementMode** pole ukazuje **HttpsEnabled**.hello následující obrázek ukazuje tato nastavení v PuTTY.</span><span class="sxs-lookup"><span data-stu-id="96419-201">Make sure that hello **RemoteManagementMode** field shows **HttpsEnabled**.hello following illustration shows these settings in PuTTY.</span></span>
   
     ![Sériového portu HTTPS povoleno](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. <span data-ttu-id="96419-203">Z výstupu hello `Get-HcsSystem`, zkopírujte sériové číslo zařízení hello hello a uložit pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="96419-203">From hello output of `Get-HcsSystem`, copy hello serial number of hello device and save it for later use.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="96419-204">sériové číslo Hello mapuje toohello název CN certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="96419-204">hello serial number maps toohello CN name in hello certificate.</span></span>
   
5. <span data-ttu-id="96419-205">Získejte certifikát pro vzdálenou správu zadáním:</span><span class="sxs-lookup"><span data-stu-id="96419-205">Obtain a remote management certificate by typing:</span></span> 
   
     `Get-HcsRemoteManagementCert`
   
    <span data-ttu-id="96419-206">Zobrazí se následující toohello podobně jako na certifikát.</span><span class="sxs-lookup"><span data-stu-id="96419-206">A certificate similar toohello following will appear.</span></span>
   
    ![Získání certifikátu vzdálené správy](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. <span data-ttu-id="96419-208">Zkopírujte hello informace v certifikátu hello z **---BEGIN CERTIFICATE---** příliš**---END CERTIFICATE---** do textového editoru, například Poznámkový blok a uložte ho jako soubor .cer.</span><span class="sxs-lookup"><span data-stu-id="96419-208">Copy hello information in hello certificate from **-----BEGIN CERTIFICATE-----** too**-----END CERTIFICATE-----** into a text editor such as Notepad, and save it as a .cer file.</span></span> <span data-ttu-id="96419-209">(Tento soubor tooyour vzdálený hostitel se zkopírujte při přípravě hello hostitele.)</span><span class="sxs-lookup"><span data-stu-id="96419-209">(You will copy this file tooyour remote host when you prepare hello host.)</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="96419-210">toogenerate nový certifikát, použijte hello `Set-HcsRemoteManagementCert` rutiny.</span><span class="sxs-lookup"><span data-stu-id="96419-210">toogenerate a new certificate, use hello `Set-HcsRemoteManagementCert` cmdlet.</span></span>
   
### <a name="prepare-hello-host-for-remote-management"></a><span data-ttu-id="96419-211">Příprava pro vzdálenou správu hostitele hello</span><span class="sxs-lookup"><span data-stu-id="96419-211">Prepare hello host for remote management</span></span>

<span data-ttu-id="96419-212">tooprepare hello hostitelského počítače pro vzdálené připojení, který používá relace HTTPS, proveďte následující postupy hello:</span><span class="sxs-lookup"><span data-stu-id="96419-212">tooprepare hello host computer for a remote connection that uses an HTTPS session, perform hello following procedures:</span></span>

* <span data-ttu-id="96419-213">[Soubor .cer hello importu do kořenového úložiště hello hello klienta nebo vzdálený hostitel](#to-import-the-certificate-on-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="96419-213">[Import hello .cer file into hello root store of hello client or remote host](#to-import-the-certificate-on-the-remote-host).</span></span>
* <span data-ttu-id="96419-214">[Přidání souboru hostitelů hello zařízení sériová čísla toohello na vzdáleného hostitele](#to-add-device-serial-numbers-to-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="96419-214">[Add hello device serial numbers toohello hosts file on your remote host](#to-add-device-serial-numbers-to-the-remote-host).</span></span>

<span data-ttu-id="96419-215">Každá z předchozích postupech hello je popsána níže.</span><span class="sxs-lookup"><span data-stu-id="96419-215">Each of hello preceding procedures, is described below.</span></span>

#### <a name="tooimport-hello-certificate-on-hello-remote-host"></a><span data-ttu-id="96419-216">tooimport hello certifikát u vzdáleného hostitele hello</span><span class="sxs-lookup"><span data-stu-id="96419-216">tooimport hello certificate on hello remote host</span></span>
1. <span data-ttu-id="96419-217">Klikněte pravým tlačítkem na soubor .cer hello a vyberte **instalace certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="96419-217">Right-click hello .cer file and select **Install certificate**.</span></span> <span data-ttu-id="96419-218">Tím se spustí hello Průvodce importem certifikátu.</span><span class="sxs-lookup"><span data-stu-id="96419-218">This starts hello Certificate Import Wizard.</span></span>
   
    ![Průvodce importem certifikátu 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. <span data-ttu-id="96419-220">Pro **umístění úložiště**, vyberte **místního počítače**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="96419-220">For **Store location**, select **Local Machine**, and then click **Next**.</span></span>
3. <span data-ttu-id="96419-221">Vyberte **všechny certifikáty umístit v následujícím úložiště hello**a potom klikněte na **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="96419-221">Select **Place all certificates in hello following store**, and then click **Browse**.</span></span> <span data-ttu-id="96419-222">Přejděte toohello kořenového úložiště vzdáleného hostitele a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="96419-222">Navigate toohello root store of your remote host, and then click **Next**.</span></span>
   
    ![Průvodce importem certifikátu 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. <span data-ttu-id="96419-224">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="96419-224">Click **Finish**.</span></span> <span data-ttu-id="96419-225">Zobrazí zpráva informující o úspěšném importu hello.</span><span class="sxs-lookup"><span data-stu-id="96419-225">A message that tells you that hello import was successful appears.</span></span>
   
    ![Průvodce importem certifikátu 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="tooadd-device-serial-numbers-toohello-remote-host"></a><span data-ttu-id="96419-227">tooadd zařízení sériová čísla toohello vzdáleného hostitele</span><span class="sxs-lookup"><span data-stu-id="96419-227">tooadd device serial numbers toohello remote host</span></span>
1. <span data-ttu-id="96419-228">Spusťte jako správce program Poznámkový blok a pak otevřete soubor hosts hello umístěný ve \Windows\System32\Drivers\etc.</span><span class="sxs-lookup"><span data-stu-id="96419-228">Start Notepad as an administrator, and then open hello hosts file located at \Windows\System32\Drivers\etc.</span></span>
2. <span data-ttu-id="96419-229">Přidejte následující tři položky tooyour hostitele soubor hello: **DATA 0 IP adresu**, **řadič 0 pevné IP adresy**, a **řadič 1 pevné IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="96419-229">Add hello following three entries tooyour hosts file: **DATA 0 IP address**, **Controller 0 Fixed IP address**, and **Controller 1 Fixed IP address**.</span></span>
3. <span data-ttu-id="96419-230">Zadejte sériové číslo zařízení hello, který jste předtím uložili.</span><span class="sxs-lookup"><span data-stu-id="96419-230">Enter hello device serial number that you saved earlier.</span></span> <span data-ttu-id="96419-231">Tato IP adresa toohello mapování, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="96419-231">Map this toohello IP address as shown in hello following image.</span></span> <span data-ttu-id="96419-232">Pro řadič 0 a řadič 1 připojit **Controller0** a **Controller1** na konci hello hello sériové číslo (CN název).</span><span class="sxs-lookup"><span data-stu-id="96419-232">For Controller 0 and Controller 1, append **Controller0** and **Controller1** at hello end of hello serial number (CN name).</span></span>
   
    ![Přidání souboru toohosts název CN](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. <span data-ttu-id="96419-234">Uložení souboru hostitelů hello.</span><span class="sxs-lookup"><span data-stu-id="96419-234">Save hello hosts file.</span></span>

### <a name="connect-toohello-device-from-hello-remote-host"></a><span data-ttu-id="96419-235">Připojte zařízení toohello hello vzdáleného hostitele</span><span class="sxs-lookup"><span data-stu-id="96419-235">Connect toohello device from hello remote host</span></span>

<span data-ttu-id="96419-236">Pomocí prostředí Windows PowerShell a SSL tooenter na SSAdmin relaci na vašem zařízení ze vzdáleného hostitele nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="96419-236">Use Windows PowerShell and SSL tooenter an SSAdmin session on your device from a remote host or client.</span></span> <span data-ttu-id="96419-237">relace SSAdmin Hello mapuje toooption 1 v hello [konzoly sériového portu](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) nabídky vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="96419-237">hello SSAdmin session maps toooption 1 in hello [serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menu of your device.</span></span>

<span data-ttu-id="96419-238">Proveďte následující postup u hello počítačů, ze kterého mají být připojení ke vzdálené prostředí Windows PowerShell toomake hello hello.</span><span class="sxs-lookup"><span data-stu-id="96419-238">Perform hello following procedure on hello computer from which you want toomake hello remote Windows PowerShell connection.</span></span>

#### <a name="tooenter-an-ssadmin-session-on-hello-device-by-using-windows-powershell-and-ssl"></a><span data-ttu-id="96419-239">tooenter na relaci SSAdmin na zařízení hello pomocí prostředí Windows PowerShell a SSL</span><span class="sxs-lookup"><span data-stu-id="96419-239">tooenter an SSAdmin session on hello device by using Windows PowerShell and SSL</span></span>
1. <span data-ttu-id="96419-240">Spusťte relaci prostředí Windows PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="96419-240">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="96419-241">Přidání důvěryhodných hostitelů hello zařízení IP adres toohello klienta zadáním:</span><span class="sxs-lookup"><span data-stu-id="96419-241">Add hello device IP address toohello client’s trusted hosts by typing:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    <span data-ttu-id="96419-242">Kde <*device_ip*> je hello IP adresa zařízení, třeba:</span><span class="sxs-lookup"><span data-stu-id="96419-242">Where <*device_ip*> is hello IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="96419-243">toocreate nových přihlašovacích údajů, zadejte:</span><span class="sxs-lookup"><span data-stu-id="96419-243">toocreate a new credential, type:</span></span>
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    <span data-ttu-id="96419-244">Kde <*IP cílové zařízení*> je IP adresa hello DATA 0 pro vaše zařízení; například **10.126.173.90** jak je znázorněno v předchozích bitovou kopii souboru hostitelů hello hello.</span><span class="sxs-lookup"><span data-stu-id="96419-244">Where <*IP of target device*> is hello IP address of DATA 0 for your device; for example, **10.126.173.90** as shown in hello preceding image of hello hosts file.</span></span> <span data-ttu-id="96419-245">Zadejte také hello heslo správce pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="96419-245">Also, supply hello administrator password for your device.</span></span>
4. <span data-ttu-id="96419-246">Vytvořte relaci zadáním:</span><span class="sxs-lookup"><span data-stu-id="96419-246">Create a session by typing:</span></span>
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    <span data-ttu-id="96419-247">Pro parametr - ComputerName hello v hello rutiny, zadejte hello <*sériové číslo cílového zařízení*>.</span><span class="sxs-lookup"><span data-stu-id="96419-247">For hello -ComputerName parameter in hello cmdlet, provide hello <*serial number of target device*>.</span></span> <span data-ttu-id="96419-248">Toto sériové číslo bylo mapováno toohello IP adresu DATA 0 v souboru hostitelů hello na vzdáleném hostiteli; například **SHX0991003G44MT** jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="96419-248">This serial number was mapped toohello IP address of DATA 0 in hello hosts file on your remote host; for example, **SHX0991003G44MT** as shown in hello following image.</span></span>
5. <span data-ttu-id="96419-249">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="96419-249">Type:</span></span>
   
     `Enter-PSSession $session`
6. <span data-ttu-id="96419-250">Budete potřebovat toowait několik minut a pak bude zařízení připojených tooyour přes HTTPS přes protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="96419-250">You will need toowait a few minutes, and then you will be connected tooyour device via HTTPS over SSL.</span></span> <span data-ttu-id="96419-251">Zobrazí zprávu, která označuje, že jsou připojené tooyour zařízení.</span><span class="sxs-lookup"><span data-stu-id="96419-251">You see a message that indicates you are connected tooyour device.</span></span>
   
    ![Vzdálená komunikace prostředí PowerShell pomocí protokolu HTTPS a SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a><span data-ttu-id="96419-253">Další kroky</span><span class="sxs-lookup"><span data-stu-id="96419-253">Next steps</span></span>

* <span data-ttu-id="96419-254">Další informace o [pomocí prostředí Windows PowerShell tooadminister zařízení StorSimple](storsimple-8000-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="96419-254">Learn more about [using Windows PowerShell tooadminister your StorSimple device](storsimple-8000-windows-powershell-administration.md).</span></span>
* <span data-ttu-id="96419-255">Další informace o [pomocí hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="96419-255">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

