---
title: "aaaNever úložiště citlivá data v vlastních bitových kopií pro Azure Remoteappu | Microsoft Docs"
description: "Další informace o hello pokyny pro ukládání dat do vlastních bitových kopií v Azure Remoteappu"
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
ms.openlocfilehash: 86a0fea218f8826d6d25f50d3c4c36e368e26fb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="never-store-sensitive-data-on-custom-images"></a><span data-ttu-id="5946e-103">Nikdy ukládat citlivá data na vlastních bitových kopií</span><span class="sxs-lookup"><span data-stu-id="5946e-103">Never store sensitive data on custom images</span></span>
> [!IMPORTANT]
> <span data-ttu-id="5946e-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="5946e-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="5946e-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="5946e-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="5946e-106">Pokud je hostitelem vaší vlastní aplikaci v Azure Remoteappu, hello prvním krokem je toocreate vlastní image.</span><span class="sxs-lookup"><span data-stu-id="5946e-106">When you host your own application in Azure RemoteApp, hello first step is toocreate a custom image.</span></span> <span data-ttu-id="5946e-107">Jsme použít této instance virtuálních počítačů toocreate vlastní image, které slouží tooyour uživatelů vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5946e-107">We use that custom image toocreate VM instances that serve your apps tooyour users.</span></span> <span data-ttu-id="5946e-108">vlastní image Hello by měly obsahovat jenom aplikace a nikdy citlivá data, která může dojít ke ztrátě, například SQL databáze, soubory pracovníky nebo speciální datových souborů, jako jsou soubory QuickBooks společnosti.</span><span class="sxs-lookup"><span data-stu-id="5946e-108">hello custom image should contain ONLY applications and never sensitive data that can be lost, such as SQL databases, personnel files, or special data files like QuickBooks company files.</span></span> <span data-ttu-id="5946e-109">Všechny citlivá data by měl být umístěn externí tooAzure vzdálené aplikace RemoteApp na souborovém serveru, jiným virtuálním Počítačem Azure, nebo v SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="5946e-109">All sensitive data should reside external tooAzure RemoteApp on a file server, another Azure VM, or in SQL Azure.</span></span> <span data-ttu-id="5946e-110">Obrázek Hello má právě aplikace hello hostitele, která připojí toohello zdroj dat a uvede hello data.</span><span class="sxs-lookup"><span data-stu-id="5946e-110">hello image should just host hello application that connects toohello data source and presents hello data.</span></span> <span data-ttu-id="5946e-111">Zkontrolujte [požadavky pro Azure RemoteApp image](remoteapp-imagereqs.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="5946e-111">Review [Requirements for Azure RemoteApp images](remoteapp-imagereqs.md) for more information.</span></span> 

<span data-ttu-id="5946e-112">toounderstand, proč by neměla ukládat citlivá data, je nutné toounderstand jak funguje Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="5946e-112">toounderstand why you should not store sensitive data, you need toounderstand how Azure RemoteApp works.</span></span> <span data-ttu-id="5946e-113">Kolekce se vytvoří nebo aktualizuje, pozadí hello více klony nebo kopie Image hello vytvářejí.</span><span class="sxs-lookup"><span data-stu-id="5946e-113">When a collection is created or updated, behind hello scenes multiple clones or copies of hello image are created.</span></span> <span data-ttu-id="5946e-114">Všechny tyto instance virtuálních počítačů jsou přesný repliky hello vlastní image; Když uživatelé spustí aplikace jsou připojené tooone instancí těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5946e-114">All these VM instances are exact replicas of hello custom image; when users launch applications they are connected tooone of these VM instances.</span></span> <span data-ttu-id="5946e-115">Ale hello stejnou instanci není zaručena a neměli důležité, protože jsou dočasnou.</span><span class="sxs-lookup"><span data-stu-id="5946e-115">But hello same instance is not guaranteed and should not matter because they are non-persistent.</span></span> <span data-ttu-id="5946e-116">Hello instancí virtuálních počítačů v hostitelských aplikací hello trvalé a může být zničený, nebo odstranit na základě, například během aktualizace kolekce.</span><span class="sxs-lookup"><span data-stu-id="5946e-116">hello VM instances hosting hello applications are non-persistent and can be destroyed or deleted based, for example, during collection update.</span></span> 

<span data-ttu-id="5946e-117">Jakmile hello kolekce je zřízený a uživatelé začít připojování toohello virtuálních počítačů, uživatelská data je trvalé a chránit, protože je uložen na samostatné úložiště v rámci virtuální pevný disk, který nazýváme [disk profilu uživatele (UPD)](remoteapp-upd.md), což je hello uživatelský profil v c:\users\<userprofile >.</span><span class="sxs-lookup"><span data-stu-id="5946e-117">Once hello collection is provisioned and users start connecting toohello VMs, user data is persistent and protected because it is saved on separate storage within a VHD that we call a [user profile disk (UPD)](remoteapp-upd.md), which is hello user profile in c:\users\<userprofile>.</span></span> <span data-ttu-id="5946e-118">Při spuštění aplikace, hello UPD je připojena a zpracováván jako místní uživatelský profil hello operačním systému.</span><span class="sxs-lookup"><span data-stu-id="5946e-118">When an application starts, hello UPD is mounted and treated just like a local user profile by hello operating system.</span></span> <span data-ttu-id="5946e-119">Další informace o [Azure RemoteApp uloží uživatelských dat a nastavení](remoteapp-upd.md).</span><span class="sxs-lookup"><span data-stu-id="5946e-119">Read more about how [Azure RemoteApp saves user data and settings](remoteapp-upd.md).</span></span>

<span data-ttu-id="5946e-120">Příklad data, která se nesmí nacházet v bitové kopii hello:</span><span class="sxs-lookup"><span data-stu-id="5946e-120">Example data that should not reside in hello image:</span></span>

* <span data-ttu-id="5946e-121">Sdílených dat pro tooaccess uživatelů</span><span class="sxs-lookup"><span data-stu-id="5946e-121">Shared data for users tooaccess</span></span>
* <span data-ttu-id="5946e-122">Databázi SQL nebo QuickBooks DB</span><span class="sxs-lookup"><span data-stu-id="5946e-122">SQL DB or QuickBooks DB</span></span>
* <span data-ttu-id="5946e-123">Všechna data v D:\\</span><span class="sxs-lookup"><span data-stu-id="5946e-123">Any data in D:\\</span></span>

<span data-ttu-id="5946e-124">Příklad dat, která mohou být umístěny v hello výchozí profil toobe zkopírují do UPD každých uživatelů:</span><span class="sxs-lookup"><span data-stu-id="5946e-124">Example data that can reside in hello default profile toobe copied into every users’ UPD:</span></span>

* <span data-ttu-id="5946e-125">Konfigurační soubory na uživatele</span><span class="sxs-lookup"><span data-stu-id="5946e-125">Configuration files per user</span></span>
* <span data-ttu-id="5946e-126">Skripty, které by uživatelé potřebují zachovaná v jejich UPD</span><span class="sxs-lookup"><span data-stu-id="5946e-126">Scripts that users would need preserved in their UPD</span></span>

<span data-ttu-id="5946e-127">Klíčové body:</span><span class="sxs-lookup"><span data-stu-id="5946e-127">Key points:</span></span>

* <span data-ttu-id="5946e-128">Nikdy neukládají citlivá data, která může dojít ke ztrátě na bitovou kopii hello při vytváření vlastní image.</span><span class="sxs-lookup"><span data-stu-id="5946e-128">Never store sensitive data that can be lost on hello image when creating a custom image.</span></span>
* <span data-ttu-id="5946e-129">Citlivá data by měla vždy nacházet v samostatném souborovém serveru, jednotlivé virtuální počítač Azure, na cloudu hello a instancí virtuálních počítačů vždy externí toohello hostování vaší aplikace v rámci Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="5946e-129">Sensitive data should always reside on a separate file server, separate Azure VM, on hello cloud, and always external toohello VM instances hosting your applications within Azure RemoteApp.</span></span> 
* <span data-ttu-id="5946e-130">Uživatelská data budou uložena a přetrvává v hello disk profilu uživatele (UPD)</span><span class="sxs-lookup"><span data-stu-id="5946e-130">User data is saved and persists in hello user profile disk (UPD)</span></span>

