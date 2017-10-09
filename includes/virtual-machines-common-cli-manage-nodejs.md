Než budete moci použít hello rozhraní příkazového řádku Azure s Resource Manager příkazy a šablony toodeploy Azure prostředky a úlohy, použití skupin prostředků, budete potřebovat účet s Azure. Pokud účet nemáte, [tady můžete získat bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).

Pokud jste ještě nenainstalovali hello rozhraní příkazového řádku Azure a připojené tooyour předplatné, přečtěte si téma [hello instalace rozhraní příkazového řádku Azure](../articles/cli-install-nodejs.md) nastavení režimu hello příliš`arm` s `azure config mode arm`a připojte tooAzure s hello `azure login` příkaz.

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- Rozhraní příkazového řádku Azure 10 – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](../articles/virtual-machines/linux/cli-manage.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello

## <a name="basic-azure-resource-manager-commands-in-azure-cli"></a>Základní příkazy Azure Resource Manageru v Azure CLI
Tento článek se týká základních příkazů bude chcete toouse pomocí rozhraní příkazového řádku Azure toomanage a pracovat s prostředky (hlavně virtuální počítače) ve vašem předplatném Azure.  Podrobné pomoc s přepínače konkrétní příkazového řádku a možnosti, můžete použít příkaz online nápovědu hello a možnosti zadáním `azure <command> <subcommand> --help` nebo `azure help <command> <subcommand>`.

> [!NOTE]
> Tyto příklady nezahrnují operace založené na šablonách, které se obecně doporučují pro nasazení virtuálních počítačů v Resource Manageru. Informace najdete v tématu [hello použití Azure CLI s Azure Resource Manager](../articles/xplat-cli-azure-resource-manager.md) a [nasadit a spravovat virtuální počítače pomocí šablony Azure Resource Manager a hello rozhraní příkazového řádku Azure](../articles/virtual-machines/linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

| Úkol | Resource Manager |
| --- | --- | --- |
| Vytvoření hello nejzákladnější virtuálních počítačů |`azure vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password>`<br/><br/>(Získat hello `image-urn` z hello `azure vm image list` příkaz. Příklady najdete v [tomto článku](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).) |
| Vytvoření virtuálního počítače s Linuxem |`azure  vm create [options] <resource-group> <name> <location> -y "Linux"` |
| Vytvoření virtuálního počítače s Windows |`azure  vm create [options] <resource-group> <name> <location> -y "Windows"` |
| Výpis virtuálních počítačů |`azure  vm list [options]` |
| Získání informací o virtuálním počítači |`azure  vm show [options] <resource_group> <name>` |
| Spuštění virtuálního počítače |`azure vm start [options] <resource_group> <name>` |
| Zastavení virtuálního počítače |`azure vm stop [options] <resource_group> <name>` |
| Uvolnění virtuálního počítače |`azure vm deallocate [options] <resource-group> <name>` |
| Restartování virtuálního počítače |`azure vm restart [options] <resource_group> <name>` |
| Odstranění virtuálního počítače |`azure vm delete [options] <resource_group> <name>` |
| Zachycení virtuálního počítače |`azure vm capture [options] <resource_group> <name>` |
| Vytvoření virtuálního počítače z uživatelské image |`azure  vm create [options] –q <image-name> <resource-group> <name> <location> <os-type>` |
| Vytvoření virtuálního počítače ze specializovaného disku |`azue  vm create [options] –d <os-disk-vhd> <resource-group> <name> <location> <os-type>` |
| Přidání disku tooa data virtuálního počítače |`azure  vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]` |
| Odebrání datového disku z virtuálního počítače |`azure  vm disk detach [options] <resource-group> <vm-name> <lun>` |
| Přidat tooa obecné rozšíření virtuálního počítače |`azure  vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>` |
| Přidat virtuální počítač přístup tooa rozšíření virtuálního počítače |`azure vm reset-access [options] <resource-group> <name>` |
| Přidat tooa Docker rozšíření virtuálního počítače |`azure  vm docker create [options] <resource-group> <name> <location> <os-type>` |
| Odebrání rozšíření virtuálního počítače |`azure  vm extension set [options] –u <resource-group> <vm-name> <name> <publisher-name> <version>` |
| Získání využití prostředků virtuálního počítače |`azure vm list-usage [options] <location>` |
| Získání všech dostupných velikostí virtuálních počítačů |`azure vm sizes [options]` |

## <a name="next-steps"></a>Další kroky
* Další příklady příkazů hello rozhraní příkazového řádku, než jsou základní správu virtuálních počítačů najdete v tématu [hello pomocí rozhraní příkazového řádku Azure s Azure Resource Manager](../articles/virtual-machines/azure-cli-arm-commands.md).
