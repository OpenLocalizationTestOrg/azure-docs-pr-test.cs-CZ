---
title: "aaaAzure služby Security Center a Azure SQL Database | Microsoft Docs"
description: "Tento článek ukazuje, jak Security Center vám může pomoct zabezpečit vaše databáze v Azure SQL Database."
services: sql-database
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: f109adfd-daed-4257-9692-2042a1399480
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 173590500f0ce64140f214ada24b9692e01dbd4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-and-azure-sql-database-service"></a>Služba Azure Security Center a Azure SQL Database
[Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/) pomáhá zabránit, zjistit a reagovat toothreats. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

Tento článek ukazuje, jak Security Center vám může pomoct zabezpečit vaše databáze v Azure SQL Database.

## <a name="why-use-security-center"></a>Proč používat Security Center?
Security Center pomáhá zabezpečit data v databázi SQL tím, že poskytuje přehled o zabezpečení hello serverů a databází. Security Center můžete:

* Definujte zásady pro šifrování SQL Database a auditování.
* Monitorování hello zabezpečení prostředků databáze SQL ve vašich předplatných.
* Rychle identifikovat a opravit problémy se zabezpečením.
* Integrovat výstrahy z [detekce hrozeb Azure SQL Database](../sql-database/sql-database-threat-detection.md).

Kromě toho toohelping chránit prostředky SQL Database, Security Center nabízí také monitorování zabezpečení a správy pro virtuální počítače Azure, cloudové služby, aplikační služby, virtuální sítě a další. Další informace o službě Security Center [zde](security-center-intro.md).

## <a name="prerequisites"></a>Požadavky
tooget začít s Security Center, musíte mít tooMicrosoft předplatné Azure. úroveň Free Hello služby Security Center je povolená s vaším předplatným. Další informace o volné a standardní úrovně Security Center, najdete v části [Security Center ceny](https://azure.microsoft.com/pricing/details/security-center/).

Security Center podporuje přístup na základě rolí. toolearn Další informace o řízení přístupu na základě role (RBAC) v Azure, najdete v části [řízení přístupu na základě Role v Azure Active Directory](../active-directory/role-based-access-control-configure.md). Hello Security Center – nejčastější dotazy poskytuje informace o [zpracování oprávnění ve službě Security Center](security-center-faq.md#permissions).

## <a name="access-security-center"></a>Přístup ke službě Security Center
Security Center je přístupná z hello [portál Azure](https://azure.microsoft.com/features/azure-portal/). [Přihlaste se toohello portál](https://portal.azure.com/) a vyberte hello **Security Center možnost**.

![Možnost Security Center][1]

Hello **Security Center** otevře se okno.
![Okno Security Center][2]

## <a name="set-security-policy"></a>Nastavení zásad zabezpečení
Zásady zabezpečení definuje hello sadu ovládacích prvků, které se doporučují pro prostředky v rámci zadané předplatné nebo prostředek skupiny hello. V Security Center určíte zásady pro předplatné nebo prostředek skupiny podle potřeb zabezpečení tooyour společnosti a hello typu aplikací nebo citlivosti dat hello v každém předplatném.

Můžete nastavit zásady tooshow doporučení pro auditování SQL a SQL transparentní šifrování dat (šifrování TDE).

* Když zapnete **auditování SQL a zjišťování hrozeb**, Security Center doporučuje pro dodržování předpisů, rozšířeného zjišťování a vyšetřování účely musí být povoleno auditování přístupu k tooAzure databáze.
* Když zapnete **SQL transparentní šifrování dat**, Security Center doporučí šifrování povolit pro vaši databázi SQL Azure, přidružených záloh a souborů protokolů transakci.

tooset zásady zabezpečení, vyberte hello **zásad** na okno Security Center hello dlaždici. Na hello **zásady zabezpečení** okně, vyberte hello předplatné, na kterém chcete zásady zabezpečení tooenable hello. Vyberte **zásada Zabránění** a vypnout **na** hello doporučení zabezpečení, které chcete toouse u tohoto předplatného.
![Zásady zabezpečení][3]

Další, najdete v části toolearn [nastavovat zásady zabezpečení](security-center-policies.md).

## <a name="manage-security-recommendation"></a>Správa doporučení zabezpečení
Security Center pravidelně analyzuje stav zabezpečení hello vašich prostředků Azure. Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří doporučení. Hello doporučení vás provede procesem hello konfigurace hello potřebné ovládací prvky.

Jakmile nastavíte zásadu zabezpečení, Security Center analyzuje stav zabezpečení hello vaše prostředky tooidentify potenciální ohrožení zabezpečení. Hello doporučení se zobrazí ve formátu tabulky, kde každý řádek představuje jeden konkrétní doporučení. Pomocí následující tabulky jako odkaz toohelp, rozumíte hello dostupná doporučení pro Azure SQL Database a jaké jednotlivá doporučení nepodporuje, pokud ji použijete hello. Výběr doporučení přejdete tooan článek, který vysvětluje, jak tooimplement hello doporučení ve službě Security Center.

| Doporučení | Popis |
| --- | --- |
| [Povolení auditování a zjišťování hrozeb na serverech SQL](security-center-enable-auditing-on-sql-servers.md) |Doporučuje zapnout auditování a zjišťování hrozeb pro servery SQL Database. (Pouze služby SQL Database. Neobsahuje Microsoft SQL Server běžící na virtuálních počítačích.) |
| [Povolení auditování a zjišťování hrozeb v databázích SQL](security-center-enable-auditing-on-sql-databases.md) |Doporučuje zapnout auditování a zjišťování hrozeb pro databáze SQL Database. (Pouze služby SQL Database. Neobsahuje Microsoft SQL Server běžící na virtuálních počítačích.) |
| [Povolení transparentního šifrování dat](security-center-enable-transparent-data-encryption.md) |Doporučuje se, že povolíte šifrování pro databáze SQL. (Služba SQL Database pouze.) |

toosee doporučení pro vaše prostředky Azure, vyberte hello **doporučení** na okno Security Center hello dlaždici. Na hello **doporučení** okně vyberte podrobnosti toosee doporučení. V tomto příkladu budeme vyberte **povolení auditování a detekce hrozeb na serverech SQL**.

![Doporučení][4]

Jak ukazuje níže ukazuje Security Center hello SQL servery, kde nejsou povoleno auditování a zjišťování hrozeb. Po zapnutí auditování, můžete nakonfigurovat nastavení detekce hrozeb a e-mailu tooreceive nastavení výstrah zabezpečení. Detekce hrozeb vás upozorní, když se zjistila nezvyklé databázové aktivity, které indikují potenciální bezpečnostní hrozby toohello databáze. Hello výstrahy se zobrazují na řídicím panelu Security Center hello.
![Auditování a zjišťování hrozeb][5]

Postupujte podle kroků hello v [detekce hrozeb SQL Database v hello portál Azure](../sql-database/sql-database-threat-detection-portal.md) tooturn na a konfigurace detekce hrozeb a tooconfigure hello seznam e-mailů, které budou dostávat upozornění zabezpečení při zjištění nezvyklých aktivit.

toolearn Další informace o doporučení, najdete v části [Správa doporučení zabezpečení](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Monitorování stavu zabezpečení
Po povolení [zásady zabezpečení](security-center-policies.md) pro prostředky předplatného bude Security Center analyzovat zabezpečení hello vaše prostředky tooidentify potenciální ohrožení zabezpečení.  Hello stav zabezpečení vašich prostředků můžete zobrazit v hello **stav zabezpečení prostředků** dlaždici. Když kliknete na tlačítko **Data** v hello **stav zabezpečení prostředků** dlaždici hello **datové prostředky** otevře se okno s SQL doporučeními pro problémy, jako je například auditování a transparentní šifrování dat není povoleno. Také obsahuje doporučení pro hello obecný stav databáze hello.
![Stav zabezpečení prostředků][6]

Další, najdete v části toolearn [sledování stavu zabezpečení](security-center-monitoring.md).

## <a name="manage-and-respond-toosecurity-alerts"></a>Spravovat a reagovat toosecurity výstrahy
Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z [detekce hrozeb SQL Azure](../sql-database/sql-database-threat-detection.md), stejně jako ostatní prostředky Azure, toodetect skutečné hrozby a snížil počet falešných poplachů. Seznam upřednostňovaných výstrah zabezpečení se zobrazí v Centru zabezpečení společně s hello informace, které potřebujete tooquickly prozkoumat hello problému a doporučení, jak tooremediate útoku.

toosee výstrahy, vyberte hello **výstrahy zabezpečení** na okno Security Center hello dlaždici. Na hello **výstrahy zabezpečení** vyberte výstrahy toolearn Další informace o hello události, které aktivuje výstraha hello a co, pokud existuje, kroky, které potřebují tootake tooremediate útoku. V tomto příkladu budeme vyberte **Injektáž SQL potenciální**.
![Výstrahy zabezpečení][7]

Jak vidíte níže, Security Center nabízí další podrobnosti, které nabízejí přehled o jaké výstrahy spouštěná hello hello cíle prostředku, pokud příslušné hello zdrojovou IP adresu a doporučení, jak tooremediate.
![Potenciální Injektáž SQL][8]

Další, najdete v části toolearn [správy a zda odpovídá výstrahy toosecurity](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Další kroky
* [Security Center – nejčastější dotazy](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Průvodce plánováním a operace Security Center](security-center-planning-and-operations-guide.md) – postupujte podle sadu kroků a úloh toooptimize použití služby Security Center na základě požadavků zabezpečení vaší organizace a modelu správy cloudu.
* [Zabezpečení dat Security Center](security-center-data-security.md) – zjistěte, jak Security Center shromažďuje a zpracovává data o vašich prostředků Azure, včetně informací o konfiguraci, metadata, protokoly událostí, soubory se stavem systému a další.
* [Zpracování incidentů zabezpečení](security-center-incident.md) – zjistěte, jak výstrahy zabezpečení hello toouse funkci tooassist Security Center v zpracování incidentů zabezpečení.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
