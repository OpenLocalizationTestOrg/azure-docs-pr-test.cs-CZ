---
title: 'Kurz: Azure Active Directory integrace s M soubory | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a M soubory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4536fd49-3a65-4cff-9620-860904f726d0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 0f2682cf7cd3e11a5a7156938fbe9d4c7f541312
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-m-files"></a><span data-ttu-id="de1b3-103">Kurz: Azure Active Directory integrace s M – soubory</span><span class="sxs-lookup"><span data-stu-id="de1b3-103">Tutorial: Azure Active Directory integration with M-Files</span></span>

<span data-ttu-id="de1b3-104">V tomto kurzu zjistěte, jak integrovat M soubory s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="de1b3-104">In this tutorial, you learn how to integrate M-Files with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="de1b3-105">Integrace M soubory s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="de1b3-105">Integrating M-Files with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="de1b3-106">Můžete řídit ve službě Azure AD, který má přístup k souborům M</span><span class="sxs-lookup"><span data-stu-id="de1b3-106">You can control in Azure AD who has access to M-Files</span></span>
- <span data-ttu-id="de1b3-107">Můžete povolit uživatelům, aby automaticky získat přihlášeného k souborům M (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="de1b3-107">You can enable your users to automatically get signed-on to M-Files (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="de1b3-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="de1b3-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="de1b3-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="de1b3-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de1b3-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="de1b3-110">Prerequisites</span></span>

<span data-ttu-id="de1b3-111">Ke konfiguraci integrace služby Azure AD se soubory M, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="de1b3-111">To configure Azure AD integration with M-Files, you need the following items:</span></span>

- <span data-ttu-id="de1b3-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="de1b3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="de1b3-113">M soubory jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="de1b3-113">A M-Files single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="de1b3-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="de1b3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="de1b3-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="de1b3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="de1b3-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="de1b3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="de1b3-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="de1b3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="de1b3-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="de1b3-118">Scenario description</span></span>
<span data-ttu-id="de1b3-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="de1b3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="de1b3-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="de1b3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="de1b3-121">Přidávání M souborů z Galerie</span><span class="sxs-lookup"><span data-stu-id="de1b3-121">Adding M-Files from the gallery</span></span>
2. <span data-ttu-id="de1b3-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="de1b3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-m-files-from-the-gallery"></a><span data-ttu-id="de1b3-123">Přidávání M souborů z Galerie</span><span class="sxs-lookup"><span data-stu-id="de1b3-123">Adding M-Files from the gallery</span></span>
<span data-ttu-id="de1b3-124">Při konfiguraci integrace M souborů do služby Azure AD, potřebujete přidat M soubory z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="de1b3-124">To configure the integration of M-Files into Azure AD, you need to add M-Files from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="de1b3-125">**Pokud chcete přidat soubory M z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="de1b3-125">**To add M-Files from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="de1b3-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="de1b3-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="de1b3-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="de1b3-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="de1b3-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="de1b3-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="de1b3-133">Do vyhledávacího pole zadejte **M soubory**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-133">In the search box, type **M-Files**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_search.png)

5. <span data-ttu-id="de1b3-135">Na panelu výsledků vyberte **M soubory**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="de1b3-135">In the results panel, select **M-Files**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="de1b3-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="de1b3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="de1b3-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s M-soubory podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="de1b3-138">In this section, you configure and test Azure AD single sign-on with M-Files based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="de1b3-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v souborech M je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="de1b3-139">For single sign-on to work, Azure AD needs to know what the counterpart user in M-Files is to a user in Azure AD.</span></span> <span data-ttu-id="de1b3-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v souborech M musí navázat.</span><span class="sxs-lookup"><span data-stu-id="de1b3-140">In other words, a link relationship between an Azure AD user and the related user in M-Files needs to be established.</span></span>

<span data-ttu-id="de1b3-141">V souborech M, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="de1b3-141">In M-Files, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="de1b3-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování se soubory M, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="de1b3-142">To configure and test Azure AD single sign-on with M-Files, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="de1b3-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="de1b3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="de1b3-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="de1b3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="de1b3-145">**[Vytvoření zkušebního uživatele M soubory](#creating-a-m-files-test-user)**  – Pokud chcete mít protějšek Britta Simon v souborech M, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="de1b3-145">**[Creating a M-Files test user](#creating-a-m-files-test-user)** - to have a counterpart of Britta Simon in M-Files that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="de1b3-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="de1b3-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="de1b3-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="de1b3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="de1b3-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="de1b3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="de1b3-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci M soubory.</span><span class="sxs-lookup"><span data-stu-id="de1b3-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your M-Files application.</span></span>

<span data-ttu-id="de1b3-150">**Ke konfiguraci Azure AD jednotné přihlašování se soubory M, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="de1b3-150">**To configure Azure AD single sign-on with M-Files, perform the following steps:**</span></span>

1. <span data-ttu-id="de1b3-151">Na portálu Azure na **M soubory** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-151">In the Azure portal, on the **M-Files** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="de1b3-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="de1b3-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_samlbase.png)

3. <span data-ttu-id="de1b3-155">Na **M soubory domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="de1b3-155">On the **M-Files Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_url.png)

    <span data-ttu-id="de1b3-157">a.</span><span class="sxs-lookup"><span data-stu-id="de1b3-157">a.</span></span> <span data-ttu-id="de1b3-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`</span><span class="sxs-lookup"><span data-stu-id="de1b3-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`</span></span>

    <span data-ttu-id="de1b3-159">b.</span><span class="sxs-lookup"><span data-stu-id="de1b3-159">b.</span></span> <span data-ttu-id="de1b3-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenantname>.cloudvault.m-files.com`</span><span class="sxs-lookup"><span data-stu-id="de1b3-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenantname>.cloudvault.m-files.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="de1b3-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="de1b3-161">These values are not real.</span></span> <span data-ttu-id="de1b3-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="de1b3-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="de1b3-163">Obraťte se na [tým podpory M soubory klienta](mailto:support@m-files.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="de1b3-163">Contact [M-Files Client support team](mailto:support@m-files.com) to get these values.</span></span> 
 
4. <span data-ttu-id="de1b3-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="de1b3-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_certificate.png) 

5. <span data-ttu-id="de1b3-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="de1b3-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-m-files-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="de1b3-168">Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaše aplikace, obraťte se na [tým podpory M soubory](mailto:support@m-files.com) a poskytněte stažené Metadata.</span><span class="sxs-lookup"><span data-stu-id="de1b3-168">To get SSO configured for your application, contact [M-Files support team](mailto:support@m-files.com) and provide them the downloaded Metadata.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="de1b3-169">Pokud chcete nakonfigurovat jednotné přihlašování pro vás aplikace pracovní plochy M soubor, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="de1b3-169">Follow the next steps if you want to configure SSO for you M-File desktop application.</span></span> <span data-ttu-id="de1b3-170">Pokud chcete nakonfigurovat jednotné přihlašování pro verzi webové M soubory nejsou potřeba žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="de1b3-170">No extra steps are required if you only want to configure SSO for M-Files web version.</span></span>  

7. <span data-ttu-id="de1b3-171">Podle další kroky pro konfiguraci aplikace pracovní plochy M souboru k povolení přihlášení SSO s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="de1b3-171">Follow the next steps to configure the M-File desktop application to enable SSO with Azure AD.</span></span> <span data-ttu-id="de1b3-172">Chcete-li stáhnout soubory M, přejděte na [M soubory stáhnout](https://www.m-files.com/en/download-latest-version) stránky.</span><span class="sxs-lookup"><span data-stu-id="de1b3-172">To download M-Files, go to [M-Files download](https://www.m-files.com/en/download-latest-version) page.</span></span>

8. <span data-ttu-id="de1b3-173">Otevřete **nastavení plochy M soubory** okno.</span><span class="sxs-lookup"><span data-stu-id="de1b3-173">Open the **M-Files Desktop Settings** window.</span></span> <span data-ttu-id="de1b3-174">Potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-174">Then, click **Add**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_10.png)

9. <span data-ttu-id="de1b3-176">Na **vlastnosti připojení trezoru dokumentu** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="de1b3-176">On the **Document Vault Connection Properties** window, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_11.png)  

    <span data-ttu-id="de1b3-178">V rámci serveru tématu typ, hodnoty následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="de1b3-178">Under the Server section type, the values as follows:</span></span>  

    <span data-ttu-id="de1b3-179">a.</span><span class="sxs-lookup"><span data-stu-id="de1b3-179">a.</span></span> <span data-ttu-id="de1b3-180">Pro **název**, typ `<tenant-name>.cloudvault.m-files.com`.</span><span class="sxs-lookup"><span data-stu-id="de1b3-180">For **Name**, type `<tenant-name>.cloudvault.m-files.com`.</span></span> 
 
    <span data-ttu-id="de1b3-181">b.</span><span class="sxs-lookup"><span data-stu-id="de1b3-181">b.</span></span> <span data-ttu-id="de1b3-182">Pro **číslo portu**, typ **4466**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-182">For **Port Number**, type **4466**.</span></span> 

    <span data-ttu-id="de1b3-183">c.</span><span class="sxs-lookup"><span data-stu-id="de1b3-183">c.</span></span> <span data-ttu-id="de1b3-184">Pro **protokol**, vyberte **HTTPS**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-184">For **Protocol**, select **HTTPS**.</span></span> 

    <span data-ttu-id="de1b3-185">d.</span><span class="sxs-lookup"><span data-stu-id="de1b3-185">d.</span></span> <span data-ttu-id="de1b3-186">V **ověřování** pole, vyberte **konkrétní Windows uživatele**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-186">In the **Authentication** field, select **Specific Windows user**.</span></span> <span data-ttu-id="de1b3-187">Potom zobrazí se výzva s podpisový stránky.</span><span class="sxs-lookup"><span data-stu-id="de1b3-187">Then, you are prompted with a signing page.</span></span> <span data-ttu-id="de1b3-188">Vložte přihlašovací údaje Azure AD.</span><span class="sxs-lookup"><span data-stu-id="de1b3-188">Insert your Azure AD credentials.</span></span> 

    <span data-ttu-id="de1b3-189">e.</span><span class="sxs-lookup"><span data-stu-id="de1b3-189">e.</span></span> <span data-ttu-id="de1b3-190">Pro **úložiště na serveru**, vyberte odpovídající úložiště na serveru.</span><span class="sxs-lookup"><span data-stu-id="de1b3-190">For the **Vault on Server**,  select the corresponding vault on server.</span></span>
 
    <span data-ttu-id="de1b3-191">f.</span><span class="sxs-lookup"><span data-stu-id="de1b3-191">f.</span></span> <span data-ttu-id="de1b3-192">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-192">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="de1b3-193">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="de1b3-193">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="de1b3-194">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="de1b3-194">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="de1b3-195">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="de1b3-195">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="de1b3-196">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="de1b3-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="de1b3-197">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="de1b3-197">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="de1b3-199">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="de1b3-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="de1b3-200">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="de1b3-200">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="de1b3-202">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-202">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="de1b3-204">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="de1b3-204">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="de1b3-206">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="de1b3-206">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="de1b3-208">a.</span><span class="sxs-lookup"><span data-stu-id="de1b3-208">a.</span></span> <span data-ttu-id="de1b3-209">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-209">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="de1b3-210">b.</span><span class="sxs-lookup"><span data-stu-id="de1b3-210">b.</span></span> <span data-ttu-id="de1b3-211">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="de1b3-211">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="de1b3-212">c.</span><span class="sxs-lookup"><span data-stu-id="de1b3-212">c.</span></span> <span data-ttu-id="de1b3-213">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-213">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="de1b3-214">d.</span><span class="sxs-lookup"><span data-stu-id="de1b3-214">d.</span></span> <span data-ttu-id="de1b3-215">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-215">Click **Create**.</span></span>
 
### <a name="creating-a-m-files-test-user"></a><span data-ttu-id="de1b3-216">Vytvoření zkušebního uživatele M – soubory</span><span class="sxs-lookup"><span data-stu-id="de1b3-216">Creating a M-Files test user</span></span>

<span data-ttu-id="de1b3-217">Cílem této části je vytvoření uživatele volat Britta Simon v souborech M.</span><span class="sxs-lookup"><span data-stu-id="de1b3-217">The objective of this section is to create a user called Britta Simon in M-Files.</span></span> <span data-ttu-id="de1b3-218">Práce s [tým podpory M soubory](mailto:support@m-files.com) přidejte uživatele, v souborech M.</span><span class="sxs-lookup"><span data-stu-id="de1b3-218">Work with  [M-Files support team](mailto:support@m-files.com) to add the users in the M-Files.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="de1b3-219">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="de1b3-219">Assigning the Azure AD test user</span></span>

<span data-ttu-id="de1b3-220">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k souborům M.</span><span class="sxs-lookup"><span data-stu-id="de1b3-220">In this section, you enable Britta Simon to use Azure single sign-on by granting access to M-Files.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="de1b3-222">**Pokud chcete přiřadit Britta Simon M soubory, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="de1b3-222">**To assign Britta Simon to M-Files, perform the following steps:**</span></span>

1. <span data-ttu-id="de1b3-223">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-223">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="de1b3-225">V seznamu aplikací vyberte **M soubory**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-225">In the applications list, select **M-Files**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_app.png) 

3. <span data-ttu-id="de1b3-227">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="de1b3-227">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="de1b3-229">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="de1b3-229">Click **Add** button.</span></span> <span data-ttu-id="de1b3-230">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="de1b3-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="de1b3-232">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="de1b3-232">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="de1b3-233">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="de1b3-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="de1b3-234">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="de1b3-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="de1b3-235">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="de1b3-235">Testing single sign-on</span></span>

<span data-ttu-id="de1b3-236">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="de1b3-236">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="de1b3-237">Když kliknete na dlaždici M soubory na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci M soubory.</span><span class="sxs-lookup"><span data-stu-id="de1b3-237">When you click the M-Files tile in the Access Panel, you should get automatically signed-on to your M-Files application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="de1b3-238">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="de1b3-238">Additional resources</span></span>

* [<span data-ttu-id="de1b3-239">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="de1b3-239">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="de1b3-240">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="de1b3-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_203.png

