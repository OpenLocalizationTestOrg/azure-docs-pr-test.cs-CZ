---
title: "Zachovat soubory v prostředí cloudu Azure (Preview) | Microsoft Docs"
description: "Návod, jak Azure Cloud prostředí potrvají soubory."
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
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: 61a8bfcf3704f361432400771d8fcc8b81927b53
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="persist-files-in-azure-cloud-shell"></a><span data-ttu-id="9e3f9-103">Zachovat soubory v prostředí cloudu Azure</span><span class="sxs-lookup"><span data-stu-id="9e3f9-103">Persist files in Azure Cloud Shell</span></span>
<span data-ttu-id="9e3f9-104">Cloudové prostředí využívá Azure File storage pro soubory zachová napříč relacemi.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-104">Cloud Shell takes advantage of Azure File storage to persist files across sessions.</span></span>

## <a name="set-up-a-clouddrive-file-share"></a><span data-ttu-id="9e3f9-105">Nastavení `clouddrive` sdílené složky</span><span class="sxs-lookup"><span data-stu-id="9e3f9-105">Set up a `clouddrive` file share</span></span>
<span data-ttu-id="9e3f9-106">Při počáteční spuštění prostředí cloudu vás vyzve k přidružit nové nebo existující sdílenou složku souborů zachová napříč relacemi.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-106">On initial start, Cloud Shell prompts you to associate a new or existing file share to persist files across sessions.</span></span>

### <a name="create-new-storage"></a><span data-ttu-id="9e3f9-107">Vytvoření nového úložiště</span><span class="sxs-lookup"><span data-stu-id="9e3f9-107">Create new storage</span></span>

<span data-ttu-id="9e3f9-108">Když používáte základní nastavení a vyberte pouze předplatné, cloudové prostředí vytvoří tři zdroje vaším jménem v podporované oblasti, které je nejblíže můžete:</span><span class="sxs-lookup"><span data-stu-id="9e3f9-108">When you use basic settings and select only a subscription, Cloud Shell creates three resources on your behalf in the supported region that's nearest to you:</span></span>
* <span data-ttu-id="9e3f9-109">Skupina prostředků:`cloud-shell-storage-<region>`</span><span class="sxs-lookup"><span data-stu-id="9e3f9-109">Resource group: `cloud-shell-storage-<region>`</span></span>
* <span data-ttu-id="9e3f9-110">Účet úložiště:`cs<uniqueGuid>`</span><span class="sxs-lookup"><span data-stu-id="9e3f9-110">Storage account: `cs<uniqueGuid>`</span></span>
* <span data-ttu-id="9e3f9-111">Sdílené složky:`cs-<user>-<domain>-com-<uniqueGuid>`</span><span class="sxs-lookup"><span data-stu-id="9e3f9-111">File share: `cs-<user>-<domain>-com-<uniqueGuid>`</span></span>

![Nastavení předplatného](media/basic-storage.png)

<span data-ttu-id="9e3f9-113">Sdílené složky připojí jako `clouddrive` ve vaší `$Home` adresáře.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-113">The file share mounts as `clouddrive` in your `$Home` directory.</span></span> <span data-ttu-id="9e3f9-114">Sdílené složky se používá i k uložení 5 GB image, který je pro vás vytvořil a který automaticky aktualizuje a přetrvává vaší `$Home` adresáře.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-114">The file share is also used to store a 5-GB image that's created for you and that automatically updates and persists your `$Home` directory.</span></span> <span data-ttu-id="9e3f9-115">Toto je jednorázová akce a sdílené složky automaticky připojí v následné relace.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-115">This is a one-time action, and the file share mounts automatically in subsequent sessions.</span></span>

### <a name="use-existing-resources"></a><span data-ttu-id="9e3f9-116">Používat existující prostředky</span><span class="sxs-lookup"><span data-stu-id="9e3f9-116">Use existing resources</span></span>

<span data-ttu-id="9e3f9-117">Pomocí pokročilé možnosti můžete přidružit existující prostředky.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-117">By using the advanced option, you can associate existing resources.</span></span> <span data-ttu-id="9e3f9-118">Po zobrazení výzvy se instalační program úložiště, vyberte **zobrazit upřesňující nastavení** zobrazíte další možnosti.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-118">When the storage setup prompt appears, select **Show advanced settings** to view additional options.</span></span> <span data-ttu-id="9e3f9-119">Existující sdílené složky zobrazí 5 GB uživatelská image se zachovat vaše `$Home` adresáře.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-119">Existing file shares receive a 5-GB user image to persist your `$Home` directory.</span></span> <span data-ttu-id="9e3f9-120">Rozevíracích nabídek jsou filtrovány pro vaši oblast cloudové prostředí a pro účty místní redundantní & geograficky redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-120">The drop-down menus are filtered for your Cloud Shell region and for local-redundant & geo-redundant storage accounts.</span></span>

![Nastavení skupiny prostředků](media/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a><span data-ttu-id="9e3f9-122">Omezit vytvoření prostředku zásadami prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-122">Restrict resource creation with an Azure resource policy</span></span>
<span data-ttu-id="9e3f9-123">Účty úložiště, které vytvoříte v prostředí cloudu jsou označené `ms-resource-usage:azure-cloud-shell`.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-123">Storage accounts that you create in Cloud Shell are tagged with `ms-resource-usage:azure-cloud-shell`.</span></span> <span data-ttu-id="9e3f9-124">Pokud chcete zakázat uživatelům ve vytváření účtů úložiště v prostředí cloudu, vytvořte [zásad prostředků Azure pro značky](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) jsou aktivovány tato konkrétní značka.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-124">If you want to disallow users from creating storage accounts in Cloud Shell, create an [Azure resource policy for tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) that are triggered by this specific tag.</span></span>

## <a name="how-cloud-shell-works"></a><span data-ttu-id="9e3f9-125">Jak funguje cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="9e3f9-125">How Cloud Shell works</span></span>
<span data-ttu-id="9e3f9-126">Cloudové prostředí potrvají soubory pomocí obě z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="9e3f9-126">Cloud Shell persists files through both of the following methods:</span></span>
* <span data-ttu-id="9e3f9-127">Vytvoření image disku vaší `$Home` adresáře se zachovat veškerý obsah v adresáři.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-127">Creating a disk image of your `$Home` directory to persist all contents within the directory.</span></span> <span data-ttu-id="9e3f9-128">Bitové kopie disku je uložen v zadané sdílené složky jako `acc_<User>.img` v `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, a je automaticky synchronizuje změny.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-128">The disk image is saved in your specified file share as `acc_<User>.img` at `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, and it automatically syncs changes.</span></span>

* <span data-ttu-id="9e3f9-129">Připojení zadané sdílené složky jako `clouddrive` ve vaší `$Home` adresář pro přímé interakci se sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-129">Mounting your specified file share as `clouddrive` in your `$Home` directory for direct file share interaction.</span></span> <span data-ttu-id="9e3f9-130">`/Home/<User>/clouddrive`se mapuje na `fileshare.storage.windows.net/fileshare`.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-130">`/Home/<User>/clouddrive` is mapped to `fileshare.storage.windows.net/fileshare`.</span></span>
 
> [!NOTE]
> <span data-ttu-id="9e3f9-131">Všechny soubory ve vašem `$Home` adresář, jako jsou klíče SSH, jsou nastavené jako trvalé v bitové kopii disku uživatele, který je uložený v připojené sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-131">All files in your `$Home` directory, such as SSH keys, are persisted in your user disk image, which is stored in your mounted file share.</span></span> <span data-ttu-id="9e3f9-132">Použít osvědčené postupy při zachování informace ve vaší `$Home` adresář a připojené sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-132">Apply best practices when you persist information in your `$Home` directory and mounted file share.</span></span>

## <a name="use-the-clouddrive-command"></a><span data-ttu-id="9e3f9-133">Použití `clouddrive` příkaz</span><span class="sxs-lookup"><span data-stu-id="9e3f9-133">Use the `clouddrive` command</span></span>
<span data-ttu-id="9e3f9-134">S cloudové prostředí, můžete spustit příkaz s názvem `clouddrive`, což umožňuje ručně aktualizovat sdílené složky, která je připojena k cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-134">With Cloud Shell, you can run a command called `clouddrive`, which enables you to manually update the file share that's mounted to Cloud Shell.</span></span>
<span data-ttu-id="9e3f9-135">![Spuštěním příkazu "clouddrive"](media/clouddrive-h.png)</span><span class="sxs-lookup"><span data-stu-id="9e3f9-135">![Running the "clouddrive" command](media/clouddrive-h.png)</span></span>

## <a name="mount-a-new-clouddrive"></a><span data-ttu-id="9e3f9-136">Připojit nový`clouddrive`</span><span class="sxs-lookup"><span data-stu-id="9e3f9-136">Mount a new `clouddrive`</span></span>

### <a name="prerequisites-for-manual-mounting"></a><span data-ttu-id="9e3f9-137">Předpoklady pro ruční připojení</span><span class="sxs-lookup"><span data-stu-id="9e3f9-137">Prerequisites for manual mounting</span></span>
<span data-ttu-id="9e3f9-138">Sdílené složky, který je spojen s cloudové prostředí pomocí můžete aktualizovat `clouddrive mount` příkaz.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-138">You can update the file share that's associated with Cloud Shell by using the `clouddrive mount` command.</span></span>

<span data-ttu-id="9e3f9-139">Pokud připojíte existující sdílené složky, musí být účty úložiště:</span><span class="sxs-lookup"><span data-stu-id="9e3f9-139">If you mount an existing file share, the storage accounts must be:</span></span>
* <span data-ttu-id="9e3f9-140">Místně redundantní úložiště nebo geograficky redundantní úložiště pro podporu sdílených složek.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-140">Locally-redundant storage or geo-redundant storage to support file shares.</span></span>
* <span data-ttu-id="9e3f9-141">Nachází se ve vašem regionu přiřazené.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-141">Located in your assigned region.</span></span> <span data-ttu-id="9e3f9-142">Po registraci oblasti jsou přiřazeny k je uvedena v názvu skupiny prostředků `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-142">When you are onboarding, the region you are assigned to is listed in the resource group name `cloud-shell-storage-<region>`.</span></span>

### <a name="supported-storage-regions"></a><span data-ttu-id="9e3f9-143">Oblasti podporované úložiště</span><span class="sxs-lookup"><span data-stu-id="9e3f9-143">Supported storage regions</span></span>
<span data-ttu-id="9e3f9-144">Soubory Azure se musí nacházet ve stejné oblasti jako počítač cloudové prostředí, které jste jim připojení.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-144">The Azure files must reside in the same region as the Cloud Shell machine that you're mounting them to.</span></span> <span data-ttu-id="9e3f9-145">Clustery prostředí cloudu aktuálně existuje v následujících oblastech:</span><span class="sxs-lookup"><span data-stu-id="9e3f9-145">Cloud Shell clusters currently exist in the following regions:</span></span>
|<span data-ttu-id="9e3f9-146">Oblast</span><span class="sxs-lookup"><span data-stu-id="9e3f9-146">Area</span></span>|<span data-ttu-id="9e3f9-147">Oblast</span><span class="sxs-lookup"><span data-stu-id="9e3f9-147">Region</span></span>|
|---|---|
|<span data-ttu-id="9e3f9-148">Amerika</span><span class="sxs-lookup"><span data-stu-id="9e3f9-148">Americas</span></span>|<span data-ttu-id="9e3f9-149">Východ USA, střed USA – Jih, západ USA</span><span class="sxs-lookup"><span data-stu-id="9e3f9-149">East US, South Central US, West US</span></span>|
|<span data-ttu-id="9e3f9-150">Evropa</span><span class="sxs-lookup"><span data-stu-id="9e3f9-150">Europe</span></span>|<span data-ttu-id="9e3f9-151">Severní Evropa, Západní Evropa</span><span class="sxs-lookup"><span data-stu-id="9e3f9-151">North Europe, West Europe</span></span>|
|<span data-ttu-id="9e3f9-152">Asie a Tichomoří</span><span class="sxs-lookup"><span data-stu-id="9e3f9-152">Asia Pacific</span></span>|<span data-ttu-id="9e3f9-153">Indie centrální, jihovýchodní Asie</span><span class="sxs-lookup"><span data-stu-id="9e3f9-153">India Central, Southeast Asia</span></span>|

### <a name="the-clouddrive-mount-command"></a><span data-ttu-id="9e3f9-154">`clouddrive mount` Příkaz</span><span class="sxs-lookup"><span data-stu-id="9e3f9-154">The `clouddrive mount` command</span></span>

> [!NOTE]
> <span data-ttu-id="9e3f9-155">Pokud jste připojení nové sdílené složky, vytvoří se nové uživatelské image pro vaše `$Home` adresáře, protože váš předchozí `$Home` bitové kopie je uložen v předchozí sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-155">If you're mounting a new file share, a new user image is created for your `$Home` directory, because your previous `$Home` image is kept in the previous file share.</span></span>

<span data-ttu-id="9e3f9-156">Spustit `clouddrive mount` příkazu s následujícími parametry:</span><span class="sxs-lookup"><span data-stu-id="9e3f9-156">Run the `clouddrive mount` command with the following parameters:</span></span>

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

<span data-ttu-id="9e3f9-157">Chcete-li zobrazit další podrobnosti, spusťte `clouddrive mount -h`, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="9e3f9-157">To view more details, run `clouddrive mount -h`, as shown here:</span></span>

![Spuštění ' clouddrive mount'command](media/mount-h.png)

## <a name="unmount-clouddrive"></a><span data-ttu-id="9e3f9-159">Odpojení`clouddrive`</span><span class="sxs-lookup"><span data-stu-id="9e3f9-159">Unmount `clouddrive`</span></span>
<span data-ttu-id="9e3f9-160">Sdílení souborů, které je připojené k prostředí cloudu kdykoli můžete odpojit.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-160">You can unmount a file share that's mounted to Cloud Shell at any time.</span></span> <span data-ttu-id="9e3f9-161">Jakmile je vaše sdílená složka nepřipojené, vyzve k připojit nové sdílené složky před další relace.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-161">Once your file share is unmounted, you will be prompted to mount a new file share prior to your next session.</span></span>

<span data-ttu-id="9e3f9-162">Odebrat sdílenou složku z prostředí cloudu:</span><span class="sxs-lookup"><span data-stu-id="9e3f9-162">To remove a file share from Cloud Shell:</span></span>
1. <span data-ttu-id="9e3f9-163">Spusťte `clouddrive unmount`.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-163">Run `clouddrive unmount`.</span></span>
2. <span data-ttu-id="9e3f9-164">Na vědomí a potvrďte výzvy.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-164">Acknowledge and confirm the prompts.</span></span>

<span data-ttu-id="9e3f9-165">Sdílené složky budou nadále existovat, pokud chcete odstranit ručně.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-165">Your file share will continue to exist unless you delete it manually.</span></span> <span data-ttu-id="9e3f9-166">Cloudové prostředí už Hledat pro tuto sdílenou složku na následné relace.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-166">Cloud Shell will no longer search for this file share on subsequent sessions.</span></span>

<span data-ttu-id="9e3f9-167">Chcete-li zobrazit další podrobnosti, spusťte `clouddrive unmount -h`, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="9e3f9-167">To view more details, run `clouddrive unmount -h`, as shown here:</span></span>

![Spuštění ' clouddrive unmount'command](media/unmount-h.png)

> [!WARNING]
> <span data-ttu-id="9e3f9-169">Spuštění tohoto příkazu se neodstraní žádné prostředky.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-169">Running this command will not delete any resources.</span></span> <span data-ttu-id="9e3f9-170">Ručním odstranění skupiny prostředků, účet úložiště nebo sdílené složky, který je namapovaný na cloudové prostředí dojde k trvalému odstranění vaší `$Home` directory image a další soubory do sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-170">Manually deleting a resource group, storage account, or file share that is mapped to Cloud Shell will permanently delete your `$Home` directory image and any other files in your file share.</span></span> <span data-ttu-id="9e3f9-171">Tuto akci nelze vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-171">This action cannot be undone.</span></span>

## <a name="list-clouddrive-file-shares"></a><span data-ttu-id="9e3f9-172">Seznam `clouddrive` sdílené složky</span><span class="sxs-lookup"><span data-stu-id="9e3f9-172">List `clouddrive` file shares</span></span>
<span data-ttu-id="9e3f9-173">Chcete-li zjistit, které sdílené složky je připojit jako `clouddrive`, spusťte následující `df` příkaz.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-173">To discover which file share is mounted as `clouddrive`, run the following `df` command.</span></span> 

<span data-ttu-id="9e3f9-174">Cestu k souboru na clouddrive ukazuje, že váš název účtu úložiště a sdílených složek v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-174">The file path to clouddrive shows your storage account name and file share in the URL.</span></span> <span data-ttu-id="9e3f9-175">Například `//storageaccountname.file.core.windows.net/filesharename`.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-175">For example, `//storageaccountname.file.core.windows.net/filesharename`</span></span>

```
justin@Azure:~$ df
Filesystem                                               1K-blocks     Used Available Use% Mounted on
overlay                                                   30428648 15585636  14826628  52% /
tmpfs                                                       986704        0    986704   0% /dev
tmpfs                                                       986704        0    986704   0% /sys/fs/cgroup
/dev/sda1                                                 30428648 15585636  14826628  52% /etc/hosts
shm                                                          65536        0     65536   0% /dev/shm
//mystoragename.file.core.windows.net/fileshareName        6291456  5242944   1048512  84% /usr/justin/clouddrive
/dev/loop0                                                 5160576   601652   4296780  13% /home/justin
```

## <a name="transfer-local-files-to-cloud-shell"></a><span data-ttu-id="9e3f9-176">Přenos místních souborů do prostředí cloudu</span><span class="sxs-lookup"><span data-stu-id="9e3f9-176">Transfer local files to Cloud Shell</span></span>
<span data-ttu-id="9e3f9-177">`clouddrive` Synchronizace adresáře se v okně portálu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-177">The `clouddrive` directory syncs with the Azure portal storage blade.</span></span> <span data-ttu-id="9e3f9-178">Pomocí tohoto okna pro přenos místní soubory do nebo ze sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-178">Use this blade to transfer local files to or from your file share.</span></span> <span data-ttu-id="9e3f9-179">Aktualizace soubory z prostředí cloudu se projeví v úložišti file grafického uživatelského rozhraní při aktualizaci okna.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-179">Updating files from within Cloud Shell is reflected in the file storage GUI when you refresh the blade.</span></span>

### <a name="download-files"></a><span data-ttu-id="9e3f9-180">Stažení souborů</span><span class="sxs-lookup"><span data-stu-id="9e3f9-180">Download files</span></span>

![Seznam místních souborů](media/download.png)
1. <span data-ttu-id="9e3f9-182">Na portálu Azure přejděte do sdílené složky připojeného souboru.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-182">In the Azure portal, go to the mounted file share.</span></span>
2. <span data-ttu-id="9e3f9-183">Vyberte cílový soubor.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-183">Select the target file.</span></span>
3. <span data-ttu-id="9e3f9-184">Vyberte **Stáhnout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-184">Select the **Download** button.</span></span>

### <a name="upload-files"></a><span data-ttu-id="9e3f9-185">Nahrání souborů</span><span class="sxs-lookup"><span data-stu-id="9e3f9-185">Upload files</span></span>

![Místní soubory k odeslání](media/upload.png)
1. <span data-ttu-id="9e3f9-187">Přejděte do sdílené složky připojeného souboru.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-187">Go to your mounted file share.</span></span>
2. <span data-ttu-id="9e3f9-188">Vyberte **nahrát** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-188">Select the **Upload** button.</span></span>
3. <span data-ttu-id="9e3f9-189">Vyberte soubor nebo soubory, které chcete nahrát.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-189">Select the file or files that you want to upload.</span></span>
4. <span data-ttu-id="9e3f9-190">Potvrďte nahrávání.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-190">Confirm the upload.</span></span>

<span data-ttu-id="9e3f9-191">Teď byste měli vidět soubory, které jsou dostupné ve vaší `clouddrive` adresář v prostředí cloudu.</span><span class="sxs-lookup"><span data-stu-id="9e3f9-191">You should now see the files that are accessible in your `clouddrive` directory in Cloud Shell.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9e3f9-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9e3f9-192">Next steps</span></span>
[<span data-ttu-id="9e3f9-193">Rychlý start cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="9e3f9-193">Cloud Shell Quickstart</span></span>](quickstart.md) <br><span data-ttu-id="9e3f9-194">
[Další informace o Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage)</span><span class="sxs-lookup"><span data-stu-id="9e3f9-194">
[Learn about Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage)</span></span> <br><span data-ttu-id="9e3f9-195">
[Další informace o značkách úložiště](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)</span><span class="sxs-lookup"><span data-stu-id="9e3f9-195">
[Learn about storage tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)</span></span> <br>
