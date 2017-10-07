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
# <a name="how-to-send-scheduled-notifications"></a><span data-ttu-id="ea9b8-104">Postupy: Odesílání naplánované oznámení</span><span class="sxs-lookup"><span data-stu-id="ea9b8-104">How To: Send scheduled notifications</span></span>
## <a name="overview"></a><span data-ttu-id="ea9b8-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="ea9b8-105">Overview</span></span>
<span data-ttu-id="ea9b8-106">Pokud máte scénář, ve kterém chcete toosend oznámení v určitém okamžiku hello budoucí, ale nemají toowake snadno si kód back endu toosend hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="ea9b8-106">If you have a scenario in which you want toosend a notification at some point in hello future, but do not have an easy way toowake up your back-end code toosend hello notification.</span></span> <span data-ttu-id="ea9b8-107">Úroveň Standard Notification Hubs podporuje funkce, která vám umožní oznámení tooschedule až too7 dní v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="ea9b8-107">Standard tier Notification Hubs supports a feature that enables you tooschedule notifications up too7 days in hello future.</span></span>

<span data-ttu-id="ea9b8-108">Při odesílání oznámení, jednoduše použijte hello [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) třídy v hello SDK centra oznámení, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="ea9b8-108">When sending a notification, simply use hello [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) class in hello Notification Hubs SDK as shown in hello following example:</span></span>

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

<span data-ttu-id="ea9b8-109">Navíc může zrušit dříve naplánované oznámení pomocí jeho notificationId:</span><span class="sxs-lookup"><span data-stu-id="ea9b8-109">Also, you can cancel a previously scheduled notification using its notificationId:</span></span>

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

<span data-ttu-id="ea9b8-110">Neexistují žádná omezení počtu hello naplánované oznámení, která můžete odeslat.</span><span class="sxs-lookup"><span data-stu-id="ea9b8-110">There are no limits on hello number of scheduled notifications you can send.</span></span>

