---
title: "aaaGet začít s Azure CLI pro dávku | Microsoft Docs"
description: "Rychlý úvod toohello Batch příkazy Get v rozhraní příkazového řádku Azure pro správu prostředků služby Azure Batch"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: fcd76587-1827-4bc8-a84d-bba1cd980d85
ms.service: batch
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 14f28311ecb16c8097d0d304a4ad89de282a2e9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-batch-resources-with-azure-cli"></a><span data-ttu-id="b3bb0-103">Správa prostředků služby Batch pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b3bb0-103">Manage Batch resources with Azure CLI</span></span>

<span data-ttu-id="b3bb0-104">Hello Azure CLI 2.0 je Azure nové prostředí příkazového řádku pro správu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-104">hello Azure CLI 2.0 is Azure's new command-line experience for managing Azure resources.</span></span> <span data-ttu-id="b3bb0-105">Je možné používat ho v systémech macOS, Linux a Windows.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-105">It can be used on macOS, Linux, and Windows.</span></span> <span data-ttu-id="b3bb0-106">Azure CLI 2.0 je optimalizovaná pro správu a Správa prostředků Azure z příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-106">Azure CLI 2.0 is optimized for managing and administering Azure resources from hello command line.</span></span> <span data-ttu-id="b3bb0-107">Toomanage hello rozhraní příkazového řádku Azure můžete použít účty Azure Batch a toomanage prostředkům, například fondy, úlohy a úkoly.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-107">You can use hello Azure CLI toomanage your Azure Batch accounts and toomanage resources such as pools, jobs, and tasks.</span></span> <span data-ttu-id="b3bb0-108">S hello rozhraní příkazového řádku Azure, můžete skript řadu hello hello stejné úlohy, které se provádějí pomocí rozhraní API služby Batch, portálu Azure a rutiny prostředí PowerShell Batch.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-108">With hello Azure CLI, you can script many of hello same tasks you carry out with hello Batch APIs, Azure portal, and Batch PowerShell cmdlets.</span></span>

<span data-ttu-id="b3bb0-109">Tento článek obsahuje přehled používání rozhraní [Azure CLI verze 2.0](https://docs.microsoft.com/cli/azure/overview) se službou Batch.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-109">This article provides an overview of using [Azure CLI version 2.0](https://docs.microsoft.com/cli/azure/overview) with Batch.</span></span> <span data-ttu-id="b3bb0-110">V tématu [Začínáme s Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) přehled pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-110">See [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) for an overview of using hello CLI with Azure.</span></span>

<span data-ttu-id="b3bb0-111">Společnost Microsoft doporučuje používat nejnovější verzi hello hello rozhraní příkazového řádku Azure, verze 2.0.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-111">Microsoft recommends using hello latest version of hello Azure CLI, version 2.0.</span></span> <span data-ttu-id="b3bb0-112">Další informace o verzi 2.0 najdete v článku [Příkazový řádek Azure 2.0 je nyní veřejně k dispozici](https://azure.microsoft.com/blog/announcing-general-availability-of-vm-storage-and-network-azure-cli-2-0/).</span><span class="sxs-lookup"><span data-stu-id="b3bb0-112">For more information about version 2.0, see [Azure Command Line 2.0 now generally available](https://azure.microsoft.com/blog/announcing-general-availability-of-vm-storage-and-network-azure-cli-2-0/).</span></span>

## <a name="set-up-hello-azure-cli"></a><span data-ttu-id="b3bb0-113">Nastavit hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="b3bb0-113">Set up hello Azure CLI</span></span>

<span data-ttu-id="b3bb0-114">tooinstall hello rozhraní příkazového řádku Azure, postupujte podle kroků hello uvedených v [hello instalace rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/install-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b3bb0-114">tooinstall hello Azure CLI, follow hello steps outlined in [Install hello Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli.md).</span></span>

> [!TIP]
> <span data-ttu-id="b3bb0-115">Doporučujeme aktualizovat vaše instalace rozhraní příkazového řádku Azure často tootake výhod aktualizace a vylepšení služby.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-115">We recommend that you update your Azure CLI installation frequently tootake advantage of service updates and enhancements.</span></span>
> 
> 

## <a name="command-help"></a><span data-ttu-id="b3bb0-116">Nápověda k příkazům</span><span class="sxs-lookup"><span data-stu-id="b3bb0-116">Command help</span></span>

<span data-ttu-id="b3bb0-117">Můžete zobrazit text nápovědy pro každý příkaz v hello rozhraní příkazového řádku Azure připojením `-h` toohello příkaz.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-117">You can display help text for every command in hello Azure CLI by appending `-h` toohello command.</span></span> <span data-ttu-id="b3bb0-118">Jiné parametry vynechejte.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-118">Omit any other options.</span></span> <span data-ttu-id="b3bb0-119">Například:</span><span class="sxs-lookup"><span data-stu-id="b3bb0-119">For example:</span></span>

* <span data-ttu-id="b3bb0-120">tooget nápovědu pro hello `az` příkazu, zadejte:`az -h`</span><span class="sxs-lookup"><span data-stu-id="b3bb0-120">tooget help for hello `az` command, enter: `az -h`</span></span>
* <span data-ttu-id="b3bb0-121">tooget seznam všech příkazů Batch v hello rozhraní příkazového řádku, použijte:`az batch -h`</span><span class="sxs-lookup"><span data-stu-id="b3bb0-121">tooget a list of all Batch commands in hello CLI, use: `az batch -h`</span></span>
* <span data-ttu-id="b3bb0-122">tooget nápovědu k vytvoření účtu Batch, zadejte:`az batch account create -h`</span><span class="sxs-lookup"><span data-stu-id="b3bb0-122">tooget help on creating a Batch account, enter: `az batch account create -h`</span></span>

<span data-ttu-id="b3bb0-123">Pokud máte pochybnosti, použijte hello `-h` možnost příkazového řádku tooget nápovědu k libovolnému příkazu příkazového řádku Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-123">When in doubt, use hello `-h` command-line option tooget help on any Azure CLI command.</span></span>

> [!NOTE]
> <span data-ttu-id="b3bb0-124">Starší verze hello používá rozhraní příkazového řádku Azure `azure` toopreface příkaz rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-124">Earlier versions of hello Azure CLI used `azure` toopreface a CLI command.</span></span> <span data-ttu-id="b3bb0-125">Ve verzi 2.0 začínají všechny příkazy příznakem `az`.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-125">In version 2.0, all commands are now prefaced with `az`.</span></span> <span data-ttu-id="b3bb0-126">Být jisti tooupdate skripty toouse hello novou syntaxi s verze 2.0.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-126">Be sure tooupdate your scripts toouse hello new syntax with version 2.0.</span></span>
>
>  

<span data-ttu-id="b3bb0-127">Kromě toho naleznete v dokumentaci referenční dokumentace rozhraní příkazového řádku Azure toohello podrobnosti o [rozhraní příkazového řádku Azure pro dávku](https://docs.microsoft.com/cli/azure/batch).</span><span class="sxs-lookup"><span data-stu-id="b3bb0-127">Additionally, refer toohello Azure CLI reference documentation for details about [Azure CLI commands for Batch](https://docs.microsoft.com/cli/azure/batch).</span></span> 

## <a name="log-in-and-authenticate"></a><span data-ttu-id="b3bb0-128">Přihlášení a ověření</span><span class="sxs-lookup"><span data-stu-id="b3bb0-128">Log in and authenticate</span></span>

<span data-ttu-id="b3bb0-129">toouse hello rozhraní příkazového řádku Azure pomocí služby Batch, potřebujete toolog v a ověření.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-129">toouse hello Azure CLI with Batch, you need toolog in and authenticate.</span></span> <span data-ttu-id="b3bb0-130">Existují dvě toofollow jednoduchých kroků:</span><span class="sxs-lookup"><span data-stu-id="b3bb0-130">There are two simple steps toofollow:</span></span>

1. <span data-ttu-id="b3bb0-131">**Přihlaste se k Azure.**</span><span class="sxs-lookup"><span data-stu-id="b3bb0-131">**Log into Azure.**</span></span> <span data-ttu-id="b3bb0-132">Protokolování do Azure poskytuje přístup příkazů tooAzure Resource Manager, včetně [služba Batch Management](batch-management-dotnet.md) příkazy.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-132">Logging into Azure gives you access tooAzure Resource Manager commands, including [Batch Management service](batch-management-dotnet.md) commands.</span></span>  
2. <span data-ttu-id="b3bb0-133">**Přihlaste se ke svému účtu Batch.**</span><span class="sxs-lookup"><span data-stu-id="b3bb0-133">**Log into your Batch account.**</span></span> <span data-ttu-id="b3bb0-134">Protokolování do vašeho dávkového účtu poskytuje přístup k tooBatch služby příkazy.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-134">Logging into your Batch account gives you access tooBatch service commands.</span></span>   

### <a name="log-in-tooazure"></a><span data-ttu-id="b3bb0-135">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="b3bb0-135">Log in tooAzure</span></span>

<span data-ttu-id="b3bb0-136">Existuje několik různých způsobů toolog do Azure, které jsou podrobně popsány v [Přihlaste se pomocí Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli):</span><span class="sxs-lookup"><span data-stu-id="b3bb0-136">There are a few different ways toolog into Azure, described in detail in [Log in with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli):</span></span>

1. <span data-ttu-id="b3bb0-137">[Interaktivní přihlášení](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#interactive-log-in):</span><span class="sxs-lookup"><span data-stu-id="b3bb0-137">[Log in interactively](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#interactive-log-in).</span></span> <span data-ttu-id="b3bb0-138">Přihlaste interaktivně Pokud používáte rozhraní příkazového řádku Azure sami z příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-138">Log in interactively when you are running Azure CLI commands yourself from hello command line.</span></span>
2. <span data-ttu-id="b3bb0-139">[Přihlášení pomocí instančního objektu](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#logging-in-with-a-service-principal):</span><span class="sxs-lookup"><span data-stu-id="b3bb0-139">[Log in with a service principal](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#logging-in-with-a-service-principal).</span></span> <span data-ttu-id="b3bb0-140">Pokud spouštíte příkazy rozhraní příkazového řádku Azure CLI ze skriptu nebo aplikace, přihlaste se pomocí instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-140">Log in with a service principal when you are running Azure CLI commands from a script or an application.</span></span>

<span data-ttu-id="b3bb0-141">Pro účely hello tohoto článku, ukážeme, jak toolog do Azure interaktivně.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-141">For hello purposes of this article, we show how toolog into Azure interactively.</span></span> <span data-ttu-id="b3bb0-142">Typ [az přihlášení](https://docs.microsoft.com/cli/azure/#login) na příkazovém řádku hello:</span><span class="sxs-lookup"><span data-stu-id="b3bb0-142">Type [az login](https://docs.microsoft.com/cli/azure/#login) on hello command line:</span></span>

```azurecli
# Log in tooAzure and authenticate interactively.
az login
```

<span data-ttu-id="b3bb0-143">Hello `az login` příkaz vrátí token tooauthenticate, můžete použít, jak je vidět tady.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-143">hello `az login` command returns a token that you can use tooauthenticate, as shown here.</span></span> <span data-ttu-id="b3bb0-144">Postupujte podle pokynů tooopen hello webové stránky a odeslat hello token tooAzure:</span><span class="sxs-lookup"><span data-stu-id="b3bb0-144">Follow hello instructions provided tooopen a web page and submit hello token tooAzure:</span></span>

![Přihlaste se tooAzure](./media/batch-cli-get-started/az-login.png)

<span data-ttu-id="b3bb0-146">Příklady Hello uvedené v hello [ukázkové skripty prostředí](#sample-shell-scripts) tématu zobrazit také jak toostart relaci příkazového řádku Azure CLI interaktivně protokolováním do Azure.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-146">hello examples listed in hello [Sample shell scripts](#sample-shell-scripts) section also show how toostart your Azure CLI session by logging into Azure interactively.</span></span> <span data-ttu-id="b3bb0-147">Jakmile jste přihlášení, můžete volat příkazy toowork s Batch správy prostředků, včetně účty Batch, klíče, balíčky aplikací a kvóty.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-147">Once you have logged in, you can call commands toowork with Batch Management resources, including Batch accounts, keys, application packages, and quotas.</span></span>  

### <a name="log-in-tooyour-batch-account"></a><span data-ttu-id="b3bb0-148">Přihlaste se tooyour účtu Batch</span><span class="sxs-lookup"><span data-stu-id="b3bb0-148">Log in tooyour Batch account</span></span>

<span data-ttu-id="b3bb0-149">toouse hello rozhraní příkazového řádku Azure toomanage prostředky Batch, například fondy, úlohy a úkoly, je nutné toolog do vašeho účtu Batch a ověření.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-149">toouse hello Azure CLI toomanage Batch resources, such as pools, jobs, and tasks, you need toolog into your Batch account and authenticate.</span></span> <span data-ttu-id="b3bb0-150">toolog v toohello služby Batch, použijte hello [připojení k účtu batch az](https://docs.microsoft.com/cli/azure/batch/account#login) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-150">toolog in toohello Batch service, use hello [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#login) command.</span></span> 

<span data-ttu-id="b3bb0-151">Máte dvě možnosti ověření proti účtu Batch:</span><span class="sxs-lookup"><span data-stu-id="b3bb0-151">You have two options for authenticating against your Batch account:</span></span>

- <span data-ttu-id="b3bb0-152">**Ověření pomocí služby Azure Active Directory (Azure AD)**</span><span class="sxs-lookup"><span data-stu-id="b3bb0-152">**By using Azure Active Directory (Azure AD) authentication.**</span></span> 

    <span data-ttu-id="b3bb0-153">Ověřování s Azure AD při použití hello rozhraní příkazového řádku Azure pomocí služby Batch je hello výchozí a doporučuje pro většinu scénářů.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-153">Authenticating with Azure AD is hello default when you use hello Azure CLI with Batch, and recommended for most scenarios.</span></span> 
    
    <span data-ttu-id="b3bb0-154">Při přihlášení tooAzure interaktivně, jak je popsáno v předchozí části hello, vaše přihlašovací údaje jsou ukládány do mezipaměti, tedy hello rozhraní příkazového řádku Azure můžete přihlásit tooyour pomocí těchto stejné přihlašovací údaje účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-154">When you log in tooAzure interactively, as described in hello previous section, your credentials are cached, so hello Azure CLI can log you in tooyour Batch account using those same credentials.</span></span> <span data-ttu-id="b3bb0-155">Pokud se přihlásit pomocí objektu služby tooAzure, tyto přihlašovací údaje jsou také použít toolog v tooyour účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-155">If you log in tooAzure using a service principal, those credentials are also used toolog in tooyour Batch account.</span></span>

    <span data-ttu-id="b3bb0-156">Výhoda služby Azure AD je, že nabízí řízení přístupu na základě role (RBAC).</span><span class="sxs-lookup"><span data-stu-id="b3bb0-156">An advantage of Azure AD is that it offers role-based access control (RBAC).</span></span> <span data-ttu-id="b3bb0-157">S RBAC závisí přístup uživatele na jejich přiřazené role místo, jestli budou mít klíče účtu hello.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-157">With RBAC, a user's access depends on their assigned role, rather than whether or not they possess hello account keys.</span></span> <span data-ttu-id="b3bb0-158">Místo správy klíčů účtu můžete spravovat role RBAC a nechat řízení přístupu a ověřování na službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-158">Instead of managing account keys, you can manage RBAC roles, and let Azure AD handle access and authentication.</span></span>  

    <span data-ttu-id="b3bb0-159">Ověřování s Azure AD je povinný, pokud jste vytvořili nastavení vašeho účtu Azure Batch pomocí jeho režim přidělení fondu too'User předplatné '.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-159">Authenticating with Azure AD is required if you created your Azure Batch account with its pool allocation mode set too'User Subscription'.</span></span> 

    <span data-ttu-id="b3bb0-160">toolog v tooyour dávkového účtu pomocí Azure AD, volejte hello [připojení k účtu batch az](https://docs.microsoft.com/cli/azure/batch/account#login) příkaz:</span><span class="sxs-lookup"><span data-stu-id="b3bb0-160">toolog in tooyour Batch account using Azure AD, call hello [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#login) command:</span></span> 

    ```azurecli
    az batch account login -g myresource group -n mybatchaccount
    ```

- <span data-ttu-id="b3bb0-161">**Ověření pomocí sdíleného klíče**</span><span class="sxs-lookup"><span data-stu-id="b3bb0-161">**By using Shared Key authentication.**</span></span>

    <span data-ttu-id="b3bb0-162">[Ověření pomocí sdíleného klíče](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service#authentication-via-shared-key) služba Batch používá příkazy příkazového řádku Azure CLI tooauthenticate pro hello klíčů pro váš účet přístup.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-162">[Shared Key authentication](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service#authentication-via-shared-key) uses your account access keys tooauthenticate Azure CLI commands for hello Batch service.</span></span>

    <span data-ttu-id="b3bb0-163">Pokud vytváříte skripty rozhraní příkazového řádku Azure Batch příkazy volání tooautomate, můžete použít ověření sdíleným klíčem nebo hlavní služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-163">If you are creating Azure CLI scripts tooautomate calling Batch commands, you can use either Shared Key authentication, or an Azure AD service principal.</span></span> <span data-ttu-id="b3bb0-164">V některých scénářích může být ověření pomocí sdíleného klíče jednodušší než vytvoření instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-164">In some scenarios, using Shared Key authentication may be simpler than creating a service principal.</span></span>  

    <span data-ttu-id="b3bb0-165">toolog pomocí ověření sdíleným klíčem, zahrnují hello `--shared-key-auth` možnost na příkazovém řádku hello:</span><span class="sxs-lookup"><span data-stu-id="b3bb0-165">toolog in using Shared Key authentication, include hello `--shared-key-auth` option on hello command line:</span></span>

    ```azurecli
    az batch account login -g myresourcegroup -n mybatchaccount --shared-key-auth
    ```

<span data-ttu-id="b3bb0-166">Příklady Hello uvedené v hello [ukázkové skripty prostředí](#sample-shell-scripts) části ukazují, jak toolog do vašeho účtu Batch s hello rozhraní příkazového řádku Azure pomocí Azure AD a sdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-166">hello examples listed in hello [Sample shell scripts](#sample-shell-scripts) section show how toolog into your Batch account with hello Azure CLI using both Azure AD and Shared Key.</span></span>

## <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a><span data-ttu-id="b3bb0-167">Použití šablon rozhraní příkazového řádku služby Batch a přenos souborů (Preview)</span><span class="sxs-lookup"><span data-stu-id="b3bb0-167">Use Azure Batch CLI Templates and File Transfer (Preview)</span></span>

<span data-ttu-id="b3bb0-168">Můžete vytvořit hello rozhraní příkazového řádku Azure toorun dávkové úlohy na kompletní bez nutnosti psaní kódu.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-168">You can use hello Azure CLI toorun Batch jobs end-to-end without writing code.</span></span> <span data-ttu-id="b3bb0-169">Dávkové soubory šablony podporují vytváření fondů, úloh a úloh s hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-169">Batch template files support creating pools, jobs, and tasks with hello Azure CLI.</span></span> <span data-ttu-id="b3bb0-170">Můžete taky hello rozhraní příkazového řádku Azure tooupload úlohy vstupní soubory toohello účet úložiště Azure přidružený hello účet Batch a z něj stahují úlohy výstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-170">You can also use hello Azure CLI tooupload job input files toohello Azure Storage account associated with hello Batch account, and download job output files from it.</span></span> <span data-ttu-id="b3bb0-171">Další informace najdete v tématu [Použití šablon rozhraní příkazového řádku služby Batch a přenos souborů (Preview)](batch-cli-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b3bb0-171">For more information, see [Use Azure Batch CLI Templates and File Transfer (Preview)](batch-cli-templates.md).</span></span>

## <a name="sample-shell-scripts"></a><span data-ttu-id="b3bb0-172">Ukázkové skripty prostředí</span><span class="sxs-lookup"><span data-stu-id="b3bb0-172">Sample shell scripts</span></span>

<span data-ttu-id="b3bb0-173">Ukázkové skripty Hello uvedené v následující tabulce zobrazit text hello, jak příkazy toouse rozhraní příkazového řádku Azure pomocí služby Batch hello a běžných úlohách tooaccomplish Batch Management service.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-173">hello sample scripts listed in hello following table show how toouse Azure CLI commands with hello Batch service and Batch Management service tooaccomplish common tasks.</span></span> <span data-ttu-id="b3bb0-174">Tyto ukázkové skripty zahrnují řadu hello příkazy, které jsou k dispozici v hello rozhraní příkazového řádku Azure pro dávku.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-174">These sample scripts cover many of hello commands available in hello Azure CLI for Batch.</span></span> 

| <span data-ttu-id="b3bb0-175">Skript</span><span class="sxs-lookup"><span data-stu-id="b3bb0-175">Script</span></span> | <span data-ttu-id="b3bb0-176">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b3bb0-176">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b3bb0-177">Vytvoření účtu služby Batch</span><span class="sxs-lookup"><span data-stu-id="b3bb0-177">Create a Batch account</span></span>](./scripts/batch-cli-sample-create-account.md) | <span data-ttu-id="b3bb0-178">Vytvoří účet Batch a přidruží ho k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-178">Creates a Batch account and associates it with a storage account.</span></span> |
| [<span data-ttu-id="b3bb0-179">Přidání aplikace</span><span class="sxs-lookup"><span data-stu-id="b3bb0-179">Add an application</span></span>](./scripts/batch-cli-sample-add-application.md) | <span data-ttu-id="b3bb0-180">Přidá aplikaci a nahraje zabalené binární soubory.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-180">Adds an application and uploads packaged binaries.</span></span>|
| [<span data-ttu-id="b3bb0-181">Správa fondů služby Batch</span><span class="sxs-lookup"><span data-stu-id="b3bb0-181">Manage Batch pools</span></span>](./scripts/batch-cli-sample-manage-pool.md) | <span data-ttu-id="b3bb0-182">Ukazuje vytváření, změny velikosti a správu fondů.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-182">Demonstrates creating, resizing, and managing pools.</span></span> |
| [<span data-ttu-id="b3bb0-183">Spuštění úlohy a úkolů pomocí služby Batch</span><span class="sxs-lookup"><span data-stu-id="b3bb0-183">Run a job and tasks with Batch</span></span>](./scripts/batch-cli-sample-run-job.md) | <span data-ttu-id="b3bb0-184">Ukazuje spuštění úlohy a přidávání úkolů.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-184">Demonstrates running a job and adding tasks.</span></span> |

## <a name="json-files-for-resource-creation"></a><span data-ttu-id="b3bb0-185">Soubory JSON pro vytváření prostředků</span><span class="sxs-lookup"><span data-stu-id="b3bb0-185">JSON files for resource creation</span></span>

<span data-ttu-id="b3bb0-186">Když vytvoříte prostředky Batch jako fondů a úloh, můžete zadat soubor JSON obsahující konfiguraci hello nový prostředek místo předávání jeho parametrů jako možnosti příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-186">When you create Batch resources like pools and jobs, you can specify a JSON file containing hello new resource's configuration instead of passing its parameters as command-line options.</span></span> <span data-ttu-id="b3bb0-187">Například:</span><span class="sxs-lookup"><span data-stu-id="b3bb0-187">For example:</span></span>

```azurecli
az batch pool create my_batch_pool.json
```

<span data-ttu-id="b3bb0-188">Můžete si sice vytvořit většina prostředky Batch pomocí pouze možnosti příkazového řádku, některé funkce vyžadují, aby souboru formátu JSON, který obsahuje podrobnosti prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-188">While you can create most Batch resources using only command-line options, some features require that you specify a JSON-formatted file containing hello resource details.</span></span> <span data-ttu-id="b3bb0-189">Pokud chcete, aby toospecify zdrojových souborů pro spouštěcí úkol, například musíte použít soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-189">For example, you must use a JSON file if you want toospecify resource files for a start task.</span></span>

<span data-ttu-id="b3bb0-190">toosee hello syntaxe JSON požadované toocreate prostředku, získáte toohello [Batch REST API odkaz] [ rest_api] dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-190">toosee hello JSON syntax required toocreate a resource, refer toohello [Batch REST API reference][rest_api] documentation.</span></span> <span data-ttu-id="b3bb0-191">Každý "Přidat *typ prostředku*" v nápovědě k hello odkazu k REST API obsahuje ukázkové skripty JSON pro vytvoření prostředku.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-191">Each "Add *resource type*" topic in hello REST API reference contains sample JSON scripts for creating that resource.</span></span> <span data-ttu-id="b3bb0-192">Tyto ukázkové skripty JSON můžete použít jako šablona pro toouse soubory JSON s hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-192">You can use those sample JSON scripts as templates for JSON files toouse with hello Azure CLI.</span></span> <span data-ttu-id="b3bb0-193">Například toosee hello syntaxe JSON pro vytvoření fondu odkazovat příliš[přidat účet fondu tooan][rest_add_pool].</span><span class="sxs-lookup"><span data-stu-id="b3bb0-193">For example, toosee hello JSON syntax for pool creation, refer too[Add a pool tooan account][rest_add_pool].</span></span>

<span data-ttu-id="b3bb0-194">Ukázkový skript, který určuje soubor JSON, najdete v článku [Spuštění úlohy a úkolů pomocí služby Batch](./scripts/batch-cli-sample-run-job.md).</span><span class="sxs-lookup"><span data-stu-id="b3bb0-194">For a sample script that specifies a JSON file, see [Run a job and tasks with Batch](./scripts/batch-cli-sample-run-job.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b3bb0-195">Pokud zadáte soubor JSON, když vytvoříte prostředek, se ignorují další parametry, které jste zadali na hello příkazového řádku pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-195">If you specify a JSON file when you create a resource, any other parameters that you specify on hello command line for that resource are ignored.</span></span>
> 
> 

## <a name="efficient-queries-for-batch-resources"></a><span data-ttu-id="b3bb0-196">Efektivní dotazy na prostředky služby Batch</span><span class="sxs-lookup"><span data-stu-id="b3bb0-196">Efficient queries for Batch resources</span></span>

<span data-ttu-id="b3bb0-197">Každý typ prostředku Batch podporuje příkaz `list`, který zadá dotaz na účet Batch a vypíše seznam prostředků příslušného typu.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-197">Each Batch resource type supports a `list` command that queries your Batch account and lists resources of that type.</span></span> <span data-ttu-id="b3bb0-198">Například můžete vytvořit seznam hello fondy ve vašem účtu a hello úkoly v úloze:</span><span class="sxs-lookup"><span data-stu-id="b3bb0-198">For example, you can list hello pools in your account and hello tasks in a job:</span></span>

```azurecli
az batch pool list
az batch task list --job-id job001
```

<span data-ttu-id="b3bb0-199">Při dotazování služby Batch hello pomocí `list` operace, můžete zadat dobu hello toolimit klauzule OData vrácená data.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-199">When you query hello Batch service with a `list` operation, you can specify an OData clause toolimit hello amount of data returned.</span></span> <span data-ttu-id="b3bb0-200">Vzhledem k tomu, že všechny filtrování dojde na straně serveru, pouze hello data, která budete požadovat protne hello přenosu.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-200">Because all filtering occurs server-side, only hello data you request crosses hello wire.</span></span> <span data-ttu-id="b3bb0-201">Využití šířky pásma tyto klauzule toosave (a tedy času) při provádění operace výpisu.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-201">Use these clauses toosave bandwidth (and therefore time) when you perform list operations.</span></span>

<span data-ttu-id="b3bb0-202">Hello následující tabulka popisuje hello klauzule OData nepodporuje hello služby Batch:</span><span class="sxs-lookup"><span data-stu-id="b3bb0-202">hello following table describes hello OData clauses supported by hello Batch service:</span></span>

| <span data-ttu-id="b3bb0-203">Klauzule</span><span class="sxs-lookup"><span data-stu-id="b3bb0-203">Clause</span></span> | <span data-ttu-id="b3bb0-204">Popis</span><span class="sxs-lookup"><span data-stu-id="b3bb0-204">Description</span></span> |
|---|---|
| `--select-clause [select-clause]` | <span data-ttu-id="b3bb0-205">Vrátí podmnožinu vlastností pro každou entitu.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-205">Returns a subset of properties for each entity.</span></span> |
| `--filter-clause [filter-clause]` | <span data-ttu-id="b3bb0-206">Vrátí pouze entity, které odpovídají hello zadaný výraz OData.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-206">Returns only entities that match hello specified OData expression.</span></span> |
| `--expand-clause [expand-clause]` | <span data-ttu-id="b3bb0-207">Získá informace hello entity v jediném volání REST základní.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-207">Obtains hello entity information in a single underlying REST call.</span></span> <span data-ttu-id="b3bb0-208">Hello rozbalte klauzule aktuálně podporuje pouze hello `stats` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-208">hello expand clause currently supports only hello `stats` property.</span></span> |

<span data-ttu-id="b3bb0-209">Pro ukázku skriptu této ukazuje, jak zjistit, toouse klauzuli OData [spouštějí úlohy a úlohy pomocí služby Batch](./scripts/batch-cli-sample-run-job.md).</span><span class="sxs-lookup"><span data-stu-id="b3bb0-209">For a sample script that shows how toouse an OData clause, see [Run a job and tasks with Batch](./scripts/batch-cli-sample-run-job.md).</span></span>

<span data-ttu-id="b3bb0-210">Další informace o provádění dotazů efektivní seznamu s klauzulemi OData najdete v tématu [efektivní dotazování služby Azure Batch hello](batch-efficient-list-queries.md).</span><span class="sxs-lookup"><span data-stu-id="b3bb0-210">For more information on performing efficient list queries with OData clauses, see [Query hello Azure Batch service efficiently](batch-efficient-list-queries.md).</span></span>

## <a name="troubleshooting-tips"></a><span data-ttu-id="b3bb0-211">Rady pro řešení potíží</span><span class="sxs-lookup"><span data-stu-id="b3bb0-211">Troubleshooting tips</span></span>

<span data-ttu-id="b3bb0-212">Hello následující tipy mohou pomoci při odstraňování problémů, rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="b3bb0-212">hello following tips may help when you are troubleshooting Azure CLI issues:</span></span>

* <span data-ttu-id="b3bb0-213">Použití `-h` tooget **text nápovědy** k libovolnému příkazu rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="b3bb0-213">Use `-h` tooget **help text** for any CLI command</span></span>
* <span data-ttu-id="b3bb0-214">Použití `-v` a `-vv` toodisplay **podrobné** příkaz výstupu.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-214">Use `-v` and `-vv` toodisplay **verbose** command output.</span></span> <span data-ttu-id="b3bb0-215">Když hello `-vv` příznak je součástí, hello rozhraní příkazového řádku Azure zobrazí hello skutečné REST požadavky a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-215">When hello `-vv` flag is included, hello Azure CLI displays hello actual REST requests and responses.</span></span> <span data-ttu-id="b3bb0-216">Tyto přepínače jsou užitečné pro zobrazení úplného chybového výstupu.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-216">These switches are handy for displaying full error output.</span></span>
* <span data-ttu-id="b3bb0-217">Můžete zobrazit **příkaz výstup jako JSON** s hello `--json` možnost.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-217">You can view **command output as JSON** with hello `--json` option.</span></span> <span data-ttu-id="b3bb0-218">Příkaz `az batch pool show pool001 --json` například zobrazí vlastnosti fondu pool001 ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-218">For example, `az batch pool show pool001 --json` displays pool001's properties in JSON format.</span></span> <span data-ttu-id="b3bb0-219">Můžete zkopírovat a upravit tento výstup toouse v `--json-file` (viz [soubory JSON](#json-files) výše v tomto článku).</span><span class="sxs-lookup"><span data-stu-id="b3bb0-219">You can then copy and modify this output toouse in a `--json-file` (see [JSON files](#json-files) earlier in this article).</span></span>
<!---Loc Comment: Please, check link [JSON files] since it's not redirecting tooany location.--->
* <span data-ttu-id="b3bb0-220">Hello [fórum Batch] [ batch_forum] je monitorován pomocí členové týmu Batch.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-220">hello [Batch forum][batch_forum] is monitored by Batch team members.</span></span> <span data-ttu-id="b3bb0-221">Pokud narazíte na problémy nebo hledáte pomoc s konkrétní operací, můžete tu uveřejnit své otázky.</span><span class="sxs-lookup"><span data-stu-id="b3bb0-221">You can post your questions there if you run into issues or would like help with a specific operation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b3bb0-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b3bb0-222">Next steps</span></span>

* <span data-ttu-id="b3bb0-223">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu hello [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b3bb0-223">For more information about hello Azure CLI, see hello [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>
* <span data-ttu-id="b3bb0-224">Další informace o prostředcích služby Batch najdete v článku [Přehled služby Azure Batch pro vývojáře](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="b3bb0-224">For more information about Batch resources, see [Overview of Azure Batch for developers](batch-api-basics.md).</span></span>
* <span data-ttu-id="b3bb0-225">Další informace o použití šablony toocreate fondy, úlohy a úlohy Batch bez nutnosti psaní kódu najdete v tématu [použití Azure Batch rozhraní příkazového řádku šablony a přenos souborů (Preview)](batch-cli-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b3bb0-225">For more information about using Batch templates toocreate pools, jobs, and tasks without writing code, see [Use Azure Batch CLI Templates and File Transfer (Preview)](batch-cli-templates.md).</span></span>

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
