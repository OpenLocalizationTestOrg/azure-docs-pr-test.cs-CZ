---
title: aaaRetry logiku hello sady Media Services SDK pro .NET | Microsoft Docs
description: "Hello téma nabízí přehled logika opakovaných pokusů v hello sady Media Services SDK pro .NET."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 527b61a6-c862-4bd8-bcbc-b9aea1ffdee3
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: juliako
ms.openlocfilehash: 18d0a9d68e55a48bc769fb6ae5711ddba78ed8e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="retry-logic-in-hello-media-services-sdk-for-net"></a>Opakujte logiku hello sady Media Services SDK pro .NET
Při práci se službou Microsoft Azure, může dojít přechodných. Pokud dojde k přechodná chyba, ve většině případů po několika pokusech hello operace úspěšná. Hello sady Media Services SDK pro .NET implementuje hello opakování logiku toohandle přechodné chyby související s výjimek a chyb, které jsou způsobeny webových požadavků, provádění dotazy, uložení změn a operace úložiště.  Ve výchozím nastavení spustí hello sady Media Services SDK pro .NET do čtyř opakovaných vyhledávání před znovu vyvolání hello výjimka tooyour aplikace. Hello kód ve vaší aplikaci pak musí správně zpracovat výjimku.  

 Hello Následuje stručný platí webovému požadavku, úložiště, dotazů a SaveChanges zásad:  

* Hello úložiště zásad se používá pro operace úložiště objektů blob (nahrávání nebo stahování souborů asset).  
* Hello zásady webového požadavku se používá pro obecné webových požadavků (například pro získání tokenu ověřování a řešení, že uživatelé hello clusteru koncového bodu).  
* Hello dotazu zásad se používá k dotazování entity z REST (například mediaContext.Assets.Where(...)).  
* Hello SaveChanges zásad se používá k provádění všechno, co změny dat v rámci služby hello (například vytváření entity aktualizaci entity, volání funkce služby pro operace).  
  
  Toto téma uvádí typy výjimek a chybové kódy, které jsou zpracovávány hello sady Media Services SDK pro .NET logika opakovaných pokusů.  

## <a name="exception-types"></a>Typy výjimek
Hello následující tabulka popisuje tento hello sady Media Services SDK pro .NET obslužné rutiny výjimky nebo nezpracovává pro některé operace, které může způsobit přechodné chyby.  

| Výjimka | Webové žádosti | Úložiště | Dotaz | SaveChanges |
| --- | --- | --- | --- | --- |
| Výjimku WebException<br/>Další informace najdete v tématu hello [výjimku WebException stavové kódy](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) části. |Ano |Ano |Ano |Ano |
| DataServiceClientException<br/> Další informace najdete v tématu [stavové kódy HTTP Chyba](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Ne |Ano |Ano |Ano |
| DataServiceQueryException<br/> Další informace najdete v tématu [stavové kódy HTTP Chyba](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Ne |Ano |Ano |Ano |
| DataServiceRequestException<br/> Další informace najdete v tématu [stavové kódy HTTP Chyba](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Ne |Ano |Ano |Ano |
| DataServiceTransportException |Ne |Ne |Ano |Ano |
| TimeoutException |Ano |Ano |Ano |Ne |
| SocketException |Ano |Ano |Ano |Ano |
| StorageException |Ne |Ano |Ne |Ne |
| Výjimka vstupu/výstupu |Ne |Ano |Ne |Ne |

### <a name="WebExceptionStatus"></a>Výjimku WebException stavové kódy
Hello následující tabulka uvádí pro výjimku WebException chybu, která je implementovaná logika opakovaných pokusů hello kódy. Hello [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) výčet definuje hello stavové kódy.  

| Status | Webové žádosti | Úložiště | Dotaz | SaveChanges |
| --- | --- | --- | --- | --- |
| ConnectFailure |Ano |Ano |Ano |Ano |
| NameResolutionFailure |Ano |Ano |Ano |Ano |
| ProxyNameResolutionFailure |Ano |Ano |Ano |Ano |
| SendFailure |Ano |Ano |Ano |Ano |
| PipelineFailure |Ano |Ano |Ano |Ne |
| ConnectionClosed |Ano |Ano |Ano |Ne |
| KeepAliveFailure |Ano |Ano |Ano |Ne |
| Neznámé chyby |Ano |Ano |Ano |Ne |
| ReceiveFailure |Ano |Ano |Ano |Ne |
| RequestCanceled |Ano |Ano |Ano |Ne |
| Časový limit |Ano |Ano |Ano |Ne |
| Požadavku <br/>zpracování kód stavu HTTP hello řídí Hello opakování v požadavku. Další informace najdete v tématu [stavové kódy HTTP Chyba](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Ano |Ano |Ano |Ano |

### <a name="HTTPStatusCode"></a>Stavové kódy chyb HTTP
Když operace dotazů a SaveChanges throw DataServiceClientException, DataServiceQueryException nebo DataServiceQueryException, hello stavový kód chyby HTTP se vrátí v hello StatusCode vlastnost.  Hello následující tabulka ukazuje pro kódy chyb logika opakovaných pokusů hello je implementována.  

| Status | Webové žádosti | Úložiště | Dotaz | SaveChanges |
| --- | --- | --- | --- | --- |
| 401 |Ne |Ano |Ne |Ne |
| 403 |Ne |Ano<br/>Zpracování opakování s delší čeká. |Ne |Ne |
| 408 |Ano |Ano |Ano |Ano |
| 429 |Ano |Ano |Ano |Ano |
| 500 |Ano |Ano |Ano |Ne |
| 502 |Ano |Ano |Ano |Ne |
| 503 |Ano |Ano |Ano |Ano |
| 504 |Ano |Ano |Ano |Ne |

Pokud chcete tootake podívejte se na hello skutečné použití hello sady Media Services SDK pro .NET logika opakovaných pokusů, najdete v části [azure-sdk pro media-services](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Další kroky
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

