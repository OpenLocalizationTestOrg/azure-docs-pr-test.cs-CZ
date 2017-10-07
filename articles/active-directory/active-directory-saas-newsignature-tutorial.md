---
title: "Kurz: Integrace Azure Active Directory pomocí portálu správy cloudu Microsoft Azure | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a portálu pro správu cloudu Microsoft Azure."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4ea9f47c-25ca-42b0-a878-9e7aa6f34973
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 9596826e3dc1289b95009bf01ec8b86f823ef345
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cloud-management-portal-for-microsoft-azure"></a><span data-ttu-id="6fe7f-103">Kurz: Integrace Azure Active Directory pomocí portálu správy cloudu Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="6fe7f-103">Tutorial: Azure Active Directory integration with Cloud Management Portal for Microsoft Azure</span></span>

<span data-ttu-id="6fe7f-104">V tomto kurzu zjistíte, jak toointegrate portál pro správu cloudu pro Microsoft Azure s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6fe7f-104">In this tutorial, you learn how toointegrate Cloud Management Portal for Microsoft Azure with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6fe7f-105">Integrace portálu pro správu cloudu Microsoft Azure s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6fe7f-105">Integrating Cloud Management Portal for Microsoft Azure with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6fe7f-106">Můžete řídit ve službě Azure AD, který má přístup tooCloud portál pro správu pro Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="6fe7f-106">You can control in Azure AD who has access tooCloud Management Portal for Microsoft Azure</span></span>
- <span data-ttu-id="6fe7f-107">Vaši uživatelé tooautomatically get přihlášeného tooCloud portál pro správu pro Microsoft Azure (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="6fe7f-107">You can enable your users tooautomatically get signed-on tooCloud Management Portal for Microsoft Azure (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6fe7f-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6fe7f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6fe7f-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6fe7f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6fe7f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6fe7f-110">Prerequisites</span></span>

<span data-ttu-id="6fe7f-111">tooconfigure integrace Azure AD pomocí portálu správy cloudu Microsoft Azure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6fe7f-111">tooconfigure Azure AD integration with Cloud Management Portal for Microsoft Azure, you need hello following items:</span></span>

- <span data-ttu-id="6fe7f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6fe7f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6fe7f-113">Portál pro správu cloudu pro Microsoft Azure jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6fe7f-113">A Cloud Management Portal for Microsoft Azure single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6fe7f-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6fe7f-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6fe7f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6fe7f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6fe7f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6fe7f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6fe7f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6fe7f-118">Scenario description</span></span>
<span data-ttu-id="6fe7f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6fe7f-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6fe7f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6fe7f-121">Přidání portálu pro správu cloudu Microsoft Azure z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6fe7f-121">Adding Cloud Management Portal for Microsoft Azure from hello gallery</span></span>
2. <span data-ttu-id="6fe7f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6fe7f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cloud-management-portal-for-microsoft-azure-from-hello-gallery"></a><span data-ttu-id="6fe7f-123">Přidání portálu pro správu cloudu Microsoft Azure z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6fe7f-123">Adding Cloud Management Portal for Microsoft Azure from hello gallery</span></span>
<span data-ttu-id="6fe7f-124">tooconfigure hello integrace portálu pro správu cloudu Microsoft Azure do Azure AD, musíte pro Microsoft Azure z hello Galerie tooyour seznam spravovaných aplikací SaaS tooadd portálu pro správu cloudu.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-124">tooconfigure hello integration of Cloud Management Portal for Microsoft Azure into Azure AD, you need tooadd Cloud Management Portal for Microsoft Azure from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6fe7f-125">**tooadd portál pro správu cloudu pro Microsoft Azure z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6fe7f-125">**tooadd Cloud Management Portal for Microsoft Azure from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6fe7f-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6fe7f-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6fe7f-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="6fe7f-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="6fe7f-133">Hello vyhledávacího pole zadejte **portál pro správu cloudu pro Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-133">In hello search box, type **Cloud Management Portal for Microsoft Azure**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_search.png)

5. <span data-ttu-id="6fe7f-135">Na panelu výsledků hello vyberte **portál pro správu cloudu pro Microsoft Azure**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-135">In hello results panel, select **Cloud Management Portal for Microsoft Azure**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6fe7f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6fe7f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6fe7f-138">V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí portálu správy cloudu Microsoft Azure na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="6fe7f-138">In this section, you configure and test Azure AD single sign-on with Cloud Management Portal for Microsoft Azure based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6fe7f-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele na portálu pro správu cloudu Microsoft Azure je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Cloud Management Portal for Microsoft Azure is tooa user in Azure AD.</span></span> <span data-ttu-id="6fe7f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello na portálu pro správu cloudu Microsoft Azure musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-140">In other words, a link relationship between an Azure AD user and hello related user in Cloud Management Portal for Microsoft Azure needs toobe established.</span></span>

<span data-ttu-id="6fe7f-141">V portálu pro správu cloudu Microsoft Azure, přiřaďte hello hodnotu hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-141">In Cloud Management Portal for Microsoft Azure, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6fe7f-142">tooconfigure a testování Azure AD jednotné přihlašování pomocí portálu správy cloudu Microsoft Azure, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="6fe7f-142">tooconfigure and test Azure AD single sign-on with Cloud Management Portal for Microsoft Azure, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6fe7f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6fe7f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6fe7f-145">**[Vytváření portál pro správu cloudu pro Microsoft Azure testovacího uživatele](#creating-a-cloud-management-portal-for-microsoft-azure-test-user)**  -toohave protějšek Britta Simon na portálu pro správu cloudu Microsoft Azure, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-145">**[Creating a Cloud Management Portal for Microsoft Azure test user](#creating-a-cloud-management-portal-for-microsoft-azure-test-user)** - toohave a counterpart of Britta Simon in Cloud Management Portal for Microsoft Azure that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6fe7f-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6fe7f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6fe7f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6fe7f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6fe7f-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v portálu pro správu cloudu pro aplikaci Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Cloud Management Portal for Microsoft Azure application.</span></span>

<span data-ttu-id="6fe7f-150">**tooconfigure Azure AD jednotné přihlašování pomocí portálu správy cloudu Microsoft Azure, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6fe7f-150">**tooconfigure Azure AD single sign-on with Cloud Management Portal for Microsoft Azure, perform hello following steps:**</span></span>

1. <span data-ttu-id="6fe7f-151">V portálu Azure, na hello hello **portál pro správu cloudu pro Microsoft Azure** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-151">In hello Azure portal, on hello **Cloud Management Portal for Microsoft Azure** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="6fe7f-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_samlbase.png)

3. <span data-ttu-id="6fe7f-155">Na hello **domény Microsoft Azure a adresy URL portálu pro správu cloudu** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6fe7f-155">On hello **Cloud Management Portal for Microsoft Azure Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_url.png)

    <span data-ttu-id="6fe7f-157">a.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-157">a.</span></span> <span data-ttu-id="6fe7f-158">V hello **přihlašovací adresa URL** textové pole, zadejte adresu URL pomocí hello následující vzory:</span><span class="sxs-lookup"><span data-stu-id="6fe7f-158">In hello **Sign-on URL** textbox, type a URL using hello following patterns:</span></span> 
    
    | |
    |--|
    | `https://portal.newsignature.com/<instancename>` |   
    | `https://portal.igcm.com/<instancename>` |
    
    <span data-ttu-id="6fe7f-159">b.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-159">b.</span></span> <span data-ttu-id="6fe7f-160">V hello **identifikátor** textové pole, zadejte adresu URL pomocí hello následující vzory:</span><span class="sxs-lookup"><span data-stu-id="6fe7f-160">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span> 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com` |
    | `https://<subdomain>.newsignature.com` |

    <span data-ttu-id="6fe7f-161">c.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-161">c.</span></span> <span data-ttu-id="6fe7f-162">V hello **adresa URL odpovědi** textové pole, zadejte adresu URL pomocí hello následující vzory:</span><span class="sxs-lookup"><span data-stu-id="6fe7f-162">In hello **Reply URL** textbox, type a URL using hello following patterns:</span></span> 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com/<instancename>` |
    | `https://<subdomain>.newsignature.com` |
    | `https://<subdomain>.newsignature.com/<instancename>` |

    > [!NOTE] 
    > <span data-ttu-id="6fe7f-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-163">These values are not real.</span></span> <span data-ttu-id="6fe7f-164">Tyto hodnoty aktualizujte pomocí hello skutečná adresa URL přihlašování, identifikátor a odpovědi adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-164">Update these values with hello actual Sign-On URL, Identifier and Reply URL.</span></span> <span data-ttu-id="6fe7f-165">Obraťte se na [portál pro správu cloudu pro klienta Microsoft Azure tým podpory](mailto:jczernuszka@newsignature.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-165">Contact [Cloud Management Portal for Microsoft Azure Client support team](mailto:jczernuszka@newsignature.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="6fe7f-166">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-166">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_certificate.png) 

5. <span data-ttu-id="6fe7f-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6fe7f-170">Na hello **portál pro správu cloudu pro Microsoft Azure Configuration** klikněte na tlačítko **nakonfigurovat portál správy cloudu pro Microsoft Azure** tooopen **konfigurovat přihlašování**okno.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-170">On hello **Cloud Management Portal for Microsoft Azure Configuration** section, click **Configure Cloud Management Portal for Microsoft Azure** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6fe7f-171">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="6fe7f-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_configure.png) 

7. <span data-ttu-id="6fe7f-173">tooconfigure jednotného přihlašování na **portál pro správu cloudu pro Microsoft Azure** straně, je nutné stáhnout hello toosend **certifikát**, **Sign-Out URL**, **SAML jeden přihlašování adresa URL služby** a **SAML Entity ID** příliš[portál pro správu cloudu pro tým podpory Microsoft Azure](mailto:jczernuszka@newsignature.com).</span><span class="sxs-lookup"><span data-stu-id="6fe7f-173">tooconfigure single sign-on on **Cloud Management Portal for Microsoft Azure** side, you need toosend hello downloaded **Certificate**, **Sign-Out URL**, **SAML Single Sign-On Service URL** and **SAML Entity ID** too[Cloud Management Portal for Microsoft Azure support team](mailto:jczernuszka@newsignature.com).</span></span> <span data-ttu-id="6fe7f-174">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-174">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="6fe7f-175">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="6fe7f-175">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6fe7f-176">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-176">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6fe7f-177">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6fe7f-177">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6fe7f-178">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6fe7f-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="6fe7f-179">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-179">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="6fe7f-181">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6fe7f-181">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6fe7f-182">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-182">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6fe7f-184">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-184">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6fe7f-186">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-186">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6fe7f-188">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6fe7f-188">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6fe7f-190">a.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-190">a.</span></span> <span data-ttu-id="6fe7f-191">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-191">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6fe7f-192">b.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-192">b.</span></span> <span data-ttu-id="6fe7f-193">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-193">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6fe7f-194">c.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-194">c.</span></span> <span data-ttu-id="6fe7f-195">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-195">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6fe7f-196">d.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-196">d.</span></span> <span data-ttu-id="6fe7f-197">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-197">Click **Create**.</span></span>
 
### <a name="creating-a-cloud-management-portal-for-microsoft-azure-test-user"></a><span data-ttu-id="6fe7f-198">Vytváření portál pro správu cloudu pro Microsoft Azure testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="6fe7f-198">Creating a Cloud Management Portal for Microsoft Azure test user</span></span>

<span data-ttu-id="6fe7f-199">Hello cílem této části je toocreate uživatele volat Britta Simon na portálu pro správu cloudu pro Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-199">hello objective of this section is toocreate a user called Britta Simon in Cloud Management Portal for Microsoft Azure.</span></span> <span data-ttu-id="6fe7f-200">Spojte se s [portál pro správu cloudu pro tým podpory Microsoft Azure](mailto:jczernuszka@newsignature.com) tooadd hello uživatele v hello portálu pro správu cloudu pro účet Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-200">Please work with [Cloud Management Portal for Microsoft Azure support team](mailto:jczernuszka@newsignature.com) tooadd hello users in hello Cloud Management Portal for Microsoft Azure account.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6fe7f-201">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="6fe7f-201">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6fe7f-202">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooCloud přístup k portálu pro správu Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCloud Management Portal for Microsoft Azure.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="6fe7f-204">**tooassign tooCloud Britta Simon portál pro správu pro Microsoft Azure, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6fe7f-204">**tooassign Britta Simon tooCloud Management Portal for Microsoft Azure, perform hello following steps:**</span></span>

1. <span data-ttu-id="6fe7f-205">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6fe7f-207">V seznamu aplikace hello vyberte **portál pro správu cloudu pro Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-207">In hello applications list, select **Cloud Management Portal for Microsoft Azure**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_app.png) 

3. <span data-ttu-id="6fe7f-209">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="6fe7f-211">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-211">Click **Add** button.</span></span> <span data-ttu-id="6fe7f-212">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="6fe7f-214">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6fe7f-215">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6fe7f-216">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6fe7f-217">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6fe7f-217">Testing single sign-on</span></span>

<span data-ttu-id="6fe7f-218">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-218">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>
<span data-ttu-id="6fe7f-219">Po kliknutí na tlačítko hello portálu pro správu cloudu pro dlaždice Microsoft Azure v hello přístupového panelu, měli byste obdržet tooyour automaticky přihlášeného portálu pro správu cloudu pro aplikaci Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="6fe7f-219">When you click hello Cloud Management Portal for Microsoft Azure tile in hello Access Panel, you should get automatically signed-on tooyour Cloud Management Portal for Microsoft Azure application.</span></span>

<span data-ttu-id="6fe7f-220">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6fe7f-220">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6fe7f-221">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6fe7f-221">Additional resources</span></span>

* [<span data-ttu-id="6fe7f-222">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6fe7f-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6fe7f-223">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6fe7f-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_203.png

