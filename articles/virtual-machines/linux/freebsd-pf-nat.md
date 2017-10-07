---
title: "Filtr paketů toocreate aaaUse FreeBSD bránou firewall v Azure | Microsoft Docs"
description: "Zjistěte, jak toodeploy NAT brány firewall pomocí na FreeBSD PF v Azure."
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/20/2017
ms.author: kyliel
ms.openlocfilehash: 3d3a5dde2ca03ba6fc384581c786f5eb746e6d92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-freebsds-packet-filter-toocreate-a-secure-firewall-in-azure"></a>Jak filtr paketů toocreate toouse FreeBSD zabezpečené brány firewall v Azure
Tento článek představuje jak toodeploy brány firewall NAT. pomocí filtru na FreeBSD balírna pomocí šablony Azure Resource Manageru pro běžný scénář webového serveru.

## <a name="what-is-pf"></a>Co je PF?
PF (filtr paketů, zapisují také pf) je filtr licencovanou paketů BSD, centrální softwarového produktu pro fungující brána firewall může. PF od vyvinul rychle a teď má několik výhod oproti další dostupné brány firewall. Překlad adres (NAT) je v souboru PF od jeden den, pak Plánovač paketů a správu active fronty integrována do souboru PF, integrací hello ALTQ a díky tomu lze změnit v nastavení konfigurace na PF. Funkce jako je například pfsync a CARP pro převzetí služeb při selhání a redundance, authpf relace ověřování a proxy server ftp tooease fungující brána firewall může hello obtížné protokolu FTP, také rozšířili PF. Stručně řečeno PF je výkonný a bohaté funkce Brána firewall. 

## <a name="get-started"></a>Začínáme
Pokud mají zájem o nastavení zabezpečení brány firewall v cloudu hello pro webové servery a potom můžeme začít. Můžete taky použít skripty hello použité v této tooset šablony Azure Resource Manageru si topologii vaší sítě.
šablony Azure Resource Manageru Hello nastavte FreeBSD virtuální počítač, který provede NAT /redirection PF a dva virtuální počítače FreeBSD pomocí hello Nginx webový server nainstalován a nakonfigurován. Kromě toho tooperforming NAT pro dva webové servery hello výstupních provoz, hello NAT/přesměrování virtuálního počítače zachytí požadavků HTTP a přesměruje je toohello dva webové servery v kruhového dotazování. Hello virtuální sítě používá hello privátní směrovat IP adres místo 10.0.0.2/24 a můžete změnit parametry hello hello šablony. šablony Azure Resource Manageru Hello také definuje tabulka směrování pro hello celý virtuální síť, což je kolekce jednotlivých tras použít toooverride Azure výchozích tras na základě hello cílové IP adresy. 

![pf_topology](./media/freebsd-pf-nat/pf_topology.jpg)
    
### <a name="deploy-through-azure-cli"></a>Nasazení pomocí rozhraní příkazového řádku Azure
Je třeba hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login). Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Hello následující příklad vytvoří název skupiny prostředků `myResourceGroup` v hello `West US` umístění.

```azurecli
az group create --name myResourceGroup --location westus
```

V dalším kroku nasaďte šablonu hello [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup) s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create). Stáhněte si [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/pf-freebsd-setup/azuredeploy.parameters.json) v části hello stejnou cestu a definovat vlastní hodnoty prostředků, jako například `adminPassword`, `networkPrefix`, a `domainNamePrefix`. 

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeploymentName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/pf-freebsd-setup/azuredeploy.json \
    --parameters '@azuredeploy.parameters.json' --verbose
```

Po přibližně pět minut, zobrazí se informace hello `"provisioningState": "Succeeded"`. Pak můžete ssh toohello front-endu virtuálních počítačů (NAT) nebo přístup Nginx webovému serveru v prohlížeči pomocí hello veřejnou IP adresu nebo plně kvalifikovaný název domény hello front-endu virtuálních počítačů (NAT). Hello následující příklad vypíše plně kvalifikovaný název domény a veřejnou IP adresu, která přiřazená toohello front-endu virtuálních počítačů (NAT) v hello `myResourceGroup` skupinu prostředků. 

```azurecli
az network public-ip list --resource-group myResourceGroup
```
    
## <a name="next-steps"></a>Další kroky
Chcete tooset si vlastní NAT v Azure? Otevřít zdroj, bezplatné ale výkonné? Potom PF je vhodné použít. Pomocí šablony hello [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup), stačí pouze pět minut tooset brány firewall NAT. s vyrovnávání pomocí FreeBSD na zatížení pomocí kruhového dotazování PF v Azure pro běžný scénář webového serveru. 

Pokud chcete, aby toolearn hello nabízení FreeBSD v Azure, podívejte se příliš[tooFreeBSD Úvod v Azure](freebsd-intro-on-azure.md).

Pokud chcete více informací o souboru PF tooknow, podívejte se příliš[FreeBSD k používání](https://www.freebsd.org/doc/handbook/firewalls-pf.html) nebo [PF – uživatelská příručka](https://www.freebsd.org/doc/handbook/firewalls-pf.html).
