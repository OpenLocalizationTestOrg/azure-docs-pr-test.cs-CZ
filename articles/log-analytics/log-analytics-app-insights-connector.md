---
title: "aaaView data aplikací Azure Application Insights | Microsoft Docs"
description: "Můžete použít problémy s výkonem toodiagnose hello konektor služby Statistika aplikace řešení a pochopit, co uživatelé dělají s vaší aplikací, pokud monitorované pomocí Application Insights."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 49280cad-3526-43e1-a365-c6a3bf66db52
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: banders
ms.openlocfilehash: 38109337ebbc8970dccb65365ba8284d9cee19a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-connector-solution-preview-in-operations-management-suite-oms"></a>Řešení konektor služby Statistika aplikace (Preview) v Operations Management Suite (OMS)

![Application Insights symbol](./media/log-analytics-app-insights-connector/app-insights-connector-symbol.png)

Hello řešení konektor služby Statistika aplikací umožňuje diagnostikovat problémy s výkonem a pochopit, co uživatelé dělají s vaší aplikací když je monitorovaný s [Application Insights](../application-insights/app-insights-overview.md). Zobrazení hello stejná telemetrická data aplikací, které vývojáři zobrazit ve službě Application Insights jsou k dispozici v OMS. Při integraci aplikace Application Insights s OMS, zda se aplikace zvýšit tak, že data operace a aplikace na jednom místě. S hello stejné zobrazení vám pomůže toocollaborate s vývojáři vaší aplikace. Obecná zobrazení Hello může pomoci snížit čas toodetect hello a řešení aplikace a problém s platformou.

Pokud používáte hello řešení, můžete:

- Zobrazit všechny aplikace služby Application Insights na jednom místě, i když se nachází v různých předplatných Azure
- Korelaci dat infrastruktury se data aplikací
- Vizualizovat data aplikací s perspektivami v hledání protokolů
- Otáčení z Azure portály a analýzy protokolů data tooyour Application Insights aplikace v hello OMS

## <a name="connected-sources"></a>Připojené zdroje

Na rozdíl od většiny jiných řešení analýzy protokolů není data shromážděná pro hello konektor služby Statistika aplikace pomocí agentů. Všechna data, která používá řešení hello pochází přímo z Azure.

| Připojený zdroj | Podporuje se | Popis |
| --- | --- | --- |
| [Agenti systému Windows](log-analytics-windows-agents.md) | Ne | řešení Hello neshromažďuje informace z agentů v systému Windows. |
| [Agenti systému Linux](log-analytics-linux-agents.md) | Ne | řešení Hello neshromažďuje informace od agentů systému Linux. |
| [Skupiny správy nástroje SCOM](log-analytics-om-agents.md) | Ne | řešení Hello neshromažďuje informace od agentů v připojené skupině pro správu SCOM. |
| [Účet služby Azure Storage](log-analytics-azure-storage.md) | Ne | řešení Hello nemá shromažďování informací z úložiště Azure. |

## <a name="prerequisites"></a>Požadavky

- tooaccess informace o konektoru Application Insights, musíte mít předplatné Azure
- Musí mít alespoň jeden nakonfigurované prostředek Application Insights.
- Musí být hello vlastníka nebo přispěvatele daného hello prostředek Application Insights.

## <a name="configuration"></a>Konfigurace

1. Povolit řešení Azure Web Apps Analytics hello z hello [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ApplicationInsights?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).
2. Na portálu OMS hello, klikněte na tlačítko **nastavení** &gt; **Data** &gt; **Application Insights**.
3. V části **vybrat odběr, který**, vyberte odběr, který má prostředky Application Insights a pak v části **název aplikace**, vyberte jednu nebo více aplikací.
4. Klikněte na **Uložit**.

Přibližně 30 minut bude k dispozici data a hello dlaždici Application Insights se aktualizuje s daty, jako je hello následující obrázek:

![Dlaždice Application Insights](./media/log-analytics-app-insights-connector/app-insights-tile.png)

Ostatní body tookeep nezapomeňte:

- Můžete propojit jenom Application Insights pracovní prostor OMS tooone aplikace.
- Můžete propojit pouze [Standard nebo Premium Application Insights prostředky](https://azure.microsoft.com/pricing/details/application-insights) tooOMS analýzy protokolů. Můžete však použít úroveň Free hello Log Analytics.

## <a name="management-packs"></a>Sady Management Pack

Toto řešení nenainstaluje žádné sady management Pack v připojených skupin pro správu.

## <a name="use-hello-solution"></a>Použít hello řešení

Hello následující části popisují, jak můžete použít okna hello ukazuje hello Application Insights řídicí panel tooview a interagovat s daty z vašich aplikací.

### <a name="view-application-insights-connector-information"></a>Zobrazení informací o konektor služby Statistika aplikace

Klikněte na tlačítko hello **Application Insights** dlaždice tooopen hello **Application Insights** hello toosee řídicí panel následující okna.

![Přehledný řídicí panel aplikací](./media/log-analytics-app-insights-connector/app-insights-dash01.png)

![Přehledný řídicí panel aplikací](./media/log-analytics-app-insights-connector/app-insights-dash02.png)

řídicí panel Hello zahrnuje hello okna uvedené v tabulce hello. Každý okno uvádí až too10 položky odpovídající, aby na okno kritéria pro hello zadán oboru a časový rozsah. Můžete spustit hledání protokolů, která vrací všechny záznamy, po kliknutí na tlačítko **zobrazit všechny** dole hello v okně hello nebo když kliknete na záhlaví okna hello.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

| **Sloupec** | **Popis** |
| --- | --- |
| Aplikace – počet aplikací | Zobrazuje hello počet aplikací v prostředky aplikace. Také aplikace zobrazí názvy a pro každou, hello počet záznamů aplikace. Klikněte na číslo toorun hello protokolu vyhledejte<code>Type=ApplicationInsights &#124; measure sum(SampledCount) by ApplicationName</code> <br><br>  Klikněte na název toorun aplikaci hledání protokolů hello aplikaci, která zobrazuje aplikace záznamů na hostitele, záznamy podle typu telemetrie a všechna data podle typu (na základě hello poslední den). |
| Datový svazek – hostitelů odeslání dat | Zobrazuje počet hello počítač hostitele, kteří odesílají data. Také uvádí hostitele počítače a počet záznamů pro každého hostitele. Klikněte na číslo toorun hello protokolu vyhledejte<code>Type=ApplicationInsights &#124; measure sum(SampledCount) by Host</code> <br><br> Klikněte na název toorun na počítači hledání protokolů pro hello hostitele, který ukazuje záznamů aplikací na hostitele, záznamy podle typu telemetrie a všechna data podle typu (na základě hello poslední den). |
| Dostupnost – výsledky testu webu | Zobrazí prstencový graf pro web výsledky testů, označující průchodu nebo selhání. Klikněte na graf toorun hello protokolu vyhledejte<code>Type=ApplicationInsights TelemetryType=Availability &#124; measure sum(SampledCount) by AvailabilityResult</code> <br><br> Zobrazí počet hello předává a chyby pro všechny testy. Zobrazuje všechny webové aplikace s přenosy dat pro hello poslední minutu. Klikněte na název tooview aplikaci vyhledávání protokolu s podrobnostmi o testy webu se nezdařilo. |
| Požadavky serveru – počet požadavků za hodinu | Zobrazuje graf řádku hello požadavky serveru za hodinu pro různé aplikace. Najeďte myší na řádek v hello grafu toosee hello 3 hlavních aplikace přijímá požadavky na bod v čase. Také ukazuje seznam aplikací hello přijímání požadavků a hello počet požadavků, které hello vybrané období. <br><br>Klikněte na graf toorun hello protokolu vyhledejte <code>Type=ApplicationInsights TelemetryType=Request &#124; measure sum(SampledCount) by ApplicationName interval 1hour</code> který ukazuje na podrobnější spojnicový graf hello požadavky serveru za hodinu pro různé aplikace. <br><br> Klikněte na aplikaci v seznamu toorun hello protokolu vyhledejte <code>Type=ApplicationInsights  ApplicationName=yourapplicationname  TelemetryType=Request</code> , obsahuje seznam požadavků, grafy pro požadavky na čas a žádost o dobu trvání a seznam požadavek kódů odpovědi.   |
| Chyby – neúspěšné požadavky za hodinu | Zobrazí graf řádku žádostí o selhání aplikace za hodinu. Pozastavte ukazatel myši nad hello grafu toosee hello hlavních 3 aplikací s neúspěšných požadavků pro bod v čase. Také ukazuje seznam aplikací s hello počet neúspěšných požadavků pro každou. Klikněte na graf toorun hello protokolu vyhledejte <code>Type=ApplicationInsights TelemetryType=Request  RequestSuccess = false &#124; measure sum(SampledCount) by ApplicationName interval 1hour</code> který ukazuje podrobnější spojnicový graf požadavků na aplikaci, která selhala. <br><br>Klikněte na položku v seznamu toorun hello protokolu vyhledejte <code>Type=ApplicationInsights ApplicationName=yourapplicationname TelemetryType=Request  RequestSuccess=false</code> , ukazuje neúspěšné požadavky, grafy pro neúspěšné požadavky přes čas a žádost o doba trvání a seznam kódů odpovědi chybných požadavků. |
| Výjimky – výjimky za hodinu | Zobrazí graf řádku výjimek za hodinu. Pozastavte ukazatel myši nad hello grafu toosee hello hlavních 3 aplikací s výjimky pro bod v čase. Také ukazuje seznam aplikací s hello počet výjimek pro každou. Klikněte na graf toorun hello protokolu vyhledejte <code>Type=ApplicationInsights TelemetryType=Exception &#124; measure sum(SampledCount) by ApplicationName interval 1hour</code> , zobrazí podrobnější graf odkaz výjimek. <br><br>Klikněte na položku v seznamu toorun hello protokolu vyhledejte <code>Type=ApplicationInsights  ApplicationName=yourapplicationname TelemetryType=Exception</code> , zobrazí se seznam výjimek, grafy pro výjimky přes čas a k selhání požadavků a seznam typů výjimek.  |

### <a name="view-hello-application-insights-perspective-with-log-search"></a>Zobrazení hello Application Insights perspektivy s hledání protokolů

Když kliknete na libovolnou položku v řídicím panelu hello, uvidíte Application Insights perspektivy uvedené v hledání. Hello perspektivy poskytuje rozšířené vizualizace, založené na vybraném typu telemetrie hello. Ano, vizualizace změny obsahu pro jiné telemetrie typy.

Když v okně aplikace hello kliknete na libovolné místo, se zobrazí výchozí hello **aplikace** perspektivy.

![Application Insights aplikace perspektivy](./media/log-analytics-app-insights-connector/applications-blade-drill-search.png)

Hello perspektivy ukazuje přehled hello aplikace, kterou jste vybrali.

Hello **dostupnosti** okno ukazuje různé perspektivy zobrazení, kde můžete zobrazit výsledky testů webových a související neúspěšných požadavků.

![Perspektiva statistice dostupnosti aplikace](./media/log-analytics-app-insights-connector/availability-blade-drill-search.png)

Když kliknete na libovolné místo v hello **požadavků serveru** nebo **selhání** oken, hello perspektivy součásti změnit toogive jste vizualizace, který související toorequests.

![Okno statistiky selhání aplikace](./media/log-analytics-app-insights-connector/server-requests-failures-drill-search.png)

Když kliknete na libovolné místo v hello **výjimky** okně uvidíte vizualizace, přesně podle tooexceptions.

![Okna Statistika výjimky aplikací](./media/log-analytics-app-insights-connector/exceptions-blade-drill-search.png)

Bez ohledu na to, jestli klepnutí na něco hello **konektor služby Statistika aplikace** řídicího panelu, v rámci hello **vyhledávání** samostatně, stránka jakýkoli dotaz vrací Application Insights data zobrazují hello Application Insights perspektivy. Pokud se nacházíte Application Insights data, například **&#42;** dotaz také zobrazí karta perspektivy hello jako hello následující bitové kopie:

![Application Insights ](./media/log-analytics-app-insights-connector/app-insights-search.png)

Perspektivy součásti jsou aktualizovány v závislosti na hello vyhledávací dotaz. To znamená, že můžete filtrovat výsledky hello pomocí každé pole hledání, které poskytuje hello možnost toosee hello data z:

- Všechny aplikace
- Jediné vybrané aplikace
- Skupinu aplikací

### <a name="pivot-tooan-app-in-hello-azure-portal"></a>Otáčení tooan aplikace v hello portálu Azure

Konektor služby Statistika okna aplikace jsou navrženou tooenable toopivot toohello vybrané aplikace služby Application Insights *při použití portálu OMS hello*. Hello řešení můžete použít jako základní monitorování platformu, která vám pomůže vyřešit aplikace. Když se na potenciální problém v některé z vašich připojených aplikací, můžete buď přejít k podrobnostem ho v OMS vyhledávání nebo můžete přímo otáčení toohello Application Insights aplikace.

toopivot, klikněte na symbol tří teček hello (**...** ), zobrazí se na konci hello každého řádku a vyberte **otevřete ve službě Application Insights**.

>[!NOTE]
>**Otevřete ve službě Application Insights** není k dispozici v hello portálu Azure.

![Otevřete ve službě Application Insights](./media/log-analytics-app-insights-connector/open-in-app-insights.png)

### <a name="sample-corrected-data"></a>Opravě ukázková data

Poskytuje služby Application Insights  *[vzorkování oprava](../application-insights/app-insights-sampling.md)*  toohelp omezit přenos telemetrie. Když povolíte vzorkování ve vaší aplikaci Application Insights, zobrazí menší počet položek, které jsou uložené ve službě Application Insights a v OMS. Při konzistenci dat se zachová hello **konektor služby Statistika aplikace** stránky a perspektivy, je nutno ručně opravit jen Vzorkovaná data pro své vlastní dotazy.

Tady je příklad vzorkování oprava do protokolu vyhledávacího dotazu:

```
Type=ApplicationInsights | measure sum(SampledCount) by TelemetryType
```

Hello **počet vzorků** pole je k dispozici ve všech položek a ukazuje hello počet datových bodů, které představují vstupní hello. Pokud zapnete vzorkování pro vaši aplikaci Application Insights **počet vzorků** je větší než 1. toocount hello skutečný počet položek, které vaše aplikace generuje, součet hello **počet vzorků** pole.

Vzorkování ovlivňuje pouze hello celkový počet položek, které vaše aplikace generuje. Nepotřebujete toocorrect vzorkování pro metriky pole **RequestDuration** nebo **AvailabilityDuration** vzhledem k tomu, že tato pole Zobrazit hello průměr pro reprezentována položky.

## <a name="input-data"></a>Vstupní data

řešení Hello obdrží hello následující typy telemetrických dat z připojených aplikací Application Insights:

- Dostupnost
- Výjimky
- Požadavky
- Stránka zobrazení – pro váš pracovní prostor tooreceive stránky zobrazení, musíte nakonfigurovat toocollect vaše aplikace tyto informace. Další informace naleznete v tématu [PageViews](../application-insights/app-insights-api-custom-events-metrics.md#page-views).
- Vlastní události – pro tooreceive vlastních událostí vašeho pracovního prostoru, musíte nakonfigurovat toocollect vaše aplikace tyto informace. Další informace naleznete v tématu [TrackEvent](../application-insights/app-insights-api-custom-events-metrics.md#trackevent).

Data je přijatých OMS z Application Insights, jakmile je k dispozici.

## <a name="output-data"></a>výstupní data

Záznam s *typ* z *ApplicationInsights* se vytvoří pro každý typ vstupní data. ApplicationInsights záznamy mají vlastnosti zobrazené v následující části hello:

### <a name="generic-fields"></a>Obecné pole

| Vlastnost | Popis |
| --- | --- |
| Typ | ApplicationInsights |
| Když |   |
| TimeGenerated | Čas záznamu hello |
| ApplicationId | Klíč instrumentace Application Insights aplikace hello |
| ApplicationName | Název aplikace služby Application Insights hello |
| RoleInstance | ID hostitelského serveru |
| DeviceType | Klientské zařízení |
| ScreenResolution |   |
| Kontinent | Kontinentě, kde hello žádost pochází |
| Země | Zemi, kde hello žádost pochází |
| Provincie | Okres, stavu nebo národní prostředí, kde hello požadavek pochází |
| Město | Města nebo lokality, kde hello žádost pochází města |
| isSynthetic | Určuje, zda hello požadavek nebyl vytvořen uživatelem nebo automatizované metodou. Hodnotu true = uživatelem generovaný nebo = false automatizované – metoda |
| SamplingRate | Procento generované hello SDK, která je odeslána tooportal telemetrie. V rozsahu 0,0 100.0. |
| SampledCount | 100/(SamplingRate). Například, 4 =&gt; 25 % |
| IsAuthenticated | True nebo False |
| OperationID | Položky, které mají stejné ID operace se zobrazují jako související položky v portálu hello hello. ID žádosti obvykle hello |
| ParentOperationID | ID operace nadřazené hello |
| OperationName |   |
| ID relace | Identifikátor GUID toouniquely identifikovat hello relaci, kde byla vytvořena žádost hello |
| SourceSystem | ApplicationInsights |

### <a name="availability-specific-fields"></a>Dostupnost konkrétních polí

| Vlastnost | Popis |
| --- | --- |
| TelemetryType | Dostupnost |
| AvailabilityTestName | Název testu webu hello |
| AvailabilityRunLocation | Zeměpisná zdroj požadavku http |
| AvailabilityResult | Označuje úspěšný výsledek hello hello webového testu |
| AvailabilityMessage | uvítací zprávu připojené toohello webového testu |
| AvailabilityCount | 100 /(Sampling Rate). Například, 4 =&gt; 25 % |
| DataSizeMetricValue | 1.0 nebo 0,0 |
| DataSizeMetricCount | 100 /(Sampling Rate). Například, 4 =&gt; 25 % |
| AvailabilityDuration | Doba v milisekundách doba trvání testu webové hello |
| AvailabilityDurationCount | 100 /(Sampling Rate). Například, 4 =&gt; 25 % |
| AvailabilityValue |   |
| AvailabilityMetricCount |   |
| AvailabilityTestId | Jedinečný identifikátor GUID pro hello webového testu |
| AvailabilityTimestamp | Přesné časové razítko test dostupnosti hello |
| AvailabilityDurationMin | Pro jen Vzorkovaná záznamy toto pole ukazuje hello webové minimální doba trvání testu (v milisekundách) pro hello reprezentované datových bodů |
| AvailabilityDurationMax | Pro jen Vzorkovaná záznamy toto pole ukazuje hello webové maximální doba trvání testu (v milisekundách) pro hello reprezentované datových bodů |
| AvailabilityDurationStdDev | Pro jen Vzorkovaná záznamy toto pole ukazuje hello směrodatná odchylka mezi všechny webový test doby trvání (v milisekundách) pro hello reprezentované datových bodů |
| AvailabilityMin |   |
| AvailabilityMax |   |
| AvailabilityStdDev | &nbsp;  |

### <a name="exception-specific-fields"></a>Pole specifické pro výjimky

| Typ | ApplicationInsights |
| --- | --- |
| TelemetryType | Výjimka |
| ExceptionType | Typ výjimky hello |
| ExceptionMethod | Hello metoda, která vytvoří hello výjimky |
| ExceptionAssembly | Sestavení obsahuje hello framework a verzi a také hello tokenu veřejného klíče |
| ExceptionGroup | Typ výjimky hello |
| ExceptionHandledAt | Označuje úroveň hello, která zpracovává hello výjimky |
| ExceptionCount | 100 /(Sampling Rate). Například, 4 =&gt; 25 % |
| ExceptionMessage | Zpráva výjimky hello |
| Zásobník výjimky | Úplné zásobníku výjimky hello |
| ExceptionHasStack | Hodnota TRUE, pokud má zásobník výjimek |



### <a name="request-specific-fields"></a>Pole specifické pro žádost

| Vlastnost | Popis |
| --- | --- |
| Typ | ApplicationInsights |
| TelemetryType | Žádost |
| ResponseCode | Tooclient odeslané odpovědi HTTP |
| RequestSuccess | Označuje úspěch nebo selhání. Hodnotu true nebo false. |
| ID žádosti | ID toouniquely identifikovat hello požadavku |
| RequestName | GET nebo POST + základní adresu URL |
| RequestDuration | Čas v sekundách, doba trvání požadavku hello |
| ADRESA URL | Adresa URL požadavku hello není včetně hostitele |
| Hostitel | Webový server hostitel |
| URLBase musí | Úplnou adresu URL požadavku hello |
| ApplicationProtocol | Typ protokolu používaný hello aplikací |
| RequestCount | 100 /(Sampling Rate). Například, 4 =&gt; 25 % |
| RequestDurationCount | 100 /(Sampling Rate). Například, 4 =&gt; 25 % |
| RequestDurationMin | Toto pole jen Vzorkovaná záznamů ukazuje hello minimální požadavek trvání (v milisekundách) pro hello reprezentované datových bodů. |
| RequestDurationMax | Pro jen Vzorkovaná záznamy toto pole ukazuje hello maximální doba trvání požadavku (v milisekundách) pro hello reprezentované datových bodů |
| RequestDurationStdDev | Pro jen Vzorkovaná záznamy toto pole ukazuje hello směrodatná odchylka mezi všechny žádosti o doby trvání (v milisekundách) pro hello reprezentované datových bodů |

## <a name="sample-log-searches"></a>Ukázky hledání v protokolech

Toto řešení nemá sadu ukázkový protokol hledání zobrazený na řídicí panel hello. Ukázkové dotazy vyhledávání protokolu s popisy se však uvádějí v hello [informace o zobrazení Application Insights konektoru](#view-application-insights-connector-information) části.

## <a name="next-steps"></a>Další kroky

- Použití [hledání protokolů](log-analytics-log-searches.md) tooview podrobné informace pro vaše aplikace Application Insights.
