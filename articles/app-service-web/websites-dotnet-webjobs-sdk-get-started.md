---
title: "Vytvořit webovou úlohu .NET v Azure App Service | Microsoft Docs"
description: "Vytvořte vícevrstvou aplikaci pomocí ASP.NET MVC a Azure. Front-endu spustí ve webové aplikaci ve službě Azure App Service a back end běží jako webová. Aplikace používá rozhraní Entity Framework, SQL Database a fronty Azure storage a objekty BLOB."
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
ms.openlocfilehash: a20b13058caecff75af14957468f20e63a3325c9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-net-webjob-in-azure-app-service"></a>Vytvoření webové úlohy .NET ve službě Azure App Service
Tento kurz ukazuje, jak napsat kód pro jednoduchou aplikaci ASP.NET MVC 5 několika vrstvami, která používá [WebJobs SDK](websites-dotnet-webjobs-sdk.md).

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

Účelem [WebJobs SDK](websites-webjobs-resources.md) je můžete zjednodušit kód napsaný pro běžné úkoly, které může provádět webovou úlohu, například zpracování obrázků, zpracování fronty, RSS agregace, údržba souborů a odesílání e-mailů. Sada SDK webové úlohy obsahuje integrované funkce pro práci s Azure Storage a Service Bus, plánování úloh a zpracování chyb a pro jiné běžné scénáře. Kromě toho je určený pro rozšiřitelnost a dojde [otevřete zdrojové úložiště pro rozšíření](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

Vzorová aplikace je vývěsní Tabule pro inzerci. Uživatelé mohou odesílat obrázky pro reklamy a proces back-end převede obrázky miniatur. Stránka seznamu ad zobrazuje miniatur a stránce s podrobnostmi o ad zobrazí obrázek v plné velikosti. Zde je snímek obrazovky:

![Seznam reklam](./media/websites-dotnet-webjobs-sdk-get-started/list.png)

Tato ukázková aplikace funguje s [Azure fronty](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) a [objektů Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage). Tento kurz ukazuje, jak nasadit aplikaci do [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) a [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279).

## <a id="prerequisites"></a>Požadavky
Kurz předpokládá, že víte, jak pracovat s [ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) projekty v sadě Visual Studio.

Tento kurz byl původně zapsán pro Visual Studio 2013, ale lze použít s novější verzí sady Visual Studio. Pokud používáte Visual Studio 2015 nebo 2017, poznamenejte si, že předtím, než spustíte aplikaci místně, je nutné změnit `Data Source` součástí připojovací řetězec SQL serveru LocalDB v souborech Web.config a App.config z `Data Source=(localdb)\v11.0` k `Data Source=(LocalDb)\MSSQLLocalDB`.

> [!NOTE]
> <a name="note"></a>Musíte mít účet Azure k dokončení tohoto kurzu:
>
> * Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F): získáte kredity, můžete použít k vyzkoušení placených služeb Azure, a až se vyčerpáte, můžete účet ponechat a používat bezplatné služby Azure, jako jsou weby. Nikdy vám nebudeme účtovat žádné poplatky, pokud si sami nezměníte nastavení a nezačnete používat placené služby.
> * Můžete [aktivovat platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): vaše předplatné vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.
>
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
>
>

## <a id="learn"></a>Co se dozvíte
Tento kurz ukazuje, jak provést následující úkoly:

* Povolte počítači pro vývoj pro Azure tak, že instalace sady Azure SDK (pouze pro Visual Studio 2013 a uživatelé 2015).
* Vytvořte projekt konzolové aplikace, který automaticky nasadí jako webová úloha Azure při nasazení přidružené webového projektu.
* Testování back-end WebJobs SDK na vývojovém počítači místně.
* Publikování aplikace pomocí backendu webové úlohy do webové aplikace ve službě App Service.
* Odeslání souborů a jejich uložení ve službě Azure Blob.
* Práce s Azure Storage fronty a objekty BLOB pomocí Azure WebJobs SDK.

## <a id="contosoads"></a>Architektura aplikace
Ukázková aplikace používá [fronty způsob práce zaměřený](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) k vyvážila práci náročná na prostředky procesoru při vytváření miniatur procesu back-end.

Aplikace ukládá reklamy do databáze SQL a k vytváření tabulky a přístupu k datům používá Entity Framework Code First. U každé reklamy databáze ukládá dvě adresy URL: jednu pro obrázek v plné velikosti a druhou pro miniaturu.

![Tabulka reklam](./media/websites-dotnet-webjobs-sdk-get-started/adtable.png)

Když uživatel odešle obrázek, webové aplikace obrázek uloží [objektů blob v Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), a ukládá informace o reklamě do databáze s adresa URL, která odkazuje na objekt blob. Ve stejný okamžik zapíše zprávu do fronty Azure. V back-end proces, který běží jako webová úloha služby Azure dotazuje WebJobs SDK fronty pro nové zprávy. Když se objeví nová zpráva, vytvářené webové úlohy vytvoří miniaturu obrázku a aktualizuje databázi pole miniatur adresa URL pro tuto reklamu. Zde je diagram, který znázorňuje interakci částí aplikace:

![Architektura Contoso Ads](./media/websites-dotnet-webjobs-sdk-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]  
Pokyny kurzu platí pro Azure SDK pro .NET 2.7.1 nebo novější.

## <a id="storage"></a>Vytvoření účtu úložiště Azure
Účet úložiště Azure poskytuje prostředky pro ukládání dat front a objektů blob v cloudu. Také se používá WebJobs SDK k ukládání dat protokolování pro řídicí panel.

V reálné aplikaci obvykle vytvoříte samostatné účty pro data aplikací a pro data protokolování a samostatné účty pro testovací data a pro produkční data. V tomto kurzu budete používat jenom jeden účet.

1. Otevřete **Průzkumníka serveru** oken v sadě Visual Studio.
2. Klikněte pravým tlačítkem myši **Azure** uzel a pak klikněte na tlačítko **připojit k předplatnému Microsoft Azure...** .
   
   ![Připojení k Azure](./media/websites-dotnet-webjobs-sdk-get-started/connaz.png)

3. Přihlaste se pomocí přihlašovacích údajů Azure.
4. Klikněte pravým tlačítkem na **úložiště** pod Azure uzel a pak klikněte na tlačítko **vytvořit účet úložiště**.
   
   ![Vytvořit účet úložiště](./media/websites-dotnet-webjobs-sdk-get-started/createstor.png)
   
5. V **vytvořit účet úložiště** dialogové okno, zadejte název pro účet úložiště.

    Název musí být musí být jedinečné (žádný jiný účet úložiště Azure můžete mít stejný název). Pokud název, který zadáte, se už používá, získáte ho změnit.

    Adresa URL pro přístup k účtu úložiště bude *{name}*. core.windows.net.
6. Nastavte **oblast nebo skupinu vztahů** rozevíracího seznamu pro danou oblast nejblíže k vám.

    Toto nastavení určuje, které datové centrum Azure bude hostitelem účtu úložiště. V tomto kurzu nebude Zkontrolujte své volby významné rozdíly. Ale pro produkční webové aplikace, budete chtít váš webový server a účet úložiště ve stejné oblasti k minimalizaci latence a data s nimi spojeným nákladům. Webové aplikace (aplikaci, kterou vytvoříte později) datacenter by měl být co nejblíže do prohlížečů, aby se minimalizoval latence přístupu k webové aplikaci.
7. V rozevíracím seznamu **Replikace** vyberte **Místně redundantní**.

    Když má účet úložiště povolenou geografickou replikaci, bude se uložený obsah replikovat do sekundárního datacentra, které zajistí převzetí služeb při selhání v případě významnější havárie v primárním umístění. Geografická replikace může způsobit dodatečné náklady. V případě testovacích a vývojových účtů je zbytečné za geografickou replikaci platit. Další informace naleznete v článku o [vytvoření, správě nebo odstranění účtu úložiště](../storage/common/storage-create-storage-account.md).
8. Klikněte na možnost **Vytvořit**.

    ![Nový účet úložiště](./media/websites-dotnet-webjobs-sdk-get-started/newstorage.png)

## <a id="download"></a>Stažení aplikace
1. Stáhněte a rozbalte [dokončené řešení](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb).
2. Spusťte Visual Studio.
3. Z **soubor** nabídce zvolte **otevřít > projekt nebo řešení**, přejděte na kterém jste řešení stáhli a pak otevřete soubor řešení.
4. Stisknutím kláves CTRL+SHIFT+B řešení sestavíte.

    Visual Studio ve výchozím nastavení automaticky obnoví obsah balíčku NuGet, který nebyl součástí souboru *.zip*. Pokud se balíčky neobnoví, nainstalujte je ručně tak, že přejdete do **spravovat balíčky NuGet pro řešení** dialogové okno a kliknutím na **obnovení** tlačítko v pravé horní.
5. V **Průzkumníku řešení**, ujistěte se, že **ContosoAdsWeb** je vybraná jako spouštěný projekt.

## <a id="configurestorage"></a>Konfigurace aplikace pro použití účtu úložiště
1. Otevřete aplikaci *Web.config* souboru v projektu ContosoAdsWeb.

    Tento soubor obsahuje připojovací řetězec SQL a úložiště Azure připojovací řetězec pro práci s objekty BLOB a frontami.

    Připojovací řetězec SQL odkazuje na [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) databáze.

    Připojovací řetězec úložiště je příklad, který má zástupné symboly pro klíč název a přístup k účtu úložiště. Nahraďte ho budete s připojovacím řetězcem, který má název a klíč účtu úložiště.  

    ```
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
        <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
    </connectionStrings>
    ```
    Připojovací řetězec úložiště je pojmenovaná AzureWebJobsStorage vzhledem k tomu, že se jedná o název, který využívá sadu WebJobs SDK ve výchozím nastavení. Se stejným názvem se tady je použita, takže budete muset nastavit pouze jednu hodnotu řetězce připojení v prostředí Azure.
2. V **Průzkumníka serveru**, klikněte pravým tlačítkem na účtu úložiště v rámci **úložiště** uzel a pak klikněte na tlačítko **vlastnosti**.

    ![Klikněte na tlačítko Vlastnosti účtu úložiště](./media/websites-dotnet-webjobs-sdk-get-started/storppty.png)
3. V **vlastnosti** okně klikněte na tlačítko **klíče účtu úložiště**a pak klikněte na tlačítko se třemi tečkami.

    ![Klíče účtu úložiště](./media/websites-dotnet-webjobs-sdk-get-started/stor-account-keys.png)
4. Kopírování **připojovací řetězec**.

    ![Dialogové okno klíče účtu úložiště](./media/websites-dotnet-webjobs-sdk-get-started/cpak.png)
5. Nahraďte připojovací řetězec úložiště *Web.config* souboru připojovacím řetězcem, který jste právě zkopírovali. Ujistěte se, že všechno, co vyberete v uvozovkách, ale ne včetně uvozovek před vkládání.
6. Otevřete *App.config* v ContosoAdsWebJob projektu.

    Tento soubor má dva řetězce připojení úložiště, jeden pro data aplikací a jeden pro protokolování. Můžete použít samostatné úložiště účty pro data aplikací a protokolování, a můžete použít [účtů více úložiště pro data](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs). V tomto kurzu budete používat účet jednoho úložiště. Připojovací řetězce mají zástupné symboly pro klíče účtu úložiště.

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

    Ve výchozím nastavení vyhledá připojovací řetězce s názvem AzureWebJobsStorage a AzureWebJobsDashboard WebJobs SDK. Jako alternativu můžete [úložiště připojovací řetězec, ale chcete a předejte ji v explicitně na `JobHost` objekt](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#config).
7. Nahraďte oba připojovací řetězce úložiště připojovací řetězec, který jste zkopírovali dříve.
8. Uložte provedené změny.

## <a id="run"></a>Místní spuštění aplikace
1. Front-endu webové aplikace, stiskněte CTRL + F5.

    Výchozí prohlížeč otevře domovskou stránku. (Webový projekt běží vzhledem k tomu, že jste ji provedli projekt po spuštění).

    ![Domovská stránka Contoso Ads](./media/websites-dotnet-webjobs-sdk-get-started/home.png)
2. Pokud chcete spustit úlohu back-end aplikace, klikněte pravým tlačítkem na projekt ContosoAdsWebJob v **Průzkumníku řešení**a potom klikněte na **ladění** > **spustit novou instanci**.

    Okno aplikace konzoly se zobrazí protokolování zpráv oznamující, že objekt WebJobs SDK JobHost bylo zahájeno ke spuštění.

    ![Okno konzoly aplikace zobrazuje, zda je spuštěna back-end](./media/websites-dotnet-webjobs-sdk-get-started/backendrunning.png)
3. V prohlížeči, klikněte na tlačítko **vytvořit reklamu**.
4. Zadejte nějaká testovací data, vyberte bitovou kopii a potom klikněte na **vytvořit**.

    ![Vytvoření stránky](./media/websites-dotnet-webjobs-sdk-get-started/create.png)

    Aplikace přejde na indexovou stránku, ale nezobrazí miniaturu nové reklamy, protože toto zpracování ještě neproběhlo.

    Mezitím po krátké čekání protokolování zpráv v okně konzoly aplikace ukazuje, že fronty zpráv byla přijata a byla zpracována.

    ![Okno konzoly aplikace zobrazující, že po zpracování zprávy fronty](./media/websites-dotnet-webjobs-sdk-get-started/backendlogs.png)
5. Po najdete v části protokolování zprávy v okně konzoly aplikace, aktualizujte indexovou stránku Miniatura se teď zobrazí.

    ![Indexová stránka](./media/websites-dotnet-webjobs-sdk-get-started/list.png)
6. Pokud chcete obrázek reklamy zobrazit v plné velikosti, klikněte na **Podrobnosti**.

    ![Stránka podrobností](./media/websites-dotnet-webjobs-sdk-get-started/details.png)

Aplikaci jste byl spuštěn v místním počítači a používá SQL Server databáze nachází na váš počítač, ale funguje v případě front a objektů BLOB v cloudu. V následující části spustíte aplikaci v cloudu, používání cloudové databáze a také objekty BLOB cloudové a front.  

## <a id="runincloud"></a>Spusťte aplikaci v cloudu
Pokud chcete aplikaci spustit v cloudu, proveďte následující kroky:

* Nasazení do webové aplikace. Visual Studio automaticky vytvoří nové webové aplikace v App Service a instanci databáze SQL.
* Konfigurovat webovou aplikaci k používání účtu Azure SQL database a úložiště.

Po vytvoření některé reklamy při spuštění v cloudu, budete zobrazení řídicího panelu WebJobs SDK zobrazíte bohaté monitorovací funkce, které se má na nabídku.

### <a name="deploy-to-web-apps"></a>Nasazení do webové aplikace

1. Zavřete prohlížeč a v okně konzoly aplikace.
2. Postupujte podle kroků v [publikovat do Azure SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) části.
3. Po dokončení kroků pro nasazení, pokračujte ve zbývajících úkolech v tomto článku.

### <a name="configure-the-web-app-to-use-your-azure-sql-database-and-storage-account"></a>Konfigurovat webovou aplikaci k používání účtu Azure SQL database a úložiště
Je osvědčeným postupem zabezpečení [neukládejte citlivé informace, jako je například připojovací řetězce v souborech, které jsou uložené v úložišť zdrojového kódu](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets). Azure poskytuje způsob, jak to udělat: můžete nastavit připojovací řetězec a další nastavení hodnoty v prostředí Azure a rozhraní API pro konfiguraci ASP.NET automaticky vyzvedne, až tyto hodnoty při spuštění aplikace v Azure. Tyto hodnoty lze nastavit v Azure pomocí **Průzkumníka serveru**, portálu Azure, prostředí Windows PowerShell nebo rozhraní příkazového řádku a platformy. Další informace najdete v tématu [fungování řetězců aplikace a připojovacích řetězců](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).

V této části můžete použít **Průzkumníka serveru** nastavit připojovací řetězec hodnoty v Azure.

1. V **Průzkumníka serveru**, klikněte pravým tlačítkem na vaší webové aplikace v rámci **Azure > aplikační služby > {vaší skupiny prostředků}**a potom klikněte na **nastavení zobrazení**.

    **Webové aplikace Azure** na se otevře okno **konfigurace** kartě.
2. Změňte název připojovacího řetězce možnost DefaultConnection na název, který jste zvolili, když jste nakonfigurovali v databázi SQL [publikovat do Azure SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) článku.

    Při vytváření webové aplikace s přidružené databáze, takže už je hodnota správný připojovací řetězec, Azure automaticky vytvoří tento připojovací řetězec. Na co kódu hledá měníte jenom název.
3. Přidejte dva nové připojovací řetězce s názvem AzureWebJobsStorage a AzureWebJobsDashboard. Nastavte typ databáze na **vlastní**a nastavte hodnotu připojovacího řetězce na stejnou hodnotu, jakou jste použili pro dříve *Web.config* a *App.config* soubory. (Nezapomeňte zahrnout celý připojovací řetězec, ne jenom přístupový klíč a nechcete použít uvozovky.)

    Tyto připojovací řetězce jsou používány WebJobs SDK, jeden pro data aplikací a jeden pro protokolování. Jak už jste viděli dříve, jeden pro data aplikací také používá kód front-endu webové.
4. Klikněte na **Uložit**.

    ![Připojovací řetězce v portálu Azure](./media/websites-dotnet-webjobs-sdk-get-started/azconnstr.png)
5. V **Průzkumníka serveru**, klikněte pravým tlačítkem na webovou aplikaci a pak klikněte na tlačítko **Zastavit**.
6. Po zastavení webové aplikace, znovu klikněte pravým tlačítkem webovou aplikaci a pak klikněte na tlačítko **spustit**.

   Vytvářené webové úlohy se automaticky spustí, když publikujete, ale se zastaví, když provedete změnu konfigurace. Chcete-li restartovat, můžete buď restartování webové aplikace nebo webové úlohy v [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715). Obecně se doporučuje k restartu webové aplikace po změně konfigurace.
7. Aktualizujte okno prohlížeče, který má adresa URL webové aplikace v jeho panelu Adresa.

    Zobrazí se na domovskou stránku.
8. Vytvořte ad, jako když jste [byla aplikace spuštěná místně](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk-get-started#a-idrunarun-the-application-locally).

   Index stránky zobrazí bez miniaturu na první.
9. Jakmile se objeví pár sekund a miniaturu, aktualizujte stránku.

   Pokud se nezobrazí miniaturu, můžete chtít Počkejte minutu pro webové úlohy znovu. Pokud po chvíli stále nevidíte miniaturu když obnovíte stránku, nemusí mít vytvářené webové úlohy automaticky spustit. V takovém případě přejděte na **App Services** okno v [portál Azure](https://portal.azure.com/), vyhledejte vaší webové aplikace a pak klikněte na tlačítko **spustit**.

### <a name="view-the-webjobs-sdk-dashboard"></a>Zobrazení řídicího panelu WebJobs SDK
1. V [portál Azure](https://portal.azure.com/), vyberte **okno App Services**, vyhledejte vaší webové aplikace a vyberte **WebJobs**.
3. Vyberte **protokoly** kartě.

    ![Karta protokoly](./media/websites-dotnet-webjobs-sdk-get-started/log-tab.png)

    Na řídicím panelu WebJobs SDK otevře novou kartu prohlížeče. Řídicího panelu ukazuje, že vytvářené webové úlohy běží a zobrazí seznam funkcí ve vašem kódu, který aktivoval WebJobs SDK.
4. Klikněte na některou z funkcí, které mají-li zobrazit podrobnosti o jeho spuštění.

    ![Řídicí panel WebJobs SDK](./media/websites-dotnet-webjobs-sdk-get-started/wjdashboardhome.png)

    ![Řídicí panel WebJobs SDK](./media/websites-dotnet-webjobs-sdk-get-started/wjfunctiondetails.png)

    **Funkce opětovného přehrání** tlačítko na této stránce způsobí, že rozhraní WebJobs SDK na zavolejte funkci znovu a nabízí možnost změnit data nejprve předaný funkci.

> [!NOTE]
> Po dokončení testování, zvažte odstranění webové aplikace, účet úložiště a vaše instance databáze SQL. Tato webová aplikace je volné, ale nabíhat poplatky za úložiště účet a databáze instance SQL (i když, minimální kvůli malá velikost). Navíc pokud webová aplikace spuštěná, každý, kdo najde vaši adresu URL můžete vytvořit a zobrazovat reklamy. 
>
>

### <a name="delete-your-web-app"></a>Odstranění webové aplikace
Na portálu, přejděte na **App Services** okno, najděte a vyberte vaší webové aplikace a pak klikněte na tlačítko **odstranit**. Pokud chcete dočasně jiným uživatelům zabránit v přístupu k webové aplikaci, klikněte na tlačítko **Zastavit** místo. V takovém případě budou poplatky dál nabíhat účtu SQL Database a úložiště.

### <a name="delete-your-storage-account"></a>Odstranění účtu úložiště
Pokud chcete odstranit účet úložiště, najdete v části [odstranit účet úložiště](https://docs.microsoft.com/azure/storage/storage-create-storage-account#delete-a-storage-account). 

### <a name="delete-your-database"></a>Odstranění databáze
Pokud chcete odstranit vaší databázi SQL, najdete v článku [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/) dokumentaci.

## <a id="create"></a>Vytvoření aplikace od začátku
V této části, můžete to udělat následující úkoly:

* Vytvoření řešení sady Visual Studio s webového projektu.
* Přidání projektu knihovny tříd pro data vrstva přístupu, která jsou sdílena mezi front-endu a back-end.
* Přidejte projekt konzolové aplikace pro back-end, se webové úlohy nasazení povolené.
* Přidání balíčků NuGet.
* Nastavení odkazů projektu
* Zkopírujte soubory kódu a konfigurace aplikací ze stažených aplikací, kterou jste pracovali v předchozí části kurzu.
* Projděte si části kódu, které fungují s Azure BLOB a fronty a WebJobs SDK.

### <a name="create-a-visual-studio-solution-with-a-web-project-and-class-library-project"></a>Vytvoření řešení Visual Studio s webový projekt a projekt knihovny tříd
1. V sadě Visual Studio, vyberte **soubor** > **nový** > **projektu**.
2. V **nový projekt** dialogovém okně, vyberte **Visual C#** > **webové** > **webové aplikace ASP.NET (rozhraní .NET Framework)**.
3. Název projektu ContosoAdsWeb, název řešení ContosoAdsWebJobsSDK (změnit název řešení, pokud jste ji umístíte ve stejné složce jako staženého řešení) a potom klikněte na **OK**.

    ![Nový projekt](./media/websites-dotnet-webjobs-sdk-get-started/newproject.png)
4. V **nové webové aplikace ASP.NET** dialogovém okně, vyberte šablonu MVC a vyberte **změna ověřování**.

    ![Změna ověřování](./media/websites-dotnet-webjobs-sdk-get-started/chgauth.png)
5. V **změna ověřování** dialogovém okně, vyberte **bez ověřování**a potom klikněte na **OK**.

    ![Bez ověřování](./media/websites-dotnet-webjobs-sdk-get-started/noauth.png)
6. V **nové webové aplikace ASP.NET** dialogové okno, klikněte na tlačítko **OK**.

    Visual Studio vytvoří řešení a webového projektu.
7. V **Průzkumníku řešení**, klikněte pravým tlačítkem na řešení (nikoli projekt) a zvolte **přidat** > **nový projekt**.
8. V **přidat nový projekt** dialogovém okně, vyberte **Visual C#** > **Windows Classic Desktop** > **knihovny tříd (rozhraní .NET Framework)**  šablony.  
9. Pojmenujte projekt *ContosoAdsCommon* a potom klikněte na tlačítko **OK**.

    Tento projekt bude obsahovat kontext Entity Framework a datový model, který použije front-end i back-end. Jako alternativu můžete třídy související s EF definovat v projektu webové a odkazovat na takový projekt z projektu webové úlohy. Ale pak projekt webové úlohy by mít odkaz na webová sestavení, která nepotřebuje.

### <a name="add-a-console-application-project-that-has-webjobs-deployment-enabled"></a>Přidání projektu konzolové aplikace, který má povoleno nasazení webové úlohy
1. Klikněte pravým tlačítkem na projekt webové (ne řešení nebo projektu knihovny tříd) a pak klikněte na **přidat** > **nový projekt webové úlohy Azure**.

    ![Nový projekt webové úlohy Azure výběr nabídky](./media/websites-dotnet-webjobs-sdk-get-started/newawjp.png)
2. V **přidat webové úlohy Azure** dialogové okno, zadejte ContosoAdsWebJob jako i **název projektu** a **název webové úlohy**. Nechte **webové úlohy spustit režim** nastavena na **spouštět nepřetržitě**.
3. Klikněte na **OK**.

   Visual Studio vytvoří konzolovou aplikaci, která je nakonfigurovat pro nasazení jako webová vždy, když je nasazení webového projektu. Kvůli tomu provádět následující úlohy po vytvoření projektu:

   * Přidat *webové úlohy publikovat settings.json* soubor ve složce vlastnosti projektu webové úlohy.
   * Přidat *webjobs list.json* soubor ve složce webového projektu vlastnosti.
   * Balíček Microsoft.Web.WebJobs.Publish NuGet nainstalované v projektu webové úlohy.

   Další informace o těchto změnách najdete v tématu [postup nasazení webové úlohy pomocí sady Visual Studio](websites-dotnet-deploy-webjobs.md).

### <a name="add-nuget-packages"></a>Přidání balíčků NuGet
Nový projekt šablony pro projekt webové úlohy automaticky nainstaluje balíček NuGet pro webové úlohy SDK [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) a jeho závislosti.

Jednu ze závislostí WebJobs SDK, které se instaluje automaticky v projektu webové úlohy je knihovna pro klienta úložiště Azure (SCL). Potřebujete přidat k webovému projektu pro práci s objekty BLOB a fronty.

1. Otevřete **spravovat balíčky NuGet** dialogové okno pro řešení.
2. V levém podokně vyberte **nainstalované balíky**.
3. Najít *Azure Storage* balíček a potom klikněte na **spravovat**.
4. V **vyberte projekty** , vyberte **ContosoAdsWeb** zaškrtněte políčko a potom klikněte na **OK**.

    Všechny tři projekty pomocí rozhraní Entity Framework pro práci s daty v databázi SQL.
5. V levém podokně vyberte **Online**.
6. Najděte balíček NuGet *EntityFramework* a nainstalujte ho do všech tří projektů.

### <a name="set-project-references"></a>Nastavení odkazů na projekty
Web a webové úlohy projekty pracovat databázi SQL, takže i musí být odkaz na projekt ContosoAdsCommon.

1. V projektu ContosoAdsWeb nastavte odkaz na projekt ContosoAdsCommon. (Klikněte pravým tlačítkem na projekt ContosoAdsWeb a potom klikněte na **přidat** > **odkaz**. 
2. V **správce odkazů** dialogové okno, vyberte **projekty** > **řešení** > **ContosoAdsCommon**, a pak klikněte na **OK**.)
   
    Projekt webové úlohy musí odkazy pro práci s obrázky a pro přístup k připojovací řetězce.

4. V projektu ContosoAdsWebJob nastavte odkaz na `System.Drawing` a `System.Configuration`.

### <a name="add-code-and-configuration-files"></a>Přidání kódu a konfiguračních souborů
V tomto kurzu nezobrazuje postup [vytvořit kontrolery a zobrazení pomocí generování uživatelského rozhraní MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started), jak k [napsat kód Entity Framework, který funguje s databází serveru SQL Server](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc), nebo [základy asynchronní programování v technologii ASP.NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async). Proto, všechno, co udělat zbývá kopie kódu a konfigurační soubory ze staženého řešení do nového řešení. Poté, co můžete udělat, v následujících částech zobrazit a vysvětlí klíčová místa kódu.

Pokud chcete přidat soubory do projektu nebo složky, klikněte pravým tlačítkem na projekt nebo složku a potom klikněte na **Přidat** > **Existující položka**. Vyberte soubory a klikněte na tlačítko **přidat**. Pokud se zobrazí dotaz, jestli chcete nahradit existující soubory, klikněte na **Ano**.

1. V projektu ContosoAdsCommon odstraňte *Class1.cs* souborů a na jeho místo přidejte následující soubory ze staženého projektu.

   * *AD.cs*
   * *ContosoAdscontext.cs*
   * *BlobInformation.cs*
2. Do projektu ContosoAdsWeb přidejte následující soubory ze staženého projektu.

   * *Soubor Web.config*
   * *Global.asax.cs*  
   * V *řadiče* složky: *AdController.cs*
   * V *Views\Shared* složky: *_Layout.cshtml* souboru
   * V *Views\Home* složky: *Index.cshtml*
   * V *Views\Ad* složky (nejdřív složku vytvořte): pět *.cshtml* soubory
3. V projektu ContosoAdsWebJob přidejte následující soubory ze staženého projektu.

   * *App.config* (změnit filtr typu souboru k **všechny soubory**)
   * *Program.cs*
   * *Functions.cs*

Teď můžete sestavit, spuštění a nasazení aplikace podle pokynů v tomto kurzu. Předtím, než můžete udělat, ale Zastavte úlohu, která je stále spuštěná v první webové aplikace, který jste nasadili do. Tuto úlohu, jinak bude zpracovávat fronty zpráv vytvořených místně nebo v aplikaci spuštěné v nové webové aplikace, protože všechny používají stejný účet úložiště.

## <a id="code"></a>Zkontrolujte kódu aplikace
Následující části popisují kód týkající se práce s WebJobs SDK a úložiště Azure BLOB a fronty.

> [!NOTE]
> Kód, které jsou specifické pro WebJobs SDK, najdete [Program.cs a Functions.cs](#programcs) oddíly.
>
>

### <a name="contosoadscommon---adcs"></a>ContosoAdsCommon – Ad.cs
Soubor Ad.cs definuje výčet kategorií reklam a třídu entity objektů POCO pro informace o reklamách.

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
Třída ContosoAdsContext Určuje, že Ad třída se používá v kolekci DbSet, kterou Entity Framework jsou uloženy v databázi SQL.

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

Třída má dva konstruktory. První je používané webovým projektem a určuje název připojovacího řetězce, který je uložený v souboru Web.config nebo Azure běhové prostředí. Druhý konstruktor vám umožňuje předat samotný připojovací řetězec. Projekt webové úlohy, je potřeba, protože sám nemá soubor Web.config. Už dříve jste viděli, kam se tento připojovací řetězec uložil, a později uvidíte, jak kód získává připojovací řetězec při vytvoření instance třídy DbContext.

### <a name="contosoadscommon---blobinformationcs"></a>ContosoAdsCommon – BlobInformation.cs
`BlobInformation` Třída se používá k ukládání informací o objektu blob bitové kopie do zprávy fronty.

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
Kód, který se volá z metody `Application_Start`, vytvoří kontejner objektů blob s *obrázky* a frontu *obrázků*, pokud ještě neexistují. To zajistí, že při každém spuštění pomocí nového účtu úložiště požadovaný kontejner objektů blob a fronty jsou vytvořeny automaticky.

Kód získá přístup k účtu úložiště pomocí připojovacího řetězce úložiště ze *Web.config* soubor nebo Azure běhového prostředí.

        var storageAccount = CloudStorageAccount.Parse
            (ConfigurationManager.ConnectionStrings["AzureWebJobsStorage"].ToString());

Potom získá odkaz na *bitové kopie* kontejner objektů blob, vytvoří kontejner, pokud ještě neexistuje, a nastaví přístupová oprávnění na nový kontejner. Ve výchozím nastavení povolí nové kontejnery přístup k objektům BLOB jenom klientům pomocí přihlašovacích údajů účtu úložiště. Webová aplikace musí objekty BLOB byly veřejné, tak, aby ho nemohl zobrazit obrázky pomocí adres URL, které odkazují na objekty BLOB bitové kopie.

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

Podobný kód získá odkaz na *thumbnailrequest* fronty a vytvoří novou frontu. V tomto případě není nutná žádná oprávnění.

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        var imagesQueue = queueClient.GetQueueReference("thumbnailrequest");
        imagesQueue.CreateIfNotExists();

### <a name="contosoadsweb---layoutcshtml"></a>ContosoAdsWeb – _Layout.cshtml
Soubor *_Layout.cshtml* nastaví název aplikace v záhlaví a zápatí a vytvoří položku nabídky „Reklamy“.

### <a name="contosoadsweb---viewshomeindexcshtml"></a>ContosoAdsWeb – Views\Home\Index.cshtml
Soubor *Views\Home\Index.cshtml* zobrazuje na domovské stránce odkazy na kategorie. Odkazy předají celočíselnou hodnotu výčtu `Category` v proměnné řetězce dotazu na indexovou stránku reklam.

        <li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
        <li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
        <li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
        <li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>

### <a name="contosoadsweb---adcontrollercs"></a>ContosoAdsWeb – AdController.cs
V souboru *AdController.cs* volá konstruktor metodu `InitializeStorage`, aby vytvořil objekty knihovny klienta služby Azure Storage, které poskytují rozhraní API pro práci s objekty blob a frontami.

Potom kód získá odkaz na *bitové kopie* kontejner objektů blob, jako jste viděli dříve v *Global.asax.cs*. Během toho, nastaví výchozí [zásady opakování](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) vhodné pro webovou aplikaci. Výchozí zásady opakování exponenciálního omezení rychlosti můžou způsobit, že webová aplikace přestane při opakovaných pokusech reagovat na dobu delší než jednu minutu. Důvodem může být přechodná chyba. Zde určené zásady opakování čekají po každém pokusu tři sekundy a celkem provádějí tři pokusy.

        var blobClient = storageAccount.CreateCloudBlobClient();
        blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesBlobContainer = blobClient.GetContainerReference("images");

Podobný kód získá odkaz na frontu *obrázků*.

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesQueue = queueClient.GetQueueReference("blobnamerequest");

Většinu kódu kontroleru je typická pro práci s datovým modelem Entity Framework za použití třídy DbContext. Výjimkou je metoda HttpPost `Create`, která soubor odešle a uloží ho do úložiště objektů blob. Vazač modelu poskytuje metodě objekt [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx).

        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> Create(
            [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
            HttpPostedFileBase imageFile)

Pokud uživatel vybral soubor k odeslání, kód soubor odešle, uloží ho do objektu blob a zaktualizuje záznam v databázi reklam pomocí adresy URL, která odkazuje na objekt blob.

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            blob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = blob.Uri.ToString();
        }

Kód, který odesílání provádí, je umístěný v metodě `UploadAndSaveBlobAsync`. Vytvoří identifikátor GUID pro objekt blob, odešle a uloží soubor a vrátí odkaz na uložený objekt blob.

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

Po HttpPost `Create` metoda odešle objekt blob a zaktualizuje databázi, vytvoří zprávu fronty, aby proces back-end informovat, že obrázek je připravený k převodu na miniaturu.

        BlobInformation blobInfo = new BlobInformation() { AdId = ad.AdId, BlobUri = new Uri(ad.ImageURL) };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        await thumbnailRequestQueue.AddMessageAsync(queueMessage);

Kód pro HttpPost `Edit` metoda je podobné, s tím rozdílem, že pokud uživatel vybere nový obrázkový soubor, musíte odstranit všechny objekty BLOB, které jsou už pro tento ad.

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            await DeleteAdBlobsAsync(ad);
            imageBlob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = imageBlob.Uri.ToString();
        }

Tady je kód, který odstraní objekty BLOB při odstranění ad:

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
*Index.cshtml* zobrazí miniatury s dalšími daty reklam:

        <img  src="@Html.Raw(item.ThumbnailURL)" />

*Details.cshtml* zobrazí obrázek v plné velikosti:

        <img src="@Html.Raw(Model.ImageURL)" />

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a>ContosoAdsWeb – Views\Ad\Create.cshtml a Edit.cshtml
Soubory *Create.cshtml* a *Edit.cshtml* určují kódování formuláře, které kontroleru umožňuje získání objektu `HttpPostedFileBase`.

        @using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))

Prvek `<input>` sděluje prohlížeči, aby zobrazil dialogové okno pro výběr souboru.

        <input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />

### <a id="programcs"></a>ContosoAdsWebJob - Program.cs
Při spuštění vytvářené webové úlohy, `Main` metoda volá WebJobs SDK `JobHost.RunAndBlock` metoda zahájíte provádění aktivaci funkcí na aktuální vlákno.

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

### <a id="generatethumbnail"></a>Metoda GenerateThumbnail ContosoAdsWebJob - Functions.cs-
Sada WebJobs SDK volá tuto metodu při příjmu zprávy fronty. Metoda vytvoří miniaturu a uloží miniaturu adresy URL v databázi.

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
            // be instantiated and disposed within the function.
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

* `QueueTrigger` Atribut přesměruje WebJobs SDK mohli volat tuto metodu, když je obdržena novou zprávu ve frontě thumbnailrequest.

        [QueueTrigger("thumbnailrequest")] BlobInformation blobInfo,

    `BlobInformation` Objekt v zprávy ve frontě je automaticky deserializovat do `blobInfo` parametr. Po dokončení metody odstraní zprávu fronty. Pokud před dokončením selže metodu, nebude odstraněna zprávy ve frontě. Po vypršení platnosti zapůjčení 10 minut, zpráva vydané být zachyceny znovu a zpracovat. Toto pořadí nebude opakuje bez omezení, pokud zprávu vždy dojde k výjimce. Po 5 neúspěšných pokusech o zpracovat zprávu, zpráva bude přesunuta do fronty s názvem {queuename}-poškozených. Maximální počet pokusů o je možné konfigurovat.
* Dva `Blob` atributy zadejte objekty, které jsou vázány na objekty BLOB: jednu pro existující objekt blob image a jeden pro nové miniatur objekt blob, který vytvoří metodu.

        [Blob("images/{BlobName}", FileAccess.Read)] Stream input,
        [Blob("images/{BlobNameWithoutExtension}_thumbnail.jpg")] CloudBlockBlob outputBlob)

    Názvy objektů BLOB pocházet z vlastnosti `BlobInformation` objekt dostali zprávy ve frontě (`BlobName` a `BlobNameWithoutExtension`). Chcete-li získat úplné funkce Klientská knihovna pro úložiště, můžete použít `CloudBlockBlob` třídy pro práci s objekty BLOB. Pokud chcete znovu použít kód, který byl napsán pro práci s `Stream` objekty, můžete použít `Stream` třídy.

Další informace o tom, jak psát funkce, které se používají atributy WebJobs SDK najdete v následujících zdrojích informací:

* [Použití Azure Queue Storage se sadou WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Použití Azure Blob Storage se sadou WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Použití Azure Table Storage se sadou WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Použití Azure Service Bus se sadou WebJobs SDK](websites-dotnet-webjobs-sdk-service-bus.md)

> [!NOTE]
> * Pokud vaše webová aplikace běží na víc virtuálních počítačů, více webové úlohy budou používat současně a v některých případech může být výsledkem stejná data zpracovává více než jednou. To není problém, pokud používáte integrované fronty, objektů blob a aktivační události služby Service Bus. Sada SDK zajistí, že funkce budou zpracovány pouze jednou pro každou zprávu nebo objektů blob.
> * Informace o tom, jak implementovat řádné vypnutí najdete v tématu [řádné vypnutí](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).
> * Kód `ConvertImageToThumbnailJPG` – metoda (není vidět) používá třídy v `System.Drawing` obor názvů pro jednoduchost. Třídy v tomto oboru názvů však byly navrženy pro používání s formuláři Windows. Jejich používání není podporované ve Windows a službě ASP.NET. Další informace o možnostech zpracování obrázků najdete v článcích o [dynamickém generování obrázků](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) a o [hloubkové změně velikosti uvnitř obrázků](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).
>
>

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se seznámili jednoduché vícevrstvé aplikace, který využívá sadu WebJobs SDK pro zpracování back-end. Tato část nabízí některé návrhy pro dozvědět více o vícevrstvých aplikací ASP.NET a webové úlohy.

### <a name="missing-features"></a>Chybějící funkce
Aplikace má jednoduché kurz – Začínáme. V reálné aplikaci byste implementovat [vkládání závislostí](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) a [úložiště a jednotky pracovních vzorů](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), použijte [rozhraní pro protokolování](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), použijte [Migrace Code First EF](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) ke správě změn datových modelů a použít [odolnost připojení EF](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) k správě přechodných síťových chyb.

### <a name="scaling-webjobs"></a>Škálování webové úlohy
Webové úlohy spustit v kontextu webové aplikace a nejsou škálovatelné samostatně. Pokud máte jednu instanci standardní webové aplikace, máte jenom jednu instanci spuštěn proces na pozadí a používá některé prostředky serveru (procesoru, paměti, atd.), které jinak by být k dispozici k obsluze webového obsahu.

Pokud provoz se liší podle času den nebo den v týdnu, a pokud zpracování back-end, budete muset počkat, naplánujete vaší webové úlohy na spuštění v časech špičky. Pokud stále zatížení příliš vysoká. pro toto řešení, můžete spustit back-end jako webová úloha v samostatné webové aplikace, který je vyhrazený pro tento účel. Webové aplikace back-end je pak možné škálovat nezávisle z vaší webové aplikace front-endu.

Další informace najdete v tématu [škálování WebJobs](websites-webjobs-resources.md#scale).

### <a name="avoiding-web-app-timeout-shut-downs"></a>Webové aplikace časový limit vypnutí seznamy vyloučení
A ujistěte se, že vaše webové úlohy jsou vždy spuštěné a spuštěná na všech instancích vaší webové aplikace, je třeba povolit [AlwaysOn](http://weblogs.asp.net/scottgu/archive/2014/01/16/windows-azure-staging-publishing-support-for-web-sites-monitoring-improvements-hyper-v-recovery-manager-ga-and-pci-compliance.aspx) funkce.

### <a name="using-the-webjobs-sdk-outside-of-webjobs"></a>Pomocí sady SDK webové úlohy mimo webové úlohy
Program, který využívá sadu WebJobs SDK nemá na spuštění v Azure v webovou úlohu. Můžete spustit místně a také může být spuštěn v jiných prostředích, jako je například roli pracovního procesu cloudové služby nebo služby systému Windows. Ale můžete jenom přístup k řídicímu panelu WebJobs SDK prostřednictvím webové aplikace Azure. Použití řídicího panelu, budete muset webovou aplikaci připojit k účtu úložiště používáte nastavením AzureWebJobsDashboard připojovací řetězec **konfigurace** kartě portálu classic. Potom můžete získat na řídicí panel pomocí následující adresu URL:

https://{webappname}.SCM.azurewebsites.NET/azurejobs/#/functions

Další informace najdete v tématu [získávání řídicí panel pro místní vývoj pomocí WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), ale Všimněte si, že se zobrazí původní název připojovacího řetězce.

### <a name="more-webjobs-documentation"></a>Další dokumentaci webové úlohy
Další informace najdete v tématu [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226).
