# <a name="container-service-frequently-asked-questions"></a>Nejčastější dotazy ke službě Azure Container Service

## <a name="orchestrators"></a>Orchestrátory

### <a name="which-container-orchestrators-do-you-support-on-azure-container-service"></a>Jaké orchestrátory kontejneru podporujete ve službě Azure Container Service? 

Podporuje se open source DC/OS, Docker Swarm a Kubernetes. Další informace najdete v tématu hello [přehled](../articles/container-service/kubernetes/container-service-intro-kubernetes.md).
 
### <a name="do-you-support-docker-swarm-mode"></a>Podporujete režim Docker Swarm? 

Aktuálně se nepodporuje režim Swarm, ale je na plán služby hello. 

### <a name="does-azure-container-service-support-windows-containers"></a>Podporuje služba Azure Container Service kontejnery Windows?  

Aktuálně jsou podporovány kontejnery Linux se všemi orchestrátory. Podpora pro Windows kontejnery s Kubernetes je ve verzi Preview.

### <a name="do-you-recommend-a-specific-orchestrator-in-azure-container-service"></a>Doporučujete ve službě Azure Container Service použití konkrétního orchestrátoru? 
Obecně nedoporučujeme konkrétní orchestrator. Pokud máte zkušenosti s jedním z orchestrators hello podporována, můžete použít tento prostředí v Azure Container Service. Trendy v datech nicméně naznačují, že systém DC/OS se v produkčním prostředí osvědčil pro úlohy s velkým objemem dat a úlohy IoT, Kubernetes se skvěle hodí pro úlohy nativní pro cloud a Docker Swarm je známý svou integrací s nástroji Dockeru a jednoduchostí osvojování.

V závislosti na scénáři můžete také vytvořit a spravovat vlastní řešení kontejnerů pomocí dalších služeb Azure. Mezi tyto služby patří [Virtual Machines](../articles/virtual-machines/linux/overview.md), [Service Fabric](../articles/service-fabric/service-fabric-overview.md), [Web Apps](../articles/app-service-web/app-service-web-overview.md) a [Batch](../articles/batch/batch-technical-overview.md).  

### <a name="what-is-hello-difference-between-azure-container-service-and-acs-engine"></a>Co je hello rozdíl mezi Azure Container Service a ACS modulu? 
Azure Container Service je zálohovaný SLA služby Azure pomocí funkcí v hello portálu Azure, nástroje příkazového řádku Azure a rozhraní API služby Azure. Služba Hello umožňuje tooquickly implementace a Správa clusterů s relativně malý počet možnostmi konfigurace nástroje orchestration standardní kontejneru. 

[Modul služby ACS](http://github.com/Azure/acs-engine) je open-source projekt, který umožňuje konfiguraci clusteru power uživatelé toocustomize hello na všech úrovních. Tuto možnost tooalter hello konfiguraci infrastruktury a softwaru znamená, že nabízíme žádné SLA pro modul služby ACS. Podpora zpracovává prostřednictvím hello open source projektu na Githubu, a nikoli prostřednictvím oficiální Microsoft kanálů. 

## <a name="cluster-management"></a>Správa clusteru

### <a name="how-do-i-create-ssh-keys-for-my-cluster"></a>Jak pro cluster vytvořím klíče SSH?

Můžete vytvořit standardní nástroje na váš operační systém toocreate SSH veřejné a privátní pár klíče RSA pro ověřování proti hello Linuxové virtuální počítače pro váš cluster. Pokyny najdete v tématu hello [OS X a Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) nebo [Windows](../articles/virtual-machines/linux/ssh-from-windows.md) pokyny. 

Pokud používáte [příkazy Azure CLI 2.0](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) toodeploy a kontejneru služby clusteru, SSH klíče můžete automaticky generovány pro váš cluster.

### <a name="how-do-i-create-a-service-principal-for-my-kubernetes-cluster"></a>Jak vytvořím instanční objekt pro cluster Kubernetes?

ID objektu zabezpečení služby Azure Active Directory a heslo jsou také potřebná toocreate Kubernetes cluster Azure Container Service. Další informace najdete v tématu [o hello instanční objekt pro cluster s podporou Kubernetes](../articles/container-service/kubernetes/container-service-kubernetes-service-principal.md).

Pokud používáte [příkazy Azure CLI 2.0](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) toodeploy Kubernetes clusteru, služba hlavní přihlašovací údaje pro svůj cluster automaticky generovány.

### <a name="how-large-a-cluster-can-i-create"></a>Jak velký cluster můžu vytvořit?
Můžete vytvořit cluster s 1, 3 nebo 5 řídicími uzly. Můžete si too100 agenta uzly.

> [!IMPORTANT]
> Pro větší clustery a v závislosti na hello velikost virtuálního počítače, které zvolíte pro uzly bude pravděpodobně nutné tooincrease hello jader kvóty ve vašem předplatném. toorequest zvýšení kvóty, otevřete [žádost o podporu online zákazníka](../articles/azure-supportability/how-to-create-azure-support-request.md) zdarma. Pokud používáte [bezplatný účet Azure](https://azure.microsoft.com/free/), můžete použít pouze omezený počet výpočetních jader Azure.
> 

### <a name="how-do-i-increase-hello-number-of-masters-after-a-cluster-is-created"></a>Jak se po vytvoření clusteru s podporou zvýšit hello: počet hlavních serverů? 
Po vytvoření clusteru hello hello: počet hlavních serverů je pevná a nedá se změnit. Během vytváření hello hello clusteru v ideálním případě byste měli vybrat více hlavních serverů pro zajištění vysoké dostupnosti.

### <a name="how-do-i-increase-hello-number-of-agents-after-a-cluster-is-created"></a>Jak se po vytvoření clusteru s podporou zvýšit hello počet agentů? 
Je možné škálovat hello počet agentů v clusteru hello pomocí hello portál Azure nebo nástroje příkazového řádku. Viz [Škálování clusteru Azure Container Service](../articles/container-service/kubernetes/container-service-scale.md).

### <a name="what-are-hello-urls-of-my-masters-and-agents"></a>Co jsou adresy URL hello Moje hlavních serverů a agentů? 
Hello adresy URL clusteru, který prostředky v Azure Container Service jsou založené na hello názvů DNS Předpona zadejte a hello název hello oblast Azure, které jste zvolili pro nasazení. Například hello plně kvalifikovaný název domény (FQDN) hlavního uzlu hello je tento formulář:

``` 
DNSnamePrefix.AzureRegion.cloudapp.azure.net
```

Můžete najít běžně používané adresy URL pro váš cluster v hello portálu Azure, hello Průzkumníka prostředků Azure nebo jiné nástroje Azure.

### <a name="how-do-i-tell-which-orchestrator-version-is-running-in-my-cluster"></a>Jak zjistím, která verze orchestrátoru je spuštěna v mém clusteru?

* DC/OS: Viz hello [dokumentaci Mesosphere](https://support.mesosphere.com/hc/en-us/articles/207719793-How-to-get-the-DCOS-version-from-the-command-line-)
* Docker Swarm: Spusťte příkaz `docker version`.
* Kubernetes: Spusťte příkaz `kubectl version`.

### <a name="how-do-i-upgrade-hello-orchestrator-after-deployment"></a>Jak upgradovat hello orchestrator po nasazení?

V současné době Azure Container Service neposkytuje nástroje tooupgrade hello verze hello orchestrator, které jste nasadili cluster. Pokud služba Container Service podporuje novější verzi, můžete nasadit nový cluster. Další možností je nástroje orchestrator specifické toouse, pokud jsou k dispozici tooupgrade cluster na místě. Podívejte se například na [Upgrade DC/OS](https://dcos.io/docs/1.8/administration/upgrading/).
 
### <a name="where-do-i-find-hello-ssh-connection-string-toomy-cluster"></a>Kde hello SSH připojovací řetězec toomy clusteru

Připojovací řetězec hello můžete najít v hello portál Azure nebo pomocí nástroje příkazového řádku Azure. 

1. Hello portálu přejděte toohello skupinu prostředků pro nasazení clusteru hello.  

2. Klikněte na tlačítko **přehled** a klikněte na odkaz hello **nasazení** pod **Essentials**. 

3. V hello **historii nasazení** okně klikněte na tlačítko hello nasazení, který má název začíná **microsoft acs** následuje datum nasazení. Příklad: microsoft-acs-201701310000.  

4. Na hello **Souhrn** v části **výstupy**, jsou k dispozici několik odkazy clusteru. **SSHMaster0** poskytuje SSH připojovací řetězec toohello prvního hlavního serveru v clusteru služby kontejneru. 

Jak je uvedeno dříve můžete použít také nástroje Azure toofind hello plně kvalifikovaný název domény hlavního serveru hello. Vytvoření připojení SSH toohello hlavní server pomocí hello plně kvalifikovaný název domény hello hlavní a hello uživatelské jméno, které jste zadali při vytváření clusteru hello. Například:

```bash
ssh userName@masterFQDN –A –p 22 
```

Další informace najdete v tématu [připojení clusteru Azure Container Service tooan](../articles/container-service/kubernetes/container-service-connect.md).

## <a name="next-steps"></a>Další kroky

* [Další informace](../articles/container-service/kubernetes/container-service-intro-kubernetes.md) o službě Azure Container Service.
* Nasazení clusteru služby kontejneru pomocí hello [portál](../articles/container-service/dcos-swarm/container-service-deployment.md) nebo [Azure CLI 2.0](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md).
