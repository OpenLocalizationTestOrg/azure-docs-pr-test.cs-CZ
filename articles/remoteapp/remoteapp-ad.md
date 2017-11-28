---
title: "aaaAzure AD + požadavky služby Active Directory pro Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak tooset až toowork služby Active Directory s Azure Remoteappem."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 66366b25-6012-45fa-a4f6-da0ddfe0b486
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: c1c4a7ad6fb96ec4d479fdc231f03d81b58b2b71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a><span data-ttu-id="47f7e-103">Azure AD + požadavky služby Active Directory pro Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="47f7e-103">Azure AD + Active Directory requirements for Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="47f7e-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="47f7e-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="47f7e-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="47f7e-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="47f7e-106">Hybridní kolekce Azure Remoteappu nebo cloudové kolekce, které chcete toofederate pomocí služby AD Connect potřebujete následující toodo hello.</span><span class="sxs-lookup"><span data-stu-id="47f7e-106">For your Azure RemoteApp hybrid collection or for a cloud collection that you want toofederate using AD Connect, you need toodo hello following.</span></span>

### <a name="connect-azure-ad-and-active-directory"></a><span data-ttu-id="47f7e-107">Azure AD Connect a služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="47f7e-107">Connect Azure AD and Active Directory</span></span>
<span data-ttu-id="47f7e-108">Pokud chcete tooconnect klientovi Azure AD a prostředí vaší místní služby Active Directory, použijte AD Connect.</span><span class="sxs-lookup"><span data-stu-id="47f7e-108">If you want tooconnect your Azure AD tenant and your on-premises Active Directory environments, use AD Connect.</span></span> <span data-ttu-id="47f7e-109">Můžete pouze bude trvat [4 kliknutí](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) tooconnect hello dva adresáře.</span><span class="sxs-lookup"><span data-stu-id="47f7e-109">It will take you only [4 clicks](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) tooconnect hello two directories.</span></span>

<span data-ttu-id="47f7e-110">Poznámka: synchronizace adresářů je vyžadován pro hybridní kolekce.</span><span class="sxs-lookup"><span data-stu-id="47f7e-110">Note - Directory synchronization is required for hybrid collections.</span></span>

### <a name="make-sure-your-domaincom-match"></a><span data-ttu-id="47f7e-111">Zajistěte, aby vaše "@domain.com" odpovídat</span><span class="sxs-lookup"><span data-stu-id="47f7e-111">Make sure your "@domain.com" match</span></span>
<span data-ttu-id="47f7e-112">Než začnete, ujistěte se, že hello UPN pro vaše místní doménové struktuře odpovídá hello příponu domény služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="47f7e-112">Before you get started, make sure that hello UPN for your on-premises forest matches hello suffix of your Azure AD domain.</span></span> 

<span data-ttu-id="47f7e-113">Po nastavení příponu UPN domény hello ve službě Azure AD, všechny uživatele přihlašující se do Azure RemoteApp se přihlaste se jako "uživatel @<hello suffix you set up>."</span><span class="sxs-lookup"><span data-stu-id="47f7e-113">After you set up hello UPN domain suffix in Azure AD, all users logging into Azure RemoteApp will log in as "user@<hello suffix you set up>."</span></span> <span data-ttu-id="47f7e-114">Ujistěte se, že uživatelé také přihlásit pomocí hello stejné user@suffix do hello místní domény.</span><span class="sxs-lookup"><span data-stu-id="47f7e-114">Make sure that users can also log in with hello same user@suffix into hello on-premises domain.</span></span> <span data-ttu-id="47f7e-115">V některých případech můžete nastavit až jeden název domény ve službě Azure AD při zadávání příponou jinou doménu pro hello uživatele v-místním prostředí</span><span class="sxs-lookup"><span data-stu-id="47f7e-115">In certain cases you can set up one domain name in Azure AD while specifying a different domain suffix for hello user on-prem.</span></span> <span data-ttu-id="47f7e-116">V takovém případě vaši uživatelé nebudou mít tooconnect tooany připojený k doméně počítače nebo prostředků pomocí Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="47f7e-116">In this case, your users won't be able tooconnect tooany domain-joined computers or resources through Azure RemoteApp.</span></span>

<span data-ttu-id="47f7e-117">Například pokud nastavíte vaší domény přípona UPN v AAD jako contoso.com, ale někteří uživatelé na místní nebo AD jsou nakonfigurované toolog s @contoso.uk, pak se tito uživatelé nebudou moct toocorrectly protokolu do hello ARA kolekce.</span><span class="sxs-lookup"><span data-stu-id="47f7e-117">For example, if you set up your UPN domain suffix in AAD as contoso.com, but some users on premises/AD are configured toolog in with @contoso.uk, then those users will not be able toocorrectly log into hello ARA collection.</span></span> <span data-ttu-id="47f7e-118">Uživatele, které musí být hlavní název uživatele v AAD a AD hello stejné pro hello přihlášení toobe možné"</span><span class="sxs-lookup"><span data-stu-id="47f7e-118">Users UPN in AAD and AD must be hello same for hello login toobe possible”</span></span>

### <a name="create-objects-for-azure-remoteapp"></a><span data-ttu-id="47f7e-119">Vytvoření objektů pro Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="47f7e-119">Create objects for Azure RemoteApp</span></span>
<span data-ttu-id="47f7e-120">Musíte taky toocreate hello následující objekty služby Active Directory v místě:</span><span class="sxs-lookup"><span data-stu-id="47f7e-120">You also need toocreate hello following on-premises Active Directory objects:</span></span>

* <span data-ttu-id="47f7e-121">Účtu tooprovide přístup toodomain prostředky služby u aplikací RemoteApp díky připojení ke službě RDSH koncové body toohello místní domény.</span><span class="sxs-lookup"><span data-stu-id="47f7e-121">A service account tooprovide access toodomain resources for RemoteApp programs by joining RDSH end points toohello on-premises domain.</span></span>
* <span data-ttu-id="47f7e-122">Organizační jednotce (OU) toocontain vzdálené aplikace RemoteApp objekty počítačů.</span><span class="sxs-lookup"><span data-stu-id="47f7e-122">An Organizational Unit (OU) toocontain RemoteApp machine objects.</span></span> <span data-ttu-id="47f7e-123">Použití hello organizační jednotky je doporučené (ale není požadováno) tooisolate hello účty a zásady, které budete používat s Remoteappem.</span><span class="sxs-lookup"><span data-stu-id="47f7e-123">Use of hello OU is recommended (but not required) tooisolate hello accounts and policies you will use with RemoteApp.</span></span>

<span data-ttu-id="47f7e-124">Obě tyto objekty musíte při vytváření kolekcí vzdálené aplikace RemoteApp, tak buďte zda toodo tyto kroky nejdřív.</span><span class="sxs-lookup"><span data-stu-id="47f7e-124">You need both of these objects when you create your RemoteApp collection, so be sure toodo these steps first.</span></span>

