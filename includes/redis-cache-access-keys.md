<span data-ttu-id="83c5e-101">instance služby Azure Redis Cache tooan tooconnect, klienti mezipaměti musí hello název hostitele, porty a klíče mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="83c5e-101">tooconnect tooan Azure Redis Cache instance, cache clients need hello host name, ports, and keys of hello cache.</span></span> <span data-ttu-id="83c5e-102">Někteří klienti mohou odkazovat toothese položky mírně odlišné názvy.</span><span class="sxs-lookup"><span data-stu-id="83c5e-102">Some clients may refer toothese items by slightly different names.</span></span> <span data-ttu-id="83c5e-103">Tyto informace v hello portál Azure nebo pomocí nástroje příkazového řádku, jako je například rozhraní příkazového řádku Azure můžete načíst.</span><span class="sxs-lookup"><span data-stu-id="83c5e-103">You can retrieve this information in hello Azure portal or by using command-line tools such as Azure CLI.</span></span>

### <a name="retrieve-host-name-ports-and-access-keys-using-hello-azure-portal"></a><span data-ttu-id="83c5e-104">Načíst název hostitele, porty a přístupové klíče pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="83c5e-104">Retrieve host name, ports, and access keys using hello Azure Portal</span></span>
<span data-ttu-id="83c5e-105">tooretrieve hostitelem název, porty a přístupové klíče pomocí portálu Azure, hello [Procházet](../articles/redis-cache/cache-configure.md#configure-redis-cache-settings) tooyour mezipaměti hello [portál Azure](https://portal.azure.com) a klikněte na tlačítko **přístupové klíče** a  **Vlastnosti** v hello **prostředků nabídky**.</span><span class="sxs-lookup"><span data-stu-id="83c5e-105">tooretrieve host name, ports, and access keys using hello Azure Portal, [browse](../articles/redis-cache/cache-configure.md#configure-redis-cache-settings) tooyour cache in hello [Azure portal](https://portal.azure.com) and click **Access keys** and **Properties** in hello **Resource menu**.</span></span> 

![Nastavení mezipaměti Redis](media/redis-cache-access-keys/redis-cache-hostname-ports-keys.png)

### <a name="retrieve-host-name-ports-and-access-keys-using-azure-cli"></a><span data-ttu-id="83c5e-107">Načtení názvu hostitele, portů a přístupových klíčů pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="83c5e-107">Retrieve host name, ports, and access keys using Azure CLI</span></span>
<span data-ttu-id="83c5e-108">název hostitele hello tooretrieve a porty pomocí Azure CLI 2.0 můžete volat [az redis zobrazit](https://docs.microsoft.com/cli/azure/redis#show)a klíče hello tooretrieve můžete volat [az redis seznamu klíčů](https://docs.microsoft.com/cli/azure/redis#list-keys).</span><span class="sxs-lookup"><span data-stu-id="83c5e-108">tooretrieve hello host name and ports using Azure CLI 2.0 you can call [az redis show](https://docs.microsoft.com/cli/azure/redis#show), and tooretrieve hello keys you can call [az redis list-keys](https://docs.microsoft.com/cli/azure/redis#list-keys).</span></span> <span data-ttu-id="83c5e-109">Hello následující skript volá tyto dva příkazy a toto využití hello název hostitele, porty a klíče toohello konzoly.</span><span class="sxs-lookup"><span data-stu-id="83c5e-109">hello following script calls these two commands and echos hello hostname, ports, and keys toohello console.</span></span>

```azurecli
#/bin/bash

# Retrieve hello hostname, ports, and keys for contosoCache located in contosoGroup

# Retrieve hello hostname and ports for an Azure Redis Cache instance
redis=($(az redis show --name contosoCache --resource-group contosoGroup --query [hostName,enableNonSslPort,port,sslPort] --output tsv))

# Retrieve hello keys for an Azure Redis Cache instance
keys=($(az redis list-keys --name contosoCache --resource-group contosoGroup --query [primaryKey,secondaryKey] --output tsv))

# Display hello retrieved hostname, keys, and ports
echo "Hostname:" ${redis[0]}
echo "Non SSL Port:" ${redis[2]}
echo "Non SSL Port Enabled:" ${redis[1]}
echo "SSL Port:" ${redis[3]}
echo "Primary Key:" ${keys[0]}
echo "Secondary Key:" ${keys[1]}
```

<span data-ttu-id="83c5e-110">Další informace o tento skript najdete v tématu [získat název hostitele hello, porty a klíče pro Azure Redis Cache](../articles/redis-cache/scripts/cache-keys-ports.md).</span><span class="sxs-lookup"><span data-stu-id="83c5e-110">For more information about this script, see [Get hello hostname, ports, and keys for Azure Redis Cache](../articles/redis-cache/scripts/cache-keys-ports.md).</span></span> <span data-ttu-id="83c5e-111">Další informace o Azure CLI 2.0 najdete v tématech [Instalace Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) a [Začínáme s Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="83c5e-111">For more information on Azure CLI 2.0, see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) and [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span></span>
