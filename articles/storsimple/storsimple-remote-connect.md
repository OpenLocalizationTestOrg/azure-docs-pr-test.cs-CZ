---
title: "Vzdálené připojení k zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak nakonfigurovat zařízení pro vzdálenou správu a jak se připojit k Windows Powershellu pro StorSimple prostřednictvím protokolu HTTP nebo HTTPS."
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
ms.openlocfilehash: b916173e127394d3ea06eded36285bdbbf884b12
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-remotely-to-your-storsimple-8000-series-device"></a><span data-ttu-id="bc5ca-103">Vzdálené připojení k zařízení řady StorSimple 8000</span><span class="sxs-lookup"><span data-stu-id="bc5ca-103">Connect remotely to your StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="bc5ca-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="bc5ca-104">Overview</span></span>
<span data-ttu-id="bc5ca-105">Vzdálená komunikace prostředí Windows PowerShell můžete použít pro připojení k zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-105">You can use Windows PowerShell remoting to connect to your StorSimple device.</span></span> <span data-ttu-id="bc5ca-106">Když připojíte tímto způsobem, neuvidíte nabídky.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-106">When you connect this way, you will not see a menu.</span></span> <span data-ttu-id="bc5ca-107">(Zobrazí nabídky pouze v případě, že použijete konzole sériového portu v zařízení pro připojení.) Díky vzdálenou komunikaci prostředí Windows PowerShell můžete připojit k konkrétní prostředí runspace.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-107">(You see a menu only if you use the serial console on the device to connect.) With Windows PowerShell remoting, you connect to a specific runspace.</span></span> <span data-ttu-id="bc5ca-108">Můžete také zadat jazyk zobrazení.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-108">You can also specify the display language.</span></span> 

<span data-ttu-id="bc5ca-109">Další informace o používání vzdálenou komunikaci prostředí Windows PowerShell ke správě vašich zařízení, přejděte na [pomocí Windows Powershellu pro StorSimple ke správě zařízení StorSimple](storsimple-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="bc5ca-109">For more information about using Windows PowerShell remoting to manage your device, go to [Use Windows PowerShell for StorSimple to administer your StorSimple device](storsimple-windows-powershell-administration.md).</span></span>

<span data-ttu-id="bc5ca-110">Tento kurz vysvětluje, jak nakonfigurovat zařízení pro vzdálenou správu a jak se připojit k Windows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-110">This tutorial explains how to configure your device for remote management and then how to connect to Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="bc5ca-111">Připojení přes vzdálenou komunikaci prostředí Windows PowerShell můžete použít protokol HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-111">You can use HTTP or HTTPS to connect via Windows PowerShell remoting.</span></span> <span data-ttu-id="bc5ca-112">Při výběru jak se připojit k Windows Powershellu pro StorSimple, však zvažte následující:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-112">However, when you are deciding how to connect to Windows PowerShell for StorSimple, consider the following:</span></span> 

* <span data-ttu-id="bc5ca-113">Připojení přímo ke konzole sériového portu zařízení je bezpečné, ale připojující se ke konzole sériového portu přes síťové přepínače není.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-113">Connecting directly to the device serial console is secure, but connecting to the serial console over network switches is not.</span></span> <span data-ttu-id="bc5ca-114">Buďte opatrní rizika zabezpečení při připojování ke konzole sériového portu zařízení přes síťové přepínače.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-114">Be cautious of the security risk when connecting to the device serial console over network switches.</span></span> 
* <span data-ttu-id="bc5ca-115">Připojení přes relaci protokolu HTTP může nabízí lepší zabezpečení než připojení prostřednictvím sériové konzoly přes síť.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-115">Connecting through an HTTP session might offer more security than connecting through the serial console over the network.</span></span> <span data-ttu-id="bc5ca-116">I když to není nejbezpečnější metodou, je přijatelné v důvěryhodných sítích.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-116">Although this is not the most secure method, it is acceptable on trusted networks.</span></span> 
* <span data-ttu-id="bc5ca-117">Připojení prostřednictvím relace HTTPS se certifikát podepsaný svým držitelem je nejbezpečnější a doporučená možnost.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-117">Connecting through an HTTPS session with a self-signed certificate is the most secure and the recommended option.</span></span>

<span data-ttu-id="bc5ca-118">Můžete vzdáleně připojit k rozhraní Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-118">You can connect remotely to the Windows PowerShell interface.</span></span> <span data-ttu-id="bc5ca-119">Ve výchozím nastavení však není povolen vzdálený přístup k zařízení StorSimple pomocí rozhraní Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-119">However, remote access to your StorSimple device via the Windows PowerShell interface is not enabled by default.</span></span> <span data-ttu-id="bc5ca-120">Je nutné nejprve povolit vzdálené správy na zařízení, a pak v klientském počítači používané pro přístup k zařízení.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-120">You need to enable remote management on the device first, and then on the client that is used to access your device.</span></span>

<span data-ttu-id="bc5ca-121">Podle pokynů popsaných v tomto článku byly provedeny v hostitelském systému, systémem Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-121">The steps described in this article were performed on a host system running Windows Server 2012 R2.</span></span>

## <a name="connect-through-http"></a><span data-ttu-id="bc5ca-122">Připojení prostřednictvím protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="bc5ca-122">Connect through HTTP</span></span>
<span data-ttu-id="bc5ca-123">Připojení k Windows Powershellu pro StorSimple přes relaci protokolu HTTP poskytuje lepší zabezpečení než připojení prostřednictvím konzoly sériového portu zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-123">Connecting to Windows PowerShell for StorSimple through an HTTP session offers more security than connecting through the serial console of your StorSimple device.</span></span> <span data-ttu-id="bc5ca-124">I když to není nejbezpečnější metodou, je přijatelné v důvěryhodných sítích.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-124">Although this is not the most secure method, it is acceptable on trusted networks.</span></span>

<span data-ttu-id="bc5ca-125">Konfigurace vzdálené správy můžete portál Azure classic nebo konzole sériového portu.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-125">You can use either the Azure classic portal or the serial console to configure remote management.</span></span> <span data-ttu-id="bc5ca-126">Vyberte z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-126">Select from the following procedures:</span></span>

* [<span data-ttu-id="bc5ca-127">Povolení vzdálené správy přes protokol HTTP pomocí portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="bc5ca-127">Use the Azure classic portal to enable remote management over HTTP</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [<span data-ttu-id="bc5ca-128">Povolení vzdálené správy přes protokol HTTP pomocí konzoly sériového portu</span><span class="sxs-lookup"><span data-stu-id="bc5ca-128">Use the serial console to enable remote management over HTTP</span></span>](#use-the-serial-console-to-enable-remote-management-over-http)

<span data-ttu-id="bc5ca-129">Po povolení vzdálené správy pomocí následujícího postupu Příprava klienta pro vzdálené připojení.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-129">After you enable remote management, use the following procedure to prepare the client for a remote connection.</span></span>

* [<span data-ttu-id="bc5ca-130">Příprava klienta pro připojení ke vzdálené</span><span class="sxs-lookup"><span data-stu-id="bc5ca-130">Prepare the client for remote connection</span></span>](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-http"></a><span data-ttu-id="bc5ca-131">Povolení vzdálené správy přes protokol HTTP pomocí portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="bc5ca-131">Use the Azure classic portal to enable remote management over HTTP</span></span>
<span data-ttu-id="bc5ca-132">Proveďte následující kroky na portálu Azure classic povolení vzdálené správy přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-132">Perform the following steps in the Azure classic portal to enable remote management over HTTP.</span></span>

#### <a name="to-enable-remote-management-through-the-azure-classic-portal"></a><span data-ttu-id="bc5ca-133">Chcete-li povolit vzdálenou správu prostřednictvím portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="bc5ca-133">To enable remote management through the Azure classic portal</span></span>
1. <span data-ttu-id="bc5ca-134">Přístup k **zařízení** > **konfigurace** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-134">Access **Devices** > **Configure** for your device.</span></span>
2. <span data-ttu-id="bc5ca-135">Přejděte dolů do části **Vzdálená správa**.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-135">Scroll down to the **Remote Management** section.</span></span>
3. <span data-ttu-id="bc5ca-136">U položky **Povolit vzdálenou správu** zvolte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-136">Set **Enable Remote Management** to **Yes**.</span></span>
4. <span data-ttu-id="bc5ca-137">Teď můžete zvolit připojení pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-137">You can now choose to connect using HTTP.</span></span> <span data-ttu-id="bc5ca-138">(Ve výchozím nastavení se připojují přes protokol HTTPS.) Ujistěte se, že je vybraný HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-138">(The default is to connect over HTTPS.) Make sure that HTTP is selected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="bc5ca-139">Připojení pomocí protokolu HTTP je přijatelné jenom v důvěryhodných sítích.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-139">Connecting over HTTP is acceptable only on trusted networks.</span></span>
   > 
   > 
5. <span data-ttu-id="bc5ca-140">V dolní části stránky klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-140">Click **Save** at the bottom of the page.</span></span>

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a><span data-ttu-id="bc5ca-141">Povolení vzdálené správy přes protokol HTTP pomocí konzoly sériového portu</span><span class="sxs-lookup"><span data-stu-id="bc5ca-141">Use the serial console to enable remote management over HTTP</span></span>
<span data-ttu-id="bc5ca-142">Proveďte následující kroky na konzole sériového portu zařízení povolení vzdálené správy.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-142">Perform the following steps on the device serial console to enable remote management.</span></span>

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a><span data-ttu-id="bc5ca-143">Chcete-li povolit vzdálenou správu prostřednictvím konzole sériového portu zařízení</span><span class="sxs-lookup"><span data-stu-id="bc5ca-143">To enable remote management through the device serial console</span></span>
1. <span data-ttu-id="bc5ca-144">V nabídce konzoly sériového portu vyberte možnost 1.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-144">On the serial console menu, select option 1.</span></span> <span data-ttu-id="bc5ca-145">Další informace o používání konzoly sériového portu v zařízení, přejděte na [připojit k Windows Powershellu pro StorSimple prostřednictvím konzoly sériového portu zařízení](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="bc5ca-145">For more information about using the serial console on the device, go to [Connect to Windows PowerShell for StorSimple via device serial console](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="bc5ca-146">Do příkazového řádku zadejte:`Enable-HcsRemoteManagement –AllowHttp`</span><span class="sxs-lookup"><span data-stu-id="bc5ca-146">At the prompt, type: `Enable-HcsRemoteManagement –AllowHttp`</span></span>
3. <span data-ttu-id="bc5ca-147">Budete informováni o ohrožení zabezpečení zabezpečení pomocí protokolu HTTP pro připojení k zařízení.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-147">You will be notified about the security vulnerabilities of using HTTP to connect to the device.</span></span> <span data-ttu-id="bc5ca-148">Po zobrazení výzvy potvrďte zadáním **Y**.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-148">When prompted, confirm by typing **Y**.</span></span>
4. <span data-ttu-id="bc5ca-149">Ověřte, že je povolený protokol HTTP zadáním:`Get-HcsSystem`</span><span class="sxs-lookup"><span data-stu-id="bc5ca-149">Verify that HTTP is enabled by typing: `Get-HcsSystem`</span></span>
5. <span data-ttu-id="bc5ca-150">Ověřte, zda **RemoteManagementMode** pole ukazuje **HttpsAndHttpEnabled**. Následující obrázek znázorňuje tato nastavení v PuTTY.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-150">Verify that the **RemoteManagementMode** field shows **HttpsAndHttpEnabled**.The following illustration shows these settings in PuTTY.</span></span>
   
     ![Sériového portu HTTPS a HTTP povoleno](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a><span data-ttu-id="bc5ca-152">Příprava klienta pro připojení ke vzdálené</span><span class="sxs-lookup"><span data-stu-id="bc5ca-152">Prepare the client for remote connection</span></span>
<span data-ttu-id="bc5ca-153">Proveďte následující kroky na klientovi se povolení vzdálené správy.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-153">Perform the following steps on the client to enable remote management.</span></span>

#### <a name="to-prepare-the-client-for-remote-connection"></a><span data-ttu-id="bc5ca-154">Příprava klienta pro připojení ke vzdálené</span><span class="sxs-lookup"><span data-stu-id="bc5ca-154">To prepare the client for remote connection</span></span>
1. <span data-ttu-id="bc5ca-155">Spusťte relaci prostředí Windows PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-155">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="bc5ca-156">Zadejte následující příkaz Přidat IP adresu zařízení StorSimple do seznamu důvěryhodných hostitelů klienta:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-156">Type the following command to add the IP address of the StorSimple device to the client’s trusted hosts list:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     <span data-ttu-id="bc5ca-157">Nahraďte <*device_ip*> s IP adresou vašeho zařízení; například:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-157">Replace <*device_ip*> with the IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="bc5ca-158">Zadejte následující příkaz pro uložení přihlašovacích údajů zařízení v proměnné:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-158">Type the following command to save the device credentials in a variable:</span></span> 
   
    ```
    $cred = Get-Credential
    ```
    
4. <span data-ttu-id="bc5ca-159">V dialogovém okně, které se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-159">In the dialog box that appears:</span></span>
   
   1. <span data-ttu-id="bc5ca-160">Zadejte uživatelské jméno v tomto formátu: *device_ip\SSAdmin*.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-160">Type the user name in this format: *device_ip\SSAdmin*.</span></span>
   2. <span data-ttu-id="bc5ca-161">Zadejte heslo správce zařízení, která byla nastavena, když v zařízení byl nakonfigurovaný pomocí Průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-161">Type the device administrator password that was set when the device was configured with the setup wizard.</span></span> <span data-ttu-id="bc5ca-162">Výchozí heslo je *Heslo1*.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-162">The default password is *Password1*.</span></span>
5. <span data-ttu-id="bc5ca-163">Spusťte relaci prostředí Windows PowerShell na zařízení tak, že zadáte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-163">Start a Windows PowerShell session on the device by typing this command:</span></span>
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > <span data-ttu-id="bc5ca-164">Chcete-li vytvořit relaci prostředí Windows PowerShell pro použití s virtuální zařízení StorSimple, připojte `–Port` parametr a zadejte veřejný port, který jste nakonfigurovali v vzdálenou komunikaci pro virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-164">To create a Windows PowerShell session for use with the StorSimple virtual device, append the `–Port` parameter and specify the public port that you configured in Remoting for StorSimple Virtual Appliance.</span></span>
   > 
   > 
   
     <span data-ttu-id="bc5ca-165">V tomto okamžiku byste měli mít aktivní vzdálené relace prostředí Windows PowerShell na zařízení.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-165">At this point, you should have an active remote Windows PowerShell session to the device.</span></span>
   
    ![Vzdálená komunikace prostředí PowerShell pomocí protokolu HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a><span data-ttu-id="bc5ca-167">Připojení přes protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="bc5ca-167">Connect through HTTPS</span></span>
<span data-ttu-id="bc5ca-168">Připojení k Windows Powershellu pro StorSimple prostřednictvím relace HTTPS je nejbezpečnější a doporučená metoda vzdáleně připojit k zařízení s Microsoft Azure StorSimple.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-168">Connecting to Windows PowerShell for StorSimple through an HTTPS session is the most secure and recommended method of remotely connecting to your Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="bc5ca-169">Následující postupy popisují, jak nastavit sériové konzoly a klientské počítače tak, že použijete protokol HTTPS pro připojení k Windows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-169">The following procedures explain how to set up the serial console and client computers so that you can use HTTPS to connect to Windows PowerShell for StorSimple.</span></span>

<span data-ttu-id="bc5ca-170">Konfigurace vzdálené správy můžete portál Azure classic nebo konzole sériového portu.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-170">You can use either the Azure classic portal or the serial console to configure remote management.</span></span> <span data-ttu-id="bc5ca-171">Vyberte z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-171">Select from the following procedures:</span></span>

* [<span data-ttu-id="bc5ca-172">Povolení vzdálené správy přes protokol HTTPS pomocí portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="bc5ca-172">Use the Azure classic portal to enable remote management over HTTPS</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [<span data-ttu-id="bc5ca-173">Povolení vzdálené správy přes protokol HTTPS pomocí konzoly sériového portu</span><span class="sxs-lookup"><span data-stu-id="bc5ca-173">Use the serial console to enable remote management over HTTPS</span></span>](#use-the-serial-console-to-enable-remote-management-over-https)

<span data-ttu-id="bc5ca-174">Po povolení vzdálené správy pomocí následujících postupů můžete připravit hostitele pro vzdálenou správu a připojte k zařízení od vzdáleného hostitele.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-174">After you enable remote management, use the following procedures to prepare the host for a remote management and connect to the device from the remote host.</span></span>

* [<span data-ttu-id="bc5ca-175">Příprava pro vzdálenou správu hostitele</span><span class="sxs-lookup"><span data-stu-id="bc5ca-175">Prepare the host for remote management</span></span>](#prepare-the-host-for-remote-management)
* [<span data-ttu-id="bc5ca-176">Připojení k zařízení od vzdáleného hostitele</span><span class="sxs-lookup"><span data-stu-id="bc5ca-176">Connect to the device from the remote host</span></span>](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-https"></a><span data-ttu-id="bc5ca-177">Povolení vzdálené správy přes protokol HTTPS pomocí portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="bc5ca-177">Use the Azure classic portal to enable remote management over HTTPS</span></span>
<span data-ttu-id="bc5ca-178">Proveďte následující kroky na portálu Azure classic povolení vzdálené správy přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-178">Perform the following steps in the Azure classic portal to enable remote management over HTTPS.</span></span>

#### <a name="to-enable-remote-management-over-https-from-the-azure-classic-portal"></a><span data-ttu-id="bc5ca-179">Povolení vzdálené správy přes protokol HTTPS z portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="bc5ca-179">To enable remote management over HTTPS from the Azure classic portal</span></span>
1. <span data-ttu-id="bc5ca-180">Přístup k **zařízení** > **konfigurace** pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-180">Access **Devices** > **Configure** for your device.</span></span>
2. <span data-ttu-id="bc5ca-181">Přejděte dolů do části **Vzdálená správa**.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-181">Scroll down to the **Remote Management** section.</span></span>
3. <span data-ttu-id="bc5ca-182">U položky **Povolit vzdálenou správu** zvolte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-182">Set **Enable Remote Management** to **Yes**.</span></span>
4. <span data-ttu-id="bc5ca-183">Nyní můžete připojit pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-183">You can now choose to connect using HTTPS.</span></span> <span data-ttu-id="bc5ca-184">(Ve výchozím nastavení se připojují přes protokol HTTPS.) Ujistěte se, že je vybraný protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-184">(The default is to connect over HTTPS.) Make sure that HTTPS is selected.</span></span> 
5. <span data-ttu-id="bc5ca-185">Klikněte na tlačítko **stáhnout certifikát pro vzdálenou správu**.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-185">Click **Download Remote Management Certificate**.</span></span> <span data-ttu-id="bc5ca-186">Zadejte umístění pro uložení tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-186">Specify a location to save this file.</span></span> <span data-ttu-id="bc5ca-187">Musíte nainstalovat tento certifikát na klientský nebo hostitelský počítač, který budete používat pro připojení k zařízení.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-187">You will need to install this certificate on the client or host computer that you will use to connect to the device.</span></span>
6. <span data-ttu-id="bc5ca-188">V dolní části stránky klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-188">Click **Save** at the bottom of the page.</span></span>

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a><span data-ttu-id="bc5ca-189">Povolení vzdálené správy přes protokol HTTPS pomocí konzoly sériového portu</span><span class="sxs-lookup"><span data-stu-id="bc5ca-189">Use the serial console to enable remote management over HTTPS</span></span>
<span data-ttu-id="bc5ca-190">Proveďte následující kroky na konzole sériového portu zařízení povolení vzdálené správy.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-190">Perform the following steps on the device serial console to enable remote management.</span></span>

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a><span data-ttu-id="bc5ca-191">Chcete-li povolit vzdálenou správu prostřednictvím konzole sériového portu zařízení</span><span class="sxs-lookup"><span data-stu-id="bc5ca-191">To enable remote management through the device serial console</span></span>
1. <span data-ttu-id="bc5ca-192">V nabídce konzoly sériového portu vyberte možnost 1.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-192">On the serial console menu, select option 1.</span></span> <span data-ttu-id="bc5ca-193">Další informace o používání konzoly sériového portu v zařízení, přejděte na [připojit k Windows Powershellu pro StorSimple prostřednictvím konzoly sériového portu zařízení](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="bc5ca-193">For more information about using the serial console on the device, go to [Connect to Windows PowerShell for StorSimple via device serial console](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="bc5ca-194">Do příkazového řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-194">At the prompt, type:</span></span> 
   
     `Enable-HcsRemoteManagement`
   
    <span data-ttu-id="bc5ca-195">To by měl povolit protokol HTTPS na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-195">This should enable HTTPS on your device.</span></span>
3. <span data-ttu-id="bc5ca-196">Ověřte, že je povoleno HTTPS zadáním:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-196">Verify that HTTPS has been enabled by typing:</span></span> 
   
     `Get-HcsSystem`
   
    <span data-ttu-id="bc5ca-197">Ujistěte se, že **RemoteManagementMode** pole ukazuje **HttpsEnabled**. Následující obrázek znázorňuje tato nastavení v PuTTY.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-197">Make sure that the **RemoteManagementMode** field shows **HttpsEnabled**.The following illustration shows these settings in PuTTY.</span></span>
   
     ![Sériového portu HTTPS povoleno](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. <span data-ttu-id="bc5ca-199">Z výstupu `Get-HcsSystem`, zkopírujte sériové číslo zařízení a uložit pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-199">From the output of `Get-HcsSystem`, copy the serial number of the device and save it for later use.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="bc5ca-200">Sériové číslo se mapuje na název CN certifikátu.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-200">The serial number maps to the CN name in the certificate.</span></span>
   > 
   > 
5. <span data-ttu-id="bc5ca-201">Získejte certifikát pro vzdálenou správu zadáním:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-201">Obtain a remote management certificate by typing:</span></span> 
   
     `Get-HcsRemoteManagementCert`
   
    <span data-ttu-id="bc5ca-202">Zobrazí se certifikát podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-202">A certificate similar to the following will appear.</span></span>
   
    ![Získání certifikátu vzdálené správy](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. <span data-ttu-id="bc5ca-204">Zkopírujte informace v certifikátu z **---BEGIN CERTIFICATE---** k **---END CERTIFICATE---** do textového editoru, například Poznámkový blok a uložte ho jako soubor .cer.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-204">Copy the information in the certificate from **-----BEGIN CERTIFICATE-----** to **-----END CERTIFICATE-----** into a text editor such as Notepad, and save it as a .cer file.</span></span> <span data-ttu-id="bc5ca-205">(Bude tento soubor je zkopírovat do vzdáleného hostitele při přípravě hostitele.)</span><span class="sxs-lookup"><span data-stu-id="bc5ca-205">(You will copy this file to your remote host when you prepare the host.)</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="bc5ca-206">Pokud chcete vygenerovat nový certifikát, použijte `Set-HcsRemoteManagementCert` rutiny.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-206">To generate a new certificate, use the `Set-HcsRemoteManagementCert` cmdlet.</span></span>
   > 
   > 

### <a name="prepare-the-host-for-remote-management"></a><span data-ttu-id="bc5ca-207">Příprava pro vzdálenou správu hostitele</span><span class="sxs-lookup"><span data-stu-id="bc5ca-207">Prepare the host for remote management</span></span>
<span data-ttu-id="bc5ca-208">Pro přípravu na hostitelském počítači vzdáleného připojení, který používá relaci protokolu HTTPS, proveďte následující postupy:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-208">To prepare the host computer for a remote connection that uses an HTTPS session, perform the following procedures:</span></span>

* <span data-ttu-id="bc5ca-209">[Importovat soubor .cer do kořenového úložiště klienta nebo vzdálený hostitel](#to-import-the-certificate-on-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="bc5ca-209">[Import the .cer file into the root store of the client or remote host](#to-import-the-certificate-on-the-remote-host).</span></span>
* <span data-ttu-id="bc5ca-210">[Sériová čísla zařízení přidat do souboru hostitelů na vzdáleného hostitele](#to-add-device-serial-numbers-to-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="bc5ca-210">[Add the device serial numbers to the hosts file on your remote host](#to-add-device-serial-numbers-to-the-remote-host).</span></span>

<span data-ttu-id="bc5ca-211">Každá z těchto postupů je popsána níže.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-211">Each of these procedures is described below.</span></span>

#### <a name="to-import-the-certificate-on-the-remote-host"></a><span data-ttu-id="bc5ca-212">Chcete-li importovat certifikát u vzdáleného hostitele</span><span class="sxs-lookup"><span data-stu-id="bc5ca-212">To import the certificate on the remote host</span></span>
1. <span data-ttu-id="bc5ca-213">Klikněte pravým tlačítkem na soubor .cer a vyberte **instalace certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-213">Right-click the .cer file and select **Install certificate**.</span></span> <span data-ttu-id="bc5ca-214">Tím se spustí Průvodce importem certifikátu.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-214">This will start the Certificate Import Wizard.</span></span>
   
    ![Průvodce importem certifikátu 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. <span data-ttu-id="bc5ca-216">Pro **umístění úložiště**, vyberte **místního počítače**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-216">For **Store location**, select **Local Machine**, and then click **Next**.</span></span>
3. <span data-ttu-id="bc5ca-217">Vyberte **všechny certifikáty umístit v následujícím úložišti**a potom klikněte na **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-217">Select **Place all certificates in the following store**, and then click **Browse**.</span></span> <span data-ttu-id="bc5ca-218">Přejděte do kořenového úložiště vzdáleného hostitele a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-218">Navigate to the root store of your remote host, and then click **Next**.</span></span>
   
    ![Průvodce importem certifikátu 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. <span data-ttu-id="bc5ca-220">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-220">Click **Finish**.</span></span> <span data-ttu-id="bc5ca-221">Zobrazí se zpráva, která vám ukáže, že import proběhl úspěšně.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-221">A message that tells you that the import was successful appears.</span></span>
   
    ![Průvodce importem certifikátu 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a><span data-ttu-id="bc5ca-223">Chcete-li přidat sériová čísla zařízení se vzdáleným hostitelem</span><span class="sxs-lookup"><span data-stu-id="bc5ca-223">To add device serial numbers to the remote host</span></span>
1. <span data-ttu-id="bc5ca-224">Spusťte jako správce program Poznámkový blok a pak otevřete soubor hosts umístěný ve \Windows\System32\Drivers\etc.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-224">Start Notepad as an administrator, and then open the hosts file located at \Windows\System32\Drivers\etc.</span></span>
2. <span data-ttu-id="bc5ca-225">Přidejte následující tři položky do souboru hostitele: **DATA 0 IP adresu**, **řadič 0 pevné IP adresy**, a **řadič 1 pevné IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-225">Add the following three entries to your hosts file: **DATA 0 IP address**, **Controller 0 Fixed IP address**, and **Controller 1 Fixed IP address**.</span></span>
3. <span data-ttu-id="bc5ca-226">Zadejte sériové číslo zařízení, který jste předtím uložili.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-226">Enter the device serial number that you saved earlier.</span></span> <span data-ttu-id="bc5ca-227">Mapování těchto na IP adresu, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-227">Map this to the IP address as shown in the following image.</span></span> <span data-ttu-id="bc5ca-228">Pro řadič 0 a řadič 1 připojit **Controller0** a **Controller1** na konci sériové číslo (CN název).</span><span class="sxs-lookup"><span data-stu-id="bc5ca-228">For Controller 0 and Controller 1, append **Controller0** and **Controller1** at the end of the serial number (CN name).</span></span>
   
    ![Název CN přidání do souboru hostitelů](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. <span data-ttu-id="bc5ca-230">Uložte soubor hostitelů.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-230">Save the hosts file.</span></span>

### <a name="connect-to-the-device-from-the-remote-host"></a><span data-ttu-id="bc5ca-231">Připojení k zařízení od vzdáleného hostitele</span><span class="sxs-lookup"><span data-stu-id="bc5ca-231">Connect to the device from the remote host</span></span>
<span data-ttu-id="bc5ca-232">Zadejte relaci SSAdmin na zařízení ze vzdáleného hostitele nebo klienta pomocí prostředí Windows PowerShell a SSL.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-232">Use Windows PowerShell and SSL to enter an SSAdmin session on your device from a remote host or client.</span></span> <span data-ttu-id="bc5ca-233">Relace SSAdmin se mapuje na možnost 1 v [konzoly sériového portu](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) nabídky vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-233">The SSAdmin session maps to option 1 in the [serial console](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menu of your device.</span></span>

<span data-ttu-id="bc5ca-234">Následující postup proveďte v počítači, ze kterého chcete pro vzdálené připojení prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-234">Perform the following procedure on the computer from which you want to make the remote Windows PowerShell connection.</span></span>

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a><span data-ttu-id="bc5ca-235">K zadání relaci SSAdmin na zařízení pomocí prostředí Windows PowerShell a SSL</span><span class="sxs-lookup"><span data-stu-id="bc5ca-235">To enter an SSAdmin session on the device by using Windows PowerShell and SSL</span></span>
1. <span data-ttu-id="bc5ca-236">Spusťte relaci prostředí Windows PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-236">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="bc5ca-237">Přidejte IP adresu zařízení do důvěryhodných hostitelů klienta zadáním:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-237">Add the device IP address to the client’s trusted hosts by typing:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    <span data-ttu-id="bc5ca-238">Kde <*device_ip*> je IP adresa vašeho zařízení, třeba:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-238">Where <*device_ip*> is the IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="bc5ca-239">Vytvoření nového pověření zadáním:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-239">Create a new credential by typing:</span></span> 
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    <span data-ttu-id="bc5ca-240">Kde <*IP cílové zařízení*> je IP adresa DATA 0 pro vaše zařízení; například **10.126.173.90** jak je vidět na předchozím obrázku souboru hostitelů.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-240">Where <*IP of target device*> is the IP address of DATA 0 for your device; for example, **10.126.173.90** as shown in the preceding image of the hosts file.</span></span> <span data-ttu-id="bc5ca-241">Zadejte také heslo správce pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-241">Also, supply the administrator password for your device.</span></span>
4. <span data-ttu-id="bc5ca-242">Vytvořte relaci zadáním:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-242">Create a session by typing:</span></span>
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    <span data-ttu-id="bc5ca-243">Pro parametr - ComputerName do rutiny, zadejte <*sériové číslo cílového zařízení*>.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-243">For the -ComputerName parameter in the cmdlet, provide the <*serial number of target device*>.</span></span> <span data-ttu-id="bc5ca-244">Toto sériové číslo bylo mapováno na IP adresu DATA 0 v souboru hostitelů na vzdáleném hostiteli; například **SHX0991003G44MT** jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-244">This serial number was mapped to the IP address of DATA 0 in the hosts file on your remote host; for example, **SHX0991003G44MT** as shown in the following image.</span></span>
5. <span data-ttu-id="bc5ca-245">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="bc5ca-245">Type:</span></span> 
   
     `Enter-PSSession $session`
6. <span data-ttu-id="bc5ca-246">Budete muset několik minut počkat a potom budete připojeni do zařízení pomocí protokolu HTTPS přes protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-246">You will need to wait a few minutes, and then you will be connected to your device via HTTPS over SSL.</span></span> <span data-ttu-id="bc5ca-247">Zobrazí se zpráva, která určuje, že jste připojeni k zařízení.</span><span class="sxs-lookup"><span data-stu-id="bc5ca-247">You will see a message that indicates you are connected to your device.</span></span>
   
    ![Vzdálená komunikace prostředí PowerShell pomocí protokolu HTTPS a SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a><span data-ttu-id="bc5ca-249">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bc5ca-249">Next steps</span></span>
* <span data-ttu-id="bc5ca-250">Další informace o [pomocí prostředí Windows PowerShell ke správě zařízení StorSimple](storsimple-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="bc5ca-250">Learn more about [using Windows PowerShell to administer your StorSimple device](storsimple-windows-powershell-administration.md).</span></span>
* <span data-ttu-id="bc5ca-251">Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="bc5ca-251">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

