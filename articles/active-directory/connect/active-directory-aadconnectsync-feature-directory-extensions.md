---
title: "Synchronizace Azure AD Connect: rozšíření adresáře | Microsoft Docs"
description: "Toto téma popisuje funkce rozšíření hello adresáře v Azure AD Connect."
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
ms.openlocfilehash: 31525ae914aa4d9e047ea1515b460a8311d5c815
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-directory-extensions"></a><span data-ttu-id="13787-103">Synchronizace Azure AD Connect: rozšíření adresáře</span><span class="sxs-lookup"><span data-stu-id="13787-103">Azure AD Connect sync: Directory extensions</span></span>
<span data-ttu-id="13787-104">Rozšíření adresáře můžete tooextend hello schématu ve službě Azure AD s vlastními atributy z místní služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="13787-104">Directory extensions allows you tooextend hello schema in Azure AD with your own attributes from on-premises Active Directory.</span></span> <span data-ttu-id="13787-105">Tato funkce umožňuje využívání atributy pokračovat toomanage místní toobuild obchodní aplikace.</span><span class="sxs-lookup"><span data-stu-id="13787-105">This feature allows you toobuild LOB apps consuming attributes you continue toomanage on-premises.</span></span> <span data-ttu-id="13787-106">Tyto atributy mohou být využívány prostřednictvím [rozšíření adresáře Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) nebo [Microsoft Graph](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="13787-106">These attributes can be consumed through [Azure AD Graph directory extensions](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) or [Microsoft Graph](https://graph.microsoft.io/).</span></span> <span data-ttu-id="13787-107">Můžete zobrazit hello atributů není k dispozici pomocí [explorer Azure AD Graph](https://graphexplorer.cloudapp.net) a [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="13787-107">You can see hello attributes available using [Azure AD Graph explorer](https://graphexplorer.cloudapp.net) and [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) respectively.</span></span>

<span data-ttu-id="13787-108">Žádné úlohy Office 365 v současné době využívá tyto atributy.</span><span class="sxs-lookup"><span data-stu-id="13787-108">At present no Office 365 workload consumes these attributes.</span></span>

<span data-ttu-id="13787-109">Můžete nakonfigurovat další atributy, které chcete toosynchronize v cestě hello vlastní nastavení v Průvodci instalací hello.</span><span class="sxs-lookup"><span data-stu-id="13787-109">You configure which additional attributes you want toosynchronize in hello custom settings path in hello installation wizard.</span></span>
<span data-ttu-id="13787-110">![Průvodce rozšíření schématu](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)</span><span class="sxs-lookup"><span data-stu-id="13787-110">![Schema Extension Wizard](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)</span></span>  
<span data-ttu-id="13787-111">instalace Hello ukazuje hello atributy, které jsou platné kandidáty následující:</span><span class="sxs-lookup"><span data-stu-id="13787-111">hello installation shows hello following attributes, which are valid candidates:</span></span>

* <span data-ttu-id="13787-112">Typy objektů uživatelů a skupin</span><span class="sxs-lookup"><span data-stu-id="13787-112">User and Group object types</span></span>
* <span data-ttu-id="13787-113">Jednohodnotové atributy: řetězec, logická hodnota, celé číslo, binární</span><span class="sxs-lookup"><span data-stu-id="13787-113">Single-valued attributes: String, Boolean, Integer, Binary</span></span>
* <span data-ttu-id="13787-114">Více hodnot atributů: String, binární</span><span class="sxs-lookup"><span data-stu-id="13787-114">Multi-valued attributes: String, Binary</span></span>

<span data-ttu-id="13787-115">Hello seznam atributů je načten z mezipaměti schématu hello vytvořen během instalace služby Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="13787-115">hello list of attributes is read from hello schema cache created during installation of Azure AD Connect.</span></span> <span data-ttu-id="13787-116">Pokud jste rozšířili schéma služby Active Directory hello s další atributy, pak hello [je třeba aktualizovat schéma](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) před tyto nové atributy jsou viditelné.</span><span class="sxs-lookup"><span data-stu-id="13787-116">If you have extended hello Active Directory schema with additional attributes, then hello [schema must be refreshed](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) before these new attributes are visible.</span></span>

<span data-ttu-id="13787-117">Objekt ve službě Azure AD může mít až too100 atributů rozšíření adresáře.</span><span class="sxs-lookup"><span data-stu-id="13787-117">An object in Azure AD can have up too100 directory extensions attributes.</span></span> <span data-ttu-id="13787-118">Maximální délka Hello je 250 znaků.</span><span class="sxs-lookup"><span data-stu-id="13787-118">hello max length is 250 characters.</span></span> <span data-ttu-id="13787-119">Pokud hodnota atributu je delší, zkrátí se synchronizační modul hello.</span><span class="sxs-lookup"><span data-stu-id="13787-119">If an attribute value is longer, then it is truncated by hello sync engine.</span></span>

<span data-ttu-id="13787-120">Během instalace služby Azure AD Connect je zaregistrován aplikace, které jsou k dispozici tyto atributy.</span><span class="sxs-lookup"><span data-stu-id="13787-120">During installation of Azure AD Connect, an application is registered where these attributes are available.</span></span> <span data-ttu-id="13787-121">Můžete zobrazit tuto aplikaci v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="13787-121">You can see this application in hello Azure portal.</span></span>  
![Aplikace rozšíření schématu](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

<span data-ttu-id="13787-123">Tyto atributy jsou nyní k dispozici prostřednictvím grafu:</span><span class="sxs-lookup"><span data-stu-id="13787-123">These attributes are now available through Graph:</span></span>  
![Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

<span data-ttu-id="13787-125">atributy Hello mají předponu rozšíření\_{AppClientId}\_.</span><span class="sxs-lookup"><span data-stu-id="13787-125">hello attributes are prefixed with extension\_{AppClientId}\_.</span></span> <span data-ttu-id="13787-126">Hello AppClientId má hello stejnou hodnotu pro všechny atributy v klientovi služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="13787-126">hello AppClientId has hello same value for all attributes in your Azure AD tenant.</span></span>

## <a name="next-steps"></a><span data-ttu-id="13787-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="13787-127">Next steps</span></span>
<span data-ttu-id="13787-128">Další informace o hello [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.</span><span class="sxs-lookup"><span data-stu-id="13787-128">Learn more about hello [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="13787-129">Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="13787-129">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
