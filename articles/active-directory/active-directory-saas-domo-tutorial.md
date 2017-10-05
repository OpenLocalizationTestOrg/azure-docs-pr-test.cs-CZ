---
title: 'Kurz: Azure Active Directory integrace s Domo | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Domo."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 058626e4-73b3-4dc2-86ca-b060d002d70a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2017
ms.author: jeedes
ms.openlocfilehash: 919d2262cf9f14159a13370037301005b5b69da2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-domo"></a><span data-ttu-id="837cb-103">Kurz: Azure Active Directory integrace s Domo</span><span class="sxs-lookup"><span data-stu-id="837cb-103">Tutorial: Azure Active Directory integration with Domo</span></span>

<span data-ttu-id="837cb-104">V tomto kurzu zjistěte, jak integrovat Domo s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="837cb-104">In this tutorial, you learn how to integrate Domo with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="837cb-105">Integrace Domo s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="837cb-105">Integrating Domo with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="837cb-106">Můžete řídit ve službě Azure AD, který má přístup k Domo</span><span class="sxs-lookup"><span data-stu-id="837cb-106">You can control in Azure AD who has access to Domo</span></span>
- <span data-ttu-id="837cb-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Domo (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="837cb-107">You can enable your users to automatically get signed-on to Domo (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="837cb-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="837cb-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="837cb-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="837cb-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="837cb-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="837cb-110">Prerequisites</span></span>

<span data-ttu-id="837cb-111">Konfigurace integrace Azure AD s Domo, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="837cb-111">To configure Azure AD integration with Domo, you need the following items:</span></span>

- <span data-ttu-id="837cb-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="837cb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="837cb-113">Domo jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="837cb-113">A Domo single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="837cb-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="837cb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="837cb-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="837cb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="837cb-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="837cb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="837cb-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="837cb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="837cb-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="837cb-118">Scenario description</span></span>
<span data-ttu-id="837cb-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="837cb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="837cb-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="837cb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="837cb-121">Přidání Domo z Galerie</span><span class="sxs-lookup"><span data-stu-id="837cb-121">Adding Domo from the gallery</span></span>
2. <span data-ttu-id="837cb-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="837cb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-domo-from-the-gallery"></a><span data-ttu-id="837cb-123">Přidání Domo z Galerie</span><span class="sxs-lookup"><span data-stu-id="837cb-123">Adding Domo from the gallery</span></span>
<span data-ttu-id="837cb-124">Při konfiguraci integrace Domo do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Domo z galerie.</span><span class="sxs-lookup"><span data-stu-id="837cb-124">To configure the integration of Domo into Azure AD, you need to add Domo from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="837cb-125">**Pokud chcete přidat Domo z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="837cb-125">**To add Domo from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="837cb-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="837cb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="837cb-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="837cb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="837cb-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="837cb-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="837cb-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="837cb-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="837cb-133">Do vyhledávacího pole zadejte **Domo**.</span><span class="sxs-lookup"><span data-stu-id="837cb-133">In the search box, type **Domo**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-domo-tutorial/tutorial_domo_search.png)

5. <span data-ttu-id="837cb-135">Na panelu výsledků vyberte **Domo**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="837cb-135">In the results panel, select **Domo**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-domo-tutorial/tutorial_domo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="837cb-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="837cb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="837cb-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Domo podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="837cb-138">In this section, you configure and test Azure AD single sign-on with Domo based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="837cb-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Domo je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="837cb-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Domo is to a user in Azure AD.</span></span> <span data-ttu-id="837cb-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Domo musí navázat.</span><span class="sxs-lookup"><span data-stu-id="837cb-140">In other words, a link relationship between an Azure AD user and the related user in Domo needs to be established.</span></span>

<span data-ttu-id="837cb-141">V Domo, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="837cb-141">In Domo, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="837cb-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Domo, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="837cb-142">To configure and test Azure AD single sign-on with Domo, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="837cb-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="837cb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="837cb-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="837cb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="837cb-145">**[Vytvoření zkušebního uživatele Domo](#creating-a-domo-test-user)**  – Pokud chcete mít protějšek Britta Simon v Domo propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="837cb-145">**[Creating a Domo test user](#creating-a-domo-test-user)** - to have a counterpart of Britta Simon in Domo that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="837cb-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="837cb-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="837cb-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="837cb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="837cb-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="837cb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="837cb-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Domo.</span><span class="sxs-lookup"><span data-stu-id="837cb-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Domo application.</span></span>

<span data-ttu-id="837cb-150">**Ke konfiguraci Azure AD jednotné přihlašování s Domo, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="837cb-150">**To configure Azure AD single sign-on with Domo, perform the following steps:**</span></span>

1. <span data-ttu-id="837cb-151">Na portálu Azure na **Domo** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="837cb-151">In the Azure portal, on the **Domo** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="837cb-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="837cb-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_domo_samlbase.png)

3. <span data-ttu-id="837cb-155">Na **Domo domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="837cb-155">On the **Domo Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_domo_url.png)

    <span data-ttu-id="837cb-157">a.</span><span class="sxs-lookup"><span data-stu-id="837cb-157">a.</span></span> <span data-ttu-id="837cb-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.domo.com`</span><span class="sxs-lookup"><span data-stu-id="837cb-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.domo.com`</span></span>

    <span data-ttu-id="837cb-159">b.</span><span class="sxs-lookup"><span data-stu-id="837cb-159">b.</span></span> <span data-ttu-id="837cb-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="837cb-160">In the **Identifier** textbox, type a URL using the following patterns:</span></span>     

    | |
    |--|    
    | `https://<companyname>.domo.com` |
    | `https://<companyname>.beta.domo.com` |
    | `https://<companyname>.demo.domo.com` |
    | `https://<companyname>.dev.domo.com` | 
    | `https://<companyname>.fastage1.domo.com` |       
    | `https://<companyname>.frdev.domo.com` |       
    | `https://<companyname>.gastage.domo.com` |       
    | `https://<companyname>.load.domo.com` |       
    | `https://<companyname>.local.domo.com` |       
    | `https://<companyname>.qa.domo.com` |
    | `https://<companyname>.stage.domo.com` |
    
    > [!NOTE] 
    > <span data-ttu-id="837cb-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="837cb-161">These values are not real.</span></span> <span data-ttu-id="837cb-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="837cb-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="837cb-163">Obraťte se na [tým podpory Domo klienta](mailto:support@domo.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="837cb-163">Contact [Domo Client support team](mailto:support@domo.com) to get these values.</span></span>

4. <span data-ttu-id="837cb-164">Aplikace Domo očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="837cb-164">Domo application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="837cb-165">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="837cb-165">Configure the following claims for this application.</span></span> <span data-ttu-id="837cb-166">Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="837cb-166">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="837cb-167">Následující snímek obrazovky ukazuje příklad pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="837cb-167">The following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_domo_attributes.png)
    
5. <span data-ttu-id="837cb-169">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="837cb-169">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="837cb-170">Název atributu</span><span class="sxs-lookup"><span data-stu-id="837cb-170">Attribute Name</span></span> | <span data-ttu-id="837cb-171">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="837cb-171">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="837cb-172">jméno</span><span class="sxs-lookup"><span data-stu-id="837cb-172">name</span></span> | <span data-ttu-id="837cb-173">User.DisplayName</span><span class="sxs-lookup"><span data-stu-id="837cb-173">user.displayname</span></span> |
    | <span data-ttu-id="837cb-174">E-mailu</span><span class="sxs-lookup"><span data-stu-id="837cb-174">email</span></span> | <span data-ttu-id="837cb-175">User.Mail</span><span class="sxs-lookup"><span data-stu-id="837cb-175">user.mail</span></span> |
    
    <span data-ttu-id="837cb-176">a.</span><span class="sxs-lookup"><span data-stu-id="837cb-176">a.</span></span> <span data-ttu-id="837cb-177">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="837cb-177">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="837cb-180">b.</span><span class="sxs-lookup"><span data-stu-id="837cb-180">b.</span></span> <span data-ttu-id="837cb-181">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="837cb-181">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="837cb-182">c.</span><span class="sxs-lookup"><span data-stu-id="837cb-182">c.</span></span> <span data-ttu-id="837cb-183">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="837cb-183">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="837cb-184">d.</span><span class="sxs-lookup"><span data-stu-id="837cb-184">d.</span></span> <span data-ttu-id="837cb-185">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="837cb-185">Click **Ok**.</span></span> 
 
6. <span data-ttu-id="837cb-186">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="837cb-186">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_domo_certificate.png) 

7. <span data-ttu-id="837cb-188">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="837cb-188">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_general_400.png)


8. <span data-ttu-id="837cb-190">Na **Domo konfigurace** klikněte na tlačítko **konfigurace Domo** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="837cb-190">On the **Domo Configuration** section, click **Configure Domo** to open **Configure sign-on** window.</span></span> <span data-ttu-id="837cb-191">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="837cb-191">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>   

   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_domo_configure.png) 

9. <span data-ttu-id="837cb-193">Konfigurace jednotného přihlašování na **Domo** straně, budete muset odeslat stažené **certifikát**, **SAML Entity ID**, **SAML jeden přihlašování adresa URL služby** a **Sign-Out URL** k [tým podpory Domo](mailto:support@domo.com).</span><span class="sxs-lookup"><span data-stu-id="837cb-193">To configure single sign-on on **Domo** side, you need to send the downloaded **Certificate**, **SAML Entity ID**, the **SAML Single Sign-On Service URL** and the **Sign-Out URL** to [Domo support team](mailto:support@domo.com).</span></span> <span data-ttu-id="837cb-194">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="837cb-194">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="837cb-195">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="837cb-195">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="837cb-196">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="837cb-196">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="837cb-197">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="837cb-197">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="837cb-198">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="837cb-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="837cb-199">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="837cb-199">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="837cb-201">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="837cb-201">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="837cb-202">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="837cb-202">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="837cb-204">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="837cb-204">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="837cb-206">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="837cb-206">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="837cb-208">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="837cb-208">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="837cb-210">a.</span><span class="sxs-lookup"><span data-stu-id="837cb-210">a.</span></span> <span data-ttu-id="837cb-211">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="837cb-211">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="837cb-212">b.</span><span class="sxs-lookup"><span data-stu-id="837cb-212">b.</span></span> <span data-ttu-id="837cb-213">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="837cb-213">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="837cb-214">c.</span><span class="sxs-lookup"><span data-stu-id="837cb-214">c.</span></span> <span data-ttu-id="837cb-215">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="837cb-215">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="837cb-216">d.</span><span class="sxs-lookup"><span data-stu-id="837cb-216">d.</span></span> <span data-ttu-id="837cb-217">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="837cb-217">Click **Create**.</span></span>
 
### <a name="creating-a-domo-test-user"></a><span data-ttu-id="837cb-218">Vytvoření zkušebního uživatele Domo</span><span class="sxs-lookup"><span data-stu-id="837cb-218">Creating a Domo test user</span></span>

<span data-ttu-id="837cb-219">Cílem této části je vytvoření uživatele v Domo nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="837cb-219">The objective of this section is to create a user called Britta Simon in Domo.</span></span> <span data-ttu-id="837cb-220">Domo podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="837cb-220">Domo supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="837cb-221">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="837cb-221">There is no action item for you in this section.</span></span> <span data-ttu-id="837cb-222">Nový uživatel se vytvoří během pokusu o přístup k Domo, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="837cb-222">A new user is created during an attempt to access Domo if it doesn't exist yet.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="837cb-223">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="837cb-223">Assigning the Azure AD test user</span></span>

<span data-ttu-id="837cb-224">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Domo.</span><span class="sxs-lookup"><span data-stu-id="837cb-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Domo.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="837cb-226">**Pokud chcete přiřadit Britta Simon Domo, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="837cb-226">**To assign Britta Simon to Domo, perform the following steps:**</span></span>

1. <span data-ttu-id="837cb-227">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="837cb-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="837cb-229">V seznamu aplikací vyberte **Domo**.</span><span class="sxs-lookup"><span data-stu-id="837cb-229">In the applications list, select **Domo**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_domo_app.png) 

3. <span data-ttu-id="837cb-231">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="837cb-231">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="837cb-233">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="837cb-233">Click **Add** button.</span></span> <span data-ttu-id="837cb-234">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="837cb-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="837cb-236">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="837cb-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="837cb-237">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="837cb-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="837cb-238">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="837cb-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="837cb-239">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="837cb-239">Testing single sign-on</span></span>

<span data-ttu-id="837cb-240">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="837cb-240">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
<span data-ttu-id="837cb-241">Když kliknete na dlaždici Domo na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Domo.</span><span class="sxs-lookup"><span data-stu-id="837cb-241">When you click the Domo tile in the Access Panel, you should get automatically signed-on to your Domo application.</span></span>

<span data-ttu-id="837cb-242">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="837cb-242">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="837cb-243">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="837cb-243">Additional resources</span></span>

* [<span data-ttu-id="837cb-244">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="837cb-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="837cb-245">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="837cb-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-domo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-domo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-domo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-domo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-domo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-domo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-domo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-domo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-domo-tutorial/tutorial_general_203.png

