# <a name="internet-of-things-security-architecture"></a>Architektura zabezpečení Internetu věcí
Při návrhu systému, je důležité toounderstand hello potenciální hrozby toothat systém a přidejte příslušné obrany podle toho, jak systém hello je navržená tak a navržen. To je zvláště důležité, protože pochopení, jak útočník může být schopný toocompromise systému pomáhá, ujistěte se, že jsou splněné od začátku hello odpovídající jejich zmírnění toodesign hello produktu od začátku hello s důrazem na bezpečnost. 

## <a name="security-starts-with-a-threat-model"></a>Zabezpečení začíná model hrozeb
Společnost Microsoft dlouho používá modely hrozeb pro jeho produktů a udělal hello společnosti threat modelování proces veřejně dostupné. Hello zkušeností společnosti ukazuje, zda text hello modelování má neočekávaný výhody nad rámec hello okamžitou porozumět tomu, jaké hrozby jsou hello většina týkající se. Například vytvoří také způsob otevřete informace s ostatními uživateli mimo hello vývojový tým, což může vést toonew nápady a vylepšení v produktu hello.

cílem Hello modelování hrozeb je toounderstand jak útočník může být schopný toocompromise systému a ujistěte se, že jejich odpovídající zmírnění jsou na místě. Modelování hrozeb vynutí jejich hello návrhu team tooconsider zmírnění navrženou hello systému místo po nasazení systému. Tato skutečnost má zásadní význam, protože modernizace zabezpečení obrany tooa velkého počtu zařízení v poli hello je nemožné, chyba náchylné k chybám a ponechá zákazníků v ohrožení.

Mnoho vývojové týmy provést úlohu vynikající zaznamenávání hello funkční požadavky pro systém hello využívající zákazníků. Identifikace není zřejmé způsoby, že někdo může zneužít hello systému je však další náročné. Modelování hrozeb může pomoct pochopit, co může útočník dělat vývojové týmy a proč. Modelování hrozeb je strukturovaných proces, který vytvoří rozhodnutí o návrhu diskuzi o hello zabezpečení v systému hello, jakož i změny toohello návrhu, které jsou vytvářeny podél hello tak, aby mít vliv na zabezpečení. Jednoduše dokumentu při model hrozeb této dokumentace také představuje ideální způsob tooensure kontinuity znalostí, uchování lekce naučili a rychle pomoci zaváděním nový tým. Nakonec výsledek modelování hrozeb je tooenable tooconsider jste další aspekty zabezpečení, jako jsou například jaké závazky týkajícími se zabezpečení chcete tooprovide tooyour zákazníků. Tyto závazky ve spojení s modelování hrozeb bude informovat o tom a jednotky testování pro vaše řešení Internetu věcí (IoT).

### <a name="when-toothreat-model"></a>Když toothreat modelu
[Modelování hrozeb](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx) nabízí hello nejvyšší hodnota, pokud je součástí hello fáze návrhu. Při návrhu, máte hello nejvyšší flexibilitu toomake změny tooeliminate hrozeb. Odstranění hrozeb návrhu je hello požadovaný výsledek. Je mnohem jednodušší než přidávání způsoby zmírnění rizik, je testování a zajištění zůstanou aktuální a kromě toho toto odstranění není možné. Bude těžší tooeliminate hrozby, jakmile produkt k další pro dospělé a pak bude nakonec vyžadovat další práci a kompromisy je mnohem obtížnější než hrozby již v rané fázi na modelování vývojem hello.

### <a name="what-toothreat-model"></a>Jaké toothreat modelu
Měli vláken modelu hello řešení jako celek a také fokusu v hello následující oblasti:

* funkce zabezpečení a ochrany osobních údajů Hello
* Funkce Hello, jejichž selhání jsou relevantní zabezpečení
* Hello funkce, které touch hranice vztahu důvěryhodnosti 

### <a name="who-threat-models"></a>Kdo hrozby modely
Modelování hrozeb je stejně jako jakýkoli jiný proces.  Je vhodné tootreat hello threat modelu dokumentu jako jakoukoli jinou součástí hello řešení a ověřte ji. Mnoho vývojové týmy provést úlohu vynikající zaznamenávání hello funkční požadavky pro systém hello využívající zákazníků. Identifikace není zřejmé způsoby, že někdo může zneužít hello systému je však další náročné. Modelování hrozeb může pomoct pochopit, co může útočník dělat vývojové týmy a proč.

### <a name="how-toothreat-model"></a>Jak toothreat modelu
proces modelování hrozeb Hello se skládá z čtyři kroky; Hello kroky jsou:

* Aplikace modelu hello
* Zobrazení výčtu hrozeb
* Zmírnit hrozby
* Ověřit jejich zmírnění hello

#### <a name="hello-process-steps"></a>kroky zpracování Hello
Tři zásady tookeep v paměti při sestavování model hrozeb:

1. Vytvořte diagram mimo referenční architektura. 
2. Spusťte spektra první. Přehled a pochopit hello systém jako celek, než začnete přímým.  Tato pomáhá zajistit, že můžete nabídnout detailní v pravém hello umístí.
3. Disk hello procesu, nenechte hello procesu můžete jednotky. Pokud jste v hello modelování fáze najít problém a chcete tooexplore je pro ni přejděte!  Není zaregistrované, je třeba toofollow tyto kroky slavishly.  

#### <a name="threats"></a>Hrozby
Hello čtyři základní prvky model hrozeb jsou:

* Procesy (webové služby, služby Win32 * nix démoni atd. Všimněte si, že některé komplexní entity (například pole brány a senzory) můžete abstrakci jako proces při technické rozbalení v těchto oblastech není možné.
* Úložiště dat (všude, kde jsou data uložena, jako je například konfigurace souboru nebo databázi)
* Tok dat (kde dat prochází mezi další prvky v aplikaci hello)
* Externí entity (všechno, co komunikuje s hello systému, ale není pod kontrolou hello hello aplikace, příklady zahrnují uživatele a satelitní informační kanály)

Všechny elementy ve hello diagram architektury jsou subjektu toovarious hrozeb; použijeme symbolické STRIDE hello. Čtení [Threat modelování znovu, STRIDE](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) tooknow více informací o hello STRIDE elementy.

Různé prvky diagram aplikace hello jsou hrozeb STRIDE toocertain subjektu:

* Procesy jsou tooSTRIDE subjektu
* Toky dat jsou tooTID subjektu
* Úložiště dat jsou tooTID subjektu a někdy R, pokud úložiště dat hello soubory protokolu.
* Externí entity jsou tooSRD subjektu

## <a name="security-in-iot"></a>Zabezpečení v IoT
Připojená zařízení speciální mít velký počet potenciální interakce ploch a interakce vzorů, které je třeba zvážit tooprovide rozhraní pro zabezpečení digitální přístup toothose zařízení. Hello termín "Digitální přístup" je použité sem toodistinguish z jakékoli operace, které se provádějí pomocí interakci přímé zařízení, kde je zabezpečení přístupu zadaný prostřednictvím řízení fyzický přístup. Například uvedení hello zařízení do místnosti s zámek na dveře hello. Při fyzického přístupu nemůže být odepřeno pomocí softwaru a hardwaru, můžete se opatření tooprevent fyzický přístup z úvodní toosystem narušení. 

Jak jsme prozkoumat hello interakce vzory, podíváme se na "ovládání zařízení" a "data zařízení" s hello stejnou úroveň pozornost. "Ovládání zařízení" můžou být klasifikované jako veškeré informace, které poskytuje tooa zařízení libovolné strany s cílem hello změna nebo ovlivňující své chování směrem k jeho stavu nebo stavu hello jeho prostředí. "Zařízení data" můžou být klasifikované jako jakékoli informace vysílá zařízení tooany druhou stranou o jeho stavu a hello zjištěnými stav jeho prostředí.

V pořadí toooptimize osvědčené postupy zabezpečení je doporučeno, typické architektuře IoT rozdělit na několik součástí/zóny v rámci hello threat modelování cvičení. Tyto zóny jsou plně popsané v této části a zahrnují:

* Zařízení,
* Brána pole
* Cloudové brány, a
* Služby.

Zóny jsou široké toosegment způsob řešení; každou zónu často má vlastní požadavky na data a ověřování a autorizace. Zóny může být také použít tooisolation poškodit a omezit hello dopad nízkou důvěryhodnosti zóny na vyšší vztahu důvěryhodnosti zóny.

Každé zóny je oddělené hranicí vztahu důvěryhodnosti, který je uvedený jako hello s tečkami red řádku v následujícím diagramu hello. Představuje přechod data nebo informace z jednoho zdroje tooanother. Během tento přechod může být hello data nebo informace o subjektu tooSpoofing, Nepovolená manipulace, Odvolatelnost, zpřístupnění informací, dostupnost služby a zvýšení oprávnění (STRIDE).

![Zóny zabezpečení IoT](media/iot-security-architecture/iot-security-architecture-fig1.png) 

Hello součásti znázorněný v rámci každé hranice, jsou také vystaveno tooSTRIDE, povolení úplné 360 zobrazení hello řešení modelování hrozeb. v níže uvedených částech Hello propracovanější na každé součásti hello a konkrétní bezpečnostní otázky a řešení, které mají být vloženy do místní.

Hello oddíly, které následuje zabývat standardní součásti, které se většinou nacházejí v těchto zónách.

### <a name="hello-device-zone"></a>Hello zařízení zóny
Hello prostředí zařízení je hello okamžitou fyzického místa na disku kolem hello zařízení, kde fyzický přístup nebo "místní sítě" peer-to-peer Digitální přístup toohello je zařízení v rámci výpočetních procesů. "Místní sítě" se předpokládá toobe síť, který se odlišuje a je izolovaná od – ale potenciálně přidat do mostu příliš hello – veřejného Internetu a obsahuje všechny krátkého dosahu bezdrátové přepínačů technologie, které umožňuje komunikaci peer-to-peer zařízení. Provede *není* zahrnout všechny technologie virtualizace sítě vytváření hello iluzi místní sítě a také nebude obsahovat veřejný operátor sítě, které vyžadují jakékoli dvě zařízení toocommunicate prostoru veřejné síti Pokud byly vztah komunikace tooenter peer-to-peer.

### <a name="hello-field-gateway-zone"></a>Hello pole brány zóny
Brána pole je zařízení nebo zařízení nebo server pro obecné účely počítači software, který funguje jako aktivátor komunikace a i jako systém správy zařízení a zařízení zpracování dat rozbočovače. Hello pole brány zóny obsahuje samotná brána pole hello a všechna zařízení, které jsou připojené tooit. Jak hello název napovídá, pole brány fungují zařízení mimo vyhrazené zpracování dat, jsou obvykle umístění vázán, jsou potenciálně narušení toophysical subjektu a bude mít omezenou provozní redundance. Všechny toosay, které brána pole se často věcí, jeden můžete touch a sabotáž a zároveň budete vědět, co se jeho funkce. 

Brána pole se liší od směrovač pouhé provoz v tom došlo aktivní roli při správě přístupu a toku informací, což znamená, že je aplikace řešit entitu a síťové připojení nebo relace terminálu. Zařízení NAT nebo brány firewall, naopak nemohou být jako brány pole vzhledem k tomu, že nejsou explicitní spojení nebo terminály relace, ale spíš trasy (nebo bloku) připojení nebo provedeny prostřednictvím jejich relací. Brána pole Hello má dvě odlišné prostor oblasti. Jeden otočená hello zařízení, které jsou připojené tooit a představuje hello uvnitř hello zóny a hello jiných otočená všechny externí strany a je hello okraji hello zóny.   

### <a name="hello-cloud-gateway-zone"></a>Hello cloudové brány zóny
Cloudová brána je systém, který umožňuje vzdálenou komunikaci z a toodevices nebo pole brány z několika různých lokalit prostoru veřejnou síť, obvykle k cloudové řízení a datové analýzy systému federace těchto systémů. V některých případech může usnadnit cloudové brány okamžitě toospecial účel zařízení pro přístup z terminály, jako například tablety nebo telefony. V kontextu hello popsané tady "cloud" je určená toorefer tooa vyhrazené zpracování dat systému, který není vázané toohello stejné lokalitě jako hello připojen zařízení nebo pole brány. V cloudu zóně, také provozní míry zabránit cílové fyzický přístup a není nutně zveřejněné tooa "veřejného cloudu" infrastruktury.  

Cloudové brány může být namapovaný potenciálně do sítě virtualizace překrytí tooinsulate hello cloudové brány a všechna jeho připojená zařízení nebo brány pole od ostatního síťového přenosu. samotná brána cloudu Hello je ani řídicím systémem zařízení ani zpracování nebo zařízení úložiště pro data zařízení; Tyto vlastnosti rozhraní s hello Cloudová brána. Hello cloudové brány zóny obsahuje hello cloudu brána samotná společně s všechny brány pole a zařízení přímo nebo nepřímo připojené tooit. Hello okraji hello zóny je odlišné prostor oblasti, kde všechny externí strany komunikovat prostřednictvím.

### <a name="hello-services-zone"></a>zóny služby Hello
"Služba" je definována pro tento kontext jako všechny součásti softwaru nebo modul, který je během propojení s zařízení prostřednictvím brány pole nebo cloud pro shromažďování dat a analýzy, a také pro příkazy a ovládání.  Zprostředkovatelé jsou služby. Budou fungovat v rámci své identity směrem brány a dalších subsystémy, ukládat a analyzovat data, samostatně toodevices příkazy problém na základě dat po zobrazení statistiky nebo plány a vystavit informace a řízení tooauthorized koncoví uživatelé možnosti.

### <a name="information-devices-vs-special-purpose-devices"></a>Informace o zařízení oproti zařízeními pro zvláštní účely
Počítače, telefony a tablety jsou primárně interaktivní informace o zařízení. Telefony a tablety jsou explicitně optimalizované kolem maximalizace životnosti baterie. Jejich ideálně vypnout částečně při interakci s osoba není okamžitě, nebo když není poskytující služby jako přehrávání Hudba nebo vedení jejich vlastníkovi tooa konkrétního umístění. Z hlediska systémy jsou tato zařízení technologie informace především funguje jako proxy směrem osoby. Jsou to "osoby válcích" návrhy akce a "osoby senzory" shromažďování vstupu. 

Zařízeními pro zvláštní účely, z teploty jednoduché senzorů toocomplex objekt pro vytváření produkční řádků s tisíci součásti v nich, se liší. Tato zařízení jsou mnohem víc obor v účel a to i v případě, že obsahují některé uživatelské rozhraní, jsou z velké části oboru toointerfacing s nebo integrovat do prostředky ve fyzické hello, world. Jejich měření a sestavy prostředí okolností, zapněte ventily, řídit servos, zvukových výstrahy, přepínač indikátory a udělat celou řadu dalších úloh. Mohou pomoci toodo fungovat, pro které zařízení se systémem informace je příliš obecný, příliš nákladné, příliš velký nebo příliš křehká. konkrétní účel Hello okamžitě stanoví jejich technického návrhu jako dobře hello k dispozici peněžní nároky jejich produkčního prostředí a životnosti naplánované operace. Hello kombinace těchto zahrnuje dva klíčové faktory omezí hello k dispozici provozní energetické nároky, fyzické nároky a proto k dispozici úložiště, výpočetního prostředí a možnosti zabezpečení.  

Pokud něco "vloží nesprávný" s automatizované nebo vzdálené ovladatelné zařízení, například fyzická poškození nebo ovládací prvek logiku defekty toowillful Neautorizováno narušení a zpracování. Hello produkční mnoha může být zničený, budovy může být looted nebo zapsaný a osoby mohou být poškozené nebo dokonce die. Toto je samozřejmě zcela liší třída škod než někdo maxing out odcizené karty platební limit. Přesuňte technologie Hello panel zabezpečení pro zařízení, které věcí a také pro data snímačů, nakonec výsledky v příkazech, které způsobí toomove věcí, musí být vyšší než v libovolném elektronického obchodování nebo bankovnictví scénář. 

### <a name="device-control-and-device-data-interactions"></a>Ovládání zařízení a zařízení dat interakce
Připojená zařízení speciální mít velký počet potenciální interakce ploch a interakce vzorů, které je třeba zvážit tooprovide rozhraní pro zabezpečení digitální přístup toothose zařízení. Hello termín "Digitální přístup" je použité sem toodistinguish z jakékoli operace, které se provádějí pomocí interakci přímé zařízení, kde je zabezpečení přístupu zadaný prostřednictvím řízení fyzický přístup. Například uvedení hello zařízení do místnosti s zámek na dveře hello. Při fyzického přístupu nemůže být odepřeno pomocí softwaru a hardwaru, můžete se opatření tooprevent fyzický přístup z úvodní toosystem narušení. 

Jak jsme prozkoumat hello interakce vzory, podíváme se na "ovládání zařízení" a "data zařízení" s hello stejnou úroveň pozornost při modelování hrozeb. "Ovládání zařízení" můžou být klasifikované jako veškeré informace, které poskytuje tooa zařízení libovolné strany s cílem hello změna nebo ovlivňující své chování směrem k jeho stavu nebo stavu hello jeho prostředí. "Zařízení data" můžou být klasifikované jako jakékoli informace vysílá zařízení tooany druhou stranou o jeho stavu a hello zjištěnými stav jeho prostředí. 

## <a name="threat-modeling-hello-azure-iot-reference-architecture"></a>Referenční architektura Azure IoT hello modelování hrozeb
Společnost Microsoft používá hello framework uvedených výše toodo threat modelování pro Azure IoT. V následující části hello používáme proto hello konkrétní příklad toodemonstrate referenční architektura IoT Azure, jak identifikovat toothink o modelování hrozeb pro IoT a jak tooaddress hello hrozeb. V našem případě myslíme, čtyři hlavní oblasti fokus:

* Zařízení a zdroje dat
* Přenos dat
* Zařízení a zpracování událostí a
* Prezentace

![Hrozby modelování pro Azure IoT](media/iot-security-architecture/iot-security-architecture-fig2.png) 

Následující diagram Hello poskytuje zjednodušený přehled o architektuře IoT společnosti Microsoft pomocí modelu Diagram toku dat, který je používán hello Microsoft Threat modelování nástroj:

![Hrozby modelování pro Azure IoT pomocí nástroje pro modelování hrozeb MS](media/iot-security-architecture/iot-security-architecture-fig3.png)

Je důležité, že toonote, který hello architektura odděluje možností hello zařízení a brány. To umožňuje hello uživatele tooleverage zařízení brány, které jsou bezpečnější: jsou schopné komunikovat s hello cloudové brány, pomocí zabezpečených protokolů, která obvykle vyžaduje větší zpracování režie, může nativní zařízení – například termostatu - Zadejte svoje vlastní. V zóně hello služby Azure předpokládáme, že hello Cloudová brána je reprezentována hello služby Azure IoT Hub.

### <a name="device-and-data-sourcesdata-transport"></a>Přenosová vrstva služby zdroje nebo data zařízení a dat
V této části jsou zde popsány hello architektura uvedených výše prostřednictvím hello přehledu modelování hrozeb a poskytuje přehled o tom, jak některé aspekty vyplývajících hello jsou adresování jsme. Se zaměříme na model hrozeb hello základní prvky:

* Procesy (ty v rámci našeho řízení a externí položky)
* Komunikace (také nazývané toky dat)
* Úložiště (také nazývané úložiště dat)

#### <a name="processes"></a>Procesy
V každém hello kategorií uvedených v architektuře Azure IoT hello se pokusíme toomitigate v existuje několik různých hrozeb v různých fázích hello data nebo informace: proces komunikace a úložiště. Níže budeme poskytnutí přehledu o hello nejběžnější ty, které jsou pro kategorii "proces" hello, za nímž následuje přehled o tom, jak to může být nejlépe omezeny: 

**Falšování identity (S)**: útočník může extrahovat materiál kryptografické klíče ze zařízení, buď na úrovni hello softwaru nebo hardwaru a následně přístup k systému hello s identitou hello hello zařízení hello jiné fyzické nebo virtuální zařízení materiál klíče byly převzaty z. Dobrý obrázku je vzdálené ovládací prvky, který můžete zapnout jakékoli Televizi a oblíbených prankster nástroje, které jsou.

**Útok na dostupnost služby (D)**: zařízení mohou být vykresleny nepodporující funguje nebo komunikaci pomocí zasahovala do činnosti rádiových frekvencí nebo vyjímání vodičům. Například sledováním fotoaparát, který měl jeho napájení nebo síťového připojení záměrně vykrojí nebudou data sestavy, vůbec.

**Manipulaci (T)**: útočník může částečně nebo zcela nahradit hello softwaru spuštěné na hello zařízení, což může nahradit hello softwaru tooleverage hello originální identity zařízení hello, pokud hello klíče materiálu nebo hello kryptografických zařízení, která uchovává klíče materiály byly k dispozici toohello nedovolené programu. Například útočník může využít extrahované klíče podstatným toointercept a potlačit data ze zařízení hello v cestě hello komunikace a nahraďte ji metodou false data, která je k ověření pomocí hello odcizení materiál klíče.

**Přístup k informacím (I)**: Pokud hello zařízení běží zpracovatelné softwaru, takový zpracovatelné software může potenciálně úniku dat toounauthorized strany. Například může útočník využít extrahované klíče podstatným tooinject samotné do hello komunikační trasa mezi zařízením hello a bránu řadiče nebo pole nebo Cloudová brána toosiphon vypnout informace.

**Zvýšení oprávnění (E)**: zařízení, která provádí konkrétní funkce může být vynucené toodo něco jiného. Například ventil, který je naprogramovaných tooopen polovinu způsobem může být, aby všechny hello tooopen způsobem.

| **Komponenta** | **Hrozby** | **Zmírnění dopadů** | **Riziko** | **Implementace** |
| --- | --- | --- | --- | --- |
| Zařízení |S |Přiřazení zařízení toohello identity a ověřování zařízení hello |Nahrazení součástí hello zařízení nebo zařízení s jiným zařízením. Jak jsme si vědomi, že jsme mluvíme toohello správné zařízení? |Ověřovací hello zařízení pomocí zabezpečení TLS (Transport Layer) nebo protokol IPSec. Infrastruktura by měla podporovat použití předsdílený klíč (PSK) na těchto zařízeních, které nelze zpracovat úplné asymetrické šifrování. Využít Azure AD, [OAuth](http://www.rfc-editor.org/in-notes/internet-drafts/draft-ietf-ace-oauth-authz-01.txt) |
| TRID |Například použijte tamperproof mechanismy toohello zařízení tím, že ji velmi obtížné tooimpossible tooextract klíčů a jiných kryptografických materiálu z hello zařízení. |riziko Hello je, pokud někdo je falšování hello zařízení (fyzického narušení). Jak jsme je opravdu, nebylo manipulováno toto zařízení. |co nejúčinnější zmírnění Hello je funkcí module (TPM) důvěryhodné platformy, které umožňuje ukládání klíčů v speciální na čipu zapojení, ze které hello klíče nelze číst, ale lze použít pouze pro kryptografické operace využívající klíč hello, ale nikdy zveřejnit hello klíč . Paměť šifrování zařízení hello. Správy klíčů pro hello zařízení. Podepisování kódu hello. | |
| E |S řízení přístupu hello zařízení. Schéma ověřování. |Pokud zařízení hello umožňuje pro jednotlivé akce toobe provést podle příkazy z vnějšího zdroje, nebo i dojde k ohrožení senzorů, bude možné hello útoku tooperform operations jinak přístupné. |S schéma ověřování pro zařízení hello | |
| Brána pole |S |Ověřování hello pole brány tooCloud brány (na základě certifikátu, PSK, deklarace identity na základě,...) |Pokud někdo může zfalšovat brána pole, pak se může zobrazit i jako jakékoli zařízení. |Protokol TLS RSA/PSK, IPSec, [RFC 4279](https://tools.ietf.org/html/rfc4279). Všechny hello stejné úložiště klíčů a riziko z hlediska ověření zařízení v obecné – případ nejlepší je použít čip TPM. 6LowPAN rozšíření pro protokol IPSec toosupport bezdrátové senzor sítě (WSN). |
| TRID |Ochrana hello brána pole proti manipulaci (TPM)? |Falšování identity útoků, které oklamat hello Cloudová brána myslím, že ho je rozhovoru toofield brány může mít za následek zpřístupnění informací a manipulaci dat |Paměť šifrování, TPM společnosti, ověřování. | |
| E |Mechanismus řízení přístupu pro brána pole | | | |

Tady jsou některé příklady hrozeb v této kategorii:

Falšování identity: Útočník může extrakce materiál kryptografické klíče ze zařízení, buď na úrovni hello softwaru nebo hardwaru a následně přístup byl systém hello s jiné fyzické nebo virtuální zařízení pod identitou hello hello zařízení hello materiál klíče odebrat.

**Odmítnutí služby**: zařízení mohou být vykresleny nepodporující funguje nebo komunikaci pomocí zasahovala do činnosti rádiových frekvencí nebo vyjímání vodičům. Například sledováním fotoaparát, který měl jeho napájení nebo síťového připojení záměrně vykrojí nebudou data sestavy, vůbec.

**Manipulaci**: útočník může částečně nebo zcela nahradit hello softwaru spuštěné na hello zařízení, což může nahradit hello softwaru tooleverage hello originální identity zařízení hello, pokud hello klíče materiálu nebo hello kryptografických zařízení, která uchovává klíče materiály byly k dispozici toohello nedovolené programu.

**Manipulaci**: sledováním fotoaparát, který se zobrazuje viditelné spektrum přehled o prázdný hallway může zaměřené na fotografie takové hallway. Senzor kouř nebo ještě efektivněji může reporting někdo podržíte světlejšího v něm. V obou případech hello zařízení může být technicky plně důvěryhodný směrem hello systému, ale bude hlášené zpracovatelné informace.

**Manipulaci**: útočník může využít extrahované klíče podstatným toointercept a potlačit data ze zařízení hello v cestě hello komunikace a nahraďte ji metodou false data, která je k ověření pomocí hello odcizení materiál klíče.

**Manipulaci**: útočník může částečně nebo zcela nahradit hello softwaru spuštěné na hello zařízení, což může nahradit hello softwaru tooleverage hello originální identity zařízení hello, pokud hello klíče materiálu nebo hello kryptografických zařízení, která uchovává klíče materiály byly k dispozici toohello nedovolené programu.

**Zpřístupnění informací**: Pokud hello zařízení běží zpracovatelné softwaru, takový zpracovatelné software může potenciálně úniku dat toounauthorized strany.

**Zpřístupnění informací**: útočník může využít extrahované klíče podstatným tooinject samotné do hello komunikační trasa mezi zařízením hello a bránu řadiče nebo pole nebo cloudové brány toosiphon vypnout informace.

**Odmítnutí služby**: hello zařízení může být vypnutý nebo převedena na režim, kde komunikace není možná (což je úmyslné v mnoha průmyslových strojů).

**Manipulaci**: hello zařízení může být změněnou konfigurací toooperate ve stavu neznámé toohello řízení systému (mimo známých kalibračních parametrů) a tak poskytuje data, která může být nesprávné interpretaci

**Zvýšení úrovně oprávnění**: zařízení, která provádí konkrétní funkce může být vynucené toodo něco jiného. Například ventil, který je naprogramovaných tooopen polovinu způsobem může být, aby všechny hello tooopen způsobem.

**Odmítnutí služby**: hello zařízení mohou být změněny na stav, kdy komunikace není možná.

**Manipulaci**: hello zařízení může být změněnou konfigurací toooperate ve stavu neznámé toohello řízení systému (mimo známých kalibračních parametrů) a tak poskytuje data, která může být nesprávné interpretaci.

**Falšování, Nepovolená manipulace/Odvolatelnost**: Pokud není zabezpečen (což je zřídka hello případ s ovládacími prvky pro vzdálené k příjemce) útočník můžete upravit anonymně hello stav zařízení. Dobrý obrázku je vzdálené ovládací prvky, který můžete zapnout jakékoli Televizi a oblíbených prankster nástroje, které jsou.

#### <a name="communication"></a>Komunikace
Hrozby kolem cesta komunikaci mezi zařízeními, zařízení a pole brány a zařízení a cloudové brány. Následující tabulka Hello má některé pokyny kolem otevřete sockets na hello zařízení a sítě VPN:

| **Komponenta** | **Hrozby** | **Zmírnění dopadů** | **Riziko** | **Implementace** |
| --- | --- | --- | --- | --- |
| Zařízení IoT Hub |TID |(D) Provoz hello tooencrypt TLS (PSK/RSA) |Odposlouchávání nebo vzájemnému hello komunikaci mezi zařízením hello a hello brány |Zabezpečení na úrovni protokolu hello. S vlastní protokoly, potřebujeme toofigure jak tooprotect je. Ve většině případů hello komunikace probíhá z toohello hello zařízení IoT Hub (hello připojení iniciuje zařízení). |
| Zařízení zařízení |TID |(D) Protokol TLS (PSK/RSA) tooencrypt hello provoz. |Čtení dat během přenosu mezi zařízeními. Manipulaci s daty hello. Přetížení hello zařízení s nových připojení |Zabezpečení na úrovni protokolu hello (MQTT nebo AMQP nebo HTTP/CoAP. S vlastní protokoly, potřebujeme toofigure jak tooprotect je. Hello zmírnění dopadů pro hello DoS threat je toopeer zařízení prostřednictvím brány cloudu nebo pole a mít je jenom akce jako klienti směrem hello sítě. partnerský vztah Hello může vést k přímé připojení mezi rovnocennými počítači hello po s byla zprostředkované bránou hello |
| Externí Entity zařízení |TID |Silné párování hello externí entity toohello zařízení |Odposlechu hello připojení toohello zařízení. Rušivých hello komunikace s hello zařízení |Bezpečně párování hello externí entity toohello zařízení LE NFC/Bluetooth. Řízení provozu panely hello hello zařízení (fyzické) |
| Brána cloudové brány pole |TID |Protokol TLS (PSK/RSA) tooencrypt hello provoz. |Odposlouchávání nebo vzájemnému hello komunikaci mezi zařízením hello a hello brány |Zabezpečení na úrovni protokolu hello (MQTT nebo AMQP nebo HTTP/CoAP). S vlastní protokoly, potřebujeme toofigure jak tooprotect je. |
| Zařízení brány cloudu |TID |Protokol TLS (PSK/RSA) tooencrypt hello provoz. |Odposlouchávání nebo vzájemnému hello komunikaci mezi zařízením hello a hello brány |Zabezpečení na úrovni protokolu hello (MQTT nebo AMQP nebo HTTP/CoAP). S vlastní protokoly, potřebujeme toofigure jak tooprotect je. |

Tady jsou některé příklady hrozeb v této kategorii:

**Odmítnutí služby**: omezené zařízení jsou obecně ohrožené DoS při jejich aktivně naslouchat příchozí připojení nebo nevyžádané datagramy v síti, protože útočník není možné otevřít mnoha připojení paralelně a jejich služby nebo služby je velmi pomalu nebo hello zařízení může být zaplnění nevyžádaný provoz. V obou případech hello zařízení, které lze účinně vykreslit přestane fungovat v síti hello.

**Falšování identity, zpřístupnění informací**: omezené zařízeními a zařízeními pro zvláštní účely často mít jeden pro all zabezpečení jako heslo nebo PIN kód ochrany, nebo zcela spoléhají na důvěřující hello síť, což znamená, že udělí přístup Když je zařízení na hello tooinformation stejné síti a že síť je často jenom chráněn sdílený klíč. To znamená, že když hello sdílený tajný toodevice nebo jsou zveřejňovány sítě, je možné toocontrol hello zařízení nebo pozorovat data vygenerované ze zařízení hello.  

**Falšování identity**: útočník může zachytávat nebo částečně přepsat hello všesměrové vysílání a falešná identita původce hello (man uprostřed hello)

**Manipulaci**: útočník může zachytávat nebo částečně přepsat hello vysílání a odeslat nepravdivé informace 

**Přístup k informacím:** útočník může tajně poslouchat vysílání a získat informace o bez autorizace **Denial of Service:** útočník může zablokování hello vysílání signální a odepřít distribuční informace

#### <a name="storage"></a>Úložiště
Každé zařízení a pole brány má určitou formu úložiště (dočasný pro data služby Řízení front hello úložiště bitové kopie operačního systému (OS)).

| **Komponenta** | **Hrozby** | **Zmírnění dopadů** | **Riziko** | **Implementace** |
| --- | --- | --- | --- | --- |
| Úložiště zařízení. |TRID |Šifrování úložiště, podepisování hello protokoly |Čtení dat z úložiště hello (PII data), manipulaci s telemetrická data. Manipulaci s zařazených do fronty nebo ukládání do mezipaměti příkaz data ovládacího prvku. Manipulaci s konfigurace nebo firmware balíčky aktualizací při do mezipaměti nebo místně do fronty může způsobit tooOS nebo systému součásti ohrožení |Šifrování, ověřovací kód zprávy (MAC) nebo digitální podpis. Kde řízení možné silné přístup prostřednictvím přístupu k prostředkům řízení oprávnění nebo seznamy (ACL). |
| Bitové kopie operačního systému zařízení |TRID | |Manipulaci s operačním systémem / nahrazení komponent hello operačního systému |Jen pro čtení oddílu operačního systému podepsané bitové kopie operačního systému, šifrování |
| Brána pole úložiště (data služby Řízení front hello) |TRID |Šifrování úložiště, podepisování hello protokoly |Čtení dat z úložiště hello (PII data), manipulaci s daty telemetrie manipulaci s zařazených do fronty nebo ukládání do mezipaměti příkaz data ovládacího prvku. Manipulaci s balíčky aktualizací pro konfiguraci a firmware (určené pro zařízení nebo brána pole) do mezipaměti nebo místně do fronty může způsobit tooOS nebo systému součásti ohrožení |Nástroj BitLocker |
| Image OS brána pole |TRID | |Manipulaci s operačním systémem / nahrazení komponent hello operačního systému |Jen pro čtení oddílu operačního systému podepsané bitové kopie operačního systému, šifrování |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Zařízení a události zpracování nebo cloudové brány zóny
Cloudové brány je systém, který umožňuje vzdálenou komunikaci z a toodevices nebo pole brány z několika různých lokalit prostoru veřejnou síť, obvykle k cloudové řízení a datové analýzy systému federace těchto systémů. V některých případech může usnadnit cloudové brány okamžitě toospecial účel zařízení pro přístup z terminály, jako například tablety nebo telefony. V kontextu hello popsané tady "cloud" se rozumí toorefer tooa vyhrazené zpracování dat systému, která není vázaná toohello stejné lokalitě jako hello připojen zařízení nebo pole brány, a jestliže by provozní míry cílová fyzický přístup, ale není nutně tooa "veřejného cloudu" infrastruktury.  Cloudové brány může být namapovaný potenciálně do sítě virtualizace překrytí tooinsulate hello cloudové brány a všechna jeho připojená zařízení nebo brány pole od ostatního síťového přenosu. samotná brána cloudu Hello je ani řídicím systémem zařízení ani zpracování nebo zařízení úložiště pro data zařízení; Tyto vlastnosti rozhraní s hello Cloudová brána. Hello cloudové brány zóny obsahuje hello cloudu brána samotná společně s všechny brány pole a zařízení přímo nebo nepřímo připojené tooit.

Brána cloudu je většinou vlastní integrované část softwaru jako službu s brána pole toowhich zveřejněné koncových bodů a připojení zařízení. Jako takový musí být vytvořeny s důrazem na bezpečnost. Postupujte podle [SDL](http://www.microsoft.com/sdl) procesů pro navrhování a vytváření této služby. 

#### <a name="services-zone"></a>Zóny služby
Ovládací prvek systému (nebo řadič) je softwarové řešení, které sdílí rozhraní se zařízení, nebo brána pole nebo Cloudová brána hello za účelem řízení jeden nebo více zařízení nebo toocollect nebo úložiště a analyzovat data zařízení pro prezentaci, nebo pro účely další řízení. Ovládací prvek systémy jsou hello pouze entity v oboru hello toto pojednání, která by mohla okamžitě usnadnit interakce s uživateli. Hello výjimky jsou zprostředkující fyzické ovládací prvek ploch na zařízení, jako přepínač, který umožňuje osoby tooturn hello zařízení vypnout nebo změnit další vlastnosti, a pro který není žádný funkční ekvivalent, která je přístupná digitálně. 

Zprostředkující fyzické povrchy řízení jsou ty, kde jakéhokoliv druhu řídících logiku omezí hello funkce řízení povrchu fyzické hello tak, aby ekvivalentní funkce lze inicializovat vzdáleně nebo vstupní je v konfliktu s vzdálené vstup může být, kterým se lze vyhnout – takové intermediated řízení tři oblasti jsou koncepčně připojené tooa místní ovládací systém využívá jako všechny ostatní systémy vzdálené řízení, které hello zařízení může být připojené tooin paralelní text hello stejné základní funkce. Horní hrozeb toohello cloud computing, můžete si jej přečíst v [cloudu zabezpečení Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/) stránky.

## <a name="additional-resources"></a>Další zdroje
Odkažte toohello následující články pro další informace:

* [Nástroj modelování hrozeb SDL](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
* [Referenční Architektura Microsoft Azure IoT](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)

