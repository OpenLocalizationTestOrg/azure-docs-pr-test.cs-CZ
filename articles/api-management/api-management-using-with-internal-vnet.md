---
title: "Jak používat Azure API Management s interní virtuální sítě | Microsoft Docs"
description: "Zjistěte, jak nainstalovat a nakonfigurovat Azure API Management v interní virtuální síť."
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
ms.openlocfilehash: 55248387c7e78d05c1cf1afd615b7b921e9669d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-api-management-service-with-internal-virtual-network"></a>Interní virtuální sítě pomocí služby Azure API Management
Pomocí virtuální sítě Azure (virtuální sítě) můžete spravovat rozhraní API správy rozhraní API není dostupný na Internetu. Jsou k dispozici pro připojení VPN technologií pro a API Management se dá nasadit v dva hlavní režimy uvnitř virtuální sítě:
* Externí
* Interní

## <a name="overview"> </a>Přehled
Po nasazení v režimu interní virtuální sítě se API Management všechny koncové body služby (brána, portál pro vývojáře, portál vydavatele, přímou správu a Git) viditelné pouze uvnitř virtuální sítě, který ovládáte přístup. Žádné koncové body služby jsou registrované na veřejném serveru DNS.

Použití služby API Management v interní režimu, můžete dosáhnout následujících scénářů
* Ujistěte se, bezpečně rozhraní API hostovaných ve vašem privátním datacentru přístupné 3. stranami mimo pomocí připojení Site-to-Site a ExpressRoute VPN.
* Povolte hybridní cloudové scénáře vystavení rozhraní API pro cloudové a místní rozhraní API prostřednictvím brány běžné.
* Spravujte vaše rozhraní API hostovaných ve více zeměpisných oblastí pomocí jeden koncový bod služby Gateway. 

## <a name="enable-vpn"></a>Vytváření API Management v interní virtuální sítě
Služba API Management v interní virtuální sítě je hostován za k interní Balancer(ILB) zatížení. IP adresa ILB je v [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) rozsahu.  

### <a name="enable-vnet-connection-using-azure-portal"></a>Povolit připojení virtuální sítě pomocí portálu Azure
Pomocí následujícího postupu nejdřív vytvořit službu API Management [služba API Management vytvořit][Create API Management service]. Pak nastavení API Management k nasazení uvnitř virtuální sítě.

![Nabídky pro nastavení APIM v interní virtuální síť][api-management-using-internal-vnet-menu]

Po úspěšné nasazení, měli byste vidět interní virtuální IP adresu služby na řídicím panelu.

![Řídicí panel správy rozhraní API s interní virtuální síť nakonfigurované][api-management-internal-vnet-dashboard]

### <a name="enable-vnet-connection-using-powershell-cmdlets"></a>Povolit připojení virtuální sítě pomocí rutin prostředí Powershell
Můžete také povolit připojení virtuální sítě pomocí rutin prostředí PowerShell.

* **Vytvoření služby API Management uvnitř virtuální sítě**: použijte rutinu [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) k vytvoření služby Azure API Management uvnitř virtuální sítě a nakonfigurujte ji chcete použít typ interní virtuální sítě.

* **Nasazení služby API Management existující uvnitř virtuální sítě**: použijte rutinu [aktualizace AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) přesunout existující službu Azure API Management uvnitř virtuální sítě a nakonfigurovat ji chcete použít typ interní virtuální sítě.

## <a name="apim-dns-configuration"></a>Konfigurace služby DNS
Pokud používáte API Management v režimu externí virtuální sítě, DNS spravuje Azure. Pro režim interní virtuální sítě budete muset spravovat vlastní DNS.

> [!NOTE]
> Služba API Management nepřijímá požadavky na požadavky přicházející na IP adresy. Pouze reaguje na požadavky na název hostitele nakonfigurovaného na jeho koncové body služby (která zahrnuje brány, portál pro vývojáře, portál vydavatele, koncový bod přímou správu a git).

### <a name="access-on-default-host-names"></a>Přístup na výchozí názvy hostitelů:
Při vytváření služby API Management ve veřejném cloudu Azure, například s názvem "contoso", jsou ve výchozím nastavení nakonfigurované následující služby na koncové body.

>   Brána nebo proxy server - contoso.azure api.net

> Portál vydavatele a portál pro vývojáře - contoso.portal.azure api.net

> Přímá správa koncový bod - contoso.management.azure api.net

>   GIT - contoso.scm.azure api.net

Pro přístup k tyto koncové body služby API Management, můžete vytvořit virtuální počítač v podsíti, připojený k virtuální síti, ve kterém je nasazený API Management. Za předpokladu, že interní virtuální IP adresy pro služby je 10.0.0.5, můžete provést hostitelů souboru mapování (% SystemDrive%\drivers\etc\hosts) následujícím způsobem:

> 10.0.0.5 contoso.azure-api.net

> 10.0.0.5 contoso.portal.azure-api.net

> 10.0.0.5 contoso.management.azure-api.net

> 10.0.0.5 contoso.scm.azure-api.net

Všechny koncové body služby můžete poté přistoupit z virtuálního počítače, které jste vytvořili. Pokud používáte server DNS vlastní ve virtuální síti, můžete také vytvořit záznamy A DNS a využít těchto koncových bodů z libovolného místa ve vaší virtuální síti. 

### <a name="access-on-custom-domain-names"></a>Přístup na vlastní názvy domén:
Pokud nechcete, aby přístup ke službě API Management s výchozí názvy hostitelů, můžete nastavit vlastní názvy domén pro všechny vaše koncové body služby jako níže

![Nastavení vlastní domény pro rozhraní API Management][api-management-custom-domain-name]

Potom můžete vytvořit záznamy v serveru DNS pro přístup k těchto koncových bodů, které jsou přístupné z ve vaší virtuální síti.

## <a name="related-content"></a>Související obsah
* [Běžné problémy s konfigurací sítě při nastavování APIM ve virtuální síti][Common Network Configuration Issues]
* [Nejčastější dotazy virtuální sítě](../virtual-network/virtual-networks-faq.md)
* [Vytváření A záznam v DNS](https://msdn.microsoft.com/en-us/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: api-management-get-started.md#create-service-instance
[Common Network Configuration Issues]: api-management-using-with-vnet.md#network-configuration-issues
