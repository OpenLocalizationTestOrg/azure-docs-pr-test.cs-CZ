---
title: "Synchronizace Azure AD Connect: rozšíření adresáře | Microsoft Docs"
description: "Toto téma popisuje funkce rozšíření adresáře v Azure AD Connect."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 8ab9944437443a6d796345782f4f798aec96a0f6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-directory-extensions"></a><span data-ttu-id="43e8c-103">Synchronizace Azure AD Connect: rozšíření adresáře</span><span class="sxs-lookup"><span data-stu-id="43e8c-103">Azure AD Connect sync: Directory extensions</span></span>
<span data-ttu-id="43e8c-104">Rozšíření adresáře můžete rozšířit schéma ve službě Azure AD s vlastními atributy z místní služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="43e8c-104">Directory extensions allows you to extend the schema in Azure AD with your own attributes from on-premises Active Directory.</span></span> <span data-ttu-id="43e8c-105">Tato funkce umožňuje vytvářet obchodní aplikace, které využívají atributy nadále spravovat místní.</span><span class="sxs-lookup"><span data-stu-id="43e8c-105">This feature allows you to build LOB apps consuming attributes you continue to manage on-premises.</span></span> <span data-ttu-id="43e8c-106">Tyto atributy mohou být využívány prostřednictvím [rozšíření adresáře Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) nebo [Microsoft Graph](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="43e8c-106">These attributes can be consumed through [Azure AD Graph directory extensions](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) or [Microsoft Graph](https://graph.microsoft.io/).</span></span> <span data-ttu-id="43e8c-107">Můžete zobrazit dostupné atributy pomocí [Azure AD Graph explorer](https://graphexplorer.cloudapp.net) a [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="43e8c-107">You can see the attributes available using [Azure AD Graph explorer](https://graphexplorer.cloudapp.net) and [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) respectively.</span></span>

<span data-ttu-id="43e8c-108">Žádné úlohy Office 365 v současné době využívá tyto atributy.</span><span class="sxs-lookup"><span data-stu-id="43e8c-108">At present no Office 365 workload consumes these attributes.</span></span>

<span data-ttu-id="43e8c-109">Můžete nakonfigurovat další atributy, které chcete synchronizovat v cestě vlastní nastavení v Průvodci instalací.</span><span class="sxs-lookup"><span data-stu-id="43e8c-109">You configure which additional attributes you want to synchronize in the custom settings path in the installation wizard.</span></span>
<span data-ttu-id="43e8c-110">![Průvodce rozšíření schématu](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)</span><span class="sxs-lookup"><span data-stu-id="43e8c-110">![Schema Extension Wizard](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)</span></span>  
<span data-ttu-id="43e8c-111">Instalace ukazuje následující atributy, které jsou kandidáty platné:</span><span class="sxs-lookup"><span data-stu-id="43e8c-111">The installation shows the following attributes, which are valid candidates:</span></span>

* <span data-ttu-id="43e8c-112">Typy objektů uživatelů a skupin</span><span class="sxs-lookup"><span data-stu-id="43e8c-112">User and Group object types</span></span>
* <span data-ttu-id="43e8c-113">Jednohodnotové atributy: řetězec, logická hodnota, celé číslo, binární</span><span class="sxs-lookup"><span data-stu-id="43e8c-113">Single-valued attributes: String, Boolean, Integer, Binary</span></span>
* <span data-ttu-id="43e8c-114">Více hodnot atributů: String, binární</span><span class="sxs-lookup"><span data-stu-id="43e8c-114">Multi-valued attributes: String, Binary</span></span>

<span data-ttu-id="43e8c-115">V seznamu atributů je načten z mezipaměti schématu vytvořen během instalace služby Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="43e8c-115">The list of attributes is read from the schema cache created during installation of Azure AD Connect.</span></span> <span data-ttu-id="43e8c-116">Pokud jste rozšířili schéma Active Directory s další atributy, pak se [je třeba aktualizovat schéma](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) před tyto nové atributy jsou viditelné.</span><span class="sxs-lookup"><span data-stu-id="43e8c-116">If you have extended the Active Directory schema with additional attributes, then the [schema must be refreshed](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) before these new attributes are visible.</span></span>

<span data-ttu-id="43e8c-117">Objekt ve službě Azure AD může mít až 100 atributů rozšíření adresáře.</span><span class="sxs-lookup"><span data-stu-id="43e8c-117">An object in Azure AD can have up to 100 directory extensions attributes.</span></span> <span data-ttu-id="43e8c-118">Maximální délka je 250 znaků.</span><span class="sxs-lookup"><span data-stu-id="43e8c-118">The max length is 250 characters.</span></span> <span data-ttu-id="43e8c-119">Pokud hodnota atributu je delší, zkrátí se synchronizační modul.</span><span class="sxs-lookup"><span data-stu-id="43e8c-119">If an attribute value is longer, then it is truncated by the sync engine.</span></span>

<span data-ttu-id="43e8c-120">Během instalace služby Azure AD Connect je zaregistrován aplikace, které jsou k dispozici tyto atributy.</span><span class="sxs-lookup"><span data-stu-id="43e8c-120">During installation of Azure AD Connect, an application is registered where these attributes are available.</span></span> <span data-ttu-id="43e8c-121">Můžete zobrazit tuto aplikaci na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="43e8c-121">You can see this application in the Azure portal.</span></span>  
![Aplikace rozšíření schématu](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

<span data-ttu-id="43e8c-123">Tyto atributy jsou nyní k dispozici prostřednictvím grafu:</span><span class="sxs-lookup"><span data-stu-id="43e8c-123">These attributes are now available through Graph:</span></span>  
![Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

<span data-ttu-id="43e8c-125">Atributy mají předponu rozšíření\_{AppClientId}\_.</span><span class="sxs-lookup"><span data-stu-id="43e8c-125">The attributes are prefixed with extension\_{AppClientId}\_.</span></span> <span data-ttu-id="43e8c-126">AppClientId mají stejnou hodnotu pro všechny atributy v klientovi služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="43e8c-126">The AppClientId has the same value for all attributes in your Azure AD tenant.</span></span>

## <a name="next-steps"></a><span data-ttu-id="43e8c-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="43e8c-127">Next steps</span></span>
<span data-ttu-id="43e8c-128">Další informace o [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.</span><span class="sxs-lookup"><span data-stu-id="43e8c-128">Learn more about the [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="43e8c-129">Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="43e8c-129">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
