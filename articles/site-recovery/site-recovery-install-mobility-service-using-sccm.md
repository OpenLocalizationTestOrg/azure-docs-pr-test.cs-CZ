---
title: "Instalace služby Mobility pro Azure Site Recovery automatizovat pomocí nástroje pro nasazení softwaru | Microsoft Docs"
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
ms.openlocfilehash: 49b72cd306aa91f114af7688f02d95db6f6eca05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automate-mobility-service-installation-by-using-software-deployment-tools"></a><span data-ttu-id="3c316-103">Instalace služby Mobility automatizovat pomocí nástroje pro nasazení softwaru</span><span class="sxs-lookup"><span data-stu-id="3c316-103">Automate Mobility Service installation by using software deployment tools</span></span>

>[!IMPORTANT]
<span data-ttu-id="3c316-104">Tento dokument předpokládá, že používáte verzi **9.9.4510.1** nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="3c316-104">This document assumes you are using version **9.9.4510.1** or higher.</span></span>

<span data-ttu-id="3c316-105">Tento článek obsahuje příklad, jak můžete pomocí nástroje System Center Configuration Manager nasadit službu Mobility Azure Site Recovery ve vašem datovém centru.</span><span class="sxs-lookup"><span data-stu-id="3c316-105">This article provides you an example of how you can use System Center Configuration Manager to deploy the Azure Site Recovery Mobility Service in your datacenter.</span></span> <span data-ttu-id="3c316-106">Pomocí nástroje pro nasazení softwaru, jako jsou nástroje Configuration Manager má následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3c316-106">Using a software deployment tool like Configuration Manager has the following advantages:</span></span>
* <span data-ttu-id="3c316-107">Plánování nasazení nové instalace a upgradu, během plánované údržby pro aktualizace softwaru</span><span class="sxs-lookup"><span data-stu-id="3c316-107">Scheduling deployment of fresh installations and upgrades, during your planned maintenance window for software updates</span></span>
* <span data-ttu-id="3c316-108">Škálování nasazení stovky serverů současně</span><span class="sxs-lookup"><span data-stu-id="3c316-108">Scaling deployment to hundreds of servers simultaneously</span></span>


> [!NOTE]
> <span data-ttu-id="3c316-109">Tento článek používá System Center Configuration Manager 2012 R2 k předvedení aktivity nasazení.</span><span class="sxs-lookup"><span data-stu-id="3c316-109">This article uses System Center Configuration Manager 2012 R2 to demonstrate the deployment activity.</span></span> <span data-ttu-id="3c316-110">Instalace služby Mobility může také automatizovat pomocí [Azure Automation a konfigurace požadovaného stavu](site-recovery-automate-mobility-service-install.md).</span><span class="sxs-lookup"><span data-stu-id="3c316-110">You could also automate Mobility Service installation by using [Azure Automation and Desired State Configuration](site-recovery-automate-mobility-service-install.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c316-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3c316-111">Prerequisites</span></span>
1. <span data-ttu-id="3c316-112">Nástroj nasazení softwaru, jako jsou nástroje Configuration Manager, který je už nasazená ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="3c316-112">A software deployment tool, like Configuration Manager, that is already deployed in your environment.</span></span>
  <span data-ttu-id="3c316-113">Vytvořte dvě [kolekce zařízení](https://technet.microsoft.com/library/gg682169.aspx), jeden pro všechny **servery Windows**a druhou pro všechny **servery se systémem Linux**, chcete chránit pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="3c316-113">Create two [device collections](https://technet.microsoft.com/library/gg682169.aspx), one for all **Windows servers**, and another for all **Linux servers**, that you want to protect by using Site Recovery.</span></span>
3. <span data-ttu-id="3c316-114">Konfigurační server, který je již zaregistrována k Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="3c316-114">A configuration server that is already registered with Site Recovery.</span></span>
4. <span data-ttu-id="3c316-115">Zabezpečené síťové sdílené složky (Server Message Block sdílené složky), který je přístupný pomocí serveru nástroje Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="3c316-115">A secure network file share (Server Message Block share) that can be accessed by the Configuration Manager server.</span></span>

## <a name="deploy-mobility-service-on-computers-running-windows"></a><span data-ttu-id="3c316-116">Nasazení služby Mobility na počítačích se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="3c316-116">Deploy Mobility Service on computers running Windows</span></span>
> [!NOTE]
> <span data-ttu-id="3c316-117">Tento článek předpokládá, že je IP adresa serveru konfigurace 192.168.3.121 a zda zabezpečené sdílení souborů \\\ContosoSecureFS\MobilityServiceInstallers.</span><span class="sxs-lookup"><span data-stu-id="3c316-117">This article assumes that the IP address of the configuration server is 192.168.3.121, and that the secure network file share is \\\ContosoSecureFS\MobilityServiceInstallers.</span></span>

### <a name="step-1-prepare-for-deployment"></a><span data-ttu-id="3c316-118">Krok 1: Příprava na nasazení</span><span class="sxs-lookup"><span data-stu-id="3c316-118">Step 1: Prepare for deployment</span></span>
1. <span data-ttu-id="3c316-119">Vytvořte složku ve sdílené síťové složce a pojmenujte ji **MobSvcWindows**.</span><span class="sxs-lookup"><span data-stu-id="3c316-119">Create a folder on the network share, and name it **MobSvcWindows**.</span></span>
2. <span data-ttu-id="3c316-120">Přihlaste se k konfigurační server a otevřete příkazový řádek pro správu.</span><span class="sxs-lookup"><span data-stu-id="3c316-120">Sign in to your configuration server, and open an administrative command prompt.</span></span>
3. <span data-ttu-id="3c316-121">Spusťte následující příkazy ke generování souboru přístupové heslo:</span><span class="sxs-lookup"><span data-stu-id="3c316-121">Run the following commands to generate a passphrase file:</span></span>

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. <span data-ttu-id="3c316-122">Kopírování **MobSvc.passphrase** soubor do **MobSvcWindows** složky do sdílené síťové složky.</span><span class="sxs-lookup"><span data-stu-id="3c316-122">Copy the **MobSvc.passphrase** file into the **MobSvcWindows** folder on your network share.</span></span>
5. <span data-ttu-id="3c316-123">Přejděte do instalačního programu úložiště na konfiguračním serveru tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3c316-123">Browse to the installer repository on the configuration server by running the following command:</span></span>

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. <span data-ttu-id="3c316-124">Kopírování  **Microsoft automatické obnovení systému\_uživatelský Agent\_*verze*\_Windows\_GA\_*datum* \_Release.exe** k **MobSvcWindows** složky do sdílené síťové složky.</span><span class="sxs-lookup"><span data-stu-id="3c316-124">Copy the **Microsoft-ASR\_UA\_*version*\_Windows\_GA\_*date*\_Release.exe** to the **MobSvcWindows** folder on your network share.</span></span>
7. <span data-ttu-id="3c316-125">Zkopírujte následující kód a uložte ho jako **install.bat** do **MobSvcWindows** složky.</span><span class="sxs-lookup"><span data-stu-id="3c316-125">Copy the following code, and save it as **install.bat** into the **MobSvcWindows** folder.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3c316-126">Nahraďte zástupné symboly [CSIP] ve skriptu skutečnými hodnotami IP adresy konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="3c316-126">Replace the [CSIP] placeholders in this script with the actual values of the IP address of your configuration server.</span></span>

```DOS
Time /t >> C:\Temp\logfile.log
REM ==================================================
REM ==== Clean up the folders ========================
RMDIR /S /q %temp%\MobSvc
MKDIR %Temp%\MobSvc
MKDIR C:\Temp
REM ==================================================

REM ==== Copy new files ==============================
COPY M*.* %Temp%\MobSvc
CD %Temp%\MobSvc
REN Micro*.exe MobSvcInstaller.exe
REM ==================================================

REM ==== Extract the installer =======================
MobSvcInstaller.exe /q /x:%Temp%\MobSvc\Extracted
REM ==== Wait 10s for extraction to complete =========
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

### <a name="step-2-create-a-package"></a><span data-ttu-id="3c316-127">Krok 2: Vytvoření balíčku</span><span class="sxs-lookup"><span data-stu-id="3c316-127">Step 2: Create a package</span></span>

1. <span data-ttu-id="3c316-128">Přihlaste se ke konzole nástroje Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="3c316-128">Sign in to your Configuration Manager console.</span></span>
2. <span data-ttu-id="3c316-129">Přejděte do **softwarová knihovna** > **Správa aplikací** > **balíčky**.</span><span class="sxs-lookup"><span data-stu-id="3c316-129">Browse to **Software Library** > **Application Management** > **Packages**.</span></span>
3. <span data-ttu-id="3c316-130">Klikněte pravým tlačítkem na **balíčky**a vyberte **vytvořit balíček**.</span><span class="sxs-lookup"><span data-stu-id="3c316-130">Right-click **Packages**, and select **Create Package**.</span></span>
4. <span data-ttu-id="3c316-131">Zadejte hodnoty pro název, popis, výrobce, jazyk a verzi.</span><span class="sxs-lookup"><span data-stu-id="3c316-131">Provide values for the name, description, manufacturer, language, and version.</span></span>
5. <span data-ttu-id="3c316-132">Vyberte **tento balíček obsahuje zdrojové soubory** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="3c316-132">Select the **This package contains source files** check box.</span></span>
6. <span data-ttu-id="3c316-133">Klikněte na tlačítko **Procházet**a vyberte síťové sdílené položce, kde je uložený instalační program (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).</span><span class="sxs-lookup"><span data-stu-id="3c316-133">Click **Browse**, and select the network share where the installer is stored (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).</span></span>

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package.png)

7. <span data-ttu-id="3c316-135">Na **zvolte typ programu, který chcete vytvořit** vyberte **standardní Program**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="3c316-135">On the **Choose the program type that you want to create** page, select **Standard Program**, and click **Next**.</span></span>

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. <span data-ttu-id="3c316-137">Na **zadejte informace o tomto standardním programu** stránky, zadejte následující vstupy a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="3c316-137">On the **Specify information about this standard program** page, provide the following inputs, and click **Next**.</span></span> <span data-ttu-id="3c316-138">(Další vstupních hodnot můžete použít výchozí hodnoty.)</span><span class="sxs-lookup"><span data-stu-id="3c316-138">(The other inputs can use their default values.)</span></span>

  | <span data-ttu-id="3c316-139">**Název parametru**</span><span class="sxs-lookup"><span data-stu-id="3c316-139">**Parameter name**</span></span> | <span data-ttu-id="3c316-140">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="3c316-140">**Value**</span></span> |
  |--|--|
  | <span data-ttu-id="3c316-141">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="3c316-141">Name</span></span> | <span data-ttu-id="3c316-142">Instalaci služby Mobility Microsoft Azure (Windows)</span><span class="sxs-lookup"><span data-stu-id="3c316-142">Install Microsoft Azure Mobility Service (Windows)</span></span> |
  | <span data-ttu-id="3c316-143">Příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="3c316-143">Command line</span></span> | <span data-ttu-id="3c316-144">Install.bat</span><span class="sxs-lookup"><span data-stu-id="3c316-144">install.bat</span></span> |
  | <span data-ttu-id="3c316-145">Program lze spustit</span><span class="sxs-lookup"><span data-stu-id="3c316-145">Program can run</span></span> | <span data-ttu-id="3c316-146">Zda je přihlášený uživatel</span><span class="sxs-lookup"><span data-stu-id="3c316-146">Whether or not a user is logged on</span></span> |

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties.png)

9. <span data-ttu-id="3c316-148">Na další stránce vyberte cíl operační systémy.</span><span class="sxs-lookup"><span data-stu-id="3c316-148">On the next page, select the target operating systems.</span></span> <span data-ttu-id="3c316-149">Služba mobility lze nainstalovat pouze na Windows Server 2012 R2, Windows Server 2012 a Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="3c316-149">Mobility Service can be installed only on Windows Server 2012 R2, Windows Server 2012, and Windows Server 2008 R2.</span></span>

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2.png)

10. <span data-ttu-id="3c316-151">Dokončete průvodce, klikněte na tlačítko **Další** dvakrát.</span><span class="sxs-lookup"><span data-stu-id="3c316-151">To complete the wizard, click **Next** twice.</span></span>


> [!NOTE]
> <span data-ttu-id="3c316-152">Skript podporuje i nové instalace služby Mobility agentů a aktualizace agentů, kteří jsou již nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="3c316-152">The script supports both new installations of Mobility Service agents and updates to agents that are already installed.</span></span>

### <a name="step-3-deploy-the-package"></a><span data-ttu-id="3c316-153">Krok 3: Nasazení balíčku</span><span class="sxs-lookup"><span data-stu-id="3c316-153">Step 3: Deploy the package</span></span>
1. <span data-ttu-id="3c316-154">V konzole nástroje Configuration Manager, klikněte pravým tlačítkem na váš balíček a vyberte **distribuovat obsah**.</span><span class="sxs-lookup"><span data-stu-id="3c316-154">In the Configuration Manager console, right-click your package, and select **Distribute Content**.</span></span>
  <span data-ttu-id="3c316-155">![Snímek obrazovky nástroje Configuration Manager konzoly](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span><span class="sxs-lookup"><span data-stu-id="3c316-155">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span></span>
2. <span data-ttu-id="3c316-156">Vyberte  **[distribuční body](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  na které balíčky by se měl zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="3c316-156">Select the **[distribution points](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)** on to which the packages should be copied.</span></span>
3. <span data-ttu-id="3c316-157">Dokončete průvodce.</span><span class="sxs-lookup"><span data-stu-id="3c316-157">Complete the wizard.</span></span> <span data-ttu-id="3c316-158">Balíček pak spustí replikující se do určených distribučních bodů.</span><span class="sxs-lookup"><span data-stu-id="3c316-158">The package then starts replicating to the specified distribution points.</span></span>
4. <span data-ttu-id="3c316-159">Po dokončení distribuci balíčku, klikněte pravým tlačítkem na balíček a vyberte **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="3c316-159">After the package distribution is done, right-click the package, and select **Deploy**.</span></span>
  <span data-ttu-id="3c316-160">![Snímek obrazovky nástroje Configuration Manager konzoly](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span><span class="sxs-lookup"><span data-stu-id="3c316-160">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span></span>
5. <span data-ttu-id="3c316-161">Vyberte kolekci zařízení systému Windows Server, kterou jste vytvořili v části předpoklady jako cílovou kolekci pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="3c316-161">Select the Windows Server device collection you created in the prerequisites section as the target collection for deployment.</span></span>

  ![Průvodce snímek nasazení softwaru](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection.png)

6. <span data-ttu-id="3c316-163">Na **zadejte cílové umístění obsahu** vyberte vaše **distribuční body**.</span><span class="sxs-lookup"><span data-stu-id="3c316-163">On the **Specify the content destination** page, select your **Distribution Points**.</span></span>
7. <span data-ttu-id="3c316-164">Na **zadejte nastavení pro kontrolu způsobu nasazení tohoto softwaru** stránka, zkontrolujte, zda je účelem **požadované**.</span><span class="sxs-lookup"><span data-stu-id="3c316-164">On the **Specify settings to control how this software is deployed** page, ensure that the purpose is **Required**.</span></span>

  ![Průvodce snímek nasazení softwaru](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. <span data-ttu-id="3c316-166">Na **zadejte plán pro toto nasazení** určete plán.</span><span class="sxs-lookup"><span data-stu-id="3c316-166">On the **Specify the schedule for this deployment** page, specify a schedule.</span></span> <span data-ttu-id="3c316-167">Další informace najdete v tématu [plánování balíčky](https://technet.microsoft.com/library/gg682178.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c316-167">For more information, see [scheduling packages](https://technet.microsoft.com/library/gg682178.aspx).</span></span>
9. <span data-ttu-id="3c316-168">Na **distribuční body** nakonfigurujte vlastnosti podle potřeb vašeho datového centra.</span><span class="sxs-lookup"><span data-stu-id="3c316-168">On the **Distribution Points** page, configure the properties according to the needs of your datacenter.</span></span> <span data-ttu-id="3c316-169">Dokončete průvodce.</span><span class="sxs-lookup"><span data-stu-id="3c316-169">Then complete the wizard.</span></span>

> [!TIP]
> <span data-ttu-id="3c316-170">Předejdete zbytečnému resetování Naplánujte instalaci balíčku během měsíčního časového období údržby nebo okno aktualizace softwaru.</span><span class="sxs-lookup"><span data-stu-id="3c316-170">To avoid unnecessary reboots, schedule the package installation during your monthly maintenance window or software updates window.</span></span>

<span data-ttu-id="3c316-171">Průběh nasazení můžete monitorovat pomocí konzoly nástroje Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="3c316-171">You can monitor the deployment progress by using the Configuration Manager console.</span></span> <span data-ttu-id="3c316-172">Přejděte na **monitorování** > **nasazení** > *[váš název balíčku]*.</span><span class="sxs-lookup"><span data-stu-id="3c316-172">Go to **Monitoring** > **Deployments** > *[your package name]*.</span></span>

  ![Snímek obrazovky nástroje Configuration Manager možnost monitorování nasazení](./media/site-recovery-install-mobility-service-using-sccm/report.PNG)

## <a name="deploy-mobility-service-on-computers-running-linux"></a><span data-ttu-id="3c316-174">Nasazení služby Mobility na počítačích se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="3c316-174">Deploy Mobility Service on computers running Linux</span></span>
> [!NOTE]
> <span data-ttu-id="3c316-175">Tento článek předpokládá, že je IP adresa serveru konfigurace 192.168.3.121 a zda zabezpečené sdílení souborů \\\ContosoSecureFS\MobilityServiceInstallers.</span><span class="sxs-lookup"><span data-stu-id="3c316-175">This article assumes that the IP address of the configuration server is 192.168.3.121, and that the secure network file share is \\\ContosoSecureFS\MobilityServiceInstallers.</span></span>

### <a name="step-1-prepare-for-deployment"></a><span data-ttu-id="3c316-176">Krok 1: Příprava na nasazení</span><span class="sxs-lookup"><span data-stu-id="3c316-176">Step 1: Prepare for deployment</span></span>
1. <span data-ttu-id="3c316-177">Vytvořte složku ve sdílené síťové složce a pojmenujte ji jako **MobSvcLinux**.</span><span class="sxs-lookup"><span data-stu-id="3c316-177">Create a folder on the network share, and name it as **MobSvcLinux**.</span></span>
2. <span data-ttu-id="3c316-178">Přihlaste se k konfigurační server a otevřete příkazový řádek pro správu.</span><span class="sxs-lookup"><span data-stu-id="3c316-178">Sign in to your configuration server, and open an administrative command prompt.</span></span>
3. <span data-ttu-id="3c316-179">Spusťte následující příkazy ke generování souboru přístupové heslo:</span><span class="sxs-lookup"><span data-stu-id="3c316-179">Run the following commands to generate a passphrase file:</span></span>

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. <span data-ttu-id="3c316-180">Kopírování **MobSvc.passphrase** soubor do **MobSvcLinux** složky do sdílené síťové složky.</span><span class="sxs-lookup"><span data-stu-id="3c316-180">Copy the **MobSvc.passphrase** file into the **MobSvcLinux** folder on your network share.</span></span>
5. <span data-ttu-id="3c316-181">Přejděte do instalačního programu úložiště na konfiguračním serveru spuštěním příkazu:</span><span class="sxs-lookup"><span data-stu-id="3c316-181">Browse to the installer repository on the configuration server by running the command:</span></span>

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. <span data-ttu-id="3c316-182">Zkopírujte následující soubory do **MobSvcLinux** složky do sdílené síťové složky:</span><span class="sxs-lookup"><span data-stu-id="3c316-182">Copy the following files to the **MobSvcLinux** folder on your network share:</span></span>
   * <span data-ttu-id="3c316-183">Microsoft automatické obnovení systému\_uživatelský Agent\*RHEL6 64*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="3c316-183">Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz</span></span>
   * <span data-ttu-id="3c316-184">Microsoft automatické obnovení systému\_uživatelský Agent\*RHEL7-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="3c316-184">Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz</span></span>
   * <span data-ttu-id="3c316-185">Microsoft automatické obnovení systému\_uživatelský Agent\*SLES11-SP3-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="3c316-185">Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz</span></span>
   * <span data-ttu-id="3c316-186">Microsoft automatické obnovení systému\_uživatelský Agent\*SLES11-SP4-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="3c316-186">Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz</span></span>
   * <span data-ttu-id="3c316-187">Microsoft automatické obnovení systému\_uživatelský Agent\*OL6-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="3c316-187">Microsoft-ASR\_UA\*OL6-64\*release.tar.gz</span></span>
   * <span data-ttu-id="3c316-188">Microsoft automatické obnovení systému\_uživatelský Agent\*UBUNTU 14.04-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="3c316-188">Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz</span></span>


7. <span data-ttu-id="3c316-189">Zkopírujte následující kód a uložte ho jako **install_linux.sh** do **MobSvcLinux** složky.</span><span class="sxs-lookup"><span data-stu-id="3c316-189">Copy the following code, and save it as **install_linux.sh** into the **MobSvcLinux** folder.</span></span>
   > [!NOTE]
   > <span data-ttu-id="3c316-190">Nahraďte zástupné symboly [CSIP] ve skriptu skutečnými hodnotami IP adresy konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="3c316-190">Replace the [CSIP] placeholders in this script with the actual values of the IP address of your configuration server.</span></span>

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
        echo "Installation has succeeded. Proceed to configuration." >> /tmp/MobSvc/sccm.log
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
        echo "Agent is already configured. Proceed to Upgrade." >> /tmp/MobSvc/sccm.log
        Upgrade
    else
        echo "Agent is not configured. Proceed to Configure." >> /tmp/MobSvc/sccm.log
        Configure
    fi
else
    Install
fi

cd /tmp

```

### <a name="step-2-create-a-package"></a><span data-ttu-id="3c316-191">Krok 2: Vytvoření balíčku</span><span class="sxs-lookup"><span data-stu-id="3c316-191">Step 2: Create a package</span></span>

1. <span data-ttu-id="3c316-192">Přihlaste se ke konzole nástroje Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="3c316-192">Sign in  to your Configuration Manager console.</span></span>
2. <span data-ttu-id="3c316-193">Přejděte do **softwarová knihovna** > **Správa aplikací** > **balíčky**.</span><span class="sxs-lookup"><span data-stu-id="3c316-193">Browse to **Software Library** > **Application Management** > **Packages**.</span></span>
3. <span data-ttu-id="3c316-194">Klikněte pravým tlačítkem na **balíčky**a vyberte **vytvořit balíček**.</span><span class="sxs-lookup"><span data-stu-id="3c316-194">Right-click **Packages**, and select **Create Package**.</span></span>
4. <span data-ttu-id="3c316-195">Zadejte hodnoty pro název, popis, výrobce, jazyk a verzi.</span><span class="sxs-lookup"><span data-stu-id="3c316-195">Provide values for the name, description, manufacturer, language, and version.</span></span>
5. <span data-ttu-id="3c316-196">Vyberte **tento balíček obsahuje zdrojové soubory** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="3c316-196">Select the **This package contains source files** check box.</span></span>
6. <span data-ttu-id="3c316-197">Klikněte na tlačítko **Procházet**a vyberte síťové sdílené položce, kde je uložený instalační program (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).</span><span class="sxs-lookup"><span data-stu-id="3c316-197">Click **Browse**, and select the network share where the installer is stored (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).</span></span>

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package-linux.png)

7. <span data-ttu-id="3c316-199">Na **zvolte typ programu, který chcete vytvořit** vyberte **standardní Program**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="3c316-199">On the **Choose the program type that you want to create** page, select **Standard Program**, and click **Next**.</span></span>

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. <span data-ttu-id="3c316-201">Na **zadejte informace o tomto standardním programu** stránky, zadejte následující vstupy a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="3c316-201">On the **Specify information about this standard program** page, provide the following inputs, and click **Next**.</span></span> <span data-ttu-id="3c316-202">(Další vstupních hodnot můžete použít výchozí hodnoty.)</span><span class="sxs-lookup"><span data-stu-id="3c316-202">(The other inputs can use their default values.)</span></span>

    | <span data-ttu-id="3c316-203">**Název parametru**</span><span class="sxs-lookup"><span data-stu-id="3c316-203">**Parameter name**</span></span> | <span data-ttu-id="3c316-204">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="3c316-204">**Value**</span></span> |
  |--|--|
  | <span data-ttu-id="3c316-205">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="3c316-205">Name</span></span> | <span data-ttu-id="3c316-206">Instalaci služby Mobility Microsoft Azure (Linux)</span><span class="sxs-lookup"><span data-stu-id="3c316-206">Install Microsoft Azure Mobility Service (Linux)</span></span> |
  | <span data-ttu-id="3c316-207">Příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="3c316-207">Command line</span></span> | <span data-ttu-id="3c316-208">./install_linux.SH</span><span class="sxs-lookup"><span data-stu-id="3c316-208">./install_linux.sh</span></span> |
  | <span data-ttu-id="3c316-209">Program lze spustit</span><span class="sxs-lookup"><span data-stu-id="3c316-209">Program can run</span></span> | <span data-ttu-id="3c316-210">Zda je přihlášený uživatel</span><span class="sxs-lookup"><span data-stu-id="3c316-210">Whether or not a user is logged on</span></span> |

  ![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-linux.png)

9. <span data-ttu-id="3c316-212">Na další stránce vyberte **tento program lze spustit na jakékoli platformě**.</span><span class="sxs-lookup"><span data-stu-id="3c316-212">On the next page, select **This program can run on any platform**.</span></span>
  <span data-ttu-id="3c316-213">![Snímek obrazovky vytvořením balíčku a programu Průvodce](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2-linux.png)</span><span class="sxs-lookup"><span data-stu-id="3c316-213">![Screenshot of Create Package and Program wizard](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2-linux.png)</span></span>

10. <span data-ttu-id="3c316-214">Dokončete průvodce, klikněte na tlačítko **Další** dvakrát.</span><span class="sxs-lookup"><span data-stu-id="3c316-214">To complete the wizard, click **Next** twice.</span></span>

> [!NOTE]
> <span data-ttu-id="3c316-215">Skript podporuje i nové instalace služby Mobility agentů a aktualizace agentů, kteří jsou již nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="3c316-215">The script supports both new installations of Mobility Service agents and updates to agents that are already installed.</span></span>

### <a name="step-3-deploy-the-package"></a><span data-ttu-id="3c316-216">Krok 3: Nasazení balíčku</span><span class="sxs-lookup"><span data-stu-id="3c316-216">Step 3: Deploy the package</span></span>
1. <span data-ttu-id="3c316-217">V konzole nástroje Configuration Manager, klikněte pravým tlačítkem na váš balíček a vyberte **distribuovat obsah**.</span><span class="sxs-lookup"><span data-stu-id="3c316-217">In the Configuration Manager console, right-click your package, and select **Distribute Content**.</span></span>
  <span data-ttu-id="3c316-218">![Snímek obrazovky nástroje Configuration Manager konzoly](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span><span class="sxs-lookup"><span data-stu-id="3c316-218">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span></span>
2. <span data-ttu-id="3c316-219">Vyberte  **[distribuční body](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  na které balíčky by se měl zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="3c316-219">Select the **[distribution points](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)** on to which the packages should be copied.</span></span>
3. <span data-ttu-id="3c316-220">Dokončete průvodce.</span><span class="sxs-lookup"><span data-stu-id="3c316-220">Complete the wizard.</span></span> <span data-ttu-id="3c316-221">Balíček pak spustí replikující se do určených distribučních bodů.</span><span class="sxs-lookup"><span data-stu-id="3c316-221">The package then starts replicating to the specified distribution points.</span></span>
4. <span data-ttu-id="3c316-222">Po dokončení distribuci balíčku, klikněte pravým tlačítkem na balíček a vyberte **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="3c316-222">After the package distribution is done, right-click the package, and select **Deploy**.</span></span>
  <span data-ttu-id="3c316-223">![Snímek obrazovky nástroje Configuration Manager konzoly](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span><span class="sxs-lookup"><span data-stu-id="3c316-223">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span></span>
5. <span data-ttu-id="3c316-224">Vyberte kolekci zařízení Linux Server, kterou jste vytvořili v části předpoklady jako cílovou kolekci pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="3c316-224">Select the Linux Server device collection you created in the prerequisites section as the target collection for deployment.</span></span>

  ![Průvodce snímek nasazení softwaru](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection-linux.png)

6. <span data-ttu-id="3c316-226">Na **zadejte cílové umístění obsahu** vyberte vaše **distribuční body**.</span><span class="sxs-lookup"><span data-stu-id="3c316-226">On the **Specify the content destination** page, select your **Distribution Points**.</span></span>
7. <span data-ttu-id="3c316-227">Na **zadejte nastavení pro kontrolu způsobu nasazení tohoto softwaru** stránka, zkontrolujte, zda je účelem **požadované**.</span><span class="sxs-lookup"><span data-stu-id="3c316-227">On the **Specify settings to control how this software is deployed** page, ensure that the purpose is **Required**.</span></span>

  ![Průvodce snímek nasazení softwaru](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. <span data-ttu-id="3c316-229">Na **zadejte plán pro toto nasazení** určete plán.</span><span class="sxs-lookup"><span data-stu-id="3c316-229">On the **Specify the schedule for this deployment** page, specify a schedule.</span></span> <span data-ttu-id="3c316-230">Další informace najdete v tématu [plánování balíčky](https://technet.microsoft.com/library/gg682178.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c316-230">For more information, see [scheduling packages](https://technet.microsoft.com/library/gg682178.aspx).</span></span>
9. <span data-ttu-id="3c316-231">Na **distribuční body** nakonfigurujte vlastnosti podle potřeb vašeho datového centra.</span><span class="sxs-lookup"><span data-stu-id="3c316-231">On the **Distribution Points** page, configure the properties according to the needs of your datacenter.</span></span> <span data-ttu-id="3c316-232">Dokončete průvodce.</span><span class="sxs-lookup"><span data-stu-id="3c316-232">Then complete the wizard.</span></span>

<span data-ttu-id="3c316-233">Služba mobility získá nainstalovaná na kolekci zařízení Server Linux podle plánu, který jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="3c316-233">Mobility Service gets installed on the Linux Server Device Collection, according to the schedule you configured.</span></span>

## <a name="other-methods-to-install-mobility-service"></a><span data-ttu-id="3c316-234">Další metody pro instalaci služby Mobility</span><span class="sxs-lookup"><span data-stu-id="3c316-234">Other methods to install Mobility Service</span></span>
<span data-ttu-id="3c316-235">Zde jsou některé další možnosti pro instalaci služby Mobility:</span><span class="sxs-lookup"><span data-stu-id="3c316-235">Here are some other options for installing Mobility Service:</span></span>
* [<span data-ttu-id="3c316-236">Ruční instalace pomocí grafického uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="3c316-236">Manual Installation using GUI</span></span>](http://aka.ms/mobsvcmanualinstall)
* [<span data-ttu-id="3c316-237">Ruční instalace pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="3c316-237">Manual Installation using command-line</span></span>](http://aka.ms/mobsvcmanualinstallcli)
* [<span data-ttu-id="3c316-238">Nabízená instalace pomocí konfigurace serveru</span><span class="sxs-lookup"><span data-stu-id="3c316-238">Push Installation using configuration server </span></span>](http://aka.ms/pushinstall)
* [<span data-ttu-id="3c316-239">Automatická instalace pomocí Azure Automation a konfigurace požadovaného stavu</span><span class="sxs-lookup"><span data-stu-id="3c316-239">Automated Installation using Azure Automation & Desired State Configuration </span></span>](http://aka.ms/mobsvcdscinstall)

## <a name="uninstall-mobility-service"></a><span data-ttu-id="3c316-240">Odinstalujte službu Mobility</span><span class="sxs-lookup"><span data-stu-id="3c316-240">Uninstall Mobility Service</span></span>
<span data-ttu-id="3c316-241">Můžete vytvořit balíčků nástroje Configuration Manager, odinstalujte službu Mobility.</span><span class="sxs-lookup"><span data-stu-id="3c316-241">You can create Configuration Manager packages to uninstall Mobility Service.</span></span> <span data-ttu-id="3c316-242">K tomu použijte následující skript:</span><span class="sxs-lookup"><span data-stu-id="3c316-242">Use the following script to do so:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="3c316-243">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c316-243">Next steps</span></span>
<span data-ttu-id="3c316-244">Nyní jste připraveni k [povolení ochrany](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmware-to-azure#step-6-replicate-applications) pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="3c316-244">You are now ready to [enable protection](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmware-to-azure#step-6-replicate-applications) for your virtual machines.</span></span>
