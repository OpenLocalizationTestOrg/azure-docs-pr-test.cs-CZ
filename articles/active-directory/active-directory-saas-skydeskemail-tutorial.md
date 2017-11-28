---
title: 'Kurz: Azure Active Directory integrace s e-mailu SkyDesk | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SkyDesk e-mailu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 19c670a60f581a2be55b74eacdb5393a36e38be3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a><span data-ttu-id="8ec9e-103">Kurz: Azure Active Directory integrace s SkyDesk e-mailu</span><span class="sxs-lookup"><span data-stu-id="8ec9e-103">Tutorial: Azure Active Directory integration with SkyDesk Email</span></span>

<span data-ttu-id="8ec9e-104">V tomto kurzu zjistíte, jak toointegrate SkyDesk e-mailu s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8ec9e-104">In this tutorial, you learn how toointegrate SkyDesk Email with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8ec9e-105">Integrace SkyDesk e-mailu s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8ec9e-105">Integrating SkyDesk Email with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8ec9e-106">Můžete řídit ve službě Azure AD, který má přístup tooSkyDesk e-mailu</span><span class="sxs-lookup"><span data-stu-id="8ec9e-106">You can control in Azure AD who has access tooSkyDesk Email</span></span>
- <span data-ttu-id="8ec9e-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSkyDesk e-mailu (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ec9e-107">You can enable your users tooautomatically get signed-on tooSkyDesk Email (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8ec9e-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8ec9e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8ec9e-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8ec9e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ec9e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8ec9e-110">Prerequisites</span></span>

<span data-ttu-id="8ec9e-111">tooconfigure integrace Azure AD s SkyDesk e-mailu, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="8ec9e-111">tooconfigure Azure AD integration with SkyDesk Email, you need hello following items:</span></span>

- <span data-ttu-id="8ec9e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ec9e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8ec9e-113">E-mailu SkyDesk jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8ec9e-113">A SkyDesk Email single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8ec9e-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8ec9e-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8ec9e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8ec9e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8ec9e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8ec9e-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8ec9e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8ec9e-118">Scenario description</span></span>
<span data-ttu-id="8ec9e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8ec9e-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8ec9e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8ec9e-121">Přidání e-mailu SkyDesk z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8ec9e-121">Adding SkyDesk Email from hello gallery</span></span>
2. <span data-ttu-id="8ec9e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ec9e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-skydesk-email-from-hello-gallery"></a><span data-ttu-id="8ec9e-123">Přidání e-mailu SkyDesk z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8ec9e-123">Adding SkyDesk Email from hello gallery</span></span>
<span data-ttu-id="8ec9e-124">tooconfigure hello integraci e-mailů SkyDesk do služby Azure AD, je nutné tooadd SkyDesk e-mailu z hello Galerie tooyour seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-124">tooconfigure hello integration of SkyDesk Email into Azure AD, you need tooadd SkyDesk Email from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8ec9e-125">**tooadd SkyDesk e-mailu z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8ec9e-125">**tooadd SkyDesk Email from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8ec9e-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8ec9e-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8ec9e-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="8ec9e-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="8ec9e-133">Hello vyhledávacího pole zadejte **SkyDesk e-mailu**.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-133">In hello search box, type **SkyDesk Email**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_search.png)

5. <span data-ttu-id="8ec9e-135">Na panelu výsledků hello vyberte **e-mailu SkyDesk**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-135">In hello results panel, select **SkyDesk Email**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8ec9e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ec9e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8ec9e-138">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s e-mailu SkyDesk podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="8ec9e-138">In this section, you configure and test Azure AD single sign-on with SkyDesk Email based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8ec9e-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v e-mailu SkyDesk je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SkyDesk Email is tooa user in Azure AD.</span></span> <span data-ttu-id="8ec9e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v e-mailu SkyDesk musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-140">In other words, a link relationship between an Azure AD user and hello related user in SkyDesk Email needs toobe established.</span></span>

<span data-ttu-id="8ec9e-141">V e-mailu SkyDesk, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-141">In SkyDesk Email, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8ec9e-142">tooconfigure a testu Azure AD jednotné přihlašování s SkyDesk e-mailu, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="8ec9e-142">tooconfigure and test Azure AD single sign-on with SkyDesk Email, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8ec9e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8ec9e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8ec9e-145">**[Vytvoření zkušebního uživatele e-mailu SkyDesk](#creating-a-skydesk-email-test-user)**  -toohave protějšek Britta Simon v SkyDesk e-mailu, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-145">**[Creating a SkyDesk Email test user](#creating-a-skydesk-email-test-user)** - toohave a counterpart of Britta Simon in SkyDesk Email that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8ec9e-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8ec9e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8ec9e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ec9e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8ec9e-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci SkyDesk e-mailu.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SkyDesk Email application.</span></span>

<span data-ttu-id="8ec9e-150">**tooconfigure Azure AD jednotné přihlašování s SkyDesk e-mailu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8ec9e-150">**tooconfigure Azure AD single sign-on with SkyDesk Email, perform hello following steps:**</span></span>

1. <span data-ttu-id="8ec9e-151">V portálu Azure, na hello hello **e-mailu SkyDesk** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-151">In hello Azure portal, on hello **SkyDesk Email** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="8ec9e-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_samlbase.png)

3. <span data-ttu-id="8ec9e-155">Na hello **SkyDesk e-mailovou doménu a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8ec9e-155">On hello **SkyDesk Email Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_url.png)

    <span data-ttu-id="8ec9e-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://mail.skydesk.jp/portal/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="8ec9e-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://mail.skydesk.jp/portal/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8ec9e-158">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-158">hello value is not real.</span></span> <span data-ttu-id="8ec9e-159">Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="8ec9e-160">Obraťte se na [tým podpory e-mailového klienta SkyDesk](https://www.skydesk.sg/support/) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-160">Contact [SkyDesk Email Client support team](https://www.skydesk.sg/support/) tooget hello value.</span></span> 
 
4. <span data-ttu-id="8ec9e-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_certificate.png) 

5. <span data-ttu-id="8ec9e-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8ec9e-165">Na hello **SkyDesk e-mailové konfigurace** klikněte na tlačítko **nakonfigurovat e-mailu SkyDesk** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-165">On hello **SkyDesk Email Configuration** section, click **Configure SkyDesk Email** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8ec9e-166">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="8ec9e-166">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_configure.png) 

7. <span data-ttu-id="8ec9e-168">tooenable jednotné přihlašování v **e-mailu SkyDesk**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8ec9e-168">tooenable SSO in **SkyDesk Email**, perform hello following steps:</span></span>

    <span data-ttu-id="8ec9e-169">a.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-169">a.</span></span> <span data-ttu-id="8ec9e-170">Přihlášení tooyour SkyDesk e-mailový účet jako správce.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-170">Sign-on tooyour SkyDesk Email account as administrator.</span></span>

    <span data-ttu-id="8ec9e-171">b.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-171">b.</span></span> <span data-ttu-id="8ec9e-172">V nabídce hello hello nahoře, klikněte na tlačítko **instalace**a vyberte **Org**.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-172">In hello menu on hello top, click **Setup**, and select **Org**.</span></span> 
    
      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
    <span data-ttu-id="8ec9e-174">c.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-174">c.</span></span> <span data-ttu-id="8ec9e-175">Klikněte na **domén** z hello levém panelu.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-175">Click on **Domains** from hello left panel.</span></span>
    
      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    <span data-ttu-id="8ec9e-177">d.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-177">d.</span></span> <span data-ttu-id="8ec9e-178">Klikněte na **přidat doménu**.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-178">Click on **Add Domain**.</span></span>
    
      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    <span data-ttu-id="8ec9e-180">e.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-180">e.</span></span> <span data-ttu-id="8ec9e-181">Zadejte název domény a potom ověřte hello domény.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-181">Enter your Domain name, and then verify hello Domain.</span></span>
    
      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    <span data-ttu-id="8ec9e-183">f.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-183">f.</span></span> <span data-ttu-id="8ec9e-184">Klikněte na **ověřování SAML** z hello levém panelu.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-184">Click on **SAML Authentication** from hello left panel.</span></span>
    
      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

8. <span data-ttu-id="8ec9e-186">Na hello **ověřování SAML** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8ec9e-186">On hello **SAML Authentication** dialog page, perform hello following steps:</span></span>
   
      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)
   
    >[!NOTE]
    ><span data-ttu-id="8ec9e-188">ověřování na základě toouse SAML, musí mít buď **ověřené doméně** nebo **portál URL** instalační program.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-188">toouse SAML based authentication, you should either have **verified domain** or **portal URL** setup.</span></span> <span data-ttu-id="8ec9e-189">Můžete nastavit portál hello adresu URL s jedinečným názvem hello.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-189">You can set hello portal URL with hello unique name.</span></span>
    > 
    > 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    <span data-ttu-id="8ec9e-191">a.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-191">a.</span></span> <span data-ttu-id="8ec9e-192">V hello **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-192">In hello **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="8ec9e-193">b.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-193">b.</span></span> <span data-ttu-id="8ec9e-194">V hello **odhlášení** textovému poli Adresa URL, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-194">In hello **Logout** URL textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="8ec9e-195">c.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-195">c.</span></span> <span data-ttu-id="8ec9e-196">**Změnit heslo URL** je volitelný, nechte pole prázdné.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-196">**Change Password URL** is optional so leave it blank.</span></span>

    <span data-ttu-id="8ec9e-197">d.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-197">d.</span></span> <span data-ttu-id="8ec9e-198">Klikněte na **získat klíč ze souboru** tooselect svůj certifikát stažený z portálu Azure a pak klikněte na tlačítko **otevřete** tooupload hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-198">Click on **Get Key From File** tooselect your downloaded certificate from Azure portal, and then click **Open** tooupload hello certificate.</span></span>

    <span data-ttu-id="8ec9e-199">e.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-199">e.</span></span> <span data-ttu-id="8ec9e-200">Jako **algoritmus**, vyberte **RSA**.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-200">As **Algorithm**, select **RSA**.</span></span>

    <span data-ttu-id="8ec9e-201">f.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-201">f.</span></span> <span data-ttu-id="8ec9e-202">Klikněte na tlačítko **Ok** toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-202">Click **Ok** toosave hello changes.</span></span>

> [!TIP]
> <span data-ttu-id="8ec9e-203">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="8ec9e-203">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8ec9e-204">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-204">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8ec9e-205">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8ec9e-205">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8ec9e-206">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ec9e-206">Creating an Azure AD test user</span></span>
<span data-ttu-id="8ec9e-207">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-207">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="8ec9e-209">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8ec9e-209">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8ec9e-210">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-210">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8ec9e-212">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-212">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8ec9e-214">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-214">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8ec9e-216">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8ec9e-216">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8ec9e-218">a.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-218">a.</span></span> <span data-ttu-id="8ec9e-219">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-219">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8ec9e-220">b.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-220">b.</span></span> <span data-ttu-id="8ec9e-221">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-221">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8ec9e-222">c.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-222">c.</span></span> <span data-ttu-id="8ec9e-223">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-223">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8ec9e-224">d.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-224">d.</span></span> <span data-ttu-id="8ec9e-225">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-225">Click **Create**.</span></span>
 
### <a name="creating-a-skydesk-email-test-user"></a><span data-ttu-id="8ec9e-226">Vytvoření zkušebního uživatele SkyDesk e-mailu</span><span class="sxs-lookup"><span data-stu-id="8ec9e-226">Creating a SkyDesk Email test user</span></span>

<span data-ttu-id="8ec9e-227">V této části vytvoříte uživatele volat Britta Simon SkyDesk e-mailem.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-227">In this section, you create a user called Britta Simon in SkyDesk Email.</span></span>

1. <span data-ttu-id="8ec9e-228">Klikněte na **přístup uživatelů** ze zbývajících hello panelu v e-mailu SkyDesk a pak zadejte svoje uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-228">Click on **User Access** from hello left panel in SkyDesk Email and then enter your username.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)

>[!NOTE] 
><span data-ttu-id="8ec9e-230">Pokud potřebujete toocreate hromadné uživatelů, je nutné toocontact hello [tým podpory SkyDesk e-mailového klienta](https://www.skydesk.sg/support/).</span><span class="sxs-lookup"><span data-stu-id="8ec9e-230">If you need toocreate bulk users, you need toocontact hello [SkyDesk Email Client support team](https://www.skydesk.sg/support/).</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8ec9e-231">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="8ec9e-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8ec9e-232">V této části povolíte tak, že udělíte přístup tooSkyDesk e-mailu Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSkyDesk Email.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="8ec9e-234">**tooassign Britta Simon tooSkyDesk e-mailu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8ec9e-234">**tooassign Britta Simon tooSkyDesk Email, perform hello following steps:**</span></span>

1. <span data-ttu-id="8ec9e-235">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8ec9e-237">V seznamu aplikace hello vyberte **SkyDesk e-mailu**.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-237">In hello applications list, select **SkyDesk Email**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_app.png) 

3. <span data-ttu-id="8ec9e-239">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="8ec9e-241">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-241">Click **Add** button.</span></span> <span data-ttu-id="8ec9e-242">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="8ec9e-244">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8ec9e-245">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8ec9e-246">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8ec9e-247">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ec9e-247">Testing single sign-on</span></span>

<span data-ttu-id="8ec9e-248">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-248">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="8ec9e-249">Po kliknutí na tlačítko hello e-mailu SkyDesk dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour SkyDesk e-mailové aplikace.</span><span class="sxs-lookup"><span data-stu-id="8ec9e-249">When you click hello SkyDesk Email tile in hello Access Panel, you should get automatically signed-on tooyour SkyDesk Email application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8ec9e-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8ec9e-250">Additional resources</span></span>

* [<span data-ttu-id="8ec9e-251">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8ec9e-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8ec9e-252">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8ec9e-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png

