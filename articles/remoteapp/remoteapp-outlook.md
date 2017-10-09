---
title: aaaUsing Outlook v Azure Remoteappu | Microsoft Docs
description: "Zjistěte, jak tooconfigure a používat Outlook v Azure Remoteappu | Microsoft Azure"
services: remoteapp
documentationcenter: 
author: pavithir
manager: mbaldwin
ms.assetid: cb2a498f-9539-4522-a874-542114926a61
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 119d2629ac47bd8d20d617985a9b488068aa959f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>Používání Outlooku v Azure RemoteAppu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Azure RemoteApp podporuje Microsoft Outlook O365. Další informace o [fungování Office v Azure RemoteAppu](remoteapp-officesubscription.md). Existuje několik doporučených nastavení pro Outlook, když se používá v Azure RemoteAppu.

## <a name="cached-mode"></a>Režim mezipaměti
Režim mezipaměti je při používání Outlooku v Azure RemoteAppu doporučenou konfigurací. Při konfiguraci toouse účet Outlook 2013 z místní kopie poštovní schránky Microsoft Exchange hello uživatele, který je uložen v datovém souboru offline (soubor OST) v počítači uživatele hello, společně s hello Offline Address Book funguje režim Cached Exchange, aplikace Outlook 2013 ADRESÁŘE (OFFLINE). poštovní schránky v mezipaměti Hello a OAB jsou pravidelně aktualizované z hello služby O365. Další informace o [hello rozdíly mezi mezipaměti a online režim](https://technet.microsoft.com/library/jj683103.aspx).

můžete vybrat uživatele Hello **režim Cached Exchange** nebo **Online režim** během úvodního nastavení účtu nebo změnou nastavení účtu hello. Můžete také nasadit jeden režim nebo hello jiných pomocí hello nástroje pro přizpůsobení sady Office (OCT) nebo zásad skupiny.  

Přečtěte si [podrobné pokyny k povolení režimu mezipaměti](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## <a name="search"></a>Hledání
Použití hledání v Outlooku v Azure RemoteAppu má určitá omezení. Azure RemoteApp používá ve fondu virtuálních počítačů tooaccommodate uživatelských relací. Indexování pro hledání závisí na ID hello počítače, které je pro různé virtuální počítače. Je možné, že při každém přihlášení uživatele do Azure Remoteappu, jsou řízené tooa nového virtuálního počítače. To znamená, že pokud povolíte místní vyhledávání, hello spustí se indexer při každé změně ID počítače hello (Pokud hello uživatel na odlišném virtuálním počítači). V závislosti na velikosti hello hello. Soubor OST hello indexer může trvat dlouhou dobu toocomplete a spotřebovávat zdroje potřebné pro jiné aplikace. Hledání je pak nejen pomalé, ale také nemusí vracet výsledky. Pomocí profilu účtu v režimu Online by tento problém obejít, ale by sníží celkový výkon z důvodu nedostatku toohello místní mezipaměti (viz výše na odkaz Další informace o hello rozdíl mezi mezipaměti a online režim hello). Bohužel však v Outlooku 2013 nelze ve výchozím nastavení zakázat indexované/místní hledání a povolit online hledání.

