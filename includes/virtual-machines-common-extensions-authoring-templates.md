## <a name="overview-of-azure-resource-manager-templates"></a>Přehled šablon Azure Resource Manageru
Šablony Azure Resource Manageru umožní deklarativně zadejte infrastruktury Azure IaaS v jazyce Json definováním závislosti mezi prostředky. Podrobný přehled o šablon Azure Resource Manageru naleznete v článku níže:

[Skupina prostředků – přehled](../articles/azure-resource-manager/resource-group-overview.md)

## <a name="sample-template-snippet-for-vm-extensions"></a>Ukázka šablony fragment kódu pro rozšíření virtuálního počítače
Nasazení rozšíření virtuálního počítače v rámci služby Správce prostředků Azure vyžaduje šablony vám deklarativně určit konfigurace rozšíření v šabloně.
Zde je formát pro zadání konfigurace rozšíření.

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

Jak vidíte z výše uvedeného, šablona rozšíření obsahuje dvě hlavní části:

1. Rozšíření název, vydavatel a verze
2. Konfigurace rozšíření.

## <a name="identifying-the-publisher-type-and-typehandlerversion-for-any-extension"></a>Identifikace vydavatele, typ a typeHandlerVersion pro jakoukoli příponu
Rozšíření virtuálního počítače Azure jsou publikovány společností Microsoft a důvěryhodných vydavatelů 3. stran a každé rozšíření je jedinečně identifikovaný vydavatele, typ a typeHandlerVersion. To se dá určit jako následující:  

