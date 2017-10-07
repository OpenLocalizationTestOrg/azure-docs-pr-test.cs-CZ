---
title: "referenční informace o protokolu AD SAML aaaAzure | Microsoft Docs"
description: "Tento článek obsahuje přehled hello jednotné přihlašování a jeden Sign-Out SAML profily v Azure Active Directory."
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 88125cfc-45c1-448b-9903-a629d8f31b01
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: priyamo
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: d712289b16dc40a6b43a96fadef729c55cdaac47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Jak Azure Active Directory používá protokol SAML hello
Azure Active Directory (Azure AD) používá hello uživatelům tootheir prostředí aplikace SAML 2.0 protokolu tooenable tooprovide jednotné přihlašování. Hello [jednotné přihlašování](active-directory-single-sign-on-protocol-reference.md) a [jedním odhlašování](active-directory-single-sign-out-protocol-reference.md) SAML profilů Azure AD vysvětlují, jak SAML kontrolní výrazy, protokoly a vazby používá služba Zprostředkovatel identity hello.

Protokol SAML vyžaduje zprostředkovatele identity hello (Azure AD) a hello služby zprostředkovatele (aplikace hello) tooexchange informace o sami.

Při registraci aplikace s Azure AD, vývojáři aplikace hello zaregistruje informací souvisejících s federační službou Azure AD. To zahrnuje hello **identifikátor URI pro přesměrování** a **Metadata URI** aplikace hello.

Azure AD používá hello **Metadata URI** z hello cloudové služby tooretrieve hello podpisový klíč a hello odhlášení URI hello cloudové služby. Pokud aplikace hello nepodporuje metadata identifikátor URI, hello developer musí obraťte se na Microsoft podporu tooprovide hello odhlášení URI a podpisového klíče.

Azure Active Directory zpřístupní konkrétního klienta a běžných (nezávislé na klienta) jeden přihlášení a jeden odhlašování koncových bodů. Představují tyto adresy URL adresovatelné umístění – nejsou právě identifikátory –, můžete přejít toohello koncový bod tooread hello metadat.

* koncový bod Hello konkrétního klienta se nachází v `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  Hello <TenantDomainName> zástupný symbol představuje registrovaný název domény nebo TenantID GUID klienta Azure AD. Například hello federačních metadat hello contoso.com klienta je v: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

* koncový bod Hello nezávislé na klienta se nachází v `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. V této adresa koncového bodu **běžné** se zobrazí místo název domény klienta nebo ID.

Informace o dokumentech hello federačních metadat, která publikuje Azure AD najdete v tématu [federačních metadat](active-directory-federation-metadata.md).
