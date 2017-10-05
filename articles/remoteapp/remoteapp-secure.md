---
title: "Zabezpečení aplikacím a prostředkům v Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak zamknout aplikacím a prostředkům v Azure Remoteappu"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7fbade87-a453-426d-bfa5-c72227ea83cd
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 1c052906788f0f4fe4ca9fd6d3af63336245174a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Zabezpečení aplikacím a prostředkům v Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Azure RemoteApp poskytuje uživatelům přístup k centrálně spravované aplikace pro Windows, který vám umožňuje řídit, co uživatelé uvidí a nelze provést.  To je zvlášť užitečné, když se uživatel připojuje z nespravovaných zařízení (jako jsou jejich osobní Macbook) a chcete řídit přístup uživatele nebo prostředí.

Například pokud používáte k ověřování uživatelů služby Active Directory a chcete zabránit uživatelům v kopírování dat z aplikace, můžete nakonfigurovat zásady skupiny Vzdálená plocha pro blokování uživatele z kopírování dat.

Dalším příkladem je, pokud chcete blokovat přístup k Internetu pro konkrétní aplikace v kolekci. Můžete vytvořit pravidlo brány Windows Firewall, které blokuje přístup při vytvoření bitové kopie pro vaše kolekce.

## <a name="implementation-options"></a>Možnosti implementace
  Zde jsou možnosti klíče implementace, které můžete použít samostatně nebo v kombinaci podle potřeby:

1. Pokud vaše kolekce vzdálené aplikace RemoteApp je připojený k doméně můžete vynutit žádné [zásad skupiny](https://technet.microsoft.com/library/cc725828.aspx) (s výjimkou zásady časový limit nečinnosti a odpojení popsané [sem](../azure-subscription-service-limits.md)).
2. Jako alternativu k zásad skupiny (Pokud kolekce není připojený k doméně, nebo nemáte správná oprávnění ve službě AD), můžete nakonfigurovat [místní zásady](https://technet.microsoft.com/library/cc775702.aspx) do image šablony.  Všimněte si, že tuto skupinu zásady trumfy místní zásady, když dojde ke konfliktu.
3. Některá nastavení operačního systému nebo aplikace se nedají konfigurovat prostřednictvím zásad, ale mohou být prostřednictvím pomocí klíče registru [nástroj RegEdit](remoteapp-hybridtrouble.md) při konfiguraci image šablony.
4. Můžete použít [brány Windows Firewall](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) do ovládacího prvku do a z počítače, na kterém je spuštěna aplikace přístup k síti. Ujistěte se, že nedošlo k blokování adres URL a portů, které jsou definované v tomto poli.
5. Můžete použít [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) řídit, kteří uživatelé aplikací a souborů můžete spustit. Uživatele můžete například zjistit, jak spouštět aplikace, aby nemohl publikovat, ale které jsou k dispozici v bitové kopii, kterou jste použili k vytvoření kolekce – nástroje AppLocker můžete blokovat to.

## <a name="detailed-information"></a>Podrobné informace
* Tyto zásady vzdálené plochy by mohly být velmi užitečné:
  * [Zařízení a prostředků](https://technet.microsoft.com/library/ee791794.aspx)
  * [Přesměrování tiskárny](https://technet.microsoft.com/library/ee791784.aspx)
  * [Profily](https://technet.microsoft.com/library/ee791865.aspx).
* Všimněte si, že konfigurace přesměrování přes modul vzdálené aplikace RemoteApp PowerShell (jak je vidět [sem](remoteapp-redirection.md)) závisí na klientském počítači. Chcete-li tyto zásady vynutit, aby primární cílem je zabezpečení budete chtít vynutit zásady prostřednictvím image šablony místní zásady nebo prostřednictvím zásad skupiny.
* [Windows Server 2012 R2 zásady](https://technet.microsoft.com/library/hh831791.aspx).
* [Office 2013 zásady](https://technet.microsoft.com/library/cc178969.aspx) (včetně [postup přizpůsobení panelu nástrojů Office](https://technet.microsoft.com/library/cc179143.aspx)).

