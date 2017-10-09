Kolekce vlastních měření. Pomocí této kolekce tooreport s názvem měření přidružený k položce telemetrie hello. Typické případy použití jsou:
- velikost Hello Telemetrických závislostí datové části
- Hello počet položek fronty zpracovávaných požadavků Telemetrie
- čas tohoto zákazníka trvalo toocomplete hello krok při dokončení průvodce krok události Telemetrie.

Můžete zadat dotaz [vlastní měření](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) v analýza aplikace:

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > Vlastní měření jsou přidružené k položce telemetrie hello, ke které patří. Jsou to subjektu toosampling s položkou telemetrie hello obsahující tyto měření. tootrack Měrná jednotka, která má nezávislé na jiné typy telemetrických dat, použijte hodnotu [metriky telemetrie](../articles/application-insights/app-insights-api-custom-events-metrics.md).

Maximální délka klíče: 150
