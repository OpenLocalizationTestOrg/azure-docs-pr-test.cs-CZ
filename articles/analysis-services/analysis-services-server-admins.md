---
title: "Správa serveru admins ve službě Azure Analysis Services | Microsoft Docs"
description: "Naučte se spravovat správci serveru pro server služby Analysis Services v Azure."
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
ms.openlocfilehash: a1b58125dafdf73f245b6a8cd0f4917513b22ea9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-server-administrators"></a><span data-ttu-id="1d6fb-103">Spravovat správce serveru</span><span class="sxs-lookup"><span data-stu-id="1d6fb-103">Manage server administrators</span></span>
<span data-ttu-id="1d6fb-104">Správci serveru musí být platný uživatel nebo skupina ve službě Azure Active Directory (Azure AD) pro klienta, ve kterém se nachází server.</span><span class="sxs-lookup"><span data-stu-id="1d6fb-104">Server administrators must be a valid user or group in the Azure Active Directory (Azure AD) for the tenant in which the server resides.</span></span> <span data-ttu-id="1d6fb-105">Můžete použít **Analysis Services Admins** v okně Ovládací prvek pro server na portálu Azure nebo vlastnosti serveru v aplikaci SSMS ke správě správci serveru.</span><span class="sxs-lookup"><span data-stu-id="1d6fb-105">You can use **Analysis Services Admins** in the control blade for your server in Azure portal, or Server Properties in SSMS to manage server administrators.</span></span> 

## <a name="to-add-server-administrators-by-using-azure-portal"></a><span data-ttu-id="1d6fb-106">Přidat správce serveru pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1d6fb-106">To add server administrators by using Azure portal</span></span>
1. <span data-ttu-id="1d6fb-107">V okně Ovládací prvek pro váš server, klikněte na tlačítko **Analysis Services Admins**.</span><span class="sxs-lookup"><span data-stu-id="1d6fb-107">In the control blade for your server, click **Analysis Services Admins**.</span></span>
2. <span data-ttu-id="1d6fb-108">V  **\<servername >-Analysis Services Admins** okně klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="1d6fb-108">In the **\<servername> - Analysis Services Admins** blade, click **Add**.</span></span>
3. <span data-ttu-id="1d6fb-109">V **přidat správci serveru** okně vyberte uživatelské účty ze služby Azure AD a zvaní externí uživatele podle e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="1d6fb-109">In the **Add Server Administrators** blade, select user accounts from your Azure AD or invite external users by email address.</span></span>

    ![Správci serveru na portálu Azure](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a><span data-ttu-id="1d6fb-111">Přidat správce serveru pomocí aplikace SSMS</span><span class="sxs-lookup"><span data-stu-id="1d6fb-111">To add server administrators by using SSMS</span></span>
1. <span data-ttu-id="1d6fb-112">Pravým tlačítkem na server > **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="1d6fb-112">Right-click the server > **Properties**.</span></span>
2. <span data-ttu-id="1d6fb-113">V **vlastnosti serveru pro analýzu**, klikněte na tlačítko **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="1d6fb-113">In **Analysis Server Properties**, click **Security**.</span></span>
3. <span data-ttu-id="1d6fb-114">Klikněte na tlačítko **přidat**a potom zadejte e-mailovou adresu pro uživatele nebo skupiny ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1d6fb-114">Click **Add**, and then enter the email address for a user or group in your Azure AD.</span></span>
   
    ![Přidat správce serveru v aplikaci SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a><span data-ttu-id="1d6fb-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d6fb-116">Next steps</span></span> 
[<span data-ttu-id="1d6fb-117">Ověřování a uživatelská oprávnění</span><span class="sxs-lookup"><span data-stu-id="1d6fb-117">Authentication and user permissions</span></span>](analysis-services-manage-users.md)  
[<span data-ttu-id="1d6fb-118">Spravovat role databáze a uživatele</span><span class="sxs-lookup"><span data-stu-id="1d6fb-118">Manage database roles and users</span></span>](analysis-services-database-users.md)  
[<span data-ttu-id="1d6fb-119">Řízení přístupu na základě rolí</span><span class="sxs-lookup"><span data-stu-id="1d6fb-119">Role-Based Access Control</span></span>](../active-directory/role-based-access-control-what-is.md)  

