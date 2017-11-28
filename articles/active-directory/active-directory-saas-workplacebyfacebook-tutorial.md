---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook. | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a síti na pracovišti ve službě Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f71a59527394730757d501a973251dc293fd3683
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a><span data-ttu-id="cf6d2-103">Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-103">Tutorial: Azure Active Directory integration with Workplace by Facebook</span></span>

<span data-ttu-id="cf6d2-104">V tomto kurzu zjistíte, jak toointegrate síti na pracovišti ve službě Facebook se službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cf6d2-104">In this tutorial, you learn how toointegrate Workplace by Facebook with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cf6d2-105">Integrace síti na pracovišti ve službě Facebook s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="cf6d2-105">Integrating Workplace by Facebook with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cf6d2-106">Můžete řídit ve službě Azure AD, který má přístup tooWorkplace ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-106">You can control in Azure AD who has access tooWorkplace by Facebook</span></span>
- <span data-ttu-id="cf6d2-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWorkplace ve službě Facebook (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="cf6d2-107">You can enable your users tooautomatically get signed-on tooWorkplace by Facebook (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cf6d2-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cf6d2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="cf6d2-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cf6d2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cf6d2-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cf6d2-110">Prerequisites</span></span>

<span data-ttu-id="cf6d2-111">tooconfigure integrace Azure AD se na pracovišti ve službě Facebook, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="cf6d2-111">tooconfigure Azure AD integration with Workplace by Facebook, you need hello following items:</span></span>

- <span data-ttu-id="cf6d2-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="cf6d2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cf6d2-113">Firemní síti pomocí sítě Facebook jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="cf6d2-113">A Workplace by Facebook single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cf6d2-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cf6d2-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="cf6d2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cf6d2-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cf6d2-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cf6d2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cf6d2-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="cf6d2-118">Scenario description</span></span>
<span data-ttu-id="cf6d2-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cf6d2-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="cf6d2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cf6d2-121">Přidání síti na pracovišti ve službě Facebook z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cf6d2-121">Adding Workplace by Facebook from hello gallery</span></span>
2. <span data-ttu-id="cf6d2-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cf6d2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workplace-by-facebook-from-hello-gallery"></a><span data-ttu-id="cf6d2-123">Přidání síti na pracovišti ve službě Facebook z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cf6d2-123">Adding Workplace by Facebook from hello gallery</span></span>
<span data-ttu-id="cf6d2-124">tooconfigure hello integrace síti na pracovišti ve službě Facebook do Azure AD, je nutné tooadd síti na pracovišti ve službě Facebook hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-124">tooconfigure hello integration of Workplace by Facebook into Azure AD, you need tooadd Workplace by Facebook from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cf6d2-125">**tooadd síti na pracovišti ve službě Facebook z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cf6d2-125">**tooadd Workplace by Facebook from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf6d2-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cf6d2-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cf6d2-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="cf6d2-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="cf6d2-133">Hello vyhledávacího pole zadejte **síti na pracovišti ve službě Facebook**.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-133">In hello search box, type **Workplace by Facebook**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

5. <span data-ttu-id="cf6d2-135">Na panelu výsledků hello vyberte **síti na pracovišti ve službě Facebook**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-135">In hello results panel, select **Workplace by Facebook**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cf6d2-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cf6d2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cf6d2-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování se na pracovišti ve službě Facebook podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="cf6d2-138">In this section, you configure and test Azure AD single sign-on with Workplace by Facebook based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="cf6d2-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v síti na pracovišti ve službě Facebook se tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workplace by Facebook is tooa user in Azure AD.</span></span> <span data-ttu-id="cf6d2-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v síti na pracovišti ve službě Facebook musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-140">In other words, a link relationship between an Azure AD user and hello related user in Workplace by Facebook needs toobe established.</span></span>

<span data-ttu-id="cf6d2-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Workplace by Facebook.</span></span>

<span data-ttu-id="cf6d2-142">tooconfigure a testování Azure AD jednotné přihlašování se na pracovišti ve službě Facebook, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="cf6d2-142">tooconfigure and test Azure AD single sign-on with Workplace by Facebook, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cf6d2-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cf6d2-144">**[Konfiguraci frekvence opětovné ověření](#configuring-reauthentication-frequency)**  -tooconfigure tooprompt síti na pracovišti pro kontrolu SAML.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-144">**[Configuring Reauthentication Frequency](#configuring-reauthentication-frequency)** - tooconfigure Workplace tooprompt for a SAML check.</span></span>
3. <span data-ttu-id="cf6d2-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="cf6d2-146">**[Vytváření pracovišti uživatelem testovací Facebook](#creating-a-workplace-by-facebook-test-user)**  -toohave protějšek Britta Simon v síti na pracovišti ve službě Facebook, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-146">**[Creating a Workplace by Facebook test user](#creating-a-workplace-by-facebook-test-user)** - toohave a counterpart of Britta Simon in Workplace by Facebook that is linked toohello Azure AD representation of user.</span></span>
5. <span data-ttu-id="cf6d2-147">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
6. <span data-ttu-id="cf6d2-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cf6d2-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cf6d2-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cf6d2-150">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování na vašem pracovišti aplikace Facebook.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workplace by Facebook application.</span></span>

<span data-ttu-id="cf6d2-151">**tooconfigure Azure AD jednotné přihlašování se na pracovišti ve službě Facebook, provést hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cf6d2-151">**tooconfigure Azure AD single sign-on with Workplace by Facebook, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf6d2-152">V portálu Azure, na hello hello **síti na pracovišti ve službě Facebook** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-152">In hello Azure portal, on hello **Workplace by Facebook** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="cf6d2-154">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. <span data-ttu-id="cf6d2-156">Na hello **síti na pracovišti Facebook domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="cf6d2-156">On hello **Workplace by Facebook Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    <span data-ttu-id="cf6d2-158">a.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-158">a.</span></span> <span data-ttu-id="cf6d2-159">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instancename>.facebook.com`</span><span class="sxs-lookup"><span data-stu-id="cf6d2-159">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<instancename>.facebook.com`</span></span>

    <span data-ttu-id="cf6d2-160">b.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-160">b.</span></span> <span data-ttu-id="cf6d2-161">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.facebook.com/company/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="cf6d2-161">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.facebook.com/company/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cf6d2-162">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-162">These values are not hello real.</span></span> <span data-ttu-id="cf6d2-163">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-163">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="cf6d2-164">Obraťte se na [síti na pracovišti tým podpory klienta sítě Facebook](https://workplace.fb.com/faq/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-164">Contact [Workplace by Facebook Client support team](https://workplace.fb.com/faq/) tooget these values.</span></span> 

4. <span data-ttu-id="cf6d2-165">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-165">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. <span data-ttu-id="cf6d2-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cf6d2-169">Na hello **síti na pracovišti v konfiguraci sítě Facebook** klikněte na tlačítko **pracoviště nakonfigurovat ve službě Facebook** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-169">On hello **Workplace by Facebook Configuration** section, click **Configure Workplace by Facebook** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="cf6d2-170">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="cf6d2-170">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/config.png) 

7. <span data-ttu-id="cf6d2-172">V okně prohlížeče jiný web, přihlášení tooyour síti na pracovišti lokalitou Facebook společnosti jako správce.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-172">In a different web browser window, login tooyour Workplace by Facebook company site as an administrator.</span></span>
  
   > [!NOTE] 
   > <span data-ttu-id="cf6d2-173">Jako součást hello procesu ověřování SAML může využívat síti na pracovišti řetězce dotazu až too2.5 kilobajtů velikostí v pořadí toopass parametry tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-173">As part of hello SAML authentication process, Workplace may utilize query strings of up too2.5 kilobytes in size in order toopass parameters tooAzure AD.</span></span>

8. <span data-ttu-id="cf6d2-174">V hello **řídicí panel společnosti**, přejděte toohello **ověřování** kartě.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-174">In hello **Company Dashboard**, go toohello **Authentication** tab.</span></span>

9. <span data-ttu-id="cf6d2-175">V části **ověřování SAML**, vyberte **pouze jednotné přihlašování** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-175">Under **SAML Authentication**, select **SSO Only** from hello drop-down list.</span></span>

10. <span data-ttu-id="cf6d2-176">Vstupní hello hodnoty zkopírovaných z **síti na pracovišti v konfiguraci sítě Facebook** části hello portálu Azure do odpovídajícího pole hello:</span><span class="sxs-lookup"><span data-stu-id="cf6d2-176">Input hello values copied from **Workplace by Facebook Configuration** section of hello Azure portal into hello corresponding fields:</span></span>

    *   <span data-ttu-id="cf6d2-177">V **SAML URL** textovému poli, vložte hodnotu hello **jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-177">In **SAML URL** textbox, paste hello value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="cf6d2-178">V **URL vystavitele SAML textbox**, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-178">In **SAML Issuer URL textbox**, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="cf6d2-179">V **přesměrování odhlašovací SAML** (volitelné), vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-179">In **SAML Logout Redirect** (Optional), paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="cf6d2-180">Otevřete váš **certifikátu s kódováním base-64** v poznámkovém bloku stáhli z portálu Azure, kopírovat obsah hello ho do schránky a pak ji vložit toothe **certifikátu SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-180">Open your **base-64 encoded certificate** in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toothe **SAML Certificate** textbox.</span></span>

11. <span data-ttu-id="cf6d2-181">Může být nutné tooenter hello cílovou skupinu adresu URL, adresa URL příjemce, a adresa URL služby ACS (služba Assertion příjemce) uvedené v části hello **konfigurace SAML** části.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-181">You may need tooenter hello Audience URL, Recipient URL, and ACS (Assertion Consumer Service) URL listed under hello **SAML Configuration** section.</span></span>

12. <span data-ttu-id="cf6d2-182">Posuňte se toohello dolní části hello a klikněte na tlačítko hello **Test jednotného přihlašování k** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-182">Scroll toohello bottom of hello section and click hello **Test SSO** button.</span></span> <span data-ttu-id="cf6d2-183">Zobrazí se tato výsledky v automaticky otevíraném okně, zobrazí se přihlašovací stránku služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-183">This results in a popup window appearing with Azure AD login page presented.</span></span> <span data-ttu-id="cf6d2-184">Zadejte přihlašovací údaje v jako normální tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-184">Enter your credentials in as normal tooauthenticate.</span></span> 

    <span data-ttu-id="cf6d2-185">**Řešení potíží:** zajistěte, aby hello e-mailová adresa se vrací zpět z Azure AD je hello stejná hodnota jako hello pracovní účet, které jste se přihlásili.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-185">**Troubleshooting:** Ensure hello email address being returned back from Azure AD is hello same as hello Workplace account you are logged in with.</span></span>

13. <span data-ttu-id="cf6d2-186">Po úspěšném dokončení testů hello, posuňte toohello dolní části stránky hello a klikněte na tlačítko hello **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-186">Once hello test has been completed successfully, scroll toohello bottom of hello page and click hello **Save** button.</span></span>

14. <span data-ttu-id="cf6d2-187">Všichni uživatelé používali síti na pracovišti nyní zobrazí se přihlašovací stránku služby Azure AD pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-187">All users using Workplace will now be presented with Azure AD login page for authentication.</span></span>

15. <span data-ttu-id="cf6d2-188">**SAML odhlášení přesměrovat (volitelné)** -</span><span class="sxs-lookup"><span data-stu-id="cf6d2-188">**SAML Logout Redirect (optional)** -</span></span> 

    <span data-ttu-id="cf6d2-189">Můžete zvolit toooptionally postup konfigurace adresy Url odhlašovací SAML, který lze použít toopoint v Azure AD odhlašovací stránce.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-189">You can choose toooptionally configure a SAML Logout Url, which can be used toopoint at Azure AD's logout page.</span></span> <span data-ttu-id="cf6d2-190">Pokud je toto nastavení povolené a nakonfigurované, hello uživatele už nebude směrovanou toohello síti na pracovišti odhlašovací stránce.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-190">When this setting is enabled and configured, hello user will no longer be directed toohello Workplace logout page.</span></span> <span data-ttu-id="cf6d2-191">Místo toho bude uživatel hello přesměrovaného toohello url, která byla přidána do nastavení přesměrování odhlašovací SAML hello.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-191">Instead, hello user will be redirected toohello url that was added in hello SAML Logout Redirect setting.</span></span>


> [!TIP]
> <span data-ttu-id="cf6d2-192">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="cf6d2-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="cf6d2-193">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="cf6d2-194">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cf6d2-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="configuring-reauthentication-frequency"></a><span data-ttu-id="cf6d2-195">Konfiguraci frekvence opětovné ověření</span><span class="sxs-lookup"><span data-stu-id="cf6d2-195">Configuring Reauthentication Frequency</span></span>

<span data-ttu-id="cf6d2-196">Můžete nakonfigurovat tooprompt síti na pracovišti pro kontrolu SAML každý den, tři dny týdne dvou týdnů, měsíců nebo vůbec.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-196">You can configure Workplace tooprompt for a SAML check every day, three days, week, two weeks, month or never.</span></span>

> [!NOTE] 
><span data-ttu-id="cf6d2-197">Hello minimální hodnota hello SAML kontroly na mobilní aplikace nastavena tooone týden.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-197">hello minimum value for hello SAML check on mobile applications is set tooone week.</span></span>

<span data-ttu-id="cf6d2-198">Můžete taky přinutit SAML obnovit pro všechny uživatele pomocí tlačítka hello: vyžadovat ověřování SAML pro všechny uživatele nyní.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-198">You can also force a SAML reset for all users using hello button: Require SAML authentication for all users now.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cf6d2-199">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cf6d2-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="cf6d2-200">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-200">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="cf6d2-202">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cf6d2-202">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf6d2-203">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-203">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cf6d2-205">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-205">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cf6d2-207">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-207">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cf6d2-209">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cf6d2-209">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cf6d2-211">a.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-211">a.</span></span> <span data-ttu-id="cf6d2-212">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-212">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cf6d2-213">b.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-213">b.</span></span> <span data-ttu-id="cf6d2-214">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-214">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cf6d2-215">c.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-215">c.</span></span> <span data-ttu-id="cf6d2-216">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-216">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cf6d2-217">d.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-217">d.</span></span> <span data-ttu-id="cf6d2-218">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-218">Click **Create**.</span></span>
 
### <a name="creating-a-workplace-by-facebook-test-user"></a><span data-ttu-id="cf6d2-219">Vytváření pracovišti Facebook testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="cf6d2-219">Creating a Workplace by Facebook test user</span></span>

<span data-ttu-id="cf6d2-220">V této části uživatele volat Britta Simon vytvoří v síti na pracovišti Facebook.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-220">In this section, a user called Britta Simon is created in Workplace by Facebook.</span></span> <span data-ttu-id="cf6d2-221">Síti na pracovišti ve službě Facebook podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-221">Workplace by Facebook supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="cf6d2-222">Neexistuje žádná akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-222">There is no action for you in this section.</span></span> <span data-ttu-id="cf6d2-223">Pokud uživatel neexistuje v síti na pracovišti ve službě Facebook, je vytvořen nový pokusíte-li tooaccess síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-223">If a user doesn't exist in Workplace by Facebook, a new one is created when you attempt tooaccess Workplace by Facebook.</span></span>

>[!Note]
><span data-ttu-id="cf6d2-224">Pokud potřebujete toocreate uživatel ručně, obraťte se na [síti na pracovišti tým podpory klienta sítě Facebook](https://workplace.fb.com/faq/)</span><span class="sxs-lookup"><span data-stu-id="cf6d2-224">If you need toocreate a user manually, Contact [Workplace by Facebook Client support team](https://workplace.fb.com/faq/)</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cf6d2-225">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="cf6d2-225">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cf6d2-226">V této části je povolit Britta Simon toouse Azure jednotné přihlašování tak, že udělíte přístup tooWorkplace ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkplace by Facebook.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="cf6d2-228">**tooassign tooWorkplace Britta Simon ve službě Facebook, provést hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cf6d2-228">**tooassign Britta Simon tooWorkplace by Facebook, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf6d2-229">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="cf6d2-231">V seznamu aplikace hello vyberte **síti na pracovišti ve službě Facebook**.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-231">In hello applications list, select **Workplace by Facebook**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

3. <span data-ttu-id="cf6d2-233">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="cf6d2-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-235">Click **Add** button.</span></span> <span data-ttu-id="cf6d2-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="cf6d2-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cf6d2-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cf6d2-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cf6d2-241">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cf6d2-241">Testing single sign-on</span></span>

<span data-ttu-id="cf6d2-242">Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="cf6d2-242">If you want tootest your single sign-on settings, open hello Access Panel.</span></span>
<span data-ttu-id="cf6d2-243">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cf6d2-243">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="cf6d2-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cf6d2-244">Additional resources</span></span>

* [<span data-ttu-id="cf6d2-245">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cf6d2-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cf6d2-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cf6d2-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="cf6d2-247">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="cf6d2-247">Configure User Provisioning</span></span>](active-directory-saas-workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_203.png

