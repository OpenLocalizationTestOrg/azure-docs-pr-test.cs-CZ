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
# <a name="manage-a-configuration-server"></a>Správa konfigurace serveru

Konfigurační Server funguje jako koordinátor mezi služby Site Recovery a na místní infrastrukturu. Tento článek popisuje, jak můžete nastavit, konfigurovat a spravovat konfigurační Server.

## <a name="prerequisites"></a>Požadavky
Toto jsou minimální hardware, software a konfiguraci sítě, které jsou nezbytné k nastavení konfigurace serveru.

> [!NOTE]
> [Plánování kapacity](site-recovery-capacity-planner.md) je důležitý krok zajistit nasazení konfigurace serveru s konfigurací této sady požadavků na zatížení. Další informace o [Změna velikosti požadavky pro konfiguraci serveru](#sizing-requirements-for-a-configuration-server).

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-the-configuration-server-software"></a>Stahování softwaru konfigurační Server
1. Přihlaste se k portálu Azure a přejděte do trezoru služeb zotavení.
2. Přejděte do **infrastruktury obnovení lokality** > **konfigurační servery** (v části pro VMware a fyzické počítače).

  ![Přidat stránku servery](./media/site-recovery-vmware-to-azure-manage-configuration-server/AddServers.png)
3. Klikněte **+ servery** tlačítko.
4. Na **přidat Server** klikněte na tlačítko Stáhnout na stáhnout registrační klíč. Je nutné tento klíč během instalace konfigurační Server a zaregistrujte ho pomocí služby Azure Site Recovery.
5. Klikněte **stáhnout instalaci služby Microsoft Azure Site Recovery Unified** odkaz ke stažení nejnovější verze konfigurace serveru.

  ![Stažení stránky](./media/site-recovery-vmware-to-azure-manage-configuration-server/downloadcs.png)

  > [!TIP]
  Nejnovější verze konfigurace serveru si můžete stáhnout přímo z [stránky pro stažení Microsoft Download Center](http://aka.ms/unifiedsetup)

## <a name="installing-and-registering-a-configuration-server-from-gui"></a>Instalace a registrace konfigurace serveru z grafického uživatelského rozhraní
[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

## <a name="installing-and-registering-a-configuration-server-using-command-line"></a>Instalace a registrace konfigurace serveru pomocí příkazového řádku

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a>Využití vzorků
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="configuration-server-installer-command-line-arguments"></a>Konfigurace serveru instalační program argumenty příkazového řádku.
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-mysql-credentials-file"></a>Vytvořte soubor přihlašovacích údajů MySql
Parametr MySQLCredsFilePath vezme jako vstupní soubor. Vytvořte soubor v následujícím formátu a předejte jej jako vstupní parametr MySQLCredsFilePath.
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-a-proxy-settings-configuration-file"></a>Vytvoření souboru konfiguračního nastavení proxy serveru
Parametr ProxySettingsFilePath vezme jako vstupní soubor. Vytvořte soubor v následujícím formátu a předejte jej jako vstupní parametr ProxySettingsFilePath.

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-configuration-server"></a>Úprava nastavení proxy serveru pro konfigurační Server
1. Přihlášení k konfigurační Server.
2. Spusťte cspsconfigtool.exe pomocí zástupce na vaše.
3. Klikněte **registrace trezoru** kartě.
4. Stáhněte si nový soubor registrace trezoru z portálu a zadejte jako vstup pro nástroj.

  ![Register – konfigurace serveru](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
5. Zadejte nové podrobnosti o Proxy serveru a klikněte na **zaregistrovat** tlačítko.
6. Otevřete okno příkazového prostředí PowerShell pro správu.
7. Spusťte následující příkaz
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  Pokud máte Škálováním na více systémů proces servery připojené k této konfigurační Server, budete muset [opravte nastavení proxy serveru na všech serverech Škálováním na více systémů proces](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) ve vašem nasazení.

## <a name="re-register-a-configuration-server-with-the-same-recovery-services-vault"></a>Znovu zaregistrujte konfigurační Server se stejným trezoru služeb zotavení
  1. Přihlášení k konfigurační Server.
  2. Spusťte cspsconfigtool.exe pomocí zástupce na ploše.
  3. Klikněte **registrace trezoru** kartě.
  4. Stáhněte si nový soubor registrace z portálu a zadejte jako vstup pro nástroj.
        ![Register – konfigurace serveru](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
  5. Zadejte podrobnosti o Proxy serveru a klikněte na **zaregistrovat** tlačítko.  
  6. Otevřete okno příkazového prostředí PowerShell pro správu.
  7. Spusťte následující příkaz

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

  >[!WARNING]
  Pokud máte Škálováním na více systémů proces servery připojené k této konfigurační Server, budete muset [znovu registraci všech serverů škálovaného na více systémů proces](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) ve vašem nasazení.

## <a name="registering-a-configuration-server-with-a-different-recovery-services-vault"></a>Registrace konfigurace serveru s jinou trezoru služeb zotavení.
1. Přihlášení k konfigurační Server.
2. z příkazového řádku správce spusťte příkaz

```
reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
net stop dra
```
3. Spusťte cspsconfigtool.exe pomocí zástupce na vaše.
4. Klikněte **registrace trezoru** kartě.
5. Stáhněte si nový soubor registrace z portálu a zadejte jako vstup pro nástroj.

    ![Register – konfigurace serveru](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
6. Zadejte podrobnosti o Proxy serveru a klikněte na **zaregistrovat** tlačítko.  
7. Otevřete okno příkazového prostředí PowerShell pro správu.
8. Spusťte následující příkaz
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="decommissioning-a-configuration-server"></a>Vyřazení z provozu konfigurace serveru
Zkontrolujte následující před zahájením vyřazení z provozu konfigurační Server.
1. Zakažte ochranu pro všechny virtuální počítače v rámci této konfigurace serveru.
2. Zrušit přidružení všechny zásady replikace z konfiguračního serveru.
3. Odstraňte všechny hostitele Vcenter vSphere servery, které jsou přidružené k konfigurační Server.

### <a name="delete-the-configuration-server-from-azure-portal"></a>Odstraňte konfigurační Server z portálu Azure
1. Na portálu Azure, přejděte do **infrastruktura Site Recovery** > **konfigurační servery** nabídce trezoru.
2. Klikněte na konfigurační Server, který chcete vyřadit z provozu.
3. Na stránce s podrobnostmi o konfigurační Server klikněte na tlačítko Odstranit.

  ![odstranění. konfigurační server](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.PNG)
4. Klikněte na tlačítko **Ano** potvrďte odstranění serveru.

  >[!WARNING]
  Pokud máte jakékoli virtuálních počítačů, zásad replikace nebo hostitelů vSphere servery vCenter přidružené k této konfiguraci serveru, nelze odstranit server. Před pokusem o odstranění trezoru, odstraňte tyto entity.

### <a name="uninstall-the-configuration-server-software-and-its-dependencies"></a>Odinstalujte software konfigurační Server a jeho závislosti
  > [!TIP]
  Pokud budete chtít použít konfigurační Server s Azure Site Recovery znovu, potom můžete přeskočit ke kroku 4 přímo

1. Přihlaste se k serveru Configuration jako správce.
2. Otevřete ovládací panely > Program > odinstalovat programy
3. Odinstalujte programy v tomto pořadí:
  * Agent Microsoft Azure Recovery Services
  * Microsoft Azure Site Recovery Mobility služby nebo hlavní cílový server
  * Zprostředkovatele služby Microsoft Azure Site Recovery
  * Microsoft Azure Site obnovení konfigurace serveru nebo procesový Server
  * Microsoft Azure Site Recovery konfigurace serveru závislosti
  * MySQL Server 5.5
4. Spusťte následující příkaz a správu příkazového řádku.
  ```
  reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
  ```

## <a name="renew-configuration-server-secure-socket-layerssl-certificates"></a>Obnovení konfigurace serveru Secure Socket Layer(SSL) certifikátů
Konfigurační Server má integrované webový server, která orchestruje aktivity služby Mobility, proces servery a hlavního cíle serverů připojených k serveru Configuration. Konfigurace serveru webový server používá certifikát SSL k ověřování svým klientům. Tento certifikát má vypršela platnost tři roky a lze ji obnovit kdykoli použití následující metody:

> [!WARNING]
Vypršení platnosti certifikátu lze provést pouze na verze 9.4.XXXX. X nebo vyšší. Upgradovat všechny součásti Azure Site Recovery (konfigurační Server, Server procesu, hlavní cílový Server, služba Mobility) předtím, než můžete spustit pracovní postup obnovit certifikáty.

1. Na portálu Azure přejděte do trezoru > infrastruktura Site Recovery > konfigurační Server.
2. Klikněte na konfigurační Server, pro kterou je potřeba obnovit certifikát SSL pro.
3. V části stav konfigurace serveru zobrazí se datum vypršení platnosti pro certifikát SSL.
4. Obnovit certifikát tak, že kliknete **obnovit certifikáty** akce, jak je znázorněno na následujícím obrázku:

  ![odstranění. konfigurační server](./media/site-recovery-vmware-to-azure-manage-configuration-server/renew-cert-page.png)

### <a name="secure-socket-layer-certificate-expiry-warning"></a>Secure Socket Layer varování vypršení platnosti certifikátu

> [!NOTE]
Platnost certifikátu SSL pro všechny instalace, které bylo provedeno před 2016 pravděpodobně byl nastaven na jeden rok. jste spustili zobrazuje oznámení vypršení platnosti certifikátu zobrazovat na portálu Azure.

1. Pokud vyprší v následujících dvou měsíců konfigurace serverového certifikátu protokolu SSL, bude spuštěna služba oznámení uživatelům pomocí portálu Azure & e-mailu (potřebujete přihlásit k odběru oznámení Azure Site Recovery). Spuštění zobrazuje zpráva, která upozorňuje na stránce prostředků v úložišti.

  ![certifikát – oznámení](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-renew-notification.png)
2. Klikněte na informační zprávě zobrazíte další podrobnosti k vypršení platnosti certifikátu.

  ![Podrobnosti o certifikátu](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-expiry-details.png)

  >[!TIP]
  Pokud místo **obnovit nyní** tlačítko se zobrazí **upgradovat nyní** tlačítko. To znamená, že jsou některé součásti ve vašem prostředí, které dosud nebyly upgradovány na 9.4.xxxx.x nebo vyšší verze.

## <a name="sizing-requirements-for-a-configuration-server"></a>Změna velikosti požadavky konfigurace serveru

| **VYUŽITÍ PROCESORU** | **Paměť** | **Velikost disku mezipaměti** | **Míry změny dat** | **Chráněné počítače** |
| --- | --- | --- | --- | --- |
| 8 Vcpu (2 sockets * @ 2,5 GHz 4 jádra) |16 GB |300 GB |500 GB nebo méně |Replikovat počítače méně než 100. |
| 12 Vcpu (2 sockets * @ 2,5 GHz 6 jader) |18 GB |600 GB |500 GB až 1 TB |Replikovat mezi 100 150 počítačů. |
| 16 Vcpu (2 sockets * @ 2,5 GHz 8 jader) |32 GB |1 TB |1 TB 2 TB |Replikovat mezi 150 až 200 počítačů. |

  >[!TIP]
  Pokud vaše denním pohybem dat je větší než 2 TB, nebo máte v plánu replikovat více než 200 virtuálních počítačů, doporučujeme nasadit další proces servery pro provoz replikace Vyrovnávání zatížení. Další informace o tom, jak nasadit proces Škálováním na více systémů servery.


## <a name="common-issues"></a>Běžné problémy
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]
