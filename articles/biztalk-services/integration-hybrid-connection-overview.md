---
title: "Přehled připojení aaaHybrid | Microsoft Docs"
description: "Získejte informace o hybridních připojeních, zabezpečení, portech TCP a podporovaných konfiguracích. MABS, WABS."
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: erikre
editor: 
ms.assetid: 216e4927-6863-46e7-aa7c-77fec575c8a6
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: f092c6019aae761e1e73f13d1af8446a896515c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hybrid-connections-overview"></a>Přehled hybridních připojení

> [!IMPORTANT]
> Hybridní připojení BizTalk jsou vyřazena z provozu a nahrazena hybridními připojeními App Service. Další informace, včetně jak toomanage vaše stávající hybridní připojení BizTalk, najdete v části [Azure App Service hybridní připojení](../app-service/app-service-hybrid-connections.md).

Úvod tooHybrid připojení, uvádí hello podporované konfigurace a seznamy hello požadované porty TCP.

## <a name="what-is-a-hybrid-connection"></a>Co je hybridní připojení
Hybridní připojení patří mezi funkce služby Azure BizTalk Services. Hybridní připojení poskytují snadný a pohodlný způsob tooconnect hello webových aplikací funkci ve službě Azure App Service (dříve Websites) a hello funkce Mobile Apps v Azure App Service (dříve Mobile Services) tooon místním prostředkům za vaší bránou firewall.

![Hybridní připojení][HCImage]

Některé z výhod hybridních připojení:

* Funkce Web Apps a Mobile Apps můžou získat zabezpečený přístup k místním datům a službám.
* Více Web Apps nebo Mobile Apps můžete sdílet tooaccess hybridní připojení místnímu prostředku.
* Minimální počet portů TCP jsou požadované tooaccess vaší sítě.
* Aplikace, které používají hybridní připojení přístup jenom konkrétní místnímu prostředku hello, která je publikovaná pomocí hello hybridní připojení.
* Můžete připojit tooany místnímu prostředku, který používá statický port TCP, jako je například SQL Server, MySQL, webové rozhraní API HTTP a většina vlastních webových služeb.
  
  > [!NOTE]
  > V současné době není dostupná podpora služeb založených na protokolu TCP, které používají dynamické porty (jako je třeba pasivní režim FTP nebo rozšířený pasivní režim). Kromě toho není dostupná podpora protokolu LDAP. LDAP sice používá statický port TCP, ale může používat i protokol UDP. Z toho důvodu se nepodporuje.
  > 
  > 
* Dají se použít u všech rozhraní podporovaných funkcí Web Apps (.NET, PHP, Java, Python, Node.js) a Mobile Apps (Node.js, .NET).
* Funkce Web Apps a Mobile Apps můžete přístup k prostředkům místně v přesně hello stejným způsobem, jako kdyby hello Web nebo mobilní aplikace se nachází v místní síti. Například hello stejný připojovací řetězec použitý místní lze také v Azure.

Hybridní připojení také poskytují podnikovým správcům kontrolu a přehled hello podnikovým prostředkům získat přístup hybridní aplikace, včetně:

* Pomocí nastavení zásad skupiny, Správce může povolit hybridní připojení v síti hello a také určit prostředky, které můžete získat přístup hybridní aplikace.
* Protokoly událostí a auditu v podnikové síti hello poskytují přehled o prostředky hello přistupují hybridní připojení.

## <a name="example-scenarios"></a>Ukázkové scénáře
Hybridní připojení podporují následující kombinace rozhraní a aplikací hello:

* Rozhraní .NET framework přístup tooSQL serveru
* Rozhraní .NET framework přístup tooHTTP a HTTPS služby s webovým klientem přes
* PHP přístup tooSQL Server, MySQL
* Java přístup tooSQL Server, MySQL a Oracle
* Služba tooHTTP nebo HTTPS přístup Java

Když pomocí hybridních připojení tooaccess místní systém SQL Server, zvažte následující hello:

* Pojmenované instance SQL Express musí být nakonfigurované toouse statických portů. Ve výchozím nastavení používají pojmenované instance SQL Express dynamické porty.
* Výchozí instance SQL Express používají statický port, ale musí být povolený protokol TCP. Ve výchozím nastavení není protokol TCP povolený.
* Při použití clusteringu nebo skupin dostupnosti, hello `MultiSubnetFailover=true` režimu se momentálně nepodporuje.
* Hello `ApplicationIntent=ReadOnly` není aktuálně podporována.
* Ověřování SQL může být požadováno jako metoda autorizace začátku do konce hello nepodporuje hello aplikací Azure a hello místní SQL server.

## <a name="security-and-ports"></a>Zabezpečení a porty
Hybridní připojení pomocí sdíleného přístupového podpisu (SAS) autorizace toosecure hello připojení z hello Azure aplikace a hello místní správce hybridního připojení toohello hybridní připojení. Oddělené klíče připojení jsou vytvořeny pro aplikace hello a hello místní správce hybridního připojení. Tyto klíče připojení se dají nezávisle převracet a odvolávat.

Hybridní připojení poskytují hladkou a zabezpečenou distribuci aplikací toohello hello klíče a hello místní správce hybridního připojení.

Další informace najdete v článku [Vytváření a správa hybridních připojení](integration-hybrid-connection-create-manage.md).

*Autorizace aplikací probíhá odděleně od hybridního připojení hello*. Dá se použít libovolná vhodná autorizační metoda. Hello metoda autorizace závisí na metodách autorizace začátku do konce hello podporovány v rámci hello cloudu Azure a místní komponenty hello. Vaše aplikace Azure může třeba získávat přístup k místnímu SQL Serveru. V tomto scénáři může být SQL autorizační metoda autorizace hello je podporované začátku do konce.

#### <a name="tcp-ports"></a>Porty TCP
Hybridní připojení vyžadují jenom odchozí připojení TCP nebo HTTP z vaší privátní sítě. Není nutné tooopen žádné porty brány firewall nebo změňte tooallow konfigurace vaší sítě hraniční příchozí připojení do vaší sítě.

Hybridní připojení používá následující porty TCP Hello:

| Port | Proč to potřebujete |
| --- | --- |
| 9350–9354 |Tyto porty se používají k přenosům dat. Správce předávání přes Service Bus Hello sondy port 9350 toodetermine, pokud je dostupné připojení TCP. Pokud ano, správce předpokládá, že je dostupný i port 9352. Přenos dat prochází přes port 9352. <br/><br/>Povolte odchozí připojení toothese porty. |
| 5671 |Pokud se pro přenos dat používá port 9352, port 5671 slouží jako řídicí kanál hello. <br/><br/>Povolte odchozí připojení toothis portu. |
| 80, 443 |Tyto porty se používají pro některé požadavky tooAzure data. Také pokud porty 9352 a 5671 nejsou použitelné, *pak* jsou porty 80 a 443 pro přenos dat a hello řídicí kanál použijí záložní porty hello.<br/><br/>Povolte odchozí připojení toothese porty. <br/><br/>**Poznámka:** není doporučeno toouse, tyto jako záložní porty místo hello hello jiné porty TCP. Hello HTTP/WebSocket je použit protokol hello místo nativního protokolu TCP pro datové kanály. To může způsobit snížení výkonu. |

## <a name="next-steps"></a>Další kroky
[Vytváření a správa hybridních připojení](integration-hybrid-connection-create-manage.md)<br/>
[Připojení Azure Web Apps tooan místnímu prostředku](../app-service-web/web-sites-hybrid-connection-get-started.md)<br/>
[Připojit tooon místnímu SQL serveru z webové aplikace Azure](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/>

## <a name="see-also"></a>Viz také
[Rozhraní API REST pro správu služby BizTalk Services v Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)
[BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md)<br/>
[Vytvoření služby BizTalk pomocí webu Azure Portal](biztalk-provision-services.md)<br/>
[BizTalk Services: Karty Řídicí panel, Sledování a Škálování](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png
