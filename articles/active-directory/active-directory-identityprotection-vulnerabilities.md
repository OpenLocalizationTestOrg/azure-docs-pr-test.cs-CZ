---
title: "aaaVulnerabilities detekovaných službou Azure Active Directory Identity Protection | Microsoft Docs"
description: "Přehled hello chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection."
services: active-directory
keywords: "ochrany identit Azure active directory, cloud app discovery,. Správa aplikací, zabezpečení, rizik, úroveň rizika, ohrožení zabezpečení, zásady zabezpečení"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 92233a5b-cb34-4d28-88cc-d5d29c0f3256
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 5e1cb401f8b566a180eb46e3420a090bcfc66767
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection
Ohrožení zabezpečení jsou slabá místa ve vašem prostředí, může útočník zneužít. Doporučujeme, abyste vyřešte tyto chyby zabezpečení tooimprove hello postavení zabezpečení vaší organizace a útočníkům zabránit v jejich využití.


![ohrožení zabezpečení](./media/active-directory-identityprotection-vulnerabilities/101.png "ohrožení zabezpečení")



Hello následující části poskytují přehled o ohrožení zabezpečení hello hlášené Identity Protection.

## <a name="multi-factor-authentication-registration-not-configured"></a>Registrace služby Multi-Factor authentication není nakonfigurováno
Toto ohrožení zabezpečení umožňuje řídit hello nasazení služby Azure Multi-Factor Authentication ve vaší organizaci. 

Ověřování Azure Multi-Factor authentication poskytuje druhou vrstvu zabezpečení toouser ověřování. Při splnění požadavků uživatelů pro jednoduchý proces přihlášení pomáhá chránit přístup toodata a aplikace. Zajišťuje silné ověřování přes celou řadu možností snadno ověření – telefonní hovor, textová zpráva nebo mobilní aplikace oznámení nebo ověřovací kód a 3. stran tokeny OATH.

Doporučujeme vám, že vyžadujete ověřování Azure Multi-Factor Authentication pro přihlášení uživatele. Služba Multi-Factor authentication hrají roli klíče v zásadách podmíněného přístupu na základě riziko k dispozici prostřednictvím Identity Protection.

Další podrobnosti najdete v tématu [co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)

## <a name="unmanaged-cloud-apps"></a>Nespravované cloudové aplikace
Toto ohrožení zabezpečení pomáhá identifikovat nespravované cloudových aplikací ve vaší organizaci.

V rámci moderní firmy IT oddělení neberou často všechny hello cloudové aplikace, uživateli v organizaci používáte toodo práci. Je snadno toosee, proč by správci mají obavy o neoprávněný přístup k datům toocorporate, možné úniku a další bezpečnostní rizika. 

Doporučujeme vám, že vaše organizace nasazení Cloud App Discovery toodiscover nespravované cloudových aplikací a toomanage tyto aplikace pomocí Azure Active Directory.

Další podrobnosti najdete v tématu [hledání nespravované cloudových aplikací s Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).

## <a name="security-alerts-from-privileged-identity-management"></a>Výstrahy zabezpečení z Správa privilegovaných identit
Toto ohrožení zabezpečení vám pomůže zjistit a vyřešit výstrahy týkající se privilegované identity ve vaší organizaci.  

Uživatelé toocarry tooenable out privilegovaných operací, organizace potřebují uživatelé toogrant dočasné nebo trvalé privilegovaný přístup k ve službě Azure AD, Azure nebo Office 365 prostředků nebo jiných aplikací SaaS. Každý z těchto mohou uživatelé s oprávněním zvyšuje hello prostor pro útoky vaší organizace. Toto ohrožení zabezpečení vám pomůže identifikovat uživatele s nepotřebné privilegovaného přístupu a proveďte příslušnou akci tooreduce nebo odstranění hello nebezpečí, jež představují. 

Doporučujeme vám, že vaše organizace používá Azure AD Privileged Identity Management toomanage, řízení a monitorování privilegované identity a jejich tooresources přístupu ve službě Azure AD a také jiných služeb Microsoft online services jako je Office 365 nebo Microsoft Intune.

Další podrobnosti najdete v tématu [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 

## <a name="see-also"></a>Viz také
* [Ochrany identit Azure Active Directory](active-directory-identityprotection.md)

