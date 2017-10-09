
* [<span data-ttu-id="6847b-101">Rychlé vytvoření virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="6847b-101">Quick-create a virtual machine in Azure</span></span>](#quick-create-a-vm-in-azure)
* [<span data-ttu-id="6847b-102">Nasazení virtuálního počítače v Azure ze šablony</span><span class="sxs-lookup"><span data-stu-id="6847b-102">Deploy a virtual machine in Azure from a template</span></span>](#deploy-a-vm-in-azure-from-a-template)
* [<span data-ttu-id="6847b-103">Vytvoření virtuálního počítače z vlastní image</span><span class="sxs-lookup"><span data-stu-id="6847b-103">Create a virtual machine from a custom image</span></span>](#create-a-custom-vm-image)
* [<span data-ttu-id="6847b-104">Nasazení virtuálního počítače, který používá virtuální síť a nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="6847b-104">Deploy a virtual machine that uses a virtual network and a load balancer</span></span>](#deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer)
* [<span data-ttu-id="6847b-105">Odebrání skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="6847b-105">Remove a resource group</span></span>](#remove-a-resource-group)
* [<span data-ttu-id="6847b-106">Zobrazit hello protokolu pro nasazení skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="6847b-106">Show hello log for a resource group deployment</span></span>](#show-the-log-for-a-resource-group-deployment)
* [<span data-ttu-id="6847b-107">Zobrazení informací o virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="6847b-107">Display information about a virtual machine</span></span>](#display-information-about-a-virtual-machine)
* [<span data-ttu-id="6847b-108">Připojit virtuální počítač založený na Linuxu tooa</span><span class="sxs-lookup"><span data-stu-id="6847b-108">Connect tooa Linux-based virtual machine</span></span>](#log-on-to-a-linux-based-virtual-machine)
* [<span data-ttu-id="6847b-109">Zastavení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6847b-109">Stop a virtual machine</span></span>](#stop-a-virtual-machine)
* [<span data-ttu-id="6847b-110">Spuštění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6847b-110">Start a virtual machine</span></span>](#start-a-virtual-machine)
* [<span data-ttu-id="6847b-111">Připojení datového disku</span><span class="sxs-lookup"><span data-stu-id="6847b-111">Attach a data disk</span></span>](#attach-a-data-disk)

## <a name="getting-ready"></a><span data-ttu-id="6847b-112">Příprava</span><span class="sxs-lookup"><span data-stu-id="6847b-112">Getting ready</span></span>
<span data-ttu-id="6847b-113">Před použitím hello rozhraní příkazového řádku Azure s skupin prostředků Azure, budete potřebovat toohave hello správnou verzi rozhraní příkazového řádku Azure a účet Azure.</span><span class="sxs-lookup"><span data-stu-id="6847b-113">Before you can use hello Azure CLI with Azure resource groups, you need toohave hello right Azure CLI version and an Azure account.</span></span> <span data-ttu-id="6847b-114">Pokud nemáte hello rozhraní příkazového řádku Azure, [ji nainstalovat](../articles/cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="6847b-114">If you don't have hello Azure CLI, [install it](../articles/cli-install-nodejs.md).</span></span>

### <a name="update-your-azure-cli-version-too090-or-later"></a><span data-ttu-id="6847b-115">Aktualizace vašeho too0.9.0 verze rozhraní příkazového řádku Azure nebo novější</span><span class="sxs-lookup"><span data-stu-id="6847b-115">Update your Azure CLI version too0.9.0 or later</span></span>
<span data-ttu-id="6847b-116">Typ `azure --version` toosee, zda jste již nainstalovali verzi 0.9.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6847b-116">Type `azure --version` toosee whether you have already installed version 0.9.0 or later.</span></span>

```azurecli
azure --version
0.9.0 (node: 0.10.25)
```

<span data-ttu-id="6847b-117">Pokud vaše verze není 0.9.0 nebo novější, je nutné tooupdate ji pomocí jedné z hello nativní instalační programy nebo pomocí **npm** zadáním `npm update -g azure-cli`.</span><span class="sxs-lookup"><span data-stu-id="6847b-117">If your version is not 0.9.0 or later, you need tooupdate it by using one of hello native installers or through **npm** by typing `npm update -g azure-cli`.</span></span>

<span data-ttu-id="6847b-118">Rozhraní příkazového řádku Azure můžete také spustit jako kontejner Docker pomocí následující hello [Docker image](https://registry.hub.docker.com/u/microsoft/azure-cli/).</span><span class="sxs-lookup"><span data-stu-id="6847b-118">You can also run Azure CLI as a Docker container by using hello following [Docker image](https://registry.hub.docker.com/u/microsoft/azure-cli/).</span></span> <span data-ttu-id="6847b-119">Z hostitele Docker spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="6847b-119">From a Docker host, run hello following command:</span></span>

```bash
docker run -it microsoft/azure-cli
```

### <a name="set-your-azure-account-and-subscription"></a><span data-ttu-id="6847b-120">Nastavení předplatného a účtu Azure</span><span class="sxs-lookup"><span data-stu-id="6847b-120">Set your Azure account and subscription</span></span>
<span data-ttu-id="6847b-121">Pokud ještě nemáte předplatné Azure, ale máte předplatné MSDN, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="6847b-121">If you don't already have an Azure subscription but you do have an MSDN subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span> <span data-ttu-id="6847b-122">Můžete si zaregistrovat i [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6847b-122">Or you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

<span data-ttu-id="6847b-123">Nyní [interaktivně přihlásit tooyour účet Azure](../articles/xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login) zadáním `azure login` a následující hello vyzve k tooyour prostředí interaktivní přihlašovací účet Azure.</span><span class="sxs-lookup"><span data-stu-id="6847b-123">Now [log in tooyour Azure account interactively](../articles/xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login) by typing `azure login` and following hello prompts for an interactive login experience tooyour Azure account.</span></span> 

> [!NOTE]
> <span data-ttu-id="6847b-124">Pokud máte pracovní nebo školní ID, a znát nemáte povoleno dvoufaktorové ověřování, můžete **také** použít `azure login -u` společně s hello pracovní nebo školní ID toolog v *bez* interaktivní relace.</span><span class="sxs-lookup"><span data-stu-id="6847b-124">If you have a work or school ID and you know you do not have two-factor authentication enabled, you can **also** use `azure login -u` along with hello work or school ID toolog in *without* an interactive session.</span></span> <span data-ttu-id="6847b-125">Pokud nemáte pracovní nebo školní ID, můžete [vytvořit pracovní nebo školní id z vašeho osobního účtu Microsoft](../articles/virtual-machines/windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toolog v hello stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="6847b-125">If you don't have a work or school ID, you can [create a work or school id from your personal Microsoft account](../articles/virtual-machines/windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toolog in hello same way.</span></span>
>
>

<span data-ttu-id="6847b-126">Váš účet může mít víc než jedno předplatné.</span><span class="sxs-lookup"><span data-stu-id="6847b-126">Your account may have more than one subscription.</span></span> <span data-ttu-id="6847b-127">Seznam předplatných můžete zobrazit zadáním `azure account list`. Výsledek bude vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="6847b-127">You can list your subscriptions by typing `azure account list`, which might look something like this:</span></span>

```azurecli
azure account list
info:    Executing command account list
data:    Name                              Id                                    Tenant Id                            Current
data:    --------------------------------  ------------------------------------  ------------------------------------  -------
data:    Contoso Admin                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  true
data:    Fabrikam dev                      xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Fabrikam test                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Contoso production                xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
```

<span data-ttu-id="6847b-128">Hello aktuální předplatné můžete nastavit tak, že zadáte následující hello.</span><span class="sxs-lookup"><span data-stu-id="6847b-128">You can set hello current Azure subscription by typing hello following.</span></span> <span data-ttu-id="6847b-129">Použijte hello název nebo hello ID předplatného obsahující hello prostředky, které chcete toomanage.</span><span class="sxs-lookup"><span data-stu-id="6847b-129">Use hello subscription name or hello ID that has hello resources you want toomanage.</span></span>

```azurecli
azure account set <subscription name or ID> true
```

### <a name="switch-toohello-azure-cli-resource-group-mode"></a><span data-ttu-id="6847b-130">Přepnout režim skupiny prostředků toohello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="6847b-130">Switch toohello Azure CLI resource group mode</span></span>
<span data-ttu-id="6847b-131">Ve výchozím nastavení spustí hello rozhraní příkazového řádku Azure v režimu správy služby hello (**asm** režim).</span><span class="sxs-lookup"><span data-stu-id="6847b-131">By default, hello Azure CLI starts in hello service management mode (**asm** mode).</span></span> <span data-ttu-id="6847b-132">Zadejte hello následující tooswitch tooresource skupiny režimu.</span><span class="sxs-lookup"><span data-stu-id="6847b-132">Type hello following tooswitch tooresource group mode.</span></span>

```azurecli
azure config mode arm
```

## <a name="understanding-azure-resource-templates-and-resource-groups"></a><span data-ttu-id="6847b-133">Principy skupin prostředků a šablon prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="6847b-133">Understanding Azure resource templates and resource groups</span></span>
<span data-ttu-id="6847b-134">Většina aplikací je vytvořená pomocí kombinace různých typů prostředků (jako je jeden nebo několik virtuálních počítačů a účtů úložiště, databáze SQL, virtuální síť nebo síť pro doručování obsahu).</span><span class="sxs-lookup"><span data-stu-id="6847b-134">Most applications are built from a combination of different resource types (such as one or more VMs and storage accounts, a SQL database, a virtual network, or a content delivery network).</span></span> <span data-ttu-id="6847b-135">Hello výchozí Azure service management API a hello portál Azure classic reprezentována tyto položky přístup pomocí služby.</span><span class="sxs-lookup"><span data-stu-id="6847b-135">hello default Azure service management API and hello Azure classic portal represented these items by using a service-by-service approach.</span></span> <span data-ttu-id="6847b-136">Tento postup vyžaduje, abyste toodeploy a spravovat jednotlivé služby hello jednotlivě (nebo najít jiné nástroje, které tak) a ne jako jednu logickou jednotku nasazení.</span><span class="sxs-lookup"><span data-stu-id="6847b-136">This approach requires you toodeploy and manage hello individual services individually (or find other tools that do so), and not as a single logical unit of deployment.</span></span>

<span data-ttu-id="6847b-137">*Šablony Azure Resource Manageru*, ale, díky kterému budete toodeploy a spravovat tyto různé prostředky jako jednu jednotku logické nasazení deklarativní způsobem.</span><span class="sxs-lookup"><span data-stu-id="6847b-137">*Azure Resource Manager templates*, however, make it possible for you toodeploy and manage these different resources as one logical deployment unit in a declarative fashion.</span></span> <span data-ttu-id="6847b-138">Místo imperativní informuje Azure co toodeploy jednoho příkazu za druhým, popisují celého nasazení v souboru JSON – všechny prostředky hello a přidružené konfigurace a nasazení parametry – a řekněte Azure toodeploy tyto prostředky jako jeden Skupina.</span><span class="sxs-lookup"><span data-stu-id="6847b-138">Instead of imperatively telling Azure what toodeploy one command after another, you describe your entire deployment in a JSON file -- all of hello resources and associated configuration and deployment parameters -- and tell Azure toodeploy those resources as one group.</span></span>

<span data-ttu-id="6847b-139">Pak můžete spravovat hello celkový životní cyklus hello skupiny prostředků pomocí příkazů správu prostředků Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="6847b-139">You can then manage hello overall life cycle of hello group's resources by using Azure CLI resource management commands to:</span></span>

* <span data-ttu-id="6847b-140">Zastavení, spuštění a odstranit všechny prostředky hello v rámci skupiny hello současně.</span><span class="sxs-lookup"><span data-stu-id="6847b-140">Stop, start, or delete all of hello resources within hello group at once.</span></span>
* <span data-ttu-id="6847b-141">Použití řízení přístupu na základě rolí (RBAC) pravidla toolock dolů oprávnění zabezpečení na ně.</span><span class="sxs-lookup"><span data-stu-id="6847b-141">Apply Role-Based Access Control (RBAC) rules toolock down security permissions on them.</span></span>
* <span data-ttu-id="6847b-142">Auditování operací.</span><span class="sxs-lookup"><span data-stu-id="6847b-142">Audit operations.</span></span>
* <span data-ttu-id="6847b-143">Označení prostředků dalšími metadaty pro lepší sledování.</span><span class="sxs-lookup"><span data-stu-id="6847b-143">Tag resources with additional metadata for better tracking.</span></span>

<span data-ttu-id="6847b-144">Další informace o skupin prostředků Azure a co dělají za vás v hello mnoha [přehled Azure Resource Manageru](../articles/azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6847b-144">You can learn lots more about Azure resource groups and what they can do for you in hello [Azure Resource Manager overview](../articles/azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="6847b-145">Pokud vás zajímá vytváření šablon, přečtěte si téma věnované [vytváření šablon Azure Resource Manageru](../articles/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="6847b-145">If you're interested in authoring templates, see [Authoring Azure Resource Manager templates](../articles/resource-group-authoring-templates.md).</span></span>

## <span data-ttu-id="6847b-146"><a id="quick-create-a-vm-in-azure"></a>Úkol: Rychlé vytvoření virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="6847b-146"><a id="quick-create-a-vm-in-azure"></a>Task: Quick-create a VM in Azure</span></span>
<span data-ttu-id="6847b-147">Někdy víte, jaké image, budete potřebovat, a teď potřebujete virtuálního počítače z této bitové kopie a vám nezáleží příliš mnoho hello infrastruktury – možná máte tootest něco čistou virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6847b-147">Sometimes you know what image you need, and you need a VM from that image right now and you don't care too much about hello infrastructure -- maybe you have tootest something on a clean VM.</span></span> <span data-ttu-id="6847b-148">Pokud je chcete toouse hello `azure vm quick-create` příkazů a předat nezbytné toocreate argumenty hello virtuálního počítače a jeho infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="6847b-148">That's when you want toouse hello `azure vm quick-create` command, and pass hello arguments necessary toocreate a VM and its infrastructure.</span></span>

<span data-ttu-id="6847b-149">Nejdřív vytvoříte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6847b-149">First, create your resource group.</span></span>

```azurecli
azure group create coreos-quick westus
info:    Executing command group create
+ Getting resource group coreos-quick
+ Creating resource group coreos-quick
info:    Created resource group coreos-quick
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick
data:    Name:                coreos-quick
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

<span data-ttu-id="6847b-150">Potom budete potřebovat image.</span><span class="sxs-lookup"><span data-stu-id="6847b-150">Second, you'll need an image.</span></span> <span data-ttu-id="6847b-151">toofind na bitovou kopii s hello Azure CLI, najdete v části [navigace a výběr imagí virtuálních počítačů Azure pomocí prostředí PowerShell a rozhraní příkazového řádku Azure hello](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6847b-151">toofind an image with hello Azure CLI, see [Navigating and selecting Azure virtual machine images with PowerShell and hello Azure CLI](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="6847b-152">Ale pro účely tohoto článku použijeme krátký seznam oblíbených imagí.</span><span class="sxs-lookup"><span data-stu-id="6847b-152">But for this article, here's a short list of popular images.</span></span> <span data-ttu-id="6847b-153">Pro toto rychlé vytvoření použijeme image Stable CoreOS.</span><span class="sxs-lookup"><span data-stu-id="6847b-153">We'll use CoreOS's Stable image for this quick-create.</span></span>

> [!NOTE]
> <span data-ttu-id="6847b-154">Pro ComputeImageVersion můžete taky jednoduše zadat 'nejnovější' jako hello parametr v obou jazyk hello šablony a hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="6847b-154">For ComputeImageVersion, you can also simply supply 'latest' as hello parameter in both hello template language and in hello Azure CLI.</span></span> <span data-ttu-id="6847b-155">To vám umožní, že tooalways používáte hello nejnovější a opravou verze hello bitové kopie bez nutnosti toomodify skripty nebo šablon.</span><span class="sxs-lookup"><span data-stu-id="6847b-155">This will allow you tooalways use hello latest and patched version of hello image without having toomodify your scripts or templates.</span></span> <span data-ttu-id="6847b-156">Příklad najdete níž.</span><span class="sxs-lookup"><span data-stu-id="6847b-156">This is shown below.</span></span>
>
>

| <span data-ttu-id="6847b-157">Název vydavatele</span><span class="sxs-lookup"><span data-stu-id="6847b-157">PublisherName</span></span> | <span data-ttu-id="6847b-158">Nabídka</span><span class="sxs-lookup"><span data-stu-id="6847b-158">Offer</span></span> | <span data-ttu-id="6847b-159">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="6847b-159">Sku</span></span> | <span data-ttu-id="6847b-160">Verze</span><span class="sxs-lookup"><span data-stu-id="6847b-160">Version</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="6847b-161">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="6847b-161">OpenLogic</span></span> |<span data-ttu-id="6847b-162">CentOS</span><span class="sxs-lookup"><span data-stu-id="6847b-162">CentOS</span></span> |<span data-ttu-id="6847b-163">7</span><span class="sxs-lookup"><span data-stu-id="6847b-163">7</span></span> |<span data-ttu-id="6847b-164">7.0.201503</span><span class="sxs-lookup"><span data-stu-id="6847b-164">7.0.201503</span></span> |
| <span data-ttu-id="6847b-165">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="6847b-165">OpenLogic</span></span> |<span data-ttu-id="6847b-166">CentOS</span><span class="sxs-lookup"><span data-stu-id="6847b-166">CentOS</span></span> |<span data-ttu-id="6847b-167">7.1</span><span class="sxs-lookup"><span data-stu-id="6847b-167">7.1</span></span> |<span data-ttu-id="6847b-168">7.1.201504</span><span class="sxs-lookup"><span data-stu-id="6847b-168">7.1.201504</span></span> |
| <span data-ttu-id="6847b-169">CoreOS</span><span class="sxs-lookup"><span data-stu-id="6847b-169">CoreOS</span></span> |<span data-ttu-id="6847b-170">CoreOS</span><span class="sxs-lookup"><span data-stu-id="6847b-170">CoreOS</span></span> |<span data-ttu-id="6847b-171">Beta</span><span class="sxs-lookup"><span data-stu-id="6847b-171">Beta</span></span> |<span data-ttu-id="6847b-172">647.0.0</span><span class="sxs-lookup"><span data-stu-id="6847b-172">647.0.0</span></span> |
| <span data-ttu-id="6847b-173">CoreOS</span><span class="sxs-lookup"><span data-stu-id="6847b-173">CoreOS</span></span> |<span data-ttu-id="6847b-174">CoreOS</span><span class="sxs-lookup"><span data-stu-id="6847b-174">CoreOS</span></span> |<span data-ttu-id="6847b-175">Stable</span><span class="sxs-lookup"><span data-stu-id="6847b-175">Stable</span></span> |<span data-ttu-id="6847b-176">633.1.0</span><span class="sxs-lookup"><span data-stu-id="6847b-176">633.1.0</span></span> |
| <span data-ttu-id="6847b-177">MicrosoftDynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="6847b-177">MicrosoftDynamicsNAV</span></span> |<span data-ttu-id="6847b-178">DynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="6847b-178">DynamicsNAV</span></span> |<span data-ttu-id="6847b-179">2015</span><span class="sxs-lookup"><span data-stu-id="6847b-179">2015</span></span> |<span data-ttu-id="6847b-180">8.0.40459</span><span class="sxs-lookup"><span data-stu-id="6847b-180">8.0.40459</span></span> |
| <span data-ttu-id="6847b-181">MicrosoftSharePoint</span><span class="sxs-lookup"><span data-stu-id="6847b-181">MicrosoftSharePoint</span></span> |<span data-ttu-id="6847b-182">MicrosoftSharePointServer</span><span class="sxs-lookup"><span data-stu-id="6847b-182">MicrosoftSharePointServer</span></span> |<span data-ttu-id="6847b-183">2013</span><span class="sxs-lookup"><span data-stu-id="6847b-183">2013</span></span> |<span data-ttu-id="6847b-184">1.0.0</span><span class="sxs-lookup"><span data-stu-id="6847b-184">1.0.0</span></span> |
| <span data-ttu-id="6847b-185">msopentech</span><span class="sxs-lookup"><span data-stu-id="6847b-185">msopentech</span></span> |<span data-ttu-id="6847b-186">Oracle-Database-12c-Weblogic-Server-12c</span><span class="sxs-lookup"><span data-stu-id="6847b-186">Oracle-Database-12c-Weblogic-Server-12c</span></span> |<span data-ttu-id="6847b-187">Standard</span><span class="sxs-lookup"><span data-stu-id="6847b-187">Standard</span></span> |<span data-ttu-id="6847b-188">1.0.0</span><span class="sxs-lookup"><span data-stu-id="6847b-188">1.0.0</span></span> |
| <span data-ttu-id="6847b-189">msopentech</span><span class="sxs-lookup"><span data-stu-id="6847b-189">msopentech</span></span> |<span data-ttu-id="6847b-190">Oracle-Database-12c-Weblogic-Server-12c</span><span class="sxs-lookup"><span data-stu-id="6847b-190">Oracle-Database-12c-Weblogic-Server-12c</span></span> |<span data-ttu-id="6847b-191">Enterprise</span><span class="sxs-lookup"><span data-stu-id="6847b-191">Enterprise</span></span> |<span data-ttu-id="6847b-192">1.0.0</span><span class="sxs-lookup"><span data-stu-id="6847b-192">1.0.0</span></span> |
| <span data-ttu-id="6847b-193">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="6847b-193">MicrosoftSQLServer</span></span> |<span data-ttu-id="6847b-194">SQL2014-WS2012R2</span><span class="sxs-lookup"><span data-stu-id="6847b-194">SQL2014-WS2012R2</span></span> |<span data-ttu-id="6847b-195">Enterprise-Optimized-for-DW</span><span class="sxs-lookup"><span data-stu-id="6847b-195">Enterprise-Optimized-for-DW</span></span> |<span data-ttu-id="6847b-196">12.0.2430</span><span class="sxs-lookup"><span data-stu-id="6847b-196">12.0.2430</span></span> |
| <span data-ttu-id="6847b-197">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="6847b-197">MicrosoftSQLServer</span></span> |<span data-ttu-id="6847b-198">SQL2014-WS2012R2</span><span class="sxs-lookup"><span data-stu-id="6847b-198">SQL2014-WS2012R2</span></span> |<span data-ttu-id="6847b-199">Enterprise-Optimized-for-OLTP</span><span class="sxs-lookup"><span data-stu-id="6847b-199">Enterprise-Optimized-for-OLTP</span></span> |<span data-ttu-id="6847b-200">12.0.2430</span><span class="sxs-lookup"><span data-stu-id="6847b-200">12.0.2430</span></span> |
| <span data-ttu-id="6847b-201">Canonical</span><span class="sxs-lookup"><span data-stu-id="6847b-201">Canonical</span></span> |<span data-ttu-id="6847b-202">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="6847b-202">UbuntuServer</span></span> |<span data-ttu-id="6847b-203">12.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="6847b-203">12.04.5-LTS</span></span> |<span data-ttu-id="6847b-204">12.04.201504230</span><span class="sxs-lookup"><span data-stu-id="6847b-204">12.04.201504230</span></span> |
| <span data-ttu-id="6847b-205">Canonical</span><span class="sxs-lookup"><span data-stu-id="6847b-205">Canonical</span></span> |<span data-ttu-id="6847b-206">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="6847b-206">UbuntuServer</span></span> |<span data-ttu-id="6847b-207">14.04.2-LTS</span><span class="sxs-lookup"><span data-stu-id="6847b-207">14.04.2-LTS</span></span> |<span data-ttu-id="6847b-208">14.04.201503090</span><span class="sxs-lookup"><span data-stu-id="6847b-208">14.04.201503090</span></span> |
| <span data-ttu-id="6847b-209">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="6847b-209">MicrosoftWindowsServer</span></span> |<span data-ttu-id="6847b-210">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="6847b-210">WindowsServer</span></span> |<span data-ttu-id="6847b-211">2012-Datacenter</span><span class="sxs-lookup"><span data-stu-id="6847b-211">2012-Datacenter</span></span> |<span data-ttu-id="6847b-212">3.0.201503</span><span class="sxs-lookup"><span data-stu-id="6847b-212">3.0.201503</span></span> |
| <span data-ttu-id="6847b-213">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="6847b-213">MicrosoftWindowsServer</span></span> |<span data-ttu-id="6847b-214">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="6847b-214">WindowsServer</span></span> |<span data-ttu-id="6847b-215">2012-R2-Datacenter</span><span class="sxs-lookup"><span data-stu-id="6847b-215">2012-R2-Datacenter</span></span> |<span data-ttu-id="6847b-216">4.0.201503</span><span class="sxs-lookup"><span data-stu-id="6847b-216">4.0.201503</span></span> |
| <span data-ttu-id="6847b-217">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="6847b-217">MicrosoftWindowsServer</span></span> |<span data-ttu-id="6847b-218">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="6847b-218">WindowsServer</span></span> |<span data-ttu-id="6847b-219">Windows-Server-Technical-Preview</span><span class="sxs-lookup"><span data-stu-id="6847b-219">Windows-Server-Technical-Preview</span></span> |<span data-ttu-id="6847b-220">5.0.201504</span><span class="sxs-lookup"><span data-stu-id="6847b-220">5.0.201504</span></span> |
| <span data-ttu-id="6847b-221">MicrosoftWindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="6847b-221">MicrosoftWindowsServerEssentials</span></span> |<span data-ttu-id="6847b-222">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="6847b-222">WindowsServerEssentials</span></span> |<span data-ttu-id="6847b-223">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="6847b-223">WindowsServerEssentials</span></span> |<span data-ttu-id="6847b-224">1.0.141204</span><span class="sxs-lookup"><span data-stu-id="6847b-224">1.0.141204</span></span> |
| <span data-ttu-id="6847b-225">MicrosoftWindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="6847b-225">MicrosoftWindowsServerHPCPack</span></span> |<span data-ttu-id="6847b-226">WindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="6847b-226">WindowsServerHPCPack</span></span> |<span data-ttu-id="6847b-227">2012R2</span><span class="sxs-lookup"><span data-stu-id="6847b-227">2012R2</span></span> |<span data-ttu-id="6847b-228">4.3.4665</span><span class="sxs-lookup"><span data-stu-id="6847b-228">4.3.4665</span></span> |

<span data-ttu-id="6847b-229">Právě vytvoření virtuálního počítače tak, že zadáte hello `azure vm quick-create` příkaz a je připravený pro hello výzvy.</span><span class="sxs-lookup"><span data-stu-id="6847b-229">Just create your VM by entering hello `azure vm quick-create` command and being ready for hello prompts.</span></span> <span data-ttu-id="6847b-230">Výsledek by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="6847b-230">It should look something like this:</span></span>

```azurecli
azure vm quick-create
info:    Executing command vm quick-create
Resource group name: coreos-quick
Virtual machine name: coreos
Location name: westus
Operating system Type [Windows, Linux]: linux
ImageURN (format: "publisherName:offer:skus:version"): coreos:coreos:stable:latest
User name: ops
Password: *********
Confirm password: *********
+ Looking up hello VM "coreos"
info:    Using hello VM Size "Standard_A1"
info:    hello [OS, Data] Disk or image configuration requires storage account
+ Retrieving storage accounts
info:    Could not find any storage accounts in hello region "westus", trying toocreate new one
+ Creating storage account "cli9fd3fce49e9a9b3d14302" in "westus"
+ Looking up hello storage account cli9fd3fce49e9a9b3d14302
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
info:    An nic with given name "coreo-westu-1430261891570-nic" not found, creating a new one
+ Looking up hello virtual network "coreo-westu-1430261891570-vnet"
info:    Preparing toocreate new virtual network and subnet
/ Creating a new virtual network "coreo-westu-1430261891570-vnet" [address prefix: "10.0.0.0/16"] with subnet "coreo-westu-1430261891570-sne+" [address prefix: "10.0.1.0/24"]
+ Looking up hello virtual network "coreo-westu-1430261891570-vnet"
+ Looking up hello subnet "coreo-westu-1430261891570-snet" under hello virtual network "coreo-westu-1430261891570-vnet"
info:    Found public ip parameters, trying toosetup PublicIP profile
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
info:    PublicIP with given name "coreo-westu-1430261891570-pip" not found, creating a new one
+ Creating public ip "coreo-westu-1430261891570-pip"
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
+ Creating NIC "coreo-westu-1430261891570-nic"
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
+ Creating VM "coreos"
+ Looking up hello VM "coreos"
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Compute/virtualMachines/coreos
data:    ProvisioningState               :Succeeded
data:    Name                            :coreos
data:    Location                        :westus
data:    FQDN                            :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :coreos
data:        Offer                       :coreos
data:        Sku                         :stable
data:        Version                     :633.1.0
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :cli9fd3fce49e9a9b3d-os-1430261892283
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli9fd3fce49e9a9b3d14302.blob.core.windows.net/vhds/cli9fd3fce49e9a9b3d-os-1430261892283.vhd
data:
data:    OS Profile:
data:      Computer Name                 :coreos
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :false
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Network/networkInterfaces/coreo-westu-1430261891570-nic
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-30-72-E3
data:          Provisioning State        :Succeeded
data:          Name                      :coreo-westu-1430261891570-nic
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.1.4
data:            Public IP address       :104.40.24.124
data:            FQDN                    :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
info:    vm quick-create command OK
```

<span data-ttu-id="6847b-231">A váš nový virtuální počítač je připravený.</span><span class="sxs-lookup"><span data-stu-id="6847b-231">And away you go with your new VM.</span></span>

## <span data-ttu-id="6847b-232"><a id="deploy-a-vm-in-azure-from-a-template"></a>Úkol: Nasazení virtuálního počítače v Azure ze šablony</span><span class="sxs-lookup"><span data-stu-id="6847b-232"><a id="deploy-a-vm-in-azure-from-a-template"></a>Task: Deploy a VM in Azure from a template</span></span>
<span data-ttu-id="6847b-233">Použijte hello pokyny v těchto částech toodeploy nového virtuálního počítače Azure pomocí šablony s hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="6847b-233">Use hello instructions in these sections toodeploy a new Azure VM by using a template with hello Azure CLI.</span></span> <span data-ttu-id="6847b-234">Tato šablona vytvoří jeden virtuální počítač v nové virtuální sítě s jedinou podsítí a na rozdíl od `azure vm quick-create`, umožňuje toodescribe jste, co chcete přesněji a opakujte bez chyb.</span><span class="sxs-lookup"><span data-stu-id="6847b-234">This template creates a single virtual machine in a new virtual network with a single subnet, and unlike `azure vm quick-create`, enables you toodescribe what you want precisely and repeat it without errors.</span></span> <span data-ttu-id="6847b-235">Tato šablona vytvoří tohle:</span><span class="sxs-lookup"><span data-stu-id="6847b-235">Here's what this template creates:</span></span>

![](./media/virtual-machines-common-cli-deploy-templates/new-vm.png)

### <a name="step-1-examine-hello-json-file-for-hello-template-parameters"></a><span data-ttu-id="6847b-236">Krok 1: Zkontrolujte hello JSON v souboru parametrů šablony hello</span><span class="sxs-lookup"><span data-stu-id="6847b-236">Step 1: Examine hello JSON file for hello template parameters</span></span>
<span data-ttu-id="6847b-237">Tady jsou hello obsah souboru JSON hello hello šablony.</span><span class="sxs-lookup"><span data-stu-id="6847b-237">Here are hello contents of hello JSON file for hello template.</span></span> <span data-ttu-id="6847b-238">(hello šablona se také nachází v [Githubu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json).)</span><span class="sxs-lookup"><span data-stu-id="6847b-238">(hello template is also located in [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json).)</span></span>

<span data-ttu-id="6847b-239">Šablony jsou flexibilní, proto hello Návrhář může mít vybrali toogive velké množství parametry nebo vybrali toooffer jen několik tak, že vytvoříte šablonu, která více vyřešen.</span><span class="sxs-lookup"><span data-stu-id="6847b-239">Templates are flexible, so hello designer may have chosen toogive you lots of parameters or chosen toooffer only a few by creating a template that is more fixed.</span></span> <span data-ttu-id="6847b-240">V pořadí toocollect hello informace, které potřebujete toopass hello šablony jako parametry, otevřete soubor šablony hello (Toto téma obsahuje vložené šablony níže) a zkontrolujte hello **parametry** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6847b-240">In order toocollect hello information you need toopass hello template as parameters, open hello template file (this topic has a template inline, below) and examine hello **parameters** values.</span></span>

<span data-ttu-id="6847b-241">V takovém případě níže uvedená šablona hello požádá pro:</span><span class="sxs-lookup"><span data-stu-id="6847b-241">In this case, hello template below will ask for:</span></span>

* <span data-ttu-id="6847b-242">Jedinečný název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="6847b-242">A unique storage account name.</span></span>
* <span data-ttu-id="6847b-243">Uživatelské jméno hello virtuálních počítačů správce.</span><span class="sxs-lookup"><span data-stu-id="6847b-243">An admin user name for hello VM.</span></span>
* <span data-ttu-id="6847b-244">Heslo.</span><span class="sxs-lookup"><span data-stu-id="6847b-244">A password.</span></span>
* <span data-ttu-id="6847b-245">Název domény pro hello mimo world toouse.</span><span class="sxs-lookup"><span data-stu-id="6847b-245">A domain name for hello outside world toouse.</span></span>
* <span data-ttu-id="6847b-246">Číslo verze Ubuntu Serveru (ale přijme jenom jednu z hodnot v seznamu).</span><span class="sxs-lookup"><span data-stu-id="6847b-246">An Ubuntu Server version number -- but it will accept only one of a list.</span></span>

<span data-ttu-id="6847b-247">Přečtěte si další informace o [požadavcích na uživatelské jméno a heslo](../articles/virtual-machines/linux/faq.md#what-are-the-username-requirements-when-creating-a-vm).</span><span class="sxs-lookup"><span data-stu-id="6847b-247">See more about [username and password requirements](../articles/virtual-machines/linux/faq.md#what-are-the-username-requirements-when-creating-a-vm).</span></span>

<span data-ttu-id="6847b-248">Jakmile se rozhodnete tyto hodnoty jste skupinu pro připravený toocreate a nasazení této šablony do vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="6847b-248">Once you decide on these values, you're ready toocreate a group for and deploy this template into your Azure subscription.</span></span>

```json
{
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": {
    "newStorageAccountName": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for hello storage account where hello virtual machine's disks will be placed."
    }
    },
    "adminUsername": {
    "type": "string",
    "metadata": {
        "description": "User name for hello virtual machine."
    }
    },
    "adminPassword": {
    "type": "securestring",
    "metadata": {
        "description": "Password for hello virtual machine."
    }
    },
    "dnsNameForPublicIP": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for hello public IP used tooaccess hello virtual machine."
    }
    },
    "ubuntuOSVersion": {
    "type": "string",
    "defaultValue": "14.04.2-LTS",
    "allowedValues": [
        "12.04.5-LTS",
        "14.04.2-LTS",
        "15.04"
    ],
    "metadata": {
        "description": "hello Ubuntu version for hello VM. This will pick a fully patched image of this given Ubuntu version. Allowed values: 12.04.5-LTS, 14.04.2-LTS, 15.04."
    }
    }
},
"variables": {
    "location": "West US",
    "imagePublisher": "Canonical",
    "imageOffer": "UbuntuServer",
    "OSDiskName": "osdiskforlinuxsimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyUbuntuVM",
    "vmSize": "Standard_D1",
    "virtualNetworkName": "MyVNET",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
},
"resources": [
    {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('newStorageAccountName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[variables('location')]",
    "properties": {
        "accountType": "[variables('storageAccountType')]"
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[variables('publicIPAddressName')]",
    "location": "[variables('location')]",
    "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
        "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
        }
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
    "location": "[variables('location')]",
    "properties": {
        "addressSpace": {
        "addressPrefixes": [
            "[variables('addressPrefix')]"
        ]
        },
        "subnets": [
        {
            "name": "[variables('subnetName')]",
            "properties": {
            "addressPrefix": "[variables('subnetPrefix')]"
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('nicName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
    ],
    "properties": {
        "ipConfigurations": [
        {
            "name": "ipconfig1",
            "properties": {
            "privateIPAllocationMethod": "Dynamic",
            "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
            },
            "subnet": {
                "id": "[variables('subnetRef')]"
            }
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', parameters('newStorageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
        "computername": "[variables('vmName')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
        "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('ubuntuOSVersion')]",
            "version": "latest"
        },
        "osDisk": {
            "name": "osdisk",
            "vhd": {
            "uri": "[concat('http://',parameters('newStorageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
        }
        },
        "networkProfile": {
        "networkInterfaces": [
            {
            "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
        ]
        }
    }
    }
]
}
```

### <a name="step-2-create-hello-virtual-machine-by-using-hello-template"></a><span data-ttu-id="6847b-249">Krok 2: Vytvoření hello virtuálního počítače pomocí šablony hello</span><span class="sxs-lookup"><span data-stu-id="6847b-249">Step 2: Create hello virtual machine by using hello template</span></span>
<span data-ttu-id="6847b-250">Až budete mít vaše hodnoty parametrů připraven, musíte vytvořit skupinu prostředků pro šablonu nasazení a potom nasaďte šablonu hello.</span><span class="sxs-lookup"><span data-stu-id="6847b-250">Once you have your parameter values ready, you must create a resource group for your template deployment and then deploy hello template.</span></span>

<span data-ttu-id="6847b-251">Skupina prostředků toocreate hello, typ `azure group create <group name> <location>` s názvem hello hello skupiny chcete a hello datacenter umístění, do kterého chcete toodeploy.</span><span class="sxs-lookup"><span data-stu-id="6847b-251">toocreate hello resource group, type `azure group create <group name> <location>` with hello name of hello group you want and hello datacenter location into which you want toodeploy.</span></span> <span data-ttu-id="6847b-252">Akce proběhne rychle:</span><span class="sxs-lookup"><span data-stu-id="6847b-252">This happens quickly:</span></span>

```azurecli
azure group create myResourceGroup westus
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

<span data-ttu-id="6847b-253">Nyní toocreate hello nasazení, volání `azure group deployment create` a předejte:</span><span class="sxs-lookup"><span data-stu-id="6847b-253">Now toocreate hello deployment, call `azure group deployment create` and pass:</span></span>

* <span data-ttu-id="6847b-254">soubor šablony Hello (Pokud jste uložili hello výše JSON šablony tooa místního souboru).</span><span class="sxs-lookup"><span data-stu-id="6847b-254">hello template file (if you saved hello above JSON template tooa local file).</span></span>
* <span data-ttu-id="6847b-255">Šablona identifikátor URI (v případě potřeby toopoint v souboru hello v Githubu nebo některých jiných webovou adresu).</span><span class="sxs-lookup"><span data-stu-id="6847b-255">A template URI (if you want toopoint at hello file in GitHub or some other web address).</span></span>
* <span data-ttu-id="6847b-256">Skupina prostředků Hello, do kterého chcete toodeploy.</span><span class="sxs-lookup"><span data-stu-id="6847b-256">hello resource group into which you want toodeploy.</span></span>
* <span data-ttu-id="6847b-257">Volitelný název nasazení.</span><span class="sxs-lookup"><span data-stu-id="6847b-257">An optional deployment name.</span></span>

<span data-ttu-id="6847b-258">Bude výzvami toosupply hello hodnoty parametrů v části "parametry" hello souboru JSON hello.</span><span class="sxs-lookup"><span data-stu-id="6847b-258">You will be prompted toosupply hello values of parameters in hello "parameters" section of hello JSON file.</span></span> <span data-ttu-id="6847b-259">Pokud jste zadali hodnoty parametrů všechny hello, bude zahájena vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="6847b-259">When you have specified all hello parameter values, your deployment will begin.</span></span>

<span data-ttu-id="6847b-260">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="6847b-260">Here is an example:</span></span>

```azurecli
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json myResourceGroup firstDeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
newStorageAccountName: storageaccount
adminUsername: ops
adminPassword: password
dnsNameForPublicIP: newdomainname
```

<span data-ttu-id="6847b-261">Zobrazí hello následující typy informací:</span><span class="sxs-lookup"><span data-stu-id="6847b-261">You will receive hello following type of information:</span></span>

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "firstDeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment toocomplete
data:    DeploymentName     : firstDeployment
data:    ResourceGroupName  : myResourceGroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T07:53:55.1828878Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  -------------
data:    newStorageAccountName  String        storageaccount
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameForPublicIP     String        newdomainname
data:    ubuntuOSVersion        String        14.10
info:    group deployment create command OK
```


## <span data-ttu-id="6847b-262"><a id="create-a-custom-vm-image"></a>Úkol: Vytvoření vlastní image virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6847b-262"><a id="create-a-custom-vm-image"></a>Task: Create a custom VM image</span></span>
<span data-ttu-id="6847b-263">Jste se seznámili s hello základní použití šablony výše, takže teď můžeme použít podobné pokyny toocreate vlastní virtuální počítač ze souboru konkrétní VHD v Azure pomocí šablony prostřednictvím hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="6847b-263">You've seen hello basic usage of templates above, so now we can use similar instructions toocreate a custom VM from a specific .vhd file in Azure by using a template via hello Azure CLI.</span></span> <span data-ttu-id="6847b-264">Hello rozdíl je, že tato šablona vytvoří jeden virtuální počítač z zadaný virtuální pevný disk (VHD).</span><span class="sxs-lookup"><span data-stu-id="6847b-264">hello difference here is that this template creates a single virtual machine from a specified virtual hard disk (VHD).</span></span>

### <a name="step-1-examine-hello-json-file-for-hello-template"></a><span data-ttu-id="6847b-265">Krok 1: Zkontrolujte hello JSON v souboru šablony hello</span><span class="sxs-lookup"><span data-stu-id="6847b-265">Step 1: Examine hello JSON file for hello template</span></span>
<span data-ttu-id="6847b-266">Tady jsou hello obsah souboru JSON hello hello šablony, která v této části se používá jako příklad.</span><span class="sxs-lookup"><span data-stu-id="6847b-266">Here are hello contents of hello JSON file for hello template that this section uses as an example.</span></span> <span data-ttu-id="6847b-267">(hello šablona se také nachází v [Githubu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).)</span><span class="sxs-lookup"><span data-stu-id="6847b-267">(hello template is also located in [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).)</span></span>

<span data-ttu-id="6847b-268">Znovu budete potřebovat toofind hello hodnoty, které mají tooenter pro hello parametry, které nemají výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6847b-268">Again, you will need toofind hello values you want tooenter for hello parameters that do not have default values.</span></span> <span data-ttu-id="6847b-269">Když spustíte hello `azure group deployment create` příkaz hello příkazového řádku Azure CLI vás vyzve tooenter můžete tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6847b-269">When you run hello `azure group deployment create` command, hello Azure CLI will prompt you tooenter those values.</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters" : {
        "userImageStorageAccountName": {
            "type": "string",
            "defaultValue" : "userImageStorageAccountName"
        },
        "userImageStorageContainerName" : {
            "type" : "string",
            "defaultValue" : "userImageStorageContainerName"
        },
        "userImageVhdName" : {
            "type" : "string",
            "defaultValue" : "userImageVhdName"
        },
        "dnsNameForPublicIP" : {
            "type" : "string",
            "defaultValue": "uniqueDnsNameForPublicIP"
        },
        "adminUserName": {
            "type": "string"
        },
        "adminPassword": {
            "type": "securestring"
        },
        "osType" : {
            "type" : "string",
            "allowedValues" : [
                "windows",
                "linux"
            ]
        },
        "subscriptionId":{
            "type" : "string"
        },
        "location": {
            "type": "String",
            "defaultValue" : "West US"
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A2"
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue" : "myPublicIP"
        },
        "vmName": {
            "type": "string",
            "defaultValue" : "myVM"
        },
        "virtualNetworkName":{
            "type" : "string",
            "defaultValue" : "myVNET"
        },
        "nicName":{
            "type" : "string",
            "defaultValue":"myNIC"
        }
    },
    "variables": {
        "addressPrefix":"10.0.0.0/16",
        "subnet1Name": "Subnet-1",
        "subnet2Name": "Subnet-2",
        "subnet1Prefix" : "10.0.0.0/24",
        "subnet2Prefix" : "10.0.1.0/24",
        "publicIPAddressType" : "Dynamic",
        "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
        "subnet1Ref" : "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
        "userImageName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('userImageVhdName'))]",
        "osDiskVhdName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
    },
    "resources": [
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[parameters('publicIPAddressName')]",
        "location": "[parameters('location')]",
        "properties": {
            "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
            }
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/virtualNetworks",
        "name": "[parameters('virtualNetworkName')]",
        "location": "[parameters('location')]",
        "properties": {
        "addressSpace": {
            "addressPrefixes": [
            "[variables('addressPrefix')]"
            ]
        },
        "subnets": [
            {
            "name": "[variables('subnet1Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet1Prefix')]"
            }
            },
            {
            "name": "[variables('subnet2Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet2Prefix')]"
            }
            }
        ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[parameters('nicName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
            "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
        ],
        "properties": {
            "ipConfigurations": [
            {
                "name": "ipconfig1",
                "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                        "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                    },
                    "subnet": {
                        "id": "[variables('subnet1Ref')]"
                    }
                }
            }
            ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[parameters('vmName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
        ],
        "properties": {
            "hardwareProfile": {
                "vmSize": "[parameters('vmSize')]"
            },
            "osProfile": {
                "computername": "[parameters('vmName')]",
                "adminUsername": "[parameters('adminUsername')]",
                "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
                "osDisk" : {
                    "name" : "[concat(parameters('vmName'),'-osDisk')]",
                    "osType" : "[parameters('osType')]",
                    "caching" : "ReadWrite",
                    "image" : {
                        "uri" : "[variables('userImageName')]"
                    },
                    "vhd" : {
                        "uri" : "[variables('osDiskVhdName')]"
                    }
                }
            },
            "networkProfile": {
                "networkInterfaces" : [
                {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                }
                ]
            }
        }
    }
    ]
}
```

### <a name="step-2-obtain-hello-vhd"></a><span data-ttu-id="6847b-270">Krok 2: Získání hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="6847b-270">Step 2: Obtain hello VHD</span></span>
<span data-ttu-id="6847b-271">Pro tyto účely budete samozřejmě potřebovat soubor .vhd.</span><span class="sxs-lookup"><span data-stu-id="6847b-271">Obviously, you'll need a .vhd for this.</span></span> <span data-ttu-id="6847b-272">Můžete použít ten, který už máte v Azure, nebo nějaký nahrát.</span><span class="sxs-lookup"><span data-stu-id="6847b-272">You can use one you already have in Azure, or you can upload one.</span></span>

<span data-ttu-id="6847b-273">Virtuální počítač systému Windows, najdete v části [vytvoření a nahrání virtuálního pevného disku serveru Windows tooAzure](../articles/virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6847b-273">For a Windows-based virtual machine, see [Create and upload a Windows Server VHD tooAzure](../articles/virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="6847b-274">Pro virtuální počítač založený na Linuxu, viz [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux hello](../articles/virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6847b-274">For a Linux-based virtual machine, see [Creating and uploading a virtual hard disk that contains hello Linux operating system](../articles/virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

### <a name="step-3-create-hello-virtual-machine-by-using-hello-template"></a><span data-ttu-id="6847b-275">Krok 3: Vytvoření hello virtuálního počítače pomocí šablony hello</span><span class="sxs-lookup"><span data-stu-id="6847b-275">Step 3: Create hello virtual machine by using hello template</span></span>
<span data-ttu-id="6847b-276">Nyní jste připravené toocreate nového virtuálního počítače podle hello VHD.</span><span class="sxs-lookup"><span data-stu-id="6847b-276">Now you're ready toocreate a new virtual machine based on hello .vhd.</span></span> <span data-ttu-id="6847b-277">Vytvoření skupiny toodeploy do, pomocí `azure group create <location>`:</span><span class="sxs-lookup"><span data-stu-id="6847b-277">Create a group toodeploy into, by using `azure group create <location>`:</span></span>

```azurecli
azure group create myResourceGroupUser eastus
info:    Executing command group create
+ Getting resource group myResourceGroupUser
+ Creating resource group myResourceGroupUser
info:    Created resource group myResourceGroupUser
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupUser
data:    Name:                myResourceGroupUser
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

<span data-ttu-id="6847b-278">Pak vytvořte hello nasazení pomocí hello `--template-uri` toocall možnost v šabloně hello přímo (nebo můžete použít hello `--template-file` toouse možnost soubor, který jste uložili místně).</span><span class="sxs-lookup"><span data-stu-id="6847b-278">Then create hello deployment by using hello `--template-uri` option toocall in hello template directly (or you can use hello `--template-file` option toouse a file that you have saved locally).</span></span> <span data-ttu-id="6847b-279">Všimněte si, že protože hello šablony má výchozí hodnoty zadané, budete vyzváni k jenom pár věcí.</span><span class="sxs-lookup"><span data-stu-id="6847b-279">Note that because hello template has defaults specified, you are prompted for only a few things.</span></span> <span data-ttu-id="6847b-280">Pokud nasadíte hello šablony na různých místech, můžete zjistit, že některé pojmenování kolizí ke kterým dochází u hello výchozí hodnoty (zejména hello název DNS, které vytvoříte).</span><span class="sxs-lookup"><span data-stu-id="6847b-280">If you deploy hello template in different places, you may find that some naming collisions occur with hello default values (particularly hello DNS name you create).</span></span>

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json \
> myResourceGroup \
> customVhdDeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
adminUserName: ops
adminPassword: password
osType: linux
subscriptionId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

<span data-ttu-id="6847b-281">Výstup vypadá podobně jako následující hello:</span><span class="sxs-lookup"><span data-stu-id="6847b-281">Output looks something like hello following:</span></span>

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "customVhdDeployment"
+ Registering providers
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment toocomplete
error:   Deployment provisioning state was not successful
data:    DeploymentName     : customVhdDeployment
data:    ResourceGroupName  : myResourceGroupUser
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T14:55:48.0963829Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                           Type          Value
data:    -----------------------------  ------------  ------------------------------------
data:    userImageStorageAccountName    String        userImageStorageAccountName
data:    userImageStorageContainerName  String        userImageStorageContainerName
data:    userImageVhdName               String        userImageVhdName
data:    dnsNameForPublicIP             String        uniqueDnsNameForPublicIP
data:    adminUserName                  String        ops
data:    adminPassword                  SecureString  undefined
data:    osType                         String        linux
data:    subscriptionId                 String        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
data:    location                       String        West US
data:    vmSize                         String        Standard_A2
data:    publicIPAddressName            String        myPublicIP
data:    vmName                         String        myVM
data:    virtualNetworkName             String        myVNET
data:    nicName                        String        myNIC
info:    group deployment create command OK
```

## <span data-ttu-id="6847b-282"><a id="deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a>Úloha: Nasazení aplikace s více virtuálními počítači, která používá virtuální síť a externí nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="6847b-282"><a id="deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a>Task: Deploy a multi-VM application that uses a virtual network and an external load balancer</span></span>
<span data-ttu-id="6847b-283">Tato šablona vám umožní toocreate dva virtuální počítače pod nástrojem pro vyrovnávání zatížení a nakonfigurujte pravidlo Vyrovnávání zatížení na Port 80.</span><span class="sxs-lookup"><span data-stu-id="6847b-283">This template allows you toocreate two virtual machines under a load balancer and configure a load-balancing rule on Port 80.</span></span> <span data-ttu-id="6847b-284">Tato šablona také nasadí účet úložiště, virtuální síť, veřejnou IP adresu, skupinu dostupnosti a síťová rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6847b-284">This template also deploys a storage account, virtual network, public IP address, availability set, and network interfaces.</span></span>

![](./media/virtual-machines-common-cli-deploy-templates/multivmextlb.png)

<span data-ttu-id="6847b-285">Postupujte podle těchto kroků toodeploy aplikace více virtuálních počítačů, která používá virtuální síť a nástroj pro vyrovnávání zatížení pomocí šablony Resource Manageru v šabloně úložiště GitHub hello pomocí příkazů prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6847b-285">Follow these steps toodeploy a multi-VM application that uses a virtual network and a load balancer by using a Resource Manager template in hello GitHub template repository via Azure PowerShell commands.</span></span>

### <a name="step-1-examine-hello-json-file-for-hello-template"></a><span data-ttu-id="6847b-286">Krok 1: Zkontrolujte hello JSON v souboru šablony hello</span><span class="sxs-lookup"><span data-stu-id="6847b-286">Step 1: Examine hello JSON file for hello template</span></span>
<span data-ttu-id="6847b-287">Tady jsou hello obsah souboru JSON hello hello šablony.</span><span class="sxs-lookup"><span data-stu-id="6847b-287">Here are hello contents of hello JSON file for hello template.</span></span> <span data-ttu-id="6847b-288">Pokud chcete, aby hello nejnovější verzi, ho má nachází [v úložišti GitHub hello pro šablony](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="6847b-288">If you want hello most recent version, it's located [at hello GitHub repository for templates](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json).</span></span> <span data-ttu-id="6847b-289">Toto téma používá hello `--template-uri` toocall přepínače v hello šablony, ale můžete také použít hello `--template-file` přepínač toopass místní verze.</span><span class="sxs-lookup"><span data-stu-id="6847b-289">This topic uses hello `--template-uri` switch toocall in hello template, but you can also use hello `--template-file` switch toopass a local version.</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location of resources"
            }
        },
        "storageAccountName": {
            "type": "string",
            "metadata": {
                "description": "Name of storage account"
            }
        },
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "Admin user name"
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Admin password"
            }
        },
        "dnsNameforLBIP": {
            "type": "string",
            "metadata": {
                "description": "DNS for load balancer IP"
            }
        },
        "backendPort": {
            "type": "int",
            "defaultValue": 3389,
            "metadata": {
                "description": "Back-end port"
            }
        },
        "vmNamePrefix": {
            "type": "string",
            "defaultValue": "myVM",
            "metadata": {
                "description": "Prefix toouse for VM names"
            }
        },
        "vmSourceImageName": {
            "type": "string",
            "defaultValue": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201412.01-en.us-127GB.vhd"
        },
        "lbName": {
            "type": "string",
            "defaultValue": "myLB",
            "metadata": {
                "description": "Load balancer name"
            }
        },
        "nicNamePrefix": {
            "type": "string",
            "defaultValue": "nic",
            "metadata": {
                "description": "Network interface name prefix"
            }
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue": "myPublicIP",
            "metadata": {
                "description": "Public IP name"
            }
        },
        "vnetName": {
            "type": "string",
            "defaultValue": "myVNET",
            "metadata": {
                "description": "Virtual network name"
            }
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A1",
            "metadata": {
                "description": "Size of hello VM"
            }
        }
    },
    "variables": {
        "storageAccountType": "Standard_LRS",
        "vmStorageAccountContainerName": "vhds",
        "availabilitySetName": "myAvSet",
        "addressPrefix": "10.0.0.0/16",
        "subnetName": "Subnet-1",
        "subnetPrefix": "10.0.0.0/24",
        "publicIPAddressType": "Dynamic",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
        "numberOfInstances": 2,
        "nicId1": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 0))]",
        "nicId2": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 1))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LBFE')]",
        "backEndIPConfigID1": "[concat(variables('nicId1'),'/ipConfigurations/ipconfig1')]",
        "backEndIPConfigID2": "[concat(variables('nicId2'),'/ipConfigurations/ipconfig1')]",
        "sourceImageName": "[concat('/', subscription().subscriptionId,'/services/images/',parameters('vmSourceImageName'))]",
        "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/LBBE')]",
        "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccountName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": { }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddressName')]",
            "location": "[parameters('location')]",
            "properties": {
                "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                "dnsSettings": {
                    "domainNameLabel": "[parameters('dnsNameforLBIP')]"
                }
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('vnetName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('addressPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('subnetName')]",
                        "properties": {
                            "addressPrefix": "[variables('subnetPrefix')]"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/networkInterfaces",
            "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
            "location": "[parameters('location')]",
            "copy": {
                "name": "nicLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
            ],
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "subnet": {
                                "id": "[variables('subnetRef')]"
                            }
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/backendAddressPools/LBBE')]"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/inboundNatRule/RDP-VM', copyindex())]"
                            }
                        ]


                    }
                ]

            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "name": "[parameters('lbName')]",
            "type": "Microsoft.Network/loadBalancers",
            "location": "[parameters('location')]",
            "dependsOn": [
                "nicLoop",
                "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
            ],
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "LBFE",
                        "properties": {
                            "publicIPAddress": {
                                "id": "[variables('publicIPAddressID')]"
                            }
                        }
                    }
                ],
                "backendAddressPools": [
                    {
                        "name": "LBBE"

                    }
                ],
                "inboundNatRules": [
                    {
                        "name": "RDP-VM1",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50001,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    },
                    {
                        "name": "RDP-VM2",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50002,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "LBRule",
                        "properties": {
                            "frontendIPConfiguration": {
                                "id": "[variables('frontEndIPConfigID')]"
                            },
                            "backendAddressPool": {
                                "id": "[variables('lbPoolID')]"
                            },
                            "protocol": "tcp",
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 5,
                            "probe": {
                                "id": "[variables('lbProbeID')]"
                            }
                        }
                    }
                ],
                "probes": [
                    {
                        "name": "tcpProbe",
                        "properties": {
                            "protocol": "tcp",
                            "port": 80,
                            "intervalInSeconds": "5",
                            "numberOfProbes": "2"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
            "copy": {
                "name": "virtualMachineLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
                "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
                "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
            ],
            "properties": {
                "availabilitySet": {
                    "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
                },
                "hardwareProfile": {
                    "vmSize": "[parameters('vmSize')]"
                },
                "osProfile": {
                    "computername": "[concat(parameters('vmNamePrefix'), copyIndex())]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                    "sourceImage": {
                        "id": "[variables('sourceImageName')]"
                    },
                    "destinationVhdsContainer": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/')]"
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
                        }
                    ]
                }
            }
        }
    ]
}
```

### <a name="step-2-create-hello-deployment-by-using-hello-template"></a><span data-ttu-id="6847b-290">Krok 2: Vytvoření hello nasazení pomocí šablony hello</span><span class="sxs-lookup"><span data-stu-id="6847b-290">Step 2: Create hello deployment by using hello template</span></span>
<span data-ttu-id="6847b-291">Vytvořte skupinu prostředků pro šablonu hello pomocí `azure group create <location>`.</span><span class="sxs-lookup"><span data-stu-id="6847b-291">Create a resource group for hello template by using `azure group create <location>`.</span></span> <span data-ttu-id="6847b-292">Pak vytvořte nasazení do této skupiny prostředků pomocí `azure group deployment create` a předání hello skupinu prostředků, předávání název nasazení a volaného hello výzvy pro parametry v hello šablonu, která nemá výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6847b-292">Then, create a deployment into that resource group by using `azure group deployment create` and passing hello resource group, passing a deployment name, and answering hello prompts for parameters in hello template that did not have default values.</span></span>

```azurecli
azure group create lbgroup westus
info:    Executing command group create
+ Getting resource group lbgroup
+ Creating resource group lbgroup
info:    Created resource group lbgroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/lbgroup
data:    Name:                lbgroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

<span data-ttu-id="6847b-293">Teď použít hello `azure group deployment create` příkaz a hello `--template-uri` možnost toodeploy hello šablony.</span><span class="sxs-lookup"><span data-stu-id="6847b-293">Now use hello `azure group deployment create` command and hello `--template-uri` option toodeploy hello template.</span></span> <span data-ttu-id="6847b-294">Připravte si hodnoty parametrů a po zobrazení příslušných výzev je zadejte, jak znázorňuje následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="6847b-294">Be ready with your parameter values when it prompts you, as shown below.</span></span>

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json \
> lbgroup \
> newdeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
location: westus
newStorageAccountName: storagename
adminUsername: ops
adminPassword: password
dnsNameforLBIP: lbdomainname
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "newdeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.compute
info:    Registering provider microsoft.network
+ Waiting for deployment toocomplete
data:    DeploymentName     : newdeployment
data:    ResourceGroupName  : lbgroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T20:58:40.1678876Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  ----------------------
data:    location               String        westus
data:    newStorageAccountName  String        storagename
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameforLBIP         String        lbdomainname
data:    backendPort            Int           3389
data:    vmNamePrefix           String        myVM
data:    imagePublisher         String        MicrosoftWindowsServer
data:    imageOffer             String        WindowsServer
data:    imageSKU               String        2012-R2-Datacenter
data:    lbName                 String        myLB
data:    nicNamePrefix          String        nic
data:    publicIPAddressName    String        myPublicIP
data:    vnetName               String        myVNET
data:    vmSize                 String        Standard_A1
info:    group deployment create command OK
```

<span data-ttu-id="6847b-295">Všimněte si, že tato šablona nasadí image Windows Serveru. Můžete ji ale snadno nahradit libovolnou linuxovou imagí.</span><span class="sxs-lookup"><span data-stu-id="6847b-295">Note that this template deploys a Windows Server image; however, it could easily be replaced by any Linux image.</span></span> <span data-ttu-id="6847b-296">Chcete toocreate Docker clusteru s několika správci swarm?</span><span class="sxs-lookup"><span data-stu-id="6847b-296">Want toocreate a Docker cluster with multiple swarm managers?</span></span> <span data-ttu-id="6847b-297">[Můžete](https://azure.microsoft.com/documentation/templates/docker-swarm-cluster/).</span><span class="sxs-lookup"><span data-stu-id="6847b-297">[You can do it](https://azure.microsoft.com/documentation/templates/docker-swarm-cluster/).</span></span>

## <span data-ttu-id="6847b-298"><a id="remove-a-resource-group"></a>Úkol: Odebrání skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="6847b-298"><a id="remove-a-resource-group"></a>Task: Remove a resource group</span></span>
<span data-ttu-id="6847b-299">Mějte na paměti, že můžete znovu nasadit tooa skupinu prostředků, ale pokud jste hotovi s jedním, můžete ho odstranit pomocí `azure group delete <group name>`.</span><span class="sxs-lookup"><span data-stu-id="6847b-299">Remember that you can redeploy tooa resource group, but if you are done with one, you can delete it by using `azure group delete <group name>`.</span></span>

```azurecli
azure group delete myResourceGroup
info:    Executing command group delete
Delete resource group myResourceGroup? [y/n] y
+ Deleting resource group myResourceGroup
info:    group delete command OK
```

## <span data-ttu-id="6847b-300"><a id="show-the-log-for-a-resource-group-deployment"></a>Úloha: Zobrazit hello protokolu pro nasazení skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="6847b-300"><a id="show-the-log-for-a-resource-group-deployment"></a>Task: Show hello log for a resource group deployment</span></span>
<span data-ttu-id="6847b-301">Tento úkol je obvyklý při vytváření nebo používání šablon.</span><span class="sxs-lookup"><span data-stu-id="6847b-301">This one is common while you're creating or using templates.</span></span> <span data-ttu-id="6847b-302">protokoly Hello volání toodisplay hello nasazení pro skupinu je `azure group log show <groupname>`, který zobrazuje s bit informace, které jsou užitečné při hledání něco stalo--nebo nebyla.</span><span class="sxs-lookup"><span data-stu-id="6847b-302">hello call toodisplay hello deployment logs for a group is `azure group log show <groupname>`, which displays quite a bit of information that's useful for understanding why something happened -- or didn't.</span></span> <span data-ttu-id="6847b-303">(Další informace o řešení potíží s nasazeními a také další informace o problémech najdete v tématu [Řešení chyb nasazení v Azure pomocí Azure Resource Manageru](../articles/azure-resource-manager/resource-manager-common-deployment-errors.md).)</span><span class="sxs-lookup"><span data-stu-id="6847b-303">(For more information on troubleshooting your deployments, as well as other information about issues, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](../articles/azure-resource-manager/resource-manager-common-deployment-errors.md).)</span></span>

<span data-ttu-id="6847b-304">tootarget specifických chybách, například můžete použít nástroje, například **jq** tooquery věcí, které jsou o něco víc, přesněji, například které jednotlivé chyby, je nutné toocorrect.</span><span class="sxs-lookup"><span data-stu-id="6847b-304">tootarget specific failures, for example, you might use tools like **jq** tooquery things a bit more precisely, such as which individual failures you need toocorrect.</span></span> <span data-ttu-id="6847b-305">Hello následující příklad používá **jq** tooparse nasazení protokolu **lbgroup**, kteří hledají selhání.</span><span class="sxs-lookup"><span data-stu-id="6847b-305">hello following example uses **jq** tooparse a deployment log for **lbgroup**, looking for failures.</span></span>

```azurecli
azure group log show lbgroup -l --json | jq '.[] | select(.status.value == "Failed") | .properties'
```
<span data-ttu-id="6847b-306">Umožňuje rychle zjistit, co se nepovedlo, opravit to a zkusit znovu.</span><span class="sxs-lookup"><span data-stu-id="6847b-306">You can discover very quickly what went wrong, fix, and retry.</span></span> <span data-ttu-id="6847b-307">V následujícím případě hello, hello šablonu měl byla vytváření dva virtuální počítače v hello současně, který vytvoří zámek na VHD hello.</span><span class="sxs-lookup"><span data-stu-id="6847b-307">In hello following case, hello template had been creating two VMs at hello same time, which created a lock on hello .vhd.</span></span> <span data-ttu-id="6847b-308">(Po úpravě šablony hello jsme hello nasazení bylo úspěšné rychle.)</span><span class="sxs-lookup"><span data-stu-id="6847b-308">(After we modified hello template, hello deployment succeeded quickly.)</span></span>

```json
{
    "statusCode": "Conflict",
    "statusMessage": "{\"status\":\"Failed\",\"error\":{\"code\":\"ResourceDeploymentFailure\",\"message\":\"hello resource operation completed with terminal provisioning state 'Failed'.\",\"details\":[{\"code\":\"AcquireDiskLeaseFailed\",\"message\":\"Failed tooacquire lease while creating disk 'osdisk' using blob with URI http://storage.blob.core.windows.net/vhds/osdisk.vhd.\"}]}}"
}
```

## <span data-ttu-id="6847b-309"><a id="display-information-about-a-virtual-machine"></a>Úkol: Zobrazení informací o virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="6847b-309"><a id="display-information-about-a-virtual-machine"></a>Task: Display information about a virtual machine</span></span>
<span data-ttu-id="6847b-310">Zobrazí se informace o konkrétní virtuální počítače ve vaší skupině prostředků s použitím hello `azure vm show <groupname> <vmname>` příkaz.</span><span class="sxs-lookup"><span data-stu-id="6847b-310">You can see information about specific VMs in your resource group by using hello `azure vm show <groupname> <vmname>` command.</span></span> <span data-ttu-id="6847b-311">Pokud máte více než jeden virtuální počítač ve vaší skupině, bude pravděpodobně nutné nejprve toolist hello virtuální počítače ve skupině pomocí `azure vm list <groupname>`.</span><span class="sxs-lookup"><span data-stu-id="6847b-311">If you have more than one VM in your group, you might first need toolist hello VMs in a group by using `azure vm list <groupname>`.</span></span>

```azurecli
azure vm list zoo
info:    Executing command vm list
+ Getting virtual machines
data:    Name   ProvisioningState  Location  Size
data:    -----  -----------------  --------  -----------
data:    myVM0  Succeeded          westus    Standard_A1
data:    myVM1  Failed             westus    Standard_A1
```

<span data-ttu-id="6847b-312">A potom vyhledat počítač myVM1:</span><span class="sxs-lookup"><span data-stu-id="6847b-312">And then, looking up myVM1:</span></span>

```azurecli
azure vm show zoo myVM1
info:    Executing command vm show
+ Looking up hello VM "myVM1"
+ Looking up hello NIC "nic1"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Failed
data:    Name                            :myVM1
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :MicrosoftWindowsServer
data:        Offer                       :WindowsServer
data:        Sku                         :2012-R2-Datacenter
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Windows
data:        Name                        :osdisk
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :http://zoostorageralph.blob.core.windows.net/vhds/osdisk.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Windows Configuration:
data:        Provision VM Agent          :true
data:        Enable automatic updates    :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Network/networkInterfaces/nic1
data:          Primary                   :false
data:          Provisioning State        :Succeeded
data:          Name                      :nic1
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.0.5
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/availabilitySets/MYAVSET
info:    vm show command OK
```

> [!NOTE]
> <span data-ttu-id="6847b-313">Pokud chcete tooprogrammatically úložiště a manipulaci s výstup hello příkazů vaší konzoly, může být vhodné toouse JSON analýza nástroje, jako  **[jq](https://github.com/stedolan/jq)**  nebo  **[jsawk](https://github.com/micha/jsawk)** , nebo knihovny jazyka, které jsou vhodné pro úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="6847b-313">If you want tooprogrammatically store and manipulate hello output of your console commands, you may want toouse a JSON parsing tool such as **[jq](https://github.com/stedolan/jq)** or **[jsawk](https://github.com/micha/jsawk)**, or language libraries that are good for hello task.</span></span>
>
>

## <span data-ttu-id="6847b-314"><a id="log-on-to-a-linux-based-virtual-machine"></a>Úloha: Přihlaste tooa systémem Linux virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6847b-314"><a id="log-on-to-a-linux-based-virtual-machine"></a>Task: Log on tooa Linux-based virtual machine</span></span>
<span data-ttu-id="6847b-315">Počítače se systémem Linux jsou obvykle připojené toothrough SSH.</span><span class="sxs-lookup"><span data-stu-id="6847b-315">Typically Linux machines are connected toothrough SSH.</span></span> <span data-ttu-id="6847b-316">Další informace najdete v tématu [jak toouse SSH s Linuxem v Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6847b-316">For more information, see [How toouse SSH with Linux on Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <span data-ttu-id="6847b-317"><a id="stop-a-virtual-machine"></a>Úkol: Zastavení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6847b-317"><a id="stop-a-virtual-machine"></a>Task: Stop a VM</span></span>
<span data-ttu-id="6847b-318">Spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="6847b-318">Run this command:</span></span>

```azurecli
azure vm stop <group name> <virtual machine name>
```

> [!IMPORTANT]
> <span data-ttu-id="6847b-319">Použijte tento parametr tookeep hello virtuální IP (VIP) hello sítě vnet, v případě, že je hello poslední virtuální počítač v této virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6847b-319">Use this parameter tookeep hello virtual IP (VIP) of hello vnet in case it's hello last VM in that vnet.</span></span> <br><br> <span data-ttu-id="6847b-320">Pokud používáte hello `StayProvisioned` parametr stále platit budete pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6847b-320">If you use hello `StayProvisioned` parameter, you'll still be billed for hello VM.</span></span>
>
>

## <span data-ttu-id="6847b-321"><a id="start-a-virtual-machine"></a>Úkol: Spuštění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6847b-321"><a id="start-a-virtual-machine"></a>Task: Start a VM</span></span>
<span data-ttu-id="6847b-322">Spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="6847b-322">Run this command:</span></span>

```azurecli
azure vm start <group name> <virtual machine name>
```

## <span data-ttu-id="6847b-323"><a id="attach-a-data-disk"></a>Úkol: Připojení datového disku</span><span class="sxs-lookup"><span data-stu-id="6847b-323"><a id="attach-a-data-disk"></a>Task: Attach a data disk</span></span>
<span data-ttu-id="6847b-324">Budete také potřebovat toodecide zda tooattach nový disk nebo jeden, který obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="6847b-324">You'll also need toodecide whether tooattach a new disk or one that contains data.</span></span> <span data-ttu-id="6847b-325">Pro nový disk, hello příkaz vytvoří soubor VHD hello a připojí jej v hello stejný příkaz.</span><span class="sxs-lookup"><span data-stu-id="6847b-325">For a new disk, hello command creates hello .vhd file and attaches it in hello same command.</span></span>

<span data-ttu-id="6847b-326">tooattach nový disk, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="6847b-326">tooattach a new disk, run this command:</span></span>

```azurecli
    azure vm disk attach-new <resource-group> <vm-name> <size-in-gb>
```

<span data-ttu-id="6847b-327">tooattach stávající datový disk, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="6847b-327">tooattach an existing data disk, run this command:</span></span>

```azurecli
azure vm disk attach <resource-group> <vm-name> [vhd-url]
```

<span data-ttu-id="6847b-328">Toomount hello disku, pak musíte jako za normálních okolností byste v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="6847b-328">Then you'll need toomount hello disk, as you normally would in Linux.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6847b-329">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6847b-329">Next steps</span></span>
<span data-ttu-id="6847b-330">Daleko Další příklady použití Azure CLI s hello **arm** režimu, najdete v části [hello pomocí Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](../articles/xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="6847b-330">For far more examples of Azure CLI usage with hello **arm** mode, see [Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](../articles/xplat-cli-azure-resource-manager.md).</span></span> <span data-ttu-id="6847b-331">toolearn Další informace o prostředků Azure a jejich koncepty, najdete v části [přehled Azure Resource Manageru](../articles/azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6847b-331">toolearn more about Azure resources and their concepts, see [Azure Resource Manager overview](../articles/azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="6847b-332">Další šablony, které můžete použít, najdete v tématech věnovaných [rychlému úvodu do šablon pro Azure](https://azure.microsoft.com/documentation/templates/) a [aplikačním architekturám využívajícím šablony](../articles/virtual-machines/linux/app-frameworks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6847b-332">For more templates you can use, see [Azure Quickstart templates](https://azure.microsoft.com/documentation/templates/) and [Application frameworks using templates](../articles/virtual-machines/linux/app-frameworks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
