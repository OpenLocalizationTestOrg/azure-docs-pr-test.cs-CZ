


## <a name="tagging-a-virtual-machine-through-templates"></a><span data-ttu-id="43332-101">Označování virtuálního počítače prostřednictvím šablony</span><span class="sxs-lookup"><span data-stu-id="43332-101">Tagging a Virtual Machine through Templates</span></span>
<span data-ttu-id="43332-102">První Podíváme se na označování prostřednictvím šablon.</span><span class="sxs-lookup"><span data-stu-id="43332-102">First, let’s look at tagging through templates.</span></span> <span data-ttu-id="43332-103">[Tato šablona](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) umístí značky na hello následující prostředky: výpočetní (virtuálním počítači), úložiště (účet úložiště) a sítě (veřejnou IP adresu, virtuální sítě a síťové rozhraní).</span><span class="sxs-lookup"><span data-stu-id="43332-103">[This template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) places tags on hello following resources: Compute (Virtual Machine), Storage (Storage Account), and Network (Public IP Address, Virtual Network, and Network Interface).</span></span> <span data-ttu-id="43332-104">Tato šablona je pro virtuální počítač s Windows, ale můžete přizpůsobit pro virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="43332-104">This template is for a Windows VM but can be adapted for Linux VMs.</span></span>

<span data-ttu-id="43332-105">Klikněte na tlačítko hello **nasazení tooAzure** tlačítko z hello [šablony odkaz](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags).</span><span class="sxs-lookup"><span data-stu-id="43332-105">Click hello **Deploy tooAzure** button from hello [template link](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags).</span></span> <span data-ttu-id="43332-106">To bude přejděte toohello [portál Azure](https://portal.azure.com/) kde můžete nasadit tuto šablonu.</span><span class="sxs-lookup"><span data-stu-id="43332-106">This will navigate toohello [Azure portal](https://portal.azure.com/) where you can deploy this template.</span></span>

![Jednoduché nasazení pomocí značek](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

<span data-ttu-id="43332-108">Tato šablona zahrnuje hello následující značky: *oddělení*, *aplikace*, a *vytvořil*.</span><span class="sxs-lookup"><span data-stu-id="43332-108">This template includes hello following tags: *Department*, *Application*, and *Created By*.</span></span> <span data-ttu-id="43332-109">Vám může přidat či upravit tyto značky přímo v šabloně hello Pokud byste chtěli názvy různých značek.</span><span class="sxs-lookup"><span data-stu-id="43332-109">You can add/edit these tags directly in hello template if you would like different tag names.</span></span>

![Azure značky v šabloně](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

<span data-ttu-id="43332-111">Jak vidíte, hello značky jsou definovány jako páry klíč/hodnota, oddělené dvojtečkou (:).</span><span class="sxs-lookup"><span data-stu-id="43332-111">As you can see, hello tags are defined as key/value pairs, separated by a colon (:).</span></span> <span data-ttu-id="43332-112">Hello značky musí být definovány v tomto formátu:</span><span class="sxs-lookup"><span data-stu-id="43332-112">hello tags must be defined in this format:</span></span>

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

<span data-ttu-id="43332-113">Uložte soubor šablony hello po dokončení úprav s hello značkami, podle vašeho výběru.</span><span class="sxs-lookup"><span data-stu-id="43332-113">Save hello template file after you finish editing it with hello tags of your choice.</span></span>

<span data-ttu-id="43332-114">Vedle hello **upravit parametry** části můžete vyplnit hello hodnoty pro značek.</span><span class="sxs-lookup"><span data-stu-id="43332-114">Next, in hello **Edit Parameters** section, you can fill out hello values for your tags.</span></span>

![Úprava značek na portálu Azure](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

<span data-ttu-id="43332-116">Klikněte na tlačítko **vytvořit** toodeploy této šablony se vaše hodnoty značky.</span><span class="sxs-lookup"><span data-stu-id="43332-116">Click **Create** toodeploy this template with your tag values.</span></span>

## <a name="tagging-through-hello-portal"></a><span data-ttu-id="43332-117">Označování prostřednictvím hello portálu</span><span class="sxs-lookup"><span data-stu-id="43332-117">Tagging through hello Portal</span></span>
<span data-ttu-id="43332-118">Po vytvoření vaše prostředky pomocí značek, můžete zobrazit, přidat a odstranit značky hello portálu.</span><span class="sxs-lookup"><span data-stu-id="43332-118">After creating your resources with tags, you can view, add, and delete tags in hello portal.</span></span>

<span data-ttu-id="43332-119">Vyberte hello značky ikonu tooview značek:</span><span class="sxs-lookup"><span data-stu-id="43332-119">Select hello tags icon tooview your tags:</span></span>

![Ikona značky na portálu Azure](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

<span data-ttu-id="43332-121">Přidejte novou značku prostřednictvím portálu hello tak, že definujete vlastní dvojice klíč/hodnota a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="43332-121">Add a new tag through hello portal by defining your own Key/Value pair, and save it.</span></span>

![Přidat novou značku na portálu Azure](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

<span data-ttu-id="43332-123">Novou značku by se měla zobrazit v seznamu hello značky prostředku.</span><span class="sxs-lookup"><span data-stu-id="43332-123">Your new tag should now appear in hello list of tags for your resource.</span></span>

![Novou značku uložit na portálu Azure](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

