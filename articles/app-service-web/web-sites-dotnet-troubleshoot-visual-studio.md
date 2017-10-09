---
title: "aaaTroubleshoot webové aplikace ve službě Azure App Service pomocí sady Visual Studio"
description: "Zjistěte, jak tootroubleshoot webové aplikace Azure pomocí vzdálené ladění, trasování a protokolování nástrojů integrovaných ve tooVisual Studio 2013."
services: app-service
documentationcenter: .net
author: tdykstra
manager: erikre
editor: 
ms.assetid: def8e481-7803-4371-aa55-64025d116c97
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/29/2016
ms.author: rachelap
ms.openlocfilehash: 8e3a8a58293f2ebcdc131fbf2534f8ff99b26730
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-web-app-in-azure-app-service-using-visual-studio"></a>Řešení potíží s webovou aplikaci v Azure App Service pomocí sady Visual Studio
## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak toouse nástroje Visual Studio, které pomáhají ladit webové aplikace ve [služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714), protože běží na [režimu ladění](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) vzdáleně nebo zobrazením protokoly aplikací a protokoly webového serveru.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

Naučíte se:

* Funkcích správy, které Azure webové aplikace jsou k dispozici v sadě Visual Studio.
* Jak toouse Visual Studio vzdálené zobrazení toomake rychlé změny ve vzdálené webové aplikaci.
* Režim ladění toorun vzdáleně při projektu jak běží v Azure, pro webovou aplikaci a pro webovou úlohu.
* Jak aplikace toocreate protokoly trasování a zobrazit je při jejich vytváření aplikace hello.
* Jak tooview protokolů webového serveru, včetně podrobné chybové zprávy a trasování neúspěšných žádostí.
* Jak toosend diagnostiky protokoly tooan účet úložiště Azure a zobrazit je.

Pokud máte Visual Studio Ultimate, můžete také použít [IntelliTrace](http://msdn.microsoft.com/library/vstudio/dd264915.aspx) pro ladění. V tomto kurzu nepopisuje IntelliTrace.

## <a name="prerequisites"></a>Požadavky
V tomto kurzu pracuje s hello vývojového prostředí, webový projekt a Azure webové aplikace, které jste nastavili v [Začínáme s Azure a ASP.NET][GetStarted]. Pro hello části webové úlohy, budete potřebovat hello aplikace, které vytvoříte v [Začínáme s Azure WebJobs SDK hello][GetStartedWJ].

Kód Hello vzorků, v tomto kurzu jsou pro MVC C# webovou aplikaci, ale hello postupy pro řešení potíží se hello stejné pro aplikace Visual Basic a webových formulářů.

Hello kurz předpokládá, že používáte Visual Studio 2015 nebo 2013. Pokud používáte Visual Studio 2013, hello WebJobs funkce vyžadují [aktualizace 4](http://go.microsoft.com/fwlink/?LinkID=510314) nebo novější.

protokoly streamování Hello funkce pracuje pouze pro aplikace, které cílí na rozhraní .NET Framework 4 nebo novější.

## <a name="sitemanagement"></a>Správa a konfigurace webové aplikace
Visual Studio poskytuje přístup tooa podmnožinu funkcí pro správu hello webové aplikace a nastavení konfigurace, které jsou k dispozici v hello [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715). V této části se zobrazí, co je dostupné pomocí **Průzkumníka serveru**. vyzkoušet toosee hello nejnovější Azure funkcí pro integraci, **Průzkumník cloudu** také. Systému windows můžete otevřít z hello **zobrazení** nabídky.

1. Pokud už nejste přihlášení tooAzure v sadě Visual Studio, klikněte na tlačítko hello **připojit tooAzure** tlačítka na **Průzkumníka serveru**.

    Alternativou je tooinstall certifikát správy, že umožňuje přístup k účtu tooyour. Pokud si zvolíte tooinstall certifikát, klikněte pravým tlačítkem na hello **Azure** uzlu v **Průzkumníka serveru**a potom klikněte na **spravovat a odběry filtru** v kontextové nabídce hello. V hello **spravovat předplatná Azure** dialogovém okně klikněte na hello **certifikáty** a pak klikněte **Import**. Postupujte podle pokynů toodownload hello a pak importovat soubor odběru (také nazývané *.publishsettings* souboru) pro účet Azure.

   > [!NOTE]
   > Pokud chcete stáhnout soubor předplatné, uložit jej tooa složky mimo adresáře zdrojový kód (například ve složce stahování hello) a pak odstraňte po dokončení importu hello. Uživatel se zlými úmysly, který získá soubor odběru toohello přístup můžete upravit, vytvoření a odstranění služeb Azure.
   >
   >

    Další informace o připojení tooAzure prostředky ze sady Visual Studio najdete v tématu [Správa účtů, odběry a správu role](http://go.microsoft.com/fwlink/?LinkId=324796#BKMK_AccountVCert).
2. V **Průzkumníka serveru**, rozbalte položku **Azure** a rozbalte **služby App Service**.
3. Rozbalte skupinu prostředků hello, která zahrnuje hello webovou aplikaci, kterou jste vytvořili v [Začínáme s Azure a ASP.NET][GetStarted]a potom klikněte pravým tlačítkem na uzel hello webové aplikace a klikněte na tlačítko **nastavení zobrazení**.

    ![Nastavení zobrazení v Průzkumníku serveru](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewsettings.png)

    Hello **webové aplikace Azure** se zobrazí karta a zobrazí se, že hello webové aplikace správu a konfiguraci úloh, které jsou k dispozici v sadě Visual Studio.

    ![Azure okna webové aplikace](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configtab.png)

    V tomto kurzu budete používat hello protokolování a trasování rozevírací seznamy. Budete používat vzdálené ladění ale budete používat jinou metodu tooenable ho.

    Informace o nastavení aplikace a připojovacích řetězců oknech hello v tomto okně najdete v tématu [Azure Web Apps: fungování řetězců aplikace a připojovacích řetězců](http://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

    Pokud chcete tooperform úlohu správy webové aplikace, která není možné v tomto okně, klikněte na tlačítko **otevřít na portálu pro správu** tooopen toohello okno prohlížeče portálu Azure.

## <a name="remoteview"></a>Přístup k webové aplikaci souborům v Průzkumníku serveru
Obvykle nasazení webového projektu s hello `customErrors` příznak v hello sadou souborů Web.config příliš`On` nebo `RemoteOnly`, tzn., neobdržíte užitečné chybová zpráva, pokud něco pokazí. Pro mnoho chyb všechny, které máte je stránka jako jednu z těch, které jsou následující hello.

**Chyba serveru v aplikaci '/':**

![Neužitečné chybová stránka](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror.png)

**Došlo k chybě:**

![Neužitečné chybová stránka](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror1.png)

**Hello webu nelze zobrazit stránku hello**

![Neužitečné chybová stránka](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror2.png)

Často hello nejjednodušší způsob, jak toofind hello příčinou chyby hello je tooenable podrobné chybové zprávy, které hello první hello předchozích snímcích obrazovky vysvětluje, jak toodo. Který vyžaduje že změnu hello nasazeném souboru Web.config. Může upravit hello *Web.config* v hello projekt a projekt znovu ho zaveďte hello soubor, nebo vytvořte [transformaci Web.config](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) a nasadit sestavení ladicí verze, ale existuje rychlejší způsob: v **řešení Průzkumník** přímo můžete zobrazit a upravit soubory v hello vzdálené webové aplikace pomocí hello *vzdálené zobrazení* funkce.

1. V **Průzkumníka serveru**, rozbalte položku **Azure**, rozbalte položku **služby App Service**, rozbalte skupinu prostředků hello, umístěné ve vaší webové aplikace a pak rozbalte uzel hello pro vaši webovou aplikaci.

    Zobrazí uzly, které vám poskytnou soubory obsahu přístup toohello webové aplikace a soubory protokolu.
2. Rozbalte hello **soubory** uzel a dvakrát klikněte na hello *Web.config* souboru.

    ![Otevřete soubor Web.config](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfig.png)

    Visual Studio otevře soubor Web.config hello z hello vzdálené webové aplikace a zobrazuje [vzdálený] další název souboru toohello v záhlaví hello.
3. Přidejte následující řádek toohello hello `system.web` element:

    `<customErrors mode="Off"></customErrors>`

    ![Upravit soubor Web.config](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfigedit.png)
4. Aktualizovat hello prohlížeč, který se zobrazuje hello neužitečné chybovou zprávu, a teď se podrobná chybová zpráva, jako je například hello následující ukázka:

    ![Podrobná chybová zpráva](./media/web-sites-dotnet-troubleshoot-visual-studio/detailederror.png)

    (Chyba hello zobrazí byl vytvořen přidáním hello řádku zobrazené červeně příliš*Views\Home\Index.cshtml*.)

Úpravy souboru Web.config hello je pouze jeden příklad scénáře, ve které hello možnost tooread a úpravy souborů na vaší webové aplikace Azure Ujistěte se, při odstraňování problémů.

## <a name="remotedebug"></a>Vzdálené ladění webových aplikací
Pokud hello Podrobná chybová zpráva neposkytuje dostatek informací, a nelze znovu vytvořit hello chyba místně, je jiný způsob tootroubleshoot vzdáleně toorun v režimu ladění. Můžete nastavit zarážky, pracovat přímo s paměti, krok prostřednictvím kódu a dokonce změnit hello kódové cestě.

Vzdálené ladění nefunguje, pokud je v edice Express sady Visual Studio.

V této části ukazuje, jak toodebug vzdáleně pomocí hello projektu, můžete vytvořit v [Začínáme s Azure a ASP.NET][GetStarted].

1. Otevřete hello webový projekt, který jste vytvořili v [Začínáme s Azure a ASP.NET][GetStarted].
2. Otevřete *Controllers\HomeController.cs*.
3. Odstranit hello `About()` metoda a vložení hello následující kód na příslušné místo.

        public ActionResult About()
        {
            string currentTime = DateTime.Now.ToLongTimeString();
            ViewBag.Message = "hello current time is " + currentTime;
            return View();
        }
4. [Nastavit zarážky](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) na hello `ViewBag.Message` řádku.
5. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a klikněte na tlačítko **publikovat**.
6. V hello **profil** rozevíracího seznamu, vyberte hello stejný profil této jste použili v [Začínáme s Azure a ASP.NET][GetStarted].
7. Klikněte na tlačítko hello **nastavení** kartě a změňte **konfigurace** příliš**ladění**a potom klikněte na **publikovat**.

    ![Publikování v režimu ladění](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-publishdebug.png)
8. Po nasazení otevře toohello adresy URL Azure vaší webové aplikace, prohlížeč zavřít hello dokončení a prohlížeč.
9. V **Průzkumníka serveru**, klikněte pravým tlačítkem na vaší webové aplikace a pak klikněte na tlačítko **připojit ladicí program**.

    ![Připojit ladicí program](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-attachdebugger.png)

    Hello prohlížeči se automaticky otevře domovskou stránku tooyour běžící v Azure. Můžete mít toowait 20 sekund a proto při Azure nastaví server hello pro ladění. Tato prodleva pouze se stane hello prvním spuštění v režimu ladění na webovou aplikaci. Následné čas v rámci hello další 48 hodin, při prvním spuštění ladění znovu existuje nebude zpoždění.

    **Poznámka:** Pokud máte jakékoli potíže při spuštění ladicího programu hello, zkuste toodo ho pomocí **Průzkumník cloudu** místo **Průzkumníka serveru**.
10. Klikněte na tlačítko **o** v nabídce hello.

     Visual Studio zastaví na hello zarážek a hello kód běží v Azure, není v místním počítači.
11. Pozastavte ukazatel myši nad hello `currentTime` hodnota proměnné toosee hello času.

     ![Zobrazení proměnné v režimu ladění běžící v Azure](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugviewinwa.png)

     Hello času, které vidíte je čas hello Azure serveru, který může být v jiném časovém pásmu než místního počítače.
12. Zadejte novou hodnotu pro hello `currentTime` proměnné, jako je například "Teď spuštění v Azure".
13. Stisknutím klávesy F5 toocontinue systémem.

     Hello o stránce běžící v Azure zobrazí hello nová hodnota, kterou jste zadali do proměnné aktualnicas hello.

     ![O stránce s novou hodnotou](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugchangeinwa.png)

## <a name="remotedebugwj"></a>Vzdálené ladění webové úlohy
V této části ukazuje, jak toodebug vzdáleně pomocí hello projektu a webové aplikace můžete vytvořit v [začít pracovat s hello Azure WebJobs SDK](websites-dotnet-webjobs-sdk.md).

Funkce Hello uvedené v této části jsou k dispozici pouze v sadě Visual Studio 2013 s aktualizací 4 nebo novější.

Vzdálené ladění pracuje pouze s nepřetržité webové úlohy. Naplánované i na vyžádání webové úlohy se nepodporuje ladění.

1. Otevřete hello webový projekt, který jste vytvořili v [Začínáme s Azure WebJobs SDK hello][GetStartedWJ].
2. V projektu ContosoAdsWebJob hello otevřete *Functions.cs*.
3. [Nastavit zarážky](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) na první příkaz hello v hello `GnerateThumbnail` metoda.

    ![Sada zarážek](./media/web-sites-dotnet-troubleshoot-visual-studio/wjbreakpoint.png)
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt webové hello (ne projektu úlohy WebJob hello) a klikněte na tlačítko **publikovat**.
5. V hello **profil** rozevíracího seznamu, vyberte hello stejný profil této jste použili v [začít pracovat s hello Azure WebJobs SDK](websites-dotnet-webjobs-sdk.md).
6. Klikněte na tlačítko hello **nastavení** kartě a změňte **konfigurace** příliš**ladění**a potom klikněte na **publikovat**.

    Visual Studio nasadí hello web a webové úlohy projekty a prohlížeč otevře toohello adresy URL Azure vaší webové aplikace.
7. V **Průzkumníka serveru** rozbalte **Azure > aplikační služby > vaší skupiny prostředků > vaší webové aplikace > webové úlohy > souvislý**a potom klikněte pravým tlačítkem na **ContosoAdsWebJob**.
8. Klikněte na tlačítko **připojit ladicí program**.

    ![Připojit ladicí program](./media/web-sites-dotnet-troubleshoot-visual-studio/wjattach.png)

    Hello prohlížeči se automaticky otevře domovskou stránku tooyour běžící v Azure. Můžete mít toowait 20 sekund a proto při Azure nastaví server hello pro ladění. Tato prodleva pouze se stane hello prvním spuštění v režimu ladění na webovou aplikaci. Hello příštím připojit ladicí program hello existuje nebudou ke zpoždění, pokud je to provést v rámci 48 hodin.
9. Ve webovém prohlížeči hello je otevřen toohello Contoso Ads domovskou stránku vytvořte nové reklamy.

    Vytváření ad způsobí, že fronty zpráv toobe vytvořili, který bude zachyceny pomocí hello webové úlohy a zpracovat. Když hello WebJobs SDK volá hello funkce tooprocess hello fronty zpráv, se setkají hello kódu vaší zarážce.
10. Když ladicího programu hello dělí na vaší zarážce, můžete prozkoumat a měnit hodnoty proměnných během hello programu hello cloudu. V hello ladicí hello následující obrázek ukazuje hello obsah hello blobInfo objektu, který byl předán toohello GenerateThumbnail metoda.

     ![objekt blobInfo v ladicím programu](./media/web-sites-dotnet-troubleshoot-visual-studio/blobinfo.png)
11. Stisknutím klávesy F5 toocontinue systémem.

     Hello metoda GenerateThumbnail dokončí vytváření hello miniaturu.
12. V prohlížeči hello indexovou stránku hello aktualizace a v tématu hello miniaturu.
13. Ve Visual Studiu stisknutím klávesy SHIFT + F5 toostop ladění.
14. V **Průzkumníka serveru**, klikněte pravým tlačítkem na uzel ContosoAdsWebJob hello a klikněte na **zobrazení řídicího panelu**.
15. Přihlaste se pomocí přihlašovacích údajů Azure a klikněte hello webové úlohy název toogo toohello stránky pro vaše webová úloha.

     ![Klikněte na tlačítko ContosoAdsWebJob](./media/web-sites-dotnet-troubleshoot-visual-studio/clickcaw.png)

     Hello řídicího panelu ukazuje, že hello GenerateThumbnail funkce nedávno.

     (hello příštím kliknutí na tlačítko **zobrazení řídicího panelu**, nemáte toosign v a hello prohlížeč přejde přímo toohello stránky pro vaše webová úloha.)
16. Klikněte na tlačítko hello funkce název toosee podrobnosti o spuštění funkce hello.

     ![Podrobností funkce](./media/web-sites-dotnet-troubleshoot-visual-studio/funcdetails.png)

Pokud funkce [napsali protokoly](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs), uživatel může klepnout **ToggleOutput** toosee je.

## <a name="notes-about-remote-debugging"></a>Poznámky o vzdálené ladění
* Spuštění v režimu ladění v produkčním prostředí se nedoporučuje. Pokud vaše produkční webová aplikace není škálovat instance serveru toomultiple, ladění zabrání hello webový server z odpovídá tooother žádostí. V takovém případě několika instancemi webového serveru, když připojit ladicí program toohello získáte náhodných instance a mít žádný způsob, jak tooensure prohlížeč tohoto následné požadavky přejde toothat instance. Navíc obvykle nenasazujete tooproduction sestavení ladění a optimalizace kompilátoru pro verzi sestavení může činit možné tooshow, co se děje řádek po řádku ve zdrojovém kódu. Při řešení problémů produkční, je nejlepší prostředku aplikace protokoly trasování a webového serveru.
* Vyhněte se dlouho se zastavuje na zarážky při vzdáleném ladění. Azure zpracovává proces, který je zastavena po dobu delší než několik minut jako reagovat proces a ukončí se.
* Při ladění, hello server odesílá data tooVisual Studio, které by mohly ovlivnit poplatky šířky pásma. Informace o sazbách šířky pásma najdete v tématu [Azure – ceny](https://azure.microsoft.com/pricing/calculator/).
* Ujistěte se, že hello `debug` atribut hello `compilation` element v hello *Web.config* soubor tootrue. Ho tootrue ve výchozím nastavení při publikování konfiguraci sestavení ladění.

        <system.web>
          <compilation debug="true" targetFramework="4.5" />
          <httpRuntime targetFramework="4.5" />
        </system.web>
* Pokud zjistíte, že tento ladicího programu hello nebude kroku do kódu, které chcete toodebug, bude pravděpodobně toochange hello pouze můj kód nastavení.  Další informace najdete v tématu [omezit taktování tooJust vlastní kód](http://msdn.microsoft.com/library/vstudio/y740d9d3.aspx#BKMK_Restrict_stepping_to_Just_My_Code).
* Časovač se spustí na serveru hello, když povolíte hello funkce vzdálené ladění a po 48 hodinách hello funkce je automaticky vypnuta. Tento limit 48 hodin se provádí z důvodů zabezpečení a výkonu. Můžete snadno zapnout funkci hello zpět jako tolikrát, kolikrát chcete. Doporučujeme ponechat zakázané při nejsou aktivně ladění.
* Můžete ručně připojit hello ladicí tooany proces, ne jenom hello webové aplikace proces (w3wp.exe). Další informace o tom, jak toouse ladění režimu v sadě Visual Studio najdete v tématu [ladění v sadě Visual Studio](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx).

## <a name="logsoverview"></a>Diagnostické protokoly – přehled
Aplikace ASP.NET, která běží ve webové aplikace Azure můžete vytvořit následující typy protokolů hello:

* **Protokoly trasování aplikací**<br/>
  Hello aplikace vytvoří tyto protokoly voláním metod hello [System.Diagnostics.Trace](http://msdn.microsoft.com/library/system.diagnostics.trace.aspx) třídy.
* **Protokoly webového serveru**<br/>
  webový server Hello vytvoří položku protokolu pro každý HTTP žádost toohello webovou aplikaci.
* **Protokoly podrobné chybové zprávy**<br/>
  webový server Hello vytvoří stránku HTML se některé další informace o selhání požadavků HTTP (ty, které mít za následek stavový kód 400 nebo vyšší).
* **Protokoly trasování požadavku se nezdařilo**<br/>
  webový server Hello vytvoří soubor XML s informacemi o podrobného trasování pro chybné žádosti HTTP. webový server Hello také poskytuje XSL souboru tooformat hello XML v prohlížeči.

Protokolování ovlivňuje výkon webové aplikace, takže Azure vám dává možnost tooenable hello nebo zakázat každého typu protokolu podle potřeby. Pro protokoly aplikací můžete zadat, že by měly být zapsány pouze protokoly do určité míry závažnosti. Při vytváření nové webové aplikace ve výchozím nastavení všechny protokolování je zakázána.

Protokoly jsou zapsány v toofiles *LogFiles* složku v systému souborů hello vaší webové aplikace a jsou přístupné přes FTP. Protokoly webového serveru a v protokolu aplikací lze zapsat také tooan účet úložiště Azure. Můžete zachovat větší objem protokolů v účtu úložiště, než je možné v systému souborů hello. Při použití systému souborů hello jste omezené tooa maximálně 100 megabajtů protokolů. (Soubor systémové protokoly jsou pouze pro krátkodobé ukládání. Azure odstraní původní místo toomake soubory protokolu pro nové po dosažení limitu hello.)  

## <a name="apptracelogs"></a>Vytvořit a zobrazit protokoly trasování aplikací
V této části, můžete to udělat hello následující úlohy:

* Přidat trasování příkazy toohello webový projekt, který jste vytvořili v [Začínáme s Azure a ASP.NET][GetStarted].
* Zobrazit protokoly hello při spuštění projektu hello místně.
* Zobrazit protokoly hello generovaná hello aplikace běžící v Azure.

Informace o způsobu toocreate aplikace přihlásí webové úlohy najdete v tématu [jak toowork s Azure queue storage pomocí hello WebJobs SDK – jak toowrite protokoly](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs). Hello následující pokyny pro prohlížení protokolů a řídí, jak byly uloženy do Azure použít také tooapplication protokoly vytvořené webové úlohy.

### <a name="add-tracing-statements-toohello-application"></a>Přidat aplikaci toohello příkazy trasování
1. Otevřete *Controllers\HomeController.cs*a nahraďte hello `Index`, `About`, a `Contact` metody s hello následující kód v pořadí tooadd `Trace` příkazy a `using` – příkaz pro `System.Diagnostics`:

        public ActionResult Index()
        {
            Trace.WriteLine("Entering Index method");
            ViewBag.Message = "Modify this template toojump-start your ASP.NET MVC application.";
            Trace.TraceInformation("Displaying hello Index page at " + DateTime.Now.ToLongTimeString());
            Trace.WriteLine("Leaving Index method");
            return View();
        }

        public ActionResult About()
        {
            Trace.WriteLine("Entering About method");
            ViewBag.Message = "Your app description page.";
            Trace.TraceWarning("Transient error on hello About page at " + DateTime.Now.ToShortTimeString());
            Trace.WriteLine("Leaving About method");
            return View();
        }

        public ActionResult Contact()
        {
            Trace.WriteLine("Entering Contact method");
            ViewBag.Message = "Your contact page.";
            Trace.TraceError("Fatal error on hello Contact page at " + DateTime.Now.ToLongTimeString());
            Trace.WriteLine("Leaving Contact method");
            return View();
        }        
2. Přidat `using System.Diagnostics;` toohello příkaz na začátek souboru hello.

### <a name="view-hello-tracing-output-locally"></a>Zobrazení hello trasování výstup místně
1. Stisknutím klávesy F5 toorun hello aplikace v režimu ladění.

    naslouchací proces Hello výchozí trasování zapíše všechny toohello výstup trasování **výstup** okno, společně s další výstup ladění. Hello následující obrázek znázorňuje hello výstup hello příkazy trasování, zda jste přidali toohello `Index` metoda.

    ![Trasování v okně ladění](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugtracing.png)

    Hello následující kroky ukazují, jak výstupu trasování tooview na webové stránce, bez kompilace v režimu ladění.
2. Otevřete soubor Web.config aplikace hello (hello jeden umístěný ve složce projektu hello) a přidejte `<system.diagnostics>` element v hello konec souboru hello těsně před uzavírací hello `</configuration>` element:

          <system.diagnostics>
            <trace>
              <listeners>
                <add name="WebPageTraceListener"
                    type="System.Web.WebPageTraceListener,
                    System.Web,
                    Version=4.0.0.0,
                    Culture=neutral,
                    PublicKeyToken=b03f5f7f11d50a3a" />
              </listeners>
            </trace>
          </system.diagnostics>

    Hello `WebPageTraceListener` umožňuje zobrazit výstup trasování procházením příliš`/trace.axd`.
3. Přidat <a href="http://msdn.microsoft.com/library/vstudio/6915t83k(v=vs.100).aspx">trasování element</a> pod `<system.web>` v souboru Web.config hello, jako je například hello následující ukázka:

        <trace enabled="true" writeToDiagnosticsTrace="true" mostRecent="true" pageOutput="false" />
4. Stisknutím kombinace kláves CTRL + F5 toorun hello aplikace.
5. V panelu Adresa hello hello okna prohlížeče, přidejte *trace.axd* toohello adresu URL, a stiskněte klávesu Enter (hello bude mít adresu URL podobnou toohttp://localhost:53370/trace.axd).
6. Na hello **trasování aplikací** klikněte na tlačítko **zobrazit podrobnosti** na první řádek hello (ne hello BrowserLink řádku).

    ![Trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd1.png)

    Hello **podrobností žádosti o** se zobrazí stránka a v hello **informace trasování** části uvidíte výstup hello hello trasovacích příkazů, zda jste přidali toohello `Index` metoda.

    ![Trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd2.png)

    Ve výchozím nastavení `trace.axd` je k dispozici pouze místně. Pokud byste chtěli toomake je k dispozici ze vzdálené webové aplikace, můžete přidat `localOnly="false"` toohello `trace` element v hello *Web.config* souboru, jak je znázorněno v hello následující ukázka:

        <trace enabled="true" writeToDiagnosticsTrace="true" localOnly="false" mostRecent="true" pageOutput="false" />

    Povolení však `trace.axd` v produkční webové aplikace se obecně nedoporučuje z bezpečnostních důvodů a v následující části hello uvidíte jednodušší způsob tooread trasování protokoly v webové aplikace Azure.

### <a name="view-hello-tracing-output-in-azure"></a>Zobrazit výstup trasování hello v Azure
1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello webový projekt a klikněte na **publikovat**.
2. V hello **Publikovat Web** dialogové okno, klikněte na tlačítko **publikovat**.

    Po Visual Studio publikuje aktualizace, otevře se okno prohlížeče tooyour domovské stránky (za předpokladu, že nebyla zrušíte **cílová adresa URL** na hello **připojení** karta).
3. V **Průzkumníka serveru**, klikněte pravým tlačítkem na vaší webové aplikace a vyberte **zobrazit protokoly streamování**.

    ![Protokoly streamování zobrazení v místní nabídce](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewlogsmenu.png)

    Hello **výstup** zobrazí okno, které jsou připojené toohello vysílání datového proudu protokolu služby a přidá řádek oznámení každou minutu, který prochází bez toodisplay protokolu.

    ![Protokoly streamování zobrazení v místní nabídce](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-nologsyet.png)
4. V okně prohlížeče hello, který ukazuje domovskou stránku aplikace, klikněte na tlačítko **kontaktujte**.

    Během pár sekund hello výstup z trasování úroveň chyb hello jste přidali toohello `Contact` metoda se zobrazí v hello **výstup** okno.

    ![Chyba trasování v okně výstupu](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-errortrace.png)

    Visual Studio je zobrazuje pouze trasování úroveň chyb, protože se jedná hello výchozí nastavení, když povolíte službu monitorování protokolu hello. Když vytvoříte novou Azure webovou aplikaci, všechny protokolování ve výchozím nastavení vypnutá, jako jste viděli při otevření stránky nastavení hello dříve:

    ![Aplikace odhlášení](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-apploggingoff.png)

    Ale když vyberete **zobrazit protokoly streamování**, Visual Studio automaticky změnit **aplikace Logging(File System)** příliš**chyba**, což znamená, že úroveň chyb protokoly získat ohlásil. V pořadí toosee všechny protokoly trasování, můžete změnit toto nastavení příliš**podrobné**. Když vyberete úroveň závažnosti, která je nižší než chyba, jsou rovněž uvedeny všechny protokoly pro vyšší úrovně závažnosti. Proto když vyberete podrobné, je také zobrazit informace, upozornění a protokoly chyb.  

1. V **Průzkumníka serveru**, klikněte pravým tlačítkem na hello webové aplikace a pak klikněte na tlačítko **nastavení zobrazení** stejně jako dříve.
2. Změna **protokolování aplikace (systém souborů)** příliš**podrobné**a potom klikněte na **Uložit**.

    ![Nastavení úrovně tooVerbose trasování](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-applogverbose.png)
3. V okně prohlížeče hello, který se teď zobrazuje vaše **obraťte se na** klikněte na tlačítko **Domů**, pak klikněte na tlačítko **o**a potom klikněte na **kontaktujte**.

    Během pár sekund, hello **výstup** v okně se zobrazí všechny vaše výstup trasování.

    ![Výstup podrobného trasování](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-verbosetraces.png)

    V této části povolit a zakázat protokolování pomocí nástroje nastavení Azure webové aplikace. Můžete také povolit nebo zakázat trasování – moduly naslouchání úpravou souboru Web.config hello. Ale úprava souboru Web.config hello způsobí, že toorecycle domény aplikace hello, zatímco povolení protokolování prostřednictvím konfiguraci webové aplikace hello neprovádí. Pokud hello problém trvá dlouho tooreproduce nebo je přerušované, recyklace domény aplikace hello může "Automatická oprava" a vynutit toowait dokud Odehrává se znovu. Povolení diagnostiky v Azure neprovádí toho, abyste mohli začít okamžitě zaznamenávání informací o chybách.

### <a name="output-window-features"></a>Funkce výstup – okno
Hello **protokolů Azure** kartě hello **výstup** okno obsahuje několik tlačítek a textové pole:

![Protokoly kartě tlačítka](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-icons.png)

To proveďte hello následující funkce:

* Vymazat hello **výstup** okno.
* Povolit nebo zakázat zalamování řádků.
* Spuštění nebo zastavení monitorování protokolů.
* Určete, které protokoly toomonitor.
* Stažení protokolů.
* Filtrujte protokoly na základě hledaný řetězec nebo regulární výraz.
* Zavřít hello **výstup** okno.

Pokud zadáte hledaný řetězec nebo regulární výraz, Visual Studio filtruje informace o protokolování na klientovi hello. To znamená, že můžete zadat kritéria hello po hello protokoly jsou zobrazeny v hello **výstup** okno nebo můžete změnit kritéria filtrování bez nutnosti tooregenerate hello protokoly.

## <a name="webserverlogs"></a>Zobrazení protokolů webového serveru
Protokoly webového serveru zaznamenejte všechny aktivitu protokolu HTTP pro webovou aplikaci hello. V pořadí toosee je v hello **výstup** okna máte tooenable je hello webová aplikace a sdělte Visual Studio, které chcete toomonitor je.

1. V hello **konfiguraci webové aplikace Azure** karty, které jste otevřeli z **Průzkumníka serveru**, protokolování webového serveru změňte příliš**na**a pak klikněte na tlačítko **Uložit**.

    ![Povolení protokolování webového serveru](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-webserverloggingon.png)
2. V hello **výstup** okně klikněte na tlačítko hello **určit, které Azure protokoly toomonitor** tlačítko.

    ![Určit, které Azure protokoly toomonitor](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-specifylogs.png)
3. V hello **možnosti protokolování Azure** dialogové okno, vyberte **webové protokoly serveru**a potom klikněte na **OK**.

    ![Monitorování protokolů webového serveru](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorwslogson.png)
4. V okně prohlížeče hello, který ukazuje hello webové aplikace, klikněte na tlačítko **Domů**, pak klikněte na tlačítko **o**a potom klikněte na **kontaktujte**.

    protokoly aplikací Hello obecně zobrazit jako první, následuje hello protokolů webového serveru. Můžete mít toowait chvíli hello protokoly tooappear.

    ![Protokoly webového serveru v okně výstupu](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-wslogs.png)

Při prvním zapnutí protokolů webového serveru pomocí sady Visual Studio, Azure ve výchozím nastavení zapíše systém souborů toohello protokoly hello. Jako alternativu, můžete použít hello Azure portálu toospecify, který webové protokoly serveru by měly být zapsány tooa kontejner objektů blob v účtu úložiště.

Pokud používáte protokolování účtu úložiště Azure tooan hello portálu tooenable webový server a potom zakázat protokolování v sadě Visual Studio, pokud znovu povolíte protokolování v sadě Visual Studio se obnoví nastavení svého účtu úložiště.

## <a name="detailederrorlogs"></a>Zobrazit podrobné chybové zprávy protokoly
Podrobné protokoly chyb zadat nějaké další informace o požadavcích HTTP, jejichž výsledkem odpovědi kódy chyb (400 nebo novější). V pořadí toosee je v hello **výstup** okně máte tooenable je hello webová aplikace a sdělte Visual Studio, které chcete toomonitor je.

1. V hello **konfiguraci webové aplikace Azure** karty, které jste otevřeli z **Průzkumníka serveru**, změňte **podrobné chybové zprávy** příliš**na**, a pak klikněte na tlačítko **Uložit**.

    ![Povolit podrobné chybové zprávy](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailedlogson.png)
2. V hello **výstup** okně klikněte na tlačítko hello **určit, které Azure protokoly toomonitor** tlačítko.
3. V hello **možnosti protokolování Azure** dialogové okno, klikněte na tlačítko **všechny protokoly**a potom klikněte na **OK**.

    ![Monitorování všech protokolů](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorall.png)
4. V panelu Adresa hello hello okna prohlížeče, přidejte adresu URL toocause toohello znak navíc chybu 404 (například `http://localhost:53370/Home/Contactx`), a stiskněte klávesu Enter.

    Po několika sekundách hello podrobné informace o chybě protokol se zobrazí v sadě Visual Studio hello **výstup** okno.

    ![Podrobné informace o chybě přihlášení výstup – okno](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorlog.png)

    Řízení a klikněte na tlačítko hello odkaz toosee hello protokolu výstup ve formátu v prohlížeči:

    ![Podrobné informace o chybě protokolů v okně prohlížeče](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorloginbrowser.png)

## <a name="downloadlogs"></a>Stáhněte si soubor protokolu systému
Všechny protokoly, které můžete sledovat v hello **výstup** okno lze také stáhnout jako *.zip* souboru.

1. V hello **výstup** okně klikněte na tlačítko **stažení protokolů streamování**.

    ![Protokoly kartě tlačítka](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadicon.png)

    Otevře se Průzkumník souborů tooyour *stáhne* složku s hello stáhnout soubor vybraný.

    ![Stažený soubor](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadedfile.png)
2. Extrahování hello *.zip* soubor a v tématu hello následující strukturu složek:

    ![Stažený soubor](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilefolders.png)

   * Protokoly trasování aplikace jsou ve *.txt* soubory v hello *LogFiles\Application* složky.
   * Protokoly webového serveru jsou v *.log* soubory v hello *LogFiles\http\RawLogs* složky. Nástroj můžete použít jako [analyzátoru protokolů](http://www.microsoft.com/download/details.aspx?displaylang=en&id=24659) tooview a zpracování těchto souborů.
   * Podrobné chybové zprávy protokoly jsou ve *.html* soubory v hello *LogFiles\DetailedErrors* složky.

     (hello *nasazení* složka je pro soubory vytvořené serverem správy zdrojového kódu publikování; nemá cokoli související tooVisual Studio publikování. Hello *Git* složka je pro trasování související toosource řízení publikování hello soubor protokolu a datových proudů služby.)  

## <a name="storagelogs"></a>Zobrazit protokoly úložiště
Protokoly trasování aplikací lze je také odeslat tooan účtu úložiště Azure a lze je zobrazit v sadě Visual Studio. toodo budete vytvořit účet úložiště, povolte protokol úložiště na portálu classic hello a zobrazit v hello **protokoly** kartě hello **webové aplikace Azure** okno.

Můžete odeslat protokoly tooany nebo všechny tři cíle:

* systém souborů Hello.
* Tabulky účet úložiště.
* Objekty BLOB účet úložiště.

Můžete zadat úroveň závažnosti různé pro jednotlivé cíle.

Tabulky bylo snadné tooview podrobnosti protokolů online, a podporují streamování; se můžete dotazovat protokoly v tabulkách a v tématu nové protokoly, jako během vytváření. Objekty BLOB zkontrolujte ho snadno toodownload protokoly v souborech a tooanalyze je pomocí HDInsight, protože zná HDInsight jak toowork pomocí úložiště objektů blob. Další informace najdete v tématu **Hadoop a MapReduce** v [možnosti ukládání dat (vytváření reálných cloudových aplikací s Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options).

Momentálně máte úroveň systému souborů protokolů sady tooverbose; Hello následující postup vás provede procesem nastavení informace o úrovni protokoly toogo toostorage účet tabulky. Informace o úrovni znamená všechny protokoly vytvořená voláním `Trace.TraceInformation`, `Trace.TraceWarning`, a `Trace.TraceError` se zobrazí, ale není protokoly vytvořená voláním `Trace.WriteLine`.

Účty úložiště nabízí že další úložiště a jejich uchovávání dlouhodobou pro protokoly porovnání toohello systému souborů. Další výhodou odesílající aplikací toostorage protokoly trasování je, že dostanete doplňující informace, přičemž každý protokol, který Nezískávat z protokolů systému souborů.

1. Klikněte pravým tlačítkem na **úložiště** v části hello Azure uzel a potom klikněte na **vytvořit účet úložiště**.

![Vytvořit účet úložiště](./media/web-sites-dotnet-troubleshoot-visual-studio/createstor.png)

1. V hello **vytvořit účet úložiště** dialogové okno, zadejte název pro účet úložiště hello.

    Název Hello musí být musí být jedinečné (žádný jiný účet úložiště Azure může mít hello stejný název). Pokud zadáte název hello je již používán získáte možnost toochange ho.

    Hello tooaccess adresa URL účtu úložiště bude *{name}*. core.windows.net.
2. Sada hello **oblast nebo skupinu vztahů** rozevíracího seznamu toohello oblast nejbližší tooyou.

    Toto nastavení určuje, které datové centrum Azure bude hostitelem účtu úložiště. Pro účely tohoto kurzu nebude Zkontrolujte své volby, významné rozdíly, ale pro produkční webové aplikace, aby váš webový server a vaše toobe účet úložiště v hello stejnou oblast toominimize latenci a data výstupních poplatky. v oblasti co možná toohello prohlížeče přístup k vaší webové aplikace v pořadí toominimize latence měli spustit Hello webové aplikace (aplikaci, kterou vytvoříte později).
3. Sada hello **replikace** rozevírací seznam příliš**místně redundantní**.
   
    Když je povolené geografická replikace pro účet úložiště, hello uložený obsah je umístění toothat replikované tooa sekundárního datacentra tooenable převzetí služeb při selhání v případě významnější havárie v primárním umístění hello. Geografická replikace může způsobit dodatečné náklady. Pro testovací a vývojové účty obecně nechcete, aby toopay za geografickou replikaci. Další informace naleznete v článku o [vytvoření, správě nebo odstranění účtu úložiště](../storage/common/storage-create-storage-account.md).
4. Klikněte na možnost **Vytvořit**.

    ![Nový účet úložiště](./media/web-sites-dotnet-troubleshoot-visual-studio/newstorage.png)    
5. V sadě Visual Studio hello **webové aplikace Azure** okně klikněte na tlačítko hello **protokoly** a pak klikněte **konfigurace protokolování na portálu pro správu**.

    <!-- todo:screenshot of new portal if hello VS page link goes toonew portal -->
    ![Konfigurace protokolování](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configlogging.png)

    Tím se otevře hello **konfigurace** kartě portálu classic hello pro vaši webovou aplikaci.
6. V hello portálu classic **konfigurace** kartě, posuňte se dolů oddílu diagnostiky aplikací toohello a poté změňte **protokolování aplikace (Table Storage)** příliš**na**.
7. Změna **úroveň protokolování** příliš**informace**.
8. Klikněte na tlačítko **spravovat úložiště Table**.

    ![Kliknutím na spravovat TableStorage](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-stgsettingsmgmtportal.png)

    V hello **spravovat úložiště table pro rozhraní application diagnostics** pole můžete vašeho účtu úložiště, pokud máte více než jeden. Můžete vytvořit novou tabulku nebo použijte existující.

    ![Správa úložiště table](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-choosestorageacct.png)
9. V hello **spravovat úložiště table pro rozhraní application diagnostics** pole, klikněte na hello zaškrtnutí tooclose hello pole.
10. V hello portálu classic **konfigurace** , klikněte na **Uložit**.
11. V okně prohlížeče hello, která zobrazuje hello aplikace webové aplikace, klikněte na tlačítko **Domů**, pak klikněte na tlačítko **o**a potom klikněte na **kontaktujte**.

     informace o protokolování Hello vyprodukované procházení tyto webové stránky, bude napsán toohello účet úložiště.
12. V hello **protokoly** kartě hello **webové aplikace Azure** oken v sadě Visual Studio, klikněte na tlačítko **aktualizovat** pod **diagnostiky Souhrn**.

     ![Klikněte na tlačítko Aktualizovat](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-refreshstorage.png)

     Hello **diagnostiky Souhrn** část ukazuje protokoly pro hello posledních 15 minut ve výchozím nastavení. Období toosee hello můžete změnit další protokoly.

     (Pokud se zobrazí chyba "Tabulka nebyla nalezena", ověřte, že jste vyhledali toohello stránek, které hello trasování po jste povolili **protokolování aplikace (úložiště)** a po kliknutí na **Uložit**.)

     ![Protokoly úložiště](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-storagelogs.png)

     Všimněte si, že v tomto zobrazení můžete najdete v části **ID procesu** a **ID vlákna** pro každý protokol, který není dostupná v systému protokoly hello. Zobrazí se další pole zobrazením hello úložiště Azure table přímo.
13. Klikněte na tlačítko **zobrazit všechny protokoly aplikací**.

     tabulku protokolu trasování Hello se zobrazí v prohlížeči tabulky hello úložiště Azure.

     (Pokud dojde k chybě "sekvence neobsahuje žádné elementy", otevřete **Průzkumníka serveru**, rozbalte uzel hello pro váš účet úložiště v rámci hello **Azure** uzel a potom klikněte pravým tlačítkem na **tabulky**a klikněte na tlačítko **aktualizovat**.)

     ![Protokoly úložiště v zobrazení tabulky](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracelogtableview.png)

     Toto zobrazení uvádí další pole, že se nezobrazí v ostatních zobrazeních. Toto zobrazení můžete také toofilter protokoly pomocí speciální dotazu Tvůrce uživatelského rozhraní pro tvorbu dotazu. Další informace najdete v tématu práci s prostředky tabulky – filtrování entity v [procházení prostředků úložiště pomocí Průzkumníka serveru](http://msdn.microsoft.com/library/ff683677.aspx).
14. toolook v hello podrobnosti jeden řádek, poklikejte na jednoho z řádků hello.

     ![Tabulka trasování v Průzkumníku serveru](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracetablerow.png)

## <a name="failedrequestlogs"></a>Zobrazit protokoly trasování neúspěšných žádostí.
Protokoly trasování neúspěšných žádostí jsou užitečné, když potřebujete toounderstand hello podrobnosti jak služba IIS zpracovává požadavek HTTP ve scénářích, jako je například adresa URL přepisování nebo ověřování problémy.

Funkce trasování, která jsou k dispozici se službou IIS 7.0 a novějším požadavku službě Azure web apps použití hello stejné se nezdařilo. Toohello přístupu, které se budou Protokolovat nastavení služby IIS, které konfigurují které chyby, ale nemáte. Pokud je povoleno trasování chybných požadavků, všechny chyby zaznamenání.

Trasování chybných požadavků můžete povolit pomocí sady Visual Studio, ale nelze je zobrazit v sadě Visual Studio. Tyto protokoly jsou soubory formátu XML. monitoruje Hello streamování služby Protokolování pouze soubory, které se považují za čitelný v režimu prostého textu: *.txt*, *.html*, a *.log* soubory.

Protokoly trasování chybných požadavků můžete zobrazit v prohlížeči přímo přes FTP nebo místně po pomocí FTP nástroj toodownload je tooyour místního počítače. V této části budete zobrazí je v prohlížeči přímo.

1. V hello **konfigurace** kartě hello **webové aplikace Azure** okno, které jste otevřeli z **Průzkumníka serveru**, změňte **trasování chybných požadavků**příliš**na**a potom klikněte na **Uložit**.

    ![Povolit trasování neúspěšných žádostí.](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequeston.png)
2. V panelu Adresa hello hello okna prohlížeče, který ukazuje hello webové aplikace přidat adresu URL toohello znak navíc a klikněte na Enter toocause chybu 404.

    To způsobí, že toobe protokolu trasování chybných požadavků vytvořit a hello následující kroky ukazují, jak tooview nebo stažení hello protokolu.
3. V sadě Visual Studio v hello **konfigurace** kartě hello **webové aplikace Azure** okně klikněte na tlačítko **otevřít na portálu pro správu**.
4. V hello [portálu Azure](https://portal.azure.com) **nastavení** pro webovou aplikaci, klikněte na **přihlašovací údaje nasazení**a poté zadejte nové uživatelské jméno a heslo.

    ![Nové FTP uživatelské jméno a heslo](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-enterftpcredentials.png)

    ** Při přihlášení, máte toouse hello úplné uživatelské jméno s hello webové aplikace s předponou názvu tooit. Například pokud zadáte "myid" jako uživatelské jméno a hello webu je "myexample", můžete přihlásit jako "myexample\myid".
5. V nové okno prohlížeče, přejděte toohello adresu URL, které se zobrazí v části **název hostitele FTP** nebo **FTPS hostname** v hello **webové aplikace** okno pro vaši webovou aplikaci.
6. Přihlaste se pomocí přihlašovacích údajů hello FTP, které jste vytvořili dříve (včetně hello webové aplikace předpona názvu pro hello uživatelské jméno).

    Hello prohlížeč zobrazí hello kořenové složky hello webové aplikace.
7. Otevřete hello *LogFiles* složky.

    ![Otevřete složku LogFiles](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilesfolder.png)
8. Otevřete složku hello, který je pojmenován W3SVC plus číselná hodnota.

    ![Otevřete složku W3SVC](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfolder.png)

    Hello složka obsahuje soubory XML pro všechny chyby, které byly zaprotokolovány po povolení trasování chybných požadavků a soubor XSL, můžete prohlížeč tooformat hello XML.

    ![W3SVC složky](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfoldercontents.png)
9. Klikněte na tlačítko hello soubor XML pro hello chybných požadavků, které chcete toosee informace trasování pro.

    Hello následující obrázek znázorňuje součástí hello trasování informace o chybě ukázka.

    ![Trasování neúspěšných žádostí v prohlížeči](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequestinbrowser.png)

## <a name="nextsteps"></a>Další kroky
Už víte, jak Visual Studio umožňuje snadno tooview protokoly vytvořené webové aplikace Azure. Hello následující části obsahují odkazy toomore prostředky na související témata:

* Řešení potíží s Azure webové aplikace
* Ladění v sadě Visual Studio
* Vzdálené ladění v Azure
* Trasování v aplikacích ASP.NET
* Analýza protokolů webového serveru
* Analýza protokolů trasování neúspěšných žádostí.
* Ladění cloudové služby

### <a name="azure-web-app-troubleshooting"></a>Řešení potíží s Azure webové aplikace
Další informace o řešení potíží s webovými aplikacemi ve službě Azure App Service najdete v tématu hello následující prostředky:

* [Jak toomonitor webové aplikace](/manage/services/web-sites/how-to-monitor-websites/)
* [Nevracení paměti v Azure Web Apps s nástroji Visual Studio 2013 příčin](http://blogs.msdn.com/b/visualstudioalm/archive/2013/12/20/investigating-memory-leaks-in-azure-web-sites-with-visual-studio-2013.aspx). Microsoft ALM příspěvku na blogu o funkcích nástroje Visual Studio pro analýzu spravovat problémy s pamětí.
* [Službě Azure web apps online nástroje byste měli vědět o](https://azure.microsoft.com/blog/2014/03/28/windows-azure-websites-online-tools-you-should-know-about-2/). Příspěvek blogu společností Apple Amitu.

Nápovědu k řešení problémů s konkrétní dotaz spusťte vlákno v jednom z následujících fóra hello:

* [Hello Azure fórum na webu technologie ASP.NET hello](http://forums.asp.net/1247.aspx/1?Azure+and+ASP+NET).
* [Hello Azure fórum na webu MSDN](http://social.msdn.microsoft.com/Forums/windowsazure/).
* [StackOverflow.com](http://www.stackoverflow.com).

### <a name="debugging-in-visual-studio"></a>Ladění v sadě Visual Studio
Další informace o tom, jak toouse ladění režimu v sadě Visual Studio najdete v tématu hello [ladění v sadě Visual Studio](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx) tématu MSDN a [ladění tipy sadou Visual Studio 2010](http://weblogs.asp.net/scottgu/archive/2010/08/18/debugging-tips-with-visual-studio-2010.aspx).

### <a name="remote-debugging-in-azure"></a>Vzdálené ladění v Azure
Další informace o vzdálené ladění pro webové aplikace Azure a webové úlohy najdete v tématu hello následující prostředky:

* [Úvod tooRemote ladění Azure App Service Web Apps](https://azure.microsoft.com/blog/2014/05/06/introduction-to-remote-debugging-on-azure-web-sites/).
* [Úvod tooRemote ladění Azure App Service Web Apps část 2 - uvnitř vzdálené ladění](https://azure.microsoft.com/blog/2014/05/07/introduction-to-remote-debugging-azure-web-sites-part-2-inside-remote-debugging/)
* [Úvod tooRemote ladění na Azure App Service Web Apps část 3 - prostředí s více instancemi a GIT](https://azure.microsoft.com/blog/2014/05/08/introduction-to-remote-debugging-on-azure-web-sites-part-3-multi-instance-environment-and-git/)
* [Webové úlohy ladění (video)](https://www.youtube.com/watch?v=ncQm9q5ZFZs&list=UU_SjTh-ZltPmTYzAybypB-g&index=1)

Pokud vaše webová aplikace používá back-end Azure webového rozhraní API nebo Mobile Services a budete potřebovat toodebug, který najdete v části [ladění v rozhraní .NET back-end v sadě Visual Studio](http://blogs.msdn.com/b/azuremobile/archive/2014/03/14/debugging-net-backend-in-visual-studio.aspx).

### <a name="tracing-in-aspnet-applications"></a>Trasování v aplikacích ASP.NET
Neexistují žádné tooASP.NET důkladné a aktuální přehled trasování k dispozici na hello Internetu. můžete provést doporučené Hello je začít pracovat s původním úvodní materiály, které jsou vytvořeny pro webové formuláře protože MVC nebyla ještě neexistuje a který doplnit s novější blog odešle které se zaměřují na konkrétní problémy. Některé toostart místa vhodná jsou hello následující prostředky:

* [Monitorování a Telemetrie (vytváření reálných cloudových aplikací s Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).<br>
  Elektronická kniha kapitoly s doporučeními pro trasování v cloudu Azure aplikace.
* [Trasování rozhraní ASP.NET](http://msdn.microsoft.com/library/ms972204.aspx)<br/>
  Starý, ale stále dobrý prostředku pro základní toohello předmět.
* [Trasování – moduly naslouchání](http://msdn.microsoft.com/library/4y5y10s7.aspx)<br/>
  Informace o trasování – moduly naslouchání, ale není zmínili hello [WebPageTraceListener](http://msdn.microsoft.com/library/system.web.webpagetracelistener.aspx).
* [Návod: Integrace s trasováním System.Diagnostics trasování technologie ASP.NET](http://msdn.microsoft.com/library/b0ectfxd.aspx)<br/>
  To příliš je v minulosti, ale zahrnuje některé další informace o této úvodní článek hello nezahrnuje.
* [Trasování v zobrazení syntaxe Razor rozhraní ASP.NET MVC](http://blogs.msdn.com/b/webdev/archive/2013/07/16/tracing-in-asp-net-mvc-razor-views.aspx)<br/>
  Kromě toho trasování v zobrazení syntaxe Razor hello post taky vysvětluje, jak filtrovat toocreate k chybě v pořadí toolog všechny neošetřenými výjimkami v aplikaci MVC. Informace o jak toolog všechny neošetřené výjimky v aplikaci webových formulářů, viz příklad souboru Global.asax hello v [kompletní příklad pro obslužné rutiny chyba](http://msdn.microsoft.com/library/bb397417.aspx) na webu MSDN. V MVC nebo webového formuláře Pokud chcete toolog určité výjimky, ale nechat framework výchozí hello zpracování vstoupí v platnost, můžete zachytit a opětovné jako hello následující ukázka:

        try
        {
           // Your code that might cause an exception toobe thrown.
        }
        catch (Exception ex)
        {
            Trace.TraceError("Exception: " + ex.ToString());
            throw;
        }
* [Streamování diagnostické trasování protokolování z hello příkazového řádku Azure (plus balíčku Glimpse!)](http://www.hanselman.com/blog/StreamingDiagnosticsTraceLoggingFromTheAzureCommandLinePlusGlimpse.aspx)<br/>
  Jak toouse hello příkazového řádku toodo co tento kurz ukazuje způsob toodo v sadě Visual Studio. [Balíčku glimpse](http://www.hanselman.com/blog/IfYoureNotUsingGlimpseWithASPNETForDebuggingAndProfilingYoureMissingOut.aspx) je nástroj pro ladění aplikací ASP.NET.
* [Webové aplikace, protokolování a diagnostiky - pomocí David Ebbo](/documentation/videos/azure-web-site-logging-and-diagnostics/) a [protokoly z webových aplikací – s David Ebbo streamování](/documentation/videos/log-streaming-with-azure-web-sites/)<br>
  Videa Scott Hanselman a David Ebbo.

Pro protokolování chyb alternativní toowriting vlastní kód trasování je rozhraní protokolování toouse open-source jako [ELMAH](http://nuget.org/packages/elmah/). Další informace najdete v tématu [příspěvky Scott Hanselman o ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx).

Všimněte si také, že nemáte toouse ASP.NET nebo System.Diagnostics trasování, pokud chcete, aby tooget streamování protokoly z Azure. Hello webové aplikace Azure streamování služby protokolování bude stream žádné *.txt*, *.html*, nebo *.log* soubor, který najde v hello *LogFiles* složky. Proto můžete vytvořit vlastní protokolování systému, který zapíše systém souborů toohello hello webové aplikace a souboru budou automaticky streamování a stáhli. Máte toodo je zápisu aplikační kód, který vytvoří soubory v hello *d:\home\logfiles* složky.

### <a name="analyzing-web-server-logs"></a>Analýza protokolů webového serveru
Další informace o analýze protokolů webového serveru najdete v tématu hello následující prostředky:

* [LogParser](http://www.microsoft.com/download/details.aspx?id=24659)<br/>
  Nástroj pro zobrazení dat v protokolů webového serveru (*.log* soubory).
* [Řešení potíží s problémy s výkonem služby IIS nebo pomocí LogParser chyb aplikací](http://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser)<br/>
  Zaznamená Úvod toohello analyzátoru protokolů nástroje, které můžete použít tooanalyze webový server.
* [Příspěvky podle Roberta Mcmurrayho na pomocí LogParser](http://blogs.msdn.com/b/robert_mcmurray/archive/tags/logparser/)<br/>
* [Hello stavový kód HTTP ve službě IIS 7.0, IIS 7.5 a IIS 8.0](http://support.microsoft.com/kb/943891)

### <a name="analyzing-failed-request-tracing-logs"></a>Analýza protokolů trasování neúspěšných žádostí.
zahrnuje Hello webu Microsoft TechNet [pomocí trasování chybných požadavků](http://www.iis.net/learn/troubleshoot/using-failed-request-tracing) oddíl, který může být užitečné pro pochopení jak toouse tyto protokoly. Tato dokumentace je však zaměřen především na Konfigurace trasování neúspěšných žádostí ve službě IIS, což nelze provést ve službě Azure Web Apps.

[GetStarted]: app-service-web-get-started-dotnet.md
[GetStartedWJ]: websites-dotnet-webjobs-sdk.md
