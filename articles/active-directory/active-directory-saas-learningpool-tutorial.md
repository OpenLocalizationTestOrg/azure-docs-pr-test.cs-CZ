---
title: 'Kurz: Azure Active Directory integrace s Learningpool Act | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Learningpool akce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 51e8695f-31e1-4d09-8eb3-13241999d99f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 932f5f12c75299e532d3fa2c31f1805a7df30158
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learningpool-act"></a><span data-ttu-id="01b5f-103">Kurz: Azure Active Directory integrace s Learningpool Act</span><span class="sxs-lookup"><span data-stu-id="01b5f-103">Tutorial: Azure Active Directory integration with Learningpool Act</span></span>

<span data-ttu-id="01b5f-104">V tomto kurzu zjistěte, jak integrovat Learningpool Act s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="01b5f-104">In this tutorial, you learn how to integrate Learningpool Act with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="01b5f-105">Integrace s Azure AD Application Compatibility Toolkit Learningpool poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="01b5f-105">Integrating Learningpool Act with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="01b5f-106">Můžete řídit ve službě Azure AD, který má přístup k Learningpool Act</span><span class="sxs-lookup"><span data-stu-id="01b5f-106">You can control in Azure AD who has access to Learningpool Act</span></span>
- <span data-ttu-id="01b5f-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Learningpool Act (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="01b5f-107">You can enable your users to automatically get signed-on to Learningpool Act (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="01b5f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="01b5f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="01b5f-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="01b5f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01b5f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="01b5f-110">Prerequisites</span></span>

<span data-ttu-id="01b5f-111">Konfigurace integrace Azure AD s Learningpool Application Compatibility Toolkit, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="01b5f-111">To configure Azure AD integration with Learningpool Act, you need the following items:</span></span>

- <span data-ttu-id="01b5f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="01b5f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="01b5f-113">Learningpool Act jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="01b5f-113">A Learningpool Act single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="01b5f-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="01b5f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="01b5f-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="01b5f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="01b5f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="01b5f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="01b5f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="01b5f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="01b5f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="01b5f-118">Scenario description</span></span>
<span data-ttu-id="01b5f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="01b5f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="01b5f-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="01b5f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="01b5f-121">Přidání Application Compatibility Toolkit Learningpool z Galerie</span><span class="sxs-lookup"><span data-stu-id="01b5f-121">Adding Learningpool Act from the gallery</span></span>
2. <span data-ttu-id="01b5f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="01b5f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learningpool-act-from-the-gallery"></a><span data-ttu-id="01b5f-123">Přidání Application Compatibility Toolkit Learningpool z Galerie</span><span class="sxs-lookup"><span data-stu-id="01b5f-123">Adding Learningpool Act from the gallery</span></span>
<span data-ttu-id="01b5f-124">Při konfiguraci integrace Learningpool Act do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Learningpool Act z galerie.</span><span class="sxs-lookup"><span data-stu-id="01b5f-124">To configure the integration of Learningpool Act into Azure AD, you need to add Learningpool Act from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="01b5f-125">**Pokud chcete přidat Learningpool Act z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="01b5f-125">**To add Learningpool Act from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="01b5f-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="01b5f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="01b5f-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="01b5f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="01b5f-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="01b5f-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="01b5f-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01b5f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="01b5f-133">Do vyhledávacího pole zadejte **Learningpool Act**.</span><span class="sxs-lookup"><span data-stu-id="01b5f-133">In the search box, type **Learningpool Act**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_search.png)

5. <span data-ttu-id="01b5f-135">Na panelu výsledků vyberte **Learningpool Act**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01b5f-135">In the results panel, select **Learningpool Act**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="01b5f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="01b5f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="01b5f-138">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s Learningpool Act podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="01b5f-138">In this section, you configure and test Azure AD single sign-on with Learningpool Act based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="01b5f-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Learningpool Act je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="01b5f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Learningpool Act is to a user in Azure AD.</span></span> <span data-ttu-id="01b5f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Learningpool Act musí navázat.</span><span class="sxs-lookup"><span data-stu-id="01b5f-140">In other words, a link relationship between an Azure AD user and the related user in Learningpool Act needs to be established.</span></span>

<span data-ttu-id="01b5f-141">V Learningpool Act přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="01b5f-141">In Learningpool Act, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="01b5f-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Learningpool akce, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="01b5f-142">To configure and test Azure AD single sign-on with Learningpool Act, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="01b5f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="01b5f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="01b5f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="01b5f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="01b5f-145">**[Vytvoření zkušebního uživatele Learningpool Act](#creating-a-learningpool-act-test-user)**  – Pokud chcete mít protějšek Britta Simon v Learningpool Application Compatibility Toolkit, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="01b5f-145">**[Creating a Learningpool Act test user](#creating-a-learningpool-act-test-user)** - to have a counterpart of Britta Simon in Learningpool Act that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="01b5f-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="01b5f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="01b5f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="01b5f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="01b5f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="01b5f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="01b5f-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Learningpool akce.</span><span class="sxs-lookup"><span data-stu-id="01b5f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Learningpool Act application.</span></span>

<span data-ttu-id="01b5f-150">**Ke konfiguraci Azure AD jednotné přihlašování s Learningpool Application Compatibility Toolkit, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="01b5f-150">**To configure Azure AD single sign-on with Learningpool Act, perform the following steps:**</span></span>

1. <span data-ttu-id="01b5f-151">Na portálu Azure na **Learningpool Act** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="01b5f-151">In the Azure portal, on the **Learningpool Act** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="01b5f-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="01b5f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_samlbase.png)

3. <span data-ttu-id="01b5f-155">Na **Learningpool Act domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="01b5f-155">On the **Learningpool Act Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_url.png)

    <span data-ttu-id="01b5f-157">a.</span><span class="sxs-lookup"><span data-stu-id="01b5f-157">a.</span></span> <span data-ttu-id="01b5f-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL:`https://parliament.preview.Learningpool.com/auth/shibboleth/index.php`</span><span class="sxs-lookup"><span data-stu-id="01b5f-158">In the **Sign-on URL** textbox, type the URL: `https://parliament.preview.Learningpool.com/auth/shibboleth/index.php`</span></span>

    <span data-ttu-id="01b5f-159">b.</span><span class="sxs-lookup"><span data-stu-id="01b5f-159">b.</span></span> <span data-ttu-id="01b5f-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="01b5f-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<subdomain>.Learningpool.com/shibboleth` |
    | `https://<subdomain>.preview.Learningpool.com/shibboleth` |

    > [!NOTE] 
    > <span data-ttu-id="01b5f-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="01b5f-161">These values are not real.</span></span> <span data-ttu-id="01b5f-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="01b5f-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="01b5f-163">Obraťte se na [tým podpory Learningpool Act klienta](https://www.Learningpool.com/support) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="01b5f-163">Contact [Learningpool Act Client support team](https://www.Learningpool.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="01b5f-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="01b5f-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_certificate.png) 

5. <span data-ttu-id="01b5f-166">Aplikace Learningpool Act očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="01b5f-166">Learningpool Act application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="01b5f-167">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01b5f-167">Please configure the following claims for this application.</span></span> <span data-ttu-id="01b5f-168">Můžete spravovat hodnoty těchto atributů z **"Atrribute"** aplikace.</span><span class="sxs-lookup"><span data-stu-id="01b5f-168">You can manage the values of these attributes from the **"Atrribute"** tab of the application.</span></span> <span data-ttu-id="01b5f-169">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="01b5f-169">The following screenshot shows an example for this.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_attribute.png) 

6. <span data-ttu-id="01b5f-171">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="01b5f-171">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="01b5f-172">Název atributu</span><span class="sxs-lookup"><span data-stu-id="01b5f-172">Attribute Name</span></span> | <span data-ttu-id="01b5f-173">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="01b5f-173">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="01b5f-174">název urn: oid:1.2.840.113556.1.4.221</span><span class="sxs-lookup"><span data-stu-id="01b5f-174">urn:oid:1.2.840.113556.1.4.221</span></span> | <span data-ttu-id="01b5f-175">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="01b5f-175">user.userprincipalname</span></span> |
    | <span data-ttu-id="01b5f-176">název urn: oid:2.5.4.42</span><span class="sxs-lookup"><span data-stu-id="01b5f-176">urn:oid:2.5.4.42</span></span> | <span data-ttu-id="01b5f-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="01b5f-177">user.givenname</span></span> |
    | <span data-ttu-id="01b5f-178">název urn: oid:0.9.2342.19200300.100.1.3</span><span class="sxs-lookup"><span data-stu-id="01b5f-178">urn:oid:0.9.2342.19200300.100.1.3</span></span> | <span data-ttu-id="01b5f-179">User.Mail</span><span class="sxs-lookup"><span data-stu-id="01b5f-179">user.mail</span></span> |    
    | <span data-ttu-id="01b5f-180">název urn: oid:2.5.4.4</span><span class="sxs-lookup"><span data-stu-id="01b5f-180">urn:oid:2.5.4.4</span></span> | <span data-ttu-id="01b5f-181">User.Surname</span><span class="sxs-lookup"><span data-stu-id="01b5f-181">user.surname</span></span> |
    
    <span data-ttu-id="01b5f-182">a.</span><span class="sxs-lookup"><span data-stu-id="01b5f-182">a.</span></span> <span data-ttu-id="01b5f-183">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01b5f-183">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="01b5f-186">b.</span><span class="sxs-lookup"><span data-stu-id="01b5f-186">b.</span></span> <span data-ttu-id="01b5f-187">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="01b5f-187">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="01b5f-188">c.</span><span class="sxs-lookup"><span data-stu-id="01b5f-188">c.</span></span> <span data-ttu-id="01b5f-189">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="01b5f-189">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="01b5f-190">d.</span><span class="sxs-lookup"><span data-stu-id="01b5f-190">d.</span></span> <span data-ttu-id="01b5f-191">Ponechte **Namespace** prázdné.</span><span class="sxs-lookup"><span data-stu-id="01b5f-191">Leave the **Namespace** blank.</span></span>
    
    <span data-ttu-id="01b5f-192">e.</span><span class="sxs-lookup"><span data-stu-id="01b5f-192">e.</span></span> <span data-ttu-id="01b5f-193">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="01b5f-193">Click **Ok**.</span></span>

7. <span data-ttu-id="01b5f-194">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="01b5f-194">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="01b5f-196">Konfigurace jednotného přihlašování na **Learningpool Act** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Learningpool Act](https://www.Learningpool.com/support).</span><span class="sxs-lookup"><span data-stu-id="01b5f-196">To configure single sign-on on **Learningpool Act** side, you need to send the downloaded **Metadata XML** to [Learningpool Act support team](https://www.Learningpool.com/support).</span></span> <span data-ttu-id="01b5f-197">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="01b5f-197">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="01b5f-198">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="01b5f-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="01b5f-199">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="01b5f-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="01b5f-200">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="01b5f-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="01b5f-201">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="01b5f-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="01b5f-202">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="01b5f-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="01b5f-204">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="01b5f-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="01b5f-205">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="01b5f-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="01b5f-207">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="01b5f-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="01b5f-209">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01b5f-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="01b5f-211">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="01b5f-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="01b5f-213">a.</span><span class="sxs-lookup"><span data-stu-id="01b5f-213">a.</span></span> <span data-ttu-id="01b5f-214">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="01b5f-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="01b5f-215">b.</span><span class="sxs-lookup"><span data-stu-id="01b5f-215">b.</span></span> <span data-ttu-id="01b5f-216">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="01b5f-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="01b5f-217">c.</span><span class="sxs-lookup"><span data-stu-id="01b5f-217">c.</span></span> <span data-ttu-id="01b5f-218">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="01b5f-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="01b5f-219">d.</span><span class="sxs-lookup"><span data-stu-id="01b5f-219">d.</span></span> <span data-ttu-id="01b5f-220">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01b5f-220">Click **Create**.</span></span>
 
### <a name="creating-a-learningpool-act-test-user"></a><span data-ttu-id="01b5f-221">Vytvoření zkušebního uživatele Learningpool Act</span><span class="sxs-lookup"><span data-stu-id="01b5f-221">Creating a Learningpool Act test user</span></span>

<span data-ttu-id="01b5f-222">Povolit uživatelům Azure AD přihlášení k Learningpool Application Compatibility Toolkit, musí být zřízená do Learningpool akce.</span><span class="sxs-lookup"><span data-stu-id="01b5f-222">To enable Azure AD users to log in to Learningpool Act, they must be provisioned into Learningpool Act.</span></span>

<span data-ttu-id="01b5f-223">Neexistuje žádná položka akce můžete nakonfigurovat zřizování na základě možnosti jednat Learningpool uživatelů.</span><span class="sxs-lookup"><span data-stu-id="01b5f-223">There is no action item for you to configure user provisioning to Learningpool Act.</span></span>  
<span data-ttu-id="01b5f-224">Uživatelé musí být vytvořený vaše [tým podpory Learningpool Act](https://www.Learningpool.com/support).</span><span class="sxs-lookup"><span data-stu-id="01b5f-224">Users need to be created by your [Learningpool Act support team](https://www.Learningpool.com/support).</span></span>

>[!NOTE]
><span data-ttu-id="01b5f-225">Můžete použít všechny ostatní Learningpool Act uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Learningpool Act zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="01b5f-225">You can use any other Learningpool Act user account creation tools or APIs provided by Learningpool Act to provision AAD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="01b5f-226">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="01b5f-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="01b5f-227">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Learningpool akce.</span><span class="sxs-lookup"><span data-stu-id="01b5f-227">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Learningpool Act.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="01b5f-229">**Pokud chcete přiřadit Britta Simon Learningpool Application Compatibility Toolkit, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="01b5f-229">**To assign Britta Simon to Learningpool Act, perform the following steps:**</span></span>

1. <span data-ttu-id="01b5f-230">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="01b5f-230">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="01b5f-232">V seznamu aplikací vyberte **Learningpool Act**.</span><span class="sxs-lookup"><span data-stu-id="01b5f-232">In the applications list, select **Learningpool Act**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_app.png) 

3. <span data-ttu-id="01b5f-234">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="01b5f-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="01b5f-236">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="01b5f-236">Click **Add** button.</span></span> <span data-ttu-id="01b5f-237">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01b5f-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="01b5f-239">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="01b5f-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="01b5f-240">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01b5f-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="01b5f-241">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01b5f-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="01b5f-242">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="01b5f-242">Testing single sign-on</span></span>

<span data-ttu-id="01b5f-243">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="01b5f-243">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="01b5f-244">Když kliknete na dlaždici Learningpool Act na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Learningpool akce.</span><span class="sxs-lookup"><span data-stu-id="01b5f-244">When you click the Learningpool Act tile in the Access Panel, you should get automatically signed-on to your Learningpool Act application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01b5f-245">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="01b5f-245">Additional resources</span></span>

* [<span data-ttu-id="01b5f-246">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="01b5f-246">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="01b5f-247">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="01b5f-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_203.png
