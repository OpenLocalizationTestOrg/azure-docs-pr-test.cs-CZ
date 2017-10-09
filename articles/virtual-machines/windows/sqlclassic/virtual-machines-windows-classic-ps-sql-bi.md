---
title: aaaSQL Server Business Intelligence | Microsoft Docs
description: "Toto téma používá prostředky, které jsou vytvořené pomocí modelu nasazení classic hello a popisuje dostupné funkce hello Business Intelligence (BI) pro SQL Server běžící na virtuálních počítačích Azure (VM)."
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: c681e7a7-eeda-48aa-bc35-6277f4828244
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/30/2017
ms.author: asaxton
ms.openlocfilehash: e3288f0835d6c4a19baeeea5f6b65fec16cd751f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>SQL Server Business Intelligence v Azure Virtual Machines
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

Virtuální počítač Microsoft Azure Galerie Hello zahrnuje bitové kopie, které obsahují instalace systému SQL Server. Hello jsou v hello Galerie obrázků podporované edice systému SQL Server hello stejný instalační soubory, které můžete nainstalovat tooon místních počítačů a virtuálních počítačů. Toto téma shrnuje hello SQL Server Business Intelligence (BI) funkce nainstalované na hello bitové kopie a kroků konfigurace požadovaných po zřízení virtuálního počítače. Toto téma také popisuje podporované topologie nasazení funkce BI a osvědčené postupy.

## <a name="license-considerations"></a>Důležité informace o licenci
Existují dva způsoby toolicense systému SQL Server ve virtuálních počítačích Microsoft Azure:

1. Výhody mobility licencí, které jsou součástí programu Software Assurance. Další informace najdete v tématu [mobilitou licencí v rámci programu Software Assurance na Azure](https://azure.microsoft.com/pricing/license-mobility/).
2. Platba za hodinu míra Azure virtuálních počítačů s nainstalovaným serverem SQL. Najdete v tématu "SQL Server" hello [ceny služeb Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Další informace o licencování a aktuální míry najdete v tématu [virtuální počítače nejčastější dotazy ohledně licencování](https://azure.microsoft.com/pricing/licensing-faq/%20/).

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>SQL Server bitové kopie k dispozici v galerii virtuálních počítačů Azure
Virtuální počítač Microsoft Azure Galerie Hello obsahuje několik imagí, které obsahují Microsoft SQL Server. Hello softwaru nainstalovaného na hello bitové kopie virtuálních počítačů se liší podle verze hello hello operačního systému a hello verzi systému SQL Server. Hello seznam bitové kopie k dispozici v galerii virtuálních počítačů Azure hello často mění.

<!--![SQL image in azure VM gallery](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)-->
![SQL obrázek v galerii virtuálních počítačů Azure](./media/virtual-machines-windows-classic-ps-sql-bi/vm-sql-images.png)

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Hello následující skript prostředí PowerShell vrátí hello seznam Azure bitové kopie, které obsahují "SQL serverem" v hello ImageName:

    # assumes you have already uploaded a management certificate tooyour Microsoft Azure Subscription. View hello thumbprint value from hello "Subscriptions" menu in Azure portal.

    $subscriptionID = ""    # REQUIRED: Provide your subscription ID.
    $subscriptionName = "" # REQUIRED: Provide your subscription name.
    $thumbPrint = "" # REQUIRED: Provide your certificate thumbprint.
    $certificate = Get-Item cert:\currentuser\my\$thumbPrint # REQUIRED: If your certificate is in a different store, provide it here.-Ser  store is hello one specified with hello -ss parameter on MakeCert

    Set-AzureSubscription -SubscriptionName $subscriptionName -Certificate $certificate -SubscriptionID $subscriptionID

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2016"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2016*"} | select imagename,category, location, label, description

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2014"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2014*"} | select imagename,category, location, label, description

Další informace o vydání a funkce v systému SQL Server podporovány naleznete následující hello:

* [Edice serveru SQL](https://www.microsoft.com/server-cloud/products/sql-server-editions/#fbid=Zae0-E6r5oh)
* [Funkce podporované nástrojem hello edice systému SQL Server 2016](https://msdn.microsoft.com/library/cc645993.aspx)

### <a name="bi-features-installed-on-hello-sql-server-virtual-machine-gallery-images"></a>BI funkce nainstalovány na hello Galerie bitové kopie na virtuální počítač serveru SQL
Hello následující tabulka shrnuje funkce Business Intelligence hello nainstalované na hello běžné virtuální počítač Microsoft Azure Galerie obrázků pro SQL Server:

* SQL Server 2016 SP1 Enterprise
* SQL Server 2016 SP1 Standard
* SQL Server 2014 SP2 Enterprise
* SQL Server 2014 SP2 Standard
* SQL Server 2012 SP3 Enterprise
* SQL Server 2012 SP3 Standard

| Funkci SQL Server BI | Nainstalovat na bitovou Galerie hello | Poznámky |
| --- | --- | --- |
| **Reporting Services – nativní režim** |Ano |Nainstalován, ale vyžaduje konfigurace, včetně hello adresa URL správce sestav. Části hello [konfigurace služby Reporting Services](#configure-reporting-services). |
| **Služby Reporting Services režimu serveru SharePoint** |Ne |Hello virtuální počítač Microsoft Azure Galerie image neobsahuje SharePoint nebo SharePoint instalační soubory. <sup>1</sup> |
| **Analysis Services Multidimensional a Data dolování (OLAP)** |Ano |Instance služby Analysis Services hello instalovaná a nakonfigurovaná jako výchozí |
| **Tabulkové služby Analysis Services** |Ne |Podporované v systému SQL Server 2012, 2014 a 2016 bitové kopie ale není nainstalována ve výchozím nastavení. Instalovat další instanci služby Analysis Services. Části hello nainstalovat další služby SQL Server a funkce v tomto tématu. |
| **Power Pivot. Analysis Services pro službu SharePoint** |Ne |Hello virtuální počítač Microsoft Azure Galerie image neobsahuje SharePoint nebo SharePoint instalační soubory. <sup>1</sup> |

<sup>1</sup> Další informace o virtuálních počítačích Azure a služby SharePoint, naleznete v části [architektury Microsoft Azure pro službu SharePoint 2013](https://technet.microsoft.com/library/dn635309.aspx) a [nasazení služby SharePoint v Microsoft Azure Virtual Machines](https://www.microsoft.com/download/details.aspx?id=34598).

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) V názvu služby hello spusťte seznam nainstalovaných služeb, které obsahují "SQL" hello následující tooget příkaz prostředí PowerShell.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Obecná doporučení a osvědčené postupy
* Hello Minimální doporučená velikost virtuálního počítače je **A3** při použití SQL Server Enterprise Edition. Hello **A4** velikost virtuálního počítače se doporučuje pro nasazení systému SQL Server BI Analysis Services a Reporting Services.
  
    Informace o hello aktuální velikosti virtuálních počítačů, v tématu [velikostí virtuálních počítačů pro Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Osvědčeným postupem pro správu disků je jiné než toostore dat, log a záložní soubory na jednotkách **C**: a **D**:. Můžete například vytvořit datové disky **E**: a **F**:.
  
  * jednotka Hello ukládání do mezipaměti zásady pro hello výchozí jednotce **C**: nevhodné pro práci s daty.
  * Hello **D**: jednotka je dočasné jednotce, která se používá hlavně pro hello stránkovacího souboru. Hello **D**: jednotka není trvalý a neukládají se v úložišti objektů blob. Úloh správy, jako je změna velikost virtuálního počítače toohello resetovat hello **D**: jednotky. Doporučuje se příliš**není** použít hello **D**: disku pro soubory databáze, včetně databáze tempdb.
    
    Další informace o vytváření a připojení disků najdete v tématu [jak tooAttach tooa datový Disk virtuálního počítače](../classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* Zastavení nebo odinstalovat neplánujete toouse služby. Pro příklad Pokud hello virtuální počítač se používá pouze pro služby Reporting Services, zastavte nebo odinstalaci služby Analysis Services a SQL Server Integration Services. Hello následující obrázek je příkladem hello služby, které jsou spuštěny ve výchozím nastavení.
  
    ![Službu SQL Server.](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)
  
  > [!NOTE]
  > databázový stroj SQL Server Hello je vyžadována ve scénářích BI hello podporována. Topologie virtuálních počítačů jeden server, databázový stroj hello je vyžadován toobe systémem hello stejného virtuálního počítače.
  
    Další informace najdete v tématu následující hello: [odinstalaci služby Reporting Services](https://msdn.microsoft.com/library/hh479745.aspx) a [odinstalovat instanci služby Analysis Services](https://msdn.microsoft.com/library/ms143687.aspx).
* Zkontrolujte **Windows Update** nové, důležité aktualizace'. Virtuální počítač Microsoft Azure Hello bitové kopie jsou často aktualizovat; ale může být důležité aktualizace k dispozici z **Windows Update** po poslední aktualizaci hello image virtuálního počítače.

## <a name="example-deployment-topologies"></a>Příklad topologie nasazení
Hello následují příklad nasazení, které používají virtuální počítače Microsoft Azure. topologie Hello v těchto diagramy jsou jenom některé z hello možné topologie, které můžete použít s funkcemi SQL Server BI a virtuální počítače Microsoft Azure.

### <a name="single-virtual-machine"></a>Jeden virtuální počítač
Analysis Services, služby Reporting Services, databázového stroje SQL Server a zdrojů dat na jednom virtuálním počítači.

![scénář BI IAS se 1 virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>Dva virtuální počítače
* Služba Analysis Services, služby Reporting Services a hello databázového stroje SQL Server na jeden virtuální počítač. Toto nasazení obsahuje hello databáze serveru sestav.
* Zdroje dat na druhý virtuální počítač. Hello druhý virtuální počítač obsahuje databázového stroje SQL Server jako zdroj dat.

![scénář BI iaas s 2 virtuální počítače](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Smíšená Azure – data v databázi Azure SQL
* Služba Analysis Services, služby Reporting Services a hello databázového stroje SQL Server na jeden virtuální počítač. Toto nasazení obsahuje hello databáze serveru sestav.
* Zdroj dat je Azure SQL database.

![virtuální počítač scénáře BI iaas a AzureSQL jako zdroj dat](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Hybridní – data místně
* V tomto příkladu nasazení služby Analysis Services, služby Reporting Services a hello databázového stroje SQL Server spustit na jednom virtuálním počítači. hostitelé virtuálních počítačů Hello hello databáze serveru sestav. Hello virtuální počítač je připojený k tooan místní domény prostřednictvím virtuálních sítí Azure nebo některých jiných tunelového propojení řešení VPN.
* Zdroj dat je místně.

![BI iaas scénáře virtuálních počítačů a místní zdroje dat](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>Konfigurace služby Reporting Services v nativním režimu
image Galerie Hello virtuálního počítače pro SQL Server obsahuje Reporting Services na nativní režim nainstalovaná, ale není nakonfigurovaný server sestav hello. Hello kroky v této části nakonfigurovat server sestav služby Reporting Services hello. Podrobné informace o konfiguraci Reporting Services na nativní režim, najdete v části [instalace sestav Reporting Services nativní režim serveru (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx).

> [!NOTE]
> Podobně jako obsah, který používá server sestav hello tooconfigure skripty prostředí Windows PowerShell, najdete v části [tooCreate použít PowerShell Azure virtuálních počítačů s nativní režim serveru sestav](../classic/ps-sql-report.md).

### <a name="connect-toohello-virtual-machine-and-start-hello-reporting-services-configuration-manager"></a>Připojení toohello virtuálního počítače a spuštění hello Správce konfigurace služby Reporting Services
Existují dvě běžné pracovní postupy pro připojování tooan virtuální počítač Azure:

* tooconnect v hello, klikněte na název hello hello virtuálního počítače a potom klikněte na **Connect**. Otevře připojení ke vzdálené ploše a název počítače hello se automaticky vyplní.
  
    ![připojit tooazure virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)
* Připojte toohello virtuálního počítače s Windows připojení ke vzdálené ploše. V uživatelském rozhraní hello hello vzdálené plochy:
  
  1. Typ hello **název cloudové služby** jako název počítače hello.
  2. Zadejte dvojtečkou (:) a číslem veřejného portu, který je nakonfigurován pro hello vzdálené plochy koncový bod TCP hello.
     
      MyService.cloudapp.NET:63133
     
      Další informace najdete v tématu [co je Cloudová služba?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).


**Spuštění, vytváření sestav služby Configuration Manager**

V **systému Windows Server 2012/2016**:

1. Z hello **spustit** zadejte **služby Reporting Services** toosee seznam aplikací.
2. Klikněte pravým tlačítkem na **Správce konfigurace služby Reporting Services** a klikněte na tlačítko **spustit jako správce**.

V **systému Windows Server 2008 R2**:

1. Klikněte na tlačítko **spustit**a potom klikněte na **všechny programy**.
2. Klikněte na tlačítko **Microsoft SQL Server 2016**.
3. Klikněte na tlačítko **konfigurační nástroje**.
4. Klikněte pravým tlačítkem na **Správce konfigurace služby Reporting Services** a klikněte na tlačítko **spustit jako správce**.

nebo:

1. Klikněte na tlačítko **spustit**.
2. V hello **Prohledat programy a soubory** typ dialogu **služby reporting services**. Pokud hello virtuálních počítačů se systémem Windows Server 2012, zadejte **služby reporting services** na obrazovce Start systému Windows Server 2012 hello.
3. Klikněte pravým tlačítkem na **Správce konfigurace služby Reporting Services** a klikněte na tlačítko **spustit jako správce**.
   
    ![Vyhledejte Správce konfigurace služby ssrs](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Konfigurace služby Reporting Services
**Účet služby a adresu URL webové služby:**

1. Ověřte hello **název serveru** je hello název místního serveru a klikněte na tlačítko **Connect**.
2. Poznámka: hello prázdné **název databáze serveru sestav**. Hello databáze je vytvořen po dokončení konfigurace hello.
3. Ověřte hello **stav serveru sestav** je **Začínáme**. Pokud chcete tooverify hello služby ve Správci serveru systému Windows, je služba hello hello **SQL Server Reporting Services** služba systému Windows.
4. Klikněte na tlačítko **účet služby** a podle potřeby změňte účet hello. Pokud virtuální počítač hello slouží v prostředí připojený k doméně, hello integrovanou **ReportServer** účet je dostatečná. Další informace o účtu služby hello najdete v tématu [účet služby](https://msdn.microsoft.com/library/ms189964.aspx).
5. Klikněte na tlačítko **adresa URL webové služby** v levém podokně hello.
6. Klikněte na tlačítko **použít** tooconfigure hello výchozí hodnoty.
7. Poznámka: hello **adres webové služby Server sestav**. Všimněte si, hello výchozí port TCP je 80 a je součástí adresy URL hello. V pozdější fázi můžete vytvořit koncový bod Microsoft Azure virtuálního počítače pro hello port.
8. V hello **výsledky** podokně ověřte hello akce byla úspěšně dokončena.

**Databáze:**

1. Klikněte na tlačítko **databáze** v levém podokně hello.
2. Klikněte na tlačítko **změnit databázi**.
3. Ověřte **vytvořit novou databázi serveru sestav** je vybrána a pak klikněte na tlačítko Další.
4. Ověřte **název serveru** a klikněte na tlačítko **Test připojení**.
5. Pokud je výsledek hello **testovací připojení bylo úspěšné**, klikněte na tlačítko **OK** a pak klikněte na **Další**.
6. Poznámka: název databáze hello je **ReportServer** a hello **režim serveru sestav** je **nativní** klikněte **Další**.
7. Klikněte na tlačítko **Další** na hello **pověření** stránky.
8. Klikněte na tlačítko **Další** na hello **Souhrn** stránky.
9. Klikněte na tlačítko **Další** na hello **průběhu a dokončit** stránky.

**Webová adresa URL portálu nebo adresa URL správce sestav pro 2012 a 2014:**

1. Klikněte na tlačítko **adresy URL webového portálu**, nebo **adresa URL správce sestav** 2014 a 2012, v levém podokně hello.
2. Klikněte na tlačítko **Použít**.
3. V hello **výsledky** podokně ověřte hello akce byla úspěšně dokončena.
4. Klikněte na tlačítko **ukončení**.

Informace o oprávnění serveru sestav, naleznete v části [udělení oprávnění na serveru sestav, nativní režim](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-toohello-local-report-manager"></a>Procházet toohello místní správce sestav
tooverify hello konfigurace, správce tooreport Procházet na hello virtuálních počítačů.

1. Na hello virtuálního počítače spusťte Internet Explorer s oprávněními správce.
2. Procházejte toohttp://localhost/reports na hello virtuálních počítačů.

### <a name="tooconnect-tooremote-web-portal-or-report-manager-for-2014-and-2012"></a>tooConnect tooRemote webový portál nebo správce sestav pro 2014 a 2012
Pokud chcete tooconnect toohello webový portál nebo správce sestav pro 2014 a 2012 hello virtuálního počítače ze vzdáleného počítače, vytvořte nový virtuální počítač koncový bod TCP. Ve výchozím nastavení, server sestav hello naslouchá požadavkům HTTP na **port 80**. Pokud nakonfigurujete hello sestavy serveru adresy URL toouse jiný port, zadejte toto číslo portu v hello pokynů.

1. Vytvoření koncového bodu pro hello virtuálního počítače TCP Port 80. Další informace najdete v tématu, hello [koncové body virtuálního počítače a porty brány Firewall](#virtual-machine-endpoints-and-firewall-ports) v tomto dokumentu.
2. Otevřete port 80 v bráně firewall hello virtuálního počítače.
3. Procházet toohello webový portál nebo správce sestav, pomocí virtuální počítač Azure **název DNS** jako název serveru hello v adrese URL hello. Například:
   
    **Server sestav**: http://uebi.cloudapp.net/reportserver **webový portál**: http://uebi.cloudapp.net/reports
   
    [Configure a Firewall for Report Server Access](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="toocreate-and-publish-reports-toohello-azure-virtual-machine"></a>tooCreate a publikovat sestavy toohello virtuální počítač Azure
Hello následující tabulka shrnuje některé hello možnosti k dispozici toopublish existující sestavy ze serveru místní počítač toohello sestavy musí být hostované na hello virtuální počítač Microsoft Azure:

* **Tvůrce sestav**: hello virtuální počítač obsahuje hello kliknutím-jednou verzi Tvůrce sestav Microsoft SQL serveru pro SQL 2014 a 2012. toostart sestavy Tvůrce hello poprvé hello virtuálního počítače s SQL 2016:
  
  1. Spustíte prohlížeč s oprávněními správce.
  2. Procházet toohello webový portál, hello virtuálního počítače a vyberte hello **Stáhnout** ikonu v pravém horním hello.
  3. Vyberte **Tvůrce sestav**.
     
     Další informace najdete v tématu [spuštění Tvůrce sestav](https://msdn.microsoft.com/library/ms159221.aspx).
* **SQL Server Data Tools**: virtuálních počítačů: SQL Server Data Tools je nainstalovaná hello virtuálního počítače a může být použité toocreate **projekty serveru sestav** a sestavy na virtuálním počítači hello. SQL Server Data Tools můžete publikovat hello sestavy toohello serveru sestav na virtuálním počítači hello.
* **SQL Server Data Tools: Vzdálené**: V místním počítači, vytvoření projektu služby Reporting Services v SQL Server Data Tools, obsahující sestavy služby Reporting Services. Nakonfigurujte hello projektu tooconnect toohello adresa URL webové služby.
  
    ![Vlastnosti projektu rozšíření SSDT pro projekt služby SSRS](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)
* Vytvoření. Pevný disk VHD, který obsahuje sestavy a pak nahrajte a připojit jednotku hello.
  
  1. Vytvoření. Virtuální pevný disk pevného disku v místním počítači, který obsahuje sestavy.
  2. Vytvoření a nainstalujte certifikát pro správu.
  3. Nahrát tooAzure souboru virtuálního pevného disku hello pomocí rutiny Add-AzureVHD hello [vytvoření a nahrání virtuálního pevného disku serveru Windows tooAzure](../classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
  4. Připojte hello disku toohello virtuálního počítače.

## <a name="install-other-sql-server-services-and-features"></a>Instalace dalších služby SQL Server a funkcí
tooinstall další služby systému SQL Server, jako je služba Analysis Services v tabulkovém režimu, spusťte Průvodce instalací serveru SQL hello. Hello instalační soubory jsou na místním disku hello virtuálního počítače.

1. Klikněte na tlačítko **spustit** a pak klikněte na **všechny programy**.
2. Klikněte na tlačítko **Microsoft SQL Server 2016**, **Microsoft SQL Server 2014** nebo **Microsoft SQL Server 2012** a pak klikněte na **nástroje pro konfiguraci** .
3. Klikněte na tlačítko **centrum instalace SQL serveru**.

Nebo spusťte C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe nebo C:\SQLServer_11.0_full\setup.exe

> [!NOTE]
> Hello při prvním spuštění instalačního programu systému SQL Server, další instalační soubory stáhnout a vyžadují restart hello virtuálního počítače a restartování instalace systému SQL Server.
> 
> Pokud potřebujete toorepeatedly přizpůsobení bitové kopie hello vybraný hello virtuální počítač Microsoft Azure, zvažte vytvoření vlastní image SQL serveru. Funkce Analysis Services SysPrep bylo povoleno s SQL Server 2012 SP1 CU2. Další informace najdete v tématu [důležité informace týkající se instalace SQL serveru pomocí nástroje SysPrep](https://msdn.microsoft.com/library/ee210754.aspx) a [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).
> 
> 

### <a name="tooinstall-analysis-services-tabular-mode"></a>tooInstall tabulkovém režimu Analysis Services
Hello kroky v této části **shrnout** hello instalace služby Analysis Services tabulkovém režimu. Další informace najdete v tématu hello následující:

* [Instalace služby Analysis Services v tabulkovém režimu](https://msdn.microsoft.com/library/hh231722.aspx)
* [Tabulkové modelování (společnosti Adventure Works kurzu)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**tooInstall tabulkovém režimu Analysis Services:**

1. V Průvodci instalací systému SQL Server hello, klikněte na tlačítko **instalace** v levém podokně hello a pak klikněte na **samostatná instalace nové SQL server, nebo přidejte existující instalace funkce tooan**.
   
   * Pokud se zobrazí hello **vyhledat složku**, vyhledejte tooc:\SQLServer_13.0_full, c:\SQLServer_12.0_full nebo c:\SQLServer_11.0_full a pak klikněte na tlačítko **Ok**.
2. Klikněte na tlačítko **Další** na stránce aktualizace produktu hello.
3. Na hello **typ instalace** vyberte **nová instalace systému SQL Server** a klikněte na tlačítko **Další**.
4. Na hello **Role instalace** klikněte na tlačítko **instalace funkce SQL Server**.
5. Na hello **výběr funkce** klikněte na tlačítko **služby Analysis Services**.
6. Na hello **konfigurace Instance** stránky, zadejte popisný název, například **tabulkového** do **s názvem Instance** a **Instance Id** textu polí.
7. Na hello **konfigurace služby Analysis Services** vyberte **tabulkovém režimu**. Přidejte hello aktuálního uživatele toohello oprávnění ke správě seznamu.
8. Dokončete a zavřete průvodce instalací systému SQL Server hello.

## <a name="analysis-services-configuration"></a>Konfigurace služby Analysis Services
### <a name="remote-access-tooanalysis-services-server"></a>Vzdálený přístup tooAnalysis služby Server
Server služby Analysis Services podporuje pouze ověřování systému windows. tooaccess služby Analysis Services vzdáleně z klientské aplikace jako SQL Server Management Studio nebo SQL Server Data Tools hello virtuální počítač vyžaduje místní doméně připojené k tooyour toobe, pomocí virtuální sítě Azure. Další informace najdete v tématu [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md).

A **výchozí instance** služby Analysis Services naslouchá na portu TCP **2383**. Otevřete hello port v bráně firewall hello virtuálních počítačů. Clusteru s názvem instance služby Analysis Services také sleduje na portu **2383**.

Pro **pojmenovanou instanci** služby Analysis Services, není třeba hello služba SQL Server Browser toomanage port přístup. Hello SQL Server Browser výchozí konfigurace je port **. 2382**.

V bráně firewall hello virtuální počítače, otevřete port **. 2382** a vytvořte statické Analysis Services s názvem instance portu.

1. tooverify porty, které jsou již v použít na hello virtuálních počítačů a které proces používá hello porty, spusťte následující příkaz s oprávněními správce hello:
   
        netstat /ao
2. Použití SQL Server Management Studio toocreate statické služby Analysis Services s názvem instance port aktualizací, Port, hodnota v tabulkovém AS obecné vlastnosti instance. Další informace najdete v tématu hello "Použít pevný port pro výchozí nebo pojmenované instance" v [konfigurace brány Windows Firewall tooAllow hello Analysis Services přístup](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed).
3. Restartujte hello tabulkové instanci hello služby Analysis Services.

Další informace najdete v tématu, hello **koncové body virtuálního počítače a porty brány Firewall** v tomto dokumentu.

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>Koncové body virtuálního počítače a porty brány Firewall
Tento oddíl shrnuje tooopen toocreate a porty koncových bodů virtuálního počítače Microsoft Azure v hello virtuálního počítače brány firewall. Hello následující tabulka shrnuje hello **TCP** porty toocreate koncové body pro a hello tooopen porty v bráně firewall hello virtuálních počítačů.

* Pokud používáte jeden virtuální počítač a hello následující dvě položky jsou splněny, není nutné koncové body toocreate virtuálních počítačů a není nutné tooopen hello porty v bráně firewall hello na hello virtuálních počítačů.
  
  * Funkce SQL serveru toohello na hello virtuálního počítače není vzdáleně připojit. Navazování připojení ke vzdálené ploše toohello virtuálních počítačů a přístupu k funkcím systému SQL Server hello místně na hello virtuálního počítače není považováno za funkcí systému SQL Server toohello vzdáleného připojení.
  * Zapojit se hello virtuálních počítačů tooan místní domény prostřednictvím virtuálních sítí Azure nebo jiné řešení tunelového propojení sítě VPN.
* Pokud hello virtuální počítač není připojený k tooa domény, ale chcete připojit tooremotely toohello funkcí systému SQL Server na virtuálním počítači:
  
  * Otevřete hello porty v bráně firewall hello na hello virtuálních počítačů.
  * Vytvoření virtuálního počítače koncových bodů pro hello uvedeno porty (*).
* Pokud je virtuální počítač hello tooa připojené k doméně pomocí tunelového připojení sítě VPN, jako je například virtuální sítí Azure, pak hello koncových bodů nejsou potřeba. Ale otevřete hello porty v bráně firewall hello na hello virtuálních počítačů.
  
  | Port | Typ | Popis |
  | --- | --- | --- |
  | **80** |TCP |Server sestav vzdáleného přístupu (*). |
  | **1433** |TCP |SQL Server Management Studio (*). |
  | **1434** |UDP |SQL Server Browser. To je nutné, když hello virtuálních počítačů v tooa připojené k doméně. |
  | **2382** |TCP |SQL Server Browser. |
  | **2383** |TCP |Výchozí instance SQL Server Analysis Services a clusteru s názvem instance. |
  | **Uživatelem definované** |TCP |Vytvořte statické Analysis Services s názvem instance, port pro číslo portu, který zvolíte a pak odblokovat hello číslo portu v bráně firewall hello. |

Další informace o vytváření koncových bodů najdete v části hello následující:

* Vytvoření koncových bodů:[jak tooSet až koncové body tooa virtuální počítač](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* SQL Server: Najdete v části "Kompletní konfigurace kroky tooconnect toohello virtuálního počítače pomocí SQL Server Management Studio" hello [zřizování virtuálního počítače systému SQL Server na platformě Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).

Hello následující diagram znázorňuje hello porty tooopen v hello toofeatures vzdáleného přístupu tooallow brány firewall virtuálních počítačů a součástí v hello virtuálních počítačů.

![tooopen porty pro bi aplikace ve virtuálních počítačích Azure](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Zdroje
* Zkontrolujte hello zásady podpory pro použít v prostředí virtuálního počítače Azure hello serverového softwaru společnosti Microsoft. Hello následující téma shrnuje podporu pro funkce, například nástrojem BitLocker, Clustering převzetí služeb při selhání a vyrovnávání zatížení sítě. [Podpora Microsoft serveru software pro virtuální počítače Azure](http://support.microsoft.com/kb/2721672).
* [SQL Server na virtuálních počítačích Azure – přehled](../sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Zřizování virtuálního počítače systému SQL Server v Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md)
* [Jak tooAttach tooa datový Disk virtuálního počítače](../classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Migrace databáze tooSQL Server na virtuálním počítači Azure](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json)
* [Určení hello režim serveru instance Analysis Services](https://msdn.microsoft.com/library/gg471594.aspx)
* [Multidimenzionální modelování (společnosti Adventure Works kurzu)](https://technet.microsoft.com/library/ms170208.aspx)
* [Centru dokumentace Azure](https://azure.microsoft.com/documentation/)
* [Pomocí Power BI v hybridním prostředí](https://msdn.microsoft.com/library/dn798994.aspx)

> [!NOTE]
> [Odeslat zpětnou vazbu a kontaktních informací prostřednictvím připojení ke službě Microsoft SQL Server](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Komunitním obsahu
* [Správa databáze Azure SQL pomocí prostředí PowerShell](http://blogs.msdn.com/b/windowsazure/archive/2013/02/07/windows-azure-sql-database-management-with-powershell.aspx)

