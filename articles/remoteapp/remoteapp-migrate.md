---
title: "Migrace dat uživatele z Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak migrovat uživatelská data do/z Azure Remoteappu."
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
ms.openlocfilehash: ba3cf4c6834279bbd7f94d666fd8abbb7ac05bf0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-migrate-data-into-and-out-of-azure-remoteapp"></a><span data-ttu-id="4bc6f-103">Migrace dat do a z Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="4bc6f-103">How to migrate data into and out of Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="4bc6f-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="4bc6f-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="4bc6f-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="4bc6f-106">Můžete použít řadu různých nástrojů a metody pro přenos [uživatelská data](remoteapp-upd.md) do a z Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-106">You can use many different tools and methods to transfer [user data](remoteapp-upd.md) into and out of Azure RemoteApp.</span></span> <span data-ttu-id="4bc6f-107">Tady je několik metod:</span><span class="sxs-lookup"><span data-stu-id="4bc6f-107">Here are a few methods:</span></span>

* <span data-ttu-id="4bc6f-108">Zkopírujte a vložte pomocí sdílení schránky</span><span class="sxs-lookup"><span data-stu-id="4bc6f-108">Copy and paste using clipboard sharing</span></span>
* <span data-ttu-id="4bc6f-109">Zkopírujte soubory a data na souborovém serveru</span><span class="sxs-lookup"><span data-stu-id="4bc6f-109">Copy files and data to a file server</span></span>
* <span data-ttu-id="4bc6f-110">Zkopírujte soubory na OneDrive pro firmy prostřednictvím prohlížeče</span><span class="sxs-lookup"><span data-stu-id="4bc6f-110">Copy files to OneDrive for Business through a browser</span></span>
* <span data-ttu-id="4bc6f-111">Zkopírujte soubory pomocí přesměrování</span><span class="sxs-lookup"><span data-stu-id="4bc6f-111">Copy files using redirection</span></span>

> [!NOTE]
> <span data-ttu-id="4bc6f-112">Nelze povolit Onedrivu pro firmy nebo příjemce synchronizace agenty - jejich [nejsou podporovány](remoteapp-onedrive.md) v Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-112">You cannot enable the OneDrive for Business or Consumer sync agents - they [are not supported](remoteapp-onedrive.md) in Azure RemoteApp.</span></span>
> 
> 

## <a name="use-copy-and-paste-in-file-explorer"></a><span data-ttu-id="4bc6f-113">Kopírování a vkládání v Průzkumníku souborů</span><span class="sxs-lookup"><span data-stu-id="4bc6f-113">Use copy and paste in File Explorer</span></span>
<span data-ttu-id="4bc6f-114">Kopírování a vkládání použití schránky je povolena v nasazeních vzdálené aplikace RemoteApp [ve výchozím nastavení](remoteapp-redirection.md).</span><span class="sxs-lookup"><span data-stu-id="4bc6f-114">Copy and paste using the clipboard is enabled in RemoteApp deployments [by default](remoteapp-redirection.md).</span></span> <span data-ttu-id="4bc6f-115">To umožňuje uživatelům kopírovat soubory mezi místním počítači a vzdálené aplikace RemoteApp aplikace.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-115">This lets users copy files between their local PC and RemoteApp apps.</span></span> <span data-ttu-id="4bc6f-116">Často prostřednictvím běžném průběhu používání aplikace v Remoteappu, uživatelé uložit soubory k jejich UPD - přesunutí, že je snadné data ze vzdálené aplikace RemoteApp:</span><span class="sxs-lookup"><span data-stu-id="4bc6f-116">Often, through the normal course of using apps in RemoteApp, users have saved files to their UPDs - moving that data out of RemoteApp is easy:</span></span>

1. <span data-ttu-id="4bc6f-117">[Publikování Průzkumníka souborů jako aplikace](remoteapp-publish.md) v kolekci vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-117">[Publish File Explorer as an app](remoteapp-publish.md) in a RemoteApp collection.</span></span> <span data-ttu-id="4bc6f-118">(Všimněte si, že toto je Správce úloh.)</span><span class="sxs-lookup"><span data-stu-id="4bc6f-118">(Note that this is an administrative task.)</span></span>
2. <span data-ttu-id="4bc6f-119">Směrovat své uživatele, spusťte aplikaci Průzkumník souborů, které jste publikovali a použít pro kopírování a vkládání souborů do jejich UPD i mimo ho.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-119">Direct your users to launch the File Explorer app you published and to use that to copy and paste files both into their UPD and out of it.</span></span>

## <a name="upload-files-and-data-to-a-file-server-by-using-standard-network-file-copy"></a><span data-ttu-id="4bc6f-120">Nahrajte soubory a data na souborový server s použitím standardního síťového kopírování souborů</span><span class="sxs-lookup"><span data-stu-id="4bc6f-120">Upload files and data to a file server by using standard network file copy</span></span>
<span data-ttu-id="4bc6f-121">Často organizace používat k ukládání dat obecné souborové servery.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-121">Often organizations use file servers to store general data.</span></span> <span data-ttu-id="4bc6f-122">Pokud znáte název serveru nebo umístění, můžete uživatelům procházet místní sítě pro server a poté zkopírujte své soubory, jako tomu bylo výše.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-122">If you know the server name or location, your users can browse the local network for the server and then copy their files there, much like they did above.</span></span> <span data-ttu-id="4bc6f-123">Můžete znovu publikovat Průzkumníka souborů do vzdálené aplikace RemoteApp a sdílet je s uživateli.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-123">Again you'll want to publish File Explorer to RemoteApp and then share it with your users.</span></span>

> [!NOTE]
> <span data-ttu-id="4bc6f-124">Souborový server musí být na směrovatelné síti, který byl nasazen vzdálené aplikace RemoteApp do.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-124">The file server must be on the routable network that RemoteApp was deployed into.</span></span>
> 
> 

## <a name="copy-files-to-onedrive-for-business"></a><span data-ttu-id="4bc6f-125">Zkopírujte soubory na OneDrive pro firmy</span><span class="sxs-lookup"><span data-stu-id="4bc6f-125">Copy files to OneDrive for Business</span></span>
<span data-ttu-id="4bc6f-126">I když Onedrivu pro firmy agenta synchronizace v Remoteappu nelze povolit, můžete kopírovat soubory z vaší UPD na OneDrive pro firmy prostřednictvím prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-126">Although you cannot enable the OneDrive for Business sync agent in RemoteApp, you can still copy files from your UPD to OneDrive for Business through a browser.</span></span> 

1. <span data-ttu-id="4bc6f-127">Publikování Průzkumníka souborů do vzdálené aplikace RemoteApp a pak říct uživatelům přístup k souborům prostřednictvím této aplikace.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-127">Publish File Explorer to RemoteApp and then tell users to access the files through that app.</span></span> 
2. <span data-ttu-id="4bc6f-128">Je to nejjednodušší provádět přenos souborů, pokud byla komprimovaná, aby uživatelé měli vytvořit soubor .zip, který obsahuje všechny soubory pro přesun do Onedrivu pro firmy.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-128">It's easiest to transfer files if they are compressed, so users should create a .zip file that contains all of the files to move to OneDrive for Business.</span></span>
3. <span data-ttu-id="4bc6f-129">Požádejte uživatele, přejděte na portál Office 365 a pak přejděte na OneDrive a nahrát soubor .zip.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-129">Ask users to go to the Office 365 portal, and then go to OneDrive and upload the .zip file.</span></span>

## <a name="copy-files-by-using-drive-redirection"></a><span data-ttu-id="4bc6f-130">Zkopírujte soubory pomocí přesměrování jednotky</span><span class="sxs-lookup"><span data-stu-id="4bc6f-130">Copy files by using drive redirection</span></span>
<span data-ttu-id="4bc6f-131">Pokud jste povolili [jednotka přesměrování](remoteapp-redirection.md), jste již vytvořili mapované jednotky pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-131">If you have enabled [drive redirection](remoteapp-redirection.md), you have already created a mapped drive for your users.</span></span> <span data-ttu-id="4bc6f-132">V takovém případě můžete své soubory na tato jednotka zip a uložit je do svého místního počítače.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-132">In this case, they can zip their files on the redirected drive and then save them to their local PC.</span></span>

## <a name="how-administrators-can-export-data"></a><span data-ttu-id="4bc6f-133">Jak mohou správci exportovat data</span><span class="sxs-lookup"><span data-stu-id="4bc6f-133">How administrators can export data</span></span>

<span data-ttu-id="4bc6f-134">Všechny kolekce v rámci předplatného pro Azure Storage pomocí rutiny Azure Powershellu, Export AzureRemoteAppUserDisk slouží ke správě pro Azure RemoteApp můžete exportovat všechny disky profilu uživatele (UPD).</span><span class="sxs-lookup"><span data-stu-id="4bc6f-134">Administers for Azure RemoteApp can export all user profile disks (UPD) for all collections within a subscription to Azure Storage using Azure PowerShell cmdlet, Export-AzureRemoteAppUserDisk.</span></span>  <span data-ttu-id="4bc6f-135">Neexistuje žádná možnost vybrat jednotlivé UPD.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-135">There is no ability to select individual UPD's.</span></span>  <span data-ttu-id="4bc6f-136">Když je proveden příkaz prostředí PowerShell, každého disku, uživatel bude 50gb velikost pevného disku a exportovat do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-136">When the PowerShell command is executed, each user disk will be a 50gb in fixed disk size and be exported to Azure storage.</span></span>  <span data-ttu-id="4bc6f-137">Pro toto úložiště je možné hned za náklady na úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-137">Costs of Azure storage will incur immediately for this storage.</span></span>  <span data-ttu-id="4bc6f-138">Při spuštění tohoto příkazu zkontrolujte neexistují žádné relace, jinak hodnota export se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-138">When running this command ensure there are no sessions otherwise the export will fail.</span></span>

<span data-ttu-id="4bc6f-139">UPD je připojený k doméně Azure RemoteApp nasazení lze použít pouze znovu v nasazení služby Vzdálená plocha, nelze použít nasazení připojený k doméně.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-139">UPD's for domain joined Azure RemoteApp deployments can only be used again in an RDS deployment, non-domain joined deployments cannot be used.</span></span>  <span data-ttu-id="4bc6f-140">Pokud tyto disky se použije v nasazení služby Vzdálená plocha doporučujeme používat naše [automatizované skripty](https://github.com/arcadiahlyy/aramigration) , bude exportovat, převod a import UPD do nasazení služby Vzdálená plocha.</span><span class="sxs-lookup"><span data-stu-id="4bc6f-140">If these disks will be used in an RDS deployment we recommend to use our [automated scripts](https://github.com/arcadiahlyy/aramigration) that will export, convert, and import the UPD's into an RDS deployment.</span></span>

