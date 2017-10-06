---
title: "aaaAzure AD Connect v Německu Microsoft Cloud"
description: "Azure AD Connect integruje vaše místní adresáře do služby Azure Active Directory. To vám umožní tooprovide společnou identitu pro aplikace Office 365, Azure a SaaS integrované s Azure AD."
keywords: "Úvod tooAzure AD Connect, Azure AD Connect přehled, co je Azure AD Connect, nainstalovat službu active directory, Německo, černé doménové struktury"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2bcb0caf-5d97-46cb-8c32-bda66cc22dad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: f32194fa6c365614f68e5d1ddcf0dac44d223292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD Connect v Microsoft Cloudu Německo – verze Public Preview
## <a name="introduction"></a>Úvod
Azure AD Connect poskytuje synchronizaci mezi vaší místní službou Active Directory a službou Azure Active Directory.
V současné době mnoho scénářů hello v [Microsoft Cloud Německo](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) je třeba provést hello operátorem. Pokud používáte Microsoft Cloud Německo, musí mít přehled o hello následující:

* Hello následující adresy URL musí být otevřen na proxy server pro synchronizaci toooccur úspěšně:
  
  * *.microsoftonline.de
  * *.windows.net
  * * Seznamy odvolaných certifikátů
* Při přihlášení tooyour adresář Azure AD, musíte použít účet v doméně onmicrosoft.de hello.
* nejsou k dispozici Hello následující funkce:
  * Azure AD Connect Health
  * Automatické aktualizace
 
## <a name="download"></a>Ke stažení
Azure AD Connect si můžete stáhnout z okna Azure AD Connect hello hello portálu.  Použijte pokyny hello níže toolocate hello Azure AD Connect okno.

### <a name="hello-azure-ad-connect-blade"></a>Hello Azure AD Connect okno
Po přihlášení toohello portál Azure hello následující:

1. Přejděte tooBrowse
2. Vyberte Azure Active Directory.
3. Poté vyberte Azure AD Connect.

Měli byste vidět hello následující:

![Okno Azure AD Connect](media/active-directory-aadconnect-germany/germany1.png)

Hello následující tabulka popisuje funkce hello zobrazí v okně hello.

| Název | Popis |
| --- | --- |
| STAV SYNCHRONIZACE |Uvádí, zda je synchronizace povolená nebo zakázaná. |
| POSLEDNÍ SYNCHRONIZACE |Hello čas poslední úspěšné synchronizace byla dokončena. |
| FEDEROVANÉ DOMÉNY |Zobrazuje počet hello federované domény aktuálně konfigurována. |

## <a name="installation"></a>Instalace
tooinstall Azure AD Connect, můžete použít dokumentaci hello [zde](active-directory-aadconnect.md#install-azure-ad-connect).

## <a name="advanced-features-and-additional-information"></a>Pokročilé funkce a další informace
Hledáte-li další informace a doprovodné materiály k vlastním nastavením nebo pokročilým konfiguracím, začněte s tématem [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).  Tato stránka obsahuje informace a odkazy tooadditional pokyny.

