---
title: "aaaGet začít s Azure Notification Hubs pomocí Baidu | Microsoft Docs"
description: "V tomto kurzu zjistíte, jak toouse Azure Notification Hubs toopush oznámení tooAndroid zařízení pomocí Baidu."
services: notification-hubs
documentationcenter: android
author: ysxu
manager: erikre
editor: 
ms.assetid: 23bde1ea-f978-43b2-9eeb-bfd7b9edc4c1
ms.service: notification-hubs
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: mobile-baidu
ms.workload: mobile
ms.date: 08/19/2016
ms.author: yuaxu
ms.openlocfilehash: 2767fdd3bb04674e7a531634237cc05cd8c21cb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-notification-hubs-using-baidu"></a><span data-ttu-id="9b55a-103">Začínáme s použitím Notification Hubs pomocí Baidu</span><span class="sxs-lookup"><span data-stu-id="9b55a-103">Get started with Notification Hubs using Baidu</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="9b55a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="9b55a-104">Overview</span></span>
<span data-ttu-id="9b55a-105">Je cloudu baidu představuje čínskou cloudovou službu, které můžete použít toosend nabízená oznámení toomobile zařízení.</span><span class="sxs-lookup"><span data-stu-id="9b55a-105">Baidu cloud push is a Chinese cloud service that you can use toosend push notifications toomobile devices.</span></span> <span data-ttu-id="9b55a-106">Tato služba je užitečné v Číně, kde doručování nabízených oznámení, že tooAndroid je komplexní z důvodu hello přítomnosti různých obchodů s aplikacemi a nabízených služeb, kromě toohello dostupnosti zařízení Android, které nejsou obvykle připojené tooGCM (Google Cloud Messaging).</span><span class="sxs-lookup"><span data-stu-id="9b55a-106">This service is useful in China, where delivering push notifications tooAndroid is complex because of hello presence of different app stores and push services, in addition toohello availability of Android devices that are not typically connected tooGCM (Google Cloud Messaging).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9b55a-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9b55a-107">Prerequisites</span></span>
<span data-ttu-id="9b55a-108">V tomto kurzu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="9b55a-108">This tutorial requires:</span></span>

* <span data-ttu-id="9b55a-109">Sadu Android SDK (předpokládáme, že použijete Eclipse), kterou si můžete stáhnout z hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">lokality Android</a></span><span class="sxs-lookup"><span data-stu-id="9b55a-109">Android SDK (we assume that you use Eclipse), which you can download from hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android site</a></span></span>
* <span data-ttu-id="9b55a-110">[Mobile Services Android SDK]</span><span class="sxs-lookup"><span data-stu-id="9b55a-110">[Mobile Services Android SDK]</span></span>
* <span data-ttu-id="9b55a-111">[Baidu Push Android SDK]</span><span class="sxs-lookup"><span data-stu-id="9b55a-111">[Baidu Push Android SDK]</span></span>

> [!NOTE]
> <span data-ttu-id="9b55a-112">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="9b55a-112">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="9b55a-113">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="9b55a-113">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="9b55a-114">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="9b55a-114">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).</span></span>
> 
> 

## <a name="create-a-baidu-account"></a><span data-ttu-id="9b55a-115">Vytvořte účet Baidu</span><span class="sxs-lookup"><span data-stu-id="9b55a-115">Create a Baidu account</span></span>
<span data-ttu-id="9b55a-116">toouse Baidu, musíte mít účet Baidu.</span><span class="sxs-lookup"><span data-stu-id="9b55a-116">toouse Baidu, you must have a Baidu account.</span></span> <span data-ttu-id="9b55a-117">Pokud již účet máte, přihlaste se toohello [portál Baidu] a toohello další krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="9b55a-117">If you already have one, log in toohello [Baidu portal] and skip toohello next step.</span></span> <span data-ttu-id="9b55a-118">Jinak, najdete v části hello následující pokyny o tom, toocreate účet Baidu.</span><span class="sxs-lookup"><span data-stu-id="9b55a-118">Otherwise, see hello following instructions on how toocreate a Baidu account.</span></span>  

1. <span data-ttu-id="9b55a-119">Přejděte toohello [portál Baidu] a klikněte na tlačítko hello**登录**(**přihlášení**) odkaz.</span><span class="sxs-lookup"><span data-stu-id="9b55a-119">Go toohello [Baidu portal] and click hello **登录** (**Login**) link.</span></span> <span data-ttu-id="9b55a-120">Klikněte na tlačítko**立即注册**toostart hello účet registraci.</span><span class="sxs-lookup"><span data-stu-id="9b55a-120">Click **立即注册** toostart hello account registration process.</span></span>
   
   ![][1]
2. <span data-ttu-id="9b55a-121">Zadejte hello požadované podrobnosti – telefon nebo e-mailovou adresu, heslo a ověřovací kód – a klikněte na tlačítko **registrace**.</span><span class="sxs-lookup"><span data-stu-id="9b55a-121">Enter hello required details—phone/email address, password, and verification code—and click **Signup**.</span></span>
   
   ![][2]
3. <span data-ttu-id="9b55a-122">Vám bude zasláno e-mailovou toohello e-mailovou adresu, že jste zadali s tooactivate odkaz účet Baidu.</span><span class="sxs-lookup"><span data-stu-id="9b55a-122">You will be sent an email toohello email address that you entered with a link tooactivate your Baidu account.</span></span>
   
   ![][3]
4. <span data-ttu-id="9b55a-123">Přihlaste tooyour e-mailový účet, otevřete aktivaci baidu hello a klikněte na tlačítko tooactivate odkaz hello aktivaci účtu Baidu.</span><span class="sxs-lookup"><span data-stu-id="9b55a-123">Log in tooyour email account, open hello Baidu activation mail, and click hello activation link tooactivate your Baidu account.</span></span>
   
   ![][4]

<span data-ttu-id="9b55a-124">Po aktivaci účtu Baidu máte, přihlaste se toohello [portál Baidu].</span><span class="sxs-lookup"><span data-stu-id="9b55a-124">Once you have an activated Baidu account, log in toohello [Baidu portal].</span></span>

## <a name="register-as-a-baidu-developer"></a><span data-ttu-id="9b55a-125">Zaregistrujte se jako vývojář Baidu</span><span class="sxs-lookup"><span data-stu-id="9b55a-125">Register as a Baidu developer</span></span>
1. <span data-ttu-id="9b55a-126">Jakmile jste byli přihlášeni toohello [portál Baidu], klikněte na tlačítko**更多 >>** (**Další**).</span><span class="sxs-lookup"><span data-stu-id="9b55a-126">Once you have logged in toohello [Baidu portal], click **更多>>** (**more**).</span></span>
   
      ![][5]
2. <span data-ttu-id="9b55a-127">Přejděte dolů v hello**站长与开发者服务 (správce webového serveru a služby pro vývojáře)** části a klikněte na tlačítko**百度开放云平台**(**Baidu otevřete Cloudová platforma**).</span><span class="sxs-lookup"><span data-stu-id="9b55a-127">Scroll down in hello **站长与开发者服务 (Webmaster and Developer Services)** section and click **百度开放云平台** (**Baidu open cloud platform**).</span></span>
   
      ![][6]
3. <span data-ttu-id="9b55a-128">Na další stránku hello, klikněte na tlačítko**开发者服务**(**služby pro vývojáře**) v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="9b55a-128">On hello next page, click **开发者服务** (**Developer Services**) on hello top-right corner.</span></span>
   
      ![][7]
4. <span data-ttu-id="9b55a-129">Na další stránku hello, klikněte na tlačítko**注册开发者**(**zaregistrován vývojáři**) z nabídky hello v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="9b55a-129">On hello next page, click **注册开发者** (**Registered Developers**) from hello menu on hello top-right corner.</span></span>
   
      ![][8]
5. <span data-ttu-id="9b55a-130">Zadejte své jméno, popis a číslo mobilního telefonu pro příjem ověřovací textové zprávy a pak klikněte na tlačítko **送验证码** (**Odeslat ověřovací kód**).</span><span class="sxs-lookup"><span data-stu-id="9b55a-130">Enter your name, a description, and a mobile phone number for receiving a verification text message, and then click **送验证码** (**Send Verification Code**).</span></span> <span data-ttu-id="9b55a-131">Pro mezinárodní telefonní čísla budete potřebovat kód země hello tooenclose v závorkách.</span><span class="sxs-lookup"><span data-stu-id="9b55a-131">For international phone numbers, you need tooenclose hello country code in parentheses.</span></span> <span data-ttu-id="9b55a-132">Například pro USA se bude jednat o číslo **(1) 1234567890**.</span><span class="sxs-lookup"><span data-stu-id="9b55a-132">For example, for a United States number, it is **(1)1234567890**.</span></span>
   
      ![][9]
6. <span data-ttu-id="9b55a-133">Následně byste měli obdržet textovou zprávu s číslem ověření, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="9b55a-133">You should then receive a text message with a verification number, as shown in hello following example:</span></span>
   
      ![][10]
7. <span data-ttu-id="9b55a-134">Zadejte číslo ověření hello z uvítací zprávu v**验证码**(**potvrzovací kód**).</span><span class="sxs-lookup"><span data-stu-id="9b55a-134">Enter hello verification number from hello message in **验证码** (**Confirmation code**).</span></span>
8. <span data-ttu-id="9b55a-135">Nakonec dokončit registraci vývojáře hello přijetí smlouvy hello Baidu a kliknutím na**提交**(**odeslání**).</span><span class="sxs-lookup"><span data-stu-id="9b55a-135">Finally, complete hello developer registration by accepting hello Baidu agreement and clicking **提交** (**Submit**).</span></span> <span data-ttu-id="9b55a-136">Zobrazí se následující stránka při úspěšném dokončení registrace hello:</span><span class="sxs-lookup"><span data-stu-id="9b55a-136">You will see hello following page on successful completion of registration:</span></span>
   
      ![][11]

## <a name="create-a-baidu-cloud-push-project"></a><span data-ttu-id="9b55a-137">Vytvořte projekt nabízených oznámení cloudu Baidu</span><span class="sxs-lookup"><span data-stu-id="9b55a-137">Create a Baidu cloud push project</span></span>
<span data-ttu-id="9b55a-138">Při vytváření projektu nabízených oznámení cloudu Baidu obdržíte své ID aplikace, klíč rozhraní API a tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="9b55a-138">When you create a Baidu cloud push project, you receive your app ID, API key, and secret key.</span></span>

1. <span data-ttu-id="9b55a-139">Jakmile jste byli přihlášeni toohello [portál Baidu], klikněte na tlačítko**更多 >>** (**Další**).</span><span class="sxs-lookup"><span data-stu-id="9b55a-139">Once you have logged in toohello [Baidu portal], click **更多>>** (**more**).</span></span>
   
      ![][5]
2. <span data-ttu-id="9b55a-140">Přejděte dolů v hello**站长与开发者服务**(**správce webového serveru a služby pro vývojáře**) a klikněte na**百度开放云平台**(**Baidu otevřete Cloudová platforma**).</span><span class="sxs-lookup"><span data-stu-id="9b55a-140">Scroll down in hello **站长与开发者服务** (**Webmaster and Developer Services**) section and click **百度开放云平台** (**Baidu open cloud platform**).</span></span>
   
      ![][6]
3. <span data-ttu-id="9b55a-141">Na další stránku hello, klikněte na tlačítko**开发者服务**(**služby pro vývojáře**) v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="9b55a-141">On hello next page, click **开发者服务** (**Developer Services**) on hello top-right corner.</span></span>
   
      ![][7]
4. <span data-ttu-id="9b55a-142">Na další stránku hello, klikněte na tlačítko**云推送**(**cloudu Push**) z hello**云服务**(**cloudové služby**) oddílu.</span><span class="sxs-lookup"><span data-stu-id="9b55a-142">On hello next page, click **云推送** (**Cloud Push**) from hello **云服务** (**Cloud Services**) section.</span></span>
   
      ![][12]
5. <span data-ttu-id="9b55a-143">Jakmile jste vývojář registrovaný, zobrazí**管理控制台**(**konzoly pro správu**) v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="9b55a-143">Once you are a registered developer, you see **管理控制台** (**Management Console**) at hello top menu.</span></span> <span data-ttu-id="9b55a-144">Klikněte na tlačítko **开发者服务管理** (**Správa služeb pro vývojáře**).</span><span class="sxs-lookup"><span data-stu-id="9b55a-144">Click **开发者服务管理** (**Developers Service Management**).</span></span>
   
      ![][13]
6. <span data-ttu-id="9b55a-145">Na další stránku hello, klikněte na tlačítko**创建工程**(**vytvoření projektu**).</span><span class="sxs-lookup"><span data-stu-id="9b55a-145">On hello next page, click **创建工程** (**Create Project**).</span></span>
   
      ![][14]
7. <span data-ttu-id="9b55a-146">Zadejte název aplikace a klikněte na tlačítko **创建** (**Vytvořit**).</span><span class="sxs-lookup"><span data-stu-id="9b55a-146">Enter an application name and click **创建** (**Create**).</span></span>
   
      ![][15]
8. <span data-ttu-id="9b55a-147">Po úspěšném vytvoření projektu nabízených oznámení cloudu Baidu se zobrazí stránka s **AppID**, **klíčem rozhraní API** a **tajným klíčem**.</span><span class="sxs-lookup"><span data-stu-id="9b55a-147">Upon successful creation of a Baidu cloud push project, you see a page with **AppID**, **API Key**, and **Secret Key**.</span></span> <span data-ttu-id="9b55a-148">Poznamenejte si klíč hello rozhraní API a tajný klíč, který použijeme později.</span><span class="sxs-lookup"><span data-stu-id="9b55a-148">Make a note of hello API key and secret key, which we will use later.</span></span>
   
      ![][16]
9. <span data-ttu-id="9b55a-149">Konfigurace projektu hello nabízená oznámení kliknutím**云推送**(**cloudu nabízené**) v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="9b55a-149">Configure hello project for push notifications by clicking **云推送** (**Cloud Push**) on hello left pane.</span></span>
   
      ![][31]
10. <span data-ttu-id="9b55a-150">Na další stránku hello, klikněte na tlačítko hello**推送设置**(**Push nastavení**) tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9b55a-150">On hello next page, click hello **推送设置** (**Push settings**) button.</span></span>
    
    ![][32]  
11. <span data-ttu-id="9b55a-151">Na stránce konfigurace hello, přidejte název hello balíčku, který budete používat v projektu Android v hello**应用包名**(**balíčku aplikace**) pole a pak klikněte na**保存设置**() **Uložit**).</span><span class="sxs-lookup"><span data-stu-id="9b55a-151">On hello configuration page, add hello package name that you will be using in your Android project in hello **应用包名** (**Application package**) field, and then click **保存设置** (**Save**).</span></span>  
    
    ![][33]

<span data-ttu-id="9b55a-152">Zobrazí hello**保存成功!** zpráva (**Úspěšně uloženo!**) .</span><span class="sxs-lookup"><span data-stu-id="9b55a-152">You see hello **保存成功！** (**Successfully saved!**) message.</span></span>

## <a name="configure-your-notification-hub"></a><span data-ttu-id="9b55a-153">Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="9b55a-153">Configure your notification hub</span></span>
1. <span data-ttu-id="9b55a-154">Přihlaste se toohello [portálu Azure Classic]a potom klikněte na **+ nový** v hello dolní části obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="9b55a-154">Sign in toohello [Azure Classic Portal], and then click **+NEW** at hello bottom of hello screen.</span></span>
2. <span data-ttu-id="9b55a-155">Klikněte na tlačítko **aplikační služby**, klikněte na tlačítko **Sběrnice**, klikněte na tlačítko **Notification Hubs** a pak klikněte na tlačítko **Rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9b55a-155">Click **App Services**, click **Service Bus**, click **Notification Hub**, and then click **Quick Create**.</span></span>
3. <span data-ttu-id="9b55a-156">Zadejte název vaší **centra oznámení**, vyberte hello **oblast** a hello **Namespace** kde toto centrum oznámení se vytvoří a pak klikněte na tlačítko  **Vytvoření nového centra oznámení**.</span><span class="sxs-lookup"><span data-stu-id="9b55a-156">Provide a name for your **Notification Hub**, select hello **Region** and hello **Namespace** where this notification hub will be created, and then click **Create a New Notification Hub**.</span></span>  
   
      ![][17]
4. <span data-ttu-id="9b55a-157">Klikněte na tlačítko hello obor názvů, ve které jste vytvořili centrum oznámení a pak klikněte na **Notification Hubs** v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="9b55a-157">Click hello namespace in which you created your notification hub, and then click **Notification Hubs** at hello top.</span></span>
   
      ![][18]
5. <span data-ttu-id="9b55a-158">Vyberte hello centra oznámení, který jste vytvořili a pak klikněte na tlačítko **konfigurace** hello hlavní nabídce.</span><span class="sxs-lookup"><span data-stu-id="9b55a-158">Select hello notification hub that you created, and then click **Configure** from hello top menu.</span></span>
   
      ![][19]
6. <span data-ttu-id="9b55a-159">Projděte dolů toohello **nastavení oznámení baidu** a zadejte klíč hello rozhraní API a tajný klíč, který jste získali z konzoly Baidu hello dříve pro váš projekt nabízených oznámení cloudu Baidu.</span><span class="sxs-lookup"><span data-stu-id="9b55a-159">Scroll down toohello **baidu notification settings** section and enter hello API key and secret key that you obtained from hello Baidu console previously for your Baidu cloud push project.</span></span> <span data-ttu-id="9b55a-160">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9b55a-160">Click **Save**.</span></span>
   
      ![][20]
7. <span data-ttu-id="9b55a-161">Klikněte na tlačítko hello **řídicí panel** hello horní pro Centrum oznámení hello a pak klikněte **zobrazení připojovacího řetězce**.</span><span class="sxs-lookup"><span data-stu-id="9b55a-161">Click hello **Dashboard** tab at hello top for hello notification hub, and then click **View Connection String**.</span></span>
   
      ![][21]
8. <span data-ttu-id="9b55a-162">Poznamenejte si hello **DefaultListenSharedAccessSignature** a **DefaultFullSharedAccessSignature** z hello **informace o přístupovém připojení** okno.</span><span class="sxs-lookup"><span data-stu-id="9b55a-162">Make a note of hello **DefaultListenSharedAccessSignature** and **DefaultFullSharedAccessSignature** from hello **Access connection information** window.</span></span>
   
    ![][22]

## <a name="connect-your-app-toohello-notification-hub"></a><span data-ttu-id="9b55a-163">Připojit vaše Centrum oznámení toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="9b55a-163">Connect your app toohello notification hub</span></span>
1. <span data-ttu-id="9b55a-164">V Eclipse ADT vytvořte nový projekt Android (**Soubor** > **Nový** > **Projekt aplikace Android**).</span><span class="sxs-lookup"><span data-stu-id="9b55a-164">In Eclipse ADT, create a new Android project (**File** > **New** > **Android Application Project**).</span></span>
   
    ![][23]
2. <span data-ttu-id="9b55a-165">Zadejte **název aplikace** a ujistěte se, že hello **minimální požadované SDK** verze je nastaven příliš**rozhraní API 16: Android 4.1**.</span><span class="sxs-lookup"><span data-stu-id="9b55a-165">Enter an **Application Name** and ensure that hello **Minimum Required SDK** version is set too**API 16: Android 4.1**.</span></span>
   
    ![][24]
3. <span data-ttu-id="9b55a-166">Klikněte na tlačítko **Další** a pokračovat, dokud hello následující hello průvodce **vytvořit aktivity** se zobrazí v okně.</span><span class="sxs-lookup"><span data-stu-id="9b55a-166">Click **Next** and continue following hello wizard until hello **Create Activity** window appears.</span></span> <span data-ttu-id="9b55a-167">Ujistěte se, že **prázdné aktivity** je vybrané a nakonec vyberte možnost **Dokončit** toocreate nové aplikace pro Android.</span><span class="sxs-lookup"><span data-stu-id="9b55a-167">Make sure that **Blank Activity** is selected, and finally select **Finish** toocreate a new Android Application.</span></span>
   
    ![][25]
4. <span data-ttu-id="9b55a-168">Ujistěte se, že hello **cíl sestavení projektu** nastavena správně.</span><span class="sxs-lookup"><span data-stu-id="9b55a-168">Make sure that hello **Project Build Target** is set correctly.</span></span>
   
    ![][26]
5. <span data-ttu-id="9b55a-169">Stáhněte si soubor notification-hubs-0.4.jar hello ze hello **soubory** kartě hello [Notification-Hubs-Android-SDK na panelu koše](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4).</span><span class="sxs-lookup"><span data-stu-id="9b55a-169">Download hello notification-hubs-0.4.jar file from hello **Files** tab of hello [Notification-Hubs-Android-SDK on Bintray](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4).</span></span> <span data-ttu-id="9b55a-170">Přidejte soubor toohello hello **knihovny** složky projektu Eclipse a aktualizace hello *knihovny* složky.</span><span class="sxs-lookup"><span data-stu-id="9b55a-170">Add hello file toohello **libs** folder of your Eclipse project, and refresh hello *libs* folder.</span></span>
6. <span data-ttu-id="9b55a-171">Stáhněte a rozbalte hello [Baidu Push Android SDK], otevřete hello **knihovny** složku a potom kopie hello **pushservice-x.y.z** jar souborové služby a hello **armeabi**  &  **mips** složek v hello **knihovny** složky vaší aplikace Android.</span><span class="sxs-lookup"><span data-stu-id="9b55a-171">Download and unzip hello [Baidu Push Android SDK], open hello **libs** folder, and then copy hello **pushservice-x.y.z** jar file and hello **armeabi** & **mips** folders in hello **libs** folder of your Android application.</span></span>
7. <span data-ttu-id="9b55a-172">Otevřete hello **AndroidManifest.xml** souboru systémem Android projekt a přidejte hello oprávnění, které jsou vyžadované Baidu SDK hello.</span><span class="sxs-lookup"><span data-stu-id="9b55a-172">Open hello **AndroidManifest.xml** file of your Android project and add hello permissions that are required by hello Baidu SDK.</span></span>
   
        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />
8. <span data-ttu-id="9b55a-173">Přidat hello **android: name** vlastnost tooyour **aplikace** element v **AndroidManifest.xml**, ve které nahradíte *názevvašehoprojektu* (pro například **com.example.BaiduTest**).</span><span class="sxs-lookup"><span data-stu-id="9b55a-173">Add hello **android:name** property tooyour **application** element in **AndroidManifest.xml**, replacing *yourprojectname* (for example, **com.example.BaiduTest**).</span></span> <span data-ttu-id="9b55a-174">Ujistěte se, že tento název projektu odpovídá hello jeden, který jste nakonfigurovali v konzole Baidu hello.</span><span class="sxs-lookup"><span data-stu-id="9b55a-174">Make sure that this project name matches hello one that you configured in hello Baidu console.</span></span>
   
        <application android:name="yourprojectname.DemoApplication"
9. <span data-ttu-id="9b55a-175">Přidejte následující konfigurace v rámci elementu aplikace hello po hello hello **. MainActivity** prvku aktivity, nahraďte *názevvašehoprojektu* (například **com.example.BaiduTest**):</span><span class="sxs-lookup"><span data-stu-id="9b55a-175">Add hello following configuration within hello application element after hello **.MainActivity** activity element, replacing *yourprojectname* (for example, **com.example.BaiduTest**):</span></span>
   
        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>
   
        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>
10. <span data-ttu-id="9b55a-176">Přidejte novou třídu s názvem **ConfigurationSettings.java** toohello projektu.</span><span class="sxs-lookup"><span data-stu-id="9b55a-176">Add a new class called **ConfigurationSettings.java** toohello project.</span></span>
    
     ![][28]
    
     ![][29]
11. <span data-ttu-id="9b55a-177">Přidejte následující kód tooit hello:</span><span class="sxs-lookup"><span data-stu-id="9b55a-177">Add hello following code tooit:</span></span>
    
        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }
    
    <span data-ttu-id="9b55a-178">Nastavte hodnotu hello **API_KEY** s co jste získali z projektu cloudu Baidu hello dříve, **NotificationHubName** s vaším jménem centra oznámení z hello portálu Azure Classic a  **NotificationHubConnectionString** pomocí DefaultListenSharedAccessSignature z portálu Azure Classic hello.</span><span class="sxs-lookup"><span data-stu-id="9b55a-178">Set hello value of **API_KEY** with what you retrieved from hello Baidu cloud project earlier, **NotificationHubName** with your notification hub name from hello Azure Classic Portal and **NotificationHubConnectionString** with DefaultListenSharedAccessSignature from hello Azure Classic Portal.</span></span>
12. <span data-ttu-id="9b55a-179">Přidejte novou třídu s názvem **DemoApplication.java**a přidejte následující kód tooit hello:</span><span class="sxs-lookup"><span data-stu-id="9b55a-179">Add a new class called **DemoApplication.java**, and add hello following code tooit:</span></span>
    
        import com.baidu.frontia.FrontiaApplication;
    
        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }
13. <span data-ttu-id="9b55a-180">Přidejte další novou třídu s názvem **MyPushMessageReceiver.java**a přidejte následující kód tooit hello.</span><span class="sxs-lookup"><span data-stu-id="9b55a-180">Add another new class called **MyPushMessageReceiver.java**, and add hello following code tooit.</span></span> <span data-ttu-id="9b55a-181">Je hello třídu, která zpracovává hello nabízená oznámení, které jsou přijaty ze serveru nabízených oznámení Baidu hello.</span><span class="sxs-lookup"><span data-stu-id="9b55a-181">It is hello class that handles hello push notifications that are received from hello Baidu push server.</span></span>
    
        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;
    
        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG tooLog */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();
    
            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;
    
                try {
                    if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }
    
                registerWithNotificationHubs();
            }
    
            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                 + ConfigurationSettings.NotificationHubName + "'"
                                 + " with UserId - '"
                                 + mUserId + "' and Channel Id - '"
                                 + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }
    
            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }
    
            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }
14. <span data-ttu-id="9b55a-182">Otevřete **MainActivity.java**a přidejte následující toohello hello **onCreate** metoda:</span><span class="sxs-lookup"><span data-stu-id="9b55a-182">Open **MainActivity.java**, and add hello following toohello **onCreate** method:</span></span>
    
            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);
15. <span data-ttu-id="9b55a-183">Otevřete následující prohlášení importu v horní části hello hello:</span><span class="sxs-lookup"><span data-stu-id="9b55a-183">Open hello following import statements at hello top:</span></span>
    
            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

## <a name="send-notifications-tooyour-app"></a><span data-ttu-id="9b55a-184">Odeslat oznámení tooyour aplikace</span><span class="sxs-lookup"><span data-stu-id="9b55a-184">Send notifications tooyour app</span></span>
<span data-ttu-id="9b55a-185">Můžete rychle otestovat příjem oznámení ve vaší aplikaci odesláním oznámení hello [portál Azure](https://portal.azure.com/) pomocí hello **odeslat** tlačítko hello centra oznámení, jak je znázorněno v hello následující obrazovka:</span><span class="sxs-lookup"><span data-stu-id="9b55a-185">You can quickly test receiving notifications in your app by sending notifications in hello [Azure portal](https://portal.azure.com/) using hello **Send** button on hello notification hub, as shown in hello following screen:</span></span>

![](./media/notification-hubs-baidu-get-started/notification-hub-test-send-baidu.png)

<span data-ttu-id="9b55a-186">Nabízená oznámení se většinou posílají ve službě back-end, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny.</span><span class="sxs-lookup"><span data-stu-id="9b55a-186">Push notifications are normally sent in a back-end service like Mobile Services or ASP.NET using a compatible library.</span></span> <span data-ttu-id="9b55a-187">Pokud není žádná knihovna k dispozici pro back-end, můžete použít hello REST API přímo toosend zpráv s oznámením.</span><span class="sxs-lookup"><span data-stu-id="9b55a-187">If a library is not available for your back-end, you can use hello REST API directly toosend notification messages .</span></span>

<span data-ttu-id="9b55a-188">V tomto kurzu jsme dělat nic složitého a jednoduše předvedeme testování vaší klientské aplikace pomocí odesílání oznámení hello .NET SDK pro centra oznámení v konzolové aplikaci, místo back-end službu.</span><span class="sxs-lookup"><span data-stu-id="9b55a-188">In this tutorial, we keep it simple and just demonstrate testing your client app by sending notifications using hello .NET SDK for notification hubs in a console application instead of a backend service.</span></span> <span data-ttu-id="9b55a-189">Doporučujeme, abyste hello [použití centra oznámení toopush oznámení toousers](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) kurz jako hello další krok pro odesílání oznámení z backendu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9b55a-189">We recommend hello [Use Notification Hubs toopush notifications toousers](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) tutorial as hello next step for sending notifications from an ASP.NET backend.</span></span> <span data-ttu-id="9b55a-190">Hello následující přístupy lze však použít pro zasílání oznámení:</span><span class="sxs-lookup"><span data-stu-id="9b55a-190">However, hello following approaches can be used for sending notifications:</span></span>

* <span data-ttu-id="9b55a-191">**Rozhraní REST**: oznámení můžete podporovat na jakékoli platformě back-end pomocí hello [rozhraní REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="9b55a-191">**REST Interface**:  You can support notification on any backend platform using  hello [REST interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span></span>
* <span data-ttu-id="9b55a-192">**Microsoft Azure oznámení centra .NET SDK**: hello Správce balíčků Nuget pro Visual Studio, spusťte [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="9b55a-192">**Microsoft Azure Notification Hubs .NET SDK**: In hello Nuget Package Manager for Visual Studio, run [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>
* <span data-ttu-id="9b55a-193">**Node.js**: [jak toouse Notification Hubs z Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="9b55a-193">**Node.js**: [How toouse Notification Hubs from Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span></span>
* <span data-ttu-id="9b55a-194">**Mobile Apps**: pro příklad toosend oznámení z backendu Azure App Service Mobile Apps, které jsou integrovány v centrech oznámení najdete v části [přidat nabízená oznámení tooyour mobilní aplikace](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="9b55a-194">**Mobile Apps**: For an example of how toosend notifications from an Azure App Service Mobile Apps backend that's integrated with Notification Hubs, see [Add push notifications tooyour mobile app](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span></span>
* <span data-ttu-id="9b55a-195">**Java / PHP**: Příklad jak hello toosend oznámení pomocí rozhraní REST API, naleznete v části "jak toouse centra oznámení z Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span><span class="sxs-lookup"><span data-stu-id="9b55a-195">**Java / PHP**: For an example of how toosend notifications by using hello REST APIs, see "How toouse Notification Hubs from Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span></span>

## <a name="optional-send-notifications-from-a-net-console-app"></a><span data-ttu-id="9b55a-196">(Volitelné) Odesílání oznámení z konzoly aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="9b55a-196">(Optional) Send notifications from a .NET console app.</span></span>
<span data-ttu-id="9b55a-197">V této části ukážeme odesílání oznámení pomocí konzolové aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="9b55a-197">In this section, we show sending a notification using a .NET console app.</span></span>

1. <span data-ttu-id="9b55a-198">Vytvořte novou konzolovou aplikaci Visual C#:</span><span class="sxs-lookup"><span data-stu-id="9b55a-198">Create a new Visual C# console application:</span></span>
   
    ![][30]
2. <span data-ttu-id="9b55a-199">V okně konzoly Správce balíčků hello, nastavte hello **výchozí projekt** tooyour nové konzoly projekt aplikace a potom v okně konzoly hello, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="9b55a-199">In hello Package Manager Console window, set hello **Default project** tooyour new console application project, and then in hello console window, execute hello following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="9b55a-200">Tento pokyn přidá odkaz toohello SDK centra oznámení Azure pomocí hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.</span><span class="sxs-lookup"><span data-stu-id="9b55a-200">This instruction adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
3. <span data-ttu-id="9b55a-201">Soubor otevřete hello **Program.cs** a přidejte následující hello pomocí příkazu:</span><span class="sxs-lookup"><span data-stu-id="9b55a-201">Open hello file **Program.cs** and add hello following using statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
4. <span data-ttu-id="9b55a-202">Ve vašem `Program` třídy, přidejte následující metodu hello a nahraďte *DefaultFullSharedAccessSignatureSASConnectionString* a *NotificationHubName* hello hodnotami, které máte.</span><span class="sxs-lookup"><span data-stu-id="9b55a-202">In your `Program` class, add hello following method and replace *DefaultFullSharedAccessSignatureSASConnectionString* and *NotificationHubName* with hello values that you have.</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }
5. <span data-ttu-id="9b55a-203">Přidejte následující řádky do hello vaše **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="9b55a-203">Add hello following lines in your **Main** method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();

## <a name="test-your-app"></a><span data-ttu-id="9b55a-204">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="9b55a-204">Test your app</span></span>
<span data-ttu-id="9b55a-205">tootest této aplikace pomocí skutečného telefonu, jednoduše připojte hello phone tooyour počítači pomocí kabelu USB.</span><span class="sxs-lookup"><span data-stu-id="9b55a-205">tootest this app with an actual phone, just connect hello phone tooyour computer by using a USB cable.</span></span> <span data-ttu-id="9b55a-206">Tato akce načte aplikaci do phone hello připojen.</span><span class="sxs-lookup"><span data-stu-id="9b55a-206">This action loads your app onto hello attached phone.</span></span>

<span data-ttu-id="9b55a-207">Klikněte na této aplikaci pomocí hello emulátoru na horním nástrojů Eclipse hello tootest **spustit**a pak vyberte svou aplikaci: začne hello emulátoru, zatížení, a spustí hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b55a-207">tootest this app with hello emulator, on hello Eclipse top toolbar, click **Run**, and then select your app: it starts hello emulator, loads, and runs hello app.</span></span>

<span data-ttu-id="9b55a-208">aplikace Hello načte hello "userId" a "channelId" ze hello služby nabízených oznámení Baidu a zaregistruje hello centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="9b55a-208">hello app retrieves hello 'userId' and 'channelId' from hello Baidu Push notification service and registers with hello notification hub.</span></span>

<span data-ttu-id="9b55a-209">toosend testovací oznámení, můžete použít hello ladění kartě hello portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="9b55a-209">toosend a test notification, you can use hello debug tab of hello Azure Classic Portal.</span></span> <span data-ttu-id="9b55a-210">Pokud jste vytvořili hello konzolové aplikace .NET pro Visual Studio, stačí stisknout klávesu F5 klíč hello v sadě Visual Studio toorun hello aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9b55a-210">If you built hello .NET console application for Visual Studio, just press hello F5 key in Visual Studio toorun hello application.</span></span> <span data-ttu-id="9b55a-211">aplikace Hello odešle oznámení, že se zobrazí v horní oznamovací oblasti hello emulátoru nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="9b55a-211">hello application sends a notification that appears in hello top notification area of your device or emulator.</span></span>

<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu Push Android SDK]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[portálu Azure Classic]: https://manage.windowsazure.com/
[portál Baidu]: http://www.baidu.com/
