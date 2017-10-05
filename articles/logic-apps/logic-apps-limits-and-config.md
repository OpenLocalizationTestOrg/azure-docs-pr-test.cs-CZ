---
title: "Omezení aplikace logiky a konfigurace | Microsoft Docs"
description: "Přehled limity služby a hodnoty konfigurace, které jsou k dispozici pro Logic Apps."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: da23bd9fe71a0c41bc236b55bc9f56e123a9d77a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="logic-app-limits-and-configuration"></a>Omezení a konfigurace aplikací logiky

Toto je informace o aktuální omezení a podrobnosti o konfiguraci pro Azure Logic Apps.

## <a name="limits"></a>Omezení

### <a name="http-request-limits"></a>Omezení požadavků HTTP

Toto jsou omezení pro jednoho volání požadavků a konektor protokolu HTTP.

#### <a name="timeout"></a>Časový limit

|Name (Název)|Omezení|Poznámky|
|----|----|----|
|Časový limit požadavku.|120 sekundách|[Asynchronní vzor](../logic-apps/logic-apps-create-api-app.md) nebo [dokud smyčky](logic-apps-loops-and-scopes.md) můžete odpovídajícím způsobem podle potřeby|

#### <a name="message-size"></a>Velikost zpráv

|Name (Název)|Omezení|Poznámky|
|----|----|----|
|Velikost zpráv|100 MB|Některé konektory a rozhraní API nemusí podporovat 100 MB |
|Limit vyhodnocení výrazu|131 072 znaků|`@concat()`, `@base64()`, `string` nesmí být delší než tento limit|

#### <a name="retry-policy"></a>Zásady opakování

|Name (Název)|Omezení|Poznámky|
|----|----|----|
|Opakované pokusy|10| Výchozí hodnota je 4. Můžete nakonfigurovat [opakujte parametr zásad](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Maximální zpoždění při opakování|1 hodina|Můžete nakonfigurovat [opakujte parametr zásad](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Minimální zpoždění při opakování|5 s|Můžete nakonfigurovat [opakujte parametr zásad](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|

### <a name="run-duration-and-retention"></a>Doba trvání spuštění a jejich uchovávání

Toto jsou omezení u jednoho logiku aplikace spustit.

|Name (Název)|Omezení|Poznámky|
|----|----|----|
|Doba trvání spuštění|90 dnů||
|Uchování úložiště|90 dnů|Od doby běhu start|
|Interval opakování min.|1 sekundu|| 15 sekund pro logic apps se plán služby App Service
|Maximální interval opakování|500 dnů||

Pokud byste měli být delší než doba trvání spuštění nebo omezení uchování úložiště v normálním zpracování toku [kontaktujte nás](mailto://logicappsemail@microsoft.com) tak, aby bylo možné pomoci vašim požadavkům.


### <a name="looping-and-debatching-limits"></a>Ve smyčce a debatching omezení

Toto jsou omezení pro aplikace logiky jeden spustit.

|Name (Název)|Omezení|Poznámky|
|----|----|----|
|Foreach – položky|100,000|Můžete použít [dotaz akce](../connectors/connectors-native-query.md) vyfiltrujete větší pole, podle potřeby|
|Dokud iterací|5,000||
|SplitOn položky|100,000||
|ForEach paralelismus|50| Výchozí hodnota je 20. Můžete nastavit na sekvenční foreach přidáním `"operationOptions": "Sequential"` k `foreach` akce nebo konkrétní úroveň pomocí paralelismus`runtimeConfiguration`|


### <a name="throughput-limits"></a>Omezení propustnosti

Toto jsou omezení pro instanci jednoho logiku aplikace. 

|Name (Název)|Omezení|Poznámky|
|----|----|----|
|Akce spuštěních za 5 minut |100,000|Můžete rozdělit zatížení mezi více aplikacemi podle potřeby|
|Souběžných volání odchozí akce |~2,500|Snižte počet souběžných požadavků nebo zkrátit dobu trvání podle potřeby|
|Modul runtime koncový bod souběžných příchozí volání |~1,000|Snižte počet souběžných požadavků nebo zkrátit dobu trvání podle potřeby|
|Modul runtime koncový bod čtení volání za 5 minut |60,000|Můžete rozdělit zatížení mezi více aplikacemi podle potřeby|
|Koncový bod runtime vyvolat volání za 5 minut |45,000|Můžete rozdělit zatížení mezi více aplikacemi podle potřeby|

Pokud budete chtít překročí toto omezení v normálním zpracování nebo chcete spustit testování zatížení, které může být delší než tento limit pro určitou dobu, [kontaktujte nás](mailto://logicappsemail@microsoft.com) tak, aby bylo možné pomoci vašim požadavkům.

### <a name="definition-limits"></a>Definice omezení

Toto jsou omezení pro definici aplikace logiky jeden.

|Name (Název)|Omezení|Poznámky|
|----|----|----|
|Akce za pracovního postupu|500|Vnořené pracovní postupy a rozšířit tento limit, podle potřeby můžete přidat|
|Povolené akce hloubky vnoření|8|Vnořené pracovní postupy a rozšířit tento limit, podle potřeby můžete přidat|
|Pracovní postupy na oblast na předplatné|1000||
|Aktivační události za pracovního postupu|10||
|Přepínač oboru případech limit|25||
|Počet proměnných za pracovního postupu|250||
|Maximální počet znaků na jednu výraz|8 192||
|Maximální počet `trackedProperties` velikost ve znacích|16,000|
|`action`/`trigger`Název omezení|80||
|`description`omezení délky|256||
|`parameters`limit|50||
|`outputs`limit|10||

### <a name="integration-account-limits"></a>Limity účtu integrace

Toto jsou omezení pro artefakty přidány do integrace účtu

|Name (Název)|Omezení|Poznámky|
|----|----|----|
|Schéma|8 MB|Můžete použít [identifikátor URI objektu blob](logic-apps-enterprise-integration-schemas.md) odeslat soubory větší než 2 MB |
|Mapu (soubor XSLT)|2 MB| |
|Modul runtime koncový bod čtení volání za 5 minut |60,000|Můžete rozdělit zatížení mezi více účtů podle potřeby|
|Koncový bod runtime vyvolat volání za 5 minut |45,000|Můžete rozdělit zatížení mezi více účtů podle potřeby|
|Koncový bod runtime sledování volání za 5 minut |45,000|Můžete rozdělit zatížení mezi více účtů podle potřeby|
|Koncový bod runtime blokování souběžných volání |~1,000|Snižte počet souběžných požadavků nebo zkrátit dobu trvání podle potřeby|

### <a name="b2b-protocols-as2-x12-edifact-message-size"></a>Velikost zprávy protokoly B2B (AS2, X12, EDIFACT)

Toto jsou limity pro protokoly B2B

|Name (Název)|Omezení|Poznámky|
|----|----|----|
|AS2|50 MB|Použít kódování a dekódování|
|X12|50 MB|Použít kódování a dekódování|
|EDIFACT|50 MB|Použít kódování a dekódování|

## <a name="configuration"></a>Konfigurace

### <a name="ip-address"></a>IP adresa

#### <a name="logic-app-service"></a>Služba Logic App Service

Volání z aplikace logiky přímo (který je prostřednictvím [HTTP](../connectors/connectors-native-http.md) nebo [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)) nebo jiné požadavky HTTP, které pocházejí z zadaná IP adresa v následujícím seznamu:

|Oblasti aplikace logiky|Odchozí IP|
|-----|----|
|Austrálie – východ|13.75.153.66, 104.210.89.222, 104.210.89.244, 13.75.149.4, 104.210.91.55, 104.210.90.241|
|Austrálie – jihovýchod|13.73.115.153, 40.115.78.70, 40.115.78.237, 13.73.114.207, 13.77.3.139, 13.70.159.205|
|Brazílie – jih|191.235.86.199, 191.235.95.229, 191.235.94.220, 191.235.82.221, 191.235.91.7, 191.234.182.26|
|Střední Kanada|52.233.29.92, 52.228.39.241, 52.228.39.244|
|Východní Kanada|52.232.128.155, 52.229.120.45, 52.229.126.25|
|Střed Indie|52.172.157.194, 52.172.184.192, 52.172.191.194, 52.172.154.168, 52.172.186.159, 52.172.185.79|
|Střed USA|13.67.236.76, 40.77.111.254, 40.77.31.87, 13.67.236.125, 104.208.25.27, 40.122.170.198|
|Východní Asie|168.63.200.173, 13.75.89.159, 23.97.68.172, 13.75.94.173, 40.83.127.19, 52.175.33.254|
|Východ USA|137.135.106.54, 40.117.99.79, 40.117.100.228, 13.92.98.111, 40.121.91.41, 40.114.82.191|
|Východní USA 2|40.84.25.234, 40.79.44.7, 40.84.59.136, 40.84.30.147, 104.208.155.200, 104.208.158.174|
|Japonsko – východ|13.71.146.140, 13.78.84.187, 13.78.62.130, 13.71.158.3, 13.73.4.207, 13.71.158.120|
|Japonsko – západ|40.74.140.173, 40.74.81.13, 40.74.85.215, 40.74.140.4, 104.214.137.243, 138.91.26.45|
|Střed USA – sever|168.62.249.81, 157.56.12.202, 65.52.211.164, 168.62.248.37, 157.55.210.61, 157.55.212.238|
|Severní Evropa|13.79.173.49, 52.169.218.253, 52.169.220.174, 40.113.12.95, 52.178.165.215, 52.178.166.21|
|Střed USA – jih|13.65.98.39, 13.84.41.46, 13.84.43.45, 104.210.144.48, 13.65.82.17, 13.66.52.232|
|Jihovýchodní Asie|52.163.93.214, 52.187.65.81, 52.187.65.155, 13.76.133.155, 52.163.228.93, 52.163.230.166|
|Indie – jih|52.172.9.47, 52.172.49.43, 52.172.51.140, 52.172.50.24, 52.172.55.231, 52.172.52.0|
|Západní Evropa|13.95.155.53, 52.174.54.218, 52.174.49.6, 40.68.222.65, 40.68.209.23, 13.95.147.65|
|Indie – západ|104.211.164.112, 104.211.165.81, 104.211.164.25, 104.211.164.80, 104.211.162.205, 104.211.164.136|
|Západní USA|52.160.90.237, 138.91.188.137, 13.91.252.184, 52.160.92.112, 40.118.244.241, 40.118.241.243|
|Spojené království – jih|51.140.74.14, 51.140.73.85, 51.140.78.44|
|Spojené království – západ|51.141.54.185, 51.141.45.238, 51.141.47.136|

#### <a name="connectors"></a>Konektory

Volání z [konektor](../connectors/apis-list.md) pocházet z zadaná IP adresa v následujícím seznamu:

|Oblasti aplikace logiky|Odchozí IP|
|-----|----|
|Austrálie – východ|40.126.251.213|
|Austrálie – jihovýchod|40.127.80.34|
|Brazílie – jih|191.232.38.129|
|Střední Kanada|52.233.31.197, 52.228.42.205, 52.228.33.76, 52.228.34.13|
|Východní Kanada|52.229.123.98, 52.229.120.178, 52.229.126.202, 52.229.120.52|
|Střed Indie|104.211.98.164|
|Střed USA|40.122.49.51|
|Východní Asie|23.99.116.181|
|Východ USA|191.237.41.52|
|Východní USA 2|104.208.233.100|
|Japonsko – východ|40.115.186.96|
|Japonsko – západ|40.74.130.77|
|Střed USA – sever|65.52.218.230|
|Severní Evropa|104.45.93.9|
|Střed USA – jih|104.214.70.191|
|Jihovýchodní Asie|13.76.231.68|
|Indie – jih|104.211.227.225|
|Západní Evropa|40.115.50.13|
|Indie – západ|104.211.161.203|
|Západní USA|104.40.51.248|
|Spojené království – jih|51.140.80.51|
|Spojené království – západ|51.141.47.105|


## <a name="next-steps"></a>Další kroky  

- Chcete-li začít s Logic Apps, postupujte [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) kurzu.  
- [Zobrazení běžných příkladů a scénářů](../logic-apps/logic-apps-examples-and-scenarios.md).
- [S Logic Apps můžete automatizovat firemní procesy](http://channel9.msdn.com/Events/Build/2016/T694). 
- [Zjistěte, jak integrovat své systémy s Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462).
