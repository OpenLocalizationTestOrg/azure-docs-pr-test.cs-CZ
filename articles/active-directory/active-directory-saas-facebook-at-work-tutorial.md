---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook. | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a síti na pracovišti ve službě Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: jeedes
ms.openlocfilehash: fd19b3f178a2aee7dd2f204d6d3cf6df8fe6b444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a><span data-ttu-id="15b1e-103">Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="15b1e-103">Tutorial: Azure Active Directory integration with Workplace by Facebook</span></span>

<span data-ttu-id="15b1e-104">V tomto kurzu zjistíte, jak toointegrate síti na pracovišti ve službě Facebook se službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="15b1e-104">In this tutorial, you learn how toointegrate Workplace by Facebook with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="15b1e-105">Integrace síti na pracovišti ve službě Facebook s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="15b1e-105">Integrating Workplace by Facebook with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="15b1e-106">Můžete ovládat ve službě Azure AD, který má přístup tooWorkplace ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="15b1e-106">You can control in Azure AD who has access tooWorkplace by Facebook.</span></span>
- <span data-ttu-id="15b1e-107">Můžete povolit uživatelům tooautomatically získat podepsaný na tooWorkplace Facebook (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="15b1e-107">You can enable your users tooautomatically get signed on tooWorkplace by Facebook (single sign-on) with their Azure AD accounts.</span></span>
- <span data-ttu-id="15b1e-108">Můžete spravovat vaše účty v jednom centrálním místě: hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="15b1e-108">You can manage your accounts in one central location: hello Azure portal.</span></span>

<span data-ttu-id="15b1e-109">Další podrobnosti o softwaru, služba (SaaS) aplikace integraci s Azure AD najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="15b1e-109">For more details about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15b1e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="15b1e-110">Prerequisites</span></span>

<span data-ttu-id="15b1e-111">tooconfigure integrace Azure AD se na pracovišti ve službě Facebook, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="15b1e-111">tooconfigure Azure AD integration with Workplace by Facebook, you need hello following items:</span></span>

- <span data-ttu-id="15b1e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="15b1e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="15b1e-113">Předplatné povolené firemní síti pomocí sítě Facebook jednotné přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="15b1e-113">A Workplace by Facebook single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="15b1e-114">tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="15b1e-114">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="15b1e-115">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="15b1e-115">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="15b1e-116">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="15b1e-116">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="15b1e-117">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="15b1e-117">Scenario description</span></span>
<span data-ttu-id="15b1e-118">V tomto kurzu Azure AD jednotného přihlašování k testování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="15b1e-118">In this tutorial, you test Azure AD SSO in a test environment.</span></span> <span data-ttu-id="15b1e-119">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="15b1e-119">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="15b1e-120">Přidejte síti na pracovišti ve službě Facebook z Galerie hello.</span><span class="sxs-lookup"><span data-stu-id="15b1e-120">Add Workplace by Facebook from hello gallery.</span></span>
2. <span data-ttu-id="15b1e-121">Konfigurace a otestování Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="15b1e-121">Configure and test Azure AD single sign-on.</span></span>

## <a name="add-workplace-by-facebook-from-hello-gallery"></a><span data-ttu-id="15b1e-122">Přidat pracovní ploše ve službě Facebook z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="15b1e-122">Add Workplace by Facebook from hello gallery</span></span>
<span data-ttu-id="15b1e-123">tooconfigure hello integrace síti na pracovišti ve službě Facebook do Azure AD, přidejte síti na pracovišti ve službě Facebook hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="15b1e-123">tooconfigure hello integration of Workplace by Facebook into Azure AD, add Workplace by Facebook from hello gallery tooyour list of managed SaaS apps.</span></span>

1. <span data-ttu-id="15b1e-124">V hello [portál Azure](https://portal.azure.com), v levém podokně text hello, vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-124">In hello [Azure portal](https://portal.azure.com), in hello left pane, select **Azure Active Directory**.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="15b1e-126">Procházet příliš**podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-126">Browse too**Enterprise applications** > **All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="15b1e-128">tooadd hello novou aplikaci, vyberte **novou aplikaci** hello nahoře dialogového okna hello.</span><span class="sxs-lookup"><span data-stu-id="15b1e-128">tooadd hello new application, select **New application** on hello top of hello dialog box.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="15b1e-130">Hello vyhledávacího pole zadejte **síti na pracovišti ve službě Facebook**a vyberte **síti na pracovišti ve službě Facebook** z výsledků.</span><span class="sxs-lookup"><span data-stu-id="15b1e-130">In hello search box, type **Workplace by Facebook**, and select **Workplace by Facebook** from results.</span></span> <span data-ttu-id="15b1e-131">Potom vyberte **přidat**, tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="15b1e-131">Then select **Add**, tooadd hello application.</span></span>

    ![Síti na pracovišti ve službě Facebook v seznamu výsledků hello](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_search.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="15b1e-133">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="15b1e-133">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="15b1e-134">V této části můžete nakonfigurovat a otestovat Azure AD přihlášení SSO se síti na pracovišti ve službě Facebook, podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="15b1e-134">In this section, you configure and test Azure AD SSO with Workplace by Facebook, based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="15b1e-135">Azure AD musí pro jednotné přihlašování toowork tooknow hello příslušného uživatele v síti na pracovišti ve službě Facebook se tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="15b1e-135">For SSO toowork, Azure AD needs tooknow what hello counterpart user in Workplace by Facebook is tooa user in Azure AD.</span></span> <span data-ttu-id="15b1e-136">Jinými slovy je třeba vytvořit propojené vztah mezi uživatele Azure AD a související uživatelské hello v síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="15b1e-136">In other words, a linked relationship between an Azure AD user and hello related user in Workplace by Facebook should be established.</span></span>

<span data-ttu-id="15b1e-137">Vytvořit tento vztah přiřazením hello hodnotu hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="15b1e-137">Establish this relationship by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Workplace by Facebook.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="15b1e-138">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="15b1e-138">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="15b1e-139">V této části Povolení jednotného přihlašování Azure AD v hello portál Azure a nakonfigurovat jednotné přihlašování na vašem pracovišti aplikace Facebook.</span><span class="sxs-lookup"><span data-stu-id="15b1e-139">In this section, you enable Azure AD SSO in hello Azure portal, and you configure SSO in your Workplace by Facebook application.</span></span>

1. <span data-ttu-id="15b1e-140">V portálu Azure, na hello hello **síti na pracovišti ve službě Facebook** stránky integrace aplikací, vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-140">In hello Azure portal, on hello **Workplace by Facebook** application integration page, select **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="15b1e-142">V hello **jednotného přihlašování** dialogové okno, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="15b1e-142">In hello **Single sign-on** dialog box, select **Mode** as **SAML-based Sign-on** tooenable SSO.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. <span data-ttu-id="15b1e-144">V hello **síti na pracovišti Facebook domény a adresy URL** část, hello následující:</span><span class="sxs-lookup"><span data-stu-id="15b1e-144">In hello **Workplace by Facebook Domain and URLs** section, do hello following:</span></span>

    <span data-ttu-id="15b1e-145">a.</span><span class="sxs-lookup"><span data-stu-id="15b1e-145">a.</span></span> <span data-ttu-id="15b1e-146">V hello **přihlašovací adresa URL** textového pole zadejte adresu URL, která používá následující vzor hello:`https://<company subdomain>.facebook.com`</span><span class="sxs-lookup"><span data-stu-id="15b1e-146">In hello **Sign-on URL** text box, type a URL that uses hello following pattern: `https://<company subdomain>.facebook.com`</span></span>

    <span data-ttu-id="15b1e-147">b.</span><span class="sxs-lookup"><span data-stu-id="15b1e-147">b.</span></span> <span data-ttu-id="15b1e-148">V hello **identifikátor** textového pole zadejte adresu URL, která používá následující vzor hello:`https://www.facebook.com/company/<scim company id>`</span><span class="sxs-lookup"><span data-stu-id="15b1e-148">In hello **Identifier** text box, type a URL that uses hello following pattern: `https://www.facebook.com/company/<scim company id>`</span></span>

    > [!NOTE]
    > <span data-ttu-id="15b1e-149">Tyto hodnoty jsou pouze příklad.</span><span class="sxs-lookup"><span data-stu-id="15b1e-149">These values are an example only.</span></span> <span data-ttu-id="15b1e-150">Tyto hodnoty aktualizujte hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="15b1e-150">Update these values with hello actual sign-on URL and identifier.</span></span> <span data-ttu-id="15b1e-151">Kontaktujte hello [síti na pracovišti tým podpory klienta sítě Facebook](https://workplace.fb.com/faq/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="15b1e-151">Contact hello [Workplace by Facebook Client support team](https://workplace.fb.com/faq/) tooget these values.</span></span> 

4. <span data-ttu-id="15b1e-152">V hello **SAML podpisový certifikát** vyberte **certifikátu (Base64)**a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="15b1e-152">In hello **SAML Signing Certificate** section, select **Certificate (Base64)**, and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. <span data-ttu-id="15b1e-154">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-154">Select **Save**.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="15b1e-156">V hello **síti na pracovišti v konfiguraci sítě Facebook** vyberte **pracoviště nakonfigurovat ve službě Facebook** tooopen hello **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="15b1e-156">In hello **Workplace by Facebook Configuration** section, select **Configure Workplace by Facebook** tooopen hello **Configure sign-on** window.</span></span> <span data-ttu-id="15b1e-157">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka** části.</span><span class="sxs-lookup"><span data-stu-id="15b1e-157">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference** section.</span></span>

    ![Síti na pracovišti v konfiguraci sítě Facebook](./media/active-directory-saas-facebook-at-work-tutorial/config.png) 

7. <span data-ttu-id="15b1e-159">V okně prohlížeče jiný web přihlaste jako správce tooyour síti na pracovišti lokalitou společnosti Facebook.</span><span class="sxs-lookup"><span data-stu-id="15b1e-159">In a different web browser window, sign in tooyour Workplace by Facebook company site as an administrator.</span></span>
  
   > [!NOTE] 
   > <span data-ttu-id="15b1e-160">Jako součást hello procesu ověřování SAML síti na pracovišti, můžete použít řetězce dotazu až too2.5 kilobajtů velikostí v pořadí toopass parametry tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="15b1e-160">As part of hello SAML authentication process, Workplace can use query strings of up too2.5 kilobytes in size in order toopass parameters tooAzure AD.</span></span>

8. <span data-ttu-id="15b1e-161">V hello **řídicí panel společnosti**, přejděte toohello **ověřování** kartě.</span><span class="sxs-lookup"><span data-stu-id="15b1e-161">In hello **Company Dashboard**, go toohello **Authentication** tab.</span></span>

9. <span data-ttu-id="15b1e-162">V části **ověřování SAML**, vyberte **pouze jednotné přihlašování** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="15b1e-162">Under **SAML Authentication**, select **SSO Only** from hello drop-down list.</span></span>

10. <span data-ttu-id="15b1e-163">Zadejte hodnoty hello zkopírovaných z hello **síti na pracovišti v konfiguraci sítě Facebook** části hello portálu Azure do odpovídajícího pole hello:</span><span class="sxs-lookup"><span data-stu-id="15b1e-163">Enter hello values copied from hello **Workplace by Facebook Configuration** section of hello Azure portal into hello corresponding fields:</span></span>

    *   <span data-ttu-id="15b1e-164">V **SAML URL** textového pole, vložte hodnotu hello **jeden přihlašování adresa URL služby**, který jste zkopírovali z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="15b1e-164">In the **SAML URL** text box, paste hello value of **Single Sign-On Service URL**, which you have copied from hello Azure portal.</span></span>
    *   <span data-ttu-id="15b1e-165">V **URL vystavitele SAML** textového pole, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="15b1e-165">In the **SAML Issuer URL** text box, paste hello value of **SAML Entity ID**, which you have copied from hello Azure portal.</span></span>
    *   <span data-ttu-id="15b1e-166">V **SAML odhlášení přesměrovat (volitelné)**, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="15b1e-166">In **SAML Logout Redirect (optional)**, paste hello value of **Sign-Out URL**, which you have copied from hello Azure portal.</span></span>
    *   <span data-ttu-id="15b1e-167">Otevřete váš **certifikátu s kódováním base-64** v poznámkovém bloku stáhli z portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="15b1e-167">Open your **base-64 encoded certificate** in Notepad, downloaded from hello Azure portal.</span></span> <span data-ttu-id="15b1e-168">Kopírovat obsah hello ho do schránky a pak ji vložit toothe **certifikátu SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="15b1e-168">Copy hello content of it into your clipboard, and then paste it toothe **SAML Certificate** text box.</span></span>

11. <span data-ttu-id="15b1e-169">Může být nutné cílovou skupinu hello tooenter adresu URL, příjemce adresy URL a služby ACS (služba Assertion příjemce) adresy URL, které jsou uvedené v části hello **konfigurace SAML** části.</span><span class="sxs-lookup"><span data-stu-id="15b1e-169">You might need tooenter hello audience URL, recipient URL, and ACS (Assertion Consumer Service) URL, listed under hello **SAML Configuration** section.</span></span>

12. <span data-ttu-id="15b1e-170">Posuňte zobrazení dolní toohello hello oddílu a vyberte **Test jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-170">Scroll toohello bottom of hello section, and select **Test SSO**.</span></span> <span data-ttu-id="15b1e-171">Automaticky otevírané okno se zobrazí s přihlašovací stránku hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="15b1e-171">A pop-up window appears, with hello Azure AD sign-in page.</span></span> <span data-ttu-id="15b1e-172">tooauthenticate, zadejte své přihlašovací údaje jako normální.</span><span class="sxs-lookup"><span data-stu-id="15b1e-172">tooauthenticate, enter your credentials as normal.</span></span> <span data-ttu-id="15b1e-173">Ujistěte se, stejně jako hello pracovní účet, kterým jste přihlášeni s je hello hello e-mailovou adresu, která je vrácena zpět z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="15b1e-173">Ensure hello email address returned back from Azure AD is hello same as hello Workplace account you are logged in with.</span></span>

13. <span data-ttu-id="15b1e-174">Pokud hello testu se úspěšně dokončil, posuňte toohello dolní části stránky hello a vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-174">If hello test has finished successfully, scroll toohello bottom of hello page and select **Save**.</span></span>

14. <span data-ttu-id="15b1e-175">Každý, kdo používá síti na pracovišti se teď zobrazí s Azure AD přihlašovací stránka pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="15b1e-175">Anyone using Workplace is now presented with Azure AD sign-in page for authentication.</span></span>

<span data-ttu-id="15b1e-176">Můžete tooconfigure SAML odhlaste se adresa URL, která lze použít toopoint na odhlášení stránku hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="15b1e-176">You can choose tooconfigure a SAML sign out URL, which can be used toopoint at hello Azure AD sign-out page.</span></span> <span data-ttu-id="15b1e-177">Pokud je toto nastavení povolené a nakonfigurované, hello uživatel je již směrovanou toohello síti na pracovišti odhlašovací stránky.</span><span class="sxs-lookup"><span data-stu-id="15b1e-177">When this setting is enabled and configured, hello user is no longer directed toohello Workplace sign-out page.</span></span> <span data-ttu-id="15b1e-178">Místo toho uživatel hello je přesměrovaného toohello URL, která byla přidána do nastavení odhlášení přesměrování SAML hello.</span><span class="sxs-lookup"><span data-stu-id="15b1e-178">Instead, hello user is redirected toohello URL that was added in hello SAML sign-out redirect setting.</span></span>


> [!TIP]
> <span data-ttu-id="15b1e-179">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="15b1e-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app.</span></span> <span data-ttu-id="15b1e-180">Po přidání této aplikace z hello **služby Active Directory** > **podnikové aplikace, které** jednoduše vyberte hello **jednotné přihlašování** kartě a přístup hello vložené dokumentace prostřednictvím hello **konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="15b1e-180">After adding this app from hello **Active Directory** > **Enterprise Applications** section, simply select hello **Single Sign-On** tab, and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="15b1e-181">Další informace o funkci hello embedded dokumentace v hello [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="15b1e-181">You can read more about hello embedded documentation feature in hello [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="configure-reauthentication-frequency"></a><span data-ttu-id="15b1e-182">Nakonfigurovat četnost opětovné ověření</span><span class="sxs-lookup"><span data-stu-id="15b1e-182">Configure reauthentication frequency</span></span>

<span data-ttu-id="15b1e-183">Můžete nakonfigurovat tooprompt síti na pracovišti pro kontrolu SAML každý den, tři dny jeden týden, dva týdny, jeden měsíc, nebo vůbec.</span><span class="sxs-lookup"><span data-stu-id="15b1e-183">You can configure Workplace tooprompt for a SAML check every day, three days, one week, two weeks, one month, or never.</span></span>

> [!NOTE] 
><span data-ttu-id="15b1e-184">Hello minimální hodnota hello SAML kontroly na mobilní aplikace nastavena tooone týden.</span><span class="sxs-lookup"><span data-stu-id="15b1e-184">hello minimum value for hello SAML check on mobile applications is set tooone week.</span></span>

<span data-ttu-id="15b1e-185">Můžete taky přinutit SAML obnovit pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="15b1e-185">You can also force a SAML reset for all users.</span></span> <span data-ttu-id="15b1e-186">toodo se použití hello **vyžadovat ověřování SAML pro všechny uživatele nyní** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="15b1e-186">toodo this, use hello **Require SAML authentication for all users now** button.</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="15b1e-187">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="15b1e-187">Create an Azure AD test user</span></span>
<span data-ttu-id="15b1e-188">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="15b1e-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

1. <span data-ttu-id="15b1e-190">V hello **portál Azure**, v levém podokně text hello, vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-190">In hello **Azure portal**, in hello left pane, select **Azure Active Directory**.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="15b1e-192">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a vyberte **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-192">toodisplay hello list of users, go too**Users and groups**, and select **All users**.</span></span>
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="15b1e-194">tooopen hello **uživatele** dialogové okno, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-194">tooopen hello **User** dialog box, select **Add**.</span></span>
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="15b1e-196">V hello **uživatele** dialogové okno pole, hello následující:</span><span class="sxs-lookup"><span data-stu-id="15b1e-196">In hello **User** dialog box, do hello following:</span></span>
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="15b1e-198">a.</span><span class="sxs-lookup"><span data-stu-id="15b1e-198">a.</span></span> <span data-ttu-id="15b1e-199">V hello **název** textového pole, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-199">In hello **Name** text box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="15b1e-200">b.</span><span class="sxs-lookup"><span data-stu-id="15b1e-200">b.</span></span> <span data-ttu-id="15b1e-201">V hello **uživatelské jméno** textového pole, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="15b1e-201">In hello **User name** text box, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="15b1e-202">c.</span><span class="sxs-lookup"><span data-stu-id="15b1e-202">c.</span></span> <span data-ttu-id="15b1e-203">Vyberte **zobrazit hesla**a poznamenejte si ho.</span><span class="sxs-lookup"><span data-stu-id="15b1e-203">Select **Show Password**, and write it down.</span></span>

    <span data-ttu-id="15b1e-204">d.</span><span class="sxs-lookup"><span data-stu-id="15b1e-204">d.</span></span> <span data-ttu-id="15b1e-205">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-205">Select **Create**.</span></span>
 
### <a name="create-a-workplace-by-facebook-test-user"></a><span data-ttu-id="15b1e-206">Vytvoření pracovišti Facebook testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="15b1e-206">Create a Workplace by Facebook test user</span></span>

<span data-ttu-id="15b1e-207">V této části uživatele volat Britta Simon vytvoří v síti na pracovišti Facebook.</span><span class="sxs-lookup"><span data-stu-id="15b1e-207">In this section, a user called Britta Simon is created in Workplace by Facebook.</span></span> <span data-ttu-id="15b1e-208">Síti na pracovišti ve službě Facebook podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="15b1e-208">Workplace by Facebook supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="15b1e-209">Neexistuje žádná akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="15b1e-209">There is no action for you in this section.</span></span> <span data-ttu-id="15b1e-210">Pokud uživatel neexistuje v síti na pracovišti ve službě Facebook, je vytvořen nový pokusíte-li tooaccess síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="15b1e-210">If a user doesn't exist in Workplace by Facebook, a new one is created when you attempt tooaccess Workplace by Facebook.</span></span>

>[!Note]
><span data-ttu-id="15b1e-211">Pokud potřebujete toocreate uživatel ručně, obraťte se na hello [síti na pracovišti tým podpory klienta sítě Facebook](https://workplace.fb.com/faq/).</span><span class="sxs-lookup"><span data-stu-id="15b1e-211">If you need toocreate a user manually, contact hello [Workplace by Facebook Client support team](https://workplace.fb.com/faq/).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="15b1e-212">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="15b1e-212">Assign hello Azure AD test user</span></span>

<span data-ttu-id="15b1e-213">V této části povolíte Britta Simon toouse Azure jednotného přihlašování k udělení přístupu tooWorkplace ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="15b1e-213">In this section, you enable Britta Simon toouse Azure SSO by granting access tooWorkplace by Facebook.</span></span>

![Přiřadit uživatele][200] 

1. <span data-ttu-id="15b1e-215">V hello Azure aplikace hello portálu, otevřete zobrazení.</span><span class="sxs-lookup"><span data-stu-id="15b1e-215">In hello Azure portal, open hello applications view.</span></span> <span data-ttu-id="15b1e-216">Přejděte toohello directory zobrazení, přejděte příliš**podnikové aplikace, které**a potom vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-216">Go toohello directory view, go too**Enterprise applications**, and then select **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="15b1e-218">V seznamu aplikace hello vyberte **síti na pracovišti ve službě Facebook**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-218">In hello applications list, select **Workplace by Facebook**.</span></span>

    ![Hello síti na pracovišti podle propojení sítě Facebook v seznamu aplikace hello](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_app.png) 

3. <span data-ttu-id="15b1e-220">V nabídce hello na levé straně hello vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-220">In hello menu on hello left, select **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202] 

4. <span data-ttu-id="15b1e-222">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-222">Select **Add**.</span></span> <span data-ttu-id="15b1e-223">Potom v hello **přidat přiřazení** podokně, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-223">Then, in hello **Add Assignment** pane, select **Users and groups**.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="15b1e-225">V hello **uživatelů a skupin** dialogové okno, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="15b1e-225">In hello **Users and groups** dialog box, select **Britta Simon** in hello users list.</span></span>

6. <span data-ttu-id="15b1e-226">V hello **uživatelů a skupin** dialogové okno, vyberte **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-226">In hello **Users and groups** dialog box, select **Select**.</span></span>

7. <span data-ttu-id="15b1e-227">V hello **přidat přiřazení** dialogové okno, vyberte **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="15b1e-227">In hello **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="15b1e-228">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="15b1e-228">Test single sign-on</span></span>

<span data-ttu-id="15b1e-229">Pokud chcete tootest nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="15b1e-229">If you want tootest your SSO settings, open hello Access Panel.</span></span>
<span data-ttu-id="15b1e-230">Další informace najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="15b1e-230">For more information, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="15b1e-231">Další kroky</span><span class="sxs-lookup"><span data-stu-id="15b1e-231">Next steps</span></span>

* <span data-ttu-id="15b1e-232">V tématu hello [seznam kurzů toointegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="15b1e-232">See hello [list of tutorials on how toointegrate SaaS Apps with Azure Active Directory](active-directory-saas-tutorial-list.md).</span></span>
* <span data-ttu-id="15b1e-233">Čtení [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="15b1e-233">Read [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>
* <span data-ttu-id="15b1e-234">Další informace o příliš[konfiguraci zřizování uživatelů](active-directory-saas-facebook-at-work-provisioning-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="15b1e-234">Learn more about how too[configure user provisioning](active-directory-saas-facebook-at-work-provisioning-tutorial.md).</span></span>



<!--Image references-->

[1]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_203.png

