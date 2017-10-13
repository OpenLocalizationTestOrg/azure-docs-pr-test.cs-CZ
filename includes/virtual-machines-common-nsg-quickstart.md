<span data-ttu-id="d50e5-101">Otevřete port, nebo vytvořit koncový bod, tooa virtuální počítač (VM) v Azure vytvořením filtr sítě na podsíť nebo rozhraní sítě virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d50e5-101">You open a port, or create an endpoint, tooa virtual machine (VM) in Azure by creating a network filter on a subnet or VM network interface.</span></span> <span data-ttu-id="d50e5-102">Tyto filtry, které řídí příchozí a odchozí přenosy, můžete umístit na skupinu zabezpečení sítě připojené toohello prostředku, který přijímá provoz hello.</span><span class="sxs-lookup"><span data-stu-id="d50e5-102">You place these filters, which control both inbound and outbound traffic, on a Network Security Group attached toohello resource that receives hello traffic.</span></span>

<span data-ttu-id="d50e5-103">Použijeme Běžným příkladem webové přenosy na portu 80.</span><span class="sxs-lookup"><span data-stu-id="d50e5-103">Let's use a common example of web traffic on port 80.</span></span> <span data-ttu-id="d50e5-104">Jakmile je virtuální počítač, který je nakonfigurovaný tooserve webových požadavků na hello standardní port TCP 80 (mějte na paměti, toostart hello odpovídající služby a otevřít všechna pravidla brány firewall operačního systému na hello virtuálních počítačů i), můžete:</span><span class="sxs-lookup"><span data-stu-id="d50e5-104">Once you have a VM that is configured tooserve web requests on hello standard TCP port 80 (remember toostart hello appropriate services and open any OS firewall rules on hello VM as well), you:</span></span>

1. <span data-ttu-id="d50e5-105">Vytvořte skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="d50e5-105">Create a Network Security Group.</span></span>
2. <span data-ttu-id="d50e5-106">Vytvoření příchozího pravidla umožňuje provoz se:</span><span class="sxs-lookup"><span data-stu-id="d50e5-106">Create an inbound rule allowing traffic with:</span></span>
   * <span data-ttu-id="d50e5-107">Rozsah cílových portů Hello "80"</span><span class="sxs-lookup"><span data-stu-id="d50e5-107">hello destination port range of "80"</span></span>
   * <span data-ttu-id="d50e5-108">Rozsah zdrojových portů z Hello "*" (což jakéhokoli zdrojového portu)</span><span class="sxs-lookup"><span data-stu-id="d50e5-108">hello source port range of "*" (allowing any source port)</span></span>
   * <span data-ttu-id="d50e5-109">hodnotou priority menší 65 500 (toobe vyšší priorita, než hello catch všechny výchozí příchozí pravidlo Odepřít)</span><span class="sxs-lookup"><span data-stu-id="d50e5-109">a priority value of less 65,500 (toobe higher in priority than hello default catch-all deny inbound rule)</span></span>
3. <span data-ttu-id="d50e5-110">Přidružte hello rozhraní sítě virtuálních počítačů nebo podsíť hello skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="d50e5-110">Associate hello Network Security Group with hello VM network interface or subnet.</span></span>

<span data-ttu-id="d50e5-111">Konfigurace toosecure nejsložitějších sítí můžete vytvořit prostředí pomocí skupin zabezpečení sítě a pravidla.</span><span class="sxs-lookup"><span data-stu-id="d50e5-111">You can create complex network configurations toosecure your environment using Network Security Groups and rules.</span></span> <span data-ttu-id="d50e5-112">Naše Ukázka používá jenom jednu nebo dvě pravidla, která povolit provoz protokolu HTTP nebo pro vzdálenou správu.</span><span class="sxs-lookup"><span data-stu-id="d50e5-112">Our example uses only one or two rules that allow HTTP traffic or remote management.</span></span> <span data-ttu-id="d50e5-113">Další informace najdete v tématu hello následující ["Další informace"](#more-information-on-network-security-groups) části nebo [co je skupina zabezpečení sítě?](../articles/virtual-network/virtual-networks-nsg.md)</span><span class="sxs-lookup"><span data-stu-id="d50e5-113">For more information, see hello following ['More Information'](#more-information-on-network-security-groups) section or [What is a Network Security Group?](../articles/virtual-network/virtual-networks-nsg.md)</span></span>
