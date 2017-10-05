---
title: " Správa serveru proces Škálováním na více systémů v Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak nastavit a spravovat Server proces Škálováním na více systémů v Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: e5c01de19917235c34c035415df86291b9152bf0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-a-scale-out-process-server"></a><span data-ttu-id="482b2-103">Správa serveru proces Škálováním na více systémů</span><span class="sxs-lookup"><span data-stu-id="482b2-103">Manage a Scale-out Process Server</span></span>

<span data-ttu-id="482b2-104">Škálováním na více systémů procesový Server funguje jako koordinátor pro přenos dat mezi služby Site Recovery a na místní infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="482b2-104">Scale-out Process Server acts as a coordinator for data transfer between the Site Recovery services and your on-premises infrastructure.</span></span> <span data-ttu-id="482b2-105">Tento článek popisuje, jak můžete nastavit, konfigurovat a spravovat proces serveru Škálovaného na více systémů.</span><span class="sxs-lookup"><span data-stu-id="482b2-105">This article describes how you can set up, configure, and manage a Scale-out Process Server.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="482b2-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="482b2-106">Prerequisites</span></span>
<span data-ttu-id="482b2-107">Následují doporučený hardware, software a konfiguraci sítě, které jsou nezbytné k nastavení procesového serveru se Škálováním na.</span><span class="sxs-lookup"><span data-stu-id="482b2-107">The following are the recommended hardware, software, and network configuration required to set up a Scale-out Process Server.</span></span>

> [!NOTE]
> <span data-ttu-id="482b2-108">[Plánování kapacity](site-recovery-capacity-planner.md) je důležitý krok zajistit nasazení procesového serveru Škálovaného na více systémů s konfigurací této sady požadavků na zatížení.</span><span class="sxs-lookup"><span data-stu-id="482b2-108">[Capacity planning](site-recovery-capacity-planner.md) is an important step to ensure that you deploy the Scale-out Process Server with a configuration that suites your load requirements.</span></span> <span data-ttu-id="482b2-109">Další informace o [škálování vlastnosti pro Server se Škálováním na proces](#sizing-requirements-for-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="482b2-109">Read more about [Scaling characteristics for a Scale-out Process Server](#sizing-requirements-for-a-configuration-server).</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-the-scale-out-process-server-software"></a><span data-ttu-id="482b2-110">Stahování softwaru Škálováním na více systémů procesového serveru</span><span class="sxs-lookup"><span data-stu-id="482b2-110">Downloading the Scale-out Process Server software</span></span>
1. <span data-ttu-id="482b2-111">Přihlaste se k portálu Azure a přejděte do trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="482b2-111">Log on to the Azure portal and browse to your Recovery Services Vault.</span></span>
2. <span data-ttu-id="482b2-112">Přejděte do **infrastruktury obnovení lokality** > **konfigurační servery** (v části pro VMware a fyzické počítače).</span><span class="sxs-lookup"><span data-stu-id="482b2-112">Browse to **Site Recovery Infrastructure** > **Configuration Servers** (under For VMware & Physical Machines).</span></span>
3. <span data-ttu-id="482b2-113">Vyberte svůj server konfigurace můžete rozbalit stránce s podrobnostmi o konfiguračním serveru.</span><span class="sxs-lookup"><span data-stu-id="482b2-113">Select your configuration server to drill down into the configuration server's details page.</span></span>
4. <span data-ttu-id="482b2-114">Klikněte **+ procesový Server** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="482b2-114">Click the **+ Process Server** button.</span></span>
5. <span data-ttu-id="482b2-115">V **přidejte procesový server** vyberte **místní Server proces nasazení horizontální** možnost z **zvolte, kam chcete procesový server nasadit** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="482b2-115">In the **Add Process server** page, select **Deploy a Scale-out Process Server on-premises** option from the **Choose where you want to deploy your process server** drop-down.</span></span>

  ![Přidat stránku servery](./media/site-recovery-vmware-to-azure-manage-scaleout-process-server/add-process-server.png)
6. <span data-ttu-id="482b2-117">Klikněte **stáhnout instalaci služby Microsoft Azure Site Recovery Unified** odkaz ke stažení nejnovější verze serveru se Škálováním procesu instalace.</span><span class="sxs-lookup"><span data-stu-id="482b2-117">Click the **Download the Microsoft Azure Site Recovery Unified Setup** link to download the latest version of the Scale-out Process Server installation.</span></span>

  > [!WARNING]
  <span data-ttu-id="482b2-118">Verze procesový Server se Škálováním na musí být rovna nebo menší než konfigurační Server verze ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="482b2-118">The version of your Scale-out Process Server should be equal to or lesser than the Configuration Server version running in your environment.</span></span> <span data-ttu-id="482b2-119">Jednoduchý způsob, jak zajistit kompatibilitu verze se má používat stejný Instalační služby bits, které jste naposledy použili k instalovat nebo aktualizovat konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="482b2-119">A simple way to ensure version compatibility is to use the same installer bits that you recently used to install/update your Configuration Server.</span></span>

## <a name="installing-and-registering-a-scale-out-process-server-from-gui"></a><span data-ttu-id="482b2-120">Instalace a registrace serveru se Škálováním na proces z grafického uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="482b2-120">Installing and registering a Scale-out Process Server from GUI</span></span>
<span data-ttu-id="482b2-121">Pokud máte pro horizontální škálování vašeho nasazení nad 200 zdrojového počítače nebo celkový denní míry změn více než 2 TB, je třeba servery další proces pro zpracování provozu.</span><span class="sxs-lookup"><span data-stu-id="482b2-121">If you have to scale out your deployment beyond 200 source machines, or a total daily churn rate of more than 2 TB, you need additional process servers to handle the traffic volume.</span></span>

<span data-ttu-id="482b2-122">Zkontrolujte [velikost doporučení pro servery proces](#size-recommendations-for-the-process-server)a pak postupujte podle těchto pokynů můžete nastavit na procesní server.</span><span class="sxs-lookup"><span data-stu-id="482b2-122">Check the [size recommendations for process servers](#size-recommendations-for-the-process-server), and then follow these instructions to set up the process server.</span></span> <span data-ttu-id="482b2-123">Po nastavení serveru, migrovat zdrojový počítač používat.</span><span class="sxs-lookup"><span data-stu-id="482b2-123">After setting up the server, you migrate source machines to use it.</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="installing-and-registering-a-scale-out-process-server-using-command-line"></a><span data-ttu-id="482b2-124">Instalace a registrace serveru se Škálováním na proces pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="482b2-124">Installing and registering a Scale-out Process Server using command-line</span></span>

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

### <a name="sample-usage"></a><span data-ttu-id="482b2-125">Využití vzorků</span><span class="sxs-lookup"><span data-stu-id="482b2-125">Sample usage</span></span>
```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```

### <a name="scale-out-process-server-installer-command-line-arguments"></a><span data-ttu-id="482b2-126">Škálováním na více systémů procesový Server instalační program argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="482b2-126">Scale-out Process Server installer command-line arguments.</span></span>
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-proxy-settings-configuration-file"></a><span data-ttu-id="482b2-127">Vytvoření souboru konfiguračního nastavení proxy serveru</span><span class="sxs-lookup"><span data-stu-id="482b2-127">Create a proxy settings configuration file</span></span>
<span data-ttu-id="482b2-128">Parametr ProxySettingsFilePath vezme jako vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="482b2-128">ProxySettingsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="482b2-129">Vytvořte soubor v následujícím formátu a předejte ji jako vstupní parametr ProxySettingsFilePath.</span><span class="sxs-lookup"><span data-stu-id="482b2-129">Create file using the following format and pass it as input ProxySettingsFilePath parameter.</span></span>
```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-scale-out-process-server"></a><span data-ttu-id="482b2-130">Úprava nastavení proxy serveru pro škálování procesového serveru</span><span class="sxs-lookup"><span data-stu-id="482b2-130">Modifying proxy settings for Scale-out Process Server</span></span>
1. <span data-ttu-id="482b2-131">Přihlásit se k serveru proces Škálováním na více systémů.</span><span class="sxs-lookup"><span data-stu-id="482b2-131">Login  into your Scale-out Process Server.</span></span>
2. <span data-ttu-id="482b2-132">Otevřete okno příkazového prostředí PowerShell pro správu.</span><span class="sxs-lookup"><span data-stu-id="482b2-132">Open an Admin PowerShell command window.</span></span>
3. <span data-ttu-id="482b2-133">Spusťte následující příkaz</span><span class="sxs-lookup"><span data-stu-id="482b2-133">Run the following command</span></span>
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
4. <span data-ttu-id="482b2-134">Dále přejděte do adresáře **%PROGRAMDATA%\ASR\Agent** a spusťte následující příkaz</span><span class="sxs-lookup"><span data-stu-id="482b2-134">Next browse to the directory **%PROGRAMDATA%\ASR\Agent** and run the following command</span></span>
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```

## <a name="re-registering-a-scale-out-process-server"></a><span data-ttu-id="482b2-135">Opakováním registrace serveru proces Škálováním na více systémů</span><span class="sxs-lookup"><span data-stu-id="482b2-135">Re-registering a Scale-out Process Server</span></span>
[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

* <span data-ttu-id="482b2-136">Dále otevřete příkazový řádek správce.</span><span class="sxs-lookup"><span data-stu-id="482b2-136">Next open an Admin command prompt.</span></span>
* <span data-ttu-id="482b2-137">Přejděte do adresáře **%PROGRAMDATA%\ASR\Agent** a spusťte příkaz</span><span class="sxs-lookup"><span data-stu-id="482b2-137">Browse to the directory **%PROGRAMDATA%\ASR\Agent** and run the command</span></span>

```
cdpcli.exe --registermt

net stop obengine

net start obengine
```

## <a name="upgrading-a-scale-out-process-server"></a><span data-ttu-id="482b2-138">Upgrade serveru proces Škálováním na více systémů</span><span class="sxs-lookup"><span data-stu-id="482b2-138">Upgrading a Scale-out Process Server</span></span>
[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

## <a name="decommissioning-a-scale-out-process-server"></a><span data-ttu-id="482b2-139">Vyřazení z provozu Škálováním na více systémů procesového serveru</span><span class="sxs-lookup"><span data-stu-id="482b2-139">Decommissioning a Scale-out Process Server</span></span>
1. <span data-ttu-id="482b2-140">Zajistěte, aby:</span><span class="sxs-lookup"><span data-stu-id="482b2-140">Ensure that:</span></span>
  - <span data-ttu-id="482b2-141">Zobrazuje stav připojení konfigurační Server jako **připojeno** na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="482b2-141">Configuration Server's connection state shows as **Connected** in the Azure portal</span></span>
  - <span data-ttu-id="482b2-142">Procesový Server je stále moci komunikovat s konfiguračním serverem.</span><span class="sxs-lookup"><span data-stu-id="482b2-142">Process Server's is still able to communicate with the Configuration server.</span></span>
2. <span data-ttu-id="482b2-143">Přihlaste se k procesový server jako správce</span><span class="sxs-lookup"><span data-stu-id="482b2-143">Log in to the process server as an administrator</span></span>
3. <span data-ttu-id="482b2-144">Otevřete ovládací panely > Program > odinstalovat programy</span><span class="sxs-lookup"><span data-stu-id="482b2-144">Open up Control Panel > Program > Uninstall Programs</span></span>
4. <span data-ttu-id="482b2-145">Odinstalujte programy v pořadí zadané následující:</span><span class="sxs-lookup"><span data-stu-id="482b2-145">Uninstall the programs in the sequence given following:</span></span>
  * <span data-ttu-id="482b2-146">Microsoft Azure Site obnovení konfigurace serveru nebo procesový Server</span><span class="sxs-lookup"><span data-stu-id="482b2-146">Microsoft Azure Site Recovery Configuration Server/Process Server</span></span>
  * <span data-ttu-id="482b2-147">Microsoft Azure Site Recovery konfigurace serveru závislosti</span><span class="sxs-lookup"><span data-stu-id="482b2-147">Microsoft Azure Site Recovery Configuration Server Dependencies</span></span>
  * <span data-ttu-id="482b2-148">Agent Microsoft Azure Recovery Services</span><span class="sxs-lookup"><span data-stu-id="482b2-148">Microsoft Azure Recovery Services Agent</span></span>

<span data-ttu-id="482b2-149">Může trvat až 15 minut pro odstranění procesový Server tak, aby odrážela na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="482b2-149">It can take up-to 15 minutes for the Process Server deletion to reflect in the Azure portal.</span></span>

  > [!NOTE]
  <span data-ttu-id="482b2-150">Pokud je procesový server nemůže komunikovat s konfiguračním serverem (stav připojení portálu není připojen), je třeba provést následující kroky, abyste ji vymazat z konfiguračního serveru.</span><span class="sxs-lookup"><span data-stu-id="482b2-150">If the Process server is unable to communicate with the Configuration Server (Connection State in portal is Disconnected), then you need to follow the following steps to purge it from the Configuration Server.</span></span>

## <a name="unregistering-a-disconnected-scale-out-process-server-from-a-configuration-server"></a><span data-ttu-id="482b2-151">Zrušení registrace odpojený server proces Škálováním na více systémů z konfigurace serveru</span><span class="sxs-lookup"><span data-stu-id="482b2-151">Unregistering a disconnected Scale-out Process server from a Configuration Server</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="sizing-requirements-for-a-scale-out-process-server"></a><span data-ttu-id="482b2-152">Změna velikosti požadavky pro Server proces Škálováním na více systémů</span><span class="sxs-lookup"><span data-stu-id="482b2-152">Sizing requirements for a Scale-out Process Server</span></span>

| <span data-ttu-id="482b2-153">**Další procesového serveru**</span><span class="sxs-lookup"><span data-stu-id="482b2-153">**Additional process server**</span></span> | <span data-ttu-id="482b2-154">**Velikost disku mezipaměti**</span><span class="sxs-lookup"><span data-stu-id="482b2-154">**Cache disk size**</span></span> | <span data-ttu-id="482b2-155">**Míry změny dat**</span><span class="sxs-lookup"><span data-stu-id="482b2-155">**Data change rate**</span></span> | <span data-ttu-id="482b2-156">**Chráněné počítače**</span><span class="sxs-lookup"><span data-stu-id="482b2-156">**Protected machines**</span></span> |
| --- | --- | --- | --- |
|<span data-ttu-id="482b2-157">4 Vcpu (2 sockets * 2 jádra @ 2,5 GHz), 8 GB paměti</span><span class="sxs-lookup"><span data-stu-id="482b2-157">4 vCPUs (2 sockets * 2 cores @ 2.5 GHz), 8-GB memory</span></span> |<span data-ttu-id="482b2-158">300 GB</span><span class="sxs-lookup"><span data-stu-id="482b2-158">300 GB</span></span> |<span data-ttu-id="482b2-159">250 GB nebo méně</span><span class="sxs-lookup"><span data-stu-id="482b2-159">250 GB or less</span></span> |<span data-ttu-id="482b2-160">Replikovat počítače 85 nebo méně.</span><span class="sxs-lookup"><span data-stu-id="482b2-160">Replicate 85 or less machines.</span></span> |
|<span data-ttu-id="482b2-161">8 Vcpu (2 sockets * 4 jádra @ 2,5 GHz), 12 GB paměti</span><span class="sxs-lookup"><span data-stu-id="482b2-161">8 vCPUs (2 sockets * 4 cores @ 2.5 GHz), 12-GB memory</span></span> |<span data-ttu-id="482b2-162">600 GB</span><span class="sxs-lookup"><span data-stu-id="482b2-162">600 GB</span></span> |<span data-ttu-id="482b2-163">250 GB až 1 TB</span><span class="sxs-lookup"><span data-stu-id="482b2-163">250 GB to 1 TB</span></span> |<span data-ttu-id="482b2-164">Replikovat mezi 85 150 počítačů.</span><span class="sxs-lookup"><span data-stu-id="482b2-164">Replicate between 85-150 machines.</span></span> |
|<span data-ttu-id="482b2-165">12 Vcpu (2 sockets * @ 2,5 GHz 6 jader) 24 GB paměti</span><span class="sxs-lookup"><span data-stu-id="482b2-165">12 vCPUs (2 sockets * 6 cores @ 2.5 GHz) 24-GB memory</span></span> |<span data-ttu-id="482b2-166">1 TB</span><span class="sxs-lookup"><span data-stu-id="482b2-166">1 TB</span></span> |<span data-ttu-id="482b2-167">1 TB 2 TB</span><span class="sxs-lookup"><span data-stu-id="482b2-167">1 TB to 2 TB</span></span> |<span data-ttu-id="482b2-168">Replikovat mezi 150 225 počítačů.</span><span class="sxs-lookup"><span data-stu-id="482b2-168">Replicate between 150-225 machines.</span></span> |
