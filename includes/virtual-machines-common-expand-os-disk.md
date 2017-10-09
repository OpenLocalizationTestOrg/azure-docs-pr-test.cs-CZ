## <a name="overview"></a>Přehled
Při vytváření nového virtuálního počítače (VM) ve skupině prostředků nasazením bitové kopie z [Azure Marketplace](https://azure.microsoft.com/marketplace/), jednotka operačního systému výchozí hello je 127 GB. Přestože je možné tooadd datové disky toohello virtuálních počítačů (kolik v závislosti na hello SKU jste zvolili) a kromě toho je doporučené tooinstall aplikací a zatížení s intenzivním procesoru v těchto discích dodatek, často mají zákazníci tooexpand hello operačního systému jednotka toosupport určité scénáře, jako je následující:

1. Podpora starších verzí aplikací, které instalují komponenty na jednotku operačního systému.
2. Migrace fyzického nebo virtuálního počítače s větší jednotkou operačního systému z místního prostředí.

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: Resource Manager a Classic. Tento článek se zabývá pomocí modelu Resource Manager hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.
> 
> 

## <a name="resize-hello-os-drive"></a>Změna velikosti disku hello operačního systému
V tomto článku jsme vám dá dosáhnout hello o změně velikosti disku hello operačního systému pomocí resource manager modul [prostředí Azure Powershell](/powershell/azureps-cmdlets-docs). Otevřete prostředí Powershell ISE nebo okno prostředí Powershell v režimu správce a postupujte podle následujících kroků hello:

1. Přihlášení tooyour Microsoft Azure účet v režimu správy prostředků a vybrat své předplatné následujícím způsobem:
   
   ```Powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Nastavte název skupiny prostředků a název virtuálního počítače následujícím způsobem:
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Získejte odkaz tooyour virtuální počítač následujícím způsobem:
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Zastavte hello virtuální počítač před změnou velikosti disku hello následujícím způsobem:
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. A tady se dodává hello okamžiku, kdy jsme jste čekali pro! Nastavení velikosti hello hodnota toohello potřeby disku hello operačního systému a aktualizace hello virtuální počítač následujícím způsobem:
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > Nová velikost Hello musí být větší než velikost disku existující hello. maximální povolené Hello je 1023 GB.
   > 
   > 
6. Aktualizace hello virtuálních počítačů může trvat několik sekund. Po dokončení provádění příkazu hello restartujte hello virtuální počítač následujícím způsobem:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

A to je vše! Nyní RDP do hello virtuální počítač, otevřete správu počítače (nebo Správa disků) a rozbalte pomocí hello nově přidělené místo na disku hello.

## <a name="summary"></a>Souhrn
V tomto článku jsme použili moduly Azure Resource Manageru z prostředí Powershell tooexpand hello jednotky operačního systému virtuálního počítače IaaS. Hello celý skript pro vaši informaci reprodukovat níže je:

```Powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="next-steps"></a>Další kroky
Když v tomto článku jsme zaměřuje především na rozšiřování hello operačního systému disku hello virtuálních počítačů, hello vyvinuté skript může být rovněž pro rozšíření hello datové disky připojené toohello virtuálních počítačů tak, že změníte jeden řádek kódu. Například tooexpand hello první data připojené toohello virtuálních počítačů na disku, nahraďte hello ```OSDisk``` objekt ```StorageProfile``` s ```DataDisks``` pole a používat číselný index tooobtain odkaz toofirst připojené datový disk, jak je uvedeno níže:

```Powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```
Podobně můžete odkazovat na jiné datové disky připojené toohello virtuálních počítačů, buď pomocí indexu jako v příkladu nahoře, nebo hello ```Name``` vlastnost hello disku, jak je uvedeno dále:

```Powershell
($vm.StorageProfile.DataDisks | Where {$_.Name -eq 'my-second-data-disk'})[0].DiskSizeGB = 1023
```

Pokud chcete toofind na tom, jak tooattach disky tooan virtuálního počítače Azure Resource Manager, zaškrtněte toto políčko [článku](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

