---
title: Aktualizace kolekce Azure Remoteappu | Microsoft Docs
description: Postup aktualizace kolekce Azure Remoteappu
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: e553d432-e581-48fe-b996-c432357eb64a
ms.service: remoteapp
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 454d78445d6092aec9eaa383e4c50cf15195848c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="update-a-collection-in-azure-remoteapp"></a><span data-ttu-id="0bb85-103">Aktualizace kolekce v Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="0bb85-103">Update a collection in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0bb85-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="0bb85-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="0bb85-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="0bb85-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="0bb85-106">Existuje bude pocházet určité doby nevyhnutelnou, když je potřeba aktualizovat aplikací nebo bitové kopie v kolekci Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="0bb85-106">There will come a time, inevitably, when you need to update the apps or image in your Azure RemoteApp collection.</span></span> <span data-ttu-id="0bb85-107">Pokud použijete jednu z imagí, které jsou součástí vašeho předplatného Azure RemoteApp, v cloud nebo hybridní kolekci, všechny aktualizace jsou zpracovávány Azure RemoteApp sebe, tak můžete easy rest.</span><span class="sxs-lookup"><span data-stu-id="0bb85-107">If you are using one of the images included with your Azure RemoteApp subscription, in either a cloud or hybrid collection, any and all updates are handled by Azure RemoteApp itself, so you can rest easy.</span></span>

<span data-ttu-id="0bb85-108">Ale pokud používáte vlastní image (jste vytvořili od začátku nebo jste vytvořili úpravou jeden z našich obrázků), odpovídáte za údržbu image a aplikací.</span><span class="sxs-lookup"><span data-stu-id="0bb85-108">However, if you are using a custom image (either that you built from scratch or that you created by modifying one of our images), you are in charge of maintaining the image and apps.</span></span> <span data-ttu-id="0bb85-109">Pokud je potřeba aktualizovat image nebo některé z aplikací je uvnitř, budete muset vytvořit nové a aktualizované verze bitové kopie a poté nahraďte tento nový aktualizovanou bitovou kopii existující bitová kopie v kolekci.</span><span class="sxs-lookup"><span data-stu-id="0bb85-109">If you need to update your image or any of the apps inside it, you need to create a new, updated version of the image, and then replace the existing image in your collection with this new updated image.</span></span>

<span data-ttu-id="0bb85-110">Ano jak tedy můžete o aktualizaci vaší kolekce?</span><span class="sxs-lookup"><span data-stu-id="0bb85-110">So, how do you go about updating your collection?</span></span> <span data-ttu-id="0bb85-111">Jedná se o přímočará:</span><span class="sxs-lookup"><span data-stu-id="0bb85-111">It's fairly straightforward:</span></span>

1. <span data-ttu-id="0bb85-112">Aktualizujte bitovou kopii, která jste použili v kolekci.</span><span class="sxs-lookup"><span data-stu-id="0bb85-112">Update the image that you used in your collection.</span></span> <span data-ttu-id="0bb85-113">Použít všechny opravy nebo potřebné aktualizace a pak ho uložte s novým názvem.</span><span class="sxs-lookup"><span data-stu-id="0bb85-113">Apply any patches or updates needed, and then save it with a new name.</span></span>
2. <span data-ttu-id="0bb85-114">[Nahrát](remoteapp-uploadimage.md) nebo [importovat](remoteapp-image-on-azurevm.md) této bitové kopie do vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="0bb85-114">[Upload](remoteapp-uploadimage.md) or [import](remoteapp-image-on-azurevm.md) that image to RemoteApp.</span></span>
3. <span data-ttu-id="0bb85-115">Nyní na stránce kolekce, klikněte na **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="0bb85-115">Now, on the collection page, click **Update**.</span></span>
4. <span data-ttu-id="0bb85-116">Vyberte novou bitovou kopii z **Image šablony** seznamu.</span><span class="sxs-lookup"><span data-stu-id="0bb85-116">Choose the new image from the **Template Image** list.</span></span>
5. <span data-ttu-id="0bb85-117">Zde je složité část – je potřeba rozhodnout, jak nakládat s žádným uživatelem, které aktuálně používáte aplikaci v kolekci.</span><span class="sxs-lookup"><span data-stu-id="0bb85-117">Here's the tricky part - you need to decide how to deal with any users that are currently using an app in the collection.</span></span> <span data-ttu-id="0bb85-118">K dispozici jsou následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="0bb85-118">You have the following choices:</span></span>
   
   * <span data-ttu-id="0bb85-119">**Ponechat uživatelům po aktualizaci 60 minut**.</span><span class="sxs-lookup"><span data-stu-id="0bb85-119">**Give users 60 minutes after the update**.</span></span> <span data-ttu-id="0bb85-120">Ihned po dokončení aktualizace, Azure RemoteApp se zobrazí zpráva pro všechny aktivní uživatele upozorněním uložit práci a odhlásit a znovu přihlásit.</span><span class="sxs-lookup"><span data-stu-id="0bb85-120">As soon as the update is finished, Azure RemoteApp will display a message to any active users telling them to save their work and log off and log back in.</span></span> <span data-ttu-id="0bb85-121">Po 60 minutách všech aktivních uživatelů, kteří nejsou odhlášením automaticky odhlášeni.</span><span class="sxs-lookup"><span data-stu-id="0bb85-121">After 60 minutes, any active users who have not logged off will be automatically logged off.</span></span> <span data-ttu-id="0bb85-122">Uživatelé mohou okamžitě přihlásit znovu.</span><span class="sxs-lookup"><span data-stu-id="0bb85-122">Users can immediately log back on.</span></span>
   * <span data-ttu-id="0bb85-123">**Odhlásit uživatele hned**.</span><span class="sxs-lookup"><span data-stu-id="0bb85-123">**Sign users out immediately**.</span></span> <span data-ttu-id="0bb85-124">Ihned po dokončení aktualizace, odhlaste se od všech uživatelů automaticky bez varování.</span><span class="sxs-lookup"><span data-stu-id="0bb85-124">As soon as the update is finished, log off all users automatically without any warning.</span></span> <span data-ttu-id="0bb85-125">Pokud zvolíte tuto možnost, uživatelé mohou ztratit data.</span><span class="sxs-lookup"><span data-stu-id="0bb85-125">If you choose this option, users might lose data.</span></span> <span data-ttu-id="0bb85-126">Však se znovu připojit k aplikaci okamžitě.</span><span class="sxs-lookup"><span data-stu-id="0bb85-126">However, they can reconnect to the app immediately.</span></span>
6. <span data-ttu-id="0bb85-127">Kliknutím na značku zaškrtnutí zahájíte aktualizace.</span><span class="sxs-lookup"><span data-stu-id="0bb85-127">Click the check mark to start the update.</span></span>
