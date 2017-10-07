---
title: "algoritmus hash podpisu aaaChange Office 365 vztahu důvěryhodnosti předávající strany | Microsoft Docs"
description: "Tato stránka obsahuje pokyny pro změnu algoritmus SHA pro vztah důvěryhodnosti federace s Office 365"
keywords: "SHA1, SHA256, O365, federace, aadconnect, služby AD FS, služby ad fs, změny sha, vztah důvěryhodnosti federace, vztah důvěryhodnosti předávající strany"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: samueld
editor: 
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: anandy
ms.openlocfilehash: 3333d1384aff8bdf6b3bcc894f8c633fd9ccc3a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a>Změňte algoritmus hash podpisu pro Office 365 vztah důvěryhodnosti předávající strany
## <a name="overview"></a>Přehled
Active Directory Federation Services (AD FS) podepisuje jeho tooensure Azure Active Directory tooMicrosoft tokeny, který nemůže být úmyslně poškozena. Tento podpis může být založen na SHA1 nebo SHA256. Azure Active Directory teď podporuje tokeny, které jsou podepsané s algoritmem SHA256 a doporučujeme nastavení hello token podpisový algoritmus tooSHA256 hello nejvyšší úroveň zabezpečení. Tento článek popisuje kroky hello potřeby tooset hello token podpisový algoritmus toohello více secure úroveň SHA256.

>[!NOTE]
>Společnost Microsoft doporučuje použití SHA256 jako algoritmu hello k podepisování tokenů, jako je bezpečnější než SHA1 ale SHA1 zůstává stále podporované možnosti.

## <a name="change-hello-token-signing-algorithm"></a>Změna algoritmu podpisové hello
Po nastavení algoritmus podpisu hello s jedním z hello dva procesy služby AD FS podepisuje tokeny hello pro Office 365 vztah důvěryhodnosti předávající strany s SHA256. Nepotřebujete toomake změny další konfigurace a tato změna nemá žádný vliv na vaše možnost tooaccess Office 365 nebo jiné aplikace Azure AD.

### <a name="ad-fs-management-console"></a>Konzola pro správu AD FS
1. Otevřete konzolu pro správu hello AD FS na server hello primární AD FS.
2. Rozbalte uzel hello AD FS a klikněte na tlačítko **vztahy důvěryhodnosti předávající strany**.
3. Klikněte pravým tlačítkem na důvěryhodnosti předávající strany Office 365 nebo Azure a vyberte **vlastnosti**.
4. Vyberte hello **Upřesnit** kartě a vyberte hello zabezpečený algoritmus hash SHA256.
5. Klikněte na **OK**.

![Podpisový algoritmus SHA256 – konzoly MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>Rutiny služby AD FS prostředí PowerShell
1. Na všechny servery služby AD FS otevřete prostředí PowerShell v části oprávnění správce.
2. Sada hello zabezpečený algoritmus hash pomocí hello **Set-AdfsRelyingPartyTrust** rutiny.
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Také pro čtení
* [Opravit vztah důvěryhodnosti Office 365 s Azure AD Connect](connect/active-directory-aadconnect-federation-management.md#repairthetrust)

