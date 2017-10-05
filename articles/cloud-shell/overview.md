---
title: "Přehled služby Azure Cloud prostředí (Preview) | Microsoft Docs"
description: "Přehled prostředí cloudu Azure."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: juluk
ms.openlocfilehash: 7165633cd354eeea2e3619f839338e6af1524e56
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="overview-of-azure-cloud-shell-preview"></a><span data-ttu-id="752f4-103">Přehled prostředí cloudu Azure (Preview)</span><span class="sxs-lookup"><span data-stu-id="752f4-103">Overview of Azure Cloud Shell (Preview)</span></span>
<span data-ttu-id="752f4-104">Prostředí Azure Cloud je interaktivní, přístupných prohlížeče prostředí pro správu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="752f4-104">Azure Cloud Shell is an interactive, browser-accessible shell for managing Azure resources.</span></span>

![](media/overview-pic.png)

## <a name="features"></a><span data-ttu-id="752f4-105">Funkce</span><span class="sxs-lookup"><span data-stu-id="752f4-105">Features</span></span>
### <a name="browser-based-shell-experience"></a><span data-ttu-id="752f4-106">Prostředí shell založené na prohlížeči</span><span class="sxs-lookup"><span data-stu-id="752f4-106">Browser-based shell experience</span></span>
<span data-ttu-id="752f4-107">Cloudové prostředí umožňuje přístup k založené na prohlížeči příkazového řádku prostředí, vytvořené s nástroji úlohy správy Azure v paměti.</span><span class="sxs-lookup"><span data-stu-id="752f4-107">Cloud Shell enables access to a browser-based command-line experience built with Azure management tasks in mind.</span></span> <span data-ttu-id="752f4-108">Můžete zadat využívání cloudové prostředí pro práci untethered z místního počítače způsobem, jenom do cloudu.</span><span class="sxs-lookup"><span data-stu-id="752f4-108">Leverage Cloud Shell to work untethered from a local machine in a way only the cloud can provide.</span></span>

### <a name="pre-configured-azure-workstation"></a><span data-ttu-id="752f4-109">Předem nakonfigurovaná Azure pracovní stanice</span><span class="sxs-lookup"><span data-stu-id="752f4-109">Pre-configured Azure workstation</span></span>
<span data-ttu-id="752f4-110">Cloudové prostředí předem nainstalovaný pomocí oblíbené nástroje příkazového řádku a podpora jazyků, abyste mohli pracovat rychleji.</span><span class="sxs-lookup"><span data-stu-id="752f4-110">Cloud Shell comes pre-installed with popular command-line tools and language support so you can work faster.</span></span>

[<span data-ttu-id="752f4-111">Sem zobrazte seznam úplné nástrojů pro cloudové prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="752f4-111">View the full tooling list for Azure Cloud Shell here.</span></span>](features.md#tools)

### <a name="automatic-authentication"></a><span data-ttu-id="752f4-112">Automatické ověření</span><span class="sxs-lookup"><span data-stu-id="752f4-112">Automatic authentication</span></span>
<span data-ttu-id="752f4-113">Cloudové prostředí bezpečně ověřuje automaticky na každou relaci pro okamžitý přístup k prostředkům prostřednictvím Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="752f4-113">Cloud Shell securely authenticates automatically on each session for instant access to your resources through the Azure CLI 2.0.</span></span>

### <a name="connect-your-azure-file-storage"></a><span data-ttu-id="752f4-114">Připojení Azure File storage</span><span class="sxs-lookup"><span data-stu-id="752f4-114">Connect your Azure File storage</span></span>
<span data-ttu-id="752f4-115">Cloudové prostředí počítače jsou dočasné a v důsledku vyžadují sdílenou složku Azure chcete připojit jako `clouddrive` udržení adresáře $Home.</span><span class="sxs-lookup"><span data-stu-id="752f4-115">Cloud Shell machines are temporary and as a result require an Azure file share to be mounted as `clouddrive` to persist your $Home directory.</span></span>
<span data-ttu-id="752f4-116">Při prvním spuštění prostředí cloudu vás vyzve k vytvoření prostředku skupiny, účet úložiště a sdílených složek vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="752f4-116">On first launch Cloud Shell prompts to create a resource group, storage account, and file share on your behalf.</span></span> <span data-ttu-id="752f4-117">To je jednorázový krok a bude automaticky připojen pro všechny relace.</span><span class="sxs-lookup"><span data-stu-id="752f4-117">This is a one-time step and will be automatically attached for all sessions.</span></span> 

#### <a name="create-new-storage"></a><span data-ttu-id="752f4-118">Vytvoření nového úložiště</span><span class="sxs-lookup"><span data-stu-id="752f4-118">Create new storage</span></span>
![](media/basic-storage.png)

<span data-ttu-id="752f4-119">Místně redundantní úložiště (LRS) účet může být vytvořen vaším jménem pomocí sdílenou složku Azure obsahující výchozí obrázek 5-GB místa na disku.</span><span class="sxs-lookup"><span data-stu-id="752f4-119">A locally-redundant storage (LRS) account can be created on your behalf with an Azure file share containing a default 5-GB disk image.</span></span> <span data-ttu-id="752f4-120">Sdílené složky připojí jako `clouddrive` pro soubor sdílet interakce se používá k synchronizaci a zachovat adresáře $Home bitové kopie disku.</span><span class="sxs-lookup"><span data-stu-id="752f4-120">The file share mounts as `clouddrive` for file share interaction with the disk image being used to sync and persist your $Home directory.</span></span> <span data-ttu-id="752f4-121">Náklady na úložiště regulární použít.</span><span class="sxs-lookup"><span data-stu-id="752f4-121">Regular storage costs apply.</span></span>

<span data-ttu-id="752f4-122">Vaším jménem vytvoří tři zdroje:</span><span class="sxs-lookup"><span data-stu-id="752f4-122">Three resources will be created on your behalf:</span></span>
1. <span data-ttu-id="752f4-123">Skupinu prostředků s názvem:`cloud-shell-storage-<region>`</span><span class="sxs-lookup"><span data-stu-id="752f4-123">Resource Group named: `cloud-shell-storage-<region>`</span></span>
2. <span data-ttu-id="752f4-124">Účet úložiště s názvem:`cs<uniqueGuid>`</span><span class="sxs-lookup"><span data-stu-id="752f4-124">Storage Account named: `cs<uniqueGuid>`</span></span>
3. <span data-ttu-id="752f4-125">Sdílené složky s názvem:`cs-<user>-<domain>-com-uniqueGuid`</span><span class="sxs-lookup"><span data-stu-id="752f4-125">File Share named: `cs-<user>-<domain>-com-uniqueGuid`</span></span>

> [!Note]
> <span data-ttu-id="752f4-126">Všechny soubory v adresáři $Home například klíče SSH zůstávají v bitové kopii disku uživatele uložený ve sdílené složce vaší připojeného souboru.</span><span class="sxs-lookup"><span data-stu-id="752f4-126">All files in your $Home directory such as SSH keys are persisted in your user disk image stored in your mounted file share.</span></span> <span data-ttu-id="752f4-127">Použít osvědčené postupy při ukládání souborů v adresáři $Home a připojené sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="752f4-127">Apply best practices when saving files in your $Home directory and mounted file share.</span></span>

#### <a name="use-existing-resources"></a><span data-ttu-id="752f4-128">Používat existující prostředky</span><span class="sxs-lookup"><span data-stu-id="752f4-128">Use existing resources</span></span>
![](media/advanced-storage.png)

<span data-ttu-id="752f4-129">Upřesňující možnost je k dispozici také umožňuje přidružit existující prostředky pro cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="752f4-129">An advanced option is also provided allowing you to associate existing resources to Cloud Shell.</span></span> <span data-ttu-id="752f4-130">Při s výzvou nastavení úložiště, klikněte na tlačítko "Zobrazit upřesňující nastavení" a vyberte další možnosti.</span><span class="sxs-lookup"><span data-stu-id="752f4-130">When presented with the storage setup prompt, click "Show advanced settings" to select additional options.</span></span> <span data-ttu-id="752f4-131">Rozevírací seznamy jsou filtrovány přiřazené oblast prostředí cloudu a místně nebo globálně redundantní úložiště účtů.</span><span class="sxs-lookup"><span data-stu-id="752f4-131">Dropdowns are filtered for your assigned Cloud Shell region and locally/globally-redundant storage accounts.</span></span>

<span data-ttu-id="752f4-132">[Informace o prostředí cloudové úložiště, aktualizace sdílené složky a nahrávání nebo stahování souborů.] (uložením prostředí storage.md)</span><span class="sxs-lookup"><span data-stu-id="752f4-132">[Learn about Cloud Shell storage, updating file shares, and uploading/downloading files.] (persisting-shell-storage.md)</span></span>

## <a name="concepts"></a><span data-ttu-id="752f4-133">Koncepty</span><span class="sxs-lookup"><span data-stu-id="752f4-133">Concepts</span></span>
* <span data-ttu-id="752f4-134">Cloudové prostředí běží na dočasné počítači k dispozici na každou relaci, jednotlivých uživatelů</span><span class="sxs-lookup"><span data-stu-id="752f4-134">Cloud Shell runs on a temporary machine provided on a per-session, per-user basis</span></span>
* <span data-ttu-id="752f4-135">Cloudové prostředí vyprší po 20 minutách bez interaktivní aktivity</span><span class="sxs-lookup"><span data-stu-id="752f4-135">Cloud Shell times out after 20 minutes without interactive activity</span></span>
* <span data-ttu-id="752f4-136">Cloudové prostředí lze přistupovat pouze u sdílené složky připojit</span><span class="sxs-lookup"><span data-stu-id="752f4-136">Cloud Shell can only be accessed with a file share attached</span></span>
* <span data-ttu-id="752f4-137">Cloudové prostředí je přiřazený jeden počítač na uživatelský účet</span><span class="sxs-lookup"><span data-stu-id="752f4-137">Cloud Shell is assigned one machine per user account</span></span>
* <span data-ttu-id="752f4-138">Máte nastavená oprávnění jako běžný uživatel Linux</span><span class="sxs-lookup"><span data-stu-id="752f4-138">Permissions are set as a regular Linux user</span></span>

[<span data-ttu-id="752f4-139">Další informace o všech funkcích cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="752f4-139">Learn more about all Cloud Shell features.</span></span>](features.md)

## <a name="examples"></a><span data-ttu-id="752f4-140">Příklady</span><span class="sxs-lookup"><span data-stu-id="752f4-140">Examples</span></span>
* <span data-ttu-id="752f4-141">Vytvořte nebo upravte skripty pro automatizaci správy Azure</span><span class="sxs-lookup"><span data-stu-id="752f4-141">Create or edit scripts to automate Azure management</span></span>
* <span data-ttu-id="752f4-142">Současně spravovat prostředky prostřednictvím portálu Azure a Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="752f4-142">Simultaneously manage resources via Azure portal and Azure CLI 2.0</span></span>
* <span data-ttu-id="752f4-143">Test-Drive rozhraní příkazového řádku Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="752f4-143">Test-drive Azure CLI 2.0</span></span>

[<span data-ttu-id="752f4-144">Vyzkoušejte všech těchto příkladech v prostředí cloudu rychlý start.</span><span class="sxs-lookup"><span data-stu-id="752f4-144">Try out all these examples at the Cloud Shell quickstart.</span></span>](quickstart.md)

## <a name="pricing"></a><span data-ttu-id="752f4-145">Ceny</span><span class="sxs-lookup"><span data-stu-id="752f4-145">Pricing</span></span>
<span data-ttu-id="752f4-146">Tento počítač hostování prostředí cloudu je bezplatná s předpoklad připojené Azure sdílené složky se zachovat adresáře $Home.</span><span class="sxs-lookup"><span data-stu-id="752f4-146">The machine hosting Cloud Shell is free, with a pre-requisite of a mounted Azure file share to persist your $Home directory.</span></span> <span data-ttu-id="752f4-147">Náklady na úložiště regulární použít.</span><span class="sxs-lookup"><span data-stu-id="752f4-147">Regular storage costs apply.</span></span>

## <a name="supported-browsers"></a><span data-ttu-id="752f4-148">Podporované prohlížeče</span><span class="sxs-lookup"><span data-stu-id="752f4-148">Supported browsers</span></span>
<span data-ttu-id="752f4-149">Cloudové prostředí se doporučuje pro Chrome, okraj a Safari.</span><span class="sxs-lookup"><span data-stu-id="752f4-149">Cloud Shell is recommended for Chrome, Edge, and Safari.</span></span> <span data-ttu-id="752f4-150">Když cloudové prostředí je podporované pro Chrome, Firefox, Safari, aplikace Internet Explorer a Edge, cloudové prostředí podléhá nastavení konkrétní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="752f4-150">While Cloud Shell is supported for Chrome, Firefox, Safari, IE, and Edge, Cloud Shell is subject to specific browser settings.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="752f4-151">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="752f4-151">Troubleshooting</span></span>
1. <span data-ttu-id="752f4-152">Pokud používáte předplatné služby Azure Active Directory, nelze vytvořit úložiště z důvodu chyby: 400 DisallowedOperation.</span><span class="sxs-lookup"><span data-stu-id="752f4-152">When using an Azure Active Directory subscription, I cannot create storage due to Error: 400 DisallowedOperation.</span></span> <span data-ttu-id="752f4-153">To vyřešit, použijte prosím předplatné Azure podporuje vytváření prostředků úložiště.</span><span class="sxs-lookup"><span data-stu-id="752f4-153">To resolve this, please use an Azure subscription capable of creating storage resources.</span></span> <span data-ttu-id="752f4-154">Nebudou se moct vytváření prostředků Azure AD odběry.</span><span class="sxs-lookup"><span data-stu-id="752f4-154">AD subscriptions are not able to create Azure resources.</span></span>

<span data-ttu-id="752f4-155">Konkrétní známá omezení, najdete v článku [omezení cloudové prostředí](limitations.md).</span><span class="sxs-lookup"><span data-stu-id="752f4-155">For specific known limitations, visit [limitations of Cloud Shell](limitations.md).</span></span>
