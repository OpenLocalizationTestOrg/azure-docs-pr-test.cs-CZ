---
title: "aaaBest postupy pro vytváření šablon Resource Manageru | Microsoft Docs"
description: "Pokyny pro zjednodušit vaše šablony Azure Resource Manager."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 31b10deb-0183-47ce-a5ba-6d0ff2ae8ab3
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: tomfitz
ms.openlocfilehash: ec9bbe218c4f2c6a92ca44b5e9c9c71029e22151
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a>Osvědčené postupy pro vytváření šablon Azure Resource Manageru
Tyto pokyny vám pomůžou vytvořit šablony Azure Resource Manager, které jsou toouse spolehlivé a snadné. Hello pokyny jsou pouze návrhy. Nejsou požadavky, pokud není uvedeno jinak. Váš scénář může vyžadovat varianta mezi hello následující přístupy nebo příklady.

## <a name="resource-names"></a>Názvy prostředků
Obecně platí pracujete s tři typy názvy prostředků ve službě Správce prostředků:

* Názvy prostředků, které musí být jedinečný.
* Názvy prostředků, které nejsou nezbytné toobe jedinečné, ale zvolíte tooprovide název, který vám může pomoct identifikovat prostředků na základě kontextu.
* Názvy prostředků, které mohou být obecný.

 Informace o omezení přístupu názvem prostředků najdete v tématu [doporučená zásady vytváření názvů pro prostředky Azure](../guidance/guidance-naming-conventions.md).

### <a name="unique-resource-names"></a>Názvy jedinečný prostředků
Je nutné zadat název jedinečný prostředek pro jakýkoli typ prostředku, který má přístup koncový bod data. Některé běžné typy prostředků, které vyžadují jedinečný název patří:

* Úložiště Azure<sup>1</sup> 
* Funkce Web Apps ve službě Azure App Service
* SQL Server
* Azure Key Vault
* Azure Redis Cache
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Azure HDInsight

<sup>1</sup> názvy účtů úložiště musí být také malými písmeny, 24 znaků nebo méně, a nemusí být všechny pomlčky.

Pokud zadáte parametr pro název prostředku, musí se při nasazení prostředků hello zadejte jedinečný název. Volitelně můžete vytvořit proměnné, která používá hello [uniqueString()](resource-group-template-functions-string.md#uniquestring) toogenerate název funkce. 

Také může má tooadd předponu nebo příponu toohello **uniqueString** výsledek. Změny hello jedinečný název můžete další snadno identifikovat typ prostředku hello z názvu hello. Můžete například vygenerovat jedinečný název pro účet úložiště pomocí hello následující proměnné:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Názvy prostředků pro identifikaci
Některé typy prostředků můžete chtít tooname, avšak jejich názvy nemáte toobe jedinečný. Pro tyto typy prostředků můžete zadat název, který identifikuje prostředek kontextu hello a typ prostředku hello. Zadejte popisný název, který pomáhá identifikovat hello prostředku v seznamu prostředků. Pokud potřebujete toouse název jiného prostředku pro jiné nasazení, můžete použít parametr pro název hello:

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "hello name of hello VM toocreate."
        }
    }
}
```

Pokud nepotřebujete toopass v názvu během nasazení, můžete použít proměnné: 

```json
"variables": {
    "vmName": "demoLinuxVM"
}
```

Můžete taky použít hodnotu pevně:

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "demoLinuxVM",
  ...
}
```

### <a name="generic-resource-names"></a>Obecný zdroj názvy
Pro typy prostředků, které většinou přistupovat prostřednictvím jiného prostředku můžete použít obecný název, který je pevně zakódovaná v šabloně hello. Například můžete nastavit standardní, obecný název pravidla brány firewall na serveru SQL server:

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="parameters"></a>Parametry
Hello následující informace mohou být užitečné při práci s parametry:

* Minimalizujte využití parametrů. Pokud je to možné, použijte proměnnou nebo literálovou hodnotou. Použijte parametry jenom pro tyto scénáře:
   
   * Nastavení, které chcete toouse variace podle tooenvironment (SKU, velikost, kapacity).
   * Názvy prostředků, který má toospecify pro snazší identifikaci.
   * Hodnoty, že používáte často toocomplete další úkoly (například správce uživatelské jméno).
   * Tajné klíče (jako jsou hesla).
   * číslo Hello nebo pole hodnoty toouse při vytváření více instancí typu prostředku.
* Použijte formát camelCase pro názvy parametrů.
* Zadejte popis všechny parametry v metadatech hello:

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "hello type of hello new storage account created toostore hello VM disks."
           }
       }
   }
   ```

* Definujte výchozí hodnoty pro parametry (s výjimkou hesla a klíče SSH):
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "hello type of hello new storage account created toostore hello VM disks."
            }
        }
   }
   ```

* Použití **SecureString** pro všechny hesla a tajné klíče: 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "hello value of hello secret toostore in hello vault."
           }
       }
   }
   ```

* Pokud je to možné, nepoužívejte parametr toospecify umístění. Místo toho použijte hello **umístění** vlastnost hello skupiny prostředků. Pomocí hello **resourceGroup () .location** výraz pro všechny prostředky, prostředky v šabloně hello nasadí hello stejné umístění jako skupina prostředků hello:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         ...
     }
   ]
   ```
   
   Pokud typ prostředku je podporován pouze omezený počet umístění, můžete chtít toospecify platné umístění přímo v šabloně hello. Pokud musíte použít **umístění** parametr, že hodnota parametru co nejvíce sdílet s prostředky, které jsou pravděpodobně toobe v hello stejné umístění. Tím se minimalizují hello počet oznámení, která jsou uživatelé vyzváni tooprovide informace o umístění.
* Vyhněte se použití parametr nebo proměnná pro hello verze rozhraní API pro typ prostředku. Vlastnosti prostředku a hodnoty se může lišit podle čísla verze. IntelliSense v editoru kódu nemůže určit správné schéma hello verze rozhraní API hello nastavena tooa parametr nebo proměnná. Místo toho pevně hello verze rozhraní API v šabloně hello.

## <a name="variables"></a>Proměnné
Hello následující informace mohou být užitečné při práci s proměnnými:

* Proměnné lze použijte pro hodnoty, je nutné toouse více než jednou v šabloně. Pokud hodnota je použit pouze jednou, hodnotě pevně je snazší tooread vaší šablony.
* Nemůžete použít hello [odkaz](resource-group-template-functions-resource.md#reference) funkce v hello **proměnné** části hello šablony. Hello **odkaz** funkce odvozuje svou hodnotu z stav modulu runtime hello prostředků. Proměnné jsou však vyřešen během počáteční analýzy hello šablony hello. Vytvoření hodnot, které je třeba hello **odkaz** funkce přímo v hello **prostředky** nebo **výstupy** části hello šablony.
* Zahrnout proměnné pro názvy prostředků, které musí být jedinečný, jak je popsáno v [názvy prostředků](#resource-names).
* Proměnné můžete seskupovat do komplexních objektů. Použití hello **variable.subentry** formátu tooreference hodnotu komplexního objektu. Seskupování proměnných vám mohou pomoci sledovat související proměnné. Také zlepšuje čitelnost hello šablony. Tady je příklad:
   
   ```json
   "variables": {
       "storage": {
           "name": "[concat(uniqueString(resourceGroup().id),'storage')]",
           "type": "Standard_LRS"
       }
   },
   "resources": [
     {
         "type": "Microsoft.Storage/storageAccounts",
         "name": "[variables('storage').name]",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "sku": {
             "name": "[variables('storage').type]"
         },
         ...
     }
   ]
   ```
   
   > [!NOTE]
   > Komplexní objekt nemůže obsahovat výraz, který odkazuje na hodnotu z komplexního objektu. Definujte proměnnou samostatné pro tento účel.
   > 
   > 
   
     Pokročilé příklady používání komplexních objektů jako proměnné najdete v tématu [sdílet stavu v šablonách Azure Resource Manager](best-practices-resource-manager-state.md).

## <a name="resources"></a>Zdroje
Hello následující informace mohou být užitečné při práci s prostředky:

* toohelp jiné přispěvatele pochopit účel hello hello prostředku, zadejte **komentáře** pro všechny prostředky v šabloně hello:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used toostore hello VM disks.",
         ...
     }
   ]
   ```

* Můžete použít tooresources metadata tooadd značky. Použijte tooadd informací o vašich prostředků. Například můžete přidat metadata toorecord fakturačních údajů pro prostředek. Další informace najdete v tématu [pomocí značky tooorganize vašich prostředků Azure](resource-group-using-tags.md).
* Pokud používáte *veřejný koncový bod* ve vaší šabloně (například Azure Blob storage veřejný koncový bod), *proveďte pevné kódování* hello oboru názvů. Použití hello **odkaz** funkce toodynamically načíst hello oboru názvů. Tento přístup toodeploy hello šablony toodifferent veřejné obor názvů prostředí můžete použít beze změny ručně hello koncového bodu v šabloně hello. Nastavit hello rozhraní API verze toohello stejnou verzi, kterou používáte pro účet úložiště hello v šabloně:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Pokud účet úložiště hello je nasazena v hello stejné šablony, kterou vytváříte, není nutné obor názvů zprostředkovatele hello toospecify když odkazujete hello prostředků. Toto je hello zjednodušenou syntaxi:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Pokud máte další hodnoty v šabloně, které jsou nakonfigurované toouse veřejné obor názvů, tyto hodnoty změnit tooreflect hello stejné **odkaz** funkce. Například můžete nastavit hello **storageUri** vlastnost diagnostického profilu virtuálního počítače hello:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   Můžete také použít stávající účet úložiště, který je v jiné skupině prostředků:

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* Přiřaďte veřejné IP adresy tooa virtuální počítač jenom v případě, že aplikace vyžaduje. tooconnect tooa virtuální počítač (VM) pro ladění, nebo pro správu nebo správu účely, použijte příchozích pravidel NAT, bránu virtuální sítě nebo jumpbox.
   
     Další informace o připojení toovirtual počítače najdete v tématu:
   
   * [Spustit virtuální počítače pro architekturu N-vrstvá v Azure](../guidance/guidance-compute-n-tier-vm.md)
   * [Nastavit přístup WinRM pro virtuální počítače ve službě Správce prostředků Azure](../virtual-machines/windows/winrm.md)
   * [Povolit tooyour externí přístup virtuálních počítačů pomocí hello portálu Azure](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [Povolit tooyour externí přístup virtuálních počítačů pomocí prostředí PowerShell](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [Povolit externí přístup tooyour virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* Hello **domainNameLabel** vlastnost pro veřejné IP adresy musí být jedinečný. Hello **domainNameLabel** hodnota musí být v rozmezí 3 až 63 znaků a postupujte podle pravidla hello určeného tento regulární výraz: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`. Protože hello **uniqueString** funkce generuje řetězec, který je 13 znaků, hello **dnsPrefixString** parametr je omezená too50 znaků:

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "hello DNS label for hello public IP address. It must be lowercase. It should match hello following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* Když přidáte rozšíření vlastních skriptů tooa a heslo, používat hello **commandToExecute** vlastnost hello **protectedSettings** vlastnost:
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > tooensure, které jsou tajné klíče šifrují, pokud jsou předávány jako parametry tooVMs a rozšíření, použijte hello **protectedSettings** vlastnost hello relevantní rozšíření.
   > 
   > 

## <a name="outputs"></a>Výstupy
Pokud chcete použít veřejné IP adresy toocreate šablony, patří **výstupy** oddíl, který vrátí podrobnosti hello IP adresy a hello plně kvalifikovaný název domény (FQDN). Po nasazení můžete použít výstup hodnoty tooeasily načíst podrobnosti o veřejné IP adresy a plně kvalifikované názvy domén. Když odkazujete hello prostředků, použijte hello rozhraní API verze, kterou jste použili toocreate ho: 

```json
"outputs": {
    "fqdn": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').ipAddress]",
        "type": "string"
    }
}
```

## <a name="single-template-vs-nested-templates"></a>Jediné šabloně vs. vnořené šablony
toodeploy řešení, můžete použít jednu šablonu nebo hlavní šablonu s více vnořených šablon. Vnořené šablony jsou společné pro pokročilejší scénáře. Pomocí šablony vnořené poskytuje, hello následující výhody:

* Lze rozčlenit řešení do cílové součásti.
* Vnořené šablon s jinou hlavní šablony můžete znovu použít.

Pokud si zvolíte toouse vnořené šablony, hello následující pokyny vám může pomoct standardizovat návrhu šablony. Tyto pokyny jsou založené na [vzory pro navrhování šablon Azure Resource Manageru](best-practices-resource-manager-design-templates.md). Doporučujeme, abyste k návrhu, který má hello následující šablony:

* **Hlavní šablonu** (azuredeploy.json). Používejte pro vstupní parametry hello.
* **Sdílené prostředky šablony**. Použití toodeploy sdílené prostředky, které používají jiné prostředky (například virtuální sítě a dostupnost sady). Použití hello **dependsOn** tooensure výrazu, tuto šablonu je nasadit před další šablony.
* **Volitelné prostředků šablony**. Použití tooconditionally nasadit prostředky v závislosti na parametr (například jumpbox).
* **Šablony prostředků člen**. Každý typ instance v aplikační vrstvě má svou vlastní konfiguraci. V rámci vrstvy můžete definovat typy jiné instance. (Například hello první instance vytvoří cluster a další instance jsou přidány toohello existujícího clusteru.) Každý typ instance má svou vlastní šablonu nasazení.
* **Skripty**. Široce opakovaně použitelné skripty platí pro každý typ instance (například inicializovat a formát další disky). Vlastní skripty, které vytvoříte za účelem přizpůsobení konkrétní se liší, v závislosti na typu instance hello.

![Vnořené šablony](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

Další informace najdete v tématu [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).

## <a name="conditionally-link-toonested-templates"></a>Podmíněná propojení toonested šablony
Můžete použít parametr tooconditionally odkaz toonested šablonách. Parametr Hello stane součástí hello identifikátor URI pro šablonu hello:

```json
"parameters": {
    "newOrExisting": {
        "type": "String",
        "allowedValues": [
            "new",
            "existing"
        ]
    }
},
"variables": {
    "templatelink": "[concat('https://raw.githubusercontent.com/Contoso/Templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
},
"resources": [
    {
        "apiVersion": "2015-01-01",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
            "mode": "incremental",
            "templateLink": {
                "uri": "[variables('templatelink')]",
                "contentVersion": "1.0.0.0"
            },
            "parameters": {
            }
        }
    }
]
```

## <a name="template-format"></a>Formát šablony
Je dobrým zvykem toopass šablony prostřednictvím validátor JSON. Validátor můžete odebrat nadbytečné čárkami, kulaté závorky a hranaté závorky, které může způsobit chybu během nasazení. Zkuste [JSONLint](http://jsonlint.com/) nebo linter balíček pro své oblíbené úpravou prostředí (Visual Studio Code, Atom, Sublime Text, Visual Studio).

Je také vhodné tooformat vaše struktury JSON pro lepší čitelnost. Můžete vytvořit balíček formátovací modul JSON pro vaše místní editor. V sadě Visual Studio, tooformat hello dokumentu, stiskněte klávesu **Ctrl + K, Ctrl + D**. V sadě Visual Studio Code stiskněte **Alt + Shift + F**. Pokud vaše místní editor nebude formátu hello dokumentu, můžete použít [online formátování](https://www.bing.com/search?q=json+formatter).

## <a name="next-steps"></a>Další kroky
* Pokyny na architekturu řešení virtuálních počítačů najdete v tématu [spustit virtuální počítač s Windows v Azure](../guidance/guidance-compute-single-vm.md) a [spuštění virtuálního počítače s Linuxem v Azure](../guidance/guidance-compute-single-vm-linux.md).
* Informace o nastavení účtu úložiště, najdete v části [kontrolní seznam výkonu a škálovatelnosti Azure Storage](../storage/common/storage-performance-checklist.md).
* toolearn o tom, jak podniku můžete použít tooeffectively Resource Manager můžete spravovat odběry, najdete v části [Azure enterprise vygenerované uživatelské rozhraní: doporučený předplatné zásad správného řízení](resource-manager-subscription-governance.md).

