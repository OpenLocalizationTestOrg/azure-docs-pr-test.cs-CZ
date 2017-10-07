---
title: "aaaManage serveru admins ve službě Azure Analysis Services | Microsoft Docs"
description: "Zjistěte, jak správci serveru toomanage pro serveru služby Analysis Services v Azure."
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
ms.openlocfilehash: e04387e48e9b9483c382ee5cc9fd65f8331fb2a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-server-administrators"></a>Spravovat správce serveru
Správci serveru musí být platný uživatel nebo skupina v hello Azure Active Directory (Azure AD) pro klienta hello, ve které hello nachází server. Můžete použít **Analysis Services Admins** v okně hello ovládací prvek pro server na portálu Azure nebo vlastnosti serveru ve Správci serveru toomanage SSMS. 

## <a name="tooadd-server-administrators-by-using-azure-portal"></a>Správci serveru tooadd pomocí portálu Azure
1. V okně hello ovládací prvek pro váš server, klikněte na tlačítko **Analysis Services Admins**.
2. V hello  **\<servername >-Analysis Services Admins** okně klikněte na tlačítko **přidat**.
3. V hello **přidat správci serveru** okně vyberte uživatelské účty ze služby Azure AD a zvaní externí uživatele podle e-mailovou adresu.

    ![Správci serveru na portálu Azure](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="tooadd-server-administrators-by-using-ssms"></a>Správci serveru tooadd pomocí aplikace SSMS
1. Klikněte pravým tlačítkem na server hello > **vlastnosti**.
2. V **vlastnosti serveru pro analýzu**, klikněte na tlačítko **zabezpečení**.
3. Klikněte na tlačítko **přidat**a pak zadejte hello e-mailovou adresu pro uživatele nebo skupiny ve službě Azure AD.
   
    ![Přidat správce serveru v aplikaci SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a>Další kroky 
[Ověřování a uživatelská oprávnění](analysis-services-manage-users.md)  
[Spravovat role databáze a uživatele](analysis-services-database-users.md)  
[Řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md)  

