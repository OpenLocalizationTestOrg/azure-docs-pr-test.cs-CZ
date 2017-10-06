---
title: "aaaContinuous doručení pro cloudové služby se sadou TFS v Azure | Microsoft Docs"
description: "Zjistěte, jak tooset až nastavené průběžné doručování pro Azure cloudových aplikací. Ukázky kódu pro MSBuild příkazy příkazového řádku a skripty prostředí PowerShell."
services: cloud-services
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 4f3c93c6-5c82-4779-9d19-7404a01e82ca
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/12/2017
ms.author: kraigb
ms.openlocfilehash: c0e5e72ffbd3c05b84ce1733068e92c528bcc4b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-for-cloud-services-in-azure"></a>Nastavené průběžné doručování pro cloudové služby v Azure
Hello proces popsaný v tomto článku se dozvíte, jak tooset až nastavené průběžné doručování Azure cloudových aplikací. Tento proces umožňuje automatické vytvoření balíčků a nasadit balíček tooAzure hello po každé změnami kódu. Hello procesu sestavení balíčku popsané v tomto článku je ekvivalentní toohello **balíček** příkaz v sadě Visual Studio a publikování kroky jsou ekvivalentní toohello **publikovat** příkazu v sadě Visual Studio.
Hello článek obsahuje hello metody toocreate sestavení serveru by používat s příkazy příkazového řádku nástroje MSBuild a skriptů prostředí Windows PowerShell a také ukazuje, jak nakonfigurovat toooptionally Visual Studio Team Foundation Server – definice sestavení Team toouse hello MSBuild příkazy a skripty prostředí PowerShell. proces Hello je přizpůsobitelné prostředí pro sestavení a prostředí Azure cíl.

Můžete také použít Visual Studio Team Services, verzi sady TFS, která je hostovaná v Azure, toodo to snadněji. 

Než začnete, byste měli publikovat aplikace ze sady Visual Studio.
To zajistí, že jsou všechny prostředky hello dostupné a inicializovaného když pokusí tooautomate hello publikace procesu.

## <a name="1-configure-hello-build-server"></a>1: Konfigurace hello serveru sestavení
Než vytvoříte balíček Azure pomocí nástroje MSBuild je na serveru sestavení hello nainstalovat hello požadované softwaru a nástroje.

Visual Studio není požadovaná toobe nainstalován na serveru sestavení hello. Pokud chcete službu sestavení Team Foundation toomanage toouse sestavení serveru, postupujte podle pokynů hello v hello [služby sestavení Team Foundation] [ Team Foundation Build Service] dokumentaci.

1. Na serveru hello sestavení, nainstalujte hello [rozhraní .NET Framework 4.5.2][.NET Framework 4.5.2], což zahrnuje MSBuild.
2. Nainstalujte nejnovější hello [nástroje pro tvorbu Azure pro .NET](https://azure.microsoft.com/develop/net/).
3. Nainstalujte hello [knihovny Azure pro .NET](http://go.microsoft.com/fwlink/?LinkId=623519).
4. Kopírovat soubor Microsoft.WebApplication.targets hello z toohello instalace sady Visual Studio vytvořit server.

   Na počítači s nainstalovanou sadu Visual Studio, tento soubor je umístěný v adresáři hello C:\\Program Files(x86)\\MSBuild\\Microsoft\\Visual Studio\\v14.0\\WebApplications. Měli byste ho zkopírovat toohello stejný adresář na serveru sestavení hello.
5. Nainstalujte hello [nástroje Azure pro sadu Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).

## <a name="2-build-a-package-using-msbuild-commands"></a>2: vytvoření balíčku pomocí příkazů nástroje MSBuild
Tato část popisuje, jak tooconstruct MSBuild příkaz, který vytvoří balíček Azure. Tento krok lze spusťte na hello sestavení serveru tooverify že všechno správně nakonfigurován a že hello MSBuild příkaz provést požadované ho toodo. Jak je popsáno v další části hello, můžete přidat buď tento příkazový řádek tooexisting skripty sestavení na hello sestavení serveru nebo můžete použít v definici sestavení TFS hello příkazového řádku. Další informace o parametry příkazového řádku a nástroje MSBuild najdete v tématu [příkazového řádku MSBuild – Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

1. Pokud na serveru sestavení hello je nainstalovaná sada Visual Studio, vyhledejte a vyberte **Visual Studio – příkazový řádek** v hello **nástroje sady Visual Studio** složky v systému Windows.

   Pokud Visual Studio není nainstalována na serveru hello sestavení, otevřete příkazový řádek a ujistěte se, že MSBuild.exe je dostupný v cestě. MSBuild se instaluje s hello rozhraní .NET Framework v hello cesta % WINDIR %\\Microsoft.NET\\Framework\\*verze*. Například proměnné prostředí PATH toohello MSBuild.exe přidat v případě, že máte nainstalované rozhraní .NET Framework 4, zadejte následující příkaz na příkazovém řádku hello hello:

       set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"
2. Na příkazovém řádku hello přejděte toohello složce obsahující soubor projektu Azure, které chcete toobuild.
3. Spusťte nástroje MSBuild s hello/target: možnost jako hello následující ukázka publikování:

       MSBuild /target:Publish

   Tuto možnost lze zkrátit na /t: publikování. možnost /t:Publish Hello v nástroji MSBuild Nezaměňovat s příkazy publikovat hello k dispozici v sadě Visual Studio až budete mít hello instalace Azure SDK. Hello /t: možnost publikování jen sestavení hello Azure balíčky. Stejně jako hello publikovat příkazy v sadě Visual Studio Nenasazuje hello balíčky.

   Volitelně můžete zadat název projektu hello jako parametr MSBuild. Pokud není zadaný, použije se hello aktuální adresář. Další informace o možnostech příkazového řádku nástroje MSBuild najdete v tématu [příkazového řádku MSBuild – Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).
4. Vyhledejte výstup hello. Ve výchozím nastavení, tento příkaz vytvoří adresář vztah toohello kořenové složky pro projekt hello, jako například *ProjectDir*\\bin\\*konfigurace* \\ app.publish\\. Při sestavování projektu Azure generovat ze dvou souborů, samotný soubor balíčku hello a hello doplňujícími konfigurační soubor:

   * Project.cspkg
   * Objekt ServiceConfiguration. *TargetProfile*.cscfg

   Ve výchozím nastavení každý projekt, Azure zahrnuje jednu konfigurační soubor služby (soubor .cscfg) pro místní sestavení (ladění) a druhou pro sestavení cloudu (pracovním nebo produkčním), ale můžete přidat nebo odebrat soubory konfigurace služby podle potřeby. Když vytvoříte balíček Visual Studia, zobrazí se výzva které služby konfigurační soubor tooinclude spolu s hello balíčku.
5. Zadejte konfigurační soubor služby hello. Když vytvoříte balíček pomocí nástroje MSBuild, hello místní služby konfigurační soubor je zahrnuté ve výchozím nastavení. tooinclude konfigurační soubor jinou službu, nastavte vlastnost TargetProfile hello MSBuild příkazu, stejně jako hello následující ukázka:

       MSBuild /t:Publish /p:TargetProfile=Cloud
6. Zadejte umístění hello pro výstup hello. Cesta hello sady pomocí /p:PublishDir =*Directory* \\ možnost, včetně hello koncové oddělovače zpětné lomítko, stejně jako hello následující ukázka:

       MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

   Jakmile jste sestavený a otestovat příslušné nástroje MSBuild příkazového řádku toobuild vašich projektů a zkombinovat do balíčku s Azure, můžete přidat skripty sestavení tooyour Tento příkazový řádek. Pokud váš server sestavení používá vlastní skripty, tento proces bude záviset na jaké jsou specifikace svůj vlastní sestavovací proces. Pokud používáte jako prostředí pro sestavování sady TFS, můžete provést hello pokyny v hello další krok tooadd hello balíček Azure sestavení tooyour procesu sestavení.

## <a name="3-build-a-package-using-tfs-team-build"></a>3: vytvoření balíčku pomocí TFS Team Build
Máte-li nastavit jako řadič sestavení a hello sestavení serveru Team Foundation Server (TFS) nastavit jako počítač sestavení sady TFS a potom automatizované sestavení Volitelně můžete nastavit pro balíčku Azure. Informace o tom, jak tooset nahoru a použít Team Foundation server jako systém sestavení, najdete v části [škálování systém sestavení][Scale out your build system]. Zejména následující postup předpokládá, že jste nakonfigurovali vašem serveru sestavení, jak je popsáno v [nasadit a nakonfigurovat server sestavení][Deploy and configure a build server], a že jste vytvořili týmového projektu, vytvoření cloudu projekt služby v hello týmového projektu.

tooconfigure TFS toobuild Azure balíčky, proveďte následující kroky hello:

1. V sadě Visual Studio ve svém vývojovém počítači, v nabídce zobrazení hello zvolte **Team Explorer**, nebo zvolte Ctrl +\\, Ctrl + M. V okně Team Explorer rozbalte hello **sestavení** uzlu, nebo zvolte hello **sestavení** stránce a vyberte **novou definici sestavení**.

   ![Nová možnost definice sestavení][0]
2. Zvolte hello **aktivační událost** a zadejte hello požadovaných podmínek, když chcete hello toobe balíček vytvořen. Můžete například zadat **průběžnou integraci** dojde k toobuild hello balíček vždy, když zdroj řídí změnami.
3. Zvolte hello **nastavení zdroje** kartě a zajistěte, aby složky projektu je uvedena v hello **zdrojové složky řízení** sloupce, a stav hello **Active**.
4. Zvolte hello **sestavení výchozí** a v kontroleru buildu, ověřte název hello hello sestavení serveru.  Také vyberte možnost hello **kopie sestavení výstupu toohello následující vyřadit složky** a zadejte umístění rozevírací hello potřeby.
5. Zvolte hello **proces** kartě. Na kartě hello procesu, zvolte hello výchozí šablony, v části **sestavení**, zvolte projekt hello, pokud není vybrána a rozbalte hello **Upřesnit** část v hello **sestavení**části hello mřížky.
6. Zvolte **argumenty MSBuild**a nastavte hello příslušné nástroje MSBuild argumenty příkazového řádku, jak je popsáno v kroku 2 výše. Zadejte například **/t: publikování /p:PublishDir =\\\\myserver\\zahodí\\**  toobuild hello balíček balíčku a zkopírujte soubory toohello umístění \\ \\myserver\\zahodí\\:

   ![Argumenty MSBuild][2]

   > [!NOTE]
   > Kopírování souborů tooa hello veřejné složky umožňuje snazší toomanually nasadit balíčky hello z vývojovém počítači.
7. Testování úspěšnosti hello kroku sestavení kontrolou v projektu tooyour změnu nebo fronty, nové sestavení. Klikněte pravým tlačítkem na tooqueue až nového sestavení, v nástroji Team Explorer **všechny definice sestavení,** a potom zvolte **fronty nové sestavení**.

## <a name="4-publish-a-package-using-a-powershell-script"></a>4: publikování balíčku pomocí skriptu prostředí PowerShell
Tato část popisuje, jak tooconstruct skript prostředí Windows PowerShell, která bude publikovat balíček aplikace hello cloudu výstup tooAzure pomocí volitelné parametry. Tento skript je možné volat po kroku hello sestavení ve vaší vlastní sestavovací automation. Také může být volána z aktivit pracovního postupu šablonu procesu v sestavení Team TFS s Visual Studio.

1. Nainstalujte hello [rutin prostředí Azure PowerShell] [ Azure PowerShell cmdlets] (v0.6.1 nebo vyšší).
   Během fáze instalace hello rutiny vyberte tooinstall jako modul snap-in. Všimněte si, že tato oficiálně podporované verze nahrazuje starší verze hello nabízeným přes webu CodePlex, i když bylo předchozí verze hello číslované 2.x.x.
2. Otevřete prostředí Azure PowerShell pomocí hello nabídky Start nebo úvodní stránky. Pokud spustíte tímto způsobem, budou načteny hello rutin prostředí Azure PowerShell.
3. Na příkazovém řádku prostředí PowerShell text hello, ověřte, zda jsou načteny hello rutiny prostředí PowerShell zadáním příkazu částečné hello `Get-Azure` a stisknutím klávesy tabulátor pro dokončování hello.

   Když stisknete klávesy Tab hello opakovaně, měli byste vidět různé příkazy prostředí Azure PowerShell.
4. Ověřte, že můžete importovat informace o vašem předplatném ze souboru .publishsettings hello připojit tooyour předplatného Azure.

   `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

   Potom zadejte příkaz hello

   `Get-AzureSubscription`

   Ukazuje to informace o vašem předplatném. Ověřte, že je vše v pořádku.
5. Uložení šablony skriptu hello uvedených v hello konci tohoto článku do složky skriptů jako c:\\skripty\\WindowsAzure\\**PublishCloudService.ps1**.
6. Projděte si část parametry hello hello skriptu. Přidat nebo upravit všechny výchozí hodnoty. Tyto hodnoty mohou být přepsány vždy explicitní parametrů.
7. Ujistěte se, nejsou platná cloudové služby a účty úložiště vytvořené v rámci vašeho předplatného, který může být cílem hello publikování skriptu. Účet úložiště (úložiště objektů blob) bude použité tooupload a dočasně uložit hello nasazení balíčku a konfigurační soubor je při vytváření nasazení.

   * toocreate novou cloudovou službu, můžete volat tento skript nebo použijte hello [portál Azure](https://portal.azure.com). Název cloudové služby Hello se použije jako předponu v platný plně kvalifikovaný název domény, a proto musí být jedinečný.

         New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
   * toocreate nový účet úložiště, můžete volat tento skript nebo použijte hello [portál Azure](https://portal.azure.com). název účtu úložiště Hello se použije jako předponu v platný plně kvalifikovaný název domény, a proto musí být jedinečný. Můžete se pokusit hello stejný název jako cloudová služba.

         New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
8. Volání skriptu hello přímo z prostředí Azure PowerShell nebo propojit se tento skript tooyour hostitele sestavení automatizace toooccur po sestavení balíčku hello.

   > [!IMPORTANT]
   > Hello skript bude vždy odstranit nebo nahradit existující nasazení ve výchozím nastavení, pokud jsou zjišťovány. Toto je nutná pro povolení průběžné doručování z automatizace, kde je možné použít bez vyzvání uživatele.
   >
   >

   **Příklad scénáře 1:** toohello průběžné nasazování pracovní prostředí služby:

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   To je obvykle následuje test spusťte ověření a prohození virtuálních IP adres. swap Hello VIP, můžete to udělat pomocí hello [portál Azure](https://portal.azure.com) nebo pomocí rutiny hello přesunutí nasazení.

   **Příklad scénáře 2:** průběžné nasazování toohello provozním prostředí služby vyhrazené testu

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   **Vzdálená plocha:**

   Pokud je povoleno vzdálené plochy v projektu Azure budete potřebovat další jednorázové kroky tooperform tooensure hello je správný certifikát cloudové služby nahrán tooall cloudové služby, která je cílem tohoto skriptu.

   Vyhledejte hodnoty kryptografického otisku certifikátu hello očekávanou role. Kryptografický otisk hodnoty jsou viditelné v části certifikáty hello souboru konfigurace cloudu (tj. ServiceConfiguration.Cloud.cscfg). Je také v dialogovém okně hello konfigurace vzdálené plochy v sadě Visual Studio viditelné zobrazit možnosti a zobrazení hello výběru certifikátu.

       <Certificates>
             <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
       </Certificates>

   Nahrání certifikátů vzdálené plochy jako krok jednorázové instalace pomocí následující rutiny skriptu hello:

       Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

   Například:

       Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

   Případně můžete exportovat soubor certifikátu PFX hello s privátní klíč a nahrávání certifikátů tooeach cílový cloud service pomocí [portál Azure](https://portal.azure.com).

   <!---
   Fixing broken links for Azure content migration from ACOM tooDOCS. I'm unable toofind a replacement links, so I'm commenting out this reference for now. hello author can investigate in hello future. "Read hello following article toolearn more: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx.
   -->
   **Upgrade a nasazení. Odstranění nasazení -\> nové nasazení**

   Hello skript bude ve výchozím nastavení provádět nasazení upgradu ($enableDeploymentUpgrade = 1) Pokud žádný parametr se předává v nebo hodnota 1, je předaná explicitně. Pro jeden instance to nabízí výhodu v podobě trvá kratší dobu, než úplné nasazení. Pro instance, které vyžadují vysokou dostupnost, to také obsahuje výhod hello ponechat některé instancí spuštěných zatímco jiné jsou upgradovány (proti vaší doméně aktualizace), plus vaše virtuální IP adresy se neodstraní.

   Nasazení upgradu můžete zakázat v hello skriptu ($enableDeploymentUpgrade = 0) nebo pomocí předání *- enableDeploymentUpgrade 0* jako parametr, který mění skriptu chování toofirst odstranit všechna stávající nasazení a pak vytvořte nové nasazení.

   > [!IMPORTANT]
   > Hello skript bude vždy odstranit nebo nahradit existující nasazení ve výchozím nastavení, pokud jsou zjišťovány. Toto je nutná pro povolení nastavené průběžné doručování ze služby automation, kde je možné použít bez vyzvání uživatele / – operátor.
   >
   >

## <a name="5-publish-a-package-using-tfs-team-build"></a>5: publikování balíčku pomocí TFS Team Build
Tento volitelný krok připojí TFS Team Build toohello skript vytvořený v kroku 4, která zpracovává publikování tooAzure sestavení balíčku hello. To má za následek změny hello používá svou definici sestavení tak, aby běžel aktivitu publikovat na konci hello hello pracovní postup šablony procesu. Hello publikovat aktivita provede předávání v parametrech ze sestavení hello příkazu prostředí PowerShell. Výstup hello MSBuild cílí a publikovat skript bude přesměrovat do výstupu standardní sestavení hello.

1. Upravit hello sestavení definice zodpovědná za průběžné nasazování.
2. Vyberte hello **proces** kartě.
3. Postupujte podle [tyto pokyny](http://msdn.microsoft.com/library/dd647551.aspx) tooadd projektu aktivity pro hello šablony procesu sestavení, stáhnout výchozí šablonu pro hello, přidejte ho do projektu hello a vrácení se změnami. Zadejte nový název, jako je například AzureBuildProcessTemplate šablony procesu sestavení hello.
4. Vrátí toohello **proces** kartě a používat **zobrazit podrobnosti** tooshow seznam šablony procesu sestavení k dispozici. Zvolte hello **nový...**  tlačítko a přejděte toohello projektu, stačí přidat a vrátit se změnami. Vyhledejte hello šablonu, kterou jste právě vytvořili a zvolte **OK**.
5. Otevřete hello vybrané šablony procesu pro úpravy. Můžete otevřít přímo v Návrháři pracovních postupů hello nebo v toowork editor XML hello s hello XAML.
6. Přidejte následující seznam nových argumentů jako samostatné řádku položky v kartě argumenty hello Návrháře pracovního postupu hello hello. Všechny argumenty by měl mít směr = v a typ = řetězec. Budou tyto parametry použité tooflow z definice sestavení hello do hello pracovního postupu, který pak použít toocall hello get publikování skriptu.

       SubscriptionName
       StorageAccountName
       CloudConfigLocation
       PackageLocation
       Environment
       SubscriptionDataFileLocation
       PublishScriptLocation
       ServiceName

   ![Seznam argumentů][3]

   Hello odpovídající XAML vypadá takto:

       <Activity  _ />
         <x:Members>
           <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
           <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
           <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
           <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
           <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
           <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
           <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
           <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
           <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
           <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
           <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
           <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
           <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
           <x:Property Name="GetVersion" Type="InArgument(x:String)" />
           <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
           <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
           <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
           <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
           <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
           <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
           <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
           <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
           <x:Property Name="Environment" Type="InArgument(x:String)" />
           <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
           <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
           <x:Property Name="ServiceName" Type="InArgument(x:String)" />
         </x:Members>

         <this:Process.MSBuildArguments>
7. Přidejte nové pořadí na konci hello spustit na agenta:

   1. Začněte přidáním toocheck Pokud příkaz aktivity pro soubor platný skriptu. Nastavte hodnotu toothis hello podmínku:

          Not String.IsNullOrEmpty(PublishScriptLocation)
   2. Hello pak případ hello příkaz If přidejte nová aktivita pořadí. Sada hello zobrazovaný název too'Start publikovat.
   3. S hello počáteční publikování pořadí při výběru přidejte jako samostatné položky řádek na kartě proměnné hello Návrháře pracovního postupu v následujícím seznamu nové proměnné. Všechny proměnné, měl by být proměnné typ = řetězec a obor = počáteční publikovat. Budou tyto parametry použité tooflow z definice sestavení hello do pracovního postupu, který pak použít toocall hello get publikování skriptu.

      * SubscriptionDataFilePath typu řetězec.
      * PublishScriptFilePath typu řetězec.

        ![Nové proměnné][4]
   4. Pokud používáte TFS 2012 nebo starší, přidat aktivitu ConvertWorkspaceItem na začátku hello hello nové pořadí. Pokud používáte sady TFS 2013 nebo novější, přidejte aktivitu GetLocalPath na začátku hello hello nové pořadí. Pro ConvertWorkspaceItem, nastavte vlastnosti hello následujícím způsobem: směr = ServerToLocal, DisplayName = název souboru skriptu publikovat převést, vstup = PublishScriptLocation, výsledek = PublishScriptFilePath, pracovní prostor = 'Prostoru'. Pro aktivitu GetLocalPath nastavit hello vlastnost IncomingPath too'PublishScriptLocation', a výsledek too'PublishScriptFilePath hello ". Tato aktivita převede hello cesta toohello publikování skript z umístění serveru TFS (pokud existuje) tooa disků na úrovni standard místní cesta.
   5. Pokud používáte TFS 2012 nebo starší, přidejte další aktivitu ConvertWorkspaceItem na konci hello hello nové pořadí. Směr ServerToLocal, DisplayName = = převést předplatné filename, vstup = SubscriptionDataFileLocation, výsledek = SubscriptionDataFilePath, pracovní prostor = 'Prostoru'. Pokud používáte sady TFS 2013 nebo novější, přidejte další GetLocalPath. IncomingPath = 'SubscriptionDataFileLocation' a výsledek = "SubscriptionDataFilePath."
   6. Přidejte aktivitu, InvokeProcess na konci hello hello nové pořadí.
      Toto volání aktivity PowerShell.exe s argumenty hello předaná v hello definice sestavení.

      + Argumenty = String.Format ("-""{0}" "- serviceName {1} - storageAccountName {2} - packageLocation""{3}" "– cloudConfigLocation""{4}" "– subscriptionDataFile""{5}" "- selectedSubscription {6} souboru-prostředí""{7}" "", PublishScriptFilePath, ServiceName, StorageAccountName, PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, Název_předplatného, prostředí)
      + DisplayName = Execute publikování skriptu
      + Název souboru = "PowerShell" (včetně uvozovek hello)
      + OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)
   7. V hello **zpracování standardní výstup** část textového InvokeProcess, nastavte too'data hodnota textbox hello ". Toto je proměnné toostore hello standardní výstupní data.
   8. Přidat aktivitu WriteBuildMessage pod hello **zpracování standardní výstup** části. Nastavit hello důležitosti = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' a hello zpráva = "data". Tím se zajistí, že hello standardní výstup skriptu získat zapíšou toohello výstupu sestavení.
   9. V hello **zpracovávat chyby výstupu** část textového InvokeProcess, nastavte too'data hodnota textbox hello ". Toto je dat proměnné toostore hello standardní chyba.
   10. Přidat aktivitu WriteBuildError pod hello **zpracovávat chyby výstupu** části. Nastavit hello zpráva = "data". Tím se zajistí, že hello standard chyby skriptu hello získat zapíšou toohello sestavení chyby výstupu.
   11. Opravte všechny chyby, indikován blue vykřičníku značky. Pozastavte ukazatel myši nad tooget vykřičníku značky nápovědu o chybě hello. Uložení pracovního postupu hello zrušte chyby.

   Konečný výsledek Hello hello publikovat pracovní postup, který aktivity bude vypadat například takto v Návrháři hello:

   ![Aktivity pracovního postupu][5]

   Konečný výsledek Hello hello publikovat pracovní postup, který aktivity bude vypadat například takto v jazyce XAML:

       <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
           <If.Then>
             <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
               <Sequence.Variables>
                 <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                 <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
               </Sequence.Variables>
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
               <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                 <mtbwa:InvokeProcess.ErrorDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.ErrorDataReceived>
                 <mtbwa:InvokeProcess.OutputDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.OutputDataReceived>
               </mtbwa:InvokeProcess>
             </Sequence>
           </If.Then>
         </If>
       </Sequence>
8. Uložte hello pracovní postup šablony procesu sestavení a kontrola v tomto souboru.
9. Upravit definici sestavení hello (zavřete ho Pokud už je otevřený) a vyberte hello **nový** tlačítko, pokud ještě nevidíte hello nové šablony v seznamu hello šablon procesů.
10. Nastavte hodnoty hello parametr vlastností v hello různé části takto:

    1. CloudConfigLocation = "c:\\zahodí\\app.publish\\ServiceConfiguration.Cloud.cscfg' *tato hodnota se odvozuje od: ($PublishDir)ServiceConfiguration.Cloud.cscfg*
    2. PackageLocation = "c:\\zahodí\\app.publish\\ContactManager.Azure.cspkg' *tato hodnota se odvozuje od: ($PublishDir)($ProjectName) .cspkg*
    3. PublishScriptLocation = "c:\\skripty\\WindowsAzure\\PublishCloudService.ps1.
    4. ServiceName = 'mycloudservicename' *použití hello odpovídající název cloudové služby zde*
    5. Prostředí = "pracovní.
    6. StorageAccountName = 'mystorageaccountname' *název účtu úložiště odpovídající pomocí hello sem*
    7. SubscriptionDataFileLocation = "c:\\skripty\\WindowsAzure\\Subscription.xml.
    8. Název_předplatného = "default"

    ![Vlastnost hodnoty parametru][6]
11. Uložte změny toohello hello definice sestavení.
12. Fronta sestavení tooexecute obě hello balíček sestavení a publikování. Pokud máte aktivační událost nastavit tooContinuous integrace, provede toto chování na každý vrácení se změnami.

### <a name="publishcloudserviceps1-script-template"></a>Šablona PublishCloudService.ps1 skriptu
```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy too$servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according too$alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress tooactivity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>Další kroky
tooenable vzdálené ladění při použití nastavené průběžné doručování, najdete v části [povolení vzdáleného ladění při použití nastavené průběžné doručování toopublish tooAzure](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).

[Team Foundation Build Service]: https://msdn.microsoft.com/library/ee259687.aspx
[.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
[.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
[.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
[Scale out your build system]: https://msdn.microsoft.com/library/dd793166.aspx
[Deploy and configure a build server]: https://msdn.microsoft.com/library/ms181712.aspx
[Azure PowerShell cmdlets]: /powershell/azureps-cmdlets-docs
[hello .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
[0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
[2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
[3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
[4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
[5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
[6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
