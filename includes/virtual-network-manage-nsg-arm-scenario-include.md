## <a name="sample-scenario"></a>Vzorový scénář
toobetter znázorňují, jak toomanage Nsg, tento článek používá hello scénář níže.

![Scénář sítě VNet](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

V tomto scénáři vytvoříte skupinu NSG pro každou podsíť v hello **TestVNet** virtuální sítě, jak je popsáno níže: 

* **Skupina NSG front-endu**. Hello front-endu NSG bude použité toohello *front-endu* podsítě a obsahují dvě pravidla:    
  * **pravidlo protokolu RDP**. Toto pravidlo povolí toohello provoz protokolu RDP *front-endu* podsítě.
  * **pravidlo webové**. Toto pravidlo povolí provoz toohello HTTP *front-endu* podsítě.
* **Skupina NSG back-end**. Hello back-end NSG bude použité toohello *back-end* podsítě a obsahují dvě pravidla:    
  * **Pravidlo SQL**. Toto pravidlo umožňuje SQL přenos jenom z hello *front-endu* podsítě.
  * **pravidlo webové**. Toto pravidlo odmítne všechny internet vázaný provoz z hello *back-end* podsítě.

Hello kombinace těchto pravidel vytvořit jako zóna DMZ scénář, kde můžete přijímat pouze příchozí provoz pro provoz SQL z podsítě front end hello hello back-end podsítě a má toohello žádný přístup k Internetu, zatímco podsítě front end hello může komunikovat s hello Internet a přijímat pouze příchozí požadavky HTTP.

postupujte podle toodeploy hello scénář popsaný výše, [tento odkaz](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), klikněte na tlačítko **nasazení tooAzure**, nahraďte hello výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů hello hello portálu. V pokynech ukázka hello níže hello šablony byla použité toodeploy prostředek názvy skupin **RG NSG**. 

