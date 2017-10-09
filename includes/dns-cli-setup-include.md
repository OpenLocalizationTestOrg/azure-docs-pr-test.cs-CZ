## <a name="set-up-azure-cli-for-azure-dns"></a>Nastavení rozhraní příkazového řádku Azure pro Azure DNS

### <a name="before-you-begin"></a>Než začnete

Ověřte, zda máte následující položky před zahájením konfigurace hello.

* Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).
* Nainstalujte nejnovější verzi hello hello Azure CLI, k dispozici pro Windows, Linux nebo MAC. Další informace najdete v [hello instalace rozhraní příkazového řádku Azure](../articles/cli-install-nodejs.md).

### <a name="sign-in-tooyour-azure-account"></a>Přihlaste se tooyour účet Azure

Otevřete okno konzoly a proveďte ověření pomocí svých přihlašovacích údajů. Další informace najdete v tématu [přihlásit tooAzure z hello rozhraní příkazového řádku Azure](../articles/xplat-cli-connect.md)

```azurecli
azure login
```

### <a name="switch-cli-mode"></a>Přepnutí režimu rozhraní příkazového řádku

Azure DNS používá Azure Resource Manager. Ujistěte se, že jste přešli příkazy Azure Resource Manager toouse režimu rozhraní příkazového řádku.

```azurecli
azure config mode arm
```

### <a name="select-hello-subscription"></a>Vyberte předplatné hello

Zkontrolujte předplatná hello pro účet hello.

```azurecli
azure account list
```

Zvolte, které vaše toouse předplatných Azure.

```azurecli
azure account set "subscription name"
```

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. To slouží jako hello výchozí umístění pro prostředky v příslušné skupině prostředků. Ale vzhledem k tomu, že všechny prostředky DNS jsou globální, a ne místní, hello Volba umístění skupiny prostředků nemá žádný vliv na Azure DNS.

Pokud používáte některou ze stávajících skupin prostředků, můžete tento krok přeskočit.

```azurecli
azure group create -n myresourcegroup --location "West US"
```

### <a name="register-resource-provider"></a>Registrace poskytovatele prostředků

Hello služba Azure DNS spravuje poskytovatel prostředků Microsoft.Network hello. Vaše předplatné Azure musí být registrovaný toouse tohoto poskytovatele prostředků než budete moci použít Azure DNS. Jedná se o jednorázovou operaci u každého odběru.

```azurecli
azure provider register --namespace Microsoft.Network
```

