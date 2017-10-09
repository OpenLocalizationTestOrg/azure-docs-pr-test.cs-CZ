---
title: " Správa konfigurace serveru v Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak tooset a spravovat konfigurační Server."
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
ms.openlocfilehash: 2852bcd25409121be46a1ebf135ebfcdce6e5de5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-configuration-server"></a><span data-ttu-id="e58df-103">Správa konfigurace serveru</span><span class="sxs-lookup"><span data-stu-id="e58df-103">Manage a Configuration Server</span></span>

<span data-ttu-id="e58df-104">Konfigurační Server funguje jako koordinátor mezi hello služby Site Recovery a na místní infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="e58df-104">Configuration Server acts as a coordinator between hello Site Recovery services and your on-premises infrastructure.</span></span> <span data-ttu-id="e58df-105">Tento článek popisuje, jak můžete nastavit, konfigurovat a spravovat hello konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="e58df-105">This article describes how you can set up, configure, and manage hello Configuration Server.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e58df-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e58df-106">Prerequisites</span></span>
<span data-ttu-id="e58df-107">Hello následují hello minimální hardware, software a síťové konfigurace požadovaná tooset až konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="e58df-107">hello following are hello minimum hardware, software, and network configuration required tooset up a Configuration Server.</span></span>

> [!NOTE]
> <span data-ttu-id="e58df-108">[Plánování kapacity](site-recovery-capacity-planner.md) je důležitým krokem tooensure nasazení hello konfigurační Server s konfigurací této sady požadavků na zatížení.</span><span class="sxs-lookup"><span data-stu-id="e58df-108">[Capacity planning](site-recovery-capacity-planner.md) is an important step tooensure that you deploy hello Configuration Server with a configuration that suites your load requirements.</span></span> <span data-ttu-id="e58df-109">Další informace o [Změna velikosti požadavky pro konfiguraci serveru](#sizing-requirements-for-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="e58df-109">Read more about [Sizing requirements for a Configuration Server](#sizing-requirements-for-a-configuration-server).</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-hello-configuration-server-software"></a><span data-ttu-id="e58df-110">Stahování softwaru konfigurační Server hello</span><span class="sxs-lookup"><span data-stu-id="e58df-110">Downloading hello Configuration Server software</span></span>
1. <span data-ttu-id="e58df-111">Přihlaste se toohello Azure portal a procházení tooyour trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="e58df-111">Log on toohello Azure portal and browse tooyour Recovery Services Vault.</span></span>
2. <span data-ttu-id="e58df-112">Procházet příliš**infrastruktura Site Recovery** > **konfigurační servery** (v rámci For VMware a fyzické počítače).</span><span class="sxs-lookup"><span data-stu-id="e58df-112">Browse too**Site Recovery Infrastructure** > **Configuration Servers** (under For VMware & Physical Machines).</span></span>

  ![Přidat stránku servery](./media/site-recovery-vmware-to-azure-manage-configuration-server/AddServers.png)
3. <span data-ttu-id="e58df-114">Klikněte na tlačítko hello **+ servery** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e58df-114">Click hello **+Servers** button.</span></span>
4. <span data-ttu-id="e58df-115">Na hello **přidat Server** klikněte na tlačítko hello stažení tlačítko toodownload hello registrační klíč.</span><span class="sxs-lookup"><span data-stu-id="e58df-115">On hello **Add Server** page, click hello Download button toodownload hello Registration key.</span></span> <span data-ttu-id="e58df-116">Během tooregister instalace konfigurační Server hello potřebujete tento klíč se službou Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="e58df-116">You need this key during hello Configuration Server installation tooregister it with Azure Site Recovery service.</span></span>
5. <span data-ttu-id="e58df-117">Klikněte na tlačítko hello **stažení hello Unified instalace serveru Microsoft Azure Site Recovery** odkaz toodownload hello nejnovější verzi hello konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="e58df-117">Click hello **Download hello Microsoft Azure Site Recovery Unified Setup** link toodownload hello latest version of hello Configuration Server.</span></span>

  ![Stažení stránky](./media/site-recovery-vmware-to-azure-manage-configuration-server/downloadcs.png)

  > [!TIP]
  <span data-ttu-id="e58df-119">Nejnovější verzi hello konfigurační Server si můžete stáhnout přímo z [stránky pro stažení Microsoft Download Center](http://aka.ms/unifiedsetup)</span><span class="sxs-lookup"><span data-stu-id="e58df-119">Latest version of hello Configuration Server can be downloaded directly from [Microsoft Download Center download page](http://aka.ms/unifiedsetup)</span></span>

## <a name="installing-and-registering-a-configuration-server-from-gui"></a><span data-ttu-id="e58df-120">Instalace a registrace konfigurace serveru z grafického uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="e58df-120">Installing and Registering a Configuration Server from GUI</span></span>
[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

## <a name="installing-and-registering-a-configuration-server-using-command-line"></a><span data-ttu-id="e58df-121">Instalace a registrace konfigurace serveru pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e58df-121">Installing and registering a Configuration Server using Command line</span></span>

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address toobe used for data transfer] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a><span data-ttu-id="e58df-122">Využití vzorků</span><span class="sxs-lookup"><span data-stu-id="e58df-122">Sample usage</span></span>
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="configuration-server-installer-command-line-arguments"></a><span data-ttu-id="e58df-123">Konfigurace serveru instalační program argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e58df-123">Configuration Server installer command-line arguments.</span></span>
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-mysql-credentials-file"></a><span data-ttu-id="e58df-124">Vytvořte soubor přihlašovacích údajů MySql</span><span class="sxs-lookup"><span data-stu-id="e58df-124">Create a MySql credentials file</span></span>
<span data-ttu-id="e58df-125">Parametr MySQLCredsFilePath vezme jako vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="e58df-125">MySQLCredsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="e58df-126">Vytvoření souboru hello pomocí hello následující formátování a předejte ji jako vstupní parametr MySQLCredsFilePath.</span><span class="sxs-lookup"><span data-stu-id="e58df-126">Create hello file using hello following format and pass it as input MySQLCredsFilePath parameter.</span></span>
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-a-proxy-settings-configuration-file"></a><span data-ttu-id="e58df-127">Vytvoření souboru konfiguračního nastavení proxy serveru</span><span class="sxs-lookup"><span data-stu-id="e58df-127">Create a proxy settings configuration file</span></span>
<span data-ttu-id="e58df-128">Parametr ProxySettingsFilePath vezme jako vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="e58df-128">ProxySettingsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="e58df-129">Vytvoření souboru hello pomocí hello následující formátování a předejte ji jako vstupní parametr ProxySettingsFilePath.</span><span class="sxs-lookup"><span data-stu-id="e58df-129">Create hello file using hello following format and pass it as input ProxySettingsFilePath parameter.</span></span>

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-configuration-server"></a><span data-ttu-id="e58df-130">Úprava nastavení proxy serveru pro konfigurační Server</span><span class="sxs-lookup"><span data-stu-id="e58df-130">Modifying proxy settings for Configuration Server</span></span>
1. <span data-ttu-id="e58df-131">Přihlášení tooyour konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="e58df-131">Login tooyour Configuration Server.</span></span>
2. <span data-ttu-id="e58df-132">Spusťte hello cspsconfigtool.exe pomocí zástupce hello na vaše.</span><span class="sxs-lookup"><span data-stu-id="e58df-132">Launch hello cspsconfigtool.exe using hello shortcut on your.</span></span>
3. <span data-ttu-id="e58df-133">Klikněte na tlačítko hello **registrace trezoru** kartě.</span><span class="sxs-lookup"><span data-stu-id="e58df-133">Click hello **Vault Registration** tab.</span></span>
4. <span data-ttu-id="e58df-134">Stáhněte si nový soubor registrace trezoru z portálu hello a zadat jako vstupní toohello nástroj.</span><span class="sxs-lookup"><span data-stu-id="e58df-134">Download a new Vault Registration file from hello portal and provide it as input toohello tool.</span></span>

  ![Register – konfigurace serveru](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
5. <span data-ttu-id="e58df-136">Zadejte hello nové podrobnosti o Proxy serveru a klikněte na hello **zaregistrovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e58df-136">Provide hello new Proxy Server details and click hello **Register** button.</span></span>
6. <span data-ttu-id="e58df-137">Otevřete okno příkazového prostředí PowerShell pro správu.</span><span class="sxs-lookup"><span data-stu-id="e58df-137">Open an Admin PowerShell command window.</span></span>
7. <span data-ttu-id="e58df-138">Spusťte následující příkaz hello</span><span class="sxs-lookup"><span data-stu-id="e58df-138">Run hello following command</span></span>
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  <span data-ttu-id="e58df-139">Pokud máte Škálováním na více systémů proces servery připojené toothis konfigurační Server, musíte příliš[opravte hello nastavení proxy serveru na všech serverech Škálováním na více systémů proces hello](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) ve vašem nasazení.</span><span class="sxs-lookup"><span data-stu-id="e58df-139">If you have Scale-out Process servers attached toothis Configuration Server, you need too[fix hello proxy settings on all hello scale-out process servers](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) in your deployment.</span></span>

## <a name="re-register-a-configuration-server-with-hello-same-recovery-services-vault"></a><span data-ttu-id="e58df-140">Znovu zaregistrujte konfigurační Server s hello stejné trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="e58df-140">Re-register a Configuration Server with hello same Recovery Services Vault</span></span>
  1. <span data-ttu-id="e58df-141">Přihlášení tooyour konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="e58df-141">Login tooyour Configuration Server.</span></span>
  2. <span data-ttu-id="e58df-142">Spusťte hello cspsconfigtool.exe pomocí hello zástupce na ploše.</span><span class="sxs-lookup"><span data-stu-id="e58df-142">Launch hello cspsconfigtool.exe using hello shortcut on your desktop.</span></span>
  3. <span data-ttu-id="e58df-143">Klikněte na tlačítko hello **registrace trezoru** kartě.</span><span class="sxs-lookup"><span data-stu-id="e58df-143">Click hello **Vault Registration** tab.</span></span>
  4. <span data-ttu-id="e58df-144">Stáhněte si nový soubor registračního z portálu hello a poskytnout ho jako vstupní toohello nástroj.</span><span class="sxs-lookup"><span data-stu-id="e58df-144">Download a new Registration file from hello portal and provide it as input toohello tool.</span></span>
        <span data-ttu-id="e58df-145">![Register – konfigurace serveru](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)</span><span class="sxs-lookup"><span data-stu-id="e58df-145">![register-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)</span></span>
  5. <span data-ttu-id="e58df-146">Zadejte podrobnosti Proxy Server hello a klikněte na tlačítko hello **zaregistrovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e58df-146">Provide hello Proxy Server details and click hello **Register** button.</span></span>  
  6. <span data-ttu-id="e58df-147">Otevřete okno příkazového prostředí PowerShell pro správu.</span><span class="sxs-lookup"><span data-stu-id="e58df-147">Open an Admin PowerShell command window.</span></span>
  7. <span data-ttu-id="e58df-148">Spusťte následující příkaz hello</span><span class="sxs-lookup"><span data-stu-id="e58df-148">Run hello following command</span></span>

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

  >[!WARNING]
  <span data-ttu-id="e58df-149">Pokud máte Škálováním na více systémů proces servery připojené toothis konfigurační Server, musíte příliš[znovu registraci všech serverů škálovaného na více systémů proces hello](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) ve vašem nasazení.</span><span class="sxs-lookup"><span data-stu-id="e58df-149">If you have Scale-out Process servers attached toothis Configuration Server, you need too[re-register all hello scale-out process servers](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) in your deployment.</span></span>

## <a name="registering-a-configuration-server-with-a-different-recovery-services-vault"></a><span data-ttu-id="e58df-150">Registrace konfigurace serveru s jinou trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="e58df-150">Registering a Configuration Server with a different Recovery Services Vault.</span></span>
1. <span data-ttu-id="e58df-151">Přihlášení tooyour konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="e58df-151">Login tooyour Configuration Server.</span></span>
2. <span data-ttu-id="e58df-152">z příkazového řádku správce spusťte příkaz hello</span><span class="sxs-lookup"><span data-stu-id="e58df-152">from an admin command prompt, run hello command</span></span>

```
reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
net stop dra
```
3. <span data-ttu-id="e58df-153">Spusťte hello cspsconfigtool.exe pomocí zástupce hello na vaše.</span><span class="sxs-lookup"><span data-stu-id="e58df-153">Launch hello cspsconfigtool.exe using hello shortcut on your.</span></span>
4. <span data-ttu-id="e58df-154">Klikněte na tlačítko hello **registrace trezoru** kartě.</span><span class="sxs-lookup"><span data-stu-id="e58df-154">Click hello **Vault Registration** tab.</span></span>
5. <span data-ttu-id="e58df-155">Stáhněte si nový soubor registračního z portálu hello a poskytnout ho jako vstupní toohello nástroj.</span><span class="sxs-lookup"><span data-stu-id="e58df-155">Download a new Registration file from hello portal and provide it as input toohello tool.</span></span>

    ![Register – konfigurace serveru](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
6. <span data-ttu-id="e58df-157">Zadejte podrobnosti Proxy Server hello a klikněte na tlačítko hello **zaregistrovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e58df-157">Provide hello Proxy Server details and click hello **Register** button.</span></span>  
7. <span data-ttu-id="e58df-158">Otevřete okno příkazového prostředí PowerShell pro správu.</span><span class="sxs-lookup"><span data-stu-id="e58df-158">Open an Admin PowerShell command window.</span></span>
8. <span data-ttu-id="e58df-159">Spusťte následující příkaz hello</span><span class="sxs-lookup"><span data-stu-id="e58df-159">Run hello following command</span></span>
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="decommissioning-a-configuration-server"></a><span data-ttu-id="e58df-160">Vyřazení z provozu konfigurace serveru</span><span class="sxs-lookup"><span data-stu-id="e58df-160">Decommissioning a Configuration Server</span></span>
<span data-ttu-id="e58df-161">Zkontrolujte následující hello před zahájením vyřazení z provozu konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="e58df-161">Ensure hello following before you start decommissioning your Configuration Server.</span></span>
1. <span data-ttu-id="e58df-162">Zakažte ochranu pro všechny virtuální počítače v rámci této konfigurace serveru.</span><span class="sxs-lookup"><span data-stu-id="e58df-162">Disable protection for all virtual machines under this Configuration Server.</span></span>
2. <span data-ttu-id="e58df-163">Zrušit přidružení všechny zásady replikace z hello konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="e58df-163">Disassociate all Replication policies from hello Configuration Server.</span></span>
3. <span data-ttu-id="e58df-164">Odstraňte všechny hostitele Vcenter vSphere servery, které jsou přidružené toohello konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="e58df-164">Delete all vCenters servers/vSphere hosts that are associated toohello Configuration Server.</span></span>

### <a name="delete-hello-configuration-server-from-azure-portal"></a><span data-ttu-id="e58df-165">Odstraňte konfigurační Server hello z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e58df-165">Delete hello Configuration Server from Azure portal</span></span>
1. <span data-ttu-id="e58df-166">Na portálu Azure, procházet příliš**infrastruktura Site Recovery** > **konfigurační servery** nabídce trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="e58df-166">In Azure portal, browse too**Site Recovery Infrastructure** > **Configuration Servers** from hello Vault menu.</span></span>
2. <span data-ttu-id="e58df-167">Klikněte na tlačítko hello konfigurační Server, které chcete toodecommission.</span><span class="sxs-lookup"><span data-stu-id="e58df-167">Click hello Configuration Server that you want toodecommission.</span></span>
3. <span data-ttu-id="e58df-168">Na stránce s podrobnostmi o hello konfigurační Server klikněte na tlačítko Odstranit hello.</span><span class="sxs-lookup"><span data-stu-id="e58df-168">On hello Configuration Server's details page, click hello Delete button.</span></span>

  ![odstranění. konfigurační server](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.PNG)
4. <span data-ttu-id="e58df-170">Klikněte na tlačítko **Ano** tooconfirm hello odstranění serveru hello.</span><span class="sxs-lookup"><span data-stu-id="e58df-170">Click **Yes** tooconfirm hello deletion of hello server.</span></span>

  >[!WARNING]
  <span data-ttu-id="e58df-171">Pokud máte jakékoli virtuálních počítačů, zásad replikace nebo hostitelů vSphere servery vCenter přidružené k této konfiguraci serveru, nelze odstranit hello server.</span><span class="sxs-lookup"><span data-stu-id="e58df-171">If you have any virtual machines, Replication policies or vCenter servers/vSphere hosts associated with this Configuration Server, you cannot delete hello server.</span></span> <span data-ttu-id="e58df-172">Odstraňte tyto entity, než se pokusíte toodelete hello trezoru.</span><span class="sxs-lookup"><span data-stu-id="e58df-172">Delete these entities before you try toodelete hello vault.</span></span>

### <a name="uninstall-hello-configuration-server-software-and-its-dependencies"></a><span data-ttu-id="e58df-173">Odinstalujte software konfigurační Server hello a jeho závislosti</span><span class="sxs-lookup"><span data-stu-id="e58df-173">Uninstall hello Configuration Server software and its dependencies</span></span>
  > [!TIP]
  <span data-ttu-id="e58df-174">Pokud máte v úmyslu znovu tooreuse hello konfigurační Server s Azure Site Recovery, pak můžete přeskočit toostep 4 přímo</span><span class="sxs-lookup"><span data-stu-id="e58df-174">If you plan tooreuse hello Configuration Server with Azure Site Recovery again, then you can skip toostep 4 directly</span></span>

1. <span data-ttu-id="e58df-175">Toohello konfiguračním serveru se přihlaste jako správce.</span><span class="sxs-lookup"><span data-stu-id="e58df-175">Log on toohello Configuration Server as an Administrator.</span></span>
2. <span data-ttu-id="e58df-176">Otevřete ovládací panely > Program > odinstalovat programy</span><span class="sxs-lookup"><span data-stu-id="e58df-176">Open up Control Panel > Program > Uninstall Programs</span></span>
3. <span data-ttu-id="e58df-177">Odinstalujte programy hello v hello následující pořadí:</span><span class="sxs-lookup"><span data-stu-id="e58df-177">Uninstall hello programs in hello following sequence:</span></span>
  * <span data-ttu-id="e58df-178">Agent Microsoft Azure Recovery Services</span><span class="sxs-lookup"><span data-stu-id="e58df-178">Microsoft Azure Recovery Services Agent</span></span>
  * <span data-ttu-id="e58df-179">Microsoft Azure Site Recovery Mobility služby nebo hlavní cílový server</span><span class="sxs-lookup"><span data-stu-id="e58df-179">Microsoft Azure Site Recovery Mobility Service/Master Target server</span></span>
  * <span data-ttu-id="e58df-180">Zprostředkovatele služby Microsoft Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="e58df-180">Microsoft Azure Site Recovery Provider</span></span>
  * <span data-ttu-id="e58df-181">Microsoft Azure Site obnovení konfigurace serveru nebo procesový Server</span><span class="sxs-lookup"><span data-stu-id="e58df-181">Microsoft Azure Site Recovery Configuration Server/Process Server</span></span>
  * <span data-ttu-id="e58df-182">Microsoft Azure Site Recovery konfigurace serveru závislosti</span><span class="sxs-lookup"><span data-stu-id="e58df-182">Microsoft Azure Site Recovery Configuration Server Dependencies</span></span>
  * <span data-ttu-id="e58df-183">MySQL Server 5.5</span><span class="sxs-lookup"><span data-stu-id="e58df-183">MySQL Server 5.5</span></span>
4. <span data-ttu-id="e58df-184">Spusťte následující příkaz z hello a správu příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e58df-184">Run hello following command from and admin command prompt.</span></span>
  ```
  reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
  ```

## <a name="renew-configuration-server-secure-socket-layerssl-certificates"></a><span data-ttu-id="e58df-185">Obnovení konfigurace serveru Secure Socket Layer(SSL) certifikátů</span><span class="sxs-lookup"><span data-stu-id="e58df-185">Renew Configuration Server Secure Socket Layer(SSL) Certificates</span></span>
<span data-ttu-id="e58df-186">Hello konfigurační Server je integrované webový server, která orchestruje hello činnosti hello služby Mobility, proces servery a servery hlavního cíle připojeny toohello konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="e58df-186">hello Configuration Server has an inbuilt webserver, which orchestrates hello activities of hello Mobility Service, Process Servers, and Master Target servers connected toohello Configuration Server.</span></span> <span data-ttu-id="e58df-187">webový server Hello konfigurační Server používá tooauthenticate certifikát SSL svým klientům.</span><span class="sxs-lookup"><span data-stu-id="e58df-187">hello Configuration Server's webserver uses an SSL certificate tooauthenticate its clients.</span></span> <span data-ttu-id="e58df-188">Tento certifikát má vypršela platnost tři roky a lze ji obnovit kdykoli použití hello následující metodu:</span><span class="sxs-lookup"><span data-stu-id="e58df-188">This certificate has an expiry of three years and can be renewed at any time using hello following method:</span></span>

> [!WARNING]
<span data-ttu-id="e58df-189">Vypršení platnosti certifikátu lze provést pouze na verze 9.4.XXXX. X nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="e58df-189">Certificate expiry can be performed only on version 9.4.XXXX.X or higher.</span></span> <span data-ttu-id="e58df-190">Upgradovat všechny součásti Azure Site Recovery hello (konfigurační Server, Server procesu, hlavní cílový Server, služba Mobility) předtím, než můžete spustit pracovní postup obnovit certifikáty hello.</span><span class="sxs-lookup"><span data-stu-id="e58df-190">Upgrade all hello Azure Site Recovery components (Configuration Server, Process Server, Master Target Server, Mobility Service) before you start hello Renew Certificates workflow.</span></span>

1. <span data-ttu-id="e58df-191">Na portálu Azure text hello, procházet tooyour trezoru > infrastruktura Site Recovery > konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="e58df-191">On hello Azure portal, browse tooyour Vault > Site Recovery Infrastructure > Configuration Server.</span></span>
2. <span data-ttu-id="e58df-192">Klikněte na tlačítko hello konfigurační Server, pro kterou je třeba toorenew hello certifikát SSL pro.</span><span class="sxs-lookup"><span data-stu-id="e58df-192">Click hello Configuration Server for which you need toorenew hello SSL Certificate for.</span></span>
3. <span data-ttu-id="e58df-193">V části hello stav konfigurace serveru se zobrazí datum vypršení platnosti hello hello certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="e58df-193">Under hello Configuration Server health, you can see hello expiry date for hello SSL Certificate.</span></span>
4. <span data-ttu-id="e58df-194">Obnovit certifikát hello kliknutím hello **obnovit certifikáty** akce, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="e58df-194">Renew hello certificate by clicking hello **Renew Certificates** action as shown in hello following image:</span></span>

  ![odstranění. konfigurační server](./media/site-recovery-vmware-to-azure-manage-configuration-server/renew-cert-page.png)

### <a name="secure-socket-layer-certificate-expiry-warning"></a><span data-ttu-id="e58df-196">Secure Socket Layer varování vypršení platnosti certifikátu</span><span class="sxs-lookup"><span data-stu-id="e58df-196">Secure Socket Layer certificate expiry warning</span></span>

> [!NOTE]
<span data-ttu-id="e58df-197">Hello platnosti certifikátu protokolu SSL pro všechny instalace, které bylo provedeno před 2016 pravděpodobně byl nastaven tooone roku.</span><span class="sxs-lookup"><span data-stu-id="e58df-197">hello SSL Certificate's validity for all installations that happened before May 2016 was set tooone year.</span></span> <span data-ttu-id="e58df-198">jste spustili zobrazuje oznámení vypršení platnosti certifikátu zobrazovat na portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="e58df-198">you have started seeing certificate expiry notifications showing up in hello Azure portal.</span></span>

1. <span data-ttu-id="e58df-199">Pokud certifikát SSL hello konfigurační Server budete tooexpire v hello následujících dvou měsíců, spustí služba hello oznámení uživatelům pomocí portálu Azure hello & e-mailu (potřebujete předplatné toobe tooAzure Site Recovery oznámení).</span><span class="sxs-lookup"><span data-stu-id="e58df-199">If hello Configuration Server's SSL certificate is going tooexpire in hello next two months, hello service starts notifying users via hello Azure portal & email (you need toobe subscribed tooAzure Site Recovery notifications).</span></span> <span data-ttu-id="e58df-200">Spuštění zobrazuje zpráva, která upozorňuje na stránce prostředků hello trezoru.</span><span class="sxs-lookup"><span data-stu-id="e58df-200">You start seeing a notification banner on hello Vault's resource page.</span></span>

  ![certifikát – oznámení](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-renew-notification.png)
2. <span data-ttu-id="e58df-202">Klikněte na tlačítko hello banner tooget další podrobnosti o vypršení platnosti certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="e58df-202">Click hello banner tooget additional details on hello Certificate expiry.</span></span>

  ![Podrobnosti o certifikátu](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-expiry-details.png)

  >[!TIP]
  <span data-ttu-id="e58df-204">Pokud místo **obnovit nyní** tlačítko se zobrazí **upgradovat nyní** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e58df-204">If instead of a **Renew Now** button you see an **Upgrade Now** button.</span></span> <span data-ttu-id="e58df-205">To znamená, že jsou některé součásti ve vašem prostředí, které ještě nebyly upgradované too9.4.xxxx.x nebo vyšší verze.</span><span class="sxs-lookup"><span data-stu-id="e58df-205">This means that there are some components in your environment that have not yet been upgraded too9.4.xxxx.x or higher versions.</span></span>

## <a name="sizing-requirements-for-a-configuration-server"></a><span data-ttu-id="e58df-206">Změna velikosti požadavky konfigurace serveru</span><span class="sxs-lookup"><span data-stu-id="e58df-206">Sizing requirements for a Configuration Server</span></span>

| <span data-ttu-id="e58df-207">**VYUŽITÍ PROCESORU**</span><span class="sxs-lookup"><span data-stu-id="e58df-207">**CPU**</span></span> | <span data-ttu-id="e58df-208">**Paměť**</span><span class="sxs-lookup"><span data-stu-id="e58df-208">**Memory**</span></span> | <span data-ttu-id="e58df-209">**Velikost disku mezipaměti**</span><span class="sxs-lookup"><span data-stu-id="e58df-209">**Cache disk size**</span></span> | <span data-ttu-id="e58df-210">**Míry změny dat**</span><span class="sxs-lookup"><span data-stu-id="e58df-210">**Data change rate**</span></span> | <span data-ttu-id="e58df-211">**Chráněné počítače**</span><span class="sxs-lookup"><span data-stu-id="e58df-211">**Protected machines**</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="e58df-212">8 Vcpu (2 sockets * @ 2,5 GHz 4 jádra)</span><span class="sxs-lookup"><span data-stu-id="e58df-212">8 vCPUs (2 sockets * 4 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="e58df-213">16 GB</span><span class="sxs-lookup"><span data-stu-id="e58df-213">16 GB</span></span> |<span data-ttu-id="e58df-214">300 GB</span><span class="sxs-lookup"><span data-stu-id="e58df-214">300 GB</span></span> |<span data-ttu-id="e58df-215">500 GB nebo méně</span><span class="sxs-lookup"><span data-stu-id="e58df-215">500 GB or less</span></span> |<span data-ttu-id="e58df-216">Replikovat počítače méně než 100.</span><span class="sxs-lookup"><span data-stu-id="e58df-216">Replicate fewer than 100 machines.</span></span> |
| <span data-ttu-id="e58df-217">12 Vcpu (2 sockets * @ 2,5 GHz 6 jader)</span><span class="sxs-lookup"><span data-stu-id="e58df-217">12 vCPUs (2 sockets * 6 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="e58df-218">18 GB</span><span class="sxs-lookup"><span data-stu-id="e58df-218">18 GB</span></span> |<span data-ttu-id="e58df-219">600 GB</span><span class="sxs-lookup"><span data-stu-id="e58df-219">600 GB</span></span> |<span data-ttu-id="e58df-220">500 GB too1 TB</span><span class="sxs-lookup"><span data-stu-id="e58df-220">500 GB too1 TB</span></span> |<span data-ttu-id="e58df-221">Replikovat mezi 100 150 počítačů.</span><span class="sxs-lookup"><span data-stu-id="e58df-221">Replicate between 100-150 machines.</span></span> |
| <span data-ttu-id="e58df-222">16 Vcpu (2 sockets * @ 2,5 GHz 8 jader)</span><span class="sxs-lookup"><span data-stu-id="e58df-222">16 vCPUs (2 sockets * 8 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="e58df-223">32 GB</span><span class="sxs-lookup"><span data-stu-id="e58df-223">32 GB</span></span> |<span data-ttu-id="e58df-224">1 TB</span><span class="sxs-lookup"><span data-stu-id="e58df-224">1 TB</span></span> |<span data-ttu-id="e58df-225">1 TB too2 TB</span><span class="sxs-lookup"><span data-stu-id="e58df-225">1 TB too2 TB</span></span> |<span data-ttu-id="e58df-226">Replikovat mezi 150 až 200 počítačů.</span><span class="sxs-lookup"><span data-stu-id="e58df-226">Replicate between 150-200 machines.</span></span> |

  >[!TIP]
  <span data-ttu-id="e58df-227">Pokud vaše denním pohybem dat je větší než 2 TB nebo plánujete tooreplicate více než 200 virtuálních počítačů, se doporučuje toodeploy další proces servery tooload vyrovnávání hello provoz replikace.</span><span class="sxs-lookup"><span data-stu-id="e58df-227">If your daily data churn exceeds 2 TB, or you plan tooreplicate more than 200 virtual machines, it is recommended toodeploy additional process servers tooload balance hello replication traffic.</span></span> <span data-ttu-id="e58df-228">Další informace o jak servery toodeploy proces Škálováním na více systémů.</span><span class="sxs-lookup"><span data-stu-id="e58df-228">Learn more about How toodeploy Scale-out Process severs.</span></span>


## <a name="common-issues"></a><span data-ttu-id="e58df-229">Běžné problémy</span><span class="sxs-lookup"><span data-stu-id="e58df-229">Common issues</span></span>
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]
