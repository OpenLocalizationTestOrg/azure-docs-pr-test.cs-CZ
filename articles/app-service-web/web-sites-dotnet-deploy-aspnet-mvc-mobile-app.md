---
title: "aaaDeploy ASP.NET MVC 5 mobilní webové aplikace v Azure App Service"
description: "Kurz, který se naučíte, jak toodeploy tooAzure webové aplikace služby App Service pomocí mobilní funkce v architektuře ASP.NET MVC 5 webové aplikace."
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
ms.openlocfilehash: 01119c07246c0252fd357562774a2e90b3ef77d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-aspnet-mvc-5-mobile-web-app-in-azure-app-service"></a>Nasazení mobilní webové aplikace ASP.NET MVC 5 v Azure App Service
V tomto kurzu se naučit hello základní informace o tom, jak toobuild ASP.NET MVC 5 webové aplikaci, která je mobilní zařízení a nasadit tooAzure služby App Service. V tomto kurzu budete potřebovat [Visual Studio Express 2013 pro Web] [ Visual Studio Express 2013] nebo edice professional hello sady Visual Studio, pokud již máte který. Můžete použít [Visual Studio 2015] , ale snímky obrazovky hello se bude lišit a je nutné použít hello ASP.NET 4.x šablony.

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-youll-build"></a>Co budete sestavení
V tomto kurzu přidáte mobilní funkce toohello jednoduchou konferenční výpis aplikaci, která je k dispozici v hello [starter projektu][StarterProject]. Hello následující snímek obrazovky ukazuje relací ASP.NET hello v aplikaci hello dokončit, jak je vidět v emulátoru hello prohlížeče v Internet Exploreru 11 F12 nástroje pro vývojáře.

![][FixedSessionsByTag]

Můžete použít nástroje pro vývojáře hello Internet Explorer 11 F12 a hello [nástroj Fiddler] [ Fiddler] toohelp ladění aplikace. 

## <a name="skills-youll-learn"></a>Dovedností, které se dozvíte
Zde je, co se dozvíte:

* Jak toouse Visual Studio 2013 toopublish webové aplikace přímo tooa webové aplikace v Azure App Service.
* Jak hello ASP.NET MVC 5 šablony pomocí šablon stylů CSS Bootstrap framework hello zlepšit zobrazení na mobilních zařízeních
* Jak toocreate specifické mobilní zobrazení tootarget konkrétní mobilní prohlížeče, jako je hello iPhone a Android
* Jak toocreate přizpůsobivý zobrazení (zobrazení, které reagují na zařízeních toodifferent prohlížeče)

## <a name="set-up-hello-development-environment"></a>Nastavit hello vývojového prostředí
Nastavení vývojového prostředí instalací hello Azure SDK pro .NET 2.5.1 nebo novější. 

1. tooinstall hello Azure SDK pro platformu .NET, klikněte na níže uvedený odkaz hello. Pokud nemáte Visual Studio 2013 ještě nainstalované, nainstaluje se podle hello propojení. Tento kurz vyžaduje Visual Studio 2013. [Azure SDK pro Visual Studio 2013][AzureSDKVs2013]
2. V okně hello instalačního programu webové platformy, klikněte na tlačítko **nainstalovat** a pokračovat v instalaci hello.

Budete také potřebovat emulátoru prohlížeč pro mobilní zařízení. Některé z následujících hello bude fungovat:

* Emulátor prohlížeče v [nástroje pro vývojáře aplikace Internet Explorer 11 F12] [ EmulatorIE11] (používá se v všechny snímky obrazovky prohlížeč pro mobilní zařízení). Má přednastavení řetězec uživatelského agenta pro Windows Phone 8, Windows Phone 7 a Apple iPad.
* Emulátor prohlížeče v [Google Chrome DevTools][EmulatorChrome]. Obsahuje přednastavení množství zařízení se systémem Android, a také Apple iPhone, Apple iPad a Amazon Kindle ještě efektivněji. Emuluje také touch události.
* [Emulátoru mobilního Opera][EmulatorOpera]

Projekty Visual Studio s C\# zdrojového kódu jsou k dispozici tooaccompany v tomto tématu:

* [Stažení Starter projektu][StarterProject]
* [Dokončení stažení projektu][CompletedProject]

## <a name="bkmk_DeployStarterProject"></a>Nasazení hello starter projektu tooan Azure webové aplikace
1. Stažení aplikace hello konferenční výpis [starter projektu][StarterProject].
2. Potom v Průzkumníku Windows, klikněte pravým tlačítkem na soubor ZIP hello stáhli a zvolte *vlastnosti*.
3. V hello **vlastnosti** dialogovém okně vyberte hello **Odblokovat** tlačítko. (Odblokování brání upozornění zabezpečení, která nastane, když toouse *.zip* souboru, který jste stáhli z webové hello.)
4. Klikněte pravým tlačítkem na soubor ZIP hello a vyberte **Extrahovat vše** dekomprimovat soubor hello. 
5. V sadě Visual Studio otevřete hello *C#\Mvc5Mobile.sln* souboru.
6. V Průzkumníku řešení klikněte pravým tlačítkem na projekt hello a klikněte na tlačítko **publikovat**.
   
   ![][DeployClickPublish]
7. V Publikovat Web, klikněte na **Microsoft Azure App Service**.
   
   ![][DeployClickWebSites]
8. Pokud již jste přihlášeni do Azure, klikněte na tlačítko **přidat účet**.
   
   ![][DeploySignIn]
9. Postupujte podle pokynů toolog hello ke svému účtu Azure.
10. Hello dialogovém okně App Service, by měl nyní může zobrazit můžete jako přihlášení. Klikněte na možnost **Nové**.
    
    ![][DeployNewWebsite]  
11. V hello **název webové aplikace** pole, zadejte předponu aplikace jedinečný název. Váš název plně kvalifikovaný webové aplikace bude  *&lt;předpony >*. azurewebsites.net. Také vyberte nebo zadejte nový název skupiny prostředků v **skupiny prostředků**. Potom klikněte na **nový** toocreate nový plán aplikační služby.
    
    ![][DeploySiteSettings]
12. Nakonfigurujte hello nový plán aplikační služby a klikněte na **OK**. 
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7a.png)
13. Zpět v dialogovém okně vytvořit službu App Service hello, klikněte na tlačítko **vytvořit**.
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7b.png) 
14. Po hello prostředků Azure jsou vytvoření dialogového okna Publikovat Web hello bude vyplněn hello nastavení pro novou aplikaci. Klikněte na **Publikovat**.
    
    ![][DeployPublishSite]
    
    Po dokončení publikování hello úvodní projektu toohello Azure webovou aplikaci Visual Studio otevře prohlížeč pro stolní počítač hello toodisplay hello živou webovou aplikaci.
15. Spusťte emulátor váš prohlížeč pro mobilní zařízení, zkopírujte adresu URL aplikace hello konferenční hello (*<prefix>*. azurewebsites.net) do hello emulátoru a pak klikněte na tlačítko pravém horním a vyberte **Procházet podle značky**. Pokud používáte Internet Explorer 11 jako hello výchozí prohlížeč, bude třeba jen tootype `F12`, pak `Ctrl+8`a poté změňte hello prohlížeče profil příliš**Windows Phone**. Následující obrázek ukazuje hello *AllTags* zobrazení v režimu na výšku (z výběru **Procházet podle značky**).
    
    ![][AllTags]

> [!TIP]
> Zatímco lze ladit aplikace MVC 5 v sadě Visual Studio, můžete publikovat vaší webové aplikace tooAzure znovu tooverify hello živou webovou aplikaci přímo z prohlížeče emulátoru nebo prohlížeč pro mobilní zařízení.
> 
> 

zobrazení Hello je velmi čitelná na mobilním zařízení. Můžete také již zobrazit některé vizuální efekty hello použít rámcem hello Bootstrap šablon stylů CSS.
Klikněte na tlačítko hello **ASP.NET** odkaz.

![][SessionsByTagASP.NET]

zobrazení značek ASP.NET Hello je namontováno přiblížení toohello obrazovce, která nemá Bootstrap pro vás automaticky. Však může zvýšit tento zobrazení toobetter barvy hello prohlížeč pro mobilní zařízení. Například hello **datum** sloupec je obtížné číst. Později v kurzu hello změníte hello *AllTags* zobrazení toomake je mobilní zařízení.

## <a name="bkmk_bootstrap"></a>Framework Bootstrap šablon stylů CSS
V hello MVC 5 nová šablona je integrovanou podporu zavedení. Jste už viděli, jak okamžitě vylepšuje hello různá zobrazení ve vaší aplikaci. Navigační panel hello v horní části hello je například automaticky sbalitelné po menší šířka prohlížeče hello. Na prohlížeč pro stolní počítač hello pokuste se změna velikosti hello okno prohlížeče a v tématu Jak hello navigační panel změní jeho vzhled a chování. Toto je návrh hello přizpůsobivý webu, který je součástí Bootstrap.

toosee jak webová aplikace hello vypadat bez Bootstrap, otevřete *aplikace\_spustit\\BundleConfig.cs* a Odkomentujte hello řádky, které obsahují *bootstrap.js* a *bootstrap.css*. Hello následující kód ukazuje hello poslední dva příkazy hello `RegisterBundles` metoda po změně hello:

     bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
              //"~/Scripts/bootstrap.js",
              "~/Scripts/respond.js"));

    bundles.Add(new StyleBundle("~/Content/css").Include(
              //"~/Content/bootstrap.css",
              "~/Content/site.css"));

Stiskněte klávesu `Ctrl+F5` toorun hello aplikace.

Zkontrolujte, že tento hello sbalitelné navigační panel je teď právě obyčejnou neuspořádaný seznam. Klikněte na tlačítko **Procházet podle značky** znovu, pak klikněte na tlačítko **ASP.NET**.
V zobrazení emulátoru mobilního hello uvidíte nyní, když je už namontováno přiblížení toohello obrazovky a musí ze strany posouvání pořadí toosee hello pravé straně hello tabulky.

![][SessionsByTagASP.NETNoBootstrap]

Vrátit zpět změny a aktualizujte hello prohlížeč pro mobilní zařízení tooverify obnovila zobrazení hello mobilní zařízení.

Bootstrap není konkrétní tooASP.NET MVC 5 a můžete využít výhod těchto funkcí v jakékoli webové aplikace. Ale nyní integrovaná do šablony projektu ASP.NET MVC 5, tak, aby MVC 5 webové aplikace mohou využít výhod Bootstrap ve výchozím nastavení.

Další informace o Bootstrap přejděte toothe [Bootstrap] [ BootstrapSite] lokality.

V další části hello uvidíte jak tooprovide mobilní prohlížeče konkrétní zobrazení.

## <a name="bkmk_overrideviews"></a>Přepsání hello zobrazení, rozložení a částečné zobrazení
Pro mobilní prohlížeče obecně pro jednotlivé mobilního prohlížeče nebo pro jakékoli konkrétní prohlížeč můžete přepsat všechna zobrazení (včetně rozložení a částečné zobrazení). Zobrazit tooprovide konkrétního mobile, můžete zkopírovat soubor zobrazení a přidat *. Mobilní* toohello název souboru. Například toocreate mobilních *Index* zobrazení, můžete zkopírovat *zobrazení\\Domů\\Index.cshtml* k *zobrazení\\Domů\\ Index.Mobile.cshtml*.

V této části vytvoříte soubor specifické mobilní rozložení.

toostart kopie *zobrazení\\sdílené\\\_Layout.cshtml* k *zobrazení\\sdílené\\\_Layout.Mobile.cshtml* . Otevřete  *\_Layout.Mobile.cshtml* a změňte název hello z **MVC5 aplikace** příliš**MVC5 aplikace (mobilní)**.

V každé `Html.ActionLink` volání pro hello navigačním panelu, odeberte "Procházet podle" v každé propojení *ActionLink*. Hello následující kód ukazuje hello Dokončit `<ul class="nav navbar-nav">` značky hello mobilní rozložení souboru.

    <ul class="nav navbar-nav">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
    </ul>

Kopírování hello *zobrazení\\Domů\\AllTags.cshtml* do souboru *zobrazení\\Domů\\AllTags.Mobile.cshtml*. Otevřete nový soubor hello a změňte `<h2>` element z "Značky" příliš "značky (M)":

    <h2>Tags (M)</h2>

Procházet toohello značky stránky pomocí prohlížeč pro stolní počítač a pomocí emulátoru prohlížeč pro mobilní zařízení. Hello prohlížeč pro mobilní zařízení emulátoru ukazuje hello dvě změny (hello titul z  *\_Layout.Mobile.cshtml* a hello titul z *AllTags.Mobile.cshtml*).

![][AllTagsMobile_LayoutMobile]

Naproti tomu nedošlo ke změně zobrazení plochy hello (s názvy z  *\_Layout.cshtml* a *AllTags.cshtml*).

![][AllTagsMobile_LayoutMobileDesktop]

## <a name="bkmk_browserviews"></a>Vytvoření vlastních zobrazení
Kromě toho toomobile-desktop specifické a zobrazení, můžete vytvořit zobrazení pro jednotlivé prohlížeče. Můžete například vytvořit zobrazení, které jsou speciálně určené pro hello iPhone nebo hello prohlížeč systému Android. V této části vytvoříte rozložení pro prohlížeč hello iPhone a na zařízení iPhone verzi hello *AllTags* zobrazení.

Otevřete hello *Global.asax* souboru a přidejte následující kód toohello dolní části hello `Application_Start` metoda.

    DisplayModeProvider.Instance.Modes.Insert(0, new DefaultDisplayMode("iPhone")
    {
        ContextCondition = (context => context.GetOverriddenUserAgent().IndexOf
            ("iPhone", StringComparison.OrdinalIgnoreCase) >= 0)
    });

Tento kód definuje nový režim zobrazení s názvem "iPhone", který bude porovnání jednotlivých příchozích požadavků. Pokud hello příchozí požadavek odpovídá podmínku, kterou jste definovali (to znamená, pokud uživatelský agent hello obsahuje řetězec hello "iPhone"), rozhraní ASP.NET MVC bude hledat zobrazení, jejíž název obsahuje příponu "iPhone".

> [!NOTE]
> Při přidávání mobilní prohlížeče specifické pro režimy zobrazení, například pro iPhone a Android, že tooset hello první argument je příliš`0` toomake (vložení hello horní části seznamu hello), že tento režim specifické pro prohlížeč hello má přednost před hello mobilní šablony (*. Mobile.cshtml). Pokud mobilní šablony hello hello seznamu hello první místo, bude vybrána přes vaše režim určený zobrazení (hello první shodu wins a mobilní šablona hello odpovídá všechny mobilní prohlížeče). 
> 
> 

V kódu hello, klikněte pravým tlačítkem na `DefaultDisplayMode`, zvolte **vyřešit**a potom vyberte `using System.Web.WebPages;`. Tento postup přidá odkaz toothe `System.Web.WebPages` názvů, který je tam, kde `DisplayModeProvider` a `DefaultDisplayMode` typy jsou definované.

![][ResolveDefaultDisplayMode]

Alternativně můžete právě ručně přidat hello následující řádek toothe `using` hello souboru.

    using System.Web.WebPages;

Uložte změny hello. Kopírování *zobrazení\\sdílené\\\_Layout.Mobile.cshtml* do souboru *zobrazení\\sdílené\\\_Layout.iPhone.cshtml*. Otevřete nový soubor hello a potom změňte název hello z `MVC5 Application (Mobile)` k `MVC5 Application (iPhone)`.

Kopírování hello *zobrazení\\Domů\\AllTags.Mobile.cshtml* do souboru *zobrazení\\Domů\\AllTags.iPhone.cshtml*. V novém souboru hello změnit hello `<h2>` element z "značky (M)" příliš "značky (iPhone)".

Spusťte aplikaci hello. Spustit prohlížeč pro mobilní zařízení emulátoru, ujistěte se, jeho uživatelský agent je nastaven příliš "iPhone" a vyhledejte toohello *AllTags* zobrazení. Pokud používáte emulátor hello v Internet Exploreru 11 F12 nástroje pro vývojáře, konfigurace emulace toohello následující:

* Profil Browser = **Windows Phone**
* Řetězec uživatelského agenta = **vlastní**
* Vlastní řetězec = **Apple-iPhone5C1/1001.525**

Hello následující snímek obrazovky ukazuje hello *AllTags* zobrazení vykreslen v emulátoru v Internet Exploreru 11 F12 nástroje pro vývojáře řetězcem hello vlastního uživatelského agenta (jedná se řetězec iPhone 5 C uživatelského agenta).

![][AllTagsIPhone_LayoutIPhone]

V hello prohlížeč pro mobilní zařízení, vyberte hello **Řečníci** odkaz. Protože mobilní zobrazení není (*AllSpeakers.Mobile.cshtml*), zobrazit výchozí mluvčí hello (*AllSpeakers.cshtml*) je vykreslen pomocí zobrazení mobilní rozložení hello ( *\_ Layout.Mobile.cshtml*). Jak je uvedeno níže, název hello **MVC5 aplikace (mobilní)** je definována v  *\_Layout.Mobile.cshtml*.

![][AllSpeakers_LayoutMobile]

Výchozí zobrazení (jiných než mobilních) z vykreslování uvnitř mobilní rozložení můžete zakázat globálně nastavením `RequireConsistentDisplayMode` k `true` v hello *zobrazení\\\_ViewStart.cshtml* souboru, například takto:

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = true;
    }

Když `RequireConsistentDisplayMode` je nastaven příliš`true`, mobilní rozložení hello (*\_Layout.Mobile.cshtml*) se používá pouze pro mobilní zobrazení (tj. Po zobrazení souboru ve formátu hello  ***ViewName** . Mobile.cshtml*). Můžete chtít tooset `RequireConsistentDisplayMode` příliš`true` Pokud vaše mobilní rozložení nebude fungovat dobře u jiných než mobilních zobrazení. Hello – snímek obrazovky níže znázorňuje způsob hello *Řečníci* stránka vykresluje při `RequireConsistentDisplayMode` je nastaven příliš`true` (bez hello řetězec "(mobilní)" hello navigační panel v horní části hello).

![][AllSpeakers_LayoutMobileOverridden]

Konzistentní režim zobrazení v určitém zobrazení můžete zakázat nastavením `RequireConsistentDisplayMode` příliš`false` v souboru zobrazení hello. Následující kód v hello *zobrazení\\Domů\\AllSpeakers.cshtml* souboru nastaví `RequireConsistentDisplayMode` příliš`false`:

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All speakers";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = false;
    }

V této části jsme viděli jak toocreate mobilní rozložení a zobrazení a jak toocreate rozložení a zobrazení pro konkrétní zařízení, jako hello iPhone.
Hlavní výhodou hello Bootstrap CSS framework hello je však přizpůsobivé rozložení, což znamená, že jeden šablony stylů mohou být použity u plochy, phone a tablet prohlížeče toocreate konzistentní vzhled a chování. V další části hello uvidíte, jak tooleverage Bootstrap mobilní zařízení toocreate zobrazení.

## <a name="bkmk_Improvespeakerslist"></a>Zlepšení hello mluvčí seznamu
Protože jste právě viděli, hello *Řečníci* zobrazení je čitelná, ale hello odkazy jsou malé a jsou těžko tootap na mobilním zařízení. V této části, budete mít hello *AllSpeakers* zobrazení mobilní zařízení, která zobrazuje velké, snadno klepněte odkazy a obsahuje tooquickly pole hledání najít mluvčí.

Můžete použít hello Bootstrap [skupiny odkazovaného seznamu] [ linked list group] stylů ke zlepšení hello *Řečníci* zobrazení. V *zobrazení\\Domů\\AllSpeakers.cshtml*, hello obsah souboru nástroje Razor hello nahraďte hello kód níže.

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

Hello `class="list-group"` atribut v hello `<div>` značky platí Bootstrap seznamu stylů a hello `class="input-group-item"` atribut platí Bootstrap seznamu položky stylů tooeach odkaz.

Aktualizujte hello mobilní prohlížeče. Hello aktualizovat zobrazení vypadá takto:

![][AllSpeakersFixed]

Hello Bootstrap [skupiny odkazovaného seznamu] [ linked list group] stylů díky hello celé pole pro každý odkaz můžete kliknout, což je mnohem lepší prostředí. Přepněte zobrazení plochy toothe a sledovat hello konzistentní vzhled a chování.

![][AllSpeakersFixedDesktop]

I když se zlepšila zobrazení hello prohlížeč pro mobilní zařízení, je obtížné přejděte hello dlouhý seznam mluvčí. Bootstrap neposkytuje hledání filtru funkce out-of-the-box, ale můžete jej přidat pár řádků kódu. Nejprve bude přidejte zobrazení toohello pole hledání a pak propojte s hello kódu jazyka JavaScript pro funkci filtru hello. V *zobrazení\\Domů\\AllSpeakers.cshtml*, přidejte \<formuláře\> značky bezprostředně za hello \<h2\> značka, jak je uvedeno níže:

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

Všimněte si, že hello `<form>` a `<input>` značky oba mají toothem Bootstrap styly použít hello. Hello `<span>` element přidá Bootstrap [glyphicon] [ glyphicon] toothe vyhledávacího pole.

V hello *skripty* složky, přidejte do souboru JavaScript s názvem *filter.js*. Otevřete soubor hello a vložte následující kód do ní hello:

    $(function () {

        // reset hello search form when hello page loads
        $("form").each(function () {
            this.reset();
        });

        // wire up hello events toohello <input> element for search/filter
        $("input").bind("keyup change", function () {
            var searchtxt = this.value.toLowerCase();
            var items = $(".list-group-item");

            // show all speakers that begin with hello typed text and hide others
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

Budete také potřebovat tooinclude filter.js v registrovaných sad. Otevřete *aplikace\_spustit\\BundleConfig.cs* a změňte hello první sady. Změnit první `bundles.Add` – příkaz (pro hello **jquery** sady) tooinclude *skripty\\filter.js*, a to takto:

     bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-{version}.js",
                "~/Scripts/filter.js"));

Hello **jquery** sady vykreslením již ve výchozím nastavení hello  *\_rozložení* zobrazení. Později, můžete využít hello stejné JavaScript code tooapply zobrazení seznamu tooother funkce filtru.

Aktualizujte hello mobilní prohlížeče a přejděte toohello *AllSpeakers* zobrazení. Do vyhledávacího pole zadejte "sc". seznam mluvčí Hello by měl být nyní filtrovaný podle tooyour hledaný řetězec.

![][AllSpeakersFixedSearchBySC]

## <a name="bkmk_improvetags"></a>Zlepšení hello seznam značek
Jako hello *Řečníci* zobrazit, hello *značky* zobrazení je čitelná, ale hello odkazy jsou malé a obtížně tootap na mobilním zařízení. Můžete je vyřešit hello *značky* zobrazení hello stejný způsob, jak opravit hello *Řečníci* zobrazit, je-li použít změny kódu hello popsané výše, ale s následující hello `Html.ActionLink` syntaxe využívající metody v  *Zobrazení\\Domů\\AllTags.cshtml*:

    @Html.ActionLink(tag, 
                     "SessionsByTag", 
                     new { tag }, 
                     new { @class = "list-group-item" })

Hello aktualizovat prohlížeč pro stolní počítač vypadá takto:

![][AllTagsFixedDesktop]

A hello aktualizují prohlížeč pro mobilní zařízení vypadá takto: 

![][AllTagsFixed]

> [!NOTE]
> Pokud si všimnete, že hello původní formátování seznamu je stále existuje v hello prohlížeč pro mobilní zařízení a zajímat, co se stalo tooyour dobrý Bootstrap stylů, to je artefakt starší akce toocreate mobilní konkrétní zobrazení. Teď, když používáte hello Bootstrap CSS framework toocreate návrhu přizpůsobivý webu, přejděte head a odebrat tyto specifické mobilní zobrazení a zobrazení konkrétní mobilní rozložení hello. Po dokončení tak hello aktualizovat prohlížeč pro mobilní zařízení se zobrazí hello Bootstrap stylů.
> 
> 

## <a name="bkmk_improvedates"></a>Zlepšení hello seznamu kalendářních dat
Můžete zvýšit hello *data* zobrazit jako vylepšené hello *Řečníci* a *značky* zobrazení, pokud používáte hello změn kódu popsané výše, ale s hello následující `Html.ActionLink` syntaxe využívající metody v *zobrazení\\Domů\\AllDates.cshtml*:

    @Html.ActionLink(date.ToString("ddd, MMM dd, h:mm tt"), 
                     "SessionsByDate", 
                     new { date }, 
                     new { @class = "list-group-item" })

Zobrazí se zobrazení aktualizovat prohlížeč pro mobilní zařízení takto:

![][AllDatesFixed]

Lze dále zvýšit hello *data* zobrazení uspořádání hello hodnoty data a času podle data. To lze provést pomocí hello Bootstrap [panelů] [ panels] styly. Nahraďte obsah hello hello *zobrazení\\Domů\\AllDates.cshtml* soubor s následujícím kódem:

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

Tento kód vytvoří samostatné `<div class="panel panel-primary">` značky pro každý odlišné datum v seznamu hello a hello používá [skupiny odkazovaného seznamu] [ linked list group] pro příslušné odkazy jako předtím. Tady je co hello mobilní prohlížeče vypadá jako při spuštění tohoto kódu:

![][AllDatesFixed2]

Přepínač toohello prohlížeč pro stolní počítač. Znovu si všimněte konzistentní vzhled hello.

![][AllDatesFixed2Desktop]

## <a name="bkmk_improvesessionstable"></a>Zlepšení hello SessionsTable zobrazení
V této části, budete mít hello *SessionsTable* zobrazit další mobilní zařízení. Tuto změnu je rozsáhlejší hello předchozí změny.

V hello prohlížeč pro mobilní zařízení, klepněte na hello **značka** tlačítko a potom zadejte `asp` do vyhledávacího pole.

![][AllTagsFixedSearchByASP]

Klepněte na hello **ASP.NET** odkaz.

![][SessionsTableTagASP.NET]

Jak vidíte, zobrazení hello je naformátován jako tabulku, která je aktuálně navrženou toobe zobrazit v prohlížeči počítače hello. Je však chvíli obtížné tooread na prohlížeč pro mobilní zařízení. toofix tuto, otevřete *zobrazení\\Domů\\SessionsTable.cshtml* a hello obsah souboru nahraďte hello následující kód:

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

Kód Hello provádí 3 věci:

* používá hello Bootstrap [skupiny vlastní odkazovaného seznamu] [ custom linked list group] tooformat hello informací o relaci ve svislém směru, aby tyto informace je čitelná na prohlížeč pro mobilní zařízení (pomocí třídy, jako seznam skupiny--text položky)
* platí hello [mřížky systému] [ grid system] toothe rozložení, takže této relaci hello položky tok v prohlížeč pro stolní počítač hello vodorovně a svisle ve hello prohlížeč pro mobilní zařízení (pomocí třídy hello sloupec md-4)
* hello používá [přizpůsobivý nástroje] [ responsive utilities] ke skrytí hello relace značky v hello prohlížeč pro mobilní zařízení (pomocí třídy skryté xs hello)

Klepnutím na název odkazu toogo toohello příslušné relace. Následující obrázek Hello odráží změny kódu hello.

![][FixedSessionsByTag]

Hello Bootstrap mřížky systém, který můžete použít automaticky uspořádá relací svisle v hello prohlížeč pro mobilní zařízení. Všimněte si také, že nejsou zobrazeny hello značky. Přepínač toohello prohlížeč pro stolní počítač.

![][SessionsTableFixedTagASP.NETDesktop]

V hello prohlížeč pro stolní počítač Všimněte si, že hello značky se teď zobrazují. Zkontrolujte také, že hello Bootstrap mřížky systému, který jste použili uspořádá hello relace položky v dva sloupce. V případě zvětšení v prohlížeči, zobrazí se, že změní uspořádání hello toothree sloupce.

## <a name="bkmk_improvesessionbycode"></a>Zlepšení hello SessionByCode zobrazení
Nakonec budete opravte hello *SessionByCode* zobrazení toomake je mobilní zařízení.

V hello prohlížeč pro mobilní zařízení, klepněte na hello **značka** tlačítko a potom zadejte `asp` do vyhledávacího pole.

![][AllTagsFixedSearchByASP]

Klepněte na hello **ASP.NET** odkaz. Zobrazí se relace značky ASP.NET hello.

![][FixedSessionsByTag]

Zvolte hello **vytváření jedné stránky aplikace s ASP.NET a AngularJS** odkaz.

![][SessionByCode3-644]

zobrazení plochy výchozí Hello je v pořádku, ale můžete zlepšit vzhled hello snadno pomocí některé součásti Bootstrap grafickým uživatelským rozhraním.

Otevřete *zobrazení\\Domů\\SessionByCode.cshtml* a nahraďte obsah hello hello následující kód:

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

nový kód Hello používá Bootstrap panelů styly tooimprove hello mobilní zobrazení. 

Aktualizujte hello mobilní prohlížeče. Hello následující obrázek zobrazuje hello změn kódu, které jste právě vytvořili:

![][SessionByCodeFixed3-644]

## <a name="wrap-up-and-review"></a>Zabalení a zkontrolovat
Tento kurz vám ukázal jak toouse ASP.NET MVC 5 toodevelop mobilní zařízení webových aplikací. Mezi ně patří:

* Nasazení tooan aplikace ASP.NET MVC 5 webové aplikace App Service
* Použít rozložení Bootstrap toocreate přizpůsobivý webové stránky v aplikaci MVC 5
* Přepsání částečné zobrazení, zobrazení a rozložení globálně i pro jednotlivé zobrazení
* Rozložení ovládacích prvků a částečné přepsat pomocí vynucení `RequireConsistentDisplayMode` vlastnost
* Vytvoření zobrazení, které cílí určité prohlížeče, jako je třeba hello iPhone prohlížeč
* Použít Bootstrap stylů v kódu Razor

## <a name="see-also"></a>Viz také
* [9 základní principy návrhu přizpůsobivý webu](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
* [Bootstrap][BootstrapSite]
* [Oficiální Blog zavedení][Official Bootstrap Blog]
* [Twitter Bootstrap kurzu z kurzu republika][Twitter Bootstrap Tutorial from Tutorial Republic]
* [Hello Bootstrap Playground][hello Bootstrap Playground]
* [Osvědčené postupy W3C doporučení mobilní webové aplikace][W3C Recommendation Mobile Web Application Best Practices]
* [W3C Candidate doporučení pro dotazy na média][W3C Candidate Recommendation for media queries]

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- Internal Links -->
[Deploy hello starter project tooan Azure web app]: #bkmk_DeployStarterProject
[Bootstrap CSS Framework]: #bkmk_bootstrap
[Override hello Views, Layouts, and Partial Views]: #bkmk_overrideviews
[Create Browser-Specific Views]:#bkmk_browserviews
[Improve hello Speakers List]: #bkmk_Improvespeakerslist
[Improve hello Tags List]: #bkmk_improvetags
[Improve hello Dates List]: #bkmk_improvedates
[Improve hello SessionsTable View]: #bkmk_improvesessionstable
[Improve hello SessionByCode View]: #bkmk_improvesessionbycode

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
[hello Bootstrap Playground]: http://www.bootply.com/
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

