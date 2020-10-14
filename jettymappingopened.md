---
description: >-
  *Požadovanou operaci nelze provést se souborem, jehož uživateli mapovaný oddíl
  je otevřen* - Jak vyřešit?
---

# Jetty + CSS redeploy problém

Tato stránka řeší problém, kdy se do projektu přidají statické soubory. Při pokusu o rychlé obnovení \(Update classes nebo Redeploy\) prostředí zahlásí chybu **Požadovanou operaci nelze provést se souborem, jehož uživateli mapovaný oddíl je otevřen**.

## Popis

Problém vzniká, protože statické soubory Jetty připojuje pomocí NIO konektorů \(vlastně otevřených kanálů pro čtení\) do daných souborů. Tento postup ale způsobí, že soubor se uzamkne v operačním systému pro změny a nelze jej nahradit.

## Řešení

Rychlým řešením může být nastavení webové aplikace tak, aby nepoužívala mapování do statických souborů. Toto lze udělat několika způsoby. Jedním ze základních je úprava souboru `web.xml`. Soubor se nachází ve webové složce projektu `{projekt}/src/main/webapp/WEB-INF`. Do projektu vložíme blok kódu, aby soubor vypadal cca takto:

{% tabs %}
{% tab title="Vkládaný blok kódu" %}
```markup
<servlet>
    <servlet-name>default</servlet-name>
    <init-param>
        <param-name>useFileMappedBuffer</param-name>
        <param-value>false</param-value>
    </init-param>
</servlet>
```
{% endtab %}

{% tab title="Příklad výsledného souboru \'web.xml\'" %}
{% code title="web.xml" %}
```markup
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>default</servlet-name>
        <init-param>
            <param-name>useFileMappedBuffer</param-name>
            <param-value>false</param-value>
        </init-param>
    </servlet>
</web-app>
```
{% endcode %}
{% endtab %}
{% endtabs %}

Následně webový server zastavíme a znovu spustíme. Po dalších úpravách CSS soubor by již vše mělo běžet správně.

Pokud řešení nefunguje, pokračujte do sekce **Více informací**. 

## Více informací

Více informací lze nalézt přímo na stránkách výrobce: [https://www.eclipse.org/jetty/documentation/current/troubleshooting-locked-files-on-windows.html](https://www.eclipse.org/jetty/documentation/current/troubleshooting-locked-files-on-windows.html), nebo přes vyhledávání, například [https://www.google.com/search?q=jetty+update+mapped+file&oq=jetty+update+mapped+file](https://www.google.com/search?q=jetty+update+mapped+file&oq=jetty+update+mapped+file).

