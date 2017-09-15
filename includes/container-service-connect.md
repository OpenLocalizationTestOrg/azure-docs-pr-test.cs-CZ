# <a name="make-a-remote-connection-to-a-kubernetes-dcos-or-docker-swarm-cluster"></a><span data-ttu-id="132cc-101">Vzdálené připojení ke clusteru Kubernetes, DC/OS nebo Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="132cc-101">Make a remote connection to a Kubernetes, DC/OS, or Docker Swarm cluster</span></span>
<span data-ttu-id="132cc-102">Po vytvoření clusteru Azure Container Service je nutné se ke clusteru připojit kvůli nasazení a správě úloh.</span><span class="sxs-lookup"><span data-stu-id="132cc-102">After creating an Azure Container Service cluster, you need to connect to the cluster to deploy and manage workloads.</span></span> <span data-ttu-id="132cc-103">Tento článek popisuje, jak se připojit k hlavnímu virtuálnímu počítači clusteru ze vzdáleného počítače.</span><span class="sxs-lookup"><span data-stu-id="132cc-103">This article describes how to connect to the master VM of the cluster from a remote computer.</span></span> 

<span data-ttu-id="132cc-104">Clustery Kubernetes, DC/OS a Docker Swarm poskytují koncové body HTTP místně.</span><span class="sxs-lookup"><span data-stu-id="132cc-104">The Kubernetes, DC/OS, and Docker Swarm clusters provide HTTP endpoints locally.</span></span> <span data-ttu-id="132cc-105">V případě Kubernetes je tento koncový bod bezpečně zpřístupněn na internetu a můžete se k němu přímo připojit z libovolného počítače připojeného k internetu spuštěním pomocí nástroje `kubectl` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="132cc-105">For Kubernetes, this endpoint is securely exposed on the internet, and you can access it by running the `kubectl` command-line tool from any internet-connected machine.</span></span> 

<span data-ttu-id="132cc-106">V případě DC/OS a Docker Swarm doporučujeme z místního počítače k systému pro správu clusteru vytvořit tunel Secure Shell (SSH).</span><span class="sxs-lookup"><span data-stu-id="132cc-106">For DC/OS and Docker Swarm, we recommend that you create a secure shell (SSH) tunnel from your local computer to the cluster management system.</span></span> <span data-ttu-id="132cc-107">Po vytvoření tunelu můžete spouštět příkazy, které využívají koncové body HTTP, a zobrazovat webové rozhraní orchestrátoru (pokud je k dispozici) z místního systému.</span><span class="sxs-lookup"><span data-stu-id="132cc-107">After the tunnel is established, you can run commands which use the HTTP endpoints and view the orchestrator's web interface (if available) from your local system.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="132cc-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="132cc-108">Prerequisites</span></span>

* <span data-ttu-id="132cc-109">Cluster Kubernetes, DC/OS nebo Docker Swarm [nasazený ve službě Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="132cc-109">A Kubernetes, DC/OS, or Docker Swarm cluster [deployed in Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md).</span></span>
* <span data-ttu-id="132cc-110">Privátní klíč SSH RSA odpovídající veřejnému klíči SSH přidaného do clusteru během nasazení</span><span class="sxs-lookup"><span data-stu-id="132cc-110">SSH RSA private key file, corresponding to the public key added to the cluster during deployment.</span></span> <span data-ttu-id="132cc-111">Tyto příkazy předpokládají, že privátní klíč SSH je ve vašem počítači ve složce `$HOME/.ssh/id_rsa`.</span><span class="sxs-lookup"><span data-stu-id="132cc-111">These commands assume that the private SSH key is in `$HOME/.ssh/id_rsa` on your computer.</span></span> <span data-ttu-id="132cc-112">Tyto pokyny existují také pro [macOS a Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) nebo pro [Windows](../articles/virtual-machines/linux/ssh-from-windows.md).</span><span class="sxs-lookup"><span data-stu-id="132cc-112">See these instructions for [macOS and Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) or [Windows](../articles/virtual-machines/linux/ssh-from-windows.md) for more information.</span></span> <span data-ttu-id="132cc-113">Pokud připojení SSH nefunguje, možná budete muset [resetovat vaše klíče SSH](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).</span><span class="sxs-lookup"><span data-stu-id="132cc-113">If the SSH connection isn't working, you may need to [reset your SSH keys](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).</span></span>

## <a name="connect-to-a-kubernetes-cluster"></a><span data-ttu-id="132cc-114">Připojení ke clusteru Kubernetes</span><span class="sxs-lookup"><span data-stu-id="132cc-114">Connect to a Kubernetes cluster</span></span>

<span data-ttu-id="132cc-115">Podle následujícího postupu nainstalujete a nakonfigurujete `kubectl` ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="132cc-115">Follow these steps to install and configure `kubectl` on your computer.</span></span>

> [!NOTE] 
> <span data-ttu-id="132cc-116">V Linuxu nebo v macOS může být nutné spouštět uvedené příkazy pomocí `sudo`.</span><span class="sxs-lookup"><span data-stu-id="132cc-116">On Linux or macOS, you might need to run the commands in this section using `sudo`.</span></span>
> 

### <a name="install-kubectl"></a><span data-ttu-id="132cc-117">Instalace kubectl</span><span class="sxs-lookup"><span data-stu-id="132cc-117">Install kubectl</span></span>
<span data-ttu-id="132cc-118">Jedním ze způsobů, jak tento nástroj nainstalovat, je použít nástroj příkazového řádku Azure 2.0 `az acs kubernetes install-cli`.</span><span class="sxs-lookup"><span data-stu-id="132cc-118">One way to install this tool is to use the `az acs kubernetes install-cli` Azure CLI 2.0 command.</span></span> <span data-ttu-id="132cc-119">Pokud chcete spustit tento příkaz, ujistěte se, že jste [nainstalovali](/cli/azure/install-az-cli2) nejnovější příkazový řádek Azure CLI 2.0 a jste přihlášení k účtu Azure (`az login`).</span><span class="sxs-lookup"><span data-stu-id="132cc-119">To run this command, make sure that you [installed](/cli/azure/install-az-cli2) the latest Azure CLI 2.0 and logged in to an Azure account (`az login`).</span></span>

```azurecli
# Linux or macOS
az acs kubernetes install-cli [--install-location=/some/directory/kubectl]

# Windows
az acs kubernetes install-cli [--install-location=C:\some\directory\kubectl.exe]
```

<span data-ttu-id="132cc-120">Případně si můžete nejnovějšího klienta `kubectl` stáhnout přímo ze [stránky vydaných verzí Kubernetes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).</span><span class="sxs-lookup"><span data-stu-id="132cc-120">Alternatively, you can download the latest `kubectl` client directly from the [Kubernetes releases page](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).</span></span> <span data-ttu-id="132cc-121">Další informace najdete v tématu [Instalace a nastavení kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span><span class="sxs-lookup"><span data-stu-id="132cc-121">For more information, see [Installing and Setting up kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span></span>

### <a name="download-cluster-credentials"></a><span data-ttu-id="132cc-122">Stažení přihlašovacích údajů clusteru</span><span class="sxs-lookup"><span data-stu-id="132cc-122">Download cluster credentials</span></span>
<span data-ttu-id="132cc-123">Jakmile budete mít `kubectl` nainstalovaný, je třeba, abyste na svůj počítač zkopírovali přihlašovací údaje clusteru.</span><span class="sxs-lookup"><span data-stu-id="132cc-123">Once you have `kubectl` installed, you need to copy the cluster credentials to your machine.</span></span> <span data-ttu-id="132cc-124">Přihlašovací údaje můžete získat například příkazem `az acs kubernetes get-credentials`.</span><span class="sxs-lookup"><span data-stu-id="132cc-124">One way to do get the credentials is with the `az acs kubernetes get-credentials` command.</span></span> <span data-ttu-id="132cc-125">Předejte název skupiny prostředků a název prostředku kontejnerové služby:</span><span class="sxs-lookup"><span data-stu-id="132cc-125">Pass the name of the resource group and the name of the container service resource:</span></span>

```azurecli
az acs kubernetes get-credentials --resource-group=<cluster-resource-group> --name=<cluster-name>
```

<span data-ttu-id="132cc-126">Tento příkaz stáhne přihlašovací údaje clusteru do složky `$HOME/.kube/config`, kde `kubectl` očekává, že je najde.</span><span class="sxs-lookup"><span data-stu-id="132cc-126">This command downloads the cluster credentials to `$HOME/.kube/config`, where `kubectl` expects it to be located.</span></span>

<span data-ttu-id="132cc-127">Případně můžete použít `scp` a bezpečně zkopírovat soubor ze složky `$HOME/.kube/config` na hlavním virtuálním počítači do svého místního počítače.</span><span class="sxs-lookup"><span data-stu-id="132cc-127">Alternatively, you can use `scp` to securely copy the file from `$HOME/.kube/config` on the master VM to your local machine.</span></span> <span data-ttu-id="132cc-128">Například:</span><span class="sxs-lookup"><span data-stu-id="132cc-128">For example:</span></span>

```bash
mkdir $HOME/.kube
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

<span data-ttu-id="132cc-129">V systému Windows můžete použít Bash on Ubuntu on Windows, klienta pro bezpečné kopírování souborů PuTTY nebo podobný nástroj.</span><span class="sxs-lookup"><span data-stu-id="132cc-129">If you are on Windows, you can use Bash on Ubuntu on Windows, the PuTTy secure file copy client, or a similar tool.</span></span>

### <a name="use-kubectl"></a><span data-ttu-id="132cc-130">Použití kubectl</span><span class="sxs-lookup"><span data-stu-id="132cc-130">Use kubectl</span></span>

<span data-ttu-id="132cc-131">Jakmile bude `kubectl` nakonfigurovaný, otestujte připojení výpisem uzlů ve vašem clusteru:</span><span class="sxs-lookup"><span data-stu-id="132cc-131">Once you have `kubectl` configured, test the connection by listing the nodes in your cluster:</span></span>

```bash
kubectl get nodes
```

<span data-ttu-id="132cc-132">Můžete vyzkoušet i další příkazy nástroje `kubectl`.</span><span class="sxs-lookup"><span data-stu-id="132cc-132">You can try other `kubectl` commands.</span></span> <span data-ttu-id="132cc-133">Můžete například zobrazit řídicí panel Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="132cc-133">For example, you can view the Kubernetes Dashboard.</span></span> <span data-ttu-id="132cc-134">Nejprve spusťte proxy na serveru Kubernetes API:</span><span class="sxs-lookup"><span data-stu-id="132cc-134">First, run a proxy to the Kubernetes API server:</span></span>

```bash
kubectl proxy
```

<span data-ttu-id="132cc-135">Uživatelské rozhraní Kubernetes je nyní k dispozici na adrese: `http://localhost:8001/ui`.</span><span class="sxs-lookup"><span data-stu-id="132cc-135">The Kubernetes UI is now available at: `http://localhost:8001/ui`.</span></span>

<span data-ttu-id="132cc-136">Další informace najdete v tématu [Rychlé představení Kubernetes](http://kubernetes.io/docs/user-guide/quick-start/).</span><span class="sxs-lookup"><span data-stu-id="132cc-136">For more information, see the [Kubernetes quick start](http://kubernetes.io/docs/user-guide/quick-start/).</span></span>

## <a name="connect-to-a-dcos-or-swarm-cluster"></a><span data-ttu-id="132cc-137">Připojení ke clusteru DC/OS nebo Swarm</span><span class="sxs-lookup"><span data-stu-id="132cc-137">Connect to a DC/OS or Swarm cluster</span></span>

<span data-ttu-id="132cc-138">Pokud chcete používat clustery DC/OS a Docker Swarm nasazené ve službě Azure Container Service, postupujte podle těchto pokynů a vytvořte tunel SSH z místního systému Linux, macOS nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="132cc-138">To use the DC/OS and Docker Swarm clusters deployed by Azure Container Service, follow these instructions to create a SSH tunnel from your local Linux, macOS, or Windows system.</span></span> 

> [!NOTE]
> <span data-ttu-id="132cc-139">Tyto pokyny se zaměřují na tunelování přenosů TCP přes protokol SSH.</span><span class="sxs-lookup"><span data-stu-id="132cc-139">These instructions focus on tunneling TCP traffic over SSH.</span></span> <span data-ttu-id="132cc-140">Můžete také spustit interaktivní relaci SSH s jedním z interních systémů pro správu clusteru, ale nedoporučuje se to.</span><span class="sxs-lookup"><span data-stu-id="132cc-140">You can also start an interactive SSH session with one of the internal cluster management systems, but we don't recommend this.</span></span> <span data-ttu-id="132cc-141">Práce přímo v interním systému s sebou nese riziko neúmyslných změn konfigurace.</span><span class="sxs-lookup"><span data-stu-id="132cc-141">Working directly on an internal system risks inadvertent configuration changes.</span></span>  
> 

### <a name="create-an-ssh-tunnel-on-linux-or-macos"></a><span data-ttu-id="132cc-142">Vytvoření tunelu SSH v Linuxu a macOS</span><span class="sxs-lookup"><span data-stu-id="132cc-142">Create an SSH tunnel on Linux or macOS</span></span>
<span data-ttu-id="132cc-143">První věc, kterou je nutné udělat, když vytváříte tunel SSH v Linuxu nebo macOS, je nalezení veřejného názvu DNS hlavních serverů s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="132cc-143">The first thing that you do when you create an SSH tunnel on Linux or macOS is to locate the public DNS name of the load-balanced masters.</span></span> <span data-ttu-id="132cc-144">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="132cc-144">Follow these steps:</span></span>


1. <span data-ttu-id="132cc-145">Na webu [Azure Portal](https://portal.azure.com) přejděte do skupiny prostředků obsahující váš cluster kontejnerové služby.</span><span class="sxs-lookup"><span data-stu-id="132cc-145">In the [Azure portal](https://portal.azure.com), browse to the resource group containing your container service cluster.</span></span> <span data-ttu-id="132cc-146">Rozbalte skupinu prostředků, aby se zobrazily všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="132cc-146">Expand the resource group so that each resource is displayed.</span></span> 

2. <span data-ttu-id="132cc-147">Klikněte na prostředek **Služba kontejneru** a potom klikněte na **Přehled**.</span><span class="sxs-lookup"><span data-stu-id="132cc-147">Click the **Container service** resource, and click **Overview**.</span></span> <span data-ttu-id="132cc-148">V části **Základy** se zobrazí **Hlavní plně kvalifikovaný název domény** clusteru.</span><span class="sxs-lookup"><span data-stu-id="132cc-148">The **Master FQDN** of the cluster appears under **Essentials**.</span></span> <span data-ttu-id="132cc-149">Uložte si tento název pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="132cc-149">Save this name for later use.</span></span> 

    ![Veřejný název DNS](./media/container-service-connect/pubdns.png)

    <span data-ttu-id="132cc-151">Další možností je spustit příkaz `az acs show` pro kontejnerovou službu.</span><span class="sxs-lookup"><span data-stu-id="132cc-151">Alternatively, run the `az acs show` command on your container service.</span></span> <span data-ttu-id="132cc-152">Ve výstupu tohoto příkazu vyhledejte vlastnost **Master Profile:fqdn**.</span><span class="sxs-lookup"><span data-stu-id="132cc-152">Look for the **Master Profile:fqdn** property in the command output.</span></span>

3. <span data-ttu-id="132cc-153">Nyní otevřete prostředí shell a spusťte příkaz `ssh` s následujícími parametry:</span><span class="sxs-lookup"><span data-stu-id="132cc-153">Now open a shell and run the `ssh` command by specifying the following values:</span></span> 

    <span data-ttu-id="132cc-154">**LOCAL_PORT** je port TCP na straně služby tunelu, ke kterému se chcete připojit.</span><span class="sxs-lookup"><span data-stu-id="132cc-154">**LOCAL_PORT** is the TCP port on the service side of the tunnel to connect to.</span></span> <span data-ttu-id="132cc-155">Pro Swarm nastavte tuto hodnotu na 2375.</span><span class="sxs-lookup"><span data-stu-id="132cc-155">For Swarm, set this to 2375.</span></span> <span data-ttu-id="132cc-156">Pro DC/OS nastavte tuto hodnotu na 80.</span><span class="sxs-lookup"><span data-stu-id="132cc-156">For DC/OS, set this to 80.</span></span> 
    <span data-ttu-id="132cc-157">**REMOTE_PORT** je port koncového bodu, který chcete zveřejnit.</span><span class="sxs-lookup"><span data-stu-id="132cc-157">**REMOTE_PORT** is the port of the endpoint that you want to expose.</span></span> <span data-ttu-id="132cc-158">Pro Swarm použijte 2375.</span><span class="sxs-lookup"><span data-stu-id="132cc-158">For Swarm, use port 2375.</span></span> <span data-ttu-id="132cc-159">Pro DC/OS použijte port 80.</span><span class="sxs-lookup"><span data-stu-id="132cc-159">For DC/OS, use port 80.</span></span>  
    <span data-ttu-id="132cc-160">**USERNAME** je uživatelské jméno zadané ve chvíli, kdy jste nasadili cluster.</span><span class="sxs-lookup"><span data-stu-id="132cc-160">**USERNAME** is the user name that was provided when you deployed the cluster.</span></span>  
    <span data-ttu-id="132cc-161">**DNSPREFIX** je předpona DNS zadaná ve chvíli, kdy jste nasadili cluster.</span><span class="sxs-lookup"><span data-stu-id="132cc-161">**DNSPREFIX** is the DNS prefix that you provided when you deployed the cluster.</span></span>  
    <span data-ttu-id="132cc-162">**REGION** je oblast, ve které je umístěna skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="132cc-162">**REGION** is the region in which your resource group is located.</span></span>  
    <span data-ttu-id="132cc-163">**PATH_TO_PRIVATE_KEY** [NEPOVINNÉ] je cesta k privátnímu klíči, který odpovídá veřejnému klíči zadanému při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="132cc-163">**PATH_TO_PRIVATE_KEY** [OPTIONAL] is the path to the private key that corresponds to the public key you provided when you created the cluster.</span></span> <span data-ttu-id="132cc-164">Tuto možnost použijte spolu s příznakem `-i`.</span><span class="sxs-lookup"><span data-stu-id="132cc-164">Use this option with the `-i` flag.</span></span>

    ```bash
    ssh -fNL LOCAL_PORT:localhost:REMOTE_PORT -p 2200 [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com
    ```
  
  > [!NOTE]
  > <span data-ttu-id="132cc-165">Port pro připojení SSH je 2200, nikoli standardní port 22.</span><span class="sxs-lookup"><span data-stu-id="132cc-165">The SSH connection port is 2200 and not the standard port 22.</span></span> <span data-ttu-id="132cc-166">V clusteru s více hlavními virtuálními počítači je to port pro připojení k prvnímu hlavnímu virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="132cc-166">In a cluster with more than one master VM, this is the connection port to the first master VM.</span></span>
  > 

  <span data-ttu-id="132cc-167">Příkaz nevrací žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="132cc-167">The command returns without output.</span></span>

<span data-ttu-id="132cc-168">Podívejte se na příklady pro DC/OS a Swarm v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="132cc-168">See the examples for DC/OS and Swarm in the following sections.</span></span>    

### <a name="dcos-tunnel"></a><span data-ttu-id="132cc-169">Tunel DC/OS</span><span class="sxs-lookup"><span data-stu-id="132cc-169">DC/OS tunnel</span></span>
<span data-ttu-id="132cc-170">Pokud chcete otevřít tunel pro koncové body DC/OS, spusťte příkaz podobný následujícímu příkazu:</span><span class="sxs-lookup"><span data-stu-id="132cc-170">To open a tunnel for DC/OS endpoints, run a command like the following:</span></span>

```bash
sudo ssh -fNL 80:localhost:80 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com 
```

> [!NOTE]
> <span data-ttu-id="132cc-171">Ujistěte se, že se na port 80 neváže žádný jiný místní proces.</span><span class="sxs-lookup"><span data-stu-id="132cc-171">Ensure that you do not have another local process that binds port 80.</span></span> <span data-ttu-id="132cc-172">V případě potřeby můžete zadat jiný místní port než 80, třeba port 8080.</span><span class="sxs-lookup"><span data-stu-id="132cc-172">If necessary, you can specify a local port other than port 80, such as port 8080.</span></span> <span data-ttu-id="132cc-173">Při použití tohoto portu ale některé odkazy webového uživatelského rozhraní nemusí fungovat.</span><span class="sxs-lookup"><span data-stu-id="132cc-173">However, some web UI links might not work when you use this port.</span></span>
>

<span data-ttu-id="132cc-174">Nyní máte ke koncovým bodům DC/OS přístup z místního systému prostřednictvím následujících adres URL (předpokládá se místní port 80):</span><span class="sxs-lookup"><span data-stu-id="132cc-174">You can now access the DC/OS endpoints from your local system through the following URLs (assuming local port 80):</span></span>

* <span data-ttu-id="132cc-175">DC/OS: `http://localhost:80/`</span><span class="sxs-lookup"><span data-stu-id="132cc-175">DC/OS: `http://localhost:80/`</span></span>
* <span data-ttu-id="132cc-176">Marathon: `http://localhost:80/marathon`</span><span class="sxs-lookup"><span data-stu-id="132cc-176">Marathon: `http://localhost:80/marathon`</span></span>
* <span data-ttu-id="132cc-177">Mesos: `http://localhost:80/mesos`</span><span class="sxs-lookup"><span data-stu-id="132cc-177">Mesos: `http://localhost:80/mesos`</span></span>

<span data-ttu-id="132cc-178">Obdobně můžete přes tento tunel kontaktovat rozhraní REST API pro každou z aplikací.</span><span class="sxs-lookup"><span data-stu-id="132cc-178">Similarly, you can reach the rest APIs for each application through this tunnel.</span></span>

### <a name="swarm-tunnel"></a><span data-ttu-id="132cc-179">Tunel Swarm</span><span class="sxs-lookup"><span data-stu-id="132cc-179">Swarm tunnel</span></span>
<span data-ttu-id="132cc-180">Pokud chcete otevřít tunel ke koncovým bodům Swarmu, spusťte příkaz podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="132cc-180">To open a tunnel to the Swarm endpoint, run a command like the following:</span></span>

```bash
ssh -fNL 2375:localhost:2375 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com
```
> [!NOTE]
> <span data-ttu-id="132cc-181">Ujistěte se, že se na port 2375 neváže žádný jiný místní proces.</span><span class="sxs-lookup"><span data-stu-id="132cc-181">Ensure that you do not have another local process that binds port 2375.</span></span> <span data-ttu-id="132cc-182">Pokud například místně spouštíte démona Dockeru, ve výchozím nastavení používá port 2375.</span><span class="sxs-lookup"><span data-stu-id="132cc-182">For example, if you are running the Docker daemon locally, it's set by default to use port 2375.</span></span> <span data-ttu-id="132cc-183">V případě potřeby můžete zadat jiný místní port než 2375.</span><span class="sxs-lookup"><span data-stu-id="132cc-183">If necessary, you can specify a local port other than port 2375.</span></span>
>

<span data-ttu-id="132cc-184">Nyní můžete přistupovat ke clusteru Docker Swarm pomocí rozhraní příkazového řádku Dockeru (Docker CLI) v místním systému.</span><span class="sxs-lookup"><span data-stu-id="132cc-184">Now you can access the Docker Swarm cluster using the Docker command-line interface (Docker CLI) on your local system.</span></span> <span data-ttu-id="132cc-185">Pokyny k instalaci najdete v tématu [Instalace Dockeru](https://docs.docker.com/engine/installation/).</span><span class="sxs-lookup"><span data-stu-id="132cc-185">For installation instructions, see [Install Docker](https://docs.docker.com/engine/installation/).</span></span>

<span data-ttu-id="132cc-186">Nastavte proměnnou prostředí DOCKER_HOST na místní port, který jste nakonfigurovali pro tunel.</span><span class="sxs-lookup"><span data-stu-id="132cc-186">Set your DOCKER_HOST environment variable to the local port you configured for the tunnel.</span></span> 

```bash
export DOCKER_HOST=:2375
```

<span data-ttu-id="132cc-187">Spusťte příkazy Dockeru, které vytvoří tunel ke clusteru Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="132cc-187">Run Docker commands that tunnel to the Docker Swarm cluster.</span></span> <span data-ttu-id="132cc-188">Například:</span><span class="sxs-lookup"><span data-stu-id="132cc-188">For example:</span></span>

```bash
docker info
```

### <a name="create-an-ssh-tunnel-on-windows"></a><span data-ttu-id="132cc-189">Vytvoření tunelu SSH ve Windows</span><span class="sxs-lookup"><span data-stu-id="132cc-189">Create an SSH tunnel on Windows</span></span>
<span data-ttu-id="132cc-190">Tunely SSH je ve Windows možné vytvořit několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="132cc-190">There are multiple options for creating SSH tunnels on Windows.</span></span> <span data-ttu-id="132cc-191">Pokud používáte Bash on Ubuntu on Windows nebo podobný nástroj, můžete postupovat podle pokynů k tunelování SSH popsaných výše v tomto článku pro macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="132cc-191">If you are running Bash on Ubuntu on Windows or a similar tool, you can follow the SSH tunneling instructions shown earlier in this article for macOS and Linux.</span></span> <span data-ttu-id="132cc-192">Tato část popisuje, jak ve Windows alternativně vytvořit tunel pomocí PuTTY.</span><span class="sxs-lookup"><span data-stu-id="132cc-192">As an alternative on Windows, this section describes how to use PuTTY to create the tunnel.</span></span>

1. <span data-ttu-id="132cc-193">[Stáhněte si PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) do svého počítače s Windows.</span><span class="sxs-lookup"><span data-stu-id="132cc-193">[Download PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) to your Windows system.</span></span>

2. <span data-ttu-id="132cc-194">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="132cc-194">Run the application.</span></span>

3. <span data-ttu-id="132cc-195">Zadejte název hostitele, který se skládá z uživatelského jména správce clusteru a veřejného názvu DNS prvního hlavního serveru v clusteru.</span><span class="sxs-lookup"><span data-stu-id="132cc-195">Enter a host name that is comprised of the cluster admin user name and the public DNS name of the first master in the cluster.</span></span> <span data-ttu-id="132cc-196">**Název hostitele** vypadá podobně jako `azureuser@PublicDNSName`.</span><span class="sxs-lookup"><span data-stu-id="132cc-196">The **Host Name** looks similar to `azureuser@PublicDNSName`.</span></span> <span data-ttu-id="132cc-197">Jako **port** zadejte 2200.</span><span class="sxs-lookup"><span data-stu-id="132cc-197">Enter 2200 for the **Port**.</span></span>

    ![Konfigurace PuTTY 1](./media/container-service-connect/putty1.png)

4. <span data-ttu-id="132cc-199">Vyberte **SSH > Ověřování**.</span><span class="sxs-lookup"><span data-stu-id="132cc-199">Select **SSH > Auth**.</span></span> <span data-ttu-id="132cc-200">Pro ověření přidejte cestu ke svému souboru privátního klíče (formát .ppk).</span><span class="sxs-lookup"><span data-stu-id="132cc-200">Add a path to your private key file (.ppk format) for authentication.</span></span> <span data-ttu-id="132cc-201">Můžete použít nástroj typu [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) k vygenerování tohoto souboru z klíč SSH použitého při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="132cc-201">You can use a tool such as [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) to generate this file from the SSH key used to create the cluster.</span></span>

    ![Konfigurace PuTTY 2](./media/container-service-connect/putty2.png)

5. <span data-ttu-id="132cc-203">Vyberte **SSH > Tunely** a nakonfigurujte následující přesměrované porty:</span><span class="sxs-lookup"><span data-stu-id="132cc-203">Select **SSH > Tunnels** and configure the following forwarded ports:</span></span>

    * <span data-ttu-id="132cc-204">**Zdrojový port:** Použijte 80 pro DC/OS nebo 2375 pro Swarm.</span><span class="sxs-lookup"><span data-stu-id="132cc-204">**Source Port:** Use 80 for DC/OS or 2375 for Swarm.</span></span>
    * <span data-ttu-id="132cc-205">**Cíl:** Použijte localhost:80 pro DC/OS nebo localhost:2375 pro Swarm.</span><span class="sxs-lookup"><span data-stu-id="132cc-205">**Destination:** Use localhost:80 for DC/OS or localhost:2375 for Swarm.</span></span>

    <span data-ttu-id="132cc-206">Následující příklad je nakonfigurován pro DC/OS, ale pro Docker Swarm bude vypadat obdobně.</span><span class="sxs-lookup"><span data-stu-id="132cc-206">The following example is configured for DC/OS, but will look similar for Docker Swarm.</span></span>

    > [!NOTE]
    > <span data-ttu-id="132cc-207">Při vytváření tohoto tunelu se port 80 nesmí používat.</span><span class="sxs-lookup"><span data-stu-id="132cc-207">Port 80 must not be in use when you create this tunnel.</span></span>
    > 

    ![Konfigurace PuTTY 3](./media/container-service-connect/putty3.png)

6. <span data-ttu-id="132cc-209">Po dokončení uložte konfiguraci připojení kliknutím na **Relace > Uložit**.</span><span class="sxs-lookup"><span data-stu-id="132cc-209">When you're finished, click **Session > Save** to save the connection configuration.</span></span>

7. <span data-ttu-id="132cc-210">K relaci PuTTY se připojíte kliknutím na **Otevřít**.</span><span class="sxs-lookup"><span data-stu-id="132cc-210">To connect to the PuTTY session, click **Open**.</span></span> <span data-ttu-id="132cc-211">Po připojení je možné v protokolu událostí PuTTY vidět konfiguraci portů.</span><span class="sxs-lookup"><span data-stu-id="132cc-211">When you connect, you can see the port configuration in the PuTTY event log.</span></span>

    ![Protokol událostí PuTTY](./media/container-service-connect/putty4.png)

<span data-ttu-id="132cc-213">Až bude tunel pro DC/OS nakonfigurovaný, budete mít k příslušným koncovým bodům přístup přes tyto adresy:</span><span class="sxs-lookup"><span data-stu-id="132cc-213">After you've configured the tunnel for DC/OS, you can access the related endpoints at:</span></span>

* <span data-ttu-id="132cc-214">DC/OS: `http://localhost/`</span><span class="sxs-lookup"><span data-stu-id="132cc-214">DC/OS: `http://localhost/`</span></span>
* <span data-ttu-id="132cc-215">Marathon: `http://localhost/marathon`</span><span class="sxs-lookup"><span data-stu-id="132cc-215">Marathon: `http://localhost/marathon`</span></span>
* <span data-ttu-id="132cc-216">Mesos: `http://localhost/mesos`</span><span class="sxs-lookup"><span data-stu-id="132cc-216">Mesos: `http://localhost/mesos`</span></span>

<span data-ttu-id="132cc-217">Po dokončení konfigurace tunelu pro Docker Swarm otevřete nastavení systému Windows a nastavte proměnnou prostředí `DOCKER_HOST` na hodnotu `:2375`.</span><span class="sxs-lookup"><span data-stu-id="132cc-217">After you've configured the tunnel for Docker Swarm, open your Windows settings to configure a system environment variable named `DOCKER_HOST` with a value of `:2375`.</span></span> <span data-ttu-id="132cc-218">Pak budete mít přístup ke clusteru Swarm přes příkazový řádek Dockeru.</span><span class="sxs-lookup"><span data-stu-id="132cc-218">Then, you can access the Swarm cluster through the Docker CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="132cc-219">Další kroky</span><span class="sxs-lookup"><span data-stu-id="132cc-219">Next steps</span></span>
<span data-ttu-id="132cc-220">Nasazení a správa kontejnerů ve vašem clusteru:</span><span class="sxs-lookup"><span data-stu-id="132cc-220">Deploy and manage containers in your cluster:</span></span>

* [<span data-ttu-id="132cc-221">Práce s Azure Container Service a Kubernetes</span><span class="sxs-lookup"><span data-stu-id="132cc-221">Work with Azure Container Service and Kubernetes</span></span>](../articles/container-service/kubernetes/container-service-kubernetes-ui.md)
* [<span data-ttu-id="132cc-222">Práce se službou Azure Container Service a DC/OS</span><span class="sxs-lookup"><span data-stu-id="132cc-222">Work with Azure Container Service and DC/OS</span></span>](../articles/container-service//dcos-swarm/container-service-mesos-marathon-rest.md)
* [<span data-ttu-id="132cc-223">Práce s Azure Container Service a Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="132cc-223">Work with the Azure Container Service and Docker Swarm</span></span>](../articles//container-service/dcos-swarm/container-service-docker-swarm.md)

