> [!div class="op_single_selector"]
> * [Azure Portal](../articles/virtual-network/virtual-network-multiple-ip-addresses-portal.md)
> * [PowerShell](../articles/virtual-network/virtual-network-multiple-ip-addresses-powershell.md)
> * [CLI 2.0](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli.md)
> * [CLI 1.0](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli-nodejs.md)
> * [Šablona](../articles/virtual-network/virtual-network-multiple-ip-addresses-template.md)
>

Virtuální počítač Azure (VM) má jeden nebo více síťových rozhraní (NIC) připojených tooit. Všechny síťové adaptéry, může mít jeden nebo více statické nebo dynamické veřejné a privátní IP adresy přiřazené tooit. Přiřazení více IP adres tooa virtuálních počítačů umožňuje hello následující možnosti:

* Hostovat několik webů nebo služeb s různými IP adresami a certifikáty SSL na jednom serveru.
* Využívat počítač jako virtuální síťové zařízení, jako je třeba brána firewall nebo nástroj pro vyrovnávání zatížení.
* Hello možnost tooadd žádné hello privátní IP adresy pro všechny síťové adaptéry tooan hello fond back-end pro vyrovnávání zatížení Azure. V hello po pouze hello primární IP adresu pro hello primární síťový adaptér může být přidána tooa fond back-end. Další informace o toolearn jak tooload vyrovnávat víc konfigurací IP adres, přečtěte si hello [víc konfigurací IP adres služby Vyrovnávání zatížení](../articles/load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.

Každou síťovou kartu připojenou tooa virtuální počítač obsahuje jednu nebo více konfigurací IP adres přidružené tooit. Každá konfigurace má přiřazenou jednu statickou nebo dynamickou privátní IP adresu. Každá konfigurace mohou mít i jeden veřejný tooit prostředků přidružené adresy IP. Prostředek veřejné IP adresy se buď dynamická nebo statická veřejnou IP adresu přiřazenou tooit. toolearn Další informace o IP adresy v Azure, přečtěte si hello [IP adresách v Azure](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) článku. 

Existuje limit toohow mnoho privátní IP adresy lze přiřadit tooa síťový adaptér. Je zde také limit toohow mnoho veřejné IP adresy, které mohou být používány předplatné Azure. V tématu hello [Azure omezuje](../articles/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) článku.
