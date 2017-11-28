
* [<span data-ttu-id="71128-101">Rychlé vytvoření virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="71128-101">Quick-create a virtual machine in Azure</span></span>](#quick-create-a-vm-in-azure)
* [<span data-ttu-id="71128-102">Nasazení virtuálního počítače v Azure ze šablony</span><span class="sxs-lookup"><span data-stu-id="71128-102">Deploy a virtual machine in Azure from a template</span></span>](#deploy-a-vm-in-azure-from-a-template)
* [<span data-ttu-id="71128-103">Vytvoření virtuálního počítače z vlastní image</span><span class="sxs-lookup"><span data-stu-id="71128-103">Create a virtual machine from a custom image</span></span>](#create-a-custom-vm-image)
* [<span data-ttu-id="71128-104">Nasazení virtuálního počítače, který používá virtuální síť a nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="71128-104">Deploy a virtual machine that uses a virtual network and a load balancer</span></span>](#deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer)
* [<span data-ttu-id="71128-105">Odebrání skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="71128-105">Remove a resource group</span></span>](#remove-a-resource-group)
* [<span data-ttu-id="71128-106">Zobrazení protokolu pro nasazení skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="71128-106">Show the log for a resource group deployment</span></span>](#show-the-log-for-a-resource-group-deployment)
* [<span data-ttu-id="71128-107">Zobrazení informací o virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="71128-107">Display information about a virtual machine</span></span>](#display-information-about-a-virtual-machine)
* [<span data-ttu-id="71128-108">Připojení k virtuálnímu počítači s Linuxem</span><span class="sxs-lookup"><span data-stu-id="71128-108">Connect to a Linux-based virtual machine</span></span>](#log-on-to-a-linux-based-virtual-machine)
* [<span data-ttu-id="71128-109">Zastavení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="71128-109">Stop a virtual machine</span></span>](#stop-a-virtual-machine)
* [<span data-ttu-id="71128-110">Spuštění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="71128-110">Start a virtual machine</span></span>](#start-a-virtual-machine)
* [<span data-ttu-id="71128-111">Připojení datového disku</span><span class="sxs-lookup"><span data-stu-id="71128-111">Attach a data disk</span></span>](#attach-a-data-disk)

## <a name="getting-ready"></a><span data-ttu-id="71128-112">Příprava</span><span class="sxs-lookup"><span data-stu-id="71128-112">Getting ready</span></span>
<span data-ttu-id="71128-113">Abyste mohli použít rozhraní příkazového řádku Azure se skupinou prostředků Azure, musíte mít správnou verzi rozhraní příkazového řádku Azure a účet Azure.</span><span class="sxs-lookup"><span data-stu-id="71128-113">Before you can use the Azure CLI with Azure resource groups, you need to have the right Azure CLI version and an Azure account.</span></span> <span data-ttu-id="71128-114">Pokud nemáte rozhraní příkazového řádku Azure, [nainstalujte ho](../articles/cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="71128-114">If you don't have the Azure CLI, [install it](../articles/cli-install-nodejs.md).</span></span>

### <a name="update-your-azure-cli-version-to-090-or-later"></a><span data-ttu-id="71128-115">Aktualizace rozhraní příkazového řádku Azure na verzi 0.9.0 nebo novější</span><span class="sxs-lookup"><span data-stu-id="71128-115">Update your Azure CLI version to 0.9.0 or later</span></span>
<span data-ttu-id="71128-116">Zadejte `azure --version` a podívejte se, jestli už máte nainstalovanou verzi 0.9.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="71128-116">Type `azure --version` to see whether you have already installed version 0.9.0 or later.</span></span>

```azurecli
azure --version
0.9.0 (node: 0.10.25)
```

<span data-ttu-id="71128-117">Pokud nemáte verzi 0.9.0 nebo novější, musíte rozhraní aktualizovat pomocí jednoho z nativních instalačních programů nebo prostřednictvím **npm**, a to zadáním příkazu `npm update -g azure-cli`.</span><span class="sxs-lookup"><span data-stu-id="71128-117">If your version is not 0.9.0 or later, you need to update it by using one of the native installers or through **npm** by typing `npm update -g azure-cli`.</span></span>

<span data-ttu-id="71128-118">Rozhraní příkazového řádku Azure můžete také spustit jako kontejner Dockeru pomocí následující [image Dockeru](https://registry.hub.docker.com/u/microsoft/azure-cli/).</span><span class="sxs-lookup"><span data-stu-id="71128-118">You can also run Azure CLI as a Docker container by using the following [Docker image](https://registry.hub.docker.com/u/microsoft/azure-cli/).</span></span> <span data-ttu-id="71128-119">Z hostitele Docker spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="71128-119">From a Docker host, run the following command:</span></span>

```bash
docker run -it microsoft/azure-cli
```

### <a name="set-your-azure-account-and-subscription"></a><span data-ttu-id="71128-120">Nastavení předplatného a účtu Azure</span><span class="sxs-lookup"><span data-stu-id="71128-120">Set your Azure account and subscription</span></span>
<span data-ttu-id="71128-121">Pokud ještě nemáte předplatné Azure, ale máte předplatné MSDN, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="71128-121">If you don't already have an Azure subscription but you do have an MSDN subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span> <span data-ttu-id="71128-122">Můžete si zaregistrovat i [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="71128-122">Or you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

<span data-ttu-id="71128-123">Teď se [interaktivně přihlaste k účtu Azure](../articles/xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Zadejte `azure login`, postupujte podle výzev a využijte možnost interaktivního přihlášení k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="71128-123">Now [log in to your Azure account interactively](../articles/xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login) by typing `azure login` and following the prompts for an interactive login experience to your Azure account.</span></span> 

> [!NOTE]
> <span data-ttu-id="71128-124">Pokud máte pracovní nebo školní ID a víte, že nemáte povolené dvoufaktorové ověřování, můžete **také** použít `azure login -u` společně s pracovním nebo školním ID a přihlásit se *bez* interaktivní relace.</span><span class="sxs-lookup"><span data-stu-id="71128-124">If you have a work or school ID and you know you do not have two-factor authentication enabled, you can **also** use `azure login -u` along with the work or school ID to log in *without* an interactive session.</span></span> <span data-ttu-id="71128-125">Pokud nemáte pracovní nebo školní ID, můžete [vytvořit pracovní nebo školní ID z vašeho osobního účtu Microsoft](../articles/virtual-machines/windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a přihlásit se stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="71128-125">If you don't have a work or school ID, you can [create a work or school id from your personal Microsoft account](../articles/virtual-machines/windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) to log in the same way.</span></span>
>
>

<span data-ttu-id="71128-126">Váš účet může mít víc než jedno předplatné.</span><span class="sxs-lookup"><span data-stu-id="71128-126">Your account may have more than one subscription.</span></span> <span data-ttu-id="71128-127">Seznam předplatných můžete zobrazit zadáním `azure account list`. Výsledek bude vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="71128-127">You can list your subscriptions by typing `azure account list`, which might look something like this:</span></span>

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

<span data-ttu-id="71128-128">Pomocí následujícího příkazu můžete nastavit aktuální předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="71128-128">You can set the current Azure subscription by typing the following.</span></span> <span data-ttu-id="71128-129">Použijte název předplatného nebo ID s prostředky, které chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="71128-129">Use the subscription name or the ID that has the resources you want to manage.</span></span>

```azurecli
azure account set <subscription name or ID> true
```

### <a name="switch-to-the-azure-cli-resource-group-mode"></a><span data-ttu-id="71128-130">Přepnutí do režimu skupiny prostředků rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="71128-130">Switch to the Azure CLI resource group mode</span></span>
<span data-ttu-id="71128-131">Ve výchozím nastavení se rozhraní příkazového řádku Azure spustí v režimu správy služeb (režim **asm**).</span><span class="sxs-lookup"><span data-stu-id="71128-131">By default, the Azure CLI starts in the service management mode (**asm** mode).</span></span> <span data-ttu-id="71128-132">Pro přepnutí do režimu skupiny prostředků zadejte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="71128-132">Type the following to switch to resource group mode.</span></span>

```azurecli
azure config mode arm
```

## <a name="understanding-azure-resource-templates-and-resource-groups"></a><span data-ttu-id="71128-133">Principy skupin prostředků a šablon prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="71128-133">Understanding Azure resource templates and resource groups</span></span>
<span data-ttu-id="71128-134">Většina aplikací je vytvořená pomocí kombinace různých typů prostředků (jako je jeden nebo několik virtuálních počítačů a účtů úložiště, databáze SQL, virtuální síť nebo síť pro doručování obsahu).</span><span class="sxs-lookup"><span data-stu-id="71128-134">Most applications are built from a combination of different resource types (such as one or more VMs and storage accounts, a SQL database, a virtual network, or a content delivery network).</span></span> <span data-ttu-id="71128-135">Výchozí rozhraní API pro správu služeb Azure a portál Azure Classic reprezentuje tyto položky podle jednotlivých služeb.</span><span class="sxs-lookup"><span data-stu-id="71128-135">The default Azure service management API and the Azure classic portal represented these items by using a service-by-service approach.</span></span> <span data-ttu-id="71128-136">Tento přístup vyžaduje, abyste jednotlivé služby nasadili a spravovali po jedné (nebo k tomuto účelu našli jiné nástroje), a ne jako jednu logickou jednotku nasazení.</span><span class="sxs-lookup"><span data-stu-id="71128-136">This approach requires you to deploy and manage the individual services individually (or find other tools that do so), and not as a single logical unit of deployment.</span></span>

<span data-ttu-id="71128-137">*Šablony Azure Resource Manageru* ale umožňují nasadit a spravovat tyto různé prostředky jako jednu logickou jednotku nasazení, a to deklarativní způsobem.</span><span class="sxs-lookup"><span data-stu-id="71128-137">*Azure Resource Manager templates*, however, make it possible for you to deploy and manage these different resources as one logical deployment unit in a declarative fashion.</span></span> <span data-ttu-id="71128-138">Místo toho, abyste Azure dali přesné pokyny k nasazení jednoho příkazu za druhým, popíšete celé nasazení v souboru JSON (to znamená všechny prostředky a přidružené konfigurace a parametry nasazení) a sdělíte Azure, že se tyto prostředky nasadí jako jedna skupina.</span><span class="sxs-lookup"><span data-stu-id="71128-138">Instead of imperatively telling Azure what to deploy one command after another, you describe your entire deployment in a JSON file -- all of the resources and associated configuration and deployment parameters -- and tell Azure to deploy those resources as one group.</span></span>

<span data-ttu-id="71128-139">Potom můžete celý životní cyklus prostředků této skupiny spravovat pomocí příkazů pro správu prostředků rozhraní příkazového řádku Azure a provádět tyto operace:</span><span class="sxs-lookup"><span data-stu-id="71128-139">You can then manage the overall life cycle of the group's resources by using Azure CLI resource management commands to:</span></span>

* <span data-ttu-id="71128-140">Zastavení, spuštění a odstranění všech prostředků v rámci skupiny najednou.</span><span class="sxs-lookup"><span data-stu-id="71128-140">Stop, start, or delete all of the resources within the group at once.</span></span>
* <span data-ttu-id="71128-141">Použití pravidel řízení přístupu na základě rolí (RBAC) k tomu, abyste pro tyto prostředky zamkli oprávnění zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="71128-141">Apply Role-Based Access Control (RBAC) rules to lock down security permissions on them.</span></span>
* <span data-ttu-id="71128-142">Auditování operací.</span><span class="sxs-lookup"><span data-stu-id="71128-142">Audit operations.</span></span>
* <span data-ttu-id="71128-143">Označení prostředků dalšími metadaty pro lepší sledování.</span><span class="sxs-lookup"><span data-stu-id="71128-143">Tag resources with additional metadata for better tracking.</span></span>

<span data-ttu-id="71128-144">Spoustu dalších informací o skupinách prostředků Azure a tom, k čemu je můžete využít, najdete v [přehledu Azure Resource Manageru](../articles/azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="71128-144">You can learn lots more about Azure resource groups and what they can do for you in the [Azure Resource Manager overview](../articles/azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="71128-145">Pokud vás zajímá vytváření šablon, přečtěte si téma věnované [vytváření šablon Azure Resource Manageru](../articles/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="71128-145">If you're interested in authoring templates, see [Authoring Azure Resource Manager templates](../articles/resource-group-authoring-templates.md).</span></span>

## <span data-ttu-id="71128-146"><a id="quick-create-a-vm-in-azure"></a>Úkol: Rychlé vytvoření virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="71128-146"><a id="quick-create-a-vm-in-azure"></a>Task: Quick-create a VM in Azure</span></span>
<span data-ttu-id="71128-147">Někdy víte, kterou image chcete použít, potřebujete virtuální počítač z této image hned a moc vás nezajímá infrastruktura – chcete třeba něco otestovat na čistém virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="71128-147">Sometimes you know what image you need, and you need a VM from that image right now and you don't care too much about the infrastructure -- maybe you have to test something on a clean VM.</span></span> <span data-ttu-id="71128-148">A právě to je chvíle pro použití příkazu `azure vm quick-create`, kterému předáte všechny argumenty potřebné k vytvoření tohoto virtuálního počítače a jeho infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="71128-148">That's when you want to use the `azure vm quick-create` command, and pass the arguments necessary to create a VM and its infrastructure.</span></span>

<span data-ttu-id="71128-149">Nejdřív vytvoříte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="71128-149">First, create your resource group.</span></span>

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

<span data-ttu-id="71128-150">Potom budete potřebovat image.</span><span class="sxs-lookup"><span data-stu-id="71128-150">Second, you'll need an image.</span></span> <span data-ttu-id="71128-151">Pokud chcete najít image pomocí rozhraní příkazového řádku Azure, přečtěte si téma věnované [navigaci a výběru imagí virtuálního počítače Azure pomocí PowerShellu a rozhraní příkazového řádku Azure](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="71128-151">To find an image with the Azure CLI, see [Navigating and selecting Azure virtual machine images with PowerShell and the Azure CLI](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="71128-152">Ale pro účely tohoto článku použijeme krátký seznam oblíbených imagí.</span><span class="sxs-lookup"><span data-stu-id="71128-152">But for this article, here's a short list of popular images.</span></span> <span data-ttu-id="71128-153">Pro toto rychlé vytvoření použijeme image Stable CoreOS.</span><span class="sxs-lookup"><span data-stu-id="71128-153">We'll use CoreOS's Stable image for this quick-create.</span></span>

> [!NOTE]
> <span data-ttu-id="71128-154">Jako ComputeImageVersion můžete také v jazyce šablony i v rozhraní příkazového řádku Azure jednoduše zadat parametr latest.</span><span class="sxs-lookup"><span data-stu-id="71128-154">For ComputeImageVersion, you can also simply supply 'latest' as the parameter in both the template language and in the Azure CLI.</span></span> <span data-ttu-id="71128-155">To vám umožní vždycky použít nejnovější opravenou verzi image, aniž byste museli upravovat svoje skripty nebo šablony.</span><span class="sxs-lookup"><span data-stu-id="71128-155">This will allow you to always use the latest and patched version of the image without having to modify your scripts or templates.</span></span> <span data-ttu-id="71128-156">Příklad najdete níž.</span><span class="sxs-lookup"><span data-stu-id="71128-156">This is shown below.</span></span>
>
>

| <span data-ttu-id="71128-157">Název vydavatele</span><span class="sxs-lookup"><span data-stu-id="71128-157">PublisherName</span></span> | <span data-ttu-id="71128-158">Nabídka</span><span class="sxs-lookup"><span data-stu-id="71128-158">Offer</span></span> | <span data-ttu-id="71128-159">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="71128-159">Sku</span></span> | <span data-ttu-id="71128-160">Verze</span><span class="sxs-lookup"><span data-stu-id="71128-160">Version</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="71128-161">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="71128-161">OpenLogic</span></span> |<span data-ttu-id="71128-162">CentOS</span><span class="sxs-lookup"><span data-stu-id="71128-162">CentOS</span></span> |<span data-ttu-id="71128-163">7</span><span class="sxs-lookup"><span data-stu-id="71128-163">7</span></span> |<span data-ttu-id="71128-164">7.0.201503</span><span class="sxs-lookup"><span data-stu-id="71128-164">7.0.201503</span></span> |
| <span data-ttu-id="71128-165">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="71128-165">OpenLogic</span></span> |<span data-ttu-id="71128-166">CentOS</span><span class="sxs-lookup"><span data-stu-id="71128-166">CentOS</span></span> |<span data-ttu-id="71128-167">7.1</span><span class="sxs-lookup"><span data-stu-id="71128-167">7.1</span></span> |<span data-ttu-id="71128-168">7.1.201504</span><span class="sxs-lookup"><span data-stu-id="71128-168">7.1.201504</span></span> |
| <span data-ttu-id="71128-169">CoreOS</span><span class="sxs-lookup"><span data-stu-id="71128-169">CoreOS</span></span> |<span data-ttu-id="71128-170">CoreOS</span><span class="sxs-lookup"><span data-stu-id="71128-170">CoreOS</span></span> |<span data-ttu-id="71128-171">Beta</span><span class="sxs-lookup"><span data-stu-id="71128-171">Beta</span></span> |<span data-ttu-id="71128-172">647.0.0</span><span class="sxs-lookup"><span data-stu-id="71128-172">647.0.0</span></span> |
| <span data-ttu-id="71128-173">CoreOS</span><span class="sxs-lookup"><span data-stu-id="71128-173">CoreOS</span></span> |<span data-ttu-id="71128-174">CoreOS</span><span class="sxs-lookup"><span data-stu-id="71128-174">CoreOS</span></span> |<span data-ttu-id="71128-175">Stable</span><span class="sxs-lookup"><span data-stu-id="71128-175">Stable</span></span> |<span data-ttu-id="71128-176">633.1.0</span><span class="sxs-lookup"><span data-stu-id="71128-176">633.1.0</span></span> |
| <span data-ttu-id="71128-177">MicrosoftDynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="71128-177">MicrosoftDynamicsNAV</span></span> |<span data-ttu-id="71128-178">DynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="71128-178">DynamicsNAV</span></span> |<span data-ttu-id="71128-179">2015</span><span class="sxs-lookup"><span data-stu-id="71128-179">2015</span></span> |<span data-ttu-id="71128-180">8.0.40459</span><span class="sxs-lookup"><span data-stu-id="71128-180">8.0.40459</span></span> |
| <span data-ttu-id="71128-181">MicrosoftSharePoint</span><span class="sxs-lookup"><span data-stu-id="71128-181">MicrosoftSharePoint</span></span> |<span data-ttu-id="71128-182">MicrosoftSharePointServer</span><span class="sxs-lookup"><span data-stu-id="71128-182">MicrosoftSharePointServer</span></span> |<span data-ttu-id="71128-183">2013</span><span class="sxs-lookup"><span data-stu-id="71128-183">2013</span></span> |<span data-ttu-id="71128-184">1.0.0</span><span class="sxs-lookup"><span data-stu-id="71128-184">1.0.0</span></span> |
| <span data-ttu-id="71128-185">msopentech</span><span class="sxs-lookup"><span data-stu-id="71128-185">msopentech</span></span> |<span data-ttu-id="71128-186">Oracle-Database-12c-Weblogic-Server-12c</span><span class="sxs-lookup"><span data-stu-id="71128-186">Oracle-Database-12c-Weblogic-Server-12c</span></span> |<span data-ttu-id="71128-187">Standard</span><span class="sxs-lookup"><span data-stu-id="71128-187">Standard</span></span> |<span data-ttu-id="71128-188">1.0.0</span><span class="sxs-lookup"><span data-stu-id="71128-188">1.0.0</span></span> |
| <span data-ttu-id="71128-189">msopentech</span><span class="sxs-lookup"><span data-stu-id="71128-189">msopentech</span></span> |<span data-ttu-id="71128-190">Oracle-Database-12c-Weblogic-Server-12c</span><span class="sxs-lookup"><span data-stu-id="71128-190">Oracle-Database-12c-Weblogic-Server-12c</span></span> |<span data-ttu-id="71128-191">Enterprise</span><span class="sxs-lookup"><span data-stu-id="71128-191">Enterprise</span></span> |<span data-ttu-id="71128-192">1.0.0</span><span class="sxs-lookup"><span data-stu-id="71128-192">1.0.0</span></span> |
| <span data-ttu-id="71128-193">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="71128-193">MicrosoftSQLServer</span></span> |<span data-ttu-id="71128-194">SQL2014-WS2012R2</span><span class="sxs-lookup"><span data-stu-id="71128-194">SQL2014-WS2012R2</span></span> |<span data-ttu-id="71128-195">Enterprise-Optimized-for-DW</span><span class="sxs-lookup"><span data-stu-id="71128-195">Enterprise-Optimized-for-DW</span></span> |<span data-ttu-id="71128-196">12.0.2430</span><span class="sxs-lookup"><span data-stu-id="71128-196">12.0.2430</span></span> |
| <span data-ttu-id="71128-197">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="71128-197">MicrosoftSQLServer</span></span> |<span data-ttu-id="71128-198">SQL2014-WS2012R2</span><span class="sxs-lookup"><span data-stu-id="71128-198">SQL2014-WS2012R2</span></span> |<span data-ttu-id="71128-199">Enterprise-Optimized-for-OLTP</span><span class="sxs-lookup"><span data-stu-id="71128-199">Enterprise-Optimized-for-OLTP</span></span> |<span data-ttu-id="71128-200">12.0.2430</span><span class="sxs-lookup"><span data-stu-id="71128-200">12.0.2430</span></span> |
| <span data-ttu-id="71128-201">Canonical</span><span class="sxs-lookup"><span data-stu-id="71128-201">Canonical</span></span> |<span data-ttu-id="71128-202">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="71128-202">UbuntuServer</span></span> |<span data-ttu-id="71128-203">12.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="71128-203">12.04.5-LTS</span></span> |<span data-ttu-id="71128-204">12.04.201504230</span><span class="sxs-lookup"><span data-stu-id="71128-204">12.04.201504230</span></span> |
| <span data-ttu-id="71128-205">Canonical</span><span class="sxs-lookup"><span data-stu-id="71128-205">Canonical</span></span> |<span data-ttu-id="71128-206">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="71128-206">UbuntuServer</span></span> |<span data-ttu-id="71128-207">14.04.2-LTS</span><span class="sxs-lookup"><span data-stu-id="71128-207">14.04.2-LTS</span></span> |<span data-ttu-id="71128-208">14.04.201503090</span><span class="sxs-lookup"><span data-stu-id="71128-208">14.04.201503090</span></span> |
| <span data-ttu-id="71128-209">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="71128-209">MicrosoftWindowsServer</span></span> |<span data-ttu-id="71128-210">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="71128-210">WindowsServer</span></span> |<span data-ttu-id="71128-211">2012-Datacenter</span><span class="sxs-lookup"><span data-stu-id="71128-211">2012-Datacenter</span></span> |<span data-ttu-id="71128-212">3.0.201503</span><span class="sxs-lookup"><span data-stu-id="71128-212">3.0.201503</span></span> |
| <span data-ttu-id="71128-213">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="71128-213">MicrosoftWindowsServer</span></span> |<span data-ttu-id="71128-214">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="71128-214">WindowsServer</span></span> |<span data-ttu-id="71128-215">2012-R2-Datacenter</span><span class="sxs-lookup"><span data-stu-id="71128-215">2012-R2-Datacenter</span></span> |<span data-ttu-id="71128-216">4.0.201503</span><span class="sxs-lookup"><span data-stu-id="71128-216">4.0.201503</span></span> |
| <span data-ttu-id="71128-217">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="71128-217">MicrosoftWindowsServer</span></span> |<span data-ttu-id="71128-218">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="71128-218">WindowsServer</span></span> |<span data-ttu-id="71128-219">Windows-Server-Technical-Preview</span><span class="sxs-lookup"><span data-stu-id="71128-219">Windows-Server-Technical-Preview</span></span> |<span data-ttu-id="71128-220">5.0.201504</span><span class="sxs-lookup"><span data-stu-id="71128-220">5.0.201504</span></span> |
| <span data-ttu-id="71128-221">MicrosoftWindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="71128-221">MicrosoftWindowsServerEssentials</span></span> |<span data-ttu-id="71128-222">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="71128-222">WindowsServerEssentials</span></span> |<span data-ttu-id="71128-223">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="71128-223">WindowsServerEssentials</span></span> |<span data-ttu-id="71128-224">1.0.141204</span><span class="sxs-lookup"><span data-stu-id="71128-224">1.0.141204</span></span> |
| <span data-ttu-id="71128-225">MicrosoftWindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="71128-225">MicrosoftWindowsServerHPCPack</span></span> |<span data-ttu-id="71128-226">WindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="71128-226">WindowsServerHPCPack</span></span> |<span data-ttu-id="71128-227">2012R2</span><span class="sxs-lookup"><span data-stu-id="71128-227">2012R2</span></span> |<span data-ttu-id="71128-228">4.3.4665</span><span class="sxs-lookup"><span data-stu-id="71128-228">4.3.4665</span></span> |

<span data-ttu-id="71128-229">Teď vytvořte virtuální počítač zadáním příkazu `azure vm quick-create` a připravte se na výzvy.</span><span class="sxs-lookup"><span data-stu-id="71128-229">Just create your VM by entering the `azure vm quick-create` command and being ready for the prompts.</span></span> <span data-ttu-id="71128-230">Výsledek by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="71128-230">It should look something like this:</span></span>

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
+ Looking up the VM "coreos"
info:    Using the VM Size "Standard_A1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Retrieving storage accounts
info:    Could not find any storage accounts in the region "westus", trying to create new one
+ Creating storage account "cli9fd3fce49e9a9b3d14302" in "westus"
+ Looking up the storage account cli9fd3fce49e9a9b3d14302
+ Looking up the NIC "coreo-westu-1430261891570-nic"
info:    An nic with given name "coreo-westu-1430261891570-nic" not found, creating a new one
+ Looking up the virtual network "coreo-westu-1430261891570-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "coreo-westu-1430261891570-vnet" [address prefix: "10.0.0.0/16"] with subnet "coreo-westu-1430261891570-sne+" [address prefix: "10.0.1.0/24"]
+ Looking up the virtual network "coreo-westu-1430261891570-vnet"
+ Looking up the subnet "coreo-westu-1430261891570-snet" under the virtual network "coreo-westu-1430261891570-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "coreo-westu-1430261891570-pip"
info:    PublicIP with given name "coreo-westu-1430261891570-pip" not found, creating a new one
+ Creating public ip "coreo-westu-1430261891570-pip"
+ Looking up the public ip "coreo-westu-1430261891570-pip"
+ Creating NIC "coreo-westu-1430261891570-nic"
+ Looking up the NIC "coreo-westu-1430261891570-nic"
+ Creating VM "coreos"
+ Looking up the VM "coreos"
+ Looking up the NIC "coreo-westu-1430261891570-nic"
+ Looking up the public ip "coreo-westu-1430261891570-pip"
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

<span data-ttu-id="71128-231">A váš nový virtuální počítač je připravený.</span><span class="sxs-lookup"><span data-stu-id="71128-231">And away you go with your new VM.</span></span>

## <span data-ttu-id="71128-232"><a id="deploy-a-vm-in-azure-from-a-template"></a>Úkol: Nasazení virtuálního počítače v Azure ze šablony</span><span class="sxs-lookup"><span data-stu-id="71128-232"><a id="deploy-a-vm-in-azure-from-a-template"></a>Task: Deploy a VM in Azure from a template</span></span>
<span data-ttu-id="71128-233">Pokyny v těchto odstavcích použijte pro nasazení nového virtuálního počítače Azure ze šablony pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="71128-233">Use the instructions in these sections to deploy a new Azure VM by using a template with the Azure CLI.</span></span> <span data-ttu-id="71128-234">Tato šablona vytvoří jeden virtuální počítač v nové virtuální síti s jedinou podsítí a na rozdíl od příkazu `azure vm quick-create` umožňuje popsat, co přesně chcete, a zopakovat to bez chyb.</span><span class="sxs-lookup"><span data-stu-id="71128-234">This template creates a single virtual machine in a new virtual network with a single subnet, and unlike `azure vm quick-create`, enables you to describe what you want precisely and repeat it without errors.</span></span> <span data-ttu-id="71128-235">Tato šablona vytvoří tohle:</span><span class="sxs-lookup"><span data-stu-id="71128-235">Here's what this template creates:</span></span>

![](./media/virtual-machines-common-cli-deploy-templates/new-vm.png)

### <a name="step-1-examine-the-json-file-for-the-template-parameters"></a><span data-ttu-id="71128-236">Krok 1: Prohlídka parametrů šablony v souboru JSON</span><span class="sxs-lookup"><span data-stu-id="71128-236">Step 1: Examine the JSON file for the template parameters</span></span>
<span data-ttu-id="71128-237">Obsah souboru JSON pro příslušnou šablonu vypadá takto.</span><span class="sxs-lookup"><span data-stu-id="71128-237">Here are the contents of the JSON file for the template.</span></span> <span data-ttu-id="71128-238">(Tato šablona je také umístěná na [GitHubu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json).)</span><span class="sxs-lookup"><span data-stu-id="71128-238">(The template is also located in [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json).)</span></span>

<span data-ttu-id="71128-239">Šablony jsou flexibilní, takže se návrhář může rozhodnout, jestli vám zpřístupní spoustu parametrů, nebo nabídne jenom několik variant a vytvoří šablonu, která je pevněji daná.</span><span class="sxs-lookup"><span data-stu-id="71128-239">Templates are flexible, so the designer may have chosen to give you lots of parameters or chosen to offer only a few by creating a template that is more fixed.</span></span> <span data-ttu-id="71128-240">Abyste shromáždili informace, které musíte šabloně předat jako parametry, otevřete soubor šablony (v tomto tématu je obsah šablony uvedený dál) a prohlédněte si hodnoty **parametrů**.</span><span class="sxs-lookup"><span data-stu-id="71128-240">In order to collect the information you need to pass the template as parameters, open the template file (this topic has a template inline, below) and examine the **parameters** values.</span></span>

<span data-ttu-id="71128-241">V tomto případě vás šablona vyzve k zadání těchto údajů:</span><span class="sxs-lookup"><span data-stu-id="71128-241">In this case, the template below will ask for:</span></span>

* <span data-ttu-id="71128-242">Jedinečný název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="71128-242">A unique storage account name.</span></span>
* <span data-ttu-id="71128-243">Uživatelské jméno správce pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="71128-243">An admin user name for the VM.</span></span>
* <span data-ttu-id="71128-244">Heslo.</span><span class="sxs-lookup"><span data-stu-id="71128-244">A password.</span></span>
* <span data-ttu-id="71128-245">Název domény pro okolní svět.</span><span class="sxs-lookup"><span data-stu-id="71128-245">A domain name for the outside world to use.</span></span>
* <span data-ttu-id="71128-246">Číslo verze Ubuntu Serveru (ale přijme jenom jednu z hodnot v seznamu).</span><span class="sxs-lookup"><span data-stu-id="71128-246">An Ubuntu Server version number -- but it will accept only one of a list.</span></span>

<span data-ttu-id="71128-247">Přečtěte si další informace o [požadavcích na uživatelské jméno a heslo](../articles/virtual-machines/linux/faq.md#what-are-the-username-requirements-when-creating-a-vm).</span><span class="sxs-lookup"><span data-stu-id="71128-247">See more about [username and password requirements](../articles/virtual-machines/linux/faq.md#what-are-the-username-requirements-when-creating-a-vm).</span></span>

<span data-ttu-id="71128-248">Jakmile se o těchto hodnotách rozhodnete, jste připraveni vytvořit skupinu a nasadit tuto šablonu do předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="71128-248">Once you decide on these values, you're ready to create a group for and deploy this template into your Azure subscription.</span></span>

```json
{
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": {
    "newStorageAccountName": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for the storage account where the virtual machine's disks will be placed."
    }
    },
    "adminUsername": {
    "type": "string",
    "metadata": {
        "description": "User name for the virtual machine."
    }
    },
    "adminPassword": {
    "type": "securestring",
    "metadata": {
        "description": "Password for the virtual machine."
    }
    },
    "dnsNameForPublicIP": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for the public IP used to access the virtual machine."
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
        "description": "The Ubuntu version for the VM. This will pick a fully patched image of this given Ubuntu version. Allowed values: 12.04.5-LTS, 14.04.2-LTS, 15.04."
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

### <a name="step-2-create-the-virtual-machine-by-using-the-template"></a><span data-ttu-id="71128-249">Krok 2: Vytvoření virtuálního počítače pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="71128-249">Step 2: Create the virtual machine by using the template</span></span>
<span data-ttu-id="71128-250">Až budete mít všechny hodnoty parametrů připravené, musíte pro nasazení šablony vytvořit skupinu prostředků a potom šablonu nasadit.</span><span class="sxs-lookup"><span data-stu-id="71128-250">Once you have your parameter values ready, you must create a resource group for your template deployment and then deploy the template.</span></span>

<span data-ttu-id="71128-251">Pro vytvoření skupiny prostředků zadejte příkaz `azure group create <group name> <location>` s požadovaným názvem skupiny a umístěním datacentra, do kterého chcete nasazovat.</span><span class="sxs-lookup"><span data-stu-id="71128-251">To create the resource group, type `azure group create <group name> <location>` with the name of the group you want and the datacenter location into which you want to deploy.</span></span> <span data-ttu-id="71128-252">Akce proběhne rychle:</span><span class="sxs-lookup"><span data-stu-id="71128-252">This happens quickly:</span></span>

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

<span data-ttu-id="71128-253">Teď chcete vytvořit nasazení. Použijte příkaz `azure group deployment create` a předejte mu:</span><span class="sxs-lookup"><span data-stu-id="71128-253">Now to create the deployment, call `azure group deployment create` and pass:</span></span>

* <span data-ttu-id="71128-254">Soubor šablony (pokud jste výše uvedenou šablonu JSON uložili do místního souboru).</span><span class="sxs-lookup"><span data-stu-id="71128-254">The template file (if you saved the above JSON template to a local file).</span></span>
* <span data-ttu-id="71128-255">Identifikátor URI šablony (pokud chcete odkazovat na soubor na Githubu nebo jiné webové adrese).</span><span class="sxs-lookup"><span data-stu-id="71128-255">A template URI (if you want to point at the file in GitHub or some other web address).</span></span>
* <span data-ttu-id="71128-256">Skupinu prostředků, do které chcete nasazovat.</span><span class="sxs-lookup"><span data-stu-id="71128-256">The resource group into which you want to deploy.</span></span>
* <span data-ttu-id="71128-257">Volitelný název nasazení.</span><span class="sxs-lookup"><span data-stu-id="71128-257">An optional deployment name.</span></span>

<span data-ttu-id="71128-258">K zadání hodnot parametrů vás vyzve oddíl parameters souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="71128-258">You will be prompted to supply the values of parameters in the "parameters" section of the JSON file.</span></span> <span data-ttu-id="71128-259">Až zadáte všechny hodnoty parametrů, zahájí se proces nasazení.</span><span class="sxs-lookup"><span data-stu-id="71128-259">When you have specified all the parameter values, your deployment will begin.</span></span>

<span data-ttu-id="71128-260">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="71128-260">Here is an example:</span></span>

```azurecli
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json myResourceGroup firstDeployment
info:    Executing command group deployment create
info:    Supply values for the following parameters
newStorageAccountName: storageaccount
adminUsername: ops
adminPassword: password
dnsNameForPublicIP: newdomainname
```

<span data-ttu-id="71128-261">Získáte tento typ informací:</span><span class="sxs-lookup"><span data-stu-id="71128-261">You will receive the following type of information:</span></span>

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "firstDeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment to complete
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


## <span data-ttu-id="71128-262"><a id="create-a-custom-vm-image"></a>Úkol: Vytvoření vlastní image virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="71128-262"><a id="create-a-custom-vm-image"></a>Task: Create a custom VM image</span></span>
<span data-ttu-id="71128-263">Se základy využití šablon jste se seznámili v předcházejících krocích, takže teď můžeme podobné pokyny využít k vytvoření vlastního virtuálního počítače z konkrétního souboru .vhd v Azure pomocí šablony s využitím rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="71128-263">You've seen the basic usage of templates above, so now we can use similar instructions to create a custom VM from a specific .vhd file in Azure by using a template via the Azure CLI.</span></span> <span data-ttu-id="71128-264">Rozdíl je v tom, že tato šablona vytvoří jeden virtuální počítač ze zadaného virtuálního pevného disku (VHD).</span><span class="sxs-lookup"><span data-stu-id="71128-264">The difference here is that this template creates a single virtual machine from a specified virtual hard disk (VHD).</span></span>

### <a name="step-1-examine-the-json-file-for-the-template"></a><span data-ttu-id="71128-265">Krok 1: Prohlídka šablony v souboru JSON</span><span class="sxs-lookup"><span data-stu-id="71128-265">Step 1: Examine the JSON file for the template</span></span>
<span data-ttu-id="71128-266">Tady je obsah souboru JSON pro šablonu, která je v této části použitá jako příklad.</span><span class="sxs-lookup"><span data-stu-id="71128-266">Here are the contents of the JSON file for the template that this section uses as an example.</span></span> <span data-ttu-id="71128-267">(Tato šablona je také umístěná na [GitHubu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).)</span><span class="sxs-lookup"><span data-stu-id="71128-267">(The template is also located in [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).)</span></span>

<span data-ttu-id="71128-268">Znovu musíte zjistit, jaké hodnoty chcete použít pro parametry, které nemají výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="71128-268">Again, you will need to find the values you want to enter for the parameters that do not have default values.</span></span> <span data-ttu-id="71128-269">Po spuštění příkazu `azure group deployment create` vás rozhraní příkazového řádku Azure vyzve k zadání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="71128-269">When you run the `azure group deployment create` command, the Azure CLI will prompt you to enter those values.</span></span>

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

### <a name="step-2-obtain-the-vhd"></a><span data-ttu-id="71128-270">Krok 2: Získání virtuálního pevného disku (VHD)</span><span class="sxs-lookup"><span data-stu-id="71128-270">Step 2: Obtain the VHD</span></span>
<span data-ttu-id="71128-271">Pro tyto účely budete samozřejmě potřebovat soubor .vhd.</span><span class="sxs-lookup"><span data-stu-id="71128-271">Obviously, you'll need a .vhd for this.</span></span> <span data-ttu-id="71128-272">Můžete použít ten, který už máte v Azure, nebo nějaký nahrát.</span><span class="sxs-lookup"><span data-stu-id="71128-272">You can use one you already have in Azure, or you can upload one.</span></span>

<span data-ttu-id="71128-273">Pro virtuální počítač s Windows najdete informace v tématu věnovaném [vytvoření a nahrání VHD s Windows Serverem do Azure](../articles/virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="71128-273">For a Windows-based virtual machine, see [Create and upload a Windows Server VHD to Azure](../articles/virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="71128-274">Pro virtuální počítač s Linuxem najdete informace v tématu [Vytvoření a nahrání virtuálního pevného disku obsahujícího operační systém Linux](../articles/virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="71128-274">For a Linux-based virtual machine, see [Creating and uploading a virtual hard disk that contains the Linux operating system](../articles/virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

### <a name="step-3-create-the-virtual-machine-by-using-the-template"></a><span data-ttu-id="71128-275">Krok 3: Vytvoření virtuálního počítače pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="71128-275">Step 3: Create the virtual machine by using the template</span></span>
<span data-ttu-id="71128-276">Teď jste připravení vytvořit na základě tohoto souboru .vhd nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="71128-276">Now you're ready to create a new virtual machine based on the .vhd.</span></span> <span data-ttu-id="71128-277">Pomocí příkazu `azure group create <location>` vytvořte skupinu, do které se provede nasazení:</span><span class="sxs-lookup"><span data-stu-id="71128-277">Create a group to deploy into, by using `azure group create <location>`:</span></span>

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

<span data-ttu-id="71128-278">Potom vytvořte nasazení použitím možnosti `--template-uri` pro přímé volání šablony (nebo můžete použít možnost `--template-file` a soubor, který jste uložili místně).</span><span class="sxs-lookup"><span data-stu-id="71128-278">Then create the deployment by using the `--template-uri` option to call in the template directly (or you can use the `--template-file` option to use a file that you have saved locally).</span></span> <span data-ttu-id="71128-279">Všimněte si, že vzhledem k tomu, že šablona má zadané výchozí hodnoty, zobrazí se jenom pár výzev k zadání.</span><span class="sxs-lookup"><span data-stu-id="71128-279">Note that because the template has defaults specified, you are prompted for only a few things.</span></span> <span data-ttu-id="71128-280">Pokud šablonu nasazujete na různých místech, může se stát, že u výchozích hodnot dojde ke kolizím pojmenování (hlavně u názvu DNS, který vytvoříte).</span><span class="sxs-lookup"><span data-stu-id="71128-280">If you deploy the template in different places, you may find that some naming collisions occur with the default values (particularly the DNS name you create).</span></span>

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json \
> myResourceGroup \
> customVhdDeployment
info:    Executing command group deployment create
info:    Supply values for the following parameters
adminUserName: ops
adminPassword: password
osType: linux
subscriptionId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

<span data-ttu-id="71128-281">Výstup by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="71128-281">Output looks something like the following:</span></span>

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "customVhdDeployment"
+ Registering providers
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment to complete
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

## <span data-ttu-id="71128-282"><a id="deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a>Úloha: Nasazení aplikace s více virtuálními počítači, která používá virtuální síť a externí nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="71128-282"><a id="deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a>Task: Deploy a multi-VM application that uses a virtual network and an external load balancer</span></span>
<span data-ttu-id="71128-283">Tato šablona umožňuje vytvořit dva virtuální počítače s nástrojem pro vyrovnávání zatížení a nakonfigurovat pravidlo vyrovnávání zatížení na portu 80.</span><span class="sxs-lookup"><span data-stu-id="71128-283">This template allows you to create two virtual machines under a load balancer and configure a load-balancing rule on Port 80.</span></span> <span data-ttu-id="71128-284">Tato šablona také nasadí účet úložiště, virtuální síť, veřejnou IP adresu, skupinu dostupnosti a síťová rozhraní.</span><span class="sxs-lookup"><span data-stu-id="71128-284">This template also deploys a storage account, virtual network, public IP address, availability set, and network interfaces.</span></span>

![](./media/virtual-machines-common-cli-deploy-templates/multivmextlb.png)

<span data-ttu-id="71128-285">Pomocí těchto kroků nasadíte aplikaci s více virtuálními počítači, která používá virtuální síť a nástroj pro vyrovnávání zatížení, a to využitím šablony Resource Manageru v úložišti šablon GitHub pomocí příkazů prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="71128-285">Follow these steps to deploy a multi-VM application that uses a virtual network and a load balancer by using a Resource Manager template in the GitHub template repository via Azure PowerShell commands.</span></span>

### <a name="step-1-examine-the-json-file-for-the-template"></a><span data-ttu-id="71128-286">Krok 1: Prohlídka šablony v souboru JSON</span><span class="sxs-lookup"><span data-stu-id="71128-286">Step 1: Examine the JSON file for the template</span></span>
<span data-ttu-id="71128-287">Obsah souboru JSON pro příslušnou šablonu vypadá takto.</span><span class="sxs-lookup"><span data-stu-id="71128-287">Here are the contents of the JSON file for the template.</span></span> <span data-ttu-id="71128-288">Pokud chcete, aby nejnovější verzi, ho má nachází [v úložišti GitHub pro šablony](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="71128-288">If you want the most recent version, it's located [at the GitHub repository for templates](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json).</span></span> <span data-ttu-id="71128-289">Toto téma používá k volání šablony přepínač `--template-uri`, ale můžete také použít přepínač `--template-file` a předat místní verzi.</span><span class="sxs-lookup"><span data-stu-id="71128-289">This topic uses the `--template-uri` switch to call in the template, but you can also use the `--template-file` switch to pass a local version.</span></span>

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
                "description": "Prefix to use for VM names"
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
                "description": "Size of the VM"
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

### <a name="step-2-create-the-deployment-by-using-the-template"></a><span data-ttu-id="71128-290">Krok 2: Vytvoření nasazení pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="71128-290">Step 2: Create the deployment by using the template</span></span>
<span data-ttu-id="71128-291">Pomocí `azure group create <location>` vytvořte pro šablonu skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="71128-291">Create a resource group for the template by using `azure group create <location>`.</span></span> <span data-ttu-id="71128-292">Potom vytvořte nasazení do této skupiny prostředků. Použijte příkaz `azure group deployment create`, předejte skupinu prostředků a název nasazení a odpovězte na výzvy k zadání parametrů šablony, které nemají výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="71128-292">Then, create a deployment into that resource group by using `azure group deployment create` and passing the resource group, passing a deployment name, and answering the prompts for parameters in the template that did not have default values.</span></span>

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

<span data-ttu-id="71128-293">Potom k nasazení šablony použijte příkaz `azure group deployment create` s možností `--template-uri`.</span><span class="sxs-lookup"><span data-stu-id="71128-293">Now use the `azure group deployment create` command and the `--template-uri` option to deploy the template.</span></span> <span data-ttu-id="71128-294">Připravte si hodnoty parametrů a po zobrazení příslušných výzev je zadejte, jak znázorňuje následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="71128-294">Be ready with your parameter values when it prompts you, as shown below.</span></span>

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json \
> lbgroup \
> newdeployment
info:    Executing command group deployment create
info:    Supply values for the following parameters
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
+ Waiting for deployment to complete
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

<span data-ttu-id="71128-295">Všimněte si, že tato šablona nasadí image Windows Serveru. Můžete ji ale snadno nahradit libovolnou linuxovou imagí.</span><span class="sxs-lookup"><span data-stu-id="71128-295">Note that this template deploys a Windows Server image; however, it could easily be replaced by any Linux image.</span></span> <span data-ttu-id="71128-296">Chcete vytvořit cluster Dockeru s několika správci Swarm?</span><span class="sxs-lookup"><span data-stu-id="71128-296">Want to create a Docker cluster with multiple swarm managers?</span></span> <span data-ttu-id="71128-297">[Můžete](https://azure.microsoft.com/documentation/templates/docker-swarm-cluster/).</span><span class="sxs-lookup"><span data-stu-id="71128-297">[You can do it](https://azure.microsoft.com/documentation/templates/docker-swarm-cluster/).</span></span>

## <span data-ttu-id="71128-298"><a id="remove-a-resource-group"></a>Úkol: Odebrání skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="71128-298"><a id="remove-a-resource-group"></a>Task: Remove a resource group</span></span>
<span data-ttu-id="71128-299">Mějte na paměti, že do skupiny prostředků je možné znovu provádět nasazení, ale pokud jste s ní hotoví, můžete ji odstranit pomocí `azure group delete <group name>`.</span><span class="sxs-lookup"><span data-stu-id="71128-299">Remember that you can redeploy to a resource group, but if you are done with one, you can delete it by using `azure group delete <group name>`.</span></span>

```azurecli
azure group delete myResourceGroup
info:    Executing command group delete
Delete resource group myResourceGroup? [y/n] y
+ Deleting resource group myResourceGroup
info:    group delete command OK
```

## <span data-ttu-id="71128-300"><a id="show-the-log-for-a-resource-group-deployment"></a>Úkol: Zobrazení protokolu pro nasazení skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="71128-300"><a id="show-the-log-for-a-resource-group-deployment"></a>Task: Show the log for a resource group deployment</span></span>
<span data-ttu-id="71128-301">Tento úkol je obvyklý při vytváření nebo používání šablon.</span><span class="sxs-lookup"><span data-stu-id="71128-301">This one is common while you're creating or using templates.</span></span> <span data-ttu-id="71128-302">K zobrazení protokolů nasazení pro skupinu se použije volání `azure group log show <groupname>`. Zobrazí poměrně hodně informací, které jsou užitečné ke zjištění, proč se něco stalo, nebo nestalo.</span><span class="sxs-lookup"><span data-stu-id="71128-302">The call to display the deployment logs for a group is `azure group log show <groupname>`, which displays quite a bit of information that's useful for understanding why something happened -- or didn't.</span></span> <span data-ttu-id="71128-303">(Další informace o řešení potíží s nasazeními a také další informace o problémech najdete v tématu [Řešení chyb nasazení v Azure pomocí Azure Resource Manageru](../articles/azure-resource-manager/resource-manager-common-deployment-errors.md).)</span><span class="sxs-lookup"><span data-stu-id="71128-303">(For more information on troubleshooting your deployments, as well as other information about issues, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](../articles/azure-resource-manager/resource-manager-common-deployment-errors.md).)</span></span>

<span data-ttu-id="71128-304">Pokud se zaměřujete na konkrétní selhání, můžete použít nástroje, jako je **jq**, podívat se na věci víc zblízka a zjistit třeba, která jednotlivá selhání je potřeba napravit.</span><span class="sxs-lookup"><span data-stu-id="71128-304">To target specific failures, for example, you might use tools like **jq** to query things a bit more precisely, such as which individual failures you need to correct.</span></span> <span data-ttu-id="71128-305">Následující příklad využívá **jq** k analýze protokolu nasazení pro **lbgroup** a hledá selhání.</span><span class="sxs-lookup"><span data-stu-id="71128-305">The following example uses **jq** to parse a deployment log for **lbgroup**, looking for failures.</span></span>

```azurecli
azure group log show lbgroup -l --json | jq '.[] | select(.status.value == "Failed") | .properties'
```
<span data-ttu-id="71128-306">Umožňuje rychle zjistit, co se nepovedlo, opravit to a zkusit znovu.</span><span class="sxs-lookup"><span data-stu-id="71128-306">You can discover very quickly what went wrong, fix, and retry.</span></span> <span data-ttu-id="71128-307">V následujícím případě šablona vytvářela dva virtuální počítače ve stejnou dobu. Výsledkem byl zámek na souboru .vhd.</span><span class="sxs-lookup"><span data-stu-id="71128-307">In the following case, the template had been creating two VMs at the same time, which created a lock on the .vhd.</span></span> <span data-ttu-id="71128-308">(Po příslušné úpravě šablony se nasazení rychle povedlo.)</span><span class="sxs-lookup"><span data-stu-id="71128-308">(After we modified the template, the deployment succeeded quickly.)</span></span>

```json
{
    "statusCode": "Conflict",
    "statusMessage": "{\"status\":\"Failed\",\"error\":{\"code\":\"ResourceDeploymentFailure\",\"message\":\"The resource operation completed with terminal provisioning state 'Failed'.\",\"details\":[{\"code\":\"AcquireDiskLeaseFailed\",\"message\":\"Failed to acquire lease while creating disk 'osdisk' using blob with URI http://storage.blob.core.windows.net/vhds/osdisk.vhd.\"}]}}"
}
```

## <span data-ttu-id="71128-309"><a id="display-information-about-a-virtual-machine"></a>Úkol: Zobrazení informací o virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="71128-309"><a id="display-information-about-a-virtual-machine"></a>Task: Display information about a virtual machine</span></span>
<span data-ttu-id="71128-310">Informace o konkrétním virtuálním počítači ve skupině prostředků můžete zobrazit pomocí příkazu `azure vm show <groupname> <vmname>`.</span><span class="sxs-lookup"><span data-stu-id="71128-310">You can see information about specific VMs in your resource group by using the `azure vm show <groupname> <vmname>` command.</span></span> <span data-ttu-id="71128-311">Pokud máte ve skupině víc než jeden virtuální počítač, můžete nejdřív zobrazit seznam virtuálních počítačů ve skupině pomocí příkazu `azure vm list <groupname>`.</span><span class="sxs-lookup"><span data-stu-id="71128-311">If you have more than one VM in your group, you might first need to list the VMs in a group by using `azure vm list <groupname>`.</span></span>

```azurecli
azure vm list zoo
info:    Executing command vm list
+ Getting virtual machines
data:    Name   ProvisioningState  Location  Size
data:    -----  -----------------  --------  -----------
data:    myVM0  Succeeded          westus    Standard_A1
data:    myVM1  Failed             westus    Standard_A1
```

<span data-ttu-id="71128-312">A potom vyhledat počítač myVM1:</span><span class="sxs-lookup"><span data-stu-id="71128-312">And then, looking up myVM1:</span></span>

```azurecli
azure vm show zoo myVM1
info:    Executing command vm show
+ Looking up the VM "myVM1"
+ Looking up the NIC "nic1"
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
> <span data-ttu-id="71128-313">Pokud chcete prostřednictvím kódu programu uložit výstup příkazů konzoly a zpracovat je, můžete využít třeba nástroj pro analýzu JSON, jako je  **[jq](https://github.com/stedolan/jq)**  nebo  **[jsawk](https://github.com/micha/jsawk)**, nebo knihovny jazyků, které jsou pro tyto účely vhodné.</span><span class="sxs-lookup"><span data-stu-id="71128-313">If you want to programmatically store and manipulate the output of your console commands, you may want to use a JSON parsing tool such as **[jq](https://github.com/stedolan/jq)** or **[jsawk](https://github.com/micha/jsawk)**, or language libraries that are good for the task.</span></span>
>
>

## <span data-ttu-id="71128-314"><a id="log-on-to-a-linux-based-virtual-machine"></a>Úkol: Připojení k virtuálnímu počítači s Linuxem</span><span class="sxs-lookup"><span data-stu-id="71128-314"><a id="log-on-to-a-linux-based-virtual-machine"></a>Task: Log on to a Linux-based virtual machine</span></span>
<span data-ttu-id="71128-315">K počítačům se systémem Linux se obvykle připojuje prostřednictvím SSH.</span><span class="sxs-lookup"><span data-stu-id="71128-315">Typically Linux machines are connected to through SSH.</span></span> <span data-ttu-id="71128-316">Další informace najdete v tématu [Jak použít SSH s Linuxem v Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="71128-316">For more information, see [How to use SSH with Linux on Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <span data-ttu-id="71128-317"><a id="stop-a-virtual-machine"></a>Úkol: Zastavení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="71128-317"><a id="stop-a-virtual-machine"></a>Task: Stop a VM</span></span>
<span data-ttu-id="71128-318">Spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="71128-318">Run this command:</span></span>

```azurecli
azure vm stop <group name> <virtual machine name>
```

> [!IMPORTANT]
> <span data-ttu-id="71128-319">Tento parametr použijte k uchování virtuální IP adresy (VIP) virtuální sítě pro případ, že se jedná o poslední virtuální počítač v této síti.</span><span class="sxs-lookup"><span data-stu-id="71128-319">Use this parameter to keep the virtual IP (VIP) of the vnet in case it's the last VM in that vnet.</span></span> <br><br> <span data-ttu-id="71128-320">Pokud použijete parametr `StayProvisioned`, bude se vám tento virtuální počítač nadále účtovat.</span><span class="sxs-lookup"><span data-stu-id="71128-320">If you use the `StayProvisioned` parameter, you'll still be billed for the VM.</span></span>
>
>

## <span data-ttu-id="71128-321"><a id="start-a-virtual-machine"></a>Úkol: Spuštění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="71128-321"><a id="start-a-virtual-machine"></a>Task: Start a VM</span></span>
<span data-ttu-id="71128-322">Spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="71128-322">Run this command:</span></span>

```azurecli
azure vm start <group name> <virtual machine name>
```

## <span data-ttu-id="71128-323"><a id="attach-a-data-disk"></a>Úkol: Připojení datového disku</span><span class="sxs-lookup"><span data-stu-id="71128-323"><a id="attach-a-data-disk"></a>Task: Attach a data disk</span></span>
<span data-ttu-id="71128-324">Musíte se také rozhodnout, jestli se má připojit nový disk, nebo disk, který obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="71128-324">You'll also need to decide whether to attach a new disk or one that contains data.</span></span> <span data-ttu-id="71128-325">V případě nového disku tento příkaz vytvoří soubor .vhd a rovnou ho i připojí.</span><span class="sxs-lookup"><span data-stu-id="71128-325">For a new disk, the command creates the .vhd file and attaches it in the same command.</span></span>

<span data-ttu-id="71128-326">Pokud chcete připojit nový disk, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="71128-326">To attach a new disk, run this command:</span></span>

```azurecli
    azure vm disk attach-new <resource-group> <vm-name> <size-in-gb>
```

<span data-ttu-id="71128-327">Pokud chcete připojit stávající datový disk, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="71128-327">To attach an existing data disk, run this command:</span></span>

```azurecli
azure vm disk attach <resource-group> <vm-name> [vhd-url]
```

<span data-ttu-id="71128-328">Pak bude potřeba disk připojit běžným způsobem, který v Linuxu používáte.</span><span class="sxs-lookup"><span data-stu-id="71128-328">Then you'll need to mount the disk, as you normally would in Linux.</span></span>

## <a name="next-steps"></a><span data-ttu-id="71128-329">Další kroky</span><span class="sxs-lookup"><span data-stu-id="71128-329">Next steps</span></span>
<span data-ttu-id="71128-330">Další příklady použití rozhraní příkazového řádku Azure s režimem **arm** najdete v tématu věnovaném [použití rozhraní příkazového řádku Azure pro Mac, Linux a Windows s Azure Resource Managerem](../articles/xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="71128-330">For far more examples of Azure CLI usage with the **arm** mode, see [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](../articles/xplat-cli-azure-resource-manager.md).</span></span> <span data-ttu-id="71128-331">Další informace o prostředcích Azure a jejich konceptech najdete v [přehledu Azure Resource Manageru](../articles/azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="71128-331">To learn more about Azure resources and their concepts, see [Azure Resource Manager overview](../articles/azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="71128-332">Další šablony, které můžete použít, najdete v tématech věnovaných [rychlému úvodu do šablon pro Azure](https://azure.microsoft.com/documentation/templates/) a [aplikačním architekturám využívajícím šablony](../articles/virtual-machines/linux/app-frameworks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="71128-332">For more templates you can use, see [Azure Quickstart templates](https://azure.microsoft.com/documentation/templates/) and [Application frameworks using templates](../articles/virtual-machines/linux/app-frameworks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
