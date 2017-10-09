---
title: "oprávnění aaaAuthentication a uživatele ve službě Azure Analysis Services | Microsoft Docs"
description: "Další informace o ověřování a uživatel oprávnění ve službě Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: dd49fd59eec1f68dfe8a0fe373fa612ac46de4e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-user-permissions"></a>Ověřování a uživatel oprávnění
Azure Analysis Services použije Azure Active Directory (Azure AD) pro ověřování identity management a uživatelů. Jakýkoli uživatel, vytváření, Správa nebo připojení tooan Azure Analysis Services server musí mít platné uživatelské identity [klienta Azure AD](../active-directory/active-directory-administer.md) v hello stejného předplatného.

Podporuje Azure Analysis Services [spolupráce Azure AD B2B](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md). S B2B můžete pozvat uživatele mimo organizaci jako uživatele typu Host v adresáři služby Azure AD. Hosté může být z jiného adresáře služby Azure AD klienta nebo žádné platné e-mailovou adresu. Jednou pozvali a hello uživatel přijme podmínky hello pozvánku k odesílání e-mailem z Azure, hello identita uživatele je přidána toohello adresář tenanta. Tyto identity lze přidat toosecurity skupiny nebo jako členy role správce nebo databázi serveru.

![Architektura ověřování Azure Analysis Services](./media/analysis-services-manage-users/aas-manage-users-arch.png)

## <a name="authentication"></a>Authentication
Všechny klientské aplikace a nástroje použít jeden nebo více hello služby Analysis Services [klientské knihovny](analysis-services-data-providers.md) (AMO, MSOLAP, ADOMD) tooconnect tooa serveru. 

Všechny tři klientské knihovny podporují interaktivní toku Azure AD a metody neinteraktivní ověřování. Dobrý den dvě metody neinteraktivní, metody hesla služby Active Directory a integrované ověřování Active Directory můžete použít v aplikacích využitím AMOMD a MSOLAP. Tyto dvě metody nikdy výsledkem automaticky otevíraná okna.

Klientské aplikace, jako třeba aplikace Excel a Power BI Desktop a nástroje, například aplikace SSMS a rozšíření SSDT nainstalujte hello nejnovější verze knihoven hello při aktualizaci toohello nejnovější verzi. Power BI Desktop, aplikace SSMS a rozšíření SSDT se aktualizují každý měsíc. Aplikace Excel je [aktualizován s Office 365](https://support.office.com/en-us/article/When-do-I-get-the-newest-features-in-Office-2016-for-Office-365-da36192c-58b9-4bc9-8d51-bb6eed468516). Aktualizace Office 365 jsou méně časté a některé organizace používat hello odložené kanál, jsou si toothree měsíců odložení význam aktualizace.

 V závislosti na hello klientská aplikace nebo nástroje, které používáte může být hello typ ověřování a jak přihlášení jiný. Každá aplikace může podporovat různé funkce pro připojení toocloud služby, jako je Azure Analysis Services.


### <a name="sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS)
Azure servery služby Analysis Services podporují připojení od [SSMS V17.1](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) a vyšší pomocí ověřování systému Windows, ověřování hesla Active Directory a univerzální ověřování služby Active Directory. Obecně se doporučuje, že abyste použili Universal ověřování služby Active Directory, protože:

*  Podporuje metody interaktivní i neinteraktivní ověřování.

*  Podporuje uživatele typu Host Azure B2B pozvat do klienta Azure AS hello. Při připojování serveru tooa, musíte uživatele typu Host při připojování serveru toohello vybrat Universal ověřování služby Active Directory.

*  Podporuje Vícefaktorové ověřování (MFA). Azure MFA pomáhá chránit přístup toodata a aplikace s celou řadu možností ověřování: telefonní hovor, textová zpráva, čipové karty s PIN kód nebo oznámení mobilní aplikace. Interaktivní vícefaktorového ověřování s Azure AD může způsobit v místním dialogovém okně pro ověření.

### <a name="sql-server-data-tools-ssdt"></a>SQL Server Data Tools (SSDT)
Rozšíření SSDT připojí tooAzure Analysis Services pomocí ověřování služby Active Directory Universal podpora vícefaktorového ověřování. Uživatelé se výzvami toosign v tooAzure na prvním nasazení hello pomocí jejich ID organizace (e-mailu). Uživatelé musí přihlásit tooAzure pomocí účtu s oprávněními správce serveru na server hello, které provádíte nasazení. Při přihlášení tooAzure hello poprvé, je přiřazen token. Rozšíření SSDT hello tokenu v paměti pro budoucí připojování ukládá do mezipaměti.

### <a name="power-bi-desktop"></a>Power BI Desktop
Power BI Desktop připojí tooAzure Analysis Services pomocí ověřování služby Active Directory Universal podpora vícefaktorového ověřování. Uživatelé se výzvami toosign v tooAzure na prvním připojení hello pomocí jejich ID organizace (e-mailu). Uživatelé musí přihlásit tooAzure pomocí účtu, který je součástí Správce serveru nebo role databáze.

### <a name="excel"></a>Excel
Uživatelé aplikace Excel můžete připojit tooa serveru pomocí účtu systému Windows, ID organizace (e-mailovou adresu) nebo externí e-mailovou adresu. Externí e-mailové identity musí existovat v hello Azure AD jako uživatel guest.

## <a name="user-permissions"></a>Uživatelská oprávnění

**Správci serveru** jsou tooan konkrétní instanci serveru Azure Analysis Services. Připojení pomocí nástrojů, například portál Azure, aplikace SSMS a rozšíření SSDT tooperform úlohám jako přidání databází a Správa rolí uživatelů. Ve výchozím nastavení se automaticky přidá hello uživatele, který vytvoří hello serveru jako správce serveru služby Analysis Services. Další správci můžete přidat pomocí portálu Azure nebo SSMS. Správci serveru musí mít účet v klientovi hello Azure AD v hello stejného předplatného. Další, najdete v části toolearn [spravovat správce serveru](analysis-services-server-admins.md). 


**Databáze uživatelů** připojení databáze toomodel pomocí klientské aplikace jako aplikace Excel nebo produktu Power BI. Uživatelé musí být přidaný toodatabase role. Role databáze definují, správce procesu nebo oprávnění ke čtení pro databázi. Je důležité toounderstand databáze uživatelů v roli s oprávněními správce se liší od správce serveru. Ve výchozím nastavení, správci serveru jsou však také správci databází. Další, najdete v části toolearn [spravovat role databáze a uživatele](analysis-services-database-users.md).

**Vlastníci prostředků Azure**. Vlastníci prostředků spravovat prostředky pro předplatné Azure. Vlastníci prostředků můžete přidat tooOwner identity uživatele Azure AD nebo role Přispěvatel v rámci předplatného pomocí **řízení přístupu** na portálu Azure nebo pomocí šablony Azure Resource Manager. 

![Řízení přístupu na portálu Azure](./media/analysis-services-manage-users/aas-manage-users-rbac.png)

Role na této úrovni použít toousers nebo účty, které je třeba tooperform úlohy, které lze dokončit hello portálu nebo pomocí šablony Azure Resource Manager. Další, najdete v části toolearn [řízení přístupu na základě Role](../active-directory/role-based-access-control-what-is.md). 


## <a name="database-roles"></a>Databázové role

 Role pro tabulkový model definován jsou databázové role. To znamená hello role obsahovat členy skládající se z Azure AD Uživatelé a skupiny zabezpečení, které mít specifické oprávnění, které definují akce hello členů, může trvat na modelové databáze. Databázové role je vytvořen jako samostatný objekt v databázi hello a platí pouze toohello databáze, ve kterém se vytvoří dané roli.   
  
 Ve výchozím nastavení když vytvoříte nový projekt tabulkový model, projekt modelu hello nemá žádné role. Role lze definovat pomocí dialogové okno Správce rolí hello v sadě SSDT. Role jsou definovány při návrhu projektu modelu, jsou použité pouze toohello modelu pracovní prostor databáze. Při nasazení modelu hello hello stejné role jsou použité toohello nasadit model. Po nasazení model serveru a správci databází můžete spravovat role a členy pomocí aplikace SSMS. Další, najdete v části toolearn [spravovat role databáze a uživatele](analysis-services-database-users.md).
  


## <a name="next-steps"></a>Další kroky

[Spravovat přístup tooresources pomocí skupin Azure Active Directory](../active-directory/active-directory-manage-groups.md)   
[Spravovat role databáze a uživatele](analysis-services-database-users.md)  
[Správa správců serveru](analysis-services-server-admins.md)  
[Řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md)  