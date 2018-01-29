Existují některá omezení počtu metrik a událostí na aplikaci (tj. na instrumentační klíč). Omezení závisí na zvoleném [cenovém plánu](https://azure.microsoft.com/pricing/details/application-insights/).

| **Prostředek** | **Výchozí omezení** | **Poznámka**
| --- | --- | --- |
| Celkem dat za den | 100 GB | Objem dat jde snížit nastavením limitu. Pokud potřebujete více, můžete zvýšit limit až 1 000 GB z portálu. Kapacitou větší než 1 000 GB, odesílat e-mailu AIDataCap@microsoft.com.
| Volná data za měsíc<br/> (Základní cenový plán) | 1 GB | Další data se účtují za gigabajt.
| Omezování | 32 000 událostí za sekundu | Omezení se měří se po minutách.
| Uchovávání dat | 90 dnů | Tento prostředek je pro funkce [Vyhledávání](../articles/application-insights/app-insights-diagnostic-search.md), [Analýza](../articles/application-insights/app-insights-analytics.md) a [Průzkumník metrik](../articles/application-insights/app-insights-metrics-explorer.md).
| [Vícekrokový test dostupnosti](../articles/application-insights/app-insights-monitor-web-app-availability.md#multi-step-web-tests) – uchování podrobných výsledků | 90 dnů | Tento prostředek poskytuje podrobné výsledky každého kroku.
| Maximální velikost události | 64 KB | 
| Délka názvu vlastnosti a metriky | 150 | V tématu [zadejte schémat.](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/)
| Délka řetězce hodnoty vlastnosti | 8 192 | V tématu [zadejte schémat.](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/)
| Délka zprávy trasování a výjimky | 10 000 | V tématu [zadejte schémat.](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/)
| [Testy dostupnosti](../articles/application-insights/app-insights-monitor-web-app-availability.md) – počet na aplikaci  | 100 |
| [Profileru](../articles/application-insights/app-insights-profiler.md) uchovávání dat | 5 dní |
| [Profileru](../articles/application-insights/app-insights-profiler.md) data odeslaná za den | 10GB |

Další informace najdete v tématu [Ceny a kvóty ve službě Application Insights](../articles/application-insights/app-insights-pricing.md).

