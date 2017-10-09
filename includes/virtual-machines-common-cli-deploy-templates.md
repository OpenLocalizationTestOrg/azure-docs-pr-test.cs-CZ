
* [Rychlé vytvoření virtuálního počítače v Azure](#quick-create-a-vm-in-azure)
* [Nasazení virtuálního počítače v Azure ze šablony](#deploy-a-vm-in-azure-from-a-template)
* [Vytvoření virtuálního počítače z vlastní image](#create-a-custom-vm-image)
* [Nasazení virtuálního počítače, který používá virtuální síť a nástroj pro vyrovnávání zatížení](#deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer)
* [Odebrání skupiny prostředků](#remove-a-resource-group)
* [Zobrazit hello protokolu pro nasazení skupiny prostředků](#show-the-log-for-a-resource-group-deployment)
* [Zobrazení informací o virtuálním počítači](#display-information-about-a-virtual-machine)
* [Připojit virtuální počítač založený na Linuxu tooa](#log-on-to-a-linux-based-virtual-machine)
* [Zastavení virtuálního počítače](#stop-a-virtual-machine)
* [Spuštění virtuálního počítače](#start-a-virtual-machine)
* [Připojení datového disku](#attach-a-data-disk)

## <a name="getting-ready"></a>Příprava
Před použitím hello rozhraní příkazového řádku Azure s skupin prostředků Azure, budete potřebovat toohave hello správnou verzi rozhraní příkazového řádku Azure a účet Azure. Pokud nemáte hello rozhraní příkazového řádku Azure, [ji nainstalovat](../articles/cli-install-nodejs.md).

### <a name="update-your-azure-cli-version-too090-or-later"></a>Aktualizace vašeho too0.9.0 verze rozhraní příkazového řádku Azure nebo novější
Typ `azure --version` toosee, zda jste již nainstalovali verzi 0.9.0 nebo novější.

```azurecli
azure --version
0.9.0 (node: 0.10.25)
```

Pokud vaše verze není 0.9.0 nebo novější, je nutné tooupdate ji pomocí jedné z hello nativní instalační programy nebo pomocí **npm** zadáním `npm update -g azure-cli`.

Rozhraní příkazového řádku Azure můžete také spustit jako kontejner Docker pomocí následující hello [Docker image](https://registry.hub.docker.com/u/microsoft/azure-cli/). Z hostitele Docker spusťte následující příkaz hello:

```bash
docker run -it microsoft/azure-cli
```

### <a name="set-your-azure-account-and-subscription"></a>Nastavení předplatného a účtu Azure
Pokud ještě nemáte předplatné Azure, ale máte předplatné MSDN, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Můžete si zaregistrovat i [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

Nyní [interaktivně přihlásit tooyour účet Azure](../articles/xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login) zadáním `azure login` a následující hello vyzve k tooyour prostředí interaktivní přihlašovací účet Azure. 

> [!NOTE]
> Pokud máte pracovní nebo školní ID, a znát nemáte povoleno dvoufaktorové ověřování, můžete **také** použít `azure login -u` společně s hello pracovní nebo školní ID toolog v *bez* interaktivní relace. Pokud nemáte pracovní nebo školní ID, můžete [vytvořit pracovní nebo školní id z vašeho osobního účtu Microsoft](../articles/virtual-machines/windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toolog v hello stejným způsobem.
>
>

Váš účet může mít víc než jedno předplatné. Seznam předplatných můžete zobrazit zadáním `azure account list`. Výsledek bude vypadat přibližně takto:

```azurecli
azure account list
info:    Executing command account list
data:    Name                              Id                                    Tenant Id                            Current
data:    --------------------------------  ------------------------------------  ------------------------------------  -------
data:    Contoso Admin                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  true
data:    Fabrikam dev                      xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Fabrikam test                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Contoso production                xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
```

Hello aktuální předplatné můžete nastavit tak, že zadáte následující hello. Použijte hello název nebo hello ID předplatného obsahující hello prostředky, které chcete toomanage.

```azurecli
azure account set <subscription name or ID> true
```

### <a name="switch-toohello-azure-cli-resource-group-mode"></a>Přepnout režim skupiny prostředků toohello rozhraní příkazového řádku Azure
Ve výchozím nastavení spustí hello rozhraní příkazového řádku Azure v režimu správy služby hello (**asm** režim). Zadejte hello následující tooswitch tooresource skupiny režimu.

```azurecli
azure config mode arm
```

## <a name="understanding-azure-resource-templates-and-resource-groups"></a>Principy skupin prostředků a šablon prostředků Azure
Většina aplikací je vytvořená pomocí kombinace různých typů prostředků (jako je jeden nebo několik virtuálních počítačů a účtů úložiště, databáze SQL, virtuální síť nebo síť pro doručování obsahu). Hello výchozí Azure service management API a hello portál Azure classic reprezentována tyto položky přístup pomocí služby. Tento postup vyžaduje, abyste toodeploy a spravovat jednotlivé služby hello jednotlivě (nebo najít jiné nástroje, které tak) a ne jako jednu logickou jednotku nasazení.

*Šablony Azure Resource Manageru*, ale, díky kterému budete toodeploy a spravovat tyto různé prostředky jako jednu jednotku logické nasazení deklarativní způsobem. Místo imperativní informuje Azure co toodeploy jednoho příkazu za druhým, popisují celého nasazení v souboru JSON – všechny prostředky hello a přidružené konfigurace a nasazení parametry – a řekněte Azure toodeploy tyto prostředky jako jeden Skupina.

Pak můžete spravovat hello celkový životní cyklus hello skupiny prostředků pomocí příkazů správu prostředků Azure CLI:

* Zastavení, spuštění a odstranit všechny prostředky hello v rámci skupiny hello současně.
* Použití řízení přístupu na základě rolí (RBAC) pravidla toolock dolů oprávnění zabezpečení na ně.
* Auditování operací.
* Označení prostředků dalšími metadaty pro lepší sledování.

Další informace o skupin prostředků Azure a co dělají za vás v hello mnoha [přehled Azure Resource Manageru](../articles/azure-resource-manager/resource-group-overview.md). Pokud vás zajímá vytváření šablon, přečtěte si téma věnované [vytváření šablon Azure Resource Manageru](../articles/resource-group-authoring-templates.md).

## <a id="quick-create-a-vm-in-azure"></a>Úkol: Rychlé vytvoření virtuálního počítače v Azure
Někdy víte, jaké image, budete potřebovat, a teď potřebujete virtuálního počítače z této bitové kopie a vám nezáleží příliš mnoho hello infrastruktury – možná máte tootest něco čistou virtuálního počítače. Pokud je chcete toouse hello `azure vm quick-create` příkazů a předat nezbytné toocreate argumenty hello virtuálního počítače a jeho infrastruktury.

Nejdřív vytvoříte skupinu prostředků.

```azurecli
azure group create coreos-quick westus
info:    Executing command group create
+ Getting resource group coreos-quick
+ Creating resource group coreos-quick
info:    Created resource group coreos-quick
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick
data:    Name:                coreos-quick
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Potom budete potřebovat image. toofind na bitovou kopii s hello Azure CLI, najdete v části [navigace a výběr imagí virtuálních počítačů Azure pomocí prostředí PowerShell a rozhraní příkazového řádku Azure hello](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ale pro účely tohoto článku použijeme krátký seznam oblíbených imagí. Pro toto rychlé vytvoření použijeme image Stable CoreOS.

> [!NOTE]
> Pro ComputeImageVersion můžete taky jednoduše zadat 'nejnovější' jako hello parametr v obou jazyk hello šablony a hello rozhraní příkazového řádku Azure. To vám umožní, že tooalways používáte hello nejnovější a opravou verze hello bitové kopie bez nutnosti toomodify skripty nebo šablon. Příklad najdete níž.
>
>

| Název vydavatele | Nabídka | Skladová jednotka (SKU) | Verze |
|:--- |:--- |:--- |:--- |
| OpenLogic |CentOS |7 |7.0.201503 |
| OpenLogic |CentOS |7.1 |7.1.201504 |
| CoreOS |CoreOS |Beta |647.0.0 |
| CoreOS |CoreOS |Stable |633.1.0 |
| MicrosoftDynamicsNAV |DynamicsNAV |2015 |8.0.40459 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2013 |1.0.0 |
| msopentech |Oracle-Database-12c-Weblogic-Server-12c |Standard |1.0.0 |
| msopentech |Oracle-Database-12c-Weblogic-Server-12c |Enterprise |1.0.0 |
| MicrosoftSQLServer |SQL2014-WS2012R2 |Enterprise-Optimized-for-DW |12.0.2430 |
| MicrosoftSQLServer |SQL2014-WS2012R2 |Enterprise-Optimized-for-OLTP |12.0.2430 |
| Canonical |UbuntuServer |12.04.5-LTS |12.04.201504230 |
| Canonical |UbuntuServer |14.04.2-LTS |14.04.201503090 |
| MicrosoftWindowsServer |WindowsServer |2012-Datacenter |3.0.201503 |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |4.0.201503 |
| MicrosoftWindowsServer |WindowsServer |Windows-Server-Technical-Preview |5.0.201504 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |1.0.141204 |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |4.3.4665 |

Právě vytvoření virtuálního počítače tak, že zadáte hello `azure vm quick-create` příkaz a je připravený pro hello výzvy. Výsledek by měl vypadat přibližně takto:

```azurecli
azure vm quick-create
info:    Executing command vm quick-create
Resource group name: coreos-quick
Virtual machine name: coreos
Location name: westus
Operating system Type [Windows, Linux]: linux
ImageURN (format: "publisherName:offer:skus:version"): coreos:coreos:stable:latest
User name: ops
Password: *********
Confirm password: *********
+ Looking up hello VM "coreos"
info:    Using hello VM Size "Standard_A1"
info:    hello [OS, Data] Disk or image configuration requires storage account
+ Retrieving storage accounts
info:    Could not find any storage accounts in hello region "westus", trying toocreate new one
+ Creating storage account "cli9fd3fce49e9a9b3d14302" in "westus"
+ Looking up hello storage account cli9fd3fce49e9a9b3d14302
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
info:    An nic with given name "coreo-westu-1430261891570-nic" not found, creating a new one
+ Looking up hello virtual network "coreo-westu-1430261891570-vnet"
info:    Preparing toocreate new virtual network and subnet
/ Creating a new virtual network "coreo-westu-1430261891570-vnet" [address prefix: "10.0.0.0/16"] with subnet "coreo-westu-1430261891570-sne+" [address prefix: "10.0.1.0/24"]
+ Looking up hello virtual network "coreo-westu-1430261891570-vnet"
+ Looking up hello subnet "coreo-westu-1430261891570-snet" under hello virtual network "coreo-westu-1430261891570-vnet"
info:    Found public ip parameters, trying toosetup PublicIP profile
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
info:    PublicIP with given name "coreo-westu-1430261891570-pip" not found, creating a new one
+ Creating public ip "coreo-westu-1430261891570-pip"
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
+ Creating NIC "coreo-westu-1430261891570-nic"
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
+ Creating VM "coreos"
+ Looking up hello VM "coreos"
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Compute/virtualMachines/coreos
data:    ProvisioningState               :Succeeded
data:    Name                            :coreos
data:    Location                        :westus
data:    FQDN                            :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :coreos
data:        Offer                       :coreos
data:        Sku                         :stable
data:        Version                     :633.1.0
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :cli9fd3fce49e9a9b3d-os-1430261892283
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli9fd3fce49e9a9b3d14302.blob.core.windows.net/vhds/cli9fd3fce49e9a9b3d-os-1430261892283.vhd
data:
data:    OS Profile:
data:      Computer Name                 :coreos
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :false
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Network/networkInterfaces/coreo-westu-1430261891570-nic
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-30-72-E3
data:          Provisioning State        :Succeeded
data:          Name                      :coreo-westu-1430261891570-nic
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.1.4
data:            Public IP address       :104.40.24.124
data:            FQDN                    :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
info:    vm quick-create command OK
```

A váš nový virtuální počítač je připravený.

## <a id="deploy-a-vm-in-azure-from-a-template"></a>Úkol: Nasazení virtuálního počítače v Azure ze šablony
Použijte hello pokyny v těchto částech toodeploy nového virtuálního počítače Azure pomocí šablony s hello rozhraní příkazového řádku Azure. Tato šablona vytvoří jeden virtuální počítač v nové virtuální sítě s jedinou podsítí a na rozdíl od `azure vm quick-create`, umožňuje toodescribe jste, co chcete přesněji a opakujte bez chyb. Tato šablona vytvoří tohle:

![](./media/virtual-machines-common-cli-deploy-templates/new-vm.png)

### <a name="step-1-examine-hello-json-file-for-hello-template-parameters"></a>Krok 1: Zkontrolujte hello JSON v souboru parametrů šablony hello
Tady jsou hello obsah souboru JSON hello hello šablony. (hello šablona se také nachází v [Githubu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json).)

Šablony jsou flexibilní, proto hello Návrhář může mít vybrali toogive velké množství parametry nebo vybrali toooffer jen několik tak, že vytvoříte šablonu, která více vyřešen. V pořadí toocollect hello informace, které potřebujete toopass hello šablony jako parametry, otevřete soubor šablony hello (Toto téma obsahuje vložené šablony níže) a zkontrolujte hello **parametry** hodnoty.

V takovém případě níže uvedená šablona hello požádá pro:

* Jedinečný název účtu úložiště.
* Uživatelské jméno hello virtuálních počítačů správce.
* Heslo.
* Název domény pro hello mimo world toouse.
* Číslo verze Ubuntu Serveru (ale přijme jenom jednu z hodnot v seznamu).

Přečtěte si další informace o [požadavcích na uživatelské jméno a heslo](../articles/virtual-machines/linux/faq.md#what-are-the-username-requirements-when-creating-a-vm).

Jakmile se rozhodnete tyto hodnoty jste skupinu pro připravený toocreate a nasazení této šablony do vašeho předplatného Azure.

```json
{
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": {
    "newStorageAccountName": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for hello storage account where hello virtual machine's disks will be placed."
    }
    },
    "adminUsername": {
    "type": "string",
    "metadata": {
        "description": "User name for hello virtual machine."
    }
    },
    "adminPassword": {
    "type": "securestring",
    "metadata": {
        "description": "Password for hello virtual machine."
    }
    },
    "dnsNameForPublicIP": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for hello public IP used tooaccess hello virtual machine."
    }
    },
    "ubuntuOSVersion": {
    "type": "string",
    "defaultValue": "14.04.2-LTS",
    "allowedValues": [
        "12.04.5-LTS",
        "14.04.2-LTS",
        "15.04"
    ],
    "metadata": {
        "description": "hello Ubuntu version for hello VM. This will pick a fully patched image of this given Ubuntu version. Allowed values: 12.04.5-LTS, 14.04.2-LTS, 15.04."
    }
    }
},
"variables": {
    "location": "West US",
    "imagePublisher": "Canonical",
    "imageOffer": "UbuntuServer",
    "OSDiskName": "osdiskforlinuxsimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyUbuntuVM",
    "vmSize": "Standard_D1",
    "virtualNetworkName": "MyVNET",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
},
"resources": [
    {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('newStorageAccountName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[variables('location')]",
    "properties": {
        "accountType": "[variables('storageAccountType')]"
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[variables('publicIPAddressName')]",
    "location": "[variables('location')]",
    "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
        "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
        }
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
    "location": "[variables('location')]",
    "properties": {
        "addressSpace": {
        "addressPrefixes": [
            "[variables('addressPrefix')]"
        ]
        },
        "subnets": [
        {
            "name": "[variables('subnetName')]",
            "properties": {
            "addressPrefix": "[variables('subnetPrefix')]"
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('nicName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
    ],
    "properties": {
        "ipConfigurations": [
        {
            "name": "ipconfig1",
            "properties": {
            "privateIPAllocationMethod": "Dynamic",
            "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
            },
            "subnet": {
                "id": "[variables('subnetRef')]"
            }
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', parameters('newStorageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
        "computername": "[variables('vmName')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
        "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('ubuntuOSVersion')]",
            "version": "latest"
        },
        "osDisk": {
            "name": "osdisk",
            "vhd": {
            "uri": "[concat('http://',parameters('newStorageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
        }
        },
        "networkProfile": {
        "networkInterfaces": [
            {
            "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
        ]
        }
    }
    }
]
}
```

### <a name="step-2-create-hello-virtual-machine-by-using-hello-template"></a>Krok 2: Vytvoření hello virtuálního počítače pomocí šablony hello
Až budete mít vaše hodnoty parametrů připraven, musíte vytvořit skupinu prostředků pro šablonu nasazení a potom nasaďte šablonu hello.

Skupina prostředků toocreate hello, typ `azure group create <group name> <location>` s názvem hello hello skupiny chcete a hello datacenter umístění, do kterého chcete toodeploy. Akce proběhne rychle:

```azurecli
azure group create myResourceGroup westus
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Nyní toocreate hello nasazení, volání `azure group deployment create` a předejte:

* soubor šablony Hello (Pokud jste uložili hello výše JSON šablony tooa místního souboru).
* Šablona identifikátor URI (v případě potřeby toopoint v souboru hello v Githubu nebo některých jiných webovou adresu).
* Skupina prostředků Hello, do kterého chcete toodeploy.
* Volitelný název nasazení.

Bude výzvami toosupply hello hodnoty parametrů v části "parametry" hello souboru JSON hello. Pokud jste zadali hodnoty parametrů všechny hello, bude zahájena vaše nasazení.

Zde naleznete příklad:

```azurecli
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json myResourceGroup firstDeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
newStorageAccountName: storageaccount
adminUsername: ops
adminPassword: password
dnsNameForPublicIP: newdomainname
```

Zobrazí hello následující typy informací:

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "firstDeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment toocomplete
data:    DeploymentName     : firstDeployment
data:    ResourceGroupName  : myResourceGroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T07:53:55.1828878Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  -------------
data:    newStorageAccountName  String        storageaccount
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameForPublicIP     String        newdomainname
data:    ubuntuOSVersion        String        14.10
info:    group deployment create command OK
```


## <a id="create-a-custom-vm-image"></a>Úkol: Vytvoření vlastní image virtuálního počítače
Jste se seznámili s hello základní použití šablony výše, takže teď můžeme použít podobné pokyny toocreate vlastní virtuální počítač ze souboru konkrétní VHD v Azure pomocí šablony prostřednictvím hello rozhraní příkazového řádku Azure. Hello rozdíl je, že tato šablona vytvoří jeden virtuální počítač z zadaný virtuální pevný disk (VHD).

### <a name="step-1-examine-hello-json-file-for-hello-template"></a>Krok 1: Zkontrolujte hello JSON v souboru šablony hello
Tady jsou hello obsah souboru JSON hello hello šablony, která v této části se používá jako příklad. (hello šablona se také nachází v [Githubu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).)

Znovu budete potřebovat toofind hello hodnoty, které mají tooenter pro hello parametry, které nemají výchozí hodnoty. Když spustíte hello `azure group deployment create` příkaz hello příkazového řádku Azure CLI vás vyzve tooenter můžete tyto hodnoty.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters" : {
        "userImageStorageAccountName": {
            "type": "string",
            "defaultValue" : "userImageStorageAccountName"
        },
        "userImageStorageContainerName" : {
            "type" : "string",
            "defaultValue" : "userImageStorageContainerName"
        },
        "userImageVhdName" : {
            "type" : "string",
            "defaultValue" : "userImageVhdName"
        },
        "dnsNameForPublicIP" : {
            "type" : "string",
            "defaultValue": "uniqueDnsNameForPublicIP"
        },
        "adminUserName": {
            "type": "string"
        },
        "adminPassword": {
            "type": "securestring"
        },
        "osType" : {
            "type" : "string",
            "allowedValues" : [
                "windows",
                "linux"
            ]
        },
        "subscriptionId":{
            "type" : "string"
        },
        "location": {
            "type": "String",
            "defaultValue" : "West US"
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A2"
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue" : "myPublicIP"
        },
        "vmName": {
            "type": "string",
            "defaultValue" : "myVM"
        },
        "virtualNetworkName":{
            "type" : "string",
            "defaultValue" : "myVNET"
        },
        "nicName":{
            "type" : "string",
            "defaultValue":"myNIC"
        }
    },
    "variables": {
        "addressPrefix":"10.0.0.0/16",
        "subnet1Name": "Subnet-1",
        "subnet2Name": "Subnet-2",
        "subnet1Prefix" : "10.0.0.0/24",
        "subnet2Prefix" : "10.0.1.0/24",
        "publicIPAddressType" : "Dynamic",
        "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
        "subnet1Ref" : "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
        "userImageName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('userImageVhdName'))]",
        "osDiskVhdName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
    },
    "resources": [
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[parameters('publicIPAddressName')]",
        "location": "[parameters('location')]",
        "properties": {
            "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
            }
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/virtualNetworks",
        "name": "[parameters('virtualNetworkName')]",
        "location": "[parameters('location')]",
        "properties": {
        "addressSpace": {
            "addressPrefixes": [
            "[variables('addressPrefix')]"
            ]
        },
        "subnets": [
            {
            "name": "[variables('subnet1Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet1Prefix')]"
            }
            },
            {
            "name": "[variables('subnet2Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet2Prefix')]"
            }
            }
        ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[parameters('nicName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
            "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
        ],
        "properties": {
            "ipConfigurations": [
            {
                "name": "ipconfig1",
                "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                        "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                    },
                    "subnet": {
                        "id": "[variables('subnet1Ref')]"
                    }
                }
            }
            ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[parameters('vmName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
        ],
        "properties": {
            "hardwareProfile": {
                "vmSize": "[parameters('vmSize')]"
            },
            "osProfile": {
                "computername": "[parameters('vmName')]",
                "adminUsername": "[parameters('adminUsername')]",
                "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
                "osDisk" : {
                    "name" : "[concat(parameters('vmName'),'-osDisk')]",
                    "osType" : "[parameters('osType')]",
                    "caching" : "ReadWrite",
                    "image" : {
                        "uri" : "[variables('userImageName')]"
                    },
                    "vhd" : {
                        "uri" : "[variables('osDiskVhdName')]"
                    }
                }
            },
            "networkProfile": {
                "networkInterfaces" : [
                {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                }
                ]
            }
        }
    }
    ]
}
```

### <a name="step-2-obtain-hello-vhd"></a>Krok 2: Získání hello virtuálního pevného disku
Pro tyto účely budete samozřejmě potřebovat soubor .vhd. Můžete použít ten, který už máte v Azure, nebo nějaký nahrát.

Virtuální počítač systému Windows, najdete v části [vytvoření a nahrání virtuálního pevného disku serveru Windows tooAzure](../articles/virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Pro virtuální počítač založený na Linuxu, viz [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux hello](../articles/virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="step-3-create-hello-virtual-machine-by-using-hello-template"></a>Krok 3: Vytvoření hello virtuálního počítače pomocí šablony hello
Nyní jste připravené toocreate nového virtuálního počítače podle hello VHD. Vytvoření skupiny toodeploy do, pomocí `azure group create <location>`:

```azurecli
azure group create myResourceGroupUser eastus
info:    Executing command group create
+ Getting resource group myResourceGroupUser
+ Creating resource group myResourceGroupUser
info:    Created resource group myResourceGroupUser
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupUser
data:    Name:                myResourceGroupUser
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Pak vytvořte hello nasazení pomocí hello `--template-uri` toocall možnost v šabloně hello přímo (nebo můžete použít hello `--template-file` toouse možnost soubor, který jste uložili místně). Všimněte si, že protože hello šablony má výchozí hodnoty zadané, budete vyzváni k jenom pár věcí. Pokud nasadíte hello šablony na různých místech, můžete zjistit, že některé pojmenování kolizí ke kterým dochází u hello výchozí hodnoty (zejména hello název DNS, které vytvoříte).

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json \
> myResourceGroup \
> customVhdDeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
adminUserName: ops
adminPassword: password
osType: linux
subscriptionId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

Výstup vypadá podobně jako následující hello:

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "customVhdDeployment"
+ Registering providers
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment toocomplete
error:   Deployment provisioning state was not successful
data:    DeploymentName     : customVhdDeployment
data:    ResourceGroupName  : myResourceGroupUser
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T14:55:48.0963829Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                           Type          Value
data:    -----------------------------  ------------  ------------------------------------
data:    userImageStorageAccountName    String        userImageStorageAccountName
data:    userImageStorageContainerName  String        userImageStorageContainerName
data:    userImageVhdName               String        userImageVhdName
data:    dnsNameForPublicIP             String        uniqueDnsNameForPublicIP
data:    adminUserName                  String        ops
data:    adminPassword                  SecureString  undefined
data:    osType                         String        linux
data:    subscriptionId                 String        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
data:    location                       String        West US
data:    vmSize                         String        Standard_A2
data:    publicIPAddressName            String        myPublicIP
data:    vmName                         String        myVM
data:    virtualNetworkName             String        myVNET
data:    nicName                        String        myNIC
info:    group deployment create command OK
```

## <a id="deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a>Úloha: Nasazení aplikace s více virtuálními počítači, která používá virtuální síť a externí nástroj pro vyrovnávání zatížení
Tato šablona vám umožní toocreate dva virtuální počítače pod nástrojem pro vyrovnávání zatížení a nakonfigurujte pravidlo Vyrovnávání zatížení na Port 80. Tato šablona také nasadí účet úložiště, virtuální síť, veřejnou IP adresu, skupinu dostupnosti a síťová rozhraní.

![](./media/virtual-machines-common-cli-deploy-templates/multivmextlb.png)

Postupujte podle těchto kroků toodeploy aplikace více virtuálních počítačů, která používá virtuální síť a nástroj pro vyrovnávání zatížení pomocí šablony Resource Manageru v šabloně úložiště GitHub hello pomocí příkazů prostředí Azure PowerShell.

### <a name="step-1-examine-hello-json-file-for-hello-template"></a>Krok 1: Zkontrolujte hello JSON v souboru šablony hello
Tady jsou hello obsah souboru JSON hello hello šablony. Pokud chcete, aby hello nejnovější verzi, ho má nachází [v úložišti GitHub hello pro šablony](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json). Toto téma používá hello `--template-uri` toocall přepínače v hello šablony, ale můžete také použít hello `--template-file` přepínač toopass místní verze.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location of resources"
            }
        },
        "storageAccountName": {
            "type": "string",
            "metadata": {
                "description": "Name of storage account"
            }
        },
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "Admin user name"
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Admin password"
            }
        },
        "dnsNameforLBIP": {
            "type": "string",
            "metadata": {
                "description": "DNS for load balancer IP"
            }
        },
        "backendPort": {
            "type": "int",
            "defaultValue": 3389,
            "metadata": {
                "description": "Back-end port"
            }
        },
        "vmNamePrefix": {
            "type": "string",
            "defaultValue": "myVM",
            "metadata": {
                "description": "Prefix toouse for VM names"
            }
        },
        "vmSourceImageName": {
            "type": "string",
            "defaultValue": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201412.01-en.us-127GB.vhd"
        },
        "lbName": {
            "type": "string",
            "defaultValue": "myLB",
            "metadata": {
                "description": "Load balancer name"
            }
        },
        "nicNamePrefix": {
            "type": "string",
            "defaultValue": "nic",
            "metadata": {
                "description": "Network interface name prefix"
            }
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue": "myPublicIP",
            "metadata": {
                "description": "Public IP name"
            }
        },
        "vnetName": {
            "type": "string",
            "defaultValue": "myVNET",
            "metadata": {
                "description": "Virtual network name"
            }
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A1",
            "metadata": {
                "description": "Size of hello VM"
            }
        }
    },
    "variables": {
        "storageAccountType": "Standard_LRS",
        "vmStorageAccountContainerName": "vhds",
        "availabilitySetName": "myAvSet",
        "addressPrefix": "10.0.0.0/16",
        "subnetName": "Subnet-1",
        "subnetPrefix": "10.0.0.0/24",
        "publicIPAddressType": "Dynamic",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
        "numberOfInstances": 2,
        "nicId1": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 0))]",
        "nicId2": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 1))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LBFE')]",
        "backEndIPConfigID1": "[concat(variables('nicId1'),'/ipConfigurations/ipconfig1')]",
        "backEndIPConfigID2": "[concat(variables('nicId2'),'/ipConfigurations/ipconfig1')]",
        "sourceImageName": "[concat('/', subscription().subscriptionId,'/services/images/',parameters('vmSourceImageName'))]",
        "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/LBBE')]",
        "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccountName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": { }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddressName')]",
            "location": "[parameters('location')]",
            "properties": {
                "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                "dnsSettings": {
                    "domainNameLabel": "[parameters('dnsNameforLBIP')]"
                }
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('vnetName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('addressPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('subnetName')]",
                        "properties": {
                            "addressPrefix": "[variables('subnetPrefix')]"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/networkInterfaces",
            "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
            "location": "[parameters('location')]",
            "copy": {
                "name": "nicLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
            ],
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "subnet": {
                                "id": "[variables('subnetRef')]"
                            }
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/backendAddressPools/LBBE')]"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/inboundNatRule/RDP-VM', copyindex())]"
                            }
                        ]


                    }
                ]

            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "name": "[parameters('lbName')]",
            "type": "Microsoft.Network/loadBalancers",
            "location": "[parameters('location')]",
            "dependsOn": [
                "nicLoop",
                "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
            ],
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "LBFE",
                        "properties": {
                            "publicIPAddress": {
                                "id": "[variables('publicIPAddressID')]"
                            }
                        }
                    }
                ],
                "backendAddressPools": [
                    {
                        "name": "LBBE"

                    }
                ],
                "inboundNatRules": [
                    {
                        "name": "RDP-VM1",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50001,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    },
                    {
                        "name": "RDP-VM2",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50002,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "LBRule",
                        "properties": {
                            "frontendIPConfiguration": {
                                "id": "[variables('frontEndIPConfigID')]"
                            },
                            "backendAddressPool": {
                                "id": "[variables('lbPoolID')]"
                            },
                            "protocol": "tcp",
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 5,
                            "probe": {
                                "id": "[variables('lbProbeID')]"
                            }
                        }
                    }
                ],
                "probes": [
                    {
                        "name": "tcpProbe",
                        "properties": {
                            "protocol": "tcp",
                            "port": 80,
                            "intervalInSeconds": "5",
                            "numberOfProbes": "2"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
            "copy": {
                "name": "virtualMachineLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
                "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
                "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
            ],
            "properties": {
                "availabilitySet": {
                    "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
                },
                "hardwareProfile": {
                    "vmSize": "[parameters('vmSize')]"
                },
                "osProfile": {
                    "computername": "[concat(parameters('vmNamePrefix'), copyIndex())]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                    "sourceImage": {
                        "id": "[variables('sourceImageName')]"
                    },
                    "destinationVhdsContainer": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/')]"
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
                        }
                    ]
                }
            }
        }
    ]
}
```

### <a name="step-2-create-hello-deployment-by-using-hello-template"></a>Krok 2: Vytvoření hello nasazení pomocí šablony hello
Vytvořte skupinu prostředků pro šablonu hello pomocí `azure group create <location>`. Pak vytvořte nasazení do této skupiny prostředků pomocí `azure group deployment create` a předání hello skupinu prostředků, předávání název nasazení a volaného hello výzvy pro parametry v hello šablonu, která nemá výchozí hodnoty.

```azurecli
azure group create lbgroup westus
info:    Executing command group create
+ Getting resource group lbgroup
+ Creating resource group lbgroup
info:    Created resource group lbgroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/lbgroup
data:    Name:                lbgroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Teď použít hello `azure group deployment create` příkaz a hello `--template-uri` možnost toodeploy hello šablony. Připravte si hodnoty parametrů a po zobrazení příslušných výzev je zadejte, jak znázorňuje následující obrázek.

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json \
> lbgroup \
> newdeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
location: westus
newStorageAccountName: storagename
adminUsername: ops
adminPassword: password
dnsNameforLBIP: lbdomainname
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "newdeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.compute
info:    Registering provider microsoft.network
+ Waiting for deployment toocomplete
data:    DeploymentName     : newdeployment
data:    ResourceGroupName  : lbgroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T20:58:40.1678876Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  ----------------------
data:    location               String        westus
data:    newStorageAccountName  String        storagename
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameforLBIP         String        lbdomainname
data:    backendPort            Int           3389
data:    vmNamePrefix           String        myVM
data:    imagePublisher         String        MicrosoftWindowsServer
data:    imageOffer             String        WindowsServer
data:    imageSKU               String        2012-R2-Datacenter
data:    lbName                 String        myLB
data:    nicNamePrefix          String        nic
data:    publicIPAddressName    String        myPublicIP
data:    vnetName               String        myVNET
data:    vmSize                 String        Standard_A1
info:    group deployment create command OK
```

Všimněte si, že tato šablona nasadí image Windows Serveru. Můžete ji ale snadno nahradit libovolnou linuxovou imagí. Chcete toocreate Docker clusteru s několika správci swarm? [Můžete](https://azure.microsoft.com/documentation/templates/docker-swarm-cluster/).

## <a id="remove-a-resource-group"></a>Úkol: Odebrání skupiny prostředků
Mějte na paměti, že můžete znovu nasadit tooa skupinu prostředků, ale pokud jste hotovi s jedním, můžete ho odstranit pomocí `azure group delete <group name>`.

```azurecli
azure group delete myResourceGroup
info:    Executing command group delete
Delete resource group myResourceGroup? [y/n] y
+ Deleting resource group myResourceGroup
info:    group delete command OK
```

## <a id="show-the-log-for-a-resource-group-deployment"></a>Úloha: Zobrazit hello protokolu pro nasazení skupiny prostředků
Tento úkol je obvyklý při vytváření nebo používání šablon. protokoly Hello volání toodisplay hello nasazení pro skupinu je `azure group log show <groupname>`, který zobrazuje s bit informace, které jsou užitečné při hledání něco stalo--nebo nebyla. (Další informace o řešení potíží s nasazeními a také další informace o problémech najdete v tématu [Řešení chyb nasazení v Azure pomocí Azure Resource Manageru](../articles/azure-resource-manager/resource-manager-common-deployment-errors.md).)

tootarget specifických chybách, například můžete použít nástroje, například **jq** tooquery věcí, které jsou o něco víc, přesněji, například které jednotlivé chyby, je nutné toocorrect. Hello následující příklad používá **jq** tooparse nasazení protokolu **lbgroup**, kteří hledají selhání.

```azurecli
azure group log show lbgroup -l --json | jq '.[] | select(.status.value == "Failed") | .properties'
```
Umožňuje rychle zjistit, co se nepovedlo, opravit to a zkusit znovu. V následujícím případě hello, hello šablonu měl byla vytváření dva virtuální počítače v hello současně, který vytvoří zámek na VHD hello. (Po úpravě šablony hello jsme hello nasazení bylo úspěšné rychle.)

```json
{
    "statusCode": "Conflict",
    "statusMessage": "{\"status\":\"Failed\",\"error\":{\"code\":\"ResourceDeploymentFailure\",\"message\":\"hello resource operation completed with terminal provisioning state 'Failed'.\",\"details\":[{\"code\":\"AcquireDiskLeaseFailed\",\"message\":\"Failed tooacquire lease while creating disk 'osdisk' using blob with URI http://storage.blob.core.windows.net/vhds/osdisk.vhd.\"}]}}"
}
```

## <a id="display-information-about-a-virtual-machine"></a>Úkol: Zobrazení informací o virtuálním počítači
Zobrazí se informace o konkrétní virtuální počítače ve vaší skupině prostředků s použitím hello `azure vm show <groupname> <vmname>` příkaz. Pokud máte více než jeden virtuální počítač ve vaší skupině, bude pravděpodobně nutné nejprve toolist hello virtuální počítače ve skupině pomocí `azure vm list <groupname>`.

```azurecli
azure vm list zoo
info:    Executing command vm list
+ Getting virtual machines
data:    Name   ProvisioningState  Location  Size
data:    -----  -----------------  --------  -----------
data:    myVM0  Succeeded          westus    Standard_A1
data:    myVM1  Failed             westus    Standard_A1
```

A potom vyhledat počítač myVM1:

```azurecli
azure vm show zoo myVM1
info:    Executing command vm show
+ Looking up hello VM "myVM1"
+ Looking up hello NIC "nic1"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Failed
data:    Name                            :myVM1
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :MicrosoftWindowsServer
data:        Offer                       :WindowsServer
data:        Sku                         :2012-R2-Datacenter
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Windows
data:        Name                        :osdisk
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :http://zoostorageralph.blob.core.windows.net/vhds/osdisk.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Windows Configuration:
data:        Provision VM Agent          :true
data:        Enable automatic updates    :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Network/networkInterfaces/nic1
data:          Primary                   :false
data:          Provisioning State        :Succeeded
data:          Name                      :nic1
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.0.5
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/availabilitySets/MYAVSET
info:    vm show command OK
```

> [!NOTE]
> Pokud chcete tooprogrammatically úložiště a manipulaci s výstup hello příkazů vaší konzoly, může být vhodné toouse JSON analýza nástroje, jako  **[jq](https://github.com/stedolan/jq)**  nebo  **[jsawk](https://github.com/micha/jsawk)** , nebo knihovny jazyka, které jsou vhodné pro úlohu hello.
>
>

## <a id="log-on-to-a-linux-based-virtual-machine"></a>Úloha: Přihlaste tooa systémem Linux virtuálního počítače
Počítače se systémem Linux jsou obvykle připojené toothrough SSH. Další informace najdete v tématu [jak toouse SSH s Linuxem v Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a id="stop-a-virtual-machine"></a>Úkol: Zastavení virtuálního počítače
Spusťte tento příkaz:

```azurecli
azure vm stop <group name> <virtual machine name>
```

> [!IMPORTANT]
> Použijte tento parametr tookeep hello virtuální IP (VIP) hello sítě vnet, v případě, že je hello poslední virtuální počítač v této virtuální sítě. <br><br> Pokud používáte hello `StayProvisioned` parametr stále platit budete pro hello virtuálních počítačů.
>
>

## <a id="start-a-virtual-machine"></a>Úkol: Spuštění virtuálního počítače
Spusťte tento příkaz:

```azurecli
azure vm start <group name> <virtual machine name>
```

## <a id="attach-a-data-disk"></a>Úkol: Připojení datového disku
Budete také potřebovat toodecide zda tooattach nový disk nebo jeden, který obsahuje data. Pro nový disk, hello příkaz vytvoří soubor VHD hello a připojí jej v hello stejný příkaz.

tooattach nový disk, spusťte tento příkaz:

```azurecli
    azure vm disk attach-new <resource-group> <vm-name> <size-in-gb>
```

tooattach stávající datový disk, spusťte tento příkaz:

```azurecli
azure vm disk attach <resource-group> <vm-name> [vhd-url]
```

Toomount hello disku, pak musíte jako za normálních okolností byste v systému Linux.

## <a name="next-steps"></a>Další kroky
Daleko Další příklady použití Azure CLI s hello **arm** režimu, najdete v části [hello pomocí Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](../articles/xplat-cli-azure-resource-manager.md). toolearn Další informace o prostředků Azure a jejich koncepty, najdete v části [přehled Azure Resource Manageru](../articles/azure-resource-manager/resource-group-overview.md).

Další šablony, které můžete použít, najdete v tématech věnovaných [rychlému úvodu do šablon pro Azure](https://azure.microsoft.com/documentation/templates/) a [aplikačním architekturám využívajícím šablony](../articles/virtual-machines/linux/app-frameworks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
