<span data-ttu-id="c183f-101">Použití hello rozhraní příkazového řádku Azure tooget hello nasazení Vzdálená adresa URL pro vaše aplikace API.</span><span class="sxs-lookup"><span data-stu-id="c183f-101">Use hello Azure CLI tooget hello remote deployment URL for your API App.</span></span> <span data-ttu-id="c183f-102">Následující příkaz a nahraďte v hello  *\<app_name >* s názvem vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c183f-102">In hello following command, replace *\<app_name>* with your web app's name.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

<span data-ttu-id="c183f-103">Konfigurace vaší místní Git nasazení toobe možné toopush toohello vzdálené.</span><span class="sxs-lookup"><span data-stu-id="c183f-103">Configure your local Git deployment toobe able toopush toohello remote.</span></span>

```bash
git remote add azure <URI from previous step>
```

<span data-ttu-id="c183f-104">Push toohello Azure vzdálené toodeploy vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c183f-104">Push toohello Azure remote toodeploy your app.</span></span> <span data-ttu-id="c183f-105">Zobrazí se výzva pro hello heslo, které jste vytvořili dříve při vytvoření uživatele nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="c183f-105">You are prompted for hello password you created earlier when you created hello deployment user.</span></span> <span data-ttu-id="c183f-106">Ujistěte se, abyste zadali heslo hello, kterou jste vytvořili v dříve v hello rychlý start a není hello heslo použít toolog v toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c183f-106">Make sure that you enter hello password you created in earlier in hello quickstart, and not hello password you use toolog in toohello Azure portal.</span></span>

```bash
git push azure master
```
