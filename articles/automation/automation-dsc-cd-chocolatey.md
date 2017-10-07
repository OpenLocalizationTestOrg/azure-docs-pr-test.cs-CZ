---
title: "Automatizace DSC průběžné nasazování pomocí Chocolatey aaaAzure | Microsoft Docs"
description: "DevOps průběžné nasazování pomocí Azure Automation DSC a správce Chocolatey balíčků.  Příklad s kompletní šablonou JSON ARM a zdroj PowerShell."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: c0baa411-eb76-4f91-8d14-68f68b4805b6
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 10/29/2016
ms.author: golive
ms.openlocfilehash: 60af52af5f834fd48e3a0dc4677919397b53f0f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="usage-example-continuous-deployment-toovirtual-machines-using-automation-dsc-and-chocolatey"></a>Příklad použití: TooVirtual průběžné nasazování počítačů pomocí Automation DSC a Chocolatey
V DevOps world existuje mnoho tooassist nástroje s různými body v kanálu průběžnou integraci hello.  Konfigurace Azure Automation žádaný stavu (DSC) je úvodní nové přidání toohello možnosti, které můžete použít DevOps týmy.  Tento článek ukazuje nastavení nahoru průběžné nasazení (CD) pro počítač se systémem Windows.  Můžete snadno rozšířit hello technika tooinclude tolik počítače se systémem Windows v případě potřeby v roli hello (webový server, např.) a z tooadditional zde také role.

![Průběžné nasazování pro virtuální počítače IaaS](./media/automation-dsc-cd-chocolatey/cdforiaasvm.png)

## <a name="at-a-high-level"></a>Na vysoké úrovni
Je celkem chvilku přechodem sem, ale naštěstí možné ho rozdělit do dvou hlavních procesů: 

* Psaní kódu a jeho otestováním, pak vytváření a publikování instalační balíčky pro hlavní verze a podverze systému hello. 
* Vytváření a správa virtuálních počítačů, které budou nainstalovány a spouštění kódu vytvořeného hello v balíčcích hello.  

Jakmile jsou obě tyto klíčové procesy v místě, je balíček, aktualizace hello krátké krok tooautomatically systémem žádné konkrétní virtuální počítače, jako jsou vytvoření a nasazení nových verzí.

## <a name="component-overview"></a>– Přehled komponenty
Balíček správce například [výstižný get](https://en.wikipedia.org/wiki/Advanced_Packaging_Tool) jsou poměrně dobře známé v hello, world Linux, ale není tak většinu v systému Windows hello, world.  [Chocolatey](https://chocolatey.org/) je taková věc a Scott Hanselman [blog](http://www.hanselman.com/blog/IsTheWindowsUserReadyForAptget.aspx) na hello téma je skvělým úvod.  Stručně řečeno Chocolatey vám umožní tooinstall balíčky z centrální úložiště balíčků do systému Windows hello příkazového řádku.  Můžete vytvořit a spravovat vlastní úložiště, a Chocolatey mohou instalovat balíčky z libovolný počet úložiště, které určíte.

Požadovaného konfigurace stavu (DSC) ([přehled](https://technet.microsoft.com/library/dn249912.aspx)) je nástroj prostředí PowerShell, který vám umožní toodeclare hello konfigurace, který chcete pro počítač.  Řekněme například, je možné, "Chci, aby byla nainstalována Chocolatey, chci, aby byla nainstalována služba IIS, má být otevřený port 80, chci, aby byla verze 1.0.0 svého webu."  Hello správce místní konfigurace DSC (LCM) implementuje tuto konfiguraci. DSC pro vyžádání obsahu Server obsahuje úložiště konfigurace pro počítače. Hello LCM na každém počítači pravidelně toosee změnami, pokud jeho konfigurace odpovídá konfiguraci uložené hello. Můžete buď sestavy stavu nebo pokus toobring hello počítač zpět do zarovnání hello uložené konfiguraci. Můžete upravit hello uložené konfigurace na hello vyžádání obsahu server toocause na počítači nebo sadu počítačů toocome do zarovnání s konfigurací hello změnit.

Služby Azure Automation je spravovaná služba ve službě Microsoft Azure, který vám umožní tooautomate různé úkoly pomocí sady runbook, uzly, přihlašovací údaje, prostředky a prostředky, jako je například plány a globální proměnné. Azure Automation DSC rozšiřuje tato automatizace schopností tooinclude DSC prostředí PowerShell nástroje.  Tady je skvělá [přehled](automation-dsc-overview.md).

Prostředek DSC je modul kódu, který má specifické možnosti, jako je například Správa sítě služby Active Directory nebo SQL Server.  Hello Chocolatey prostředek DSC neví, jak tooaccess NuGet serveru (mimo jiné) stáhnout balíčky, instalovat balíčky a tak dále.  Existuje mnoho dalších prostředků DSC v hello [Galerie prostředí PowerShell](http://www.powershellgallery.com/packages?q=dsc+resources&prerelease=&sortOrder=package-title).  Tyto moduly jsou nainstalovány do Azure Automation DSC pro vyžádání obsahu serveru (které jste), je možné použít vaše konfigurace.

Šablony Resource Manageru poskytnout deklarativní způsob generování infrastruktury - věcmi, jako jsou sítě, podsítě, zabezpečení sítě a směrování, zatížení nástroje pro vyrovnávání, síťové adaptéry, virtuální počítače a tak dále.  Tady je [článku](../azure-resource-manager/resource-manager-deployment-model.md) , porovná hello modelu nasazení Resource Manager (deklarativní) s hello Azure Service Management (ASM nebo classic) nasazení modelu (imperativní) a popisuje základní prostředků hello poskytovatelů, výpočetní, úložiště a sítě.

Klíčovou vlastností šablony správce prostředků je jeho tooinstall možnost rozšíření virtuálního počítače do hello virtuálních počítačů, jako je zřízený.  Rozšíření virtuálního počítače má specifické možnosti, například při spuštění vlastního skriptu, instalaci antivirového softwaru nebo spouštění skriptu konfigurace DSC.  Existuje mnoho dalších typů rozšíření virtuálního počítače.

## <a name="quick-trip-around-hello-diagram"></a>Rychlá cesta kolem hello diagram
Postupuje shora hello, zadejte kód, sestavení a testování a pak vytvořit instalační balíček.  Chocolatey může zpracovávat různých typů instalačních balíčků, jako je například MSI, MSU, ZIP.  A máte hello úplné výkon prostředí PowerShell toodo hello vlastní instalace, pokud na Chocolatey nativní funkce nejsou poměrně až tooit.  Uvést hello balíček do někde dosažitelný – úložiště balíčků.  Tento příklad použití používá do veřejné složky v účtu úložiště objektů blob v Azure, ale může být kdekoli.  Chocolatey funguje nativně se servery NuGet a některých jiných pro správu metadata balíčků.  [Tento článek](https://github.com/chocolatey/choco/wiki/How-To-Host-Feed) popisuje možnosti hello.  Tento příklad použití používá NuGet.  Nuspec je metadata o vašich balíčků.  Hello Nuspec jsou "zkompilovat" do na NuPkg a uloženo na serveru NuGet.  Při konfiguraci požadavky balíček podle názvu a odkazuje na NuGet server, získá hello balíček hello Chocolatey prostředek DSC (nyní na hello virtuálních počítačů) a nainstaluje za vás.  Také můžete vyžádat na konkrétní verzi balíčku.

Ve spodní levé části hello hello obrázku je šablonu Azure Resource Manager (ARM).  V tomto příkladu využití hello rozšíření virtuálního počítače zaregistruje hello virtuálních počítačů s hello Server Azure Automation DSC za (tedy vyžádání obsahu server) jako uzel.  Konfigurace Hello je uložená v hello načítacího serveru.  Ve skutečnosti, uloží se dvakrát: jednou jako prostý text a jakmile zkompilován jako soubor MOF (pro ty, které vědět o takové věci.)  Hello MOF hello portálu, je "konfigurace uzlu" (jako názvem na rozdíl od toosimply "konfigurace").  Jeho hello artefaktu, který je spojen s uzlem, tak uzel hello věděli, její konfiguraci.  Níže uvedené podrobnosti ukazují, jak tooassign hello uzlu toohello konfigurace uzlu.

Pravděpodobně již provádíte hello bit hello horní nebo většinu ho.  Vytváření hello nuspec, kompilace a ukládání na serveru NuGet je malý věcí.  A už spravujete virtuální počítače.  Pořízení hello další krok toocontinuous nasazení vyžaduje nastavení (jednou) hello načítacího serveru, registrace uzly s ním (jednou), vytváření a ukládání konfigurace hello existuje (původně).  Potom jako balíčky jsou upgradovány a nasadit toohello úložiště aktualizujte hello konfigurace a konfigurace uzlu v serveru pro vyžádání obsahu hello (opakování podle potřeby).

Pokud nejsou od verze šablony ARM, který je také OK.  Existují toohelp rutin určených prostředí PowerShell zaregistrujte virtuální počítače s hello načítacího serveru a všech hello rest. Další podrobnosti najdete v tématu v tomto článku: [registrace počítačů pro správu Azure Automation DSC.](automation-dsc-onboarding.md)

## <a name="step-1-setting-up-hello-pull-server-and-automation-account"></a>Krok 1: Nastavení účtu pro server a automatizace vyžádání hello
V příkazovém řádku prostředí PowerShell (Add-AzureRmAccount) ověření: (může trvat několik minut hello vyžádání obsahu server je nastavený)

    New-AzureRmResourceGroup –Name MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES
    New-AzureRmAutomationAccount –ResourceGroupName MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES –Name MY-AUTOMATION-ACCOUNT 

Můžete vložit účtu automation do některé z následujících oblastí (neboli umístění) hello: východní USA 2, Jižní střední USA, Virginia nám verze pro státní správu, západní Evropa, jihovýchodní Asie, Japonsko – východ, Indie centrální a Austrálie – jihovýchod, Střední Kanada, Severní Evropa.

## <a name="step-2-vm-extension-tweaks-toohello-arm-template"></a>Krok 2: Rozšíření virtuálního počítače vylepšení toohello šablony ARM
Podrobnosti registrace virtuálních počítačů (pomocí rozšíření virtuálního počítače DSC prostředí PowerShell hello) zadaný v tomto [šablony rychlý start Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/dsc-extension-azure-automation-pullserver).  Tento krok zaregistruje nový virtuální počítač se serverem pro vyžádání obsahu hello v hello seznam uzlů DSC.  Součástí této registrace je zadání hello uzlu Konfigurace toobe použít toohello uzlu.  Tato konfigurace uzlu nemá tooexist ještě hello vyžádání obsahu serveru, tak, aby byl OK, který krok 4 je, kde to se provádí pro hello poprvé.  Zde v kroku 2 je však nutné toohave rozhodla hello název uzlu hello a název hello hello konfigurace.  V tomto příkladu využití hello uzel je 'isvbox' a 'ISVBoxConfig' je konfigurace hello.  Název konfigurace uzlu hello (zadaný v DeploymentTemplate.json toobe) je proto 'ISVBoxConfig.isvbox'.  

## <a name="step-3-adding-required-dsc-resources-toohello-pull-server"></a>Krok 3: Přidání server vyžaduje DSC prostředky toohello za
Hello Galerie prostředí PowerShell je prostředky DSC instrumentovaného tooinstall do účtu Azure Automation.  Přejděte toohello prostředků a klikněte na tlačítko "Nasadit tooAzure automatizace" hello.

![Příklad galerie prostředí PowerShell](./media/automation-dsc-cd-chocolatey/xNetworking.PNG)

Jiné technika nedávno přidali, že toohello portálu Azure vám umožní toopull v nové nebo moduly aktualizovat existující. Proklikejte se prostřednictvím hello účet Automation prostředek, hello prostředky dlaždice a nakonec hello moduly dlaždice.  Ikona procházet galerii Hello vám umožní toosee hello seznam modulů v galerii hello, Rozbalit podrobnosti a nakonec naimportovat do vašeho účtu Automation. To je skvělým způsobem tookeep moduly až toodate z tootime čas. A funkce importu hello zkontroluje závislosti s další moduly tooensure, nic získává synchronizován.

Nebo je ruční metodu hello.  struktura složek Hello modulu integrace prostředí PowerShell pro počítač se systémem Windows je jen málo liší od očekávaného hello Azure Automation struktura složek hello.  To vyžaduje trochu postupně je upravujte z vaší strany.  Ale není pevný a se provádí jenom jednou za prostředků (Pokud chcete, aby tooupgrade ho v budoucnosti.)  Další informace o vytváření moduly integrace prostředí PowerShell, najdete v tomto článku: [vytváření modulů integrace pro Azure Automation.](https://azure.microsoft.com/blog/authoring-integration-modules-for-azure-automation/)

* Nainstalujte hello modul, který je nutné na pracovní stanici, následujícím způsobem:
  * Nainstalujte [Windows Management Framework, verze 5](http://aka.ms/wmf5latest) (nejsou potřeba pro Windows 10)
  * `Install-Module –Name MODULE-NAME`< – získá hello modulu z hello Galerie prostředí PowerShell 
* Kopírovat hello modulu složku z `c:\Program Files\WindowsPowerShell\Modules\MODULE-NAME` tooa dočasnou složku 
* Ukázky a dokumentace odstranit z hlavní složky hello 
* Hlavní složky hello ZIP pojmenování souboru ZIP hello přesně stejný hello jako složka hello 
* Soubor ZIP hello uvést do dosažitelný umístění HTTP, jako je například úložiště objektů blob v účtu úložiště Azure.
* Spusťte tuto prostředí PowerShell:
  
      New-AzureRmAutomationModule `
          -ResourceGroupName MY-AUTOMATION-RG -AutomationAccountName MY-AUTOMATION-ACCOUNT `
          -Name MODULE-NAME –ContentLink "https://STORAGE-URI/CONTAINERNAME/MODULE-NAME.zip"

Příklad Hello zahrnuté provádí tyto kroky pro cChoco a xNetworking. V tématu hello [poznámky](#notes) pro zvláštní zpracování pro cChoco.

## <a name="step-4-adding-hello-node-configuration-toohello-pull-server"></a>Krok 4: Přidání hello uzlu Konfigurace toohello načítacího serveru
Není co speciální o hello poprvé importovat konfiguraci na vyžádání obsahu server hello a kompilaci.  Všechny následné importu/zkompiluje z hello stejné konfigurace vzhledu přesně hello stejné.  Pokaždé, když vaše balíček aktualizace a potřebovat toopush ho tooproduction je provést tento krok po zajištění hello konfigurační soubor je správná – včetně hello novou verzi vašeho balíčku.  Tady je hello konfigurační soubor a prostředí PowerShell:

ISVBoxConfig.ps1:

    Configuration ISVBoxConfig 
    { 
        Import-DscResource -ModuleName cChoco 
        Import-DscResource -ModuleName xNetworking

        Node "isvbox" {   

            cChocoInstaller installChoco 
            { 
                InstallDir = "C:\choco" 
            }

            WindowsFeature installIIS 
            { 
                Ensure="Present" 
                Name="Web-Server" 
            }

            xFirewall WebFirewallRule 
            { 
                Direction = "Inbound" 
                Name = "Web-Server-TCP-In" 
                DisplayName = "Web Server (TCP-In)" 
                Description = "IIS allow incoming web site traffic." 
                DisplayGroup = "IIS Incoming Traffic" 
                State = "Enabled" 
                Access = "Allow" 
                Protocol = "TCP" 
                LocalPort = "80" 
                Ensure = "Present" 
            }

            cChocoPackageInstaller trivialWeb 
            {            
                Name = "trivialweb" 
                Version = "1.0.0" 
                Source = “MY-NUGET-V2-SERVER-ADDRESS” 
                DependsOn = "[cChocoInstaller]installChoco", 
                "[WindowsFeature]installIIS" 
            } 
        }    
    }

Nový-ConfigurationScript.ps1:

    Import-AzureRmAutomationDscConfiguration ` 
        -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT ` 
        -SourcePath C:\temp\AzureAutomationDsc\ISVBoxConfig.ps1 ` 
        -Published –Force

    $jobData = Start-AzureRmAutomationDscCompilationJob ` 
        -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT ` 
        -ConfigurationName ISVBoxConfig 

    $compilationJobId = $jobData.Id

    Get-AzureRmAutomationDscCompilationJob ` 
        -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT ` 
        -Id $compilationJobId

Tyto kroky výsledek v konfiguraci se nový uzel s názvem "ISVBoxConfig.isvbox" je umístěn na serveru vyžádané replikace hello.  Název konfigurace uzlu Hello je vytvořená jako "configurationName.nodeName".

## <a name="step-5-creating-and-maintaining-package-metadata"></a>Krok 5: Vytvoření a údržbu metadata balíčků
Pro každý balíček, který vložíte do úložiště balíčků hello budete potřebovat soubor nuspec, s popisem.  Tento soubor nuspec, je nutné zkompilovat a uložit na vašem serveru NuGet. Tento proces je popsán [zde](http://docs.nuget.org/create/creating-and-publishing-a-package).  MyGet.org slouží jako NuGet server.  Prodeje této služby, ale mají starter SKU, které je právě volné.  V NuGet.org najdete pokyny k instalaci NuGet server pro soukromé balíčky.

## <a name="step-6-tying-it-all-together"></a>Krok 6: Příkazů je všechny najednou
Pokaždé, když na verzi předá QA a je schválená pro nasazení, hello balíček je vytvořen, nuspec a nupkg aktualizovat a nasadit toohello NuGet server.  Kromě toho konfigurace hello (kroku 4 výše) musí být aktualizované tooagree s hello nové číslo verze.  Musí být odeslána toohello načítacího serveru a zkompilovat.  Od tohoto okamžiku je až toohello virtuálních počítačů, které závisí na tuto aktualizaci konfigurace toopull hello a nainstalujte ji.  Všechny tyto aktualizace jsou jednoduché - právě řádku nebo dva prostředí PowerShell.  V případě hello Visual Studio Team Services některé z nich zapouzdřený v úlohách sestavení, které může být zřetězen dohromady v sestavení.  To [článku](https://www.visualstudio.com/en-us/docs/alm-devops-feature-index#continuous-delivery) obsahuje další podrobnosti.  To [úložiště GitHub](https://github.com/Microsoft/vso-agent-tasks) podrobnosti hello různé úlohy dostupné sestavení.

## <a name="notes"></a>Poznámky
Tento příklad použití začíná virtuální počítač z bitové kopie obecné Windows Server 2012 R2 z Galerie Azure hello.  Můžete spustit z jakékoli uložené image a pak upravit odtud s konfigurací DSC hello.  Změna konfigurace, která je zaručená do bitové kopie, je však mnohem složitější než dynamicky aktualizuje hello se konfigurace pomocí DSC.

Nemáte toouse šablony ARM a toouse rozšíření virtuálního počítače hello Tato technika pomocí vašich virtuálních počítačů.  A virtuální počítače nemáte toobe na Azure toobe pod správou disku CD.  Všechny možnosti, které je třeba je nainstalovat tento Chocolatey a hello LCM nakonfigurované na hello virtuálních počítačů tak bude vědět, kde hello vyžádání obsahu server je.  

Samozřejmě pokud aktualizujete balíček ve virtuálním počítači, který je v produkčním prostředí, je potřeba tootake tohoto virtuálního počítače mimo otočení při instalaci aktualizace hello.  Tento postup se výrazně liší.  Například v případě virtuálních počítačů za pro vyrovnávání zatížení Azure, můžete přidat vlastní Probe.  Při aktualizaci hello virtuálních počítačů, mají koncový bod testu hello vrátit 400.  Hello TweakUI nezbytné toocause tato změna může být v konfiguraci, můžete hello TweakUI tooswitch zpátky tooreturning 200 až po dokončení aktualizace hello.

Úplný zdrojový v tomto příkladu využití je v [tento projekt sady Visual Studio](https://github.com/sebastus/ARM/tree/master/CDIaaSVM) na Githubu.

## <a name="related-articles"></a>Související články
* [Přehled služby Azure Automation DSC](automation-dsc-overview.md)
* [Rutiny Azure Automation DSC](https://msdn.microsoft.com/library/mt244122.aspx)
* [Registrace počítačů pro správu Azure Automation DSC.](automation-dsc-onboarding.md)

