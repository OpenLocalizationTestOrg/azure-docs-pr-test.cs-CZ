---
title: "aaaUsing tooDev tooPublish skriptů prostředí PowerShell systému Windows a testovací prostředí | Microsoft Docs"
description: "Zjistěte, jak toouse prostředí Windows PowerShell skriptů z Visual Studio toopublish toodevelopment a testovací prostředí."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5fff1301-5469-4d97-be88-c85c30f837c1
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 491a058f96255576afa74f6156f20ae9559bb9f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-windows-powershell-scripts-toopublish-toodev-and-test-environments"></a>Pomocí prostředí Windows PowerShell skripty toopublish toodev a testovací prostředí
Když vytvoříte webovou aplikaci v sadě Visual Studio, můžete vygenerovat skript prostředí Windows PowerShell, které můžete později publikování tooautomate hello vašeho webu tooAzure jako webové aplikace ve službě Azure App Service nebo virtuální počítač. Můžete upravit a rozšířit své požadavky na skriptu prostředí Windows PowerShell hello v toosuit editor Visual Studio hello nebo skriptu hello integrovat existující sestavení, testování a publikování skripty.

Pomocí těchto skriptů, můžete zřídit vlastní verze webu pro dočasné použití (také označované jako vývojářů a testovací prostředí). Může například nastavit na konkrétní verzi vašeho webu na virtuálním počítači Azure nebo na hello pracovní patice na webu toorun sady testů, reprodukujte chyby, testovací oprava chyb, zkušební verze navrhované změny nebo nastavit vlastní prostředí pro ukázku nebo prezentace. Po vytvoření skriptu, který publikuje projektu, můžete znovu vytvořit stejná prostředí znovu spuštěním skriptu hello podle potřeby nebo spusťte skript hello s vlastní sestavení vaší webové aplikace toocreate vlastního prostředí pro testování.

## <a name="what-you-need"></a>Co potřebujete
* Azure SDK 2.3 nebo novější. V tématu [Visual Studio stáhne](http://go.microsoft.com/fwlink/?LinkID=624384) Další informace.

Není nutné hello Azure SDK toogenerate hello skripty pro webové projekty. Tato funkce je pro webové projekty, není webové role v cloudové služby.

* Prostředí Azure PowerShell 0.7.4 nebo novější. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace.
* [Prostředí Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) nebo novější.

## <a name="additional-tools"></a>Další nástroje
Další nástroje a zdroje pro práci s prostředím PowerShell v sadě Visual Studio pro vývoj pro Azure jsou k dispozici. V tématu [prostředí PowerShell nástroje pro sadu Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).

## <a name="generating-hello-publish-scripts"></a>Generování hello publikovat skripty
Můžete vygenerovat hello publikovat skripty pro virtuální počítač, který je hostitelem vašeho webu, když vytvoříte nový projekt pomocí následujících [tyto pokyny](virtual-machines/windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Můžete také [generovat publikovat skripty pro webové aplikace v Azure App Service](app-service-web/app-service-web-get-started-dotnet.md).

## <a name="scripts-that-visual-studio-generates"></a>Skripty, které generuje Visual Studio
Visual Studio vytvoří řešení úrovni složku s názvem **PublishScripts** obsahující dva soubory prostředí Windows PowerShell, skript publikování pro virtuální počítač nebo webu a modul, který obsahuje funkce, které můžete použít v hello skripty. Visual Studio také vygeneruje soubor ve formátu JSON hello, která určuje podrobnosti hello hello projektu, které nasazujete.

### <a name="windows-powershell-publish-script"></a>Publikování skriptu prostředí Windows PowerShell
Hello publikování skript obsahuje konkrétní publikování kroky pro nasazení webu tooa nebo virtuálního počítače. Visual Studio poskytuje pro prostředí Windows PowerShell vývoj zvýrazňování syntaxe. Nápověda pro funkce hello je k dispozici, a můžete volně upravit funkce hello ve skriptu toosuit hello proměnlivé potřeby.

### <a name="windows-powershell-module"></a>Modul prostředí Windows PowerShell
Hello prostředí Windows PowerShell modul, který generuje Visual Studio obsahuje funkce, které hello publikovat používá skript. Tyto jsou funkce Azure PowerShell a nejsou určený toobe upravit. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace.

### <a name="json-configuration-file"></a>Konfigurační soubor JSON
soubor JSON Hello se vytvoří v hello **konfigurace** složky a obsahuje konfigurační data, která určuje přesně které tooAzure toodeploy prostředky. Název Hello hello souboru, který generuje Visual Studio je projekt název WAWS-dev.json Pokud jste vytvořili na webu nebo projektu název VM-dev.json, pokud jste vytvořili virtuální počítač. Tady je příklad konfiguračního souboru JSON, který se vygeneruje při vytvoření webu. Většina hodnot hello je není potřeba vysvětlovat. název webu Hello je generován Azure, takže se nemusí odpovídat název projektu.

```json
{
    "environmentSettings": {
        "webSite": {
            "name": "WebApplication26632",
            "location": "West US"
        },
        "databases": [{
            "connectionStringName": "DefaultConnection",
            "databaseName": "WebApplication26632_db",
            "serverName": "YourDatabaseServerName",
            "user": "sqluser2",
            "password": "",
            "edition": "",
            "size": "",
            "collation": "",
            "location": "West US"
        }]
    }
}
```
Když vytvoříte virtuální počítač, konfigurační soubor JSON hello vypadá podobně jako následující toohello. Všimněte si, že Cloudová služba je vytvořen jako kontejner pro hello virtuálního počítače. Hello virtuální počítač obsahuje hello obvyklé koncové body pro webový přístup prostřednictvím protokolu HTTP a HTTPS, stejně jako koncové body pro nasazení webu, která umožňuje publikovat toohello webu z vašeho místního počítače, vzdálené plochy a prostředí Windows PowerShell.

```json
{
    "environmentSettings": {
        "cloudService": {
            "name": "myusernamevm1",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myusernamevm1",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [{
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [{
            "connectionStringName": "",
            "databaseName": "",
            "serverName": "",
            "user": "",
            "password": ""
        }],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Můžete upravit hello JSON konfigurace toochange co se stane, když spustíte hello publikovat skripty. Hello `cloudService` a `virtualMachine` oddíly jsou povinné, ale můžete odstranit hello `databases` části Pokud tomu tak není. Hello vlastnosti, které jsou prázdné v hello výchozí konfigurační soubor, který generuje Visual Studio jsou volitelné. ty, které mají v konfiguračním souboru na hello výchozí hodnoty jsou povinné.

Pokud máte web, který má několik prostředí nasazení (označované jako sloty) místo jednoho pracoviště v Azure, můžete zahrnout název slotu hello v hello název webu hello v konfiguračním souboru JSON hello. Například, pokud máte web s názvem **server** a slotu pro něj s názvem **testování** pak hello identifikátor URI je server test.cloudapp.net, ale hello správný název toouse v konfiguračním souboru hello je mysite(test) . Můžete provést jenom to pokud hello webu a sloty již existují v rámci vašeho předplatného. Pokud ještě neexistují, vytvoření webu hello spuštěním skriptu hello bez zadání hello slot a pak vytvořit hello slot v hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), a poté spusťte skript hello s názvem hello upravené webové stránky. Další informace o nasazovací sloty pro webové aplikace najdete v tématu [nastavení přípravných prostředí pro webové aplikace v Azure App Service](app-service-web/web-sites-staged-publishing.md).

## <a name="how-toorun-hello-publish-scripts"></a>Jak publikovat toorun hello skripty
Pokud jste spustili nikdy skript prostředí Windows PowerShell před, musíte nejprve nastavit hello provádění zásad tooenable skripty toorun. Toto je uživatel tooprevent funkce zabezpečení ve spouštění skriptů prostředí Windows PowerShell, pokud jsou snadno napadnutelný toomalware nebo viry, které zahrnují spouštění skriptů.

### <a name="run-hello-script"></a>Spusťte skript hello
1. Vytvořte hello balíčku nástroje nasazení webu pro váš projekt. Balíček nasazení webu je komprimovaný archiv (soubor .zip), které obsahují soubory, které chcete toocopy tooyour webu nebo virtuálního počítače. Balíčky webového nasazení v sadě Visual Studio můžete vytvořit pro všechny webové aplikace.

![Vytváření webového nasazení balíčku](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

Další informace najdete v tématu [postupy: vytvoření balíčku pro nasazení webu v sadě Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx). Můžete také automatizovat hello vytvoření vašeho balíčku nástroje nasazení webu, jak je popsáno v části hello **přizpůsobení a rozšíření hello publikovat skripty** dál v tomto tématu.

1. V **Průzkumníku řešení**, otevřete hello kontextovou nabídku hello skript a potom zvolte **otevřete pomocí prostředí PowerShell ISE**.
2. Pokud je to hello poprvé, co jste na tomto počítači spuštěno skriptů prostředí Windows PowerShell, otevřete okno příkazového řádku s oprávněními správce a hello zadejte následující příkaz:

    ```powershell
    Set-ExecutionPolicy RemoteSigned
    ```

3. Přihlaste se tooAzure pomocí hello následující příkaz.

    ```powershell
    Add-AzureAccount
    ```

    Po zobrazení výzvy zadejte své uživatelské jméno a heslo.

    Všimněte si, že při automatizaci hello skriptu, tato metoda poskytnout přihlašovací údaje Azure nebude fungovat. Místo toho používejte hello přihlašovací údaje tooprovide soubor .publishsettings. Jednou pouze, můžete použít příkaz hello **Get-AzurePublishSettingsFile** toodownload hello z Azure a následně použít **Import AzurePublishSettingsFile** tooimport hello souboru. Podrobné pokyny najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

4. (Volitelné) Pokud chcete, aby toocreate Azure prostředkům, například hello virtuálního počítače, databáze a webu bez publikování webové aplikace, použijte hello **publikovat WebApplication.ps1** s hello **-konfigurace** argument nastaven toohello JSON konfigurační soubor. Tento příkaz používá hello JSON konfigurační soubor toodetermine které toocreate prostředky. Protože výchozí nastavení hello používá pro další argumenty příkazového řádku, vytvoří hello prostředků, ale nepodporuje publikování webové aplikace. Hello – podrobné možnost získáte další informace o co se děje.

    ```powershell
    Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json
    ```

5. Použití hello **publikovat WebApplication.ps1** příkaz, jak je znázorněno v jednom z hello následující příklady tooinvoke hello skriptu a publikování webové aplikace. Pokud potřebujete toooverride hello výchozí nastavení pro všechny hello další argumenty, jako je například název odběru hello, publikování název balíčku, přihlašovací údaje virtuálního počítače nebo přihlašovací údaje databáze serveru, můžete určit tyto parametry. Použití hello **– podrobný** možnost toosee Další informace o průběhu hello hello procesu publikování.

    ```powershell
    Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
    –SubscriptionName Contoso `
    -WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
    -DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
    -Verbose
    ```

    Pokud vytváříte virtuální počítač, hello příkaz vypadat jako následující hello. Tento příklad také ukazuje, jak toospecify hello přihlašovací údaje pro více databází. Hello virtuálních počítačů, které vytvářejí tyto skripty není certifikát SSL hello z důvěryhodnou kořenovou autoritou. Proto je nutné toouse hello **– AllowUntrusted** možnost.

    ```powershell
    Publish-WebApplication.ps1 `
    -Configuration C:\Path\ADVM-VM-test.json `
    -SubscriptionName Contoso `
    -WebDeployPackage C:\Path\ADVM.zip `
    -AllowUntrusted `
    -VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
    -DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
    -Verbose
    ```

    databáze můžete vytvářet Hello skript, ale nevytvoří databázové servery. Pokud chcete, aby toocreate databázový server, můžete použít hello **New-AzureSqlDatabaseServer** funkce v hello Azure modulu.

## <a name="customizing-and-extending-hello-publish-scripts"></a>Přizpůsobení a rozšíření hello publikování skripty
Můžete přizpůsobit hello publikovat skriptu a konfigurační soubor JSON. Hello funkce v modulu Windows PowerShell hello **AzureWebAppPublishModule.psm1** nejsou určený toobe upravit. Chcete-li právě toospecify jiné databázi nebo změnit některé vlastnosti hello hello virtuálního počítače, upravte konfigurační soubor JSON hello. Pokud chcete tooextend hello funkce hello skriptu tooautomate vytváření a testování projektu, můžete implementovat funkce zástupných procedur v **publikovat WebApplication.ps1**.

tooautomate sestavení projektu, přidat kód, který volá MSBuild příliš`New-WebDeployPackage` jak ukazuje tento příklad kódu. Hello cesta toohello příkaz MSBuild se liší v závislosti na verzi hello sady Visual Studio, které jste nainstalovali. tooget hello správnou cestu, můžete použít funkce hello **Get-MSBuildCmd**, jak je uvedeno v následujícím příkladu.

### <a name="tooautomate-building-your-project"></a>tooautomate sestavení projektu
1. Přidat hello `$ProjectFile` parametru v části globální param hello.

    ```powershell
    [Parameter(Mandatory = $false)]
    [ValidateScript({Test-Path $_ -PathType Leaf})]
    [String]
    $ProjectFile,
    ```

2. Copy – funkce hello `Get-MSBuildCmd` do souboru skriptu.

    ```powershell
    function Get-MSBuildCmd
    {
            process
    {

                $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                    Sort-Object {[double]$_.PSChildName} -Descending |
                                    Select-Object -First 1 |
                                    Get-ItemProperty -Name MSBuildToolsPath |
                                    Select -ExpandProperty MSBuildToolsPath

                $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

            return Get-Item $path
        }
    }
    ```

3. Nahraďte `New-WebDeployPackage` s hello následující kód a nahraďte zástupné symboly hello při vytváření řádku hello `$msbuildCmd`. Tento kód je pro Visual Studio 2015. Pokud používáte Visual Studio 2013, změňte hello **VisualStudioVersion** vlastnost níže příliš`12.0`.

    ```powershell
    function New-WebDeployPackage
    {
        #Write a function toobuild and package your web application
    ```

    toobuild vaší webové aplikace, použijte MsBuild.exe. Nápovědu najdete v tématu Reference k příkazovému řádku nástroje MSBuild v: [http://go.microsoft.com/fwlink/?LinkId=391339](http://go.microsoft.com/fwlink/?LinkId=391339)

    ```powershell
    Write-VerboseWithTime 'Build-WebDeployPackage: Start'

    $msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory

    Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
    ```

### <a name="start-execution-of-hello-build-command"></a>Spustit provádění příkazu hello sestavení

```powershell
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru

if ($job.ExitCode -ne 0) {
    throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain hello project name
$projectName = (Get-Item $ProjectFile).BaseName

#Construct hello path tooweb deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName

#Get hello full path for hello web deploy zip package. This is required for MSDeploy toowork
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir

Write-VerboseWithTime 'Build-WebDeployPackage: End'

return $WebDeployPackage
}
```

1. Volání hello `New-WebDeployPackage` funkce před tento řádek: `$Config = Read-ConfigFile $Configuration` pro webové aplikace nebo `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` pro virtuální počítače.

    ```powershell
    if($ProjectFile)
    {
    $WebDeployPackage = New-WebDeployPackage
    }
    ```

2. Vyvolání hello přizpůsobit skript z příkazového řádku pomocí předávání hello `$Project` argument jako hello následující příklad příkazového řádku.

    ```powershell
    .\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
    -ProjectFile ..\WebApplication5\WebApplication5.csproj `
    -VMPassword @{Name="VMUser";Password="Test.123"} `
    -AllowUntrusted `
    -Verbose
    ```

    tooautomate testování vaší aplikace, přidejte kód příliš`Test-WebApplication`. Být jisti toouncomment hello řádků v **publikovat WebApplication.ps1** kde jsou tyto funkce volána. Pokud nezadáte implementace, můžete ručně vytvořit projekt pomocí sady Visual Studio a potom spusťte hello publikovat tooAzure toopublish skriptu.

## <a name="publishing-function-summary"></a>Publikování souhrn funkcí
tooget nápovědy pro funkce, které můžete použít na příkazovém řádku prostředí Windows PowerShell text hello, použijte příkaz hello `Get-Help function-name`. Nápověda Hello obsahuje příklady a nápovědu parametr. Hello stejný text nápovědy je taky v hello skriptu zdrojové soubory, **AzureWebAppPublishModule.psm1** a **publikovat WebApplication.ps1**. v jazyce Visual Studio jsou lokalizované Hello skriptu a Nápověda.

**AzureWebAppPublishModule**

| Název funkce | Popis |
| --- | --- |
| Přidání azuresqldatabase. |Vytvoří novou databázi Azure SQL. |
| Přidat AzureSQLDatabases |Vytvoří databáze Azure SQL z hodnot hello ve hello konfigurační soubor JSON, který generuje Visual Studio. |
| Přidat AzureVM |Vytvoří virtuální počítač Azure a vrátí hodnotu že Hello URL hello nasazení virtuálních počítačů. Hello funkce nastaví hello požadavků a pak volání hello **New-AzureVM** funkce toocreate (Azure modul) nového virtuálního počítače. |
| Přidat AzureVMEndpoints |Přidá nový virtuální počítač tooa vstupních koncových bodů a vrátí hello virtuální počítač s hello nový koncový bod. |
| Přidat AzureVMStorage |Vytvoří nový účet úložiště Azure v aktuálním předplatném hello. Hello název účtu hello začíná "devtest", za nímž následuje jedinečný alfanumerický řetězec. Funkce Hello vrací hello název nového účtu úložiště hello. Musíte zadat umístění nebo skupina vztahů pro nový účet úložiště hello. |
| Přidat AzureWebsite |Vytvoří web s hello zadaný název a umístění. Tato funkce volá hello **New-AzureWebsite** funkce v hello Azure modulu. Pokud hello předplatné už neobsahuje web se zadaným názvem hello, tato funkce vytvoří hello web a vrátí objekt webu. Funkce `$null`. |
| Zálohování předplatného |Uloží hello aktuální předplatné Azure v hello `$Script:originalSubscription` proměnné v oboru skriptu. Tato funkce uloží aktuální předplatné Azure hello (jak získat `Get-AzureSubscription -Current`) a jeho účet úložiště a hello předplatné, které mění tímto skriptem (uložené v proměnné hello `$UserSpecifiedSubscription`) a jeho účet úložiště, v oboru skriptu. Ukládání hello hodnoty, můžete pomocí funkce, jako například `Restore-Subscription`, toorestore hello původní aktuální předplatné a úložiště toocurrent stav účtu pokud došlo ke změně aktuální stav hello. |
| Najít AzureVM |Získá hello zadaný virtuální počítač Azure. |
| Formát DevTestMessageWithTime |Přidá zprávy tooa hello datum a čas. Tato funkce je určená pro zpráv zapsaných toohello chyba a podrobná datových proudů. |
| Get-AzureSQLDatabaseConnectionString |Sestaví připojovací řetězec tooconnect tooan Azure SQL database. |
| Get-AzureVMStorage |Vrátí hello název hello první účet úložiště se stejným vzor názvu hello "devtest*" (malá a velká písmena) v zadané umístění nebo vztahů skupiny hello. Pokud hello "devtest*" účet úložiště se neshoduje se hello umístění nebo skupina vztahů, funkce hello ji ignoruje. Musíte zadat umístění nebo skupině vztahů. |
| Get-MSDeployCmd |Vrátí příkaz toorun hello MsDeploy.exe nástroj. |
| Nové AzureVMEnvironment |Vyhledá nebo vytvoří virtuální počítač v rámci předplatného hello, který odpovídá hello hodnoty v konfiguračním souboru JSON hello. |
| Publikování WebPackage |Používá MsDeploy.exe a webové publikování balíčku. ZIP souboru toodeploy prostředky tooa webu. Tato funkce negeneruje žádný výstup. V případě selhání hello volání tooMSDeploy.exe hello funkce vyvolá výjimku. tooget více podrobný výstup, použijte hello **-Verbose** možnost. |
| Publikování WebPackageToVM |Ověří hello hodnoty parametrů a pak zavolá hello **publikovat WebPackage** funkce. |
| ConfigFile pro čtení |Ověří hello JSON konfigurační soubor a vrátí hodnotu hash tabulku vybraných hodnot. |
| Obnovení předplatného |Obnoví hello aktuální předplatné toohello původního předplatného. |
| Test AzureModule |Vrátí `$true` Pokud hello nainstalovaná verze modulu Azure 0.7.4 nebo novější. Vrátí `$false` Pokud hello modul není nainstalován nebo je starší verze. Tato funkce nemá žádné parametry. |
| Test AzureModuleVersion |Vrátí `$true` Pokud hello verze hello Azure modulu 0.7.4 nebo novější. Vrátí `$false` Pokud hello modul není nainstalován nebo je starší verze. Tato funkce nemá žádné parametry. |
| Test HttpsUrl |Převede objekt System.Uri tooa vstupní adresy URL hello. Vrátí `$True` Pokud adresa URL hello absolutní a její schéma je protokol https. Vrátí `$false` hello adresa URL je relativní, jeho schématu není HTTPS, nebo hello vstupní řetězec nemůže být převedená tooa adresy URL. |
| Test člena |Vrátí `$true` Pokud vlastnosti nebo metody je členem objektu hello. Jinak vrátí `$false`. |
| Zápis ErrorWithTime |Zapíše chybovou zprávu předponu hello aktuální čas. Tato funkce volá hello **formátu DevTestMessageWithTime** funkce tooprepend hello čas před zápisem hello zpráva toohello chybový proud. |
| Zápis HostWithTime |Zapíše zpráva toohello hostitele program (**Write-Host**) předponu hello aktuální čas. zápis toohello hostitelského programu Hello účinek se liší. Většiny programů tohoto hostitele prostředí Windows PowerShell zápisu tyto zprávy toostandard výstupu. |
| Zápis VerboseWithTime |Zapíše podrobnou zprávu předponu hello aktuální čas. Vzhledem k tomu, že zavolá **Write-Verbose**, uvítací zprávu zobrazí, jenom když hello skript se spustí s hello **podrobné** parametr nebo když text hello **VerbosePreference** je předvoleb Nastavení příliš**pokračovat**. |

**Publikovat webovou aplikaci**

| Název funkce | Popis |
| --- | --- |
| Nové AzureWebApplicationEnvironment |Vytvoří prostředky Azure, jako je web nebo virtuálního počítače. |
| Nové WebDeployPackage |Tato funkce není implementována. Příkazy můžete přidat v této funkce toobuild projektu. |
| Publikování AzureWebApplication |Publikuje tooAzure webové aplikace. |
| Publikovat webovou aplikaci |Vytvoří a nasadí webových aplikací, virtuálních počítačů, databází SQL a účty úložiště pro webový projekt sady Visual Studio. |
| Test-WebApplication |Tato funkce není implementována. Příkazy můžete přidat v této funkce tootest vaší aplikace. |

## <a name="next-steps"></a>Další kroky
Další informace o prostředí PowerShell skriptování čtení [skriptování v prostředí Windows PowerShell](https://technet.microsoft.com/library/bb978526.aspx) a jiné skripty prostředí Azure PowerShell v hello [centra skriptů](https://azure.microsoft.com/documentation/scripts/).
