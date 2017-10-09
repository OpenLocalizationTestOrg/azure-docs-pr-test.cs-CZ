---
title: "aaaManage Azure Key Vault, pomocí rozhraní příkazového řádku | Microsoft Docs"
description: "Použijte tento kurz tooautomate běžné úlohy v Key Vault pomocí hello 2.0 rozhraní příkazového řádku"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: ambapat
ms.openlocfilehash: 76855c0ea09b6b307159468382a6a63627205556
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-key-vault-using-cli-20"></a><span data-ttu-id="7587f-103">Spravovat pomocí rozhraní příkazového řádku 2.0 Key Vault</span><span class="sxs-lookup"><span data-stu-id="7587f-103">Manage Key Vault using CLI 2.0</span></span>
<span data-ttu-id="7587f-104">Azure Key Vault je dostupný ve většině oblastí.</span><span class="sxs-lookup"><span data-stu-id="7587f-104">Azure Key Vault is available in most regions.</span></span> <span data-ttu-id="7587f-105">Další informace najdete v tématu hello [stránce s cenami Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="7587f-105">For more information, see hello [Key Vault pricing page](https://azure.microsoft.com/pricing/details/key-vault/).</span></span>

## <a name="introduction"></a><span data-ttu-id="7587f-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="7587f-106">Introduction</span></span>
<span data-ttu-id="7587f-107">Použijte tento kurz toohelp získáte začít s Azure Key Vault toocreate zesílený kontejner (trezor) v Azure, toostore a spravovat kryptografické klíče a tajné klíče v Azure.</span><span class="sxs-lookup"><span data-stu-id="7587f-107">Use this tutorial toohelp you get started with Azure Key Vault toocreate a hardened container (a vault) in Azure, toostore and manage cryptographic keys and secrets in Azure.</span></span> <span data-ttu-id="7587f-108">Provede vás procesem hello pomocí rozhraní příkazového řádku pro různé platformy Azure toocreate trezoru, který obsahuje klíč nebo heslo, které pak můžete použít s aplikací Azure.</span><span class="sxs-lookup"><span data-stu-id="7587f-108">It walks you through hello process of using Azure Cross-Platform Command-Line Interface toocreate a vault that contains a key or password that you can then use with an Azure application.</span></span> <span data-ttu-id="7587f-109">Poté vám ukáže, jak aplikaci můžete potom použít tento klíč nebo heslo.</span><span class="sxs-lookup"><span data-stu-id="7587f-109">It then shows you how an application can then use that key or password.</span></span>

<span data-ttu-id="7587f-110">**Odhadovaný čas toocomplete:** 20 minut</span><span class="sxs-lookup"><span data-stu-id="7587f-110">**Estimated time toocomplete:** 20 minutes</span></span>

> [!NOTE]
> <span data-ttu-id="7587f-111">Tento kurz neobsahuje pokyny, jak toowrite hello Azure aplikace, která obsahuje jeden z hello kroků, které ukazuje, jak tooauthorize aplikaci toouse klíč nebo tajný klíč v hello klíč trezoru.</span><span class="sxs-lookup"><span data-stu-id="7587f-111">This tutorial does not include instructions on how toowrite hello Azure application that one of hello steps includes, which shows how tooauthorize an application toouse a key or secret in hello key vault.</span></span>
>
> <span data-ttu-id="7587f-112">Tento kurz používá hello nejnovější 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="7587f-112">This tutorial uses hello latest Azure CLI 2.0.</span></span>
>
>

<span data-ttu-id="7587f-113">Souhrnné informace o Azure Key Vault naleznete v tématu [Co je Azure Key Vault?](key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7587f-113">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7587f-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7587f-114">Prerequisites</span></span>
<span data-ttu-id="7587f-115">toocomplete tento kurz, musíte mít hello následující:</span><span class="sxs-lookup"><span data-stu-id="7587f-115">toocomplete this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="7587f-116">TooMicrosoft předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="7587f-116">A subscription tooMicrosoft Azure.</span></span> <span data-ttu-id="7587f-117">Pokud jeden nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial).</span><span class="sxs-lookup"><span data-stu-id="7587f-117">If you do not have one, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial).</span></span>
* <span data-ttu-id="7587f-118">Rozhraní příkazového řádku verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="7587f-118">Command-Line Interface version 2.0 or later.</span></span> <span data-ttu-id="7587f-119">tooinstall hello nejnovější verzi a připojte tooyour předplatné Azure, najdete v části [instalace a konfigurace hello 2.0 rozhraní příkazového řádku pro různé platformy Azure](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7587f-119">tooinstall hello latest version and connect tooyour Azure subscription, see [Install and Configure hello Azure Cross-Platform Command-Line Interface 2.0](/cli/azure/install-azure-cli).</span></span>
* <span data-ttu-id="7587f-120">Aplikace, které budou nakonfigurované toouse hello klíč nebo heslo, které vytvoříte v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="7587f-120">An application that will be configured toouse hello key or password that you create in this tutorial.</span></span> <span data-ttu-id="7587f-121">Vzorová aplikace je k dispozici z hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span><span class="sxs-lookup"><span data-stu-id="7587f-121">A sample application is available from hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span></span> <span data-ttu-id="7587f-122">Pokyny najdete v tématu hello doplňujícími souboru Readme.</span><span class="sxs-lookup"><span data-stu-id="7587f-122">For instructions, see hello accompanying Readme file.</span></span>

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a><span data-ttu-id="7587f-123">Získání nápovědy pomocí rozhraní příkazového řádku Azure a platformy</span><span class="sxs-lookup"><span data-stu-id="7587f-123">Getting help with Azure Cross-Platform Command-Line Interface</span></span>
<span data-ttu-id="7587f-124">V tomto kurzu se předpokládá, že jste obeznámeni s hello rozhraní příkazového řádku (Bash, terminálu, příkazového řádku)</span><span class="sxs-lookup"><span data-stu-id="7587f-124">This tutorial assumes that you are familiar with hello command-line interface (Bash, Terminal, Command prompt)</span></span>

<span data-ttu-id="7587f-125">Hello – Nápověda nebo -h parametr může být použit tooview nápovědy pro určité příkazy.</span><span class="sxs-lookup"><span data-stu-id="7587f-125">hello --help or -h parameter can be used tooview help for specific commands.</span></span> <span data-ttu-id="7587f-126">Alternativně hello azure nápovědy [příkaz] [možnosti], že formát lze také použít tooreturn hello stejné informace.</span><span class="sxs-lookup"><span data-stu-id="7587f-126">Alternately, hello azure help [command] [options] format can also be used tooreturn hello same information.</span></span> <span data-ttu-id="7587f-127">Například následující hello příkazy všechny návratové hello stejné informace:</span><span class="sxs-lookup"><span data-stu-id="7587f-127">For example, hello following commands all return hello same information:</span></span>

```
az account set --help
az account set -h
```

<span data-ttu-id="7587f-128">Pokud máte pochybnosti o parametrech hello vyžaduje příkaz, naleznete v toohelp pomocí – Nápověda, – h nebo az help [příkaz].</span><span class="sxs-lookup"><span data-stu-id="7587f-128">When in doubt about hello parameters needed by a command, refer toohelp using --help, -h or az help [command].</span></span>

<span data-ttu-id="7587f-129">Také si můžete přečíst následující kurzy tooget obeznámeni s Azure Resource Manager v rozhraní příkazového řádku a platformy Azure hello:</span><span class="sxs-lookup"><span data-stu-id="7587f-129">You can also read hello following tutorials tooget familiar with Azure Resource Manager in Azure Cross-Platform Command-Line Interface:</span></span>

* [<span data-ttu-id="7587f-130">Instalace rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="7587f-130">Install Azure CLI</span></span>](/cli/azure/install-azure-cli)
* [<span data-ttu-id="7587f-131">Začínáme s Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="7587f-131">Get started with Azure CLI 2.0</span></span>](/cli/azure/get-started-with-azure-cli)

## <a name="connect-tooyour-subscriptions"></a><span data-ttu-id="7587f-132">Připojit tooyour odběrů</span><span class="sxs-lookup"><span data-stu-id="7587f-132">Connect tooyour subscriptions</span></span>
<span data-ttu-id="7587f-133">toolog pomocí účtu organizace, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7587f-133">toolog in using an organizational account, use hello following command:</span></span>

```
az login -u username@domain.com -p password
```

<span data-ttu-id="7587f-134">nebo pokud chcete toolog zadáním interaktivně</span><span class="sxs-lookup"><span data-stu-id="7587f-134">or if you want toolog in by typing interactively</span></span>

```
az login
```

<span data-ttu-id="7587f-135">Pokud máte více předplatných a chcete toospecify konkrétní jeden toouse pro Azure Key Vault, zadejte hello následující toosee hello předplatných pro váš účet:</span><span class="sxs-lookup"><span data-stu-id="7587f-135">If you have multiple subscriptions and want toospecify a specific one toouse for Azure Key Vault, type hello following toosee hello subscriptions for your account:</span></span>

```
az account list
```

<span data-ttu-id="7587f-136">Potom toospecify hello předplatné toouse, zadejte:</span><span class="sxs-lookup"><span data-stu-id="7587f-136">Then, toospecify hello subscription toouse, type:</span></span>

```
az account set --subscription <subscription name or ID>
```

<span data-ttu-id="7587f-137">Další informace o konfiguraci rozhraní příkazového řádku pro různé platformy Azure najdete v tématu [nainstalovat rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7587f-137">For more information about configuring Azure Cross-Platform Command-Line Interface, see [Install Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="create-a-new-resource-group"></a><span data-ttu-id="7587f-138">Vytvoření nové skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="7587f-138">Create a new resource group</span></span>
<span data-ttu-id="7587f-139">Pokud používáte Azure Resource Manager, všechny související prostředky vytváří v uvnitř skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="7587f-139">When using Azure Resource Manager, all related resources are created inside a resource group.</span></span> <span data-ttu-id="7587f-140">Pro účely tohoto kurzu vytvoříme novou skupinu prostředků, ContosoResourceGroup'.</span><span class="sxs-lookup"><span data-stu-id="7587f-140">We will create a new resource group 'ContosoResourceGroup' for this tutorial.</span></span>

```
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

<span data-ttu-id="7587f-141">první parametr Hello je název skupiny prostředků a druhý parametr hello je hello umístění.</span><span class="sxs-lookup"><span data-stu-id="7587f-141">hello first parameter is resource group name and hello second parameter is hello location.</span></span> <span data-ttu-id="7587f-142">Pro umístění, použijte příkaz hello `az account list-locations` tooidentify jak toospecify alternativní umístění toohello jednu v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="7587f-142">For location, use hello command `az account list-locations` tooidentify how toospecify an alternative location toohello one in this example.</span></span> <span data-ttu-id="7587f-143">Pokud potřebujete více informací, zadejte: `az account list-locations -h`.</span><span class="sxs-lookup"><span data-stu-id="7587f-143">If you need more information, type: `az account list-locations -h`.</span></span>

## <a name="register-hello-key-vault-resource-provider"></a><span data-ttu-id="7587f-144">Registrace poskytovatele prostředků hello Key Vault</span><span class="sxs-lookup"><span data-stu-id="7587f-144">Register hello Key Vault resource provider</span></span>
<span data-ttu-id="7587f-145">Ujistěte se, že poskytovatele prostředků Key Vault je zaregistrován v rámci vašeho předplatného:</span><span class="sxs-lookup"><span data-stu-id="7587f-145">Make sure that Key Vault resource provider is registered in your subscription:</span></span>

```
az provider register -n Microsoft.KeyVault
```

<span data-ttu-id="7587f-146">Stačí to toobe provádí jednou za předplatné.</span><span class="sxs-lookup"><span data-stu-id="7587f-146">This only needs toobe done once per subscription.</span></span>

## <a name="create-a-key-vault"></a><span data-ttu-id="7587f-147">Vytvořte trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="7587f-147">Create a key vault</span></span>
<span data-ttu-id="7587f-148">Použití hello `az keyvault create` příkaz toocreate trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="7587f-148">Use hello `az keyvault create` command toocreate a key vault.</span></span> <span data-ttu-id="7587f-149">Tento skript má tři povinné parametry: název skupiny prostředků, název trezoru klíčů a hello zeměpisné umístění.</span><span class="sxs-lookup"><span data-stu-id="7587f-149">This script has three mandatory parameters: a resource group name, a key vault name, and hello geographic location.</span></span>

<span data-ttu-id="7587f-150">Například pokud použijete název trezoru hello ContosoKeyVault, název skupiny prostředků hello ContosoResourceGroup a hello umístění východní Asie, zadejte:</span><span class="sxs-lookup"><span data-stu-id="7587f-150">For example, if you use hello vault name of ContosoKeyVault, hello resource group name of ContosoResourceGroup, and hello location of East Asia, type:</span></span>
```
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

<span data-ttu-id="7587f-151">Hello výstup tohoto příkazu zobrazuje vlastnosti trezoru klíčů hello, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="7587f-151">hello output of this command shows properties of hello key vault that you've just created.</span></span> <span data-ttu-id="7587f-152">nejdůležitější vlastnosti Hello dva jsou:</span><span class="sxs-lookup"><span data-stu-id="7587f-152">hello two most important properties are:</span></span>

* <span data-ttu-id="7587f-153">**název**: V příkladu hello je to ContosoKeyVault.</span><span class="sxs-lookup"><span data-stu-id="7587f-153">**name**: In hello example this is ContosoKeyVault.</span></span> <span data-ttu-id="7587f-154">Tento název pro jiné příkazy Key Vault budete používat.</span><span class="sxs-lookup"><span data-stu-id="7587f-154">You will use this name for other Key Vault commands.</span></span>
* <span data-ttu-id="7587f-155">**vaultUri**: V příkladu hello je to https://contosokeyvault.vault.azure.net.</span><span class="sxs-lookup"><span data-stu-id="7587f-155">**vaultUri**: In hello example this is https://contosokeyvault.vault.azure.net.</span></span> <span data-ttu-id="7587f-156">Aplikace, které používají váš trezor prostřednictvím REST API musí používat tento identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="7587f-156">Applications that use your vault through its REST API must use this URI.</span></span>

<span data-ttu-id="7587f-157">Váš účet Azure je autorizovaný tooperform žádné operace pro tento klíč trezoru.</span><span class="sxs-lookup"><span data-stu-id="7587f-157">Your Azure account is now authorized tooperform any operations on this key vault.</span></span> <span data-ttu-id="7587f-158">Zatím k tomu není oprávněn nikdo jiný.</span><span class="sxs-lookup"><span data-stu-id="7587f-158">As yet, nobody else is.</span></span>

## <a name="add-a-key-or-secret-toohello-key-vault"></a><span data-ttu-id="7587f-159">Přidat klíč nebo tajný toohello trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="7587f-159">Add a key or secret toohello key vault</span></span>
<span data-ttu-id="7587f-160">Pokud chcete Azure Key Vault toocreate klíč chráněný softwarem pro vás, použijte hello `az key create` příkaz a zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="7587f-160">If you want Azure Key Vault toocreate a software-protected key for you, use hello `az key create` command, and type hello following:</span></span>
```
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```
<span data-ttu-id="7587f-161">Nicméně pokud už máte existující klíč v soubor .pem uložený jako místního souboru do souboru s názvem softkey.pem, které chcete tooupload tooAzure Key Vault, zadejte hello následujících tooimport hello klíč z hello. Soubor PEM, který chrání klíč hello softwarem hello služby Key Vault:</span><span class="sxs-lookup"><span data-stu-id="7587f-161">However, if you have an existing key in a .pem file saved as local file in a file named softkey.pem that you want tooupload tooAzure Key Vault, type hello following tooimport hello key from hello .PEM file, which protects hello key by software in hello Key Vault service:</span></span>
```
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'PaSSWORD' --protection software
```
<span data-ttu-id="7587f-162">Nyní můžete odkazovat hello klíče vytvořené nebo odeslán tooAzure Key Vault, pomocí jeho identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="7587f-162">You can now reference hello key that you created or uploaded tooAzure Key Vault, by using its URI.</span></span> <span data-ttu-id="7587f-163">Použít **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways získat aktuální verzi hello a používat **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** tooget konkrétní verzi.</span><span class="sxs-lookup"><span data-stu-id="7587f-163">Use  **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways get hello current version, and use **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** tooget this specific version.</span></span>

<span data-ttu-id="7587f-164">tooadd tajný toohello úložiště, který je hesla s názvem SQLPassword a hodnotou Pa$ $w0rd tooAzure Key Vault, typ hello následující hello:</span><span class="sxs-lookup"><span data-stu-id="7587f-164">tooadd a secret toohello vault, which is a password named SQLPassword and that has hello value of Pa$$w0rd tooAzure Key Vault, type hello following:</span></span>
```
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```
<span data-ttu-id="7587f-165">Nyní si můžete odkazy Toto heslo přidali tooAzure Key Vault, a to pomocí jeho identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="7587f-165">You can now reference this password that you added tooAzure Key Vault, by using its URI.</span></span> <span data-ttu-id="7587f-166">Použít **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways získat aktuální verzi hello a používat **https://ContosoVault.vault.azure.net/secrets/SQLPassword/ 90018dbb96a84117a0d2847ef8e7189d** tooget konkrétní verzi.</span><span class="sxs-lookup"><span data-stu-id="7587f-166">Use **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways get hello current version, and use **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** tooget this specific version.</span></span>

<span data-ttu-id="7587f-167">Podívejme se, zda text hello klíč nebo tajný klíč, který jste právě vytvořili:</span><span class="sxs-lookup"><span data-stu-id="7587f-167">Let's view hello key or secret that you just created:</span></span>

* <span data-ttu-id="7587f-168">tooview vaše, zadejte:`az keyvault key list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="7587f-168">tooview your key, type: `az keyvault key list --vault-name 'ContosoKeyVault'`</span></span>
* <span data-ttu-id="7587f-169">tooview váš tajný, typ:`az keyvault secret list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="7587f-169">tooview your secret, type: `az keyvault secret list --vault-name 'ContosoKeyVault'`</span></span>

## <a name="register-an-application-with-azure-active-directory"></a><span data-ttu-id="7587f-170">Registrujte aplikaci s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7587f-170">Register an application with Azure Active Directory</span></span>
<span data-ttu-id="7587f-171">Tento krok obvykle provádí vývojář na samostatném počítači.</span><span class="sxs-lookup"><span data-stu-id="7587f-171">This step would usually be done by a developer, on a separate computer.</span></span> <span data-ttu-id="7587f-172">Není konkrétní tooAzure Key Vault, ale je zahrnuté pro úplnost.</span><span class="sxs-lookup"><span data-stu-id="7587f-172">It is not specific tooAzure Key Vault but is included here, for completeness.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7587f-173">toocomplete hello kurzu, váš účet, hello trezoru a hello aplikace, která zaregistrujete v tomto kroku musí být ve hello stejném adresáři Azure.</span><span class="sxs-lookup"><span data-stu-id="7587f-173">toocomplete hello tutorial, your account, hello vault, and hello application that you will register in this step must all be in hello same Azure directory.</span></span>
>
>

<span data-ttu-id="7587f-174">Aplikace, které používají trezor klíčů, se musí ověřit pomocí tokenu z Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7587f-174">Applications that use a key vault must authenticate by using a token from Azure Active Directory.</span></span> <span data-ttu-id="7587f-175">toodo se hello vlastníka aplikace hello musí hello aplikaci nejprve zaregistrovat v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7587f-175">toodo this, hello owner of hello application must first register hello application in their Azure Active Directory.</span></span> <span data-ttu-id="7587f-176">Na hello konci registrace obdrží majitel aplikace hello hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="7587f-176">At hello end of registration, hello application owner gets hello following values:</span></span>

* <span data-ttu-id="7587f-177">**ID aplikace** (také označované jako ID klienta) a **ověřovací klíč** (také označované jako hello sdílený tajný klíč).</span><span class="sxs-lookup"><span data-stu-id="7587f-177">An **Application ID** (also known as a Client ID) and **authentication key** (also known as hello shared secret).</span></span> <span data-ttu-id="7587f-178">Hello aplikace musí být obě tyto hodnoty tooAzure služby Active Directory, tooget token.</span><span class="sxs-lookup"><span data-stu-id="7587f-178">hello application must present both of these values tooAzure Active Directory, tooget a token.</span></span> <span data-ttu-id="7587f-179">Jak aplikace hello je nakonfigurován toodo to závisí na aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="7587f-179">How hello application is configured toodo this depends on hello application.</span></span> <span data-ttu-id="7587f-180">Pro hello ukázkovou aplikaci Key Vault nastavuje majitel aplikace hello tyto hodnoty v souboru app.config hello.</span><span class="sxs-lookup"><span data-stu-id="7587f-180">For hello Key Vault sample application, hello application owner sets these values in hello app.config file.</span></span>

<span data-ttu-id="7587f-181">tooregister hello aplikace v Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="7587f-181">tooregister hello application in Azure Active Directory:</span></span>

1. <span data-ttu-id="7587f-182">Přihlaste se toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7587f-182">Sign in toohello Azure portal.</span></span>
2. <span data-ttu-id="7587f-183">Na levé straně hello, klikněte na tlačítko **Azure Active Directory**a potom vyberte hello adresář, ve kterém zaregistrujete vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7587f-183">On hello left, click **Azure Active Directory**, and then select hello directory in which you will register your application.</span></span> <br> <br> 

> [!Note] 
> <span data-ttu-id="7587f-184">Je nutné vybrat hello hello stejný adresář, který obsahuje předplatné Azure, který jste použili pro vytvoření trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="7587f-184">You must select hello same directory that contains hello Azure subscription with which you created your key vault.</span></span> <span data-ttu-id="7587f-185">Pokud si nejste jisti který adresář to je, klikněte na tlačítko **nastavení**, určete hello předplatné, které jste použili pro vytvoření trezoru klíčů, a Poznámka hello název adresáře hello zobrazí v posledním sloupci hello.</span><span class="sxs-lookup"><span data-stu-id="7587f-185">If you do not know which directory this is, click **Settings**, identify hello subscription with which you created your key vault, and note hello name of hello directory displayed in hello last column.</span></span>

3. <span data-ttu-id="7587f-186">Klikněte na **Aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7587f-186">Click **APPLICATIONS**.</span></span> <span data-ttu-id="7587f-187">Pokud nebyly přidané žádné aplikace tooyour directory, tato stránka se zobrazí pouze hello **přidat aplikaci** odkaz.</span><span class="sxs-lookup"><span data-stu-id="7587f-187">If no apps have been added tooyour directory, this page will show only hello **Add an App** link.</span></span> <span data-ttu-id="7587f-188">Kliknutím na odkaz hello nebo, případně můžete kliknutím na tlačítko hello **přidat** na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="7587f-188">Click hello link, or alternatively, you can click hello **ADD** on hello command bar.</span></span>
4. <span data-ttu-id="7587f-189">V hello **přidat aplikaci** na hello **co chcete toodo?** klikněte na tlačítko **přidat aplikaci, kterou vyvíjí Moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="7587f-189">In hello **ADD APPLICATION** wizard, on hello **What do you want toodo?** page, click **Add an application my organization is developing**.</span></span>
5. <span data-ttu-id="7587f-190">Na hello **Řekněte nám o své aplikaci** , zadejte název pro vaši aplikaci a vyberte **webové aplikace nebo webové rozhraní API** (hello výchozí).</span><span class="sxs-lookup"><span data-stu-id="7587f-190">On hello **Tell us about your application** page, specify a name for your application and select **WEB APPLICATION AND/OR WEB API** (hello default).</span></span> <span data-ttu-id="7587f-191">Kliknutím na ikonu další hello.</span><span class="sxs-lookup"><span data-stu-id="7587f-191">Click hello Next icon.</span></span>
6. <span data-ttu-id="7587f-192">Na hello **vlastností aplikace** zadejte hello **adresa URL přihlašování** a **identifikátor ID URI aplikace** pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7587f-192">On hello **App properties** page, specify hello **SIGN-ON URL** and **APP ID URI** for your web application.</span></span> <span data-ttu-id="7587f-193">Pokud vaše aplikace tyto hodnoty neobsahuje, v tomto kroku si je můžete vymyslet (například můžete do obou polí zadat http://test1.contoso.com).</span><span class="sxs-lookup"><span data-stu-id="7587f-193">If your application does not have these values, you can make them up for this step (for example, you could specify http://test1.contoso.com for both boxes).</span></span> <span data-ttu-id="7587f-194">Není důležité, pokud tyto stránky existují; Co je důležité je, že tuto aplikaci hello identifikátor ID URI pro každou aplikaci se liší pro každou aplikaci ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="7587f-194">It does not matter if these sites exist; what is important is that hello app ID URI for each application is different for every application in your directory.</span></span> <span data-ttu-id="7587f-195">Hello directory používá tento řetězec tooidentify vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7587f-195">hello directory uses this string tooidentify your app.</span></span>
7. <span data-ttu-id="7587f-196">Klikněte na tlačítko hello dokončení ikonu toosave změny v Průvodci hello.</span><span class="sxs-lookup"><span data-stu-id="7587f-196">Click hello Complete icon toosave your changes in hello wizard.</span></span>
8. <span data-ttu-id="7587f-197">Na stránce hello rychlý Start, klikněte na **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="7587f-197">On hello Quick Start page, click **CONFIGURE**.</span></span>
9. <span data-ttu-id="7587f-198">Posuňte se toohello **klíče** vyberte dobu trvání hello a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7587f-198">Scroll toohello **keys** section, select hello duration, and then click **SAVE**.</span></span> <span data-ttu-id="7587f-199">stránku Hello se obnoví a zobrazí hodnotu klíče.</span><span class="sxs-lookup"><span data-stu-id="7587f-199">hello page refreshes and now shows a key value.</span></span> <span data-ttu-id="7587f-200">Vaši aplikaci je nutné nakonfigurovat s touto hodnotou klíče a hello **ID klienta** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7587f-200">You must configure your application with this key value and hello **CLIENT ID** value.</span></span> <span data-ttu-id="7587f-201">(Pokyny k této konfiguraci se budou lišit podle aplikace.)</span><span class="sxs-lookup"><span data-stu-id="7587f-201">(Instructions for this configuration will be application-specific.)</span></span>
10. <span data-ttu-id="7587f-202">Zkopírujte hodnotu ID klienta hello z této stránky, které budete používat v hello další krok tooset oprávnění pro váš trezoru.</span><span class="sxs-lookup"><span data-stu-id="7587f-202">Copy hello client ID value from this page, which you will use in hello next step tooset permissions on your vault.</span></span>

## <a name="authorize-hello-application-toouse-hello-key-or-secret"></a><span data-ttu-id="7587f-203">Autorizovat hello aplikace toouse hello klíče nebo tajného klíče</span><span class="sxs-lookup"><span data-stu-id="7587f-203">Authorize hello application toouse hello key or secret</span></span>
<span data-ttu-id="7587f-204">tooaccess aplikace hello tooauthorize hello klíče nebo tajného klíče v trezoru hello, použijte hello `az keyvault set-policy` příkaz.</span><span class="sxs-lookup"><span data-stu-id="7587f-204">tooauthorize hello application tooaccess hello key or secret in hello vault, use hello `az keyvault set-policy` command.</span></span>

<span data-ttu-id="7587f-205">Například pokud je název vaší trezoru ContosoKeyVault a hello aplikace, které chcete tooauthorize má hodnotu 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed ID klienta a chcete toodecrypt aplikace hello tooauthorize a přihlaste s klíči v trezoru, spusťte hello následující:</span><span class="sxs-lookup"><span data-stu-id="7587f-205">For example, if your vault name is ContosoKeyVault and hello application you want tooauthorize has a client ID of 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, and you want tooauthorize hello application toodecrypt and sign with keys in your vault, then run hello following:</span></span>
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

<span data-ttu-id="7587f-206">Pokud chcete, tooauthorize Tento stejný tooread tajné klíče aplikace ve vašem trezoru, spusťte hello následující:</span><span class="sxs-lookup"><span data-stu-id="7587f-206">If you want tooauthorize that same application tooread secrets in your vault, run hello following:</span></span>
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```
## <a name="if-you-want-toouse-a-hardware-security-module-hsm"></a><span data-ttu-id="7587f-207">Pokud chcete, aby toouse modul hardwarového zabezpečení (HSM)</span><span class="sxs-lookup"><span data-stu-id="7587f-207">If you want toouse a hardware security module (HSM)</span></span>
<span data-ttu-id="7587f-208">Pro lepší kontrolu můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM), které nikdy neopustí hranice HSM hello.</span><span class="sxs-lookup"><span data-stu-id="7587f-208">For added assurance, you can import or generate keys in hardware security modules (HSMs) that never leave hello HSM boundary.</span></span> <span data-ttu-id="7587f-209">Hello moduly hardwarového zabezpečení jsou FIPS 140-2 Level 2 ověřit.</span><span class="sxs-lookup"><span data-stu-id="7587f-209">hello HSMs are FIPS 140-2 Level 2 validated.</span></span> <span data-ttu-id="7587f-210">Pokud tento požadavek netýká tooyou, tuto část přeskočte a přejděte příliš[odstranit hello trezoru klíčů, přidružených klíčů a tajných klíčů](#delete-the-key-vault-and-associated-keys-and-secrets).</span><span class="sxs-lookup"><span data-stu-id="7587f-210">If this requirement doesn't apply tooyou, skip this section and go too[Delete hello key vault and associated keys and secrets](#delete-the-key-vault-and-associated-keys-and-secrets).</span></span>

<span data-ttu-id="7587f-211">toocreate těchto klíčů chráněných pomocí HSM musíte mít předplatné trezoru s podporou klíčů chráněných pomocí HSM.</span><span class="sxs-lookup"><span data-stu-id="7587f-211">toocreate these HSM-protected keys, you must have a vault subscription that supports HSM-protected keys.</span></span>

<span data-ttu-id="7587f-212">Když vytvoříte hello keyvault, přidejte parametr "sku" hello:</span><span class="sxs-lookup"><span data-stu-id="7587f-212">When you create hello keyvault, add hello 'sku' parameter:</span></span>

```
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```
<span data-ttu-id="7587f-213">Můžete přidat klíče chráněné softwarem (jak je uvedeno výše) a trezoru klíčů chráněných pomocí HSM toothis.</span><span class="sxs-lookup"><span data-stu-id="7587f-213">You can add software-protected keys (as shown earlier) and HSM-protected keys toothis vault.</span></span> <span data-ttu-id="7587f-214">toocreate klíč chráněný HSM, sada hello cílový parametr too'HSM':</span><span class="sxs-lookup"><span data-stu-id="7587f-214">toocreate an HSM-protected key, set hello Destination parameter too'HSM':</span></span>

```
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

<span data-ttu-id="7587f-215">Můžete použít následující příkaz tooimport klíč ze souboru .pem ve vašem počítači hello.</span><span class="sxs-lookup"><span data-stu-id="7587f-215">You can use hello following command tooimport a key from a .pem file on your computer.</span></span> <span data-ttu-id="7587f-216">Tento příkaz importuje hello klíč do HSM ve hello služby Key Vault:</span><span class="sxs-lookup"><span data-stu-id="7587f-216">This command imports hello key into HSMs in hello Key Vault service:</span></span>

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```
<span data-ttu-id="7587f-217">Hello další příkaz importuje "přineste si vlastní klíč" (BYOK) balíčku.</span><span class="sxs-lookup"><span data-stu-id="7587f-217">hello next command imports a “bring your own key" (BYOK) package.</span></span> <span data-ttu-id="7587f-218">To umožňuje vygenerovat klíč v místním HSM a jeho přenesení tooHSMs v hello služby Key Vault, bez hello klíč opustil hranice HSM hello:</span><span class="sxs-lookup"><span data-stu-id="7587f-218">This lets you generate your key in your local HSM, and transfer it tooHSMs in hello Key Vault service, without hello key leaving hello HSM boundary:</span></span>

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```
<span data-ttu-id="7587f-219">Podrobnější pokyny, jak toogenerate tento balíček BYOK najdete v části [jak toouse HSM-Protected klíčů s Azure Key Vault](key-vault-hsm-protected-keys.md).</span><span class="sxs-lookup"><span data-stu-id="7587f-219">For more detailed instructions about how toogenerate this BYOK package, see [How toouse HSM-Protected Keys with Azure Key Vault](key-vault-hsm-protected-keys.md).</span></span>

## <a name="delete-hello-key-vault-and-associated-keys-and-secrets"></a><span data-ttu-id="7587f-220">Odstranit hello trezoru klíčů, přidružených klíčů a tajných klíčů</span><span class="sxs-lookup"><span data-stu-id="7587f-220">Delete hello key vault and associated keys and secrets</span></span>
<span data-ttu-id="7587f-221">Pokud již nepotřebujete trezor klíčů hello a hello klíč nebo tajný klíč, který obsahuje, můžete hello trezor klíčů odstranit pomocí hello `az keyvault delete` příkaz:</span><span class="sxs-lookup"><span data-stu-id="7587f-221">If you no longer need hello key vault and hello key or secret that it contains, you can delete hello key vault by using hello `az keyvault delete` command:</span></span>

```
az keyvault delete --name 'ContosoKeyVault'
```

<span data-ttu-id="7587f-222">Nebo můžete odstranit skupinu prostředků celý Azure, která zahrnuje trezor klíčů hello a další prostředky, které jste zahrnuli do této skupiny:</span><span class="sxs-lookup"><span data-stu-id="7587f-222">Or, you can delete an entire Azure resource group, which includes hello key vault and any other resources that you included in that group:</span></span>

```
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a><span data-ttu-id="7587f-223">Ostatní příkazy rozhraní příkazového řádku Azure a platformy</span><span class="sxs-lookup"><span data-stu-id="7587f-223">Other Azure Cross-Platform Command-line Interface Commands</span></span>
<span data-ttu-id="7587f-224">Další příkazy, že může využít ke správě Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="7587f-224">Other commands that you might useful for managing Azure Key Vault.</span></span>

<span data-ttu-id="7587f-225">Tento příkaz vypíše tabulkové zobrazení všech klíčů a vybraných vlastností:</span><span class="sxs-lookup"><span data-stu-id="7587f-225">This command lists a tabular display of all keys and selected properties:</span></span>

<span data-ttu-id="7587f-226">AZ seznam klíčů keyvault – název trezoru ContosoKeyVault</span><span class="sxs-lookup"><span data-stu-id="7587f-226">az keyvault key list --vault-name 'ContosoKeyVault'</span></span>

<span data-ttu-id="7587f-227">Tento příkaz zobrazí úplný seznam vlastností pro zadaný klíč hello:</span><span class="sxs-lookup"><span data-stu-id="7587f-227">This command displays a full list of properties for hello specified key:</span></span>

<span data-ttu-id="7587f-228">Zobrazit klíč keyvault az--název trezoru ContosoKeyVault – název 'ContosoFirstKey.</span><span class="sxs-lookup"><span data-stu-id="7587f-228">az keyvault key show --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'</span></span>

<span data-ttu-id="7587f-229">Tento příkaz vypíše tabulkové zobrazení všech názvů tajných klíčů a vybraných vlastností:</span><span class="sxs-lookup"><span data-stu-id="7587f-229">This command lists a tabular display of all secret names and selected properties:</span></span>

<span data-ttu-id="7587f-230">AZ keyvault tajný seznamu – název trezoru ContosoKeyVault</span><span class="sxs-lookup"><span data-stu-id="7587f-230">az keyvault secret list --vault-name 'ContosoKeyVault'</span></span>

<span data-ttu-id="7587f-231">Tady je příklad toho, jak tooremove konkrétního klíče:</span><span class="sxs-lookup"><span data-stu-id="7587f-231">Here's an example of how tooremove a specific key:</span></span>

<span data-ttu-id="7587f-232">odstranění klíče keyvault az--název trezoru ContosoKeyVault – název 'ContosoFirstKey.</span><span class="sxs-lookup"><span data-stu-id="7587f-232">az keyvault key delete --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'</span></span>

<span data-ttu-id="7587f-233">Tady je příklad toho, jak tooremove určitého tajného klíče:</span><span class="sxs-lookup"><span data-stu-id="7587f-233">Here's an example of how tooremove a specific secret:</span></span>

<span data-ttu-id="7587f-234">Odstranit tajný klíč keyvault az--název trezoru ContosoKeyVault – název 'SQLPassword.</span><span class="sxs-lookup"><span data-stu-id="7587f-234">az keyvault secret delete --vault-name 'ContosoKeyVault' --name 'SQLPassword'</span></span>


## <a name="next-steps"></a><span data-ttu-id="7587f-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7587f-235">Next steps</span></span>
<span data-ttu-id="7587f-236">Úplný referenční rozhraní příkazového řádku Azure pro příkazy trezoru klíčů, najdete v části [odkaz klíč trezoru rozhraní příkazového řádku](/cli/azure/keyvault)</span><span class="sxs-lookup"><span data-stu-id="7587f-236">For complete Azure CLI reference for key vault commands, see [Key Vault CLI reference](/cli/azure/keyvault)</span></span>

<span data-ttu-id="7587f-237">Programátorské reference najdete v části [hello Příručka pro vývojáře Azure Key Vault](key-vault-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="7587f-237">For programming references, see [hello Azure Key Vault developer's guide](key-vault-developers-guide.md).</span></span>
