---
title: "aplikace spravovaného aaaOverview Azure | Microsoft Docs"
description: "Popisuje hello koncepty Azure spravované aplikace"
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 2fd1844a442515f4492c890c9798073475a66f88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-managed-applications-overview"></a>Přehled Azure spravované aplikace

Dodavatelů, kteří používají Azure nabízí řešení toocustomers kolem hello, world. Azure Marketplace je galerie, která se skládá z stovky komplexní, multiresource šablony od první strany a jiných dodavatelů. Zákazníci v rámci minut, můžete nasadit a začít používat platforma jako služba (PaaS) a software jako služba (SaaS) aplikace. 

I když hello Marketplace nabízí skvělý způsob pro zákazníky tooquickly nasadit nabídky, hello zákazníka je zodpovědná za údržbu a aktualizace hello řešení. Nad rámec hello fakturace bitové kopie virtuálního počítače, nelze dodavatelé účtují zákazníci hello použití aplikace. Kromě toho dodavatelé nelze zákazníkům zabránit v úprava prostředky kritické aplikace. Dodavatelé taky nelze zablokovat přístup toointellectual vlastnost, která tvoří aplikace. Azure spravované aplikace poskytovat řešení pro tyto problémy. 

Spravované aplikace je podobné šablona řešení tooa v hello Marketplace, s jedním klíče rozdílem. Ve spravované aplikaci hello prostředky jsou zřízené tooa skupinu prostředků, který je spravovaný nástrojem hello dodavatele. Skupina prostředků Hello je v rámci předplatného hello zákazníka, ale identity v klientovi hello dodavatele má skupinu prostředků toohello přístup.

## <a name="advantages-of-managed-applications"></a>Výhody spravovaných aplikací

Poskytovatelé služeb (MSPs), nezávislí výrobci softwaru, spravovat a podnikové centrální týmy IT můžete použít řešení toodeliver spravované aplikace prostřednictvím hello Marketplace nebo hello katalogu služeb. I když zákazníci nasadit tyto spravované aplikace ve svých předplatných, nemají toomaintain, aktualizace nebo služba je. Protože dodavatelé spravovat a podporovat aplikace hello, nemají zákazníci toodevelop domény specifické pro aplikaci znalostní báze toomanage tyto aplikace. Zákazníci automaticky získávat aktualizace aplikací bez hello nutné tooworry o řešení potíží a diagnostice problémů s aplikací hello.

Spravované aplikace pro dodavatele a poskytovatelů, vytvořte kanál toosell infrastruktury a softwaru prostřednictvím hello Marketplace. Spravované aplikace také zadat způsob tooattach služeb a provozní podpora tooAzure zákazníky. Dodavatelé můžete vyúčtování pro zákazníky s použitím hello Azure fakturačních systémů. Uživatelé můžou používat šablony toomanage hello životní cyklus nasazené aplikace. Těchto řešení je samostatná a uzavřené toohello zákazníka, výrobci můžou poskytovat vysoce kvalitních služeb. Tento přístup výhody PaaS a SaaS dodavateli. Také pomáhá podnikovým centrální platformy týmy a systémových integrátorech (si) jací uživatelé mají toopackage a dále prodávat jejich řešení.

## <a name="managed-application-types"></a>Typy spravovaných aplikací
Spravované aplikace Azure mají dva typy: katalogu služeb a Marketplace.
 
### <a name="service-catalog"></a>Service Catalog  

Zákazníci vytvářet s hello katalogu služeb, katalog schválené řešení pro Azure toobe používaných lidmi ve své organizaci. Například údržba katalog řešení je užitečné pro centrální IT týmy v podnicích. Hello katalogu tooensure dodržování standardů některé organizace mohou použít při poskytují řešení pro jejich organizace. Můžete řídit, aktualizovat a spravovat tyto aplikace. Zaměstnanci se můžou pomocí hello katalogu tooeasily zjistit hello bohatou sadu aplikací, které jsou schváleny podle jejich IT oddělení a doporučené. Zákazníkům, kteří v hello katalogu služeb, které jsou spravované aplikace, které vytvořili. Také můžete uvidí, že hello spravované aplikace, který další lidé ve své organizaci sdílené složce s nimi.
 
Informace o publikování aplikace spravované katalogu služeb najdete v tématu [vytvoření a publikování aplikace spravované katalogu služeb](managed-application-publishing.md).
 
Informace o použití katalogu služeb spravovaných aplikací najdete v tématu [využívat katalogu služeb, které jsou spravované aplikace](managed-application-consumption.md).
 
### <a name="marketplace"></a>Marketplace

Spravované aplikace jsou k dispozici prostřednictvím hello Marketplace v hello portálu Azure. Po hello dodavatele publikuje tyto aplikace, jsou k dispozici pro každého uvnitř nebo vně tooconsume organizaci. S tímto přístupem MSPs nezávislí dodavatelé softwaru a SIs můžete nabízet jejich tooall řešení Azure zákazníků. Zákazníci získat výhody hello pomocí těchto komplexní řešení bez tooinvest nutné hello vysvětlující Princip fungování a udržování hello řešení. 

V současné době vydavatelů můžete zpřístupnit jejich nabídky jako spravované aplikace nebo jako šablona řešení, má nespravované. publikování spravované aplikace Hello hlavní součásti zahrnují soubor šablony hello a soubor definice hello uživatelského rozhraní. soubor šablony Hello popisuje hello prostředky, které jsou zřízené. Soubor definice uživatelského rozhraní Hello popisuje, jak hello požadované vstupy pro zřizování tyto prostředky jsou zobrazeny v portálu hello. Hello požadované soubory jsou zabalené do souboru ZIP a nahrát prostřednictvím portálu publikování hello.
 
Informace o publikování spravované aplikaci toohello Marketplace najdete v tématu [Azure spravované aplikace v hello Marketplace](managed-application-author-marketplace.md).

Informace o použití spravovaných aplikací z hello Marketplace najdete v tématu [využívat Azure spravované aplikace v hello Marketplace](managed-application-consume-marketplace.md).

## <a name="key-concepts"></a>Klíčové koncepty

### <a name="managed-resource-group"></a>Skupina spravovaných prostředků
Hello spravované skupina prostředků je kde všechny hello Azure vytvářejí prostředky, které jsou zřízené v šabloně hello. Například pokud hello zařízení používaných toocreate účet úložiště, tato skupina prostředků obsahuje prostředků účtu úložiště hello. Neobsahuje hello zařízení prostředků.

### <a name="appliance-package"></a>Balíček zařízení
Vydavatel Hello vytvoří balíček, který obsahuje soubory hello šablonu a soubor createUIDefinition hello. Konkrétně obsahuje hello následující soubory:

- **applianceMainTemplate.json**: Tento soubor šablony definuje všechny prostředky hello, které jsou zřizovány nástrojem hello zařízení. Tento soubor je soubor regulární šablony, který byl použit toocreate prostředky.

- **MainTemplate.json**: Tento soubor šablony definuje hello zařízení prostředků (Microsoft.Solutions/appliances). Jednu klíčovou vlastnost definované v tento prostředek je ManagedResourceGroupId. Tato vlastnost určuje, které skupina prostředků je hello použité toohost skutečné se prostředky, které jsou definovány v applianceMainTemplate.json.

- **applianceCreateUIDefinition.json**: Tento soubor popisuje, jak je vykreslen hello uživatelského rozhraní, které jsou potřebné pro hello parametry definované v šabloně hello.

### <a name="authorization"></a>Autorizace
Vydavatel Hello musíte zadat hello oprávněních hello dodavatele toomanage hello prostředky jménem zákazníka hello. Toto oprávnění platí toohello skupinu spravovaných prostředků. Nastavte hello následující hodnoty:

- **PrincipalID**: hello Azure Active Directory (Azure AD) identifikátor hello uživatele, skupinu nebo aplikaci, která se používá skupina spravovaných prostředků toohello toogrant přístup. Tento identifikátor patří toohello vydavatele klienta.

- **Hodnoty vlastnosti RoleDefinitionID**: hello Azure AD identifikátor toohello přiřazenou roli hello předcházející ID objektu zabezpečení. Může být libovolná z hello předdefinované role řízení přístupu na základě Role v klientovi hello vydavatele. Další informace najdete v tématu [předdefinované role pro řízení přístupu](../active-directory/role-based-access-built-in-roles.md).

## <a name="next-steps"></a>Další kroky

* Informace o publikování spravované aplikace toohello Marketplace najdete v tématu [Azure spravované aplikace v hello Marketplace](managed-application-author-marketplace.md).
* Informace o použití spravovaných aplikací z hello Marketplace najdete v tématu [využívat Azure spravované aplikace v hello Marketplace](managed-application-consume-marketplace.md).
* Informace o publikování aplikace spravované katalogu služeb najdete v tématu [vytvoření a publikování aplikace spravované katalogu služeb](managed-application-publishing.md).
* Informace o použití katalogu služeb spravovaných aplikací najdete v tématu [využívat katalogu služeb, které jsou spravované aplikace](managed-application-consumption.md).
* toocreate soubor definice uživatelského rozhraní, najdete v části [začít pracovat s CreateUiDefinition](managed-application-createuidefinition-overview.md).
