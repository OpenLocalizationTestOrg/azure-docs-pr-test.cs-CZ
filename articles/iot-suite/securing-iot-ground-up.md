---
title: "aaaSecuring vaše Internet věcí z hello pozadí až | Microsoft Docs"
description: "Tento článek popisuje funkcím zabezpečení hello hello Microsoft Azure IoT Suite"
services: 
suite: iot-suite
documentationcenter: 
author: YuriDio
manager: timlt
editor: 
ms.assetid: 10252dfa-8313-4a97-9bd6-a3f1345dd3be
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: yurid
ms.openlocfilehash: a97e8cea753641e1e3c895f44e3fde1e5739d665
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-from-hello-ground-up"></a>Zabezpečení Internetu věcí z hello pozadí
Hello Internet věcí (IoT) představuje jedinečné zabezpečení, ochrany osobních údajů a dodržování předpisů výzvy toobusinesses po celém světě. Na rozdíl od tradičních internetový technologie, kde tyto problémy základem softwaru a o tom, jak je implementována IoT se vztahuje na co se stane, když internetový hello a fyzické světů hello sloučit. Ochrana řešení IoT vyžaduje zajištění zabezpečené zřizování zařízení, zabezpečené připojení mezi těchto zařízení a hello cloudu a ochranu zabezpečení dat v cloudu hello při zpracování a úložiště. Pracovat se tyto funkce, ale jsou zařízení s omezenými zdroji, geografické rozptýlení nasazení a velký počet zařízení v rámci řešení.

Tento článek popisuje, jak hello Microsoft Azure IoT Suite nabízí zabezpečení a privátní cloudové řešení Internetu věcí. Hello Azure IoT Suite nabízí úplného začátku do konce řešení, se zabezpečení integrovaná v každé fázi z hello pozadí. Společnost Microsoft se vývoj softwaru pro zabezpečení je součástí hello softwaru inženýrství postupem je integrován do našich desetiletí dlouho prostředí vývoje zabezpečení softwaru. tooensure tohoto životního cyklu SDL (Security Development) je hello základní vývoj metodika, spolu se hostitel služeb zabezpečení na úrovni infrastruktury, jako je provozní zajištění zabezpečení (OSA) a hello Microsoft jejichž šíření tým Dcu, Microsoft Security Response Center a Microsoft Malware Protection Center. 

Hello Azure IoT Suite nabízí jedinečné funkce, které zřizování, připojení k a ukládání dat ze zařízení IoT snadný a transparentní a nejvyšší všech, zabezpečení. V tomto článku jsme zkontrolujte funkce zabezpečení Azure IoT Suite hello a řešeny problémy zabezpečení, ochrany osobních údajů a dodržování předpisů strategie tooensure nasazení. 

## <a name="introduction"></a>Úvod
Hello Internet věcí (IoT) je hello wave Dobrý den budoucí, nabízí firmám okamžitou a náklady na tooreduce reálného příležitostí, zvýšit výnosy a transformace své firmy. Mnoho firem jsou však odhodlání toodeploy IoT v jejich organizace kvůli tooconcerns o zabezpečení, ochrany osobních údajů a dodržování předpisů. Hlavní bodu zájmu pochází z hello jedinečnosti hello IoT infrastrukturu, která sloučí hello internetový a fyzické světů společně vícesložkovém jednotlivých rizik vyplývajících z těchto dvou světů. Zabezpečení IoT vztahují tooensuring hello integrity kódu běžící na zařízení, poskytuje ověřování zařízení a uživatelů, definování zrušte vlastnictví zařízení (stejně jako data generována tato zařízení) a použití odolné toocyber a fyzickým útokům. 

Pak je problém hello ochrany osobních údajů. Společnosti mají být průhledná týkající se shromažďování dat, jako v co jsou shromažďována a proč, kteří mohou vidět ho, který určuje přístup a tak dále. Nakonec existují obecné bezpečnostní problémy hello zařízení společně s hello osoby je operační a problematiky uchovávání oborových standardů dodržování předpisů.

Zadané hello zabezpečení, ochrany osobních údajů, průhlednost a aspekty dodržování předpisů, výběr poskytovatele řešení IoT správné hello zůstane výzvu. Ve hřbetu společně jednotlivé IoT softwaru a služeb od různých dodavatelů zavádí mezery v zabezpečení, ochrany osobních údajů, průhlednost a dodržování předpisů, které může být pevný toodetect, let alone opravit. Výběr Hello hello vpravo IoT software a služby zprostředkovatele je založena na hledání poskytovatelů, které mají rozsáhlých zkušeností s službami, které jsou rozmístěny napříč pocházejícími a zeměpisných oblastí, ale jsou taky možné tooscale zabezpečené a transparentním způsobem. Podobně, pomáhá pro hello vybraný zprostředkovatel toohave desetiletí prostředí s vývojem zabezpečené software spuštěný na až miliardy počítačů po celém světě, a mít hello možnost tooappreciate hello threat šířku chráněná tento nový world hello internetové služby Věcí.

## <a name="secure-infrastructure-from-hello-ground-up"></a>Zabezpečení infrastruktury z hello pozadí
Hello [Microsoft Cloud](https://www.microsoft.com/enterprise/microsoftcloud/default.aspx#fbid=WzBsRQi6aGk) infrastruktura podporuje více než jednu miliardu zákazníků v 127 zemích. Kreslení na našich zkušeností desetiletí dlouho vytváření podnikového softwaru a spuštění některých z hello největší online služeb v hello, world, poskytujeme vyšší úrovně lepší zabezpečení, ochrany osobních údajů, dodržování předpisů a hrozeb zmírnění postupy než většina zákazníků může dosáhnout na své vlastní.

Naše [životního cyklu SDL (Security Development)](https://www.microsoft.com/sdl/) poskytuje povinné vývoj společnosti proces, který se vloží do hello softwaru celý životní cyklus požadavky na zabezpečení. toohelp Ujistěte se, že provozní aktivity podle hello stejnou úroveň postupy zabezpečení, používáme pokyny pro zabezpečení přísných nastíněny v našem procesu provozní zajištění zabezpečení (OSA). Můžeme také pracovat se podniky auditu třetích stran pro probíhající ověření, že jsme splnit Naše závazky dodržování předpisů a jsme vykonávat úsilí široký zabezpečení pomocí vytvoření hello vynikajících, včetně hello Microsoft jejichž šíření tým Dcu, Microsoft Security Center Response Center a Microsoft Malware Protection Center.

## <a name="microsoft-azure---secure-iot-infrastructure-for-your-business"></a>Microsoft Azure - zabezpečené IoT infrastrukturu pro vaši organizaci
Microsoft Azure nabízí řešení kompletní cloud, který kombinuje neustále rostoucí kolekce integrovaných cloudové služby – analytics, machine learning, úložiště, zabezpečení, sítě a webové – chráněného toohello závazků v špičkový a ochrana osobních údajů vaše data. Naše [předpokládá porušení](https://azure.microsoft.com/blog/red-teaming-using-cutting-edge-threat-simulation-to-harden-the-microsoft-enterprise-cloud/) strategie používá vyhrazený "red tým" odborníků zabezpečení softwaru, kteří simulovat útoky, testování hello schopnost Azure toodetect, ochranu před vznikajícími hrozbami a obnovit z narušení. Naše [globální reakcí na incidenty](https://www.microsoft.com/TrustCenter/Security/DesignOpSecurity) týmu funguje kolem hello taktovací toomitigate hello důsledky útoky a škodlivé aktivity. tým Hello následuje stanovené postupy pro správu incidentů, komunikaci a obnovení a používá rozhraní zjistitelný a předvídatelný s interní a externí partnery.

Naše systémy zadejte zjišťování neoprávněných vniknutí průběžné a prevence, zabránění útoku služby, regulární průnikům testování a forenzní nástroje, které pomáhají identifikovat a zmírnit hrozby. [Služba Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication.md) poskytuje další vrstvu zabezpečení pro koncové uživatele tooaccess hello sítě. A pro aplikace hello a hello hostitele zprostředkovatele, nabízíme řízení přístupu, monitorování, proti malwaru, zjišťování ohrožení zabezpečení, opravy a konfigurace správy.

Hello Microsoft Azure IoT Suite využívá hello zabezpečení a ochrany osobních údajů součástí hello platformy Azure společně s naše SDL a OSA procesy pro zabezpečený vývoj a provoz veškerého softwaru společnosti Microsoft. Tyto postupy poskytovat infrastrukturu ochrany, NAP a identita a Správa funkce zabezpečení základní toohello řešení. 

Hello [Azure IoT Hub](../iot-hub/iot-hub-what-is-iot-hub.md) v rámci hello [IoT Suite](iot-suite-what-is-azure-iot.md) nabízí plně spravovanou službu, která umožňuje spolehlivou a zabezpečenou obousměrnou komunikaci mezi zařízeními IoT a službám Azure, jako je například [Azure Machine Learning](../machine-learning/machine-learning-what-is-machine-learning.md) a [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) pomocí přihlašovacích údajů na zařízení zabezpečení a řízení přístupu.

toobest komunikaci zabezpečení a ochrana osobních údajů funkce, které jsou součástí hello Azure IoT Suite, jsme jsme rozdělit hello suite do tří oblastí primární zabezpečení hello. 

![Azure IoT Suite](media/securing-iot-ground-up/securing-iot-ground-up-fig3.png)

### <a name="secure-device-provisioning-and-authentication"></a>Zabezpečené zřizování zařízení a ověřování
Hello Azure IoT Suite zabezpečuje zařízení v době, kdy jsou se v poli hello tím, že poskytuje klíč jedinečnou identitu pro každé zařízení, která mohou využívat hello IoT infrastruktury toocommunicate s hello zařízení zůstane v operaci. proces Hello je rychlý a snadný tooset nahoru. Hello generovaný klíč na vybrané uživatelem zařízení ID forms hello bázi token používá v veškerá komunikace mezi hello zařízení a hello Azure IoT Hub.

ID zařízení může být přidružen zařízení během výrobní (tj. nainstalovaný v modulu hardwarového vztahu důvěryhodnosti), nebo mohou využít stávající pevné identity jako proxy server (například procesoru sériová čísla). Vzhledem k tomu, že změna tento identifikační údaje v zařízení hello není jednoduchý, je důležité toointroduce ID logické zařízení v případě, že změny hardwaru zařízení základní hello ale hello logického zařízení zůstanou hello stejné. V některých případech může dojít, hello přidružení identitu zařízení v době nasazení zařízení (tj. technika ověřené pole fyzicky nakonfiguruje nové zařízení při komunikaci se serverem back-end řešení hello). Hello [registru identit Azure IoT Hub](../iot-hub/iot-hub-devguide.md) poskytuje zabezpečené úložiště identit zařízení a zabezpečení klíčů pro řešení. Jednotlivé nebo skupiny identit zařízení mohou být přidány tooan povolit seznam nebo seznamy bloku, povolení plnou kontrolu nad přístup k zařízení.

Zásady řízení přístupu Azure IoT Hub v cloudu hello povolit aktivace a zakázání všechny identity zařízení, poskytuje způsob toodisassociate zařízení z nasazení služby IoT případě potřeby. Toto přidružení a zrušení přidružení zařízení je založena na každou identitu zařízení.

Funkce zabezpečení další zařízení hello následující:

* Zařízení nepřijímají nevyžádané síťová připojení. Vytvoří všechna připojení a trasy způsobem pouze odchozí. Pro zařízení tooreceive příkaz z back-end hello musí zařízení hello inicializovat připojení toocheck pro všechny čekající příkazy tooprocess. Jakmile je bezpečně navázat připojení mezi hello zařízení a služby IoT Hub, zasílání zpráv ze hello cloud toohello zařízení a zařízení toohello cloudu nelze odesílat transparentně.  
* Zařízení se připojují jenom tooor vytvořit trasy toowell známé služby, se kterými se kterými mají partnerský, jako je Azure IoT Hub.
* Úrovni systému autorizaci a ověřování používat identity podle zařízení, což přístupové údaje a povolení téměř-okamžitě odvolatelné.

### <a name="secure-connectivity"></a>Zabezpečené připojení
Odolnost zasílání zpráv je důležitou součást řešení IoT. Hello potřebovat toodurably příkazy poskytovat nebo přijímat data ze zařízení, je podtržený hello fakt, že zařízení IoT připojeni přes hello Internet nebo jiné podobné sítě, které může nespolehlivé. Azure IoT Hub nabízí odolnost zasílání zpráv mezi cloudu a zařízením prostřednictvím systému potvrzování v toomessages odpovědi. Ukládání do mezipaměti zprávy v hello IoT Hub pro tooseven dní telemetrie a dva dny pro příkazy se dosahuje další odolnost pro zasílání zpráv.

Efektivita je důležité tooensure uchování zdrojů a operaci v prostředí s omezenými zdroji. HTTPS (HTTP zabezpečení), hello standardní zabezpečené verzi protokolu http oblíbených hello, podporuje Azure IoT Hub, povolení efektivní komunikace. Pokročilé služby Řízení front zpráv (protokolu AMQP) a zpráv služby Řízení front Telemetrie přenosu (MQTT), podporované službou Azure IoT Hub, jsou navrženy nejen pro efektivitu z hlediska využití prostředků, ale také doručování zpráv spolehlivé. 

Škálovatelnost vyžaduje hello možnost toosecurely spolupracovat s širokou škálu zařízení. Azure IoT hub umožňuje zařízení s povolenou adresou IP a nepodporujícím IP tooboth zabezpečené připojení. Zařízení s podporou IP jsou možné toodirectly připojit a komunikovat se službou IoT Hub hello prostřednictvím zabezpečeného připojení. Zapnutá non-IP adresa zařízení jsou omezené prostředků a připojit pouze přes krátkou vzdálenost komunikační protokoly, jako je například Zwave, ZigBee a Bluetooth. Brána pole je použité tooaggregate těchto zařízení a provádí protokol překladu tooenable zabezpečené obousměrnou komunikaci s cloudem hello.

Funkce zabezpečení další připojení hello následující:

* Hello komunikační trasa mezi zařízením a Azure IoT Hub, nebo mezi brány a Azure IoT Hub, je zabezpečena pomocí Azure IoT Hub ověřování pomocí protokolu X.509 standardní zabezpečení TLS (Transport Layer).
* Azure IoT Hub v zařízení tooprotect pořadí z nevyžádaný příchozí připojení, neotevře jakékoli zařízení toohello připojení. zařízení Hello zahájí všechna připojení. 
* Azure IoT Hub spolehlivě uloží zprávy pro zařízení a čeká tooconnect hello zařízení. Tyto příkazy se uchovávají po dobu dvou dní, povolení zařízení připojující se k, z důvodu bezpečnosti toopower nebo připojení tooreceive těchto příkazů. Azure IoT Hub uchovává fronty za zařízení pro každé zařízení.

### <a name="secure-processing-and-storage-in-hello-cloud"></a>Zabezpečené zpracování a úložiště v cloudu hello
Z dat tooprocessing šifrování komunikace v cloudu hello hello Azure IoT Suite pomáhá zabezpečit data. Poskytuje flexibilitu tooimplement další šifrování a správy klíčů zabezpečení. Pomocí Azure Active Directory (AAD) pro ověřování uživatelů a autorizaci, Azure IoT Suite může poskytnout model na základě zásad autorizace pro data v cloudu hello povolení správy snadný přístup, které je možné auditovat a zkontrolovat. Tento model taky umožňuje téměř rychlých odvolání přístupu toodata v cloudu hello a toohello připojené zařízení Azure IoT Suite.

Jakmile jsou data v cloudu hello, můžete zpracovat a uložené v pracovním postupu žádné uživatelem definované. Přístup tooeach část dat, hello je řízena pomocí Azure Active Directory, v závislosti na služby úložiště hello používá.

Všechny klíče používané hello IoT infrastruktury jsou uložené v cloudu hello v zabezpečeném úložišti s hello možnost tooroll přes v případě, že klíče potřebovat toobe znovu zřídit. Data se uloží v [Azure Cosmos DB](../documentdb/documentdb-introduction.md) nebo v [databází SQL](../sql-database/sql-database-faq.md), povolení definice hello úroveň zabezpečení potřeby. Kromě toho Azure poskytuje způsob toomonitor a auditování všech přístup tooyour data tooalert jste narušení nebo neoprávněného přístupu.

## <a name="conclusion"></a>Závěr
Hello Internet věcí začíná vaší věcí – hello věci většina toobusinesses. IoT může úžasné obchodní tooa hodnota poskytovaným snižuje náklady, zvýšit výnosy a transformace firmy. Úspěch této transformace do značné míry závisí na výběr hello správné IoT software a služby zprostředkovatele. To znamená, hledání zprostředkovatele, který pouze catalyzes Tato transformace požadavky a pochopení obchodních potřeb, ale také poskytuje služeb a softwaru, které jsou vytvořené s nástroji zabezpečení, ochrany osobních údajů, průhlednost a dodržování předpisů jako hlavní faktorů. Microsoft má rozsáhlých zkušeností s vývoj a nasazení zabezpečené softwaru a služeb a pokračuje toobe vedoucí postavení v této nové stáří Internet věcí. 

Hello Microsoft Azure IoT Suite sestavení v bezpečnostní opatření návrh povolení zabezpečeného monitorování prostředky tooimprove efektivitu, implementovat provozní výkonu tooenable inovace a společnosti využívají pokročilou analýzu dat tootransform firmy. S jeho vrstveného přístupu k zabezpečení, více funkce zabezpečení a vzory návrhu Azure IoT Suite pomáhá nasazení infrastruktury, která může být důvěryhodný tootransform žádné firmy. 

## <a name="additional-information"></a>Další informace
Každé předkonfigurované řešení Azure IoT Suite vytváří instance služby Azure, jako je například hello následující:

* [**Azure IoT Hub**](https://azure.microsoft.com/services/iot-hub/): bránu, která se připojuje hello cloudu příliš "věcí". Je možné škálovat toomillions připojení na rozbočovače a procesu ohromné objemy dat s podporou ověřování podle zařízení pomáhá vám zabezpečit vaše řešení.
* [**Azure Cosmos DB**](https://azure.microsoft.com/services/documentdb/): škálovatelné a plně indexované databázová služba pro částečně strukturovaná data, která spravuje metadata pro zařízení hello zřídíte, jako je například atributy, konfiguraci a vlastnosti zabezpečení. Cosmos DB nabízí vysoce výkonné a vysokou propustností zpracování, bez ohledu na schéma indexování dat a bohaté rozhraní SQL.
* [**Azure Stream Analytics**](https://azure.microsoft.com/services/stream-analytics/): streamu v reálném čase zpracování hello cloudu, který umožňuje vám toorapidly vývoji a nasazení nízkými náklady analytics řešení toouncover přehledy v reálném čase ze zařízení, senzorů, infrastruktury, a aplikace. Hello data z tato plně spravovaná služba můžete škálovat tooany svazku, zatímco stále dosahuje vysoké propustnosti, s nízkou latencí a odolnost proti chybám.
* [**Azure App Services**](https://azure.microsoft.com/services/app-service/): cloudové platformy toobuild výkonné webové a mobilní aplikace, které se připojují toodata kdekoli; v cloudu hello nebo místně. Vytvářejte poutavé mobilní aplikace pro iOS, Android a Windows. Integrate váš Software jako služba (SaaS) a podnikové aplikace s toodozens připojení se na pole cloudových služeb a podnikové aplikace. Kód v váš oblíbený jazyk a IDE – rozhraní .NET, Node.js, PHP, Python nebo Java – toobuild webové aplikace a rozhraní API rychleji než kdy dřív.
* [**Služba Logic Apps**](https://azure.microsoft.com/services/app-service/logic/): hello funkce Logic Apps služby Azure App Service pomáhá integrovat IoT řešení tooyour existující-obchodní systémy a automatizovat pracovní postupy. Služba Logic Apps umožňuje vývojářům toodesign pracovních, které začínají spouštěčem událostí a provádějí sérii kroků – pravidla a akce, které pomocí výkonné konektory toointegrate své obchodní procesy. Služba Logic Apps nabízí připojení se na pole tooa velká ekosystém SaaS, cloudových a místních aplikací.
* [**Úložiště objektů blob Azure**](https://azure.microsoft.com/services/storage/): spolehlivé a ekonomické cloudové úložiště pro data hello, že vaše zařízení odesílají toohello cloudu.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o zabezpečení řešení IoT najdete v části:

* [Osvědčené postupy zabezpečení IoT][lnk-security-best-practices]
* [Architektura IoT zabezpečení][lnk-security-architecture]
* [Zabezpečit vaše nasazení IoT][lnk-security-deployment]

[lnk-security-best-practices]: iot-security-best-practices.md
[lnk-security-architecture]: iot-security-architecture.md
[lnk-security-deployment]: iot-suite-security-deployment.md

Můžete také prozkoumat některé hello další funkce a možnosti hello předkonfigurovaná řešení IoT Suite:

* [Přehled řešení předkonfigurované prediktivní údržby][lnk-predictive-overview]
* [Nejčastější dotazy k sadě IoT Suite][lnk-faq]

Další informace o zabezpečení služby IoT Hub v [řízení přístupu tooIoT rozbočovače] [ lnk-devguide-security] v hello Příručka vývojáře pro službu IoT Hub.

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-devguide-security]: ../iot-hub/iot-hub-devguide-security.md
