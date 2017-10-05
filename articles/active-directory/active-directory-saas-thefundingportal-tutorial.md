---
title: "Kurz: Azure Active Directory integrace s aplikace finančních prostředků Portal | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a portálu finančních prostředků."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4663cc8a-976a-4c6c-b3b4-1e5df9b66744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: d0bfc793bb26c551f85706eaec857962a3415e1f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-the-funding-portal"></a><span data-ttu-id="f0c2c-103">Kurz: Azure Active Directory integrace s aplikace Portal finančních prostředků</span><span class="sxs-lookup"><span data-stu-id="f0c2c-103">Tutorial: Azure Active Directory integration with The Funding Portal</span></span>

<span data-ttu-id="f0c2c-104">V tomto kurzu zjistěte, jak integrovat aplikace Portal finančních prostředků s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f0c2c-104">In this tutorial, you learn how to integrate The Funding Portal with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f0c2c-105">Integrace aplikace Portal finančních prostředků s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f0c2c-105">Integrating The Funding Portal with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f0c2c-106">Můžete řídit ve službě Azure AD, který má přístup k portálu finančních prostředků</span><span class="sxs-lookup"><span data-stu-id="f0c2c-106">You can control in Azure AD who has access to The Funding Portal</span></span>
- <span data-ttu-id="f0c2c-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k aplikace finančních prostředků Portal (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0c2c-107">You can enable your users to automatically get signed-on to The Funding Portal (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f0c2c-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f0c2c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f0c2c-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f0c2c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f0c2c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f0c2c-110">Prerequisites</span></span>

<span data-ttu-id="f0c2c-111">Ke konfiguraci integrace služby Azure AD pomocí portálu finančních prostředků, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="f0c2c-111">To configure Azure AD integration with The Funding Portal, you need the following items:</span></span>

- <span data-ttu-id="f0c2c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0c2c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f0c2c-113">Portálu finančních prostředků jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f0c2c-113">A The Funding Portal single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f0c2c-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f0c2c-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f0c2c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f0c2c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f0c2c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f0c2c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f0c2c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f0c2c-118">Scenario description</span></span>
<span data-ttu-id="f0c2c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f0c2c-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f0c2c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f0c2c-121">Přidání aplikace Portal finančních prostředků z Galerie</span><span class="sxs-lookup"><span data-stu-id="f0c2c-121">Adding The Funding Portal from the gallery</span></span>
2. <span data-ttu-id="f0c2c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f0c2c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-the-funding-portal-from-the-gallery"></a><span data-ttu-id="f0c2c-123">Přidání aplikace Portal finančních prostředků z Galerie</span><span class="sxs-lookup"><span data-stu-id="f0c2c-123">Adding The Funding Portal from the gallery</span></span>
<span data-ttu-id="f0c2c-124">Konfigurace integrace portál finančních prostředků do Azure AD, potřebujete přidat portálu finančních prostředků z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-124">To configure the integration of The Funding Portal into Azure AD, you need to add The Funding Portal from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f0c2c-125">**Pokud chcete přidat aplikace Portal finančních prostředků z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f0c2c-125">**To add The Funding Portal from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f0c2c-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f0c2c-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f0c2c-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="f0c2c-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="f0c2c-133">Do vyhledávacího pole zadejte **aplikace finančních prostředků Portal**.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-133">In the search box, type **The Funding Portal**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_search.png)

5. <span data-ttu-id="f0c2c-135">Na panelu výsledků vyberte **aplikace finančních prostředků Portal**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-135">In the results panel, select **The Funding Portal**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f0c2c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f0c2c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f0c2c-138">V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí aplikace Portal finančních prostředků na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="f0c2c-138">In this section, you configure and test Azure AD single sign-on with The Funding Portal based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f0c2c-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem portálu finančních prostředků je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in The Funding Portal is to a user in Azure AD.</span></span> <span data-ttu-id="f0c2c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské portálu finančních prostředků je potřeba vytvořit.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-140">In other words, a link relationship between an Azure AD user and the related user in The Funding Portal needs to be established.</span></span>

<span data-ttu-id="f0c2c-141">Portálu finančních prostředků, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-141">In The Funding Portal, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f0c2c-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování portálu finančních prostředků, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="f0c2c-142">To configure and test Azure AD single sign-on with The Funding Portal, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f0c2c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f0c2c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f0c2c-145">**[Vytváření testovacího uživatele portálu finančních prostředků](#creating-the-funding-portal-test-user)**  – Pokud chcete mít protějšek Britta Simon v The finančních prostředků portál, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-145">**[Creating The Funding Portal test user](#creating-the-funding-portal-test-user)** - to have a counterpart of Britta Simon in The Funding Portal that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f0c2c-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f0c2c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f0c2c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f0c2c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f0c2c-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci portálu finančních prostředků.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your The Funding Portal application.</span></span>

<span data-ttu-id="f0c2c-150">**Ke konfiguraci Azure AD jednotné přihlašování portálu finančních prostředků, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f0c2c-150">**To configure Azure AD single sign-on with The Funding Portal, perform the following steps:**</span></span>

1. <span data-ttu-id="f0c2c-151">Na portálu Azure na **aplikace finančních prostředků Portal** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-151">In the Azure portal, on the **The Funding Portal** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="f0c2c-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_samlbase.png)

3. <span data-ttu-id="f0c2c-155">Na **doméně prostředků portál a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f0c2c-155">On the **The Funding Portal Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_url.png)

    <span data-ttu-id="f0c2c-157">a.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-157">a.</span></span> <span data-ttu-id="f0c2c-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.regenteducation.net/`</span><span class="sxs-lookup"><span data-stu-id="f0c2c-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.regenteducation.net/`</span></span>

    <span data-ttu-id="f0c2c-159">b.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-159">b.</span></span> <span data-ttu-id="f0c2c-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.regenteducation.net`</span><span class="sxs-lookup"><span data-stu-id="f0c2c-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.regenteducation.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f0c2c-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-161">These values are not real.</span></span> <span data-ttu-id="f0c2c-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f0c2c-163">Obraťte se na [tým podpory finančních prostředků klient portál](mailto:info@regenteducation.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-163">Contact [The Funding Portal Client support team](mailto:info@regenteducation.com) to get these values.</span></span> 

4. <span data-ttu-id="f0c2c-164">Aplikace z portálu finančních prostředků očekává kontrolní výrazy SAML obsahuje atribut s názvem "externalId1".</span><span class="sxs-lookup"><span data-stu-id="f0c2c-164">The Funding Portal application expects the SAML assertions to contain an attribute named "externalId1".</span></span> <span data-ttu-id="f0c2c-165">Hodnota "externalId1" by měl být rozpoznaný studentID.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-165">The value of "externalId1" should be a recognized studentID.</span></span> <span data-ttu-id="f0c2c-166">Konfigurace deklarace identity "externalId1" pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-166">Configure the "externalId1" claim for this application.</span></span> <span data-ttu-id="f0c2c-167">Můžete spravovat hodnoty těchto atributů z **uživatelské atributy** aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-167">You can manage the values of these attributes from the **User Attributes** of the application.</span></span> <span data-ttu-id="f0c2c-168">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-168">The following screenshot shows an example for this.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_attribute.png)

5. <span data-ttu-id="f0c2c-170">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f0c2c-170">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>

    | <span data-ttu-id="f0c2c-171">Název atributu</span><span class="sxs-lookup"><span data-stu-id="f0c2c-171">Attribute Name</span></span> | <span data-ttu-id="f0c2c-172">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="f0c2c-172">Attribute Value</span></span> |
    | ------------------- | ---------------- |
    | <span data-ttu-id="f0c2c-173">externalId1</span><span class="sxs-lookup"><span data-stu-id="f0c2c-173">externalId1</span></span> | <span data-ttu-id="f0c2c-174">User.extensionattribute1</span><span class="sxs-lookup"><span data-stu-id="f0c2c-174">user.extensionattribute1</span></span> |

    <span data-ttu-id="f0c2c-175">a.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-175">a.</span></span> <span data-ttu-id="f0c2c-176">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-176">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="f0c2c-179">b.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-179">b.</span></span> <span data-ttu-id="f0c2c-180">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-180">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="f0c2c-181">c.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-181">c.</span></span> <span data-ttu-id="f0c2c-182">Z **hodnota atributu** vyberte atribut, který chcete použít týkající se vaší implementace.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-182">From the **Attribute Value** list, select the attribute you want to use for your implementation.</span></span> <span data-ttu-id="f0c2c-183">Například pokud je hodnota StudentID mít uložen v ExtensionAttribute1, pak vyberte user.extensionattribute1.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-183">For example, if you have stored the StudentID value in the ExtensionAttribute1, then select user.extensionattribute1.</span></span>
    
    <span data-ttu-id="f0c2c-184">d.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-184">d.</span></span> <span data-ttu-id="f0c2c-185">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-185">Click **Ok**.</span></span>
 
6. <span data-ttu-id="f0c2c-186">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-186">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_certificate.png) 

7. <span data-ttu-id="f0c2c-188">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-188">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="f0c2c-190">Konfigurace jednotného přihlašování na **aplikace finančních prostředků Portal** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory aplikace finančních prostředků Portal](mailto:info@regenteducation.com).</span><span class="sxs-lookup"><span data-stu-id="f0c2c-190">To configure single sign-on on **The Funding Portal** side, you need to send the downloaded **Metadata XML** to [The Funding Portal support team](mailto:info@regenteducation.com).</span></span> <span data-ttu-id="f0c2c-191">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-191">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="f0c2c-192">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="f0c2c-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f0c2c-193">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f0c2c-194">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f0c2c-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f0c2c-195">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0c2c-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="f0c2c-196">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="f0c2c-198">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f0c2c-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f0c2c-199">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-199">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f0c2c-201">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-201">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f0c2c-203">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-203">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f0c2c-205">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f0c2c-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f0c2c-207">a.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-207">a.</span></span> <span data-ttu-id="f0c2c-208">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f0c2c-209">b.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-209">b.</span></span> <span data-ttu-id="f0c2c-210">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f0c2c-211">c.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-211">c.</span></span> <span data-ttu-id="f0c2c-212">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f0c2c-213">d.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-213">d.</span></span> <span data-ttu-id="f0c2c-214">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-214">Click **Create**.</span></span>
 
### <a name="creating-the-funding-portal-test-user"></a><span data-ttu-id="f0c2c-215">Vytvoření aplikace finančních prostředků Portal testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f0c2c-215">Creating The Funding Portal test user</span></span>

<span data-ttu-id="f0c2c-216">V této části vytvoříte uživatele volat Britta Simon portálu finančních prostředků.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-216">In this section, you create a user called Britta Simon in The Funding Portal.</span></span> <span data-ttu-id="f0c2c-217">Práce s [tým podpory aplikace Portal finančních prostředků](mailto:info@regenteducation.com) k přidání testovacího uživatele a povolení jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-217">Work with [The Funding Portal support team](mailto:info@regenteducation.com) to add the test user and enable SSO.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f0c2c-218">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0c2c-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f0c2c-219">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu aplikace Portal finančních prostředků.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to The Funding Portal.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="f0c2c-221">**Pokud chcete přiřadit Britta Simon aplikace Portal finančních prostředků, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f0c2c-221">**To assign Britta Simon to The Funding Portal, perform the following steps:**</span></span>

1. <span data-ttu-id="f0c2c-222">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f0c2c-224">V seznamu aplikací vyberte **aplikace finančních prostředků Portal**.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-224">In the applications list, select **The Funding Portal**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_app.png) 

3. <span data-ttu-id="f0c2c-226">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="f0c2c-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-228">Click **Add** button.</span></span> <span data-ttu-id="f0c2c-229">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="f0c2c-231">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f0c2c-232">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f0c2c-233">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f0c2c-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f0c2c-234">Testing single sign-on</span></span>

<span data-ttu-id="f0c2c-235">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-235">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f0c2c-236">Když kliknete na dlaždici aplikace finančních prostředků Portal na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci portálu finančních prostředků.</span><span class="sxs-lookup"><span data-stu-id="f0c2c-236">When you click the The Funding Portal tile in the Access Panel, you should get automatically signed-on to your The Funding Portal application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f0c2c-237">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f0c2c-237">Additional resources</span></span>

* [<span data-ttu-id="f0c2c-238">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f0c2c-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f0c2c-239">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f0c2c-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_203.png

