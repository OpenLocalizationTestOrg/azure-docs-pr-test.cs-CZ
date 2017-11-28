---
title: "aaaPersist soubory v prostředí cloudu Azure (Preview) | Microsoft Docs"
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
ms.openlocfilehash: b230453d5551c545553d63559741950206e4f1b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="persist-files-in-azure-cloud-shell"></a><span data-ttu-id="e477e-103">Zachovat soubory v prostředí cloudu Azure</span><span class="sxs-lookup"><span data-stu-id="e477e-103">Persist files in Azure Cloud Shell</span></span>
<span data-ttu-id="e477e-104">Cloudové prostředí využívá soubory toopersist úložiště Azure File napříč relacemi.</span><span class="sxs-lookup"><span data-stu-id="e477e-104">Cloud Shell takes advantage of Azure File storage toopersist files across sessions.</span></span>

## <a name="set-up-a-clouddrive-file-share"></a><span data-ttu-id="e477e-105">Nastavení `clouddrive` sdílené složky</span><span class="sxs-lookup"><span data-stu-id="e477e-105">Set up a `clouddrive` file share</span></span>
<span data-ttu-id="e477e-106">Při počáteční spuštění prostředí cloudu vás vyzve k tooassociate nový nebo existující sdílenou toopersist soubory mezi relace.</span><span class="sxs-lookup"><span data-stu-id="e477e-106">On initial start, Cloud Shell prompts you tooassociate a new or existing file share toopersist files across sessions.</span></span>

### <a name="create-new-storage"></a><span data-ttu-id="e477e-107">Vytvoření nového úložiště</span><span class="sxs-lookup"><span data-stu-id="e477e-107">Create new storage</span></span>

<span data-ttu-id="e477e-108">Když používáte základní nastavení a vyberte pouze předplatné, cloudové prostředí vytvoří tři zdroje vaším jménem v hello podporované oblasti, které je nejblíže tooyou:</span><span class="sxs-lookup"><span data-stu-id="e477e-108">When you use basic settings and select only a subscription, Cloud Shell creates three resources on your behalf in hello supported region that's nearest tooyou:</span></span>
* <span data-ttu-id="e477e-109">Skupina prostředků:`cloud-shell-storage-<region>`</span><span class="sxs-lookup"><span data-stu-id="e477e-109">Resource group: `cloud-shell-storage-<region>`</span></span>
* <span data-ttu-id="e477e-110">Účet úložiště:`cs<uniqueGuid>`</span><span class="sxs-lookup"><span data-stu-id="e477e-110">Storage account: `cs<uniqueGuid>`</span></span>
* <span data-ttu-id="e477e-111">Sdílené složky:`cs-<user>-<domain>-com-<uniqueGuid>`</span><span class="sxs-lookup"><span data-stu-id="e477e-111">File share: `cs-<user>-<domain>-com-<uniqueGuid>`</span></span>

![Nastavení předplatného Hello](media/basic-storage.png)

<span data-ttu-id="e477e-113">sdílenou složku Hello připojí jako `clouddrive` ve vaší `$Home` adresáře.</span><span class="sxs-lookup"><span data-stu-id="e477e-113">hello file share mounts as `clouddrive` in your `$Home` directory.</span></span> <span data-ttu-id="e477e-114">Hello je sdílená složka také použít toostore 5 GB bitové kopie, který je automaticky vytvořen a která aktualizuje a přetrvává vaší `$Home` adresáře.</span><span class="sxs-lookup"><span data-stu-id="e477e-114">hello file share is also used toostore a 5-GB image that's created for you and that automatically updates and persists your `$Home` directory.</span></span> <span data-ttu-id="e477e-115">Toto je jednorázová akce a sdílenou složku hello automaticky připojí v následné relace.</span><span class="sxs-lookup"><span data-stu-id="e477e-115">This is a one-time action, and hello file share mounts automatically in subsequent sessions.</span></span>

### <a name="use-existing-resources"></a><span data-ttu-id="e477e-116">Používat existující prostředky</span><span class="sxs-lookup"><span data-stu-id="e477e-116">Use existing resources</span></span>

<span data-ttu-id="e477e-117">Pomocí hello rozšířené možnosti, můžete přidružit existující prostředky.</span><span class="sxs-lookup"><span data-stu-id="e477e-117">By using hello advanced option, you can associate existing resources.</span></span> <span data-ttu-id="e477e-118">Jakmile se zobrazí hello úložiště instalace řádku, vyberte **zobrazit upřesňující nastavení** tooview další možnosti.</span><span class="sxs-lookup"><span data-stu-id="e477e-118">When hello storage setup prompt appears, select **Show advanced settings** tooview additional options.</span></span> <span data-ttu-id="e477e-119">Existující sdílené složky přijímat toopersist bitové kopie 5 GB uživatele vaše `$Home` adresáře.</span><span class="sxs-lookup"><span data-stu-id="e477e-119">Existing file shares receive a 5-GB user image toopersist your `$Home` directory.</span></span> <span data-ttu-id="e477e-120">Hello rozevíracích nabídek jsou filtrovány pro vaši oblast cloudové prostředí a pro účty místní redundantní & geograficky redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="e477e-120">hello drop-down menus are filtered for your Cloud Shell region and for local-redundant & geo-redundant storage accounts.</span></span>

![nastavení skupiny prostředků Hello](media/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a><span data-ttu-id="e477e-122">Omezit vytvoření prostředku zásadami prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="e477e-122">Restrict resource creation with an Azure resource policy</span></span>
<span data-ttu-id="e477e-123">Účty úložiště, které vytvoříte v prostředí cloudu jsou označené `ms-resource-usage:azure-cloud-shell`.</span><span class="sxs-lookup"><span data-stu-id="e477e-123">Storage accounts that you create in Cloud Shell are tagged with `ms-resource-usage:azure-cloud-shell`.</span></span> <span data-ttu-id="e477e-124">Pokud chcete toodisallow uživatelům ve vytváření účtů úložiště v prostředí cloudu, vytvořte [zásad prostředků Azure pro značky](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) jsou aktivovány tato konkrétní značka.</span><span class="sxs-lookup"><span data-stu-id="e477e-124">If you want toodisallow users from creating storage accounts in Cloud Shell, create an [Azure resource policy for tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) that are triggered by this specific tag.</span></span>

## <a name="how-cloud-shell-works"></a><span data-ttu-id="e477e-125">Jak funguje cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="e477e-125">How Cloud Shell works</span></span>
<span data-ttu-id="e477e-126">Cloudové prostředí potrvají soubory pomocí obou hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="e477e-126">Cloud Shell persists files through both of hello following methods:</span></span>
* <span data-ttu-id="e477e-127">Vytvoření image disku vaší `$Home` directory toopersist všechny obsah v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="e477e-127">Creating a disk image of your `$Home` directory toopersist all contents within hello directory.</span></span> <span data-ttu-id="e477e-128">bitová kopie disku Hello je uložen v zadané sdílené složky jako `acc_<User>.img` v `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, a je automaticky synchronizuje změny.</span><span class="sxs-lookup"><span data-stu-id="e477e-128">hello disk image is saved in your specified file share as `acc_<User>.img` at `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, and it automatically syncs changes.</span></span>

* <span data-ttu-id="e477e-129">Připojení zadané sdílené složky jako `clouddrive` ve vaší `$Home` adresář pro přímé interakci se sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="e477e-129">Mounting your specified file share as `clouddrive` in your `$Home` directory for direct file share interaction.</span></span> <span data-ttu-id="e477e-130">`/Home/<User>/clouddrive`je mapován příliš`fileshare.storage.windows.net/fileshare`.</span><span class="sxs-lookup"><span data-stu-id="e477e-130">`/Home/<User>/clouddrive` is mapped too`fileshare.storage.windows.net/fileshare`.</span></span>
 
> [!NOTE]
> <span data-ttu-id="e477e-131">Všechny soubory ve vašem `$Home` adresář, jako jsou klíče SSH, jsou nastavené jako trvalé v bitové kopii disku uživatele, který je uložený v připojené sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="e477e-131">All files in your `$Home` directory, such as SSH keys, are persisted in your user disk image, which is stored in your mounted file share.</span></span> <span data-ttu-id="e477e-132">Použít osvědčené postupy při zachování informace ve vaší `$Home` adresář a připojené sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="e477e-132">Apply best practices when you persist information in your `$Home` directory and mounted file share.</span></span>

## <a name="use-hello-clouddrive-command"></a><span data-ttu-id="e477e-133">Použití hello `clouddrive` příkaz</span><span class="sxs-lookup"><span data-stu-id="e477e-133">Use hello `clouddrive` command</span></span>
<span data-ttu-id="e477e-134">S cloudové prostředí, můžete spustit příkaz s názvem `clouddrive`, což umožňuje vám toomanually aktualizace hello sdílená toho připojené tooCloud prostředí.</span><span class="sxs-lookup"><span data-stu-id="e477e-134">With Cloud Shell, you can run a command called `clouddrive`, which enables you toomanually update hello file share that's mounted tooCloud Shell.</span></span>
<span data-ttu-id="e477e-135">![Spuštěním příkazu "clouddrive" hello](media/clouddrive-h.png)</span><span class="sxs-lookup"><span data-stu-id="e477e-135">![Running hello "clouddrive" command](media/clouddrive-h.png)</span></span>

## <a name="mount-a-new-clouddrive"></a><span data-ttu-id="e477e-136">Připojit nový`clouddrive`</span><span class="sxs-lookup"><span data-stu-id="e477e-136">Mount a new `clouddrive`</span></span>

### <a name="prerequisites-for-manual-mounting"></a><span data-ttu-id="e477e-137">Předpoklady pro ruční připojení</span><span class="sxs-lookup"><span data-stu-id="e477e-137">Prerequisites for manual mounting</span></span>
<span data-ttu-id="e477e-138">Můžete aktualizovat hello sdílené složky, který je spojen s cloudové prostředí pomocí hello `clouddrive mount` příkaz.</span><span class="sxs-lookup"><span data-stu-id="e477e-138">You can update hello file share that's associated with Cloud Shell by using hello `clouddrive mount` command.</span></span>

<span data-ttu-id="e477e-139">Pokud připojíte existující sdílené složky, musí být účty úložiště hello:</span><span class="sxs-lookup"><span data-stu-id="e477e-139">If you mount an existing file share, hello storage accounts must be:</span></span>
* <span data-ttu-id="e477e-140">Místně redundantní úložiště nebo sdílené složky toosupport geograficky redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="e477e-140">Locally-redundant storage or geo-redundant storage toosupport file shares.</span></span>
* <span data-ttu-id="e477e-141">Nachází se ve vašem regionu přiřazené.</span><span class="sxs-lookup"><span data-stu-id="e477e-141">Located in your assigned region.</span></span> <span data-ttu-id="e477e-142">Pokud jste registrace, hello oblast, která máte přiřazená toois uvedené v název skupiny prostředků hello `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="e477e-142">When you are onboarding, hello region you are assigned toois listed in hello resource group name `cloud-shell-storage-<region>`.</span></span>

### <a name="supported-storage-regions"></a><span data-ttu-id="e477e-143">Oblasti podporované úložiště</span><span class="sxs-lookup"><span data-stu-id="e477e-143">Supported storage regions</span></span>
<span data-ttu-id="e477e-144">Hello soubory Azure se musí nacházet v hello stejné oblasti jako počítač hello cloudové prostředí, který jste jim připojení.</span><span class="sxs-lookup"><span data-stu-id="e477e-144">hello Azure files must reside in hello same region as hello Cloud Shell machine that you're mounting them to.</span></span> <span data-ttu-id="e477e-145">Clustery prostředí cloudu se momentálně existují v hello následující oblasti:</span><span class="sxs-lookup"><span data-stu-id="e477e-145">Cloud Shell clusters currently exist in hello following regions:</span></span>
|<span data-ttu-id="e477e-146">Oblast</span><span class="sxs-lookup"><span data-stu-id="e477e-146">Area</span></span>|<span data-ttu-id="e477e-147">Oblast</span><span class="sxs-lookup"><span data-stu-id="e477e-147">Region</span></span>|
|---|---|
|<span data-ttu-id="e477e-148">Amerika</span><span class="sxs-lookup"><span data-stu-id="e477e-148">Americas</span></span>|<span data-ttu-id="e477e-149">Východ USA, střed USA – Jih, západ USA</span><span class="sxs-lookup"><span data-stu-id="e477e-149">East US, South Central US, West US</span></span>|
|<span data-ttu-id="e477e-150">Evropa</span><span class="sxs-lookup"><span data-stu-id="e477e-150">Europe</span></span>|<span data-ttu-id="e477e-151">Severní Evropa, Západní Evropa</span><span class="sxs-lookup"><span data-stu-id="e477e-151">North Europe, West Europe</span></span>|
|<span data-ttu-id="e477e-152">Asie a Tichomoří</span><span class="sxs-lookup"><span data-stu-id="e477e-152">Asia Pacific</span></span>|<span data-ttu-id="e477e-153">Indie centrální, jihovýchodní Asie</span><span class="sxs-lookup"><span data-stu-id="e477e-153">India Central, Southeast Asia</span></span>|

### <a name="hello-clouddrive-mount-command"></a><span data-ttu-id="e477e-154">Hello `clouddrive mount` příkaz</span><span class="sxs-lookup"><span data-stu-id="e477e-154">hello `clouddrive mount` command</span></span>

> [!NOTE]
> <span data-ttu-id="e477e-155">Pokud jste připojení nové sdílené složky, vytvoří se nové uživatelské image pro vaše `$Home` adresáře, protože váš předchozí `$Home` bitové kopie je uložen v hello předchozí sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="e477e-155">If you're mounting a new file share, a new user image is created for your `$Home` directory, because your previous `$Home` image is kept in hello previous file share.</span></span>

<span data-ttu-id="e477e-156">Spustit hello `clouddrive mount` s hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="e477e-156">Run hello `clouddrive mount` command with hello following parameters:</span></span>

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

<span data-ttu-id="e477e-157">Spusťte další podrobnosti, tooview `clouddrive mount -h`, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="e477e-157">tooview more details, run `clouddrive mount -h`, as shown here:</span></span>

![Spuštěné hello ' clouddrive mount'command](media/mount-h.png)

## <a name="unmount-clouddrive"></a><span data-ttu-id="e477e-159">Odpojení`clouddrive`</span><span class="sxs-lookup"><span data-stu-id="e477e-159">Unmount `clouddrive`</span></span>
<span data-ttu-id="e477e-160">To je připojený tooCloud prostředí Shell kdykoli sdílené složky můžete odpojit.</span><span class="sxs-lookup"><span data-stu-id="e477e-160">You can unmount a file share that's mounted tooCloud Shell at any time.</span></span> <span data-ttu-id="e477e-161">Po nepřipojené sdílené složky bude výzvami toomount nové tooyour předchozí sdílené složky souboru další relace.</span><span class="sxs-lookup"><span data-stu-id="e477e-161">Once your file share is unmounted, you will be prompted toomount a new file share prior tooyour next session.</span></span>

<span data-ttu-id="e477e-162">tooremove soubor sdílet z prostředí cloudu:</span><span class="sxs-lookup"><span data-stu-id="e477e-162">tooremove a file share from Cloud Shell:</span></span>
1. <span data-ttu-id="e477e-163">Spusťte `clouddrive unmount`.</span><span class="sxs-lookup"><span data-stu-id="e477e-163">Run `clouddrive unmount`.</span></span>
2. <span data-ttu-id="e477e-164">Na vědomí a potvrďte hello výzvy.</span><span class="sxs-lookup"><span data-stu-id="e477e-164">Acknowledge and confirm hello prompts.</span></span>

<span data-ttu-id="e477e-165">Sdílené složky bude pokračovat tooexist, pokud chcete odstranit ručně.</span><span class="sxs-lookup"><span data-stu-id="e477e-165">Your file share will continue tooexist unless you delete it manually.</span></span> <span data-ttu-id="e477e-166">Cloudové prostředí už Hledat pro tuto sdílenou složku na následné relace.</span><span class="sxs-lookup"><span data-stu-id="e477e-166">Cloud Shell will no longer search for this file share on subsequent sessions.</span></span>

<span data-ttu-id="e477e-167">Spusťte další podrobnosti, tooview `clouddrive unmount -h`, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="e477e-167">tooview more details, run `clouddrive unmount -h`, as shown here:</span></span>

![Spuštěné hello ' clouddrive unmount'command](media/unmount-h.png)

> [!WARNING]
> <span data-ttu-id="e477e-169">Spuštění tohoto příkazu se neodstraní žádné prostředky.</span><span class="sxs-lookup"><span data-stu-id="e477e-169">Running this command will not delete any resources.</span></span> <span data-ttu-id="e477e-170">Ručním odstranění skupiny prostředků, účet úložiště nebo sdílené složky, která je namapovaná tooCloud dojde k trvalému odstranění prostředí vaší `$Home` directory image a další soubory do sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="e477e-170">Manually deleting a resource group, storage account, or file share that is mapped tooCloud Shell will permanently delete your `$Home` directory image and any other files in your file share.</span></span> <span data-ttu-id="e477e-171">Tuto akci nelze vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="e477e-171">This action cannot be undone.</span></span>

## <a name="list-clouddrive-file-shares"></a><span data-ttu-id="e477e-172">Seznam `clouddrive` sdílené složky</span><span class="sxs-lookup"><span data-stu-id="e477e-172">List `clouddrive` file shares</span></span>
<span data-ttu-id="e477e-173">které sdílené složky je připojit jako toodiscover `clouddrive`, spusťte následující hello `df` příkaz.</span><span class="sxs-lookup"><span data-stu-id="e477e-173">toodiscover which file share is mounted as `clouddrive`, run hello following `df` command.</span></span> 

<span data-ttu-id="e477e-174">tooclouddrive cesta k souboru Hello ukazuje, že váš název účtu úložiště a sdílených složek v adrese URL hello.</span><span class="sxs-lookup"><span data-stu-id="e477e-174">hello file path tooclouddrive shows your storage account name and file share in hello URL.</span></span> <span data-ttu-id="e477e-175">Například `//storageaccountname.file.core.windows.net/filesharename`.</span><span class="sxs-lookup"><span data-stu-id="e477e-175">For example, `//storageaccountname.file.core.windows.net/filesharename`</span></span>

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

## <a name="transfer-local-files-toocloud-shell"></a><span data-ttu-id="e477e-176">Přenos místních souborů tooCloud prostředí</span><span class="sxs-lookup"><span data-stu-id="e477e-176">Transfer local files tooCloud Shell</span></span>
<span data-ttu-id="e477e-177">Hello `clouddrive` synchronizace adresáře se okno hello portálu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="e477e-177">hello `clouddrive` directory syncs with hello Azure portal storage blade.</span></span> <span data-ttu-id="e477e-178">Pomocí této tooor okno tootransfer místních souborů ze sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="e477e-178">Use this blade tootransfer local files tooor from your file share.</span></span> <span data-ttu-id="e477e-179">Aktualizace soubory z prostředí cloudu se projeví v úložišti file hello grafického uživatelského rozhraní při aktualizaci okna hello.</span><span class="sxs-lookup"><span data-stu-id="e477e-179">Updating files from within Cloud Shell is reflected in hello file storage GUI when you refresh hello blade.</span></span>

### <a name="download-files"></a><span data-ttu-id="e477e-180">Stažení souborů</span><span class="sxs-lookup"><span data-stu-id="e477e-180">Download files</span></span>

![Seznam místních souborů](media/download.png)
1. <span data-ttu-id="e477e-182">V hello portálu Azure přejděte toohello připojené sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="e477e-182">In hello Azure portal, go toohello mounted file share.</span></span>
2. <span data-ttu-id="e477e-183">Vyberte cílový soubor hello.</span><span class="sxs-lookup"><span data-stu-id="e477e-183">Select hello target file.</span></span>
3. <span data-ttu-id="e477e-184">Vyberte hello **Stáhnout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e477e-184">Select hello **Download** button.</span></span>

### <a name="upload-files"></a><span data-ttu-id="e477e-185">Nahrání souborů</span><span class="sxs-lookup"><span data-stu-id="e477e-185">Upload files</span></span>

![Nahrát toobe místních souborů](media/upload.png)
1. <span data-ttu-id="e477e-187">Přejděte tooyour připojit sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="e477e-187">Go tooyour mounted file share.</span></span>
2. <span data-ttu-id="e477e-188">Vyberte hello **nahrát** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e477e-188">Select hello **Upload** button.</span></span>
3. <span data-ttu-id="e477e-189">Vyberte soubory, které chcete tooupload, hello.</span><span class="sxs-lookup"><span data-stu-id="e477e-189">Select hello file or files that you want tooupload.</span></span>
4. <span data-ttu-id="e477e-190">Potvrďte nahrávání hello.</span><span class="sxs-lookup"><span data-stu-id="e477e-190">Confirm hello upload.</span></span>

<span data-ttu-id="e477e-191">Teď byste měli vidět hello soubory, které jsou dostupné ve vaší `clouddrive` adresář v prostředí cloudu.</span><span class="sxs-lookup"><span data-stu-id="e477e-191">You should now see hello files that are accessible in your `clouddrive` directory in Cloud Shell.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e477e-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e477e-192">Next steps</span></span>
[<span data-ttu-id="e477e-193">Rychlý start cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="e477e-193">Cloud Shell Quickstart</span></span>](quickstart.md) <br><span data-ttu-id="e477e-194">
[Další informace o Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage)</span><span class="sxs-lookup"><span data-stu-id="e477e-194">
[Learn about Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage)</span></span> <br><span data-ttu-id="e477e-195">
[Další informace o značkách úložiště](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)</span><span class="sxs-lookup"><span data-stu-id="e477e-195">
[Learn about storage tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)</span></span> <br>
