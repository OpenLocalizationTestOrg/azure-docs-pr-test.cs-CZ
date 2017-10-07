---
title: aaaAdministrative jednotky management preview v Azure Active Directory
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
ms.openlocfilehash: ee2c7beb6f9f6292bbf3cdeab00801ac066ae0e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="administrative-units-management-in-azure-ad---public-preview"></a><span data-ttu-id="8517d-103">Správa administrativních jednotek ve službě Azure AD - verzi public preview</span><span class="sxs-lookup"><span data-stu-id="8517d-103">Administrative units management in Azure AD - public preview</span></span>
<span data-ttu-id="8517d-104">Tento článek popisuje administrativních jednotek – nový kontejner služby Active Directory Azure prostředků, které lze použít pro delegování oprávnění ke správě přes podmnožiny uživatelů a použití zásady tooa podmnožině uživatelů.</span><span class="sxs-lookup"><span data-stu-id="8517d-104">This article describes administrative units – a new Azure Active Directory container of resources that can be used for delegating administrative permissions over subsets of users and applying policies tooa subset of users.</span></span> <span data-ttu-id="8517d-105">V Azure Active Directory povolte administrativních jednotek centrální správci toodelegate oprávnění tooregional správci nebo tooset zásad na podrobné úrovni.</span><span class="sxs-lookup"><span data-stu-id="8517d-105">In Azure Active Directory, administrative units enable central administrators toodelegate permissions tooregional administrators or tooset policy at a granular level.</span></span>

<span data-ttu-id="8517d-106">To je užitečné v organizacích s nezávislé rozdělení, například velké školy, která se skládá z mnoha autonomního škol (Business školy, školní inženýrství atd.), které jsou na sobě nezávislé.</span><span class="sxs-lookup"><span data-stu-id="8517d-106">This is useful in organizations with independent divisions, for example, a large university that is made up of many autonomous schools (Business school, Engineering school, and so on) which are independent from each other.</span></span> <span data-ttu-id="8517d-107">Takové rozdělení mají své vlastní správce IT, kteří řízení přístupu, Správa uživatelů a nastavit zásady speciálně pro jejich dělení.</span><span class="sxs-lookup"><span data-stu-id="8517d-107">Such divisions have their own IT administrators who control access, manage users, and set policies specifically for their division.</span></span> <span data-ttu-id="8517d-108">Centrální Správci mají možnost udělit toobe tato oprávnění správci oddělení přes hello uživatelům v jejich konkrétní rozdělení.</span><span class="sxs-lookup"><span data-stu-id="8517d-108">Central administrators want toobe able grant these divisional administrators permissions over hello users in their particular divisions.</span></span> <span data-ttu-id="8517d-109">Přesněji řečeno v tomto příkladu, centrální správce můžete, například vytvořit správce jednotku pro konkrétní školní (Business školní) a jeho naplnění jenom hello firmy školní uživatelé.</span><span class="sxs-lookup"><span data-stu-id="8517d-109">More specifically, using this example, a central administrator can, for instance, create an administrative unit for a particular school (Business school) and populate it with only hello Business school users.</span></span> <span data-ttu-id="8517d-110">Centrální správce potom může přidat hello firmy školní IT pracovníci tooa obor role, jinými slovy, udělte hello pracovníky IT firmy školní administrativní oprávnění jenom přes správu jednotky školní hello firmy.</span><span class="sxs-lookup"><span data-stu-id="8517d-110">Then a central administrator can add hello Business school IT staff tooa scoped role, in other words, grant hello IT staff of Business school administrative permissions only over hello Business school administrative unit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8517d-111">Role pro správu Správce obor jednotky můžete přiřadit pouze v případě, že povolíte Azure Active Directory Premium.</span><span class="sxs-lookup"><span data-stu-id="8517d-111">You can assign administrative unit-scoped admin roles only if you enable Azure Active Directory Premium.</span></span> <span data-ttu-id="8517d-112">Další informace najdete v tématu [Začínáme s Azure AD Premium](active-directory-get-started-premium.md).</span><span class="sxs-lookup"><span data-stu-id="8517d-112">For more information, see [Getting started with Azure AD Premium](active-directory-get-started-premium.md).</span></span>
>


<span data-ttu-id="8517d-113">Z hello správce centrální administrativní jednotky je objekt adresáře, který může být vytvořeny a naplněny s prostředky.</span><span class="sxs-lookup"><span data-stu-id="8517d-113">From hello central administrator’s point of view, an administrative unit is a directory object that can be created and populated with resources.</span></span> <span data-ttu-id="8517d-114">**V této verzi preview tyto prostředky mohou být pouze uživatelé.**</span><span class="sxs-lookup"><span data-stu-id="8517d-114">**In this preview release, these resources can be only users.**</span></span> <span data-ttu-id="8517d-115">Jakmile vytvořeny a naplněny, hello administrativní jednotky můžete využít jako text hello toorestrict obor oprávnění jen přes prostředky obsažené v hello administrativní jednotky.</span><span class="sxs-lookup"><span data-stu-id="8517d-115">Once created and populated, hello administrative unit can be used as a scope toorestrict hello granted permission only over resources contained in hello administrative unit.</span></span>

## <a name="managing-administrative-units"></a><span data-ttu-id="8517d-116">Správa administrativních jednotek</span><span class="sxs-lookup"><span data-stu-id="8517d-116">Managing administrative units</span></span>
<span data-ttu-id="8517d-117">V této verzi preview můžete vytvořit a spravovat administrativních jednotek rutin hello Azure Active Directory modul pro prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8517d-117">In this preview release, you can create and manage administrative units using hello Azure Active Directory Module for Windows PowerShell cmdlets.</span></span> <span data-ttu-id="8517d-118">Další informace o tom toolearn toodo, který najdete v části [práce s administrativních jednotek](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)</span><span class="sxs-lookup"><span data-stu-id="8517d-118">toolearn more about how toodo that, see [Working with Administrative Units](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)</span></span>

<span data-ttu-id="8517d-119">Další informace o instalaci modulu hello Azure AD a požadavky na software a informace o hello rutiny modulu Azure AD pro správu administrativních jednotek, včetně syntaxe, popisy parametrů a příklady naleznete v tématu [Azure Active Prostředí PowerShell Directory](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="8517d-119">For more information on software requirements and installing hello Azure AD module, and for information on hello Azure AD Module cmdlets for managing administrative units, including syntax, parameter descriptions, and examples, see [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8517d-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8517d-120">Next steps</span></span>
[<span data-ttu-id="8517d-121">Edice služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8517d-121">Azure Active Directory editions</span></span>](active-directory-editions.md)
