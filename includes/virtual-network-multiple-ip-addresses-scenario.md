## <a name="scenario"></a>Scénář
Virtuální počítač s jednu síťovou kartu je tooa vytvořené a připojené virtuální sítě. Hello virtuálního počítače vyžaduje tři různé *privátní* IP adresy a dvě *veřejné* IP adresy. Hello IP adresy přiřazené toohello následující konfigurace protokolu IP:

* **IPConfig-1:** přiřadí *statické* privátní IP adresu a *statické* veřejnou IP adresu.
* **IPConfig-2:** přiřadí *statické* privátní IP adresu a *statické* veřejnou IP adresu.
* **IPConfig – 3:** přiřadí *statické* privátní IP adresu a žádné veřejnou IP adresu.
  
    ![Několik IP adres](./media/virtual-network-multiple-ip-addresses-scenario/multiple-ipconfigs.png)

Konfigurace protokolu IP Hello jsou přidružené toohello seskupování při hello vytvořit síťovou kartu a hello síťový adaptér je připojený toohello virtuálních počítačů při vytvoření hello virtuálních počítačů. pro ilustraci jsou Hello typy adres IP použitých pro scénář hello. Můžete přiřadit libovolnou IP adresy a přiřazení typy budete potřebovat.

> [!NOTE]
> I když hello kroky tomto článku přiřadí všechny tooa konfigurace IP jednu síťovou kartu, je také možné přiřadit více IP konfigurace tooany síťový adaptér ve virtuálním počítači více síťovými Kartami. jak toocreate virtuálního počítače s více síťovými kartami, číst toolearn hello [vytvoření virtuálního počítače s více síťovými kartami](../articles/virtual-network/virtual-network-deploy-multinic-arm-ps.md) článku.
