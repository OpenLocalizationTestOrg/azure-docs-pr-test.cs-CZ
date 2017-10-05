---
title: 'Kurz: Azure Active Directory integrace s Workpath | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Workpath."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 320b0daf-14be-4813-b59b-25a6a5070690
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: f4efa56d2c0374a977c1e46dad64b596cc9c3ea8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workpath"></a><span data-ttu-id="5395f-103">Kurz: Azure Active Directory integrace s Workpath</span><span class="sxs-lookup"><span data-stu-id="5395f-103">Tutorial: Azure Active Directory integration with Workpath</span></span>

<span data-ttu-id="5395f-104">V tomto kurzu zjistěte, jak integrovat Workpath s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5395f-104">In this tutorial, you learn how to integrate Workpath with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5395f-105">Integrace Workpath s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5395f-105">Integrating Workpath with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5395f-106">Můžete řídit ve službě Azure AD, který má přístup k Workpath</span><span class="sxs-lookup"><span data-stu-id="5395f-106">You can control in Azure AD who has access to Workpath</span></span>
- <span data-ttu-id="5395f-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Workpath (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5395f-107">You can enable your users to automatically get signed-on to Workpath (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5395f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5395f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5395f-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5395f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5395f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5395f-110">Prerequisites</span></span>

<span data-ttu-id="5395f-111">Konfigurace integrace Azure AD s Workpath, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="5395f-111">To configure Azure AD integration with Workpath, you need the following items:</span></span>

- <span data-ttu-id="5395f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5395f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5395f-113">Workpath jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5395f-113">A Workpath single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5395f-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5395f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5395f-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5395f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5395f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5395f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5395f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5395f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5395f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5395f-118">Scenario description</span></span>
<span data-ttu-id="5395f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5395f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5395f-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5395f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5395f-121">Přidání Workpath z Galerie</span><span class="sxs-lookup"><span data-stu-id="5395f-121">Adding Workpath from the gallery</span></span>
2. <span data-ttu-id="5395f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5395f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workpath-from-the-gallery"></a><span data-ttu-id="5395f-123">Přidání Workpath z Galerie</span><span class="sxs-lookup"><span data-stu-id="5395f-123">Adding Workpath from the gallery</span></span>
<span data-ttu-id="5395f-124">Při konfiguraci integrace Workpath do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Workpath z galerie.</span><span class="sxs-lookup"><span data-stu-id="5395f-124">To configure the integration of Workpath into Azure AD, you need to add Workpath from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5395f-125">**Pokud chcete přidat Workpath z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5395f-125">**To add Workpath from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5395f-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5395f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5395f-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5395f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5395f-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5395f-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5395f-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5395f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5395f-133">Do vyhledávacího pole zadejte **Workpath**.</span><span class="sxs-lookup"><span data-stu-id="5395f-133">In the search box, type **Workpath**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_search.png)

5. <span data-ttu-id="5395f-135">Na panelu výsledků vyberte **Workpath**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5395f-135">In the results panel, select **Workpath**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5395f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5395f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5395f-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workpath podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="5395f-138">In this section, you configure and test Azure AD single sign-on with Workpath based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5395f-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Workpath je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5395f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Workpath is to a user in Azure AD.</span></span> <span data-ttu-id="5395f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Workpath musí navázat.</span><span class="sxs-lookup"><span data-stu-id="5395f-140">In other words, a link relationship between an Azure AD user and the related user in Workpath needs to be established.</span></span>

<span data-ttu-id="5395f-141">V Workpath, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="5395f-141">In Workpath, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5395f-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workpath, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="5395f-142">To configure and test Azure AD single sign-on with Workpath, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5395f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="5395f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5395f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5395f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5395f-145">**[Vytvoření zkušebního uživatele Workpath](#creating-a-workpath-test-user)**  – Pokud chcete mít protějšek Britta Simon v Workpath propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="5395f-145">**[Creating a Workpath test user](#creating-a-workpath-test-user)** - to have a counterpart of Britta Simon in Workpath that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5395f-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5395f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5395f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5395f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5395f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5395f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5395f-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Workpath.</span><span class="sxs-lookup"><span data-stu-id="5395f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Workpath application.</span></span>

<span data-ttu-id="5395f-150">**Ke konfiguraci Azure AD jednotné přihlašování s Workpath, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5395f-150">**To configure Azure AD single sign-on with Workpath, perform the following steps:**</span></span>

1. <span data-ttu-id="5395f-151">Na portálu Azure na **Workpath** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5395f-151">In the Azure portal, on the **Workpath** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5395f-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5395f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_samlbase.png)

3. <span data-ttu-id="5395f-155">Na **Workpath domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP** initiated režimu proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5395f-155">On the **Workpath Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_url.png)

    <span data-ttu-id="5395f-157">a.</span><span class="sxs-lookup"><span data-stu-id="5395f-157">a.</span></span> <span data-ttu-id="5395f-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://api.workpath.com/v1/saml/metadata/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="5395f-158">In the **Identifier** textbox, type a URL using the following pattern: `https://api.workpath.com/v1/saml/metadata/<instancename>`</span></span>

    <span data-ttu-id="5395f-159">b.</span><span class="sxs-lookup"><span data-stu-id="5395f-159">b.</span></span> <span data-ttu-id="5395f-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://api.workpath.com/v1/saml/assert/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="5395f-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://api.workpath.com/v1/saml/assert/<instancename>`</span></span>

4. <span data-ttu-id="5395f-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="5395f-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="5395f-162">Pokud chcete nakonfigurovat aplikace **SP** iniciované režimu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5395f-162">If you wish to configure the application in **SP** initiated mode, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_url1.png)

    <span data-ttu-id="5395f-164">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.workpath.com/ `</span><span class="sxs-lookup"><span data-stu-id="5395f-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.workpath.com/ `</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5395f-165">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="5395f-165">These values are not real.</span></span> <span data-ttu-id="5395f-166">Tyto hodnoty aktualizujte skutečná adresa URL přihlašování, identifikátor a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="5395f-166">Update these values with the actual Sign-on URL, Identifier and Reply URL.</span></span> <span data-ttu-id="5395f-167">Obraťte se na [tým podpory Workpath](https://help.workpath.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5395f-167">Contact [Workpath support team](https://help.workpath.com) to get these values.</span></span>

5. <span data-ttu-id="5395f-168">Aplikace Workpath očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="5395f-168">Workpath application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="5395f-169">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5395f-169">Configure the following claims for this application.</span></span> <span data-ttu-id="5395f-170">Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="5395f-170">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="5395f-171">Následující snímek obrazovky ukazuje příklad pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="5395f-171">The following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_attributes.png)
    
6. <span data-ttu-id="5395f-173">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5395f-173">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="5395f-174">Název atributu</span><span class="sxs-lookup"><span data-stu-id="5395f-174">Attribute Name</span></span> | <span data-ttu-id="5395f-175">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="5395f-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="5395f-176">křestní_jméno</span><span class="sxs-lookup"><span data-stu-id="5395f-176">first_name</span></span> | <span data-ttu-id="5395f-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="5395f-177">user.givenname</span></span> |
    | <span data-ttu-id="5395f-178">Příjmení</span><span class="sxs-lookup"><span data-stu-id="5395f-178">last_name</span></span> | <span data-ttu-id="5395f-179">User.Surname</span><span class="sxs-lookup"><span data-stu-id="5395f-179">user.surname</span></span> |
    
    <span data-ttu-id="5395f-180">a.</span><span class="sxs-lookup"><span data-stu-id="5395f-180">a.</span></span> <span data-ttu-id="5395f-181">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5395f-181">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="5395f-183">b.</span><span class="sxs-lookup"><span data-stu-id="5395f-183">b.</span></span> <span data-ttu-id="5395f-184">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="5395f-184">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="5395f-186">c.</span><span class="sxs-lookup"><span data-stu-id="5395f-186">c.</span></span> <span data-ttu-id="5395f-187">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="5395f-187">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="5395f-188">d.</span><span class="sxs-lookup"><span data-stu-id="5395f-188">d.</span></span> <span data-ttu-id="5395f-189">Ponechte **Namespace** textové pole prázdné.</span><span class="sxs-lookup"><span data-stu-id="5395f-189">Leave the **Namespace** textbox blank.</span></span>
    
    <span data-ttu-id="5395f-190">e.</span><span class="sxs-lookup"><span data-stu-id="5395f-190">e.</span></span> <span data-ttu-id="5395f-191">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="5395f-191">Click **Ok**.</span></span>
    

7. <span data-ttu-id="5395f-192">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5395f-192">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_certificate.png) 

8. <span data-ttu-id="5395f-194">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5395f-194">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="5395f-196">Na **Workpath konfigurace** klikněte na tlačítko **konfigurace Workpath** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5395f-196">On the **Workpath Configuration** section, click **Configure Workpath** to open **Configure sign-on** window.</span></span> <span data-ttu-id="5395f-197">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5395f-197">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_configure.png) 

10. <span data-ttu-id="5395f-199">Konfigurace jednotného přihlašování na **Workpath** straně, budete muset odeslat stažené **soubor XML s metadaty**, **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [tým podpory Workpath](https://help.workpath.com).</span><span class="sxs-lookup"><span data-stu-id="5395f-199">To configure single sign-on on **Workpath** side, you need to send the downloaded **Metadata XML**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Workpath support team](https://help.workpath.com).</span></span> 

> [!TIP]
> <span data-ttu-id="5395f-200">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="5395f-200">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5395f-201">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="5395f-201">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5395f-202">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5395f-202">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5395f-203">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5395f-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="5395f-204">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5395f-204">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5395f-206">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5395f-206">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5395f-207">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5395f-207">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5395f-209">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5395f-209">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5395f-211">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5395f-211">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5395f-213">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5395f-213">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5395f-215">a.</span><span class="sxs-lookup"><span data-stu-id="5395f-215">a.</span></span> <span data-ttu-id="5395f-216">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5395f-216">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5395f-217">b.</span><span class="sxs-lookup"><span data-stu-id="5395f-217">b.</span></span> <span data-ttu-id="5395f-218">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5395f-218">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5395f-219">c.</span><span class="sxs-lookup"><span data-stu-id="5395f-219">c.</span></span> <span data-ttu-id="5395f-220">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5395f-220">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5395f-221">d.</span><span class="sxs-lookup"><span data-stu-id="5395f-221">d.</span></span> <span data-ttu-id="5395f-222">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5395f-222">Click **Create**.</span></span>
 
### <a name="creating-a-workpath-test-user"></a><span data-ttu-id="5395f-223">Vytvoření zkušebního uživatele Workpath</span><span class="sxs-lookup"><span data-stu-id="5395f-223">Creating a Workpath test user</span></span>

<span data-ttu-id="5395f-224">Workpath podporuje pouze v době zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5395f-224">Workpath supports Just in time user provisioning.</span></span> <span data-ttu-id="5395f-225">Po ověření uživatelé jsou automaticky vytvořené v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5395f-225">After authentication users are created in the application automatically.</span></span> 


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5395f-226">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5395f-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5395f-227">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Workpath.</span><span class="sxs-lookup"><span data-stu-id="5395f-227">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Workpath.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5395f-229">**Pokud chcete přiřadit Britta Simon Workpath, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5395f-229">**To assign Britta Simon to Workpath, perform the following steps:**</span></span>

1. <span data-ttu-id="5395f-230">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5395f-230">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5395f-232">V seznamu aplikací vyberte **Workpath**.</span><span class="sxs-lookup"><span data-stu-id="5395f-232">In the applications list, select **Workpath**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_app.png) 

3. <span data-ttu-id="5395f-234">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5395f-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5395f-236">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5395f-236">Click **Add** button.</span></span> <span data-ttu-id="5395f-237">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5395f-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5395f-239">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5395f-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5395f-240">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5395f-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5395f-241">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5395f-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5395f-242">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5395f-242">Testing single sign-on</span></span>

<span data-ttu-id="5395f-243">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5395f-243">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="5395f-244">Když kliknete na dlaždici Workpath na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Workpath.</span><span class="sxs-lookup"><span data-stu-id="5395f-244">When you click the Workpath tile in the Access Panel, you should get automatically signed-on to your Workpath application.</span></span>
<span data-ttu-id="5395f-245">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5395f-245">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5395f-246">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5395f-246">Additional resources</span></span>

* [<span data-ttu-id="5395f-247">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5395f-247">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5395f-248">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5395f-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_203.png

