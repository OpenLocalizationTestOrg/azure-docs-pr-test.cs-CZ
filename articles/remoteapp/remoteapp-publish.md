---
title: aaaPublish aplikace v Azure Remoteappu | Microsoft Docs
description: "Zjistěte, jak toopublish aplikacím a prostředkům v Azure Remoteappu."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: c7e1a2cd-8e1f-4a33-bd43-8032ec9ac952
ms.service: remoteapp
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d7d92187e9ed999ac79554c9bb61f56a8eceeb31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toopublish-an-app-in-remoteapp"></a><span data-ttu-id="8f188-103">Jak toopublish aplikace v Remoteappu</span><span class="sxs-lookup"><span data-stu-id="8f188-103">How toopublish an app in RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8f188-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="8f188-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="8f188-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8f188-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="8f188-106">Po vytvoření kolekce vzdálené aplikace RemoteApp, musíte aplikace hello toopublish nebo zdroje, které má toomake k dispozici pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="8f188-106">After you create your RemoteApp collection, you need toopublish hello apps or resources that you want toomake available for your users.</span></span> <span data-ttu-id="8f188-107">Hello Image šablony poskytnutého s vaším předplatným mít pouze několik aplikací, které zveřejnil výchozí – tooshare hello jiné aplikace, musíte toopublish je.</span><span class="sxs-lookup"><span data-stu-id="8f188-107">hello template images provided with your subscription only have a few apps published by default - tooshare hello other apps, you need toopublish them.</span></span>

> [!NOTE]
> <span data-ttu-id="8f188-108">Potřebujete tooupdate aplikace?</span><span class="sxs-lookup"><span data-stu-id="8f188-108">Do you need tooupdate an app?</span></span> <span data-ttu-id="8f188-109">Budete potřebovat příliš[aktualizace hello image](remoteapp-update.md) první.</span><span class="sxs-lookup"><span data-stu-id="8f188-109">You'll need too[update hello image](remoteapp-update.md) first.</span></span>
> 
> 

<span data-ttu-id="8f188-110">Na hello **publikování** hello portálu, klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="8f188-110">On hello **Publishing** tab in hello portal, click **Publish**.</span></span> <span data-ttu-id="8f188-111">Aplikace můžete přidat buď z image šablony **spustit** nabídky nebo zadejte hello cesta toowhere hello aplikace je nainstalovaná na image šablony hello.</span><span class="sxs-lookup"><span data-stu-id="8f188-111">You can either add an app from your template image's **Start** menu or provide hello path toowhere hello app is installed on hello template image.</span></span> <span data-ttu-id="8f188-112">Pokud zvolíte tooadd z hello **spustit** nabídce zvolte toopublish aplikace hello hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="8f188-112">If you choose tooadd from hello **Start** menu, choose hello app toopublish from hello list.</span></span> <span data-ttu-id="8f188-113">Pokud si zvolíte tooprovide hello cesta toohello aplikace, zadejte název aplikace hello a hello cesta toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8f188-113">If you choose tooprovide hello path toohello app, enter a name for hello app and hello path toohello app.</span></span> <span data-ttu-id="8f188-114">Použití proměnné v cestě hello – například "% systemdrive %" místo "c:\".</span><span class="sxs-lookup"><span data-stu-id="8f188-114">Use variables in hello path - for example, "%systemdrive%" instead of "c:\".</span></span>

> [!NOTE]
> <span data-ttu-id="8f188-115">Pokud chcete tooadd aplikaci z hello **spustit** nabídky, je třeba toohave *přidat tuto aplikaci toohello **spustit** nabídky na image šablony.*</span><span class="sxs-lookup"><span data-stu-id="8f188-115">If you want tooadd your app from hello **Start** menu, you need toohave *added that app toohello **Start** menu on your template image.*</span></span> <span data-ttu-id="8f188-116">Jinak, vzdálené aplikace RemoteApp se zobrazí jen co *je* na hello **spustit** nabídce a bude nezaměňovat.</span><span class="sxs-lookup"><span data-stu-id="8f188-116">Otherwise, RemoteApp will only see what *is* on hello **Start** menu, and you will be confused.</span></span> 
> 
> <span data-ttu-id="8f188-117">toomake, zda je vaše aplikace hello **spustit** nabídce umístit soubor zástupce - **lnk** – hello %systemdrive%\ProgramData\Microsoft\Windows\Start Start\Programy složce.</span><span class="sxs-lookup"><span data-stu-id="8f188-117">toomake sure your app is in hello **Start** menu, place a shortcut file - **.lnk** - inside hello %systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs folder.</span></span>
> 
> <span data-ttu-id="8f188-118">Pokud jste zapomněli toohello aplikace hello tooadd **spustit** nabídky při vytváření šablony hello vyberte tooadd hello cesta toohello aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8f188-118">If you forgot tooadd hello app toohello **Start** menu when you created hello template, choose tooadd hello path toohello app.</span></span> <span data-ttu-id="8f188-119">(Nebo znovu vytvořte image šablony ale který je poměrně trochu další práci.)</span><span class="sxs-lookup"><span data-stu-id="8f188-119">(Or recreate your template image, but that's quite a bit more work.)</span></span>
> 
> 

