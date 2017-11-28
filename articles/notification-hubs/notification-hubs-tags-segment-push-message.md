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
# <a name="routing-and-tag-expressions"></a><span data-ttu-id="6047e-103">Výrazy směrování a značka</span><span class="sxs-lookup"><span data-stu-id="6047e-103">Routing and tag expressions</span></span>
## <a name="overview"></a><span data-ttu-id="6047e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="6047e-104">Overview</span></span>
<span data-ttu-id="6047e-105">Výrazy značky umožňují tootarget specifické sady zařízení, konkrétně registrací, nebo při odesílání nabízených oznámení pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="6047e-105">Tag expressions enable you tootarget specific sets of devices, or more specifically registrations, when sending a push notification through Notification Hubs.</span></span>

## <a name="targeting-specific-registrations"></a><span data-ttu-id="6047e-106">Cílení na konkrétní registrace</span><span class="sxs-lookup"><span data-stu-id="6047e-106">Targeting specific registrations</span></span>
<span data-ttu-id="6047e-107">Hello pouze způsob tootarget konkrétní oznámení registrace je tooassociate značky s nimi, pak cíle těchto značek.</span><span class="sxs-lookup"><span data-stu-id="6047e-107">hello only way tootarget specific notification registrations is tooassociate tags with them, then target those tags.</span></span> <span data-ttu-id="6047e-108">Jak je popsáno v [registrace správu](notification-hubs-push-notification-registration-management.md), v pořadí tooreceive nabízených oznámení aplikace má tooregister zařízení zpracování v rozbočovači oznámení.</span><span class="sxs-lookup"><span data-stu-id="6047e-108">As discussed in [Registration Management](notification-hubs-push-notification-registration-management.md), in order tooreceive push notifications an app has tooregister a device handle on a notification hub.</span></span> <span data-ttu-id="6047e-109">Po vytvoření registrace v centru oznámení můžete odesílat back-end aplikace hello tooit nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="6047e-109">Once a registration is created on a notification hub, hello application backend can send push notifications tooit.</span></span>
<span data-ttu-id="6047e-110">back-end aplikace Hello můžete zvolit hello registrace tootarget s konkrétní oznámení v hello následující způsoby:</span><span class="sxs-lookup"><span data-stu-id="6047e-110">hello application backend can choose hello registrations tootarget with a specific notification in hello following ways:</span></span>

1. <span data-ttu-id="6047e-111">**Vysílání**: všechny registrace v centru oznámení hello přijímat oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="6047e-111">**Broadcast**: all registrations in hello notification hub receive hello notification.</span></span>
2. <span data-ttu-id="6047e-112">**Značka**: všechny registrace, které obsahují hello zadané značky přijímat oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="6047e-112">**Tag**: all registrations that contain hello specified tag receive hello notification.</span></span>
3. <span data-ttu-id="6047e-113">**Značka výraz**: všechny registrace, jejichž sadu značky shodu hello zadaný výraz přijímat oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="6047e-113">**Tag expression**: all registrations whose set of tags match hello specified expression receive hello notification.</span></span>

## <a name="tags"></a><span data-ttu-id="6047e-114">Značky</span><span class="sxs-lookup"><span data-stu-id="6047e-114">Tags</span></span>
<span data-ttu-id="6047e-115">Značku může být libovolný řetězec, až too120 znaků, který obsahuje alfanumerické znaky, hello následující jiné než alfanumerické znaky: '_', ' @', '#', '. ',':', '-'.</span><span class="sxs-lookup"><span data-stu-id="6047e-115">A tag can be any string, up too120 characters, containing alphanumeric and hello following non-alphanumeric characters: ‘_’, ‘@’, ‘#’, ‘.’, ‘:’, ‘-’.</span></span> <span data-ttu-id="6047e-116">Hello následující příklad ukazuje, aplikace, ze kterého můžete přijímat oznámení informačního nápisu o konkrétní Hudba skupinách.</span><span class="sxs-lookup"><span data-stu-id="6047e-116">hello following example shows an application from which you can receive toast notifications about specific music groups.</span></span> <span data-ttu-id="6047e-117">V tomto scénáři je tooroute oznámení jednoduchý způsob registrace toolabel pomocí značek, které představují různé pruhy hello jako hello následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="6047e-117">In this scenario, a simple way tooroute notifications is toolabel registrations with tags that represent hello different bands, as in hello following picture.</span></span>

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

<span data-ttu-id="6047e-118">Na tomto obrázku uvítací zprávu příznakem **Beatles** dosáhnou pouze hello tablet který zaregistrovali hello značka **Beatles**.</span><span class="sxs-lookup"><span data-stu-id="6047e-118">In this picture, hello message tagged **Beatles** reaches only hello tablet that registered with hello tag **Beatles**.</span></span>

<span data-ttu-id="6047e-119">Další informace o vytváření registrace pro značky najdete v tématu [registrace správu](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="6047e-119">For more information about creating registrations for tags, see [Registration Management](notification-hubs-push-notification-registration-management.md).</span></span>

<span data-ttu-id="6047e-120">Můžete odeslat oznámení tootags pomocí hello odeslat oznámení metody hello `Microsoft.Azure.NotificationHubs.NotificationHubClient` třídy v hello [Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK.</span><span class="sxs-lookup"><span data-stu-id="6047e-120">You can send notifications tootags using hello send notifications methods of hello `Microsoft.Azure.NotificationHubs.NotificationHubClient` class in hello [Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK.</span></span> <span data-ttu-id="6047e-121">Můžete také použít Node.js nebo hello nabízená oznámení REST API.</span><span class="sxs-lookup"><span data-stu-id="6047e-121">You can also use Node.js, or hello Push Notifications REST APIs.</span></span>  <span data-ttu-id="6047e-122">Tady je příklad použití hello SDK.</span><span class="sxs-lookup"><span data-stu-id="6047e-122">Here's an example using hello SDK.</span></span>

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




<span data-ttu-id="6047e-123">Značky není třeba předem zřizovat toobe a mohou odkazovat koncepty toomultiple konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6047e-123">Tags do not have toobe pre-provisioned and can refer toomultiple app-specific concepts.</span></span> <span data-ttu-id="6047e-124">Uživatelé této ukázkové aplikaci můžete například komentář k pásma a chcete tooreceive informační zprávy, nejen pro komentáře hello na svých oblíbených pásma, ale také pro všechny komentáře od svých přátel, bez ohledu na to, na kterém jsou komentářů vzdálené hello.</span><span class="sxs-lookup"><span data-stu-id="6047e-124">For example, users of this example application can comment on bands and want tooreceive toasts, not only for hello comments on their favorite bands, but also for all comments from their friends, regardless of hello band on which they are commenting.</span></span> <span data-ttu-id="6047e-125">Hello následující obrázek ukazuje příklad tohoto scénáře:</span><span class="sxs-lookup"><span data-stu-id="6047e-125">hello following picture shows an example of this scenario:</span></span>

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

<span data-ttu-id="6047e-126">Na tomto obrázku má zájem o aktualizace pro hello Beatles Alice a má zájem o aktualizace pro hello Wailers Bob.</span><span class="sxs-lookup"><span data-stu-id="6047e-126">In this picture, Alice is interested in updates for hello Beatles, and Bob is interested in updates for hello Wailers.</span></span> <span data-ttu-id="6047e-127">Bob má také zájem o Karel na komentáře a Karel v zajímá hello Wailers.</span><span class="sxs-lookup"><span data-stu-id="6047e-127">Bob is also interested in Charlie’s comments, and Charlie is in interested in hello Wailers.</span></span> <span data-ttu-id="6047e-128">Při odesílání oznámení pro Karel na komentář na hello Beatles Alice a Bob ji přijmout.</span><span class="sxs-lookup"><span data-stu-id="6047e-128">When a notification is sent for Charlie’s comment on hello Beatles, both Alice and Bob receive it.</span></span>

<span data-ttu-id="6047e-129">Když může být kódován více obavy ve značkách (například "band_Beatles" nebo "follows_Charlie"), značky jsou jednoduché řetězce a vlastnosti hodnotami.</span><span class="sxs-lookup"><span data-stu-id="6047e-129">While you can encode multiple concerns in tags (for example, “band_Beatles” or “follows_Charlie”), tags are simple strings and not properties with values.</span></span> <span data-ttu-id="6047e-130">Registrace je shoda pouze pro hello existenci nebo neexistenci těchto s konkrétní značkou tag.</span><span class="sxs-lookup"><span data-stu-id="6047e-130">A registration is matched only on hello presence or absence of a specific tag.</span></span>

<span data-ttu-id="6047e-131">Úplné podrobný kurz na tom, jak toouse značky pro odesílání toointerest skupiny, najdete v části [novinkách](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).</span><span class="sxs-lookup"><span data-stu-id="6047e-131">For a full step-by-step tutorial on how toouse tags for sending toointerest groups, see [Breaking News](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).</span></span>

## <a name="using-tags-tootarget-users"></a><span data-ttu-id="6047e-132">Pomocí značky tootarget uživatelů</span><span class="sxs-lookup"><span data-stu-id="6047e-132">Using tags tootarget users</span></span>
<span data-ttu-id="6047e-133">Značky toouse jiný způsob, jak je tooidentify všechna zařízení hello konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="6047e-133">Another way toouse tags is tooidentify all hello devices of a particular user.</span></span> <span data-ttu-id="6047e-134">Registrace lze označit s značkou, který obsahuje id uživatele, jako hello následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="6047e-134">Registrations can be tagged with a tag that contains a user id, as in hello following picture:</span></span>

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

<span data-ttu-id="6047e-135">Na tomto obrázku hello zprávu s příznakem uid:Alice dosáhne všechny registrace s příznakem uid:Alice; proto všechna zařízení od Alice.</span><span class="sxs-lookup"><span data-stu-id="6047e-135">In this picture, hello message tagged uid:Alice reaches all registrations tagged uid:Alice; hence, all of Alice’s devices.</span></span>

## <a name="tag-expressions"></a><span data-ttu-id="6047e-136">Značka výrazy</span><span class="sxs-lookup"><span data-stu-id="6047e-136">Tag expressions</span></span>
<span data-ttu-id="6047e-137">Existují případy, ve kterých má oznámení tootarget sadu registrací identifikovanou není jedinou značku, ale logický výraz na značky.</span><span class="sxs-lookup"><span data-stu-id="6047e-137">There are cases in which a notification has tootarget a set of registrations that is identified not by a single tag, but by a Boolean expression on tags.</span></span>

<span data-ttu-id="6047e-138">Vezměte v úvahu aplikace sportu, která odesílá tooeveryone připomenutí v Boston o hru mezi hello Red Sox a Cardinals.</span><span class="sxs-lookup"><span data-stu-id="6047e-138">Consider a sports application that sends a reminder tooeveryone in Boston about a game between hello Red Sox and Cardinals.</span></span> <span data-ttu-id="6047e-139">Pokud klientské aplikace hello zaregistruje značky o zájem o týmy a umístění, musí být oznámení hello cílové tooeveryone v Boston, kteří chtějí hello Red Sox nebo hello Cardinals.</span><span class="sxs-lookup"><span data-stu-id="6047e-139">If hello client app registers tags about interest in teams and location, then hello notification should be targeted tooeveryone in Boston who is interested in either hello Red Sox or hello Cardinals.</span></span> <span data-ttu-id="6047e-140">Tento stav může být vyjádřený s hello následující logický výraz:</span><span class="sxs-lookup"><span data-stu-id="6047e-140">This condition can be expressed with hello following Boolean expression:</span></span>

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

<span data-ttu-id="6047e-141">Výrazy značka může obsahovat všech logických operátorů, například AND (& &), nebo (|) a ne (!).</span><span class="sxs-lookup"><span data-stu-id="6047e-141">Tag expressions can contain all Boolean operators, such as AND (&&), OR (||), and NOT (!).</span></span> <span data-ttu-id="6047e-142">Mohou také obsahovat závorek.</span><span class="sxs-lookup"><span data-stu-id="6047e-142">They can also contain parentheses.</span></span> <span data-ttu-id="6047e-143">Značka výrazy jsou omezené too20 značky, pokud obsahují pouze ORs; jinak jsou omezené too6 značky.</span><span class="sxs-lookup"><span data-stu-id="6047e-143">Tag expressions are limited too20 tags if they contain only ORs; otherwise they are limited too6 tags.</span></span>

<span data-ttu-id="6047e-144">Tady je příklad pro odesílání oznámení pomocí výrazů značky pomocí hello SDK.</span><span class="sxs-lookup"><span data-stu-id="6047e-144">Here's an example for sending notifications with tag expressions using hello SDK.</span></span>

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
