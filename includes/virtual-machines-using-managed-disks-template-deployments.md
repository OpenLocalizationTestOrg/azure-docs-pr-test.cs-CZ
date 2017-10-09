# <a name="using-managed-disks-in-azure-resource-manager-templates"></a>Pomocí spravovaného disky v šablonách Azure Resource Manageru

Tento dokument vás provede hello rozdíly mezi spravovanými a nespravovanými disky pomocí Azure Resource Manager šablony tooprovision virtuálních počítačů. To vám pomůže tooupdate existujících šablon, které používají nespravovaná disky toomanaged disky. Pro referenci používáme hello [101-vm jednoduché – windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) šablony jako vodítko. Uvidíte hello šablony pomocí obou [discích spravovaných](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) a předchozí verze pomocí [nespravované disky](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) budete-li toodirectly porovnat.

## <a name="unmanaged-disks-template-formatting"></a>Nespravované disky šablony, formátování

toobegin, jsme se podívejte na tom, jak nespravované disky jsou nasazeny. Při vytváření nespravované disků, je třeba souborů virtuálního pevného disku hello toohold účet úložiště. Můžete vytvořit nový účet úložiště nebo použít jednu, která již existuje. Tento článek vám ukáže, jak toocreate nový účet úložiště. tooaccomplish, budete potřebovat prostředek účet úložiště v bloku hello prostředky, jak je uvedeno níže.

```
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2016-01-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
}
```

V rámci hello objektu virtuálního počítače potřebujeme závislost na hello tooensure účet úložiště, vytvořené před hello virtuálního počítače. V rámci hello `storageProfile` tématu, potom zadejte hello úplný identifikátor URI hello umístění virtuálního pevného disku, který odkazuje na účet úložiště hello a je potřebná pro disk hello operačního systému a všech datových disků. 

```
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "name": "osdisk",
                "vhd": {
                    "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/osdisk.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "name": "datadisk1",
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "vhd": {
                        "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/datadisk1.vhd')]"
                    },
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

## <a name="managed-disks-template-formatting"></a>Spravované disky šablony formátování

S Azure spravované disky hello disku se změní na nejvyšší úrovni prostředků a už vyžaduje toobe účet úložiště vytvořené uživatelem hello. Spravované disky nejprve byly vystaveny v hello `2016-04-30-preview` verze rozhraní API, jsou k dispozici ve všech dalších verzích rozhraní API a se teď typ disku výchozí hello. Hello následující části provede hello výchozí nastavení a podrobností, jak přizpůsobit toofurther vaše disky.

> [!NOTE]
> Je doporučeno toouse rozhraní API verze pozdější než `2016-04-30-preview` jako došlo k narušující změny mezi `2016-04-30-preview` a `2017-03-30`.
>
>

### <a name="default-managed-disk-settings"></a>Výchozí nastavení spravovaného disku

toocreate virtuální počítač s spravovaných disků, je už potřebovat toocreate hello úložiště účet prostředků a následujícím způsobem můžete aktualizovat prostředek virtuálního počítače. Specificky Upozorňujeme, že hello `apiVersion` odráží `2017-03-30` a hello `osDisk` a `dataDisks` už odkazovat tooa konkrétní identifikátor URI pro hello virtuálního pevného disku. Pokud nasazujete bez zadání dalších vlastností, bude používat hello disku [úložiště LRS standardní](../articles/storage/common/storage-redundancy.md). Pokud není zadán žádný název, jak dlouho trvá hello formát `<VMName>_OsDisk_1_<randomstring>` pro disk hello operačního systému a `<VMName>_disk<#>_<randomstring>` pro každý datový disk. Ve výchozím nastavení je Azure disk encryption zakázaný. ukládání do mezipaměti je pro čtení a zápis pro disk hello operačního systému a jeden pro datové disky. Můžete si povšimnout v následujícím příkladu hello, že je stále závislost účtu úložiště, i když toto je pouze pro úložiště diagnostiky a není potřeba ukládání na disk.

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="using-a-top-level-managed-disk-resource"></a>Použití spravovaných disků na nejvyšší úrovni prostředků

Jako alternativní toospecifying hello konfiguraci disku v objektu hello virtuálního počítače můžete vytvořit nejvyšší úrovně diskový prostředek a připojte ji jako součást hello vytvoření virtuálního počítače. Například můžeme vytvořit prostředek disku takto toouse jako datový disk.

```
{
    "type": "Microsoft.Compute/disks",
    "name": "[concat(variables('vmName'),'-datadisk1')]",
    "apiVersion": "2017-03-30",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "properties": {
        "creationData": {
            "createOption": "Empty"
        },
        "diskSizeGB": 1023
    }
}
```

V rámci hello objekt virtuálního počítače jsme pak můžete odkazovat toobe objekt tento disk připojit. Zadání ID prostředku hello hello spravované disk, který jsme vytvořili v hello `managedDisk` vlastnost umožňuje hello připojení disku hello jako hello vytvoří virtuální počítač. Všimněte si, že hello `apiVersion` hello prostředků virtuálního počítače je příliš nastavený`2017-03-30`. Všimněte si také, že vytvořili jsme závislost na hello disku prostředků tooensure, které je úspěšně vytvořen před vytvořením virtuálních počítačů. 

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]",
        "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "lun": 0,
                    "name": "[concat(variables('vmName'),'-datadisk1')]",
                    "createOption": "attach",
                    "managedDisk": {
                        "id": "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
                    }
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="create-managed-availability-sets-with-vms-using-managed-disks"></a>Vytvoření skupiny dostupnosti spravované s virtuálními počítači pomocí spravovaných disků

toocreate spravované dostupnost sady s virtuálními počítači pomocí spravovaných disků, přidejte hello `sku` objekt toohello dostupnosti prostředků a nastavte a hello `name` vlastnost příliš`Aligned`. Tím se zajistí hello disky pro jednotlivé virtuální počítače dostatečně izolované od sebe navzájem tooavoid jediný bod selhání. Všimněte si, že hello `apiVersion` pro prostředek skupiny dostupnosti hello je nastaven příliš`2017-03-30`.

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/availabilitySets",
    "location": "[resourceGroup().location]",
    "name": "[variables('avSetName')]",
    "properties": {
        "PlatformUpdateDomainCount": 3,
        "PlatformFaultDomainCount": 2
    },
    "sku": {
        "name": "Aligned"
    }
}
```

### <a name="additional-scenarios-and-customizations"></a>Další scénáře a přizpůsobení

úplné informace toofind na specifikacích hello REST API, přečtěte si hello [vytvoření spravovaného disku dokumentace k REST API](/rest/api/manageddisks/disks/disks-create-or-update). Zjistíte další scénáře, a také výchozí a přijatelných hodnot, které mohou být odeslaná toohello rozhraní API prostřednictvím šablony nasazení. 

## <a name="next-steps"></a>Další kroky

* Úplné šablony, které používají spravovaných disků na adrese hello následující odkazy úložišti Azure rychlý start.
    * [Virtuální počítač s Windows spravované disku](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
    * [Virtuální počítač s Linuxem pomocí spravovaného disku](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
    * [Úplný seznam spravovaných disků na šablony](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* Navštivte hello [přehled disky spravované Azure](../articles/virtual-machines/windows/managed-disks-overview.md) dokumentu toolearn Další informace o discích spravovaných.
* Zkontrolujte hello šablony referenční dokumentaci pro prostředky virtuálního počítače návštěvou hello [odkaz na šablonu Microsoft.Compute/virtualMachines](/templates/microsoft.compute/virtualmachines) dokumentu.
* Zkontrolujte hello šablony referenční dokumentaci pro prostředky disku návštěvou hello [odkaz na šablonu Microsoft.Compute/disks](/templates/microsoft.compute/disks) dokumentu.
 
