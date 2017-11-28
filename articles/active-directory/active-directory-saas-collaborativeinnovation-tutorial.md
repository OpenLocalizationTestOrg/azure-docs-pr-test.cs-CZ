---
title: "Kurz: Azure Active Directory integrace s spolupráce inovací | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a spolupráce inovace."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bba95df3-75a4-4a93-8805-b3a8aa3d4861
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: e85fabfe11a380129f319a101aa7c7a9491260f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-collaborative-innovation"></a><span data-ttu-id="2ef8c-103">Kurz: Azure Active Directory integrace s inovací spolupráce</span><span class="sxs-lookup"><span data-stu-id="2ef8c-103">Tutorial: Azure Active Directory integration with Collaborative Innovation</span></span>

<span data-ttu-id="2ef8c-104">V tomto kurzu zjistíte, jak toointegrate spolupráce inovace v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2ef8c-104">In this tutorial, you learn how toointegrate Collaborative Innovation with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2ef8c-105">Integrace spolupráce inovací s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2ef8c-105">Integrating Collaborative Innovation with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2ef8c-106">Můžete řídit ve službě Azure AD, který má přístup tooCollaborative inovací</span><span class="sxs-lookup"><span data-stu-id="2ef8c-106">You can control in Azure AD who has access tooCollaborative Innovation</span></span>
- <span data-ttu-id="2ef8c-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooCollaborative inovací (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2ef8c-107">You can enable your users tooautomatically get signed-on tooCollaborative Innovation (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2ef8c-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2ef8c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2ef8c-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2ef8c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2ef8c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2ef8c-110">Prerequisites</span></span>

<span data-ttu-id="2ef8c-111">tooconfigure integrace Azure AD s spolupráce inovací, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="2ef8c-111">tooconfigure Azure AD integration with Collaborative Innovation, you need hello following items:</span></span>

- <span data-ttu-id="2ef8c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2ef8c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2ef8c-113">Spolupráce inovací jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2ef8c-113">A Collaborative Innovation single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2ef8c-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2ef8c-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2ef8c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2ef8c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2ef8c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2ef8c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2ef8c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2ef8c-118">Scenario description</span></span>
<span data-ttu-id="2ef8c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2ef8c-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2ef8c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2ef8c-121">Přidání spolupráce inovací z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2ef8c-121">Adding Collaborative Innovation from hello gallery</span></span>
2. <span data-ttu-id="2ef8c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2ef8c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-collaborative-innovation-from-hello-gallery"></a><span data-ttu-id="2ef8c-123">Přidání spolupráce inovací z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2ef8c-123">Adding Collaborative Innovation from hello gallery</span></span>
<span data-ttu-id="2ef8c-124">tooconfigure hello integrace spolupráce inovací do Azure AD, je nutné tooadd spolupráce inovací hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-124">tooconfigure hello integration of Collaborative Innovation into Azure AD, you need tooadd Collaborative Innovation from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2ef8c-125">**tooadd spolupráce inovací z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2ef8c-125">**tooadd Collaborative Innovation from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2ef8c-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2ef8c-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2ef8c-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2ef8c-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2ef8c-133">Hello vyhledávacího pole zadejte **spolupráce inovací**.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-133">In hello search box, type **Collaborative Innovation**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_search.png)

5. <span data-ttu-id="2ef8c-135">Na panelu výsledků hello vyberte **spolupráce inovací**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-135">In hello results panel, select **Collaborative Innovation**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2ef8c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2ef8c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2ef8c-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s spolupráce inovací podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="2ef8c-138">In this section, you configure and test Azure AD single sign-on with Collaborative Innovation based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2ef8c-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v spolupráce inovací je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Collaborative Innovation is tooa user in Azure AD.</span></span> <span data-ttu-id="2ef8c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v spolupráce inovací musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-140">In other words, a link relationship between an Azure AD user and hello related user in Collaborative Innovation needs toobe established.</span></span>

<span data-ttu-id="2ef8c-141">V spolupráce inovací přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-141">In Collaborative Innovation, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="2ef8c-142">tooconfigure a testu Azure AD jednotné přihlašování s spolupráce inovací, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="2ef8c-142">tooconfigure and test Azure AD single sign-on with Collaborative Innovation, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2ef8c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2ef8c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2ef8c-145">**[Vytvoření zkušebního uživatele spolupráce inovací](#creating-a-collaborative-innovation-test-user)**  -toohave protějšek Britta Simon v spolupráce inovací, které je propojené toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-145">**[Creating a Collaborative Innovation test user](#creating-a-collaborative-innovation-test-user)** - toohave a counterpart of Britta Simon in Collaborative Innovation that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2ef8c-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2ef8c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2ef8c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2ef8c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2ef8c-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci spolupráce inovace.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Collaborative Innovation application.</span></span>

<span data-ttu-id="2ef8c-150">**tooconfigure Azure AD jednotné přihlašování s spolupráce inovací, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2ef8c-150">**tooconfigure Azure AD single sign-on with Collaborative Innovation, perform hello following steps:**</span></span>

1. <span data-ttu-id="2ef8c-151">V portálu Azure, na hello hello **spolupráce inovací** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-151">In hello Azure portal, on hello **Collaborative Innovation** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2ef8c-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_samlbase.png)

3. <span data-ttu-id="2ef8c-155">Na hello **spolupráce inovací domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2ef8c-155">On hello **Collaborative Innovation Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_url.png)

    <span data-ttu-id="2ef8c-157">a.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-157">a.</span></span> <span data-ttu-id="2ef8c-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instancename>.foundry.<companyname>.com/`</span><span class="sxs-lookup"><span data-stu-id="2ef8c-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<instancename>.foundry.<companyname>.com/`</span></span>

    <span data-ttu-id="2ef8c-159">b.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-159">b.</span></span> <span data-ttu-id="2ef8c-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instancename>.foundry.<companyname>.com`</span><span class="sxs-lookup"><span data-stu-id="2ef8c-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instancename>.foundry.<companyname>.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="2ef8c-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-161">These values are not real.</span></span> <span data-ttu-id="2ef8c-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2ef8c-163">Obraťte se na [tým podpory spolupráce klienta inovací](https://www.unilever.com/contact/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-163">Contact [Collaborative Innovation Client support team](https://www.unilever.com/contact/) tooget these values.</span></span>  

4. <span data-ttu-id="2ef8c-164">Spolupráce aplikace inovací očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-164">Collaborative Innovation application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="2ef8c-165">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-165">Please configure hello following claims for this application.</span></span> <span data-ttu-id="2ef8c-166">Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-166">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="2ef8c-167">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-167">hello following screenshot shows an example for this.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-collaborativeinnovation-tutorial/attribute.png)
    
5. <span data-ttu-id="2ef8c-169">Klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** zaškrtnout políčko hello **uživatelské atributy** části tooexpand hello atributy.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-169">Click **View and edit all other user attributes** checkbox in hello **User Attributes** section tooexpand hello attributes.</span></span> <span data-ttu-id="2ef8c-170">Proveďte následující kroky na každém z hello zobrazí atributy - hello</span><span class="sxs-lookup"><span data-stu-id="2ef8c-170">Perform hello following steps on each of hello displayed attributes-</span></span>

    | <span data-ttu-id="2ef8c-171">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2ef8c-171">Attribute Name</span></span> | <span data-ttu-id="2ef8c-172">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="2ef8c-172">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="2ef8c-173">givenName</span><span class="sxs-lookup"><span data-stu-id="2ef8c-173">givenname</span></span> | <span data-ttu-id="2ef8c-174">User.givenName</span><span class="sxs-lookup"><span data-stu-id="2ef8c-174">user.givenname</span></span> |
    | <span data-ttu-id="2ef8c-175">Příjmení</span><span class="sxs-lookup"><span data-stu-id="2ef8c-175">surname</span></span> | <span data-ttu-id="2ef8c-176">User.Surname</span><span class="sxs-lookup"><span data-stu-id="2ef8c-176">user.surname</span></span> |
    | <span data-ttu-id="2ef8c-177">EmailAddress</span><span class="sxs-lookup"><span data-stu-id="2ef8c-177">emailaddress</span></span> | <span data-ttu-id="2ef8c-178">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="2ef8c-178">user.userprincipalname</span></span> |
    | <span data-ttu-id="2ef8c-179">jméno</span><span class="sxs-lookup"><span data-stu-id="2ef8c-179">name</span></span> | <span data-ttu-id="2ef8c-180">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="2ef8c-180">user.userprincipalname</span></span> |

    <span data-ttu-id="2ef8c-181">a.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-181">a.</span></span> <span data-ttu-id="2ef8c-182">Klikněte na tlačítko hello atribut tooopen hello **Upravit atribut** okno.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-182">Click hello attribute tooopen hello **Edit Attribute** window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-collaborativeinnovation-tutorial/url_update.png)

    <span data-ttu-id="2ef8c-184">b.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-184">b.</span></span> <span data-ttu-id="2ef8c-185">Odstranit hodnotu adresy URL hello z hello **Namespace**.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-185">Delete hello URL value from hello **Namespace**.</span></span>
    
    <span data-ttu-id="2ef8c-186">c.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-186">c.</span></span> <span data-ttu-id="2ef8c-187">Klikněte na tlačítko **Ok** toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-187">Click **Ok** toosave hello setting.</span></span>

6. <span data-ttu-id="2ef8c-188">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-188">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_certificate.png) 

7. <span data-ttu-id="2ef8c-190">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-190">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="2ef8c-192">tooconfigure jednotného přihlašování na **spolupráce inovací** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory spolupráce inovací](https://www.unilever.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="2ef8c-192">tooconfigure single sign-on on **Collaborative Innovation** side, you need toosend hello downloaded **Metadata XML** too[Collaborative Innovation support team](https://www.unilever.com/contact/).</span></span> <span data-ttu-id="2ef8c-193">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-193">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="2ef8c-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="2ef8c-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2ef8c-195">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2ef8c-196">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2ef8c-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2ef8c-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2ef8c-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="2ef8c-198">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2ef8c-200">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2ef8c-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2ef8c-201">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2ef8c-203">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2ef8c-205">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2ef8c-207">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2ef8c-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2ef8c-209">a.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-209">a.</span></span> <span data-ttu-id="2ef8c-210">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2ef8c-211">b.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-211">b.</span></span> <span data-ttu-id="2ef8c-212">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2ef8c-213">c.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-213">c.</span></span> <span data-ttu-id="2ef8c-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2ef8c-215">d.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-215">d.</span></span> <span data-ttu-id="2ef8c-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-216">Click **Create**.</span></span>
 
### <a name="creating-a-collaborative-innovation-test-user"></a><span data-ttu-id="2ef8c-217">Vytvoření zkušebního uživatele spolupráce inovací</span><span class="sxs-lookup"><span data-stu-id="2ef8c-217">Creating a Collaborative Innovation test user</span></span>

<span data-ttu-id="2ef8c-218">Uživatelé toolog tooenable Azure AD v tooCollaborative inovací, se musí být zřízená do spolupráce inovace.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-218">tooenable Azure AD users toolog in tooCollaborative Innovation, they must be provisioned into Collaborative Innovation.</span></span>  

<span data-ttu-id="2ef8c-219">Zřizování je automaticky v případě této aplikace jako aplikace hello podporuje jenom při zřizování uživatelů čas.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-219">In case of this application provisioning is automatic as hello application supports just in time user provisioning.</span></span> <span data-ttu-id="2ef8c-220">Proto je bez nutnosti tooperform zde žádné kroky.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-220">So there is no need tooperform any steps here.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2ef8c-221">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="2ef8c-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2ef8c-222">V této části povolíte tak, že udělíte přístup tooCollaborative inovací Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCollaborative Innovation.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2ef8c-224">**tooassign tooCollaborative Britta Simon inovací, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2ef8c-224">**tooassign Britta Simon tooCollaborative Innovation, perform hello following steps:**</span></span>

1. <span data-ttu-id="2ef8c-225">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2ef8c-227">V seznamu aplikace hello vyberte **spolupráce inovací**.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-227">In hello applications list, select **Collaborative Innovation**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_app.png) 

3. <span data-ttu-id="2ef8c-229">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2ef8c-231">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-231">Click **Add** button.</span></span> <span data-ttu-id="2ef8c-232">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2ef8c-234">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2ef8c-235">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2ef8c-236">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2ef8c-237">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2ef8c-237">Testing single sign-on</span></span>

<span data-ttu-id="2ef8c-238">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-238">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2ef8c-239">Když kliknete na dlaždici hello spolupráce inovace v hello přístupového panelu, měli byste obdržet přihlašovací stránku aplikace spolupráce inovace.</span><span class="sxs-lookup"><span data-stu-id="2ef8c-239">When you click hello Collaborative Innovation tile in hello Access Panel, you should get login page of Collaborative Innovation application.</span></span>
<span data-ttu-id="2ef8c-240">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2ef8c-240">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2ef8c-241">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2ef8c-241">Additional resources</span></span>

* [<span data-ttu-id="2ef8c-242">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2ef8c-242">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2ef8c-243">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2ef8c-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_203.png

