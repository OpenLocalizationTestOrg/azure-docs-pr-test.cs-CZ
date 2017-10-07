---
title: "kódy chyb aaaAzure Media Services | Microsoft Docs"
description: "Hello téma nabízí přehled kódů chyb Azure Media Services."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: d3a62a64-7608-4b17-8667-479b26ba0d6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: de1ffd6dee8901a3051eb5032536c3669482d6b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-error-codes"></a>Kódy chyb Azure Media Services
Při použití Microsoft Azure Media Services, může se zobrazit kódy chyb protokolu HTTP z hello služby v závislosti na problémy, jako je například ověřování tokenů vypršení platnosti tooactions, které nejsou podporovány ve službě Media Services. Hello tady je seznam **kódy chyb HTTP** , mohou být vráceny Media Services a hello možné způsobí, že pro ně.  

## <a name="400-bad-request"></a>400 – Chybný požadavek
žádost o Hello obsahuje neplatné informace a se odmítne kvůli tooone hello následujících důvodů:

* Je zadána Nepodporovaná verze rozhraní API. Nejaktuálnější verzi hello, najdete v části [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).
* není určená verze rozhraní API Hello Media Services. Informace o tom, jak toospecify hello verze rozhraní API najdete v tématu [média referenční dokumentace rozhraní API REST služby Operations](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).
  
  > [!NOTE]
  > Pokud používáte tooconnect tooMedia hello .NET nebo sady Java SDK služby, hello verze rozhraní API je zadán pro vás vždy, když akci a provedení několika akcí proti Media Services.
  > 
  > 
* Nedefinovaná vlastnost nebyla zadána. Název vlastnosti Hello je v chybové zprávě hello. Lze zadat pouze vlastnosti, které jsou členy dané entity. V tématu [Azure Media Services REST API – referenční informace](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) seznam entity a jejich vlastnosti.
* Byla zadána neplatnou hodnotu vlastnosti. Název vlastnosti Hello je v chybové zprávě hello. Zobrazit hello předchozí odkaz pro typy platný vlastností a jejich hodnoty.
* Hodnota vlastnosti je chybějící a musí se.
* Součást hello adresa URL obsahuje neplatná hodnota.
* Pokus o byl proveden tooupdate WriteOnce vlastnost.
* Pokus o byl proveden toocreate úlohu, která má vstupní Asset primární AssetFile, který nebyl zadán nebo nebylo možné určit.
* Pokus o byl proveden tooupdate lokátoru SAS. Lokátory SAS můžou jenom vytvořit nebo odstranit. Lokátory streamování lze aktualizovat. Další informace najdete v tématu [lokátory](https://docs.microsoft.com/rest/api/media/operations/locator).
* Nepodporovanou operaci nebo dotaz byla odeslána.

## <a name="401-unauthorized"></a>401 unauthorized
Hello žádost nešlo ověřit (dříve, než lze udělit oprávnění) kvůli tooone hello následujících důvodů:

* Chybí záhlaví ověřování.
* Hodnota hlavičky chybný ověřování.
  * vypršela platnost tokenu Hello. 
  * Hello token obsahuje neplatný podpis.

## <a name="403-forbidden"></a>403 Zakázáno
Hello žádost není povolena kvůli tooone hello následujících důvodů:

* Hello účtu Media Services nebyl nalezen nebo je odstraněný.
* je zakázané Hello účtu Media Services a není typu hello požadavku HTTP GET. Operace služby vrátí 403 odpověď.
* Hello ověřovací token neobsahuje informace o pověření uživatelů hello: název účtu nebo ID předplatného. Tyto informace najdete v hello rozšíření Media Services uživatelského rozhraní pro váš účet Media Services v hello portálu pro správu Azure.
* Hello prostředků není přístupný.
  
  * Pokus o byl proveden toouse MediaProcessor, který není k dispozici pro váš účet Media Services.
  * Byl proveden pokus o tooupdate JobTemplate definované službou Media Services.
  * Pokus o byl provedené toooverwrite některé jiné účtu Media Services je lokátoru.
  * Pokus o byl provedené toooverwrite ContentKey některé jiné Media Services účtu.
* Nelze vytvořit prostředek Hello kvůli tooa služby kvótu, kterou bylo dosaženo pro účet Media Services hello. Další informace o hello služby kvóty, najdete v části [kvóty a omezení](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 – Nenalezeno
Hello požadavku není povolená na prostředku kvůli tooone hello následujících důvodů:

* Pokus o byl proveden tooupdate entita, která neexistuje.
* Pokus o byl proveden toodelete entita, která neexistuje.
* Pokus o byl proveden toocreate entita, která propojí tooan entity, která neexistuje.
* Pokus o byl proveden tooGET entita, která neexistuje.
* Pokus o byl proveden toospecify účet úložiště, který není spojen s hello účtu Media Services.  

## <a name="409-conflict"></a>409 – konflikt
Hello žádost není povolena kvůli tooone hello následujících důvodů:

* Více než jeden AssetFile má zadaný název hello v rámci hello Asset.
* Byl proveden pokus toocreate a sekundu primární AssetFile v rámci hello Asset.
* Byl proveden pokus o toocreate ContentKey s hello zadané Id již používá.
* Byl proveden pokus o toocreate lokátoru s hello zadané Id již používá.
* Více než jeden IngestManifestFile má zadaný název hello v rámci hello IngestManifest.
* Pokus o byl proveden toolink druhý toohello ContentKey šifrování úložiště Asset šifrovat úložiště.
* Byl proveden pokus o toolink hello stejné ContentKey toohello Asset.
* Byl proveden toocreate Lokátor tooan Asset jejichž kontejner úložiště chybí nebo je už přidružený hello Asset.
* Pokus o byl proveden toocreate Lokátor tooan Asset, který již má 5 lokátory používá. (Úložiště azure vynucuje omezení hello pět zásady sdíleného přístupu na jeden kontejner úložiště.)
* Propojení účtu úložiště Asset tooan IngestManifestAsset není hello stejný jako účet úložiště hello používá nadřazené hello IngestManifest.  

## <a name="500-internal-server-error"></a>500 vnitřní chybu serveru
Během zpracování požadavku hello hello zaznamená Media Services nějaká chyba, která zabraňuje hello zpracování pokračovat. To může být kvůli tooone hello následujících důvodů:

* Vytvoření úlohy nebo Asset nezdaří, protože informace o kvótě účtu Media Services hello služba je dočasně nedostupná.
* Vytvoření prostředku nebo IngestManifest kontejner úložiště objektů blob se nezdaří, protože informace o účtu úložiště hello účet je dočasně nedostupná.
* Další neočekávané chyby.

## <a name="503-service-unavailable"></a>503 Služba nedostupná
Hello server je momentálně nemůže tooreceive požadavky. Tato chyba může být způsobeno službou toohello nadměrné požadavky. Služba Media Services omezení mechanismus omezuje hello využití prostředků pro aplikace, které služby toohello nadměrné požadavku.

> [!NOTE]
> Kontrola hello chyba zprávu a chyba kód řetězec tooget podrobnější informace o hello důvod, které jste získali hello 503 chyby. Tato chyba vždy neznamená omezení.
> 
> 

Možné stav popisy jsou:

* "Server je zaneprázdněn. Předchozí spustí tento typ požadavku trvalo víc než {0} sekund."
* "Server je zaneprázdněn. Více než {0} požadavků za sekundu může být omezena."
* "Server je zaneprázdněn. Více než {0} požadavky během několika sekund {1} může být omezena."

toohandle této chybě, doporučujeme použít logiku exponenciální opakování back vypnout. To znamená, pomocí progresivně delší čeká mezi jednotlivými pokusy o po sobě jdoucích chybové odpovědi.  Další informace najdete v tématu [přechodné chyby zpracování bloku aplikace](https://msdn.microsoft.com/library/hh680905.aspx).

> [!NOTE]
> Pokud používáte [Azure Media Services SDK pro .net](https://github.com/Azure/azure-sdk-for-media-services/tree/master), byl implementován hello logika opakovaných pokusů pro chybu hello 503 ve hello SDK.  
> 
> 

## <a name="see-also"></a>Viz také
[Kódy chyb správy služby Media Services](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Další kroky
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

