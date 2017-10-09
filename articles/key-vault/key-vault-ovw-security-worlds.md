---
ms.assetid: 
title: architektury security World aaaAzure Key Vault | Microsoft Docs
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: 1995119c9e9886829d6c50af921f275d10e382f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Azure Key Vault architektury security World a geografické hranice.

Azure Key Vault je víceklientské služby a používá fond modulů hardwarového zabezpečení (HSM) v každém umístění Azure. 

Všechny moduly hardwarového zabezpečení v Azure umístěních v hello hello se stejným zeměpisnou oblast sdílejí stejnou hranici kryptografických (Thales Security World). Východ USA a západ USA sdílet hello stejné architektury security world, protože náleží toohello USA geografické umístění. Podobně všech Azure umístění ve sdílené složce Japonsko hello stejné architektury security world a všechny Azure umístění v Austrálii, Indie a tak dále. 

## <a name="backup-and-restore-behavior"></a>Zálohování a obnovení chování

Zálohy klíče z trezoru klíčů v jednom umístění Azure může být obnovena tooa trezoru klíčů v jiném umístění Azure, tak dlouho, dokud jsou splněny obě tyto podmínky:

- Obě hello Azure umístění patří toohello stejného geografického umístění
- Obě trezorů klíčů hello patří toohello stejného předplatného Azure

Například může být zálohy podle konkrétního předplatného klíče v trezoru klíčů v Západní Indie pouze obnovené tooanother trezoru klíčů v hello stejného předplatného a geografická umístění; Indie – Západ, Indie centrální nebo – Jih, Indie.

## <a name="regions-and-products"></a>Oblasti a produkty

- [Oblasti Azure](https://azure.microsoft.com/regions/)
- [Produkty společnosti Microsoft podle oblasti](https://azure.microsoft.com/regions/services/)

Oblasti jsou namapované toosecurity světů, zobrazené jako hlavní záhlaví v tabulkách hello:

V produktech hello článkem oblast, například hello **Americas** karta obsahuje VÝCHOD USA, STŘED USA, ZÁPADNÍ USA všechny oblasti Americas toohello mapování. 

>[!NOTE]
>Výjimkou je, že nám DOD VÝCHOD a nám DOD centrální mají své vlastní architektury security World. 

Podobně na hello **Evropa** kartě, severní EVROPA a ZÁPADNÍ EVROPA obě namapovány toohello Evropa oblast. Hello totéž platí také na hello **Asie a Tichomoří** kartě.



