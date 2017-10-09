instance služby Azure Redis Cache tooan tooconnect, klienti mezipaměti musí hello název hostitele, porty a klíče mezipaměti hello. Někteří klienti mohou odkazovat toothese položky mírně odlišné názvy. Tyto informace v hello portál Azure nebo pomocí nástroje příkazového řádku, jako je například rozhraní příkazového řádku Azure můžete načíst.

### <a name="retrieve-host-name-ports-and-access-keys-using-hello-azure-portal"></a>Načíst název hostitele, porty a přístupové klíče pomocí hello portálu Azure
tooretrieve hostitelem název, porty a přístupové klíče pomocí portálu Azure, hello [Procházet](../articles/redis-cache/cache-configure.md#configure-redis-cache-settings) tooyour mezipaměti hello [portál Azure](https://portal.azure.com) a klikněte na tlačítko **přístupové klíče** a  **Vlastnosti** v hello **prostředků nabídky**. 

![Nastavení mezipaměti Redis](media/redis-cache-access-keys/redis-cache-hostname-ports-keys.png)

### <a name="retrieve-host-name-ports-and-access-keys-using-azure-cli"></a>Načtení názvu hostitele, portů a přístupových klíčů pomocí Azure CLI
název hostitele hello tooretrieve a porty pomocí Azure CLI 2.0 můžete volat [az redis zobrazit](https://docs.microsoft.com/cli/azure/redis#show)a klíče hello tooretrieve můžete volat [az redis seznamu klíčů](https://docs.microsoft.com/cli/azure/redis#list-keys). Hello následující skript volá tyto dva příkazy a toto využití hello název hostitele, porty a klíče toohello konzoly.

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

Další informace o tento skript najdete v tématu [získat název hostitele hello, porty a klíče pro Azure Redis Cache](../articles/redis-cache/scripts/cache-keys-ports.md). Další informace o Azure CLI 2.0 najdete v tématech [Instalace Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) a [Začínáme s Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).
