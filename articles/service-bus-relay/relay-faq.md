---
title: "Nejčastější dotazy k předávání aaaAzure | Microsoft Docs"
description: "Získejte odpovědi toosome nejčastější dotazy o předávání přes Azure."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 886d2c7f-838f-4938-bd23-466662fb1c8e
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: ab14431e27df43287940e7d12ea37e4093638d21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-relay-faqs"></a>Nejčastější dotazy k Azure předávání

Tento článek obsahuje odpovědi na některé nejčastější dotazy (FAQ) o [předávání přes Azure](https://azure.microsoft.com/services/service-bus/). Obecné Azure – ceny a podporu informace najdete v tématu [podporu nejčastější dotazy k Azure](http://go.microsoft.com/fwlink/?LinkID=185083).

## <a name="general-questions"></a>Obecné otázky
### <a name="what-is-azure-relay"></a>Co je Azure Relay?
Hello [Azure předávací služba](relay-what-is-it.md) usnadňuje hybridní aplikace tím, že vám pomůžeme bezpečněji zveřejněte služby, které se nacházejí v podnikové síti toohello veřejného cloudu. Můžete vystavit hello služby, aniž byste museli otevřít spojení ve firewallu a bez nutnosti nežádoucí změny tooa podnikové síťové infrastruktury.

### <a name="what-is-a-relay-namespace"></a>Co je předávání názvů?
A [obor názvů](relay-create-namespace-portal.md) je kontejner oboru, které můžete použít tooaddress předávání prostředky v rámci vaší aplikace. Je potřeba vytvořit obor názvů toouse předávání. Toto je jedna z hello první kroky v části Začínáme.

### <a name="what-happened-tooservice-bus-relay-service"></a>Jaké happened tooService service Bus Relay?
Hello dříve s názvem služby předávání přes Service Bus se nyní označuje jako přenosový WCF. Můžete pokračovat v toouse této služby jako obvykle. Funkce hybridní připojení Hello je aktualizovaná verze služby, která je byla transplantované ze služby Azure BizTalk Services. Předávání WCF a hybridní připojení pokračovat toobe podporována.

## <a name="pricing"></a>Ceny
Tato část odpovědi některé nejčastější dotazy o hello předávání cenové struktury. Můžete také zobrazit [Azure podporu nejčastější dotazy k](http://go.microsoft.com/fwlink/?LinkID=185083) pro obecné Azure informace o cenách. Úplné informace o cenách předávání najdete v tématu [Service Bus podrobnosti o cenách][Pricing overview].

### <a name="how-do-you-charge-for-hybrid-connections-and-wcf-relay"></a>Jak se vám účtují pro hybridní připojení a přenosu WCF?
Úplné informace o cenách předávání najdete v tématu hello [hybridní připojení a předávací službu WCF] [ Pricing overview] tabulky na stránce s cenami podrobnosti hello Service Bus. Kromě toho toohello ceny uvedené na této stránce, budou se vám účtovat pro přenosy přidružená data o sazbách za odchozí mimo hello datacenter, ve kterém je zajištěna vaší aplikace.

### <a name="how-am-i-billed-for-hybrid-connections"></a>Jak se fakturuje pro hybridní připojení?
Tady jsou tři scénáře fakturace příklad pro hybridní připojení:

*   Scénář 1:
    *   Máte jeden naslouchací proces, jako je například instanci hello správce hybridního připojení nainstalována a spuštěna nepřetržitě pro celý měsíc hello.
    *   Odešlete 3 GB dat napříč hello připojení v měsíci hello. 
    *   Celkový poplatek je 5.
*   Scénář 2:
    *   Máte jeden naslouchací proces, jako je například instanci hello správce hybridního připojení nainstalována a spuštěna nepřetržitě pro celý měsíc hello.
    *   10 GB dat, je poslat hello připojení v měsíci hello.
    *   Celkový poplatek je 7,50 $. Prvních 5 GB + 2.50 pro hello dalších 5 GB dat, je 5 pro hello připojení.
*   Scénář 3:
    *   Máte dvě instance, A a B, hello správce hybridního připojení nainstalována a spuštěna nepřetržitě pro celý měsíc hello.
    *   Odešlete 3 GB dat napříč připojení A v měsíci hello.
    *   Odešlete 6 GB dat v měsíci hello připojení B.
    *   Celkový poplatek je $10.50. To je 5 pro připojení A + 5 pro připojení B + 0,50 (pro hello šesté gigabajt připojení B).

Upozorňujeme, že jsou použité v ukázkách hello hello ceny platí pouze během období preview hello hybridní připojení. Ceny jsou subjektu toochange při obecné dostupnosti hybridní připojení.

### <a name="how-are-hours-calculated-for-relay"></a>Jak jsou vypočítávány hodin pro předávání?

Předávání WCF je k dispozici pouze v oborech názvů úrovně Standard. Ceny a [připojení kvóty](../service-bus-messaging/service-bus-quotas.md) pro předávání, jinak hodnota nezměnily. To znamená, že předávací pokračovat, toobe účtovat na základě počtu hello zprávy (není operace) a předávání hodin. Další informace najdete v tématu hello ["Hybridní připojení a WCF předávací službu"](https://azure.microsoft.com/pricing/details/service-bus/) tabulky na stránce s podrobnostmi o cenách hello.

### <a name="what-if-i-have-more-than-one-listener-connected-tooa-specific-relay"></a>Co když máte více než jeden naslouchací proces připojené tooa konkrétní předávací službu?
V některých případech může mít jeden předávání více připojených naslouchací procesy. Přenos je považováno za otevřené, připojené tooit při předávání alespoň jeden naslouchací proces. Přidání naslouchací procesy tooan otevřete předávání výsledky do hodiny další přenosu. Hello číslo relé odesílatelé (klientů, které vyvolají nebo odesílání zprávy toorelays), které jsou připojené tooa předávání neovlivňuje hello výpočtu hodiny přenosu.

### <a name="how-is-hello-messages-meter-calculated-for-wcf-relays"></a>Výpočtu hello zpráv měření pro předávací službu WCF
(**To se týká pouze tooWCF předávání. Zprávy nejsou náklady pro hybridní připojení.** )

Obecně platí, fakturovatelný zprávy pro předávání se počítají pomocí hello stejné metody, která se používá pro zprostředkované entity (fronty, témata a odběry), popsané. Existují však určité významné rozdíly.

Odesílání předávání přes Service Bus tooa zprávy je zpracovaná jako "úplná prostřednictvím" Odeslat naslouchací proces předávání toohello, který přijme zprávu hello. Nepovažuje se za odeslání operace toohello předávání přes Service Bus, následuje naslouchací proces předávání toohello doručení. Volání služby požadavku a odpovědi styl (z až too64 KB) proti výsledků naslouchací proces předávání ve dvou fakturovatelný zpráv: jednu fakturovatelný zprávu pro žádost o hello a jednu fakturovatelný zprávu pro odpověď hello (za předpokladu, že odpověď hello je také 64 KB nebo menší). To se liší od používání fronty toomediate mezi klientem a služby. Pokud používáte fronty toomediate mezi klientem a služby, hello stejného vzoru požadavku a odpovědi vyžaduje toohello fronty požadavků odesílání, následuje dequeue nebo doručení z hello fronty toohello služby. Následují fronty tooanother odeslání odpovědi a dequeue nebo doručení z této fronty toohello klienta. Pomocí hello stejná velikost předpoklady v rámci (až too64 KB), hello zprostředkovaného vzor fronty za následek 4 fakturovatelný zprávy. Bude platit pro dvakrát hello počet hello tooimplement zprávy, které stejný vzor, můžete provést pomocí předávání. Samozřejmě existují výhody toousing fronty tooachieve tento vzor, jako je například odolnost a vyrovnávání zatížení. Tyto výhody může justify hello další náklady.

Předávání, které jsou otevřené pomocí hello **netTCPRelay** vazby WCF považovat zprávy, ne jako jednotlivé zprávy, ale jako datový proud dat předávaných mezi hello systému. Pokud použijete tuto vazbu, hello odesílatele a naslouchací proces mít pouze přehled hello rámcovacích hello jednotlivých zpráv odeslané a přijaté. Pro předávání, které používají hello **netTCPRelay** vazby, všechna data je považován za datového proudu pro výpočet fakturovatelný zprávy. V takovém případě Service Bus vypočítá hello celkové množství dat odesílané nebo přijímané prostřednictvím každé jednotlivé předávání na základě 5 minut. Potom se vydělí celkové množství dat číslem hello toodetermine 64 KB fakturovatelný zpráv pro tuto předávání během tohoto časového období.

## <a name="quotas"></a>Kvóty
| Název kvóty | Rozsah | Typ | Chování při překročení | Hodnota |
| --- | --- | --- | --- | --- |
| Souběžné moduly pro naslouchání na přenos |Entita |Statická |Odeslání dalších žádostí o další připojení odmítnuty a přijme výjimku hello volání kódu. |25 |
| Souběžné předávání – moduly naslouchání |Systémové |Statická |Odeslání dalších žádostí o další připojení odmítnuty a přijme výjimku hello volání kódu. |2,000 |
| Připojení souběžných předávání za všechny koncové body předávání v oboru názvů služby |Systémové |Statická |- |5,000 |
| Předávání koncových bodů na jeden obor názvů služby |Systémové |Statická |- |10 000 |
| Velikost zprávy [NetOnewayRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.netonewayrelaybinding.aspx) a [NetEventRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.neteventrelaybinding.aspx) předává |Systémové |Statická |Odmítne příchozí zprávy, které překračují těchto kvót a přijme výjimku hello volání kódu. |64 kB |
| Velikost zprávy [HttpRelayTransportBindingElement](https://msdn.microsoft.com/library/microsoft.servicebus.httprelaytransportbindingelement.aspx) a [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) předává |Systémové |Statická |- |Unlimited |

### <a name="does-relay-have-any-usage-quotas"></a>Má předávání žádné kvóty využití?
Ve výchozím nastavení pro všechny cloudové služby společnosti Microsoft nastaví agregační měsíční využití kvóta, která je vypočtená ve všech předplatných zákazníka. Chápeme, že v některých případech potřeb může tato omezení překročí. Kontaktovat oddělení služeb zákazníkům kdykoli, proto jsme pochopení potřeb a odpovídajícím způsobem nastavit tyto limity. Hello agregační využití kvóty pro Service Bus, jsou následující:

* miliardy 5 zprávy
* hodiny přenosu milionů 2

I když jsme rezervovat hello správné toodisable účtu, který překračuje jeho měsíční využití kvóty, poskytujeme e-mailová oznámení a před provedením jakékoli akce uděláme více pokusů toocontact hello zákazníka. Zákazníci, které překračují těchto kvót jsou stále zodpovědní za nadbytečné poplatky.

### <a name="naming-restrictions"></a>Omezení pojmenování
Název oboru názvů předávání musí být mezi 6 až 50 znaků.

## <a name="subscription-and-namespace-management"></a>Správa předplatného a obor názvů
### <a name="how-do-i-migrate-a-namespace-tooanother-azure-subscription"></a>Jak mohu migrovat obor názvů tooanother předplatné Azure?

toomove oboru názvů z jedno předplatné tooanother předplatné Azure, můžete buď použít hello [portál Azure](https://portal.azure.com) nebo použít příkazy prostředí PowerShell. toomove předplatné tooanother obor názvů, obor názvů hello musí již být aktivní. uživatel s oprávněním správce na oba hello zdrojové a cílové odběry musí být Hello uživatel příkazů hello.

#### <a name="azure-portal"></a>portál Azure

toouse hello obory názvů předávání přes Azure Azure portálu toomigrate z předplatného tooanother jedno předplatné, najdete v části [přesunout prostředky tooa novou skupinu prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

prostředí PowerShell toomove toouse oboru názvů z předplatného tooanother jedno předplatné, použijte hello následující sekvence příkazů. tooexecute této operace, obor názvů hello už musí být aktivní a hello uživatel, který spouští příkazy prostředí PowerShell hello musí být uživatel s oprávněním správce na oba odběry zdrojové a cílové hello.

```powershell
# Create a new resource group in hello target subscription.
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move hello namespace from hello source subscription toohello target subscription.
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="troubleshooting"></a>Řešení potíží
### <a name="what-are-some-of-hello-exceptions-generated-by-azure-relay-apis-and-suggested-actions-you-can-take"></a>Co jsou některé hello výjimky generované Azure předávání přes rozhraní API a doporučené postupy, které můžete provést?
Popis běžné výjimky a doporučované akce můžete provést, najdete v části [předávání výjimky][Relay exceptions].

### <a name="what-is-a-shared-access-signature-and-which-languages-can-i-use-toogenerate-a-signature"></a>Co je sdílený přístupový podpis a jazyky, které je můžete používat toogenerate podpis?
Podpisy sdíleného přístupu (SAS) se mechanismus ověřování na základě zabezpečeného hodnoty hash SHA-256 nebo identifikátory URI. Informace o tom, jak toogenerate vlastní podpisy v uzlu, PHP, Java, C a C#, najdete v části [ověření sběrnice s podpisy sdíleného přístupu][Shared Access Signatures].

### <a name="is-it-possible-toowhitelist-relay-endpoints"></a>Je možné toowhitelist předávání koncové body?
Ano. Hello předávání klient vytvoří připojení služby předávání přes Azure toohello pomocí plně kvalifikovaných názvů domény. Zákazníci můžete přidat záznam pro `*.servicebus.windows.net` na brány firewall, které podporují vytvoření seznamu povolených DNS.

## <a name="next-steps"></a>Další kroky
* [Vytvoření oboru názvů](relay-create-namespace-portal.md)
* [Začínáme s .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Začínáme s aplikací Node](relay-hybrid-connections-node-get-started.md)

[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Relay exceptions]: relay-exceptions.md
[Shared access signatures]: ../service-bus-messaging/service-bus-sas.md