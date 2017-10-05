---
title: 'Kurz: Azure Active Directory integrace s Ariba | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Ariba."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 45a8364c-55d1-4dc7-b079-9eb2a701842d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: 214367847055ba38ee03a28d0afdcc58f68333cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ariba"></a><span data-ttu-id="de81d-103">Kurz: Azure Active Directory integrace s Ariba</span><span class="sxs-lookup"><span data-stu-id="de81d-103">Tutorial: Azure Active Directory integration with Ariba</span></span>

<span data-ttu-id="de81d-104">V tomto kurzu zjistěte, jak integrovat Ariba s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="de81d-104">In this tutorial, you learn how to integrate Ariba with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="de81d-105">Integrace Ariba s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="de81d-105">Integrating Ariba with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="de81d-106">Můžete řídit ve službě Azure AD, který má přístup k Ariba</span><span class="sxs-lookup"><span data-stu-id="de81d-106">You can control in Azure AD who has access to Ariba</span></span>
- <span data-ttu-id="de81d-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Ariba (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="de81d-107">You can enable your users to automatically get signed-on to Ariba (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="de81d-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="de81d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="de81d-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="de81d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de81d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="de81d-110">Prerequisites</span></span>

<span data-ttu-id="de81d-111">Konfigurace integrace Azure AD s Ariba, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="de81d-111">To configure Azure AD integration with Ariba, you need the following items:</span></span>

- <span data-ttu-id="de81d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="de81d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="de81d-113">Ariba jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="de81d-113">An Ariba single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="de81d-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="de81d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="de81d-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="de81d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="de81d-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="de81d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="de81d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="de81d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="de81d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="de81d-118">Scenario description</span></span>
<span data-ttu-id="de81d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="de81d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="de81d-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="de81d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="de81d-121">Přidání Ariba z Galerie</span><span class="sxs-lookup"><span data-stu-id="de81d-121">Adding Ariba from the gallery</span></span>
2. <span data-ttu-id="de81d-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="de81d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ariba-from-the-gallery"></a><span data-ttu-id="de81d-123">Přidání Ariba z Galerie</span><span class="sxs-lookup"><span data-stu-id="de81d-123">Adding Ariba from the gallery</span></span>
<span data-ttu-id="de81d-124">Při konfiguraci integrace Ariba do služby Azure AD potřebujete přidat Ariba z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="de81d-124">To configure the integration of Ariba into Azure AD, you need to add Ariba from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="de81d-125">**Pokud chcete přidat Ariba z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="de81d-125">**To add Ariba from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="de81d-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="de81d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="de81d-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="de81d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="de81d-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="de81d-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="de81d-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="de81d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="de81d-133">Do vyhledávacího pole zadejte **Ariba**.</span><span class="sxs-lookup"><span data-stu-id="de81d-133">In the search box, type **Ariba**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_search.png)

5. <span data-ttu-id="de81d-135">Na panelu výsledků vyberte **Ariba**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="de81d-135">In the results panel, select **Ariba**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="de81d-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="de81d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="de81d-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Ariba podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="de81d-138">In this section, you configure and test Azure AD single sign-on with Ariba based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="de81d-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Ariba je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="de81d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Ariba is to a user in Azure AD.</span></span> <span data-ttu-id="de81d-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Ariba musí navázat.</span><span class="sxs-lookup"><span data-stu-id="de81d-140">In other words, a link relationship between an Azure AD user and the related user in Ariba needs to be established.</span></span>

<span data-ttu-id="de81d-141">V Ariba, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="de81d-141">In Ariba, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="de81d-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Ariba, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="de81d-142">To configure and test Azure AD single sign-on with Ariba, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="de81d-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="de81d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="de81d-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="de81d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="de81d-145">**[Vytváření testovacího uživatele Ariba](#creating-an-ariba-test-user)**  – Pokud chcete mít protějšek Britta Simon v Ariba propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="de81d-145">**[Creating an Ariba test user](#creating-an-ariba-test-user)** - to have a counterpart of Britta Simon in Ariba that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="de81d-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="de81d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="de81d-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="de81d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="de81d-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="de81d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="de81d-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Ariba.</span><span class="sxs-lookup"><span data-stu-id="de81d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Ariba application.</span></span>

<span data-ttu-id="de81d-150">**Ke konfiguraci Azure AD jednotné přihlašování s Ariba, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="de81d-150">**To configure Azure AD single sign-on with Ariba, perform the following steps:**</span></span>

1. <span data-ttu-id="de81d-151">Na portálu Azure na **Ariba** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="de81d-151">In the Azure portal, on the **Ariba** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="de81d-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="de81d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_samlbase.png)

3. <span data-ttu-id="de81d-155">Na **Ariba domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="de81d-155">On the **Ariba Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_url.png)

    <span data-ttu-id="de81d-157">a.</span><span class="sxs-lookup"><span data-stu-id="de81d-157">a.</span></span> <span data-ttu-id="de81d-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce: `https://<subdomain>.sourcing.ariba.com` nebo`https://<subdomain>.supplier.ariba.com`</span><span class="sxs-lookup"><span data-stu-id="de81d-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.sourcing.ariba.com` or `https://<subdomain>.supplier.ariba.com`</span></span>

    <span data-ttu-id="de81d-159">b.</span><span class="sxs-lookup"><span data-stu-id="de81d-159">b.</span></span> <span data-ttu-id="de81d-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`http://<subdomain>.procurement-2.ariba.com`</span><span class="sxs-lookup"><span data-stu-id="de81d-160">In the **Identifier** textbox, type a URL using the following pattern: `http://<subdomain>.procurement-2.ariba.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="de81d-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="de81d-161">These values are not real.</span></span> <span data-ttu-id="de81d-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="de81d-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="de81d-163">Zde, doporučujeme vám použít jedinečnou hodnotu řetězce v identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="de81d-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="de81d-164">Obraťte se na tým podpory Ariba klienta v **1-866-218-2155** k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="de81d-164">Contact Ariba Client support team at **1-866-218-2155** to get these values.</span></span> 
 

4. <span data-ttu-id="de81d-165">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="de81d-165">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate  file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_certificate.png) 

5. <span data-ttu-id="de81d-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="de81d-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ariba-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="de81d-169">Jednotné přihlašování, které jsou nakonfigurované pro vaše aplikace získáte volání na tým podpory Ariba **1-866-218-2155** a budete vám další pomůže o tom, jak poskytněte stažené **certifikátu (Base64)** souboru.</span><span class="sxs-lookup"><span data-stu-id="de81d-169">To get SSO configured for your application, call Ariba support team on **1-866-218-2155** and they'll assist you further on how to provide them the downloaded **Certificate (Base64)** file.</span></span>  
 
> [!TIP]
> <span data-ttu-id="de81d-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="de81d-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="de81d-171">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="de81d-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="de81d-172">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="de81d-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="de81d-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="de81d-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="de81d-174">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="de81d-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="de81d-176">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="de81d-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="de81d-177">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="de81d-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ariba-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="de81d-179">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="de81d-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ariba-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="de81d-181">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="de81d-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ariba-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="de81d-183">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="de81d-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ariba-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="de81d-185">a.</span><span class="sxs-lookup"><span data-stu-id="de81d-185">a.</span></span> <span data-ttu-id="de81d-186">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="de81d-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="de81d-187">b.</span><span class="sxs-lookup"><span data-stu-id="de81d-187">b.</span></span> <span data-ttu-id="de81d-188">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="de81d-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="de81d-189">c.</span><span class="sxs-lookup"><span data-stu-id="de81d-189">c.</span></span> <span data-ttu-id="de81d-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="de81d-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="de81d-191">d.</span><span class="sxs-lookup"><span data-stu-id="de81d-191">d.</span></span> <span data-ttu-id="de81d-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="de81d-192">Click **Create**.</span></span>
 
### <a name="creating-an-ariba-test-user"></a><span data-ttu-id="de81d-193">Vytvoření Ariba testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="de81d-193">Creating an Ariba test user</span></span>

<span data-ttu-id="de81d-194">Cílem této části je vytvoření uživatele v Ariba nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="de81d-194">The objective of this section is to create a user called Britta Simon in Ariba.</span></span> <span data-ttu-id="de81d-195">Práce s Ariba tým podpory v **1-866-218-2155** přidejte uživatele v systému Ariba.</span><span class="sxs-lookup"><span data-stu-id="de81d-195">Work with Ariba support team at **1-866-218-2155** to add the users in the Ariba system.</span></span> 

 >[!NOTE]
 ><span data-ttu-id="de81d-196">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat tým podpory Ariba v **1-866-218-2155**.</span><span class="sxs-lookup"><span data-stu-id="de81d-196">If you need to create a user manually, you need to contact the Ariba support team at **1-866-218-2155**.</span></span>
 >  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="de81d-197">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="de81d-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="de81d-198">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Ariba.</span><span class="sxs-lookup"><span data-stu-id="de81d-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Ariba.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="de81d-200">**Pokud chcete přiřadit Britta Simon Ariba, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="de81d-200">**To assign Britta Simon to Ariba, perform the following steps:**</span></span>

1. <span data-ttu-id="de81d-201">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="de81d-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="de81d-203">V seznamu aplikací vyberte **Ariba**.</span><span class="sxs-lookup"><span data-stu-id="de81d-203">In the applications list, select **Ariba**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_app.png) 

3. <span data-ttu-id="de81d-205">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="de81d-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="de81d-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="de81d-207">Click **Add** button.</span></span> <span data-ttu-id="de81d-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="de81d-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="de81d-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="de81d-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="de81d-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="de81d-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="de81d-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="de81d-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="de81d-213">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="de81d-213">Testing single sign-on</span></span>
<span data-ttu-id="de81d-214">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="de81d-214">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="de81d-215">Když kliknete na dlaždici Ariba na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Ariba.</span><span class="sxs-lookup"><span data-stu-id="de81d-215">When you click the Ariba tile in the Access Panel, you should get automatically signed-on to your Ariba application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="de81d-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="de81d-216">Additional resources</span></span>

* [<span data-ttu-id="de81d-217">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="de81d-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="de81d-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="de81d-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_203.png

