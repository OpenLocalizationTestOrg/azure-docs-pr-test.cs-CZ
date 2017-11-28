---
title: "Kurz: Azure Active Directory integrace s Five9 Plus adaptér (CTI, obraťte se na centrum agenty) | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Five9 Plus adaptér (CTI, obraťte se na centrum agenty)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: 2e3ea8b5f2a6eaa8ad17d39e03fa490038b14561
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a><span data-ttu-id="95ad3-103">Kurz: Azure Active Directory integrace s Five9 Plus adaptér (CTI, obraťte se na centrum agentů)</span><span class="sxs-lookup"><span data-stu-id="95ad3-103">Tutorial: Azure Active Directory integration with Five9 Plus Adapter (CTI, Contact Center Agents)</span></span>

<span data-ttu-id="95ad3-104">V tomto kurzu zjistíte, jak toointegrate Five9 Plus adaptér (CTI, obraťte se na centrum agenty) s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="95ad3-104">In this tutorial, you learn how toointegrate Five9 Plus Adapter (CTI, Contact Center Agents) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="95ad3-105">Integrace Five9 Plus adaptér (CTI, obraťte se na centrum agenty) s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="95ad3-105">Integrating Five9 Plus Adapter (CTI, Contact Center Agents) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="95ad3-106">Můžete řídit v Azure AD, který má přístup tooFive9 Plus adaptér (CTI, obraťte se na centrum agentů)</span><span class="sxs-lookup"><span data-stu-id="95ad3-106">You can control in Azure AD who has access tooFive9 Plus Adapter (CTI, Contact Center Agents)</span></span>
- <span data-ttu-id="95ad3-107">Můžete povolit uživatelům tooautomatically get přihlášeného tooFive9 Plus adaptér (CTI, obraťte se na centrum agenty) (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="95ad3-107">You can enable your users tooautomatically get signed-on tooFive9 Plus Adapter (CTI, Contact Center Agents) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="95ad3-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="95ad3-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="95ad3-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="95ad3-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95ad3-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="95ad3-110">Prerequisites</span></span>

<span data-ttu-id="95ad3-111">integrace tooconfigure Azure AD s Five9 Plus adaptérem (CTI, obraťte se na centrum agenty), musíte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="95ad3-111">tooconfigure Azure AD integration with Five9 Plus Adapter (CTI, Contact Center Agents), you need hello following items:</span></span>

- <span data-ttu-id="95ad3-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="95ad3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="95ad3-113">Five9 Plus adaptér (CTI, obraťte se na centrum agenty) jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="95ad3-113">A Five9 Plus Adapter (CTI, Contact Center Agents) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="95ad3-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="95ad3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="95ad3-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="95ad3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="95ad3-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="95ad3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="95ad3-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="95ad3-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="95ad3-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="95ad3-118">Scenario description</span></span>
<span data-ttu-id="95ad3-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="95ad3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="95ad3-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="95ad3-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="95ad3-121">Přidání Five9 Plus adaptér (CTI, obraťte se na centrum agenty) z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="95ad3-121">Adding Five9 Plus Adapter (CTI, Contact Center Agents) from hello gallery</span></span>
2. <span data-ttu-id="95ad3-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="95ad3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-hello-gallery"></a><span data-ttu-id="95ad3-123">Přidání Five9 Plus adaptér (CTI, obraťte se na centrum agenty) z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="95ad3-123">Adding Five9 Plus Adapter (CTI, Contact Center Agents) from hello gallery</span></span>
<span data-ttu-id="95ad3-124">tooconfigure hello integrace Five9 Plus adaptéru (CTI, obraťte se na centrum agenty) do Azure AD, musíte tooadd Five9 Plus adaptér (CTI, obraťte se na centrum agenty) hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="95ad3-124">tooconfigure hello integration of Five9 Plus Adapter (CTI, Contact Center Agents) into Azure AD, you need tooadd Five9 Plus Adapter (CTI, Contact Center Agents) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="95ad3-125">**tooadd Five9 Plus adaptér (CTI, obraťte se na centrum agenty) z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="95ad3-125">**tooadd Five9 Plus Adapter (CTI, Contact Center Agents) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="95ad3-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="95ad3-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="95ad3-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="95ad3-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="95ad3-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="95ad3-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="95ad3-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="95ad3-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="95ad3-133">Hello vyhledávacího pole zadejte **Five9 Plus adaptér (CTI, obraťte se na centrum agenty)**.</span><span class="sxs-lookup"><span data-stu-id="95ad3-133">In hello search box, type **Five9 Plus Adapter (CTI, Contact Center Agents)**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-five9-tutorial/tutorial_five9_search.png)

5. <span data-ttu-id="95ad3-135">Na panelu výsledků hello vyberte **Five9 Plus adaptér (CTI, obraťte se na centrum agenty)**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="95ad3-135">In hello results panel, select **Five9 Plus Adapter (CTI, Contact Center Agents)**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="95ad3-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="95ad3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="95ad3-138">V této části konfiguraci a testování Azure AD jednotné přihlašování s Five9 Plus adaptér (CTI, obraťte se na centrum agenty) na základě testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="95ad3-138">In this section, you configure and test Azure AD single sign-on with Five9 Plus Adapter (CTI, Contact Center Agents) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="95ad3-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Five9 Plus adaptéru (CTI, obraťte se na centrum agenty) je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="95ad3-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Five9 Plus Adapter (CTI, Contact Center Agents) is tooa user in Azure AD.</span></span> <span data-ttu-id="95ad3-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Five9 Plus adaptéru (CTI, obraťte se na centrum agenty) musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="95ad3-140">In other words, a link relationship between an Azure AD user and hello related user in Five9 Plus Adapter (CTI, Contact Center Agents) needs toobe established.</span></span>

<span data-ttu-id="95ad3-141">V Five9 Plus adaptér (CTI, obraťte se na centrum agenty), přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="95ad3-141">In Five9 Plus Adapter (CTI, Contact Center Agents), assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="95ad3-142">tooconfigure a testování Azure AD jednotné přihlašování s Five9 Plus adaptérem (CTI, obraťte se na centrum agenty), musíte toocomplete hello následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="95ad3-142">tooconfigure and test Azure AD single sign-on with Five9 Plus Adapter (CTI, Contact Center Agents), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="95ad3-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="95ad3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="95ad3-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="95ad3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="95ad3-145">**[Vytvoření zkušebního uživatele Five9 Plus adaptér (CTI, obraťte se na centrum agenty)](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)**  -toohave protějšek Britta Simon v Five9 Plus adaptéru (CTI, obraťte se na centrum agenty), je toohello propojené služby Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="95ad3-145">**[Creating a Five9 Plus Adapter (CTI, Contact Center Agents) test user](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)** - toohave a counterpart of Britta Simon in Five9 Plus Adapter (CTI, Contact Center Agents) that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="95ad3-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="95ad3-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="95ad3-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="95ad3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="95ad3-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="95ad3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="95ad3-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Five9 Plus adaptér (CTI, obraťte se na centrum agenty).</span><span class="sxs-lookup"><span data-stu-id="95ad3-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Five9 Plus Adapter (CTI, Contact Center Agents) application.</span></span>

<span data-ttu-id="95ad3-150">**tooconfigure Azure AD jednotné přihlašování s Five9 Plus adaptérem (CTI, obraťte se na centrum agenty), proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="95ad3-150">**tooconfigure Azure AD single sign-on with Five9 Plus Adapter (CTI, Contact Center Agents), perform hello following steps:**</span></span>

1. <span data-ttu-id="95ad3-151">V portálu Azure, na hello hello **Five9 Plus adaptér (CTI, obraťte se na centrum agenty)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="95ad3-151">In hello Azure portal, on hello **Five9 Plus Adapter (CTI, Contact Center Agents)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="95ad3-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="95ad3-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-five9-tutorial/tutorial_five9_samlbase.png)

3. <span data-ttu-id="95ad3-155">Na hello **Five9 Plus adaptér (CTI, obraťte se na centrum agenty) domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="95ad3-155">On hello **Five9 Plus Adapter (CTI, Contact Center Agents) Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-five9-tutorial/tutorial_five9_url.png)
    
    <span data-ttu-id="95ad3-157">a.</span><span class="sxs-lookup"><span data-stu-id="95ad3-157">a.</span></span> <span data-ttu-id="95ad3-158">V hello **identifikátor** textové pole, zadejte adresu URL pomocí hello následující vzory:</span><span class="sxs-lookup"><span data-stu-id="95ad3-158">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span>

    |    <span data-ttu-id="95ad3-159">Prostředí</span><span class="sxs-lookup"><span data-stu-id="95ad3-159">Environment</span></span>      |       <span data-ttu-id="95ad3-160">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="95ad3-160">URL</span></span>      |
    | :-- | :-- |
    | <span data-ttu-id="95ad3-161">Pro "Five9 Plus adaptér pro aplikaci Microsoft Dynamics CRM"</span><span class="sxs-lookup"><span data-stu-id="95ad3-161">For “Five9 Plus Adapter for Microsoft Dynamics CRM”</span></span> | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | <span data-ttu-id="95ad3-162">Pro "Five9 Plus adaptér Zendesk"</span><span class="sxs-lookup"><span data-stu-id="95ad3-162">For “Five9 Plus Adapter for Zendesk”</span></span> | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | <span data-ttu-id="95ad3-163">Pro "Five9 Plus adaptér plochy Toolkit agenta"</span><span class="sxs-lookup"><span data-stu-id="95ad3-163">For “Five9 Plus Adapter for Agent Desktop Toolkit”</span></span> | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    <span data-ttu-id="95ad3-164">b.</span><span class="sxs-lookup"><span data-stu-id="95ad3-164">b.</span></span> <span data-ttu-id="95ad3-165">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="95ad3-165">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>

    |      <span data-ttu-id="95ad3-166">Prostředí</span><span class="sxs-lookup"><span data-stu-id="95ad3-166">Environment</span></span>     |      <span data-ttu-id="95ad3-167">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="95ad3-167">URL</span></span>      |
    | :--                  | :--           |
    | <span data-ttu-id="95ad3-168">Pro "Five9 Plus adaptér pro aplikaci Microsoft Dynamics CRM"</span><span class="sxs-lookup"><span data-stu-id="95ad3-168">For “Five9 Plus Adapter for Microsoft Dynamics CRM”</span></span> | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | <span data-ttu-id="95ad3-169">Pro "Five9 Plus adaptér Zendesk"</span><span class="sxs-lookup"><span data-stu-id="95ad3-169">For “Five9 Plus Adapter for Zendesk”</span></span> | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | <span data-ttu-id="95ad3-170">Pro "Five9 Plus adaptér plochy Toolkit agenta"</span><span class="sxs-lookup"><span data-stu-id="95ad3-170">For “Five9 Plus Adapter for Agent Desktop Toolkit”</span></span> | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

4. <span data-ttu-id="95ad3-171">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="95ad3-171">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-five9-tutorial/tutorial_five9_certificate.png) 

5. <span data-ttu-id="95ad3-173">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="95ad3-173">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-five9-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="95ad3-175">Na hello **konfigurace Five9 Plus adaptéru (CTI, obraťte se na centrum agenty)** klikněte na tlačítko **konfigurace Five9 Plus adaptér (CTI, obraťte se na centrum agenty)** tooopen **konfigurovatpřihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="95ad3-175">On hello **Five9 Plus Adapter (CTI, Contact Center Agents) Configuration** section, click **Configure Five9 Plus Adapter (CTI, Contact Center Agents)** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="95ad3-176">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="95ad3-176">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-five9-tutorial/tutorial_five9_configure.png) 

7. <span data-ttu-id="95ad3-178">tooconfigure jednotného přihlašování na **Five9 Plus adaptér (CTI, obraťte se na centrum agenty)** straně, je nutné stáhnout hello toosend **Certificate(Base64), Sign-Out URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** příliš[tým podpory Five9 Plus adaptér (CTI, obraťte se na centrum agenty)](https://www.five9.com/about/contact).</span><span class="sxs-lookup"><span data-stu-id="95ad3-178">tooconfigure single sign-on on **Five9 Plus Adapter (CTI, Contact Center Agents)** side, you need toosend hello downloaded **Certificate(Base64), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Five9 Plus Adapter (CTI, Contact Center Agents) support team](https://www.five9.com/about/contact).</span></span> <span data-ttu-id="95ad3-179">Také navíc další konfigurace jednotného přihlašování k postupujte hello níže uvedených pokynů podle toohello adaptéru:</span><span class="sxs-lookup"><span data-stu-id="95ad3-179">Also additionally, for configuring SSO further please follow hello below steps according toohello adapter:</span></span>

    <span data-ttu-id="95ad3-180">a.</span><span class="sxs-lookup"><span data-stu-id="95ad3-180">a.</span></span> <span data-ttu-id="95ad3-181">"Five9 Plus adaptér plochy Toolkit agenta" Příručka správce: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)</span><span class="sxs-lookup"><span data-stu-id="95ad3-181">“Five9 Plus Adapter for Agent Desktop Toolkit” Admin Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)</span></span>
    
    <span data-ttu-id="95ad3-182">b.</span><span class="sxs-lookup"><span data-stu-id="95ad3-182">b.</span></span> <span data-ttu-id="95ad3-183">"Five9 Plus adaptér pro aplikaci Microsoft Dynamics CRM" Příručka správce: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)</span><span class="sxs-lookup"><span data-stu-id="95ad3-183">“Five9 Plus Adapter for Microsoft Dynamics CRM” Admin Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)</span></span>
    
    <span data-ttu-id="95ad3-184">c.</span><span class="sxs-lookup"><span data-stu-id="95ad3-184">c.</span></span> <span data-ttu-id="95ad3-185">"Five9 Plus adaptér Zendesk" Příručka správce: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)</span><span class="sxs-lookup"><span data-stu-id="95ad3-185">“Five9 Plus Adapter for Zendesk” Admin Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)</span></span>


> [!TIP]
> <span data-ttu-id="95ad3-186">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="95ad3-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="95ad3-187">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="95ad3-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="95ad3-188">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="95ad3-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="95ad3-189">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="95ad3-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="95ad3-190">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="95ad3-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="95ad3-192">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="95ad3-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="95ad3-193">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="95ad3-193">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="95ad3-195">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="95ad3-195">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="95ad3-197">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="95ad3-197">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="95ad3-199">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="95ad3-199">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="95ad3-201">a.</span><span class="sxs-lookup"><span data-stu-id="95ad3-201">a.</span></span> <span data-ttu-id="95ad3-202">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="95ad3-202">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="95ad3-203">b.</span><span class="sxs-lookup"><span data-stu-id="95ad3-203">b.</span></span> <span data-ttu-id="95ad3-204">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="95ad3-204">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="95ad3-205">c.</span><span class="sxs-lookup"><span data-stu-id="95ad3-205">c.</span></span> <span data-ttu-id="95ad3-206">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="95ad3-206">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="95ad3-207">d.</span><span class="sxs-lookup"><span data-stu-id="95ad3-207">d.</span></span> <span data-ttu-id="95ad3-208">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="95ad3-208">Click **Create**.</span></span>
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a><span data-ttu-id="95ad3-209">Vytvoření zkušebního uživatele Five9 Plus adaptér (CTI, obraťte se na centrum agentů)</span><span class="sxs-lookup"><span data-stu-id="95ad3-209">Creating a Five9 Plus Adapter (CTI, Contact Center Agents) test user</span></span>

<span data-ttu-id="95ad3-210">V této části vytvoříte uživatele názvem Britta Simon v Five9 Plus adaptéru (CTI, obraťte se na centrum agenty).</span><span class="sxs-lookup"><span data-stu-id="95ad3-210">In this section, you create a user called Britta Simon in Five9 Plus Adapter (CTI, Contact Center Agents).</span></span> <span data-ttu-id="95ad3-211">Práce s [tým podpory Five9 Plus adaptér (CTI, obraťte se na centrum agenty)](https://www.five9.com/about/contact) pro přidání uživatelů hello platformy hello Five9 Plus adaptér (CTI, obraťte se na centrum agenty).</span><span class="sxs-lookup"><span data-stu-id="95ad3-211">Work with [Five9 Plus Adapter (CTI, Contact Center Agents) support team](https://www.five9.com/about/contact) to add hello users in hello Five9 Plus Adapter (CTI, Contact Center Agents) platform.</span></span> <span data-ttu-id="95ad3-212">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="95ad3-212">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="95ad3-213">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="95ad3-213">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="95ad3-214">V této části povolíte tak, že udělíte přístup tooFive9 Plus adaptér (CTI, obraťte se na centrum agenty) toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="95ad3-214">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFive9 Plus Adapter (CTI, Contact Center Agents).</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="95ad3-216">**tooassign Britta Simon tooFive9 Plus adaptér (CTI, obraťte se na centrum agenty), proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="95ad3-216">**tooassign Britta Simon tooFive9 Plus Adapter (CTI, Contact Center Agents), perform hello following steps:**</span></span>

1. <span data-ttu-id="95ad3-217">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="95ad3-217">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="95ad3-219">V seznamu aplikace hello vyberte **Five9 Plus adaptér (CTI, obraťte se na centrum agenty)**.</span><span class="sxs-lookup"><span data-stu-id="95ad3-219">In hello applications list, select **Five9 Plus Adapter (CTI, Contact Center Agents)**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-five9-tutorial/tutorial_five9_app.png) 

3. <span data-ttu-id="95ad3-221">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="95ad3-221">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="95ad3-223">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="95ad3-223">Click **Add** button.</span></span> <span data-ttu-id="95ad3-224">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="95ad3-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="95ad3-226">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="95ad3-226">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="95ad3-227">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="95ad3-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="95ad3-228">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="95ad3-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="95ad3-229">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="95ad3-229">Testing single sign-on</span></span>

<span data-ttu-id="95ad3-230">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="95ad3-230">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="95ad3-231">Když kliknete na dlaždici hello Five9 Plus adaptér (CTI, obraťte se na centrum agenty) v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Five9 Plus adaptér (CTI, obraťte se na centrum agenty) aplikace.</span><span class="sxs-lookup"><span data-stu-id="95ad3-231">When you click hello Five9 Plus Adapter (CTI, Contact Center Agents) tile in hello Access Panel, you should get automatically signed-on tooyour Five9 Plus Adapter (CTI, Contact Center Agents) application.</span></span>
<span data-ttu-id="95ad3-232">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="95ad3-232">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="95ad3-233">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="95ad3-233">Additional resources</span></span>

* [<span data-ttu-id="95ad3-234">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="95ad3-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="95ad3-235">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="95ad3-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-five9-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-five9-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-five9-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-five9-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-five9-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-five9-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-five9-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-five9-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-five9-tutorial/tutorial_general_203.png

