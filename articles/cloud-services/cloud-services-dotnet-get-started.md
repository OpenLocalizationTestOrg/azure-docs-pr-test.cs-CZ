---
title: "aaaGet začít s Azure Cloud Services a technologií ASP.NET | Microsoft Docs"
description: "Zjistěte, jak toocreate vícevrstvé aplikace s použitím rozhraní ASP.NET MVC a Azure. Hello aplikace běží v cloudové službě a obsahuje webovou roli a roli pracovního procesu. Používá nástroj Entity Framework, službu Azure SQL Database a fronty a objekty blob služby Azure Storage."
services: cloud-services, storage
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: d7aa440d-af4a-4f80-b804-cc46178df4f9
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/15/2017
ms.author: adegeo
ms.openlocfilehash: 86271c29b79fad3f01f8ea0e88fd00c7aefc970c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-cloud-services-and-aspnet"></a>Začínáme s cloudovými službami Azure Cloud Services a technologií ASP.NET

## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak toocreate vícevrstvé aplikace .NET s front-endu, ASP.NET MVC a nasaďte ho tooan [cloudové služby Azure](cloud-services-choose-me.md). Hello používá aplikace [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279), hello [služby objektů Blob Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage)a hello [službu front Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern). Můžete [stažení projektu sady Visual Studio hello](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) z hello galerie kódů MSDN.

Hello kurz ukazuje, jak toobuild a spusťte hello aplikaci místně, jak toodeploy ho tooAzure a hello spuštění v cloudu a jak toobuild ho od začátku. Můžete začít tím, sestavíte od nuly a pak hello test a kroky nasazení Pokud dáváte přednost.

## <a name="contoso-ads-application"></a>Aplikace Contoso Ads
aplikace Hello je vývěsní Tabule pro inzerci. Uživatelé vytvářejí reklamu tak, že zadají text a odešlou obrázek. Vidí seznam reklam s obrázky miniatur a uvidí obrázek hello plné velikosti, když vyberou podrobnosti hello toosee služby ad.

![Seznam reklam](./media/cloud-services-dotnet-get-started/list.png)

aplikace Hello používá hello [fronty způsob práce zaměřený](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff zatížení hello náročná na prostředky procesoru práci při vytváření miniatur tooa back endovému procesu.

## <a name="alternative-architecture-websites-and-webjobs"></a>Alternativní architektura: weby a webové úlohy
Tento kurz ukazuje, jak toorun front-end i back-end v Azure cloud service. Alternativou je toorun hello front-endu v [webu Azure](/services/web-sites/) a používat hello [WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226) funkce (momentálně ve verzi preview) pro hello back-end. Kurz, který používá webové úlohy, najdete v části [začít pracovat s hello Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md). Informace o tom, jak toochoose hello služby, které nejlépe vyhovovat vašemu scénáři, najdete v části [porovnání webů Azure, cloudové služby a virtuální počítače](../app-service-web/choose-web-site-cloud-service-vm.md).

## <a name="what-youll-learn"></a>Co se dozvíte
* Jak tooenable počítači pro vývoj pro Azure nainstalováním hello Azure SDK.
* Jak toocreate sady Visual Studio cloudové služby projektu s webová role ASP.NET MVC a roli pracovního procesu.
* Jak tootest hello projekt cloudové služby místně, pomocí emulátoru úložiště Azure hello.
* Jak toopublish hello cloudu projektu tooan Azure Cloudová služba a testování pomocí účtu úložiště Azure.
* Jak tooupload souborů a uložit je do služby objektů Blob Azure hello.
* Jak toouse hello služby front Azure pro komunikaci mezi vrstvami.

## <a name="prerequisites"></a>Požadavky
Hello kurz předpokládá, že rozumíte [základní koncepty Azure cloud services](cloud-services-choose-me.md) například *webovou roli* a *role pracovního procesu* terminologie.  Také předpokládá, že víte, jak toowork s [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) nebo [webových formulářů](http://www.asp.net/web-forms/tutorials/aspnet-45/getting-started-with-aspnet-45-web-forms/introduction-and-overview) projekty v sadě Visual Studio. Hello ukázková aplikace používá MVC, ale většina kurzu hello platí také tooWeb formulářů.

Můžete spustit aplikace hello místně bez předplatného Azure, ale budete potřebovat jeden toodeploy hello aplikace toohello cloudu. Pokud nemáte účet, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A55E3C668) nebo [si zaregistrovat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A55E3C668).

Hello pokyny kurzu pracují s jedním z hello následující produkty:

* Visual Studio 2013
* Visual Studio 2015
* Visual Studio 2017

Pokud nemáte jednu z těchto, Visual Studio může být nainstalovaná automaticky při instalaci hello Azure SDK.

## <a name="application-architecture"></a>Architektura aplikace
Hello aplikace ukládá reklamy do databáze SQL pomocí Entity Framework Code First toocreate hello tabulky a přístup k datům hello. Pro každou reklamu hello hello databáze ukládá dvě adresy URL. jednu pro obrázek v plné velikosti a druhou pro miniaturu hello.

![Tabulka reklam](./media/cloud-services-dotnet-get-started/adtable.png)

Když uživatel odešle obrázek, front-end spuštěný hello ve webové roli ukládá hello bitové kopie v [objektů blob v Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), a ukládá hello ad informace v databázi hello s adresou URL, který odkazuje objekt blob toohello. Na hello stejný čas, zapíše zpráva tooan fronty Azure. Back-end proces spuštěný v roli pracovního procesu pravidelně dotazuje hello fronty pro nové zprávy. Když se objeví nová zpráva, hello role pracovního procesu vytvoří miniaturu obrázku a aktualizace hello miniatur databáze pole adresy URL pro tuto reklamu. Hello následující diagram znázorňuje interakci částí hello aplikace hello.

![Architektura Contoso Ads](./media/cloud-services-dotnet-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]

## <a name="download-and-run-hello-completed-solution"></a>Stažení a spuštění řešení hello byla dokončena
1. Stáhněte a rozbalte hello [dokončené řešení](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4).
2. Spusťte Visual Studio.
3. Z hello **soubor** nabídce zvolte **otevřít projekt**, přejděte toowhere jste si stáhli hello řešení a pak otevřete soubor řešení hello.
4. Stisknutím kombinace kláves CTRL + SHIFT + B toobuild hello řešení.

    Visual Studio ve výchozím nastavení automaticky obnoví obsah balíčku NuGet hello, který nebyl součástí hello *.zip* souboru. Pokud hello balíčky neobnoví, nainstalujte je ručně budete toohello **spravovat balíčky NuGet pro řešení** dialogové okno a kliknutím na tlačítko hello **obnovení** tlačítko v pravé horní hello.
5. V **Průzkumníku řešení**, ujistěte se, že **ContosoAdsCloudService** je vybrán jako spouštěný projekt hello.
6. Pokud používáte Visual Studio 2015 nebo vyšší, změňte připojovací řetězec SQL serveru hello v aplikaci hello *Web.config* souboru hello projektu ContosoAdsWeb a v hello *ServiceConfiguration.Local.cscfg* souboru projektu ContosoAdsCloudService hello. V každém případě změňte "(localdb) \v11.0" příliš "\MSSQLLocalDB (localdb)".
7. Stisknutím kombinace kláves CTRL + F5 toorun hello aplikace.

    Když spouštíte projekt cloudové služby místně, Visual Studio automaticky vyvolá hello Azure *emulátor služby výpočty* a Azure *emulátor úložiště*. Hello výpočetní emulátor používá počítače prostředky toosimulate hello webovou roli a prostředí role pracovního procesu. emulátor úložiště Hello používá [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) databáze toosimulate cloudového úložiště Azure.

    Hello prvním spuštění projektu cloudové služby trvá minutu pro hello emulátorů toostart nahoru. Pokud je spuštění emulátorů dokončené, hello výchozí prohlížeč otevře domovskou stránku toohello aplikace.

    ![Architektura Contoso Ads](./media/cloud-services-dotnet-get-started/home.png)
8. Klikněte na **Vytvořit reklamu**.
9. Zadejte nějaká testovací data a vyberte *.jpg* tooupload bitovou kopii a pak klikněte na tlačítko **vytvořit**.

    ![Vytvoření stránky](./media/cloud-services-dotnet-get-started/create.png)

    aplikace Hello přejde toohello indexovou stránku, ale nezobrazí miniaturu nové reklamy hello, protože toto zpracování ještě neproběhlo.
10. Chvíli počkejte a pak aktualizujte hello Index stránky toosee hello miniaturu.

     ![Indexová stránka](./media/cloud-services-dotnet-get-started/list.png)
11. Klikněte na tlačítko **podrobnosti** pro ad toosee hello plné velikosti bitové kopie.

     ![Stránka podrobností](./media/cloud-services-dotnet-get-started/details.png)

Jste byla spuštěna aplikace hello zcela v místním počítači, bez připojení toohello cloudu. Hello emulátor úložiště ukládá hello fronty a data objektů blob v databázi SQL serveru Express LocalDB a aplikace hello ukládá hello ad data do jiné databáze LocalDB. Databáze Entity Framework Code First automaticky vytvořený hello ad hello poprvé hello webové aplikace se pokusila tooaccess ho.

V následující části hello nakonfigurujete hello řešení toouse cloudové prostředky Azure pro fronty, objekty BLOB a databáze aplikace hello při spuštění v cloudu hello. Pokud jste chtěli toocontinue toorun místně, ale využívat cloudové prostředky úložiště a databáze, můžete to udělat. Ho stačí nastavit připojovací řetězec, které se zobrazí jak toodo.

## <a name="deploy-hello-application-tooazure"></a>Nasazení aplikace tooAzure hello
Můžete to udělat následující kroky toorun hello aplikaci v cloudu hello hello:

* Vytvoření cloudové služby Azure
* Vytvoření databáze SQL Azure
* Vytvoření účtu úložiště Azure
* Nakonfigurujte řešení toouse hello Azure SQL database při spuštění v Azure.
* Nakonfigurujte řešení toouse hello účtu úložiště Azure při spuštění v Azure.
* Nasaďte hello projektu tooyour cloudové služby Azure.

### <a name="create-an-azure-cloud-service"></a>Vytvoření cloudové služby Azure
Cloudové služby Azure je, že bude hello prostředí hello aplikace spuštěna.

1. V prohlížeči otevřete hello [portál Azure](https://portal.azure.com).
2. Klikněte na **Nový > Výpočty > Cloudová služba**.

3. Hello DNS název vstupní pole zadejte předponu adresy URL pro hello cloudové služby.

    Tato adresa URL má toobe jedinečný.  Budete zobrazí chybové hlášení, pokud zvolenou předponu hello je již používán.
4. Zadejte novou skupinu prostředků pro službu hello. Klikněte na tlačítko **vytvořit nový** a potom zadejte název hello prostředků skupiny vstupní pole, jako je například CS_contososadsRG.

5. Zvolte hello oblasti, kde chcete toodeploy hello aplikace.

    Toto pole určuje datové centrum, které bude hostovat vaše cloudové služby. Pro produkční aplikace vyberte hello oblast nejbližší tooyour zákazníků. V tomto kurzu zvolte nejbližší tooyou hello oblast.
5. Klikněte na možnost **Vytvořit**.

    V hello následující bitové kopie Cloudová služba se vytvoří s hello CSvccontosoads.cloudapp.net adresy URL.

    ![Nová cloudová služba](./media/cloud-services-dotnet-get-started/newcs.png)

### <a name="create-an-azure-sql-database"></a>Vytvoření databáze SQL Azure
Když hello aplikace běží v cloudu hello, použije databáze založené na cloudu.

1. V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **nový > databáze > databáze SQL**.
2. V hello **název databáze** zadejte *contosoads*.
3. V hello **skupiny prostředků**, klikněte na tlačítko **použít existující** a skupině prostředků vyberte hello používá pro hello cloudové služby.
4. Následující obrázek, klikněte v hello **Server – nakonfigurujte požadovaná nastavení** a **vytvořit nový server**.

    ![Server toodatabase tunelového propojení](./media/cloud-services-dotnet-get-started/newdb.png)

    Případně pokud vaše předplatné už má server, můžete vybrat tento server z rozevíracího seznamu hello.
5. V hello **název serveru** zadejte *csvccontosodbserver*.

6. Zadejte **Přihlašovací jméno** a **Heslo** správce.

    Pokud jste vybrali možnost **Vytvořit nový server**, nebudete zadávat existující název a heslo. Zadáváte nové jméno a heslo, že definujete teď toouse později při přístupu k databázi hello. Pokud jste vybrali serveru, který jste vytvořili dříve, budete vyzváni pro hello heslo toohello účet správce jste již vytvořili.
7. Vyberte stejný hello **umístění** kterou jste zvolili pro cloudové služby hello.

    Když hello cloudové služby a databáze jsou v různých datových centrech (různých oblastech), zvýší se latence a vám bude účtována šířka pásma mimo datové centrum hello. Šířka pásma v rámci datového centra je zdarma.
8. Zkontrolujte **povolit službám azure tooaccess server**.
9. Klikněte na tlačítko **vyberte** pro nový server hello.

    ![Nový server služby SQL Database](./media/cloud-services-dotnet-get-started/newdbserver.png)
10. Klikněte na možnost **Vytvořit**.

### <a name="create-an-azure-storage-account"></a>Vytvoření účtu úložiště Azure
Účet úložiště Azure poskytuje prostředky pro ukládání dat front a objektů blob v cloudu hello.

V reálné aplikaci byste obvykle vytvořili samostatné účty pro data aplikací a pro data protokolování a samostatné účty pro testovací data a pro produkční data. V tomto kurzu budete používat jenom jeden účet.

1. V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **nový > úložiště > účet úložiště – objekt blob, soubor, tabulka, fronta**.
2. V hello **název** zadejte předponu adresy URL.

    Tato předpona a hello text, který se zobrazí pod polem hello bude účet úložiště tooyour jedinečnou adresu URL hello. Pokud zadáte předponu hello již byl použit někdo jiný, budete mít toochoose jinou předponu.
3. Sada hello **modelu nasazení** příliš*Classic*.

4. Sada hello **replikace** rozevírací seznam příliš**místně redundantního úložiště**.

    Pokud geografická replikace je povoleno pro účet úložiště, je hello uložený obsah případě významnější havárie v primárním umístění hello replikované tooa sekundárního datacentra tooenable převzetí služeb při selhání. Geografická replikace může způsobit dodatečné náklady. Pro testovací a vývojové účty obecně nechcete, aby toopay za geografickou replikaci. Další informace naleznete v článku o [vytvoření, správě nebo odstranění účtu úložiště](../storage/common/storage-create-storage-account.md).

5. V hello **skupiny prostředků**, klikněte na tlačítko **použít existující** a skupině prostředků vyberte hello používá pro hello cloudové služby.
6. Sada hello **umístění** rozevíracího seznamu toohello stejné oblasti, které jste zvolili pro cloudové služby hello.

    Když jsou hello cloudové služby a účet úložiště v různých datových centrech (různých oblastech), zvýší se latence a vám bude účtována šířka pásma mimo datové centrum hello. Šířka pásma v rámci datového centra je zdarma.

    Skupina vztahů Azure nabízí mechanismus toominimize hello vzdálenosti mezi prostředky v datovém centru, můžete tak omezit latenci. V tomto kurzu skupinu vztahů nepoužíváme. Další informace najdete v tématu [jak tooCreate spřažení skupiny v Azure](http://msdn.microsoft.com/library/jj156209.aspx).
7. Klikněte na možnost **Vytvořit**.

    ![Nový účet úložiště](./media/cloud-services-dotnet-get-started/newstorage.png)

    Hello obrázku se vytvoří účet úložiště s adresou URL hello `csvccontosoads.core.windows.net`.

### <a name="configure-hello-solution-toouse-your-azure-sql-database-when-it-runs-in-azure"></a>Konfigurace řešení toouse hello Azure SQL database při spuštění v Azure
Hello webový projekt a hello projekt role pracovního procesu každý má vlastní připojovací řetězec databáze a každý musí toopoint toohello Azure SQL database při spuštění aplikace hello v Azure.

Budete používat [transformaci Web.config](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) pro hello webovou roli a nastavení prostředí cloudové služby pro roli pracovního procesu hello.

> [!NOTE]
> V této části a hello další části uložíte přihlašovací údaje v souborech projektu. [Citlivá data neukládejte do veřejných úložišť  zdrojového kódu](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).
>
>

1. Otevřete v projektu ContosoAdsWeb hello hello *Web.Release.config* transformační soubor pro aplikaci hello *Web.config* souboru, odstraňte blok komentáře hello, který obsahuje `<connectionStrings>` prvku a vložení Následující kód na příslušné místo Hello.

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="{connectionstring}"
        providerName="System.Data.SqlClient" xdt:Transform="SetAttributes" xdt:Locator="Match(name)"/>
    </connectionStrings>
    ```

    Soubor hello nechte otevřený pro úpravy.
2. V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **databází SQL** v levém podokně hello, klikněte na hello databáze, které jste vytvořili v tomto kurzu a pak klikněte na tlačítko **zobrazit připojovací řetězce**.

    ![Zobrazení připojovacích řetězců](./media/cloud-services-dotnet-get-started/showcs.png)

    Hello portál zobrazí připojovací řetězce s zástupný symbol pro hello heslo.

    ![Připojovací řetězce](./media/cloud-services-dotnet-get-started/connstrings.png)
3. V hello *Web.Release.config* transformační soubor, odstraňte `{connectionstring}` a vložte jeho místní hello připojovací řetězec ADO.NET z hello portálu Azure.
4. V hello připojovací řetězec, který jste vložili do hello *Web.Release.config* transformační soubor, nahraďte `{your_password_here}` hello heslem, které jste vytvořili pro novou databázi SQL hello.
5. Uložte soubor hello.  
6. Vyberte a zkopírujte připojovací řetězec hello (bez okolních uvozovek hello) pro použití v následujících krocích konfigurace projektu role pracovního procesu hello hello.
7. V **Průzkumníku řešení**v části **role** v hello projekt cloudové služby, klikněte pravým tlačítkem na **ContosoAdsWorker** a pak klikněte na tlačítko **vlastnosti**.

    ![Vlastnosti rolí](./media/cloud-services-dotnet-get-started/rolepropertiesworker.png)
8. Klikněte na tlačítko hello **nastavení** kartě.
9. Změna **konfigurace služby** příliš**cloudu**.
10. Vyberte hello **hodnotu** pole pro hello `ContosoAdsDbConnectionString` nastavení a potom vložte hello připojovací řetězec, který jste zkopírovali z předchozí části kurzu hello hello.

     ![Připojovací řetězec databáze pro roli pracovního procesu](./media/cloud-services-dotnet-get-started/workerdbcs.png)
11. Uložte provedené změny.  

### <a name="configure-hello-solution-toouse-your-azure-storage-account-when-it-runs-in-azure"></a>Konfigurace účtu úložiště Azure hello řešení toouse při spuštění v Azure
Připojovací řetězce k účtu úložiště Azure pro projekt hello webové role i projekt role pracovního procesu hello jsou uloženy v nastavení prostředí v projektu cloudové služby hello. Pro každý projekt je samostatnou sadu nastavení toobe používá při spuštění aplikace hello místně a při spuštění v cloudu hello. Nastavení cloudového prostředí pro webové a pracovní projekty role hello budete aktualizovat.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **ContosoAdsWeb** pod **role** v hello **ContosoAdsCloudService** projektu a pak klikněte na **Vlastnosti**.

    ![Vlastnosti rolí](./media/cloud-services-dotnet-get-started/roleproperties.png)
2. Klikněte na tlačítko hello **nastavení** kartě. V hello **konfigurace služby** rozevíracího seznamu vyberte **cloudu**.

    ![Konfigurace cloudu](./media/cloud-services-dotnet-get-started/sccloud.png)
3. Vyberte hello **StorageConnectionString** položku a zobrazí se třemi tečkami (**...** ) tlačítko na pravém konci řádku hello hello. Klikněte na tlačítko hello třemi tečkami tlačítko tooopen hello **vytvoření připojovacího řetězce účet úložiště** dialogové okno.

    ![Otevření pole Vytvoření připojovacího řetězce](./media/cloud-services-dotnet-get-started/opencscreate.png)
4. V hello **vytvoření připojovacího řetězce úložiště** dialogové okno, klikněte na tlačítko **vaše předplatné**, zvolte hello účet úložiště, který jste vytvořili dříve a pak klikněte na tlačítko **OK**. Pokud ještě nejste přihlášeni, budete vyzváni k zadání přihlašovacích údajů k účtu Azure.

    ![Vytvoření připojovacího řetězce k úložišti](./media/cloud-services-dotnet-get-started/createstoragecs.png)
5. Uložte provedené změny.
6. Postupujte podle hello stejný postup, který jste použili pro hello `StorageConnectionString` připojovací řetězec tooset hello `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` připojovací řetězec.

    Tento připojovací řetězec se používá k protokolování.
7. Postupujte podle hello stejný postup, který jste použili pro hello **ContosoAdsWeb** role tooset oba připojovací řetězce pro hello **ContosoAdsWorker** role. Nezapomeňte tooset **konfigurace služby** příliš**cloudu**.

nastavení prostředí role Hello, které jste nakonfigurovali pomocí rozhraní Visual Studia hello jsou uloženy v hello následující soubory v projektu ContosoAdsCloudService hello:

* *ServiceDefinition.csdef* – definuje názvy nastavení hello.
* *ServiceConfiguration.Cloud.cscfg* – poskytuje hodnoty pro spuštění aplikace hello v cloudu hello.
* *ServiceConfiguration.Local.cscfg* – poskytuje hodnoty pro spuštění aplikace hello místně.

Například hello ServiceDefinition.csdef obsahuje následující definice hello:

```xml
<ConfigurationSettings>
    <Setting name="StorageConnectionString" />
    <Setting name="ContosoAdsDbConnectionString" />
</ConfigurationSettings>
```

A hello *ServiceConfiguration.Cloud.cscfg* soubor obsahuje hello hodnoty, které jste zadali pro tato nastavení v sadě Visual Studio.

```xml
<Role name="ContosoAdsWorker">
    <Instances count="1" />
    <ConfigurationSettings>
        <Setting name="StorageConnectionString" value="{yourconnectionstring}" />
        <Setting name="ContosoAdsDbConnectionString" value="{yourconnectionstring}" />
        <!-- other settings not shown -->

    </ConfigurationSettings>
    <!-- other settings not shown -->

</Role>
```

Hello `<Instances>` nastavení určuje hello počet virtuálních počítačů, které Azure spustí pracovní hello kód role na. Hello [další kroky](#next-steps) část obsahuje odkazy toomore informace o škálování cloudové služby

### <a name="deploy-hello-project-tooazure"></a>Nasazení projektu tooAzure hello
1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **ContosoAdsCloudService** cloudový projekt a potom vyberte **publikovat**.

   ![Publikování nabídky](./media/cloud-services-dotnet-get-started/pubmenu.png)
2. V hello **přihlášení** krok hello **publikování aplikaci Azure** průvodce, klikněte na tlačítko **Další**.

    ![Krok Přihlásit se](./media/cloud-services-dotnet-get-started/pubsignin.png)
3. V hello **nastavení** krok hello průvodce, klikněte na tlačítko **Další**.

    ![Krok Nastavení](./media/cloud-services-dotnet-get-started/pubsettings.png)

    Výchozí nastavení v hello Hello **Upřesnit** kartě jsou pro tento kurz. Informace o kartě Upřesnit hello najdete v tématu [Průvodci publikováním aplikace Azure](http://msdn.microsoft.com/library/hh535756.aspx).
4. V hello **Souhrn** krok, klikněte na tlačítko **publikovat**.

    ![Krok Souhrn](./media/cloud-services-dotnet-get-started/pubsummary.png)

   Hello **protokol činnosti Azure** otevře se okno v sadě Visual Studio.
5. Klikněte na tlačítko hello šipku vpravo ikonu tooexpand hello podrobnosti o nasazení.

    Hello nasazení může trvat až minut too5 nebo další toocomplete.

    ![Okno Protokol činnosti Azure](./media/cloud-services-dotnet-get-started/waal.png)
6. Po dokončení hello stav nasazení, klikněte na tlačítko hello **adresa URL webové aplikace** toostart hello aplikace.
7. Nyní můžete otestovat aplikace hello vytváření, zobrazením a upravit některé reklamy, stejně jako při spuštění aplikace hello místně.

> [!NOTE]
> Pokud jste dokončení testování, odstraňte nebo zastavte cloudovou službu hello. I v případě, že nepoužíváte hello cloudové služby, protože jsou pro ně vyhrazené prostředky virtuálního počítače totiž nabíhají poplatky. A pokud je necháte spuštěné, může každý, kdo najde vaši adresu URL, vytvářet a zobrazovat reklamy. V hello [portál Azure](https://portal.azure.com), přejděte toohello **přehled** pro cloudové služby a pak klikněte hello **odstranit** tlačítko hello horní části stránky hello. Pokud chcete zabránit tootemporarily ostatním v přístupu k webu hello, klikněte na tlačítko **Zastavit** místo. V takovém případě budou poplatky dál tooaccrue. Pokud je již nepotřebujete, můžete provést podobné hello toodelete postup SQL databáze a účet úložiště.
>
>

## <a name="create-hello-application-from-scratch"></a>Vytvoření aplikace hello od začátku
Pokud jste ještě nestáhli [aplikace hello Dokončit](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4), udělejte to teď. Budete kopírovat soubory z projektu hello stáhli do nového projektu hello.

Vytvoření aplikace Contoso Ads hello zahrnuje hello následující kroky:

* Vytvoření řešení cloudové služby Visual Studio
* Aktualizace a přidání balíčků NuGet
* Nastavení odkazů projektu
* Konfigurace připojovacích řetězců
* Přidání souborů s kódy

Po vytvoření hello řešení zkontrolujete hello kód, který je jedinečný toocloud služby projekty a Azure objekty BLOB a fronty.

### <a name="create-a-cloud-service-visual-studio-solution"></a>Vytvoření řešení cloudové služby Visual Studio
1. V sadě Visual Studio, vyberte **nový projekt** z hello **souboru** nabídky.
2. V levém podokně hello hello **nový projekt** dialogové okno, rozbalte seznam **Visual C#** a zvolte **cloudu** šablony a potom zvolte hello **CloudováslužbaAzure** šablony.
3. Název hello projektu a řešení ContosoAdsCloudService a potom klikněte na **OK**.

    ![Nový projekt](./media/cloud-services-dotnet-get-started/newproject.png)
4. V hello **nová Cloudová služba Azure** dialogové okno pole, přidejte webovou roli a roli pracovního procesu. Název hello webovou roli ContosoAdsWeb a roli pracovního procesu hello název ContosoAdsWorker. (Použijte ikonu tužky hello v hello pravém podokně toochange hello výchozích názvů rolí hello.)

    ![Nový projekt cloudové služby](./media/cloud-services-dotnet-get-started/newcsproj.png)
5. Až se zobrazí hello **nový projekt ASP.NET** dialogové okno pro webovou roli hello, zvolte šablonu MVC hello a pak klikněte na tlačítko **změna ověřování**.

    ![Změna ověřování](./media/cloud-services-dotnet-get-started/chgauth.png)
6. V hello **změna ověřování** dialogovém okně vyberte **bez ověřování**a potom klikněte na **OK**.

    ![Bez ověřování](./media/cloud-services-dotnet-get-started/noauth.png)
7. V hello **nový projekt ASP.NET** dialogové okno, klikněte na tlačítko **OK**.
8. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello řešení (ne hello projektů) a zvolte **přidat – nový projekt**.
9. V hello **přidat nový projekt** dialogovém okně vyberte **Windows** pod **Visual C#** v levém podokně text hello a potom klikněte na hello **knihovny tříd** Šablona.  
10. Název projektu hello *ContosoAdsCommon*a potom klikněte na **OK**.

    Je nutné tooreference hello kontextu a hello datový model Entity Framework z projektů webové a pracovní role. Jako alternativu můžete definovat třídy související s EF hello v hello projekt webové role a odkazovat na takový projekt z projektu role pracovního procesu hello. Ale v hello alternativní způsob, váš projekt role pracovního procesu být referenční tooweb sestavení, která nepotřebuje.

### <a name="update-and-add-nuget-packages"></a>Aktualizace a přidání balíčků NuGet
1. Otevřete hello **spravovat balíčky NuGet** dialogové okno pro řešení hello.
2. Hello horní části okna hello, vyberte **aktualizace**.
3. Vyhledejte hello *WindowsAzure.Storage* balíčku a pokud je v seznamu hello, vyberte ho a vyberte hello webové a pracovní projekty tooupdate ho a pak klikněte na tlačítko **aktualizace**.

    Klientská knihovna pro úložiště Hello je aktualizován častěji než šablony projektů Visual Studio, tak, že hello verze budete často zjistíte v toobe nově vytvořený projekt musí aktualizovat.
4. Hello horní části okna hello, vyberte **Procházet**.
5. Najde hello *EntityFramework* NuGet balíček a nainstalujte ho do všech tří projektů.
6. Najde hello *Microsoft.WindowsAzure.ConfigurationManager* NuGet balíčku a nainstalujte ho do projektu role pracovního procesu hello.

### <a name="set-project-references"></a>Nastavení odkazů na projekty
1. V hello projektu ContosoAdsWeb nastavte odkaz na projekt ContosoAdsCommon toohello. Klikněte pravým tlačítkem na projekt ContosoAdsWeb hello a pak klikněte na **odkazy** - **přidat odkazy**. V hello **správce odkazů** dialogové okno, vyberte **řešení – projekty** v levém podokně hello vyberte **ContosoAdsCommon**a pak klikněte na tlačítko **OK**.
2. V hello projektu ContosoAdsWorker nastavte odkaz na projekt ContosAdsCommon toohello.

    ContosoAdsCommon bude obsahovat hello Entity Framework datový model a třídu kontextu, který se použije i hello front-end a back-end.
3. V hello projektu ContosoAdsWorker nastavte odkaz příliš`System.Drawing`.

    Toto sestavení používá toothumbnails bitové kopie hello tooconvert back-end.

### <a name="configure-connection-strings"></a>Konfigurace připojovacích řetězců
V této části budete konfigurovat službu Azure Storage a připojovací řetězce SQL pro místní testování. Pokyny k nasazení Hello v kurzu hello vysvětlují, jak tooset až hello připojovací řetězce pro při hello aplikace běží v cloudu hello.

1. V projektu ContosoAdsWeb hello hello otevřete aplikační soubor Web.config a vložení hello následující `connectionStrings` element po hello `configSections` elementu.

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    Pokud používáte Visual Studio 2015 nebo vyšší, nahraďte text „v11.0“ textem „MSSQLLocalDB“.
2. Uložte provedené změny.
3. V projektu ContosoAdsCloudService hello, klikněte pravým tlačítkem na ContosoAdsWeb pod **role**a potom klikněte na **vlastnosti**.

    ![Vlastnosti rolí](./media/cloud-services-dotnet-get-started/roleproperties.png)
4. V hello **ContosAdsWeb [Role]** okně vlastností klikněte na tlačítko hello **nastavení** a pak klikněte **přidat nastavení**.

    Nechte **konfigurace služby** nastavit příliš**všechny konfigurace**.
5. Přidejte nastavení s názvem *StorageConnectionString*. Nastavit **typ** příliš*ConnectionString*a nastavte **hodnotu** příliš*UseDevelopmentStorage = true*.

    ![Nový připojovací řetězec](./media/cloud-services-dotnet-get-started/scall.png)
6. Uložte provedené změny.
7. Postupujte podle hello stejný postup tooadd připojovací řetězec úložiště do vlastností role ContosoAdsWorker hello.
8. Stále v hello **ContosoAdsWorker [Role]** vlastnosti – okno, přidejte další připojovací řetězec:

   * Název: ContosoAdsDbConnectionString
   * Typ: Řetězec
   * Hodnota: Vložte hello stejný připojovací řetězec použitý pro projekt webové role hello. (hello následující ukázka je pro Visual Studio 2013. Nezapomeňte toochange hello zdroj dat Pokud tento příklad kopírujete a používáte Visual Studio 2015 nebo vyšší.)

       ```
       Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;
       ```

### <a name="add-code-files"></a>Přidání souborů s kódy
V této části zkopírujete soubory kódu z řešení hello stáhli do nové řešení hello. Hello následující části se zobrazí a vysvětlí klíčová místa tohoto kódu.

tooadd soubory tooa projektu nebo složky, klikněte pravým tlačítkem na projekt hello nebo složku a klikněte na tlačítko **přidat** - **existující položka**. Vyberte soubory hello a potom klikněte na **přidat**. Pokud se zobrazí dotaz, zda chcete, aby tooreplace existující soubory, klikněte na tlačítko **Ano**.

1. V projektu ContosoAdsCommon hello odstranit hello *Class1.cs* souboru a přidejte do jeho místní hello *Ad.cs* a *ContosoAdscontext.cs* stáhnout soubory z hello projektu.
2. V projektu ContosoAdsWeb hello přidejte následující soubory z projektu hello stáhli hello.

   * *Global.asax.cs*.  
   * V hello *Views\Shared* složky:  *\_Layout.cshtml*.
   * V hello *Views\Home* složky: *Index.cshtml*.
   * V hello *řadiče* složky: *AdController.cs*.
   * V hello *Views\Ad* složky (nejprve vytvořte složku hello): pět *.cshtml* soubory.
3. V projektu ContosoAdsWorker hello přidat *WorkerRole.cs* z hello stáhli projektu.

Teď můžete sestavit a spustit aplikaci hello podle pokynů v kurzu hello a hello aplikace bude používat místní databáze a prostředky emulátoru úložiště.

Hello následující části popisují kód hello související tooworking s hello prostředí Azure, objekty BLOB a fronty. V tomto kurzu nevysvětluje, jak řadiče MVC toocreate a zobrazení pomocí generování uživatelského rozhraní, jak toowrite kódu rozhraní Entity Framework, funguje s databází serveru SQL Server nebo hello nepopisuje základy asynchronního programování v technologii ASP.NET 4.5. Informace o těchto tématech najdete v tématu hello následující prostředky:

* [Začínáme s MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [Začínáme s EF 6 a MVC 5](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc)
* [Úvod tooasynchronous programování v rozhraní .NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).

### <a name="contosoadscommon---adcs"></a>ContosoAdsCommon – Ad.cs
Hello soubor Ad.cs definuje výčet kategorií reklam a třídu entity objektů POCO pro informace o reklamách.

```csharp
public enum Category
{
    Cars,
    [Display(Name="Real Estate")]
    RealEstate,
    [Display(Name = "Free Stuff")]
    FreeStuff
}

public class Ad
{
    public int AdId { get; set; }

    [StringLength(100)]
    public string Title { get; set; }

    public int Price { get; set; }

    [StringLength(1000)]
    [DataType(DataType.MultilineText)]
    public string Description { get; set; }

    [StringLength(1000)]
    [DisplayName("Full-size Image")]
    public string ImageURL { get; set; }

    [StringLength(1000)]
    [DisplayName("Thumbnail")]
    public string ThumbnailURL { get; set; }

    [DataType(DataType.Date)]
    [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
    public DateTime PostedDate { get; set; }

    public Category? Category { get; set; }
    [StringLength(12)]
    public string Phone { get; set; }
}
```

### <a name="contosoadscommon---contosoadscontextcs"></a>ContosoAdsCommon – ContosoAdsContext.cs
Hello třída ContosoAdsContext Určuje, zda text hello Ad třída se používá v kolekci DbSet, kterou Entity Framework uloží do databáze SQL.

```csharp
public class ContosoAdsContext : DbContext
{
    public ContosoAdsContext() : base("name=ContosoAdsContext")
    {
    }
    public ContosoAdsContext(string connString)
        : base(connString)
    {
    }
    public System.Data.Entity.DbSet<Ad> Ads { get; set; }
}
```

Hello třída má dva konstruktory. Hello první z nich je používané hello webovým projektem a určuje název připojovacího řetězce, který je uložený v souboru Web.config hello hello. Hello druhý konstruktor vám umožňuje toopass v hello samotný připojovací řetězec používaný hello projekt role pracovního procesu, protože sám nemá soubor Web.config. Už jste viděli dříve tento připojovací řetězec uložil, kde uvidíte později jak hello kód načte hello připojovací řetězec při vytvoření instance třídy DbContext hello.

### <a name="contosoadsweb---globalasaxcs"></a>ContosoAdsWeb – Global.asax.cs
Kód, který je volána z hello `Application_Start` metoda vytvoří *bitové kopie* kontejner objektů blob a *bitové kopie* fronty, pokud ještě neexistují. To zajistí, že při každém spuštění pomocí nového účtu úložiště, nebo pomocí emulátoru úložiště hello na nový počítač, hello požadovaný kontejner objektů blob a fronty se vytvoří automaticky.

Hello kód získá přístup k účtu úložiště pro toohello pomocí připojovacího řetězce úložiště hello z hello *.cscfg* souboru.

```csharp
var storageAccount = CloudStorageAccount.Parse
    (RoleEnvironment.GetConfigurationSettingValue("StorageConnectionString"));
```

Potom získá odkaz na toohello *bitové kopie* kontejner objektů blob, vytvoří kontejner hello, pokud ještě neexistuje, a nastaví přístupová oprávnění na nový kontejner hello. Ve výchozím nastavení nové kontejnery jenom klientům s účtem úložiště objektů BLOB tooaccess přihlašovací údaje. Hello webu musí hello toobe veřejné objekty BLOB tak, aby se může zobrazit obrázky pomocí adres URL objekty BLOB tento bod toohello bitové kopie.

```csharp
var blobClient = storageAccount.CreateCloudBlobClient();
var imagesBlobContainer = blobClient.GetContainerReference("images");
if (imagesBlobContainer.CreateIfNotExists())
{
    imagesBlobContainer.SetPermissions(
        new BlobContainerPermissions
        {
            PublicAccess =BlobContainerPublicAccessType.Blob
        });
}
```

Podobný kód získá odkaz na toohello *bitové kopie* fronty a vytvoří novou frontu. V tomto případě nejsou nutná žádná oprávnění.

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
var imagesQueue = queueClient.GetQueueReference("images");
imagesQueue.CreateIfNotExists();
```

### <a name="contosoadsweb---layoutcshtml"></a>ContosoAdsWeb – \_Layout.cshtml
Hello *_Layout.cshtml* soubor nastaví název aplikace hello v hello záhlaví a zápatí a vytvoří položku nabídky "Reklamy".

### <a name="contosoadsweb---viewshomeindexcshtml"></a>ContosoAdsWeb – Views\Home\Index.cshtml
Hello *Views\Home\Index.cshtml* souboru zobrazuje kategorie odkazy na domovskou stránku hello. Hello odkazy předají celočíselnou hodnotu hello hello `Category` výčtu na stránce služby Active Directory Index toohello proměnné řetězce dotazu.

```razor
<li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
<li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
<li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
<li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>
```

### <a name="contosoadsweb---adcontrollercs"></a>ContosoAdsWeb – AdController.cs
V hello *AdController.cs* soubor, hello konstruktor volání hello `InitializeStorage` metoda toocreate Klientská knihovna pro úložiště Azure objekty, které poskytují rozhraní API pro práci s objekty BLOB a frontami.

Pak hello kód získá odkaz na toohello *bitové kopie* kontejner objektů blob, jako jste viděli dříve v *Global.asax.cs*. Během toho nastaví výchozí [zásady opakování](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling), které jsou vhodné pro webovou aplikaci. zásady opakování exponenciálního omezení rychlosti výchozí Hello může přečnívat hello webové aplikace po dobu delší než minutu při opakovaných pokusech přechodná chyba. Tady určené zásady opakování Hello čeká tří sekund po každém pokusu až toothree pokusů.

```csharp
var blobClient = storageAccount.CreateCloudBlobClient();
blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesBlobContainer = blobClient.GetContainerReference("images");
```

Podobný kód získá odkaz na toohello *bitové kopie* fronty.

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesQueue = queueClient.GetQueueReference("images");
```

Většinu kódu kontroleru hello je typická pro práci s datovým modelem Entity Framework použití třídy DbContext. Výjimkou je hello HttpPost `Create` metodu, která soubor odešle a uloží ho do úložiště objektů blob. poskytuje vazač modelu Hello [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) objektu toohello metoda.

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<ActionResult> Create(
    [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
    HttpPostedFileBase imageFile)
```

Pokud hello uživatel vybral soubor tooupload, kód hello odešle soubor hello, uloží ho do objektu blob a aktualizuje hello záznam v databázi reklam pomocí adresy URL, který odkazuje objekt blob toohello.

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    blob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = blob.Uri.ToString();
}
```

Hello kód, který hello odesílání je v hello `UploadAndSaveBlobAsync` metoda. Vytvoří identifikátor GUID pro objekt blob hello, nahrávání a uloží hello soubor a vrátí objekt blob uložit toohello odkaz.

```csharp
private async Task<CloudBlockBlob> UploadAndSaveBlobAsync(HttpPostedFileBase imageFile)
{
    string blobName = Guid.NewGuid().ToString() + Path.GetExtension(imageFile.FileName);
    CloudBlockBlob imageBlob = imagesBlobContainer.GetBlockBlobReference(blobName);
    using (var fileStream = imageFile.InputStream)
    {
        await imageBlob.UploadFromStreamAsync(fileStream);
    }
    return imageBlob;
}
```

Po hello HttpPost `Create` metoda odešle objekt blob a aktualizace hello databázi, vytvoří tooinform zprávy fronty tohoto procesu back-end, že obrázek je připravený pro převod tooa miniaturu.

```csharp
string queueMessageString = ad.AdId.ToString();
var queueMessage = new CloudQueueMessage(queueMessageString);
await queue.AddMessageAsync(queueMessage);
```

Hello kód pro hello HttpPost `Edit` metoda je podobný s tím rozdílem, že pokud hello uživatel vybere nový obrázkový soubor musíte odstranit všechny objekty BLOB, které již existují.

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    await DeleteAdBlobsAsync(ad);
    imageBlob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = imageBlob.Uri.ToString();
}
```

Hello další příklad ukazuje hello kód, který odstraní objekty BLOB při odstranění ad.

```csharp
private async Task DeleteAdBlobsAsync(Ad ad)
{
    if (!string.IsNullOrWhiteSpace(ad.ImageURL))
    {
        Uri blobUri = new Uri(ad.ImageURL);
        await DeleteAdBlobAsync(blobUri);
    }
    if (!string.IsNullOrWhiteSpace(ad.ThumbnailURL))
    {
        Uri blobUri = new Uri(ad.ThumbnailURL);
        await DeleteAdBlobAsync(blobUri);
    }
}
private static async Task DeleteAdBlobAsync(Uri blobUri)
{
    string blobName = blobUri.Segments[blobUri.Segments.Length - 1];
    CloudBlockBlob blobToDelete = imagesBlobContainer.GetBlockBlobReference(blobName);
    await blobToDelete.DeleteAsync();
}
```

### <a name="contosoadsweb---viewsadindexcshtml-and-detailscshtml"></a>ContosoAdsWeb – Views\Ad\Index.cshtml a Details.cshtml
Hello *Index.cshtml* souboru zobrazí miniatury s hello dalšími daty reklam.

```razor
<img src="@Html.Raw(item.ThumbnailURL)" />
```

Hello *Details.cshtml* zobrazí obrázek hello plné velikosti.

```razor
<img src="@Html.Raw(Model.ImageURL)" />
```

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a>ContosoAdsWeb – Views\Ad\Create.cshtml a Edit.cshtml
Hello *Create.cshtml* a *Edit.cshtml* soubory zadejte formuláře kódování, že umožňuje hello řadiče tooget hello `HttpPostedFileBase` objektu.

```razor
@using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))
```

`<input>` Element informuje hello prohlížeče tooprovide dialogové okno pro výběr souboru.

```razor
<input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />
```

### <a name="contosoadsworker---workerrolecs---onstart-method"></a>ContosoAdsWorker – WorkerRole.cs – metoda OnStart 
prostředí role pracovního procesu Azure Hello volá hello `OnStart` metoda v hello `WorkerRole` třídy, když se spouští role pracovního procesu hello a zavolá hello `Run` metoda při hello `OnStart` Metoda dokončení.

Hello `OnStart` metoda získá připojovací řetězec databáze hello z hello *.cscfg* a předá ho toohello třídy Entity Framework DbContext. Poskytovatel Sqlclienta Hello se používá ve výchozím nastavení, takže hello zprostředkovatel nemá toobe zadaný.

```csharp
var dbConnString = CloudConfigurationManager.GetSetting("ContosoAdsDbConnectionString");
db = new ContosoAdsContext(dbConnString);
```

Potom hello metoda získá odkaz na účet úložiště toohello a vytvoří kontejner objektů blob hello a fronty, pokud ještě neexistují. Hello kód, pro který je podobný toowhat jste už viděli v hello webovou roli `Application_Start` metoda.

### <a name="contosoadsworker---workerrolecs---run-method"></a>ContosoAdsWorker – WorkerRole.cs – metoda Run
Hello `Run` metoda je volána, když hello `OnStart` metoda dokončí svoji inicializaci. Hello metoda spustí nekonečnou smyčku, která sleduje nové zprávy fronty a po jejich příchodu je zpracuje.

```csharp
public override void Run()
{
    CloudQueueMessage msg = null;

    while (true)
    {
        try
        {
            msg = this.imagesQueue.GetMessage();
            if (msg != null)
            {
                ProcessQueueMessage(msg);
            }
            else
            {
                System.Threading.Thread.Sleep(1000);
            }
        }
        catch (StorageException e)
        {
            if (msg != null && msg.DequeueCount > 5)
            {
                this.imagesQueue.DeleteMessage(msg);
            }
            System.Threading.Thread.Sleep(5000);
        }
    }
}
```

Po každé iteraci smyčky hello, pokud byla nalezena žádná zpráva fronty, hello program na sekundu uspí. To brání tomu, aby role pracovního procesu hello by docházelo k nadměrnému využití procesoru čas a úložiště cena za transakci. Hello poradní tým Microsoftu vypráví příběh o vývojáři, který se zapomněl tooinclude tohoto nasazení tooproduction a odjel na dovolenou. Když se vrátil zpět, byl účet za dohled vyšší než dovolenou hello.

Někdy hello obsah zprávy fronty výsledkem bude chyba ve zpracování. Tento postup se nazývá *nezpracovatelná zpráva*, a pokud jste právě zaprotokolovali chybu a restartování hello smyčky, může do nekonečna pokusíte tooprocess této zprávě.  Proto blok catch hello zahrnuje příkaz, který kontroluje toosee kolikrát aplikace hello nezkusí tooprocess hello aktuální zprávu, a pokud to bylo víc než 5krát, odstraní hello zprávu z fronty hello.

`ProcessQueueMessage` se volá při nalezení zpráv fronty.

```csharp
private void ProcessQueueMessage(CloudQueueMessage msg)
{
    var adId = int.Parse(msg.AsString);
    Ad ad = db.Ads.Find(adId);
    if (ad == null)
    {
        throw new Exception(String.Format("AdId {0} not found, can't create thumbnail", adId.ToString()));
    }

    CloudBlockBlob inputBlob = this.imagesBlobContainer.GetBlockBlobReference(ad.ImageURL);

    string thumbnailName = Path.GetFileNameWithoutExtension(inputBlob.Name) + "thumb.jpg";
    CloudBlockBlob outputBlob = this.imagesBlobContainer.GetBlockBlobReference(thumbnailName);

    using (Stream input = inputBlob.OpenRead())
    using (Stream output = outputBlob.OpenWrite())
    {
        ConvertImageToThumbnailJPG(input, output);
        outputBlob.Properties.ContentType = "image/jpeg";
    }

    ad.ThumbnailURL = outputBlob.Uri.ToString();
    db.SaveChanges();

    this.imagesQueue.DeleteMessage(msg);
}
```

Tento kód čte hello databáze tooget hello adresa URL obrázku, převede hello image tooa miniaturu, uloží miniaturu hello do objektu blob, aktualizuje hello databáze s adresou URL hello miniaturu v objektu blob a odstraní zprávu fronty hello.

> [!NOTE]
> Hello kód v hello `ConvertImageToThumbnailJPG` metoda používá třídy v oboru názvů System.Drawing hello pro jednoduchost. Hello třídy v tomto oboru názvů však byly navrženy pro používání s formuláři Windows. Jejich používání není podporované ve Windows a službě ASP.NET. Další informace o možnostech zpracování obrázků najdete v článcích o [dynamickém generování obrázků](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) a o [hloubkové změně velikosti uvnitř obrázků](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).
>
>

## <a name="troubleshooting"></a>Řešení potíží
V případě, že něco nefunguje vám při procházení kurzem hello pokyny v tomto kurzu, tady jsou některé běžné chyby a jak tooresolve je.

### <a name="serviceruntimeroleenvironmentexception"></a>ServiceRuntime.RoleEnvironmentException
Hello `RoleEnvironment` objekt při spuštění aplikace v Azure nebo při spuštění místně pomocí emulátoru služby výpočty Azure hello poskytuje Azure.  Pokud k této chybě dojde, když spouštíte místně, ujistěte se, že jste hello projektu ContosoAdsCloudService nastavili jako spouštěný projekt hello. Toto nastaví toorun hello projektu pomocí emulátoru služby výpočty Azure hello.

Jednou z věcí hello hello hello aplikace používá RoleEnvironment Azure pro je tooget hello připojení řetězcové hodnoty, které jsou uložené v hello *.cscfg* souborům, takže další možnou příčinou této výjimky je chybějící připojovací řetězec. Ujistěte se, který jste vytvořili hello nastavení StorageConnectionString pro Cloudovou i místní konfiguraci v projektu ContosoAdsWeb hello a který jste vytvořili oba připojovací řetězce pro obě konfigurace i v projektu ContosoAdsWorker hello. Pokud uděláte **najít všechny** vyhledávání storageconnectionstring v celé řešení hello měli zobrazení 9 časy v 6 soubory.

### <a name="cannot-override-tooport-xxx-new-port-below-minimum-allowed-value-8080-for-protocol-http"></a>Nejde přepsat tooport xxx. Nový port s nižší než minimální povolenou hodnotou 8080 pro protokol http
Změňte číslo portu hello používá hello webového projektu. Klikněte pravým tlačítkem na projekt ContosoAdsWeb hello a pak klikněte na **vlastnosti**. Klikněte na tlačítko hello **webové** a pak změňte číslo portu hello hello **adresa Url projektu** nastavení.

Další alternativní řešení problému hello najdete v následující části hello.

### <a name="other-errors-when-running-locally"></a>Další chyby při místním spuštění
Výchozí nové cloudové služby projektů pomocí hello Azure výpočetní emulátor express toosimulate hello prostředí Azure. Toto je Odlehčená verze hello úplného emulátoru služby výpočty a za některých podmínek hello úplný emulátor fungovat, pokud hello expresní verze nepracuje.  

toochange hello projektu toouse hello úplný emulátor, klikněte pravým tlačítkem na projekt ContosoAdsCloudService hello a pak klikněte na tlačítko **vlastnosti**. V hello **vlastnosti** okna klikněte na tlačítko hello **webové** a pak klikněte hello **použít úplný emulátor** přepínač.

V pořadí aplikace hello toorun s hello úplný emulátor máte tooopen Visual Studio s oprávněními správce.

## <a name="next-steps"></a>Další kroky
Hello aplikace Contoso Ads má záměrně jednoduchá kurz – Začínáme. Například neimplementuje [vkládání závislostí](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) nebo hello [úložiště a jednotky pracovních vzorů](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), nepodporuje [používání rozhraní k protokolování](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), nepoužívá [ Migrace Code First EF](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage datový model změny nebo [odolnost připojení EF](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage pouze dočasné chyby sítě a tak dále.

Zde jsou některé cloudové služby ukázkové aplikace, které ukazují další reálného osvědčených kódovacích postupů, ze méně složitých toomore komplexní seznamu:

* [PhluffyFotos](http://code.msdn.microsoft.com/PhluffyFotos-Sample-7ecffd31). Koncept podobný tooContoso služby Active Directory, ale implementuje víc funkcí a další realističtější postupy kódování.
* [Vícevrstvé aplikace cloudových služeb Azure s tabulkami, frontami a objekty blob](http://code.msdn.microsoft.com/windowsazure/Windows-Azure-Multi-Tier-eadceb36). Představuje tabulky služby Azure Storage a také objekty blob a fronty. Na základě na starší verzi hello Azure SDK pro .NET, bude vyžadovat některé změny toowork s aktuální verzí hello.
* [Základy cloudových služeb v Microsoft Azure Cloud](http://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Komplexní ukázka předvádějící širokou škálu osvědčených postupů, vyprodukované skupina Microsoft Patterns and Practices hello.

Obecné informace o vývoji pro hello cloud najdete v tématu [vytváření reálných cloudových aplikací s Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).

Pro úvodní video tooAzure úložiště osvědčených postupů a vzorů, najdete v části [Microsoft Azure Storage – novinky, osvědčené postupy a vzory](http://channel9.msdn.com/Events/Build/2014/3-628).

Další informace najdete v tématu hello následující prostředky:

* [Cloudové služby Azure Cloud Services část 1: Úvod](http://justazure.com/microsoft-azure-cloud-services-part-1-introduction/)
* [Jak toomanage cloudových služeb](cloud-services-how-to-manage.md)
* [Azure Storage](/documentation/services/storage/)
* [Jak toochoose cloudu poskytovatel služeb](https://azure.microsoft.com/overview/choosing-a-cloud-service-provider/)
