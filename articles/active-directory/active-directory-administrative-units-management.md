---
title: aaaAdministrative jednotky management preview v Azure Active Directory
description: "Pomocí administrativních jednotek pro podrobnější delegování oprávnění v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 8464cd6b-1d1a-470d-a4fb-ee29b8eab4c4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.openlocfilehash: ee2c7beb6f9f6292bbf3cdeab00801ac066ae0e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Správa administrativních jednotek ve službě Azure AD - verzi public preview
Tento článek popisuje administrativních jednotek – nový kontejner služby Active Directory Azure prostředků, které lze použít pro delegování oprávnění ke správě přes podmnožiny uživatelů a použití zásady tooa podmnožině uživatelů. V Azure Active Directory povolte administrativních jednotek centrální správci toodelegate oprávnění tooregional správci nebo tooset zásad na podrobné úrovni.

To je užitečné v organizacích s nezávislé rozdělení, například velké školy, která se skládá z mnoha autonomního škol (Business školy, školní inženýrství atd.), které jsou na sobě nezávislé. Takové rozdělení mají své vlastní správce IT, kteří řízení přístupu, Správa uživatelů a nastavit zásady speciálně pro jejich dělení. Centrální Správci mají možnost udělit toobe tato oprávnění správci oddělení přes hello uživatelům v jejich konkrétní rozdělení. Přesněji řečeno v tomto příkladu, centrální správce můžete, například vytvořit správce jednotku pro konkrétní školní (Business školní) a jeho naplnění jenom hello firmy školní uživatelé. Centrální správce potom může přidat hello firmy školní IT pracovníci tooa obor role, jinými slovy, udělte hello pracovníky IT firmy školní administrativní oprávnění jenom přes správu jednotky školní hello firmy.

> [!IMPORTANT]
> Role pro správu Správce obor jednotky můžete přiřadit pouze v případě, že povolíte Azure Active Directory Premium. Další informace najdete v tématu [Začínáme s Azure AD Premium](active-directory-get-started-premium.md).
>


Z hello správce centrální administrativní jednotky je objekt adresáře, který může být vytvořeny a naplněny s prostředky. **V této verzi preview tyto prostředky mohou být pouze uživatelé.** Jakmile vytvořeny a naplněny, hello administrativní jednotky můžete využít jako text hello toorestrict obor oprávnění jen přes prostředky obsažené v hello administrativní jednotky.

## <a name="managing-administrative-units"></a>Správa administrativních jednotek
V této verzi preview můžete vytvořit a spravovat administrativních jednotek rutin hello Azure Active Directory modul pro prostředí Windows PowerShell. Další informace o tom toolearn toodo, který najdete v části [práce s administrativních jednotek](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)

Další informace o instalaci modulu hello Azure AD a požadavky na software a informace o hello rutiny modulu Azure AD pro správu administrativních jednotek, včetně syntaxe, popisy parametrů a příklady naleznete v tématu [Azure Active Prostředí PowerShell Directory](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

## <a name="next-steps"></a>Další kroky
[Edice služby Azure Active Directory](active-directory-editions.md)
