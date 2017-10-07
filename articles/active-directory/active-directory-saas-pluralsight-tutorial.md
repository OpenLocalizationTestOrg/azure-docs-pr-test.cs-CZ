---
title: 'Kurz: Azure Active Directory integrace s Pluralsight | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Pluralsight."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4c3f07d2-4e1f-4ea3-9025-c663f1f2b7b4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: c8394eed79f21fb889816d8dafe2d71187be72b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pluralsight"></a><span data-ttu-id="a7839-103">Kurz: Azure Active Directory integrace s Pluralsight</span><span class="sxs-lookup"><span data-stu-id="a7839-103">Tutorial: Azure Active Directory integration with Pluralsight</span></span>

<span data-ttu-id="a7839-104">V tomto kurzu zjistíte, jak toointegrate Pluralsight s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a7839-104">In this tutorial, you learn how toointegrate Pluralsight with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a7839-105">Integrace Pluralsight s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a7839-105">Integrating Pluralsight with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a7839-106">Můžete řídit ve službě Azure AD, který má přístup tooPluralsight</span><span class="sxs-lookup"><span data-stu-id="a7839-106">You can control in Azure AD who has access tooPluralsight</span></span>
- <span data-ttu-id="a7839-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPluralsight (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a7839-107">You can enable your users tooautomatically get signed-on tooPluralsight (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a7839-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a7839-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a7839-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a7839-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a7839-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a7839-110">Prerequisites</span></span>

<span data-ttu-id="a7839-111">Integrace služby Azure AD s Pluralsight tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="a7839-111">tooconfigure Azure AD integration with Pluralsight, you need hello following items:</span></span>

- <span data-ttu-id="a7839-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a7839-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a7839-113">Pluralsight jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a7839-113">A Pluralsight single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a7839-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a7839-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a7839-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a7839-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a7839-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a7839-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a7839-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a7839-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a7839-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a7839-118">Scenario description</span></span>
<span data-ttu-id="a7839-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a7839-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a7839-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a7839-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a7839-121">Přidání Pluralsight z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a7839-121">Adding Pluralsight from hello gallery</span></span>
2. <span data-ttu-id="a7839-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a7839-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pluralsight-from-hello-gallery"></a><span data-ttu-id="a7839-123">Přidání Pluralsight z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a7839-123">Adding Pluralsight from hello gallery</span></span>
<span data-ttu-id="a7839-124">tooconfigure hello integrace Pluralsight do Azure AD, je nutné tooadd Pluralsight hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a7839-124">tooconfigure hello integration of Pluralsight into Azure AD, you need tooadd Pluralsight from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a7839-125">**tooadd Pluralsight z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a7839-125">**tooadd Pluralsight from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a7839-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a7839-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a7839-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a7839-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a7839-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a7839-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a7839-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a7839-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a7839-133">Hello vyhledávacího pole zadejte **Pluralsight**.</span><span class="sxs-lookup"><span data-stu-id="a7839-133">In hello search box, type **Pluralsight**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_search.png)

5. <span data-ttu-id="a7839-135">Na panelu výsledků hello vyberte **Pluralsight**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="a7839-135">In hello results panel, select **Pluralsight**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a7839-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a7839-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a7839-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Pluralsight podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a7839-138">In this section, you configure and test Azure AD single sign-on with Pluralsight based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a7839-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Pluralsight je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a7839-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Pluralsight is tooa user in Azure AD.</span></span> <span data-ttu-id="a7839-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Pluralsight musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="a7839-140">In other words, a link relationship between an Azure AD user and hello related user in Pluralsight needs toobe established.</span></span>

<span data-ttu-id="a7839-141">V Pluralsight, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="a7839-141">In Pluralsight, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a7839-142">tooconfigure a testu Azure AD jednotné přihlašování s Pluralsight, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="a7839-142">tooconfigure and test Azure AD single sign-on with Pluralsight, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a7839-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="a7839-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a7839-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a7839-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a7839-145">**[Vytvoření zkušebního uživatele Pluralsight](#creating-a-pluralsight-test-user)**  -toohave protějšek Britta Simon v Pluralsight, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="a7839-145">**[Creating a Pluralsight test user](#creating-a-pluralsight-test-user)** - toohave a counterpart of Britta Simon in Pluralsight that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a7839-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a7839-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a7839-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="a7839-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a7839-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a7839-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a7839-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Pluralsight.</span><span class="sxs-lookup"><span data-stu-id="a7839-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Pluralsight application.</span></span>

<span data-ttu-id="a7839-150">**tooconfigure Azure AD jednotné přihlašování s Pluralsight, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a7839-150">**tooconfigure Azure AD single sign-on with Pluralsight, perform hello following steps:**</span></span>

1. <span data-ttu-id="a7839-151">V portálu Azure, na hello hello **Pluralsight** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a7839-151">In hello Azure portal, on hello **Pluralsight** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a7839-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a7839-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_samlbase.png)

3. <span data-ttu-id="a7839-155">Na hello **Pluralsight domény a adresy URL** část, proveďte následující hello:</span><span class="sxs-lookup"><span data-stu-id="a7839-155">On hello **Pluralsight Domain and URLs** section, perform hello following:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_url.png)

    <span data-ttu-id="a7839-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instance name>.pluralsight.com/sso/<company name>`</span><span class="sxs-lookup"><span data-stu-id="a7839-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<instance name>.pluralsight.com/sso/<company name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a7839-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="a7839-158">This value is not real.</span></span> <span data-ttu-id="a7839-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a7839-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="a7839-160">Obraťte se na [tým podpory Pluralsight klienta](mailto:support@pluralsight.com) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a7839-160">Contact [Pluralsight Client support team](mailto:support@pluralsight.com) tooget this value.</span></span> 
 


4. <span data-ttu-id="a7839-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="a7839-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_certificate.png) 

5. <span data-ttu-id="a7839-163">Hello cílem této části je tooenable Azure AD jednotné přihlašování v hello portál Azure a tooconfigure jednotné přihlašování v aplikaci Pluralsight hello.</span><span class="sxs-lookup"><span data-stu-id="a7839-163">hello objective of this section is tooenable Azure AD single sign-on in hello Azure portal and tooconfigure SSO in hello Pluralsight application.</span></span>

    <span data-ttu-id="a7839-164">Hello Pluralsight aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování tooyour tokenu atributy konfigurace SAML.</span><span class="sxs-lookup"><span data-stu-id="a7839-164">hello Pluralsight application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="a7839-165">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="a7839-165">hello following screenshot shows an example for this.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_attribute.png)

    >[!NOTE]
    ><span data-ttu-id="a7839-167">Můžete také přidat hello **"Jedinečné ID"** atribut s hello odpovídající hodnotu jako EmployeeID nebo něco jiného, který vyhovuje pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="a7839-167">You can also add hello **"Unique ID"** attribute with hello appropriate value like EmployeeID or something else which suits for your organization.</span></span> <span data-ttu-id="a7839-168">Všimněte si, že to není povinný atribut hello; ale můžete přidat příliš identifikovat jedinečná uživatelská hello.</span><span class="sxs-lookup"><span data-stu-id="a7839-168">Also note that this is not hello required attribute; however, you can add it too identify hello unique user.</span></span> 

6. <span data-ttu-id="a7839-169">hello tooadd požadované **atributy tokenu SAML**, pro každý řádek v tabulce hello níže, proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a7839-169">tooadd hello required **SAML token attributes**, for each row shown in hello table below, perform hello following steps:</span></span>
   
   | <span data-ttu-id="a7839-170">Název atributu</span><span class="sxs-lookup"><span data-stu-id="a7839-170">Attribute Name</span></span> | <span data-ttu-id="a7839-171">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="a7839-171">Attribute Value</span></span> |
   | ---| --- |
   | <span data-ttu-id="a7839-172">Jméno</span><span class="sxs-lookup"><span data-stu-id="a7839-172">First Name</span></span> |<span data-ttu-id="a7839-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="a7839-173">user.givenname</span></span> |
   | <span data-ttu-id="a7839-174">Příjmení</span><span class="sxs-lookup"><span data-stu-id="a7839-174">Last Name</span></span> |<span data-ttu-id="a7839-175">User.Surname</span><span class="sxs-lookup"><span data-stu-id="a7839-175">user.surname</span></span> |
   | <span data-ttu-id="a7839-176">E-mail</span><span class="sxs-lookup"><span data-stu-id="a7839-176">Email</span></span> |<span data-ttu-id="a7839-177">User.Mail</span><span class="sxs-lookup"><span data-stu-id="a7839-177">user.mail</span></span> |
   
   <span data-ttu-id="a7839-178">a.</span><span class="sxs-lookup"><span data-stu-id="a7839-178">a.</span></span> <span data-ttu-id="a7839-179">Klikněte na tlačítko **přidat atribut uživatele** tooopen hello **přidat uživatele Attribure** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a7839-179">Click **add user attribute** tooopen hello **Add User Attribure** dialog.</span></span>
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_addattribute.png)
  
   <span data-ttu-id="a7839-181">b.</span><span class="sxs-lookup"><span data-stu-id="a7839-181">b.</span></span> <span data-ttu-id="a7839-182">V hello **název atributu** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="a7839-182">In hello **Attribute Name** textbox, type hello attribute name shown for that row.</span></span>
  
   <span data-ttu-id="a7839-183">c.</span><span class="sxs-lookup"><span data-stu-id="a7839-183">c.</span></span> <span data-ttu-id="a7839-184">Z hello **hodnota atributu** seznamu, vyberte hello hodnota atributu zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="a7839-184">From hello **Attribute Value** list, select hello attribute value shown for that row.</span></span>
  
   <span data-ttu-id="a7839-185">d.</span><span class="sxs-lookup"><span data-stu-id="a7839-185">d.</span></span> <span data-ttu-id="a7839-186">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a7839-186">Click **Ok**.</span></span>    

7. <span data-ttu-id="a7839-187">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a7839-187">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="a7839-189">tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, obraťte se na [Pluralsight Professional služby](mailTo:professionalservices@pluralsight.com) týmu a zadejte soubor stažený metadat hello.</span><span class="sxs-lookup"><span data-stu-id="a7839-189">tooget SSO configured for your application, contact [Pluralsight Professional Services](mailTo:professionalservices@pluralsight.com) team and provide hello downloaded metadata file.</span></span>

> [!TIP]
> <span data-ttu-id="a7839-190">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="a7839-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a7839-191">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="a7839-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a7839-192">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a7839-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a7839-193">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a7839-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="a7839-194">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="a7839-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a7839-196">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a7839-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a7839-197">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a7839-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a7839-199">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a7839-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a7839-201">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="a7839-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a7839-203">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a7839-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a7839-205">a.</span><span class="sxs-lookup"><span data-stu-id="a7839-205">a.</span></span> <span data-ttu-id="a7839-206">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a7839-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a7839-207">b.</span><span class="sxs-lookup"><span data-stu-id="a7839-207">b.</span></span> <span data-ttu-id="a7839-208">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a7839-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a7839-209">c.</span><span class="sxs-lookup"><span data-stu-id="a7839-209">c.</span></span> <span data-ttu-id="a7839-210">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a7839-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a7839-211">d.</span><span class="sxs-lookup"><span data-stu-id="a7839-211">d.</span></span> <span data-ttu-id="a7839-212">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a7839-212">Click **Create**.</span></span>
 
### <a name="creating-a-pluralsight-test-user"></a><span data-ttu-id="a7839-213">Vytvoření zkušebního uživatele Pluralsight</span><span class="sxs-lookup"><span data-stu-id="a7839-213">Creating a Pluralsight test user</span></span>

<span data-ttu-id="a7839-214">Hello cílem této části je toocreate volal Britta Simon v Pluralsight uživatele.</span><span class="sxs-lookup"><span data-stu-id="a7839-214">hello objective of this section is toocreate a user called Britta Simon in Pluralsight.</span></span> <span data-ttu-id="a7839-215">Spojte se s [tým podpory Pluralsight klienta](mailto:support@pluralsight.com) tooadd hello uživatele v hello Pluralsight účtu.</span><span class="sxs-lookup"><span data-stu-id="a7839-215">Please work with [Pluralsight Client support team](mailto:support@pluralsight.com) tooadd hello users in hello Pluralsight account.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a7839-216">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="a7839-216">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a7839-217">V této části povolíte tak, že udělíte přístup tooPluralsight toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a7839-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPluralsight.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a7839-219">**tooassign Britta Simon tooPluralsight, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a7839-219">**tooassign Britta Simon tooPluralsight, perform hello following steps:**</span></span>

1. <span data-ttu-id="a7839-220">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a7839-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a7839-222">V seznamu aplikace hello vyberte **Pluralsight**.</span><span class="sxs-lookup"><span data-stu-id="a7839-222">In hello applications list, select **Pluralsight**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_app.png) 

3. <span data-ttu-id="a7839-224">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a7839-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a7839-226">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a7839-226">Click **Add** button.</span></span> <span data-ttu-id="a7839-227">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a7839-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a7839-229">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="a7839-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a7839-230">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a7839-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a7839-231">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a7839-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a7839-232">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a7839-232">Testing single sign-on</span></span>

<span data-ttu-id="a7839-233">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a7839-233">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a7839-234">Když kliknete na dlaždici Pluralsight hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Pluralsight aplikace.</span><span class="sxs-lookup"><span data-stu-id="a7839-234">When you click hello Pluralsight tile in hello Access Panel, you should get automatically signed-on tooyour Pluralsight application.</span></span> <span data-ttu-id="a7839-235">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a7839-235">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a7839-236">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a7839-236">Additional resources</span></span>

* [<span data-ttu-id="a7839-237">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a7839-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a7839-238">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a7839-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_203.png

