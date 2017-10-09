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
# <a name="manage-a-configuration-server"></a>Správa konfigurace serveru

Konfigurační Server funguje jako koordinátor mezi hello služby Site Recovery a na místní infrastrukturu. Tento článek popisuje, jak můžete nastavit, konfigurovat a spravovat hello konfigurační Server.

## <a name="prerequisites"></a>Požadavky
Hello následují hello minimální hardware, software a síťové konfigurace požadovaná tooset až konfigurační Server.

> [!NOTE]
> [Plánování kapacity](site-recovery-capacity-planner.md) je důležitým krokem tooensure nasazení hello konfigurační Server s konfigurací této sady požadavků na zatížení. Další informace o [Změna velikosti požadavky pro konfiguraci serveru](#sizing-requirements-for-a-configuration-server).

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-hello-configuration-server-software"></a>Stahování softwaru konfigurační Server hello
1. Přihlaste se toohello Azure portal a procházení tooyour trezoru služeb zotavení.
2. Procházet příliš**infrastruktura Site Recovery** > **konfigurační servery** (v rámci For VMware a fyzické počítače).

  ![Přidat stránku servery](./media/site-recovery-vmware-to-azure-manage-configuration-server/AddServers.png)
3. Klikněte na tlačítko hello **+ servery** tlačítko.
4. Na hello **přidat Server** klikněte na tlačítko hello stažení tlačítko toodownload hello registrační klíč. Během tooregister instalace konfigurační Server hello potřebujete tento klíč se službou Azure Site Recovery.
5. Klikněte na tlačítko hello **stažení hello Unified instalace serveru Microsoft Azure Site Recovery** odkaz toodownload hello nejnovější verzi hello konfigurační Server.

  ![Stažení stránky](./media/site-recovery-vmware-to-azure-manage-configuration-server/downloadcs.png)

  > [!TIP]
  Nejnovější verzi hello konfigurační Server si můžete stáhnout přímo z [stránky pro stažení Microsoft Download Center](http://aka.ms/unifiedsetup)

## <a name="installing-and-registering-a-configuration-server-from-gui"></a>Instalace a registrace konfigurace serveru z grafického uživatelského rozhraní
[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

## <a name="installing-and-registering-a-configuration-server-using-command-line"></a>Instalace a registrace konfigurace serveru pomocí příkazového řádku

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address toobe used for data transfer] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>]
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
Parametr MySQLCredsFilePath vezme jako vstupní soubor. Vytvoření souboru hello pomocí hello následující formátování a předejte ji jako vstupní parametr MySQLCredsFilePath.
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-a-proxy-settings-configuration-file"></a>Vytvoření souboru konfiguračního nastavení proxy serveru
Parametr ProxySettingsFilePath vezme jako vstupní soubor. Vytvoření souboru hello pomocí hello následující formátování a předejte ji jako vstupní parametr ProxySettingsFilePath.

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-configuration-server"></a>Úprava nastavení proxy serveru pro konfigurační Server
1. Přihlášení tooyour konfigurační Server.
2. Spusťte hello cspsconfigtool.exe pomocí zástupce hello na vaše.
3. Klikněte na tlačítko hello **registrace trezoru** kartě.
4. Stáhněte si nový soubor registrace trezoru z portálu hello a zadat jako vstupní toohello nástroj.

  ![Register – konfigurace serveru](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
5. Zadejte hello nové podrobnosti o Proxy serveru a klikněte na hello **zaregistrovat** tlačítko.
6. Otevřete okno příkazového prostředí PowerShell pro správu.
7. Spusťte následující příkaz hello
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  Pokud máte Škálováním na více systémů proces servery připojené toothis konfigurační Server, musíte příliš[opravte hello nastavení proxy serveru na všech serverech Škálováním na více systémů proces hello](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) ve vašem nasazení.

## <a name="re-register-a-configuration-server-with-hello-same-recovery-services-vault"></a>Znovu zaregistrujte konfigurační Server s hello stejné trezoru služeb zotavení
  1. Přihlášení tooyour konfigurační Server.
  2. Spusťte hello cspsconfigtool.exe pomocí hello zástupce na ploše.
  3. Klikněte na tlačítko hello **registrace trezoru** kartě.
  4. Stáhněte si nový soubor registračního z portálu hello a poskytnout ho jako vstupní toohello nástroj.
        ![Register – konfigurace serveru](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
  5. Zadejte podrobnosti Proxy Server hello a klikněte na tlačítko hello **zaregistrovat** tlačítko.  
  6. Otevřete okno příkazového prostředí PowerShell pro správu.
  7. Spusťte následující příkaz hello

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

  >[!WARNING]
  Pokud máte Škálováním na více systémů proces servery připojené toothis konfigurační Server, musíte příliš[znovu registraci všech serverů škálovaného na více systémů proces hello](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) ve vašem nasazení.

## <a name="registering-a-configuration-server-with-a-different-recovery-services-vault"></a>Registrace konfigurace serveru s jinou trezoru služeb zotavení.
1. Přihlášení tooyour konfigurační Server.
2. z příkazového řádku správce spusťte příkaz hello

```
reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
net stop dra
```
3. Spusťte hello cspsconfigtool.exe pomocí zástupce hello na vaše.
4. Klikněte na tlačítko hello **registrace trezoru** kartě.
5. Stáhněte si nový soubor registračního z portálu hello a poskytnout ho jako vstupní toohello nástroj.

    ![Register – konfigurace serveru](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
6. Zadejte podrobnosti Proxy Server hello a klikněte na tlačítko hello **zaregistrovat** tlačítko.  
7. Otevřete okno příkazového prostředí PowerShell pro správu.
8. Spusťte následující příkaz hello
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="decommissioning-a-configuration-server"></a>Vyřazení z provozu konfigurace serveru
Zkontrolujte následující hello před zahájením vyřazení z provozu konfigurační Server.
1. Zakažte ochranu pro všechny virtuální počítače v rámci této konfigurace serveru.
2. Zrušit přidružení všechny zásady replikace z hello konfigurační Server.
3. Odstraňte všechny hostitele Vcenter vSphere servery, které jsou přidružené toohello konfigurační Server.

### <a name="delete-hello-configuration-server-from-azure-portal"></a>Odstraňte konfigurační Server hello z portálu Azure
1. Na portálu Azure, procházet příliš**infrastruktura Site Recovery** > **konfigurační servery** nabídce trezoru hello.
2. Klikněte na tlačítko hello konfigurační Server, které chcete toodecommission.
3. Na stránce s podrobnostmi o hello konfigurační Server klikněte na tlačítko Odstranit hello.

  ![odstranění. konfigurační server](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.PNG)
4. Klikněte na tlačítko **Ano** tooconfirm hello odstranění serveru hello.

  >[!WARNING]
  Pokud máte jakékoli virtuálních počítačů, zásad replikace nebo hostitelů vSphere servery vCenter přidružené k této konfiguraci serveru, nelze odstranit hello server. Odstraňte tyto entity, než se pokusíte toodelete hello trezoru.

### <a name="uninstall-hello-configuration-server-software-and-its-dependencies"></a>Odinstalujte software konfigurační Server hello a jeho závislosti
  > [!TIP]
  Pokud máte v úmyslu znovu tooreuse hello konfigurační Server s Azure Site Recovery, pak můžete přeskočit toostep 4 přímo

1. Toohello konfiguračním serveru se přihlaste jako správce.
2. Otevřete ovládací panely > Program > odinstalovat programy
3. Odinstalujte programy hello v hello následující pořadí:
  * Agent Microsoft Azure Recovery Services
  * Microsoft Azure Site Recovery Mobility služby nebo hlavní cílový server
  * Zprostředkovatele služby Microsoft Azure Site Recovery
  * Microsoft Azure Site obnovení konfigurace serveru nebo procesový Server
  * Microsoft Azure Site Recovery konfigurace serveru závislosti
  * MySQL Server 5.5
4. Spusťte následující příkaz z hello a správu příkazového řádku.
  ```
  reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
  ```

## <a name="renew-configuration-server-secure-socket-layerssl-certificates"></a>Obnovení konfigurace serveru Secure Socket Layer(SSL) certifikátů
Hello konfigurační Server je integrované webový server, která orchestruje hello činnosti hello služby Mobility, proces servery a servery hlavního cíle připojeny toohello konfigurační Server. webový server Hello konfigurační Server používá tooauthenticate certifikát SSL svým klientům. Tento certifikát má vypršela platnost tři roky a lze ji obnovit kdykoli použití hello následující metodu:

> [!WARNING]
Vypršení platnosti certifikátu lze provést pouze na verze 9.4.XXXX. X nebo vyšší. Upgradovat všechny součásti Azure Site Recovery hello (konfigurační Server, Server procesu, hlavní cílový Server, služba Mobility) předtím, než můžete spustit pracovní postup obnovit certifikáty hello.

1. Na portálu Azure text hello, procházet tooyour trezoru > infrastruktura Site Recovery > konfigurační Server.
2. Klikněte na tlačítko hello konfigurační Server, pro kterou je třeba toorenew hello certifikát SSL pro.
3. V části hello stav konfigurace serveru se zobrazí datum vypršení platnosti hello hello certifikát SSL.
4. Obnovit certifikát hello kliknutím hello **obnovit certifikáty** akce, jak ukazuje následující obrázek hello:

  ![odstranění. konfigurační server](./media/site-recovery-vmware-to-azure-manage-configuration-server/renew-cert-page.png)

### <a name="secure-socket-layer-certificate-expiry-warning"></a>Secure Socket Layer varování vypršení platnosti certifikátu

> [!NOTE]
Hello platnosti certifikátu protokolu SSL pro všechny instalace, které bylo provedeno před 2016 pravděpodobně byl nastaven tooone roku. jste spustili zobrazuje oznámení vypršení platnosti certifikátu zobrazovat na portálu Azure hello.

1. Pokud certifikát SSL hello konfigurační Server budete tooexpire v hello následujících dvou měsíců, spustí služba hello oznámení uživatelům pomocí portálu Azure hello & e-mailu (potřebujete předplatné toobe tooAzure Site Recovery oznámení). Spuštění zobrazuje zpráva, která upozorňuje na stránce prostředků hello trezoru.

  ![certifikát – oznámení](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-renew-notification.png)
2. Klikněte na tlačítko hello banner tooget další podrobnosti o vypršení platnosti certifikátu hello.

  ![Podrobnosti o certifikátu](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-expiry-details.png)

  >[!TIP]
  Pokud místo **obnovit nyní** tlačítko se zobrazí **upgradovat nyní** tlačítko. To znamená, že jsou některé součásti ve vašem prostředí, které ještě nebyly upgradované too9.4.xxxx.x nebo vyšší verze.

## <a name="sizing-requirements-for-a-configuration-server"></a>Změna velikosti požadavky konfigurace serveru

| **VYUŽITÍ PROCESORU** | **Paměť** | **Velikost disku mezipaměti** | **Míry změny dat** | **Chráněné počítače** |
| --- | --- | --- | --- | --- |
| 8 Vcpu (2 sockets * @ 2,5 GHz 4 jádra) |16 GB |300 GB |500 GB nebo méně |Replikovat počítače méně než 100. |
| 12 Vcpu (2 sockets * @ 2,5 GHz 6 jader) |18 GB |600 GB |500 GB too1 TB |Replikovat mezi 100 150 počítačů. |
| 16 Vcpu (2 sockets * @ 2,5 GHz 8 jader) |32 GB |1 TB |1 TB too2 TB |Replikovat mezi 150 až 200 počítačů. |

  >[!TIP]
  Pokud vaše denním pohybem dat je větší než 2 TB nebo plánujete tooreplicate více než 200 virtuálních počítačů, se doporučuje toodeploy další proces servery tooload vyrovnávání hello provoz replikace. Další informace o jak servery toodeploy proces Škálováním na více systémů.


## <a name="common-issues"></a>Běžné problémy
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]
