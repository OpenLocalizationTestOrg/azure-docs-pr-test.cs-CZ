---
title: "Chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection | Microsoft Docs"
description: "Přehled chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection."
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
ms.openlocfilehash: 364873ff54099a6123e40b12e819d1745751f285
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection
Ohrožení zabezpečení jsou slabá místa ve vašem prostředí, může útočník zneužít. Doporučujeme, abyste tyto nedostatky ke zlepšení postavení zabezpečení vaší organizace a útočníkům zabránit v jejich využití.


![ohrožení zabezpečení](./media/active-directory-identityprotection-vulnerabilities/101.png "ohrožení zabezpečení")



Následující části poskytují přehled o ohrožení zabezpečení hlášené Identity Protection.

## <a name="multi-factor-authentication-registration-not-configured"></a>Registrace služby Multi-Factor authentication není nakonfigurováno
Toto ohrožení zabezpečení umožňuje řízení nasazení služby Azure Multi-Factor Authentication ve vaší organizaci. 

Ověřování Azure Multi-Factor authentication poskytuje druhou vrstvu zabezpečení pro ověřování uživatelů. Ho pomáhá zabezpečit přístup k datům a aplikacím při splnění požadavků uživatelů pro jednoduchý proces přihlášení. Zajišťuje silné ověřování přes celou řadu možností snadno ověření – telefonní hovor, textová zpráva nebo mobilní aplikace oznámení nebo ověřovací kód a 3. stran tokeny OATH.

Doporučujeme vám, že vyžadujete ověřování Azure Multi-Factor Authentication pro přihlášení uživatele. Služba Multi-Factor authentication hrají roli klíče v zásadách podmíněného přístupu na základě riziko k dispozici prostřednictvím Identity Protection.

Další podrobnosti najdete v tématu [co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)

## <a name="unmanaged-cloud-apps"></a>Nespravované cloudové aplikace
Toto ohrožení zabezpečení pomáhá identifikovat nespravované cloudových aplikací ve vaší organizaci.

V rámci moderní firmy IT oddělení neberou často všechny cloudové aplikace, které uživateli v organizaci používáte ke své práci. Je snadno zjistit, proč by správci mají obavy o neoprávněný přístup k podnikovým datům, možné úniku a další bezpečnostní rizika. 

Doporučujeme vám, že vaše organizace nasadit Cloud App Discovery ke zjišťování nespravované cloudových aplikací a ke správě těchto aplikací pomocí služby Azure Active Directory.

Další podrobnosti najdete v tématu [hledání nespravované cloudových aplikací s Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).

## <a name="security-alerts-from-privileged-identity-management"></a>Výstrahy zabezpečení z Správa privilegovaných identit
Toto ohrožení zabezpečení vám pomůže zjistit a vyřešit výstrahy týkající se privilegované identity ve vaší organizaci.  

Pokud chcete povolit uživatelům provádět privilegované operace, organizace potřebují udělit uživatelům dočasné nebo trvalé privilegovaného přístupu ve službě Azure AD, Azure nebo Office 365 prostředků nebo jiných aplikací SaaS. Každý z těchto mohou uživatelé s oprávněním zvětšuje prostor pro útoky vaší organizace. Toto ohrožení zabezpečení pomáhá identifikovat uživatele s nepotřebné privilegovaného přístupu a proveďte příslušné akce k omezit nebo odstranit, které budou představovat riziko. 

Doporučujeme, abyste vaše organizace používá Azure AD Privileged Identity Management pro správu, řízení a monitorování privilegované identity a jejich přístup k prostředkům ve službě Azure AD, jakož i jiných služeb Microsoft online services jako je Office 365 nebo Microsoft Intune.

Další podrobnosti najdete v tématu [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 

## <a name="see-also"></a>Viz také
* [Ochrany identit Azure Active Directory](active-directory-identityprotection.md)

