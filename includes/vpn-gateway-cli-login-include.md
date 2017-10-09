<span data-ttu-id="7b4de-101">Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="7b4de-101">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span> <span data-ttu-id="7b4de-102">Další informace o přihlašování najdete v tématu [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7b4de-102">For more information about logging in, see [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>

```azurecli
az login
```

<span data-ttu-id="7b4de-103">Pokud máte více než jedno předplatné, zobrazí seznam hello předplatná pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="7b4de-103">If you have more than one Azure subscription, list hello subscriptions for hello account.</span></span>

```azurecli
az account list --all
```

<span data-ttu-id="7b4de-104">Zadejte hello předplatné, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="7b4de-104">Specify hello subscription that you want toouse.</span></span>

```azurecli
az account set --subscription <replace_with_your_subscription_id>
```