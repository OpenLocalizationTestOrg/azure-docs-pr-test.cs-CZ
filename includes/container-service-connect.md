# <a name="make-a-remote-connection-tooa-kubernetes-dcos-or-docker-swarm-cluster"></a>Ujistěte se, tooa Kubernetes, DC/OS nebo Docker Swarm clusteru vzdáleného připojení
Po vytvoření clusteru Azure Container Service, třeba tooconnect toohello clusteru toodeploy a spravovat úlohy. Tento článek popisuje, jak tooconnect toohello hlavní počítač hello clusteru ze vzdáleného počítače. 

Hello Kubernetes, DC/OS a Docker Swarm clustery poskytují koncové body HTTP místně. Pro Kubernetes, je tento koncový bod bezpečně zveřejněné na hello internet a můžete k němu přístup spuštěním hello `kubectl` nástroj pro příkazový řádek z libovolného počítače připojeného k Internetu. 

Pro DC/OS a Docker Swarm doporučujeme vytvořit tunel zabezpečené shell (SSH) z váš systém správy clusteru toohello místního počítače. Po hello tunelového propojení, můžete spouštět příkazy, které používají koncové body HTTP hello a zobrazení hello orchestrator webové rozhraní (Pokud je k dispozici) z místního systému. 

## <a name="prerequisites"></a>Požadavky

* Cluster Kubernetes, DC/OS nebo Docker Swarm [nasazený ve službě Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md).
* SSH RSA soubor privátního klíče, odpovídající toohello veřejného klíče přidané toohello clusteru během nasazení. Těchto příkazů se předpokládá hello privátní klíč SSH představuje v `$HOME/.ssh/id_rsa` ve vašem počítači. Tyto pokyny existují také pro [macOS a Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) nebo pro [Windows](../articles/virtual-machines/linux/ssh-from-windows.md). Pokud hello připojení SSH není funkční, bude pravděpodobně nutné příliš [resetovat vaše klíče SSH](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).

## <a name="connect-tooa-kubernetes-cluster"></a>Připojte tooa Kubernetes cluster

Postupujte podle těchto kroků tooinstall a nakonfigurovat `kubectl` ve vašem počítači.

> [!NOTE] 
> V systému macOS nebo Linux, může být nutné toorun hello příkazy v této části pomocí `sudo`.
> 

### <a name="install-kubectl"></a>Instalace kubectl
Jedním ze způsobů tooinstall tento nástroj je toouse hello `az acs kubernetes install-cli` příkaz 2.0 rozhraní příkazového řádku Azure. toorun tento příkaz, ujistěte se, že jste [nainstalován](/cli/azure/install-az-cli2) hello nejnovější Azure CLI 2.0 a přihlášení tooan účet Azure (`az login`).

```azurecli
# Linux or macOS
az acs kubernetes install-cli [--install-location=/some/directory/kubectl]

# Windows
az acs kubernetes install-cli [--install-location=C:\some\directory\kubectl.exe]
```

Alternativně můžete stáhnout hello nejnovější `kubectl` klienta přímo z hello [Kubernetes uvolní stránky](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md). Další informace najdete v tématu [Instalace a nastavení kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).

### <a name="download-cluster-credentials"></a>Stažení přihlašovacích údajů clusteru
Jakmile máte `kubectl` nainstalovali, musíte toocopy hello clusteru pověření tooyour počítače. Jedním ze způsobů toodo get hello přihlašovací údaje jsou s hello `az acs kubernetes get-credentials` příkaz. Předejte hello název skupiny prostředků hello a hello název prostředku služby kontejneru hello:

```azurecli
az acs kubernetes get-credentials --resource-group=<cluster-resource-group> --name=<cluster-name>
```

Tento příkaz stáhne hello clusteru pověření příliš`$HOME/.kube/config`, kde `kubectl` očekává, že toobe nachází.

Alternativně můžete použít `scp` toosecurely kopírovat hello soubor z `$HOME/.kube/config` na hello hlavní počítač tooyour místního počítače. Například:

```bash
mkdir $HOME/.kube
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

Pokud jste v systému Windows, můžete na Ubuntu na Windows, klient kopie PuTTy zabezpečeného souboru hello nebo podobné nástroj Bash.

### <a name="use-kubectl"></a>Použití kubectl

Jakmile máte `kubectl` nakonfigurovaná, testovací hello připojení tak, že uvedete hello uzlů v clusteru:

```bash
kubectl get nodes
```

Můžete vyzkoušet i další příkazy nástroje `kubectl`. Můžete například zobrazit hello Kubernetes řídicího panelu. Nejprve spusťte proxy server toohello Kubernetes API:

```bash
kubectl proxy
```

Hello Kubernetes uživatelského rozhraní je nyní k dispozici na: `http://localhost:8001/ui`.

Další informace najdete v tématu hello [Kubernetes úvodní](http://kubernetes.io/docs/user-guide/quick-start/).

## <a name="connect-tooa-dcos-or-swarm-cluster"></a>Připojte tooa cluster DC/OS nebo Swarmu

hello toouse DC/OS a Docker Swarm clustery nasazené v Azure Container Service, postupujte podle těchto pokynů toocreate tunelu SSH z místního systému Linux, systému macOS nebo Windows. 

> [!NOTE]
> Tyto pokyny se zaměřují na tunelování přenosů TCP přes protokol SSH. Můžete také spustit interaktivní relace SSH s jedním z hello systémy správy interní clusteru, ale není to doporučeno. Práce přímo v interním systému s sebou nese riziko neúmyslných změn konfigurace.  
> 

### <a name="create-an-ssh-tunnel-on-linux-or-macos"></a>Vytvoření tunelu SSH v Linuxu a macOS
Hello první věcí, kterou můžete provést při vytváření tunelového propojení SSH na Linux nebo systému macOS je toolocate hello veřejného názvu DNS hlavních serverů hello s vyrovnáváním zatížení. Postupujte následovně:


1. V hello [portál Azure](https://portal.azure.com), procházet toohello skupinu prostředků obsahující vašeho clusteru kontejnerové služby. Rozbalte skupinu prostředků hello tak, aby se zobrazí všechny prostředky. 

2. Klikněte na tlačítko hello **Container service** prostředků a klikněte na tlačítko **přehled**. Hello **plně kvalifikovaný název domény hlavního** z hello clusteru se zobrazí pod **Essentials**. Uložte si tento název pro pozdější použití. 

    ![Veřejný název DNS](./media/container-service-connect/pubdns.png)

    Alternativně spusťte hello `az acs show` v kontejneru služby. Vyhledejte hello **hlavní profilu: fqdn** vlastnost ve výstupu příkazu hello.

3. Nyní otevřete prostředí a spusťte hello `ssh` příkaz zadáním hello následující hodnoty: 

    **LOCAL_PORT** je port TCP hello na straně služby hello tooconnect tunelu hello k. Pro Swarm nastavte tuto too2375. Pro DC/OS nastavte tuto too80. 
    **REMOTE_PORT** je port hello hello koncového bodu, které chcete tooexpose. Pro Swarm použijte 2375. Pro DC/OS použijte port 80.  
    **Uživatelské jméno** je hello uživatelské jméno, které bylo zadáno při nasazení clusteru hello.  
    **DNSPREFIX** hello předpona DNS, který jste zadali při nasazení clusteru hello.  
    **OBLAST** hello oblast, ve kterém se nachází vaší skupiny prostředků.  
    **PATH_TO_PRIVATE_KEY** [Nepovinné] je hello cesta toohello privátní klíč, který odpovídá toohello veřejnému klíči zadanému při vytváření clusteru hello. Tuto možnost použijte s hello `-i` příznak.

    ```bash
    ssh -fNL LOCAL_PORT:localhost:REMOTE_PORT -p 2200 [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com
    ```
  
  > [!NOTE]
  > Hello port pro připojení SSH je 2200 a není hello standardní port 22. V clusteru s více než jeden hlavní virtuální počítač je to hello připojení portu toohello první hlavní virtuálních počítačů.
  > 

  příkaz Hello vrátí bez výstupu.

Příklady hello pro DC/OS a Swarm v následující části hello.    

### <a name="dcos-tunnel"></a>Tunel DC/OS
tooopen tunel pro DC/OS koncových bodů, spusťte příkaz jako hello následující:

```bash
sudo ssh -fNL 80:localhost:80 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com 
```

> [!NOTE]
> Ujistěte se, že se na port 80 neváže žádný jiný místní proces. V případě potřeby můžete zadat jiný místní port než 80, třeba port 8080. Při použití tohoto portu ale některé odkazy webového uživatelského rozhraní nemusí fungovat.
>

Nyní máte přístup koncových bodů DC/OS hello z místního systému prostřednictvím hello následující adresy URL (za předpokladu, že místní port 80):

* DC/OS: `http://localhost:80/`
* Marathon: `http://localhost:80/marathon`
* Mesos: `http://localhost:80/mesos`

Podobně můžete dosáhnout hello rest API pro každou aplikaci přes tento tunel.

### <a name="swarm-tunnel"></a>Tunel Swarm
tooopen koncový bod tunelu toohello Swarm, spusťte příkaz jako hello následující:

```bash
ssh -fNL 2375:localhost:2375 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com
```
> [!NOTE]
> Ujistěte se, že se na port 2375 neváže žádný jiný místní proces. Například pokud používáte hello Docker démon místně, je nastavena podle výchozí port toouse 2375. V případě potřeby můžete zadat jiný místní port než 2375.
>

Nyní můžete zobrazit hello Docker Swarm clusteru pomocí rozhraní příkazového řádku Dockeru hello (příkazového řádku Dockeru) v místním systému. Pokyny k instalaci najdete v tématu [Instalace Dockeru](https://docs.docker.com/engine/installation/).

Nastavte vaše toohello proměnnou prostředí DOCKER_HOST místní port, který jste nakonfigurovali pro hello tunel. 

```bash
export DOCKER_HOST=:2375
```

Spusťte příkazy Docker clusteru Docker Swarm toohello tunelové propojení. Například:

```bash
docker info
```

### <a name="create-an-ssh-tunnel-on-windows"></a>Vytvoření tunelu SSH ve Windows
Tunely SSH je ve Windows možné vytvořit několika způsoby. Pokud Bash na Ubuntu běží na systému Windows nebo podobné nástroj, můžete postupovat podle pokynů tunelu hello SSH uvedené dříve v tomto článku systému macOS a Linux. Jako alternativu v systému Windows, tato část popisuje, jak toouse PuTTY toocreate hello tunel.

1. [Stáhněte si PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) tooyour systému Windows.

2. Spusťte aplikaci hello.

3. Zadejte název hostitele, který se skládá z uživatelského jména správce clusteru hello a hello veřejného názvu DNS prvního hlavního serveru hello v clusteru hello. Hello **název hostitele** bude vypadat podobně jako příliš`azureuser@PublicDNSName`. Zadejte 2200 hello **Port**.

    ![Konfigurace PuTTY 1](./media/container-service-connect/putty1.png)

4. Vyberte **SSH > Ověřování**. Přidejte cestu tooyour privátní klíče souboru (formát .ppk) pro ověřování. Nástroj můžete použít jako [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) toogenerate tento soubor z hello SSH klíče používané toocreate hello clusteru.

    ![Konfigurace PuTTY 2](./media/container-service-connect/putty2.png)

5. Vyberte **SSH > tunely** a nakonfigurujte hello následující přesměrované porty:

    * **Zdrojový port:** Použijte 80 pro DC/OS nebo 2375 pro Swarm.
    * **Cíl:** Použijte localhost:80 pro DC/OS nebo localhost:2375 pro Swarm.

    Hello následující příklad je nakonfigurován pro DC/OS, ale pro Docker Swarm bude vypadat podobně jako.

    > [!NOTE]
    > Při vytváření tohoto tunelu se port 80 nesmí používat.
    > 

    ![Konfigurace PuTTY 3](./media/container-service-connect/putty3.png)

6. Až budete hotovi, klikněte na tlačítko **relace > Uložit** konfigurace připojení toosave hello.

7. tooconnect toohello PuTTY relace, klikněte na tlačítko **otevřete**. Jakmile se připojíte, uvidíte hello konfiguraci portů v protokolu událostí PuTTY hello.

    ![Protokol událostí PuTTY](./media/container-service-connect/putty4.png)

Po dokončení konfigurace hello tunel pro DC/OS, dostanete hello související koncových bodů na:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Po dokončení konfigurace hello tunel pro Docker Swarm, otevřete váš Windows nastavení tooconfigure proměnné prostředí systému s názvem `DOCKER_HOST` s hodnotou `:2375`. Potom můžete přistupovat clusteru Swarm hello prostřednictvím hello příkazového řádku Dockeru.

## <a name="next-steps"></a>Další kroky
Nasazení a správa kontejnerů ve vašem clusteru:

* [Práce s Azure Container Service a Kubernetes](../articles/container-service/kubernetes/container-service-kubernetes-ui.md)
* [Práce se službou Azure Container Service a DC/OS](../articles/container-service//dcos-swarm/container-service-mesos-marathon-rest.md)
* [Práce s hello Azure Container Service a nástrojem Docker Swarm](../articles//container-service/dcos-swarm/container-service-docker-swarm.md)

