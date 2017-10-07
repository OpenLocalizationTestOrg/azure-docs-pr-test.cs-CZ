---
title: "aaaLog Analytics – nejčastější dotazy | Microsoft Docs"
description: "Odpovědi toofrequently kladené dotazy týkající se služby Azure Log Analytics hello."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: ad536ff7-2c60-4850-a46d-230bc9e1ab45
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: magoedte
ms.openlocfilehash: 25931f521cbb6ec840184221c6c1a5794b3445f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-faq"></a>Nejčastější dotazy k Log Analytics
Tato FAQ Microsoft je seznam často kladené otázky týkající se analýzy protokolů v Operations Management Suite (OMS). Pokud máte další dotazy o analýzy protokolů, přejděte toohello [diskusní fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) a zveřejněte svoje otázky. Dotaz je dotaz, často, přidáme ji toothis článek tak, aby ho můžete rychle a snadno najít.

## <a name="general"></a>Obecné

### <a name="q-does-log-analytics-use-hello-same-agent-as-azure-security-center"></a>OTÁZKY. Používá analýzy protokolů hello stejného agenta jako Azure Security Center?

A. V časná června 2017 začal Azure Security Center pomocí agenta Microsoft Monitoring Agent hello toocollect a úložiště dat. Další, najdete v části toolearn [Azure Security Center platformy migrace – nejčastější dotazy](../security-center/security-center-platform-migration-faq.md).

### <a name="q-what-checks-are-performed-by-hello-ad-and-sql-assessment-solutions"></a>OTÁZKY. Jaké kontroly provádí hello AD a vyhodnocení SQL řešení?

A. Hello následující dotaz zobrazí popis aktuálně provést všechny kontroly:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

Hello výsledků pak lze exportovaný tooExcel hodnocení.

### <a name="q-why-do-i-see-something-different-than-oms-in-system-center-operations-manager-console"></a>Otázka: Proč něco jiného než najdete *OMS* v konzole nástroje System Center Operations Manager?

Odpověď: v závislosti na jaké aktualizace souhrnu nástroje Operations Manager na, mohou se zobrazit uzel pro *System Center Advisor*, *Statistika provozu*, nebo *analýzy protokolů*.

aktualizace řetězec textu Hello příliš*OMS* je součástí sady management pack, který se musí toobe importovat ručně. aktuální textu hello toosee a funkce, postupujte podle pokynů hello na hello nejnovější System Center Operations Manager aktualizace kumulativní KB článek a aktualizace hello konzoly.

### <a name="q-is-there-an-on-premises-version-of-log-analytics"></a>Otázka: je k dispozici *místní* verzi Log Analytics?

Odpověď: Ne. Analýzy protokolů procesy a ukládá velké objemy dat. Jako cloudová služba analýzy protokolů je možné tooscale až v případě potřeby a vyhnout se jakékoli výkonu dopad tooyour prostředí.

Další výhody patří:
- Microsoft spouští hello analýzy protokolů infrastrukturu, ušetřit tak náklady
- Regulární nasazení funkce aktualizace a opravy.

### <a name="q-how-do-i-troubleshoot-that-log-analytics-is-no-longer-collecting-data"></a>OTÁZKY. Jak odstranit analýzy protokolů je už shromažďování dat?

Odpověď: Pokud jsou na hello volné cenová úroveň a odeslali více než 500 MB dat za den, zastaví se shromažďování dat pro hello zbytek hello den. Rozsáhlejší hello denní limit je běžným důvodem, která ukončí analýzy protokolů shromažďování dat, nebo se data zobrazí toobe chybí.

Analýzy protokolů vytváří událost typu *operace* při shromažďování dat, spuštění a zastavení. 

Spusťte následující dotaz ve vyhledávání toocheck, pokud jsou dosažení denního limitu hello a chybějící data hello:`Type=Operation OperationCategory="Data Collection Status"`

Při shromažďování dat zastaví, hello *OperationStatus* je **upozornění**. Při shromažďování dat spuštění, hello *OperationStatus* je **úspěšné**. 

Hello následující tabulka popisuje důvody, které shromažďování dat zastaví a kolekce dat tooresume navrhovaná akce:

| Důvod shromažďování dat zastaví                       | shromažďování dat tooresume |
| -------------------------------------------------- | ----------------  |
| Dosažení denního limitu volné dat<sup>1</sup>       | Počkat, až hello následující den pro kolekci tooautomatically restartování, nebo<br> Změna tooa placené cenová úroveň. |
| Předplatné Azure je v pozastaveném stavu z důvodu: <br> Bezplatná zkušební verze skončila <br> Platnost Azure průchodu <br> Měsíčně útrat dosažen (například na předplatné MSDN nebo v sadě Visual Studio)                          | Převést tooa placené předplatné <br> Převést tooa placené předplatné <br> Odeberte limit nebo počkejte, až se obnoví limit |

<sup>1</sup> Pokud pracovního prostoru na hello volné cenovou úroveň, jste omezené toosending 500 MB dat za den toohello služby. Po dosažení denního limitu hello shromažďování dat zastaví dokud hello další den. Data odeslaná při ukončení shromažďování dat je není indexované a není k dispozici pro vyhledávání. Když se obnoví shromažďování dat, dojde k zpracování pouze pro nová data odeslána. 

Čas UTC využívá analýzy protokolů a spustí každý den o půlnoci času UTC. Pokud hello prostoru dosáhne hello denního limitu, obnoví zpracování během hello první hodinu hello další den UTC.

### <a name="q-how-can-i-be-notified-when-data-collection-stops"></a>OTÁZKY. Jak můžete se upozornění, když se shromažďování dat zastaví?

Odpověď: použijte hello kroků popsaných v [vytvořit pravidlo výstrahy](log-analytics-alerts-creating.md#create-an-alert-rule) toobe upozornění při shromažďování dat zastaví.

Při vytváření hello upozornění pro při shromažďování dat zastaví, nastavte:
- **Název** příliš*zastavit shromažďování dat*
- **Závažnost** příliš*upozornění*
- **Vyhledávací dotaz** příliš`Type=Operation OperationCategory="Data Collection Status" OperationStatus=Warning`
- **Časový interval** příliš*2 hodiny*.
- **Výstrahy frekvence** toobe jedné hodiny vzhledem k tomu, že data o využití hello aktualizuje jenom jednou za hodinu.
- **Generují výstrahu založenou na** toobe *počet výsledků*
- **Počet výsledků** toobe *větší než 0.*

Použít hello kroků popsaných v [přidat pravidla tooalert akce](log-analytics-alerts-actions.md) nakonfigurovat e-mailu, webhooku nebo sadu runbook akce pro pravidlo výstrahy hello.


## <a name="configuration"></a>Konfigurace
### <a name="q-can-i-change-hello-name-of-hello-tableblob-container-used-tooread-from-azure-diagnostics-wad"></a>OTÁZKY. Můžete změnit název hello hello tabulky nebo objekt blob kontejneru použít tooread z Azure Diagnostics (WAD)?

A. Ne, není aktuálně možné tooread z libovolné tabulky nebo kontejnery v úložišti Azure.

### <a name="q-what-ip-addresses-does-hello-log-analytics-service-use-how-do-i-ensure-that-my-firewall-only-allows-traffic-toohello-log-analytics-service"></a>OTÁZKY. IP adresy hello analýzy protokolů služby použít? Jak zjistím, že branou firewall umožňuje pouze provoz toohello analýzy protokolů služby?

A. Hello analýzy protokolů služby je postavená na Azure. Log Analytics IP adresy se v hello [Microsoft Azure Datacenter rozsahy IP adres](http://www.microsoft.com/download/details.aspx?id=41653).

Podle nasazení služby jsou vytvářeny, změně hello skutečné IP adres hello analýzy protokolů služby. tooallow názvy DNS Hello prostřednictvím brány firewall jsou popsána v [nakonfigurujte nastavení serveru proxy a brány firewall v analýzy protokolů](log-analytics-proxy-firewall.md).

### <a name="q-i-use-expressroute-for-connecting-tooazure-does-my-log-analytics-traffic-use-my-expressroute-connection"></a>OTÁZKY. Je možné použít ExpressRoute pro připojení tooAzure. Používá Moje analýzy protokolů přenosů Moje připojení ExpressRoute?

A. Hello různé typy provozu ExpressRoute jsou popsané v hello [dokumentace ExpressRoute](../expressroute/expressroute-faqs.md#supported-services).

Provoz tooLog Analytics používá hello partnerský vztah veřejné okruh ExpressRoute.

### <a name="q-is-there-a-simple-and-easy-way-toomove-an-existing-log-analytics-workspace-tooanother-log-analytics-workspaceazure-subscription"></a>OTÁZKY. Je k dispozici jednoduchý a snadný způsob toomove existující pracovní prostor analýzy protokolů tooanother odběru pracovního prostoru nebo Azure Log Analytics?

A. Hello `Move-AzureRmResource` rutiny můžete přesouvat z jednoho předplatného Azure tooanother pracovní prostor analýzy protokolů a účet Automation. Další informace najdete v tématu [přesunutí AzureRmResource](http://msdn.microsoft.com/library/mt652516.aspx).

Tato změna lze provést také v hello portálu Azure.

Nelze přesunout data z jednoho tooanother pracovní prostor analýzy protokolů ani změnit hello oblast, která data Log Analytics je uložena v.

### <a name="q-how-do-i-add-log-analytics-toosystem-center-operations-manager"></a>Otázka: Jak přidám Log Analytics tooSystem Center Operations Manager?

Odpověď: aktualizace nejnovější kumulativní aktualizaci toohello a import sad management Pack umožňuje tooconnect nástroje Operations Manager tooLog Analytics.

>[!NOTE]
>tooLog připojení nástroje Operations Manager Hello Analytics je dostupná jenom pro System Center Operations Manager 2012 SP1 a novější.

### <a name="q-how-can-i-confirm-that-an-agent-is-able-toocommunicate-with-log-analytics"></a>Otázka: jak můžete potvrdit, že agent je možné toocommunicate s analýzy protokolů

Odpověď: tooensure tohoto agenta hello může komunikovat s OMS, přejděte na: Ovládací panely, zabezpečení a nastavení, **agenta Microsoft Monitoring Agent**.

V části hello **Azure Log Analytics (OMS)** kartě, vyhledejte zelená značka zaškrtnutí. Ikona zelená značka zaškrtnutí potvrzuje, že tento agent hello je možné toocommunicate s hello služby OMS.

Žlutá ikona s upozorněním znamená, že hello agent má problémy komunikace s OMS. Jeden obvyklým důvodem je, že hello službu Microsoft Monitoring Agent byla zastavena. Pomocí řízení manager toorestart hello služby.

### <a name="q-how-do-i-stop-an-agent-from-communicating-with-log-analytics"></a>Otázka: Jak zabráním agenta v komunikaci s Log Analytics?

Odpověď: V nástroji System Center Operations Manager, odeberte hello počítač ze seznamu spravovaného počítače Advisor hello. Aktualizace nástroje Operations Manager hello konfigurace hello agenta toono delší sestavy tooLog Analytics. Pro agenty tooLog Analytics přímo připojené, je nezastavíte komunikaci prostřednictvím: Ovládací panely, zabezpečení a nastavení, **agenta Microsoft Monitoring Agent**.
V části **Azure Log Analytics (OMS)**, odeberte všechny pracovní prostory uvedené.

### <a name="q-why-am-i-getting-an-error-when-i-try-toomove-my-workspace-from-one-azure-subscription-tooanother"></a>Otázka: Proč dochází k chybě při toomove pracovního prostoru z tooanother jedno předplatné?

Odpověď: Pokud používáte hello portálu Azure, ověřte, že je vybrána pouze hello prostoru pro přesunutí hello. Nevybírejte hello řešení – se automaticky přesune po přesune hello prostoru. 

Zkontrolujte, zda že máte oprávnění v obou předplatných Azure.

## <a name="agent-data"></a>Data agenta
### <a name="q-how-much-data-can-i-send-through-hello-agent-toolog-analytics-is-there-a-maximum-amount-of-data-per-customer"></a>OTÁZKY. Jaké množství dat může odesílat prostřednictvím hello agenta tooLog Analytics? Existuje maximální množství dat za zákazníka?
A. plánu úrovně free Hello Nastaví denní cap 500 MB za pracovního prostoru. Hello standard a premium plány mít žádné omezení na hello množství dat, který je nahraný. Jako cloudová služba je navržený tak, analýzy protokolů tooautomatically rozšiřování škálování využívajících toohandle hello svazku pocházejících z zákazník – i když je terabajtů za den.

agent analýzy protokolů Hello se navrženou tooensure obsahuje malé nároky. Jeden z našich zákazníků napsali blogu o hello testy, které budou provedeny s naše agenta, a jak dojem byli. Hello datový svazek se liší podle hello řešení, které povolíte. Můžete najít podrobné informace o hello datový svazek a najdete v části hello rozpadu řešením v hello [využití](log-analytics-usage.md) stránky.

Další informace si můžete přečíst [zákazníka blog](http://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) o hello nízkým nárokům agenta OMS hello.

### <a name="q-how-much-network-bandwidth-is-used-by-hello-microsoft-management-agent-mma-when-sending-data-toolog-analytics"></a>OTÁZKY. Jak velkou šířku pásma sítě je používán hello správy agenta MMA (Microsoft) při odesílání dat tooLog Analytics?

A. Šířka pásma je funkce na hello množství dat odesílaných. Data se komprimují při přenosu přes síť hello.

### <a name="q-how-much-data-is-sent-per-agent"></a>OTÁZKY. Množství dat, které jsou odeslány na jednoho agenta?

A. Hello množství dat odesílaných na jednoho agenta, závisí na:

* Hello řešení, které jste povolili
* Hello počet shromažďovaných čítače výkonu a protokolování
* Hello objem dat v protokolech hello

Hello volné cenová úroveň je dobře tooonboard několik serverů a vyhodnocovat hello typické datový svazek. Celkové využití se zobrazí na hello [využití](log-analytics-usage.md) stránky.

Pro počítače, které je možné toorun hello WireData agentem použijte následující dotaz toosee kolik data je odesílána hello:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```



## <a name="next-steps"></a>Další kroky
* [Začínáme s analýzy protokolů](log-analytics-get-started.md) toolearn více informací o analýzy protokolů a a její spuštění v minutách.
