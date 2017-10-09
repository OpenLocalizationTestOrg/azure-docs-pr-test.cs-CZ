Při nesmí začínat ani připojit tooan aplikace běžící v Azure virtuální počítač (VM) se z různých důvodů. Důvody zahrnují aplikace hello není spuštěna nebo naslouchá na portech hello očekává hello port naslouchání zablokovaný nebo pravidla předávání není správně aplikace toohello provoz sítě. Tento článek popisuje problém správné hello a toofind metodický přístup.

Pokud máte problémy s připojením tooyour virtuálních počítačů pomocí protokolu RDP nebo SSH, najdete v tématu jedna z následujících hello nejprve články:

* [Řešení potíží s tooa připojení ke vzdálené ploše virtuálního počítače založené na systému Windows Azure](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md)
* [Řešení potíží s Secure Shell (SSH) připojení tooa systémem Linux virtuálního počítače Azure](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../articles/resource-manager-deployment-model.md). Tento článek popisuje použití obou modelů, ale Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello MSDN Azure a hello Stack Overflow fóra](https://azure.microsoft.com/support/forums/). Alternativně můžete také soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.

## <a name="quick-start-troubleshooting-steps"></a>Úvodní při řešení potíží
Pokud máte potíže s připojením aplikace tooan, zkuste hello následující obecné kroky řešení potíží. Po dokončení každého kroku zkuste připojování tooyour aplikaci znovu:

* Restartování virtuálního počítače hello
* Znovu vytvořte koncový bod hello / pravidla brány firewall / sítě pravidel zabezpečení skupiny (NSG)
  * [Model Resource Manager - spravovat skupiny zabezpečení sítě](../articles/virtual-network/virtual-networks-create-nsg-arm-pportal.md)
  * [Klasického modelu – koncové body spravovat cloudové služby](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
* Připojení z různých umístění, jako je například jinou virtuální síť Azure
* Znovu nasadit virtuální počítač hello
  * [Znovu nasaďte Windows virtuálního počítače.](../articles/virtual-machines/windows/redeploy-to-new-node.md)
  * [Opětovné nasazení virtuálního počítače s Linuxem](../articles/virtual-machines/linux/redeploy-to-new-node.md)
* Znovu vytvořte virtuální počítač hello

Další informace najdete v tématu [řešení potíží s připojením koncový bod (RDP/SSH/HTTP, selhání atd.)](https://social.msdn.microsoft.com/Forums/azure/en-US/538a8f18-7c1f-4d6e-b81c-70c00e25c93d/troubleshooting-endpoint-connectivity-rdpsshhttp-etc-failures?forum=WAVirtualMachinesforWindows).

## <a name="detailed-troubleshooting-overview"></a>Podrobný přehled řešení potíží
Existují čtyři hlavní oblasti tootroubleshoot hello přístupu aplikace, která běží na virtuálním počítači Azure.

![řešení potíží s nelze spustit aplikaci](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access1.png)

1. Hello aplikace běžící v hello virtuální počítač Azure.
   * Vlastní aplikace hello funguje správně?
2. Hello virtuální počítač Azure.
   * Je hello virtuální počítač funguje správně, a odpovídá toorequests?
3. Koncové body Azure sítě.
   * Koncové body služby cloudu pro virtuální počítače ve model nasazení Classic hello.
   * Skupiny zabezpečení sítě a příchozích pravidel NAT pro virtuální počítače v modelu nasazení Resource Manager.
   * Můžete provozu toku z uživatelů toohello virtuálního počítače nebo aplikace na portech hello očekávání?
4. Hraniční zařízení Internetu.
   * Pravidla brány firewall na místě brání provoz předávaných správně?

Pro klientské počítače, které mají přístup k aplikaci hello prostřednictvím připojení site-to-site VPN nebo ExpressRoute hello hlavní oblasti, které může způsobit problémy jsou aplikace hello a hello virtuální počítač Azure.

Zdroj hello toodetermine hello problému a jeho oprava podle těchto kroků.

## <a name="step-1-access-application-from-target-vm"></a>Krok 1: Přístup k aplikaci z cílového virtuálního počítače
Zkuste tooaccess hello aplikace s programem hello příslušného klienta z hello virtuálního počítače, na kterém je spuštěný. Použijte název místního hostitele hello, hello místní IP adresu nebo adresu zpětné smyčky hello (127.0.0.1).

![spustit aplikaci přímo z hello virtuálních počítačů](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access2.png)

Například pokud aplikace hello webový server, otevřete prohlížeč na hello virtuálních počítačů a akci tooaccess webové stránky hostované na hello virtuálních počítačů.

Pokud máte přístup hello aplikace, přejděte příliš[kroku 2](#step2).

Pokud máte přístup k aplikaci hello, ověřte hello následující nastavení:

* Hello aplikace běží na hello cílového virtuálního počítače.
* Hello aplikace naslouchá na porty TCP a UDP hello očekává.

V systému Windows a virtuálních počítačích se systémem Linux, použijte hello **netstat - a** příkaz tooshow hello aktivní naslouchající porty. Zkontrolujte výstup hello hello očekává porty, na kterých by vaše aplikace naslouchá. Restartujte aplikace hello nebo jej podle potřeby nakonfigurovat porty hello očekává toouse a opakujte tooaccess hello aplikace místně.

## <a id="step2"></a>Krok 2: Přístup k aplikaci z jiného virtuálního počítače v hello stejné virtuální síti
Zkuste tooaccess hello aplikací z různých virtuálního počítače, ale v hello stejné virtuální síti, pomocí názvu hostitele hello Virtuálního počítače nebo přiřazené Azure veřejné, privátní nebo poskytovatele IP adresu. Pro virtuální počítače vytvořené pomocí modelu nasazení classic hello nepoužívejte hello veřejnou IP adresu hello cloudové služby.

![spustit aplikaci z různých virtuálního počítače](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access3.png)

Například pokud aplikace hello webový server, zkuste tooaccess webové stránky z prohlížeče na jiný virtuální počítač v hello stejné virtuální síti.

Pokud máte přístup hello aplikace, přejděte příliš[krok 3](#step3).

Pokud máte přístup k aplikaci hello, ověřte hello následující nastavení:

* Brána firewall hostitele Hello na hello cílovém virtuálním počítači umožňuje hello příchozí žádosti a odpovědi odchozí provoz.
* Zjišťování neoprávněných vniknutí nebo monitorování software spuštěný na cíli hello virtuálních počítačů sítě umožňuje hello provoz.
* Koncové body cloudové služby nebo skupiny zabezpečení sítě jsou umožňuje hello provoz:
  * [Klasického modelu – koncové body spravovat cloudové služby](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
  * [Model Resource Manager - spravovat skupiny zabezpečení sítě](../articles/virtual-network/virtual-networks-create-nsg-arm-pportal.md)
* Samostatná komponenta běžících v virtuálního počítače v cestě hello mezi hello testovací virtuální počítač a virtuální počítač, jako je nástroj pro vyrovnávání zatížení nebo brány firewall, umožňuje hello provoz.

Na virtuálním počítači systému Windows pomocí brány Windows Firewall s pokročilým zabezpečením toodetermine, zda pravidla brány firewall hello vyloučit příchozí a odchozí provoz vaší aplikace.

## <a id="step3"></a>Krok 3: Přístup k aplikaci z mimo hello virtuální sítě
Zkuste tooaccess hello aplikace z počítače mimo virtuální síť hello jako hello virtuální počítač, na kterém běží aplikace hello. Použijte jinou síť jako původní klientském počítači.

![Spusťte aplikaci z počítače mimo virtuální síť hello](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access4.png)

Například pokud aplikace hello je webový server, zkuste tooaccess hello webovou stránku v prohlížeči na počítači, který není ve virtuální síti hello.

Pokud máte přístup k aplikaci hello, ověřte hello následující nastavení:

* Pro virtuální počítače vytvořené pomocí modelu nasazení classic hello:
  
  * Zkontrolujte, že hello koncový bod konfigurace pro hello virtuálních počítačů umožňuje hello příchozí provoz, zejména hello protokol (TCP nebo UDP) a čísla portů veřejné a privátní hello.
  * Zkontrolujte, zda seznamy řízení přístupu (ACL) na koncovém bodu hello nebrání příchozí provoz z Internetu hello.
  * Další informace najdete v tématu [jak tooSet až koncové body tooa virtuální počítač](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* Pro virtuální počítače vytvořené pomocí modelu nasazení Resource Manager hello:
  
  * Ověřte, že který hello příchozí pravidlo NAT. hello virtuálních počítačů umožňuje hello příchozí provoz, zejména hello protokol (TCP nebo UDP) a čísla portů veřejné a privátní hello.
  * Ověřte, že jsou skupiny zabezpečení sítě umožňuje hello příchozí žádosti a odpovědi odchozí provoz.
  * Další informace najdete v článku [Skupina zabezpečení sítě](../articles/virtual-network/virtual-networks-nsg.md).

Pokud hello virtuálního počítače nebo koncový bod je členem sady Vyrovnávání zatížení sítě:

* Ověřte správnost hello testu protokol (TCP nebo UDP) a číslo portu.
* Pokud hello probe protokol a port se liší od hello sady s vyrovnáváním zatížení protokol a port:
  * Ověřte, že aplikace hello naslouchá na hello testu protokol (TCP nebo UDP) a číslo portu (použijte **netstat-a** na hello cíle virtuálních počítačů).
  * Zkontrolujte, aby brána firewall hostitele hello na hello cílových virtuálních počítačů umožňuje hello příchozí zkušebního požadavku a odpovědi provoz odchozí testu.

Pokud máte přístup aplikace hello, ověřte, že je umožňuje vaše Internet hraniční zařízení:

* Hello odchozí aplikace požadavek provoz z vašeho klienta počítače toohello virtuální počítač Azure.
* Hello příchozí aplikace odpovědi provoz z hello virtuální počítač Azure.

## <a name="step-4-if-you-cannot-access-hello-application-use-ip-verify-toocheck-hello-settings"></a>Krok 4, pokud nemají přístup k aplikaci hello, použijte ověřte IP toocheck hello nastavení. 

Další informace najdete v tématu [síť Azure Přehled monitorování](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview). 

## <a name="additional-resources"></a>Další zdroje
[Řešení potíží s tooa připojení ke vzdálené ploše virtuálního počítače založené na systému Windows Azure](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md)

[Řešení potíží s Secure Shell (SSH) připojení tooa systémem Linux virtuálního počítače Azure](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md)

