---
title: 'Kurz: Azure Active Directory integrace s Slack | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Slack."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 5aca630b2077d3f7d4ce9273ee668ed6a5f9843d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-slack"></a><span data-ttu-id="60f8a-103">Kurz: Azure Active Directory integrace s Slack</span><span class="sxs-lookup"><span data-stu-id="60f8a-103">Tutorial: Azure Active Directory integration with Slack</span></span>

<span data-ttu-id="60f8a-104">V tomto kurzu zjistěte, jak integrovat Slack s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="60f8a-104">In this tutorial, you learn how to integrate Slack with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="60f8a-105">Integrace systému Slack s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="60f8a-105">Integrating Slack with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="60f8a-106">Můžete řídit ve službě Azure AD, který má přístup k systému Slack</span><span class="sxs-lookup"><span data-stu-id="60f8a-106">You can control in Azure AD who has access to Slack</span></span>
- <span data-ttu-id="60f8a-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k systému Slack (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="60f8a-107">You can enable your users to automatically get signed-on to Slack (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="60f8a-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="60f8a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="60f8a-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="60f8a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60f8a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="60f8a-110">Prerequisites</span></span>

<span data-ttu-id="60f8a-111">Konfigurace integrace Azure AD s Slack, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="60f8a-111">To configure Azure AD integration with Slack, you need the following items:</span></span>

- <span data-ttu-id="60f8a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="60f8a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="60f8a-113">Slack jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="60f8a-113">A Slack single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="60f8a-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="60f8a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="60f8a-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="60f8a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="60f8a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="60f8a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="60f8a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="60f8a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="60f8a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="60f8a-118">Scenario description</span></span>
<span data-ttu-id="60f8a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="60f8a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="60f8a-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="60f8a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="60f8a-121">Přidání Slack z Galerie</span><span class="sxs-lookup"><span data-stu-id="60f8a-121">Adding Slack from the gallery</span></span>
2. <span data-ttu-id="60f8a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="60f8a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-slack-from-the-gallery"></a><span data-ttu-id="60f8a-123">Přidání Slack z Galerie</span><span class="sxs-lookup"><span data-stu-id="60f8a-123">Adding Slack from the gallery</span></span>
<span data-ttu-id="60f8a-124">Pokud chcete nakonfigurovat integraci systému Slack do služby Azure AD, potřebujete přidat Slack z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="60f8a-124">To configure the integration of Slack into Azure AD, you need to add Slack from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="60f8a-125">**Pokud chcete přidat Slack z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="60f8a-125">**To add Slack from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="60f8a-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="60f8a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="60f8a-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="60f8a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="60f8a-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="60f8a-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="60f8a-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60f8a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="60f8a-133">Do vyhledávacího pole zadejte **Slack**.</span><span class="sxs-lookup"><span data-stu-id="60f8a-133">In the search box, type **Slack**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-slack-tutorial/tutorial_slack_search.png)

5. <span data-ttu-id="60f8a-135">Na panelu výsledků vyberte **Slack**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="60f8a-135">In the results panel, select **Slack**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-slack-tutorial/tutorial_slack_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="60f8a-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="60f8a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="60f8a-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Slack podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="60f8a-138">In this section, you configure and test Azure AD single sign-on with Slack based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="60f8a-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Slack je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="60f8a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Slack is to a user in Azure AD.</span></span> <span data-ttu-id="60f8a-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Slack musí navázat.</span><span class="sxs-lookup"><span data-stu-id="60f8a-140">In other words, a link relationship between an Azure AD user and the related user in Slack needs to be established.</span></span>

<span data-ttu-id="60f8a-141">V systému Slack, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="60f8a-141">In Slack, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="60f8a-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Slack, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="60f8a-142">To configure and test Azure AD single sign-on with Slack, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="60f8a-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="60f8a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="60f8a-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="60f8a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="60f8a-145">**[Vytváření Slack testovacího uživatele](#creating-a-slack-test-user)**  – Pokud chcete mít protějšek Britta Simon v Slack propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="60f8a-145">**[Creating a Slack test user](#creating-a-slack-test-user)** - to have a counterpart of Britta Simon in Slack that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="60f8a-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="60f8a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="60f8a-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="60f8a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="60f8a-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="60f8a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="60f8a-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Slack.</span><span class="sxs-lookup"><span data-stu-id="60f8a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Slack application.</span></span>

<span data-ttu-id="60f8a-150">**Ke konfiguraci Azure AD jednotné přihlašování s Slack, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="60f8a-150">**To configure Azure AD single sign-on with Slack, perform the following steps:**</span></span>

1. <span data-ttu-id="60f8a-151">Na portálu Azure na **Slack** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="60f8a-151">In the Azure portal, on the **Slack** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="60f8a-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="60f8a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_slack_samlbase.png)

3. <span data-ttu-id="60f8a-155">Na **Slack domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="60f8a-155">On the **Slack Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_slack_url.png)

    <span data-ttu-id="60f8a-157">a.</span><span class="sxs-lookup"><span data-stu-id="60f8a-157">a.</span></span> <span data-ttu-id="60f8a-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.slack.com`</span><span class="sxs-lookup"><span data-stu-id="60f8a-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.slack.com`</span></span>

    <span data-ttu-id="60f8a-159">b.</span><span class="sxs-lookup"><span data-stu-id="60f8a-159">b.</span></span> <span data-ttu-id="60f8a-160">V **identifikátor** textovému poli, zadejte adresu URL:`https://slack.com`</span><span class="sxs-lookup"><span data-stu-id="60f8a-160">In the **Identifier** textbox, type the URL: `https://slack.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="60f8a-161">Hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="60f8a-161">The value is not real.</span></span> <span data-ttu-id="60f8a-162">Budete muset aktualizovat hodnotu s skutečné přihlašovací na adresy URL.</span><span class="sxs-lookup"><span data-stu-id="60f8a-162">You have to update the value with the actual Sign On URL.</span></span> <span data-ttu-id="60f8a-163">Obraťte se na [tým podpory Slack](https://slack.com/help/contact) k získání hodnoty</span><span class="sxs-lookup"><span data-stu-id="60f8a-163">Contact [Slack support team](https://slack.com/help/contact) to get the value</span></span>
     
4. <span data-ttu-id="60f8a-164">Slack aplikace očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="60f8a-164">Slack application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="60f8a-165">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="60f8a-165">Configure the following claims for this application.</span></span> <span data-ttu-id="60f8a-166">Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="60f8a-166">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="60f8a-167">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="60f8a-167">The following screenshot shows an example for this.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute.png)

5. <span data-ttu-id="60f8a-169">V **uživatelské atributy** části na **jednotného přihlašování** dialogovém okně, vyberte **user.mail** jako **uživatelský identifikátor** a pro každý řádek v tabulce níže, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="60f8a-169">In the **User Attributes** section on the **Single sign-on** dialog, select **user.mail**  as **User Identifier** and for each row shown in the table below, perform the following steps:</span></span>
    
    | <span data-ttu-id="60f8a-170">Název atributu</span><span class="sxs-lookup"><span data-stu-id="60f8a-170">Attribute Name</span></span> | <span data-ttu-id="60f8a-171">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="60f8a-171">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="60f8a-172">křestní_jméno</span><span class="sxs-lookup"><span data-stu-id="60f8a-172">first_name</span></span> | <span data-ttu-id="60f8a-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="60f8a-173">user.givenname</span></span> |
    | <span data-ttu-id="60f8a-174">Příjmení</span><span class="sxs-lookup"><span data-stu-id="60f8a-174">last_name</span></span> | <span data-ttu-id="60f8a-175">User.Surname</span><span class="sxs-lookup"><span data-stu-id="60f8a-175">user.surname</span></span> |
    | <span data-ttu-id="60f8a-176">User.Email</span><span class="sxs-lookup"><span data-stu-id="60f8a-176">User.Email</span></span> | <span data-ttu-id="60f8a-177">User.Mail</span><span class="sxs-lookup"><span data-stu-id="60f8a-177">user.mail</span></span> |  
    | <span data-ttu-id="60f8a-178">User.Username</span><span class="sxs-lookup"><span data-stu-id="60f8a-178">User.Username</span></span> | <span data-ttu-id="60f8a-179">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="60f8a-179">user.userprincipalname</span></span> |

    <span data-ttu-id="60f8a-180">a.</span><span class="sxs-lookup"><span data-stu-id="60f8a-180">a.</span></span> <span data-ttu-id="60f8a-181">Klikněte na **atribut** otevřete **Upravit atribut** dialogové okno pole a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="60f8a-181">Click on **Attribute** to open **Edit Attribute** dialog box and perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute1.png)

    <span data-ttu-id="60f8a-183">a.</span><span class="sxs-lookup"><span data-stu-id="60f8a-183">a.</span></span> <span data-ttu-id="60f8a-184">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="60f8a-184">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="60f8a-185">b.</span><span class="sxs-lookup"><span data-stu-id="60f8a-185">b.</span></span> <span data-ttu-id="60f8a-186">Z **hodnotu** vyberte hodnotu atributu zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="60f8a-186">From the **Value** list, select the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="60f8a-187">c.</span><span class="sxs-lookup"><span data-stu-id="60f8a-187">c.</span></span> <span data-ttu-id="60f8a-188">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="60f8a-188">Click **OK**</span></span>

6. <span data-ttu-id="60f8a-189">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="60f8a-189">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_slack_certificate.png)

7. <span data-ttu-id="60f8a-191">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="60f8a-191">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="60f8a-193">Na **Slack konfigurace** klikněte na tlačítko **konfigurace Slack** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="60f8a-193">On the **Slack Configuration** section, click **Configure Slack** to open **Configure sign-on** window.</span></span> <span data-ttu-id="60f8a-194">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="60f8a-194">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_slack_configure.png) 

9.  <span data-ttu-id="60f8a-196">V okně prohlížeče jiný web Přihlaste se k webu Slack společnosti jako správce.</span><span class="sxs-lookup"><span data-stu-id="60f8a-196">In a different web browser window, log in to your Slack company site as an administrator.</span></span>

10.  <span data-ttu-id="60f8a-197">Přejděte na **Microsoft Azure AD** pak přejděte na **nastavení Team**.</span><span class="sxs-lookup"><span data-stu-id="60f8a-197">Navigate to **Microsoft Azure AD** then go to **Team Settings**.</span></span>

     ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-slack-tutorial/tutorial_slack_001.png)

11.  <span data-ttu-id="60f8a-199">V **nastavení Team** klikněte na položku **ověřování** a pak klikněte **změnit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="60f8a-199">In the **Team Settings** section, click the **Authentication** tab, and then click **Change Settings**.</span></span>

     ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-slack-tutorial/tutorial_slack_002.png)

12. <span data-ttu-id="60f8a-201">Na **nastavení ověřování SAML** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="60f8a-201">On the **SAML Authentication Settings** dialog, perform the following steps:</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-slack-tutorial/tutorial_slack_003.png)

    <span data-ttu-id="60f8a-203">a.</span><span class="sxs-lookup"><span data-stu-id="60f8a-203">a.</span></span>  <span data-ttu-id="60f8a-204">V **SAML 2.0 koncový bod (HTTP)** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="60f8a-204">In the **SAML 2.0 Endpoint (HTTP)** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="60f8a-205">b.</span><span class="sxs-lookup"><span data-stu-id="60f8a-205">b.</span></span>  <span data-ttu-id="60f8a-206">V **vystavitele zprostředkovatele Identity** textovému poli, vložte hodnotu **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="60f8a-206">In the **Identity Provider Issuer** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="60f8a-207">c.</span><span class="sxs-lookup"><span data-stu-id="60f8a-207">c.</span></span>  <span data-ttu-id="60f8a-208">Otevřete soubor stažený certifikátu v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte jej do **veřejný certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="60f8a-208">Open your downloaded certificate file in notepad, copy the content of it into your clipboard, and then paste it to the **Public Certificate** textbox.</span></span>

    <span data-ttu-id="60f8a-209">d.</span><span class="sxs-lookup"><span data-stu-id="60f8a-209">d.</span></span> <span data-ttu-id="60f8a-210">Konfiguruje nastavení výše uvedených tří vhodnou pro váš tým Slack.</span><span class="sxs-lookup"><span data-stu-id="60f8a-210">Configure the above three settings as appropriate for your Slack team.</span></span> <span data-ttu-id="60f8a-211">Další informace o nastavení najít **příručce Konfigurace jednotného přihlašování k systému Slack na** sem.</span><span class="sxs-lookup"><span data-stu-id="60f8a-211">For more information about the settings, please find the **Slack's SSO configuration guide** here.</span></span> `https://get.slack.help/hc/articles/220403548-Guide-to-single-sign-on-with-Slack%60`

    <span data-ttu-id="60f8a-212">e.</span><span class="sxs-lookup"><span data-stu-id="60f8a-212">e.</span></span>  <span data-ttu-id="60f8a-213">Klikněte na tlačítko **uložte konfiguraci**.</span><span class="sxs-lookup"><span data-stu-id="60f8a-213">Click **Save Configuration**.</span></span>
     
    <!-- Deselect **Allow users to change their email address**.

    e.  Select **Allow users to choose their own username**.

    f.  As **Authentication for your team must be used by**, select **It’s optional**. -->

> [!TIP]
> <span data-ttu-id="60f8a-214">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="60f8a-214">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="60f8a-215">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="60f8a-215">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="60f8a-216">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="60f8a-216">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="60f8a-217">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="60f8a-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="60f8a-218">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="60f8a-218">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="60f8a-220">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="60f8a-220">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="60f8a-221">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="60f8a-221">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="60f8a-223">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="60f8a-223">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="60f8a-225">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60f8a-225">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="60f8a-227">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="60f8a-227">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="60f8a-229">a.</span><span class="sxs-lookup"><span data-stu-id="60f8a-229">a.</span></span> <span data-ttu-id="60f8a-230">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="60f8a-230">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="60f8a-231">b.</span><span class="sxs-lookup"><span data-stu-id="60f8a-231">b.</span></span> <span data-ttu-id="60f8a-232">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="60f8a-232">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="60f8a-233">c.</span><span class="sxs-lookup"><span data-stu-id="60f8a-233">c.</span></span> <span data-ttu-id="60f8a-234">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="60f8a-234">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="60f8a-235">d.</span><span class="sxs-lookup"><span data-stu-id="60f8a-235">d.</span></span> <span data-ttu-id="60f8a-236">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="60f8a-236">Click **Create**.</span></span>
 
### <a name="creating-a-slack-test-user"></a><span data-ttu-id="60f8a-237">Vytváření Slack zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="60f8a-237">Creating a Slack test user</span></span>

<span data-ttu-id="60f8a-238">Cílem této části je vytvoření uživatele volal Britta Simon v Slack.</span><span class="sxs-lookup"><span data-stu-id="60f8a-238">The objective of this section is to create a user called Britta Simon in Slack.</span></span> <span data-ttu-id="60f8a-239">Slack podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="60f8a-239">Slack supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="60f8a-240">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="60f8a-240">There is no action item for you in this section.</span></span> <span data-ttu-id="60f8a-241">Nový uživatel se vytvoří během pokusu o přístup k systému Slack, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="60f8a-241">A new user is created during an attempt to access Slack if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="60f8a-242">Pokud potřebujete ručně vytvořit uživatele, musíte kontaktovat [tým podpory Slack](https://slack.com/help/contact).</span><span class="sxs-lookup"><span data-stu-id="60f8a-242">If you need to create a user manually, you need to Contact [Slack support team](https://slack.com/help/contact).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="60f8a-243">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="60f8a-243">Assigning the Azure AD test user</span></span>

<span data-ttu-id="60f8a-244">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k systému Slack.</span><span class="sxs-lookup"><span data-stu-id="60f8a-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Slack.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="60f8a-246">**Britta Simon přiřadit k systému Slack, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="60f8a-246">**To assign Britta Simon to Slack, perform the following steps:**</span></span>

1. <span data-ttu-id="60f8a-247">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="60f8a-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="60f8a-249">V seznamu aplikací vyberte **Slack**.</span><span class="sxs-lookup"><span data-stu-id="60f8a-249">In the applications list, select **Slack**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_slack_app.png) 

3. <span data-ttu-id="60f8a-251">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="60f8a-251">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="60f8a-253">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="60f8a-253">Click **Add** button.</span></span> <span data-ttu-id="60f8a-254">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60f8a-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="60f8a-256">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="60f8a-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="60f8a-257">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60f8a-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="60f8a-258">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60f8a-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="60f8a-259">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="60f8a-259">Testing single sign-on</span></span>

<span data-ttu-id="60f8a-260">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="60f8a-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="60f8a-261">Když kliknete na dlaždici Slack na přístupovém panelu jste měli získat automaticky přihlášení k aplikaci Slack.</span><span class="sxs-lookup"><span data-stu-id="60f8a-261">When you click the Slack tile in the Access Panel, you should get automatically signed-on to your Slack application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="60f8a-262">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="60f8a-262">Additional resources</span></span>

* [<span data-ttu-id="60f8a-263">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="60f8a-263">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="60f8a-264">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="60f8a-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-slack-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-slack-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-slack-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-slack-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-slack-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-slack-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-slack-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-slack-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-slack-tutorial/tutorial_general_203.png

