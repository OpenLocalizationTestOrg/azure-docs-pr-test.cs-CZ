---
title: "aaaAzure protokolování v Key Vault | Microsoft Docs"
description: "Použijte tento kurz toohelp Začínáme s Azure Key Vault protokolování."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 43f96a2b-3af8-4adc-9344-bc6041fface8
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: cabailey
ms.openlocfilehash: 38a173297948748bef45e3d857c06b50b3e21e74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-logging"></a><span data-ttu-id="8805a-103">Protokolování v Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8805a-103">Azure Key Vault Logging</span></span>
<span data-ttu-id="8805a-104">Azure Key Vault je dostupný ve většině oblastí.</span><span class="sxs-lookup"><span data-stu-id="8805a-104">Azure Key Vault is available in most regions.</span></span> <span data-ttu-id="8805a-105">Další informace najdete v tématu hello [stránce s cenami Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="8805a-105">For more information, see hello [Key Vault pricing page](https://azure.microsoft.com/pricing/details/key-vault/).</span></span>

## <a name="introduction"></a><span data-ttu-id="8805a-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="8805a-106">Introduction</span></span>
<span data-ttu-id="8805a-107">Po vytvoření jednoho nebo více trezorů klíčů, budete pravděpodobně chtít toomonitor jak a kdy váš klíč trezory jsou přístupu a kým.</span><span class="sxs-lookup"><span data-stu-id="8805a-107">After you have created one or more key vaults, you will likely want toomonitor how and when your key vaults are accessed, and by whom.</span></span> <span data-ttu-id="8805a-108">Toho docílíte povolením protokolování pro Key Vault, které ukládá informace v zadaném účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="8805a-108">You can do this by enabling logging for Key Vault, which saves information in an Azure storage account that you provide.</span></span> <span data-ttu-id="8805a-109">Nový kontejner s názvem **insights-logs-auditevent** je automaticky vytvořený pro zadaný účet úložiště a ten samý účet úložiště můžete použít pro shromažďování protokolů více trezorů klíčů.</span><span class="sxs-lookup"><span data-stu-id="8805a-109">A new container named **insights-logs-auditevent** is automatically created for your specified storage account, and you can use this same storage account for collecting logs for multiple key vaults.</span></span>

<span data-ttu-id="8805a-110">Informace o protokolování můžete přístup maximálně, operace 10 minut po hello klíč trezoru.</span><span class="sxs-lookup"><span data-stu-id="8805a-110">You can access your logging information at most, 10 minutes after hello key vault operation.</span></span> <span data-ttu-id="8805a-111">Ve většině případů to bude rychlejší.</span><span class="sxs-lookup"><span data-stu-id="8805a-111">In most cases, it will be quicker than this.</span></span>  <span data-ttu-id="8805a-112">Je to tooyou toomanage protokolů v účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="8805a-112">It's up tooyou toomanage your logs in your storage account:</span></span>

* <span data-ttu-id="8805a-113">Pomocí protokolů toosecure metody řízení přístupu standardní Azure tak, že omezíte, kdo má přístup.</span><span class="sxs-lookup"><span data-stu-id="8805a-113">Use standard Azure access control methods toosecure your logs by restricting who can access them.</span></span>
* <span data-ttu-id="8805a-114">Odstranění protokolů, které již nechcete tookeep ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="8805a-114">Delete logs that you no longer want tookeep in your storage account.</span></span>

<span data-ttu-id="8805a-115">Použijte tento kurz toohelp Začínáme s Azure Key Vault protokolování, toocreate účtu úložiště povolit protokolování a interpretací hello shromážděných informací.</span><span class="sxs-lookup"><span data-stu-id="8805a-115">Use this tutorial toohelp you get started with Azure Key Vault logging, toocreate your storage account, enable logging, and interpret hello logging information collected.</span></span>  

> [!NOTE]
> <span data-ttu-id="8805a-116">Tento kurz neobsahuje pokyny, jak toocreate klíče trezory, klíčů nebo tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="8805a-116">This tutorial does not include instructions for how toocreate key vaults, keys, or secrets.</span></span> <span data-ttu-id="8805a-117">Další informace naleznete v tématu [Začínáme s Azure Key Vault](key-vault-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="8805a-117">For this information, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span> <span data-ttu-id="8805a-118">Pokyny pro rozhraní příkazového řádku pro různé platformy naleznete v [tomto ekvivalentním kurzu](key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="8805a-118">Or, for Cross-Platform Command-Line Interface instructions, see [this equivalent tutorial](key-vault-manage-with-cli2.md).</span></span>
>
> <span data-ttu-id="8805a-119">V současné době nelze Azure Key Vault konfigurovat v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8805a-119">Currently, you cannot configure Azure Key Vault in hello Azure portal.</span></span> <span data-ttu-id="8805a-120">Místo toho použijte tyto pokyny pro Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8805a-120">Instead, use these Azure PowerShell instructions.</span></span>
>
>

<span data-ttu-id="8805a-121">Souhrnné informace o Azure Key Vault naleznete v tématu [Co je Azure Key Vault?](key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8805a-121">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8805a-122">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8805a-122">Prerequisites</span></span>
<span data-ttu-id="8805a-123">toocomplete tento kurz, musíte mít hello následující:</span><span class="sxs-lookup"><span data-stu-id="8805a-123">toocomplete this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="8805a-124">Existující trezor klíčů, který již používáte.</span><span class="sxs-lookup"><span data-stu-id="8805a-124">An existing key vault that you have been using.</span></span>  
* <span data-ttu-id="8805a-125">Azure PowerShell v **minimální verzi 1.0.1**.</span><span class="sxs-lookup"><span data-stu-id="8805a-125">Azure PowerShell, **minimum version of 1.0.1**.</span></span> <span data-ttu-id="8805a-126">tooinstall prostředí Azure PowerShell a přidružit ho ke svému předplatnému Azure, najdete v části [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8805a-126">tooinstall Azure PowerShell and associate it with your Azure subscription, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="8805a-127">Pokud jste již Azure PowerShell nainstalovali a neznáte hello verzi, z konzoly Azure PowerShell text hello, zadejte `(Get-Module azure -ListAvailable).Version`.</span><span class="sxs-lookup"><span data-stu-id="8805a-127">If you have already installed Azure PowerShell and do not know hello version, from hello Azure PowerShell console, type `(Get-Module azure -ListAvailable).Version`.</span></span>  
* <span data-ttu-id="8805a-128">Dostatečné úložiště v Azure pro vaše protokoly Key Vault.</span><span class="sxs-lookup"><span data-stu-id="8805a-128">Sufficient storage on Azure for your Key Vault logs.</span></span>

## <span data-ttu-id="8805a-129"><a id="connect"></a>Připojit tooyour odběrů</span><span class="sxs-lookup"><span data-stu-id="8805a-129"><a id="connect"></a>Connect tooyour subscriptions</span></span>
<span data-ttu-id="8805a-130">Spusťte relaci prostředí Azure PowerShell a přihlaste tooyour účet Azure s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8805a-130">Start an Azure PowerShell session and sign in tooyour Azure account with hello following command:</span></span>  

    Login-AzureRmAccount

<span data-ttu-id="8805a-131">V okně hello automaticky otevírané okno prohlížeče zadejte účet Azure uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="8805a-131">In hello pop-up browser window, enter your Azure account user name and password.</span></span> <span data-ttu-id="8805a-132">Azure PowerShell získá všechna předplatná hello, které jsou spojeny s tímto účtem a ve výchozím nastavení, používá hello první z nich.</span><span class="sxs-lookup"><span data-stu-id="8805a-132">Azure PowerShell will get all hello subscriptions that are associated with this account and by default, uses hello first one.</span></span>

<span data-ttu-id="8805a-133">Pokud máte více předplatných, může být toospecify předplatné, které bylo použité toocreate Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="8805a-133">If you have multiple subscriptions, you might have toospecify a specific one that was used toocreate your Azure Key Vault.</span></span> <span data-ttu-id="8805a-134">Zadejte hello následující toosee hello předplatných pro váš účet:</span><span class="sxs-lookup"><span data-stu-id="8805a-134">Type hello following toosee hello subscriptions for your account:</span></span>

    Get-AzureRmSubscription

<span data-ttu-id="8805a-135">Potom toospecify hello odběr, který je spojen s trezoru klíčů, který budete protokolovat, zadejte:</span><span class="sxs-lookup"><span data-stu-id="8805a-135">Then, toospecify hello subscription that's associated with your key vault you will be logging, type:</span></span>

    Set-AzureRmContext -SubscriptionId <subscription ID>

> [!NOTE]
> <span data-ttu-id="8805a-136">Jedná se důležitý a obzvláště užitečný krok, pokud máte víc předplatných přidružených vašemu účtu.</span><span class="sxs-lookup"><span data-stu-id="8805a-136">This is an important step and especially helpful if you have multiple subscriptions associated with your account.</span></span> <span data-ttu-id="8805a-137">Pokud tento krok se přeskočí, může se zobrazit k chybě tooregister Microsoft.Insights.</span><span class="sxs-lookup"><span data-stu-id="8805a-137">You may receive an error tooregister Microsoft.Insights if this step is skipped.</span></span>
>   
>

<span data-ttu-id="8805a-138">Další informace o konfiguraci Azure Powershellu najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8805a-138">For more information about configuring Azure PowerShell, see  [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <span data-ttu-id="8805a-139"><a id="storage"></a>Vytvoření nového účtu úložiště pro protokoly</span><span class="sxs-lookup"><span data-stu-id="8805a-139"><a id="storage"></a>Create a new storage account for your logs</span></span>
<span data-ttu-id="8805a-140">Přestože je možné použít existující účet úložiště pro svoje protokoly, vytvoříme nový účet úložiště, který bude vyhrazený tooKey protokoly trezoru.</span><span class="sxs-lookup"><span data-stu-id="8805a-140">Although you can use an existing storage account for your logs, we'll create a new storage account that will be dedicated tooKey Vault logs.</span></span> <span data-ttu-id="8805a-141">Abychom si usnadnili práci při máme toospecify, to později, uložíme hello podrobnosti do proměnné s názvem **sa**.</span><span class="sxs-lookup"><span data-stu-id="8805a-141">For convenience for when we have toospecify this later, we'll store hello details into a variable named **sa**.</span></span>

<span data-ttu-id="8805a-142">Pro další usnadnění správy použijeme také hello stejné skupině prostředků jako ten, který obsahuje náš trezor klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="8805a-142">For additional ease of management, we'll also use hello same resource group as hello one that contains our key vault.</span></span> <span data-ttu-id="8805a-143">Z hello [kurz Začínáme](key-vault-get-started.md), tato skupina prostředků je s názvem **ContosoResourceGroup** a budeme pokračovat v umístění východní Asie toouse hello.</span><span class="sxs-lookup"><span data-stu-id="8805a-143">From hello [getting started tutorial](key-vault-get-started.md), this resource group is named **ContosoResourceGroup** and we'll continue toouse hello East Asia location.</span></span> <span data-ttu-id="8805a-144">Tyto hodnoty nahraďte příslušnými vlastními hodnotami:</span><span class="sxs-lookup"><span data-stu-id="8805a-144">Substitute these values for your own, as applicable:</span></span>

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name contosokeyvaultlogs -Type Standard_LRS -Location 'East Asia'


> [!NOTE]
> <span data-ttu-id="8805a-145">Pokud se rozhodnete toouse stávající účet úložiště, musíte použít hello stejné předplatné jako váš trezor klíčů a musí používat model nasazení Resource Manager hello, nikoli hello model nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="8805a-145">If you decide toouse an existing storage account, it must use hello same subscription as your key vault and it must use hello Resource Manager deployment model, rather than hello Classic deployment model.</span></span>
>
>

## <span data-ttu-id="8805a-146"><a id="identify"></a>Identifikujte trezor klíčů hello pro svoje protokoly</span><span class="sxs-lookup"><span data-stu-id="8805a-146"><a id="identify"></a>Identify hello key vault for your logs</span></span>
<span data-ttu-id="8805a-147">V našem kurzu Začínáme byl název trezoru klíčů **ContosoKeyVault**, budeme tedy pokračovat toouse, zadejte název a uložte hello podrobnosti do proměnné s názvem **kv**:</span><span class="sxs-lookup"><span data-stu-id="8805a-147">In our getting started tutorial, our key vault name was **ContosoKeyVault**, so we'll continue toouse that name and store hello details into a variable named **kv**:</span></span>

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <span data-ttu-id="8805a-148"><a id="enable"></a>Povolení protokolování</span><span class="sxs-lookup"><span data-stu-id="8805a-148"><a id="enable"></a>Enable logging</span></span>
<span data-ttu-id="8805a-149">tooenable protokolování pro Key Vault použijeme rutinu Set-AzureRmDiagnosticSetting hello, společně s hello proměnné jsme vytvořili pro naše nový účet úložiště a náš trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="8805a-149">tooenable logging for Key Vault, we'll use hello Set-AzureRmDiagnosticSetting cmdlet, together with hello variables we created for our new storage account and our key vault.</span></span> <span data-ttu-id="8805a-150">Také nastavíme hello **-povolit** příznak příliš**$true** a nastavte hello kategorie tooAuditEvent (hello jediná kategorie pro protokolování v Key Vault),:</span><span class="sxs-lookup"><span data-stu-id="8805a-150">We'll also set hello **-Enabled** flag too**$true** and set hello category tooAuditEvent (hello only category for Key Vault logging), :</span></span>

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

<span data-ttu-id="8805a-151">výstup Hello zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="8805a-151">hello output for this includes:</span></span>

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


<span data-ttu-id="8805a-152">To potvrdí, že je váš trezor klíčů, ukládá informace o účtu úložiště tooyour nyní povoleno protokolování.</span><span class="sxs-lookup"><span data-stu-id="8805a-152">This confirms that logging is now enabled for your key vault, saving information tooyour storage account.</span></span>

<span data-ttu-id="8805a-153">Volitelně můžete pro své protokoly nastavit také zásady uchovávání informací tak, aby se starší protokoly automaticky odstraňovaly.</span><span class="sxs-lookup"><span data-stu-id="8805a-153">Optionally you can also set retention policy for your logs such that older logs will be automatically deleted.</span></span> <span data-ttu-id="8805a-154">Například nastavit pomocí zásad uchovávání **- RetentionEnabled** příznak příliš**$true** a nastavte **- RetentionInDays** parametr příliš**90** , aby protokoly, které jsou starší než 90 dní automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="8805a-154">For example, set retention policy using **-RetentionEnabled** flag too**$true** and set **-RetentionInDays** parameter too**90** so that logs older than 90 days will be automatically deleted.</span></span>

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

<span data-ttu-id="8805a-155">Co je protokolováno:</span><span class="sxs-lookup"><span data-stu-id="8805a-155">What's logged:</span></span>

* <span data-ttu-id="8805a-156">Protokolují se všechny ověřené požadavky REST API, což zahrnuje i neúspěšné požadavky v důsledku neoprávněného přístupu, systémových chyb nebo chybných požadavků.</span><span class="sxs-lookup"><span data-stu-id="8805a-156">All authenticated REST API requests are logged, which includes failed requests as a result of access permissions, system errors or bad requests.</span></span>
* <span data-ttu-id="8805a-157">Operace na hello klíč trezoru sebe, což zahrnuje vytváření, odstraňování, nastavení zásad přístupu trezoru klíčů, a aktualizaci atributů trezoru klíčů, jako značky.</span><span class="sxs-lookup"><span data-stu-id="8805a-157">Operations on hello key vault itself, which includes creation, deletion, setting key vault access policies, and updating key vault attributes such as tags.</span></span>
* <span data-ttu-id="8805a-158">Operace u klíčů a tajných klíčů v trezoru klíčů hello, včetně vytváření, úpravy nebo odstranění těchto klíčů nebo tajných klíčů; operace jako jsou podepsání, ověření, šifrování, dešifrování, zabalení a rozbalení klíčů, získat tajné klíče, výpis klíčů a tajných klíčů a jejich verze.</span><span class="sxs-lookup"><span data-stu-id="8805a-158">Operations on keys and secrets in hello key vault, which includes creating, modifying, or deleting these keys or secrets; operations such as sign, verify, encrypt, decrypt, wrap and unwrap keys, get secrets, list keys and secrets and their versions.</span></span>
* <span data-ttu-id="8805a-159">Neověřené požadavky, které skončí odpovědí 401 – Neoprávněno.</span><span class="sxs-lookup"><span data-stu-id="8805a-159">Unauthenticated requests that result in a 401 response.</span></span> <span data-ttu-id="8805a-160">Například požadavky, které nemají nosný token, jsou poškozené nebo jejichž platnost vypršela, nebo mají neplatný token.</span><span class="sxs-lookup"><span data-stu-id="8805a-160">For example, requests that do not have a bearer token, or are malformed or expired, or have an invalid token.</span></span>  

## <span data-ttu-id="8805a-161"><a id="access"></a>Přístup k protokolům</span><span class="sxs-lookup"><span data-stu-id="8805a-161"><a id="access"></a>Access your logs</span></span>
<span data-ttu-id="8805a-162">Protokoly trezoru klíčů se ukládají do hello **insights-logs-auditevent** kontejneru v účtu úložiště hello jste zadali.</span><span class="sxs-lookup"><span data-stu-id="8805a-162">Key vault logs are stored in hello **insights-logs-auditevent** container in hello storage account you provided.</span></span> <span data-ttu-id="8805a-163">toolist všech objektů BLOB hello v tomto kontejneru, zadejte:</span><span class="sxs-lookup"><span data-stu-id="8805a-163">toolist all hello blobs in this container, type:</span></span>

<span data-ttu-id="8805a-164">Nejprve vytvořte proměnnou pro název kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="8805a-164">First, create a variable for hello container name.</span></span> <span data-ttu-id="8805a-165">Bude se používat v rámci hello zbytek hello ukázce.</span><span class="sxs-lookup"><span data-stu-id="8805a-165">This will be used throughout hello rest of hello walk through.</span></span>

    $container = 'insights-logs-auditevent'

<span data-ttu-id="8805a-166">toolist všech objektů BLOB hello v tomto kontejneru, zadejte:</span><span class="sxs-lookup"><span data-stu-id="8805a-166">toolist all hello blobs in this container, type:</span></span>

    Get-AzureStorageBlob -Container $container -Context $sa.Context
<span data-ttu-id="8805a-167">Hello výstup bude vypadat něco podobné toothis:</span><span class="sxs-lookup"><span data-stu-id="8805a-167">hello output will look something similar toothis:</span></span>

<span data-ttu-id="8805a-168">**Container Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**</span><span class="sxs-lookup"><span data-stu-id="8805a-168">**Container Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**</span></span>

<span data-ttu-id="8805a-169">**Název**</span><span class="sxs-lookup"><span data-stu-id="8805a-169">**Name**</span></span>

- - -
<span data-ttu-id="8805a-170">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**</span><span class="sxs-lookup"><span data-stu-id="8805a-170">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**</span></span>

<span data-ttu-id="8805a-171">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**</span><span class="sxs-lookup"><span data-stu-id="8805a-171">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**</span></span>

<span data-ttu-id="8805a-172">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json****</span><span class="sxs-lookup"><span data-stu-id="8805a-172">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json****</span></span>

<span data-ttu-id="8805a-173">Jak je vidět na výpisu, objekty BLOB hello podle zásady vytváření názvů: **resourceId =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/h =<hour>/m =<minute>/filename.json**</span><span class="sxs-lookup"><span data-stu-id="8805a-173">As you can see from this output, hello blobs follow a naming convention: **resourceId=<ARM resource ID>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json**</span></span>

<span data-ttu-id="8805a-174">Hello hodnoty data a času používají UTC.</span><span class="sxs-lookup"><span data-stu-id="8805a-174">hello date and time values use UTC.</span></span>

<span data-ttu-id="8805a-175">Vzhledem k hello stejný účet úložiště můžou být použité toocollect protokoly pro více zdrojů, hello úplné ID prostředku v hello název objektu blob je velmi užitečné tooaccess nebo objekty BLOB jenom hello stahování, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="8805a-175">Because hello same storage account can be used toocollect logs for multiple resources, hello full resource ID in hello blob name is very useful tooaccess or download just hello blobs that you need.</span></span> <span data-ttu-id="8805a-176">Ale předtím, než se podíváme na to, budete nejprve nabídneme jak toodownload všechny hello objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="8805a-176">But before we do that, we'll first cover how toodownload all hello blobs.</span></span>

<span data-ttu-id="8805a-177">Nejprve vytvořte složku toodownload hello objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="8805a-177">First, create a folder toodownload hello blobs.</span></span> <span data-ttu-id="8805a-178">Například:</span><span class="sxs-lookup"><span data-stu-id="8805a-178">For example:</span></span>

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

<span data-ttu-id="8805a-179">Poté získejte seznam všech objektů blob:</span><span class="sxs-lookup"><span data-stu-id="8805a-179">Then get a list of all blobs:</span></span>  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

<span data-ttu-id="8805a-180">Tento seznam prostřednictvím 'Get-AzureStorageBlobContent' toodownload hello objekty BLOB přesměrováním do našich cílovou složku:</span><span class="sxs-lookup"><span data-stu-id="8805a-180">Pipe this list through 'Get-AzureStorageBlobContent' toodownload hello blobs into our destination folder:</span></span>

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

<span data-ttu-id="8805a-181">Když spustíte tento druhý příkaz, hello  **/**  oddělovač v názvech objektů blob hello vytvořit úplnou strukturu složek v části hello cílovou složku a tato struktura bude použité toodownload úložiště hello objektů BLOB a jako soubory.</span><span class="sxs-lookup"><span data-stu-id="8805a-181">When you run this second command, hello **/** delimiter in hello blob names create a full folder structure under hello destination folder, and this structure will be used toodownload and store hello blobs as files.</span></span>

<span data-ttu-id="8805a-182">tooselectively stáhnout objekty BLOB, použijte zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="8805a-182">tooselectively download blobs, use wildcards.</span></span> <span data-ttu-id="8805a-183">Například:</span><span class="sxs-lookup"><span data-stu-id="8805a-183">For example:</span></span>

* <span data-ttu-id="8805a-184">Pokud máte více trezorů klíčů a chcete toodownload protokoly pro právě jeden trezor klíčů, s názvem CONTOSOKEYVAULT3:</span><span class="sxs-lookup"><span data-stu-id="8805a-184">If you have multiple key vaults and want toodownload logs for just one key vault, named CONTOSOKEYVAULT3:</span></span>

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
* <span data-ttu-id="8805a-185">Pokud máte více skupin prostředků a chcete toodownload protokoly pro jenom jedné skupiny prostředků, použijte `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:</span><span class="sxs-lookup"><span data-stu-id="8805a-185">If you have multiple resource groups and want toodownload logs for just one resource group, use `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:</span></span>

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
* <span data-ttu-id="8805a-186">V případě potřeby toodownload všechny hello protokoly pro měsíc leden 2016 hello pomocí `-Blob '*/year=2016/m=01/*'`:</span><span class="sxs-lookup"><span data-stu-id="8805a-186">If you want toodownload all hello logs for hello month of January 2016, use `-Blob '*/year=2016/m=01/*'`:</span></span>

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

<span data-ttu-id="8805a-187">Jste nyní připraven toostart prohlížení co je v hello protokoly.</span><span class="sxs-lookup"><span data-stu-id="8805a-187">You're now ready toostart looking at what's in hello logs.</span></span> <span data-ttu-id="8805a-188">Ale předtím, další dva parametry Get-AzureRmDiagnosticSetting, může být nutné tooknow:</span><span class="sxs-lookup"><span data-stu-id="8805a-188">But before moving onto that, two more parameters for Get-AzureRmDiagnosticSetting that you might need tooknow:</span></span>

* <span data-ttu-id="8805a-189">tooquery hello stav nastavení diagnostiky pro prostředek vašeho trezoru klíčů:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`</span><span class="sxs-lookup"><span data-stu-id="8805a-189">tooquery hello  status of diagnostic settings for your key vault resource: `Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`</span></span>
* <span data-ttu-id="8805a-190">toodisable protokolování pro prostředek vašeho trezoru klíčů:`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`</span><span class="sxs-lookup"><span data-stu-id="8805a-190">toodisable logging for your key vault resource: `Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`</span></span>

## <span data-ttu-id="8805a-191"><a id="interpret"></a>Interpretace protokolů služby Key Vault</span><span class="sxs-lookup"><span data-stu-id="8805a-191"><a id="interpret"></a>Interpret your Key Vault logs</span></span>
<span data-ttu-id="8805a-192">Jednotlivé objekty blob jsou uloženy jako text ve formátu JSON blob.</span><span class="sxs-lookup"><span data-stu-id="8805a-192">Individual blobs are stored as text, formatted as a JSON blob.</span></span> <span data-ttu-id="8805a-193">Toto je příklad položky protokolu ze spuštění příkazu `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:</span><span class="sxs-lookup"><span data-stu-id="8805a-193">This is an example log entry from running `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:</span></span>

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


<span data-ttu-id="8805a-194">Hello následující tabulka uvádí hello názvy polí a popisy.</span><span class="sxs-lookup"><span data-stu-id="8805a-194">hello following table lists hello field names and descriptions.</span></span>

| <span data-ttu-id="8805a-195">Název pole</span><span class="sxs-lookup"><span data-stu-id="8805a-195">Field name</span></span> | <span data-ttu-id="8805a-196">Popis</span><span class="sxs-lookup"><span data-stu-id="8805a-196">Description</span></span> |
| --- | --- |
| <span data-ttu-id="8805a-197">time</span><span class="sxs-lookup"><span data-stu-id="8805a-197">time</span></span> |<span data-ttu-id="8805a-198">Datum a čas (UTC).</span><span class="sxs-lookup"><span data-stu-id="8805a-198">Date and time (UTC).</span></span> |
| <span data-ttu-id="8805a-199">resourceId</span><span class="sxs-lookup"><span data-stu-id="8805a-199">resourceId</span></span> |<span data-ttu-id="8805a-200">ID prostředku Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="8805a-200">Azure Resource Manager Resource ID.</span></span> <span data-ttu-id="8805a-201">Pro protokoly Key Vault je to vždy ID prostředku Key Vault hello</span><span class="sxs-lookup"><span data-stu-id="8805a-201">For Key Vault logs, this is always hello  Key Vault resource ID.</span></span> |
| <span data-ttu-id="8805a-202">operationName</span><span class="sxs-lookup"><span data-stu-id="8805a-202">operationName</span></span> |<span data-ttu-id="8805a-203">Název hello operace, jak je uvedeno v další tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="8805a-203">Name of hello operation, as documented in hello next table.</span></span> |
| <span data-ttu-id="8805a-204">operationVersion</span><span class="sxs-lookup"><span data-stu-id="8805a-204">operationVersion</span></span> |<span data-ttu-id="8805a-205">Toto je verze hello REST API požadovaná klientem hello.</span><span class="sxs-lookup"><span data-stu-id="8805a-205">This is hello REST API version requested by hello client.</span></span> |
| <span data-ttu-id="8805a-206">category</span><span class="sxs-lookup"><span data-stu-id="8805a-206">category</span></span> |<span data-ttu-id="8805a-207">Pro protokoly Key Vault je AuditEvent hello jeden, k dispozici hodnota.</span><span class="sxs-lookup"><span data-stu-id="8805a-207">For Key Vault logs, AuditEvent is hello single, available value.</span></span> |
| <span data-ttu-id="8805a-208">resultType</span><span class="sxs-lookup"><span data-stu-id="8805a-208">resultType</span></span> |<span data-ttu-id="8805a-209">Výsledek požadavku REST API.</span><span class="sxs-lookup"><span data-stu-id="8805a-209">Result of REST API request.</span></span> |
| <span data-ttu-id="8805a-210">resultSignature</span><span class="sxs-lookup"><span data-stu-id="8805a-210">resultSignature</span></span> |<span data-ttu-id="8805a-211">Stav HTTP.</span><span class="sxs-lookup"><span data-stu-id="8805a-211">HTTP status.</span></span> |
| <span data-ttu-id="8805a-212">resultDescription</span><span class="sxs-lookup"><span data-stu-id="8805a-212">resultDescription</span></span> |<span data-ttu-id="8805a-213">Další popis výsledku hello, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="8805a-213">Additional description about hello result, when available.</span></span> |
| <span data-ttu-id="8805a-214">durationMs</span><span class="sxs-lookup"><span data-stu-id="8805a-214">durationMs</span></span> |<span data-ttu-id="8805a-215">Čas, kdy trvalo požadavku REST API hello tooservice, v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="8805a-215">Time it took tooservice hello REST API request, in milliseconds.</span></span> <span data-ttu-id="8805a-216">Nezahrnuje latenci sítě hello, takže čas hello, naměřený na straně klienta hello se může lišit.</span><span class="sxs-lookup"><span data-stu-id="8805a-216">This does not include hello network latency, so hello time you measure on hello client side might not match this time.</span></span> |
| <span data-ttu-id="8805a-217">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="8805a-217">callerIpAddress</span></span> |<span data-ttu-id="8805a-218">IP adresa klienta hello, který vytvořil požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="8805a-218">IP address of hello client who made hello request.</span></span> |
| <span data-ttu-id="8805a-219">correlationId</span><span class="sxs-lookup"><span data-stu-id="8805a-219">correlationId</span></span> |<span data-ttu-id="8805a-220">Volitelný GUID, který hello klienta můžete předat toocorrelate protokoly na straně klienta s protokoly na straně služby (Key Vault).</span><span class="sxs-lookup"><span data-stu-id="8805a-220">An optional GUID that hello client can pass toocorrelate client-side logs with service-side (Key Vault) logs.</span></span> |
| <span data-ttu-id="8805a-221">identity</span><span class="sxs-lookup"><span data-stu-id="8805a-221">identity</span></span> |<span data-ttu-id="8805a-222">Identita z tokenu hello, který byl předložený při provádění požadavku REST API hello.</span><span class="sxs-lookup"><span data-stu-id="8805a-222">Identity from hello token that was presented when making hello REST API request.</span></span> <span data-ttu-id="8805a-223">Obvykle to je položka „user“, „service principal“ nebo kombinace „user+appId“ jako v případě požadavku pocházejícího z rutiny Azure PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="8805a-223">This is usually a "user", a "service principal" or a combination "user+appId" as in case of a request resulting from a Azure PowerShell cmdlet.</span></span> |
| <span data-ttu-id="8805a-224">properties</span><span class="sxs-lookup"><span data-stu-id="8805a-224">properties</span></span> |<span data-ttu-id="8805a-225">Toto pole bude obsahovat různé informace v závislosti na hello operaci (operationName).</span><span class="sxs-lookup"><span data-stu-id="8805a-225">This field will contain different information based on hello operation (operationName).</span></span> <span data-ttu-id="8805a-226">Ve většině případů obsahuje informace o klientovi (hello řetězec useragent předaný klientem hello), hello přesný identifikátor URI požadavku REST API a stavový kód HTTP.</span><span class="sxs-lookup"><span data-stu-id="8805a-226">In most cases, contains client information (hello useragent string passed by hello client), hello exact REST API request URI, and HTTP status code.</span></span> <span data-ttu-id="8805a-227">Kromě toho pokud je jako výsledek požadavku (například KeyCreate nebo VaultGet) vrácen objekt bude také obsahovat hello URI klíče (jako "id"), identifikátor URI trezoru nebo identifikátor URI tajného klíče.</span><span class="sxs-lookup"><span data-stu-id="8805a-227">In addition, when an object is returned as a result of a request (for example, KeyCreate or VaultGet) it will also contain hello Key URI (as "id"), Vault URI, or Secret URI.</span></span> |

<span data-ttu-id="8805a-228">Hello **operationName** hodnoty polí jsou ve formátu ObjectVerb.</span><span class="sxs-lookup"><span data-stu-id="8805a-228">hello **operationName** field values are in ObjectVerb format.</span></span> <span data-ttu-id="8805a-229">Například:</span><span class="sxs-lookup"><span data-stu-id="8805a-229">For example:</span></span>

* <span data-ttu-id="8805a-230">Všechny operace trezoru klíčů mít hello ' trezoru`<action>`' formátu, například `VaultGet` a `VaultCreate`.</span><span class="sxs-lookup"><span data-stu-id="8805a-230">All key vault operations have hello 'Vault`<action>`' format, such as `VaultGet` and `VaultCreate`.</span></span>
* <span data-ttu-id="8805a-231">Všechny operace nad klíči jsou hello se klíč`<action>`' formátu, například `KeySign` a `KeyList`.</span><span class="sxs-lookup"><span data-stu-id="8805a-231">All  key operations have hello 'Key`<action>`' format, such as `KeySign` and `KeyList`.</span></span>
* <span data-ttu-id="8805a-232">Všechny operace nad tajnými klíči mít hello "tajný klíč`<action>`' formátu, například `SecretGet` a `SecretListVersions`.</span><span class="sxs-lookup"><span data-stu-id="8805a-232">All secret operations have hello 'Secret`<action>`' format, such as `SecretGet` and `SecretListVersions`.</span></span>

<span data-ttu-id="8805a-233">Hello následující tabulka uvádí hello operationName a odpovídajících příkazů REST API.</span><span class="sxs-lookup"><span data-stu-id="8805a-233">hello following table lists hello operationName and corresponding REST API command.</span></span>

| <span data-ttu-id="8805a-234">operationName</span><span class="sxs-lookup"><span data-stu-id="8805a-234">operationName</span></span> | <span data-ttu-id="8805a-235">Příkaz REST API</span><span class="sxs-lookup"><span data-stu-id="8805a-235">REST API Command</span></span> |
| --- | --- |
| <span data-ttu-id="8805a-236">Authentication</span><span class="sxs-lookup"><span data-stu-id="8805a-236">Authentication</span></span> |<span data-ttu-id="8805a-237">Přes koncový bod služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8805a-237">Via Azure Active Directory endpoint</span></span> |
| <span data-ttu-id="8805a-238">VaultGet</span><span class="sxs-lookup"><span data-stu-id="8805a-238">VaultGet</span></span> |[<span data-ttu-id="8805a-239">Získání informací o trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="8805a-239">Get information about a key vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx) |
| <span data-ttu-id="8805a-240">VaultPut</span><span class="sxs-lookup"><span data-stu-id="8805a-240">VaultPut</span></span> |[<span data-ttu-id="8805a-241">Vytvoření nebo aktualizace trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="8805a-241">Create or update a key vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx) |
| <span data-ttu-id="8805a-242">VaultDelete</span><span class="sxs-lookup"><span data-stu-id="8805a-242">VaultDelete</span></span> |[<span data-ttu-id="8805a-243">Odstranění trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="8805a-243">Delete a key vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx) |
| <span data-ttu-id="8805a-244">VaultPatch</span><span class="sxs-lookup"><span data-stu-id="8805a-244">VaultPatch</span></span> |[<span data-ttu-id="8805a-245">Aktualizace trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="8805a-245">Update a key vault</span></span>](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| <span data-ttu-id="8805a-246">VaultList</span><span class="sxs-lookup"><span data-stu-id="8805a-246">VaultList</span></span> |[<span data-ttu-id="8805a-247">Výpis všech trezorů klíčů ve skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="8805a-247">List all key vaults in a resource group</span></span>](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx) |
| <span data-ttu-id="8805a-248">KeyCreate</span><span class="sxs-lookup"><span data-stu-id="8805a-248">KeyCreate</span></span> |[<span data-ttu-id="8805a-249">Vytvoření klíče</span><span class="sxs-lookup"><span data-stu-id="8805a-249">Create a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx) |
| <span data-ttu-id="8805a-250">KeyGet</span><span class="sxs-lookup"><span data-stu-id="8805a-250">KeyGet</span></span> |[<span data-ttu-id="8805a-251">Získání informací o klíči</span><span class="sxs-lookup"><span data-stu-id="8805a-251">Get information about a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx) |
| <span data-ttu-id="8805a-252">KeyImport</span><span class="sxs-lookup"><span data-stu-id="8805a-252">KeyImport</span></span> |[<span data-ttu-id="8805a-253">Import klíče do trezoru</span><span class="sxs-lookup"><span data-stu-id="8805a-253">Import a key into a vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx) |
| <span data-ttu-id="8805a-254">KeyBackup</span><span class="sxs-lookup"><span data-stu-id="8805a-254">KeyBackup</span></span> |<span data-ttu-id="8805a-255">[Zálohování klíče](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).</span><span class="sxs-lookup"><span data-stu-id="8805a-255">[Backup a key](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).</span></span> |
| <span data-ttu-id="8805a-256">KeyDelete</span><span class="sxs-lookup"><span data-stu-id="8805a-256">KeyDelete</span></span> |[<span data-ttu-id="8805a-257">Odstranění klíče</span><span class="sxs-lookup"><span data-stu-id="8805a-257">Delete a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx) |
| <span data-ttu-id="8805a-258">KeyRestore</span><span class="sxs-lookup"><span data-stu-id="8805a-258">KeyRestore</span></span> |[<span data-ttu-id="8805a-259">Obnovení klíče</span><span class="sxs-lookup"><span data-stu-id="8805a-259">Restore a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx) |
| <span data-ttu-id="8805a-260">KeySign</span><span class="sxs-lookup"><span data-stu-id="8805a-260">KeySign</span></span> |[<span data-ttu-id="8805a-261">Podpis klíčem</span><span class="sxs-lookup"><span data-stu-id="8805a-261">Sign with a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx) |
| <span data-ttu-id="8805a-262">KeyVerify</span><span class="sxs-lookup"><span data-stu-id="8805a-262">KeyVerify</span></span> |[<span data-ttu-id="8805a-263">Ověření pomocí klíče</span><span class="sxs-lookup"><span data-stu-id="8805a-263">Verify with a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx) |
| <span data-ttu-id="8805a-264">KeyWrap</span><span class="sxs-lookup"><span data-stu-id="8805a-264">KeyWrap</span></span> |[<span data-ttu-id="8805a-265">Zabalení klíče</span><span class="sxs-lookup"><span data-stu-id="8805a-265">Wrap a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx) |
| <span data-ttu-id="8805a-266">KeyUnwrap</span><span class="sxs-lookup"><span data-stu-id="8805a-266">KeyUnwrap</span></span> |[<span data-ttu-id="8805a-267">Rozbalení klíče</span><span class="sxs-lookup"><span data-stu-id="8805a-267">Unwrap a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx) |
| <span data-ttu-id="8805a-268">KeyEncrypt</span><span class="sxs-lookup"><span data-stu-id="8805a-268">KeyEncrypt</span></span> |[<span data-ttu-id="8805a-269">Šifrování pomocí klíče</span><span class="sxs-lookup"><span data-stu-id="8805a-269">Encrypt with a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx) |
| <span data-ttu-id="8805a-270">KeyDecrypt</span><span class="sxs-lookup"><span data-stu-id="8805a-270">KeyDecrypt</span></span> |[<span data-ttu-id="8805a-271">Dešifrování pomocí klíče</span><span class="sxs-lookup"><span data-stu-id="8805a-271">Decrypt with a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx) |
| <span data-ttu-id="8805a-272">KeyUpdate</span><span class="sxs-lookup"><span data-stu-id="8805a-272">KeyUpdate</span></span> |[<span data-ttu-id="8805a-273">Aktualizace klíče</span><span class="sxs-lookup"><span data-stu-id="8805a-273">Update a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx) |
| <span data-ttu-id="8805a-274">KeyList</span><span class="sxs-lookup"><span data-stu-id="8805a-274">KeyList</span></span> |[<span data-ttu-id="8805a-275">Seznam hello klíčů v trezoru</span><span class="sxs-lookup"><span data-stu-id="8805a-275">List hello keys in a vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx) |
| <span data-ttu-id="8805a-276">KeyListVersions</span><span class="sxs-lookup"><span data-stu-id="8805a-276">KeyListVersions</span></span> |[<span data-ttu-id="8805a-277">Seznam verzí hello klíče</span><span class="sxs-lookup"><span data-stu-id="8805a-277">List hello versions of a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx) |
| <span data-ttu-id="8805a-278">SecretSet</span><span class="sxs-lookup"><span data-stu-id="8805a-278">SecretSet</span></span> |[<span data-ttu-id="8805a-279">Vytvoření tajného kódu</span><span class="sxs-lookup"><span data-stu-id="8805a-279">Create a secret</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx) |
| <span data-ttu-id="8805a-280">SecretGet</span><span class="sxs-lookup"><span data-stu-id="8805a-280">SecretGet</span></span> |[<span data-ttu-id="8805a-281">Získání tajného kódu</span><span class="sxs-lookup"><span data-stu-id="8805a-281">Get secret</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx) |
| <span data-ttu-id="8805a-282">SecretUpdate</span><span class="sxs-lookup"><span data-stu-id="8805a-282">SecretUpdate</span></span> |[<span data-ttu-id="8805a-283">Aktualizace tajného kódu</span><span class="sxs-lookup"><span data-stu-id="8805a-283">Update a secret</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx) |
| <span data-ttu-id="8805a-284">SecretDelete</span><span class="sxs-lookup"><span data-stu-id="8805a-284">SecretDelete</span></span> |[<span data-ttu-id="8805a-285">Odstranění tajného kódu</span><span class="sxs-lookup"><span data-stu-id="8805a-285">Delete a secret</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx) |
| <span data-ttu-id="8805a-286">SecretList</span><span class="sxs-lookup"><span data-stu-id="8805a-286">SecretList</span></span> |[<span data-ttu-id="8805a-287">Výpis tajných kódů v trezoru</span><span class="sxs-lookup"><span data-stu-id="8805a-287">List secrets in a vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx) |
| <span data-ttu-id="8805a-288">SecretListVersions</span><span class="sxs-lookup"><span data-stu-id="8805a-288">SecretListVersions</span></span> |[<span data-ttu-id="8805a-289">Výpis verzí tajného kódu</span><span class="sxs-lookup"><span data-stu-id="8805a-289">List versions of a secret</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx) |

## <span data-ttu-id="8805a-290"><a id="loganalytics"></a>Použití Log Analytics</span><span class="sxs-lookup"><span data-stu-id="8805a-290"><a id="loganalytics"></a>Use Log Analytics</span></span>

<span data-ttu-id="8805a-291">V tooreview analýzy protokolů, Azure Key Vault AuditEvent protokolů můžete použít hello Azure Key Vault řešení.</span><span class="sxs-lookup"><span data-stu-id="8805a-291">You can use hello Azure Key Vault solution in Log Analytics tooreview Azure Key Vault AuditEvent logs.</span></span> <span data-ttu-id="8805a-292">Další informace, včetně jak tooset to, najdete v části [řešení Azure Key Vault ve analýzy protokolů](../log-analytics/log-analytics-azure-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="8805a-292">For more information, including how tooset this up, see [Azure Key Vault solution in Log Analytics](../log-analytics/log-analytics-azure-key-vault.md).</span></span> <span data-ttu-id="8805a-293">Tento článek také obsahuje pokyny, pokud potřebujete toomigrate z hello staré Key Vault řešení nabídnuté během analýzy protokolů hello preview, kde je první směrovat vaše protokoly tooan účet úložiště Azure a nakonfigurován tooread analýzy protokolů z ní.</span><span class="sxs-lookup"><span data-stu-id="8805a-293">This article also contains instructions if you need toomigrate from hello old Key Vault solution that was offered during hello Log Analytics preview, where you first routed your logs tooan Azure Storage account and configured Log Analytics tooread from there.</span></span>

## <span data-ttu-id="8805a-294"><a id="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="8805a-294"><a id="next"></a>Next steps</span></span>
<span data-ttu-id="8805a-295">Chcete-li používat Azure Key Vault ve webové aplikaci, podívejte se na kurz [Použití Azure Key Vault z webové aplikace](key-vault-use-from-web-application.md).</span><span class="sxs-lookup"><span data-stu-id="8805a-295">For a tutorial that uses Azure Key Vault in a web application, see [Use Azure Key Vault from a Web Application](key-vault-use-from-web-application.md).</span></span>

<span data-ttu-id="8805a-296">Programátorské reference najdete v části [hello Příručka pro vývojáře Azure Key Vault](key-vault-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="8805a-296">For programming references, see [hello Azure Key Vault developer's guide](key-vault-developers-guide.md).</span></span>

<span data-ttu-id="8805a-297">Seznam rutin Azure PowerShellu 1.0 pro Azure Key Vault naleznete v tématu [Rutiny Azure Key Vault](/powershell/module/azurerm.keyvault/#key_vault).</span><span class="sxs-lookup"><span data-stu-id="8805a-297">For a list of Azure PowerShell 1.0 cmdlets for Azure Key Vault, see [Azure Key Vault Cmdlets](/powershell/module/azurerm.keyvault/#key_vault).</span></span>

<span data-ttu-id="8805a-298">Kurz týkající se střídání klíče a protokolu auditování s Azure Key Vault, najdete v části [jak toosetup Key Vault s end tooend klíčů otočení a auditování](key-vault-key-rotation-log-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="8805a-298">For a tutorial on key rotation and log auditing with Azure Key Vault, see [How toosetup Key Vault with end tooend key rotation and auditing](key-vault-key-rotation-log-monitoring.md).</span></span>
