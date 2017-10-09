---
title: " Správa serveru proces Škálováním na více systémů v Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak tooset a Správa serveru proces Škálováním na více systémů ve službě Azure Site Recovery."
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
ms.openlocfilehash: 3d72f9c2c7014a4ff2fa2af168aa55ad1452eae5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-scale-out-process-server"></a><span data-ttu-id="89e69-103">Správa serveru proces Škálováním na více systémů</span><span class="sxs-lookup"><span data-stu-id="89e69-103">Manage a Scale-out Process Server</span></span>

<span data-ttu-id="89e69-104">Škálováním na více systémů procesový Server funguje jako koordinátor pro přenos dat mezi hello služby Site Recovery a na místní infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="89e69-104">Scale-out Process Server acts as a coordinator for data transfer between hello Site Recovery services and your on-premises infrastructure.</span></span> <span data-ttu-id="89e69-105">Tento článek popisuje, jak můžete nastavit, konfigurovat a spravovat proces serveru Škálovaného na více systémů.</span><span class="sxs-lookup"><span data-stu-id="89e69-105">This article describes how you can set up, configure, and manage a Scale-out Process Server.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="89e69-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="89e69-106">Prerequisites</span></span>
<span data-ttu-id="89e69-107">Následující Hello jsou hello doporučené hardwaru, softwaru a tooset nezbytné konfigurace sítě server proces Škálováním na více systémů.</span><span class="sxs-lookup"><span data-stu-id="89e69-107">hello following are hello recommended hardware, software, and network configuration required tooset up a Scale-out Process Server.</span></span>

> [!NOTE]
> <span data-ttu-id="89e69-108">[Plánování kapacity](site-recovery-capacity-planner.md) je důležitým krokem tooensure nasazení hello procesového serveru Škálovaného na více systémů s konfigurací této sady požadavků na zatížení.</span><span class="sxs-lookup"><span data-stu-id="89e69-108">[Capacity planning](site-recovery-capacity-planner.md) is an important step tooensure that you deploy hello Scale-out Process Server with a configuration that suites your load requirements.</span></span> <span data-ttu-id="89e69-109">Další informace o [škálování vlastnosti pro Server se Škálováním na proces](#sizing-requirements-for-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="89e69-109">Read more about [Scaling characteristics for a Scale-out Process Server](#sizing-requirements-for-a-configuration-server).</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-hello-scale-out-process-server-software"></a><span data-ttu-id="89e69-110">Stahování softwaru Škálováním na více systémů procesový Server hello</span><span class="sxs-lookup"><span data-stu-id="89e69-110">Downloading hello Scale-out Process Server software</span></span>
1. <span data-ttu-id="89e69-111">Přihlaste se toohello Azure portal a procházení tooyour trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="89e69-111">Log on toohello Azure portal and browse tooyour Recovery Services Vault.</span></span>
2. <span data-ttu-id="89e69-112">Procházet příliš**infrastruktura Site Recovery** > **konfigurační servery** (v rámci For VMware a fyzické počítače).</span><span class="sxs-lookup"><span data-stu-id="89e69-112">Browse too**Site Recovery Infrastructure** > **Configuration Servers** (under For VMware & Physical Machines).</span></span>
3. <span data-ttu-id="89e69-113">Vyberte vaše konfigurace serveru toodrill dolů na stránce s podrobnostmi o hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="89e69-113">Select your configuration server toodrill down into hello configuration server's details page.</span></span>
4. <span data-ttu-id="89e69-114">Klikněte na tlačítko hello **+ procesový Server** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="89e69-114">Click hello **+ Process Server** button.</span></span>
5. <span data-ttu-id="89e69-115">V hello **přidejte procesový server** vyberte **místní Server proces nasazení horizontální** možnost hello **vyberte místo toodeploy procesový server** rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="89e69-115">In hello **Add Process server** page, select **Deploy a Scale-out Process Server on-premises** option from hello **Choose where you want toodeploy your process server** drop-down.</span></span>

  ![Přidat stránku servery](./media/site-recovery-vmware-to-azure-manage-scaleout-process-server/add-process-server.png)
6. <span data-ttu-id="89e69-117">Klikněte na tlačítko hello **stažení hello Unified instalace serveru Microsoft Azure Site Recovery** odkaz toodownload hello nejnovější verzi instalace procesový Server hello Škálováním na více systémů.</span><span class="sxs-lookup"><span data-stu-id="89e69-117">Click hello **Download hello Microsoft Azure Site Recovery Unified Setup** link toodownload hello latest version of hello Scale-out Process Server installation.</span></span>

  > [!WARNING]
  <span data-ttu-id="89e69-118">Hello verzi procesový Server Škálováním na více systémů by měly být shodné tooor dřívější než verze konfigurační Server hello ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="89e69-118">hello version of your Scale-out Process Server should be equal tooor lesser than hello Configuration Server version running in your environment.</span></span> <span data-ttu-id="89e69-119">Verze kompatibility tooensure jednoduchý způsob je toouse hello stejné bits instalační program, který byl nedávno použit tooinstall nebo aktualizovat konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="89e69-119">A simple way tooensure version compatibility is toouse hello same installer bits that you recently used tooinstall/update your Configuration Server.</span></span>

## <a name="installing-and-registering-a-scale-out-process-server-from-gui"></a><span data-ttu-id="89e69-120">Instalace a registrace serveru se Škálováním na proces z grafického uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="89e69-120">Installing and registering a Scale-out Process Server from GUI</span></span>
<span data-ttu-id="89e69-121">Pokud máte tooscale škálování vašeho nasazení nad 200 zdrojového počítače nebo celkový denně změn počet více než 2 TB, je třeba objem provozu pro další proces servery toohandle hello.</span><span class="sxs-lookup"><span data-stu-id="89e69-121">If you have tooscale out your deployment beyond 200 source machines, or a total daily churn rate of more than 2 TB, you need additional process servers toohandle hello traffic volume.</span></span>

<span data-ttu-id="89e69-122">Zkontrolujte hello [velikost doporučení pro servery proces](#size-recommendations-for-the-process-server)a pak postupujte podle těchto pokynů tooset hello procesu serveru.</span><span class="sxs-lookup"><span data-stu-id="89e69-122">Check hello [size recommendations for process servers](#size-recommendations-for-the-process-server), and then follow these instructions tooset up hello process server.</span></span> <span data-ttu-id="89e69-123">Po nastavení hello serveru, migrovat toouse počítače zdrojového ho.</span><span class="sxs-lookup"><span data-stu-id="89e69-123">After setting up hello server, you migrate source machines toouse it.</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="installing-and-registering-a-scale-out-process-server-using-command-line"></a><span data-ttu-id="89e69-124">Instalace a registrace serveru se Škálováním na proces pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="89e69-124">Installing and registering a Scale-out Process Server using command-line</span></span>

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address toobe used for data transfer] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>]
```

### <a name="sample-usage"></a><span data-ttu-id="89e69-125">Využití vzorků</span><span class="sxs-lookup"><span data-stu-id="89e69-125">Sample usage</span></span>
```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```

### <a name="scale-out-process-server-installer-command-line-arguments"></a><span data-ttu-id="89e69-126">Škálováním na více systémů procesový Server instalační program argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="89e69-126">Scale-out Process Server installer command-line arguments.</span></span>
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-proxy-settings-configuration-file"></a><span data-ttu-id="89e69-127">Vytvoření souboru konfiguračního nastavení proxy serveru</span><span class="sxs-lookup"><span data-stu-id="89e69-127">Create a proxy settings configuration file</span></span>
<span data-ttu-id="89e69-128">Parametr ProxySettingsFilePath vezme jako vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="89e69-128">ProxySettingsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="89e69-129">Vytvoření souboru pomocí hello následující formátování a předejte ji jako vstupní parametr ProxySettingsFilePath.</span><span class="sxs-lookup"><span data-stu-id="89e69-129">Create file using hello following format and pass it as input ProxySettingsFilePath parameter.</span></span>
```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-scale-out-process-server"></a><span data-ttu-id="89e69-130">Úprava nastavení proxy serveru pro škálování procesového serveru</span><span class="sxs-lookup"><span data-stu-id="89e69-130">Modifying proxy settings for Scale-out Process Server</span></span>
1. <span data-ttu-id="89e69-131">Přihlásit se k serveru proces Škálováním na více systémů.</span><span class="sxs-lookup"><span data-stu-id="89e69-131">Login  into your Scale-out Process Server.</span></span>
2. <span data-ttu-id="89e69-132">Otevřete okno příkazového prostředí PowerShell pro správu.</span><span class="sxs-lookup"><span data-stu-id="89e69-132">Open an Admin PowerShell command window.</span></span>
3. <span data-ttu-id="89e69-133">Spusťte následující příkaz hello</span><span class="sxs-lookup"><span data-stu-id="89e69-133">Run hello following command</span></span>
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
4. <span data-ttu-id="89e69-134">Další Procházet adresář toohello **%PROGRAMDATA%\ASR\Agent** a hello spusťte následující příkaz</span><span class="sxs-lookup"><span data-stu-id="89e69-134">Next browse toohello directory **%PROGRAMDATA%\ASR\Agent** and run hello following command</span></span>
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```

## <a name="re-registering-a-scale-out-process-server"></a><span data-ttu-id="89e69-135">Opakováním registrace serveru proces Škálováním na více systémů</span><span class="sxs-lookup"><span data-stu-id="89e69-135">Re-registering a Scale-out Process Server</span></span>
[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

* <span data-ttu-id="89e69-136">Dále otevřete příkazový řádek správce.</span><span class="sxs-lookup"><span data-stu-id="89e69-136">Next open an Admin command prompt.</span></span>
* <span data-ttu-id="89e69-137">Procházet adresář toohello **%PROGRAMDATA%\ASR\Agent** a spusťte příkaz hello</span><span class="sxs-lookup"><span data-stu-id="89e69-137">Browse toohello directory **%PROGRAMDATA%\ASR\Agent** and run hello command</span></span>

```
cdpcli.exe --registermt

net stop obengine

net start obengine
```

## <a name="upgrading-a-scale-out-process-server"></a><span data-ttu-id="89e69-138">Upgrade serveru proces Škálováním na více systémů</span><span class="sxs-lookup"><span data-stu-id="89e69-138">Upgrading a Scale-out Process Server</span></span>
[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

## <a name="decommissioning-a-scale-out-process-server"></a><span data-ttu-id="89e69-139">Vyřazení z provozu Škálováním na více systémů procesového serveru</span><span class="sxs-lookup"><span data-stu-id="89e69-139">Decommissioning a Scale-out Process Server</span></span>
1. <span data-ttu-id="89e69-140">Zajistěte, aby:</span><span class="sxs-lookup"><span data-stu-id="89e69-140">Ensure that:</span></span>
  - <span data-ttu-id="89e69-141">Zobrazuje stav připojení konfigurační Server jako **připojeno** v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="89e69-141">Configuration Server's connection state shows as **Connected** in hello Azure portal</span></span>
  - <span data-ttu-id="89e69-142">Procesový Server je stále možné toocommunicate s hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="89e69-142">Process Server's is still able toocommunicate with hello Configuration server.</span></span>
2. <span data-ttu-id="89e69-143">Přihlaste se jako správce toohello procesového serveru</span><span class="sxs-lookup"><span data-stu-id="89e69-143">Log in toohello process server as an administrator</span></span>
3. <span data-ttu-id="89e69-144">Otevřete ovládací panely > Program > odinstalovat programy</span><span class="sxs-lookup"><span data-stu-id="89e69-144">Open up Control Panel > Program > Uninstall Programs</span></span>
4. <span data-ttu-id="89e69-145">Odinstalujte hello programy v pořadí hello zadané následující:</span><span class="sxs-lookup"><span data-stu-id="89e69-145">Uninstall hello programs in hello sequence given following:</span></span>
  * <span data-ttu-id="89e69-146">Microsoft Azure Site obnovení konfigurace serveru nebo procesový Server</span><span class="sxs-lookup"><span data-stu-id="89e69-146">Microsoft Azure Site Recovery Configuration Server/Process Server</span></span>
  * <span data-ttu-id="89e69-147">Microsoft Azure Site Recovery konfigurace serveru závislosti</span><span class="sxs-lookup"><span data-stu-id="89e69-147">Microsoft Azure Site Recovery Configuration Server Dependencies</span></span>
  * <span data-ttu-id="89e69-148">Agent Microsoft Azure Recovery Services</span><span class="sxs-lookup"><span data-stu-id="89e69-148">Microsoft Azure Recovery Services Agent</span></span>

<span data-ttu-id="89e69-149">V hello portálu Azure může trvat až too15 minut tooreflect odstranění hello procesový Server.</span><span class="sxs-lookup"><span data-stu-id="89e69-149">It can take up-too15 minutes for hello Process Server deletion tooreflect in hello Azure portal.</span></span>

  > [!NOTE]
  <span data-ttu-id="89e69-150">Pokud je server hello procesu nelze toocommunicate s hello konfigurační Server (stav připojení portálu není připojen), pak je nutné toofollow hello následující kroky toopurge ji z hello konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="89e69-150">If hello Process server is unable toocommunicate with hello Configuration Server (Connection State in portal is Disconnected), then you need toofollow hello following steps toopurge it from hello Configuration Server.</span></span>

## <a name="unregistering-a-disconnected-scale-out-process-server-from-a-configuration-server"></a><span data-ttu-id="89e69-151">Zrušení registrace odpojený server proces Škálováním na více systémů z konfigurace serveru</span><span class="sxs-lookup"><span data-stu-id="89e69-151">Unregistering a disconnected Scale-out Process server from a Configuration Server</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="sizing-requirements-for-a-scale-out-process-server"></a><span data-ttu-id="89e69-152">Změna velikosti požadavky pro Server proces Škálováním na více systémů</span><span class="sxs-lookup"><span data-stu-id="89e69-152">Sizing requirements for a Scale-out Process Server</span></span>

| <span data-ttu-id="89e69-153">**Další procesového serveru**</span><span class="sxs-lookup"><span data-stu-id="89e69-153">**Additional process server**</span></span> | <span data-ttu-id="89e69-154">**Velikost disku mezipaměti**</span><span class="sxs-lookup"><span data-stu-id="89e69-154">**Cache disk size**</span></span> | <span data-ttu-id="89e69-155">**Míry změny dat**</span><span class="sxs-lookup"><span data-stu-id="89e69-155">**Data change rate**</span></span> | <span data-ttu-id="89e69-156">**Chráněné počítače**</span><span class="sxs-lookup"><span data-stu-id="89e69-156">**Protected machines**</span></span> |
| --- | --- | --- | --- |
|<span data-ttu-id="89e69-157">4 Vcpu (2 sockets * 2 jádra @ 2,5 GHz), 8 GB paměti</span><span class="sxs-lookup"><span data-stu-id="89e69-157">4 vCPUs (2 sockets * 2 cores @ 2.5 GHz), 8-GB memory</span></span> |<span data-ttu-id="89e69-158">300 GB</span><span class="sxs-lookup"><span data-stu-id="89e69-158">300 GB</span></span> |<span data-ttu-id="89e69-159">250 GB nebo méně</span><span class="sxs-lookup"><span data-stu-id="89e69-159">250 GB or less</span></span> |<span data-ttu-id="89e69-160">Replikovat počítače 85 nebo méně.</span><span class="sxs-lookup"><span data-stu-id="89e69-160">Replicate 85 or less machines.</span></span> |
|<span data-ttu-id="89e69-161">8 Vcpu (2 sockets * 4 jádra @ 2,5 GHz), 12 GB paměti</span><span class="sxs-lookup"><span data-stu-id="89e69-161">8 vCPUs (2 sockets * 4 cores @ 2.5 GHz), 12-GB memory</span></span> |<span data-ttu-id="89e69-162">600 GB</span><span class="sxs-lookup"><span data-stu-id="89e69-162">600 GB</span></span> |<span data-ttu-id="89e69-163">TB too1 250 GB</span><span class="sxs-lookup"><span data-stu-id="89e69-163">250 GB too1 TB</span></span> |<span data-ttu-id="89e69-164">Replikovat mezi 85 150 počítačů.</span><span class="sxs-lookup"><span data-stu-id="89e69-164">Replicate between 85-150 machines.</span></span> |
|<span data-ttu-id="89e69-165">12 Vcpu (2 sockets * @ 2,5 GHz 6 jader) 24 GB paměti</span><span class="sxs-lookup"><span data-stu-id="89e69-165">12 vCPUs (2 sockets * 6 cores @ 2.5 GHz) 24-GB memory</span></span> |<span data-ttu-id="89e69-166">1 TB</span><span class="sxs-lookup"><span data-stu-id="89e69-166">1 TB</span></span> |<span data-ttu-id="89e69-167">1 TB too2 TB</span><span class="sxs-lookup"><span data-stu-id="89e69-167">1 TB too2 TB</span></span> |<span data-ttu-id="89e69-168">Replikovat mezi 150 225 počítačů.</span><span class="sxs-lookup"><span data-stu-id="89e69-168">Replicate between 150-225 machines.</span></span> |
