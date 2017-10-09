# <a name="scale-agent-nodes-in-a-container-service-cluster"></a>Škálování uzlů agentů v clusteru Container Service
Po [nasazení clusteru Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md), může být nutné toochange hello počet uzlů agenta. Například můžete potřebovat přidat více agentů, abyste mohli spouštět více instancí nebo aplikací typu kontejner. 

Můžete změnit hello počet agenta uzlů v clusteru DC/OS, Docker Swarm nebo Kubernetes pomocí hello portál Azure nebo hello 2.0 rozhraní příkazového řádku Azure. 

## <a name="scale-with-hello-azure-portal"></a>Škálování se hello portálu Azure

1. V hello [portál Azure](https://portal.azure.com), vyhledejte **služby kontejneru**a potom klikněte na službu kontejneru hello, které chcete toomodify.
2. V hello **Container service** okně klikněte na tlačítko **agenti**.
3. V **počet virtuálních počítačů**, zadejte hello požadovaného počtu uzlů agenty.

    ![Škálování fondu hello portálu](./media/container-service-scale/container-service-scale-portal.png)

4. toosave hello konfiguraci, klikněte na tlačítko **Uložit**.

## <a name="scale-with-hello-azure-cli-20"></a>Škálování se hello 2.0 rozhraní příkazového řádku Azure

Ujistěte se, že jste [nainstalován](/cli/azure/install-az-cli2) hello nejnovější Azure CLI 2.0 a přihlášení tooan účet azure (`az login`).

### <a name="see-hello-current-agent-count"></a>V tématu hello aktuální počet agentů
toosee hello počet agentů aktuálně hello clusteru, spusťte hello `az acs show` příkaz. Ukazuje to hello konfigurace clusteru. Například hello následující příkaz ukazuje hello konfigurace hello kontejneru služby s názvem `containerservice-myACSName` ve skupině prostředků hello `myResourceGroup`:

```azurecli
az acs show -g myResourceGroup -n containerservice-myACSName
```

příkaz Hello vrátí hello počet agentů v hello `Count` hodnoty v části `AgentPoolProfiles`.

### <a name="use-hello-az-acs-scale-command"></a>Použití hello az příkaz škálování služby acs
toochange hello počet uzlů agenta, spusťte hello `az acs scale` příkazu a dodávky hello **skupiny prostředků**, **název kontejneru služby**a hello potřeby **nový počet agentů**. Použitím nižšího nebo vyššího čísla můžete vertikálně snížit nebo navýšit kapacitu.

Například toochange hello počet agentů v hello předchozí too10 clusteru, zadejte následující příkaz hello:

```azurecli
az acs scale -g myResourceGroup -n containerservice-myACSName --new-agent-count 10
```

Hello Azure CLI 2.0 vrátí JSON řetězec představující hello novou konfiguraci kontejneru služby hello, včetně počtu nových agenta hello.

Další možnosti příkazu zobrazíte spuštěním příkazu `az acs scale --help`.

## <a name="scaling-considerations"></a>Důležité informace o škálování

* Hello počet uzlů agent musí být mezi 1 a 100, včetně. 

* Vaší kvóty jader můžete omezit počet hello agenta uzlů v clusteru.

* Operace škálování uzlu agenta jsou sadě škálování virtuálního počítače Azure použité tooan obsahující fond agenta hello. V clusteru DC/OS jsou pouze agent uzlů ve fondu privátní hello škálovat operacemi hello uvedené v tomto článku.

* V závislosti na hello orchestrator, které můžete nasadit v clusteru můžete nezávisle škálovat hello počet instancí kontejneru běžící v clusteru s hello. Například v clusteru DC/OS, použijte hello [uživatelského rozhraní Marathon](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md) toochange hello počet instancí kontejneru aplikace.

* Automatické škálování uzlů agentů v clusteru Container Service aktuálně není podporováno.

## <a name="next-steps"></a>Další kroky
* Podívejte se na [další příklady](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) použití příkazů Azure CLI 2.0 se službou Azure Container Service.
* Další informace o [fondech agentů DC/OS](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) ve službě Azure Container Service.

