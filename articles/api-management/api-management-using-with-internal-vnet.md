---
title: "aaaHow toouse Azure API Management s interní virtuální sítě | Microsoft Docs"
description: "Zjistěte, jak toosetup a nakonfigurovat Azure API Management v interní virtuální síť."
services: api-management
documentationcenter: 
author: solankisamir
manager: kjoshi
editor: 
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 8d25de14e0c0bebe7ba7b47ca432ea4e45dde312
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-api-management-service-with-internal-virtual-network"></a>Interní virtuální sítě pomocí služby Azure API Management
Pomocí virtuální sítě Azure (virtuální sítě) můžete spravovat rozhraní API správy rozhraní API není dostupný na Internetu hello. Počet technologie VPN jsou k dispozici toomake hello připojení a API Management se dá nasadit v dva hlavní režimy uvnitř hello virtuální sítě:
* Externí
* Interní

## <a name="overview"></a>Přehled
Po nasazení v režimu interní virtuální sítě se API Management jsou viditelné ve virtuální síti, která můžete řídit přístup ke pouze všechny hello koncové body služby (brána, portál pro vývojáře, portál vydavatele, přímou správu a Git). Žádné koncové body služby hello jsou registrované na hello veřejný Server DNS.

Použití služby API Management v interní režimu, můžete dosáhnout hello následující scénáře
* Ujistěte se, bezpečně rozhraní API hostovaných ve vašem privátním datacentru přístupné 3. stranami mimo pomocí připojení Site-to-Site a ExpressRoute VPN.
* Povolte hybridní cloudové scénáře vystavení rozhraní API pro cloudové a místní rozhraní API prostřednictvím brány běžné.
* Spravujte vaše rozhraní API hostovaných ve více zeměpisných oblastí pomocí jeden koncový bod služby Gateway. 

## <a name="enable-vpn"></a>Vytváření API Management v interní virtuální sítě
Hello služba API Management v interní virtuální sítě je hostován za k interní Balancer(ILB) zatížení. Hello IP adresu hello ILB je v hello [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) rozsahu.  

### <a name="enable-vnet-connection-using-azure-portal"></a>Povolit připojení virtuální sítě pomocí portálu Azure
Služba API Management hello nejdřív vytvořit pomocí následujících kroků hello [služba API Management vytvořit][Create API Management service]. Pak nastavení API Management toobe nasazené uvnitř virtuální sítě.

![Nabídky pro nastavení APIM v interní virtuální síť][api-management-using-internal-vnet-menu]

Po úspěšné nasazení hello, měli byste vidět hello interní virtuální IP adresy služby na řídicím panelu hello.

![Řídicí panel správy rozhraní API s interní virtuální síť nakonfigurované][api-management-internal-vnet-dashboard]

### <a name="enable-vnet-connection-using-powershell-cmdlets"></a>Povolit připojení virtuální sítě pomocí rutin prostředí Powershell
Můžete také povolit připojení virtuální sítě pomocí rutin prostředí PowerShell hello.

* **Vytvoření služby API Management uvnitř virtuální sítě**: použijte rutinu hello [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) toocreate službě Azure API Management služby uvnitř virtuální sítě a nakonfigurujte ji toouse hello interní virtuální síť typu.

* **Nasazení služby API Management existující uvnitř virtuální sítě**: použijte rutinu hello [aktualizace AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) toomove existující Azure API Management služby uvnitř virtuální sítě a nakonfigurujte ji toouse Interní typ virtuální sítě.

## <a name="apim-dns-configuration"></a>Konfigurace služby DNS
Pokud používáte API Management v režimu externí virtuální sítě, DNS spravuje Azure. Interní virtuální sítě režimu je nutné toomanage vlastní DNS.

> [!NOTE]
> Služba API Management nenaslouchá toorequests přicházející na IP adresy. Odpovídá pouze toorequests toohello název hostitele nakonfigurovaného na jeho koncové body služby, (která zahrnuje brány, portál pro vývojáře, portál vydavatele, koncový bod přímou správu a git).

### <a name="access-on-default-host-names"></a>Přístup na výchozí názvy hostitelů:
Při vytváření služby API Management ve veřejném cloudu Azure, například s názvem "contoso", hello následující koncové body služby jsou nakonfigurované ve výchozím nastavení.

>   Brána nebo proxy server - contoso.azure api.net

> Portál vydavatele a portál pro vývojáře - contoso.portal.azure api.net

> Přímá správa koncový bod - contoso.management.azure api.net

>   GIT - contoso.scm.azure api.net

tooaccess těchto koncových bodů služby API Management, můžete vytvořit virtuální počítač ve virtuální síti toohello připojené podsítě ve kterém je nasazený API Management. Za předpokladu, že je 10.0.0.5 hello interní virtuální IP adresy pro vaši službu, můžete provést hello mapování souborů hostitele (% SystemDrive%\drivers\etc\hosts) následujícím způsobem:

> 10.0.0.5 contoso.azure-api.net

> 10.0.0.5 contoso.portal.azure-api.net

> 10.0.0.5 contoso.management.azure-api.net

> 10.0.0.5 contoso.scm.azure-api.net

Všechny koncové body služby hello můžete poté přistoupit z hello virtuálního počítače, které jste vytvořili. Pokud používáte server DNS vlastní ve virtuální síti, můžete také vytvořit záznamy A DNS a využít těchto koncových bodů z libovolného místa ve vaší virtuální síti. 

### <a name="access-on-custom-domain-names"></a>Přístup na vlastní názvy domén:
Pokud nechcete, aby tooaccess hello služba API Management s názvy hostitelů hello výchozí, můžete nastavit vlastní názvy domén pro všechny vaše koncové body služby jako níže

![Nastavení vlastní domény pro rozhraní API Management][api-management-custom-domain-name]

Potom můžete vytvořit záznamy A v vašeho serveru DNS tooaccess těchto koncových bodů, které jsou přístupné z ve vaší virtuální síti.

## <a name="related-content"></a>Související obsah
* [Běžné problémy s konfigurací sítě při nastavování APIM ve virtuální síti][Common Network Configuration Issues]
* [Nejčastější dotazy virtuální sítě](../virtual-network/virtual-networks-faq.md)
* [Vytváření A záznam v DNS](https://msdn.microsoft.com/en-us/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: api-management-get-started.md#create-service-instance
[Common Network Configuration Issues]: api-management-using-with-vnet.md#network-configuration-issues
