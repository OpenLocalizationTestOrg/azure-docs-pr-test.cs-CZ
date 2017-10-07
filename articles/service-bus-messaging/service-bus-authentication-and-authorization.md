---
title: "aaaAzure Service Bus ověřování a autorizace | Microsoft Docs"
description: "Ověření aplikace tooService Bus pomocí sdíleného přístupového podpisu (SAS) ověřování."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 18bad0ed-1cee-4a5c-a377-facc4785c8c9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2017
ms.author: sethm
ms.openlocfilehash: 898a2144c99d8fac074b6d85604f710bf512e71e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-authentication-and-authorization"></a>Ověřování a autorizace Service Bus

Aplikace může ověřit tooAzure Service Bus pomocí sdíleného přístupového podpisu (SAS) ověřování. Sdílený přístupový podpis ověřování umožňuje aplikacím tooauthenticate tooService sběrnice pomocí přístupový klíč nakonfigurované na oboru názvů hello nebo na hello entit, ke které jsou přidružené konkrétní práva. Pak můžete použít tento klíč toogenerate token sdíleného přístupového podpisu, který můžou klienti použít tooauthenticate tooService sběrnice.

> [!IMPORTANT]
> Měli byste použít SAS místo Azure Active Directory řízení přístupu (také označované jako služby Řízení přístupu nebo služby ACS), jak služby ACS je zastaralá. SAS poskytuje jednoduchý, flexibilní a snadné použití ověřování schématu pro Service Bus. Aplikace můžete použít SAS ve scénářích, ve kterých nepotřebují toomanage hello představu o oprávněný "uživatel." Další informace najdete v tématu [tomto příspěvku na blogu](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/).

## <a name="shared-access-signature-authentication"></a>Ověření pomocí sdíleného přístupového podpisu

[Ověřování SAS](service-bus-sas.md) umožňuje vám toogrant uživatel přístup k tooService sběrnice prostředkům s konkrétní práva. Ověřování SAS v Service Bus zahrnuje konfiguraci hello kryptografického klíče s přidružená práva na prostředku služby Service Bus. Klienti mohou získat přístup toothat prostředků pak prezentací token SAS, který se skládá z identifikátoru URI přistupuje hello prostředku a vypršela platnost podepsané hello nakonfigurovaný klíč.

Klíče pro SAS můžete konfigurovat v oboru názvů Service Bus. klíč Hello platí tooall entit pro zasílání zpráv v daném oboru názvů. Můžete také nakonfigurovat klíče na témata a fronty Service Bus. SAS je podporováno také v [předávání přes Azure](../service-bus-relay/relay-authentication-and-authorization.md).

toouse SAS, můžete nakonfigurovat [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objekt v oboru názvů, fronta nebo téma. Toto pravidlo se skládá z hello následující prvky:

* *KeyName* identifikující hello pravidlo.
* *PrimaryKey* kryptografický klíč používá tokeny SAS toosign nebo ověření.
* *Sekundární klíč* kryptografický klíč používá tokeny SAS toosign nebo ověření.
* *Práva* představující hello kolekce naslouchání, odesílat, nebo spravovat udělena oprávnění.

Autorizační pravidla nakonfigurována na úrovni oboru názvů hello můžete udělit přístup tooall entity v oboru názvů pro klienty s tokeny podepsaná pomocí hello odpovídající klíč. Až too12 takové autorizační pravidla lze nastavit v oboru názvů Service Bus, fronta nebo téma. Ve výchozím nastavení [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) se všemi právy je nakonfigurován pro každý obor názvů při první je zřízený.

tooaccess entity hello klient vyžaduje token SAS, který je generována pomocí konkrétní [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Hello SAS token je generována pomocí hello je požadována HMAC SHA256 řetězce prostředků, který se skládá z prostředků hello URI toowhich přístupu a vypršení platnosti se kryptografickým klíčem přidružené hello autorizační pravidlo.

Podpora ověřování SAS pro Service Bus je zahrnuty v hello Azure .NET SDK verze 2.0 a vyšší. Zahrnuje podporu pro SAS [SharedAccessAuthorizationRule](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Všechna rozhraní API, které přijímají připojovací řetězec jako parametr zahrnují podporu pro SAS připojovací řetězce.

## <a name="next-steps"></a>Další kroky

- Materiály [ověření sběrnice s podpisy sdíleného přístupu](service-bus-sas.md) další podrobnosti o tokenu SAS.
- [Změní tooACS povoleno obory názvů.](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/)
- Odpovídající informace o předávání přes Azure ověřování a autorizace najdete v tématu [Azure předávací ověřování a autorizace](../service-bus-relay/relay-authentication-and-authorization.md). 

