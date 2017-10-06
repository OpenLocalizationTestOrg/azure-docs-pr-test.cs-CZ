---
title: "aaaAdding Azure Active Directory pomocí připojené služby v sadě Visual Studio | Microsoft Docs"
description: "Hello Visual Studio přidat připojení služby dialogovém okně Přidat služby Azure Active Directory"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.service: active-directory
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: kraigb
ms.openlocfilehash: 26c8f68edf9ec5f7bf65cbab34e4f9b4085ed18d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a><span data-ttu-id="1819e-103">Přidání Azure Active Directory pomocí připojené služby v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1819e-103">Adding an Azure Active Directory by using Connected Services in Visual Studio</span></span>
<span data-ttu-id="1819e-104">Pomocí služby Azure Active Directory (Azure AD), může podporovat jednotné přihlašování (SSO) pro webové aplikace ASP.NET MVC nebo ověřování Active Directory v webového rozhraní API služby.</span><span class="sxs-lookup"><span data-stu-id="1819e-104">By using Azure Active Directory (Azure AD), you can support Single Sign-On (SSO) for ASP.NET MVC web applications, or Active Directory Authentication in Web API services.</span></span> <span data-ttu-id="1819e-105">S Azure Active Directory Authentication mohou uživatelé používat své účty z Azure Active Directory tooconnect tooyour webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="1819e-105">With Azure Active Directory Authentication, your users can use their accounts from Azure Active Directory tooconnect tooyour web applications.</span></span> <span data-ttu-id="1819e-106">výhody Hello ověřování Azure Active Directory pomocí webového rozhraní API zahrnují rozšířené zabezpečení při vystavení rozhraní API z webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1819e-106">hello advantages of Azure Active Directory Authentication with Web API include enhanced data security when exposing an API from a web application.</span></span> <span data-ttu-id="1819e-107">S Azure AD není nutné toomanage samostatného ověření systému se správou svůj vlastní účet a uživatele.</span><span class="sxs-lookup"><span data-stu-id="1819e-107">With Azure AD, you do not have toomanage a separate authentication system with its own account and user management.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1819e-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1819e-108">Prerequisites</span></span>
- <span data-ttu-id="1819e-109">Účet Azure – Pokud nemáte účet Azure, můžete [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="1819e-109">Azure account - If you don't have an Azure account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

### <a name="connect-tooazure-active-directory-using-hello-connected-services-dialog"></a><span data-ttu-id="1819e-110">Připojit tooAzure hello připojení služby pomocí služby Active Directory dialogové okno</span><span class="sxs-lookup"><span data-stu-id="1819e-110">Connect tooAzure Active Directory using hello Connected Services dialog</span></span>
1. <span data-ttu-id="1819e-111">V sadě Visual Studio vytvoření nebo otevření projektu aplikace ASP.NET MVC nebo projekt webového rozhraní API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1819e-111">In Visual Studio, create or open an ASP.NET MVC project, or an ASP.NET Web API project.</span></span>

1. <span data-ttu-id="1819e-112">Z hello Průzkumníku řešení klikněte pravým tlačítkem na hello **připojené služby** uzel a v místní nabídce hello, vyberte **přidat připojení služby**.</span><span class="sxs-lookup"><span data-stu-id="1819e-112">From hello Solution Explorer, right-click hello **Connected Services** node, and, from hello context menu, select **Add Connected Services**.</span></span>

1. <span data-ttu-id="1819e-113">Na hello **připojené služby** vyberte **ověřování s Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1819e-113">On hello **Connected Services** page, select **Authentication with Azure Active Directory**.</span></span>
   
    ![Připojená stránka služby](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. <span data-ttu-id="1819e-115">Na hello **ÚVOD** stránku hello **konfigurovat Azure AD Authentication** průvodci vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="1819e-115">On hello **Introduction** page of hello **Configure Azure AD Authentication** wizard, select **Next**.</span></span>
   
    ![Úvodní stránka](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1. <span data-ttu-id="1819e-117">Na hello **jednotné přihlašování na** stránku hello **konfigurovat Azure AD Authentication** průvodce, vyberte doménu z hello **domény** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="1819e-117">On hello **Single-Sign On** page of hello **Configure Azure AD Authentication** wizard, select a domain from hello **Domain** drop-down list.</span></span> <span data-ttu-id="1819e-118">Hello seznam domén obsahuje všechny domény, které jsou přístupné hello účty uvedené v dialogovém okně Nastavení účtu hello.</span><span class="sxs-lookup"><span data-stu-id="1819e-118">hello list of domains contains all domains accessible by hello accounts listed in hello Account Settings dialog.</span></span> <span data-ttu-id="1819e-119">Jako alternativu, můžete zadat název domény, Pokud nenajdete hello jeden hledáte, jako například `mydomain.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="1819e-119">As an alternative, you can enter a domain name if you don’t find hello one you’re looking for, such as `mydomain.onmicrosoft.com`.</span></span> <span data-ttu-id="1819e-120">Můžete vybrat možnost toocreate hello aplikaci Azure Active Directory nebo pomocí nastavení hello z existující aplikaci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1819e-120">You can choose hello option toocreate an Azure Active Directory app or use hello settings from an existing Azure Active Directory app.</span></span> <span data-ttu-id="1819e-121">Vyberte **Další** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="1819e-121">Select **Next** when done.</span></span>
   
    ![Jednotné přihlašování na stránce](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)

1. <span data-ttu-id="1819e-123">Na hello **přístup k adresářové** stránku hello **konfigurovat Azure AD Authentication** průvodce, ujistěte se, že hello **čtení dat adresáře** zaškrtnutá možnost.</span><span class="sxs-lookup"><span data-stu-id="1819e-123">On hello **Directory Access** page of hello **Configure Azure AD Authentication** wizard, ensure that hello **Read directory data** option is checked.</span></span> 
   
    ![Stránka adresáře](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. <span data-ttu-id="1819e-125">Vyberte **Dokončit** tooadd hello tooenable nezbytné konfigurace kódu a odkazy na projekt pro ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1819e-125">Select **Finish** tooadd hello necessary configuration code and references tooenable your project for Azure AD authentication.</span></span> <span data-ttu-id="1819e-126">Uvidíte hello domény služby Active Directory na hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="1819e-126">You can see hello Active Directory domain on hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="1819e-127">Visual Studio se zobrazí [co se stalo](#how-your-project-is-modified) tooshow článku jste jak byla změněna projektu.</span><span class="sxs-lookup"><span data-stu-id="1819e-127">Visual Studio will display a [What Happened](#how-your-project-is-modified) article tooshow you how your project was modified.</span></span> <span data-ttu-id="1819e-128">Pokud chcete toocheck, který všechno fungoval, otevřete ho hello upravit konfigurační soubory a ověřte, zda text hello nastavení uvedené v článku hello existuje.</span><span class="sxs-lookup"><span data-stu-id="1819e-128">If you want toocheck that everything worked, open one of hello modified configuration files and verify that hello settings mentioned in hello article are there.</span></span> 

## <a name="how-your-project-is-modified"></a><span data-ttu-id="1819e-129">Jak se mění projektu</span><span class="sxs-lookup"><span data-stu-id="1819e-129">How your project is modified</span></span>
<span data-ttu-id="1819e-130">Když spustíte Průvodce hello, Visual Studio přidá Azure Active Directory a přidružené odkazy tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="1819e-130">When you run hello wizard, Visual Studio adds Azure Active Directory and associated references tooyour project.</span></span> <span data-ttu-id="1819e-131">Konfigurační soubory a soubory kódu v projektu jsou také upravené tooadd podpory pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1819e-131">Configuration files and code files in your project are also modified tooadd support for Azure AD.</span></span> <span data-ttu-id="1819e-132">Hello konkrétní změny, které sada Visual Studio provádí závisí na typu projektu hello.</span><span class="sxs-lookup"><span data-stu-id="1819e-132">hello specific modifications that Visual Studio makes depend on hello project type.</span></span> <span data-ttu-id="1819e-133">Podrobné informace o tom, jak jsou upraveny projekty ASP.NET MVC najdete v tématu [jaké projekty MVC happened –](http://go.microsoft.com/fwlink/p/?LinkID=513809).</span><span class="sxs-lookup"><span data-stu-id="1819e-133">For detailed information about how ASP.NET MVC projects are modified, see [What happened– MVC Projects](http://go.microsoft.com/fwlink/p/?LinkID=513809).</span></span> <span data-ttu-id="1819e-134">Projekty webového rozhraní API, najdete v části [co se stalo – projekty webového rozhraní API](http://go.microsoft.com/fwlink/p/?LinkId=513810).</span><span class="sxs-lookup"><span data-stu-id="1819e-134">For Web API projects, see [What happened – Web API Projects](http://go.microsoft.com/fwlink/p/?LinkId=513810).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1819e-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1819e-135">Next steps</span></span>
* [<span data-ttu-id="1819e-136">Fórum MSDN pro Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1819e-136">MSDN Forum for Azure Active Directory</span></span>](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)
* [<span data-ttu-id="1819e-137">Dokumentaci ke službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1819e-137">Azure Active Directory Documentation</span></span>](https://azure.microsoft.com/documentation/services/active-directory/)
* [<span data-ttu-id="1819e-138">Příspěvek blogu: Úvod tooAzure služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="1819e-138">Blog Post: Intro tooAzure Active Directory</span></span>](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

