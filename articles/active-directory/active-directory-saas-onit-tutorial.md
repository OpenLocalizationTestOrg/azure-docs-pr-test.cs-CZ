---
title: 'Kurz: Azure Active Directory integrace s Onit | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Onit."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bc479a28-8fcd-493f-ac53-681975a5149c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: 47c0055b89dbcf6a30a7f9ac5a33913e7bf463fa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-onit"></a><span data-ttu-id="b6068-103">Kurz: Azure Active Directory integrace s Onit</span><span class="sxs-lookup"><span data-stu-id="b6068-103">Tutorial: Azure Active Directory integration with Onit</span></span>

<span data-ttu-id="b6068-104">V tomto kurzu zjistěte, jak integrovat Onit s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b6068-104">In this tutorial, you learn how to integrate Onit with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b6068-105">Integrace Onit s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b6068-105">Integrating Onit with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b6068-106">Můžete ovládat ve službě Azure AD, který má přístup k Onit.</span><span class="sxs-lookup"><span data-stu-id="b6068-106">You can control in Azure AD who has access to Onit.</span></span>
- <span data-ttu-id="b6068-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Onit (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b6068-107">You can enable your users to automatically get signed-on to Onit (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="b6068-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b6068-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="b6068-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b6068-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b6068-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b6068-110">Prerequisites</span></span>

<span data-ttu-id="b6068-111">Konfigurace integrace Azure AD s Onit, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="b6068-111">To configure Azure AD integration with Onit, you need the following items:</span></span>

- <span data-ttu-id="b6068-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b6068-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b6068-113">Onit jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b6068-113">An Onit single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b6068-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b6068-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b6068-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b6068-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b6068-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b6068-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b6068-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b6068-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b6068-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b6068-118">Scenario description</span></span>

<span data-ttu-id="b6068-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b6068-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b6068-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b6068-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b6068-121">Přidání Onit z Galerie</span><span class="sxs-lookup"><span data-stu-id="b6068-121">Adding Onit from the gallery</span></span>
2. <span data-ttu-id="b6068-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b6068-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-onit-from-the-gallery"></a><span data-ttu-id="b6068-123">Přidání Onit z Galerie</span><span class="sxs-lookup"><span data-stu-id="b6068-123">Adding Onit from the gallery</span></span>
<span data-ttu-id="b6068-124">Při konfiguraci integrace Onit do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Onit z galerie.</span><span class="sxs-lookup"><span data-stu-id="b6068-124">To configure the integration of Onit into Azure AD, you need to add Onit from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b6068-125">**Pokud chcete přidat Onit z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b6068-125">**To add Onit from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b6068-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b6068-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="b6068-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b6068-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b6068-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b6068-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="b6068-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b6068-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="b6068-133">Do vyhledávacího pole zadejte **Onit**, vyberte **Onit** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b6068-133">In the search box, type **Onit**, select **Onit** from result panel then click **Add** button to add the application.</span></span>

    ![Onit v seznamu výsledků](./media/active-directory-saas-onit-tutorial/tutorial_onit_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="b6068-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b6068-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="b6068-136">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Onit podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="b6068-136">In this section, you configure and test Azure AD single sign-on with Onit based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b6068-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Onit je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b6068-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Onit is to a user in Azure AD.</span></span> <span data-ttu-id="b6068-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Onit musí navázat.</span><span class="sxs-lookup"><span data-stu-id="b6068-138">In other words, a link relationship between an Azure AD user and the related user in Onit needs to be established.</span></span>

<span data-ttu-id="b6068-139">V Onit, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="b6068-139">In Onit, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b6068-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Onit, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="b6068-140">To configure and test Azure AD single sign-on with Onit, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b6068-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="b6068-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b6068-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b6068-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b6068-143">**[Vytvořit testovací uživatele s Onit](#create-an-onit-test-user)**  – Pokud chcete mít protějšek Britta Simon v Onit propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="b6068-143">**[Create an Onit test user](#create-an-onit-test-user)** - to have a counterpart of Britta Simon in Onit that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b6068-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b6068-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b6068-145">**[Test jednotného přihlašování](#test-single-sign-on)**  ověřit, zda funguje konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b6068-145">**[Test single sign-on](#test-single-sign-on)** to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="b6068-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b6068-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="b6068-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Onit.</span><span class="sxs-lookup"><span data-stu-id="b6068-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Onit application.</span></span>

<span data-ttu-id="b6068-148">**Ke konfiguraci Azure AD jednotné přihlašování s Onit, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b6068-148">**To configure Azure AD single sign-on with Onit, perform the following steps:**</span></span>

1. <span data-ttu-id="b6068-149">Na portálu Azure na **Onit** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b6068-149">In the Azure portal, on the **Onit** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="b6068-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b6068-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-onit-tutorial/tutorial_onit_samlbase.png)

3. <span data-ttu-id="b6068-153">Na **Onit domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b6068-153">On the **Onit Domain and URLs** section, perform the following steps:</span></span>

    ![Onit domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-onit-tutorial/tutorial_onit_url.png)

    <span data-ttu-id="b6068-155">a.</span><span class="sxs-lookup"><span data-stu-id="b6068-155">a.</span></span> <span data-ttu-id="b6068-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<sub-domain>.onit.com`</span><span class="sxs-lookup"><span data-stu-id="b6068-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<sub-domain>.onit.com`</span></span>

    <span data-ttu-id="b6068-157">b.</span><span class="sxs-lookup"><span data-stu-id="b6068-157">b.</span></span> <span data-ttu-id="b6068-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<sub-domain>.onit.com`</span><span class="sxs-lookup"><span data-stu-id="b6068-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<sub-domain>.onit.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b6068-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="b6068-159">These values are not real.</span></span> <span data-ttu-id="b6068-160">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="b6068-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b6068-161">Obraťte se na [tým podpory Onit klienta](https://www.onit.com/support) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="b6068-161">Contact [Onit Client support team](https://www.onit.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="b6068-162">Na **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="b6068-162">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-onit-tutorial/tutorial_onit_certificate.png) 

5. <span data-ttu-id="b6068-164">Aplikace Onit očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="b6068-164">Onit application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="b6068-165">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b6068-165">Please configure the following claims for this application.</span></span> <span data-ttu-id="b6068-166">Můžete spravovat hodnoty těchto atributů z **"Atrribute"** aplikace.</span><span class="sxs-lookup"><span data-stu-id="b6068-166">You can manage the values of these attributes from the **"Atrribute"** tab of the application.</span></span> <span data-ttu-id="b6068-167">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="b6068-167">The following screenshot shows an example for this.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-onit-tutorial/tutorial_onit_attribute.png) 

6. <span data-ttu-id="b6068-169">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b6068-169">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="b6068-170">Název atributu</span><span class="sxs-lookup"><span data-stu-id="b6068-170">Attribute Name</span></span> | <span data-ttu-id="b6068-171">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="b6068-171">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="b6068-172">E-mailu</span><span class="sxs-lookup"><span data-stu-id="b6068-172">email</span></span> | <span data-ttu-id="b6068-173">User.Mail</span><span class="sxs-lookup"><span data-stu-id="b6068-173">user.mail</span></span> |
    
    <span data-ttu-id="b6068-174">a.</span><span class="sxs-lookup"><span data-stu-id="b6068-174">a.</span></span> <span data-ttu-id="b6068-175">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b6068-175">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-onit-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-onit-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="b6068-178">b.</span><span class="sxs-lookup"><span data-stu-id="b6068-178">b.</span></span> <span data-ttu-id="b6068-179">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="b6068-179">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="b6068-180">c.</span><span class="sxs-lookup"><span data-stu-id="b6068-180">c.</span></span> <span data-ttu-id="b6068-181">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="b6068-181">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="b6068-182">d.</span><span class="sxs-lookup"><span data-stu-id="b6068-182">d.</span></span> <span data-ttu-id="b6068-183">Ponechte **Namespace** prázdné.</span><span class="sxs-lookup"><span data-stu-id="b6068-183">Leave the **Namespace** blank.</span></span>
    
    <span data-ttu-id="b6068-184">e.</span><span class="sxs-lookup"><span data-stu-id="b6068-184">e.</span></span> <span data-ttu-id="b6068-185">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b6068-185">Click **Ok**.</span></span>

7. <span data-ttu-id="b6068-186">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b6068-186">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-onit-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="b6068-188">Na **Onit konfigurace** klikněte na tlačítko **konfigurace Onit** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="b6068-188">On the **Onit Configuration** section, click **Configure Onit** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b6068-189">Kopírování **Sign-Out adresu URL, SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="b6068-189">Copy the **Sign-Out URL, SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace Onit](./media/active-directory-saas-onit-tutorial/tutorial_onit_configure.png)

9. <span data-ttu-id="b6068-191">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Onit.</span><span class="sxs-lookup"><span data-stu-id="b6068-191">In a different web browser window, log into your Onit company site as an administrator.</span></span>

10. <span data-ttu-id="b6068-192">V nabídce v horní části, klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="b6068-192">In the menu on the top, click **Administration**.</span></span>
   
   <span data-ttu-id="b6068-193">![Správa](./media/active-directory-saas-onit-tutorial/IC791174.png "správy")</span><span class="sxs-lookup"><span data-stu-id="b6068-193">![Administration](./media/active-directory-saas-onit-tutorial/IC791174.png "Administration")</span></span>
11. <span data-ttu-id="b6068-194">Klikněte na tlačítko **úpravy Corporation**.</span><span class="sxs-lookup"><span data-stu-id="b6068-194">Click **Edit Corporation**.</span></span>
   
   <span data-ttu-id="b6068-195">![Upravit Corporation](./media/active-directory-saas-onit-tutorial/IC791175.png "Corporation úpravy")</span><span class="sxs-lookup"><span data-stu-id="b6068-195">![Edit Corporation](./media/active-directory-saas-onit-tutorial/IC791175.png "Edit Corporation")</span></span>
   
12. <span data-ttu-id="b6068-196">Klikněte **zabezpečení** kartě.</span><span class="sxs-lookup"><span data-stu-id="b6068-196">Click the **Security** tab.</span></span>
    
    <span data-ttu-id="b6068-197">![Informace o společnosti upravit](./media/active-directory-saas-onit-tutorial/IC791176.png "informace společnosti úpravy")</span><span class="sxs-lookup"><span data-stu-id="b6068-197">![Edit Company Information](./media/active-directory-saas-onit-tutorial/IC791176.png "Edit Company Information")</span></span>

13. <span data-ttu-id="b6068-198">Na **zabezpečení** kartu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b6068-198">On the **Security** tab, perform the following steps:</span></span>

    <span data-ttu-id="b6068-199">![Jednotné přihlašování](./media/active-directory-saas-onit-tutorial/IC791177.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="b6068-199">![Single Sign-On](./media/active-directory-saas-onit-tutorial/IC791177.png "Single Sign-On")</span></span>

    <span data-ttu-id="b6068-200">a.</span><span class="sxs-lookup"><span data-stu-id="b6068-200">a.</span></span> <span data-ttu-id="b6068-201">Jako **strategie ověřování**, vyberte **jednotné přihlašování a heslo**.</span><span class="sxs-lookup"><span data-stu-id="b6068-201">As **Authentication Strategy**, select **Single Sign On and Password**.</span></span>
    
    <span data-ttu-id="b6068-202">b.</span><span class="sxs-lookup"><span data-stu-id="b6068-202">b.</span></span> <span data-ttu-id="b6068-203">V **Idp cílová adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b6068-203">In **Idp Target URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="b6068-204">c.</span><span class="sxs-lookup"><span data-stu-id="b6068-204">c.</span></span> <span data-ttu-id="b6068-205">V **adresy URL odhlašovací Idp** textovému poli, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b6068-205">In **Idp logout URL** textbox, paste the value of  **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="b6068-206">d.</span><span class="sxs-lookup"><span data-stu-id="b6068-206">d.</span></span> <span data-ttu-id="b6068-207">V **Idp Cert otisk (SHA1)** textovému poli, Vložit **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b6068-207">In **Idp Cert Fingerprint (SHA1)** textbox, paste the  **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="b6068-208">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="b6068-208">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b6068-209">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="b6068-209">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b6068-210">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b6068-210">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="b6068-211">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b6068-211">Create an Azure AD test user</span></span>

<span data-ttu-id="b6068-212">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b6068-212">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="b6068-214">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b6068-214">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b6068-215">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b6068-215">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-onit-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="b6068-217">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b6068-217">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-onit-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="b6068-219">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b6068-219">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-onit-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="b6068-221">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b6068-221">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-onit-tutorial/create_aaduser_04.png)

    <span data-ttu-id="b6068-223">a.</span><span class="sxs-lookup"><span data-stu-id="b6068-223">a.</span></span> <span data-ttu-id="b6068-224">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b6068-224">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b6068-225">b.</span><span class="sxs-lookup"><span data-stu-id="b6068-225">b.</span></span> <span data-ttu-id="b6068-226">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b6068-226">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="b6068-227">c.</span><span class="sxs-lookup"><span data-stu-id="b6068-227">c.</span></span> <span data-ttu-id="b6068-228">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="b6068-228">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="b6068-229">d.</span><span class="sxs-lookup"><span data-stu-id="b6068-229">d.</span></span> <span data-ttu-id="b6068-230">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b6068-230">Click **Create**.</span></span>
 
### <a name="create-an-onit-test-user"></a><span data-ttu-id="b6068-231">Vytvořit uživatele s Onit testu</span><span class="sxs-lookup"><span data-stu-id="b6068-231">Create an Onit test user</span></span>

<span data-ttu-id="b6068-232">Pokud chcete povolit uživatelům Azure AD přihlášení do Onit, musí být zřízená do Onit.</span><span class="sxs-lookup"><span data-stu-id="b6068-232">In order to enable Azure AD users to log into Onit, they must be provisioned into Onit.</span></span>  

<span data-ttu-id="b6068-233">V případě Onit zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b6068-233">In the case of Onit, provisioning is a manual task.</span></span>

<span data-ttu-id="b6068-234">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b6068-234">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="b6068-235">Přihlaste se k vaší **Onit** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="b6068-235">Sign on to your **Onit** company site as an administrator.</span></span>
2. <span data-ttu-id="b6068-236">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b6068-236">Click **Add User**.</span></span>
   
   <span data-ttu-id="b6068-237">![Správa](./media/active-directory-saas-onit-tutorial/IC791180.png "správy")</span><span class="sxs-lookup"><span data-stu-id="b6068-237">![Administration](./media/active-directory-saas-onit-tutorial/IC791180.png "Administration")</span></span>
3. <span data-ttu-id="b6068-238">Na **přidat uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b6068-238">On the **Add User** dialog page, perform the following steps:</span></span>
   
   <span data-ttu-id="b6068-239">![Přidat uživatele](./media/active-directory-saas-onit-tutorial/IC791181.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="b6068-239">![Add User](./media/active-directory-saas-onit-tutorial/IC791181.png "Add User")</span></span>
   
  1. <span data-ttu-id="b6068-240">Typ **název** a **e-mailovou adresu** platný Azure AD účtu chcete mají být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="b6068-240">Type the **Name** and the **Email Address** of a valid Azure AD account you want to provision into the related textboxes.</span></span>
  2. <span data-ttu-id="b6068-241">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b6068-241">Click **Create**.</span></span>    
   
 > [!NOTE]
 > <span data-ttu-id="b6068-242">Držitel účtu Azure Active Directory obdrží e-mailu a dodržuje odkaz potvrďte svůj účet, pak se změní na aktivní.</span><span class="sxs-lookup"><span data-stu-id="b6068-242">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="b6068-243">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b6068-243">Assign the Azure AD test user</span></span>

<span data-ttu-id="b6068-244">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Onit.</span><span class="sxs-lookup"><span data-stu-id="b6068-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Onit.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="b6068-246">**Pokud chcete přiřadit Britta Simon Onit, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b6068-246">**To assign Britta Simon to Onit, perform the following steps:**</span></span>

1. <span data-ttu-id="b6068-247">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b6068-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b6068-249">V seznamu aplikací vyberte **Onit**.</span><span class="sxs-lookup"><span data-stu-id="b6068-249">In the applications list, select **Onit**.</span></span>

    ![V seznamu aplikací na Onit odkaz](./media/active-directory-saas-onit-tutorial/tutorial_onit_app.png)  

3. <span data-ttu-id="b6068-251">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b6068-251">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="b6068-253">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b6068-253">Click **Add** button.</span></span> <span data-ttu-id="b6068-254">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b6068-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="b6068-256">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b6068-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b6068-257">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b6068-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b6068-258">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b6068-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="b6068-259">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b6068-259">Test single sign-on</span></span>

<span data-ttu-id="b6068-260">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b6068-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b6068-261">Když kliknete na dlaždici Onit na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Onit.</span><span class="sxs-lookup"><span data-stu-id="b6068-261">When you click the Onit tile in the Access Panel, you should get automatically signed-on to your Onit application.</span></span>
<span data-ttu-id="b6068-262">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b6068-262">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="b6068-263">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b6068-263">Additional resources</span></span>

* [<span data-ttu-id="b6068-264">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b6068-264">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b6068-265">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b6068-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-onit-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-onit-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-onit-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-onit-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-onit-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-onit-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-onit-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-onit-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-onit-tutorial/tutorial_general_203.png

