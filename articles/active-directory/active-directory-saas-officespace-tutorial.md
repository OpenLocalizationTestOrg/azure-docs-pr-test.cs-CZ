---
title: 'Kurz: Azure Active Directory integrace s OfficeSpace softwarem | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi OfficeSpace softwarem a Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 95d8413f-db98-4e2c-8097-9142ef1af823
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: b53afb648b8a6057c32c782d857e34c06e152c67
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-officespace-software"></a><span data-ttu-id="848b1-103">Kurz: Azure Active Directory integrace s OfficeSpace softwaru</span><span class="sxs-lookup"><span data-stu-id="848b1-103">Tutorial: Azure Active Directory integration with OfficeSpace Software</span></span>

<span data-ttu-id="848b1-104">V tomto kurzu zjistíte, jak toointegrate OfficeSpace Software s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="848b1-104">In this tutorial, you learn how toointegrate OfficeSpace Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="848b1-105">Integrace OfficeSpace softwaru s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="848b1-105">Integrating OfficeSpace Software with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="848b1-106">Můžete ovládat ve službě Azure AD, který má přístup tooOfficeSpace softwaru.</span><span class="sxs-lookup"><span data-stu-id="848b1-106">You can control in Azure AD who has access tooOfficeSpace Software.</span></span>
- <span data-ttu-id="848b1-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooOfficeSpace softwaru (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="848b1-107">You can enable your users tooautomatically get signed-on tooOfficeSpace Software (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="848b1-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="848b1-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="848b1-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="848b1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="848b1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="848b1-110">Prerequisites</span></span>

<span data-ttu-id="848b1-111">tooconfigure integrace Azure AD s OfficeSpace softwaru, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="848b1-111">tooconfigure Azure AD integration with OfficeSpace Software, you need hello following items:</span></span>

- <span data-ttu-id="848b1-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="848b1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="848b1-113">OfficeSpace softwaru jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="848b1-113">A OfficeSpace Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="848b1-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="848b1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="848b1-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="848b1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="848b1-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="848b1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="848b1-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="848b1-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="848b1-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="848b1-118">Scenario description</span></span>
<span data-ttu-id="848b1-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="848b1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="848b1-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="848b1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="848b1-121">Přidání softwaru OfficeSpace z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="848b1-121">Adding OfficeSpace Software from hello gallery</span></span>
2. <span data-ttu-id="848b1-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="848b1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-officespace-software-from-hello-gallery"></a><span data-ttu-id="848b1-123">Přidání softwaru OfficeSpace z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="848b1-123">Adding OfficeSpace Software from hello gallery</span></span>
<span data-ttu-id="848b1-124">tooconfigure hello integrace OfficeSpace softwaru do služby Azure AD, je nutné tooadd OfficeSpace softwaru hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="848b1-124">tooconfigure hello integration of OfficeSpace Software into Azure AD, you need tooadd OfficeSpace Software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="848b1-125">**tooadd OfficeSpace softwaru z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="848b1-125">**tooadd OfficeSpace Software from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="848b1-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="848b1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="848b1-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="848b1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="848b1-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="848b1-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="848b1-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="848b1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="848b1-133">Hello vyhledávacího pole zadejte **OfficeSpace softwaru**, vyberte **OfficeSpace softwaru** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="848b1-133">In hello search box, type **OfficeSpace Software**, select **OfficeSpace Software** from result panel then click **Add** button tooadd hello application.</span></span>

    ![OfficeSpace Software v hello seznamu výsledků](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="848b1-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="848b1-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="848b1-136">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s OfficeSpace softwarem podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="848b1-136">In this section, you configure and test Azure AD single sign-on with OfficeSpace Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="848b1-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v OfficeSpace softwaru je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="848b1-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in OfficeSpace Software is tooa user in Azure AD.</span></span> <span data-ttu-id="848b1-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v softwaru OfficeSpace musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="848b1-138">In other words, a link relationship between an Azure AD user and hello related user in OfficeSpace Software needs toobe established.</span></span>

<span data-ttu-id="848b1-139">V softwaru OfficeSpace přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="848b1-139">In OfficeSpace Software, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="848b1-140">tooconfigure a testu Azure AD jednotné přihlašování s OfficeSpace softwaru, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="848b1-140">tooconfigure and test Azure AD single sign-on with OfficeSpace Software, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="848b1-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="848b1-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="848b1-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="848b1-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="848b1-143">**[Vytvoření zkušebního uživatele softwaru OfficeSpace](#create-a-officespace-software-test-user)**  -toohave protějšek Britta Simon v OfficeSpace Software, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="848b1-143">**[Create a OfficeSpace Software test user](#create-a-officespace-software-test-user)** - toohave a counterpart of Britta Simon in OfficeSpace Software that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="848b1-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="848b1-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="848b1-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="848b1-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="848b1-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="848b1-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="848b1-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci OfficeSpace softwaru.</span><span class="sxs-lookup"><span data-stu-id="848b1-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your OfficeSpace Software application.</span></span>

<span data-ttu-id="848b1-148">**tooconfigure Azure AD jednotné přihlašování s OfficeSpace softwarem, provést hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="848b1-148">**tooconfigure Azure AD single sign-on with OfficeSpace Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="848b1-149">V portálu Azure, na hello hello **OfficeSpace softwaru** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="848b1-149">In hello Azure portal, on hello **OfficeSpace Software** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="848b1-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="848b1-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_samlbase.png)

3. <span data-ttu-id="848b1-153">Na hello **OfficeSpace softwaru domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="848b1-153">On hello **OfficeSpace Software Domain and URLs** section, perform hello following steps:</span></span>

    ![OfficeSpace softwaru domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_url.png)

    <span data-ttu-id="848b1-155">a.</span><span class="sxs-lookup"><span data-stu-id="848b1-155">a.</span></span> <span data-ttu-id="848b1-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.officespacesoftware.com/users/sign_in/saml`</span><span class="sxs-lookup"><span data-stu-id="848b1-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.officespacesoftware.com/users/sign_in/saml`</span></span>

    <span data-ttu-id="848b1-157">b.</span><span class="sxs-lookup"><span data-stu-id="848b1-157">b.</span></span> <span data-ttu-id="848b1-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`<company name>.officespacesoftware.com`</span><span class="sxs-lookup"><span data-stu-id="848b1-158">In hello **Identifier** textbox, type a URL using hello following pattern: `<company name>.officespacesoftware.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="848b1-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="848b1-159">These values are not real.</span></span> <span data-ttu-id="848b1-160">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="848b1-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="848b1-161">Obraťte se na [tým podpory klientský Software OfficeSpace](mailto:support@officespacesoftware.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="848b1-161">Contact [OfficeSpace Software Client support team](mailto:support@officespacesoftware.com) tooget these values.</span></span> 

4. <span data-ttu-id="848b1-162">OfficeSpace softwarová aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="848b1-162">OfficeSpace Software application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="848b1-163">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="848b1-163">Please configure hello following claims for this application.</span></span> <span data-ttu-id="848b1-164">Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="848b1-164">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="848b1-165">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="848b1-165">hello following screenshot shows an example for this.</span></span>
    
    ![Konfigurace atributů](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_attribute.png)

5. <span data-ttu-id="848b1-167">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogovém okně, vyberte **user.mail** jako **uživatelský identifikátor** a pro každý řádek ukazuje v tabulce hello níže proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="848b1-167">In hello **User Attributes** section on hello **Single sign-on** dialog, select **user.mail**  as **User Identifier** and for each row shown in hello table below, perform hello following steps:</span></span>
    
    | <span data-ttu-id="848b1-168">Název atributu</span><span class="sxs-lookup"><span data-stu-id="848b1-168">Attribute Name</span></span> | <span data-ttu-id="848b1-169">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="848b1-169">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="848b1-170">E-mailu</span><span class="sxs-lookup"><span data-stu-id="848b1-170">email</span></span> | <span data-ttu-id="848b1-171">User.Mail</span><span class="sxs-lookup"><span data-stu-id="848b1-171">user.mail</span></span> |
    | <span data-ttu-id="848b1-172">jméno</span><span class="sxs-lookup"><span data-stu-id="848b1-172">name</span></span> | <span data-ttu-id="848b1-173">User.DisplayName</span><span class="sxs-lookup"><span data-stu-id="848b1-173">user.displayname</span></span> |
    | <span data-ttu-id="848b1-174">křestní_jméno</span><span class="sxs-lookup"><span data-stu-id="848b1-174">first_name</span></span> | <span data-ttu-id="848b1-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="848b1-175">user.givenname</span></span> |
    | <span data-ttu-id="848b1-176">Příjmení</span><span class="sxs-lookup"><span data-stu-id="848b1-176">last_name</span></span> | <span data-ttu-id="848b1-177">User.Surname</span><span class="sxs-lookup"><span data-stu-id="848b1-177">user.surname</span></span> |

    <span data-ttu-id="848b1-178">a.</span><span class="sxs-lookup"><span data-stu-id="848b1-178">a.</span></span> <span data-ttu-id="848b1-179">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="848b1-179">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![<span data-ttu-id="848b1-180">Konfigurace přidat</span><span class="sxs-lookup"><span data-stu-id="848b1-180">Configure Add</span></span> ](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_04.png)

    ![Konfigurace atributů](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="848b1-182">b.</span><span class="sxs-lookup"><span data-stu-id="848b1-182">b.</span></span> <span data-ttu-id="848b1-183">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="848b1-183">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="848b1-184">c.</span><span class="sxs-lookup"><span data-stu-id="848b1-184">c.</span></span> <span data-ttu-id="848b1-185">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="848b1-185">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="848b1-186">d.</span><span class="sxs-lookup"><span data-stu-id="848b1-186">d.</span></span> <span data-ttu-id="848b1-187">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="848b1-187">Click **Ok**</span></span>
 
6. <span data-ttu-id="848b1-188">Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnota hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="848b1-188">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of hello certificate.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_certificate.png) 

7. <span data-ttu-id="848b1-190">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="848b1-190">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-officespace-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="848b1-192">Na hello **OfficeSpace softwarové konfigurace** klikněte na tlačítko **konfigurace softwaru OfficeSpace** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="848b1-192">On hello **OfficeSpace Software Configuration** section, click **Configure OfficeSpace Software** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="848b1-193">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="848b1-193">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace OfficeSpace softwaru](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_configure.png) 

9. <span data-ttu-id="848b1-195">V okně prohlížeče jiný web Přihlaste se ke klientovi OfficeSpace softwaru jako správce.</span><span class="sxs-lookup"><span data-stu-id="848b1-195">In a different web browser window, log into your OfficeSpace Software tenant as an administrator.</span></span>

10. <span data-ttu-id="848b1-196">Přejděte příliš**nastavení** a klikněte na tlačítko **konektory**.</span><span class="sxs-lookup"><span data-stu-id="848b1-196">Go too**Settings** and click **Connectors**.</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_002.png)

11. <span data-ttu-id="848b1-198">Klikněte na tlačítko **ověřování SAML**.</span><span class="sxs-lookup"><span data-stu-id="848b1-198">Click **SAML Authentication**.</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_003.png)

12. <span data-ttu-id="848b1-200">V hello **ověřování SAML** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="848b1-200">In hello **SAML Authentication** section, perform hello following steps:</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_004.png)

    <span data-ttu-id="848b1-202">a.</span><span class="sxs-lookup"><span data-stu-id="848b1-202">a.</span></span> <span data-ttu-id="848b1-203">V hello **adresy url odhlašovací zprostředkovatele** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="848b1-203">In hello **Logout provider url** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="848b1-204">b.</span><span class="sxs-lookup"><span data-stu-id="848b1-204">b.</span></span> <span data-ttu-id="848b1-205">V hello **klienta idp cílová adresa url** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="848b1-205">In hello **Client idp target url** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="848b1-206">c.</span><span class="sxs-lookup"><span data-stu-id="848b1-206">c.</span></span> <span data-ttu-id="848b1-207">Vložení hello **kryptografický otisk** hodnotu, která jste zkopírovali z portálu Azure do hello **otisk certifikátu klienta IDP** textové pole.</span><span class="sxs-lookup"><span data-stu-id="848b1-207">Paste hello **Thumbprint** value which you have copied from Azure portal, into hello **Client IDP certificate fingerprint** textbox.</span></span> 

    <span data-ttu-id="848b1-208">d.</span><span class="sxs-lookup"><span data-stu-id="848b1-208">d.</span></span> <span data-ttu-id="848b1-209">Klikněte na tlačítko **uložit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="848b1-209">Click **Save Settings**.</span></span>


> [!TIP]
> <span data-ttu-id="848b1-210">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="848b1-210">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="848b1-211">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="848b1-211">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="848b1-212">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="848b1-212">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="848b1-213">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="848b1-213">Create an Azure AD test user</span></span>

<span data-ttu-id="848b1-214">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="848b1-214">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="848b1-216">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="848b1-216">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="848b1-217">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="848b1-217">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-officespace-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="848b1-219">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="848b1-219">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-officespace-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="848b1-221">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="848b1-221">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-officespace-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="848b1-223">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="848b1-223">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-officespace-tutorial/create_aaduser_04.png)

    <span data-ttu-id="848b1-225">a.</span><span class="sxs-lookup"><span data-stu-id="848b1-225">a.</span></span> <span data-ttu-id="848b1-226">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="848b1-226">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="848b1-227">b.</span><span class="sxs-lookup"><span data-stu-id="848b1-227">b.</span></span> <span data-ttu-id="848b1-228">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="848b1-228">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="848b1-229">c.</span><span class="sxs-lookup"><span data-stu-id="848b1-229">c.</span></span> <span data-ttu-id="848b1-230">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="848b1-230">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="848b1-231">d.</span><span class="sxs-lookup"><span data-stu-id="848b1-231">d.</span></span> <span data-ttu-id="848b1-232">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="848b1-232">Click **Create**.</span></span>
 
### <a name="create-a-officespace-software-test-user"></a><span data-ttu-id="848b1-233">Vytvoření zkušebního uživatele OfficeSpace softwaru</span><span class="sxs-lookup"><span data-stu-id="848b1-233">Create a OfficeSpace Software test user</span></span>

<span data-ttu-id="848b1-234">Hello cílem této části je toocreate uživatel volal Britta Simon v OfficeSpace softwaru.</span><span class="sxs-lookup"><span data-stu-id="848b1-234">hello objective of this section is toocreate a user called Britta Simon in OfficeSpace Software.</span></span> <span data-ttu-id="848b1-235">OfficeSpace Software podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="848b1-235">OfficeSpace Software supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="848b1-236">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="848b1-236">There is no action item for you in this section.</span></span> <span data-ttu-id="848b1-237">Nového uživatele se vytvoří během pokusu o tooaccess OfficeSpace softwaru, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="848b1-237">A new user will be created during an attempt tooaccess OfficeSpace Software if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="848b1-238">Pokud potřebujete toocreate uživatelé ručně, je nutné tooContact [tým podpory OfficeSpace softwaru](mailto:support@officespacesoftware.com).</span><span class="sxs-lookup"><span data-stu-id="848b1-238">If you need toocreate an user manually, you need tooContact [OfficeSpace Software support team](mailto:support@officespacesoftware.com).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="848b1-239">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="848b1-239">Assign hello Azure AD test user</span></span>

<span data-ttu-id="848b1-240">V této části povolíte tak, že udělíte přístup tooOfficeSpace softwaru Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="848b1-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooOfficeSpace Software.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="848b1-242">**tooassign Britta Simon tooOfficeSpace softwaru, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="848b1-242">**tooassign Britta Simon tooOfficeSpace Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="848b1-243">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="848b1-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="848b1-245">V seznamu aplikace hello vyberte **OfficeSpace softwaru**.</span><span class="sxs-lookup"><span data-stu-id="848b1-245">In hello applications list, select **OfficeSpace Software**.</span></span>

    ![Hello odkaz na OfficeSpace Software v seznamu aplikace hello](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_app.png)  

3. <span data-ttu-id="848b1-247">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="848b1-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="848b1-249">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="848b1-249">Click **Add** button.</span></span> <span data-ttu-id="848b1-250">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="848b1-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="848b1-252">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="848b1-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="848b1-253">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="848b1-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="848b1-254">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="848b1-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="848b1-255">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="848b1-255">Test single sign-on</span></span>

<span data-ttu-id="848b1-256">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="848b1-256">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="848b1-257">Po kliknutí na tlačítko hello OfficeSpace softwaru dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour OfficeSpace softwarová aplikace.</span><span class="sxs-lookup"><span data-stu-id="848b1-257">When you click hello OfficeSpace Software tile in hello Access Panel, you should get automatically signed-on tooyour OfficeSpace Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="848b1-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="848b1-258">Additional resources</span></span>

* [<span data-ttu-id="848b1-259">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="848b1-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="848b1-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="848b1-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_203.png

