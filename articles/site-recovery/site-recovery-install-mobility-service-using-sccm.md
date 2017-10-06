---
title: "instalace služby Mobility pro Azure Site Recovery pomocí nástroje pro nasazení softwaru aaaAutomate | Microsoft Docs"
description: "Tento článek vám umožňuje automatizovat instalaci služby Mobility pomocí nástroje pro nasazení softwaru jako je System Center Configuration Manager."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 6c883c6d5308dcec6e0628b0c2196b3a12e08ebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-mobility-service-installation-by-using-software-deployment-tools"></a>Instalace služby Mobility automatizovat pomocí nástroje pro nasazení softwaru

>[!IMPORTANT]
Tento dokument předpokládá, že používáte verzi **9.9.4510.1** nebo vyšší.

Tento článek obsahuje příklady použití System Center Configuration Manager toodeploy hello službě Azure Site Recovery Mobility ve vašem datovém centru. Pomocí nástroje pro nasazení softwaru jako nástroje Configuration Manager má hello následující výhody:
* Plánování nasazení nové instalace a upgradu, během plánované údržby pro aktualizace softwaru
* Škálování toohundreds nasazení serverů současně


> [!NOTE]
> Tento článek používá aktivita nasazení hello toodemonstrate System Center Configuration Manager 2012 R2. Instalace služby Mobility může také automatizovat pomocí [Azure Automation a konfigurace požadovaného stavu](site-recovery-automate-mobility-service-install.md).

## <a name="prerequisites"></a>Požadavky
1. Nástroj nasazení softwaru, jako jsou nástroje Configuration Manager, který je už nasazená ve vašem prostředí.
  Vytvořte dvě [kolekce zařízení](https://technet.microsoft.com/library/gg682169.aspx), jeden pro všechny **servery Windows**a druhou pro všechny **servery se systémem Linux**, že chcete tooprotect pomocí Site Recovery.
3. Konfigurační server, který je již zaregistrována k Site Recovery.
4. Zabezpečené síťové sdílené složky (Server Message Block sdílené složky), který je přístupný pomocí serveru nástroje Configuration Manager hello.

## <a name="deploy-mobility-service-on-computers-running-windows"></a>Nasazení služby Mobility na počítačích se systémem Windows
> [!NOTE]
> Tento článek předpokládá, že IP adresa hello hello konfigurační server je 192.168.3.121 a že hello zabezpečené sdílení souborů je \\\ContosoSecureFS\MobilityServiceInstallers.

### <a name="step-1-prepare-for-deployment"></a>Krok 1: Příprava na nasazení
1. Vytvořte složku ve sdílené síťové složce hello a pojmenujte ji **MobSvcWindows**.
2. Přihlaste se tooyour konfigurační server a otevřete příkazový řádek pro správu.
3. Spusťte následující příkazy toogenerate soubor přístupové heslo hello:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Kopírování hello **MobSvc.passphrase** souboru do hello **MobSvcWindows** složky do sdílené síťové složky.
5. Procházet toohello Instalační služby úložiště na konfiguračním serveru hello spuštěním hello následující příkaz:

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. Kopírování hello  **Microsoft automatické obnovení systému\_uživatelský Agent\_*verze*\_Windows\_GA\_*datum* \_ Release.exe** toohello **MobSvcWindows** složky do sdílené síťové složky.
7. Zkopírujte následující kód hello a uložte ho jako **install.bat** do hello **MobSvcWindows** složky.

   > [!NOTE]
   > Nahraďte zástupné symboly hello [CSIP] ve skriptu hello skutečnými hodnotami hello IP adresu konfigurační server.

```DOS
Time /t >> C:\Temp\logfile.log
REM ==================================================
REM ==== Clean up hello folders ========================
RMDIR /S /q %temp%\MobSvc
MKDIR %Temp%\MobSvc
MKDIR C:\Temp
REM ==================================================

REM ==== Copy new files ==============================
COPY M*.* %Temp%\MobSvc
CD %Temp%\MobSvc
REN Micro*.exe MobSvcInstaller.exe
REM ==================================================

REM ==== Extract hello installer =======================
MobSvcInstaller.exe /q /x:%Temp%\MobSvc\Extracted
REM ==== Wait 10s for extraction toocomplete =========
TIMEOUT /t 10
REM =================================================

REM ==== Perform installation =======================
REM =================================================

CD %Temp%\MobSvc\Extracted
whoami >> C:\Temp\logfile.log
SET PRODKEY=HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
REG QUERY %PRODKEY%\{275197FC-14FD-4560-A5EB-38217F80CBD1}
IF NOT %ERRORLEVEL% EQU 0 (
    echo "Product is not installed. Goto INSTALL." >> C:\Temp\logfile.log
    GOTO :INSTALL
) ELSE (
    echo "Product is installed." >> C:\Temp\logfile.log

    echo "Checking for Post-install action status." >> C:\Temp\logfile.log
    GOTO :POSTINSTALLCHECK
)

:POSTINSTALLCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "PostInstallActions" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Post-install actions succeeded. Checking for Configuration status." >> C:\Temp\logfile.log
        GOTO :CONFIGURATIONCHECK
    ) ELSE (
        echo "Post-install actions didn't succeed. Goto INSTALL." >> C:\Temp\logfile.log
        GOTO :INSTALL
    )

:CONFIGURATIONCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "AgentConfigurationStatus" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded. Goto UPGRADE." >> C:\Temp\logfile.log
        GOTO :UPGRADE
    ) ELSE (
        echo "Configuration didn't succeed. Goto CONFIGURE." >> C:\Temp\logfile.log
        GOTO :CONFIGURE
    )


:INSTALL
    echo "Perform installation." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Role MS /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Installation has succeeded." >> C:\Temp\logfile.log
        (GOTO :CONFIGURE)
    ) ELSE (
        echo "Installation has failed." >> C:\Temp\logfile.log
        GOTO :ENDSCRIPT
    )

:CONFIGURE
    echo "Perform configuration." >> C:\Temp\logfile.log
    cd "C:\Program Files (x86)\Microsoft Azure Site Recovery\agent"
    UnifiedAgentConfigurator.exe  /CSEndPoint "[CSIP]" /PassphraseFilePath %Temp%\MobSvc\MobSvc.passphrase
    IF %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Configuration has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:UPGRADE
    echo "Perform upgrade." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Upgrade has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Upgrade has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:ENDSCRIPT
    echo "End of script." >> C:\Temp\logfile.log


```

### <a name="step-2-create-a-package"></a>Krok 2: Vytvoření balíčku

1. Přihlaste se tooyour Konzola nástroje Configuration Manager.
2. Procházet příliš**softwarová knihovna** > **Správa aplikací** > **balíčky**.
3. Klikněte pravým tlačítkem na **balíčky**a vyberte **vytvořit balíček**.
4. Zadejte hodnoty pro hello název, popis, výrobce, jazyk a verzi.
5. Vyberte hello **tento balíček obsahuje zdrojové soubory** zaškrtávací políčko.
6. Klikněte na tlačítko **Procházet**a vyberte hello síťové sdílené položce, se uloží instalační program hello (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package.png)

7. Na hello **typ programu hello zvolte, které chcete toocreate** vyberte **standardní Program**a klikněte na tlačítko **Další**.

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. Na hello **zadejte informace o tomto standardním programu** zadejte hello následující vstupy a klikněte na tlačítko **Další**. (hello ostatní vstupy můžete použít výchozí hodnoty.)

  | **Název parametru** | **Hodnota** |
  |--|--|
  | Name (Název) | Instalaci služby Mobility Microsoft Azure (Windows) |
  | Příkazový řádek | Install.bat |
  | Program lze spustit | Zda je přihlášený uživatel |

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties.png)

9. Na další stránku hello vyberte hello cílový operační systémy. Služba mobility lze nainstalovat pouze na Windows Server 2012 R2, Windows Server 2012 a Windows Server 2008 R2.

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2.png)

10. toocomplete hello průvodce, klikněte na tlačítko **Další** dvakrát.


> [!NOTE]
> skript Hello podporuje i nové instalace služby Mobility agentů a aktualizuje tooagents, které jsou již nainstalovány.

### <a name="step-3-deploy-hello-package"></a>Krok 3: Nasazení balíčku hello
1. V konzole Configuration Manager hello, klikněte pravým tlačítkem na váš balíček a vyberte **distribuovat obsah**.
  ![Snímek obrazovky nástroje Configuration Manager konzoly](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)
2. Vyberte hello  **[distribuční body](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  na toowhich by se měl zkopírovat hello balíčky.
3. Dokončení hello průvodce. Hello balíček pak spustí replikaci toohello zadat distribuční body.
4. Po dokončení hello distribuci balíčku, klikněte pravým tlačítkem na balíček hello a vyberte **nasadit**.
  ![Snímek obrazovky nástroje Configuration Manager konzoly](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)
5. Vyberte kolekci zařízení systému Windows Server hello, kterou jste vytvořili v části předpoklady hello jako hello cílovou kolekci pro nasazení.

  ![Průvodce snímek nasazení softwaru](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection.png)

6. Na hello **zadejte cílové umístění obsahu hello** vyberte vaše **distribuční body**.
7. Na hello **toocontrol nastavení zadejte, jak tento software nasazen** stránka, zkontrolujte, zda text hello účel **požadované**.

  ![Průvodce snímek nasazení softwaru](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. Na hello **zadejte hello plán pro toto nasazení** určete plán. Další informace najdete v tématu [plánování balíčky](https://technet.microsoft.com/library/gg682178.aspx).
9. Na hello **distribuční body** nakonfigurujte vlastnosti hello podle potřeby toohello vašeho datového centra. Dokončete Průvodce hello.

> [!TIP]
> tooavoid nepotřebné dojde k restartování, instalace balíčku hello plán během měsíčního časového období údržby nebo okno aktualizace softwaru.

Hello nasazení průběh můžete sledovat pomocí konzoly nástroje Configuration Manager hello. Přejděte příliš**monitorování** > **nasazení** > *[váš název balíčku]*.

  ![Snímek obrazovky nástroje Configuration Manager možnost toomonitor nasazení](./media/site-recovery-install-mobility-service-using-sccm/report.PNG)

## <a name="deploy-mobility-service-on-computers-running-linux"></a>Nasazení služby Mobility na počítačích se systémem Linux
> [!NOTE]
> Tento článek předpokládá, že IP adresa hello hello konfigurační server je 192.168.3.121 a že hello zabezpečené sdílení souborů je \\\ContosoSecureFS\MobilityServiceInstallers.

### <a name="step-1-prepare-for-deployment"></a>Krok 1: Příprava na nasazení
1. Vytvořte složku ve sdílené síťové složce hello a pojmenujte ji jako **MobSvcLinux**.
2. Přihlaste se tooyour konfigurační server a otevřete příkazový řádek pro správu.
3. Spusťte následující příkazy toogenerate soubor přístupové heslo hello:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Kopírování hello **MobSvc.passphrase** souboru do hello **MobSvcLinux** složky do sdílené síťové složky.
5. Spuštěním příkazu hello procházet toohello Instalační služby úložiště na hello konfigurační server:

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. Kopírování hello následující soubory toohello **MobSvcLinux** složky do sdílené síťové složky:
   * Microsoft automatické obnovení systému\_uživatelský Agent\*RHEL6 64*release.tar.gz
   * Microsoft automatické obnovení systému\_uživatelský Agent\*RHEL7-64\*release.tar.gz
   * Microsoft automatické obnovení systému\_uživatelský Agent\*SLES11-SP3-64\*release.tar.gz
   * Microsoft automatické obnovení systému\_uživatelský Agent\*SLES11-SP4-64\*release.tar.gz
   * Microsoft automatické obnovení systému\_uživatelský Agent\*OL6-64\*release.tar.gz
   * Microsoft automatické obnovení systému\_uživatelský Agent\*UBUNTU 14.04-64\*release.tar.gz


7. Zkopírujte následující kód hello a uložte ho jako **install_linux.sh** do hello **MobSvcLinux** složky.
   > [!NOTE]
   > Nahraďte zástupné symboly hello [CSIP] ve skriptu hello skutečnými hodnotami hello IP adresu konfigurační server.

```Bash
#!/usr/bin/env bash

rm -rf /tmp/MobSvc
mkdir -p /tmp/MobSvc
INSTALL_DIR='/usr/local/ASR'
VX_VERSION_FILE='/usr/local/.vx_version'

echo "=============================" >> /tmp/MobSvc/sccm.log
echo `date` >> /tmp/MobSvc/sccm.log
echo "=============================" >> /tmp/MobSvc/sccm.log

if [ -f /etc/oracle-release ] && [ -f /etc/redhat-release ]; then
    if grep -q 'Oracle Linux Server release 6.*' /etc/oracle-release; then
        if uname -a | grep -q x86_64; then
            OS="OL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *OL6*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/redhat-release ]; then
    if grep -q 'Red Hat Enterprise Linux Server release 6.* (Santiago)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 6.* (Final)' /etc/redhat-release || \
        grep -q 'CentOS release 6.* (Final)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL6*.tar.gz /tmp/MobSvc
        fi
    elif grep -q 'Red Hat Enterprise Linux Server release 7.* (Maipo)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 7.* (Core)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL7-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL7*.tar.gz /tmp/MobSvc
                fi
    fi
elif [ -f /etc/SuSE-release ] && grep -q 'VERSION = 11' /etc/SuSE-release; then
    if grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 3' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP3-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP3*.tar.gz /tmp/MobSvc
        fi
    elif grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 4' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP4-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP4*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/lsb-release ] ; then
    if grep -q 'DISTRIB_RELEASE=14.04' /etc/lsb-release ; then
       if uname -a | grep -q x86_64; then
           OS="UBUNTU-14.04-64"
           echo $OS >> /tmp/MobSvc/sccm.log
           cp *UBUNTU-14*.tar.gz /tmp/MobSvc
       fi
    fi
else
    exit 1
fi

if [ -z "$OS" ]; then
    exit 1
fi

Install()
{
    echo "Perform Installation." >> /tmp/MobSvc/sccm.log
    ./install -q -d ${INSTALL_DIR} -r MS -v VmWare
    RET_VAL=$?
    echo "Installation Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Installation has succeeded. Proceed tooconfiguration." >> /tmp/MobSvc/sccm.log
        Configure
    else
        echo "Installation has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Configure()
{
    echo "Perform configuration." >> /tmp/MobSvc/sccm.log
    ${INSTALL_DIR}/Vx/bin/UnifiedAgentConfigurator.sh -i [CSIP] -P MobSvc.passphrase
    RET_VAL=$?
    echo "Configuration Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Configuration has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Configuration has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Upgrade()
{
    echo "Perform Upgrade." >> /tmp/MobSvc/sccm.log
    ./install -q -v VmWare
    RET_VAL=$?
    echo "Upgrade Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Upgrade has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Upgrade has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

cp MobSvc.passphrase /tmp/MobSvc
cd /tmp/MobSvc

tar -zxvf *.tar.gz

if [ -e ${VX_VERSION_FILE} ]; then
    echo "${VX_VERSION_FILE} exists. Checking for configuration status." >> /tmp/MobSvc/sccm.log
    agent_configuration=$(grep ^AGENT_CONFIGURATION_STATUS "${VX_VERSION_FILE}" | cut -d"=" -f2 | tr -d " ")
    echo "agent_configuration=$agent_configuration" >> /tmp/MobSvc/sccm.log
     if [ "$agent_configuration" == "Succeeded" ]; then
        echo "Agent is already configured. Proceed tooUpgrade." >> /tmp/MobSvc/sccm.log
        Upgrade
    else
        echo "Agent is not configured. Proceed tooConfigure." >> /tmp/MobSvc/sccm.log
        Configure
    fi
else
    Install
fi

cd /tmp

```

### <a name="step-2-create-a-package"></a>Krok 2: Vytvoření balíčku

1. Přihlaste se tooyour Konzola nástroje Configuration Manager.
2. Procházet příliš**softwarová knihovna** > **Správa aplikací** > **balíčky**.
3. Klikněte pravým tlačítkem na **balíčky**a vyberte **vytvořit balíček**.
4. Zadejte hodnoty pro hello název, popis, výrobce, jazyk a verzi.
5. Vyberte hello **tento balíček obsahuje zdrojové soubory** zaškrtávací políčko.
6. Klikněte na tlačítko **Procházet**a vyberte hello síťové sdílené položce, se uloží instalační program hello (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package-linux.png)

7. Na hello **typ programu hello zvolte, které chcete toocreate** vyberte **standardní Program**a klikněte na tlačítko **Další**.

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. Na hello **zadejte informace o tomto standardním programu** zadejte hello následující vstupy a klikněte na tlačítko **Další**. (hello ostatní vstupy můžete použít výchozí hodnoty.)

    | **Název parametru** | **Hodnota** |
  |--|--|
  | Name (Název) | Instalaci služby Mobility Microsoft Azure (Linux) |
  | Příkazový řádek | ./install_linux.SH |
  | Program lze spustit | Zda je přihlášený uživatel |

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-linux.png)

9. Na další stránku hello, vyberte **tento program lze spustit na jakékoli platformě**.
  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2-linux.png)

10. toocomplete hello průvodce, klikněte na tlačítko **Další** dvakrát.

> [!NOTE]
> skript Hello podporuje i nové instalace služby Mobility agentů a aktualizuje tooagents, které jsou již nainstalovány.

### <a name="step-3-deploy-hello-package"></a>Krok 3: Nasazení balíčku hello
1. V konzole Configuration Manager hello, klikněte pravým tlačítkem na váš balíček a vyberte **distribuovat obsah**.
  ![Snímek obrazovky nástroje Configuration Manager konzoly](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)
2. Vyberte hello  **[distribuční body](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  na toowhich by se měl zkopírovat hello balíčky.
3. Dokončení hello průvodce. Hello balíček pak spustí replikaci toohello zadat distribuční body.
4. Po dokončení hello distribuci balíčku, klikněte pravým tlačítkem na balíček hello a vyberte **nasadit**.
  ![Snímek obrazovky nástroje Configuration Manager konzoly](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)
5. Vyberte hello Linux Server kolekce zařízení, kterou jste vytvořili v části předpoklady hello jako hello cílovou kolekci pro nasazení.

  ![Průvodce snímek nasazení softwaru](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection-linux.png)

6. Na hello **zadejte cílové umístění obsahu hello** vyberte vaše **distribuční body**.
7. Na hello **toocontrol nastavení zadejte, jak tento software nasazen** stránka, zkontrolujte, zda text hello účel **požadované**.

  ![Průvodce snímek nasazení softwaru](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. Na hello **zadejte hello plán pro toto nasazení** určete plán. Další informace najdete v tématu [plánování balíčky](https://technet.microsoft.com/library/gg682178.aspx).
9. Na hello **distribuční body** nakonfigurujte vlastnosti hello podle potřeby toohello vašeho datového centra. Dokončete Průvodce hello.

Získá nainstalovaná služba mobility hello kolekce zařízení Server Linux, podle plánu toohello, které jste nakonfigurovali.

## <a name="other-methods-tooinstall-mobility-service"></a>Ostatní metody tooinstall služba Mobility
Zde jsou některé další možnosti pro instalaci služby Mobility:
* [Ruční instalace pomocí grafického uživatelského rozhraní](http://aka.ms/mobsvcmanualinstall)
* [Ruční instalace pomocí příkazového řádku](http://aka.ms/mobsvcmanualinstallcli)
* [Nabízená instalace pomocí konfigurace serveru](http://aka.ms/pushinstall)
* [Automatická instalace pomocí Azure Automation a konfigurace požadovaného stavu](http://aka.ms/mobsvcdscinstall)

## <a name="uninstall-mobility-service"></a>Odinstalujte službu Mobility
Můžete vytvořit toouninstall balíčků nástroje Configuration Manager služby Mobility. Použijte následující skript toodo tak hello:

```
Time /t >> C:\logfile.log
REM ==================================================
REM ==== Check if Mob Svc is already installed =======
REM ==== If not installed no operation required ========
REM ==== Else run uninstall command =====================
REM ==== {275197FC-14FD-4560-A5EB-38217F80CBD1} is ====
REM ==== guid for Mob Svc Installer ====================
whoami >> C:\logfile.log
NET START | FIND "InMage Scout Application Service"
IF  %ERRORLEVEL% EQU 1 (GOTO :INSTALL) ELSE GOTO :UNINSTALL
:NOOPERATION
                echo "No Operation Required." >> c:\logfile.log
                GOTO :ENDSCRIPT
:UNINSTALL
                echo "Uninstall" >> C:\logfile.log
                MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
:ENDSCRIPT

```

## <a name="next-steps"></a>Další kroky
Nyní jste připraveni příliš[povolení ochrany](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmware-to-azure#step-6-replicate-applications) pro virtuální počítače.
