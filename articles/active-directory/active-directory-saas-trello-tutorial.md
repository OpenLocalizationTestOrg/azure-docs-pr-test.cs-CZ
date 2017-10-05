---
title: 'Kurz: Azure Active Directory integrace s Trello | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Trello."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cd5ae365-9ed6-43a6-920b-f7814b993949
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: d93667f16f2d72995e4a42e79e9125b8e3f6b07c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-trello"></a><span data-ttu-id="40b5e-103">Kurz: Azure Active Directory integrace s Trello</span><span class="sxs-lookup"><span data-stu-id="40b5e-103">Tutorial: Azure Active Directory integration with Trello</span></span>

<span data-ttu-id="40b5e-104">V tomto kurzu zjistěte, jak integrovat Trello s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="40b5e-104">In this tutorial, you learn how to integrate Trello with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="40b5e-105">Integrace Trello s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="40b5e-105">Integrating Trello with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="40b5e-106">Můžete řídit ve službě Azure AD, který má přístup k Trello</span><span class="sxs-lookup"><span data-stu-id="40b5e-106">You can control in Azure AD who has access to Trello</span></span>
- <span data-ttu-id="40b5e-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Trello (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="40b5e-107">You can enable your users to automatically get signed-on to Trello (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="40b5e-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="40b5e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="40b5e-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="40b5e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40b5e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="40b5e-110">Prerequisites</span></span>

<span data-ttu-id="40b5e-111">Konfigurace integrace Azure AD s Trello, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="40b5e-111">To configure Azure AD integration with Trello, you need the following items:</span></span>

- <span data-ttu-id="40b5e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="40b5e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="40b5e-113">Trello jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="40b5e-113">A Trello single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="40b5e-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="40b5e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="40b5e-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="40b5e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="40b5e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="40b5e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="40b5e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="40b5e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="40b5e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="40b5e-118">Scenario description</span></span>
<span data-ttu-id="40b5e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="40b5e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="40b5e-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="40b5e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="40b5e-121">Přidání Trello z Galerie</span><span class="sxs-lookup"><span data-stu-id="40b5e-121">Adding Trello from the gallery</span></span>
2. <span data-ttu-id="40b5e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="40b5e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-trello-from-the-gallery"></a><span data-ttu-id="40b5e-123">Přidání Trello z Galerie</span><span class="sxs-lookup"><span data-stu-id="40b5e-123">Adding Trello from the gallery</span></span>
<span data-ttu-id="40b5e-124">Při konfiguraci integrace Trello do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Trello z galerie.</span><span class="sxs-lookup"><span data-stu-id="40b5e-124">To configure the integration of Trello into Azure AD, you need to add Trello from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="40b5e-125">**Pokud chcete přidat Trello z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="40b5e-125">**To add Trello from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="40b5e-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="40b5e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="40b5e-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="40b5e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="40b5e-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="40b5e-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="40b5e-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="40b5e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="40b5e-133">Do vyhledávacího pole zadejte **Trello**.</span><span class="sxs-lookup"><span data-stu-id="40b5e-133">In the search box, type **Trello**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-trello-tutorial/tutorial_trello_search.png)

5. <span data-ttu-id="40b5e-135">Na panelu výsledků vyberte **Trello**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="40b5e-135">In the results panel, select **Trello**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-trello-tutorial/tutorial_trello_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="40b5e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="40b5e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="40b5e-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Trello podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="40b5e-138">In this section, you configure and test Azure AD single sign-on with Trello based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="40b5e-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Trello je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="40b5e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Trello is to a user in Azure AD.</span></span> <span data-ttu-id="40b5e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Trello je potřeba vytvořit.</span><span class="sxs-lookup"><span data-stu-id="40b5e-140">In other words, a link relationship between an Azure AD user and the related user in Trello needs to be established.</span></span>

<span data-ttu-id="40b5e-141">V Trello, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="40b5e-141">In Trello, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="40b5e-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Trello, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="40b5e-142">To configure and test Azure AD single sign-on with Trello, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="40b5e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="40b5e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="40b5e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="40b5e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="40b5e-145">**[Vytvoření zkušebního uživatele Trello](#creating-a-trello-test-user)**  – Pokud chcete mít protějšek Britta Simon v Trello propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="40b5e-145">**[Creating a Trello test user](#creating-a-trello-test-user)** - to have a counterpart of Britta Simon in Trello that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="40b5e-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="40b5e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="40b5e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="40b5e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="40b5e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="40b5e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="40b5e-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Trello.</span><span class="sxs-lookup"><span data-stu-id="40b5e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Trello application.</span></span>

<span data-ttu-id="40b5e-150">**Ke konfiguraci Azure AD jednotné přihlašování s Trello, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="40b5e-150">**To configure Azure AD single sign-on with Trello, perform the following steps:**</span></span>

1. <span data-ttu-id="40b5e-151">Na portálu Azure na **Trello** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="40b5e-151">In the Azure portal, on the **Trello** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="40b5e-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="40b5e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_trello_samlbase.png)

3. <span data-ttu-id="40b5e-155">Na **Trello domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP iniciované režimu**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="40b5e-155">On the **Trello Domain and URLs** section, If you wish to configure the application in **IDP initiated mode**, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_trello_url.png)

    <span data-ttu-id="40b5e-157">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://trello.com/auth/saml/consume/<enterprise>`</span><span class="sxs-lookup"><span data-stu-id="40b5e-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://trello.com/auth/saml/consume/<enterprise>`</span></span>

4. <span data-ttu-id="40b5e-158">Na **Trello domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **SP iniciované režimu**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="40b5e-158">On the **Trello Domain and URLs** section, If you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_trello_url1.png)

    <span data-ttu-id="40b5e-160">a.</span><span class="sxs-lookup"><span data-stu-id="40b5e-160">a.</span></span> <span data-ttu-id="40b5e-161">Klikněte na **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="40b5e-161">Click on the **Show advanced URL settings**.</span></span>

    <span data-ttu-id="40b5e-162">b.</span><span class="sxs-lookup"><span data-stu-id="40b5e-162">b.</span></span> <span data-ttu-id="40b5e-163">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://trello.com/auth/saml/consume/<enterprise>`</span><span class="sxs-lookup"><span data-stu-id="40b5e-163">In the **Sign On URL** textbox, type a URL using the following pattern: `https://trello.com/auth/saml/consume/<enterprise>`</span></span>

    >[!NOTE]
    ><span data-ttu-id="40b5e-164">Měli byste obdržet  **\<enterprise\>**  zkráceného názvu stránky z Trello.</span><span class="sxs-lookup"><span data-stu-id="40b5e-164">You should get the **\<enterprise\>** slug from Trello.</span></span> <span data-ttu-id="40b5e-165">Pokud nemáte hodnotu zkráceného názvu stránky, obraťte se na [tým podpory Trello](mailto:support@trello.com) získat zkráceného názvu stránky pro vás enterprise.</span><span class="sxs-lookup"><span data-stu-id="40b5e-165">If you don't have the slug value, contact [Trello support team](mailto:support@trello.com) to get the slug for you enterprise.</span></span>
    > 

5. <span data-ttu-id="40b5e-166">Aplikace Trello očekává kontrolní výrazy SAML tak, aby obsahovala určité atributy.</span><span class="sxs-lookup"><span data-stu-id="40b5e-166">Trello application expects the SAML assertions to contain specific attributes.</span></span> <span data-ttu-id="40b5e-167">Nakonfigurujte následující atributy pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="40b5e-167">Configure the following attributes  for this application.</span></span> <span data-ttu-id="40b5e-168">Můžete spravovat hodnoty těchto atributů z **"Atributy uživatele"** aplikace.</span><span class="sxs-lookup"><span data-stu-id="40b5e-168">You can manage the values of these attributes from the **"User Attributes"** of the application.</span></span> <span data-ttu-id="40b5e-169">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="40b5e-169">The following screenshot shows an example for this.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_trello_attribute.png)

6. <span data-ttu-id="40b5e-171">Na **atributy tokenu SAML** dialogu pro každý řádek v tabulce níže, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="40b5e-171">On the **SAML token attributes** dialog, for each row shown in the table below, perform the following steps:</span></span>
 
    | <span data-ttu-id="40b5e-172">Název atributu</span><span class="sxs-lookup"><span data-stu-id="40b5e-172">Attribute Name</span></span> | <span data-ttu-id="40b5e-173">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="40b5e-173">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="40b5e-174">User.Email</span><span class="sxs-lookup"><span data-stu-id="40b5e-174">User.Email</span></span> | <span data-ttu-id="40b5e-175">User.Mail</span><span class="sxs-lookup"><span data-stu-id="40b5e-175">user.mail</span></span> |
    | <span data-ttu-id="40b5e-176">User.FirstName</span><span class="sxs-lookup"><span data-stu-id="40b5e-176">User.FirstName</span></span> | <span data-ttu-id="40b5e-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="40b5e-177">user.givenname</span></span> |
    | <span data-ttu-id="40b5e-178">User.LastName</span><span class="sxs-lookup"><span data-stu-id="40b5e-178">User.LastName</span></span> | <span data-ttu-id="40b5e-179">User.Surname</span><span class="sxs-lookup"><span data-stu-id="40b5e-179">user.surname</span></span> |

    <span data-ttu-id="40b5e-180">a.</span><span class="sxs-lookup"><span data-stu-id="40b5e-180">a.</span></span> <span data-ttu-id="40b5e-181">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="40b5e-181">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_officespace_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="40b5e-184">b.</span><span class="sxs-lookup"><span data-stu-id="40b5e-184">b.</span></span> <span data-ttu-id="40b5e-185">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="40b5e-185">In the **Name** textbox, type the attribute name shown for that row.</span></span> 

    <span data-ttu-id="40b5e-186">c.</span><span class="sxs-lookup"><span data-stu-id="40b5e-186">c.</span></span> <span data-ttu-id="40b5e-187">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="40b5e-187">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="40b5e-188">d.</span><span class="sxs-lookup"><span data-stu-id="40b5e-188">d.</span></span> <span data-ttu-id="40b5e-189">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="40b5e-189">Click **Ok**.</span></span> 
 
7. <span data-ttu-id="40b5e-190">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="40b5e-190">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_trello_certificate.png) 

8. <span data-ttu-id="40b5e-192">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="40b5e-192">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="40b5e-194">Na **Trello konfigurace** klikněte na tlačítko **konfigurace Trello** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="40b5e-194">On the **Trello Configuration** section, click **Configure Trello** to open **Configure sign-on** window.</span></span> <span data-ttu-id="40b5e-195">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="40b5e-195">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_trello_configure.png) 

9. <span data-ttu-id="40b5e-197">Jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, přejděte do [Konfigurace jednotného přihlašování k enterprise Trello](https://trello.com/sso-configuration) stránku k odeslání [tým podpory Trello](mailto:support@trello.com) **SAML jeden přihlašování adresa URL služby** a připojte **certifikátu (Base64)**.</span><span class="sxs-lookup"><span data-stu-id="40b5e-197">To get SSO configured for your application, go to [Trello enterprise SSO configuration](https://trello.com/sso-configuration) page to send [Trello support team](mailto:support@trello.com) the **SAML Single Sign-On Service URL** and attach the **Certificate (Base64)**.</span></span>

> [!TIP]
> <span data-ttu-id="40b5e-198">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="40b5e-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="40b5e-199">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="40b5e-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="40b5e-200">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="40b5e-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="40b5e-201">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="40b5e-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="40b5e-202">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="40b5e-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="40b5e-204">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="40b5e-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="40b5e-205">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="40b5e-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="40b5e-207">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="40b5e-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="40b5e-209">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="40b5e-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="40b5e-211">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="40b5e-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="40b5e-213">a.</span><span class="sxs-lookup"><span data-stu-id="40b5e-213">a.</span></span> <span data-ttu-id="40b5e-214">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="40b5e-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="40b5e-215">b.</span><span class="sxs-lookup"><span data-stu-id="40b5e-215">b.</span></span> <span data-ttu-id="40b5e-216">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="40b5e-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="40b5e-217">c.</span><span class="sxs-lookup"><span data-stu-id="40b5e-217">c.</span></span> <span data-ttu-id="40b5e-218">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="40b5e-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="40b5e-219">d.</span><span class="sxs-lookup"><span data-stu-id="40b5e-219">d.</span></span> <span data-ttu-id="40b5e-220">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="40b5e-220">Click **Create**.</span></span>
 
### <a name="creating-a-trello-test-user"></a><span data-ttu-id="40b5e-221">Vytvoření zkušebního uživatele Trello</span><span class="sxs-lookup"><span data-stu-id="40b5e-221">Creating a Trello test user</span></span>

<span data-ttu-id="40b5e-222">V této části vytvoříte volal Britta Simon v Trello uživatele.</span><span class="sxs-lookup"><span data-stu-id="40b5e-222">In this section, you create a user called Britta Simon in Trello.</span></span> <span data-ttu-id="40b5e-223">V této části vytvoříte volal Britta Simon v Trello uživatele.</span><span class="sxs-lookup"><span data-stu-id="40b5e-223">In this section, you create a user called Britta Simon in Trello.</span></span> <span data-ttu-id="40b5e-224">Trello podporuje zřizování za běhu a nový účet je vytvořen při prvním přihlášení z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="40b5e-224">Trello supports just-in-time provisioning and a new account is created the first time you sign in from Azure AD.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="40b5e-225">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="40b5e-225">Assigning the Azure AD test user</span></span>

<span data-ttu-id="40b5e-226">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Trello.</span><span class="sxs-lookup"><span data-stu-id="40b5e-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Trello.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="40b5e-228">**Pokud chcete přiřadit Britta Simon Trello, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="40b5e-228">**To assign Britta Simon to Trello, perform the following steps:**</span></span>

1. <span data-ttu-id="40b5e-229">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="40b5e-229">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="40b5e-231">V seznamu aplikací vyberte **Trello**.</span><span class="sxs-lookup"><span data-stu-id="40b5e-231">In the applications list, select **Trello**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_trello_app.png) 

3. <span data-ttu-id="40b5e-233">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="40b5e-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="40b5e-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="40b5e-235">Click **Add** button.</span></span> <span data-ttu-id="40b5e-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="40b5e-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="40b5e-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="40b5e-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="40b5e-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="40b5e-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="40b5e-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="40b5e-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="40b5e-241">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="40b5e-241">Testing single sign-on</span></span>

<span data-ttu-id="40b5e-242">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="40b5e-242">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="40b5e-243">Když kliknete na dlaždici Trello na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Trello.</span><span class="sxs-lookup"><span data-stu-id="40b5e-243">When you click the Trello tile in the Access Panel, you should get automatically signed-on to your Trello application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="40b5e-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="40b5e-244">Additional resources</span></span>

* [<span data-ttu-id="40b5e-245">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="40b5e-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="40b5e-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="40b5e-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-trello-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trello-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trello-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trello-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-trello-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trello-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trello-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-trello-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-trello-tutorial/tutorial_general_203.png

