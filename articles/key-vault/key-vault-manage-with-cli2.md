---
title: "Spravovat pomocí rozhraní příkazového řádku Azure Key Vault | Microsoft Docs"
description: "Tento kurz použijte k automatizaci běžných úkolů v Key Vault pomocí rozhraní příkazového řádku 2.0"
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
ms.openlocfilehash: 5da9f5eceda71ac85259193e0f183c72813e1679
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-key-vault-using-cli-20"></a><span data-ttu-id="0fba1-103">Spravovat pomocí rozhraní příkazového řádku 2.0 Key Vault</span><span class="sxs-lookup"><span data-stu-id="0fba1-103">Manage Key Vault using CLI 2.0</span></span>
<span data-ttu-id="0fba1-104">Azure Key Vault je dostupný ve většině oblastí.</span><span class="sxs-lookup"><span data-stu-id="0fba1-104">Azure Key Vault is available in most regions.</span></span> <span data-ttu-id="0fba1-105">Další informace najdete na [stránce s cenami Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="0fba1-105">For more information, see the [Key Vault pricing page](https://azure.microsoft.com/pricing/details/key-vault/).</span></span>

## <a name="introduction"></a><span data-ttu-id="0fba1-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="0fba1-106">Introduction</span></span>
<span data-ttu-id="0fba1-107">Tento kurz vám pomůže začít s Azure Key Vault a ukáže vám, jak v Azure vytvořit zesílený kontejner (trezor), a jak ukládat a spravovat kryptografické klíče a tajné klíče v Azure.</span><span class="sxs-lookup"><span data-stu-id="0fba1-107">Use this tutorial to help you get started with Azure Key Vault to create a hardened container (a vault) in Azure, to store and manage cryptographic keys and secrets in Azure.</span></span> <span data-ttu-id="0fba1-108">Provede vás provede procesem vytvoření trezoru, který obsahuje klíč nebo heslo, které pak můžete použít s aplikací Azure pomocí rozhraní příkazového řádku pro různé platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="0fba1-108">It walks you through the process of using Azure Cross-Platform Command-Line Interface to create a vault that contains a key or password that you can then use with an Azure application.</span></span> <span data-ttu-id="0fba1-109">Poté vám ukáže, jak aplikaci můžete potom použít tento klíč nebo heslo.</span><span class="sxs-lookup"><span data-stu-id="0fba1-109">It then shows you how an application can then use that key or password.</span></span>

<span data-ttu-id="0fba1-110">**Odhadovaný čas dokončení:** 20 minut</span><span class="sxs-lookup"><span data-stu-id="0fba1-110">**Estimated time to complete:** 20 minutes</span></span>

> [!NOTE]
> <span data-ttu-id="0fba1-111">Tento kurz neobsahuje pokyny o tom, jak napsat aplikaci Azure, že jeden z kroků obsahuje, který ukazuje, jak k autorizaci aplikace pro použití klíče nebo tajného klíče v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="0fba1-111">This tutorial does not include instructions on how to write the Azure application that one of the steps includes, which shows how to authorize an application to use a key or secret in the key vault.</span></span>
>
> <span data-ttu-id="0fba1-112">Tento kurz používá nejnovější 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="0fba1-112">This tutorial uses the latest Azure CLI 2.0.</span></span>
>
>

<span data-ttu-id="0fba1-113">Souhrnné informace o Azure Key Vault naleznete v tématu [Co je Azure Key Vault?](key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0fba1-113">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0fba1-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0fba1-114">Prerequisites</span></span>
<span data-ttu-id="0fba1-115">K dokončení tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="0fba1-115">To complete this tutorial, you must have the following:</span></span>

* <span data-ttu-id="0fba1-116">Předplatné Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="0fba1-116">A subscription to Microsoft Azure.</span></span> <span data-ttu-id="0fba1-117">Pokud jeden nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial).</span><span class="sxs-lookup"><span data-stu-id="0fba1-117">If you do not have one, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial).</span></span>
* <span data-ttu-id="0fba1-118">Rozhraní příkazového řádku verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0fba1-118">Command-Line Interface version 2.0 or later.</span></span> <span data-ttu-id="0fba1-119">Nainstalujte nejnovější verzi a připojení k předplatnému Azure najdete v tématu [instalace a konfigurace Azure napříč platformami rozhraní příkazového řádku 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0fba1-119">To install the latest version and connect to your Azure subscription, see [Install and Configure the Azure Cross-Platform Command-Line Interface 2.0](/cli/azure/install-azure-cli).</span></span>
* <span data-ttu-id="0fba1-120">Aplikaci, kterou nakonfigurujete pro použití klíče nebo hesla, které v tomto kurzu vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="0fba1-120">An application that will be configured to use the key or password that you create in this tutorial.</span></span> <span data-ttu-id="0fba1-121">Vzorová aplikace je k dispozici ve službě [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span><span class="sxs-lookup"><span data-stu-id="0fba1-121">A sample application is available from the [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span></span> <span data-ttu-id="0fba1-122">Pokyny naleznete v souvisejícím souboru Readme.</span><span class="sxs-lookup"><span data-stu-id="0fba1-122">For instructions, see the accompanying Readme file.</span></span>

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a><span data-ttu-id="0fba1-123">Získání nápovědy pomocí rozhraní příkazového řádku Azure a platformy</span><span class="sxs-lookup"><span data-stu-id="0fba1-123">Getting help with Azure Cross-Platform Command-Line Interface</span></span>
<span data-ttu-id="0fba1-124">V tomto kurzu se předpokládá, že jste obeznámeni s rozhraní příkazového řádku (Bash, terminálu, příkazového řádku)</span><span class="sxs-lookup"><span data-stu-id="0fba1-124">This tutorial assumes that you are familiar with the command-line interface (Bash, Terminal, Command prompt)</span></span>

<span data-ttu-id="0fba1-125">--Parametr nápovědy nebo -h lze zobrazit nápovědu pro určité příkazy.</span><span class="sxs-lookup"><span data-stu-id="0fba1-125">The --help or -h parameter can be used to view help for specific commands.</span></span> <span data-ttu-id="0fba1-126">Alternativně azure help [příkaz] [možnosti] formátu lze použít také k vrácení stejné informace.</span><span class="sxs-lookup"><span data-stu-id="0fba1-126">Alternately, The azure help [command] [options] format can also be used to return the same information.</span></span> <span data-ttu-id="0fba1-127">Například následující příkazy, které se všechny vrátí stejné informace:</span><span class="sxs-lookup"><span data-stu-id="0fba1-127">For example, the following commands all return the same information:</span></span>

```
az account set --help
az account set -h
```

<span data-ttu-id="0fba1-128">Pokud máte pochybnosti o parametry vyžadované příkaz, v nápovědě k použití – Nápověda, – h nebo az help [příkaz].</span><span class="sxs-lookup"><span data-stu-id="0fba1-128">When in doubt about the parameters needed by a command, refer to help using --help, -h or az help [command].</span></span>

<span data-ttu-id="0fba1-129">Také si můžete přečíst následující kurzy a seznamte se s Azure Resource Manager v rozhraní příkazového řádku Azure napříč platformami:</span><span class="sxs-lookup"><span data-stu-id="0fba1-129">You can also read the following tutorials to get familiar with Azure Resource Manager in Azure Cross-Platform Command-Line Interface:</span></span>

* [<span data-ttu-id="0fba1-130">Instalace rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="0fba1-130">Install Azure CLI</span></span>](/cli/azure/install-azure-cli)
* [<span data-ttu-id="0fba1-131">Začínáme s Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0fba1-131">Get started with Azure CLI 2.0</span></span>](/cli/azure/get-started-with-azure-cli)

## <a name="connect-to-your-subscriptions"></a><span data-ttu-id="0fba1-132">Připojte se ke svým předplatným</span><span class="sxs-lookup"><span data-stu-id="0fba1-132">Connect to your subscriptions</span></span>
<span data-ttu-id="0fba1-133">K přihlášení pomocí účtu organizace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0fba1-133">To log in using an organizational account, use the following command:</span></span>

```
az login -u username@domain.com -p password
```

<span data-ttu-id="0fba1-134">nebo pokud se chcete přihlásit interaktivně zadáním</span><span class="sxs-lookup"><span data-stu-id="0fba1-134">or if you want to log in by typing interactively</span></span>

```
az login
```

<span data-ttu-id="0fba1-135">Máte-li více předplatných a chcete určit jedno konkrétní, které se má použít pro Azure Key Vault, zadejte následující příkaz k zobrazení předplatných pro váš účet:</span><span class="sxs-lookup"><span data-stu-id="0fba1-135">If you have multiple subscriptions and want to specify a specific one to use for Azure Key Vault, type the following to see the subscriptions for your account:</span></span>

```
az account list
```

<span data-ttu-id="0fba1-136">Poté pro určení předplatného, které se má použít, zadejte:</span><span class="sxs-lookup"><span data-stu-id="0fba1-136">Then, to specify the subscription to use, type:</span></span>

```
az account set --subscription <subscription name or ID>
```

<span data-ttu-id="0fba1-137">Další informace o konfiguraci rozhraní příkazového řádku pro různé platformy Azure najdete v tématu [nainstalovat rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0fba1-137">For more information about configuring Azure Cross-Platform Command-Line Interface, see [Install Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="create-a-new-resource-group"></a><span data-ttu-id="0fba1-138">Vytvoření nové skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="0fba1-138">Create a new resource group</span></span>
<span data-ttu-id="0fba1-139">Pokud používáte Azure Resource Manager, všechny související prostředky vytváří v uvnitř skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="0fba1-139">When using Azure Resource Manager, all related resources are created inside a resource group.</span></span> <span data-ttu-id="0fba1-140">Pro účely tohoto kurzu vytvoříme novou skupinu prostředků, ContosoResourceGroup'.</span><span class="sxs-lookup"><span data-stu-id="0fba1-140">We will create a new resource group 'ContosoResourceGroup' for this tutorial.</span></span>

```
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

<span data-ttu-id="0fba1-141">První parametr je název skupiny prostředků a druhý parametr je umístění.</span><span class="sxs-lookup"><span data-stu-id="0fba1-141">The first parameter is resource group name and the second parameter is the location.</span></span> <span data-ttu-id="0fba1-142">Pro umístění, použijte příkaz `az account list-locations` zjišťují, jak zadat alternativní umístění na v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="0fba1-142">For location, use the command `az account list-locations` to identify how to specify an alternative location to the one in this example.</span></span> <span data-ttu-id="0fba1-143">Pokud potřebujete více informací, zadejte: `az account list-locations -h`.</span><span class="sxs-lookup"><span data-stu-id="0fba1-143">If you need more information, type: `az account list-locations -h`.</span></span>

## <a name="register-the-key-vault-resource-provider"></a><span data-ttu-id="0fba1-144">Registrace poskytovatele prostředků Key Vault</span><span class="sxs-lookup"><span data-stu-id="0fba1-144">Register the Key Vault resource provider</span></span>
<span data-ttu-id="0fba1-145">Ujistěte se, že poskytovatele prostředků Key Vault je zaregistrován v rámci vašeho předplatného:</span><span class="sxs-lookup"><span data-stu-id="0fba1-145">Make sure that Key Vault resource provider is registered in your subscription:</span></span>

```
az provider register -n Microsoft.KeyVault
```

<span data-ttu-id="0fba1-146">To jenom je nutné provést jednou za předplatné.</span><span class="sxs-lookup"><span data-stu-id="0fba1-146">This only needs to be done once per subscription.</span></span>

## <a name="create-a-key-vault"></a><span data-ttu-id="0fba1-147">Vytvořte trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="0fba1-147">Create a key vault</span></span>
<span data-ttu-id="0fba1-148">Použití `az keyvault create` příkaz pro vytvoření trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="0fba1-148">Use the `az keyvault create` command to create a key vault.</span></span> <span data-ttu-id="0fba1-149">Tento skript má tři povinné parametry: název skupiny prostředků, název trezoru klíčů a zeměpisné umístění.</span><span class="sxs-lookup"><span data-stu-id="0fba1-149">This script has three mandatory parameters: a resource group name, a key vault name, and the geographic location.</span></span>

<span data-ttu-id="0fba1-150">Například pokud použijete název trezoru ContosoKeyVault, název skupiny prostředků ContosoResourceGroup a umístění východní Asie, zadejte:</span><span class="sxs-lookup"><span data-stu-id="0fba1-150">For example, if you use the vault name of ContosoKeyVault, the resource group name of ContosoResourceGroup, and the location of East Asia, type:</span></span>
```
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

<span data-ttu-id="0fba1-151">Výstup tohoto příkazu zobrazuje vlastnosti trezoru klíčů, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0fba1-151">The output of this command shows properties of the key vault that you've just created.</span></span> <span data-ttu-id="0fba1-152">Dvě nejdůležitější vlastnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="0fba1-152">The two most important properties are:</span></span>

* <span data-ttu-id="0fba1-153">**název**: V tomto příkladu to je ContosoKeyVault.</span><span class="sxs-lookup"><span data-stu-id="0fba1-153">**name**: In the example this is ContosoKeyVault.</span></span> <span data-ttu-id="0fba1-154">Tento název pro jiné příkazy Key Vault budete používat.</span><span class="sxs-lookup"><span data-stu-id="0fba1-154">You will use this name for other Key Vault commands.</span></span>
* <span data-ttu-id="0fba1-155">**vaultUri**: V tomto příkladu to je https://contosokeyvault.vault.azure.net.</span><span class="sxs-lookup"><span data-stu-id="0fba1-155">**vaultUri**: In the example this is https://contosokeyvault.vault.azure.net.</span></span> <span data-ttu-id="0fba1-156">Aplikace, které používají váš trezor prostřednictvím REST API musí používat tento identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="0fba1-156">Applications that use your vault through its REST API must use this URI.</span></span>

<span data-ttu-id="0fba1-157">Váš účet Azure je nyní oprávněn provádět nad tímto trezorem klíčů všechny operace.</span><span class="sxs-lookup"><span data-stu-id="0fba1-157">Your Azure account is now authorized to perform any operations on this key vault.</span></span> <span data-ttu-id="0fba1-158">Zatím k tomu není oprávněn nikdo jiný.</span><span class="sxs-lookup"><span data-stu-id="0fba1-158">As yet, nobody else is.</span></span>

## <a name="add-a-key-or-secret-to-the-key-vault"></a><span data-ttu-id="0fba1-159">Přidejte k trezoru klíč nebo tajný klíč</span><span class="sxs-lookup"><span data-stu-id="0fba1-159">Add a key or secret to the key vault</span></span>
<span data-ttu-id="0fba1-160">Pokud chcete Azure Key Vault vytvořila softwarově chráněný klíč pro vás, použijte `az key create` příkaz a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0fba1-160">If you want Azure Key Vault to create a software-protected key for you, use the `az key create` command, and type the following:</span></span>
```
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```
<span data-ttu-id="0fba1-161">Nicméně pokud máte existující klíč v soubor .pem uložený jako místního souboru do souboru s názvem softkey.pem, který chcete nahrát do Azure Key Vault, zadejte následující příkaz pro import klíče z. Soubor PEM, který chrání klíč softwarem ve službě Key Vault:</span><span class="sxs-lookup"><span data-stu-id="0fba1-161">However, if you have an existing key in a .pem file saved as local file in a file named softkey.pem that you want to upload to Azure Key Vault, type the following to import the key from the .PEM file, which protects the key by software in the Key Vault service:</span></span>
```
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'PaSSWORD' --protection software
```
<span data-ttu-id="0fba1-162">Nyní můžete odkazovat klíč, který jste vytvořili nebo nahrán do Azure Key Vault, pomocí jeho identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="0fba1-162">You can now reference the key that you created or uploaded to Azure Key Vault, by using its URI.</span></span> <span data-ttu-id="0fba1-163">Použít **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** vždy získáte aktuální verzi a používat **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** Chcete-li získat konkrétní verzi.</span><span class="sxs-lookup"><span data-stu-id="0fba1-163">Use  **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** to always get the current version, and use **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** to get this specific version.</span></span>

<span data-ttu-id="0fba1-164">Chcete-li přidat tajný klíč k úložišti, který je hesla s názvem SQLPassword a hodnotou z Pa$ $w0rd do Azure Key Vault, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0fba1-164">To add a secret to the vault, which is a password named SQLPassword and that has the value of Pa$$w0rd to Azure Key Vault, type the following:</span></span>
```
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```
<span data-ttu-id="0fba1-165">Nyní na toto heslo, které jste přidali do Azure Key Vault, můžete odkazovat pomocí jeho identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="0fba1-165">You can now reference this password that you added to Azure Key Vault, by using its URI.</span></span> <span data-ttu-id="0fba1-166">Pomocí **https://ContosoVault.vault.azure.net/secrets/SQLPassword** vždy získáte aktuální verzi. Chcete-li získat konkrétní verzi, použijte **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**.</span><span class="sxs-lookup"><span data-stu-id="0fba1-166">Use **https://ContosoVault.vault.azure.net/secrets/SQLPassword** to always get the current version, and use **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** to get this specific version.</span></span>

<span data-ttu-id="0fba1-167">Podívejme se na klíč nebo tajný klíč, který jste právě vytvořili:</span><span class="sxs-lookup"><span data-stu-id="0fba1-167">Let's view the key or secret that you just created:</span></span>

* <span data-ttu-id="0fba1-168">Pokud chcete zobrazit klíč, zadejte `az keyvault key list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="0fba1-168">To view your key, type: `az keyvault key list --vault-name 'ContosoKeyVault'`</span></span>
* <span data-ttu-id="0fba1-169">Pokud chcete zobrazit tajný klíč, zadejte `az keyvault secret list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="0fba1-169">To view your secret, type: `az keyvault secret list --vault-name 'ContosoKeyVault'`</span></span>

## <a name="register-an-application-with-azure-active-directory"></a><span data-ttu-id="0fba1-170">Registrujte aplikaci s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0fba1-170">Register an application with Azure Active Directory</span></span>
<span data-ttu-id="0fba1-171">Tento krok obvykle provádí vývojář na samostatném počítači.</span><span class="sxs-lookup"><span data-stu-id="0fba1-171">This step would usually be done by a developer, on a separate computer.</span></span> <span data-ttu-id="0fba1-172">Není specifické pro Azure Key Vault, ale je zahrnuté pro úplnost.</span><span class="sxs-lookup"><span data-stu-id="0fba1-172">It is not specific to Azure Key Vault but is included here, for completeness.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0fba1-173">Pro dokončení kurzu musí být váš účet, trezor i aplikace, kterou budete v tomto kroku registrovat, ve stejném adresáři Azure.</span><span class="sxs-lookup"><span data-stu-id="0fba1-173">To complete the tutorial, your account, the vault, and the application that you will register in this step must all be in the same Azure directory.</span></span>
>
>

<span data-ttu-id="0fba1-174">Aplikace, které používají trezor klíčů, se musí ověřit pomocí tokenu z Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0fba1-174">Applications that use a key vault must authenticate by using a token from Azure Active Directory.</span></span> <span data-ttu-id="0fba1-175">K tomu je potřeba, aby majitel aplikace nejprve zaregistroval aplikaci ve vlastním Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0fba1-175">To do this, the owner of the application must first register the application in their Azure Active Directory.</span></span> <span data-ttu-id="0fba1-176">Na konci registrace obdrží majitel aplikace následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="0fba1-176">At the end of registration, the application owner gets the following values:</span></span>

* <span data-ttu-id="0fba1-177">**ID aplikace** (také označované jako ID klienta) a **ověřovací klíč** (také označovaný jako sdílený tajný klíč).</span><span class="sxs-lookup"><span data-stu-id="0fba1-177">An **Application ID** (also known as a Client ID) and **authentication key** (also known as the shared secret).</span></span> <span data-ttu-id="0fba1-178">Aplikace musí představovat obě tyto hodnoty do Azure Active Directory, chcete-li získat token.</span><span class="sxs-lookup"><span data-stu-id="0fba1-178">The application must present both of these values to Azure Active Directory, to get a token.</span></span> <span data-ttu-id="0fba1-179">Záleží na aplikaci, jak je k tomu nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="0fba1-179">How the application is configured to do this depends on the application.</span></span> <span data-ttu-id="0fba1-180">Pro ukázkovou aplikaci Key Vault nastavuje majitel tyto hodnoty v souboru app.config.</span><span class="sxs-lookup"><span data-stu-id="0fba1-180">For the Key Vault sample application, the application owner sets these values in the app.config file.</span></span>

<span data-ttu-id="0fba1-181">Chcete-li zaregistrovat aplikaci v Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="0fba1-181">To register the application in Azure Active Directory:</span></span>

1. <span data-ttu-id="0fba1-182">Přihlaste se k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0fba1-182">Sign in to the Azure portal.</span></span>
2. <span data-ttu-id="0fba1-183">Na levé straně klikněte na tlačítko **Azure Active Directory**a poté vyberte adresář, ve kterém bude registrace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0fba1-183">On the left, click **Azure Active Directory**, and then select the directory in which you will register your application.</span></span> <br> <br> 

> [!Note] 
> <span data-ttu-id="0fba1-184">Musíte vybrat stejný adresář, který obsahuje předplatné Azure, který jste použili pro vytvoření trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="0fba1-184">You must select the same directory that contains the Azure subscription with which you created your key vault.</span></span> <span data-ttu-id="0fba1-185">Pokud si nejste jisti, který adresář to je, klikněte na **Nastavení**, určete předplatné, které jste použili při vytvoření trezoru klíčů, a poznamenejte si název adresáře v posledním sloupci.</span><span class="sxs-lookup"><span data-stu-id="0fba1-185">If you do not know which directory this is, click **Settings**, identify the subscription with which you created your key vault, and note the name of the directory displayed in the last column.</span></span>

3. <span data-ttu-id="0fba1-186">Klikněte na **Aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0fba1-186">Click **APPLICATIONS**.</span></span> <span data-ttu-id="0fba1-187">Pokud do vašeho adresáře nebyly přidané žádné aplikace, tato stránka se zobrazí pouze **přidat aplikaci** odkaz.</span><span class="sxs-lookup"><span data-stu-id="0fba1-187">If no apps have been added to your directory, this page will show only the **Add an App** link.</span></span> <span data-ttu-id="0fba1-188">Klikněte na odkaz, nebo také můžete klepnout **přidat** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="0fba1-188">Click the link, or alternatively, you can click the **ADD** on the command bar.</span></span>
4. <span data-ttu-id="0fba1-189">V průvodci **Přidat aplikaci** klikněte na stránce **Co si přejete udělat?** na **Přidat aplikaci, kterou vyvíjí moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="0fba1-189">In the **ADD APPLICATION** wizard, on the **What do you want to do?** page, click **Add an application my organization is developing**.</span></span>
5. <span data-ttu-id="0fba1-190">Na **Řekněte nám o své aplikaci** , zadejte název pro vaši aplikaci a vyberte **webové aplikace nebo webové rozhraní API** (výchozí).</span><span class="sxs-lookup"><span data-stu-id="0fba1-190">On the **Tell us about your application** page, specify a name for your application and select **WEB APPLICATION AND/OR WEB API** (the default).</span></span> <span data-ttu-id="0fba1-191">Klikněte na ikonu Další.</span><span class="sxs-lookup"><span data-stu-id="0fba1-191">Click the Next icon.</span></span>
6. <span data-ttu-id="0fba1-192">Na stránce **Vlastnosti aplikace** zadejte **URL pro přihlašování** a **Identifikátor ID URI Aplikace** pro svoji webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0fba1-192">On the **App properties** page, specify the **SIGN-ON URL** and **APP ID URI** for your web application.</span></span> <span data-ttu-id="0fba1-193">Pokud vaše aplikace tyto hodnoty neobsahuje, v tomto kroku si je můžete vymyslet (například můžete do obou polí zadat http://test1.contoso.com).</span><span class="sxs-lookup"><span data-stu-id="0fba1-193">If your application does not have these values, you can make them up for this step (for example, you could specify http://test1.contoso.com for both boxes).</span></span> <span data-ttu-id="0fba1-194">Není důležité, zda tyto stránky existují, důležité je, aby identifikátor ID URI aplikace byl pro každou aplikaci ve vašem adresáři jiný.</span><span class="sxs-lookup"><span data-stu-id="0fba1-194">It does not matter if these sites exist; what is important is that the app ID URI for each application is different for every application in your directory.</span></span> <span data-ttu-id="0fba1-195">Adresář tento řetězec používá k identifikaci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0fba1-195">The directory uses this string to identify your app.</span></span>
7. <span data-ttu-id="0fba1-196">Klikněte na ikonu dokončení uložte provedené změny v průvodci.</span><span class="sxs-lookup"><span data-stu-id="0fba1-196">Click the Complete icon to save your changes in the wizard.</span></span>
8. <span data-ttu-id="0fba1-197">Na stránce Rychlý Start klikněte na **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="0fba1-197">On the Quick Start page, click **CONFIGURE**.</span></span>
9. <span data-ttu-id="0fba1-198">Posuňte se k části **klíče**, vyberte dobu trvání a poté klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0fba1-198">Scroll to the **keys** section, select the duration, and then click **SAVE**.</span></span> <span data-ttu-id="0fba1-199">Stránka se obnoví a zobrazí hodnotu klíče.</span><span class="sxs-lookup"><span data-stu-id="0fba1-199">The page refreshes and now shows a key value.</span></span> <span data-ttu-id="0fba1-200">Vaši aplikaci je nutné nakonfigurovat s touto hodnotou klíče a s hodnotou **ID klienta**.</span><span class="sxs-lookup"><span data-stu-id="0fba1-200">You must configure your application with this key value and the **CLIENT ID** value.</span></span> <span data-ttu-id="0fba1-201">(Pokyny k této konfiguraci se budou lišit podle aplikace.)</span><span class="sxs-lookup"><span data-stu-id="0fba1-201">(Instructions for this configuration will be application-specific.)</span></span>
10. <span data-ttu-id="0fba1-202">Poznamenejte si hodnotu ID klienta z této stránky, v dalším kroku ji použijete pro nastavení oprávnění pro váš trezoru.</span><span class="sxs-lookup"><span data-stu-id="0fba1-202">Copy the client ID value from this page, which you will use in the next step to set permissions on your vault.</span></span>

## <a name="authorize-the-application-to-use-the-key-or-secret"></a><span data-ttu-id="0fba1-203">Autorizujte aplikaci pro použití klíče nebo tajného klíče</span><span class="sxs-lookup"><span data-stu-id="0fba1-203">Authorize the application to use the key or secret</span></span>
<span data-ttu-id="0fba1-204">Chcete-li autorizovat aplikaci pro přístup ke klíči nebo tajný klíč v trezoru, použijte `az keyvault set-policy` příkaz.</span><span class="sxs-lookup"><span data-stu-id="0fba1-204">To authorize the application to access the key or secret in the vault, use the `az keyvault set-policy` command.</span></span>

<span data-ttu-id="0fba1-205">Například pokud je název vaší trezoru ContosoKeyVault a aplikace, které chcete autorizovat má hodnotu 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed ID klienta a chcete aplikaci autorizovat pro dešifrování a podepisování pomocí klíčů ve vašem trezoru, spusťte následující:</span><span class="sxs-lookup"><span data-stu-id="0fba1-205">For example, if your vault name is ContosoKeyVault and the application you want to authorize has a client ID of 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, and you want to authorize the application to decrypt and sign with keys in your vault, then run the following:</span></span>
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

<span data-ttu-id="0fba1-206">Chcete-li tu samou aplikaci autorizovat pro čtení tajných klíčů v trezoru, spusťte následující:</span><span class="sxs-lookup"><span data-stu-id="0fba1-206">If you want to authorize that same application to read secrets in your vault, run the following:</span></span>
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```
## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a><span data-ttu-id="0fba1-207">Pokud chcete použít modul hardwarového zabezpečení (HSM)</span><span class="sxs-lookup"><span data-stu-id="0fba1-207">If you want to use a hardware security module (HSM)</span></span>
<span data-ttu-id="0fba1-208">Pro lepší kontrolu můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM), které nikdy neopustí hranice HSM.</span><span class="sxs-lookup"><span data-stu-id="0fba1-208">For added assurance, you can import or generate keys in hardware security modules (HSMs) that never leave the HSM boundary.</span></span> <span data-ttu-id="0fba1-209">Moduly hardwarového zabezpečení jsou ověřené podle standardu FIPS 140-2 Level 2.</span><span class="sxs-lookup"><span data-stu-id="0fba1-209">The HSMs are FIPS 140-2 Level 2 validated.</span></span> <span data-ttu-id="0fba1-210">Pokud se vás tento požadavek netýká, přeskočte tuto část a přejděte k části [Odstranění trezoru klíčů, přidružených klíčů a tajných klíčů](#delete-the-key-vault-and-associated-keys-and-secrets).</span><span class="sxs-lookup"><span data-stu-id="0fba1-210">If this requirement doesn't apply to you, skip this section and go to [Delete the key vault and associated keys and secrets](#delete-the-key-vault-and-associated-keys-and-secrets).</span></span>

<span data-ttu-id="0fba1-211">Pro vytvoření těchto klíčů chráněných pomocí HSM, musíte mít předplatné trezoru s podporou klíčů chráněných pomocí HSM.</span><span class="sxs-lookup"><span data-stu-id="0fba1-211">To create these HSM-protected keys, you must have a vault subscription that supports HSM-protected keys.</span></span>

<span data-ttu-id="0fba1-212">Když vytvoříte keyvault, přidejte parametr 'sku':</span><span class="sxs-lookup"><span data-stu-id="0fba1-212">When you create the keyvault, add the 'sku' parameter:</span></span>

```
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```
<span data-ttu-id="0fba1-213">Do tohoto trezoru můžete přidat klíče chráněné softwarem (jak jsme ukázali výše) a klíče chráněné pomocí HSM.</span><span class="sxs-lookup"><span data-stu-id="0fba1-213">You can add software-protected keys (as shown earlier) and HSM-protected keys to this vault.</span></span> <span data-ttu-id="0fba1-214">Chcete-li vytvořit klíč chráněný HSM, nastavte parametr cílové na "HSM":</span><span class="sxs-lookup"><span data-stu-id="0fba1-214">To create an HSM-protected key, set the Destination parameter to 'HSM':</span></span>

```
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

<span data-ttu-id="0fba1-215">Můžete následující příkaz pro import klíče ze souboru .pem ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="0fba1-215">You can use the following command to import a key from a .pem file on your computer.</span></span> <span data-ttu-id="0fba1-216">Tento příkaz importuje klíč do HSM ve službě Key Vault:</span><span class="sxs-lookup"><span data-stu-id="0fba1-216">This command imports the key into HSMs in the Key Vault service:</span></span>

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```
<span data-ttu-id="0fba1-217">Další příkaz importuje balíček „přineste si vlastní klíč“ (BYOK).</span><span class="sxs-lookup"><span data-stu-id="0fba1-217">The next command imports a “bring your own key" (BYOK) package.</span></span> <span data-ttu-id="0fba1-218">To vám umožní vygenerovat klíč v místním HSM a jeho přenesení do HSM ve službě Key Vault, aniž by klíč opustil hranice HSM.</span><span class="sxs-lookup"><span data-stu-id="0fba1-218">This lets you generate your key in your local HSM, and transfer it to HSMs in the Key Vault service, without the key leaving the HSM boundary:</span></span>

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```
<span data-ttu-id="0fba1-219">Podrobnější pokyny o tom, jak generovat tento balíček BYOK naleznete v tématu [použití HSM-Protected klíčů s Azure Key Vault](key-vault-hsm-protected-keys.md).</span><span class="sxs-lookup"><span data-stu-id="0fba1-219">For more detailed instructions about how to generate this BYOK package, see [How to use HSM-Protected Keys with Azure Key Vault](key-vault-hsm-protected-keys.md).</span></span>

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a><span data-ttu-id="0fba1-220">Odstranění trezoru klíčů, přidružených klíčů a tajných klíčů</span><span class="sxs-lookup"><span data-stu-id="0fba1-220">Delete the key vault and associated keys and secrets</span></span>
<span data-ttu-id="0fba1-221">Pokud již nepotřebujete trezor klíčů a klíč nebo tajný klíč, který obsahuje, můžete trezor klíčů odstranit pomocí `az keyvault delete` příkaz:</span><span class="sxs-lookup"><span data-stu-id="0fba1-221">If you no longer need the key vault and the key or secret that it contains, you can delete the key vault by using the `az keyvault delete` command:</span></span>

```
az keyvault delete --name 'ContosoKeyVault'
```

<span data-ttu-id="0fba1-222">Nebo můžete odstranit celou skupinu prostředků Azure, která zahrnuje trezor klíčů a všechny další prostředky, které jste do skupiny zahrnuli.</span><span class="sxs-lookup"><span data-stu-id="0fba1-222">Or, you can delete an entire Azure resource group, which includes the key vault and any other resources that you included in that group:</span></span>

```
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a><span data-ttu-id="0fba1-223">Ostatní příkazy rozhraní příkazového řádku Azure a platformy</span><span class="sxs-lookup"><span data-stu-id="0fba1-223">Other Azure Cross-Platform Command-line Interface Commands</span></span>
<span data-ttu-id="0fba1-224">Další příkazy, že může využít ke správě Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="0fba1-224">Other commands that you might useful for managing Azure Key Vault.</span></span>

<span data-ttu-id="0fba1-225">Tento příkaz vypíše tabulkové zobrazení všech klíčů a vybraných vlastností:</span><span class="sxs-lookup"><span data-stu-id="0fba1-225">This command lists a tabular display of all keys and selected properties:</span></span>

<span data-ttu-id="0fba1-226">AZ seznam klíčů keyvault – název trezoru ContosoKeyVault</span><span class="sxs-lookup"><span data-stu-id="0fba1-226">az keyvault key list --vault-name 'ContosoKeyVault'</span></span>

<span data-ttu-id="0fba1-227">Tento příkaz zobrazí úplný seznam vlastností pro zadaný klíč:</span><span class="sxs-lookup"><span data-stu-id="0fba1-227">This command displays a full list of properties for the specified key:</span></span>

<span data-ttu-id="0fba1-228">Zobrazit klíč keyvault az--název trezoru ContosoKeyVault – název 'ContosoFirstKey.</span><span class="sxs-lookup"><span data-stu-id="0fba1-228">az keyvault key show --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'</span></span>

<span data-ttu-id="0fba1-229">Tento příkaz vypíše tabulkové zobrazení všech názvů tajných klíčů a vybraných vlastností:</span><span class="sxs-lookup"><span data-stu-id="0fba1-229">This command lists a tabular display of all secret names and selected properties:</span></span>

<span data-ttu-id="0fba1-230">AZ keyvault tajný seznamu – název trezoru ContosoKeyVault</span><span class="sxs-lookup"><span data-stu-id="0fba1-230">az keyvault secret list --vault-name 'ContosoKeyVault'</span></span>

<span data-ttu-id="0fba1-231">Tady je příklad odebrání určitého klíče:</span><span class="sxs-lookup"><span data-stu-id="0fba1-231">Here's an example of how to remove a specific key:</span></span>

<span data-ttu-id="0fba1-232">odstranění klíče keyvault az--název trezoru ContosoKeyVault – název 'ContosoFirstKey.</span><span class="sxs-lookup"><span data-stu-id="0fba1-232">az keyvault key delete --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'</span></span>

<span data-ttu-id="0fba1-233">Tady je příklad odebrání určitého tajného klíče:</span><span class="sxs-lookup"><span data-stu-id="0fba1-233">Here's an example of how to remove a specific secret:</span></span>

<span data-ttu-id="0fba1-234">Odstranit tajný klíč keyvault az--název trezoru ContosoKeyVault – název 'SQLPassword.</span><span class="sxs-lookup"><span data-stu-id="0fba1-234">az keyvault secret delete --vault-name 'ContosoKeyVault' --name 'SQLPassword'</span></span>


## <a name="next-steps"></a><span data-ttu-id="0fba1-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0fba1-235">Next steps</span></span>
<span data-ttu-id="0fba1-236">Úplný referenční rozhraní příkazového řádku Azure pro příkazy trezoru klíčů, najdete v části [odkaz klíč trezoru rozhraní příkazového řádku](/cli/azure/keyvault)</span><span class="sxs-lookup"><span data-stu-id="0fba1-236">For complete Azure CLI reference for key vault commands, see [Key Vault CLI reference](/cli/azure/keyvault)</span></span>

<span data-ttu-id="0fba1-237">Programátorské reference najdete v [příručce pro vývojáře Azure Key Vault](key-vault-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="0fba1-237">For programming references, see [the Azure Key Vault developer's guide](key-vault-developers-guide.md).</span></span>
