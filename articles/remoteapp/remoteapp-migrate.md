---
title: "aaaMigrate uživatelská data z Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak toomigrate uživatelská data do/z Azure Remoteappu."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: d7e4fbf1-cb42-4430-94a0-ed6d4676fc86
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: aefc6ccc2c6173754acf6cad06102f27c8cb1d26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomigrate-data-into-and-out-of-azure-remoteapp"></a><span data-ttu-id="67bf5-103">Jak toomigrate dat do a z Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="67bf5-103">How toomigrate data into and out of Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="67bf5-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="67bf5-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="67bf5-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="67bf5-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="67bf5-106">Můžete použít řadu různých nástrojů a metody tootransfer [uživatelská data](remoteapp-upd.md) do a z Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="67bf5-106">You can use many different tools and methods tootransfer [user data](remoteapp-upd.md) into and out of Azure RemoteApp.</span></span> <span data-ttu-id="67bf5-107">Tady je několik metod:</span><span class="sxs-lookup"><span data-stu-id="67bf5-107">Here are a few methods:</span></span>

* <span data-ttu-id="67bf5-108">Zkopírujte a vložte pomocí sdílení schránky</span><span class="sxs-lookup"><span data-stu-id="67bf5-108">Copy and paste using clipboard sharing</span></span>
* <span data-ttu-id="67bf5-109">Zkopírujte soubory a data tooa souborového serveru</span><span class="sxs-lookup"><span data-stu-id="67bf5-109">Copy files and data tooa file server</span></span>
* <span data-ttu-id="67bf5-110">Zkopírujte soubory tooOneDrive pro firmy prostřednictvím prohlížeče</span><span class="sxs-lookup"><span data-stu-id="67bf5-110">Copy files tooOneDrive for Business through a browser</span></span>
* <span data-ttu-id="67bf5-111">Zkopírujte soubory pomocí přesměrování</span><span class="sxs-lookup"><span data-stu-id="67bf5-111">Copy files using redirection</span></span>

> [!NOTE]
> <span data-ttu-id="67bf5-112">Nelze povolit hello OneDrive pro firmy nebo příjemce synchronizace agenty - jejich [nejsou podporovány](remoteapp-onedrive.md) v Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="67bf5-112">You cannot enable hello OneDrive for Business or Consumer sync agents - they [are not supported](remoteapp-onedrive.md) in Azure RemoteApp.</span></span>
> 
> 

## <a name="use-copy-and-paste-in-file-explorer"></a><span data-ttu-id="67bf5-113">Kopírování a vkládání v Průzkumníku souborů</span><span class="sxs-lookup"><span data-stu-id="67bf5-113">Use copy and paste in File Explorer</span></span>
<span data-ttu-id="67bf5-114">Kopírování a vkládání použití schránky hello je povolena v nasazeních vzdálené aplikace RemoteApp [ve výchozím nastavení](remoteapp-redirection.md).</span><span class="sxs-lookup"><span data-stu-id="67bf5-114">Copy and paste using hello clipboard is enabled in RemoteApp deployments [by default](remoteapp-redirection.md).</span></span> <span data-ttu-id="67bf5-115">To umožňuje uživatelům kopírovat soubory mezi místním počítači a vzdálené aplikace RemoteApp aplikace.</span><span class="sxs-lookup"><span data-stu-id="67bf5-115">This lets users copy files between their local PC and RemoteApp apps.</span></span> <span data-ttu-id="67bf5-116">Často prostřednictvím hello běžném průběhu používání aplikace v Remoteappu, uživatelé uložit soubory tootheir UPD - přesunutí, že je snadné data ze vzdálené aplikace RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="67bf5-116">Often, through hello normal course of using apps in RemoteApp, users have saved files tootheir UPDs - moving that data out of RemoteApp is easy:</span></span>

1. <span data-ttu-id="67bf5-117">[Publikování Průzkumníka souborů jako aplikace](remoteapp-publish.md) v kolekci vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="67bf5-117">[Publish File Explorer as an app](remoteapp-publish.md) in a RemoteApp collection.</span></span> <span data-ttu-id="67bf5-118">(Všimněte si, že toto je Správce úloh.)</span><span class="sxs-lookup"><span data-stu-id="67bf5-118">(Note that this is an administrative task.)</span></span>
2. <span data-ttu-id="67bf5-119">Přímé uživatelé toolaunch hello Průzkumníka souborů aplikace, kterou jste publikovali a toouse této toocopy a vkládání souborů do jejich UPD i mimo ho.</span><span class="sxs-lookup"><span data-stu-id="67bf5-119">Direct your users toolaunch hello File Explorer app you published and toouse that toocopy and paste files both into their UPD and out of it.</span></span>

## <a name="upload-files-and-data-tooa-file-server-by-using-standard-network-file-copy"></a><span data-ttu-id="67bf5-120">Odeslání souborů a dat tooa souborový server s použitím standardního síťového kopírování souborů</span><span class="sxs-lookup"><span data-stu-id="67bf5-120">Upload files and data tooa file server by using standard network file copy</span></span>
<span data-ttu-id="67bf5-121">Často organizace používat soubor servery toostore obecné data.</span><span class="sxs-lookup"><span data-stu-id="67bf5-121">Often organizations use file servers toostore general data.</span></span> <span data-ttu-id="67bf5-122">Pokud znáte název serveru hello nebo umístění, můžete uživatelům procházet hello místní sítě pro hello server a poté zkopírujte své soubory, jako tomu bylo výše.</span><span class="sxs-lookup"><span data-stu-id="67bf5-122">If you know hello server name or location, your users can browse hello local network for hello server and then copy their files there, much like they did above.</span></span> <span data-ttu-id="67bf5-123">Budete znovu chcete tooRemoteApp toopublish Průzkumníka souborů a sdílet je s uživateli.</span><span class="sxs-lookup"><span data-stu-id="67bf5-123">Again you'll want toopublish File Explorer tooRemoteApp and then share it with your users.</span></span>

> [!NOTE]
> <span data-ttu-id="67bf5-124">Hello souborový server musí být na hello směrovatelné síti, který byl nasazen vzdálené aplikace RemoteApp do.</span><span class="sxs-lookup"><span data-stu-id="67bf5-124">hello file server must be on hello routable network that RemoteApp was deployed into.</span></span>
> 
> 

## <a name="copy-files-tooonedrive-for-business"></a><span data-ttu-id="67bf5-125">Zkopírujte soubory tooOneDrive pro firmy</span><span class="sxs-lookup"><span data-stu-id="67bf5-125">Copy files tooOneDrive for Business</span></span>
<span data-ttu-id="67bf5-126">I když hello OneDrive pro firmy agenta synchronizace v Remoteappu nelze povolit, můžete kopírovat soubory z vaší tooOneDrive UPD pro firmy prostřednictvím prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="67bf5-126">Although you cannot enable hello OneDrive for Business sync agent in RemoteApp, you can still copy files from your UPD tooOneDrive for Business through a browser.</span></span> 

1. <span data-ttu-id="67bf5-127">Publikování tooRemoteApp Průzkumníka souborů a potom říct, že uživatelé tooaccess hello soubory pomocí této aplikace.</span><span class="sxs-lookup"><span data-stu-id="67bf5-127">Publish File Explorer tooRemoteApp and then tell users tooaccess hello files through that app.</span></span> 
2. <span data-ttu-id="67bf5-128">Je nejjednodušší soubory tootransfer Pokud byla komprimovaná, aby uživatelé měli vytvořit soubor .zip, který obsahuje všechny tooOneDrive toomove soubory hello pro firmy.</span><span class="sxs-lookup"><span data-stu-id="67bf5-128">It's easiest tootransfer files if they are compressed, so users should create a .zip file that contains all of hello files toomove tooOneDrive for Business.</span></span>
3. <span data-ttu-id="67bf5-129">Požádejte uživatele toogo toohello Office 365 portál a potom přejděte tooOneDrive a nahrajte soubor ZIP hello.</span><span class="sxs-lookup"><span data-stu-id="67bf5-129">Ask users toogo toohello Office 365 portal, and then go tooOneDrive and upload hello .zip file.</span></span>

## <a name="copy-files-by-using-drive-redirection"></a><span data-ttu-id="67bf5-130">Zkopírujte soubory pomocí přesměrování jednotky</span><span class="sxs-lookup"><span data-stu-id="67bf5-130">Copy files by using drive redirection</span></span>
<span data-ttu-id="67bf5-131">Pokud jste povolili [jednotka přesměrování](remoteapp-redirection.md), jste již vytvořili mapované jednotky pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="67bf5-131">If you have enabled [drive redirection](remoteapp-redirection.md), you have already created a mapped drive for your users.</span></span> <span data-ttu-id="67bf5-132">V takovém případě můžete své soubory na disku hello přesměrováno zip a uložit je tootheir místní počítač.</span><span class="sxs-lookup"><span data-stu-id="67bf5-132">In this case, they can zip their files on hello redirected drive and then save them tootheir local PC.</span></span>

## <a name="how-administrators-can-export-data"></a><span data-ttu-id="67bf5-133">Jak mohou správci exportovat data</span><span class="sxs-lookup"><span data-stu-id="67bf5-133">How administrators can export data</span></span>

<span data-ttu-id="67bf5-134">Spravuje pro Azure RemoteApp můžete exportovat všechny disky profilu uživatele (UPD) pro všechny kolekce v rámci předplatného tooAzure úložiště pomocí prostředí Azure PowerShell rutiny Export-AzureRemoteAppUserDisk.</span><span class="sxs-lookup"><span data-stu-id="67bf5-134">Administers for Azure RemoteApp can export all user profile disks (UPD) for all collections within a subscription tooAzure Storage using Azure PowerShell cmdlet, Export-AzureRemoteAppUserDisk.</span></span>  <span data-ttu-id="67bf5-135">Neexistuje žádná možnost tooselect jednotlivých UPD společnosti.</span><span class="sxs-lookup"><span data-stu-id="67bf5-135">There is no ability tooselect individual UPD's.</span></span>  <span data-ttu-id="67bf5-136">Po provedení hello příkaz prostředí PowerShell každého disku, uživatel bude 50gb velikost pevného disku a být exportovaný tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="67bf5-136">When hello PowerShell command is executed, each user disk will be a 50gb in fixed disk size and be exported tooAzure storage.</span></span>  <span data-ttu-id="67bf5-137">Pro toto úložiště je možné hned za náklady na úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="67bf5-137">Costs of Azure storage will incur immediately for this storage.</span></span>  <span data-ttu-id="67bf5-138">Při spuštění tohoto příkazu zkontrolujte neexistují žádné relace, jinak export hello selžou.</span><span class="sxs-lookup"><span data-stu-id="67bf5-138">When running this command ensure there are no sessions otherwise hello export will fail.</span></span>

<span data-ttu-id="67bf5-139">UPD je připojený k doméně Azure RemoteApp nasazení lze použít pouze znovu v nasazení služby Vzdálená plocha, nelze použít nasazení připojený k doméně.</span><span class="sxs-lookup"><span data-stu-id="67bf5-139">UPD's for domain joined Azure RemoteApp deployments can only be used again in an RDS deployment, non-domain joined deployments cannot be used.</span></span>  <span data-ttu-id="67bf5-140">Pokud tyto disky se použije v nasazení služby Vzdálená plocha doporučujeme toouse naše [automatizované skripty](https://github.com/arcadiahlyy/aramigration) , bude exportovat, převod a import hello na UPD do nasazení služby Vzdálená plocha.</span><span class="sxs-lookup"><span data-stu-id="67bf5-140">If these disks will be used in an RDS deployment we recommend toouse our [automated scripts](https://github.com/arcadiahlyy/aramigration) that will export, convert, and import hello UPD's into an RDS deployment.</span></span>

