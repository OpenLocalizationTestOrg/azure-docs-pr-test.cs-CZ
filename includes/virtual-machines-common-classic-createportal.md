

<span data-ttu-id="23cd4-101">*Vlastní* virtuální počítač znamená jednoduše virtuální počítač, který vytvoříte pomocí **vybrané aplikace** z **Marketplace**, protože za vás udělá velkou část práce.</span><span class="sxs-lookup"><span data-stu-id="23cd4-101">A *custom* virtual machine simply means a virtual machine that you create using a **Featured app** from the **Marketplace** because it does much of the work for you.</span></span> <span data-ttu-id="23cd4-102">I tak ale můžete upravovat konfiguraci včetně následujících položek:</span><span class="sxs-lookup"><span data-stu-id="23cd4-102">Yet, you can still make configuration choices that include the following items:</span></span>

* <span data-ttu-id="23cd4-103">Připojení virtuálního počítače k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="23cd4-103">Connecting the virtual machine to a virtual network.</span></span>
* <span data-ttu-id="23cd4-104">Instalace agenta virtuálního počítače Azure a rozšíření virtuálního počítače Azure, například pro antimalware.</span><span class="sxs-lookup"><span data-stu-id="23cd4-104">Installing the Azure Virtual Machine Agent and Azure Virtual Machine Extensions, such as for antimalware.</span></span>
* <span data-ttu-id="23cd4-105">Přidání virtuálního počítače do existujících cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="23cd4-105">Adding the virtual machine to existing cloud services.</span></span>
* <span data-ttu-id="23cd4-106">Přidání virtuálního počítače do existujícího účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="23cd4-106">Adding the virtual machine to an existing Storage account.</span></span>
* <span data-ttu-id="23cd4-107">Přidání virtuálního počítače do skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="23cd4-107">Adding the virtual machine to an availability set.</span></span>

<!--
> [!IMPORTANT]
> If you want your virtual machine to use a virtual network so you can connect to it directly by host name or set up cross-premises connections, make sure that you specify the virtual network when you create the virtual machine. A virtual machine can be configured to join a virtual network only when you create the virtual machine. For details on virtual networks, see [Azure Virtual Network overview](../articles/virtual-network/virtual-networks-overview.md).
>
>
 -->

> [!IMPORTANT]
> <span data-ttu-id="23cd4-108">Pokud chcete, aby virtuální počítač používal virtuální síť, ujistěte se, že je virtuální síť určena při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="23cd4-108">If you want your virtual machine to use a virtual network, make sure that you specify the virtual network when you create the virtual machine.</span></span>

> * <span data-ttu-id="23cd4-109">Dvě výhody používání virtuální sítě představují připojení přímo k virtuálnímu počítači a nastavení připojení mezi různými místy.</span><span class="sxs-lookup"><span data-stu-id="23cd4-109">Two benefits of using a virtual network are connecting directly to the virtual machine and to set up cross-premises connections.</span></span>

> * <span data-ttu-id="23cd4-110">Virtuální počítač je možné konfigurovat tak, aby se k virtuální síti připojil jen tehdy, když virtuální počítač vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="23cd4-110">A virtual machine can be configured to join a virtual network only when you create the virtual machine.</span></span> <span data-ttu-id="23cd4-111">Podrobnosti týkající se virtuálních sítí najdete v tématu [Přehled Azure Virtual Network](../articles/virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="23cd4-111">For details on virtual networks, see [Azure Virtual Network overview](../articles/virtual-network/virtual-networks-overview.md).</span></span>
>
>

## <a name="to-create-the-virtual-machine"></a><span data-ttu-id="23cd4-112">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="23cd4-112">To create the virtual machine</span></span>
