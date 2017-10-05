---
title: "Přidání Azure Active Directory pomocí připojené služby v sadě Visual Studio | Microsoft Docs"
description: "Přidat Azure Active Directory pomocí dialogu Visual Studio přidat připojení služby"
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
ms.openlocfilehash: a767c93fb271f3aa33d9556c69c511bcac7cb0d5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a><span data-ttu-id="07ae4-103">Přidání Azure Active Directory pomocí připojené služby v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="07ae4-103">Adding an Azure Active Directory by using Connected Services in Visual Studio</span></span>
<span data-ttu-id="07ae4-104">Pomocí služby Azure Active Directory (Azure AD), může podporovat jednotné přihlašování (SSO) pro webové aplikace ASP.NET MVC nebo ověřování Active Directory v webového rozhraní API služby.</span><span class="sxs-lookup"><span data-stu-id="07ae4-104">By using Azure Active Directory (Azure AD), you can support Single Sign-On (SSO) for ASP.NET MVC web applications, or Active Directory Authentication in Web API services.</span></span> <span data-ttu-id="07ae4-105">S Azure Active Directory Authentication uživatelům jejich účty ze služby Azure Active Directory použít k připojení k webovým aplikacím.</span><span class="sxs-lookup"><span data-stu-id="07ae4-105">With Azure Active Directory Authentication, your users can use their accounts from Azure Active Directory to connect to your web applications.</span></span> <span data-ttu-id="07ae4-106">Výhody ověřování Azure Active Directory pomocí webového rozhraní API obsahují rozšířené zabezpečení při vystavení rozhraní API z webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="07ae4-106">The advantages of Azure Active Directory Authentication with Web API include enhanced data security when exposing an API from a web application.</span></span> <span data-ttu-id="07ae4-107">S Azure AD není nutné spravovat samostatného ověření systému se správou svůj vlastní účet a uživatele.</span><span class="sxs-lookup"><span data-stu-id="07ae4-107">With Azure AD, you do not have to manage a separate authentication system with its own account and user management.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07ae4-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="07ae4-108">Prerequisites</span></span>
- <span data-ttu-id="07ae4-109">Účet Azure – Pokud nemáte účet Azure, můžete [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="07ae4-109">Azure account - If you don't have an Azure account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

### <a name="connect-to-azure-active-directory-using-the-connected-services-dialog"></a><span data-ttu-id="07ae4-110">Připojení k Azure Active Directory v dialogovém okně připojení služby</span><span class="sxs-lookup"><span data-stu-id="07ae4-110">Connect to Azure Active Directory using the Connected Services dialog</span></span>
1. <span data-ttu-id="07ae4-111">V sadě Visual Studio vytvoření nebo otevření projektu aplikace ASP.NET MVC nebo projekt webového rozhraní API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="07ae4-111">In Visual Studio, create or open an ASP.NET MVC project, or an ASP.NET Web API project.</span></span>

1. <span data-ttu-id="07ae4-112">V Průzkumníku řešení klikněte pravým tlačítkem **připojené služby** uzel a v místní nabídce vyberte **přidat připojení služby**.</span><span class="sxs-lookup"><span data-stu-id="07ae4-112">From the Solution Explorer, right-click the **Connected Services** node, and, from the context menu, select **Add Connected Services**.</span></span>

1. <span data-ttu-id="07ae4-113">Na **připojené služby** vyberte **ověřování s Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="07ae4-113">On the **Connected Services** page, select **Authentication with Azure Active Directory**.</span></span>
   
    ![Připojená stránka služby](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. <span data-ttu-id="07ae4-115">Na **ÚVOD** stránky **konfigurovat Azure AD Authentication** průvodci vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="07ae4-115">On the **Introduction** page of the **Configure Azure AD Authentication** wizard, select **Next**.</span></span>
   
    ![Úvodní stránka](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1. <span data-ttu-id="07ae4-117">Na **jednotné přihlašování na** stránky **konfigurovat Azure AD Authentication** průvodce, vyberte doménu z **domény** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="07ae4-117">On the **Single-Sign On** page of the **Configure Azure AD Authentication** wizard, select a domain from the **Domain** drop-down list.</span></span> <span data-ttu-id="07ae4-118">Seznam domén obsahuje všechny domény, které jsou dostupné účty uvedené v dialogovém okně Nastavení účtu.</span><span class="sxs-lookup"><span data-stu-id="07ae4-118">The list of domains contains all domains accessible by the accounts listed in the Account Settings dialog.</span></span> <span data-ttu-id="07ae4-119">Jako alternativu, můžete zadat název domény, pokud zde nejsou tu, kterou hledáte, jako například `mydomain.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="07ae4-119">As an alternative, you can enter a domain name if you don’t find the one you’re looking for, such as `mydomain.onmicrosoft.com`.</span></span> <span data-ttu-id="07ae4-120">Můžete vytvořit aplikaci Azure Active Directory nebo používat nastavení z existující aplikaci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="07ae4-120">You can choose the option to create an Azure Active Directory app or use the settings from an existing Azure Active Directory app.</span></span> <span data-ttu-id="07ae4-121">Vyberte **Další** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="07ae4-121">Select **Next** when done.</span></span>
   
    ![Jednotné přihlašování na stránce](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)

1. <span data-ttu-id="07ae4-123">Na **přístup k adresářové** stránky **konfigurovat Azure AD Authentication** průvodce, ujistěte se, že **čtení dat adresáře** zaškrtnutá možnost.</span><span class="sxs-lookup"><span data-stu-id="07ae4-123">On the **Directory Access** page of the **Configure Azure AD Authentication** wizard, ensure that the **Read directory data** option is checked.</span></span> 
   
    ![Stránka adresáře](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. <span data-ttu-id="07ae4-125">Vyberte **Dokončit** přidejte kód nezbytné konfigurace a odkazy na povolit svůj projekt pro ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="07ae4-125">Select **Finish** to add the necessary configuration code and references to enable your project for Azure AD authentication.</span></span> <span data-ttu-id="07ae4-126">Domény služby Active Directory můžete zobrazit na [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="07ae4-126">You can see the Active Directory domain on the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="07ae4-127">Visual Studio se zobrazí [co se stalo](#how-your-project-is-modified) článku ukazují, jak byla změněna projektu.</span><span class="sxs-lookup"><span data-stu-id="07ae4-127">Visual Studio will display a [What Happened](#how-your-project-is-modified) article to show you how your project was modified.</span></span> <span data-ttu-id="07ae4-128">Pokud chcete zkontrolovat, zda vše funguje, otevřete ho upravený konfigurační soubory a ověřte, že nastavení uvedených v článku existují.</span><span class="sxs-lookup"><span data-stu-id="07ae4-128">If you want to check that everything worked, open one of the modified configuration files and verify that the settings mentioned in the article are there.</span></span> 

## <a name="how-your-project-is-modified"></a><span data-ttu-id="07ae4-129">Jak se mění projektu</span><span class="sxs-lookup"><span data-stu-id="07ae4-129">How your project is modified</span></span>
<span data-ttu-id="07ae4-130">Když spustíte průvodce, Visual Studio přidá Azure Active Directory a přidružené odkazy na projekt.</span><span class="sxs-lookup"><span data-stu-id="07ae4-130">When you run the wizard, Visual Studio adds Azure Active Directory and associated references to your project.</span></span> <span data-ttu-id="07ae4-131">Přidání podpory pro Azure AD jsou také upravit konfigurační soubory a soubory kódu v projektu.</span><span class="sxs-lookup"><span data-stu-id="07ae4-131">Configuration files and code files in your project are also modified to add support for Azure AD.</span></span> <span data-ttu-id="07ae4-132">Určité změny, které sada Visual Studio provádí závisí na typu projektu.</span><span class="sxs-lookup"><span data-stu-id="07ae4-132">The specific modifications that Visual Studio makes depend on the project type.</span></span> <span data-ttu-id="07ae4-133">Podrobné informace o tom, jak jsou upraveny projekty ASP.NET MVC najdete v tématu [jaké projekty MVC happened –](http://go.microsoft.com/fwlink/p/?LinkID=513809).</span><span class="sxs-lookup"><span data-stu-id="07ae4-133">For detailed information about how ASP.NET MVC projects are modified, see [What happened– MVC Projects](http://go.microsoft.com/fwlink/p/?LinkID=513809).</span></span> <span data-ttu-id="07ae4-134">Projekty webového rozhraní API, najdete v části [co se stalo – projekty webového rozhraní API](http://go.microsoft.com/fwlink/p/?LinkId=513810).</span><span class="sxs-lookup"><span data-stu-id="07ae4-134">For Web API projects, see [What happened – Web API Projects](http://go.microsoft.com/fwlink/p/?LinkId=513810).</span></span>

## <a name="next-steps"></a><span data-ttu-id="07ae4-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="07ae4-135">Next steps</span></span>
* [<span data-ttu-id="07ae4-136">Fórum MSDN pro Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="07ae4-136">MSDN Forum for Azure Active Directory</span></span>](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)
* [<span data-ttu-id="07ae4-137">Dokumentaci ke službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="07ae4-137">Azure Active Directory Documentation</span></span>](https://azure.microsoft.com/documentation/services/active-directory/)
* [<span data-ttu-id="07ae4-138">Příspěvek blogu: Úvod do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="07ae4-138">Blog Post: Intro to Azure Active Directory</span></span>](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

