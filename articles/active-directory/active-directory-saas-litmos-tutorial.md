---
title: 'Kurz: Azure Active Directory integrace s Litmos | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Litmos."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: jeedes
ms.assetid: cfaae4bb-e8e5-41d1-ac88-8cc369653036
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: ef1b5860ba0a406022bbd11afb366d14bee2c57d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-litmos"></a><span data-ttu-id="0ef9a-103">Kurz: Azure Active Directory integrace s Litmos</span><span class="sxs-lookup"><span data-stu-id="0ef9a-103">Tutorial: Azure Active Directory integration with Litmos</span></span>

<span data-ttu-id="0ef9a-104">V tomto kurzu zjistěte, jak integrovat Litmos s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0ef9a-104">In this tutorial, you learn how to integrate Litmos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0ef9a-105">Integrace Litmos s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0ef9a-105">Integrating Litmos with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0ef9a-106">Můžete ovládat ve službě Azure AD, který má přístup k Litmos.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-106">You can control in Azure AD who has access to Litmos.</span></span>
- <span data-ttu-id="0ef9a-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Litmos (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-107">You can enable your users to automatically get signed-on to Litmos (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="0ef9a-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="0ef9a-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0ef9a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0ef9a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0ef9a-110">Prerequisites</span></span>

<span data-ttu-id="0ef9a-111">Konfigurace integrace Azure AD s Litmos, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="0ef9a-111">To configure Azure AD integration with Litmos, you need the following items:</span></span>

- <span data-ttu-id="0ef9a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0ef9a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0ef9a-113">Litmos jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="0ef9a-113">A Litmos single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0ef9a-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0ef9a-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0ef9a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0ef9a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0ef9a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0ef9a-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0ef9a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0ef9a-118">Scenario description</span></span>
<span data-ttu-id="0ef9a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0ef9a-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0ef9a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0ef9a-121">Přidání Litmos z Galerie</span><span class="sxs-lookup"><span data-stu-id="0ef9a-121">Adding Litmos from the gallery</span></span>
2. <span data-ttu-id="0ef9a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0ef9a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-litmos-from-the-gallery"></a><span data-ttu-id="0ef9a-123">Přidání Litmos z Galerie</span><span class="sxs-lookup"><span data-stu-id="0ef9a-123">Adding Litmos from the gallery</span></span>
<span data-ttu-id="0ef9a-124">Při konfiguraci integrace Litmos do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Litmos z galerie.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-124">To configure the integration of Litmos into Azure AD, you need to add Litmos from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0ef9a-125">**Pokud chcete přidat Litmos z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0ef9a-125">**To add Litmos from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0ef9a-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="0ef9a-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0ef9a-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="0ef9a-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="0ef9a-133">Do vyhledávacího pole zadejte **Litmos**, vyberte **Litmos** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-133">In the search box, type **Litmos**, select **Litmos** from result panel then click **Add** button to add the application.</span></span>

    ![Litmos v seznamu výsledků](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="0ef9a-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0ef9a-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="0ef9a-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Litmos podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="0ef9a-136">In this section, you configure and test Azure AD single sign-on with Litmos based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0ef9a-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Litmos je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Litmos is to a user in Azure AD.</span></span> <span data-ttu-id="0ef9a-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Litmos musí navázat.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-138">In other words, a link relationship between an Azure AD user and the related user in Litmos needs to be established.</span></span>

<span data-ttu-id="0ef9a-139">V Litmos, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-139">In Litmos, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0ef9a-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Litmos, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="0ef9a-140">To configure and test Azure AD single sign-on with Litmos, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0ef9a-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0ef9a-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0ef9a-143">**[Vytvoření zkušebního uživatele Litmos](#create-a-litmos-test-user)**  – Pokud chcete mít protějšek Britta Simon v Litmos propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-143">**[Create a Litmos test user](#create-a-litmos-test-user)** - to have a counterpart of Britta Simon in Litmos that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0ef9a-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0ef9a-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="0ef9a-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0ef9a-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="0ef9a-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Litmos.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Litmos application.</span></span>

<span data-ttu-id="0ef9a-148">**Ke konfiguraci Azure AD jednotné přihlašování s Litmos, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0ef9a-148">**To configure Azure AD single sign-on with Litmos, perform the following steps:**</span></span>

1. <span data-ttu-id="0ef9a-149">Na portálu Azure na **Litmos** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-149">In the Azure portal, on the **Litmos** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="0ef9a-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_samlbase.png)

3. <span data-ttu-id="0ef9a-153">Na **Litmos domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0ef9a-153">On the **Litmos Domain and URLs** section, perform the following steps:</span></span>

    ![Litmos domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_url.png)

    <span data-ttu-id="0ef9a-155">a.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-155">a.</span></span> <span data-ttu-id="0ef9a-156">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.litmos.com/account/Login`</span><span class="sxs-lookup"><span data-stu-id="0ef9a-156">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.litmos.com/account/Login`</span></span>

    <span data-ttu-id="0ef9a-157">b.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-157">b.</span></span> <span data-ttu-id="0ef9a-158">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.litmos.com/integration/samllogin`</span><span class="sxs-lookup"><span data-stu-id="0ef9a-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.litmos.com/integration/samllogin`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0ef9a-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-159">These values are not real.</span></span> <span data-ttu-id="0ef9a-160">Tyto hodnoty aktualizovat se skutečným identifikátorem a adresa URL odpovědi, které jsou vysvětlené později v kurzu nebo kontaktujte [tým podpory Litmos](https://www.litmos.com/contact-us/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-160">Update these values with the actual Identifier and Reply URL, which are explained later in tutorial or contact [Litmos support team](https://www.litmos.com/contact-us/) to get these values.</span></span>

4. <span data-ttu-id="0ef9a-161">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_certificate.png)

5. <span data-ttu-id="0ef9a-163">Jako součást konfigurace, budete muset přizpůsobit **atributy tokenu SAML** pro vaši aplikaci Litmos.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-163">As part of the configuration, you need to customize the **SAML Token Attributes** for your Litmos application.</span></span>

    ![Atribut části](./media/active-directory-saas-litmos-tutorial/tutorial_attribute.png)
           
    | <span data-ttu-id="0ef9a-165">Název atributu</span><span class="sxs-lookup"><span data-stu-id="0ef9a-165">Attribute Name</span></span>   | <span data-ttu-id="0ef9a-166">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="0ef9a-166">Attribute Value</span></span> |   
    | ---------------  | ----------------|
    | <span data-ttu-id="0ef9a-167">FirstName</span><span class="sxs-lookup"><span data-stu-id="0ef9a-167">FirstName</span></span> |<span data-ttu-id="0ef9a-168">User.givenName</span><span class="sxs-lookup"><span data-stu-id="0ef9a-168">user.givenname</span></span> |
    | <span data-ttu-id="0ef9a-169">Příjmení</span><span class="sxs-lookup"><span data-stu-id="0ef9a-169">LastName</span></span>  |<span data-ttu-id="0ef9a-170">User.Surname</span><span class="sxs-lookup"><span data-stu-id="0ef9a-170">user.surname</span></span> |
    | <span data-ttu-id="0ef9a-171">E-mail</span><span class="sxs-lookup"><span data-stu-id="0ef9a-171">Email</span></span> |<span data-ttu-id="0ef9a-172">User.Mail</span><span class="sxs-lookup"><span data-stu-id="0ef9a-172">user.mail</span></span> |

    <span data-ttu-id="0ef9a-173">a.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-173">a.</span></span> <span data-ttu-id="0ef9a-174">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-174">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Přidání atributu](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_04.png)

    ![Přidání atributu Dailog](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="0ef9a-177">b.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-177">b.</span></span> <span data-ttu-id="0ef9a-178">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-178">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="0ef9a-179">c.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-179">c.</span></span> <span data-ttu-id="0ef9a-180">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-180">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="0ef9a-181">d.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-181">d.</span></span> <span data-ttu-id="0ef9a-182">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-182">Click **Ok**.</span></span>     

6. <span data-ttu-id="0ef9a-183">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-183">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-litmos-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="0ef9a-185">V okně jiný prohlížeč přihlašování k webu společnosti Litmos jako správce.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-185">In a different browser window, sign-on to your Litmos company site as administrator.</span></span>

8. <span data-ttu-id="0ef9a-186">V navigačním panelu na levé straně klikněte na **účty**.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-186">In the navigation bar on the left side, click **Accounts**.</span></span>
   
    ![Oddílu účtů na straně aplikace][22] 

9. <span data-ttu-id="0ef9a-188">Klikněte **integrace** kartě.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-188">Click the **Integrations** tab.</span></span>
   
    ![Karta integrace][23] 

10. <span data-ttu-id="0ef9a-190">Na **integrace** kartě, přejděte dolů k položce **3. stran integrace**a potom klikněte na **SAML 2.0** kartě.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-190">On the **Integrations** tab, scroll down to **3rd Party Integrations**, and then click **SAML 2.0** tab.</span></span>
   
    ![SAML 2.0 části][24] 

11. <span data-ttu-id="0ef9a-192">Zkopírujte hodnotu v části **je koncový bod SAML pro litmos:** a vložte ji do **adresa URL odpovědi** textového pole v **Litmos domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-192">Copy the value under **The SAML endpoint for litmos is:** and paste it into the **Reply URL** textbox in the **Litmos Domain and URLs** section in Azure portal.</span></span> 
   
    ![Koncový bod SAML][26] 

12. <span data-ttu-id="0ef9a-194">Ve vašem **Litmos** aplikace, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0ef9a-194">In your **Litmos** application, perform the following steps:</span></span>
    
     ![Litmos aplikace][25] 
     
     <span data-ttu-id="0ef9a-196">a.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-196">a.</span></span> <span data-ttu-id="0ef9a-197">Klikněte na tlačítko **povolit SAML**.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-197">Click **Enable SAML**.</span></span>
    
     <span data-ttu-id="0ef9a-198">b.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-198">b.</span></span> <span data-ttu-id="0ef9a-199">V poznámkovém bloku otevřete váš kódování base-64 kódovaného certifikátu, zkopírujte obsah ho do schránky a vložte jej do **certifikát X.509 SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-199">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **SAML X.509 Certificate** textbox.</span></span>
     
     <span data-ttu-id="0ef9a-200">c.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-200">c.</span></span> <span data-ttu-id="0ef9a-201">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-201">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="0ef9a-202">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="0ef9a-202">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0ef9a-203">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-203">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0ef9a-204">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0ef9a-204">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="0ef9a-205">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0ef9a-205">Create an Azure AD test user</span></span>

<span data-ttu-id="0ef9a-206">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-206">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="0ef9a-208">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0ef9a-208">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0ef9a-209">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-209">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-litmos-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="0ef9a-211">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-211">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-litmos-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="0ef9a-213">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-213">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="0ef9a-215">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0ef9a-215">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png)

    <span data-ttu-id="0ef9a-217">a.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-217">a.</span></span> <span data-ttu-id="0ef9a-218">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-218">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0ef9a-219">b.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-219">b.</span></span> <span data-ttu-id="0ef9a-220">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-220">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="0ef9a-221">c.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-221">c.</span></span> <span data-ttu-id="0ef9a-222">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-222">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="0ef9a-223">d.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-223">d.</span></span> <span data-ttu-id="0ef9a-224">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-224">Click **Create**.</span></span>
  
### <a name="create-a-litmos-test-user"></a><span data-ttu-id="0ef9a-225">Vytvoření zkušebního uživatele Litmos</span><span class="sxs-lookup"><span data-stu-id="0ef9a-225">Create a Litmos test user</span></span>

<span data-ttu-id="0ef9a-226">Cílem této části je vytvoření uživatele v Litmos nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-226">The objective of this section is to create a user called Britta Simon in Litmos.</span></span>  
<span data-ttu-id="0ef9a-227">Litmos aplikace podporuje pouze za běhu zřizování.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-227">The Litmos application supports Just-in-Time provisioning.</span></span> <span data-ttu-id="0ef9a-228">To znamená, uživatelský účet se automaticky vytvoří v případě potřeby při pokusu o přístup k aplikaci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-228">This means, a user account is automatically created if necessary during an attempt to access the application using the Access Panel.</span></span>

<span data-ttu-id="0ef9a-229">**Vytvoření uživatele v Litmos nazývá Britta Simon, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0ef9a-229">**To create a user called Britta Simon in Litmos, perform the following steps:**</span></span>

1. <span data-ttu-id="0ef9a-230">V okně jiný prohlížeč přihlašování k webu společnosti Litmos jako správce.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-230">In a different browser window, sign-on to your Litmos company site as administrator.</span></span>

2. <span data-ttu-id="0ef9a-231">V navigačním panelu na levé straně klikněte na **účty**.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-231">In the navigation bar on the left side, click **Accounts**.</span></span>
   
    ![Oddílu účtů na straně aplikace][22] 

3. <span data-ttu-id="0ef9a-233">Klikněte **integrace** kartě.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-233">Click the **Integrations** tab.</span></span>
   
    ![Karta integrace][23] 

4. <span data-ttu-id="0ef9a-235">Na **integrace** kartě, přejděte dolů k položce **3. stran integrace**a potom klikněte na **SAML 2.0** kartě.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-235">On the **Integrations** tab, scroll down to **3rd Party Integrations**, and then click **SAML 2.0** tab.</span></span>
   
    ![SAML 2.0][24] 
    
5. <span data-ttu-id="0ef9a-237">Vyberte **generovat uživatelů**</span><span class="sxs-lookup"><span data-stu-id="0ef9a-237">Select **Autogenerate Users**</span></span>
   
    ![Generovat uživatelů][27] 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="0ef9a-239">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0ef9a-239">Assign the Azure AD test user</span></span>

<span data-ttu-id="0ef9a-240">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Litmos.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-240">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Litmos.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="0ef9a-242">**Pokud chcete přiřadit Britta Simon Litmos, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0ef9a-242">**To assign Britta Simon to Litmos, perform the following steps:**</span></span>

1. <span data-ttu-id="0ef9a-243">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-243">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="0ef9a-245">V seznamu aplikací vyberte **Litmos**.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-245">In the applications list, select **Litmos**.</span></span>

    ![V seznamu aplikací na Litmos odkaz](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_app.png)  

3. <span data-ttu-id="0ef9a-247">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-247">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="0ef9a-249">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-249">Click **Add** button.</span></span> <span data-ttu-id="0ef9a-250">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="0ef9a-252">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-252">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0ef9a-253">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0ef9a-254">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="0ef9a-255">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0ef9a-255">Test single sign-on</span></span>

<span data-ttu-id="0ef9a-256">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-256">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  

<span data-ttu-id="0ef9a-257">Když kliknete na dlaždici Litmos na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Litmos.</span><span class="sxs-lookup"><span data-stu-id="0ef9a-257">When you click the Litmos tile in the Access Panel, you should get automatically signed-on to your Litmos application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0ef9a-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0ef9a-258">Additional resources</span></span>

* [<span data-ttu-id="0ef9a-259">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0ef9a-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0ef9a-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0ef9a-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[100]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png

