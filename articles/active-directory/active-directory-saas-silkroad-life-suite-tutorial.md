---
title: "Kurz: Azure Active Directory integrace s SilkRoad životnosti Suite | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SilkRoad životnosti Suite."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
ms.openlocfilehash: ecf4e31ecea00d003fc47ea4cebb781ca58957f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a><span data-ttu-id="90f56-103">Kurz: Azure Active Directory integrace s SilkRoad životnosti Suite</span><span class="sxs-lookup"><span data-stu-id="90f56-103">Tutorial: Azure Active Directory integration with SilkRoad Life Suite</span></span>
<span data-ttu-id="90f56-104">Cílem tohoto kurzu je ukazují, jak integrovat SilkRoad životnosti Suite s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="90f56-104">The objective of this tutorial is to show you how to integrate SilkRoad Life Suite with Azure Active Directory (Azure AD).</span></span> 

<span data-ttu-id="90f56-105">Integrace s Azure AD SilkRoad životnosti Suite nabízí následující výhody:</span><span class="sxs-lookup"><span data-stu-id="90f56-105">Integrating SilkRoad Life Suite with Azure AD provides you with the following benefits:</span></span> 

* <span data-ttu-id="90f56-106">Můžete řídit ve službě Azure AD, kdo má přístup k SilkRoad životnosti Suite</span><span class="sxs-lookup"><span data-stu-id="90f56-106">You can control in Azure AD who has access to SilkRoad Life Suite</span></span> 
* <span data-ttu-id="90f56-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k SilkRoad životnosti Suite jednotné přihlašování (SSO) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="90f56-107">You can enable your users to automatically get signed-on to SilkRoad Life Suite single sign-on (SSO) with their Azure AD accounts</span></span>

<span data-ttu-id="90f56-108">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="90f56-108">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90f56-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="90f56-109">Prerequisites</span></span>
<span data-ttu-id="90f56-110">Konfigurace Azure AD integrace se sadou Suite životnosti SilkRoad, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="90f56-110">To configure Azure AD integration with SilkRoad Life Suite, you need the following items:</span></span>

* <span data-ttu-id="90f56-111">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="90f56-111">An Azure AD subscription</span></span>
* <span data-ttu-id="90f56-112">Předplatné SilkRoad životnosti Suite jednotné přihlašování povoleno</span><span class="sxs-lookup"><span data-stu-id="90f56-112">A SilkRoad Life Suite SSO enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="90f56-113">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="90f56-113">To test the steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="90f56-114">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="90f56-114">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="90f56-115">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="90f56-115">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="90f56-116">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="90f56-116">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="90f56-117">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="90f56-117">Scenario Description</span></span>
<span data-ttu-id="90f56-118">Cílem tohoto kurzu je vám umožní Azure AD jednotného přihlašování k testování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="90f56-118">The objective of this tutorial is to enable you to test Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="90f56-119">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="90f56-119">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="90f56-120">Přidání SilkRoad životnosti Suite z Galerie</span><span class="sxs-lookup"><span data-stu-id="90f56-120">Adding SilkRoad Life Suite from the gallery</span></span> 
2. <span data-ttu-id="90f56-121">Konfigurace a testování Azure AD SSO</span><span class="sxs-lookup"><span data-stu-id="90f56-121">Configuring and testing Azure AD SSO</span></span>

## <a name="add-silkroad-life-suite-from-the-gallery"></a><span data-ttu-id="90f56-122">Přidat Suite životnosti SilkRoad z Galerie</span><span class="sxs-lookup"><span data-stu-id="90f56-122">Add SilkRoad Life Suite from the gallery</span></span>
<span data-ttu-id="90f56-123">Při konfiguraci integrace sady životnosti SilkRoad do služby Azure AD, potřebujete přidat Suite životnosti SilkRoad z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="90f56-123">To configure the integration of SilkRoad Life Suite into Azure AD, you need to add SilkRoad Life Suite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="90f56-124">**Pokud chcete přidat Suite životnosti SilkRoad z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="90f56-124">**To add SilkRoad Life Suite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="90f56-125">V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="90f56-125">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="90f56-127">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="90f56-127">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="90f56-128">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="90f56-128">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Aplikace][2]

4. <span data-ttu-id="90f56-130">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="90f56-130">Click **Add** at the bottom of the page.</span></span>
   
    ![Aplikace][3]

5. <span data-ttu-id="90f56-132">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="90f56-132">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Aplikace][4]

6. <span data-ttu-id="90f56-134">Do vyhledávacího pole zadejte **SilkRoad životnosti Suite**.</span><span class="sxs-lookup"><span data-stu-id="90f56-134">In the search box, type **SilkRoad Life Suite**.</span></span>
   
    ![Aplikace][5]

7. <span data-ttu-id="90f56-136">V podokně výsledků vyberte **Suite životnosti SilkRoad**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="90f56-136">In the results pane, select **SilkRoad Life Suite**, and then click **Complete** to add the application.</span></span>
   
    ![Aplikace][50]

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="90f56-138">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="90f56-138">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="90f56-139">Cílem této části je ukazují, jak nakonfigurovat a otestovat Azure AD přihlášení SSO se sada životnosti SilkRoad podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="90f56-139">The objective of this section is to show you how to configure and test Azure AD SSO with SilkRoad Life Suite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="90f56-140">Pro jednotné přihlašování pro práci je potřeba vědět, co uživatel protějškem v sadě životnosti SilkRoad pro uživatele ve službě Azure AD je Azure AD.</span><span class="sxs-lookup"><span data-stu-id="90f56-140">For SSO to work, Azure AD needs to know what the counterpart user in SilkRoad Life Suite to an user in Azure AD is.</span></span> <span data-ttu-id="90f56-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v sadě životnosti SilkRoad musí navázat.</span><span class="sxs-lookup"><span data-stu-id="90f56-141">In other words, a link relationship between an Azure AD user and the related user in SilkRoad Life Suite needs to be established.</span></span>

<span data-ttu-id="90f56-142">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v sadě SilkRoad životnosti.</span><span class="sxs-lookup"><span data-stu-id="90f56-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SilkRoad Life Suite.</span></span>

<span data-ttu-id="90f56-143">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SilkRoad životnosti Suite, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="90f56-143">To configure and test Azure AD single sign-on with SilkRoad Life Suite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="90f56-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="90f56-144">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="90f56-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="90f56-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="90f56-146">**[Vytvoření zkušebního uživatele Suite životnosti SilkRoad](#creating-a-silkroad-life-suite-test-user)**  – Pokud chcete mít protějšek Britta Simon životnosti sady SilkRoad, který je propojený s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="90f56-146">**[Creating a SilkRoad Life Suite test user](#creating-a-silkroad-life-suite-test-user)** - to have a counterpart of Britta Simon in SilkRoad Life Suite that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="90f56-147">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="90f56-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="90f56-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="90f56-148">**[Testing single sign-on](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="90f56-149">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="90f56-149">Configure Azure AD single sign-on</span></span>
<span data-ttu-id="90f56-150">Cílem této části je k povolení přihlášení SSO Azure AD na portálu Azure classic a nakonfigurovat jednotné přihlašování v aplikaci SilkRoad životnosti Suite.</span><span class="sxs-lookup"><span data-stu-id="90f56-150">The objective of this section is to enable Azure AD SSO in the Azure classic portal and to configure SSO in your SilkRoad Life Suite application.</span></span>

<span data-ttu-id="90f56-151">**Ke konfiguraci Azure AD jednotné přihlašování s SilkRoad životnosti Suite, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="90f56-151">**To configure Azure AD single sign-on with SilkRoad Life Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="90f56-152">Přihlašování k webu společnosti SilkRoad jako správce.</span><span class="sxs-lookup"><span data-stu-id="90f56-152">Sign-on to your SilkRoad company site as administrator.</span></span> 

  >[!NOTE] 
  > <span data-ttu-id="90f56-153">Pokud chcete získat přístup k aplikaci SilkRoad životnosti Suite ověřování pro konfiguraci federační služby Microsoft Azure AD, kontaktujte prosím podporu SilkRoad nebo vaším zástupcem SilkRoad služby.</span><span class="sxs-lookup"><span data-stu-id="90f56-153">To obtain access to the SilkRoad Life Suite Authentication application for configuring federation with Microsoft Azure AD, please contact SilkRoad Support or your SilkRoad Services representative.</span></span>
  > 

2. <span data-ttu-id="90f56-154">Přejděte na **poskytovatele služeb**a potom klikněte na **Federation podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="90f56-154">Go to **Service Provider**, and then click **Federation Details**.</span></span> 
   
    ![Azure AD jednotné přihlášení][10] 

3. <span data-ttu-id="90f56-156">Klikněte na tlačítko **stáhnout federačních metadat**a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="90f56-156">Click **Download Federation Metadata**, and then save the metadata file on your computer.</span></span>
   
    ![Azure AD jednotné přihlášení][11] 

4. <span data-ttu-id="90f56-158">Na portálu Azure classic na **Suite životnosti SilkRoad** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="90f56-158">In the Azure classic portal, on the **SilkRoad Life Suite** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurovat jednotné přihlašování][6] 

5. <span data-ttu-id="90f56-160">Na **jak chcete uživatelům se přihlásit SilkRoad životnosti Suite** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="90f56-160">On the **How would you like users to sign on to SilkRoad Life Suite** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Azure AD jednotné přihlášení][7] 

6. <span data-ttu-id="90f56-162">Na **nakonfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="90f56-162">On the **Configure App Settings** dialog page, perform the following steps:</span></span>
   
    ![Azure AD jednotné přihlášení][8]   
 1. <span data-ttu-id="90f56-164">V **přihlašovací adresa URL** textové pole, zadejte adresu URL používá uživatelům přihlašování na váš web Suite životnosti SilkRoad (např: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).</span><span class="sxs-lookup"><span data-stu-id="90f56-164">In the **Sign On URL** textbox, type the URL used by your users to sign-on to your SilkRoad Life Suite site (e.g.: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).</span></span>  
 2. <span data-ttu-id="90f56-165">Otevřete stažené **Silkroad** soubor metadat.</span><span class="sxs-lookup"><span data-stu-id="90f56-165">Open the downloaded **Silkroad** metadata file.</span></span> 
 3. <span data-ttu-id="90f56-166">Vyhledejte **AssertionConsumerService** označení a poté zkopírujte **umístění** atribut.</span><span class="sxs-lookup"><span data-stu-id="90f56-166">Locate the **AssertionConsumerService** tag, and then copy the **Location** attribute.</span></span>         
   
    ![Azure AD jednotné přihlášení][21] 
 4. <span data-ttu-id="90f56-168">Vložte hodnotu do **adresa URL odpovědi** textové pole.</span><span class="sxs-lookup"><span data-stu-id="90f56-168">Paste the value into the **Reply URL** textbox.</span></span>  
 5. <span data-ttu-id="90f56-169">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="90f56-169">Click **Next**.</span></span>

6. <span data-ttu-id="90f56-170">Na **nakonfigurovat jednotné přihlašování v SilkRoad životnosti Suite** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="90f56-170">On the **Configure single sign-on at SilkRoad Life Suite** page, perform the following steps:</span></span>
   
    ![Azure AD jednotné přihlášení][9]  
 1. <span data-ttu-id="90f56-172">Klikněte na možnost stažení certifikátu a potom uložte soubor v počítači.</span><span class="sxs-lookup"><span data-stu-id="90f56-172">Click Download certificate, and then save the file on your computer.</span></span>  
 2. <span data-ttu-id="90f56-173">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="90f56-173">Click **Next**.</span></span>

7. <span data-ttu-id="90f56-174">Ve vaší **SilkRoad** aplikace, klikněte na tlačítko **ověřování zdrojů**.</span><span class="sxs-lookup"><span data-stu-id="90f56-174">In your **SilkRoad** application, click **Authentication Sources**.</span></span>
   
    ![Azure AD jednotné přihlášení][12] 

8. <span data-ttu-id="90f56-176">Klikněte na tlačítko **přidat zdroj ověřování**.</span><span class="sxs-lookup"><span data-stu-id="90f56-176">Click **Add Authentication Source**.</span></span> 
   
    ![Azure AD jednotné přihlášení][13] 

9. <span data-ttu-id="90f56-178">V **přidat zdroj ověřování** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="90f56-178">In the **Add Authentication Source** section, perform the following steps:</span></span> 
   
    ![Azure AD jednotné přihlášení][14]  
 1. <span data-ttu-id="90f56-180">V části **možnost 2 – soubor metadat**, klikněte na tlačítko **Procházet** nahrát soubor stažený metadat.</span><span class="sxs-lookup"><span data-stu-id="90f56-180">Under **Option 2 - Metadata File**, click **Browse** to upload the downloaded metadata file.</span></span>  
 2. <span data-ttu-id="90f56-181">Klikněte na tlačítko **vytvořit zprostředkovatelů Identity pomocí dat souboru**.</span><span class="sxs-lookup"><span data-stu-id="90f56-181">Click **Create Identity Provider using File Data**.</span></span>

10. <span data-ttu-id="90f56-182">V **ověřování zdrojů** klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="90f56-182">In the **Authentication Sources** section, click **Edit**.</span></span> 
    
     ![Azure AD jednotné přihlášení][15] 

11. <span data-ttu-id="90f56-184">Na **upravit zdroj ověřování** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="90f56-184">On the **Edit Authentication Source** dialog, perform the following steps:</span></span> 
    
     ![Azure AD jednotné přihlášení][16] 
 1. <span data-ttu-id="90f56-186">Jako **povoleno**, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="90f56-186">As **Enabled**, select **Yes**.</span></span>   
 2. <span data-ttu-id="90f56-187">V **IdP popis** textovému poli, zadejte popis pro konfiguraci (například: *Azure AD jednotného přihlašování k*).</span><span class="sxs-lookup"><span data-stu-id="90f56-187">In the **IdP Description** textbox, type a description for your configuration (e.g.: *Azure AD SSO*).</span></span>  
 3. <span data-ttu-id="90f56-188">V **IdP název** textovému poli, zadejte název, která je specifická pro konfiguraci (například: *Azure SP*).</span><span class="sxs-lookup"><span data-stu-id="90f56-188">In the **IdP Name** textbox, type a name that is specific to your configuration (e.g.: *Azure SP*).</span></span>  
 4. <span data-ttu-id="90f56-189">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="90f56-189">Click **Save**.</span></span>

12. <span data-ttu-id="90f56-190">Zakažte všechny ostatní zdroje ověřování.</span><span class="sxs-lookup"><span data-stu-id="90f56-190">Disable all other authentication sources.</span></span> 
    
     ![Azure AD jednotné přihlášení][17]

13. <span data-ttu-id="90f56-192">Na portálu Azure classic na **jednotné přihlašování potvrzení** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="90f56-192">In the Azure classic portal, on the **Single sign-on confirmation** page, click **Next**.</span></span>  
    
     ![Azure AD jednotné přihlášení][18]

14. <span data-ttu-id="90f56-194">Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="90f56-194">On the **Single sign-on confirmation** page, click **Complete**.</span></span>
    
     ![Azure AD jednotné přihlášení][19]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="90f56-196">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="90f56-196">Create an Azure AD test user</span></span>
<span data-ttu-id="90f56-197">Cílem této části je vytvoření zkušebního uživatele na portálu Azure classic názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="90f56-197">The objective of this section is to create a test user in the Azure classic portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][20]

<span data-ttu-id="90f56-199">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="90f56-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="90f56-200">V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="90f56-200">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)  

2. <span data-ttu-id="90f56-202">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="90f56-202">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="90f56-203">Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="90f56-203">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="90f56-205">Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="90f56-205">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="90f56-207">Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="90f56-207">On the **Tell us about this user** dialog page, perform the following steps:</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)  
 1. <span data-ttu-id="90f56-209">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="90f56-209">As Type Of User, select New user in your organization.</span></span>  
 2. <span data-ttu-id="90f56-210">V uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="90f56-210">In the User Name **textbox**, type **BrittaSimon**.</span></span> 
 3. <span data-ttu-id="90f56-211">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="90f56-211">Click **Next**.</span></span>

6. <span data-ttu-id="90f56-212">Na **profil uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="90f56-212">On the **User Profile** dialog page, perform the following steps:</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png)  
 1. <span data-ttu-id="90f56-214">V **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="90f56-214">In the **First Name** textbox, type **Britta**.</span></span>    
 2. <span data-ttu-id="90f56-215">V **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="90f56-215">In the **Last Name** textbox, type, **Simon**.</span></span> 
 3. <span data-ttu-id="90f56-216">V **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="90f56-216">In the **Display Name** textbox, type **Britta Simon**.</span></span> 
 4. <span data-ttu-id="90f56-217">V **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="90f56-217">In the **Role** list, select **User**.</span></span>
 5. <span data-ttu-id="90f56-218">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="90f56-218">Click **Next**.</span></span>

7. <span data-ttu-id="90f56-219">Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="90f56-219">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="90f56-221">Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="90f56-221">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png)  
 1. <span data-ttu-id="90f56-223">Poznamenejte si hodnotu **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="90f56-223">Write down the value of the **New Password**.</span></span> 
 2. <span data-ttu-id="90f56-224">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="90f56-224">Click **Complete**.</span></span>   

### <a name="create-a-silkroad-life-suite-test-user"></a><span data-ttu-id="90f56-225">Vytvoření zkušebního uživatele SilkRoad životnosti Suite</span><span class="sxs-lookup"><span data-stu-id="90f56-225">Create a SilkRoad Life Suite test user</span></span>
<span data-ttu-id="90f56-226">Cílem této části je vytvoření uživatele názvem Britta Simon v sadě SilkRoad životnosti.</span><span class="sxs-lookup"><span data-stu-id="90f56-226">The objective of this section is to create a user called Britta Simon in SilkRoad Life Suite.</span></span> <span data-ttu-id="90f56-227">Na Britta musí mít ID jednotné přihlašování (někdy označovány jako *AuthParam*) odpovídající na Britta **emailaddress** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="90f56-227">Britta's must have an SSO ID (sometimes referred to as an *AuthParam*) that matches Britta's **emailaddress** in Azure AD.</span></span>

<span data-ttu-id="90f56-228">**Vytvoření uživatele volat Britta Simon v sadě životnosti SilkRoad, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="90f56-228">**To create a user called Britta Simon in SilkRoad Life Suite, perform the following steps:**</span></span>

- <span data-ttu-id="90f56-229">Požádejte váš tým podpory SilkRoad životnosti Suite pro vytvoření uživatele, která má jako **jednotného přihlašování k ID** atribut stejnou hodnotu jako **emailaddress** z Britta Simon ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="90f56-229">Ask your SilkRoad Life Suite support team to create a user that has as **SSO ID** attribute the same value as the **emailaddress** of Britta Simon in Azure AD.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="90f56-230">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="90f56-230">Assign the Azure AD test user</span></span>
<span data-ttu-id="90f56-231">Cílem této části je umožnit Britta Simon používat jednotného přihlašování k Azure tak, že udělíte přístup k SilkRoad životnosti Suite.</span><span class="sxs-lookup"><span data-stu-id="90f56-231">The objective of this section is to enable Britta Simon to use Azure SSO by granting her access to SilkRoad Life Suite.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="90f56-233">**Pokud chcete přiřadit Britta Simon SilkRoad životnosti Suite, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="90f56-233">**To assign Britta Simon to SilkRoad Life Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="90f56-234">Na portálu Azure classic, otevřete zobrazení aplikací, v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="90f56-234">On the Azure classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="90f56-236">V seznamu aplikací vyberte **SilkRoad životnosti Suite**.</span><span class="sxs-lookup"><span data-stu-id="90f56-236">In the applications list, select **SilkRoad Life Suite**.</span></span>
   
    ![Přiřadit uživatele][202] 

3. <span data-ttu-id="90f56-238">V nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="90f56-238">In the menu on the top, click **Users**.</span></span>
   
    ![Přiřadit uživatele][203] 

4. <span data-ttu-id="90f56-240">V seznamu uživatelů vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="90f56-240">In the Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="90f56-241">Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="90f56-241">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Přiřadit uživatele][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="90f56-243">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="90f56-243">Test single sign-on</span></span>
<span data-ttu-id="90f56-244">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="90f56-244">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="90f56-245">Když kliknete na dlaždici SilkRoad životnosti Suite na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci SilkRoad životnosti Suite.</span><span class="sxs-lookup"><span data-stu-id="90f56-245">When you click the SilkRoad Life Suite tile in the Access Panel, you should get automatically signed-on to your SilkRoad Life Suite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="90f56-246">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="90f56-246">Additional Resources</span></span>
* [<span data-ttu-id="90f56-247">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="90f56-247">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="90f56-248">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="90f56-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png





