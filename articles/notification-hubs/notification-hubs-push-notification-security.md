---
title: "Zabezpečení pro centra oznámení"
description: "Toto téma vysvětluje zabezpečení služby Azure notification hubs."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: 6506177c-e25c-4af7-8508-a3ddca9dc07c
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 7c3283799806135060bb8ca57ea398c93d1106bb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="security"></a><span data-ttu-id="447c0-103">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="447c0-103">Security</span></span>
## <a name="overview"></a><span data-ttu-id="447c0-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="447c0-104">Overview</span></span>
<span data-ttu-id="447c0-105">Toto téma popisuje model zabezpečení Azure Notification hubs.</span><span class="sxs-lookup"><span data-stu-id="447c0-105">This topic describes the security model of Azure Notification Hubs.</span></span> <span data-ttu-id="447c0-106">Protože Notification Hubs entity služby sběrnice, implementace modelu zabezpečení jako Service Bus.</span><span class="sxs-lookup"><span data-stu-id="447c0-106">Because Notification Hubs are a Service Bus entity, they implement the same security model as Service Bus.</span></span> <span data-ttu-id="447c0-107">Další informace najdete v tématu [ověřování sběrnice služby](https://msdn.microsoft.com/library/azure/dn155925.aspx) témata.</span><span class="sxs-lookup"><span data-stu-id="447c0-107">For more information, see the [Service Bus Authentication](https://msdn.microsoft.com/library/azure/dn155925.aspx) topics.</span></span>

## <a name="shared-access-signature-security-sas"></a><span data-ttu-id="447c0-108">Zabezpečení sdílený přístupový podpis (SAS)</span><span class="sxs-lookup"><span data-stu-id="447c0-108">Shared Access Signature Security (SAS)</span></span>
<span data-ttu-id="447c0-109">Notification Hubs implementuje zabezpečení na úrovni entit schéma volá SAS (sdíleného přístupového podpisu).</span><span class="sxs-lookup"><span data-stu-id="447c0-109">Notification Hubs implements an entity-level security scheme called SAS (Shared Access Signature).</span></span> <span data-ttu-id="447c0-110">Toto schéma umožňuje deklarovat až 12 autorizační pravidla v popisu, která udělují oprávnění na dané entity entit pro zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="447c0-110">This scheme enables messaging entities to declare up to 12 authorization rules in their description that grant rights on that entity.</span></span>

<span data-ttu-id="447c0-111">Každé pravidlo obsahuje název, hodnotu klíče (sdílený tajný klíč) a sada práv, jak je popsáno v části "Deklarací identity zabezpečení."</span><span class="sxs-lookup"><span data-stu-id="447c0-111">Each rule contains a name, a key value (shared secret), and a set of rights, as explained in the section “Security Claims.”</span></span> <span data-ttu-id="447c0-112">Při vytváření centra oznámení, se automaticky vytvoří dvě pravidla: jednu s naslouchání právy (pomocí klientské aplikace) a druhou se všemi právy k (které používá back-end aplikace).</span><span class="sxs-lookup"><span data-stu-id="447c0-112">When creating a Notification Hub, two rules are automatically created: one with Listen rights (that the client app uses) and one with all rights (that the app backend uses).</span></span>

<span data-ttu-id="447c0-113">Při provádění správy registrace z klientské aplikace, pokud odesílané informace prostřednictvím oznámení není citlivé (například počasí aktualizace), běžný způsob přístup Centrum oznámení je umožnit hodnota klíče pravidla přístup jen pro naslouchání na klientskou aplikaci a umožňuje klíčovou hodnotu pravidlo úplný přístup k back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="447c0-113">When performing registration management from client apps, if the information sent via notifications is not sensitive (for example, weather updates), a common way to access a Notification Hub is to give the key value of the rule Listen-only access to the client app, and to give the key value of the rule full access to the app backend.</span></span>

<span data-ttu-id="447c0-114">Není doporučeno vložit hodnotu klíče klienta aplikace pro Windows Store.</span><span class="sxs-lookup"><span data-stu-id="447c0-114">It is not recommended that you embed the key value in Windows Store client apps.</span></span> <span data-ttu-id="447c0-115">Způsob, jak se vyhnout vložení hodnota klíče se se klientskou aplikaci načíst z back-end aplikace při spuštění.</span><span class="sxs-lookup"><span data-stu-id="447c0-115">A way to avoid embedding the key value is to have the client app retrieve it from the app backend at startup.</span></span>

<span data-ttu-id="447c0-116">Je důležité si uvědomit, že klíč s přístupem k naslouchání umožňuje klientskou aplikaci zaregistrovat pro všechny značky.</span><span class="sxs-lookup"><span data-stu-id="447c0-116">It is important to understand that the key with Listen access allows a client app to register for any tag.</span></span> <span data-ttu-id="447c0-117">Pokud vaše aplikace musí registrace omezit na konkrétní značky pro konkrétní klienty (například když značky představují ID uživatele), musíte provést back-end aplikace registrace.</span><span class="sxs-lookup"><span data-stu-id="447c0-117">If your app must restrict registrations to specific tags to specific clients (for example, when tags represent user IDs), then your app backend must perform the registrations.</span></span> <span data-ttu-id="447c0-118">Další informace najdete v tématu registrace správy.</span><span class="sxs-lookup"><span data-stu-id="447c0-118">For more information, see Registration Management.</span></span> <span data-ttu-id="447c0-119">Všimněte si, že tímto způsobem, klientská aplikace nebudou mít přímý přístup k centru oznámení.</span><span class="sxs-lookup"><span data-stu-id="447c0-119">Note that in this way, the client app will not have direct access to Notification Hubs.</span></span>

## <a name="security-claims"></a><span data-ttu-id="447c0-120">Deklarací identity zabezpečení</span><span class="sxs-lookup"><span data-stu-id="447c0-120">Security claims</span></span>
<span data-ttu-id="447c0-121">Podobně jako ostatní entity, jsou povolené operace centra oznámení pro tři deklarací identity zabezpečení: naslouchání, odeslání a spravovat.</span><span class="sxs-lookup"><span data-stu-id="447c0-121">Similar to other entities, Notification Hub operations are allowed for three security claims: Listen, Send, and Manage.</span></span>

| <span data-ttu-id="447c0-122">Deklarovat</span><span class="sxs-lookup"><span data-stu-id="447c0-122">Claim</span></span> | <span data-ttu-id="447c0-123">Popis</span><span class="sxs-lookup"><span data-stu-id="447c0-123">Description</span></span> | <span data-ttu-id="447c0-124">Povolené operace</span><span class="sxs-lookup"><span data-stu-id="447c0-124">Operations allowed</span></span> |
| --- | --- | --- |
| <span data-ttu-id="447c0-125">Naslouchání</span><span class="sxs-lookup"><span data-stu-id="447c0-125">Listen</span></span> |<span data-ttu-id="447c0-126">Vytvořit nebo aktualizovat, čtení a odstranění jednoho registrace</span><span class="sxs-lookup"><span data-stu-id="447c0-126">Create/Update, Read, and Delete single registrations</span></span> |<span data-ttu-id="447c0-127">Vytvořit nebo aktualizovat registraci</span><span class="sxs-lookup"><span data-stu-id="447c0-127">Create/Update registration</span></span><br><br><span data-ttu-id="447c0-128">Registrace pro čtení</span><span class="sxs-lookup"><span data-stu-id="447c0-128">Read registration</span></span><br><br><span data-ttu-id="447c0-129">Číst všechny registrace pro popisovač</span><span class="sxs-lookup"><span data-stu-id="447c0-129">Read all registrations for a handle</span></span><br><br><span data-ttu-id="447c0-130">Odstranit registrace</span><span class="sxs-lookup"><span data-stu-id="447c0-130">Delete registration</span></span> |
| <span data-ttu-id="447c0-131">Odeslat</span><span class="sxs-lookup"><span data-stu-id="447c0-131">Send</span></span> |<span data-ttu-id="447c0-132">Odesílání zpráv do centra oznámení</span><span class="sxs-lookup"><span data-stu-id="447c0-132">Send messages to the notification hub</span></span> |<span data-ttu-id="447c0-133">Odeslat zprávu</span><span class="sxs-lookup"><span data-stu-id="447c0-133">Send message</span></span> |
| <span data-ttu-id="447c0-134">Spravovat</span><span class="sxs-lookup"><span data-stu-id="447c0-134">Manage</span></span> |<span data-ttu-id="447c0-135">CRUDs v Notification Hubs (včetně aktualizace systému PNS přihlašovacích údajů a zabezpečení klíče) a čtení registrace podle značky</span><span class="sxs-lookup"><span data-stu-id="447c0-135">CRUDs on Notification Hubs (including updating PNS credentials, and security keys), and read registrations based on tags</span></span> |<span data-ttu-id="447c0-136">Vytvoření, aktualizace nebo pro čtení nebo odstranění notification hubs</span><span class="sxs-lookup"><span data-stu-id="447c0-136">Create/Update/Read/Delete notification hubs</span></span><br><br><span data-ttu-id="447c0-137">Číst registrace podle značky</span><span class="sxs-lookup"><span data-stu-id="447c0-137">Read registrations by tag</span></span> |

<span data-ttu-id="447c0-138">Centra oznámení přijímat deklarace identity udělena tokeny řízení přístupu Microsoft Azure a tokeny podpis vygeneroval s sdílených klíčů nakonfigurované přímo v centru oznámení.</span><span class="sxs-lookup"><span data-stu-id="447c0-138">Notification Hubs accept claims granted by Microsoft Azure Access Control tokens, and by signature tokens generated with shared keys configured directly on the Notification Hub.</span></span>
