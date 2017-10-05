---
title: "Azure Service Bus nejčastější dotazy (FAQ) | Microsoft Docs"
description: "Odpovídá na některé často kladené otázky týkající se Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: cc75786d-3448-4f79-9fec-eef56c0027ba
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: 1403184d96388cb03b2c767c4da342ec1c6fe236
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="service-bus-faq"></a><span data-ttu-id="b0f47-103">Nejčastější dotazy k Service Bus</span><span class="sxs-lookup"><span data-stu-id="b0f47-103">Service Bus FAQ</span></span>
<span data-ttu-id="b0f47-104">Tento článek obsahuje odpovědi na některé nejčastější dotazy týkající se služby Microsoft Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b0f47-104">This article answers some frequently asked questions about Microsoft Azure Service Bus.</span></span> <span data-ttu-id="b0f47-105">Další informace získáte [podporu nejčastější dotazy k Azure](http://go.microsoft.com/fwlink/?LinkID=185083) obecné Azure – ceny a podporu informace.</span><span class="sxs-lookup"><span data-stu-id="b0f47-105">You can also visit the [Azure Support FAQs](http://go.microsoft.com/fwlink/?LinkID=185083) for general Azure pricing and support information.</span></span>

## <a name="general-questions-about-azure-service-bus"></a><span data-ttu-id="b0f47-106">Obecné otázky o Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="b0f47-106">General questions about Azure Service Bus</span></span>
### <a name="what-is-azure-service-bus"></a><span data-ttu-id="b0f47-107">Co je Azure Service Bus?</span><span class="sxs-lookup"><span data-stu-id="b0f47-107">What is Azure Service Bus?</span></span>
<span data-ttu-id="b0f47-108">[Azure Service Bus](service-bus-messaging-overview.md) je asynchronní zasílání zpráv Cloudová platforma, která umožňuje odesílání dat mezi odpojeného systémy.</span><span class="sxs-lookup"><span data-stu-id="b0f47-108">[Azure Service Bus](service-bus-messaging-overview.md) is an asynchronous messaging cloud platform that enables you to send data between decoupled systems.</span></span> <span data-ttu-id="b0f47-109">Společnost Microsoft poskytuje jako služba, což znamená, že není potřeba žádný svůj vlastní hardware hostitele odborných tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b0f47-109">Microsoft offers this feature as a service, which means that you do not need to host any of your own hardware in order to use it.</span></span>

### <a name="what-is-a-service-bus-namespace"></a><span data-ttu-id="b0f47-110">Co je obor názvů Service Bus?</span><span class="sxs-lookup"><span data-stu-id="b0f47-110">What is a Service Bus namespace?</span></span>
<span data-ttu-id="b0f47-111">A [obor názvů](service-bus-create-namespace-portal.md) poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b0f47-111">A [namespace](service-bus-create-namespace-portal.md) provides a scoping container for addressing Service Bus resources within your application.</span></span> <span data-ttu-id="b0f47-112">Vytvořit je nutné použít Service Bus a bude jeden z první kroků v části Začínáme.</span><span class="sxs-lookup"><span data-stu-id="b0f47-112">Creating one is necessary to use Service Bus and will be one of the first steps in getting started.</span></span>

### <a name="what-is-an-azure-service-bus-queue"></a><span data-ttu-id="b0f47-113">Co je fronty Azure Service Bus?</span><span class="sxs-lookup"><span data-stu-id="b0f47-113">What is an Azure Service Bus queue?</span></span>
<span data-ttu-id="b0f47-114">A [fronty Service Bus](service-bus-queues-topics-subscriptions.md) je entita, ve kterém jsou uložené zprávy.</span><span class="sxs-lookup"><span data-stu-id="b0f47-114">A [Service Bus queue](service-bus-queues-topics-subscriptions.md) is an entity in which messages are stored.</span></span> <span data-ttu-id="b0f47-115">Fronty jsou užitečné, když máte více aplikací, nebo z několika součástí distribuované aplikace, které potřebují ke komunikaci mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="b0f47-115">Queues are particularly useful when you have multiple applications, or multiple parts of a distributed application that need to communicate with each other.</span></span> <span data-ttu-id="b0f47-116">Fronta je podobná distribučního centra v, že více produktů (zpráv) jsou přijata a pak se odešle z tohoto umístění.</span><span class="sxs-lookup"><span data-stu-id="b0f47-116">The queue is similar to a distribution center in that multiple products (messages) are received and then sent from that location.</span></span>

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a><span data-ttu-id="b0f47-117">Co jsou témata a předplatné Azure Service Bus?</span><span class="sxs-lookup"><span data-stu-id="b0f47-117">What are Azure Service Bus topics and subscriptions?</span></span>
<span data-ttu-id="b0f47-118">Téma můžete vizualizovat jako fronty a při použití více předplatných, začne bohatší zasílání zpráv modelu; v podstatě nástroj na více komunikace.</span><span class="sxs-lookup"><span data-stu-id="b0f47-118">A topic can be visualized as a queue and when using multiple subscriptions, it becomes a richer messaging model; essentially a one-to-many communication tool.</span></span> <span data-ttu-id="b0f47-119">Tento model publikování a přihlášení k odběru (nebo *pub nebo sub*) umožňuje aplikaci, která odešle zprávu do tématu s více odběry tak, aby měl tento zpráv přijatých více aplikací.</span><span class="sxs-lookup"><span data-stu-id="b0f47-119">This publish/subscribe model (or *pub/sub*) enables an application that sends a message to a topic with multiple subscriptions to have that message received by multiple applications.</span></span>

### <a name="what-is-a-partitioned-entity"></a><span data-ttu-id="b0f47-120">Co je dělené entity?</span><span class="sxs-lookup"><span data-stu-id="b0f47-120">What is a partitioned entity?</span></span>
<span data-ttu-id="b0f47-121">Konvenční fronta nebo téma zpracování zprostředkovatelem jedné zprávy a uloženy v úložišti jeden zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="b0f47-121">A conventional queue or topic is handled by a single message broker and stored in one messaging store.</span></span> <span data-ttu-id="b0f47-122">A [oddílů fronta nebo téma](service-bus-partitioning.md) zpracovává více zpráv zprostředkovatelé a uložená v úložištích více zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="b0f47-122">A [partitioned queue or topic](service-bus-partitioning.md) is handled by multiple message brokers and stored in multiple messaging stores.</span></span> <span data-ttu-id="b0f47-123">To znamená, že celkovou propustnost oddílů fronta nebo téma už není omezené podle výkonu zprostředkovatele jedné zprávy nebo úložišti pro přenos zpráv.</span><span class="sxs-lookup"><span data-stu-id="b0f47-123">This means that the overall throughput of a partitioned queue or topic is no longer limited by the performance of a single message broker or messaging store.</span></span> <span data-ttu-id="b0f47-124">Kromě toho dočasnému výpadku zasílání zpráv úložiště nevykresluje oddílů fronta nebo téma není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b0f47-124">In addition, a temporary outage of a messaging store does not render a partitioned queue or topic unavailable.</span></span>

<span data-ttu-id="b0f47-125">Všimněte si, že řazení není zajištěna při použití rozdělení entity.</span><span class="sxs-lookup"><span data-stu-id="b0f47-125">Note that ordering is not ensured when using partitioning entities.</span></span> <span data-ttu-id="b0f47-126">V případě, že oddíl není k dispozici, můžete i nadále odesílat a přijímat zprávy z jiných oddílů.</span><span class="sxs-lookup"><span data-stu-id="b0f47-126">In the event that a partition is unavailable, you can still send and receive messages from the other partitions.</span></span>

## <a name="best-practices"></a><span data-ttu-id="b0f47-127">Osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="b0f47-127">Best practices</span></span>
### <a name="what-are-some-azure-service-bus-best-practices"></a><span data-ttu-id="b0f47-128">Jaké jsou některé z osvědčených postupů Azure Service Bus?</span><span class="sxs-lookup"><span data-stu-id="b0f47-128">What are some Azure Service Bus best practices?</span></span>
* <span data-ttu-id="b0f47-129">[Osvědčené postupy pro zlepšení výkonu pomocí služby Service Bus] [ Best practices for performance improvements using Service Bus] – Tento článek popisuje, jak optimalizovat výkon při výměně zpráv.</span><span class="sxs-lookup"><span data-stu-id="b0f47-129">[Best practices for performance improvements using Service Bus][Best practices for performance improvements using Service Bus] – this article describes how to optimize performance when exchanging messages.</span></span>

### <a name="what-should-i-know-before-creating-entities"></a><span data-ttu-id="b0f47-130">Co je třeba vědět před vytvořením entity?</span><span class="sxs-lookup"><span data-stu-id="b0f47-130">What should I know before creating entities?</span></span>
<span data-ttu-id="b0f47-131">Následující vlastnosti frontu a téma se nedá změnit.</span><span class="sxs-lookup"><span data-stu-id="b0f47-131">The following properties of a queue and topic are immutable.</span></span> <span data-ttu-id="b0f47-132">Proveďte to v úvahu při zřizování vaší entity jako to nemůže být upraven, bez vytvoření nové entity nahrazení.</span><span class="sxs-lookup"><span data-stu-id="b0f47-132">Please take this into account when you provision your entities as this cannot be modified, without creating a new replacement entity.</span></span>

* <span data-ttu-id="b0f47-133">Velikost</span><span class="sxs-lookup"><span data-stu-id="b0f47-133">Size</span></span>
* <span data-ttu-id="b0f47-134">Dělení</span><span class="sxs-lookup"><span data-stu-id="b0f47-134">Partitioning</span></span>
* <span data-ttu-id="b0f47-135">Relace</span><span class="sxs-lookup"><span data-stu-id="b0f47-135">Sessions</span></span>
* <span data-ttu-id="b0f47-136">Detekce duplicitních</span><span class="sxs-lookup"><span data-stu-id="b0f47-136">Duplicate detection</span></span>
* <span data-ttu-id="b0f47-137">Expresní entity</span><span class="sxs-lookup"><span data-stu-id="b0f47-137">Express entity</span></span>

## <a name="pricing"></a><span data-ttu-id="b0f47-138">Ceny</span><span class="sxs-lookup"><span data-stu-id="b0f47-138">Pricing</span></span>
<span data-ttu-id="b0f47-139">Tato část odpovídá na některé často kladené otázky týkající se služby Service Bus cenové struktury.</span><span class="sxs-lookup"><span data-stu-id="b0f47-139">This section answers some frequently-asked questions about the Service Bus pricing structure.</span></span>

<span data-ttu-id="b0f47-140">[Service Bus ceny a fakturace](service-bus-pricing-billing.md) článek vysvětluje fakturace měřidla v Service Bus a informace o cenách možností Service Bus, najdete v části [Service Bus podrobnosti o cenách](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="b0f47-140">The [Service Bus pricing and billing](service-bus-pricing-billing.md) article explains the billing meters in Service Bus, and for information about Service Bus pricing options, see [Service Bus pricing details](https://azure.microsoft.com/pricing/details/service-bus/).</span></span>

<span data-ttu-id="b0f47-141">Další informace získáte [Azure podporu nejčastější dotazy k](http://go.microsoft.com/fwlink/?LinkID=185083) pro obecné Azure informace o cenách.</span><span class="sxs-lookup"><span data-stu-id="b0f47-141">You can also visit the [Azure Support FAQs](http://go.microsoft.com/fwlink/?LinkID=185083) for general Azure pricing information.</span></span> 

### <a name="how-do-you-charge-for-service-bus"></a><span data-ttu-id="b0f47-142">Jak se vám účtují pro Service Bus?</span><span class="sxs-lookup"><span data-stu-id="b0f47-142">How do you charge for Service Bus?</span></span>
<span data-ttu-id="b0f47-143">Úplné informace o cenách služby Service Bus, najdete v tématu [Service Bus podrobnosti o cenách][Pricing overview].</span><span class="sxs-lookup"><span data-stu-id="b0f47-143">For complete information about Service Bus pricing, please see [Service Bus pricing details][Pricing overview].</span></span> <span data-ttu-id="b0f47-144">Kromě ceny uvedené budou se vám účtovat pro přenosy přidružená data o sazbách za odchozí mimo datové centrum, ve kterém je zajištěna vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b0f47-144">In addition to the prices noted, you are charged for associated data transfers for egress outside of the data center in which your application is provisioned.</span></span>

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not"></a><span data-ttu-id="b0f47-145">Jaké využití služby Service Bus podléhá přenos dat?</span><span class="sxs-lookup"><span data-stu-id="b0f47-145">What usage of Service Bus is subject to data transfer?</span></span> <span data-ttu-id="b0f47-146">Co není?</span><span class="sxs-lookup"><span data-stu-id="b0f47-146">What is not?</span></span>
<span data-ttu-id="b0f47-147">Bez poplatků, stejně tak i všechny příchozí přenosy dat je k dispozici žádné přenos dat v dané oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="b0f47-147">Any data transfer within a given Azure region is provided at no charge, as well as any inbound data transfer.</span></span> <span data-ttu-id="b0f47-148">Přenos dat mimo oblast je předmětem poplatky za odchozí data, které můžou být nalezen [zde](https://azure.microsoft.com/pricing/details/bandwidth/).</span><span class="sxs-lookup"><span data-stu-id="b0f47-148">Data transfer outside a region is subject to egress charges which can be found [here](https://azure.microsoft.com/pricing/details/bandwidth/).</span></span>

### <a name="does-service-bus-charge-for-storage"></a><span data-ttu-id="b0f47-149">Účtují sběrnice pro úložiště?</span><span class="sxs-lookup"><span data-stu-id="b0f47-149">Does Service Bus charge for storage?</span></span>
<span data-ttu-id="b0f47-150">Ne, sběrnice není účtují pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="b0f47-150">No, Service Bus does not charge for storage.</span></span> <span data-ttu-id="b0f47-151">Je však kvótu omezení maximální množství dat, která můžete nastavit jako trvalý, za fronta nebo téma.</span><span class="sxs-lookup"><span data-stu-id="b0f47-151">However, there is a quota limiting the maximum amount of data that can be persisted per queue/topic.</span></span> <span data-ttu-id="b0f47-152">Najdete v části Nejčastější dotazy pro další.</span><span class="sxs-lookup"><span data-stu-id="b0f47-152">See the next FAQ.</span></span>

## <a name="quotas"></a><span data-ttu-id="b0f47-153">Kvóty</span><span class="sxs-lookup"><span data-stu-id="b0f47-153">Quotas</span></span>

<span data-ttu-id="b0f47-154">Seznam kvót a omezení služby Service Bus, najdete v článku [přehled kvóty Service Bus][Quotas overview].</span><span class="sxs-lookup"><span data-stu-id="b0f47-154">For a list of Service Bus limits and quotas, see the [Service Bus quotas overview][Quotas overview].</span></span>

### <a name="does-service-bus-have-any-usage-quotas"></a><span data-ttu-id="b0f47-155">Má Service Bus žádné kvóty využití?</span><span class="sxs-lookup"><span data-stu-id="b0f47-155">Does Service Bus have any usage quotas?</span></span>
<span data-ttu-id="b0f47-156">Ve výchozím nastavení pro všechny cloudové služby společnosti Microsoft nastaví agregační měsíční využití kvóta, která je vypočtená ve všech předplatných zákazníka.</span><span class="sxs-lookup"><span data-stu-id="b0f47-156">By default, for any cloud service Microsoft sets an aggregate monthly usage quota that is calculated across all of a customer's subscriptions.</span></span> <span data-ttu-id="b0f47-157">Chápeme, bude pravděpodobně vyžadovat více než tato omezení, protože oddělení služeb zákazníkům kdykoli tak, aby jsme pochopení potřeb a odpovídajícím způsobem nastavit tyto limity.</span><span class="sxs-lookup"><span data-stu-id="b0f47-157">Because we understand that you may need more than these limits, please contact customer service at any time so that we can understand your needs and adjust these limits appropriately.</span></span> <span data-ttu-id="b0f47-158">Služba Service Bus kvóty agregační využití je 5 miliardy zpráv za měsíc.</span><span class="sxs-lookup"><span data-stu-id="b0f47-158">For Service Bus, the aggregate usage quota is 5 billion messages per month.</span></span>

<span data-ttu-id="b0f47-159">Když jsme si vyhrazuje právo k zakázání účtu zákazníka, která byla překročena kvóty jeho využití v daném měsíci, poskytujeme bude e-mailová oznámení a provádění více pokusů ke kontaktování zákazníka před provedením jakékoli akce.</span><span class="sxs-lookup"><span data-stu-id="b0f47-159">While we do reserve the right to disable a customer account that has exceeded its usage quotas in a given month, we will provide e-mail notification and make multiple attempts to contact a customer before taking any action.</span></span> <span data-ttu-id="b0f47-160">Zákazníci překročení těchto kvót stále je zodpovědná za poplatky, které překračují kvóty.</span><span class="sxs-lookup"><span data-stu-id="b0f47-160">Customers exceeding these quotas will still be responsible for charges that exceed the quotas.</span></span>

<span data-ttu-id="b0f47-161">Stejně jako u jiných služeb v Azure Service Bus vynucuje sadu konkrétní kvóty pro zajištění správného využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="b0f47-161">As with other services on Azure, Service Bus enforces a set of specific quotas to ensure that there is fair usage of resources.</span></span> <span data-ttu-id="b0f47-162">Můžete najít další podrobnosti o těchto kvót v [přehled kvóty Service Bus][Quotas overview].</span><span class="sxs-lookup"><span data-stu-id="b0f47-162">You can find more details about these quotas in the [Service Bus quotas overview][Quotas overview].</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="b0f47-163">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="b0f47-163">Troubleshooting</span></span>
### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a><span data-ttu-id="b0f47-164">Jaké jsou některé výjimky generované API pro Azure Service Bus a jejich doporučované akce?</span><span class="sxs-lookup"><span data-stu-id="b0f47-164">What are some of the exceptions generated by Azure Service Bus APIs and their suggested actions?</span></span>
<span data-ttu-id="b0f47-165">Seznam možných výjimek Service Bus najdete v tématu [výjimky přehled][Exceptions overview].</span><span class="sxs-lookup"><span data-stu-id="b0f47-165">For a list of possible Service Bus exceptions, see [Exceptions overview][Exceptions overview].</span></span>

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a><span data-ttu-id="b0f47-166">Co je sdíleného přístupového podpisu a jazyky, které podporuje generování podpis?</span><span class="sxs-lookup"><span data-stu-id="b0f47-166">What is a Shared Access Signature and which languages support generating a signature?</span></span>
<span data-ttu-id="b0f47-167">Sdílené přístupové podpisy jsou mechanismus ověřování založené na SHA-256 zabezpečené hodnoty hash nebo identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="b0f47-167">Shared Access Signatures are an authentication mechanism based on SHA – 256 secure hashes or URIs.</span></span> <span data-ttu-id="b0f47-168">Informace o tom, jak generovat vlastní podpisy v uzlu, PHP, Java a C\#, najdete v článku [sdílené přístupové podpisy] [ Shared Access Signatures] článku.</span><span class="sxs-lookup"><span data-stu-id="b0f47-168">For information about how to generate your own signatures in Node, PHP, Java and C\#, see the [Shared Access Signatures][Shared Access Signatures] article.</span></span>

## <a name="subscription-and-namespace-management"></a><span data-ttu-id="b0f47-169">Správa předplatného a obor názvů</span><span class="sxs-lookup"><span data-stu-id="b0f47-169">Subscription and namespace management</span></span>
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a><span data-ttu-id="b0f47-170">Jak migrace oboru do jiného předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="b0f47-170">How do I migrate a namespace to another Azure subscription?</span></span>

<span data-ttu-id="b0f47-171">Můžete přesunout oboru názvů z jedno předplatné do druhého, buď pomocí [portál Azure](https://portal.azure.com) nebo příkazů prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b0f47-171">You can move a namespace from one Azure subscription to another, using either the [Azure portal](https://portal.azure.com) or PowerShell commands.</span></span> <span data-ttu-id="b0f47-172">Aby bylo možné provést operaci, musí být obor názvů již aktivní.</span><span class="sxs-lookup"><span data-stu-id="b0f47-172">In order to execute the operation, the namespace must already be active.</span></span> <span data-ttu-id="b0f47-173">Uživatel provádění příkazů, které musí být správce na zdrojovém i cílovém odběry.</span><span class="sxs-lookup"><span data-stu-id="b0f47-173">The user executing the commands must be an administrator on both the source and target subscriptions.</span></span>

#### <a name="portal"></a><span data-ttu-id="b0f47-174">Portál</span><span class="sxs-lookup"><span data-stu-id="b0f47-174">Portal</span></span>

<span data-ttu-id="b0f47-175">Portál Azure pro migraci oborů názvů Service Bus do jiného předplatného, postupujte podle pokynů [zde](../azure-resource-manager/resource-group-move-resources.md#use-portal).</span><span class="sxs-lookup"><span data-stu-id="b0f47-175">To use the Azure portal to migrate Service Bus namespaces to another subscription, follow the directions [here](../azure-resource-manager/resource-group-move-resources.md#use-portal).</span></span> 

#### <a name="powershell"></a><span data-ttu-id="b0f47-176">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b0f47-176">PowerShell</span></span>

<span data-ttu-id="b0f47-177">Následující sekvence příkazů prostředí PowerShell přesune do jiného oboru názvů z jedno předplatné.</span><span class="sxs-lookup"><span data-stu-id="b0f47-177">The following sequence of PowerShell commands moves a namespace from one Azure subscription to another.</span></span> <span data-ttu-id="b0f47-178">K provedení této operace, obor názvů musí již být aktivní, a uživatel, který spouští příkazy prostředí PowerShell musí být správce na zdrojovém i cílovém odběry.</span><span class="sxs-lookup"><span data-stu-id="b0f47-178">To execute this operation, the namespace must already be active, and the user running the PowerShell commands must be an administrator on both the source and target subscriptions.</span></span>

```powershell
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a><span data-ttu-id="b0f47-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b0f47-179">Next steps</span></span>
<span data-ttu-id="b0f47-180">Pokud se o službě Service Bus chcete dozvědět víc, pročtěte si následující témata.</span><span class="sxs-lookup"><span data-stu-id="b0f47-180">To learn more about Service Bus, see the following topics.</span></span>

* [<span data-ttu-id="b0f47-181">Představení Azure Service Bus Premium (příspěvek na blogu)</span><span class="sxs-lookup"><span data-stu-id="b0f47-181">Introducing Azure Service Bus Premium (blog post)</span></span>](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [<span data-ttu-id="b0f47-182">Představení služby Azure Service Bus Premium (Channel9)</span><span class="sxs-lookup"><span data-stu-id="b0f47-182">Introducing Azure Service Bus Premium (Channel9)</span></span>](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [<span data-ttu-id="b0f47-183">Přehled služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="b0f47-183">Service Bus overview</span></span>](service-bus-messaging-overview.md)
* [<span data-ttu-id="b0f47-184">Přehled architektury služby Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="b0f47-184">Azure Service Bus architecture overview</span></span>](service-bus-fundamentals-hybrid-solutions.md)
* [<span data-ttu-id="b0f47-185">Začínáme s frontami služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="b0f47-185">Get started with Service Bus queues</span></span>](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md