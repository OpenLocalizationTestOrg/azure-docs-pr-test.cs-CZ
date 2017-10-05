---
title: 'Kurz: Azure Active Directory integrace s FileCloud | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a FileCloud."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f39f0ddd-b504-4562-971f-77b88d1e75fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: ad03516f684acc59912ffc57f6e0712828bd03f2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-filecloud"></a><span data-ttu-id="ecde6-103">Kurz: Azure Active Directory integrace s FileCloud</span><span class="sxs-lookup"><span data-stu-id="ecde6-103">Tutorial: Azure Active Directory integration with FileCloud</span></span>

<span data-ttu-id="ecde6-104">V tomto kurzu zjistěte, jak integrovat FileCloud s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ecde6-104">In this tutorial, you learn how to integrate FileCloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ecde6-105">Integrace FileCloud s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ecde6-105">Integrating FileCloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ecde6-106">Můžete ovládat ve službě Azure AD, který má přístup k FileCloud.</span><span class="sxs-lookup"><span data-stu-id="ecde6-106">You can control in Azure AD who has access to FileCloud.</span></span>
- <span data-ttu-id="ecde6-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k FileCloud (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ecde6-107">You can enable your users to automatically get signed-on to FileCloud (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="ecde6-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ecde6-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="ecde6-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ecde6-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ecde6-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ecde6-110">Prerequisites</span></span>

<span data-ttu-id="ecde6-111">Konfigurace integrace Azure AD s FileCloud, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="ecde6-111">To configure Azure AD integration with FileCloud, you need the following items:</span></span>

- <span data-ttu-id="ecde6-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ecde6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ecde6-113">FileCloud jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ecde6-113">A FileCloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ecde6-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ecde6-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ecde6-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ecde6-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ecde6-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ecde6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ecde6-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ecde6-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ecde6-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ecde6-118">Scenario description</span></span>
<span data-ttu-id="ecde6-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ecde6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ecde6-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ecde6-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ecde6-121">Přidání FileCloud z Galerie</span><span class="sxs-lookup"><span data-stu-id="ecde6-121">Adding FileCloud from the gallery</span></span>
2. <span data-ttu-id="ecde6-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ecde6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-filecloud-from-the-gallery"></a><span data-ttu-id="ecde6-123">Přidání FileCloud z Galerie</span><span class="sxs-lookup"><span data-stu-id="ecde6-123">Adding FileCloud from the gallery</span></span>
<span data-ttu-id="ecde6-124">Při konfiguraci integrace FileCloud do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS FileCloud z galerie.</span><span class="sxs-lookup"><span data-stu-id="ecde6-124">To configure the integration of FileCloud into Azure AD, you need to add FileCloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ecde6-125">**Pokud chcete přidat FileCloud z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ecde6-125">**To add FileCloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ecde6-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ecde6-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="ecde6-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ecde6-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ecde6-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ecde6-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="ecde6-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ecde6-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="ecde6-133">Do vyhledávacího pole zadejte **FileCloud**, vyberte **FileCloud** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ecde6-133">In the search box, type **FileCloud**, select **FileCloud** from result panel then click **Add** button to add the application.</span></span>

    ![FileCloud v seznamu výsledků](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="ecde6-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ecde6-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="ecde6-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s FileCloud podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="ecde6-136">In this section, you configure and test Azure AD single sign-on with FileCloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ecde6-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v FileCloud je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ecde6-137">For single sign-on to work, Azure AD needs to know what the counterpart user in FileCloud is to a user in Azure AD.</span></span> <span data-ttu-id="ecde6-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v FileCloud musí navázat.</span><span class="sxs-lookup"><span data-stu-id="ecde6-138">In other words, a link relationship between an Azure AD user and the related user in FileCloud needs to be established.</span></span>

<span data-ttu-id="ecde6-139">V FileCloud, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="ecde6-139">In FileCloud, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ecde6-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s FileCloud, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="ecde6-140">To configure and test Azure AD single sign-on with FileCloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ecde6-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="ecde6-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ecde6-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ecde6-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ecde6-143">**[Vytvoření zkušebního uživatele FileCloud](#create-a-filecloud-test-user)**  – Pokud chcete mít protějšek Britta Simon v FileCloud propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="ecde6-143">**[Create a FileCloud test user](#create-a-filecloud-test-user)** - to have a counterpart of Britta Simon in FileCloud that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ecde6-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ecde6-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ecde6-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ecde6-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="ecde6-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ecde6-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="ecde6-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci FileCloud.</span><span class="sxs-lookup"><span data-stu-id="ecde6-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your FileCloud application.</span></span>

<span data-ttu-id="ecde6-148">**Ke konfiguraci Azure AD jednotné přihlašování s FileCloud, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ecde6-148">**To configure Azure AD single sign-on with FileCloud, perform the following steps:**</span></span>

1. <span data-ttu-id="ecde6-149">Na portálu Azure na **FileCloud** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ecde6-149">In the Azure portal, on the **FileCloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="ecde6-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ecde6-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_samlbase.png)

3. <span data-ttu-id="ecde6-153">Na **FileCloud domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ecde6-153">On the **FileCloud Domain and URLs** section, perform the following steps:</span></span>

    ![FileCloud domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_url.png)

    <span data-ttu-id="ecde6-155">a.</span><span class="sxs-lookup"><span data-stu-id="ecde6-155">a.</span></span> <span data-ttu-id="ecde6-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.filecloudhosted.com`</span><span class="sxs-lookup"><span data-stu-id="ecde6-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.filecloudhosted.com`</span></span>

    <span data-ttu-id="ecde6-157">b.</span><span class="sxs-lookup"><span data-stu-id="ecde6-157">b.</span></span> <span data-ttu-id="ecde6-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.filecloudhosted.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`</span><span class="sxs-lookup"><span data-stu-id="ecde6-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.filecloudhosted.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ecde6-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="ecde6-159">These values are not real.</span></span> <span data-ttu-id="ecde6-160">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ecde6-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ecde6-161">Obraťte se na [tým podpory FileCloud klienta](mailto:support@codelathe.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="ecde6-161">Contact [FileCloud Client support team](mailto:support@codelathe.com) to get these values.</span></span>

4. <span data-ttu-id="ecde6-162">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ecde6-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_certificate.png) 

5. <span data-ttu-id="ecde6-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ecde6-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-filecloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ecde6-166">Na **FileCloud konfigurace** klikněte na tlačítko **konfigurace FileCloud** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="ecde6-166">On the **FileCloud Configuration** section, click **Configure FileCloud** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ecde6-167">Kopírování **SAML Entity ID** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="ecde6-167">Copy the **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Konfigurace FileCloud](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_configure.png) 

7. <span data-ttu-id="ecde6-169">V okně prohlížeče jiných webových přihlašování ke klientovi FileCloud jako správce.</span><span class="sxs-lookup"><span data-stu-id="ecde6-169">In a different web browser window, sign-on to your FileCloud tenant as an administrator.</span></span>

8. <span data-ttu-id="ecde6-170">V levém navigačním podokně klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ecde6-170">On the left navigation pane, click **Settings**.</span></span> 
   
    ![Nastavení oddílu aplikace na straně](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_000.png)

9. <span data-ttu-id="ecde6-172">Klikněte na tlačítko **jednotného přihlašování k** karty v části nastavení.</span><span class="sxs-lookup"><span data-stu-id="ecde6-172">Click **SSO** tab on Settings section.</span></span> 
   
    ![Jeden straně přihlašování karta na aplikace](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_001.png)

10. <span data-ttu-id="ecde6-174">Vyberte **SAML** jako **výchozí typ jednotného přihlašování k** na **jednotné přihlašování na (SSO) nastavení** panelu.</span><span class="sxs-lookup"><span data-stu-id="ecde6-174">Select **SAML** as **Default SSO Type** on **Single Sign On (SSO) Settings** panel.</span></span>
   
    ![Jeden straně přihlašování nastavení panely na aplikace](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_002.png)

11. <span data-ttu-id="ecde6-176">Vložení **SAML Entity ID**, který jste zkopírovali z portálu Azure do **adresa URL koncového bodu IdP** textové pole.</span><span class="sxs-lookup"><span data-stu-id="ecde6-176">Paste **SAML Entity ID**, which you have copied from Azure portal into the **IdP End Point URL** textbox.</span></span>

    ![Adresa URL bodu IDP End textové pole](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_003.png)

12. <span data-ttu-id="ecde6-178">V poznámkovém bloku otevřete váš soubor stažený metadata, zkopírujte obsah ho do schránky a vložte jej do **IdP metadata** textové pole na **SAML nastavení** panelu.</span><span class="sxs-lookup"><span data-stu-id="ecde6-178">Open your downloaded metadata file in notepad, copy the content of it into your clipboard, and then paste it to the **IdP Meta Data** textbox on **SAML Settings** panel.</span></span>

    ![Část dat Meta IDP na straně aplikace](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_004.png)

13. <span data-ttu-id="ecde6-180">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ecde6-180">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="ecde6-181">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="ecde6-181">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ecde6-182">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="ecde6-182">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ecde6-183">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ecde6-183">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="ecde6-184">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ecde6-184">Create an Azure AD test user</span></span>

<span data-ttu-id="ecde6-185">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ecde6-185">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="ecde6-187">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ecde6-187">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ecde6-188">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ecde6-188">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-filecloud-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="ecde6-190">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ecde6-190">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-filecloud-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="ecde6-192">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ecde6-192">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-filecloud-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="ecde6-194">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ecde6-194">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-filecloud-tutorial/create_aaduser_04.png)

    <span data-ttu-id="ecde6-196">a.</span><span class="sxs-lookup"><span data-stu-id="ecde6-196">a.</span></span> <span data-ttu-id="ecde6-197">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ecde6-197">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ecde6-198">b.</span><span class="sxs-lookup"><span data-stu-id="ecde6-198">b.</span></span> <span data-ttu-id="ecde6-199">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ecde6-199">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="ecde6-200">c.</span><span class="sxs-lookup"><span data-stu-id="ecde6-200">c.</span></span> <span data-ttu-id="ecde6-201">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="ecde6-201">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="ecde6-202">d.</span><span class="sxs-lookup"><span data-stu-id="ecde6-202">d.</span></span> <span data-ttu-id="ecde6-203">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ecde6-203">Click **Create**.</span></span>
 
### <a name="create-a-filecloud-test-user"></a><span data-ttu-id="ecde6-204">Vytvoření zkušebního uživatele FileCloud</span><span class="sxs-lookup"><span data-stu-id="ecde6-204">Create a FileCloud test user</span></span>

<span data-ttu-id="ecde6-205">Cílem této části je vytvoření uživatele v FileCloud nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ecde6-205">The objective of this section is to create a user called Britta Simon in FileCloud.</span></span> <span data-ttu-id="ecde6-206">FileCloud podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="ecde6-206">FileCloud supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="ecde6-207">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="ecde6-207">There is no action item for you in this section.</span></span> <span data-ttu-id="ecde6-208">Nový uživatel se vytvoří během pokusu o přístup k FileCloud, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="ecde6-208">A new user is created during an attempt to access FileCloud if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="ecde6-209">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat [tým podpory FileCloud klienta](mailto:support@codelathe.com).</span><span class="sxs-lookup"><span data-stu-id="ecde6-209">If you need to create a user manually, you need to contact the [FileCloud Client support team](mailto:support@codelathe.com).</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="ecde6-210">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ecde6-210">Assign the Azure AD test user</span></span>

<span data-ttu-id="ecde6-211">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu FileCloud.</span><span class="sxs-lookup"><span data-stu-id="ecde6-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to FileCloud.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="ecde6-213">**Pokud chcete přiřadit Britta Simon FileCloud, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ecde6-213">**To assign Britta Simon to FileCloud, perform the following steps:**</span></span>

1. <span data-ttu-id="ecde6-214">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ecde6-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ecde6-216">V seznamu aplikací vyberte **FileCloud**.</span><span class="sxs-lookup"><span data-stu-id="ecde6-216">In the applications list, select **FileCloud**.</span></span>

    ![V seznamu aplikací na FileCloud odkaz](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_app.png)  

3. <span data-ttu-id="ecde6-218">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ecde6-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="ecde6-220">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ecde6-220">Click **Add** button.</span></span> <span data-ttu-id="ecde6-221">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ecde6-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="ecde6-223">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ecde6-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ecde6-224">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ecde6-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ecde6-225">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ecde6-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="ecde6-226">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ecde6-226">Test single sign-on</span></span>

<span data-ttu-id="ecde6-227">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="ecde6-227">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="ecde6-228">Když kliknete na dlaždici FileCloud na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci FileCloud.</span><span class="sxs-lookup"><span data-stu-id="ecde6-228">When you click the FileCloud tile in the Access Panel, you should get automatically signed-on to your FileCloud application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ecde6-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ecde6-229">Additional resources</span></span>

* [<span data-ttu-id="ecde6-230">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ecde6-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ecde6-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ecde6-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_203.png

