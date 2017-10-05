---
title: 'Kurz: Azure Active Directory integrace s etouches | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a etouches."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 76cccaa8-859c-4c16-9d1d-8a6496fc7520
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 3cd9e9d6aae924369065ca492b1f6380c0ddc5fe
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-etouches"></a><span data-ttu-id="bb375-103">Kurz: Azure Active Directory integrace s etouches</span><span class="sxs-lookup"><span data-stu-id="bb375-103">Tutorial: Azure Active Directory integration with etouches</span></span>

<span data-ttu-id="bb375-104">V tomto kurzu zjistěte, jak integrovat etouches s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bb375-104">In this tutorial, you learn how to integrate etouches with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bb375-105">Integrace etouches s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="bb375-105">Integrating etouches with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="bb375-106">Můžete řídit ve službě Azure AD, který má přístup k etouches</span><span class="sxs-lookup"><span data-stu-id="bb375-106">You can control in Azure AD who has access to etouches</span></span>
- <span data-ttu-id="bb375-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k etouches (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="bb375-107">You can enable your users to automatically get signed-on to etouches (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bb375-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="bb375-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="bb375-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bb375-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bb375-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bb375-110">Prerequisites</span></span>

<span data-ttu-id="bb375-111">Konfigurace integrace Azure AD s etouches, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="bb375-111">To configure Azure AD integration with etouches, you need the following items:</span></span>

- <span data-ttu-id="bb375-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="bb375-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bb375-113">Etouches jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="bb375-113">An etouches single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bb375-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="bb375-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bb375-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="bb375-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bb375-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="bb375-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bb375-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bb375-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bb375-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="bb375-118">Scenario description</span></span>
<span data-ttu-id="bb375-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="bb375-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bb375-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="bb375-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bb375-121">Přidání etouches z Galerie</span><span class="sxs-lookup"><span data-stu-id="bb375-121">Adding etouches from the gallery</span></span>
2. <span data-ttu-id="bb375-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bb375-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-etouches-from-the-gallery"></a><span data-ttu-id="bb375-123">Přidání etouches z Galerie</span><span class="sxs-lookup"><span data-stu-id="bb375-123">Adding etouches from the gallery</span></span>
<span data-ttu-id="bb375-124">Při konfiguraci integrace etouches do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS etouches z galerie.</span><span class="sxs-lookup"><span data-stu-id="bb375-124">To configure the integration of etouches into Azure AD, you need to add etouches from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="bb375-125">**Pokud chcete přidat etouches z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bb375-125">**To add etouches from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="bb375-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="bb375-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="bb375-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="bb375-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="bb375-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bb375-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="bb375-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bb375-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="bb375-133">Do vyhledávacího pole zadejte **etouches**, vyberte **etouches** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bb375-133">In the search box, type **etouches**, select **etouches** from result panel then click **Add** button to add the application.</span></span>

    ![etouches v seznamu výsledků](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="bb375-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bb375-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="bb375-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s etouches podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="bb375-136">In this section, you configure and test Azure AD single sign-on with etouches based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="bb375-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v etouches je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bb375-137">For single sign-on to work, Azure AD needs to know what the counterpart user in etouches is to a user in Azure AD.</span></span> <span data-ttu-id="bb375-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v etouches musí navázat.</span><span class="sxs-lookup"><span data-stu-id="bb375-138">In other words, a link relationship between an Azure AD user and the related user in etouches needs to be established.</span></span>

<span data-ttu-id="bb375-139">V etouches, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="bb375-139">In etouches, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="bb375-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s etouches, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="bb375-140">To configure and test Azure AD single sign-on with etouches, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="bb375-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="bb375-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="bb375-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bb375-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bb375-143">**[Vytvořit testovací uživatele s etouches](#create-an-etouches-test-user)**  – Pokud chcete mít protějšek Britta Simon v etouches propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="bb375-143">**[Create an etouches test user](#create-an-etouches-test-user)** - to have a counterpart of Britta Simon in etouches that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="bb375-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bb375-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bb375-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="bb375-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="bb375-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bb375-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="bb375-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci etouches.</span><span class="sxs-lookup"><span data-stu-id="bb375-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your etouches application.</span></span>

<span data-ttu-id="bb375-148">**Ke konfiguraci Azure AD jednotné přihlašování s etouches, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bb375-148">**To configure Azure AD single sign-on with etouches, perform the following steps:**</span></span>

1. <span data-ttu-id="bb375-149">Na portálu Azure na **etouches** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="bb375-149">In the Azure portal, on the **etouches** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="bb375-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bb375-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_samlbase.png)

3. <span data-ttu-id="bb375-153">Na **etouches domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bb375-153">On the **etouches Domain and URLs** section, perform the following steps:</span></span>

    ![jednotné přihlašování informace etouches domény a adresy URL](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_url.png)

    <span data-ttu-id="bb375-155">a.</span><span class="sxs-lookup"><span data-stu-id="bb375-155">a.</span></span> <span data-ttu-id="bb375-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`</span><span class="sxs-lookup"><span data-stu-id="bb375-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`</span></span>

    <span data-ttu-id="bb375-157">b.</span><span class="sxs-lookup"><span data-stu-id="bb375-157">b.</span></span> <span data-ttu-id="bb375-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.eiseverywhere.com/<instance name>`</span><span class="sxs-lookup"><span data-stu-id="bb375-158">In the **Identifier** textbox, type a URL using the following pattern: `https://www.eiseverywhere.com/<instance name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bb375-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="bb375-159">These values are not real.</span></span> <span data-ttu-id="bb375-160">Aktualizujte hodnotu skutečné znakem na adresu URL a identifikátor, který je vysvětlen později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="bb375-160">You update the value with the actual Sign on URL and Identifier, which is explained later in the tutorial.</span></span>
    > 

4. <span data-ttu-id="bb375-161">aplikace etouches očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="bb375-161">etouches application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="bb375-162">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bb375-162">Configure the following claims for this application.</span></span> <span data-ttu-id="bb375-163">Můžete spravovat hodnoty těchto atributů z **atribut uživatele** aplikace.</span><span class="sxs-lookup"><span data-stu-id="bb375-163">You can manage the values of these attributes from the **User Attribute** of the application.</span></span> <span data-ttu-id="bb375-164">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="bb375-164">The following screenshot shows an example for this.</span></span> 

    ![Atribut uživatele](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_attribute.png) 

5. <span data-ttu-id="bb375-166">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bb375-166">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="bb375-167">Název atributu</span><span class="sxs-lookup"><span data-stu-id="bb375-167">Attribute Name</span></span> | <span data-ttu-id="bb375-168">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="bb375-168">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="bb375-169">E-mail</span><span class="sxs-lookup"><span data-stu-id="bb375-169">Email</span></span> | <span data-ttu-id="bb375-170">User.Mail</span><span class="sxs-lookup"><span data-stu-id="bb375-170">user.mail</span></span> |    
    
    <span data-ttu-id="bb375-171">a.</span><span class="sxs-lookup"><span data-stu-id="bb375-171">a.</span></span> <span data-ttu-id="bb375-172">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bb375-172">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Přidání atributu](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_04.png)

    ![Atribut dialogové okno Přidání](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="bb375-175">b.</span><span class="sxs-lookup"><span data-stu-id="bb375-175">b.</span></span> <span data-ttu-id="bb375-176">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="bb375-176">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="bb375-177">c.</span><span class="sxs-lookup"><span data-stu-id="bb375-177">c.</span></span> <span data-ttu-id="bb375-178">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="bb375-178">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="bb375-179">d.</span><span class="sxs-lookup"><span data-stu-id="bb375-179">d.</span></span> <span data-ttu-id="bb375-180">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="bb375-180">Click **Ok**.</span></span> 

6. <span data-ttu-id="bb375-181">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="bb375-181">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_certificate.png) 

7. <span data-ttu-id="bb375-183">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bb375-183">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-etouches-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="bb375-185">Získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, proveďte následující kroky v aplikaci etouches:</span><span class="sxs-lookup"><span data-stu-id="bb375-185">To get SSO configured for your application, perform the following steps in the etouches application:</span></span> 

    ![Konfigurace etouches](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_06.png) 

    <span data-ttu-id="bb375-187">a.</span><span class="sxs-lookup"><span data-stu-id="bb375-187">a.</span></span> <span data-ttu-id="bb375-188">Přihlášení k **etouches** aplikace pomocí oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="bb375-188">Login to **etouches** application using the Admin rights.</span></span>
   
    <span data-ttu-id="bb375-189">b.</span><span class="sxs-lookup"><span data-stu-id="bb375-189">b.</span></span> <span data-ttu-id="bb375-190">Přejděte na **SAML** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="bb375-190">Go to the **SAML** Configuration.</span></span>

    <span data-ttu-id="bb375-191">c.</span><span class="sxs-lookup"><span data-stu-id="bb375-191">c.</span></span> <span data-ttu-id="bb375-192">V **obecné nastavení** část, otevřete svůj certifikát stažený z portálu Azure v poznámkovém bloku, kopírovat obsah a pak ji vložit do textového pole IDP metadat.</span><span class="sxs-lookup"><span data-stu-id="bb375-192">In the **General Settings** section, open your downloaded certificate from Azure portal in notepad, copy the content, and then paste it into the IDP metadata textbox.</span></span> 

    <span data-ttu-id="bb375-193">d.</span><span class="sxs-lookup"><span data-stu-id="bb375-193">d.</span></span> <span data-ttu-id="bb375-194">Klikněte na **Uložit & zůstat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bb375-194">Click on the **Save & Stay** button.</span></span>
  
    <span data-ttu-id="bb375-195">e.</span><span class="sxs-lookup"><span data-stu-id="bb375-195">e.</span></span> <span data-ttu-id="bb375-196">Klikněte na **Metadata aktualizace** tlačítko v části SAML Metadata.</span><span class="sxs-lookup"><span data-stu-id="bb375-196">Click on the **Update Metadata** button in the SAML Metadata section.</span></span> 

    <span data-ttu-id="bb375-197">f.</span><span class="sxs-lookup"><span data-stu-id="bb375-197">f.</span></span> <span data-ttu-id="bb375-198">To otevře se stránka a provádět jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bb375-198">This opens the page and perform SSO.</span></span> <span data-ttu-id="bb375-199">Jakmile funguje jednotné přihlašování pak můžete nastavit uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="bb375-199">Once the SSO is working then you can set up the username.</span></span>

    <span data-ttu-id="bb375-200">g.</span><span class="sxs-lookup"><span data-stu-id="bb375-200">g.</span></span> <span data-ttu-id="bb375-201">Do pole uživatelské jméno, vyberte **emailaddress** jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="bb375-201">In the Username field, select the **emailaddress** as shown in the image below.</span></span> 

    <span data-ttu-id="bb375-202">h.</span><span class="sxs-lookup"><span data-stu-id="bb375-202">h.</span></span> <span data-ttu-id="bb375-203">Kopírování **SP entity ID** a vložte ji do **identifikátor** textovému poli, která je v **etouches domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bb375-203">Copy the **SP entity ID** value and paste it into the **Identifier**  textbox, which is in **etouches Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="bb375-204">i.</span><span class="sxs-lookup"><span data-stu-id="bb375-204">i.</span></span> <span data-ttu-id="bb375-205">Kopírování **URL jednotného přihlašování nebo ACS** a vložte ji do **přihlásit na adrese URL** textovému poli, která je v **etouches domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bb375-205">Copy the **SSO URL / ACS** value and paste it into the **Sign on URL** textbox, which is in **etouches Domain and URLs** section on Azure portal.</span></span>
   
> [!TIP]
> <span data-ttu-id="bb375-206">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="bb375-206">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="bb375-207">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="bb375-207">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="bb375-208">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bb375-208">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="bb375-209">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="bb375-209">Create an Azure AD test user</span></span>
<span data-ttu-id="bb375-210">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bb375-210">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="bb375-212">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bb375-212">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="bb375-213">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="bb375-213">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-etouches-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bb375-215">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="bb375-215">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-etouches-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bb375-217">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bb375-217">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-etouches-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bb375-219">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bb375-219">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogové okno uživatele](./media/active-directory-saas-etouches-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bb375-221">a.</span><span class="sxs-lookup"><span data-stu-id="bb375-221">a.</span></span> <span data-ttu-id="bb375-222">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bb375-222">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bb375-223">b.</span><span class="sxs-lookup"><span data-stu-id="bb375-223">b.</span></span> <span data-ttu-id="bb375-224">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bb375-224">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bb375-225">c.</span><span class="sxs-lookup"><span data-stu-id="bb375-225">c.</span></span> <span data-ttu-id="bb375-226">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="bb375-226">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="bb375-227">d.</span><span class="sxs-lookup"><span data-stu-id="bb375-227">d.</span></span> <span data-ttu-id="bb375-228">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="bb375-228">Click **Create**.</span></span>
 
### <a name="create-an-etouches-test-user"></a><span data-ttu-id="bb375-229">Vytvořit uživatele s etouches testu</span><span class="sxs-lookup"><span data-stu-id="bb375-229">Create an etouches test user</span></span>

<span data-ttu-id="bb375-230">V této části vytvoříte volal Britta Simon v etouches uživatele.</span><span class="sxs-lookup"><span data-stu-id="bb375-230">In this section, you create a user called Britta Simon in etouches.</span></span> <span data-ttu-id="bb375-231">Práce s [tým podpory etouches klienta](https://www.etouches.com/event-software/support/customer-support/) přidat uživatele do etouches platformy.</span><span class="sxs-lookup"><span data-stu-id="bb375-231">Work with [etouches Client support team](https://www.etouches.com/event-software/support/customer-support/) to add the users in the etouches platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="bb375-232">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="bb375-232">Assign the Azure AD test user</span></span>

<span data-ttu-id="bb375-233">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k etouches.</span><span class="sxs-lookup"><span data-stu-id="bb375-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to etouches.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="bb375-235">**Pokud chcete přiřadit Britta Simon etouches, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bb375-235">**To assign Britta Simon to etouches, perform the following steps:**</span></span>

1. <span data-ttu-id="bb375-236">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bb375-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="bb375-238">V seznamu aplikací vyberte **etouches**.</span><span class="sxs-lookup"><span data-stu-id="bb375-238">In the applications list, select **etouches**.</span></span>

    ![V seznamu aplikací na etouches odkaz](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_app.png) 

3. <span data-ttu-id="bb375-240">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="bb375-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202] 

4. <span data-ttu-id="bb375-242">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bb375-242">Click **Add** button.</span></span> <span data-ttu-id="bb375-243">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bb375-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="bb375-245">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="bb375-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="bb375-246">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bb375-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bb375-247">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bb375-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="bb375-248">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bb375-248">Test single sign-on</span></span>


<span data-ttu-id="bb375-249">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="bb375-249">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="bb375-250">Když kliknete na dlaždici etouches na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci etouches.</span><span class="sxs-lookup"><span data-stu-id="bb375-250">When you click the etouches tile in the Access Panel, you should get automatically signed-on to your etouches application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bb375-251">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="bb375-251">Additional resources</span></span>

* [<span data-ttu-id="bb375-252">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bb375-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bb375-253">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bb375-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_203.png
