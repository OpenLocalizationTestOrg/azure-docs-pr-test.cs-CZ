---
title: "Šifrování CENC s více technologiemi DRM a řízením přístupu: referenční návrhu a implementace v Azure a Azure Media Services | Microsoft Docs"
description: "Informace o tom, jak toolicensing hello Microsoft® technologie Smooth Streaming klienta portování Kit."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 7814739b-cea9-4b9b-8370-538702e5c615
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: willzhan;kilroyh;yanmf;juliako
ms.openlocfilehash: 033fb618650c4fbe9069d467159a8734da759bba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>CENC využívající Multi-DRM a Access Control: Referenční návrh a implementace v prostředí Azure a Azure Media Services
 
## <a name="introduction"></a>Úvod
Je dobře známé je složité úlohy toodesign a sestavení DRM subsystém pro OTT nebo online řešení datového proudu. A je běžné praxi pro operátory, online toooutsource poskytovatelů videa poskytovatelé tuto část toospecialized DRM. Hello cílem tohoto dokumentu je toopresent odkaz návrhu a implementace začátku do konce DRM subsystému v OTT nebo streamování řešení online.

čtečky Hello cílem tohoto dokumentu se technici práce v subsystému DRM OTT nebo streamování nebo více-screen řešení online nebo všechny zájem o DRM subsystém čtečky. Hello předpokladem je, že čtečky obeznámeni s alespoň jedním z technologiemi DRM hello na trhu hello, jako je například technologie PlayReady, Widevine, FairPlay nebo Adobe Access.

Pomocí DRM jsme také zahrnovat CENC (Common Encryption) s více technologiemi DRM. Hlavní trend v online streamování a OTT odvětví je toouse CENC s více-native technologiemi DRM na různých platformách klienta, což je shift z předchozí trend hello pomocí jednoho DRM a jeho klientskou sadou SDK pro různé platformy klienta. Při použití šifrování CENC s více native DRM, technologie PlayReady a Widevine jsou šifrované podle hello [Common Encryption (ISO/IEC 23001-7 CENC)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) specifikace.

Hello CENC s více technologiemi DRM výhody následujícím způsobem:

1. Snižuje náklady, protože zpracování jedné šifrování se používá cílení na různých platformách s nativní technologiemi DRM; šifrování
2. Snižuje hello náklady na správu šifrované prostředky, protože je vyžadována pouze jedna kopie šifrované prostředků;
3. Eliminuje DRM klientů na licence náklady, protože nativního klienta DRM hello je obvykle volné v jeho nativní platforma.

Microsoft byl aktivní vykonavatel DASH a šifrování CENC společně s některé přehrávače hlavní odvětví. Microsoft Azure Media Services má poskytování podpory DASH a šifrování CENC. Poslední oznámení, najdete v tématu Mingfei na blozích: [služeb doručování licence Google Widevine uvedení ve službě Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/), a [Azure Media Services přidá balení Google Widevine pro doručení datový proud více technologiemi DRM](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Přehled tohoto článku
cílem Hello Tento článek obsahuje následující hello:

1. Poskytuje referenční návrhu podsystému DRM pomocí šifrování CENC s více technologiemi DRM;
2. Poskytuje implementaci odkaz na platformě Microsoft Azure nebo Azure Media Services;
3. Popisuje některé témata týkající se návrhu a implementace.

V článku hello obsahuje "více technologiemi DRM" hello následující:

1. Microsoft PlayReady
2. Google Widevine
3. Apple FairPlay 

Hello následující tabulka shrnuje hello nativní platforma/nativní aplikaci a prohlížeče nepodporuje každý DRM.

| **Klientské platformy** | **Podpora nativní DRM** | **Aplikace nebo prohlížeče** | **Streamování formáty** |
| --- | --- | --- | --- |
| **Inteligentní televizní přijímače, operátor externích zařízení, externích OTT zařízení** |PlayReady především se stává, nebo Widevine, nebo jiné |Linux, Opera, WebKit, jiné |Různými formáty |
| **Zařízení s Windows 10 (počítače s Windows, tablety s Windows, Windows Phone, Xbox)** |PlayReady |MS Edge/IE11/EME<br/><br/><br/>UWP |DASH (pro HLS, technologie PlayReady není podporována.)<br/><br/>DASH, technologie Smooth Streaming (HLS pro, PlayReady není podporována.) |
| **Zařízení se systémem Android (telefon, Tablet, TV)** |Widevine |Chrome nebo EME |DASH, HLS |
| **iOS (iPhone, iPad), klienty OS X a Apple TV** |FairPlay |Safari 8 +/ EME |HLS |


S ohledem hello aktuální stav nasazení pro každý DRM, služba obvykle mají toomake technologiemi DRM tooimplement 2 nebo 3 se vyřešit všechny hello typy koncových bodů v hello nejlepší způsob, jak.

Je kompromis mezi hello složitost hello služby logiky a složitost hello na hello klientské straně tooreach určitá úroveň uživatelské prostředí na hello různých klientů.

toomake váš výběr, mějte na paměti tyto skutečnosti:

* PlayReady je nativně implementované v každé zařízení s Windows, na některých zařízeních se systémem Android a dostupný prostřednictvím sady SDK softwaru na prakticky jakékoli platformě
* Widevine je nativně implementována v každé zařízení se systémem Android, Chrome a některá další zařízení
* FairPlay je k dispozici jenom v iOS a Mac OS klienty nebo prostřednictvím iTunes.

Proto by byl typické více technologiemi DRM:

* Možnost 1: PlayReady a Widevine
* Možnost 2: PlayReady, Widevine a FairPlay

## <a name="a-reference-design"></a>Návrh odkazu
V této části jsme nabídne odkaz návrhu, což je tooimplement lhostejné tootechnologies používá ji.

Subsystém DRM může obsahovat hello následující součásti:

1. Správa klíčů
2. Balení DRM
3. Doručování licencí DRM
4. Kontrola oprávnění
5. Ověřování/autorizace
6. Player
7. Původ nebo CDN

Hello následující diagram znázorňuje hello vysoké úrovně interakce mezi součástmi hello v subsystému DRM.

![Subsystému DRM s CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)

Existují tři základní "vrstvy" hello návrhu:

1. Zálohování vrstvy office (v černé), které nejsou vystaveny externě;
2. "DMZ" vrstvu (modrá), která obsahuje všechny koncové body hello čelí veřejné;
3. Veřejné internetové vrstvy (světla modrá) obsahující CDN a přehrávače s přenosy přes veřejný Internet.

Měla by existovat nástroj správy obsahu, k nimž ochrany DRM, bez ohledu na to je statické nebo dynamické šifrování. Hello vstupy pro šifrování DRM by měla obsahovat:

1. Video obsah MBR;
2. Klíč obsahu;
3. Licence získání adresy URL.

Během doby přehrávání toku na vysoké úrovni hello je:

1. Uživatel je ověřen;
2. Autorizační token se vytvoří pro uživatele hello;
3. DRM chráněný obsah (manifestu) je stažené tooplayer;
4. Přehrávač odešle licenčních pořízení požadavek toolicense serverů společně s klíče ID a autorizační token.

Před přesunutí dalším tématu toohello pár slov o hello návrhu služby správy klíčů.

| **ContentKey – prostředek** | **Scénář** |
| --- | --- |
| 1 – 1 |nejjednodušším případě Hello. Poskytuje nejlepší kontrolu hello. Ale obecně výsledkem hello nejvyšší licence doručení náklady. Požadavek na minimální jednu licenci je požadované pro každý chráněný prostředek. |
| 1 –-mnoho |Můžete použít hello obsahu stejný klíč pro více prostředků. Například pro všechny prostředky hello logické skupiny, jako je genre nebo podmnožinu genre (nebo gen film), můžete použít jeden klíč obsahu. |
| N – 1 |Více klíčů k obsahu, je potřeba pro každý prostředek. <br/><br/>Pokud potřebujete tooapply dynamické ochranu CENC s více technologiemi DRM pro MPEG-DASH a dynamické šifrování AES-128 pro HLS, například musíte dva samostatné obsahu klíče, každou s vlastním ContentKeyType. (Hello obsahu klíč používaný k dynamické šifrování CENC ochrany, ContentKeyType.CommonEncryption by měl použít, když pro hello obsahu, je klíč používaný k dynamické šifrování AES-128, ContentKeyType.EnvelopeEncryption by měl být použit.)<br/><br/>Další příklad v CENC ochrany DASH obsahu teoreticky, jeden klíč obsahu lze použít tooprotect datový proud videa a jiné zvukový datový proud obsahu tooprotect klíče. |
| Mnoho – příliš mnoho |Kombinace hello výše ve dvou situacích: jednu sadu obsahu, které se používají klíče pro každý hello více prostředků v hello stejné asset "skupina". |

Další důležitý faktor tooconsider je hello použití trvalé a dočasnou licencí.

Tyto aspekty jsou důležité

Budou mít přímý vliv toolicense doručení náklady, pokud používáte veřejného cloudu pro doručování licencí. Zvažte následující dvou různých případech tooillustrate hello:

1. Měsíční předplatné: použití trvalé licence a obsahu klíč asset mapování 1: n. Například pro všechny hello dětí filmy použijeme jeden klíč obsahu pro šifrování. V takovém případě:

    Celkový počet licencí # požadované pro všechny děti filmy a zařízení = 1
2. Měsíční předplatné: použít dočasnou licence a mapování 1: 1 mezi klíč obsahu a asset. V takovém případě:

    Celkový počet licencí # požadované pro všechny děti filmy a zařízení = [# filmy sledovaná] x [relací #]

Jak můžete snadno vidět, hello dvou různých návrzích mít za následek velmi jiné licenční žádost vzory proto doručování licencí náklady Pokud službu doručování licencí poskytuje veřejného cloudu, jako je například Azure Media Services.

## <a name="mapping-design-tootechnology-for-implementation"></a>Mapování tootechnology návrhu pro implementaci
V dalším kroku jsme mapovat naše tootechnologies obecné návrhu na platformě Microsoft Azure nebo Azure Media Services, zadáním které toouse technologie pro každý stavební blok.

Hello následující tabulka uvádí mapování hello:

| **Stavební blok.** | **Technologie** |
| --- | --- |
| **Player** |[Přehrávač médií Azure](https://azure.microsoft.com/services/media-services/media-player/) |
| **Zprostředkovatel identity (IDP)** |Azure Active Directory |
| **Služba tokenů zabezpečení (STS)** |Azure Active Directory |
| **Pracovní postup ochrany DRM** |Dynamické ochrany služby Azure Media Services |
| **Doručování licencí DRM** |1. Azure Media Services licence doručení (PlayReady, Widevine, FairPlay), nebo <br/>2. Axinom licenční Server, <br/>3. Vlastní PlayReady licenčního serveru |
| **Počátek** |Koncový bod streamování služby Azure Media Services |
| **Správa klíčů** |Není skutečně potřeba pro odkaz na implementaci |
| **Správa obsahu** |Konzolovou aplikaci C# |

Jinými slovy zprostředkovatele Identity (IDP) a zabezpečené tokenu služby (STS) bude Azure AD. Přehrávač, použijeme [rozhraní API služby Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/). Azure Media Services a Azure Media Player podporovat DASH a šifrování CENC s více technologiemi DRM.

Následující diagram ukazuje Hello hello celkové struktuře a toku s hello výše technologie mapování.

![Šifrování CENC na AMS](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

V pořadí tooset až dynamického šifrování CENC použije nástroj pro správu obsahu hello hello následující vstupy:

1. Otevřete obsahu;
2. Klíč obsahu z klíče generace nebo správu;
3. Adresy URL získávání licencí;
4. Seznam informací ze služby Azure AD.

bude mít Hello výstup hello nástroje správy obsahu:

1. ContentKeyAuthorizationPolicy obsahující specifikace hello na tom, jak doručování licencí ověří JWT token a specifikace licence DRM;
2. AssetDeliveryPolicy obsahující specifikace na streamování formátu, ochrany DRM a adresy URL získání licence.

Během doby běhu, hello tok je jako níže:

1. Po ověření uživatele se generuje JWT token;
2. Jedna z deklarací identity hello obsažených v tokenu JWT hello je "skupiny" deklarace obsahující ID objektu skupiny hello "EntitledUserGroup". Tento požadavek se použije pro předávání "Kontrola nárocích".
3. Přehrávač stahování klienta manifest CENC chráněného obsahu a "vidí" hello následující:

   1. klíč ID,
   2. šifrování CENC chráněný, je obsah Hello
   3. Licence získání adresy URL.
4. Přehrávač odešle požadavek získávání licencí podle hello prohlížeče nebo DRM podporována. V žádosti o získání licence hello, klíče ID a hello JWT bude také odeslat token. Službu doručování licencí ověří hello JWT token a hello deklarace obsažené předtím, než vydávající hello potřeby licence.

## <a name="implementation"></a>Implementace
### <a name="implementation-procedures"></a>Implementace postupů
implementace Hello bude zahrnovat hello následující kroky:

1. Příprava testovacího majetek: kódování nebo balíčku testovací video toomulti přenosovými fragmentovaný soubor MP4 ve službě Azure Media Services. Tento prostředek není DRM chráněný. DRM ochrany se provádí dynamické ochrany později.
2. Vytvoření klíče ID a obsah klíče (volitelně od počáteční hodnoty klíčů). Pro naše účely není vyžadována systémem správy klíčů, protože jsme se zabývají pouze jedinou sadu klíčů ID a klíč obsahu pro několik prostředků testovací;
3. Pomocí služeb o doručování licencí DRM více tooconfigure AMS rozhraní API pro prostředek testovací hello. Pokud používáte vlastní licenčních serverů ve vaší společnosti nebo vaše společnost dodavatelé místo licenční služby ve službě Azure Media Services, můžete tento krok přeskočit a zadat licenční získání adresy URL v kroku hello ke konfiguraci doručování licencí. Rozhraní API AMS je potřebné toospecify některé podrobné konfigurace, například omezení zásady autorizace, licence odpovědi šablony pro různé služby licence DRM, atd. V tomto okamžiku hello portál Azure nemá ještě poskytují hello potřeby uživatelského rozhraní pro tuto konfiguraci. Můžete najít informace o úroveň rozhraní API a ukázkový kód v dokumentu Dita Kornich: [pomocí PlayReady nebo Widevine běžného dynamického šifrování](media-services-protect-with-drm.md).
4. Použijte zásady doručení assetu tooconfigure AMS rozhraní API pro prostředek testovací hello. Můžete najít informace o úroveň rozhraní API a ukázkový kód v dokumentu Dita Kornich: [pomocí PlayReady nebo Widevine běžného dynamického šifrování](media-services-protect-with-drm.md).
5. Vytvořit a nakonfigurovat klienta služby Azure Active Directory v Azure;
6. Vytvořit několik uživatelských účtů a skupin v klientovi služby Azure Active Directory: je třeba vytvořit alespoň "EntitledUser" skupiny a přidat skupinu uživatelů toothis. Uživatelé v této skupině předá kontrolu oprávnění v získání licence a uživatelé není v této skupině se nezdaří kontrola ověřování toopass a nebude možné tooacquire žádné licence. Je členem této skupiny "EntitledUser" je taková deklarace požadované "skupiny" v hello tokenu JWT vydaného Azure AD. Tento požadavek deklarace identity musí být zadán v konfiguraci krok služeb doručování licence více technologiemi DRM.
7. Vytvoření aplikace ASP.NET MVC, který bude hostitelem vaší přehrávání videa. Tato aplikace ASP.NET, budou chráněné s ověřováním uživatele proti hello klienta Azure Active Directory. Správné deklarací identity bude součástí hello přístupové tokeny získat po ověření uživatele. Pro tento krok se doporučuje OpenID Connect rozhraní API. Je třeba tooinstall hello následující balíčky NuGet:
   * Install-Package Microsoft.Azure.ActiveDirectory.GraphClient
   * Install-Package Microsoft.Owin.Security.OpenIdConnect
   * Install-Package Microsoft.Owin.Security.Cookies
   * Instalovat balíček Microsoft.Owin.Host.SystemWeb
   * Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
8. Vytvoření přehrávač pomocí [rozhraní API služby Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/). [Rozhraní API Azure Media Player ProtectionInfo](http://amp.azure.net/libs/amp/latest/docs/) vám umožní toospecify které toouse technologie DRM na jiné platformě DRM.
9. Testovací matrice:

| **DRM** | **Prohlížeč** | **Výsledek nárok uživatele** | **Výsledek pro zrušení nárok uživatele** |
| --- | --- | --- | --- |
| **PlayReady** |MS Edge nebo IE11 v systému Windows 10 |Úspěšné |Selhání |
| **Widevine** |Chrome ve Windows 10 |Úspěšné |Selhání |
| **FairPlay** |Bude doplněno | | |

George Trifonov Azure Media Services týmu byl zápis blog, který poskytuje podrobné pokyny k nastavení Azure Active Directory pro aplikaci ASP.NET MVC player: [integrovat Azure Media Services OWIN MVC na základě aplikace pomocí Azure Active Directory a omezit obsah na základě deklarací JWT doručení klíče](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

George také zapisují blog na [ověření pomocí tokenu JWT v Azure Media Services a dynamickým šifrováním](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/). A tady je svůj [ukázku v Azure AD integraci s Azure Media Services doručení klíče](https://github.com/AzureMediaServicesSamples/Key-delivery-with-AAD-integration/).

Informace o službě Azure Active Directory:

* Můžete najít informace pro vývojáře v [Azure Active Directory – Příručka vývojáře](../active-directory/active-directory-developers-guide.md).
* Můžete najít informace pro správce v [spravovat váš adresář Azure AD](../active-directory/active-directory-administer.md).

### <a name="some-gotchas-in-implementation"></a>Některé gotchas v implementaci
V implementaci hello nejsou některé "gotchas". Zpravidla hello následující seznam "gotchas" vám může pomoci řešení problémů v případě, že narazíte na problémy.

1. **Vystavitel** adresa URL musí končit **"/"**.  

    **Cílová skupina** by měl být hello ID klienta aplikace přehrávače a měli byste také přidat **"/"** na konci hello URL vystavitele hello.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />

    V [JWT Decoder](http://jwt.calebb.net/), měli byste vidět **oblast** a **iss** jako níže v tokenu JWT hello:

    ![1. gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)
2. Přidejte oprávnění toohello aplikací v AAD (na kartě Konfigurace aplikace hello). To je potřeba pro každou aplikaci (místní a nasazené verze).

    ![gotcha 2.](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)
3. Použijte hello správné vystavitele v nastavení dynamické šifrování CENC ochrany:

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>

    Hello následující funkce nebudou fungovat:

        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />

    Hello GUID je ID hello AAD klienta. Hello GUID naleznete v automaticky otevřeném okně koncových bodů na portálu Azure.
4. Členství ve skupině udělit deklarací oprávnění. Zajistěte, aby v souboru manifestu aplikace AAD, že máme následující hello

    "groupMembershipClaims": "Vše", (hello výchozí hodnota je null)
5. Nastavení správné TokenType při vytváření omezení požadavků.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Vzhledem k tomu, že přidání podpory JWT (AAD) kromě tooSWT (ACS), je výchozí hello TokenType TokenType.JWT. Pokud používáte SWT/ACS, je nutné nastavit tooTokenType.SWT.

## <a name="additional-topics-for-implementation"></a>Další témata pro implementaci
Potom jsme se disk uss některá další témata v našem návrhu a implementace.

### <a name="http-or-https"></a>Protokol HTTP nebo HTTPS?
Hello player aplikace ASP.NET MVC, které vytvoříme, musí podporovat hello následující:

1. Ověření uživatele prostřednictvím služby Azure AD, který se musí toobe pod HTTPS;
2. Exchange tokenu JWT mezi klientem a Azure AD, který se musí toobe pod HTTPS;
3. Získání licence DRM hello klienta, který je požadovaný toobe v rámci protokolu HTTPS, pokud se službou Azure Media Services poskytuje doručování licencí. Samozřejmě sady produktů PlayReady nenutí HTTPS pro doručování licencí. Pokud je server licence PlayReady mimo Azure Media Services, by bylo možné pomocí protokolu HTTP nebo HTTPS.

Proto hello aplikace přehrávače ASP.NET použije HTTPS jako osvědčený postup. To znamená hello přehrávač médií Azure bude na stránce v rámci protokolu HTTPS. Ale pro streamování jsme raději HTTP, proto potřebujeme tooconsider smíšeného obsahu problém.

1. Prohlížeč nepovoluje smíšený obsah. Můžete ale povolit moduly plug-in, jako je modul plug-in Silverlight a OSMF pro protokol smooth a POMLČKY. Potíže se zabezpečením je smíšený obsah – to je z důvodu toohello hrozeb z hello možnost tooinject škodlivý JS, které mohou způsobovat hello toobe dat zákazníka v ohrožení.  To prohlížeče blokovat ve výchozím nastavení a, pokud je hello pouze způsob toowork kolem něj na straně serveru (původ) hello, tooallow všechny domény (bez ohledu na to https nebo http). To je pravděpodobně není vhodné buď.
2. Jsme byste neměli smíšený obsah: jak používat protokol HTTP nebo oba používat protokol HTTPS. Při přehrávání smíšený obsah, technická silverlightSS uděláte zrušením smíšený obsah upozornění. Technická flashSS zpracovává smíšený obsah bez upozornění smíšený obsah.
3. Pokud před srpen 2014 byl vytvořen koncový bod streamování, nebudou podporovat protokol HTTPS. V takovém případě prosím vytvořit a použít nový koncový bod streamování pro protokol HTTPS.

V implementaci odkaz hello DRM chráněný obsah v části HTTTPS bude aplikace a vysílání datového proudu. Otevřít obsah, hello player nemusí ověřování nebo licenci, abyste získali hello liberty toouse buď HTTP nebo HTTPS.

### <a name="azure-active-directory-signing-key-rollover"></a>Azure Active Directory podepisování výměna klíče
Jde důležité tootake bodu v úvahu vaší implementace. Pokud jste to nepovažujte v implementaci, hello dokončit systému nakonec přestanou fungovat úplně do maximálně 6 týdnů.

Azure AD používá oborový standard tooestablish důvěryhodnosti mezi samostatně a aplikací pomocí služby Azure AD. Konkrétně Azure AD používá podpisový klíč, který se skládá z pár veřejného a privátního klíče. Azure AD vytvoří token zabezpečení, který obsahuje informace o uživateli hello, je tento token podepsána pomocí jeho privátní klíč před odesláním zpět toohello aplikace Azure AD. tooverify, který hello token je platný, ve skutečnosti původu z Azure AD, hello aplikace musíte ověřit hello token podpis pomocí veřejného klíče hello vystavené Azure AD, která je součástí dokument federačních metadat hello klienta. Tento veřejný klíč – a hello podpisový klíč, ze kterého pochází – je hello shoduje s klíčem pro všechny klienty ve službě Azure AD.

Podrobné informace o výměně klíčů Azure AD najdete v dokumentu hello: [důležité informace o podepisování výměny klíče ve službě Azure AD](../active-directory/active-directory-signing-key-rollover.md).

Mezi hello [pár veřejného a privátního klíče](https://login.microsoftonline.com/common/discovery/keys/),

* Při použití privátního klíče Hello pomocí Azure Active Directory toogenerate JWT token;
* Hello veřejný klíč slouží aplikací, třeba služeb doručování licencí DRM v tokenu JWT hello AMS tooverify;

Azure Active Directory za účelem zabezpečení, otočí tento certifikát pravidelně (každých 6 týdny). V případě porušení zabezpečení může dojít, výměna klíče hello kdykoli. Proto služeb doručování licence hello v AMS potřebovat tooupdate hello veřejný klíč používaný jako Azure AD otočí hello pár klíčů, v opačném případě se nezdaří ověření pomocí tokenu v AMS a budou vydávat žádná licence.

Toho dosáhnete pomocí nastavení TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument při konfiguraci služeb doručování licencí DRM.

Hello tok tokenu JWT je jako níže:

1. Azure AD vydá hello JWT token s hello aktuální privátní klíč pro ověřené uživatele.
2. Když přehrávač uvidí šifrování CENC s více technologiemi DRM chráněný obsah, bude nejprve vyhledat hello tokenu JWT vydaného Azure AD.
3. přehrávač Hello odešle žádost o získání licence se služeb doručování toolicense tokenu JWT hello v AMS;
4. Hello služeb doručování licencí v AMS použije hello aktuální nebo platný veřejný klíč z tokenu JWT tooverify hello Azure AD, před vydáním licence.

Služeb doručování licencí DRM bude vždy kontroluje hello aktuální nebo platný veřejný klíč z Azure AD. veřejný klíč Hello předloží Azure AD bude hello je klíč používaný k ověřování tokenů JWT tokenem vydaným službou Azure AD.

Co v případě výměny klíčů hello se stane po generuje JWT token AAD, ale před hello JWT token odesílají přehrávače tooDRM služeb doručování licencí v AMS pro ověření?

Vzhledem k tomu, že v každém okamžiku může být vrácena klíč, je vždy více než jeden platný veřejný klíč k dispozici v dokumentu metadat federace hello. Doručování licencí Azure Media Services můžete použít některou z hello zadány klíče v dokumentu hello vzhledem k tomu, že jeden klíč může být vrácena brzy, jiné může být jeho nahrazení a tak dále.

### <a name="where-is-hello-access-token"></a>Kde je hello tokenu přístupu?
Pokud se podíváte na tom, jak webová aplikace volá aplikaci API v části [identita aplikace s udělení pověření klienta OAuth 2.0](../active-directory/develop/active-directory-authentication-scenarios.md#web-application-to-web-api), tok ověřování hello je jako níže:

1. Uživatel je přihlášený tooAzure AD ve webové aplikaci hello (viz hello [webový prohlížeč tooWeb aplikace](../active-directory/develop/active-directory-authentication-scenarios.md#web-browser-to-web-application).
2. koncový bod autorizace Hello Azure AD přesměruje hello uživatelského agenta zpět toohello klientská aplikace s autorizační kód. Hello uživatelský agent vrátí identifikátor URI pro přesměrování autorizační kód toohello klientské aplikace.
3. Hello webová aplikace musí tooacquire token přístupu, aby mohli ověřit toohello webového rozhraní API a načíst hello požadovaného prostředku. Umožňuje tooAzure požadavek na AD koncovému bodu tokenu, poskytnutí přihlašovacích údajů hello, ID klienta a identifikátor ID URI webové rozhraní API aplikace. Představuje hello autorizační kód tooprove této hello uživatelem.
4. Azure AD ověřuje hello aplikace a vrátí JWT přístupový token, který je použité toocall hello webového rozhraní API.
5. Přes protokol HTTPS hello webová aplikace používá hello vrácená JWT přístup k tokenu tooadd hello JWT řetězec, který má označení "Nosiče" v hlavičce autorizace hello hello požadavek toohello webových rozhraní API. Hello webového rozhraní API pak ověří hello JWT token, a pokud je ověření úspěšné, vrátí hello požadovaného prostředku.

V tomto toku "Identita aplikace" hello webového rozhraní API důvěřuje tohoto hello webové aplikace hello ověřeného uživatele. Z tohoto důvodu se tento vzor nazývá důvěryhodný subsystém. Hello [diagramu na této stránce](https://docs.microsoft.com/azure/active-directory/active-directory-protocols-oauth-code) popisuje, jak funguje tok poskytování autorizačních kódů.

V získání licence s omezení s tokenem, jsme jsou následující hello stejného vzoru důvěryhodný subsystém. A hello licenční doručení služby ve službě Azure Media Services je hello webového rozhraní API prostředku hello "prostředek back-end" Webová aplikace musí tooaccess. Kde je proto hello přístupový token?

Skutečně jsme se získání tokenu přístupu z Azure AD. Po úspěšné ověření uživatele je vrácen autorizační kód. Hello autorizační kód se pak použije, společně s aplikací a ID klienta klíčem, tooexchange pro přístupový token. A hello přístupový token je pro přístup k aplikaci "ukazatel" ukazující nebo představuje službu doručování licencí Azure Media Services.

Budeme potřebovat tooregister a konfigurace hello "ukazatel" aplikace ve službě Azure AD podle následujících kroků hello:

1. V klientovi hello Azure AD

   * Přidání aplikace (prostředků) s adresou URL přihlašování:

   https://[resource_name].azurewebsites.NET/ a

   * Adresa URL ID aplikace:

   https://[aad_tenant_name].onmicrosoft.com/[resource_name];
2. Přidejte nový klíč pro aplikaci hello prostředků;
3. Aktualizujte soubor manifestu aplikace hello tak, aby vlastnost groupMembershipClaims hello má hello následující hodnoty: "groupMembershipClaims": "Vše",  
4. V aplikaci Azure AD hello odkazující toohello player webové aplikace, v části hello "oprávnění tooother aplikace" přidejte hello prostředků aplikace, která byla přidána v kroku 1 výše. V části "delegovaná oprávnění" zkontrolujte zaškrtnutí "Přístup [resource_name]". To dává oprávnění webové aplikace hello toocreate přístupových tokenů pro přístup k prostředku aplikace hello. Měli byste to provést pro místní i nasazené verze webové aplikace hello Pokud vyvíjíte pomocí sady Visual Studio a Azure webové aplikace.

Proto hello tokenu JWT vydaného Azure AD, je skutečně hello přístupového tokenu pro přístup k tomuto prostředku "ukazatel".

### <a name="what-about-live-streaming"></a>Co se chystáte živě Streamovat?
V hello výše má byla tato diskuse zaměřovat na prostředky na vyžádání. Co se chystáte živé streamování?

Hello Dobrá zpráva je, že můžete použít přesně hello stejné návrhu a implementace pro ochranu živé streamování ve službě Azure Media Services tak, že považuje hello asset přidružený program jako "VOD asset".

Konkrétně je dobře známý, toodo živé streamování ve službě Azure Media Services, budete potřebovat toocreate kanál a potom program v rámci kanálu hello. programu hello toocreate, musíte toocreate prostředek, který bude obsahovat archivu živé hello programu hello. V pořadí tooprovide CENC s více technologiemi DRM ochranu živý obsah hello, stačí toodo, je tooapply hello stejné asset toohello instalace/zpracování jako kdyby byl "VOD asset" před spuštěním programu hello.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Co licenčních serverů mimo Azure Media Services?
Zákazníci mohou často investovaly do licence serverové farmy buď v svá vlastní data center nebo hostované poskytovateli služeb DRM. Naštěstí Azure Media Services obsahu Protection vám umožní toooperate v režimu hybridní: obsah hostované a dynamicky chráněné v Azure Media Services, zatímco licence DRM se dodávají servery mimo Azure Media Services. V takovém případě jsou hello následující aspekty změny:

1. Hello služby tokenů zabezpečení potřebám tooissue tokeny, které jsou přijatelné a lze ověřit pomocí hello licence serverové farmy. Například servery licence Widevine hello poskytované Axinom vyžaduje konkrétní token JWT, který obsahuje "nárocích zprávu". Proto musíte toohave tokenů zabezpečení tooissue takový token JWT. Autoři Hello dokončili takové implementace a hello informace naleznete v hello následující dokument v [centru dokumentace Azure](https://azure.microsoft.com/documentation/): [pomocí Axinom toodeliver Widevine licencí tooAzure Media Services](media-services-axinom-integration.md).
2. Již nepotřebujete tooconfigure licenční doručení služby (ContentKeyAuthorizationPolicy) ve službě Azure Media Services. Co je třeba toodo je tooprovide hello licence získání adresy URL (PlayReady, Widevine a FairPlay) při konfiguraci AssetDeliveryPolicy v nastavení šifrování CENC s více technologiemi DRM.

### <a name="what-if-i-want-toouse-a-custom-sts"></a>Co když chci, aby toouse vlastní službu tokenů zabezpečení?
Může dojít z několika důvodů, které si zákazník může vybrat toouse vlastních tokenů zabezpečení (Secure Token Service) pro poskytování tokenů JWT. Některé z nich jsou:

1. Hello zprostředkovatele Identity (IDP) používané hello zákazníka nepodporuje službu tokenů zabezpečení. Vlastní službu tokenů zabezpečení v takovém případě může být možnost.
2. Hello zákazníka může být nutné flexibilní nebo náročnější ovládací prvek v integraci služby tokenů zabezpečení do odběratele zákazníka účetním systémem. Například MVPD operátor může nabízet více balíčků OTT odběratele, jako je premium a basic, sportovní, operátor hello atd. může být vhodné toomatch hello deklarace do tokenu s balíčkem odběratele tak, aby pouze obsah v balíčku správné hello je k dispozici. V takovém případě vlastní službu tokenů zabezpečení poskytuje hello potřeby flexibilitu a řízení.

Dvě změny potřebovat toobe provedené při použití vlastních tokenů zabezpečení:

1. Pokud nakonfigurujete službu doručování licencí pro určitý prostředek, je potřeba toospecify hello zabezpečení klíč používaný k ověření službou hello vlastních tokenů zabezpečení (Další informace níž) namísto hello aktuální klíč ze služby Azure Active Directory.
2. Když se vygeneruje JTW token, je určen klíč zabezpečení místo hello privátní klíč certifikátu hello aktuální X509 v Azure Active Directory.

Existují dva typy bezpečnostních klíčů:

1. Symetrický klíč: hello stejný klíč se používá pro generování a ověřování token JWT;
2. Asymetrický klíč: páru veřejného a privátního klíče RSA ve X509 certifikát se používá s privátním klíčem pro šifrování nebo generování tokenů JWT token a hello veřejný klíč k ověření tokenu hello.

#### <a name="tech-note"></a>Technická poznámka
Pokud používáte rozhraní .NET Framework / C# jako svou platformu vývoj hello X509 certifikát používaný pro zabezpečení asymetrický klíč musí mít klíč délku aspoň 2048. Toto je požadavek třídy hello System.IdentityModel.Tokens.X509AsymmetricSecurityKey v rozhraní .NET Framework. Jinak bude vyvolána hello následující výjimky:

IDX10630: hello 'System.IdentityModel.Tokens.X509AsymmetricSecurityKey' pro podepisování nemůže být menší než '2048' bits.

## <a name="hello-completed-system-and-test"></a>Hello dokončit systému a testování
Vám ukážeme v systému začátku do konce hello dokončit několik scénářů, tak, aby čtečky může mít základní "formát" hello chování před získáním účet přihlášení.

Hello player webové aplikace a její přihlášení najdete [zde](https://openidconnectweb.azurewebsites.net/).

Pokud je třeba je "zjistili" scénář: video prostředky hostované ve službě Azure Media Services, které jsou buď nechráněné nebo DRM chráněný, ale bez ověření pomocí tokenu (vydávání licencí toowhoever, o to požádá), můžete otestovat ji bez přihlášení (podle přepínání tooHTTP Pokud video streamování přes protokol HTTP).

Pokud je třeba je integrovaný scénář začátku do konce: video prostředky je v části dynamické DRM ochrany ve službě Azure Media Services pomocí tokenu ověření a token JWT generován službou Azure AD, je třeba toologin.

### <a name="user-login"></a>Přihlášení uživatele
Pořadí tootest hello začátku do konce integrované DRM systému je nutné toohave "účet", nebo přidat.

Který účet?

I když služba Azure původně umožňovala přístup pouze uživatelé s účtem Microsoft, nyní umožňuje přístup uživatelům z obou systémů. K tomu bylo potřeba tak, že všechny vlastnosti služby azur hello Azure AD pro ověřování s Azure AD, která ověřuje organizační uživatele a tím, že vytvoříte vztah federace kde Azure AD důvěřuje systémem identit uživatelů účtů Microsoft hello tooauthenticate spotřebitelské uživatele. Azure AD v důsledku toho je možné tooauthenticate účty Microsoft "guest" a také "nativní" účty Azure AD.

Vzhledem k tomu, že Azure AD důvěřuje domény účtu Microsoft (MSA), můžete přidat všechny účty z jakéhokoli z následujících domén toohello vlastní Azure AD hello klienta a použijte účet toologin hello:

| **Název domény** | **Domény** |
| --- | --- |
| **Doména klienta vlastní Azure AD** |somename.onmicrosoft.com |
| **Podnikové doméně** |Microsoft.com |
| **Účet Microsoft (MSA) domény** |live.com, Outlook.com, hotmail.com |

Může kontaktovat kterýkoli z hello autoři toohave účet vytvořit nebo přidat za vás.

Níže jsou snímky obrazovky hello stránek jiné přihlašovací údaje používané účty v jiné doméně.

**Vlastní Azure AD klienta účet domény**: V tomto případě vidíte hello přizpůsobená přihlašovací stránku domény klienta hello vlastní Azure AD.

![Účet domény klienta vlastní Azure AD](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Microsoft doménový účet s čipovou kartu**: V tomto případě vidíte hello přihlašovací stránky přizpůsobený podle Microsoft podnikové IT s dvoufaktorové ověřování.

![Účet domény klienta vlastní Azure AD](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Účet Microsoft (MSA)**: V tomto případě se zobrazí stránka přihlášení hello Account Microsoft pro spotřebitele.

![Účet domény klienta vlastní Azure AD](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>Pomocí rozšíření šifrované média pro PlayReady
Moderní prohlížeč s šifrované média rozšíření (EME) pro podporu, jako je třeba prohlížeč Internet Exploreru 11 na Windows 8.1 nebo vyšší a Microsoft Edge ve Windows 10, PlayReady bude PlayReady hello základní DRM pro EME.

![Použití EME pro PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

oblast tmavý player Hello je z důvodu toohello fakt, že PlayReady ochrana chrání jeden provádění snímek obrazovky chráněné videa z.

Hello následující obrazovka ukazuje hello player modulů plug-in a MSE/EME podpory.

![Použití EME pro PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

EME v Microsoft Edge a aplikace Internet Explorer 11 ve Windows 10 umožňuje vyvolání z [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) na zařízení s Windows 10, které ji podporují. PlayReady SL3000 odemkne hello toku obsahu rozšířené premium (4K, záhlaví, atd.) a nové modely doručování obsahu (časná okna pro rozšířený obsah).

Zaměřit se na zařízení se systémem Windows hello: PlayReady je hello pouze DRM v HW hello k dispozici na zařízení se systémem Windows (PlayReady SL3000). Streamování služby můžete použít PlayReady prostřednictvím EME nebo aplikaci UWP a nabízí vyšší kvalitu videa pomocí PlayReady SL3000 než jiné DRM. Obvykle 2K obsahu budou procházet skrz Chrome nebo Firefox a 4 kB obsahu prostřednictvím Microsoft Edge/IE11 nebo aplikace pro UPW v hello stejného zařízení (v závislosti na nastavení služby a implementaci).

#### <a name="using-eme-for-widevine"></a>Použití EME pro Widevine
Moderní prohlížeč s podporou EME nebo Widevine, jako je například Chrome 41 + na Windows 10, Windows 8.1, Mac OSX Yosemite a Chrome na Android bodu 4.4.4 je Google Widevine DRM za EME hello.

![Použití EME pro Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Všimněte si, že Widevine nebrání z provedení snímek obrazovky chráněné videa.

![Použití EME pro Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Není nárok uživatelé
Pokud uživatel není členem skupiny "Uživatelé s názvem", hello uživatel nebude moct toopass "Kontrola nárocích" a hello více technologiemi DRM licenční služby odmítne tooissue hello požadovanou licenci, jak je uvedeno níže. Hello podrobný popis je "získat licenci se nezdařilo", což je tak, jak má.

![Zrušení nárok uživatelé](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Spuštění vlastního tokenu služby Zabezpečené
Pro scénář hello spuštěných vlastní zabezpečení tokenu služby (STS) bude hello JWT token vystavené službou tokenů zabezpečení vlastní hello pomocí buď symetrický, nebo asymetrický klíč.

Hello případ použití symetrického klíče (pomocí chromu):

![Spuštění vlastní službu tokenů zabezpečení](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

Hello případ použití asymetrický klíč prostřednictvím X509 certifikátu (s použitím moderní prohlížeče Microsoft).

![Spuštění vlastní službu tokenů zabezpečení](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

V obou hello výše případech hello uživatel ověřování zůstane stejný – prostřednictvím služby Azure AD. Hello jediným rozdílem je, že tokeny JWT vydal hello vlastní službu tokenů zabezpečení místo Azure AD. Samozřejmě, při konfiguraci ochrany dynamické šifrování CENC, omezení hello služeb doručování licence Určuje typ hello JWT token, buď symetrický, nebo asymetrický klíč.

## <a name="summary"></a>Souhrn
V tomto dokumentu jsme probrali CENC s více native DRM a řízení přístupu prostřednictvím tokenu ověřování: návrh a jeho implementace pomocí Azure, Azure Media Services a Azure Media Player.

* Návrh odkazu se zobrazí, který obsahuje všechny komponenty potřebné hello v subsystému DRM nebo CENC;
* Odkaz na implementaci na Azure, Azure Media Services a Azure Media Player.
* Některá témata přímo účastní hello návrhu a implementace jsou také popsány.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 
