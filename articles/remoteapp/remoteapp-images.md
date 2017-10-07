---
title: "aaaWhat je v hello Image šablony Azure RemoteApp? | Dokumentace Microsoftu"
description: "Další informace o imagích šablon hello součástí Azure Remoteappu."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7f8442b2-81da-421e-a453-aa53ba2066b7
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: ea012cec8dc581a8bd4a5a138ce302de19d5c6af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-in-hello-azure-remoteapp-template-images"></a>Co jsou Image šablony hello Azure RemoteApp?
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Vaše předplatné Azure RemoteAppu obsahuje tři image šablon:

* Windows Server 2012
* Microsoft Office 365 ProPlus (požadováno předplatné služeb Office 365)
* Microsoft Office 2013 Professional Plus (pouze zkušební verze)

> [!IMPORTANT]
> Vaše předplatné Azure Remoteappu vám uděluje že přístup toohello software v hello Image, s výjimkou hello Office 365 ProPlus, který vyžaduje samostatné předplatné a Office 2013, který nelze použít v produkčním prostředí. To znamená, že můžete sdílet hello programy nebo aplikace v imagích šablon hello s uživateli. Například pokud vytvoříte kolekci, která využívá image systému Windows Server 2012 R2 hello, můžete publikovat System Center Endpoint Protection pro uživatele tooaccess prostřednictvím vzdálené aplikace RemoteApp.
> 
> Podívejte se na hello [podrobnosti o licencování Remoteappu](remoteapp-licensing.md) Další informace. A [použití Officu s Azure Remoteappem](remoteapp-o365.md) pro hello Office licenční informace.
> 
> 

Přečtěte si podrobnosti o tom, co každý image obsahuje.

## <a name="windows-server-2012-r2--hello-vanilla-image"></a>Windows Server 2012 R2 ("hello vanilla image)
Tento image je založený na operačním systému Microsoft Windows Server 2012 R2 Datacenter a hello tyto role a funkce nainstaloval toomeet hello požadavky pro Image šablony Azure RemoteApp:

* Rozhraní .NET Framework 4.5, 3.5.1, 3.5
* Desktopové prostředí
* Služba podpory rukopisu
* Media Foundation
* Hostitel relace vzdálené plochy
* Windows PowerShell 4.0
* Integrované skriptovací prostředí (ISE) v prostředí Windows PowerShell
* Podpora subsystémů WoW64

Tento image má také následující aplikace nainstalované hello:

* Adobe Flash Player
* Microsoft Silverlight
* Microsoft System Center 2012 Endpoint Protection
* Microsoft Windows Media Player

## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (požadován odběr)
Office 365 je hello nejvíce požadovaná aplikace, takže jsme vytvořili "vlastní" image pro vás toowork s.

Tento image je rozšířením hello vanilla image a hello následující komponenty aplikace Microsoft Office 365 ProPlus nainstaloval kromě toohello komponent popsaných v imagi Windows Server 2012 R2 hello:

* Access
* Excel
* Lync
* OneNote
* OneDrive pro firmy (Všimněte si, Tento agent hello synchronizace není podporována pro použití s Azure Remoteappem)
* Outlook
* PowerPoint
* Word
* Nástroje kontroly pravopisu systému Microsoft Office

Hello image také zahrnuje Visio Pro a Project Pro.

A hello aplikací také následující:

* Nativní klient SQL
* Ovladač ODBC
* Klient pro získávání dat SQL Server
* Klient MasterDataServices
* Microsoft Publisher
* PowerQuery
* PowerMap

Plná funkčnost aplikací Office 365 ProPlus je k dispozici pouze pro uživatele, kteří mají plán Office 365 ProPlus. Další informace o odběru hello Office 365 najdete v části plány [plány služeb Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx). Máte další otázky? Podívejte se na hello [Office 365 + RemoteApp](remoteapp-o365.md) informace. Seznamte se také hello nový článek, [jak toouse předplatné služeb Office 365 s Azure Remoteappem](remoteapp-officesubscription.md).

Všimněte si, že budete potřebovat samostatně toolicense Office 365 ProPlus, Visio Pro a Project Pro – každá má své vlastní licenci.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (pouze zkušební verze)
Během hello bezplatné zkušební doby můžete testovat službu hello s bitovou kopií hello Office 2013.

Tento image je rozšířením hello vanilla image a hello následující komponenty aplikace Microsoft Office 2013 Professional Plus nainstaloval kromě toohello komponent popsaných v imagi Windows Server 2012 R2 hello:

* Access
* Excel
* Lync
* OneNote
* OneDrive pro firmy (Všimněte si, Tento agent hello synchronizace není podporována pro použití s Azure Remoteappem)
* Outlook
* PowerPoint
* Project
* Visio
* Word
* Nástroje kontroly pravopisu systému Microsoft Office

> [!IMPORTANT]
> **Právní informace:** Tento image neobsahuje licenci aplikace Microsoft Office a*nelze ho použít pro produkci*. image Office 2013 Professional Plus Hello je určena pouze pro zkušební verzi. Pokud chcete aplikace Office toouse v Azure Remoteappu pro produkci, je nutné image Office 365 ProPlus toouse hello. Další informace o licencování Officu najdete v části [Použití  Officu 365 s Azure RemoteAppem](remoteapp-o365.md).
> 
> 

