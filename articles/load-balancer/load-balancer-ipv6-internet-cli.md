---
title: "Služba Vyrovnávání zatížení aaaCreate směřujících Internetu s IPv6 – rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate k Internetu přístupných pro vyrovnávání zatížení s IPv6 v Azure Resource Manager pomocí rozhraní příkazového řádku Azure"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
keywords: "protokol IPv6, nástroje pro vyrovnávání zatížení azure, duálním zásobníkem, veřejnou IP adresu, nativní protokol ipv6, mobilní, iot"
ms.assetid: a1957c9c-9c1d-423e-9d5c-d71449bc1f37
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 7ff75ac90d74a74e3d0c27672b36fbd955a086a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-hello-azure-cli"></a>Vytvoření internetové Vyrovnávání zatížení s IPv6 v správce Azure Resource Manager pomocí hello rozhraní příkazového řádku Azure

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Azure CLI](load-balancer-ipv6-internet-cli.md)
> * [Šablona](load-balancer-ipv6-internet-template.md)

Azure Load Balancer je nástroj pro vyrovnávání zatížení úrovně 4 (TCP, UDP). Nástroj pro vyrovnávání zatížení Hello poskytuje vysokou dostupnost distribucí příchozí komunikaci mezi instance pořádku služby ve cloudových službách nebo virtuálních počítačů v sadě nástroje pro vyrovnávání zatížení. Azure Load Balancer můžete také tyto služby prezentovat na více portech, více IP adresách nebo obojím.

## <a name="example-deployment-scenario"></a>Příklad scénáře nasazení

Hello následující diagram znázorňuje řešení vyrovnávání zatížení hello nasazuje pomocí šablony příklad hello popsané v tomto článku.

![Scénář nástroje pro vyrovnávání zatížení](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

V tomto scénáři vytvoříte následující prostředky Azure hello:

* dva virtuální počítače (VM)
* rozhraní virtuální sítě pro každý virtuální počítač s oba protokoly IPv4 a IPv6 adresy přiřazené
* Vyrovnávání zatížení straně Internetu s IPv4 a IPv6 veřejnou IP adresu
* toothat sadu dostupnosti obsahuje hello dva virtuální počítače
* dva načíst vyrovnávání pravidla toomap hello veřejné VIP toohello privátní koncové body

## <a name="deploying-hello-solution-using-hello-azure-cli"></a>Nasazení řešení hello pomocí hello rozhraní příkazového řádku Azure

Hello následující kroky ukazují, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí rozhraní příkazového řádku Azure Resource Manager. S Azure Resource Manager, každého prostředku se vytvoří a konfigurovat individuálně a potom se spojí dohromady toocreate prostředku.

toodeploy nástroj pro vyrovnávání zatížení, můžete vytvořit a nakonfigurovat hello následující objekty:

* Konfigurace front-endových IP adres – obsahuje veřejné IP adresy pro příchozí síťový provoz.
* Fond adres back-end – obsahuje síťová rozhraní (NIC) pro hello virtuální počítače tooreceive síťový provoz z nástroje pro vyrovnávání zatížení hello.
* Pravidla pro vyrovnávání zatížení – obsahuje pravidla mapování veřejný port tooport nástroje pro vyrovnávání zatížení hello ve fondu adres back-end hello.
* Příchozí pravidla NAT – obsahuje pravidla mapování veřejný port na hello zatížení vyrovnávání tooa portu pro konkrétní virtuální počítač ve fondu adres back-end hello.
* Sondy – obsahuje dostupnosti toocheck stavu sondy používané instancí virtuálních počítačů ve fondu adres back-end hello.

Další informace najdete v tématu [Podpora služby Load Balancer v Azure Resource Manageru](load-balancer-arm.md).

## <a name="set-up-your-cli-environment-toouse-azure-resource-manager"></a>Nastavení prostředí toouse vaše rozhraní příkazového řádku Azure Resource Manager

V tomto příkladu používáme hello nástrojů příkazového řádku v příkazovém okně prostředí PowerShell. Jsme nejsou pomocí rutin prostředí Azure PowerShell hello ale také používáme skriptování možnosti tooimprove čitelnost a opětovné používání Powershellu.

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.
2. Spustit hello **azure konfigurace režim** příkaz tooswitch tooResource Manager režimu.

    ```azurecli
    azure config mode arm
    ```

    Očekávaný výstup:

        info:    New mode is arm

3. Přihlaste se tooAzure a získejte seznam předplatných.

    ```azurecli
    azure login
    ```

    Zadejte vaše přihlašovací údaje Azure po zobrazení výzvy.

    ```azurecli
    azure account list
    ```

    Vyberte hello odběr, který chcete toouse. Poznamenejte si Id předplatného hello hello další krok.

4. Nastavení proměnných prostředí PowerShell pro použití s hello rozhraní příkazového řádku.

    ```powershell
    $subscriptionid = "########-####-####-####-############"  # enter subscription id
    $location = "southcentralus"
    $rgName = "pscontosorg1southctrlus09152016"
    $vnetName = "contosoIPv4Vnet"
    $vnetPrefix = "10.0.0.0/16"
    $subnet1Name = "clicontosoIPv4Subnet1"
    $subnet1Prefix = "10.0.0.0/24"
    $subnet2Name = "clicontosoIPv4Subnet2"
    $subnet2Prefix = "10.0.1.0/24"
    $dnsLabel = "contoso09152016"
    $lbName = "myIPv4IPv6Lb"
    ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Vytvoření skupiny prostředků, nástroj pro vyrovnávání zatížení, virtuální sítě a podsítě

1. Vytvoření skupiny prostředků

    ```azurecli
    azure group create $rgName $location
    ```

2. Vytvoření nástroje pro vyrovnávání zatížení

    ```azurecli
    $lb = azure network lb create --resource-group $rgname --location $location --name $lbName
    ```

3. Vytvořte virtuální síť (VNet).

    ```azurecli
    $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix
    ```

    Vytvořte dvě podsítě v této virtuální sítě.

    ```azurecli
    $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
    $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName
    ```

## <a name="create-public-ip-addresses-for-hello-front-end-pool"></a>Vytvoření veřejné IP adresy pro front-end fond hello

1. Nastavení proměnných prostředí PowerShell hello

    ```powershell
    $publicIpv4Name = "myIPv4Vip"
    $publicIpv6Name = "myIPv6Vip"
    ```

2. Vytvoření veřejné IP hello front-end IP fondu adres.

    ```azurecli
    $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
    $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel
    ```

    > [!IMPORTANT]
    > Nástroj pro vyrovnávání zatížení Hello používá hello domény popisek hello veřejnou IP adresu jako jeho plně kvalifikovaný název domény. Tuto změnu z klasického nasazení, který používá název cloudové služby hello jako hello nástroj pro vyrovnávání zatížení plně kvalifikovaný název domény.
    > V tomto příkladu hello plně kvalifikovaný název domény je *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Vytvořte front-end a back endové fondy

Tento příklad vytvoří hello front-end IP fond, který obdrží hello příchozí síťový provoz na Vyrovnávání zatížení hello a kde hello front-end fondu odešle hello skupinu s vyrovnáváním zatížení síťového provozu fondu hello back-end IP adres.

1. Nastavení proměnných prostředí PowerShell hello

    ```powershell
    $frontendV4Name = "FrontendVipIPv4"
    $frontendV6Name = "FrontendVipIPv6"
    $backendAddressPoolV4Name = "BackendPoolIPv4"
    $backendAddressPoolV6Name = "BackendPoolIPv6"
    ```

2. Vytvořte front-end IP fond přidružení hello veřejnou IP adresu, vytvořili v předchozím kroku hello a nástroj pro vyrovnávání zatížení hello.

    ```azurecli
    $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
    $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
    $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
    $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName
    ```

## <a name="create-hello-probe-nat-rules-and-lb-rules"></a>Vytvoření testu hello, pravidla NAT a pravidel LB

Tento příklad vytvoří hello následující položky:

* toocheck pravidla testu pro připojení k tooTCP port 80
* zařízení NAT pravidlo tootranslate všechny příchozí přenosy na portu 3389 tooport 3389 pro protokol RDP<sup>1</sup>
* zařízení NAT pravidlo tootranslate všechny příchozí přenosy na portu 3391 tooport 3389 pro protokol RDP<sup>1</sup>
* toobalance pravidlo Vyrovnávání zatížení všechny příchozí přenosy na portu 80 tooport 80 na hello adresy ve fondu back-end hello.

<sup>1</sup> pravidel NAT, která jsou přidružená tooa instance konkrétní virtuální počítač za nástroj pro vyrovnávání zatížení hello. Hello síťový provoz na portu 3389 přicházejících odešle toohello konkrétní virtuální počítač a port pravidla NAT hello přidružený. Pro pravidlo překladu adres (NAT) je nutné zadat protokol (UDP nebo TCP). Oba protokoly nemůže být přiřazen toohello stejný port.

1. Nastavení proměnných prostředí PowerShell hello

    ```powershell
    $probeV4V6Name = "ProbeForIPv4AndIPv6"
    $natRule1V4Name = "NatRule-For-Rdp-VM1"
    $natRule2V4Name = "NatRule-For-Rdp-VM2"
    $lbRule1V4Name = "LBRuleForIPv4-Port80"
    $lbRule1V6Name = "LBRuleForIPv6-Port80"
    ```

2. Vytvořit test paměti hello

    Hello následující příklad vytvoří sondou TCP, který kontroluje pro připojení k tooback-end port TCP 80 každých 15 sekund. Ho označíte hello back-end prostředku není k dispozici po dvě po sobě jdoucích selhání.

    ```azurecli
    $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName
    ```

3. Vytvoření příchozích pravidel NAT, které umožňují připojení RDP toohello back endové prostředky

    ```azurecli
    $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
    $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName
    ```

4. Vytvořit pravidla, která odesílat provoz porty toodifferent back-end v závislosti na tom, které front-endu přijal žádost hello Vyrovnávání zatížení

    ```azurecli
    $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
    $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName
    ```

5. Zkontrolujte nastavení

    ```azurecli
    azure network lb show --resource-group $rgName --name $lbName
    ```

    Očekávaný výstup:

        info:    Executing command network lb show
        info:    Looking up hello load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show

## <a name="create-nics"></a>Vytvoření síťových rozhraní

Vytvořte síťové adaptéry a přiřaďte je sondy, tooNAT pravidla a pravidla nástroje pro vyrovnávání zatížení.

1. Nastavení proměnných prostředí PowerShell hello

    ```powershell
    $nic1Name = "myIPv4IPv6Nic1"
    $nic2Name = "myIPv4IPv6Nic2"
    $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
    $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
    $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
    $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
    $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
    $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"
    ```

2. Vytvořte síťovou kartu pro každý back-end a přidejte konfigurace protokolu IPv6.

    ```azurecli
    $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
    $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

    $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule2V4Id
    $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name
    ```

## <a name="create-hello-back-end-vm-resources-and-attach-each-nic"></a>Vytvořte prostředky virtuálních počítačů hello back-end a připojte každý síťový adaptér

toocreate virtuální počítače, musí mít účet úložiště. Pro vyrovnávání zatížení, musí virtuální počítače hello toobe členy skupiny dostupnosti. Další informace o vytváření virtuálních počítačů najdete v tématu [vytvoření virtuálního počítače Azure pomocí prostředí PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)

1. Nastavení proměnných prostředí PowerShell hello

    ```powershell
    $storageAccountName = "ps08092016v6sa0"
    $availabilitySetName = "myIPv4IPv6AvailabilitySet"
    $vm1Name = "myIPv4IPv6VM1"
    $vm2Name = "myIPv4IPv6VM2"
    $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
    $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
    $disk1Name = "WindowsVMosDisk1"
    $disk2Name = "WindowsVMosDisk2"
    $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
    $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
    $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
    $vmUserName = "vmUser"
    $mySecurePassword = "PlainTextPassword*1"
    ```

    > [!WARNING]
    > Tento příklad používá hello uživatelské jméno a heslo pro virtuální počítače hello ve formátu prostého textu. Odpovídající musí dát pozor při použití pověření hello zrušte. Bezpečnější způsob zpracování pověření v prostředí PowerShell, najdete v části hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) rutiny.

2. Vytvořit sadu dostupnosti a účet úložiště hello

    Při vytváření hello virtuálních počítačů, může použít existující účet úložiště. Hello následující příkaz vytvoří nový účet úložiště.

    ```azurecli
    $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"
    ```

    Dále vytvořte skupinu dostupnosti hello.

    ```azurecli
    $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location
    ```

3. Vytvořit hello virtuální počítače s hello přidružené síťové karty

    ```azurecli
    $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

    $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension
    ```

## <a name="next-steps"></a>Další kroky

[Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
