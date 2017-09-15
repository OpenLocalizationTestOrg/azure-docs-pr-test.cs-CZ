# <a name="scale-agent-nodes-in-a-container-service-cluster"></a><span data-ttu-id="0e6ad-101">Škálování uzlů agentů v clusteru Container Service</span><span class="sxs-lookup"><span data-stu-id="0e6ad-101">Scale agent nodes in a Container Service cluster</span></span>
<span data-ttu-id="0e6ad-102">Po [nasazení clusteru Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md) možná budete potřebovat změnit počet uzlů agentů.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-102">After [deploying an Azure Container Service cluster](../articles/container-service/dcos-swarm/container-service-deployment.md), you might need to change the number of agent nodes.</span></span> <span data-ttu-id="0e6ad-103">Například můžete potřebovat přidat více agentů, abyste mohli spouštět více instancí nebo aplikací typu kontejner.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-103">For example, you might need more agents so you can run more container applications or instances.</span></span> 

<span data-ttu-id="0e6ad-104">Počet uzlů agentů v clusteru DC/OS, Docker Swarm nebo Kubernetes můžete změnit na webu Azure Portal nebo pomocí Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-104">You can change the number of agent nodes in a DC/OS, Docker Swarm, or Kubernetes cluster by using the Azure portal or the Azure CLI 2.0.</span></span> 

## <a name="scale-with-the-azure-portal"></a><span data-ttu-id="0e6ad-105">Škálování pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0e6ad-105">Scale with the Azure portal</span></span>

1. <span data-ttu-id="0e6ad-106">Na webu [Azure Portal](https://portal.azure.com) přejděte na **Služby kontejneru** a klikněte na službu kontejneru, kterou chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-106">In the [Azure portal](https://portal.azure.com), browse for **Container services**, and then click the container service that you want to modify.</span></span>
2. <span data-ttu-id="0e6ad-107">V okně **Služba kontejneru** klikněte na **Agenti**.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-107">In the **Container service** blade, click **Agents**.</span></span>
3. <span data-ttu-id="0e6ad-108">Do pole **Počet virtuálních počítačů** zadejte požadovaný počet uzlů agentů.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-108">In **VM Count**, enter the desired number of agents nodes.</span></span>

    ![Škálování fondu na portálu](./media/container-service-scale/container-service-scale-portal.png)

4. <span data-ttu-id="0e6ad-110">Konfiguraci uložíte kliknutím na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-110">To save the configuration, click **Save**.</span></span>

## <a name="scale-with-the-azure-cli-20"></a><span data-ttu-id="0e6ad-111">Škálování pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0e6ad-111">Scale with the Azure CLI 2.0</span></span>

<span data-ttu-id="0e6ad-112">Ujistěte se, že jste [nainstalovali](/cli/azure/install-az-cli2) nejnovější verzi Azure CLI 2.0 a jste přihlášení k účtu Azure (`az login`).</span><span class="sxs-lookup"><span data-stu-id="0e6ad-112">Make sure that you [installed](/cli/azure/install-az-cli2) the latest Azure CLI 2.0 and logged in to an azure account (`az login`).</span></span>

### <a name="see-the-current-agent-count"></a><span data-ttu-id="0e6ad-113">Zobrazení aktuálního počtu agentů</span><span class="sxs-lookup"><span data-stu-id="0e6ad-113">See the current agent count</span></span>
<span data-ttu-id="0e6ad-114">Pokud chcete zobrazit aktuální počet agentů v clusteru, spusťte příkaz `az acs show`.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-114">To see the number of agents currently in the cluster, run the `az acs show` command.</span></span> <span data-ttu-id="0e6ad-115">Zobrazí se konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-115">This shows the cluster configuration.</span></span> <span data-ttu-id="0e6ad-116">Například následující příkaz zobrazí konfiguraci služby kontejneru `containerservice-myACSName` ve skupině prostředků `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="0e6ad-116">For example, the following command shows the configuration of the container service named `containerservice-myACSName` in the resource group `myResourceGroup`:</span></span>

```azurecli
az acs show -g myResourceGroup -n containerservice-myACSName
```

<span data-ttu-id="0e6ad-117">Příkaz vrátí počet agentů v hodnotě `Count` v části `AgentPoolProfiles`.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-117">The command returns the number of agents in the `Count` value under `AgentPoolProfiles`.</span></span>

### <a name="use-the-az-acs-scale-command"></a><span data-ttu-id="0e6ad-118">Použití příkazu az acs scale</span><span class="sxs-lookup"><span data-stu-id="0e6ad-118">Use the az acs scale command</span></span>
<span data-ttu-id="0e6ad-119">Pokud chcete změnit počet uzlů agentů, spusťte příkaz `az acs scale` a zadejte **skupinu prostředků**, **název služby kontejneru** a požadovaný **nový počet agentů**.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-119">To change the number of agent nodes, run the `az acs scale` command and supply the **resource group**, **container service name**, and the desired **new agent count**.</span></span> <span data-ttu-id="0e6ad-120">Použitím nižšího nebo vyššího čísla můžete vertikálně snížit nebo navýšit kapacitu.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-120">By using a smaller or higher number, you can scale down or up, respectively.</span></span>

<span data-ttu-id="0e6ad-121">Pokud například chcete změnit počet agentů v předchozím clusteru na 10, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0e6ad-121">For example, to change the number of agents in the previous cluster to 10, type the following command:</span></span>

```azurecli
az acs scale -g myResourceGroup -n containerservice-myACSName --new-agent-count 10
```

<span data-ttu-id="0e6ad-122">Azure CLI 2.0 vrátí řetězec JSON představující novou konfiguraci služby kontejneru, včetně nového počtu agentů.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-122">The Azure CLI 2.0 returns a JSON string representing the new configuration of the container service, including the new agent count.</span></span>

<span data-ttu-id="0e6ad-123">Další možnosti příkazu zobrazíte spuštěním příkazu `az acs scale --help`.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-123">For more command options, run `az acs scale --help`.</span></span>

## <a name="scaling-considerations"></a><span data-ttu-id="0e6ad-124">Důležité informace o škálování</span><span class="sxs-lookup"><span data-stu-id="0e6ad-124">Scaling considerations</span></span>

* <span data-ttu-id="0e6ad-125">Počet uzlů agentů musí být mezi 1 a 100 včetně.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-125">The number of agent nodes must be between 1 and 100, inclusive.</span></span> 

* <span data-ttu-id="0e6ad-126">Vaše kvóta pro jádra může omezovat počet uzlů agentů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-126">Your cores quota can limit the number of agent nodes in a cluster.</span></span>

* <span data-ttu-id="0e6ad-127">Operace škálování uzlů agentů se používají na škálovací sadu virtuálních počítačů Azure, která obsahuje fond agentů.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-127">Agent node scaling operations are applied to an Azure virtual machine scale set that contains the agent pool.</span></span> <span data-ttu-id="0e6ad-128">V clusteru DC/OS se operacemi ukázanými v tomto článku škálují pouze uzly agentů v privátním fondu.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-128">In a DC/OS cluster, only agent nodes in the private pool are scaled by the operations shown in this article.</span></span>

* <span data-ttu-id="0e6ad-129">V závislosti na orchestrátoru, který v clusteru nasadíte, můžete samostatně škálovat počet instancí kontejneru spuštěného v clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-129">Depending on the orchestrator you deploy in your cluster, you can separately scale the number of instances of a container running on the cluster.</span></span> <span data-ttu-id="0e6ad-130">Například v clusteru DC/OS můžete změnit počet instancí aplikace typu kontejner pomocí [uživatelského rozhraní Marathon](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="0e6ad-130">For example, in a DC/OS cluster, use the [Marathon UI](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md) to change the number of instances of a container application.</span></span>

* <span data-ttu-id="0e6ad-131">Automatické škálování uzlů agentů v clusteru Container Service aktuálně není podporováno.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-131">Currently, autoscaling of agent nodes in a container service cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0e6ad-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0e6ad-132">Next steps</span></span>
* <span data-ttu-id="0e6ad-133">Podívejte se na [další příklady](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) použití příkazů Azure CLI 2.0 se službou Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-133">See [more examples](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) of using Azure CLI 2.0 commands with Azure Container Service.</span></span>
* <span data-ttu-id="0e6ad-134">Další informace o [fondech agentů DC/OS](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) ve službě Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="0e6ad-134">Learn more about [DC/OS agent pools](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) in Azure Container Service.</span></span>

