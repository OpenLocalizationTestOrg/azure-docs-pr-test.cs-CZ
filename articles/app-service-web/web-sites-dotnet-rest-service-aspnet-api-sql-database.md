---
title: "aaaCreate rozhraní REST API v Azure pomocí technologie ASP.NET a databáze SQL | Microsoft Docs"
description: "Kurz, se naučíte, jak toodeploy aplikaci, která používá hello tooan rozhraní ASP.NET Web API webové aplikace Azure pomocí sady Visual Studio."
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
ms.openlocfilehash: 1ef45dd1582bfda367e53c39f863164422ad678b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-rest-service-using-aspnet-web-api-and-sql-database-in-azure-app-service"></a>Vytvoření služby pomocí rozhraní ASP.NET Web API a databázi SQL v Azure App Service
Tento kurz ukazuje, jak toodeploy technologie ASP.NET webové aplikace tooan [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) pomocí Průvodce publikování webu hello v sadě Visual Studio 2013 nebo Visual Studio 2013 Community Edition. 

Můžete otevřít účet Azure zdarma a pokud ještě nemáte Visual Studio 2013, hello SDK automaticky nainstaluje Visual Studio 2013 pro produkt Web Express. Proto můžete spustit vývoj pro Azure zcela zdarma.

Tento kurz předpokládá, že máte žádné předchozí zkušenosti s používáním Azure. Po dokončení tohoto kurzu, budete mít jednoduché webové aplikace nahoru a spouštění v cloudu hello.

Naučíte se:

* Jak tooenable počítači pro vývoj pro Azure nainstalováním hello Azure SDK.
* Jak toocreate Visual Studio ASP.NET MVC 5 projektu a publikujete ho v tooan aplikace Azure.
* Jakým způsobem volá toouse hello rozhraní ASP.NET Web API tooenable rozhraní Restful API.
* Jak toouse SQL databáze toostore data v Azure.
* Jak aplikace toopublish aktualizuje tooAzure.

Budete vytvářet jednoduché seznamu kontaktů webovou aplikaci, která je založená na technologii ASP.NET MVC 5 a používá hello ADO.NET Entity Framework pro přístup k databázi. Následující obrázek ukazuje hello Hello dokončil aplikace:

![snímek obrazovky webové stránky][intro001]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

### <a name="create-hello-project"></a>Vytvoření projektu hello
1. Spusťte Visual Studio 2013.
2. Z hello **soubor** nabídce klikněte na tlačítko **nový projekt**.
3. V hello **nový projekt** dialogové okno, rozbalte seznam **Visual C#** a vyberte **webové** a pak vyberte **webové aplikace ASP.NET**. Název aplikace hello **ContactManager** a klikněte na tlačítko **OK**.
   
    ![Dialogové okno Nový projekt](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr4.png)
4. V hello **nový projekt ASP.NET** dialogové okno, vyberte hello **MVC** šablony, zkontrolujte **webového rozhraní API** a pak klikněte na **změna ověřování**.
5. V hello **změna ověřování** dialogové okno, klikněte na tlačítko **bez ověřování**a potom klikněte na **OK**.
   
    ![Bez ověřování](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/GS13noauth.png)
   
    Funkce, které vyžadují toolog uživatelé v nebude mít Hello ukázkovou aplikaci, kterou vytváříte. Informace o tooimplement funkce ověřování a autorizace, najdete v části hello [další kroky](#nextsteps) oddíl hello konce tohoto kurzu. 
6. V hello **nový projekt ASP.NET** dialogové okno, ujistěte se, že hello **hostitel v cloudu hello** je zaškrtnuté políčko a klikněte na tlačítko **OK**.

Pokud nejste přihlášeni dříve tooAzure, bude výzvami toosign v.

1. Průvodce konfigurací Hello navrhne jedinečný název založený na *ContactManager* (viz následující obrázek hello). Vyberte oblast okolo vás. Můžete použít [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") toofind hello nejnižší latenci datového centra. 
2. Pokud jste dosud nevytvořili databázový server před, vyberte **vytvořit nový server**, zadejte jméno uživatele databáze a heslo.
   
    ![Konfigurace webu Azure](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configAz.PNG)

Pokud máte databázový server, použijte tento toocreate novou databázi. Databázové servery jsou drahocenný prostředků a obvykle mají toocreate několik databází hello stejný server pro testování a vývoj, nikoli databázový server na databázi. Zkontrolujte, zda webový server a databáze jsou v hello stejné oblasti.

![Konfigurace webu Azure](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configWithDB.PNG)

### <a name="set-hello-page-header-and-footer"></a>Nastavit hello záhlaví a zápatí stránky
1. V **Průzkumníku řešení**, rozbalte položku hello *Views\Shared* složku a otevřete hello *_Layout.cshtml* souboru.
   
    ![_Layout.cshtml v Průzkumníku řešení][newapp004]
2. Nahraďte obsah hello hello *Views\Shared_Layout.cshtml* soubor s hello následující kód:

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

Hello značek výše název aplikace hello změny z "Moje aplikace technologie ASP.NET" příliš "obraťte se na správce" a odebere odkazy hello příliš**Domů**, **o** a **kontaktujte**.

### <a name="run-hello-application-locally"></a>Místní spuštění aplikace hello
1. Stisknutím kombinace kláves CTRL + F5 toorun hello aplikace.
   domovskou stránku Hello aplikace se zobrazí v hello výchozí prohlížeč.
    ![tooDo seznamu domovskou stránku](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)

To je vše, že je nutné toodo pro nyní toocreate hello aplikaci nasadíte tooAzure. Později přidáte funkce databáze.

## <a name="deploy-hello-application-tooazure"></a>Nasazení aplikace tooAzure hello
1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt hello v **Průzkumníku řešení** a vyberte **publikovat** hello místní nabídce.
   
    ![Publikování v kontextové nabídce projektu][PublishVSSolution]
   
    Hello **Publikovat Web** otevře se průvodce.
2. Klikněte na **Publikovat**.

![Karta nastavení](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/pw.png)

Sada Visual Studio spustí proces hello kopírování hello soubory toohello Azure server. Hello **výstup** okno zobrazuje, jaké akce nasazení byly provedeny a hlásí úspěšné dokončení nasazení hello.

1. Adresa URL toohello hello nasazené lokality se automaticky otevře v Hello výchozí prohlížeč.
   
   Hello aplikaci, kterou jste vytvořili je nyní spuštěna v cloudu hello.
   
   ![tooDo seznamu domovskou stránku běžící v Azure][rxz2]

## <a name="add-a-database-toohello-application"></a>Přidání aplikace toohello databáze
Dále budete aktualizovat hello MVC aplikace tooadd hello možnost toodisplay a aktualizovat kontakty a uložení hello dat v databázi. aplikace Hello použije hello Entity Framework toocreate hello databáze a tooread a aktualizovat data v databázi hello.

### <a name="add-data-model-classes-for-hello-contacts"></a>Přidání třídy modelu dat pro kontakty hello
Začněte vytvořením jednoduchého datového modelu v kódu.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na složku modely hello, klikněte na **přidat**a potom **třída**.
   
    ![Přidání třídy v kontextové nabídce složku modely][adddb001]
2. V hello **přidat novou položku** dialogové okno, název hello nový soubor třídy *Contact.cs*a potom klikněte na **přidat**.
   
    ![Přidat novou položku – dialogové okno][adddb002]
3. Nahraďte hello obsah souboru Contacts.cs hello hello následující kód.
   
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

Hello **obraťte se na** třída definuje hello dat, které se uloží pro každý kontakt a primární klíč, KódKontaktu, která je potřeba hello databáze. Můžete získat další informace o datových modelech v hello [další kroky](#nextsteps) oddíl hello konce tohoto kurzu.

### <a name="create-web-pages-that-enable-app-users-toowork-with-hello-contacts"></a>Vytvoření webové stránky, které umožňují uživatelům toowork aplikace s kontakty hello
Hello funkce generování uživatelského rozhraní ASP.NET MVC hello může automaticky vygenerovat kód, který provádí vytvářet, číst, aktualizovat a odstraňovat akcemi (CRUD).

## <a name="add-a-controller-and-a-view-for-hello-data"></a>Přidání Kontroleru a zobrazení pro hello data
1. V **Průzkumníku**, rozbalte složku řadiče hello.
2. Sestavení projektu hello **(Ctrl + Shift + B)**. (Před použitím mechanismus generování uživatelského rozhraní musí sestavte projekt hello.) 
3. Klikněte pravým tlačítkem na složku hello řadiče a klikněte na tlačítko **přidat**a potom klikněte na **řadič**.
   
    ![Přidat řadič v kontextové nabídce řadiče složky][addcode001]
4. V hello **přidat vygenerované uživatelské rozhraní** dialogové okno, vyberte **kontroler MVC se zobrazeními s využitím nástroje Entity Framework** a klikněte na tlačítko **přidat**.
   
   ![Přidání kontroleru](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rrAC.png)
5. Nastavte název řadiče hello příliš**HomeController**. Vyberte **kontaktujte** jako třídu modelu. Klikněte na tlačítko hello **nový kontext dat** tlačítko a přijměte výchozí hello "ContactManager.Models.ContactManagerContext" pro hello **nový typ kontextu dat**. Klikněte na tlačítko **Přidat**.

    Dialogové okno zobrazí výzvu: "soubor s názvem hello HomeController již ukončí. Chcete, aby tooreplace ho? ". Klikněte na **Ano**. Přepisování jsme hello Domů řadiče, který byl vytvořen s hello nový projekt. Použijeme hello nové Domů řadiče pro naše seznamu kontaktů.

    Visual Studio vytvoří metody kontroleru a zobrazení pro operace CRUD databáze pro **kontaktujte** objekty.

## <a name="enable-migrations-create-hello-database-add-sample-data-and-a-data-initializer"></a>Povolit migrace a vytvořit databázi hello, Přidání ukázkových dat a inicializátoru dat
Další úlohou Hello je tooenable hello [migrace Code First](http://curah.microsoft.com/55220) funkce v pořadí toocreate hello databáze založené na datový model hello jste vytvořili.

1. V hello **nástroje** nabídce vyberte možnost **Správce balíčků knihoven** a potom **Konzola správce balíčků**.
   
    ![Konzola správce balíčků v nabídce Nástroje][addcode008]
2. V hello **Konzola správce balíčků** okno, zadejte následující příkaz hello:
   
        enable-migrations 
   
    Hello **enable-migrations se** příkaz vytvoří *migrace* složku a její uloží je v této složce *Configuration.cs* souboru, můžete upravit tooconfigure migrace. 
3. V hello **Konzola správce balíčků** okno, zadejte následující příkaz hello:
   
        add-migration Initial
   
    Hello **přidat migrace počáteční** příkaz vygeneruje třídy s názvem  **&lt;date_stamp&gt;počáteční** vytvářející hello databáze. první parametr Hello ( *počáteční* ) je libovolný a slouží toocreate hello název souboru hello. Uvidíte hello nové třídy soubory ve **Průzkumníku řešení**.
   
    V hello **počáteční** třídy, hello **až** metoda vytvoří tabulku kontaktů hello a hello **dolů** – metoda (používá, když chcete, aby tooreturn toohello předchozí stav) se zahodí.
4. Otevřete hello *Migrations\Configuration.cs* souboru. 
5. Přidejte následující obory názvů hello. 
   
         using ContactManager.Models;
6. Nahraďte hello *počáteční hodnoty* metoda s hello následující kód:
   
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
   
    Tento kód výše inicializuje hello databáze s hello kontaktní informace. Další informace o synchronizace replik indexů databáze hello najdete v tématu [ladění Entity Framework (EF) databází](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).
7. V hello **Konzola správce balíčků** zadejte příkaz hello:
   
        update-database
   
    ![Konzola správce balíčků příkazy][addcode009]
   
    Hello **update-database** spustí hello první migrace, která vytvoří databázi hello. Ve výchozím nastavení je databáze hello vytvoří jako databáze SQL serveru Express LocalDB.
8. Stisknutím kombinace kláves CTRL + F5 toorun hello aplikace. 

aplikace Hello zobrazuje hello počáteční hodnoty data a poskytuje úpravy, podrobnosti a odkazy odstranit.

![Zobrazení MVC dat][rxz3]

## <a name="edit-hello-view"></a>Upravit hello zobrazení
1. Otevřete hello *Views\Home\Index.cshtml* souboru. V dalším kroku hello jsme nahradí hello vygeneruje kód s kódem, který používá [jQuery](http://jquery.com/) a [Knockout.js](http://knockoutjs.com/). Tento nový kód načte hello seznamu kontaktů pomocí webového rozhraní API a JSON a pak vazby hello kontaktovat toohello data uživatelského rozhraní pomocí knockout.js. Další informace najdete v tématu hello [další kroky](#nextsteps) oddíl hello konce tohoto kurzu. 
2. Nahraďte obsah souboru hello hello hello následující kód.
   
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
3. Klikněte pravým tlačítkem na složku obsahu hello a klikněte na tlačítko **přidat**a pak klikněte na tlačítko **novou položku...** .
   
    ![Přidání šablony stylů v kontextové nabídce složky obsahu][addcode005]
4. V hello **přidat novou položku** dialogovém okně zadejte **styl** v horním pravém vyhledávacího pole text hello a potom vyberte **list stylu**.
    ![Přidat novou položku – dialogové okno][rxStyle]
5. Název souboru hello *Contacts.css* a klikněte na tlačítko **přidat**. Nahraďte obsah souboru hello hello hello následující kód.
   
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
   
    Budeme používat tuto šablonu stylů pro hello rozložení, barvy a styly využívané v aplikaci hello kontaktujte správce.
6. Otevřete hello *App_Start\BundleConfig.cs* souboru.
7. Přidejte následující kód tooregister hello hello [Knockout](http://knockoutjs.com/index.html "KO") modulu plug-in.
   
        bundles.Add(new ScriptBundle("~/bundles/knockout").Include(
                    "~/Scripts/knockout-{version}.js"));
    Tato ukázka pomocí knockout toosimplify dynamické JavaScript kód, který zpracovává šablony obrazovky hello.
8. Upravit hello obsah nebo šablon stylů css položka tooregister hello *contacts.css* šabloně stylů. Změna hello následující řádek:
   
                 bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/site.css"));
   na:
   
        bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/contacts.css",
                   "~/Content/site.css"));
9. Hello Konzola správce balíčků spusťte následující příkaz tooinstall Knockout hello.
   
        Install-Package knockoutjs

## <a name="add-a-controller-for-hello-web-api-restful-interface"></a>Přidání kontroleru rozhraní Web API Restful hello
1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na řadiče a klikněte na **přidat** a potom **řadiče...** 
2. V hello **přidat vygenerované uživatelské rozhraní** dialogovém okně zadejte **webové 2 kontroler API s akcemi používající rozhraní Entity Framework** a pak klikněte na **přidat**.
   
    ![Přidat kontroler API](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt1.png)
3. V hello **přidat kontroler** dialogovém okně zadejte "ContactsController" jako název řadiče. Vyberte "Kontakt (ContactManager.Models)" pro hello **třída modelu**.  Ponechte výchozí hodnotu hello pro hello **třída kontextu dat**. 
4. Klikněte na tlačítko **Přidat**.

### <a name="run-hello-application-locally"></a>Místní spuštění aplikace hello
1. Stisknutím kombinace kláves CTRL + F5 toorun hello aplikace.
   
    ![Indexová stránka][intro001]
2. Zadejte kontakt a klikněte na **přidat**. aplikace Hello vrátí toohello domovské stránce a zobrazí hello kontaktu, které jste zadali.
   
    ![Index stránky s položkami seznamu úkolů][addwebapi004]
3. V prohlížeči hello připojit **/api/contacts** toohello adresy URL.
   
    Výsledná adresa URL Hello bude vypadat http://localhost:1234/api/contacts. Hello RESTful webová rozhraní API, které jste přidali vrátí hello uložené kontakty. Firefox) a Chrome (zobrazí hello data ve formátu XML.
   
    ![Index stránky s položkami seznamu úkolů][rxFFchrome]

    Aplikace Internet Explorer bude výzvu tooopen nebo ukládat kontakty hello.

    ![Dialogové okno Uložit webového rozhraní API][addwebapi006]


    Můžete otevřít hello kontakty, vrátí se v programu Poznámkový blok nebo prohlížeče.

    Tento výstup mohou být spotřebovávána jiná aplikace, jako je například mobilní webové stránky nebo aplikace.

    ![Dialogové okno Uložit webového rozhraní API][addwebapi007]

    **Upozornění zabezpečení**: V tomto okamžiku je vaše aplikace tooCSRF nezabezpečené a stát terčem útoku. Později v kurzu hello jsme se odebrat toto ohrožení zabezpečení. Další informace najdete v části [útoky brání webů požadavku padělání (proti útokům CSRF)][prevent-csrf-attacks].
## <a name="add-xsrf-protection"></a>Přidat ochranu XSRF
Padělání požadavku posílaného mezi weby (také označované jako XSRF nebo proti útokům CSRF) je útok na hostované webové aplikace, které škodlivou webovou stránku můžete ovlivnit hello interakce mezi prohlížeče klienta a důvěřují prohlížeč tohoto webu. Tyto útoky jsou možné, protože webových prohlížečů bude odesílat tokeny ověřování automaticky s každou žádost tooa webu. Příklad kanonický Hello je soubor cookie ověřování, jako je například ASP. Lístek ověřování pomocí formulářů pro Asp.net. Tyto útoky však může být cílem weby, které použít žádné trvalé ověřovací mechanismus (například ověřování systému Windows, Basic a tak dále).

Útok XSRF se liší od útoky phishing. Útoky phishing nevyžadovaly interakci postižené hello. V rámci útoku phishing škodlivou webovou stránku bude napodobovat hello cílového webu a postižené hello je oklamat do poskytování útočník toohello citlivé informace. Při útoku XSRF není často žádná interakce potřebné z postižené hello. Místo toho je hello útočník spoléhat na hello prohlížeče automaticky odesílat všechny relevantní soubory cookie toohello cílového webu.

Další informace najdete v tématu hello [otevřete projekt webové aplikace zabezpečení](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).

1. V **Průzkumníku řešení**, vpravo **ContactManager** projektu a klikněte na tlačítko **přidat** a pak klikněte na **třída**.
2. Název souboru hello *ValidateHttpAntiForgeryTokenAttribute.cs* a přidejte následující kód hello:
   
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
3. Přidejte následující hello *pomocí* toohello příkaz měnící řadiče, abyste získali přístup toohello **[ValidateHttpAntiForgeryToken]** atribut.
   
        using ContactManager.Filters;
4. Přidat hello **[ValidateHttpAntiForgeryToken]** atribut metody Post toohello hello **ContactsController** tooprotect z XSRF hrozeb. Můžete ho přidá toohello "PutContact", "PostContact" a **DeleteContact** metody akce.
   
        [ValidateHttpAntiForgeryToken]
            public IHttpActionResult PutContact(int id, Contact contact)
            {
5. Aktualizace hello *skripty* části hello *Views\Home\Index.cshtml* souboru tooinclude kód tooget hello XSRF tokeny.
   
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

## <a name="publish-hello-application-update-tooazure-and-sql-database"></a>Publikovat tooAzure aktualizace aplikace hello a databáze SQL
aplikace hello toopublish, opakujte hello postupu, který jste postupovali podle dříve.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **publikovat**.
   
    ![Publikování][rxP]
2. Klikněte na tlačítko hello **nastavení** kartě.
3. V části **ContactsManagerContext(ContactsManagerContext)**, klikněte na tlačítko hello **v** ikonu toochange *vzdáleného připojovací řetězec* toohello připojovací řetězec získáte hello databáze. Klikněte na tlačítko **ContactDB**.
   
    ![Nastavení](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt5.png)
4. Zaškrtněte políčko hello pro **spustit migrace Code First (spuštěno při spuštění aplikace)**.
5. Klikněte na tlačítko **Další** a pak klikněte na **Preview**. Visual Studio zobrazí seznam hello souborů, které bude přidán nebo aktualizován.
6. Klikněte na **Publikovat**.
   Po dokončení nasazení hello hello prohlížeči se otevře toohello domovskou stránku hello aplikace.
   
    ![Index stránky s žádné kontakty.][intro001]
   
    Hello Visual Studio publikovat proces automaticky nakonfiguruje hello připojovací řetězec v hello nasazené *Web.config* soubor toopoint toohello SQL database. Také nakonfigurovat migrace Code First tooautomatically upgradu hello toohello nejnovější verze databáze, že hello první čas hello aplikace přistupuje k databázi hello po nasazení.
   
    V důsledku této konfiguraci Code First vytvořené databáze hello spuštěním kódu hello v hello **počáteční** třídu, která jste vytvořili dříve. Tato hello první čas hello aplikace se pokusila tooaccess hello databáze se nespustil po nasazení.
7. Zadejte kontakt, jako jste to udělali při spuštění místně, aplikace hello tooverify o úspěšném nasazení databáze.

Až uvidíte, že hello položku, kterou zadáte je uložena a zobrazí se na stránku hello kontaktujte správce, víte, že byla uložena v databázi hello.

![Index stránky s kontakty][addwebapi004]

Hello aplikace je nyní spuštěna v cloudu hello pomocí SQL Database toostore jeho data. Po dokončení testování hello aplikace v Azure, odstraňte jej. aplikace Hello je veřejný a nemá přístup k toolimit mechanismus.

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="next-steps"></a>Další kroky
Jiný způsob toostore dat v aplikaci Azure je toouse úložiště Azure, které poskytují úložiště nerelační data ve formuláři hello objekty BLOB a tabulek. Následující odkazy Hello poskytují další informace o webového rozhraní API, rozhraní ASP.NET MVC a okno Azure.

* [Začínáme s MVC pomocí rozhraní Entity Framework][EFCodeFirstMVCTutorial]
* [Úvod tooASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [První rozhraní ASP.NET Web API](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)
* [Ladění WAWS](web-sites-dotnet-troubleshoot-visual-studio.md)

Ukázkovou aplikaci tohoto kurzu a hello napsal [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [ @RickAndMSFT ](https://twitter.com/RickAndMSFT)) s požádat o pomoc tní Dykstra a Jiří Dorrans (Twitter [ @blowdart ](https://twitter.com/blowdart)). 

Na co líbilo nebo co chcete toosee zlepšila, jenom o hello kurzu sám sebe, ale taky o hello produkty, které ukazuje prosím sdělit svůj názor. Vaše zpětná vazba pomůže stanovení priorit vylepšení. Zejména zajímá zjistit, kolik vás zajímají je v další automatizace procesu hello konfigurace a nasazení databáze členství hello. 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- bookmarks -->
[Add an OAuth Provider]: #addOauth
[Add Roles toohello Membership Database]:#mbrDB
[Create a Data Deployment Script]:#ppd
[Update hello Membership Database]:#ppd2
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

