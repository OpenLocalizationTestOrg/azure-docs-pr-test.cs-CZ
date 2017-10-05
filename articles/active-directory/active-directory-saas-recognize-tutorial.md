---
title: "Kurz: Azure Active Directory integrace s rozpoznávat | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a rozpoznávat."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 97d85183d0307c41a3b879d440d87a6fb0c53190
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a><span data-ttu-id="ed0b9-103">Kurz: Azure Active Directory integrace s rozpoznávat</span><span class="sxs-lookup"><span data-stu-id="ed0b9-103">Tutorial: Azure Active Directory integration with Recognize</span></span>

<span data-ttu-id="ed0b9-104">V tomto kurzu zjistěte, jak integrovat rozpoznávat s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ed0b9-104">In this tutorial, you learn how to integrate Recognize with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ed0b9-105">Integrace rozpoznávat s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ed0b9-105">Integrating Recognize with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ed0b9-106">Můžete řídit ve službě Azure AD, který má přístup k rozpoznání</span><span class="sxs-lookup"><span data-stu-id="ed0b9-106">You can control in Azure AD who has access to Recognize</span></span>
- <span data-ttu-id="ed0b9-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k rozpoznání (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ed0b9-107">You can enable your users to automatically get signed-on to Recognize (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ed0b9-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ed0b9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ed0b9-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ed0b9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ed0b9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ed0b9-110">Prerequisites</span></span>

<span data-ttu-id="ed0b9-111">Konfigurace integrace Azure AD s rozpoznávat, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="ed0b9-111">To configure Azure AD integration with Recognize, you need the following items:</span></span>

- <span data-ttu-id="ed0b9-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ed0b9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ed0b9-113">Rozpoznávat jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ed0b9-113">A Recognize single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ed0b9-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ed0b9-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ed0b9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ed0b9-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ed0b9-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ed0b9-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ed0b9-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ed0b9-118">Scenario description</span></span>
<span data-ttu-id="ed0b9-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ed0b9-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ed0b9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ed0b9-121">Přidání rozpoznávat z Galerie</span><span class="sxs-lookup"><span data-stu-id="ed0b9-121">Adding Recognize from the gallery</span></span>
2. <span data-ttu-id="ed0b9-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ed0b9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-recognize-from-the-gallery"></a><span data-ttu-id="ed0b9-123">Přidání rozpoznávat z Galerie</span><span class="sxs-lookup"><span data-stu-id="ed0b9-123">Adding Recognize from the gallery</span></span>
<span data-ttu-id="ed0b9-124">Při konfiguraci integrace rozpoznávat do služby Azure AD potřebujete přidat rozpoznávat z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-124">To configure the integration of Recognize into Azure AD, you need to add Recognize from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ed0b9-125">**Pokud chcete přidat rozpoznávat z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ed0b9-125">**To add Recognize from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ed0b9-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ed0b9-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ed0b9-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ed0b9-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ed0b9-133">Do vyhledávacího pole zadejte **rozpoznávat**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-133">In the search box, type **Recognize**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_search.png)

5. <span data-ttu-id="ed0b9-135">Na panelu výsledků vyberte **rozpoznávat**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-135">In the results panel, select **Recognize**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ed0b9-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ed0b9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ed0b9-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s rozpoznávat podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="ed0b9-138">In this section, you configure and test Azure AD single sign-on with Recognize based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ed0b9-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v rozpoznávat je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Recognize is to a user in Azure AD.</span></span> <span data-ttu-id="ed0b9-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v rozpoznávat musí navázat.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-140">In other words, a link relationship between an Azure AD user and the related user in Recognize needs to be established.</span></span>

<span data-ttu-id="ed0b9-141">V rozpoznávat, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-141">In Recognize, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ed0b9-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s rozpoznávat, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="ed0b9-142">To configure and test Azure AD single sign-on with Recognize, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ed0b9-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ed0b9-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ed0b9-145">**[Vytvoření zkušebního uživatele rozpoznávat](#creating-a-recognize-test-user)**  – Pokud chcete mít protějšek Britta Simon v rozpoznávat propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-145">**[Creating a Recognize test user](#creating-a-recognize-test-user)** - to have a counterpart of Britta Simon in Recognize that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ed0b9-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ed0b9-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ed0b9-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ed0b9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ed0b9-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci rozpoznávat.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Recognize application.</span></span>

<span data-ttu-id="ed0b9-150">**Ke konfiguraci Azure AD jednotné přihlašování s rozpoznávat, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ed0b9-150">**To configure Azure AD single sign-on with Recognize, perform the following steps:**</span></span>

1. <span data-ttu-id="ed0b9-151">Na portálu Azure na **rozpoznávat** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-151">In the Azure portal, on the **Recognize** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ed0b9-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_samlbase.png)

3. <span data-ttu-id="ed0b9-155">Na **rozpoznat domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ed0b9-155">On the **Recognize Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_url.png)

    <span data-ttu-id="ed0b9-157">a.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-157">a.</span></span> <span data-ttu-id="ed0b9-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://recognizeapp.com/<your-domain>/saml/sso`</span><span class="sxs-lookup"><span data-stu-id="ed0b9-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://recognizeapp.com/<your-domain>/saml/sso`</span></span>

    <span data-ttu-id="ed0b9-159">b.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-159">b.</span></span> <span data-ttu-id="ed0b9-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://recognizeapp.com/<your-domain>`</span><span class="sxs-lookup"><span data-stu-id="ed0b9-160">In the **Identifier** textbox, type a URL using the following pattern: `https://recognizeapp.com/<your-domain>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ed0b9-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-161">These values are not real.</span></span> <span data-ttu-id="ed0b9-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ed0b9-163">Obraťte se na [tým podpory rozpoznat klienta](mailto:support@recognizeapp.com) získat přihlašovací adresa URL a získáte hodnota identifikátoru po otevření této adresy URL služby zprostředkovatele metadat z části Nastavení jednotného přihlašování, která je vysvětlená později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-163">Contact [Recognize Client support team](mailto:support@recognizeapp.com) to get Sign-On URL and you can get Identifier value by opening the Service Provider Metadata URL from the SSO Settings section that is explained later in the tutorial.</span></span> <span data-ttu-id="ed0b9-164">.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-164">.</span></span> 
 
4. <span data-ttu-id="ed0b9-165">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-165">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_certificate.png) 

5. <span data-ttu-id="ed0b9-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-recognize-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ed0b9-169">Na **rozpoznat konfigurace** klikněte na tlačítko **konfigurace rozpoznat** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-169">On the **Recognize Configuration** section, click **Configure Recognize** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ed0b9-170">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="ed0b9-170">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_configure.png) 

7. <span data-ttu-id="ed0b9-172">V okně prohlížeče jiných webových přihlašování ke klientovi rozpoznávat jako správce.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-172">In a different web browser window, sign-on to your Recognize tenant as an administrator.</span></span>

8. <span data-ttu-id="ed0b9-173">V pravém horním rohu klikněte na tlačítko **nabídky**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-173">On the upper right corner, click **Menu**.</span></span> <span data-ttu-id="ed0b9-174">Přejděte na **společnosti správce**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-174">Go to **Company Admin**.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

9. <span data-ttu-id="ed0b9-176">V levém navigačním podokně klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-176">On the left navigation pane, click **Settings**.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

10. <span data-ttu-id="ed0b9-178">Proveďte následující kroky na **nastavení jednotného přihlašování k** části.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-178">Perform the following steps on **SSO Settings** section.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)
    
    <span data-ttu-id="ed0b9-180">a.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-180">a.</span></span> <span data-ttu-id="ed0b9-181">Jako **povolit jednotné přihlašování**, vyberte **ON**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-181">As **Enable SSO**, select **ON**.</span></span>

    <span data-ttu-id="ed0b9-182">b.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-182">b.</span></span> <span data-ttu-id="ed0b9-183">V **IDP Entity ID** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-183">In the **IDP Entity ID** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="ed0b9-184">c.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-184">c.</span></span> <span data-ttu-id="ed0b9-185">V **jednotné přihlašování cílová adresa url** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-185">In the **Sso target url** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="ed0b9-186">d.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-186">d.</span></span> <span data-ttu-id="ed0b9-187">V **Slo cílová adresa url** textovému poli, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-187">In the **Slo target url** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span> 
    
    <span data-ttu-id="ed0b9-188">e.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-188">e.</span></span> <span data-ttu-id="ed0b9-189">Otevřete váš stažené **certifikátu (Base64)** souboru v programu Poznámkový blok, zkopírujte obsah ho do schránky a vložte jej do **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-189">Open your downloaded **Certificate (Base64)** file in notepad, copy the content of it into your clipboard, and then paste it to the **Certificate** textbox.</span></span>
    
    <span data-ttu-id="ed0b9-190">f.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-190">f.</span></span> <span data-ttu-id="ed0b9-191">Klikněte **uložit nastavení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-191">Click the **Save settings** button.</span></span> 

11. <span data-ttu-id="ed0b9-192">Vedle položky **nastavení jednotného přihlašování k** část, zkopírujte adresu URL v části **adresu url služby zprostředkovatele metadat**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-192">Beside the **SSO Settings** section, copy the URL under **Service Provider Metadata url**.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

12. <span data-ttu-id="ed0b9-194">Otevřete **adresu URL metadat odkaz** pod prázdnou prohlížeč o stažení dokument metadat.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-194">Open the **Metadata URL link** under a blank browser to download the metadata document.</span></span> <span data-ttu-id="ed0b9-195">Pak zkopírujte EntityDescriptor value(entityID) ze souboru a vložte jej do **identifikátor** textového pole v **rozpoznat domény a adresy URL části** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-195">Then copy the EntityDescriptor value(entityID) from the file and paste it in **Identifier** textbox in **Recognize Domain and URLs section** on Azure portal.</span></span>
    
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

> [!TIP]
> <span data-ttu-id="ed0b9-197">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="ed0b9-197">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ed0b9-198">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-198">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ed0b9-199">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ed0b9-199">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ed0b9-200">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ed0b9-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="ed0b9-201">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-201">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ed0b9-203">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ed0b9-203">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ed0b9-204">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-204">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ed0b9-206">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-206">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ed0b9-208">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-208">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ed0b9-210">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ed0b9-210">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ed0b9-212">a.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-212">a.</span></span> <span data-ttu-id="ed0b9-213">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-213">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ed0b9-214">b.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-214">b.</span></span> <span data-ttu-id="ed0b9-215">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-215">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ed0b9-216">c.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-216">c.</span></span> <span data-ttu-id="ed0b9-217">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-217">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ed0b9-218">d.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-218">d.</span></span> <span data-ttu-id="ed0b9-219">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-219">Click **Create**.</span></span>
 
### <a name="creating-a-recognize-test-user"></a><span data-ttu-id="ed0b9-220">Vytvoření zkušebního uživatele rozpoznávat</span><span class="sxs-lookup"><span data-stu-id="ed0b9-220">Creating a Recognize test user</span></span>

<span data-ttu-id="ed0b9-221">Pokud chcete povolit uživatelům Azure AD přihlášení do rozpoznávat, musí být zřízená do rozpoznávat.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-221">In order to enable Azure AD users to log into Recognize, they must be provisioned into Recognize.</span></span> <span data-ttu-id="ed0b9-222">V případě rozpoznávat zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-222">In the case of Recognize, provisioning is a manual task.</span></span>

<span data-ttu-id="ed0b9-223">Tato aplikace nepodporuje SCIM zřizování, ale má synchronizaci alternativní uživatele, která zřizuje uživatele.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-223">This app doesn't support SCIM provisioning but has an alternate user sync that provisions users.</span></span> 

<span data-ttu-id="ed0b9-224">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ed0b9-224">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="ed0b9-225">Přihlaste se k serveru vaší společnosti rozpoznávat jako správce.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-225">Log into your Recognize company site as an administrator.</span></span>

2. <span data-ttu-id="ed0b9-226">V pravém horním rohu klikněte na tlačítko **nabídky**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-226">On the upper right corner, click **Menu**.</span></span> <span data-ttu-id="ed0b9-227">Přejděte na **společnosti správce**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-227">Go to **Company Admin**.</span></span>

3. <span data-ttu-id="ed0b9-228">V levém navigačním podokně klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-228">On the left navigation pane, click **Settings**.</span></span>

4. <span data-ttu-id="ed0b9-229">Proveďte následující kroky na **synchronizace uživatele** části.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-229">Perform the following steps on **User Sync** section.</span></span>
   
   <span data-ttu-id="ed0b9-230">![Nový uživatel](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="ed0b9-230">![New User](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "New User")</span></span>
   
   <span data-ttu-id="ed0b9-231">a.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-231">a.</span></span> <span data-ttu-id="ed0b9-232">Jako **povolená synchronizace**, vyberte **ON**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-232">As **Sync Enabled**, select **ON**.</span></span>
   
   <span data-ttu-id="ed0b9-233">b.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-233">b.</span></span> <span data-ttu-id="ed0b9-234">Jako **zprostředkovatele synchronizace zvolte**, vyberte **Microsoft nebo Office 365**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-234">As **Choose sync provider**, select **Microsoft / Office 365**.</span></span>
   
   <span data-ttu-id="ed0b9-235">c.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-235">c.</span></span> <span data-ttu-id="ed0b9-236">Klikněte na tlačítko **spustíte synchronizaci uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-236">Click **Run User Sync**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ed0b9-237">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ed0b9-237">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ed0b9-238">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k rozpoznání.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-238">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Recognize.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ed0b9-240">**Pokud chcete přiřadit Britta Simon rozpoznávat, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ed0b9-240">**To assign Britta Simon to Recognize, perform the following steps:**</span></span>

1. <span data-ttu-id="ed0b9-241">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-241">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ed0b9-243">V seznamu aplikací vyberte **rozpoznávat**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-243">In the applications list, select **Recognize**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_app.png) 

3. <span data-ttu-id="ed0b9-245">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-245">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ed0b9-247">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-247">Click **Add** button.</span></span> <span data-ttu-id="ed0b9-248">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ed0b9-250">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-250">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ed0b9-251">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ed0b9-252">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ed0b9-253">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ed0b9-253">Testing single sign-on</span></span>

<span data-ttu-id="ed0b9-254">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-254">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ed0b9-255">Když kliknete na dlaždici rozpoznávat na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci rozpoznávat.</span><span class="sxs-lookup"><span data-stu-id="ed0b9-255">When you click the Recognize tile in the Access Panel, you should get automatically signed-on to your Recognize application.</span></span> <span data-ttu-id="ed0b9-256">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ed0b9-256">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed0b9-257">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ed0b9-257">Additional resources</span></span>

* [<span data-ttu-id="ed0b9-258">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ed0b9-258">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ed0b9-259">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ed0b9-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png

