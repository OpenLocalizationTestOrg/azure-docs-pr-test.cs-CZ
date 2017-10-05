---
title: "Nikdy ukládat citlivá data na vlastních bitových kopií pro Azure Remoteappu | Microsoft Docs"
description: "Další informace o pokyny pro ukládání dat do vlastních bitových kopií v Azure Remoteappu"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 5a19903b-15f9-49d9-9bc1-ae80f2671aa1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 75d5415d33324d957617426e75909a6c6c58b1f9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="never-store-sensitive-data-on-custom-images"></a><span data-ttu-id="829f9-103">Nikdy ukládat citlivá data na vlastních bitových kopií</span><span class="sxs-lookup"><span data-stu-id="829f9-103">Never store sensitive data on custom images</span></span>
> [!IMPORTANT]
> <span data-ttu-id="829f9-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="829f9-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="829f9-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="829f9-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="829f9-106">Pokud je hostitelem vaší vlastní aplikaci v Azure Remoteappu, prvním krokem je vytvoření vlastní image.</span><span class="sxs-lookup"><span data-stu-id="829f9-106">When you host your own application in Azure RemoteApp, the first step is to create a custom image.</span></span> <span data-ttu-id="829f9-107">Pro vytvoření instancí virtuálních počítačů, které slouží aplikace uživatelům používáme této vlastní bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="829f9-107">We use that custom image to create VM instances that serve your apps to your users.</span></span> <span data-ttu-id="829f9-108">Vlastní image by měly obsahovat jenom aplikace a nikdy citlivá data, která může dojít ke ztrátě, například SQL databáze, soubory pracovníky nebo speciální datových souborů, jako jsou soubory QuickBooks společnosti.</span><span class="sxs-lookup"><span data-stu-id="829f9-108">The custom image should contain ONLY applications and never sensitive data that can be lost, such as SQL databases, personnel files, or special data files like QuickBooks company files.</span></span> <span data-ttu-id="829f9-109">Všechny citlivá data by měl být umístěn mimo Azure RemoteApp na souborovém serveru, jiným virtuálním Počítačem Azure, nebo v SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="829f9-109">All sensitive data should reside external to Azure RemoteApp on a file server, another Azure VM, or in SQL Azure.</span></span> <span data-ttu-id="829f9-110">Bitovou kopii by měl hostitel jenom aplikace, která se připojuje ke zdroji dat a představuje data.</span><span class="sxs-lookup"><span data-stu-id="829f9-110">The image should just host the application that connects to the data source and presents the data.</span></span> <span data-ttu-id="829f9-111">Zkontrolujte [požadavky pro Azure RemoteApp image](remoteapp-imagereqs.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="829f9-111">Review [Requirements for Azure RemoteApp images](remoteapp-imagereqs.md) for more information.</span></span> 

<span data-ttu-id="829f9-112">Chcete-li pochopit, proč by neměla ukládat citlivá data, je potřeba pochopit, jak funguje Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="829f9-112">To understand why you should not store sensitive data, you need to understand how Azure RemoteApp works.</span></span> <span data-ttu-id="829f9-113">Při vytvoření nebo aktualizaci na pozadí více klonech nebo se obrázek kolekce se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="829f9-113">When a collection is created or updated, behind the scenes multiple clones or copies of the image are created.</span></span> <span data-ttu-id="829f9-114">Všechny tyto instance virtuálních počítačů jsou přesný replik vlastní Image; Když uživatelé spustí aplikace jsou připojeny k jednu z těchto instancí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="829f9-114">All these VM instances are exact replicas of the custom image; when users launch applications they are connected to one of these VM instances.</span></span> <span data-ttu-id="829f9-115">Ale na stejnou instanci není zaručena a neměli důležité, protože jsou dočasnou.</span><span class="sxs-lookup"><span data-stu-id="829f9-115">But the same instance is not guaranteed and should not matter because they are non-persistent.</span></span> <span data-ttu-id="829f9-116">Instance virtuálních počítačů, který je hostitelem aplikace jsou dočasnou a lze zničen nebo odstranit na základě, například během aktualizace kolekce.</span><span class="sxs-lookup"><span data-stu-id="829f9-116">The VM instances hosting the applications are non-persistent and can be destroyed or deleted based, for example, during collection update.</span></span> 

<span data-ttu-id="829f9-117">Jakmile kolekce je zřízený a uživatelé začít připojení k virtuálním počítačům, data uživatele je trvalé a chránit, protože je uložen na samostatné úložiště v rámci virtuální pevný disk, který nazýváme [disk profilu uživatele (UPD)](remoteapp-upd.md), což je profilu uživatele v c:\users\<userprofile >.</span><span class="sxs-lookup"><span data-stu-id="829f9-117">Once the collection is provisioned and users start connecting to the VMs, user data is persistent and protected because it is saved on separate storage within a VHD that we call a [user profile disk (UPD)](remoteapp-upd.md), which is the user profile in c:\users\<userprofile>.</span></span> <span data-ttu-id="829f9-118">Při spuštění aplikace, UPD je připojena a zpracováván jako místní uživatelský profil operačního systému.</span><span class="sxs-lookup"><span data-stu-id="829f9-118">When an application starts, the UPD is mounted and treated just like a local user profile by the operating system.</span></span> <span data-ttu-id="829f9-119">Další informace o [Azure RemoteApp uloží uživatelských dat a nastavení](remoteapp-upd.md).</span><span class="sxs-lookup"><span data-stu-id="829f9-119">Read more about how [Azure RemoteApp saves user data and settings](remoteapp-upd.md).</span></span>

<span data-ttu-id="829f9-120">Příklad data, která se nesmí nacházet v bitové kopii:</span><span class="sxs-lookup"><span data-stu-id="829f9-120">Example data that should not reside in the image:</span></span>

* <span data-ttu-id="829f9-121">Sdílená data uživatelům přístup k</span><span class="sxs-lookup"><span data-stu-id="829f9-121">Shared data for users to access</span></span>
* <span data-ttu-id="829f9-122">Databázi SQL nebo QuickBooks DB</span><span class="sxs-lookup"><span data-stu-id="829f9-122">SQL DB or QuickBooks DB</span></span>
* <span data-ttu-id="829f9-123">Všechna data v D:\\</span><span class="sxs-lookup"><span data-stu-id="829f9-123">Any data in D:\\</span></span>

<span data-ttu-id="829f9-124">Příklad data, která mohou být umístěny v výchozí profil zkopírovat do UPD každých uživatelů:</span><span class="sxs-lookup"><span data-stu-id="829f9-124">Example data that can reside in the default profile to be copied into every users’ UPD:</span></span>

* <span data-ttu-id="829f9-125">Konfigurační soubory na uživatele</span><span class="sxs-lookup"><span data-stu-id="829f9-125">Configuration files per user</span></span>
* <span data-ttu-id="829f9-126">Skripty, které by uživatelé potřebují zachovaná v jejich UPD</span><span class="sxs-lookup"><span data-stu-id="829f9-126">Scripts that users would need preserved in their UPD</span></span>

<span data-ttu-id="829f9-127">Klíčové body:</span><span class="sxs-lookup"><span data-stu-id="829f9-127">Key points:</span></span>

* <span data-ttu-id="829f9-128">Nikdy úložiště citlivá data, která může dojít ke ztrátě na bitovou kopii při vytváření vlastní image.</span><span class="sxs-lookup"><span data-stu-id="829f9-128">Never store sensitive data that can be lost on the image when creating a custom image.</span></span>
* <span data-ttu-id="829f9-129">Citlivá data vždy by měl být umístěn na samostatném souborovém serveru, samostatný virtuální počítač Azure, v cloudu a externí vždy k hostování vaší aplikace v rámci Azure RemoteApp instance virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="829f9-129">Sensitive data should always reside on a separate file server, separate Azure VM, on the cloud, and always external to the VM instances hosting your applications within Azure RemoteApp.</span></span> 
* <span data-ttu-id="829f9-130">Uživatelská data budou uložena a přetrvává v disk profilu uživatele (UPD)</span><span class="sxs-lookup"><span data-stu-id="829f9-130">User data is saved and persists in the user profile disk (UPD)</span></span>

