---
title: "aaaCreate webová rozhraní .NET v Azure App Service | Microsoft Docs"
description: "Vytvořte vícevrstvou aplikaci pomocí ASP.NET MVC a Azure. Hello front end běží ve webové aplikaci ve službě Azure App Service a hello back end běží jako webová. aplikace Hello používá rozhraní Entity Framework, SQL Database a fronty Azure storage a objekty BLOB."
services: app-service
documentationcenter: .net
author: tdykstra
manager: erikre
editor: mollybos
ms.assetid: 99cb9917-483a-45f8-a98d-07d19c68c753
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/14/2017
ms.author: glenga
ms.openlocfilehash: d92fc4b81cc0d58f8e900f257e747af56d32b911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-net-webjob-in-azure-app-service"></a>Vytvoření webové úlohy .NET ve službě Azure App Service
Tento kurz ukazuje, jak toowrite kód jednoduchou aplikaci ASP.NET MVC 5 několika vrstvami, která používá hello [WebJobs SDK](websites-dotnet-webjobs-sdk.md).

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

Hello účel hello [WebJobs SDK](websites-webjobs-resources.md) je toosimplify hello kód napsaný pro běžné úkoly, které může provádět webovou úlohu, například zpracování obrázků, zpracování fronty, RSS agregace, údržba souborů a odesílání e-mailů. Hello WebJobs SDK obsahuje integrované funkce pro práci s Azure Storage a Service Bus, plánování úloh a zpracování chyb a pro jiné běžné scénáře. Kromě toho má určený toobe rozšiřitelný a je [otevřete zdrojové úložiště pro rozšíření](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

Hello vzorová aplikace je vývěsní Tabule pro inzerci. Uživatelé mohou odesílat obrázky pro reklamy a proces back-end převede toothumbnails hello bitové kopie. Hello ad seznamu stránka zobrazuje hello miniatury a stránce s podrobnostmi o ad hello znázorňuje obrázek hello plné velikosti. Zde je snímek obrazovky:

![Seznam reklam](./media/websites-dotnet-webjobs-sdk-get-started/list.png)

Tato ukázková aplikace funguje s [Azure fronty](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) a [objektů Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage). Hello kurzu se dozvíte, jak toodeploy hello aplikace příliš[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) a [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279).

## <a id="prerequisites"></a>Požadavky
Hello kurz předpokládá, že víte, jak toowork s [ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) projekty v sadě Visual Studio.

kurz Hello byl původně zapsán pro Visual Studio 2013, ale lze použít s novější verzí sady Visual Studio. Pokud používáte Visual Studio 2015 nebo 2017, poznamenejte si před spuštěním aplikace hello místně, je nutné změnit hello `Data Source` součástí připojovací řetězec SQL serveru LocalDB hello v souborech Web.config a App.config hello z `Data Source=(localdb)\v11.0` příliš`Data Source=(LocalDb)\MSSQLLocalDB`.

> [!NOTE]
> <a name="note"></a>Tento kurz, musíte mít účtu Azure toocomplete:
>
> * Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F): získáte kredity, můžete použít tootry na placené služby Azure, a až se vyčerpáte, můžete zachovat hello účet a používat bezplatné služby Azure, jako jsou weby. Platební karty nikdy odečte, není-li explicitně změnit nastavení a požádejte toobe účtovat.
> * Můžete [aktivovat platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): vaše předplatné vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.
>
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
>
>

## <a id="learn"></a>Co se dozvíte
Hello kurzu se dozvíte, jak toodo hello následující úkoly:

* Povolte počítači pro vývoj pro Azure nainstalováním hello Azure SDK (pouze pro Visual Studio 2013 a uživatelé 2015).
* Vytvořte projekt konzolové aplikace, který automaticky nasadí jako webová úloha služby Azure, když nasazujete hello přidružené webového projektu.
* Testování back-end WebJobs SDK místně na vývojovém počítači hello.
* Publikování aplikace pomocí WebJobs back-end tooa webové aplikace ve službě App Service.
* Odeslání souborů a uložit je do hello služby objektů Blob Azure.
* Použijte hello Azure WebJobs SDK toowork s Azure Storage fronty a objekty BLOB.

## <a id="contosoads"></a>Architektura aplikace
Hello ukázková aplikace používá hello [fronty způsob práce zaměřený](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff zatížení hello náročná na prostředky procesoru práci při vytváření miniatur tooa back-end procesu.

Hello aplikace ukládá reklamy do databáze SQL pomocí Entity Framework Code First toocreate hello tabulky a přístup k datům hello. Pro každou reklamu hello databáze ukládá dvě adresy URL: jednu pro obrázek hello plné velikosti a druhou pro miniaturu hello.

![Tabulka reklam](./media/websites-dotnet-webjobs-sdk-get-started/adtable.png)

Když uživatel odešle obrázku, hello webové aplikace ukládá hello bitové kopie v [objektů blob v Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), a ukládá hello ad informace v databázi hello s adresou URL, který odkazuje objekt blob toohello. Na hello stejný čas, zapíše zpráva tooan fronty Azure. V back-end proces, který běží jako webová úloha služby Azure dotazuje hello WebJobs SDK hello fronty pro nové zprávy. Když se objeví nová zpráva, hello webové úlohy vytvoří miniaturu obrázku a aktualizace hello miniatur databáze pole adresy URL pro tuto reklamu. Zde je diagram, který znázorňuje interakci částí hello hello aplikace:

![Architektura Contoso Ads](./media/websites-dotnet-webjobs-sdk-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]  
pokyny kurzu Hello použít tooAzure SDK pro .NET 2.7.1 nebo novější.

## <a id="storage"></a>Vytvoření účtu úložiště Azure
Účet úložiště Azure poskytuje prostředky pro ukládání dat front a objektů blob v cloudu hello. Používá se také podle hello WebJobs SDK toostore protokolování dat pro řídicí panel hello.

V reálné aplikaci obvykle vytvoříte samostatné účty pro data aplikací a pro data protokolování a samostatné účty pro testovací data a pro produkční data. V tomto kurzu budete používat jenom jeden účet.

1. Otevřete hello **Průzkumníka serveru** oken v sadě Visual Studio.
2. Klikněte pravým tlačítkem na hello **Azure** uzel a pak klikněte na tlačítko **připojit tooMicrosoft předplatné Azure...** .
   
   ![Připojit tooAzure](./media/websites-dotnet-webjobs-sdk-get-started/connaz.png)

3. Přihlaste se pomocí přihlašovacích údajů Azure.
4. Klikněte pravým tlačítkem na **úložiště** v části hello Azure uzel a potom klikněte na **vytvořit účet úložiště**.
   
   ![Vytvořit účet úložiště](./media/websites-dotnet-webjobs-sdk-get-started/createstor.png)
   
5. V hello **vytvořit účet úložiště** dialogové okno, zadejte název pro účet úložiště hello.

    Název Hello musí být musí být jedinečné (žádný jiný účet úložiště Azure může mít hello stejný název). Pokud zadáte název hello se už používá, získáte možnost toochange ho.

    Hello tooaccess adresa URL účtu úložiště bude *{name}*. core.windows.net.
6. Sada hello **oblast nebo skupinu vztahů** rozevíracího seznamu toohello oblast nejbližší tooyou.

    Toto nastavení určuje, které datové centrum Azure bude hostitelem účtu úložiště. V tomto kurzu nebude Zkontrolujte své volby významné rozdíly. Pro produkční webové aplikace, ale chcete na váš webový server a vaše toobe účet úložiště v hello stejnou oblast toominimize latenci a data výstupních poplatky. Hello webové aplikace (aplikaci, kterou vytvoříte později) zavřete možném toohello prohlížeče přístup hello webové aplikace v pořadí toominimize latence musí být datového centra.
7. Sada hello **replikace** rozevírací seznam příliš**místně redundantní**.

    Když je povolené geografická replikace pro účet úložiště, hello uložený obsah je umístění toothat replikované tooa sekundárního datacentra tooenable převzetí služeb při selhání v případě významnější havárie v primárním umístění hello. Geografická replikace může způsobit dodatečné náklady. Pro testovací a vývojové účty obecně nechcete, aby toopay za geografickou replikaci. Další informace naleznete v článku o [vytvoření, správě nebo odstranění účtu úložiště](../storage/common/storage-create-storage-account.md).
8. Klikněte na možnost **Vytvořit**.

    ![Nový účet úložiště](./media/websites-dotnet-webjobs-sdk-get-started/newstorage.png)

## <a id="download"></a>Stažení aplikace hello
1. Stáhněte a rozbalte hello [dokončené řešení](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb).
2. Spusťte Visual Studio.
3. Z hello **soubor** nabídce zvolte **otevřít > projekt nebo řešení**, přejděte toowhere jste si stáhli hello řešení a pak otevřete soubor řešení hello.
4. Stisknutím kombinace kláves CTRL + SHIFT + B toobuild hello řešení.

    Visual Studio ve výchozím nastavení automaticky obnoví obsah balíčku NuGet hello, který nebyl součástí hello *.zip* souboru. Pokud hello balíčky neobnoví, nainstalujte je ručně budete toohello **spravovat balíčky NuGet pro řešení** dialogové okno a kliknutím na hello **obnovení** tlačítko v pravé horní hello.
5. V **Průzkumníku řešení**, ujistěte se, že **ContosoAdsWeb** je vybrán jako spouštěný projekt hello.

## <a id="configurestorage"></a>Konfigurace aplikace toouse hello účtu úložiště
1. Otevřete aplikaci hello *Web.config* souboru v projektu ContosoAdsWeb hello.

    Hello soubor obsahuje připojovací řetězec SQL a úložiště Azure připojovací řetězec pro práci s objekty BLOB a frontami.

    Hello připojovací řetězec SQL body tooa [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) databáze.

    připojovací řetězec úložiště Hello je příklad, který má zástupné symboly hello klíč účtu úložiště název a přístup. Nahraďte ho budete s připojovacím řetězcem, který má hello název a klíč účtu úložiště.  

    ```
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
        <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
    </connectionStrings>
    ```
    připojovací řetězec úložiště Hello je pojmenovaná AzureWebJobsStorage vzhledem k tomu, který je hello hello název, jaký používá WebJobs SDK ve výchozím nastavení. Hello stejný název se používá tady, takže budete mít řetězcovou hodnotu tooset pouze jedno připojení v hello prostředí Azure.
2. V **Průzkumníka serveru**, klikněte pravým tlačítkem na účtu úložiště v rámci hello **úložiště** uzel a pak klikněte na tlačítko **vlastnosti**.

    ![Klikněte na tlačítko Vlastnosti účtu úložiště](./media/websites-dotnet-webjobs-sdk-get-started/storppty.png)
3. V hello **vlastnosti** okně klikněte na tlačítko **klíče účtu úložiště**a potom klikněte na nabídku s výpustkou hello.

    ![Klíče účtu úložiště](./media/websites-dotnet-webjobs-sdk-get-started/stor-account-keys.png)
4. Kopírování hello **připojovací řetězec**.

    ![Dialogové okno klíče účtu úložiště](./media/websites-dotnet-webjobs-sdk-get-started/cpak.png)
5. Nahraďte hello připojovacího řetězce úložiště v hello *Web.config* soubor s hello připojovací řetězec jste právě zkopírovali. Ujistěte se, že jste vybrali, všechno uvnitř hello uvozovky, ale není včetně uvozovek hello před vkládání.
6. Otevřete hello *App.config* soubor v projektu ContosoAdsWebJob hello.

    Tento soubor má dva řetězce připojení úložiště, jeden pro data aplikací a jeden pro protokolování. Můžete použít samostatné úložiště účty pro data aplikací a protokolování, a můžete použít [účtů více úložiště pro data](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs). V tomto kurzu budete používat účet jednoho úložiště. připojovací řetězce Hello mít zástupné symboly hello klíče účtu úložiště.

    ```
    <configuration>
        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;"/>
    </connectionStrings>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    </configuration>

    ```

    Ve výchozím nastavení vyhledá připojovací řetězce s názvem AzureWebJobsStorage a AzureWebJobsDashboard hello WebJobs SDK. Jako alternativu můžete [ukládání hello připojovací řetězec, ale chcete a předat jej v explicitně toohello `JobHost` objekt](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#config).
7. Nahraďte oba připojovací řetězce úložiště hello připojovací řetězec, který jste zkopírovali dříve.
8. Uložte provedené změny.

## <a id="run"></a>Místní spuštění aplikace hello
1. front-end webové hello toostart aplikace hello, stiskněte CTRL + F5.

    Hello výchozí prohlížeč otevře domovskou stránku toohello. (hello webového projektu spouští vzhledem k tomu, které jste udělali hello spouštěný projekt.)

    ![Domovská stránka Contoso Ads](./media/websites-dotnet-webjobs-sdk-get-started/home.png)
2. toostart hello webové úlohy back-end systému hello aplikace, klikněte pravým tlačítkem na projekt ContosoAdsWebJob hello v **Průzkumníku řešení**a potom klikněte na **ladění** > **spustit novou instanci** .

    Okno konzoly aplikace spustí se protokolování zpráv, což značí, že objekt WebJobs SDK JobHost hello bylo zahájeno toorun.

    ![Okno konzoly aplikace zobrazuje tento back-end hello běží](./media/websites-dotnet-webjobs-sdk-get-started/backendrunning.png)
3. V prohlížeči, klikněte na tlačítko **vytvořit reklamu**.
4. Zadejte nějaká testovací data, vyberte tooupload image a pak klikněte na tlačítko **vytvořit**.

    ![Vytvoření stránky](./media/websites-dotnet-webjobs-sdk-get-started/create.png)

    aplikace Hello přejde toohello indexovou stránku, ale nezobrazí miniaturu nové reklamy hello, protože toto zpracování ještě neproběhlo.

    Mezitím po krátké čekání protokolování se v okně aplikace konzoly hello ukazuje, že fronty zpráv byla přijata a byla zpracována.

    ![Okno konzoly aplikace zobrazující, že po zpracování zprávy fronty](./media/websites-dotnet-webjobs-sdk-get-started/backendlogs.png)
5. Po uvidíte hello protokolování zprávy v okně aplikace hello konzoly, aktualizujte hello Index stránky toosee hello miniaturu.

    ![Indexová stránka](./media/websites-dotnet-webjobs-sdk-get-started/list.png)
6. Klikněte na tlačítko **podrobnosti** pro ad toosee hello plné velikosti bitové kopie.

    ![Stránka podrobností](./media/websites-dotnet-webjobs-sdk-get-started/details.png)

Aplikace hello jste byl spuštěn v místním počítači a používá SQL Server databáze nachází na váš počítač, ale funguje v případě front a objektů BLOB v hello cloudu. V následující části hello spustíte hello aplikaci v cloudu hello používání cloudové databáze a také objekty BLOB cloudové a front.  

## <a id="runincloud"></a>Spuštění aplikace hello v cloudu hello
Můžete to udělat následující kroky toorun hello aplikaci v cloudu hello hello:

* Nasazení aplikací tooWeb. Visual Studio automaticky vytvoří nové webové aplikace v App Service a instanci databáze SQL.
* Nakonfigurujte hello webové aplikace toouse účtu Azure SQL database a úložiště.

Po vytvoření některé reklamy při spuštění v cloudu hello budete zobrazit hello bohaté monitorovací funkce, které má toooffer hello toosee řídicím panelu WebJobs SDK.

### <a name="deploy-tooweb-apps"></a>Nasazení aplikací tooWeb

1. Zavřete prohlížeč hello a okno aplikace hello konzoly.
2. Postupujte podle kroků hello v hello [publikování tooAzure s databází SQL](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) části.
3. Po dokončení hello kroky pro nasazování pokračujte hello zbývajících úkolech v tomto článku.

### <a name="configure-hello-web-app-toouse-your-azure-sql-database-and-storage-account"></a>Konfigurace hello webové aplikace toouse účtu Azure SQL database a úložiště
Nejlepším postupem zabezpečení je příliš[neukládejte citlivé informace, jako je například připojovací řetězce v souborech, které jsou uložené v úložišť zdrojového kódu](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets). Azure poskytuje toodo způsobem, který: můžete nastavit připojovací řetězec a další nastavení hodnoty v hello prostředí Azure a rozhraní API pro konfiguraci ASP.NET automaticky vyzvedne, až tyto hodnoty při spuštění aplikace hello v Azure. Tyto hodnoty lze nastavit v Azure pomocí **Průzkumníka serveru**, hello portálu Azure, prostředí Windows PowerShell nebo hello multiplatformního rozhraní příkazového řádku. Další informace najdete v tématu [fungování řetězců aplikace a připojovacích řetězců](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).

V této části můžete použít **Průzkumníka serveru** tooset hodnoty připojovacího řetězce v Azure.

1. V **Průzkumníka serveru**, klikněte pravým tlačítkem na vaší webové aplikace v rámci **Azure > aplikační služby > {vaší skupiny prostředků}**a potom klikněte na **nastavení zobrazení**.

    Hello **webové aplikace Azure** na hello se otevře okno **konfigurace** kartě.
2. Název hello změnit název toohello hello objekt DefaultConnection připojovacího řetězce jste zvolili, když jste nakonfigurovali hello SQL database v hello [publikování tooAzure s databází SQL](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) článku.

    Při vytváření hello webové aplikace s přidružené databáze, takže už je hello správný připojovací řetězec hodnotu, Azure automaticky vytvoří tento připojovací řetězec. Měníte jenom název toowhat hello, které hledá kódu.
3. Přidejte dva nové připojovací řetězce s názvem AzureWebJobsStorage a AzureWebJobsDashboard. Nastavte typ databáze hello příliš**vlastní**a sadu hello připojovací řetězec hodnotu toohello stejnou hodnotu, kterou jste použili předtím pro hello *Web.config* a *App.config* soubory. (Nezapomeňte zahrnout hello celý připojovací řetězec, ne jenom hello přístupový klíč a neobsahují hello uvozovky.)

    Tyto připojovací řetězce jsou používány hello WebJobs SDK, jeden pro data aplikací a jeden pro protokolování. Jak už jste viděli dříve, hello, jeden pro data aplikací také používá kód front-endu webové hello.
4. Klikněte na **Uložit**.

    ![Připojovací řetězce v portálu Azure](./media/websites-dotnet-webjobs-sdk-get-started/azconnstr.png)
5. V **Průzkumníka serveru**, klikněte pravým tlačítkem na hello webové aplikace a pak klikněte na tlačítko **Zastavit**.
6. Po hello webové aplikace se zastaví, znovu klikněte pravým tlačítkem hello webové aplikace a pak klikněte na tlačítko **spustit**.

   Hello webová úloha se automaticky spustí, když publikujete, ale se zastaví, když provedete změnu konfigurace. toorestart, můžete buď restartování webové aplikace hello nebo hello webové úlohy v hello [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715). Po změně konfigurace je obecně doporučené toorestart hello webové aplikace.
7. Aktualizujte hello okno prohlížeče, který má adresa URL webové aplikace hello jeho panelu Adresa.

    Hello Domovská stránka se zobrazí.
8. Vytvoření ad, jako jste to udělali při jste [spustili aplikace hello místně](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk-get-started#a-idrunarun-the-application-locally).

   indexovou stránku Hello ukazuje bez miniaturu na první.
9. Aktualizujte stránku hello za několik sekund a hello Miniatura se zobrazí.

   Pokud se nezobrazí miniaturu hello, může být toowait minutu pro webové úlohy toorestart hello. Pokud se po chvíli stále nevidíte, nemusí mít hello miniaturu při aktualizaci hello stránky, hello webové úlohy automaticky spustit. V takovém případě přejděte toohello **App Services** okno v hello [portál Azure](https://portal.azure.com/), vyhledejte vaší webové aplikace a pak klikněte na tlačítko **spustit**.

### <a name="view-hello-webjobs-sdk-dashboard"></a>Hello zobrazení řídicího panelu WebJobs SDK
1. V hello [portál Azure](https://portal.azure.com/), vyberte hello **okno App Services**, vyhledejte vaší webové aplikace a vyberte **WebJobs**.
3. Vyberte hello **protokoly** kartě.

    ![Karta protokoly](./media/websites-dotnet-webjobs-sdk-get-started/log-tab.png)

    Otevře novou kartu prohlížeče toohello řídicím panelu WebJobs SDK. Hello řídicího panelu ukazuje, že hello webová úloha běží a zobrazuje seznam funkcí v kódu této hello spuštění WebJobs SDK.
4. Klikněte na jednu z hello funkce toosee podrobnosti o jeho spuštění.

    ![Řídicí panel WebJobs SDK](./media/websites-dotnet-webjobs-sdk-get-started/wjdashboardhome.png)

    ![Řídicí panel WebJobs SDK](./media/websites-dotnet-webjobs-sdk-get-started/wjfunctiondetails.png)

    Hello **funkce opětovného přehrání** tlačítko na této stránce způsobí, že hello WebJobs SDK framework toocall hello funkce znovu a nabízí funkce prvního toochange hello dat předávaných toohello nejdřív.

> [!NOTE]
> Po dokončení testování, zvažte odstranění hello webové aplikace, účet úložiště a vaše instance databáze SQL. Webová aplikace Hello je volné, ale hello účet úložiště SQL a instanci databáze nabíhat poplatky (i když, minimální kvůli toohello malá velikost). Navíc pokud hello webová aplikace spuštěná, každý, kdo najde vaši adresu URL můžete vytvořit a zobrazovat reklamy. 
>
>

### <a name="delete-your-web-app"></a>Odstranění webové aplikace
Hello portálu, přejděte toohello **App Services** okno, najděte a vyberte vaší webové aplikace a pak klikněte na tlačítko **odstranit**. Pokud chcete zabránit tootemporarily ostatním přístup hello webové aplikace, klikněte na tlačítko **Zastavit** místo. V takovém případě budou poplatky dál tooaccrue pro hello databáze SQL a účet úložiště.

### <a name="delete-your-storage-account"></a>Odstranění účtu úložiště
toodelete účtu úložiště najdete v části [odstranit účet úložiště](https://docs.microsoft.com/azure/storage/storage-create-storage-account#delete-a-storage-account). 

### <a name="delete-your-database"></a>Odstranění databáze
toodelete vaše SQL databáze najdete v tématu hello [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/) dokumentaci.

## <a id="create"></a>Vytvoření aplikace hello od začátku
V této části, můžete to udělat hello následující úlohy:

* Vytvoření řešení sady Visual Studio s webového projektu.
* Přidání projektu knihovny tříd pro hello data vrstva přístupu, která jsou sdílena mezi hello front-end a back-end.
* Přidejte projekt konzolové aplikace pro hello back-end, se webové úlohy nasazení povolené.
* Přidání balíčků NuGet.
* Nastavení odkazů projektu
* Zkopírujte soubory kódu a konfigurační soubory aplikace z aplikace hello stáhli, kterou jste pracovali v předchozí části kurzu hello hello.
* Projděte si části hello hello kódu, které fungují s Azure BLOB a frontami a hello WebJobs SDK.

### <a name="create-a-visual-studio-solution-with-a-web-project-and-class-library-project"></a>Vytvoření řešení Visual Studio s webový projekt a projekt knihovny tříd
1. V sadě Visual Studio, vyberte **soubor** > **nový** > **projektu**.
2. V hello **nový projekt** dialogovém okně, vyberte **Visual C#** > **webové** > **webové aplikace ASP.NET (rozhraní .NET Framework)**.
3. Název hello projektu ContosoAdsWeb, název řešení hello ContosoAdsWebJobsSDK (změnit název řešení hello, pokud jste ji umístíte v hello stejné složce jako hello stáhnout řešení) a potom klikněte na **OK**.

    ![Nový projekt](./media/websites-dotnet-webjobs-sdk-get-started/newproject.png)
4. V hello **nové webové aplikace ASP.NET** dialogovém okně, vyberte šablonu MVC hello a vyberte **změna ověřování**.

    ![Změna ověřování](./media/websites-dotnet-webjobs-sdk-get-started/chgauth.png)
5. V hello **změna ověřování** dialogovém okně, vyberte **bez ověřování**a potom klikněte na **OK**.

    ![Bez ověřování](./media/websites-dotnet-webjobs-sdk-get-started/noauth.png)
6. V hello **nové webové aplikace ASP.NET** dialogové okno, klikněte na tlačítko **OK**.

    Visual Studio vytvoří řešení hello a hello webového projektu.
7. V **Průzkumníku řešení**, klikněte pravým tlačítkem na řešení hello (ne hello projekt) a zvolte **přidat** > **nový projekt**.
8. V hello **přidat nový projekt** dialogovém okně, vyberte **Visual C#** > **Windows Classic Desktop** > **třídy knihovny (.NET Framework)** šablony.  
9. Název projektu hello *ContosoAdsCommon*a potom klikněte na **OK**.

    Tento projekt bude obsahovat hello na kontext Entity Framework a hello datového modelu, které oba hello front-end a back-end bude používat. Jako alternativu můžete definovat třídy související s EF hello v hello webový projekt a odkazovat na takový projekt z projektu úlohy WebJob hello. Ale pak projekt webové úlohy by měla mít referenční tooweb sestavení, která nepotřebuje.

### <a name="add-a-console-application-project-that-has-webjobs-deployment-enabled"></a>Přidání projektu konzolové aplikace, který má povoleno nasazení webové úlohy
1. Klikněte pravým tlačítkem na projekt webové hello (ne hello řešení nebo projektu knihovny tříd hello) a pak klikněte na **přidat** > **nový projekt webové úlohy Azure**.

    ![Nový projekt webové úlohy Azure výběr nabídky](./media/websites-dotnet-webjobs-sdk-get-started/newawjp.png)
2. V hello **přidat webové úlohy Azure** dialogové okno, zadejte ContosoAdsWebJob jako obou hello **název projektu** a hello **název webové úlohy**. Nechte **webové úlohy spustit režim** nastavit příliš**spouštět nepřetržitě**.
3. Klikněte na **OK**.

   Visual Studio vytvoří konzolovou aplikaci, která je nakonfigurovaná toodeploy jako webová vždy, když je nasazení webového projektu hello. toodo, že ji provést následující úlohy po vytvoření projektu hello hello:

   * Přidat *webové úlohy publikovat settings.json* soubor ve složce vlastnosti projektu hello webové úlohy.
   * Přidat *webjobs list.json* soubor ve složce vlastnosti projektu webové hello.
   * Hello balíček Microsoft.Web.WebJobs.Publish NuGet nainstalované v projektu úlohy WebJob hello.

   Další informace o těchto změnách najdete v tématu [jak toodeploy webové úlohy pomocí sady Visual Studio](websites-dotnet-deploy-webjobs.md).

### <a name="add-nuget-packages"></a>Přidání balíčků NuGet
Hello nový projekt šablony pro projekt webové úlohy automaticky nainstaluje balíček NuGet pro webové úlohy SDK hello [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) a jeho závislosti.

Jeden z hello je sada WebJobs SDK závislosti, které se instaluje automaticky v projektu úlohy WebJob hello hello Azure úložiště klienta knihovny (SCL). Ale musíte tooadd ho toohello webového projektu toowork s objekty BLOB a frontami.

1. Otevřete hello **spravovat balíčky NuGet** dialogové okno pro řešení hello.
2. V levém podokně hello vyberte **nainstalované balíky**.
3. Najde hello *Azure Storage* balíček a potom klikněte na **spravovat**.
4. V hello **vyberte projekty** pole, vyberte hello **ContosoAdsWeb** zaškrtněte políčko a potom klikněte na **OK**.

    Všechny tři projekty používat hello Entity Framework toowork s daty v databázi SQL.
5. V levém podokně hello vyberte **Online**.
6. Najde hello *EntityFramework* NuGet balíček a nainstalujte ho do všech tří projektů.

### <a name="set-project-references"></a>Nastavení odkazů na projekty
Web a webové úlohy projekty pracovat databáze SQL hello, takže i musí být v projektu ContosoAdsCommon toohello odkaz.

1. V hello projektu ContosoAdsWeb nastavte odkaz na projekt ContosoAdsCommon toohello. (Klikněte pravým tlačítkem na projekt ContosoAdsWeb hello a pak klikněte na **přidat** > **odkaz**. 
2. V hello **správce odkazů** dialogové okno, vyberte **projekty** > **řešení** > **ContosoAdsCommon**a potom klikněte na **OK**.)
   
    projekt webové úlohy Hello potřebuje odkazy pro práci s obrázky a pro přístup k připojovací řetězce.

4. V projektu ContosoAdsWebJob hello nastavte odkaz příliš`System.Drawing` a `System.Configuration`.

### <a name="add-code-and-configuration-files"></a>Přidání kódu a konfiguračních souborů
V tomto kurzu nezobrazuje jak příliš[vytvořit kontrolery a zobrazení pomocí generování uživatelského rozhraní MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started), jak příliš[napsat kód Entity Framework, který funguje s databází serveru SQL Server](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc), nebo [hello základy asynchronní programování v technologii ASP.NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async). Všechno, co, takže je zůstane toodo kopírovat soubory kódu a konfigurace z řešení hello stáhli do nové řešení hello. Poté, co můžete udělat, hello následující části zobrazit a vysvětlí klíčová místa hello kódu.

tooadd soubory tooa projektu nebo složky, klikněte pravým tlačítkem na projekt hello nebo složku a klikněte na tlačítko **přidat** > **existující položka**. Vyberte hello souborů, které jste chcete a klikněte na tlačítko **přidat**. Pokud se zobrazí dotaz, zda chcete, aby tooreplace existující soubory, klikněte na tlačítko **Ano**.

1. V projektu ContosoAdsCommon hello odstranit hello *Class1.cs* souboru a přidejte do jeho místní hello následující soubory z projektu hello stáhli.

   * *AD.cs*
   * *ContosoAdscontext.cs*
   * *BlobInformation.cs*
2. V projektu ContosoAdsWeb hello přidejte následující soubory z projektu hello stáhli hello.

   * *Soubor Web.config*
   * *Global.asax.cs*  
   * V hello *řadiče* složky: *AdController.cs*
   * V hello *Views\Shared* složky: *_Layout.cshtml* souboru
   * V hello *Views\Home* složky: *Index.cshtml*
   * V hello *Views\Ad* složky (nejprve vytvořte složku hello): pět *.cshtml* soubory
3. V projektu ContosoAdsWebJob hello přidejte následující soubory z projektu hello stáhli hello.

   * *App.config* (změnit filtr typu souboru hello příliš**všechny soubory**)
   * *Program.cs*
   * *Functions.cs*

Teď můžete sestavit, spuštění a nasazení aplikace hello podle pokynů v kurzu hello. Předtím, než můžete udělat, ale hello webové úlohy, který je stále spuštěná v hello první webové aplikace, které jste nasadili na zastavte. Jinak bude této webové úlohy zpracování zprávy fronty vytvoří místně nebo hello aplikace spuštěné v nové webové aplikace, protože všechny používá hello stejný účet úložiště.

## <a id="code"></a>Zkontrolujte kód aplikace hello
Hello následující části popisují kód hello související tooworking s hello WebJobs SDK a objektů BLOB služby Azure Storage a fronty.

> [!NOTE]
> Hello kód konkrétní toohello WebJobs SDK, najdete toohello [Program.cs a Functions.cs](#programcs) oddíly.
>
>

### <a name="contosoadscommon---adcs"></a>ContosoAdsCommon – Ad.cs
Hello soubor Ad.cs definuje výčet kategorií reklam a třídu entity objektů POCO pro informace o reklamách.

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

### <a name="contosoadscommon---contosoadscontextcs"></a>ContosoAdsCommon – ContosoAdsContext.cs
Hello třída ContosoAdsContext Určuje, zda text hello Ad třída se používá v kolekci DbSet, kterou Entity Framework jsou uloženy v databázi SQL.

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

Hello třída má dva konstruktory. Hello nejprve je používané hello webovým projektem a určuje hello název připojovacího řetězce, který je uložený v souboru Web.config hello nebo hello Azure běhového prostředí. Hello druhý konstruktor vám umožňuje toopass v hello samotný připojovací řetězec. To vyžaduje projekt webové úlohy hello protože sám nemá soubor Web.config. Už jste viděli dříve tento připojovací řetězec uložil, kde uvidíte později jak hello kód načte hello připojovací řetězec při vytvoření instance třídy DbContext hello.

### <a name="contosoadscommon---blobinformationcs"></a>ContosoAdsCommon – BlobInformation.cs
Hello `BlobInformation` třída je použité toostore informace o objektu blob bitové kopie do zprávy fronty.

        public class BlobInformation
        {
            public Uri BlobUri { get; set; }

            public string BlobName
            {
                get
                {
                    return BlobUri.Segments[BlobUri.Segments.Length - 1];
                }
            }
            public string BlobNameWithoutExtension
            {
                get
                {
                    return Path.GetFileNameWithoutExtension(BlobName);
                }
            }
            public int AdId { get; set; }
        }


### <a name="contosoadsweb---globalasaxcs"></a>ContosoAdsWeb – Global.asax.cs
Kód, který je volána z hello `Application_Start` metoda vytvoří *bitové kopie* kontejner objektů blob a *bitové kopie* fronty, pokud ještě neexistují. Tím se zajistí, že při každém spuštění pomocí nového účtu úložiště hello požadované, automaticky vytvoří kontejner objektů blob a fronty.

Hello kód získá přístup k účtu úložiště pro toohello pomocí připojovacího řetězce úložiště hello z hello *Web.config* soubor nebo Azure běhového prostředí.

        var storageAccount = CloudStorageAccount.Parse
            (ConfigurationManager.ConnectionStrings["AzureWebJobsStorage"].ToString());

Potom získá odkaz na toohello *bitové kopie* kontejner objektů blob, vytvoří kontejner hello, pokud ještě neexistuje, a nastaví přístupová oprávnění na nový kontejner hello. Ve výchozím nastavení povolí nové kontejnery jenom pro klienty s úložiště objektů BLOB tooaccess přihlašovací údaje účtu. Hello webové aplikace musí hello toobe veřejné objekty BLOB tak, aby se může zobrazit obrázky pomocí adres URL objekty BLOB tento bod toohello bitové kopie.

        var blobClient = storageAccount.CreateCloudBlobClient();
        var imagesBlobContainer = blobClient.GetContainerReference("images");
        if (imagesBlobContainer.CreateIfNotExists())
        {
            imagesBlobContainer.SetPermissions(
                new BlobContainerPermissions
                {
                    PublicAccess = BlobContainerPublicAccessType.Blob
                });
        }

Podobný kód získá odkaz na toohello *thumbnailrequest* fronty a vytvoří novou frontu. V tomto případě není nutná žádná oprávnění.

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        var imagesQueue = queueClient.GetQueueReference("thumbnailrequest");
        imagesQueue.CreateIfNotExists();

### <a name="contosoadsweb---layoutcshtml"></a>ContosoAdsWeb – _Layout.cshtml
Hello *_Layout.cshtml* soubor nastaví název aplikace hello v hello záhlaví a zápatí a vytvoří položku nabídky "Reklamy".

### <a name="contosoadsweb---viewshomeindexcshtml"></a>ContosoAdsWeb – Views\Home\Index.cshtml
Hello *Views\Home\Index.cshtml* souboru zobrazuje kategorie odkazy na domovskou stránku hello. Hello odkazy předají celočíselnou hodnotu hello hello `Category` výčtu na stránce služby Active Directory Index toohello proměnné řetězce dotazu.

        <li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
        <li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
        <li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
        <li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>

### <a name="contosoadsweb---adcontrollercs"></a>ContosoAdsWeb – AdController.cs
V hello *AdController.cs* soubor, hello konstruktor volání hello `InitializeStorage` metoda toocreate Klientská knihovna pro úložiště Azure objekty, které poskytují rozhraní API pro práci s objekty BLOB a frontami.

Potom hello kód získá odkaz na toohello *bitové kopie* kontejner objektů blob, jako jste viděli dříve v *Global.asax.cs*. Během toho, nastaví výchozí [zásady opakování](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) vhodné pro webovou aplikaci. zásady opakování exponenciálního omezení rychlosti výchozí Hello může přečnívat hello webové aplikace po dobu delší než minutu při opakovaných pokusech přechodná chyba. Tady určené zásady opakování Hello čeká tří sekund po každém pokusu až toothree pokusů.

        var blobClient = storageAccount.CreateCloudBlobClient();
        blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesBlobContainer = blobClient.GetContainerReference("images");

Podobný kód získá odkaz na toohello *bitové kopie* fronty.

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesQueue = queueClient.GetQueueReference("blobnamerequest");

Většinu kódu kontroleru hello je typická pro práci s datovým modelem Entity Framework použití třídy DbContext. Výjimkou je hello HttpPost `Create` metodu, která soubor odešle a uloží ho do úložiště objektů blob. poskytuje vazač modelu Hello [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) objektu toohello metoda.

        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> Create(
            [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
            HttpPostedFileBase imageFile)

Pokud hello uživatel vybral soubor tooupload, kód hello odešle soubor hello, uloží ho do objektu blob a aktualizuje hello záznam v databázi reklam pomocí adresy URL, který odkazuje objekt blob toohello.

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            blob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = blob.Uri.ToString();
        }

Hello kód, který hello odesílání je v hello `UploadAndSaveBlobAsync` metoda. Vytvoří identifikátor GUID pro objekt blob hello, nahrávání a uloží hello soubor a vrátí objekt blob uložit toohello odkaz.

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

Po hello HttpPost `Create` metoda odešle objekt blob a aktualizace hello databázi, vytvoří frontu zpráv tooinform hello back-end proces bitová kopie je připravená pro převod tooa miniaturu.

        BlobInformation blobInfo = new BlobInformation() { AdId = ad.AdId, BlobUri = new Uri(ad.ImageURL) };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        await thumbnailRequestQueue.AddMessageAsync(queueMessage);

Hello kód pro hello HttpPost `Edit` metoda je podobné, s tím rozdílem, že pokud hello uživatel vybere nový obrázkový soubor, musíte odstranit všechny objekty BLOB, které jsou už pro tento ad.

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            await DeleteAdBlobsAsync(ad);
            imageBlob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = imageBlob.Uri.ToString();
        }

Tady je hello kód, který odstraní objekty BLOB při odstranění ad:

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

### <a name="contosoadsweb---viewsadindexcshtml-and-detailscshtml"></a>ContosoAdsWeb – Views\Ad\Index.cshtml a Details.cshtml
Hello *Index.cshtml* souboru zobrazí miniatury s hello dalšími daty reklam:

        <img  src="@Html.Raw(item.ThumbnailURL)" />

Hello *Details.cshtml* zobrazí obrázek plné velikosti hello:

        <img src="@Html.Raw(Model.ImageURL)" />

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a>ContosoAdsWeb – Views\Ad\Create.cshtml a Edit.cshtml
Hello *Create.cshtml* a *Edit.cshtml* soubory zadejte formuláře kódování, že umožňuje hello řadiče tooget hello `HttpPostedFileBase` objektu.

        @using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))

`<input>` Element informuje hello prohlížeče tooprovide dialogové okno pro výběr souboru.

        <input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />

### <a id="programcs"></a>ContosoAdsWebJob - Program.cs
Když hello webové úlohy se spustí, hello `Main` volání metod hello WebJobs SDK `JobHost.RunAndBlock` metoda toobegin provádění spouštěná funkcí na aktuální vlákno hello.

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

### <a id="generatethumbnail"></a>Metoda GenerateThumbnail ContosoAdsWebJob - Functions.cs-
Hello WebJobs SDK volá tuto metodu, když je obdržena zprávu fronty. Metoda Hello vytvoří miniaturu a PUT hello miniaturu adresy URL v databázi hello.

        public static void GenerateThumbnail(
        [QueueTrigger("thumbnailrequest")] BlobInformation blobInfo,
        [Blob("images/{BlobName}", FileAccess.Read)] Stream input,
        [Blob("images/{BlobNameWithoutExtension}_thumbnail.jpg")] CloudBlockBlob outputBlob)
        {
            using (Stream output = outputBlob.OpenWrite())
            {
                ConvertImageToThumbnailJPG(input, output);
                outputBlob.Properties.ContentType = "image/jpeg";
            }

            // Entity Framework context class is not thread-safe, so it must
            // be instantiated and disposed within hello function.
            using (ContosoAdsContext db = new ContosoAdsContext())
            {
                var id = blobInfo.AdId;
                Ad ad = db.Ads.Find(id);
                if (ad == null)
                {
                    throw new Exception(String.Format("AdId {0} not found, can't create thumbnail", id.ToString()));
                }
                ad.ThumbnailURL = outputBlob.Uri.ToString();
                db.SaveChanges();
            }
        }

* Hello `QueueTrigger` atribut přesměruje hello WebJobs SDK toocall tato metoda při přijetí nové zprávy ve frontě thumbnailrequest hello.

        [QueueTrigger("thumbnailrequest")] BlobInformation blobInfo,

    Hello `BlobInformation` objekt v uvítací zprávu fronty je automaticky deserializovat do hello `blobInfo` parametr. Po dokončení hello metoda odstraní zprávu fronty hello. V případě selhání hello metoda před dokončením hello fronty zpráv se neodstraní; Po vypršení platnosti zapůjčení 10 minut, je uvítací zprávu vydaná toobe zachyceny znovu a zpracovat. Toto pořadí nebude opakuje bez omezení, pokud zprávu vždy dojde k výjimce. Po 5 neúspěšných pokusech tooprocess zprávu, zpráva hello je přesunutý tooa frontu s názvem {queuename}-poškozených. maximální počet pokusů o Hello je možné konfigurovat.
* dva Hello `Blob` atributy poskytují objekty, které jsou vázané tooblobs: jeden toohello existující objekt blob image a jeden tooa nové miniatur objekt blob, který vytvoří metoda hello.

        [Blob("images/{BlobName}", FileAccess.Read)] Stream input,
        [Blob("images/{BlobNameWithoutExtension}_thumbnail.jpg")] CloudBlockBlob outputBlob)

    Názvy objektů BLOB pocházet z vlastnosti hello `BlobInformation` objektu došlo k uvítací zprávu fronty (`BlobName` a `BlobNameWithoutExtension`). tooget hello plné funkčnosti hello Klientská knihovna pro úložiště, můžete použít hello `CloudBlockBlob` třída toowork s objekty BLOB. Pokud chcete, aby tooreuse kódu, která byla zapsána toowork s `Stream` objekty, můžete použít hello `Stream` třídy.

Další informace o tom, jak toowrite funkce, které používají atributy WebJobs SDK najdete v tématu hello následující prostředky:

* [Jak toouse Azure fronty úložiště s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Jak toouse Azure blob storage s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Jak toouse Azure table storage s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Jak toouse služba Azure Service Bus s hello WebJobs SDK](websites-dotnet-webjobs-sdk-service-bus.md)

> [!NOTE]
> * Pokud vaše webová aplikace běží na víc virtuálních počítačů, více webové úlohy budou používat současně a v některých případech může být výsledkem hello stejné dat zpracovává více než jednou. To není problém, pokud používáte integrované fronty hello, objektů blob a aktivační události služby Service Bus. Hello SDK zajistí, že funkce budou zpracovány pouze jednou pro každou zprávu nebo objektů blob.
> * Informace o řádné vypnutí tooimplement, najdete v části [řádné vypnutí](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).
> * Hello kód v hello `ConvertImageToThumbnailJPG` – metoda (není vidět) používá třídy v hello `System.Drawing` obor názvů pro jednoduchost. Hello třídy v tomto oboru názvů však byly navrženy pro používání s formuláři Windows. Jejich používání není podporované ve Windows a službě ASP.NET. Další informace o možnostech zpracování obrázků najdete v článcích o [dynamickém generování obrázků](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) a o [hloubkové změně velikosti uvnitř obrázků](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).
>
>

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se seznámili jednoduché vícevrstvé aplikace, která používá hello WebJobs SDK pro zpracování back-end. Tato část nabízí některé návrhy pro dozvědět více o vícevrstvých aplikací ASP.NET a webové úlohy.

### <a name="missing-features"></a>Chybějící funkce
aplikace Hello má jednoduché kurz – Začínáme. V reálné aplikaci byste implementovat [vkládání závislostí](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) a hello [úložiště a jednotky pracovních vzorů](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), použijte [rozhraní pro protokolování](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), použijte [ Migrace Code First EF](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage data modelu změny a použít [odolnost připojení EF](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage přechodných síťových chyb.

### <a name="scaling-webjobs"></a>Škálování webové úlohy
Webové úlohy spustit v kontextu hello webové aplikace a nejsou škálovatelné samostatně. Například pokud máte jednu instanci standardní webové aplikace, máte jenom jednu instanci spuštěn proces na pozadí a používá některé prostředky serveru hello (procesoru, paměti, atd.), které by jinak byly k dispozici tooserve webového obsahu.

Pokud provoz se liší podle času den nebo den v týdnu a back-end hello zpracování, je nutné počkat toodo, vaše webové úlohy toorun může naplánovat na dobu s nízkým provozem. Pokud stále hello zatížení příliš vysoká. pro toto řešení, můžete spustit back-end hello jako webová úloha v samostatné webové aplikace, který je vyhrazený pro tento účel. Webové aplikace back-end je pak možné škálovat nezávisle z vaší webové aplikace front-endu.

Další informace najdete v tématu [škálování WebJobs](websites-webjobs-resources.md#scale).

### <a name="avoiding-web-app-timeout-shut-downs"></a>Webové aplikace časový limit vypnutí seznamy vyloučení
toomake, že vaše webové úlohy jsou vždy spuštěn, a spuštěná na všech instancích vaší webové aplikace, budete mít tooenable hello [AlwaysOn](http://weblogs.asp.net/scottgu/archive/2014/01/16/windows-azure-staging-publishing-support-for-web-sites-monitoring-improvements-hyper-v-recovery-manager-ga-and-pci-compliance.aspx) funkce.

### <a name="using-hello-webjobs-sdk-outside-of-webjobs"></a>Pomocí hello WebJobs SDK mimo webové úlohy
Program, který používá hello WebJobs SDK nemá toorun v Azure v webovou úlohu. Můžete spustit místně a také může být spuštěn v jiných prostředích, jako je například roli pracovního procesu cloudové služby nebo služby systému Windows. Hello řídicím panelu WebJobs SDK však můžete pouze přistupovat prostřednictvím webové aplikace Azure. řídicí panel hello toouse máte tooconnect hello webové aplikace toohello účtu úložiště používáte nastavením hello AzureWebJobsDashboard připojovací řetězec na hello **konfigurace** kartě portálu classic hello. Potom můžete získat toohello řídicího panelu pomocí hello následující adresu URL:

https://{webappname}.SCM.azurewebsites.NET/azurejobs/#/functions

Další informace najdete v tématu [získávání řídicí panel pro místní vývoj s hello WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), ale Všimněte si, že se zobrazí původní název připojovacího řetězce.

### <a name="more-webjobs-documentation"></a>Další dokumentaci webové úlohy
Další informace najdete v tématu [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226).
