---
title: "Filtr paketů na FreeBSD použít k vytvoření brány firewall v Azure | Microsoft Docs"
description: "Informace o nasazení pomocí FreeBSD na brány firewall NAT. PF v Azure."
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
ms.openlocfilehash: cd777291a1321eabf4efe0d7b9b101f932d9398b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-freebsds-packet-filter-to-create-a-secure-firewall-in-azure"></a><span data-ttu-id="b76d6-103">Použití filtru paketů na FreeBSD v Azure vytvořit zabezpečený brány firewall</span><span class="sxs-lookup"><span data-stu-id="b76d6-103">How to use FreeBSD's Packet Filter to create a secure firewall in Azure</span></span>
<span data-ttu-id="b76d6-104">Tento článek představuje nasazení brány firewall NAT. pomocí filtru na FreeBSD balírna pomocí šablony Azure Resource Manageru pro běžný scénář webového serveru.</span><span class="sxs-lookup"><span data-stu-id="b76d6-104">This article introduces how to deploy a NAT firewall using FreeBSD’s Packer Filter through Azure Resource Manager template for common web server scenario.</span></span>

## <a name="what-is-pf"></a><span data-ttu-id="b76d6-105">Co je PF?</span><span class="sxs-lookup"><span data-stu-id="b76d6-105">What is PF?</span></span>
<span data-ttu-id="b76d6-106">PF (filtr paketů, zapisují také pf) je filtr licencovanou paketů BSD, centrální softwarového produktu pro fungující brána firewall může.</span><span class="sxs-lookup"><span data-stu-id="b76d6-106">PF (Packet Filter, also written pf) is a BSD licensed stateful packet filter, a central piece of software for firewalling.</span></span> <span data-ttu-id="b76d6-107">PF od vyvinul rychle a teď má několik výhod oproti další dostupné brány firewall.</span><span class="sxs-lookup"><span data-stu-id="b76d6-107">PF has since evolved quickly and now has several advantages over other available firewalls.</span></span> <span data-ttu-id="b76d6-108">Překlad adres (NAT) je v souboru PF od jeden den, pak Plánovač paketů a správu active fronty integrována do PF, integrací ALTQ a díky tomu lze změnit v nastavení konfigurace na PF.</span><span class="sxs-lookup"><span data-stu-id="b76d6-108">Network Address Translation (NAT) is in PF since day one, then packet scheduler and active queue management have been integrated into PF, by integrating the ALTQ and making it configurable through PF's configuration.</span></span> <span data-ttu-id="b76d6-109">Funkce jako je například pfsync a CARP pro převzetí služeb při selhání a redundance, authpf relace ověřování a ftp proxy serveru k usnadnění fungující obtížné protokolu FTP, brána firewall může také rozšířili PF.</span><span class="sxs-lookup"><span data-stu-id="b76d6-109">Features such as pfsync and CARP for failover and redundancy, authpf for session authentication, and ftp-proxy to ease firewalling the difficult FTP protocol, have also extended PF.</span></span> <span data-ttu-id="b76d6-110">Stručně řečeno PF je výkonný a bohaté funkce Brána firewall.</span><span class="sxs-lookup"><span data-stu-id="b76d6-110">In short, PF is a powerful and feature-rich firewall.</span></span> 

## <a name="get-started"></a><span data-ttu-id="b76d6-111">Začínáme</span><span class="sxs-lookup"><span data-stu-id="b76d6-111">Get started</span></span>
<span data-ttu-id="b76d6-112">Pokud mají zájem o nastavení zabezpečení brány firewall v cloudu pro webové servery a potom můžeme začít.</span><span class="sxs-lookup"><span data-stu-id="b76d6-112">If you are interested in setting up a secure firewall in the cloud for your web servers, then let’s get started.</span></span> <span data-ttu-id="b76d6-113">Můžete taky použít skripty použité v této šablony Azure Resource Manager nastavit topologii vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="b76d6-113">You can also apply the scripts used in this Azure Resource Manager template to set up your networking topology.</span></span>
<span data-ttu-id="b76d6-114">Šablony Azure Resource Manageru nastavte FreeBSD virtuální počítač, který provádí NAT /redirection PF a dva virtuální počítače FreeBSD pomocí Nginx webový server nainstalován a nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="b76d6-114">The Azure Resource Manager template set up a FreeBSD virtual machine that performs NAT /redirection using PF and two FreeBSD virtual machines with the Nginx web server installed and configured.</span></span> <span data-ttu-id="b76d6-115">Kromě provádění NAT pro dva webové servery odchozí přenosy, virtuální počítač NAT/přesměrování zachycuje požadavků HTTP a přesměruje je to dva webové servery v kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="b76d6-115">In addition to performing NAT for the two web servers egress traffic, the NAT/redirection virtual machine intercepts HTTP requests and redirect them to the two web servers in round-robin fashion.</span></span> <span data-ttu-id="b76d6-116">Virtuální sítě používá 10.0.0.2/24 privátní místo směrovat adresu IP a můžete změnit parametry šablony.</span><span class="sxs-lookup"><span data-stu-id="b76d6-116">The VNet uses the private non-routable IP address space 10.0.0.2/24 and you can modify the parameters of the template.</span></span> <span data-ttu-id="b76d6-117">Šablony Azure Resource Manageru také definuje směrovací tabulku pro celý virtuální síť, což je kolekce jednotlivých tras používaná k přepsání Azure výchozích tras na základě cílové IP adresy.</span><span class="sxs-lookup"><span data-stu-id="b76d6-117">The Azure Resource Manager template also defines a route table for the whole VNet, which is a collection of individual routes used to override Azure default routes based on the destination IP address.</span></span> 

![pf_topology](./media/freebsd-pf-nat/pf_topology.jpg)
    
### <a name="deploy-through-azure-cli"></a><span data-ttu-id="b76d6-119">Nasazení pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="b76d6-119">Deploy through Azure CLI</span></span>
<span data-ttu-id="b76d6-120">Budete potřebovat nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="b76d6-120">You need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="b76d6-121">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="b76d6-121">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="b76d6-122">Následující příklad vytvoří název skupiny prostředků `myResourceGroup` v `West US` umístění.</span><span class="sxs-lookup"><span data-stu-id="b76d6-122">The following example creates a resource group name `myResourceGroup` in the `West US` location.</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="b76d6-123">V dalším kroku nasaďte šablonu [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup) s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="b76d6-123">Next, deploy the template [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup) with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="b76d6-124">Stáhněte si [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/pf-freebsd-setup/azuredeploy.parameters.json) pod stejnou cestu a definovat vlastní hodnoty prostředků, jako například `adminPassword`, `networkPrefix`, a `domainNamePrefix`.</span><span class="sxs-lookup"><span data-stu-id="b76d6-124">Download [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/pf-freebsd-setup/azuredeploy.parameters.json) under the same path and define your own resource values, such as `adminPassword`, `networkPrefix`, and `domainNamePrefix`.</span></span> 

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeploymentName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/pf-freebsd-setup/azuredeploy.json \
    --parameters '@azuredeploy.parameters.json' --verbose
```

<span data-ttu-id="b76d6-125">Po přibližně pět minut, zobrazí se informace o `"provisioningState": "Succeeded"`.</span><span class="sxs-lookup"><span data-stu-id="b76d6-125">After about five minutes, you will get the information of `"provisioningState": "Succeeded"`.</span></span> <span data-ttu-id="b76d6-126">Potom můžete ssh na front-endu virtuálních počítačů (NAT) nebo přístup Nginx webového serveru v prohlížeči pomocí veřejnou IP adresu nebo plně kvalifikovaný název domény front-endu virtuálních počítačů (NAT).</span><span class="sxs-lookup"><span data-stu-id="b76d6-126">Then you can ssh to the frontend VM (NAT) or access Nginx web server in a browser using the public IP address or FQDN of the frontend VM (NAT).</span></span> <span data-ttu-id="b76d6-127">Následující příklad uvádí ve plně kvalifikovaný název domény a veřejnou IP adresu, která přiřazená k front-endu virtuálních počítačů (NAT) `myResourceGroup` skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b76d6-127">The following example lists FQDN and public IP address that assigned to the frontend VM (NAT) in the `myResourceGroup` resource group.</span></span> 

```azurecli
az network public-ip list --resource-group myResourceGroup
```
    
## <a name="next-steps"></a><span data-ttu-id="b76d6-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b76d6-128">Next steps</span></span>
<span data-ttu-id="b76d6-129">Opravdu chcete nastavit vlastní NAT v Azure?</span><span class="sxs-lookup"><span data-stu-id="b76d6-129">Do you want to set up your own NAT in Azure?</span></span> <span data-ttu-id="b76d6-130">Otevřít zdroj, bezplatné ale výkonné?</span><span class="sxs-lookup"><span data-stu-id="b76d6-130">Open Source, free but powerful?</span></span> <span data-ttu-id="b76d6-131">Potom PF je vhodné použít.</span><span class="sxs-lookup"><span data-stu-id="b76d6-131">Then PF is a good choice.</span></span> <span data-ttu-id="b76d6-132">Pomocí šablony [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup), stačí pouze pět minut nastavit brány firewall NAT. s vyrovnávání pomocí FreeBSD zatížení pomocí kruhového dotazování je PF v Azure pro běžný scénář webového serveru.</span><span class="sxs-lookup"><span data-stu-id="b76d6-132">By using the template [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup), you only need five minutes to set up a NAT firewall with round-robin load balancing using FreeBSD's PF in Azure for common web server scenario.</span></span> 

<span data-ttu-id="b76d6-133">Pokud chcete získat další nabídku FreeBSD v Azure, podívejte se na [Úvod do FreeBSD v Azure](freebsd-intro-on-azure.md).</span><span class="sxs-lookup"><span data-stu-id="b76d6-133">If you want to learn the offering of FreeBSD in Azure, refer to [introduction to FreeBSD on Azure](freebsd-intro-on-azure.md).</span></span>

<span data-ttu-id="b76d6-134">Pokud chcete získat další informace o souboru PF, podívejte se na [FreeBSD k používání](https://www.freebsd.org/doc/handbook/firewalls-pf.html) nebo [PF – uživatelská příručka](https://www.freebsd.org/doc/handbook/firewalls-pf.html).</span><span class="sxs-lookup"><span data-stu-id="b76d6-134">If you want to know more about PF, refer to [FreeBSD handbook](https://www.freebsd.org/doc/handbook/firewalls-pf.html) or [PF-User's Guide](https://www.freebsd.org/doc/handbook/firewalls-pf.html).</span></span>
