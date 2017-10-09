# <a name="scale-agent-nodes-in-a-container-service-cluster"></a><span data-ttu-id="80368-101">Škálování uzlů agentů v clusteru Container Service</span><span class="sxs-lookup"><span data-stu-id="80368-101">Scale agent nodes in a Container Service cluster</span></span>
<span data-ttu-id="80368-102">Po [nasazení clusteru Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md), může být nutné toochange hello počet uzlů agenta.</span><span class="sxs-lookup"><span data-stu-id="80368-102">After [deploying an Azure Container Service cluster](../articles/container-service/dcos-swarm/container-service-deployment.md), you might need toochange hello number of agent nodes.</span></span> <span data-ttu-id="80368-103">Například můžete potřebovat přidat více agentů, abyste mohli spouštět více instancí nebo aplikací typu kontejner.</span><span class="sxs-lookup"><span data-stu-id="80368-103">For example, you might need more agents so you can run more container applications or instances.</span></span> 

<span data-ttu-id="80368-104">Můžete změnit hello počet agenta uzlů v clusteru DC/OS, Docker Swarm nebo Kubernetes pomocí hello portál Azure nebo hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="80368-104">You can change hello number of agent nodes in a DC/OS, Docker Swarm, or Kubernetes cluster by using hello Azure portal or hello Azure CLI 2.0.</span></span> 

## <a name="scale-with-hello-azure-portal"></a><span data-ttu-id="80368-105">Škálování se hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="80368-105">Scale with hello Azure portal</span></span>

1. <span data-ttu-id="80368-106">V hello [portál Azure](https://portal.azure.com), vyhledejte **služby kontejneru**a potom klikněte na službu kontejneru hello, které chcete toomodify.</span><span class="sxs-lookup"><span data-stu-id="80368-106">In hello [Azure portal](https://portal.azure.com), browse for **Container services**, and then click hello container service that you want toomodify.</span></span>
2. <span data-ttu-id="80368-107">V hello **Container service** okně klikněte na tlačítko **agenti**.</span><span class="sxs-lookup"><span data-stu-id="80368-107">In hello **Container service** blade, click **Agents**.</span></span>
3. <span data-ttu-id="80368-108">V **počet virtuálních počítačů**, zadejte hello požadovaného počtu uzlů agenty.</span><span class="sxs-lookup"><span data-stu-id="80368-108">In **VM Count**, enter hello desired number of agents nodes.</span></span>

    ![Škálování fondu hello portálu](./media/container-service-scale/container-service-scale-portal.png)

4. <span data-ttu-id="80368-110">toosave hello konfiguraci, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="80368-110">toosave hello configuration, click **Save**.</span></span>

## <a name="scale-with-hello-azure-cli-20"></a><span data-ttu-id="80368-111">Škálování se hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="80368-111">Scale with hello Azure CLI 2.0</span></span>

<span data-ttu-id="80368-112">Ujistěte se, že jste [nainstalován](/cli/azure/install-az-cli2) hello nejnovější Azure CLI 2.0 a přihlášení tooan účet azure (`az login`).</span><span class="sxs-lookup"><span data-stu-id="80368-112">Make sure that you [installed](/cli/azure/install-az-cli2) hello latest Azure CLI 2.0 and logged in tooan azure account (`az login`).</span></span>

### <a name="see-hello-current-agent-count"></a><span data-ttu-id="80368-113">V tématu hello aktuální počet agentů</span><span class="sxs-lookup"><span data-stu-id="80368-113">See hello current agent count</span></span>
<span data-ttu-id="80368-114">toosee hello počet agentů aktuálně hello clusteru, spusťte hello `az acs show` příkaz.</span><span class="sxs-lookup"><span data-stu-id="80368-114">toosee hello number of agents currently in hello cluster, run hello `az acs show` command.</span></span> <span data-ttu-id="80368-115">Ukazuje to hello konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="80368-115">This shows hello cluster configuration.</span></span> <span data-ttu-id="80368-116">Například hello následující příkaz ukazuje hello konfigurace hello kontejneru služby s názvem `containerservice-myACSName` ve skupině prostředků hello `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="80368-116">For example, hello following command shows hello configuration of hello container service named `containerservice-myACSName` in hello resource group `myResourceGroup`:</span></span>

```azurecli
az acs show -g myResourceGroup -n containerservice-myACSName
```

<span data-ttu-id="80368-117">příkaz Hello vrátí hello počet agentů v hello `Count` hodnoty v části `AgentPoolProfiles`.</span><span class="sxs-lookup"><span data-stu-id="80368-117">hello command returns hello number of agents in hello `Count` value under `AgentPoolProfiles`.</span></span>

### <a name="use-hello-az-acs-scale-command"></a><span data-ttu-id="80368-118">Použití hello az příkaz škálování služby acs</span><span class="sxs-lookup"><span data-stu-id="80368-118">Use hello az acs scale command</span></span>
<span data-ttu-id="80368-119">toochange hello počet uzlů agenta, spusťte hello `az acs scale` příkazu a dodávky hello **skupiny prostředků**, **název kontejneru služby**a hello potřeby **nový počet agentů**.</span><span class="sxs-lookup"><span data-stu-id="80368-119">toochange hello number of agent nodes, run hello `az acs scale` command and supply hello **resource group**, **container service name**, and hello desired **new agent count**.</span></span> <span data-ttu-id="80368-120">Použitím nižšího nebo vyššího čísla můžete vertikálně snížit nebo navýšit kapacitu.</span><span class="sxs-lookup"><span data-stu-id="80368-120">By using a smaller or higher number, you can scale down or up, respectively.</span></span>

<span data-ttu-id="80368-121">Například toochange hello počet agentů v hello předchozí too10 clusteru, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="80368-121">For example, toochange hello number of agents in hello previous cluster too10, type hello following command:</span></span>

```azurecli
az acs scale -g myResourceGroup -n containerservice-myACSName --new-agent-count 10
```

<span data-ttu-id="80368-122">Hello Azure CLI 2.0 vrátí JSON řetězec představující hello novou konfiguraci kontejneru služby hello, včetně počtu nových agenta hello.</span><span class="sxs-lookup"><span data-stu-id="80368-122">hello Azure CLI 2.0 returns a JSON string representing hello new configuration of hello container service, including hello new agent count.</span></span>

<span data-ttu-id="80368-123">Další možnosti příkazu zobrazíte spuštěním příkazu `az acs scale --help`.</span><span class="sxs-lookup"><span data-stu-id="80368-123">For more command options, run `az acs scale --help`.</span></span>

## <a name="scaling-considerations"></a><span data-ttu-id="80368-124">Důležité informace o škálování</span><span class="sxs-lookup"><span data-stu-id="80368-124">Scaling considerations</span></span>

* <span data-ttu-id="80368-125">Hello počet uzlů agent musí být mezi 1 a 100, včetně.</span><span class="sxs-lookup"><span data-stu-id="80368-125">hello number of agent nodes must be between 1 and 100, inclusive.</span></span> 

* <span data-ttu-id="80368-126">Vaší kvóty jader můžete omezit počet hello agenta uzlů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="80368-126">Your cores quota can limit hello number of agent nodes in a cluster.</span></span>

* <span data-ttu-id="80368-127">Operace škálování uzlu agenta jsou sadě škálování virtuálního počítače Azure použité tooan obsahující fond agenta hello.</span><span class="sxs-lookup"><span data-stu-id="80368-127">Agent node scaling operations are applied tooan Azure virtual machine scale set that contains hello agent pool.</span></span> <span data-ttu-id="80368-128">V clusteru DC/OS jsou pouze agent uzlů ve fondu privátní hello škálovat operacemi hello uvedené v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="80368-128">In a DC/OS cluster, only agent nodes in hello private pool are scaled by hello operations shown in this article.</span></span>

* <span data-ttu-id="80368-129">V závislosti na hello orchestrator, které můžete nasadit v clusteru můžete nezávisle škálovat hello počet instancí kontejneru běžící v clusteru s hello.</span><span class="sxs-lookup"><span data-stu-id="80368-129">Depending on hello orchestrator you deploy in your cluster, you can separately scale hello number of instances of a container running on hello cluster.</span></span> <span data-ttu-id="80368-130">Například v clusteru DC/OS, použijte hello [uživatelského rozhraní Marathon](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md) toochange hello počet instancí kontejneru aplikace.</span><span class="sxs-lookup"><span data-stu-id="80368-130">For example, in a DC/OS cluster, use hello [Marathon UI](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md) toochange hello number of instances of a container application.</span></span>

* <span data-ttu-id="80368-131">Automatické škálování uzlů agentů v clusteru Container Service aktuálně není podporováno.</span><span class="sxs-lookup"><span data-stu-id="80368-131">Currently, autoscaling of agent nodes in a container service cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="80368-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="80368-132">Next steps</span></span>
* <span data-ttu-id="80368-133">Podívejte se na [další příklady](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) použití příkazů Azure CLI 2.0 se službou Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="80368-133">See [more examples](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) of using Azure CLI 2.0 commands with Azure Container Service.</span></span>
* <span data-ttu-id="80368-134">Další informace o [fondech agentů DC/OS](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) ve službě Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="80368-134">Learn more about [DC/OS agent pools](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) in Azure Container Service.</span></span>

