---
title: "aaaInstall a konfigurace virtuálních počítačů tooprovision Terraform a další infrastrukturou v Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall a nakonfigurujte Terraform toocreate Azure prostředky"
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
ms.openlocfilehash: b6706f53b21347442def05a592c30a2d22718984
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-terraform-tooprovision-vms-and-other-infrastructure-into-azure"></a><span data-ttu-id="72969-103">Instalace a konfigurace virtuálních počítačů tooprovision Terraform a další infrastrukturou do Azure</span><span class="sxs-lookup"><span data-stu-id="72969-103">Install and configure Terraform tooprovision VMs and other infrastructure into Azure</span></span> 
<span data-ttu-id="72969-104">Tento článek popisuje hello potřebné kroky tooinstall a nakonfigurovat Terraform tooprovision prostředky, jako jsou virtuální počítače do Azure.</span><span class="sxs-lookup"><span data-stu-id="72969-104">This article describes hello necessary steps tooinstall and configure Terraform tooprovision resources such as virtual machines into Azure.</span></span> <span data-ttu-id="72969-105">Se dozvíte, jak toocreate a používání Azure pověření tooenable Terraform tooprovision cloudovým prostředkům zabezpečené způsobem.</span><span class="sxs-lookup"><span data-stu-id="72969-105">You will learn how toocreate and use Azure credentials tooenable Terraform tooprovision cloud resources in a secure manner.</span></span>

<span data-ttu-id="72969-106">HashiCorp Terraform poskytuje snadný způsob toodefine a nasazení infrastruktury cloudu pomocí vlastní ukázka jazyk, kterému se říká HashiCorp konfigurace jazyk (kompatibilního hardwaru).</span><span class="sxs-lookup"><span data-stu-id="72969-106">HashiCorp Terraform provides an easy way toodefine and deploy cloud infrastructure by using a custom templating language called HashiCorp configuration language (HCL).</span></span> <span data-ttu-id="72969-107">Tato vlastní jazyk je [snadno toowrite a snadno toounderstand](terraform-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="72969-107">This custom language is [easy toowrite and easy toounderstand](terraform-create-complete-vm.md).</span></span> <span data-ttu-id="72969-108">Kromě toho pomocí hello `terraform plan` příkaz hello změny tooyour infrastruktury můžete vizualizovat, než je potvrzení.</span><span class="sxs-lookup"><span data-stu-id="72969-108">Additionally, by using hello `terraform plan` command, you can visualize hello changes tooyour infrastructure before you commit them.</span></span> <span data-ttu-id="72969-109">Postupujte podle těchto kroků toostart Terraform pomocí Azure.</span><span class="sxs-lookup"><span data-stu-id="72969-109">Follow these steps toostart using Terraform with Azure.</span></span>

## <a name="install-terraform"></a><span data-ttu-id="72969-110">Nainstalujte Terraform</span><span class="sxs-lookup"><span data-stu-id="72969-110">Install Terraform</span></span>
<span data-ttu-id="72969-111">tooinstall Terraform, [Stáhnout](https://www.terraform.io/downloads.html) hello balíčku pro váš operační systém do samostatné instalační adresář.</span><span class="sxs-lookup"><span data-stu-id="72969-111">tooinstall Terraform, [download](https://www.terraform.io/downloads.html) hello package appropriate for your operating system into a separate install directory.</span></span> <span data-ttu-id="72969-112">stažení Hello obsahuje jeden spustitelný soubor, pro které byste měli také definovat globální cestu.</span><span class="sxs-lookup"><span data-stu-id="72969-112">hello download contains a single executable file, for which you should also define a global path.</span></span> <span data-ttu-id="72969-113">Pokyny, jak tooset hello cestu na Linuxu a Macu, naleznete příliš[tato webová stránka](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux).</span><span class="sxs-lookup"><span data-stu-id="72969-113">For instructions on how tooset hello path on Linux and Mac, go too[this webpage](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux).</span></span> <span data-ttu-id="72969-114">Pokyny, jak tooset hello cesty v systému Windows, naleznete příliš[tato webová stránka](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).</span><span class="sxs-lookup"><span data-stu-id="72969-114">For instructions on how tooset hello path on Windows, go too[this webpage](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).</span></span> <span data-ttu-id="72969-115">tooverify instalace systému, spustit hello `terraform` příkaz.</span><span class="sxs-lookup"><span data-stu-id="72969-115">tooverify your installation, run hello `terraform` command.</span></span> <span data-ttu-id="72969-116">Zobrazí seznam dostupných možností Terraform jako výstup.</span><span class="sxs-lookup"><span data-stu-id="72969-116">You should see a list of available Terraform options as output.</span></span>

<span data-ttu-id="72969-117">Dále musíte tooallow Terraform přístup tooyour předplatného Azure tooperform infrastruktury zřizování.</span><span class="sxs-lookup"><span data-stu-id="72969-117">Next, you need tooallow Terraform access tooyour Azure subscription tooperform infrastructure provisioning.</span></span>

## <a name="set-up-terraform-access-tooazure"></a><span data-ttu-id="72969-118">Nastavení přístupu tooAzure Terraform</span><span class="sxs-lookup"><span data-stu-id="72969-118">Set up Terraform access tooAzure</span></span>
<span data-ttu-id="72969-119">tooenable Terraform tooprovision prostředky do Azure, je nutné toocreate dvě entity ve službě Azure Active Directory (Azure AD): aplikace Azure AD a instanční objekt služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="72969-119">tooenable Terraform tooprovision resources into Azure, you need toocreate two entities in Azure Active Directory (Azure AD): an Azure AD application and an Azure AD service principal.</span></span> <span data-ttu-id="72969-120">Poté použít tyto entity identifikátory v Terraform skripty.</span><span class="sxs-lookup"><span data-stu-id="72969-120">Then, you use these entities' identifiers in your Terraform scripts.</span></span> <span data-ttu-id="72969-121">Hlavní název služby je místní instanci globální Azure AD aplikace.</span><span class="sxs-lookup"><span data-stu-id="72969-121">A service principal is a local instance of a global Azure AD application.</span></span> <span data-ttu-id="72969-122">Objekt služby umožňuje granulární místní přístup k řízení tooglobal prostředkům.</span><span class="sxs-lookup"><span data-stu-id="72969-122">A service principal allows granular local access control tooglobal resources.</span></span>

<span data-ttu-id="72969-123">Existuje několik způsobů toocreate aplikaci Azure AD a instanční objekt služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="72969-123">There are several ways toocreate an Azure AD application and an Azure AD service principal.</span></span> <span data-ttu-id="72969-124">Hello nejjednodušší a nejrychlejší způsob v současné době jsou toouse Azure CLI 2.0, který [můžete stáhnout a nainstalovat na Windows, Linuxu nebo Macu](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="72969-124">hello easiest and fastest way today is toouse Azure CLI 2.0, which [you can download and install on Windows, Linux, or a Mac](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="72969-125">Můžete taky použít PowerShell nebo Azure CLI 1.0 toocreate hello potřebná zabezpečovací infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="72969-125">You also can use PowerShell or Azure CLI 1.0 toocreate hello necessary security infrastructure.</span></span> <span data-ttu-id="72969-126">pokyny Hello ukazují, jak tooconfigure Terraform pro Azure. všechny tyto přístupy.</span><span class="sxs-lookup"><span data-stu-id="72969-126">hello instructions that follow show you how tooconfigure Terraform for Azure by using all of these approaches.</span></span>

### <a name="use-azure-cli-20-for-windows-linux-or-mac-users"></a><span data-ttu-id="72969-127">Použití Azure CLI 2.0 (pro Windows, Linux nebo Mac. uživatelů)</span><span class="sxs-lookup"><span data-stu-id="72969-127">Use Azure CLI 2.0 (for Windows, Linux, or Mac users)</span></span> 
<span data-ttu-id="72969-128">Po stažení a instalaci hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), přihlaste se tooadminister vašeho předplatného Azure vydáním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="72969-128">After you download and install hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), sign in tooadminister your Azure subscription by issuing hello following command:</span></span>

```
az login
```

>[!NOTE]
><span data-ttu-id="72969-129">Pokud používáte hello Čína, Azure v Německu nebo cloudů Azure Government, musíte toofirst hello rozhraní příkazového řádku Azure toowork nakonfigurovat příslušný cloud.</span><span class="sxs-lookup"><span data-stu-id="72969-129">If you use hello China, Azure Germany, or Azure Government clouds, you need toofirst configure hello Azure CLI toowork with that cloud.</span></span> <span data-ttu-id="72969-130">Můžete provést spuštěním následující hello:</span><span class="sxs-lookup"><span data-stu-id="72969-130">You can do this by running hello following:</span></span>

```
az cloud set --name AzureChinaCloud|AzureGermanCloud|AzureUSGovernment
```

<span data-ttu-id="72969-131">Pokud máte víc předplatných Azure, jsou jejich podrobnosti vrácený hello `az login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="72969-131">If you have multiple Azure subscriptions, their details are returned by hello `az login` command.</span></span> <span data-ttu-id="72969-132">Sada hello `SUBSCRIPTION_ID` vrácena hodnota hello toohold proměnné prostředí hello `id` pole z předplatného hello chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="72969-132">Set hello `SUBSCRIPTION_ID` environment variable toohold hello value of hello returned `id` field from hello subscription you want toouse.</span></span> 

<span data-ttu-id="72969-133">Nastavte předplatné hello chcete toouse pro tuto relaci.</span><span class="sxs-lookup"><span data-stu-id="72969-133">Set hello subscription that you want toouse for this session.</span></span>

```
az account set --subscription="${SUBSCRIPTION_ID}"
```

<span data-ttu-id="72969-134">Dotaz na ID předplatného hello účtu tooget hello a hodnoty ID klienta.</span><span class="sxs-lookup"><span data-stu-id="72969-134">Query hello account tooget hello subscription ID and tenant ID values.</span></span>

```
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

<span data-ttu-id="72969-135">Dále vytvořte samostatné přihlašovací údaje pro Terraform.</span><span class="sxs-lookup"><span data-stu-id="72969-135">Next, create separate credentials for Terraform.</span></span>

```
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

<span data-ttu-id="72969-136">AppId, heslo, sp_name a klienta se vrátí.</span><span class="sxs-lookup"><span data-stu-id="72969-136">Your appId, password, sp_name, and tenant are returned.</span></span> <span data-ttu-id="72969-137">Poznamenejte si hello appId a heslo.</span><span class="sxs-lookup"><span data-stu-id="72969-137">Make a note of hello appId and password.</span></span>

<span data-ttu-id="72969-138">tooconfirm přihlašovacích údajů (instanční objekt), otevřete nové prostředí a spusťte následující příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="72969-138">tooconfirm your credentials (service principal), open a new shell and run hello following commands.</span></span> <span data-ttu-id="72969-139">SUBSTITUTE hello vrátil hodnoty sp_name, heslo a klienta:</span><span class="sxs-lookup"><span data-stu-id="72969-139">Substitute hello returned values for sp_name, password, and tenant:</span></span>

```
az login --service-principal -u SP_NAME -p PASSWORD --tenant TENANT
az vm list-sizes --location westus
```

### <a name="use-powershell-for-windows-users"></a><span data-ttu-id="72969-140">Pomocí prostředí PowerShell (pro uživatele systému Windows)</span><span class="sxs-lookup"><span data-stu-id="72969-140">Use PowerShell (for Windows users)</span></span> 
<span data-ttu-id="72969-141">toouse Windows počítač toowrite a spuštění vaší Terraform skripty a toouse prostředí PowerShell pro úlohy konfigurace, nakonfigurovat svůj počítač má prostředí PowerShell nástroje hello.</span><span class="sxs-lookup"><span data-stu-id="72969-141">toouse a Windows machine toowrite and execute your Terraform scripts and toouse PowerShell for configuration tasks, configure your machine with hello right PowerShell tools.</span></span> 

1. <span data-ttu-id="72969-142">Instalace nástrojů pro prostředí PowerShell pomocí následujících kroků hello v [nainstalovat a nakonfigurovat Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="72969-142">Install PowerShell tools by following hello steps in [Install and configure Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span></span> 

2. <span data-ttu-id="72969-143">Stažení a spuštění hello [skriptu azure setup.ps1](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) z konzoly Powershellu hello.</span><span class="sxs-lookup"><span data-stu-id="72969-143">Download and execute hello [azure-setup.ps1 script](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) from hello PowerShell console.</span></span>

3. <span data-ttu-id="72969-144">toorun hello azure setup.ps1 skriptu, jeho stažení a spuštění hello `./azure-setup.ps1 setup` v konzole PowerShell hello příkaz.</span><span class="sxs-lookup"><span data-stu-id="72969-144">toorun hello azure-setup.ps1 script, download it and execute hello `./azure-setup.ps1 setup` command from hello PowerShell console.</span></span> <span data-ttu-id="72969-145">Potom se přihlaste tooyour předplatné s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="72969-145">Then sign in tooyour Azure subscription with administrative privileges.</span></span>

4. <span data-ttu-id="72969-146">Zadejte název aplikace (libovolný řetězec, vyžaduje) po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="72969-146">Provide an application name (arbitrary string, required) when prompted.</span></span> <span data-ttu-id="72969-147">Volitelně můžete zadejte silné heslo po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="72969-147">Optionally, supply a strong password when prompted.</span></span> <span data-ttu-id="72969-148">Pokud nezadáte heslo, silné heslo se generuje pro můžete pomocí knihovny .NET zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="72969-148">If you don't provide a password, a strong password is generated for you by using .NET security libraries.</span></span>

### <a name="use-azure-cli-10-for-linux-or-mac-users"></a><span data-ttu-id="72969-149">Použití Azure CLI 1.0 (pro Linux nebo Mac. uživatelů)</span><span class="sxs-lookup"><span data-stu-id="72969-149">Use Azure CLI 1.0 (for Linux or Mac users)</span></span>
<span data-ttu-id="72969-150">tooget začít s Terraform na počítače se systémem Linux nebo Mac s Azure CLI verze 1.0, instalace hello správné knihovny v počítači.</span><span class="sxs-lookup"><span data-stu-id="72969-150">tooget started with Terraform on Linux machines or Macs with Azure CLI 1.0, install hello proper libraries on your machine.</span></span>  

1. <span data-ttu-id="72969-151">Instalace nástrojů příkazového řádku Azure xPlat podle následujících kroků hello v [nainstalovat Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="72969-151">Install Azure xPlat CLI tools by following hello steps in [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> 

2. <span data-ttu-id="72969-152">Stáhněte a nainstalujte podle pokynů hello v procesoru JSON [stáhnout jq](https://stedolan.github.io/jq/download/).</span><span class="sxs-lookup"><span data-stu-id="72969-152">Download and install a JSON processor by following hello instructions in [Download jq](https://stedolan.github.io/jq/download/).</span></span>

3. <span data-ttu-id="72969-153">Stažení a spuštění hello [skriptu azure setup.sh](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash skriptu z konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="72969-153">Download and execute hello [azure-setup.sh script](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash script from hello console.</span></span>

4. <span data-ttu-id="72969-154">toorun hello azure setup.sh skriptu, jeho stažení a spuštění hello `./azure-setup setup` příkazu z konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="72969-154">toorun hello azure-setup.sh script, download it and execute hello `./azure-setup setup` command from hello console.</span></span> <span data-ttu-id="72969-155">Potom se přihlaste tooyour předplatné s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="72969-155">Then sign in tooyour Azure subscription with administrative privileges.</span></span>
 
5. <span data-ttu-id="72969-156">Zadejte název aplikace (libovolný řetězec, vyžaduje) po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="72969-156">Provide an application name (arbitrary string, required) when prompted.</span></span> <span data-ttu-id="72969-157">Volitelně můžete zadejte silné heslo po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="72969-157">Optionally, supply a strong password when prompted.</span></span> <span data-ttu-id="72969-158">Pokud nezadáte heslo, silné heslo se generuje pro můžete pomocí knihovny .NET zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="72969-158">If you don't provide a password, a strong password is generated for you by using .NET security libraries.</span></span>

<span data-ttu-id="72969-159">Všechny předchozí skripty hello vytvoření aplikace Azure AD a služby hlavní.</span><span class="sxs-lookup"><span data-stu-id="72969-159">All hello previous scripts create an Azure AD application and service principal.</span></span> <span data-ttu-id="72969-160">Hello instanční objekt získá Přispěvatel nebo vlastníka úroveň přístupu na základě předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="72969-160">hello service principal gets a contributor or owner-level access on hello subscription.</span></span> <span data-ttu-id="72969-161">Z důvodu hello vysokou úroveň udělí přístup byste měli vždy chránit informace o zabezpečení hello generované tyto skripty.</span><span class="sxs-lookup"><span data-stu-id="72969-161">Because of hello high level of access granted, you should always protect hello security information generated by those scripts.</span></span> <span data-ttu-id="72969-162">Poznamenejte si všechny čtyři kusy informace o zabezpečení poskytované tyto skripty: appId, heslo, ID_ODBĚRU a tenant_id.</span><span class="sxs-lookup"><span data-stu-id="72969-162">Make a note of all four pieces of security information provided by those scripts: appId, password, subscription_id, and tenant_id.</span></span>

## <a name="set-environment-variables"></a><span data-ttu-id="72969-163">Proměnné prostředí sady</span><span class="sxs-lookup"><span data-stu-id="72969-163">Set environment variables</span></span>
<span data-ttu-id="72969-164">Po vytvoření a konfigurace objektu služby Azure AD, je nutné toolet Terraform vědět hello ID klienta, ID předplatného, ID klienta a tajný toouse klienta.</span><span class="sxs-lookup"><span data-stu-id="72969-164">After you create and configure an Azure AD service principal, you need toolet Terraform know hello tenant ID, subscription ID, client ID, and client secret toouse.</span></span> <span data-ttu-id="72969-165">Můžete to provést vložením tyto hodnoty v Terraform skripty, jak je popsáno v [vytvořit základní infrastruktura pomocí Terraform](terraform-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="72969-165">You can do it by embedding those values in your Terraform scripts, as described in [Create basic infrastructure by using Terraform](terraform-create-complete-vm.md).</span></span> <span data-ttu-id="72969-166">Alternativně můžete nastavit hello následující proměnné prostředí (a tedy vyhnout se omylem přihlašuje nebo sdílení přihlašovacích údajů):</span><span class="sxs-lookup"><span data-stu-id="72969-166">Alternately, you can set hello following environment variables (and thus avoid accidentally checking in or sharing your credentials):</span></span>

- <span data-ttu-id="72969-167">ARM_SUBSCRIPTION_ID</span><span class="sxs-lookup"><span data-stu-id="72969-167">ARM_SUBSCRIPTION_ID</span></span>
- <span data-ttu-id="72969-168">ARM_CLIENT_ID</span><span class="sxs-lookup"><span data-stu-id="72969-168">ARM_CLIENT_ID</span></span>
- <span data-ttu-id="72969-169">ARM_CLIENT_SECRET</span><span class="sxs-lookup"><span data-stu-id="72969-169">ARM_CLIENT_SECRET</span></span>
- <span data-ttu-id="72969-170">ARM_TENANT_ID</span><span class="sxs-lookup"><span data-stu-id="72969-170">ARM_TENANT_ID</span></span>

<span data-ttu-id="72969-171">Tato ukázka prostředí skriptu tooset můžete použít tyto proměnné:</span><span class="sxs-lookup"><span data-stu-id="72969-171">You can use this sample shell script tooset those variables:</span></span>

```
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id
```

<span data-ttu-id="72969-172">Kromě toho pokud použijete Terraform s Azure v Číně nebo buď Azure Government nebo Azure v Německu, je třeba proměnnou prostředí hello tooset správně.</span><span class="sxs-lookup"><span data-stu-id="72969-172">Additionally, if you use Terraform with Azure in China or either Azure Government or Azure Germany, you need tooset hello ENVIRONMENT variable appropriately.</span></span>

## <a name="next-steps"></a><span data-ttu-id="72969-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="72969-173">Next steps</span></span>
<span data-ttu-id="72969-174">Teď už máte nainstalovaný Terraform a nakonfigurovat přihlašovací údaje Azure, takže můžete začít nasazovat infrastruktury do vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="72969-174">You have now installed Terraform and configured Azure credentials so that you can start deploying infrastructure into your Azure subscription.</span></span> <span data-ttu-id="72969-175">Dále se naučíte, jak příliš[vytvoření infrastruktury s Terraform](terraform-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="72969-175">Next, learn how too[create infrastructure with Terraform](terraform-create-complete-vm.md).</span></span>
