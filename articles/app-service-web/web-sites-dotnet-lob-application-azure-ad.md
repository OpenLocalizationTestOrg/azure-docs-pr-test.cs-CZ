---
title: "aaaCreate aplikace obchodní Azure pomocí ověřování Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak toocreate rozhraní ASP.NET MVC – obchodní aplikace v Azure App Service, se ověřuje Azure Active Directory"
services: app-service\web, active-directory
documentationcenter: .net
author: cephalin
manager: erikre
editor: 
ms.assetid: ad947bdb-4463-43ff-a5e3-91d9b2169b60
ms.service: app-service-web
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 09/01/2016
ms.author: cephalin
ms.openlocfilehash: 3bcafad78ac0151889b3e336784cc561009f244f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-line-of-business-azure-app-with-azure-active-directory-authentication"></a>Vytvoření aplikace Azure-obchodní pomocí ověřování Azure Active Directory
Tento článek ukazuje, jak toocreate .NET – obchodní aplikace v [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) pomocí hello [ověřování / autorizace](../app-service/app-service-authentication-overview.md) funkce. Také ukazuje, jak toouse hello [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) tooquery data adresáře v aplikaci hello.

Hello klienta Azure Active Directory, který používáte, může být adresář jen Azure. Nebo může být [synchronizaci se službou Active Directory v místě](../active-directory/active-directory-aadconnect.md) toocreate jediného přihlašování rozhraní pro pracovní procesy, které jsou místní a vzdálené. Tento článek používá hello výchozí adresář pro váš účet Azure.

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a>Co se sestavení
Bude vytvořit jednoduchou aplikaci vytvořit-čtení-aktualizace-odstranění (CRUD)-obchodní v App Service Web Apps, že sleduje pracovní položky s hello následující funkce:

* Ověřuje uživatele na základě Azure Active Directory
* Dotazuje adresáře uživatelů a skupin pomocí [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx)
* Použití hello ASP.NET MVC *bez ověřování* šablony

Pokud potřebujete řízení přístupu na základě role (RBAC)-obchodní aplikace v Azure, najdete v části [další krok](#next).

<a name="bkmk_need"></a>

## <a name="what-you-need"></a>Co potřebujete
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

V tomto kurzu se třeba hello následující toocomplete:

* Klient služby Azure Active Directory s uživateli v různých skupinách
* Oprávnění aplikací toocreate na klienta Azure Active Directory hello
* Visual Studio 2013 Update 4 nebo novější
* [Azure SDK 2.8.1 nebo novější](https://azure.microsoft.com/downloads/)

<a name="bkmk_deploy"></a>

## <a name="create-and-deploy-a-web-app-tooazure"></a>Vytvoření a nasazení webové aplikace tooAzure
1. Ze sady Visual Studio, klikněte na tlačítko **soubor** > **nový** > **projektu**.
2. Vyberte **webové aplikace ASP.NET**, pojmenujte svůj projekt a klikněte na tlačítko **OK**.
3. Vyberte hello **MVC** šablony, změňte hello ověřování příliš**bez ověřování**. Zajistěte, aby **hostitel v cloudu hello** je vybrána a klikněte na tlačítko **OK**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/1-create-mvc-no-authentication.png)
4. V hello **vytvořit službu App Service** dialogové okno, klikněte na tlačítko **přidat účet** (a potom **přidat účet** v rozevírací nabídce hello) toolog v tooyour účet Azure.
5. Po přihlášení nakonfigurujte webové aplikace. Vytvořit skupinu prostředků a nový plán služby App Service kliknutím na tlačítko hello příslušných **nový** tlačítko. Klikněte na tlačítko **Objevte další služby Azure** toocontinue.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/2-create-app-service.png)
6. V hello **služby** , klikněte na  **+**  tooadd databáze SQL pro vaši aplikaci. 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/3-add-sql-database.png)
7. V **nakonfigurovat databázi SQL**, klikněte na tlačítko **nový** toocreate instance systému SQL Server.
8. V **nakonfigurujte systém SQL Server**, nakonfigurovat instanci SQL serveru. Potom klikněte na **OK**, **OK**, a **vytvořit** tookick vypnout hello vytváření aplikace v Azure.
9. V **aktivita služby Azure App Service**, se zobrazí po dokončení vytváření aplikace hello. Klikněte na tlačítko  **publikovat &lt;* appname*> toothis webové aplikace teď **, pak klikněte na tlačítko **publikovat**. 
   
    Jakmile sady Visual Studio dokončí, otevře se hello publikování aplikace v prohlížeči hello. 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/4-published-shown-in-browser.png)

<a name="bkmk_auth"></a>

## <a name="configure-authentication-and-directory-access"></a>Konfigurace ověřování a directory přístupu
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V levé nabídce hello, klikněte na **App Services** > **&lt;*appname*> ** > **ověřování / autorizace**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/5-app-service-authentication.png)
3. Zapnout ověřování Azure Active Directory kliknutím **na** > **Azure Active Directory** > **Express** > **OK**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/6-authentication-express.png)
4. Klikněte na tlačítko **Uložit** hello panelu příkazů.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/7-authentication-save.png)
   
    Po nastavení hello ověřování se úspěšně uložily, zkuste navigace tooyour aplikaci znovu v prohlížeči hello. Výchozí nastavení vynucení ověření hello celou aplikaci. Pokud už nejste přihlášeni, budete přesměrovaného tooa přihlašovací obrazovku. Po přihlášení, by se zobrazit aplikaci zabezpečené pomocí protokolu HTTPS. Dále musíte tooenable přístup toodirectory data. 
5. Přejděte toohello [portálu classic](https://manage.windowsazure.com).
6. V levé nabídce hello, klikněte na **služby Active Directory** > **výchozí adresář** > **aplikace**  >   **&lt;* appname*> **.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/8-find-aad-application.png)
   
    Toto je aplikace hello Azure Active Directory, vytvořené pro jste tooenable hello autorizace služby App Service nebo funkce ověřování.
7. Klikněte na tlačítko **uživatelé** a **skupiny** toomake, že máte některé uživatele a skupiny v adresáři hello. Pokud ne, vytvořte několik testovací uživatele a skupiny.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/9-create-users-groups.png)
8. Klikněte na tlačítko **konfigurace** tooconfigure této aplikace.
9. Projděte dolů toohello **klíče** a přidejte klíč výběrem dobu trvání. Potom klikněte na **delegovaná oprávnění** a vyberte **čtení dat adresáře**. 
   Klikněte na **Uložit**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/10-configure-aad-application.png)
10. Jakmile se vaše nastavení se ukládají, posuňte se zálohování toohello **klíče** části a klikněte na tlačítko hello **kopie** klíč klienta hello toocopy tlačítko. 
    
     ![](./media/web-sites-dotnet-lob-application-azure-ad/11-get-app-key.png)
    
    > [!IMPORTANT]
    > Pokud jste tuto stránku opustit nyní, nebudete moct tooaccess někdy znovu klíče tohoto klienta.
    > 
    > 
11. Dále musíte tooconfigure vaší webové aplikace s tímto klíčem. Přihlaste se toohello [Průzkumníka prostředků Azure](https://resources.azure.com) s vaším účtem Azure.
12. V horní části hello hello stránky, klikněte na tlačítko **pro čtení a zápis** toomake změny v hello Průzkumníka prostředků Azure.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/12-resource-manager-writable.png)
13. Najít nastavení ověřování pro aplikace umístěné v odběry hello >  **&lt;* název_předplatného*> ** > **Skupinyprostředků**  >   **&lt;* resourcegroupname*> ** > **zprostředkovatelé** > **Microsoft.Web**  >  **lokality** > **&lt;*appname*> ** > **konfigurace**  >  **authsettings**.
14. Klikněte na **Upravit**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/13-edit-authsettings.png)
15. V podokně úpravy hello, nastavte hello `clientSecret` a `additionalLoginParams` vlastnosti následujícím způsobem.
    
        ...
        "clientSecret": "<client key from hello Azure Active Directory application>",
        ...
        "additionalLoginParams": ["response_type=code id_token", "resource=https://graph.windows.net"],
        ...
16. Klikněte na tlačítko **Put** v hello nejvyšší toosubmit změny.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/14-edit-parameters.png)
17. Nyní, tootest, pokud máte hello autorizační token tooaccess hello Azure Active Directory Graph API, jednoduše přejděte k  **https://&lt;*appname*>.azurewebsites.net/.auth/me** ve vaší prohlížeč. Pokud jste nakonfigurovali všechno správně, měli byste vidět hello `access_token` vlastnost hello odpověď JSON.
    
    Hello `~/.auth/me` cestu adresy URL spravuje aplikace služby ověřování / autorizace toogive můžete všechny hello informace týkající se relace tooyour ověření. Další informace najdete v tématu [ověřování a autorizace ve službě Azure App Service](../app-service/app-service-authentication-overview.md).
    
    > [!NOTE]
    > Hello `access_token` má dobu platnosti. Ale aplikace služby ověřování / autorizace poskytuje funkce tokenu aktualizace s `~/.auth/refresh`. Další informace o tom, toouse, najdete v části [obchod Token služby](https://cgillum.tech/2016/03/07/app-service-token-store/).
    > 
    > 

V dalším kroku provedete něco užitečný v případě dat adresáře.

<a name="bkmk_crud"></a>

## <a name="add-line-of-business-functionality-tooyour-app"></a>Přidat aplikaci tooyour obchodní – funkce
Nyní můžete vytvořit jednoduché sledovací modul CRUD položky pracovní.  

1. Ve složce ~\Models hello, vytvořte soubor třídy s názvem WorkItem.cs a nahraďte `public class WorkItem {...}` s hello následující kód:
   
     pomocí System.ComponentModel.DataAnnotations;
   
     Veřejná třída {pracovní položky
   
         [Key]
         public int ItemID { get; set; }
         public string AssignedToID { get; set; }
         public string AssignedToName { get; set; }
         public string Description { get; set; }
         public WorkItemStatus Status { get; set; }
     }
   
     Veřejný výčet WorkItemStatus {
   
         Open,
         Investigating,
         Resolved,
         Closed
     }
2. Sestavení projektu toomake hello nový model přístupné toohello generování uživatelského rozhraní logika v sadě Visual Studio.
3. Přidat novou položku vygenerované `WorkItemsController` toohello ~\Controllers složky (klikněte pravým tlačítkem na **řadiče**, bod příliš**přidat**a vyberte **nové vygenerované položky**). 
4. Vyberte **kontroler MVC 5 se zobrazeními s využitím nástroje Entity Framework** a klikněte na tlačítko **přidat**.
5. Vyberte hello model, který jste vytvořili, klikněte  **+**  a potom **přidat** tooadd data kontextu a potom klikněte na **přidat**.
   
   ![](./media/web-sites-dotnet-lob-application-azure-ad/16-add-scaffolded-controller.png)
6. V ~\Views\WorkItems\Create.cshtml (automaticky vygenerované položky), vyhledejte hello `Html.BeginForm` pomocnou metodu a nastavit hello následující zvýrazněný změny:  
   
   <pre class="prettyprint">
   @model WebApplication1.Models.WorkItem
   
   @{
    ViewBag.Title = &quot;Create&quot;;
   }
   
   &lt;h2&gt;Create&lt;/h2&gt;
   
   @using (Html.BeginForm(<mark>&quot;Create&quot;, &quot;WorkItems&quot;, FormMethod.Post, new { id = &quot;main-form&quot; }</mark>)) 
   {
    @Html.AntiForgeryToken()
   
    &lt;div class=&quot;form-horizontal&quot;&gt;
        &lt;h4&gt;WorkItem&lt;/h4&gt;
        &lt;hr /&gt;
        @Html.ValidationSummary(true, &quot;&quot;, new { @class = &quot;text-danger&quot; })
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.AssignedToID, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.AssignedToID, new { htmlAttributes = new { @class = &quot;form-control&quot;<mark>, @type = &quot;hidden&quot;</mark> } })
                @Html.ValidationMessageFor(model =&gt; model.AssignedToID, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.AssignedToName, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.AssignedToName, new { htmlAttributes = new { @class = &quot;form-control&quot; } })
                @Html.ValidationMessageFor(model =&gt; model.AssignedToName, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.Description, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.Description, new { htmlAttributes = new { @class = &quot;form-control&quot; } })
                @Html.ValidationMessageFor(model =&gt; model.Description, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.Status, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EnumDropDownListFor(model =&gt; model.Status, htmlAttributes: new { @class = &quot;form-control&quot; })
                @Html.ValidationMessageFor(model =&gt; model.Status, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            &lt;div class=&quot;col-md-offset-2 col-md-10&quot;&gt;
                &lt;input type=&quot;submit&quot; value=&quot;Create&quot; class=&quot;btn btn-default&quot;<mark> id=&quot;submit-button&quot;</mark> /&gt;
            &lt;/div&gt;
        &lt;/div&gt;
    &lt;/div&gt;
   }
   
   &lt;div&gt;
    @Html.ActionLink(&quot;Back tooList&quot;, &quot;Index&quot;)
   &lt;/div&gt;
   
   @section Scripts {
    @Scripts.Render(&quot;~/bundles/jqueryval&quot;)
    <mark>&lt;script&gt;
        // People/Group Picker Code
        var maxResultsPerPage = 14;
        var input = document.getElementById(&quot;AssignedToName&quot;);
   
        // Access token from request header, and tenantID from claims identity
        var token = &quot;@Request.Headers[&quot;X-MS-TOKEN-AAD-ACCESS-TOKEN&quot;]&quot;;
        var tenant =&quot;@(System.Security.Claims.ClaimsPrincipal.Current.Claims
                        .Where(c => c.Type == &quot;http://schemas.microsoft.com/identity/claims/tenantid&quot;)
                        .Select(c => c.Value).SingleOrDefault())&quot;;
   
        var picker = new AadPicker(maxResultsPerPage, input, token, tenant);
   
        // Submit hello selected user/group toobe asssigned.
        $(&quot;#submit-button&quot;).click({ picker: picker }, function () {
            if (!picker.Selected())
                return;
            $(&quot;#main-form&quot;).get()[0].elements[&quot;AssignedToID&quot;].value = picker.Selected().objectId;
        });
    &lt;/script&gt;</mark>
   }
   </pre>
   
   Všimněte si, že `token` a `tenant` jsou používány hello `AadPicker` toomake objekt volá Azure Active Directory Graph API. Přidáte `AadPicker` později.     
   
   > [!NOTE]
   > Stejně dobře získáte `token` a `tenant` z hello na straně klienta s `~/.auth/me`, ale, že by být volání další server. Například:
   > 
   > $.ajax ({datový typ: "json", adresa url: "/.auth/me", Úspěch: funkce (data) {var token = data [0] .access_token; var klienta = dat [0] .user_claims .find (c = > c.typ === 'http://schemas.microsoft.com/identity/claims/tenantid') .val;}});
   > 
   > 
7. Provést stejné změny s hello ~ \Views\WorkItems\Edit.cshtml.
8. Hello `AadPicker` objektu je definována ve skriptu, je nutné, aby tooadd tooyour projektu. Klikněte pravým tlačítkem na složku ~\Scripts hello, přejděte příliš**přidat**a klikněte na tlačítko **soubor JavaScript**. Typ `AadPickerLibrary` hello název souboru a klikněte na **OK**.
9. Zkopírujte obsah hello z [sem](https://raw.githubusercontent.com/cephalin/active-directory-dotnet-webapp-roleclaims/master/WebApp-RoleClaims-DotNet/Scripts/AadPickerLibrary.js) do ~ \Scripts\AadPickerLibrary.js.
   
   Ve skriptu hello hello `AadPicker` objektu volání [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) toosearch pro uživatele a skupiny, které odpovídají vstupní hello.  
10. ~\Scripts\AadPickerLibrary.js také používá hello [pomůcky Autocomplete uživatelského rozhraní jQuery](https://jqueryui.com/autocomplete/). Proto musíte tooadd jQuery UI tooyour projektu. Klikněte pravým tlačítkem na projekt v a klikněte na tlačítko **spravovat balíčky NuGet**.
11. V hello Správce balíčků NuGet, klikněte na tlačítko Procházet, typ **uživatelského rozhraní jquery** v hello panelu vyhledávání a klikněte na tlačítko **jQuery.UI.Combined**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/17-add-jquery-ui-nuget.png)
12. V pravém podokně hello, klikněte na **nainstalovat**, pak klikněte na tlačítko **OK** tooproceed.
13. Otevřete ~\App_Start\BundleConfig.cs a proveďte následující zvýrazněný změny hello:  
    
    <pre class="prettyprint">
    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle(&quot;~/bundles/jquery&quot;).Include(
                    &quot;~/Scripts/jquery-{version}.js&quot;<mark>,
                    &quot;~/Scripts/jquery-ui-{version}.js&quot;,
                    &quot;~/Scripts/AadPickerLibrary.js&quot;</mark>));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/jqueryval&quot;).Include(
                    &quot;~/Scripts/jquery.validate*&quot;));
    
        // Use hello development version of Modernizr toodevelop with and learn from. Then, when you&#39;re
        // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
        bundles.Add(new ScriptBundle(&quot;~/bundles/modernizr&quot;).Include(
                    &quot;~/Scripts/modernizr-*&quot;));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/bootstrap&quot;).Include(
                    &quot;~/Scripts/bootstrap.js&quot;,
                    &quot;~/Scripts/respond.js&quot;));
    
        bundles.Add(new StyleBundle(&quot;~/Content/css&quot;).Include(
                    &quot;~/Content/bootstrap.css&quot;,
                    &quot;~/Content/site.css&quot;<mark>,
                    &quot;~/Content/themes/base/jquery-ui.css&quot;</mark>));
    }
    </pre>
    
    Existují další původce způsoby toomanage JavaScript a CSS souborů ve vaší aplikaci. Ale pro jednoduchost právě budete toopiggyback na hello sady, načítaných s každé zobrazení.
14. Nakonec v ~ \Global.asax, přidejte následující řádek kódu hello hello `Application_Start()` metoda. `Ctrl`+`.`na každém pojmenování chyby překladu názvů příliš opravte ji.
    
        AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.NameIdentifier;
    
    > [!NOTE]
    > Je nutné tento řádek kódu, protože používá výchozí šablony MVC hello <code>[ValidateAntiForgeryToken]</code> decoration u některých akcí hello. Z důvodu toohello chování popsaného [Allen společnosti Brock](https://twitter.com/BrockLAllen) v [MVC 4, AntiForgeryToken a deklarace identity](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) HTTP POST podařit ověření tokenu proti zfalšování, protože:
    > 
    > * Azure Active Directory neodešle hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider, který je požadován ve výchozím nastavení token proti padělání hello.
    > * Pokud Azure Active Directory directory synchronizované se službou AD FS, důvěryhodnosti hello služby AD FS ve výchozím nastavení neodesílá hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider deklarace identity buď, i když můžete nakonfigurovat ručně toosend služby AD FS Tato deklarace identity.
    > 
    > `ClaimTypes.NameIdentifies`Určuje hello deklarace `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`, která poskytnout Azure Active Directory.  
    > 
    > 
15. Nyní publikujte změny. Klikněte pravým tlačítkem na projekt a klikněte na tlačítko **publikovat**.
16. Klikněte na tlačítko **nastavení**, ověřte zda je k dispozici tooyour řetězce připojení SQL Database, vyberte **aktualizace databáze** toomake hello změny schématu pro váš model a klikněte na tlačítko **publikovat** .
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/18-publish-crud-changes.png)
17. Přejděte v prohlížeči hello toohttps: / /&lt;*appname*>.azurewebsites.net/workitems a klikněte na tlačítko **vytvořit nový**.
18. Klikněte na tlačítko v hello **AssignedToName** pole. Měli byste nyní vidět uživatele a skupiny z klienta služby Azure Active Directory v rozevíracím seznamu. Můžete zadat toofilter nebo použít hello `Up` nebo `Down` klíče nebo klikněte na tlačítko tooselect hello uživatele nebo skupinu. 
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/19-use-aadpicker.png)
19. Klikněte na tlačítko **vytvořit** toosave hello změny. Potom klikněte na **upravit** na hello vytvoření pracovní položky tooobserve hello stejné chování.

Congrats nyní je spuštěn-obchodní aplikace v Azure s přístupem k adresáři! Je mnohem víc, že můžete provést hello rozhraní Graph API. V tématu [referenční dokumentace rozhraní API Azure AD Graph](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog).

<a name="next"></a>

## <a name="next-step"></a>Další krok
Pokud potřebujete řízení přístupu na základě role (RBAC)-obchodní aplikace v azure, najdete v části [WebApp. RoleClaims DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) pro ukázku od týmu Azure Active Directory hello. Se dozvíte, jak tooenable role pro vaši aplikaci Azure Active Directory a potom budete autorizovat uživatele s hello `[Authorize]` decoration.

Pokud-obchodní aplikace potřebuje přístup k datům místní tooon, přečtěte si téma [přístup k místním prostředkům v Azure App Service pomocí hybridních připojení](web-sites-hybrid-connection-get-started.md).

<a name="bkmk_resources"></a>

## <a name="further-resources"></a>Další prostředky
* [Ověřování a autorizace ve službě Azure App Service](../app-service/app-service-authentication-overview.md)
* [Ověření pomocí místní služby Active Directory v aplikaci Azure](web-sites-authentication-authorization.md)
* [Vytvoření-obchodní aplikace v Azure pomocí ověřování služby AD FS](web-sites-dotnet-lob-application-adfs.md)
* [Aplikace služby ověřování a hello Azure AD Graph API](https://cgillum.tech/2016/03/25/app-service-auth-aad-graph-api/)
* [Microsoft Azure Active Directory ukázky a dokumentace](https://github.com/AzureADSamples)
* [Azure Active Directory podporované Token a typy deklarací identity](http://msdn.microsoft.com/library/azure/dn195587.aspx)
