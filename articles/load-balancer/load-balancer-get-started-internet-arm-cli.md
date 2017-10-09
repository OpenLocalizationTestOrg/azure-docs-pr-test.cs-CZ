---
title: "aaaCreate směřujících Internetu pro vyrovnávání zátěže - rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate k Internetu přístupných pro vyrovnávání zatížení v Resource Manager pomocí rozhraní příkazového řádku Azure"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a8bcdd88-f94c-4537-8143-c710eaa86818
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: cadb5edb3b4a4e2f0813109d027eaafdc7ef7303
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-internet-load-balancer-using-hello-azure-cli"></a>Vytváření k Internetu pro vyrovnávání zatížení pomocí hello rozhraní příkazového řádku Azure

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Šablona](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení Resource Manager hello. Můžete také [zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí klasického nasazení](load-balancer-get-started-internet-classic-portal.md)

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-hello-solution-using-hello-azure-cli"></a>Nasazení řešení hello pomocí hello rozhraní příkazového řádku Azure

Hello následující kroky ukazují, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí rozhraní příkazového řádku Azure Resource Manager. S Azure Resource Manager každý prostředek je vytvořen a nakonfigurován individuálně, potom připravili toocreate prostředku.

Musíte vytvořit a konfigurovat hello následující objekty toodeploy nástroj pro vyrovnávání zatížení:

* Konfigurace front-endových IP adres – obsahuje veřejné IP adresy pro příchozí síťový provoz.
* Fond adres back-end – obsahuje síťová rozhraní (NIC) pro hello virtuální počítače tooreceive síťový provoz z nástroje pro vyrovnávání zatížení hello.
* Pravidla pro vyrovnávání zatížení – obsahuje pravidla mapování veřejný port tooport nástroje pro vyrovnávání zatížení hello ve fondu adres back-end hello.
* Příchozí pravidla NAT – obsahuje pravidla mapování veřejný port na hello zatížení vyrovnávání tooa portu pro konkrétní virtuální počítač ve fondu adres back-end hello.
* Sondy – obsahuje dostupnosti toocheck stavu sondy používané instancí virtuálních počítačů ve fondu adres back-end hello.

Další informace najdete v tématu [Podpora služby Load Balancer v Azure Resource Manageru](load-balancer-arm.md).

## <a name="set-up-cli-toouse-resource-manager"></a>Nastavení rozhraní příkazového řádku toouse Resource Manager

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.
2. Spustit hello **azure konfigurace režim** příkaz tooswitch tooResource Manager režimu, jak je uvedeno níže.

    ```azurecli
        azure config mode arm
    ```

    Očekávaný výstup:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a>Vytvořit virtuální síť a veřejnou IP adresu pro fond hello front-end IP adres

1. Vytvořit virtuální síť (VNet) s názvem *NRPVnet* v umístění východní USA hello pomocí skupinu prostředků s názvem *NRPRG*.

    ```azurecli
        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16
    ```

    Ve virtuální síti *NRPVnet* vytvořte podsíť s názvem *NRPVnetSubnet* s blokem CIDR 10.0.0.0/24.

    ```azurecli
        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24
    ```

2. Vytvoření veřejné IP adresy s názvem *NRPPublicIP* toobe používá front-endu fond IP adres s názvem DNS *loadbalancernrp.eastus.cloudapp.azure.com*. následující příkaz hello používá hello statického přidělování typ a časový limit nečinnosti 4 minuty.

    ```azurecli
        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4
    ```

   > [!IMPORTANT]
   > Nástroj pro vyrovnávání zatížení Hello použije hello domény popisek hello veřejnou IP adresu jako jeho plně kvalifikovaný název domény. Tuto změnu z klasického nasazení, který používá hello cloudové služby jako hello nástroj pro vyrovnávání zatížení plně kvalifikovaný název domény (FQDN).
   > V tomto příkladu hello plně kvalifikovaný název domény je *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Vytvoření nástroje pro vyrovnávání zatížení

Hello následující příkaz vytvoří nástroj pro vyrovnávání zatížení s názvem *NRPlb* v hello *NRPRG* skupiny prostředků v hello *východní USA* umístění Azure.

    ```azurecli
    azure network lb create NRPRG NRPlb eastus
    ```

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Vytvoření front-endového fondu IP adres a back-endového fondu adres
Tento příklad ukazuje, jak toocreate hello front-end IP fond, který obdrží hello příchozí síťový provoz na dobrý den, nástroj pro vyrovnávání zatížení a hello kde hello front-end fondu odešle hello skupinu s vyrovnáváním zatížení síťového provozu fond back-end IP adres.

1. Vytvořte front-end IP fond přidružení hello veřejnou IP adresu, vytvořili v předchozím kroku hello a nástroj pro vyrovnávání zatížení hello.

    ```azurecli
        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip
    ```

2. Nastavení fondu back-end adresy použít tooreceive příchozí provoz z fondu IP front-endu hello.

    ```azurecli
        azure network lb address-pool create NRPRG NRPlb NRPbackendpool
    ```

## <a name="create-lb-rules-nat-rules-and-probe"></a>Vytvoření pravidel nástroje pro vyrovnávání zatížení, pravidel překladu adres (NAT) a testu

Tento příklad vytvoří hello následující položky.

* zařízení NAT pravidlo tootranslate všechny příchozí přenosy na portu 21 tooport 22<sup>1</sup>
* zařízení NAT pravidlo tootranslate všechny příchozí přenosy na portu 23 tooport 22
* toobalance pravidlo Vyrovnávání zatížení všechny příchozí přenosy na portu 80 tooport 80 na hello adresy ve fondu back-end hello.
* Stav testu toocheck pravidlo hello na stránku s názvem *HealthProbe.aspx*.

<sup>1</sup> pravidel NAT, která jsou přidružená tooa instance konkrétní virtuální počítač za nástroj pro vyrovnávání zatížení hello. Hello síťový provoz na portu 21 přicházejících se odesílají na port 22, které jsou spojené s tímto pravidlem NAT tooa konkrétní virtuální počítač. Pro pravidlo překladu adres (NAT) je nutné zadat protokol (UDP nebo TCP). Oba protokoly nemůže být přiřazen toohello stejný port.

1. Vytvoření pravidla NAT hello.

    ```azurecli
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22
    ```

2. Vytvořte pravidlo nástroje pro vyrovnávání zatížení.

    ```azurecli
        azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name NRPfrontendpool --backend-address-pool-name NRPbackendpool
    ```

3. Vytvořte test stavu.

    ```azurecli
        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4
    ```

4. Zkontrolujte nastavení.

    ```azurecli
        azure network lb show nrprg nrplb
    ```

    Očekávaný výstup:

        info:    Executing command network lb show
        + Looking up hello load balancer "nrplb"
        + Looking up hello public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>Vytvoření síťových rozhraní

Třeba toocreate síťových adaptérů (nebo upravit existující) a přidružovat je sondy, tooNAT pravidla a pravidla nástroje pro vyrovnávání zatížení.

1. Vytvořit síťový adaptér s názvem *lb nic1 být*a přidružte ji k hello *rdp1* NAT pravidlo a hello *NRPbackendpool* fond adres back-end.

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus
    ```

    Očekávaný výstup:

        info:    Executing command network nic create
        + Looking up hello network interface "lb-nic1-be"
        + Looking up hello subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up hello network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Vytvořit síťový adaptér s názvem *lb nic2 být*a přidružte ji k hello *rdp2* NAT pravidlo a hello *NRPbackendpool* fond adres back-end.

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus
    ```

3. Vytvoření virtuálního počítače (VM) s názvem *web1*a přidružte ji k hello síťový adaptér s názvem *lb nic1 být*. Účet úložiště s názvem *web1nrp* byl vytvořen před spuštěním příkazu hello níže.

    ```azurecli
        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

    > [!IMPORTANT]
    > Virtuální počítače v zatížení vyrovnávání nutné toobe v hello stejné skupině dostupnosti. Použití `azure availset create` toocreate sadu dostupnosti.

    výstup Hello by měl vypadat podobně jako toohello následující:

        info:    Executing command vm create
        + Looking up hello VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using hello VM Size "Standard_A1"
        info:    hello [OS, Data] Disk or image configuration requires storage account
        + Looking up hello storage account web1nrp
        + Looking up hello availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up hello NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in hello NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    > [!NOTE]
    > informační zpráva Hello **to je síťový adaptér bez publicIP nakonfigurované** očekává se, protože hello vytvořit síťovou kartu pro vyrovnávání zatížení hello připojení tooInternet pomocí hello zatížení vyrovnávání veřejnou IP adresu.

    Od hello *lb nic1 být* síťový adaptér je přidružen hello *rdp1* pravidla NAT, se můžete připojit příliš*web1* pomocí protokolu RDP přes port 3441 na Vyrovnávání zatížení hello.

4. Vytvoření virtuálního počítače (VM) s názvem *web2*a přidružte ji k hello síťový adaptér s názvem *lb nic2 být*. Účet úložiště s názvem *web1nrp* byl vytvořen před spuštěním příkazu hello níže.

    ```azurecli
        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="update-an-existing-load-balancer"></a>Aktualizace stávajícího nástroje pro vyrovnávání zatížení
Můžete přidat pravidla odkazující na stávající nástroj pro vyrovnávání zatížení. V dalším příkladu hello je přidat nové pravidlo Vyrovnávání zatížení tooan existující nástroj pro vyrovnávání zatížení **NRPlb**

```azurecli
azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool
```

## <a name="delete-a-load-balancer"></a>Odstranění nástroje pro vyrovnávání zatížení
Použijte následující příkaz tooremove nástroj pro vyrovnávání zatížení hello:

```azurecli
azure network lb delete --resource-group nrprg --name nrplb
```

## <a name="next-steps"></a>Další kroky
[Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
