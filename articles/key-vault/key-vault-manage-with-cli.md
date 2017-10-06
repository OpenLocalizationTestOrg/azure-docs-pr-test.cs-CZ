---
title: "aaaManage Azure Key Vault, pomocí rozhraní příkazového řádku | Microsoft Docs"
description: "Použijte tento kurz tooautomate běžné úlohy v Key Vault pomocí hello rozhraní příkazového řádku"
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
ms.openlocfilehash: 9ef506faa67e1f0db5b9e303300d63b135ddd7b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-key-vault-using-cli"></a><span data-ttu-id="ba335-103">Spravovat Key Vault pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="ba335-103">Manage Key Vault using CLI</span></span>

<span data-ttu-id="ba335-104">Azure Key Vault je dostupný ve většině oblastí.</span><span class="sxs-lookup"><span data-stu-id="ba335-104">Azure Key Vault is available in most regions.</span></span> <span data-ttu-id="ba335-105">Další informace najdete v tématu hello [stránce s cenami Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="ba335-105">For more information, see hello [Key Vault pricing page](https://azure.microsoft.com/pricing/details/key-vault/).</span></span>

## <a name="introduction"></a><span data-ttu-id="ba335-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="ba335-106">Introduction</span></span>

<span data-ttu-id="ba335-107">Použijte tento kurz toohelp získáte začít s Azure Key Vault toocreate zesílený kontejner (trezor) v Azure, toostore a spravovat kryptografické klíče a tajné klíče v Azure.</span><span class="sxs-lookup"><span data-stu-id="ba335-107">Use this tutorial toohelp you get started with Azure Key Vault toocreate a hardened container (a vault) in Azure, toostore and manage cryptographic keys and secrets in Azure.</span></span> <span data-ttu-id="ba335-108">Provede vás procesem hello pomocí rozhraní příkazového řádku pro různé platformy Azure toocreate trezoru, který obsahuje klíč nebo heslo, které pak můžete použít s aplikací Azure.</span><span class="sxs-lookup"><span data-stu-id="ba335-108">It walks you through hello process of using Azure Cross-Platform Command-Line Interface toocreate a vault that contains a key or password that you can then use with an Azure application.</span></span> <span data-ttu-id="ba335-109">Poté vám ukáže, jak aplikaci můžete potom použít tento klíč nebo heslo.</span><span class="sxs-lookup"><span data-stu-id="ba335-109">It then shows you how an application can then use that key or password.</span></span>

<span data-ttu-id="ba335-110">**Odhadovaný čas toocomplete:** 20 minut</span><span class="sxs-lookup"><span data-stu-id="ba335-110">**Estimated time toocomplete:** 20 minutes</span></span>

> [!NOTE]
> <span data-ttu-id="ba335-111">Tento kurz neobsahuje pokyny, jak toowrite hello Azure aplikace, která obsahuje jeden z hello kroků, které ukazuje, jak tooauthorize aplikaci toouse klíč nebo tajný klíč v hello klíč trezoru.</span><span class="sxs-lookup"><span data-stu-id="ba335-111">This tutorial does not include instructions on how toowrite hello Azure application that one of hello steps includes, which shows how tooauthorize an application toouse a key or secret in hello key vault.</span></span>
> 
> <span data-ttu-id="ba335-112">V současné době nelze Azure Key Vault konfigurovat v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba335-112">Currently, you cannot configure Azure Key Vault in hello Azure portal.</span></span> <span data-ttu-id="ba335-113">Místo toho použijte tyto pokyny Multiplatformního rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="ba335-113">Instead, use these Cross-Platform Command-Line Interface  instructions.</span></span> <span data-ttu-id="ba335-114">Nebo Azure PowerShell pokyny najdete v tématu [tomto ekvivalentním kurzu](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ba335-114">Or, for Azure PowerShell instructions, see [this equivalent tutorial](key-vault-get-started.md).</span></span>
> 
> 

<span data-ttu-id="ba335-115">Souhrnné informace o Azure Key Vault naleznete v tématu [Co je Azure Key Vault?](key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ba335-115">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ba335-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ba335-116">Prerequisites</span></span>

<span data-ttu-id="ba335-117">toocomplete tento kurz, musíte mít hello následující:</span><span class="sxs-lookup"><span data-stu-id="ba335-117">toocomplete this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="ba335-118">TooMicrosoft předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="ba335-118">A subscription tooMicrosoft Azure.</span></span> <span data-ttu-id="ba335-119">Pokud jeden nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial).</span><span class="sxs-lookup"><span data-stu-id="ba335-119">If you do not have one, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial).</span></span>
* <span data-ttu-id="ba335-120">Rozhraní příkazového řádku verzi 0.9.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ba335-120">Command-Line Interface version 0.9.1 or later.</span></span> <span data-ttu-id="ba335-121">tooinstall hello nejnovější verzi a připojte tooyour předplatné Azure, najdete v části [instalace a konfigurace rozhraní příkazového řádku pro různé platformy Azure hello](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="ba335-121">tooinstall hello latest version and connect tooyour Azure subscription, see [Install and Configure hello Azure Cross-Platform Command-Line Interface](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="ba335-122">Aplikace, které budou nakonfigurované toouse hello klíč nebo heslo, které vytvoříte v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ba335-122">An application that will be configured toouse hello key or password that you create in this tutorial.</span></span> <span data-ttu-id="ba335-123">Vzorová aplikace je k dispozici z hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span><span class="sxs-lookup"><span data-stu-id="ba335-123">A sample application is available from hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span></span> <span data-ttu-id="ba335-124">Pokyny najdete v tématu hello doplňujícími souboru Readme.</span><span class="sxs-lookup"><span data-stu-id="ba335-124">For instructions, see hello accompanying Readme file.</span></span>

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a><span data-ttu-id="ba335-125">Získání nápovědy pomocí rozhraní příkazového řádku Azure a platformy</span><span class="sxs-lookup"><span data-stu-id="ba335-125">Getting help with Azure Cross-Platform Command-Line Interface</span></span>

<span data-ttu-id="ba335-126">V tomto kurzu se předpokládá, že jste obeznámeni s hello rozhraní příkazového řádku (Bash, terminálu, příkazového řádku)</span><span class="sxs-lookup"><span data-stu-id="ba335-126">This tutorial assumes that you are familiar with hello command-line interface (Bash, Terminal, Command prompt)</span></span>

<span data-ttu-id="ba335-127">Hello – Nápověda nebo -h parametr může být použit tooview nápovědy pro určité příkazy.</span><span class="sxs-lookup"><span data-stu-id="ba335-127">hello --help or -h parameter can be used tooview help for specific commands.</span></span> <span data-ttu-id="ba335-128">Alternativně hello azure nápovědy [příkaz] [možnosti], že formát lze také použít tooreturn hello stejné informace.</span><span class="sxs-lookup"><span data-stu-id="ba335-128">Alternately, hello azure help [command] [options] format can also be used tooreturn hello same information.</span></span> <span data-ttu-id="ba335-129">Například následující hello příkazy všechny návratové hello stejné informace:</span><span class="sxs-lookup"><span data-stu-id="ba335-129">For example, hello following commands all return hello same information:</span></span>

    azure account set --help

    azure account set -h

    azure help account set

<span data-ttu-id="ba335-130">Pokud máte pochybnosti o parametrech hello vyžaduje příkaz, naleznete v toohelp pomocí – Nápověda, -h nebo azure nápovědy [příkaz].</span><span class="sxs-lookup"><span data-stu-id="ba335-130">When in doubt about hello parameters needed by a command, refer toohelp using --help, -h or azure help [command].</span></span>

<span data-ttu-id="ba335-131">Také si můžete přečíst následující kurzy tooget obeznámeni s Azure Resource Manager v rozhraní příkazového řádku a platformy Azure hello:</span><span class="sxs-lookup"><span data-stu-id="ba335-131">You can also read hello following tutorials tooget familiar with Azure Resource Manager in Azure Cross-Platform Command-Line Interface:</span></span>

* [<span data-ttu-id="ba335-132">Jak tooinstall a konfigurace rozhraní příkazového řádku Azure a platformy</span><span class="sxs-lookup"><span data-stu-id="ba335-132">How tooinstall and configure Azure Cross-Platform Command Line Interface</span></span>](../cli-install-nodejs.md)
* [<span data-ttu-id="ba335-133">Pomocí rozhraní příkazového řádku a platformy Azure s Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="ba335-133">Using Azure Cross-Platform Command-Line Interface with Azure Resource Manager</span></span>](../xplat-cli-azure-resource-manager.md)

## <a name="connect-tooyour-subscriptions"></a><span data-ttu-id="ba335-134">Připojit tooyour odběrů</span><span class="sxs-lookup"><span data-stu-id="ba335-134">Connect tooyour subscriptions</span></span>

<span data-ttu-id="ba335-135">toolog pomocí účtu organizace, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ba335-135">toolog in using an organizational account, use hello following command:</span></span>

    azure login -u username -p password

<span data-ttu-id="ba335-136">nebo pokud chcete toolog zadáním interaktivně</span><span class="sxs-lookup"><span data-stu-id="ba335-136">or if you want toolog in by typing interactively</span></span>

    azure login

> [!NOTE]
> <span data-ttu-id="ba335-137">Hello přihlášení metodu lze použít pouze s účtem organizace.</span><span class="sxs-lookup"><span data-stu-id="ba335-137">hello login method only works with organizational account.</span></span> <span data-ttu-id="ba335-138">Účet organizace je uživatel, který je spravovaná vaší organizací a definované v klienta Azure Active Directory vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="ba335-138">An organizational account is a user that is managed by your organization, and defined in your organization's Azure Active Directory tenant.</span></span>
> 
> 

<span data-ttu-id="ba335-139">Pokud nyní není k dispozici účet organizace a používají toolog účet Microsoft v tooyour předplatné Azure, můžete snadno vytvořit jednu pomocí hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="ba335-139">If you do not currently have an organizational account, and are using a Microsoft account toolog in tooyour Azure subscription, you can easily create one using hello following steps.</span></span>

1. <span data-ttu-id="ba335-140">Přihlášení toohello přihlášení toohello [portálu pro správu Azure](https://manage.windowsazure.com/)a klikněte na Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ba335-140">Login toohello Login toohello [Azure Management Portal](https://manage.windowsazure.com/), and click on Active Directory.</span></span>
2. <span data-ttu-id="ba335-141">Pokud neexistuje žádný adresář, vyberte možnost vytvoření adresáře a zajištění hello požadované informace.</span><span class="sxs-lookup"><span data-stu-id="ba335-141">If no directory exists, select Create your directory and provide hello requested information.</span></span>
3. <span data-ttu-id="ba335-142">Vyberte adresář a přidání nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="ba335-142">Select your directory and add a new user.</span></span> <span data-ttu-id="ba335-143">Tento nový uživatel je účet organizace.</span><span class="sxs-lookup"><span data-stu-id="ba335-143">This new user is an organizational account.</span></span> <span data-ttu-id="ba335-144">Během vytváření hello hello uživatele bude nutné zadat e-mailovou adresu pro hello uživatele a dočasné heslo.</span><span class="sxs-lookup"><span data-stu-id="ba335-144">During hello creation of hello user, you will be supplied with both an e-mail address for hello user and a temporary password.</span></span> <span data-ttu-id="ba335-145">Tyto informace uložte, protože se používá v jiném kroku.</span><span class="sxs-lookup"><span data-stu-id="ba335-145">Save this information as it is used in another step.</span></span>
4. <span data-ttu-id="ba335-146">Z portálu hello vyberte nastavení a pak vyberte správci.</span><span class="sxs-lookup"><span data-stu-id="ba335-146">From hello portal, select Settings and then select Administrators.</span></span> <span data-ttu-id="ba335-147">Vyberte Přidat a přidat nové uživatele hello jako spolusprávce.</span><span class="sxs-lookup"><span data-stu-id="ba335-147">Select Add, and add hello new user as a co-administrator.</span></span> <span data-ttu-id="ba335-148">To umožňuje hello účet organizace toomanage vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="ba335-148">This allows hello organizational account toomanage your Azure subscription.</span></span>
5. <span data-ttu-id="ba335-149">Nakonec odhlášení od hello portál Azure a potom znovu přihlásit pomocí účtu organizace nové hello.</span><span class="sxs-lookup"><span data-stu-id="ba335-149">Finally, log out of hello Azure portal and then log back in using hello new organizational account.</span></span> <span data-ttu-id="ba335-150">Pokud je to hello první čas přihlášení pomocí tohoto účtu, bude heslo výzvami toochange hello.</span><span class="sxs-lookup"><span data-stu-id="ba335-150">If this is hello first time logging in with this account, you will be prompted toochange hello password.</span></span>

<span data-ttu-id="ba335-151">Další informace o používání účet organizace v Microsoft Azure najdete v tématu [registrace pro Microsoft Azure jako organizace](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="ba335-151">For more information about using an organizational account with Microsoft Azure, see [Sign up for Microsoft Azure as an Organization](../active-directory/sign-up-organization.md).</span></span>

<span data-ttu-id="ba335-152">Pokud máte více předplatných a chcete toospecify konkrétní jeden toouse pro Azure Key Vault, zadejte hello následující toosee hello předplatných pro váš účet:</span><span class="sxs-lookup"><span data-stu-id="ba335-152">If you have multiple subscriptions and want toospecify a specific one toouse for Azure Key Vault, type hello following toosee hello subscriptions for your account:</span></span>

    azure account list

<span data-ttu-id="ba335-153">Potom toospecify hello předplatné toouse, zadejte:</span><span class="sxs-lookup"><span data-stu-id="ba335-153">Then, toospecify hello subscription toouse, type:</span></span>

    azure account set <subscription name>

<span data-ttu-id="ba335-154">Další informace o konfiguraci rozhraní příkazového řádku pro různé platformy Azure najdete v tématu [jak tooInstall a konfigurace rozhraní příkazového řádku Azure napříč platformami](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="ba335-154">For more information about configuring Azure Cross-Platform Command-Line Interface, see [How tooInstall and Configure Azure Cross-Platform Command-Line Interface](../cli-install-nodejs.md).</span></span>

## <a name="switch-toousing-azure-resource-manager"></a><span data-ttu-id="ba335-155">Přepínač toousing Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="ba335-155">Switch toousing Azure Resource Manager</span></span>
<span data-ttu-id="ba335-156">Azure Resource Manager vyžaduje Hello Key Vault, proto hello režimu Resource Manager tooAzure tooswitch následující:</span><span class="sxs-lookup"><span data-stu-id="ba335-156">hello Key Vault requires Azure Resource Manager, so type hello following tooswitch tooAzure Resource Manager mode:</span></span>

    azure config mode arm

## <a name="create-a-new-resource-group"></a><span data-ttu-id="ba335-157">Vytvoření nové skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="ba335-157">Create a new resource group</span></span>
<span data-ttu-id="ba335-158">Pokud používáte Azure Resource Manager, všechny související prostředky vytváří v uvnitř skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="ba335-158">When using Azure Resource Manager, all related resources are created inside a resource group.</span></span> <span data-ttu-id="ba335-159">Pro účely tohoto kurzu vytvoříme novou skupinu prostředků, ContosoResourceGroup'.</span><span class="sxs-lookup"><span data-stu-id="ba335-159">We will create a new resource group 'ContosoResourceGroup' for this tutorial.</span></span>

    azure group create 'ContosoResourceGroup' 'East Asia'

<span data-ttu-id="ba335-160">první parametr Hello je název skupiny prostředků a druhý parametr hello je hello umístění.</span><span class="sxs-lookup"><span data-stu-id="ba335-160">hello first parameter is resource group name and hello second parameter is hello location.</span></span> <span data-ttu-id="ba335-161">Pro umístění, použijte příkaz hello `azure location list` tooidentify jak toospecify alternativní umístění toohello jednu v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="ba335-161">For location, use hello command `azure location list` tooidentify how toospecify an alternative location toohello one in this example.</span></span> <span data-ttu-id="ba335-162">Pokud potřebujete více informací, zadejte:`azure help location`</span><span class="sxs-lookup"><span data-stu-id="ba335-162">If you need more information, type: `azure help location`</span></span>

## <a name="register-hello-key-vault-resource-provider"></a><span data-ttu-id="ba335-163">Registrace poskytovatele prostředků hello Key Vault</span><span class="sxs-lookup"><span data-stu-id="ba335-163">Register hello Key Vault resource provider</span></span>
<span data-ttu-id="ba335-164">Ujistěte se, že poskytovatele prostředků Key Vault je zaregistrován v rámci vašeho předplatného:</span><span class="sxs-lookup"><span data-stu-id="ba335-164">Make sure that Key Vault resource provider is registered in your subscription:</span></span>

`azure provider register Microsoft.KeyVault`

<span data-ttu-id="ba335-165">Stačí to toobe provádí jednou za předplatné.</span><span class="sxs-lookup"><span data-stu-id="ba335-165">This only needs toobe done once per subscription.</span></span>

## <a name="create-a-key-vault"></a><span data-ttu-id="ba335-166">Vytvořte trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="ba335-166">Create a key vault</span></span>

<span data-ttu-id="ba335-167">Použití hello `azure keyvault create` příkaz toocreate trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="ba335-167">Use hello `azure keyvault create` command toocreate a key vault.</span></span> <span data-ttu-id="ba335-168">Tento skript má tři povinné parametry: název skupiny prostředků, název trezoru klíčů a hello zeměpisné umístění.</span><span class="sxs-lookup"><span data-stu-id="ba335-168">This script has three mandatory parameters: a resource group name, a key vault name, and hello geographic location.</span></span>

<span data-ttu-id="ba335-169">Například pokud použijete název trezoru hello ContosoKeyVault, název skupiny prostředků hello ContosoResourceGroup a hello umístění východní Asie, zadejte:</span><span class="sxs-lookup"><span data-stu-id="ba335-169">For example, if you use hello vault name of ContosoKeyVault, hello resource group name of ContosoResourceGroup, and hello location of East Asia, type:</span></span>

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

<span data-ttu-id="ba335-170">Hello výstup tohoto příkazu zobrazuje vlastnosti trezoru klíčů hello, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ba335-170">hello output of this command shows properties of hello key vault that you've just created.</span></span> <span data-ttu-id="ba335-171">nejdůležitější vlastnosti Hello dva jsou:</span><span class="sxs-lookup"><span data-stu-id="ba335-171">hello two most important properties are:</span></span>

* <span data-ttu-id="ba335-172">**Název**: V příkladu hello je to ContosoKeyVault.</span><span class="sxs-lookup"><span data-stu-id="ba335-172">**Name**: In hello example this is ContosoKeyVault.</span></span> <span data-ttu-id="ba335-173">Tento název budete používat pro další rutiny Key Vault.</span><span class="sxs-lookup"><span data-stu-id="ba335-173">You will use this name for other Key Vault cmdlets.</span></span>
* <span data-ttu-id="ba335-174">**vaultUri**: V příkladu hello je to https://contosokeyvault.vault.azure.net.</span><span class="sxs-lookup"><span data-stu-id="ba335-174">**vaultUri**: In hello example this is https://contosokeyvault.vault.azure.net.</span></span> <span data-ttu-id="ba335-175">Aplikace, které používají váš trezor prostřednictvím REST API musí používat tento identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="ba335-175">Applications that use your vault through its REST API must use this URI.</span></span>

<span data-ttu-id="ba335-176">Váš účet Azure je autorizovaný tooperform žádné operace pro tento klíč trezoru.</span><span class="sxs-lookup"><span data-stu-id="ba335-176">Your Azure account is now authorized tooperform any operations on this key vault.</span></span> <span data-ttu-id="ba335-177">Zatím k tomu není oprávněn nikdo jiný.</span><span class="sxs-lookup"><span data-stu-id="ba335-177">As yet, nobody else is.</span></span>

## <a name="add-a-key-or-secret-toohello-key-vault"></a><span data-ttu-id="ba335-178">Přidat klíč nebo tajný toohello trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="ba335-178">Add a key or secret toohello key vault</span></span>

<span data-ttu-id="ba335-179">Pokud chcete Azure Key Vault toocreate klíč chráněný softwarem pro vás, použijte hello `azure key create` příkaz a zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="ba335-179">If you want Azure Key Vault toocreate a software-protected key for you, use hello `azure key create` command, and type hello following:</span></span>

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

<span data-ttu-id="ba335-180">Nicméně pokud už máte existující klíč v soubor .pem uložený jako místního souboru do souboru s názvem softkey.pem, které chcete tooupload tooAzure Key Vault, zadejte hello následujících tooimport hello klíč z hello. Soubor PEM, který chrání klíč hello softwarem hello služby Key Vault:</span><span class="sxs-lookup"><span data-stu-id="ba335-180">However, if you have an existing key in a .pem file saved as local file in a file named softkey.pem that you want tooupload tooAzure Key Vault, type hello following tooimport hello key from hello .PEM file, which protects hello key by software in hello Key Vault service:</span></span>

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

<span data-ttu-id="ba335-181">Nyní můžete odkazovat hello klíče vytvořené nebo odeslán tooAzure Key Vault, pomocí jeho identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="ba335-181">You can now reference hello key that you created or uploaded tooAzure Key Vault, by using its URI.</span></span> <span data-ttu-id="ba335-182">Použít **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways získat aktuální verzi hello a používat **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** tooget konkrétní verzi.</span><span class="sxs-lookup"><span data-stu-id="ba335-182">Use  **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways get hello current version, and use **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** tooget this specific version.</span></span>

<span data-ttu-id="ba335-183">tooadd tajný toohello úložiště, který je hesla s názvem SQLPassword a hodnotou Pa$ $w0rd tooAzure Key Vault, typ hello následující hello:</span><span class="sxs-lookup"><span data-stu-id="ba335-183">tooadd a secret toohello vault, which is a password named SQLPassword and that has hello value of Pa$$w0rd tooAzure Key Vault, type hello following:</span></span>

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

<span data-ttu-id="ba335-184">Nyní si můžete odkazy Toto heslo přidali tooAzure Key Vault, a to pomocí jeho identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="ba335-184">You can now reference this password that you added tooAzure Key Vault, by using its URI.</span></span> <span data-ttu-id="ba335-185">Použít **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways získat aktuální verzi hello a používat **https://ContosoVault.vault.azure.net/secrets/SQLPassword/ 90018dbb96a84117a0d2847ef8e7189d** tooget konkrétní verzi.</span><span class="sxs-lookup"><span data-stu-id="ba335-185">Use **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways get hello current version, and use **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** tooget this specific version.</span></span>

<span data-ttu-id="ba335-186">Podívejme se, zda text hello klíč nebo tajný klíč, který jste právě vytvořili:</span><span class="sxs-lookup"><span data-stu-id="ba335-186">Let's view hello key or secret that you just created:</span></span>

* <span data-ttu-id="ba335-187">tooview vaše, zadejte:`azure keyvault key list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="ba335-187">tooview your key, type: `azure keyvault key list --vault-name 'ContosoKeyVault'`</span></span>
* <span data-ttu-id="ba335-188">tooview váš tajný, typ:`azure keyvault secret list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="ba335-188">tooview your secret, type: `azure keyvault secret list --vault-name 'ContosoKeyVault'`</span></span>

## <a name="register-an-application-with-azure-active-directory"></a><span data-ttu-id="ba335-189">Registrujte aplikaci s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ba335-189">Register an application with Azure Active Directory</span></span>

<span data-ttu-id="ba335-190">Tento krok obvykle provádí vývojář na samostatném počítači.</span><span class="sxs-lookup"><span data-stu-id="ba335-190">This step would usually be done by a developer, on a separate computer.</span></span> <span data-ttu-id="ba335-191">Není konkrétní tooAzure Key Vault, ale je zahrnuté pro úplnost.</span><span class="sxs-lookup"><span data-stu-id="ba335-191">It is not specific tooAzure Key Vault but is included here, for completeness.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ba335-192">toocomplete hello kurzu, váš účet, hello trezoru a hello aplikace, která zaregistrujete v tomto kroku musí být ve hello stejném adresáři Azure.</span><span class="sxs-lookup"><span data-stu-id="ba335-192">toocomplete hello tutorial, your account, hello vault, and hello application that you will register in this step must all be in hello same Azure directory.</span></span>
> 
> 

<span data-ttu-id="ba335-193">Aplikace, které používají trezor klíčů, se musí ověřit pomocí tokenu z Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ba335-193">Applications that use a key vault must authenticate by using a token from Azure Active Directory.</span></span> <span data-ttu-id="ba335-194">toodo se hello vlastníka aplikace hello musí hello aplikaci nejprve zaregistrovat v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ba335-194">toodo this, hello owner of hello application must first register hello application in their Azure Active Directory.</span></span> <span data-ttu-id="ba335-195">Na hello konci registrace obdrží majitel aplikace hello hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="ba335-195">At hello end of registration, hello application owner gets hello following values:</span></span>

* <span data-ttu-id="ba335-196">**ID aplikace** (také označované jako ID klienta) a **ověřovací klíč** (také označované jako hello sdílený tajný klíč).</span><span class="sxs-lookup"><span data-stu-id="ba335-196">An **Application ID** (also known as a Client ID) and **authentication key** (also known as hello shared secret).</span></span> <span data-ttu-id="ba335-197">Hello aplikace musí být obě tyto hodnoty tooAzure služby Active Directory, tooget token.</span><span class="sxs-lookup"><span data-stu-id="ba335-197">hello application must present both of these values tooAzure Active Directory, tooget a token.</span></span> <span data-ttu-id="ba335-198">Jak aplikace hello je nakonfigurován toodo to závisí na aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="ba335-198">How hello application is configured toodo this depends on hello application.</span></span> <span data-ttu-id="ba335-199">Pro hello ukázkovou aplikaci Key Vault nastavuje majitel aplikace hello tyto hodnoty v souboru app.config hello.</span><span class="sxs-lookup"><span data-stu-id="ba335-199">For hello Key Vault sample application, hello application owner sets these values in hello app.config file.</span></span>

<span data-ttu-id="ba335-200">tooregister hello aplikace v Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="ba335-200">tooregister hello application in Azure Active Directory:</span></span>

1. <span data-ttu-id="ba335-201">Přihlaste se toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba335-201">Sign in toohello Azure portal.</span></span>
2. <span data-ttu-id="ba335-202">Na levé straně hello, klikněte na tlačítko **služby Active Directory**a potom vyberte hello adresář, ve kterém zaregistrujete vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba335-202">On hello left, click **Active Directory**, and then select hello directory in which you will register your application.</span></span> <br> <br> 

>[!NOTE] 
> <span data-ttu-id="ba335-203">Je nutné vybrat hello hello stejný adresář, který obsahuje předplatné Azure, který jste použili pro vytvoření trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="ba335-203">You must select hello same directory that contains hello Azure subscription with which you created your key vault.</span></span> <span data-ttu-id="ba335-204">Pokud si nejste jisti který adresář to je, klikněte na tlačítko **nastavení**, určete hello předplatné, které jste použili pro vytvoření trezoru klíčů, a Poznámka hello název adresáře hello zobrazí v posledním sloupci hello.</span><span class="sxs-lookup"><span data-stu-id="ba335-204">If you do not know which directory this is, click **Settings**, identify hello subscription with which you created your key vault, and note hello name of hello directory displayed in hello last column.</span></span>

3. <span data-ttu-id="ba335-205">Klikněte na **Aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ba335-205">Click **APPLICATIONS**.</span></span> <span data-ttu-id="ba335-206">Pokud nebyly přidané žádné aplikace tooyour directory, tato stránka se zobrazí pouze hello **přidat aplikaci** odkaz.</span><span class="sxs-lookup"><span data-stu-id="ba335-206">If no apps have been added tooyour directory, this page will show only hello **Add an App** link.</span></span> <span data-ttu-id="ba335-207">Kliknutím na odkaz hello nebo, případně můžete kliknutím na tlačítko hello **přidat** na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="ba335-207">Click hello link, or alternatively, you can click hello **ADD** on hello command bar.</span></span>
4. <span data-ttu-id="ba335-208">V hello **přidat aplikaci** na hello **co chcete toodo?** klikněte na tlačítko **přidat aplikaci, kterou vyvíjí Moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="ba335-208">In hello **ADD APPLICATION** wizard, on hello **What do you want toodo?** page, click **Add an application my organization is developing**.</span></span>
5. <span data-ttu-id="ba335-209">Na hello **Řekněte nám o své aplikaci** , zadejte název pro vaši aplikaci a vyberte **webové aplikace nebo webové rozhraní API** (hello výchozí).</span><span class="sxs-lookup"><span data-stu-id="ba335-209">On hello **Tell us about your application** page, specify a name for your application and select **WEB APPLICATION AND/OR WEB API** (hello default).</span></span> <span data-ttu-id="ba335-210">Kliknutím na ikonu další hello.</span><span class="sxs-lookup"><span data-stu-id="ba335-210">Click hello Next icon.</span></span>
6. <span data-ttu-id="ba335-211">Na hello **vlastností aplikace** zadejte hello **adresa URL přihlašování** a **identifikátor ID URI aplikace** pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ba335-211">On hello **App properties** page, specify hello **SIGN-ON URL** and **APP ID URI** for your web application.</span></span> <span data-ttu-id="ba335-212">Pokud vaše aplikace tyto hodnoty neobsahuje, v tomto kroku si je můžete vymyslet (například můžete do obou polí zadat http://test1.contoso.com).</span><span class="sxs-lookup"><span data-stu-id="ba335-212">If your application does not have these values, you can make them up for this step (for example, you could specify http://test1.contoso.com for both boxes).</span></span> <span data-ttu-id="ba335-213">Není důležité, pokud tyto stránky existují; Co je důležité je, že tuto aplikaci hello identifikátor ID URI pro každou aplikaci se liší pro každou aplikaci ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="ba335-213">It does not matter if these sites exist; what is important is that hello app ID URI for each application is different for every application in your directory.</span></span> <span data-ttu-id="ba335-214">Hello directory používá tento řetězec tooidentify vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba335-214">hello directory uses this string tooidentify your app.</span></span>
7. <span data-ttu-id="ba335-215">Klikněte na tlačítko hello dokončení ikonu toosave změny v Průvodci hello.</span><span class="sxs-lookup"><span data-stu-id="ba335-215">Click hello Complete icon toosave your changes in hello wizard.</span></span>
8. <span data-ttu-id="ba335-216">Na stránce hello rychlý Start, klikněte na **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="ba335-216">On hello Quick Start page, click **CONFIGURE**.</span></span>
9. <span data-ttu-id="ba335-217">Posuňte se toohello **klíče** vyberte dobu trvání hello a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ba335-217">Scroll toohello **keys** section, select hello duration, and then click **SAVE**.</span></span> <span data-ttu-id="ba335-218">stránku Hello se obnoví a zobrazí hodnotu klíče.</span><span class="sxs-lookup"><span data-stu-id="ba335-218">hello page refreshes and now shows a key value.</span></span> <span data-ttu-id="ba335-219">Vaši aplikaci je nutné nakonfigurovat s touto hodnotou klíče a hello **ID klienta** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ba335-219">You must configure your application with this key value and hello **CLIENT ID** value.</span></span> <span data-ttu-id="ba335-220">(Pokyny k této konfiguraci se budou lišit podle aplikace.)</span><span class="sxs-lookup"><span data-stu-id="ba335-220">(Instructions for this configuration will be application-specific.)</span></span>
10. <span data-ttu-id="ba335-221">Zkopírujte hodnotu ID klienta hello z této stránky, které budete používat v hello další krok tooset oprávnění pro váš trezoru.</span><span class="sxs-lookup"><span data-stu-id="ba335-221">Copy hello client ID value from this page, which you will use in hello next step tooset permissions on your vault.</span></span>

## <a name="authorize-hello-application-toouse-hello-key-or-secret"></a><span data-ttu-id="ba335-222">Autorizovat hello aplikace toouse hello klíče nebo tajného klíče</span><span class="sxs-lookup"><span data-stu-id="ba335-222">Authorize hello application toouse hello key or secret</span></span>
<span data-ttu-id="ba335-223">tooaccess aplikace hello tooauthorize hello klíče nebo tajného klíče v trezoru hello, použijte hello `azure keyvault set-policy` příkaz.</span><span class="sxs-lookup"><span data-stu-id="ba335-223">tooauthorize hello application tooaccess hello key or secret in hello vault, use hello `azure keyvault set-policy` command.</span></span>

<span data-ttu-id="ba335-224">Například pokud je název vaší trezoru ContosoKeyVault a hello aplikace, které chcete tooauthorize má hodnotu 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed ID klienta a chcete toodecrypt aplikace hello tooauthorize a přihlaste s klíči v trezoru, spusťte hello následující:</span><span class="sxs-lookup"><span data-stu-id="ba335-224">For example, if your vault name is ContosoKeyVault and hello application you want tooauthorize has a client ID of 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, and you want tooauthorize hello application toodecrypt and sign with keys in your vault, then run hello following:</span></span>

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '[\"decrypt\",\"sign\"]'

> [!NOTE]
> <span data-ttu-id="ba335-225">Pokud používáte na příkazovém řádku Windows, doporučujeme nahradit jednoduchých uvozovek a být dvojité uvozovky a také řídicí hello interní dvojité uvozovky.</span><span class="sxs-lookup"><span data-stu-id="ba335-225">If you are running on Windows command prompt, you should replace single quotes with double quotes, and also escape hello internal double quotes.</span></span> <span data-ttu-id="ba335-226">Například: "[\"dešifrovat\",\"přihlašovací\"]".</span><span class="sxs-lookup"><span data-stu-id="ba335-226">For example: "[\"decrypt\",\"sign\"]".</span></span>
> 
> 

<span data-ttu-id="ba335-227">Pokud chcete, tooauthorize Tento stejný tooread tajné klíče aplikace ve vašem trezoru, spusťte hello následující:</span><span class="sxs-lookup"><span data-stu-id="ba335-227">If you want tooauthorize that same application tooread secrets in your vault, run hello following:</span></span>

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '[\"get\"]'

## <a name="if-you-want-toouse-a-hardware-security-module-hsm"></a><span data-ttu-id="ba335-228">Pokud chcete, aby toouse modul hardwarového zabezpečení (HSM)</span><span class="sxs-lookup"><span data-stu-id="ba335-228">If you want toouse a hardware security module (HSM)</span></span>
<span data-ttu-id="ba335-229">Pro lepší kontrolu můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM), které nikdy neopustí hranice HSM hello.</span><span class="sxs-lookup"><span data-stu-id="ba335-229">For added assurance, you can import or generate keys in hardware security modules (HSMs) that never leave hello HSM boundary.</span></span> <span data-ttu-id="ba335-230">Hello moduly hardwarového zabezpečení jsou FIPS 140-2 Level 2 ověřit.</span><span class="sxs-lookup"><span data-stu-id="ba335-230">hello HSMs are FIPS 140-2 Level 2 validated.</span></span> <span data-ttu-id="ba335-231">Pokud tento požadavek netýká tooyou, tuto část přeskočte a přejděte příliš[odstranit hello trezoru klíčů, přidružených klíčů a tajných klíčů](#delete-the-key-vault-and-associated-keys-and-secrets).</span><span class="sxs-lookup"><span data-stu-id="ba335-231">If this requirement doesn't apply tooyou, skip this section and go too[Delete hello key vault and associated keys and secrets](#delete-the-key-vault-and-associated-keys-and-secrets).</span></span>

<span data-ttu-id="ba335-232">toocreate těchto klíčů chráněných pomocí HSM musíte mít předplatné trezoru s podporou klíčů chráněných pomocí HSM.</span><span class="sxs-lookup"><span data-stu-id="ba335-232">toocreate these HSM-protected keys, you must have a vault subscription that supports HSM-protected keys.</span></span>

<span data-ttu-id="ba335-233">Když vytvoříte hello keyvault, přidejte parametr "sku" hello:</span><span class="sxs-lookup"><span data-stu-id="ba335-233">When you create hello keyvault, add hello 'sku' parameter:</span></span>

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

<span data-ttu-id="ba335-234">Můžete přidat klíče chráněné softwarem (jak je uvedeno výše) a trezoru klíčů chráněných pomocí HSM toothis.</span><span class="sxs-lookup"><span data-stu-id="ba335-234">You can add software-protected keys (as shown earlier) and HSM-protected keys toothis vault.</span></span> <span data-ttu-id="ba335-235">toocreate klíč chráněný HSM, sada hello cílový parametr too'HSM':</span><span class="sxs-lookup"><span data-stu-id="ba335-235">toocreate an HSM-protected key, set hello Destination parameter too'HSM':</span></span>

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

<span data-ttu-id="ba335-236">Můžete použít následující příkaz tooimport klíč ze souboru .pem ve vašem počítači hello.</span><span class="sxs-lookup"><span data-stu-id="ba335-236">You can use hello following command tooimport a key from a .pem file on your computer.</span></span> <span data-ttu-id="ba335-237">Tento příkaz importuje hello klíč do HSM ve hello služby Key Vault:</span><span class="sxs-lookup"><span data-stu-id="ba335-237">This command imports hello key into HSMs in hello Key Vault service:</span></span>

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

<span data-ttu-id="ba335-238">Hello další příkaz importuje "přineste si vlastní klíč" (BYOK) balíčku.</span><span class="sxs-lookup"><span data-stu-id="ba335-238">hello next command imports a “bring your own key" (BYOK) package.</span></span> <span data-ttu-id="ba335-239">To umožňuje vygenerovat klíč v místním HSM a jeho přenesení tooHSMs v hello služby Key Vault, bez hello klíč opustil hranice HSM hello:</span><span class="sxs-lookup"><span data-stu-id="ba335-239">This lets you generate your key in your local HSM, and transfer it tooHSMs in hello Key Vault service, without hello key leaving hello HSM boundary:</span></span>

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

<span data-ttu-id="ba335-240">Podrobnější pokyny, jak toogenerate tento balíček BYOK najdete v části [jak toouse HSM-Protected klíčů s Azure Key Vault](key-vault-hsm-protected-keys.md).</span><span class="sxs-lookup"><span data-stu-id="ba335-240">For more detailed instructions about how toogenerate this BYOK package, see [How toouse HSM-Protected Keys with Azure Key Vault](key-vault-hsm-protected-keys.md).</span></span>

## <a name="delete-hello-key-vault-and-associated-keys-and-secrets"></a><span data-ttu-id="ba335-241">Odstranit hello trezoru klíčů, přidružených klíčů a tajných klíčů</span><span class="sxs-lookup"><span data-stu-id="ba335-241">Delete hello key vault and associated keys and secrets</span></span>
<span data-ttu-id="ba335-242">Pokud již nepotřebujete trezor klíčů hello a hello klíč nebo tajný klíč, který obsahuje, je hello trezor klíčů odstranit pomocí příkazu delete keyvault hello azure:</span><span class="sxs-lookup"><span data-stu-id="ba335-242">If you no longer need hello key vault and hello key or secret that it contains, you can delete hello key vault by using hello azure keyvault delete command:</span></span>

    azure keyvault delete --vault-name 'ContosoKeyVault'

<span data-ttu-id="ba335-243">Nebo můžete odstranit skupinu prostředků celý Azure, která zahrnuje trezor klíčů hello a další prostředky, které jste zahrnuli do této skupiny:</span><span class="sxs-lookup"><span data-stu-id="ba335-243">Or, you can delete an entire Azure resource group, which includes hello key vault and any other resources that you included in that group:</span></span>

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a><span data-ttu-id="ba335-244">Ostatní příkazy rozhraní příkazového řádku Azure a platformy</span><span class="sxs-lookup"><span data-stu-id="ba335-244">Other Azure Cross-Platform Command-line Interface Commands</span></span>
<span data-ttu-id="ba335-245">Další příkazy, že může využít ke správě Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="ba335-245">Other commands that you might useful for managing Azure Key Vault.</span></span>

<span data-ttu-id="ba335-246">Tento příkaz vypíše tabulkové zobrazení všech klíčů a vybraných vlastností:</span><span class="sxs-lookup"><span data-stu-id="ba335-246">This command lists a tabular display of all keys and selected properties:</span></span>

    azure keyvault key list --vault-name 'ContosoKeyVault'

<span data-ttu-id="ba335-247">Tento příkaz zobrazí úplný seznam vlastností pro zadaný klíč hello:</span><span class="sxs-lookup"><span data-stu-id="ba335-247">This command displays a full list of properties for hello specified key:</span></span>

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

<span data-ttu-id="ba335-248">Tento příkaz vypíše tabulkové zobrazení všech názvů tajných klíčů a vybraných vlastností:</span><span class="sxs-lookup"><span data-stu-id="ba335-248">This command lists a tabular display of all secret names and selected properties:</span></span>

    azure keyvault secret list --vault-name 'ContosoKeyVault'

<span data-ttu-id="ba335-249">Tady je příklad toho, jak tooremove konkrétního klíče:</span><span class="sxs-lookup"><span data-stu-id="ba335-249">Here's an example of how tooremove a specific key:</span></span>

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

<span data-ttu-id="ba335-250">Tady je příklad toho, jak tooremove určitého tajného klíče:</span><span class="sxs-lookup"><span data-stu-id="ba335-250">Here's an example of how tooremove a specific secret:</span></span>

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a><span data-ttu-id="ba335-251">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ba335-251">Next steps</span></span>
<span data-ttu-id="ba335-252">Programátorské reference najdete v části [hello Příručka pro vývojáře Azure Key Vault](key-vault-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="ba335-252">For programming references, see [hello Azure Key Vault developer's guide](key-vault-developers-guide.md).</span></span>

