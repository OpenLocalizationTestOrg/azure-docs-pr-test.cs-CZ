
# <a name="azure-and-internet-of-things"></a>Azure a internet věcí

Vítejte tooMicrosoft Azure a hello Internet věcí (IoT). Tento článek představuje architekturu řešení IoT popisující běžné vlastnosti řešení IoT, které můžete nasadit pomocí služeb Azure hello. Řešení IoT vyžadují zabezpečení, obousměrnou komunikaci mezi zařízeními, jejichž počet může jít v hello miliony a back-end řešení. Back-end řešení může například použít automatizované prediktivní analýzy toouncover statistiky z datového proudu událostí zařízení cloud.

Služba Azure IoT Hub je klíčovým stavebním blokem při implementaci této architektury řešení IoT pomocí služeb Azure. Sada IoT Suite poskytuje kompletní, ucelené implementace této architektury pro konkrétní scénáře IoT. Například:

* Hello *vzdálené monitorování* řešení vám umožní toomonitor hello stavu zařízení, třeba prodejních automatů.
* Hello *prediktivní údržby* řešení vám usnadní tooanticipate údržby musí zařízení, například čerpadel na vzdálené čerpací stanici a tooavoid mimo plánované výpadky.
* Hello *připojené factory* řešení vám pomůže tooconnect a monitorovat průmyslových zařízení.

## <a name="iot-solution-architecture"></a>Architektura řešení IoT

Hello následující diagram ukazuje typickou architekturu řešení IoT. Hello diagram nezahrnuje hello názvy konkrétním služby Azure, ale popisuje klíčové prvky hello v obecné architektuře řešení IoT. V této architektuře zařízení IoT shromažďují data odesílání tooa Cloudová brána. Hello Cloudová brána zpřístupní hello data pro zpracování i jiné služby back-end, ze kterých se data doručena tooother-obchodní aplikace nebo toohuman operátory prostřednictvím řídicího panelu nebo jiného prezentačního zařízení.

![Architektura řešení IoT][img-solution-architecture]

> [!NOTE]
> Podrobné informace o architektuře IoT najdete v části hello [referenční architektura IoT Microsoft Azure][lnk-refarch].

### <a name="device-connectivity"></a>Připojení zařízení

Zařízení v této architektuře řešení IoT odesílají telemetrická data, například odečty snímačů z čerpací stanice, koncový bod cloudu tooa pro úložiště a zpracování. Ve scénáři prediktivní údržby může back-end hello řešení použít datový proud hello toodetermine data snímačů při konkrétní čerpadlo vyžaduje údržbu. Zařízení můžete také přijímat a odpovídat zprávy toocloud zařízení tak, že čtení zpráv z koncového bodu cloudu. Například v řešení pro scénář hello hello prediktivní údržby back-end může odesílat zprávy tooother čerpadel v hello čerpací stanice toobegin přesměrování spojnic toky těsně před údržbou toostart. Tento postup by Ujistěte se, že pracovníkem údržby hello může začít hned, jak dorazí.

Jednou z největších výzev hello projekty IoT čelí je jak tooreliably a bezpečné připojení zařízení toohello back-end řešení. Zařízení IoT mají různé vlastnosti jako porovnání tooother klientů, jako například prohlížečů a mobilních aplikací. Zařízení IoT:

* Jsou často vestavěnými systémy bez lidské obsluhy.
* Mohou být nasazená ve vzdálených umístěních, kam je fyzický přístup nákladný.
* Můžou být dostupná jenom prostřednictvím back-end hello řešení. Není k dispozici žádné další způsob toointeract s hello zařízení.
* Můžou mít omezené prostředky pro napájení a zpracování.
* Můžou mít přerušované, pomalé nebo nákladné síťové připojení.
* Může být nutné toouse aplikace vlastní, vlastní nebo průmyslové protokoly.
* Můžou být vytvořená pomocí rozsáhlé sady oblíbených hardwarových a softwarových platforem.

Kromě výše uvedených požadavků toohello, jakékoli řešení IoT musí zajistit také škálování, zabezpečení a spolehlivost. Hello výslednou sadu požadavků na připojení je obtížné tooimplement pomocí tradičních technologií, jakými jsou webové kontejnery a zprostředkovatelé zasílání zpráv. Azure IoT Hub a hello SDK pro zařízení Azure IoT, aby jednodušší tooimplement řešení, které splňují tyto požadavky.

Zařízení může komunikovat přímo s bodem cloudové brány, nebo pokud zařízení hello nemůžete použít žádnou z hello komunikační protokoly, které hello cloudu brána podporuje, se může připojit prostřednictvím zprostředkující brány. Například hello [brány protokolu Azure IoT] [ lnk-protocol-gateway] můžete provádět překlad protokolu, pokud zařízení nemůžete použít žádnou z hello protokoly, které podporuje IoT Hub.

### <a name="data-processing-and-analytics"></a>Zpracování a analýza dat

V cloudu hello IoT back-end řešení je, kde většinu hello zpracování dat dojde, jako je například filtrování a agregování telemetrických dat a jeho tooother služby směrování. Hello back-endu IoT řešení:

* Přijímá škálovaná telemetrická data ze zařízení a určuje, jak tooprocess a ukládat data. 
* Můžete toosend příkazy z hello cloud toospecific zařízení.
* Poskytuje možnosti registrace zařízení, které umožňují tooprovision zařízení a toocontrol zařízení, která jsou povolena tooconnect tooyour infrastruktury.
* Umožňuje vám tootrack hello stavu zařízení a sledování jejich aktivit.

Ve scénáři prediktivní údržby hello back-end hello řešení ukládá historická telemetrická data. back-end Hello řešení můžete použít tento vzorů tooidentify toouse dat, které ukazují potřebu údržby u konkrétního čerpadla.

Řešení IoT může obsahovat smyčky automatické zpětné vazby. Analytický modul v back-end hello řešení můžete například určit z telemetrických dat, který je hello teplota konkrétního zařízení překračuje běžnou provozní úroveň. řešení Hello potom může odeslat příkaz toohello zařízení, pokyny tootake opravné akce.

### <a name="presentation-and-business-connectivity"></a>Prezentační a obchodní připojení

vrstva Hello prezentačního a obchodního připojení umožňuje koncovým uživatelům toointeract s hello řešení IoT a hello zařízení. Ji umožňuje uživatelům tooview a analyzovat hello data shromážděná z jejich zařízení. Tato zobrazení můžou mít hello formuláře řídicích panelů nebo sestav BI, které můžete zobrazit historická data i nebo téměř v reálném čase data. Operátor můžete například zkontrolovat hello stav konkrétní čerpací stanice a zobrazit všechny výstrahy vyvolané systémem hello. Tato vrstva také umožňuje integraci hello IoT back-end řešení s existující tootie-obchodní aplikace do podnikových obchodních procesů nebo pracovních postupů. Například řešení prediktivní údržby hello můžete integrovat s plánovacím systémem, že knihy pracovníkem toovisit čerpací stanice při hello řešení zjistí, že některé čerpadlo potřebuje údržbu.

![Řídicí panel řešení IoT][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
