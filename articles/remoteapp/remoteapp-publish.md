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
# <a name="how-toopublish-an-app-in-remoteapp"></a>Jak toopublish aplikace v Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Po vytvoření kolekce vzdálené aplikace RemoteApp, musíte aplikace hello toopublish nebo zdroje, které má toomake k dispozici pro vaše uživatele. Hello Image šablony poskytnutého s vaším předplatným mít pouze několik aplikací, které zveřejnil výchozí – tooshare hello jiné aplikace, musíte toopublish je.

> [!NOTE]
> Potřebujete tooupdate aplikace? Budete potřebovat příliš[aktualizace hello image](remoteapp-update.md) první.
> 
> 

Na hello **publikování** hello portálu, klikněte na **publikovat**. Aplikace můžete přidat buď z image šablony **spustit** nabídky nebo zadejte hello cesta toowhere hello aplikace je nainstalovaná na image šablony hello. Pokud zvolíte tooadd z hello **spustit** nabídce zvolte toopublish aplikace hello hello seznamu. Pokud si zvolíte tooprovide hello cesta toohello aplikace, zadejte název aplikace hello a hello cesta toohello aplikace. Použití proměnné v cestě hello – například "% systemdrive %" místo "c:\".

> [!NOTE]
> Pokud chcete tooadd aplikaci z hello **spustit** nabídky, je třeba toohave *přidat tuto aplikaci toohello **spustit** nabídky na image šablony.* Jinak, vzdálené aplikace RemoteApp se zobrazí jen co *je* na hello **spustit** nabídce a bude nezaměňovat. 
> 
> toomake, zda je vaše aplikace hello **spustit** nabídce umístit soubor zástupce - **lnk** – hello %systemdrive%\ProgramData\Microsoft\Windows\Start Start\Programy složce.
> 
> Pokud jste zapomněli toohello aplikace hello tooadd **spustit** nabídky při vytváření šablony hello vyberte tooadd hello cesta toohello aplikaci. (Nebo znovu vytvořte image šablony ale který je poměrně trochu další práci.)
> 
> 

