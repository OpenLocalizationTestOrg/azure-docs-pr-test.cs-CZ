---
title: "aaaUse prostředí PowerShell tooCreate virtuálních počítačů s nativní režim serveru sestav | Microsoft Docs"
description: "Toto téma popisuje a provede hello nasazení a konfiguraci serveru sestav v nativním režimu SQL Server Reporting Services ve virtuální počítač Azure. "
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: 553af55b-d02e-4e32-904c-682bfa20fa0f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/11/2017
ms.author: asaxton
ms.openlocfilehash: e7791199c87dff106132f1535da12de40a8dbc9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocreate-an-azure-vm-with-a-native-mode-report-server"></a>Pomocí prostředí PowerShell tooCreate Azure virtuálních počítačů s nativní režim serveru sestav
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

Toto téma popisuje a provede hello nasazení a konfiguraci serveru sestav v nativním režimu SQL Server Reporting Services ve virtuální počítač Azure. Hello kroky v tomto dokumentu kombinaci ruční kroky toocreate hello virtuálního počítače a skript prostředí Windows PowerShell tooconfigure Reporting Services na hello virtuálních počítačů. Hello konfigurační skript zahrnuje otevřít port brány firewall pro protokol HTTP nebo HTTPs.

> [!NOTE]
> Pokud nechcete, aby **HTTPS** na server sestav hello **přeskočit krok 2**.
> 
> Po vytvoření hello virtuálního počítače v kroku 1, přejděte toohello části pomocí skriptu tooconfigure hello server sestav a HTTP. Po spuštění skriptu hello hello serveru sestav je připraven toouse.

## <a name="prerequisites-and-assumptions"></a>Požadavky a předpoklady
* **Předplatné Azure**: Ověřte hello počet jader ve vašem předplatném Azure dostupná. Pokud vytvoříte hello doporučená velikost virtuálního počítače u **A3**, je nutné **4** dostupné jader. Pokud používáte velikost virtuálního počítače u **A2**, je nutné **2** dostupné jader.
  
  * tooverify hello základní omezení předplatného v hello portál Azure classic, klikněte na tlačítko nastavení v levém podokně hello a pak klikněte na tlačítko využití v horní nabídce hello.
  * tooincrease hello kvóta na jádra, kontaktujte [podporu Azure](https://azure.microsoft.com/support/options/). Informace o velikosti virtuálních počítačů, najdete v části [velikostí virtuálních počítačů pro Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* **Windows PowerShell skriptování**: hello tématu se předpokládá, že máte základní znalosti pracovní prostředí Windows PowerShell. Další informace o používání prostředí Windows PowerShell najdete v tématu hello následující:
  
  * [Spuštění prostředí Windows PowerShell v systému Windows Server](https://technet.microsoft.com/library/hh847814.aspx)
  * [Začínáme s prostředím Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>Krok 1: Zřídit virtuální počítač Azure
1. Procházejte toohello portál Azure classic.
2. Klikněte na tlačítko **virtuální počítače** v levém podokně hello.
   
    ![virtuální počítače Microsoft azure](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)
3. Klikněte na možnost **Nové**.
   
    ![tlačítko Nový](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)
4. Klikněte na tlačítko **z Galerie**.
   
    ![nový virtuální počítač z Galerie](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)
5. Klikněte na tlačítko **SQL Server 2014 RTM Standard – Windows Server 2012 R2** a pak klikněte na šipku toocontinue hello.
   
    ![Další](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
   
    Pokud potřebujete data služby Reporting Services hello řízené funkce předplatných, vyberte **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**. Další informace o edicích systému SQL Server a podporovaných funkcích najdete v tématu [funkce nepodporuje hello edice systému SQL Server 2012](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).
6. Na hello **konfigurace virtuálního počítače** upravte hello následující pole:
   
   * Pokud je více než jedna **datum vydání verze**, vyberte hello nejnovější verzi.
   * **Název virtuálního počítače**: název počítače hello se používá na další stránce konfigurace hello jako název DNS služby Cloud výchozí hello. název DNS Hello musí být jedinečný mezi hello služby Azure. Vezměte v úvahu konfigurace hello virtuálních počítačů s názvem počítače, který popisuje, jaké hello virtuálních počítačů se používá pro. Například ssrsnativecloud.
   * **Úroveň**: standardní
   * **Velikost: A3** je hello doporučená velikost virtuálního počítače pro úlohy SQL serveru. Pokud virtuální počítač je použít jenom jako server sestav, je dostatečná velikost virtuálního počítače z A2, pokud server sestav hello vyskytne velké zatížení. Informace o cenách virtuálních počítačů, najdete v části [ceny služeb Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/).
   * **Nové uživatelské jméno**: zadáte název hello je vytvořen jako správce hello virtuálních počítačů.
   * **Nové heslo** a **potvrďte**. Toto heslo se používá pro nový účet správce hello a doporučuje se, že používáte silné heslo.
   * Klikněte na **Další**. ![Další](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
7. Na další stránku hello upravte hello následující pole:
   
   * **Cloudová služba**: vyberte **vytvořte novou Cloudovou službu**.
   * **Cloudové služby DNS název**: Toto je hello veřejného názvu DNS hello Cloudová služba, která souvisí s hello virtuálních počítačů. Hello výchozí název je hello název, který jste zadali v pro hello název virtuálního počítače. Pokud v dalších krocích hello tématu, můžete vytvořit důvěryhodný certifikát SSL a pak se používá název DNS hello hello hodnotu hello "**vydané**" hello certifikátu.
   * **Oblast nebo vztahů skupiny nebo virtuální síť**: Zvolte hello oblast nejbližší tooyour koncovým uživatelům.
   * **Účet úložiště**: použít účet automaticky generované úložiště.
   * **Skupina dostupnosti**: žádné.
   * **Koncové body** zachovat hello **vzdálené plochy** a **prostředí PowerShell** koncových bodů a poté přidejte HTTP nebo HTTPS koncový bod, v závislosti na vašem prostředí.
     
     * **HTTP**: hello výchozí veřejné a privátní porty jsou **80**. Všimněte si, že pokud používáte privátní port než 80, změňte **$HTTPport = 80** ve skriptu hello protokolu http.
     * **HTTPS**: hello výchozí veřejné a privátní porty jsou **443**. Nejlepším postupem zabezpečení je toochange hello privátní port a nakonfigurujte vaší brány firewall a hello sestavy toouse hello privátní port serveru. Další informace o koncových bodů najdete v tématu [jak tooSet komunikace až s virtuálním počítačem](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Všimněte si, že pokud používáte jiný port než 443, změňte parametr hello **$HTTPsport = 443** v hello skriptu HTTPS.
   * Klikněte na tlačítko Další. ![Další](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
8. Na poslední stránce hello hello Průvodce ponechat výchozí hello **instalace agenta virtuálního počítače hello** vybrané. Hello kroků v tomto tématu není využívat hello agenta virtuálního počítače, ale pokud máte v plánu tookeep tento virtuální počítač, hello agenta virtuálního počítače a rozšíření vám umožní tooenhance he CM.  Další informace o hello agenta virtuálního počítače najdete v tématu [agenta virtuálního počítače a rozšíření – část 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). Jeden z hello výchozí nainstalovaná rozšíření ad systémem je rozšíření "BGINFO" hello, která zobrazuje na ploše virtuálního počítače hello, jednotka systémové informace, jako je například interní IP a volné místo.
9. Klikněte na dokončení. ![Ok](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)
10. Hello **stav** z virtuálního počítače se zobrazí jako hello **počáteční (zřizování)** během procesu zřizování hello a potom zobrazí jako **systémem** Pokud hello virtuálního počítače je zřízený a připravené toouse.

## <a name="step-2-create-a-server-certificate"></a>Krok 2: Vytvoření certifikátu serveru
> [!NOTE]
> Pokud nechcete, aby HTTPS na serveru sestav hello, můžete **přeskočit krok 2** a přejděte toohello části **používat server sestav hello tooconfigure skriptu a HTTP**. Použití hello HTTP skriptu tooquickly nakonfigurovat server sestav hello a hello sestavy, že server bude připravená toouse.

Pořadí toouse HTTPS na hello virtuálních počítačů je nutné důvěryhodný certifikát SSL. V závislosti na scénáři můžete použít jednu z následujících dvou metod hello:

* Platný certifikát SSL vydaný certifikační autoritou (CA) a Microsoft za důvěryhodnou. certifikáty kořenové certifikační Autority Hello jsou požadované toobe distribuovány prostřednictvím hello Microsoft Root Certificate Program. Další informace o tomto programu najdete v tématu [Windows a Windows Phone 8 SSL Root Certificate Program (člen certifikační autority)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) a [Úvod toohello Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).
* Certifikát podepsaný svým držitelem. Certifikáty podepsané svým držitelem nejsou doporučuje pro provozní prostředí.

### <a name="toouse-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>toouse vytvořit certifikát důvěryhodné certifikační autority (CA)
1. **Žádosti o certifikát serveru pro web hello od certifikační autority**. 
   
    Hello Průvodce certifikátem webového serveru můžete použít buď toogenerate soubor žádosti o certifikát (Certreq.txt), můžete odeslat tooa certifikační autoritou, nebo toogenerate požadavek pro online certifikační autoritu. Například certifikační služby společnosti Microsoft ve Windows serveru 2012. V závislosti na úrovni hello identifikace záruky poskytované certifikátem serveru je několik měsíců dnů tooseveral hello certifikační autority tooapprove vaši žádost a odeslat je soubor certifikátu. 
   
    Další informace o vyžádání certifikátů serveru najdete v tématu hello následující: 
   
   * Použití [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).
   * Nástroje pro tooAdminister zabezpečení systému Windows Server 2012.
     
     [Nástroje pro tooAdminister zabezpečení systému Windows Server 2012](https://technet.microsoft.com/library/jj730960.aspx)
     
     > [!NOTE]
     > Hello **vydané** pole hello důvěryhodný certifikát SSL by měl být hello stejné jako hello **název cloudové služby DNS** jste použili pro hello nového virtuálního počítače.

2. **Nainstalujte certifikát serveru hello na webový server hello**. Hello webový server je v tomto případě hello virtuálních počítačů, hostitelů hello server sestav a hello web je vytvořen v dalších krocích při konfiguraci služby Reporting Services. Další informace o instalaci certifikátu serveru hello hello webový server pomocí modulu snap-in konzoly MMC certifikát hello najdete v tématu [nainstalovat certifikát serveru](https://technet.microsoft.com/library/cc740068).
   
    Pokud chcete, aby toouse hello skript dodaný spolu s Toto téma, server sestav hello tooconfigure hello hodnota hello certifikátů **kryptografický otisk** se vyžaduje jako parametr hello skriptu. V části hello další podrobnosti o tom, jak tooobtain hello kryptografický otisk certifikátu hello.
3. Přiřazení serveru sestav toohello certifikát server hello. přiřazení Hello byla dokončena v další části hello při konfiguraci serveru sestav hello.

### <a name="toouse-hello-virtual-machines-self-signed-certificate"></a>toouse hello certifikát podepsaný svým držitelem virtuální počítače
Certifikát podepsaný svým držitelem byl vytvořen na hello virtuálních počítačů při zřizování hello virtuálních počítačů. Hello certifikát má stejný název jako název DNS virtuálního počítače hello hello. V pořadí tooavoid certifikát chyby, není zapotřebí hello certifikátu je důvěryhodný na hello virtuální počítač a také všichni uživatelé hello lokality.

1. tootrust hello kořenové certifikační Autority certifikátu hello na hello Virtuálního počítače, přidejte hello certifikát toohello **důvěryhodné kořenové certifikační autority**. Hello Následuje souhrn hello kroky. Podrobné kroky na tom, jak tootrust hello certifikační Autority najdete v tématu [nainstalovat certifikát serveru](https://technet.microsoft.com/library/cc740068).
   
   1. Hello portál Azure classic, vyberte hello virtuálních počítačů a klikněte na tlačítko Připojit. V závislosti na konfiguraci vašeho prohlížeče může být výzvami toosave soubor RDP pro připojení toohello virtuálních počítačů.
      
       ![připojit tooazure virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Použijte název virtuálního počítače hello uživatele, uživatelské jméno a heslo, které jste nakonfigurovali při vytváření hello virtuálních počítačů. 
      
       Například v hello následující bitové kopie, je název virtuálního počítače hello **ssrsnativecloud** a hello uživatelské jméno je **testuser**.
      
       ![název virtuálního počítače jejíž součástí přihlášení](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
   2. Spusťte mmc.exe. Další informace najdete v tématu [postupy: zobrazení certifikátů pomocí modulu Snap-in konzoly MMC hello](https://msdn.microsoft.com/library/ms788967.aspx).
   3. V konzole aplikace hello **soubor** nabídce Přidat hello **certifikáty** modul snap-in, vyberte **účet počítače** při zobrazení výzvy a pak klikněte na tlačítko **Další**.
   4. Vyberte **místního počítače** toomanage a pak klikněte na **Dokončit**.
   5. Klikněte na tlačítko **Ok** a potom rozbalte hello **certifikáty – osobní** uzly a pak klikněte na tlačítko **certifikáty**. Hello certifikát jmenuje po název DNS hello hello virtuálních počítačů a končí **cloudapp.net**. Klikněte pravým tlačítkem na název hello certifikátu a klikněte na tlačítko **kopie**.
   6. Rozbalte hello **důvěryhodné kořenové certifikační autority** uzel a potom klikněte pravým tlačítkem na **certifikáty** a pak klikněte na **vložení**.
   7. toovalidate, double klikněte na název certifikátu hello pod **důvěryhodné kořenové certifikační autority** a ověřte, že nejsou žádné chyby a svůj certifikát. Pokud chcete, aby toouse hello HTTPS skript dodaný spolu s Toto téma, server sestav hello tooconfigure hello hodnota hello certifikátů **kryptografický otisk** se vyžaduje jako parametr hello skriptu. **Hodnota kryptografického otisku hello tooget**, dokončete následující hello. V části je také prostředí PowerShell ukázkový tooretrieve hello kryptografický otisk [používat server sestav hello tooconfigure skriptu a HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).
      
      1. Klikněte dvakrát na název hello hello certifikátu, například ssrsnativecloud.cloudapp.net.
      2. Klikněte na tlačítko hello **podrobnosti** kartě.
      3. Klikněte na tlačítko **kryptografický otisk**. v poli Podrobnosti hello, například a6 se zobrazí hodnota Hello hello kryptografický otisk 08 3c df f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2c fb 2f.
      4. Zkopírujte kryptografický otisk hello a uložit pro pozdější hello hodnotu nebo upravte skript hello teď.
      5. (*) Před spuštěním skriptu hello odeberte hello mezery mezi hello dvojice hodnot. Například kryptografický otisk hello zjištěno před by nyní a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.
      6. Přiřazení serveru sestav toohello certifikát server hello. přiřazení Hello byla dokončena v další části hello při konfiguraci serveru sestav hello.

Pokud používáte certifikát podepsaný svým držitelem SSL, název hello na hello certifikátu již odpovídá hello název hostitele hello virtuálních počítačů. Proto hello DNS počítače hello je již zaregistrován globálně a je přístupná z libovolného klienta.

## <a name="step-3-configure-hello-report-server"></a>Krok 3: Konfigurace hello serveru sestav
Tato část vás provede procesem konfigurace hello virtuální počítač jako server sestav služby Reporting Services v nativním režimu. Můžete použít jednu z hello následující server sestav hello tooconfigure metody:

* Používat server sestav hello skriptu tooconfigure hello
* Použijte nástroj Configuration Manager tooConfigure hello serveru sestav.

Podrobné kroky, najdete v části hello [toohello připojit virtuální počítač a spusťte Správce konfigurace služby Reporting Services hello](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Poznámka: ověřování:** ověřování systému Windows je doporučená metoda ověřování hello a je hello výchozí služby Reporting Services ověřování. Pouze uživatelé, kteří jsou nakonfigurované na hello virtuálních počítačů přístup k službě Reporting Services a přiřazené tooReporting služeb role.

### <a name="use-script-tooconfigure-hello-report-server-and-http"></a>Server sestav pomocí skriptu tooconfigure hello a HTTP
toouse hello prostředí Windows PowerShell skriptu tooconfigure hello serveru sestav, dokončení hello následující kroky. Konfigurace Hello obsahuje protokolu HTTP, nikoli HTTPS:

1. Hello portál Azure classic, vyberte hello virtuálních počítačů a klikněte na tlačítko Připojit. V závislosti na konfiguraci vašeho prohlížeče může být výzvami toosave soubor RDP pro připojení toohello virtuálních počítačů.
   
    ![připojit tooazure virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Použijte název virtuálního počítače hello uživatele, uživatelské jméno a heslo, které jste nakonfigurovali při vytváření hello virtuálních počítačů. 
   
    Například v hello následující bitové kopie, je název virtuálního počítače hello **ssrsnativecloud** a hello uživatelské jméno je **testuser**.
   
    ![název virtuálního počítače jejíž součástí přihlášení](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. Na hello virtuálních počítačů, otevřete **Windows PowerShell ISE** s oprávněními správce. Hello PowerShell ISE je nainstalována ve výchozím nastavení v systému Windows server 2012. Doporučuje se, že používáte hello ISE místo standardní okno prostředí Windows PowerShell, aby mohli vložit hello skriptu do hello ISE, upravte hello skript a spusťte skript hello.
3. V systému Windows PowerShell ISE, klikněte na tlačítko hello **zobrazení** nabídce a pak klikněte na tlačítko **zobrazit podokno skriptu**.
4. Zkopírujte hello následující skript a vložte hello skriptu do podokno skriptu Windows PowerShell ISE hello.
   
        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
   
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change hello value if you used a different port for hello private HTTP endpoint when hello VM was created.
   
        ## Set PowerShell execution policy toobe able toorun scripts
        Set-ExecutionPolicy RemoteSigned -Force
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change hello version portion of hello path too"v11" toouse hello script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Report Server Configuration Steps
   
        ## Setting hello web service URL ##
        write-host -foregroundcolor green "Setting hello web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
   
        ## Setting hello Database ##
        write-host -foregroundcolor green "Setting hello Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating hello database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script toocreate hello database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes toosqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## Setting hello Report Manager URL ##
   
        write-host -foregroundcolor green "Setting hello Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
   
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
5. Pokud jste vytvořili hello virtuálních počítačů s k portu HTTP než 80, upravte parametr hello $HTTPport = 80.
6. skript Hello je aktuálně konfigurována pro službu Reporting Services. Pokud chcete toorun hello skript pro službu Reporting Services, upravte část verze hello hello cesta toohello oboru názvů příliš verze "11", na příkaz Get-WmiObject hello.
7. Spusťte skript hello.

**Ověření**: tooverify, který funkce serveru základní sestavy hello funguje, najdete v části hello [konfigurace ověřte, zda text hello](#verify-the-configuration) později v tomto tématu.

### <a name="use-script-tooconfigure-hello-report-server-and-https"></a>Server sestav pomocí skriptu tooconfigure hello a HTTPS
toouse prostředí Windows PowerShell tooconfigure hello serveru sestav, dokončení hello následující kroky. zahrnuje Hello konfigurace protokolu HTTPS, nikoli protokol HTTP.

1. Hello portál Azure classic, vyberte hello virtuálních počítačů a klikněte na tlačítko Připojit. V závislosti na konfiguraci vašeho prohlížeče může být výzvami toosave soubor RDP pro připojení toohello virtuálních počítačů.
   
    ![připojit tooazure virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Použijte název virtuálního počítače hello uživatele, uživatelské jméno a heslo, které jste nakonfigurovali při vytváření hello virtuálních počítačů. 
   
    Například v hello následující bitové kopie, je název virtuálního počítače hello **ssrsnativecloud** a hello uživatelské jméno je **testuser**.
   
    ![název virtuálního počítače jejíž součástí přihlášení](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. Na hello virtuálních počítačů, otevřete **Windows PowerShell ISE** s oprávněními správce. Hello PowerShell ISE je nainstalována ve výchozím nastavení v systému Windows server 2012. Doporučuje se, že používáte hello ISE místo standardní okno prostředí Windows PowerShell, aby mohli vložit hello skriptu do hello ISE, upravte hello skript a spusťte skript hello.
3. tooenable spouštění skriptů, spusťte následující příkaz prostředí Windows PowerShell hello:
   
        Set-ExecutionPolicy RemoteSigned
   
    Potom můžete spustit hello následující tooverify hello zásad:
   
        Get-ExecutionPolicy
4. V **Windows PowerShell ISE**, klikněte na tlačítko hello **zobrazení** nabídce a pak klikněte na tlačítko **zobrazit podokno skriptu**.
5. Zkopírujte následující skript a vložte jej do podokno skriptu Windows PowerShell ISE hello hello.
   
        ## This script configures hello report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when hello HTTPS endpoint was created.
   
        # You can run hello following command tooget (.cloudapp.net certificates) so you can copy hello thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # hello certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # hello certificate hash should not contain spaces
   
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If hello certificate is not a wildcard certificate, comment out hello following line, and enable hello full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        write-host "hello script will use $DNSNameAndPort as hello DNS name and port" 
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change hello version portion of hello path too"v11" toouse hello script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Reporting Services Report Server Configuration Steps
   
        ## 1. Setting hello web service URL ##
        write-host -foregroundcolor green "Setting hello web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
   
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
   
        ## 2. Setting hello Database ##
        write-host -foregroundcolor green "Setting hello Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating hello database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script toocreate hello database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes toosqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## 3. Setting hello Report Manager URL ##
   
        write-host -foregroundcolor green "Setting hello Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
   
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
   
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
6. Upravit hello **$certificatehash** parametr ve skriptu hello:
   
   * Toto je **požadované** parametr. Pokud hodnotu hello certifikátu nebyla uložena z předchozích kroků hello, použijte jednu z následujících metod toocopy hello hodnotu hash pro certifikát z kryptografický otisk certifikáty hello hello.:
     
       Na hello virtuálních počítačů otevřete Windows PowerShell ISE a spusťte následující příkaz hello:
     
           dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
     
       Hello výstup bude vypadat podobně jako následující toohello. Pokud hello skript vrátí prázdný řádek, hello virtuální počítač nemá certifikát třeba nakonfigurovat, najdete v tématu hello [toouse hello certifikát podepsaný svým držitelem virtuálních počítačů](#to-use-the-virtual-machines-self-signed-certificate).
     
     NEBO
   * Hello mmc.exe spustit virtuální počítač a poté přidejte hello **certifikáty** modul snap-in.
   * V části hello **důvěryhodné kořenové certifikační úřady** uzlu, dvakrát klikněte na název certifikátu. Pokud používáte certifikát podepsaný svým držitelem hello hello virtuálních počítačů, certifikát hello jmenuje po název DNS hello hello virtuálních počítačů a končí **cloudapp.net**.
   * Klikněte na tlačítko hello **podrobnosti** kartě.
   * Klikněte na tlačítko **kryptografický otisk**. Hodnota Hello kryptografický otisk hello je zobrazena v hello pole podrobností, například af 11 60 b6 4b 28 8 d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48
   * **Před spuštěním skriptu hello**, odebrat hello mezery mezi hello dvojice hodnot. Například af1160b64b288d890a8212ff6ba9c3664f319048
7. Upravit hello **$httpsport** parametr: 
   
   * Pokud jste použili port 443 pro koncový bod HTTPS hello, pak nepotřebujete tooupdate tento parametr ve skriptu hello. Jinak použijte hello port hodnotu, kterou jste vybrali při konfiguraci privátní koncový bod HTTPS hello na hello virtuálních počítačů.
8. Upravit hello **$DNSName** parametr: 
   
   * skript Hello je nakonfigurován pro zástupný certifikát $DNSName = "+". Pokud provádíte žádné tooconfigure chcete pro vazbu certifikátu zástupný znak, komentář $DNSName = "+"a povolte následující řádek, hello úplné $DNSNAme odkaz, ## $DNSName="$server.cloudapp.net hello".
     
       Změňte hodnotu hello $DNSName, pokud nechcete, aby název DNS toouse hello virtuálního počítače pro službu Reporting Services. Pokud použijete parametr hello, hello certifikát musíte taky použít tento název a zaregistrujte název hello globálně na serveru DNS.
9. skript Hello je aktuálně konfigurována pro službu Reporting Services. Pokud chcete toorun hello skript pro službu Reporting Services, upravte část verze hello hello cesta toohello oboru názvů příliš verze "11", na příkaz Get-WmiObject hello.
10. Spusťte skript hello.

**Ověření**: tooverify, který funkce serveru základní sestavy hello funguje, najdete v části hello [konfigurace ověřte, zda text hello](#verify-the-connection) později v tomto tématu. vazbu certifikátu hello tooverify otevřete příkazový řádek s oprávněními správce a spusťte následující příkaz hello:

    netsh http show sslcert

Hello výsledek bude obsahovat hello následující:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-tooconfigure-hello-report-server"></a>Použijte nástroj Configuration Manager tooConfigure hello serveru sestav
Pokud nechcete, aby toorun hello prostředí PowerShell skriptu tooconfigure hello serveru sestav, postupujte podle kroků hello v této části toouse hello služby Reporting Services v nativním režimu tooconfigure hello sestavy serveru nástroje configuration manager.

1. Hello portál Azure classic, vyberte hello virtuálních počítačů a klikněte na tlačítko Připojit. Použití hello uživatelské jméno a heslo, které jste nakonfigurovali při vytváření hello virtuálních počítačů.
   
    ![připojit tooazure virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)
2. Spusťte službu Windows update a nainstalovat aktualizace toohello virtuálních počítačů. Pokud se vyžaduje restartování systému hello virtuálních počítačů, restartujte hello virtuálních počítačů a znovu připojit toohello virtuálních počítačů z hello portál Azure classic.
3. Z nabídky Start hello na hello virtuálního počítače, zadejte **služby Reporting Services** a otevřete **Správce konfigurace služby Reporting Services**.
4. Nechte hello výchozí hodnoty pro **název serveru** a **instanci serveru sestav**. Klikněte na **Připojit**.
5. V levém podokně hello, klikněte na **adresa URL webové služby**.
6. Ve výchozím nastavení je RS nakonfigurován pro protokol HTTP port 80 s IP Adresou "Všechny přiřazené". tooadd HTTPS:
   
   1. V **certifikát SSL**: Vyberte hello certifikát má toouse, například [název virtuálního počítače]. cloudapp.net. Pokud nejsou uvedeny žádné certifikáty, najdete v tématu hello **krok 2: vytvoření certifikátu serveru** informace o tom, jak tooinstall a vztah důvěryhodnosti hello certifikát na hello virtuálních počítačů.
   2. V části **SSL Port**: Zvolte 443. Pokud jste nakonfigurovali privátní koncový bod HTTPS hello v hello virtuálního počítače s jinou privátní port, použijte tuto hodnotu sem.
   3. Klikněte na tlačítko **použít** a počkejte toocomplete operaci hello.
7. V levém podokně hello, klikněte na **databáze**.
   
   1. Klikněte na tlačítko **změnit Databas**e.
   2. Klikněte na tlačítko **vytvořit novou databázi serveru sestav** a pak klikněte na **Další**.
   3. Ponechte výchozí hello **název serveru**: jako hello virtuálního počítače zadejte název a ponechte výchozí hello **typ ověřování** jako **aktuální uživatel** – **integrované zabezpečení**. Klikněte na **Další**.
   4. Ponechte výchozí hello **název databáze** jako **ReportServer** a klikněte na tlačítko **Další**.
   5. Ponechte výchozí hello **typ ověřování** jako **pověření služby** a klikněte na tlačítko **Další**.
   6. Klikněte na tlačítko **Další** na hello **Souhrn** stránky.
   7. Po dokončení konfigurace hello klikněte na tlačítko **Dokončit**.
8. V levém podokně hello, klikněte na **adresa URL správce sestav**. Ponechte výchozí hello **virtuální adresář** jako **sestavy** a klikněte na tlačítko **použít**.
9. Klikněte na tlačítko **ukončení** tooclose hello Správce konfigurace služby Reporting Services.

## <a name="step-4-open-windows-firewall-port"></a>Krok 4: Port brány Firewall systému Windows otevřete
> [!NOTE]
> Pokud jste použili jedno hello skripty tooconfigure hello sestavy serveru, můžete tuto část přeskočit. skript Hello zahrnuty port brány firewall krok tooopen hello. Výchozí Hello se port 80 pro protokol HTTP a port 443 pro protokol HTTPS.
> 
> 

tooconnect vzdáleně tooReport Manager nebo hello zprávu Server hello virtuálního počítače, koncový bod TCP je vyžadován na hello virtuálních počítačů. Je požadovaná tooopen hello stejný port v bráně firewall hello Virtuálního počítače. koncový bod Hello byl vytvořen při zřizování hello virtuálních počítačů.

Tato část obsahuje základní informace o tom, jak tooopen hello port brány firewall. Další informace najdete v tématu [Configure a Firewall for Report Server Access](https://technet.microsoft.com/library/bb934283.aspx)

> [!NOTE]
> Pokud jste použili hello skriptu tooconfigure hello sestavy serveru, můžete tuto část přeskočit. skript Hello zahrnuty port brány firewall krok tooopen hello.
> 
> 

Pokud jste nakonfigurovali privátní port pro protokol HTTPS 443, upravte následující skript správně hello. tooopen port **443** na hello brány Windows Firewall, dokončete následující hello:

1. Otevřete okno prostředí Windows PowerShell s oprávněními správce.
2. Pokud jste použili jiný port než 443, když jste nakonfigurovali koncový bod HTTPS hello na hello virtuálních počítačů, aktualizujte hello port v hello následující příkaz a poté spusťte příkaz hello:
   
        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443
3. Po dokončení příkazu hello **Ok** se zobrazí v hello příkazového řádku.

tooverify, že je otevřen hello port, otevřete okno prostředí Windows PowerShell a spusťte hello následující příkaz:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-hello-configuration"></a>Ověření konfigurace hello
tooverify, který nyní pracuje funkce serveru hello základní sestavy, otevřete prohlížeč s oprávněními správce a pak procházet toohello následující zprávy Správce sestav služby ad serveru adresy URL:

* Na hello virtuálních počítačů procházet toohello adresa URL serveru sestav:
  
        http://localhost/reportserver
* Na hello virtuálních počítačů procházet toohello adresa URL správce sestav:
  
        http://localhost/Reports
* Z místního počítače, vyhledejte toohello **vzdáleného** Manager vykazovat hello virtuálních počítačů. Název DNS hello v hello následující ukázka podle potřeby aktualizujte. Po zobrazení výzvy k zadání hesla, použijte přihlašovací údaje správce hello, kterou jste vytvořili při zřizování hello virtuálních počítačů. Hello uživatelské jméno je v hello [doména]\[uživatelské jméno] formátu, kde doména hello je název počítače hello virtuálních počítačů, například ssrsnativecloud\testuser. Pokud nepoužíváte HTTP**S**, odeberte hello **s** v adrese URL hello. V části hello Další informace o vytvoření dalších uživatelů na virtuálním počítači.
  
        https://ssrsnativecloud.cloudapp.net/Reports
* Z místního počítače vyhledejte adresa URL toohello sestavu vzdáleného serveru. Název DNS hello v hello následující ukázka podle potřeby aktualizujte. Pokud nepoužíváte protokol HTTPS, odeberte hello s v adrese URL hello.
  
        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Vytvoření uživatelů a přiřazování rolí
Konfigurace a ověření hello sestavu serveru běžné úlohy správy je toocreate jeden nebo více uživatelů a přiřazení uživatelů tooReporting služby rolí. Další informace najdete v tématu hello následující:

* [Vytvořte místní uživatelský účet](https://technet.microsoft.com/library/cc770642.aspx)
* [Udělení přístupu uživatele tooa serveru sestav (Správce sestav)](https://msdn.microsoft.com/library/ms156034.aspx))
* [Vytvoření a správa přiřazení rolí](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="toocreate-and-publish-reports-toohello-azure-virtual-machine"></a>tooCreate a publikovat sestavy toohello virtuální počítač Azure
Hello následující tabulka shrnuje některé hello možnosti k dispozici toopublish existující sestavy ze serveru místní počítač toohello sestavy musí být hostované na hello virtuální počítač Microsoft Azure:

* **Skript RS.exe**: položky sestavy toocopy RS.exe použití skriptu z a existující sestavy serveru tooyour virtuální počítač Microsoft Azure. Další informace naleznete v části hello "nativní režim tooNative režimu – virtuálním počítači Microsoft Azure" [ukázka Reporting Services rs.exe skriptu tooMigrate obsah mezi servery sestav](https://msdn.microsoft.com/library/dn531017.aspx).
* **Tvůrce sestav**: hello virtuální počítač obsahuje hello kliknutím-jednou verzi Tvůrce sestav Microsoft SQL Server. toostart sestavy Tvůrce hello poprvé hello virtuálního počítače:
  
  1. Spustíte prohlížeč s oprávněními správce.
  2. Procházet tooreport manager hello virtuálního počítače a klikněte na tlačítko **Tvůrce sestav** hello pásu karet.
     
     Další informace najdete v tématu [instalace, odinstalace a podpora Tvůrce sestav](https://technet.microsoft.com/library/dd207038.aspx).
* **SQL Server Data Tools: Virtuální počítač**: Pokud jste vytvořili hello virtuálního počítače s SQL serverem 2012, pak SQL Server Data Tools je nainstalovaná hello virtuálního počítače a může být použité toocreate **projekty serveru sestav** a sestavy na virtuálním hello počítač. SQL Server Data Tools můžete publikovat hello sestavy toohello serveru sestav na virtuálním počítači hello.
  
    Pokud jste vytvořili hello virtuálních počítačů se systémem SQL server 2014, můžete nainstalovat SQL Server Data Tools - BI pro sadu visual Studio. Další informace najdete v tématu hello následující:
  
  * [Nástroje Microsoft SQL Server Data Tools – Business Intelligence pro Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313)
  * [Nástroje Microsoft SQL Server Data Tools – Business Intelligence pro sadu Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)
  * [Nástroje SQL Server Data Tools a SQL Server Business Intelligence (SSDT-BI)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)
* **SQL Server Data Tools: Vzdálené**: V místním počítači, vytvoření projektu služby Reporting Services v SQL Server Data Tools, obsahující sestavy služby Reporting Services. Nakonfigurujte hello projektu tooconnect toohello adresa URL webové služby.
  
    ![Vlastnosti projektu rozšíření SSDT pro projekt služby SSRS](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)
* **Použít skript**: použijte skript toocopy sestavy serveru obsahu. Další informace najdete v tématu [ukázka Reporting Services rs.exe skriptu tooMigrate obsah mezi servery sestav](https://msdn.microsoft.com/library/dn531017.aspx).

## <a name="minimize-cost-if-you-are-not-using-hello-vm"></a>Minimalizovat náklady, pokud nepoužíváte hello virtuálních počítačů
> [!NOTE]
> poplatky toominimize pro virtuální počítače Azure při není používán, vypněte hello virtuálních počítačů z hello portál Azure classic. Pokud použijete možnosti napájení Windows hello uvnitř virtuálního počítače tooshut dolů hello virtuálních počítačů, které budou účtovat hello stejné částka pro hello virtuálních počítačů. tooreduce poplatky, budete potřebovat tooshut dolů hello virtuálních počítačů v hello portál Azure classic. Pokud již nepotřebujete hello virtuálních počítačů, mějte na paměti, toodelete hello virtuálních počítačů a hello přidružené VHD poplatky za uložení tooavoid soubory. Další informace najdete v tématu oddíl hello – nejčastější dotazy na [podrobnosti o cenách virtuálních počítačů](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="more-information"></a>Další informace
### <a name="resources"></a>Zdroje
* Podobně jako obsah související s tooa nasazení jednoho serveru SQL Server Business Intelligence a SharePoint 2013, najdete v části [tooCreate použijte rozhraní Windows PowerShell Azure virtuálního počítače s SQL Server BI a SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).
* Podobně jako obsah související tooa nasazení více serverů SQL Server Business Intelligence a SharePoint 2013, najdete v části [nasazení SQL Server Business Intelligence v Azure Virtual Machines](https://msdn.microsoft.com/library/dn321998.aspx).
* Obecné informace najdete v části související toodeployments SQL Server Business Intelligence v Azure Virtual Machines [SQL Server Business Intelligence v Azure Virtual Machines](virtual-machines-windows-classic-ps-sql-bi.md).
* Další informace o hello náklady poplatky za výpočty Azure najdete v tématu kartě virtuální počítače hello [Azure cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).

### <a name="community-content"></a>Komunitním obsahu
* Pokyny krok za krokem jak toocreate Reporting Services na nativní režim serveru sestav bez použití skriptu, najdete v části [hostování služby SQL Reporting Services na virtuální počítač Azure](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-tooother-resources-for-sql-server-in-azure-vms"></a>Odkazy tooother prostředky pro SQL Server ve virtuálních počítačích Azure
[SQL Server na virtuálních počítačích Azure – přehled](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

