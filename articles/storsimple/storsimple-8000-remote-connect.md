---
title: "Vzdálené připojení k zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak nakonfigurovat zařízení pro vzdálenou správu a jak se připojit k Windows Powershellu pro StorSimple prostřednictvím protokolu HTTP nebo HTTPS."
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
ms.openlocfilehash: ff76884f020a0fb8a1b48bd371c419bd65e85fd3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-remotely-to-your-storsimple-8000-series-device"></a><span data-ttu-id="84290-103">Vzdálené připojení k zařízení řady StorSimple 8000</span><span class="sxs-lookup"><span data-stu-id="84290-103">Connect remotely to your StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="84290-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="84290-104">Overview</span></span>

<span data-ttu-id="84290-105">Můžete vzdáleně připojit do zařízení pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84290-105">You can remotely connect to your device via Windows PowerShell.</span></span> <span data-ttu-id="84290-106">Když připojíte tímto způsobem, neuvidíte žádné nabídky.</span><span class="sxs-lookup"><span data-stu-id="84290-106">When you connect this way, you do not see a menu.</span></span> <span data-ttu-id="84290-107">(Zobrazí nabídky pouze v případě, že použijete konzole sériového portu v zařízení pro připojení.) Díky vzdálenou komunikaci prostředí Windows PowerShell můžete připojit k konkrétní prostředí runspace.</span><span class="sxs-lookup"><span data-stu-id="84290-107">(You see a menu only if you use the serial console on the device to connect.) With Windows PowerShell remoting, you connect to a specific runspace.</span></span> <span data-ttu-id="84290-108">Můžete také zadat jazyk zobrazení.</span><span class="sxs-lookup"><span data-stu-id="84290-108">You can also specify the display language.</span></span>

<span data-ttu-id="84290-109">Další informace o používání vzdálenou komunikaci prostředí Windows PowerShell ke správě vašich zařízení, přejděte na [pomocí Windows Powershellu pro StorSimple ke správě zařízení StorSimple](storsimple-8000-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="84290-109">For more information about using Windows PowerShell remoting to manage your device, go to [Use Windows PowerShell for StorSimple to administer your StorSimple device](storsimple-8000-windows-powershell-administration.md).</span></span>

<span data-ttu-id="84290-110">Tento kurz vysvětluje, jak nakonfigurovat zařízení pro vzdálenou správu a jak se připojit k Windows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="84290-110">This tutorial explains how to configure your device for remote management and then how to connect to Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="84290-111">Vzdálené připojení pomocí prostředí Windows PowerShell můžete použít protokol HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="84290-111">You can use HTTP or HTTPS to remotely connect via Windows PowerShell.</span></span> <span data-ttu-id="84290-112">Při výběru jak se připojit k Windows Powershellu pro StorSimple, však zvažte následující informace:</span><span class="sxs-lookup"><span data-stu-id="84290-112">However, when you are deciding how to connect to Windows PowerShell for StorSimple, consider the following information:</span></span>

* <span data-ttu-id="84290-113">Připojení přímo ke konzole sériového portu zařízení je bezpečné, ale připojující se ke konzole sériového portu přes síťové přepínače není.</span><span class="sxs-lookup"><span data-stu-id="84290-113">Connecting directly to the device serial console is secure, but connecting to the serial console over network switches is not.</span></span> <span data-ttu-id="84290-114">Buďte opatrní rizika zabezpečení při připojování ke konzole sériového portu zařízení přes síťové přepínače.</span><span class="sxs-lookup"><span data-stu-id="84290-114">Be cautious of the security risk when connecting to the device serial console over network switches.</span></span>
* <span data-ttu-id="84290-115">Připojení přes relaci protokolu HTTP může nabízí lepší zabezpečení než připojení prostřednictvím sériové konzoly přes síť.</span><span class="sxs-lookup"><span data-stu-id="84290-115">Connecting through an HTTP session might offer more security than connecting through the serial console over the network.</span></span> <span data-ttu-id="84290-116">I když to není nejbezpečnější metodou, je přijatelné v důvěryhodných sítích.</span><span class="sxs-lookup"><span data-stu-id="84290-116">Although this is not the most secure method, it is acceptable on trusted networks.</span></span>
* <span data-ttu-id="84290-117">Připojení prostřednictvím relace HTTPS se certifikát podepsaný svým držitelem je nejbezpečnější a doporučená možnost.</span><span class="sxs-lookup"><span data-stu-id="84290-117">Connecting through an HTTPS session with a self-signed certificate is the most secure and the recommended option.</span></span>

<span data-ttu-id="84290-118">Můžete vzdáleně připojit k rozhraní Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84290-118">You can connect remotely to the Windows PowerShell interface.</span></span> <span data-ttu-id="84290-119">Ve výchozím nastavení však není povolen vzdálený přístup k zařízení StorSimple pomocí rozhraní Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84290-119">However, remote access to your StorSimple device via the Windows PowerShell interface is not enabled by default.</span></span> <span data-ttu-id="84290-120">Nejprve je nutné povolit vzdálenou správu na zařízení, a pak v klientském počítači používané pro přístup k zařízení.</span><span class="sxs-lookup"><span data-stu-id="84290-120">You must enable remote management on the device first, and then on the client that is used to access your device.</span></span>

<span data-ttu-id="84290-121">Podle pokynů popsaných v tomto článku byly provedeny v hostitelském systému, systémem Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="84290-121">The steps described in this article were performed on a host system running Windows Server 2012 R2.</span></span>

## <a name="connect-through-http"></a><span data-ttu-id="84290-122">Připojení prostřednictvím protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="84290-122">Connect through HTTP</span></span>

<span data-ttu-id="84290-123">Připojení k Windows Powershellu pro StorSimple přes relaci protokolu HTTP poskytuje lepší zabezpečení než připojení prostřednictvím konzoly sériového portu zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="84290-123">Connecting to Windows PowerShell for StorSimple through an HTTP session offers more security than connecting through the serial console of your StorSimple device.</span></span> <span data-ttu-id="84290-124">I když to není nejbezpečnější metodou, je přijatelné v důvěryhodných sítích.</span><span class="sxs-lookup"><span data-stu-id="84290-124">Although this is not the most secure method, it is acceptable on trusted networks.</span></span>

<span data-ttu-id="84290-125">Konfigurace vzdálené správy můžete portál Azure nebo konzole sériového portu.</span><span class="sxs-lookup"><span data-stu-id="84290-125">You can use either the Azure portal or the serial console to configure remote management.</span></span> <span data-ttu-id="84290-126">Vyberte z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="84290-126">Select from the following procedures:</span></span>

* [<span data-ttu-id="84290-127">Povolení vzdálené správy přes protokol HTTP pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="84290-127">Use the Azure portal to enable remote management over HTTP</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [<span data-ttu-id="84290-128">Povolení vzdálené správy přes protokol HTTP pomocí konzoly sériového portu</span><span class="sxs-lookup"><span data-stu-id="84290-128">Use the serial console to enable remote management over HTTP</span></span>](#use-the-serial-console-to-enable-remote-management-over-http)

<span data-ttu-id="84290-129">Po povolení vzdálené správy pomocí následujícího postupu Příprava klienta pro vzdálené připojení.</span><span class="sxs-lookup"><span data-stu-id="84290-129">After you enable remote management, use the following procedure to prepare the client for a remote connection.</span></span>

* [<span data-ttu-id="84290-130">Příprava klienta pro připojení ke vzdálené</span><span class="sxs-lookup"><span data-stu-id="84290-130">Prepare the client for remote connection</span></span>](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-portal-to-enable-remote-management-over-http"></a><span data-ttu-id="84290-131">Povolení vzdálené správy přes protokol HTTP pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="84290-131">Use the Azure portal to enable remote management over HTTP</span></span>

<span data-ttu-id="84290-132">Proveďte následující kroky na portálu Azure povolení vzdálené správy přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="84290-132">Perform the following steps in the Azure portal to enable remote management over HTTP.</span></span>

#### <a name="to-enable-remote-management-through-the-azure-portal"></a><span data-ttu-id="84290-133">Povolení vzdálené správy prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="84290-133">To enable remote management through the Azure portal</span></span>

1. <span data-ttu-id="84290-134">Přejděte do služby Správce zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="84290-134">Go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="84290-135">Vyberte **zařízení** a poté vyberte a klikněte na zařízení, kterou chcete konfigurovat pro vzdálenou správu.</span><span class="sxs-lookup"><span data-stu-id="84290-135">Select **Devices** and then select and click the device you want to configure for remote management.</span></span> <span data-ttu-id="84290-136">Přejděte na **nastavení zařízení > zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="84290-136">Go to **Device settings > Security**.</span></span>
2. <span data-ttu-id="84290-137">V **nastavení zabezpečení** okně klikněte na tlačítko **vzdálenou správu**.</span><span class="sxs-lookup"><span data-stu-id="84290-137">In the **Security settings** blade, click **Remote Management**.</span></span>
3. <span data-ttu-id="84290-138">V **vzdálenou správu** okně nastavit **povolit vzdálenou správu** k **Ano**.</span><span class="sxs-lookup"><span data-stu-id="84290-138">In the **Remote management** blade, set **Enable Remote Management** to **Yes**.</span></span>
4. <span data-ttu-id="84290-139">Teď můžete zvolit připojení pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="84290-139">You can now choose to connect using HTTP.</span></span> <span data-ttu-id="84290-140">(Ve výchozím nastavení se připojují přes protokol HTTPS.) Ujistěte se, že je vybraný HTTP.</span><span class="sxs-lookup"><span data-stu-id="84290-140">(The default is to connect over HTTPS.) Make sure that HTTP is selected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="84290-141">Připojení pomocí protokolu HTTP je přijatelné jenom v důvěryhodných sítích.</span><span class="sxs-lookup"><span data-stu-id="84290-141">Connecting over HTTP is acceptable only on trusted networks.</span></span>
   
5. <span data-ttu-id="84290-142">Klikněte na tlačítko **Uložit** a po zobrazení výzvy k potvrzení, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="84290-142">Click **Save** and when prompted for confirmation, select **Yes**.</span></span>

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a><span data-ttu-id="84290-143">Povolení vzdálené správy přes protokol HTTP pomocí konzoly sériového portu</span><span class="sxs-lookup"><span data-stu-id="84290-143">Use the serial console to enable remote management over HTTP</span></span>
<span data-ttu-id="84290-144">Proveďte následující kroky na konzole sériového portu zařízení povolení vzdálené správy.</span><span class="sxs-lookup"><span data-stu-id="84290-144">Perform the following steps on the device serial console to enable remote management.</span></span>

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a><span data-ttu-id="84290-145">Chcete-li povolit vzdálenou správu prostřednictvím konzole sériového portu zařízení</span><span class="sxs-lookup"><span data-stu-id="84290-145">To enable remote management through the device serial console</span></span>
1. <span data-ttu-id="84290-146">V nabídce konzoly sériového portu vyberte možnost 1.</span><span class="sxs-lookup"><span data-stu-id="84290-146">On the serial console menu, select option 1.</span></span> <span data-ttu-id="84290-147">Další informace o používání konzoly sériového portu v zařízení, přejděte na [připojit k Windows Powershellu pro StorSimple prostřednictvím konzoly sériového portu zařízení](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="84290-147">For more information about using the serial console on the device, go to [Connect to Windows PowerShell for StorSimple via device serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="84290-148">Do příkazového řádku zadejte:`Enable-HcsRemoteManagement –AllowHttp`</span><span class="sxs-lookup"><span data-stu-id="84290-148">At the prompt, type: `Enable-HcsRemoteManagement –AllowHttp`</span></span>
3. <span data-ttu-id="84290-149">Jsou oznámení o chyb zabezpečení pomocí protokolu HTTP pro připojení k zařízení.</span><span class="sxs-lookup"><span data-stu-id="84290-149">You are notified about the security vulnerabilities of using HTTP to connect to the device.</span></span> <span data-ttu-id="84290-150">Po zobrazení výzvy potvrďte zadáním **Y**.</span><span class="sxs-lookup"><span data-stu-id="84290-150">When prompted, confirm by typing **Y**.</span></span>
4. <span data-ttu-id="84290-151">Ověřte, že je povolený protokol HTTP zadáním:`Get-HcsSystem`</span><span class="sxs-lookup"><span data-stu-id="84290-151">Verify that HTTP is enabled by typing: `Get-HcsSystem`</span></span>
5. <span data-ttu-id="84290-152">Ověřte, zda **RemoteManagementMode** pole ukazuje **HttpsAndHttpEnabled**. Následující obrázek znázorňuje tato nastavení v PuTTY.</span><span class="sxs-lookup"><span data-stu-id="84290-152">Verify that the **RemoteManagementMode** field shows **HttpsAndHttpEnabled**.The following illustration shows these settings in PuTTY.</span></span>
   
     ![Sériového portu HTTPS a HTTP povoleno](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a><span data-ttu-id="84290-154">Příprava klienta pro připojení ke vzdálené</span><span class="sxs-lookup"><span data-stu-id="84290-154">Prepare the client for remote connection</span></span>
<span data-ttu-id="84290-155">Proveďte následující kroky na klientovi se povolení vzdálené správy.</span><span class="sxs-lookup"><span data-stu-id="84290-155">Perform the following steps on the client to enable remote management.</span></span>

#### <a name="to-prepare-the-client-for-remote-connection"></a><span data-ttu-id="84290-156">Příprava klienta pro připojení ke vzdálené</span><span class="sxs-lookup"><span data-stu-id="84290-156">To prepare the client for remote connection</span></span>
1. <span data-ttu-id="84290-157">Spusťte relaci prostředí Windows PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="84290-157">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="84290-158">Zadejte následující příkaz Přidat IP adresu zařízení StorSimple do seznamu důvěryhodných hostitelů klienta:</span><span class="sxs-lookup"><span data-stu-id="84290-158">Type the following command to add the IP address of the StorSimple device to the client’s trusted hosts list:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     <span data-ttu-id="84290-159">Nahraďte <*device_ip*> s IP adresou vašeho zařízení; například:</span><span class="sxs-lookup"><span data-stu-id="84290-159">Replace <*device_ip*> with the IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="84290-160">Zadejte následující příkaz pro uložení přihlašovacích údajů zařízení v proměnné:</span><span class="sxs-lookup"><span data-stu-id="84290-160">Type the following command to save the device credentials in a variable:</span></span> 
   
    ```
    $cred = Get-Credential
    ```
    
4. <span data-ttu-id="84290-161">V dialogovém okně, které se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="84290-161">In the dialog box that appears:</span></span>
   
   1. <span data-ttu-id="84290-162">Zadejte uživatelské jméno v tomto formátu: *device_ip\SSAdmin*.</span><span class="sxs-lookup"><span data-stu-id="84290-162">Type the user name in this format: *device_ip\SSAdmin*.</span></span>
   2. <span data-ttu-id="84290-163">Zadejte heslo správce zařízení, která byla nastavena, když v zařízení byl nakonfigurovaný pomocí Průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="84290-163">Type the device administrator password that was set when the device was configured with the setup wizard.</span></span> <span data-ttu-id="84290-164">Výchozí heslo je *Heslo1*.</span><span class="sxs-lookup"><span data-stu-id="84290-164">The default password is *Password1*.</span></span>
5. <span data-ttu-id="84290-165">Spusťte relaci prostředí Windows PowerShell na zařízení tak, že zadáte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="84290-165">Start a Windows PowerShell session on the device by typing this command:</span></span>
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > <span data-ttu-id="84290-166">Chcete-li vytvořit relaci prostředí Windows PowerShell pro použití s virtuální zařízení StorSimple, připojte `–Port` parametr a zadejte veřejný port, který jste nakonfigurovali v vzdálenou komunikaci pro virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="84290-166">To create a Windows PowerShell session for use with the StorSimple virtual device, append the `–Port` parameter and specify the public port that you configured in Remoting for StorSimple Virtual Appliance.</span></span>
   
   
<span data-ttu-id="84290-167">V tomto okamžiku byste měli mít aktivní vzdálené relace prostředí Windows PowerShell na zařízení.</span><span class="sxs-lookup"><span data-stu-id="84290-167">At this point, you should have an active remote Windows PowerShell session to the device.</span></span>
   
![Vzdálená komunikace prostředí PowerShell pomocí protokolu HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a><span data-ttu-id="84290-169">Připojení přes protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="84290-169">Connect through HTTPS</span></span>

<span data-ttu-id="84290-170">Připojení k Windows Powershellu pro StorSimple prostřednictvím relace HTTPS je nejbezpečnější a doporučená metoda vzdáleně připojit k zařízení s Microsoft Azure StorSimple.</span><span class="sxs-lookup"><span data-stu-id="84290-170">Connecting to Windows PowerShell for StorSimple through an HTTPS session is the most secure and recommended method of remotely connecting to your Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="84290-171">Následující postupy popisují, jak nastavit sériové konzoly a klientské počítače tak, že použijete protokol HTTPS pro připojení k Windows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="84290-171">The following procedures explain how to set up the serial console and client computers so that you can use HTTPS to connect to Windows PowerShell for StorSimple.</span></span>

<span data-ttu-id="84290-172">Konfigurace vzdálené správy můžete portál Azure nebo konzole sériového portu.</span><span class="sxs-lookup"><span data-stu-id="84290-172">You can use either the Azure portal or the serial console to configure remote management.</span></span> <span data-ttu-id="84290-173">Vyberte z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="84290-173">Select from the following procedures:</span></span>

* [<span data-ttu-id="84290-174">Povolení vzdálené správy přes protokol HTTPS pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="84290-174">Use the Azure portal to enable remote management over HTTPS</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [<span data-ttu-id="84290-175">Povolení vzdálené správy přes protokol HTTPS pomocí konzoly sériového portu</span><span class="sxs-lookup"><span data-stu-id="84290-175">Use the serial console to enable remote management over HTTPS</span></span>](#use-the-serial-console-to-enable-remote-management-over-https)

<span data-ttu-id="84290-176">Po povolení vzdálené správy pomocí následujících postupů můžete připravit hostitele pro vzdálenou správu a připojte k zařízení od vzdáleného hostitele.</span><span class="sxs-lookup"><span data-stu-id="84290-176">After you enable remote management, use the following procedures to prepare the host for a remote management and connect to the device from the remote host.</span></span>

* [<span data-ttu-id="84290-177">Příprava pro vzdálenou správu hostitele</span><span class="sxs-lookup"><span data-stu-id="84290-177">Prepare the host for remote management</span></span>](#prepare-the-host-for-remote-management)
* [<span data-ttu-id="84290-178">Připojení k zařízení od vzdáleného hostitele</span><span class="sxs-lookup"><span data-stu-id="84290-178">Connect to the device from the remote host</span></span>](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-portal-to-enable-remote-management-over-https"></a><span data-ttu-id="84290-179">Povolení vzdálené správy přes protokol HTTPS pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="84290-179">Use the Azure portal to enable remote management over HTTPS</span></span>

<span data-ttu-id="84290-180">Proveďte následující kroky na portálu Azure povolení vzdálené správy přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="84290-180">Perform the following steps in the Azure portal to enable remote management over HTTPS.</span></span>

#### <a name="to-enable-remote-management-over-https-from-the-azure-portal"></a><span data-ttu-id="84290-181">Povolení vzdálené správy přes protokol HTTPS z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="84290-181">To enable remote management over HTTPS from the Azure portal</span></span>

1. <span data-ttu-id="84290-182">Přejděte do služby Správce zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="84290-182">Go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="84290-183">Vyberte **zařízení** a poté vyberte a klikněte na zařízení, kterou chcete konfigurovat pro vzdálenou správu.</span><span class="sxs-lookup"><span data-stu-id="84290-183">Select **Devices** and then select and click the device you want to configure for remote management.</span></span> <span data-ttu-id="84290-184">Přejděte na **nastavení zařízení > zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="84290-184">Go to **Device settings > Security**.</span></span>
2. <span data-ttu-id="84290-185">V **nastavení zabezpečení** okně klikněte na tlačítko **vzdálenou správu**.</span><span class="sxs-lookup"><span data-stu-id="84290-185">In the **Security settings** blade, click **Remote Management**.</span></span>
3. <span data-ttu-id="84290-186">U položky **Povolit vzdálenou správu** zvolte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="84290-186">Set **Enable Remote Management** to **Yes**.</span></span>
4. <span data-ttu-id="84290-187">Nyní můžete připojit pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="84290-187">You can now choose to connect using HTTPS.</span></span> <span data-ttu-id="84290-188">(Ve výchozím nastavení se připojují přes protokol HTTPS.) Ujistěte se, že je vybraný protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="84290-188">(The default is to connect over HTTPS.) Make sure that HTTPS is selected.</span></span>
5. <span data-ttu-id="84290-189">Klikněte na tlačítko... a pak klikněte na **Download Remote Management Certificate**.</span><span class="sxs-lookup"><span data-stu-id="84290-189">Click ... and then click **Download Remote Management Certificate**.</span></span> <span data-ttu-id="84290-190">Zadejte umístění pro uložení tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="84290-190">Specify a location to save this file.</span></span> <span data-ttu-id="84290-191">Budete muset nainstalovat tento certifikát na klientský nebo hostitelský počítač, který budete používat pro připojení k zařízení.</span><span class="sxs-lookup"><span data-stu-id="84290-191">You need to install this certificate on the client or host computer that you will use to connect to the device.</span></span>
6. <span data-ttu-id="84290-192">Klikněte na tlačítko **Uložit** a pak klikněte na **Ano** po zobrazení výzvy k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="84290-192">Click **Save** and then click **Yes** when prompted for confirmation.</span></span>

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a><span data-ttu-id="84290-193">Povolení vzdálené správy přes protokol HTTPS pomocí konzoly sériového portu</span><span class="sxs-lookup"><span data-stu-id="84290-193">Use the serial console to enable remote management over HTTPS</span></span>

<span data-ttu-id="84290-194">Proveďte následující kroky na konzole sériového portu zařízení povolení vzdálené správy.</span><span class="sxs-lookup"><span data-stu-id="84290-194">Perform the following steps on the device serial console to enable remote management.</span></span>

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a><span data-ttu-id="84290-195">Chcete-li povolit vzdálenou správu prostřednictvím konzole sériového portu zařízení</span><span class="sxs-lookup"><span data-stu-id="84290-195">To enable remote management through the device serial console</span></span>
1. <span data-ttu-id="84290-196">V nabídce konzoly sériového portu vyberte možnost 1.</span><span class="sxs-lookup"><span data-stu-id="84290-196">On the serial console menu, select option 1.</span></span> <span data-ttu-id="84290-197">Další informace o používání konzoly sériového portu v zařízení, přejděte na [připojit k Windows Powershellu pro StorSimple prostřednictvím konzoly sériového portu zařízení](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="84290-197">For more information about using the serial console on the device, go to [Connect to Windows PowerShell for StorSimple via device serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="84290-198">Do příkazového řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="84290-198">At the prompt, type:</span></span>
   
     `Enable-HcsRemoteManagement`
   
    <span data-ttu-id="84290-199">To by měl povolit protokol HTTPS na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="84290-199">This should enable HTTPS on your device.</span></span>
3. <span data-ttu-id="84290-200">Ověřte, že je povoleno HTTPS zadáním:</span><span class="sxs-lookup"><span data-stu-id="84290-200">Verify that HTTPS has been enabled by typing:</span></span> 
   
     `Get-HcsSystem`
   
    <span data-ttu-id="84290-201">Ujistěte se, že **RemoteManagementMode** pole ukazuje **HttpsEnabled**. Následující obrázek znázorňuje tato nastavení v PuTTY.</span><span class="sxs-lookup"><span data-stu-id="84290-201">Make sure that the **RemoteManagementMode** field shows **HttpsEnabled**.The following illustration shows these settings in PuTTY.</span></span>
   
     ![Sériového portu HTTPS povoleno](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. <span data-ttu-id="84290-203">Z výstupu `Get-HcsSystem`, zkopírujte sériové číslo zařízení a uložit pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="84290-203">From the output of `Get-HcsSystem`, copy the serial number of the device and save it for later use.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="84290-204">Sériové číslo se mapuje na název CN certifikátu.</span><span class="sxs-lookup"><span data-stu-id="84290-204">The serial number maps to the CN name in the certificate.</span></span>
   
5. <span data-ttu-id="84290-205">Získejte certifikát pro vzdálenou správu zadáním:</span><span class="sxs-lookup"><span data-stu-id="84290-205">Obtain a remote management certificate by typing:</span></span> 
   
     `Get-HcsRemoteManagementCert`
   
    <span data-ttu-id="84290-206">Zobrazí se certifikát podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="84290-206">A certificate similar to the following will appear.</span></span>
   
    ![Získání certifikátu vzdálené správy](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. <span data-ttu-id="84290-208">Zkopírujte informace v certifikátu z **---BEGIN CERTIFICATE---** k **---END CERTIFICATE---** do textového editoru, například Poznámkový blok a uložte ho jako soubor .cer.</span><span class="sxs-lookup"><span data-stu-id="84290-208">Copy the information in the certificate from **-----BEGIN CERTIFICATE-----** to **-----END CERTIFICATE-----** into a text editor such as Notepad, and save it as a .cer file.</span></span> <span data-ttu-id="84290-209">(Bude tento soubor je zkopírovat do vzdáleného hostitele při přípravě hostitele.)</span><span class="sxs-lookup"><span data-stu-id="84290-209">(You will copy this file to your remote host when you prepare the host.)</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="84290-210">Pokud chcete vygenerovat nový certifikát, použijte `Set-HcsRemoteManagementCert` rutiny.</span><span class="sxs-lookup"><span data-stu-id="84290-210">To generate a new certificate, use the `Set-HcsRemoteManagementCert` cmdlet.</span></span>
   
### <a name="prepare-the-host-for-remote-management"></a><span data-ttu-id="84290-211">Příprava pro vzdálenou správu hostitele</span><span class="sxs-lookup"><span data-stu-id="84290-211">Prepare the host for remote management</span></span>

<span data-ttu-id="84290-212">Pro přípravu na hostitelském počítači vzdáleného připojení, který používá relaci protokolu HTTPS, proveďte následující postupy:</span><span class="sxs-lookup"><span data-stu-id="84290-212">To prepare the host computer for a remote connection that uses an HTTPS session, perform the following procedures:</span></span>

* <span data-ttu-id="84290-213">[Importovat soubor .cer do kořenového úložiště klienta nebo vzdálený hostitel](#to-import-the-certificate-on-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="84290-213">[Import the .cer file into the root store of the client or remote host](#to-import-the-certificate-on-the-remote-host).</span></span>
* <span data-ttu-id="84290-214">[Sériová čísla zařízení přidat do souboru hostitelů na vzdáleného hostitele](#to-add-device-serial-numbers-to-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="84290-214">[Add the device serial numbers to the hosts file on your remote host](#to-add-device-serial-numbers-to-the-remote-host).</span></span>

<span data-ttu-id="84290-215">Každý z předchozích postupů, je popsán níže.</span><span class="sxs-lookup"><span data-stu-id="84290-215">Each of the preceding procedures, is described below.</span></span>

#### <a name="to-import-the-certificate-on-the-remote-host"></a><span data-ttu-id="84290-216">Chcete-li importovat certifikát u vzdáleného hostitele</span><span class="sxs-lookup"><span data-stu-id="84290-216">To import the certificate on the remote host</span></span>
1. <span data-ttu-id="84290-217">Klikněte pravým tlačítkem na soubor .cer a vyberte **instalace certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="84290-217">Right-click the .cer file and select **Install certificate**.</span></span> <span data-ttu-id="84290-218">Spustí se Průvodce importem certifikátu.</span><span class="sxs-lookup"><span data-stu-id="84290-218">This starts the Certificate Import Wizard.</span></span>
   
    ![Průvodce importem certifikátu 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. <span data-ttu-id="84290-220">Pro **umístění úložiště**, vyberte **místního počítače**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="84290-220">For **Store location**, select **Local Machine**, and then click **Next**.</span></span>
3. <span data-ttu-id="84290-221">Vyberte **všechny certifikáty umístit v následujícím úložišti**a potom klikněte na **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="84290-221">Select **Place all certificates in the following store**, and then click **Browse**.</span></span> <span data-ttu-id="84290-222">Přejděte do kořenového úložiště vzdáleného hostitele a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="84290-222">Navigate to the root store of your remote host, and then click **Next**.</span></span>
   
    ![Průvodce importem certifikátu 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. <span data-ttu-id="84290-224">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="84290-224">Click **Finish**.</span></span> <span data-ttu-id="84290-225">Zobrazí se zpráva, která vám ukáže, že import proběhl úspěšně.</span><span class="sxs-lookup"><span data-stu-id="84290-225">A message that tells you that the import was successful appears.</span></span>
   
    ![Průvodce importem certifikátu 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a><span data-ttu-id="84290-227">Chcete-li přidat sériová čísla zařízení se vzdáleným hostitelem</span><span class="sxs-lookup"><span data-stu-id="84290-227">To add device serial numbers to the remote host</span></span>
1. <span data-ttu-id="84290-228">Spusťte jako správce program Poznámkový blok a pak otevřete soubor hosts umístěný ve \Windows\System32\Drivers\etc.</span><span class="sxs-lookup"><span data-stu-id="84290-228">Start Notepad as an administrator, and then open the hosts file located at \Windows\System32\Drivers\etc.</span></span>
2. <span data-ttu-id="84290-229">Přidejte následující tři položky do souboru hostitele: **DATA 0 IP adresu**, **řadič 0 pevné IP adresy**, a **řadič 1 pevné IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="84290-229">Add the following three entries to your hosts file: **DATA 0 IP address**, **Controller 0 Fixed IP address**, and **Controller 1 Fixed IP address**.</span></span>
3. <span data-ttu-id="84290-230">Zadejte sériové číslo zařízení, který jste předtím uložili.</span><span class="sxs-lookup"><span data-stu-id="84290-230">Enter the device serial number that you saved earlier.</span></span> <span data-ttu-id="84290-231">Mapování těchto na IP adresu, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="84290-231">Map this to the IP address as shown in the following image.</span></span> <span data-ttu-id="84290-232">Pro řadič 0 a řadič 1 připojit **Controller0** a **Controller1** na konci sériové číslo (CN název).</span><span class="sxs-lookup"><span data-stu-id="84290-232">For Controller 0 and Controller 1, append **Controller0** and **Controller1** at the end of the serial number (CN name).</span></span>
   
    ![Název CN přidání do souboru hostitelů](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. <span data-ttu-id="84290-234">Uložte soubor hostitelů.</span><span class="sxs-lookup"><span data-stu-id="84290-234">Save the hosts file.</span></span>

### <a name="connect-to-the-device-from-the-remote-host"></a><span data-ttu-id="84290-235">Připojení k zařízení od vzdáleného hostitele</span><span class="sxs-lookup"><span data-stu-id="84290-235">Connect to the device from the remote host</span></span>

<span data-ttu-id="84290-236">Zadejte relaci SSAdmin na zařízení ze vzdáleného hostitele nebo klienta pomocí prostředí Windows PowerShell a SSL.</span><span class="sxs-lookup"><span data-stu-id="84290-236">Use Windows PowerShell and SSL to enter an SSAdmin session on your device from a remote host or client.</span></span> <span data-ttu-id="84290-237">Relace SSAdmin se mapuje na možnost 1 v [konzoly sériového portu](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) nabídky vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="84290-237">The SSAdmin session maps to option 1 in the [serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menu of your device.</span></span>

<span data-ttu-id="84290-238">Následující postup proveďte v počítači, ze kterého chcete pro vzdálené připojení prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84290-238">Perform the following procedure on the computer from which you want to make the remote Windows PowerShell connection.</span></span>

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a><span data-ttu-id="84290-239">K zadání relaci SSAdmin na zařízení pomocí prostředí Windows PowerShell a SSL</span><span class="sxs-lookup"><span data-stu-id="84290-239">To enter an SSAdmin session on the device by using Windows PowerShell and SSL</span></span>
1. <span data-ttu-id="84290-240">Spusťte relaci prostředí Windows PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="84290-240">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="84290-241">Přidejte IP adresu zařízení do důvěryhodných hostitelů klienta zadáním:</span><span class="sxs-lookup"><span data-stu-id="84290-241">Add the device IP address to the client’s trusted hosts by typing:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    <span data-ttu-id="84290-242">Kde <*device_ip*> je IP adresa vašeho zařízení, třeba:</span><span class="sxs-lookup"><span data-stu-id="84290-242">Where <*device_ip*> is the IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="84290-243">K vytvoření nových přihlašovacích údajů, zadejte:</span><span class="sxs-lookup"><span data-stu-id="84290-243">To create a new credential, type:</span></span>
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    <span data-ttu-id="84290-244">Kde <*IP cílové zařízení*> je IP adresa DATA 0 pro vaše zařízení; například **10.126.173.90** jak je vidět na předchozím obrázku souboru hostitelů.</span><span class="sxs-lookup"><span data-stu-id="84290-244">Where <*IP of target device*> is the IP address of DATA 0 for your device; for example, **10.126.173.90** as shown in the preceding image of the hosts file.</span></span> <span data-ttu-id="84290-245">Zadejte také heslo správce pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="84290-245">Also, supply the administrator password for your device.</span></span>
4. <span data-ttu-id="84290-246">Vytvořte relaci zadáním:</span><span class="sxs-lookup"><span data-stu-id="84290-246">Create a session by typing:</span></span>
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    <span data-ttu-id="84290-247">Pro parametr - ComputerName do rutiny, zadejte <*sériové číslo cílového zařízení*>.</span><span class="sxs-lookup"><span data-stu-id="84290-247">For the -ComputerName parameter in the cmdlet, provide the <*serial number of target device*>.</span></span> <span data-ttu-id="84290-248">Toto sériové číslo bylo mapováno na IP adresu DATA 0 v souboru hostitelů na vzdáleném hostiteli; například **SHX0991003G44MT** jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="84290-248">This serial number was mapped to the IP address of DATA 0 in the hosts file on your remote host; for example, **SHX0991003G44MT** as shown in the following image.</span></span>
5. <span data-ttu-id="84290-249">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="84290-249">Type:</span></span>
   
     `Enter-PSSession $session`
6. <span data-ttu-id="84290-250">Budete muset několik minut počkat a potom budete připojeni do zařízení pomocí protokolu HTTPS přes protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="84290-250">You will need to wait a few minutes, and then you will be connected to your device via HTTPS over SSL.</span></span> <span data-ttu-id="84290-251">Zobrazí zprávu, která označuje, že jste připojeni k zařízení.</span><span class="sxs-lookup"><span data-stu-id="84290-251">You see a message that indicates you are connected to your device.</span></span>
   
    ![Vzdálená komunikace prostředí PowerShell pomocí protokolu HTTPS a SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a><span data-ttu-id="84290-253">Další kroky</span><span class="sxs-lookup"><span data-stu-id="84290-253">Next steps</span></span>

* <span data-ttu-id="84290-254">Další informace o [pomocí prostředí Windows PowerShell ke správě zařízení StorSimple](storsimple-8000-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="84290-254">Learn more about [using Windows PowerShell to administer your StorSimple device](storsimple-8000-windows-powershell-administration.md).</span></span>
* <span data-ttu-id="84290-255">Další informace o [pomocí služby StorSimple Manager zařízení ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="84290-255">Learn more about [using the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

