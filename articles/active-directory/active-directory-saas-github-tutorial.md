---
title: 'Kurz: Azure Active Directory integrace s Githubu | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Githubu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4395bd95-05de-4deb-87a5-dc3bc8ac4d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 9dc12bc2e313bcb2000724d035156c5054d14e1c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a><span data-ttu-id="17419-103">Kurz: Azure Active Directory integrace s Githubu</span><span class="sxs-lookup"><span data-stu-id="17419-103">Tutorial: Azure Active Directory integration with GitHub</span></span>

<span data-ttu-id="17419-104">V tomto kurzu zjistěte, jak integrovat Githubu s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="17419-104">In this tutorial, you learn how to integrate GitHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="17419-105">Integrace Githubu s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="17419-105">Integrating GitHub with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="17419-106">Můžete řídit ve službě Azure AD, který má přístup ke Githubu</span><span class="sxs-lookup"><span data-stu-id="17419-106">You can control in Azure AD who has access to GitHub</span></span>
- <span data-ttu-id="17419-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k webu GitHub (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="17419-107">You can enable your users to automatically get signed-on to GitHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="17419-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="17419-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="17419-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="17419-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="17419-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="17419-110">Prerequisites</span></span>

<span data-ttu-id="17419-111">Konfigurace integrace Azure AD s Githubu, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="17419-111">To configure Azure AD integration with GitHub, you need the following items:</span></span>

- <span data-ttu-id="17419-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="17419-112">An Azure AD subscription</span></span>
- <span data-ttu-id="17419-113">Githubu jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="17419-113">A GitHub single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="17419-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="17419-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="17419-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="17419-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="17419-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="17419-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="17419-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="17419-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="17419-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="17419-118">Scenario description</span></span>
<span data-ttu-id="17419-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="17419-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="17419-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="17419-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="17419-121">Přidání Githubu z Galerie</span><span class="sxs-lookup"><span data-stu-id="17419-121">Adding GitHub from the gallery</span></span>
2. <span data-ttu-id="17419-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="17419-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-github-from-the-gallery"></a><span data-ttu-id="17419-123">Přidání Githubu z Galerie</span><span class="sxs-lookup"><span data-stu-id="17419-123">Adding GitHub from the gallery</span></span>
<span data-ttu-id="17419-124">Při konfiguraci integrace Githubu do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Githubu z galerie.</span><span class="sxs-lookup"><span data-stu-id="17419-124">To configure the integration of GitHub into Azure AD, you need to add GitHub from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="17419-125">**Pokud chcete přidat Githubu z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="17419-125">**To add GitHub from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="17419-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="17419-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="17419-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="17419-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="17419-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="17419-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="17419-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="17419-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="17419-133">Do vyhledávacího pole zadejte **webu GitHub.com**.</span><span class="sxs-lookup"><span data-stu-id="17419-133">In the search box, type **GitHub.com**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/tutorial_github_search02.png)

5. <span data-ttu-id="17419-135">Na panelu výsledků vyberte **Githubu**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="17419-135">In the results panel, select **GitHub**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/tutorial_github_search_result02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="17419-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="17419-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="17419-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Githubu podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="17419-138">In this section, you configure and test Azure AD single sign-on with GitHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="17419-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Githubu je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="17419-139">For single sign-on to work, Azure AD needs to know what the counterpart user in GitHub is to a user in Azure AD.</span></span> <span data-ttu-id="17419-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Githubu, je nutné stanovit.</span><span class="sxs-lookup"><span data-stu-id="17419-140">In other words, a link relationship between an Azure AD user and the related user in GitHub needs to be established.</span></span>

<span data-ttu-id="17419-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Githubu.</span><span class="sxs-lookup"><span data-stu-id="17419-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in GitHub.</span></span>

<span data-ttu-id="17419-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Githubu, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="17419-142">To configure and test Azure AD single sign-on with GitHub, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="17419-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="17419-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="17419-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="17419-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="17419-145">**[Vytvoření zkušebního uživatele Githubu](#creating-a-GitHub-test-user)**  – Pokud chcete mít protějšek Britta Simon v Githubu, propojené služby Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="17419-145">**[Creating a GitHub test user](#creating-a-GitHub-test-user)** - to have a counterpart of Britta Simon in GitHub that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="17419-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="17419-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="17419-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="17419-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="17419-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="17419-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="17419-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci Githubu.</span><span class="sxs-lookup"><span data-stu-id="17419-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your GitHub application.</span></span>

<span data-ttu-id="17419-150">**Ke konfiguraci Azure AD jednotné přihlašování s Githubu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="17419-150">**To configure Azure AD single sign-on with GitHub, perform the following steps:**</span></span>

1. <span data-ttu-id="17419-151">Na portálu Azure Management portal na **Githubu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="17419-151">In the Azure Management portal, on the **GitHub** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="17419-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="17419-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_01.png)

3. <span data-ttu-id="17419-155">Na **Githubu domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="17419-155">On the **GitHub Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_saml011.png)

    <span data-ttu-id="17419-157">a.</span><span class="sxs-lookup"><span data-stu-id="17419-157">a.</span></span> <span data-ttu-id="17419-158">V **přihlašovací adresa URL** textovému poli, zadejte hodnotu jako:`https://github.com/orgs/<entity-id>/sso`</span><span class="sxs-lookup"><span data-stu-id="17419-158">In the **Sign-on URL** textbox, type the value as: `https://github.com/orgs/<entity-id>/sso`</span></span>

    <span data-ttu-id="17419-159">b.</span><span class="sxs-lookup"><span data-stu-id="17419-159">b.</span></span> <span data-ttu-id="17419-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://github.com/orgs/<entity-id>`</span><span class="sxs-lookup"><span data-stu-id="17419-160">In the **Identifier** textbox, type a URL using the following pattern: `https://github.com/orgs/<entity-id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="17419-161">Upozorňujeme, že tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="17419-161">Please note that these are not the real values.</span></span> <span data-ttu-id="17419-162">Budete muset aktualizovat tyto hodnoty se skutečné Sing na adresu URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="17419-162">You have to update these values with the actual Sing-on URL and Identifier.</span></span> <span data-ttu-id="17419-163">Zde, doporučujeme vám použít jedinečnou hodnotu řetězce v identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="17419-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="17419-164">Přejděte k části GitHub správce se načtení těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="17419-164">Go to GitHub Admin section to retrieve these values.</span></span> 

4. <span data-ttu-id="17419-165">Na **uživatelské atributy** vyberte **uživatelský identifikátor** jako user.mail.</span><span class="sxs-lookup"><span data-stu-id="17419-165">On the **User Attributes** section, select **User Identifier** as user.mail.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_attribute_new01.png)
    
5. <span data-ttu-id="17419-167">Na **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.</span><span class="sxs-lookup"><span data-stu-id="17419-167">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_03.png)

6. <span data-ttu-id="17419-169">Na **vytvořit nový certifikát** dialogové okno, klikněte na ikonu kalendáři a vyberte **datum vypršení platnosti**.</span><span class="sxs-lookup"><span data-stu-id="17419-169">On the **Create New Certificate** dialog, click the calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="17419-170">Pak klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="17419-170">Then click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_general_300.png)

7. <span data-ttu-id="17419-172">Na **SAML podpisový certifikát** vyberte **aktivujte nový certifikát** a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="17419-172">On the **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_04.png)

8. <span data-ttu-id="17419-174">V místní nabídce **certifikát výměny** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="17419-174">On the pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="17419-176">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="17419-176">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_05.png) 

10. <span data-ttu-id="17419-178">Na **Githubu konfigurace** klikněte na tlačítko **konfigurace Githubu** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="17419-178">On the **GitHub Configuration** section, click **Configure GitHub** to open **Configure sign-on** window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_06.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_07.png)

11. <span data-ttu-id="17419-181">V okně prohlížeče jiný web Přihlaste se jako správce k organizace webu GitHub.</span><span class="sxs-lookup"><span data-stu-id="17419-181">In a different web browser window, log into your GitHub organization site as an administrator.</span></span>

12. <span data-ttu-id="17419-182">Přejděte na **nastavení** a klikněte na tlačítko **zabezpečení**</span><span class="sxs-lookup"><span data-stu-id="17419-182">Navigate to **Settings** and click **Security**</span></span>

    ![Nastavení](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_03.png)

13. <span data-ttu-id="17419-184">Zkontrolujte **ověřování povolit SAML** pole odhalil konfigurační pole jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="17419-184">Check the **Enable SAML authentication** box, revealing the Single Sign-on configuration fields.</span></span> <span data-ttu-id="17419-185">Pak můžete použijte jednu hodnotu adresy URL přihlašování k aktualizaci adresy jednotného přihlašování konfigurace služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="17419-185">Then, use the single sign-on URL value to update the Single sign-on URL on Azure AD configuration.</span></span>

    ![Nastavení](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_13.png)

14. <span data-ttu-id="17419-187">Vyplňte následující pole:</span><span class="sxs-lookup"><span data-stu-id="17419-187">Configure the following fields:</span></span>

    <span data-ttu-id="17419-188">a.</span><span class="sxs-lookup"><span data-stu-id="17419-188">a.</span></span> <span data-ttu-id="17419-189">**Přihlaste na adrese URL**: Zadejte **SAML jednotné přihlašování adresa URL služby** z **konfigurace Githubu** části na Azure AD</span><span class="sxs-lookup"><span data-stu-id="17419-189">**Sign on URL**: Enter **SAML Single sign-on Service URL** from the **Configure GitHub** section on Azure AD</span></span>

    <span data-ttu-id="17419-190">b.</span><span class="sxs-lookup"><span data-stu-id="17419-190">b.</span></span> <span data-ttu-id="17419-191">**Vystavitel**: Zadejte **SAML Entity ID** z **konfigurace Githubu** části na Azure AD</span><span class="sxs-lookup"><span data-stu-id="17419-191">**Issuer**: Enter **SAML Entity ID** from the **Configure GitHub** section on Azure AD</span></span>

    <span data-ttu-id="17419-192">c.</span><span class="sxs-lookup"><span data-stu-id="17419-192">c.</span></span> <span data-ttu-id="17419-193">**Veřejný certifikát**: Otevřete stažený certifikát z Azure AD v programu Poznámkový blok a zkopírujte obsah, včetně "Začít certifikát" a "END CERTIFICATE"</span><span class="sxs-lookup"><span data-stu-id="17419-193">**Public Certificate**: Open the downloaded certificate from Azure AD in a notepad and copy the content including "BEGIN CERTIFICATE" and "END CERTIFICATE"</span></span>

    ![Nastavení](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_051.png)

15. <span data-ttu-id="17419-195">Klikněte na **Konfigurace testu SAML** zkontrolujte, že žádná chyb při ověřování nebo chyby během jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="17419-195">Click on **Test SAML configuration** to confirm that no validation failures or errors during SSO.</span></span>

    ![Nastavení](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_06.png)

16. <span data-ttu-id="17419-197">Klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="17419-197">Click **Save**</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="17419-198">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="17419-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="17419-199">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="17419-199">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="17419-201">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="17419-201">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="17419-202">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="17419-202">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="17419-204">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="17419-204">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="17419-206">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="17419-206">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="17419-208">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="17419-208">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="17419-210">a.</span><span class="sxs-lookup"><span data-stu-id="17419-210">a.</span></span> <span data-ttu-id="17419-211">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="17419-211">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="17419-212">b.</span><span class="sxs-lookup"><span data-stu-id="17419-212">b.</span></span> <span data-ttu-id="17419-213">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="17419-213">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="17419-214">c.</span><span class="sxs-lookup"><span data-stu-id="17419-214">c.</span></span> <span data-ttu-id="17419-215">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="17419-215">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="17419-216">d.</span><span class="sxs-lookup"><span data-stu-id="17419-216">d.</span></span> <span data-ttu-id="17419-217">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="17419-217">Click **Create**.</span></span> 


### <a name="creating-a-github-test-user"></a><span data-ttu-id="17419-218">Vytvoření zkušebního uživatele Githubu</span><span class="sxs-lookup"><span data-stu-id="17419-218">Creating a GitHub test user</span></span>

<span data-ttu-id="17419-219">Pokud chcete povolit uživatelům Azure AD přihlášení na Githubu, musí být zřízená na Githubu.</span><span class="sxs-lookup"><span data-stu-id="17419-219">In order to enable Azure AD users to log into GitHub, they must be provisioned into GitHub.</span></span>  
<span data-ttu-id="17419-220">V případě Githubu zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="17419-220">In the case of GitHub, provisioning is a manual task.</span></span>

<span data-ttu-id="17419-221">**Ke zřízení uživatelských účtů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="17419-221">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="17419-222">Přihlaste se k serveru vaší společnosti Githubu jako správce.</span><span class="sxs-lookup"><span data-stu-id="17419-222">Log in to your GitHub company site as an administrator.</span></span>

2. <span data-ttu-id="17419-223">Klikněte na tlačítko **osoby**.</span><span class="sxs-lookup"><span data-stu-id="17419-223">Click **People**.</span></span>

    <span data-ttu-id="17419-224">![Lidé](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="17419-224">![People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "People")</span></span>

3. <span data-ttu-id="17419-225">Klikněte na tlačítko **pozvání člen**.</span><span class="sxs-lookup"><span data-stu-id="17419-225">Click **Invite member**.</span></span>

    <span data-ttu-id="17419-226">![Uživatele pozvat](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="17419-226">![Invite Users](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "Invite Users")</span></span>

4. <span data-ttu-id="17419-227">Na **pozvání člen** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="17419-227">On the **Invite member** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="17419-228">a.</span><span class="sxs-lookup"><span data-stu-id="17419-228">a.</span></span> <span data-ttu-id="17419-229">V **e-mailu** textovému poli, zadejte e-mailovou adresu účtu Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="17419-229">In the **Email** textbox, type the email address of Britta Simon account.</span></span>

    <span data-ttu-id="17419-230">![Pozvěte lidi, kteří](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="17419-230">![Invite People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "Invite People")</span></span>
    
    <span data-ttu-id="17419-231">b.</span><span class="sxs-lookup"><span data-stu-id="17419-231">b.</span></span> <span data-ttu-id="17419-232">Klikněte na tlačítko **odeslat pozvánky**.</span><span class="sxs-lookup"><span data-stu-id="17419-232">Click **Send Invitation**.</span></span>

    <span data-ttu-id="17419-233">![Pozvěte lidi, kteří](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="17419-233">![Invite People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "Invite People")</span></span>

    > [!NOTE]
    > <span data-ttu-id="17419-234">Držitel účtu Azure Active Directory bude dostávat e-mailu a postupujte podle odkaz potvrďte svůj účet, pak se změní na aktivní.</span><span class="sxs-lookup"><span data-stu-id="17419-234">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="17419-235">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="17419-235">Assigning the Azure AD test user</span></span>

<span data-ttu-id="17419-236">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup ke Githubu.</span><span class="sxs-lookup"><span data-stu-id="17419-236">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to GitHub.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="17419-238">**Pokud chcete přiřadit Britta Simon Githubu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="17419-238">**To assign Britta Simon to GitHub, perform the following steps:**</span></span>

1. <span data-ttu-id="17419-239">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="17419-239">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="17419-241">V seznamu aplikací vyberte **webu GitHub.com**.</span><span class="sxs-lookup"><span data-stu-id="17419-241">In the applications list, select **GitHub.com**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_search_result021.png) 

3. <span data-ttu-id="17419-243">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="17419-243">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="17419-245">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="17419-245">Click **Add** button.</span></span> <span data-ttu-id="17419-246">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="17419-246">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="17419-248">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="17419-248">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="17419-249">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="17419-249">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="17419-250">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="17419-250">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="17419-251">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="17419-251">Testing single sign-on</span></span>

<span data-ttu-id="17419-252">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="17419-252">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="17419-253">Když kliknete na dlaždici Githubu na přístupovém panelu, jste měli získat přihlášení k aplikaci Githubu.</span><span class="sxs-lookup"><span data-stu-id="17419-253">When you click the GitHub tile in the Access Panel, you should get signed-on to your GitHub application.</span></span> <span data-ttu-id="17419-254">Se budete přihlašovat jako účet, ale pak nutné se přihlásit pomocí svého osobního účtu organizace.</span><span class="sxs-lookup"><span data-stu-id="17419-254">You'll be logging in as an Organization account but then need to log in with your personal account.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="17419-255">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="17419-255">Additional resources</span></span>

* [<span data-ttu-id="17419-256">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="17419-256">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="17419-257">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="17419-257">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-github-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-github-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-github-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-github-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-github-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-github-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-github-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-github-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-github-tutorial/tutorial_general_203.png
