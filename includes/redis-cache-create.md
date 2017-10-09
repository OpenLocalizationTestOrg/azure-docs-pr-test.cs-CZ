toocreate mezipaměti, nejdřív přihlásit toohello [portál Azure](https://portal.azure.com)a klikněte na tlačítko **nový** > **databáze** > **Redis Cache**.

> [!NOTE]
> Pokud účet Azure nemáte, můžete si během několika minut [vytvořit bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).
> 
> 

![Nová mezipaměť](media/redis-cache-create/redis-cache-new-cache-menu.png)

> [!NOTE]
> Kromě toho toocreating ukládá do mezipaměti v hello portálu Azure, můžete vytvořit také pomocí Správce prostředků šablony, prostředí PowerShell nebo rozhraní příkazového řádku Azure.
> 
> * toocreate a mezipaměti pomocí šablony Resource Manageru, najdete v části [vytvoření mezipaměti Redis pomocí šablony](../articles/redis-cache/cache-redis-cache-arm-provision.md).
> * toocreate mezipaměti pomocí Azure PowerShell, najdete v části [spravovat Azure Redis Cache pomocí Azure Powershellu](../articles/redis-cache/cache-howto-manage-redis-cache-powershell.md).
> * toocreate mezipaměti pomocí rozhraní příkazového řádku Azure, najdete v části [jak toocreate a spravovat Azure Redis Cache pomocí rozhraní příkazového řádku Azure (Azure CLI) hello](../articles/redis-cache/cache-manage-cli.md).
> 
> 

V hello **nová mezipaměť Redis** okno, zadejte požadovanou konfiguraci mezipaměti hello hello.

![Vytvoření mezipaměti](media/redis-cache-create/redis-cache-cache-create.png) 

* V **název Dns**, zadejte toouse mezipaměti jedinečný název pro koncový bod mezipaměti hello. Název mezipaměti Hello musí být řetězec o délce 1 až 63 znaků a obsahovat jenom čísla, písmena a hello `-` znak. Hello název mezipaměti nesmí začínat ani končit hello `-` znak a po sobě jdoucí `-` znaky nejsou platné.
* Pro **předplatné**, vyberte hello předplatné Azure, že chcete toouse pro mezipaměť hello. Pokud má váš účet jenom jedno předplatné, bude automaticky vybrána a hello **předplatné** rozevírací seznam se nezobrazí.
* V části **Skupina prostředků** vyberte nebo vytvořte skupinu prostředků pro mezipaměť. Další informace najdete v tématu [skupiny zdrojů pomocí toomanage vašich prostředků Azure](../articles/azure-resource-manager/resource-group-overview.md). 
* Použití **umístění** toospecify hello zeměpisného umístění, ve kterém se mezipaměť hostuje. Pro zajištění nejlepšího výkonu hello společnost Microsoft důrazně doporučuje vytvoření mezipaměti hello v hello stejné oblasti jako klientská aplikace mezipaměti hello.
* Použití **cenová úroveň** tooselect hello potřeby velikost mezipaměti a funkce.
* **Redis cluster** vám umožní toocreate mezipamětí větší než 53 GB a tooshard data mezi různými uzly Redis. Další informace najdete v tématu [jak tooconfigure clusteringu pro mezipaměť Azure Redis Cache Premium](../articles/redis-cache/cache-how-to-premium-clustering.md).
* **Trvalost redis** nabízí možnost toopersist hello vaší mezipaměti tooan účet úložiště Azure. Pokyny týkající se konfigurace trvalosti najdete v tématu [jak tooconfigure trvalosti pro mezipaměť Azure Redis Cache Premium](../articles/redis-cache/cache-how-to-premium-persistence.md).
* **Virtuální síť** poskytuje lepší zabezpečení a izolaci omezením přístupu tooyour mezipaměti tooonly tyto klienty v rámci hello zadané virtuální síti Azure. Všechny funkce hello virtuální sítě můžete použít, například podsítě, zásady řízení přístupu a další funkce toofurther omezit přístup tooRedis. Další informace najdete v tématu [jak podporují tooconfigure virtuální sítě pro mezipaměť Azure Redis Cache Premium](../articles/redis-cache/cache-how-to-premium-vnet.md).
* Přístup bez SSL je ve výchozím nastavení pro nové mezipaměti zakázaný. port bez SSL tooenable hello, zkontrolujte **odblokovat port 6379 (nejsou šifrovaná protokolem SSL)**.

Po nakonfigurování možností nové mezipaměti hello klikněte na tlačítko **vytvořit**. Může trvat několik minut, než toobe mezipaměti hello vytvořili. toocheck hello stav, můžete sledovat průběh hello na úvodní panel hello. Po vytvoření mezipaměti hello má novou mezipaměť **systémem** stavu a je připravený k použití s [výchozí nastavení](../articles/redis-cache/cache-configure.md#default-redis-server-configuration).

![Mezipaměť vytvořena](media/redis-cache-create/redis-cache-cache-created.png)

