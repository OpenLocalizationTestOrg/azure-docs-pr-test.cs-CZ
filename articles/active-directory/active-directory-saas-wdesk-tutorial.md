---
title: 'Kurz: Azure Active Directory integrace s Wdesk | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Wdesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 37660b80cfb01d6a3105aea5ce248f1e03c46695
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wdesk"></a><span data-ttu-id="95cbd-103">Kurz: Azure Active Directory integrace s Wdesk</span><span class="sxs-lookup"><span data-stu-id="95cbd-103">Tutorial: Azure Active Directory integration with Wdesk</span></span>

<span data-ttu-id="95cbd-104">V tomto kurzu zjistěte, jak integrovat Wdesk s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="95cbd-104">In this tutorial, you learn how to integrate Wdesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="95cbd-105">Integrace Wdesk s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="95cbd-105">Integrating Wdesk with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="95cbd-106">Můžete řídit ve službě Azure AD, který má přístup k Wdesk</span><span class="sxs-lookup"><span data-stu-id="95cbd-106">You can control in Azure AD who has access to Wdesk</span></span>
- <span data-ttu-id="95cbd-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Wdesk (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="95cbd-107">You can enable your users to automatically get signed-on to Wdesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="95cbd-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="95cbd-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="95cbd-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, přečtěte si téma.</span><span class="sxs-lookup"><span data-stu-id="95cbd-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="95cbd-110">[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="95cbd-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95cbd-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="95cbd-111">Prerequisites</span></span>

<span data-ttu-id="95cbd-112">Konfigurace integrace Azure AD s Wdesk, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="95cbd-112">To configure Azure AD integration with Wdesk, you need the following items:</span></span>

- <span data-ttu-id="95cbd-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="95cbd-113">An Azure AD subscription</span></span>
- <span data-ttu-id="95cbd-114">Wdesk jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="95cbd-114">A Wdesk single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="95cbd-115">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="95cbd-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="95cbd-116">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="95cbd-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="95cbd-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="95cbd-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="95cbd-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="95cbd-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="95cbd-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="95cbd-119">Scenario description</span></span>
<span data-ttu-id="95cbd-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="95cbd-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="95cbd-121">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="95cbd-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="95cbd-122">Přidání Wdesk z Galerie</span><span class="sxs-lookup"><span data-stu-id="95cbd-122">Adding Wdesk from the gallery</span></span>
2. <span data-ttu-id="95cbd-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="95cbd-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-wdesk-from-the-gallery"></a><span data-ttu-id="95cbd-124">Přidání Wdesk z Galerie</span><span class="sxs-lookup"><span data-stu-id="95cbd-124">Adding Wdesk from the gallery</span></span>
<span data-ttu-id="95cbd-125">Při konfiguraci integrace Wdesk do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Wdesk z galerie.</span><span class="sxs-lookup"><span data-stu-id="95cbd-125">To configure the integration of Wdesk into Azure AD, you need to add Wdesk from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="95cbd-126">**Pokud chcete přidat Wdesk z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="95cbd-126">**To add Wdesk from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="95cbd-127">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="95cbd-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="95cbd-129">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="95cbd-130">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-130">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="95cbd-132">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="95cbd-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="95cbd-134">Do vyhledávacího pole zadejte **Wdesk**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-134">In the search box, type **Wdesk**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_search.png)

5. <span data-ttu-id="95cbd-136">Na panelu výsledků vyberte **Wdesk**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="95cbd-136">In the results panel, select **Wdesk**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="95cbd-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="95cbd-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="95cbd-139">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Wdesk podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="95cbd-139">In this section, you configure and test Azure AD single sign-on with Wdesk based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="95cbd-140">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Wdesk je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="95cbd-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Wdesk is to a user in Azure AD.</span></span> <span data-ttu-id="95cbd-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Wdesk musí navázat.</span><span class="sxs-lookup"><span data-stu-id="95cbd-141">In other words, a link relationship between an Azure AD user and the related user in Wdesk needs to be established.</span></span>

<span data-ttu-id="95cbd-142">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Wdesk.</span><span class="sxs-lookup"><span data-stu-id="95cbd-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Wdesk.</span></span>

<span data-ttu-id="95cbd-143">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Wdesk, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="95cbd-143">To configure and test Azure AD single sign-on with Wdesk, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="95cbd-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="95cbd-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="95cbd-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="95cbd-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="95cbd-146">**[Vytvoření zkušebního uživatele Wdesk](#creating-a-wdesk-test-user)**  – Pokud chcete mít protějšek Britta Simon v Wdesk propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="95cbd-146">**[Creating a Wdesk test user](#creating-a-wdesk-test-user)** - to have a counterpart of Britta Simon in Wdesk that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="95cbd-147">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="95cbd-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="95cbd-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="95cbd-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="95cbd-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="95cbd-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="95cbd-150">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Wdesk.</span><span class="sxs-lookup"><span data-stu-id="95cbd-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Wdesk application.</span></span>

<span data-ttu-id="95cbd-151">**Ke konfiguraci Azure AD jednotné přihlašování s Wdesk, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="95cbd-151">**To configure Azure AD single sign-on with Wdesk, perform the following steps:**</span></span>

1. <span data-ttu-id="95cbd-152">Na portálu Azure na **Wdesk** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-152">In the Azure portal, on the **Wdesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="95cbd-154">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="95cbd-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_samlbase.png)

3. <span data-ttu-id="95cbd-156">Na **Wdesk domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP** initiated režimu proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="95cbd-156">On the **Wdesk Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url.png)

    <span data-ttu-id="95cbd-158">a.</span><span class="sxs-lookup"><span data-stu-id="95cbd-158">a.</span></span> <span data-ttu-id="95cbd-159">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="95cbd-159">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`</span></span>

    <span data-ttu-id="95cbd-160">b.</span><span class="sxs-lookup"><span data-stu-id="95cbd-160">b.</span></span> <span data-ttu-id="95cbd-161">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="95cbd-161">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`</span></span>

4. <span data-ttu-id="95cbd-162">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-162">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="95cbd-163">Pokud chcete nakonfigurovat aplikace **SP** iniciované režimu provést následující krok:</span><span class="sxs-lookup"><span data-stu-id="95cbd-163">If you wish to configure the application in **SP** initiated mode, perform the following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url1.png)

    <span data-ttu-id="95cbd-165">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="95cbd-165">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="95cbd-166">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="95cbd-166">These values are not real.</span></span> <span data-ttu-id="95cbd-167">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="95cbd-167">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="95cbd-168">Tyto hodnoty z portálu WDesk získat nakonfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="95cbd-168">You get these values from WDesk portal when you configure the SSO.</span></span> 
  
5. <span data-ttu-id="95cbd-169">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="95cbd-169">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_certificate.png) 

6. <span data-ttu-id="95cbd-171">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="95cbd-171">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="95cbd-173">V okně prohlížeče jiný web a přihlášení k Wdesk jako správce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="95cbd-173">In a different web browser window, login to Wdesk as a Security Administrator.</span></span>

8. <span data-ttu-id="95cbd-174">V dolní části vlevo, klikněte na **správce** a zvolte **správce účtu**:</span><span class="sxs-lookup"><span data-stu-id="95cbd-174">In the bottom left, click **Admin** and choose **Account Admin**:</span></span>
 
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

9. <span data-ttu-id="95cbd-176">V Wdesk správce, přejděte na **zabezpečení**, pak **SAML** > **SAML nastavení**:</span><span class="sxs-lookup"><span data-stu-id="95cbd-176">In Wdesk Admin, navigate to **Security**, then **SAML** > **SAML Settings**:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

10. <span data-ttu-id="95cbd-178">V části **obecné nastavení**, zkontrolujte **povolit SAML jednotné přihlašování**:</span><span class="sxs-lookup"><span data-stu-id="95cbd-178">Under **General Settings**, check the **Enable SAML Single Sign On**:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

11. <span data-ttu-id="95cbd-180">V části **podrobné informace o poskytovateli služby**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="95cbd-180">Under **Service Provider Details**, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      <span data-ttu-id="95cbd-182">a.</span><span class="sxs-lookup"><span data-stu-id="95cbd-182">a.</span></span> <span data-ttu-id="95cbd-183">Kopírování **přihlašovací adresa URL** a vložte jej do **přihlašovací adresa Url** textové pole na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="95cbd-183">Copy the **Login URL** and paste it in **Sign-on Url** textbox on Azure portal.</span></span>
   
      <span data-ttu-id="95cbd-184">b.</span><span class="sxs-lookup"><span data-stu-id="95cbd-184">b.</span></span> <span data-ttu-id="95cbd-185">Kopírování **adresu Url metadat** a vložte jej do **identifikátor** textové pole na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="95cbd-185">Copy the **Metadata Url** and paste it in **Identifier** textbox on Azure portal.</span></span>
       
      <span data-ttu-id="95cbd-186">c.</span><span class="sxs-lookup"><span data-stu-id="95cbd-186">c.</span></span> <span data-ttu-id="95cbd-187">Kopírování **příjemce url** a vložte jej do **adresa Url odpovědi** textové pole na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="95cbd-187">Copy the **Consumer url** and paste it in **Reply Url** textbox on Azure portal.</span></span>
   
      <span data-ttu-id="95cbd-188">d.</span><span class="sxs-lookup"><span data-stu-id="95cbd-188">d.</span></span> <span data-ttu-id="95cbd-189">Klikněte na tlačítko **Uložit** na portálu Azure a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="95cbd-189">Click **Save** on Azure portal to save the changes.</span></span>      

12. <span data-ttu-id="95cbd-190">Klikněte na tlačítko **konfigurovat nastavení IdP** otevřete **upravit nastavení IdP** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="95cbd-190">Click **Configure IdP Settings** to open **Edit IdP Settings** dialog.</span></span> <span data-ttu-id="95cbd-191">Klikněte na tlačítko **zvolit soubor** najít **Metadata.xml** soubor uložit z portálu Azure, pak nahrajte ho.</span><span class="sxs-lookup"><span data-stu-id="95cbd-191">Click **Choose File** to locate the **Metadata.xml** file you saved from Azure portal, then upload it.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
13. <span data-ttu-id="95cbd-193">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-193">Click **Save changes**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

> [!TIP]
> <span data-ttu-id="95cbd-195">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="95cbd-195">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="95cbd-196">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="95cbd-196">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="95cbd-197">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="95cbd-197">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="95cbd-198">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="95cbd-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="95cbd-199">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="95cbd-199">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="95cbd-201">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="95cbd-201">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="95cbd-202">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="95cbd-202">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="95cbd-204">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-204">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="95cbd-206">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="95cbd-206">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="95cbd-208">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="95cbd-208">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="95cbd-210">a.</span><span class="sxs-lookup"><span data-stu-id="95cbd-210">a.</span></span> <span data-ttu-id="95cbd-211">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-211">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="95cbd-212">b.</span><span class="sxs-lookup"><span data-stu-id="95cbd-212">b.</span></span> <span data-ttu-id="95cbd-213">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="95cbd-213">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="95cbd-214">c.</span><span class="sxs-lookup"><span data-stu-id="95cbd-214">c.</span></span> <span data-ttu-id="95cbd-215">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-215">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="95cbd-216">d.</span><span class="sxs-lookup"><span data-stu-id="95cbd-216">d.</span></span> <span data-ttu-id="95cbd-217">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-217">Click **Create**.</span></span>
 
### <a name="creating-a-wdesk-test-user"></a><span data-ttu-id="95cbd-218">Vytvoření zkušebního uživatele Wdesk</span><span class="sxs-lookup"><span data-stu-id="95cbd-218">Creating a Wdesk test user</span></span>

<span data-ttu-id="95cbd-219">Pokud chcete povolit uživatelům Azure AD přihlášení k Wdesk, musí být zřízená do Wdesk.</span><span class="sxs-lookup"><span data-stu-id="95cbd-219">To enable Azure AD users to log in to Wdesk, they must be provisioned into Wdesk.</span></span> <span data-ttu-id="95cbd-220">V Wdesk zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="95cbd-220">In Wdesk, provisioning is a manual task.</span></span>

<span data-ttu-id="95cbd-221">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="95cbd-221">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="95cbd-222">Přihlaste se k Wdesk jako správce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="95cbd-222">Log in to Wdesk as a Security Administrator.</span></span>
2. <span data-ttu-id="95cbd-223">Přejděte na **správce** > **účet správce**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-223">Navigate to **Admin** > **Account Admin**.</span></span>

     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. <span data-ttu-id="95cbd-225">Klikněte na tlačítko **členy** pod **osoby**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-225">Click **Members** under **People**.</span></span>

4. <span data-ttu-id="95cbd-226">Klikněte na tlačítko **přidat člena** otevřete **přidat člena** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="95cbd-226">Now click **Add Member** to open **Add Member** dialog box.</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser1.png)  

5. <span data-ttu-id="95cbd-228">V **uživatele** textové pole, zadejte uživatelské jméno uživatele jako  **brittasimon@contoso.com**  a klikněte na tlačítko **pokračovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="95cbd-228">In **User** text box, enter the username of user like **brittasimon@contoso.com** and click **Continue** button.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser3.png)

6.  <span data-ttu-id="95cbd-230">Zadejte podrobnosti, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="95cbd-230">Enter the details as shown below:</span></span>
  
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser4.png)
 
    <span data-ttu-id="95cbd-232">a.</span><span class="sxs-lookup"><span data-stu-id="95cbd-232">a.</span></span> <span data-ttu-id="95cbd-233">V **e-mailu** textové pole, zadejte e-mailu uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="95cbd-233">In **E-mail** text box, enter the email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="95cbd-234">b.</span><span class="sxs-lookup"><span data-stu-id="95cbd-234">b.</span></span> <span data-ttu-id="95cbd-235">V **křestní jméno** textové pole, zadejte jméno uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-235">In **First Name** text box, enter the first name of user like **Britta**.</span></span>

    <span data-ttu-id="95cbd-236">c.</span><span class="sxs-lookup"><span data-stu-id="95cbd-236">c.</span></span> <span data-ttu-id="95cbd-237">V **příjmení** text zadejte příjmení uživatele jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-237">In **Last Name** text box, enter the last name of user like **Simon**.</span></span>

7. <span data-ttu-id="95cbd-238">Klikněte na tlačítko **uložit člen** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="95cbd-238">Click **Save Member** button.</span></span>  

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser5.png)

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="95cbd-240">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="95cbd-240">Assigning the Azure AD test user</span></span>

<span data-ttu-id="95cbd-241">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Wdesk.</span><span class="sxs-lookup"><span data-stu-id="95cbd-241">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Wdesk.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="95cbd-243">**Pokud chcete přiřadit Britta Simon Wdesk, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="95cbd-243">**To assign Britta Simon to Wdesk, perform the following steps:**</span></span>

1. <span data-ttu-id="95cbd-244">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-244">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="95cbd-246">V seznamu aplikací vyberte **Wdesk**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-246">In the applications list, select **Wdesk**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_app.png) 

3. <span data-ttu-id="95cbd-248">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="95cbd-248">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="95cbd-250">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="95cbd-250">Click **Add** button.</span></span> <span data-ttu-id="95cbd-251">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="95cbd-251">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="95cbd-253">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="95cbd-253">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="95cbd-254">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="95cbd-254">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="95cbd-255">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="95cbd-255">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="95cbd-256">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="95cbd-256">Testing single sign-on</span></span>

<span data-ttu-id="95cbd-257">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="95cbd-257">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="95cbd-258">Když kliknete na dlaždici Wdesk na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Wdesk.</span><span class="sxs-lookup"><span data-stu-id="95cbd-258">When you click the Wdesk tile in the Access Panel, you should get automatically signed-on to your Wdesk application.</span></span>
<span data-ttu-id="95cbd-259">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="95cbd-259">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="95cbd-260">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="95cbd-260">Additional resources</span></span>

* [<span data-ttu-id="95cbd-261">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="95cbd-261">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="95cbd-262">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="95cbd-262">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_203.png

