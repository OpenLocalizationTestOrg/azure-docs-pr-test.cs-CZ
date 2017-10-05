---
title: "Vytvořit rozhraní REST API v Azure pomocí technologie ASP.NET a SQL DB | Microsoft Docs"
description: "Kurz, který se naučíte, jak nasadit aplikaci, která používá rozhraní ASP.NET Web API pro webové aplikace Azure pomocí sady Visual Studio."
services: app-service\web
documentationcenter: .net
author: Rick-Anderson
writer: Rick-Anderson
manager: erikre
editor: 
ms.assetid: f4916fc0-ea08-41f7-846b-73e41bc88149
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/29/2016
ms.author: riande
ms.openlocfilehash: 64c18f2cfabbb7af6ffd89b4c2a9095fca1cf799
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-rest-service-using-aspnet-web-api-and-sql-database-in-azure-app-service"></a>Vytvoření služby pomocí rozhraní ASP.NET Web API a databázi SQL v Azure App Service
Tento kurz ukazuje, jak nasadit webové aplikace ASP.NET do [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) pomocí Průvodce Publikovat Web v sadě Visual Studio 2013 nebo Visual Studio 2013 Community Edition. 

Můžete otevřít účet Azure zdarma a pokud ještě nemáte Visual Studio 2013, sady SDK automaticky nainstaluje Visual Studio 2013 pro produkt Web Express. Proto můžete spustit vývoj pro Azure zcela zdarma.

Tento kurz předpokládá, že máte žádné předchozí zkušenosti s používáním Azure. Po dokončení tohoto kurzu, budete mít jednoduché webové aplikace nahoru a běží v cloudu.

Naučíte se:

* Postup zprovoznění počítače pro vývoj na platformě Azure nainstalováním sady Azure SDK.
* Postup vytvoření projektu Visual Studio ASP.NET MVC 5 a publikujete ho v aplikaci Azure.
* Jak používat rozhraní ASP.NET Web API umožňující volání rozhraní Restful API.
* Jak používat databázi SQL pro ukládání dat v Azure.
* Jak publikovat aplikaci aktualizací do Azure.

Budete vytvářet jednoduché seznamu kontaktů webovou aplikaci, která je založená na technologii ASP.NET MVC 5 a používá ADO.NET Entity Framework pro přístup k databázi. Na následujícím obrázku je vidět hotová aplikace:

![snímek obrazovky webové stránky][intro001]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

### <a name="create-the-project"></a>Vytvoření projektu
1. Spusťte Visual Studio 2013.
2. Z **soubor** nabídce klikněte na tlačítko **nový projekt**.
3. V **nový projekt** dialogové okno, rozbalte seznam **Visual C#** a vyberte **webové** a pak vyberte **webové aplikace ASP.NET**. Název aplikace **ContactManager** a klikněte na tlačítko **OK**.
   
    ![Dialogové okno Nový projekt](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr4.png)
4. V **nový projekt ASP.NET** dialogové okno, vyberte **MVC** šablony, zkontrolujte **webového rozhraní API** a pak klikněte na **změna ověřování**.
5. V dialogovém okně **Změna ověřování** klikněte na možnost **Bez ověřování** a poté klikněte na tlačítko **OK**.
   
    ![Bez ověřování](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/GS13noauth.png)
   
    Ukázkovou aplikaci, kterou vytváříte, nebude mít funkcí, které vyžadují přihlášení uživatelů. Informace o tom, jak implementovat ověřování a autorizace funkce najdete v tématu [další kroky](#nextsteps) na konci tohoto kurzu. 
6. V **nový projekt ASP.NET** dialogové okno zkontrolujte, zda **hostitel v cloudu** je zaškrtnuté políčko a klikněte na tlačítko **OK**.

Pokud nejste přihlášeni dříve do Azure, budete vyzváni k přihlášení.

1. Průvodce konfigurací navrhne jedinečný název založený na *ContactManager* (viz následující obrázek). Vyberte oblast okolo vás. Můžete použít [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") najít datovém centru nejnižší latenci. 
2. Pokud jste dosud nevytvořili databázový server před, vyberte **vytvořit nový server**, zadejte jméno uživatele databáze a heslo.
   
    ![Konfigurace webu Azure](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configAz.PNG)

Pokud máte databázový server, použijte k vytvoření nové databáze. Databázové servery jsou drahocenný prostředků a chcete obecně vytvořit více databází na stejném serveru pro testování a vývoj, nikoli databázový server na databázi. Ujistěte se, že webový server a databáze jsou ve stejné oblasti.

![Konfigurace webu Azure](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configWithDB.PNG)

### <a name="set-the-page-header-and-footer"></a>Nastavit záhlaví a zápatí stránky
1. V **Průzkumníku řešení**, rozbalte *Views\Shared* složky a otevřete *_Layout.cshtml* souboru.
   
    ![_Layout.cshtml v Průzkumníku řešení][newapp004]
2. Nahraďte obsah *Views\Shared_Layout.cshtml* soubor s následujícím kódem:

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="utf-8" />
            <title>@ViewBag.Title - Contact Manager</title>
            <link href="~/favicon.ico" rel="shortcut icon" type="image/x-icon" />
            <meta name="viewport" content="width=device-width" />
            @Styles.Render("~/Content/css")
            @Scripts.Render("~/bundles/modernizr")
        </head>
        <body>
            <header>
                <div class="content-wrapper">
                    <div class="float-left">
                        <p class="site-title">@Html.ActionLink("Contact Manager", "Index", "Home")</p>
                    </div>
                </div>
            </header>
            <div id="body">
                @RenderSection("featured", required: false)
                <section class="content-wrapper main-content clear-fix">
                    @RenderBody()
                </section>
            </div>
            <footer>
                <div class="content-wrapper">
                    <div class="float-left">
                        <p>&copy; @DateTime.Now.Year - Contact Manager</p>
                    </div>
                </div>
            </footer>
            @Scripts.Render("~/bundles/jquery")
            @RenderSection("scripts", required: false)
        </body>
        </html>

Výše uvedený kód změní název aplikace z "Moje aplikace technologie ASP.NET" na "Obraťte se na správce" a odebere odkazy na **Domů**, **o** a **kontaktujte**.

### <a name="run-the-application-locally"></a>Místní spuštění aplikace
1. Stiskněte klávesy CTRL+F5 a spusťte aplikaci.
   Domovská stránka aplikace se zobrazí výchozí prohlížeč.
    ![Seznam úkolů domovskou stránku](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)

To je vše, je potřeba udělat teď k vytvoření aplikace, které nasadíte do Azure. Později přidáte funkce databáze.

## <a name="deploy-the-application-to-azure"></a>Nasazení aplikace v Azure
1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **publikovat** v místní nabídce.
   
    ![Publikování v kontextové nabídce projektu][PublishVSSolution]
   
    **Publikovat Web** otevře se průvodce.
2. Klikněte na **Publikovat**.

![Karta nastavení](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/pw.png)

Visual Studio spustí proces kopírování souborů na Azure server. **Výstup** okno zobrazuje, jaké akce nasazení byly provedeny a hlásí úspěšné dokončení nasazení.

1. Na adresu URL nasazené lokality se automaticky otevře výchozí prohlížeč.
   
   Aplikace, kterou jste vytvořili je nyní spuštěna v cloudu.
   
   ![Seznam úkolů domovskou stránku běžící v Azure][rxz2]

## <a name="add-a-database-to-the-application"></a>Přidání databáze do aplikace
Dále budete aktualizovat aplikaci MVC přidáte možnost zobrazit a aktualizovat kontakty a uložení dat v databázi. Aplikace bude používat rozhraní Entity Framework k vytvoření databáze a k číst a aktualizovat data v databázi.

### <a name="add-data-model-classes-for-the-contacts"></a>Přidání třídy modelu dat pro kontaktů
Začněte vytvořením jednoduchého datového modelu v kódu.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na složku modely, klikněte na **přidat**a potom **třída**.
   
    ![Přidání třídy v kontextové nabídce složku modely][adddb001]
2. V **přidat novou položku** dialogové okno, název nového souboru třídy *Contact.cs*a potom klikněte na **přidat**.
   
    ![Přidat novou položku – dialogové okno][adddb002]
3. Nahraďte obsah souboru Contacts.cs následujícím kódem.
   
        using System.Globalization;
        namespace ContactManager.Models
        {
            public class Contact
               {
                public int ContactId { get; set; }
                public string Name { get; set; }
                public string Address { get; set; }
                public string City { get; set; }
                public string State { get; set; }
                public string Zip { get; set; }
                public string Email { get; set; }
                public string Twitter { get; set; }
                public string Self
                {
                    get { return string.Format(CultureInfo.CurrentCulture,
                         "api/contacts/{0}", this.ContactId); }
                    set { }
                }
            }
        }

**Obraťte se na** třídy definují data, která se uloží pro každý kontakt a primární klíč, KódKontaktu, který je nutný pro databázi. Můžete získat další informace o datových modelech v [další kroky](#nextsteps) na konci tohoto kurzu.

### <a name="create-web-pages-that-enable-app-users-to-work-with-the-contacts"></a>Vytvoření webové stránky, které umožňují uživatelům aplikace pro práci s kontaktů
ASP.NET MVC funkci generování uživatelského rozhraní můžete automaticky generovat kód, který provádí vytvářet, číst, aktualizovat a odstraňovat akcemi (CRUD).

## <a name="add-a-controller-and-a-view-for-the-data"></a>Přidání Kontroleru a zobrazení dat
1. V **Průzkumníku**, rozbalte složku řadiče.
2. Sestavení projektu **(Ctrl + Shift + B)**. (Před použitím mechanismus generování uživatelského rozhraní musí sestavte projekt.) 
3. Klikněte pravým tlačítkem na složku řadiče a klikněte na tlačítko **přidat**a potom klikněte na **řadič**.
   
    ![Přidat řadič v kontextové nabídce řadiče složky][addcode001]
4. V **přidat vygenerované uživatelské rozhraní** dialogové okno, vyberte **kontroler MVC se zobrazeními s využitím nástroje Entity Framework** a klikněte na tlačítko **přidat**.
   
   ![Přidání kontroleru](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rrAC.png)
5. Nastavte název řadiče na **HomeController**. Vyberte **kontaktujte** jako třídu modelu. Klikněte **nový kontext dat** tlačítko a přijměte výchozí nastavení "ContactManager.Models.ContactManagerContext" pro **nový typ kontextu dat**. Klikněte na tlačítko **Přidat**.

    Dialogové okno zobrazí výzvu: "soubor s názvem HomeController již ukončí. Chcete ho nahradit? ". Klikněte na **Ano**. Přepisování jsme řadič Domů, který byl vytvořen nový projekt. Nový řadič Domů budeme používat pro naše seznamu kontaktů.

    Visual Studio vytvoří metody kontroleru a zobrazení pro operace CRUD databáze pro **kontaktujte** objekty.

## <a name="enable-migrations-create-the-database-add-sample-data-and-a-data-initializer"></a>Povolit migrace a vytvořit databázi, Přidání ukázkových dat a inicializátoru dat
Dalším krokem je povolit [migrace Code First](http://curah.microsoft.com/55220) funkci pro vytvoření databáze založené na datový model, který jste vytvořili.

1. V **nástroje** nabídce vyberte možnost **Správce balíčků knihoven** a potom **Konzola správce balíčků**.
   
    ![Konzola správce balíčků v nabídce Nástroje][addcode008]
2. V **Konzola správce balíčků** okno, zadejte následující příkaz:
   
        enable-migrations 
   
    **Enable-migrations se** příkaz vytvoří *migrace* složku a její uloží je v této složce *Configuration.cs* soubor, který můžete upravit konfigurace migrací. 
3. V **Konzola správce balíčků** okno, zadejte následující příkaz:
   
        add-migration Initial
   
    **Přidat migrace počáteční** příkaz vygeneruje třídy s názvem  **&lt;date_stamp&gt;počáteční** vytvářející databáze. První parametr ( *počáteční* ) je libovolný a slouží k vytvoření názvu souboru. Zobrazí se nové soubory tříd v **Průzkumníku řešení**.
   
    V **počáteční** třídy, **až** metoda vytvoří tabulku kontaktů a **dolů** – metoda (používá, pokud chcete vrátit do předchozího stavu) se zahodí.
4. Otevřete *Migrations\Configuration.cs* souboru. 
5. Přidejte následující obory názvů. 
   
         using ContactManager.Models;
6. Nahraďte *počáteční hodnoty* metoda následujícím kódem:
   
        protected override void Seed(ContactManager.Models.ContactManagerContext context)
        {
            context.Contacts.AddOrUpdate(p => p.Name,
               new Contact
               {
                   Name = "Debra Garcia",
                   Address = "1234 Main St",
                   City = "Redmond",
                   State = "WA",
                   Zip = "10999",
                   Email = "debra@example.com",
                   Twitter = "debra_example"
               },
                new Contact
                {
                    Name = "Thorsten Weinrich",
                    Address = "5678 1st Ave W",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "thorsten@example.com",
                    Twitter = "thorsten_example"
                },
                new Contact
                {
                    Name = "Yuhong Li",
                    Address = "9012 State st",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "yuhong@example.com",
                    Twitter = "yuhong_example"
                },
                new Contact
                {
                    Name = "Jon Orton",
                    Address = "3456 Maple St",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "jon@example.com",
                    Twitter = "jon_example"
                },
                new Contact
                {
                    Name = "Diliana Alexieva-Bosseva",
                    Address = "7890 2nd Ave E",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "diliana@example.com",
                    Twitter = "diliana_example"
                }
                );
        }
   
    Tento kód výše inicializuje databázi s kontaktní informace. Další informace o synchronizace replik indexů databáze najdete v tématu [ladění Entity Framework (EF) databází](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).
7. V **Konzola správce balíčků** zadejte příkaz:
   
        update-database
   
    ![Konzola správce balíčků příkazy][addcode009]
   
    **Update-database** spustí první migrace, která vytvoří databázi. Databáze je ve výchozím nastavení vytvoří jako databáze SQL serveru Express LocalDB.
8. Stiskněte klávesy CTRL+F5 a spusťte aplikaci. 

Aplikace zobrazuje počáteční hodnoty data a poskytuje odkazy upravit, podrobnosti a odstranění.

![Zobrazení MVC dat][rxz3]

## <a name="edit-the-view"></a>Upravit zobrazení
1. Otevřete *Views\Home\Index.cshtml* souboru. V dalším kroku, jsme nahradí generovaný kód s kódem, který používá [jQuery](http://jquery.com/) a [Knockout.js](http://knockoutjs.com/). Tento nový kód načte seznam kontaktů pomocí webového rozhraní API a JSON a pak uživatelského rozhraní pomocí knockout.js sváže kontaktní údaje. Další informace najdete v tématu [další kroky](#nextsteps) na konci tohoto kurzu. 
2. Obsah souboru nahraďte následujícím kódem.
   
        @model IEnumerable<ContactManager.Models.Contact>
        @{
            ViewBag.Title = "Home";
        }
        @section Scripts {
            @Scripts.Render("~/bundles/knockout")
            <script type="text/javascript">
                function ContactsViewModel() {
                    var self = this;
                    self.contacts = ko.observableArray([]);
                    self.addContact = function () {
                        $.post("api/contacts",
                            $("#addContact").serialize(),
                            function (value) {
                                self.contacts.push(value);
                            },
                            "json");
                    }
                    self.removeContact = function (contact) {
                        $.ajax({
                            type: "DELETE",
                            url: contact.Self,
                            success: function () {
                                self.contacts.remove(contact);
                            }
                        });
                    }
   
                    $.getJSON("api/contacts", function (data) {
                        self.contacts(data);
                    });
                }
                ko.applyBindings(new ContactsViewModel());    
        </script>
        }
        <ul id="contacts" data-bind="foreach: contacts">
            <li class="ui-widget-content ui-corner-all">
                <h1 data-bind="text: Name" class="ui-widget-header"></h1>
                <div><span data-bind="text: $data.Address || 'Address?'"></span></div>
                <div>
                    <span data-bind="text: $data.City || 'City?'"></span>,
                    <span data-bind="text: $data.State || 'State?'"></span>
                    <span data-bind="text: $data.Zip || 'Zip?'"></span>
                </div>
                <div data-bind="if: $data.Email"><a data-bind="attr: { href: 'mailto:' + Email }, text: Email"></a></div>
                <div data-bind="ifnot: $data.Email"><span>Email?</span></div>
                <div data-bind="if: $data.Twitter"><a data-bind="attr: { href: 'http://twitter.com/' + Twitter }, text: '@@' + Twitter"></a></div>
                <div data-bind="ifnot: $data.Twitter"><span>Twitter?</span></div>
                <p><a data-bind="attr: { href: Self }, click: $root.removeContact" class="removeContact ui-state-default ui-corner-all">Remove</a></p>
            </li>
        </ul>
        <form id="addContact" data-bind="submit: addContact">
            <fieldset>
                <legend>Add New Contact</legend>
                <ol>
                    <li>
                        <label for="Name">Name</label>
                        <input type="text" name="Name" />
                    </li>
                    <li>
                        <label for="Address">Address</label>
                        <input type="text" name="Address" >
                    </li>
                    <li>
                        <label for="City">City</label>
                        <input type="text" name="City" />
                    </li>
                    <li>
                        <label for="State">State</label>
                        <input type="text" name="State" />
                    </li>
                    <li>
                        <label for="Zip">Zip</label>
                        <input type="text" name="Zip" />
                    </li>
                    <li>
                        <label for="Email">E-mail</label>
                        <input type="text" name="Email" />
                    </li>
                    <li>
                        <label for="Twitter">Twitter</label>
                        <input type="text" name="Twitter" />
                    </li>
                </ol>
                <input type="submit" value="Add" />
            </fieldset>
        </form>
3. Klikněte pravým tlačítkem na složku obsahu a klikněte na tlačítko **přidat**a pak klikněte na tlačítko **novou položku...** .
   
    ![Přidání šablony stylů v kontextové nabídce složky obsahu][addcode005]
4. V **přidat novou položku** dialogovém okně zadejte **styl** v horní pravé pole pro vyhledávání a pak vyberte **list stylu**.
    ![Přidat novou položku – dialogové okno][rxStyle]
5. Název souboru *Contacts.css* a klikněte na tlačítko **přidat**. Obsah souboru nahraďte následujícím kódem.
   
        .column {
            float: left;
            width: 50%;
            padding: 0;
            margin: 5px 0;
        }
        form ol {
            list-style-type: none;
            padding: 0;
            margin: 0;
        }
        form li {
            padding: 1px;
            margin: 3px;
        }
        form input[type="text"] {
            width: 100%;
        }
        #addContact {
            width: 300px;
            float: left;
            width:30%;
        }
        #contacts {
            list-style-type: none;
            margin: 0;
            padding: 0;
            float:left;
            width: 70%;
        }
        #contacts li {
            margin: 3px 3px 3px 0;
            padding: 1px;
            float: left;
            width: 300px;
            text-align: center;
            background-image: none;
            background-color: #F5F5F5;
        }
        #contacts li h1
        {
            padding: 0;
            margin: 0;
            background-image: none;
            background-color: Orange;
            color: White;
            font-family: Trebuchet MS, Tahoma, Verdana, Arial, sans-serif;
        }
        .removeContact, .viewImage
        {
            padding: 3px;
            text-decoration: none;
        }
   
    Budeme používat tuto šablonu stylů pro rozložení, barvy a styly využívané v aplikaci kontaktujte správce.
6. Otevřete *App_Start\BundleConfig.cs* souboru.
7. Přidejte následující kód k registraci [Knockout](http://knockoutjs.com/index.html "KO") modulu plug-in.
   
        bundles.Add(new ScriptBundle("~/bundles/knockout").Include(
                    "~/Scripts/knockout-{version}.js"));
    Tato ukázka pomocí knockout zjednodušit dynamické kódu jazyka JavaScript, která zpracovává šablony obrazovky.
8. Upravit obsah nebo šablon stylů css položka registrace *contacts.css* šabloně stylů. Změňte následující řádek:
   
                 bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/site.css"));
   na:
   
        bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/contacts.css",
                   "~/Content/site.css"));
9. V konzole Správce balíčků, spusťte následující příkaz k instalaci Knockout.
   
        Install-Package knockoutjs

## <a name="add-a-controller-for-the-web-api-restful-interface"></a>Přidat řadič pro rozhraní Restful webové rozhraní API
1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na řadiče a klikněte na **přidat** a potom **řadiče...** 
2. V **přidat vygenerované uživatelské rozhraní** dialogovém okně zadejte **webové 2 kontroler API s akcemi používající rozhraní Entity Framework** a pak klikněte na **přidat**.
   
    ![Přidat kontroler API](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt1.png)
3. V **přidat kontroler** dialogovém okně zadejte "ContactsController" jako název řadiče. Vyberte "Kontakt (ContactManager.Models)" pro **třída modelu**.  Ponechte výchozí hodnotu pro **třída kontextu dat**. 
4. Klikněte na tlačítko **Přidat**.

### <a name="run-the-application-locally"></a>Místní spuštění aplikace
1. Stiskněte klávesy CTRL+F5 a spusťte aplikaci.
   
    ![Indexová stránka][intro001]
2. Zadejte kontakt a klikněte na **přidat**. Aplikace vrací na domovskou stránku a zobrazí kontaktu, které jste zadali.
   
    ![Index stránky s položkami seznamu úkolů][addwebapi004]
3. V prohlížeči připojit **/api/contacts** na adresu URL.
   
    Výsledná adresa URL bude vypadat http://localhost:1234/api/contacts. RESTful jste přidali webové rozhraní API vrátí uložené kontakty. Firefox) a Chrome (se zobrazí data ve formátu XML.
   
    ![Index stránky s položkami seznamu úkolů][rxFFchrome]

    IE vás vyzve k otevření nebo uložení kontaktů.

    ![Dialogové okno Uložit webového rozhraní API][addwebapi006]


    Vrácený kontakty můžete otevřít v programu Poznámkový blok nebo prohlížeče.

    Tento výstup mohou být spotřebovávána jiná aplikace, jako je například mobilní webové stránky nebo aplikace.

    ![Dialogové okno Uložit webového rozhraní API][addwebapi007]

    **Upozornění zabezpečení**: V tomto okamžiku aplikace je nezabezpečené a odolné vůči útoku proti útokům CSRF. Později v tomto kurzu jsme se odebrat toto ohrožení zabezpečení. Další informace najdete v části [útoky brání webů požadavku padělání (proti útokům CSRF)][prevent-csrf-attacks].
## <a name="add-xsrf-protection"></a>Přidat ochranu XSRF
Padělání požadavku posílaného mezi weby (také označované jako XSRF nebo proti útokům CSRF) je útok na hostované webové aplikace, které škodlivou webovou stránku můžete ovlivnit interakce mezi prohlížeče klienta a web důvěryhodný tento prohlížeč. Tyto útoky jsou možné, protože webových prohlížečů bude odesílat tokeny ověřování automaticky s každou žádost na web. V kanonickém tvaru příkladu je soubor cookie ověřování, jako je například ASP. Lístek ověřování pomocí formulářů pro Asp.net. Tyto útoky však může být cílem weby, které použít žádné trvalé ověřovací mechanismus (například ověřování systému Windows, Basic a tak dále).

Útok XSRF se liší od útoky phishing. Útoky phishing vyžadovat interakci z napadeného. V rámci útoku phishing škodlivou webovou stránku bude napodobovat cílového webu a napadené je oklamat do poskytování útočník citlivé informace. Při útoku XSRF není často žádná interakce potřebné z napadeného. Místo toho je útočník spoléhat na prohlížeči všechny relevantní soubory cookie automaticky odesílání do cílového webu.

Další informace najdete v tématu [otevřete projekt webové aplikace zabezpečení](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).

1. V **Průzkumníku řešení**, vpravo **ContactManager** projektu a klikněte na tlačítko **přidat** a pak klikněte na **třída**.
2. Název souboru *ValidateHttpAntiForgeryTokenAttribute.cs* a přidejte následující kód:
   
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Web.Helpers;
        using System.Web.Http.Controllers;
        using System.Web.Http.Filters;
        using System.Web.Mvc;
        namespace ContactManager.Filters
        {
            public class ValidateHttpAntiForgeryTokenAttribute : AuthorizationFilterAttribute
            {
                public override void OnAuthorization(HttpActionContext actionContext)
                {
                    HttpRequestMessage request = actionContext.ControllerContext.Request;
                    try
                    {
                        if (IsAjaxRequest(request))
                        {
                            ValidateRequestHeader(request);
                        }
                        else
                        {
                            AntiForgery.Validate();
                        }
                    }
                    catch (HttpAntiForgeryException e)
                    {
                        actionContext.Response = request.CreateErrorResponse(HttpStatusCode.Forbidden, e);
                    }
                }
                private bool IsAjaxRequest(HttpRequestMessage request)
                {
                    IEnumerable<string> xRequestedWithHeaders;
                    if (request.Headers.TryGetValues("X-Requested-With", out xRequestedWithHeaders))
                    {
                        string headerValue = xRequestedWithHeaders.FirstOrDefault();
                        if (!String.IsNullOrEmpty(headerValue))
                        {
                            return String.Equals(headerValue, "XMLHttpRequest", StringComparison.OrdinalIgnoreCase);
                        }
                    }
                    return false;
                }
                private void ValidateRequestHeader(HttpRequestMessage request)
                {
                    string cookieToken = String.Empty;
                    string formToken = String.Empty;
                    IEnumerable<string> tokenHeaders;
                    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
                    {
                        string tokenValue = tokenHeaders.FirstOrDefault();
                        if (!String.IsNullOrEmpty(tokenValue))
                        {
                            string[] tokens = tokenValue.Split(':');
                            if (tokens.Length == 2)
                            {
                                cookieToken = tokens[0].Trim();
                                formToken = tokens[1].Trim();
                            }
                        }
                    }
                    AntiForgery.Validate(cookieToken, formToken);
                }
            }
        }
3. Přidejte následující *pomocí* příkaz kontrakty řadiče, abyste získali přístup k **[ValidateHttpAntiForgeryToken]** atribut.
   
        using ContactManager.Filters;
4. Přidat **[ValidateHttpAntiForgeryToken]** atribut metody Post **ContactsController** k ochraně před hrozbami XSRF. Přidá k "PutContact", "PostContact" a **DeleteContact** metody akce.
   
        [ValidateHttpAntiForgeryToken]
            public IHttpActionResult PutContact(int id, Contact contact)
            {
5. Aktualizace *skripty* části *Views\Home\Index.cshtml* souboru kódu na získat tokeny XSRF.
   
         @section Scripts {
            @Scripts.Render("~/bundles/knockout")
            <script type="text/javascript">
                @functions{
                   public string TokenHeaderValue()
                   {
                      string cookieToken, formToken;
                      AntiForgery.GetTokens(null, out cookieToken, out formToken);
                      return cookieToken + ":" + formToken;                
                   }
                }
   
               function ContactsViewModel() {
                  var self = this;
                  self.contacts = ko.observableArray([]);
                  self.addContact = function () {
   
                     $.ajax({
                        type: "post",
                        url: "api/contacts",
                        data: $("#addContact").serialize(),
                        dataType: "json",
                        success: function (value) {
                           self.contacts.push(value);
                        },
                        headers: {
                           'RequestVerificationToken': '@TokenHeaderValue()'
                        }
                     });
   
                  }
                  self.removeContact = function (contact) {
                     $.ajax({
                        type: "DELETE",
                        url: contact.Self,
                        success: function () {
                           self.contacts.remove(contact);
                        },
                        headers: {
                           'RequestVerificationToken': '@TokenHeaderValue()'
                        }
   
                     });
                  }
   
                  $.getJSON("api/contacts", function (data) {
                     self.contacts(data);
                  });
               }
               ko.applyBindings(new ContactsViewModel());
            </script>
         }

## <a name="publish-the-application-update-to-azure-and-sql-database"></a>Publikovat aktualizaci aplikace na Azure a SQL Database
Publikování aplikace, opakujte postup, který jste postupovali podle dříve.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **publikovat**.
   
    ![Publikování][rxP]
2. Klikněte na kartu **Nastavení**.
3. V části **ContactsManagerContext(ContactsManagerContext)**, klikněte na tlačítko **v** ikonu změníte *vzdáleného připojovací řetězec* do připojovacího řetězce pro databázi kontaktu. Klikněte na tlačítko **ContactDB**.
   
    ![Nastavení](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt5.png)
4. Zaškrtněte políčko pro **spustit migrace Code First (spuštěno při spuštění aplikace)**.
5. Klikněte na tlačítko **Další** a pak klikněte na **Preview**. Visual Studio zobrazí seznam souborů, které bude přidán nebo aktualizován.
6. Klikněte na **Publikovat**.
   Po dokončení nasazení otevře prohlížeč na domovskou stránku aplikace.
   
    ![Index stránky s žádné kontakty.][intro001]
   
    Visual Studio publikovat proces automaticky nakonfigurované připojovací řetězec v nasazené *Web.config* souboru tak, aby odkazoval na databázi SQL. Také nakonfigurovat migrace Code First automaticky upgradu databáze na nejnovější verzi aplikace přistupovat k databázi po nasazení poprvé.
   
    V důsledku této konfiguraci Code First vytvořené databáze spuštěním kódu **počáteční** třídu, která jste vytvořili dříve. Stejně to při prvním pokusu o přístup k databázi po nasazení aplikace.
7. Zadejte kontakt, jako jste to udělali při spuštění aplikace místně, chcete-li ověřit, že nasazení databáze bylo úspěšné.

Až uvidíte, že položku, kterou zadáte je uložena a zobrazí se na stránce kontaktujte správce, víte, že byla uložena v databázi.

![Index stránky s kontakty][addwebapi004]

Aplikace je nyní spuštěna v cloudu, uložení svá data pomocí databáze SQL. Po dokončení testování aplikace v Azure, odstraňte jej. Aplikace je veřejný a nemá žádný mechanismus pro omezení přístupu.

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="next-steps"></a>Další kroky
Jiný způsob ukládání dat v aplikaci Azure je použití úložiště Azure, které poskytují úložiště nerelační data ve formě objekty BLOB a tabulek. Následující odkazy poskytují další informace o webového rozhraní API, rozhraní ASP.NET MVC a okno Azure.

* [Začínáme s MVC pomocí rozhraní Entity Framework][EFCodeFirstMVCTutorial]
* [Úvod do architektury ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [První rozhraní ASP.NET Web API](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)
* [Ladění WAWS](web-sites-dotnet-troubleshoot-visual-studio.md)

Tento kurz a ukázkové aplikace byla zapsána pomocí [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [ @RickAndMSFT ](https://twitter.com/RickAndMSFT)) s požádat o pomoc tní Dykstra a Jiří Dorrans (Twitter [ @blowdart ](https://twitter.com/blowdart)). 

Prosím ponechejte zpětná vazba týkající se vám líbilo nebo co chcete najdete v části zlepšila, jenom o samotné kurz, ale taky o produkty, které ukazuje. Vaše zpětná vazba pomůže stanovení priorit vylepšení. Zejména zajímá zjistit kolik zájmu v další automatizace procesu konfigurace a nasazení databáze členství. 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- bookmarks -->
[Add an OAuth Provider]: #addOauth
[Add Roles to the Membership Database]:#mbrDB
[Create a Data Deployment Script]:#ppd
[Update the Membership Database]:#ppd2
[setupdbenv]: #bkmk_setupdevenv
[setupwindowsazureenv]: #bkmk_setupwindowsazure
[createapplication]: #bkmk_createmvc4app
[deployapp1]: #bkmk_deploytowindowsazure1
[adddb]: #bkmk_addadatabase
[addcontroller]: #bkmk_addcontroller
[addwebapi]: #bkmk_addwebapi
[deploy2]: #bkmk_deploydatabaseupdate

<!-- links -->
[EFCodeFirstMVCTutorial]: http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
[dbcontext-link]: http://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx


<!-- images-->
[rxE]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxE.png
[rxP]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxP.png
[rx22]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/
[rxb2]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxb2.png
[rxz]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz.png
[rxzz]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxzz.png
[rxz2]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz2.png
[rxz3]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz3.png
[rxStyle]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxStyle.png
[rxz4]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz4.png
[rxz44]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz44.png
[rxNewCtx]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxNewCtx.png
[rxPrevDB]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxPrevDB.png
[rxOverwrite]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxOverwrite.png
[rxPWS]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxPWS.png
[rxNewCtx]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxNewCtx.png
[rxAddApiController]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxAddApiController.png
[rxFFchrome]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxFFchrome.png
[intro001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobil-intro-finished-web-app.png
[rxCreateWSwithDB]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxCreateWSwithDB.png
[setup007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-setup-azure-site-004.png
[setup009]: ../Media/dntutmobile-setup-azure-site-006.png
[newapp002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-createapp-002.png
[newapp004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-createapp-004.png
[firsdeploy007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-deploy1-publish-005.png
[firsdeploy009]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-deploy1-publish-007.png
[adddb001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-adddatabase-001.png
[adddb002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-adddatabase-002.png
[addcode001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-context-menu.png
[addcode002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-controller-dialog.png
[addcode004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-modify-index-context.png
[addcode005]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-contents-context-menu.png
[addcode007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-modify-bundleconfig-context.png
[addcode008]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-migrations-package-manager-menu.png
[addcode009]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-migrations-package-manager-console.png
[addwebapi004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-added-contact.png
[addwebapi006]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-save-returned-contacts.png
[addwebapi007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-contacts-in-notepad.png
[Add XSRF Protection]: #xsrf
[WebPIAzureSdk20NetVS12]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/WebPIAzureSdk20NetVS12.png
[Add XSRF Protection]: #xsrf
[ImportPublishSettings]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ImportPublishSettings.png
[ImportPublishProfile]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ImportPublishProfile.png
[PublishVSSolution]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/PublishVSSolution.png
[ValidateConnection]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ValidateConnection.png
[WebPIAzureSdk20NetVS12]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/WebPIAzureSdk20NetVS12.png
[prevent-csrf-attacks]: http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-(csrf)-attacks

