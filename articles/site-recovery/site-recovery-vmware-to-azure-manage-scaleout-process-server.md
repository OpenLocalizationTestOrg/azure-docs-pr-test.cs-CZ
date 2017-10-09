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
# <a name="manage-a-scale-out-process-server"></a>Správa serveru proces Škálováním na více systémů

Škálováním na více systémů procesový Server funguje jako koordinátor pro přenos dat mezi hello služby Site Recovery a na místní infrastrukturu. Tento článek popisuje, jak můžete nastavit, konfigurovat a spravovat proces serveru Škálovaného na více systémů.

## <a name="prerequisites"></a>Požadavky
Následující Hello jsou hello doporučené hardwaru, softwaru a tooset nezbytné konfigurace sítě server proces Škálováním na více systémů.

> [!NOTE]
> [Plánování kapacity](site-recovery-capacity-planner.md) je důležitým krokem tooensure nasazení hello procesového serveru Škálovaného na více systémů s konfigurací této sady požadavků na zatížení. Další informace o [škálování vlastnosti pro Server se Škálováním na proces](#sizing-requirements-for-a-configuration-server).

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-hello-scale-out-process-server-software"></a>Stahování softwaru Škálováním na více systémů procesový Server hello
1. Přihlaste se toohello Azure portal a procházení tooyour trezoru služeb zotavení.
2. Procházet příliš**infrastruktura Site Recovery** > **konfigurační servery** (v rámci For VMware a fyzické počítače).
3. Vyberte vaše konfigurace serveru toodrill dolů na stránce s podrobnostmi o hello konfigurační server.
4. Klikněte na tlačítko hello **+ procesový Server** tlačítko.
5. V hello **přidejte procesový server** vyberte **místní Server proces nasazení horizontální** možnost hello **vyberte místo toodeploy procesový server** rozevírací seznam.

  ![Přidat stránku servery](./media/site-recovery-vmware-to-azure-manage-scaleout-process-server/add-process-server.png)
6. Klikněte na tlačítko hello **stažení hello Unified instalace serveru Microsoft Azure Site Recovery** odkaz toodownload hello nejnovější verzi instalace procesový Server hello Škálováním na více systémů.

  > [!WARNING]
  Hello verzi procesový Server Škálováním na více systémů by měly být shodné tooor dřívější než verze konfigurační Server hello ve vašem prostředí. Verze kompatibility tooensure jednoduchý způsob je toouse hello stejné bits instalační program, který byl nedávno použit tooinstall nebo aktualizovat konfigurační Server.

## <a name="installing-and-registering-a-scale-out-process-server-from-gui"></a>Instalace a registrace serveru se Škálováním na proces z grafického uživatelského rozhraní
Pokud máte tooscale škálování vašeho nasazení nad 200 zdrojového počítače nebo celkový denně změn počet více než 2 TB, je třeba objem provozu pro další proces servery toohandle hello.

Zkontrolujte hello [velikost doporučení pro servery proces](#size-recommendations-for-the-process-server)a pak postupujte podle těchto pokynů tooset hello procesu serveru. Po nastavení hello serveru, migrovat toouse počítače zdrojového ho.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="installing-and-registering-a-scale-out-process-server-using-command-line"></a>Instalace a registrace serveru se Škálováním na proces pomocí příkazového řádku

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address toobe used for data transfer] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>]
```

### <a name="sample-usage"></a>Využití vzorků
```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```

### <a name="scale-out-process-server-installer-command-line-arguments"></a>Škálováním na více systémů procesový Server instalační program argumenty příkazového řádku.
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-proxy-settings-configuration-file"></a>Vytvoření souboru konfiguračního nastavení proxy serveru
Parametr ProxySettingsFilePath vezme jako vstupní soubor. Vytvoření souboru pomocí hello následující formátování a předejte ji jako vstupní parametr ProxySettingsFilePath.
```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-scale-out-process-server"></a>Úprava nastavení proxy serveru pro škálování procesového serveru
1. Přihlásit se k serveru proces Škálováním na více systémů.
2. Otevřete okno příkazového prostředí PowerShell pro správu.
3. Spusťte následující příkaz hello
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
4. Další Procházet adresář toohello **%PROGRAMDATA%\ASR\Agent** a hello spusťte následující příkaz
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```

## <a name="re-registering-a-scale-out-process-server"></a>Opakováním registrace serveru proces Škálováním na více systémů
[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

* Dále otevřete příkazový řádek správce.
* Procházet adresář toohello **%PROGRAMDATA%\ASR\Agent** a spusťte příkaz hello

```
cdpcli.exe --registermt

net stop obengine

net start obengine
```

## <a name="upgrading-a-scale-out-process-server"></a>Upgrade serveru proces Škálováním na více systémů
[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

## <a name="decommissioning-a-scale-out-process-server"></a>Vyřazení z provozu Škálováním na více systémů procesového serveru
1. Zajistěte, aby:
  - Zobrazuje stav připojení konfigurační Server jako **připojeno** v hello portálu Azure
  - Procesový Server je stále možné toocommunicate s hello konfigurační server.
2. Přihlaste se jako správce toohello procesového serveru
3. Otevřete ovládací panely > Program > odinstalovat programy
4. Odinstalujte hello programy v pořadí hello zadané následující:
  * Microsoft Azure Site obnovení konfigurace serveru nebo procesový Server
  * Microsoft Azure Site Recovery konfigurace serveru závislosti
  * Agent Microsoft Azure Recovery Services

V hello portálu Azure může trvat až too15 minut tooreflect odstranění hello procesový Server.

  > [!NOTE]
  Pokud je server hello procesu nelze toocommunicate s hello konfigurační Server (stav připojení portálu není připojen), pak je nutné toofollow hello následující kroky toopurge ji z hello konfigurační Server.

## <a name="unregistering-a-disconnected-scale-out-process-server-from-a-configuration-server"></a>Zrušení registrace odpojený server proces Škálováním na více systémů z konfigurace serveru

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="sizing-requirements-for-a-scale-out-process-server"></a>Změna velikosti požadavky pro Server proces Škálováním na více systémů

| **Další procesového serveru** | **Velikost disku mezipaměti** | **Míry změny dat** | **Chráněné počítače** |
| --- | --- | --- | --- |
|4 Vcpu (2 sockets * 2 jádra @ 2,5 GHz), 8 GB paměti |300 GB |250 GB nebo méně |Replikovat počítače 85 nebo méně. |
|8 Vcpu (2 sockets * 4 jádra @ 2,5 GHz), 12 GB paměti |600 GB |TB too1 250 GB |Replikovat mezi 85 150 počítačů. |
|12 Vcpu (2 sockets * @ 2,5 GHz 6 jader) 24 GB paměti |1 TB |1 TB too2 TB |Replikovat mezi 150 225 počítačů. |
