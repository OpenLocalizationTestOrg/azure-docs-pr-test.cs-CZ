---
title: "aaaHow toouse řízení přístupu (Java) | Microsoft Docs"
description: "Zjistěte, jak toodevelop a použití řízení přístupu s Java v Azure."
services: active-directory
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 247dfd59-0221-4193-97ec-4f3ebe01d3c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.custom: aaddev
ms.openlocfilehash: cbbce3b1a05eabf3b86a8cb91db1bde92dbb8960
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooauthenticate-web-users-with-azure-access-control-service-using-eclipse"></a>Jak tooAuthenticate webovým uživatelům s Azure přístup k řízení služby pomocí Eclipse
Tento průvodce vám ukáže, jak toouse hello služby Řízení přístupu Azure (ACS) v rámci hello nástrojů Azure pro prostředí Eclipse. Další informace o ACS najdete v tématu hello [další kroky](#next_steps) části.

> [!NOTE]
> Hello filtru řízení služeb Azure přístup je náhled technologie komunity. Jako předběžné verze softwaru není oficiálně společností Microsoft.
> 
> 

## <a name="what-is-acs"></a>Novinky služby ACS
Většina vývojářů nejsou odborníky identity a obecně nechcete zatěžovat vývoj mechanismy ověřování a autorizace na čas pro své aplikace a služby. Služby ACS je služba Azure, která poskytuje snadný způsob ověřování uživatelů, kteří potřebují tooaccess webových aplikací a služeb bez nutnosti toofactor komplexní ověřování logika do kódu.

Hello následující funkce jsou k dispozici v rámci služby ACS:

* Integrace s Windows Identity Foundation (WIF).
* Podpora zprostředkovatelů identity oblíbených webových (IP) včetně Windows Live ID, Google, Yahoo! a Facebook.
* Podpora pro Active Directory Federation Services (AD FS) 2.0.
* Open Data Protocol (OData) – na základě služba správy, které zajišťují programový přístup tooACS nastavení.
* Portál správy, který umožňuje přístup pro správu nastavení toohello služby ACS.

Další informace o ACS najdete v tématu [2.0 služby Řízení přístupu][Access Control Service 2.0].

## <a name="concepts"></a>Koncepty
Azure služby ACS je založen na hello objektů na základě deklarace identity – mechanismy ověřování konzistentní přístup toocreating, pro aplikace spuštěné místně nebo v cloudu hello. Na základě deklarace identity poskytuje pro aplikace a služby tooacquire, které potřebují identity informací o uživatelích v jejich organizace v jiných organizacích a na Internetu hello běžným způsobem.

toocomplete hello úlohy v této příručce, byste měli vědět hello následující koncepty:

**Klient** – v kontextu hello tento tooguide postupy, je to prohlížeč, který se pokouší toogain přístup tooyour webové aplikace.

**Aplikace předávající stranu** – RP aplikace je webu nebo službu, která outsources externí autority tooone ověřování. V žargonu identity říkáme, že tento hello RP důvěřuje této autoritě. Tato příručka vysvětluje, jak tooconfigure tootrust vaše aplikace služby ACS.

**Token** -token je kolekce dat zabezpečení, která se obvykle objeví po úspěšném ověření uživatele. Obsahuje sadu *deklarace identity*, atributy hello ověřeného uživatele. Deklarace identity může představovat uživatelské jméno, identifikátor pro roli uživatele patří do, stáří uživatele a tak dále. Token je obvykle digitálně podepsané, což znamená, může být vždy vystavitele z domácích zdrojů zpět tooits a její obsah nemůže být úmyslně poškozena. Uživatel dostane tooa RP aplikace access prezentací platný token vydán autoritou, která důvěřuje hello RP aplikace.

**Zprostředkovatel identity (IP)** -IP je autoritu, která ověřuje identity uživatelů a vydává tokeny zabezpečení. Hello samotnou práci vydává tokeny se implementuje, když speciální služba s názvem tokenu služby zabezpečení (STS). Typické příklady IP adres patří Windows Live ID, Facebook, obchodní uživatele úložiště (třeba služby Active Directory) a tak dále.
Když služby ACS je nakonfigurované tootrust IP adresy, systém hello přijmout a ověřit tokeny vydané tuto IP. ACS může důvěřovat více IP adres najednou, což znamená, že pokud vaše aplikace důvěřuje služby ACS, můžete okamžitě nabízejí hello tooall vaší aplikace ověřené uživatele z všechny hello IP adresy této služby ACS vztahy důvěryhodnosti vaším jménem.

**Federační zprostředkovatel (FP)** -IP adresy mají přímý znalosti o uživatele a ověřit jejich pomocí svých přihlašovacích údajů a vydat deklarace identity o vědí o nich. Federační zprostředkovatel (FP) je jiný druh autority: místo přímo ověřování uživatelů, funguje jako zprostředkovatel a zprostředkovatelé ověřování mezi jeden RP a jedním nebo více IP adres. IP adresy a FPs vystavovat tokeny zabezpečení, proto používají obě tokenu služby zabezpečení (STS). Služby ACS je jeden FP.

**Modul pravidel ACS** -pravidla transformace deklarací logiku hello použité tootransform příchozí tokeny od důvěryhodných IP adres tootokens určená toobe spotřebovávají hello RP je právně upraveny v formu jednoduché. Služba ACS funkce stroj pravidel, která má na starosti použití libovolnou logiku transformace jste zadali pro vaše RP.

**Namespace ACS** – obor názvů je nejvyšší úrovně oddílu služby ACS, který používáte pro uspořádání nastavení. Obor názvů obsahuje seznam IP adres, které důvěřujete, aplikace hello RP tooserve hello pravidla, které očekáváte, že chcete hello pravidlo modul tooprocess příchozí tokeny a tak dále. Obor názvů zpřístupní různé koncové body, které budou používat aplikace hello a tooperform tooget ACS vývojáře jeho funkce.

Hello následující obrázek ukazuje, jak funguje ověřovací služby ACS s webovou aplikací:

![Vývojový diagram služby ACS][acs_flow]

1. Hello klienta (v tomto případě a prohlížeče) stránka žádosti z hello RP.
2. Vzhledem k tomu, že žádost o hello ještě není ověřen, přesměruje hello RP hello uživatele toohello autoritu, která důvěřuje, což je služby ACS. Hello ACS uvede hello uživatele s volbou hello z IP adresy, které bylo zadáno pro tento RP. Hello uživatel vybere odpovídající IP hello.
3. Klient Hello umožňuje toohello IP ověřovací stránku a vyzve uživatele toolog hello na.
4. Po ověření hello klienta (například hello identity, které pověření se zadávají) hello IP vydá token zabezpečení.
5. Po vydání tokenu zabezpečení, hello IP přesměruje klienta tooACS hello a hello klient odešle hello tokenu zabezpečení vydaného hello IP tooACS.
6. Služba ACS ověří hello tokenu zabezpečení vydaného hello IP adresy, vstupy hello identitu deklarace identity ve tento token do stroj pravidel hello služby ACS, vypočítá deklarace identity výstup hello a vydá nový token zabezpečení, která obsahuje tyto deklarace výstupu.
7. Služba ACS přesměruje klienta toohello hello RP. Hello klient odešle hello nový ACS toohello RP vystavený token zabezpečení. Hello RP ověří podpis hello na hello tokenu zabezpečení vydaného služby ACS, ověří hello deklarací identity v tento token a vrátí stránku hello, která původně požádal.

## <a name="prerequisites"></a>Požadavky
toocomplete hello úlohy v této příručce, budete potřebovat následující hello:

* Java Developer Kit (JDK), v 1.6 nebo novější.
* Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE, džínovinu nebo novější. To si můžete stáhnout z <http://www.eclipse.org/downloads/>. 
* Distribuce založené na jazyce Java webový server nebo aplikačního serveru, jako je například Apache Tomcat, GlassFish, aplikační Server JBoss nebo Jetty.
* předplatné Azure, který můžete získat z <http://www.microsoft.com/windowsazure/offers/>.
* Hello nástrojů Azure pro prostředí Eclipse. dubna 2014 verzi nebo novější. Další informace najdete v tématu [hello instalace nástrojů Azure pro Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx).
* Toouse certifikát X.509 s vaší aplikací. Je nutné tento certifikát veřejného certifikátu (.cer) a Personal Information Exchange (. Formát PFX). (Možnosti pro vytvoření tohoto certifikátu bude popsané později v tomto kurzu).
* Znalost hello Azure výpočetní emulátor a nasazení techniky popsané v [vytvoření aplikace Hello World služby Azure v prostředí Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx).

## <a name="create-an-acs-namespace"></a>Vytvoření služby ACS Namespace
toobegin pomocí služby Řízení přístupu (ACS) v Azure, je nutné vytvořit na obor názvů služby ACS. obor názvů Hello poskytuje jedinečný obor pro adresování prostředků služby ACS z v rámci vaší aplikace.

1. Přihlaste se k hello [portálu pro správu Azure][Azure Management Portal].
2. Klikněte na tlačítko **služby Active Directory**. 
3. toocreate nový obor názvů řízení přístupu, klikněte na tlačítko **nový**, klikněte na tlačítko **App Services**, klikněte na tlačítko **řízení přístupu**a potom klikněte na **rychle vytvořit** . 
4. Zadejte název pro obor názvů hello. Azure ověřuje, zda text hello jméno jedinečný.
5. Vyberte hello oblast, ve které hello se používá obor názvů. Pro zajištění nejlepšího výkonu hello použijte hello oblast, ve kterém jsou nasazení aplikace.
6. Pokud máte více než jedno předplatné, vyberte předplatné hello chcete toouse pro obor názvů hello služby ACS.
7. Klikněte na možnost **Vytvořit**.

Azure vytvoří a aktivuje hello oboru názvů. Počkejte, dokud je stav hello nový obor názvů hello **Active** než budete pokračovat. 

## <a name="add-identity-providers"></a>Přidat zprostředkovatele identity
V této úloze přidejte toouse IP adresy s vaší aplikací RP pro ověřování. Pro demonstrační účely tato úloha ukazuje, jak může tooadd Windows Live jako IP adresy, ale můžete použít některou z hello IP adresy uvedené v hello portálu pro správu služby ACS.

1. V hello [portálu pro správu Azure][Azure Management Portal], klikněte na tlačítko **služby Active Directory**, vyberte na obor názvů řízení přístupu a pak klikněte na tlačítko **spravovat**. Otevře se Hello portálu pro správu služby ACS.
2. V levém navigačním podokně hello hello portálu pro správu služby ACS, klikněte na **zprostředkovatelů Identity**.
3. Windows Live ID ve výchozím nastavení zapnutá a nelze ji odstranit. Pro účely tohoto kurzu se používá pouze Windows Live ID. Tato obrazovka se ale, kde můžete přidat další IP adresy kliknutím hello **přidat** tlačítko.

Windows Live ID je teď povolené jako IP adresy pro obor názvů služby ACS. Dále určit webové aplikace Java (toobe vytvořit později) jako RP.

## <a name="add-a-relying-party-application"></a>Přidání aplikace předávající strany
V této úloze nakonfigurujete ACS toorecognize webové aplikace v jazyce Java jako platnou aplikaci RP.

1. Na portálu pro správu služby ACS hello, klikněte na tlačítko **aplikace předávající strany**.
2. Na hello **aplikace předávající strany** klikněte na tlačítko **přidat**.
3. Na hello **přidat aplikaci předávající strany** stránky, hello následující:
   
   1. V **název**, název typu hello hello RP. Pro účely tohoto kurzu, zadejte **webové aplikace Azure**.
   2. V **režimu**, vyberte **zadejte nastavení ručně**.
   3. V **sféry**, použije typ hello URI toowhich hello zabezpečení tokenem vydaným službou ACS. Pro tuto úlohu, zadejte **adrese http://localhost: 8080 /**.
      ![Předávající strany sféru pro použití v emulátoru služby výpočty][relying_party_realm_emulator]
   4. V **vrátí adresu URL,** typ hello URL toowhich ACS vrátí hello tokenu zabezpečení. Pro tuto úlohu, zadejte **http://localhost:8080/MyACSHelloWorld/index.jsp**
      ![předávající strany návratovou adresu URL pro použití v emulátoru služby výpočty][relying_party_return_url_emulator]
   5. Přijměte výchozí hodnoty hello v hello zbytek hello pole.
4. Klikněte na **Uložit**.

Nyní jste úspěšně nakonfigurovali webové aplikace v jazyce Java při spuštění v emulátoru výpočtů Azure hello (na adrese http://localhost: 8080 /) toobe RP v oboru názvů služby ACS. Dále vytvořte pravidla hello ACS používá tooprocess deklarace pro hello RP.

## <a name="create-rules"></a>Vytvoření pravidel
V této úloze definujete hello pravidel, která určují, jak se předávají deklarace identity z IP adresy tooyour RP. Pro hello účel tohoto průvodce můžeme jednoduše nakonfiguruje ACS toocopy hello vstupní deklaraci identity typů a hodnot přímo v tokenu výstup hello, bez jejich změně nebo filtrování.

1. Na hlavní stránce portálu pro správu služby ACS hello klikněte **pravidla skupiny**.
2. Na hello **skupiny pravidel** klikněte na tlačítko **výchozí skupiny pravidel pro webové aplikace Azure**.
3. Na hello **upravit skupinu pravidel** klikněte na tlačítko **generování**.
4. Na hello **generováním pravidel: výchozí skupiny pravidel pro webové aplikace Azure** , zkontrolujte Windows Live ID je zaškrtnuta možnost a pak klikněte na tlačítko **generování**.    
5. Na hello **upravit skupinu pravidel** klikněte na tlačítko **Uložit**.

## <a name="upload-a-certificate-tooyour-acs-namespace"></a>Nahrajte certifikát tooyour ACS obor názvů
V této úloze nahrajete. Certifikát PFX, který bude použité toosign žádostí o token vytvořený obor názvů služby ACS.

1. Na hlavní stránce portálu pro správu služby ACS hello klikněte **certifikáty a klíče**.
2. Na hello **certifikáty a klíče** klikněte na tlačítko **přidat** výše **podepisování**.
3. Na hello **přidat Token podpisový certifikát nebo klíče** stránky:
   1. V hello **používá pro** klikněte na tlačítko **aplikaci předávající strany** a vyberte **webové aplikace Azure** (která dříve nastavená jako hello název aplikace předávající strany).
   2. V hello **typ** vyberte **certifikát X.509**.
   3. V hello **certifikát** části, klikněte na tlačítko Procházet hello a přejděte na soubor certifikátu X.509 toohello, které chcete toouse. Bude jím. Soubor PFX. Vyberte soubor hello, klikněte na tlačítko **otevřete**a pak zadejte heslo certifikátu hello v hello **heslo** textové pole. Upozorňujeme, že pro účely testování, může použít samoobslužných-signed-certifikát. toocreate certifikát podepsaný svým držitelem, použijte hello **nový** tlačítka na hello **ACS filtru knihovny** dialogové okno (popsáno dále), nebo použijte hello **encutil.exe** nástroj z hello [projektu webu] [ project website] z hello Starter Kit Azure pro jazyk Java.
   4. Ujistěte se, že **zkontrolujte primární** je zaškrtnuté. Vaše **přidat Token podpisový certifikát nebo klíče** stránka by měla vypadat podobně jako následující toohello.
       ![Přidat podpisový certifikát tokenu][add_token_signing_cert]
   5. Klikněte na tlačítko **Uložit** toosave vaše nastavení a zavřete hello **přidat Token podpisový certifikát nebo klíče** stránky.

Dále zkontrolujte hello informace hello integraci aplikací stránky a zkopírujte hello identifikátor URI, že budete potřebovat tooconfigure vaše Java webové aplikace toouse služby ACS.

## <a name="review-hello-application-integration-page"></a>Stránka Kontrola hello integraci aplikací
Všechny hello informace a potřebné tooconfigure hello kódu můžete najít váš Java webové aplikace (hello RP aplikace) toowork službou ACS na stránce integraci aplikací hello hello portálu pro správu služby ACS. Tyto informace budete potřebovat při konfiguraci webové aplikace Java federovaného ověřování.

1. Na portálu pro správu služby ACS hello, klikněte na tlačítko **integraci aplikací**.  
2. V hello **integraci aplikací** klikněte na tlačítko **přihlašovací stránky**.
3. V hello **přihlašovací stránky integrace** klikněte na tlačítko **webové aplikace Azure**.

V hello **přihlašovací stránky integrace: webové aplikace Azure** hello adresy URL uvedené v stránky **možnost 1: odkaz tooan hostované služby ACS přihlašovací stránky** se použije na webové aplikace v jazyce Java. Tuto hodnotu budete potřebovat při přidání hello filtru služeb řízení přístupu Azure knihovna tooyour aplikace v jazyce Java.

## <a name="create-a-java-web-application"></a>Vytvoření webové aplikace Java
1. V prostředí Eclipse, klikněte v nabídce hello **soubor**, klikněte na tlačítko **nový**a potom klikněte na **Dynamic Web Project**. (Pokud nevidíte **Dynamic Web Project** uvedené jako dostupné projekt po kliknutí na **soubor**, **nový**, pak hello následující: klikněte na tlačítko **soubor**, klikněte na tlačítko **nový**, klikněte na tlačítko **projektu**, rozbalte položku **webové**, klikněte na tlačítko **Dynamic Web Project**a klikněte na tlačítko  **Další**.) Pro účely tohoto kurzu, pojmenujte projekt hello **MyACSHelloWorld**. (Ujistěte se, použijte tento název, vaše toobe soubor WAR s názvem MyACSHelloWorld očekávat následné kroky v tomto kurzu). Na obrazovce zobrazí podobné toohello následující:
   
    ![Vytvoření projektu Hello, World pro exampple služby ACS][create_acs_hello_world]
   
    Klikněte na **Dokončit**.
2. V rámci zobrazení Eclipse na prohlížeči projektu rozbalte **MyACSHelloWorld**. Klikněte pravým tlačítkem na **WebContent**, pak na **New** (Nový) a nakonec na **JSP File** (Soubor JSP).
3. V hello **nový soubor JSP** dialogové okno, název souboru hello **index.jsp**. Udržovat hello nadřazené složky jako MyACSHelloWorld nebo WebContent, jak je znázorněno v následující hello:
   
    ![Přidá soubor JSP například služby ACS][add_jsp_file_acs]
   
    Klikněte na **Další**.
4. V hello **vybrat šablonu JSP** vyberte **nový soubor JSP (html)** a klikněte na tlačítko **Dokončit**.
5. Když se soubor index.jsp hello se otevře v prostředí Eclipse, přidejte v textu toodisplay **ACS Hello, World!** v rámci existující hello `<body>` elementu. Aktualizovaný `<body>` obsah se mají zobrazit jako hello následující:
   
        <body>
          <b><% out.println("Hello ACS World!"); %></b>
        </body>
   
    Uložte index.jsp.

## <a name="add-hello-acs-filter-library-tooyour-application"></a>Přidat hello ACS filtru knihovny tooyour aplikace
1. V prostředí Eclipse v prohlížeči projektu klikněte pravým tlačítkem na **MyACSHelloWorld**, klikněte na tlačítko **cesta sestavení**a potom klikněte na **konfigurovat cestu sestavení**.
2. V hello **cesta sestavení Java** dialogové okno, klikněte na tlačítko hello **knihovny** kartě.
3. Klikněte na tlačítko **přidání knihovny**.
4. Klikněte na tlačítko **Azure přístup k řízení služby filtrovat (podle otevřete technická MS)** a pak klikněte na **Další**. Hello **filtru služeb řízení přístupu Azure** se zobrazí dialogové okno.  (hello **umístění** pole může mít jinou cestu, v závislosti na tom, kam jste nainstalovali Eclipse, a číslo verze hello může být liší v závislosti na aktualizace softwaru.)
   
    ![Přidání filtru ACS knihovny][add_acs_filter_lib]
5. Pomocí prohlížeče otevřít toohello **přihlašovací stránky integrace** stránku hello portálu pro správu, kopie hello adresy URL uvedené v hello **možnost 1: odkaz tooan hostované služby ACS přihlašovací stránku** pole a vložte jej do hello **Koncový bod ACS ověřování** pole hello Eclipse dialogového okna.
6. Pomocí prohlížeče otevřít toohello **upravit aplikaci předávající strany** stránku hello portálu pro správu, kopie hello adresy URL uvedené v hello **sféry** pole a vložte jej do hello **sféry předávající strany**  pole hello Eclipse dialogového okna.
7. V rámci hello **zabezpečení** části hello dialogového okna Eclipse, pokud chcete, aby toouse existující certifikát, klikněte na tlačítko **Procházet**, přejděte toohello certifikát má toouse, vyberte ho a klikněte na tlačítko  **Otevřete**. Nebo, pokud chcete toocreate nový certifikát, klikněte na tlačítko **nový** toodisplay hello **nový certifikát** dialogové okno, zadejte heslo hello, název souboru .cer hello a název pro hello nový soubor .pfx hello certifikát.
8. Zkontrolujte **vložení hello certifikát v souboru WAR hello**. Vnoření hello certifikát tímto způsobem ji obsahuje ve vašem nasazení, aniž by bylo nutné toomanually ji přidat jako součást. (Pokud místo toho je třeba uložit vašeho certifikátu externě ze souboru WAR, můžete přidat certifikát hello jako součást role a zrušte zaškrtnutí políčka **vložení hello certifikát v souboru WAR hello**.)
9. [Nepovinné] Zachovat **připojení vyžadují HTTPS** zaškrtnutí. Pokud nastavíte tuto možnost, budete potřebovat tooaccess aplikace pomocí protokolu HTTPS hello. Pokud nechcete, aby toorequire připojení prostřednictvím protokolu HTTPS, zrušte tuto možnost.
10. Pro nasazení toohello výpočetní emulátor, vaše **Azure ACS filtru** nastavení bude vypadat podobně jako následující toohello.
    
    ![Emulátor služby výpočty Azure nastavení filtru služby ACS pro toohello nasazení][add_acs_filter_lib_emulator]
11. Klikněte na **Dokončit**.
12. Klikněte na tlačítko **Ano** při předání s dialogové okno pole s oznámením, že se vytvoří soubor web.xml.
13. Klikněte na tlačítko **OK** tooclose hello **cesta sestavení Java** dialogové okno.

## <a name="deploy-toohello-compute-emulator"></a>Nasazení emulátoru služby výpočty toohello
1. V prostředí Eclipse v prohlížeči projektu klikněte pravým tlačítkem na **MyACSHelloWorld**, klikněte na tlačítko **Azure**a potom klikněte na **balíček pro Azure**.
2. Pro **název projektu**, typ **MyAzureACSProject** a klikněte na tlačítko **Další**.
3. Vyberte JDK a aplikačního serveru. (Tyto kroky jsou podrobně popsány v hello [vytvoření aplikace Hello World služby Azure v prostředí Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) kurzu).
4. Klikněte na **Dokončit**.
5. Klikněte na tlačítko hello **spustit v emulátoru Azure** tlačítko.
6. Po spuštění webové aplikace v jazyce Java v emulátoru služby výpočty hello, zavřete všechny instance prohlížeče (tak, aby všechny aktuální relace prohlížeče nezpůsobují konflikt s testováním přihlášení služby ACS).
7. Spusťte aplikaci otevřením <adrese http://localhost: 8080/MyACSHelloWorld/> v prohlížeči (nebo <https://localhost:8080/MyACSHelloWorld/> v případě, že je zaškrtnuté **připojení vyžadují protokol HTTPS** ). Jste vyzváni k přihlášení Windows Live ID a potom by měla být provedena toohello zpáteční adresa URL zadaná pro aplikace předávající strany.
8. Po dokončení zobrazení vaší aplikace, klikněte na tlačítko hello **resetovat emulátoru Azure** tlačítko.

## <a name="deploy-tooazure"></a>Nasazení tooAzure
toodeploy tooAzure toochange hello předávající strany sféry návratové adresy URL a budete potřebovat pro obor názvů služby ACS.

1. V rámci hello portálu pro správu Azure, v hello **upravit aplikaci předávající strany** stránky, upravte **sféry** toobe hello URL adresu webu nasazené. Nahraďte **příklad** s názvem DNS hello jste zadali pro vaše nasazení.
   
    ![Předávající strany sféru pro použití v produkčním prostředí][relying_party_realm_production]
2. Upravit **adresa URL pro návrat** toobe hello adresu URL aplikace. Nahraďte **příklad** s názvem DNS hello jste zadali pro vaše nasazení.
   
    ![Předávající strany návratovou adresu URL pro použití v produkčním prostředí][relying_party_return_url_production]
3. Klikněte na tlačítko **Uložit** toosave vaše aktualizované odpovídajících stran sféry a vraťte se změní adresa URL.
4. Zachovat hello **přihlašovací stránky integrace** stránku v prohlížeči otevřít, budete se muset toocopy z něj za chvíli.
5. V prostředí Eclipse v prohlížeči projektu klikněte pravým tlačítkem na **MyACSHelloWorld**, klikněte na tlačítko **cesta sestavení**a potom klikněte na **konfigurovat cestu sestavení**.
6. Klikněte na tlačítko hello **knihovny** , klikněte na **filtru služeb řízení přístupu Azure**a potom klikněte na **upravit**.
7. Pomocí prohlížeče otevřít toohello **přihlašovací stránky integrace** stránku hello portálu pro správu, kopie hello adresy URL uvedené v hello **možnost 1: odkaz tooan hostované služby ACS přihlašovací stránku** pole a vložte jej do hello **Koncový bod ACS ověřování** pole hello Eclipse dialogového okna.
8. Pomocí prohlížeče otevřít toohello **upravit aplikaci předávající strany** stránku hello portálu pro správu, kopie hello adresy URL uvedené v hello **sféry** pole a vložte jej do hello **sféry předávající strany**  pole hello Eclipse dialogového okna.
9. V rámci hello **zabezpečení** části hello dialogového okna Eclipse, pokud chcete, aby toouse existující certifikát, klikněte na tlačítko **Procházet**, přejděte toohello certifikát má toouse, vyberte ho a klikněte na tlačítko  **Otevřete**. Nebo, pokud chcete toocreate nový certifikát, klikněte na tlačítko **nový** toodisplay hello **nový certifikát** dialogové okno, zadejte heslo hello, název souboru .cer hello a název pro hello nový soubor .pfx hello certifikát.
10. Zachovat **vložení hello certifikát v souboru WAR hello** zaškrtnuto, za předpokladu, že chcete tooembed hello certifikát v souboru WAR hello.
11. [Nepovinné] Zachovat **připojení vyžadují HTTPS** zaškrtnutí. Pokud nastavíte tuto možnost, budete potřebovat tooaccess aplikace pomocí protokolu HTTPS hello. Pokud nechcete, aby toorequire připojení prostřednictvím protokolu HTTPS, zrušte tuto možnost.
12. Pro tooAzure nasazení nastavení filtru ACS Azure bude vypadat podobně jako následující toohello.
    
    ![Nastavení Azure ACS filtru pro produkční nasazení][add_acs_filter_lib_production]
13. Klikněte na tlačítko **Dokončit** tooclose hello **upravit knihovnu** dialogové okno.
14. Klikněte na tlačítko **OK** tooclose hello **vlastnosti pro MyACSHelloWorld** dialogové okno.
15. V prostředí Eclipse, klikněte na tlačítko hello **publikování tooAzure cloudu** tlačítko. Odpověď výzvy toohello, podobné jako v hello **toodeploy vaší aplikace tooAzure** části hello [vytvoření aplikace Hello World služby Azure v prostředí Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) tématu. 

Poté, co byla nasazena webová aplikace, zavřete všechny relace prohlížeče otevřete spuštění webové aplikace a zobrazí se výzva toosign se pomocí přihlašovacích údajů Windows Live ID, za nímž následuje odesílány toohello vrátí adresu URL aplikace předávající strany.

Po dokončení pomocí aplikace Hello World služby ACS, mějte na paměti, toodelete hello nasazení (dozvíte, jak toodelete nasazení v hello [vytvoření aplikace Hello World služby Azure v prostředí Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) tématu).

## <a name="next_steps"></a>Další kroky
Zkoumání hello zabezpečení Assertion SAML (Markup Language) vrácena ACS tooyour aplikací, najdete v části [jak tooview SAML vrácený hello služby Řízení přístupu Azure][How tooview SAML returned by hello Azure Access Control Service]. toofurther prozkoumat funkce služby ACS a tooexperiment se složitější scénáři najdete v tématu [2.0 služby Řízení přístupu][Access Control Service 2.0].

Navíc tento příklad používá hello **vložení hello certifikát v souboru WAR hello** možnost. Tato možnost umožňuje jednoduché toodeploy hello certifikátu. Pokud místo toho chcete tookeep podpisového certifikátu oddělené od souboru WAR, můžete použít hello následujícím způsobem:

1. V rámci hello **zabezpečení** části hello **filtru služeb řízení přístupu Azure** dialogové okno, typ **${env. JAVA_HOME}/mycert.cer** a zrušte zaškrtnutí políčka **vložení hello certifikát v souboru WAR hello**. (Pokud název souboru certifikátu se liší, upravte Můj_certifikát.cer.) Klikněte na tlačítko **Dokončit** tooclose hello dialogu.
2. Kopírování hello certifikátu jako součást ve vašem nasazení: V prohlížeči na Eclipse projektu, rozbalte položku **MyAzureACSProject**, klikněte pravým tlačítkem na **WorkerRole1**, klikněte na tlačítko **vlastnosti** , rozbalte položku **Role v Azure**a klikněte na tlačítko **součásti**.
3. Klikněte na tlačítko **Přidat**.
4. V rámci hello **přidat součást** dialogové okno:
   
   1. V hello **Import** části:
      1. Použití hello **souboru** tlačítko toonavigate toohello certifikát má toouse. 
      2. Pro **metoda**, vyberte **kopie**.
   2. Pro **název jako**, klikněte na hello textového pole a přijměte výchozí název hello.
   3. V hello **nasadit** části:
      1. Pro **metoda**, vyberte **kopie**.
      2. Pro **toodirectory**, typ **JAVA_HOME %**.
   4. Vaše **přidat součást** dialogové okno by měl vypadat podobně jako následující toohello.
      
       ![Přidat součást certifikátu][add_cert_component]
   5. Klikněte na **OK**.

V tomto okamžiku by váš certifikát zahrnuty ve vašem nasazení. Všimněte si, bez ohledu na to, jestli vložení hello certifikát do souboru WAR hello nebo ji přidat jako součást tooyour nasazení, je nutné názvů tooyour certifikátů hello tooupload jak je popsáno v hello [nahrát certifikát tooyour ACS obor názvů] [ Upload a certificate tooyour ACS namespace] části.

[What is ACS?]: #what-is
[Concepts]: #concepts
[Prerequisites]: #pre
[Create a Java web application]: #create-java-app
[Create an ACS Namespace]: #create-namespace
[Add Identity Providers]: #add-IP
[Add a Relying Party Application]: #add-RP
[Create Rules]: #create-rules
[Upload a certificate tooyour ACS namespace]: #upload-certificate
[Review hello Application Integration Page]: #review-app-int
[Configure Trust between ACS and Your ASP.NET Web Application]: #config-trust
[Add hello ACS Filter library tooyour application]: #add_acs_filter_library
[Deploy toohello compute emulator]: #deploy_compute_emulator
[Deploy tooAzure]: #deploy_azure
[Next steps]: #next_steps
[project website]: http://wastarterkit4java.codeplex.com/releases/view/61026
[How tooview SAML returned by hello Azure Access Control Service]: active-directory-java-view-saml-returned-by-access-control.md
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Windows Identity Foundation]: http://www.microsoft.com/download/en/details.aspx?id=17331
[Windows Identity Foundation SDK]: http://www.microsoft.com/download/en/details.aspx?id=4451
[Azure Management Portal]: https://manage.windowsazure.com
[acs_flow]: ./media/active-directory-java-authenticate-users-access-control-eclipse/ACSFlow.png

<!-- Eclipse-specific -->
[add_acs_filter_lib]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibrary.png
[add_acs_filter_lib_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryEmulator.png
[add_acs_filter_lib_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryProduction.png

[relying_party_realm_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmEmulator.png
[relying_party_return_url_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLEmulator.png
[relying_party_realm_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmProduction.png
[relying_party_return_url_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLProduction.png
[add_cert_component]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddCertificateComponent.png
[add_jsp_file_acs]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddJSPFileACS.png
[create_acs_hello_world]: ./media/active-directory-java-authenticate-users-access-control-eclipse/CreateACSHelloWorld.png
[add_token_signing_cert]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddTokenSigningCertificate.png

