---
title: "Vytvoření aplikace Azure-obchodní pomocí ověřování Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak vytvořit aplikaci ASP.NET MVC – obchodní v Azure App Service ověřování s Azure Active Directory"
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
ms.openlocfilehash: 6eadf0a521a32c5bc580908e4e4b7f4305e2bf7e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-line-of-business-azure-app-with-azure-active-directory-authentication"></a>Vytvoření aplikace Azure-obchodní pomocí ověřování Azure Active Directory
V tomto článku se dozvíte, jak vytvořit-obchodní aplikace .NET v [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) pomocí [ověřování / autorizace](../app-service/app-service-authentication-overview.md) funkce. Také ukazuje, jak používat [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) do adresáře dotaz na data v aplikaci.

Klienta Azure Active Directory, který používáte, může být adresář jen Azure. Nebo může být [synchronizaci se službou Active Directory v místě](../active-directory/active-directory-aadconnect.md) k vytvoření jedné prostředí přihlášení pro pracovní procesy, které jsou místní a vzdálené. Tento článek používá výchozí adresář pro váš účet Azure.

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a>Co se sestavení
Bude vytvořit jednoduchou aplikaci vytvořit-čtení-aktualizace-odstranění (CRUD)-obchodní v App Service Web Apps, že sleduje pracovní položky pomocí následujících funkcí:

* Ověřuje uživatele na základě Azure Active Directory
* Dotazuje adresáře uživatelů a skupin pomocí [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx)
* Pomocí rozhraní ASP.NET MVC *bez ověřování* šablony

Pokud potřebujete řízení přístupu na základě role (RBAC)-obchodní aplikace v Azure, najdete v části [další krok](#next).

<a name="bkmk_need"></a>

## <a name="what-you-need"></a>Co potřebujete
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

Budete potřebovat k dokončení tohoto kurzu:

* Klient služby Azure Active Directory s uživateli v různých skupinách
* Oprávnění k vytvoření aplikace na klienta Azure Active Directory
* Visual Studio 2013 Update 4 nebo novější
* [Azure SDK 2.8.1 nebo novější](https://azure.microsoft.com/downloads/)

<a name="bkmk_deploy"></a>

## <a name="create-and-deploy-a-web-app-to-azure"></a>Vytvoření a nasazení webové aplikace do Azure
1. Ze sady Visual Studio, klikněte na tlačítko **soubor** > **nový** > **projektu**.
2. Vyberte **webové aplikace ASP.NET**, pojmenujte svůj projekt a klikněte na tlačítko **OK**.
3. Vyberte **MVC** šablony, pak změňte ověření na **bez ověřování**. Zajistěte, aby **hostitel v cloudu** je vybrána a klikněte na tlačítko **OK**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/1-create-mvc-no-authentication.png)
4. V **vytvořit službu App Service** dialogové okno, klikněte na tlačítko **přidat účet** (a potom **přidat účet** v rozevírací nabídce) pro přihlášení k účtu Azure.
5. Po přihlášení nakonfigurujte webové aplikace. Kliknutím příslušné vytvořit skupinu prostředků a nový plán aplikační služby **nový** tlačítko. Klikněte na tlačítko **Objevte další služby Azure** pokračujte.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/2-create-app-service.png)
6. V **služby** , klikněte na  **+**  přidání databáze SQL pro vaši aplikaci. 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/3-add-sql-database.png)
7. V **nakonfigurovat databázi SQL**, klikněte na tlačítko **nový** k vytvoření instance systému SQL Server.
8. V **nakonfigurujte systém SQL Server**, nakonfigurovat instanci SQL serveru. Potom klikněte na **OK**, **OK**, a **vytvořit** k ji vytváření aplikace v Azure.
9. V **aktivita služby Azure App Service**, se zobrazí po dokončení vytváření aplikace. Klikněte na tlačítko  **publikovat &lt;* appname*> a této webové aplikace teď **, potom klikněte na **publikovat**. 
   
    Po dokončení sady Visual Studio otevře publikování aplikace v prohlížeči. 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/4-published-shown-in-browser.png)

<a name="bkmk_auth"></a>

## <a name="configure-authentication-and-directory-access"></a>Konfigurace ověřování a directory přístupu
1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. V levé nabídce klikněte na tlačítko **App Services** > **&lt;*appname*> ** > **ověřování / autorizace**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/5-app-service-authentication.png)
3. Zapnout ověřování Azure Active Directory kliknutím **na** > **Azure Active Directory** > **Express** > **OK**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/6-authentication-express.png)
4. Klikněte na tlačítko **Uložit** na panelu příkazů.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/7-authentication-save.png)
   
    Po nastavení ověřování se úspěšně uložily, zkuste přejdete do vaší aplikace znovu v prohlížeči. Výchozí nastavení vynucení ověřování na celou aplikaci. Pokud už nejste přihlášeni, budete přesměrováni na přihlašovací obrazovku. Po přihlášení, by se zobrazit aplikaci zabezpečené pomocí protokolu HTTPS. Dále musíte povolit přístup k datům adresáře. 
5. Přejděte na [portálu classic](https://manage.windowsazure.com).
6. V levé nabídce klikněte na tlačítko **služby Active Directory** > **výchozí adresář** > **aplikace** > **&lt;*appname*> **.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/8-find-aad-application.png)
   
    Toto je aplikace Azure Active Directory, která vytvoří služby App Service Povolit autorizaci nebo funkce ověřování.
7. Klikněte na tlačítko **uživatelé** a **skupiny** a ujistěte se, že máte některé uživatele a skupiny v adresáři. Pokud ne, vytvořte několik testovací uživatele a skupiny.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/9-create-users-groups.png)
8. Klikněte na tlačítko **konfigurace** ke konfiguraci této aplikace.
9. Přejděte dolů k položce **klíče** a přidejte klíč výběrem dobu trvání. Potom klikněte na **delegovaná oprávnění** a vyberte **čtení dat adresáře**. 
   Klikněte na **Uložit**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/10-configure-aad-application.png)
10. Jakmile vaše nastavení se ukládají, přejděte zpět na **klíče** části a klikněte na tlačítko **kopie** tlačítko zkopírujte klíč klienta. 
    
     ![](./media/web-sites-dotnet-lob-application-azure-ad/11-get-app-key.png)
    
    > [!IMPORTANT]
    > Pokud jste tuto stránku opustit nyní, nebudete mít přístup k tento klíč klienta někdy znovu.
    > 
    > 
11. Dále musíte nakonfigurovat webové aplikace s tímto klíčem. Přihlaste se k [Průzkumníka prostředků Azure](https://resources.azure.com) s vaším účtem Azure.
12. V horní části stránky klikněte na tlačítko **pro čtení a zápis** provést změny v Průzkumníku prostředků Azure.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/12-resource-manager-writable.png)
13. Najít nastavení ověřování pro aplikace umístěné v odběry >  **&lt;* název_předplatného*> ** > **Skupinyprostředků** > **&lt;*resourcegroupname*> ** > **zprostředkovatelé** > **Microsoft.Web** > **lokality** > **&lt;*appname*> ** > **konfigurace** > **authsettings**.
14. Klikněte na **Upravit**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/13-edit-authsettings.png)
15. V podokně úpravy nastavit `clientSecret` a `additionalLoginParams` vlastnosti následujícím způsobem.
    
        ...
        "clientSecret": "<client key from the Azure Active Directory application>",
        ...
        "additionalLoginParams": ["response_type=code id_token", "resource=https://graph.windows.net"],
        ...
16. Klikněte na tlačítko **Put** v horní části k odeslání změn.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/14-edit-parameters.png)
17. Nyní Pokud chcete otestovat, pokud máte autorizační token pro přístup k Azure Active Directory Graph API, jednoduše přejděte k  **https://&lt;*appname*>.azurewebsites.net/.auth/me** v prohlížeči. Pokud jste nakonfigurovali všechno správně, měli byste vidět `access_token` vlastnost v odpovědi JSON.
    
    `~/.auth/me` Cestu adresy URL spravuje aplikace služby ověřování / autorizace tak, abyste získali všechny informace související s ověřená relace. Další informace najdete v tématu [ověřování a autorizace ve službě Azure App Service](../app-service/app-service-authentication-overview.md).
    
    > [!NOTE]
    > `access_token` Má dobu platnosti. Ale aplikace služby ověřování / autorizace poskytuje funkce tokenu aktualizace s `~/.auth/refresh`. Další informace o tom, jak ho použít, najdete v části [obchod Token služby](https://cgillum.tech/2016/03/07/app-service-token-store/).
    > 
    > 

V dalším kroku provedete něco užitečný v případě dat adresáře.

<a name="bkmk_crud"></a>

## <a name="add-line-of-business-functionality-to-your-app"></a>Přidat další funkce – obchodní aplikace
Nyní můžete vytvořit jednoduché sledovací modul CRUD položky pracovní.  

1. Ve složce ~\Models, vytvořte soubor třídy s názvem WorkItem.cs a nahraďte `public class WorkItem {...}` následujícím kódem:
   
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
2. Sestavte projekt a zpřístupněte nový model pro logiku generování uživatelského rozhraní v sadě Visual Studio.
3. Přidat novou položku vygenerované `WorkItemsController` ke složce ~\Controllers (klikněte pravým tlačítkem na **řadiče**, přejděte na příkaz **přidat**a vyberte **nové vygenerované položky**). 
4. Vyberte **kontroler MVC 5 se zobrazeními s využitím nástroje Entity Framework** a klikněte na tlačítko **přidat**.
5. Vyberte model, který jste vytvořili a pak klikněte na tlačítko  **+**  a potom **přidat** přidat data kontextu, a potom klikněte na **přidat**.
   
   ![](./media/web-sites-dotnet-lob-application-azure-ad/16-add-scaffolded-controller.png)
6. V ~\Views\WorkItems\Create.cshtml (automaticky vygenerované položky), najdete `Html.BeginForm` Pomocná metoda a proveďte následující zvýrazněný změny:  
   
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
    @Html.ActionLink(&quot;Back to List&quot;, &quot;Index&quot;)
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
   
        // Submit the selected user/group to be asssigned.
        $(&quot;#submit-button&quot;).click({ picker: picker }, function () {
            if (!picker.Selected())
                return;
            $(&quot;#main-form&quot;).get()[0].elements[&quot;AssignedToID&quot;].value = picker.Selected().objectId;
        });
    &lt;/script&gt;</mark>
   }
   </pre>
   
   Všimněte si, že `token` a `tenant` jsou používány `AadPicker` objekt volání Azure Active Directory Graph API. Přidáte `AadPicker` později.     
   
   > [!NOTE]
   > Stejně dobře získáte `token` a `tenant` ze strany klienta s `~/.auth/me`, ale, že by být volání další server. Například:
   > 
   > $.ajax ({datový typ: "json", adresa url: "/.auth/me", Úspěch: funkce (data) {var token = data [0] .access_token; var klienta = dat [0] .user_claims .find (c = > c.typ === 'http://schemas.microsoft.com/identity/claims/tenantid') .val;}});
   > 
   > 
7. Provést stejné změny s ~ \Views\WorkItems\Edit.cshtml.
8. `AadPicker` Objektu je definována v skript, který je nutné přidat do projektu. Klikněte pravým tlačítkem na složku ~\Scripts, přejděte na **přidat**a klikněte na tlačítko **soubor JavaScript**. Typ `AadPickerLibrary` pro název souboru a klikněte na tlačítko **OK**.
9. Kopírovat obsah z [sem](https://raw.githubusercontent.com/cephalin/active-directory-dotnet-webapp-roleclaims/master/WebApp-RoleClaims-DotNet/Scripts/AadPickerLibrary.js) do ~ \Scripts\AadPickerLibrary.js.
   
   Ve skriptu `AadPicker` objektu volání [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) pro vyhledání uživatelů a skupin, které odpovídají vstupní.  
10. ~\Scripts\AadPickerLibrary.js používá také [pomůcky Autocomplete uživatelského rozhraní jQuery](https://jqueryui.com/autocomplete/). Proto je nutné přidat do projektu jQuery uživatelského rozhraní. Klikněte pravým tlačítkem na projekt v a klikněte na tlačítko **spravovat balíčky NuGet**.
11. V Správce balíčků NuGet, klikněte na tlačítko Procházet, typ **uživatelského rozhraní jquery** v panelu vyhledávání a klikněte na **jQuery.UI.Combined**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/17-add-jquery-ui-nuget.png)
12. V pravém podokně klikněte na **nainstalovat**, pak klikněte na tlačítko **OK** pokračovat.
13. Otevřete ~\App_Start\BundleConfig.cs a proveďte následující zvýrazněný změny:  
    
    <pre class="prettyprint">
    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle(&quot;~/bundles/jquery&quot;).Include(
                    &quot;~/Scripts/jquery-{version}.js&quot;<mark>,
                    &quot;~/Scripts/jquery-ui-{version}.js&quot;,
                    &quot;~/Scripts/AadPickerLibrary.js&quot;</mark>));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/jqueryval&quot;).Include(
                    &quot;~/Scripts/jquery.validate*&quot;));
    
        // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
        // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
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
    
    Existuje více způsobů původce ke správě souborů JavaScript a CSS ve vaší aplikaci. Ale pro jednoduchost právě budete počítač na sady načítaných s každé zobrazení.
14. Nakonec v ~ \Global.asax, přidejte následující řádek kódu `Application_Start()` metoda. `Ctrl`+`.`na každém pojmenování chyby překladu názvů a opravte ji.
    
        AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.NameIdentifier;
    
    > [!NOTE]
    > Je nutné tento řádek kódu, protože používá výchozí šablony MVC <code>[ValidateAntiForgeryToken]</code> decoration na některé akce. Z důvodu chování popsaného [Allen společnosti Brock](https://twitter.com/BrockLAllen) v [MVC 4, AntiForgeryToken a deklarace identity](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) HTTP POST podařit ověření tokenu proti zfalšování, protože:
    > 
    > * Azure Active Directory neodešle http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider, který je požadován ve výchozím nastavení token proti padělání.
    > * Pokud Azure Active Directory directory synchronizované se službou AD FS, vztah důvěryhodnosti služby AD FS ve výchozím nastavení neodesílá http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider deklarace identity buď, i když můžete ručně nakonfigurovat na odesílání této deklarace identity služby AD FS.
    > 
    > `ClaimTypes.NameIdentifies`Určuje deklarace `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`, která poskytnout Azure Active Directory.  
    > 
    > 
15. Nyní publikujte změny. Klikněte pravým tlačítkem na projekt a klikněte na tlačítko **publikovat**.
16. Klikněte na tlačítko **nastavení**, ujistěte se, že je připojovací řetězec k vaší databázi SQL, vyberte **aktualizace databáze** provést změny schématu pro váš model a klikněte na tlačítko **publikovat**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/18-publish-crud-changes.png)
17. V prohlížeči přejděte na https://&lt;*appname*>.azurewebsites.net/workitems a klikněte na tlačítko **vytvořit nový**.
18. Kliknutím na tlačítko ve **AssignedToName** pole. Měli byste nyní vidět uživatele a skupiny z klienta služby Azure Active Directory v rozevíracím seznamu. Můžete zadat pro filtrování, nebo pomocí `Up` nebo `Down` klíče nebo klikněte na tlačítko Vybrat uživatele nebo skupinu. 
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/19-use-aadpicker.png)
19. Klikněte na tlačítko **vytvořit** a uložte změny. Potom klikněte na **upravit** stejné chování zachovávají vytvořený pracovní položku.

Congrats nyní je spuštěn-obchodní aplikace v Azure s přístupem k adresáři! Je mnohem víc, že můžete provést pomocí rozhraní Graph API. V tématu [referenční dokumentace rozhraní API Azure AD Graph](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog).

<a name="next"></a>

## <a name="next-step"></a>Další krok
Pokud potřebujete řízení přístupu na základě role (RBAC)-obchodní aplikace v azure, najdete v části [WebApp. RoleClaims DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) pro ukázku od týmu Azure Active Directory. Ukazuje, jak povolit role pro vaše aplikace Azure Active Directory, a potom budete autorizovat uživatele s `[Authorize]` decoration.

Pokud vaše-obchodní aplikace potřebuje přístup k místním datům, přečtěte si téma [přístup k místním prostředkům v Azure App Service pomocí hybridních připojení](web-sites-hybrid-connection-get-started.md).

<a name="bkmk_resources"></a>

## <a name="further-resources"></a>Další prostředky
* [Ověřování a autorizace ve službě Azure App Service](../app-service/app-service-authentication-overview.md)
* [Ověření pomocí místní služby Active Directory v aplikaci Azure](web-sites-authentication-authorization.md)
* [Vytvoření-obchodní aplikace v Azure pomocí ověřování služby AD FS](web-sites-dotnet-lob-application-adfs.md)
* [Ověřování služby App Service a Azure AD Graph API](https://cgillum.tech/2016/03/25/app-service-auth-aad-graph-api/)
* [Microsoft Azure Active Directory ukázky a dokumentace](https://github.com/AzureADSamples)
* [Azure Active Directory podporované Token a typy deklarací identity](http://msdn.microsoft.com/library/azure/dn195587.aspx)
