## <a name="scenario"></a>Scénář
toobetter znázorňují toocreate Nsg, tento dokument se použití hello scénář níže.

![Scénář sítě VNet](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

V tomto scénáři vytvoříte skupinu NSG pro každou podsíť v hello **TestVNet** virtuální sítě, jak je popsáno níže: 

* **Skupina NSG front-endu**. Hello front-endu NSG bude použité toohello *front-endu* podsítě a obsahují dvě pravidla:    
  * **pravidlo protokolu RDP**. Toto pravidlo povolí toohello provoz protokolu RDP *front-endu* podsítě.
  * **pravidlo webové**. Toto pravidlo povolí provoz toohello HTTP *front-endu* podsítě.
* **Skupina NSG back-end**. Hello back-end NSG bude použité toohello *back-end* podsítě a obsahují dvě pravidla:    
  * **Pravidlo SQL**. Toto pravidlo umožňuje SQL přenos jenom z hello *front-endu* podsítě.
  * **pravidlo webové**. Toto pravidlo odmítne všechny internet vázaný provoz z hello *back-end* podsítě.

Hello kombinace tato pravidla vytvořit jako DMZ scénář, kde hello back-end podsíť může přijímat pouze příchozí provoz SQL z podsítě front end hello a nemá žádné toohello přístup k Internetu, zatímco podsítě front end hello může komunikovat s hello Internet, a přijímat pouze příchozí požadavky HTTP.

