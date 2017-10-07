---
title: aaaBuild aplikace ASP.NET v Azure SQL Database | Microsoft Docs
description: "Zjistěte, jak tooget ASP.NET aplikace v Azure, funguje s tooa připojení databáze SQL."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 03c584f1-a93c-4e3d-ac1b-c82b50c75d3e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 06/09/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: d21c2bc404bfe038608c17e5a94d96847153002c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-an-aspnet-app-in-azure-with-sql-database"></a>Sestavení aplikace ASP.NET v Azure SQL Database

[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů. Tento kurz ukazuje, jak toodeploy ASP.NET se datové webové aplikace v Azure a připojte ho příliš[Azure SQL Database](../sql-database/sql-database-technical-overview.md). Jakmile budete hotovi, máte aplikaci ASP.NET spuštěná [Azure App Service](../app-service/app-service-value-prop-what-is.md) a připojené tooSQL databáze.

![Publikované aplikace ASP.NET ve službě Azure web app](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření databáze SQL v Azure
> * Připojit tooSQL aplikace ASP.NET databáze
> * Nasazení aplikace tooAzure hello
> * Aktualizovat hello datový model a znovu nasaďte aplikace hello
> * Datový proud protokolů z Azure tooyour terminálu
> * Spravovat aplikace hello v hello portálu Azure

## <a name="prerequisites"></a>Požadavky

toocomplete v tomto kurzu:

* Nainstalujte [Visual Studio 2017](https://www.visualstudio.com/downloads/) s hello následující úlohy:
  - **Vývoj pro ASP.NET a web**
  - **Azure – vývoj**

  ![Vývoj pro ASP.NET a Azure – vývoj (v části Web a cloud)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample"></a>Stažení ukázky hello

[Stáhněte si ukázkový projekt hello](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).

Extrahování (rozbalte) hello *dotnet-sqldb kurzu master.zip* souboru.

Hello ukázkový projekt obsahuje základní [ASP.NET MVC](https://www.asp.net/mvc) CRUD (vytvoření čtení aktualizace odstranění) aplikace pomocí [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

### <a name="run-hello-app"></a>Spuštění aplikace hello

Otevřete hello *dotnet-sqldb – kurz – hlavní/DotNetAppSqlDb.sln* souborů v sadě Visual Studio. 

Typ `Ctrl+F5` toorun hello aplikace bez ladění. Hello aplikace se zobrazí ve výchozím prohlížeči. Vyberte hello **vytvořit nový** propojit a vytvořit pár *úkolů* položky. 

![Dialogové okno Nový projekt ASP.NET](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

Test hello **upravit**, **podrobnosti**, a **odstranit** odkazy.

aplikace Hello používá tooconnect kontextu databáze s databází hello. V této ukázce kontext databáze hello používá připojovací řetězec s názvem `MyDbConnection`. Hello připojovací řetězec je nastavena v hello *Web.config* souborové služby a odkazovaná v hello *Models/MyDatabaseContext.cs* souboru. název připojovacího řetězce Hello se používá novější v hello kurz tooconnect hello službě Azure web app tooan Azure SQL Database. 

## <a name="publish-tooazure-with-sql-database"></a>Publikování tooAzure s databází SQL

V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na vaše **DotNetAppSqlDb** projektu a vyberte **publikovat**.

![Publikování z Průzkumníka řešení](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Zkontrolujte, že je vybraná možnost **Microsoft Azure App Service** a klikněte na **Publikovat**.

![Publikování ze stránky přehledu projektu](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

Publikování otevře hello **vytvořit službu App Service** dialogové okno, které vám pomůže vytvořit všechny hello prostředky Azure, budete potřebovat toorun webové aplikace ASP.NET v Azure.

### <a name="sign-in-tooazure"></a>Přihlaste se tooAzure

V hello **vytvořit službu App Service** dialogové okno, klikněte na tlačítko **přidat účet**a potom se přihlaste tooyour předplatného Azure. Pokud jste již přihlášení k účtu Microsoft, ujistěte se, že odpovídá vašemu předplatnému Azure. Pokud hello přihlášeného účtu Microsoft nemá vašeho předplatného Azure, klikněte na něj tooadd hello správný účet.
   
![Přihlaste se tooAzure](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

Jakmile se přihlásíte, jste připravené toocreate všechny prostředky, které potřebujete k Azure webové aplikace v tomto dialogovém okně hello.

### <a name="configure-hello-web-app-name"></a>Nakonfigurujte název webové aplikace hello

Můžete ponechat hello vygeneruje název webové aplikace, nebo ho změnit jedinečný název tooanother (platnými znaky jsou `a-z`, `0-9`, a `-`). Název webové aplikace Hello se používá jako součást hello výchozí adresa URL pro aplikaci (`<app_name>.azurewebsites.net`, kde `<app_name>` je název vaší webové aplikace). Název webové aplikace Hello musí toobe jedinečný mezi všechny aplikace v Azure. 

![Vytvoření dialogovém okně app service](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Další příliš**skupiny prostředků**, klikněte na tlačítko **nový**.

![Další tooResource skupiny, klepněte na tlačítko Nový.](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

Název skupiny prostředků hello **myResourceGroup**.

> [!NOTE]
> Neklikejte na **vytvořit**. Je nutné nejprve tooset vytvářet databáze SQL v pozdější fázi.

### <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Další příliš**plán služby App Service**, klikněte na tlačítko **nový**. 

V hello **nakonfigurovat plán služby App Service** dialogu Nový plán aplikační služby hello nakonfigurovat hello následující nastavení:

![Vytvoření plánu služby App Service](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

| Nastavení  | Navrhovaná hodnota | Další informace |
| ----------------- | ------------ | ----|
|**Plán služby App Service**| myAppServicePlan | [Plány služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |
|**Umístění**| Západní Evropa | [Oblasti Azure](https://azure.microsoft.com/regions/) |
|**Velikost**| Free | [Cenové úrovně](https://azure.microsoft.com/pricing/details/app-service/)|

### <a name="create-a-sql-server-instance"></a>Vytvoření instance systému SQL Server

Před vytvořením databázi, budete potřebovat [logického serveru Azure SQL Database](../sql-database/sql-database-features.md). Logický server obsahuje soubor databází spravovaných jako skupina.

Vyberte **Objevte další služby Azure**.

![Nastavení názvu webové aplikace](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

V hello **služby** , klikněte na hello  **+**  ikonu další příliš**SQL Database**. 

![V kartě hello služeb, klikněte na ikonu + hello další tooSQL databáze.](media/app-service-web-tutorial-dotnet-sqldatabase/sql.png)

V hello **nakonfigurovat databázi SQL** dialogové okno, klikněte na tlačítko **nový** další příliš**systému SQL Server**. 

Generuje se název jedinečný serveru. Tento název se používá jako součást výchozí adresa URL hello logického serveru, `<server_name>.database.windows.net`. Musí být ve všech instancích logický server v Azure jedinečný. Můžete změnit název serveru hello, ale pro účely tohoto kurzu, ponechte hodnotu hello vygenerovat.

Přidat správce uživatelské jméno a heslo a potom vyberte **OK**. Požadavky na složitost hesla, najdete v části [zásady hesel](/sql/relational-databases/security/password-policy).

Mějte na paměti Toto uživatelské jméno a heslo. Musíte je logický server hello toomanage instance později.

![Vytvoření instance systému SQL Server](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

### <a name="create-a-sql-database"></a>Vytvoření databáze SQL

V hello **nakonfigurovat databázi SQL** dialogové okno: 

* Ponechat výchozí hello generovaný **název databáze**.
* V **název připojovacího řetězce**, typ *MyDbConnection*. Tento název musí odpovídat hello připojovací řetězec, který se odkazuje v *Models/MyDatabaseContext.cs*.
* Vyberte **OK**.

![Konfigurace databáze SQL](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

Hello **vytvořit službu App Service** dialog zobrazuje hello prostředky jste vytvořili. Klikněte na možnost **Vytvořit**. 

![Hello prostředky, které jste vytvořili](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

Po dokončení Průvodce hello hello vytváření prostředků Azure, publikuje vaše tooAzure aplikace ASP.NET. Výchozí prohlížeč se spustí s hello URL toohello nasazené aplikace. 

Přidejte několik položek úkolů.

![Publikované aplikace ASP.NET ve službě Azure web app](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

Blahopřejeme! Vaše aplikace ASP.NET řízené daty je spuštěna za provozu v Azure App Service.

## <a name="access-hello-sql-database-locally"></a>Přístup k místně hello databáze SQL

Visual Studio umožňuje prozkoumat a spravovat nové databáze SQL snadno v hello **Průzkumník objektů systému SQL Server**.

### <a name="create-a-database-connection"></a>Vytvoření připojení k databázi

Z hello **zobrazení** nabídce vyberte možnost **Průzkumník objektů systému SQL Server**.

Na začátku hello **Průzkumník objektů systému SQL Server**, klikněte na tlačítko hello **přidat SQL Server** tlačítko.

### <a name="configure-hello-database-connection"></a>Konfigurace připojení k databázi hello

V hello **připojit** dialogové okno, rozbalte položku hello **Azure** uzlu. Všechny instance databáze SQL v Azure jsou zde uvedeny.

Vyberte hello `DotNetAppSqlDb` databáze SQL. v dolní části hello se vyplní automaticky Hello připojení, které jste vytvořili dříve.

Zadejte heslo správce databáze hello jste dříve vytvořili a klikněte na **Connect**.

![Konfigurace připojení k databázi ze sady Visual Studio](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a>Povolit připojení klienta z vašeho počítače

Hello **vytvořit nové pravidlo brány firewall** se otevře dialogové okno. Ve výchozím nastavení umožňuje vaší instanci služby SQL Database pouze připojení ze služby Azure, jako je například Azure webové aplikace. tooconnect tooyour databáze, vytvořte pravidlo brány firewall v instanci služby SQL Database hello. pravidlo brány firewall Hello umožňuje hello veřejnou IP adresu místního počítače.

Dialogové okno Hello je již vyplněn veřejná IP adresa vašeho počítače.

Ujistěte se, že **přidat Moje IP adresa klienta** je vybrána a klikněte na tlačítko **OK**. 

![Nastavení brány firewall pro instanci databáze SQL](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

Jakmile sady Visual Studio dokončí vytváření hello nastavení brány firewall pro instanci SQL databáze, připojení se zobrazí v **Průzkumník objektů systému SQL Server**.

Zde můžete provádět hello nejběžnější databázových operací, například spuštění dotazů, vytvořit zobrazení a uložených procedur a další. 

Klikněte pravým tlačítkem na hello `Todoes` tabulky a vyberte **Data zobrazení**. 

![Prozkoumejte objekty databáze SQL](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="update-app-with-code-first-migrations"></a>Aktualizovat aplikaci migrace Code First

Můžete vytvořit hello známých nástrojů v sadě Visual Studio tooupdate databáze a webové aplikace v Azure. V tomto kroku použijete migrace Code First v Entity Framework toomake schéma databáze tooyour změn a publikujete ji tooAzure.

Další informace o použití migrace Entity Framework Code First najdete v tématu [Začínáme s Entity Framework 6 Code First pomocí MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

### <a name="update-your-data-model"></a>Aktualizovat data modelu

Otevřete _Models\Todo.cs_ v editoru kódu hello. Přidejte následující vlastnost toohello hello `ToDo` třídy:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Spustit migrace Code First místně

Spusťte několik příkazů toomake aktualizace tooyour místní databáze. 

Z hello **nástroje** nabídky, klikněte na tlačítko **Správce balíčků NuGet** > **Konzola správce balíčků**.

V okně konzoly Správce balíčků hello povolte migrace Code First:

```PowerShell
Enable-Migrations
```

Přidejte migrace:

```PowerShell
Add-Migration AddProperty
```

Aktualizujte místní databázi hello:

```PowerShell
Update-Database
```

Typ `Ctrl+F5` toorun hello aplikace. Test hello upravit podrobnosti a vytvoření odkazů.

Pokud aplikace hello načte bez chyb, proběhla úspěšně migrace Code First. Však stále vypadá vaše stránka hello stejné protože aplikační logiku ještě nepoužívá tuto novou vlastnost. 

### <a name="use-hello-new-property"></a>Použít novou vlastnost hello

Provedeme některé změny v váš kód toouse hello `Done` vlastnost. Pro zjednodušení v tomto kurzu pouze budete toochange hello `Index` a `Create` zobrazení vlastnost hello toosee v akci.

Otevřete _Controllers\TodosController.cs_.

Najde hello `Create()` metoda a přidejte `Done` toohello seznam vlastností v hello `Bind` atribut. Když jste hotovi, vaše `Create()` podpis metody vypadá hello následující kód:

```csharp
public ActionResult Create([Bind(Include = "id,Description,CreatedDate,Done")] Todo todo)
```

Otevřete _Views\Todos\Create.cshtml_.

V hello kódu Razor, měli byste vidět `<div class="form-group">` element, který používá `model.Description`a pak další `<div class="form-group">` element, který používá `model.CreatedDate`. Hned za tyto dva prvky, přidejte další `<div class="form-group">` element, který používá `model.Done`:

```csharp
<div class="form-group">
    @Html.LabelFor(model => model.Done, htmlAttributes: new { @class = "control-label col-md-2" })
    <div class="col-md-10">
        <div class="checkbox">
            @Html.EditorFor(model => model.Done)
            @Html.ValidationMessageFor(model => model.Done, "", new { @class = "text-danger" })
        </div>
    </div>
</div>
```

Otevřete _Views\Todos\Index.cshtml_.

Vyhledejte hello prázdný `<th></th>` elementu. Nad tento element přidejte následující kódu Razor hello:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Najde hello `<td>` elementu, který obsahuje hello `Html.ActionLink()` pomocné metody. Nad tento element přidejte následující kódu Razor hello:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

To je všechno potřebujete toosee hello změny v hello `Index` a `Create` zobrazení. 

Typ `Ctrl+F5` toorun hello aplikace.

Teď můžete přidat položku seznamu úkolů a zkontrolujte **provádí**. Potom ho měli zobrazí v domovské stránce jako dokončené položka. Mějte na paměti, že hello `Edit` zobrazení nezobrazí hello `Done` pole, protože jste nezměnili hello `Edit` zobrazení.

### <a name="enable-code-first-migrations-in-azure"></a>Povolení migrace Code First v Azure

Teď, když kód změnit funguje, včetně migrace databáze, můžete ji publikovat tooyour webové aplikace Azure a příliš aktualizace databáze SQL se migrace Code First.

Podobně jako před, klikněte pravým tlačítkem na projekt a vyberte **publikovat**.

Klikněte na tlačítko **nastavení** Průvodce publikováním tooopen hello.

![Otevřete nastavení publikování](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

V Průvodci hello, klikněte na tlačítko **Další**.

Zajistěte, aby tento hello připojovací řetězec pro databáze SQL se naplní v **MyDatabaseContext (MyDbConnection)**. Může být nutné tooselect hello **myToDoAppDb** databáze z rozevíracího seznamu hello. 

Vyberte **spustit migrace Code First (spuštěno při spuštění aplikace)**, pak klikněte na tlačítko **Uložit**.

![Povolení migrace Code First ve webové aplikace Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### <a name="publish-your-changes"></a>Publikování změn

Teď, když jste povolili migrace Code First ve vaší webové aplikace Azure, publikování změn kódu.

V hello publikovat stránku, klikněte na tlačítko **publikovat**.

Přidejte položkami seznamu úkolů znovu a vyberte **provádí**, a měli objeví v domovské stránce jako dokončené položka.

![Webové aplikace Azure po migraci první kódu](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Všechny vaše existující položky seznamu úkolů se pořád zobrazí. Při opětovném aplikace ASP.NET, není stávající data v databázi SQL ztraceny. Navíc migrace Code First pouze změní schéma dat hello a svoje existující data zůstanou zachovány.


## <a name="stream-application-logs"></a>Protokoly aplikací datového proudu

Přímo z vaší službě Azure web app tooVisual Studio dá Streamovat trasování zpráv.

Otevřete _Controllers\TodosController.cs_.

Každá akce začíná `Trace.WriteLine()` metoda. Tento kód se přidá tooshow můžete jak tooadd trasování zprávy tooyour webové aplikace Azure.

### <a name="open-server-explorer"></a>V Průzkumníku serveru otevřete

Z hello **zobrazení** nabídce vyberte možnost **Průzkumníka serveru**. Můžete nakonfigurovat protokolování pro Azure webové aplikace v **Průzkumníka serveru**. 

### <a name="enable-log-streaming"></a>Povolení protokolu streamování

V **Průzkumníka serveru**, rozbalte položku **Azure** > **služby App Service**.

Rozbalte hello **myResourceGroup** skupinu prostředků, jste vytvořili při prvním vytvoření hello webové aplikace Azure.

Klikněte pravým tlačítkem na vaší webové aplikace Azure a vyberte **zobrazit protokoly streamování**.

![Povolení protokolu streamování](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

Hello protokoly jsou nyní datového proudu do hello **výstup** okno. 

![V okně výstupu datový proud protokolu](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

Ale nezobrazí žádná hello trasování zpráv ještě. To je proto když nejprve vyberete **zobrazit protokoly streamování**, vaší webové aplikace Azure nastaví úroveň trasování hello příliš`Error`, který jenom protokoluje události chyby (s hello `Trace.TraceError()` metoda).

### <a name="change-trace-levels"></a>Změna úrovně trasování

trasování hello toochange úrovně toooutput dalších trasování zpráv, přejděte zpět příliš**Průzkumníka serveru**.

Znovu klikněte pravým tlačítkem Azure webové aplikace a vyberte **nastavení**.

V hello **protokolování aplikace (systém souborů)** rozevíracího seznamu vyberte **podrobné**. Klikněte na **Uložit**.

![Změna úrovně tooVerbose trasování](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> Můžete experimentovat s jinou trasování úrovně toosee jaké typy zprávy se zobrazují pro každou úroveň. Například hello **informace** úroveň zahrnuje všechny protokoly, které jsou vytvořené `Trace.TraceInformation()`, `Trace.TraceWarning()`, a `Trace.TraceError()`, ale není protokoly vytvořené `Trace.WriteLine()`.
>
>

V prohlížeči zkuste klepnout kolem hello aplikaci seznamu úkolů v Azure. Hello trasovací zprávy jsou nyní streamování toohello **výstup** oken v sadě Visual Studio.

```
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```



### <a name="stop-log-streaming"></a>Zastavit streamování protokolu

toostop hello vysílání datového proudu protokolu služby, klikněte na tlačítko hello **zastavit monitorování** tlačítka na hello **výstup** okno.

![Zastavit streamování protokolu](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="manage-your-azure-web-app"></a>Správa Azure webové aplikace

Přejděte toohello [portál Azure](https://portal.azure.com) toosee hello webovou aplikaci jste vytvořili. 



V levé nabídce hello, klikněte na **služby App Service**, pak klikněte na název hello Azure webové aplikace.

![Portálu tooAzure webové aplikace](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

Dostali jste na stránce vaší webové aplikace. 

Ve výchozím nastavení, hello portál zobrazuje hello **přehled** stránky. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. Hello karty na levé straně stránky hello hello zobrazit stránky hello jinou konfiguraci, které můžete otevřít. 

![Stránka služby App Service na webu Azure Portal](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

<a name="next"></a>

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření databáze SQL v Azure
> * Připojit tooSQL aplikace ASP.NET databáze
> * Nasazení aplikace tooAzure hello
> * Aktualizovat hello datový model a znovu nasaďte aplikace hello
> * Datový proud protokolů z Azure tooyour terminálu
> * Spravovat aplikace hello v hello portálu Azure

Posunutí toohello další kurz toolearn jak toomap vlastní DNS název toohello webové aplikace.

> [!div class="nextstepaction"]
> [Mapovat existující vlastní DNS název tooAzure webové aplikace](app-service-web-tutorial-custom-domain.md)
