---
title: "aaaRouting a značky výrazy"
description: "Toto téma vysvětluje směrování a značky výrazy pro Azure notification hubs."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: 0fffb3bb-8ed8-4e0f-89e8-0de24a47f644
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: c2c60500f7469f1cb1a73a5cf63c221a9ad6cbb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="routing-and-tag-expressions"></a>Výrazy směrování a značka
## <a name="overview"></a>Přehled
Výrazy značky umožňují tootarget specifické sady zařízení, konkrétně registrací, nebo při odesílání nabízených oznámení pomocí centra oznámení.

## <a name="targeting-specific-registrations"></a>Cílení na konkrétní registrace
Hello pouze způsob tootarget konkrétní oznámení registrace je tooassociate značky s nimi, pak cíle těchto značek. Jak je popsáno v [registrace správu](notification-hubs-push-notification-registration-management.md), v pořadí tooreceive nabízených oznámení aplikace má tooregister zařízení zpracování v rozbočovači oznámení. Po vytvoření registrace v centru oznámení můžete odesílat back-end aplikace hello tooit nabízená oznámení.
back-end aplikace Hello můžete zvolit hello registrace tootarget s konkrétní oznámení v hello následující způsoby:

1. **Vysílání**: všechny registrace v centru oznámení hello přijímat oznámení hello.
2. **Značka**: všechny registrace, které obsahují hello zadané značky přijímat oznámení hello.
3. **Značka výraz**: všechny registrace, jejichž sadu značky shodu hello zadaný výraz přijímat oznámení hello.

## <a name="tags"></a>Značky
Značku může být libovolný řetězec, až too120 znaků, který obsahuje alfanumerické znaky, hello následující jiné než alfanumerické znaky: '_', ' @', '#', '. ',':', '-'. Hello následující příklad ukazuje, aplikace, ze kterého můžete přijímat oznámení informačního nápisu o konkrétní Hudba skupinách. V tomto scénáři je tooroute oznámení jednoduchý způsob registrace toolabel pomocí značek, které představují různé pruhy hello jako hello následující obrázek.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

Na tomto obrázku uvítací zprávu příznakem **Beatles** dosáhnou pouze hello tablet který zaregistrovali hello značka **Beatles**.

Další informace o vytváření registrace pro značky najdete v tématu [registrace správu](notification-hubs-push-notification-registration-management.md).

Můžete odeslat oznámení tootags pomocí hello odeslat oznámení metody hello `Microsoft.Azure.NotificationHubs.NotificationHubClient` třídy v hello [Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. Můžete také použít Node.js nebo hello nabízená oznámení REST API.  Tady je příklad použití hello SDK.

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Značky není třeba předem zřizovat toobe a mohou odkazovat koncepty toomultiple konkrétní aplikaci. Uživatelé této ukázkové aplikaci můžete například komentář k pásma a chcete tooreceive informační zprávy, nejen pro komentáře hello na svých oblíbených pásma, ale také pro všechny komentáře od svých přátel, bez ohledu na to, na kterém jsou komentářů vzdálené hello. Hello následující obrázek ukazuje příklad tohoto scénáře:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

Na tomto obrázku má zájem o aktualizace pro hello Beatles Alice a má zájem o aktualizace pro hello Wailers Bob. Bob má také zájem o Karel na komentáře a Karel v zajímá hello Wailers. Při odesílání oznámení pro Karel na komentář na hello Beatles Alice a Bob ji přijmout.

Když může být kódován více obavy ve značkách (například "band_Beatles" nebo "follows_Charlie"), značky jsou jednoduché řetězce a vlastnosti hodnotami. Registrace je shoda pouze pro hello existenci nebo neexistenci těchto s konkrétní značkou tag.

Úplné podrobný kurz na tom, jak toouse značky pro odesílání toointerest skupiny, najdete v části [novinkách](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).

## <a name="using-tags-tootarget-users"></a>Pomocí značky tootarget uživatelů
Značky toouse jiný způsob, jak je tooidentify všechna zařízení hello konkrétního uživatele. Registrace lze označit s značkou, který obsahuje id uživatele, jako hello následujícím obrázku:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

Na tomto obrázku hello zprávu s příznakem uid:Alice dosáhne všechny registrace s příznakem uid:Alice; proto všechna zařízení od Alice.

## <a name="tag-expressions"></a>Značka výrazy
Existují případy, ve kterých má oznámení tootarget sadu registrací identifikovanou není jedinou značku, ale logický výraz na značky.

Vezměte v úvahu aplikace sportu, která odesílá tooeveryone připomenutí v Boston o hru mezi hello Red Sox a Cardinals. Pokud klientské aplikace hello zaregistruje značky o zájem o týmy a umístění, musí být oznámení hello cílové tooeveryone v Boston, kteří chtějí hello Red Sox nebo hello Cardinals. Tento stav může být vyjádřený s hello následující logický výraz:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Výrazy značka může obsahovat všech logických operátorů, například AND (& &), nebo (|) a ne (!). Mohou také obsahovat závorek. Značka výrazy jsou omezené too20 značky, pokud obsahují pouze ORs; jinak jsou omezené too6 značky.

Tady je příklad pro odesílání oznámení pomocí výrazů značky pomocí hello SDK.

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)";    

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on hello Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on hello Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
