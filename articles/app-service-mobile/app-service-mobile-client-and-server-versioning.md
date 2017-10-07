---
title: "aaaClient a server pro správu verzí sady SDK v Mobile Apps a Mobile Services | Microsoft Docs"
description: "Seznam klientskou sadu SDK a kompatibilitu s verzí serveru SDK pro Mobile Services a Azure Mobile Apps"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 35b19672-c9d6-49b5-b405-a6dcd1107cd5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 5874b7455ea407ca8c77fb1bd03d97d0767ebb47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Správa verzí klientských a serverových v Mobile Apps a Mobile Services
nejnovější verzi Azure Mobile Services Hello je hello **Mobile Apps** funkce Azure App Service.

Hello mobilní aplikace klienta a serveru SDK jsou původně založené na těch, které v Mobile Services, ale jsou *není* vzájemně kompatibilní.
To znamená, že musí použít *Mobile Apps* klienta SDK s *Mobile Apps* serveru SDK a podobně pro *Mobile Services*. Tato smlouva se vynucuje prostřednictvím hodnotu speciální hlavičky používané hello klientských a serverových sad SDK, `ZUMO-API-VERSION`.

Poznámka: když tento dokument odkazuje tooa *Mobile Services* back-end, nemusí nutně toobe hostované na Mobile Services. Je teď možné toomigrate toorun mobilní službu v App Service beze změn kódu, ale stále používat služby hello *Mobile Services* verze sady SDK.

Další informace o toolearn migrace tooApp služby beze změn kódu, najdete v článku hello [migrovat tooAzure služby mobilní služby App Service].

## <a name="header-specification"></a>Specifikace záhlaví
klíč Hello `ZUMO-API-VERSION` je možné zadat hello HTTP, hlavičku nebo řetězec dotazu hello. Hello hodnota je řetězec verze ve formuláři hello **x.y.z**.

Například:

ZÍSKAT https://service.azurewebsites.net/tables/TodoItem

HLAVIČKY: ZÁHLAVÍ ZUMO-API-VERSION: 2.0.0

POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Zrušení kontrolu verzí
Můžete vyjádření výslovného nesouhlasu kontroly nastavením hodnotu verze **true** pro nastavení aplikace hello **MS_SkipVersionCheck**. Tuto verzi uveďte v souboru web.config nebo v hello část nastavení aplikace hello portálu Azure.

> [!NOTE]
> Existuje několik změn v chování mezi Mobile Services a mobilní aplikace, zejména v oblasti hello ověřování, offline synchronizace a nabízených oznámení. Má jenom vyjádření výslovného nesouhlasu s verze kontrola po dokončení testování tooensure tyto změny chování není rozdělit funkce vaší aplikace.
>
>

## <a name="summary-of-compatibility-for-all-versions"></a>Souhrn kompatibility pro všechny verze
Graf Hello níže znázorňuje hello kompatibilitu mezi všechny typy klienta a serveru. Back-end je jsou klasifikovány jako buď Mobile **služby** nebo Mobile **aplikace** založené na serveru hello SDK, která používá.

|  | **Mobilní služby** Node.js, nebo .NET | **Mobilní aplikace** Node.js, nebo .NET |
| --- | --- | --- |
| [Klienti Mobile Services] |Ok |Chyba\* |
| [Klienti Mobile Apps] |Chyba\* |Ok |

\*To se dá nastavit podle určení **MS_SkipVersionCheck**.

<!-- IMPORTANT!  hello anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: hello fwlink toothis document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>Klientem Mobile Services a serveru
Hello klienta sady SDK v tabulce hello jsou kompatibilní s **Mobile Services**.

Poznámka: hello klientské sady SDK Mobile Services *nepodporují* odeslání hodnotu hlavičky pro `ZUMO-API-VERSION`. Pokud služba hello obdrží tato záhlaví nebo hodnotu řetězce dotazu, bude vrácena chyba, pokud jste explicitně zvolili odhlašování, jak je popsáno výše.

### <a name="MobileServicesClients"></a>Mobilní *služby* klientskou sadu SDK
| Klientské platformy | Verze | Hodnota záhlaví verze |
| --- | --- | --- |
| Spravované klientské (Windows, Xamarin) |[1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) |neuvedeno |
| iOS |[2.2.2](http://aka.ms/gc6fex) |neuvedeno |
| Android |[2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126) |neuvedeno |
| HTML |[1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) |neuvedeno |

### <a name="mobile-services-server-sdks"></a>Mobilní *služby* server sady SDK
| Platforma serveru | Verze | Přijatá verze hlavičky |
| --- | --- | --- |
| .NET |[Verze WindowsAzure.MobileServices.Backend.* 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) |** Žádné záhlaví verze ** |
| Node.js |(už brzy) |**Žádná hlavička verze** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>Chování back-EndY mobilní služby
| ZÁHLAVÍ ZUMO-API-VERSION | Hodnota MS_SkipVersionCheck | Odpověď |
| --- | --- | --- |
| Není zadaná. |Všechny |200 – OK |
| Libovolná hodnota |True |200 – OK |
| Libovolná hodnota |False nebo nebyla zadána |400 – Chybný požadavek |

## <a name="2.0.0"></a>Azure Mobile Apps klienta a serveru
### <a name="MobileAppsClients"></a>Mobilní *aplikace* klientskou sadu SDK
Kontrola verze byla zavedena počínaje hello následující verzí hello klienta SDK pro **Azure Mobile Apps**:

| Klientské platformy | Verze | Hodnota záhlaví verze |
| --- | --- | --- |
| Spravované klientské (Windows, Xamarin) |[2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |2.0.0 |
| iOS |[3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) |2.0.0 |
| Android |[3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>Mobilní *aplikace* server sady SDK
Kontrola verze je součástí následující verze sady SDK serveru:

| Platforma serveru | Sada SDK | Přijatá verze hlavičky |
| --- | --- | --- |
| .NET |[Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |2.0.0 |
| Node.js |[Azure mobile apps)](https://www.npmjs.com/package/azure-mobile-apps) |2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Chování back-EndY mobilní aplikace
| ZÁHLAVÍ ZUMO-API-VERSION | Hodnota MS_SkipVersionCheck | Odpověď |
| --- | --- | --- |
| x.y.z nebo hodnota Null. |True |200 – OK |
| Hodnotu Null |False nebo nebyla zadána |400 – Chybný požadavek |
| 1.x.y |False nebo nebyla zadána |400 – Chybný požadavek |
| 2.0.0-2.x.y |False nebo nebyla zadána |200 – OK |
| 3.0.0-3.x.y |False nebo nebyla zadána |400 – Chybný požadavek |

## <a name="next-steps"></a>Další kroky
* [migrovat tooAzure služby mobilní služby App Service]

[Klienti Mobile Services]: #MobileServicesClients
[Klienti Mobile Apps]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[migrovat tooAzure služby mobilní služby App Service]: app-service-mobile-migrating-from-mobile-services.md
