---
title: 'Kurz: Azure Active Directory integrace s FreshDesk | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a FreshDesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 577a5eb6d9b1bc03030a2b47f63d375869c903bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a><span data-ttu-id="5a906-103">Kurz: Azure Active Directory integrace s FreshDesk</span><span class="sxs-lookup"><span data-stu-id="5a906-103">Tutorial: Azure Active Directory integration with FreshDesk</span></span>

<span data-ttu-id="5a906-104">V tomto kurzu zjistíte, jak toointegrate FreshDesk s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5a906-104">In this tutorial, you learn how toointegrate FreshDesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5a906-105">Integrace FreshDesk s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5a906-105">Integrating FreshDesk with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5a906-106">Můžete řídit ve službě Azure AD, který má přístup tooFreshDesk</span><span class="sxs-lookup"><span data-stu-id="5a906-106">You can control in Azure AD who has access tooFreshDesk</span></span>
- <span data-ttu-id="5a906-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooFreshDesk (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5a906-107">You can enable your users tooautomatically get signed-on tooFreshDesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5a906-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="5a906-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="5a906-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5a906-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a906-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5a906-110">Prerequisites</span></span>

<span data-ttu-id="5a906-111">Integrace služby Azure AD s FreshDesk tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5a906-111">tooconfigure Azure AD integration with FreshDesk, you need hello following items:</span></span>

- <span data-ttu-id="5a906-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5a906-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5a906-113">FreshDesk jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5a906-113">A FreshDesk single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5a906-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5a906-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5a906-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5a906-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5a906-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="5a906-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="5a906-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5a906-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5a906-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5a906-118">Scenario description</span></span>
<span data-ttu-id="5a906-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5a906-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5a906-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5a906-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5a906-121">Přidání FreshDesk z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5a906-121">Adding FreshDesk from hello gallery</span></span>
2. <span data-ttu-id="5a906-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5a906-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-freshdesk-from-hello-gallery"></a><span data-ttu-id="5a906-123">Přidání FreshDesk z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5a906-123">Adding FreshDesk from hello gallery</span></span>
<span data-ttu-id="5a906-124">tooconfigure hello integrace FreshDesk do Azure AD, je nutné tooadd FreshDesk hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5a906-124">tooconfigure hello integration of FreshDesk into Azure AD, you need tooadd FreshDesk from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5a906-125">**tooadd FreshDesk z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5a906-125">**tooadd FreshDesk from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5a906-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5a906-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5a906-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5a906-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5a906-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5a906-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5a906-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5a906-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5a906-133">Hello vyhledávacího pole zadejte **FreshDesk**.</span><span class="sxs-lookup"><span data-stu-id="5a906-133">In hello search box, type **FreshDesk**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_search.png)

5. <span data-ttu-id="5a906-135">Na panelu výsledků hello vyberte **FreshDesk**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5a906-135">In hello results panel, select **FreshDesk**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5a906-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5a906-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5a906-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s FreshDesk podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="5a906-138">In this section, you configure and test Azure AD single sign-on with FreshDesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5a906-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v FreshDesk je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5a906-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FreshDesk is tooa user in Azure AD.</span></span> <span data-ttu-id="5a906-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v FreshDesk musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="5a906-140">In other words, a link relationship between an Azure AD user and hello related user in FreshDesk needs toobe established.</span></span>

<span data-ttu-id="5a906-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="5a906-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in FreshDesk.</span></span>

<span data-ttu-id="5a906-142">tooconfigure a testu Azure AD jednotné přihlašování s FreshDesk, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5a906-142">tooconfigure and test Azure AD single sign-on with FreshDesk, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5a906-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5a906-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5a906-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5a906-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5a906-145">**[Vytvoření zkušebního uživatele FreshDesk](#creating-a-freshdesk-test-user)**  -toohave protějšek Britta Simon v FreshDesk, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5a906-145">**[Creating a FreshDesk test user](#creating-a-freshdesk-test-user)** - toohave a counterpart of Britta Simon in FreshDesk that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="5a906-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5a906-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5a906-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="5a906-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5a906-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5a906-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5a906-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="5a906-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your FreshDesk application.</span></span>

<span data-ttu-id="5a906-150">**tooconfigure Azure AD jednotné přihlašování s FreshDesk, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5a906-150">**tooconfigure Azure AD single sign-on with FreshDesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="5a906-151">V hello Azure Management portal na hello **FreshDesk** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5a906-151">In hello Azure Management portal, on hello **FreshDesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5a906-153">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="5a906-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

3. <span data-ttu-id="5a906-155">Na hello **FreshDesk domény a adresy URL** prosím zadejte hello **přihlašovací adresa URL** jako: `https://<tenant-name>.freshdesk.com` nebo jakoukoli jinou hodnotu Freshdesk navrhl.</span><span class="sxs-lookup"><span data-stu-id="5a906-155">On hello **FreshDesk Domain and URLs** section, please enter hello **Sign-on URL** as: `https://<tenant-name>.freshdesk.com` or any other value Freshdesk has suggested.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_url.png)

    > [!NOTE] 
    > <span data-ttu-id="5a906-157">Upozorňujeme, že se nejedná skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5a906-157">Please note that this is not the real value.</span></span> <span data-ttu-id="5a906-158">Budete muset aktualizovat hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5a906-158">You have to update the value with the actual Sign-on URL.</span></span> <span data-ttu-id="5a906-159">Obraťte se na [tým podpory FreshDesk klienta](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5a906-159">Contact [FreshDesk Client support team](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) to get this value.</span></span>  

4. <span data-ttu-id="5a906-160">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte certifikát hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5a906-160">On hello **SAML Signing Certificate** section, click **Certificate** and then save hello certificate on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_certificate.png) 

5. <span data-ttu-id="5a906-162">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5a906-162">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5a906-164">Na hello **FreshDesk konfigurace** klikněte na tlačítko **konfigurace FreshDesk** tooopen přihlašování konfigurace okna.</span><span class="sxs-lookup"><span data-stu-id="5a906-164">On hello **FreshDesk Configuration** section, click **Configure FreshDesk** tooopen Configure sign-on window.</span></span> <span data-ttu-id="5a906-165">Zkopírujte hello SAML jednu přihlašování služby adresu URL a adresy URL Sign-Out z hello **Stručná referenční příručka** části.</span><span class="sxs-lookup"><span data-stu-id="5a906-165">Copy hello SAML Single Sign-On Service URL and Sign-Out URL from hello **Quick Reference** section.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_configure.png)

7. <span data-ttu-id="5a906-167">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Freshdesk.</span><span class="sxs-lookup"><span data-stu-id="5a906-167">In a different web browser window, log into your Freshdesk company site as an administrator.</span></span>

8. <span data-ttu-id="5a906-168">V nabídce hello hello nahoře, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="5a906-168">In hello menu on hello top, click **Admin**.</span></span>
   
   <span data-ttu-id="5a906-169">![Správce](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "správce")</span><span class="sxs-lookup"><span data-stu-id="5a906-169">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Admin")</span></span>

9. <span data-ttu-id="5a906-170">V hello **obecné nastavení** , klikněte na **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="5a906-170">In hello **General Settings** tab, click **Security**.</span></span>
   
   <span data-ttu-id="5a906-171">![Zabezpečení](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="5a906-171">![Security](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Security")</span></span>

10. <span data-ttu-id="5a906-172">V hello **zabezpečení** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5a906-172">In hello **Security** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="5a906-173">![Jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5a906-173">![Single Sign On](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "Single Sign On")</span></span>
   
    <span data-ttu-id="5a906-174">a.</span><span class="sxs-lookup"><span data-stu-id="5a906-174">a.</span></span> <span data-ttu-id="5a906-175">Pro **jednotné přihlašování na (SSO)**, vyberte **na**.</span><span class="sxs-lookup"><span data-stu-id="5a906-175">For **Single Sign On (SSO)**, select **On**.</span></span>

    <span data-ttu-id="5a906-176">b.</span><span class="sxs-lookup"><span data-stu-id="5a906-176">b.</span></span> <span data-ttu-id="5a906-177">Vyberte **jednotné přihlašování SAML**.</span><span class="sxs-lookup"><span data-stu-id="5a906-177">Select **SAML SSO**.</span></span>

    <span data-ttu-id="5a906-178">c.</span><span class="sxs-lookup"><span data-stu-id="5a906-178">c.</span></span> <span data-ttu-id="5a906-179">Typ hello **SAML jeden přihlašování adresa URL služby** jste zkopírovali z portálu Azure do hello **SAML přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5a906-179">Type hello **SAML Single Sign-On Service URL** you copied from Azure portal into hello **SAML Login URL** textbox.</span></span>

    <span data-ttu-id="5a906-180">d.</span><span class="sxs-lookup"><span data-stu-id="5a906-180">d.</span></span> <span data-ttu-id="5a906-181">Typ hello **Sign-Out URL** jste zkopírovali z portálu Azure do hello **adresy URL odhlašovací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5a906-181">Type hello **Sign-Out URL**  you copied from Azure portal into hello **Logout URL** textbox.</span></span>

    <span data-ttu-id="5a906-182">e.</span><span class="sxs-lookup"><span data-stu-id="5a906-182">e.</span></span> <span data-ttu-id="5a906-183">Kopírování hello **kryptografický otisk** z certifikátu hello stáhli z portálu Azure a vložte ji do hello **otisků certifikátu zabezpečení** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5a906-183">Copy hello **Thumbprint** value from hello downloaded certificate from Azure portal and paste it into hello **Security Certificate Fingerprint** textbox.</span></span>  
 
    >[!TIP]
    ><span data-ttu-id="5a906-184">Další podrobnosti najdete v tématu [jak tooretrieve hodnota kryptografického otisku certifikátu](http://youtu.be/YKQF266SAxI).</span><span class="sxs-lookup"><span data-stu-id="5a906-184">For more details, see [How tooretrieve a certificate's thumbprint value](http://youtu.be/YKQF266SAxI).</span></span> 
    
    <span data-ttu-id="5a906-185">f.</span><span class="sxs-lookup"><span data-stu-id="5a906-185">f.</span></span> <span data-ttu-id="5a906-186">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5a906-186">Click **Save**.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5a906-187">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5a906-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="5a906-188">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5a906-188">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5a906-190">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5a906-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5a906-191">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5a906-191">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5a906-193">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5a906-193">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5a906-195">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5a906-195">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5a906-197">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5a906-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5a906-199">a.</span><span class="sxs-lookup"><span data-stu-id="5a906-199">a.</span></span> <span data-ttu-id="5a906-200">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5a906-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5a906-201">b.</span><span class="sxs-lookup"><span data-stu-id="5a906-201">b.</span></span> <span data-ttu-id="5a906-202">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5a906-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5a906-203">c.</span><span class="sxs-lookup"><span data-stu-id="5a906-203">c.</span></span> <span data-ttu-id="5a906-204">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5a906-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5a906-205">d.</span><span class="sxs-lookup"><span data-stu-id="5a906-205">d.</span></span> <span data-ttu-id="5a906-206">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5a906-206">Click **Create**.</span></span>
 
### <a name="creating-a-freshdesk-test-user"></a><span data-ttu-id="5a906-207">Vytvoření zkušebního uživatele FreshDesk</span><span class="sxs-lookup"><span data-stu-id="5a906-207">Creating a FreshDesk test user</span></span>

<span data-ttu-id="5a906-208">V pořadí tooenable Azure AD Uživatelé toolog do FreshDesk musí být zřízená do FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="5a906-208">In order tooenable Azure AD users toolog into FreshDesk, they must be provisioned into FreshDesk.</span></span>  
<span data-ttu-id="5a906-209">V případě hello FreshDesk zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="5a906-209">In hello case of FreshDesk, provisioning is a manual task.</span></span>

<span data-ttu-id="5a906-210">**tooprovision uživatelské účty, provádět hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5a906-210">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="5a906-211">Přihlaste se tooyour **Freshdesk** klienta.</span><span class="sxs-lookup"><span data-stu-id="5a906-211">Log in tooyour **Freshdesk** tenant.</span></span>
2. <span data-ttu-id="5a906-212">V nabídce hello hello nahoře, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="5a906-212">In hello menu on hello top, click **Admin**.</span></span>
   
   <span data-ttu-id="5a906-213">![Správce](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "správce")</span><span class="sxs-lookup"><span data-stu-id="5a906-213">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Admin")</span></span>

3. <span data-ttu-id="5a906-214">V hello **obecné nastavení** , klikněte na **agenti**.</span><span class="sxs-lookup"><span data-stu-id="5a906-214">In hello **General Settings** tab, click **Agents**.</span></span>
   
   <span data-ttu-id="5a906-215">![Agenti](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "agentů")</span><span class="sxs-lookup"><span data-stu-id="5a906-215">![Agents](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Agents")</span></span>

4. <span data-ttu-id="5a906-216">Klikněte na tlačítko **nového agenta**.</span><span class="sxs-lookup"><span data-stu-id="5a906-216">Click **New Agent**.</span></span>
   
    <span data-ttu-id="5a906-217">![Nový Agent](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "nového agenta.")</span><span class="sxs-lookup"><span data-stu-id="5a906-217">![New Agent](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "New Agent")</span></span>

5. <span data-ttu-id="5a906-218">V dialogovém okně informace o agentovi hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5a906-218">On hello Agent Information dialog, perform hello following steps:</span></span>
   
   <span data-ttu-id="5a906-219">![Informace o agentovi](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "informace o agentovi")</span><span class="sxs-lookup"><span data-stu-id="5a906-219">![Agent Information](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Agent Information")</span></span>
   
   <span data-ttu-id="5a906-220">a.</span><span class="sxs-lookup"><span data-stu-id="5a906-220">a.</span></span> <span data-ttu-id="5a906-221">V hello **úplný název** textovému poli, název typu hello účtu hello Azure AD má tooprovision.</span><span class="sxs-lookup"><span data-stu-id="5a906-221">In hello **Full Name** textbox, type hello name of hello Azure AD account you want tooprovision.</span></span>

   <span data-ttu-id="5a906-222">b.</span><span class="sxs-lookup"><span data-stu-id="5a906-222">b.</span></span> <span data-ttu-id="5a906-223">V hello **e-mailu** textovému poli, typ hello Azure AD e-mailová adresa účtu hello Azure AD má tooprovision.</span><span class="sxs-lookup"><span data-stu-id="5a906-223">In hello **Email** textbox, type hello Azure AD email address of hello Azure AD account you want tooprovision.</span></span>

   <span data-ttu-id="5a906-224">c.</span><span class="sxs-lookup"><span data-stu-id="5a906-224">c.</span></span> <span data-ttu-id="5a906-225">V hello **název** textovému poli, název typu hello účtu hello Azure AD má tooprovision.</span><span class="sxs-lookup"><span data-stu-id="5a906-225">In hello **Title** textbox, type hello title of hello Azure AD account you want tooprovision.</span></span>

   <span data-ttu-id="5a906-226">d.</span><span class="sxs-lookup"><span data-stu-id="5a906-226">d.</span></span> <span data-ttu-id="5a906-227">Vyberte **agenti role**a potom klikněte na **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="5a906-227">Select **Agents role**, and then click **Assign**.</span></span>
       
   <span data-ttu-id="5a906-228">e.</span><span class="sxs-lookup"><span data-stu-id="5a906-228">e.</span></span> <span data-ttu-id="5a906-229">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5a906-229">Click **Save**.</span></span>     
   
    >[!NOTE]
    ><span data-ttu-id="5a906-230">Držitel účtu Azure AD Hello dostane e-mailu, který zahrnuje účet odkaz tooconfirm hello předtím, než se aktivuje.</span><span class="sxs-lookup"><span data-stu-id="5a906-230">hello Azure AD account holder will get an email that includes a link tooconfirm hello account before it is activated.</span></span> 
    > 
    
    >[!NOTE]
    ><span data-ttu-id="5a906-231">Můžete použít všechny ostatní Freshdesk uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Freshdesk tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="5a906-231">You can use any other Freshdesk user account creation tools or APIs provided by Freshdesk tooprovision AAD user accounts.</span></span> <span data-ttu-id="5a906-232">tooFreshDesk.</span><span class="sxs-lookup"><span data-stu-id="5a906-232">tooFreshDesk.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5a906-233">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5a906-233">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5a906-234">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooBox svůj přístup.</span><span class="sxs-lookup"><span data-stu-id="5a906-234">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooBox.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5a906-236">**tooassign Britta Simon tooFreshDesk, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5a906-236">**tooassign Britta Simon tooFreshDesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="5a906-237">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5a906-237">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5a906-239">V seznamu aplikace hello vyberte **FreshDesk**.</span><span class="sxs-lookup"><span data-stu-id="5a906-239">In hello applications list, select **FreshDesk**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_app.png) 

3. <span data-ttu-id="5a906-241">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5a906-241">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5a906-243">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5a906-243">Click **Add** button.</span></span> <span data-ttu-id="5a906-244">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5a906-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5a906-246">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="5a906-246">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5a906-247">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5a906-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5a906-248">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5a906-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5a906-249">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5a906-249">Testing single sign-on</span></span>

<span data-ttu-id="5a906-250">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5a906-250">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5a906-251">Když kliknete na dlaždici FreshDesk hello v hello přístupového panelu, měli byste obdržet přihlašovací stránky tooget přihlášeného tooyour FreshDesk aplikace.</span><span class="sxs-lookup"><span data-stu-id="5a906-251">When you click hello FreshDesk tile in hello Access Panel, you should get login page tooget signed-on tooyour FreshDesk application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5a906-252">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5a906-252">Additional resources</span></span>

* [<span data-ttu-id="5a906-253">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5a906-253">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5a906-254">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5a906-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_203.png

