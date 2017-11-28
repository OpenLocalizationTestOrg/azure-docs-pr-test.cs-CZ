---
title: "aaaConnect vzdáleně tooyour zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak tooconfigure zařízení pro vzdálenou správu a jak tooconnect tooWindows Powershellu pro StorSimple prostřednictvím protokolu HTTP nebo HTTPS."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 923377aa-f451-4656-87de-5e95a34a6a2a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 55ed8fcdd997901301e0adc164a302216cde0332
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-remotely-tooyour-storsimple-8000-series-device"></a><span data-ttu-id="5a5c8-103">Vzdálené připojení zařízení řady StorSimple 8000 tooyour</span><span class="sxs-lookup"><span data-stu-id="5a5c8-103">Connect remotely tooyour StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="5a5c8-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="5a5c8-104">Overview</span></span>
<span data-ttu-id="5a5c8-105">Můžete použít zařízení StorSimple tooyour tooconnect vzdálenou komunikaci prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-105">You can use Windows PowerShell remoting tooconnect tooyour StorSimple device.</span></span> <span data-ttu-id="5a5c8-106">Když připojíte tímto způsobem, neuvidíte nabídky.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-106">When you connect this way, you will not see a menu.</span></span> <span data-ttu-id="5a5c8-107">(Zobrazí nabídky pouze v případě, že používáte hello konzoly sériového portu v zařízení tooconnect hello.) Díky vzdálené komunikace Windows Powershellu připojit tooa konkrétní prostředí runspace.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-107">(You see a menu only if you use hello serial console on hello device tooconnect.) With Windows PowerShell remoting, you connect tooa specific runspace.</span></span> <span data-ttu-id="5a5c8-108">Můžete také zadat hello jazyk zobrazení.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-108">You can also specify hello display language.</span></span> 

<span data-ttu-id="5a5c8-109">Další informace o použití toomanage vzdálenou komunikaci prostředí Windows PowerShell zařízení, přejděte příliš[pomocí Windows Powershellu pro StorSimple tooadminister zařízení StorSimple](storsimple-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="5a5c8-109">For more information about using Windows PowerShell remoting toomanage your device, go too[Use Windows PowerShell for StorSimple tooadminister your StorSimple device](storsimple-windows-powershell-administration.md).</span></span>

<span data-ttu-id="5a5c8-110">Tento kurz vysvětluje, jak tooconfigure zařízení pro vzdálenou správu a pak tooconnect tooWindows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-110">This tutorial explains how tooconfigure your device for remote management and then how tooconnect tooWindows PowerShell for StorSimple.</span></span> <span data-ttu-id="5a5c8-111">Pomocí protokolu HTTP nebo HTTPS tooconnect přes vzdálenou komunikaci prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-111">You can use HTTP or HTTPS tooconnect via Windows PowerShell remoting.</span></span> <span data-ttu-id="5a5c8-112">Ale při rozhodování jak tooconnect tooWindows Powershellu pro StorSimple, zvažte následující hello:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-112">However, when you are deciding how tooconnect tooWindows PowerShell for StorSimple, consider hello following:</span></span> 

* <span data-ttu-id="5a5c8-113">Připojení přímo toohello konzoly sériového portu zařízení je bezpečné, ale není připojování konzoly sériového portu toohello přes síťové přepínače.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-113">Connecting directly toohello device serial console is secure, but connecting toohello serial console over network switches is not.</span></span> <span data-ttu-id="5a5c8-114">Buďte opatrní hello rizika zabezpečení při připojování konzoly sériového portu zařízení toohello přes síťové přepínače.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-114">Be cautious of hello security risk when connecting toohello device serial console over network switches.</span></span> 
* <span data-ttu-id="5a5c8-115">Připojení přes relaci protokolu HTTP může nabízí lepší zabezpečení než připojení prostřednictvím sériové konzoly hello přes síť hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-115">Connecting through an HTTP session might offer more security than connecting through hello serial console over hello network.</span></span> <span data-ttu-id="5a5c8-116">I když to není hello nejbezpečnější metodou, je přijatelné v důvěryhodných sítích.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-116">Although this is not hello most secure method, it is acceptable on trusted networks.</span></span> 
* <span data-ttu-id="5a5c8-117">Připojení prostřednictvím relace HTTPS se certifikát podepsaný svým držitelem je hello nejbezpečnější a doporučená možnost hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-117">Connecting through an HTTPS session with a self-signed certificate is hello most secure and hello recommended option.</span></span>

<span data-ttu-id="5a5c8-118">Můžete vzdáleně připojit toohello rozhraní Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-118">You can connect remotely toohello Windows PowerShell interface.</span></span> <span data-ttu-id="5a5c8-119">Zařízení StorSimple tooyour vzdáleného přístupu prostřednictvím rozhraní Windows PowerShell hello však není povoleno ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-119">However, remote access tooyour StorSimple device via hello Windows PowerShell interface is not enabled by default.</span></span> <span data-ttu-id="5a5c8-120">Je třeba tooenable vzdálené správy na zařízení hello první a potom na hello klienta, který je použité tooaccess zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-120">You need tooenable remote management on hello device first, and then on hello client that is used tooaccess your device.</span></span>

<span data-ttu-id="5a5c8-121">Hello kroky popsané v tomto článku byly provedeny v hostitelském systému, systémem Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-121">hello steps described in this article were performed on a host system running Windows Server 2012 R2.</span></span>

## <a name="connect-through-http"></a><span data-ttu-id="5a5c8-122">Připojení prostřednictvím protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="5a5c8-122">Connect through HTTP</span></span>
<span data-ttu-id="5a5c8-123">Připojení tooWindows Powershellu pro StorSimple přes relaci protokolu HTTP poskytuje lepší zabezpečení než připojení prostřednictvím sériové konzoly hello zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-123">Connecting tooWindows PowerShell for StorSimple through an HTTP session offers more security than connecting through hello serial console of your StorSimple device.</span></span> <span data-ttu-id="5a5c8-124">I když to není hello nejbezpečnější metodou, je přijatelné v důvěryhodných sítích.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-124">Although this is not hello most secure method, it is acceptable on trusted networks.</span></span>

<span data-ttu-id="5a5c8-125">Můžete použít buď hello portál Azure classic nebo vzdálenou správu tooconfigure hello konzoly sériového portu.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-125">You can use either hello Azure classic portal or hello serial console tooconfigure remote management.</span></span> <span data-ttu-id="5a5c8-126">Vyberte z následujících postupů hello:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-126">Select from hello following procedures:</span></span>

* [<span data-ttu-id="5a5c8-127">Používání hello Azure classic portálu tooenable vzdálené správy přes protokol HTTP</span><span class="sxs-lookup"><span data-stu-id="5a5c8-127">Use hello Azure classic portal tooenable remote management over HTTP</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [<span data-ttu-id="5a5c8-128">Používání hello konzoly sériového portu tooenable vzdálené správy přes protokol HTTP</span><span class="sxs-lookup"><span data-stu-id="5a5c8-128">Use hello serial console tooenable remote management over HTTP</span></span>](#use-the-serial-console-to-enable-remote-management-over-http)

<span data-ttu-id="5a5c8-129">Po povolení vzdálené správy, použijte následující postup tooprepare hello klienta pro připojení ke vzdálené hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-129">After you enable remote management, use hello following procedure tooprepare hello client for a remote connection.</span></span>

* [<span data-ttu-id="5a5c8-130">Připravit hello klienta pro připojení ke vzdálené</span><span class="sxs-lookup"><span data-stu-id="5a5c8-130">Prepare hello client for remote connection</span></span>](#prepare-the-client-for-remote-connection)

### <a name="use-hello-azure-classic-portal-tooenable-remote-management-over-http"></a><span data-ttu-id="5a5c8-131">Používání hello Azure classic portálu tooenable vzdálené správy přes protokol HTTP</span><span class="sxs-lookup"><span data-stu-id="5a5c8-131">Use hello Azure classic portal tooenable remote management over HTTP</span></span>
<span data-ttu-id="5a5c8-132">Proveďte následující kroky v hello Azure classic portálu tooenable vzdálené správy přes protokol HTTP hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-132">Perform hello following steps in hello Azure classic portal tooenable remote management over HTTP.</span></span>

#### <a name="tooenable-remote-management-through-hello-azure-classic-portal"></a><span data-ttu-id="5a5c8-133">tooenable vzdálenou správu prostřednictvím hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="5a5c8-133">tooenable remote management through hello Azure classic portal</span></span>
1. <span data-ttu-id="5a5c8-134">Přístup k **zařízení** > **konfigurace** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-134">Access **Devices** > **Configure** for your device.</span></span>
2. <span data-ttu-id="5a5c8-135">Projděte dolů toohello **vzdálenou správu** části.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-135">Scroll down toohello **Remote Management** section.</span></span>
3. <span data-ttu-id="5a5c8-136">Nastavit **povolit vzdálenou správu** příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-136">Set **Enable Remote Management** too**Yes**.</span></span>
4. <span data-ttu-id="5a5c8-137">Teď můžete zvolit tooconnect pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-137">You can now choose tooconnect using HTTP.</span></span> <span data-ttu-id="5a5c8-138">(výchozí hello je tooconnect přes protokol HTTPS.) Ujistěte se, že je vybraný HTTP.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-138">(hello default is tooconnect over HTTPS.) Make sure that HTTP is selected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="5a5c8-139">Připojení pomocí protokolu HTTP je přijatelné jenom v důvěryhodných sítích.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-139">Connecting over HTTP is acceptable only on trusted networks.</span></span>
   > 
   > 
5. <span data-ttu-id="5a5c8-140">Klikněte na tlačítko **Uložit** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-140">Click **Save** at hello bottom of hello page.</span></span>

### <a name="use-hello-serial-console-tooenable-remote-management-over-http"></a><span data-ttu-id="5a5c8-141">Používání hello konzoly sériového portu tooenable vzdálené správy přes protokol HTTP</span><span class="sxs-lookup"><span data-stu-id="5a5c8-141">Use hello serial console tooenable remote management over HTTP</span></span>
<span data-ttu-id="5a5c8-142">Proveďte následující kroky na hello zařízení konzoly sériového portu tooenable vzdálenou správu hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-142">Perform hello following steps on hello device serial console tooenable remote management.</span></span>

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a><span data-ttu-id="5a5c8-143">tooenable vzdálenou správu prostřednictvím konzoly sériového portu zařízení hello</span><span class="sxs-lookup"><span data-stu-id="5a5c8-143">tooenable remote management through hello device serial console</span></span>
1. <span data-ttu-id="5a5c8-144">V nabídce konzoly sériového portu hello vyberte možnost 1.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-144">On hello serial console menu, select option 1.</span></span> <span data-ttu-id="5a5c8-145">Další informace o používání konzoly sériového portu hello na hello zařízení, přejděte příliš[připojení tooWindows Powershellu pro StorSimple prostřednictvím konzoly sériového portu zařízení](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="5a5c8-145">For more information about using hello serial console on hello device, go too[Connect tooWindows PowerShell for StorSimple via device serial console](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="5a5c8-146">Hello řádku zadejte:`Enable-HcsRemoteManagement –AllowHttp`</span><span class="sxs-lookup"><span data-stu-id="5a5c8-146">At hello prompt, type: `Enable-HcsRemoteManagement –AllowHttp`</span></span>
3. <span data-ttu-id="5a5c8-147">Budete informováni o ohrožení zabezpečení hello pomocí protokolu HTTP tooconnect toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-147">You will be notified about hello security vulnerabilities of using HTTP tooconnect toohello device.</span></span> <span data-ttu-id="5a5c8-148">Po zobrazení výzvy potvrďte zadáním **Y**.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-148">When prompted, confirm by typing **Y**.</span></span>
4. <span data-ttu-id="5a5c8-149">Ověřte, že je povolený protokol HTTP zadáním:`Get-HcsSystem`</span><span class="sxs-lookup"><span data-stu-id="5a5c8-149">Verify that HTTP is enabled by typing: `Get-HcsSystem`</span></span>
5. <span data-ttu-id="5a5c8-150">Ověřte, že hello **RemoteManagementMode** pole ukazuje **HttpsAndHttpEnabled**.hello následující obrázek ukazuje tato nastavení v PuTTY.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-150">Verify that hello **RemoteManagementMode** field shows **HttpsAndHttpEnabled**.hello following illustration shows these settings in PuTTY.</span></span>
   
     ![Sériového portu HTTPS a HTTP povoleno](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-hello-client-for-remote-connection"></a><span data-ttu-id="5a5c8-152">Připravit hello klienta pro připojení ke vzdálené</span><span class="sxs-lookup"><span data-stu-id="5a5c8-152">Prepare hello client for remote connection</span></span>
<span data-ttu-id="5a5c8-153">Proveďte následující kroky na vzdálenou správu serveru signálu hello klienta tooenable hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-153">Perform hello following steps on hello client tooenable remote management.</span></span>

#### <a name="tooprepare-hello-client-for-remote-connection"></a><span data-ttu-id="5a5c8-154">tooprepare hello klienta pro připojení ke vzdálené</span><span class="sxs-lookup"><span data-stu-id="5a5c8-154">tooprepare hello client for remote connection</span></span>
1. <span data-ttu-id="5a5c8-155">Spusťte relaci prostředí Windows PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-155">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="5a5c8-156">Zadejte následující příkaz tooadd hello IP adresu ze seznamu důvěryhodných hostitelů hello StorSimple zařízení toohello klienta hello:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-156">Type hello following command tooadd hello IP address of hello StorSimple device toohello client’s trusted hosts list:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     <span data-ttu-id="5a5c8-157">Nahraďte <*device_ip*> s hello IP adresa vašeho zařízení; například:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-157">Replace <*device_ip*> with hello IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="5a5c8-158">Zadejte následující příkaz toosave hello přihlašovací údaje zařízení v proměnné hello:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-158">Type hello following command toosave hello device credentials in a variable:</span></span> 
   
    ```
    $cred = Get-Credential
    ```
    
4. <span data-ttu-id="5a5c8-159">V dialogovém okně hello které se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-159">In hello dialog box that appears:</span></span>
   
   1. <span data-ttu-id="5a5c8-160">Zadejte jméno uživatele hello v tomto formátu: *device_ip\SSAdmin*.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-160">Type hello user name in this format: *device_ip\SSAdmin*.</span></span>
   2. <span data-ttu-id="5a5c8-161">Zadejte heslo správce zařízení hello, který byl nastaven při konfiguraci zařízení hello pomocí Průvodce instalací hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-161">Type hello device administrator password that was set when hello device was configured with hello setup wizard.</span></span> <span data-ttu-id="5a5c8-162">výchozí heslo Hello je *Heslo1*.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-162">hello default password is *Password1*.</span></span>
5. <span data-ttu-id="5a5c8-163">Spusťte relaci prostředí Windows PowerShell na zařízení hello zadáním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-163">Start a Windows PowerShell session on hello device by typing this command:</span></span>
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > <span data-ttu-id="5a5c8-164">toocreate relaci prostředí Windows PowerShell pro použití s hello virtuálního zařízení StorSimple, připojit hello `–Port` parametr a zadejte hello veřejný port, který jste nakonfigurovali v vzdálenou komunikaci pro virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-164">toocreate a Windows PowerShell session for use with hello StorSimple virtual device, append hello `–Port` parameter and specify hello public port that you configured in Remoting for StorSimple Virtual Appliance.</span></span>
   > 
   > 
   
     <span data-ttu-id="5a5c8-165">V tomto okamžiku by měl mít zařízení aktivní toohello vzdálené relace prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-165">At this point, you should have an active remote Windows PowerShell session toohello device.</span></span>
   
    ![Vzdálená komunikace prostředí PowerShell pomocí protokolu HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a><span data-ttu-id="5a5c8-167">Připojení přes protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="5a5c8-167">Connect through HTTPS</span></span>
<span data-ttu-id="5a5c8-168">Připojení tooWindows Powershellu pro StorSimple prostřednictvím relace HTTPS je hello nejbezpečnější a doporučená metoda zařízení Microsoft Azure StorSimple pro vzdálené připojení tooyour.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-168">Connecting tooWindows PowerShell for StorSimple through an HTTPS session is hello most secure and recommended method of remotely connecting tooyour Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="5a5c8-169">Hello následující postupy popisují, jak tooset až hello sériové konzoly a klientské počítače tak, aby HTTPS tooconnect tooWindows prostředí PowerShell můžete použít pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-169">hello following procedures explain how tooset up hello serial console and client computers so that you can use HTTPS tooconnect tooWindows PowerShell for StorSimple.</span></span>

<span data-ttu-id="5a5c8-170">Můžete použít buď hello portál Azure classic nebo vzdálenou správu tooconfigure hello konzoly sériového portu.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-170">You can use either hello Azure classic portal or hello serial console tooconfigure remote management.</span></span> <span data-ttu-id="5a5c8-171">Vyberte z následujících postupů hello:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-171">Select from hello following procedures:</span></span>

* [<span data-ttu-id="5a5c8-172">Použít vzdálenou správu Azure classic portálu tooenable hello přes protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="5a5c8-172">Use hello Azure classic portal tooenable remote management over HTTPS</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [<span data-ttu-id="5a5c8-173">Použít hello konzoly sériového portu tooenable vzdálené správy přes protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="5a5c8-173">Use hello serial console tooenable remote management over HTTPS</span></span>](#use-the-serial-console-to-enable-remote-management-over-https)

<span data-ttu-id="5a5c8-174">Po povolení vzdálené správy, použijte následující postupy tooprepare hello hostitele pro vzdálenou správu hello a připojení zařízení toohello hello vzdáleného hostitele.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-174">After you enable remote management, use hello following procedures tooprepare hello host for a remote management and connect toohello device from hello remote host.</span></span>

* [<span data-ttu-id="5a5c8-175">Příprava pro vzdálenou správu hostitele hello</span><span class="sxs-lookup"><span data-stu-id="5a5c8-175">Prepare hello host for remote management</span></span>](#prepare-the-host-for-remote-management)
* [<span data-ttu-id="5a5c8-176">Připojte zařízení toohello hello vzdáleného hostitele</span><span class="sxs-lookup"><span data-stu-id="5a5c8-176">Connect toohello device from hello remote host</span></span>](#connect-to-the-device-from-the-remote-host)

### <a name="use-hello-azure-classic-portal-tooenable-remote-management-over-https"></a><span data-ttu-id="5a5c8-177">Použít vzdálenou správu Azure classic portálu tooenable hello přes protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="5a5c8-177">Use hello Azure classic portal tooenable remote management over HTTPS</span></span>
<span data-ttu-id="5a5c8-178">Proveďte následující kroky v hello Azure classic portálu tooenable vzdálené správy přes protokol HTTPS hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-178">Perform hello following steps in hello Azure classic portal tooenable remote management over HTTPS.</span></span>

#### <a name="tooenable-remote-management-over-https-from-hello-azure-classic-portal"></a><span data-ttu-id="5a5c8-179">tooenable vzdálené správy přes protokol HTTPS z hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="5a5c8-179">tooenable remote management over HTTPS from hello Azure classic portal</span></span>
1. <span data-ttu-id="5a5c8-180">Přístup k **zařízení** > **konfigurace** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-180">Access **Devices** > **Configure** for your device.</span></span>
2. <span data-ttu-id="5a5c8-181">Projděte dolů toohello **vzdálenou správu** části.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-181">Scroll down toohello **Remote Management** section.</span></span>
3. <span data-ttu-id="5a5c8-182">Nastavit **povolit vzdálenou správu** příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-182">Set **Enable Remote Management** too**Yes**.</span></span>
4. <span data-ttu-id="5a5c8-183">Teď můžete zvolit tooconnect pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-183">You can now choose tooconnect using HTTPS.</span></span> <span data-ttu-id="5a5c8-184">(výchozí hello je tooconnect přes protokol HTTPS.) Ujistěte se, že je vybraný protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-184">(hello default is tooconnect over HTTPS.) Make sure that HTTPS is selected.</span></span> 
5. <span data-ttu-id="5a5c8-185">Klikněte na tlačítko **stáhnout certifikát pro vzdálenou správu**.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-185">Click **Download Remote Management Certificate**.</span></span> <span data-ttu-id="5a5c8-186">Zadejte umístění toosave tento soubor.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-186">Specify a location toosave this file.</span></span> <span data-ttu-id="5a5c8-187">Je nutné tooinstall tento certifikát na hello klientský nebo hostitelský počítač, že budete používat tooconnect toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-187">You will need tooinstall this certificate on hello client or host computer that you will use tooconnect toohello device.</span></span>
6. <span data-ttu-id="5a5c8-188">Klikněte na tlačítko **Uložit** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-188">Click **Save** at hello bottom of hello page.</span></span>

### <a name="use-hello-serial-console-tooenable-remote-management-over-https"></a><span data-ttu-id="5a5c8-189">Použít hello konzoly sériového portu tooenable vzdálené správy přes protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="5a5c8-189">Use hello serial console tooenable remote management over HTTPS</span></span>
<span data-ttu-id="5a5c8-190">Proveďte následující kroky na hello zařízení konzoly sériového portu tooenable vzdálenou správu hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-190">Perform hello following steps on hello device serial console tooenable remote management.</span></span>

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a><span data-ttu-id="5a5c8-191">tooenable vzdálenou správu prostřednictvím konzoly sériového portu zařízení hello</span><span class="sxs-lookup"><span data-stu-id="5a5c8-191">tooenable remote management through hello device serial console</span></span>
1. <span data-ttu-id="5a5c8-192">V nabídce konzoly sériového portu hello vyberte možnost 1.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-192">On hello serial console menu, select option 1.</span></span> <span data-ttu-id="5a5c8-193">Další informace o používání konzoly sériového portu hello na hello zařízení, přejděte příliš[připojení tooWindows Powershellu pro StorSimple prostřednictvím konzoly sériového portu zařízení](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="5a5c8-193">For more information about using hello serial console on hello device, go too[Connect tooWindows PowerShell for StorSimple via device serial console](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="5a5c8-194">Hello řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-194">At hello prompt, type:</span></span> 
   
     `Enable-HcsRemoteManagement`
   
    <span data-ttu-id="5a5c8-195">To by měl povolit protokol HTTPS na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-195">This should enable HTTPS on your device.</span></span>
3. <span data-ttu-id="5a5c8-196">Ověřte, že je povoleno HTTPS zadáním:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-196">Verify that HTTPS has been enabled by typing:</span></span> 
   
     `Get-HcsSystem`
   
    <span data-ttu-id="5a5c8-197">Ujistěte se, že hello **RemoteManagementMode** pole ukazuje **HttpsEnabled**.hello následující obrázek ukazuje tato nastavení v PuTTY.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-197">Make sure that hello **RemoteManagementMode** field shows **HttpsEnabled**.hello following illustration shows these settings in PuTTY.</span></span>
   
     ![Sériového portu HTTPS povoleno](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. <span data-ttu-id="5a5c8-199">Z výstupu hello `Get-HcsSystem`, zkopírujte sériové číslo zařízení hello hello a uložit pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-199">From hello output of `Get-HcsSystem`, copy hello serial number of hello device and save it for later use.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="5a5c8-200">sériové číslo Hello mapuje toohello název CN certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-200">hello serial number maps toohello CN name in hello certificate.</span></span>
   > 
   > 
5. <span data-ttu-id="5a5c8-201">Získejte certifikát pro vzdálenou správu zadáním:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-201">Obtain a remote management certificate by typing:</span></span> 
   
     `Get-HcsRemoteManagementCert`
   
    <span data-ttu-id="5a5c8-202">Zobrazí se následující toohello podobně jako na certifikát.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-202">A certificate similar toohello following will appear.</span></span>
   
    ![Získání certifikátu vzdálené správy](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. <span data-ttu-id="5a5c8-204">Zkopírujte hello informace v certifikátu hello z **---BEGIN CERTIFICATE---** příliš**---END CERTIFICATE---** do textového editoru, například Poznámkový blok a uložte ho jako soubor .cer.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-204">Copy hello information in hello certificate from **-----BEGIN CERTIFICATE-----** too**-----END CERTIFICATE-----** into a text editor such as Notepad, and save it as a .cer file.</span></span> <span data-ttu-id="5a5c8-205">(Tento soubor tooyour vzdálený hostitel se zkopírujte při přípravě hello hostitele.)</span><span class="sxs-lookup"><span data-stu-id="5a5c8-205">(You will copy this file tooyour remote host when you prepare hello host.)</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="5a5c8-206">toogenerate nový certifikát, použijte hello `Set-HcsRemoteManagementCert` rutiny.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-206">toogenerate a new certificate, use hello `Set-HcsRemoteManagementCert` cmdlet.</span></span>
   > 
   > 

### <a name="prepare-hello-host-for-remote-management"></a><span data-ttu-id="5a5c8-207">Příprava pro vzdálenou správu hostitele hello</span><span class="sxs-lookup"><span data-stu-id="5a5c8-207">Prepare hello host for remote management</span></span>
<span data-ttu-id="5a5c8-208">tooprepare hello hostitelského počítače pro vzdálené připojení, který používá relace HTTPS, proveďte následující postupy hello:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-208">tooprepare hello host computer for a remote connection that uses an HTTPS session, perform hello following procedures:</span></span>

* <span data-ttu-id="5a5c8-209">[Soubor .cer hello importu do kořenového úložiště hello hello klienta nebo vzdálený hostitel](#to-import-the-certificate-on-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="5a5c8-209">[Import hello .cer file into hello root store of hello client or remote host](#to-import-the-certificate-on-the-remote-host).</span></span>
* <span data-ttu-id="5a5c8-210">[Přidání souboru hostitelů hello zařízení sériová čísla toohello na vzdáleného hostitele](#to-add-device-serial-numbers-to-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="5a5c8-210">[Add hello device serial numbers toohello hosts file on your remote host](#to-add-device-serial-numbers-to-the-remote-host).</span></span>

<span data-ttu-id="5a5c8-211">Každá z těchto postupů je popsána níže.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-211">Each of these procedures is described below.</span></span>

#### <a name="tooimport-hello-certificate-on-hello-remote-host"></a><span data-ttu-id="5a5c8-212">tooimport hello certifikát u vzdáleného hostitele hello</span><span class="sxs-lookup"><span data-stu-id="5a5c8-212">tooimport hello certificate on hello remote host</span></span>
1. <span data-ttu-id="5a5c8-213">Klikněte pravým tlačítkem na soubor .cer hello a vyberte **instalace certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-213">Right-click hello .cer file and select **Install certificate**.</span></span> <span data-ttu-id="5a5c8-214">Tato akce spustí hello Průvodce importem certifikátu.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-214">This will start hello Certificate Import Wizard.</span></span>
   
    ![Průvodce importem certifikátu 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. <span data-ttu-id="5a5c8-216">Pro **umístění úložiště**, vyberte **místního počítače**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-216">For **Store location**, select **Local Machine**, and then click **Next**.</span></span>
3. <span data-ttu-id="5a5c8-217">Vyberte **všechny certifikáty umístit v následujícím úložiště hello**a potom klikněte na **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-217">Select **Place all certificates in hello following store**, and then click **Browse**.</span></span> <span data-ttu-id="5a5c8-218">Přejděte toohello kořenového úložiště vzdáleného hostitele a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-218">Navigate toohello root store of your remote host, and then click **Next**.</span></span>
   
    ![Průvodce importem certifikátu 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. <span data-ttu-id="5a5c8-220">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-220">Click **Finish**.</span></span> <span data-ttu-id="5a5c8-221">Zobrazí zpráva informující o úspěšném importu hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-221">A message that tells you that hello import was successful appears.</span></span>
   
    ![Průvodce importem certifikátu 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="tooadd-device-serial-numbers-toohello-remote-host"></a><span data-ttu-id="5a5c8-223">tooadd zařízení sériová čísla toohello vzdáleného hostitele</span><span class="sxs-lookup"><span data-stu-id="5a5c8-223">tooadd device serial numbers toohello remote host</span></span>
1. <span data-ttu-id="5a5c8-224">Spusťte jako správce program Poznámkový blok a pak otevřete soubor hosts hello umístěný ve \Windows\System32\Drivers\etc.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-224">Start Notepad as an administrator, and then open hello hosts file located at \Windows\System32\Drivers\etc.</span></span>
2. <span data-ttu-id="5a5c8-225">Přidejte následující tři položky tooyour hostitele soubor hello: **DATA 0 IP adresu**, **řadič 0 pevné IP adresy**, a **řadič 1 pevné IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-225">Add hello following three entries tooyour hosts file: **DATA 0 IP address**, **Controller 0 Fixed IP address**, and **Controller 1 Fixed IP address**.</span></span>
3. <span data-ttu-id="5a5c8-226">Zadejte sériové číslo zařízení hello, který jste předtím uložili.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-226">Enter hello device serial number that you saved earlier.</span></span> <span data-ttu-id="5a5c8-227">Tato IP adresa toohello mapování, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-227">Map this toohello IP address as shown in hello following image.</span></span> <span data-ttu-id="5a5c8-228">Pro řadič 0 a řadič 1 připojit **Controller0** a **Controller1** na konci hello hello sériové číslo (CN název).</span><span class="sxs-lookup"><span data-stu-id="5a5c8-228">For Controller 0 and Controller 1, append **Controller0** and **Controller1** at hello end of hello serial number (CN name).</span></span>
   
    ![Přidání souboru toohosts název CN](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. <span data-ttu-id="5a5c8-230">Uložení souboru hostitelů hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-230">Save hello hosts file.</span></span>

### <a name="connect-toohello-device-from-hello-remote-host"></a><span data-ttu-id="5a5c8-231">Připojte zařízení toohello hello vzdáleného hostitele</span><span class="sxs-lookup"><span data-stu-id="5a5c8-231">Connect toohello device from hello remote host</span></span>
<span data-ttu-id="5a5c8-232">Pomocí prostředí Windows PowerShell a SSL tooenter na SSAdmin relaci na vašem zařízení ze vzdáleného hostitele nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-232">Use Windows PowerShell and SSL tooenter an SSAdmin session on your device from a remote host or client.</span></span> <span data-ttu-id="5a5c8-233">relace SSAdmin Hello mapuje toooption 1 v hello [konzoly sériového portu](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) nabídky vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-233">hello SSAdmin session maps toooption 1 in hello [serial console](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menu of your device.</span></span>

<span data-ttu-id="5a5c8-234">Proveďte následující postup u hello počítačů, ze kterého mají být připojení ke vzdálené prostředí Windows PowerShell toomake hello hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-234">Perform hello following procedure on hello computer from which you want toomake hello remote Windows PowerShell connection.</span></span>

#### <a name="tooenter-an-ssadmin-session-on-hello-device-by-using-windows-powershell-and-ssl"></a><span data-ttu-id="5a5c8-235">tooenter na relaci SSAdmin na zařízení hello pomocí prostředí Windows PowerShell a SSL</span><span class="sxs-lookup"><span data-stu-id="5a5c8-235">tooenter an SSAdmin session on hello device by using Windows PowerShell and SSL</span></span>
1. <span data-ttu-id="5a5c8-236">Spusťte relaci prostředí Windows PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-236">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="5a5c8-237">Přidání důvěryhodných hostitelů hello zařízení IP adres toohello klienta zadáním:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-237">Add hello device IP address toohello client’s trusted hosts by typing:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    <span data-ttu-id="5a5c8-238">Kde <*device_ip*> je hello IP adresa zařízení, třeba:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-238">Where <*device_ip*> is hello IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="5a5c8-239">Vytvoření nového pověření zadáním:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-239">Create a new credential by typing:</span></span> 
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    <span data-ttu-id="5a5c8-240">Kde <*IP cílové zařízení*> je IP adresa hello DATA 0 pro vaše zařízení; například **10.126.173.90** jak je znázorněno v předchozích bitovou kopii souboru hostitelů hello hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-240">Where <*IP of target device*> is hello IP address of DATA 0 for your device; for example, **10.126.173.90** as shown in hello preceding image of hello hosts file.</span></span> <span data-ttu-id="5a5c8-241">Zadejte také hello heslo správce pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-241">Also, supply hello administrator password for your device.</span></span>
4. <span data-ttu-id="5a5c8-242">Vytvořte relaci zadáním:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-242">Create a session by typing:</span></span>
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    <span data-ttu-id="5a5c8-243">Pro parametr - ComputerName hello v hello rutiny, zadejte hello <*sériové číslo cílového zařízení*>.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-243">For hello -ComputerName parameter in hello cmdlet, provide hello <*serial number of target device*>.</span></span> <span data-ttu-id="5a5c8-244">Toto sériové číslo bylo mapováno toohello IP adresu DATA 0 v souboru hostitelů hello na vzdáleném hostiteli; například **SHX0991003G44MT** jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-244">This serial number was mapped toohello IP address of DATA 0 in hello hosts file on your remote host; for example, **SHX0991003G44MT** as shown in hello following image.</span></span>
5. <span data-ttu-id="5a5c8-245">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="5a5c8-245">Type:</span></span> 
   
     `Enter-PSSession $session`
6. <span data-ttu-id="5a5c8-246">Budete potřebovat toowait několik minut a pak bude zařízení připojených tooyour přes HTTPS přes protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-246">You will need toowait a few minutes, and then you will be connected tooyour device via HTTPS over SSL.</span></span> <span data-ttu-id="5a5c8-247">Zobrazí se zpráva, která označuje, že jsou připojené tooyour zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a5c8-247">You will see a message that indicates you are connected tooyour device.</span></span>
   
    ![Vzdálená komunikace prostředí PowerShell pomocí protokolu HTTPS a SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a><span data-ttu-id="5a5c8-249">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a5c8-249">Next steps</span></span>
* <span data-ttu-id="5a5c8-250">Další informace o [pomocí prostředí Windows PowerShell tooadminister zařízení StorSimple](storsimple-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="5a5c8-250">Learn more about [using Windows PowerShell tooadminister your StorSimple device](storsimple-windows-powershell-administration.md).</span></span>
* <span data-ttu-id="5a5c8-251">Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="5a5c8-251">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

