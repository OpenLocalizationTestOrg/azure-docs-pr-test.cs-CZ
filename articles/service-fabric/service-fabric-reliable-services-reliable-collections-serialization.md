---
title: "aaaReliable serializaci kolekce objektů v Azure Service Fabric | Microsoft Docs"
description: "Serializace objektu Azure Service Fabric spolehlivé kolekce"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 9d35374c-2d75-4856-b776-e59284641956
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/8/2017
ms.author: mcoskun
ms.openlocfilehash: 248defbe0ae6f65b4ac5dc7c74e80d8f1152ce94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-collection-object-serialization-in-azure-service-fabric"></a>Spolehlivé serializace objektu kolekce v Azure Service Fabric
Spolehlivé kolekce replikovat a zachovat jejich toomake položky se, že jsou odolné napříč selhání počítače a výpadky napájení.
Položky tooreplicate a toopersist, tooserialize nutné spolehlivé kolekce je.

Spolehlivé kolekce získat hello odpovídající serializátor pro daný typ z spolehlivé správce stavu.
Správce spolehlivé stavu obsahuje integrovanou serializátorů a umožňuje vlastní serializátorů toobe zaregistrovat pro daného typu.

## <a name="built-in-serializers"></a>Předdefinované Serializátorů

Správce spolehlivé stavu zahrnuje integrovanou serializátor pro některé běžné typy tak, aby se Dal serializovat efektivně ve výchozím nastavení. Pro ostatní typy spolehlivé správce stavu spadne zpět toouse hello [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).
Předdefinované serializátorů jsou efektivnější, protože vědí, nelze změnit jejich typy a nepotřebují tooinclude informace o typu hello jako její název typu.

Správce spolehlivé stavu má integrovanou serializátor pro následující typy: 
- Identifikátor GUID
- BOOL
- Bajtů
- SByte –
- Byte]
- Char
- Řetězec
- Decimal
- Double
- Plovoucí desetinná čárka
- celá čísla
- uint
- dlouhá
- ulong –
- krátký
- ushort –

## <a name="custom-serialization"></a>Vlastní serializace

Vlastní serializátorů jsou běžně používané tooincrease výkonu nebo tooencrypt hello data přes přenosu hello a na disku. Mezi z jiných důvodů jsou často efektivnější než obecné serializátor vlastní serializátorů, vzhledem k tomu nepotřebují tooserialize informace o typu hello. 

[IReliableStateManager.TryAddStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer--1?Microsoft_ServiceFabric_Data_IReliableStateManager_TryAddStateSerializer__1_Microsoft_ServiceFabric_Data_IStateSerializer___0__) je použité tooregister vlastní serializátor hello daného typu T. Tato registrace má proběhnout v hello konstrukce hello StatefulServiceBase tooensure, před zahájením obnovení, všechny spolehlivé kolekce mají přístup k toohello relevantní serializátor tooread jejich trvalá data.

```C#
public StatefulBackendService(StatefulServiceContext context)
  : base(context)
  {
    if (!this.StateManager.TryAddStateSerializer(new OrderKeySerializer()))
    {
      throw new InvalidOperationException("Failed tooset OrderKey custom serializer");
    }
  }
```

> [!NOTE]
> Vlastní serializátorů dané priority nad předdefinované serializátorů. Například když je zaregistrovaný vlastní serializátor int, je celá čísla použité tooserialize místo hello předdefinované serializátor pro int.

### <a name="how-tooimplement-a-custom-serializer"></a>Jak tooimplement vlastní serializátor

Vlastní serializátor musí tooimplement hello [IStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) rozhraní.

> [!NOTE]
> IStateSerializer<T> zahrnuje přetížení pro zápis a čtení, která přebírá další t označuje základní hodnota. Toto rozhraní API je pro rozdílové serializace. Aktuálně nebude vystavena, funkce rozdílové serializace. Proto nejsou tyto dvě přetížení volá dokud rozdílové serializace je vystavený a povolený.

Následuje příklad vlastního typu, v názvem OrderKey, který obsahuje čtyři vlastnosti

```C#
public class OrderKey : IComparable<OrderKey>, IEquatable<OrderKey>
{
    public byte Warehouse { get; set; }

    public short District { get; set; }

    public int Customer { get; set; }

    public long Order { get; set; }

    #region Object Overrides for GetHashCode, CompareTo and Equals
    #endregion
}
```

Následuje příklad implementace IStateSerializer<OrderKey>.
Všimněte si, že čtení a zápis přetížení, které provést ve baseValue, jejich příslušné přetížení volání pro předávání kompatibility.

```C#
public class OrderKeySerializer : IStateSerializer<OrderKey>
{
  OrderKey IStateSerializer<OrderKey>.Read(BinaryReader reader)
  {
      var value = new OrderKey();
      value.Warehouse = reader.ReadByte();
      value.District = reader.ReadInt16();
      value.Customer = reader.ReadInt32();
      value.Order = reader.ReadInt64();

      return value;
  }

  void IStateSerializer<OrderKey>.Write(OrderKey value, BinaryWriter writer)
  {
      writer.Write(value.Warehouse);
      writer.Write(value.District);
      writer.Write(value.Customer);
      writer.Write(value.Order);
  }
  
  // Read overload for differential de-serialization
  OrderKey IStateSerializer<OrderKey>.Read(OrderKey baseValue, BinaryReader reader)
  {
      return ((IStateSerializer<OrderKey>)this).Read(reader);
  }

  // Write overload for differential serialization
  void IStateSerializer<OrderKey>.Write(OrderKey baseValue, OrderKey newValue, BinaryWriter writer)
  {
      ((IStateSerializer<OrderKey>)this).Write(newValue, writer);
  }
}
```

## <a name="upgradability"></a>Zdokonalovány
V [vrácení upgradu aplikace](service-fabric-application-upgrade.md), hello upgrade je použité tooa dílčí sadu uzlů, jednu upgradovací doménu najednou. Během tohoto procesu několik domén upgradu bude v novější verzi aplikace hello a některých domén upgradu bude na hello starší verzi aplikace. Při zavedení hello hello novou verzi vaší aplikace musí být schopný tooread hello starší verzi vaše data a hello starší verzi aplikace musí být schopný tooread hello novou verzi vaše data. Pokud formát dat hello není vpřed a zpětně kompatibilní, upgrade hello může selže nebo horší, data mohou být ke ztrátě nebo poškozený.

Pokud používáte integrované serializátor, není nutné tooworry o kompatibilitě.
Ale pokud používáte vlastní serializátor nebo hello DataContractSerializer, hello data mají toobe nekonečnou zpětné a předává kompatibilní.
Jinými slovy každou verzi serializátor musí mít tooserialize toobe a deserializovat všechny verze hello typu.

Uživatelé kontraktu dat postupujte podle pravidla hello dobře definovaný verzí pro přidání, odebrání a změna pole. Kontrakt dat má také podpora zabývají neznámé pole, zapojování do procesu hello serializace a deserializace a plánování práce s dědičnosti tříd. Další informace najdete v tématu [pomocí kontrakt dat](https://msdn.microsoft.com/library/ms733127.aspx).

Vlastní serializátor uživatelé by měl splňovat toohello pokynů hello serializátor používají toomake, zda je zpětné a předává kompatibilní.
Běžný způsob podpory všechny verze je přidání informací o velikosti na začátku hello a jenom přidávání volitelné vlastnosti.
Tímto způsobem jednotlivých verzí může číst co nejvíc může a přejít přes hello zbývající část datového proudu hello.

## <a name="next-steps"></a>Další kroky
  * [Serializace a upgrade](service-fabric-application-upgrade-data-serialization.md)
  * [Referenční informace pro vývojáře pro spolehlivé kolekce](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  * [Upgrade vaší aplikace pomocí sady Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí sady Visual Studio.
  * [Upgrade vaší aplikace pomocí prostředí Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vás provede upgrade aplikace pomocí prostředí PowerShell.
  * Řídí, jak vaše aplikace upgraduje pomocí [Upgrade parametry](service-fabric-application-upgrade-parameters.md).
  * Zjistěte, jak toouse pokročilé funkce při upgradu vaší aplikace tím, že odkazuje příliš[Pokročilá témata](service-fabric-application-upgrade-advanced.md).
  * Řešení běžných potíží v upgradů aplikací tím, že odkazuje toohello kroky v [řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md).
