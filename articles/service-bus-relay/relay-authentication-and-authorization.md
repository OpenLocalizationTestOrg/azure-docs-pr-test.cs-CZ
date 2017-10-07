---
title: "aaaAzure předávání ověřování a autorizace | Microsoft Docs"
description: "Přehled sdíleného přístupového podpisu (SAS) ověřování v Azure předávání"
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: sethm
ms.openlocfilehash: b27914672ce968da2bddba8dafc5683ebf3834ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-relay-authentication-and-authorization"></a>Azure předávací ověřování a autorizace
Aplikace může ověřit tooAzure předávání pomocí sdíleného přístupového podpisu (SAS) ověřování. Podobně jako příliš[zasílání zpráv Service Bus](../service-bus-messaging/service-bus-authentication-and-authorization.md), SAS ověření umožňuje toohello tooauthenticate aplikace Azure předávací službou vytvoříte pomocí přístupový klíč nakonfigurovaný na předávání hello oboru názvů. Pak můžete použít tento klíč toogenerate token sdíleného přístupového podpisu, který můžou klienti použít tooauthenticate toohello předávací služba.

## <a name="shared-access-signature-authentication"></a>Ověření pomocí sdíleného přístupového podpisu
[Ověřování SAS](../service-bus-messaging/service-bus-sas.md) umožňuje vám toogrant uživatel přístup k tooAzure předávání prostředkům s konkrétní práva. SAS ověřování zahrnuje konfiguraci hello kryptografického klíče s přidružená práva na prostředku. Klienti mohou získat přístup toothat prostředků pak prezentací token SAS, který se skládá z identifikátoru URI přistupuje hello prostředku a vypršela platnost podepsané hello nakonfigurovaný klíč.

Klíče pro SAS můžete nakonfigurovat na předávání názvů. Na rozdíl od zasílání zpráv Service Bus, [předávání hybridní připojení](relay-hybrid-connections-protocol.md) podporuje odesílatelé neoprávněným nebo anonymní. Anonymní přístup pro entitu hello můžete povolit při vytváření, jak ukazuje následující snímek z portálu hello obrazovky hello:

![][0]

toouse SAS, můžete nakonfigurovat [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objekt na předávání obor názvů, který se skládá z následujících hello:

* *KeyName* identifikující hello pravidlo.
* *PrimaryKey* kryptografický klíč používá tokeny SAS toosign nebo ověření.
* *Sekundární klíč* kryptografický klíč používá tokeny SAS toosign nebo ověření.
* *Práva* představující hello kolekce naslouchání, odesílat, nebo spravovat udělena oprávnění.

Autorizační pravidla nakonfigurována na úrovni oboru názvů hello můžete udělit přístup tooall předávání připojení v oboru názvů pro klienty s tokeny podepsaná pomocí hello odpovídající klíč. Až too12 takové autorizační pravidla lze nastavit u oboru názvů předávání. Ve výchozím nastavení [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) se všemi právy je nakonfigurován pro každý obor názvů při první je zřízený.

tooaccess entity hello klient vyžaduje token SAS, který je generována pomocí konkrétní [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Hello SAS token je generována pomocí hello je požadována HMAC SHA256 řetězce prostředků, který se skládá z prostředků hello URI toowhich přístupu a vypršení platnosti se kryptografickým klíčem přidružené hello autorizační pravidlo.

Podpora ověřování SAS pro předávání přes Azure je zahrnuty v hello Azure .NET SDK verze 2.0 a vyšší. Zahrnuje podporu pro SAS [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Všechna rozhraní API, které přijímají připojovací řetězec jako parametr zahrnují podporu pro SAS připojovací řetězce.

## <a name="next-steps"></a>Další kroky
- Materiály [ověření sběrnice s podpisy sdíleného přístupu](../service-bus-messaging/service-bus-sas.md) další podrobnosti o tokenu SAS.
- V tématu hello [Azure předávání hybridní připojení protokolu Průvodce](relay-hybrid-connections-protocol.md) podrobné informace o hello schopností hybridní připojení.
- Odpovídající informace o zasílání zpráv Service Bus ověřování a autorizace najdete v tématu [Service Bus ověřování a autorizace](../service-bus-messaging/service-bus-authentication-and-authorization.md). 

[0]: ./media/relay-authentication-and-authorization/hcanon.png