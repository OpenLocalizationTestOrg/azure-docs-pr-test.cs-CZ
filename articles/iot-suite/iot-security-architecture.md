---
title: "Architektura zabezpečení IoT | Microsoft Docs"
description: "Architektura pokyny pro IoT zabezpečení a důležité informace"
services: 
suite: iot-suite
documentationcenter: 
author: YuriDio
manager: timlt
editor: 
ms.assetid: 18ed3eb0-9406-44e1-8a3a-93dc6726c7ac
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: yurid
ms.openlocfilehash: 2482dade7d17d05b2fc90fbf22b0466227a5983b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="internet-of-things-security-architecture"></a>Architektura zabezpečení Internetu věcí
Při návrhu systému, je důležité pochopit potenciální hrozby na daný systém a přidejte příslušné obrany podle toho, jak systém je navržená tak a navržen. Je zvlášť důležité při návrhu produktu od začátku s důrazem na bezpečnost, protože pochopení, jak může být útočník mohl ohrozit systém pomáhá, ujistěte se, zda odpovídající jejich zmírnění jsou zavedené od začátku. 

## <a name="security-starts-with-a-threat-model"></a>Zabezpečení začíná model hrozeb
Společnost Microsoft dlouho používá modely hrozeb pro jeho produktů a udělal společnosti threat modelování proces veřejně dostupné. Prostředí společnosti ukazuje, že modelování obsahuje neočekávané výhody mimo okamžitou pochopení co hrozeb je nejvíce týkající se. Například vytvoří také způsob otevřete informace s ostatními uživateli mimo vývojový tým, což může vést k nových nápadů a vylepšení produktu.

Cílem modelování hrozeb je pochopit, jak může být útočník může ohrozit systém a ujistěte se, že jejich odpovídající zmírnění jsou na místě. Vynutí modelování hrozeb tým vzít v úvahu jejich zmírnění navrženou systému spíše než po systému se nasazuje. Tato skutečnost má zásadní význam, protože modernizace obrany zabezpečení do velkého počtu zařízení v poli je nemožné, chyba náchylné k chybám a ponechá zákazníků v ohrožení.

Mnoho vývojové týmy provést úlohu vynikající zaznamenávání funkční požadavky pro systém, využívající zákazníků. Identifikace není zřejmé způsoby, že někdo může zneužít v systému je však další náročné. Modelování hrozeb může pomoct pochopit, co může útočník dělat vývojové týmy a proč. Modelování hrozeb je strukturovaných proces, který vytvoří rozhodnutí o návrhu diskuzi o zabezpečení v systému a také změní na návrh, který probíhají na cestě této dopad zabezpečení. Jednoduše dokumentu při model hrozeb této dokumentace také představuje nejvhodnější způsob a zajistit kontinuitu znalostí, uchování lekce se naučili, a nápovědu nový tým zaváděním rychle. Nakonec výsledek modelování hrozeb je umožnit vzít v úvahu další aspekty zabezpečení, jako jsou například jaké závazky týkajícími se zabezpečení chcete poskytnout zákazníkům. Tyto závazky ve spojení s modelování hrozeb bude informovat o tom a jednotky testování pro vaše řešení Internetu věcí (IoT).

### <a name="when-to-threat-model"></a>Kdy hrozby modelu
[Modelování hrozeb](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx) nabízí nejvyšší hodnota, pokud je součástí do fáze návrhu. Při návrhu, máte nejvyšší flexibilitu provést změny eliminovat hrozeb. Odstranění hrozeb návrhu je požadovaný výsledek. Je mnohem jednodušší než přidávání způsoby zmírnění rizik, je testování a zajištění zůstanou aktuální a kromě toho toto odstranění není možné. Bude těžší eliminovat hrozeb jako produkt stane víc vyspělých a pak bude nakonec vyžadovat další práci a kompromisy je mnohem obtížnější než hrozby již v rané fázi na modelování pro vývoj.

### <a name="what-to-threat-model"></a>Co je potřeba model hrozeb
Měli byste vláken modelu řešení jako celek a také soustředit se v těchto oblastech:

* Funkce zabezpečení a ochrana osobních údajů
* Funkce, jejíž selhání jsou relevantní zabezpečení
* Funkce, které touch hranice vztahu důvěryhodnosti 

### <a name="who-threat-models"></a>Kdo hrozby modely
Modelování hrozeb je stejně jako jakýkoli jiný proces.  Je vhodné pracovat s dokumentu model hrozeb jako jakoukoli jinou součástí řešení a ověřte ji. Mnoho vývojové týmy provést úlohu vynikající zaznamenávání funkční požadavky pro systém, využívající zákazníků. Identifikace není zřejmé způsoby, že někdo může zneužít v systému je však další náročné. Modelování hrozeb může pomoct pochopit, co může útočník dělat vývojové týmy a proč.

### <a name="how-to-threat-model"></a>Postup model hrozeb
Riziko, že proces modelování se skládá ze čtyř kroků; Tyto kroky jsou:

* Modelování aplikace
* Zobrazení výčtu hrozeb
* Zmírnit hrozby
* Ověření jejich zmírnění

#### <a name="the-process-steps"></a>Kroky procesu
Tři zásady třeba vzít v úvahu při sestavování model hrozeb:

1. Vytvořte diagram mimo referenční architektura. 
2. Spusťte spektra první. Přehled a pochopit systém jako celek, než začnete přímým.  To pomáhá zajistit, že můžete nabídnout detailní na správných místech.
3. Disk proces, nenechte proces je jednotka. Je-li najít problém ve fázi modelování a chcete ho prozkoumat, přejděte na její!  Není zaregistrované, budete muset postupovat podle těchto kroků slavishly.  

#### <a name="threats"></a>Hrozby
Čtyři základní prvky model hrozeb jsou:

* Procesy (webové služby, služby Win32 * nix démoni atd. Všimněte si, že některé komplexní entity (například pole brány a senzory) můžete abstrakci jako proces při technické podrobnostem v těchto oblastech není možné.
* Úložiště dat (všude, kde jsou data uložena, jako je například konfigurace souboru nebo databázi)
* Tok dat (kde dat prochází mezi další prvky v aplikaci)
* Externí entity (všechno, co komunikuje s systému, ale není pod kontrolou aplikace, příklady zahrnují uživatele a satelitní informační kanály)

Všechny elementy ve diagram architektury podléhají různé hrozby; použijeme symbolické STRIDE. Čtení [Threat modelování znovu, STRIDE](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) Další informace o STRIDE elementy.

Různé elementy diagramu aplikace podléhají určité hrozby STRIDE:

* Procesy, které podléhají STRIDE
* Toky dat podléhají TID
* Úložiště dat se vztahují TID a někdy R, pokud úložiště dat jsou soubory protokolu.
* Externí entity podléhají SRD

## <a name="security-in-iot"></a>Zabezpečení v IoT
Připojená zařízení speciální mít velký počet potenciální interakce ploch a vzory interakce, které musí považovat zajistit rozhraní pro zabezpečení digitální přístupu k těmto zařízením. Termín "digitální access" se používá zde odlišit od žádné činnosti, které se provádějí pomocí zařízení přímo interakce kde zabezpečení přístupu je zajišťováno prostřednictvím řízení fyzického přístupu. Například uvedení zařízení do místnosti s zámek na dvířka. Při fyzického přístupu nemůže být odepřeno pomocí softwaru a hardwaru, můžete se opatření zabránit fyzickému přístupu z úvodního k narušení systému. 

Jak jsme prozkoumat vzory interakce, podíváme se na "ovládání zařízení" a "zařízení data" se stejnou úrovní pozornost. "Ovládání zařízení" můžou být klasifikované jako veškeré informace, které zajišťuje žádná strana na zařízení, s cílem změna nebo ovlivňující své chování směrem stavu nebo stavu jeho prostředí. "Zařízení data" můžou být klasifikované jako veškeré informace, které zařízení vysílá na druhé straně o jejím stavu a zjištěnou stav jeho prostředí.

Chcete-li optimalizovat osvědčené postupy zabezpečení, se doporučuje, aby typické architektuře IoT rozdělit na několik součástí/zóny jako součást threat modelování cvičení. Tyto zóny jsou plně popsané v této části a zahrnují:

* Zařízení,
* Brána pole
* Cloudové brány, a
* Služby.

Zóny jsou široké způsob, jak segmentovat řešení; každou zónu často má vlastní požadavky na data a ověřování a autorizace. Zóny můžete také použít k izolaci poškození a omezit dopad nízkou důvěryhodnosti zóny na vyšší vztahu důvěryhodnosti zóny.

Každé zóny je oddělené hranicí vztahu důvěryhodnosti, který je uvedený jako tečkovaná čára red v následujícím diagramu. Představuje přechod data nebo informace z jednoho zdroje do jiného. Během tento přechod může data nebo informace podléhat falšování, Nepovolená manipulace, Odvolatelnost, zpřístupnění informací, dostupnost služby a zvýšení oprávnění (STRIDE).

![Zóny zabezpečení IoT](media/iot-security-architecture/iot-security-architecture-fig1.png) 

Součásti použité v ukázkách v rámci každé hranice, se také podrobí STRIDE, povolení úplné 360 zobrazení řešení modelování hrozeb. V níže uvedených částech propracovanější na jednotlivých součástí a konkrétní bezpečnostní otázky a řešení, které mají být vloženy do místní.

V následujících zabývat standardní součásti, které se většinou nacházejí v těchto zónách.

### <a name="the-device-zone"></a>Zóny zařízení
Prostředí zařízení je okamžitou fyzického místa kolem zařízení, kde je to fyzického přístupu nebo "místní sítě" možné je peer-to-peer digitální přístupu k zařízení. "Místní sítě" se předpokládá, že se k síti, který se odlišuje a je izolovaná od – ale potenciálně přidat do mostu k – veřejného Internetu a obsahuje všechny krátkého dosahu bezdrátové přepínačů technologie, které umožňuje komunikaci peer-to-peer zařízení. Provede *není* zahrnout všechny technologie virtualizace sítě vzniká dojem místní síti a také nebude obsahovat veřejný operátor sítě, které vyžadují jakékoli dvě zařízení komunikovat místo veřejné síti, pokud by byly zadejte vztahu komunikace peer-to-peer.

### <a name="the-field-gateway-zone"></a>Zóny brána pole
Brána pole je zařízení nebo zařízení nebo server pro obecné účely počítači software, který funguje jako aktivátor komunikace a i jako systém správy zařízení a zařízení zpracování dat rozbočovače. Zóny brána pole zahrnuje samotná brána pole a všechna zařízení, které jsou připojené k němu. Jak již název napovídá, pole brány fungují zařízení mimo vyhrazené zpracování dat, jsou obvykle umístění vázán, potenciálně podléhají fyzického narušení a bude mít omezenou provozní redundance. Do říkají, že je brána pole běžně co jeden touch a sabotáž a zároveň budete vědět, co se jeho funkce. 

Brána pole se liší od směrovač pouhé provoz v tom došlo aktivní roli při správě přístupu a toku informací, což znamená, že je aplikace řešit entitu a síťové připojení nebo relace terminálu. Zařízení NAT nebo brány firewall, naproti tomu není způsobilá jako brány pole vzhledem k tomu, že nejsou explicitní spojení nebo terminály relace, ale spíš trasy (nebo bloku) připojení nebo provedeny prostřednictvím jejich relací. Brána pole má dvě odlišné prostor oblasti. Jeden otočená zařízení, které jsou připojené k němu a představuje uvnitř zóny a dalších otočená všechny externí strany a je okraji zóny.   

### <a name="the-cloud-gateway-zone"></a>Zóna cloudové brány
Cloudová brána je systém, který umožňuje vzdálenou komunikaci od a do zařízení nebo brány pole z několika různých lokalit prostoru veřejnou síť, obvykle směrem řízení na základě cloud a systém analýzy dat, federace těchto systémů. V některých případech cloudové brány může okamžitě usnadnění přístupu k zařízení speciální z terminály, jako například tablety nebo telefony. V kontextu tady popisovaných "cloud" slouží k odkazování na systém vyhrazené zpracování dat, který není vázán na stejném místě jako připojená zařízení nebo pole brány. V cloudu zóně, také provozní míry zabránit cílové fyzickému přístupu a nejsou viditelné nutně s infrastrukturou "veřejného cloudu".  

Cloudové brány může být namapovaný potenciálně do překrytí virtualizace sítě do izolovat Cloudová brána a všechny jeho připojená zařízení nebo brány pole od ostatního síťového přenosu. Samotná brána cloudu je ani řídicím systémem zařízení ani zpracování nebo zařízení úložiště pro data zařízení; Tyto vlastnosti rozhraní s Cloudová brána. Oblast cloudové brány zahrnuje brány cloudu společně s všechny brány pole a zařízení k němu připojen přímo nebo nepřímo. Okraji zóny je odlišné prostor oblasti, kde všechny externí strany komunikovat prostřednictvím.

### <a name="the-services-zone"></a>Zóny služby
"Služba" je definována pro tento kontext jako všechny součásti softwaru nebo modul, který je během propojení s zařízení prostřednictvím brány pole nebo cloud pro shromažďování dat a analýzy, a také pro příkazy a ovládání.  Zprostředkovatelé jsou služby. Budou fungovat v rámci své identity směrem brány a dalších subsystémy, ukládání a analyzovat data, samostatně vydávat příkazy zařízení na základě dat po zobrazení statistiky nebo plány a vystavit informace a řídit možnosti autorizovaný koncovým uživatelům.

### <a name="information-devices-vs-special-purpose-devices"></a>Informace o zařízení oproti zařízeními pro zvláštní účely
Počítače, telefony a tablety jsou primárně interaktivní informace o zařízení. Telefony a tablety jsou explicitně optimalizované kolem maximalizace životnosti baterie. Jejich ideálně vypnout částečně při interakci s osoba není okamžitě, nebo když není poskytující služby jako přehrávání Hudba nebo vedení jejich vlastníkovi na určité místo. Z hlediska systémy jsou tato zařízení technologie informace především funguje jako proxy směrem osoby. Jsou to "osoby válcích" návrhy akce a "osoby senzory" shromažďování vstupu. 

Zařízeními pro zvláštní účely, z senzory teploty jednoduché komplexní objekt pro vytváření produkční řádcích s tisíci součásti v nich, se liší. Tato zařízení jsou mnohem víc obor v účel a i v případě, že obsahují některé uživatelské rozhraní, jsou z velké části omezená na rozhraní k nebo integrovat do prostředky na fyzických světě. Jejich měření a sestavy prostředí okolností, zapněte ventily, řídit servos, zvukových výstrahy, přepínač indikátory a udělat celou řadu dalších úloh. Postup pomáhají, pro které zařízení se systémem informace je příliš obecný, příliš nákladné, příliš velký nebo příliš křehká. Konkrétní účel okamžitě stanoví jejich technického návrhu jako i k dispozici peněžní nároky jejich výroby a životnosti naplánované operace. Kombinace těchto zahrnuje dva klíčové faktory omezí provozní dostupných výpočetních energetické nároky, fyzické nároky a proto úložiště k dispozici a funkcích zabezpečení.  

Pokud něco "vloží nesprávný" s automatizované nebo vzdálené ovladatelné zařízení, například fyzická poškození nebo ovládací prvek logiku vady úmyslného neoprávněné manipulaci a manipulace s. Velké produkční může být zničený, budovy může být looted nebo zapsaný a osoby mohou být poškozené nebo dokonce die. Toto je samozřejmě zcela liší třída škod než někdo maxing out odcizené karty platební limit. Panel zabezpečení pro zařízení, které věcí přesunout a také pro data snímačů, který nakonec výsledkem příkazy, které způsobí věcí, které chcete přesunout, musí být vyšší než v libovolném elektronického obchodování nebo bankovnictví scénář. 

### <a name="device-control-and-device-data-interactions"></a>Ovládání zařízení a zařízení dat interakce
Připojená zařízení speciální mít velký počet potenciální interakce ploch a vzory interakce, které musí považovat zajistit rozhraní pro zabezpečení digitální přístupu k těmto zařízením. Termín "digitální access" se používá zde odlišit od žádné činnosti, které se provádějí pomocí zařízení přímo interakce kde zabezpečení přístupu je zajišťováno prostřednictvím řízení fyzického přístupu. Například uvedení zařízení do místnosti s zámek na dvířka. Při fyzického přístupu nemůže být odepřeno pomocí softwaru a hardwaru, můžete se opatření zabránit fyzickému přístupu z úvodního k narušení systému. 

Jak jsme prozkoumat interakce vzory, podíváme se na "ovládání zařízení" a "data zařízení" se stejnou úrovní pozornost při modelování hrozeb. "Ovládání zařízení" můžou být klasifikované jako veškeré informace, které zajišťuje žádná strana na zařízení, s cílem změna nebo ovlivňující své chování směrem stavu nebo stavu jeho prostředí. "Zařízení data" můžou být klasifikované jako veškeré informace, které zařízení vysílá na druhé straně o jejím stavu a zjištěnou stav jeho prostředí. 

## <a name="threat-modeling-the-azure-iot-reference-architecture"></a>Referenční architektura Azure IoT modelování hrozeb
Společnost Microsoft používá rozhraní uvedených výše udělat threat modelování pro Azure IoT. V části níže používáme proto konkrétní příklad referenční architektura IoT Azure k ukazují myslíte o ohrožení modelování pro IoT a řešení hrozeb identifikovat. V našem případě myslíme, čtyři hlavní oblasti fokus:

* Zařízení a zdroje dat
* Přenos dat
* Zařízení a zpracování událostí a
* Prezentace

![Hrozby modelování pro Azure IoT](media/iot-security-architecture/iot-security-architecture-fig2.png) 

Následující obrázek poskytuje zjednodušený přehled o architektuře IoT společnosti Microsoft s využitím Diagram toku dat model, který používá nástroj Microsoft Threat modelování:

![Hrozby modelování pro Azure IoT pomocí nástroje pro modelování hrozeb MS](media/iot-security-architecture/iot-security-architecture-fig3.png)

Je důležité si uvědomit, že architektura odděluje možností zařízení a brány. To umožňuje uživatelům využívat zařízení brány, které jsou bezpečnější: jsou schopné komunikovat s Cloudová brána pomocí zabezpečených protokolů, které vyžadují vyšší režie zpracování, nativní zařízení – například termostatu - může poskytnout svoje vlastní. V zóně služby Azure předpokládáme, že Cloudová brána je reprezentována službu Azure IoT Hub.

### <a name="device-and-data-sourcesdata-transport"></a>Přenosová vrstva služby zdroje nebo data zařízení a dat
V této části jsou zde popsány architektura uvedených výše prostřednictvím přehledu modelování hrozeb a poskytuje přehled o tom, jak některé vyplývajících otázky jsou adresování jsme. Se zaměříme na základní prvky model hrozeb:

* Procesy (ty v rámci našeho řízení a externí položky)
* Komunikace (také nazývané toky dat)
* Úložiště (také nazývané úložiště dat)

#### <a name="processes"></a>Procesy
V každé z kategorií uvedených v architektuře Azure IoT, pokusíme se zmírnit počet různých hrozeb v různých fázích informace o dat nebo existuje v: proces komunikace a úložiště. Níže budeme poskytnutí přehledu o nejběžnějších ty pro kategorii "proces", za nímž následuje přehled o tom, jak to může být nejlépe omezeny: 

**Falšování identity (S)**: útočník může extrahování materiál kryptografické klíče ze zařízení, buď na úrovni softwaru nebo hardwaru, a následně byly převzaty přístup v systému s jiné fyzické nebo virtuální zařízení pod identitou zařízení materiál klíče z. Dobrý obrázku je vzdálené ovládací prvky, který můžete zapnout jakékoli Televizi a oblíbených prankster nástroje, které jsou.

**Útok na dostupnost služby (D)**: zařízení mohou být vykresleny nepodporující funguje nebo komunikaci pomocí zasahovala do činnosti rádiových frekvencí nebo vyjímání vodičům. Například sledováním fotoaparát, který měl jeho napájení nebo síťového připojení záměrně vykrojí nebudou data sestavy, vůbec.

**Manipulaci (T)**: útočník může částečně nebo zcela nahradit softwaru spuštěné na zařízení, což může nahrazené softwaru využít originální identitu zařízení, pokud materiál klíče nebo kryptografických zařízení, která uchovává klíče materiály, které byly dostupné pro nedovolené program. Například může útočník využít extrahované materiál klíče zachytí a potlačit data ze zařízení na cestě komunikace a nahraďte ji metodou false data, která je k ověření pomocí odcizeného materiál klíče.

**Přístup k informacím (I)**: Pokud je na zařízení spuštěný zpracovatelné softwaru, takový zpracovatelné software může potenciálně úniku dat neoprávněným osobám. Například může útočník využít extrahované materiál klíče se sám vložit do komunikační trasa mezi zařízení a brána řadiče nebo pole nebo Cloudová brána k siphon vypnout informace.

**Zvýšení oprávnění (E)**: zařízení, která provádí konkrétní funkce se dá vynutit na dělejte něco jiného. Například může být ventil, který je naprogramovaný tak, aby otevřete poloviční způsobem, aby otevřete úplně.

| **Komponenta** | **Hrozby** | **Zmírnění dopadů** | **Riziko** | **Implementace** |
| --- | --- | --- | --- | --- |
| Zařízení |S |Přiřazování identitu zařízení a ověřování zařízení |Nahrazení zařízení nebo jeho část zařízení s jiným zařízením. Jak jsme si vědomi, že jsme mluvíme na správné zařízení? |Ověření zařízení, pomocí zabezpečení TLS (Transport Layer) nebo protokol IPSec. Infrastruktura by měla podporovat použití předsdílený klíč (PSK) na těchto zařízeních, které nelze zpracovat úplné asymetrické šifrování. Využít Azure AD, [OAuth](http://www.rfc-editor.org/in-notes/internet-drafts/draft-ietf-ace-oauth-authz-01.txt) |
| TRID |Například platí tamperproof mechanismy pro zařízení tím, že ji velmi obtížné na možné extrahovat klíče a jiného kryptografických materiálu ze zařízení. |Riziko je, pokud někdo je falšování zařízení (fyzického narušení). Jak jsme je opravdu, nebylo manipulováno toto zařízení. |Co nejúčinnější zmírnění je funkcí module (TPM) důvěryhodné platformy, které umožňuje ukládání klíčů v speciální na čipu zapojení, ze kterého klíče nelze číst, ale lze použít pouze pro kryptografické operace využívající klíč, ale nikdy zveřejnit klíč. Paměť šifrování zařízení. Správy klíčů pro zařízení. Podepisování kódu. | |
| E |S řízení přístupu zařízení. Schéma ověřování. |Pokud zařízení umožňuje pro jednotlivé akce provést podle příkazy z vnějšího zdroje nebo i ohroženými senzorů, bude možné útoku k provádění operací jinak nejsou dostupné. |Schéma ověřování pro zařízení s | |
| Brána pole |S |Ověřování brána pole cloudové brány (na základě certifikátu, PSK, deklarace identity na základě,...) |Pokud někdo může zfalšovat brána pole, pak se může zobrazit i jako jakékoli zařízení. |Protokol TLS RSA/PSK, IPSec, [RFC 4279](https://tools.ietf.org/html/rfc4279). Stejné klíčové otázky úložiště a ověření zařízení v obecné – případ nejlepší je použít TPM. 6LowPAN rozšíření pro protokol IPSec pro podporu bezdrátové senzor sítě (WSN). |
| TRID |Chránit brána pole proti manipulaci (TPM)? |Falšování identity útoků, které oklamat přemýšlení brány cloudu, které je rozhovoru s brána pole může mít za následek zpřístupnění informací a manipulaci dat |Paměť šifrování, TPM společnosti, ověřování. | |
| E |Mechanismus řízení přístupu pro brána pole | | | |

Tady jsou některé příklady hrozeb v této kategorii:

Falšování identity: Útočník může extrahovat materiál kryptografické klíče ze zařízení, buď na softwaru nebo hardwaru úrovni a následně přístupu, kterou systém s jiné fyzické nebo virtuální zařízení pod identitou zařízení materiál klíče byly převzaty z.

**Odmítnutí služby**: zařízení mohou být vykresleny nepodporující funguje nebo komunikaci pomocí zasahovala do činnosti rádiových frekvencí nebo vyjímání vodičům. Například sledováním fotoaparát, který měl jeho napájení nebo síťového připojení záměrně vykrojí nebudou data sestavy, vůbec.

**Manipulaci**: útočník může částečně nebo zcela nahradit softwaru spuštěné na zařízení, což může nahrazené softwaru využít originální identitu zařízení, pokud materiál klíče nebo kryptografických zařízení, která uchovává klíče materiály, které byly dostupné pro nedovolené program.

**Manipulaci**: sledováním fotoaparát, který se zobrazuje viditelné spektrum přehled o prázdný hallway může zaměřené na fotografie takové hallway. Senzor kouř nebo ještě efektivněji může reporting někdo podržíte světlejšího v něm. V obou případech může být zařízení technicky plně důvěryhodného směrem systému, ale bude hlášené zpracovatelné informace.

**Manipulaci**: útočník může využít extrahované materiál klíče zachytí a potlačit data ze zařízení na cestě komunikace a nahraďte ji metodou false data, která je k ověření pomocí odcizeného materiál klíče.

**Manipulaci**: útočník může částečně nebo zcela nahradit softwaru spuštěné na zařízení, což může nahrazené softwaru využít originální identitu zařízení, pokud materiál klíče nebo kryptografických zařízení, která uchovává klíče materiály, které byly dostupné pro nedovolené program.

**Zpřístupnění informací**: Pokud je na zařízení spuštěný zpracovatelné softwaru, takový zpracovatelné software může potenciálně úniku dat neoprávněným osobám.

**Zpřístupnění informací**: útočník může využít extrahované materiál klíče se sám vložit do komunikační trasa mezi zařízením a řadiče nebo pole brány nebo Cloudová brána k siphon vypnout informace.

**Odmítnutí služby**: zařízení může být vypnutý nebo převedena na režim, kde komunikace není možná (což je úmyslné v mnoha průmyslových strojů).

**Manipulaci**: zařízení můžete nakonfigurovat tak, aby provoz ve stavu neznámé do systému správy (mimo známých kalibračních parametrů) a tak poskytuje data, která může být nesprávné interpretaci

**Zvýšení úrovně oprávnění**: zařízení, která provádí konkrétní funkce se dá vynutit na dělejte něco jiného. Například může být ventil, který je naprogramovaný tak, aby otevřete poloviční způsobem, aby otevřete úplně.

**Odmítnutí služby**: zařízení mohou být změněny na stav, kdy komunikace není možná.

**Manipulaci**: zařízení můžete nakonfigurovat tak, aby provoz ve stavu neznámé do systému správy (mimo známých kalibračních parametrů) a tak poskytuje data, která může být nesprávné interpretaci.

**Falšování, Nepovolená manipulace/Odvolatelnost**: Pokud není zabezpečen (což je zřídka případ s ovládacími prvky pro vzdálené k příjemce) útočník můžete upravit anonymně stav zařízení. Dobrý obrázku je vzdálené ovládací prvky, který můžete zapnout jakékoli Televizi a oblíbených prankster nástroje, které jsou.

#### <a name="communication"></a>Komunikace
Hrozby kolem cesta komunikaci mezi zařízeními, zařízení a pole brány a zařízení a cloudové brány. Následující tabulka obsahuje některé pokyny kolem otevřete sockets na zařízení sítě VPN:

| **Komponenta** | **Hrozby** | **Zmírnění dopadů** | **Riziko** | **Implementace** |
| --- | --- | --- | --- | --- |
| Zařízení IoT Hub |TID |(D) (PSK/RSA) k šifrování přenosů TLS |Odposlouchávání nebo vzájemnému komunikaci mezi zařízením a brány |Zabezpečení na úrovni protokolu. S vlastní protokoly musíme zjistěte, jak je chránit. Ve většině případů komunikace probíhá ze zařízení ke službě IoT Hub (připojení iniciuje zařízení). |
| Zařízení zařízení |TID |(D) Protokol TLS (PSK/RSA) k šifrování přenosů. |Čtení dat během přenosu mezi zařízeními. Manipulaci s daty. Přetížení zařízení v rámci nového připojení |Zabezpečení na úrovni protokolu (MQTT nebo AMQP nebo HTTP/CoAP. S vlastní protokoly musíme zjistěte, jak je chránit. Zmírnění dopadů na hrozby DoS je peer zařízení prostřednictvím brány cloudu nebo pole a nechat je jenom akce jako klienti směrem k síti. Partnerský vztah může vést k přímé připojení mezi rovnocennými počítači po s byla zprostředkované bránou |
| Externí Entity zařízení |TID |Silné párování externí entity do zařízení |Odposlouchávání připojení k zařízení. Vzájemnému komunikaci se zařízením |Bezpečně párování externí entity, která má zařízení LE NFC/Bluetooth. Řízení provozu panelu zařízení (fyzické) |
| Brána cloudové brány pole |TID |Protokol TLS (PSK/RSA) k šifrování přenosů. |Odposlouchávání nebo vzájemnému komunikaci mezi zařízením a brány |Zabezpečení na úrovni protokolu (MQTT nebo AMQP nebo HTTP/CoAP). S vlastní protokoly musíme zjistěte, jak je chránit. |
| Zařízení brány cloudu |TID |Protokol TLS (PSK/RSA) k šifrování přenosů. |Odposlouchávání nebo vzájemnému komunikaci mezi zařízením a brány |Zabezpečení na úrovni protokolu (MQTT nebo AMQP nebo HTTP/CoAP). S vlastní protokoly musíme zjistěte, jak je chránit. |

Tady jsou některé příklady hrozeb v této kategorii:

**Odmítnutí služby**: omezené zařízení, obecně ohrožené DoS při jejich aktivně naslouchat příchozí připojení nebo nevyžádané datagramy v síti, protože útočník může otevřít mnoha připojení paralelně a není služby je nebo je služba velmi pomalu nebo je možné, že je zařízení zaplnění nevyžádaný provoz. V obou případech zařízení, které lze účinně vykreslit přestane fungovat v síti.

**Falšování identity, zpřístupnění informací**: omezené zařízeními a zařízeními pro zvláštní účely často mít jeden pro all zabezpečení jako heslo nebo PIN kód ochrany, nebo zcela spoléhají na důvěřující síť, což znamená, bude udělit přístup k informacím, když je zařízení ve stejné síti, a tuto síť je často jenom chráněn sdílený klíč. To znamená, že když jsou zveřejňovány sdílený tajný klíč pro zařízení nebo sítě, je možné k řízení zařízení nebo sledování dat vygenerované ze zařízení.  

**Falšování identity**: útočník může zachytávat nebo částečně přepsat vysílání a zfalšovat původce (man uprostřed)

**Manipulaci**: útočník může zachytávat nebo částečně přepsat vysílání a odeslat nepravdivé informace 

**Přístup k informacím:** útočník může tajně poslouchat vysílání a získat informace o bez autorizace **Denial of Service:** útočník může zablokování signál vysílání a odepřít distribuční informace

#### <a name="storage"></a>Úložiště
Každé zařízení a pole brány má určitou formu úložiště (dočasný pro službu Řízení front dat a úložiště bitové kopie operačního systému (OS)).

| **Komponenta** | **Hrozby** | **Zmírnění dopadů** | **Riziko** | **Implementace** |
| --- | --- | --- | --- | --- |
| Úložiště zařízení. |TRID |Šifrování úložiště, podepisování protokoly |Čtení dat z úložiště (PII data), manipulaci s telemetrická data. Manipulaci s zařazených do fronty nebo ukládání do mezipaměti příkaz data ovládacího prvku. Manipulaci s konfigurace nebo firmware balíčky aktualizací do mezipaměti nebo zařazených do fronty místně, může dojít k součásti operačního systému nebo systému se ohrožení zabezpečení |Šifrování, ověřovací kód zprávy (MAC) nebo digitální podpis. Kde řízení možné silné přístup prostřednictvím přístupu k prostředkům řízení oprávnění nebo seznamy (ACL). |
| Bitové kopie operačního systému zařízení |TRID | |Manipulaci s operačním systémem / nahrazení součásti operačního systému |Jen pro čtení oddílu operačního systému podepsané bitové kopie operačního systému, šifrování |
| Brána pole úložiště (data služby Řízení front) |TRID |Šifrování úložiště, podepisování protokoly |Čtení dat z úložiště (PII data), manipulaci s daty telemetrie manipulaci s zařazených do fronty nebo ukládání do mezipaměti příkaz data ovládacího prvku. Manipulaci s balíčky aktualizací pro konfiguraci a firmware (určené pro zařízení nebo brána pole) do mezipaměti nebo zařazených do fronty místně, může dojít k součásti operačního systému nebo systému se ohrožení zabezpečení |Nástroj BitLocker |
| Image OS brána pole |TRID | |Manipulaci s operačním systémem / nahrazení součásti operačního systému |Jen pro čtení oddílu operačního systému podepsané bitové kopie operačního systému, šifrování |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Zařízení a události zpracování nebo cloudové brány zóny
Cloudové brány je systém, který umožňuje vzdálenou komunikaci od a do zařízení nebo brány pole z několika různých lokalit prostoru veřejnou síť, obvykle směrem řízení na základě cloud a systém analýzy dat, federace těchto systémů. V některých případech cloudové brány může okamžitě usnadnění přístupu k zařízení speciální z terminály, jako například tablety nebo telefony. V kontextu tady popisovaných "cloud" je určená k odkazování na systém vyhrazené zpracování dat, který není vázán na stejném místě jako připojená zařízení nebo pole brány a kde provozní míry zabránit cílová fyzický přístup, ale není nutně s infrastrukturou "veřejného cloudu".  Cloudové brány může být namapovaný potenciálně do překrytí virtualizace sítě do izolovat Cloudová brána a všechny jeho připojená zařízení nebo brány pole od ostatního síťového přenosu. Samotná brána cloudu je ani řídicím systémem zařízení ani zpracování nebo zařízení úložiště pro data zařízení; Tyto vlastnosti rozhraní s Cloudová brána. Oblast cloudové brány zahrnuje brány cloudu společně s všechny brány pole a zařízení k němu připojen přímo nebo nepřímo.

Cloudová brána je ve většině případů vlastní integrované část softwaru jako službu s zveřejněné koncových bodů, do kterých brána pole a zařízení připojit. Jako takový musí být vytvořeny s důrazem na bezpečnost. Postupujte podle [SDL](http://www.microsoft.com/sdl) procesů pro navrhování a vytváření této služby. 

#### <a name="services-zone"></a>Zóny služby
Ovládací prvek systému (nebo řadič) je softwarové řešení, které sdílí rozhraní se zařízení, nebo brána pole nebo Cloudová brána za účelem řízení jedno nebo více zařízení nebo pro shromažďování nebo ukládat a analyzovat data zařízení pro prezentace, nebo pro účely další ovládací prvek. Ovládací prvek systémy jsou pouze entity, které v oboru toto pojednání, která by mohla okamžitě usnadnit interakce s uživateli. Výjimkou je zprostředkující fyzické ovládací prvek ploch na zařízení, jako jsou přepínače, který umožňuje uživateli vypnout zařízení nebo změnit ostatní vlastnosti, a pro které není žádný funkční ekvivalent, která je přístupná digitálně. 

Zprostředkující fyzické povrchy řízení jsou ty, kde jakéhokoliv druhu řídících logiku omezí funkce povrchu fyzické ovládacího prvku tak, aby ekvivalentní funkce lze inicializovat vzdáleně nebo vstupní je v konfliktu s vzdálené vstup se vyhnout – intermediated ploch řízení koncepčně jsou připojené k řízení místní systém, který využívá stejné základní funkce jako všechny ostatní systémy vzdálené řízení, které zařízení může připojit k paralelně. Horní hrozeb pro cloud computing, můžete si jej přečíst v [cloudu zabezpečení Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/) stránky.

## <a name="additional-resources"></a>Další zdroje
Najdete další informace v následujících článcích:

* [Nástroj modelování hrozeb SDL](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
* [Referenční Architektura Microsoft Azure IoT](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)

## <a name="see-also"></a>Viz také
Další informace o zabezpečení řešení IoT najdete v tématu [zabezpečit vaše nasazení IoT][lnk-security-deployment].

Můžete si taky prostudovat některé další funkce a možnosti předkonfigurovaných řešení sady IoT Suite:

* [Přehled řešení předkonfigurované prediktivní údržby][lnk-predictive-overview]
* [Nejčastější dotazy k sadě IoT Suite][lnk-faq]

Další informace o zabezpečení služby IoT Hub v [řízení přístupu ke službě IoT Hub] [ lnk-devguide-security] v příručce pro vývojáře IoT Hub.

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md

[lnk-security-deployment]: iot-suite-security-deployment.md
[lnk-devguide-security]: ../iot-hub/iot-hub-devguide-security.md