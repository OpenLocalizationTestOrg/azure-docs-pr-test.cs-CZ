---
title: "aaaManage osobních údajů v Microsoft Azure | Microsoft Docs"
description: "informace o tom, jak toocorrect, aktualizovat, odstranit a exportujte osobních údajů v Azure Active Directory a Azure SQL Database"
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.openlocfilehash: 032f70d32377cb9395cb2c35c27dad05001537c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-personal-data-in-microsoft-azure"></a>Správa osobních údajů v Microsoft Azure

Tento článek obsahuje pokyny k jak toocorrect, aktualizovat, odstranit a exportujte osobních údajů v Azure Active Directory a Azure SQL Database.

## <a name="scenario"></a>Scénář

Na základě Dublin společnost poskytuje centrální místo pro výkonných cílové svatby v Irsku a kolem hello, world pro obě na základní místní i mezinárodní zákazníka. Mají pobočky, zákazníky, zaměstnanci a dodavatelé nachází kolem hello world toofully služby hello místa, které nabízejí.

Mezi mnoha dalším položkám uchovává informace společnosti hello o RSVPs, které zahrnují alergií a podávání předvolby. Hosté svatební můžete zaregistrovat pro různé aktivity, například na koni JÍZDA, procházení webu, lodních lyžovačku atd. a dokonce vzájemné interakce na webové stránce Centrální během hello měsíců až toohello událostí. Hello společnosti shromažďuje z zaměstnanců, dodavatelů, zákazníků a svatební hosté osobní informace. Z důvodu hello mezinárodní povaha hello firmy hello společnosti musí být v souladu s více úrovněmi nařízení.

## <a name="problem-statement"></a>Popis problému

- Data správců musí být schopný toocorrect nesprávné osobní informace a aktualizace neúplný nebo změna osobní údaje.

- Třeba admins data musí být schopný toodelete osobní údaje na vyžádání hello subjektu data.

- Data admins potřebovat tooexport dat a poskytnout ho subjektu tooa data ve formátu běžné, strukturovaných při jeho žádost.

## <a name="company-goals"></a>Cíle společnosti

- Nesprávné nebo neúplné zákazníka, svatební hosta, zaměstnance a dodavatele osobní údaje, musí být opraveny nebo aktualizovány v Azure Active Directory a Azure SQL Database.

- Osobní údaje, třeba jej odstranit v Azure Active Directory a Azure SQL Database na vyžádání hello subjektu data.

- Osobní data musí být exportován ve formátu běžné, strukturovaných na vyžádání hello subjektu data.

## <a name="solutions"></a>Řešení

### <a name="azure-active-directory-rectifycorrect-inaccurate-or-incomplete-personal-data-and-erasedelete-personal-datauser-profiles"></a>Azure Active Directory: Tato napravila nebo opravte nesprávné nebo neúplné osobní údaje a vymazání a odstranění dat nebo uživatelských profilů

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) je společnosti Microsoft založená na cloudu, víceklientské adresáře a identity management service.
Opravte, aktualizovat nebo odstranit zákazníků a profily uživatelů a pracovní informace o uživateli, které obsahují osobní údaje, jako je například název, název pracovní, adresa nebo telefonní číslo uživatele v vaše [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) prostředí pomocí hello [portál Azure](https://portal.azure.com/).

Přihlaste se pod účtem, který je globálním správcem adresáře hello.

#### <a name="how-do-i-correct-or-update-user-profile-and-work-information-in-azure-active-directory"></a>Jak opravit nebo aktualizovat uživatelský profil a pracovní informace v Azure Active Directory?

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.

2. Vyberte **další služby**, zadejte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.

    ![média nebo image1.png](media/manage-personal-data-azure/image001.png)

3. Na hello **uživatelů a skupin** vyberte **uživatelé**.

    ![média nebo image2.png](media/manage-personal-data-azure/image003.png)

4. Na hello **uživatelé a skupiny - Uživatelé** okně vyberte uživatele ze seznamu hello a poté v okně hello hello vybraného uživatele, vyberte možnost **profil** tooview hello informace o profilu uživatele vyžadující toobe opravě nebo aktualizované.

    ![média nebo image3.png](media/manage-personal-data-azure/image005.png)

5. Opravte nebo aktualizovat informace o hello a pak v řádku nabídek hello vyberte **uložit.**

6.  V okně hello hello vybraného uživatele, vyberte **fungovat informace** tooview pracovní informace o uživateli musí toobe opraveny nebo aktualizovat.

    ![média nebo image4.png](media/manage-personal-data-azure/image007.png)

7. Opravte nebo aktualizovat informace o práci hello uživatele a pak v řádku nabídek hello vyberte **uložit.**

#### <a name="how-do-i-delete-a-user-profile-in-azure-active-directory"></a>Jak lze odstranit profil uživatele v Azure Active Directory?

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.

2. Vyberte **další služby**, zadejte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.

    ![](media/manage-personal-data-azure/image001.png)

3. Na hello **uživatelů a skupin** vyberte **uživatelé**.

    ![média nebo image2.png](media/manage-personal-data-azure/image003.png)

4. Na hello **uživatelé a skupiny - Uživatelé** okně vyberte uživatele ze seznamu hello.

    ![média nebo image3.png](media/manage-personal-data-azure/image007.png)

5. V okně hello hello vybraného uživatele, vyberte **přehled**a pak v řádku nabídek hello vyberte **odstranit**.

    ![](media/manage-personal-data-azure/image013.png)

### <a name="sql-database-rectifycorrect-inaccurate-or-incomplete-personal-data-erasedelete-personal-data-export-personal-data"></a>SQL Database: nápravě nebo opravte nesprávné nebo neúplné osobní údaje; smazání nebo odstranění osobních údajů; Exportovat osobní údaje 

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) je databáze cloudu, která pomáhá vývojářům vytvářet a udržovat aplikace.

Osobní data můžete aktualizovat v [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) pomocí standardní dotazy SQL a můžete také odstranit. Kromě toho je možné exportovat osobní data z databáze SQL pomocí různých metod, včetně hello Azure SQL Server import a export průvodce a v různých formátech, včetně souboru BACPAC souboru.

#### <a name="how-do-i-correct-update-or-erase-personal-data-in-sql-database"></a>Jak opravit, aktualizovat a vymazat osobní data v databázi SQL?

toolearn jak toocorrect nebo aktualizace osobní data v databázi SQL, navštivte hello [aktualizace (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql), [aktualizovat Text](https://docs.microsoft.com/sql/t-sql/queries/updatetext-transact-sql), [aktualizace pomocí výrazu běžné tabulky](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql), nebo [ Aktualizovat zápis textu](https://docs.microsoft.com/sql/t-sql/queries/writetext-transact-sql) dokumentaci.

toolearn jak toodelete osobní data v databázi SQL, navštivte hello [odstranit (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) dokumentaci.

#### <a name="how-do-i-export-personal-data-tooa-bacpac-file-in-sql-database"></a>Jak exportovat soubor souboru BACPAC tooa osobní data v databázi SQL?

Soubor souboru BACPAC zahrnuje hello databáze SQL data a metadata a je soubor zip s příponou souboru BACPAC. To lze provést pomocí hello [portál Azure](https://portal.azure.com/), hello SQLPackage nástroj příkazového řádku, SQL Server Management Studio (SSMS) nebo prostředí PowerShell.

toolearn jak tooexport data tooa souboru BACPAC souboru, navštivte hello [exportujte soubor Azure SQL database tooa souboru BACPAC](https://docs.microsoft.com/azure/sql-database/sql-database-export) stránku, která obsahuje podrobné pokyny pro jednotlivé metody uvedené výše.

Jak exportovat osobní data z databáze SQL s hello Microsoft SQL Server importovat a exportovat Průvodce?

Tento průvodce vám pomůže zkopírovat data z zdroj tooa cíl. Průvodce Úvod toohello, včetně jak tooget ji, informace o oprávnění a jak v hello nástroj, pomoci tooget navštívit hello [Import a Export dat s hello Import SQL serveru a Průvodce exportem](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard) webové stránky.

Přehled kroků průvodce hello najdete na adrese hello [kroky v hello Microsoft SQL Server importovat a exportovat průvodce](https://docs.microsoft.com/sql/integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard) webové stránky.

## <a name="next-steps"></a>Další kroky:

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) 

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

