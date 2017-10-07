---
title: "aaaGet spuštění pomocí prostředí PowerShell pro Azure Batch | Microsoft Docs"
description: "Rychlý úvod toohello toomanage prostředky Batch můžete použít rutiny prostředí Azure PowerShell."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: f9ad62c5-27bf-4e6b-a5bf-c5f5914e6199
ms.service: batch
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: powershell
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e4d12e9c1e52a5b2db2dd44346edda93b7ef92b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-batch-resources-with-powershell-cmdlets"></a>Správa prostředků služby Batch pomocí rutin PowerShellu

S hello rutiny prostředí PowerShell Azure Batch, můžete provést a skriptu řadu hello stejné úlohy, které se provádějí pomocí rozhraní API služby Batch, hello hello portál Azure a hello rozhraní příkazového řádku Azure (CLI). Toto je toohello rutiny rychlý úvod, můžete použít toomanage účty Batch a pracovat s prostředky služby Batch, například fondy, úlohy a úkoly.

Úplný seznam rutin prostředí Batch a podrobný popis syntaxe rutin najdete v části hello [rutiny Azure Batch – reference](/powershell/module/azurerm.batch/#batch).

Tento článek vychází z rutin prostředí Azure PowerShell verze 3.0.0. Doporučujeme aktualizovat prostředí Azure PowerShell často tootake výhod aktualizace a vylepšení služby.

## <a name="prerequisites"></a>Požadavky
Proveďte následující operace toouse prostředí Azure PowerShell toomanage hello prostředky služby Batch.

* [Nainstalujte a nakonfigurujte Azure PowerShell.](/powershell/azure/overview)
* Spustit hello **Login-AzureRmAccount** rutiny tooconnect tooyour předplatné (hello Azure Batch lodě rutiny v modulu Azure Resource Manager hello):
  
    `Login-AzureRmAccount`
* **Zaregistrovat se obor názvů zprostředkovatele Batch hello**. Tuto operaci, stačí provést toobe **jednou za předplatné**.
  
    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Správa účtů a klíčů služby Batch
### <a name="create-a-batch-account"></a>Vytvoření účtu Batch
Rutina **New-AzureRmBatchAccount** vytvoří v zadané skupině prostředků účet služby Batch. Pokud ještě nemáte skupinu prostředků, vytvořte ji spuštěním hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) rutiny. Zadejte jednu z hello Azure oblasti v hello **umístění** parametru, jako je například "Střed USA". Například:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Pak vytvořte dávkový účet ve skupině prostředků hello, zadejte název pro účet hello v <*account_name*> a hello umístění a název vaší skupiny prostředků. Vytvoření účtu Batch hello může trvat některé toocomplete čas. Například:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [!NOTE]
> účet Batch Hello název musí být jedinečný toohello oblast Azure pro skupinu prostředků hello, obsahovat 3 až 24 znaků a používat jenom malá písmena a číslice.
> 
> 

### <a name="get-account-access-keys"></a>Získání přístupových klíčů k účtu
**Get-AzureRmBatchAccountKeys** ukazuje hello přístupové klíče asociované s účtem Azure Batch. Například spusťte hello následující tooget hello primární a sekundární klíče hello účtu, který jste vytvořili.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Vygenerování nového přístupového klíče
**New-AzureRmBatchAccountKey** vygeneruje nový primární nebo sekundární přístupový klíč pro účet Azure Batch. Například toogenerate nový primární klíč pro dávkový účet, zadejte:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [!NOTE]
> toogenerate nový sekundární klíč, zadejte "Sekundární" hello **KeyType** parametr. Máte tooregenerate hello primární a sekundární klíče samostatně.
> 
> 

### <a name="delete-a-batch-account"></a>Odstranění účtu Batch
**Remove-AzureRmBatchAccount** odstraní účet Batch. Například:

    Remove-AzureRmBatchAccount -AccountName <account_name>

Po zobrazení výzvy potvrďte, že chcete účet tooremove hello. Odebrání účtu může trvat některé toocomplete čas.

## <a name="create-a-batchaccountcontext-object"></a>Vytvoření objektu BatchAccountContext
rutiny prostředí PowerShell Batch pomocí tooauthenticate hello, při vytváření a správě Batch fondy, úlohy a úlohy, a další prostředky, nejprve vytvořit toostore objekt BatchAccountContext, název účtu a klíče:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Předání objektu BatchAccountContext hello do rutin tohoto použití hello **BatchContext** parametr.

> [!NOTE]
> Ve výchozím nastavení, primární klíč účtu hello slouží k ověřování, ale můžete explicitně vybrat klíče toouse hello změnou objekt BatchAccountContext **KeyInUse** vlastnost: `$context.KeyInUse = "Secondary"`.
> 
> 

## <a name="create-and-modify-batch-resources"></a>Vytváření a úpravy prostředků služby Batch
Pomocí rutin, jako například **New-AzureBatchPool**, **New-AzureBatchJob**, a **New-AzureBatchTask** toocreate prostředky v účtu Batch. Existují odpovídající **Get -** a **Set -** rutiny tooupdate hello vlastnosti existujících prostředků a **Remove -** rutiny tooremove prostředky v účtu Batch.

Při použití řadu tyto rutiny v přidání toopassing objekt BatchContext, třeba toocreate nebo předejte objekty, které obsahují nastavení podrobné prostředků, jak ukazuje následující příklad hello. V tématu hello podrobnou nápovědu pro všechny rutiny pro další příklady.

### <a name="create-a-batch-pool"></a>Vytvoření fondu služby Batch
Při vytváření nebo aktualizaci fondu služby Batch, vyberete konfigurace hello cloudové služby nebo konfigurace virtuálního počítače hello pro hello operační systém na hello výpočetních uzlů (viz [přehled funkcí Batch](batch-api-basics.md#pool)). Pokud zadáte konfigurace hello cloudové služby, výpočetní uzly se vytvoří jeho bitová kopie s jedním z hello [uvolní Azure hostovaného operačního systému](../cloud-services/cloud-services-guestos-update-matrix.md#releases). Pokud zadáte hello konfigurace virtuálního počítače, můžete zadat jednu z hello nepodporuje Linux nebo Image virtuálního počítače s Windows uvedených v hello [Marketplace virtuálních počítačů Azure][vm_marketplace], nebo zadejte vlastní obrázek, který jste připravili.

Při spuštění **New-AzureBatchPool**, předat objekt PSCloudServiceConfiguration nebo PSVirtualMachineConfiguration hello nastavení operačního systému. Například hello následující rutina vytvoří nový fond služby Batch s velikost malých výpočetní uzly v konfiguraci služby hello cloudu s obrázky hello nejnovější verzi operačního systému rodiny 3 (Windows Server 2012). Zde hello **CloudServiceConfiguration** parametr určuje hello *$configuration* proměnné jako objekt PSCloudServiceConfiguration hello. Hello **BatchContext** parametr určuje dříve definovanou proměnnou *$context* jako objekt BatchAccountContext hello.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Hello cílovým počtem výpočetních uzlů ve fondu nové hello je určen vzorcem pro automatické škálování. V takovém případě se jednoduše hello vzorec **$TargetDedicated = 4**, určující hello počet výpočetních uzlů ve fondu hello je maximálně 4.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Dotazy na fondy, úlohy, úkoly a další podrobnosti
Pomocí rutin, jako například **Get-AzureBatchPool**, **Get-AzureBatchJob**, a **Get-AzureBatchTask** tooquery entity vytvořené v účtu Batch.

### <a name="query-for-data"></a>Dotazy na data
Jako příklad použijte **Get-AzureBatchPools** toofind fondech. Ve výchozím nastavení dotazuje na všechny fondy v účtu, za předpokladu, že jste již uložený objekt BatchAccountContext hello *$context*:

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>Použití filtru OData
Můžete zadat filtru OData pomocí hello **filtru** parametr toofind hello pouze objekty, které vás zajímají. Například můžete vyhledat všechny fondy s ID začínajícími řetězcem myPool.

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

Tato metoda není tak účinná jako použití klauzule Where-Object v místním kanálu. Ale hello dotazu se odešlou toohello služby Batch přímo, aby veškeré filtrování provede na straně serveru hello, ukládání šířky pásma Internetu.

### <a name="use-hello-id-parameter"></a>Pomocí parametru Id hello
Alternativní tooan filtru OData je toouse hello **Id** parametr. tooquery na konkrétní fond s id "myPool":

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

Hello **Id** podporuje pouze vyhledávání úplných id, nepodporuje zástupné znaky nebo filtry stylu typu OData.

### <a name="use-hello-maxcount-parameter"></a>Použití parametru MaxCount hello
Ve výchozím nastavení každá rutina vrací maximálně 1 000 objektů. Pokud tento limit překročíte, buď upřesněte vaše filtru toobring vracel méně objektů, nebo explicitně nastavit maximální pomocí hello **MaxCount** parametr. Například:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

nastavit tooremove hello horní mez, **MaxCount** too0 nebo méně.

### <a name="use-hello-powershell-pipeline"></a>Použití kanálu hello prostředí PowerShell
Rutiny služby batch mohou využívat hello prostředí PowerShell kanálu toosend dat mezi rutinami. Tato akce nemá hello stejné ovlivňuje jako zadání parametru, ale díky práci s více entit.

Když chcete například najít a zobrazit všechny úlohy ve svém účtu:

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Restartování všech výpočetních uzlů ve fondu:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Správa balíčků aplikací
Balíčky aplikací poskytují jednodušší způsob toodeploy aplikace toohello výpočetních uzlů ve fondech. S hello rutin Powershellu ve službě Batch můžete odeslat a spravovat balíčky aplikací v účtu Batch a nasadit balíček verze toocompute uzlů.

**Vytvoření** aplikace:

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

**Přidání** balíčku aplikace:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

Sada hello **výchozí verze** aplikace hello:

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

**Výčet** balíčků aplikací:

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

**Odstranění** balíčku aplikace:

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

**Odstranění** aplikace:

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

> [!NOTE]
> Před odstraněním hello aplikace je nutné odstranit všechny aplikace verze balíčku aplikace. Zobrazí se chybové 'konfliktu, a pokud se pokusíte toodelete aplikace, která má v současné době balíčky aplikací.
> 
> 

### <a name="deploy-an-application-package"></a>Nasazení balíčku aplikace
Při vytváření fondu můžete zadat jeden nebo více balíčků aplikací, které budete nasazovat. Když zadáte balíček v okamžiku vytvoření fondu, je nasazené tooeach uzlu jako hello uzlu spojení fondu. Balíčky se také nasazují při restartování uzlu nebo jeho obnovení z image.

Zadejte hello `-ApplicationPackageReference` možnost při vytváření fondu toodeploy balíček toohello fondu aplikací uzly připojí hello fondu. Nejprve vytvořte **PSApplicationPackageReference** objektu a nakonfigurujte ho s hello aplikace Id balíčku verze a chcete, aby toodeploy toohello fondu výpočetních uzlů:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Teď vytvořte fond hello a určit hello balíček referenční objekt jako hello argument toohello `ApplicationPackageReferences` možnost:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

Můžete najít další informace o balíčky aplikací v [nasazení uzly toocompute aplikací pomocí balíčků aplikací Batch](batch-application-packages.md).

> [!IMPORTANT]
> Je nutné [propojení účtu Azure Storage](#linked-storage-account-autostorage) tooyour dávkového účtu toouse balíčky aplikací.
> 
> 

### <a name="update-a-pools-application-packages"></a>Aktualizace balíčků aplikací fondu
aplikace hello tooupdate přidělené tooan existujícího fondu, nejprve vytvořit objekt PSApplicationPackageReference s vlastnostmi hello požadovaného (Id a balíček verze aplikace):

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

V dalším kroku sám hello fondu Batch, vymažte všechny existující balíčky, přidat naše nový odkaz na balíček a aktualizaci služby Batch hello se nové nastavení fondu hello:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

Jste nyní aktualizovat vlastnosti hello fondu v hello služby Batch. tooactually nasadit hello nové aplikace balíčku toocompute uzly ve fondu hello, ale musí restartovat nebo obnovit z Image tyto uzly. K restartování všech uzlů ve fondu můžete použít tento příkaz:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

> [!TIP]
> Můžete nasadit víc aplikací balíčky toohello výpočetních uzlů ve fondu. Pokud chcete příliš*přidat* balíček aplikace místo nahrazení hello aktuálně nasazená balíčky, vynechejte hello `$pool.ApplicationPackageReferences.Clear()` řádku výše.
> 
> 

## <a name="next-steps"></a>Další kroky
* Podrobný popis syntaxe rutin najdete v článku [Rutiny služby Azure Batch – reference](/powershell/module/azurerm.batch/#batch).
* Další informace o aplikacích a balíčky aplikací ve službě Batch najdete v tématu [nasazení uzly toocompute aplikací pomocí balíčků aplikací Batch](batch-application-packages.md).

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/