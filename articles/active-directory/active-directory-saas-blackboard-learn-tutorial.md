---
title: "Kurz: Azure Active Directory integrace s další Tabule | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a další Tabule."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0b8ca505-61ea-487c-9a3e-fa50c936df0c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: jeedes
ms.openlocfilehash: c2b7638e99fa46ff41a7f2202bdf0e7a2b017c19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn"></a><span data-ttu-id="1d48e-103">Kurz: Azure Active Directory integrace s další Tabule</span><span class="sxs-lookup"><span data-stu-id="1d48e-103">Tutorial: Azure Active Directory integration with Blackboard Learn</span></span>

<span data-ttu-id="1d48e-104">V tomto kurzu zjistěte, jak integrovat další tabule s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1d48e-104">In this tutorial, you learn how to integrate Blackboard Learn with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1d48e-105">Integrace Další tabule s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="1d48e-105">Integrating Blackboard Learn with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1d48e-106">Můžete řídit ve službě Azure AD, který má přístup k další Tabule</span><span class="sxs-lookup"><span data-stu-id="1d48e-106">You can control in Azure AD who has access to Blackboard Learn</span></span>
- <span data-ttu-id="1d48e-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k další Tabule (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="1d48e-107">You can enable your users to automatically get signed-on to Blackboard Learn (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1d48e-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1d48e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="1d48e-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1d48e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d48e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1d48e-110">Prerequisites</span></span>

<span data-ttu-id="1d48e-111">Konfigurace integrace Azure AD s další Tabule, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="1d48e-111">To configure Azure AD integration with Blackboard Learn, you need the following items:</span></span>

- <span data-ttu-id="1d48e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="1d48e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1d48e-113">Další tabule jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="1d48e-113">A Blackboard Learn single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1d48e-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1d48e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1d48e-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="1d48e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1d48e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="1d48e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1d48e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1d48e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1d48e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="1d48e-118">Scenario description</span></span>
<span data-ttu-id="1d48e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="1d48e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1d48e-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="1d48e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1d48e-121">Přidání další Tabule z Galerie</span><span class="sxs-lookup"><span data-stu-id="1d48e-121">Adding Blackboard Learn from the gallery</span></span>
2. <span data-ttu-id="1d48e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1d48e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-blackboard-learn-from-the-gallery"></a><span data-ttu-id="1d48e-123">Přidání další Tabule z Galerie</span><span class="sxs-lookup"><span data-stu-id="1d48e-123">Adding Blackboard Learn from the gallery</span></span>
<span data-ttu-id="1d48e-124">Chcete-li nakonfigurovat integraci Tabule informace do služby Azure AD, přidejte další Tabule z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="1d48e-124">To configure the integration of Blackboard Learn into Azure AD, you need to add Blackboard Learn from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1d48e-125">**Pokud chcete přidat další Tabule z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1d48e-125">**To add Blackboard Learn from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1d48e-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1d48e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1d48e-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="1d48e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1d48e-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1d48e-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="1d48e-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1d48e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="1d48e-133">Do vyhledávacího pole zadejte **další Tabule**.</span><span class="sxs-lookup"><span data-stu-id="1d48e-133">In the search box, type **Blackboard Learn**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_search.png)

5. <span data-ttu-id="1d48e-135">Na panelu výsledků vyberte **další Tabule**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1d48e-135">In the results panel, select **Blackboard Learn**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1d48e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1d48e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1d48e-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Tabule další podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="1d48e-138">In this section, you configure and test Azure AD single sign-on with Blackboard Learn based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="1d48e-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v další Tabule je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1d48e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Blackboard Learn is to a user in Azure AD.</span></span> <span data-ttu-id="1d48e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v další Tabule musí navázat.</span><span class="sxs-lookup"><span data-stu-id="1d48e-140">In other words, a link relationship between an Azure AD user and the related user in Blackboard Learn needs to be established.</span></span>

<span data-ttu-id="1d48e-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Tabule Další.</span><span class="sxs-lookup"><span data-stu-id="1d48e-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Blackboard Learn.</span></span>

<span data-ttu-id="1d48e-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s další Tabule, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="1d48e-142">To configure and test Azure AD single sign-on with Blackboard Learn, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1d48e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="1d48e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1d48e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1d48e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1d48e-145">**[Vytvoření zkušebního uživatele další Tabule](#creating-a-blackboard-learn-test-user)**  – Pokud chcete mít protějšek Britta Simon v Tabule další propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="1d48e-145">**[Creating a Blackboard Learn test user](#creating-a-blackboard-learn-test-user)** - to have a counterpart of Britta Simon in Blackboard Learn that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1d48e-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1d48e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1d48e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1d48e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1d48e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1d48e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1d48e-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Tabule Další.</span><span class="sxs-lookup"><span data-stu-id="1d48e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Blackboard Learn application.</span></span>

<span data-ttu-id="1d48e-150">**Ke konfiguraci Azure AD jednotné přihlašování s další Tabule, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1d48e-150">**To configure Azure AD single sign-on with Blackboard Learn, perform the following steps:**</span></span>

1. <span data-ttu-id="1d48e-151">Na portálu Azure na **další Tabule** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="1d48e-151">In the Azure portal, on the **Blackboard Learn** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="1d48e-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1d48e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_samlbase.png)

3. <span data-ttu-id="1d48e-155">Na **Tabule další domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1d48e-155">On the **Blackboard Learn Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_url.png)

    <span data-ttu-id="1d48e-157">a.</span><span class="sxs-lookup"><span data-stu-id="1d48e-157">a.</span></span> <span data-ttu-id="1d48e-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.blackboard.com/`</span><span class="sxs-lookup"><span data-stu-id="1d48e-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.blackboard.com/`</span></span>

    <span data-ttu-id="1d48e-159">b.</span><span class="sxs-lookup"><span data-stu-id="1d48e-159">b.</span></span> <span data-ttu-id="1d48e-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.blackboard.com/auth-saml/saml/SSO/entity-id/SAML_AD`</span><span class="sxs-lookup"><span data-stu-id="1d48e-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.blackboard.com/auth-saml/saml/SSO/entity-id/SAML_AD`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="1d48e-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="1d48e-161">These values are not real.</span></span> <span data-ttu-id="1d48e-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="1d48e-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="1d48e-163">Obraťte se na [tým podpory pro další klienta Tabule](https://www.blackboard.com/support/index.aspx) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="1d48e-163">Contact [Blackboard Learn Client support team](https://www.blackboard.com/support/index.aspx) to get these values.</span></span> 

4. <span data-ttu-id="1d48e-164">Další aplikace Tabule očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="1d48e-164">Blackboard Learn application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="1d48e-165">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1d48e-165">Configure the following claims for this application.</span></span> <span data-ttu-id="1d48e-166">Můžete spravovat hodnoty těchto atributů z **uživatelské atributy** části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d48e-166">You can manage the values of these attributes from the **User Attributes** section on application integration page.</span></span>
 <span data-ttu-id="1d48e-167">Následující snímek obrazovky ukazuje příklad o něm.</span><span class="sxs-lookup"><span data-stu-id="1d48e-167">The following screenshot shows an example about it.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute.png)

5. <span data-ttu-id="1d48e-169">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, konfigurovat atributy tokenu SAML, jak je znázorněno na obrázku a proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="1d48e-169">In the **User Attributes** section on **Single sign-on** dialog, configure SAML token attributes as shown in the image and perform the following steps.</span></span> <span data-ttu-id="1d48e-170">Userprincipalname budeme mít mapovaná jako atribut jedinečného uživatelského ale můžete ji namapovat na odpovídající hodnotu, která jednoznačně rozlišující uživatel v organizaci a která se mapuje Tabule další pole pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="1d48e-170">We have mapped the Userprincipalname as the unique user attribute here but you can map it to the appropriate value, which uniquely distinguishes the user in the organization and that maps to Blackboard Learn username field.</span></span>
           
    | <span data-ttu-id="1d48e-171">Název atributu</span><span class="sxs-lookup"><span data-stu-id="1d48e-171">Attribute Name</span></span> | <span data-ttu-id="1d48e-172">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="1d48e-172">Attribute Value</span></span> |   
    | ---------------| ----------------|
    | <span data-ttu-id="1d48e-173">urn:oid:1.3.6.1.4.1.5923.1.1.1.6</span><span class="sxs-lookup"><span data-stu-id="1d48e-173">urn:oid:1.3.6.1.4.1.5923.1.1.1.6</span></span> |<span data-ttu-id="1d48e-174">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="1d48e-174">user.userprincipalname</span></span> |

    <span data-ttu-id="1d48e-175">a.</span><span class="sxs-lookup"><span data-stu-id="1d48e-175">a.</span></span> <span data-ttu-id="1d48e-176">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1d48e-176">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="1d48e-179">b.</span><span class="sxs-lookup"><span data-stu-id="1d48e-179">b.</span></span> <span data-ttu-id="1d48e-180">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="1d48e-180">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="1d48e-181">c.</span><span class="sxs-lookup"><span data-stu-id="1d48e-181">c.</span></span> <span data-ttu-id="1d48e-182">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="1d48e-182">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="1d48e-183">d.</span><span class="sxs-lookup"><span data-stu-id="1d48e-183">d.</span></span> <span data-ttu-id="1d48e-184">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1d48e-184">Click **Ok**.</span></span>

4. <span data-ttu-id="1d48e-185">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="1d48e-185">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_certificate.png)

7. <span data-ttu-id="1d48e-187">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1d48e-187">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="1d48e-189">Na **Tabule další konfigurace** klikněte na tlačítko **nakonfigurovat další Tabule** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="1d48e-189">On the **Blackboard Learn Configuration** section, click **Configure Blackboard Learn** to open **Configure sign-on** window.</span></span> <span data-ttu-id="1d48e-190">Kopírování **SAML Entity ID** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="1d48e-190">Copy the **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_configure.png) 

9. <span data-ttu-id="1d48e-192">Konfigurace jednotného přihlašování na **další Tabule** straně, budete muset odeslat stažené **soubor XML s metadaty** a **SAML Entity ID** k [další Tabule Podpora](https://www.blackboard.com/support/index.aspx).</span><span class="sxs-lookup"><span data-stu-id="1d48e-192">To configure single sign-on on **Blackboard Learn** side, you need to send the downloaded **Metadata XML** and **SAML Entity ID** to [Blackboard Learn support](https://www.blackboard.com/support/index.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="1d48e-193">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="1d48e-193">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1d48e-194">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="1d48e-194">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1d48e-195">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1d48e-195">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1d48e-196">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="1d48e-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="1d48e-197">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1d48e-197">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="1d48e-199">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1d48e-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1d48e-200">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1d48e-200">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1d48e-202">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="1d48e-202">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1d48e-204">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1d48e-204">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1d48e-206">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1d48e-206">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1d48e-208">a.</span><span class="sxs-lookup"><span data-stu-id="1d48e-208">a.</span></span> <span data-ttu-id="1d48e-209">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1d48e-209">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1d48e-210">b.</span><span class="sxs-lookup"><span data-stu-id="1d48e-210">b.</span></span> <span data-ttu-id="1d48e-211">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1d48e-211">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="1d48e-212">c.</span><span class="sxs-lookup"><span data-stu-id="1d48e-212">c.</span></span> <span data-ttu-id="1d48e-213">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="1d48e-213">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="1d48e-214">d.</span><span class="sxs-lookup"><span data-stu-id="1d48e-214">d.</span></span> <span data-ttu-id="1d48e-215">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1d48e-215">Click **Create**.</span></span>
 
### <a name="creating-a-blackboard-learn-test-user"></a><span data-ttu-id="1d48e-216">Vytvoření zkušebního uživatele další Tabule</span><span class="sxs-lookup"><span data-stu-id="1d48e-216">Creating a Blackboard Learn test user</span></span>
<span data-ttu-id="1d48e-217">V této části vytvoříte volal Britta Simon v Tabule další uživatele.</span><span class="sxs-lookup"><span data-stu-id="1d48e-217">In this section, you create a user called Britta Simon in Blackboard Learn.</span></span> 

<span data-ttu-id="1d48e-218">Tabule další aplikace podporují jenom při zřizování uživatelů čas.</span><span class="sxs-lookup"><span data-stu-id="1d48e-218">Blackboard Learn application support just in time user provisioning.</span></span> <span data-ttu-id="1d48e-219">Ujistěte se, že jste nakonfigurovali deklarace identity, jak je popsáno v části  **[konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**</span><span class="sxs-lookup"><span data-stu-id="1d48e-219">Make sure that you have configured the claims as described in the section **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**</span></span>
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="1d48e-220">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="1d48e-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="1d48e-221">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Tabule Další.</span><span class="sxs-lookup"><span data-stu-id="1d48e-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Blackboard Learn.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="1d48e-223">**Přiřadit Britta Simon další Tabule, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1d48e-223">**To assign Britta Simon to Blackboard Learn, perform the following steps:**</span></span>

1. <span data-ttu-id="1d48e-224">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1d48e-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="1d48e-226">V seznamu aplikací vyberte **další Tabule**.</span><span class="sxs-lookup"><span data-stu-id="1d48e-226">In the applications list, select **Blackboard Learn**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_app.png) 

3. <span data-ttu-id="1d48e-228">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="1d48e-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="1d48e-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1d48e-230">Click **Add** button.</span></span> <span data-ttu-id="1d48e-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1d48e-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="1d48e-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1d48e-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1d48e-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1d48e-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1d48e-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1d48e-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1d48e-236">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1d48e-236">Testing single sign-on</span></span>

<span data-ttu-id="1d48e-237">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="1d48e-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="1d48e-238">Po kliknutí na tlačítko Další tabule dlaždice na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Tabule Další.</span><span class="sxs-lookup"><span data-stu-id="1d48e-238">When you click the Blackboard Learn tile in the Access Panel, you should get automatically signed-on to your Blackboard Learn application.</span></span> <span data-ttu-id="1d48e-239">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1d48e-239">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="1d48e-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1d48e-240">Additional resources</span></span>

* [<span data-ttu-id="1d48e-241">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1d48e-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1d48e-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1d48e-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_203.png

