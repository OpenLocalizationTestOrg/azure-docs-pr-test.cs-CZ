---
title: "aaaHow toosend naplánované oznámení | Microsoft Docs"
description: "Toto téma popisuje naplánované oznámení pomocí Azure Notification Hubs."
services: notification-hubs
documentationcenter: .net
keywords: "nabízená oznámení, nabízených oznámení, plánování nabízená oznámení"
author: ysxu
manager: erikre
editor: 
ms.assetid: 6b718c75-75dd-4c99-aee3-db1288235c1a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 9b3ba715dad6f5d824a615e83f2863b0db47b533
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-send-scheduled-notifications"></a>Postupy: Odesílání naplánované oznámení
## <a name="overview"></a>Přehled
Pokud máte scénář, ve kterém chcete toosend oznámení v určitém okamžiku hello budoucí, ale nemají toowake snadno si kód back endu toosend hello oznámení. Úroveň Standard Notification Hubs podporuje funkce, která vám umožní oznámení tooschedule až too7 dní v budoucnu hello.

Při odesílání oznámení, jednoduše použijte hello [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) třídy v hello SDK centra oznámení, jak je znázorněno v hello následující ukázka:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Navíc může zrušit dříve naplánované oznámení pomocí jeho notificationId:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Neexistují žádná omezení počtu hello naplánované oznámení, která můžete odeslat.

