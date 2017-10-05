---
title: 'Kurz: Azure Active Directory integrace s Citrix ShareFile | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Citrix ShareFile."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e14fc310-bac4-4f09-99ef-87e5c77288b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2017
ms.author: jeedes
ms.openlocfilehash: b85680104fe4f33638c559b2a12483a2312a4476
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a><span data-ttu-id="d4596-103">Kurz: Azure Active Directory integrace s Citrix ShareFile</span><span class="sxs-lookup"><span data-stu-id="d4596-103">Tutorial: Azure Active Directory integration with Citrix ShareFile</span></span>

<span data-ttu-id="d4596-104">V tomto kurzu zjistěte, jak integrovat Citrix ShareFile s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d4596-104">In this tutorial, you learn how to integrate Citrix ShareFile with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d4596-105">Integrace Citrix ShareFile s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d4596-105">Integrating Citrix ShareFile with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d4596-106">Můžete ovládat ve službě Azure AD, který má přístup k Citrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="d4596-106">You can control in Azure AD who has access to Citrix ShareFile.</span></span>
- <span data-ttu-id="d4596-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k systému Citrix ShareFile (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d4596-107">You can enable your users to automatically get signed-on to Citrix ShareFile (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="d4596-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d4596-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="d4596-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d4596-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d4596-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d4596-110">Prerequisites</span></span>

<span data-ttu-id="d4596-111">Konfigurace integrace Azure AD s Citrix ShareFile, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="d4596-111">To configure Azure AD integration with Citrix ShareFile, you need the following items:</span></span>

- <span data-ttu-id="d4596-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d4596-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d4596-113">Citrix ShareFile jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d4596-113">A Citrix ShareFile single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d4596-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d4596-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d4596-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d4596-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d4596-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d4596-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d4596-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d4596-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d4596-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d4596-118">Scenario description</span></span>
<span data-ttu-id="d4596-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d4596-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d4596-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d4596-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d4596-121">Přidat Citrix ShareFile z Galerie</span><span class="sxs-lookup"><span data-stu-id="d4596-121">Add Citrix ShareFile from the gallery</span></span>
2. <span data-ttu-id="d4596-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d4596-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-citrix-sharefile-from-the-gallery"></a><span data-ttu-id="d4596-123">Přidat Citrix ShareFile z Galerie</span><span class="sxs-lookup"><span data-stu-id="d4596-123">Add Citrix ShareFile from the gallery</span></span>
<span data-ttu-id="d4596-124">Při konfiguraci integrace Citrix ShareFile do služby Azure AD potřebujete přidat Citrix ShareFile z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="d4596-124">To configure the integration of Citrix ShareFile into Azure AD, you need to add Citrix ShareFile from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d4596-125">**Pokud chcete přidat Citrix ShareFile z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d4596-125">**To add Citrix ShareFile from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d4596-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d4596-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="d4596-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d4596-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d4596-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d4596-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="d4596-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d4596-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="d4596-133">Do vyhledávacího pole zadejte **Citrix ShareFile**, vyberte **Citrix ShareFile** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d4596-133">In the search box, type **Citrix ShareFile**, select **Citrix ShareFile** from result panel then click **Add** button to add the application.</span></span>

    ![Citrix ShareFile v seznamu výsledků](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="d4596-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d4596-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="d4596-136">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s Citrix ShareFile podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="d4596-136">In this section, you configure and test Azure AD single sign-on with Citrix ShareFile based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d4596-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Citrix ShareFile je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d4596-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Citrix ShareFile is to a user in Azure AD.</span></span> <span data-ttu-id="d4596-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Citrix ShareFile musí navázat.</span><span class="sxs-lookup"><span data-stu-id="d4596-138">In other words, a link relationship between an Azure AD user and the related user in Citrix ShareFile needs to be established.</span></span>

<span data-ttu-id="d4596-139">V systému Citrix ShareFile přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="d4596-139">In Citrix ShareFile, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d4596-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Citrix ShareFile, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="d4596-140">To configure and test Azure AD single sign-on with Citrix ShareFile, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d4596-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="d4596-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d4596-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d4596-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d4596-143">**[Vytvoření zkušebního uživatele Citrix ShareFile](#create-a-citrix-sharefile-test-user)**  – Pokud chcete mít protějšek Britta Simon v Citrix ShareFile, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="d4596-143">**[Create a Citrix ShareFile test user](#create-a-citrix-sharefile-test-user)** - to have a counterpart of Britta Simon in Citrix ShareFile that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d4596-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d4596-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d4596-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d4596-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="d4596-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d4596-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="d4596-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Citrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="d4596-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Citrix ShareFile application.</span></span>

<span data-ttu-id="d4596-148">**Ke konfiguraci Azure AD jednotné přihlašování s Citrix ShareFile, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d4596-148">**To configure Azure AD single sign-on with Citrix ShareFile, perform the following steps:**</span></span>

1. <span data-ttu-id="d4596-149">Na portálu Azure na **Citrix ShareFile** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d4596-149">In the Azure portal, on the **Citrix ShareFile** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="d4596-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d4596-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_samlbase.png)

3. <span data-ttu-id="d4596-153">Na **Citrix ShareFile domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d4596-153">On the **Citrix ShareFile Domain and URLs** section, perform the following steps:</span></span>

    ![Citrix ShareFile domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_url.png)
    
    <span data-ttu-id="d4596-155">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant-name>.sharefile.com/saml/login`</span><span class="sxs-lookup"><span data-stu-id="d4596-155">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.sharefile.com/saml/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d4596-156">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="d4596-156">This value is not real.</span></span> <span data-ttu-id="d4596-157">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d4596-157">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="d4596-158">Obraťte se na [tým podpory Citrix ShareFile klienta](https://www.citrix.co.in/products/sharefile/support.html) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d4596-158">Contact [Citrix ShareFile Client support team](https://www.citrix.co.in/products/sharefile/support.html) to get this value.</span></span> 

4. <span data-ttu-id="d4596-159">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="d4596-159">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_certificate.png) 

5. <span data-ttu-id="d4596-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d4596-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-sharefile-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d4596-163">Na **ShareFile konfigurace systému Citrix** klikněte na tlačítko **konfigurace Citrix ShareFile** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="d4596-163">On the **Citrix ShareFile Configuration** section, click **Configure Citrix ShareFile** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d4596-164">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="d4596-164">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace systému Citrix ShareFile](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_configure.png) 

7. <span data-ttu-id="d4596-166">V okně prohlížeče jiný web, přihlaste se k vaší **Citrix ShareFile** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="d4596-166">In a different web browser window, log into your **Citrix ShareFile** company site as an administrator.</span></span>

8. <span data-ttu-id="d4596-167">Na panelu nástrojů v horní části klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="d4596-167">In the toolbar on the top, click **Admin**.</span></span>

9. <span data-ttu-id="d4596-168">V levém navigačním podokně, vyberte **nakonfigurovat jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d4596-168">In the left navigation pane, select **Configure Single Sign-On**.</span></span>
   
    <span data-ttu-id="d4596-169">![Účet správy](./media/active-directory-saas-sharefile-tutorial/ic773627.png "účet správy")</span><span class="sxs-lookup"><span data-stu-id="d4596-169">![Account Administration](./media/active-directory-saas-sharefile-tutorial/ic773627.png "Account Administration")</span></span>

10. <span data-ttu-id="d4596-170">Na **jednotného přihlašování nebo konfigurace SAML 2.0** dialogové okno stránky v rámci **základní nastavení**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d4596-170">On the **Single Sign-On/ SAML 2.0 Configuration** dialog page under **Basic Settings**, perform the following steps:</span></span>
   
    <span data-ttu-id="d4596-171">![Jednotné přihlašování](./media/active-directory-saas-sharefile-tutorial/ic773628.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="d4596-171">![Single sign-on](./media/active-directory-saas-sharefile-tutorial/ic773628.png "Single sign-on")</span></span>
   
    <span data-ttu-id="d4596-172">a.</span><span class="sxs-lookup"><span data-stu-id="d4596-172">a.</span></span> <span data-ttu-id="d4596-173">Klikněte na tlačítko **povolit SAML**.</span><span class="sxs-lookup"><span data-stu-id="d4596-173">Click **Enable SAML**.</span></span>
    
    <span data-ttu-id="d4596-174">b.</span><span class="sxs-lookup"><span data-stu-id="d4596-174">b.</span></span> <span data-ttu-id="d4596-175">V **vaše Vystavitel deklarací identity nebo Entity ID** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d4596-175">In **Your IDP Issuer/ Entity ID** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d4596-176">c.</span><span class="sxs-lookup"><span data-stu-id="d4596-176">c.</span></span> <span data-ttu-id="d4596-177">Klikněte na tlačítko **změnu** vedle **certifikát X.509** pole a pak nahrajte certifikát jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d4596-177">Click **Change** next to the **X.509 Certificate** field and then upload the certificate you downloaded from the Azure portal.</span></span>
    
    <span data-ttu-id="d4596-178">d.</span><span class="sxs-lookup"><span data-stu-id="d4596-178">d.</span></span> <span data-ttu-id="d4596-179">V **přihlašovací adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d4596-179">In **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="d4596-180">e.</span><span class="sxs-lookup"><span data-stu-id="d4596-180">e.</span></span> <span data-ttu-id="d4596-181">V **adresy URL odhlašovací** textovému poli, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d4596-181">In **Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

11. <span data-ttu-id="d4596-182">Klikněte na tlačítko **Uložit** na portálu pro správu Citrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="d4596-182">Click **Save** on the Citrix ShareFile management portal.</span></span>

> [!TIP]
> <span data-ttu-id="d4596-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="d4596-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d4596-184">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="d4596-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d4596-185">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d4596-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="d4596-186">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d4596-186">Create an Azure AD test user</span></span>

<span data-ttu-id="d4596-187">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d4596-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="d4596-189">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d4596-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d4596-190">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d4596-190">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-sharefile-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="d4596-192">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d4596-192">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-sharefile-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="d4596-194">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d4596-194">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-sharefile-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="d4596-196">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d4596-196">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-sharefile-tutorial/create_aaduser_04.png)

    <span data-ttu-id="d4596-198">a.</span><span class="sxs-lookup"><span data-stu-id="d4596-198">a.</span></span> <span data-ttu-id="d4596-199">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d4596-199">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d4596-200">b.</span><span class="sxs-lookup"><span data-stu-id="d4596-200">b.</span></span> <span data-ttu-id="d4596-201">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d4596-201">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="d4596-202">c.</span><span class="sxs-lookup"><span data-stu-id="d4596-202">c.</span></span> <span data-ttu-id="d4596-203">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="d4596-203">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="d4596-204">d.</span><span class="sxs-lookup"><span data-stu-id="d4596-204">d.</span></span> <span data-ttu-id="d4596-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d4596-205">Click **Create**.</span></span>
 
### <a name="create-a-citrix-sharefile-test-user"></a><span data-ttu-id="d4596-206">Vytvoření zkušebního uživatele Citrix ShareFile</span><span class="sxs-lookup"><span data-stu-id="d4596-206">Create a Citrix ShareFile test user</span></span>

<span data-ttu-id="d4596-207">Pokud chcete povolit uživatelům Azure AD přihlášení do systému Citrix ShareFile, musí být zřízená do Citrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="d4596-207">In order to enable Azure AD users to log into Citrix ShareFile, they must be provisioned into Citrix ShareFile.</span></span> <span data-ttu-id="d4596-208">V případě Citrix ShareFile zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="d4596-208">In the case of Citrix ShareFile, provisioning is a manual task.</span></span>

<span data-ttu-id="d4596-209">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d4596-209">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="d4596-210">Přihlaste se k vaší **Citrix ShareFile** klienta.</span><span class="sxs-lookup"><span data-stu-id="d4596-210">Log in to your **Citrix ShareFile** tenant.</span></span>

2. <span data-ttu-id="d4596-211">Klikněte na tlačítko **Správa uživatelů \> spravovat uživatelé domovské \> + vytvořit zaměstnanec**.</span><span class="sxs-lookup"><span data-stu-id="d4596-211">Click **Manage Users \> Manage Users Home \> + Create Employee**.</span></span>
   
   <span data-ttu-id="d4596-212">![Vytvoření zaměstnance](./media/active-directory-saas-sharefile-tutorial/IC781050.png "vytvoření zaměstnance")</span><span class="sxs-lookup"><span data-stu-id="d4596-212">![Create Employee](./media/active-directory-saas-sharefile-tutorial/IC781050.png "Create Employee")</span></span>

3. <span data-ttu-id="d4596-213">Na **základní informace** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d4596-213">On the **Basic Information** section, perform below steps:</span></span>
   
   <span data-ttu-id="d4596-214">![Základní informace](./media/active-directory-saas-sharefile-tutorial/IC799951.png "základní informace")</span><span class="sxs-lookup"><span data-stu-id="d4596-214">![Basic Information](./media/active-directory-saas-sharefile-tutorial/IC799951.png "Basic Information")</span></span>
   
   <span data-ttu-id="d4596-215">a.</span><span class="sxs-lookup"><span data-stu-id="d4596-215">a.</span></span> <span data-ttu-id="d4596-216">V **e-mailovou adresu** textovému poli, zadejte e-mailovou adresu Britta Simon jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="d4596-216">In the **Email Address** textbox, type the email address of Britta Simon as **brittasimon@contoso.com**.</span></span>
   
   <span data-ttu-id="d4596-217">b.</span><span class="sxs-lookup"><span data-stu-id="d4596-217">b.</span></span> <span data-ttu-id="d4596-218">V **křestní jméno** textovému poli, typ **křestní jméno** uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="d4596-218">In the **First Name** textbox, type **first name** of user as **Britta**.</span></span>
   
   <span data-ttu-id="d4596-219">c.</span><span class="sxs-lookup"><span data-stu-id="d4596-219">c.</span></span> <span data-ttu-id="d4596-220">V **příjmení** textovému poli, typ **příjmení** uživatele jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="d4596-220">In the **Last Name** textbox, type **last name** of user as **Simon**.</span></span>

4. <span data-ttu-id="d4596-221">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="d4596-221">Click **Add User**.</span></span>
  
   >[!NOTE]
   ><span data-ttu-id="d4596-222">Držitel účtu Azure AD budou dostávat e-mailu a postupujte podle odkaz potvrďte svůj účet, pak se změní na aktivní. Další nástroje pro tvorbu účet uživatele systému Citrix ShareFile nebo rozhraní API poskytovaných Citrix ShareFile můžete použít ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d4596-222">The Azure AD account holder will receive an email and follow a link to confirm their account before it becomes active.You can use any other Citrix ShareFile user account creation tools or APIs provided by Citrix ShareFile to provision Azure AD user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="d4596-223">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d4596-223">Assign the Azure AD test user</span></span>

<span data-ttu-id="d4596-224">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Citrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="d4596-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Citrix ShareFile.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="d4596-226">**Pokud chcete přiřadit Britta Simon Citrix ShareFile, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d4596-226">**To assign Britta Simon to Citrix ShareFile, perform the following steps:**</span></span>

1. <span data-ttu-id="d4596-227">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d4596-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d4596-229">V seznamu aplikací vyberte **Citrix ShareFile**.</span><span class="sxs-lookup"><span data-stu-id="d4596-229">In the applications list, select **Citrix ShareFile**.</span></span>

    ![V seznamu aplikací na odkaz Citrix ShareFile](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_app.png)  

3. <span data-ttu-id="d4596-231">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d4596-231">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="d4596-233">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d4596-233">Click **Add** button.</span></span> <span data-ttu-id="d4596-234">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d4596-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="d4596-236">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d4596-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d4596-237">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d4596-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d4596-238">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d4596-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="d4596-239">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d4596-239">Test single sign-on</span></span>

<span data-ttu-id="d4596-240">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d4596-240">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d4596-241">Když kliknete na dlaždici Citrix ShareFile na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Citrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="d4596-241">When you click the Citrix ShareFile tile in the Access Panel, you should get automatically signed-on to your Citrix ShareFile application.</span></span>
<span data-ttu-id="d4596-242">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d4596-242">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d4596-243">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d4596-243">Additional resources</span></span>

* [<span data-ttu-id="d4596-244">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d4596-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d4596-245">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d4596-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_203.png

