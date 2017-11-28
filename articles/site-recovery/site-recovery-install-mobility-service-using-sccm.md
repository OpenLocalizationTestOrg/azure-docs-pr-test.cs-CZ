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
# <a name="automate-mobility-service-installation-by-using-software-deployment-tools"></a><span data-ttu-id="31836-103">Instalace služby Mobility automatizovat pomocí nástroje pro nasazení softwaru</span><span class="sxs-lookup"><span data-stu-id="31836-103">Automate Mobility Service installation by using software deployment tools</span></span>

>[!IMPORTANT]
<span data-ttu-id="31836-104">Tento dokument předpokládá, že používáte verzi **9.9.4510.1** nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="31836-104">This document assumes you are using version **9.9.4510.1** or higher.</span></span>

<span data-ttu-id="31836-105">Tento článek obsahuje příklady použití System Center Configuration Manager toodeploy hello službě Azure Site Recovery Mobility ve vašem datovém centru.</span><span class="sxs-lookup"><span data-stu-id="31836-105">This article provides you an example of how you can use System Center Configuration Manager toodeploy hello Azure Site Recovery Mobility Service in your datacenter.</span></span> <span data-ttu-id="31836-106">Pomocí nástroje pro nasazení softwaru jako nástroje Configuration Manager má hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="31836-106">Using a software deployment tool like Configuration Manager has hello following advantages:</span></span>
* <span data-ttu-id="31836-107">Plánování nasazení nové instalace a upgradu, během plánované údržby pro aktualizace softwaru</span><span class="sxs-lookup"><span data-stu-id="31836-107">Scheduling deployment of fresh installations and upgrades, during your planned maintenance window for software updates</span></span>
* <span data-ttu-id="31836-108">Škálování toohundreds nasazení serverů současně</span><span class="sxs-lookup"><span data-stu-id="31836-108">Scaling deployment toohundreds of servers simultaneously</span></span>


> [!NOTE]
> <span data-ttu-id="31836-109">Tento článek používá aktivita nasazení hello toodemonstrate System Center Configuration Manager 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="31836-109">This article uses System Center Configuration Manager 2012 R2 toodemonstrate hello deployment activity.</span></span> <span data-ttu-id="31836-110">Instalace služby Mobility může také automatizovat pomocí [Azure Automation a konfigurace požadovaného stavu](site-recovery-automate-mobility-service-install.md).</span><span class="sxs-lookup"><span data-stu-id="31836-110">You could also automate Mobility Service installation by using [Azure Automation and Desired State Configuration](site-recovery-automate-mobility-service-install.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="31836-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="31836-111">Prerequisites</span></span>
1. <span data-ttu-id="31836-112">Nástroj nasazení softwaru, jako jsou nástroje Configuration Manager, který je už nasazená ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="31836-112">A software deployment tool, like Configuration Manager, that is already deployed in your environment.</span></span>
  <span data-ttu-id="31836-113">Vytvořte dvě [kolekce zařízení](https://technet.microsoft.com/library/gg682169.aspx), jeden pro všechny **servery Windows**a druhou pro všechny **servery se systémem Linux**, že chcete tooprotect pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="31836-113">Create two [device collections](https://technet.microsoft.com/library/gg682169.aspx), one for all **Windows servers**, and another for all **Linux servers**, that you want tooprotect by using Site Recovery.</span></span>
3. <span data-ttu-id="31836-114">Konfigurační server, který je již zaregistrována k Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="31836-114">A configuration server that is already registered with Site Recovery.</span></span>
4. <span data-ttu-id="31836-115">Zabezpečené síťové sdílené složky (Server Message Block sdílené složky), který je přístupný pomocí serveru nástroje Configuration Manager hello.</span><span class="sxs-lookup"><span data-stu-id="31836-115">A secure network file share (Server Message Block share) that can be accessed by hello Configuration Manager server.</span></span>

## <a name="deploy-mobility-service-on-computers-running-windows"></a><span data-ttu-id="31836-116">Nasazení služby Mobility na počítačích se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="31836-116">Deploy Mobility Service on computers running Windows</span></span>
> [!NOTE]
> <span data-ttu-id="31836-117">Tento článek předpokládá, že IP adresa hello hello konfigurační server je 192.168.3.121 a že hello zabezpečené sdílení souborů je \\\ContosoSecureFS\MobilityServiceInstallers.</span><span class="sxs-lookup"><span data-stu-id="31836-117">This article assumes that hello IP address of hello configuration server is 192.168.3.121, and that hello secure network file share is \\\ContosoSecureFS\MobilityServiceInstallers.</span></span>

### <a name="step-1-prepare-for-deployment"></a><span data-ttu-id="31836-118">Krok 1: Příprava na nasazení</span><span class="sxs-lookup"><span data-stu-id="31836-118">Step 1: Prepare for deployment</span></span>
1. <span data-ttu-id="31836-119">Vytvořte složku ve sdílené síťové složce hello a pojmenujte ji **MobSvcWindows**.</span><span class="sxs-lookup"><span data-stu-id="31836-119">Create a folder on hello network share, and name it **MobSvcWindows**.</span></span>
2. <span data-ttu-id="31836-120">Přihlaste se tooyour konfigurační server a otevřete příkazový řádek pro správu.</span><span class="sxs-lookup"><span data-stu-id="31836-120">Sign in tooyour configuration server, and open an administrative command prompt.</span></span>
3. <span data-ttu-id="31836-121">Spusťte následující příkazy toogenerate soubor přístupové heslo hello:</span><span class="sxs-lookup"><span data-stu-id="31836-121">Run hello following commands toogenerate a passphrase file:</span></span>

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. <span data-ttu-id="31836-122">Kopírování hello **MobSvc.passphrase** souboru do hello **MobSvcWindows** složky do sdílené síťové složky.</span><span class="sxs-lookup"><span data-stu-id="31836-122">Copy hello **MobSvc.passphrase** file into hello **MobSvcWindows** folder on your network share.</span></span>
5. <span data-ttu-id="31836-123">Procházet toohello Instalační služby úložiště na konfiguračním serveru hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="31836-123">Browse toohello installer repository on hello configuration server by running hello following command:</span></span>

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. <span data-ttu-id="31836-124">Kopírování hello  **Microsoft automatické obnovení systému\_uživatelský Agent\_*verze*\_Windows\_GA\_*datum* \_ Release.exe** toohello **MobSvcWindows** složky do sdílené síťové složky.</span><span class="sxs-lookup"><span data-stu-id="31836-124">Copy hello **Microsoft-ASR\_UA\_*version*\_Windows\_GA\_*date*\_Release.exe** toohello **MobSvcWindows** folder on your network share.</span></span>
7. <span data-ttu-id="31836-125">Zkopírujte následující kód hello a uložte ho jako **install.bat** do hello **MobSvcWindows** složky.</span><span class="sxs-lookup"><span data-stu-id="31836-125">Copy hello following code, and save it as **install.bat** into hello **MobSvcWindows** folder.</span></span>

   > [!NOTE]
   > <span data-ttu-id="31836-126">Nahraďte zástupné symboly hello [CSIP] ve skriptu hello skutečnými hodnotami hello IP adresu konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="31836-126">Replace hello [CSIP] placeholders in this script with hello actual values of hello IP address of your configuration server.</span></span>

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

### <a name="step-2-create-a-package"></a><span data-ttu-id="31836-127">Krok 2: Vytvoření balíčku</span><span class="sxs-lookup"><span data-stu-id="31836-127">Step 2: Create a package</span></span>

1. <span data-ttu-id="31836-128">Přihlaste se tooyour Konzola nástroje Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="31836-128">Sign in tooyour Configuration Manager console.</span></span>
2. <span data-ttu-id="31836-129">Procházet příliš**softwarová knihovna** > **Správa aplikací** > **balíčky**.</span><span class="sxs-lookup"><span data-stu-id="31836-129">Browse too**Software Library** > **Application Management** > **Packages**.</span></span>
3. <span data-ttu-id="31836-130">Klikněte pravým tlačítkem na **balíčky**a vyberte **vytvořit balíček**.</span><span class="sxs-lookup"><span data-stu-id="31836-130">Right-click **Packages**, and select **Create Package**.</span></span>
4. <span data-ttu-id="31836-131">Zadejte hodnoty pro hello název, popis, výrobce, jazyk a verzi.</span><span class="sxs-lookup"><span data-stu-id="31836-131">Provide values for hello name, description, manufacturer, language, and version.</span></span>
5. <span data-ttu-id="31836-132">Vyberte hello **tento balíček obsahuje zdrojové soubory** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="31836-132">Select hello **This package contains source files** check box.</span></span>
6. <span data-ttu-id="31836-133">Klikněte na tlačítko **Procházet**a vyberte hello síťové sdílené položce, se uloží instalační program hello (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).</span><span class="sxs-lookup"><span data-stu-id="31836-133">Click **Browse**, and select hello network share where hello installer is stored (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).</span></span>

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package.png)

7. <span data-ttu-id="31836-135">Na hello **typ programu hello zvolte, které chcete toocreate** vyberte **standardní Program**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="31836-135">On hello **Choose hello program type that you want toocreate** page, select **Standard Program**, and click **Next**.</span></span>

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. <span data-ttu-id="31836-137">Na hello **zadejte informace o tomto standardním programu** zadejte hello následující vstupy a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="31836-137">On hello **Specify information about this standard program** page, provide hello following inputs, and click **Next**.</span></span> <span data-ttu-id="31836-138">(hello ostatní vstupy můžete použít výchozí hodnoty.)</span><span class="sxs-lookup"><span data-stu-id="31836-138">(hello other inputs can use their default values.)</span></span>

  | <span data-ttu-id="31836-139">**Název parametru**</span><span class="sxs-lookup"><span data-stu-id="31836-139">**Parameter name**</span></span> | <span data-ttu-id="31836-140">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="31836-140">**Value**</span></span> |
  |--|--|
  | <span data-ttu-id="31836-141">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="31836-141">Name</span></span> | <span data-ttu-id="31836-142">Instalaci služby Mobility Microsoft Azure (Windows)</span><span class="sxs-lookup"><span data-stu-id="31836-142">Install Microsoft Azure Mobility Service (Windows)</span></span> |
  | <span data-ttu-id="31836-143">Příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="31836-143">Command line</span></span> | <span data-ttu-id="31836-144">Install.bat</span><span class="sxs-lookup"><span data-stu-id="31836-144">install.bat</span></span> |
  | <span data-ttu-id="31836-145">Program lze spustit</span><span class="sxs-lookup"><span data-stu-id="31836-145">Program can run</span></span> | <span data-ttu-id="31836-146">Zda je přihlášený uživatel</span><span class="sxs-lookup"><span data-stu-id="31836-146">Whether or not a user is logged on</span></span> |

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties.png)

9. <span data-ttu-id="31836-148">Na další stránku hello vyberte hello cílový operační systémy.</span><span class="sxs-lookup"><span data-stu-id="31836-148">On hello next page, select hello target operating systems.</span></span> <span data-ttu-id="31836-149">Služba mobility lze nainstalovat pouze na Windows Server 2012 R2, Windows Server 2012 a Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="31836-149">Mobility Service can be installed only on Windows Server 2012 R2, Windows Server 2012, and Windows Server 2008 R2.</span></span>

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2.png)

10. <span data-ttu-id="31836-151">toocomplete hello průvodce, klikněte na tlačítko **Další** dvakrát.</span><span class="sxs-lookup"><span data-stu-id="31836-151">toocomplete hello wizard, click **Next** twice.</span></span>


> [!NOTE]
> <span data-ttu-id="31836-152">skript Hello podporuje i nové instalace služby Mobility agentů a aktualizuje tooagents, které jsou již nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="31836-152">hello script supports both new installations of Mobility Service agents and updates tooagents that are already installed.</span></span>

### <a name="step-3-deploy-hello-package"></a><span data-ttu-id="31836-153">Krok 3: Nasazení balíčku hello</span><span class="sxs-lookup"><span data-stu-id="31836-153">Step 3: Deploy hello package</span></span>
1. <span data-ttu-id="31836-154">V konzole Configuration Manager hello, klikněte pravým tlačítkem na váš balíček a vyberte **distribuovat obsah**.</span><span class="sxs-lookup"><span data-stu-id="31836-154">In hello Configuration Manager console, right-click your package, and select **Distribute Content**.</span></span>
  <span data-ttu-id="31836-155">![Snímek obrazovky nástroje Configuration Manager konzoly](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span><span class="sxs-lookup"><span data-stu-id="31836-155">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span></span>
2. <span data-ttu-id="31836-156">Vyberte hello  **[distribuční body](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  na toowhich by se měl zkopírovat hello balíčky.</span><span class="sxs-lookup"><span data-stu-id="31836-156">Select hello **[distribution points](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)** on toowhich hello packages should be copied.</span></span>
3. <span data-ttu-id="31836-157">Dokončení hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="31836-157">Complete hello wizard.</span></span> <span data-ttu-id="31836-158">Hello balíček pak spustí replikaci toohello zadat distribuční body.</span><span class="sxs-lookup"><span data-stu-id="31836-158">hello package then starts replicating toohello specified distribution points.</span></span>
4. <span data-ttu-id="31836-159">Po dokončení hello distribuci balíčku, klikněte pravým tlačítkem na balíček hello a vyberte **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="31836-159">After hello package distribution is done, right-click hello package, and select **Deploy**.</span></span>
  <span data-ttu-id="31836-160">![Snímek obrazovky nástroje Configuration Manager konzoly](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span><span class="sxs-lookup"><span data-stu-id="31836-160">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span></span>
5. <span data-ttu-id="31836-161">Vyberte kolekci zařízení systému Windows Server hello, kterou jste vytvořili v části předpoklady hello jako hello cílovou kolekci pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="31836-161">Select hello Windows Server device collection you created in hello prerequisites section as hello target collection for deployment.</span></span>

  ![Průvodce snímek nasazení softwaru](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection.png)

6. <span data-ttu-id="31836-163">Na hello **zadejte cílové umístění obsahu hello** vyberte vaše **distribuční body**.</span><span class="sxs-lookup"><span data-stu-id="31836-163">On hello **Specify hello content destination** page, select your **Distribution Points**.</span></span>
7. <span data-ttu-id="31836-164">Na hello **toocontrol nastavení zadejte, jak tento software nasazen** stránka, zkontrolujte, zda text hello účel **požadované**.</span><span class="sxs-lookup"><span data-stu-id="31836-164">On hello **Specify settings toocontrol how this software is deployed** page, ensure that hello purpose is **Required**.</span></span>

  ![Průvodce snímek nasazení softwaru](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. <span data-ttu-id="31836-166">Na hello **zadejte hello plán pro toto nasazení** určete plán.</span><span class="sxs-lookup"><span data-stu-id="31836-166">On hello **Specify hello schedule for this deployment** page, specify a schedule.</span></span> <span data-ttu-id="31836-167">Další informace najdete v tématu [plánování balíčky](https://technet.microsoft.com/library/gg682178.aspx).</span><span class="sxs-lookup"><span data-stu-id="31836-167">For more information, see [scheduling packages](https://technet.microsoft.com/library/gg682178.aspx).</span></span>
9. <span data-ttu-id="31836-168">Na hello **distribuční body** nakonfigurujte vlastnosti hello podle potřeby toohello vašeho datového centra.</span><span class="sxs-lookup"><span data-stu-id="31836-168">On hello **Distribution Points** page, configure hello properties according toohello needs of your datacenter.</span></span> <span data-ttu-id="31836-169">Dokončete Průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="31836-169">Then complete hello wizard.</span></span>

> [!TIP]
> <span data-ttu-id="31836-170">tooavoid nepotřebné dojde k restartování, instalace balíčku hello plán během měsíčního časového období údržby nebo okno aktualizace softwaru.</span><span class="sxs-lookup"><span data-stu-id="31836-170">tooavoid unnecessary reboots, schedule hello package installation during your monthly maintenance window or software updates window.</span></span>

<span data-ttu-id="31836-171">Hello nasazení průběh můžete sledovat pomocí konzoly nástroje Configuration Manager hello.</span><span class="sxs-lookup"><span data-stu-id="31836-171">You can monitor hello deployment progress by using hello Configuration Manager console.</span></span> <span data-ttu-id="31836-172">Přejděte příliš**monitorování** > **nasazení** > *[váš název balíčku]*.</span><span class="sxs-lookup"><span data-stu-id="31836-172">Go too**Monitoring** > **Deployments** > *[your package name]*.</span></span>

  ![Snímek obrazovky nástroje Configuration Manager možnost toomonitor nasazení](./media/site-recovery-install-mobility-service-using-sccm/report.PNG)

## <a name="deploy-mobility-service-on-computers-running-linux"></a><span data-ttu-id="31836-174">Nasazení služby Mobility na počítačích se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="31836-174">Deploy Mobility Service on computers running Linux</span></span>
> [!NOTE]
> <span data-ttu-id="31836-175">Tento článek předpokládá, že IP adresa hello hello konfigurační server je 192.168.3.121 a že hello zabezpečené sdílení souborů je \\\ContosoSecureFS\MobilityServiceInstallers.</span><span class="sxs-lookup"><span data-stu-id="31836-175">This article assumes that hello IP address of hello configuration server is 192.168.3.121, and that hello secure network file share is \\\ContosoSecureFS\MobilityServiceInstallers.</span></span>

### <a name="step-1-prepare-for-deployment"></a><span data-ttu-id="31836-176">Krok 1: Příprava na nasazení</span><span class="sxs-lookup"><span data-stu-id="31836-176">Step 1: Prepare for deployment</span></span>
1. <span data-ttu-id="31836-177">Vytvořte složku ve sdílené síťové složce hello a pojmenujte ji jako **MobSvcLinux**.</span><span class="sxs-lookup"><span data-stu-id="31836-177">Create a folder on hello network share, and name it as **MobSvcLinux**.</span></span>
2. <span data-ttu-id="31836-178">Přihlaste se tooyour konfigurační server a otevřete příkazový řádek pro správu.</span><span class="sxs-lookup"><span data-stu-id="31836-178">Sign in tooyour configuration server, and open an administrative command prompt.</span></span>
3. <span data-ttu-id="31836-179">Spusťte následující příkazy toogenerate soubor přístupové heslo hello:</span><span class="sxs-lookup"><span data-stu-id="31836-179">Run hello following commands toogenerate a passphrase file:</span></span>

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. <span data-ttu-id="31836-180">Kopírování hello **MobSvc.passphrase** souboru do hello **MobSvcLinux** složky do sdílené síťové složky.</span><span class="sxs-lookup"><span data-stu-id="31836-180">Copy hello **MobSvc.passphrase** file into hello **MobSvcLinux** folder on your network share.</span></span>
5. <span data-ttu-id="31836-181">Spuštěním příkazu hello procházet toohello Instalační služby úložiště na hello konfigurační server:</span><span class="sxs-lookup"><span data-stu-id="31836-181">Browse toohello installer repository on hello configuration server by running hello command:</span></span>

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. <span data-ttu-id="31836-182">Kopírování hello následující soubory toohello **MobSvcLinux** složky do sdílené síťové složky:</span><span class="sxs-lookup"><span data-stu-id="31836-182">Copy hello following files toohello **MobSvcLinux** folder on your network share:</span></span>
   * <span data-ttu-id="31836-183">Microsoft automatické obnovení systému\_uživatelský Agent\*RHEL6 64*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="31836-183">Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz</span></span>
   * <span data-ttu-id="31836-184">Microsoft automatické obnovení systému\_uživatelský Agent\*RHEL7-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="31836-184">Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz</span></span>
   * <span data-ttu-id="31836-185">Microsoft automatické obnovení systému\_uživatelský Agent\*SLES11-SP3-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="31836-185">Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz</span></span>
   * <span data-ttu-id="31836-186">Microsoft automatické obnovení systému\_uživatelský Agent\*SLES11-SP4-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="31836-186">Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz</span></span>
   * <span data-ttu-id="31836-187">Microsoft automatické obnovení systému\_uživatelský Agent\*OL6-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="31836-187">Microsoft-ASR\_UA\*OL6-64\*release.tar.gz</span></span>
   * <span data-ttu-id="31836-188">Microsoft automatické obnovení systému\_uživatelský Agent\*UBUNTU 14.04-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="31836-188">Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz</span></span>


7. <span data-ttu-id="31836-189">Zkopírujte následující kód hello a uložte ho jako **install_linux.sh** do hello **MobSvcLinux** složky.</span><span class="sxs-lookup"><span data-stu-id="31836-189">Copy hello following code, and save it as **install_linux.sh** into hello **MobSvcLinux** folder.</span></span>
   > [!NOTE]
   > <span data-ttu-id="31836-190">Nahraďte zástupné symboly hello [CSIP] ve skriptu hello skutečnými hodnotami hello IP adresu konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="31836-190">Replace hello [CSIP] placeholders in this script with hello actual values of hello IP address of your configuration server.</span></span>

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

### <a name="step-2-create-a-package"></a><span data-ttu-id="31836-191">Krok 2: Vytvoření balíčku</span><span class="sxs-lookup"><span data-stu-id="31836-191">Step 2: Create a package</span></span>

1. <span data-ttu-id="31836-192">Přihlaste se tooyour Konzola nástroje Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="31836-192">Sign in  tooyour Configuration Manager console.</span></span>
2. <span data-ttu-id="31836-193">Procházet příliš**softwarová knihovna** > **Správa aplikací** > **balíčky**.</span><span class="sxs-lookup"><span data-stu-id="31836-193">Browse too**Software Library** > **Application Management** > **Packages**.</span></span>
3. <span data-ttu-id="31836-194">Klikněte pravým tlačítkem na **balíčky**a vyberte **vytvořit balíček**.</span><span class="sxs-lookup"><span data-stu-id="31836-194">Right-click **Packages**, and select **Create Package**.</span></span>
4. <span data-ttu-id="31836-195">Zadejte hodnoty pro hello název, popis, výrobce, jazyk a verzi.</span><span class="sxs-lookup"><span data-stu-id="31836-195">Provide values for hello name, description, manufacturer, language, and version.</span></span>
5. <span data-ttu-id="31836-196">Vyberte hello **tento balíček obsahuje zdrojové soubory** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="31836-196">Select hello **This package contains source files** check box.</span></span>
6. <span data-ttu-id="31836-197">Klikněte na tlačítko **Procházet**a vyberte hello síťové sdílené položce, se uloží instalační program hello (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).</span><span class="sxs-lookup"><span data-stu-id="31836-197">Click **Browse**, and select hello network share where hello installer is stored (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).</span></span>

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package-linux.png)

7. <span data-ttu-id="31836-199">Na hello **typ programu hello zvolte, které chcete toocreate** vyberte **standardní Program**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="31836-199">On hello **Choose hello program type that you want toocreate** page, select **Standard Program**, and click **Next**.</span></span>

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. <span data-ttu-id="31836-201">Na hello **zadejte informace o tomto standardním programu** zadejte hello následující vstupy a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="31836-201">On hello **Specify information about this standard program** page, provide hello following inputs, and click **Next**.</span></span> <span data-ttu-id="31836-202">(hello ostatní vstupy můžete použít výchozí hodnoty.)</span><span class="sxs-lookup"><span data-stu-id="31836-202">(hello other inputs can use their default values.)</span></span>

    | <span data-ttu-id="31836-203">**Název parametru**</span><span class="sxs-lookup"><span data-stu-id="31836-203">**Parameter name**</span></span> | <span data-ttu-id="31836-204">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="31836-204">**Value**</span></span> |
  |--|--|
  | <span data-ttu-id="31836-205">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="31836-205">Name</span></span> | <span data-ttu-id="31836-206">Instalaci služby Mobility Microsoft Azure (Linux)</span><span class="sxs-lookup"><span data-stu-id="31836-206">Install Microsoft Azure Mobility Service (Linux)</span></span> |
  | <span data-ttu-id="31836-207">Příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="31836-207">Command line</span></span> | <span data-ttu-id="31836-208">./install_linux.SH</span><span class="sxs-lookup"><span data-stu-id="31836-208">./install_linux.sh</span></span> |
  | <span data-ttu-id="31836-209">Program lze spustit</span><span class="sxs-lookup"><span data-stu-id="31836-209">Program can run</span></span> | <span data-ttu-id="31836-210">Zda je přihlášený uživatel</span><span class="sxs-lookup"><span data-stu-id="31836-210">Whether or not a user is logged on</span></span> |

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-linux.png)

9. <span data-ttu-id="31836-212">Na další stránku hello, vyberte **tento program lze spustit na jakékoli platformě**.</span><span class="sxs-lookup"><span data-stu-id="31836-212">On hello next page, select **This program can run on any platform**.</span></span>
  <span data-ttu-id="31836-213">![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2-linux.png)</span><span class="sxs-lookup"><span data-stu-id="31836-213">![Screenshot of Create Package and Program wizard](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2-linux.png)</span></span>

10. <span data-ttu-id="31836-214">toocomplete hello průvodce, klikněte na tlačítko **Další** dvakrát.</span><span class="sxs-lookup"><span data-stu-id="31836-214">toocomplete hello wizard, click **Next** twice.</span></span>

> [!NOTE]
> <span data-ttu-id="31836-215">skript Hello podporuje i nové instalace služby Mobility agentů a aktualizuje tooagents, které jsou již nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="31836-215">hello script supports both new installations of Mobility Service agents and updates tooagents that are already installed.</span></span>

### <a name="step-3-deploy-hello-package"></a><span data-ttu-id="31836-216">Krok 3: Nasazení balíčku hello</span><span class="sxs-lookup"><span data-stu-id="31836-216">Step 3: Deploy hello package</span></span>
1. <span data-ttu-id="31836-217">V konzole Configuration Manager hello, klikněte pravým tlačítkem na váš balíček a vyberte **distribuovat obsah**.</span><span class="sxs-lookup"><span data-stu-id="31836-217">In hello Configuration Manager console, right-click your package, and select **Distribute Content**.</span></span>
  <span data-ttu-id="31836-218">![Snímek obrazovky nástroje Configuration Manager konzoly](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span><span class="sxs-lookup"><span data-stu-id="31836-218">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span></span>
2. <span data-ttu-id="31836-219">Vyberte hello  **[distribuční body](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  na toowhich by se měl zkopírovat hello balíčky.</span><span class="sxs-lookup"><span data-stu-id="31836-219">Select hello **[distribution points](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)** on toowhich hello packages should be copied.</span></span>
3. <span data-ttu-id="31836-220">Dokončení hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="31836-220">Complete hello wizard.</span></span> <span data-ttu-id="31836-221">Hello balíček pak spustí replikaci toohello zadat distribuční body.</span><span class="sxs-lookup"><span data-stu-id="31836-221">hello package then starts replicating toohello specified distribution points.</span></span>
4. <span data-ttu-id="31836-222">Po dokončení hello distribuci balíčku, klikněte pravým tlačítkem na balíček hello a vyberte **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="31836-222">After hello package distribution is done, right-click hello package, and select **Deploy**.</span></span>
  <span data-ttu-id="31836-223">![Snímek obrazovky nástroje Configuration Manager konzoly](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span><span class="sxs-lookup"><span data-stu-id="31836-223">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span></span>
5. <span data-ttu-id="31836-224">Vyberte hello Linux Server kolekce zařízení, kterou jste vytvořili v části předpoklady hello jako hello cílovou kolekci pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="31836-224">Select hello Linux Server device collection you created in hello prerequisites section as hello target collection for deployment.</span></span>

  ![Průvodce snímek nasazení softwaru](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection-linux.png)

6. <span data-ttu-id="31836-226">Na hello **zadejte cílové umístění obsahu hello** vyberte vaše **distribuční body**.</span><span class="sxs-lookup"><span data-stu-id="31836-226">On hello **Specify hello content destination** page, select your **Distribution Points**.</span></span>
7. <span data-ttu-id="31836-227">Na hello **toocontrol nastavení zadejte, jak tento software nasazen** stránka, zkontrolujte, zda text hello účel **požadované**.</span><span class="sxs-lookup"><span data-stu-id="31836-227">On hello **Specify settings toocontrol how this software is deployed** page, ensure that hello purpose is **Required**.</span></span>

  ![Průvodce snímek nasazení softwaru](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. <span data-ttu-id="31836-229">Na hello **zadejte hello plán pro toto nasazení** určete plán.</span><span class="sxs-lookup"><span data-stu-id="31836-229">On hello **Specify hello schedule for this deployment** page, specify a schedule.</span></span> <span data-ttu-id="31836-230">Další informace najdete v tématu [plánování balíčky](https://technet.microsoft.com/library/gg682178.aspx).</span><span class="sxs-lookup"><span data-stu-id="31836-230">For more information, see [scheduling packages](https://technet.microsoft.com/library/gg682178.aspx).</span></span>
9. <span data-ttu-id="31836-231">Na hello **distribuční body** nakonfigurujte vlastnosti hello podle potřeby toohello vašeho datového centra.</span><span class="sxs-lookup"><span data-stu-id="31836-231">On hello **Distribution Points** page, configure hello properties according toohello needs of your datacenter.</span></span> <span data-ttu-id="31836-232">Dokončete Průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="31836-232">Then complete hello wizard.</span></span>

<span data-ttu-id="31836-233">Získá nainstalovaná služba mobility hello kolekce zařízení Server Linux, podle plánu toohello, které jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="31836-233">Mobility Service gets installed on hello Linux Server Device Collection, according toohello schedule you configured.</span></span>

## <a name="other-methods-tooinstall-mobility-service"></a><span data-ttu-id="31836-234">Ostatní metody tooinstall služba Mobility</span><span class="sxs-lookup"><span data-stu-id="31836-234">Other methods tooinstall Mobility Service</span></span>
<span data-ttu-id="31836-235">Zde jsou některé další možnosti pro instalaci služby Mobility:</span><span class="sxs-lookup"><span data-stu-id="31836-235">Here are some other options for installing Mobility Service:</span></span>
* [<span data-ttu-id="31836-236">Ruční instalace pomocí grafického uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="31836-236">Manual Installation using GUI</span></span>](http://aka.ms/mobsvcmanualinstall)
* [<span data-ttu-id="31836-237">Ruční instalace pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="31836-237">Manual Installation using command-line</span></span>](http://aka.ms/mobsvcmanualinstallcli)
* [<span data-ttu-id="31836-238">Nabízená instalace pomocí konfigurace serveru</span><span class="sxs-lookup"><span data-stu-id="31836-238">Push Installation using configuration server </span></span>](http://aka.ms/pushinstall)
* [<span data-ttu-id="31836-239">Automatická instalace pomocí Azure Automation a konfigurace požadovaného stavu</span><span class="sxs-lookup"><span data-stu-id="31836-239">Automated Installation using Azure Automation & Desired State Configuration </span></span>](http://aka.ms/mobsvcdscinstall)

## <a name="uninstall-mobility-service"></a><span data-ttu-id="31836-240">Odinstalujte službu Mobility</span><span class="sxs-lookup"><span data-stu-id="31836-240">Uninstall Mobility Service</span></span>
<span data-ttu-id="31836-241">Můžete vytvořit toouninstall balíčků nástroje Configuration Manager služby Mobility.</span><span class="sxs-lookup"><span data-stu-id="31836-241">You can create Configuration Manager packages toouninstall Mobility Service.</span></span> <span data-ttu-id="31836-242">Použijte následující skript toodo tak hello:</span><span class="sxs-lookup"><span data-stu-id="31836-242">Use hello following script toodo so:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="31836-243">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31836-243">Next steps</span></span>
<span data-ttu-id="31836-244">Nyní jste připraveni příliš[povolení ochrany](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmware-to-azure#step-6-replicate-applications) pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="31836-244">You are now ready too[enable protection](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmware-to-azure#step-6-replicate-applications) for your virtual machines.</span></span>
