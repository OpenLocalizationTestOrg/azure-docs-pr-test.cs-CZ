<span data-ttu-id="6f6d7-101">Verze 3.0 hello AzureRm.Resources modulu součástí práce se značky významné změny.</span><span class="sxs-lookup"><span data-stu-id="6f6d7-101">Version 3.0 of hello AzureRm.Resources module included significant changes in how you work with tags.</span></span> <span data-ttu-id="6f6d7-102">Než budete pokračovat, zkontrolujte svoji verzi:</span><span class="sxs-lookup"><span data-stu-id="6f6d7-102">Before you proceed, check your version:</span></span>

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

<span data-ttu-id="6f6d7-103">Pokud výsledky zobrazit verze 3.0 nebo novější, hello příklady v tomto tématu fungovat s vaším prostředím.</span><span class="sxs-lookup"><span data-stu-id="6f6d7-103">If your results show version 3.0 or later, hello examples in this topic work with your environment.</span></span> <span data-ttu-id="6f6d7-104">Pokud nemáte verzi 3.0 nebo novější, než budete v tomto tématu pokračovat, [aktualizujte verzi](/powershell/azureps-cmdlets-docs/) pomocí Galerie prostředí PowerShell nebo Instalace webové platformy.</span><span class="sxs-lookup"><span data-stu-id="6f6d7-104">If you do not have version 3.0 or later, [update your version](/powershell/azureps-cmdlets-docs/) by using PowerShell Gallery or Web Platform Installer before you proceed with this topic.</span></span>

```powershell
Version
-------
3.5.0
```

<span data-ttu-id="6f6d7-105">toosee hello stávající značky pro *skupiny prostředků*, použijte:</span><span class="sxs-lookup"><span data-stu-id="6f6d7-105">toosee hello existing tags for a *resource group*, use:</span></span>

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

<span data-ttu-id="6f6d7-106">Tento skript vrátí hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="6f6d7-106">That script returns hello following format:</span></span>

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

<span data-ttu-id="6f6d7-107">toosee hello stávající značky pro *prostředek, který má zadaný prostředek ID*, použijte:</span><span class="sxs-lookup"><span data-stu-id="6f6d7-107">toosee hello existing tags for a *resource that has a specified resource ID*, use:</span></span>

```powershell
(Get-AzureRmResource -ResourceId {resource-id}).Tags
```

<span data-ttu-id="6f6d7-108">Nebo toosee hello stávající značky pro *prostředek, který má zadaný název a prostředků skupinu*, použijte:</span><span class="sxs-lookup"><span data-stu-id="6f6d7-108">Or, toosee hello existing tags for a *resource that has a specified name and resource group*, use:</span></span>

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

<span data-ttu-id="6f6d7-109">tooget *skupin prostředků, které mají s konkrétní značkou tag*, použijte:</span><span class="sxs-lookup"><span data-stu-id="6f6d7-109">tooget *resource groups that have a specific tag*, use:</span></span>

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

<span data-ttu-id="6f6d7-110">tooget *prostředky, které mají s konkrétní značkou tag*, použijte:</span><span class="sxs-lookup"><span data-stu-id="6f6d7-110">tooget *resources that have a specific tag*, use:</span></span>

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```

<span data-ttu-id="6f6d7-111">Pokaždé, když použijete značky tooa prostředek nebo skupina zdrojů, můžete přepsat hello existující značek pro daný prostředek nebo skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="6f6d7-111">Every time you apply tags tooa resource or a resource group, you overwrite hello existing tags on that resource or resource group.</span></span> <span data-ttu-id="6f6d7-112">Proto je nutné použít jiný přístup na základě toho, jestli hello prostředek nebo skupina prostředků má stávající značky.</span><span class="sxs-lookup"><span data-stu-id="6f6d7-112">Therefore, you must use a different approach based on whether hello resource or resource group has existing tags.</span></span> 

<span data-ttu-id="6f6d7-113">tooadd značky tooa *skupiny prostředků bez existující značky*, použijte:</span><span class="sxs-lookup"><span data-stu-id="6f6d7-113">tooadd tags tooa *resource group without existing tags*, use:</span></span>

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

<span data-ttu-id="6f6d7-114">tooadd značky tooa *skupinu prostředků, který má existující značky*, načtení hello existující značky, přidejte novou značku hello a znovu zásadu použijte hello značky:</span><span class="sxs-lookup"><span data-stu-id="6f6d7-114">tooadd tags tooa *resource group that has existing tags*, retrieve hello existing tags, add hello new tag, and reapply hello tags:</span></span>

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

<span data-ttu-id="6f6d7-115">tooadd značky tooa *prostředku bez existující značky*, použijte:</span><span class="sxs-lookup"><span data-stu-id="6f6d7-115">tooadd tags tooa *resource without existing tags*, use:</span></span>

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName examplevnet -ResourceGroupName examplegroup
```

<span data-ttu-id="6f6d7-116">tooadd značky tooa *prostředek, který má existující značky*, použijte:</span><span class="sxs-lookup"><span data-stu-id="6f6d7-116">tooadd tags tooa *resource that has existing tags*, use:</span></span>

```powershell
$tags = (Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName examplevnet -ResourceGroupName examplegroup
```

<span data-ttu-id="6f6d7-117">tooapply všechny značky ze zdroje tooits skupiny prostředků, a *není zachována stávající značky na prostředcích hello*, použijte hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="6f6d7-117">tooapply all tags from a resource group tooits resources, and *not retain existing tags on hello resources*, use hello following script:</span></span>

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

<span data-ttu-id="6f6d7-118">tooapply všechny značky ze zdroje tooits skupiny prostředků, a *zachovat stávající značky na prostředcích, které nebudou duplikáty*, použijte hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="6f6d7-118">tooapply all tags from a resource group tooits resources, and *retain existing tags on resources that are not duplicates*, use hello following script:</span></span>

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

<span data-ttu-id="6f6d7-119">tooremove všechny značky, předejte prázdný zatřiďovací tabulku:</span><span class="sxs-lookup"><span data-stu-id="6f6d7-119">tooremove all tags, pass an empty hash table:</span></span>

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```



