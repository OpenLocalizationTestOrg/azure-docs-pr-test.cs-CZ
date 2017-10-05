---
title: 'Kurz: Azure Active Directory integrace s Promapp | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Promapp."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 418d0601-6e7a-4997-a683-73fa30a2cfb5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: jeedes
ms.openlocfilehash: 27013ca9724cf2f57fc85f5f4ccb71921ca57a3b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-promapp"></a><span data-ttu-id="52dc7-103">Kurz: Azure Active Directory integrace s Promapp</span><span class="sxs-lookup"><span data-stu-id="52dc7-103">Tutorial: Azure Active Directory integration with Promapp</span></span>

<span data-ttu-id="52dc7-104">V tomto kurzu zjistěte, jak integrovat Promapp s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="52dc7-104">In this tutorial, you learn how to integrate Promapp with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="52dc7-105">Integrace Promapp s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="52dc7-105">Integrating Promapp with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="52dc7-106">Můžete řídit ve službě Azure AD, který má přístup k Promapp</span><span class="sxs-lookup"><span data-stu-id="52dc7-106">You can control in Azure AD who has access to Promapp</span></span>
- <span data-ttu-id="52dc7-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Promapp (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="52dc7-107">You can enable your users to automatically get signed-on to Promapp (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="52dc7-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="52dc7-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="52dc7-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="52dc7-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52dc7-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="52dc7-110">Prerequisites</span></span>

<span data-ttu-id="52dc7-111">Konfigurace integrace Azure AD s Promapp, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="52dc7-111">To configure Azure AD integration with Promapp, you need the following items:</span></span>

- <span data-ttu-id="52dc7-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="52dc7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="52dc7-113">Promapp jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="52dc7-113">A Promapp single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="52dc7-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="52dc7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="52dc7-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="52dc7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="52dc7-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="52dc7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="52dc7-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="52dc7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="52dc7-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="52dc7-118">Scenario description</span></span>
<span data-ttu-id="52dc7-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="52dc7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="52dc7-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="52dc7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="52dc7-121">Přidání Promapp z Galerie</span><span class="sxs-lookup"><span data-stu-id="52dc7-121">Adding Promapp from the gallery</span></span>
2. <span data-ttu-id="52dc7-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="52dc7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-promapp-from-the-gallery"></a><span data-ttu-id="52dc7-123">Přidání Promapp z Galerie</span><span class="sxs-lookup"><span data-stu-id="52dc7-123">Adding Promapp from the gallery</span></span>
<span data-ttu-id="52dc7-124">Při konfiguraci integrace Promapp do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Promapp z galerie.</span><span class="sxs-lookup"><span data-stu-id="52dc7-124">To configure the integration of Promapp into Azure AD, you need to add Promapp from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="52dc7-125">**Pokud chcete přidat Promapp z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="52dc7-125">**To add Promapp from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="52dc7-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="52dc7-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="52dc7-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="52dc7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="52dc7-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="52dc7-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="52dc7-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="52dc7-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="52dc7-133">Do vyhledávacího pole zadejte **Promapp**.</span><span class="sxs-lookup"><span data-stu-id="52dc7-133">In the search box, type **Promapp**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_search.png)

5. <span data-ttu-id="52dc7-135">Na panelu výsledků vyberte **Promapp**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="52dc7-135">In the results panel, select **Promapp**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="52dc7-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="52dc7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="52dc7-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Promapp podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="52dc7-138">In this section, you configure and test Azure AD single sign-on with Promapp based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="52dc7-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Promapp je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="52dc7-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Promapp is to a user in Azure AD.</span></span> <span data-ttu-id="52dc7-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Promapp musí navázat.</span><span class="sxs-lookup"><span data-stu-id="52dc7-140">In other words, a link relationship between an Azure AD user and the related user in Promapp needs to be established.</span></span>

<span data-ttu-id="52dc7-141">V Promapp, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="52dc7-141">In Promapp, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="52dc7-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Promapp, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="52dc7-142">To configure and test Azure AD single sign-on with Promapp, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="52dc7-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="52dc7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="52dc7-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="52dc7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="52dc7-145">**[Vytvoření zkušebního uživatele Promapp](#creating-a-promapp-test-user)**  – Pokud chcete mít protějšek Britta Simon v Promapp propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="52dc7-145">**[Creating a Promapp test user](#creating-a-promapp-test-user)** - to have a counterpart of Britta Simon in Promapp that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="52dc7-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="52dc7-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="52dc7-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="52dc7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="52dc7-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="52dc7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="52dc7-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Promapp.</span><span class="sxs-lookup"><span data-stu-id="52dc7-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Promapp application.</span></span>

<span data-ttu-id="52dc7-150">**Ke konfiguraci Azure AD jednotné přihlašování s Promapp, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="52dc7-150">**To configure Azure AD single sign-on with Promapp, perform the following steps:**</span></span>

1. <span data-ttu-id="52dc7-151">Na portálu Azure na **Promapp** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="52dc7-151">In the Azure portal, on the **Promapp** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="52dc7-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="52dc7-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_samlbase.png)

3. <span data-ttu-id="52dc7-155">Na **Promapp domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="52dc7-155">On the **Promapp Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_url.png)

    <span data-ttu-id="52dc7-157">a.</span><span class="sxs-lookup"><span data-stu-id="52dc7-157">a.</span></span> <span data-ttu-id="52dc7-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://DOMAINNAME.promapp.com/TENANTNAME/saml/authenticate`</span><span class="sxs-lookup"><span data-stu-id="52dc7-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://DOMAINNAME.promapp.com/TENANTNAME/saml/authenticate`</span></span>

    <span data-ttu-id="52dc7-159">b.</span><span class="sxs-lookup"><span data-stu-id="52dc7-159">b.</span></span> <span data-ttu-id="52dc7-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://DOMAINNAME.promapp.com/TENANTNAME`</span><span class="sxs-lookup"><span data-stu-id="52dc7-160">In the **Identifier** textbox, type a URL using the following pattern: `https://DOMAINNAME.promapp.com/TENANTNAME`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="52dc7-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="52dc7-161">These values are not real.</span></span> <span data-ttu-id="52dc7-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="52dc7-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="52dc7-163">Obraťte se na [tým podpory Promapp klienta](https://www.promapp.com/about-us/contact-us/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="52dc7-163">Contact [Promapp Client support team](https://www.promapp.com/about-us/contact-us/) to get these values.</span></span>

4. <span data-ttu-id="52dc7-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="52dc7-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_certificate.png) 

5. <span data-ttu-id="52dc7-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="52dc7-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-promapp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="52dc7-168">Na **Promapp konfigurace** klikněte na tlačítko **konfigurace Promapp** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="52dc7-168">On the **Promapp Configuration** section, click **Configure Promapp** to open **Configure sign-on** window.</span></span> <span data-ttu-id="52dc7-169">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="52dc7-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_configure.png) 

7. <span data-ttu-id="52dc7-171">Přihlašování k webu společnosti Promapp jako správce.</span><span class="sxs-lookup"><span data-stu-id="52dc7-171">Sign-on to your Promapp company site as administrator.</span></span> 

8. <span data-ttu-id="52dc7-172">V nabídce v horní části, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="52dc7-172">In the menu on the top, click **Admin**.</span></span> 
   
    ![Azure AD jednotné přihlášení][12]

9. <span data-ttu-id="52dc7-174">Klikněte na **Konfigurovat**.</span><span class="sxs-lookup"><span data-stu-id="52dc7-174">Click **Configure**.</span></span> 
   
    ![Azure AD jednotné přihlášení][13]

10. <span data-ttu-id="52dc7-176">Na **zabezpečení** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="52dc7-176">On the **Security** dialog, perform the following steps:</span></span>
   
    ![Azure AD jednotné přihlášení][14]
    
    <span data-ttu-id="52dc7-178">a.</span><span class="sxs-lookup"><span data-stu-id="52dc7-178">a.</span></span> <span data-ttu-id="52dc7-179">Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure do **adresu URL pro přihlášení SSO** textové pole.</span><span class="sxs-lookup"><span data-stu-id="52dc7-179">Paste **SAML Single Sign-On Service URL**, which you have copied from the Azure portal into the **SSO-Login URL** textbox.</span></span>
    
    <span data-ttu-id="52dc7-180">b.</span><span class="sxs-lookup"><span data-stu-id="52dc7-180">b.</span></span> <span data-ttu-id="52dc7-181">Jako **jednotné přihlašování – jednotné přihlašování v režimu**, vyberte **volitelné**a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="52dc7-181">As **SSO - Single Sign-on Mode**, select **Optional**, and then click **Save**.</span></span>

    <span data-ttu-id="52dc7-182">c.</span><span class="sxs-lookup"><span data-stu-id="52dc7-182">c.</span></span> <span data-ttu-id="52dc7-183">Otevřete stažený certifikát v poznámkovém bloku kopírování obsahu certifikátu bez první řádek (---BEGIN CERTIFICATE---) a poslední řádek (---END CERTIFICATE---), vložte jej do **certifikát x.509 jednotného přihlašování k** textovému poli a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="52dc7-183">Open the downloaded certificate in notepad, copy the certificate content without the first line (-----BEGIN CERTIFICATE-----) and the last line (-----END CERTIFICATE-----), paste it into the **SSO-x.509 Certificate** textbox, and then click **Save**.</span></span>
        
> [!TIP]
> <span data-ttu-id="52dc7-184">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="52dc7-184">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="52dc7-185">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="52dc7-185">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="52dc7-186">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="52dc7-186">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="52dc7-187">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="52dc7-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="52dc7-188">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="52dc7-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="52dc7-190">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="52dc7-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="52dc7-191">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="52dc7-191">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-promapp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="52dc7-193">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="52dc7-193">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-promapp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="52dc7-195">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="52dc7-195">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-promapp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="52dc7-197">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="52dc7-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-promapp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="52dc7-199">a.</span><span class="sxs-lookup"><span data-stu-id="52dc7-199">a.</span></span> <span data-ttu-id="52dc7-200">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="52dc7-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="52dc7-201">b.</span><span class="sxs-lookup"><span data-stu-id="52dc7-201">b.</span></span> <span data-ttu-id="52dc7-202">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="52dc7-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="52dc7-203">c.</span><span class="sxs-lookup"><span data-stu-id="52dc7-203">c.</span></span> <span data-ttu-id="52dc7-204">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="52dc7-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="52dc7-205">d.</span><span class="sxs-lookup"><span data-stu-id="52dc7-205">d.</span></span> <span data-ttu-id="52dc7-206">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="52dc7-206">Click **Create**.</span></span>
 
### <a name="creating-a-promapp-test-user"></a><span data-ttu-id="52dc7-207">Vytvoření zkušebního uživatele Promapp</span><span class="sxs-lookup"><span data-stu-id="52dc7-207">Creating a Promapp test user</span></span>

<span data-ttu-id="52dc7-208">Promapp aplikace podporuje pouze za běhu zřizování.</span><span class="sxs-lookup"><span data-stu-id="52dc7-208">The Promapp application supports Just-in-Time provisioning.</span></span> <span data-ttu-id="52dc7-209">To znamená, uživatelský účet se automaticky vytvoří v případě potřeby při pokusu o přístup k aplikaci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="52dc7-209">This means, a user account is automatically created if necessary during an attempt to access the application using the Access Panel.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="52dc7-210">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="52dc7-210">Assigning the Azure AD test user</span></span>

<span data-ttu-id="52dc7-211">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Promapp.</span><span class="sxs-lookup"><span data-stu-id="52dc7-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Promapp.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="52dc7-213">**Pokud chcete přiřadit Britta Simon Promapp, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="52dc7-213">**To assign Britta Simon to Promapp, perform the following steps:**</span></span>

1. <span data-ttu-id="52dc7-214">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="52dc7-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="52dc7-216">V seznamu aplikací vyberte **Promapp**.</span><span class="sxs-lookup"><span data-stu-id="52dc7-216">In the applications list, select **Promapp**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_app.png) 

3. <span data-ttu-id="52dc7-218">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="52dc7-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="52dc7-220">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="52dc7-220">Click **Add** button.</span></span> <span data-ttu-id="52dc7-221">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="52dc7-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="52dc7-223">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="52dc7-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="52dc7-224">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="52dc7-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="52dc7-225">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="52dc7-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="52dc7-226">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="52dc7-226">Testing single sign-on</span></span>

<span data-ttu-id="52dc7-227">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="52dc7-227">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="52dc7-228">Když kliknete na dlaždici Promapp na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Promapp.</span><span class="sxs-lookup"><span data-stu-id="52dc7-228">When you click the Promapp tile in the Access Panel, you should get automatically signed-on to your Promapp application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="52dc7-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="52dc7-229">Additional resources</span></span>

* [<span data-ttu-id="52dc7-230">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="52dc7-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="52dc7-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="52dc7-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_04.png
[12]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_05.png
[13]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_06.png
[14]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_07.png

[100]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_203.png

