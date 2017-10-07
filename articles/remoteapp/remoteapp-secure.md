---
title: "aaaSecure aplikacím a prostředkům v Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak toolock dolů aplikacím a prostředkům v Azure Remoteappu"
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
ms.openlocfilehash: 26325826e92855a12a0087f19a3e32cbe1116449
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Zabezpečení aplikacím a prostředkům v Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Azure RemoteApp poskytuje uživatelům přístup toocentrally spravované aplikace pro Windows, který vám umožňuje řídit, co uživatelé uvidí a nelze provést.  To je zvlášť užitečné, když uživatel hello připojení z nespravovaném zařízení (jako jsou jejich osobní Macbook) a vy chcete toocontrol hello uživatelský přístup nebo prostředí.

Například pokud používáte k ověřování uživatelů služby Active Directory a chcete tooprevent uživatelé z kopírování dat z aplikace, můžete nakonfigurovat zásady skupiny Vzdálená plocha tooblock uživatelů z kopírování dat.

Dalším příkladem je, pokud chcete přístup k Internetu tooblock pro konkrétní aplikace v kolekci. Můžete vytvořit pravidlo brány Windows Firewall blokuje při vytváření bitové kopie hello kolekce text hello přístup.

## <a name="implementation-options"></a>Možnosti implementace
  Zde jsou možnosti hello klíče implementace, které můžete použít samostatně nebo v kombinaci podle potřeby:

1. Pokud vaše kolekce vzdálené aplikace RemoteApp je připojený k doméně můžete vynutit žádné [zásad skupiny](https://technet.microsoft.com/library/cc725828.aspx) (s výjimkou hello hello nečinnosti a odpojení časový limit zásad popsané [sem](../azure-subscription-service-limits.md)).
2. Jako alternativní tooGroup zásady (Pokud kolekce není připojený k doméně, nebo nemáte správná oprávnění hello ve službě AD), můžete nakonfigurovat [místní zásady](https://technet.microsoft.com/library/cc775702.aspx) do image šablony.  Všimněte si, že tuto skupinu zásady trumfy místní zásady, když dojde ke konfliktu.
3. Některá nastavení operačního systému nebo aplikace se nedají konfigurovat prostřednictvím zásad, ale může být pomocí klíče registru pomocí hello [nástroj RegEdit](remoteapp-hybridtrouble.md) při konfiguraci image šablony.
4. Můžete použít [brány Windows Firewall](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) toocontrol síťový přístup tooand z hello počítače, kde je spuštěna aplikace hello. Ujistěte se, že nedošlo k blokování hello adresy URL a portů, které jsou definované v tomto poli.
5. Můžete použít [Applockeru](https://technet.microsoft.com/library/hh831440.aspx) toocontrol, který můžete spustit aplikace a soubory uživatele. Například můžete zjistit, jak toorun aplikace, které jste nepublikoval, ale které jsou k dispozici v hello image jste použili toocreate hello kolekce uživatele – nástroje AppLocker můžete blokovat to.

## <a name="detailed-information"></a>Podrobné informace
* Hello následující zásady vzdálené plochy se pravděpodobně toobe nejužitečnější:
  * [Zařízení a prostředků](https://technet.microsoft.com/library/ee791794.aspx)
  * [Přesměrování tiskárny](https://technet.microsoft.com/library/ee791784.aspx)
  * [Profily](https://technet.microsoft.com/library/ee791865.aspx).
* Všimněte si, že konfigurace přesměrování prostřednictvím hello modulu PowerShell vzdálené aplikace RemoteApp (jak je vidět [sem](remoteapp-redirection.md)) závisí na hello klientský počítač tooenforce hello zásady, takže pokud hello hlavním cílem je zabezpečení můžete zásady hello tooenforce prostřednictvím Hello místní zásady image šablony nebo prostřednictvím zásad skupiny.
* [Windows Server 2012 R2 zásady](https://technet.microsoft.com/library/hh831791.aspx).
* [Office 2013 zásady](https://technet.microsoft.com/library/cc178969.aspx) (včetně [jak toocustomize hello nástrojů Office](https://technet.microsoft.com/library/cc179143.aspx)).

