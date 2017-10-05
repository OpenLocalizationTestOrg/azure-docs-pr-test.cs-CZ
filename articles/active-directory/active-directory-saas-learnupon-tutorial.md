---
title: 'Kurz: Azure Active Directory integrace s LearnUpon | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a LearnUpon."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: b6ac8acc244e9029be01ede5e0865c280171217d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a><span data-ttu-id="9ae42-103">Kurz: Azure Active Directory integrace s LearnUpon</span><span class="sxs-lookup"><span data-stu-id="9ae42-103">Tutorial: Azure Active Directory integration with LearnUpon</span></span>

<span data-ttu-id="9ae42-104">V tomto kurzu zjistěte, jak integrovat LearnUpon s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9ae42-104">In this tutorial, you learn how to integrate LearnUpon with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9ae42-105">Integrace LearnUpon s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9ae42-105">Integrating LearnUpon with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9ae42-106">Můžete řídit ve službě Azure AD, který má přístup k LearnUpon</span><span class="sxs-lookup"><span data-stu-id="9ae42-106">You can control in Azure AD who has access to LearnUpon</span></span>
- <span data-ttu-id="9ae42-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k LearnUpon (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9ae42-107">You can enable your users to automatically get signed-on to LearnUpon (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9ae42-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9ae42-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9ae42-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9ae42-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ae42-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9ae42-110">Prerequisites</span></span>

<span data-ttu-id="9ae42-111">Konfigurace integrace Azure AD s LearnUpon, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="9ae42-111">To configure Azure AD integration with LearnUpon, you need the following items:</span></span>

- <span data-ttu-id="9ae42-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9ae42-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9ae42-113">LearnUpon jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9ae42-113">A LearnUpon single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9ae42-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9ae42-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9ae42-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9ae42-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9ae42-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9ae42-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9ae42-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9ae42-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9ae42-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9ae42-118">Scenario description</span></span>
<span data-ttu-id="9ae42-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9ae42-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9ae42-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9ae42-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9ae42-121">Přidání LearnUpon z Galerie</span><span class="sxs-lookup"><span data-stu-id="9ae42-121">Adding LearnUpon from the gallery</span></span>
2. <span data-ttu-id="9ae42-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ae42-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learnupon-from-the-gallery"></a><span data-ttu-id="9ae42-123">Přidání LearnUpon z Galerie</span><span class="sxs-lookup"><span data-stu-id="9ae42-123">Adding LearnUpon from the gallery</span></span>
<span data-ttu-id="9ae42-124">Při konfiguraci integrace LearnUpon do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS LearnUpon z galerie.</span><span class="sxs-lookup"><span data-stu-id="9ae42-124">To configure the integration of LearnUpon into Azure AD, you need to add LearnUpon from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9ae42-125">**Pokud chcete přidat LearnUpon z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9ae42-125">**To add LearnUpon from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9ae42-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9ae42-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9ae42-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9ae42-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9ae42-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ae42-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9ae42-133">Do vyhledávacího pole zadejte **LearnUpon**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-133">In the search box, type **LearnUpon**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_search.png)

5. <span data-ttu-id="9ae42-135">Na panelu výsledků vyberte **LearnUpon**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9ae42-135">In the results panel, select **LearnUpon**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9ae42-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ae42-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9ae42-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s LearnUpon podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="9ae42-138">In this section, you configure and test Azure AD single sign-on with LearnUpon based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9ae42-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v LearnUpon je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9ae42-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LearnUpon is to a user in Azure AD.</span></span> <span data-ttu-id="9ae42-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v LearnUpon musí navázat.</span><span class="sxs-lookup"><span data-stu-id="9ae42-140">In other words, a link relationship between an Azure AD user and the related user in LearnUpon needs to be established.</span></span>

<span data-ttu-id="9ae42-141">V LearnUpon, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="9ae42-141">In LearnUpon, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9ae42-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s LearnUpon, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="9ae42-142">To configure and test Azure AD single sign-on with LearnUpon, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9ae42-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="9ae42-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9ae42-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9ae42-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9ae42-145">**[Vytvoření zkušebního uživatele LearnUpon](#creating-a-learnupon-test-user)**  – Pokud chcete mít protějšek Britta Simon v LearnUpon propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ae42-145">**[Creating a LearnUpon test user](#creating-a-learnupon-test-user)** - to have a counterpart of Britta Simon in LearnUpon that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9ae42-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9ae42-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9ae42-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9ae42-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9ae42-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ae42-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9ae42-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci LearnUpon.</span><span class="sxs-lookup"><span data-stu-id="9ae42-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LearnUpon application.</span></span>

<span data-ttu-id="9ae42-150">**Ke konfiguraci Azure AD jednotné přihlašování s LearnUpon, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9ae42-150">**To configure Azure AD single sign-on with LearnUpon, perform the following steps:**</span></span>

1. <span data-ttu-id="9ae42-151">Na portálu Azure na **LearnUpon** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-151">In the Azure portal, on the **LearnUpon** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9ae42-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9ae42-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_samlbase.png)

3. <span data-ttu-id="9ae42-155">Na **LearnUpon domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9ae42-155">On the **LearnUpon Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_url.png)

    <span data-ttu-id="9ae42-157">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.learnupon.com/saml/consumer`</span><span class="sxs-lookup"><span data-stu-id="9ae42-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.learnupon.com/saml/consumer`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9ae42-158">Upozorňujeme, že se nejedná skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9ae42-158">Please note that this is not the real value.</span></span> <span data-ttu-id="9ae42-159">budete muset aktualizovat tuto hodnotu s skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9ae42-159">you have to update this value with the actual Reply URL.</span></span> <span data-ttu-id="9ae42-160">Chcete-li získat tuto hodnotu, obraťte se na [tým podpory LearnUpon](https://www.learnupon.com/features/support/).</span><span class="sxs-lookup"><span data-stu-id="9ae42-160">To get this value Contact [LearnUpon support team](https://www.learnupon.com/features/support/).</span></span>



4. <span data-ttu-id="9ae42-161">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Raw)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="9ae42-161">On the **SAML Signing Certificate** section, click **Certificate (Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_certificate.png) 

5. <span data-ttu-id="9ae42-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ae42-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9ae42-165">Na **LearnUpon konfigurace** klikněte na tlačítko **konfigurace LearnUpon** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9ae42-165">On the **LearnUpon Configuration** section, click **Configure LearnUpon** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9ae42-166">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="9ae42-166">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_configure.png) 

7. <span data-ttu-id="9ae42-168">Otevřete jiné instance prohlížeče a přihlášení do LearnUpon s účtem správce.</span><span class="sxs-lookup"><span data-stu-id="9ae42-168">Open another browser instance and login into LearnUpon with an administrator account.</span></span> 

8. <span data-ttu-id="9ae42-169">Klikněte **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="9ae42-169">Click the **settings** tab.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png)

9. <span data-ttu-id="9ae42-171">Klikněte na tlačítko **jednotné přihlašování – SAML**a potom klikněte na **obecné nastavení** ke konfiguraci nastavení SAML.</span><span class="sxs-lookup"><span data-stu-id="9ae42-171">Click **Single Sign On - SAML**, and then click **General Settings** to configure SAML settings.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 

10. <span data-ttu-id="9ae42-173">V **obecné nastavení** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9ae42-173">In the **General Settings** section, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png)  
  
    <span data-ttu-id="9ae42-175">a.</span><span class="sxs-lookup"><span data-stu-id="9ae42-175">a.</span></span> <span data-ttu-id="9ae42-176">Vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-176">Select **Enabled**.</span></span>

    <span data-ttu-id="9ae42-177">b.</span><span class="sxs-lookup"><span data-stu-id="9ae42-177">b.</span></span> <span data-ttu-id="9ae42-178">Vyberte **verze** jako **2.0**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-178">Select **Version** as **2.0**.</span></span>

    <span data-ttu-id="9ae42-179">c.</span><span class="sxs-lookup"><span data-stu-id="9ae42-179">c.</span></span> <span data-ttu-id="9ae42-180">Vyberte **přeskočit podmínky** jako **ne**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-180">Select **Skip conditions** as **No**.</span></span>

    <span data-ttu-id="9ae42-181">d.</span><span class="sxs-lookup"><span data-stu-id="9ae42-181">d.</span></span> <span data-ttu-id="9ae42-182">V **vystavení tokenu SAML název parametru** textovému poli, typ výše uvedený název parametru požadavek post na adresu URL SAML příjemce, který obsahuje kontrolního výrazu SAML ověřit a ověřit – například **SAMLResponse** .</span><span class="sxs-lookup"><span data-stu-id="9ae42-182">In the **SAML Token Post param name** textbox, type the name of request post parameter to the SAML consumer URL indicated above that contains the SAML Assertion to be verified and authenticated - for example **SAMLResponse**.</span></span>

    <span data-ttu-id="9ae42-183">e.</span><span class="sxs-lookup"><span data-stu-id="9ae42-183">e.</span></span> <span data-ttu-id="9ae42-184">V **formát názvu identifikátor** textovému poli, zadejte hodnotu, která určuje, kam ve vaší kontrolního výrazu SAML identifikátor uživatele (e-mailovou adresu) nachází – například **urn: oasis: názvy: tc: SAML:1.1:nameid-formát: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-184">In the **Name Identifier Format** textbox, type the value that indicates where in your SAML Assertion the users identifier (Email address) resides - for example **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>
  
    <span data-ttu-id="9ae42-185">f.</span><span class="sxs-lookup"><span data-stu-id="9ae42-185">f.</span></span> <span data-ttu-id="9ae42-186">V **identifikovat umístění poskytovatele** textovému poli, zadejte hodnotu, která určuje, kdy uživatelé odesílají na Pokud kliknutí na ikonu vašeho nahrané z obrazovky přihlášení k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9ae42-186">In the **Identify Provider Location** textbox, type the value that indicates where the users are sent to if they click on your uploaded icon from your Azure portal login screen.</span></span>
  
    <span data-ttu-id="9ae42-187">g.</span><span class="sxs-lookup"><span data-stu-id="9ae42-187">g.</span></span> <span data-ttu-id="9ae42-188">V **Odhlásit se adresa URL** textovému poli, Vložit **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9ae42-188">In the **Sign out URL** textbox, paste the **Sign-Out URL** which you have copied from the Azure portal.</span></span>
    
    <span data-ttu-id="9ae42-189">h.</span><span class="sxs-lookup"><span data-stu-id="9ae42-189">h.</span></span> <span data-ttu-id="9ae42-190">Klikněte na tlačítko **spravovat prstem výtisků**a pak nahrajte prstu stažené certifikátu.</span><span class="sxs-lookup"><span data-stu-id="9ae42-190">Click **Manage finger prints**, and then upload the finger print of your downloaded certificate.</span></span>

11. <span data-ttu-id="9ae42-191">Klikněte na tlačítko **uživatelská nastavení**a poté proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9ae42-191">Click **User Settings**, and then perform the following steps:</span></span>
   
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png)  
 
    <span data-ttu-id="9ae42-193">a.</span><span class="sxs-lookup"><span data-stu-id="9ae42-193">a.</span></span> <span data-ttu-id="9ae42-194">V **křestní jméno identifikátor formátu** textovému poli, zadejte hodnotu, která sděluje nám v vaší kontrolního výrazu SAML firstname uživatelé nachází – například: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-194">In the **First Name Identifier Format** textbox, type the value that tells us where in your SAML Assertion the users firstname resides - for example: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>
  
    <span data-ttu-id="9ae42-195">b.</span><span class="sxs-lookup"><span data-stu-id="9ae42-195">b.</span></span> <span data-ttu-id="9ae42-196">V **poslední formát názvu identifikátor** textovému poli, zadejte hodnotu, která sděluje nám v vaší kontrolního výrazu SAML lastname uživatelé nachází – například: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname** .</span><span class="sxs-lookup"><span data-stu-id="9ae42-196">In the **Last Name Identifier Format** textbox, type the value that tells us where in your SAML Assertion the users lastname resides - for example: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span></span>

> [!TIP]
> <span data-ttu-id="9ae42-197">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="9ae42-197">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9ae42-198">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="9ae42-198">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9ae42-199">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9ae42-199">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9ae42-200">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9ae42-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="9ae42-201">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9ae42-201">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9ae42-203">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9ae42-203">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9ae42-204">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9ae42-204">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9ae42-206">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-206">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9ae42-208">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ae42-208">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9ae42-210">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9ae42-210">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9ae42-212">a.</span><span class="sxs-lookup"><span data-stu-id="9ae42-212">a.</span></span> <span data-ttu-id="9ae42-213">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-213">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9ae42-214">b.</span><span class="sxs-lookup"><span data-stu-id="9ae42-214">b.</span></span> <span data-ttu-id="9ae42-215">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9ae42-215">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9ae42-216">c.</span><span class="sxs-lookup"><span data-stu-id="9ae42-216">c.</span></span> <span data-ttu-id="9ae42-217">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-217">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9ae42-218">d.</span><span class="sxs-lookup"><span data-stu-id="9ae42-218">d.</span></span> <span data-ttu-id="9ae42-219">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-219">Click **Create**.</span></span>
 
### <a name="creating-a-learnupon-test-user"></a><span data-ttu-id="9ae42-220">Vytvoření zkušebního uživatele LearnUpon</span><span class="sxs-lookup"><span data-stu-id="9ae42-220">Creating a LearnUpon test user</span></span>

<span data-ttu-id="9ae42-221">Cílem této části je vytvoření uživatele v LearnUpon nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9ae42-221">The objective of this section is to create a user called Britta Simon in LearnUpon.</span></span> <span data-ttu-id="9ae42-222">LearnUpon podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="9ae42-222">LearnUpon supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="9ae42-223">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="9ae42-223">There is no action item for you in this section.</span></span> <span data-ttu-id="9ae42-224">Vytvoří se nový uživatel během pokusu o přístup k LearnUpon, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="9ae42-224">A new user will be created during an attempt to access LearnUpon if it doesn't exist yet.</span></span> <span data-ttu-id="9ae42-225">[Konfigurace Azure AD jednotné přihlášení](#configuring-azure-ad-single-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="9ae42-225">[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on).</span></span>

>[!NOTE]
><span data-ttu-id="9ae42-226">Pokud potřebujete vytvořit uživatele s ručně, budete muset kontaktovat [tým podpory LearnUpon](https://www.learnupon.com/features/support/).</span><span class="sxs-lookup"><span data-stu-id="9ae42-226">If you need to create an user manually, you need to contact [LearnUpon support team](https://www.learnupon.com/features/support/).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9ae42-227">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9ae42-227">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9ae42-228">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu LearnUpon.</span><span class="sxs-lookup"><span data-stu-id="9ae42-228">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LearnUpon.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9ae42-230">**Pokud chcete přiřadit Britta Simon LearnUpon, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9ae42-230">**To assign Britta Simon to LearnUpon, perform the following steps:**</span></span>

1. <span data-ttu-id="9ae42-231">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-231">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9ae42-233">V seznamu aplikací vyberte **LearnUpon**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-233">In the applications list, select **LearnUpon**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_app.png) 

3. <span data-ttu-id="9ae42-235">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9ae42-235">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9ae42-237">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ae42-237">Click **Add** button.</span></span> <span data-ttu-id="9ae42-238">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ae42-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9ae42-240">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9ae42-240">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9ae42-241">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ae42-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9ae42-242">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ae42-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9ae42-243">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ae42-243">Testing single sign-on</span></span>

<span data-ttu-id="9ae42-244">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9ae42-244">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9ae42-245">Když kliknete na dlaždici LearnUpon na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci LearnUpon.</span><span class="sxs-lookup"><span data-stu-id="9ae42-245">When you click the LearnUpon tile in the Access Panel, you should get automatically signed-on to your LearnUpon application.</span></span>
<span data-ttu-id="9ae42-246">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9ae42-246">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9ae42-247">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9ae42-247">Additional resources</span></span>

* [<span data-ttu-id="9ae42-248">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9ae42-248">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9ae42-249">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9ae42-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png

