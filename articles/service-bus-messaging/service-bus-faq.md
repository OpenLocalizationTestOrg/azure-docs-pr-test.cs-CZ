---
title: "aaaAzure Service Bus nejčastější dotazy (FAQ) | Microsoft Docs"
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
ms.openlocfilehash: 71fe9eac46647a3e4026dbcaf2196984dd4b6a44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-faq"></a>Nejčastější dotazy k Service Bus
Tento článek obsahuje odpovědi na některé nejčastější dotazy týkající se služby Microsoft Azure Service Bus. Můžete také navštívit hello [podporu nejčastější dotazy k Azure](http://go.microsoft.com/fwlink/?LinkID=185083) obecné Azure – ceny a podporu informace.

## <a name="general-questions-about-azure-service-bus"></a>Obecné otázky o Azure Service Bus
### <a name="what-is-azure-service-bus"></a>Co je Azure Service Bus?
[Azure Service Bus](service-bus-messaging-overview.md) je asynchronní zasílání zpráv Cloudová platforma, která vám umožní toosend data mezi odpojeného systémy. Společnost Microsoft nabízí tato funkce jako služba, což znamená, že není nutné toohost některý vlastní hardware v pořadí toouse ho.

### <a name="what-is-a-service-bus-namespace"></a>Co je obor názvů Service Bus?
A [obor názvů](service-bus-create-namespace-portal.md) poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace. Vytvořit je nezbytné toouse Service Bus a jeden z hello bude první kroky v části Začínáme.

### <a name="what-is-an-azure-service-bus-queue"></a>Co je fronty Azure Service Bus?
A [fronty Service Bus](service-bus-queues-topics-subscriptions.md) je entita, ve kterém jsou uložené zprávy. Fronty jsou užitečné, když máte více aplikací, nebo z několika součástí distribuované aplikace vyžadující toocommunicate mezi sebou. Hello fronty se podobně jako centra distribuce tooa, že více produktů (zpráv) jsou přijata a pak se odešle z tohoto umístění.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Co jsou témata a předplatné Azure Service Bus?
Téma můžete vizualizovat jako fronty a při použití více předplatných, začne bohatší zasílání zpráv modelu; v podstatě nástroj na více komunikace. Tento model publikování a přihlášení k odběru (nebo *pub nebo sub*) umožňuje aplikaci, která odešle téma tooa zpráva s více odběry toohave tuto zprávu přijatých více aplikací.

### <a name="what-is-a-partitioned-entity"></a>Co je dělené entity?
Konvenční fronta nebo téma zpracování zprostředkovatelem jedné zprávy a uloženy v úložišti jeden zasílání zpráv. A [oddílů fronta nebo téma](service-bus-partitioning.md) zpracovává více zpráv zprostředkovatelé a uložená v úložištích více zasílání zpráv. To znamená, že hello celkovou propustnost oddílů fronta nebo téma je již omezena hello výkonu zprostředkovatele jedné zprávy nebo úložišti pro přenos zpráv. Kromě toho dočasnému výpadku zasílání zpráv úložiště nevykresluje oddílů fronta nebo téma není k dispozici.

Všimněte si, že řazení není zajištěna při použití rozdělení entity. V případě hello oddíl je k dispozici můžete i nadále odesílat a přijímat zprávy z hello oddíly.

## <a name="best-practices"></a>Osvědčené postupy
### <a name="what-are-some-azure-service-bus-best-practices"></a>Jaké jsou některé z osvědčených postupů Azure Service Bus?
* [Osvědčené postupy pro zlepšení výkonu pomocí služby Service Bus] [ Best practices for performance improvements using Service Bus] – Tento článek popisuje, jak toooptimize výkon při výměně zpráv.

### <a name="what-should-i-know-before-creating-entities"></a>Co je třeba vědět před vytvořením entity?
Následující vlastnosti frontu a téma Hello se nedá změnit. Proveďte to v úvahu při zřizování vaší entity jako to nemůže být upraven, bez vytvoření nové entity nahrazení.

* Velikost
* Dělení
* Relace
* Detekce duplicitních
* Expresní entity

## <a name="pricing"></a>Ceny
Tato část odpovídá na některé často kladené otázky týkající se Cenová struktura hello Service Bus.

Hello [Service Bus ceny a fakturace](service-bus-pricing-billing.md) článek vysvětluje hello fakturace měřidla v Service Bus a informace o cenách možností Service Bus, najdete v části [Service Bus podrobnosti o cenách](https://azure.microsoft.com/pricing/details/service-bus/).

Můžete také navštívit hello [Azure podporu nejčastější dotazy k](http://go.microsoft.com/fwlink/?LinkID=185083) pro obecné Azure informace o cenách. 

### <a name="how-do-you-charge-for-service-bus"></a>Jak se vám účtují pro Service Bus?
Úplné informace o cenách služby Service Bus, najdete v tématu [Service Bus podrobnosti o cenách][Pricing overview]. Kromě toho toohello ceny uvedené, budou se vám účtovat pro přenosy přidružená data o sazbách za odchozí mimo hello datového centra, ve kterém je zajištěna vaší aplikace.

### <a name="what-usage-of-service-bus-is-subject-toodata-transfer-what-is-not"></a>Jaké využití služby Service Bus je subjektu toodata přenosu? Co není?
Bez poplatků, stejně tak i všechny příchozí přenosy dat je k dispozici žádné přenos dat v dané oblasti Azure. Přenos dat mimo oblast je subjektu tooegress poplatky, která je k dispozici [zde](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="does-service-bus-charge-for-storage"></a>Účtují sběrnice pro úložiště?
Ne, sběrnice není účtují pro úložiště. Je však kvóty limitující hello maximální množství dat, která můžete nastavit jako trvalý, za fronta nebo téma. Najdete v části Nejčastější dotazy týkající se další hello.

## <a name="quotas"></a>Kvóty

Seznam kvót a omezení služby Service Bus, najdete v části hello [přehled kvóty Service Bus][Quotas overview].

### <a name="does-service-bus-have-any-usage-quotas"></a>Má Service Bus žádné kvóty využití?
Ve výchozím nastavení pro všechny cloudové služby společnosti Microsoft nastaví agregační měsíční využití kvóta, která je vypočtená ve všech předplatných zákazníka. Chápeme, bude pravděpodobně vyžadovat více než tato omezení, protože oddělení služeb zákazníkům kdykoli tak, aby jsme pochopení potřeb a odpovídajícím způsobem nastavit tyto limity. Služba Service Bus je hello agregační využití kvóty 5 miliardy zpráv za měsíc.

Když jsme rezervovat hello správné toodisable účtu zákazníka, která byla překročena kvóty jeho využití v daném měsíci, jsme se zadejte e-mailová oznámení a proveďte více pokusů toocontact zákazníka před provedením jakékoli akce. Zákazníci překročení těchto kvót stále je zodpovědná za poplatky, které překračují hello kvóty.

Stejně jako u jiných služeb v Azure Service Bus vynucuje sadu tooensure konkrétní kvóty se správného využití prostředků. Můžete najít další podrobnosti o těchto kvót v hello [přehled kvóty Service Bus][Quotas overview].

## <a name="troubleshooting"></a>Řešení potíží
### <a name="what-are-some-of-hello-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Jaké jsou některé z hello výjimky generované API pro Azure Service Bus a jejich doporučované akce?
Seznam možných výjimek Service Bus najdete v tématu [výjimky přehled][Exceptions overview].

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Co je sdíleného přístupového podpisu a jazyky, které podporuje generování podpis?
Sdílené přístupové podpisy jsou mechanismus ověřování založené na SHA-256 zabezpečené hodnoty hash nebo identifikátory URI. Informace o toogenerate vlastní podpisy v uzlu, PHP, Java a C\#, najdete v části hello [sdílené přístupové podpisy] [ Shared Access Signatures] článku.

## <a name="subscription-and-namespace-management"></a>Správa předplatného a obor názvů
### <a name="how-do-i-migrate-a-namespace-tooanother-azure-subscription"></a>Jak mohu migrovat obor názvů tooanother předplatné Azure?

Obor názvů můžete přesunout z jednoho tooanother předplatné Azure, pomocí buď hello [portál Azure](https://portal.azure.com) nebo příkazů prostředí PowerShell. V pořadí tooexecute hello operaci musí být obor názvů hello již aktivní. uživatel Hello provádění příkazů hello musí mít oprávnění správce na obou hello zdrojové a cílové předplatné.

#### <a name="portal"></a>Portál

toouse hello portálu toomigrate Azure Service Bus obory názvů tooanother předplatného, postupujte podle pokynů hello [zde](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

Hello následující sekvence příkazů prostředí PowerShell přesune oboru názvů z jednoho tooanother předplatného Azure. tooexecute této operace, obor názvů hello už musí být aktivní a hello uživatel, který spouští příkazy prostředí PowerShell hello musí být správcem na oba odběry zdrojové a cílové hello.

```powershell
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription tootarget subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Další kroky
toolearn víc o službě Service Bus, najdete v následujících tématech hello.

* [Představení Azure Service Bus Premium (příspěvek na blogu)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Představení služby Azure Service Bus Premium (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Přehled služby Service Bus](service-bus-messaging-overview.md)
* [Přehled architektury služby Azure Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Začínáme s frontami služby Service Bus](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md
