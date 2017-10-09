Použití hello rozhraní příkazového řádku Azure tooget hello nasazení Vzdálená adresa URL pro vaše aplikace API. Následující příkaz a nahraďte v hello  *\<app_name >* s názvem vaší webové aplikace.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Konfigurace vaší místní Git nasazení toobe možné toopush toohello vzdálené.

```bash
git remote add azure <URI from previous step>
```

Push toohello Azure vzdálené toodeploy vaší aplikace. Zobrazí se výzva pro hello heslo, které jste vytvořili dříve při vytvoření uživatele nasazení hello. Ujistěte se, abyste zadali heslo hello, kterou jste vytvořili v dříve v hello rychlý start a není hello heslo použít toolog v toohello portálu Azure.

```bash
git push azure master
```
