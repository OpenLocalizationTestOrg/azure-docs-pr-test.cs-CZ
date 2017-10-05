---
title: "Přehled služby Azure spravované aplikace | Microsoft Docs"
description: "Popisuje koncepce pro Azure spravované aplikace"
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 7ace8e1ea8038e0748bfed00c0cc0a4fa340588b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-managed-applications-overview"></a>Přehled Azure spravované aplikace

Dodavatelů, kteří používají Azure můžete nabídnout zákazníkům po celém světě řešení. Azure Marketplace je galerie, která se skládá z stovky komplexní, multiresource šablony od první strany a jiných dodavatelů. Zákazníci v rámci minut, můžete nasadit a začít používat platforma jako služba (PaaS) a software jako služba (SaaS) aplikace. 

I když Marketplace nabízí skvělý způsob, jak zákazníkům rychle nasadit nabídky, zákazník je zodpovědná za údržbu a aktualizace řešení. Nad rámec fakturace bitové kopie virtuálního počítače, nelze dodavatelé účtují zákazníci použití aplikace. Kromě toho dodavatelé nelze zákazníkům zabránit v úprava prostředky kritické aplikace. Dodavatelé taky nelze blokovat přístup k duševního vlastnictví, která vytváří aplikace. Azure spravované aplikace poskytovat řešení pro tyto problémy. 

Spravované aplikace je podobná šablonu řešení na webu Marketplace, s jedním klíče rozdílem. Ve spravované aplikaci jsou zřízené prostředky do skupiny prostředků, který je spravovaný nástrojem dodavatele. Skupina prostředků je v rámci předplatného zákazníka, ale identity v klientovi dodavatele má přístup ke skupině prostředků.

## <a name="advantages-of-managed-applications"></a>Výhody spravovaných aplikací

Poskytovatelé služeb (MSPs), nezávislí výrobci softwaru, spravovat a podnikové centrální týmy IT můžete použít spravované aplikace pro doručování řešení prostřednictvím Marketplace nebo katalogu služeb. I když zákazníci nasadit tyto spravované aplikace ve svých předplatných, nemají zachovat, aktualizace nebo služba je. Protože dodavatelé spravovat a podporovat aplikace, zákazníci nemají k vývoji znalostní báze specifické pro aplikaci domény ke správě těchto aplikací. Zákazníci automaticky získávat aktualizace aplikace bez nutnosti starat o řešení potíží a diagnostice problémů s aplikacemi.

Spravované aplikace pro dodavatele a poskytovatelé vytvořit kanál prodávat infrastruktury a software přes Marketplace. Spravované aplikace také poskytují způsob, jak připojit službám a podpoře provozní Azure zákazníkům. Dodavatelé můžete vyúčtování pro zákazníky s použitím Azure fakturačních systémů. Použitím šablony pro správu životního cyklu nasazené aplikace. Tato řešení jsou samostatné a zapečetěné zákazníkovi, výrobci můžou poskytovat vysoce kvalitních služeb. Tento přístup výhody PaaS a SaaS dodavateli. Také pomáhá podnikovým centrální platformy týmy a systémových integrátorech (si) kdo chcete balíček a dále prodávat jejich řešení.

## <a name="managed-application-types"></a>Typy spravovaných aplikací
Spravované aplikace Azure mají dva typy: katalogu služeb a Marketplace.
 
### <a name="service-catalog"></a>Service Catalog  

Pomocí katalogu služeb zákazníkům vytvářet katalog schválené řešení pro Azure a používat lidé ve své organizaci. Například údržba katalog řešení je užitečné pro centrální IT týmy v podnicích. Katalog se můžete použít k zajištění dodržování určitých organizační standardy při poskytují řešení pro jejich organizace. Můžete řídit, aktualizovat a spravovat tyto aplikace. Zaměstnancům můžete snadno zjistit bohatou sadu aplikací, které jsou schváleny podle jejich IT oddělení a doporučené pomocí katalogu. Zákazníkům, kteří v katalogu služeb, které jsou spravované aplikace, které budou vytvořeny. Také mohou zobrazit spravované aplikace, které jiní lidé ve své organizaci sdílet s nimi.
 
Informace o publikování aplikace spravované katalogu služeb najdete v tématu [vytvoření a publikování aplikace spravované katalogu služeb](managed-application-publishing.md).
 
Informace o použití katalogu služeb spravovaných aplikací najdete v tématu [využívat katalogu služeb, které jsou spravované aplikace](managed-application-consumption.md).
 
### <a name="marketplace"></a>Marketplace

Spravované aplikace jsou k dispozici prostřednictvím portálu Azure Marketplace. Po dodavatele publikuje tyto aplikace, jsou k dispozici pro každého uvnitř nebo vně organizace využívat. S tímto přístupem MSPs nezávislí dodavatelé softwaru a SIs nabízejí svá řešení pro všechny zákazníky využívající Azure. Zákazníci využívat výhody použití těchto komplexní řešení bez nutnosti investovat do pochopení a údržbu řešení. 

V současné době vydavatelů můžete zpřístupnit jejich nabídky jako spravované aplikace nebo jako šablona řešení, má nespravované. Publikování spravované aplikaci hlavní součásti zahrnují soubor šablony a definiční soubor uživatelského rozhraní. Soubor šablony popisuje prostředky, které jsou zřízené. Soubor definice uživatelského rozhraní popisuje zobrazení požadované vstupy pro zřizování tyto prostředky na portálu. Požadované soubory jsou zabalené do souboru ZIP a nahrát prostřednictvím portálu publikování.
 
Informace o publikování spravované aplikace na web Marketplace najdete v tématu [Azure spravované aplikace na webu Marketplace](managed-application-author-marketplace.md).

Informace o využívání spravované aplikace z webu Marketplace najdete v tématu [využívat Azure spravované aplikace na webu Marketplace](managed-application-consume-marketplace.md).

## <a name="key-concepts"></a>Klíčové koncepty

### <a name="managed-resource-group"></a>Skupina spravovaných prostředků
Skupina spravovaných prostředků je, kde jsou vytvořeny všechny prostředky Azure, které jsou zřízené v šabloně. Například pokud zařízení používá k vytvoření účtu úložiště, tato skupina prostředků obsahuje prostředků účtu úložiště. Neobsahuje prostředků zařízení.

### <a name="appliance-package"></a>Balíček zařízení
Vydavatel vytvoří balíček, který obsahuje soubory šablon a soubor createUIDefinition. Konkrétně obsahuje následující soubory:

- **applianceMainTemplate.json**: Tento soubor šablony definuje všechny prostředky, které jsou zřizovány nástrojem zařízení. Tento soubor je soubor regulární šablony, který se používá k vytvoření prostředky.

- **MainTemplate.json**: Tento soubor šablony definuje zařízení prostředků (Microsoft.Solutions/appliances). Jednu klíčovou vlastnost definované v tento prostředek je ManagedResourceGroupId. Tato vlastnost určuje, které skupiny prostředků se používá k hostování skutečné prostředky, které jsou definovány v applianceMainTemplate.json.

- **applianceCreateUIDefinition.json**: Tento soubor popisuje vykreslení uživatelského rozhraní potřebné pro parametry definované v šabloně.

### <a name="authorization"></a>Autorizace
Vydavatel musíte zadat oprávnění potřebná ke správě prostředků jménem zákazníka dodavatelem. Toto oprávnění platí pro skupinu spravovaných prostředků. Nastavte následující hodnoty:

- **PrincipalID**: identifikátor uživatele, skupiny nebo aplikace, která se používá k udělení přístupu ke skupině spravovaných prostředků Azure Active Directory (Azure AD). Tento identifikátor patří ke klientovi vydavatele.

- **Hodnoty vlastnosti RoleDefinitionID**: Azure AD identifikátor role přiřazené předchozí ID objektu zabezpečení. Může být libovolná z vestavěné role řízení přístupu na základě Role v klientovi vydavatele. Další informace najdete v tématu [předdefinované role pro řízení přístupu](../active-directory/role-based-access-built-in-roles.md).

## <a name="next-steps"></a>Další kroky

* Informace o publikování spravovaných aplikací na webu Marketplace najdete v tématu [Azure spravované aplikace na webu Marketplace](managed-application-author-marketplace.md).
* Informace o využívání spravované aplikace z webu Marketplace najdete v tématu [využívat Azure spravované aplikace na webu Marketplace](managed-application-consume-marketplace.md).
* Informace o publikování aplikace spravované katalogu služeb najdete v tématu [vytvoření a publikování aplikace spravované katalogu služeb](managed-application-publishing.md).
* Informace o použití katalogu služeb spravovaných aplikací najdete v tématu [využívat katalogu služeb, které jsou spravované aplikace](managed-application-consumption.md).
* Pokud chcete vytvořit soubor definice uživatelského rozhraní, najdete v části [začít pracovat s CreateUiDefinition](managed-application-createuidefinition-overview.md).
