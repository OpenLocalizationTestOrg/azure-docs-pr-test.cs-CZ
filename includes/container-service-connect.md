# <a name="make-a-remote-connection-tooa-kubernetes-dcos-or-docker-swarm-cluster"></a><span data-ttu-id="dcb06-101">Ujistěte se, tooa Kubernetes, DC/OS nebo Docker Swarm clusteru vzdáleného připojení</span><span class="sxs-lookup"><span data-stu-id="dcb06-101">Make a remote connection tooa Kubernetes, DC/OS, or Docker Swarm cluster</span></span>
<span data-ttu-id="dcb06-102">Po vytvoření clusteru Azure Container Service, třeba tooconnect toohello clusteru toodeploy a spravovat úlohy.</span><span class="sxs-lookup"><span data-stu-id="dcb06-102">After creating an Azure Container Service cluster, you need tooconnect toohello cluster toodeploy and manage workloads.</span></span> <span data-ttu-id="dcb06-103">Tento článek popisuje, jak tooconnect toohello hlavní počítač hello clusteru ze vzdáleného počítače.</span><span class="sxs-lookup"><span data-stu-id="dcb06-103">This article describes how tooconnect toohello master VM of hello cluster from a remote computer.</span></span> 

<span data-ttu-id="dcb06-104">Hello Kubernetes, DC/OS a Docker Swarm clustery poskytují koncové body HTTP místně.</span><span class="sxs-lookup"><span data-stu-id="dcb06-104">hello Kubernetes, DC/OS, and Docker Swarm clusters provide HTTP endpoints locally.</span></span> <span data-ttu-id="dcb06-105">Pro Kubernetes, je tento koncový bod bezpečně zveřejněné na hello internet a můžete k němu přístup spuštěním hello `kubectl` nástroj pro příkazový řádek z libovolného počítače připojeného k Internetu.</span><span class="sxs-lookup"><span data-stu-id="dcb06-105">For Kubernetes, this endpoint is securely exposed on hello internet, and you can access it by running hello `kubectl` command-line tool from any internet-connected machine.</span></span> 

<span data-ttu-id="dcb06-106">Pro DC/OS a Docker Swarm doporučujeme vytvořit tunel zabezpečené shell (SSH) z váš systém správy clusteru toohello místního počítače.</span><span class="sxs-lookup"><span data-stu-id="dcb06-106">For DC/OS and Docker Swarm, we recommend that you create a secure shell (SSH) tunnel from your local computer toohello cluster management system.</span></span> <span data-ttu-id="dcb06-107">Po hello tunelového propojení, můžete spouštět příkazy, které používají koncové body HTTP hello a zobrazení hello orchestrator webové rozhraní (Pokud je k dispozici) z místního systému.</span><span class="sxs-lookup"><span data-stu-id="dcb06-107">After hello tunnel is established, you can run commands which use hello HTTP endpoints and view hello orchestrator's web interface (if available) from your local system.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="dcb06-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dcb06-108">Prerequisites</span></span>

* <span data-ttu-id="dcb06-109">Cluster Kubernetes, DC/OS nebo Docker Swarm [nasazený ve službě Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="dcb06-109">A Kubernetes, DC/OS, or Docker Swarm cluster [deployed in Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md).</span></span>
* <span data-ttu-id="dcb06-110">SSH RSA soubor privátního klíče, odpovídající toohello veřejného klíče přidané toohello clusteru během nasazení.</span><span class="sxs-lookup"><span data-stu-id="dcb06-110">SSH RSA private key file, corresponding toohello public key added toohello cluster during deployment.</span></span> <span data-ttu-id="dcb06-111">Těchto příkazů se předpokládá hello privátní klíč SSH představuje v `$HOME/.ssh/id_rsa` ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="dcb06-111">These commands assume that hello private SSH key is in `$HOME/.ssh/id_rsa` on your computer.</span></span> <span data-ttu-id="dcb06-112">Tyto pokyny existují také pro [macOS a Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) nebo pro [Windows](../articles/virtual-machines/linux/ssh-from-windows.md).</span><span class="sxs-lookup"><span data-stu-id="dcb06-112">See these instructions for [macOS and Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) or [Windows](../articles/virtual-machines/linux/ssh-from-windows.md) for more information.</span></span> <span data-ttu-id="dcb06-113">Pokud hello připojení SSH není funkční, bude pravděpodobně nutné příliš [resetovat vaše klíče SSH](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).</span><span class="sxs-lookup"><span data-stu-id="dcb06-113">If hello SSH connection isn't working, you may need too [reset your SSH keys](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).</span></span>

## <a name="connect-tooa-kubernetes-cluster"></a><span data-ttu-id="dcb06-114">Připojte tooa Kubernetes cluster</span><span class="sxs-lookup"><span data-stu-id="dcb06-114">Connect tooa Kubernetes cluster</span></span>

<span data-ttu-id="dcb06-115">Postupujte podle těchto kroků tooinstall a nakonfigurovat `kubectl` ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="dcb06-115">Follow these steps tooinstall and configure `kubectl` on your computer.</span></span>

> [!NOTE] 
> <span data-ttu-id="dcb06-116">V systému macOS nebo Linux, může být nutné toorun hello příkazy v této části pomocí `sudo`.</span><span class="sxs-lookup"><span data-stu-id="dcb06-116">On Linux or macOS, you might need toorun hello commands in this section using `sudo`.</span></span>
> 

### <a name="install-kubectl"></a><span data-ttu-id="dcb06-117">Instalace kubectl</span><span class="sxs-lookup"><span data-stu-id="dcb06-117">Install kubectl</span></span>
<span data-ttu-id="dcb06-118">Jedním ze způsobů tooinstall tento nástroj je toouse hello `az acs kubernetes install-cli` příkaz 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="dcb06-118">One way tooinstall this tool is toouse hello `az acs kubernetes install-cli` Azure CLI 2.0 command.</span></span> <span data-ttu-id="dcb06-119">toorun tento příkaz, ujistěte se, že jste [nainstalován](/cli/azure/install-az-cli2) hello nejnovější Azure CLI 2.0 a přihlášení tooan účet Azure (`az login`).</span><span class="sxs-lookup"><span data-stu-id="dcb06-119">toorun this command, make sure that you [installed](/cli/azure/install-az-cli2) hello latest Azure CLI 2.0 and logged in tooan Azure account (`az login`).</span></span>

```azurecli
# Linux or macOS
az acs kubernetes install-cli [--install-location=/some/directory/kubectl]

# Windows
az acs kubernetes install-cli [--install-location=C:\some\directory\kubectl.exe]
```

<span data-ttu-id="dcb06-120">Alternativně můžete stáhnout hello nejnovější `kubectl` klienta přímo z hello [Kubernetes uvolní stránky](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).</span><span class="sxs-lookup"><span data-stu-id="dcb06-120">Alternatively, you can download hello latest `kubectl` client directly from hello [Kubernetes releases page](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).</span></span> <span data-ttu-id="dcb06-121">Další informace najdete v tématu [Instalace a nastavení kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span><span class="sxs-lookup"><span data-stu-id="dcb06-121">For more information, see [Installing and Setting up kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span></span>

### <a name="download-cluster-credentials"></a><span data-ttu-id="dcb06-122">Stažení přihlašovacích údajů clusteru</span><span class="sxs-lookup"><span data-stu-id="dcb06-122">Download cluster credentials</span></span>
<span data-ttu-id="dcb06-123">Jakmile máte `kubectl` nainstalovali, musíte toocopy hello clusteru pověření tooyour počítače.</span><span class="sxs-lookup"><span data-stu-id="dcb06-123">Once you have `kubectl` installed, you need toocopy hello cluster credentials tooyour machine.</span></span> <span data-ttu-id="dcb06-124">Jedním ze způsobů toodo get hello přihlašovací údaje jsou s hello `az acs kubernetes get-credentials` příkaz.</span><span class="sxs-lookup"><span data-stu-id="dcb06-124">One way toodo get hello credentials is with hello `az acs kubernetes get-credentials` command.</span></span> <span data-ttu-id="dcb06-125">Předejte hello název skupiny prostředků hello a hello název prostředku služby kontejneru hello:</span><span class="sxs-lookup"><span data-stu-id="dcb06-125">Pass hello name of hello resource group and hello name of hello container service resource:</span></span>

```azurecli
az acs kubernetes get-credentials --resource-group=<cluster-resource-group> --name=<cluster-name>
```

<span data-ttu-id="dcb06-126">Tento příkaz stáhne hello clusteru pověření příliš`$HOME/.kube/config`, kde `kubectl` očekává, že toobe nachází.</span><span class="sxs-lookup"><span data-stu-id="dcb06-126">This command downloads hello cluster credentials too`$HOME/.kube/config`, where `kubectl` expects it toobe located.</span></span>

<span data-ttu-id="dcb06-127">Alternativně můžete použít `scp` toosecurely kopírovat hello soubor z `$HOME/.kube/config` na hello hlavní počítač tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="dcb06-127">Alternatively, you can use `scp` toosecurely copy hello file from `$HOME/.kube/config` on hello master VM tooyour local machine.</span></span> <span data-ttu-id="dcb06-128">Například:</span><span class="sxs-lookup"><span data-stu-id="dcb06-128">For example:</span></span>

```bash
mkdir $HOME/.kube
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

<span data-ttu-id="dcb06-129">Pokud jste v systému Windows, můžete na Ubuntu na Windows, klient kopie PuTTy zabezpečeného souboru hello nebo podobné nástroj Bash.</span><span class="sxs-lookup"><span data-stu-id="dcb06-129">If you are on Windows, you can use Bash on Ubuntu on Windows, hello PuTTy secure file copy client, or a similar tool.</span></span>

### <a name="use-kubectl"></a><span data-ttu-id="dcb06-130">Použití kubectl</span><span class="sxs-lookup"><span data-stu-id="dcb06-130">Use kubectl</span></span>

<span data-ttu-id="dcb06-131">Jakmile máte `kubectl` nakonfigurovaná, testovací hello připojení tak, že uvedete hello uzlů v clusteru:</span><span class="sxs-lookup"><span data-stu-id="dcb06-131">Once you have `kubectl` configured, test hello connection by listing hello nodes in your cluster:</span></span>

```bash
kubectl get nodes
```

<span data-ttu-id="dcb06-132">Můžete vyzkoušet i další příkazy nástroje `kubectl`.</span><span class="sxs-lookup"><span data-stu-id="dcb06-132">You can try other `kubectl` commands.</span></span> <span data-ttu-id="dcb06-133">Můžete například zobrazit hello Kubernetes řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="dcb06-133">For example, you can view hello Kubernetes Dashboard.</span></span> <span data-ttu-id="dcb06-134">Nejprve spusťte proxy server toohello Kubernetes API:</span><span class="sxs-lookup"><span data-stu-id="dcb06-134">First, run a proxy toohello Kubernetes API server:</span></span>

```bash
kubectl proxy
```

<span data-ttu-id="dcb06-135">Hello Kubernetes uživatelského rozhraní je nyní k dispozici na: `http://localhost:8001/ui`.</span><span class="sxs-lookup"><span data-stu-id="dcb06-135">hello Kubernetes UI is now available at: `http://localhost:8001/ui`.</span></span>

<span data-ttu-id="dcb06-136">Další informace najdete v tématu hello [Kubernetes úvodní](http://kubernetes.io/docs/user-guide/quick-start/).</span><span class="sxs-lookup"><span data-stu-id="dcb06-136">For more information, see hello [Kubernetes quick start](http://kubernetes.io/docs/user-guide/quick-start/).</span></span>

## <a name="connect-tooa-dcos-or-swarm-cluster"></a><span data-ttu-id="dcb06-137">Připojte tooa cluster DC/OS nebo Swarmu</span><span class="sxs-lookup"><span data-stu-id="dcb06-137">Connect tooa DC/OS or Swarm cluster</span></span>

<span data-ttu-id="dcb06-138">hello toouse DC/OS a Docker Swarm clustery nasazené v Azure Container Service, postupujte podle těchto pokynů toocreate tunelu SSH z místního systému Linux, systému macOS nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="dcb06-138">toouse hello DC/OS and Docker Swarm clusters deployed by Azure Container Service, follow these instructions toocreate a SSH tunnel from your local Linux, macOS, or Windows system.</span></span> 

> [!NOTE]
> <span data-ttu-id="dcb06-139">Tyto pokyny se zaměřují na tunelování přenosů TCP přes protokol SSH.</span><span class="sxs-lookup"><span data-stu-id="dcb06-139">These instructions focus on tunneling TCP traffic over SSH.</span></span> <span data-ttu-id="dcb06-140">Můžete také spustit interaktivní relace SSH s jedním z hello systémy správy interní clusteru, ale není to doporučeno.</span><span class="sxs-lookup"><span data-stu-id="dcb06-140">You can also start an interactive SSH session with one of hello internal cluster management systems, but we don't recommend this.</span></span> <span data-ttu-id="dcb06-141">Práce přímo v interním systému s sebou nese riziko neúmyslných změn konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dcb06-141">Working directly on an internal system risks inadvertent configuration changes.</span></span>  
> 

### <a name="create-an-ssh-tunnel-on-linux-or-macos"></a><span data-ttu-id="dcb06-142">Vytvoření tunelu SSH v Linuxu a macOS</span><span class="sxs-lookup"><span data-stu-id="dcb06-142">Create an SSH tunnel on Linux or macOS</span></span>
<span data-ttu-id="dcb06-143">Hello první věcí, kterou můžete provést při vytváření tunelového propojení SSH na Linux nebo systému macOS je toolocate hello veřejného názvu DNS hlavních serverů hello s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="dcb06-143">hello first thing that you do when you create an SSH tunnel on Linux or macOS is toolocate hello public DNS name of hello load-balanced masters.</span></span> <span data-ttu-id="dcb06-144">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="dcb06-144">Follow these steps:</span></span>


1. <span data-ttu-id="dcb06-145">V hello [portál Azure](https://portal.azure.com), procházet toohello skupinu prostředků obsahující vašeho clusteru kontejnerové služby.</span><span class="sxs-lookup"><span data-stu-id="dcb06-145">In hello [Azure portal](https://portal.azure.com), browse toohello resource group containing your container service cluster.</span></span> <span data-ttu-id="dcb06-146">Rozbalte skupinu prostředků hello tak, aby se zobrazí všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="dcb06-146">Expand hello resource group so that each resource is displayed.</span></span> 

2. <span data-ttu-id="dcb06-147">Klikněte na tlačítko hello **Container service** prostředků a klikněte na tlačítko **přehled**.</span><span class="sxs-lookup"><span data-stu-id="dcb06-147">Click hello **Container service** resource, and click **Overview**.</span></span> <span data-ttu-id="dcb06-148">Hello **plně kvalifikovaný název domény hlavního** z hello clusteru se zobrazí pod **Essentials**.</span><span class="sxs-lookup"><span data-stu-id="dcb06-148">hello **Master FQDN** of hello cluster appears under **Essentials**.</span></span> <span data-ttu-id="dcb06-149">Uložte si tento název pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="dcb06-149">Save this name for later use.</span></span> 

    ![Veřejný název DNS](./media/container-service-connect/pubdns.png)

    <span data-ttu-id="dcb06-151">Alternativně spusťte hello `az acs show` v kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="dcb06-151">Alternatively, run hello `az acs show` command on your container service.</span></span> <span data-ttu-id="dcb06-152">Vyhledejte hello **hlavní profilu: fqdn** vlastnost ve výstupu příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="dcb06-152">Look for hello **Master Profile:fqdn** property in hello command output.</span></span>

3. <span data-ttu-id="dcb06-153">Nyní otevřete prostředí a spusťte hello `ssh` příkaz zadáním hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="dcb06-153">Now open a shell and run hello `ssh` command by specifying hello following values:</span></span> 

    <span data-ttu-id="dcb06-154">**LOCAL_PORT** je port TCP hello na straně služby hello tooconnect tunelu hello k.</span><span class="sxs-lookup"><span data-stu-id="dcb06-154">**LOCAL_PORT** is hello TCP port on hello service side of hello tunnel tooconnect to.</span></span> <span data-ttu-id="dcb06-155">Pro Swarm nastavte tuto too2375.</span><span class="sxs-lookup"><span data-stu-id="dcb06-155">For Swarm, set this too2375.</span></span> <span data-ttu-id="dcb06-156">Pro DC/OS nastavte tuto too80.</span><span class="sxs-lookup"><span data-stu-id="dcb06-156">For DC/OS, set this too80.</span></span> 
    <span data-ttu-id="dcb06-157">**REMOTE_PORT** je port hello hello koncového bodu, které chcete tooexpose.</span><span class="sxs-lookup"><span data-stu-id="dcb06-157">**REMOTE_PORT** is hello port of hello endpoint that you want tooexpose.</span></span> <span data-ttu-id="dcb06-158">Pro Swarm použijte 2375.</span><span class="sxs-lookup"><span data-stu-id="dcb06-158">For Swarm, use port 2375.</span></span> <span data-ttu-id="dcb06-159">Pro DC/OS použijte port 80.</span><span class="sxs-lookup"><span data-stu-id="dcb06-159">For DC/OS, use port 80.</span></span>  
    <span data-ttu-id="dcb06-160">**Uživatelské jméno** je hello uživatelské jméno, které bylo zadáno při nasazení clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="dcb06-160">**USERNAME** is hello user name that was provided when you deployed hello cluster.</span></span>  
    <span data-ttu-id="dcb06-161">**DNSPREFIX** hello předpona DNS, který jste zadali při nasazení clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="dcb06-161">**DNSPREFIX** is hello DNS prefix that you provided when you deployed hello cluster.</span></span>  
    <span data-ttu-id="dcb06-162">**OBLAST** hello oblast, ve kterém se nachází vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="dcb06-162">**REGION** is hello region in which your resource group is located.</span></span>  
    <span data-ttu-id="dcb06-163">**PATH_TO_PRIVATE_KEY** [Nepovinné] je hello cesta toohello privátní klíč, který odpovídá toohello veřejnému klíči zadanému při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="dcb06-163">**PATH_TO_PRIVATE_KEY** [OPTIONAL] is hello path toohello private key that corresponds toohello public key you provided when you created hello cluster.</span></span> <span data-ttu-id="dcb06-164">Tuto možnost použijte s hello `-i` příznak.</span><span class="sxs-lookup"><span data-stu-id="dcb06-164">Use this option with hello `-i` flag.</span></span>

    ```bash
    ssh -fNL LOCAL_PORT:localhost:REMOTE_PORT -p 2200 [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com
    ```
  
  > [!NOTE]
  > <span data-ttu-id="dcb06-165">Hello port pro připojení SSH je 2200 a není hello standardní port 22.</span><span class="sxs-lookup"><span data-stu-id="dcb06-165">hello SSH connection port is 2200 and not hello standard port 22.</span></span> <span data-ttu-id="dcb06-166">V clusteru s více než jeden hlavní virtuální počítač je to hello připojení portu toohello první hlavní virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="dcb06-166">In a cluster with more than one master VM, this is hello connection port toohello first master VM.</span></span>
  > 

  <span data-ttu-id="dcb06-167">příkaz Hello vrátí bez výstupu.</span><span class="sxs-lookup"><span data-stu-id="dcb06-167">hello command returns without output.</span></span>

<span data-ttu-id="dcb06-168">Příklady hello pro DC/OS a Swarm v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="dcb06-168">See hello examples for DC/OS and Swarm in hello following sections.</span></span>    

### <a name="dcos-tunnel"></a><span data-ttu-id="dcb06-169">Tunel DC/OS</span><span class="sxs-lookup"><span data-stu-id="dcb06-169">DC/OS tunnel</span></span>
<span data-ttu-id="dcb06-170">tooopen tunel pro DC/OS koncových bodů, spusťte příkaz jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="dcb06-170">tooopen a tunnel for DC/OS endpoints, run a command like hello following:</span></span>

```bash
sudo ssh -fNL 80:localhost:80 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com 
```

> [!NOTE]
> <span data-ttu-id="dcb06-171">Ujistěte se, že se na port 80 neváže žádný jiný místní proces.</span><span class="sxs-lookup"><span data-stu-id="dcb06-171">Ensure that you do not have another local process that binds port 80.</span></span> <span data-ttu-id="dcb06-172">V případě potřeby můžete zadat jiný místní port než 80, třeba port 8080.</span><span class="sxs-lookup"><span data-stu-id="dcb06-172">If necessary, you can specify a local port other than port 80, such as port 8080.</span></span> <span data-ttu-id="dcb06-173">Při použití tohoto portu ale některé odkazy webového uživatelského rozhraní nemusí fungovat.</span><span class="sxs-lookup"><span data-stu-id="dcb06-173">However, some web UI links might not work when you use this port.</span></span>
>

<span data-ttu-id="dcb06-174">Nyní máte přístup koncových bodů DC/OS hello z místního systému prostřednictvím hello následující adresy URL (za předpokladu, že místní port 80):</span><span class="sxs-lookup"><span data-stu-id="dcb06-174">You can now access hello DC/OS endpoints from your local system through hello following URLs (assuming local port 80):</span></span>

* <span data-ttu-id="dcb06-175">DC/OS: `http://localhost:80/`</span><span class="sxs-lookup"><span data-stu-id="dcb06-175">DC/OS: `http://localhost:80/`</span></span>
* <span data-ttu-id="dcb06-176">Marathon: `http://localhost:80/marathon`</span><span class="sxs-lookup"><span data-stu-id="dcb06-176">Marathon: `http://localhost:80/marathon`</span></span>
* <span data-ttu-id="dcb06-177">Mesos: `http://localhost:80/mesos`</span><span class="sxs-lookup"><span data-stu-id="dcb06-177">Mesos: `http://localhost:80/mesos`</span></span>

<span data-ttu-id="dcb06-178">Podobně můžete dosáhnout hello rest API pro každou aplikaci přes tento tunel.</span><span class="sxs-lookup"><span data-stu-id="dcb06-178">Similarly, you can reach hello rest APIs for each application through this tunnel.</span></span>

### <a name="swarm-tunnel"></a><span data-ttu-id="dcb06-179">Tunel Swarm</span><span class="sxs-lookup"><span data-stu-id="dcb06-179">Swarm tunnel</span></span>
<span data-ttu-id="dcb06-180">tooopen koncový bod tunelu toohello Swarm, spusťte příkaz jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="dcb06-180">tooopen a tunnel toohello Swarm endpoint, run a command like hello following:</span></span>

```bash
ssh -fNL 2375:localhost:2375 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com
```
> [!NOTE]
> <span data-ttu-id="dcb06-181">Ujistěte se, že se na port 2375 neváže žádný jiný místní proces.</span><span class="sxs-lookup"><span data-stu-id="dcb06-181">Ensure that you do not have another local process that binds port 2375.</span></span> <span data-ttu-id="dcb06-182">Například pokud používáte hello Docker démon místně, je nastavena podle výchozí port toouse 2375.</span><span class="sxs-lookup"><span data-stu-id="dcb06-182">For example, if you are running hello Docker daemon locally, it's set by default toouse port 2375.</span></span> <span data-ttu-id="dcb06-183">V případě potřeby můžete zadat jiný místní port než 2375.</span><span class="sxs-lookup"><span data-stu-id="dcb06-183">If necessary, you can specify a local port other than port 2375.</span></span>
>

<span data-ttu-id="dcb06-184">Nyní můžete zobrazit hello Docker Swarm clusteru pomocí rozhraní příkazového řádku Dockeru hello (příkazového řádku Dockeru) v místním systému.</span><span class="sxs-lookup"><span data-stu-id="dcb06-184">Now you can access hello Docker Swarm cluster using hello Docker command-line interface (Docker CLI) on your local system.</span></span> <span data-ttu-id="dcb06-185">Pokyny k instalaci najdete v tématu [Instalace Dockeru](https://docs.docker.com/engine/installation/).</span><span class="sxs-lookup"><span data-stu-id="dcb06-185">For installation instructions, see [Install Docker](https://docs.docker.com/engine/installation/).</span></span>

<span data-ttu-id="dcb06-186">Nastavte vaše toohello proměnnou prostředí DOCKER_HOST místní port, který jste nakonfigurovali pro hello tunel.</span><span class="sxs-lookup"><span data-stu-id="dcb06-186">Set your DOCKER_HOST environment variable toohello local port you configured for hello tunnel.</span></span> 

```bash
export DOCKER_HOST=:2375
```

<span data-ttu-id="dcb06-187">Spusťte příkazy Docker clusteru Docker Swarm toohello tunelové propojení.</span><span class="sxs-lookup"><span data-stu-id="dcb06-187">Run Docker commands that tunnel toohello Docker Swarm cluster.</span></span> <span data-ttu-id="dcb06-188">Například:</span><span class="sxs-lookup"><span data-stu-id="dcb06-188">For example:</span></span>

```bash
docker info
```

### <a name="create-an-ssh-tunnel-on-windows"></a><span data-ttu-id="dcb06-189">Vytvoření tunelu SSH ve Windows</span><span class="sxs-lookup"><span data-stu-id="dcb06-189">Create an SSH tunnel on Windows</span></span>
<span data-ttu-id="dcb06-190">Tunely SSH je ve Windows možné vytvořit několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="dcb06-190">There are multiple options for creating SSH tunnels on Windows.</span></span> <span data-ttu-id="dcb06-191">Pokud Bash na Ubuntu běží na systému Windows nebo podobné nástroj, můžete postupovat podle pokynů tunelu hello SSH uvedené dříve v tomto článku systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="dcb06-191">If you are running Bash on Ubuntu on Windows or a similar tool, you can follow hello SSH tunneling instructions shown earlier in this article for macOS and Linux.</span></span> <span data-ttu-id="dcb06-192">Jako alternativu v systému Windows, tato část popisuje, jak toouse PuTTY toocreate hello tunel.</span><span class="sxs-lookup"><span data-stu-id="dcb06-192">As an alternative on Windows, this section describes how toouse PuTTY toocreate hello tunnel.</span></span>

1. <span data-ttu-id="dcb06-193">[Stáhněte si PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) tooyour systému Windows.</span><span class="sxs-lookup"><span data-stu-id="dcb06-193">[Download PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) tooyour Windows system.</span></span>

2. <span data-ttu-id="dcb06-194">Spusťte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="dcb06-194">Run hello application.</span></span>

3. <span data-ttu-id="dcb06-195">Zadejte název hostitele, který se skládá z uživatelského jména správce clusteru hello a hello veřejného názvu DNS prvního hlavního serveru hello v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="dcb06-195">Enter a host name that is comprised of hello cluster admin user name and hello public DNS name of hello first master in hello cluster.</span></span> <span data-ttu-id="dcb06-196">Hello **název hostitele** bude vypadat podobně jako příliš`azureuser@PublicDNSName`.</span><span class="sxs-lookup"><span data-stu-id="dcb06-196">hello **Host Name** looks similar too`azureuser@PublicDNSName`.</span></span> <span data-ttu-id="dcb06-197">Zadejte 2200 hello **Port**.</span><span class="sxs-lookup"><span data-stu-id="dcb06-197">Enter 2200 for hello **Port**.</span></span>

    ![Konfigurace PuTTY 1](./media/container-service-connect/putty1.png)

4. <span data-ttu-id="dcb06-199">Vyberte **SSH > Ověřování**. Přidejte cestu tooyour privátní klíče souboru (formát .ppk) pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="dcb06-199">Select **SSH > Auth**. Add a path tooyour private key file (.ppk format) for authentication.</span></span> <span data-ttu-id="dcb06-200">Nástroj můžete použít jako [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) toogenerate tento soubor z hello SSH klíče používané toocreate hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="dcb06-200">You can use a tool such as [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) toogenerate this file from hello SSH key used toocreate hello cluster.</span></span>

    ![Konfigurace PuTTY 2](./media/container-service-connect/putty2.png)

5. <span data-ttu-id="dcb06-202">Vyberte **SSH > tunely** a nakonfigurujte hello následující přesměrované porty:</span><span class="sxs-lookup"><span data-stu-id="dcb06-202">Select **SSH > Tunnels** and configure hello following forwarded ports:</span></span>

    * <span data-ttu-id="dcb06-203">**Zdrojový port:** Použijte 80 pro DC/OS nebo 2375 pro Swarm.</span><span class="sxs-lookup"><span data-stu-id="dcb06-203">**Source Port:** Use 80 for DC/OS or 2375 for Swarm.</span></span>
    * <span data-ttu-id="dcb06-204">**Cíl:** Použijte localhost:80 pro DC/OS nebo localhost:2375 pro Swarm.</span><span class="sxs-lookup"><span data-stu-id="dcb06-204">**Destination:** Use localhost:80 for DC/OS or localhost:2375 for Swarm.</span></span>

    <span data-ttu-id="dcb06-205">Hello následující příklad je nakonfigurován pro DC/OS, ale pro Docker Swarm bude vypadat podobně jako.</span><span class="sxs-lookup"><span data-stu-id="dcb06-205">hello following example is configured for DC/OS, but will look similar for Docker Swarm.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dcb06-206">Při vytváření tohoto tunelu se port 80 nesmí používat.</span><span class="sxs-lookup"><span data-stu-id="dcb06-206">Port 80 must not be in use when you create this tunnel.</span></span>
    > 

    ![Konfigurace PuTTY 3](./media/container-service-connect/putty3.png)

6. <span data-ttu-id="dcb06-208">Až budete hotovi, klikněte na tlačítko **relace > Uložit** konfigurace připojení toosave hello.</span><span class="sxs-lookup"><span data-stu-id="dcb06-208">When you're finished, click **Session > Save** toosave hello connection configuration.</span></span>

7. <span data-ttu-id="dcb06-209">tooconnect toohello PuTTY relace, klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="dcb06-209">tooconnect toohello PuTTY session, click **Open**.</span></span> <span data-ttu-id="dcb06-210">Jakmile se připojíte, uvidíte hello konfiguraci portů v protokolu událostí PuTTY hello.</span><span class="sxs-lookup"><span data-stu-id="dcb06-210">When you connect, you can see hello port configuration in hello PuTTY event log.</span></span>

    ![Protokol událostí PuTTY](./media/container-service-connect/putty4.png)

<span data-ttu-id="dcb06-212">Po dokončení konfigurace hello tunel pro DC/OS, dostanete hello související koncových bodů na:</span><span class="sxs-lookup"><span data-stu-id="dcb06-212">After you've configured hello tunnel for DC/OS, you can access hello related endpoints at:</span></span>

* <span data-ttu-id="dcb06-213">DC/OS: `http://localhost/`</span><span class="sxs-lookup"><span data-stu-id="dcb06-213">DC/OS: `http://localhost/`</span></span>
* <span data-ttu-id="dcb06-214">Marathon: `http://localhost/marathon`</span><span class="sxs-lookup"><span data-stu-id="dcb06-214">Marathon: `http://localhost/marathon`</span></span>
* <span data-ttu-id="dcb06-215">Mesos: `http://localhost/mesos`</span><span class="sxs-lookup"><span data-stu-id="dcb06-215">Mesos: `http://localhost/mesos`</span></span>

<span data-ttu-id="dcb06-216">Po dokončení konfigurace hello tunel pro Docker Swarm, otevřete váš Windows nastavení tooconfigure proměnné prostředí systému s názvem `DOCKER_HOST` s hodnotou `:2375`.</span><span class="sxs-lookup"><span data-stu-id="dcb06-216">After you've configured hello tunnel for Docker Swarm, open your Windows settings tooconfigure a system environment variable named `DOCKER_HOST` with a value of `:2375`.</span></span> <span data-ttu-id="dcb06-217">Potom můžete přistupovat clusteru Swarm hello prostřednictvím hello příkazového řádku Dockeru.</span><span class="sxs-lookup"><span data-stu-id="dcb06-217">Then, you can access hello Swarm cluster through hello Docker CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dcb06-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dcb06-218">Next steps</span></span>
<span data-ttu-id="dcb06-219">Nasazení a správa kontejnerů ve vašem clusteru:</span><span class="sxs-lookup"><span data-stu-id="dcb06-219">Deploy and manage containers in your cluster:</span></span>

* [<span data-ttu-id="dcb06-220">Práce s Azure Container Service a Kubernetes</span><span class="sxs-lookup"><span data-stu-id="dcb06-220">Work with Azure Container Service and Kubernetes</span></span>](../articles/container-service/kubernetes/container-service-kubernetes-ui.md)
* [<span data-ttu-id="dcb06-221">Práce se službou Azure Container Service a DC/OS</span><span class="sxs-lookup"><span data-stu-id="dcb06-221">Work with Azure Container Service and DC/OS</span></span>](../articles/container-service//dcos-swarm/container-service-mesos-marathon-rest.md)
* [<span data-ttu-id="dcb06-222">Práce s hello Azure Container Service a nástrojem Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="dcb06-222">Work with hello Azure Container Service and Docker Swarm</span></span>](../articles//container-service/dcos-swarm/container-service-docker-swarm.md)

