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
# <a name="how-to-publish-an-app-in-remoteapp"></a>Postup publikování aplikace v Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Po vytvoření kolekce vzdálené aplikace RemoteApp, musíte publikovat aplikace nebo prostředky, které mají být k dispozici pro vaše uživatele. Image šablony poskytnutého s vaším předplatným mít pouze několik aplikací, které jsou publikovány ve výchozím nastavení - sdílet dalších aplikací, musíte publikovat je.

> [!NOTE]
> Je potřeba aktualizovat aplikaci? Budete muset [aktualizovat bitovou kopii](remoteapp-update.md) první.
> 
> 

Na **publikování** na portálu, klikněte na **publikovat**. Aplikace můžete přidat buď z image šablony **spustit** nabídky nebo zadejte cestu k nainstalovanou aplikaci na image šablony. Pokud zvolíte možnost přidat z **spustit** nabídce vyberte aplikaci, chcete-li publikovat ze seznamu. Pokud zvolíte možnost zadat cestu k aplikaci, zadejte název aplikace a cestu k aplikaci. Použití proměnné v cestě – například "% systemdrive %" místo "c:\".

> [!NOTE]
> Pokud chcete přidat aplikaci z **spustit** nabídky, musíte mít *přidat aplikaci do **spustit** nabídky na image šablony.* Jinak, vzdálené aplikace RemoteApp se zobrazí jen co *je* na **spustit** nabídce a bude nezaměňovat. 
> 
> Zajistěte, aby vaše aplikace je v **spustit** nabídce umístit soubor zástupce - **lnk** – v této složce Start\Programy %systemdrive%\ProgramData\Microsoft\Windows\Start.
> 
> Pokud jste zapomněli přidat aplikaci **spustit** nabídky při vytvoření šablony se rozhodnete přidat cestu k aplikaci. (Nebo znovu vytvořte image šablony ale který je poměrně trochu další práci.)
> 
> 

