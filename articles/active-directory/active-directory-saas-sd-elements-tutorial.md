---
title: 'Kurz: Azure Active Directory integrace s SD elementy | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SD elementy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 624eff0a0da8f548877e4a4346b21df89cd37b67
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a><span data-ttu-id="5e3ed-103">Kurz: Azure Active Directory integrace s SD elementy</span><span class="sxs-lookup"><span data-stu-id="5e3ed-103">Tutorial: Azure Active Directory integration with SD Elements</span></span>

<span data-ttu-id="5e3ed-104">V tomto kurzu zjistěte, jak integrovat SD elementů s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5e3ed-104">In this tutorial, you learn how to integrate SD Elements with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5e3ed-105">Integrace s Azure AD SD elementy poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5e3ed-105">Integrating SD Elements with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5e3ed-106">Můžete řídit ve službě Azure AD, který má přístup k elementům SD</span><span class="sxs-lookup"><span data-stu-id="5e3ed-106">You can control in Azure AD who has access to SD Elements</span></span>
- <span data-ttu-id="5e3ed-107">Můžete povolit uživatelům, aby automaticky získat přihlášeného k elementům SD (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5e3ed-107">You can enable your users to automatically get signed-on to SD Elements (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5e3ed-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5e3ed-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5e3ed-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5e3ed-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e3ed-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5e3ed-110">Prerequisites</span></span>

<span data-ttu-id="5e3ed-111">Ke konfiguraci integrace služby Azure AD SD elementy, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="5e3ed-111">To configure Azure AD integration with SD Elements, you need the following items:</span></span>

- <span data-ttu-id="5e3ed-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5e3ed-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5e3ed-113">Elementy SD jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5e3ed-113">A SD Elements single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5e3ed-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5e3ed-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5e3ed-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5e3ed-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5e3ed-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5e3ed-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5e3ed-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5e3ed-118">Scenario description</span></span>
<span data-ttu-id="5e3ed-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5e3ed-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5e3ed-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5e3ed-121">Přidávání elementů SD z Galerie</span><span class="sxs-lookup"><span data-stu-id="5e3ed-121">Adding SD Elements from the gallery</span></span>
2. <span data-ttu-id="5e3ed-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5e3ed-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sd-elements-from-the-gallery"></a><span data-ttu-id="5e3ed-123">Přidávání elementů SD z Galerie</span><span class="sxs-lookup"><span data-stu-id="5e3ed-123">Adding SD Elements from the gallery</span></span>
<span data-ttu-id="5e3ed-124">Při konfiguraci integrace SD elementů do služby Azure AD, potřebujete přidat SD elementy z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-124">To configure the integration of SD Elements into Azure AD, you need to add SD Elements from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5e3ed-125">**Pokud chcete přidat SD elementy z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5e3ed-125">**To add SD Elements from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5e3ed-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5e3ed-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5e3ed-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5e3ed-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5e3ed-133">Do vyhledávacího pole zadejte **SD elementy**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-133">In the search box, type **SD Elements**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_search.png)

5. <span data-ttu-id="5e3ed-135">Na panelu výsledků vyberte **SD elementy**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-135">In the results panel, select **SD Elements**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5e3ed-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5e3ed-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5e3ed-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s SD elementy podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="5e3ed-138">In this section, you configure and test Azure AD single sign-on with SD Elements based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5e3ed-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v elementech SD je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SD Elements is to a user in Azure AD.</span></span> <span data-ttu-id="5e3ed-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v elementech SD musí navázat.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-140">In other words, a link relationship between an Azure AD user and the related user in SD Elements needs to be established.</span></span>

<span data-ttu-id="5e3ed-141">V elementech SD přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-141">In SD Elements, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5e3ed-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SD elementy, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="5e3ed-142">To configure and test Azure AD single sign-on with SD Elements, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5e3ed-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5e3ed-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5e3ed-145">**[Vytvoření zkušebního uživatele SD elementy](#creating-a-sd-elements-test-user)**  – Pokud chcete mít protějšek Britta Simon v elementech SD, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-145">**[Creating a SD Elements test user](#creating-a-sd-elements-test-user)** - to have a counterpart of Britta Simon in SD Elements that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5e3ed-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5e3ed-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5e3ed-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5e3ed-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5e3ed-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci SD elementy.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SD Elements application.</span></span>

<span data-ttu-id="5e3ed-150">**Ke konfiguraci Azure AD jednotné přihlašování s prvky SD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5e3ed-150">**To configure Azure AD single sign-on with SD Elements, perform the following steps:**</span></span>

1. <span data-ttu-id="5e3ed-151">Na portálu Azure na **SD elementy** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-151">In the Azure portal, on the **SD Elements** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5e3ed-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_samlbase.png)

3. <span data-ttu-id="5e3ed-155">Na **SD elementy domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5e3ed-155">On the **SD Elements Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_url.png)

    <span data-ttu-id="5e3ed-157">a.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-157">a.</span></span> <span data-ttu-id="5e3ed-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenantname>.sdelements.com/sso/saml2/metadata`</span><span class="sxs-lookup"><span data-stu-id="5e3ed-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenantname>.sdelements.com/sso/saml2/metadata`</span></span>

    <span data-ttu-id="5e3ed-159">b.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-159">b.</span></span> <span data-ttu-id="5e3ed-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenantname>.sdelements.com/sso/saml2/acs/`</span><span class="sxs-lookup"><span data-stu-id="5e3ed-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<tenantname>.sdelements.com/sso/saml2/acs/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5e3ed-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-161">These values are not real.</span></span> <span data-ttu-id="5e3ed-162">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="5e3ed-163">Obraťte se na [tým podpory SD elementy](mailto:support@sdelements.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-163">Contact [SD Elements support team](mailto:support@sdelements.com) to get these values.</span></span>

4. <span data-ttu-id="5e3ed-164">Elementy SD aplikace očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-164">SD Elements application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="5e3ed-165">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-165">Configure the following claims for this application.</span></span> <span data-ttu-id="5e3ed-166">Můžete spravovat hodnoty těchto atributů z **"Atribut uživatele"** aplikace.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-166">You can manage the values of these attributes from the **"User Attribute"** tab of the application.</span></span> <span data-ttu-id="5e3ed-167">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-167">The following screenshot shows an example for this.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_attribute.png)

5. <span data-ttu-id="5e3ed-169">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5e3ed-169">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span> 

    | <span data-ttu-id="5e3ed-170">Název atributu</span><span class="sxs-lookup"><span data-stu-id="5e3ed-170">Attribute Name</span></span> | <span data-ttu-id="5e3ed-171">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="5e3ed-171">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="5e3ed-172">E-mailu</span><span class="sxs-lookup"><span data-stu-id="5e3ed-172">email</span></span> |<span data-ttu-id="5e3ed-173">User.Mail</span><span class="sxs-lookup"><span data-stu-id="5e3ed-173">user.mail</span></span> |
    | <span data-ttu-id="5e3ed-174">FirstName</span><span class="sxs-lookup"><span data-stu-id="5e3ed-174">firstname</span></span> |<span data-ttu-id="5e3ed-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="5e3ed-175">user.givenname</span></span> |
    | <span data-ttu-id="5e3ed-176">Příjmení</span><span class="sxs-lookup"><span data-stu-id="5e3ed-176">lastname</span></span> |<span data-ttu-id="5e3ed-177">User.Surname</span><span class="sxs-lookup"><span data-stu-id="5e3ed-177">user.surname</span></span> |

    <span data-ttu-id="5e3ed-178">a.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-178">a.</span></span> <span data-ttu-id="5e3ed-179">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-179">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="5e3ed-182">b.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-182">b.</span></span> <span data-ttu-id="5e3ed-183">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-183">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="5e3ed-184">c.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-184">c.</span></span> <span data-ttu-id="5e3ed-185">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-185">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="5e3ed-186">d.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-186">d.</span></span> <span data-ttu-id="5e3ed-187">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-187">Click **Ok**.</span></span>
 
6. <span data-ttu-id="5e3ed-188">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-188">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_certificate.png) 

7. <span data-ttu-id="5e3ed-190">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-190">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="5e3ed-192">Na **SD prvky konfigurace** klikněte na tlačítko **konfigurace elementy SD** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-192">On the **SD Elements Configuration** section, click **Configure SD Elements** to open **Configure sign-on** window.</span></span> <span data-ttu-id="5e3ed-193">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5e3ed-193">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_configure.png)

9. <span data-ttu-id="5e3ed-195">Pokud chcete získat jednotné přihlašování povolené, obraťte se na vaše [tým podpory SD elementy](mailto:support@sdelements.com) a poskytnout soubor stažený certifikát.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-195">To get single sign-on enabled, contact your [SD Elements support team](mailto:support@sdelements.com) and provide them with the downloaded certificate file.</span></span> 

10. <span data-ttu-id="5e3ed-196">V okně jiný prohlížeč přihlášení jako správce klienta SD elementy.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-196">In a different browser window, sign-on to your SD Elements tenant as an administrator.</span></span>

11. <span data-ttu-id="5e3ed-197">V nabídce v horní části, klikněte na tlačítko **systému**a potom **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-197">In the menu on the top, click **System**, and then **Single Sign-on**.</span></span> 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_09.png) 

12. <span data-ttu-id="5e3ed-199">Na **nastavení jednotného přihlašování** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5e3ed-199">On the **Single Sign-On Settings** dialog, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    <span data-ttu-id="5e3ed-201">a.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-201">a.</span></span> <span data-ttu-id="5e3ed-202">Jako **typ jednotného přihlašování k**, vyberte **SAML**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-202">As **SSO Type**, select **SAML**.</span></span>
   
    <span data-ttu-id="5e3ed-203">b.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-203">b.</span></span> <span data-ttu-id="5e3ed-204">V **ID Entity zprostředkovatele Identity** textovému poli, vložte hodnotu **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-204">In the **Identity Provider Entity ID** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 
   
    <span data-ttu-id="5e3ed-205">c.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-205">c.</span></span> <span data-ttu-id="5e3ed-206">V **Identity zprostředkovatele-služby přihlášení** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-206">In the **Identity Provider Single Sign-On Service** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span> 
   
    <span data-ttu-id="5e3ed-207">d.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-207">d.</span></span> <span data-ttu-id="5e3ed-208">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-208">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="5e3ed-209">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="5e3ed-209">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5e3ed-210">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-210">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5e3ed-211">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5e3ed-211">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5e3ed-212">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5e3ed-212">Creating an Azure AD test user</span></span>
<span data-ttu-id="5e3ed-213">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-213">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5e3ed-215">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5e3ed-215">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5e3ed-216">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-216">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5e3ed-218">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-218">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5e3ed-220">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-220">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5e3ed-222">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5e3ed-222">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5e3ed-224">a.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-224">a.</span></span> <span data-ttu-id="5e3ed-225">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-225">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5e3ed-226">b.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-226">b.</span></span> <span data-ttu-id="5e3ed-227">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-227">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5e3ed-228">c.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-228">c.</span></span> <span data-ttu-id="5e3ed-229">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-229">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5e3ed-230">d.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-230">d.</span></span> <span data-ttu-id="5e3ed-231">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-231">Click **Create**.</span></span>
 
### <a name="creating-a-sd-elements-test-user"></a><span data-ttu-id="5e3ed-232">Vytvoření zkušebního uživatele SD elementy</span><span class="sxs-lookup"><span data-stu-id="5e3ed-232">Creating a SD Elements test user</span></span>

<span data-ttu-id="5e3ed-233">Cílem této části je vytvoření uživatele volal Britta Simon v SD elementy.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-233">The objective of this section is to create a user called Britta Simon in SD Elements.</span></span> <span data-ttu-id="5e3ed-234">V případě elementů SD vytváření SD elementy uživatelů je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-234">In the case of SD Elements, creating SD Elements users is a manual task.</span></span>

<span data-ttu-id="5e3ed-235">**Pokud chcete vytvořit Britta Simon v SD elementy, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5e3ed-235">**To create Britta Simon in SD Elements, perform the following steps:**</span></span>

1. <span data-ttu-id="5e3ed-236">V okně webového prohlížeče přihlašování k webu společnosti SD elementy jako správce.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-236">In a web browser window, sign-on to your SD Elements company site as an administrator.</span></span>

2. <span data-ttu-id="5e3ed-237">V nabídce v horní části, klikněte na tlačítko **Správa uživatelů**a potom **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-237">In the menu on the top, click **User Management**, and then **Users**.</span></span>
   
    ![Vytvoření zkušebního uživatele SD elementy](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_11.png) 

3. <span data-ttu-id="5e3ed-239">Klikněte na tlačítko **přidat nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-239">Click **Add New User**.</span></span>
   
    ![Vytvoření zkušebního uživatele SD elementy](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_12.png)
 
4. <span data-ttu-id="5e3ed-241">Na **přidat nové uživatele** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5e3ed-241">On the **Add New User** dialog, perform the following steps:</span></span>
   
    ![Vytvoření zkušebního uživatele SD elementy](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
    <span data-ttu-id="5e3ed-243">a.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-243">a.</span></span> <span data-ttu-id="5e3ed-244">V **e-mailu** textovému poli, zadejte e-mailu uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="5e3ed-244">In the **E-mail** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="5e3ed-245">b.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-245">b.</span></span> <span data-ttu-id="5e3ed-246">V **křestní jméno** textovému poli, zadejte jméno uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-246">In the **First Name** textbox, enter the first name of user like **Britta**.</span></span>
   
    <span data-ttu-id="5e3ed-247">c.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-247">c.</span></span> <span data-ttu-id="5e3ed-248">V **příjmení** textovému poli, zadejte příjmení uživatele jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-248">In the **Last Name** textbox, enter the last name of user like **Simon**.</span></span>
   
    <span data-ttu-id="5e3ed-249">d.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-249">d.</span></span> <span data-ttu-id="5e3ed-250">Jako **Role**, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-250">As **Role**, select **User**.</span></span> 
   
    <span data-ttu-id="5e3ed-251">e.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-251">e.</span></span> <span data-ttu-id="5e3ed-252">Klikněte na tlačítko **vytvořit uživatele**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-252">Click **Create User**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5e3ed-253">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5e3ed-253">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5e3ed-254">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu SD elementy.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-254">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SD Elements.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5e3ed-256">**Pokud chcete přiřadit Britta Simon SD elementy, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5e3ed-256">**To assign Britta Simon to SD Elements, perform the following steps:**</span></span>

1. <span data-ttu-id="5e3ed-257">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-257">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5e3ed-259">V seznamu aplikací vyberte **SD elementy**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-259">In the applications list, select **SD Elements**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_app.png) 

3. <span data-ttu-id="5e3ed-261">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-261">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5e3ed-263">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-263">Click **Add** button.</span></span> <span data-ttu-id="5e3ed-264">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-264">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5e3ed-266">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-266">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5e3ed-267">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-267">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5e3ed-268">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-268">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5e3ed-269">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5e3ed-269">Testing single sign-on</span></span>

<span data-ttu-id="5e3ed-270">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-270">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>
  
<span data-ttu-id="5e3ed-271">Když kliknete na dlaždici SD elementy na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci SD elementy.</span><span class="sxs-lookup"><span data-stu-id="5e3ed-271">When you click the SD Elements tile in the Access Panel, you should get automatically signed-on to your SD Elements application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5e3ed-272">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5e3ed-272">Additional resources</span></span>

* [<span data-ttu-id="5e3ed-273">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5e3ed-273">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5e3ed-274">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5e3ed-274">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_203.png

