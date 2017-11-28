---
title: "Výpočetní aaaAzure - rozšíření diagnostiky Linux | Microsoft Docs"
description: "Jak tooconfigure hello Azure Linux diagnostiky rozšíření (LAD) toocollect metrik a protokolování událostí, z virtuálních počítačů Linux běží v Azure."
services: virtual-machines-linux
author: jasonzio
manager: anandram
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/09/2017
ms.author: jasonzio
ms.openlocfilehash: 2b27ec00a876ded359a75170b407e28c40d8445d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-linux-diagnostic-extension-toomonitor-metrics-and-logs"></a><span data-ttu-id="550df-103">Pomocí rozšíření diagnostiky Linux toomonitor metriky a protokoly</span><span class="sxs-lookup"><span data-stu-id="550df-103">Use Linux Diagnostic Extension toomonitor metrics and logs</span></span>

<span data-ttu-id="550df-104">Tento dokument popisuje verze 3.0 a novějších systému hello rozšíření diagnostiky Linux.</span><span class="sxs-lookup"><span data-stu-id="550df-104">This document describes version 3.0 and newer of hello Linux Diagnostic Extension.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="550df-105">Informace o 2.3 a starší verze najdete v tématu [tento dokument](./classic/diagnostic-extension-v2.md).</span><span class="sxs-lookup"><span data-stu-id="550df-105">For information about version 2.3 and older, see [this document](./classic/diagnostic-extension-v2.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="550df-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="550df-106">Introduction</span></span>

<span data-ttu-id="550df-107">Hello rozšíření diagnostiky Linux pomáhá monitorování hello stav uživatele z virtuálního počítače s Linuxem systémem Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="550df-107">hello Linux Diagnostic Extension helps a user monitor hello health of a Linux VM running on Microsoft Azure.</span></span> <span data-ttu-id="550df-108">Má hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="550df-108">It has hello following capabilities:</span></span>

* <span data-ttu-id="550df-109">Shromažďuje metriky výkonu systému z hello virtuálních počítačů a ukládá je do určité tabulky v účtu úložiště určený.</span><span class="sxs-lookup"><span data-stu-id="550df-109">Collects system performance metrics from hello VM and stores them in a specific table in a designated storage account.</span></span>
* <span data-ttu-id="550df-110">Načte události protokolu z syslog a ukládá je do určité tabulky v hello určený účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="550df-110">Retrieves log events from syslog and stores them in a specific table in hello designated storage account.</span></span>
* <span data-ttu-id="550df-111">Umožňuje uživatelům toocustomize hello data metriky, které jsou shromažďovány a nahrát.</span><span class="sxs-lookup"><span data-stu-id="550df-111">Enables users toocustomize hello data metrics that are collected and uploaded.</span></span>
* <span data-ttu-id="550df-112">Umožňuje uživatelům toocustomize hello syslog zařízení a úrovně závažnosti události, které jsou shromažďovány a nahrát.</span><span class="sxs-lookup"><span data-stu-id="550df-112">Enables users toocustomize hello syslog facilities and severity levels of events that are collected and uploaded.</span></span>
* <span data-ttu-id="550df-113">Umožňuje uživatelům tooupload protokol příslušné soubory tooa určené úložiště tabulky.</span><span class="sxs-lookup"><span data-stu-id="550df-113">Enables users tooupload specified log files tooa designated storage table.</span></span>
* <span data-ttu-id="550df-114">Podporuje odesílání metriky a protokolu událostí tooarbitrary EventHub koncových bodů a objekty BLOB ve formátu JSON v hello určený účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="550df-114">Supports sending metrics and log events tooarbitrary EventHub endpoints and JSON-formatted blobs in hello designated storage account.</span></span>

<span data-ttu-id="550df-115">Toto rozšíření pracuje s obou modelech nasazení Azure.</span><span class="sxs-lookup"><span data-stu-id="550df-115">This extension works with both Azure deployment models.</span></span>

## <a name="installing-hello-extension-in-your-vm"></a><span data-ttu-id="550df-116">Instaluje se rozšíření hello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="550df-116">Installing hello extension in your VM</span></span>

<span data-ttu-id="550df-117">Toto rozšíření můžete povolit pomocí rutin prostředí Azure PowerShell text hello, skripty rozhraní příkazového řádku Azure nebo Azure nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="550df-117">You can enable this extension by using hello Azure PowerShell cmdlets, Azure CLI scripts, or Azure deployment templates.</span></span> <span data-ttu-id="550df-118">Další informace najdete v tématu [rozšíření funkce](./extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="550df-118">For more information, see [Extensions Features](./extensions-features.md).</span></span>

<span data-ttu-id="550df-119">Hello portál Azure nelze použít tooenable nebo nakonfigurovat LAD 3.0.</span><span class="sxs-lookup"><span data-stu-id="550df-119">hello Azure portal cannot be used tooenable or configure LAD 3.0.</span></span> <span data-ttu-id="550df-120">Místo toho nainstaluje a nakonfiguruje verze 2.3.</span><span class="sxs-lookup"><span data-stu-id="550df-120">Instead, it installs and configures version 2.3.</span></span> <span data-ttu-id="550df-121">Výstrahy a portálu Azure grafy pracovat s daty v obou verzích hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="550df-121">Azure portal graphs and alerts work with data from both versions of hello extension.</span></span>

<span data-ttu-id="550df-122">Tyto pokyny k instalaci a [ke stažení ukázkové konfiguraci](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) konfigurace LAD 3.0 na:</span><span class="sxs-lookup"><span data-stu-id="550df-122">These installation instructions and a [downloadable sample configuration](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) configure LAD 3.0 to:</span></span>

* <span data-ttu-id="550df-123">zachycení a úložiště hello metriky stejné jako byly poskytované LAD 2.3;</span><span class="sxs-lookup"><span data-stu-id="550df-123">capture and store hello same metrics as were provided by LAD 2.3;</span></span>
* <span data-ttu-id="550df-124">zaznamenat užitečné sadu metriky systému souborů, nové tooLAD 3.0;</span><span class="sxs-lookup"><span data-stu-id="550df-124">capture a useful set of file system metrics, new tooLAD 3.0;</span></span>
* <span data-ttu-id="550df-125">zaznamenat hello výchozí syslog kolekci povoleno podle LAD 2.3;</span><span class="sxs-lookup"><span data-stu-id="550df-125">capture hello default syslog collection enabled by LAD 2.3;</span></span>
* <span data-ttu-id="550df-126">Povolte hello Azure portálu prostředí pro vytváření grafů a výstrahy na metriky virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="550df-126">enable hello Azure portal experience for charting and alerting on VM metrics.</span></span>

<span data-ttu-id="550df-127">zaváděná konfigurace Hello je jenom jako příklad; Upravit toosuit vašich potřeb.</span><span class="sxs-lookup"><span data-stu-id="550df-127">hello downloadable configuration is just an example; modify it toosuit your own needs.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="550df-128">Požadavky</span><span class="sxs-lookup"><span data-stu-id="550df-128">Prerequisites</span></span>

* <span data-ttu-id="550df-129">**Azure Linux Agent verze 2.2.0 nebo novější**.</span><span class="sxs-lookup"><span data-stu-id="550df-129">**Azure Linux Agent version 2.2.0 or later**.</span></span> <span data-ttu-id="550df-130">Většina virtuálních počítačů Linux Azure Galerie Image zahrnují verze 2.2.7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="550df-130">Most Azure VM Linux gallery images include version 2.2.7 or later.</span></span> <span data-ttu-id="550df-131">Spustit `/usr/sbin/waagent -version` tooconfirm hello verze nainstalované v hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="550df-131">Run `/usr/sbin/waagent -version` tooconfirm hello version installed on hello VM.</span></span> <span data-ttu-id="550df-132">Pokud hello virtuální počítač běží starší verze agenta hosta hello, postupujte podle [tyto pokyny](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) tooupdate ho.</span><span class="sxs-lookup"><span data-stu-id="550df-132">If hello VM is running an older version of hello guest agent, follow [these instructions](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) tooupdate it.</span></span>
* <span data-ttu-id="550df-133">**Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="550df-133">**Azure CLI**.</span></span> <span data-ttu-id="550df-134">[Nastavit hello Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) prostředí na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="550df-134">[Set up hello Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) environment on your machine.</span></span>
* <span data-ttu-id="550df-135">Hello wget příkazu, pokud ji nemáte: Spusťte `sudo apt-get install wget`.</span><span class="sxs-lookup"><span data-stu-id="550df-135">hello wget command, if you don't already have it: Run `sudo apt-get install wget`.</span></span>
* <span data-ttu-id="550df-136">Stávající předplatné Azure a existující úložiště účtu v něm toostore hello data.</span><span class="sxs-lookup"><span data-stu-id="550df-136">An existing Azure subscription and an existing storage account within it toostore hello data.</span></span>

### <a name="sample-installation"></a><span data-ttu-id="550df-137">Ukázka instalace</span><span class="sxs-lookup"><span data-stu-id="550df-137">Sample installation</span></span>

<span data-ttu-id="550df-138">Zadejte správné parametry hello na hello první tři řádky, a poté spusťte tento skript jako kořenového adresáře:</span><span class="sxs-lookup"><span data-stu-id="550df-138">Fill in hello correct parameters on hello first three lines, then execute this script as root:</span></span>

```bash
# Set your Azure VM diagnostic parameters correctly below
my_resource_group=<your_azure_resource_group_name_containing_your_azure_linux_vm>
my_linux_vm=<your_azure_linux_vm_name>
my_diagnostic_storage_account=<your_azure_storage_account_for_storing_vm_diagnostic_data>

# Should login tooAzure first before anything else
az login

# Select hello subscription containing hello storage account
az account set --subscription <your_azure_subscription_id>

# Download hello sample Public settings. (You could also use curl or any web browser)
wget https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json -O portal_public_settings.json

# Build hello VM resource ID. Replace storage account name and resource ID in hello public settings.
my_vm_resource_id=$(az vm show -g $my_resource_group -n $my_linux_vm --query "id" -o tsv)
sed -i "s#__DIAGNOSTIC_STORAGE_ACCOUNT__#$my_diagnostic_storage_account#g" portal_public_settings.json
sed -i "s#__VM_RESOURCE_ID__#$my_vm_resource_id#g" portal_public_settings.json

# Build hello protected settings (storage account SAS token)
my_diagnostic_storage_account_sastoken=$(az storage account generate-sas --account-name $my_diagnostic_storage_account --expiry 9999-12-31T23:59Z --permissions wlacu --resource-types co --services bt -o tsv)
my_lad_protected_settings="{'storageAccountName': '$my_diagnostic_storage_account', 'storageAccountSasToken': '$my_diagnostic_storage_account_sastoken'}"

# Finallly tell Azure tooinstall and enable hello extension
az vm extension set --publisher Microsoft.Azure.Diagnostics --name LinuxDiagnostic --version 3.0 --resource-group $my_resource_group --vm-name $my_linux_vm --protected-settings "${my_lad_protected_settings}" --settings portal_public_settings.json
```

<span data-ttu-id="550df-139">Adresa URL Hello hello Ukázková konfigurace a její obsah, jsou toochange subjektu.</span><span class="sxs-lookup"><span data-stu-id="550df-139">hello URL for hello sample configuration, and its contents, are subject toochange.</span></span> <span data-ttu-id="550df-140">Stáhněte si kopii souboru JSON hello nastavení portálu a přizpůsobit pro své potřeby.</span><span class="sxs-lookup"><span data-stu-id="550df-140">Download a copy of hello portal settings JSON file and customize it for your needs.</span></span> <span data-ttu-id="550df-141">Všechny šablony nebo automatizace, které vytvoříte měli používat vlastní kopie místo stahování pokaždé, když tuto adresu URL.</span><span class="sxs-lookup"><span data-stu-id="550df-141">Any templates or automation you construct should use your own copy, rather than downloading that URL each time.</span></span>

### <a name="updating-hello-extension-settings"></a><span data-ttu-id="550df-142">Aktualizuje se nastavení rozšíření hello</span><span class="sxs-lookup"><span data-stu-id="550df-142">Updating hello extension settings</span></span>

<span data-ttu-id="550df-143">Po změně chráněné nebo veřejné nastavení, je nasadit toohello virtuálních počítačů tak, že spustíte hello stejný příkaz.</span><span class="sxs-lookup"><span data-stu-id="550df-143">After you've changed your Protected or Public settings, deploy them toohello VM by running hello same command.</span></span> <span data-ttu-id="550df-144">Pokud nic změnit v nastavení hello, odešlou hello aktualizovat nastavení toohello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="550df-144">If anything changed in hello settings, hello updated settings are sent toohello extension.</span></span> <span data-ttu-id="550df-145">LAD hello konfigurace znovu načte a restartuje sám sebe.</span><span class="sxs-lookup"><span data-stu-id="550df-145">LAD reloads hello configuration and restarts itself.</span></span>

### <a name="migration-from-previous-versions-of-hello-extension"></a><span data-ttu-id="550df-146">Migrace z předchozích verzí rozšíření hello</span><span class="sxs-lookup"><span data-stu-id="550df-146">Migration from previous versions of hello extension</span></span>

<span data-ttu-id="550df-147">nejnovější verze rozšíření hello Hello je **3.0**.</span><span class="sxs-lookup"><span data-stu-id="550df-147">hello latest version of hello extension is **3.0**.</span></span> <span data-ttu-id="550df-148">**Všechny starší verze (2.x) jsou zastaralé a může být publikování na nebo po 31. července 2018**.</span><span class="sxs-lookup"><span data-stu-id="550df-148">**Any old versions (2.x) are deprecated and may be unpublished on or after July 31, 2018**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="550df-149">Toto rozšíření zavádí narušující změny toohello konfigurace rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="550df-149">This extension introduces breaking changes toohello configuration of hello extension.</span></span> <span data-ttu-id="550df-150">Jedna taková změna byla provedená tooimprove hello zabezpečení hello rozšíření; v důsledku toho zpětné kompatibility s 2.x nebylo možné udržovat.</span><span class="sxs-lookup"><span data-stu-id="550df-150">One such change was made tooimprove hello security of hello extension; as a result, backwards compatibility with 2.x could not be maintained.</span></span> <span data-ttu-id="550df-151">Navíc se liší od vydavatele hello verze 2.x hello hello vydavatel rozšíření pro tuto příponu.</span><span class="sxs-lookup"><span data-stu-id="550df-151">Also, hello Extension Publisher for this extension is different than hello publisher for hello 2.x versions.</span></span>
>
> <span data-ttu-id="550df-152">toomigrate z 2.x toothis novou verzi hello rozšíření, je nutné odinstalovat původní rozšíření hello (pod původním názvem vydavatele hello) a potom nainstalovat verzi 3 hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="550df-152">toomigrate from 2.x toothis new version of hello extension, you must uninstall hello old extension (under hello old publisher name), then install version 3 of hello extension.</span></span>

<span data-ttu-id="550df-153">Doporučení:</span><span class="sxs-lookup"><span data-stu-id="550df-153">Recommendations:</span></span>

* <span data-ttu-id="550df-154">Nainstalujte rozšíření hello automatické podverze upgradu povolena.</span><span class="sxs-lookup"><span data-stu-id="550df-154">Install hello extension with automatic minor version upgrade enabled.</span></span>
  * <span data-ttu-id="550df-155">V modelu nasazení classic virtuální počítače zadejte '3.*' jako verze hello při instalaci rozšíření hello prostřednictvím příkazového řádku Azure XPLAT nebo Powershellu.</span><span class="sxs-lookup"><span data-stu-id="550df-155">On classic deployment model VMs, specify '3.*' as hello version if you are installing hello extension through Azure XPLAT CLI or Powershell.</span></span>
  * <span data-ttu-id="550df-156">Při nasazení Azure Resource Manager modelu virtuálních počítačů, zahrnují ' "autoUpgradeMinorVersion": true, v hello šablony nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="550df-156">On Azure Resource Manager deployment model VMs, include '"autoUpgradeMinorVersion": true' in hello VM deployment template.</span></span>
* <span data-ttu-id="550df-157">Použijte nový nebo jiný účet úložiště pro LAD 3.0.</span><span class="sxs-lookup"><span data-stu-id="550df-157">Use a new/different storage account for LAD 3.0.</span></span> <span data-ttu-id="550df-158">Existuje několik malých nekompatibility mezi LAD 2.3 a LAD 3.0, které usnadňují sdílení problémových účet:</span><span class="sxs-lookup"><span data-stu-id="550df-158">There are several small incompatibilities between LAD 2.3 and LAD 3.0 that make sharing an account troublesome:</span></span>
  * <span data-ttu-id="550df-159">LAD 3.0 ukládá události procesu syslog v tabulce s jiným názvem.</span><span class="sxs-lookup"><span data-stu-id="550df-159">LAD 3.0 stores syslog events in a table with a different name.</span></span>
  * <span data-ttu-id="550df-160">Hello counterSpecifier řetězce pro `builtin` metriky rozdílnou LAD 3.0.</span><span class="sxs-lookup"><span data-stu-id="550df-160">hello counterSpecifier strings for `builtin` metrics differ in LAD 3.0.</span></span>

## <a name="protected-settings"></a><span data-ttu-id="550df-161">Chráněné nastavení</span><span class="sxs-lookup"><span data-stu-id="550df-161">Protected settings</span></span>

<span data-ttu-id="550df-162">Tato sada informace o konfiguraci obsahuje citlivé informace, které by měly být chráněné z veřejného zobrazení, například přihlašovací údaje úložiště.</span><span class="sxs-lookup"><span data-stu-id="550df-162">This set of configuration information contains sensitive information that should be protected from public view, for example, storage credentials.</span></span> <span data-ttu-id="550df-163">Tato nastavení jsou přenášená tooand uložené hello rozšíření v šifrovaném tvaru.</span><span class="sxs-lookup"><span data-stu-id="550df-163">These settings are transmitted tooand stored by hello extension in encrypted form.</span></span>

```json
{
    "storageAccountName" : "hello storage account tooreceive data",
    "storageAccountEndPoint": "hello hostname suffix for hello cloud for this account",
    "storageAccountSasToken": "SAS access token",
    "mdsdHttpProxy": "HTTP proxy settings",
    "sinksConfig": { ... }
}
```

<span data-ttu-id="550df-164">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="550df-164">Name</span></span> | <span data-ttu-id="550df-165">Hodnota</span><span class="sxs-lookup"><span data-stu-id="550df-165">Value</span></span>
---- | -----
<span data-ttu-id="550df-166">storageAccountName</span><span class="sxs-lookup"><span data-stu-id="550df-166">storageAccountName</span></span> | <span data-ttu-id="550df-167">Hello název účtu úložiště hello, ve kterém je rozšířením hello zapisovat data.</span><span class="sxs-lookup"><span data-stu-id="550df-167">hello name of hello storage account in which data is written by hello extension.</span></span>
<span data-ttu-id="550df-168">storageAccountEndPoint</span><span class="sxs-lookup"><span data-stu-id="550df-168">storageAccountEndPoint</span></span> | <span data-ttu-id="550df-169">koncový bod (volitelné) hello identifikace hello cloudu, ve které hello existuje účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="550df-169">(optional) hello endpoint identifying hello cloud in which hello storage account exists.</span></span> <span data-ttu-id="550df-170">Chybí-li toto nastavení, LAD výchozí toohello veřejného cloudu Azure, `https://core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="550df-170">If this setting is absent, LAD defaults toohello Azure public cloud, `https://core.windows.net`.</span></span> <span data-ttu-id="550df-171">Tuto hodnotu nastavit toouse účet úložiště v Azure v Německu, Azure Government nebo Azure China odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="550df-171">toouse a storage account in Azure Germany, Azure Government, or Azure China, set this value accordingly.</span></span>
<span data-ttu-id="550df-172">storageAccountSasToken</span><span class="sxs-lookup"><span data-stu-id="550df-172">storageAccountSasToken</span></span> | <span data-ttu-id="550df-173">[Tokenu SAS účtu](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) pro služby objektů Blob a tabulky (`ss='bt'`), použít toocontainers a objekty (`srt='co'`), která uděluje přidat, vytvářet, seznam, aktualizovat a oprávnění k zápisu (`sp='acluw'`).</span><span class="sxs-lookup"><span data-stu-id="550df-173">An [Account SAS token](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) for Blob and Table services (`ss='bt'`), applicable toocontainers and objects (`srt='co'`), which grants add, create, list, update, and write permissions (`sp='acluw'`).</span></span> <span data-ttu-id="550df-174">Proveďte *není* zahrnují hello úvodní otazník (?).</span><span class="sxs-lookup"><span data-stu-id="550df-174">Do *not* include hello leading question-mark (?).</span></span>
<span data-ttu-id="550df-175">mdsdHttpProxy</span><span class="sxs-lookup"><span data-stu-id="550df-175">mdsdHttpProxy</span></span> | <span data-ttu-id="550df-176">(volitelné) HTTP proxy informace potřebné tooenable hello rozšíření tooconnect toohello zadaný účet úložiště a koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="550df-176">(optional) HTTP proxy information needed tooenable hello extension tooconnect toohello specified storage account and endpoint.</span></span>
<span data-ttu-id="550df-177">sinksConfig</span><span class="sxs-lookup"><span data-stu-id="550df-177">sinksConfig</span></span> | <span data-ttu-id="550df-178">(volitelné) Podrobnosti o alternativní cíle toowhich metriky a události mohou být zajišťovány.</span><span class="sxs-lookup"><span data-stu-id="550df-178">(optional) Details of alternative destinations toowhich metrics and events can be delivered.</span></span> <span data-ttu-id="550df-179">Hello konkrétní podrobnosti o každém jímku dat nepodporuje hello rozšíření jsou popsané v následujících hello částech.</span><span class="sxs-lookup"><span data-stu-id="550df-179">hello specific details of each data sink supported by hello extension are covered in hello sections that follow.</span></span>

<span data-ttu-id="550df-180">Můžete snadno vytvořit token SAS hello požadované prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="550df-180">You can easily construct hello required SAS token through hello Azure portal.</span></span>

1. <span data-ttu-id="550df-181">Vyberte toowhich účet úložiště pro obecné účely hello chcete toowrite rozšíření hello</span><span class="sxs-lookup"><span data-stu-id="550df-181">Select hello general-purpose storage account toowhich you want hello extension toowrite</span></span>
1. <span data-ttu-id="550df-182">Vyberte "Sdílený přístupový podpis" z hello nastavení součástí levé nabídce hello</span><span class="sxs-lookup"><span data-stu-id="550df-182">Select "Shared access signature" from hello Settings part of hello left menu</span></span>
1. <span data-ttu-id="550df-183">Ujistěte se, jak už bylo popsáno příslušné části hello</span><span class="sxs-lookup"><span data-stu-id="550df-183">Make hello appropriate sections as previously described</span></span>
1. <span data-ttu-id="550df-184">Klikněte na tlačítko "Generovat SAS" hello.</span><span class="sxs-lookup"><span data-stu-id="550df-184">Click hello "Generate SAS" button.</span></span>

![Bitové kopie](./media/diagnostic-extension/make_sas.png)

<span data-ttu-id="550df-186">Kopírování hello vygeneruje SAS do pole storageAccountSasToken hello; odebrat hello úvodní otazník ("?").</span><span class="sxs-lookup"><span data-stu-id="550df-186">Copy hello generated SAS into hello storageAccountSasToken field; remove hello leading question-mark ("?").</span></span>

### <a name="sinksconfig"></a><span data-ttu-id="550df-187">sinksConfig</span><span class="sxs-lookup"><span data-stu-id="550df-187">sinksConfig</span></span>

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

<span data-ttu-id="550df-188">Tento volitelný oddíl definuje další cíle, které toowhich hello rozšíření odesílá hello informace, které shromáždí.</span><span class="sxs-lookup"><span data-stu-id="550df-188">This optional section defines additional destinations toowhich hello extension sends hello information it collects.</span></span> <span data-ttu-id="550df-189">pole "podřízený" Hello obsahuje objekt pro každý další data jímky.</span><span class="sxs-lookup"><span data-stu-id="550df-189">hello "sink" array contains an object for each additional data sink.</span></span> <span data-ttu-id="550df-190">Hello atributu "typ" Určuje hello další atributy v objektu hello.</span><span class="sxs-lookup"><span data-stu-id="550df-190">hello "type" attribute determines hello other attributes in hello object.</span></span>

<span data-ttu-id="550df-191">Element</span><span class="sxs-lookup"><span data-stu-id="550df-191">Element</span></span> | <span data-ttu-id="550df-192">Hodnota</span><span class="sxs-lookup"><span data-stu-id="550df-192">Value</span></span>
------- | -----
<span data-ttu-id="550df-193">jméno</span><span class="sxs-lookup"><span data-stu-id="550df-193">name</span></span> | <span data-ttu-id="550df-194">Řetězec se používá podřízený toothis toorefer jinde v konfiguraci rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="550df-194">A string used toorefer toothis sink elsewhere in hello extension configuration.</span></span>
<span data-ttu-id="550df-195">type</span><span class="sxs-lookup"><span data-stu-id="550df-195">type</span></span> | <span data-ttu-id="550df-196">Hello typ jímky definovaný.</span><span class="sxs-lookup"><span data-stu-id="550df-196">hello type of sink being defined.</span></span> <span data-ttu-id="550df-197">Určuje instance tohoto typu hello ostatní hodnoty (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="550df-197">Determines hello other values (if any) in instances of this type.</span></span>

<span data-ttu-id="550df-198">Verze 3.0 hello rozšíření diagnostiky Linux podporuje dva typy podřízený: EventHub a JsonBlob.</span><span class="sxs-lookup"><span data-stu-id="550df-198">Version 3.0 of hello Linux Diagnostic Extension supports two sink types: EventHub, and JsonBlob.</span></span>

#### <a name="hello-eventhub-sink"></a><span data-ttu-id="550df-199">podřízený EventHub Hello</span><span class="sxs-lookup"><span data-stu-id="550df-199">hello EventHub sink</span></span>

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

<span data-ttu-id="550df-200">zadání "sasURL" Hello obsahuje hello úplnou adresu URL, včetně tokenu SAS pro hello centra událostí toowhich data by měla být zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="550df-200">hello "sasURL" entry contains hello full URL, including SAS token, for hello Event Hub toowhich data should be published.</span></span> <span data-ttu-id="550df-201">LAD vyžaduje SAS pojmenování zásadu, která umožňuje odesílání hello deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="550df-201">LAD requires a SAS naming a policy that enables hello Send claim.</span></span> <span data-ttu-id="550df-202">Příklad:</span><span class="sxs-lookup"><span data-stu-id="550df-202">An example:</span></span>

* <span data-ttu-id="550df-203">Vytvořit obor názvů Event Hubs s názvem`contosohub`</span><span class="sxs-lookup"><span data-stu-id="550df-203">Create an Event Hubs namespace called `contosohub`</span></span>
* <span data-ttu-id="550df-204">Vytvoření centra událostí v oboru názvů hello názvem`syslogmsgs`</span><span class="sxs-lookup"><span data-stu-id="550df-204">Create an Event Hub in hello namespace called `syslogmsgs`</span></span>
* <span data-ttu-id="550df-205">Vytvoření zásady sdíleného přístupu na hello centra událostí s názvem `writer` , umožňuje hello odesílat deklarace identity</span><span class="sxs-lookup"><span data-stu-id="550df-205">Create a Shared access policy on hello Event Hub named `writer` that enables hello Send claim</span></span>

<span data-ttu-id="550df-206">Pokud jste vytvořili SAS funkční až půlnoci času UTC na 1. ledna 2018 hello sasURL hodnota může být:</span><span class="sxs-lookup"><span data-stu-id="550df-206">If you created a SAS good until midnight UTC on January 1, 2018, hello sasURL value might be:</span></span>

```url
https://contosohub.servicebus.windows.net/syslogmsgs?sr=contosohub.servicebus.windows.net%2fsyslogmsgs&sig=xxxxxxxxxxxxxxxxxxxxxxxxx&se=1514764800&skn=writer
```

<span data-ttu-id="550df-207">Další informace o generování tokeny SAS pro Event Hubs naleznete v tématu [této webové stránce](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).</span><span class="sxs-lookup"><span data-stu-id="550df-207">For more information about generating SAS tokens for Event Hubs, see [this web page](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).</span></span>

#### <a name="hello-jsonblob-sink"></a><span data-ttu-id="550df-208">podřízený JsonBlob Hello</span><span class="sxs-lookup"><span data-stu-id="550df-208">hello JsonBlob sink</span></span>

```json
"sink": [
    {
        "name": "sinkname",
        "type": "JsonBlob"
    },
    ...
]
```

<span data-ttu-id="550df-209">Data směrované podřízený JsonBlob tooa se ukládají do objektů BLOB v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="550df-209">Data directed tooa JsonBlob sink is stored in blobs in Azure storage.</span></span> <span data-ttu-id="550df-210">Každá instance LAD vytvoří každou hodinu pro každý název podřízený objekt blob.</span><span class="sxs-lookup"><span data-stu-id="550df-210">Each instance of LAD creates a blob every hour for each sink name.</span></span> <span data-ttu-id="550df-211">Každý objekt blob vždy obsahuje syntakticky pole JSON objektu.</span><span class="sxs-lookup"><span data-stu-id="550df-211">Each blob always contains a syntactically valid JSON array of object.</span></span> <span data-ttu-id="550df-212">Nové položky se přidají atomicky toohello pole.</span><span class="sxs-lookup"><span data-stu-id="550df-212">New entries are atomically added toohello array.</span></span> <span data-ttu-id="550df-213">Objekty BLOB jsou uloženy v kontejneru s hello stejný název jako jímku hello.</span><span class="sxs-lookup"><span data-stu-id="550df-213">Blobs are stored in a container with hello same name as hello sink.</span></span> <span data-ttu-id="550df-214">Hello pravidla úložiště Azure pro názvy objektů blob kontejneru použít názvy toohello JsonBlob jímky: mezi 3 a 63 malé alfanumerické znaky ASCII nebo pomlčky.</span><span class="sxs-lookup"><span data-stu-id="550df-214">hello Azure storage rules for blob container names apply toohello names of JsonBlob sinks: between 3 and 63 lower-case alphanumeric ASCII characters or dashes.</span></span>

## <a name="public-settings"></a><span data-ttu-id="550df-215">Nastavení veřejné</span><span class="sxs-lookup"><span data-stu-id="550df-215">Public settings</span></span>

<span data-ttu-id="550df-216">Tato struktura obsahuje různé bloky nastavení, které řídí hello shromažďované nástrojem hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="550df-216">This structure contains various blocks of settings that control hello information collected by hello extension.</span></span> <span data-ttu-id="550df-217">Každé nastavení je volitelné.</span><span class="sxs-lookup"><span data-stu-id="550df-217">Each setting is optional.</span></span> <span data-ttu-id="550df-218">Pokud zadáte `ladCfg`, musíte zadat také `StorageAccount`.</span><span class="sxs-lookup"><span data-stu-id="550df-218">If you specify `ladCfg`, you must also specify `StorageAccount`.</span></span>

```json
{
    "ladCfg":  { ... },
    "perfCfg": { ... },
    "fileLogs": { ... },
    "StorageAccount": "hello storage account tooreceive data",
    "mdsdHttpProxy" : ""
}
```

<span data-ttu-id="550df-219">Element</span><span class="sxs-lookup"><span data-stu-id="550df-219">Element</span></span> | <span data-ttu-id="550df-220">Hodnota</span><span class="sxs-lookup"><span data-stu-id="550df-220">Value</span></span>
------- | -----
<span data-ttu-id="550df-221">StorageAccount</span><span class="sxs-lookup"><span data-stu-id="550df-221">StorageAccount</span></span> | <span data-ttu-id="550df-222">Hello název účtu úložiště hello, ve kterém je rozšířením hello zapisovat data.</span><span class="sxs-lookup"><span data-stu-id="550df-222">hello name of hello storage account in which data is written by hello extension.</span></span> <span data-ttu-id="550df-223">Musí být stejný název, jako jsou zadány v hello hello [chráněné nastavení](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="550df-223">Must be hello same name as is specified in hello [Protected settings](#protected-settings).</span></span>
<span data-ttu-id="550df-224">mdsdHttpProxy</span><span class="sxs-lookup"><span data-stu-id="550df-224">mdsdHttpProxy</span></span> | <span data-ttu-id="550df-225">(volitelné) Stejné jako hello [chráněné nastavení](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="550df-225">(optional) Same as in hello [Protected settings](#protected-settings).</span></span> <span data-ttu-id="550df-226">hodnota veřejného Hello je přepsat privátní hodnotou hello, pokud nastavení.</span><span class="sxs-lookup"><span data-stu-id="550df-226">hello public value is overridden by hello private value, if set.</span></span> <span data-ttu-id="550df-227">Umístěte nastavení proxy serveru, které obsahují tajného klíče, jako jsou hesla, v hello [chráněné nastavení](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="550df-227">Place proxy settings that contain a secret, such as a password, in hello [Protected settings](#protected-settings).</span></span>

<span data-ttu-id="550df-228">Hello zbývající prvky jsou podrobně popsané v v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="550df-228">hello remaining elements are described in detail in hello following sections.</span></span>

### <a name="ladcfg"></a><span data-ttu-id="550df-229">ladCfg</span><span class="sxs-lookup"><span data-stu-id="550df-229">ladCfg</span></span>

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

<span data-ttu-id="550df-230">Tento volitelný struktura ovládací prvky hello shromažďování metrik a protokoly pro doručení toohello dat služby a tooother metrik Azure jímky.</span><span class="sxs-lookup"><span data-stu-id="550df-230">This optional structure controls hello gathering of metrics and logs for delivery toohello Azure Metrics service and tooother data sinks.</span></span> <span data-ttu-id="550df-231">Je nutné zadat buď `performanceCounters` nebo `syslogEvents` nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="550df-231">You must specify either `performanceCounters` or `syslogEvents` or both.</span></span> <span data-ttu-id="550df-232">Je nutné zadat hello `metrics` struktura.</span><span class="sxs-lookup"><span data-stu-id="550df-232">You must specify hello `metrics` structure.</span></span>

<span data-ttu-id="550df-233">Element</span><span class="sxs-lookup"><span data-stu-id="550df-233">Element</span></span> | <span data-ttu-id="550df-234">Hodnota</span><span class="sxs-lookup"><span data-stu-id="550df-234">Value</span></span>
------- | -----
<span data-ttu-id="550df-235">eventVolume</span><span class="sxs-lookup"><span data-stu-id="550df-235">eventVolume</span></span> | <span data-ttu-id="550df-236">(volitelné) Ovládací prvky hello počet oddílů v rámci tabulku úložiště hello vytvořili.</span><span class="sxs-lookup"><span data-stu-id="550df-236">(optional) Controls hello number of partitions created within hello storage table.</span></span> <span data-ttu-id="550df-237">Musí mít jednu z `"Large"`, `"Medium"`, nebo `"Small"`.</span><span class="sxs-lookup"><span data-stu-id="550df-237">Must be one of `"Large"`, `"Medium"`, or `"Small"`.</span></span> <span data-ttu-id="550df-238">Pokud není zadaný, hello výchozí hodnota je `"Medium"`.</span><span class="sxs-lookup"><span data-stu-id="550df-238">If not specified, hello default value is `"Medium"`.</span></span>
<span data-ttu-id="550df-239">sampleRateInSeconds</span><span class="sxs-lookup"><span data-stu-id="550df-239">sampleRateInSeconds</span></span> | <span data-ttu-id="550df-240">(volitelné) hello výchozí interval mezi kolekce nezpracovaná (neagregovaným) metrik.</span><span class="sxs-lookup"><span data-stu-id="550df-240">(optional) hello default interval between collection of raw (unaggregated) metrics.</span></span> <span data-ttu-id="550df-241">Hello nejmenší podporované vzorkovací frekvence je 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="550df-241">hello smallest supported sample rate is 15 seconds.</span></span> <span data-ttu-id="550df-242">Pokud není zadaný, hello výchozí hodnota je `15`.</span><span class="sxs-lookup"><span data-stu-id="550df-242">If not specified, hello default value is `15`.</span></span>

#### <a name="metrics"></a><span data-ttu-id="550df-243">Průzkumníku metrik</span><span class="sxs-lookup"><span data-stu-id="550df-243">metrics</span></span>

```json
"metrics": {
    "resourceId": "/subscriptions/...",
    "metricAggregation" : [
        { "scheduledTransferPeriod" : "PT1H" },
        { "scheduledTransferPeriod" : "PT5M" }
    ]
}
```

<span data-ttu-id="550df-244">Element</span><span class="sxs-lookup"><span data-stu-id="550df-244">Element</span></span> | <span data-ttu-id="550df-245">Hodnota</span><span class="sxs-lookup"><span data-stu-id="550df-245">Value</span></span>
------- | -----
<span data-ttu-id="550df-246">resourceId</span><span class="sxs-lookup"><span data-stu-id="550df-246">resourceId</span></span> | <span data-ttu-id="550df-247">ID prostředku Azure Resource Manager Hello hello virtuálního počítače nebo škálování virtuálních počítačů hello nastavit hello toowhich, ke které patří virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="550df-247">hello Azure Resource Manager resource ID of hello VM or of hello virtual machine scale set toowhich hello VM belongs.</span></span> <span data-ttu-id="550df-248">Toto nastavení musí být zadaná také pokud žádné podřízený JsonBlob se používá v konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="550df-248">This setting must be also specified if any JsonBlob sink is used in hello configuration.</span></span>
<span data-ttu-id="550df-249">scheduledTransferPeriod</span><span class="sxs-lookup"><span data-stu-id="550df-249">scheduledTransferPeriod</span></span> | <span data-ttu-id="550df-250">frekvence Hello, kdy jsou agregovaná metrika toobe počítaný a přenést tooAzure metriky, vyjádřené jako interval času je 8601.</span><span class="sxs-lookup"><span data-stu-id="550df-250">hello frequency at which aggregate metrics are toobe computed and transferred tooAzure Metrics, expressed as an IS 8601 time interval.</span></span> <span data-ttu-id="550df-251">doba přenosu nejmenší Hello je 60 sekund, který je PT1M.</span><span class="sxs-lookup"><span data-stu-id="550df-251">hello smallest transfer period is 60 seconds, that is, PT1M.</span></span> <span data-ttu-id="550df-252">Je nutné zadat alespoň jeden scheduledTransferPeriod.</span><span class="sxs-lookup"><span data-stu-id="550df-252">You must specify at least one scheduledTransferPeriod.</span></span>

<span data-ttu-id="550df-253">Ukázky hello metriky uvedený v oddílu hello čítače výkonu jsou shromažďována každých 15 sekund nebo na hello vzorkovací frekvence explicitně definován čítače hello.</span><span class="sxs-lookup"><span data-stu-id="550df-253">Samples of hello metrics specified in hello performanceCounters section are collected every 15 seconds or at hello sample rate explicitly defined for hello counter.</span></span> <span data-ttu-id="550df-254">Pokud se zobrazí více scheduledTransferPeriod frekvence (jako třeba hello), každé agregace se počítá samostatně.</span><span class="sxs-lookup"><span data-stu-id="550df-254">If multiple scheduledTransferPeriod frequencies appear (as in hello example), each aggregation is computed independently.</span></span>

#### <a name="performancecounters"></a><span data-ttu-id="550df-255">čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="550df-255">performanceCounters</span></span>

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

<span data-ttu-id="550df-256">Tento volitelný oddíl ovládací prvky kolekce hello metrik.</span><span class="sxs-lookup"><span data-stu-id="550df-256">This optional section controls hello collection of metrics.</span></span> <span data-ttu-id="550df-257">Nezpracovaná ukázky jsou agregována pro každou [scheduledTransferPeriod](#metrics) tooproduce tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="550df-257">Raw samples are aggregated for each [scheduledTransferPeriod](#metrics) tooproduce these values:</span></span>

* <span data-ttu-id="550df-258">střední</span><span class="sxs-lookup"><span data-stu-id="550df-258">mean</span></span>
* <span data-ttu-id="550df-259">minimální</span><span class="sxs-lookup"><span data-stu-id="550df-259">minimum</span></span>
* <span data-ttu-id="550df-260">Maximální počet</span><span class="sxs-lookup"><span data-stu-id="550df-260">maximum</span></span>
* <span data-ttu-id="550df-261">shromážděné poslední hodnota</span><span class="sxs-lookup"><span data-stu-id="550df-261">last-collected value</span></span>
* <span data-ttu-id="550df-262">počet nezpracovaných vzorků, které se používá toocompute hello agregace</span><span class="sxs-lookup"><span data-stu-id="550df-262">count of raw samples used toocompute hello aggregate</span></span>

<span data-ttu-id="550df-263">Element</span><span class="sxs-lookup"><span data-stu-id="550df-263">Element</span></span> | <span data-ttu-id="550df-264">Hodnota</span><span class="sxs-lookup"><span data-stu-id="550df-264">Value</span></span>
------- | -----
<span data-ttu-id="550df-265">jímky</span><span class="sxs-lookup"><span data-stu-id="550df-265">sinks</span></span> | <span data-ttu-id="550df-266">(volitelné) Čárkami oddělený seznam názvy jímky toowhich LAD odesílá agregované výsledky metriky.</span><span class="sxs-lookup"><span data-stu-id="550df-266">(optional) A comma-separated list of names of sinks toowhich LAD sends aggregated metric results.</span></span> <span data-ttu-id="550df-267">Všechny agregovaných metrik jsou publikované tooeach uvedené jímky.</span><span class="sxs-lookup"><span data-stu-id="550df-267">All aggregated metrics are published tooeach listed sink.</span></span> <span data-ttu-id="550df-268">V tématu [sinksConfig](#sinksconfig).</span><span class="sxs-lookup"><span data-stu-id="550df-268">See [sinksConfig](#sinksconfig).</span></span> <span data-ttu-id="550df-269">Příklad: `"EHsink1, myjsonsink"`.</span><span class="sxs-lookup"><span data-stu-id="550df-269">Example: `"EHsink1, myjsonsink"`.</span></span>
<span data-ttu-id="550df-270">type</span><span class="sxs-lookup"><span data-stu-id="550df-270">type</span></span> | <span data-ttu-id="550df-271">Identifikuje hello skutečné zprostředkovatele hello metriky.</span><span class="sxs-lookup"><span data-stu-id="550df-271">Identifies hello actual provider of hello metric.</span></span>
<span data-ttu-id="550df-272">– Třída</span><span class="sxs-lookup"><span data-stu-id="550df-272">class</span></span> | <span data-ttu-id="550df-273">Společně s "čítač" identifikuje hello určité metriky v rámci obor názvů zprostředkovatele hello.</span><span class="sxs-lookup"><span data-stu-id="550df-273">Together with "counter", identifies hello specific metric within hello provider's namespace.</span></span>
<span data-ttu-id="550df-274">Čítač</span><span class="sxs-lookup"><span data-stu-id="550df-274">counter</span></span> | <span data-ttu-id="550df-275">Společně s "class" identifikuje hello určité metriky v rámci obor názvů zprostředkovatele hello.</span><span class="sxs-lookup"><span data-stu-id="550df-275">Together with "class", identifies hello specific metric within hello provider's namespace.</span></span>
<span data-ttu-id="550df-276">counterSpecifier</span><span class="sxs-lookup"><span data-stu-id="550df-276">counterSpecifier</span></span> | <span data-ttu-id="550df-277">Identifikuje hello určité metriky v rámci oboru názvů hello metrik Azure.</span><span class="sxs-lookup"><span data-stu-id="550df-277">Identifies hello specific metric within hello Azure Metrics namespace.</span></span>
<span data-ttu-id="550df-278">Podmínka</span><span class="sxs-lookup"><span data-stu-id="550df-278">condition</span></span> | <span data-ttu-id="550df-279">(volitelné) Použije konkrétní instanci hello objekt toowhich hello metrika vybere nebo vybere hello agregace napříč všemi instancemi tohoto objektu.</span><span class="sxs-lookup"><span data-stu-id="550df-279">(optional) Selects a specific instance of hello object toowhich hello metric applies or selects hello aggregation across all instances of that object.</span></span> <span data-ttu-id="550df-280">Další informace najdete v tématu hello [ `builtin` definice metrik](#metrics-supported-by-builtin).</span><span class="sxs-lookup"><span data-stu-id="550df-280">For more information, see hello [`builtin` metric definitions](#metrics-supported-by-builtin).</span></span>
<span data-ttu-id="550df-281">sampleRate</span><span class="sxs-lookup"><span data-stu-id="550df-281">sampleRate</span></span> | <span data-ttu-id="550df-282">JE 8601 interval, který nastaví četnost Dobrý den, kdy se shromažďují nezpracovaná ukázky pro tuto metriku.</span><span class="sxs-lookup"><span data-stu-id="550df-282">IS 8601 interval that sets hello rate at which raw samples for this metric are collected.</span></span> <span data-ttu-id="550df-283">Pokud není nastavená, interval sběru hello se nastavuje hodnotu hello [sampleRateInSeconds](#ladcfg).</span><span class="sxs-lookup"><span data-stu-id="550df-283">If not set, hello collection interval is set by hello value of [sampleRateInSeconds](#ladcfg).</span></span> <span data-ttu-id="550df-284">Hello nejkratší podporované vzorkovací frekvence je 15 sekund (PT15S).</span><span class="sxs-lookup"><span data-stu-id="550df-284">hello shortest supported sample rate is 15 seconds (PT15S).</span></span>
<span data-ttu-id="550df-285">jednotka</span><span class="sxs-lookup"><span data-stu-id="550df-285">unit</span></span> | <span data-ttu-id="550df-286">By měl být jeden z těchto řetězců: "Count", "Bajtů", "Seconds", "Procenta", "CountPerSecond", "BytesPerSecond", "Milisekundu".</span><span class="sxs-lookup"><span data-stu-id="550df-286">Should be one of these strings: "Count", "Bytes", "Seconds", "Percent", "CountPerSecond", "BytesPerSecond", "Millisecond".</span></span> <span data-ttu-id="550df-287">Definuje hello jednotky pro metriku hello.</span><span class="sxs-lookup"><span data-stu-id="550df-287">Defines hello unit for hello metric.</span></span> <span data-ttu-id="550df-288">Spotřebitelé dat shromážděných hello očekávat, že hello shromažďují data hodnoty toomatch tuto jednotku.</span><span class="sxs-lookup"><span data-stu-id="550df-288">Consumers of hello collected data expect hello collected data values toomatch this unit.</span></span> <span data-ttu-id="550df-289">LAD ignoruje toto pole.</span><span class="sxs-lookup"><span data-stu-id="550df-289">LAD ignores this field.</span></span>
<span data-ttu-id="550df-290">displayName</span><span class="sxs-lookup"><span data-stu-id="550df-290">displayName</span></span> | <span data-ttu-id="550df-291">Popisek (v jazyce hello určeného přidružené nastavení národního prostředí v hello) toobe Hello připojit toothis data v Azure metriky.</span><span class="sxs-lookup"><span data-stu-id="550df-291">hello label (in hello language specified by hello associated locale setting) toobe attached toothis data in Azure Metrics.</span></span> <span data-ttu-id="550df-292">LAD ignoruje toto pole.</span><span class="sxs-lookup"><span data-stu-id="550df-292">LAD ignores this field.</span></span>

<span data-ttu-id="550df-293">Hello counterSpecifier je libovolný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="550df-293">hello counterSpecifier is an arbitrary identifier.</span></span> <span data-ttu-id="550df-294">Příjemci metrik, jako jsou hello grafů, portálu Azure a výstrah funkce, použijte counterSpecifier jako hello "klíč", který identifikuje metriky nebo instanci metriky.</span><span class="sxs-lookup"><span data-stu-id="550df-294">Consumers of metrics, like hello Azure portal charting and alerting feature, use counterSpecifier as hello "key" that identifies a metric or an instance of a metric.</span></span> <span data-ttu-id="550df-295">Pro `builtin` metriky, doporučujeme použít counterSpecifier hodnoty, které začínají `/builtin/`.</span><span class="sxs-lookup"><span data-stu-id="550df-295">For `builtin` metrics, we recommend you use counterSpecifier values that begin with `/builtin/`.</span></span> <span data-ttu-id="550df-296">Pokud shromažďujete konkrétní instanci metriky, doporučujeme, abyste že připojení hello identifikátor hello instance toohello counterSpecifier hodnoty.</span><span class="sxs-lookup"><span data-stu-id="550df-296">If you are collecting a specific instance of a metric, we recommend you attach hello identifier of hello instance toohello counterSpecifier value.</span></span> <span data-ttu-id="550df-297">Příklady:</span><span class="sxs-lookup"><span data-stu-id="550df-297">Some examples:</span></span>

* <span data-ttu-id="550df-298">`/builtin/Processor/PercentIdleTime`-Doba nečinnosti, po průměrem všech jader</span><span class="sxs-lookup"><span data-stu-id="550df-298">`/builtin/Processor/PercentIdleTime` - Idle time averaged across all cores</span></span>
* <span data-ttu-id="550df-299">`/builtin/Disk/FreeSpace(/mnt)`-Volného místa pro systém souborů /mnt hello</span><span class="sxs-lookup"><span data-stu-id="550df-299">`/builtin/Disk/FreeSpace(/mnt)` - Free space for hello /mnt filesystem</span></span>
* <span data-ttu-id="550df-300">`/builtin/Disk/FreeSpace`-Volného místa průměrem všech připojené systémy</span><span class="sxs-lookup"><span data-stu-id="550df-300">`/builtin/Disk/FreeSpace` - Free space averaged across all mounted filesystems</span></span>

<span data-ttu-id="550df-301">LAD ani hello portál Azure očekává hello counterSpecifier hodnotu toomatch žádnému vzoru.</span><span class="sxs-lookup"><span data-stu-id="550df-301">Neither LAD nor hello Azure portal expects hello counterSpecifier value toomatch any pattern.</span></span> <span data-ttu-id="550df-302">Být konzistentní v tom, jak vytvořit counterSpecifier hodnoty.</span><span class="sxs-lookup"><span data-stu-id="550df-302">Be consistent in how you construct counterSpecifier values.</span></span>

<span data-ttu-id="550df-303">Pokud zadáte `performanceCounters`, LAD vždy zapíše data tooa tabulky v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="550df-303">When you specify `performanceCounters`, LAD always writes data tooa table in Azure storage.</span></span> <span data-ttu-id="550df-304">Vám může mít hello stejná data zapsaná tooJSON objekty BLOB a/nebo Event Hubs, ale nelze zakázat ukládání dat tooa tabulky.</span><span class="sxs-lookup"><span data-stu-id="550df-304">You can have hello same data written tooJSON blobs and/or Event Hubs, but you cannot disable storing data tooa table.</span></span> <span data-ttu-id="550df-305">Všechny instance hello nakonfigurované rozšíření diagnostiky toouse hello stejný účet úložiště, název a koncový bod přidat jejich metriky a protokoly toohello stejné tabulky.</span><span class="sxs-lookup"><span data-stu-id="550df-305">All instances of hello diagnostic extension configured toouse hello same storage account name and endpoint add their metrics and logs toohello same table.</span></span> <span data-ttu-id="550df-306">Pokud jsou příliš mnoho virtuálních počítačů zápis toohello, který může omezit stejného oddílu tabulky, Azure zapíše toothat oddílu.</span><span class="sxs-lookup"><span data-stu-id="550df-306">If too many VMs are writing toohello same table partition, Azure can throttle writes toothat partition.</span></span> <span data-ttu-id="550df-307">nastavení příčiny položky toobe eventVolume Hello rozloženy 1 (malá), 10 (střední) nebo 100 různých oddílů (velká).</span><span class="sxs-lookup"><span data-stu-id="550df-307">hello eventVolume setting causes entries toobe spread across 1 (Small), 10 (Medium), or 100 (Large) different partitions.</span></span> <span data-ttu-id="550df-308">"Střední" je obvykle dostatečná není omezen tooensure provoz.</span><span class="sxs-lookup"><span data-stu-id="550df-308">Usually, "Medium" is sufficient tooensure traffic is not throttled.</span></span> <span data-ttu-id="550df-309">Funkce Azure metriky Hello hello portál Azure používá hello data v této tabulce tooproduce grafy nebo tootrigger výstrahy.</span><span class="sxs-lookup"><span data-stu-id="550df-309">hello Azure Metrics feature of hello Azure portal uses hello data in this table tooproduce graphs or tootrigger alerts.</span></span> <span data-ttu-id="550df-310">Název tabulky Hello je hello zřetězení tyto řetězce:</span><span class="sxs-lookup"><span data-stu-id="550df-310">hello table name is hello concatenation of these strings:</span></span>

* `WADMetrics`
* <span data-ttu-id="550df-311">Hello "scheduledTransferPeriod" pro hello agregovat hodnoty, které jsou uložené v tabulce hello</span><span class="sxs-lookup"><span data-stu-id="550df-311">hello "scheduledTransferPeriod" for hello aggregated values stored in hello table</span></span>
* `P10DV2S`
* <span data-ttu-id="550df-312">Datum, v podobě hello "RRRRMMDD", který změní každých 10 dní</span><span class="sxs-lookup"><span data-stu-id="550df-312">A date, in hello form "YYYYMMDD", which changes every 10 days</span></span>

<span data-ttu-id="550df-313">Mezi příklady patří `WADMetricsPT1HP10DV2S20170410` a `WADMetricsPT1MP10DV2S20170609`.</span><span class="sxs-lookup"><span data-stu-id="550df-313">Examples include `WADMetricsPT1HP10DV2S20170410` and `WADMetricsPT1MP10DV2S20170609`.</span></span>

#### <a name="syslogevents"></a><span data-ttu-id="550df-314">syslogEvents</span><span class="sxs-lookup"><span data-stu-id="550df-314">syslogEvents</span></span>

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

<span data-ttu-id="550df-315">Tento volitelný oddíl ovládací prvky kolekce hello protokolu událostí z syslog.</span><span class="sxs-lookup"><span data-stu-id="550df-315">This optional section controls hello collection of log events from syslog.</span></span> <span data-ttu-id="550df-316">Pokud je vynechán části hello, nejsou vůbec zachytit události procesu syslog.</span><span class="sxs-lookup"><span data-stu-id="550df-316">If hello section is omitted, syslog events are not captured at all.</span></span>

<span data-ttu-id="550df-317">kolekce syslogEventConfiguration Hello má jeden záznam pro každý mechanismus syslog, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="550df-317">hello syslogEventConfiguration collection has one entry for each syslog facility of interest.</span></span> <span data-ttu-id="550df-318">Pokud minSeverity je "Žádný" pro konkrétní zařízení, nebo pokud tohoto zařízení nezobrazí v elementu hello vůbec, jsou zachyceny žádné události z tohoto zařízení.</span><span class="sxs-lookup"><span data-stu-id="550df-318">If minSeverity is "NONE" for a particular facility, or if that facility does not appear in hello element at all, no events from that facility are captured.</span></span>

<span data-ttu-id="550df-319">Element</span><span class="sxs-lookup"><span data-stu-id="550df-319">Element</span></span> | <span data-ttu-id="550df-320">Hodnota</span><span class="sxs-lookup"><span data-stu-id="550df-320">Value</span></span>
------- | -----
<span data-ttu-id="550df-321">jímky</span><span class="sxs-lookup"><span data-stu-id="550df-321">sinks</span></span> | <span data-ttu-id="550df-322">Čárkami oddělený seznam názvy jímky toowhich jednotlivých protokolů, které se události publikují.</span><span class="sxs-lookup"><span data-stu-id="550df-322">A comma-separated list of names of sinks toowhich individual log events are published.</span></span> <span data-ttu-id="550df-323">Všechny události protokolu odpovídající omezení hello v syslogEventConfiguration jsou publikované tooeach uvedené jímky.</span><span class="sxs-lookup"><span data-stu-id="550df-323">All log events matching hello restrictions in syslogEventConfiguration are published tooeach listed sink.</span></span> <span data-ttu-id="550df-324">Příklad: "EHforsyslog"</span><span class="sxs-lookup"><span data-stu-id="550df-324">Example: "EHforsyslog"</span></span>
<span data-ttu-id="550df-325">facilityName</span><span class="sxs-lookup"><span data-stu-id="550df-325">facilityName</span></span> | <span data-ttu-id="550df-326">Název zařízení syslog (například "protokolu\_uživatele" nebo "protokolu\_LOCAL0").</span><span class="sxs-lookup"><span data-stu-id="550df-326">A syslog facility name (such as "LOG\_USER" or "LOG\_LOCAL0").</span></span> <span data-ttu-id="550df-327">Najdete v části "zařízení" hello hello [syslog man stránky](http://man7.org/linux/man-pages/man3/syslog.3.html) pro úplný seznam hello.</span><span class="sxs-lookup"><span data-stu-id="550df-327">See hello "facility" section of hello [syslog man page](http://man7.org/linux/man-pages/man3/syslog.3.html) for hello full list.</span></span>
<span data-ttu-id="550df-328">minSeverity</span><span class="sxs-lookup"><span data-stu-id="550df-328">minSeverity</span></span> | <span data-ttu-id="550df-329">Úroveň závažnosti syslog (například "protokolu\_chyba" nebo "protokolu\_informace").</span><span class="sxs-lookup"><span data-stu-id="550df-329">A syslog severity level (such as "LOG\_ERR" or "LOG\_INFO").</span></span> <span data-ttu-id="550df-330">Najdete v části "úroveň" hello hello [syslog man stránky](http://man7.org/linux/man-pages/man3/syslog.3.html) pro úplný seznam hello.</span><span class="sxs-lookup"><span data-stu-id="550df-330">See hello "level" section of hello [syslog man page](http://man7.org/linux/man-pages/man3/syslog.3.html) for hello full list.</span></span> <span data-ttu-id="550df-331">rozšíření Hello zaznamená zařízení toohello událostí odeslaných na nebo výše hello zadané úrovně.</span><span class="sxs-lookup"><span data-stu-id="550df-331">hello extension captures events sent toohello facility at or above hello specified level.</span></span>

<span data-ttu-id="550df-332">Pokud zadáte `syslogEvents`, LAD vždy zapíše data tooa tabulky v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="550df-332">When you specify `syslogEvents`, LAD always writes data tooa table in Azure storage.</span></span> <span data-ttu-id="550df-333">Vám může mít hello stejná data zapsaná tooJSON objekty BLOB a/nebo Event Hubs, ale nelze zakázat ukládání dat tooa tabulky.</span><span class="sxs-lookup"><span data-stu-id="550df-333">You can have hello same data written tooJSON blobs and/or Event Hubs, but you cannot disable storing data tooa table.</span></span> <span data-ttu-id="550df-334">Hello dělení chování pro tuto tabulku je hello stejné popsané pro `performanceCounters`.</span><span class="sxs-lookup"><span data-stu-id="550df-334">hello partitioning behavior for this table is hello same as described for `performanceCounters`.</span></span> <span data-ttu-id="550df-335">Název tabulky Hello je hello zřetězení tyto řetězce:</span><span class="sxs-lookup"><span data-stu-id="550df-335">hello table name is hello concatenation of these strings:</span></span>

* `LinuxSyslog`
* <span data-ttu-id="550df-336">Datum, v podobě hello "RRRRMMDD", který změní každých 10 dní</span><span class="sxs-lookup"><span data-stu-id="550df-336">A date, in hello form "YYYYMMDD", which changes every 10 days</span></span>

<span data-ttu-id="550df-337">Mezi příklady patří `LinuxSyslog20170410` a `LinuxSyslog20170609`.</span><span class="sxs-lookup"><span data-stu-id="550df-337">Examples include `LinuxSyslog20170410` and `LinuxSyslog20170609`.</span></span>

### <a name="perfcfg"></a><span data-ttu-id="550df-338">perfCfg</span><span class="sxs-lookup"><span data-stu-id="550df-338">perfCfg</span></span>

<span data-ttu-id="550df-339">Spuštění libovolné ovládací prvky této volitelné části [OMI](https://github.com/Microsoft/omi) dotazy.</span><span class="sxs-lookup"><span data-stu-id="550df-339">This optional section controls execution of arbitrary [OMI](https://github.com/Microsoft/omi) queries.</span></span>

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

<span data-ttu-id="550df-340">Element</span><span class="sxs-lookup"><span data-stu-id="550df-340">Element</span></span> | <span data-ttu-id="550df-341">Hodnota</span><span class="sxs-lookup"><span data-stu-id="550df-341">Value</span></span>
------- | -----
<span data-ttu-id="550df-342">obor názvů</span><span class="sxs-lookup"><span data-stu-id="550df-342">namespace</span></span> | <span data-ttu-id="550df-343">obor názvů OMI hello (volitelné) v rámci které hello se má provést dotaz.</span><span class="sxs-lookup"><span data-stu-id="550df-343">(optional) hello OMI namespace within which hello query should be executed.</span></span> <span data-ttu-id="550df-344">Pokud tento parametr nezadáte, hello výchozí hodnota je "kořenový/scx", implementované hello [System Center a platformy zprostředkovatelé](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).</span><span class="sxs-lookup"><span data-stu-id="550df-344">If unspecified, hello default value is "root/scx", implemented by hello [System Center Cross-platform Providers](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).</span></span>
<span data-ttu-id="550df-345">query</span><span class="sxs-lookup"><span data-stu-id="550df-345">query</span></span> | <span data-ttu-id="550df-346">Hello OMI toobe dotaz spustit.</span><span class="sxs-lookup"><span data-stu-id="550df-346">hello OMI query toobe executed.</span></span>
<span data-ttu-id="550df-347">Tabulka</span><span class="sxs-lookup"><span data-stu-id="550df-347">table</span></span> | <span data-ttu-id="550df-348">(volitelné) hello tabulky úložiště Azure, v hello určený účet úložiště (viz [chráněné nastavení](#protected-settings)).</span><span class="sxs-lookup"><span data-stu-id="550df-348">(optional) hello Azure storage table, in hello designated storage account (see [Protected settings](#protected-settings)).</span></span>
<span data-ttu-id="550df-349">frequency</span><span class="sxs-lookup"><span data-stu-id="550df-349">frequency</span></span> | <span data-ttu-id="550df-350">(volitelné) hello počet sekund mezi spuštění dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="550df-350">(optional) hello number of seconds between execution of hello query.</span></span> <span data-ttu-id="550df-351">Výchozí hodnota je 300 (5 minut); minimální hodnota je 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="550df-351">Default value is 300 (5 minutes); minimum value is 15 seconds.</span></span>
<span data-ttu-id="550df-352">jímky</span><span class="sxs-lookup"><span data-stu-id="550df-352">sinks</span></span> | <span data-ttu-id="550df-353">(volitelné) Čárkami oddělený seznam názvů další jímky toowhich nezpracovaná ukázka metriky výsledků je nutné ji publikovat.</span><span class="sxs-lookup"><span data-stu-id="550df-353">(optional) A comma-separated list of names of additional sinks toowhich raw sample metric results should be published.</span></span> <span data-ttu-id="550df-354">Žádné agregace ukázek nezpracovaná se počítá pomocí přípony hello nebo metrik Azure.</span><span class="sxs-lookup"><span data-stu-id="550df-354">No aggregation of these raw samples is computed by hello extension or by Azure Metrics.</span></span>

<span data-ttu-id="550df-355">Musí být zadán buď "tabulka" nebo "jímky" nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="550df-355">Either "table" or "sinks", or both, must be specified.</span></span>

### <a name="filelogs"></a><span data-ttu-id="550df-356">fileLogs</span><span class="sxs-lookup"><span data-stu-id="550df-356">fileLogs</span></span>

<span data-ttu-id="550df-357">Ovládací prvky hello zaznamenání souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="550df-357">Controls hello capture of log files.</span></span> <span data-ttu-id="550df-358">LAD zaznamená nové řádky textu, jako jsou zapsané toohello soubor a zapíše tootable řádků nebo všechny zadané jímky (JsonBlob nebo EventHub).</span><span class="sxs-lookup"><span data-stu-id="550df-358">LAD captures new text lines as they are written toohello file and writes them tootable rows and/or any specified sinks (JsonBlob or EventHub).</span></span>

```json
"fileLogs": [
    {
        "file": "/var/log/mydaemonlog",
        "table": "MyDaemonEvents",
        "sinks": ""
    }
]
```

<span data-ttu-id="550df-359">Element</span><span class="sxs-lookup"><span data-stu-id="550df-359">Element</span></span> | <span data-ttu-id="550df-360">Hodnota</span><span class="sxs-lookup"><span data-stu-id="550df-360">Value</span></span>
------- | -----
<span data-ttu-id="550df-361">Soubor</span><span class="sxs-lookup"><span data-stu-id="550df-361">file</span></span> | <span data-ttu-id="550df-362">Úplná cesta toobe souboru protokolu hello Hello sledovaná a zachycení.</span><span class="sxs-lookup"><span data-stu-id="550df-362">hello full pathname of hello log file toobe watched and captured.</span></span> <span data-ttu-id="550df-363">Název musí být Hello pathname jeden soubor; nemůže název adresáře nebo obsahovat zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="550df-363">hello pathname must name a single file; it cannot name a directory or contain wildcards.</span></span>
<span data-ttu-id="550df-364">Tabulka</span><span class="sxs-lookup"><span data-stu-id="550df-364">table</span></span> | <span data-ttu-id="550df-365">tabulky (volitelné) hello úložiště Azure, v úložišti hello určené účtu (jako je zadaný v konfiguraci hello chráněné), do které nové řádky z hello "tail" hello souboru se zapisují.</span><span class="sxs-lookup"><span data-stu-id="550df-365">(optional) hello Azure storage table, in hello designated storage account (as specified in hello protected configuration), into which new lines from hello "tail" of hello file are written.</span></span>
<span data-ttu-id="550df-366">jímky</span><span class="sxs-lookup"><span data-stu-id="550df-366">sinks</span></span> | <span data-ttu-id="550df-367">(volitelné) Čárkami oddělený seznam názvů další jímky toowhich protokolu řádky odeslána.</span><span class="sxs-lookup"><span data-stu-id="550df-367">(optional) A comma-separated list of names of additional sinks toowhich log lines sent.</span></span>

<span data-ttu-id="550df-368">Musí být zadán buď "tabulka" nebo "jímky" nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="550df-368">Either "table" or "sinks", or both, must be specified.</span></span>

## <a name="metrics-supported-by-hello-builtin-provider"></a><span data-ttu-id="550df-369">Metriky podporované poskytovatelem builtin hello</span><span class="sxs-lookup"><span data-stu-id="550df-369">Metrics supported by hello builtin provider</span></span>

<span data-ttu-id="550df-370">Zprostředkovatel metriky builtin Hello je zdroj metriky nejvíce zajímavé tooa širokou škálu uživatele.</span><span class="sxs-lookup"><span data-stu-id="550df-370">hello builtin metric provider is a source of metrics most interesting tooa broad set of users.</span></span> <span data-ttu-id="550df-371">Tyto metriky spadají do pěti široký třídy:</span><span class="sxs-lookup"><span data-stu-id="550df-371">These metrics fall into five broad classes:</span></span>

* <span data-ttu-id="550df-372">Procesor</span><span class="sxs-lookup"><span data-stu-id="550df-372">Processor</span></span>
* <span data-ttu-id="550df-373">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="550df-373">Memory</span></span>
* <span data-ttu-id="550df-374">Síť</span><span class="sxs-lookup"><span data-stu-id="550df-374">Network</span></span>
* <span data-ttu-id="550df-375">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="550df-375">Filesystem</span></span>
* <span data-ttu-id="550df-376">Disk</span><span class="sxs-lookup"><span data-stu-id="550df-376">Disk</span></span>

### <a name="builtin-metrics-for-hello-processor-class"></a><span data-ttu-id="550df-377">předdefinované metriky pro hello procesoru – třída</span><span class="sxs-lookup"><span data-stu-id="550df-377">builtin metrics for hello Processor class</span></span>

<span data-ttu-id="550df-378">Hello procesoru třída metrik poskytuje informace o využití procesoru v hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="550df-378">hello Processor class of metrics provides information about processor usage in hello VM.</span></span> <span data-ttu-id="550df-379">Při agregování procenta, výsledkem hello je hello průměr mezi všechny procesory.</span><span class="sxs-lookup"><span data-stu-id="550df-379">When aggregating percentages, hello result is hello average across all CPUs.</span></span> <span data-ttu-id="550df-380">V dva základní virtuální počítač Pokud jednoho jádra bylo 100 % zaneprázdněn a hello dalších 100 % nečinnosti, hello uvádět, že PercentIdleTime by bylo 50.</span><span class="sxs-lookup"><span data-stu-id="550df-380">In a two core VM, if one core was 100% busy and hello other was 100% idle, hello reported PercentIdleTime would be 50.</span></span> <span data-ttu-id="550df-381">Pokud byl 50 % zaneprázdněn pro každý základní hello stejné období, hello hlášené výsledek by také bylo 50.</span><span class="sxs-lookup"><span data-stu-id="550df-381">If each core was 50% busy for hello same period, hello reported result would also be 50.</span></span> <span data-ttu-id="550df-382">V čtyři základní virtuální počítač, pomocí jednoho jádra 100 % zaneprázdněn a hello ostatní nečinnosti, hello oznámilo že percentidletime by 75.</span><span class="sxs-lookup"><span data-stu-id="550df-382">In a four core VM, with one core 100% busy and hello others idle, hello reported PercentIdleTime would be 75.</span></span>

<span data-ttu-id="550df-383">Čítač</span><span class="sxs-lookup"><span data-stu-id="550df-383">counter</span></span> | <span data-ttu-id="550df-384">Význam</span><span class="sxs-lookup"><span data-stu-id="550df-384">Meaning</span></span>
------- | -------
<span data-ttu-id="550df-385">PercentIdleTime</span><span class="sxs-lookup"><span data-stu-id="550df-385">PercentIdleTime</span></span> | <span data-ttu-id="550df-386">Procento času období hello agregace, procesory byly provádění hello jádra nečinné smyčky</span><span class="sxs-lookup"><span data-stu-id="550df-386">Percentage of time during hello aggregation window that processors were executing hello kernel idle loop</span></span>
<span data-ttu-id="550df-387">percentProcessorTime</span><span class="sxs-lookup"><span data-stu-id="550df-387">PercentProcessorTime</span></span> | <span data-ttu-id="550df-388">Procento doby provádění jiných než nečinných vláken</span><span class="sxs-lookup"><span data-stu-id="550df-388">Percentage of time executing a non-idle thread</span></span>
<span data-ttu-id="550df-389">PercentIOWaitTime</span><span class="sxs-lookup"><span data-stu-id="550df-389">PercentIOWaitTime</span></span> | <span data-ttu-id="550df-390">Procento doby čekání na vstupně-výstupní operace toocomplete</span><span class="sxs-lookup"><span data-stu-id="550df-390">Percentage of time waiting for IO operations toocomplete</span></span>
<span data-ttu-id="550df-391">PercentInterruptTime</span><span class="sxs-lookup"><span data-stu-id="550df-391">PercentInterruptTime</span></span> | <span data-ttu-id="550df-392">Procento doby provádění hardware a software přerušení a DPC (odložených volání procedur)</span><span class="sxs-lookup"><span data-stu-id="550df-392">Percentage of time executing hardware/software interrupts and DPCs (deferred procedure calls)</span></span>
<span data-ttu-id="550df-393">PercentUserTime</span><span class="sxs-lookup"><span data-stu-id="550df-393">PercentUserTime</span></span> | <span data-ttu-id="550df-394">Jiných než nečinných času období agregace hello hello procento času stráveného v uživatele více se střední důležitostí</span><span class="sxs-lookup"><span data-stu-id="550df-394">Of non-idle time during hello aggregation window, hello percentage of time spent in user more at normal priority</span></span>
<span data-ttu-id="550df-395">PercentNiceTime</span><span class="sxs-lookup"><span data-stu-id="550df-395">PercentNiceTime</span></span> | <span data-ttu-id="550df-396">Jiných než nečinných času stráví hello procento snížený (dobrý) důležitostí</span><span class="sxs-lookup"><span data-stu-id="550df-396">Of non-idle time, hello percentage spent at lowered (nice) priority</span></span>
<span data-ttu-id="550df-397">PercentPrivilegedTime</span><span class="sxs-lookup"><span data-stu-id="550df-397">PercentPrivilegedTime</span></span> | <span data-ttu-id="550df-398">Jiných než nečinných času stráví hello procento v privilegovaném režimu režimu (jádra)</span><span class="sxs-lookup"><span data-stu-id="550df-398">Of non-idle time, hello percentage spent in privileged (kernel) mode</span></span>

<span data-ttu-id="550df-399">Hello první čtyři čítače by měl součet too100 %.</span><span class="sxs-lookup"><span data-stu-id="550df-399">hello first four counters should sum too100%.</span></span> <span data-ttu-id="550df-400">Hello poslední tři čítače také součet too100 %; jejich rozdělit hello součet PercentProcessorTime, PercentIOWaitTime a PercentInterruptTime.</span><span class="sxs-lookup"><span data-stu-id="550df-400">hello last three counters also sum too100%; they subdivide hello sum of PercentProcessorTime, PercentIOWaitTime, and PercentInterruptTime.</span></span>

<span data-ttu-id="550df-401">nastavit tooobtain jednu metriku agregovat do všech procesorů `"condition": "IsAggregate=TRUE"`.</span><span class="sxs-lookup"><span data-stu-id="550df-401">tooobtain a single metric aggregated across all processors, set `"condition": "IsAggregate=TRUE"`.</span></span> <span data-ttu-id="550df-402">tooobtain metriky pro specifické procesory, například hello druhý logický procesor čtyři základní virtuální počítač, nastavte `"condition": "Name=\\"1\\""`.</span><span class="sxs-lookup"><span data-stu-id="550df-402">tooobtain a metric for a specific processor, such as hello second logical processor of a four core VM, set `"condition": "Name=\\"1\\""`.</span></span> <span data-ttu-id="550df-403">Logický procesor čísla jsou v rozsahu hello `[0..n-1]`.</span><span class="sxs-lookup"><span data-stu-id="550df-403">Logical processor numbers are in hello range `[0..n-1]`.</span></span>

### <a name="builtin-metrics-for-hello-memory-class"></a><span data-ttu-id="550df-404">předdefinované metriky pro hello paměti – třída</span><span class="sxs-lookup"><span data-stu-id="550df-404">builtin metrics for hello Memory class</span></span>

<span data-ttu-id="550df-405">Hello paměti třída metrik poskytuje informace o využití paměti, stránkování a vzájemná záměna.</span><span class="sxs-lookup"><span data-stu-id="550df-405">hello Memory class of metrics provides information about memory utilization, paging, and swapping.</span></span>

<span data-ttu-id="550df-406">Čítač</span><span class="sxs-lookup"><span data-stu-id="550df-406">counter</span></span> | <span data-ttu-id="550df-407">Význam</span><span class="sxs-lookup"><span data-stu-id="550df-407">Meaning</span></span>
------- | -------
<span data-ttu-id="550df-408">AvailableMemory</span><span class="sxs-lookup"><span data-stu-id="550df-408">AvailableMemory</span></span> | <span data-ttu-id="550df-409">Dostupná fyzická paměť v MiB</span><span class="sxs-lookup"><span data-stu-id="550df-409">Available physical memory in MiB</span></span>
<span data-ttu-id="550df-410">PercentAvailableMemory</span><span class="sxs-lookup"><span data-stu-id="550df-410">PercentAvailableMemory</span></span> | <span data-ttu-id="550df-411">Dostupná fyzická paměť v procentech celkové paměti</span><span class="sxs-lookup"><span data-stu-id="550df-411">Available physical memory as a percent of total memory</span></span>
<span data-ttu-id="550df-412">UsedMemory</span><span class="sxs-lookup"><span data-stu-id="550df-412">UsedMemory</span></span> | <span data-ttu-id="550df-413">Použití fyzické paměti (MiB)</span><span class="sxs-lookup"><span data-stu-id="550df-413">In-use physical memory (MiB)</span></span>
<span data-ttu-id="550df-414">PercentUsedMemory</span><span class="sxs-lookup"><span data-stu-id="550df-414">PercentUsedMemory</span></span> | <span data-ttu-id="550df-415">Fyzické paměti používané jako procento celkové paměti</span><span class="sxs-lookup"><span data-stu-id="550df-415">In-use physical memory as a percent of total memory</span></span>
<span data-ttu-id="550df-416">PagesPerSec</span><span class="sxs-lookup"><span data-stu-id="550df-416">PagesPerSec</span></span> | <span data-ttu-id="550df-417">Celkový počet stránkování (čtení a zápis)</span><span class="sxs-lookup"><span data-stu-id="550df-417">Total paging (read/write)</span></span>
<span data-ttu-id="550df-418">PagesReadPerSec</span><span class="sxs-lookup"><span data-stu-id="550df-418">PagesReadPerSec</span></span> | <span data-ttu-id="550df-419">Čtení stránek ze záložního úložiště (odkládacího souboru, soubor programu, mapovaný soubor, atd.)</span><span class="sxs-lookup"><span data-stu-id="550df-419">Pages read from backing store (swap file, program file, mapped file, etc.)</span></span>
<span data-ttu-id="550df-420">PagesWrittenPerSec</span><span class="sxs-lookup"><span data-stu-id="550df-420">PagesWrittenPerSec</span></span> | <span data-ttu-id="550df-421">Uložení stránek zapsaných toobacking (odkládací soubor, mapovaný soubor, atd.)</span><span class="sxs-lookup"><span data-stu-id="550df-421">Pages written toobacking store (swap file, mapped file, etc.)</span></span>
<span data-ttu-id="550df-422">AvailableSwap</span><span class="sxs-lookup"><span data-stu-id="550df-422">AvailableSwap</span></span> | <span data-ttu-id="550df-423">Nepoužívané odkládacího prostoru (MiB)</span><span class="sxs-lookup"><span data-stu-id="550df-423">Unused swap space (MiB)</span></span>
<span data-ttu-id="550df-424">PercentAvailableSwap</span><span class="sxs-lookup"><span data-stu-id="550df-424">PercentAvailableSwap</span></span> | <span data-ttu-id="550df-425">Nepoužívané odkládacího souboru jako procentuální hodnotu celkové velikosti odkládacího souboru</span><span class="sxs-lookup"><span data-stu-id="550df-425">Unused swap space as a percentage of total swap</span></span>
<span data-ttu-id="550df-426">UsedSwap</span><span class="sxs-lookup"><span data-stu-id="550df-426">UsedSwap</span></span> | <span data-ttu-id="550df-427">Využití odkládacího souboru (MiB)</span><span class="sxs-lookup"><span data-stu-id="550df-427">In-use swap space (MiB)</span></span>
<span data-ttu-id="550df-428">PercentUsedSwap</span><span class="sxs-lookup"><span data-stu-id="550df-428">PercentUsedSwap</span></span> | <span data-ttu-id="550df-429">Používané místo odkládacího souboru jako procentuální hodnotu celkové velikosti odkládacího souboru</span><span class="sxs-lookup"><span data-stu-id="550df-429">In-use swap space as a percentage of total swap</span></span>

<span data-ttu-id="550df-430">Tato třída metrik obsahuje pouze jednu instanci.</span><span class="sxs-lookup"><span data-stu-id="550df-430">This class of metrics has only a single instance.</span></span> <span data-ttu-id="550df-431">atribut "podmínky" Hello neexistuje užitečné nastavení a by měl být vynechán.</span><span class="sxs-lookup"><span data-stu-id="550df-431">hello "condition" attribute has no useful settings and should be omitted.</span></span>

### <a name="builtin-metrics-for-hello-network-class"></a><span data-ttu-id="550df-432">předdefinované metriky pro hello sítě – třída</span><span class="sxs-lookup"><span data-stu-id="550df-432">builtin metrics for hello Network class</span></span>

<span data-ttu-id="550df-433">Hello sítě třída metrik poskytuje informace o síťové aktivity na rozhraní jednotlivým síťovým od spuštění počítače.</span><span class="sxs-lookup"><span data-stu-id="550df-433">hello Network class of metrics provides information about network activity on an individual network interfaces since boot.</span></span> <span data-ttu-id="550df-434">LAD nevystavuje metriky šířky pásma, které je možné načíst z hostitele metriky.</span><span class="sxs-lookup"><span data-stu-id="550df-434">LAD does not expose bandwidth metrics, which can be retrieved from host metrics.</span></span>

<span data-ttu-id="550df-435">Čítač</span><span class="sxs-lookup"><span data-stu-id="550df-435">counter</span></span> | <span data-ttu-id="550df-436">Význam</span><span class="sxs-lookup"><span data-stu-id="550df-436">Meaning</span></span>
------- | -------
<span data-ttu-id="550df-437">BytesTransmitted</span><span class="sxs-lookup"><span data-stu-id="550df-437">BytesTransmitted</span></span> | <span data-ttu-id="550df-438">Celkový počet bajtů odeslaných od spuštění</span><span class="sxs-lookup"><span data-stu-id="550df-438">Total bytes sent since boot</span></span>
<span data-ttu-id="550df-439">BytesReceived</span><span class="sxs-lookup"><span data-stu-id="550df-439">BytesReceived</span></span> | <span data-ttu-id="550df-440">Celkový počet bajtů přijatých od spuštění</span><span class="sxs-lookup"><span data-stu-id="550df-440">Total bytes received since boot</span></span>
<span data-ttu-id="550df-441">BytesTotal</span><span class="sxs-lookup"><span data-stu-id="550df-441">BytesTotal</span></span> | <span data-ttu-id="550df-442">Celkový počet bajtů odesílané nebo přijímané od spuštění počítače</span><span class="sxs-lookup"><span data-stu-id="550df-442">Total bytes sent or received since boot</span></span>
<span data-ttu-id="550df-443">PacketsTransmitted</span><span class="sxs-lookup"><span data-stu-id="550df-443">PacketsTransmitted</span></span> | <span data-ttu-id="550df-444">Celkový počet paketů odeslaných od spuštění</span><span class="sxs-lookup"><span data-stu-id="550df-444">Total packets sent since boot</span></span>
<span data-ttu-id="550df-445">PacketsReceived</span><span class="sxs-lookup"><span data-stu-id="550df-445">PacketsReceived</span></span> | <span data-ttu-id="550df-446">Celkový počet paketů přijatých od spuštění</span><span class="sxs-lookup"><span data-stu-id="550df-446">Total packets received since boot</span></span>
<span data-ttu-id="550df-447">TotalRxErrors</span><span class="sxs-lookup"><span data-stu-id="550df-447">TotalRxErrors</span></span> | <span data-ttu-id="550df-448">Počet chyb, obdrží od spuštění počítače</span><span class="sxs-lookup"><span data-stu-id="550df-448">Number of receive errors since boot</span></span>
<span data-ttu-id="550df-449">TotalTxErrors</span><span class="sxs-lookup"><span data-stu-id="550df-449">TotalTxErrors</span></span> | <span data-ttu-id="550df-450">Počet chyb přenosu od spuštění počítače</span><span class="sxs-lookup"><span data-stu-id="550df-450">Number of transmit errors since boot</span></span>
<span data-ttu-id="550df-451">TotalCollisions</span><span class="sxs-lookup"><span data-stu-id="550df-451">TotalCollisions</span></span> | <span data-ttu-id="550df-452">Počet kolizí hlášené hello síťové porty od spuštění počítače</span><span class="sxs-lookup"><span data-stu-id="550df-452">Number of collisions reported by hello network ports since boot</span></span>

 <span data-ttu-id="550df-453">I když tato třída je instance, nepodporuje LAD zaznamenávání metriky sítě s agregovat přes všechna síťová zařízení.</span><span class="sxs-lookup"><span data-stu-id="550df-453">Although this class is instanced, LAD does not support capturing Network metrics aggregated across all network devices.</span></span> <span data-ttu-id="550df-454">nastavení metriky hello tooobtain pro určité rozhraní, jako je například eth0, `"condition": "InstanceID=\\"eth0\\""`.</span><span class="sxs-lookup"><span data-stu-id="550df-454">tooobtain hello metrics for a specific interface, such as eth0, set `"condition": "InstanceID=\\"eth0\\""`.</span></span>

### <a name="builtin-metrics-for-hello-filesystem-class"></a><span data-ttu-id="550df-455">předdefinované metriky pro hello Filesystem – třída</span><span class="sxs-lookup"><span data-stu-id="550df-455">builtin metrics for hello Filesystem class</span></span>

<span data-ttu-id="550df-456">Hello třída Filesystem metrik poskytuje informace o použití systému souborů.</span><span class="sxs-lookup"><span data-stu-id="550df-456">hello Filesystem class of metrics provides information about filesystem usage.</span></span> <span data-ttu-id="550df-457">Absolutní a procentuální hodnoty jsou hlášeny, jako by byly zobrazené tooan obyčejnou uživatel (není uživatel root).</span><span class="sxs-lookup"><span data-stu-id="550df-457">Absolute and percentage values are reported as they'd be displayed tooan ordinary user (not root).</span></span>

<span data-ttu-id="550df-458">Čítač</span><span class="sxs-lookup"><span data-stu-id="550df-458">counter</span></span> | <span data-ttu-id="550df-459">Význam</span><span class="sxs-lookup"><span data-stu-id="550df-459">Meaning</span></span>
------- | -------
<span data-ttu-id="550df-460">FreeSpace</span><span class="sxs-lookup"><span data-stu-id="550df-460">FreeSpace</span></span> | <span data-ttu-id="550df-461">Dostupné místo na disku v bajtech</span><span class="sxs-lookup"><span data-stu-id="550df-461">Available disk space in bytes</span></span>
<span data-ttu-id="550df-462">UsedSpace</span><span class="sxs-lookup"><span data-stu-id="550df-462">UsedSpace</span></span> | <span data-ttu-id="550df-463">Využité místo na disku v bajtech</span><span class="sxs-lookup"><span data-stu-id="550df-463">Used disk space in bytes</span></span>
<span data-ttu-id="550df-464">PercentFreeSpace</span><span class="sxs-lookup"><span data-stu-id="550df-464">PercentFreeSpace</span></span> | <span data-ttu-id="550df-465">Procento volného místa</span><span class="sxs-lookup"><span data-stu-id="550df-465">Percentage free space</span></span>
<span data-ttu-id="550df-466">PercentUsedSpace</span><span class="sxs-lookup"><span data-stu-id="550df-466">PercentUsedSpace</span></span> | <span data-ttu-id="550df-467">Procento využitého prostoru</span><span class="sxs-lookup"><span data-stu-id="550df-467">Percentage used space</span></span>
<span data-ttu-id="550df-468">PercentFreeInodes</span><span class="sxs-lookup"><span data-stu-id="550df-468">PercentFreeInodes</span></span> | <span data-ttu-id="550df-469">Procentuální hodnota nepoužívané uzlů Inode</span><span class="sxs-lookup"><span data-stu-id="550df-469">Percentage of unused inodes</span></span>
<span data-ttu-id="550df-470">PercentUsedInodes</span><span class="sxs-lookup"><span data-stu-id="550df-470">PercentUsedInodes</span></span> | <span data-ttu-id="550df-471">Procentuální hodnota přidělené (používán) uzlů Inode sčítají mezi všechny systémy</span><span class="sxs-lookup"><span data-stu-id="550df-471">Percentage of allocated (in use) inodes summed across all filesystems</span></span>
<span data-ttu-id="550df-472">BytesReadPerSecond</span><span class="sxs-lookup"><span data-stu-id="550df-472">BytesReadPerSecond</span></span> | <span data-ttu-id="550df-473">Načíst počet bajtů za sekundu</span><span class="sxs-lookup"><span data-stu-id="550df-473">Bytes read per second</span></span>
<span data-ttu-id="550df-474">BytesWrittenPerSecond</span><span class="sxs-lookup"><span data-stu-id="550df-474">BytesWrittenPerSecond</span></span> | <span data-ttu-id="550df-475">Bajtů zapsaných za sekundu</span><span class="sxs-lookup"><span data-stu-id="550df-475">Bytes written per second</span></span>
<span data-ttu-id="550df-476">BytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="550df-476">BytesPerSecond</span></span> | <span data-ttu-id="550df-477">Bajtů číst nebo zapisovat za sekundu</span><span class="sxs-lookup"><span data-stu-id="550df-477">Bytes read or written per second</span></span>
<span data-ttu-id="550df-478">ReadsPerSecond</span><span class="sxs-lookup"><span data-stu-id="550df-478">ReadsPerSecond</span></span> | <span data-ttu-id="550df-479">Operace čtení za sekundu</span><span class="sxs-lookup"><span data-stu-id="550df-479">Read operations per second</span></span>
<span data-ttu-id="550df-480">WritesPerSecond</span><span class="sxs-lookup"><span data-stu-id="550df-480">WritesPerSecond</span></span> | <span data-ttu-id="550df-481">Zápis operací za sekundu</span><span class="sxs-lookup"><span data-stu-id="550df-481">Write operations per second</span></span>
<span data-ttu-id="550df-482">TransfersPerSecond</span><span class="sxs-lookup"><span data-stu-id="550df-482">TransfersPerSecond</span></span> | <span data-ttu-id="550df-483">Operace čtení nebo zápisu za sekundu</span><span class="sxs-lookup"><span data-stu-id="550df-483">Read or write operations per second</span></span>

<span data-ttu-id="550df-484">Agregované hodnoty mezi všechny systémy souborů je možné získat nastavení `"condition": "IsAggregate=True"`.</span><span class="sxs-lookup"><span data-stu-id="550df-484">Aggregated values across all file systems can be obtained by setting `"condition": "IsAggregate=True"`.</span></span> <span data-ttu-id="550df-485">Hodnoty pro konkrétní připojeného souboru systém, například "/ mnt", je možné získat nastavení `"condition": 'Name="/mnt"'`.</span><span class="sxs-lookup"><span data-stu-id="550df-485">Values for a specific mounted file system, such as "/mnt", can be obtained by setting `"condition": 'Name="/mnt"'`.</span></span>

### <a name="builtin-metrics-for-hello-disk-class"></a><span data-ttu-id="550df-486">předdefinované metriky pro hello disku – třída</span><span class="sxs-lookup"><span data-stu-id="550df-486">builtin metrics for hello Disk class</span></span>

<span data-ttu-id="550df-487">Hello disku třída metrik poskytuje informace o využití disku zařízení.</span><span class="sxs-lookup"><span data-stu-id="550df-487">hello Disk class of metrics provides information about disk device usage.</span></span> <span data-ttu-id="550df-488">Ve statistikách použít toohello celé jednotky.</span><span class="sxs-lookup"><span data-stu-id="550df-488">These statistics apply toohello entire drive.</span></span> <span data-ttu-id="550df-489">Pokud existují více systémy souborů na zařízení, hello čítače pro toto zařízení jsou, efektivně, agregovat přes všechny z nich.</span><span class="sxs-lookup"><span data-stu-id="550df-489">If there are multiple file systems on a device, hello counters for that device are, effectively, aggregated across all of them.</span></span>

<span data-ttu-id="550df-490">Čítač</span><span class="sxs-lookup"><span data-stu-id="550df-490">counter</span></span> | <span data-ttu-id="550df-491">Význam</span><span class="sxs-lookup"><span data-stu-id="550df-491">Meaning</span></span>
------- | -------
<span data-ttu-id="550df-492">ReadsPerSecond</span><span class="sxs-lookup"><span data-stu-id="550df-492">ReadsPerSecond</span></span> | <span data-ttu-id="550df-493">Operace čtení za sekundu</span><span class="sxs-lookup"><span data-stu-id="550df-493">Read operations per second</span></span>
<span data-ttu-id="550df-494">WritesPerSecond</span><span class="sxs-lookup"><span data-stu-id="550df-494">WritesPerSecond</span></span> | <span data-ttu-id="550df-495">Zápis operací za sekundu</span><span class="sxs-lookup"><span data-stu-id="550df-495">Write operations per second</span></span>
<span data-ttu-id="550df-496">TransfersPerSecond</span><span class="sxs-lookup"><span data-stu-id="550df-496">TransfersPerSecond</span></span> | <span data-ttu-id="550df-497">Celkový počet operací za sekundu</span><span class="sxs-lookup"><span data-stu-id="550df-497">Total operations per second</span></span>
<span data-ttu-id="550df-498">AverageReadTime</span><span class="sxs-lookup"><span data-stu-id="550df-498">AverageReadTime</span></span> | <span data-ttu-id="550df-499">Průměrný počet sekund na operace čtení</span><span class="sxs-lookup"><span data-stu-id="550df-499">Average seconds per read operation</span></span>
<span data-ttu-id="550df-500">AverageWriteTime</span><span class="sxs-lookup"><span data-stu-id="550df-500">AverageWriteTime</span></span> | <span data-ttu-id="550df-501">Průměrný počet sekund na operace zápisu</span><span class="sxs-lookup"><span data-stu-id="550df-501">Average seconds per write operation</span></span>
<span data-ttu-id="550df-502">AverageTransferTime</span><span class="sxs-lookup"><span data-stu-id="550df-502">AverageTransferTime</span></span> | <span data-ttu-id="550df-503">Průměrný počet sekund na operaci</span><span class="sxs-lookup"><span data-stu-id="550df-503">Average seconds per operation</span></span>
<span data-ttu-id="550df-504">AverageDiskQueueLength</span><span class="sxs-lookup"><span data-stu-id="550df-504">AverageDiskQueueLength</span></span> | <span data-ttu-id="550df-505">Průměrný počet operací zařazených do fronty disku</span><span class="sxs-lookup"><span data-stu-id="550df-505">Average number of queued disk operations</span></span>
<span data-ttu-id="550df-506">ReadBytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="550df-506">ReadBytesPerSecond</span></span> | <span data-ttu-id="550df-507">Počet bajtů přečtených za sekundu</span><span class="sxs-lookup"><span data-stu-id="550df-507">Number of bytes read per second</span></span>
<span data-ttu-id="550df-508">WriteBytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="550df-508">WriteBytesPerSecond</span></span> | <span data-ttu-id="550df-509">Počet bajtů zapsaných za sekundu</span><span class="sxs-lookup"><span data-stu-id="550df-509">Number of bytes written per second</span></span>
<span data-ttu-id="550df-510">BytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="550df-510">BytesPerSecond</span></span> | <span data-ttu-id="550df-511">Počet bajtů přečtených nebo zapsaných za sekundu</span><span class="sxs-lookup"><span data-stu-id="550df-511">Number of bytes read or written per second</span></span>

<span data-ttu-id="550df-512">Agregované hodnoty všech disků je možné získat nastavení `"condition": "IsAggregate=True"`.</span><span class="sxs-lookup"><span data-stu-id="550df-512">Aggregated values across all disks can be obtained by setting `"condition": "IsAggregate=True"`.</span></span> <span data-ttu-id="550df-513">nastavit tooget informace pro konkrétní zařízení (například/dev/sdf1), `"condition": "Name=\\"/dev/sdf1\\""`.</span><span class="sxs-lookup"><span data-stu-id="550df-513">tooget information for a specific device (for example, /dev/sdf1), set `"condition": "Name=\\"/dev/sdf1\\""`.</span></span>

## <a name="installing-and-configuring-lad-30-via-cli"></a><span data-ttu-id="550df-514">Instalace a konfigurace LAD 3.0 prostřednictvím rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="550df-514">Installing and configuring LAD 3.0 via CLI</span></span>

<span data-ttu-id="550df-515">Za předpokladu, že jsou chráněný nastavení v souboru hello PrivateConfig.json a veřejné konfigurační informace v PublicConfig.json, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="550df-515">Assuming your protected settings are in hello file PrivateConfig.json and your public configuration information is in PublicConfig.json, run this command:</span></span>

```azurecli
az vm extension set *resource_group_name* *vm_name* LinuxDiagnostic Microsoft.Azure.Diagnostics '3.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json
```

<span data-ttu-id="550df-516">příkaz Hello předpokládá, že používáte hello Správa prostředků Azure režim (arm) hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="550df-516">hello command assumes you are using hello Azure Resource Management mode (arm) of hello Azure CLI.</span></span> <span data-ttu-id="550df-517">tooconfigure LAD pro nasazení classic modelu virtuálních počítačů (ASM), přepínače příliš "asm" režimu (`azure config mode asm`) a název skupiny prostředků hello v příkazu hello vynechat.</span><span class="sxs-lookup"><span data-stu-id="550df-517">tooconfigure LAD for classic deployment model (ASM) VMs, switch too"asm" mode (`azure config mode asm`) and omit hello resource group name in hello command.</span></span> <span data-ttu-id="550df-518">Další informace najdete v tématu hello [dokumentaci rozhraní příkazového řádku a platformy](https://docs.microsoft.com/azure/xplat-cli-connect).</span><span class="sxs-lookup"><span data-stu-id="550df-518">For more information, see hello [cross-platform CLI documentation](https://docs.microsoft.com/azure/xplat-cli-connect).</span></span>

## <a name="an-example-lad-30-configuration"></a><span data-ttu-id="550df-519">Příklad LAD 3.0 konfigurace</span><span class="sxs-lookup"><span data-stu-id="550df-519">An example LAD 3.0 configuration</span></span>

<span data-ttu-id="550df-520">Podle předchozích definice, zde je ukázka konfigurace rozšíření LAD 3.0 s vysvětlení hello.</span><span class="sxs-lookup"><span data-stu-id="550df-520">Based on hello preceding definitions, here's a sample LAD 3.0 extension configuration with some explanation.</span></span> <span data-ttu-id="550df-521">tooapply případě tohohle vzorku tooyour, by měli používat svůj vlastní název účtu úložiště, účet tokenu SAS a tokeny EventHubs SAS.</span><span class="sxs-lookup"><span data-stu-id="550df-521">tooapply this sample tooyour case, you should use your own storage account name, account SAS token, and EventHubs SAS tokens.</span></span>

### <a name="privateconfigjson"></a><span data-ttu-id="550df-522">PrivateConfig.json</span><span class="sxs-lookup"><span data-stu-id="550df-522">PrivateConfig.json</span></span>

<span data-ttu-id="550df-523">Tato nastavení privátní konfigurace:</span><span class="sxs-lookup"><span data-stu-id="550df-523">These private settings configure:</span></span>

* <span data-ttu-id="550df-524">účet úložiště</span><span class="sxs-lookup"><span data-stu-id="550df-524">a storage account</span></span>
* <span data-ttu-id="550df-525">odpovídající tokenu SAS účtu</span><span class="sxs-lookup"><span data-stu-id="550df-525">a matching account SAS token</span></span>
* <span data-ttu-id="550df-526">několik jímky (JsonBlob nebo EventHubs se tokeny SAS)</span><span class="sxs-lookup"><span data-stu-id="550df-526">several sinks (JsonBlob or EventHubs with SAS tokens)</span></span>

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

### <a name="publicconfigjson"></a><span data-ttu-id="550df-527">PublicConfig.json</span><span class="sxs-lookup"><span data-stu-id="550df-527">PublicConfig.json</span></span>

<span data-ttu-id="550df-528">Tato nastavení veřejných způsobit LAD na:</span><span class="sxs-lookup"><span data-stu-id="550df-528">These public settings cause LAD to:</span></span>

* <span data-ttu-id="550df-529">Nahrát procentuální hodnoty času procesoru a použitého místa disku metriky toohello `WADMetrics*` tabulky</span><span class="sxs-lookup"><span data-stu-id="550df-529">Upload percent-processor-time and used-disk-space metrics toohello `WADMetrics*` table</span></span>
* <span data-ttu-id="550df-530">Odeslání zprávy syslog budovy "user" a závažnost "informace" toohello `LinuxSyslog*` tabulky</span><span class="sxs-lookup"><span data-stu-id="550df-530">Upload messages from syslog facility "user" and severity "info" toohello `LinuxSyslog*` table</span></span>
* <span data-ttu-id="550df-531">Nahrát nezpracovaná OMI dotazu výsledky (PercentProcessorTime a PercentIdleTime) toohello s názvem `LinuxCPU` tabulky</span><span class="sxs-lookup"><span data-stu-id="550df-531">Upload raw OMI query results (PercentProcessorTime and PercentIdleTime) toohello named `LinuxCPU` table</span></span>
* <span data-ttu-id="550df-532">Nahrát připojením řádků v souboru `/var/log/myladtestlog` toohello `MyLadTestLog` tabulky</span><span class="sxs-lookup"><span data-stu-id="550df-532">Upload appended lines in file `/var/log/myladtestlog` toohello `MyLadTestLog` table</span></span>

<span data-ttu-id="550df-533">Data jsou v každém případě také odeslána a:</span><span class="sxs-lookup"><span data-stu-id="550df-533">In each case, data is also uploaded to:</span></span>

* <span data-ttu-id="550df-534">Azure Blob storage (název kontejneru je definovaný v hello JsonBlob jímky)</span><span class="sxs-lookup"><span data-stu-id="550df-534">Azure Blob storage (container name is as defined in hello JsonBlob sink)</span></span>
* <span data-ttu-id="550df-535">Koncový bod EventHubs (jak je uvedeno v hello EventHubs jímky)</span><span class="sxs-lookup"><span data-stu-id="550df-535">EventHubs endpoint (as specified in hello EventHubs sink)</span></span>

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

<span data-ttu-id="550df-536">Hello `resourceId` v hello konfigurace shodovat, že z škálování virtuálních počítačů virtuálního počítače nebo hello hello nastavená.</span><span class="sxs-lookup"><span data-stu-id="550df-536">hello `resourceId` in hello configuration must match that of hello VM or hello virtual machine scale set.</span></span>

* <span data-ttu-id="550df-537">Metriky platformy Azure grafů a výstrah zná hello resourceId Dobrý den pracujete na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="550df-537">Azure platform metrics charting and alerting knows hello resourceId of hello VM you're working on.</span></span> <span data-ttu-id="550df-538">Se očekává, že toofind hello data pro virtuální počítač pomocí hello resourceId hello vyhledávání klíče.</span><span class="sxs-lookup"><span data-stu-id="550df-538">It expects toofind hello data for your VM using hello resourceId hello lookup key.</span></span>
* <span data-ttu-id="550df-539">Pokud používáte Azure škálování, musí odpovídat hello resourceId v konfiguraci automatického škálování hello používá LAD resourceId hello.</span><span class="sxs-lookup"><span data-stu-id="550df-539">If you use Azure autoscale, hello resourceId in hello autoscale configuration must match hello resourceId used by LAD.</span></span>
* <span data-ttu-id="550df-540">Hello resourceId je integrovaná do hello názvy JsonBlobs zapsaných správcem LAD.</span><span class="sxs-lookup"><span data-stu-id="550df-540">hello resourceId is built into hello names of JsonBlobs written by LAD.</span></span>

## <a name="view-your-data"></a><span data-ttu-id="550df-541">Zobrazení dat</span><span class="sxs-lookup"><span data-stu-id="550df-541">View your data</span></span>

<span data-ttu-id="550df-542">Informace o výkonu Azure portálu tooview hello nebo nastavit výstrahy:</span><span class="sxs-lookup"><span data-stu-id="550df-542">Use hello Azure portal tooview performance data or set alerts:</span></span>

![Bitové kopie](./media/diagnostic-extension/graph_metrics.png)

<span data-ttu-id="550df-544">Hello `performanceCounters` dat byly vždy uloženy v tabulce Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="550df-544">hello `performanceCounters` data are always stored in an Azure Storage table.</span></span> <span data-ttu-id="550df-545">Rozhraní API Azure úložiště jsou k dispozici pro mnoho jazyky a platformy.</span><span class="sxs-lookup"><span data-stu-id="550df-545">Azure Storage APIs are available for many languages and platforms.</span></span>

<span data-ttu-id="550df-546">Data odeslaná jímky tooJsonBlob se ukládají do objektů BLOB v účtu úložiště hello s názvem v hello [chráněné nastavení](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="550df-546">Data sent tooJsonBlob sinks is stored in blobs in hello storage account named in hello [Protected settings](#protected-settings).</span></span> <span data-ttu-id="550df-547">Můžete využívat data objektu blob hello pomocí všechny rozhraní API úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="550df-547">You can consume hello blob data using any Azure Blob Storage APIs.</span></span>

<span data-ttu-id="550df-548">Kromě toho můžete tyto uživatelského rozhraní nástroje tooaccess hello dat ve službě Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="550df-548">In addition, you can use these UI tools tooaccess hello data in Azure Storage:</span></span>

* <span data-ttu-id="550df-549">Průzkumníka serveru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="550df-549">Visual Studio Server Explorer.</span></span>
* <span data-ttu-id="550df-550">[Microsoft Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").</span><span class="sxs-lookup"><span data-stu-id="550df-550">[Microsoft Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").</span></span>

<span data-ttu-id="550df-551">Tento snímek relaci Microsoft Azure Storage Explorer zobrazuje hello vygenerovat tabulky služby Azure Storage a kontejnery z správně nakonfigurována rozšíření LAD 3.0 na testovací virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="550df-551">This snapshot of a Microsoft Azure Storage Explorer session shows hello generated Azure Storage tables and containers from a correctly configured LAD 3.0 extension on a test VM.</span></span> <span data-ttu-id="550df-552">Obrázek Hello neodpovídá přesně hello [Ukázková konfigurace LAD 3.0](#an-example-lad-30-configuration).</span><span class="sxs-lookup"><span data-stu-id="550df-552">hello image doesn't match exactly with hello [sample LAD 3.0 configuration](#an-example-lad-30-configuration).</span></span>

![Bitové kopie](./media/diagnostic-extension/stg_explorer.png)

<span data-ttu-id="550df-554">V tématu hello relevantní [EventHubs dokumentace](../../event-hubs/event-hubs-what-is-event-hubs.md) toolearn jak tooconsume zprávy publikované tooan EventHubs koncový bod.</span><span class="sxs-lookup"><span data-stu-id="550df-554">See hello relevant [EventHubs documentation](../../event-hubs/event-hubs-what-is-event-hubs.md) toolearn how tooconsume messages published tooan EventHubs endpoint.</span></span>

## <a name="next-steps"></a><span data-ttu-id="550df-555">Další kroky</span><span class="sxs-lookup"><span data-stu-id="550df-555">Next steps</span></span>

* <span data-ttu-id="550df-556">Vytvoření metriky výstrahy v [Azure monitorování](../../monitoring-and-diagnostics/insights-alerts-portal.md) pro shromažďování metrik hello.</span><span class="sxs-lookup"><span data-stu-id="550df-556">Create metric alerts in [Azure Monitor](../../monitoring-and-diagnostics/insights-alerts-portal.md) for hello metrics you collect.</span></span>
* <span data-ttu-id="550df-557">Vytvoření [tabulek sledování](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) pro vaše metriky.</span><span class="sxs-lookup"><span data-stu-id="550df-557">Create [monitoring charts](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) for your metrics.</span></span>
* <span data-ttu-id="550df-558">Zjistěte, jak příliš[vytvořit škálovací sadu virtuálních počítačů](/azure/virtual-machines/linux/tutorial-create-vmss) pomocí vaší automatické škálování toocontrol metriky.</span><span class="sxs-lookup"><span data-stu-id="550df-558">Learn how too[create a virtual machine scale set](/azure/virtual-machines/linux/tutorial-create-vmss) using your metrics toocontrol autoscaling.</span></span>
