---
title: 'Kurz: Azure Active Directory integrace s Clever | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Clever."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 069ff13a-310e-4366-a147-d6ec5cca12a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 84082ff567e37d7fff80be9e089c67cfab911861
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clever"></a><span data-ttu-id="88d92-103">Kurz: Azure Active Directory integrace s Clever</span><span class="sxs-lookup"><span data-stu-id="88d92-103">Tutorial: Azure Active Directory integration with Clever</span></span>

<span data-ttu-id="88d92-104">V tomto kurzu zjistěte, jak integrovat Clever s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="88d92-104">In this tutorial, you learn how to integrate Clever with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="88d92-105">Integrace Clever s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="88d92-105">Integrating Clever with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="88d92-106">Můžete ovládat ve službě Azure AD, který má přístup k Clever.</span><span class="sxs-lookup"><span data-stu-id="88d92-106">You can control in Azure AD who has access to Clever.</span></span>
- <span data-ttu-id="88d92-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Clever (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="88d92-107">You can enable your users to automatically get signed-on to Clever (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="88d92-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="88d92-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="88d92-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="88d92-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="88d92-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="88d92-110">Prerequisites</span></span>

<span data-ttu-id="88d92-111">Konfigurace integrace Azure AD s Clever, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="88d92-111">To configure Azure AD integration with Clever, you need the following items:</span></span>

- <span data-ttu-id="88d92-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="88d92-112">An Azure AD subscription</span></span>
- <span data-ttu-id="88d92-113">Inteligentní jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="88d92-113">A Clever single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="88d92-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="88d92-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="88d92-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="88d92-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="88d92-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="88d92-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="88d92-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="88d92-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="88d92-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="88d92-118">Scenario description</span></span>
<span data-ttu-id="88d92-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="88d92-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="88d92-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="88d92-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="88d92-121">Přidání Clever z Galerie</span><span class="sxs-lookup"><span data-stu-id="88d92-121">Adding Clever from the gallery</span></span>
2. <span data-ttu-id="88d92-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="88d92-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-clever-from-the-gallery"></a><span data-ttu-id="88d92-123">Přidání Clever z Galerie</span><span class="sxs-lookup"><span data-stu-id="88d92-123">Adding Clever from the gallery</span></span>
<span data-ttu-id="88d92-124">Při konfiguraci integrace Clever do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Clever z galerie.</span><span class="sxs-lookup"><span data-stu-id="88d92-124">To configure the integration of Clever into Azure AD, you need to add Clever from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="88d92-125">**Pokud chcete přidat Clever z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="88d92-125">**To add Clever from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="88d92-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="88d92-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="88d92-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="88d92-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="88d92-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="88d92-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="88d92-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="88d92-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="88d92-133">Do vyhledávacího pole zadejte **Clever**, vyberte **Clever** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="88d92-133">In the search box, type **Clever**, select **Clever** from result panel then click **Add** button to add the application.</span></span>

    ![Inteligentní v seznamu výsledků](./media/active-directory-saas-clever-tutorial/tutorial_clever_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="88d92-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="88d92-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="88d92-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Clever podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="88d92-136">In this section, you configure and test Azure AD single sign-on with Clever based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="88d92-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Clever je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="88d92-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Clever is to a user in Azure AD.</span></span> <span data-ttu-id="88d92-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Clever musí navázat.</span><span class="sxs-lookup"><span data-stu-id="88d92-138">In other words, a link relationship between an Azure AD user and the related user in Clever needs to be established.</span></span>

<span data-ttu-id="88d92-139">V Clever, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="88d92-139">In Clever, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="88d92-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Clever, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="88d92-140">To configure and test Azure AD single sign-on with Clever, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="88d92-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="88d92-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="88d92-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="88d92-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="88d92-143">**[Vytvoření inteligentní zkušebního uživatele](#create-a-clever-test-user)**  – Pokud chcete mít protějšek Britta Simon v Clever propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="88d92-143">**[Create a Clever test user](#create-a-clever-test-user)** - to have a counterpart of Britta Simon in Clever that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="88d92-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="88d92-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="88d92-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="88d92-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="88d92-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="88d92-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="88d92-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci inteligentní.</span><span class="sxs-lookup"><span data-stu-id="88d92-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Clever application.</span></span>

<span data-ttu-id="88d92-148">**Ke konfiguraci Azure AD jednotné přihlašování s Clever, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="88d92-148">**To configure Azure AD single sign-on with Clever, perform the following steps:**</span></span>

1. <span data-ttu-id="88d92-149">Na portálu Azure na **Clever** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="88d92-149">In the Azure portal, on the **Clever** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="88d92-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="88d92-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-clever-tutorial/tutorial_clever_samlbase.png)

3. <span data-ttu-id="88d92-153">Na **inteligentní domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="88d92-153">On the **Clever Domain and URLs** section, perform the following steps:</span></span>

    ![Inteligentní domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-clever-tutorial/tutorial_clever_url.png)

    <span data-ttu-id="88d92-155">a.</span><span class="sxs-lookup"><span data-stu-id="88d92-155">a.</span></span> <span data-ttu-id="88d92-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://clever.com/in/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="88d92-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://clever.com/in/<companyname>`</span></span>

    <span data-ttu-id="88d92-157">b.</span><span class="sxs-lookup"><span data-stu-id="88d92-157">b.</span></span> <span data-ttu-id="88d92-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://clever.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="88d92-158">In the **Identifier** textbox, type a URL using the following pattern: `https://clever.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="88d92-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="88d92-159">These values are not real.</span></span> <span data-ttu-id="88d92-160">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="88d92-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="88d92-161">Obraťte se na [tým podpory inteligentní klienta](https://clever.com/about/contact/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="88d92-161">Contact [Clever Client support team](https://clever.com/about/contact/) to get these values.</span></span>

4. <span data-ttu-id="88d92-162">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="88d92-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-clever-tutorial/tutorial_clever_certificate.png)

5. <span data-ttu-id="88d92-164">Inteligentní aplikace očekává SAML kontrolní výrazy ve specifickém formátu, který můžete přidat mapování vlastní atribut vyžaduje vaše **atributy tokenu SAML** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="88d92-164">The Clever application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **SAML Token Attributes** configuration.</span></span>

    <span data-ttu-id="88d92-165">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="88d92-165">The following screenshot shows an example for this.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-clever-tutorial/tutorial_clever_07.png) 

6. <span data-ttu-id="88d92-167">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku výše a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="88d92-167">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="88d92-168">Název atributu</span><span class="sxs-lookup"><span data-stu-id="88d92-168">Attribute Name</span></span>  | <span data-ttu-id="88d92-169">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="88d92-169">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    | <span data-ttu-id="88d92-170">clever.student.credentials.District\_uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="88d92-170">clever.student.credentials.district\_username</span></span>  | <span data-ttu-id="88d92-171">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="88d92-171">user.userprincipalname</span></span> |
    | <span data-ttu-id="88d92-172">FirstName</span><span class="sxs-lookup"><span data-stu-id="88d92-172">Firstname</span></span>  | <span data-ttu-id="88d92-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="88d92-173">user.givenname</span></span> |
    | <span data-ttu-id="88d92-174">Příjmení</span><span class="sxs-lookup"><span data-stu-id="88d92-174">Lastname</span></span>  | <span data-ttu-id="88d92-175">User.Surname</span><span class="sxs-lookup"><span data-stu-id="88d92-175">user.surname</span></span> |    

    <span data-ttu-id="88d92-176">a.</span><span class="sxs-lookup"><span data-stu-id="88d92-176">a.</span></span> <span data-ttu-id="88d92-177">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="88d92-177">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-clever-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-clever-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="88d92-180">b.</span><span class="sxs-lookup"><span data-stu-id="88d92-180">b.</span></span> <span data-ttu-id="88d92-181">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="88d92-181">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="88d92-182">c.</span><span class="sxs-lookup"><span data-stu-id="88d92-182">c.</span></span> <span data-ttu-id="88d92-183">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="88d92-183">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="88d92-184">d.</span><span class="sxs-lookup"><span data-stu-id="88d92-184">d.</span></span> <span data-ttu-id="88d92-185">Ponechte **Namespace** textové pole prázdné.</span><span class="sxs-lookup"><span data-stu-id="88d92-185">Leave the **Namespace** textbox blank.</span></span>
    
    <span data-ttu-id="88d92-186">d.</span><span class="sxs-lookup"><span data-stu-id="88d92-186">d.</span></span> <span data-ttu-id="88d92-187">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="88d92-187">Click **Ok**.</span></span>     

5. <span data-ttu-id="88d92-188">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="88d92-188">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-clever-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="88d92-190">Ke generování **Metadata** adresu url, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="88d92-190">To generate the **Metadata** url, perform the following steps:</span></span>

    <span data-ttu-id="88d92-191">a.</span><span class="sxs-lookup"><span data-stu-id="88d92-191">a.</span></span> <span data-ttu-id="88d92-192">Klikněte na tlačítko **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="88d92-192">Click **App registrations**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-clever-tutorial/tutorial_clever_appregistrations.png)
   
    <span data-ttu-id="88d92-194">b.</span><span class="sxs-lookup"><span data-stu-id="88d92-194">b.</span></span> <span data-ttu-id="88d92-195">Klikněte na tlačítko **koncové body** otevřete **koncové body** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="88d92-195">Click **Endpoints** to open **Endpoints** dialog box.</span></span>  
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-clever-tutorial/tutorial_clever_endpointicon.png)

    <span data-ttu-id="88d92-197">c.</span><span class="sxs-lookup"><span data-stu-id="88d92-197">c.</span></span> <span data-ttu-id="88d92-198">Klikněte na tlačítko Kopírovat kopírování **dokument FEDERAČNÍCH METADAT** adresy url a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="88d92-198">Click the copy button to copy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-clever-tutorial/tutorial_clever_endpoint.png)
     
    <span data-ttu-id="88d92-200">d.</span><span class="sxs-lookup"><span data-stu-id="88d92-200">d.</span></span> <span data-ttu-id="88d92-201">Nyní přejděte na stránku vlastností **Clever** a zkopírujte **Id aplikace** pomocí **kopie** tlačítko a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="88d92-201">Now go to the property page of **Clever** and copy the **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-clever-tutorial/tutorial_clever_appid.png)

    <span data-ttu-id="88d92-203">e.</span><span class="sxs-lookup"><span data-stu-id="88d92-203">e.</span></span> <span data-ttu-id="88d92-204">Vygenerovat **adresu URL metadat** pomocí následujícího vzorce:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="88d92-204">Generate the **Metadata URL** using the following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>   

9. <span data-ttu-id="88d92-205">V okně prohlížeče jiný web Přihlaste se k webu inteligentní společnosti jako správce.</span><span class="sxs-lookup"><span data-stu-id="88d92-205">In a different web browser window, log in to your Clever company site as an administrator.</span></span>

10. <span data-ttu-id="88d92-206">Na panelu nástrojů klikněte na tlačítko **rychlé přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="88d92-206">In the toolbar, click **Instant Login**.</span></span>

    <span data-ttu-id="88d92-207">![Rychlé přihlášení](./media/active-directory-saas-clever-tutorial/ic798984.png "rychlé přihlášení")</span><span class="sxs-lookup"><span data-stu-id="88d92-207">![Instant Login](./media/active-directory-saas-clever-tutorial/ic798984.png "Instant Login")</span></span>

11. <span data-ttu-id="88d92-208">Na **rychlé přihlášení** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="88d92-208">On the **Instant Login** page, perform the following steps:</span></span>
      
      <span data-ttu-id="88d92-209">![Rychlé přihlášení](./media/active-directory-saas-clever-tutorial/ic798985.png "rychlé přihlášení")</span><span class="sxs-lookup"><span data-stu-id="88d92-209">![Instant Login](./media/active-directory-saas-clever-tutorial/ic798985.png "Instant Login")</span></span>
      
      <span data-ttu-id="88d92-210">a.</span><span class="sxs-lookup"><span data-stu-id="88d92-210">a.</span></span> <span data-ttu-id="88d92-211">Typ **přihlašovací adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="88d92-211">Type the **Login URL**.</span></span>
      
      >[!NOTE]
      ><span data-ttu-id="88d92-212">**Přihlašovací adresa URL** je vlastní hodnota.</span><span class="sxs-lookup"><span data-stu-id="88d92-212">The **Login URL** is a custom value.</span></span> <span data-ttu-id="88d92-213">Obraťte se na [tým podpory inteligentní klienta](https://clever.com/about/contact/) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="88d92-213">Contact [Clever Client support team](https://clever.com/about/contact/) to get this value.</span></span>
      
      <span data-ttu-id="88d92-214">b.</span><span class="sxs-lookup"><span data-stu-id="88d92-214">b.</span></span> <span data-ttu-id="88d92-215">Jako **systém identit**, vyberte **služby AD FS**.</span><span class="sxs-lookup"><span data-stu-id="88d92-215">As **Identity System**, select **ADFS**.</span></span>

      <span data-ttu-id="88d92-216">c.</span><span class="sxs-lookup"><span data-stu-id="88d92-216">c.</span></span> <span data-ttu-id="88d92-217">Typ **adresu URL metadat** v **adresu URL metadat** textové pole.</span><span class="sxs-lookup"><span data-stu-id="88d92-217">Type the **Metadata URL** in the **Metadata URL** textbox.</span></span>
      
      <span data-ttu-id="88d92-218">d.</span><span class="sxs-lookup"><span data-stu-id="88d92-218">d.</span></span> <span data-ttu-id="88d92-219">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="88d92-219">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="88d92-220">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="88d92-220">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="88d92-221">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="88d92-221">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="88d92-222">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="88d92-222">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="88d92-223">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="88d92-223">Create an Azure AD test user</span></span>

<span data-ttu-id="88d92-224">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="88d92-224">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="88d92-226">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="88d92-226">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="88d92-227">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="88d92-227">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-clever-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="88d92-229">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="88d92-229">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-clever-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="88d92-231">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="88d92-231">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-clever-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="88d92-233">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="88d92-233">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-clever-tutorial/create_aaduser_04.png)

    <span data-ttu-id="88d92-235">a.</span><span class="sxs-lookup"><span data-stu-id="88d92-235">a.</span></span> <span data-ttu-id="88d92-236">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="88d92-236">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="88d92-237">b.</span><span class="sxs-lookup"><span data-stu-id="88d92-237">b.</span></span> <span data-ttu-id="88d92-238">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="88d92-238">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="88d92-239">c.</span><span class="sxs-lookup"><span data-stu-id="88d92-239">c.</span></span> <span data-ttu-id="88d92-240">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="88d92-240">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="88d92-241">d.</span><span class="sxs-lookup"><span data-stu-id="88d92-241">d.</span></span> <span data-ttu-id="88d92-242">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="88d92-242">Click **Create**.</span></span>
 
### <a name="create-a-clever-test-user"></a><span data-ttu-id="88d92-243">Vytvoření inteligentní zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="88d92-243">Create a Clever test user</span></span>

<span data-ttu-id="88d92-244">Pokud chcete povolit uživatelům Azure AD přihlášení k Clever, musí být zřízená do Clever.</span><span class="sxs-lookup"><span data-stu-id="88d92-244">To enable Azure AD users to log in to Clever, they must be provisioned into Clever.</span></span>

<span data-ttu-id="88d92-245">V případě Clever, pracovat s [tým podpory inteligentní klienta](https://clever.com/about/contact/) přidat uživatele do inteligentní platformy.</span><span class="sxs-lookup"><span data-stu-id="88d92-245">In case of Clever, Work with [Clever Client support team](https://clever.com/about/contact/) to add the users in the Clever platform.</span></span> <span data-ttu-id="88d92-246">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="88d92-246">Users must be created and activated before you use single sign-on.</span></span> 

>[!NOTE]
><span data-ttu-id="88d92-247">Můžete použít jakékoli jiné nástroje vytvoření inteligentní uživatelského účtu nebo rozhraní API poskytované Clever ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="88d92-247">You can use any other Clever user account creation tools or APIs provided by Clever to provision Azure AD user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="88d92-248">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="88d92-248">Assign the Azure AD test user</span></span>

<span data-ttu-id="88d92-249">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Clever.</span><span class="sxs-lookup"><span data-stu-id="88d92-249">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Clever.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="88d92-251">**Pokud chcete přiřadit Britta Simon Clever, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="88d92-251">**To assign Britta Simon to Clever, perform the following steps:**</span></span>

1. <span data-ttu-id="88d92-252">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="88d92-252">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="88d92-254">V seznamu aplikací vyberte **Clever**.</span><span class="sxs-lookup"><span data-stu-id="88d92-254">In the applications list, select **Clever**.</span></span>

    ![Odkaz Clever v seznamu aplikací](./media/active-directory-saas-clever-tutorial/tutorial_clever_app.png)  

3. <span data-ttu-id="88d92-256">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="88d92-256">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="88d92-258">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="88d92-258">Click **Add** button.</span></span> <span data-ttu-id="88d92-259">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="88d92-259">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="88d92-261">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="88d92-261">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="88d92-262">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="88d92-262">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="88d92-263">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="88d92-263">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="88d92-264">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="88d92-264">Test single sign-on</span></span>

<span data-ttu-id="88d92-265">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="88d92-265">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="88d92-266">Když kliknete na dlaždici inteligentní na přístupovém panelu jste měli získat automaticky přihlášení k aplikaci inteligentní.</span><span class="sxs-lookup"><span data-stu-id="88d92-266">When you click the Clever tile in the Access Panel, you should get automatically signed-on to your Clever application.</span></span>
<span data-ttu-id="88d92-267">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="88d92-267">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="88d92-268">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="88d92-268">Additional resources</span></span>

* [<span data-ttu-id="88d92-269">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="88d92-269">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="88d92-270">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="88d92-270">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clever-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clever-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clever-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clever-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clever-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clever-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clever-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clever-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clever-tutorial/tutorial_general_203.png

