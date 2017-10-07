---
title: 'Kurz: Azure Active Directory integrace s PerformanceCentre | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a PerformanceCentre."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 65288c32-f7e6-4eb3-a6dc-523c3d748d1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 19781c0087093a67c70dc90072cf1a119bb2ade0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-performancecentre"></a><span data-ttu-id="e83aa-103">Kurz: Azure Active Directory integrace s PerformanceCentre</span><span class="sxs-lookup"><span data-stu-id="e83aa-103">Tutorial: Azure Active Directory integration with PerformanceCentre</span></span>

<span data-ttu-id="e83aa-104">V tomto kurzu zjistíte, jak toointegrate PerformanceCentre s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e83aa-104">In this tutorial, you learn how toointegrate PerformanceCentre with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e83aa-105">Integrace PerformanceCentre s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e83aa-105">Integrating PerformanceCentre with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e83aa-106">Můžete řídit ve službě Azure AD, který má přístup tooPerformanceCentre</span><span class="sxs-lookup"><span data-stu-id="e83aa-106">You can control in Azure AD who has access tooPerformanceCentre</span></span>
- <span data-ttu-id="e83aa-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPerformanceCentre (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e83aa-107">You can enable your users tooautomatically get signed-on tooPerformanceCentre (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e83aa-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e83aa-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e83aa-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e83aa-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e83aa-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e83aa-110">Prerequisites</span></span>

<span data-ttu-id="e83aa-111">Integrace služby Azure AD s PerformanceCentre tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="e83aa-111">tooconfigure Azure AD integration with PerformanceCentre, you need hello following items:</span></span>

- <span data-ttu-id="e83aa-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e83aa-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e83aa-113">PerformanceCentre jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e83aa-113">A PerformanceCentre single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e83aa-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e83aa-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e83aa-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e83aa-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e83aa-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e83aa-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e83aa-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e83aa-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e83aa-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e83aa-118">Scenario description</span></span>
<span data-ttu-id="e83aa-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e83aa-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e83aa-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e83aa-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e83aa-121">Přidání PerformanceCentre z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e83aa-121">Adding PerformanceCentre from hello gallery</span></span>
2. <span data-ttu-id="e83aa-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e83aa-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-performancecentre-from-hello-gallery"></a><span data-ttu-id="e83aa-123">Přidání PerformanceCentre z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e83aa-123">Adding PerformanceCentre from hello gallery</span></span>
<span data-ttu-id="e83aa-124">tooconfigure hello integrace PerformanceCentre do Azure AD, je nutné tooadd PerformanceCentre hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e83aa-124">tooconfigure hello integration of PerformanceCentre into Azure AD, you need tooadd PerformanceCentre from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e83aa-125">**tooadd PerformanceCentre z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e83aa-125">**tooadd PerformanceCentre from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e83aa-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e83aa-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e83aa-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e83aa-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e83aa-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e83aa-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e83aa-133">Hello vyhledávacího pole zadejte **PerformanceCentre**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-133">In hello search box, type **PerformanceCentre**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_search.png)

5. <span data-ttu-id="e83aa-135">Na panelu výsledků hello vyberte **PerformanceCentre**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="e83aa-135">In hello results panel, select **PerformanceCentre**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e83aa-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e83aa-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e83aa-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s PerformanceCentre podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="e83aa-138">In this section, you configure and test Azure AD single sign-on with PerformanceCentre based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e83aa-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v PerformanceCentre je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e83aa-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in PerformanceCentre is tooa user in Azure AD.</span></span> <span data-ttu-id="e83aa-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v PerformanceCentre musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="e83aa-140">In other words, a link relationship between an Azure AD user and hello related user in PerformanceCentre needs toobe established.</span></span>

<span data-ttu-id="e83aa-141">V PerformanceCentre, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="e83aa-141">In PerformanceCentre, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e83aa-142">tooconfigure a testu Azure AD jednotné přihlašování s PerformanceCentre, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="e83aa-142">tooconfigure and test Azure AD single sign-on with PerformanceCentre, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e83aa-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="e83aa-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e83aa-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e83aa-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e83aa-145">**[Vytvoření zkušebního uživatele PerformanceCentre](#creating-a-performancecentre-test-user)**  -toohave protějšek Britta Simon v PerformanceCentre, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="e83aa-145">**[Creating a PerformanceCentre test user](#creating-a-performancecentre-test-user)** - toohave a counterpart of Britta Simon in PerformanceCentre that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e83aa-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e83aa-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e83aa-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="e83aa-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e83aa-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e83aa-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e83aa-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci PerformanceCentre.</span><span class="sxs-lookup"><span data-stu-id="e83aa-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your PerformanceCentre application.</span></span>

<span data-ttu-id="e83aa-150">**tooconfigure Azure AD jednotné přihlašování s PerformanceCentre, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e83aa-150">**tooconfigure Azure AD single sign-on with PerformanceCentre, perform hello following steps:**</span></span>

1. <span data-ttu-id="e83aa-151">V portálu Azure, na hello hello **PerformanceCentre** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-151">In hello Azure portal, on hello **PerformanceCentre** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e83aa-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e83aa-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_samlbase.png)

3. <span data-ttu-id="e83aa-155">Na hello **PerformanceCentre domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e83aa-155">On hello **PerformanceCentre Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_url.png)

    <span data-ttu-id="e83aa-157">a.</span><span class="sxs-lookup"><span data-stu-id="e83aa-157">a.</span></span> <span data-ttu-id="e83aa-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`http://companyname.performancecentre.com/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="e83aa-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `http://companyname.performancecentre.com/saml/SSO`</span></span>

    <span data-ttu-id="e83aa-159">b.</span><span class="sxs-lookup"><span data-stu-id="e83aa-159">b.</span></span> <span data-ttu-id="e83aa-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`http://companyname.performancecentre.com`</span><span class="sxs-lookup"><span data-stu-id="e83aa-160">In hello **Identifier** textbox, type a URL using hello following pattern: `http://companyname.performancecentre.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e83aa-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="e83aa-161">These values are not real.</span></span> <span data-ttu-id="e83aa-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="e83aa-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e83aa-163">Obraťte se na [tým podpory PerformanceCentre klienta](https://www.performancecentre.com/contact-us/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e83aa-163">Contact [PerformanceCentre Client support team](https://www.performancecentre.com/contact-us/) tooget these values.</span></span> 

4. <span data-ttu-id="e83aa-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e83aa-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_certificate.png) 

5. <span data-ttu-id="e83aa-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e83aa-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-performancecentre-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e83aa-168">Na hello **PerformanceCentre konfigurace** klikněte na tlačítko **konfigurace PerformanceCentre** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="e83aa-168">On hello **PerformanceCentre Configuration** section, click **Configure PerformanceCentre** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e83aa-169">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="e83aa-169">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_configure.png) 

7. <span data-ttu-id="e83aa-171">Přihlášení tooyour **PerformanceCentre** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="e83aa-171">Sign-on tooyour **PerformanceCentre** company site as administrator.</span></span>

8. <span data-ttu-id="e83aa-172">Na kartě hello na levé straně hello, klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-172">In hello tab on hello left side, click **Configure**.</span></span>
   
    ![Azure AD jednotné přihlášení][10]

9. <span data-ttu-id="e83aa-174">Na kartě hello na levé straně hello, klikněte na tlačítko **různé**a potom klikněte na **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-174">In hello tab on hello left side, click **Miscellaneous**, and then click **Single Sign On**.</span></span>
   
    ![Azure AD jednotné přihlášení][11]

10. <span data-ttu-id="e83aa-176">Jako **protokol**, vyberte **SAML**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-176">As **Protocol**, select **SAML**.</span></span>
   
    ![Azure AD jednotné přihlášení][12]

11. <span data-ttu-id="e83aa-178">Otevřete váš soubor stažený metadata v poznámkovém bloku kopie hello obsahu, vložte jej do hello **metadat zprostředkovatelů Identity** textovému poli a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-178">Open your downloaded metadata file in notepad, copy hello content, paste it into hello **Identity Provider Metadata** textbox, and then click **Save**.</span></span>
   
    ![Azure AD jednotné přihlášení][13]

12. <span data-ttu-id="e83aa-180">Ověřte, zda text hello hello hodnoty pro **Entity základní adresa URL** a **Entity ID URL** jsou správné.</span><span class="sxs-lookup"><span data-stu-id="e83aa-180">Verify that hello values for hello **Entity Base URL** and **Entity ID URL** are correct.</span></span>
    
     ![Azure AD jednotné přihlášení][14]

> [!TIP]
> <span data-ttu-id="e83aa-182">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="e83aa-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e83aa-183">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="e83aa-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e83aa-184">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e83aa-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e83aa-185">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e83aa-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="e83aa-186">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="e83aa-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e83aa-188">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e83aa-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e83aa-189">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e83aa-189">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e83aa-191">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-191">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e83aa-193">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="e83aa-193">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e83aa-195">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e83aa-195">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e83aa-197">a.</span><span class="sxs-lookup"><span data-stu-id="e83aa-197">a.</span></span> <span data-ttu-id="e83aa-198">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-198">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e83aa-199">b.</span><span class="sxs-lookup"><span data-stu-id="e83aa-199">b.</span></span> <span data-ttu-id="e83aa-200">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e83aa-200">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e83aa-201">c.</span><span class="sxs-lookup"><span data-stu-id="e83aa-201">c.</span></span> <span data-ttu-id="e83aa-202">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-202">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e83aa-203">d.</span><span class="sxs-lookup"><span data-stu-id="e83aa-203">d.</span></span> <span data-ttu-id="e83aa-204">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-204">Click **Create**.</span></span>
 
### <a name="creating-a-performancecentre-test-user"></a><span data-ttu-id="e83aa-205">Vytvoření zkušebního uživatele PerformanceCentre</span><span class="sxs-lookup"><span data-stu-id="e83aa-205">Creating a PerformanceCentre test user</span></span>

<span data-ttu-id="e83aa-206">Hello cílem této části je toocreate volal Britta Simon v PerformanceCentre uživatele.</span><span class="sxs-lookup"><span data-stu-id="e83aa-206">hello objective of this section is toocreate a user called Britta Simon in PerformanceCentre.</span></span>

<span data-ttu-id="e83aa-207">**toocreate uživatel volal Britta Simon v PerformanceCentre, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e83aa-207">**toocreate a user called Britta Simon in PerformanceCentre, perform hello following steps:**</span></span>

1. <span data-ttu-id="e83aa-208">Přihlaste se na tooyour PerformanceCentre společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="e83aa-208">Sign on tooyour PerformanceCentre company site as administrator.</span></span>

2. <span data-ttu-id="e83aa-209">V nabídce hello hello vlevo, klikněte na **Interrelate**a potom klikněte na **vytvořit účastník**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-209">In hello menu on hello left, click **Interrelate**, and then click **Create Participant**.</span></span>
   
    ![Vytvoření uživatele][400]

3. <span data-ttu-id="e83aa-211">Na hello **vztah mezi – vytvoření účastník** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e83aa-211">On hello **Interrelate - Create Participant** dialog, perform hello following steps:</span></span>
   
    ![Vytvoření uživatele][401]
    
    <span data-ttu-id="e83aa-213">a.</span><span class="sxs-lookup"><span data-stu-id="e83aa-213">a.</span></span> <span data-ttu-id="e83aa-214">Zadejte atributy hello požadované pro Britta Simon do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="e83aa-214">Type hello required attributes for Britta Simon into related textboxes.</span></span>
    
    >[!IMPORTANT]
    ><span data-ttu-id="e83aa-215">Britta na uživatelské jméno, které musí být atribut v PerformanceCentre hello stejná hodnota jako hello uživatelské jméno ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e83aa-215">Britta's User Name attribute in PerformanceCentre must be hello same as hello User Name in Azure AD.</span></span>
    
    <span data-ttu-id="e83aa-216">b.</span><span class="sxs-lookup"><span data-stu-id="e83aa-216">b.</span></span> <span data-ttu-id="e83aa-217">Vyberte **správce klienta** jako **zvolte roli**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-217">Select **Client Administrator** as **Choose Role**.</span></span>
    
    <span data-ttu-id="e83aa-218">c.</span><span class="sxs-lookup"><span data-stu-id="e83aa-218">c.</span></span> <span data-ttu-id="e83aa-219">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-219">Click **Save**.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e83aa-220">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="e83aa-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e83aa-221">V této části povolíte tak, že udělíte přístup tooPerformanceCentre toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e83aa-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPerformanceCentre.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e83aa-223">**tooassign Britta Simon tooPerformanceCentre, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e83aa-223">**tooassign Britta Simon tooPerformanceCentre, perform hello following steps:**</span></span>

1. <span data-ttu-id="e83aa-224">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e83aa-226">V seznamu aplikace hello vyberte **PerformanceCentre**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-226">In hello applications list, select **PerformanceCentre**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_app.png) 

3. <span data-ttu-id="e83aa-228">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e83aa-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e83aa-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e83aa-230">Click **Add** button.</span></span> <span data-ttu-id="e83aa-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e83aa-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e83aa-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="e83aa-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e83aa-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e83aa-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e83aa-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e83aa-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e83aa-236">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e83aa-236">Testing single sign-on</span></span>

<span data-ttu-id="e83aa-237">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="e83aa-237">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="e83aa-238">Když kliknete na dlaždici PerformanceCentre hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour PerformanceCentre aplikace.</span><span class="sxs-lookup"><span data-stu-id="e83aa-238">When you click hello PerformanceCentre tile in hello Access Panel, you should get automatically signed-on tooyour PerformanceCentre application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e83aa-239">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e83aa-239">Additional resources</span></span>

* [<span data-ttu-id="e83aa-240">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e83aa-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e83aa-241">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e83aa-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_06.png
[11]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_07.png
[12]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_08.png
[13]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_09.png
[14]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_10.png

[100]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_203.png
[400]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_11.png
[401]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_12.png

