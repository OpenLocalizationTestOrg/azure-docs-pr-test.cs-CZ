---
title: aaaNetworking vzory pro Azure Service Fabric | Microsoft Docs
description: "Popisuje běžné sítě vzory pro Service Fabric a jak toocreate cluster pomocí Azure síťových funkcí."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2017
ms.author: ryanwi
ms.openlocfilehash: 5973e3f9917076c6a36e71443ec256e0f414ff87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-networking-patterns"></a>Vzory sítě Service Fabric
Cluster Azure Service Fabric může integrovat další funkce Azure sítě. V tomto článku jsme ukážeme, jak toocreate clusterů této hello používá následující funkce:

- [Existující virtuální síť nebo podsíť](#existingvnet)
- [Statickou veřejnou IP adresu](#staticpublicip)
- [Pouze interní zátěže.](#internallb)
- [Nástroj pro vyrovnávání zatížení pro vnitřní a vnější](#internalexternallb)

Service Fabric se spustí ve škálovací sadě standardní virtuální počítač. Žádné funkce, které můžete použít v škálovací sadu virtuálních počítačů, můžete se cluster Service Fabric. Hello sítě části hello šablon Azure Resource Manageru pro sady škálování virtuálního počítače a služby prostředků infrastruktury jsou identické. Poté, co nasadíte tooan existující virtuální sítě, je snadné tooincorporate dalších síťových funkcí, jako je Azure ExpressRoute, Azure VPN Gateway, skupinu zabezpečení sítě a partnerský vztah virtuální sítě.

Service Fabric se liší od dalších síťových funkcí v jeden aspekt. Hello [portál Azure](https://portal.azure.com) interně používá hello Service Fabric prostředků zprostředkovatele toocall tooa clusteru tooget informace o uzly a aplikací. Poskytovatel prostředků Service Fabric Hello vyžaduje veřejně přístupná příchozí přístup toohello HTTP brány port (port 19080, ve výchozím nastavení) na koncový bod správy hello. [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) používá hello toomanage koncový bod správy clusteru. Poskytovatel prostředků Service Fabric Hello také používá tento port tooquery informace o clusteru, toodisplay v hello portálu Azure. 

Pokud port 19080 není přístupný ze zprostředkovatele prostředků hello Service Fabric, třeba zprávu *uzly nebyl nalezen* se zobrazí v hello portál, a seznamu uzlů a aplikace, zobrazí se prázdný. Pokud chcete toosee cluster v hello portálu Azure, nástroj pro vyrovnávání zatížení musí vystavit veřejnou IP adresu a vaše skupina zabezpečení sítě musí umožňovat příchozí port 19080 provoz. Pokud vaše instalace těchto požadavků nesplňuje, nezobrazuje hello portál Azure hello stav clusteru.

## <a name="templates"></a>Šablony

Všechny šablony Service Fabric se v [jeden soubor ke stažení souboru](https://msdnshared.blob.core.windows.net/media/2016/10/SF_Networking_Templates.zip). Musí být schopný toodeploy hello šablony jako-pomocí hello následující příkazy prostředí PowerShell. Pokud nasazujete hello existující virtuální síť Azure nebo hello statické veřejné IP šablona, nejdřív přečíst hello [počáteční instalace](#initialsetup) tohoto článku.

<a id="initialsetup"></a>
## <a name="initial-setup"></a>Počáteční nastavení

### <a name="existing-virtual-network"></a>Existující virtuální síť

V následujícím příkladu hello, můžeme začít s existující virtuální síť s názvem ExistingRG-vnet v hello **ExistingRG** skupinu prostředků. podsíť Hello se nazývá výchozí. Tyto výchozí prostředky jsou vytvořeny při použití hello Azure portálu toocreate standardní virtuální počítač (VM). Můžete vytvořit hello virtuální síť a podsíť bez vytvoření hello virtuálních počítačů, ale přidání clusteru tooan existující virtuální síť hello hlavním cílem je tooprovide síťové připojení tooother virtuálních počítačů. Vytváření hello virtuálních počítačů poskytuje dobrým příkladem jak se obvykle používá existující virtuální síť. Pokud váš cluster Service Fabric používá jenom k interním pro vyrovnávání zatížení, bez veřejnou IP adresu, můžete použít hello virtuální počítač a jeho veřejná IP adresa jako zabezpečený *přejít pole*.

### <a name="static-public-ip-address"></a>Statickou veřejnou IP adresu

Statickou veřejnou IP adresu obecně je vyhrazený prostředek, který je spravován od něj odděleně hello virtuálního počítače nebo virtuální počítače je přiřazena k. Ve skupině prostředků vyhrazené sítě je zřízený (jako názvem na rozdíl od tooin hello Service Fabric skupinu prostředků clusteru sám sebe). Vytvořit statickou veřejnou IP adresu s názvem staticIP1 v hello stejnou skupinu prostředků ExistingRG v hello portál Azure nebo pomocí prostředí PowerShell:

```powershell
PS C:\Users\user> New-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG -Location westus -AllocationMethod Static -DomainNameLabel sfnetworking

Name                     : staticIP1
ResourceGroupName        : ExistingRG
Location                 : westus
Id                       : /subscriptions/1237f4d2-3dce-1236-ad95-123f764e7123/resourceGroups/ExistingRG/providers/Microsoft.Network/publicIPAddresses/staticIP1
Etag                     : W/"fc8b0c77-1f84-455d-9930-0404ebba1b64"
ResourceGuid             : 77c26c06-c0ae-496c-9231-b1a114e08824
ProvisioningState        : Succeeded
Tags                     :
PublicIpAllocationMethod : Static
IpAddress                : 40.83.182.110
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : null
DnsSettings              : {
                             "DomainNameLabel": "sfnetworking",
                             "Fqdn": "sfnetworking.westus.cloudapp.azure.com"
                           }
```

### <a name="service-fabric-template"></a>Šablona Service Fabric

V příkladech hello v tomto článku používáme template.json Service Fabric hello. Hello standardní portálu Průvodce toodownload hello šablonu z portálu hello můžete použít, před vytvořením clusteru. Také můžete použít jednu z šablon hello v hello [Galerie šablon](https://azure.microsoft.com/en-us/documentation/templates/?term=service+fabric), jako je hello [pěti uzly clusteru Service Fabric](https://azure.microsoft.com/en-us/documentation/templates/service-fabric-unsecure-cluster-5-node-1-nodetype/).

<a id="existingvnet"></a>
## <a name="existing-virtual-network-or-subnet"></a>Existující virtuální síť nebo podsíť

1. Změňte hello podsíť parametr toohello název hello existující podsíť a pak přidejte dva nové parametry tooreference hello existující virtuální sítě:

    ```
        "subnet0Name": {
                "type": "string",
                "defaultValue": "default"
            },
            "existingVNetRGName": {
                "type": "string",
                "defaultValue": "ExistingRG"
            },

            "existingVNetName": {
                "type": "string",
                "defaultValue": "ExistingRG-vnet"
            },
            /*
            "subnet0Name": {
                "type": "string",
                "defaultValue": "Subnet-0"
            },
            "subnet0Prefix": {
                "type": "string",
                "defaultValue": "10.0.0.0/24"
            },*/
    ```


2. Změna hello `vnetID` proměnné toopoint toohello existující virtuální síť:

    ```
            /*old "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",*/
            "vnetID": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingVNetRGName'), '/providers/Microsoft.Network/virtualNetworks/', parameters('existingVNetName'))]",
    ```

3. Odebrat `Microsoft.Network/virtualNetworks` z vašich prostředků, takže Azure nevytvoří nové virtuální sítě:

    ```
    /*{
    "apiVersion": "[variables('vNetApiVersion')]",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('virtualNetworkName')]",
    "location": "[parameters('computeLocation')]",
    "properities": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "subnets": [
            {
                "name": "[parameters('subnet0Name')]",
                "properties": {
                    "addressPrefix": "[parameters('subnet0Prefix')]"
                }
            }
        ]
    },
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    }
    },*/
    ```

4. Komentář hello virtuální sítě z hello `dependsOn` atribut `Microsoft.Compute/virtualMachineScaleSets`, takže nemusíte závisí na vytvoření nové virtuální sítě:

    ```
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Computer/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType0Name')]",
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        /*"[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
        */
        "[Concat('Microsoft.Storage/storageAccounts/', variables('uniqueStringArray0')[0])]",

    ```

5. Nasazení šablony hello:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingexistingvnet -Location westus
    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingexistingvnet -TemplateFile C:\SFSamples\Final\template\_existingvnet.json
    ```

    Po nasazení by měla obsahovat vaše virtuální síť hello nové škálovací sady virtuálních počítačů. Typ uzlu sady škálování virtuálního počítače Hello by měl zobrazit hello existující virtuální síť a podsíť. Můžete taky použít protokol RDP (Remote Desktop) tooaccess hello virtuální počítač, který již byl ve virtuální síti hello a tooping hello nové škálovací sady virtuálních počítačů:

    ```
    C:>\Users\users>ping 10.0.0.5 -n 1
    C:>\Users\users>ping NOde1000000 -n 1
    ```

Jiný příklad najdete v tématu [ten, který není konkrétní tooService Fabric](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet).


<a id="staticpublicip"></a>
## <a name="static-public-ip-address"></a>Statickou veřejnou IP adresu

1. Přidání parametrů pro název hello hello existující statická skupina prostředků IP, název a plně kvalifikovaný název domény (FQDN):

    ```
    "existingStaticIPResourceGroup": {
                "type": "string"
            },
            "existingStaticIPName": {
                "type": "string"
            },
            "existingStaticIPDnsFQDN": {
                "type": "string"
    }
    ```

2. Odebrat hello `dnsName` parametr. (hello statickou IP adresu již jedna.)

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

3. Přidejte proměnnou tooreference hello existující statickou IP adresu:

    ```
    "existingStaticIP": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingStaticIPResourceGroup'), '/providers/Microsoft.Network/publicIPAddresses/', parameters('existingStaticIPName'))]",
    ```

4. Odebrat `Microsoft.Network/publicIPAddresses` z vašich prostředků, takže Azure nevytvoří novou IP adresu:

    ```
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

5. Komentář hello IP adresu z hello `dependsOn` atribut `Microsoft.Network/loadBalancers`, takže nemusíte závisí na vytvoření novou IP adresu:

    ```
    "apiVersion": "[variables('lbIPApiVersion')]",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB', '-', parameters('clusterName'), '-', parameters('vmNodeType0Name'))]",
    "location": "[parameters('computeLocation')]",
    /*
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', concat(parameters('lbIPName'), '-', '0'))]"
    ], */
    "properties": {
    ```

6. V hello `Microsoft.Network/loadBalancers` prostředků, změna hello `publicIPAddress` element `frontendIPConfigurations` tooreference hello existující statické IP adresy místo nově vytvořený jeden:

    ```
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                "publicIPAddress": {
                                    /*"id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"*/
                                    "id": "[variables('existingStaticIP')]"
                                }
                            }
                        }
                    ],
    ```

7. V hello `Microsoft.ServiceFabric/clusters` prostředků, změna `managementEndpoint` toohello plně kvalifikovaný název domény DNS hello statickou IP adresu. Pokud používáte zabezpečení clusteru, ujistěte se, změníte *http://* příliš*https://*. (Všimněte si, že tento krok platí jenom tooService clustery infrastruktury. Pokud používáte škálovací sadu virtuálních počítačů, tento krok přeskočte.)

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',parameters('existingStaticIPDnsFQDN'),':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

8. Nasazení šablony hello:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingstaticip -Location westus

    $staticip = Get-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG

    $staticip

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingstaticip -TemplateFile C:\SFSamples\Final\template\_staticip.json -existingStaticIPResourceGroup $staticip.ResourceGroupName -existingStaticIPName $staticip.Name -existingStaticIPDnsFQDN $staticip.DnsSettings.Fqdn
    ```

Po nasazení můžete zjistit, že je nástroj pro vyrovnávání zatížení vázané toohello veřejnou statickou IP adresu z hello jiné skupině prostředků. Hello koncového bodu připojení klienta Service Fabric a [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) koncový bod bod toohello plně kvalifikovaný název domény DNS hello statickou IP adresu.

<a id="internallb"></a>
## <a name="internal-only-load-balancer"></a>Pouze interní zátěže.

Tento scénář nahrazuje hello externím vyrovnáváním zatížení v šabloně Service Fabric výchozí hello s nástrojem pro vyrovnávání zatížení pouze interní. Důsledky pro hello portál Azure a pro poskytovatele prostředků hello Service Fabric najdete v předcházející části hello.

1. Odebrat hello `dnsName` parametr. (Jej není nutné.)

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

2. Pokud používáte metodu statického přidělování, Volitelně můžete přidat parametr statických adres IP. Pokud používáte metody dynamického přidělení, není nutné toodo tento krok.

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

3. Odebrat `Microsoft.Network/publicIPAddresses` z vašich prostředků, takže Azure nevytvoří novou IP adresu:

    ```
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

4. Odeberte hello IP adresu `dependsOn` atribut `Microsoft.Network/loadBalancers`, takže nemusíte závisí na vytvoření nové adresy IP. Přidat virtuální síť hello `dependsOn` atributů, protože nástroj pro vyrovnávání zatížení hello teď závisí na podsíť hello z virtuální sítě hello:

    ```
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'))]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /*"[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"*/
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
    ```

5. Změnit Vyrovnávání zatížení hello `frontendIPConfigurations` ze pomocí nastavení `publicIPAddress`, toousing podsíť a `privateIPAddress`. `privateIPAddress`používá předdefinovanou statické interní IP adresu. toouse dynamickou IP adresu, odeberte hello `privateIPAddress` element a poté změňte `privateIPAllocationMethod` příliš**dynamické**.

    ```
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /*
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                } */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
    ```

6. V hello `Microsoft.ServiceFabric/clusters` prostředků, změna `managementEndpoint` adresa služby Vyrovnávání zatížení pro vnitřní toohello toopoint. Pokud používáte zabezpečení clusteru, ujistěte se, změníte *http://* příliš*https://*. (Všimněte si, že tento krok platí jenom tooService clustery infrastruktury. Pokud používáte škálovací sadu virtuálních počítačů, tento krok přeskočte.)

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',reference(variables('lbID0')).frontEndIPConfigurations[0].properties.privateIPAddress,':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

7. Nasazení šablony hello:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternallb -TemplateFile C:\SFSamples\Final\template\_internalonlyLB.json
    ```

Po nasazení používá nástroj pro vyrovnávání zatížení hello 10.0.0.250 privátní statickou IP adresu. Pokud máte jiný počítač ve stejné virtuální síti, můžete přejít toohello interní [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) koncový bod. Všimněte si, že se připojí tooone uzlů hello za nástroj pro vyrovnávání zatížení hello.

<a id="internalexternallb"></a>
## <a name="internal-and-external-load-balancer"></a>Nástroj pro vyrovnávání zatížení pro vnitřní a vnější

V tomto scénáři začínat hello existující typ s jedním uzlem externí nástroj pro vyrovnávání zatížení a přidejte interní nástroj pro hello stejný typ uzlu. Fond back-end adresy tooa back-end port připojený lze přiřadit pouze tooa jeden nástroj pro vyrovnávání zatížení. Zvolte, které nástroj pro vyrovnávání zatížení bude mít vaše aplikace porty a který nástroj pro vyrovnávání zatížení bude mít vaše koncové body správy (porty 19000 a 19080). Když vložíte koncových bodů pro správu hello na Vyrovnávání zatížení interní hello, mějte na paměti hello Service Fabric prostředků zprostředkovatele omezení popsané dříve v článku hello. V příkladu hello, které používáme koncových bodů pro správu hello zůstanou hello externím vyrovnáváním zatížení. Také přidat na port 80 aplikace portu a umístěte ji na Vyrovnávání zatížení interní hello.

V typu dva uzlu clusteru je jeden typ uzlu na hello externím vyrovnáváním zatížení. Hello jiný typ uzlu je na Vyrovnávání zatížení interní hello. toouse dva typ uzlu clusteru, v hello portál vytvořit typ uzlu dvě šablony (který se dodává s dva nástroje pro vyrovnávání zatížení), přepínače hello druhý vyrovnávání tooan interní služby load Vyrovnávání zatížení. Další informace najdete v tématu hello [nástroj pro vyrovnávání zatížení pouze pro interní](#internallb) části.

1. Přidejte hello statické interní služby load vyrovnávání IP adresu parametr. (Poznámky související toousing dynamickou IP adresu, naleznete v předchozích částech tohoto článku.)

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

2. Přidáte parametr aplikace port 80.

3. Interní verze tooadd hello existující sítě proměnné, zkopírovat a vložit a přidejte "-Int" toohello název:

    ```
    /* Add internal load balancer networking variables */
            "lbID0-Int": "[resourceId('Microsoft.Network/loadBalancers', concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal'))]",
            "lbIPConfig0-Int": "[concat(variables('lbID0-Int'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
            "lbPoolID0-Int": "[concat(variables('lbID0-Int'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
            "lbProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricGatewayProbe')]",
            "lbHttpProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricHttpGatewayProbe')]",
            "lbNatPoolID0-Int": "[concat(variables('lbID0-Int'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]",
            /* Internal load balancer networking variables end */
    ```

4. Pokud spustíte pomocí generované portál šablony hello, který používá aplikace port 80, hello výchozí šablony portálu přidá AppPort1 (port 80) na hello externím vyrovnáváním zatížení. V takovém případě odebrání hello externím vyrovnáváním zatížení AppPort1 `loadBalancingRules` a sondy, takže ho můžete přidat toohello interní vyrovnávání zátěže:

    ```
    "loadBalancingRules": [
        {
            "name": "LBHttpRule",
            "properties":{
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[variables('lbHttpProbeID0')]"
                },
                "protocol": "tcp"
            }
        } /* Remove AppPort1 from hello external load balancer.
        {
            "name": "AppPortLBRule1",
            "properties": {
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('loadBalancedAppPort1')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[concate(variables('lbID0'), '/probes/AppPortProbe1')]"
                },
                "protocol": "tcp"
            }
        }*/

    ],
    "probes": [
        {
            "name": "FabricGatewayProbe",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricTcpGatewayPort')]",
                "protocol": "tcp"
            }
        },
        {
            "name": "FabricHttpGatewayProbe",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricHttpGatewayPort')]",
                "protocol": "tcp"
            }
        } /* Remove AppPort1 from hello external load balancer.
        {
            "name": "AppPortProbe1",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('loadBalancedAppPort1')]",
                "protocol": "tcp"
            }
        } */

    ],
    "inboundNatPools": [
    ```

5. Přidejte druhý `Microsoft.Network/loadBalancers` prostředků. Vypadá to, podobně jako toohello interní nástroj pro vyrovnávání zatížení vytvořené v hello [nástroj pro vyrovnávání zatížení pouze pro interní](#internallb) části, ale používá hello "-Int" zatížení vyrovnávání proměnné a implementuje jenom hello aplikace port 80. Tím se taky odeberou `inboundNatPools`, tookeep koncových bodů protokolu RDP na Vyrovnávání zatížení veřejnou hello. Pokud chcete na Vyrovnávání zatížení interní hello RDP, přesuňte `inboundNatPools` z toothis nástroje pro vyrovnávání zatížení externí hello interní nástroj pro vyrovnávání zatížení:

    ```
            /* Add a second load balancer, configured with a static privateIPAddress and hello "-Int" load balancer variables. */
            {
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                /* Add "-Internal" toohello name. */
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal')]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /* Remove public IP dependsOn, add vnet dependsOn
                    "[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"
                    */
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
                "properties": {
                    "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /* Switch from Public tooPrivate IP address
                                */
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                }
                                */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
                    "backendAddressPools": [
                        {
                            "name": "LoadBalancerBEAddressPool",
                            "properties": {}
                        }
                    ],
                    "loadBalancingRules": [
                        /* Add hello AppPort rule. Be sure tooreference hello "-Int" versions of backendAddressPool, frontendIPConfiguration, and hello probe variables. */
                        {
                            "name": "AppPortLBRule1",
                            "properties": {
                                "backendAddressPool": {
                                    "id": "[variables('lbPoolID0-Int')]"
                                },
                                "backendPort": "[parameters('loadBalancedAppPort1')]",
                                "enableFloatingIP": "false",
                                "frontendIPConfiguration": {
                                    "id": "[variables('lbIPConfig0-Int')]"
                                },
                                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                                "idleTimeoutInMinutes": "5",
                                "probe": {
                                    "id": "[concat(variables('lbID0-Int'),'/probes/AppPortProbe1')]"
                                },
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "probes": [
                    /* Add hello probe for hello app port. */
                    {
                            "name": "AppPortProbe1",
                            "properties": {
                                "intervalInSeconds": 5,
                                "numberOfProbes": 2,
                                "port": "[parameters('loadBalancedAppPort1')]",
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "inboundNatPools": [
                    ]
                },
                "tags": {
                    "resourceType": "Service Fabric",
                    "clusterName": "[parameters('clusterName')]"
                }
            },
    ```

6. V `networkProfile` pro hello `Microsoft.Compute/virtualMachineScaleSets` prostředků, přidat fond adres interní back-end hello:

    ```
    "loadBalancerBackendAddressPools": [
                                                        {
                                                            "id": "[variables('lbPoolID0')]"
                                                        },
                                                        {
                                                            /* Add internal BE pool */
                                                            "id": "[variables('lbPoolID0-Int')]"
                                                        }
    ],
    ```

7. Nasazení šablony hello:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternalexternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternalexternallb -TemplateFile C:\SFSamples\Final\template\_internalexternalLB.json
    ```

Po nasazení se zobrazí dvě služby Vyrovnávání zatížení ve skupině prostředků hello. Pokud hello nástroje pro vyrovnávání zatížení, uvidíte hello veřejnou IP adresu a správa koncových bodů (porty 19000 a 19080) přiřazen toohello veřejnou IP adresu. Taky uvidíte hello statické interní IP adresu a aplikace koncového bodu (port 80) přiřazen toohello interní nástroj pro vyrovnávání zatížení. Současně pomocí nástroje pro vyrovnávání zátěže hello stejné škálovací sady virtuálních počítačů fond back-end.

## <a name="next-steps"></a>Další kroky
[Vytvoření clusteru](service-fabric-cluster-creation-via-arm.md)
