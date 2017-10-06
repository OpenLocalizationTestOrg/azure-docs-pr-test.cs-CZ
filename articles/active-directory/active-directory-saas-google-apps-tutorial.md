---
title: 'Kurz: Azure Active Directory integrace s Google Apps v Azure | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Google Apps."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 2093b5ab605ec0d7bbefe7a78e1eede34d756f53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-google-apps"></a><span data-ttu-id="cd0e3-103">Kurz: Azure Active Directory integrace s Google Apps</span><span class="sxs-lookup"><span data-stu-id="cd0e3-103">Tutorial: Azure Active Directory integration with Google Apps</span></span>

<span data-ttu-id="cd0e3-104">V tomto kurzu zjistíte, jak toointegrate Google Apps v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cd0e3-104">In this tutorial, you learn how toointegrate Google Apps with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cd0e3-105">Integrace s Azure AD Google Apps poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="cd0e3-105">Integrating Google Apps with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cd0e3-106">Můžete řídit ve službě Azure AD, který má přístup tooGoogle aplikace</span><span class="sxs-lookup"><span data-stu-id="cd0e3-106">You can control in Azure AD who has access tooGoogle Apps</span></span>
- <span data-ttu-id="cd0e3-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooGoogle aplikace (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="cd0e3-107">You can enable your users tooautomatically get signed-on tooGoogle Apps (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cd0e3-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cd0e3-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="cd0e3-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cd0e3-109">If you want tooknow more information about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd0e3-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cd0e3-110">Prerequisites</span></span>

<span data-ttu-id="cd0e3-111">tooconfigure integrace Azure AD s Google Apps, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="cd0e3-111">tooconfigure Azure AD integration with Google Apps, you need hello following items:</span></span>

- <span data-ttu-id="cd0e3-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="cd0e3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cd0e3-113">Google Apps jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="cd0e3-113">A Google Apps single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cd0e3-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cd0e3-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="cd0e3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cd0e3-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cd0e3-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cd0e3-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="video-tutorial"></a><span data-ttu-id="cd0e3-118">Videokurz</span><span class="sxs-lookup"><span data-stu-id="cd0e3-118">Video tutorial</span></span>
<span data-ttu-id="cd0e3-119">Jak tooEnable jednotné přihlašování tooGoogle aplikace během 2 minut:</span><span class="sxs-lookup"><span data-stu-id="cd0e3-119">How tooEnable Single Sign-On tooGoogle Apps in 2 Minutes:</span></span>

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enable-single-sign-on-to-Google-Apps-in-2-minutes-with-Azure-AD/player]

## <a name="frequently-asked-questions"></a><span data-ttu-id="cd0e3-120">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="cd0e3-120">Frequently Asked Questions</span></span>
1. <span data-ttu-id="cd0e3-121">**Otázka: je Chromebooks a dalších zařízení Chrome kompatibilní s Azure AD jednotné přihlašování?**</span><span class="sxs-lookup"><span data-stu-id="cd0e3-121">**Q: Are Chromebooks and other Chrome devices compatible with Azure AD single sign-on?**</span></span>
   
    <span data-ttu-id="cd0e3-122">Odpověď: Ano, jsou uživatelé moct toosign do jejich zařízení Chromebook pomocí svých přihlašovacích údajů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-122">A: Yes, users are able toosign into their Chromebook devices using their Azure AD credentials.</span></span> <span data-ttu-id="cd0e3-123">Toto [Google Apps podporují článku](https://support.google.com/chrome/a/answer/6060880) informace o důvod, proč může získat uživatelé vyzváni k zadání pověření dvakrát.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-123">See this [Google Apps support article](https://support.google.com/chrome/a/answer/6060880) for information on why users may get prompted for credentials twice.</span></span>

2. <span data-ttu-id="cd0e3-124">**Otázka:-li povolit jednotné přihlašování, uživatelé budou mít možnost toouse jejich toosign přihlašovací údaje Azure AD do jakékoli Google produkt, například Google učebny, GMail, Google Drive, YouTube a tak dále?**</span><span class="sxs-lookup"><span data-stu-id="cd0e3-124">**Q: If I enable single sign-on, will users be able toouse their Azure AD credentials toosign into any Google product, such as Google Classroom, GMail, Google Drive, YouTube, and so on?**</span></span>
   
    <span data-ttu-id="cd0e3-125">Odpověď: Ano, v závislosti na [které Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) zvolte tooenable nebo zakázat pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-125">A: Yes, depending on [which Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) you choose tooenable or disable for your organization.</span></span>

3. <span data-ttu-id="cd0e3-126">**Otázka: je možné povolit jednotné přihlašování pro pouze podmnožinu Moji uživatelé Google Apps?**</span><span class="sxs-lookup"><span data-stu-id="cd0e3-126">**Q: Can I enable single sign-on for only a subset of my Google Apps users?**</span></span>
   
    <span data-ttu-id="cd0e3-127">Odpověď: Ne, zapnout jednotné přihlašování okamžitě vyžaduje všechny tooauthenticate uživatelé vaší Google Apps pomocí svých přihlašovacích údajů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-127">A: No, turning on single sign-on immediately requires all your Google Apps users tooauthenticate with their Azure AD credentials.</span></span> <span data-ttu-id="cd0e3-128">Protože Google Apps nepodporuje, s více poskytovatelů identit, hello zprostředkovatele identity pro vaše prostředí Google Apps může být buď Azure AD nebo Google – ale ne oba v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-128">Because Google Apps doesn't support having multiple identity providers, hello identity provider for your Google Apps environment can either be Azure AD or Google -- but not both at hello same time.</span></span>

4. <span data-ttu-id="cd0e3-129">**Otázka: Pokud je uživatel přihlášený prostřednictvím systému Windows, jsou že automaticky ověřují tooGoogle aplikace bez získávání výzva k zadání hesla?**</span><span class="sxs-lookup"><span data-stu-id="cd0e3-129">**Q: If a user is signed in through Windows, are they automatically authenticate tooGoogle Apps without getting prompted for a password?**</span></span>
   
    <span data-ttu-id="cd0e3-130">Odpověď: existují dvě možnosti pro povolení tohoto scénáře.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-130">A: There are two options for enabling this scenario.</span></span> <span data-ttu-id="cd0e3-131">Nejdřív by uživatelé přihlašovat do zařízení s Windows 10 prostřednictvím [Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cd0e3-131">First, users could sign into Windows 10 devices via [Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span></span> <span data-ttu-id="cd0e3-132">Alternativně může uživatelé přihlašovat do zařízení s Windows, které jsou připojené k doméně tooan místní služby Active Directory, která byla povolená pro jeden přihlašování tooAzure AD prostřednictvím [Active Directory Federation Services (AD FS)](active-directory-aadconnect-user-signin.md) nasazení.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-132">Alternatively, users could sign into Windows devices that are domain-joined tooan on-premises Active Directory that has been enabled for single sign-on tooAzure AD via an [Active Directory Federation Services (AD FS)](active-directory-aadconnect-user-signin.md) deployment.</span></span> <span data-ttu-id="cd0e3-133">Obě možnosti vyžadovat tooperform hello kroky v následujícím kurzu tooenable jednotné přihlašování mezi službou Azure AD hello a Google Apps.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-133">Both options require you tooperform hello steps in hello following tutorial tooenable single sign-on between Azure AD and Google Apps.</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cd0e3-134">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="cd0e3-134">Scenario description</span></span>
<span data-ttu-id="cd0e3-135">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-135">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cd0e3-136">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="cd0e3-136">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cd0e3-137">Přidání Google Apps z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cd0e3-137">Adding Google Apps from hello gallery</span></span>
2. <span data-ttu-id="cd0e3-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cd0e3-138">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-google-apps-from-hello-gallery"></a><span data-ttu-id="cd0e3-139">Přidání Google Apps z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cd0e3-139">Adding Google Apps from hello gallery</span></span>
<span data-ttu-id="cd0e3-140">tooconfigure hello integrace Google Apps do Azure AD, je nutné tooadd Google Apps hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-140">tooconfigure hello integration of Google Apps into Azure AD, you need tooadd Google Apps from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cd0e3-141">**tooadd Google Apps z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cd0e3-141">**tooadd Google Apps from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd0e3-142">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-142">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cd0e3-144">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-144">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cd0e3-145">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-145">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="cd0e3-147">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-147">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="cd0e3-149">Hello vyhledávacího pole zadejte **Google Apps**.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-149">In hello search box, type **Google Apps**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_search.png)

5. <span data-ttu-id="cd0e3-151">Na panelu výsledků hello vyberte **Google Apps**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-151">In hello results panel, select **Google Apps**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cd0e3-153">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cd0e3-153">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cd0e3-154">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Google Apps podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="cd0e3-154">In this section, you configure and test Azure AD single sign-on with Google Apps based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="cd0e3-155">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Google Apps je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-155">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Google Apps is tooa user in Azure AD.</span></span> <span data-ttu-id="cd0e3-156">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Google Apps musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-156">In other words, a link relationship between an Azure AD user and hello related user in Google Apps needs toobe established.</span></span>

<span data-ttu-id="cd0e3-157">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Google Apps.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-157">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Google Apps.</span></span>

<span data-ttu-id="cd0e3-158">tooconfigure a testu Azure AD jednotné přihlašování s Google Apps, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="cd0e3-158">tooconfigure and test Azure AD single sign-on with Google Apps, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cd0e3-159">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-159">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cd0e3-160">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-160">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cd0e3-161">**[Vytvoření zkušebního uživatele Google Apps](#creating-a-google-apps-test-user)**  -toohave protějšek Britta Simon v Google Apps, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-161">**[Creating a Google Apps test user](#creating-a-google-apps-test-user)** - toohave a counterpart of Britta Simon in Google Apps that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="cd0e3-162">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-162">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cd0e3-163">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-163">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cd0e3-164">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cd0e3-164">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cd0e3-165">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Google Apps.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-165">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Google Apps application.</span></span>

<span data-ttu-id="cd0e3-166">**tooconfigure Azure AD jednotné přihlašování s Google Apps, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cd0e3-166">**tooconfigure Azure AD single sign-on with Google Apps, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd0e3-167">V portálu Azure, na hello hello **Google Apps** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-167">In hello Azure portal, on hello **Google Apps** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="cd0e3-169">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-169">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_samlbase.png)

3. <span data-ttu-id="cd0e3-171">Na hello **Google Apps domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="cd0e3-171">On hello **Google Apps Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_url.png)

    <span data-ttu-id="cd0e3-173">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://mail.google.com/a/<yourdomain>`</span><span class="sxs-lookup"><span data-stu-id="cd0e3-173">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://mail.google.com/a/<yourdomain>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cd0e3-174">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-174">This value is not real.</span></span> <span data-ttu-id="cd0e3-175">Aktualizujte hodnotu hello s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-175">Update hello value with hello actual Sign-on URL.</span></span> <span data-ttu-id="cd0e3-176">Obraťte se na hello [tým podpory Google](https://www.google.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="cd0e3-176">contact hello [Google support team](https://www.google.com/contact/).</span></span>
 
4. <span data-ttu-id="cd0e3-177">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte certifikát hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-177">On hello **SAML Signing Certificate** section, click **Certificate** and then save hello certificate on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_certificate.png) 

5. <span data-ttu-id="cd0e3-179">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-179">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cd0e3-181">Na hello **Google Apps konfigurace** klikněte na tlačítko **konfigurace Google Apps** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-181">On hello **Google Apps Configuration** section, click **Configure Google Apps** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="cd0e3-182">Kopírování hello **Sign-Out adresu URL, SAML jeden přihlašování adresa URL služby a změnit heslo URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="cd0e3-182">Copy hello **Sign-Out URL, SAML Single Sign-On Service URL and Change password URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_configure.png) 

7. <span data-ttu-id="cd0e3-184">V prohlížeči otevřete novou kartu a přihlaste se k hello [konzoly pro správu aplikace Google](http://admin.google.com/) pomocí účtu správce.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-184">Open a new tab in your browser, and sign into hello [Google Apps Admin Console](http://admin.google.com/) using your administrator account.</span></span>

8. <span data-ttu-id="cd0e3-185">Klikněte na tlačítko **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-185">Click **Security**.</span></span> <span data-ttu-id="cd0e3-186">Pokud nevidíte odkaz hello, mohou být skryty pod hello **více ovládacích prvků** nabídky v hello dolní části obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-186">If you don't see hello link, it may be hidden under hello **More Controls** menu at hello bottom of hello screen.</span></span>
   
    ![Klikněte na Zabezpečení.][10]

9. <span data-ttu-id="cd0e3-188">Na hello **zabezpečení** klikněte na tlačítko **nastavit jednotné přihlašování (SSO).**</span><span class="sxs-lookup"><span data-stu-id="cd0e3-188">On hello **Security** page, click **Set up single sign-on (SSO).**</span></span>
   
    ![Klikněte na tlačítko jednotné přihlašování.][11]

10. <span data-ttu-id="cd0e3-190">Proveďte následující změny konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="cd0e3-190">Perform hello following configuration changes:</span></span>
   
    ![Konfigurace jednotného přihlašování][12]
   
    <span data-ttu-id="cd0e3-192">a.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-192">a.</span></span> <span data-ttu-id="cd0e3-193">Vyberte **nastavení jednotného přihlašování pomocí zprostředkovatele identity jiných výrobců**.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-193">Select **Setup SSO with third-party identity provider**.</span></span>

    <span data-ttu-id="cd0e3-194">b.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-194">b.</span></span> <span data-ttu-id="cd0e3-195">V **přihlašovací adresa URL stránky** pole v Google Apps, vložte hodnotu hello **jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-195">In the **Sign-in page URL** field in Google Apps, paste hello value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="cd0e3-196">c.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-196">c.</span></span> <span data-ttu-id="cd0e3-197">V hello **adresy URL odhlašovací stránky** pole v Google Apps, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-197">In hello **Sign-out page URL** field in Google Apps, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="cd0e3-198">d.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-198">d.</span></span> <span data-ttu-id="cd0e3-199">V hello **změnit heslo URL** pole v Google Apps, vložte hodnotu hello **změnit heslo URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-199">In hello **Change password URL** field in Google Apps, paste hello value of **Change password URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="cd0e3-200">e.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-200">e.</span></span> <span data-ttu-id="cd0e3-201">V Google Apps pro hello **ověřovací certifikát**, nahrávání hello certifikátu, kterou jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-201">In Google Apps, for hello **Verification certificate**, upload hello certificate that you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="cd0e3-202">f.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-202">f.</span></span> <span data-ttu-id="cd0e3-203">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-203">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="cd0e3-204">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="cd0e3-204">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="cd0e3-205">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-205">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="cd0e3-206">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cd0e3-206">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cd0e3-207">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cd0e3-207">Creating an Azure AD test user</span></span>
<span data-ttu-id="cd0e3-208">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-208">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="cd0e3-210">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cd0e3-210">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd0e3-211">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-211">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cd0e3-213">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-213">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cd0e3-215">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-215">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cd0e3-217">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cd0e3-217">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cd0e3-219">a.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-219">a.</span></span> <span data-ttu-id="cd0e3-220">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-220">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cd0e3-221">b.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-221">b.</span></span> <span data-ttu-id="cd0e3-222">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-222">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cd0e3-223">c.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-223">c.</span></span> <span data-ttu-id="cd0e3-224">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-224">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cd0e3-225">d.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-225">d.</span></span> <span data-ttu-id="cd0e3-226">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-226">Click **Create**.</span></span>
 
### <a name="creating-a-google-apps-test-user"></a><span data-ttu-id="cd0e3-227">Vytvoření zkušebního uživatele Google Apps</span><span class="sxs-lookup"><span data-stu-id="cd0e3-227">Creating a Google Apps test user</span></span>

<span data-ttu-id="cd0e3-228">Hello cílem této části je toocreate uživatel volal Britta Simon v Google Apps softwaru.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-228">hello objective of this section is toocreate a user called Britta Simon in Google Apps Software.</span></span> <span data-ttu-id="cd0e3-229">Google Apps podporuje automatické zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-229">Google Apps supports auto provisioning, which is by default enabled.</span></span> <span data-ttu-id="cd0e3-230">Neexistuje žádná akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-230">There is no action for you in this section.</span></span> <span data-ttu-id="cd0e3-231">Pokud uživatel ještě neexistuje v Google Apps softwaru, novou vytvoří při pokusíte tooaccess Google Apps softwaru.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-231">If a user doesn't already exist in Google Apps Software, a new one is created when you attempt tooaccess Google Apps Software.</span></span>

>[!NOTE] 
><span data-ttu-id="cd0e3-232">Pokud potřebujete toocreate uživatel ručně, obraťte se na hello [tým podpory Google](https://www.google.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="cd0e3-232">If you need toocreate a user manually, contact hello [Google support team](https://www.google.com/contact/).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cd0e3-233">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="cd0e3-233">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cd0e3-234">V této části povolíte Britta Simon toouse Azure jednotného přihlašování k udělení přístupu tooGoogle aplikace.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-234">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooGoogle Apps.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="cd0e3-236">**tooassign Britta Simon tooGoogle aplikace, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cd0e3-236">**tooassign Britta Simon tooGoogle Apps, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd0e3-237">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-237">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="cd0e3-239">V seznamu aplikace hello vyberte **Google Apps**.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-239">In hello applications list, select **Google Apps**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_app.png) 

3. <span data-ttu-id="cd0e3-241">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-241">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="cd0e3-243">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-243">Click **Add** button.</span></span> <span data-ttu-id="cd0e3-244">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="cd0e3-246">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-246">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cd0e3-247">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cd0e3-248">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cd0e3-249">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cd0e3-249">Testing single sign-on</span></span>

<span data-ttu-id="cd0e3-250">V této části tootest jeden přihlašování nastavení, otevřete hello přístupovému panelu na adrese [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md), pak se přihlaste do hello testovací účet a klikněte na tlačítko **Google Apps** dlaždici v hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="cd0e3-250">In this section, tootest your single sign-on settings, open hello Access Panel at [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md), then sign into hello test account, and click **Google Apps** tile in hello Access Panel.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cd0e3-251">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cd0e3-251">Additional resources</span></span>

* [<span data-ttu-id="cd0e3-252">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cd0e3-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cd0e3-253">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cd0e3-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="cd0e3-254">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="cd0e3-254">Configure User Provisioning</span></span>](active-directory-saas-google-apps-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_203.png

[0]: ./media/active-directory-saas-google-apps-tutorial/azure-active-directory.png

[5]: ./media/active-directory-saas-google-apps-tutorial/gapps-added.png
[6]: ./media/active-directory-saas-google-apps-tutorial/config-sso.png
[7]: ./media/active-directory-saas-google-apps-tutorial/sso-gapps.png
[8]: ./media/active-directory-saas-google-apps-tutorial/sso-url.png
[9]: ./media/active-directory-saas-google-apps-tutorial/download-cert.png
[10]: ./media/active-directory-saas-google-apps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-google-apps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-config.png
[13]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-confirm.png
[14]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-email.png
[15]: ./media/active-directory-saas-google-apps-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-tutorial/gapps-api-enabled.png
[17]: ./media/active-directory-saas-google-apps-tutorial/add-custom-domain.png
[18]: ./media/active-directory-saas-google-apps-tutorial/specify-domain.png
[19]: ./media/active-directory-saas-google-apps-tutorial/verify-domain.png
[20]: ./media/active-directory-saas-google-apps-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-another.png
[23]: ./media/active-directory-saas-google-apps-tutorial/apps-gapps.png
[24]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-tutorial/gapps-auth.png
[29]: ./media/active-directory-saas-google-apps-tutorial/assign-users.png
[30]: ./media/active-directory-saas-google-apps-tutorial/assign-confirm.png