---
title: "Výpočetní - rozšíření diagnostiky Linux | Microsoft Docs"
description: "Postup konfigurace Azure Linux diagnostiky rozšíření (LAD) pro shromažďování metrik a protokolování událostí z virtuálních počítačů Linux běží v Azure."
services: virtual-machines-linux
author: jasonzio
manager: anandram
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/09/2017
ms.author: jasonzio
ms.openlocfilehash: 525d706bd709ae72f2dca1c21e06db533ccf32b4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-linux-diagnostic-extension-to-monitor-metrics-and-logs"></a><span data-ttu-id="bf3b1-103">Použití rozšíření diagnostiky Linux ke sledování metrik a protokoly</span><span class="sxs-lookup"><span data-stu-id="bf3b1-103">Use Linux Diagnostic Extension to monitor metrics and logs</span></span>

<span data-ttu-id="bf3b1-104">Tento dokument popisuje verze 3.0 a novějších rozšíření diagnostiky Linux.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-104">This document describes version 3.0 and newer of the Linux Diagnostic Extension.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bf3b1-105">Informace o 2.3 a starší verze najdete v tématu [tento dokument](./classic/diagnostic-extension-v2.md).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-105">For information about version 2.3 and older, see [this document](./classic/diagnostic-extension-v2.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="bf3b1-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="bf3b1-106">Introduction</span></span>

<span data-ttu-id="bf3b1-107">Rozšíření diagnostiky Linux pomáhá uživatele monitorování stavu virtuálního počítače s Linuxem systémem Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-107">The Linux Diagnostic Extension helps a user monitor the health of a Linux VM running on Microsoft Azure.</span></span> <span data-ttu-id="bf3b1-108">Má následující funkce:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-108">It has the following capabilities:</span></span>

* <span data-ttu-id="bf3b1-109">Shromažďuje metriky výkonu systému z virtuálního počítače a ukládá je do určité tabulky v účtu úložiště určený.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-109">Collects system performance metrics from the VM and stores them in a specific table in a designated storage account.</span></span>
* <span data-ttu-id="bf3b1-110">Načte události protokolu z syslog a ukládá je do určité tabulky v účtu úložiště určený.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-110">Retrieves log events from syslog and stores them in a specific table in the designated storage account.</span></span>
* <span data-ttu-id="bf3b1-111">Umožňuje uživatelům umožnit přizpůsobení metriky dat, které jsou shromažďovány a nahrát.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-111">Enables users to customize the data metrics that are collected and uploaded.</span></span>
* <span data-ttu-id="bf3b1-112">Umožňuje uživatelům umožnit přizpůsobení syslog zařízení a úrovně závažnosti události, které jsou shromažďovány a nahrát.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-112">Enables users to customize the syslog facilities and severity levels of events that are collected and uploaded.</span></span>
* <span data-ttu-id="bf3b1-113">Umožňuje uživatelům odesílat zadané soubory protokolu do tabulky určené úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-113">Enables users to upload specified log files to a designated storage table.</span></span>
* <span data-ttu-id="bf3b1-114">Podporuje odesílání metriky a protokolu událostí na libovolný EventHub koncových bodů a formátu JSON BLOB v účtu úložiště určený.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-114">Supports sending metrics and log events to arbitrary EventHub endpoints and JSON-formatted blobs in the designated storage account.</span></span>

<span data-ttu-id="bf3b1-115">Toto rozšíření pracuje s obou modelech nasazení Azure.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-115">This extension works with both Azure deployment models.</span></span>

## <a name="installing-the-extension-in-your-vm"></a><span data-ttu-id="bf3b1-116">Instalaci rozšíření ve vašem virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="bf3b1-116">Installing the extension in your VM</span></span>

<span data-ttu-id="bf3b1-117">Toto rozšíření můžete povolit pomocí rutin prostředí Azure PowerShell, rozhraní příkazového řádku Azure skripty nebo šablony nasazení Azure.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-117">You can enable this extension by using the Azure PowerShell cmdlets, Azure CLI scripts, or Azure deployment templates.</span></span> <span data-ttu-id="bf3b1-118">Další informace najdete v tématu [rozšíření funkce](./extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-118">For more information, see [Extensions Features](./extensions-features.md).</span></span>

<span data-ttu-id="bf3b1-119">Povolit nebo konfigurovat LAD 3.0 nelze použít na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-119">The Azure portal cannot be used to enable or configure LAD 3.0.</span></span> <span data-ttu-id="bf3b1-120">Místo toho nainstaluje a nakonfiguruje verze 2.3.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-120">Instead, it installs and configures version 2.3.</span></span> <span data-ttu-id="bf3b1-121">Výstrahy a portálu Azure grafy pracovat s daty z obě verze rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-121">Azure portal graphs and alerts work with data from both versions of the extension.</span></span>

<span data-ttu-id="bf3b1-122">Tyto pokyny k instalaci a [ke stažení ukázkové konfiguraci](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) konfigurace LAD 3.0 na:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-122">These installation instructions and a [downloadable sample configuration](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) configure LAD 3.0 to:</span></span>

* <span data-ttu-id="bf3b1-123">zaznamenání a uložení metriku stejné jako byly poskytované LAD 2.3;</span><span class="sxs-lookup"><span data-stu-id="bf3b1-123">capture and store the same metrics as were provided by LAD 2.3;</span></span>
* <span data-ttu-id="bf3b1-124">zaznamenat užitečné sadu metriky systému souborů, nový LAD 3.0;</span><span class="sxs-lookup"><span data-stu-id="bf3b1-124">capture a useful set of file system metrics, new to LAD 3.0;</span></span>
* <span data-ttu-id="bf3b1-125">zachycení výchozí kolekci syslog ve LAD 2.3; povolené</span><span class="sxs-lookup"><span data-stu-id="bf3b1-125">capture the default syslog collection enabled by LAD 2.3;</span></span>
* <span data-ttu-id="bf3b1-126">Povolte Azure portálu prostředí pro vytváření grafů a výstrahy na metriky virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-126">enable the Azure portal experience for charting and alerting on VM metrics.</span></span>

<span data-ttu-id="bf3b1-127">Zaváděná konfigurace je jenom jako příklad; upravte tak, aby vyhovovaly potřebám vaší vlastní.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-127">The downloadable configuration is just an example; modify it to suit your own needs.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="bf3b1-128">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bf3b1-128">Prerequisites</span></span>

* <span data-ttu-id="bf3b1-129">**Azure Linux Agent verze 2.2.0 nebo novější**.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-129">**Azure Linux Agent version 2.2.0 or later**.</span></span> <span data-ttu-id="bf3b1-130">Většina virtuálních počítačů Linux Azure Galerie Image zahrnují verze 2.2.7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-130">Most Azure VM Linux gallery images include version 2.2.7 or later.</span></span> <span data-ttu-id="bf3b1-131">Spustit `/usr/sbin/waagent -version` chcete ověřit verzi nainstalovaný na Virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-131">Run `/usr/sbin/waagent -version` to confirm the version installed on the VM.</span></span> <span data-ttu-id="bf3b1-132">Pokud virtuální počítač běží starší verze agenta hosta, postupujte podle [tyto pokyny](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) jej aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-132">If the VM is running an older version of the guest agent, follow [these instructions](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) to update it.</span></span>
* <span data-ttu-id="bf3b1-133">**Rozhraní příkazového řádku Azure**.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-133">**Azure CLI**.</span></span> <span data-ttu-id="bf3b1-134">[Nastavení Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) prostředí na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-134">[Set up the Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) environment on your machine.</span></span>
* <span data-ttu-id="bf3b1-135">Příkaz wget, pokud ji nemáte: Spusťte `sudo apt-get install wget`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-135">The wget command, if you don't already have it: Run `sudo apt-get install wget`.</span></span>
* <span data-ttu-id="bf3b1-136">Stávající předplatné Azure a v něm k uložení dat stávající účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-136">An existing Azure subscription and an existing storage account within it to store the data.</span></span>

### <a name="sample-installation"></a><span data-ttu-id="bf3b1-137">Ukázka instalace</span><span class="sxs-lookup"><span data-stu-id="bf3b1-137">Sample installation</span></span>

<span data-ttu-id="bf3b1-138">Zadejte správné parametry na první tři řádky, a poté spusťte tento skript jako kořenového adresáře:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-138">Fill in the correct parameters on the first three lines, then execute this script as root:</span></span>

```bash
# Set your Azure VM diagnostic parameters correctly below
my_resource_group=<your_azure_resource_group_name_containing_your_azure_linux_vm>
my_linux_vm=<your_azure_linux_vm_name>
my_diagnostic_storage_account=<your_azure_storage_account_for_storing_vm_diagnostic_data>

# Should login to Azure first before anything else
az login

# Select the subscription containing the storage account
az account set --subscription <your_azure_subscription_id>

# Download the sample Public settings. (You could also use curl or any web browser)
wget https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json -O portal_public_settings.json

# Build the VM resource ID. Replace storage account name and resource ID in the public settings.
my_vm_resource_id=$(az vm show -g $my_resource_group -n $my_linux_vm --query "id" -o tsv)
sed -i "s#__DIAGNOSTIC_STORAGE_ACCOUNT__#$my_diagnostic_storage_account#g" portal_public_settings.json
sed -i "s#__VM_RESOURCE_ID__#$my_vm_resource_id#g" portal_public_settings.json

# Build the protected settings (storage account SAS token)
my_diagnostic_storage_account_sastoken=$(az storage account generate-sas --account-name $my_diagnostic_storage_account --expiry 9999-12-31T23:59Z --permissions wlacu --resource-types co --services bt -o tsv)
my_lad_protected_settings="{'storageAccountName': '$my_diagnostic_storage_account', 'storageAccountSasToken': '$my_diagnostic_storage_account_sastoken'}"

# Finallly tell Azure to install and enable the extension
az vm extension set --publisher Microsoft.Azure.Diagnostics --name LinuxDiagnostic --version 3.0 --resource-group $my_resource_group --vm-name $my_linux_vm --protected-settings "${my_lad_protected_settings}" --settings portal_public_settings.json
```

<span data-ttu-id="bf3b1-139">Adresa URL pro ukázkové konfiguraci a její obsah se mohou změnit.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-139">The URL for the sample configuration, and its contents, are subject to change.</span></span> <span data-ttu-id="bf3b1-140">Stáhněte si kopii souboru JSON nastavení portálu a přizpůsobit pro své potřeby.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-140">Download a copy of the portal settings JSON file and customize it for your needs.</span></span> <span data-ttu-id="bf3b1-141">Všechny šablony nebo automatizace, které vytvoříte měli používat vlastní kopie místo stahování pokaždé, když tuto adresu URL.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-141">Any templates or automation you construct should use your own copy, rather than downloading that URL each time.</span></span>

### <a name="updating-the-extension-settings"></a><span data-ttu-id="bf3b1-142">Aktualizuje se nastavení rozšíření</span><span class="sxs-lookup"><span data-stu-id="bf3b1-142">Updating the extension settings</span></span>

<span data-ttu-id="bf3b1-143">Po změně chráněné nebo nastavení veřejných, nasaďte do virtuálního počítače tak, že spuštění stejného příkazu.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-143">After you've changed your Protected or Public settings, deploy them to the VM by running the same command.</span></span> <span data-ttu-id="bf3b1-144">Pokud nic změnit v nastavení, rozšíření jsou odesílány aktualizovaným nastavením.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-144">If anything changed in the settings, the updated settings are sent to the extension.</span></span> <span data-ttu-id="bf3b1-145">LAD znovu načte konfiguraci a restartuje sám sebe.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-145">LAD reloads the configuration and restarts itself.</span></span>

### <a name="migration-from-previous-versions-of-the-extension"></a><span data-ttu-id="bf3b1-146">Migrace z předchozích verzí rozšíření</span><span class="sxs-lookup"><span data-stu-id="bf3b1-146">Migration from previous versions of the extension</span></span>

<span data-ttu-id="bf3b1-147">Nejnovější verze rozšíření je **3.0**.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-147">The latest version of the extension is **3.0**.</span></span> <span data-ttu-id="bf3b1-148">**Všechny starší verze (2.x) jsou zastaralé a může být publikování na nebo po 31. července 2018**.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-148">**Any old versions (2.x) are deprecated and may be unpublished on or after July 31, 2018**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bf3b1-149">Toto rozšíření zavádí nejnovější změny v konfiguraci rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-149">This extension introduces breaking changes to the configuration of the extension.</span></span> <span data-ttu-id="bf3b1-150">Pro zlepšení zabezpečení rozšíření; provedla jeden taková změna v důsledku toho zpětné kompatibility s 2.x nebylo možné udržovat.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-150">One such change was made to improve the security of the extension; as a result, backwards compatibility with 2.x could not be maintained.</span></span> <span data-ttu-id="bf3b1-151">Navíc se liší od vydavatele pro verze 2.x vydavatel rozšíření pro tuto příponu.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-151">Also, the Extension Publisher for this extension is different than the publisher for the 2.x versions.</span></span>
>
> <span data-ttu-id="bf3b1-152">Při migraci z 2.x do této nové verzi rozšíření, je nutné odinstalovat původní rozšíření (pod původním názvem vydavatele) a potom nainstalovat verzi 3 rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-152">To migrate from 2.x to this new version of the extension, you must uninstall the old extension (under the old publisher name), then install version 3 of the extension.</span></span>

<span data-ttu-id="bf3b1-153">Doporučení:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-153">Recommendations:</span></span>

* <span data-ttu-id="bf3b1-154">Nainstalujte rozšíření automatické podverze upgradu povolena.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-154">Install the extension with automatic minor version upgrade enabled.</span></span>
  * <span data-ttu-id="bf3b1-155">V modelu nasazení classic virtuální počítače zadejte '3.*, jako je verze při instalaci rozšíření prostřednictvím příkazového řádku Azure XPLAT nebo Powershellu.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-155">On classic deployment model VMs, specify '3.*' as the version if you are installing the extension through Azure XPLAT CLI or Powershell.</span></span>
  * <span data-ttu-id="bf3b1-156">Při nasazení Azure Resource Manager modelu virtuálních počítačů, zahrnují ' "autoUpgradeMinorVersion": true, v nasazení šablony virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-156">On Azure Resource Manager deployment model VMs, include '"autoUpgradeMinorVersion": true' in the VM deployment template.</span></span>
* <span data-ttu-id="bf3b1-157">Použijte nový nebo jiný účet úložiště pro LAD 3.0.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-157">Use a new/different storage account for LAD 3.0.</span></span> <span data-ttu-id="bf3b1-158">Existuje několik malých nekompatibility mezi LAD 2.3 a LAD 3.0, které usnadňují sdílení problémových účet:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-158">There are several small incompatibilities between LAD 2.3 and LAD 3.0 that make sharing an account troublesome:</span></span>
  * <span data-ttu-id="bf3b1-159">LAD 3.0 ukládá události procesu syslog v tabulce s jiným názvem.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-159">LAD 3.0 stores syslog events in a table with a different name.</span></span>
  * <span data-ttu-id="bf3b1-160">CounterSpecifier řetězce pro `builtin` metriky rozdílnou LAD 3.0.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-160">The counterSpecifier strings for `builtin` metrics differ in LAD 3.0.</span></span>

## <a name="protected-settings"></a><span data-ttu-id="bf3b1-161">Chráněné nastavení</span><span class="sxs-lookup"><span data-stu-id="bf3b1-161">Protected settings</span></span>

<span data-ttu-id="bf3b1-162">Tato sada informace o konfiguraci obsahuje citlivé informace, které by měly být chráněné z veřejného zobrazení, například přihlašovací údaje úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-162">This set of configuration information contains sensitive information that should be protected from public view, for example, storage credentials.</span></span> <span data-ttu-id="bf3b1-163">Tato nastavení jsou předávány a uložená rozšíření v šifrovaném tvaru.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-163">These settings are transmitted to and stored by the extension in encrypted form.</span></span>

```json
{
    "storageAccountName" : "the storage account to receive data",
    "storageAccountEndPoint": "the hostname suffix for the cloud for this account",
    "storageAccountSasToken": "SAS access token",
    "mdsdHttpProxy": "HTTP proxy settings",
    "sinksConfig": { ... }
}
```

<span data-ttu-id="bf3b1-164">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="bf3b1-164">Name</span></span> | <span data-ttu-id="bf3b1-165">Hodnota</span><span class="sxs-lookup"><span data-stu-id="bf3b1-165">Value</span></span>
---- | -----
<span data-ttu-id="bf3b1-166">storageAccountName</span><span class="sxs-lookup"><span data-stu-id="bf3b1-166">storageAccountName</span></span> | <span data-ttu-id="bf3b1-167">Název účtu úložiště, ve kterém je rozšíření zapisovat data.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-167">The name of the storage account in which data is written by the extension.</span></span>
<span data-ttu-id="bf3b1-168">storageAccountEndPoint</span><span class="sxs-lookup"><span data-stu-id="bf3b1-168">storageAccountEndPoint</span></span> | <span data-ttu-id="bf3b1-169">(volitelné) Koncový bod identifikace cloudu, ve kterém existuje účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-169">(optional) The endpoint identifying the cloud in which the storage account exists.</span></span> <span data-ttu-id="bf3b1-170">Chybí-li toto nastavení, LAD výchozí veřejného cloudu Azure `https://core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-170">If this setting is absent, LAD defaults to the Azure public cloud, `https://core.windows.net`.</span></span> <span data-ttu-id="bf3b1-171">Pokud chcete používat účet úložiště v Azure v Německu, Azure Government nebo Azure China, tato hodnota odpovídajícím způsobem nastavte.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-171">To use a storage account in Azure Germany, Azure Government, or Azure China, set this value accordingly.</span></span>
<span data-ttu-id="bf3b1-172">storageAccountSasToken</span><span class="sxs-lookup"><span data-stu-id="bf3b1-172">storageAccountSasToken</span></span> | <span data-ttu-id="bf3b1-173">[Tokenu SAS účtu](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) pro služby objektů Blob a tabulky (`ss='bt'`), platí pro kontejnery a objekty (`srt='co'`), která uděluje přidat, vytvářet, seznam, aktualizovat a oprávnění k zápisu (`sp='acluw'`).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-173">An [Account SAS token](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) for Blob and Table services (`ss='bt'`), applicable to containers and objects (`srt='co'`), which grants add, create, list, update, and write permissions (`sp='acluw'`).</span></span> <span data-ttu-id="bf3b1-174">Proveďte *není* zahrnují na úvodní otazník (?).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-174">Do *not* include the leading question-mark (?).</span></span>
<span data-ttu-id="bf3b1-175">mdsdHttpProxy</span><span class="sxs-lookup"><span data-stu-id="bf3b1-175">mdsdHttpProxy</span></span> | <span data-ttu-id="bf3b1-176">(volitelné) Informace o proxy serveru HTTP potřebnými k povolení rozšíření pro připojení k zadaný účet úložiště a koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-176">(optional) HTTP proxy information needed to enable the extension to connect to the specified storage account and endpoint.</span></span>
<span data-ttu-id="bf3b1-177">sinksConfig</span><span class="sxs-lookup"><span data-stu-id="bf3b1-177">sinksConfig</span></span> | <span data-ttu-id="bf3b1-178">(volitelné) Podrobnosti o alternativní umístění, do kterých mohou být zajišťovány metriky a události.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-178">(optional) Details of alternative destinations to which metrics and events can be delivered.</span></span> <span data-ttu-id="bf3b1-179">Konkrétní podrobnosti o každém jímku dat nepodporuje rozšíření jsou popsané v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-179">The specific details of each data sink supported by the extension are covered in the sections that follow.</span></span>

<span data-ttu-id="bf3b1-180">Můžete snadno vytvořit požadovaný token SAS prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-180">You can easily construct the required SAS token through the Azure portal.</span></span>

1. <span data-ttu-id="bf3b1-181">Vyberte účet úložiště pro obecné účely, u kterého chcete rozšíření pro zápis</span><span class="sxs-lookup"><span data-stu-id="bf3b1-181">Select the general-purpose storage account to which you want the extension to write</span></span>
1. <span data-ttu-id="bf3b1-182">Vyberte "Sdílený přístupový podpis" z nastavení součástí v levé nabídce</span><span class="sxs-lookup"><span data-stu-id="bf3b1-182">Select "Shared access signature" from the Settings part of the left menu</span></span>
1. <span data-ttu-id="bf3b1-183">Ujistěte se, jak se popisuje výš příslušné části</span><span class="sxs-lookup"><span data-stu-id="bf3b1-183">Make the appropriate sections as previously described</span></span>
1. <span data-ttu-id="bf3b1-184">Klikněte na tlačítko "Generovat SAS".</span><span class="sxs-lookup"><span data-stu-id="bf3b1-184">Click the "Generate SAS" button.</span></span>

![Bitové kopie](./media/diagnostic-extension/make_sas.png)

<span data-ttu-id="bf3b1-186">Zkopírujte vygenerovaný SAS do pole storageAccountSasToken; odebrat úvodní otazník ("?").</span><span class="sxs-lookup"><span data-stu-id="bf3b1-186">Copy the generated SAS into the storageAccountSasToken field; remove the leading question-mark ("?").</span></span>

### <a name="sinksconfig"></a><span data-ttu-id="bf3b1-187">sinksConfig</span><span class="sxs-lookup"><span data-stu-id="bf3b1-187">sinksConfig</span></span>

```json
"sinksConfig": {
    "sink": [
        {
            "name": "sinkname",
            "type": "sinktype",
            ...
        },
        ...
    ]
},
```

<span data-ttu-id="bf3b1-188">Tento volitelný oddíl definuje další cílových míst, na které rozšíření odešle informace, které shromáždí.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-188">This optional section defines additional destinations to which the extension sends the information it collects.</span></span> <span data-ttu-id="bf3b1-189">Pole "podřízený" obsahuje objekt pro každý další data jímky.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-189">The "sink" array contains an object for each additional data sink.</span></span> <span data-ttu-id="bf3b1-190">Atributu "typ" Určuje atributy v objektu.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-190">The "type" attribute determines the other attributes in the object.</span></span>

<span data-ttu-id="bf3b1-191">Element</span><span class="sxs-lookup"><span data-stu-id="bf3b1-191">Element</span></span> | <span data-ttu-id="bf3b1-192">Hodnota</span><span class="sxs-lookup"><span data-stu-id="bf3b1-192">Value</span></span>
------- | -----
<span data-ttu-id="bf3b1-193">jméno</span><span class="sxs-lookup"><span data-stu-id="bf3b1-193">name</span></span> | <span data-ttu-id="bf3b1-194">Řetězec se používá k odkazování na tento podřízený jinde v konfiguraci rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-194">A string used to refer to this sink elsewhere in the extension configuration.</span></span>
<span data-ttu-id="bf3b1-195">type</span><span class="sxs-lookup"><span data-stu-id="bf3b1-195">type</span></span> | <span data-ttu-id="bf3b1-196">Typ jímky definovaný.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-196">The type of sink being defined.</span></span> <span data-ttu-id="bf3b1-197">Určuje ostatní hodnoty (pokud existuje) v instance tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-197">Determines the other values (if any) in instances of this type.</span></span>

<span data-ttu-id="bf3b1-198">Verze 3.0 rozšíření diagnostiky Linux podporuje dva typy podřízený: EventHub a JsonBlob.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-198">Version 3.0 of the Linux Diagnostic Extension supports two sink types: EventHub, and JsonBlob.</span></span>

#### <a name="the-eventhub-sink"></a><span data-ttu-id="bf3b1-199">Podřízený EventHub</span><span class="sxs-lookup"><span data-stu-id="bf3b1-199">The EventHub sink</span></span>

```json
"sink": [
    {
        "name": "sinkname",
        "type": "EventHub",
        "sasURL": "https SAS URL"
    },
    ...
]
```

<span data-ttu-id="bf3b1-200">Položka "sasURL" obsahuje úplnou adresu URL, včetně tokenu SAS pro centra událostí, ke které je nutné ji publikovat data.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-200">The "sasURL" entry contains the full URL, including SAS token, for the Event Hub to which data should be published.</span></span> <span data-ttu-id="bf3b1-201">LAD vyžaduje SAS pojmenování zásadu, která umožňuje odesílání deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-201">LAD requires a SAS naming a policy that enables the Send claim.</span></span> <span data-ttu-id="bf3b1-202">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-202">An example:</span></span>

* <span data-ttu-id="bf3b1-203">Vytvořit obor názvů Event Hubs s názvem`contosohub`</span><span class="sxs-lookup"><span data-stu-id="bf3b1-203">Create an Event Hubs namespace called `contosohub`</span></span>
* <span data-ttu-id="bf3b1-204">Vytvoření centra událostí v oboru názvů názvem`syslogmsgs`</span><span class="sxs-lookup"><span data-stu-id="bf3b1-204">Create an Event Hub in the namespace called `syslogmsgs`</span></span>
* <span data-ttu-id="bf3b1-205">Vytvoření zásady sdíleného přístupu v Centru událostí s názvem `writer` umožňující odesílat deklarace identity</span><span class="sxs-lookup"><span data-stu-id="bf3b1-205">Create a Shared access policy on the Event Hub named `writer` that enables the Send claim</span></span>

<span data-ttu-id="bf3b1-206">Pokud jste vytvořili SAS funkční až půlnoci času UTC na 1. ledna 2018, hodnota sasURL může být:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-206">If you created a SAS good until midnight UTC on January 1, 2018, the sasURL value might be:</span></span>

```url
https://contosohub.servicebus.windows.net/syslogmsgs?sr=contosohub.servicebus.windows.net%2fsyslogmsgs&sig=xxxxxxxxxxxxxxxxxxxxxxxxx&se=1514764800&skn=writer
```

<span data-ttu-id="bf3b1-207">Další informace o generování tokeny SAS pro Event Hubs naleznete v tématu [této webové stránce](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-207">For more information about generating SAS tokens for Event Hubs, see [this web page](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).</span></span>

#### <a name="the-jsonblob-sink"></a><span data-ttu-id="bf3b1-208">Podřízený JsonBlob</span><span class="sxs-lookup"><span data-stu-id="bf3b1-208">The JsonBlob sink</span></span>

```json
"sink": [
    {
        "name": "sinkname",
        "type": "JsonBlob"
    },
    ...
]
```

<span data-ttu-id="bf3b1-209">Data směrované podřízený JsonBlob se ukládají do objektů BLOB v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-209">Data directed to a JsonBlob sink is stored in blobs in Azure storage.</span></span> <span data-ttu-id="bf3b1-210">Každá instance LAD vytvoří každou hodinu pro každý název podřízený objekt blob.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-210">Each instance of LAD creates a blob every hour for each sink name.</span></span> <span data-ttu-id="bf3b1-211">Každý objekt blob vždy obsahuje syntakticky pole JSON objektu.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-211">Each blob always contains a syntactically valid JSON array of object.</span></span> <span data-ttu-id="bf3b1-212">Do pole jsou atomicky přidány nové položky.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-212">New entries are atomically added to the array.</span></span> <span data-ttu-id="bf3b1-213">Objekty BLOB jsou uloženy v kontejneru se stejným názvem jako jímky.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-213">Blobs are stored in a container with the same name as the sink.</span></span> <span data-ttu-id="bf3b1-214">Úložiště Azure pravidla pro názvy kontejneru objektů blob platí pro názvy JsonBlob jímky: mezi 3 a 63 malé alfanumerické znaky ASCII nebo pomlčky.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-214">The Azure storage rules for blob container names apply to the names of JsonBlob sinks: between 3 and 63 lower-case alphanumeric ASCII characters or dashes.</span></span>

## <a name="public-settings"></a><span data-ttu-id="bf3b1-215">Nastavení veřejné</span><span class="sxs-lookup"><span data-stu-id="bf3b1-215">Public settings</span></span>

<span data-ttu-id="bf3b1-216">Tato struktura obsahuje různé bloky nastavení, které řídí údaje shromážděné pomocí rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-216">This structure contains various blocks of settings that control the information collected by the extension.</span></span> <span data-ttu-id="bf3b1-217">Každé nastavení je volitelné.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-217">Each setting is optional.</span></span> <span data-ttu-id="bf3b1-218">Pokud zadáte `ladCfg`, musíte zadat také `StorageAccount`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-218">If you specify `ladCfg`, you must also specify `StorageAccount`.</span></span>

```json
{
    "ladCfg":  { ... },
    "perfCfg": { ... },
    "fileLogs": { ... },
    "StorageAccount": "the storage account to receive data",
    "mdsdHttpProxy" : ""
}
```

<span data-ttu-id="bf3b1-219">Element</span><span class="sxs-lookup"><span data-stu-id="bf3b1-219">Element</span></span> | <span data-ttu-id="bf3b1-220">Hodnota</span><span class="sxs-lookup"><span data-stu-id="bf3b1-220">Value</span></span>
------- | -----
<span data-ttu-id="bf3b1-221">StorageAccount</span><span class="sxs-lookup"><span data-stu-id="bf3b1-221">StorageAccount</span></span> | <span data-ttu-id="bf3b1-222">Název účtu úložiště, ve kterém je rozšíření zapisovat data.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-222">The name of the storage account in which data is written by the extension.</span></span> <span data-ttu-id="bf3b1-223">Musí být stejný jako název, jako jsou zadány v [chráněné nastavení](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-223">Must be the same name as is specified in the [Protected settings](#protected-settings).</span></span>
<span data-ttu-id="bf3b1-224">mdsdHttpProxy</span><span class="sxs-lookup"><span data-stu-id="bf3b1-224">mdsdHttpProxy</span></span> | <span data-ttu-id="bf3b1-225">(volitelné) Stejné jako v [chráněné nastavení](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-225">(optional) Same as in the [Protected settings](#protected-settings).</span></span> <span data-ttu-id="bf3b1-226">Hodnota veřejného je přepsat privátní hodnotou, pokud nastavení.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-226">The public value is overridden by the private value, if set.</span></span> <span data-ttu-id="bf3b1-227">Nastavení proxy serveru, které obsahují tajného klíče, jako jsou hesla, v umístit [chráněné nastavení](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-227">Place proxy settings that contain a secret, such as a password, in the [Protected settings](#protected-settings).</span></span>

<span data-ttu-id="bf3b1-228">Zbývající prvky jsou podrobně popsané v v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-228">The remaining elements are described in detail in the following sections.</span></span>

### <a name="ladcfg"></a><span data-ttu-id="bf3b1-229">ladCfg</span><span class="sxs-lookup"><span data-stu-id="bf3b1-229">ladCfg</span></span>

```json
"ladCfg": {
    "diagnosticMonitorConfiguration": {
        "eventVolume": "Medium",
        "metrics": { ... },
        "performanceCounters": { ... },
        "syslogEvents": { ... }
    },
    "sampleRateInSeconds": 15
}
```

<span data-ttu-id="bf3b1-230">Ovládací prvky této volitelné struktura jímky shromažďování metrik a protokoly pro doručení ke službě Azure metriky a další data.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-230">This optional structure controls the gathering of metrics and logs for delivery to the Azure Metrics service and to other data sinks.</span></span> <span data-ttu-id="bf3b1-231">Je nutné zadat buď `performanceCounters` nebo `syslogEvents` nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-231">You must specify either `performanceCounters` or `syslogEvents` or both.</span></span> <span data-ttu-id="bf3b1-232">Je nutné zadat `metrics` struktura.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-232">You must specify the `metrics` structure.</span></span>

<span data-ttu-id="bf3b1-233">Element</span><span class="sxs-lookup"><span data-stu-id="bf3b1-233">Element</span></span> | <span data-ttu-id="bf3b1-234">Hodnota</span><span class="sxs-lookup"><span data-stu-id="bf3b1-234">Value</span></span>
------- | -----
<span data-ttu-id="bf3b1-235">eventVolume</span><span class="sxs-lookup"><span data-stu-id="bf3b1-235">eventVolume</span></span> | <span data-ttu-id="bf3b1-236">(volitelné) Určuje počet oddílů vytvořit v tabulce úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-236">(optional) Controls the number of partitions created within the storage table.</span></span> <span data-ttu-id="bf3b1-237">Musí mít jednu z `"Large"`, `"Medium"`, nebo `"Small"`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-237">Must be one of `"Large"`, `"Medium"`, or `"Small"`.</span></span> <span data-ttu-id="bf3b1-238">Pokud není zadáno, výchozí hodnota je `"Medium"`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-238">If not specified, the default value is `"Medium"`.</span></span>
<span data-ttu-id="bf3b1-239">sampleRateInSeconds</span><span class="sxs-lookup"><span data-stu-id="bf3b1-239">sampleRateInSeconds</span></span> | <span data-ttu-id="bf3b1-240">(volitelné) Výchozí interval mezi kolekce nezpracovaná (neagregovaným) metrik.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-240">(optional) The default interval between collection of raw (unaggregated) metrics.</span></span> <span data-ttu-id="bf3b1-241">Nejmenší podporované vzorkovací frekvence je 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-241">The smallest supported sample rate is 15 seconds.</span></span> <span data-ttu-id="bf3b1-242">Pokud není zadáno, výchozí hodnota je `15`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-242">If not specified, the default value is `15`.</span></span>

#### <a name="metrics"></a><span data-ttu-id="bf3b1-243">Průzkumníku metrik</span><span class="sxs-lookup"><span data-stu-id="bf3b1-243">metrics</span></span>

```json
"metrics": {
    "resourceId": "/subscriptions/...",
    "metricAggregation" : [
        { "scheduledTransferPeriod" : "PT1H" },
        { "scheduledTransferPeriod" : "PT5M" }
    ]
}
```

<span data-ttu-id="bf3b1-244">Element</span><span class="sxs-lookup"><span data-stu-id="bf3b1-244">Element</span></span> | <span data-ttu-id="bf3b1-245">Hodnota</span><span class="sxs-lookup"><span data-stu-id="bf3b1-245">Value</span></span>
------- | -----
<span data-ttu-id="bf3b1-246">resourceId</span><span class="sxs-lookup"><span data-stu-id="bf3b1-246">resourceId</span></span> | <span data-ttu-id="bf3b1-247">Nastavit ID prostředku Azure Resource Manager virtuálního počítače nebo virtuálního počítače měřítka, ke které patří virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-247">The Azure Resource Manager resource ID of the VM or of the virtual machine scale set to which the VM belongs.</span></span> <span data-ttu-id="bf3b1-248">Toto nastavení musí být zadaná také pokud žádné podřízený JsonBlob se používá v konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-248">This setting must be also specified if any JsonBlob sink is used in the configuration.</span></span>
<span data-ttu-id="bf3b1-249">scheduledTransferPeriod</span><span class="sxs-lookup"><span data-stu-id="bf3b1-249">scheduledTransferPeriod</span></span> | <span data-ttu-id="bf3b1-250">Frekvence, kdy jsou agregovaná metrika počítaný a přenést do Azure metriky, vyjádřené jako interval času je 8601.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-250">The frequency at which aggregate metrics are to be computed and transferred to Azure Metrics, expressed as an IS 8601 time interval.</span></span> <span data-ttu-id="bf3b1-251">V nejmenší období je 60 sekund, který je PT1M.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-251">The smallest transfer period is 60 seconds, that is, PT1M.</span></span> <span data-ttu-id="bf3b1-252">Je nutné zadat alespoň jeden scheduledTransferPeriod.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-252">You must specify at least one scheduledTransferPeriod.</span></span>

<span data-ttu-id="bf3b1-253">Ukázky metrik zadané v části čítače výkonu se shromažďují každých 15 sekund nebo v ukázce míra explicitně definované čítače.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-253">Samples of the metrics specified in the performanceCounters section are collected every 15 seconds or at the sample rate explicitly defined for the counter.</span></span> <span data-ttu-id="bf3b1-254">Pokud se zobrazí více scheduledTransferPeriod frekvence (jako v příkladu), každé agregace se počítá samostatně.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-254">If multiple scheduledTransferPeriod frequencies appear (as in the example), each aggregation is computed independently.</span></span>

#### <a name="performancecounters"></a><span data-ttu-id="bf3b1-255">čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="bf3b1-255">performanceCounters</span></span>

```json
"performanceCounters": {
    "sinks": "",
    "performanceCounterConfiguration": [
        {
            "type": "builtin",
            "class": "Processor",
            "counter": "PercentIdleTime",
            "counterSpecifier": "/builtin/Processor/PercentIdleTime",
            "condition": "IsAggregate=TRUE",
            "sampleRate": "PT15S",
            "unit": "Percent",
            "annotation": [
                {
                    "displayName" : "Aggregate CPU %idle time",
                    "locale" : "en-us"
                }
            ]
        }
    ]
}
```

<span data-ttu-id="bf3b1-256">Tento volitelný oddíl pod kontrolou shromažďování metrik.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-256">This optional section controls the collection of metrics.</span></span> <span data-ttu-id="bf3b1-257">Nezpracovaná ukázky jsou agregována pro každou [scheduledTransferPeriod](#metrics) k vytvoření tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-257">Raw samples are aggregated for each [scheduledTransferPeriod](#metrics) to produce these values:</span></span>

* <span data-ttu-id="bf3b1-258">střední</span><span class="sxs-lookup"><span data-stu-id="bf3b1-258">mean</span></span>
* <span data-ttu-id="bf3b1-259">minimální</span><span class="sxs-lookup"><span data-stu-id="bf3b1-259">minimum</span></span>
* <span data-ttu-id="bf3b1-260">Maximální počet</span><span class="sxs-lookup"><span data-stu-id="bf3b1-260">maximum</span></span>
* <span data-ttu-id="bf3b1-261">shromážděné poslední hodnota</span><span class="sxs-lookup"><span data-stu-id="bf3b1-261">last-collected value</span></span>
* <span data-ttu-id="bf3b1-262">počet nezpracovaných ukázky slouží k výpočtu agregace</span><span class="sxs-lookup"><span data-stu-id="bf3b1-262">count of raw samples used to compute the aggregate</span></span>

<span data-ttu-id="bf3b1-263">Element</span><span class="sxs-lookup"><span data-stu-id="bf3b1-263">Element</span></span> | <span data-ttu-id="bf3b1-264">Hodnota</span><span class="sxs-lookup"><span data-stu-id="bf3b1-264">Value</span></span>
------- | -----
<span data-ttu-id="bf3b1-265">jímky</span><span class="sxs-lookup"><span data-stu-id="bf3b1-265">sinks</span></span> | <span data-ttu-id="bf3b1-266">(volitelné) Seznam názvů jímky, do které LAD zasílá agregovat metriky výsledky oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-266">(optional) A comma-separated list of names of sinks to which LAD sends aggregated metric results.</span></span> <span data-ttu-id="bf3b1-267">Všechna agregovaná metrika se publikují do jednotlivých uvedených jímky.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-267">All aggregated metrics are published to each listed sink.</span></span> <span data-ttu-id="bf3b1-268">V tématu [sinksConfig](#sinksconfig).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-268">See [sinksConfig](#sinksconfig).</span></span> <span data-ttu-id="bf3b1-269">Příklad: `"EHsink1, myjsonsink"`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-269">Example: `"EHsink1, myjsonsink"`.</span></span>
<span data-ttu-id="bf3b1-270">type</span><span class="sxs-lookup"><span data-stu-id="bf3b1-270">type</span></span> | <span data-ttu-id="bf3b1-271">Identifikuje skutečné zprostředkovatele metriky.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-271">Identifies the actual provider of the metric.</span></span>
<span data-ttu-id="bf3b1-272">– Třída</span><span class="sxs-lookup"><span data-stu-id="bf3b1-272">class</span></span> | <span data-ttu-id="bf3b1-273">Společně s "čítač" jsou uvedeny konkrétní metriky v rámci oboru názvů poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-273">Together with "counter", identifies the specific metric within the provider's namespace.</span></span>
<span data-ttu-id="bf3b1-274">Čítač</span><span class="sxs-lookup"><span data-stu-id="bf3b1-274">counter</span></span> | <span data-ttu-id="bf3b1-275">Společně s "class" jsou uvedeny konkrétní metriky v rámci oboru názvů poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-275">Together with "class", identifies the specific metric within the provider's namespace.</span></span>
<span data-ttu-id="bf3b1-276">counterSpecifier</span><span class="sxs-lookup"><span data-stu-id="bf3b1-276">counterSpecifier</span></span> | <span data-ttu-id="bf3b1-277">Identifikuje určité metriky v rámci oboru názvů metrik Azure.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-277">Identifies the specific metric within the Azure Metrics namespace.</span></span>
<span data-ttu-id="bf3b1-278">Podmínka</span><span class="sxs-lookup"><span data-stu-id="bf3b1-278">condition</span></span> | <span data-ttu-id="bf3b1-279">(volitelné) Vybere konkrétní instanci objektu do které metriku platí nebo vybere agregace napříč všemi instancemi tohoto objektu.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-279">(optional) Selects a specific instance of the object to which the metric applies or selects the aggregation across all instances of that object.</span></span> <span data-ttu-id="bf3b1-280">Další informace najdete v tématu [ `builtin` definice metrik](#metrics-supported-by-builtin).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-280">For more information, see the [`builtin` metric definitions](#metrics-supported-by-builtin).</span></span>
<span data-ttu-id="bf3b1-281">sampleRate</span><span class="sxs-lookup"><span data-stu-id="bf3b1-281">sampleRate</span></span> | <span data-ttu-id="bf3b1-282">Interval 8601, která nastavuje rychlost, jakou jsou shromažďovány nezpracovaná ukázky pro tato metrika je.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-282">IS 8601 interval that sets the rate at which raw samples for this metric are collected.</span></span> <span data-ttu-id="bf3b1-283">Pokud není nastavena, je kolekce interval nastaven hodnotou [sampleRateInSeconds](#ladcfg).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-283">If not set, the collection interval is set by the value of [sampleRateInSeconds](#ladcfg).</span></span> <span data-ttu-id="bf3b1-284">Nejkratší podporované vzorkovací frekvence je 15 sekund (PT15S).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-284">The shortest supported sample rate is 15 seconds (PT15S).</span></span>
<span data-ttu-id="bf3b1-285">jednotka</span><span class="sxs-lookup"><span data-stu-id="bf3b1-285">unit</span></span> | <span data-ttu-id="bf3b1-286">By měl být jeden z těchto řetězců: "Count", "Bajtů", "Seconds", "Procenta", "CountPerSecond", "BytesPerSecond", "Milisekundu".</span><span class="sxs-lookup"><span data-stu-id="bf3b1-286">Should be one of these strings: "Count", "Bytes", "Seconds", "Percent", "CountPerSecond", "BytesPerSecond", "Millisecond".</span></span> <span data-ttu-id="bf3b1-287">Definuje jednotky pro metriku.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-287">Defines the unit for the metric.</span></span> <span data-ttu-id="bf3b1-288">Příjemci shromážděná data očekávat shromážděná data hodnoty tak, aby odpovídaly tuto jednotku.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-288">Consumers of the collected data expect the collected data values to match this unit.</span></span> <span data-ttu-id="bf3b1-289">LAD ignoruje toto pole.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-289">LAD ignores this field.</span></span>
<span data-ttu-id="bf3b1-290">displayName</span><span class="sxs-lookup"><span data-stu-id="bf3b1-290">displayName</span></span> | <span data-ttu-id="bf3b1-291">Popisek (v jazyce určeném nastavením přidružené národní prostředí) být připojené k těmto datům v Azure metriky.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-291">The label (in the language specified by the associated locale setting) to be attached to this data in Azure Metrics.</span></span> <span data-ttu-id="bf3b1-292">LAD ignoruje toto pole.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-292">LAD ignores this field.</span></span>

<span data-ttu-id="bf3b1-293">CounterSpecifier je libovolný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-293">The counterSpecifier is an arbitrary identifier.</span></span> <span data-ttu-id="bf3b1-294">Příjemci metrik, Azure portálu grafů, jako a výstrahy funkce, použijte counterSpecifier jako "klíč", který identifikuje metriky nebo instanci metriky.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-294">Consumers of metrics, like the Azure portal charting and alerting feature, use counterSpecifier as the "key" that identifies a metric or an instance of a metric.</span></span> <span data-ttu-id="bf3b1-295">Pro `builtin` metriky, doporučujeme použít counterSpecifier hodnoty, které začínají `/builtin/`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-295">For `builtin` metrics, we recommend you use counterSpecifier values that begin with `/builtin/`.</span></span> <span data-ttu-id="bf3b1-296">Pokud shromažďujete konkrétní instanci metriky, doporučujeme, abyste že na hodnotu counterSpecifier připojíte identifikátor instance.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-296">If you are collecting a specific instance of a metric, we recommend you attach the identifier of the instance to the counterSpecifier value.</span></span> <span data-ttu-id="bf3b1-297">Příklady:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-297">Some examples:</span></span>

* <span data-ttu-id="bf3b1-298">`/builtin/Processor/PercentIdleTime`-Doba nečinnosti, po průměrem všech jader</span><span class="sxs-lookup"><span data-stu-id="bf3b1-298">`/builtin/Processor/PercentIdleTime` - Idle time averaged across all cores</span></span>
* <span data-ttu-id="bf3b1-299">`/builtin/Disk/FreeSpace(/mnt)`-Volného místa pro systém souborů /mnt</span><span class="sxs-lookup"><span data-stu-id="bf3b1-299">`/builtin/Disk/FreeSpace(/mnt)` - Free space for the /mnt filesystem</span></span>
* <span data-ttu-id="bf3b1-300">`/builtin/Disk/FreeSpace`-Volného místa průměrem všech připojené systémy</span><span class="sxs-lookup"><span data-stu-id="bf3b1-300">`/builtin/Disk/FreeSpace` - Free space averaged across all mounted filesystems</span></span>

<span data-ttu-id="bf3b1-301">LAD ani portálu Azure očekává, že hodnota counterSpecifier odpovídat žádnému vzoru.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-301">Neither LAD nor the Azure portal expects the counterSpecifier value to match any pattern.</span></span> <span data-ttu-id="bf3b1-302">Být konzistentní v tom, jak vytvořit counterSpecifier hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-302">Be consistent in how you construct counterSpecifier values.</span></span>

<span data-ttu-id="bf3b1-303">Pokud zadáte `performanceCounters`, LAD vždy zapisuje data do tabulky v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-303">When you specify `performanceCounters`, LAD always writes data to a table in Azure storage.</span></span> <span data-ttu-id="bf3b1-304">Může mít stejné dat zapsaných na objekty BLOB JSON nebo Event Hubs, ale nelze zakázat ukládání dat do tabulky.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-304">You can have the same data written to JSON blobs and/or Event Hubs, but you cannot disable storing data to a table.</span></span> <span data-ttu-id="bf3b1-305">Všechny instance rozšíření diagnostiky nakonfigurovaný tak, aby používají stejný název účtu úložiště a koncový bod přidejte protokoly a metriky do stejné tabulky.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-305">All instances of the diagnostic extension configured to use the same storage account name and endpoint add their metrics and logs to the same table.</span></span> <span data-ttu-id="bf3b1-306">Pokud jsou moc velký počet virtuálních počítačů zápis do stejného oddílu tabulky, Azure může omezit zápisy do tohoto oddílu.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-306">If too many VMs are writing to the same table partition, Azure can throttle writes to that partition.</span></span> <span data-ttu-id="bf3b1-307">Nastavení eventVolume způsobí, že položky šíření mezi 1 (malé), 10 (střední), nebo 100 různých oddílů (velká).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-307">The eventVolume setting causes entries to be spread across 1 (Small), 10 (Medium), or 100 (Large) different partitions.</span></span> <span data-ttu-id="bf3b1-308">Obvykle je dostačující Ujistěte se, že není omezen provoz "Střední".</span><span class="sxs-lookup"><span data-stu-id="bf3b1-308">Usually, "Medium" is sufficient to ensure traffic is not throttled.</span></span> <span data-ttu-id="bf3b1-309">K vytvoření grafy nebo výstrahy aktivovat funkci Azure metriky na portálu Azure používá data v této tabulce.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-309">The Azure Metrics feature of the Azure portal uses the data in this table to produce graphs or to trigger alerts.</span></span> <span data-ttu-id="bf3b1-310">Název tabulky je zřetězení tyto řetězce:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-310">The table name is the concatenation of these strings:</span></span>

* `WADMetrics`
* <span data-ttu-id="bf3b1-311">"ScheduledTransferPeriod" pro agregované hodnoty, které jsou uložené v tabulce</span><span class="sxs-lookup"><span data-stu-id="bf3b1-311">The "scheduledTransferPeriod" for the aggregated values stored in the table</span></span>
* `P10DV2S`
* <span data-ttu-id="bf3b1-312">Datum ve formátu "RRRRMMDD", který změní každých 10 dní</span><span class="sxs-lookup"><span data-stu-id="bf3b1-312">A date, in the form "YYYYMMDD", which changes every 10 days</span></span>

<span data-ttu-id="bf3b1-313">Mezi příklady patří `WADMetricsPT1HP10DV2S20170410` a `WADMetricsPT1MP10DV2S20170609`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-313">Examples include `WADMetricsPT1HP10DV2S20170410` and `WADMetricsPT1MP10DV2S20170609`.</span></span>

#### <a name="syslogevents"></a><span data-ttu-id="bf3b1-314">syslogEvents</span><span class="sxs-lookup"><span data-stu-id="bf3b1-314">syslogEvents</span></span>

```json
"syslogEvents": {
    "sinks": "",
    "syslogEventConfiguration": {
        "facilityName1": "minSeverity",
        "facilityName2": "minSeverity",
        ...
    }
}
```

<span data-ttu-id="bf3b1-315">Tento volitelný oddíl ovládací prvky kolekce protokolu událostí z syslog.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-315">This optional section controls the collection of log events from syslog.</span></span> <span data-ttu-id="bf3b1-316">Pokud je vynechán části, nejsou vůbec zachytit události procesu syslog.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-316">If the section is omitted, syslog events are not captured at all.</span></span>

<span data-ttu-id="bf3b1-317">Kolekce syslogEventConfiguration má jeden záznam pro každý mechanismus syslog, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-317">The syslogEventConfiguration collection has one entry for each syslog facility of interest.</span></span> <span data-ttu-id="bf3b1-318">Pokud minSeverity "Žádný" je pro konkrétní zařízení, nebo pokud tohoto zařízení nezobrazí v elementu vůbec, jsou zachyceny žádné události z tohoto zařízení.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-318">If minSeverity is "NONE" for a particular facility, or if that facility does not appear in the element at all, no events from that facility are captured.</span></span>

<span data-ttu-id="bf3b1-319">Element</span><span class="sxs-lookup"><span data-stu-id="bf3b1-319">Element</span></span> | <span data-ttu-id="bf3b1-320">Hodnota</span><span class="sxs-lookup"><span data-stu-id="bf3b1-320">Value</span></span>
------- | -----
<span data-ttu-id="bf3b1-321">jímky</span><span class="sxs-lookup"><span data-stu-id="bf3b1-321">sinks</span></span> | <span data-ttu-id="bf3b1-322">Seznam názvů jímky, na které jsou publikovány jednotlivých protokolu událostí oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-322">A comma-separated list of names of sinks to which individual log events are published.</span></span> <span data-ttu-id="bf3b1-323">Všechny události protokolu odpovídající omezení v syslogEventConfiguration se publikují do jednotlivých uvedených jímky.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-323">All log events matching the restrictions in syslogEventConfiguration are published to each listed sink.</span></span> <span data-ttu-id="bf3b1-324">Příklad: "EHforsyslog"</span><span class="sxs-lookup"><span data-stu-id="bf3b1-324">Example: "EHforsyslog"</span></span>
<span data-ttu-id="bf3b1-325">facilityName</span><span class="sxs-lookup"><span data-stu-id="bf3b1-325">facilityName</span></span> | <span data-ttu-id="bf3b1-326">Název zařízení syslog (například "protokolu\_uživatele" nebo "protokolu\_LOCAL0").</span><span class="sxs-lookup"><span data-stu-id="bf3b1-326">A syslog facility name (such as "LOG\_USER" or "LOG\_LOCAL0").</span></span> <span data-ttu-id="bf3b1-327">Najdete v části "zařízení" [syslog man stránky](http://man7.org/linux/man-pages/man3/syslog.3.html) pro úplný seznam.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-327">See the "facility" section of the [syslog man page](http://man7.org/linux/man-pages/man3/syslog.3.html) for the full list.</span></span>
<span data-ttu-id="bf3b1-328">minSeverity</span><span class="sxs-lookup"><span data-stu-id="bf3b1-328">minSeverity</span></span> | <span data-ttu-id="bf3b1-329">Úroveň závažnosti syslog (například "protokolu\_chyba" nebo "protokolu\_informace").</span><span class="sxs-lookup"><span data-stu-id="bf3b1-329">A syslog severity level (such as "LOG\_ERR" or "LOG\_INFO").</span></span> <span data-ttu-id="bf3b1-330">Najdete v části "úroveň" [syslog man stránky](http://man7.org/linux/man-pages/man3/syslog.3.html) pro úplný seznam.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-330">See the "level" section of the [syslog man page](http://man7.org/linux/man-pages/man3/syslog.3.html) for the full list.</span></span> <span data-ttu-id="bf3b1-331">Přípona zachytí událostí odeslaných do zařízení nebo vyšší úrovni zadané.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-331">The extension captures events sent to the facility at or above the specified level.</span></span>

<span data-ttu-id="bf3b1-332">Pokud zadáte `syslogEvents`, LAD vždy zapisuje data do tabulky v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-332">When you specify `syslogEvents`, LAD always writes data to a table in Azure storage.</span></span> <span data-ttu-id="bf3b1-333">Může mít stejné dat zapsaných na objekty BLOB JSON nebo Event Hubs, ale nelze zakázat ukládání dat do tabulky.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-333">You can have the same data written to JSON blobs and/or Event Hubs, but you cannot disable storing data to a table.</span></span> <span data-ttu-id="bf3b1-334">Rozdělení chování pro tuto tabulku je stejný jako popsané pro `performanceCounters`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-334">The partitioning behavior for this table is the same as described for `performanceCounters`.</span></span> <span data-ttu-id="bf3b1-335">Název tabulky je zřetězení tyto řetězce:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-335">The table name is the concatenation of these strings:</span></span>

* `LinuxSyslog`
* <span data-ttu-id="bf3b1-336">Datum ve formátu "RRRRMMDD", který změní každých 10 dní</span><span class="sxs-lookup"><span data-stu-id="bf3b1-336">A date, in the form "YYYYMMDD", which changes every 10 days</span></span>

<span data-ttu-id="bf3b1-337">Mezi příklady patří `LinuxSyslog20170410` a `LinuxSyslog20170609`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-337">Examples include `LinuxSyslog20170410` and `LinuxSyslog20170609`.</span></span>

### <a name="perfcfg"></a><span data-ttu-id="bf3b1-338">perfCfg</span><span class="sxs-lookup"><span data-stu-id="bf3b1-338">perfCfg</span></span>

<span data-ttu-id="bf3b1-339">Spuštění libovolné ovládací prvky této volitelné části [OMI](https://github.com/Microsoft/omi) dotazy.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-339">This optional section controls execution of arbitrary [OMI](https://github.com/Microsoft/omi) queries.</span></span>

```json
"perfCfg": [
    {
        "namespace": "root/scx",
        "query": "SELECT PercentAvailableMemory, PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
        "table": "LinuxOldMemory",
        "frequency": 300,
        "sinks": ""
    }
]
```

<span data-ttu-id="bf3b1-340">Element</span><span class="sxs-lookup"><span data-stu-id="bf3b1-340">Element</span></span> | <span data-ttu-id="bf3b1-341">Hodnota</span><span class="sxs-lookup"><span data-stu-id="bf3b1-341">Value</span></span>
------- | -----
<span data-ttu-id="bf3b1-342">obor názvů</span><span class="sxs-lookup"><span data-stu-id="bf3b1-342">namespace</span></span> | <span data-ttu-id="bf3b1-343">(volitelné) OMI obor názvů, ve kterém se má provést dotaz.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-343">(optional) The OMI namespace within which the query should be executed.</span></span> <span data-ttu-id="bf3b1-344">Pokud tento parametr nezadáte, výchozí hodnota je "kořenový/scx", implementované [System Center a platformy zprostředkovatelé](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-344">If unspecified, the default value is "root/scx", implemented by the [System Center Cross-platform Providers](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).</span></span>
<span data-ttu-id="bf3b1-345">query</span><span class="sxs-lookup"><span data-stu-id="bf3b1-345">query</span></span> | <span data-ttu-id="bf3b1-346">OMI dotaz, který má být proveden.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-346">The OMI query to be executed.</span></span>
<span data-ttu-id="bf3b1-347">Tabulka</span><span class="sxs-lookup"><span data-stu-id="bf3b1-347">table</span></span> | <span data-ttu-id="bf3b1-348">(volitelné) Tabulky úložiště Azure, v účtu úložiště určený (viz [chráněné nastavení](#protected-settings)).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-348">(optional) The Azure storage table, in the designated storage account (see [Protected settings](#protected-settings)).</span></span>
<span data-ttu-id="bf3b1-349">frekvence</span><span class="sxs-lookup"><span data-stu-id="bf3b1-349">frequency</span></span> | <span data-ttu-id="bf3b1-350">(volitelné) Počet sekund mezi provádění dotazu.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-350">(optional) The number of seconds between execution of the query.</span></span> <span data-ttu-id="bf3b1-351">Výchozí hodnota je 300 (5 minut); minimální hodnota je 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-351">Default value is 300 (5 minutes); minimum value is 15 seconds.</span></span>
<span data-ttu-id="bf3b1-352">jímky</span><span class="sxs-lookup"><span data-stu-id="bf3b1-352">sinks</span></span> | <span data-ttu-id="bf3b1-353">(volitelné) Seznam názvy další jímky, na které je nutné ji publikovat metriky výsledky nezpracovaná ukázkový textový soubor s oddělovači.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-353">(optional) A comma-separated list of names of additional sinks to which raw sample metric results should be published.</span></span> <span data-ttu-id="bf3b1-354">Žádné agregace ukázek nezpracovaná se počítá rozšíření nebo metrik Azure.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-354">No aggregation of these raw samples is computed by the extension or by Azure Metrics.</span></span>

<span data-ttu-id="bf3b1-355">Musí být zadán buď "tabulka" nebo "jímky" nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-355">Either "table" or "sinks", or both, must be specified.</span></span>

### <a name="filelogs"></a><span data-ttu-id="bf3b1-356">fileLogs</span><span class="sxs-lookup"><span data-stu-id="bf3b1-356">fileLogs</span></span>

<span data-ttu-id="bf3b1-357">Řídí zaznamenávání souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-357">Controls the capture of log files.</span></span> <span data-ttu-id="bf3b1-358">LAD zaznamená nové řádky textu, jako jsou zapsané do souboru a zapíše na řádky tabulky a všechny zadané jímky (JsonBlob nebo EventHub).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-358">LAD captures new text lines as they are written to the file and writes them to table rows and/or any specified sinks (JsonBlob or EventHub).</span></span>

```json
"fileLogs": [
    {
        "file": "/var/log/mydaemonlog",
        "table": "MyDaemonEvents",
        "sinks": ""
    }
]
```

<span data-ttu-id="bf3b1-359">Element</span><span class="sxs-lookup"><span data-stu-id="bf3b1-359">Element</span></span> | <span data-ttu-id="bf3b1-360">Hodnota</span><span class="sxs-lookup"><span data-stu-id="bf3b1-360">Value</span></span>
------- | -----
<span data-ttu-id="bf3b1-361">Soubor</span><span class="sxs-lookup"><span data-stu-id="bf3b1-361">file</span></span> | <span data-ttu-id="bf3b1-362">Úplná cesta souboru protokolu bude sledovaná a zachycení.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-362">The full pathname of the log file to be watched and captured.</span></span> <span data-ttu-id="bf3b1-363">Název musí být názvu cesty jeden soubor; nemůže název adresáře nebo obsahovat zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-363">The pathname must name a single file; it cannot name a directory or contain wildcards.</span></span>
<span data-ttu-id="bf3b1-364">Tabulka</span><span class="sxs-lookup"><span data-stu-id="bf3b1-364">table</span></span> | <span data-ttu-id="bf3b1-365">(volitelné) Tabulka úložiště Azure v účtu úložiště určený (jako je zadaný v konfiguraci chráněného), do které se zapisují nové řádky "zadní" část souboru.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-365">(optional) The Azure storage table, in the designated storage account (as specified in the protected configuration), into which new lines from the "tail" of the file are written.</span></span>
<span data-ttu-id="bf3b1-366">jímky</span><span class="sxs-lookup"><span data-stu-id="bf3b1-366">sinks</span></span> | <span data-ttu-id="bf3b1-367">(volitelné) Seznam dalších jímky do protokolu linky, které odeslaných názvy oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-367">(optional) A comma-separated list of names of additional sinks to which log lines sent.</span></span>

<span data-ttu-id="bf3b1-368">Musí být zadán buď "tabulka" nebo "jímky" nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-368">Either "table" or "sinks", or both, must be specified.</span></span>

## <a name="metrics-supported-by-the-builtin-provider"></a><span data-ttu-id="bf3b1-369">Zprostředkovatel předdefinované metriky</span><span class="sxs-lookup"><span data-stu-id="bf3b1-369">Metrics supported by the builtin provider</span></span>

<span data-ttu-id="bf3b1-370">Zprostředkovatel metriky builtin je zdroj metriky nejvíce zajímavé pro širokou škálu uživatele.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-370">The builtin metric provider is a source of metrics most interesting to a broad set of users.</span></span> <span data-ttu-id="bf3b1-371">Tyto metriky spadají do pěti široký třídy:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-371">These metrics fall into five broad classes:</span></span>

* <span data-ttu-id="bf3b1-372">Procesor</span><span class="sxs-lookup"><span data-stu-id="bf3b1-372">Processor</span></span>
* <span data-ttu-id="bf3b1-373">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="bf3b1-373">Memory</span></span>
* <span data-ttu-id="bf3b1-374">Síť</span><span class="sxs-lookup"><span data-stu-id="bf3b1-374">Network</span></span>
* <span data-ttu-id="bf3b1-375">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="bf3b1-375">Filesystem</span></span>
* <span data-ttu-id="bf3b1-376">Disk</span><span class="sxs-lookup"><span data-stu-id="bf3b1-376">Disk</span></span>

### <a name="builtin-metrics-for-the-processor-class"></a><span data-ttu-id="bf3b1-377">předdefinované metriky pro procesor – třída</span><span class="sxs-lookup"><span data-stu-id="bf3b1-377">builtin metrics for the Processor class</span></span>

<span data-ttu-id="bf3b1-378">Třída procesoru metrik poskytuje informace o využití procesoru ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-378">The Processor class of metrics provides information about processor usage in the VM.</span></span> <span data-ttu-id="bf3b1-379">Při agregování procenta, výsledkem je průměr mezi všechny procesory.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-379">When aggregating percentages, the result is the average across all CPUs.</span></span> <span data-ttu-id="bf3b1-380">V dva základní virtuální počítač Pokud jednoho jádra bylo 100 % zaneprázdněn a dalších 100 % nečinnosti, bude hlášené PercentIdleTime 50.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-380">In a two core VM, if one core was 100% busy and the other was 100% idle, the reported PercentIdleTime would be 50.</span></span> <span data-ttu-id="bf3b1-381">Pokud každý jádra byla 50 % zaneprázdněn stejnou dobu, by také hlášené výsledek 50.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-381">If each core was 50% busy for the same period, the reported result would also be 50.</span></span> <span data-ttu-id="bf3b1-382">V čtyři základní virtuální počítač, pomocí jednoho jádra 100 % zaneprázdněn a ostatní nečinnosti bude hlášené PercentIdleTime 75.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-382">In a four core VM, with one core 100% busy and the others idle, the reported PercentIdleTime would be 75.</span></span>

<span data-ttu-id="bf3b1-383">Čítač</span><span class="sxs-lookup"><span data-stu-id="bf3b1-383">counter</span></span> | <span data-ttu-id="bf3b1-384">Význam</span><span class="sxs-lookup"><span data-stu-id="bf3b1-384">Meaning</span></span>
------- | -------
<span data-ttu-id="bf3b1-385">PercentIdleTime</span><span class="sxs-lookup"><span data-stu-id="bf3b1-385">PercentIdleTime</span></span> | <span data-ttu-id="bf3b1-386">Procento času během časového období agregace, procesory byly provádění jádra nečinné smyčky</span><span class="sxs-lookup"><span data-stu-id="bf3b1-386">Percentage of time during the aggregation window that processors were executing the kernel idle loop</span></span>
<span data-ttu-id="bf3b1-387">percentProcessorTime</span><span class="sxs-lookup"><span data-stu-id="bf3b1-387">PercentProcessorTime</span></span> | <span data-ttu-id="bf3b1-388">Procento doby provádění jiných než nečinných vláken</span><span class="sxs-lookup"><span data-stu-id="bf3b1-388">Percentage of time executing a non-idle thread</span></span>
<span data-ttu-id="bf3b1-389">PercentIOWaitTime</span><span class="sxs-lookup"><span data-stu-id="bf3b1-389">PercentIOWaitTime</span></span> | <span data-ttu-id="bf3b1-390">Procento doby čekání na dokončení operací vstupně-výstupní operace</span><span class="sxs-lookup"><span data-stu-id="bf3b1-390">Percentage of time waiting for IO operations to complete</span></span>
<span data-ttu-id="bf3b1-391">PercentInterruptTime</span><span class="sxs-lookup"><span data-stu-id="bf3b1-391">PercentInterruptTime</span></span> | <span data-ttu-id="bf3b1-392">Procento doby provádění hardware a software přerušení a DPC (odložených volání procedur)</span><span class="sxs-lookup"><span data-stu-id="bf3b1-392">Percentage of time executing hardware/software interrupts and DPCs (deferred procedure calls)</span></span>
<span data-ttu-id="bf3b1-393">PercentUserTime</span><span class="sxs-lookup"><span data-stu-id="bf3b1-393">PercentUserTime</span></span> | <span data-ttu-id="bf3b1-394">Jiných než nečinných dobu, během časového období agregace procento času stráveného v uživatele více se střední důležitostí</span><span class="sxs-lookup"><span data-stu-id="bf3b1-394">Of non-idle time during the aggregation window, the percentage of time spent in user more at normal priority</span></span>
<span data-ttu-id="bf3b1-395">PercentNiceTime</span><span class="sxs-lookup"><span data-stu-id="bf3b1-395">PercentNiceTime</span></span> | <span data-ttu-id="bf3b1-396">Jiných než nečinných času stráveného procento snížený (dobrý) důležitostí</span><span class="sxs-lookup"><span data-stu-id="bf3b1-396">Of non-idle time, the percentage spent at lowered (nice) priority</span></span>
<span data-ttu-id="bf3b1-397">PercentPrivilegedTime</span><span class="sxs-lookup"><span data-stu-id="bf3b1-397">PercentPrivilegedTime</span></span> | <span data-ttu-id="bf3b1-398">Jiných než nečinných času stráveného procento v privilegovaném režimu režimu (jádra)</span><span class="sxs-lookup"><span data-stu-id="bf3b1-398">Of non-idle time, the percentage spent in privileged (kernel) mode</span></span>

<span data-ttu-id="bf3b1-399">První čtyři čítače by měl součet, kterou se 100 %.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-399">The first four counters should sum to 100%.</span></span> <span data-ttu-id="bf3b1-400">Poslední tři čítače také součet na 100 %; jejich rozdělit součet PercentProcessorTime, PercentIOWaitTime a PercentInterruptTime.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-400">The last three counters also sum to 100%; they subdivide the sum of PercentProcessorTime, PercentIOWaitTime, and PercentInterruptTime.</span></span>

<span data-ttu-id="bf3b1-401">Chcete-li získat jeden metrika agregovat do všech procesorů, nastavte `"condition": "IsAggregate=TRUE"`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-401">To obtain a single metric aggregated across all processors, set `"condition": "IsAggregate=TRUE"`.</span></span> <span data-ttu-id="bf3b1-402">Chcete-li získat metriky pro specifické procesory, jako je druhý logický procesor čtyři základní virtuální počítač, nastavte `"condition": "Name=\\"1\\""`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-402">To obtain a metric for a specific processor, such as the second logical processor of a four core VM, set `"condition": "Name=\\"1\\""`.</span></span> <span data-ttu-id="bf3b1-403">Logický procesor čísla jsou v rozsahu `[0..n-1]`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-403">Logical processor numbers are in the range `[0..n-1]`.</span></span>

### <a name="builtin-metrics-for-the-memory-class"></a><span data-ttu-id="bf3b1-404">předdefinované metriky pro třídu paměti</span><span class="sxs-lookup"><span data-stu-id="bf3b1-404">builtin metrics for the Memory class</span></span>

<span data-ttu-id="bf3b1-405">Třída paměti metrik poskytuje informace o využití paměti, stránkování a vzájemná záměna.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-405">The Memory class of metrics provides information about memory utilization, paging, and swapping.</span></span>

<span data-ttu-id="bf3b1-406">Čítač</span><span class="sxs-lookup"><span data-stu-id="bf3b1-406">counter</span></span> | <span data-ttu-id="bf3b1-407">Význam</span><span class="sxs-lookup"><span data-stu-id="bf3b1-407">Meaning</span></span>
------- | -------
<span data-ttu-id="bf3b1-408">AvailableMemory</span><span class="sxs-lookup"><span data-stu-id="bf3b1-408">AvailableMemory</span></span> | <span data-ttu-id="bf3b1-409">Dostupná fyzická paměť v MiB</span><span class="sxs-lookup"><span data-stu-id="bf3b1-409">Available physical memory in MiB</span></span>
<span data-ttu-id="bf3b1-410">PercentAvailableMemory</span><span class="sxs-lookup"><span data-stu-id="bf3b1-410">PercentAvailableMemory</span></span> | <span data-ttu-id="bf3b1-411">Dostupná fyzická paměť v procentech celkové paměti</span><span class="sxs-lookup"><span data-stu-id="bf3b1-411">Available physical memory as a percent of total memory</span></span>
<span data-ttu-id="bf3b1-412">UsedMemory</span><span class="sxs-lookup"><span data-stu-id="bf3b1-412">UsedMemory</span></span> | <span data-ttu-id="bf3b1-413">Použití fyzické paměti (MiB)</span><span class="sxs-lookup"><span data-stu-id="bf3b1-413">In-use physical memory (MiB)</span></span>
<span data-ttu-id="bf3b1-414">PercentUsedMemory</span><span class="sxs-lookup"><span data-stu-id="bf3b1-414">PercentUsedMemory</span></span> | <span data-ttu-id="bf3b1-415">Fyzické paměti používané jako procento celkové paměti</span><span class="sxs-lookup"><span data-stu-id="bf3b1-415">In-use physical memory as a percent of total memory</span></span>
<span data-ttu-id="bf3b1-416">PagesPerSec</span><span class="sxs-lookup"><span data-stu-id="bf3b1-416">PagesPerSec</span></span> | <span data-ttu-id="bf3b1-417">Celkový počet stránkování (čtení a zápis)</span><span class="sxs-lookup"><span data-stu-id="bf3b1-417">Total paging (read/write)</span></span>
<span data-ttu-id="bf3b1-418">PagesReadPerSec</span><span class="sxs-lookup"><span data-stu-id="bf3b1-418">PagesReadPerSec</span></span> | <span data-ttu-id="bf3b1-419">Čtení stránek ze záložního úložiště (odkládacího souboru, soubor programu, mapovaný soubor, atd.)</span><span class="sxs-lookup"><span data-stu-id="bf3b1-419">Pages read from backing store (swap file, program file, mapped file, etc.)</span></span>
<span data-ttu-id="bf3b1-420">PagesWrittenPerSec</span><span class="sxs-lookup"><span data-stu-id="bf3b1-420">PagesWrittenPerSec</span></span> | <span data-ttu-id="bf3b1-421">Stránek zapsaných k záložnímu úložišti (odkládací soubor, mapovaný soubor, atd.)</span><span class="sxs-lookup"><span data-stu-id="bf3b1-421">Pages written to backing store (swap file, mapped file, etc.)</span></span>
<span data-ttu-id="bf3b1-422">AvailableSwap</span><span class="sxs-lookup"><span data-stu-id="bf3b1-422">AvailableSwap</span></span> | <span data-ttu-id="bf3b1-423">Nepoužívané odkládacího prostoru (MiB)</span><span class="sxs-lookup"><span data-stu-id="bf3b1-423">Unused swap space (MiB)</span></span>
<span data-ttu-id="bf3b1-424">PercentAvailableSwap</span><span class="sxs-lookup"><span data-stu-id="bf3b1-424">PercentAvailableSwap</span></span> | <span data-ttu-id="bf3b1-425">Nepoužívané odkládacího souboru jako procentuální hodnotu celkové velikosti odkládacího souboru</span><span class="sxs-lookup"><span data-stu-id="bf3b1-425">Unused swap space as a percentage of total swap</span></span>
<span data-ttu-id="bf3b1-426">UsedSwap</span><span class="sxs-lookup"><span data-stu-id="bf3b1-426">UsedSwap</span></span> | <span data-ttu-id="bf3b1-427">Využití odkládacího souboru (MiB)</span><span class="sxs-lookup"><span data-stu-id="bf3b1-427">In-use swap space (MiB)</span></span>
<span data-ttu-id="bf3b1-428">PercentUsedSwap</span><span class="sxs-lookup"><span data-stu-id="bf3b1-428">PercentUsedSwap</span></span> | <span data-ttu-id="bf3b1-429">Používané místo odkládacího souboru jako procentuální hodnotu celkové velikosti odkládacího souboru</span><span class="sxs-lookup"><span data-stu-id="bf3b1-429">In-use swap space as a percentage of total swap</span></span>

<span data-ttu-id="bf3b1-430">Tato třída metrik obsahuje pouze jednu instanci.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-430">This class of metrics has only a single instance.</span></span> <span data-ttu-id="bf3b1-431">Atribut "podmínky" neexistuje užitečné nastavení a by měl být vynechán.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-431">The "condition" attribute has no useful settings and should be omitted.</span></span>

### <a name="builtin-metrics-for-the-network-class"></a><span data-ttu-id="bf3b1-432">předdefinované metriky pro sítě – třída</span><span class="sxs-lookup"><span data-stu-id="bf3b1-432">builtin metrics for the Network class</span></span>

<span data-ttu-id="bf3b1-433">Třída sítě metrik poskytuje informace o síťové aktivity na rozhraní jednotlivým síťovým od spuštění počítače.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-433">The Network class of metrics provides information about network activity on an individual network interfaces since boot.</span></span> <span data-ttu-id="bf3b1-434">LAD nevystavuje metriky šířky pásma, které je možné načíst z hostitele metriky.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-434">LAD does not expose bandwidth metrics, which can be retrieved from host metrics.</span></span>

<span data-ttu-id="bf3b1-435">Čítač</span><span class="sxs-lookup"><span data-stu-id="bf3b1-435">counter</span></span> | <span data-ttu-id="bf3b1-436">Význam</span><span class="sxs-lookup"><span data-stu-id="bf3b1-436">Meaning</span></span>
------- | -------
<span data-ttu-id="bf3b1-437">BytesTransmitted</span><span class="sxs-lookup"><span data-stu-id="bf3b1-437">BytesTransmitted</span></span> | <span data-ttu-id="bf3b1-438">Celkový počet bajtů odeslaných od spuštění</span><span class="sxs-lookup"><span data-stu-id="bf3b1-438">Total bytes sent since boot</span></span>
<span data-ttu-id="bf3b1-439">BytesReceived</span><span class="sxs-lookup"><span data-stu-id="bf3b1-439">BytesReceived</span></span> | <span data-ttu-id="bf3b1-440">Celkový počet bajtů přijatých od spuštění</span><span class="sxs-lookup"><span data-stu-id="bf3b1-440">Total bytes received since boot</span></span>
<span data-ttu-id="bf3b1-441">BytesTotal</span><span class="sxs-lookup"><span data-stu-id="bf3b1-441">BytesTotal</span></span> | <span data-ttu-id="bf3b1-442">Celkový počet bajtů odesílané nebo přijímané od spuštění počítače</span><span class="sxs-lookup"><span data-stu-id="bf3b1-442">Total bytes sent or received since boot</span></span>
<span data-ttu-id="bf3b1-443">PacketsTransmitted</span><span class="sxs-lookup"><span data-stu-id="bf3b1-443">PacketsTransmitted</span></span> | <span data-ttu-id="bf3b1-444">Celkový počet paketů odeslaných od spuštění</span><span class="sxs-lookup"><span data-stu-id="bf3b1-444">Total packets sent since boot</span></span>
<span data-ttu-id="bf3b1-445">PacketsReceived</span><span class="sxs-lookup"><span data-stu-id="bf3b1-445">PacketsReceived</span></span> | <span data-ttu-id="bf3b1-446">Celkový počet paketů přijatých od spuštění</span><span class="sxs-lookup"><span data-stu-id="bf3b1-446">Total packets received since boot</span></span>
<span data-ttu-id="bf3b1-447">TotalRxErrors</span><span class="sxs-lookup"><span data-stu-id="bf3b1-447">TotalRxErrors</span></span> | <span data-ttu-id="bf3b1-448">Počet chyb, obdrží od spuštění počítače</span><span class="sxs-lookup"><span data-stu-id="bf3b1-448">Number of receive errors since boot</span></span>
<span data-ttu-id="bf3b1-449">TotalTxErrors</span><span class="sxs-lookup"><span data-stu-id="bf3b1-449">TotalTxErrors</span></span> | <span data-ttu-id="bf3b1-450">Počet chyb přenosu od spuštění počítače</span><span class="sxs-lookup"><span data-stu-id="bf3b1-450">Number of transmit errors since boot</span></span>
<span data-ttu-id="bf3b1-451">TotalCollisions</span><span class="sxs-lookup"><span data-stu-id="bf3b1-451">TotalCollisions</span></span> | <span data-ttu-id="bf3b1-452">Počet kolizí hlášené síťové porty od spuštění počítače</span><span class="sxs-lookup"><span data-stu-id="bf3b1-452">Number of collisions reported by the network ports since boot</span></span>

 <span data-ttu-id="bf3b1-453">I když tato třída je instance, nepodporuje LAD zaznamenávání metriky sítě s agregovat přes všechna síťová zařízení.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-453">Although this class is instanced, LAD does not support capturing Network metrics aggregated across all network devices.</span></span> <span data-ttu-id="bf3b1-454">Chcete-li získat metriky pro určité rozhraní, jako je například eth0, nastavte `"condition": "InstanceID=\\"eth0\\""`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-454">To obtain the metrics for a specific interface, such as eth0, set `"condition": "InstanceID=\\"eth0\\""`.</span></span>

### <a name="builtin-metrics-for-the-filesystem-class"></a><span data-ttu-id="bf3b1-455">předdefinované metriky pro třídy systému souborů</span><span class="sxs-lookup"><span data-stu-id="bf3b1-455">builtin metrics for the Filesystem class</span></span>

<span data-ttu-id="bf3b1-456">Třída Filesystem metrik poskytuje informace o použití systému souborů.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-456">The Filesystem class of metrics provides information about filesystem usage.</span></span> <span data-ttu-id="bf3b1-457">Absolutní a procentuální hodnoty jsou hlášeny, jako by se zobrazit běžného uživatele (není uživatel root).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-457">Absolute and percentage values are reported as they'd be displayed to an ordinary user (not root).</span></span>

<span data-ttu-id="bf3b1-458">Čítač</span><span class="sxs-lookup"><span data-stu-id="bf3b1-458">counter</span></span> | <span data-ttu-id="bf3b1-459">Význam</span><span class="sxs-lookup"><span data-stu-id="bf3b1-459">Meaning</span></span>
------- | -------
<span data-ttu-id="bf3b1-460">FreeSpace</span><span class="sxs-lookup"><span data-stu-id="bf3b1-460">FreeSpace</span></span> | <span data-ttu-id="bf3b1-461">Dostupné místo na disku v bajtech</span><span class="sxs-lookup"><span data-stu-id="bf3b1-461">Available disk space in bytes</span></span>
<span data-ttu-id="bf3b1-462">UsedSpace</span><span class="sxs-lookup"><span data-stu-id="bf3b1-462">UsedSpace</span></span> | <span data-ttu-id="bf3b1-463">Využité místo na disku v bajtech</span><span class="sxs-lookup"><span data-stu-id="bf3b1-463">Used disk space in bytes</span></span>
<span data-ttu-id="bf3b1-464">PercentFreeSpace</span><span class="sxs-lookup"><span data-stu-id="bf3b1-464">PercentFreeSpace</span></span> | <span data-ttu-id="bf3b1-465">Procento volného místa</span><span class="sxs-lookup"><span data-stu-id="bf3b1-465">Percentage free space</span></span>
<span data-ttu-id="bf3b1-466">PercentUsedSpace</span><span class="sxs-lookup"><span data-stu-id="bf3b1-466">PercentUsedSpace</span></span> | <span data-ttu-id="bf3b1-467">Procento využitého prostoru</span><span class="sxs-lookup"><span data-stu-id="bf3b1-467">Percentage used space</span></span>
<span data-ttu-id="bf3b1-468">PercentFreeInodes</span><span class="sxs-lookup"><span data-stu-id="bf3b1-468">PercentFreeInodes</span></span> | <span data-ttu-id="bf3b1-469">Procentuální hodnota nepoužívané uzlů Inode</span><span class="sxs-lookup"><span data-stu-id="bf3b1-469">Percentage of unused inodes</span></span>
<span data-ttu-id="bf3b1-470">PercentUsedInodes</span><span class="sxs-lookup"><span data-stu-id="bf3b1-470">PercentUsedInodes</span></span> | <span data-ttu-id="bf3b1-471">Procentuální hodnota přidělené (používán) uzlů Inode sčítají mezi všechny systémy</span><span class="sxs-lookup"><span data-stu-id="bf3b1-471">Percentage of allocated (in use) inodes summed across all filesystems</span></span>
<span data-ttu-id="bf3b1-472">BytesReadPerSecond</span><span class="sxs-lookup"><span data-stu-id="bf3b1-472">BytesReadPerSecond</span></span> | <span data-ttu-id="bf3b1-473">Načíst počet bajtů za sekundu</span><span class="sxs-lookup"><span data-stu-id="bf3b1-473">Bytes read per second</span></span>
<span data-ttu-id="bf3b1-474">BytesWrittenPerSecond</span><span class="sxs-lookup"><span data-stu-id="bf3b1-474">BytesWrittenPerSecond</span></span> | <span data-ttu-id="bf3b1-475">Bajtů zapsaných za sekundu</span><span class="sxs-lookup"><span data-stu-id="bf3b1-475">Bytes written per second</span></span>
<span data-ttu-id="bf3b1-476">BytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="bf3b1-476">BytesPerSecond</span></span> | <span data-ttu-id="bf3b1-477">Bajtů číst nebo zapisovat za sekundu</span><span class="sxs-lookup"><span data-stu-id="bf3b1-477">Bytes read or written per second</span></span>
<span data-ttu-id="bf3b1-478">ReadsPerSecond</span><span class="sxs-lookup"><span data-stu-id="bf3b1-478">ReadsPerSecond</span></span> | <span data-ttu-id="bf3b1-479">Operace čtení za sekundu</span><span class="sxs-lookup"><span data-stu-id="bf3b1-479">Read operations per second</span></span>
<span data-ttu-id="bf3b1-480">WritesPerSecond</span><span class="sxs-lookup"><span data-stu-id="bf3b1-480">WritesPerSecond</span></span> | <span data-ttu-id="bf3b1-481">Zápis operací za sekundu</span><span class="sxs-lookup"><span data-stu-id="bf3b1-481">Write operations per second</span></span>
<span data-ttu-id="bf3b1-482">TransfersPerSecond</span><span class="sxs-lookup"><span data-stu-id="bf3b1-482">TransfersPerSecond</span></span> | <span data-ttu-id="bf3b1-483">Operace čtení nebo zápisu za sekundu</span><span class="sxs-lookup"><span data-stu-id="bf3b1-483">Read or write operations per second</span></span>

<span data-ttu-id="bf3b1-484">Agregované hodnoty mezi všechny systémy souborů je možné získat nastavení `"condition": "IsAggregate=True"`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-484">Aggregated values across all file systems can be obtained by setting `"condition": "IsAggregate=True"`.</span></span> <span data-ttu-id="bf3b1-485">Hodnoty pro konkrétní připojeného souboru systém, například "/ mnt", je možné získat nastavení `"condition": 'Name="/mnt"'`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-485">Values for a specific mounted file system, such as "/mnt", can be obtained by setting `"condition": 'Name="/mnt"'`.</span></span>

### <a name="builtin-metrics-for-the-disk-class"></a><span data-ttu-id="bf3b1-486">předdefinované metriky pro třídu disku</span><span class="sxs-lookup"><span data-stu-id="bf3b1-486">builtin metrics for the Disk class</span></span>

<span data-ttu-id="bf3b1-487">Třída disku metrik poskytuje informace o využití disku zařízení.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-487">The Disk class of metrics provides information about disk device usage.</span></span> <span data-ttu-id="bf3b1-488">Tyto statistické údaje se vztahují na celou jednotku.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-488">These statistics apply to the entire drive.</span></span> <span data-ttu-id="bf3b1-489">Pokud existují více systémy souborů na zařízení, čítačů pro toto zařízení se prakticky agregovat přes všechny z nich.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-489">If there are multiple file systems on a device, the counters for that device are, effectively, aggregated across all of them.</span></span>

<span data-ttu-id="bf3b1-490">Čítač</span><span class="sxs-lookup"><span data-stu-id="bf3b1-490">counter</span></span> | <span data-ttu-id="bf3b1-491">Význam</span><span class="sxs-lookup"><span data-stu-id="bf3b1-491">Meaning</span></span>
------- | -------
<span data-ttu-id="bf3b1-492">ReadsPerSecond</span><span class="sxs-lookup"><span data-stu-id="bf3b1-492">ReadsPerSecond</span></span> | <span data-ttu-id="bf3b1-493">Operace čtení za sekundu</span><span class="sxs-lookup"><span data-stu-id="bf3b1-493">Read operations per second</span></span>
<span data-ttu-id="bf3b1-494">WritesPerSecond</span><span class="sxs-lookup"><span data-stu-id="bf3b1-494">WritesPerSecond</span></span> | <span data-ttu-id="bf3b1-495">Zápis operací za sekundu</span><span class="sxs-lookup"><span data-stu-id="bf3b1-495">Write operations per second</span></span>
<span data-ttu-id="bf3b1-496">TransfersPerSecond</span><span class="sxs-lookup"><span data-stu-id="bf3b1-496">TransfersPerSecond</span></span> | <span data-ttu-id="bf3b1-497">Celkový počet operací za sekundu</span><span class="sxs-lookup"><span data-stu-id="bf3b1-497">Total operations per second</span></span>
<span data-ttu-id="bf3b1-498">AverageReadTime</span><span class="sxs-lookup"><span data-stu-id="bf3b1-498">AverageReadTime</span></span> | <span data-ttu-id="bf3b1-499">Průměrný počet sekund na operace čtení</span><span class="sxs-lookup"><span data-stu-id="bf3b1-499">Average seconds per read operation</span></span>
<span data-ttu-id="bf3b1-500">AverageWriteTime</span><span class="sxs-lookup"><span data-stu-id="bf3b1-500">AverageWriteTime</span></span> | <span data-ttu-id="bf3b1-501">Průměrný počet sekund na operace zápisu</span><span class="sxs-lookup"><span data-stu-id="bf3b1-501">Average seconds per write operation</span></span>
<span data-ttu-id="bf3b1-502">AverageTransferTime</span><span class="sxs-lookup"><span data-stu-id="bf3b1-502">AverageTransferTime</span></span> | <span data-ttu-id="bf3b1-503">Průměrný počet sekund na operaci</span><span class="sxs-lookup"><span data-stu-id="bf3b1-503">Average seconds per operation</span></span>
<span data-ttu-id="bf3b1-504">AverageDiskQueueLength</span><span class="sxs-lookup"><span data-stu-id="bf3b1-504">AverageDiskQueueLength</span></span> | <span data-ttu-id="bf3b1-505">Průměrný počet operací zařazených do fronty disku</span><span class="sxs-lookup"><span data-stu-id="bf3b1-505">Average number of queued disk operations</span></span>
<span data-ttu-id="bf3b1-506">ReadBytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="bf3b1-506">ReadBytesPerSecond</span></span> | <span data-ttu-id="bf3b1-507">Počet bajtů přečtených za sekundu</span><span class="sxs-lookup"><span data-stu-id="bf3b1-507">Number of bytes read per second</span></span>
<span data-ttu-id="bf3b1-508">WriteBytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="bf3b1-508">WriteBytesPerSecond</span></span> | <span data-ttu-id="bf3b1-509">Počet bajtů zapsaných za sekundu</span><span class="sxs-lookup"><span data-stu-id="bf3b1-509">Number of bytes written per second</span></span>
<span data-ttu-id="bf3b1-510">BytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="bf3b1-510">BytesPerSecond</span></span> | <span data-ttu-id="bf3b1-511">Počet bajtů přečtených nebo zapsaných za sekundu</span><span class="sxs-lookup"><span data-stu-id="bf3b1-511">Number of bytes read or written per second</span></span>

<span data-ttu-id="bf3b1-512">Agregované hodnoty všech disků je možné získat nastavení `"condition": "IsAggregate=True"`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-512">Aggregated values across all disks can be obtained by setting `"condition": "IsAggregate=True"`.</span></span> <span data-ttu-id="bf3b1-513">Chcete-li získat informace pro konkrétní zařízení (například/dev/sdf1), nastavte `"condition": "Name=\\"/dev/sdf1\\""`.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-513">To get information for a specific device (for example, /dev/sdf1), set `"condition": "Name=\\"/dev/sdf1\\""`.</span></span>

## <a name="installing-and-configuring-lad-30-via-cli"></a><span data-ttu-id="bf3b1-514">Instalace a konfigurace LAD 3.0 prostřednictvím rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="bf3b1-514">Installing and configuring LAD 3.0 via CLI</span></span>

<span data-ttu-id="bf3b1-515">Za předpokladu, že jsou chráněný nastavení v souboru PrivateConfig.json a veřejné konfigurační informace v PublicConfig.json, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-515">Assuming your protected settings are in the file PrivateConfig.json and your public configuration information is in PublicConfig.json, run this command:</span></span>

```azurecli
az vm extension set *resource_group_name* *vm_name* LinuxDiagnostic Microsoft.Azure.Diagnostics '3.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json
```

<span data-ttu-id="bf3b1-516">Příkaz předpokládá, že používáte správu prostředků Azure režim rozhraní příkazového řádku Azure (arm).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-516">The command assumes you are using the Azure Resource Management mode (arm) of the Azure CLI.</span></span> <span data-ttu-id="bf3b1-517">Konfigurace pro nasazení classic LAD modelu virtuálních počítačů (ASM), přepněte do režimu "asm" (`azure config mode asm`) a název skupiny prostředků v příkazu vynechat.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-517">To configure LAD for classic deployment model (ASM) VMs, switch to "asm" mode (`azure config mode asm`) and omit the resource group name in the command.</span></span> <span data-ttu-id="bf3b1-518">Další informace najdete v tématu [dokumentaci rozhraní příkazového řádku a platformy](https://docs.microsoft.com/azure/xplat-cli-connect).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-518">For more information, see the [cross-platform CLI documentation](https://docs.microsoft.com/azure/xplat-cli-connect).</span></span>

## <a name="an-example-lad-30-configuration"></a><span data-ttu-id="bf3b1-519">Příklad LAD 3.0 konfigurace</span><span class="sxs-lookup"><span data-stu-id="bf3b1-519">An example LAD 3.0 configuration</span></span>

<span data-ttu-id="bf3b1-520">Podle definice předchozí, zde je ukázka konfigurace rozšíření LAD 3.0 s vysvětlení.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-520">Based on the preceding definitions, here's a sample LAD 3.0 extension configuration with some explanation.</span></span> <span data-ttu-id="bf3b1-521">Tuto ukázku použít pro váš případ, měli byste použít vlastní název účtu úložiště, token SAS účtu a tokeny EventHubs SAS.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-521">To apply this sample to your case, you should use your own storage account name, account SAS token, and EventHubs SAS tokens.</span></span>

### <a name="privateconfigjson"></a><span data-ttu-id="bf3b1-522">PrivateConfig.json</span><span class="sxs-lookup"><span data-stu-id="bf3b1-522">PrivateConfig.json</span></span>

<span data-ttu-id="bf3b1-523">Tato nastavení privátní konfigurace:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-523">These private settings configure:</span></span>

* <span data-ttu-id="bf3b1-524">účet úložiště</span><span class="sxs-lookup"><span data-stu-id="bf3b1-524">a storage account</span></span>
* <span data-ttu-id="bf3b1-525">odpovídající tokenu SAS účtu</span><span class="sxs-lookup"><span data-stu-id="bf3b1-525">a matching account SAS token</span></span>
* <span data-ttu-id="bf3b1-526">několik jímky (JsonBlob nebo EventHubs se tokeny SAS)</span><span class="sxs-lookup"><span data-stu-id="bf3b1-526">several sinks (JsonBlob or EventHubs with SAS tokens)</span></span>

```json
{
  "storageAccountName": "yourdiagstgacct",
  "storageAccountSasToken": "sv=xxxx-xx-xx&ss=bt&srt=co&sp=wlacu&st=yyyy-yy-yyT21%3A22%3A00Z&se=zzzz-zz-zzT21%3A22%3A00Z&sig=fake_signature",
  "sinksConfig": {
    "sink": [
      {
        "name": "SyslogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "FilelogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "MyJsonMetricsBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=fake_signature&se=1808096361&skn=yourehpolicy"
      },
      {
        "name": "MyMetricEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&skn=yourehpolicy"
      },
      {
        "name": "LoggingEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&se=1808096361&skn=yourehpolicy"
      }
    ]
  }
}
```

### <a name="publicconfigjson"></a><span data-ttu-id="bf3b1-527">PublicConfig.json</span><span class="sxs-lookup"><span data-stu-id="bf3b1-527">PublicConfig.json</span></span>

<span data-ttu-id="bf3b1-528">Tato nastavení veřejných způsobit LAD na:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-528">These public settings cause LAD to:</span></span>

* <span data-ttu-id="bf3b1-529">Nahrát procentuální hodnoty času procesoru a použitého místa disku metrik `WADMetrics*` tabulky</span><span class="sxs-lookup"><span data-stu-id="bf3b1-529">Upload percent-processor-time and used-disk-space metrics to the `WADMetrics*` table</span></span>
* <span data-ttu-id="bf3b1-530">Odeslání zprávy z syslog budovy "user" a závažnost "informace" k `LinuxSyslog*` tabulky</span><span class="sxs-lookup"><span data-stu-id="bf3b1-530">Upload messages from syslog facility "user" and severity "info" to the `LinuxSyslog*` table</span></span>
* <span data-ttu-id="bf3b1-531">Nahrát nezpracovaná OMI výsledků dotazu (PercentProcessorTime a PercentIdleTime) na zadaný `LinuxCPU` tabulky</span><span class="sxs-lookup"><span data-stu-id="bf3b1-531">Upload raw OMI query results (PercentProcessorTime and PercentIdleTime) to the named `LinuxCPU` table</span></span>
* <span data-ttu-id="bf3b1-532">Nahrát připojením řádků v souboru `/var/log/myladtestlog` k `MyLadTestLog` tabulky</span><span class="sxs-lookup"><span data-stu-id="bf3b1-532">Upload appended lines in file `/var/log/myladtestlog` to the `MyLadTestLog` table</span></span>

<span data-ttu-id="bf3b1-533">Data jsou v každém případě také odeslána a:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-533">In each case, data is also uploaded to:</span></span>

* <span data-ttu-id="bf3b1-534">Azure Blob storage (název kontejneru je definovaný v jímce JsonBlob)</span><span class="sxs-lookup"><span data-stu-id="bf3b1-534">Azure Blob storage (container name is as defined in the JsonBlob sink)</span></span>
* <span data-ttu-id="bf3b1-535">Koncový bod EventHubs (jak je uvedeno v jímce EventHubs)</span><span class="sxs-lookup"><span data-stu-id="bf3b1-535">EventHubs endpoint (as specified in the EventHubs sink)</span></span>

```json
{
  "StorageAccount": "yourdiagstgacct",
  "sampleRateInSeconds": 15,
  "ladCfg": {
    "diagnosticMonitorConfiguration": {
      "performanceCounters": {
        "sinks": "MyMetricEventHub,MyJsonMetricsBlob",
        "performanceCounterConfiguration": [
          {
            "unit": "Percent",
            "type": "builtin",
            "counter": "PercentProcessorTime",
            "counterSpecifier": "/builtin/Processor/PercentProcessorTime",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Aggregate CPU %utilization"
              }
            ],
            "condition": "IsAggregate=TRUE",
            "class": "Processor"
          },
          {
            "unit": "Bytes",
            "type": "builtin",
            "counter": "UsedSpace",
            "counterSpecifier": "/builtin/FileSystem/UsedSpace",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Used disk space on /"
              }
            ],
            "condition": "Name=\"/\"",
            "class": "Filesystem"
          }
        ]
      },
      "metrics": {
        "metricAggregation": [
          {
            "scheduledTransferPeriod": "PT1H"
          },
          {
            "scheduledTransferPeriod": "PT1M"
          }
        ],
        "resourceId": "/subscriptions/your_azure_subscription_id/resourceGroups/your_resource_group_name/providers/Microsoft.Compute/virtualMachines/your_vm_name"
      },
      "eventVolume": "Large",
      "syslogEvents": {
        "sinks": "SyslogJsonBlob,LoggingEventHub",
        "syslogEventConfiguration": {
          "LOG_USER": "LOG_INFO"
        }
      }
    }
  },
  "perfCfg": [
    {
      "query": "SELECT PercentProcessorTime, PercentIdleTime FROM SCX_ProcessorStatisticalInformation WHERE Name='_TOTAL'",
      "table": "LinuxCpu",
      "frequency": 60,
      "sinks": "LinuxCpuJsonBlob,LinuxCpuEventHub"
    }
  ],
  "fileLogs": [
    {
      "file": "/var/log/myladtestlog",
      "table": "MyLadTestLog",
      "sinks": "FilelogJsonBlob,LoggingEventHub"
    }
  ]
}
```

<span data-ttu-id="bf3b1-536">`resourceId` v konfiguraci se musí shodovat, virtuální počítač nebo virtuální počítač měřítka nastavit.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-536">The `resourceId` in the configuration must match that of the VM or the virtual machine scale set.</span></span>

* <span data-ttu-id="bf3b1-537">Metriky platformy Azure grafů a výstrah zná resourceId pracujete virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-537">Azure platform metrics charting and alerting knows the resourceId of the VM you're working on.</span></span> <span data-ttu-id="bf3b1-538">Se očekává, že chcete najít data pro virtuální počítač pomocí resourceId klíč vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-538">It expects to find the data for your VM using the resourceId the lookup key.</span></span>
* <span data-ttu-id="bf3b1-539">Pokud používáte Azure škálování, resourceId v konfiguraci automatického škálování se musí shodovat resourceId používané LAD.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-539">If you use Azure autoscale, the resourceId in the autoscale configuration must match the resourceId used by LAD.</span></span>
* <span data-ttu-id="bf3b1-540">ResourceId je integrovaná do názvy JsonBlobs zapsaných správcem LAD.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-540">The resourceId is built into the names of JsonBlobs written by LAD.</span></span>

## <a name="view-your-data"></a><span data-ttu-id="bf3b1-541">Zobrazení dat</span><span class="sxs-lookup"><span data-stu-id="bf3b1-541">View your data</span></span>

<span data-ttu-id="bf3b1-542">Pomocí portálu Azure do zobrazení dat výkonu nebo nastavit výstrahy:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-542">Use the Azure portal to view performance data or set alerts:</span></span>

![Bitové kopie](./media/diagnostic-extension/graph_metrics.png)

<span data-ttu-id="bf3b1-544">`performanceCounters` Dat byly vždy uloženy v tabulce Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-544">The `performanceCounters` data are always stored in an Azure Storage table.</span></span> <span data-ttu-id="bf3b1-545">Rozhraní API Azure úložiště jsou k dispozici pro mnoho jazyky a platformy.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-545">Azure Storage APIs are available for many languages and platforms.</span></span>

<span data-ttu-id="bf3b1-546">Data odeslaná jímky JsonBlob se ukládají do objektů BLOB v účtu úložiště s názvem v [chráněné nastavení](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-546">Data sent to JsonBlob sinks is stored in blobs in the storage account named in the [Protected settings](#protected-settings).</span></span> <span data-ttu-id="bf3b1-547">Můžete využívat data objektů blob s využitím jakékoli rozhraní API úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-547">You can consume the blob data using any Azure Blob Storage APIs.</span></span>

<span data-ttu-id="bf3b1-548">Kromě toho můžete tyto nástroje uživatelského rozhraní pro přístup k datům ve službě Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="bf3b1-548">In addition, you can use these UI tools to access the data in Azure Storage:</span></span>

* <span data-ttu-id="bf3b1-549">Průzkumníka serveru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-549">Visual Studio Server Explorer.</span></span>
* <span data-ttu-id="bf3b1-550">[Microsoft Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").</span><span class="sxs-lookup"><span data-stu-id="bf3b1-550">[Microsoft Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").</span></span>

<span data-ttu-id="bf3b1-551">Tento snímek relaci Microsoft Azure Storage Explorer zobrazuje generovaného tabulky služby Azure Storage a kontejnery z správně nakonfigurována rozšíření LAD 3.0 na testovací virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-551">This snapshot of a Microsoft Azure Storage Explorer session shows the generated Azure Storage tables and containers from a correctly configured LAD 3.0 extension on a test VM.</span></span> <span data-ttu-id="bf3b1-552">Obrázek neodpovídá přesně [Ukázková konfigurace LAD 3.0](#an-example-lad-30-configuration).</span><span class="sxs-lookup"><span data-stu-id="bf3b1-552">The image doesn't match exactly with the [sample LAD 3.0 configuration](#an-example-lad-30-configuration).</span></span>

![Bitové kopie](./media/diagnostic-extension/stg_explorer.png)

<span data-ttu-id="bf3b1-554">V tématu odpovídajícího [EventHubs dokumentace](../../event-hubs/event-hubs-what-is-event-hubs.md) se dozvíte, jak využívat zpráv publikovaných do EventHubs koncový bod.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-554">See the relevant [EventHubs documentation](../../event-hubs/event-hubs-what-is-event-hubs.md) to learn how to consume messages published to an EventHubs endpoint.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf3b1-555">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf3b1-555">Next steps</span></span>

* <span data-ttu-id="bf3b1-556">Vytvoření metriky výstrahy v [Azure monitorování](../../monitoring-and-diagnostics/insights-alerts-portal.md) podle metrik, které shromažďujete.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-556">Create metric alerts in [Azure Monitor](../../monitoring-and-diagnostics/insights-alerts-portal.md) for the metrics you collect.</span></span>
* <span data-ttu-id="bf3b1-557">Vytvoření [tabulek sledování](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) pro vaše metriky.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-557">Create [monitoring charts](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) for your metrics.</span></span>
* <span data-ttu-id="bf3b1-558">Zjistěte, jak [vytvořit škálovací sadu virtuálních počítačů](/azure/virtual-machines/linux/tutorial-create-vmss) pomocí vaše metriky pro řízení automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="bf3b1-558">Learn how to [create a virtual machine scale set](/azure/virtual-machines/linux/tutorial-create-vmss) using your metrics to control autoscaling.</span></span>
