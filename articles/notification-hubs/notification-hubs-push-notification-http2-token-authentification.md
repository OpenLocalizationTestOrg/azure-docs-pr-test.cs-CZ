---
title: "na základě aaaToken ověřování (HTTP/2) pro služby APN v Azure Notification Hubs | Microsoft Docs"
description: "Toto téma vysvětluje, jak tooleverage hello nový token ověřování pro služby APN"
services: notification-hubs
documentationcenter: .net
author: kpiteira
manager: erikre
editor: 
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 05/17/2017
ms.author: kapiteir
ms.openlocfilehash: 3353d7f16033ce0b68edec9ee9aeb98f47faa1fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="token-based-http2-authentication-for-apns"></a>Ověřování na základě tokenu (HTTP/2) pro služby APN
## <a name="overview"></a>Přehled
Tento článek podrobně popisuje, jak ověřování na základě toouse hello nový APNS HTTP/2 protokol s tokenem.

pomocí nového protokolu hello Hello klíčové výhody patří:
-   Generování tokenů je poměrně jednoduchý volné (porovnání toocertificates)
-   Žádná další data vypršení – jsou v ovládacím prvku ověřovací tokeny a jejich odvolání
-   Datové části teď může být až too4 KB
- Synchronní zpětné vazby
-   Jste nejnovější protokol společnosti Apple – certifikátů dál používat hello binární protokol, který je označen pro vyřazení

Pomocí tohoto nového mechanismu lze provést ve dvou krocích za pár minut:
1.  Získat informace potřebné hello z portálu Apple Developer účtu hello
2.  Konfigurace centra oznámení s informací o novém hello

Centra oznámení je nyní všechny sady toouse hello nového ověřování systému pomocí služby APNS. 

Pamatujte, že pokud jste migrovali ze pomocí přihlašovacích údajů certifikát služby APN:
- token vlastnosti Hello přepsat svůj certifikát v našem systému
- ale aplikace stále tooreceive oznámení bez problémů.

## <a name="obtaining-authentication-information-from-apple"></a>Získávání informací o ověření od společnosti Apple
ověřování na základě tokenu tooenable, je třeba hello následující vlastnosti z vašeho účtu Apple Developer:
### <a name="key-identifier"></a>Identifikátor klíče
Nelze získat identifikátor klíče Hello ze stránky "Klíče" hello ve vašem účtu vývojáře Apple

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier--application-name"></a>Identifikátor aplikace & název aplikace
název aplikace Hello je k dispozici prostřednictvím stránku hello ID aplikace hello vývojářský účet. 
![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

identifikátor aplikace Hello je k dispozici prostřednictvím hello stránce s podrobnostmi o členství v hello vývojářský účet.
![](./media/notification-hubs-push-notification-http2-token-authentification/app-id.png)


### <a name="authentication-token"></a>Ověřovací token
Hello ověřovací token si můžete stáhnout po vygenerování tokenu pro vaši aplikaci. Podrobnosti o tom, jak toogenerate tento token, naleznete v příliš[dokumentaci pro vývojáře Apple](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).

## <a name="configuring-your-notification-hub-toouse-token-based-authentication"></a>Konfigurace oznámení centra toouse na základě tokenu ověřování
### <a name="configure-via-hello-azure-portal"></a>Nakonfigurovat přes hello portálu Azure
tooenable token ověřování hello portálu, přihlášení toohello portál Azure a přejděte tooyour centra oznámení > oznámení služby > panely služby APN. 

Existuje nový vlastnost – *režim ověřování*. Výběr Token vám umožní tooupdate vašeho centra s všech hello relevantní token vlastností.

![](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

- Zadejte vlastnosti hello, který jste získali z účtu vývojáře Apple 
- Zvolte režim vaší aplikace (produkční nebo izolovaného prostoru) 
- Klikněte na Uložit tooupdate vaše přihlašovací údaje APNS. 

### <a name="configure-via-management-api-rest"></a>Nakonfigurovat přes rozhraní API (REST) pro správu

Můžete použít naše [rozhraní API pro správu](https://msdn.microsoft.com/library/azure/dn495827.aspx) tooupdate oznámení centra toouse na základě tokenu ověřování.
V závislosti na tom, jestli je hello aplikaci, kterou konfigurujete izolovaného prostoru nebo produkční aplikace (zadané v účtu vývojáře Apple) použijte jednu z hello odpovídající koncové body:

- Koncový bod izolovaného prostoru: [https://api.development.push.apple.com:443, 3 nebo zařízení](https://api.development.push.apple.com:443/3/device)
- Koncový bod produkční: [https://api.push.apple.com:443, 3 nebo zařízení](https://api.push.apple.com:443/3/device)

> [!IMPORTANT]
> Tokeny ověřování vyžaduje verzi rozhraní API z: **2017-04 nebo novější**.
> 
> 

Tady je příklad tooupdate požadavek PUT rozbočovač s ověřováním na základě tokenu:


        PUT https://{namespace}.servicebus.windows.net/{Notification Hub}?api-version=2017-04
          "Properties": {
            "ApnsCredential": {
              "Properties": {
                "KeyId": "<Your Key Id>",
                "Token": "<Your Authentication Token>",
                "AppName": "<Your Application Name>",
                "AppId": "<Your Application Id>",
                "Endpoint":"<Sandbox/Production Endpoint>"
              }
            }
          }
        

### <a name="configure-via-hello-net-sdk"></a>Nakonfigurovat přes hello .NET SDK
Můžete nakonfigurovat vaše centra toouse tokenu ověřování založené na našem [nejnovější verzi klienta SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8). 

Zde je ukázka kódu ilustrující hello správné použití:


        NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
        string token = "YOUR TOKEN HERE";
        string keyId = "YOUR KEY ID HERE";
        string appName = "YOUR APP NAME HERE";
        string appId = "YOUR APP ID HERE";
        NotificationHubDescription desc = new NotificationHubDescription("PATH tooYOUR HUB");
        desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
        desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
        nm.UpdateNotificationHubAsync(desc);

## <a name="reverting-toousing-certificate-based-authentication"></a>Ověřování pomocí certifikátů navrácení toousing
Při ověřování založené na certifikátech toousing čas můžete obnovit pomocí jakékoli předchozí hello certifikátu metoda a předávání místo hello token vlastností. Aby akce přepíše hello dříve uložené přihlašovací údaje.
