<span data-ttu-id="59841-101">Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="59841-101">Log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span> <span data-ttu-id="59841-102">Další informace o přihlašování najdete v tématu [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="59841-102">For more information about logging in, see [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>

```azurecli
az login
```

<span data-ttu-id="59841-103">Pokud máte více než jedno předplatné Azure, vypište předplatná pro daný účet.</span><span class="sxs-lookup"><span data-stu-id="59841-103">If you have more than one Azure subscription, list the subscriptions for the account.</span></span>

```azurecli
az account list --all
```

<span data-ttu-id="59841-104">Určete předplatné, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="59841-104">Specify the subscription that you want to use.</span></span>

```azurecli
az account set --subscription <replace_with_your_subscription_id>
```