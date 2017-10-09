## <a name="overview-of-azure-resource-manager-templates"></a>Přehled šablon Azure Resource Manageru
Šablony Azure Resource Manager umožňují toodeclaratively zadejte hello infrastruktury Azure IaaS v jazyce Json definováním hello závislosti mezi prostředky. Podrobný přehled o šablon Azure Resource Manageru najdete v článku toohello níže:

[Skupina prostředků – přehled](../articles/azure-resource-manager/resource-group-overview.md)

## <a name="sample-template-snippet-for-vm-extensions"></a>Ukázka šablony fragment kódu pro rozšíření virtuálního počítače
Nasazení rozšíření virtuálního počítače jako součást šablonu Azure Resource Manager, musíte zadat toodeclaratively konfigurace rozšíření hello v šabloně hello.
Zde je hello formát pro zadání konfigurace rozšíření hello.

      {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "MyExtension",
      "apiVersion": "2015-05-01-preview",
      "location": "[parameters('location')]",
      "dependsOn": ["[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"],
      "properties":
      {
      "publisher": "Publisher Namespace",
      "type": "extension Name",
      "typeHandlerVersion": "extension version",
      "settings": {
      // Extension specific configuration goes in here.
      }
      }
      }

Jak je vidět z výše uvedených hello hello rozšíření šablona obsahuje dvě hlavní části:

1. Rozšíření název, vydavatel a verze
2. Konfigurace rozšíření.

## <a name="identifying-hello-publisher-type-and-typehandlerversion-for-any-extension"></a>Identifikace hello vydavatele, typ a typeHandlerVersion pro jakoukoli příponu
Rozšíření virtuálního počítače Azure jsou publikovány společností Microsoft a důvěryhodných vydavatelů 3. stran a každé rozšíření je jedinečně identifikovaný typeHandlerVersion jeho vydavatele, typ a hello. To se dá určit jako následující:  

