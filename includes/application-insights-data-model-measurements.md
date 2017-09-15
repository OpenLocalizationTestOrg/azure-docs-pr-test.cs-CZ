<span data-ttu-id="94cb3-101">Kolekce vlastních měření.</span><span class="sxs-lookup"><span data-stu-id="94cb3-101">Collection of custom measurements.</span></span> <span data-ttu-id="94cb3-102">Použijte tuto kolekci pro sestavy s názvem měření přidružené k položce telemetrie.</span><span class="sxs-lookup"><span data-stu-id="94cb3-102">Use this collection to report named measurement associated with the telemetry item.</span></span> <span data-ttu-id="94cb3-103">Typické případy použití jsou:</span><span class="sxs-lookup"><span data-stu-id="94cb3-103">Typical use cases are:</span></span>
- <span data-ttu-id="94cb3-104">velikost Telemetrických závislostí datové části</span><span class="sxs-lookup"><span data-stu-id="94cb3-104">the size of Dependency Telemetry payload</span></span>
- <span data-ttu-id="94cb3-105">počet položek fronty zpracovávaných požadavků Telemetrie</span><span class="sxs-lookup"><span data-stu-id="94cb3-105">the number of queue items processed by Request Telemetry</span></span>
- <span data-ttu-id="94cb3-106">čas tohoto zákazníka trvalo dokončení kroku při dokončení průvodce krok události Telemetrie.</span><span class="sxs-lookup"><span data-stu-id="94cb3-106">time that customer took to complete the step in wizard step completion Event Telemetry.</span></span>

<span data-ttu-id="94cb3-107">Můžete zadat dotaz [vlastní měření](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) v analýza aplikace:</span><span class="sxs-lookup"><span data-stu-id="94cb3-107">You can query [custom measurements](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) in Application Analytics:</span></span>

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > <span data-ttu-id="94cb3-108">Vlastní měření jsou přidružená k položce telemetrie, ke které patří.</span><span class="sxs-lookup"><span data-stu-id="94cb3-108">Custom measurements are associated with the telemetry item they belong to.</span></span> <span data-ttu-id="94cb3-109">Jsou předmětem vzorkování s položkou telemetrie obsahující tyto měření.</span><span class="sxs-lookup"><span data-stu-id="94cb3-109">They are subject to sampling with the telemetry item containing those measurements.</span></span> <span data-ttu-id="94cb3-110">Chcete-li sledovat Měrná jednotka, která má hodnotu nezávislé na jiné typy telemetrických dat, použijte [metriky telemetrie](../articles/application-insights/app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="94cb3-110">To track a measurement that has a value independent from other telemetry types, use [Metric telemetry](../articles/application-insights/app-insights-api-custom-events-metrics.md).</span></span>

<span data-ttu-id="94cb3-111">Maximální délka klíče: 150</span><span class="sxs-lookup"><span data-stu-id="94cb3-111">Max key length: 150</span></span>
