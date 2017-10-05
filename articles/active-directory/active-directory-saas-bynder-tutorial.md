---
title: 'Kurz: Azure Active Directory integrace s Bynder | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Bynder."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 4fb0ab26-b3b9-420a-8072-a0be80ea021e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 6786d7eb6a11405278ef7267f25279f9e39b3bde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bynder"></a><span data-ttu-id="050a9-103">Kurz: Azure Active Directory integrace s Bynder</span><span class="sxs-lookup"><span data-stu-id="050a9-103">Tutorial: Azure Active Directory integration with Bynder</span></span>
<span data-ttu-id="050a9-104">Cílem tohoto kurzu je ukazují, jak integrovat Bynder s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="050a9-104">The objective of this tutorial is to show you how to integrate Bynder with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="050a9-105">Integrace Bynder s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="050a9-105">Integrating Bynder with Azure AD provides you with the following benefits:</span></span>

* <span data-ttu-id="050a9-106">Můžete řídit ve službě Azure AD, který má přístup k Bynder</span><span class="sxs-lookup"><span data-stu-id="050a9-106">You can control in Azure AD who has access to Bynder</span></span>
* <span data-ttu-id="050a9-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Bynder jednotné přihlašování (SSO) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="050a9-107">You can enable your users to automatically get signed-on to Bynder single sign-on (SSO) with their Azure AD accounts</span></span>
* <span data-ttu-id="050a9-108">Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="050a9-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="050a9-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="050a9-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="050a9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="050a9-110">Prerequisites</span></span>
<span data-ttu-id="050a9-111">Konfigurace integrace Azure AD s Bynder, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="050a9-111">To configure Azure AD integration with Bynder, you need the following items:</span></span>

* <span data-ttu-id="050a9-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="050a9-112">An Azure AD subscription</span></span>
* <span data-ttu-id="050a9-113">Předplatné povolené Bynder jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="050a9-113">A Bynder single-sign on (SSO) enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="050a9-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="050a9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="050a9-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="050a9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="050a9-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="050a9-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="050a9-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="050a9-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="050a9-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="050a9-118">Scenario description</span></span>
<span data-ttu-id="050a9-119">Cílem tohoto kurzu je vám umožní Microsoft Azure AD jednotného přihlašování k testování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="050a9-119">The objective of this tutorial is to enable you to test Microsoft Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="050a9-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="050a9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="050a9-121">Přidání Bynder z Galerie</span><span class="sxs-lookup"><span data-stu-id="050a9-121">Adding Bynder from the gallery</span></span>
2. <span data-ttu-id="050a9-122">Konfigurace a testování jednotného přihlašování pro aplikaci Microsoft Azure AD</span><span class="sxs-lookup"><span data-stu-id="050a9-122">Configuring and testing Microsoft Azure AD SSO</span></span>

## <a name="add-bynder-from-the-gallery"></a><span data-ttu-id="050a9-123">Přidat Bynder z Galerie</span><span class="sxs-lookup"><span data-stu-id="050a9-123">Add Bynder from the gallery</span></span>
<span data-ttu-id="050a9-124">Při konfiguraci integrace Bynder do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Bynder z galerie.</span><span class="sxs-lookup"><span data-stu-id="050a9-124">To configure the integration of Bynder into Azure AD, you need to add Bynder from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="050a9-125">**Pokud chcete přidat Bynder z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="050a9-125">**To add Bynder from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="050a9-126">V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="050a9-126">In the **Azure classic Portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="050a9-128">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="050a9-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="050a9-129">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="050a9-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Aplikace][2]
4. <span data-ttu-id="050a9-131">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="050a9-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Aplikace][3]
5. <span data-ttu-id="050a9-133">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="050a9-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Aplikace][4]
6. <span data-ttu-id="050a9-135">Do vyhledávacího pole zadejte **Bynder**.</span><span class="sxs-lookup"><span data-stu-id="050a9-135">In the search box, type **Bynder**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_01.png)
7. <span data-ttu-id="050a9-137">Na panelu výsledků vyberte **Bynder**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="050a9-137">In the results panel, select **Bynder**, and then click **Complete** to add the application.</span></span>
   
    ![Výběr aplikace v galerii](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_001.png)

## <a name="configure-and-test-microsoft-azure-ad-sso"></a><span data-ttu-id="050a9-139">Konfigurace a otestování jednotného přihlašování pro aplikaci Microsoft Azure AD</span><span class="sxs-lookup"><span data-stu-id="050a9-139">Configure and test Microsoft Azure AD SSO</span></span>
<span data-ttu-id="050a9-140">Cílem této části je ukazují, jak nakonfigurovat a otestovat Microsoft Azure AD přihlášení SSO se Bynder podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="050a9-140">The objective of this section is to show you how to configure and test Microsoft Azure AD SSO with Bynder based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="050a9-141">Pro jednotné přihlašování pro práci je potřeba vědět, co uživatel protějškem v Bynder pro uživatele ve službě Azure AD je Azure AD.</span><span class="sxs-lookup"><span data-stu-id="050a9-141">For SSO to work, Azure AD needs to know what the counterpart user in Bynder to an user in Azure AD is.</span></span> <span data-ttu-id="050a9-142">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Bynder musí navázat.</span><span class="sxs-lookup"><span data-stu-id="050a9-142">In other words, a link relationship between an Azure AD user and the related user in Bynder needs to be established.</span></span>

<span data-ttu-id="050a9-143">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Bynder.</span><span class="sxs-lookup"><span data-stu-id="050a9-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Bynder.</span></span>

<span data-ttu-id="050a9-144">Nakonfigurovat a otestovat Microsoft Azure AD přihlášení SSO se Bynder, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="050a9-144">To configure and test Microsoft Azure AD SSO with Bynder, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="050a9-145">**[Konfigurace Microsoft Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="050a9-145">**[Configuring Microsoft Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="050a9-146">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Microsoft Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="050a9-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Microsoft Azure AD Single Sign-On with Britta Simon.</span></span>
3. <span data-ttu-id="050a9-147">**[Vytvoření zkušebního uživatele Bynder](#creating-a-bynder-test-user)**  – Pokud chcete mít protějšek Britta Simon v Bynder propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="050a9-147">**[Creating a Bynder test user](#creating-a-bynder-test-user)** - to have a counterpart of Britta Simon in Bynder that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="050a9-148">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Microsoft Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="050a9-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Microsoft Azure AD Single Sign-On.</span></span>
5. <span data-ttu-id="050a9-149">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="050a9-149">**[Testing single sign-on](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-microsoft-azure-ad-sso"></a><span data-ttu-id="050a9-150">Konfigurace jednotného přihlašování Microsoft Azure AD</span><span class="sxs-lookup"><span data-stu-id="050a9-150">Configuring Microsoft Azure AD SSO</span></span>
<span data-ttu-id="050a9-151">V této části můžete povolit jednotné přihlašování pro aplikaci Microsoft Azure AD na portálu classic a nakonfigurovat jednotné přihlašování v aplikaci Bynder.</span><span class="sxs-lookup"><span data-stu-id="050a9-151">In this section, you enable Microsoft Azure AD SSO in the classic portal and configure SSO in your Bynder application.</span></span>

<span data-ttu-id="050a9-152">**Pokud chcete konfigurovat jednotné přihlašování pro aplikaci Microsoft Azure AD s Bynder, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="050a9-152">**To configure Microsoft Azure AD SSO with Bynder, perform the following steps:**</span></span>

1. <span data-ttu-id="050a9-153">Na portálu classic na **Bynder** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="050a9-153">In the classic portal, on the **Bynder** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurovat jednotné přihlašování][6] 
2. <span data-ttu-id="050a9-155">Na **jak chcete uživatelům se přihlásit Bynder** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="050a9-155">On the **How would you like users to sign on to Bynder** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_03.png)
3. <span data-ttu-id="050a9-157">Na **nakonfigurovat nastavení aplikace** dialogové okno stránky, pokud chcete nakonfigurovat aplikace **IDP iniciované režimu**, proveďte následující kroky a klikněte na **Další**:</span><span class="sxs-lookup"><span data-stu-id="050a9-157">On the **Configure App Settings** dialog page, If you wish to configure the application in **IDP initiated mode**, perform the following steps and click **Next**:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_04.png)
  1. <span data-ttu-id="050a9-159">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.getbynder.com/sso/SAML/authenticate/`</span><span class="sxs-lookup"><span data-stu-id="050a9-159">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.getbynder.com/sso/SAML/authenticate/`</span></span>
  2. <span data-ttu-id="050a9-160">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="050a9-160">Click **Next**.</span></span>
4. <span data-ttu-id="050a9-161">Pokud chcete nakonfigurovat aplikace **SP iniciované režimu** na **nakonfigurovat nastavení aplikace** stránku dialogové okno a potom kliknutím na tlačítko **"Zobrazit upřesňující nastavení (volitelné)"** a pak zadejte **přihlašovací adresa URL** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="050a9-161">If you wish to configure the application in **SP initiated mode** on the **Configure App Settings** dialog page, then click on the **“Show advanced settings (optional)”** and then enter the **Sign On URL** and click **Next**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_10.png)
  1. <span data-ttu-id="050a9-163">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.getbynder.com/login/`</span><span class="sxs-lookup"><span data-stu-id="050a9-163">In the **Sign On URL** textbox, type a URL using the following pattern: `https://<company name>.getbynder.com/login/`</span></span>
  2. <span data-ttu-id="050a9-164">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="050a9-164">Click **Next**.</span></span>
  
   >[!NOTE]
   ><span data-ttu-id="050a9-165">Hodnota pro přihlášení na URL v tomto kurzu je právě placeholfer.</span><span class="sxs-lookup"><span data-stu-id="050a9-165">The value for the Sign On URL in this tutorial is just a placeholfer.</span></span> <span data-ttu-id="050a9-166">Chcete-li získat skutečný vlaue pro vaše prostředí, obraťte se na Bynder.</span><span class="sxs-lookup"><span data-stu-id="050a9-166">To get the actual vlaue for your environment, contact Bynder.</span></span>
   >

5. <span data-ttu-id="050a9-167">Na **nakonfigurovat jednotné přihlašování v Bynder** , proveďte následující kroky a klikněte na tlačítko **Další**:</span><span class="sxs-lookup"><span data-stu-id="050a9-167">On the **Configure single sign-on at Bynder** page, perform the following steps and click **Next**:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_05.png)  
  1. <span data-ttu-id="050a9-169">Klikněte na tlačítko **stáhnout metadata**a potom uložte soubor v počítači.</span><span class="sxs-lookup"><span data-stu-id="050a9-169">Click **Download metadata**, and then save the file on your computer.</span></span>
  2. <span data-ttu-id="050a9-170">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="050a9-170">Click **Next**.</span></span>
6. <span data-ttu-id="050a9-171">Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, kontaktujte tým podpory Bynder.</span><span class="sxs-lookup"><span data-stu-id="050a9-171">To get SSO configured for your application, contact your Bynder support team.</span></span> <span data-ttu-id="050a9-172">Připojte soubor stažený metadat a sdílet je s týmem Bynder na jejich straně nastavit jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="050a9-172">Attach the downloaded metadata file and share it with Bynder team to set up SSO on their side.</span></span>
7. <span data-ttu-id="050a9-173">Klasický portál, vyberte potvrzení konfigurace přihlášení a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="050a9-173">In the classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span>
   
    ![Azure AD jednotné přihlášení][10]
8. <span data-ttu-id="050a9-175">Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="050a9-175">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Azure AD jednotné přihlášení][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="050a9-177">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="050a9-177">Create an Azure AD test user</span></span>
<span data-ttu-id="050a9-178">Cílem této části je vytvoření zkušebního uživatele na portálu classic názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="050a9-178">The objective of this section is to create a test user in the classic portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][20]

<span data-ttu-id="050a9-180">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="050a9-180">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="050a9-181">V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="050a9-181">In the **Azure classic Portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_09.png)
2. <span data-ttu-id="050a9-183">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="050a9-183">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="050a9-184">Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="050a9-184">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_03.png)
4. <span data-ttu-id="050a9-186">Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="050a9-186">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_04.png)
5. <span data-ttu-id="050a9-188">Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="050a9-188">On the **Tell us about this user** dialog page, perform the following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_05.png)
  1. <span data-ttu-id="050a9-190">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="050a9-190">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="050a9-191">V uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="050a9-191">In the User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="050a9-192">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="050a9-192">Click **Next**.</span></span>
6. <span data-ttu-id="050a9-193">Na **profil uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="050a9-193">On the **User Profile** dialog page, perform the following steps:</span></span>
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_06.png)
  1. <span data-ttu-id="050a9-195">V **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="050a9-195">In the **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="050a9-196">V **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="050a9-196">In the **Last Name** textbox, type, **Simon**.</span></span> 
  3. <span data-ttu-id="050a9-197">V **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="050a9-197">In the **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="050a9-198">V **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="050a9-198">In the **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="050a9-199">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="050a9-199">Click **Next**.</span></span>
7. <span data-ttu-id="050a9-200">Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="050a9-200">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_07.png)
8. <span data-ttu-id="050a9-202">Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="050a9-202">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_08.png)
   1. <span data-ttu-id="050a9-204">Poznamenejte si hodnotu **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="050a9-204">Write down the value of the **New Password**.</span></span>
   2. <span data-ttu-id="050a9-205">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="050a9-205">Click **Complete**.</span></span>   

### <a name="create-a-bynder-test-user"></a><span data-ttu-id="050a9-206">Vytvoření zkušebního uživatele Bynder</span><span class="sxs-lookup"><span data-stu-id="050a9-206">Create a Bynder test user</span></span>
<span data-ttu-id="050a9-207">Cílem této části je vytvoření uživatele v Bynder nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="050a9-207">The objective of this section is to create a user called Britta Simon in Bynder.</span></span> <span data-ttu-id="050a9-208">Bynder podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="050a9-208">Bynder supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="050a9-209">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="050a9-209">There is no action item for you in this section.</span></span> <span data-ttu-id="050a9-210">Vytvoří se nový uživatel během pokusu o přístup k Bynder, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="050a9-210">A new user will be created during an attempt to access Bynder if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="050a9-211">Pokud potřebujete vytvořit uživatele s ručně, budete muset kontaktujte tým podpory Bynder.</span><span class="sxs-lookup"><span data-stu-id="050a9-211">If you need to create an user manually, you need to contact the Bynder support team.</span></span> 
> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="050a9-212">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="050a9-212">Assign the Azure AD test user</span></span>
<span data-ttu-id="050a9-213">Cílem této části je povolení Britta Simon používat jednotného přihlašování k Azure tak, že udělíte přístup k Bynder.</span><span class="sxs-lookup"><span data-stu-id="050a9-213">The objective of this section is to enabling Britta Simon to use Azure SSO by granting her access to Bynder.</span></span>

   ![Přiřadit uživatele][200]

<span data-ttu-id="050a9-215">**Pokud chcete přiřadit Britta Simon Bynder, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="050a9-215">**To assign Britta Simon to Bynder, perform the following steps:**</span></span>

1. <span data-ttu-id="050a9-216">Na portálu classic k otevření zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="050a9-216">On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Přiřadit uživatele][201]
2. <span data-ttu-id="050a9-218">V seznamu aplikací vyberte **Bynder**.</span><span class="sxs-lookup"><span data-stu-id="050a9-218">In the applications list, select **Bynder**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_50.png)
3. <span data-ttu-id="050a9-220">V nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="050a9-220">In the menu on the top, click **Users**.</span></span>
   
    ![Přiřadit uživatele][203]
4. <span data-ttu-id="050a9-222">V seznamu uživatelů vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="050a9-222">In the Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="050a9-223">Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="050a9-223">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Přiřadit uživatele][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="050a9-225">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="050a9-225">Test single sign-on</span></span>
<span data-ttu-id="050a9-226">Cílem této části je testování konfigurace jednotného přihlašování pro aplikaci Microsoft Azure AD pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="050a9-226">The objective of this section is to test your Microsoft Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="050a9-227">Když kliknete na dlaždici Bynder na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Bynder.</span><span class="sxs-lookup"><span data-stu-id="050a9-227">When you click the Bynder tile in the Access Panel, you should get automatically signed-on to your Bynder application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="050a9-228">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="050a9-228">Additional resources</span></span>
* [<span data-ttu-id="050a9-229">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="050a9-229">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="050a9-230">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="050a9-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_205.png
