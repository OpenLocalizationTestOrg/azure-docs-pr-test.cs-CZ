---
title: "aaaHow toouse Azure API Management s virtuálními sítěmi"
description: "Zjistěte, jak toosetup tooa připojení virtuální sítě v Azure API Management a přístup web services přes něj."
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
ms.openlocfilehash: 426b3974e4fed7daffdb0c3f02381edbc326dc28
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-api-management-with-virtual-networks"></a>Jak toouse Azure API Management s virtuálními sítěmi
Virtuální sítě Azure (virtuální sítě) umožňují tooplace některé z vašich prostředků Azure v Internetu jiných routeable síti, která můžete řídit přístup k. Tyto sítě pak může být připojené tooyour místními sítěmi pomocí různých technologií sítě VPN. informace o virtuálních sítí Azure začínat hello informace v tomto poli toolearn: [Přehled virtuálních sítí Azure](../virtual-network/virtual-networks-overview.md).

Azure API Management se dá nasadit ve virtuální síti hello (VNET), má přístup k back-end služby v rámci sítě hello. Dobrý den, portál pro vývojáře a Brána rozhraní API, může být nakonfigurované toobe přístupný z Internetu hello nebo pouze v rámci virtuální sítě hello.

> [!NOTE]
> Azure API Management podporuje classic a Azure Resource Manager virtuálních sítí patřících.
>
>

## <a name="enable-vpn"></a>Připojení povolte virtuální sítě
> [!NOTE]
> Připojení k virtuální síti je k dispozici v hello **Premium** a **vývojáře** vrstev. tooswitch mezi vrstvami hello otevřete služby API Management v hello portál Azure a pak otevřete hello **škálování a ceny** kartě. V části hello **cenová úroveň** , vyberte úroveň Premium nebo vývojáře hello a klikněte na tlačítko Uložit.
>

připojení k virtuální síti tooenable, otevřete služby API Management v hello portál Azure a otevřete hello **virtuální síť** stránky.

![Virtuální síť nabídky služby API Management][api-management-using-vnet-menu]

Vyberte typ přístupu hello potřeby:

* **Externí**: portál brány a vývojáře hello API Management jsou dostupné z hello veřejný internet prostřednictvím externím vyrovnáváním zatížení. Hello brány mají přístup k prostředkům v rámci virtuální sítě hello.

![Veřejný partnerský vztah][api-management-vnet-public]

* **Interní**: portál brány a vývojáře hello API Management jsou k dispozici pouze v rámci virtuální sítě hello prostřednictvím interní nástroj. Hello brány mají přístup k prostředkům v rámci virtuální sítě hello.

![Soukromý partnerský vztah][api-management-vnet-private]

Zobrazí seznam všech oblastech, kde je zřízený služby API Management. Vyberte virtuální síť a podsíť pro každou oblast. Hello seznam se naplní se classic i Resource Manager virtuální sítě k dispozici v rámci vašich předplatných Azure, které jsou nastavení v oblasti hello, který konfigurujete.

> [!NOTE]
> **Koncový bod služby** v hello výše diagram zahrnuje brány nebo Proxy, portál vydavatele, portál pro vývojáře, GIT a hello přímé koncový bod správy.
> **Koncový bod správy** v hello výše diagramu je koncový bod hello hostované v konfiguraci toomanage hello služby prostřednictvím portálu Azure a prostředí Powershell.
> Mějte také na paměti, že přestože hello diagram znázorňuje IP adresy pro své různé koncové body služby API Management **pouze** odpoví na své nakonfigurované názvy hostitelů.

> [!IMPORTANT]
> Při nasazení Azure API Management instance tooa virtuální sítě Resource Manageru, hello služba musí být v vyhrazené podsíť, která obsahuje žádné další prostředky s výjimkou instance Azure API Management. Pokud je proveden pokus o toodeploy tooa instance Azure API Management podsíť virtuální sítě Resource Manageru, který obsahuje jiné prostředky, hello nasazení se nezdaří.
>
>

![Vyberte síť VPN][api-management-setup-vpn-select]

Klikněte na tlačítko **Uložit** hello horní části úvodní obrazovka.

> [!NOTE]
> Hello adresy VIP instance služby API Management hello se změní pokaždé, když virtuální sítě je povolený nebo zakázaný.  
> Hello adresa VIP změní také při API Management se přesune ze **externí** příliš**interní** nebo naopak
>


> [!IMPORTANT]
> Když odeberete API Management z virtuální sítě nebo změnit hello jeden, který je nasazen v, hello použitých virtuální sítě může zůstat uzamčen, pro too4 hodin. Během této doby nebude být možné toodelete hello virtuální síť nebo nasazení nové tooit prostředků.

## <a name="enable-vnet-powershell"></a>Připojení VNET povolit pomocí rutin prostředí PowerShell
Můžete také povolit připojení virtuální sítě pomocí rutin prostředí PowerShell hello

* **Vytvoření služby API Management uvnitř virtuální sítě**: použijte rutinu hello [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) toocreate služby Azure API Management uvnitř virtuální sítě.

* **Nasazení služby API Management existující uvnitř virtuální sítě**: použijte rutinu hello [aktualizace AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) toomove existující službu Azure API Management uvnitř virtuální sítě.

## <a name="connect-vnet"></a>Připojit tooa webovou službu hostovanou v rámci virtuální sítě
Po služby API Management je toohello připojené virtuální sítě, je přístup ke službám back-end v něm nejsou jiné než přístup ke službám veřejné. Jednoduše zadejte hello místní IP adresu nebo hello název hostitele (Pokud je DNS server nakonfigurovaný pro hello virtuální sítě) webové služby do hello **adresu URL webové služby** pole při vytváření nové rozhraní API nebo úpravou existující.

![Přidání rozhraní API z sítě VPN][api-management-setup-vpn-add-api]

## <a name="network-configuration-issues"></a>Běžné problémy s konfigurací sítě
Následuje seznam běžných problémů chybné konfigurace, které se mohou vyskytnout při nasazení služby API Management do virtuální sítě.

* **Vlastní instalace serveru DNS**: hello služba API Management závisí na několik služeb Azure. Je-li API Management je umístěn ve virtuální síti s vlastního serveru DNS, musí být názvy hostitelů hello tooresolve těchto služeb Azure. Postupujte podle [to](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) pokyny na vlastní instalační program DNS. Zobrazit hello porty tabulce a další požadavky sítě pro referenci.

> [!IMPORTANT]
> Doporučuje se, že pokud používáte vlastní servery DNS pro hello virtuální síť, můžete nastavit, **před** nasazení služby API Management do ní. V opačném případě je nutné příliš aktualizovat služba API Management hello při každé změně servery DNS hello (s) spuštěním hello [použít operace konfigurace sítě](https://docs.microsoft.com/en-us/rest/api/apimanagement/apimanagementservice#ApiManagementService_ApplyNetworkConfigurationUpdates)

* **Porty vyžadované pro API Management**: příchozí a odchozí přenosy do hello podsíť, ve kterém je nasazený API Management se dá řídit pomocí [skupinu zabezpečení sítě][Network Security Group]. Pokud některá z těchto portů není k dispozici, API Management nemusí pracovat správně a může být nedostupný. Nejméně jeden z těchto portů blokované je jiné běžné chybné konfigurace problém při použití služby API Management s virtuální sítě.

Pokud instanci služby API Management je umístěn ve virtuální síti, se použijí hello porty hello následující tabulka.

| Zdrojové nebo cílové porty | Směr | Přenosový protokol | Účel | Zdroj / cíl | Typ přístupu |
| --- | --- | --- | --- | --- | --- |
| * / 80, 443 |Příchozí |TCP |TooAPI komunikace klienta správy |INTERNET NEBO VIRTUAL_NETWORK |Externí |
| * / 3443 |Příchozí |TCP |Koncový bod správy pro portál Azure a prostředí Powershell |INTERNET NEBO VIRTUAL_NETWORK |Externí & interní |
| * / 80, 443 |Odchozí |TCP |Závislost na úložiště Azure a Azure Service Bus |VIRTUAL_NETWORK NEBO INTERNET |Externí & interní |
| * / 1433 |Odchozí |TCP |Závislost na Azure SQL |VIRTUAL_NETWORK NEBO INTERNET |Externí & interní |
| * / 11000 - 11999 |Odchozí |TCP |Závislost na Azure SQL verze 12 |VIRTUAL_NETWORK NEBO INTERNET |Externí & interní |
| * / 14000 - 14999 |Odchozí |TCP |Závislost na Azure SQL verze 12 |VIRTUAL_NETWORK NEBO INTERNET |Externí & interní |
| * / 5671 |Odchozí |AMQP |Závislost pro zásady rozbočovače tooEvent protokolu a agent monitorování |VIRTUAL_NETWORK NEBO INTERNET |Externí & interní |
| 6381 - 6383 / 6381 - 6383 |Příchozí a odchozí |UDP |Závislost na mezipaměti Redis |VIRTUAL_NETWORK / VIRTUAL_NETWORK |Externí & interní |-
| * / 445 |Odchozí |TCP |Závislost na sdílenou složku Azure pro GIT |VIRTUAL_NETWORK NEBO INTERNET |Externí & interní |
| * / * | Příchozí |TCP |Nástroj pro vyrovnávání zatížení infrastruktury Azure | AZURE_LOAD_BALANCER / VIRTUAL_NETWORK |Externí & interní |

* **Funkce SSL**: tooenable certifikát SSL musí služba API Management hello řetězec pro vytváření a ověřování tooocsp.msocsp.com odchozí síťové připojení, mscrl.microsoft.com a crl.microsoft.com. Tuto závislost není vyžadována, pokud některý z certifikátů, že nahrajete tooAPI správu obsahovat kořenové toohello certifikační Autority úplný řetěz hello.

* **DNS přístup**: odchozí přístup na port 53 se vyžaduje pro komunikaci se servery DNS. Pokud existuje vlastního serveru DNS na hello druhém konci brána sítě VPN, musí být dostupný z podsítě hello hostování API Management hello DNS server.

* **Monitorování stavu a metrik**: odchozí připojení tooAzure sledování koncových bodů sítě, které řešení v části hello následující domény: global.metrics.nsatc.net, shoebox2.metrics.nsatc.net, prod3.metrics.nsatc.net.

* **Expresní instalace trasy**: obvyklé konfigurace zákazníka je toodefine vlastní výchozí trasa (0.0.0.0/0), který vynutí odchozí internetové přenosy tooinstead toku místně. Tento tok provozu vždy dělí připojení ke službě Azure API Management, protože odchozí přenosy hello je blokované místně nebo NAT'd tooan nerozpoznatelný sadu adresy, které přestane fungovat v různých koncové body Azure. Hello řešení je trasy definované uživatelem toodefine jeden (nebo více) ([udr][UDRs]) v podsíti hello, který obsahuje hello Azure API Management. UDR definuje tras konkrétní podsítě, které bude dodržení místo hello výchozí trasu.
  Pokud je to možné doporučujeme toouse hello následující konfigurace:
 * Konfigurace ExpressRoute Hello inzeruje 0.0.0.0/0 a ve výchozím nastavení vynucené tunelových propojení všechny odchozí přenosy na místě.
 * Hello UDR použít toohello podsíť obsahující hello Azure API Management definuje 0.0.0.0/0 s typ dalšího segmentu z Internetu.
 Hello celkové požadavky tyto kroky je úrovni podsítě hello UDR má přednost před hello ExpressRoute vynuceného tunelování, čímž zajišťuje odchozí přístup k Internetu z hello Azure API Management.

>[!WARNING]  
>Azure API Management není podporovaný s konfigurací ExpressRoute, **nesprávně Inzerovat trasy z hello cesty veřejného partnerského vztahu cesta toohello soukromého partnerského vztahu mezi**. Konfigurace ExpressRoute, které mají veřejné partnerské vztahy nakonfigurované, obdrží inzerování trasy od společnosti Microsoft pro velké sady rozsahů adres Microsoft Azure IP. Pokud tyto rozsahy adres nesprávně ohlášené mezi na hello cestou soukromého partnerského vztahu, hello konečný výsledek je, že všechny odchozí síťových paketů z instance Azure API Management hello podsítě jsou nesprávně tunelovým propojením force tooa zákazníka do místní sítě infrastruktura. Tento tok sítě dělí Azure API Management. je ale problém toothis řešení Hello směrování mezi – reklamu toostop z hello cesty veřejného partnerského vztahu cesta toohello soukromého partnerského vztahu.


## <a name="troubleshooting"></a>Řešení potíží
Při provádění změn tooyour sítě, naleznete v příliš[NetworkStatus API](https://docs.microsoft.com/en-us/rest/api/apimanagement/networkstatus), toovalidate Pokud hello služba API Management ještě ztratili přístup tooany Dobrý den důležité prostředky, které je závislá na. stavu připojení Hello by měl být aktualizováno každých 15 minut.

## <a name="limitations"></a>Omezení
* Podsíť obsahující instance API Management nemůže obsahovat u jiných typů prostředků Azure.
* Hello podsíť a hello API Management service musí být v hello stejného předplatného.
* Nelze přesunout podsíť obsahující instance API Management napříč odběry.
* Při použití interní virtuální sítě, pouze pro interní IP adresu, bude k dispozici z rozsahu hello uvádí [RFC 1918](https://tools.ietf.org/html/rfc1918), nejde zadat veřejnou IP adresu.
* Pro více oblast API Management nasazení s interní virtuální sítě nakonfigurovaný, uživatelé jsou zodpovědní za správu vlastní rozložení zátěže jako vlastní hello DNS.


## <a name="related-content"></a>Související obsah
* [Připojení virtuální síti toobackend použitím brány Vpn](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti)
* [Propojení virtuální sítě z různé modely nasazení](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [Jakým způsobem volá toouse hello tootrace Inspector rozhraní API v Azure API Management](api-management-howto-api-inspector.md)

[api-management-using-vnet-menu]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-type.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-private]: ./media/api-management-using-with-vnet/api-management-vnet-private.png
[api-management-vnet-public]: ./media/api-management-using-with-vnet/api-management-vnet-public.png

[Enable VPN connections]: #enable-vpn
[Connect tooa web service behind VPN]: #connect-vpn
[Related content]: #related-content

[UDRs]: ../virtual-network/virtual-networks-udr-overview.md
[Network Security Group]: ../virtual-network/virtual-networks-nsg.md
