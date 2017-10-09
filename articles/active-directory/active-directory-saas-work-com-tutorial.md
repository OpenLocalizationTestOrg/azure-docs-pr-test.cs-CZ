---
title: 'Kurz: Azure Active Directory integrace s Work.com | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Work.com."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: dcdc51c884abd78c945b649de99f942d32373cf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a><span data-ttu-id="a6821-103">Kurz: Azure Active Directory integrace s Work.com</span><span class="sxs-lookup"><span data-stu-id="a6821-103">Tutorial: Azure Active Directory integration with Work.com</span></span>

<span data-ttu-id="a6821-104">V tomto kurzu zjistíte, jak toointegrate Work.com s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a6821-104">In this tutorial, you learn how toointegrate Work.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a6821-105">Integrace Work.com s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a6821-105">Integrating Work.com with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a6821-106">Můžete řídit ve službě Azure AD, který má přístup tooWork.com</span><span class="sxs-lookup"><span data-stu-id="a6821-106">You can control in Azure AD who has access tooWork.com</span></span>
- <span data-ttu-id="a6821-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWork.com (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a6821-107">You can enable your users tooautomatically get signed-on tooWork.com (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a6821-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a6821-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a6821-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a6821-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a6821-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a6821-110">Prerequisites</span></span>

<span data-ttu-id="a6821-111">Integrace služby Azure AD s Work.com tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="a6821-111">tooconfigure Azure AD integration with Work.com, you need hello following items:</span></span>

- <span data-ttu-id="a6821-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a6821-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a6821-113">Work.com jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a6821-113">A Work.com single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a6821-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a6821-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a6821-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a6821-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a6821-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a6821-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a6821-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a6821-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a6821-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a6821-118">Scenario description</span></span>
<span data-ttu-id="a6821-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a6821-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a6821-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a6821-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a6821-121">Přidat Work.com z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a6821-121">Add Work.com from hello gallery</span></span>
2. <span data-ttu-id="a6821-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a6821-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-workcom-from-hello-gallery"></a><span data-ttu-id="a6821-123">Přidat Work.com z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a6821-123">Add Work.com from hello gallery</span></span>
<span data-ttu-id="a6821-124">tooconfigure hello integrace Work.com do Azure AD, je nutné tooadd Work.com hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a6821-124">tooconfigure hello integration of Work.com into Azure AD, you need tooadd Work.com from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a6821-125">**tooadd Work.com z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a6821-125">**tooadd Work.com from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6821-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a6821-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a6821-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a6821-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a6821-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a6821-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a6821-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a6821-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a6821-133">Hello vyhledávacího pole zadejte **Work.com**, vyberte **Work.com** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="a6821-133">In hello search box, type **Work.com**, select **Work.com** from results panel then click **Add** button tooadd hello application.</span></span>

    ![Přidat z Galerie](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="a6821-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a6821-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="a6821-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Work.com podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a6821-136">In this section, you configure and test Azure AD single sign-on with Work.com based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a6821-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Work.com je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a6821-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Work.com is tooa user in Azure AD.</span></span> <span data-ttu-id="a6821-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Work.com musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="a6821-138">In other words, a link relationship between an Azure AD user and hello related user in Work.com needs toobe established.</span></span>

<span data-ttu-id="a6821-139">V Work.com, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="a6821-139">In Work.com, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a6821-140">tooconfigure a testu Azure AD jednotné přihlašování s Work.com, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="a6821-140">tooconfigure and test Azure AD single sign-on with Work.com, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a6821-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="a6821-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a6821-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a6821-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a6821-143">**[Vytvoření zkušebního uživatele Work.com](#create-a-workcom-test-user)**  -toohave protějšek Britta Simon v Work.com, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="a6821-143">**[Create a Work.com test user](#create-a-workcom-test-user)** - toohave a counterpart of Britta Simon in Work.com that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a6821-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a6821-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a6821-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="a6821-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="a6821-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a6821-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="a6821-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Work.com.</span><span class="sxs-lookup"><span data-stu-id="a6821-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Work.com application.</span></span>

>[!NOTE]
><span data-ttu-id="a6821-148">tooconfigure jednotné přihlašování, musíte toosetup vlastní název domény Work.com ještě.</span><span class="sxs-lookup"><span data-stu-id="a6821-148">tooconfigure single sign-on, you need toosetup a custom Work.com domain name yet.</span></span> <span data-ttu-id="a6821-149">Je třeba toodefine alespoň domény název, testovací název vaší domény a nasadíte ho tooyour celé organizace.</span><span class="sxs-lookup"><span data-stu-id="a6821-149">You need toodefine at least a domain name, test your domain name, and deploy it tooyour entire organization.</span></span>

<span data-ttu-id="a6821-150">**tooconfigure Azure AD jednotné přihlašování s Work.com, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a6821-150">**tooconfigure Azure AD single sign-on with Work.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6821-151">V portálu Azure, na hello hello **Work.com** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a6821-151">In hello Azure portal, on hello **Work.com** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a6821-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a6821-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Na základě SAML přihlášení](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_samlbase.png)

3. <span data-ttu-id="a6821-155">Na hello **Work.com domény a adresy URL** část, proveďte následující hello:</span><span class="sxs-lookup"><span data-stu-id="a6821-155">On hello **Work.com Domain and URLs** section, perform hello following:</span></span>

    ![Část Work.com domény a adresy URL](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_url.png)

    <span data-ttu-id="a6821-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`http://<companyname>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="a6821-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `http://<companyname>.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a6821-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="a6821-158">This value is not real.</span></span> <span data-ttu-id="a6821-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a6821-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="a6821-160">Obraťte se na [tým podpory Work.com klienta](https://help.salesforce.com/articleView?id=000159855&type=3) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a6821-160">Contact [Work.com Client support team](https://help.salesforce.com/articleView?id=000159855&type=3) tooget this value.</span></span> 

4. <span data-ttu-id="a6821-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="a6821-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Certifikát pro podpis SAML části](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_certificate.png) 

5. <span data-ttu-id="a6821-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a6821-163">Click **Save** button.</span></span>

    ![Tlačítko Uložit](./media/active-directory-saas-work-com-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a6821-165">Na hello **Work.com konfigurace** klikněte na tlačítko **konfigurace Work.com** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a6821-165">On hello **Work.com Configuration** section, click **Configure Work.com** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a6821-166">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="a6821-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Work.com konfigurační oddíl](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_configure.png) 
7. <span data-ttu-id="a6821-168">Přihlaste se tooyour Work.com klienta jako správce.</span><span class="sxs-lookup"><span data-stu-id="a6821-168">Log in tooyour Work.com tenant as administrator.</span></span>

8. <span data-ttu-id="a6821-169">Přejděte příliš**instalační program**.</span><span class="sxs-lookup"><span data-stu-id="a6821-169">Go too**Setup**.</span></span>
   
    <span data-ttu-id="a6821-170">![Instalační program](./media/active-directory-saas-work-com-tutorial/ic794108.png "instalační program")</span><span class="sxs-lookup"><span data-stu-id="a6821-170">![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")</span></span>

9. <span data-ttu-id="a6821-171">V levém navigačním podokně hello v hello **Správa** klikněte na tlačítko **Správa domén** tooexpand hello související části a pak klikněte na **Moje domény** tooopen hello  **Moje doména** stránky.</span><span class="sxs-lookup"><span data-stu-id="a6821-171">On hello left navigation pane, in hello **Administer** section, click **Domain Management** tooexpand hello related section, and then click **My Domain** tooopen hello **My Domain** page.</span></span> 
   
    <span data-ttu-id="a6821-172">![Moje doména](./media/active-directory-saas-work-com-tutorial/ic767825.png "Moje doména")</span><span class="sxs-lookup"><span data-stu-id="a6821-172">![My Domain](./media/active-directory-saas-work-com-tutorial/ic767825.png "My Domain")</span></span>

10. <span data-ttu-id="a6821-173">tooverify, který vaše doména byla nastavena správně, ujistěte se, že je v "**krok 4 nasazené tooUsers**" a zkontrolovat vaše "**Moje nastavení domény**".</span><span class="sxs-lookup"><span data-stu-id="a6821-173">tooverify that your domain has been set up correctly, make sure that it is in “**Step 4 Deployed tooUsers**” and review your “**My Domain Settings**”.</span></span>
   
    <span data-ttu-id="a6821-174">![Domény nasazen tooUser](./media/active-directory-saas-work-com-tutorial/ic784377.png "tooUser nasazené v doméně")</span><span class="sxs-lookup"><span data-stu-id="a6821-174">![Domain Deployed tooUser](./media/active-directory-saas-work-com-tutorial/ic784377.png "Domain Deployed tooUser")</span></span>

11. <span data-ttu-id="a6821-175">Přihlaste se tooyour Work.com klienta.</span><span class="sxs-lookup"><span data-stu-id="a6821-175">Log in tooyour Work.com tenant.</span></span>

12. <span data-ttu-id="a6821-176">Přejděte příliš**instalační program**.</span><span class="sxs-lookup"><span data-stu-id="a6821-176">Go too**Setup**.</span></span>
    
    <span data-ttu-id="a6821-177">![Instalační program](./media/active-directory-saas-work-com-tutorial/ic794108.png "instalační program")</span><span class="sxs-lookup"><span data-stu-id="a6821-177">![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")</span></span>

13. <span data-ttu-id="a6821-178">Rozbalte hello **ovládacích prvků zabezpečení** nabídce a pak klikněte na tlačítko **nastavení jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a6821-178">Expand hello **Security Controls** menu, and then click **Single Sign-On Settings**.</span></span>
    
    <span data-ttu-id="a6821-179">![Jednotné přihlašování v nastavení](./media/active-directory-saas-work-com-tutorial/ic794113.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="a6821-179">![Single Sign-On Settings](./media/active-directory-saas-work-com-tutorial/ic794113.png "Single Sign-On Settings")</span></span>

14. <span data-ttu-id="a6821-180">Na hello **nastavení jednotného přihlašování** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a6821-180">On hello **Single Sign-On Settings** dialog page, perform hello following steps:</span></span>
    
    <span data-ttu-id="a6821-181">![Povolit SAML](./media/active-directory-saas-work-com-tutorial/ic781026.png "povoleno SAML")</span><span class="sxs-lookup"><span data-stu-id="a6821-181">![SAML Enabled](./media/active-directory-saas-work-com-tutorial/ic781026.png "SAML Enabled")</span></span>
    
    <span data-ttu-id="a6821-182">a.</span><span class="sxs-lookup"><span data-stu-id="a6821-182">a.</span></span> <span data-ttu-id="a6821-183">Vyberte **povoleno SAML**.</span><span class="sxs-lookup"><span data-stu-id="a6821-183">Select **SAML Enabled**.</span></span>
    
    <span data-ttu-id="a6821-184">b.</span><span class="sxs-lookup"><span data-stu-id="a6821-184">b.</span></span> <span data-ttu-id="a6821-185">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="a6821-185">Click **New**.</span></span>

15. <span data-ttu-id="a6821-186">V hello **SAML jeden nastavení přihlášení** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="a6821-186">In hello **SAML Single Sign-On Settings** section, perform hello following steps:</span></span>
    
    <span data-ttu-id="a6821-187">![SAML jeden přihlašování nastavení](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML jeden přihlašování nastavení")</span><span class="sxs-lookup"><span data-stu-id="a6821-187">![SAML Single Sign-On Setting](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML Single Sign-On Setting")</span></span>
    
    <span data-ttu-id="a6821-188">a.</span><span class="sxs-lookup"><span data-stu-id="a6821-188">a.</span></span> <span data-ttu-id="a6821-189">V hello **název** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="a6821-189">In hello **Name** textbox, type a name for your configuration.</span></span>  
       
    > [!NOTE]
    > <span data-ttu-id="a6821-190">Poskytuje hodnotu pro **název** automaticky vyplnit hello **název rozhraní API** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a6821-190">Providing a value for **Name** does automatically populate hello **API Name** textbox.</span></span>
    
    <span data-ttu-id="a6821-191">b.</span><span class="sxs-lookup"><span data-stu-id="a6821-191">b.</span></span> <span data-ttu-id="a6821-192">V **vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a6821-192">In **Issuer** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="a6821-193">c.</span><span class="sxs-lookup"><span data-stu-id="a6821-193">c.</span></span> <span data-ttu-id="a6821-194">Klikněte na certifikát tooupload hello stáhli z portálu Azure, **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="a6821-194">tooupload hello downloaded certificate from Azure portal, click **Browse**.</span></span>
    
    <span data-ttu-id="a6821-195">d.</span><span class="sxs-lookup"><span data-stu-id="a6821-195">d.</span></span> <span data-ttu-id="a6821-196">V hello **Entity Id** textovému poli, typ `https://salesforce-work.com`.</span><span class="sxs-lookup"><span data-stu-id="a6821-196">In hello **Entity Id** textbox, type `https://salesforce-work.com`.</span></span>
    
    <span data-ttu-id="a6821-197">e.</span><span class="sxs-lookup"><span data-stu-id="a6821-197">e.</span></span> <span data-ttu-id="a6821-198">Jako **typ Identity SAML**, vyberte **kontrolní výraz obsahuje hello federace z objektu uživatele hello**.</span><span class="sxs-lookup"><span data-stu-id="a6821-198">As **SAML Identity Type**, select **Assertion contains hello Federation ID from hello User object**.</span></span>
    
    <span data-ttu-id="a6821-199">f.</span><span class="sxs-lookup"><span data-stu-id="a6821-199">f.</span></span> <span data-ttu-id="a6821-200">Jako **umístění Identity SAML**, vyberte **identita je v elementu NameIdentfier hello hello subjektu příkaz**.</span><span class="sxs-lookup"><span data-stu-id="a6821-200">As **SAML Identity Location**, select **Identity is in hello NameIdentfier element of hello Subject statement**.</span></span>
    
    <span data-ttu-id="a6821-201">g.</span><span class="sxs-lookup"><span data-stu-id="a6821-201">g.</span></span> <span data-ttu-id="a6821-202">V **adresu URL pro přihlášení zprostředkovatele Identity** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a6821-202">In **Identity Provider Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="a6821-203">h.</span><span class="sxs-lookup"><span data-stu-id="a6821-203">h.</span></span> <span data-ttu-id="a6821-204">V **adresa URL odhlašovací zprostředkovatele Identity** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a6821-204">In **Identity Provider Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="a6821-205">i.</span><span class="sxs-lookup"><span data-stu-id="a6821-205">i.</span></span> <span data-ttu-id="a6821-206">Jako **zprostředkovatele iniciované žádosti vazby služby**, vyberte **HTTP Post**.</span><span class="sxs-lookup"><span data-stu-id="a6821-206">As **Service Provider Initiated Request Binding**, select **HTTP Post**.</span></span>
    
    <span data-ttu-id="a6821-207">j.</span><span class="sxs-lookup"><span data-stu-id="a6821-207">j.</span></span> <span data-ttu-id="a6821-208">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a6821-208">Click **Save**.</span></span>

16. <span data-ttu-id="a6821-209">Portál classic Work.com, na levém navigačním podokně hello, klikněte na tlačítko **Správa domén** tooexpand hello související části a pak klikněte na **Moje domény** tooopen hello **Moje domény**stránky.</span><span class="sxs-lookup"><span data-stu-id="a6821-209">In your Work.com classic portal, on hello left navigation pane, click **Domain Management** tooexpand hello related section, and then click **My Domain** tooopen hello **My Domain** page.</span></span> 
    
    <span data-ttu-id="a6821-210">![Moje doména](./media/active-directory-saas-work-com-tutorial/ic794115.png "Moje doména")</span><span class="sxs-lookup"><span data-stu-id="a6821-210">![My Domain](./media/active-directory-saas-work-com-tutorial/ic794115.png "My Domain")</span></span>

17. <span data-ttu-id="a6821-211">Na hello **Moje domény** stránku hello **Branding přihlašovací stránky** klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="a6821-211">On hello **My Domain** page, in hello **Login Page Branding** section, click **Edit**.</span></span>
    
    <span data-ttu-id="a6821-212">![Branding přihlašovací stránky](./media/active-directory-saas-work-com-tutorial/ic767826.png "Branding přihlašovací stránky")</span><span class="sxs-lookup"><span data-stu-id="a6821-212">![Login Page Branding](./media/active-directory-saas-work-com-tutorial/ic767826.png "Login Page Branding")</span></span>

14. <span data-ttu-id="a6821-213">Na hello **Branding přihlašovací stránky** stránku hello **ověřovací služby** část, hello název vaší **nastavení jednotného přihlašování SAML** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="a6821-213">On hello **Login Page Branding** page, in hello **Authentication Service** section, hello name of your **SAML SSO Settings** is displayed.</span></span> <span data-ttu-id="a6821-214">Vyberte ji a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a6821-214">Select it, and then click **Save**.</span></span>
    
    <span data-ttu-id="a6821-215">![Branding přihlašovací stránky](./media/active-directory-saas-work-com-tutorial/ic784366.png "Branding přihlašovací stránky")</span><span class="sxs-lookup"><span data-stu-id="a6821-215">![Login Page Branding](./media/active-directory-saas-work-com-tutorial/ic784366.png "Login Page Branding")</span></span>

> [!TIP]
> <span data-ttu-id="a6821-216">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="a6821-216">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a6821-217">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="a6821-217">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a6821-218">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a6821-218">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="a6821-219">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a6821-219">Create an Azure AD test user</span></span>
<span data-ttu-id="a6821-220">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="a6821-220">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a6821-222">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a6821-222">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6821-223">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a6821-223">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-work-com-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a6821-225">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a6821-225">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Uživatelé a skupiny -> všichni uživatelé](./media/active-directory-saas-work-com-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a6821-227">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="a6821-227">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Přidat](./media/active-directory-saas-work-com-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a6821-229">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a6821-229">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Dialogové okno stránka uživatel](./media/active-directory-saas-work-com-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a6821-231">a.</span><span class="sxs-lookup"><span data-stu-id="a6821-231">a.</span></span> <span data-ttu-id="a6821-232">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a6821-232">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a6821-233">b.</span><span class="sxs-lookup"><span data-stu-id="a6821-233">b.</span></span> <span data-ttu-id="a6821-234">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a6821-234">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a6821-235">c.</span><span class="sxs-lookup"><span data-stu-id="a6821-235">c.</span></span> <span data-ttu-id="a6821-236">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a6821-236">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a6821-237">d.</span><span class="sxs-lookup"><span data-stu-id="a6821-237">d.</span></span> <span data-ttu-id="a6821-238">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a6821-238">Click **Create**.</span></span>
 
### <a name="create-a-workcom-test-user"></a><span data-ttu-id="a6821-239">Vytvoření zkušebního uživatele Work.com</span><span class="sxs-lookup"><span data-stu-id="a6821-239">Create a Work.com test user</span></span>
<span data-ttu-id="a6821-240">Pro Azure Active Directory uživatelům toobe možné toosign v musí být zřízená tooWork.com. V případě hello Work.com zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="a6821-240">For Azure Active Directory users toobe able toosign in, they must be provisioned tooWork.com. In hello case of Work.com, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="a6821-241">tooconfigure zřizování uživatelů, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="a6821-241">tooconfigure user provisioning, perform hello following steps:</span></span>
1. <span data-ttu-id="a6821-242">Přihlaste se na tooyour Work.com společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="a6821-242">Sign on tooyour Work.com company site as an administrator.</span></span>

2. <span data-ttu-id="a6821-243">Přejděte příliš**instalační program**.</span><span class="sxs-lookup"><span data-stu-id="a6821-243">Go too**Setup**.</span></span>
   
    <span data-ttu-id="a6821-244">![Instalační program](./media/active-directory-saas-work-com-tutorial/IC794108.png "instalační program")</span><span class="sxs-lookup"><span data-stu-id="a6821-244">![Setup](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")</span></span>
3. <span data-ttu-id="a6821-245">Přejděte příliš**spravovat uživatele \> uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a6821-245">Go too**Manage Users \> Users**.</span></span>
   
    <span data-ttu-id="a6821-246">![Správa uživatelů](./media/active-directory-saas-work-com-tutorial/IC784369.png "Správa uživatelů")</span><span class="sxs-lookup"><span data-stu-id="a6821-246">![Manage Users](./media/active-directory-saas-work-com-tutorial/IC784369.png "Manage Users")</span></span>

4. <span data-ttu-id="a6821-247">Klikněte na tlačítko **nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="a6821-247">Click **New User**.</span></span>
   
    <span data-ttu-id="a6821-248">![Všichni uživatelé](./media/active-directory-saas-work-com-tutorial/IC794117.png "všichni uživatelé")</span><span class="sxs-lookup"><span data-stu-id="a6821-248">![All Users](./media/active-directory-saas-work-com-tutorial/IC794117.png "All Users")</span></span>

5. <span data-ttu-id="a6821-249">V hello uživatele upravte část, provádět hello následující kroky v atributech platný Azure AD účtu chcete tooprovision do hello související textových polí:</span><span class="sxs-lookup"><span data-stu-id="a6821-249">In hello User Edit section, perform hello following steps, in attributes of a valid Azure AD account you want tooprovision into hello related textboxes:</span></span>
   
    <span data-ttu-id="a6821-250">![Úprava uživatele](./media/active-directory-saas-work-com-tutorial/ic794118.png "Úprava uživatele")</span><span class="sxs-lookup"><span data-stu-id="a6821-250">![User Edit](./media/active-directory-saas-work-com-tutorial/ic794118.png "User Edit")</span></span>
   
    <span data-ttu-id="a6821-251">a.</span><span class="sxs-lookup"><span data-stu-id="a6821-251">a.</span></span> <span data-ttu-id="a6821-252">V hello **křestní jméno** textovému poli, typ hello **křestní jméno** hello uživatele **Britta**.</span><span class="sxs-lookup"><span data-stu-id="a6821-252">In hello **First Name** textbox, type hello **first name** of hello user **Britta**.</span></span>
    
    <span data-ttu-id="a6821-253">b.</span><span class="sxs-lookup"><span data-stu-id="a6821-253">b.</span></span> <span data-ttu-id="a6821-254">V hello **příjmení** textovému poli, typ hello **příjmení** hello uživatele **Simon**.</span><span class="sxs-lookup"><span data-stu-id="a6821-254">In hello **Last Name** textbox, type hello **last name** of hello user **Simon**.</span></span>
    
    <span data-ttu-id="a6821-255">c.</span><span class="sxs-lookup"><span data-stu-id="a6821-255">c.</span></span> <span data-ttu-id="a6821-256">V hello **Alias** textovému poli, typ hello **název** hello uživatele **BrittaS**.</span><span class="sxs-lookup"><span data-stu-id="a6821-256">In hello **Alias** textbox, type hello **name** of hello user **BrittaS**.</span></span>
    
    <span data-ttu-id="a6821-257">d.</span><span class="sxs-lookup"><span data-stu-id="a6821-257">d.</span></span> <span data-ttu-id="a6821-258">V hello **e-mailu** textovému poli, typ hello **e-mailová adresa** uživatele  **Brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="a6821-258">In hello **Email** textbox, type hello **email address** of user **Brittasimon@contoso.com**.</span></span>
    
    <span data-ttu-id="a6821-259">e.</span><span class="sxs-lookup"><span data-stu-id="a6821-259">e.</span></span> <span data-ttu-id="a6821-260">V hello **uživatelské jméno** textové pole, zadejte uživatelské jméno uživatele jako  **Brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="a6821-260">In hello **User Name** textbox, type a user name of user like **Brittasimon@contoso.com**.</span></span>
    
    <span data-ttu-id="a6821-261">f.</span><span class="sxs-lookup"><span data-stu-id="a6821-261">f.</span></span> <span data-ttu-id="a6821-262">V hello **Přezdívka** textovému poli, zadejte **Přezdívka** uživatele **Simon**.</span><span class="sxs-lookup"><span data-stu-id="a6821-262">In hello **Nick Name** textbox, type a **nick name** of user **Simon**.</span></span>
    
    <span data-ttu-id="a6821-263">g.</span><span class="sxs-lookup"><span data-stu-id="a6821-263">g.</span></span> <span data-ttu-id="a6821-264">Vyberte **Role**, **uživatelské licence pro**, a **profil**.</span><span class="sxs-lookup"><span data-stu-id="a6821-264">Select **Role**, **User License**, and **Profile**.</span></span>
    
    <span data-ttu-id="a6821-265">h.</span><span class="sxs-lookup"><span data-stu-id="a6821-265">h.</span></span> <span data-ttu-id="a6821-266">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a6821-266">Click **Save**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="a6821-267">Držitel účtu Azure AD Hello dostane e-mailu, včetně účet odkaz tooconfirm hello předtím, než se stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="a6821-267">hello Azure AD account holder will get an email including a link tooconfirm hello account before it becomes active.</span></span>
    > 
    > 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="a6821-268">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="a6821-268">Assign hello Azure AD test user</span></span>

<span data-ttu-id="a6821-269">V této části povolíte tak, že udělíte přístup tooWork.com toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a6821-269">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWork.com.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a6821-271">**tooassign Britta Simon tooWork.com, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a6821-271">**tooassign Britta Simon tooWork.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6821-272">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a6821-272">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a6821-274">V seznamu aplikace hello vyberte **Work.com**.</span><span class="sxs-lookup"><span data-stu-id="a6821-274">In hello applications list, select **Work.com**.</span></span>

    ![Work.com v seznamu aplikace](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_app.png) 

3. <span data-ttu-id="a6821-276">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a6821-276">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a6821-278">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a6821-278">Click **Add** button.</span></span> <span data-ttu-id="a6821-279">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a6821-279">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a6821-281">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="a6821-281">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a6821-282">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a6821-282">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a6821-283">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a6821-283">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="a6821-284">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a6821-284">Test single sign-on</span></span>

<span data-ttu-id="a6821-285">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a6821-285">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a6821-286">Když kliknete na dlaždici Work.com hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Work.com aplikace.</span><span class="sxs-lookup"><span data-stu-id="a6821-286">When you click hello Work.com tile in hello Access Panel, you should get automatically signed-on tooyour Work.com application.</span></span>
<span data-ttu-id="a6821-287">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a6821-287">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a6821-288">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a6821-288">Additional resources</span></span>

* [<span data-ttu-id="a6821-289">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a6821-289">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a6821-290">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a6821-290">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_203.png

