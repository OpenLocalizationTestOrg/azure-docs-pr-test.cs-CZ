---
title: "Jak migrovat z virtuální sítě vzdálené aplikace RemoteApp o virtuální síť Azure | Microsoft Docs"
description: "Zjistěte, jak migrovat z virtuální sítě vzdálené aplikace RemoteApp o virtuální síť Azure"
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
ms.openlocfilehash: 10b5f4844a38fe97852dee8634e8cf54f1a23a1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-migrate-a-hybrid-collection-from-a-remoteapp-vnet-to-an-azure-vnet"></a><span data-ttu-id="f4df5-103">Jak migrovat hybridní kolekce z virtuální sítě vzdálené aplikace RemoteApp o virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="f4df5-103">How to migrate a hybrid collection from a RemoteApp VNET to an Azure VNET</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f4df5-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="f4df5-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="f4df5-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="f4df5-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="f4df5-106">Dobrá zpráva!</span><span class="sxs-lookup"><span data-stu-id="f4df5-106">Good news!</span></span> <span data-ttu-id="f4df5-107">Jsme povolili můžete nasadit hybridní kolekce vzdálené aplikace RemoteApp přímo do vaší stávající virtuální sítě Azure (virtuální sítě) místo vytvoření virtuální sítě vzdálené aplikace RemoteApp specifické.</span><span class="sxs-lookup"><span data-stu-id="f4df5-107">We have enabled you to deploy hybrid RemoteApp collections directly into your existing Azure virtual networks (VNETs) instead of creating RemoteApp-specific VNETs.</span></span> <span data-ttu-id="f4df5-108">To vám umožní využívat výhody nejnovějších funkcí virtuální síť (jako je ExpressRoute) a poskytnout přímý přístup k síti v případě hybridních kolekcí k dalším službám Azure a virtuální počítače nasazené do ní.</span><span class="sxs-lookup"><span data-stu-id="f4df5-108">This lets you take advantage of the latest VNET features (like ExpressRoute) and give your hybrid collections direct network access to other Azure services and virtual machines deployed to that VNET.</span></span>  <span data-ttu-id="f4df5-109">(To získá je lepší výkon a jednodušší nastavení než konfigurace propojení VNET-to-VNET).</span><span class="sxs-lookup"><span data-stu-id="f4df5-109">(This gets you better performance and easier setup than VNET-to-VNET configurations).</span></span>

<span data-ttu-id="f4df5-110">Řekněme, že jste už vytvořili hybridní kolekce vzdálené aplikace RemoteApp názvem *OriginalCollection* v virtuální sítě vzdálené aplikace RemoteApp označuje jako *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="f4df5-110">Let’s say that you’ve already created a hybrid RemoteApp collection called *OriginalCollection* with a RemoteApp VNET called *RemoteAppVNET*.</span></span> <span data-ttu-id="f4df5-111">Tady jsou kroky k migraci na nové virtuální sítě Azure volá *AzureVNET*.</span><span class="sxs-lookup"><span data-stu-id="f4df5-111">Here are the steps to migrate it to a new Azure VNET called *AzureVNET*.</span></span>

1. <span data-ttu-id="f4df5-112">Na **sítě** kartě v [portálu pro správu](http://manage.windowsazure.com/), vytvořit virtuální síť, která volá *AzureVNET*, pomocí stejného umístění, konfigurace serveru DNS a adresní prostor (pro minimálně jeden z *AzureVNET* podsítě) jako jste použili pro *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="f4df5-112">On the **Networks** tab in the [management portal](http://manage.windowsazure.com/), create a VNET called *AzureVNET*, using the same location, DNS configuration, and address space (for at least one of the *AzureVNET* subnets) as you used for *RemoteAppVNET*.</span></span>
2. <span data-ttu-id="f4df5-113">Konfigurace *AzureVNET* na hostitele nebo mít síťové připojení k nasazení služby Active Directory, *OriginalCollection* je připojený k doméně do.</span><span class="sxs-lookup"><span data-stu-id="f4df5-113">Configure *AzureVNET* to either host or have network connectivity to the Active Directory deployment that *OriginalCollection* is domain joined to.</span></span>
3. <span data-ttu-id="f4df5-114">Na **vzdálené aplikace RemoteApp** kartě, vytvořit novou kolekci vzdálené aplikace RemoteApp názvem *nové kolekce*.</span><span class="sxs-lookup"><span data-stu-id="f4df5-114">On the **RemoteApps** tab, create a new RemoteApp collection called *New Collection*.</span></span> <span data-ttu-id="f4df5-115">(Použití **vytvořit s virtuální síť** možnost, není **rychle vytvořit**.)</span><span class="sxs-lookup"><span data-stu-id="f4df5-115">(Use the **Create with VNET** option, not **Quick Create**.)</span></span>
4. <span data-ttu-id="f4df5-116">Konfigurace *NewCollection* k nasazení pro podsíť v *AzureVNET*.</span><span class="sxs-lookup"><span data-stu-id="f4df5-116">Configure *NewCollection* to be deployed to a subnet in *AzureVNET*.</span></span>
5. <span data-ttu-id="f4df5-117">Konfigurace *NewCollection* použít stejnou bitovou kopii a informace o připojení k doméně, jako jste použili pro *OriginalCollection*.</span><span class="sxs-lookup"><span data-stu-id="f4df5-117">Configure *NewCollection* to use the same image and domain join information as you used for *OriginalCollection*.</span></span>
6. <span data-ttu-id="f4df5-118">Po několik hodin *NewCollection* se zobrazí v seznamu kolekce s aktivním stavu.</span><span class="sxs-lookup"><span data-stu-id="f4df5-118">After a few hours, *NewCollection* will show up in your collection list with an Active state.</span></span>

<span data-ttu-id="f4df5-119">Nyní pokud nepotřebujete migrovat nějaké informace o uživateli z původní kolekci do nové kolekce, proveďte tyto kroky dále:</span><span class="sxs-lookup"><span data-stu-id="f4df5-119">Now, if you DON’T need to migrate any user information from the original collection to the new collection, do these steps next:</span></span>

1. <span data-ttu-id="f4df5-120">Odstranit *OriginalCollection*.</span><span class="sxs-lookup"><span data-stu-id="f4df5-120">Delete *OriginalCollection*.</span></span>
2. <span data-ttu-id="f4df5-121">Odstranit *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="f4df5-121">Delete *RemoteAppVNET*.</span></span>

<span data-ttu-id="f4df5-122">A jste hotovi!</span><span class="sxs-lookup"><span data-stu-id="f4df5-122">And, you’re done!</span></span>

<span data-ttu-id="f4df5-123">Případně pokud potřebujete migrovat informace o uživateli z původní kolekci do nové kolekce, proveďte tyto kroky dále:</span><span class="sxs-lookup"><span data-stu-id="f4df5-123">Alternately, if you DO need to migrate user information from the original collection to the new collection, do these steps next:</span></span>

1. <span data-ttu-id="f4df5-124">E-mailovou zprávu na [ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) svoje ID předplatného Azure, názvem vašeho původního kolekce a s názvem vaší nové kolekce a požádejte je o migrovat informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="f4df5-124">Send an email to [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) with your Azure subscription ID, the name of your original collection, and the name of your new collection, and ask them to migrate your user information.</span></span>
2. <span data-ttu-id="f4df5-125">Do 2 pracovních dnů se tým služby RemoteApp přesune přístup k seznamu uživatelů a všechny dokumenty uživatele a nastavení uživatele z původní kolekce do nové kolekce.</span><span class="sxs-lookup"><span data-stu-id="f4df5-125">Within 2 business days the RemoteApp team will move the user access list and all user documents and user settings from the original collection to the new collection.</span></span>
3. <span data-ttu-id="f4df5-126">Odstranit *OriginalCollection*.</span><span class="sxs-lookup"><span data-stu-id="f4df5-126">Delete *OriginalCollection*.</span></span>
4. <span data-ttu-id="f4df5-127">Odstranit *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="f4df5-127">Delete *RemoteAppVNET*.</span></span>

<span data-ttu-id="f4df5-128">A teď jste hotovi!</span><span class="sxs-lookup"><span data-stu-id="f4df5-128">And now, you’re done!</span></span>

<span data-ttu-id="f4df5-129">Pokud máte nějaké otázky nebo potřebujete speciální pomoc, pošlete e-mail [ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).</span><span class="sxs-lookup"><span data-stu-id="f4df5-129">If you have any questions or need special assistance, please email [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).</span></span>

