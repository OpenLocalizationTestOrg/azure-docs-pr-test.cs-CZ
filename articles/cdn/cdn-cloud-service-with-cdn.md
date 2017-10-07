---
title: "cloudové služby Azure s Azure CDN aaaIntegrate | Microsoft Docs"
description: "Zjistěte, jak toodeploy Cloudová služba, poskytuje obsah z integrované koncového bodu Azure CDN"
services: cdn, cloud-services
documentationcenter: .net
author: zhangmanling
manager: erikre
editor: tysonn
ms.assetid: b3c0108f-9ec5-43a8-8fd0-40eafbd32637
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: f20d60b0b5edc133adf06d010633a15f62e2b8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="intro"></a>Cloudové služby integrovat Azure CDN
Cloudové služby můžete integrovat Azure CDN, obsluhující žádný obsah z umístění hello cloudové služby. Tento postup nabízí hello následující výhody:

* Můžete snadno nasadit a aktualizaci bitové kopie, skriptů a šablon v adresářích projektu cloudové služby
* Snadno upgradujte hello balíčků NuGet v rámci cloudové služby, například jQuery nebo Bootstrap verze
* Správa webové aplikace a vaše CDN-poskytovaného obsahu všechny z hello stejné rozhraní sady Visual Studio
* Pracovní postup jednotná nasazení pro webové aplikace a obsah obsluhuje CDN
* ASP.NET sdružování a minimalizace integrovat Azure CDN

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto kurzu se dozvíte, jak:

* [Koncový bod Azure CDN integrovat s cloudovou službou a poskytovat statický obsah na webových stránkách z Azure CDN](#deploy)
* [Konfigurace nastavení mezipaměti pro statický obsah v rámci cloudové služby](#caching)
* [Poskytovat obsah z akce kontroleru prostřednictvím Azure CDN](#controller)
* [Používat spojeno dohromady a minifikovaný obsah prostřednictvím Azure CDN při zachování hello skriptu ladění prostředí v sadě Visual Studio](#bundling)
* [Konfigurace záložního skriptů a šablon stylů CSS při offline Azure CDN](#fallback)

## <a name="what-you-will-build"></a>Co se sestavení
Nasazení webové role služby cloudu pomocí hello výchozí šablony ASP.NET MVC, přidáte kód tooserve obsah z integrované CDN Azure, jako je například bitovou kopii, výsledky akce kontroleru a hello výchozí souborů JavaScript a CSS a také psát kód tooconfigure hello nouzový mechanismus, sady, které jsou poskytovány na hello událostí tohoto hello CDN je offline.

## <a name="what-you-will-need"></a>Co budete potřebovat
V tomto kurzu má hello následující požadavky:

* Aktivní [účtem Microsoft Azure.](/account/)
* Visual Studio 2015 se [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)

> [!NOTE]
> Je třeba účtu Azure toocomplete v tomto kurzu:
> 
> * Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/) -získáte kredity, můžete použít tootry na placené služby Azure a i po jejich použití až můžete hello účet ponechat a používat bezplatné služby Azure, jako jsou weby.
> * Můžete [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.
> 
> 

<a name="deploy"></a>

## <a name="deploy-a-cloud-service"></a>Nasazení cloudové služby
V této části nasazení hello výchozí šablony aplikace ASP.NET MVC v sadě Visual Studio 2015 tooa cloudové služby webovou roli a potom ji integrovat s nástrojem nový koncový bod CDN. Postupujte podle pokynů hello níže:

1. V sadě Visual Studio 2015, vytvořte novou službu Azure cloud z řádku nabídek hello přechodem příliš**soubor > Nový > Projekt > Cloud > Cloudová služba Azure**. Pojmenujte ho a klikněte na tlačítko **OK**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)
2. Vyberte **webovou roli ASP.NET** a klikněte na tlačítko hello  **>**  tlačítko. Klikněte na tlačítko OK.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)
3. Vyberte **MVC** a klikněte na tlačítko **OK**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)
4. Nyní publikování této webové role tooan cloudové služby Azure. Klikněte pravým tlačítkem na projekt cloudové služby hello a vyberte **publikovat**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)
5. Pokud jste ještě nebyly přihlášeni Microsoft Azure, klikněte na tlačítko hello **přidat účet...**  rozevíracího seznamu a klikněte na tlačítko hello **přidat účet** položku nabídky.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)
6. Hello přihlašovací stránce Přihlaste se pomocí účtu Microsoft, které jste použili tooactivate účtu Azure hello.
7. Jakmile jste přihlášení, klikněte na možnost **Další**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)
8. Za předpokladu, že jste nevytvořili účet cloudové služby nebo úložiště, Visual Studio vám pomůže vytvořit obě. V hello **vytvoření cloudové služby a účet** dialogové okno, název typu hello požadované služby a hello vyberte požadovanou oblast. Poté klikněte na možnost **Vytvořit**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)
9. V hello stránky nastavení publikování, ověřte konfiguraci hello a klikněte na **publikovat**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)
   
   > [!NOTE]
   > proces publikování Hello pro cloudové služby trvá příliš dlouho. Hello povolit nasazení webu pro všechny role možnost provádět ladění mnohem rychlejší cloudové služby tím, že poskytuje rychlé (ale dočasné) aktualizace tooyour webové role. Další informace o této možnosti najdete v tématu [publikování cloudové služby pomocí nástroje Azure hello](http://msdn.microsoft.com/library/ff683672.aspx).
   > 
   > 
   
    Když hello **protokoly aktivit Microsoft Azure** ukazuje, že stav publikování je **dokončeno**, vytvoří koncový bod CDN, která je integrovaná s touto cloudovou službou.
   
   > [!WARNING]
   > Pokud po publikování, hello nasazení cloudové služby zobrazí chybové obrazovce, je pravděpodobné, protože používá hello cloudové služby, které jste nasadili [hosta operační systém, který nezahrnuje .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).  Vám může tento problém obejít tím [nasazení .NET 4.5.2 jako úloha spuštění](../cloud-services/cloud-services-dotnet-install-dotnet.md).
   > 
   > 

## <a name="create-a-new-cdn-profile"></a>Vytvoření nového profilu CDN
Profil CDN je kolekcí koncových bodů CDN.  Jednotlivé profily obsahují jeden nebo víc koncových bodů CDN.  Můžete toouse více profilů tooorganize koncové body CDN podle internetové domény, webové aplikace nebo jiných kritérií.

> [!TIP]
> Pokud již máte profil CDN, že chcete toouse pro účely tohoto kurzu, pokračujte příliš[vytvořte nový koncový bod CDN](#create-a-new-cdn-endpoint).
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Vytvoření nového koncového bodu CDN
**toocreate nový koncový bod CDN pro váš účet úložiště**

1. V hello [portálu pro správu Azure](https://portal.azure.com), přejděte tooyour profil CDN.  Je může mít připnutý toohello řídicí panel v předchozím kroku hello.  Pokud není, můžete najdete ho kliknutím na **Procházet**, pak **profilů CDN**, a kliknutím na profil hello máte v plánu tooadd koncový bod.
   
    Otevře se okno profil CDN Hello.
   
    ![Profil CDN][cdn-profile-settings]
2. Klikněte na tlačítko hello **přidat koncový bod** tlačítko.
   
    ![Tlačítko Přidat koncový bod][cdn-new-endpoint-button]
   
    Hello **přidání koncového bodu** se zobrazí okno.
   
    ![Okno Přidání koncového bodu][cdn-add-endpoint]
3. Zadejte **Název** tohoto koncového bodu CDN.  Tento název bude použité tooaccess prostředkům v mezipaměti v doméně hello `<EndpointName>.azureedge.net`.
4. V hello **typ původu** rozevíracího seznamu vyberte *Cloudová služba*.  
5. V hello **název počátečního hostitele** rozevíracího seznamu, vyberte cloudovou službu.
6. Ponechte výchozí hodnoty hello **cesty ke zdroji**, **hlavičky hostitele počátku**, a **port protokolu nebo původu**.  Je nutné zadat aspoň jeden protokol (HTTP nebo HTTPS).
7. Klikněte na tlačítko hello **přidat** tlačítko toocreate hello nový koncový bod.
8. Po vytvoření koncového bodu hello se zobrazí v seznamu koncových bodů pro profil hello. zobrazení seznamu Hello ukazuje, že hello URL toouse tooaccess do mezipaměti obsah, a také doménu původu hello.
   
    ![Koncový bod CDN][cdn-endpoint-success]
   
   > [!NOTE]
   > Hello koncový bod nebude hned dostupný pro použití.  To může trvat až minut too90 hello registrace toopropagate přes síť CDN hello. Uživatelů, kteří chtějí název domény CDN hello toouse okamžitě obdržet stavový kód 404, dokud nebude k dispozici prostřednictvím hello CDN hello obsah.
   > 
   > 

## <a name="test-hello-cdn-endpoint"></a>Test hello koncový bod CDN
Pokud je stav publikování hello **dokončeno**, otevřete okno prohlížeče a přejděte příliš**http://<cdnName>*.azureedge.net/Content/bootstrap.css**. V části Moje nastavení je tato adresa URL:

    http://camservice.azureedge.net/Content/bootstrap.css

Která odpovídá toohello následující původní adresu URL na koncový bod CDN hello:

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

Když přejdete příliš**http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, v závislosti na prohlížeči, bude výzvami toodownload nebo otevřete hello bootstrap.css, pocházejí z vaší publikované webové aplikace.

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

Stejně tak přístup k jakékoli veřejně přístupné URL u  **http://*&lt;serviceName >*.cloudapp.net/** přímo z vašeho koncového bodu CDN. Například:

* Soubor .js z cesty/Script hello
* Všechny soubory obsahu z hello/Content cesta
* Kontroler nebo akce
* Pokud řetězec dotazu hello je povoleno na koncový bod CDN, libovolná adresa URL s řetězci dotazů

Ve skutečnosti s hello výše konfigurace, může hostovat hello celý cloudové služby z  **http://*&lt;cdnName >*.azureedge.net/**. Pokud I přejděte příliš**http://camservice.azureedge.net/ ** dostat výsledek akce hello z domovské nebo Index.

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

To neznamená, ale, že je vždy vhodné tooserve celý cloudové služby prostřednictvím Azure CDN. 

CDN s optimalizací statické doručení není zrychlení nutně doručování dynamické prostředky, které nejsou určeny toobe do mezipaměti, nebo jsou aktualizovány velmi často, protože hello CDN musí novou verzi hello asset načítat z hello zdrojový server velmi často. V tomto scénáři můžete povolit [dynamické akcelerace lokality](cdn-dynamic-site-acceleration.md) optimalizace (DSA) na váš koncový bod CDN, která využívá různé techniky toospeed až doručení neurčené dynamické prostředků. 

Pokud máte síť se smíšenými statické a dynamické obsah, můžete tooserve statický obsah z CDN s typem statické optimalizace (například doručení obecné webové) a tooserve dynamický obsah přímo ze serveru původu hello nebo prostřednictvím název CDN koncový bod s optimalizací DSA zapnutá případ od případu. toothat end jste už viděli, jak jednotlivé obsah tooaccess souborů z koncového bodu CDN hello. I vám ukáže, jak tooserve konkrétní řadič akce prostřednictvím konkrétní koncový bod CDN v sloužit obsahu z akce kontroleru prostřednictvím Azure CDN.

Hello alternativou je toodetermine, který obsahu tooserve z Azure CDN v případech v rámci cloudové služby. toothat end jste už viděli, jak jednotlivé obsah tooaccess souborů z koncového bodu CDN hello. Můžu si ukážeme, jak tooserve konkrétní řadič akce prostřednictvím hello koncový bod CDN v [poskytovat obsah z akce kontroleru prostřednictvím Azure CDN](#controller).

<a name="caching"></a>

## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>Konfigurace možností ukládání do mezipaměti pro statické soubory v rámci cloudové služby
Díky integraci Azure CDN v cloudové službě můžete určit, jak se mají statického obsahu toobe uložené v mezipaměti v koncový bod CDN hello. toodo tuto, otevřete *Web.config* z vaší webové role projektu (např. WebRole1) a přidejte `<staticContent>` element příliš`<system.webServer>`. Hello XML níže nakonfiguruje hello mezipaměti tooexpire v 3 dny.  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

Jakmile to uděláte, bude sledovat všechny statické soubory v rámci cloudové služby hello stejné pravidlo v mezipaměti CDN. K podrobnějšímu řízení nastavení mezipaměti, přidejte *Web.config* soubor do složky a přidat nastavení existuje. Například přidejte *Web.config* souboru toohello *\Content* složky a nahradit text hello obsah s hello následující XML:

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

Toto nastavení způsobí, že všechny statické soubory z hello *\Content* toobe Složka uložená v mezipaměti pro 15 dnů.

Další informace o tom, tooconfigure hello `<clientCache>` elementu, najdete v části [mezipaměti klienta &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

V [poskytovat obsah z akce kontroleru prostřednictvím Azure CDN](#controller), I také ukazují, jak můžete nakonfigurovat nastavení mezipaměti pro výsledky akce kontroleru v hello CDN mezipaměti.

<a name="controller"></a>

## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>Poskytovat obsah z akce kontroleru prostřednictvím Azure CDN
Při integraci s Azure CDN webové role cloudové služby, je poměrně snadné tooserve obsah z akce kontroleru prostřednictvím hello Azure CDN. Než obsluhující vaše cloudové služby přímo přes Azure CDN (ukázán výše), [Maarten Balliauw](https://twitter.com/maartenballiauw) ukazuje, jak toodo její zábavné MemeGenerator řadiče v [sníží se latence na webu hello s hello Azure CDN ](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN). I bude jednoduše ho znovu vyvolali sem.

Předpokládejme, že v rámci cloudové služby na základě malí bitové kopie Karel Norris memes toogenerate chcete (fotografii podle [Jakub Light](http://www.flickr.com/photos/alan-light/218493788/)) podobné výjimky:

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

Máte jednoduchou `Index` akce, která umožňuje zákazníkům hello toospecify hello jejich hello obrázku, pak generuje hello meme po vystavení toohello akce. Vzhledem k tomu, že je Karel Norris, kterou byste očekávali tuto stránku toobecome velkým oblíbených globálně. Toto je dobrým příkladem obsluhovat polovičním dynamický obsah s Azure CDN.

Postupujte podle kroků hello výše toosetup tato akce kontroleru:

1. V hello *\Controllers* složky, vytvořte nový soubor .cs s názvem *MemeGeneratorController.cs* a nahraďte hello obsahu s hello následující kód. Být jisti tooreplace hello zvýrazněná část s název CDN.  
   
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;
   
        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();
   
                public ActionResult Index()
                {
                    return View();
                }
   
                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }
   
                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }
   
                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }
   
                    if (Debugger.IsAttached) // Preserve hello debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }
   
                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);
   
                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }
   
                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }
   
                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);
   
                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }
   
                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }
2. Klikněte pravým tlačítkem na výchozí hello `Index()` akce a vyberte **přidat zobrazení**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)
3. Přijměte hello následující nastavení a klikněte na **přidat**.
   
   ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)
4. Otevřete nový hello *Views\MemeGenerator\Index.cshtml* a nahraďte obsah hello hello následující jednoduché HTML pro odesílání jejich hello:
   
        <h2>Meme Generator</h2>
   
        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>
5. Znovu publikovat hello cloudové služby a přejděte příliš**http://*&lt;serviceName >*.cloudapp.net/MemeGenerator/Index** v prohlížeči.

Po odeslání formuláře hodnoty hello příliš`/MemeGenerator/Index`, hello `Index_Post` metoda akce vrací odkaz toohello `Show` metoda akce s příslušnými vstupní identifikátor hello. Při kliknutí na odkaz hello nedostanete hello následující kód:  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve hello debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

Pokud je připojena vaše místní ladicí program, obdržíte hello regulární ladění zkušenosti s místní přesměrování. Pokud je spuštěn v hello cloudové služby, se přesměruje na:

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

Která odpovídá toohello původní adresu URL na koncový bod CDN následující:

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


Pak můžete použít hello `OutputCacheAttribute` atribut hello `Generate` metoda toospecify jak by měla být v mezipaměti hello výsledek akce, který bude respektovat Azure CDN. Následující kód Hello zadejte vypršení platnosti mezipaměti 1 hodina (3 600 sekund).

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

Podobně můžete může sloužit obsah z jakékoli akce kontroleru v rámci cloudové služby prostřednictvím Azure CDN, s možností ukládání do mezipaměti hello potřeby.

V další části hello I vám ukáže, jak tooserve hello dodávat a minifikovaný skriptů a šablon stylů CSS pomocí Azure CDN.

<a name="bundling"></a>

## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>ASP.NET sdružování a minimalizace integrovat Azure CDN
Skripty a šablon stylů CSS předlohy se styly změňte zřídka a prvotní kandidáty pro hello mezipaměti Azure CDN. Slouží hello celou webovou roli prostřednictvím Azure CDN je nejjednodušší způsob, jak toointegrate hello sdružování a minimalizace s Azure CDN. Ale tak, jak má toodo nemusí být to, I vám ukáže, jak toodo je při zachování hello žádoucí vývojářský činnost ASP.NET sdružování a minimalizace, například:

* Skvělé ladění režimu prostředí
* Zjednodušené nasazení
* Okamžitá aktualizace tooclients pro upgrade verze skriptu nebo šablon stylů CSS
* Nouzový mechanismus, když se nezdaří koncový bod CDN
* Minimalizovat úpravy kódu

V hello **WebRole1** projekt, který jste vytvořili v [koncový bod Azure CDN integrovat svůj web Azure a poskytovat statický obsah na webových stránkách z Azure CDN](#deploy), otevřete *App_Start\ BundleConfig.cs* a podívejte se na hello `bundles.Add()` volání metody.

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

nejprve Hello `bundles.Add()` příkaz přidá sady skriptu na virtuální adresář hello `~/bundles/jquery`. Potom otevřete *Views\Shared\_Layout.cshtml* toosee vykreslení značky sady script hello. Měli byste mít možnost toofind hello následující řádek kódu Razor:

    @Scripts.Render("~/bundles/jquery")

Při spuštění tohoto kódu Razor v roli hello webů Azure, se budou vykreslovat `<script>` značky pro hello skript sady podobné toohello následující:

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

Ale když ji spustí v sadě Visual Studio zadáním `F5`, se budou vykreslovat každý soubor skriptu v kompletu hello jednotlivě (v případě hello výše pouze jeden soubor skriptu nachází v sady hello):

    <script src="/Scripts/jquery-1.10.2.js"></script>

To vám umožní kódu jazyka JavaScript hello toodebug ve vašem vývojovém prostředí při snížení souběžných klientských připojení (sdružování) a vylepšuje soubor stáhnout výkonu (minimalizace) v provozním prostředí. Je skvělé funkce toopreserve díky integraci Azure CDN. Navíc vzhledem k tomu, že sady hello vykresluje již obsahuje řetězec automaticky generované verze, chcete tooreplicate, který funkce proto hello při každé aktualizaci vaší verzí jQuery prostřednictvím balíčku NuGet, mohou být aktualizovány na straně klienta hello co nejrychleji možné.

Postupujte podle kroků hello toointegration ASP.NET sdružování a minimalizace s koncový bod CDN.

1. Zpět v *App_Start\BundleConfig.cs*, upravte hello `bundles.Add()` jiné metody toouse [sady konstruktor](http://msdn.microsoft.com/library/jj646464.aspx), ten, který určuje adresu CDN. toodo se nahradit hello `RegisterBundles` definici metody s hello následující kód:  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));
   
            // Use hello development version of Modernizr toodevelop with and learn from. Then, when you're
            // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    Být jisti tooreplace `<yourCDNName>` s názvem hello Azure CDN.
   
    Uveďte stručný nastavujete `bundles.UseCdn = true` a přidat pečlivě vytvořenou sadu tooeach CDN URL. Například hello první konstruktor v kódu hello:
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
   
    je hello totéž jako:
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))
   
    Tento konstruktor informuje ASP.NET sdružování a minimalizace soubory jednotlivých skriptu toorender při ladit místně, ale použití hello zadán CDN adresu tooaccess hello skriptu nejistá. Pamatujte však dvě důležité charakteristiky s Tento pečlivě vytvořené CDN adresy URL:
   
   * Hello původ pro tuto adresu URL CDN `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, který je ve skutečnosti hello virtuální adresář sady hello skript v rámci cloudové služby.
   * Vzhledem k tomu, že používáte konstruktor CDN, hello značky script CDN pro sadu hello už obsahuje hello automaticky generované řetězec verze ve hello vykresluje adresy URL. Řetězec verze jedinečný musíte vygenerovat ručně pokaždé, když hello skript sady je upravený tooforce mezipaměti neproběhly v Azure CDN. Na hello stejný čas, musí zůstat po nasazení sady hello konstantní prostřednictvím hello životnosti hello nasazení toomaximize přístupů do mezipaměti v Azure CDN tento řetězec jedinečný verze.
   * řetězec dotazu v Hello = < W.X.Y.Z > si z *Properties\AssemblyInfo.cs* v projektu webové role. Může mít nasazení pracovního postupu, který zahrnuje zvyšování verze sestavení hello pokaždé, když publikujete tooAzure. Nebo můžete upravit pouze *Properties\AssemblyInfo.cs* ve vašem projektu tooautomatically přírůstek hello verze řetězci pokaždé, když vytvoříte, pomocí hello zástupný znak ' *'. Například:
     
        [sestavení: AssemblyVersion("1.0.0.*")]
     
     Další strategie toostreamline generování jedinečného řetězce dobu životnosti hello nasazení bude fungovat v tomto poli.
2. Znovu publikujte hello cloudové služby a přístup hello domovskou stránku.
3. Zobrazení hello kód HTML pro stránku hello. Musí být schopný toosee hello CDN URL vykresleno, řetězcem jedinečný verze pokaždé, když je znovu publikovat změny tooyour cloudové služby. Například:  
   
        ...
   
        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>
   
        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>
   
        ...
   
        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>
   
        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>
   
        ...
4. V sadě Visual Studio, ladění hello cloudové služby v sadě Visual Studio zadáním `F5`.,
5. Zobrazení hello kód HTML pro stránku hello. Zobrazí se stále každý soubor skriptu jednotlivě vykreslen tak, že máte konzistentní ladění prostředí v sadě Visual Studio.  
   
        ...
   
            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>
   
            <script src="/Scripts/modernizr-2.6.2.js"></script>
   
        ...
   
            <script src="/Scripts/jquery-1.10.2.js"></script>
   
            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>
   
        ...   

<a name="fallback"></a>

## <a name="fallback-mechanism-for-cdn-urls"></a>Nouzový mechanismus adresy URL CDN
Pokud z nějakého důvodu selže koncový bod Azure CDN, budete chtít vaší webové stránky toobe inteligentní dostatek tooaccess váš počátek webový server jako záložní volbu hello načítání JavaScript nebo Bootstrap. Je dostatečně závažné toolose obrázků na váš web z důvodu nedostupnosti tooCDN, ale mnohem závažnější funkce zásadní stránky toolose poskytované službou skriptů a šablon.

Hello [sady](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) třída obsahuje vlastnost s názvem [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) , která umožní tooconfigure hello nouzový mechanismus selhání CDN. toouse tuto vlastnost, postupujte podle následujících kroků hello:

1. Otevřete v projektu webové role *App_Start\BundleConfig.cs*, které jste přidali adresu URL CDN v každé [sady konstruktor](http://msdn.microsoft.com/library/jj646464.aspx)a proveďte následující hello zvýrazněná změny tooadd nouzový mechanismus, který toohello výchozí sady:  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));
   
            // Use hello development version of Modernizr toodevelop with and learn from. Then, when you&#39;re
            // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                  "~/Scripts/bootstrap.js",
                                  "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    Když `CdnFallbackExpression` je hodnotou not null, skript je vloženy do hello HTML tootest zda byla úspěšně zavedena hello sady a pokud ne, přístup k sadě hello přímo z hello původu webového serveru. Tato vlastnost musí toobe sady tooa JavaScript výraz, který kontroluje, zda hello příslušné sady CDN je načtena správně. výraz Hello potřeby tootest každého svazku se liší podle toohello obsah. Pro výchozí sady hello výše:
   
   * `window.jquery`je definována v jquery-{version} .js
   * `$.validator`je definována v jquery.validate.js
   * `window.Modernizr`je definována v modernizer-{version} .js
   * `$.fn.modal`je definována v bootstrap.js
     
     Možná jste si všimli I nenastavili CdnFallbackExpression pro hello `~/Cointent/css` sady. Důvodem je, že je nyní [chyb v System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) , vloží `<script>` značky pro hello záložní CSS místo hello očekává `<link>` značky.
     
     Existuje, ale velká [záložní styl sady](https://github.com/EmberConsultingGroup/StyleBundleFallback) nabízené [členskými konzultace ohledně skupiny](https://github.com/EmberConsultingGroup).
2. alternativní řešení hello toouse pro šablon stylů CSS, vytvořte nový soubor .cs v projektu webové role *App_Start* složku s názvem *StyleBundleExtensions.cs*a nahraďte jeho obsah hello [kódu z GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).
3. V *App_Start\StyleFundleExtensions.cs*, přejmenujte hello obor názvů tooyour webové role název (například **WebRole1**).
4. Přejděte zpět příliš`App_Start\BundleConfig.cs` a poslední změna hello `bundles.Add` příkaz s hello následující zvýrazněný kód:  
   
        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));
   
    Tato nová metoda rozšíření používá hello stejné nápad tooinject skript v modelu DOM hello toocheck hello HTML pro hello odpovídající název třídy, název pravidla a pravidla hodnota definovaná v sady hello šablon stylů CSS a spadá back toohello počátek webový server Pokud selže toofind hello shodu.
5. Publikujte znovu hello cloudové služby a přístup hello domovskou stránku.
6. Zobrazení hello kód HTML pro stránku hello. Byste měli najít vloženého skripty podobné toohello následující:    
   
        ...
   
        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>
   
        ...
   
            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>
   
        ...

    Vložený skript pro sady šablon stylů CSS hello stále obsahuje nesprávně pracujících zbývajících hello z hello `CdnFallbackExpression` vlastnost hello řádku:

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    Ale protože hello první část hello || výraz vždy vrátí hodnotu PRAVDA (na řádku hello přímo vyšší), bude funkce document.write() hello nebude nikdy spuštěn.

## <a name="more-information"></a>Další informace
* [Přehled hello Azure Content Delivery Network (CDN)](http://msdn.microsoft.com/library/azure/ff919703.aspx)
* [Používání Azure CDN](cdn-create-new-endpoint.md)
* [ASP.NET sdružování a minimalizace](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)

[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
