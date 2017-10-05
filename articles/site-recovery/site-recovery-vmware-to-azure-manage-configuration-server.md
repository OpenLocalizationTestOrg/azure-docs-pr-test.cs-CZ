---
title: " Správa konfigurace serveru v Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak nastavit a spravovat konfigurační Server."
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
ms.openlocfilehash: 36da8c7d0f3ace194522e5288f26069cf46d470e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-a-configuration-server"></a><span data-ttu-id="d38e0-103">Správa konfigurace serveru</span><span class="sxs-lookup"><span data-stu-id="d38e0-103">Manage a Configuration Server</span></span>

<span data-ttu-id="d38e0-104">Konfigurační Server funguje jako koordinátor mezi služby Site Recovery a na místní infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="d38e0-104">Configuration Server acts as a coordinator between the Site Recovery services and your on-premises infrastructure.</span></span> <span data-ttu-id="d38e0-105">Tento článek popisuje, jak můžete nastavit, konfigurovat a spravovat konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="d38e0-105">This article describes how you can set up, configure, and manage the Configuration Server.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d38e0-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d38e0-106">Prerequisites</span></span>
<span data-ttu-id="d38e0-107">Toto jsou minimální hardware, software a konfiguraci sítě, které jsou nezbytné k nastavení konfigurace serveru.</span><span class="sxs-lookup"><span data-stu-id="d38e0-107">The following are the minimum hardware, software, and network configuration required to set up a Configuration Server.</span></span>

> [!NOTE]
> <span data-ttu-id="d38e0-108">[Plánování kapacity](site-recovery-capacity-planner.md) je důležitý krok zajistit nasazení konfigurace serveru s konfigurací této sady požadavků na zatížení.</span><span class="sxs-lookup"><span data-stu-id="d38e0-108">[Capacity planning](site-recovery-capacity-planner.md) is an important step to ensure that you deploy the Configuration Server with a configuration that suites your load requirements.</span></span> <span data-ttu-id="d38e0-109">Další informace o [Změna velikosti požadavky pro konfiguraci serveru](#sizing-requirements-for-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="d38e0-109">Read more about [Sizing requirements for a Configuration Server](#sizing-requirements-for-a-configuration-server).</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-the-configuration-server-software"></a><span data-ttu-id="d38e0-110">Stahování softwaru konfigurační Server</span><span class="sxs-lookup"><span data-stu-id="d38e0-110">Downloading the Configuration Server software</span></span>
1. <span data-ttu-id="d38e0-111">Přihlaste se k portálu Azure a přejděte do trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="d38e0-111">Log on to the Azure portal and browse to your Recovery Services Vault.</span></span>
2. <span data-ttu-id="d38e0-112">Přejděte do **infrastruktury obnovení lokality** > **konfigurační servery** (v části pro VMware a fyzické počítače).</span><span class="sxs-lookup"><span data-stu-id="d38e0-112">Browse to **Site Recovery Infrastructure** > **Configuration Servers** (under For VMware & Physical Machines).</span></span>

  ![Přidat stránku servery](./media/site-recovery-vmware-to-azure-manage-configuration-server/AddServers.png)
3. <span data-ttu-id="d38e0-114">Klikněte **+ servery** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d38e0-114">Click the **+Servers** button.</span></span>
4. <span data-ttu-id="d38e0-115">Na **přidat Server** klikněte na tlačítko Stáhnout na stáhnout registrační klíč.</span><span class="sxs-lookup"><span data-stu-id="d38e0-115">On the **Add Server** page, click the Download button to download the Registration key.</span></span> <span data-ttu-id="d38e0-116">Je nutné tento klíč během instalace konfigurační Server a zaregistrujte ho pomocí služby Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="d38e0-116">You need this key during the Configuration Server installation to register it with Azure Site Recovery service.</span></span>
5. <span data-ttu-id="d38e0-117">Klikněte **stáhnout instalaci služby Microsoft Azure Site Recovery Unified** odkaz ke stažení nejnovější verze konfigurace serveru.</span><span class="sxs-lookup"><span data-stu-id="d38e0-117">Click the **Download the Microsoft Azure Site Recovery Unified Setup** link to download the latest version of the Configuration Server.</span></span>

  ![Stažení stránky](./media/site-recovery-vmware-to-azure-manage-configuration-server/downloadcs.png)

  > [!TIP]
  <span data-ttu-id="d38e0-119">Nejnovější verze konfigurace serveru si můžete stáhnout přímo z [stránky pro stažení Microsoft Download Center](http://aka.ms/unifiedsetup)</span><span class="sxs-lookup"><span data-stu-id="d38e0-119">Latest version of the Configuration Server can be downloaded directly from [Microsoft Download Center download page](http://aka.ms/unifiedsetup)</span></span>

## <a name="installing-and-registering-a-configuration-server-from-gui"></a><span data-ttu-id="d38e0-120">Instalace a registrace konfigurace serveru z grafického uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d38e0-120">Installing and Registering a Configuration Server from GUI</span></span>
[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

## <a name="installing-and-registering-a-configuration-server-using-command-line"></a><span data-ttu-id="d38e0-121">Instalace a registrace konfigurace serveru pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="d38e0-121">Installing and registering a Configuration Server using Command line</span></span>

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a><span data-ttu-id="d38e0-122">Využití vzorků</span><span class="sxs-lookup"><span data-stu-id="d38e0-122">Sample usage</span></span>
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="configuration-server-installer-command-line-arguments"></a><span data-ttu-id="d38e0-123">Konfigurace serveru instalační program argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d38e0-123">Configuration Server installer command-line arguments.</span></span>
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-mysql-credentials-file"></a><span data-ttu-id="d38e0-124">Vytvořte soubor přihlašovacích údajů MySql</span><span class="sxs-lookup"><span data-stu-id="d38e0-124">Create a MySql credentials file</span></span>
<span data-ttu-id="d38e0-125">Parametr MySQLCredsFilePath vezme jako vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="d38e0-125">MySQLCredsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="d38e0-126">Vytvořte soubor v následujícím formátu a předejte jej jako vstupní parametr MySQLCredsFilePath.</span><span class="sxs-lookup"><span data-stu-id="d38e0-126">Create the file using the following format and pass it as input MySQLCredsFilePath parameter.</span></span>
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-a-proxy-settings-configuration-file"></a><span data-ttu-id="d38e0-127">Vytvoření souboru konfiguračního nastavení proxy serveru</span><span class="sxs-lookup"><span data-stu-id="d38e0-127">Create a proxy settings configuration file</span></span>
<span data-ttu-id="d38e0-128">Parametr ProxySettingsFilePath vezme jako vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="d38e0-128">ProxySettingsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="d38e0-129">Vytvořte soubor v následujícím formátu a předejte jej jako vstupní parametr ProxySettingsFilePath.</span><span class="sxs-lookup"><span data-stu-id="d38e0-129">Create the file using the following format and pass it as input ProxySettingsFilePath parameter.</span></span>

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-configuration-server"></a><span data-ttu-id="d38e0-130">Úprava nastavení proxy serveru pro konfigurační Server</span><span class="sxs-lookup"><span data-stu-id="d38e0-130">Modifying proxy settings for Configuration Server</span></span>
1. <span data-ttu-id="d38e0-131">Přihlášení k konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="d38e0-131">Login to your Configuration Server.</span></span>
2. <span data-ttu-id="d38e0-132">Spusťte cspsconfigtool.exe pomocí zástupce na vaše.</span><span class="sxs-lookup"><span data-stu-id="d38e0-132">Launch the cspsconfigtool.exe using the shortcut on your.</span></span>
3. <span data-ttu-id="d38e0-133">Klikněte **registrace trezoru** kartě.</span><span class="sxs-lookup"><span data-stu-id="d38e0-133">Click the **Vault Registration** tab.</span></span>
4. <span data-ttu-id="d38e0-134">Stáhněte si nový soubor registrace trezoru z portálu a zadejte jako vstup pro nástroj.</span><span class="sxs-lookup"><span data-stu-id="d38e0-134">Download a new Vault Registration file from the portal and provide it as input to the tool.</span></span>

  ![Register – konfigurace serveru](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
5. <span data-ttu-id="d38e0-136">Zadejte nové podrobnosti o Proxy serveru a klikněte na **zaregistrovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d38e0-136">Provide the new Proxy Server details and click the **Register** button.</span></span>
6. <span data-ttu-id="d38e0-137">Otevřete okno příkazového prostředí PowerShell pro správu.</span><span class="sxs-lookup"><span data-stu-id="d38e0-137">Open an Admin PowerShell command window.</span></span>
7. <span data-ttu-id="d38e0-138">Spusťte následující příkaz</span><span class="sxs-lookup"><span data-stu-id="d38e0-138">Run the following command</span></span>
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  <span data-ttu-id="d38e0-139">Pokud máte Škálováním na více systémů proces servery připojené k této konfigurační Server, budete muset [opravte nastavení proxy serveru na všech serverech Škálováním na více systémů proces](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) ve vašem nasazení.</span><span class="sxs-lookup"><span data-stu-id="d38e0-139">If you have Scale-out Process servers attached to this Configuration Server, you need to [fix the proxy settings on all the scale-out process servers](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) in your deployment.</span></span>

## <a name="re-register-a-configuration-server-with-the-same-recovery-services-vault"></a><span data-ttu-id="d38e0-140">Znovu zaregistrujte konfigurační Server se stejným trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="d38e0-140">Re-register a Configuration Server with the same Recovery Services Vault</span></span>
  1. <span data-ttu-id="d38e0-141">Přihlášení k konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="d38e0-141">Login to your Configuration Server.</span></span>
  2. <span data-ttu-id="d38e0-142">Spusťte cspsconfigtool.exe pomocí zástupce na ploše.</span><span class="sxs-lookup"><span data-stu-id="d38e0-142">Launch the cspsconfigtool.exe using the shortcut on your desktop.</span></span>
  3. <span data-ttu-id="d38e0-143">Klikněte **registrace trezoru** kartě.</span><span class="sxs-lookup"><span data-stu-id="d38e0-143">Click the **Vault Registration** tab.</span></span>
  4. <span data-ttu-id="d38e0-144">Stáhněte si nový soubor registrace z portálu a zadejte jako vstup pro nástroj.</span><span class="sxs-lookup"><span data-stu-id="d38e0-144">Download a new Registration file from the portal and provide it as input to the tool.</span></span>
        <span data-ttu-id="d38e0-145">![Register – konfigurace serveru](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)</span><span class="sxs-lookup"><span data-stu-id="d38e0-145">![register-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)</span></span>
  5. <span data-ttu-id="d38e0-146">Zadejte podrobnosti o Proxy serveru a klikněte na **zaregistrovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d38e0-146">Provide the Proxy Server details and click the **Register** button.</span></span>  
  6. <span data-ttu-id="d38e0-147">Otevřete okno příkazového prostředí PowerShell pro správu.</span><span class="sxs-lookup"><span data-stu-id="d38e0-147">Open an Admin PowerShell command window.</span></span>
  7. <span data-ttu-id="d38e0-148">Spusťte následující příkaz</span><span class="sxs-lookup"><span data-stu-id="d38e0-148">Run the following command</span></span>

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

  >[!WARNING]
  <span data-ttu-id="d38e0-149">Pokud máte Škálováním na více systémů proces servery připojené k této konfigurační Server, budete muset [znovu registraci všech serverů škálovaného na více systémů proces](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) ve vašem nasazení.</span><span class="sxs-lookup"><span data-stu-id="d38e0-149">If you have Scale-out Process servers attached to this Configuration Server, you need to [re-register all the scale-out process servers](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) in your deployment.</span></span>

## <a name="registering-a-configuration-server-with-a-different-recovery-services-vault"></a><span data-ttu-id="d38e0-150">Registrace konfigurace serveru s jinou trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="d38e0-150">Registering a Configuration Server with a different Recovery Services Vault.</span></span>
1. <span data-ttu-id="d38e0-151">Přihlášení k konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="d38e0-151">Login to your Configuration Server.</span></span>
2. <span data-ttu-id="d38e0-152">z příkazového řádku správce spusťte příkaz</span><span class="sxs-lookup"><span data-stu-id="d38e0-152">from an admin command prompt, run the command</span></span>

```
reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
net stop dra
```
3. <span data-ttu-id="d38e0-153">Spusťte cspsconfigtool.exe pomocí zástupce na vaše.</span><span class="sxs-lookup"><span data-stu-id="d38e0-153">Launch the cspsconfigtool.exe using the shortcut on your.</span></span>
4. <span data-ttu-id="d38e0-154">Klikněte **registrace trezoru** kartě.</span><span class="sxs-lookup"><span data-stu-id="d38e0-154">Click the **Vault Registration** tab.</span></span>
5. <span data-ttu-id="d38e0-155">Stáhněte si nový soubor registrace z portálu a zadejte jako vstup pro nástroj.</span><span class="sxs-lookup"><span data-stu-id="d38e0-155">Download a new Registration file from the portal and provide it as input to the tool.</span></span>

    ![Register – konfigurace serveru](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
6. <span data-ttu-id="d38e0-157">Zadejte podrobnosti o Proxy serveru a klikněte na **zaregistrovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d38e0-157">Provide the Proxy Server details and click the **Register** button.</span></span>  
7. <span data-ttu-id="d38e0-158">Otevřete okno příkazového prostředí PowerShell pro správu.</span><span class="sxs-lookup"><span data-stu-id="d38e0-158">Open an Admin PowerShell command window.</span></span>
8. <span data-ttu-id="d38e0-159">Spusťte následující příkaz</span><span class="sxs-lookup"><span data-stu-id="d38e0-159">Run the following command</span></span>
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="decommissioning-a-configuration-server"></a><span data-ttu-id="d38e0-160">Vyřazení z provozu konfigurace serveru</span><span class="sxs-lookup"><span data-stu-id="d38e0-160">Decommissioning a Configuration Server</span></span>
<span data-ttu-id="d38e0-161">Zkontrolujte následující před zahájením vyřazení z provozu konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="d38e0-161">Ensure the following before you start decommissioning your Configuration Server.</span></span>
1. <span data-ttu-id="d38e0-162">Zakažte ochranu pro všechny virtuální počítače v rámci této konfigurace serveru.</span><span class="sxs-lookup"><span data-stu-id="d38e0-162">Disable protection for all virtual machines under this Configuration Server.</span></span>
2. <span data-ttu-id="d38e0-163">Zrušit přidružení všechny zásady replikace z konfiguračního serveru.</span><span class="sxs-lookup"><span data-stu-id="d38e0-163">Disassociate all Replication policies from the Configuration Server.</span></span>
3. <span data-ttu-id="d38e0-164">Odstraňte všechny hostitele Vcenter vSphere servery, které jsou přidružené k konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="d38e0-164">Delete all vCenters servers/vSphere hosts that are associated to the Configuration Server.</span></span>

### <a name="delete-the-configuration-server-from-azure-portal"></a><span data-ttu-id="d38e0-165">Odstraňte konfigurační Server z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d38e0-165">Delete the Configuration Server from Azure portal</span></span>
1. <span data-ttu-id="d38e0-166">Na portálu Azure, přejděte do **infrastruktura Site Recovery** > **konfigurační servery** nabídce trezoru.</span><span class="sxs-lookup"><span data-stu-id="d38e0-166">In Azure portal, browse to **Site Recovery Infrastructure** > **Configuration Servers** from the Vault menu.</span></span>
2. <span data-ttu-id="d38e0-167">Klikněte na konfigurační Server, který chcete vyřadit z provozu.</span><span class="sxs-lookup"><span data-stu-id="d38e0-167">Click the Configuration Server that you want to decommission.</span></span>
3. <span data-ttu-id="d38e0-168">Na stránce s podrobnostmi o konfigurační Server klikněte na tlačítko Odstranit.</span><span class="sxs-lookup"><span data-stu-id="d38e0-168">On the Configuration Server's details page, click the Delete button.</span></span>

  ![odstranění. konfigurační server](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.PNG)
4. <span data-ttu-id="d38e0-170">Klikněte na tlačítko **Ano** potvrďte odstranění serveru.</span><span class="sxs-lookup"><span data-stu-id="d38e0-170">Click **Yes** to confirm the deletion of the server.</span></span>

  >[!WARNING]
  <span data-ttu-id="d38e0-171">Pokud máte jakékoli virtuálních počítačů, zásad replikace nebo hostitelů vSphere servery vCenter přidružené k této konfiguraci serveru, nelze odstranit server.</span><span class="sxs-lookup"><span data-stu-id="d38e0-171">If you have any virtual machines, Replication policies or vCenter servers/vSphere hosts associated with this Configuration Server, you cannot delete the server.</span></span> <span data-ttu-id="d38e0-172">Před pokusem o odstranění trezoru, odstraňte tyto entity.</span><span class="sxs-lookup"><span data-stu-id="d38e0-172">Delete these entities before you try to delete the vault.</span></span>

### <a name="uninstall-the-configuration-server-software-and-its-dependencies"></a><span data-ttu-id="d38e0-173">Odinstalujte software konfigurační Server a jeho závislosti</span><span class="sxs-lookup"><span data-stu-id="d38e0-173">Uninstall the Configuration Server software and its dependencies</span></span>
  > [!TIP]
  <span data-ttu-id="d38e0-174">Pokud budete chtít použít konfigurační Server s Azure Site Recovery znovu, potom můžete přeskočit ke kroku 4 přímo</span><span class="sxs-lookup"><span data-stu-id="d38e0-174">If you plan to reuse the Configuration Server with Azure Site Recovery again, then you can skip to step 4 directly</span></span>

1. <span data-ttu-id="d38e0-175">Přihlaste se k serveru Configuration jako správce.</span><span class="sxs-lookup"><span data-stu-id="d38e0-175">Log on to the Configuration Server as an Administrator.</span></span>
2. <span data-ttu-id="d38e0-176">Otevřete ovládací panely > Program > odinstalovat programy</span><span class="sxs-lookup"><span data-stu-id="d38e0-176">Open up Control Panel > Program > Uninstall Programs</span></span>
3. <span data-ttu-id="d38e0-177">Odinstalujte programy v tomto pořadí:</span><span class="sxs-lookup"><span data-stu-id="d38e0-177">Uninstall the programs in the following sequence:</span></span>
  * <span data-ttu-id="d38e0-178">Agent Microsoft Azure Recovery Services</span><span class="sxs-lookup"><span data-stu-id="d38e0-178">Microsoft Azure Recovery Services Agent</span></span>
  * <span data-ttu-id="d38e0-179">Microsoft Azure Site Recovery Mobility služby nebo hlavní cílový server</span><span class="sxs-lookup"><span data-stu-id="d38e0-179">Microsoft Azure Site Recovery Mobility Service/Master Target server</span></span>
  * <span data-ttu-id="d38e0-180">Zprostředkovatele služby Microsoft Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="d38e0-180">Microsoft Azure Site Recovery Provider</span></span>
  * <span data-ttu-id="d38e0-181">Microsoft Azure Site obnovení konfigurace serveru nebo procesový Server</span><span class="sxs-lookup"><span data-stu-id="d38e0-181">Microsoft Azure Site Recovery Configuration Server/Process Server</span></span>
  * <span data-ttu-id="d38e0-182">Microsoft Azure Site Recovery konfigurace serveru závislosti</span><span class="sxs-lookup"><span data-stu-id="d38e0-182">Microsoft Azure Site Recovery Configuration Server Dependencies</span></span>
  * <span data-ttu-id="d38e0-183">MySQL Server 5.5</span><span class="sxs-lookup"><span data-stu-id="d38e0-183">MySQL Server 5.5</span></span>
4. <span data-ttu-id="d38e0-184">Spusťte následující příkaz a správu příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d38e0-184">Run the following command from and admin command prompt.</span></span>
  ```
  reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
  ```

## <a name="renew-configuration-server-secure-socket-layerssl-certificates"></a><span data-ttu-id="d38e0-185">Obnovení konfigurace serveru Secure Socket Layer(SSL) certifikátů</span><span class="sxs-lookup"><span data-stu-id="d38e0-185">Renew Configuration Server Secure Socket Layer(SSL) Certificates</span></span>
<span data-ttu-id="d38e0-186">Konfigurační Server má integrované webový server, která orchestruje aktivity služby Mobility, proces servery a hlavního cíle serverů připojených k serveru Configuration.</span><span class="sxs-lookup"><span data-stu-id="d38e0-186">The Configuration Server has an inbuilt webserver, which orchestrates the activities of the Mobility Service, Process Servers, and Master Target servers connected to the Configuration Server.</span></span> <span data-ttu-id="d38e0-187">Konfigurace serveru webový server používá certifikát SSL k ověřování svým klientům.</span><span class="sxs-lookup"><span data-stu-id="d38e0-187">The Configuration Server's webserver uses an SSL certificate to authenticate its clients.</span></span> <span data-ttu-id="d38e0-188">Tento certifikát má vypršela platnost tři roky a lze ji obnovit kdykoli použití následující metody:</span><span class="sxs-lookup"><span data-stu-id="d38e0-188">This certificate has an expiry of three years and can be renewed at any time using the following method:</span></span>

> [!WARNING]
<span data-ttu-id="d38e0-189">Vypršení platnosti certifikátu lze provést pouze na verze 9.4.XXXX. X nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="d38e0-189">Certificate expiry can be performed only on version 9.4.XXXX.X or higher.</span></span> <span data-ttu-id="d38e0-190">Upgradovat všechny součásti Azure Site Recovery (konfigurační Server, Server procesu, hlavní cílový Server, služba Mobility) předtím, než můžete spustit pracovní postup obnovit certifikáty.</span><span class="sxs-lookup"><span data-stu-id="d38e0-190">Upgrade all the Azure Site Recovery components (Configuration Server, Process Server, Master Target Server, Mobility Service) before you start the Renew Certificates workflow.</span></span>

1. <span data-ttu-id="d38e0-191">Na portálu Azure přejděte do trezoru > infrastruktura Site Recovery > konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="d38e0-191">On the Azure portal, browse to your Vault > Site Recovery Infrastructure > Configuration Server.</span></span>
2. <span data-ttu-id="d38e0-192">Klikněte na konfigurační Server, pro kterou je potřeba obnovit certifikát SSL pro.</span><span class="sxs-lookup"><span data-stu-id="d38e0-192">Click the Configuration Server for which you need to renew the SSL Certificate for.</span></span>
3. <span data-ttu-id="d38e0-193">V části stav konfigurace serveru zobrazí se datum vypršení platnosti pro certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="d38e0-193">Under the Configuration Server health, you can see the expiry date for the SSL Certificate.</span></span>
4. <span data-ttu-id="d38e0-194">Obnovit certifikát tak, že kliknete **obnovit certifikáty** akce, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="d38e0-194">Renew the certificate by clicking the **Renew Certificates** action as shown in the following image:</span></span>

  ![odstranění. konfigurační server](./media/site-recovery-vmware-to-azure-manage-configuration-server/renew-cert-page.png)

### <a name="secure-socket-layer-certificate-expiry-warning"></a><span data-ttu-id="d38e0-196">Secure Socket Layer varování vypršení platnosti certifikátu</span><span class="sxs-lookup"><span data-stu-id="d38e0-196">Secure Socket Layer certificate expiry warning</span></span>

> [!NOTE]
<span data-ttu-id="d38e0-197">Platnost certifikátu SSL pro všechny instalace, které bylo provedeno před 2016 pravděpodobně byl nastaven na jeden rok.</span><span class="sxs-lookup"><span data-stu-id="d38e0-197">The SSL Certificate's validity for all installations that happened before May 2016 was set to one year.</span></span> <span data-ttu-id="d38e0-198">jste spustili zobrazuje oznámení vypršení platnosti certifikátu zobrazovat na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d38e0-198">you have started seeing certificate expiry notifications showing up in the Azure portal.</span></span>

1. <span data-ttu-id="d38e0-199">Pokud vyprší v následujících dvou měsíců konfigurace serverového certifikátu protokolu SSL, bude spuštěna služba oznámení uživatelům pomocí portálu Azure & e-mailu (potřebujete přihlásit k odběru oznámení Azure Site Recovery).</span><span class="sxs-lookup"><span data-stu-id="d38e0-199">If the Configuration Server's SSL certificate is going to expire in the next two months, the service starts notifying users via the Azure portal & email (you need to be subscribed to Azure Site Recovery notifications).</span></span> <span data-ttu-id="d38e0-200">Spuštění zobrazuje zpráva, která upozorňuje na stránce prostředků v úložišti.</span><span class="sxs-lookup"><span data-stu-id="d38e0-200">You start seeing a notification banner on the Vault's resource page.</span></span>

  ![certifikát – oznámení](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-renew-notification.png)
2. <span data-ttu-id="d38e0-202">Klikněte na informační zprávě zobrazíte další podrobnosti k vypršení platnosti certifikátu.</span><span class="sxs-lookup"><span data-stu-id="d38e0-202">Click the banner to get additional details on the Certificate expiry.</span></span>

  ![Podrobnosti o certifikátu](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-expiry-details.png)

  >[!TIP]
  <span data-ttu-id="d38e0-204">Pokud místo **obnovit nyní** tlačítko se zobrazí **upgradovat nyní** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d38e0-204">If instead of a **Renew Now** button you see an **Upgrade Now** button.</span></span> <span data-ttu-id="d38e0-205">To znamená, že jsou některé součásti ve vašem prostředí, které dosud nebyly upgradovány na 9.4.xxxx.x nebo vyšší verze.</span><span class="sxs-lookup"><span data-stu-id="d38e0-205">This means that there are some components in your environment that have not yet been upgraded to 9.4.xxxx.x or higher versions.</span></span>

## <a name="sizing-requirements-for-a-configuration-server"></a><span data-ttu-id="d38e0-206">Změna velikosti požadavky konfigurace serveru</span><span class="sxs-lookup"><span data-stu-id="d38e0-206">Sizing requirements for a Configuration Server</span></span>

| <span data-ttu-id="d38e0-207">**VYUŽITÍ PROCESORU**</span><span class="sxs-lookup"><span data-stu-id="d38e0-207">**CPU**</span></span> | <span data-ttu-id="d38e0-208">**Paměť**</span><span class="sxs-lookup"><span data-stu-id="d38e0-208">**Memory**</span></span> | <span data-ttu-id="d38e0-209">**Velikost disku mezipaměti**</span><span class="sxs-lookup"><span data-stu-id="d38e0-209">**Cache disk size**</span></span> | <span data-ttu-id="d38e0-210">**Míry změny dat**</span><span class="sxs-lookup"><span data-stu-id="d38e0-210">**Data change rate**</span></span> | <span data-ttu-id="d38e0-211">**Chráněné počítače**</span><span class="sxs-lookup"><span data-stu-id="d38e0-211">**Protected machines**</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="d38e0-212">8 Vcpu (2 sockets * @ 2,5 GHz 4 jádra)</span><span class="sxs-lookup"><span data-stu-id="d38e0-212">8 vCPUs (2 sockets * 4 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="d38e0-213">16 GB</span><span class="sxs-lookup"><span data-stu-id="d38e0-213">16 GB</span></span> |<span data-ttu-id="d38e0-214">300 GB</span><span class="sxs-lookup"><span data-stu-id="d38e0-214">300 GB</span></span> |<span data-ttu-id="d38e0-215">500 GB nebo méně</span><span class="sxs-lookup"><span data-stu-id="d38e0-215">500 GB or less</span></span> |<span data-ttu-id="d38e0-216">Replikovat počítače méně než 100.</span><span class="sxs-lookup"><span data-stu-id="d38e0-216">Replicate fewer than 100 machines.</span></span> |
| <span data-ttu-id="d38e0-217">12 Vcpu (2 sockets * @ 2,5 GHz 6 jader)</span><span class="sxs-lookup"><span data-stu-id="d38e0-217">12 vCPUs (2 sockets * 6 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="d38e0-218">18 GB</span><span class="sxs-lookup"><span data-stu-id="d38e0-218">18 GB</span></span> |<span data-ttu-id="d38e0-219">600 GB</span><span class="sxs-lookup"><span data-stu-id="d38e0-219">600 GB</span></span> |<span data-ttu-id="d38e0-220">500 GB až 1 TB</span><span class="sxs-lookup"><span data-stu-id="d38e0-220">500 GB to 1 TB</span></span> |<span data-ttu-id="d38e0-221">Replikovat mezi 100 150 počítačů.</span><span class="sxs-lookup"><span data-stu-id="d38e0-221">Replicate between 100-150 machines.</span></span> |
| <span data-ttu-id="d38e0-222">16 Vcpu (2 sockets * @ 2,5 GHz 8 jader)</span><span class="sxs-lookup"><span data-stu-id="d38e0-222">16 vCPUs (2 sockets * 8 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="d38e0-223">32 GB</span><span class="sxs-lookup"><span data-stu-id="d38e0-223">32 GB</span></span> |<span data-ttu-id="d38e0-224">1 TB</span><span class="sxs-lookup"><span data-stu-id="d38e0-224">1 TB</span></span> |<span data-ttu-id="d38e0-225">1 TB 2 TB</span><span class="sxs-lookup"><span data-stu-id="d38e0-225">1 TB to 2 TB</span></span> |<span data-ttu-id="d38e0-226">Replikovat mezi 150 až 200 počítačů.</span><span class="sxs-lookup"><span data-stu-id="d38e0-226">Replicate between 150-200 machines.</span></span> |

  >[!TIP]
  <span data-ttu-id="d38e0-227">Pokud vaše denním pohybem dat je větší než 2 TB, nebo máte v plánu replikovat více než 200 virtuálních počítačů, doporučujeme nasadit další proces servery pro provoz replikace Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="d38e0-227">If your daily data churn exceeds 2 TB, or you plan to replicate more than 200 virtual machines, it is recommended to deploy additional process servers to load balance the replication traffic.</span></span> <span data-ttu-id="d38e0-228">Další informace o tom, jak nasadit proces Škálováním na více systémů servery.</span><span class="sxs-lookup"><span data-stu-id="d38e0-228">Learn more about How to deploy Scale-out Process severs.</span></span>


## <a name="common-issues"></a><span data-ttu-id="d38e0-229">Běžné problémy</span><span class="sxs-lookup"><span data-stu-id="d38e0-229">Common issues</span></span>
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]
