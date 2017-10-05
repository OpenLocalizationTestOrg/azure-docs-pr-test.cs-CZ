---
title: "Publikování aplikace v Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak publikovat aplikace a prostředky v Azure Remoteappu."
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
ms.openlocfilehash: 4565fa498dbadd0601004c73bfee5171efe1fad1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-publish-an-app-in-remoteapp"></a><span data-ttu-id="46522-103">Postup publikování aplikace v Remoteappu</span><span class="sxs-lookup"><span data-stu-id="46522-103">How to publish an app in RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="46522-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="46522-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="46522-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="46522-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="46522-106">Po vytvoření kolekce vzdálené aplikace RemoteApp, musíte publikovat aplikace nebo prostředky, které mají být k dispozici pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="46522-106">After you create your RemoteApp collection, you need to publish the apps or resources that you want to make available for your users.</span></span> <span data-ttu-id="46522-107">Image šablony poskytnutého s vaším předplatným mít pouze několik aplikací, které jsou publikovány ve výchozím nastavení - sdílet dalších aplikací, musíte publikovat je.</span><span class="sxs-lookup"><span data-stu-id="46522-107">The template images provided with your subscription only have a few apps published by default - to share the other apps, you need to publish them.</span></span>

> [!NOTE]
> <span data-ttu-id="46522-108">Je potřeba aktualizovat aplikaci?</span><span class="sxs-lookup"><span data-stu-id="46522-108">Do you need to update an app?</span></span> <span data-ttu-id="46522-109">Budete muset [aktualizovat bitovou kopii](remoteapp-update.md) první.</span><span class="sxs-lookup"><span data-stu-id="46522-109">You'll need to [update the image](remoteapp-update.md) first.</span></span>
> 
> 

<span data-ttu-id="46522-110">Na **publikování** na portálu, klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="46522-110">On the **Publishing** tab in the portal, click **Publish**.</span></span> <span data-ttu-id="46522-111">Aplikace můžete přidat buď z image šablony **spustit** nabídky nebo zadejte cestu k nainstalovanou aplikaci na image šablony.</span><span class="sxs-lookup"><span data-stu-id="46522-111">You can either add an app from your template image's **Start** menu or provide the path to where the app is installed on the template image.</span></span> <span data-ttu-id="46522-112">Pokud zvolíte možnost přidat z **spustit** nabídce vyberte aplikaci, chcete-li publikovat ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="46522-112">If you choose to add from the **Start** menu, choose the app to publish from the list.</span></span> <span data-ttu-id="46522-113">Pokud zvolíte možnost zadat cestu k aplikaci, zadejte název aplikace a cestu k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="46522-113">If you choose to provide the path to the app, enter a name for the app and the path to the app.</span></span> <span data-ttu-id="46522-114">Použití proměnné v cestě – například "% systemdrive %" místo "c:\".</span><span class="sxs-lookup"><span data-stu-id="46522-114">Use variables in the path - for example, "%systemdrive%" instead of "c:\".</span></span>

> [!NOTE]
> <span data-ttu-id="46522-115">Pokud chcete přidat aplikaci z **spustit** nabídky, musíte mít *přidat aplikaci do **spustit** nabídky na image šablony.*</span><span class="sxs-lookup"><span data-stu-id="46522-115">If you want to add your app from the **Start** menu, you need to have *added that app to the **Start** menu on your template image.*</span></span> <span data-ttu-id="46522-116">Jinak, vzdálené aplikace RemoteApp se zobrazí jen co *je* na **spustit** nabídce a bude nezaměňovat.</span><span class="sxs-lookup"><span data-stu-id="46522-116">Otherwise, RemoteApp will only see what *is* on the **Start** menu, and you will be confused.</span></span> 
> 
> <span data-ttu-id="46522-117">Zajistěte, aby vaše aplikace je v **spustit** nabídce umístit soubor zástupce - **lnk** – v této složce Start\Programy %systemdrive%\ProgramData\Microsoft\Windows\Start.</span><span class="sxs-lookup"><span data-stu-id="46522-117">To make sure your app is in the **Start** menu, place a shortcut file - **.lnk** - inside the %systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs folder.</span></span>
> 
> <span data-ttu-id="46522-118">Pokud jste zapomněli přidat aplikaci **spustit** nabídky při vytvoření šablony se rozhodnete přidat cestu k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="46522-118">If you forgot to add the app to the **Start** menu when you created the template, choose to add the path to the app.</span></span> <span data-ttu-id="46522-119">(Nebo znovu vytvořte image šablony ale který je poměrně trochu další práci.)</span><span class="sxs-lookup"><span data-stu-id="46522-119">(Or recreate your template image, but that's quite a bit more work.)</span></span>
> 
> 

