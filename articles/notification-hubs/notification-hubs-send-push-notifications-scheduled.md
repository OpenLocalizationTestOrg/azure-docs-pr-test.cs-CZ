---
title: "Postup odesílání oznámení naplánované | Microsoft Docs"
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
ms.openlocfilehash: efac6e1ecc00359f1622d380333140bc055c83e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-send-scheduled-notifications"></a><span data-ttu-id="7f905-104">Postupy: Odesílání naplánované oznámení</span><span class="sxs-lookup"><span data-stu-id="7f905-104">How To: Send scheduled notifications</span></span>
## <a name="overview"></a><span data-ttu-id="7f905-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="7f905-105">Overview</span></span>
<span data-ttu-id="7f905-106">Pokud máte scénář, ve kterém chcete poslat oznámení v určitém okamžiku v budoucnu, ale nemají snadný způsob, jak probuzení si kód back-end pro odeslání oznámení.</span><span class="sxs-lookup"><span data-stu-id="7f905-106">If you have a scenario in which you want to send a notification at some point in the future, but do not have an easy way to wake up your back-end code to send the notification.</span></span> <span data-ttu-id="7f905-107">Úroveň Standard Notification Hubs podporuje funkce, která umožňuje naplánovat oznámení až 7 dní v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="7f905-107">Standard tier Notification Hubs supports a feature that enables you to schedule notifications up to 7 days in the future.</span></span>

<span data-ttu-id="7f905-108">Při odesílání oznámení, jednoduše použijte [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) třídy v sadě SDK centra oznámení, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="7f905-108">When sending a notification, simply use the [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) class in the Notification Hubs SDK as shown in the following example:</span></span>

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

<span data-ttu-id="7f905-109">Navíc může zrušit dříve naplánované oznámení pomocí jeho notificationId:</span><span class="sxs-lookup"><span data-stu-id="7f905-109">Also, you can cancel a previously scheduled notification using its notificationId:</span></span>

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

<span data-ttu-id="7f905-110">Neexistují žádná omezení počtu naplánovaných oznámení, která můžete odeslat.</span><span class="sxs-lookup"><span data-stu-id="7f905-110">There are no limits on the number of scheduled notifications you can send.</span></span>

