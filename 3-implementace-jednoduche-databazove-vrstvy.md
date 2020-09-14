# 3 Implementace jednoduché databázové vrstvy

Nejdříve vysvětlíme základní princip fungování databázové vrstvy \(viz obrázek níže\):

1. Někdo \(vyšší vrstva\) pošle požadavek, že potřebuje realizovat nějakou operaci s databází \(načtení/uložení dat\). Tyto požadavky zasílá výhradně a pouze DAO objektu, u nás "BookDAO".
2. BookDAO požadavek zpracuje, předpřipraví si data \(je-li to třeba\) a následně požadavek předá níže objektu "EntityManager". EntityManager je implementován v JPA a my jen budeme využívat jeho hotové funkcionality.
3. EntityManager nám vrátí data. BookDAO je opět zpracuje, je-li třeba, a výsledek vrací volajícímu objektu.

![](.gitbook/assets/3-schema.png)

{% hint style="info" %}
V kontextu této vrstvy budeme pracovat se dvěma typy třídy - DAO \(data access object\) bude třídou, která poskytuje operace s danou entitou. DAO je typicky postfixem třídy, kdy prefixem je název tabulky \(například BookDAO, UserDAO, AuthorDAO\). Dnes má tato třída také postfíx Persistence \(BookPersistence, UserPersistence atd\). Každá entita má vlastn DAO objekt.

Obdobně, třída, která nám bude reprezentovat záznamy z tabulky, má název složený z názvu tabulky a příponou Entity. Takovým třídám říkáme _entity_.

Tato pravidla pro pojmenování tříd jsou libovolná, je však dobrá si nějaká stanovit, protože to výrazně zvyšuje čitelnost kódu.
{% endhint %}

{% hint style="info" %}
Vytváření jednoho DAO objektu pro všechny entity je možné, ale není to vhodné, protože se s množícím se počtem metod rychle snižuje čitelnost jejího kódu.
{% endhint %}

Databázová vrstva je pro nás tedy z implementačního hlediska představována objektem BookDAO - hlavní třídou, která se bude starat o všechny databázové operace s knihami, a třídou BookEntity, která reprezentuje jeden řádek v tabulce. Protože tato třída nic aktivně nevykonává \(je pouze nosičem dat\), není v procesu volání znázorněna. Pokud má třída BookDAO realizovat základní databázové operace, nejdříve rychle nadefinujeme operace, které po ní budeme vyžadovat - vkládání, mazání a získání všech záznamů jako listu:

{% code title="BookDao.java" %}
```java
public class BookDAO {

    public void insert(BookEntity book) { }

    public void delete(int bookId) { }

    public List<BookEntity> getAll(int maxCount) { }
}
```
{% endcode %}

