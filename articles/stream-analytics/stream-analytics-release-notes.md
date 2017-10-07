---
title: "aaaStream poznámky k verzi Analytics | Microsoft Docs"
description: "Poznámky k verzi Stream Analytics"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 5e59f893-cd2c-43fb-9eca-c146ce637203
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 05/03/2017
ms.author: jeffstok
ms.openlocfilehash: 1426ebc260be19fbde60518b25501fe53d908d46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-release-notes"></a>Poznámky k verzi služby Stream Analytics

## <a name="notes-for-06142017-update-of-stream-analytics-tools-for-visual-studio"></a>Poznámky pro aktualizaci 06/14/2017 Stream Analytics Tools pro Visual Studio
Tato aktualizace je pro naše nástroje sady Visual Studio. Tato verze obsahuje následující nové funkce hello.

| Název | Popis |
| --- | --- |
| Podpora editor skriptů v jazyce Java |Můžete sledovat hello nativní java skriptu editor zkušeností po vytvoření skriptu funkce vaší java.|
| Zobrazit úlohy spustit čas chybová zpráva | Pokud během provádění úlohy chyby za běhu, můžete je zobrazit v kartě chyby hello úpravou hello zobrazení časové okno. Ve výchozím nastavení zobrazuje hello chybové zprávy pro posledních 30 minut. |
| Podpora sdílených svazků clusteru a Avro pro místní testování vstup | Kromě toho formát JSON teď můžete formát souboru CSV a Avro pro místní testování vstup.|

## <a name="notes-for-05032017-update-of-stream-analytics"></a>Poznámky pro aktualizaci 03/05/2017 Stream Analytics
Tato aktualizace je naše řešení potíží dokumentaci k verzi.

Hello [Průvodce odstraňováním potíží s](stream-analytics-troubleshooting-guide.md) a byly vydané jiné dokumenty. Zkontrolujte, zpětná vazba je úvodní.

## <a name="notes-for-04242017-update-of-stream-analytics-tools-for-visual-studio"></a>Poznámky pro aktualizaci 04/24/2017 Stream Analytics Tools pro Visual Studio
Tato aktualizace je pro naše nástroje sady Visual Studio. Tato verze obsahuje následující nové funkce hello.

| Název | Popis |
| --- | --- |
| Zobrazení výsledků místní testů v sadě Visual Studio | výsledek výstup hello tooview hello místní testovací, stačí stisknout klávesu ENTER v okně konzoly výstup hello nebo ho zavřete. výsledek Hello se zobrazí v okně v sadě Visual Studio ve formátu tabulky. |
| Místní výsledných výstupů ve formátu JSON | Při spuštění testu místní výsledných výstupů hello se budou generovat jako JSON a CSV formátů souborů. |
| Zobrazte náhled dat vstupy/výstupy úložiště objektů Blob nebo table | Poklepáním na na úložiště objektů blob nebo table vstupu a výstupu v zobrazení úloh hello je velmi snadno prohlédnout hello data v sadě Visual Studio. |
| Zobrazení chybové zprávy pro vstupy/výstupy | Pokud jsou některé runtime chyby související tooyou úlohy vstupů nebo výstupů, se bude zobrazovat v diagramu hello úlohy, kde můžete podržet na něm toosee hello Podrobná chybová zpráva.|


## <a name="notes-for-02012017-release-of-stream-analytics"></a>Poznámky k verzi 02/01/2017 Stream Analytics
Tato verze obsahuje hello následující aktualizace.

| Název | Popis |
| --- | --- |
| Představení JavaScript uživatelsky definované funkce (UDF) |[Funkce definované uživatelem Java](stream-analytics-javascript-user-defined-functions.md) jsou nyní k dispozici pro větší flexibilita při vytváření dotazů. |
| Představení nástroje pro Visual Studio a Stream Analytics |[Nástroje pro sadu Visual Studio](stream-analytics-tools-for-visual-studio.md) jsou nyní k dispozici pro nástroj pro ladění a větší. |
| Představení protokolování diagnostiky |[Protokolování diagnostiky](stream-analytics-job-diagnostic-logs.md) je nyní k dispozici pro další možnosti řešení problémů. |
| Představení geoprostorové funkce |[Funkce geoprostorové](http://msdn.microsoft.com/library/mt778980(Azure.100).aspx) jsou nyní obecně k dispozici. |

## <a name="notes-for-04152016-release-of-stream-analytics"></a>Poznámky k verzi 04/15 2016 služby Stream Analytics
Tato verze obsahuje hello následující aktualizace.

| Název | Popis |
| --- | --- |
| Obecné dostupnosti pro výstupů Power BI |[Power BI výstupy](stream-analytics-power-bi-dashboard.md) jsou nyní obecně k dispozici. Odebrali jsme Hello 90denní autorizace vypršení platnosti pro Power BI. Další informace o scénářích, kde autorizace musí toobe obnovit najdete v části hello [obnovit autorizace](stream-analytics-power-bi-dashboard.md#renew-authorization) části vytváření řídicí panel Power BI. |

## <a name="notes-for-03032016-release-of-stream-analytics"></a>Poznámky k verzi 03 03. 2016 Stream Analytics
Tato verze obsahuje hello následující aktualizace.

| Název | Popis |
| --- | --- |
| Nové položky Stream Analytics Query Language |SAQL má nyní [GetType](https://msdn.microsoft.com/library/azure/mt643890.aspx "stránka MSDN GetType"), [TRY_CAST](https://msdn.microsoft.com/library/azure/mt643735.aspx "stránka MSDN TRY_CAST") a [REGEXMATCH] (https://msdn.microsoft.com/library/azure/mt643891.aspx "Stránka MSDN REGEXMATCH"). |

## <a name="notes-for-12102015-release-of-stream-analytics"></a>Poznámky k verzi 12/10/2015 Stream Analytics
Tato verze obsahuje hello následující aktualizace.

| Název | Popis |
| --- | --- |
| Aktualizace verze rozhraní REST API |verze rozhraní REST API Hello byl aktualizovaný too2015-10-01. Podrobnosti najdete na webu MSDN na [referenční příručka Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx) a [integrace Machine Learning v Stream Analytics](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md). |
| Integrace Azure Machine Learning |Tato verze obsahuje podporu pro Azure Machine Learning uživatelem definované funkce. V tématu hello [kurzu](stream-analytics-machine-learning-integration-tutorial.md) pro další informace a také hello [obecné blog oznámení](http://blogs.technet.com/b/machinelearning/archive/2015/12/10/apply-azure-ml-as-a-function-in-azure-stream-analytics.aspx). |

## <a name="notes-for-11122015-release-of-stream-analytics"></a>Poznámky k verzi 11. 12/2015 Stream Analytics
Tato verze obsahuje hello následující aktualizace.

| Název | Popis |
| --- | --- |
| Nové chování vyberte |Vyberte v Stream Analytics byl rozšířené tooallow * jako přistupujícího objektu vlastnosti vnořené záznamu. Další informace vám poskytne [http://msdn.microsoft.com/library/mt622759.aspx](http://msdn.microsoft.com/library/mt622759.aspx "komplexní typy dat"). |

## <a name="notes-for-10222015-release-of-stream-analytics"></a>Poznámky k verzi 22/10/2015 Stream Analytics
Tato verze obsahuje hello následující aktualizace.

| Název | Popis |
| --- | --- |
| Další dotaz jazykové funkce |Stream Analytics obsahuje dotazovací jazyk hello rozšiřovat, včetně hello následující funkce: [ABS](https://msdn.microsoft.com/library/azure/mt574054.aspx), [mezní hodnoty](https://msdn.microsoft.com/library/azure/mt605286.aspx), [EXP](https://msdn.microsoft.com/library/azure/mt605289.aspx), [PODLAŽÍ](https://msdn.microsoft.com/library/azure/mt605240.aspx), [POWER](https://msdn.microsoft.com/library/azure/mt605287.aspx), [PŘIHLAŠOVACÍ](https://msdn.microsoft.com/library/azure/mt605290.aspx), [HRANATÉ](https://msdn.microsoft.com/library/azure/mt605288.aspx), a [SQRT](https://msdn.microsoft.com/library/azure/mt605238.aspx). |
| Odebrat agregační omezení |Tato verze odebere hello omezení 15 agregací v dotazu. Je nyní žádné omezení toohello počet agregace na jeden dotaz. |
| Funkce skupiny podle System.Timestamp |Hello [GROUP BY](https://msdn.microsoft.com/library/azure/dn835023.aspx) funkce teď umožňuje buď window_type nebo [System.Timestamp](https://msdn.microsoft.com/library/azure/mt598501.aspx). |
| Přidání POSUNUTÍ pro Přeskakujícího a přepínání windows |Ve výchozím nastavení [Přeskakujícího](https://msdn.microsoft.com/library/azure/dn835055.aspx) a [Hopping](https://msdn.microsoft.com/library/azure/dn835041.aspx) windows jsou v souladu s přechodu do režimu (1/1/0001 12:00:00 AM UTC). Hello nové (volitelné) parametr 'offsetsize' umožňuje zadat vlastní posun (nebo zarovnání). |

## <a name="notes-for-09292015-release-of-stream-analytics"></a>Poznámky k verzi 29/09/2015 Stream Analytics
Tato verze obsahuje hello následující aktualizace.

| Název | Popis |
| --- | --- |
| Verzi Public Preview služby Azure IoT Suite |Stream Analytics je součástí hello Public Preview hello Azure IoT Suite. |
| Integrace se službou Azure Portal |V přidání toocontinued přítomnosti v hello Azure Management portal, Stream Analytics je teď plně integrovaný v hello [portálu Azure](https://azure.microsoft.com/overview/preview-portal/). Všimněte si, že funkce Stream Analytics na portálu Preview hello je aktuálně podmnožinu funkcí hello nenabízí hello Azure Management portal, bez podpory pro testování dotazů v prohlížeči, že Power BI výstup konfigurace a procházení tooor vytváření nových vstupní a výstupní prostředky v odběry, které máte přístup. |
| Podpora pro výstup Cosmos DB |Úlohy Stream Analytics můžete teď výstupní příliš[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/). |
| Podpora pro vstup IoT Hub |Úlohy Stream Analytics můžete nyní načítání dat z centra IoT. |
| TIMESTAMP BY pro heterogenní události |Když jeden datový proud obsahuje více typů událostí s časová razítka v různých polí, teď můžete použít [TIMESTAMP BY](http://msdn.microsoft.com/library/mt573293.aspx) s výrazy toospecify různých časové razítko pole pro každý případ. |

## <a name="notes-for-09102015-release-of-stream-analytics"></a>Poznámky k verzi 09/10/2015 Stream Analytics
Tato verze obsahuje hello následující aktualizace.

| Název | Popis |
| --- | --- |
| Podpora pro PowerBI skupiny |sdílení dat s jinými uživateli Power BI, Stream Analytics úloh můžete nyní zápisu příliš tooenable[PowerBI skupiny](stream-analytics-define-outputs.md#power-bi) uvnitř svůj účet Power BI. |

## <a name="notes-for-08202015-release-of-stream-analytics"></a>Poznámky k verzi 08/20/2015 Stream Analytics
Tato verze obsahuje hello následující aktualizace.

| Název | Popis |
| --- | --- |
| Přidat poslední – funkce |Hello [poslední](http://msdn.microsoft.com/library/mt421186.aspx) funkce je nyní k dispozici v úlohy Stream Analytics, takže se budete tooretrieve hello poslední událost v datového proudu událostí v daném časovém rámci. |
| Nové funkce pole |Pole funkce [GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx), [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx) a [GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx) jsou nyní k dispozici. |
| Nové funkce v záznamu |Zaznamenejte funkce [GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx) a [GetRecordPropertyValue](http://msdn.microsoft.com/library/mt270220.aspx) jsou nyní k dispozici. |

## <a name="notes-for-07302015-release-of-stream-analytics"></a>Poznámky k verzi 07/30/2015 Stream Analytics
Tato verze obsahuje hello následující aktualizace.

| Název | Popis |
| --- | --- |
| Power BI Org Id odpojené od Azure Id |Tato funkce umožňuje [výstup Power BI](stream-analytics-power-bi-dashboard.md) pro úlohy ASA pod libovolného typu účet Azure (Id organizace nebo Live Id). Kromě toho můžete mít jeden Org Id pro účet Azure a použijte jiný určovat výstup Power BI. |
| Podpora pro výstupní fronty služby Service Bus |[Fronty služby Service Bus](stream-analytics-define-outputs.md#service-bus-queues) výstupy jsou nyní k dispozici v úlohy Stream Analytics. |
| Podpora pro výstup témata služby Service Bus |[Témata služby Service Bus](stream-analytics-define-outputs.md#service-bus-topics) výstupy jsou nyní k dispozici v úlohy Stream Analytics. |

## <a name="notes-for-07092015-release-of-stream-analytics"></a>Poznámky k verzi 07/09/2015 Stream Analytics
Tato verze obsahuje hello následující aktualizace.

| Název | Popis |
| --- | --- |
| Vlastní Blob výstup dělení |Výstupy úložiště objektů BLOB teď povolit možnost toospecify hello frekvence, který výstup objekty BLOB se zapisují a struktura hello a formát hello výstupní datová struktura složky cesta. |

## <a name="notes-for-05032015-release-of-stream-analytics"></a>Poznámky k verzi 03/05/2015 Stream Analytics
Tato verze obsahuje hello následující aktualizace.

| Název | Popis |
| --- | --- |
| Zvýšit maximální hodnota pro interval Tolerance pořádku |maximální velikost hello interval Tolerance pořádku Hello je nyní 59:59 (mm: ss) |
| Výstupní formát JSON: Řádcích nebo pole |Nyní je možnost při výstupu tooBlob úložiště nebo Centrum událostí toooutput buď pole objektů JSON nebo oddělením objekty JSON s novým řádkem. |

## <a name="notes-for-04162015-release-of-stream-analytics"></a>Poznámky k verzi 04/16/2015 Stream Analytics
| Název | Popis |
| --- | --- |
| Zpoždění v konfiguraci účtu Azure Storage |Při vytváření úlohy Stream Analytics v oblasti pro hello poprvé, budete mít výzvami toocreate nový účet úložiště nebo zadat účet existujícího pro monitorování úlohy Stream Analytics v této oblasti. Kvůli toolatency v konfiguraci, monitorování, vytváření jiná úloha Stream Analytics ve stejné oblasti do 30 minut zobrazí výzvu k zadání hello druhý účtu úložiště místo zobrazující hello hello nedávno nenakonfigurovali v hello monitorování úložiště Účet rozevíracího seznamu. tooavoid vytvoření nepotřebné účtu úložiště, počkejte 30 minut po vytvoření úlohy v oblasti pro hello poprvé před zřizováním další úlohy v této oblasti. |
| Úlohy upgradu |V tomto okamžiku Stream Analytics nepodporuje živé úpravy toohello definici nebo konfiguraci běžící úlohy. V pořadí toochange hello vstupní, výstupní, dotaz, škálování nebo konfigurace běžící úlohy je třeba nejprve ukončit úlohu hello. |
| Datové typy odvozené ze vstupního zdroje |Pokud příkaz CREATE TABLE se nepoužívá, hello vstupní typ je odvozen od formát vstupu, například všechna pole ze souboru CSV jsou řetězec. Pole toobe potřeba převést explicitně toohello správný typ. pomocí funkce CAST hello v pořadí tooavoid typ chyby. |
| Chybějící pole jsou výstupem jako hodnoty null |Odkazování na pole, které se nenachází v hello vstupní zdroj bude výsledkem hodnoty null v případě výstup hello. |
| S příkazy musí předcházet příkazů SELECT |V dotazu musí následovat příkazů SELECT poddotazy definované pomocí příkazů. |
| Problém z důvodu nedostatku paměti |Úloha streamování analýzy velkých toleranci události mimo pořadí nebo složitých dotazů zachování velké množství stavu, může dojít hello úlohy toorun nedostatek paměti, což vede k úlohu restartujte. Hello spuštění a zastavení operace budou viditelné v protokolech úlohy hello operaci. tooavoid toto chování, dotaz hello škálování odhlašování napříč více oddílů. V budoucí verzi bude toto omezení řešit došlo ke snížení výkonu u ovlivněné úlohy místo restartování je. |
| Vstupy velkých objektů blob bez časového razítka datové části může způsobit problém z důvodu nedostatku paměti |Použití velkých souborů z úložiště objektů Blob může způsobit toocrash úlohy Stream Analytics, pokud pole časového razítka není zadaný prostřednictvím TIMESTAMP BY. tooavoid-li tento problém, mějte velikost jednotlivých objektů blob v části 10MB. |
| Omezení svazku událostí databáze SQL |Při použití SQL Database jako cíl výstupu, může způsobit velmi velký objem výstupní data tootime úlohy Stream Analytics hello se. tooresolve-li tento problém, buď snížit objem výstup hello pomocí agregací nebo operátory filtru, nebo místo toho zvolte úložiště objektů Blob v Azure nebo Event Hubs jako cíl výstupu. |
| Datové sady PowerBI může obsahovat pouze jednu tabulku |PowerBI nepodporuje více než jedné tabulky v dané datové sadě. |

## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
