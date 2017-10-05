---
title: "Kurz: Azure Active Directory integrace s Office virtuální 8 x 8 | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a 8 x 8 virtuální Office."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: d8dcf0171b93fec15347e810a1b525bd815dbf04
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a><span data-ttu-id="251b0-103">Kurz: Azure Active Directory integrace s Office virtuální 8 x 8</span><span class="sxs-lookup"><span data-stu-id="251b0-103">Tutorial: Azure Active Directory integration with 8x8 Virtual Office</span></span>

<span data-ttu-id="251b0-104">V tomto kurzu zjistěte, jak integrovat 8 x 8 virtuální Office s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="251b0-104">In this tutorial, you learn how to integrate 8x8 Virtual Office with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="251b0-105">Integrace 8 x 8 virtuální Office s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="251b0-105">Integrating 8x8 Virtual Office with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="251b0-106">Můžete řídit ve službě Azure AD, kdo má přístup k Office virtuální 8 x 8</span><span class="sxs-lookup"><span data-stu-id="251b0-106">You can control in Azure AD who has access to 8x8 Virtual Office</span></span>
- <span data-ttu-id="251b0-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Office virtuální 8 x 8 (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="251b0-107">You can enable your users to automatically get signed-on to 8x8 Virtual Office (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="251b0-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="251b0-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="251b0-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="251b0-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="251b0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="251b0-110">Prerequisites</span></span>

<span data-ttu-id="251b0-111">Konfigurace integrace Azure AD s 8 x 8 virtuální Office, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="251b0-111">To configure Azure AD integration with 8x8 Virtual Office, you need the following items:</span></span>

- <span data-ttu-id="251b0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="251b0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="251b0-113">8 x 8 virtuální Office jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="251b0-113">A 8x8 Virtual Office single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="251b0-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="251b0-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="251b0-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="251b0-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="251b0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="251b0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="251b0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="251b0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="251b0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="251b0-118">Scenario description</span></span>
<span data-ttu-id="251b0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="251b0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="251b0-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="251b0-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="251b0-121">Přidání Office virtuální 8 x 8 z Galerie</span><span class="sxs-lookup"><span data-stu-id="251b0-121">Adding 8x8 Virtual Office from the gallery</span></span>
2. <span data-ttu-id="251b0-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="251b0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-8x8-virtual-office-from-the-gallery"></a><span data-ttu-id="251b0-123">Přidání Office virtuální 8 x 8 z Galerie</span><span class="sxs-lookup"><span data-stu-id="251b0-123">Adding 8x8 Virtual Office from the gallery</span></span>
<span data-ttu-id="251b0-124">Při konfiguraci integrace Office virtuální 8 x 8 do služby Azure AD, potřebujete přidat 8 x 8 virtuální Office z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="251b0-124">To configure the integration of 8x8 Virtual Office into Azure AD, you need to add 8x8 Virtual Office from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="251b0-125">**Pokud chcete přidat virtuální Office 8 x 8 z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="251b0-125">**To add 8x8 Virtual Office from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="251b0-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="251b0-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="251b0-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="251b0-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="251b0-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="251b0-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="251b0-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="251b0-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="251b0-133">Do vyhledávacího pole zadejte **8 x 8 virtuální Office**.</span><span class="sxs-lookup"><span data-stu-id="251b0-133">In the search box, type **8x8 Virtual Office**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_search.png)

5. <span data-ttu-id="251b0-135">Na panelu výsledků vyberte **8 x 8 virtuální Office**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="251b0-135">In the results panel, select **8x8 Virtual Office**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="251b0-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="251b0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="251b0-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s 8 x 8, které virtuální Office podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="251b0-138">In this section, you configure and test Azure AD single sign-on with 8x8 Virtual Office based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="251b0-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, jaké příslušného uživatele v 8 x 8 virtuální Office je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="251b0-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 8x8 Virtual Office is to a user in Azure AD.</span></span> <span data-ttu-id="251b0-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské 8 x 8 virtuální Office musí navázat.</span><span class="sxs-lookup"><span data-stu-id="251b0-140">In other words, a link relationship between an Azure AD user and the related user in 8x8 Virtual Office needs to be established.</span></span>

<span data-ttu-id="251b0-141">V systému Office virtuální 8 x 8 přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="251b0-141">In 8x8 Virtual Office, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="251b0-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s 8 x 8 virtuální Office, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="251b0-142">To configure and test Azure AD single sign-on with 8x8 Virtual Office, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="251b0-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="251b0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="251b0-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="251b0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="251b0-145">**[Vytvoření zkušebního uživatele virtuální Office 8 x 8](#creating-a-8x8-virtual-office-test-user)**  – Pokud chcete mít protějšek Britta Simon v Office virtuální 8 x 8, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="251b0-145">**[Creating a 8x8 Virtual Office test user](#creating-a-8x8-virtual-office-test-user)** - to have a counterpart of Britta Simon in 8x8 Virtual Office that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="251b0-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="251b0-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="251b0-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="251b0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="251b0-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="251b0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="251b0-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Office virtuální 8 x 8.</span><span class="sxs-lookup"><span data-stu-id="251b0-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 8x8 Virtual Office application.</span></span>

<span data-ttu-id="251b0-150">**Ke konfiguraci Azure AD jednotné přihlašování s 8 x 8 virtuální Office, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="251b0-150">**To configure Azure AD single sign-on with 8x8 Virtual Office, perform the following steps:**</span></span>

1. <span data-ttu-id="251b0-151">Na portálu Azure na **8 x 8 virtuální Office** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="251b0-151">In the Azure portal, on the **8x8 Virtual Office** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="251b0-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="251b0-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_samlbase.png)

3. <span data-ttu-id="251b0-155">Na **8 x 8 virtuální Office domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="251b0-155">On the **8x8 Virtual Office Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    <span data-ttu-id="251b0-157">a.</span><span class="sxs-lookup"><span data-stu-id="251b0-157">a.</span></span> <span data-ttu-id="251b0-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="251b0-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>

    <span data-ttu-id="251b0-159">| `https://sso.8x8.com/<companyname>` |
    | `https://www.8x8.com/<companyname>` |
    | `https://sso.8x8pilot.com/<companyname>` |</span><span class="sxs-lookup"><span data-stu-id="251b0-159">| `https://sso.8x8.com/<companyname>` |
 | `https://www.8x8.com/<companyname>` |
 | `https://sso.8x8pilot.com/<companyname>` |</span></span>

    <span data-ttu-id="251b0-160">b.</span><span class="sxs-lookup"><span data-stu-id="251b0-160">b.</span></span> <span data-ttu-id="251b0-161">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="251b0-161">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>

    <span data-ttu-id="251b0-162">| `https://<subdomain>.8x8.com/saml2` |
    | `https://<subdomain>.8x8pilot.com/saml2`|</span><span class="sxs-lookup"><span data-stu-id="251b0-162">| `https://<subdomain>.8x8.com/saml2` |
 | `https://<subdomain>.8x8pilot.com/saml2`|</span></span>

    > [!NOTE] 
    > <span data-ttu-id="251b0-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="251b0-163">These values are not real.</span></span> <span data-ttu-id="251b0-164">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="251b0-164">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="251b0-165">Obraťte se na [tým podpory virtuální Office 8 x 8](https://www.8x8.com/about-us/contact-us) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="251b0-165">Contact [8x8 Virtual Office support team](https://www.8x8.com/about-us/contact-us) to get these values.</span></span>
 


4. <span data-ttu-id="251b0-166">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Raw)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="251b0-166">On the **SAML Signing Certificate** section, click **Certificate (Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

5. <span data-ttu-id="251b0-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="251b0-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="251b0-170">Na **8 x 8 virtuální Office konfigurace** klikněte na tlačítko **virtuální Office konfigurovat 8 x 8** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="251b0-170">On the **8x8 Virtual Office Configuration** section, click **Configure 8x8 Virtual Office** to open **Configure sign-on** window.</span></span> <span data-ttu-id="251b0-171">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="251b0-171">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_configure.png) 

7. <span data-ttu-id="251b0-173">Přihlášení ke klientovi 8 x 8 virtuální Office jako správce.</span><span class="sxs-lookup"><span data-stu-id="251b0-173">Sign-on to your 8x8 Virtual Office tenant as an administrator.</span></span>

8. <span data-ttu-id="251b0-174">Vyberte **virtuální Office účet Mgr** na panelu aplikace.</span><span class="sxs-lookup"><span data-stu-id="251b0-174">Select **Virtual Office Account Mgr** on Application Panel.</span></span>
   
    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. <span data-ttu-id="251b0-176">Vyberte **obchodní** účet, který chcete spravovat a klikněte na tlačítko **přihlásit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="251b0-176">Select **Business** account to manage and click **Sign In** button.</span></span>
   
    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. <span data-ttu-id="251b0-178">Klikněte na tlačítko **účty** karta v seznamu v nabídce.</span><span class="sxs-lookup"><span data-stu-id="251b0-178">Click **Accounts** tab in the menu list.</span></span>
   
    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. <span data-ttu-id="251b0-180">Klikněte na tlačítko **jednotné přihlašování** v seznamu účtů.</span><span class="sxs-lookup"><span data-stu-id="251b0-180">Click **Single Sign On** in the list of Accounts.</span></span>
   
    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. <span data-ttu-id="251b0-182">Vyberte **jednotné přihlašování** pod metodu ověřování a klikněte na tlačítko **SAML**.</span><span class="sxs-lookup"><span data-stu-id="251b0-182">Select **Single Sign On** under Authentication method and click **SAML**.</span></span>
    
    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. <span data-ttu-id="251b0-184">Kopírování **jednotné přihlašování SAML URL**, **jednotné Sing se adresa URL služby** a **URL vystavitele** z Azure AD **přihlašovací adresa URL**, **Odhlásit se adresa URL** a **URL vystavitele** v 8 x 8 virtuální Office.</span><span class="sxs-lookup"><span data-stu-id="251b0-184">Copy **SAML SSO URL**, **Single Sing Out Service URL** and **Issuer URL** from Azure AD to **Sign In URL**, **Sign Out URL** and **Issuer URL** in 8x8 Virtual Office.</span></span> 
    
    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)
    
14. <span data-ttu-id="251b0-186">Klikněte na tlačítko **prohlížeče** tlačítko odeslat certifikát, který jste si stáhli z Azure AD a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="251b0-186">Click **Browser** button to upload the certificate which you downloaded from Azure AD, and click the **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="251b0-187">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="251b0-187">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="251b0-188">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="251b0-188">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="251b0-189">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="251b0-189">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="251b0-190">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="251b0-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="251b0-191">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="251b0-191">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="251b0-193">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="251b0-193">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="251b0-194">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="251b0-194">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="251b0-196">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="251b0-196">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="251b0-198">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="251b0-198">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="251b0-200">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="251b0-200">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="251b0-202">a.</span><span class="sxs-lookup"><span data-stu-id="251b0-202">a.</span></span> <span data-ttu-id="251b0-203">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="251b0-203">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="251b0-204">b.</span><span class="sxs-lookup"><span data-stu-id="251b0-204">b.</span></span> <span data-ttu-id="251b0-205">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="251b0-205">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="251b0-206">c.</span><span class="sxs-lookup"><span data-stu-id="251b0-206">c.</span></span> <span data-ttu-id="251b0-207">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="251b0-207">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="251b0-208">d.</span><span class="sxs-lookup"><span data-stu-id="251b0-208">d.</span></span> <span data-ttu-id="251b0-209">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="251b0-209">Click **Create**.</span></span>
 
### <a name="creating-a-8x8-virtual-office-test-user"></a><span data-ttu-id="251b0-210">Vytvoření zkušebního uživatele virtuální Office 8 x 8</span><span class="sxs-lookup"><span data-stu-id="251b0-210">Creating a 8x8 Virtual Office test user</span></span>

<span data-ttu-id="251b0-211">Cílem této části je vytvoření uživatele volal Britta Simon v Office virtuální 8 x 8.</span><span class="sxs-lookup"><span data-stu-id="251b0-211">The objective of this section is to create a user called Britta Simon in 8x8 Virtual Office.</span></span> <span data-ttu-id="251b0-212">8 x 8 virtuální Office podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="251b0-212">8x8 Virtual Office supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="251b0-213">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="251b0-213">There is no action item for you in this section.</span></span> <span data-ttu-id="251b0-214">Nový uživatel se vytvoří během pokusu o přístup k Office virtuální 8 x 8, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="251b0-214">A new user is created during an attempt to access 8x8 Virtual Office if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="251b0-215">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat [tým podpory virtuální Office 8 x 8](https://www.8x8.com/about-us/contact-us).</span><span class="sxs-lookup"><span data-stu-id="251b0-215">If you need to create a user manually, you need to contact the [8x8 Virtual Office support team](https://www.8x8.com/about-us/contact-us).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="251b0-216">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="251b0-216">Assigning the Azure AD test user</span></span>

<span data-ttu-id="251b0-217">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu 8 x 8 virtuální Office.</span><span class="sxs-lookup"><span data-stu-id="251b0-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 8x8 Virtual Office.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="251b0-219">**Pokud chcete přiřadit Britta Simon 8 x 8 virtuální Office, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="251b0-219">**To assign Britta Simon to 8x8 Virtual Office, perform the following steps:**</span></span>

1. <span data-ttu-id="251b0-220">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="251b0-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="251b0-222">V seznamu aplikací vyberte **8 x 8 virtuální Office**.</span><span class="sxs-lookup"><span data-stu-id="251b0-222">In the applications list, select **8x8 Virtual Office**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. <span data-ttu-id="251b0-224">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="251b0-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="251b0-226">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="251b0-226">Click **Add** button.</span></span> <span data-ttu-id="251b0-227">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="251b0-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="251b0-229">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="251b0-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="251b0-230">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="251b0-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="251b0-231">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="251b0-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="251b0-232">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="251b0-232">Testing single sign-on</span></span>

<span data-ttu-id="251b0-233">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="251b0-233">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="251b0-234">Když kliknete na dlaždici 8 x 8 virtuální Office na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci virtuální Office 8 x 8.</span><span class="sxs-lookup"><span data-stu-id="251b0-234">When you click the 8x8 Virtual Office tile in the Access Panel, you should get automatically signed-on to your 8x8 Virtual Office application.</span></span>
<span data-ttu-id="251b0-235">Další informace o na přístupovém panelu najdete v tématu [Úvod do přístupového panelu](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="251b0-235">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="251b0-236">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="251b0-236">Additional resources</span></span>

* [<span data-ttu-id="251b0-237">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="251b0-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="251b0-238">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="251b0-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png

