Verze 3.0 hello AzureRm.Resources modulu součástí práce se značky významné změny. Než budete pokračovat, zkontrolujte svoji verzi:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Pokud výsledky zobrazit verze 3.0 nebo novější, hello příklady v tomto tématu fungovat s vaším prostředím. Pokud nemáte verzi 3.0 nebo novější, než budete v tomto tématu pokračovat, [aktualizujte verzi](/powershell/azureps-cmdlets-docs/) pomocí Galerie prostředí PowerShell nebo Instalace webové platformy.

```powershell
Version
-------
3.5.0
```

toosee hello stávající značky pro *skupiny prostředků*, použijte:

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

Tento skript vrátí hello následující formát:

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

toosee hello stávající značky pro *prostředek, který má zadaný prostředek ID*, použijte:

```powershell
(Get-AzureRmResource -ResourceId {resource-id}).Tags
```

Nebo toosee hello stávající značky pro *prostředek, který má zadaný název a prostředků skupinu*, použijte:

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

tooget *skupin prostředků, které mají s konkrétní značkou tag*, použijte:

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

tooget *prostředky, které mají s konkrétní značkou tag*, použijte:

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```

Pokaždé, když použijete značky tooa prostředek nebo skupina zdrojů, můžete přepsat hello existující značek pro daný prostředek nebo skupina prostředků. Proto je nutné použít jiný přístup na základě toho, jestli hello prostředek nebo skupina prostředků má stávající značky. 

tooadd značky tooa *skupiny prostředků bez existující značky*, použijte:

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

tooadd značky tooa *skupinu prostředků, který má existující značky*, načtení hello existující značky, přidejte novou značku hello a znovu zásadu použijte hello značky:

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

tooadd značky tooa *prostředku bez existující značky*, použijte:

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName examplevnet -ResourceGroupName examplegroup
```

tooadd značky tooa *prostředek, který má existující značky*, použijte:

```powershell
$tags = (Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName examplevnet -ResourceGroupName examplegroup
```

tooapply všechny značky ze zdroje tooits skupiny prostředků, a *není zachována stávající značky na prostředcích hello*, použijte hello následující skript:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

tooapply všechny značky ze zdroje tooits skupiny prostředků, a *zachovat stávající značky na prostředcích, které nebudou duplikáty*, použijte hello následující skript:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    if ($g.Tags -ne $null) {
        $resources = Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName 
        foreach ($r in $resources)
        {
            $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
            foreach ($key in $g.Tags.Keys)
            {
                if ($resourcetags.ContainsKey($key)) { $resourcetags.Remove($key) }
            }
            $resourcetags += $g.Tags
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
    }
}
```

tooremove všechny značky, předejte prázdný zatřiďovací tabulku:

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```



