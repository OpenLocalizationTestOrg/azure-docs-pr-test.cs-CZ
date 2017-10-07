---
title: aaaUpdate kolekce Azure Remoteappu | Microsoft Docs
description: "Zjistěte, jak tooupdate kolekcí vzdálené aplikace Azure RemoteApp"
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
ms.openlocfilehash: 849d7abfdfad4dbe6a235d2a28c71f7943eb812c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-a-collection-in-azure-remoteapp"></a><span data-ttu-id="085ac-103">Aktualizace kolekce v Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="085ac-103">Update a collection in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="085ac-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="085ac-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="085ac-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="085ac-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="085ac-106">Existuje bude pocházet určité doby nevyhnutelnou, když potřebujete aplikace hello tooupdate nebo bitové kopie v kolekci Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="085ac-106">There will come a time, inevitably, when you need tooupdate hello apps or image in your Azure RemoteApp collection.</span></span> <span data-ttu-id="085ac-107">Pokud použijete jednu z imagí hello součástí vašeho předplatného Azure RemoteApp, v cloud nebo hybridní kolekci, všechny aktualizace jsou zpracovávány Azure RemoteApp sebe, tak můžete easy rest.</span><span class="sxs-lookup"><span data-stu-id="085ac-107">If you are using one of hello images included with your Azure RemoteApp subscription, in either a cloud or hybrid collection, any and all updates are handled by Azure RemoteApp itself, so you can rest easy.</span></span>

<span data-ttu-id="085ac-108">Ale pokud používáte vlastní image (jste vytvořili od začátku nebo jste vytvořili úpravou jeden z našich obrázků), odpovídáte za údržbu hello image a aplikací.</span><span class="sxs-lookup"><span data-stu-id="085ac-108">However, if you are using a custom image (either that you built from scratch or that you created by modifying one of our images), you are in charge of maintaining hello image and apps.</span></span> <span data-ttu-id="085ac-109">Pokud potřebujete tooupdate bitové kopie nebo některé z aplikací hello je uvnitř, je třeba toocreate nové a aktualizované verze hello bitové kopie, a poté nahraďte hello stávající image v kolekci pomocí této nové aktualizovanou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="085ac-109">If you need tooupdate your image or any of hello apps inside it, you need toocreate a new, updated version of hello image, and then replace hello existing image in your collection with this new updated image.</span></span>

<span data-ttu-id="085ac-110">Ano jak tedy můžete o aktualizaci vaší kolekce?</span><span class="sxs-lookup"><span data-stu-id="085ac-110">So, how do you go about updating your collection?</span></span> <span data-ttu-id="085ac-111">Jedná se o přímočará:</span><span class="sxs-lookup"><span data-stu-id="085ac-111">It's fairly straightforward:</span></span>

1. <span data-ttu-id="085ac-112">Obrázek hello aktualizace, který jste použili v kolekci.</span><span class="sxs-lookup"><span data-stu-id="085ac-112">Update hello image that you used in your collection.</span></span> <span data-ttu-id="085ac-113">Použít všechny opravy nebo potřebné aktualizace a pak ho uložte s novým názvem.</span><span class="sxs-lookup"><span data-stu-id="085ac-113">Apply any patches or updates needed, and then save it with a new name.</span></span>
2. <span data-ttu-id="085ac-114">[Nahrát](remoteapp-uploadimage.md) nebo [importovat](remoteapp-image-on-azurevm.md) tooRemoteApp této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="085ac-114">[Upload](remoteapp-uploadimage.md) or [import](remoteapp-image-on-azurevm.md) that image tooRemoteApp.</span></span>
3. <span data-ttu-id="085ac-115">Nyní na stránce hello kolekce, klikněte na **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="085ac-115">Now, on hello collection page, click **Update**.</span></span>
4. <span data-ttu-id="085ac-116">Vybrat novou bitovou kopii hello z hello **Image šablony** seznamu.</span><span class="sxs-lookup"><span data-stu-id="085ac-116">Choose hello new image from hello **Template Image** list.</span></span>
5. <span data-ttu-id="085ac-117">Zde je složité součástí hello – jak budete potřebovat toodecide toodeal s žádným uživatelem, které aktuálně používáte aplikaci v kolekci hello.</span><span class="sxs-lookup"><span data-stu-id="085ac-117">Here's hello tricky part - you need toodecide how toodeal with any users that are currently using an app in hello collection.</span></span> <span data-ttu-id="085ac-118">Máte hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="085ac-118">You have hello following choices:</span></span>
   
   * <span data-ttu-id="085ac-119">**Ponechat uživatelům po aktualizaci hello 60 minut**.</span><span class="sxs-lookup"><span data-stu-id="085ac-119">**Give users 60 minutes after hello update**.</span></span> <span data-ttu-id="085ac-120">Ihned po dokončení aktualizace hello, Azure RemoteApp se zobrazí uživatelům active tooany zpráva upozorněním toosave jejich pracovních a protokolu vypnout a znovu přihlásit.</span><span class="sxs-lookup"><span data-stu-id="085ac-120">As soon as hello update is finished, Azure RemoteApp will display a message tooany active users telling them toosave their work and log off and log back in.</span></span> <span data-ttu-id="085ac-121">Po 60 minutách všech aktivních uživatelů, kteří nejsou odhlášením automaticky odhlášeni.</span><span class="sxs-lookup"><span data-stu-id="085ac-121">After 60 minutes, any active users who have not logged off will be automatically logged off.</span></span> <span data-ttu-id="085ac-122">Uživatelé mohou okamžitě přihlásit znovu.</span><span class="sxs-lookup"><span data-stu-id="085ac-122">Users can immediately log back on.</span></span>
   * <span data-ttu-id="085ac-123">**Odhlásit uživatele hned**.</span><span class="sxs-lookup"><span data-stu-id="085ac-123">**Sign users out immediately**.</span></span> <span data-ttu-id="085ac-124">Ihned po dokončení hello aktualizace, odhlaste se od všichni uživatelé automaticky bez varování.</span><span class="sxs-lookup"><span data-stu-id="085ac-124">As soon as hello update is finished, log off all users automatically without any warning.</span></span> <span data-ttu-id="085ac-125">Pokud zvolíte tuto možnost, uživatelé mohou ztratit data.</span><span class="sxs-lookup"><span data-stu-id="085ac-125">If you choose this option, users might lose data.</span></span> <span data-ttu-id="085ac-126">Však se znovu připojit aplikace toohello okamžitě.</span><span class="sxs-lookup"><span data-stu-id="085ac-126">However, they can reconnect toohello app immediately.</span></span>
6. <span data-ttu-id="085ac-127">Kliknutím na tlačítko hello zaškrtnutí toostart hello aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="085ac-127">Click hello check mark toostart hello update.</span></span>

