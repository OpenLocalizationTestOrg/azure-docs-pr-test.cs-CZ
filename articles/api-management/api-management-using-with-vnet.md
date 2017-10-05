---
title: "Jak používat Azure API Management s virtuálními sítěmi"
description: "Zjistěte, jak nastavit připojení k virtuální síti ve webové služby Azure API Management a přístup přes něj."
services: api-management
documentationcenter: 
author: antonba
manager: erikre
editor: 
ms.assetid: 64b58f7b-ca22-47dc-89c0-f6bb0af27a48
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 268cee739188d81feffc36ac07fcdfa18ff95a4d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-azure-api-management-with-virtual-networks"></a>Jak používat Azure API Management s virtuálními sítěmi
Virtuální sítě Azure (virtuální sítě) umožňují některé z vašich prostředků Azure umístění v síti routeable Internetu jiných výrobců, která můžete řídit přístup ke. Tyto sítě můžete pak připojené k vaší místní sítě pomocí různých technologií sítě VPN. Další informace o virtuálních sítí Azure začínat zde uvedené informace: [Přehled virtuálních sítí Azure](../virtual-network/virtual-networks-overview.md).

Azure API Management se dá nasadit ve virtuální síti (VNET), takže má přístup k back-end služby v rámci sítě. Portál pro vývojáře a Brána rozhraní API, lze nakonfigurovat byla přístupná z Internetu nebo pouze v rámci virtuální sítě.

> [!NOTE]
> Azure API Management podporuje classic a Azure Resource Manager virtuálních sítí patřících.
>
>

## <a name="enable-vpn"></a>Připojení povolte virtuální sítě
> [!NOTE]
> Připojení k virtuální síti je k dispozici v **Premium** a **vývojáře** vrstev. Chcete-li přepnout mezi vrstvami, otevřete služby API Management na portálu Azure a pak otevřete **škálování a ceny** kartě. V části **cenová úroveň** , vyberte úroveň Premium nebo Developer a klikněte na tlačítko Uložit.
>

Pokud chcete povolit připojení virtuální sítě, otevřete služby API Management ve službě Azure portálu a otevřete **virtuální síť** stránky.

![Virtuální síť nabídky služby API Management][api-management-using-vnet-menu]

Vyberte typ požadovaný přístup:

* **Externí**: portál brány a vývojáře služby API Management jsou přístupné z veřejného Internetu prostřednictvím externím vyrovnáváním zatížení. Brány mají přístup k prostředkům v rámci virtuální sítě.

![Veřejný partnerský vztah][api-management-vnet-public]

* **Interní**: rozhraní API brány a vývojáře portálu pro správu jsou k dispozici pouze v rámci virtuální sítě prostřednictvím interní nástroj. Brány mají přístup k prostředkům v rámci virtuální sítě.

![Soukromý partnerský vztah][api-management-vnet-private]

Zobrazí seznam všech oblastech, kde je zřízený služby API Management. Vyberte virtuální síť a podsíť pro každou oblast. Naplnění seznamu se classic i Resource Manager virtuální sítě k dispozici v rámci vašich předplatných Azure, které jsou nastavení v oblasti, který konfigurujete.

> [!NOTE]
> **Koncový bod služby** v diagramu zahrnuje brány nebo Proxy, portál vydavatele, portál pro vývojáře, GIT a přímé koncový bod správy.
> **Koncový bod správy** v diagramu je koncový bod hostované na službu, kterou chcete spravovat konfiguraci prostřednictvím portálu Azure a prostředí Powershell.
> Mějte také na paměti, že přestože diagram znázorňuje IP adresy pro své různé koncové body služby API Management **pouze** odpoví na své nakonfigurované názvy hostitelů.

> [!IMPORTANT]
> Pokud nasazujete instanci Azure API Management k virtuální síti Resource Manager, služba musí být ve vyhrazené podsíť, která obsahuje žádné další prostředky s výjimkou instance Azure API Management. Pokud je proveden pokus o nasazení instance služby Azure API Management na podsíť virtuální sítě Resource Manageru obsahující další prostředky, instalace se nezdaří.
>
>

![Vyberte síť VPN][api-management-setup-vpn-select]

Klikněte na tlačítko **Uložit** v horní části obrazovky.

> [!NOTE]
> Adresa VIP instanci služby API Management se změní pokaždé, když virtuální sítě je povolený nebo zakázaný.  
> Adresa VIP změní také při API Management se přesune ze **externí** k **interní** nebo naopak
>


> [!IMPORTANT]
> Když odeberete API Management z virtuální sítě nebo změnit, které je nasazena v, může zůstat použitých VNET uzamčeném až 4 hodiny. Během této doby nebude možné odstranit virtuální sítě nebo nasazení nového prostředku do ní.

## <a name="enable-vnet-powershell"></a>Připojení VNET povolit pomocí rutin prostředí PowerShell
Můžete také povolit připojení virtuální sítě pomocí rutin prostředí PowerShell

* **Vytvoření služby API Management uvnitř virtuální sítě**: použijte rutinu [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) k vytvoření služby Azure API Management uvnitř virtuální sítě.

* **Nasazení služby API Management existující uvnitř virtuální sítě**: použijte rutinu [aktualizace AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) přesunout existující službu Azure API Management uvnitř virtuální sítě.

## <a name="connect-vnet"></a>Připojení k webové službě hostované v rámci virtuální sítě
Po služby API Management je připojen k virtuální síti, je přístup ke službám back-end v něm nejsou jiné než přístup ke službám veřejné. Jednoduše zadejte místní IP adresu nebo název hostitele (Pokud je nakonfigurovaný DNS server pro virtuální sítě) do webové služby **adresu URL webové služby** pole při vytváření nové rozhraní API nebo úpravou existující.

![Přidání rozhraní API z sítě VPN][api-management-setup-vpn-add-api]

## <a name="network-configuration-issues"></a>Běžné problémy s konfigurací sítě
Následuje seznam běžných problémů chybné konfigurace, které se mohou vyskytnout při nasazení služby API Management do virtuální sítě.

* **Vlastní instalace serveru DNS**: rozhraní API správy služby závisí na několik služeb Azure. Při API Management je umístěn ve virtuální síti s vlastního serveru DNS, je nutné přeložit názvy hostitelů těchto služeb Azure. Postupujte podle [to](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) pokyny na vlastní instalační program DNS. Zobrazit následující porty tabulce a další požadavky sítě pro referenci.

> [!IMPORTANT]
> Doporučuje se, že pokud používáte vlastní servery DNS pro virtuální sítě, můžete nastavit, **před** nasazení služby API Management do ní. V opačném případě je potřeba aktualizovat služba API Management pokaždé, když změníte servery DNS (s) spuštěním [použít operace konfigurace sítě](https://docs.microsoft.com/en-us/rest/api/apimanagement/apimanagementservice#ApiManagementService_ApplyNetworkConfigurationUpdates)

* **Porty vyžadované pro API Management**: příchozí a odchozí přenosy do podsítě, ve kterém je nasazený API Management se dá řídit pomocí [skupinu zabezpečení sítě][Network Security Group]. Pokud některá z těchto portů není k dispozici, API Management nemusí pracovat správně a může být nedostupný. Nejméně jeden z těchto portů blokované je jiné běžné chybné konfigurace problém při použití služby API Management s virtuální sítě.

Pokud je instance služby API Management je hostováno ve virtuální síti, se používají porty v následující tabulce.

| Zdrojové nebo cílové porty | Směr | Přenosový protokol | Účel | Zdroj / cíl | Typ přístupu |
| --- | --- | --- | --- | --- | --- |
| * / 80, 443 |Příchozí |TCP |Komunikaci klientů API Management |INTERNET NEBO VIRTUAL_NETWORK |Externí |
| * / 3443 |Příchozí |TCP |Koncový bod správy pro portál Azure a prostředí Powershell |INTERNET NEBO VIRTUAL_NETWORK |Externí & interní |
| * / 80, 443 |Odchozí |TCP |Závislost na úložiště Azure a Azure Service Bus |VIRTUAL_NETWORK NEBO INTERNET |Externí & interní |
| * / 1433 |Odchozí |TCP |Závislost na Azure SQL |VIRTUAL_NETWORK NEBO INTERNET |Externí & interní |
| * / 11000 - 11999 |Odchozí |TCP |Závislost na Azure SQL verze 12 |VIRTUAL_NETWORK NEBO INTERNET |Externí & interní |
| * / 14000 - 14999 |Odchozí |TCP |Závislost na Azure SQL verze 12 |VIRTUAL_NETWORK NEBO INTERNET |Externí & interní |
| * / 5671 |Odchozí |AMQP |Závislosti pro protokol do centra událostí zásadu a agent monitorování |VIRTUAL_NETWORK NEBO INTERNET |Externí & interní |
| 6381 - 6383 / 6381 - 6383 |Příchozí a odchozí |UDP |Závislost na mezipaměti Redis |VIRTUAL_NETWORK / VIRTUAL_NETWORK |Externí & interní |-
| * / 445 |Odchozí |TCP |Závislost na sdílenou složku Azure pro GIT |VIRTUAL_NETWORK NEBO INTERNET |Externí & interní |
| * / * | Příchozí |TCP |Nástroj pro vyrovnávání zatížení infrastruktury Azure | AZURE_LOAD_BALANCER / VIRTUAL_NETWORK |Externí & interní |

* **Funkce SSL**: Chcete-li povolit vytváření řetězu certifikátů SSL a ověření API Management service potřebuje odchozí síťové připojení k ocsp.msocsp.com, mscrl.microsoft.com a crl.microsoft.com. Tuto závislost není vyžadována, pokud některý z certifikátů, který nahrajete do služby API Management obsahovat úplný řetěz do kořenové certifikační Autority.

* **DNS přístup**: odchozí přístup na port 53 se vyžaduje pro komunikaci se servery DNS. Pokud existuje vlastního serveru DNS na druhém konci brána sítě VPN, musí být dostupný z podsítě hostování API Management DNS server.

* **Monitorování stavu a metrik**: odchozí síťové připojení k Azure Monitoring koncových bodů, které řešení pod následující domény: global.metrics.nsatc.net, shoebox2.metrics.nsatc.net, prod3.metrics.nsatc.net.

* **Expresní instalace trasy**: obvyklé konfigurace zákazníka je definovat vlastní výchozí trasa (0.0.0.0/0), který vynutí odchozí přenosy z Internetu do místo toku místní. Tento tok provozu vždy dělí připojení ke službě Azure API Management, protože odchozí provoz je blokované místně nebo NAT by nerozpoznatelný sadu adresy, které přestane fungovat v různých koncové body Azure. Řešení je definovat jeden (nebo více) trasy definované uživatelem ([udr][UDRs]) v podsíti, který obsahuje Azure API Management. UDR definuje tras konkrétní podsítě, které bude dodržení místo výchozí trasu.
  Pokud je to možné doporučujeme použít následující konfigurace:
 * Konfigurace ExpressRoute inzeruje 0.0.0.0/0 a ve výchozím nastavení vynucené tunelových propojení všechny odchozí přenosy na místě.
 * UDR použije na podsíť obsahující Azure API Management definuje 0.0.0.0/0 s typ dalšího segmentu z Internetu.
 Celkové požadavky tyto kroky je, že na úrovni podsítě UDR má přednost před ExpressRoute vynucené tunelování, čímž zajišťuje odchozí přístup k Internetu z Azure API Management.

>[!WARNING]  
>Azure API Management není podporovaný s konfigurací ExpressRoute, **nesprávně Inzerovat trasy z cesty veřejného partnerského vztahu k cestou soukromého partnerského vztahu mezi**. Konfigurace ExpressRoute, které mají veřejné partnerské vztahy nakonfigurované, obdrží inzerování trasy od společnosti Microsoft pro velké sady rozsahů adres Microsoft Azure IP. Pokud tyto rozsahy adres nesprávně ohlášené mezi na cestou soukromého partnerského vztahu, konečným výsledkem je, že všechny odchozí síťových paketů z podsítě instance Azure API Management jsou nesprávně force tunelovým propojením zákazníka místní síťové infrastruktuře. Tento tok sítě dělí Azure API Management. Řešení tohoto problému je zastavit směrování mezi – reklamu z cesty veřejného partnerského vztahu cestou soukromého partnerského vztahu.


## <a name="troubleshooting"></a>Řešení potíží
Při provádění změn k síti, najdete [NetworkStatus API](https://docs.microsoft.com/en-us/rest/api/apimanagement/networkstatus), ověřte, jestli služba API Management ještě ztráty přístupu k některému z důležitých prostředků, které je závislá na. Stav připojení musí být aktualizováno každých 15 minut.

## <a name="limitations"></a>Omezení
* Podsíť obsahující instance API Management nemůže obsahovat u jiných typů prostředků Azure.
* Podsíť a služba API Management musí být ve stejném předplatném.
* Nelze přesunout podsíť obsahující instance API Management napříč odběry.
* Při použití interní virtuální sítě, pouze interní IP adresu budou k dispozici z rozsahu uvádí [RFC 1918](https://tools.ietf.org/html/rfc1918), nejde zadat veřejnou IP adresu.
* Pro více oblast API Management nasazení s interní virtuální sítě nakonfigurovaný, uživatelé jsou zodpovědní za správu vlastní rozložení zátěže jako vlastní DNS.


## <a name="related-content"></a>Související obsah
* [Propojení virtuální sítě s back-end pomocí brány sítě Vpn](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti)
* [Propojení virtuální sítě z různé modely nasazení](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [Jak používat nástroj Inspector rozhraní API pro trasování volání v Azure API Management](api-management-howto-api-inspector.md)

[api-management-using-vnet-menu]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-type.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-private]: ./media/api-management-using-with-vnet/api-management-vnet-private.png
[api-management-vnet-public]: ./media/api-management-using-with-vnet/api-management-vnet-public.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[UDRs]: ../virtual-network/virtual-networks-udr-overview.md
[Network Security Group]: ../virtual-network/virtual-networks-nsg.md
