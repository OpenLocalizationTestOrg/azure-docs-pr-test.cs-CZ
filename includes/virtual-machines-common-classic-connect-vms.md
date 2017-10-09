

![Virtuální počítače v samostatné cloudové služby](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

Pokud vaše virtuální počítače ve virtuální síti, můžete se rozhodnout, kolik cloudové služby, které chcete toouse pro načtení sady vyrovnávání a dostupnost. Kromě toho můžete uspořádat hello virtuální počítače v podsítích v hello stejným způsobem, jak místní sítě a připojení virtuální sítě tooyour hello místní sítě. Tady je příklad:

![Virtuální počítače ve virtuální síti](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

Virtuální sítě jsou hello doporučená způsob tooconnect virtuálních počítačů v Azure. Hello osvědčeným postupem je tooconfigure jednotlivých úrovní vaší aplikace v samostatných cloudové službě. Ale musíte toocombine některé virtuální počítače z vrstvy jinou aplikaci do hello stejný cloudový tooremain služby v rámci hello maximálně 200 cloudové služby na jedno předplatné. tooreview tato a další omezení, najdete v části [předplatné Azure a omezení služby, kvóty a omezení](../articles/azure-subscription-service-limits.md).

## <a name="connect-vms-in-a-virtual-network"></a>Připojit virtuální počítače ve virtuální síti
tooconnect virtuální počítače ve virtuální síti:

1. Vytvoření virtuální sítě hello v hello [portál Azure](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) a zadejte 'nasazení classic'.
2. Vytvořit sadu hello cloudových služeb pro vaše nasazení tooreflect návrhu pro skupiny dostupnosti a vyrovnávání zatížení. V hello portálu Azure, klikněte na **nový > výpočetní > Cloudová služba** pro jednotlivých cloudových služeb.

  Při vyplňování hello cloudové služby podrobnosti, vyberte stejný hello _skupiny prostředků_ použít s hello virtuální sítě.

3. toocreate každý nový virtuální počítač, klikněte na tlačítko **nový > výpočetní**, pak vyberte hello příslušné bitové kopie virtuálního počítače z hello **vybrané aplikace**.

  V hello virtuálních počítačů **Základy** okně zvolte hello stejné _skupiny prostředků_ použít s hello virtuální sítě.

  ![Okno základy virtuálních počítačů při použití virtuální sítě](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_VN.png)

4. Při vyplňování hello virtuálních počítačů **nastavení**, zvolte hello správné _Cloudová služba_ nebo _virtuální sítě_ pro hello virtuálních počítačů.

  Azure vybere hello jiných položek na základě vašeho výběru.

  ![Okno nastavení virtuálního počítače při použití virtuální sítě](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_VN.png)


## <a name="connect-vms-in-a-standalone-cloud-service"></a>Připojit virtuální počítače v samostatné cloudové služby
tooconnect virtuální počítače v cloudové službě samostatné:

1. Vytvoření hello cloudové služby v hello [portál Azure](http://portal.azure.com). Klikněte na tlačítko **nový > výpočetní > Cloudová služba**. Nebo můžete vytvořit hello cloudovou službu pro nasazení, když vytvoříte první virtuální počítač.
2. Když vytvoříte hello virtuální počítače, zvolte hello používá stejnou skupinu prostředků s cloudovou službou hello.

  ![Přidat virtuální počítač tooan existující cloudové služby](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_SA.png)

3.  Při vyplňování hello virtuálních počítačů podrobnosti, vyberte název hello vytvořili v prvním kroku hello cloudové služby.

  ![Výběr cloudové služby pro virtuální počítač](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_SA.png)
