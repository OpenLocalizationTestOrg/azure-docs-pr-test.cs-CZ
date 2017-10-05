---
title: "Nasazení mobilní webové aplikace ASP.NET MVC 5 v Azure App Service"
description: "Kurz, který se naučíte, jak nasadit webovou aplikaci do služby Azure App Service pomocí funkce mobilní webové aplikace ASP.NET MVC 5."
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 0752c802-8609-4956-a755-686116913645
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: c98e9b485c52a82e5be5c0f6b0b67912d1e890b9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-an-aspnet-mvc-5-mobile-web-app-in-azure-app-service"></a>Nasazení mobilní webové aplikace ASP.NET MVC 5 v Azure App Service
V tomto kurzu naučit základní informace o tom, jak sestavit webové aplikace ASP.NET MVC 5, který je mobilní zařízení a nasadíte ho do Azure App Service. V tomto kurzu budete potřebovat [Visual Studio Express 2013 pro Web] [ Visual Studio Express 2013] nebo edice sady Visual Studio, pokud již máte, professional. Můžete použít [Visual Studio 2015] , ale snímky obrazovky se bude lišit a je nutné použít šablony ASP.NET 4.x.

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-youll-build"></a>Co budete sestavení
V tomto kurzu přidáte mobilní funkce pro jednoduchou aplikaci seznamu konference, která je součástí [starter projektu][StarterProject]. Následující snímek obrazovky ukazuje relace ASP.NET v hotová aplikace, jak je vidět v emulátoru prohlížeče v Internet Exploreru 11 F12 nástroje pro vývojáře.

![][FixedSessionsByTag]

Můžete použít nástroje pro vývojáře Internet Explorer 11 F12 a [nástroj Fiddler] [ Fiddler] pomoci při ladění aplikace. 

## <a name="skills-youll-learn"></a>Dovedností, které se dozvíte
Zde je, co se dozvíte:

* Postup publikování webové aplikace přímo do webové aplikace v Azure App Service pomocí sady Visual Studio 2013.
* Jak šablony ASP.NET MVC 5 pomocí rozhraní šablon stylů CSS Bootstrap zlepšit zobrazení na mobilních zařízeních
* Postup vytvoření zobrazení mobile specifické pro konkrétní mobilní prohlížeče, jako je iPhone a Android
* Postup vytvoření přizpůsobivý zobrazení (zobrazení, které reagují na různé prohlížeče na zařízeních)

## <a name="set-up-the-development-environment"></a>Nastavení vývojového prostředí
Nastavení vývojového prostředí instalací sady Azure SDK pro .NET 2.5.1 nebo novější. 

1. Chcete-li nainstalovat sadu Azure SDK pro .NET, klikněte na níže uvedený odkaz. Pokud nemáte dosud nainstalován Visual Studio 2013, bude nainstalována ve odkaz. Tento kurz vyžaduje Visual Studio 2013. [Azure SDK pro Visual Studio 2013][AzureSDKVs2013]
2. V okně webové platformy, klikněte na **nainstalovat** a pokračujte v instalaci.

Budete také potřebovat emulátoru prohlížeč pro mobilní zařízení. Bude fungovat některé z následujících:

* Emulátor prohlížeče v [nástroje pro vývojáře aplikace Internet Explorer 11 F12] [ EmulatorIE11] (používá se v všechny snímky obrazovky prohlížeč pro mobilní zařízení). Má přednastavení řetězec uživatelského agenta pro Windows Phone 8, Windows Phone 7 a Apple iPad.
* Emulátor prohlížeče v [Google Chrome DevTools][EmulatorChrome]. Obsahuje přednastavení množství zařízení se systémem Android, a také Apple iPhone, Apple iPad a Amazon Kindle ještě efektivněji. Emuluje také touch události.
* [Emulátoru mobilního Opera][EmulatorOpera]

Projekty Visual Studio s C\# zdrojového kódu jsou k dispozici v tomto tématu:

* [Stažení Starter projektu][StarterProject]
* [Dokončení stažení projektu][CompletedProject]

## <a name="bkmk_DeployStarterProject"></a>Nasazení projektu starter do webové aplikace Azure
1. Stažení aplikace konferenční výpis [starter projektu][StarterProject].
2. Potom v Průzkumníku Windows, klikněte pravým tlačítkem na stažený soubor ZIP a zvolte *vlastnosti*.
3. V **vlastnosti** dialogovém okně vyberte **Odblokovat** tlačítko. (Odblokování brání upozornění zabezpečení, která nastane, když se pokusíte použít *.zip* souboru, který jste stáhli z webu.)
4. Klikněte pravým tlačítkem na soubor ZIP a vyberte **Extrahovat vše** soubor rozbalit. 
5. V sadě Visual Studio, otevřete *C#\Mvc5Mobile.sln* souboru.
6. V Průzkumníku řešení klikněte pravým tlačítkem na projekt a klikněte na tlačítko **publikovat**.
   
   ![][DeployClickPublish]
7. V Publikovat Web, klikněte na **Microsoft Azure App Service**.
   
   ![][DeployClickWebSites]
8. Pokud již jste přihlášeni do Azure, klikněte na tlačítko **přidat účet**.
   
   ![][DeploySignIn]
9. Postupujte podle pokynů pro přihlášení k účtu Azure.
10. Dialogovém okně App Service, by měl nyní může zobrazit můžete jako přihlášení. Klikněte na možnost **Nové**.
    
    ![][DeployNewWebsite]  
11. V **název webové aplikace** pole, zadejte předponu aplikace jedinečný název. Váš název plně kvalifikovaný webové aplikace bude  *&lt;předpony >*. azurewebsites.net. Také vyberte nebo zadejte nový název skupiny prostředků v **skupiny prostředků**. Potom klikněte na **nový** vytvořit nový plán aplikační služby.
    
    ![][DeploySiteSettings]
12. Nakonfigurujte nový plán aplikační služby a klikněte na **OK**. 
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7a.png)
13. Zpět v dialogovém okně vytvořit službu App Service, klikněte na tlačítko **vytvořit**.
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7b.png) 
14. Po Azure prostředky jsou vytvořeny, Publikovat Web dialogové okno bude vyplněn nastavení pro novou aplikaci. Klikněte na **Publikovat**.
    
    ![][DeployPublishSite]
    
    Jakmile sady Visual Studio dokončí publikování starter projektu do webové aplikace Azure, otevře prohlížeč pro stolní počítač se živou webovou aplikaci.
15. Spusťte emulátor váš prohlížeč pro mobilní zařízení, zkopírujte adresu URL pro aplikaci konferenční (*<prefix>*. azurewebsites.net) do emulátoru a pak klikněte na tlačítko pravém horním a vyberte **Procházet podle značky**. Pokud používáte Internet Explorer 11 jako výchozího prohlížeče, bude třeba jen na typ `F12`, pak `Ctrl+8`a poté změňte prohlížeče profil, který se **Windows Phone**. Obrázek níže znázorňuje *AllTags* zobrazení v režimu na výšku (z výběru **Procházet podle značky**).
    
    ![][AllTags]

> [!TIP]
> Zatímco lze ladit aplikace MVC 5 v sadě Visual Studio, můžete publikovat webovou aplikaci do Azure znovu k ověření živou webovou aplikaci přímo z prohlížeče emulátoru nebo prohlížeč pro mobilní zařízení.
> 
> 

Zobrazení je velmi čitelná na mobilním zařízení. Můžete také již zobrazit některé vizuální efekty použít rámcem Bootstrap šablon stylů CSS.
Klikněte **ASP.NET** odkaz.

![][SessionsByTagASP.NET]

Zobrazení značek ASP.NET je přiblížení namontováno k obrazovce, která zajišťuje Bootstrap pro vás automaticky. Však může zvýšit toto zobrazení tak, aby lépe vyhovoval prohlížeč pro mobilní zařízení. Například **datum** sloupec je obtížné číst. Později v tomto kurzu budete změníte *AllTags* zobrazení, aby bylo mobilní zařízení.

## <a name="bkmk_bootstrap"></a>Framework Bootstrap šablon stylů CSS
V MVC 5 nová šablona je integrovanou podporu zavedení. Jste už viděli, jak okamžitě vylepšuje různá zobrazení ve vaší aplikaci. Navigační panel v horní části je například automaticky sbalitelné po menší šířku prohlížeče. Na ploše prohlížeč zkuste upravit velikost okna prohlížeče a najdete v části Jak navigační panel změní jeho vzhled a chování. Toto je návrh přizpůsobivý webu, který je součástí Bootstrap.

Chcete-li zjistit, jak by bez Bootstrap zobrazovat webové aplikace, otevřete *aplikace\_spustit\\BundleConfig.cs* a Odkomentujte řádky, které obsahují *bootstrap.js* a  *Bootstrap.CSS*. Následující kód ukazuje posledních dvou prohlášení o `RegisterBundles` metoda po provedení změny:

     bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
              //"~/Scripts/bootstrap.js",
              "~/Scripts/respond.js"));

    bundles.Add(new StyleBundle("~/Content/css").Include(
              //"~/Content/bootstrap.css",
              "~/Content/site.css"));

Stiskněte klávesu `Ctrl+F5` ke spuštění aplikace.

Sledujte sbalitelné navigačním panelu je teď právě obyčejnou neuspořádaný seznam. Klikněte na tlačítko **Procházet podle značky** znovu, pak klikněte na tlačítko **ASP.NET**.
V emulátoru mobilního zobrazení uvidíte teď, když je už přiblížení namontováno na obrazovku a musí přejděte do stran Chcete-li zobrazit pravé tabulky.

![][SessionsByTagASP.NETNoBootstrap]

Vrátit zpět změny a aktualizujte prohlížeč pro mobilní zařízení k ověření, že byla obnovena zobrazení mobilní zařízení.

Bootstrap není specifické pro ASP.NET MVC 5 a můžete využít výhod těchto funkcí v jakékoli webové aplikace. Ale nyní integrovaná do šablony projektu ASP.NET MVC 5, tak, aby MVC 5 webové aplikace mohou využít výhod Bootstrap ve výchozím nastavení.

Další informace o Bootstrap, přejděte na [Bootstrap] [ BootstrapSite] lokality.

V další části se zobrazí, jak poskytnout konkrétní zobrazení mobilní prohlížeče.

## <a name="bkmk_overrideviews"></a>Přepsání, zobrazení, rozložení a částečné zobrazení
Pro mobilní prohlížeče obecně pro jednotlivé mobilního prohlížeče nebo pro jakékoli konkrétní prohlížeč můžete přepsat všechna zobrazení (včetně rozložení a částečné zobrazení). Abyste si mohli zobrazit konkrétní mobilní, můžete zkopírovat soubor zobrazení a přidat *. Mobilní* k názvu souboru. Chcete-li například vytvořit mobilních *Index* zobrazení, můžete zkopírovat *zobrazení\\Domů\\Index.cshtml* k *zobrazení\\Domů\\ Index.Mobile.cshtml*.

V této části vytvoříte soubor specifické mobilní rozložení.

Chcete-li začít, zkopírujte *zobrazení\\sdílené\\\_Layout.cshtml* k *zobrazení\\sdílené\\\_Layout.Mobile.cshtml*. Otevřete  *\_Layout.Mobile.cshtml* a změňte název od **MVC5 aplikace** k **MVC5 aplikace (mobilní)**.

V každé `Html.ActionLink` volání pro navigační panel, odeberte "Procházet podle" v každé propojení *ActionLink*. Následující kód ukazuje dokončené `<ul class="nav navbar-nav">` značky mobilní rozložení souboru.

    <ul class="nav navbar-nav">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
    </ul>

Kopírování *zobrazení\\Domů\\AllTags.cshtml* do souboru *zobrazení\\Domů\\AllTags.Mobile.cshtml*. Otevřete nový soubor a změňte `<h2>` element z "Značky" na "značky (M)":

    <h2>Tags (M)</h2>

Přejděte na stránku značky pomocí prohlížeč pro stolní počítač a pomocí emulátoru prohlížeč pro mobilní zařízení. Emulátor mobilní prohlížeče ukazuje dva provedené změny (titul z  *\_Layout.Mobile.cshtml* a titul z *AllTags.Mobile.cshtml*).

![][AllTagsMobile_LayoutMobile]

Naproti tomu nedošlo ke změně zobrazení plochy (s názvy z  *\_Layout.cshtml* a *AllTags.cshtml*).

![][AllTagsMobile_LayoutMobileDesktop]

## <a name="bkmk_browserviews"></a>Vytvoření vlastních zobrazení
Kromě zobrazení specifická pro mobilní a desktop můžete vytvořit zobrazení pro jednotlivé prohlížeče. Můžete například vytvořit zobrazení, které jsou speciálně určené pro iPhone nebo prohlížeč systému Android. V této části vytvoříte rozložení pro prohlížeč iPhone a na zařízení iPhone verzi *AllTags* zobrazení.

Otevřete *Global.asax* souboru a přidejte následující kód k dolnímu okraji `Application_Start` metoda.

    DisplayModeProvider.Instance.Modes.Insert(0, new DefaultDisplayMode("iPhone")
    {
        ContextCondition = (context => context.GetOverriddenUserAgent().IndexOf
            ("iPhone", StringComparison.OrdinalIgnoreCase) >= 0)
    });

Tento kód definuje nový režim zobrazení s názvem "iPhone", který bude porovnání jednotlivých příchozích požadavků. Pokud příchozí požadavek odpovídá podmínku, kterou jste definovali (Pokud uživatelský agent obsahuje řetězec "iPhone"), rozhraní ASP.NET MVC bude hledat zobrazení, jejíž název obsahuje příponu "iPhone".

> [!NOTE]
> Při přidávání režimy mobilní zobrazení specifické pro prohlížeč, například konkrétní iPhone a Android, nezapomeňte nastavit první argument `0` (Vložit v horní části seznamu) a ujistěte se, specifické pro prohlížeč režim má přednost před mobilní šablony (*. Mobile.cshtml). Pokud mobilní šablona je k dispozici místo v horní části seznamu, bude vybrána přes vaše režim určený zobrazení (první shodu wins a mobilní šablonu odpovídá všechny mobilní prohlížeče). 
> 
> 

V kódu, klikněte pravým tlačítkem na `DefaultDisplayMode`, zvolte **vyřešit**a potom vyberte `using System.Web.WebPages;`. Tento postup přidá odkaz na `System.Web.WebPages` názvů, který je tam, kde `DisplayModeProvider` a `DefaultDisplayMode` typy jsou definované.

![][ResolveDefaultDisplayMode]

Alternativně můžete právě ručně přidejte následující řádek na `using` část souboru.

    using System.Web.WebPages;

Uložte změny. Kopírování *zobrazení\\sdílené\\\_Layout.Mobile.cshtml* do souboru *zobrazení\\sdílené\\\_Layout.iPhone.cshtml*. Otevřete nový soubor a potom změňte název od `MVC5 Application (Mobile)` k `MVC5 Application (iPhone)`.

Kopírování *zobrazení\\Domů\\AllTags.Mobile.cshtml* do souboru *zobrazení\\Domů\\AllTags.iPhone.cshtml*. Nový soubor, změňte `<h2>` element z "značky (M)" pro "Značky (iPhone)".

Spusťte aplikaci. Spustit prohlížeč pro mobilní zařízení emulátoru, ujistěte se jeho uživatelský agent je nastavena na "iPhone" a přejděte do *AllTags* zobrazení. Pokud používáte emulátor serveru v Internet Exploreru 11 F12 nástroje pro vývojáře, konfigurace emulace s následujícím:

* Profil Browser = **Windows Phone**
* Řetězec uživatelského agenta = **vlastní**
* Vlastní řetězec = **Apple-iPhone5C1/1001.525**

Následující snímek obrazovky ukazuje *AllTags* zobrazení vykreslen v emulátoru v Internet Exploreru 11 F12 nástroje pro vývojáře s vlastní identifikační řetězec prohlížeče (jedná se řetězec iPhone 5 C uživatelského agenta).

![][AllTagsIPhone_LayoutIPhone]

Mobilní prohlížeče, vyberte **Řečníci** odkaz. Protože mobilní zobrazení není (*AllSpeakers.Mobile.cshtml*), zobrazit výchozí mluvčí (*AllSpeakers.cshtml*) je vykreslen pomocí mobilních rozložení zobrazení ( *\_ Layout.Mobile.cshtml*). Jak je uvedeno níže, název **MVC5 aplikace (mobilní)** je definována v  *\_Layout.Mobile.cshtml*.

![][AllSpeakers_LayoutMobile]

Výchozí zobrazení (jiných než mobilních) z vykreslování uvnitř mobilní rozložení můžete zakázat globálně nastavením `RequireConsistentDisplayMode` k `true` v *zobrazení\\\_ViewStart.cshtml* souboru, například takto:

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = true;
    }

Když `RequireConsistentDisplayMode` je nastaven na `true`, mobilní rozložení (*\_Layout.Mobile.cshtml*) se používá pouze pro mobilní zobrazení (tj. Po zobrazení souboru ve formátu  ***ViewName**. Mobile.cshtml*). Můžete chtít nastavit `RequireConsistentDisplayMode` k `true` Pokud vaše mobilní rozložení nebude fungovat dobře u jiných než mobilních zobrazení. Na snímku obrazovky níže znázorňuje jak *Řečníci* stránka vykresluje při `RequireConsistentDisplayMode` je nastaven na `true` (bez řetězec "(mobilní)" v navigační panel v horní části).

![][AllSpeakers_LayoutMobileOverridden]

Konzistentní režim zobrazení v určitém zobrazení můžete zakázat nastavením `RequireConsistentDisplayMode` k `false` v souboru zobrazení. Následující kód v *zobrazení\\Domů\\AllSpeakers.cshtml* souboru nastaví `RequireConsistentDisplayMode` k `false`:

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All speakers";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = false;
    }

V této části jsme viděli postup vytvoření mobilní rozložení a zobrazení a postup vytvoření zobrazení pro konkrétní zařízení, jako je iPhone a rozložení.
Hlavní výhodou rozhraní Bootstrap šablon stylů CSS je však přizpůsobivé rozložení, což znamená, že jeden šablony stylů mohou být použity u plochy, phone a prohlížeče tabletu k vytvoření konzistentního vzhledu a chování. V další části se zobrazí, jak využít Bootstrap vytvořte zobrazení mobilní zařízení.

## <a name="bkmk_Improvespeakerslist"></a>Zlepšení seznamu mluvčí
Protože jste právě viděli, *mluvčí* zobrazení je čitelná, ale odkazy jsou malé a obtížně se klepnout na mobilní zařízení. V této části, budete mít *AllSpeakers* zobrazit mobilní zařízení, která zobrazuje velké, snadno klepněte odkazy a obsahuje vyhledávací pole a rychle tak najít mluvčí.

Můžete použít službou Bootstrap nástroje [skupiny odkazovaného seznamu] [ linked list group] stylů ke zlepšení *Řečníci* zobrazení. V *zobrazení\\Domů\\AllSpeakers.cshtml*, nahraďte obsah souboru Razor kód níže.

     @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, "SessionsBySpeaker", new { speaker }, new { @class = "list-group-item" })
        }
    </div>

`class="list-group"` Atribut `<div>` značky platí Bootstrap seznamu stylu a `class="input-group-item"` atribut platí stylů položky Bootstrap seznamu pro každý odkaz.

Aktualizujte prohlížeč pro mobilní zařízení. Aktualizovaná zobrazení vypadá takto:

![][AllSpeakersFixed]

Službou Bootstrap nástroje [skupiny odkazovaného seznamu] [ linked list group] stylů díky celé pole pro každý odkaz můžete kliknout, což je mnohem lepší prostředí. Přepněte do zobrazení plochy a sledovat konzistentní vzhled a chování.

![][AllSpeakersFixedDesktop]

I když se zlepšila zobrazení prohlížeč pro mobilní zařízení, je obtížné přejděte dlouhý seznam mluvčí. Bootstrap neposkytuje hledání filtru funkce out-of-the-box, ale můžete jej přidat pár řádků kódu. Nejprve bude přidejte vyhledávací pole k zobrazení a pak propojte s kód jazyka JavaScript pro funkci filtru. V *zobrazení\\Domů\\AllSpeakers.cshtml*, přidejte \<formuláře\> hned za značkami \<h2\> značka, jak je uvedeno níže:

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <form class="input-group">
        <span class="input-group-addon"><span class="glyphicon glyphicon-search"></span></span>
        <input type="text" class="form-control" placeholder="Search speaker">
    </form>
    <br />
    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class = "list-group-item" })
        }
    </div>

Všimněte si, že `<form>` a `<input>` značky oba mají Bootstrap stylů na ně použity. `<span>` Element přidá Bootstrap [glyphicon] [ glyphicon] do vyhledávacího pole.

V *skripty* složky, přidejte do souboru JavaScript s názvem *filter.js*. Otevřete soubor a vložte do něj následující kód:

    $(function () {

        // reset the search form when the page loads
        $("form").each(function () {
            this.reset();
        });

        // wire up the events to the <input> element for search/filter
        $("input").bind("keyup change", function () {
            var searchtxt = this.value.toLowerCase();
            var items = $(".list-group-item");

            // show all speakers that begin with the typed text and hide others
            for (var i = 0; i < items.length; i++) {
                var val = items[i].text.toLowerCase();
                val = val.substring(0, searchtxt.length);
                if (val == searchtxt) {
                    $(items[i]).show();
                }
                else {
                    $(items[i]).hide();
                }
            }
        });
    });

Musíte taky zahrnout filter.js registrovaných sad. Otevřete *aplikace\_spustit\\BundleConfig.cs* a změňte první sady. Změnit první `bundles.Add` – příkaz (pro **jquery** sady) zahrnout *skripty\\filter.js*, a to takto:

     bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-{version}.js",
                "~/Scripts/filter.js"));

**Jquery** sady je již vykreslen pomocí výchozí  *\_rozložení* zobrazení. Později můžete použít stejný kód JavaScript pro použití funkce filtru s dalšími zobrazeními seznamu.

Aktualizujte mobilní prohlížeče a přejděte na *AllSpeakers* zobrazení. Do vyhledávacího pole zadejte "sc". Seznamu mluvčí by měl být nyní filtrovaných podle hledanému řetězci.

![][AllSpeakersFixedSearchBySC]

## <a name="bkmk_improvetags"></a>Zlepšení seznam klíčových slov
Jako *Řečníci* zobrazení, *značky* zobrazení je čitelná, ale jsou odkazy na malé a obtížně se klepnout na mobilní zařízení. Můžete je vyřešit *značky* zobrazení stejným způsobem jako neopravíte *Řečníci* zobrazit, je-li použít změny kódu popsané výše, ale s následujícími službami `Html.ActionLink` syntaxe využívající metody v *zobrazení\\ Domů\\AllTags.cshtml*:

    @Html.ActionLink(tag, 
                     "SessionsByTag", 
                     new { tag }, 
                     new { @class = "list-group-item" })

Aktualizovat prohlížeč pro stolní počítač vypadá takto:

![][AllTagsFixedDesktop]

A aktualizovat mobilní prohlížeče vypadá takto: 

![][AllTagsFixed]

> [!NOTE]
> Pokud si všimnete, že původní formátování seznamu stále existuje v prohlížeč pro mobilní zařízení a zajímat, co se stalo s vaší dobrý Bootstrap stylů, jde artefakt starší akci k vytvoření mobilní konkrétní zobrazení. Teď, když používáte framework Bootstrap CSS k vytvoření návrhu přizpůsobivý webu, přejděte head a odebrat tyto specifické mobilní zobrazení a rozložení specifické pro mobilní zobrazení. Po dokončení tak aktualizovat mobilní prohlížeče zobrazí Bootstrap stylů.
> 
> 

## <a name="bkmk_improvedates"></a>Zlepšení seznamu kalendářních dat
Můžete zvýšit *data* zobrazit stejně, jako je vylepšená *Řečníci* a *značky* zobrazení, je-li použít změny kódu popsané výše, ale s následujícími službami `Html.ActionLink` Syntaxe využívající metody v *zobrazení\\Domů\\AllDates.cshtml*:

    @Html.ActionLink(date.ToString("ddd, MMM dd, h:mm tt"), 
                     "SessionsByDate", 
                     new { date }, 
                     new { @class = "list-group-item" })

Zobrazí se zobrazení aktualizovat prohlížeč pro mobilní zařízení takto:

![][AllDatesFixed]

Lze dále zvýšit *data* zobrazení uspořádání hodnoty data a času podle data. To lze provést pomocí Bootstrap [panelů] [ panels] styly. Nahraďte obsah *zobrazení\\Domů\\AllDates.cshtml* soubor s následujícím kódem:

    @model IEnumerable<DateTime>

    @{
        ViewBag.Title = "All Dates";
    }

    <h2>Dates</h2>

    @foreach (var dategroup in Model.GroupBy(x=>x.Date))
    {
        <div class="panel panel-primary">
            <div class="panel-heading">
                @dategroup.Key.ToString("ddd, MMM dd")
            </div>
            <div class="panel-body list-group">
                @foreach (var date in dategroup)
                {
                    @Html.ActionLink(date.ToString("h:mm tt"), 
                                     "SessionsByDate", 
                                     new { date }, 
                                     new { @class = "list-group-item" })
                }
            </div>
        </div>
    }

Tento kód vytvoří samostatné `<div class="panel panel-primary">` značky pro každé odlišné datum v seznamu a použije [skupiny odkazovaného seznamu] [ linked list group] pro příslušné odkazy jako předtím. Tady je prohlížeč pro mobilní zařízení, které bude vypadat takto spuštění tento kód:

![][AllDatesFixed2]

Přepnout na prohlížeč pro stolní počítač. Znovu si všimněte konzistentní vzhled.

![][AllDatesFixed2Desktop]

## <a name="bkmk_improvesessionstable"></a>Zlepšení SessionsTable zobrazení
V této části, budete mít *SessionsTable* zobrazit další mobilní zařízení. Tuto změnu je rozsáhlejší předchozí změny.

Mobilní prohlížeče, klepněte na **značka** tlačítko a potom zadejte `asp` do vyhledávacího pole.

![][AllTagsFixedSearchByASP]

Klepněte **ASP.NET** odkaz.

![][SessionsTableTagASP.NET]

Jak vidíte, zobrazení je naformátován jako tabulku, který je aktuálně určený lze zobrazit v prohlížeči počítače. Je však chvíli obtížné číst na prohlížeč pro mobilní zařízení. Chcete-li odstranit tento problém, otevřete *zobrazení\\Domů\\SessionsTable.cshtml* a obsah souboru nahraďte následujícím kódem:

    @model IEnumerable<Mvc5Mobile.Models.Session>

    <h2>@ViewBag.Title</h2>

    <div class="container">
        <div class="row">
            @foreach (var session in Model)
            {
                <div class="col-md-4">
                    <div class="list-group">
                        @Html.ActionLink(session.Title, 
                                         "SessionByCode", 
                                         new { session.Code }, 
                                         new { @class="list-group-item active" })
                        <div class="list-group-item">
                            <div class="list-group-item-text">
                                @Html.Partial("_SpeakersLinks", session)
                            </div>
                            <div class="list-group-item-info">
                                @session.DateText
                            </div>
                            <div class="list-group-item-info small hidden-xs">
                                @Html.Partial("_TagsLinks", session)
                            </div>
                        </div>
                    </div>
                </div>
            }
        </div>
    </div>

Kód provádí 3 věci:

* používá službou Bootstrap nástroje [skupiny vlastní odkazovaného seznamu] [ custom linked list group] k formátování informací o relaci ve svislém směru, aby tyto informace je čitelná na prohlížeč pro mobilní zařízení (pomocí třídy, jako je seznam skupiny položek textu)
* se vztahuje [mřížky systému] [ grid system] k rozložení, tak, aby položek relací toku vodorovně v desktopového prohlížeče a svisle mobilní prohlížeče (pomocí třídy sloupec md-4)
* používá [přizpůsobivý nástroje] [ responsive utilities] skrýt značky relace v mobilní prohlížeče (pomocí třídy skryté xs)

Taky můžou klepnout na odkaz title přejděte na příslušné relace. Následující obrázek zobrazuje změny kódu.

![][FixedSessionsByTag]

Zavedení mřížky systém, který můžete použít automaticky uspořádá relací svisle v prohlížeč pro mobilní zařízení. Všimněte si také, že nejsou zobrazeny značek. Přepnout na prohlížeč pro stolní počítač.

![][SessionsTableFixedTagASP.NETDesktop]

V prohlížeči počítače Všimněte si, že značky se teď zobrazují. Zkontrolujte také, že systém Bootstrap mřížky, které jste provedli Uspořádá položky relace v dva sloupce. V případě zvětšení v prohlížeči, zobrazí se, že změní uspořádání na tři sloupce.

## <a name="bkmk_improvesessionbycode"></a>Zlepšení SessionByCode zobrazení
Nakonec budete opravit *SessionByCode* zobrazení, aby bylo mobilní zařízení.

Mobilní prohlížeče, klepněte na **značka** tlačítko a potom zadejte `asp` do vyhledávacího pole.

![][AllTagsFixedSearchByASP]

Klepněte **ASP.NET** odkaz. Relace pro značku ASP.NET se zobrazí.

![][FixedSessionsByTag]

Vyberte **vytváření jedné stránky aplikace s ASP.NET a AngularJS** odkaz.

![][SessionByCode3-644]

Zobrazení plochy výchozí je v pořádku, ale můžete zlepšit vzhled snadno pomocí některé součásti Bootstrap grafického uživatelského rozhraní.

Otevřete *zobrazení\\Domů\\SessionByCode.cshtml* a nahraďte jeho obsah s následující kód:

    @model Mvc5Mobile.Models.Session

    @{
        ViewBag.Title = "Session details";
    }
    <h3>@Model.Title (@Model.Code)</h3>
    <p>
        <strong>@Model.DateText</strong> in <strong>@Model.Room</strong>
    </p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Speakers
        </div>
        @foreach (var speaker in Model.Speakers)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class="panel-body" })
        }
    </div>

    <p>@Model.Abstract</p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Tags
        </div>
        @foreach (var tag in Model.Tags)
        {
            @Html.ActionLink(tag, 
                             "SessionsByTag", 
                             new { tag }, 
                             new { @class = "panel-body" })
        }
    </div>

Nový kód používá Bootstrap panelů styly ke zlepšení mobilní zobrazení. 

Aktualizujte prohlížeč pro mobilní zařízení. Následující obrázek zobrazuje změny kódu, které jste právě vytvořili:

![][SessionByCodeFixed3-644]

## <a name="wrap-up-and-review"></a>Zabalení a zkontrolovat
Tento kurz vám ukázal jak vyvíjet webové aplikace pro mobilní zařízení pomocí ASP.NET MVC 5. Mezi ně patří:

* Nasazení aplikace ASP.NET MVC 5 do webové aplikace služby App Service
* Použít Bootstrap k vytvoření rozložení přizpůsobivý webové stránky v aplikaci MVC 5
* Přepsání částečné zobrazení, zobrazení a rozložení globálně i pro jednotlivé zobrazení
* Rozložení ovládacích prvků a částečné přepsat pomocí vynucení `RequireConsistentDisplayMode` vlastnost
* Vytvoření zobrazení, které cílí určité prohlížeče, jako je třeba iPhone prohlížeč
* Použít Bootstrap stylů v kódu Razor

## <a name="see-also"></a>Viz také
* [9 základní principy návrhu přizpůsobivý webu](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
* [Bootstrap][BootstrapSite]
* [Oficiální Blog zavedení][Official Bootstrap Blog]
* [Twitter Bootstrap kurzu z kurzu republika][Twitter Bootstrap Tutorial from Tutorial Republic]
* [Zavedení Playground][The Bootstrap Playground]
* [Osvědčené postupy W3C doporučení mobilní webové aplikace][W3C Recommendation Mobile Web Application Best Practices]
* [W3C Candidate doporučení pro dotazy na média][W3C Candidate Recommendation for media queries]

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- Internal Links -->
[Deploy the starter project to an Azure web app]: #bkmk_DeployStarterProject
[Bootstrap CSS Framework]: #bkmk_bootstrap
[Override the Views, Layouts, and Partial Views]: #bkmk_overrideviews
[Create Browser-Specific Views]:#bkmk_browserviews
[Improve the Speakers List]: #bkmk_Improvespeakerslist
[Improve the Tags List]: #bkmk_improvetags
[Improve the Dates List]: #bkmk_improvedates
[Improve the SessionsTable View]: #bkmk_improvesessionstable
[Improve the SessionByCode View]: #bkmk_improvesessionbycode

<!-- External Links -->
[Visual Studio Express 2013]: http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-web
[Visual Studio 2015]: https://www.visualstudio.com/downloads/download-visual-studio-vs
[AzureSDKVs2013]: http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409
[Fiddler]: http://www.fiddler2.com/fiddler2/
[EmulatorIE11]: http://msdn.microsoft.com/library/ie/dn255001.aspx
[EmulatorChrome]: https://developers.google.com/chrome-developer-tools/docs/mobile-emulation
[EmulatorOpera]: http://www.opera.com/developer/tools/mobile/
[StarterProject]: http://go.microsoft.com/fwlink/?LinkID=398780&clcid=0x409
[CompletedProject]: http://go.microsoft.com/fwlink/?LinkID=398781&clcid=0x409
[BootstrapSite]: http://getbootstrap.com/
[WebPIAzureSdk23NetVS13]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/WebPIAzureSdk23NetVS13.png
[linked list group]: http://getbootstrap.com/components/#list-group-linked
[glyphicon]: http://getbootstrap.com/components/#glyphicons
[panels]: http://getbootstrap.com/components/#panels
[custom linked list group]: http://getbootstrap.com/components/#list-group-custom-content
[grid system]: http://getbootstrap.com/css/#grid
[responsive utilities]: http://getbootstrap.com/css/#responsive-utilities
[Official Bootstrap Blog]: http://blog.getbootstrap.com/
[Twitter Bootstrap Tutorial from Tutorial Republic]: http://www.tutorialrepublic.com/twitter-bootstrap-tutorial/
[The Bootstrap Playground]: http://www.bootply.com/
[W3C Recommendation Mobile Web Application Best Practices]: http://www.w3.org/TR/mwabp/
[W3C Candidate Recommendation for media queries]: http://www.w3.org/TR/css3-mediaqueries/

<!-- Images -->
[DeployClickPublish]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-1.png
[DeployClickWebSites]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-2.png
[DeploySignIn]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-3.png
[DeployUsername]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-4.png
[DeployPassword]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-5.png
[DeployNewWebsite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-6.png
[DeploySiteSettings]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7.png
[DeployPublishSite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-8.png
[MobileHomePage]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/mobile-home-page.png
[FixedSessionsByTag]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-Fixed.png
[AllTags]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags.png
[SessionsByTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET.png
[SessionsByTagASP.NETNoBootstrap]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-NoBootstrap.png
[AllTagsMobile_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile.png
[AllTagsMobile_LayoutMobileDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile-Desktop.png
[ResolveDefaultDisplayMode]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/Resolve-DefaultDisplayMode.png
[AllTagsIPhone_LayoutIPhone]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsIPhone-_LayoutIPhone.png
[AllSpeakers_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile.png
[AllSpeakers_LayoutMobileOverridden]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile-Overridden.png
[AllSpeakersFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed.png
[AllSpeakersFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-Desktop.png
[AllSpeakersFixedSearchBySC]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-SearchBySC.png
[AllTagsFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-Desktop.png 
[AllTagsFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed.png
[AllDatesFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed.png
[AllDatesFixed2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2.png
[AllDatesFixed2Desktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2-Desktop.png
[AllTagsFixedSearchByASP]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-SearchByASP.png
[SessionsTableTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Tag-ASP.NET.png
[SessionsTableFixedTagASP.NETDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Fixed-Tag-ASP.NET-Desktop.png
[SessionByCode3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-3-644.png
[SessionByCodeFixed3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-Fixed-3-644.png

