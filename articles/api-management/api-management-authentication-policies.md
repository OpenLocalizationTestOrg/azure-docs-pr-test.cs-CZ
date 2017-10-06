---
title: "zásady ověřování aaaAzure API Management | Microsoft Docs"
description: "Další informace o zásadách ověřování hello k dispozici pro použití v Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: ce93cced66cb67520e97c7c15f3685bffb08e1f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-authentication-policies"></a><span data-ttu-id="5053e-103">Zásady ověřování služby API Management</span><span class="sxs-lookup"><span data-stu-id="5053e-103">API Management authentication policies</span></span>
<span data-ttu-id="5053e-104">Toto téma obsahuje odkaz pro hello následující zásady služby API Management.</span><span class="sxs-lookup"><span data-stu-id="5053e-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="5053e-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="5053e-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="5053e-106"><a name="AuthenticationPolicies"></a>Zásady ověřování</span><span class="sxs-lookup"><span data-stu-id="5053e-106"><a name="AuthenticationPolicies"></a> Authentication policies</span></span>  
  
-   <span data-ttu-id="5053e-107">[Ověřování Basic](api-management-authentication-policies.md#Basic) -ověření pomocí back-end službu pomocí základního ověřování.</span><span class="sxs-lookup"><span data-stu-id="5053e-107">[Authenticate with Basic](api-management-authentication-policies.md#Basic) - Authenticate with a backend service using Basic authentication.</span></span>  
  
-   <span data-ttu-id="5053e-108">[Ověřování pomocí certifikátu klienta](api-management-authentication-policies.md#ClientCertificate) -ověření pomocí back-end službu pomocí klientských certifikátů.</span><span class="sxs-lookup"><span data-stu-id="5053e-108">[Authenticate with client certificate](api-management-authentication-policies.md#ClientCertificate) - Authenticate with a backend service using client certificates.</span></span>  
  
##  <span data-ttu-id="5053e-109"><a name="Basic"></a>Ověřování Basic</span><span class="sxs-lookup"><span data-stu-id="5053e-109"><a name="Basic"></a> Authenticate with Basic</span></span>  
 <span data-ttu-id="5053e-110">Použití hello `authentication-basic` zásad tooauthenticate s back-end službu pomocí základního ověřování.</span><span class="sxs-lookup"><span data-stu-id="5053e-110">Use hello `authentication-basic` policy tooauthenticate with a backend service using Basic authentication.</span></span> <span data-ttu-id="5053e-111">Tato zásada nastavuje toohello záhlaví HTTP autorizace hello odpovídající údaje toohello hodnota poskytnutá v zásadě hello.</span><span class="sxs-lookup"><span data-stu-id="5053e-111">This policy effectively sets hello HTTP Authorization header toohello value corresponding toohello credentials provided in hello policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="5053e-112">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="5053e-112">Policy statement</span></span>  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a><span data-ttu-id="5053e-113">Příklad</span><span class="sxs-lookup"><span data-stu-id="5053e-113">Example</span></span>  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a><span data-ttu-id="5053e-114">Elementy</span><span class="sxs-lookup"><span data-stu-id="5053e-114">Elements</span></span>  
  
|<span data-ttu-id="5053e-115">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5053e-115">Name</span></span>|<span data-ttu-id="5053e-116">Popis</span><span class="sxs-lookup"><span data-stu-id="5053e-116">Description</span></span>|<span data-ttu-id="5053e-117">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5053e-117">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="5053e-118">ověřování – základní</span><span class="sxs-lookup"><span data-stu-id="5053e-118">authentication-basic</span></span>|<span data-ttu-id="5053e-119">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="5053e-119">Root element.</span></span>|<span data-ttu-id="5053e-120">Ano</span><span class="sxs-lookup"><span data-stu-id="5053e-120">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="5053e-121">Atributy</span><span class="sxs-lookup"><span data-stu-id="5053e-121">Attributes</span></span>  
  
|<span data-ttu-id="5053e-122">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5053e-122">Name</span></span>|<span data-ttu-id="5053e-123">Popis</span><span class="sxs-lookup"><span data-stu-id="5053e-123">Description</span></span>|<span data-ttu-id="5053e-124">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5053e-124">Required</span></span>|<span data-ttu-id="5053e-125">Výchozí</span><span class="sxs-lookup"><span data-stu-id="5053e-125">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="5053e-126">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5053e-126">username</span></span>|<span data-ttu-id="5053e-127">Určuje uživatelské jméno hello hello základní pověření.</span><span class="sxs-lookup"><span data-stu-id="5053e-127">Specifies hello username of hello Basic credential.</span></span>|<span data-ttu-id="5053e-128">Ano</span><span class="sxs-lookup"><span data-stu-id="5053e-128">Yes</span></span>|<span data-ttu-id="5053e-129">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5053e-129">N/A</span></span>|  
|<span data-ttu-id="5053e-130">heslo</span><span class="sxs-lookup"><span data-stu-id="5053e-130">password</span></span>|<span data-ttu-id="5053e-131">Určuje heslo hello hello základní pověření.</span><span class="sxs-lookup"><span data-stu-id="5053e-131">Specifies hello password of hello Basic credential.</span></span>|<span data-ttu-id="5053e-132">Ano</span><span class="sxs-lookup"><span data-stu-id="5053e-132">Yes</span></span>|<span data-ttu-id="5053e-133">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5053e-133">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="5053e-134">Využití</span><span class="sxs-lookup"><span data-stu-id="5053e-134">Usage</span></span>  
 <span data-ttu-id="5053e-135">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="5053e-135">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="5053e-136">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="5053e-136">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="5053e-137">**Zásady obory:** rozhraní API</span><span class="sxs-lookup"><span data-stu-id="5053e-137">**Policy scopes:** API</span></span>  
  
##  <span data-ttu-id="5053e-138"><a name="ClientCertificate"></a>Ověřování pomocí certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="5053e-138"><a name="ClientCertificate"></a> Authenticate with client certificate</span></span>  
 <span data-ttu-id="5053e-139">Použití hello `authentication-certificate` zásad tooauthenticate službě back-end pomocí klientského certifikátu.</span><span class="sxs-lookup"><span data-stu-id="5053e-139">Use hello `authentication-certificate` policy tooauthenticate with a backend service using client certificate.</span></span> <span data-ttu-id="5053e-140">certifikát Hello musí toobe [nainstalován do rozhraní API správy](http://go.microsoft.com/fwlink/?LinkID=511599) první a je určený podle jeho kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="5053e-140">hello certificate needs toobe [installed into API Management](http://go.microsoft.com/fwlink/?LinkID=511599) first and is identified by its thumbprint.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="5053e-141">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="5053e-141">Policy statement</span></span>  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a><span data-ttu-id="5053e-142">Příklad</span><span class="sxs-lookup"><span data-stu-id="5053e-142">Example</span></span>  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a><span data-ttu-id="5053e-143">Elementy</span><span class="sxs-lookup"><span data-stu-id="5053e-143">Elements</span></span>  
  
|<span data-ttu-id="5053e-144">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5053e-144">Name</span></span>|<span data-ttu-id="5053e-145">Popis</span><span class="sxs-lookup"><span data-stu-id="5053e-145">Description</span></span>|<span data-ttu-id="5053e-146">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5053e-146">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="5053e-147">ověřovací certifikát</span><span class="sxs-lookup"><span data-stu-id="5053e-147">authentication-certificate</span></span>|<span data-ttu-id="5053e-148">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="5053e-148">Root element.</span></span>|<span data-ttu-id="5053e-149">Ano</span><span class="sxs-lookup"><span data-stu-id="5053e-149">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="5053e-150">Atributy</span><span class="sxs-lookup"><span data-stu-id="5053e-150">Attributes</span></span>  
  
|<span data-ttu-id="5053e-151">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5053e-151">Name</span></span>|<span data-ttu-id="5053e-152">Popis</span><span class="sxs-lookup"><span data-stu-id="5053e-152">Description</span></span>|<span data-ttu-id="5053e-153">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5053e-153">Required</span></span>|<span data-ttu-id="5053e-154">Výchozí</span><span class="sxs-lookup"><span data-stu-id="5053e-154">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="5053e-155">Kryptografický otisk</span><span class="sxs-lookup"><span data-stu-id="5053e-155">thumbprint</span></span>|<span data-ttu-id="5053e-156">Hello kryptografický otisk certifikátu klienta hello.</span><span class="sxs-lookup"><span data-stu-id="5053e-156">hello thumbprint for hello client certificate.</span></span>|<span data-ttu-id="5053e-157">Ano</span><span class="sxs-lookup"><span data-stu-id="5053e-157">Yes</span></span>|<span data-ttu-id="5053e-158">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5053e-158">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="5053e-159">Využití</span><span class="sxs-lookup"><span data-stu-id="5053e-159">Usage</span></span>  
 <span data-ttu-id="5053e-160">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="5053e-160">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="5053e-161">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="5053e-161">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="5053e-162">**Zásady obory:** rozhraní API</span><span class="sxs-lookup"><span data-stu-id="5053e-162">**Policy scopes:** API</span></span>  
  

## <a name="next-steps"></a><span data-ttu-id="5053e-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5053e-163">Next steps</span></span>
<span data-ttu-id="5053e-164">Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="5053e-164">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  