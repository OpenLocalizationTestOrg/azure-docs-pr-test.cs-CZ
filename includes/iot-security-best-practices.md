# <a name="internet-of-things-security-best-practices"></a>Osvědčené postupy pro zabezpečení Internetu věcí
toosecure infrastruktury Internetu věcí (IoT) vyžaduje přísných strategii zabezpečení do hloubky. Tato strategie vyžaduje, abyste toosecure dat v cloudu hello, chránit integritu dat v průběhu přenosu přes hello veřejného Internetu a bezpečně zřizovat zařízení. Jednotlivé úrovně sestavení větší zajištění zabezpečení v hello celé infrastruktury.

## <a name="secure-an-iot-infrastructure"></a>Zabezpečení infrastruktury IoT
Tato strategie zabezpečení do hloubky můžete vyvinuté a provést s aktivní účast různé přehrávače spojené s hello výrobní, vývoj a nasazení zařízení IoT a infrastruktury. Následuje stručný popis těchto přehrávače.  

* **Výrobce hardwaru IoT/integrátor**: Toto jsou obvykle hello výrobců hardwaru IoT nasazuje integrátorem ty se hardware od různých výrobců nebo dodavatelů, poskytuje hardwaru pro nasazení služby IoT vyrobila nebo integrované jinými dodavateli.
* **Vývojář řešení IoT**: hello vývoji řešení IoT se obvykle provádí vývojář řešení. Tento vývojář může součástí interní týmu nebo systémový integrátor (SI) specializované této aktivity. Hello vývojáře řešení IoT můžete vyvíjet různé součásti hello řešení IoT od začátku, integrovat různé součásti dodávaných nebo open-source nebo přijmout předkonfigurované řešení s menšími přizpůsobení.
* **Nástroje pro nasazení řešení IoT**: vyvinutý po IoT řešení, je nutné toobe nasazené v poli hello. To zahrnuje nasazení hardwaru, propojení zařízení a nasazení řešení v hardwarových zařízeních nebo hello cloudu.
* **Operátor řešení IoT**: po nasazení hello řešení IoT vyžaduje dlouhodobé provoz, sledování, upgradu a údržby. Tento krok můžete provést interní tým, který obsahuje informace o technologii odborníky, hardwarových operací a údržby týmů a odborníky domény, kteří monitorování hello správné fungování celé infrastruktury IoT.

Hello části obsahují osvědčené postupy pro každou z těchto toohelp přehrávače vyvíjet, nasadit a provozovat zabezpečené infrastruktury IoT.

## <a name="iot-hardware-manufacturerintegrator"></a>Výrobce hardwaru IoT/integrátor
Hello následují hello osvědčené postupy pro IoT výrobcům hardwaru a integrátorem hardwaru.

* **Obor požadavky na hardware toominimum**: návrh hardwaru hello by měla zahrnovat nutných pro operaci hello hardwaru a nic další minimální funkce hello. Příkladem je portů USB tooinclude pouze v případě potřeby pro operaci hello hello zařízení. Tyto další funkce Otevřít hello zařízení pro nežádoucí útoky, které by se mělo zabránit.
* **Ujistěte se, hardwaru manipulovat ověření**: sestavení v mechanismy toodetect fyzické manipulaci, jako je například otevírání hello zařízení krytí nebo odebrání součástí hello zařízení. Tyto manipulovat signály mohou být součástí hello data datového proudu nahrán toohello cloudu, což může výstrahy operátory tyto události.
* **Sestavení kolem zabezpečený hardware**: Pokud spotřebu povoluje, sestavení funkce zabezpečení, jako je zabezpečená a šifrovaná úložiště nebo spouštěcí funkce založené na čipu Trusted Platform Module (TPM). Tato funkce zařízení zkontrolujte informace zabezpečení a ochraně hello celé infrastruktury IoT.
* **Zabezpečit upgrady**: upgrade firmwaru během doby života hello hello zařízení je nevyhnutelné. Vytváření zařízení s zabezpečené cesty pro upgrade a kryptografické záruku verzí firmwaru vám umožní hello zařízení toobe zabezpečené během a po upgradu.

## <a name="iot-solution-developer"></a>Vývojář řešení IoT
Hello následují hello osvědčené postupy pro vývojáře řešení IoT:

* **Postupujte podle zabezpečené softwaru vývoj metodika**: vývoj zabezpečené softwaru vyžaduje základů přemýšlíte o zabezpečení z hello zahájení projektu hello všechny hello způsob tooits implementace, testování a nasazení. Hello volby platforem, jazyků a nástroje jsou vliv pomocí této metody. Hello Microsoft Security Development Lifecycle poskytuje podrobný postup toobuilding zabezpečení softwaru.
* **Zvolte open-source softwaru pečlivě**: Open-source softwaru vám dává příležitost tooquickly vývoj řešení. Pokud zvolíte open-source softwaru, zvažte úrovni aktivity hello hello komunity pro každou součást open source. Aktivní komunitě zajišťuje, že software podporuje a zjišťování a řešit problémy. Případně se nemusí být podporován skrytého a neaktivní open-source softwaru a budou pravděpodobně nejsou zjištěny problémy.
* **Integrovat dát pozor**: řadu nedostatků zabezpečení softwaru existovat hello hranice knihovny a rozhraní API. Funkce, které nemusí být požadovány pro aktuální nasazení hello může být k dispozici prostřednictvím vrstvu rozhraní API. tooensure celkové zabezpečení, ujistěte se, že toocheck všech rozhraní součásti, kterou je prováděna integrace pro chyby zabezpečení.      

## <a name="iot-solution-deployer"></a>Nástroje pro nasazení řešení IoT
Hello následují osvědčené postupy pro IoT řešení moduly pro nasazení softwaru:

* **Nasazení hardwaru bezpečně**: IoT nasazení může vyžadovat toobe hardware nasazený v nezabezpečená umístění, například veřejné mezery nebo bez dohledu národní prostředí. V takových situacích zajistěte, aby byl nasazení hardwaru v maximálním možném rozsahu toohello manipulací. Pokud USB nebo jiné porty jsou dostupné na hello hardwaru, ujistěte se, zahrnutých bezpečně. Mnoho vektory útoku můžete použít jako vstupní body.
* **Chránit ověřovací klíče**: během nasazování každé zařízení vyžaduje ID zařízení a související ověřovací klíče generované hello cloudové služby. Chránit tyto klíče fyzicky i po nasazení hello. Všechny ohrožené klíč lze toomasquerade škodlivý zařízení jako ze stávajících zařízení.

## <a name="iot-solution-operator"></a>Operátor řešení IoT
Hello následují hello osvědčené postupy pro operátory řešení IoT:

* **Zachovat provozu toodate systému hello**: Zkontrolujte, zda operační systémy zařízení a všechny ovladače zařízení jsou upgradovaný toohello nejnovější verze. Pokud zapnete automatické aktualizace v systému Windows 10 (IoT nebo jiných SKU), Microsoft udržuje ho nahoru toodate, zajištění zabezpečení operačního systému pro zařízení IoT. Zachování jinými operačními systémy (například Linux) až toodate pomáhá zajistit, že jsou taky chráněná před útoky se zlými úmysly.
* **Ochrana proti škodlivé aktivity**: Pokud hello operačního systému povolí, nainstalujte nejnovější antivirového a antimalwarového možnosti hello na každý operační systém zařízení. To může pomoci zmírnit hrozby nejvíce externí. Většina moderních operačních systémů proti hrozbám můžete chránit pomocí příslušné kroky.
* **Audit často**: auditování IoT infrastruktury pro problémy související se zabezpečením je klíč při odpovědi toosecurity incidenty. Většina operačních systémů poskytují protokolování integrované událostí, který by měl být zkontrolovány často toomake se, že žádná porušení zabezpečení došlo k chybě. Informace o auditování lze odesílat jako samostatné telemetrie datového proudu toohello cloudovou službu, kde lze analyzovat.
* **Fyzicky ochrana hello IoT infrastruktury**: hello nejhorší zabezpečení útoky na infrastrukturu IoT spustily pomocí toodevices fyzický přístup. Jeden důležité zabezpečení postupem je tooprotect před zneužitím portů USB a dalších fyzický přístup. Jeden klíč toouncovering narušení, jež mohly nastat je protokolování fyzický přístup, jako je například použití portu USB. Windows 10 (IoT a další identifikátory SKU) znovu, zapne podrobné protokolování těchto událostí.
* **Ochranu přihlašovacích údajů cloudu**: cloudové ověřování pověření použitá pro konfiguraci a provozní nasazení služby IoT se pravděpodobně hello nejjednodušší způsob, jak toogain přístup a ohrozit systém IoT. Ochranu přihlašovacích údajů hello změnou hesla hello často a nepoužívejte tyto přihlašovací údaje na veřejné počítače.

Možnosti různých zařízení IoT se liší. Některá zařízení může být počítače se systémem běžné operační systémy a některá zařízení může být velmi šedé operačním systémem. osvědčené postupy pro Hello zabezpečení popsané dříve může být zařízení použít toothese v různých stupňů. Pokud je zadán, musí pak další nasazení osvědčené postupy pro zabezpečení a od výrobců hello těchto zařízení.

Některá zařízení starší verze a omezené nemusí byl určený speciálně pro IoT nasazení. Tato zařízení může nedostatku hello schopností tooencrypt data, připojení k Internetu hello nebo zadejte pokročilé auditování. Brána pole moderní a zabezpečení v těchto případech můžete shromažďovat data ze zařízení se starší verzí a hello zabezpečení potřebné pro tato zařízení připojující se přes hello Internet. Pole brány může poskytovat vyjednávání šifrovaných relací zabezpečeného ověřování, obdržení příkazy z cloudu hello a řadu dalších funkcí zabezpečení.

