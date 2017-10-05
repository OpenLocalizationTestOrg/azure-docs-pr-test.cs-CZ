---
title: "Náhled správy administrativních jednotek v Azure Active Directory"
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
ms.openlocfilehash: e12a0aea8264b1ea67c26294ec5bbe9c404a171e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="administrative-units-management-in-azure-ad---public-preview"></a><span data-ttu-id="3320d-103">Správa administrativních jednotek ve službě Azure AD - verzi public preview</span><span class="sxs-lookup"><span data-stu-id="3320d-103">Administrative units management in Azure AD - public preview</span></span>
<span data-ttu-id="3320d-104">Tento článek popisuje administrativních jednotek – nový kontejner služby Active Directory Azure prostředků, které lze použít pro delegování oprávnění ke správě přes podmnožiny uživatelů a použití zásady na určitou podskupinu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3320d-104">This article describes administrative units – a new Azure Active Directory container of resources that can be used for delegating administrative permissions over subsets of users and applying policies to a subset of users.</span></span> <span data-ttu-id="3320d-105">Administrativních jednotek v Azure Active Directory, povolte centrální správcům delegovat oprávnění pro místní správce nebo nastavení zásad na nejnižší úrovni.</span><span class="sxs-lookup"><span data-stu-id="3320d-105">In Azure Active Directory, administrative units enable central administrators to delegate permissions to regional administrators or to set policy at a granular level.</span></span>

<span data-ttu-id="3320d-106">To je užitečné v organizacích s nezávislé rozdělení, například velké školy, která se skládá z mnoha autonomního škol (Business školy, školní inženýrství atd.), které jsou na sobě nezávislé.</span><span class="sxs-lookup"><span data-stu-id="3320d-106">This is useful in organizations with independent divisions, for example, a large university that is made up of many autonomous schools (Business school, Engineering school, and so on) which are independent from each other.</span></span> <span data-ttu-id="3320d-107">Takové rozdělení mají své vlastní správce IT, kteří řízení přístupu, Správa uživatelů a nastavit zásady speciálně pro jejich dělení.</span><span class="sxs-lookup"><span data-stu-id="3320d-107">Such divisions have their own IT administrators who control access, manage users, and set policies specifically for their division.</span></span> <span data-ttu-id="3320d-108">Centrální správci chcete mít možnost udělit tyto oddělení oprávnění správci přes uživatele v jejich konkrétní oddělení.</span><span class="sxs-lookup"><span data-stu-id="3320d-108">Central administrators want to be able grant these divisional administrators permissions over the users in their particular divisions.</span></span> <span data-ttu-id="3320d-109">Přesněji řečeno v tomto příkladu, centrální správce můžete, například vytvořit správce jednotku pro konkrétní školní (Business školní) a jeho naplnění pouze podnikoví uživatelé školní.</span><span class="sxs-lookup"><span data-stu-id="3320d-109">More specifically, using this example, a central administrator can, for instance, create an administrative unit for a particular school (Business school) and populate it with only the Business school users.</span></span> <span data-ttu-id="3320d-110">Jinými slovy, pak centrální správce můžete přidat školní obchodní pracovníky IT k roli vymezená udělte pracovníků IT firmy školní administrativní oprávnění jen přes školní správní organizační jednotky.</span><span class="sxs-lookup"><span data-stu-id="3320d-110">Then a central administrator can add the Business school IT staff to a scoped role, in other words, grant the IT staff of Business school administrative permissions only over the Business school administrative unit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3320d-111">Role pro správu Správce obor jednotky můžete přiřadit pouze v případě, že povolíte Azure Active Directory Premium.</span><span class="sxs-lookup"><span data-stu-id="3320d-111">You can assign administrative unit-scoped admin roles only if you enable Azure Active Directory Premium.</span></span> <span data-ttu-id="3320d-112">Další informace najdete v tématu [Začínáme s Azure AD Premium](active-directory-get-started-premium.md).</span><span class="sxs-lookup"><span data-stu-id="3320d-112">For more information, see [Getting started with Azure AD Premium](active-directory-get-started-premium.md).</span></span>
>


<span data-ttu-id="3320d-113">Z centrální správce administrativní jednotky je objekt adresáře, který může být vytvořeny a naplněny s prostředky.</span><span class="sxs-lookup"><span data-stu-id="3320d-113">From the central administrator’s point of view, an administrative unit is a directory object that can be created and populated with resources.</span></span> <span data-ttu-id="3320d-114">**V této verzi preview tyto prostředky mohou být pouze uživatelé.**</span><span class="sxs-lookup"><span data-stu-id="3320d-114">**In this preview release, these resources can be only users.**</span></span> <span data-ttu-id="3320d-115">Po vytvořeny a naplněny, lze jako obor pro správu jednotky omezit udělené oprávnění jen přes prostředky obsažené v administrativní jednotky.</span><span class="sxs-lookup"><span data-stu-id="3320d-115">Once created and populated, the administrative unit can be used as a scope to restrict the granted permission only over resources contained in the administrative unit.</span></span>

## <a name="managing-administrative-units"></a><span data-ttu-id="3320d-116">Správa administrativních jednotek</span><span class="sxs-lookup"><span data-stu-id="3320d-116">Managing administrative units</span></span>
<span data-ttu-id="3320d-117">V této verzi preview můžete vytvořit a spravovat administrativních jednotek pomocí Azure Active Directory modulu pro rutiny prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3320d-117">In this preview release, you can create and manage administrative units using the Azure Active Directory Module for Windows PowerShell cmdlets.</span></span> <span data-ttu-id="3320d-118">Další informace o tom, jak to udělat, najdete v části [práce s administrativních jednotek](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)</span><span class="sxs-lookup"><span data-stu-id="3320d-118">To learn more about how to do that, see [Working with Administrative Units](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)</span></span>

<span data-ttu-id="3320d-119">Další informace o požadavcích na software a instalace modulu Azure AD a informace o rutiny modulu Azure AD pro správu administrativních jednotek, včetně syntaxe, popisy parametrů a příklady naleznete v tématu [Azure Active Prostředí PowerShell Directory](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="3320d-119">For more information on software requirements and installing the Azure AD module, and for information on the Azure AD Module cmdlets for managing administrative units, including syntax, parameter descriptions, and examples, see [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3320d-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3320d-120">Next steps</span></span>
[<span data-ttu-id="3320d-121">Edice služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3320d-121">Azure Active Directory editions</span></span>](active-directory-editions.md)
