---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti Soonr | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Soonr síti na pracovišti."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na
ms.assetid: b75f5f00-ea8b-4850-ae2e-134e5d678d97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 76946e4af624d70f2202601ee935523ca3db4314
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a><span data-ttu-id="fd45c-103">Kurz: Azure Active Directory integrace s Soonr síti na pracovišti</span><span class="sxs-lookup"><span data-stu-id="fd45c-103">Tutorial: Azure Active Directory integration with Soonr Workplace</span></span>

<span data-ttu-id="fd45c-104">Cílem tohoto kurzu je ukazují, jak k síti na pracovišti Soonr integraci s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fd45c-104">The objective of this tutorial is to show you how to integrate Soonr Workplace with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="fd45c-105">Integrace Soonr síti na pracovišti s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="fd45c-105">Integrating Soonr Workplace with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="fd45c-106">Můžete řídit ve službě Azure AD, který má přístup k síti na pracovišti Soonr</span><span class="sxs-lookup"><span data-stu-id="fd45c-106">You can control in Azure AD who has access to Soonr Workplace</span></span>
- <span data-ttu-id="fd45c-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k síti na pracovišti Soonr (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd45c-107">You can enable your users to automatically get signed-on to Soonr Workplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fd45c-108">Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="fd45c-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="fd45c-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fd45c-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fd45c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fd45c-110">Prerequisites</span></span>

<span data-ttu-id="fd45c-111">Konfigurace integrace Azure AD s Soonr síti na pracovišti, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="fd45c-111">To configure Azure AD integration with Soonr Workplace, you need the following items:</span></span>

- <span data-ttu-id="fd45c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd45c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fd45c-113">Síti na pracovišti Soonr jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="fd45c-113">A Soonr Workplace single-sign on enabled subscription</span></span>


> [!NOTE] 
> <span data-ttu-id="fd45c-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="fd45c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="fd45c-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="fd45c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fd45c-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="fd45c-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="fd45c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fd45c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="fd45c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="fd45c-118">Scenario description</span></span>
<span data-ttu-id="fd45c-119">Cílem tohoto kurzu je vám umožní testování Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="fd45c-119">The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="fd45c-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="fd45c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fd45c-121">Přidání Soonr síti na pracovišti z Galerie</span><span class="sxs-lookup"><span data-stu-id="fd45c-121">Adding Soonr Workplace from the gallery</span></span>
2. <span data-ttu-id="fd45c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="fd45c-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-soonr-workplace-from-the-gallery"></a><span data-ttu-id="fd45c-123">Přidání Soonr síti na pracovišti z Galerie</span><span class="sxs-lookup"><span data-stu-id="fd45c-123">Adding Soonr Workplace from the gallery</span></span>
<span data-ttu-id="fd45c-124">Při konfiguraci integrace síti na pracovišti Soonr do služby Azure AD potřebujete přidat Soonr síti na pracovišti z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="fd45c-124">To configure the integration of Soonr Workplace into Azure AD, you need to add Soonr Workplace from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="fd45c-125">**Pokud chcete přidat Soonr síti na pracovišti z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fd45c-125">**To add Soonr Workplace from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="fd45c-126">V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fd45c-128">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="fd45c-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="fd45c-129">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="fd45c-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Aplikace][2]

4. <span data-ttu-id="fd45c-131">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="fd45c-131">Click **Add** at the bottom of the page.</span></span>

    ![Aplikace][3]

5. <span data-ttu-id="fd45c-133">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
 
    ![Aplikace][4]

6. <span data-ttu-id="fd45c-135">Do vyhledávacího pole zadejte **Soonr síti na pracovišti**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-135">In the search box, type **Soonr Workplace**.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_01.png)

7. <span data-ttu-id="fd45c-137">V podokně výsledků vyberte **síti na pracovišti Soonr**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="fd45c-137">In the results pane, select **Soonr Workplace**, and then click **Complete** to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fd45c-139">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="fd45c-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="fd45c-140">Cílem této části je ukazují, jak nakonfigurovat a otestovat Azure AD jednotné přihlašování se Soonr pracovišti na základě testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="fd45c-140">The objective of this section is to show you how to configure and test Azure AD single sign-on with Soonr Workplace based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fd45c-141">Azure AD pro jednotné přihlašování pro práci, musí vědět, co je příslušného uživatele v síti na pracovišti Soonr pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fd45c-141">For single sign-on to work, Azure AD needs to know what the counterpart user in Soonr Workplace to an user in Azure AD is.</span></span> <span data-ttu-id="fd45c-142">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživateli v síti na pracovišti Soonr musí navázat.</span><span class="sxs-lookup"><span data-stu-id="fd45c-142">In other words, a link relationship between an Azure AD user and the related user in Soonr Workplace needs to be established.</span></span>  

<span data-ttu-id="fd45c-143">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v síti na pracovišti Soonr.</span><span class="sxs-lookup"><span data-stu-id="fd45c-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Soonr Workplace.</span></span>

<span data-ttu-id="fd45c-144">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Soonr síti na pracovišti, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="fd45c-144">To configure and test Azure AD single sign-on with Soonr Workplace, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="fd45c-145">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="fd45c-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="fd45c-146">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fd45c-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fd45c-147">**[Vytvoření zkušebního uživatele síti na pracovišti Soonr](#creating-a-soonr-workplace-test-user)**  – Pokud chcete mít protějšek Britta Simon v síti na pracovišti Soonr propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="fd45c-147">**[Creating a Soonr Workplace test user](#creating-a-soonr-workplace-test-user)** - to have a counterpart of Britta Simon in Soonr Workplace that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="fd45c-148">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fd45c-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fd45c-149">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fd45c-149">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fd45c-150">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="fd45c-150">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fd45c-151">V této části můžete povolit Azure AD jednotného přihlašování na portálu classic a nakonfigurovat jednotné přihlašování v aplikaci Soonr síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="fd45c-151">In this section, you enable Azure AD single sign-on in the classic portal and configure single sign-on in your Soonr Workplace application.</span></span>


<span data-ttu-id="fd45c-152">**Ke konfiguraci Azure AD jednotné přihlašování s Soonr síti na pracovišti, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fd45c-152">**To configure Azure AD single sign-on with Soonr Workplace, perform the following steps:**</span></span>

1. <span data-ttu-id="fd45c-153">Na portálu Azure classic na **síti na pracovišti Soonr** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fd45c-153">In the Azure classic portal, on the **Soonr Workplace** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>

    ![Konfigurovat jednotné přihlašování][6] 

2. <span data-ttu-id="fd45c-155">Na **jak chcete uživatelům přihlásit se k síti na pracovišti Soonr** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-155">On the **How would you like users to sign on to Soonr Workplace** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_03.png) 

3. <span data-ttu-id="fd45c-157">Na **nakonfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:.</span><span class="sxs-lookup"><span data-stu-id="fd45c-157">On the **Configure App Settings** dialog page, perform the following steps:.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_04.png) 

    <span data-ttu-id="fd45c-159">a.</span><span class="sxs-lookup"><span data-stu-id="fd45c-159">a.</span></span> <span data-ttu-id="fd45c-160">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.</span><span class="sxs-lookup"><span data-stu-id="fd45c-160">In the **Sign On URL** textbox, type a URL using the following pattern: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.</span></span>

    <span data-ttu-id="fd45c-161">b.</span><span class="sxs-lookup"><span data-stu-id="fd45c-161">b.</span></span> <span data-ttu-id="fd45c-162">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-162">Click **Next**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fd45c-163">Upozorňujeme, že se nejedná skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="fd45c-163">Please note that this is not the real value.</span></span> <span data-ttu-id="fd45c-164">Budete muset aktualizovat tuto hodnotu s skutečné přihlašovací na adresy URL.</span><span class="sxs-lookup"><span data-stu-id="fd45c-164">You have to update this value with the actual Sign On URL.</span></span> <span data-ttu-id="fd45c-165">Kontaktujte tým podpory Soonr síti na pracovišti, chcete-li získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fd45c-165">Contact Soonr Workplace support team to get this value.</span></span>

4. <span data-ttu-id="fd45c-166">Na **nakonfigurovat jednotné přihlašování v síti na pracovišti Soonr** klikněte na tlačítko **stáhnout metadata** a uložte soubor v počítači:</span><span class="sxs-lookup"><span data-stu-id="fd45c-166">On the **Configure single sign-on at Soonr Workplace** page, click **Download metadata** and then save the file on your computer:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_05.png) 

5. <span data-ttu-id="fd45c-168">Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, kontaktujte tým podpory Soonr síti na pracovišti a poskytněte s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="fd45c-168">To get SSO configured for your application, contact your Soonr Workplace support team and provide them with the following:</span></span> 

    <span data-ttu-id="fd45c-169">• Stažené **Metadata** souboru</span><span class="sxs-lookup"><span data-stu-id="fd45c-169">•  The downloaded **Metadata** file</span></span>

    <span data-ttu-id="fd45c-170">• **URL vystavitele**</span><span class="sxs-lookup"><span data-stu-id="fd45c-170">•  The **Issuer URL**</span></span>

    <span data-ttu-id="fd45c-171">• **URL jednotné přihlašování SAML**</span><span class="sxs-lookup"><span data-stu-id="fd45c-171">•  The **SAML SSO URL**</span></span>

    <span data-ttu-id="fd45c-172">• **Jedna adresa URL služby odhlášení**</span><span class="sxs-lookup"><span data-stu-id="fd45c-172">•  The **Single Sign-Out Service URL**</span></span>

    >[!NOTE]
    ><span data-ttu-id="fd45c-173">Tato aplikace je nahrazena <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">síti na pracovišti Autotask</a> a najdete <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">to</a> kurzu konfigurace aplikace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fd45c-173">This application is superseded by <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">Autotask Workplace</a> and you can refer <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">this</a> tutorial for configuring the application with Azure AD.</span></span>
   
6. <span data-ttu-id="fd45c-174">Portál Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-174">In the Azure classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span>

    ![Azure AD jednotné přihlášení][10]

7. <span data-ttu-id="fd45c-176">Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-176">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
  
    ![Azure AD jednotné přihlášení][11]



### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fd45c-178">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd45c-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="fd45c-179">Cílem této části je vytvoření zkušebního uživatele na portálu Azure classic názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fd45c-179">The objective of this section is to create a test user in the Azure classic portal called Britta Simon.</span></span>  

![Vytvořit uživatele Azure AD][20]

<span data-ttu-id="fd45c-181">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fd45c-181">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="fd45c-182">V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-182">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="fd45c-184">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="fd45c-184">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="fd45c-185">Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-185">To display the list of users, in the menu on the top, click **Users**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fd45c-187">Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-187">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="fd45c-189">Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fd45c-189">On the **Tell us about this user** dialog page, perform the following steps:</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_05.png) 

    <span data-ttu-id="fd45c-191">a.</span><span class="sxs-lookup"><span data-stu-id="fd45c-191">a.</span></span> <span data-ttu-id="fd45c-192">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="fd45c-192">As Type Of User, select New user in your organization.</span></span>

    <span data-ttu-id="fd45c-193">b.</span><span class="sxs-lookup"><span data-stu-id="fd45c-193">b.</span></span> <span data-ttu-id="fd45c-194">V uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-194">In the User Name **textbox**, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fd45c-195">c.</span><span class="sxs-lookup"><span data-stu-id="fd45c-195">c.</span></span> <span data-ttu-id="fd45c-196">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-196">Click **Next**.</span></span>

6.  <span data-ttu-id="fd45c-197">Na **profil uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fd45c-197">On the **User Profile** dialog page, perform the following steps:</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_06.png) 

    <span data-ttu-id="fd45c-199">a.</span><span class="sxs-lookup"><span data-stu-id="fd45c-199">a.</span></span> <span data-ttu-id="fd45c-200">V **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-200">In the **First Name** textbox, type **Britta**.</span></span>  

    <span data-ttu-id="fd45c-201">b.</span><span class="sxs-lookup"><span data-stu-id="fd45c-201">b.</span></span> <span data-ttu-id="fd45c-202">V **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-202">In the **Last Name** textbox, type, **Simon**.</span></span>

    <span data-ttu-id="fd45c-203">c.</span><span class="sxs-lookup"><span data-stu-id="fd45c-203">c.</span></span> <span data-ttu-id="fd45c-204">V **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-204">In the **Display Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="fd45c-205">d.</span><span class="sxs-lookup"><span data-stu-id="fd45c-205">d.</span></span> <span data-ttu-id="fd45c-206">V **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-206">In the **Role** list, select **User**.</span></span>

    <span data-ttu-id="fd45c-207">e.</span><span class="sxs-lookup"><span data-stu-id="fd45c-207">e.</span></span> <span data-ttu-id="fd45c-208">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-208">Click **Next**.</span></span>

7. <span data-ttu-id="fd45c-209">Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-209">On the **Get temporary password** dialog page, click **create**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="fd45c-211">Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fd45c-211">On the **Get temporary password** dialog page, perform the following steps:</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_08.png) 

    <span data-ttu-id="fd45c-213">a.</span><span class="sxs-lookup"><span data-stu-id="fd45c-213">a.</span></span> <span data-ttu-id="fd45c-214">Poznamenejte si hodnotu **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-214">Write down the value of the **New Password**.</span></span>

    <span data-ttu-id="fd45c-215">b.</span><span class="sxs-lookup"><span data-stu-id="fd45c-215">b.</span></span> <span data-ttu-id="fd45c-216">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-216">Click **Complete**.</span></span>   



### <a name="creating-a-soonr-workplace-test-user"></a><span data-ttu-id="fd45c-217">Vytvoření zkušebního uživatele Soonr síti na pracovišti</span><span class="sxs-lookup"><span data-stu-id="fd45c-217">Creating a Soonr Workplace test user</span></span>

<span data-ttu-id="fd45c-218">Cílem této části je vytvoření uživatele v síti na pracovišti Soonr nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fd45c-218">The objective of this section is to create a user called Britta Simon in Soonr Workplace.</span></span> <span data-ttu-id="fd45c-219">Spojte se s tým podpory Soonr síti na pracovišti a vytvořte uživatele v platformu.</span><span class="sxs-lookup"><span data-stu-id="fd45c-219">Please work with Soonr Workplace support team to create a user in the platform.</span></span> <span data-ttu-id="fd45c-220">Můžete zvýšit lístku podpory s Soonr z <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">zde</a>.</span><span class="sxs-lookup"><span data-stu-id="fd45c-220">You can raise the support ticket with Soonr from <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">here</a>.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="fd45c-221">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd45c-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="fd45c-222">Cílem této části je povolení Britta Simon používat tak, že udělíte přístup k síti na pracovišti Soonr Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fd45c-222">The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to Soonr Workplace.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="fd45c-224">**Britta Simon přiřadit k síti na pracovišti Soonr, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fd45c-224">**To assign Britta Simon to Soonr Workplace, perform the following steps:**</span></span>

1. <span data-ttu-id="fd45c-225">Na portálu Azure classic, otevřete zobrazení aplikací, v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="fd45c-225">On the Azure classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="fd45c-227">V seznamu aplikací vyberte **Soonr síti na pracovišti**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-227">In the applications list, select **Soonr Workplace**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_50.png) 

1. <span data-ttu-id="fd45c-229">V nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-229">In the menu on the top, click **Users**.</span></span>

    ![Přiřadit uživatele][203] 

1. <span data-ttu-id="fd45c-231">V seznamu uživatelů vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-231">In the Users list, select **Britta Simon**.</span></span>

2. <span data-ttu-id="fd45c-232">Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="fd45c-232">In the toolbar on the bottom, click **Assign**.</span></span>

    ![Přiřadit uživatele][205]



### <a name="testing-single-sign-on"></a><span data-ttu-id="fd45c-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="fd45c-234">Testing single sign-on</span></span>

<span data-ttu-id="fd45c-235">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="fd45c-235">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="fd45c-236">Když kliknete na dlaždici Soonr síti na pracovišti na přístupovém panelu, můžete by měl získat automaticky přihlášení k síti na pracovišti Soonr aplikace.</span><span class="sxs-lookup"><span data-stu-id="fd45c-236">When you click the Soonr Workplace tile in the Access Panel, you should get automatically signed-on to your Soonr Workplace application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="fd45c-237">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="fd45c-237">Additional resources</span></span>

* [<span data-ttu-id="fd45c-238">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fd45c-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fd45c-239">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fd45c-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_205.png
