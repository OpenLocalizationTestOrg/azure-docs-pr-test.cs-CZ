---
title: "aaaScript vývoj akcí s HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toocustomize Hadoop clusterů s akce skriptu. Akce skriptu lze použít tooinstall systémem Hadoop clusteru nebo toochange hello konfigurací aplikace nainstalované v clusteru s podporou další software."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 836d68a8-8b21-4d69-8b61-281a7fe67f21
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 4fc3a389df8a003f7129ab00b4cd9bc7ad81a419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>Vývoj skriptů akce skriptu pro clustery se systémem HDInsight Windows
Zjistěte, jak toowrite akce skriptu skriptů pro HDInsight. Informace o použití akce skriptu skriptů najdete v tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md). Hello stejný článek napsán pro clustery HDInsight se systémem Linux naleznete v části [vyvíjet akce skriptu skripty pro HDInsight](hdinsight-hadoop-script-actions-linux.md).



> [!IMPORTANT]
> Hello kroky v této dokumentu funguje jenom pro clustery HDInsight se systémem Windows. HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Informace o použití akce skriptu s clustery se systémem Linux najdete v tématu [vývoj akcí skriptů v prostředí HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).
>
>



Akce skriptu lze použít tooinstall systémem Hadoop clusteru nebo toochange hello konfigurací aplikace nainstalované v clusteru s podporou další software. Akce skriptů jsou skripty, které jsou spuštěny na uzlech clusteru hello při nasazených clusterů HDInsight, a jsou prováděna po uzly v clusteru hello dokončení konfigurace HDInsight. Akce skriptu se spustí v části oprávnění k účtu správce systému a poskytuje úplná přístupová práva toohello uzly clusteru. Každý cluster lze zadat seznam toobe skript akce provést v hello pořadí, ve kterém jsou uvedené.

> [!NOTE]
> Pokud se setkáte hello následující chybová zpráva:
>
> System.Management.Automation.CommandNotFoundException; ExceptionMessage: hello termín 'Uložit-HDIFile' nebyl rozpoznán jako hello název rutiny, funkce, soubor skriptu nebo spustitelného programu. Kontrola pravopisu hello hello názvu, nebo pokud byl zahrnut cestu, ověřte, zda text hello cesta správné a zkuste to znovu.
> Je to proto, že jste nezahrnuli hello pomocné metody.  V tématu [pomocné metody pro vlastní skripty](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).
>
>

## <a name="sample-scripts"></a>Ukázkové skripty
Hello akce skriptu pro vytváření clusterů HDInsight v operačním systému Windows, je skript prostředí Azure PowerShell. Hello následující skript je ukázkou pro konfiguraci hello lokality konfigurační soubory:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable tooconfigure $ConfigFileName because it is not part of hello HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

skript Hello trvá čtyři parametry, hello název konfiguračního souboru, vlastnosti hello chcete toomodify, hodnota hello chcete tooset a popis. Například:

    hive-site.xml hive.metastore.client.socket.timeout 90

Tyto parametry nastaví hello hive.metastore.client.socket.timeout hodnotu too90 v souboru hive-site.xml hello.  Hello výchozí hodnota je 60 sekund.

Tento vzorový skript naleznete také v [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).

HDInsight nabízí několik skriptů tooinstall další součásti v clusterech HDInsight:

| Name (Název) | Skript |
| --- | --- |
| **Nainstalujte Spark** |https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. V tématu [instalací a použitím clustery Spark v HDInsight][hdinsight-install-spark]. |
| **Nainstalujte jazyk R** |https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. V tématu [instalací a použitím R v clusterech HDInsight][hdinsight-r-scripts]. |
| **Nainstalujte Solr** |https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. V tématu [instalace a použití clusterů v HDInsight Solr](hdinsight-hadoop-solr-install.md). |
| - **Nainstalujte Giraph** |https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. V tématu [instalace a použití clusterů v HDInsight Giraph](hdinsight-hadoop-giraph-install.md). |

Akce skriptu můžete nasadit z hello portál Azure, Azure PowerShell nebo pomocí hello HDInsight .NET SDK.  Další informace najdete v tématu [HDInsight přizpůsobit clustery pomocí akce skriptu][hdinsight-cluster-customize].

> [!NOTE]
> Ukázkové skripty Hello pracovat pouze s verze clusteru HDInsight verze 3.1 nebo vyšší. Další informace o verzích clusterů HDInsight, naleznete v části [verze clusteru HDInsight](hdinsight-component-versioning.md).
>
>

## <a name="helper-methods-for-custom-scripts"></a>Pomocné metody pro vlastní skripty
Pomocné metody akcí skriptů jsou nástroje, které můžete použít při zápisu vlastních skriptů. Tyto metody jsou definovány v [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1)a můžou být součástí skripty pomocí hello následující ukázka:

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module toomake writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed tooload HDInsightUtilities module, exiting ...";
        exit;
    }

Zde jsou hello pomocné metody, které jsou poskytovány tento skript:

| Pomocná metoda | Popis |
| --- | --- |
| **Uložit HDIFile** |Stažení souboru z hello zadaný identifikátor URI (Uniform Resource) tooa umístění na hello místní disk, který je přidružen cluster pro uzly přiřazené toohello hello virtuálního počítače Azure. |
| **Rozbalte položku HDIZippedFile** |Rozbalte soubor ZIP. |
| **Vyvolání HDICmdScript** |Spusťte skript z cmd.exe. |
| **Zápis HDILog** |Zapište výstup z vlastní skript hello používané pro akci skriptu. |
| **Get-Services** |Získejte seznam služby spuštěné na počítači hello kde hello skript spustí. |
| **Get-Service** |S názvem hello konkrétní služby jako vstup, získat podrobné informace pro konkrétní službu (název služby, zpracovat ID, stavu, atd.) v počítači hello kde hello skript spustí. |
| **Get-HDIServices** |Získejte seznam služby HDInsight na hello počítači spuštěny, kde hello skript spustí. |
| **Get-HDIService** |S hello konkrétní HDInsight název služby jako vstup, získat podrobné informace pro konkrétní službu (název služby, zpracovat ID, stavu, atd.) v počítači hello kde hello skript spustí. |
| **Get-ServicesRunning** |Získáte seznam služeb, které jsou spuštěny v počítači hello kde hello skript spustí. |
| **Get-ServiceRunning** |Zkontrolujte, zda specifické služby (podle názvu) je spuštěn v počítači hello, kde hello skript spustí. |
| **Get-HDIServicesRunning** |Získejte seznam služby HDInsight na hello počítači spuštěny, kde hello skript spustí. |
| **Get-HDIServiceRunning** |Zkontrolujte, zda specifické služby HDInsight (podle názvu) je spuštěn na hello počítači, kde hello skript spustí. |
| **Get-HDIHadoopVersion** |Vrátí hello Hadoop nainstalovaná na počítači hello kde hello skript spustí. |
| **Test IsHDIHeadNode** |Zkontrolujte, zda text hello kde hello skript spustí hlavního uzlu. |
| **Test IsActiveHDIHeadNode** |Zkontrolujte, zda text hello kde hello skript spustí active hlavního uzlu. |
| **Test IsHDIDataNode** |Zkontrolujte, zda text hello kde hello skript spustí datový uzel. |
| **Upravit HDIConfigFile** |Upravte hello konfigurační soubory hive-site.xml, core-site.xml, hdfs-site.xml, mapred-site.xml nebo yarn-site.xml. |

## <a name="best-practices-for-script-development"></a>Osvědčené postupy pro vývoj skriptů
Při vývoji vlastních skriptů pro cluster služby HDInsight, existuje několik osvědčených postupů tookeep nezapomeňte:

* Kontrola verze Hadoop hello

    Pouze HDInsight verze 3.1 (Hadoop 2.4) a vyšší podpora použití akce skriptu tooinstall vlastní součásti v clusteru. Ve vašem vlastního skriptu, je nutné použít hello **Get-HDIHadoopVersion** Pomocná metoda toocheck hello Hadoop verze než budete pokračovat v provádění další úloh ve skriptu hello.
* Zadejte, že stabilní odkazy tooscript prostředky

    Uživatelé měli ujistit, všechny hello skripty a další artefaktů použít v hello přizpůsobení clusteru zůstaly dostupné v celém hello životnost hello clusteru a že hello verze těchto souborů se nezmění pro dobu trvání hello. Pokud hello obnovování uzlů v clusteru hello je vyžadováno, je nutné tyto prostředky. Hello osvědčeným postupem je toodownload a archivaci, které se nacházejí v účtu úložiště, který hello uživatelské ovládací prvky. To může být hello výchozí účet úložiště nebo některé z dalších účtů úložiště hello zadaný v době hello nasazení pro vlastní cluster.
    V hello Spark a R přizpůsobit clusteru ukázky zadaný hello dokumentace, například jsme provedli místní kopie hello prostředků v rámci tohoto účtu úložiště: https://hdiconfigactions.blob.core.windows.net/.
* Zajistěte, aby hello clusteru přizpůsobení skriptu idempotent

    Měli očekávat, že je hello uzly clusteru HDInsight obnovit z Image během doby života hello clusteru. spuštění skriptu přizpůsobení clusteru Hello vždy, když je obnovit z Image clusteru. Tento skript musí být navrženou toobe idempotent v hello smyslu, že při obnovování, hello skriptu by měl zajistěte, aby že tento cluster hello se vrátí stejné přizpůsobené stavu, který byl právě po hello skript spustili pro hello první čas, kdy hello clusteru bylo původně toohello vytvořit. Například pokud vlastní skript nainstalovat aplikaci na D:\AppLocation na prvním spuštění, pak na každé následné spuštění při obnovování, by měl hello skriptu zkontrolujte, zda text hello aplikace existuje v hello D:\AppLocation umístění než budete pokračovat s jinými kroky ve skriptu hello.
* Vlastní součásti nainstalovat na optimální umístění hello

    Když se obnoví z Image uzly clusteru, hello C:\ prostředků disku a jednotku D:\ systému můžete naformátována, tím hello dojít ke ztrátě dat a aplikací nainstalovaných na těchto jednotkách. To může také dojít, pokud do virtuálních počítačů (VM) Azure uzlu, který je součástí clusteru hello přestane fungovat a bude nahrazen nový uzel. Součásti můžete nainstalovat na jednotku D:\ hello nebo v umístění C:\apps hello v clusteru hello. Všech jiných umístění na jednotce C:\ hello jsou vyhrazené. Zadejte hello umístění, kde jsou aplikace nebo knihovny toobe nainstalován ve skriptu přizpůsobení hello clusteru.
* Ujistěte se, vysokou dostupnost clusteru architektury hello

    HDInsight má aktivní – pasivní architekturu pro zajištění vysoké dostupnosti, v head jeden uzel, který je v aktivním režimu (kde hello HDInsight jsou spuštěny služby) a hello jiných hlavního uzlu v pohotovostním režimu (v HDInsight, které nejsou spuštěny služby). uzly Hello přepínače aktivní a pasivní režim, pokud jsou přerušení služby HDInsight. Pokud akce skriptu použité tooinstall služeb v obou head uzlů pro vysokou dostupnost, Poznámka že této hello HDInsight mechanismus převzetí služeb při selhání není možné tooautomatically selhání přes tyto služby uživatel nainstaloval. Proto uživatel nainstaloval služeb v HDInsight head uzlů, které jsou očekávané toobe vysokou dostupností musí mít vlastní mechanismus převzetí služeb při selhání, pokud v režimu aktivní pasivní nebo v režimu aktivní aktivní.

    Příkaz akce skriptu HDInsight spustí na obou hlavních uzlech při role hello hlavní uzel je zadán jako hodnota v hello *ClusterRoleCollection* parametr. Proto při návrhu vlastní skript, ujistěte se, že váš skript známa tento instalační program. Nespouštějte k potížím, kde hello stejné služby jsou instalaci a spuštění na oba uzly head hello a jejich skončili neslučitelných mezi sebou. Navíc mějte na paměti, že dojde ke ztrátě dat během obnovování, takže softwaru nainstalované prostřednictvím akce skriptu má toobe odolné toosuch události. Aplikace by měla být navrženou toowork s vysokou dostupností data, která se distribuuje do mnoha uzly. Všimněte si, že až 1/5 hello uzlů v clusteru lze obnovit z Image na hello stejnou dobu.
* Konfigurace úložiště objektů Azure Blob toouse hello vlastní komponenty

    Hello vlastní součásti, které instalujete na uzlech clusteru hello pravděpodobně výchozí konfigurace toouse Hadoop Distributed File System (HDFS) úložiště. Místo toho byste měli změnit hello konfigurace toouse úložiště objektů Blob v Azure. Na obnovení z Image clusteru systému souborů HDFS hello získá formátu a by ztratíte všechna data, která je uložena existuje. Použití úložiště objektů Blob v Azure místo toho zajišťuje, aby vaše data se uchovávají.

## <a name="common-usage-patterns"></a>Obecné vzory využití
Tato část obsahuje pokyny k implementaci některé hello běžné využití vzory, které se mohou vyskytnout při zápisu vlastních skriptů.

### <a name="configure-environment-variables"></a>Nakonfigurujte proměnné prostředí
Často v vývoj akcí skriptů, si myslíte, že hello potřebovat tooset proměnné prostředí. Například je nejpravděpodobnější scénář při stahování binární z externího webu, nainstalujte na hello clusteru a přidejte hello umístění tam, kde je proměnná prostředí nainstalovaná tooyour 'PATH'. Hello následující fragment kódu ukazuje, jak tooset proměnné prostředí v hello vlastních skriptů.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

Tento příkaz nastaví proměnnou prostředí hello **MDS_RUNNER_CUSTOM_CLUSTER** toohello hodnotu "true" a také nastaví hello oboru této proměnné toobe celého systému. V některých případech je důležité, aby proměnné prostředí se nastavují v příslušné oboru hello – počítače nebo uživatele. Odkazovat [sem] [ 1] Další informace o nastavení proměnných prostředí.

### <a name="access-toolocations-where-hello-custom-scripts-are-stored"></a>Přístup k toolocations, kde jsou uloženy vlastní skripty hello
Skripty použité toocustomize tooeither clusteru musí být v hello výchozí účet úložiště pro hello cluster nebo ve veřejném kontejneru jen pro čtení na jiný účet úložiště. Pokud skript odkazuje na prostředky, které se nacházejí jinde požadavky toobe ve veřejně přístupné (alespoň veřejné jen pro čtení). Pro instanci může tooaccess soubor a uložte ji pomocí příkazu hello SaveFile HDI.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

V tomto příkladu můžete zajistit, aby byl tento kontejner hello 'somecontainer' v účtu úložiště 'somestorageaccount' veřejně přístupná. Skript hello, jinak hodnota vyhodí výjimku "Nebyl nalezen" a selhání.

### <a name="pass-parameters-toohello-add-azurermhdinsightscriptaction-cmdlet"></a>Předávání parametrů toohello přidat AzureRmHDInsightScriptAction rutiny
toopass více rutiny toohello AzureRmHDInsightScriptAction přidat parametry, musíte všechny parametry pro tooformat hello řetězec hodnotu toocontain hello skriptu. Například:

    "-CertifcateUri wasb:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

nebo

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Throw – výjimka pro nasazení clusteru se nezdařilo
Tooget přesně oznámení faktu hello, pokud přizpůsobení tohoto clusteru se nezdařilo podle očekávání, je důležité toothrow výjimku a vytváření clusteru hello nezdaří. Můžete například chcete tooprocess soubor, pokud existuje a zpracovat hello chyba případy, kdy hello soubor neexistuje. To by zajistěte, aby skript hello ukončí řádně a se správně označuje stav hello hello clusteru. Hello následující fragment kódu poskytuje příklad tooachieve toto:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

V tento fragment kódu Pokud soubor hello neexistoval, by vedlo tooa stavu hello skriptu ve skutečnosti řádně ukončí po tisku hello chybová zpráva, kde hello clusteru dosáhne stavu spuštěno za předpokladu, že ji "úspěšně" dokončit proces přizpůsobení clusteru. Toobe přesně oznámení faktu hello, pokud přizpůsobení tohoto clusteru v podstatě se nezdařilo z důvodu chybějícího souboru očekávaným způsobem, je vhodnější toothrow výjimku a selhání hello clusteru přizpůsobení krok. tooachieve to hello následující fragment kódu ukázka místo toho musíte použít.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Kontrolní seznam pro nasazení akce skriptu
Zde jsou hello kroky, které jsme trvalo při přípravě toodeploy tyto skripty:

1. Uveďte hello soubory, které obsahují vlastní skripty hello na místě, které je přístupné uzly clusteru hello během nasazení. To může být libovolná z výchozí hello nebo další účty úložiště zadaný v době hello nasazení clusteru nebo jiných veřejně přístupná úložiště kontejneru.
2. Přidat kontroly do skriptů toomake jistotu, že spouštění idempotently, tak, aby hello skriptu lze provést několikrát u hello stejného uzlu.
3. Použití hello **Write-Output** tooSTDOUT tooprint rutiny prostředí Azure PowerShell, jakož i STDERR. Nepoužívejte **Write-Host**.
4. Použijte složku dočasného souboru, jako je například $env: dočasné, tookeep hello stažený soubor používá hello skripty a pak vyčištění je po mají spouštět skripty.
5. Nainstalujte jenom na D:\ nebo C:\apps vlastní software. Jiných umístění na jednotce C: hello nepoužívejte, protože se jedná o vyhrazené. Všimněte si, že instalaci souborů na jednotce C: hello mimo složku C:\apps hello může vést instalace selhání během reimages hello uzlu.
6. V případě hello, nastavení na úrovni operačního systému nebo Hadoop služby konfigurační soubory se změnily můžete služby HDInsight toorestart tak, aby se vyzvedávat všechna nastavení, úrovni operačního systému, jako je například nastavit ve skriptech hello proměnné prostředí hello.

## <a name="debug-custom-scripts"></a>Ladění vlastních skriptů
protokoly chyb skriptu Hello ukládají spolu s další výstupu v hello výchozí úložiště účet, který jste zadali pro hello clusteru při jeho vytváření. Hello protokoly se ukládají v tabulce s názvem hello *u < \cluster-name-fragment >< \time-stamp > setuplog*. Toto jsou agregovaná protokoly, které mají záznamy ze všech uzlů hello (hlavního uzlu a pracovní uzly) na které hello skript se spustí v clusteru hello.
Snadný způsob toocheck hello protokoly je toouse nástroje HDInsight pro Visual Studio. Instalace nástrojů hello, najdete v části [začněte používat nástroje Visual Studio Hadoop pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)

**toocheck hello protokolu pomocí sady Visual Studio**

1. Otevřete sadu Visual Studio.
2. Klikněte na tlačítko **zobrazení**a potom klikněte na **Průzkumníka serveru**.
3. Klikněte pravým tlačítkem na "Azure", klikněte na tlačítko připojit příliš**předplatná Microsoft Azure**a pak zadejte svoje přihlašovací údaje.
4. Rozbalte položku **úložiště**, rozbalte účet úložiště Azure hello používá jako výchozí systém souborů hello, rozbalte položku **tabulky**a potom dvakrát klikněte na název tabulky hello.

Můžete také vzdáleného do toosee uzly clusteru hello STDOUT a STDERR pro vlastní skripty. Hello protokoly na každém uzlu jsou konkrétním uzlu pouze toothat a se protokolují do **C:\HDInsightLogs\DeploymentAgent.log**. Tyto soubory protokolu zaznamenejte všechny výstupy z hello vlastních skriptů. Fragment kódu příklad protokolu pro akci skriptu Spark vypadá takto:

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


Tento protokol je jasné, že akce skriptu Spark hello provedl na hello virtuálního počítače s názvem HEADNODE0 a že žádné výjimky došlo k během provádění hello.

V hello události, k níž dojde k chybě provádění je výstup hello popisující ho také obsažený v tomto souboru protokolu. Hello informací uvedených v těchto protokolech by měl být užitečné při ladění skriptu problémy, které by mohlo dojít.

## <a name="see-also"></a>Viz také
* [Přizpůsobení clusterů HDInsight pomocí akce skriptu][hdinsight-cluster-customize]
* [Nainstalovat a používat Spark v HDInsight clustery][hdinsight-install-spark]
* [Nainstalovat a používat R na clustery HDInsight][hdinsight-r-scripts]
* [Nainstalovat a používat Solr v clusterech HDInsight](hdinsight-hadoop-solr-install.md).
* [Nainstalovat a používat Giraph v clusterech HDInsight](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
