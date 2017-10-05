---
title: "Spravovat pomocí rozhraní příkazového řádku Azure Key Vault | Microsoft Docs"
description: "Tento kurz použijte k automatizaci běžných úkolů v Key Vault pomocí rozhraní příkazového řádku"
services: key-vault
documentationcenter: 
author: BrucePerlerMS
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 66be6e44-684a-411b-802e-884628458ae7
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: bruceper
ms.openlocfilehash: c2565a742ce4f6ab5f7639a54c4a475f00cbc260
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-key-vault-using-cli"></a><span data-ttu-id="0c899-103">Spravovat Key Vault pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="0c899-103">Manage Key Vault using CLI</span></span>

<span data-ttu-id="0c899-104">Azure Key Vault je dostupný ve většině oblastí.</span><span class="sxs-lookup"><span data-stu-id="0c899-104">Azure Key Vault is available in most regions.</span></span> <span data-ttu-id="0c899-105">Další informace najdete na [stránce s cenami Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="0c899-105">For more information, see the [Key Vault pricing page](https://azure.microsoft.com/pricing/details/key-vault/).</span></span>

## <a name="introduction"></a><span data-ttu-id="0c899-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="0c899-106">Introduction</span></span>

<span data-ttu-id="0c899-107">Tento kurz vám pomůže začít s Azure Key Vault a ukáže vám, jak v Azure vytvořit zesílený kontejner (trezor), a jak ukládat a spravovat kryptografické klíče a tajné klíče v Azure.</span><span class="sxs-lookup"><span data-stu-id="0c899-107">Use this tutorial to help you get started with Azure Key Vault to create a hardened container (a vault) in Azure, to store and manage cryptographic keys and secrets in Azure.</span></span> <span data-ttu-id="0c899-108">Provede vás provede procesem vytvoření trezoru, který obsahuje klíč nebo heslo, které pak můžete použít s aplikací Azure pomocí rozhraní příkazového řádku pro různé platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="0c899-108">It walks you through the process of using Azure Cross-Platform Command-Line Interface to create a vault that contains a key or password that you can then use with an Azure application.</span></span> <span data-ttu-id="0c899-109">Poté vám ukáže, jak aplikaci můžete potom použít tento klíč nebo heslo.</span><span class="sxs-lookup"><span data-stu-id="0c899-109">It then shows you how an application can then use that key or password.</span></span>

<span data-ttu-id="0c899-110">**Odhadovaný čas dokončení:** 20 minut</span><span class="sxs-lookup"><span data-stu-id="0c899-110">**Estimated time to complete:** 20 minutes</span></span>

> [!NOTE]
> <span data-ttu-id="0c899-111">Tento kurz neobsahuje pokyny o tom, jak napsat aplikaci Azure, že jeden z kroků obsahuje, který ukazuje, jak k autorizaci aplikace pro použití klíče nebo tajného klíče v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="0c899-111">This tutorial does not include instructions on how to write the Azure application that one of the steps includes, which shows how to authorize an application to use a key or secret in the key vault.</span></span>
> 
> <span data-ttu-id="0c899-112">V současné době nelze Azure Key Vault konfigurovat na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0c899-112">Currently, you cannot configure Azure Key Vault in the Azure portal.</span></span> <span data-ttu-id="0c899-113">Místo toho použijte tyto pokyny Multiplatformního rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0c899-113">Instead, use these Cross-Platform Command-Line Interface  instructions.</span></span> <span data-ttu-id="0c899-114">Nebo Azure PowerShell pokyny najdete v tématu [tomto ekvivalentním kurzu](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0c899-114">Or, for Azure PowerShell instructions, see [this equivalent tutorial](key-vault-get-started.md).</span></span>
> 
> 

<span data-ttu-id="0c899-115">Souhrnné informace o Azure Key Vault naleznete v tématu [Co je Azure Key Vault?](key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0c899-115">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0c899-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0c899-116">Prerequisites</span></span>

<span data-ttu-id="0c899-117">K dokončení tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="0c899-117">To complete this tutorial, you must have the following:</span></span>

* <span data-ttu-id="0c899-118">Předplatné Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="0c899-118">A subscription to Microsoft Azure.</span></span> <span data-ttu-id="0c899-119">Pokud jeden nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial).</span><span class="sxs-lookup"><span data-stu-id="0c899-119">If you do not have one, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial).</span></span>
* <span data-ttu-id="0c899-120">Rozhraní příkazového řádku verzi 0.9.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0c899-120">Command-Line Interface version 0.9.1 or later.</span></span> <span data-ttu-id="0c899-121">Nainstalujte nejnovější verzi a připojení k předplatnému Azure najdete v tématu [instalace a konfigurace rozhraní příkazového řádku pro různé platformy Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="0c899-121">To install the latest version and connect to your Azure subscription, see [Install and Configure the Azure Cross-Platform Command-Line Interface](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="0c899-122">Aplikaci, kterou nakonfigurujete pro použití klíče nebo hesla, které v tomto kurzu vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="0c899-122">An application that will be configured to use the key or password that you create in this tutorial.</span></span> <span data-ttu-id="0c899-123">Vzorová aplikace je k dispozici ve službě [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span><span class="sxs-lookup"><span data-stu-id="0c899-123">A sample application is available from the [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span></span> <span data-ttu-id="0c899-124">Pokyny naleznete v souvisejícím souboru Readme.</span><span class="sxs-lookup"><span data-stu-id="0c899-124">For instructions, see the accompanying Readme file.</span></span>

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a><span data-ttu-id="0c899-125">Získání nápovědy pomocí rozhraní příkazového řádku Azure a platformy</span><span class="sxs-lookup"><span data-stu-id="0c899-125">Getting help with Azure Cross-Platform Command-Line Interface</span></span>

<span data-ttu-id="0c899-126">V tomto kurzu se předpokládá, že jste obeznámeni s rozhraní příkazového řádku (Bash, terminálu, příkazového řádku)</span><span class="sxs-lookup"><span data-stu-id="0c899-126">This tutorial assumes that you are familiar with the command-line interface (Bash, Terminal, Command prompt)</span></span>

<span data-ttu-id="0c899-127">--Parametr nápovědy nebo -h lze zobrazit nápovědu pro určité příkazy.</span><span class="sxs-lookup"><span data-stu-id="0c899-127">The --help or -h parameter can be used to view help for specific commands.</span></span> <span data-ttu-id="0c899-128">Alternativně azure help [příkaz] [možnosti] formátu lze použít také k vrácení stejné informace.</span><span class="sxs-lookup"><span data-stu-id="0c899-128">Alternately, The azure help [command] [options] format can also be used to return the same information.</span></span> <span data-ttu-id="0c899-129">Například následující příkazy, které se všechny vrátí stejné informace:</span><span class="sxs-lookup"><span data-stu-id="0c899-129">For example, the following commands all return the same information:</span></span>

    azure account set --help

    azure account set -h

    azure help account set

<span data-ttu-id="0c899-130">Pokud máte pochybnosti o parametry vyžadované příkaz, v nápovědě k použití – Nápověda, -h nebo azure nápovědy [příkaz].</span><span class="sxs-lookup"><span data-stu-id="0c899-130">When in doubt about the parameters needed by a command, refer to help using --help, -h or azure help [command].</span></span>

<span data-ttu-id="0c899-131">Také si můžete přečíst následující kurzy a seznamte se s Azure Resource Manager v rozhraní příkazového řádku Azure napříč platformami:</span><span class="sxs-lookup"><span data-stu-id="0c899-131">You can also read the following tutorials to get familiar with Azure Resource Manager in Azure Cross-Platform Command-Line Interface:</span></span>

* [<span data-ttu-id="0c899-132">Postup instalace a konfigurace rozhraní příkazového řádku Azure a platformy</span><span class="sxs-lookup"><span data-stu-id="0c899-132">How to install and configure Azure Cross-Platform Command Line Interface</span></span>](../cli-install-nodejs.md)
* [<span data-ttu-id="0c899-133">Pomocí rozhraní příkazového řádku a platformy Azure s Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0c899-133">Using Azure Cross-Platform Command-Line Interface with Azure Resource Manager</span></span>](../xplat-cli-azure-resource-manager.md)

## <a name="connect-to-your-subscriptions"></a><span data-ttu-id="0c899-134">Připojte se ke svým předplatným</span><span class="sxs-lookup"><span data-stu-id="0c899-134">Connect to your subscriptions</span></span>

<span data-ttu-id="0c899-135">K přihlášení pomocí účtu organizace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0c899-135">To log in using an organizational account, use the following command:</span></span>

    azure login -u username -p password

<span data-ttu-id="0c899-136">nebo pokud se chcete přihlásit interaktivně zadáním</span><span class="sxs-lookup"><span data-stu-id="0c899-136">or if you want to log in by typing interactively</span></span>

    azure login

> [!NOTE]
> <span data-ttu-id="0c899-137">Přihlášení metodu lze použít pouze s účtem organizace.</span><span class="sxs-lookup"><span data-stu-id="0c899-137">The login method only works with organizational account.</span></span> <span data-ttu-id="0c899-138">Účet organizace je uživatel, který je spravovaná vaší organizací a definované v klienta Azure Active Directory vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="0c899-138">An organizational account is a user that is managed by your organization, and defined in your organization's Azure Active Directory tenant.</span></span>
> 
> 

<span data-ttu-id="0c899-139">Pokud nyní není k dispozici účet organizace a používáte účet Microsoft k přihlášení k předplatnému Azure, můžete snadno vytvořit pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="0c899-139">If you do not currently have an organizational account, and are using a Microsoft account to log in to your Azure subscription, you can easily create one using the following steps.</span></span>

1. <span data-ttu-id="0c899-140">Přihlášení k přihlášení k [portálu pro správu Azure](https://manage.windowsazure.com/)a klikněte na Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0c899-140">Login to the Login to the [Azure Management Portal](https://manage.windowsazure.com/), and click on Active Directory.</span></span>
2. <span data-ttu-id="0c899-141">Pokud žádný adresář existuje, vyberte možnost vytvořit adresáře a zadejte požadované informace.</span><span class="sxs-lookup"><span data-stu-id="0c899-141">If no directory exists, select Create your directory and provide the requested information.</span></span>
3. <span data-ttu-id="0c899-142">Vyberte adresář a přidání nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="0c899-142">Select your directory and add a new user.</span></span> <span data-ttu-id="0c899-143">Tento nový uživatel je účet organizace.</span><span class="sxs-lookup"><span data-stu-id="0c899-143">This new user is an organizational account.</span></span> <span data-ttu-id="0c899-144">Při vytváření uživatele bude nutné zadat obě e-mailovou adresu pro uživatele a dočasné heslo.</span><span class="sxs-lookup"><span data-stu-id="0c899-144">During the creation of the user, you will be supplied with both an e-mail address for the user and a temporary password.</span></span> <span data-ttu-id="0c899-145">Tyto informace uložte, protože se používá v jiném kroku.</span><span class="sxs-lookup"><span data-stu-id="0c899-145">Save this information as it is used in another step.</span></span>
4. <span data-ttu-id="0c899-146">Z portálu vyberte nastavení a pak vyberte správci.</span><span class="sxs-lookup"><span data-stu-id="0c899-146">From the portal, select Settings and then select Administrators.</span></span> <span data-ttu-id="0c899-147">Vyberte Přidat a přidat nové uživatele jako spolusprávce.</span><span class="sxs-lookup"><span data-stu-id="0c899-147">Select Add, and add the new user as a co-administrator.</span></span> <span data-ttu-id="0c899-148">To umožňuje účet organizace ke správě vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="0c899-148">This allows the organizational account to manage your Azure subscription.</span></span>
5. <span data-ttu-id="0c899-149">Nakonec odhlaste se z portálu Azure a potom znovu přihlásit pomocí nového účtu organizace.</span><span class="sxs-lookup"><span data-stu-id="0c899-149">Finally, log out of the Azure portal and then log back in using the new organizational account.</span></span> <span data-ttu-id="0c899-150">Pokud je to první čas přihlášení pomocí tohoto účtu, vyzve ke změně hesla.</span><span class="sxs-lookup"><span data-stu-id="0c899-150">If this is the first time logging in with this account, you will be prompted to change the password.</span></span>

<span data-ttu-id="0c899-151">Další informace o používání účet organizace v Microsoft Azure najdete v tématu [registrace pro Microsoft Azure jako organizace](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="0c899-151">For more information about using an organizational account with Microsoft Azure, see [Sign up for Microsoft Azure as an Organization](../active-directory/sign-up-organization.md).</span></span>

<span data-ttu-id="0c899-152">Máte-li více předplatných a chcete určit jedno konkrétní, které se má použít pro Azure Key Vault, zadejte následující příkaz k zobrazení předplatných pro váš účet:</span><span class="sxs-lookup"><span data-stu-id="0c899-152">If you have multiple subscriptions and want to specify a specific one to use for Azure Key Vault, type the following to see the subscriptions for your account:</span></span>

    azure account list

<span data-ttu-id="0c899-153">Poté pro určení předplatného, které se má použít, zadejte:</span><span class="sxs-lookup"><span data-stu-id="0c899-153">Then, to specify the subscription to use, type:</span></span>

    azure account set <subscription name>

<span data-ttu-id="0c899-154">Další informace o konfiguraci rozhraní příkazového řádku pro různé platformy Azure najdete v tématu [postup instalace a konfigurace rozhraní příkazového řádku Azure napříč platformami](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="0c899-154">For more information about configuring Azure Cross-Platform Command-Line Interface, see [How to Install and Configure Azure Cross-Platform Command-Line Interface](../cli-install-nodejs.md).</span></span>

## <a name="switch-to-using-azure-resource-manager"></a><span data-ttu-id="0c899-155">Přejít na používání Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0c899-155">Switch to using Azure Resource Manager</span></span>
<span data-ttu-id="0c899-156">Key Vault vyžaduje Azure Resource Manager, takže zadejte následující příkaz a přepněte do režimu Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="0c899-156">The Key Vault requires Azure Resource Manager, so type the following to switch to Azure Resource Manager mode:</span></span>

    azure config mode arm

## <a name="create-a-new-resource-group"></a><span data-ttu-id="0c899-157">Vytvoření nové skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="0c899-157">Create a new resource group</span></span>
<span data-ttu-id="0c899-158">Pokud používáte Azure Resource Manager, všechny související prostředky vytváří v uvnitř skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="0c899-158">When using Azure Resource Manager, all related resources are created inside a resource group.</span></span> <span data-ttu-id="0c899-159">Pro účely tohoto kurzu vytvoříme novou skupinu prostředků, ContosoResourceGroup'.</span><span class="sxs-lookup"><span data-stu-id="0c899-159">We will create a new resource group 'ContosoResourceGroup' for this tutorial.</span></span>

    azure group create 'ContosoResourceGroup' 'East Asia'

<span data-ttu-id="0c899-160">První parametr je název skupiny prostředků a druhý parametr je umístění.</span><span class="sxs-lookup"><span data-stu-id="0c899-160">The first parameter is resource group name and the second parameter is the location.</span></span> <span data-ttu-id="0c899-161">Pro umístění, použijte příkaz `azure location list` zjišťují, jak zadat alternativní umístění na v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="0c899-161">For location, use the command `azure location list` to identify how to specify an alternative location to the one in this example.</span></span> <span data-ttu-id="0c899-162">Pokud potřebujete více informací, zadejte:`azure help location`</span><span class="sxs-lookup"><span data-stu-id="0c899-162">If you need more information, type: `azure help location`</span></span>

## <a name="register-the-key-vault-resource-provider"></a><span data-ttu-id="0c899-163">Registrace poskytovatele prostředků Key Vault</span><span class="sxs-lookup"><span data-stu-id="0c899-163">Register the Key Vault resource provider</span></span>
<span data-ttu-id="0c899-164">Ujistěte se, že poskytovatele prostředků Key Vault je zaregistrován v rámci vašeho předplatného:</span><span class="sxs-lookup"><span data-stu-id="0c899-164">Make sure that Key Vault resource provider is registered in your subscription:</span></span>

`azure provider register Microsoft.KeyVault`

<span data-ttu-id="0c899-165">To jenom je nutné provést jednou za předplatné.</span><span class="sxs-lookup"><span data-stu-id="0c899-165">This only needs to be done once per subscription.</span></span>

## <a name="create-a-key-vault"></a><span data-ttu-id="0c899-166">Vytvořte trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="0c899-166">Create a key vault</span></span>

<span data-ttu-id="0c899-167">Použití `azure keyvault create` příkaz pro vytvoření trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="0c899-167">Use the `azure keyvault create` command to create a key vault.</span></span> <span data-ttu-id="0c899-168">Tento skript má tři povinné parametry: název skupiny prostředků, název trezoru klíčů a zeměpisné umístění.</span><span class="sxs-lookup"><span data-stu-id="0c899-168">This script has three mandatory parameters: a resource group name, a key vault name, and the geographic location.</span></span>

<span data-ttu-id="0c899-169">Například pokud použijete název trezoru ContosoKeyVault, název skupiny prostředků ContosoResourceGroup a umístění východní Asie, zadejte:</span><span class="sxs-lookup"><span data-stu-id="0c899-169">For example, if you use the vault name of ContosoKeyVault, the resource group name of ContosoResourceGroup, and the location of East Asia, type:</span></span>

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

<span data-ttu-id="0c899-170">Výstup tohoto příkazu zobrazuje vlastnosti trezoru klíčů, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0c899-170">The output of this command shows properties of the key vault that you've just created.</span></span> <span data-ttu-id="0c899-171">Dvě nejdůležitější vlastnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="0c899-171">The two most important properties are:</span></span>

* <span data-ttu-id="0c899-172">**Název**: V tomto příkladu to je ContosoKeyVault.</span><span class="sxs-lookup"><span data-stu-id="0c899-172">**Name**: In the example this is ContosoKeyVault.</span></span> <span data-ttu-id="0c899-173">Tento název budete používat pro další rutiny Key Vault.</span><span class="sxs-lookup"><span data-stu-id="0c899-173">You will use this name for other Key Vault cmdlets.</span></span>
* <span data-ttu-id="0c899-174">**vaultUri**: V tomto příkladu to je https://contosokeyvault.vault.azure.net.</span><span class="sxs-lookup"><span data-stu-id="0c899-174">**vaultUri**: In the example this is https://contosokeyvault.vault.azure.net.</span></span> <span data-ttu-id="0c899-175">Aplikace, které používají váš trezor prostřednictvím REST API musí používat tento identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="0c899-175">Applications that use your vault through its REST API must use this URI.</span></span>

<span data-ttu-id="0c899-176">Váš účet Azure je nyní oprávněn provádět nad tímto trezorem klíčů všechny operace.</span><span class="sxs-lookup"><span data-stu-id="0c899-176">Your Azure account is now authorized to perform any operations on this key vault.</span></span> <span data-ttu-id="0c899-177">Zatím k tomu není oprávněn nikdo jiný.</span><span class="sxs-lookup"><span data-stu-id="0c899-177">As yet, nobody else is.</span></span>

## <a name="add-a-key-or-secret-to-the-key-vault"></a><span data-ttu-id="0c899-178">Přidejte k trezoru klíč nebo tajný klíč</span><span class="sxs-lookup"><span data-stu-id="0c899-178">Add a key or secret to the key vault</span></span>

<span data-ttu-id="0c899-179">Pokud chcete Azure Key Vault vytvořila softwarově chráněný klíč pro vás, použijte `azure key create` příkaz a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0c899-179">If you want Azure Key Vault to create a software-protected key for you, use the `azure key create` command, and type the following:</span></span>

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

<span data-ttu-id="0c899-180">Nicméně pokud máte existující klíč v soubor .pem uložený jako místního souboru do souboru s názvem softkey.pem, který chcete nahrát do Azure Key Vault, zadejte následující příkaz pro import klíče z. Soubor PEM, který chrání klíč softwarem ve službě Key Vault:</span><span class="sxs-lookup"><span data-stu-id="0c899-180">However, if you have an existing key in a .pem file saved as local file in a file named softkey.pem that you want to upload to Azure Key Vault, type the following to import the key from the .PEM file, which protects the key by software in the Key Vault service:</span></span>

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

<span data-ttu-id="0c899-181">Nyní můžete odkazovat klíč, který jste vytvořili nebo nahrán do Azure Key Vault, pomocí jeho identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="0c899-181">You can now reference the key that you created or uploaded to Azure Key Vault, by using its URI.</span></span> <span data-ttu-id="0c899-182">Použít **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** vždy získáte aktuální verzi a používat **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** Chcete-li získat konkrétní verzi.</span><span class="sxs-lookup"><span data-stu-id="0c899-182">Use  **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** to always get the current version, and use **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** to get this specific version.</span></span>

<span data-ttu-id="0c899-183">Chcete-li přidat tajný klíč k úložišti, který je hesla s názvem SQLPassword a hodnotou z Pa$ $w0rd do Azure Key Vault, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0c899-183">To add a secret to the vault, which is a password named SQLPassword and that has the value of Pa$$w0rd to Azure Key Vault, type the following:</span></span>

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

<span data-ttu-id="0c899-184">Nyní na toto heslo, které jste přidali do Azure Key Vault, můžete odkazovat pomocí jeho identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="0c899-184">You can now reference this password that you added to Azure Key Vault, by using its URI.</span></span> <span data-ttu-id="0c899-185">Pomocí **https://ContosoVault.vault.azure.net/secrets/SQLPassword** vždy získáte aktuální verzi. Chcete-li získat konkrétní verzi, použijte **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**.</span><span class="sxs-lookup"><span data-stu-id="0c899-185">Use **https://ContosoVault.vault.azure.net/secrets/SQLPassword** to always get the current version, and use **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** to get this specific version.</span></span>

<span data-ttu-id="0c899-186">Podívejme se na klíč nebo tajný klíč, který jste právě vytvořili:</span><span class="sxs-lookup"><span data-stu-id="0c899-186">Let's view the key or secret that you just created:</span></span>

* <span data-ttu-id="0c899-187">Pokud chcete zobrazit klíč, zadejte `azure keyvault key list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="0c899-187">To view your key, type: `azure keyvault key list --vault-name 'ContosoKeyVault'`</span></span>
* <span data-ttu-id="0c899-188">Pokud chcete zobrazit tajný klíč, zadejte `azure keyvault secret list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="0c899-188">To view your secret, type: `azure keyvault secret list --vault-name 'ContosoKeyVault'`</span></span>

## <a name="register-an-application-with-azure-active-directory"></a><span data-ttu-id="0c899-189">Registrujte aplikaci s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0c899-189">Register an application with Azure Active Directory</span></span>

<span data-ttu-id="0c899-190">Tento krok obvykle provádí vývojář na samostatném počítači.</span><span class="sxs-lookup"><span data-stu-id="0c899-190">This step would usually be done by a developer, on a separate computer.</span></span> <span data-ttu-id="0c899-191">Není specifické pro Azure Key Vault, ale je zahrnuté pro úplnost.</span><span class="sxs-lookup"><span data-stu-id="0c899-191">It is not specific to Azure Key Vault but is included here, for completeness.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0c899-192">Pro dokončení kurzu musí být váš účet, trezor i aplikace, kterou budete v tomto kroku registrovat, ve stejném adresáři Azure.</span><span class="sxs-lookup"><span data-stu-id="0c899-192">To complete the tutorial, your account, the vault, and the application that you will register in this step must all be in the same Azure directory.</span></span>
> 
> 

<span data-ttu-id="0c899-193">Aplikace, které používají trezor klíčů, se musí ověřit pomocí tokenu z Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0c899-193">Applications that use a key vault must authenticate by using a token from Azure Active Directory.</span></span> <span data-ttu-id="0c899-194">K tomu je potřeba, aby majitel aplikace nejprve zaregistroval aplikaci ve vlastním Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0c899-194">To do this, the owner of the application must first register the application in their Azure Active Directory.</span></span> <span data-ttu-id="0c899-195">Na konci registrace obdrží majitel aplikace následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="0c899-195">At the end of registration, the application owner gets the following values:</span></span>

* <span data-ttu-id="0c899-196">**ID aplikace** (také označované jako ID klienta) a **ověřovací klíč** (také označovaný jako sdílený tajný klíč).</span><span class="sxs-lookup"><span data-stu-id="0c899-196">An **Application ID** (also known as a Client ID) and **authentication key** (also known as the shared secret).</span></span> <span data-ttu-id="0c899-197">Aplikace musí představovat obě tyto hodnoty do Azure Active Directory, chcete-li získat token.</span><span class="sxs-lookup"><span data-stu-id="0c899-197">The application must present both of these values to Azure Active Directory, to get a token.</span></span> <span data-ttu-id="0c899-198">Záleží na aplikaci, jak je k tomu nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="0c899-198">How the application is configured to do this depends on the application.</span></span> <span data-ttu-id="0c899-199">Pro ukázkovou aplikaci Key Vault nastavuje majitel tyto hodnoty v souboru app.config.</span><span class="sxs-lookup"><span data-stu-id="0c899-199">For the Key Vault sample application, the application owner sets these values in the app.config file.</span></span>

<span data-ttu-id="0c899-200">Chcete-li zaregistrovat aplikaci v Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="0c899-200">To register the application in Azure Active Directory:</span></span>

1. <span data-ttu-id="0c899-201">Přihlaste se k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0c899-201">Sign in to the Azure portal.</span></span>
2. <span data-ttu-id="0c899-202">Na levé straně klikněte na **Active Directory** a poté vyberte adresář, ve kterém zaregistrujete svoji aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0c899-202">On the left, click **Active Directory**, and then select the directory in which you will register your application.</span></span> <br> <br> 

>[!NOTE] 
> <span data-ttu-id="0c899-203">Musíte vybrat stejný adresář, který obsahuje předplatné Azure, který jste použili pro vytvoření trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="0c899-203">You must select the same directory that contains the Azure subscription with which you created your key vault.</span></span> <span data-ttu-id="0c899-204">Pokud si nejste jisti, který adresář to je, klikněte na **Nastavení**, určete předplatné, které jste použili při vytvoření trezoru klíčů, a poznamenejte si název adresáře v posledním sloupci.</span><span class="sxs-lookup"><span data-stu-id="0c899-204">If you do not know which directory this is, click **Settings**, identify the subscription with which you created your key vault, and note the name of the directory displayed in the last column.</span></span>

3. <span data-ttu-id="0c899-205">Klikněte na **Aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0c899-205">Click **APPLICATIONS**.</span></span> <span data-ttu-id="0c899-206">Pokud do vašeho adresáře nebyly přidané žádné aplikace, tato stránka se zobrazí pouze **přidat aplikaci** odkaz.</span><span class="sxs-lookup"><span data-stu-id="0c899-206">If no apps have been added to your directory, this page will show only the **Add an App** link.</span></span> <span data-ttu-id="0c899-207">Klikněte na odkaz, nebo také můžete klepnout **přidat** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="0c899-207">Click the link, or alternatively, you can click the **ADD** on the command bar.</span></span>
4. <span data-ttu-id="0c899-208">V průvodci **Přidat aplikaci** klikněte na stránce **Co si přejete udělat?** na **Přidat aplikaci, kterou vyvíjí moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="0c899-208">In the **ADD APPLICATION** wizard, on the **What do you want to do?** page, click **Add an application my organization is developing**.</span></span>
5. <span data-ttu-id="0c899-209">Na **Řekněte nám o své aplikaci** , zadejte název pro vaši aplikaci a vyberte **webové aplikace nebo webové rozhraní API** (výchozí).</span><span class="sxs-lookup"><span data-stu-id="0c899-209">On the **Tell us about your application** page, specify a name for your application and select **WEB APPLICATION AND/OR WEB API** (the default).</span></span> <span data-ttu-id="0c899-210">Klikněte na ikonu Další.</span><span class="sxs-lookup"><span data-stu-id="0c899-210">Click the Next icon.</span></span>
6. <span data-ttu-id="0c899-211">Na stránce **Vlastnosti aplikace** zadejte **URL pro přihlašování** a **Identifikátor ID URI Aplikace** pro svoji webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0c899-211">On the **App properties** page, specify the **SIGN-ON URL** and **APP ID URI** for your web application.</span></span> <span data-ttu-id="0c899-212">Pokud vaše aplikace tyto hodnoty neobsahuje, v tomto kroku si je můžete vymyslet (například můžete do obou polí zadat http://test1.contoso.com).</span><span class="sxs-lookup"><span data-stu-id="0c899-212">If your application does not have these values, you can make them up for this step (for example, you could specify http://test1.contoso.com for both boxes).</span></span> <span data-ttu-id="0c899-213">Není důležité, zda tyto stránky existují, důležité je, aby identifikátor ID URI aplikace byl pro každou aplikaci ve vašem adresáři jiný.</span><span class="sxs-lookup"><span data-stu-id="0c899-213">It does not matter if these sites exist; what is important is that the app ID URI for each application is different for every application in your directory.</span></span> <span data-ttu-id="0c899-214">Adresář tento řetězec používá k identifikaci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0c899-214">The directory uses this string to identify your app.</span></span>
7. <span data-ttu-id="0c899-215">Klikněte na ikonu dokončení uložte provedené změny v průvodci.</span><span class="sxs-lookup"><span data-stu-id="0c899-215">Click the Complete icon to save your changes in the wizard.</span></span>
8. <span data-ttu-id="0c899-216">Na stránce Rychlý Start klikněte na **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="0c899-216">On the Quick Start page, click **CONFIGURE**.</span></span>
9. <span data-ttu-id="0c899-217">Posuňte se k části **klíče**, vyberte dobu trvání a poté klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0c899-217">Scroll to the **keys** section, select the duration, and then click **SAVE**.</span></span> <span data-ttu-id="0c899-218">Stránka se obnoví a zobrazí hodnotu klíče.</span><span class="sxs-lookup"><span data-stu-id="0c899-218">The page refreshes and now shows a key value.</span></span> <span data-ttu-id="0c899-219">Vaši aplikaci je nutné nakonfigurovat s touto hodnotou klíče a s hodnotou **ID klienta**.</span><span class="sxs-lookup"><span data-stu-id="0c899-219">You must configure your application with this key value and the **CLIENT ID** value.</span></span> <span data-ttu-id="0c899-220">(Pokyny k této konfiguraci se budou lišit podle aplikace.)</span><span class="sxs-lookup"><span data-stu-id="0c899-220">(Instructions for this configuration will be application-specific.)</span></span>
10. <span data-ttu-id="0c899-221">Poznamenejte si hodnotu ID klienta z této stránky, v dalším kroku ji použijete pro nastavení oprávnění pro váš trezoru.</span><span class="sxs-lookup"><span data-stu-id="0c899-221">Copy the client ID value from this page, which you will use in the next step to set permissions on your vault.</span></span>

## <a name="authorize-the-application-to-use-the-key-or-secret"></a><span data-ttu-id="0c899-222">Autorizujte aplikaci pro použití klíče nebo tajného klíče</span><span class="sxs-lookup"><span data-stu-id="0c899-222">Authorize the application to use the key or secret</span></span>
<span data-ttu-id="0c899-223">Chcete-li autorizovat aplikaci pro přístup ke klíči nebo tajný klíč v trezoru, použijte `azure keyvault set-policy` příkaz.</span><span class="sxs-lookup"><span data-stu-id="0c899-223">To authorize the application to access the key or secret in the vault, use the `azure keyvault set-policy` command.</span></span>

<span data-ttu-id="0c899-224">Například pokud je název vaší trezoru ContosoKeyVault a aplikace, které chcete autorizovat má hodnotu 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed ID klienta a chcete aplikaci autorizovat pro dešifrování a podepisování pomocí klíčů ve vašem trezoru, spusťte následující:</span><span class="sxs-lookup"><span data-stu-id="0c899-224">For example, if your vault name is ContosoKeyVault and the application you want to authorize has a client ID of 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, and you want to authorize the application to decrypt and sign with keys in your vault, then run the following:</span></span>

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '[\"decrypt\",\"sign\"]'

> [!NOTE]
> <span data-ttu-id="0c899-225">Pokud používáte na příkazovém řádku Windows, doporučujeme nahradit jednoduchých uvozovek a být dvojité uvozovky a také řídicí interní dvojité uvozovky.</span><span class="sxs-lookup"><span data-stu-id="0c899-225">If you are running on Windows command prompt, you should replace single quotes with double quotes, and also escape the internal double quotes.</span></span> <span data-ttu-id="0c899-226">Například: "[\"dešifrovat\",\"přihlašovací\"]".</span><span class="sxs-lookup"><span data-stu-id="0c899-226">For example: "[\"decrypt\",\"sign\"]".</span></span>
> 
> 

<span data-ttu-id="0c899-227">Chcete-li tu samou aplikaci autorizovat pro čtení tajných klíčů v trezoru, spusťte následující:</span><span class="sxs-lookup"><span data-stu-id="0c899-227">If you want to authorize that same application to read secrets in your vault, run the following:</span></span>

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '[\"get\"]'

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a><span data-ttu-id="0c899-228">Pokud chcete použít modul hardwarového zabezpečení (HSM)</span><span class="sxs-lookup"><span data-stu-id="0c899-228">If you want to use a hardware security module (HSM)</span></span>
<span data-ttu-id="0c899-229">Pro lepší kontrolu můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM), které nikdy neopustí hranice HSM.</span><span class="sxs-lookup"><span data-stu-id="0c899-229">For added assurance, you can import or generate keys in hardware security modules (HSMs) that never leave the HSM boundary.</span></span> <span data-ttu-id="0c899-230">Moduly hardwarového zabezpečení jsou ověřené podle standardu FIPS 140-2 Level 2.</span><span class="sxs-lookup"><span data-stu-id="0c899-230">The HSMs are FIPS 140-2 Level 2 validated.</span></span> <span data-ttu-id="0c899-231">Pokud se vás tento požadavek netýká, přeskočte tuto část a přejděte k části [Odstranění trezoru klíčů, přidružených klíčů a tajných klíčů](#delete-the-key-vault-and-associated-keys-and-secrets).</span><span class="sxs-lookup"><span data-stu-id="0c899-231">If this requirement doesn't apply to you, skip this section and go to [Delete the key vault and associated keys and secrets](#delete-the-key-vault-and-associated-keys-and-secrets).</span></span>

<span data-ttu-id="0c899-232">Pro vytvoření těchto klíčů chráněných pomocí HSM, musíte mít předplatné trezoru s podporou klíčů chráněných pomocí HSM.</span><span class="sxs-lookup"><span data-stu-id="0c899-232">To create these HSM-protected keys, you must have a vault subscription that supports HSM-protected keys.</span></span>

<span data-ttu-id="0c899-233">Když vytvoříte keyvault, přidejte parametr 'sku':</span><span class="sxs-lookup"><span data-stu-id="0c899-233">When you create the keyvault, add the 'sku' parameter:</span></span>

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

<span data-ttu-id="0c899-234">Do tohoto trezoru můžete přidat klíče chráněné softwarem (jak jsme ukázali výše) a klíče chráněné pomocí HSM.</span><span class="sxs-lookup"><span data-stu-id="0c899-234">You can add software-protected keys (as shown earlier) and HSM-protected keys to this vault.</span></span> <span data-ttu-id="0c899-235">Chcete-li vytvořit klíč chráněný HSM, nastavte parametr cílové na "HSM":</span><span class="sxs-lookup"><span data-stu-id="0c899-235">To create an HSM-protected key, set the Destination parameter to 'HSM':</span></span>

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

<span data-ttu-id="0c899-236">Můžete následující příkaz pro import klíče ze souboru .pem ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="0c899-236">You can use the following command to import a key from a .pem file on your computer.</span></span> <span data-ttu-id="0c899-237">Tento příkaz importuje klíč do HSM ve službě Key Vault:</span><span class="sxs-lookup"><span data-stu-id="0c899-237">This command imports the key into HSMs in the Key Vault service:</span></span>

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

<span data-ttu-id="0c899-238">Další příkaz importuje balíček „přineste si vlastní klíč“ (BYOK).</span><span class="sxs-lookup"><span data-stu-id="0c899-238">The next command imports a “bring your own key" (BYOK) package.</span></span> <span data-ttu-id="0c899-239">To vám umožní vygenerovat klíč v místním HSM a jeho přenesení do HSM ve službě Key Vault, aniž by klíč opustil hranice HSM.</span><span class="sxs-lookup"><span data-stu-id="0c899-239">This lets you generate your key in your local HSM, and transfer it to HSMs in the Key Vault service, without the key leaving the HSM boundary:</span></span>

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

<span data-ttu-id="0c899-240">Podrobnější pokyny o tom, jak generovat tento balíček BYOK naleznete v tématu [použití HSM-Protected klíčů s Azure Key Vault](key-vault-hsm-protected-keys.md).</span><span class="sxs-lookup"><span data-stu-id="0c899-240">For more detailed instructions about how to generate this BYOK package, see [How to use HSM-Protected Keys with Azure Key Vault](key-vault-hsm-protected-keys.md).</span></span>

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a><span data-ttu-id="0c899-241">Odstranění trezoru klíčů, přidružených klíčů a tajných klíčů</span><span class="sxs-lookup"><span data-stu-id="0c899-241">Delete the key vault and associated keys and secrets</span></span>
<span data-ttu-id="0c899-242">Pokud již nepotřebujete trezor klíčů a klíč nebo tajný klíč, který obsahuje, můžete trezor klíčů odstranit pomocí příkazu delete azure keyvault:</span><span class="sxs-lookup"><span data-stu-id="0c899-242">If you no longer need the key vault and the key or secret that it contains, you can delete the key vault by using the azure keyvault delete command:</span></span>

    azure keyvault delete --vault-name 'ContosoKeyVault'

<span data-ttu-id="0c899-243">Nebo můžete odstranit celou skupinu prostředků Azure, která zahrnuje trezor klíčů a všechny další prostředky, které jste do skupiny zahrnuli.</span><span class="sxs-lookup"><span data-stu-id="0c899-243">Or, you can delete an entire Azure resource group, which includes the key vault and any other resources that you included in that group:</span></span>

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a><span data-ttu-id="0c899-244">Ostatní příkazy rozhraní příkazového řádku Azure a platformy</span><span class="sxs-lookup"><span data-stu-id="0c899-244">Other Azure Cross-Platform Command-line Interface Commands</span></span>
<span data-ttu-id="0c899-245">Další příkazy, že může využít ke správě Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="0c899-245">Other commands that you might useful for managing Azure Key Vault.</span></span>

<span data-ttu-id="0c899-246">Tento příkaz vypíše tabulkové zobrazení všech klíčů a vybraných vlastností:</span><span class="sxs-lookup"><span data-stu-id="0c899-246">This command lists a tabular display of all keys and selected properties:</span></span>

    azure keyvault key list --vault-name 'ContosoKeyVault'

<span data-ttu-id="0c899-247">Tento příkaz zobrazí úplný seznam vlastností pro zadaný klíč:</span><span class="sxs-lookup"><span data-stu-id="0c899-247">This command displays a full list of properties for the specified key:</span></span>

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

<span data-ttu-id="0c899-248">Tento příkaz vypíše tabulkové zobrazení všech názvů tajných klíčů a vybraných vlastností:</span><span class="sxs-lookup"><span data-stu-id="0c899-248">This command lists a tabular display of all secret names and selected properties:</span></span>

    azure keyvault secret list --vault-name 'ContosoKeyVault'

<span data-ttu-id="0c899-249">Tady je příklad odebrání určitého klíče:</span><span class="sxs-lookup"><span data-stu-id="0c899-249">Here's an example of how to remove a specific key:</span></span>

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

<span data-ttu-id="0c899-250">Tady je příklad odebrání určitého tajného klíče:</span><span class="sxs-lookup"><span data-stu-id="0c899-250">Here's an example of how to remove a specific secret:</span></span>

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a><span data-ttu-id="0c899-251">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0c899-251">Next steps</span></span>
<span data-ttu-id="0c899-252">Programátorské reference najdete v [příručce pro vývojáře Azure Key Vault](key-vault-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="0c899-252">For programming references, see [the Azure Key Vault developer's guide](key-vault-developers-guide.md).</span></span>

