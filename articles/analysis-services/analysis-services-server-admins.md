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
# <a name="manage-server-administrators"></a><span data-ttu-id="cf03f-103">Spravovat správce serveru</span><span class="sxs-lookup"><span data-stu-id="cf03f-103">Manage server administrators</span></span>
<span data-ttu-id="cf03f-104">Správci serveru musí být platný uživatel nebo skupina v hello Azure Active Directory (Azure AD) pro klienta hello, ve které hello nachází server.</span><span class="sxs-lookup"><span data-stu-id="cf03f-104">Server administrators must be a valid user or group in hello Azure Active Directory (Azure AD) for hello tenant in which hello server resides.</span></span> <span data-ttu-id="cf03f-105">Můžete použít **Analysis Services Admins** v okně hello ovládací prvek pro server na portálu Azure nebo vlastnosti serveru ve Správci serveru toomanage SSMS.</span><span class="sxs-lookup"><span data-stu-id="cf03f-105">You can use **Analysis Services Admins** in hello control blade for your server in Azure portal, or Server Properties in SSMS toomanage server administrators.</span></span> 

## <a name="tooadd-server-administrators-by-using-azure-portal"></a><span data-ttu-id="cf03f-106">Správci serveru tooadd pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cf03f-106">tooadd server administrators by using Azure portal</span></span>
1. <span data-ttu-id="cf03f-107">V okně hello ovládací prvek pro váš server, klikněte na tlačítko **Analysis Services Admins**.</span><span class="sxs-lookup"><span data-stu-id="cf03f-107">In hello control blade for your server, click **Analysis Services Admins**.</span></span>
2. <span data-ttu-id="cf03f-108">V hello  **\<servername >-Analysis Services Admins** okně klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="cf03f-108">In hello **\<servername> - Analysis Services Admins** blade, click **Add**.</span></span>
3. <span data-ttu-id="cf03f-109">V hello **přidat správci serveru** okně vyberte uživatelské účty ze služby Azure AD a zvaní externí uživatele podle e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="cf03f-109">In hello **Add Server Administrators** blade, select user accounts from your Azure AD or invite external users by email address.</span></span>

    ![Správci serveru na portálu Azure](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="tooadd-server-administrators-by-using-ssms"></a><span data-ttu-id="cf03f-111">Správci serveru tooadd pomocí aplikace SSMS</span><span class="sxs-lookup"><span data-stu-id="cf03f-111">tooadd server administrators by using SSMS</span></span>
1. <span data-ttu-id="cf03f-112">Klikněte pravým tlačítkem na server hello > **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="cf03f-112">Right-click hello server > **Properties**.</span></span>
2. <span data-ttu-id="cf03f-113">V **vlastnosti serveru pro analýzu**, klikněte na tlačítko **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="cf03f-113">In **Analysis Server Properties**, click **Security**.</span></span>
3. <span data-ttu-id="cf03f-114">Klikněte na tlačítko **přidat**a pak zadejte hello e-mailovou adresu pro uživatele nebo skupiny ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cf03f-114">Click **Add**, and then enter hello email address for a user or group in your Azure AD.</span></span>
   
    ![Přidat správce serveru v aplikaci SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a><span data-ttu-id="cf03f-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cf03f-116">Next steps</span></span> 
[<span data-ttu-id="cf03f-117">Ověřování a uživatelská oprávnění</span><span class="sxs-lookup"><span data-stu-id="cf03f-117">Authentication and user permissions</span></span>](analysis-services-manage-users.md)  
[<span data-ttu-id="cf03f-118">Spravovat role databáze a uživatele</span><span class="sxs-lookup"><span data-stu-id="cf03f-118">Manage database roles and users</span></span>](analysis-services-database-users.md)  
[<span data-ttu-id="cf03f-119">Řízení přístupu na základě role</span><span class="sxs-lookup"><span data-stu-id="cf03f-119">Role-Based Access Control</span></span>](../active-directory/role-based-access-control-what-is.md)  

