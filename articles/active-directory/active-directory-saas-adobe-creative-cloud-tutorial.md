---
title: 'Kurz: Azure Active Directory integrace s Adobe Creative cloudu | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Adobe Creative cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ba1171e-56b1-4475-b308-58637d35e5a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 3d13608612c77236346b0e98551d7fc427d602e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a><span data-ttu-id="a757e-103">Kurz: Azure Active Directory integrace s Adobe Creative cloudu</span><span class="sxs-lookup"><span data-stu-id="a757e-103">Tutorial: Azure Active Directory integration with Adobe Creative Cloud</span></span>

<span data-ttu-id="a757e-104">V tomto kurzu zjistěte, jak integrovat Adobe Creative cloudu s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a757e-104">In this tutorial, you learn how to integrate Adobe Creative Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a757e-105">Integrace Adobe Creative cloudu s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a757e-105">Integrating Adobe Creative Cloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a757e-106">Můžete řídit ve službě Azure AD, který má přístup do cloudu tvůrčí Adobe</span><span class="sxs-lookup"><span data-stu-id="a757e-106">You can control in Azure AD who has access to Adobe Creative Cloud</span></span>
- <span data-ttu-id="a757e-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Adobe Creative cloudu (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a757e-107">You can enable your users to automatically get signed-on to Adobe Creative Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a757e-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="a757e-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="a757e-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a757e-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a757e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a757e-110">Prerequisites</span></span>

<span data-ttu-id="a757e-111">Konfigurace integrace Azure AD s Adobe Creative cloudu, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a757e-111">To configure Azure AD integration with Adobe Creative Cloud, you need the following items:</span></span>

- <span data-ttu-id="a757e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a757e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a757e-113">Tvůrčí cloudu Adobe jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a757e-113">A Adobe Creative Cloud single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a757e-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a757e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a757e-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a757e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a757e-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="a757e-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="a757e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a757e-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a757e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a757e-118">Scenario description</span></span>
<span data-ttu-id="a757e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a757e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a757e-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a757e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a757e-121">Přidání Adobe Creative cloudu z Galerie</span><span class="sxs-lookup"><span data-stu-id="a757e-121">Adding Adobe Creative Cloud from the gallery</span></span>
2. <span data-ttu-id="a757e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a757e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adobe-creative-cloud-from-the-gallery"></a><span data-ttu-id="a757e-123">Přidání Adobe Creative cloudu z Galerie</span><span class="sxs-lookup"><span data-stu-id="a757e-123">Adding Adobe Creative Cloud from the gallery</span></span>
<span data-ttu-id="a757e-124">Při konfiguraci Integrace Adobe Creative cloudu do služby Azure AD, potřebujete přidat Adobe Creative cloudu z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a757e-124">To configure the integration of Adobe Creative Cloud into Azure AD, you need to add Adobe Creative Cloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a757e-125">**Pokud chcete přidat Adobe Creative cloudu z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="a757e-125">**To add Adobe Creative Cloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a757e-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a757e-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a757e-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a757e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a757e-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a757e-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a757e-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a757e-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a757e-133">Do vyhledávacího pole zadejte **Adobe Creative cloudu**.</span><span class="sxs-lookup"><span data-stu-id="a757e-133">In the search box, type **Adobe Creative Cloud**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_000.png)

5. <span data-ttu-id="a757e-135">Na panelu výsledků vyberte **Adobe Creative cloudu**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a757e-135">In the results panel, select **Adobe Creative Cloud**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a757e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a757e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a757e-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Adobe Creative cloudu podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a757e-138">In this section, you configure and test Azure AD single sign-on with Adobe Creative Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a757e-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v cloudu tvůrčí Adobe je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a757e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Adobe Creative Cloud is to a user in Azure AD.</span></span> <span data-ttu-id="a757e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v cloudu tvůrčí Adobe musí navázat.</span><span class="sxs-lookup"><span data-stu-id="a757e-140">In other words, a link relationship between an Azure AD user and the related user in Adobe Creative Cloud needs to be established.</span></span>

<span data-ttu-id="a757e-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Adobe Creative cloudu.</span><span class="sxs-lookup"><span data-stu-id="a757e-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Adobe Creative Cloud.</span></span>

<span data-ttu-id="a757e-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Adobe Creative cloudu, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a757e-142">To configure and test Azure AD single sign-on with Adobe Creative Cloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a757e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a757e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a757e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a757e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a757e-145">**[Vytváření testovacího uživatele Adobe Creative cloudu](#creating-an-adobe-creative-cloud-test-user)**  – Pokud chcete mít protějšek Britta Simon Adobe Creative cloudu, který je propojený s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="a757e-145">**[Creating an Adobe Creative Cloud test user](#creating-an-adobe-creative-cloud-test-user)** - to have a counterpart of Britta Simon in Adobe Creative Cloud that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="a757e-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a757e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a757e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a757e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a757e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a757e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a757e-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci Adobe Creative cloudu.</span><span class="sxs-lookup"><span data-stu-id="a757e-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Adobe Creative Cloud application.</span></span>

<span data-ttu-id="a757e-150">**Ke konfiguraci Azure AD jednotné přihlašování s Adobe Creative cloudu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a757e-150">**To configure Azure AD single sign-on with Adobe Creative Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="a757e-151">Na portálu Azure Management portal na **Adobe Creative cloudu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a757e-151">In the Azure Management portal, on the **Adobe Creative Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a757e-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="a757e-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_01.png)

3. <span data-ttu-id="a757e-155">Na **Adobe Creative cloudové domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="a757e-155">On the **Adobe Creative Cloud Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url1.png)

    <span data-ttu-id="a757e-157">a.</span><span class="sxs-lookup"><span data-stu-id="a757e-157">a.</span></span> <span data-ttu-id="a757e-158">V **identifikátor** textovému poli, zadejte hodnotu jako:`https://www.okta.com/saml2/service-provider/<token>`</span><span class="sxs-lookup"><span data-stu-id="a757e-158">In the **Identifier** textbox, type the value as: `https://www.okta.com/saml2/service-provider/<token>`</span></span>

    <span data-ttu-id="a757e-159">b.</span><span class="sxs-lookup"><span data-stu-id="a757e-159">b.</span></span> <span data-ttu-id="a757e-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.okta.com/auth/saml20/accauthlinktest`</span><span class="sxs-lookup"><span data-stu-id="a757e-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.okta.com/auth/saml20/accauthlinktest`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a757e-161">Upozorňujeme, že tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a757e-161">Please note that these are not the real values.</span></span> <span data-ttu-id="a757e-162">Budete muset aktualizovat tyto hodnoty se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a757e-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="a757e-163">Zde, doporučujeme vám použít jedinečnou hodnotu řetězce v identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="a757e-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="a757e-164">Pokud potřebujete vytvořit uživatele s ručně, budete muset kontaktujte tým podpory Adobe Creative cloudu.</span><span class="sxs-lookup"><span data-stu-id="a757e-164">If you need to create an user manually, you need to contact the Adobe Creative Cloud support team.</span></span>

4. <span data-ttu-id="a757e-165">Na **Adobe Creative cloudové domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="a757e-165">On the **Adobe Creative Cloud Domain and URLs** section, perform the following steps if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url2.png)

    <span data-ttu-id="a757e-167">a.</span><span class="sxs-lookup"><span data-stu-id="a757e-167">a.</span></span> <span data-ttu-id="a757e-168">Klikněte na **zobrazit upřesňující nastavení adresy URL** možnost</span><span class="sxs-lookup"><span data-stu-id="a757e-168">Click on the **Show advanced URL settings** option</span></span>

    <span data-ttu-id="a757e-169">b.</span><span class="sxs-lookup"><span data-stu-id="a757e-169">b.</span></span> <span data-ttu-id="a757e-170">V **přihlašovací adresa URL** textovému poli, zadejte hodnotu jako:`https://adobe.com`</span><span class="sxs-lookup"><span data-stu-id="a757e-170">In the **Sign-on URL** textbox, type the value as: `https://adobe.com`</span></span>

5. <span data-ttu-id="a757e-171">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="a757e-171">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_05.png) 

6. <span data-ttu-id="a757e-173">Na **Adobe Creative konfigurace cloudu** klikněte na tlačítko **konfigurace cloudu tvůrčí Adobe** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a757e-173">On the **Adobe Creative Cloud Configuration** section, click **Configure Adobe Creative Cloud** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a757e-174">Zkopírujte **SAML Entity Id** a **adresa URL služby Jednotné přihlašování SAML** z Stručná referenční příručka oddílu.</span><span class="sxs-lookup"><span data-stu-id="a757e-174">Please copy the **SAML Entity Id** and **SAML SSO Service URL** from Quick Reference section.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_06.png) 

7. <span data-ttu-id="a757e-176">V okně prohlížeče jiný web přihlášení jako správce klienta Adobe Creative cloudu.</span><span class="sxs-lookup"><span data-stu-id="a757e-176">In a different web browser window, sign-on to your Adobe Creative Cloud tenant as an administrator.</span></span>

8.  <span data-ttu-id="a757e-177">Přejděte na **Identity** na levém navigačním podokně a klikněte na doménu.</span><span class="sxs-lookup"><span data-stu-id="a757e-177">Go to **Identity** on the left navigation pane and click your domain.</span></span> <span data-ttu-id="a757e-178">Potom proveďte následující kroky na **jeden znak na konfigurace požadované** části.</span><span class="sxs-lookup"><span data-stu-id="a757e-178">Then perform the following steps on **Single Sign On Configuration Required** section.</span></span>

    <span data-ttu-id="a757e-179">![Nastavení](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="a757e-179">![Settings](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "Settings")</span></span>

9. <span data-ttu-id="a757e-180">Klikněte na tlačítko **Procházet** na kterou odešlete certifikát stažený z Azure AD **IDP certifikát**.</span><span class="sxs-lookup"><span data-stu-id="a757e-180">Click **Browse** to upload the downloaded certificate from Azure AD to **IDP Certificate**.</span></span>

10. <span data-ttu-id="a757e-181">V **IDP vystavitele** textovému poli, vložte hodnotu **SAML Entity Id** který jste zkopírovali ze **konfigurovat přihlášení** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a757e-181">In the **IDP issuer** textbox, put the value of **SAML Entity Id** which you copied from **Configure sign-on** section in Azure portal.</span></span>

11. <span data-ttu-id="a757e-182">V **IDP přihlašovací adresa URL** textovému poli, vložte hodnotu **adresa URL služby Jednotné přihlašování SAML** který jste zkopírovali ze **konfigurovat přihlášení** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a757e-182">In the **IDP Login URL** textbox, put the value of **SAML SSO Service URL** which you copied from **Configure sign-on** section in Azure portal.</span></span>

12. <span data-ttu-id="a757e-183">Vyberte **HTTP - přesměrování** jako **IDP vazby**.</span><span class="sxs-lookup"><span data-stu-id="a757e-183">Select **HTTP - Redirect** as **IDP Binding**.</span></span>

13. <span data-ttu-id="a757e-184">Vyberte **e-mailová adresa** jako **nastavení přihlášení uživatele**.</span><span class="sxs-lookup"><span data-stu-id="a757e-184">Select **Email Address** as **User Login Setting**.</span></span>
 
14. <span data-ttu-id="a757e-185">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a757e-185">Click **Save** button.</span></span>

15. <span data-ttu-id="a757e-186">Řídicí panel bude nyní k dispozici soubor XML **"Stáhnout Metadata"** souboru.</span><span class="sxs-lookup"><span data-stu-id="a757e-186">The dashboard will now present the XML **"Download Metadata"** file.</span></span> <span data-ttu-id="a757e-187">Obsahuje EntityDescriptor adresy URL a adresy URL AssertionConsumerService na Adobe.</span><span class="sxs-lookup"><span data-stu-id="a757e-187">It contains Adobe’s EntityDescriptor URL and AssertionConsumerService URL.</span></span> <span data-ttu-id="a757e-188">Otevřete soubor a nakonfigurujete je v aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a757e-188">Please open the file and configure them in the Azure AD application.</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_002.png)

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    <span data-ttu-id="a757e-191">a.</span><span class="sxs-lookup"><span data-stu-id="a757e-191">a.</span></span> <span data-ttu-id="a757e-192">Použijte hodnotu EntityDescriptor Adobe zadaný pro **identifikátor** na **nakonfigurovat nastavení aplikace** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a757e-192">Use the EntityDescriptor value Adobe provided you for **Identifier** on the **Configure App Settings** dialog.</span></span>

    <span data-ttu-id="a757e-193">b.</span><span class="sxs-lookup"><span data-stu-id="a757e-193">b.</span></span> <span data-ttu-id="a757e-194">Použijte hodnotu AssertionConsumerService Adobe zadaný pro **adresa URL odpovědi** na **nakonfigurovat nastavení aplikace** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a757e-194">Use the AssertionConsumerService value Adobe provided you for **Reply URL** on the **Configure App Settings** dialog.</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a757e-195">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a757e-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="a757e-196">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a757e-196">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a757e-198">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a757e-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a757e-199">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a757e-199">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a757e-201">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a757e-201">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a757e-203">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a757e-203">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a757e-205">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a757e-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a757e-207">a.</span><span class="sxs-lookup"><span data-stu-id="a757e-207">a.</span></span> <span data-ttu-id="a757e-208">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a757e-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a757e-209">b.</span><span class="sxs-lookup"><span data-stu-id="a757e-209">b.</span></span> <span data-ttu-id="a757e-210">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a757e-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a757e-211">c.</span><span class="sxs-lookup"><span data-stu-id="a757e-211">c.</span></span> <span data-ttu-id="a757e-212">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a757e-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a757e-213">d.</span><span class="sxs-lookup"><span data-stu-id="a757e-213">d.</span></span> <span data-ttu-id="a757e-214">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a757e-214">Click **Create**.</span></span> 

### <a name="creating-an-adobe-creative-cloud-test-user"></a><span data-ttu-id="a757e-215">Vytváření testovacího uživatele Adobe Creative cloudu</span><span class="sxs-lookup"><span data-stu-id="a757e-215">Creating an Adobe Creative Cloud test user</span></span>

<span data-ttu-id="a757e-216">Pokud chcete povolit uživatelům Azure AD přihlášení do cloudu tvůrčí Adobe, musí být zřízená do Adobe Creative cloudu.</span><span class="sxs-lookup"><span data-stu-id="a757e-216">In order to enable Azure AD users to log into Adobe Creative Cloud, they must be provisioned into Adobe Creative Cloud.</span></span>  
<span data-ttu-id="a757e-217">V případě Adobe Creative cloudu zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="a757e-217">In the case of Adobe Creative Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="a757e-218">**Ke zřízení uživatelských účtů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a757e-218">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="a757e-219">Přihlaste se na váš web společnosti Adobe Creative Cloud jako správce.</span><span class="sxs-lookup"><span data-stu-id="a757e-219">Log in to your Adobe Creative Cloud company site as an administrator.</span></span>

2. <span data-ttu-id="a757e-220">Klikněte na tlačítko **osoby**.</span><span class="sxs-lookup"><span data-stu-id="a757e-220">Click **People**.</span></span>

    <span data-ttu-id="a757e-221">![Lidé](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="a757e-221">![People](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "People")</span></span>

3. <span data-ttu-id="a757e-222">Klikněte na tlačítko **pozvat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="a757e-222">Click **Invite User**.</span></span>

    <span data-ttu-id="a757e-223">![Uživatele pozvat](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="a757e-223">![Invite Users](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "Invite Users")</span></span>

4. <span data-ttu-id="a757e-224">Na **pozvat osoby** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a757e-224">On the **Invite People** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="a757e-225">![Pozvěte lidi, kteří](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="a757e-225">![Invite People](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "Invite People")</span></span>

    <span data-ttu-id="a757e-226">a.</span><span class="sxs-lookup"><span data-stu-id="a757e-226">a.</span></span> <span data-ttu-id="a757e-227">V **e-mailu** textovému poli, zadejte e-mailovou adresu účtu Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a757e-227">In the **Email** textbox, type the email address of Britta Simon account.</span></span>
    
    <span data-ttu-id="a757e-228">b.</span><span class="sxs-lookup"><span data-stu-id="a757e-228">b.</span></span> <span data-ttu-id="a757e-229">Klikněte na tlačítko **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="a757e-229">Click **Invite**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a757e-230">Držitel účtu Azure Active Directory bude dostávat e-mailu a postupujte podle odkaz potvrďte svůj účet, pak se změní na aktivní.</span><span class="sxs-lookup"><span data-stu-id="a757e-230">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a757e-231">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a757e-231">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a757e-232">V této části povolíte Britta Simon používat tak, že udělíte přístup do cloudu tvůrčí Adobe Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a757e-232">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Adobe Creative Cloud.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a757e-234">**Přiřadit Britta Simon Adobe Creative cloudu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a757e-234">**To assign Britta Simon to Adobe Creative Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="a757e-235">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a757e-235">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a757e-237">V seznamu aplikací vyberte **Adobe Creative cloudu**.</span><span class="sxs-lookup"><span data-stu-id="a757e-237">In the applications list, select **Adobe Creative Cloud**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_50.png) 

3. <span data-ttu-id="a757e-239">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a757e-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a757e-241">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a757e-241">Click **Add** button.</span></span> <span data-ttu-id="a757e-242">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a757e-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a757e-244">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a757e-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a757e-245">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a757e-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a757e-246">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a757e-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a757e-247">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a757e-247">Testing single sign-on</span></span>

<span data-ttu-id="a757e-248">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a757e-248">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a757e-249">Když kliknete na dlaždici Adobe Creative cloudu na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Adobe Creative cloudu.</span><span class="sxs-lookup"><span data-stu-id="a757e-249">When you click the Adobe Creative Cloud tile in the Access Panel, you should get automatically signed-on to your Adobe Creative Cloud application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="a757e-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a757e-250">Additional resources</span></span>

* [<span data-ttu-id="a757e-251">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a757e-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a757e-252">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a757e-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_203.png