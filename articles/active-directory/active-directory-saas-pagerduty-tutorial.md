---
title: 'Kurz: Azure Active Directory integrace s PagerDuty | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a PagerDuty."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: c3cfbedac3bf075e2d8cd833d5de7ca0bc9468b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a><span data-ttu-id="b1bac-103">Kurz: Azure Active Directory integrace s PagerDuty</span><span class="sxs-lookup"><span data-stu-id="b1bac-103">Tutorial: Azure Active Directory integration with PagerDuty</span></span>

<span data-ttu-id="b1bac-104">V tomto kurzu zjistíte, jak toointegrate PagerDuty s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b1bac-104">In this tutorial, you learn how toointegrate PagerDuty with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b1bac-105">Integrace PagerDuty s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b1bac-105">Integrating PagerDuty with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b1bac-106">Můžete řídit ve službě Azure AD, který má přístup tooPagerDuty</span><span class="sxs-lookup"><span data-stu-id="b1bac-106">You can control in Azure AD who has access tooPagerDuty</span></span>
- <span data-ttu-id="b1bac-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPagerDuty (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b1bac-107">You can enable your users tooautomatically get signed-on tooPagerDuty (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b1bac-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b1bac-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b1bac-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b1bac-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1bac-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b1bac-110">Prerequisites</span></span>

<span data-ttu-id="b1bac-111">Integrace služby Azure AD s PagerDuty tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b1bac-111">tooconfigure Azure AD integration with PagerDuty, you need hello following items:</span></span>

- <span data-ttu-id="b1bac-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b1bac-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b1bac-113">PagerDuty jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b1bac-113">A PagerDuty single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b1bac-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b1bac-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b1bac-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b1bac-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b1bac-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b1bac-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b1bac-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b1bac-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b1bac-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b1bac-118">Scenario description</span></span>
<span data-ttu-id="b1bac-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b1bac-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b1bac-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b1bac-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b1bac-121">Přidání PagerDuty z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b1bac-121">Adding PagerDuty from hello gallery</span></span>
2. <span data-ttu-id="b1bac-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1bac-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pagerduty-from-hello-gallery"></a><span data-ttu-id="b1bac-123">Přidání PagerDuty z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b1bac-123">Adding PagerDuty from hello gallery</span></span>
<span data-ttu-id="b1bac-124">tooconfigure hello integrace PagerDuty do Azure AD, je nutné tooadd PagerDuty hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b1bac-124">tooconfigure hello integration of PagerDuty into Azure AD, you need tooadd PagerDuty from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b1bac-125">**tooadd PagerDuty z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1bac-125">**tooadd PagerDuty from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1bac-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b1bac-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="b1bac-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b1bac-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="b1bac-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b1bac-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="b1bac-133">Hello vyhledávacího pole zadejte **PagerDuty**, vyberte **PagerDuty** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1bac-133">In hello search box, type **PagerDuty**, select  **PagerDuty**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="b1bac-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1bac-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="b1bac-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s PagerDuty podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="b1bac-136">In this section, you configure and test Azure AD single sign-on with PagerDuty based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b1bac-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v PagerDuty je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b1bac-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in PagerDuty is tooa user in Azure AD.</span></span> <span data-ttu-id="b1bac-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v PagerDuty musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="b1bac-138">In other words, a link relationship between an Azure AD user and hello related user in PagerDuty needs toobe established.</span></span>

<span data-ttu-id="b1bac-139">V PagerDuty, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="b1bac-139">In PagerDuty, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b1bac-140">tooconfigure a testu Azure AD jednotné přihlašování s PagerDuty, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="b1bac-140">tooconfigure and test Azure AD single sign-on with PagerDuty, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b1bac-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b1bac-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b1bac-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b1bac-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b1bac-143">**[Vytvoření zkušebního uživatele PagerDuty](#create-a-pagerduty-test-user)**  -toohave protějšek Britta Simon v PagerDuty, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="b1bac-143">**[Create a PagerDuty test user](#create-a-pagerduty-test-user)** - toohave a counterpart of Britta Simon in PagerDuty that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b1bac-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b1bac-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b1bac-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="b1bac-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="b1bac-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1bac-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="b1bac-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="b1bac-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your PagerDuty application.</span></span>

<span data-ttu-id="b1bac-148">**tooconfigure Azure AD jednotné přihlašování s PagerDuty, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1bac-148">**tooconfigure Azure AD single sign-on with PagerDuty, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1bac-149">V portálu Azure, na hello hello **PagerDuty** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-149">In hello Azure portal, on hello **PagerDuty** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="b1bac-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b1bac-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_samlbase.png)

3. <span data-ttu-id="b1bac-153">Na hello **PagerDuty domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b1bac-153">On hello **PagerDuty Domain and URLs** section, perform hello following steps:</span></span>

    ![PagerDuty domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_url.png)

    <span data-ttu-id="b1bac-155">a.</span><span class="sxs-lookup"><span data-stu-id="b1bac-155">a.</span></span> <span data-ttu-id="b1bac-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.pagerduty.com`</span><span class="sxs-lookup"><span data-stu-id="b1bac-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.pagerduty.com`</span></span>

    <span data-ttu-id="b1bac-157">b.</span><span class="sxs-lookup"><span data-stu-id="b1bac-157">b.</span></span> <span data-ttu-id="b1bac-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.pagerduty.com`</span><span class="sxs-lookup"><span data-stu-id="b1bac-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenant-name>.pagerduty.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b1bac-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="b1bac-159">These values are not real.</span></span> <span data-ttu-id="b1bac-160">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="b1bac-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b1bac-161">Obraťte se na [tým podpory PagerDuty klienta](https://www.pagerduty.com/support/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b1bac-161">Contact [PagerDuty Client support team](https://www.pagerduty.com/support/) tooget these values.</span></span> 

4. <span data-ttu-id="b1bac-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b1bac-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_certificate.png) 

5. <span data-ttu-id="b1bac-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b1bac-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-pagerduty-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b1bac-166">Na hello **PagerDuty konfigurace** klikněte na tlačítko **konfigurace PagerDuty** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="b1bac-166">On hello **PagerDuty Configuration** section, click **Configure PagerDuty** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b1bac-167">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="b1bac-167">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace PagerDuty](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_configure.png) 

7. <span data-ttu-id="b1bac-169">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Pagerduty.</span><span class="sxs-lookup"><span data-stu-id="b1bac-169">In a different web browser window, log into your Pagerduty company site as an administrator.</span></span>

8. <span data-ttu-id="b1bac-170">V nabídce hello hello nahoře, klikněte na tlačítko **nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-170">In hello menu on hello top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="b1bac-171">![Nastavení účtu](./media/active-directory-saas-pagerduty-tutorial/ic778535.png "nastavení účtu")</span><span class="sxs-lookup"><span data-stu-id="b1bac-171">![Account Settings](./media/active-directory-saas-pagerduty-tutorial/ic778535.png "Account Settings")</span></span>

9. <span data-ttu-id="b1bac-172">Klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-172">Click **Single Sign-on**.</span></span>
   
    <span data-ttu-id="b1bac-173">![Jednotné přihlašování](./media/active-directory-saas-pagerduty-tutorial/ic778536.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="b1bac-173">![Single sign-on](./media/active-directory-saas-pagerduty-tutorial/ic778536.png "Single sign-on")</span></span>

10. <span data-ttu-id="b1bac-174">Na hello **povolit jednotné přihlašování (SSO)** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b1bac-174">On hello **Enable Single Sign-on (SSO)** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="b1bac-175">![Povolit jednotné přihlašování](./media/active-directory-saas-pagerduty-tutorial/ic778537.png "povolit jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="b1bac-175">![Enable single sign-on](./media/active-directory-saas-pagerduty-tutorial/ic778537.png "Enable single sign-on")</span></span>
   
    <span data-ttu-id="b1bac-176">a.</span><span class="sxs-lookup"><span data-stu-id="b1bac-176">a.</span></span> <span data-ttu-id="b1bac-177">Otevření kódovaného certifikátu kódování base-64 stáhli z portálu Azure v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát X.509** textbox</span><span class="sxs-lookup"><span data-stu-id="b1bac-177">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox</span></span>
  
    <span data-ttu-id="b1bac-178">b.</span><span class="sxs-lookup"><span data-stu-id="b1bac-178">b.</span></span> <span data-ttu-id="b1bac-179">V hello **přihlašovací adresa URL** textovému poli, vložte **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b1bac-179">In hello **Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="b1bac-180">c.</span><span class="sxs-lookup"><span data-stu-id="b1bac-180">c.</span></span> <span data-ttu-id="b1bac-181">V hello **adresy URL odhlašovací** textovému poli, vložte **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b1bac-181">In hello **Logout URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="b1bac-182">d.</span><span class="sxs-lookup"><span data-stu-id="b1bac-182">d.</span></span> <span data-ttu-id="b1bac-183">Vyberte **zapnout Single Sign-on**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-183">Select **Turn on Single Sign-on**.</span></span>
 
    <span data-ttu-id="b1bac-184">e.</span><span class="sxs-lookup"><span data-stu-id="b1bac-184">e.</span></span> <span data-ttu-id="b1bac-185">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-185">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="b1bac-186">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="b1bac-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b1bac-187">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="b1bac-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b1bac-188">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b1bac-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="b1bac-189">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b1bac-189">Create an Azure AD test user</span></span>

<span data-ttu-id="b1bac-190">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="b1bac-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="b1bac-192">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1bac-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1bac-193">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b1bac-193">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b1bac-195">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-195">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b1bac-197">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b1bac-197">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b1bac-199">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b1bac-199">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b1bac-201">a.</span><span class="sxs-lookup"><span data-stu-id="b1bac-201">a.</span></span> <span data-ttu-id="b1bac-202">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-202">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b1bac-203">b.</span><span class="sxs-lookup"><span data-stu-id="b1bac-203">b.</span></span> <span data-ttu-id="b1bac-204">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b1bac-204">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b1bac-205">c.</span><span class="sxs-lookup"><span data-stu-id="b1bac-205">c.</span></span> <span data-ttu-id="b1bac-206">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-206">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b1bac-207">d.</span><span class="sxs-lookup"><span data-stu-id="b1bac-207">d.</span></span> <span data-ttu-id="b1bac-208">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-208">Click **Create**.</span></span>
 
### <a name="create-a-pagerduty-test-user"></a><span data-ttu-id="b1bac-209">Vytvoření zkušebního uživatele PagerDuty</span><span class="sxs-lookup"><span data-stu-id="b1bac-209">Create a PagerDuty test user</span></span>

<span data-ttu-id="b1bac-210">Uživatelé toolog tooenable Azure AD v tooPagerDuty, se musí být zřízená do PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="b1bac-210">tooenable Azure AD users toolog in tooPagerDuty, they must be provisioned into PagerDuty.</span></span>  
<span data-ttu-id="b1bac-211">V případě hello PagerDuty zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b1bac-211">In hello case of PagerDuty, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="b1bac-212">Můžete použít všechny ostatní Pagerduty uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Pagerduty tooprovision Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="b1bac-212">You can use any other Pagerduty user account creation tools or APIs provided by Pagerduty tooprovision Azure Active Directory user accounts.</span></span>

<span data-ttu-id="b1bac-213">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1bac-213">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1bac-214">Přihlaste se tooyour **Pagerduty** klienta.</span><span class="sxs-lookup"><span data-stu-id="b1bac-214">Log in tooyour **Pagerduty** tenant.</span></span>

2. <span data-ttu-id="b1bac-215">V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-215">In hello menu on hello top, click **Users**.</span></span>

3. <span data-ttu-id="b1bac-216">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-216">Click **Add Users**.</span></span>
   
    <span data-ttu-id="b1bac-217">![Přidání uživatelů](./media/active-directory-saas-pagerduty-tutorial/ic778539.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="b1bac-217">![Add Users](./media/active-directory-saas-pagerduty-tutorial/ic778539.png "Add Users")</span></span>

4.  <span data-ttu-id="b1bac-218">Na hello **pozvat váš tým** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b1bac-218">On hello **Invite your team** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="b1bac-219">![Pozvěte váš tým](./media/active-directory-saas-pagerduty-tutorial/ic778540.png "pozvat váš tým")</span><span class="sxs-lookup"><span data-stu-id="b1bac-219">![Invite your team](./media/active-directory-saas-pagerduty-tutorial/ic778540.png "Invite your team")</span></span>

    <span data-ttu-id="b1bac-220">a.</span><span class="sxs-lookup"><span data-stu-id="b1bac-220">a.</span></span> <span data-ttu-id="b1bac-221">Typ hello **první a poslední název** uživatele jako **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-221">Type hello **First and Last Name** of user like **Britta Simon**.</span></span> 
   
    <span data-ttu-id="b1bac-222">b.</span><span class="sxs-lookup"><span data-stu-id="b1bac-222">b.</span></span> <span data-ttu-id="b1bac-223">Zadejte **e-mailu** adresu uživatele, například  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="b1bac-223">Enter **Email** address of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="b1bac-224">c.</span><span class="sxs-lookup"><span data-stu-id="b1bac-224">c.</span></span> <span data-ttu-id="b1bac-225">Klikněte na tlačítko **přidat**a potom klikněte na **odesílání žádostí**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-225">Click **Add**, and then click **Send Invites**.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="b1bac-226">Všechny přidaní uživatelé dostanou pozvání toocreate PagerDuty účtu.</span><span class="sxs-lookup"><span data-stu-id="b1bac-226">All added users will receive an invite toocreate a PagerDuty account.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="b1bac-227">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b1bac-227">Assign hello Azure AD test user</span></span>

<span data-ttu-id="b1bac-228">V této části povolíte tak, že udělíte přístup tooPagerDuty toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b1bac-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPagerDuty.</span></span>

![Přiřadit role uživatele hello][200]

<span data-ttu-id="b1bac-230">**tooassign Britta Simon tooPagerDuty, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1bac-230">**tooassign Britta Simon tooPagerDuty, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1bac-231">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-231">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b1bac-233">V seznamu aplikace hello vyberte **PagerDuty**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-233">In hello applications list, select **PagerDuty**.</span></span>

    ![v seznamu aplikace hello Hello PagerDuty odkaz](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_app.png) 

3. <span data-ttu-id="b1bac-235">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b1bac-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="b1bac-237">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b1bac-237">Click **Add** button.</span></span> <span data-ttu-id="b1bac-238">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b1bac-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="b1bac-240">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="b1bac-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b1bac-241">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b1bac-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b1bac-242">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b1bac-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="b1bac-243">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1bac-243">Test single sign-on</span></span>

<span data-ttu-id="b1bac-244">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b1bac-244">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b1bac-245">Po kliknutí na tlačítko hello PagerDuty dlaždice v hello přístup Panelyou měli získat automaticky přihlášeného tooyour PagerDuty aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1bac-245">When you click hello PagerDuty tile in hello Access Panelyou should get automatically signed-on tooyour PagerDuty application.</span></span>

<span data-ttu-id="b1bac-246">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b1bac-246">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1bac-247">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b1bac-247">Additional resources</span></span>

* [<span data-ttu-id="b1bac-248">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b1bac-248">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b1bac-249">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b1bac-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_203.png

