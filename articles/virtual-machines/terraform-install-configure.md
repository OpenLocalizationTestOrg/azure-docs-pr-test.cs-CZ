---
title: "Instalace a konfigurace Terraform ke zřízení virtuálních počítačů a další infrastrukturou v Azure | Microsoft Docs"
description: "Zjistěte, jak nainstalovat a nakonfigurovat Terraform vytváření prostředků Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: echuvyrov
ms.openlocfilehash: 1f26bccf279ebb61fbf77767186d0435e4f4ba40
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="install-and-configure-terraform-to-provision-vms-and-other-infrastructure-into-azure"></a><span data-ttu-id="aa0bc-103">Instalace a konfigurace Terraform ke zřízení virtuálních počítačů a další infrastrukturou do Azure</span><span class="sxs-lookup"><span data-stu-id="aa0bc-103">Install and configure Terraform to provision VMs and other infrastructure into Azure</span></span> 
<span data-ttu-id="aa0bc-104">Tento článek popisuje kroky potřebné k instalaci a konfiguraci Terraform k poskytování prostředkům, například virtuálních počítačů do Azure.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-104">This article describes the necessary steps to install and configure Terraform to provision resources such as virtual machines into Azure.</span></span> <span data-ttu-id="aa0bc-105">Se dozvíte, jak vytvořit a použít přihlašovací údaje Azure umožňující Terraform ke zřízení cloudovým prostředkům zabezpečené způsobem.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-105">You will learn how to create and use Azure credentials to enable Terraform to provision cloud resources in a secure manner.</span></span>

<span data-ttu-id="aa0bc-106">HashiCorp Terraform poskytuje snadný způsob, jak definovat a nasazovat infrastruktury cloudu s využitím vlastní ukázka jazyk, kterému se říká HashiCorp konfigurace jazyk (kompatibilního hardwaru).</span><span class="sxs-lookup"><span data-stu-id="aa0bc-106">HashiCorp Terraform provides an easy way to define and deploy cloud infrastructure by using a custom templating language called HashiCorp configuration language (HCL).</span></span> <span data-ttu-id="aa0bc-107">Tato vlastní jazyk je [snadno k zápisu a snadno pochopitelné](terraform-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="aa0bc-107">This custom language is [easy to write and easy to understand](terraform-create-complete-vm.md).</span></span> <span data-ttu-id="aa0bc-108">Kromě toho pomocí `terraform plan` příkazů, můžete vizualizovat změny k infrastruktuře předtím, než je potvrzení.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-108">Additionally, by using the `terraform plan` command, you can visualize the changes to your infrastructure before you commit them.</span></span> <span data-ttu-id="aa0bc-109">Postupujte podle těchto kroků k použití Terraform s Azure.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-109">Follow these steps to start using Terraform with Azure.</span></span>

## <a name="install-terraform"></a><span data-ttu-id="aa0bc-110">Nainstalujte Terraform</span><span class="sxs-lookup"><span data-stu-id="aa0bc-110">Install Terraform</span></span>
<span data-ttu-id="aa0bc-111">Chcete-li nainstalovat Terraform, [Stáhnout](https://www.terraform.io/downloads.html) balíčku pro váš operační systém do samostatné instalační adresář.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-111">To install Terraform, [download](https://www.terraform.io/downloads.html) the package appropriate for your operating system into a separate install directory.</span></span> <span data-ttu-id="aa0bc-112">Stahování obsahuje jeden spustitelný soubor, pro které byste měli také definovat globální cestu.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-112">The download contains a single executable file, for which you should also define a global path.</span></span> <span data-ttu-id="aa0bc-113">Pokyny o tom, jak nastavit cestu, na Linuxu a Macu, přejděte na [tato webová stránka](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux).</span><span class="sxs-lookup"><span data-stu-id="aa0bc-113">For instructions on how to set the path on Linux and Mac, go to [this webpage](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux).</span></span> <span data-ttu-id="aa0bc-114">Pokyny o tom, jak nastavit cestu, v systému Windows, přejděte na [tato webová stránka](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).</span><span class="sxs-lookup"><span data-stu-id="aa0bc-114">For instructions on how to set the path on Windows, go to [this webpage](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).</span></span> <span data-ttu-id="aa0bc-115">Pokud chcete ověřit instalaci, spusťte `terraform` příkaz.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-115">To verify your installation, run the `terraform` command.</span></span> <span data-ttu-id="aa0bc-116">Zobrazí seznam dostupných možností Terraform jako výstup.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-116">You should see a list of available Terraform options as output.</span></span>

<span data-ttu-id="aa0bc-117">Dále musíte povolit Terraform přístup k vašemu předplatnému Azure, chcete-li provést zřízení infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-117">Next, you need to allow Terraform access to your Azure subscription to perform infrastructure provisioning.</span></span>

## <a name="set-up-terraform-access-to-azure"></a><span data-ttu-id="aa0bc-118">Nastavení přístupu k Terraform do Azure</span><span class="sxs-lookup"><span data-stu-id="aa0bc-118">Set up Terraform access to Azure</span></span>
<span data-ttu-id="aa0bc-119">Pokud chcete povolit Terraform k prostředkům zřídit do Azure, musíte vytvořit dvě entity ve službě Azure Active Directory (Azure AD): aplikace Azure AD a instanční objekt služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-119">To enable Terraform to provision resources into Azure, you need to create two entities in Azure Active Directory (Azure AD): an Azure AD application and an Azure AD service principal.</span></span> <span data-ttu-id="aa0bc-120">Poté použít tyto entity identifikátory v Terraform skripty.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-120">Then, you use these entities' identifiers in your Terraform scripts.</span></span> <span data-ttu-id="aa0bc-121">Hlavní název služby je místní instanci globální Azure AD aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-121">A service principal is a local instance of a global Azure AD application.</span></span> <span data-ttu-id="aa0bc-122">Objekt služby umožňuje řídit granulární místní přístup k globální prostředky.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-122">A service principal allows granular local access control to global resources.</span></span>

<span data-ttu-id="aa0bc-123">Existuje několik způsobů, jak vytvořit objekt aplikaci Azure AD a službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-123">There are several ways to create an Azure AD application and an Azure AD service principal.</span></span> <span data-ttu-id="aa0bc-124">Nejjednodušší a nejrychlejší způsob dnes je použití Azure CLI 2.0, který [můžete stáhnout a nainstalovat na Windows, Linuxu nebo Macu](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="aa0bc-124">The easiest and fastest way today is to use Azure CLI 2.0, which [you can download and install on Windows, Linux, or a Mac](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="aa0bc-125">K vytvoření infrastruktury potřebné zabezpečení můžete použít také prostředí PowerShell nebo Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-125">You also can use PowerShell or Azure CLI 1.0 to create the necessary security infrastructure.</span></span> <span data-ttu-id="aa0bc-126">Následující pokyny popisují, jak konfigurace Terraform pro Azure pomocí všech těchto přístupů.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-126">The instructions that follow show you how to configure Terraform for Azure by using all of these approaches.</span></span>

### <a name="use-azure-cli-20-for-windows-linux-or-mac-users"></a><span data-ttu-id="aa0bc-127">Použití Azure CLI 2.0 (pro Windows, Linux nebo Mac. uživatelů)</span><span class="sxs-lookup"><span data-stu-id="aa0bc-127">Use Azure CLI 2.0 (for Windows, Linux, or Mac users)</span></span> 
<span data-ttu-id="aa0bc-128">Po stažení a instalaci [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), přihlaste se ke správě vašeho předplatného Azure po vydání následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="aa0bc-128">After you download and install the [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), sign in to administer your Azure subscription by issuing the following command:</span></span>

```
az login
```

>[!NOTE]
><span data-ttu-id="aa0bc-129">Pokud používáte Čína, Azure v Německu a cloudy Azure Government, musíte nejdřív nakonfigurovat rozhraní příkazového řádku Azure pro práci s tímto cloudem.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-129">If you use the China, Azure Germany, or Azure Government clouds, you need to first configure the Azure CLI to work with that cloud.</span></span> <span data-ttu-id="aa0bc-130">Můžete provést spuštěním následující:</span><span class="sxs-lookup"><span data-stu-id="aa0bc-130">You can do this by running the following:</span></span>

```
az cloud set --name AzureChinaCloud|AzureGermanCloud|AzureUSGovernment
```

<span data-ttu-id="aa0bc-131">Pokud máte víc předplatných Azure, jsou jejich podrobnosti vrácený `az login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-131">If you have multiple Azure subscriptions, their details are returned by the `az login` command.</span></span> <span data-ttu-id="aa0bc-132">Nastavte `SUBSCRIPTION_ID` proměnnou prostředí pro uložení hodnota vrácený `id` pole z předplatného, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-132">Set the `SUBSCRIPTION_ID` environment variable to hold the value of the returned `id` field from the subscription you want to use.</span></span> 

<span data-ttu-id="aa0bc-133">Nastavte předplatné, které chcete použít pro tuto relaci.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-133">Set the subscription that you want to use for this session.</span></span>

```
az account set --subscription="${SUBSCRIPTION_ID}"
```

<span data-ttu-id="aa0bc-134">Dotaz na účet, který chcete získat předplatné hodnoty ID ID a klienta.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-134">Query the account to get the subscription ID and tenant ID values.</span></span>

```
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

<span data-ttu-id="aa0bc-135">Dále vytvořte samostatné přihlašovací údaje pro Terraform.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-135">Next, create separate credentials for Terraform.</span></span>

```
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

<span data-ttu-id="aa0bc-136">AppId, heslo, sp_name a klienta se vrátí.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-136">Your appId, password, sp_name, and tenant are returned.</span></span> <span data-ttu-id="aa0bc-137">Poznamenejte si ID aplikace a heslo.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-137">Make a note of the appId and password.</span></span>

<span data-ttu-id="aa0bc-138">Pokud chcete ověřit vaše přihlašovací údaje (instanční objekt), otevřete nové prostředí a spusťte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-138">To confirm your credentials (service principal), open a new shell and run the following commands.</span></span> <span data-ttu-id="aa0bc-139">Nahraďte vrácené hodnoty sp_name, heslo a klienta:</span><span class="sxs-lookup"><span data-stu-id="aa0bc-139">Substitute the returned values for sp_name, password, and tenant:</span></span>

```
az login --service-principal -u SP_NAME -p PASSWORD --tenant TENANT
az vm list-sizes --location westus
```

### <a name="use-powershell-for-windows-users"></a><span data-ttu-id="aa0bc-140">Pomocí prostředí PowerShell (pro uživatele systému Windows)</span><span class="sxs-lookup"><span data-stu-id="aa0bc-140">Use PowerShell (for Windows users)</span></span> 
<span data-ttu-id="aa0bc-141">Počítače s Windows k zápisu a spustit skripty Terraform a jak pomocí prostředí PowerShell pro úlohy konfigurace, nakonfigurujte svůj počítač správné nástroje prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-141">To use a Windows machine to write and execute your Terraform scripts and to use PowerShell for configuration tasks, configure your machine with the right PowerShell tools.</span></span> 

1. <span data-ttu-id="aa0bc-142">Instalace nástrojů pro prostředí PowerShell pomocí následujících kroků v [nainstalovat a nakonfigurovat Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="aa0bc-142">Install PowerShell tools by following the steps in [Install and configure Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span></span> 

2. <span data-ttu-id="aa0bc-143">Stažení a spuštění [skriptu azure setup.ps1](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) z konzoly Powershellu.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-143">Download and execute the [azure-setup.ps1 script](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) from the PowerShell console.</span></span>

3. <span data-ttu-id="aa0bc-144">Pro spuštění skriptu azure setup.ps1, stažení a spuštění `./azure-setup.ps1 setup` v konzole PowerShell příkaz.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-144">To run the azure-setup.ps1 script, download it and execute the `./azure-setup.ps1 setup` command from the PowerShell console.</span></span> <span data-ttu-id="aa0bc-145">Přihlaste se k předplatnému Azure s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-145">Then sign in to your Azure subscription with administrative privileges.</span></span>

4. <span data-ttu-id="aa0bc-146">Zadejte název aplikace (libovolný řetězec, vyžaduje) po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-146">Provide an application name (arbitrary string, required) when prompted.</span></span> <span data-ttu-id="aa0bc-147">Volitelně můžete zadejte silné heslo po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-147">Optionally, supply a strong password when prompted.</span></span> <span data-ttu-id="aa0bc-148">Pokud nezadáte heslo, silné heslo se generuje pro můžete pomocí knihovny .NET zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-148">If you don't provide a password, a strong password is generated for you by using .NET security libraries.</span></span>

### <a name="use-azure-cli-10-for-linux-or-mac-users"></a><span data-ttu-id="aa0bc-149">Použití Azure CLI 1.0 (pro Linux nebo Mac. uživatelů)</span><span class="sxs-lookup"><span data-stu-id="aa0bc-149">Use Azure CLI 1.0 (for Linux or Mac users)</span></span>
<span data-ttu-id="aa0bc-150">Začít pracovat s Terraform na počítače se systémem Linux nebo počítače Mac pomocí Azure CLI 1.0, nainstalujte správné knihovny na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-150">To get started with Terraform on Linux machines or Macs with Azure CLI 1.0, install the proper libraries on your machine.</span></span>  

1. <span data-ttu-id="aa0bc-151">Instalace nástrojů příkazového řádku Azure xPlat podle kroků v [nainstalovat Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="aa0bc-151">Install Azure xPlat CLI tools by following the steps in [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> 

2. <span data-ttu-id="aa0bc-152">Stáhněte a nainstalujte podle pokynů v mapě JSON [stáhnout jq](https://stedolan.github.io/jq/download/).</span><span class="sxs-lookup"><span data-stu-id="aa0bc-152">Download and install a JSON processor by following the instructions in [Download jq](https://stedolan.github.io/jq/download/).</span></span>

3. <span data-ttu-id="aa0bc-153">Stažení a spuštění [skriptu azure setup.sh](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash skriptu z konzoly.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-153">Download and execute the [azure-setup.sh script](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash script from the console.</span></span>

4. <span data-ttu-id="aa0bc-154">Pro spuštění skriptu azure setup.sh, stažení a spuštění `./azure-setup setup` příkazu z konzoly.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-154">To run the azure-setup.sh script, download it and execute the `./azure-setup setup` command from the console.</span></span> <span data-ttu-id="aa0bc-155">Přihlaste se k předplatnému Azure s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-155">Then sign in to your Azure subscription with administrative privileges.</span></span>
 
5. <span data-ttu-id="aa0bc-156">Zadejte název aplikace (libovolný řetězec, vyžaduje) po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-156">Provide an application name (arbitrary string, required) when prompted.</span></span> <span data-ttu-id="aa0bc-157">Volitelně můžete zadejte silné heslo po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-157">Optionally, supply a strong password when prompted.</span></span> <span data-ttu-id="aa0bc-158">Pokud nezadáte heslo, silné heslo se generuje pro můžete pomocí knihovny .NET zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-158">If you don't provide a password, a strong password is generated for you by using .NET security libraries.</span></span>

<span data-ttu-id="aa0bc-159">Všechny předchozí skripty vytvoření aplikace Azure AD a služby hlavní.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-159">All the previous scripts create an Azure AD application and service principal.</span></span> <span data-ttu-id="aa0bc-160">Objekt služby získá Přispěvatel nebo vlastníka úroveň přístupu u předplatného.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-160">The service principal gets a contributor or owner-level access on the subscription.</span></span> <span data-ttu-id="aa0bc-161">Z důvodu vysokou úroveň udělení přístupu byste měli vždy chránit informace o zabezpečení generované tyto skripty.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-161">Because of the high level of access granted, you should always protect the security information generated by those scripts.</span></span> <span data-ttu-id="aa0bc-162">Poznamenejte si všechny čtyři kusy informace o zabezpečení poskytované tyto skripty: appId, heslo, ID_ODBĚRU a tenant_id.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-162">Make a note of all four pieces of security information provided by those scripts: appId, password, subscription_id, and tenant_id.</span></span>

## <a name="set-environment-variables"></a><span data-ttu-id="aa0bc-163">Proměnné prostředí sady</span><span class="sxs-lookup"><span data-stu-id="aa0bc-163">Set environment variables</span></span>
<span data-ttu-id="aa0bc-164">Po vytvoření a konfigurace objektu služby Azure AD, budete muset umožní Terraform vědět klienta ID, ID předplatného, ID klienta a tajný klíč klienta použít.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-164">After you create and configure an Azure AD service principal, you need to let Terraform know the tenant ID, subscription ID, client ID, and client secret to use.</span></span> <span data-ttu-id="aa0bc-165">Můžete to provést vložením tyto hodnoty v Terraform skripty, jak je popsáno v [vytvořit základní infrastruktura pomocí Terraform](terraform-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="aa0bc-165">You can do it by embedding those values in your Terraform scripts, as described in [Create basic infrastructure by using Terraform](terraform-create-complete-vm.md).</span></span> <span data-ttu-id="aa0bc-166">Alternativně můžete nastavit následující proměnné prostředí (a tedy vyhnout se omylem přihlašuje nebo sdílení přihlašovacích údajů):</span><span class="sxs-lookup"><span data-stu-id="aa0bc-166">Alternately, you can set the following environment variables (and thus avoid accidentally checking in or sharing your credentials):</span></span>

- <span data-ttu-id="aa0bc-167">ARM_SUBSCRIPTION_ID</span><span class="sxs-lookup"><span data-stu-id="aa0bc-167">ARM_SUBSCRIPTION_ID</span></span>
- <span data-ttu-id="aa0bc-168">ARM_CLIENT_ID</span><span class="sxs-lookup"><span data-stu-id="aa0bc-168">ARM_CLIENT_ID</span></span>
- <span data-ttu-id="aa0bc-169">ARM_CLIENT_SECRET</span><span class="sxs-lookup"><span data-stu-id="aa0bc-169">ARM_CLIENT_SECRET</span></span>
- <span data-ttu-id="aa0bc-170">ARM_TENANT_ID</span><span class="sxs-lookup"><span data-stu-id="aa0bc-170">ARM_TENANT_ID</span></span>

<span data-ttu-id="aa0bc-171">Chcete-li nastavit tyto proměnné můžete použít tento ukázkový skript prostředí:</span><span class="sxs-lookup"><span data-stu-id="aa0bc-171">You can use this sample shell script to set those variables:</span></span>

```
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id
```

<span data-ttu-id="aa0bc-172">Kromě toho pokud použijete Terraform s Azure v Číně nebo buď Azure Government nebo Azure v Německu, budete muset nastavit proměnnou prostředí odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-172">Additionally, if you use Terraform with Azure in China or either Azure Government or Azure Germany, you need to set the environment variable appropriately.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa0bc-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aa0bc-173">Next steps</span></span>
<span data-ttu-id="aa0bc-174">Teď už máte nainstalovaný Terraform a nakonfigurovat přihlašovací údaje Azure, takže můžete začít nasazovat infrastruktury do vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="aa0bc-174">You have now installed Terraform and configured Azure credentials so that you can start deploying infrastructure into your Azure subscription.</span></span> <span data-ttu-id="aa0bc-175">Dále se naučíte, jak [vytvoření infrastruktury s Terraform](terraform-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="aa0bc-175">Next, learn how to [create infrastructure with Terraform](terraform-create-complete-vm.md).</span></span>
