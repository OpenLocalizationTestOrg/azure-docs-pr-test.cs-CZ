---
title: "Přehled služby Media Services aaaAzure | Microsoft Docs"
description: "Toto téma nabízí přehled Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7a5e9723-c379-446b-b4d6-d0e41bd7d31f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/04/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 81f9f4d9ff75effea30c10fd09449e9d2025f377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-overview"></a>Přehled služby Azure Media Services 

Microsoft Azure Media Services je rozšiřitelná platforma založená na cloudu, která umožňuje vývojářům toobuild škálovatelné média správu a doručování aplikacím. Služba Media Services je založena na rozhraní REST API, která umožňují toosecurely nahrávání, ukládání, kódování a video nebo zvuk obsah balíčku pro vysílání na vyžádání i živé streamování doručení toovarious klienty (například TV, počítače a mobilní zařízení).

Pomocí Media Services můžete vytvářet pracovní postupy od začátku až do konce. Můžete také toouse komponenty třetích stran pro některé části pracovního postupu. Můžete například kódovat pomocí kodéru třetí strany. Potom obsah nahrajete, zabezpečíte, zabalíte a doručíte pomocí služby Media Services.

Můžete vybrat toostream obsah za provozu nebo doručování obsahu na vyžádání. Hello téma obsahuje i odkazy tooother související témata.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
Mapu kurzů k AMS můžete zobrazit tady:

* [Pracovní postup živého streamování AMS](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [Pracovní postup streamování AMS na vyžádání](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a>Požadavky

toostart využívající Azure Media Services, měli byste mít hello následující:

* Účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com).
* Účet Azure Media Services. Další informace najdete v článku o [vytvoření účtu](media-services-portal-create-account.md).
* (Volitelné) Nastavte vývojové prostředí. Jako vývojové prostředí si zvolte .NET nebo REST API. Další informace najdete v článku o [nastavení prostředí](media-services-dotnet-how-to-use.md).

    Také zjistěte, jak příliš[připojení prostřednictvím kódu programu tooAMS rozhraní API](media-services-use-aad-auth-to-access-ams-api.md).
* Koncový bod streamování Standard nebo Premium ve spuštěném stavu.  Další informace najdete v článku o [správě koncových bodů streamování](media-services-portal-manage-streaming-endpoints.md).

## <a name="sdks-and-tools"></a>Sady SDK a nástroje

toobuild řešení Media Services, můžete:

* [Rozhraní REST API služby Media Services](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* Jeden z dostupných klientů hello sady SDK:
    * [Azure Media Services SDK pro .NET](https://github.com/Azure/azure-sdk-for-media-services),
    * [Azure SDK pro Javu](https://github.com/Azure/azure-sdk-for-java),
    * [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),
    * [Azure Media Services pro Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Jedná se o verzi sady SDK, kterou nevytvořil Microsoft. Ho je možný díky komunita a aktuálně nemá 100 % pokrytí rozhraní API pro AMS hello).
* Existující nástroje:
    * [Azure Portal](https://portal.azure.com/)
    * [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) je aplikace napsaná v jazyce Winforms/C# pro Windows.)

## <a name="concepts-and-overview"></a>Koncepty a přehled
Informace o konceptech Azure Media Services najdete v článku [Koncepty](media-services-concepts.md).

Jak – tooseries vás seznámí s tooall hello hlavními součástmi Azure Media Services, najdete v části [Azure Media Services podrobných kurzech](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series). Tato návody obsahují skvělý Přehled konceptů a používá úlohy toodemonstrate AMS nástroj AMSE hello. Nástroj AMSE je nástrojem systému Windows. Tento nástroj podporuje většinu úloh hello můžete dosáhnout prostřednictvím kódu programu s [AMS SDK pro .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK pro jazyk Java](https://github.com/Azure/azure-sdk-for-java), nebo [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a>Podporované scénáře a dostupnost služby Media Services v datových centrech

Podrobné informace najdete v tématu [Scénáře a dostupnost funkcí a služeb AMS v datových centrech](scenarios-and-availability.md).

## <a name="service-level-agreement-sla"></a>Smlouva SLA

* V případě služby Media Services Encoding zaručujeme 99,9% dostupnosti transakcí REST API.
* V případě streamování budeme úspěšně obsluhovat požadavky se zárukou 99,9% dostupnosti pro existující mediální obsah, pokud jste zakoupili koncový bod streamování Standard nebo Premium.
* Živých kanálů Zaručujeme, že spuštění kanály bude mít externí konektivitu minimálně 99,9 % času hello.
* V případě Content Protection Zaručujeme, že jsme bude úspěšné plnění klíčových požadavků minimálně 99,9 % času hello.
* Pro Indexer, úspěšně obsluhujeme požadavky úloh indexeru zpracovat rezervovaná pro kódování jednotky 99,9 % času hello.

Další informace najdete v článku [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).

Informace o dostupnosti v datových centrech najdete v tématu hello [Avaiability](scenarios-and-availability.md#availability) části.

## <a name="support"></a>Podpora

[Podpora Azure](https://azure.microsoft.com/support/options/) nabízí možnosti odborné pomoci pro Azure včetně služby Media Services.

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
