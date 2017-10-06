---
title: "Přehled Azure IoT Suite aaaMicrosoft | Microsoft Docs"
description: "Přehled o tom, jak Azure IoT Suite nabízí internet věcí toocollect předkonfigurovaných řešení, analyzovat a ukládání dat, vizualizacím a integraci s jinými systémy."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 2d38d08a-4133-4e5c-8b28-f93cadb5df05
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 385025c5ec0d37c74689a928bc09e85b33439634
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-iot-suite"></a>Přehled sady Azure IoT Suite

Hello Azure pro internet věcí (IoT) services nabízí celou řadu funkcí. Tyto služby na úrovni řešení pro velké firmy umožňují:

* sběr dat ze zařízení
* analýzu datových proudů za provozu
* ukládání objemných datových sad a dotazy na ně
* vizualizaci historických dat i dat v reálném čase
* integraci se systémy administrativní podpory (back-office)
* správu zařízení

toodeliver tyto funkce Azure IoT Suite balíčky současně více služeb Azure s vlastními rozšířeními v podobě *předkonfigurovaných řešení*. Tato předkonfigurovaná řešení jsou základní implementací běžných vzorů řešení IoT, které pomáhají tooreduce hello čas trvat toodeliver řešení IoT. Pomocí hello [IoT software development Kit][lnk-sdks], můžete přizpůsobit a rozšířit těchto řešení toomeet vlastní požadavky. Tato řešení můžete využít i jako příklady či šablony při vývoji nových řešení IoT.

Hello následující video obsahuje úvod tooAzure IoT Suite:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON309/player]
> 
> 

## <a name="azure-iot-services-in-azure-iot-suite"></a>Služby Azure IoT v Azure IoT Suite
Hello předkonfigurované řešení obvykle využívají hello následující služby:

* Základní tooAzure IoT Suite je hello [Azure IoT Hub] [ lnk-iot-hub] služby. Tato služba poskytuje hello zařízení cloud a funkce pro zasílání zpráv typu cloud zařízení a jednání jako hello brány toohello cloudu a hello jiných klíčovým službám IoT Suite. Hello služba vám umožní tooreceive zpráv ze zařízení v měřítku a odesílat příkazy tooyour zařízení. Hello služba vám také umožní příliš[spravovat vaše zařízení][lnk-device-management]. Můžete například nakonfigurovat, restartovat nebo obnovit tovární nastavení na jeden nebo více zařízení připojených toohello rozbočovače.
* Služba [Azure Stream Analytics][lnk-asa] umožňuje analyzovat data za provozu. IoT Suite využívá tuto službu tooprocess příchozí telemetrii, vytváření agregací a zjišťování událostí. Hello předkonfigurovaná řešení dále s využitím stream analytics tooprocess informační zprávy, které obsahují data, jako je například metadata nebo odezvy ze zařízení. Hello řešení pomocí služby Stream Analytics tooprocess hello zpráv ze zařízení a přenosu těchto zpráv tooother služby.
* [Úložiště Azure] [ lnk-azure-storage] a [Azure Cosmos DB] [ lnk-document-db] poskytují možnosti úložiště dat hello. Hello předkonfigurovaná řešení využívají telemetrie toostore úložiště objektů blob a toomake je k dispozici pro analýzu. Hello řešení pomocí metadat zařízení toostore Cosmos DB a povolit možnosti správy zařízení hello hello řešení.
* [Azure Web Apps] [ lnk-web-apps] a [Microsoft Power BI] [ lnk-power-bi] poskytovat funkce pro vizualizaci dat hello. Hello flexibilitu Power BI vám umožní tooquickly sestavení vlastní interaktivní řídicí panely, které používají data IoT Suite.

Přehled hello architektura typického řešení IoT najdete v tématu [Microsoft Azure a Internet věcí (IoT) hello][iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>Předkonfigurovaná řešení

Sada IoT Suite zahrnuje předkonfigurovaná řešení tohoto povolení tooquickly můžete začít pracovat s a tooexplore hello běžným scénářům IoT, jako například:

* Vzdálené monitorování
* Prediktivní údržba
* Propojená továrna

Můžete nasadit tyto řešení tooyour předplatného Azure a pak spusťte scénář IoT dokončení, začátku do konce.

## <a name="next-steps"></a>Další kroky

Teď, když máte přehled co IoT Suite dělat a jaké jsou jeho hlavní součásti, můžete další informace o hello předkonfigurované řešení IoT Suite. Další informace najdete v tématu [co jsou hello Azure IoT předkonfigurovaných řešení?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
[lnk-device-management]: ../iot-hub/iot-hub-device-management-overview.md
