---
title: aaaIntroduction tooStream Analytics | Microsoft Docs
description: "Další informace o Stream Analytics, spravované služby, která pomáhá analyzovat data streamovaná z hello Internet věcí (IoT) v reálném čase."
keywords: "analytics jako služba, spravované služby, zpracování datového proudu, streamování analytics, co je datový proud analytics"
services: stream-analytics
documentationcenter: 
author: jenniehubbard
manager: jhubbard
editor: cgronlun
ms.assetid: 613c9b01-d103-46e0-b0ca-0839fee94ca8
ms.service: stream-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/08/2017
ms.author: jhubbard
ms.openlocfilehash: 6dd7ea1d358bcc94e927a3e699a2771a25104d72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-stream-analytics"></a>Co je služba Stream Analytics?

Azure Stream Analytics je plně spravovaný modul na zpracování událostí, který umožňuje nastavit a provádět analytické výpočty streamovaných dat v reálném čase. Hello data mohou pocházet ze zařízení, senzorů, webů, informačních kanálů sociálních médií, aplikací, systémů infrastruktury a další. 

## <a name="what-can-i-do-with-stream-analytics"></a>Co se dá se službou Stream Analytics dělat?

Použití Stream Analytics tooexamine velký objem dat odesílaných ze zařízení nebo procesy, extrahovat informace z datového proudu hello a vyhledejte vzory, trendy a vztahy. Podle toho, co je v datech hello, pak můžete provádět úlohy aplikace. Může vyvolat výstrahy, ji automatizace pracovních, kanálu tooa informace o vytváření sestav nástroje, jako je Power BI nebo ukládání dat pro novější šetření. 

Příklady:

* Personalizované analýzy obchodování na burze a výstrahy v reálném čase nabízené finančními institucemi
* Odhalování podvodů v reálném čase prověřováním dat transakcí 
* Služby ochrany dat a identity
* Analýza dat generovaných senzory a poháněcími zařízeními ve fyzických objektech (Internet věcí nebo IoT)
* Analýza navštívených webových stránek
* Aplikace řízení vztahů se zákazníky (CRM), například vydání výstrahy, když se v určitém časovém rámci sníží kvalita zákaznických služeb

## <a name="how-does-stream-analytics-work"></a>Jak funguje Stream Analytics?

Tento diagram znázorňuje hello Stream Analytics kanálu, jak požity, analyzovat a pak odešlou pro prezentaci nebo akce data. 

![Kanál Stream Analytics](./media/stream-analytics-introduction/stream_analytics_intro_pipeline.png)

Stream Analytics začíná zdrojem streamovaných dat. Hello data můžete ze zařízení pomocí Azure event hub nebo služby IoT hub konzumaci do Azure. Hello dat může vyžádat také z jiného úložiště dat jako úložiště objektů Blob Azure. 

datový proud hello tooexamine, vytvořit Stream Analytics *úlohy* určující, kde hello data pocházejí. rovněž určuje úlohy Hello *transformace*&mdash;jak toolook dat, vzory nebo vztahy. Stream Analytics podporuje pro tuto úlohu dotazovací jazyk typu SQL, který umožňuje filtrovat, řadit, agregovat a spojovat streamovaná data za určité časové období.

Nakonec hello úlohy Určuje výstupní toosend hello transformovat data do. Díky tomu můžete řídit, jaké toodo v odpovědi toohello informace jste analyzovat. V odpovědi tooanalysis pravděpodobně například:

* Odešlete příkaz toochange nastavení zařízení. 
* Odeslání dat tooa fronty, který je monitorován pomocí procesu, který provede akci založenou na co najde. 
* Odeslání dat řídicí panel Power BI tooa pro vytváření sestav.
* Odeslání dat toostorage jako Data Lake Store, databáze systému SQL Server nebo úložiště objektů Blob v Azure nebo tabulky.

Probíhající úlohu můžete monitorovat a upravovat počet událostí, které zpracovává za sekundu. Úlohy můžou také vytvářet diagnostické protokoly k řešení potíží.

## <a name="key-capabilities-and-benefits"></a>Klíčové funkce a výhody

Stream Analytics je snadno toouse navrženou toobe, flexibilní, škálovatelný tooany úlohy velikost a ekonomické.

### <a name="connectivity-toomany-inputs-and-outputs"></a>Připojení toomany vstupy a výstupy

Stream Analytics připojuje přímo příliš[Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) a [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) pro přijímání datového proudu a hello [služby úložiště objektů Blob Azure](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage-accounts) tooingest historická data. Když získáte data z center událostí, můžete Stream Analytics zkombinovat s dalšími zdroji dat a moduly na zpracování událostí.

Vstup úlohy může také zahrnovat referenční data (statická nebo pomalu se měnící data). Toho se můžete zapojit streamování dat toothis referenční data tooperform vyhledávání operations hello stejně jako kdybyste s dotazy na databázi.

Výstup úlohy Stream Analytics můžete směrovat mnoha směry. Můžete napsat toostorage, jako je například objektů BLOB služby Azure Storage nebo tabulky, databázi SQL Azure, Azure Data Lake úložišť nebo databázi Azure Cosmos. Odtud může přejděte hello data pro analýzu batch prostřednictvím Azure HDInsight. Můžete odeslat hello výstup tooanother službu pro spotřebu jiným procesem, například služby event hubs, Azure Service Bus témata nebo fronty. Můžete odeslat tooPower výstup hello BI pro vizualizaci.

### <a name="ease-of-use"></a>Snadné používání

Transformace toodefine použít jednoduchý deklarativní [Stream Analytics query language](https://msdn.microsoft.com/library/azure/dn834998.aspx) který umožňuje vytvořit sofistikované analýzy se žádné programování. dotazovací jazyk Hello trvá dat jako vstup. Můžete pak filtrování a řazení dat hello, souhrnné hodnoty, provádění výpočtů, připojení dat (v rámci datového proudu nebo tooreference data) a použít geoprostorové funkce. Můžete upravit dotazy hello portálu pomocí IntelliSense a kontrolu syntaxe a můžete otestovat dotazy pomocí ukázková data, která lze extrahovat z hello živý datový proud.

### <a name="extensible-query-language"></a>Rozšiřitelný dotazovací jazyk

Definováním a vyvolání další funkce můžete rozšířit možnosti hello hello dotazovací jazyk. Volání funkcí můžete definovat v hello využít tootake služby Azure Machine Learning řešení Azure Machine Learning. Můžete také integrovat JavaScript uživatelsky definované funkce (UDF) ve výpočtech komplexní tooperform pořadí v rámci dotazu Stream Analytics.

### <a name="scalability"></a>Škálovatelnost

Stream Analytics může zpracovat až too1 GB příchozích dat za sekundu. Integrace s [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) a [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) umožňuje úlohy tooingest miliony událostí za sekundu z připojených zařízení, webů a soubory protokolu, tooname pár. Pomocí funkce oddílu hello služby event hubs můžete oddílu výpočtů do logických kroků, každý s hello možnost Další toobe tooincrease škálovatelnost rozdělena na oddíly.

### <a name="low-cost"></a>Nízké náklady

Jako cloudová služba Stream Analytics je optimalizované toolet, které jste mohli začít za nízkou cenu. Platíte rovnou na základě na jednotek streaming využití a hello množství dat, zpracuje systém hello. Využití je založena na hello objemu zpracovaných událostí a hello množství výpočetním výkonu poskytnutém v rámci clusteru toohandle hello úlohy Stream Analytics.

### <a name="reliability-quick-recovery-and-repeatability"></a>Spolehlivost, rychlé obnovení a opakovatelnost

Jako spravované služby v cloudu hello Stream Analytics pomáhá bránit ztrátám dat a poskytuje kontinuity podnikových procesů. Pokud dojde k selhání, hello služba poskytuje vestavěným funkcím pro obnovení. Se schopností hello toointernally Udržovat stav, hello service poskytuje je možné tooarchive události a znovu použít zpracování v budoucnu hello vždy získávání hello stejné výsledky. To vám umožní toogo zpět v čase a prozkoumat výpočty při provádění analýzy příčin, analýz a tak dále.

## <a name="next-steps"></a>Další kroky

* Začněte [experimentovat se vstupy a dotazy ze zařízení IoT](stream-analytics-get-started-with-azure-stream-analytics-to-process-data-from-iot-devices.md).
* Sestavení [začátku do konce Stream Analytics řešení](stream-analytics-real-time-fraud-detection.md) který zkoumá telefonní toolook metadata pro podvodné volání.
* Informace o hello SQL jako dotazovacího jazyka pro Stream Analytics a o jedinečný koncepty jako [okno funkce](stream-analytics-window-functions.md).
* Zjistěte, jak příliš[škálování úlohy Stream Analytics](stream-analytics-scale-jobs.md). 
* Zjistěte, jak příliš[integrovat Stream Analytics a Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).
* Odpovědi tooyour Stream Analytics dotazy v hello [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

