použít tooadd skupinu prostředků tooa značka **sadu azure skupin**. Pokud skupina prostředků hello nemá všechny existující značky, předejte hello značky.

```azurecli
azure group set -n tag-demo-group -t Dept=Finance
```

Značky jsou aktualizovány jako celek. Pokud chcete tooadd skupinu prostředků tooa značku, která má existující značky, předejte všechny značky hello. 

```azurecli
azure group set -n tag-demo-group -t Dept=Finance;Environment=Production;Project=Upgrade
```

Značky nejsou zdědí prostředky ve skupině prostředků. použít tooadd prostředek tooa značka **sadu prostředků azure**. Předejte hello číslo verze rozhraní API pro hello typ prostředku, který chcete přidat značku hello. Pokud potřebujete tooretrieve hello rozhraní API verze, použijte následující příkaz s hello poskytovatel prostředků pro typ text hello, že jsou nastavení hello:

```azurecli
azure provider show -n Microsoft.Storage --json
```

V hello výsledky vyhledejte požadovaný typ prostředku hello.

```azurecli
"resourceTypes": [
{
  "resourceType": "storageAccounts",
  ...
  "apiVersions": [
    "2016-01-01",
    "2015-06-15",
    "2015-05-01-preview"
  ]
}
...
```

Nyní, zadejte tuto verzi rozhraní API, název skupiny prostředků, prostředků název, typ prostředku a hodnota značky jako parametry.

```azurecli
azure resource set -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -t Dept=Finance -o 2016-01-01
```

Značky existují přímo na skupiny prostředků a prostředky. toosee hello existující značky, získat skupinu prostředků a její prostředky s **zobrazit skupiny azure**.

```azurecli
azure group show -n tag-demo-group --json
```

Která vrací metadata o hello skupinu prostředků, včetně tooit všechny značky, které jsou použity.

```azurecli
{
  "id": "/subscriptions/4705409c-9372-42f0-914c-64a504530837/resourceGroups/tag-demo-group",
  "name": "tag-demo-group",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "location": "southcentralus",
  "tags": {
    "Dept": "Finance",
    "Environment": "Production",
    "Project": "Upgrade"
  },
  ...
}
```

Zobrazit hello značky pro určitý prostředek pomocí **prostředků azure zobrazit**.

```azurecli
azure resource show -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -o 2016-01-01 --json
```

tooretrieve všechny prostředky hello s hodnotou značku, použijte:

```azurecli
azure resource list -t Dept=Finance --json
```

tooretrieve všechny skupiny prostředků hello s hodnota značky, použijte:

```azurecli
azure group list -t Dept=Finance
```

Můžete zobrazit stávající značky hello ve vašem předplatném s hello následující příkaz:

```azurecli
azure tag list
```
