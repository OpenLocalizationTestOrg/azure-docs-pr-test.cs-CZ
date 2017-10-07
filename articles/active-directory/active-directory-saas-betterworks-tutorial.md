---
title: 'Kurz: Azure Active Directory integrace s BetterWorks | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a BetterWorks."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5bb9505a-be02-46ae-9979-5308715d2b47
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 9803593124318ea82e5a8888cc5a95b5da84472e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-betterworks"></a><span data-ttu-id="467f2-103">Kurz: Azure Active Directory integrace s BetterWorks</span><span class="sxs-lookup"><span data-stu-id="467f2-103">Tutorial: Azure Active Directory integration with BetterWorks</span></span>

<span data-ttu-id="467f2-104">V tomto kurzu zjistíte, jak toointegrate BetterWorks s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="467f2-104">In this tutorial, you learn how toointegrate BetterWorks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="467f2-105">Integrace BetterWorks s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="467f2-105">Integrating BetterWorks with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="467f2-106">Můžete řídit ve službě Azure AD, který má přístup tooBetterWorks</span><span class="sxs-lookup"><span data-stu-id="467f2-106">You can control in Azure AD who has access tooBetterWorks</span></span>
- <span data-ttu-id="467f2-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooBetterWorks (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="467f2-107">You can enable your users tooautomatically get signed-on tooBetterWorks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="467f2-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="467f2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="467f2-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="467f2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="467f2-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="467f2-110">Prerequisites</span></span>

<span data-ttu-id="467f2-111">Integrace služby Azure AD s BetterWorks tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="467f2-111">tooconfigure Azure AD integration with BetterWorks, you need hello following items:</span></span>

- <span data-ttu-id="467f2-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="467f2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="467f2-113">BetterWorks jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="467f2-113">A BetterWorks single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="467f2-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="467f2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="467f2-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="467f2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="467f2-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="467f2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="467f2-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="467f2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="467f2-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="467f2-118">Scenario description</span></span>
<span data-ttu-id="467f2-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="467f2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="467f2-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="467f2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="467f2-121">Přidání BetterWorks z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="467f2-121">Adding BetterWorks from hello gallery</span></span>
2. <span data-ttu-id="467f2-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="467f2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-betterworks-from-hello-gallery"></a><span data-ttu-id="467f2-123">Přidání BetterWorks z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="467f2-123">Adding BetterWorks from hello gallery</span></span>
<span data-ttu-id="467f2-124">tooconfigure hello integrace BetterWorks do Azure AD, je nutné tooadd BetterWorks hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="467f2-124">tooconfigure hello integration of BetterWorks into Azure AD, you need tooadd BetterWorks from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="467f2-125">**tooadd BetterWorks z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="467f2-125">**tooadd BetterWorks from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="467f2-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="467f2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="467f2-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="467f2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="467f2-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="467f2-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="467f2-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="467f2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="467f2-133">Hello vyhledávacího pole zadejte **BetterWorks**.</span><span class="sxs-lookup"><span data-stu-id="467f2-133">In hello search box, type **BetterWorks**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_search.png)

5. <span data-ttu-id="467f2-135">Na panelu výsledků hello vyberte **BetterWorks**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="467f2-135">In hello results panel, select **BetterWorks**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="467f2-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="467f2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="467f2-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s BetterWorks podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="467f2-138">In this section, you configure and test Azure AD single sign-on with BetterWorks based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="467f2-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v BetterWorks je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="467f2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BetterWorks is tooa user in Azure AD.</span></span> <span data-ttu-id="467f2-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v BetterWorks musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="467f2-140">In other words, a link relationship between an Azure AD user and hello related user in BetterWorks needs toobe established.</span></span>

<span data-ttu-id="467f2-141">V BetterWorks, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="467f2-141">In BetterWorks, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="467f2-142">tooconfigure a testu Azure AD jednotné přihlašování s BetterWorks, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="467f2-142">tooconfigure and test Azure AD single sign-on with BetterWorks, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="467f2-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="467f2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="467f2-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="467f2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="467f2-145">**[Vytvoření zkušebního uživatele BetterWorks](#creating-a-betterworks-test-user)**  -toohave protějšek Britta Simon v BetterWorks, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="467f2-145">**[Creating a BetterWorks test user](#creating-a-betterworks-test-user)** - toohave a counterpart of Britta Simon in BetterWorks that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="467f2-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="467f2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="467f2-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="467f2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="467f2-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="467f2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="467f2-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci BetterWorks.</span><span class="sxs-lookup"><span data-stu-id="467f2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BetterWorks application.</span></span>

<span data-ttu-id="467f2-150">**tooconfigure Azure AD jednotné přihlašování s BetterWorks, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="467f2-150">**tooconfigure Azure AD single sign-on with BetterWorks, perform hello following steps:**</span></span>

1. <span data-ttu-id="467f2-151">V portálu Azure, na hello hello **BetterWorks** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="467f2-151">In hello Azure portal, on hello **BetterWorks** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="467f2-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="467f2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_samlbase.png)

3. <span data-ttu-id="467f2-155">Na hello **BetterWorks domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu**:</span><span class="sxs-lookup"><span data-stu-id="467f2-155">On hello **BetterWorks Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_url.png)

    <span data-ttu-id="467f2-157">a.</span><span class="sxs-lookup"><span data-stu-id="467f2-157">a.</span></span> <span data-ttu-id="467f2-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://app.betterworks.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="467f2-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://app.betterworks.com/saml2/metadata/`</span></span>

    <span data-ttu-id="467f2-159">b.</span><span class="sxs-lookup"><span data-stu-id="467f2-159">b.</span></span> <span data-ttu-id="467f2-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://app.betterworks.com/saml2/acs/`</span><span class="sxs-lookup"><span data-stu-id="467f2-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://app.betterworks.com/saml2/acs/`</span></span>

4. <span data-ttu-id="467f2-161">Na hello **BetterWorks domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **SP iniciované režimu**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="467f2-161">On hello **BetterWorks Domain and URLs** section, If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_url1.png)

    <span data-ttu-id="467f2-163">a.</span><span class="sxs-lookup"><span data-stu-id="467f2-163">a.</span></span> <span data-ttu-id="467f2-164">Klikněte na hello **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="467f2-164">Click on hello **Show advanced URL settings**.</span></span>

    <span data-ttu-id="467f2-165">b.</span><span class="sxs-lookup"><span data-stu-id="467f2-165">b.</span></span> <span data-ttu-id="467f2-166">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://app.betterworks.com`</span><span class="sxs-lookup"><span data-stu-id="467f2-166">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://app.betterworks.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="467f2-167">Tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="467f2-167">These are not real values.</span></span> <span data-ttu-id="467f2-168">Tyto hodnoty aktualizujte s hello adresa URL odpovědi, identifikátor a skutečný přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="467f2-168">Update these values with hello Reply URL, Identifier and actual Sign On URL.</span></span> <span data-ttu-id="467f2-169">Obraťte se na [tým podpory BetterWorks](mailto:support@betterworks.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="467f2-169">Contact [BetterWorks support team](mailto:support@betterworks.com) tooget these values.</span></span>
 
4. <span data-ttu-id="467f2-170">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="467f2-170">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_certificate.png)  

5. <span data-ttu-id="467f2-172">Aplikace BetterWorks očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="467f2-172">BetterWorks application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="467f2-173">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="467f2-173">Configure hello following claims for this application.</span></span> <span data-ttu-id="467f2-174">Můžete spravovat hello hodnoty těchto atributů z hello "**atribut**" Karta aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="467f2-174">You can manage hello values of these attributes from hello "**Attribute**" tab of hello application.</span></span> <span data-ttu-id="467f2-175">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="467f2-175">hello following screenshot shows an example for this.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_attribute.png)

6. <span data-ttu-id="467f2-177">Na hello **atributy tokenu SAML** dialogu pro každý řádek v tabulce hello níže, proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="467f2-177">On hello **SAML token attributes** dialog, for each row shown in hello table below, perform hello following steps:</span></span>
 
   | <span data-ttu-id="467f2-178">Název atributu</span><span class="sxs-lookup"><span data-stu-id="467f2-178">Attribute Name</span></span> | <span data-ttu-id="467f2-179">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="467f2-179">Attribute Value</span></span> |
   | -------------- |  ------------ |
   | <span data-ttu-id="467f2-180">saml_token</span><span class="sxs-lookup"><span data-stu-id="467f2-180">saml_token</span></span>     | <span data-ttu-id="467f2-181">bd189cf6-1701-11e6-8f90-d26992eca2a5</span><span class="sxs-lookup"><span data-stu-id="467f2-181">bd189cf6-1701-11e6-8f90-d26992eca2a5</span></span> |

   <span data-ttu-id="467f2-182">a.</span><span class="sxs-lookup"><span data-stu-id="467f2-182">a.</span></span> <span data-ttu-id="467f2-183">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="467f2-183">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_officespace_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_officespace_05.png)

   <span data-ttu-id="467f2-186">b.</span><span class="sxs-lookup"><span data-stu-id="467f2-186">b.</span></span> <span data-ttu-id="467f2-187">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="467f2-187">In hello **Name** textbox, type hello attribute name shown for that row.</span></span> 

   <span data-ttu-id="467f2-188">c.</span><span class="sxs-lookup"><span data-stu-id="467f2-188">c.</span></span> <span data-ttu-id="467f2-189">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="467f2-189">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
   <span data-ttu-id="467f2-190">d.</span><span class="sxs-lookup"><span data-stu-id="467f2-190">d.</span></span> <span data-ttu-id="467f2-191">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="467f2-191">Click **Ok**.</span></span>

7. <span data-ttu-id="467f2-192">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="467f2-192">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="467f2-194">tooconfigure jednotného přihlašování na **BetterWorks** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory BetterWorks](mailto:support@betterworks.com).</span><span class="sxs-lookup"><span data-stu-id="467f2-194">tooconfigure single sign-on on **BetterWorks** side, you need toosend hello downloaded **Metadata XML** too[BetterWorks support team](mailto:support@betterworks.com).</span></span>


> [!TIP]
> <span data-ttu-id="467f2-195">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="467f2-195">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="467f2-196">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="467f2-196">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="467f2-197">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="467f2-197">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="467f2-198">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="467f2-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="467f2-199">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="467f2-199">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="467f2-201">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="467f2-201">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="467f2-202">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="467f2-202">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="467f2-204">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="467f2-204">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="467f2-206">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="467f2-206">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="467f2-208">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="467f2-208">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="467f2-210">a.</span><span class="sxs-lookup"><span data-stu-id="467f2-210">a.</span></span> <span data-ttu-id="467f2-211">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="467f2-211">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="467f2-212">b.</span><span class="sxs-lookup"><span data-stu-id="467f2-212">b.</span></span> <span data-ttu-id="467f2-213">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="467f2-213">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="467f2-214">c.</span><span class="sxs-lookup"><span data-stu-id="467f2-214">c.</span></span> <span data-ttu-id="467f2-215">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="467f2-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="467f2-216">d.</span><span class="sxs-lookup"><span data-stu-id="467f2-216">d.</span></span> <span data-ttu-id="467f2-217">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="467f2-217">Click **Create**.</span></span>
 
### <a name="creating-a-betterworks-test-user"></a><span data-ttu-id="467f2-218">Vytvoření zkušebního uživatele BetterWorks</span><span class="sxs-lookup"><span data-stu-id="467f2-218">Creating a BetterWorks test user</span></span>

<span data-ttu-id="467f2-219">V této části vytvoříte volal Britta Simon v BetterWorks uživatele.</span><span class="sxs-lookup"><span data-stu-id="467f2-219">In this section, you create a user called Britta Simon in BetterWorks.</span></span> <span data-ttu-id="467f2-220">Práce s [tým podpory BetterWorks](mailto:support@betterworks.com) tooadd hello uživatelé v platformě BetterWorks hello.</span><span class="sxs-lookup"><span data-stu-id="467f2-220">Work with [BetterWorks support team](mailto:support@betterworks.com) tooadd hello users in hello BetterWorks platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="467f2-221">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="467f2-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="467f2-222">V této části povolíte tak, že udělíte přístup tooBetterWorks toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="467f2-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBetterWorks.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="467f2-224">**tooassign Britta Simon tooBetterWorks, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="467f2-224">**tooassign Britta Simon tooBetterWorks, perform hello following steps:**</span></span>

1. <span data-ttu-id="467f2-225">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="467f2-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="467f2-227">V seznamu aplikace hello vyberte **BetterWorks**.</span><span class="sxs-lookup"><span data-stu-id="467f2-227">In hello applications list, select **BetterWorks**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_app.png) 

3. <span data-ttu-id="467f2-229">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="467f2-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="467f2-231">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="467f2-231">Click **Add** button.</span></span> <span data-ttu-id="467f2-232">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="467f2-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="467f2-234">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="467f2-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="467f2-235">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="467f2-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="467f2-236">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="467f2-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="467f2-237">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="467f2-237">Testing single sign-on</span></span>

<span data-ttu-id="467f2-238">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="467f2-238">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="467f2-239">Po kliknutí na tlačítko hello BetterWorks dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour BetterWorks aplikace.</span><span class="sxs-lookup"><span data-stu-id="467f2-239">When you click hello BetterWorks tile in hello Access Panel, you should get automatically signed-on tooyour BetterWorks application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="467f2-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="467f2-240">Additional resources</span></span>

* [<span data-ttu-id="467f2-241">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="467f2-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="467f2-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="467f2-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_203.png

