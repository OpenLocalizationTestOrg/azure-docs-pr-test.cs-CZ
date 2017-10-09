---
title: "aaaCreate interní nástroj pro vyrovnávání - zatížení, rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate interní zátěže pomocí rozhraní příkazového řádku Azure ve službě Správce prostředků"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c7a24e92-b4da-43c0-90f2-841c1b7ce489
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 3aea6fdb07600f0d661ec6b8ffc784b03380a127
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-by-using-hello-azure-cli"></a>Vytvořit interní nástroj pomocí hello rozhraní příkazového řádku Azure

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Šablona](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).  Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](load-balancer-get-started-ilb-classic-cli.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-hello-solution-by-using-hello-azure-cli"></a>Nasazení řešení hello pomocí hello rozhraní příkazového řádku Azure

Hello následující kroky ukazují, jak toocreate internetové nástroj pro vyrovnávání zatížení pomocí rozhraní příkazového řádku Azure Resource Manager. S Azure Resource Manager, každého prostředku se vytvoří a konfigurovat individuálně a potom se spojí dohromady toocreate prostředku.

Je třeba toocreate a nakonfigurovat hello následující objekty toodeploy nástroj pro vyrovnávání zatížení:

* **Konfigurace front-endových IP adres**: obsahuje veřejné IP adresy pro příchozí síťový provoz.
* **Fond back-end adres**: obsahuje síťová rozhraní (NIC), které umožňují hello virtuální počítače tooreceive síťový provoz z nástroje pro vyrovnávání zatížení hello
* **Pravidla Vyrovnávání zatížení**: obsahuje pravidla, které mapují veřejný port tooport nástroje pro vyrovnávání zatížení hello ve fondu adres back-end hello
* **Příchozí pravidla NAT**: obsahuje pravidla, které mapují veřejný port na hello zatížení vyrovnávání tooa portu pro konkrétní virtuální počítač ve fondu adres back-end hello
* **Sondy**: obsahuje sondy stavu, které jsou používané toocheck hello dostupnosti instancí virtuálních počítačů ve fondu adres back-end hello

Další informace najdete v tématu [Podpora služby Load Balancer v Azure Resource Manageru](load-balancer-arm.md).

## <a name="set-up-cli-toouse-resource-manager"></a>Nastavení rozhraní příkazového řádku toouse Resource Manager

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md). Postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.
2. Spustit hello **azure konfigurace režim** příkaz režimu Manager tooResource tooswitch, následujícím způsobem:

    ```azurecli
    azure config mode arm
    ```

    Očekávaný výstup:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Vytvoření interního nástroje pro vyrovnávání zatížení krok za krokem

1. Přihlaste se tooAzure.

    ```azurecli
    azure login
    ```

    Po zobrazení výzvy zadejte své přihlašovací údaje Azure.

2. Změna režimu Resource Manager tooAzure nástroje příkaz hello.

    ```azurecli
    azure config mode arm
    ```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Všechny prostředky v Azure Resource Manageru jsou přidruženy ke skupině prostředků. Pokud jste tak ještě neučinili, vytvořte skupinu prostředků.

```azurecli
azure group create <resource group name> <location>
```

## <a name="create-an-internal-load-balancer-set"></a>Vytvoření sady interního nástroje pro vyrovnávání zatížení

1. Vytvořte interní nástroj pro vyrovnávání zatížení.

    V následujícím scénáři hello se vytvoří skupinu prostředků s názvem nrprg v oblasti Východ USA.

    ```azurecli
    azure network lb create --name nrprg --location eastus
    ```

   > [!NOTE]
   > Všechny prostředky pro interní služby load balancer, jako je například virtuální sítě a podsítě virtuální sítě, musí být v hello stejnou skupinu prostředků a v hello stejné oblasti.

2. Vytvořte front-end IP adresu pro vyrovnávání zatížení interní hello.

    Hello IP adresu, která používáte musí být v rozsahu hello podsítě virtuální sítě.

    ```azurecli
    azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet
    ```

3. Vytvořte fond back-end adres hello.

    ```azurecli
    azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb
    ```

    Po definování front-endové IP adresy a back-endového fondu adres můžete vytvořit pravidla nástroje pro vyrovnávání zatížení, pravidla příchozího překladu adres (NAT) a vlastní testy stavu.

4. Vytvořte pravidlo Vyrovnávání zatížení nástroje pro vyrovnávání zatížení interní hello.

    Pokud budete postupovat podle předchozích kroků hello, hello příkaz vytvoří pravidlo Vyrovnávání zatížení pro naslouchání tooport 1433 ve front-endu fondu hello a odesílání Vyrovnávání zatížení provozu toohello back-end fondu síťových adres, také používá port 1433.

    ```azurecli
    azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb
    ```

5. Vytvořte pravidla příchozího překladu adres (NAT).

    Příchozí pravidla NAT jsou koncové body používané toocreate, nástroji pro vyrovnávání zatížení, které přejděte instance tooa konkrétní virtuální počítač. předchozí kroky Hello vytvořit dvě pravidla NAT pro vzdálenou plochu.

    ```azurecli
    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389
    ```

6. Vytvořte sondy stavu služby pro vyrovnávání zatížení hello.

    Test stavu kontroluje všechny instance virtuálního počítače toomake se, že se mohou odesílat provoz sítě. Hello instanci virtuálního počítače s kontroly selhání testu se odebere z nástroje pro vyrovnávání zatížení hello dokud přejde do režimu online a kontrolu testu Určuje, že je v pořádku.

    ```azurecli
    azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4
    ```

    > [!NOTE]
    > Platforma Microsoft Azure Hello používá statické veřejně směrovatelné IPv4 adresu pro různé scénáře pro správu. Hello IP adresa je 168.63.129.16. Tuto IP adresu by neměla blokovat žádná brána firewall, protože by to mohlo způsobit neočekávané chování.
    > S ohledem tooAzure interní Vyrovnávání zatížení se sledováním sondy z hello zatížení vyrovnávání toodetermine hello stav pro virtuální počítače v sadě Vyrovnávání zatížení sítě používá tuto IP adresu. Pokud skupina zabezpečení sítě je použité toorestrict provoz tooAzure virtuálních počítačů v sadu interně Vyrovnávání zatížení sítě nebo je použité tooa podsíť virtuální sítě, ujistěte se, zda pravidla zabezpečení sítě je přidána tooallow provoz z 168.63.129.16.

## <a name="create-nics"></a>Vytvoření síťových rozhraní

Třeba toocreate síťových adaptérů (nebo upravit existující) a přidružovat je sondy, tooNAT pravidla a pravidla nástroje pro vyrovnávání zatížení.

1. Vytvořte síťový adaptér s názvem *lb nic1 být*a přidružte ji k hello *rdp1* NAT pravidlo a hello *beilb* fond adres back-end.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus
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

2. Vytvořte síťový adaptér s názvem *lb nic2 být*a přidružte ji k hello *rdp2* NAT pravidlo a hello *beilb* fond adres back-end.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus
    ```

3. Vytvoření virtuálního počítače s názvem *DB1*a přidružte ji k hello síťový adaptér s názvem *lb nic1 být*. Účet úložiště s názvem *web1nrp* je vytvořen před hello následující příkaz spustí:

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```
    > [!IMPORTANT]
    > Virtuální počítače v zatížení vyrovnávání nutné toobe v hello stejné skupině dostupnosti. Použití `azure availset create` toocreate sadu dostupnosti.

4. Vytvoření virtuálního počítače (VM) s názvem *DB2*a přidružte ji k hello síťový adaptér s názvem *lb nic2 být*. Účet úložiště s názvem *web1nrp* byl vytvořen před spuštěním hello následující příkaz.

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="delete-a-load-balancer"></a>Odstranění nástroje pro vyrovnávání zatížení

tooremove nástroj pro vyrovnávání zatížení hello použijte následující příkaz:

```azurecli
azure network lb delete --resource-group nrprg --name ilbset
```

## <a name="next-steps"></a>Další kroky

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení pomocí spřažení se zdrojovou IP adresou](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)

