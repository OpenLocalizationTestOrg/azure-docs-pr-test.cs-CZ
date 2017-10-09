<span data-ttu-id="fee5a-101">Kolekce vlastních měření.</span><span class="sxs-lookup"><span data-stu-id="fee5a-101">Collection of custom measurements.</span></span> <span data-ttu-id="fee5a-102">Pomocí této kolekce tooreport s názvem měření přidružený k položce telemetrie hello.</span><span class="sxs-lookup"><span data-stu-id="fee5a-102">Use this collection tooreport named measurement associated with hello telemetry item.</span></span> <span data-ttu-id="fee5a-103">Typické případy použití jsou:</span><span class="sxs-lookup"><span data-stu-id="fee5a-103">Typical use cases are:</span></span>
- <span data-ttu-id="fee5a-104">velikost Hello Telemetrických závislostí datové části</span><span class="sxs-lookup"><span data-stu-id="fee5a-104">hello size of Dependency Telemetry payload</span></span>
- <span data-ttu-id="fee5a-105">Hello počet položek fronty zpracovávaných požadavků Telemetrie</span><span class="sxs-lookup"><span data-stu-id="fee5a-105">hello number of queue items processed by Request Telemetry</span></span>
- <span data-ttu-id="fee5a-106">čas tohoto zákazníka trvalo toocomplete hello krok při dokončení průvodce krok události Telemetrie.</span><span class="sxs-lookup"><span data-stu-id="fee5a-106">time that customer took toocomplete hello step in wizard step completion Event Telemetry.</span></span>

<span data-ttu-id="fee5a-107">Můžete zadat dotaz [vlastní měření](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) v analýza aplikace:</span><span class="sxs-lookup"><span data-stu-id="fee5a-107">You can query [custom measurements](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) in Application Analytics:</span></span>

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > <span data-ttu-id="fee5a-108">Vlastní měření jsou přidružené k položce telemetrie hello, ke které patří.</span><span class="sxs-lookup"><span data-stu-id="fee5a-108">Custom measurements are associated with hello telemetry item they belong to.</span></span> <span data-ttu-id="fee5a-109">Jsou to subjektu toosampling s položkou telemetrie hello obsahující tyto měření.</span><span class="sxs-lookup"><span data-stu-id="fee5a-109">They are subject toosampling with hello telemetry item containing those measurements.</span></span> <span data-ttu-id="fee5a-110">tootrack Měrná jednotka, která má nezávislé na jiné typy telemetrických dat, použijte hodnotu [metriky telemetrie](../articles/application-insights/app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="fee5a-110">tootrack a measurement that has a value independent from other telemetry types, use [Metric telemetry](../articles/application-insights/app-insights-api-custom-events-metrics.md).</span></span>

<span data-ttu-id="fee5a-111">Maximální délka klíče: 150</span><span class="sxs-lookup"><span data-stu-id="fee5a-111">Max key length: 150</span></span>
