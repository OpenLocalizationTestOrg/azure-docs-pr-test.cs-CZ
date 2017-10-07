---
title: "Kurz: Azure Active Directory integrace s týmovou spolupráci | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a týmovou spolupráci."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 03760032-3d76-4b47-ab84-241f72fbd561
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: f3a88a146f2a0a70de5ef58abd46f7f26b4104f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamwork"></a><span data-ttu-id="cd7ff-103">Kurz: Azure Active Directory integrace s týmovou spolupráci</span><span class="sxs-lookup"><span data-stu-id="cd7ff-103">Tutorial: Azure Active Directory integration with Teamwork</span></span>

<span data-ttu-id="cd7ff-104">V tomto kurzu zjistíte, jak toointegrate týmovou spolupráci se službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cd7ff-104">In this tutorial, you learn how toointegrate Teamwork with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cd7ff-105">Integrace týmovou spolupráci s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="cd7ff-105">Integrating Teamwork with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cd7ff-106">Můžete řídit ve službě Azure AD, který má přístup tooTeamwork</span><span class="sxs-lookup"><span data-stu-id="cd7ff-106">You can control in Azure AD who has access tooTeamwork</span></span>
- <span data-ttu-id="cd7ff-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTeamwork (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="cd7ff-107">You can enable your users tooautomatically get signed-on tooTeamwork (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cd7ff-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="cd7ff-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="cd7ff-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cd7ff-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd7ff-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cd7ff-110">Prerequisites</span></span>

<span data-ttu-id="cd7ff-111">tooconfigure integrace Azure AD s týmovou spolupráci, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="cd7ff-111">tooconfigure Azure AD integration with Teamwork, you need hello following items:</span></span>

- <span data-ttu-id="cd7ff-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="cd7ff-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cd7ff-113">Týmovou spolupráci jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="cd7ff-113">A Teamwork single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="cd7ff-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="cd7ff-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="cd7ff-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cd7ff-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="cd7ff-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cd7ff-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="cd7ff-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="cd7ff-118">Scenario description</span></span>
<span data-ttu-id="cd7ff-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cd7ff-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="cd7ff-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cd7ff-121">Přidání týmovou spolupráci z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cd7ff-121">Adding Teamwork from hello gallery</span></span>
2. <span data-ttu-id="cd7ff-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cd7ff-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-teamwork-from-hello-gallery"></a><span data-ttu-id="cd7ff-123">Přidání týmovou spolupráci z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cd7ff-123">Adding Teamwork from hello gallery</span></span>
<span data-ttu-id="cd7ff-124">tooconfigure hello integrace týmovou spolupráci do Azure AD, je nutné tooadd týmovou spolupráci hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-124">tooconfigure hello integration of Teamwork into Azure AD, you need tooadd Teamwork from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cd7ff-125">**tooadd týmovou spolupráci z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cd7ff-125">**tooadd Teamwork from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd7ff-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cd7ff-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cd7ff-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="cd7ff-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="cd7ff-133">Hello vyhledávacího pole zadejte **týmovou spolupráci**.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-133">In hello search box, type **Teamwork**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_001.png)

5. <span data-ttu-id="cd7ff-135">Na panelu výsledků hello vyberte **týmovou spolupráci**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-135">In hello results panel, select **Teamwork**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cd7ff-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cd7ff-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cd7ff-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s týmovou spolupráci podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="cd7ff-138">In this section, you configure and test Azure AD single sign-on with Teamwork based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cd7ff-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v týmovou spolupráci je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Teamwork is tooa user in Azure AD.</span></span> <span data-ttu-id="cd7ff-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v týmovou spolupráci musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-140">In other words, a link relationship between an Azure AD user and hello related user in Teamwork needs toobe established.</span></span>

<span data-ttu-id="cd7ff-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v týmovou spolupráci.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Teamwork.</span></span>

<span data-ttu-id="cd7ff-142">tooconfigure a testu Azure AD jednotné přihlašování s týmovou spolupráci, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="cd7ff-142">tooconfigure and test Azure AD single sign-on with Teamwork, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cd7ff-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cd7ff-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cd7ff-145">**[Vytvoření zkušebního uživatele týmovou spolupráci](#creating-a-teamwork-test-user)**  -toohave protějšek Britta Simon v týmovou spolupráci, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-145">**[Creating a Teamwork test user](#creating-a-teamwork-test-user)** - toohave a counterpart of Britta Simon in Teamwork that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="cd7ff-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cd7ff-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cd7ff-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cd7ff-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cd7ff-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci týmovou spolupráci.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Teamwork application.</span></span>

<span data-ttu-id="cd7ff-150">**tooconfigure Azure AD jednotné přihlašování s týmovou spolupráci, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cd7ff-150">**tooconfigure Azure AD single sign-on with Teamwork, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd7ff-151">V hello Azure Management portal na hello **týmovou spolupráci** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-151">In hello Azure Management portal, on hello **Teamwork** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="cd7ff-153">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_01.png)

3. <span data-ttu-id="cd7ff-155">Na hello **týmovou spolupráci domény a adresy URL** oddílem se v hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.teamwork.com`</span><span class="sxs-lookup"><span data-stu-id="cd7ff-155">On hello **Teamwork Domain and URLs** section, in hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<company name>.teamwork.com`</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_02.png)

    > [!NOTE] 
    > <span data-ttu-id="cd7ff-157">Upozorňujeme, že se nejedná hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-157">Please note that this is not hello real value.</span></span> <span data-ttu-id="cd7ff-158">Máte tooupdate tuto hodnotu s hello skutečné přihlásit na adrese URL.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-158">You have tooupdate this value with hello actual Sign On URL.</span></span> <span data-ttu-id="cd7ff-159">Obraťte se na [tým podpory týmovou spolupráci](mailto:support@teamwork.com) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-159">Contact [Teamwork support team](mailto:support@teamwork.com) tooget this value.</span></span> 

4. <span data-ttu-id="cd7ff-160">Na hello **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-160">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_03.png)   

5. <span data-ttu-id="cd7ff-162">Na hello **vytvořit nový certifikát** dialogové okno, klikněte na ikonu hello kalendáře a vyberte **datum vypršení platnosti**.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-162">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="cd7ff-163">Pak klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-163">Then click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamwork-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="cd7ff-165">Na hello **SAML podpisový certifikát** vyberte **aktivujte nový certifikát** a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-165">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_04.png)

7. <span data-ttu-id="cd7ff-167">V místní nabídce hello **certifikát výměny** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-167">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamwork-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="cd7ff-169">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_05.png) 

9. <span data-ttu-id="cd7ff-171">tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, obraťte se na [tým podpory týmovou spolupráci](mailto:support@teamwork.com) a poskytnout hello Stáhnout **metadata**.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-171">tooget SSO configured for your application, contact [Teamwork support team](mailto:support@teamwork.com) and provide them with hello downloaded **metadata**.</span></span>
  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cd7ff-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cd7ff-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="cd7ff-173">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-173">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="cd7ff-175">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cd7ff-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd7ff-176">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-176">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamwork-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cd7ff-178">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-178">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamwork-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cd7ff-180">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-180">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamwork-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cd7ff-182">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cd7ff-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamwork-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cd7ff-184">a.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-184">a.</span></span> <span data-ttu-id="cd7ff-185">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cd7ff-186">b.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-186">b.</span></span> <span data-ttu-id="cd7ff-187">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cd7ff-188">c.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-188">c.</span></span> <span data-ttu-id="cd7ff-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cd7ff-190">d.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-190">d.</span></span> <span data-ttu-id="cd7ff-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-191">Click **Create**.</span></span> 



### <a name="creating-a-teamwork-test-user"></a><span data-ttu-id="cd7ff-192">Vytvoření zkušebního uživatele týmovou spolupráci</span><span class="sxs-lookup"><span data-stu-id="cd7ff-192">Creating a Teamwork test user</span></span>

<span data-ttu-id="cd7ff-193">V této části vytvoříte uživatele volal Britta Simon v týmovou spolupráci.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-193">In this section, you create a user called Britta Simon in Teamwork.</span></span> <span data-ttu-id="cd7ff-194">Spojte se s [tým podpory týmovou spolupráci](mailto:support@teamwork.com) tooadd hello uživatelé v platformě týmovou spolupráci hello.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-194">Please work with [Teamwork support team](mailto:support@teamwork.com) tooadd hello users in hello Teamwork platform.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cd7ff-195">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="cd7ff-195">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cd7ff-196">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooTeamwork svůj přístup.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-196">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooTeamwork.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="cd7ff-198">**tooassign Britta Simon tooTeamwork, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cd7ff-198">**tooassign Britta Simon tooTeamwork, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd7ff-199">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-199">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="cd7ff-201">V seznamu aplikace hello vyberte **týmovou spolupráci**.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-201">In hello applications list, select **Teamwork**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_50.png) 

3. <span data-ttu-id="cd7ff-203">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-203">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="cd7ff-205">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-205">Click **Add** button.</span></span> <span data-ttu-id="cd7ff-206">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="cd7ff-208">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-208">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cd7ff-209">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cd7ff-210">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="cd7ff-211">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cd7ff-211">Testing single sign-on</span></span>

<span data-ttu-id="cd7ff-212">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-212">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="cd7ff-213">Když kliknete na dlaždici týmovou spolupráci hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour týmovou spolupráci aplikací.</span><span class="sxs-lookup"><span data-stu-id="cd7ff-213">When you click hello Teamwork tile in hello Access Panel, you should get automatically signed-on tooyour Teamwork application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="cd7ff-214">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cd7ff-214">Additional resources</span></span>

* [<span data-ttu-id="cd7ff-215">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cd7ff-215">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cd7ff-216">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cd7ff-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_203.png