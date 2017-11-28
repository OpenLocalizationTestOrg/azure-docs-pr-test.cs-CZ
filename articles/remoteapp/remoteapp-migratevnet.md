---
title: "aaaHow toomigrate z virtuální sítě vzdálené aplikace RemoteApp tooan virtuální síť Azure | Microsoft Docs"
description: "Zjistěte, jak toomigrate z virtuální sítě vzdálené aplikace RemoteApp tooan virtuální síť Azure"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: baea5d29-353b-48f8-b47f-806f2163e067
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: c0f8617556c6f1e33eca8322febf67ff33937ecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomigrate-a-hybrid-collection-from-a-remoteapp-vnet-tooan-azure-vnet"></a><span data-ttu-id="508e4-103">Jak toomigrate hybridní kolekci z virtuální sítě vzdálené aplikace RemoteApp tooan virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="508e4-103">How toomigrate a hybrid collection from a RemoteApp VNET tooan Azure VNET</span></span>
> [!IMPORTANT]
> <span data-ttu-id="508e4-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="508e4-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="508e4-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="508e4-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="508e4-106">Dobrá zpráva!</span><span class="sxs-lookup"><span data-stu-id="508e4-106">Good news!</span></span> <span data-ttu-id="508e4-107">Jsme jste povolili toodeploy hybridní kolekce vzdálené aplikace RemoteApp přímo do vaší stávající virtuální sítě Azure (virtuální sítě) místo vytvoření virtuální sítě vzdálené aplikace RemoteApp specifické.</span><span class="sxs-lookup"><span data-stu-id="508e4-107">We have enabled you toodeploy hybrid RemoteApp collections directly into your existing Azure virtual networks (VNETs) instead of creating RemoteApp-specific VNETs.</span></span> <span data-ttu-id="508e4-108">To vám umožní využít výhod hello nejnovější funkce virtuální síť (jako je ExpressRoute) a poskytnout vaší hybridní kolekce přímého síťového přístupu tooother služby Azure a virtuální počítače nasazené toothat virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="508e4-108">This lets you take advantage of hello latest VNET features (like ExpressRoute) and give your hybrid collections direct network access tooother Azure services and virtual machines deployed toothat VNET.</span></span>  <span data-ttu-id="508e4-109">(To získá je lepší výkon a jednodušší nastavení než konfigurace propojení VNET-to-VNET).</span><span class="sxs-lookup"><span data-stu-id="508e4-109">(This gets you better performance and easier setup than VNET-to-VNET configurations).</span></span>

<span data-ttu-id="508e4-110">Řekněme, že jste už vytvořili hybridní kolekce vzdálené aplikace RemoteApp názvem *OriginalCollection* v virtuální sítě vzdálené aplikace RemoteApp označuje jako *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="508e4-110">Let’s say that you’ve already created a hybrid RemoteApp collection called *OriginalCollection* with a RemoteApp VNET called *RemoteAppVNET*.</span></span> <span data-ttu-id="508e4-111">Tady jsou kroky toomigrate hello ho tooa novou virtuální síť Azure volá *AzureVNET*.</span><span class="sxs-lookup"><span data-stu-id="508e4-111">Here are hello steps toomigrate it tooa new Azure VNET called *AzureVNET*.</span></span>

1. <span data-ttu-id="508e4-112">Na hello **sítě** ve hello [portálu pro správu](http://manage.windowsazure.com/), vytvořte virtuální síť, která volá *AzureVNET*pomocí hello stejné umístění, konfigurace serveru DNS a adresní prostor (pro minimálně jeden Dobrý den *AzureVNET* podsítě) jako jste použili pro *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="508e4-112">On hello **Networks** tab in hello [management portal](http://manage.windowsazure.com/), create a VNET called *AzureVNET*, using hello same location, DNS configuration, and address space (for at least one of hello *AzureVNET* subnets) as you used for *RemoteAppVNET*.</span></span>
2. <span data-ttu-id="508e4-113">Konfigurace *AzureVNET* tooeither hostitele nebo mít nasazení služby Active Directory toohello připojení k síti, *OriginalCollection* je připojený k doméně do.</span><span class="sxs-lookup"><span data-stu-id="508e4-113">Configure *AzureVNET* tooeither host or have network connectivity toohello Active Directory deployment that *OriginalCollection* is domain joined to.</span></span>
3. <span data-ttu-id="508e4-114">Na hello **vzdálené aplikace RemoteApp** kartě, vytvořit novou kolekci vzdálené aplikace RemoteApp názvem *nové kolekce*.</span><span class="sxs-lookup"><span data-stu-id="508e4-114">On hello **RemoteApps** tab, create a new RemoteApp collection called *New Collection*.</span></span> <span data-ttu-id="508e4-115">(Použití hello **vytvořit s virtuální síť** možnost, není **rychle vytvořit**.)</span><span class="sxs-lookup"><span data-stu-id="508e4-115">(Use hello **Create with VNET** option, not **Quick Create**.)</span></span>
4. <span data-ttu-id="508e4-116">Konfigurace *NewCollection* toobe nasazené tooa podsítě v *AzureVNET*.</span><span class="sxs-lookup"><span data-stu-id="508e4-116">Configure *NewCollection* toobe deployed tooa subnet in *AzureVNET*.</span></span>
5. <span data-ttu-id="508e4-117">Konfigurace *NewCollection* toouse hello stejnou bitovou kopii a informace o připojení k doméně jako jste použili pro *OriginalCollection*.</span><span class="sxs-lookup"><span data-stu-id="508e4-117">Configure *NewCollection* toouse hello same image and domain join information as you used for *OriginalCollection*.</span></span>
6. <span data-ttu-id="508e4-118">Po několik hodin *NewCollection* se zobrazí v seznamu kolekce s aktivním stavu.</span><span class="sxs-lookup"><span data-stu-id="508e4-118">After a few hours, *NewCollection* will show up in your collection list with an Active state.</span></span>

<span data-ttu-id="508e4-119">Nyní pokud nepotřebujete toomigrate nějaké informace o uživateli z hello původní kolekce toohello nové kolekce, proveďte tyto kroky dále:</span><span class="sxs-lookup"><span data-stu-id="508e4-119">Now, if you DON’T need toomigrate any user information from hello original collection toohello new collection, do these steps next:</span></span>

1. <span data-ttu-id="508e4-120">Odstranit *OriginalCollection*.</span><span class="sxs-lookup"><span data-stu-id="508e4-120">Delete *OriginalCollection*.</span></span>
2. <span data-ttu-id="508e4-121">Odstranit *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="508e4-121">Delete *RemoteAppVNET*.</span></span>

<span data-ttu-id="508e4-122">A jste hotovi!</span><span class="sxs-lookup"><span data-stu-id="508e4-122">And, you’re done!</span></span>

<span data-ttu-id="508e4-123">Případně pokud potřebujete informace o uživateli toomigrate z hello původní kolekce toohello nové kolekce, proveďte tyto kroky dále:</span><span class="sxs-lookup"><span data-stu-id="508e4-123">Alternately, if you DO need toomigrate user information from hello original collection toohello new collection, do these steps next:</span></span>

1. <span data-ttu-id="508e4-124">Odeslat e-mail příliš[ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) s svoje ID předplatného Azure hello název původní kolekci a hello název nové kolekce a požádejte je toomigrate informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="508e4-124">Send an email too[remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) with your Azure subscription ID, hello name of your original collection, and hello name of your new collection, and ask them toomigrate your user information.</span></span>
2. <span data-ttu-id="508e4-125">V rámci 2 pracovních dnů přesune tým služby RemoteApp hello hello uživatel přístup k seznamu a všechny dokumenty uživatele a nastavení uživatele z hello původní kolekce toohello nové kolekce.</span><span class="sxs-lookup"><span data-stu-id="508e4-125">Within 2 business days hello RemoteApp team will move hello user access list and all user documents and user settings from hello original collection toohello new collection.</span></span>
3. <span data-ttu-id="508e4-126">Odstranit *OriginalCollection*.</span><span class="sxs-lookup"><span data-stu-id="508e4-126">Delete *OriginalCollection*.</span></span>
4. <span data-ttu-id="508e4-127">Odstranit *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="508e4-127">Delete *RemoteAppVNET*.</span></span>

<span data-ttu-id="508e4-128">A teď jste hotovi!</span><span class="sxs-lookup"><span data-stu-id="508e4-128">And now, you’re done!</span></span>

<span data-ttu-id="508e4-129">Pokud máte nějaké otázky nebo potřebujete speciální pomoc, pošlete e-mail [ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).</span><span class="sxs-lookup"><span data-stu-id="508e4-129">If you have any questions or need special assistance, please email [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).</span></span>

