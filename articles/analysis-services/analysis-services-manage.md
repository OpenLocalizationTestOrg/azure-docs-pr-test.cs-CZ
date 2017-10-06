---
title: aaaManage Azure Analysis Services | Microsoft Docs
description: "Zjistěte, jak toomanage Analysis Services serveru v Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 79491d0b-b00d-4e02-9ca7-adc99bc02fdb
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: b03bc440801a68162039e28cdb4f863da374014e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-analysis-services"></a>Správa služby Analysis Services
Po vytvoření serveru služby Analysis Services v Azure, může být některé úlohy správy a správy musíte tooperform hned nebo zopakovat dolů silniční hello. Například spusťte zpracování toohello aktualizace dat, určovat, kdo může získat přístup hello modely na vašem serveru, nebo sledování stavu vašeho serveru. Některé úlohy správy lze provést pouze na portálu Azure, jiné v SQL Server Management Studio (SSMS) a některé úlohy lze provádět buď.

## <a name="azure-portal"></a>portál Azure
[Portál Azure](http://portal.azure.com/) je, kde můžete vytvořit a odstraněním serverů, monitorování prostředky serveru, změnit velikost a nastavit, kdo má přístup tooyour servery.  Pokud máte nějaké problémy, můžete také odeslat žádost o podporu.

![Získání názvu serveru v Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Připojení serveru tooyour v Azure je stejně jako připojení tooa instance serveru ve vaší vlastní organizaci. Pomocí aplikace SSMS je možné provádět řadu hello stejné úlohy, jako je například zpracování dat nebo vytvořte skript zpracování, správě rolí a pomocí prostředí PowerShell.
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a>Stažení a instalace aplikace SSMS
tooget všechny hello nejnovější funkce a možnosti nejhladší hello při připojování serveru Azure Analysis Services tooyour, ujistěte se, že používáte nejnovější verzi aplikace SSMS hello. 

[Stáhněte si SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


### <a name="tooconnect-with-ssms"></a>tooconnect pomocí SSMS
 Při použití aplikace SSMS, než server hello připojování tooyour poprvé, ujistěte se, že vaše uživatelské jméno je součástí skupiny Admins služby Analysis hello. Další, najdete v části toolearn [správci serveru](#server-administrators) dále v tomto článku.

1. Než připojíte, je třeba název serveru tooget hello. V **portál Azure** > serveru > **přehled** > **název serveru**, název serveru hello kopie.
   
    ![Získání názvu serveru v Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. V aplikaci SSMS > **Průzkumník objektů**, klikněte na tlačítko **připojit** > **služby Analysis Services**.
3. V hello **připojit tooServer** dialogové okno, vložte v hello název serveru, a potom v **ověřování**, vyberte jednu z hello následující typy ověřování:
   
    **Ověřování systému Windows** toouse přihlašovací údaje doména\uživatelské jméno a heslo systému Windows.

    **Ověřování hesla Active Directory** toouse účet organizace. Například když připojení z jiné domény připojený počítač.

    **Univerzální ověřování služby Active Directory** toouse [neinteraktivní nebo vícefaktorového ověřování](../sql-database/sql-database-ssms-mfa-authentication.md). 
   
    ![V aplikaci SSMS připojit](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a>Správci serveru a databáze uživatelů
V Azure Analysis Services existují dva typy uživatelů, správce serveru a uživatele databáze. Oba typy uživatelů, musí být ve vašem Azure Active Directory a musí být určena organizační e-mailovou adresu nebo hlavní název uživatele. Další, najdete v části toolearn [ověřování a uživatel oprávnění](analysis-services-manage-users.md).


## <a name="troubleshooting-connection-problems"></a>Odstraňování potíží s připojením
Pokud se připojujete pomocí aplikace SSMS, pokud narazíte na problém, může být nutné tooclear hello přihlášení mezipaměti. Nic je uložená v mezipaměti toodisc. tooclear mezipaměti hello, zavřete a restartujte hello připojit proces. 

## <a name="next-steps"></a>Další kroky
Pokud již jste nenasadili nový server tooyour tabulkový model, teď je vhodná doba. Další, najdete v části toolearn [nasazení služby Analysis Services tooAzure](analysis-services-deploy.md).

Pokud jste nasadili server tooyour modelu, jste připravené tooconnect tooit pomocí klienta nebo prohlížeče. Další, najdete v části toolearn [načíst data ze serveru Azure Analysis Services](analysis-services-connect.md).

