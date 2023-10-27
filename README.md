# Detalii despre integrarea cu sistemul ROeiD

## Modelul de incredere pentru SSO

În vederea realizării parteneriatului între Platforma ROeID și Furnizorii de Servicii de eGuvernare este necesara stabilirea unor acorduri / protocoale de colaborare pentru asigurarea încrederii și transferului de identități și credențiale de autentificare între Platforma ROeID și sistemele eGuvernare. Încheierea acestor acorduri / protocoale va permite Furnizorilor de Servicii eGuvernare să ia decizii privind accesul în sistemele proprii pe baza nivelului de încredere definit în cadrul Platformei ROeID![image](https://user-images.githubusercontent.com/113096980/215775261-e123ff17-dd47-4443-a14c-76610c82836a.png)

## Primul pas

Primul pas din punct de vedere tehnic dupa ce se semneaza protocolul de colaborare este sa transmiteti echipei ADR solicitarea pentru credentiale mediu de test si productie, specificand care este **redirect_url** 

>ATENTIE !!!
>Redirect_url reprezinta adresa unde sa redirecteze sistemul ROeID utilizatorul spre platforma dvs. Acest element este **obligatoriu** sa fie solicitat in avans, cu mentiunea ca se poate modifica pe parcurs la cerere

## Autentificare tip Single-Sign-On (SSO)

Accesul Single Sign-On (SSO) presupune o relație între două sau mai multe părți interesate în ceea ce privește autentificarea utilizatorilor și care depășește diferențele tehnologice între diferite entități. 
Platforma ROeID reprezintă în cadrul acestui sistem de încredere entitatea Furnizor de Identitate (Identity Provider - IdP) fiind responsabilă pentru gestionarea identităților și garantarea identității utilizatorilor. Furnizorii de Servicii eGuvernare (denumiți Service Provider - SP) autorizează accesul la serviciile oferite pe baza informațiilor despre utilizator primite de la Furnizorul de Identitate (respectiv ROeID). 
Tranzacțiile dintre partenerii care formează Federația SSO permit:
- Schimb sigur de informații de identificare a utilizatorilor între parteneri.
- Stabilirea unei relații între identitatea utilizatorului din partea Furnizor de Identitate (ROeID) și identitatea utilizatorului în sistemul Furnizorului de Servicii eGuvernare.
- Funcționalitate de conectare unică între site-urile web ale partenerilor din cadrul SSO.
- Controlul accesului la resursele Furnizorului de Servicii eGuvernare pe baza informațiilor primite de la Furnizorul de Identitate.
- Interoperabilitate între medii eterogene

Autentificarea SSO poate fi realizată prin implementarea oricăruia dintre următoarelor două standarde de comunicare securizate / protocoale de autentificare:
- Security Assertion Markup Language (SAML) 2.0 - https://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html  
- OpenID Connect (OIDC) - https://openid.net  

## Protocolul Security Assertion Markup Language (SAML) – descriere generală

SAML este un protocol dezvoltat de OASIS (Organization for the Advancement of Structured Information Standards). SAML este un cadru XML destinat schimbului de date de autentificare și autorizare. 
SAML transmite acreditările utilizatorului de la un IdP (Identity Provider) care deține și menține identități pentru a verifica drepturile de acces catre un SP (Service Provider). Furnizorii de servicii necesită autentificare înainte de a acorda acces utilizatorilor la resursă. Fiecare utilizator (sau grup) are atribute care conturează informațiile de profil și afirmă exact ce anume este autorizat să acceseze. 
SAML folosește documente cu metadate în limbajul de markup extensibil (XML) (token-uri SAML) pentru un proces de afirmare pentru a verifica identitatea și privilegiile de acces ale unui utilizator. 
Dezvoltatorii folosesc pluginuri SAML în aplicații sau resurse pentru o experiență de conectare SSO care asigură că sunt respectate practicile de securitate, iar acreditările/afirmațiile determină cine poate accesa o aplicație. În plus, SAML poate fi folosit pentru a controla ce resurse poate accesa o identitate într-o aplicație. 
SAML folosește o asertiune (SAML Token) pentru a face schimb de informații de identificare a utilizatorului între entități. Tokenul este semnat criptografic, criptarea este opțională. Asertiunile SAML pot fi:
- Asertiune de autentificare – confirmă autentificarea utilizatorului, timpul de conectare și metoda de autentificare
- Asertiune de autorizare – oferă informații despre utilizator: dacă utilizatorul este autorizat să utilizeze serviciul sau dacă Identity Provider a respins cererea din cauza autentificării incorecte
- Asertiunea atributului - trimite anumite atribute SAML către Service Provider (informații suplimentare despre utilizator)
Protocolul SAML asigură un proces de autentificare și autorizare în mediul în care se stabilește Federația SSO între entități. 

## Protocolul OpenID Connect – descriere generală

OpenID Connect (OIDC) extinde protocolul OAuth, astfel încât serviciile client (aplicațiile dvs.) să verifice identitățile utilizatorilor și să facă schimb de informații despre profil cu furnizorii OpenID prin intermediul API-urilor RESTful care trimit jetoane/token web JSON (JWT) pentru a partaja informații în timpul procesului de autentificare. Furnizorii sunt în esență servere de autentificare. Mulți dezvoltatori sunt atrași de această abordare deoarece este foarte scalabilă, flexibilă pe platforme și este relativ simplu de implementat. Componentele sale principale sunt un flux de lucru unic de identificare a utilizatorului cu baze OAuth. 

Autentificarea utilizatorului: un utilizator se autentifică și este autorizat să acceseze o aplicație client prin intermediul unui server de autorizare care acordă un jeton/token de acces care permite aplicațiilor să primească informații cu consimțământ de la un punct final UserInfo. Un punct final UserInfo este o resursă protejată găsită pe un server OpenID care conține afirmații despre fiecare utilizator dintr-un obiect JSON. Informațiile de autentificare sunt apoi codificate într-un simbol ID primit de aplicație. Aceste informații sunt stocate în cache pentru performanțe scalabile și personalizează experiența utilizatorului final.

Asemănări: 
- Ambele  sunt protocoale de identitate care fac posibil SSO. 
- Ambele sunt sigure, bine documentate și mature. 
- Utilizatorii se autentifică printr-un IdP (Identity Provider – respectiv ROeID), de obicei o dată, apoi pot accesa alte aplicații care „au încredere” în IdP. Unele servicii Zero Trust se vor autentifica în fiecare punct de-a lungul lanțului și se va verifica periodic accesul folosind o altă metodă de autentificare.
- Fluxul de lucru de conectare pare foarte similar cu utilizatorul final, care nu are nicio vizibilitate asupra diferitelor diferențe tehnice care apar în culise. 

Diferente:
- Mulți dezvoltatori considera că OpenID Connect este mai simplu de implementat, deoarece nu există manipulare XML. 
- OpenID nu are date de autorizare a utilizatorului (cum ar fi permisiunile) și se concentrează în primul rând pe afirmarea identității. SAML este un schimb de date de identitate și este foarte bogat în caracteristici. 
- Autentificarea este descentralizată cu OpenID. 
- SAML utilizează afirmații în comparație cu arhitectura OpenID și OAuth a tokenurilor de identitate.
- OIDC este conceput pentru integrari API și poate fi folosit pentru a securiza API-urile.

### Cazuri de utilizare

Dezvoltatorii și furnizorii de servicii ar trebui să aleagă soluția care funcționează cel mai bine pentru cazurile lor de utilizare specifice. 

Mai jos sunt câteva recomandări generale pentru cazurile de utilizare: 
- O aplicație mobilă utilizează de obicei un serviciu cum ar fi OIDC, deoarece are o greutate mai mică și multe dintre facilitățile pe care le folosesc dezvoltatorii sunt pre-construite sau disponibile din bibliotecile suplimentare.
- Aplicațiile care au SAML încorporat sunt simplu de implementat, trebuie doar să utilizați SAML.
- Utilizați SAML pentru a accesa aplicațiile prin intermediul unui portal.
- Protejarea API-urilor sau expunerea API-urilor sunt servicii care vor necesita OAuth 2.0 sau OIDC.

### Folosind OIDC și SAML împreună 

Aceste protocoale nu se exclud reciproc. Luați în considerare utilizarea SAML pentru SSO pentru a securiza accesul la resursele organizației dvs. și OpenID Connect (OIDC) pentru cazurile de utilizare mobile care au cerințe ridicate de scalabilitate. Fiecare are propriile sale avantaje inerente și oricare poate oferi servicii SSO.

## Procesul de autentificare prin ROeID 

Prin înrolarea serviciilor de eGuvernare în Platforma ROeID, cetatenii vor avea la dispoziție un catalog de servicii care vor putea fi accesate prin SSO. 
În cazul în care cetățenii vor accesa un serviciu prin modul de autentificare ROeID, aceștia vor fi identificați cu utilizatori deja existenți în serviciul accesat (prin potrivirea unui atribut unic de identificare) sau (în cazul în care sunt la prima utilizare a serviciului respectiv) vor fi trecuți printr-un proces transparent de înrolare pe baza atributelor livrate de către ROeID în procesul de autentificare. 
În vederea înrolării în serviciul solicitat, cetățenii vor trebui să acorde un consimțământ privind transferul de date.
Pentru înrolarea în ROeID, după încheierea protocoalelor / acordurilor inter-instituționale,  trebuie parcurse următoarele etape tehnice:

1. Furnizorul de Servicii eGuvernare și Administratorul platformei ROeID vor agrea protocolul de autentificare utilizat (SAML sau OpenID) pentru interconectarea SSO. 
2. Furnizorul de Servicii de eGuvernare și Administratorul platformei ROeID vor agrea:
  - Detaliile procesului de autentificare, conform protocolului utilizat (OpenID / SAML)
  - Atributele transmise de platforma ROeID către Serviciul de eGuvernare după consimțământul utilizatorului și autentificarea cu succes a acestuia, precum și elementul unic prin care utilizatorul să fie identificat în sistemele informatice (pentru a putea realiza identificarea utilizatorilor deja existenți în serviciile eGuvernare se va folosi codul CNP al utilizatorului)
  - Acțiunea Furnizorului de Servicii de eGuvernare în situația în care utilizatorul care se autentifică prin platforma ROeID nu are deja un cont în platforma utilizată de Furnizorul de Servicii de eGuvernare – respectiv dacă pentru utilizator va fi creat automat un cont utilizând atributele/datele primite de la ROeID sau dacă Furnizorului de Servicii de eGuvernare va contacta direct utilizatorul, utilizând atributele/datele primite de la ROeID pentru realizarea procedurii specifice de deschidere a contului
  - Utilizatorii platformei ROeID vor fi informați, pentru fiecare Furnizor de Servicii de eGuvernare, dacă în urma autentificarii cu succes prin ROeID va fi deschis automat un cont de utilizator în platforma Furnizorului de Servicii de eGuvernare sau dacă vor fi contactați ulterior de acesta pentru deschiderea contului.
  
### Procedura de înrolare utilizând protocolul OpenID Connect (OIDC)

Înrolarea Furnizorilor de Servicii de eGuvernare folosind protocolul OpenID Connect (OIDC) implică:
- Stabilirea atributelor (datelor) utilizatorului ce vor fi transmise de ROeID către Furnizorul de servicii eGuvernare
- Stabilirea adreselor web la care se va face comunicarea. Acestea trebuie să fie securizate și trebuie făcut schimb de date între ROeID și furnizorul de servicii pentru a putea cripta/decripta informațiile.

Din punct de vedere al Platformei ROeID integrarea prin protocol OpenID implică realizarea următorilor pași în platforma ROeID:

a)	Se definește maparea de atribute în zona de Claims de la Authorization Provider: Exemplu: telefon<>mobile, prenume<>givenName, etc.

b)	Se creează un profil nou (scope) în Authorization Provider-ul definit deja. Acest profil va conține lista de atribute stabilită anterior. Exemplu:ServiceProviderNameScope:Gender,CurrentAddress, PersonalIdentifier,FirstName,LastName

c)	Se creează un nou client de OpenID în ROeID cu următoarele caracteristici:
- SeviceProviderName– numele furnizorului de servicii
- Client ID – generat automat
- Client Secret – generat automat
- Logo URL: link către logo-ul furnizorului de servicii
- Authorization Provider – IDP
- Scopes: openid (selectat default) + scope-ul definit la pasul anterior
- Redirect URIs – lista cu adresele URL cu care se va face comunicarea. Orice alta cerere pentru client si care nu vine de la aceste url-uri va fi respinsa de catre server
- Use client keys form encryption – la alegerea dezvoltatorilor. Default datele sunt criptate cu certificatele ROeID și acestea trebuie importate în sistemul furnizorului de servicii eGuvernare. Daca se decide folosirea cheilor de la client, atunci acestea trebuie importate in ROeID si setarile sunt stabilite aici.

După ce a fost creat clientul OpenID, se vor genera o serie de informații care trebuie transmise către furnizorul de servicii eGuvernare: 
- Client name
- Client ID
- Client Secret
- Scope names 
- Enpoints (Issuer, uthorization, Token, User Info, Introspection, Revocation, JWK Set, Provider Metadata)


Din punct de vedere al sistemului informatic utilizat de Furnizorii de servicii eGuvernare, integrarea cu Platforma ROeID utilizând protocolul OpenID implică realizarea pașilor următori:

1. Integrarea/dezvoltarea unui plugin de comunicare OpenID. 
2. Majoritatea soluțiilor COTS au deja acest plugin integrat și trebuie doar configurat. Dacă este nevoie de dezvoltarea unui plugin nou (pentru soluțiile dezvoltate custom), atunci se pot folosi librăriile standard ale limbajului de dezvoltare. 
3. Lista cu librăriile și plugin-urile certificate pentru o gamă variată de limbaje de programare și tehnologii este disponibila la adresa: https://openid.net/developers/certified/
4. (Optional) Modificarea procesului de autentificare și autorizare astfel încât să accepte sesiunea SSO și să o transforme in sesiunea de aplicatie. Acest lucru este făcut automat de către pluginul dezvoltat anterior, dar dacă procesul de autentificare necesită si alte verificări care nu au legătură cu datele primite de la ROeID, atunci va fi nevoie de dezvoltări adiționale. De exemplu accesul utilizatorului va fi respins dacă trebuie să fie membru al unui grup care există în cadrul sistemului informatic utilizat de furnizorul de servicii de eGuvernare, iar utilizatorul nu este membru al acestui grup.  
5. (Optional) Actualizarea procesului de creare cont. 
6. Dacă se dorește crearea automată a contului utilizatorului utilizând informațiile primite de la ROeID, atunci va fi nevoie de customizarea aplicației utilizată de furnizprul de servicii eGuvernare. Datele despre utilizator sunt primite de la ROeID prin UserInfo Endpoint și se poate automatiza creearea contului în platforma furnizorului de servicii eGuvernare. Daca este nevoie de date suplimentare care nu sunt diponibile în platforma ROeID, atunci acestea trebuie solicitate de către furnizorul de servicii eGuvernare direct de la utilizator pe baza informațiilor de contact primite (e-mail / telefon etc.).
7. Modificarea paginii de login și adaugarea unui buton/link pentru ”Login prin ROeID” și care să utilizeze pluginul creat/configurat anterior. 

### Procedura de înrolare utilizând protocolul SAML

Înrolarea Furnizorilor de Servicii de eGuvernare în platforma ROeID folosind protocolul SAML 2.0, necesită realizarea următorilor pași în platforma ROeID:

În platforma ROeID există deja definită o entitate ROeID IDP (Identity Provider) pentru SAML 2.0 și trebuie create următoarele:
- Entitate pentru Furnizorul de Servicii eGuvernare
- Federație între entitatea ROeID IDP și entitatea Furnizorului de Servicii eGuvernare nou creată

Pentru a crea o entitate SAML 2.0 pentru Furnizorul de Servicii eGuvernare este necesară realizarea următorilor pași:

1. Creare Partnership Entity 
2. Se definesc Entity ID și Entity Name și se introduc url-urile la
3. Consumer service = url-ul care va procesa datele primite de la ROeID. 
4. SLO service = url-ul pentru logout din soluția Furnizorul de Servicii eGuvernare. Acesta este optional, dar daca dorim ca logout-ul din SSO să facă logout și din soluția utilizată de Furnizorul de Servicii eGuvernare, atunci este obligatoriu
5. Se selectează certificatele care sunt folosite pentru semnarea și criptarea datelor 
6. Se bifează formatul de nume pentru atributul care va face autentificarea. 
7. Se confirmă datele si se salveaza entitatea

După crearea cu succes a entității SAML 2.0 trebuie realizată federația între entitatea ROeID IDP și entitatea Furnizorului de Servicii eGuvernare nou creată. 

- Se configurează lista cu atributele (datele utilizatorului) care vor fi transmise către Furnizorul de Servicii eGuvernare 
- Se configurează opțiunile de semnare/criptare/decriptare prin alegerea valorilor pentru: cheie, algoritm, certificat. De asemenea, se bifează opțiunile: Require Sign Authentication Request, Encrypt Name ID, Encrypt Assertion.
- Se confirmă datele și se salvează/ activează federația astfel configurată.

Din punct de vedere al sistemului informatic utilizat de Furnizorii de servicii eGuvernare, integrarea cu Platforma ROeID prin protocolul SAML implică realizarea pașilor următori:

1. Integrarea/dezvoltarea unui plugin de comunicare SAML 2.0. Majoritatea soluțiilor standard / COTS au deja acest plugin integrat și acesta trebuie doar configurat. 
2. Dacă este nevoie de dezvoltarea unui plugin nou (pentru soluțiile dezvoltate custom), atunci se poate dezvolta un client/plugin nou sau se pot folosi clienți SAML (SAML toolkit) existenți specifici limbajului de dezvoltare a soluției. 
3. Clienți SAML (SAML toolkit) pentru diverse tehnologii sunt disponibili la adresa: https://www.samltool.com/toolkits.php
4. Mai multe informații privind SAML sunt disponibile la https://wiki.oasis-open.org/security/FrontPage 
5. Modificarea procesului de autentificare și autorizare astfel încât să accepte sesiunea SSO și să o transforme în sesiune de aplicație. 
6. (Optional) Modificarea procesului de creare cont. 
7. Dacă se dorește crearea automată a contului utilizatorului utilizând informațiile primite de la ROeID, atunci va fi nevoie de customizarea aplicației utilizată de Furnizorul de Servicii eGuvernare. Datele despre utilizator sunt primite de la ROeID prin atributele din response assertion și se poate automatiza crearea contului în platforma Furnizorului de Servicii eGuvernare. Daca este nevoie de date suplimentare care nu sunt diponibile în platforma ROeID, atunci acestea trebuie solicitate de către furnizorul de servicii eGuvernare direct de la utilizator pe baza informațiilor de contact primite (e-mail / telefon etc.).
8. Modificarea paginii de login și adaugarea unui buton/link pentru ”Login prin ROeID” și care să utilizeze pluginul creat/configurat anterior.

## Expunerea de servicii pe portalul www.roeid.ro si in aplicatia mobila ROeiD

Pentru a adauga un serviciu in pagina aplicatiei mobile si pe portalul web www.roeid.ro furnizorul de servicii va transmite ADR urmatoarele informatii:
1. Nume scurt (12 caractere) care va aparea sub iconita. De exemplu **ghiseul**
2. Logo 300x300 in format PNG, versiune light/dark (pe fundal alb si negru)

![pcue pcue logo_320x320_fundal_alb](https://github.com/roeid-ro/integrare/assets/113096980/36ae9160-bd85-47c2-8fa5-1c4e6dade6f3)
![logo_320x320_fundal_ex_roeid](https://github.com/roeid-ro/integrare/assets/113096980/eadb12e1-3eb2-49a9-b967-474f3e4a6413)

3. Descriere scurta de 60 de caractere a denumirii, de exemplu **Plateste online impozite si taxe catre institutiile publice inrolate in sistem**
4. Descriere lunga, intre 100 si 200 de cuvinte care apare in ecranul afisat la apasarea pe iconita.
5. URL pentru initierea conectarii la sistem, de preferat un URL care face redirect la Roeid login page, ca de exemplu: https://www.ghiseul.ro/ghiseul/public/login-pscid . Scopul este ca la apasarea acestui link utilizatorului sa i se deschida pagina de login unde introduce user/parola urmand ca la finalizare proces sa fie redirectionat in aplicatia dvs.
6. Un redirect_uri spre care sistemul de autentificare sa faca redirect dupa ce autentificarea a fost realizata cu succes

## Date puse la dispozitie de ROeiD si formatul acestora

ROeID pune la dispozitia furnizorilor de servicii datele din buletinul cetateanului, cu acceptul acestuia, la fiecare conectare. Datele puse la dispozitie sunt verificate, si vin pe claims ca de exemplu:

`  "claims_supported": [
    "scara",
    "cnp",
    "judet",
    "nr",
    "prenume",
    "nume complet",
    "nume",
    "bloc",
    "apartament",
    "telefon",
    "etaj",
    "adresa",
    "strada",
    "sector",
    "localitate",
    "email"
  ],
`
Mai multe detalii pot fi vizualizate in fisierele de configurare openID la adrese de tipul: https://sso.roeid.ro/affwebservices/CASSO/oidc/demo/.well-known/openid-configuration

## Identitate vizuala ROeiD

Paleta de culori folosita in ROeiD

- ![#000000](https://placehold.co/15x15/000000/000000.png) `#000000` Black
- ![#09338a](https://placehold.co/15x15/09338a/09338a.png) `#09338a` Button hover
- ![#2e5ae6](https://placehold.co/15x15/2e5ae6/2e5ae6.png) `#2e5ae6` Royal blue
- ![#8298db](https://placehold.co/15x15/8298db/8298db.png) `#8298db` Cornflower blue
- ![#f4f4f4](https://placehold.co/15x15/f4f4f4/f4f4f4.png) `#f4f4f4` Light gray
- ![#f2b62c](https://placehold.co/15x15/f2b62c/f2b62c.png) `#f2b62c` Goldenrod
- ![#f1f3f8](https://placehold.co/15x15/f1f3f8/f1f3f8.png) `#f1f3f8` Ghost white
- ![#d1d9e8](https://placehold.co/15x15/d1d9e8/d1d9e8.png) `#d1d9e8` Levender

In paginile web ale furnizorilor de servicii se vor insera butoane de tip "Conectare cu ROeiD" folosind fisierele regasite aici: https://github.com/roeid-ro/integrare/tree/main/html

Iata un exemplu de vizualizare a butoanelor pe fundal alb si albastru. Culorile propuse sunt din paleta ROeiD dar imaginile in format SVG precum si fontul permit integrarea in orice alta paleta de culori a siturilor partenere. Recomandam folosirea logo-ului primar pentru fundal deschis la culoare si a celui secundar pentru fundal inchis la culoare. Se va afisa textul `Conectare cu ROeID` intocmai in continutul butonului.

Border, border radius, border, precum si celelalte elemente de stil se vor implementa in concordanta cu identitatea vizuala a sitului web al furnizorului de servicii, fara a acoperi textul sau logo-ul ROeiD.

![image](https://user-images.githubusercontent.com/113096980/236464864-6d572607-00e2-4cc4-bf2c-5443f6dc6d97.png)


Fonturile folosite in aplicatia mobila si in situl web ROeiD sunt din familia Sora, accesibil aici: https://fonts.googleapis.com/css2?family=Sora:wght@100;200;300;400;500;600;700;800&display=swap

Logo-ul ROeiD in varianta color negru-albastru si alb negativ il gasiti aici:
- https://github.com/roeid-ro/integrare/blob/main/html/roeid_logo_primary.svg
- https://github.com/roeid-ro/integrare/blob/main/html/roeid_logo_secondary.svg


## Mediul de test ROeID

Pentru a veni in sprijinul furnizorilor de servicii in perioada premergatoare inrolarii am creat un mediu complet de dezvoltare și integrare disponibil pentru a fi folosit in teste.

In acest scop am creat o aplicatie de inrolare utilizatori test si autentificare cu al doilea factor (push notification) speciala pentru mediul de test, versiune valabila doar pentru Android. O puteti downloada de la urmatoarea adresa web :

[https://drive.google.com/drive/folders/1wnWUCj61-x-LSnSpgq2LZflMHQQy0xPz?usp=sharing](https://drive.google.com/drive/folders/1wnWUCj61-x-LSnSpgq2LZflMHQQy0xPz?usp=sharing)

**Testarea in mediul de test se face doar cu aplicatia mentionata mai sus**, aplicatia disponibila in Google Play si Apple Store nu poate fi folosita pe mediul de test

**Pe mediul de test se aproba automat identitatea digitala daca se prezinta un SELFIE similar cu imaginea din buletin**

Dupa inrolare, puteti in continuare testa integrarea cu mediul dvs. folosind utilizatorii inrolati, precum puteti testa si autentificarea in site-ul demo creat pentru a simula integrarea: http://demo.beta.roeid.ro/sts


## Pasi prin care un browser se autentifica in ROeID pornind de la situl furnizorului de servicii

Recomandam sa urmariti acesi pasi in browser pornind de la mediul de test al ghiseul.ro, puteti folosi consola de debugging a browserului Chrome activand optiunea de pastrare a logului activitatii de retea. Veti intelege mai bine la ce sunt folositi parametrii: response_type, redirect_uri, scope. De asemenea va recomandam sa verificati configuratia .well-known a clientului dvs.

```
1. GET https://www.ghiseul.ro/testare/ghiseul

2. GET https://www.ghiseul.ro/testare/ghiseul/public/login-pscid 

3. GET https://sso.beta.roeid.ro/affwebservices/CASSO/oidc/ghiseul/authorize?response_type=code&client_id=ghiseul&scope=openid+profile+ghiseul&state=1698308536-441d47&redirect_uri=https%3A%2F%2Fwww.ghiseul.ro%2Ftestare%2Fghiseul%2Fpublic%2Flogin-pscid%2Findex

4. GET https://sso.beta.roeid.ro/affwebservices/secure/secureredirect?response_type=code&client_id=ghiseul&scope=openid+profile+ghiseul&state=1698308536-441d47&redirect_uri=https%3A%2F%2Fwww.ghiseul.ro%2Ftestare%2Fghiseul%2Fpublic%2Flogin-pscid%2Findex&SMPORTALURL=x4%2FhHg3RAgUeQLhibKTJjbh4XcmhnbiyABkvshd1Fq2U4uBSDrRIcjKofI2U2MC69KmkY42AmSChet9AIN861tJ21Tled%2FV04A9axdvVSPqBq67qGh2vEdyNGItnVklo

5. GET https://sso.beta.roeid.ro/siteminderagent/forms/shim.fcc?TYPE=33554433&REALMOID=06-0005a1dd-fc2d-132d-abb5-04140a000000&GUID=&SMAUTHREASON=0&METHOD=GET&SMAGENTNAME=accessgateway&TARGET=-SM-HTTPS%3a%2f%2fsso%2ebeta%2eroeid%2ero%2faffwebservices%2fsecure%2fsecureredirect%3fresponse_type%3dcode%26client_id%3dghiseul%26scope%3dopenid%2bprofile%2bghiseul%26state%3d1698308536--441d47%26redirect_uri%3dhttps-%3A-%2F-%2Fwww%2eghiseul%2ero-%2Ftestare-%2Fghiseul-%2Fpublic-%2Flogin--pscid-%2Findex%26SMPORTALURL%3dx4-%2FhHg3RAgUeQLhibKTJjbh4XcmhnbiyABkvshd1Fq2U4uBSDrRIcjKofI2U2MC69KmkY42AmSChet9AIN861tJ21Tled-%2FV04A9axdvVSPqBq67qGh2vEdyNGItnVklo
-- aici este afisata pagina cu user/parola

6. POST https://sso.beta.roeid.ro/arcotafm/MasterController.jsp?profile=ldapandpush&tokenID=41ff090076adb6ac26710bf579125c78e8535d99&SMAUTHREASON=27&SMAGENTNAME=accessgateway&TARGET=-SM-HTTPS%3a%2f%2fsso%2ebeta%2eroeid%2ero%2faffwebservices%2fsecure%2fsecureredirect%3fresponse_type%3dcode%26client_id%3dghiseul%26scope%3dopenid%2bprofile%2bghiseul%26state%3d1698308536--441d47%26redirect_uri%3dhttps-%3A-%2F-%2Fwww%2eghiseul%2ero-%2Ftestare-%2Fghiseul-%2Fpublic-%2Flogin--pscid-%2Findex%26SMPORTALURL%3dx4-%2FhHg3RAgUeQLhibKTJjbh4XcmhnbiyABkvshd1Fq2U4uBSDrRIcjKofI2U2MC69KmkY42AmSChet9AIN861tJ21Tled-%2FV04A9axdvVSPqBq67qGh2vEdyNGItnVklo
-- aici se afiseaza mesajul de asteptare pentru 2FA PUSH

7. GET https://sso.beta.roeid.ro/arcotafm/getOTTFromPushSMToken.jsp?txnId=89b5c46e70f4b3876de8f663114592e2f51a95d9
7. GET https://sso.beta.roeid.ro/arcotafm/getOTTFromPushSMToken.jsp?txnId=89b5c46e70f4b3876de8f663114592e2f51a95d9
7. GET https://sso.beta.roeid.ro/arcotafm/getOTTFromPushSMToken.jsp?txnId=89b5c46e70f4b3876de8f663114592e2f51a95d9
-- imediat dupa autorizare din aplicatia mobila se redirecteaza

8. POST https://sso.beta.roeid.ro/arcotafm/MasterController.jsp?profile=ldapandpush&tokenID=7328f07ba3b60f604961eb9f5802c9c297af8a1e&SMAUTHREASON=27&SMAGENTNAME=accessgateway&TARGET=-SM-HTTPS%3a%2f%2fsso%2ebeta%2eroeid%2ero%2faffwebservices%2fsecure%2fsecureredirect%3fresponse_type%3dcode%26client_id%3dghiseul%26scope%3dopenid%2bprofile%2bghiseul%26state%3d1698308536--441d47%26redirect_uri%3dhttps-%3A-%2F-%2Fwww%2eghiseul%2ero-%2Ftestare-%2Fghiseul-%2Fpublic-%2Flogin--pscid-%2Findex%26SMPORTALURL%3dx4-%2FhHg3RAgUeQLhibKTJjbh4XcmhnbiyABkvshd1Fq2U4uBSDrRIcjKofI2U2MC69KmkY42AmSChet9AIN861tJ21Tled-%2FV04A9axdvVSPqBq67qGh2vEdyNGItnVklo

9. GET https://sso.beta.roeid.ro/affwebservices/CASSO/oidc/userconsent
--- aici se afiseaza cetateanului consimtamantul pentru datele trimise

10. POST https://sso.beta.roeid.ro/affwebservices/CASSO/oidc/userconsent
-- se trimite consentul

11. GET https://sso.beta.roeid.ro/affwebservices/CASSO/oidc/ghiseul/authorize?SMASSERTIONREF=QUERY&response_type=code&client_id=ghiseul&scope=openid+profile+ghiseul&state=1698308536-441d47&redirect_uri=https%3A%2F%2Fwww.ghiseul.ro%2Ftestare%2Fghiseul%2Fpublic%2Flogin-pscid%2Findex
-- se pregateste redirectarea la redirect_uri

12. GET https://www.ghiseul.ro/testare/ghiseul/public/login-pscid/index?code=YjAxOWJlZDYtYzdiZi00ZGZmLWFhZTQtNjU4YThiYTJhMTFkLStON3lLN3dKQ2NYRmY3TWh3QUlaR2czMmdPWT0%3D&state=1698308536-441d47
-- Redirect final pe situl web al furnizorului de servicii sore redirect_uri furnizat de catre acesta.
```

## Cazuri pe care trebuie sa le tratati

Pentru toate conturile care vin din ROeID va recomandam sa stocati undeva acest atribut astfel incat sa puteti sti oricand faptul ca este un cont ROeID. Contul ROeID presupune ca cetateanul a fost verificat de la distanta si este cel care pretinde ca este. Accesul la datele unui cont ROeID pe alte canale decat prin autentificarea ROeID se considera o bresa de securitate si nu ar trebui permisa.

### La fiecare login prin ROeID

Va recomandam sa preluati informatiile care vin pe atributele userului in OpenID si sa le stocati in baza dvs. de date. Utilizatorul a fost informat si si-a dat acceptul pentru acest lucru. 
Unele softuri de tip Identity Management (ex. Keycloak) au o optiune de a importa aceste atribute de fiecare data si a le salva in baza proprie de date. Mai jos este prezentat un exemplu in acest sens.

### Daca este un utilizator nou

Va recomandam sa il creati in baza de date iar ulterior sa il conduceti in ecranul unde trebuie sa isi completeze alte date solicitate de sistemul dvs, sau sa verifice corectitudinea datelor preluate din ROeID. Daca sistemul dvs. presupune introducerea de date suplimentare fata de ce primiti din cartea de identitate este recomandat sa nu marcati profilul userului ca fiind complet decat dupa ce face acest lucru.

### Daca este un utilizator existent

Va recomandam sa il asociati cu utilizatorul din baza dvs de date, stocand intr-o coloana separata faptul ca este user provenit din roeid. Asocierea se poate face fie pe CNP fie pe email. Sistemul ROeID verifica faptul ca emailul apartine acestei identitati, confirmand cu cod OTP trimis pe email in cadrul procesului de inrolare.

Acest comportament este implementat pe situl https://www.ghiseul.ro

### Daca in sistemul dvs. sunt mai multe conturi asociate acelui CNP

Va recomandam sa asociati conturile pornind de la email

### Daca in sistemul dvs. pe un cont de cetatean exista mai multe conturi de persoane fizica si juridice

Va recomandam sa afisati imediat dupa login pagina in care cetateanul sa selecteze mai departe ce subcont doreste sa foloseasca.
Puteti vedea un exemplu functional la PCUe: https://edirect.e-guvernare.ro/

## Logout

Va recomandam sa implementati functie de logout doar pe sesiunea de browser din situl dvs, daca utilizatorul doreste sa se reconecteze o poate face apasand butonul "Autentificare cu ROeID", fara sa mai introduca user/parola.

Pentru a face logout din ROeID se redirecteaza browserul la adresa de tipul  

```
https://sso.beta.roeid.ro/logout?target=https://dev.mydemosystem.ro
```

astfel sesiunea ROeID este stearsa iar borwserul retrimite utilizatorul spre pagina principala a sitului dv.

**ATENTIE** logout din ROeID face logout in toate siturile care sunt inrolate in ROeID, utilizatorul va trebui sa introduca din nou user/parola si accept pe telefonul mobil. 
**LOGOUT** se va afisa clar cu buton pe situl dvs **Deconectare din ROeID** pentru a informa utilizatorul ca nu mai este logat in tot sistemul ROeID.

Recomandam sa afisati **Deconectare din ROeID** doar pentru calculatoare de tip shared/kiosk unde se presupune ca sunt utilizatori diferiti care pot accesa sistemul dvs.


## Tips & Tricks pentru dezvoltatori software

Presupunand ca dezvoltati un sistem informatic pe care doriti sa il integrati cu ROeID este obligatoriu sa alegeti un nume de domeniu pe care sa testati integrarea sistemului dvs. Spre exemplu **dev.mydemosystem.ro**. 

Daca lucrati pe un server dedicat este util ca acest DNS (dev.demosystem.ro) sa fie accesibil de pe calculatorul dvs prin intermediul **HTTPS** cu un certificat **TRUSTED** astfel incat sa nu fie probleme cu browserul. Daca lucrati pe **localhost** gazduind situl pe propriul dvs. calculator va recomandam sa folositi utilitarul **caddy** pentru a implementa un reverse proxy SSL trusted.

https://caddyserver.com/docs/quick-starts/reverse-proxy

Iata un exemplu de parametri de pornire pentru caddy pentru a trimite tot traficul spre aplicatia dvs pornita pe portul 3000.
```
caddy reverse-proxy --from dev.mydemosystem.ro:443 --to locahost:3000
```

Astfel din browserul dvs veti putea accesa la adresa https://dev.mydemosystem.ro:443  situl la care lucrati.

Nu uitati in HOSTS sa configurati adresa dev.mydemosystems.ro sa fie tot localhost sau IP-ul serverului unde hostati serverul (in Windows c:\Windows\System32\Drivers\etc\hosts si in Linux /etc/hosts)

Integrarea cu roeid presupune din partea dvs. crearea unei pagini spre care ROeID sa trimita browserul dupa ce procesul de autentificare s-a terminat cu succes, pagina regasita in documentatie sub numele de **redirect_uri**. Aceasta adresa se transmite echipei tehnice atat pentru mediul de test cat si pentru cel de productie, putand avea mai multe adrese simultan pentru ambele medii.

### Exemplu de configurare Java Application Server cu Keycloak filter

Acest exemplu provide de la un furnizor de servicii care are aplicatia existenta hostata in application server WAS la care a atasatat backend keycloak pentru autentificare utilizatori

keycloak-servlet-filter-adapter-18.0.2.jar

![Imagine WhatsApp 2023-10-26 la 12 07 40_fe38ccd7](https://github.com/roeid-ro/integrare/assets/113096980/c7c711d5-8886-4335-852c-6a6348c5de50)
![Imagine WhatsApp 2023-10-26 la 12 08 56_6d6abc30](https://github.com/roeid-ro/integrare/assets/113096980/dd40dddd-adc9-437f-83eb-c9fa87d08246)

in cod pentru extragere principal folosim:

![Imagine WhatsApp 2023-10-26 la 12 12 02_a86493a4](https://github.com/roeid-ro/integrare/assets/113096980/e915bbbe-daa6-42b2-b135-af41b0ce6bd4)

Pentru extragere claims folosim:
```
String email = keycloakPrincipal.getKeycloakSecurityContext().getIdToken().getPreferredUsername();
String cnp = keycloakPrincipal.getKeycloakSecurityContext().getIdToken().getOtherClaims().get("cnp");
```

### Exemplu de configurare pentru Keycloak

![image](https://github.com/roeid-ro/integrare/assets/113096980/45cf9ffe-8b06-4666-ac32-92dbc0327ace)

![image](https://github.com/roeid-ro/integrare/assets/113096980/06f67b48-92c4-4529-b165-2344e30d7063)

![image](https://github.com/roeid-ro/integrare/assets/113096980/024b373d-4508-423d-aeb9-91d487c0cd5c)

![image](https://github.com/roeid-ro/integrare/assets/113096980/d3301999-3256-456b-81ea-5ab90c39f707)

![image](https://github.com/roeid-ro/integrare/assets/113096980/d39c598f-5cac-4841-bbec-71168ec50317)
Se vor complata toate atributele folosite

Atentie la setarile atributelor, conform imaginii de mai jos:

![image](https://github.com/roeid-ro/integrare/assets/113096980/9a18a030-87b2-4af9-aafc-833f16804aaf)


La fiecare login din ROeID in Keycloack se vor prelua automat aceste atribute ca in exemplul de mai jos.

![image](https://github.com/roeid-ro/integrare/assets/113096980/3894cde4-8795-40b6-9ebd-256c4c4ad451)

### Exemplu de integrare cu node.js

Codul de mai jos este cu titlu de exemplu, nu functioneaza daca faceti copy paste, mai are nevoie de initializari specifice node.js cu middleware express

```
const openId = require('express-openid-connect')

const openIdConfig = {
  session: {
    store: new RedisStore({ client: cache.redisLegacy })
  },
  authRequired: false,
  routes: {
    // Override the default login route to use your own login route as shown below
    login: false,
    // Pass a custom path to redirect users to a different
    // path after logout.
    callback: '/sso'
  },
  authorizationParams: {
    response_type: constants.OPENID_RESPONSE_TYPE, // This requires you to provide a client secret
    scope: constants.OPENID_SCOPE,
    response_mode: constants.OPENID_RESPONSE_MODE
  },
  baseURL: constants.OPENID_BASEURL,
  clientID: constants.OPENID_CLIENTID,
  issuerBaseURL: constants.OPENID_ISSUER,
  secret: constants.OPENID_SECRET,
  clientSecret: constants.OPENID_CLIENTSECRET
}
router.use(openId.auth(openIdConfig))

// Middleware to make the `user` object available for all views
router.use(function (req, res, next) {
  res.locals.user = req.oidc.user
  next()
})

router.get('/', (req, res) => {
  res.send('<a href="/api/v2be/admin">Admin Section</a>')
})

router.get(
  '/login',
  tryCatch((req, res) =>
    res.oidc.login({
      returnTo: '/public/app',
      authorizationParams: {
        redirect_uri: constants.OPENID_REDIRECT,
        response_type: 'id_token', // This requires you to provide a client secret
        scope: 'openid bapi', // bapi is provided by ROeID for this client id
        response_mode: 'form_post'
      }
    })
  )
)

router.get(
  '/admin',
  openId.requiresAuth(),
  tryCatch((req, res) => {
    res.send(`Hello ${req.oidc.user.sub}, this is the admin section.`)
    // ...
  })
)

router.post(
  '/sso',
  express.urlencoded({ extended: false }),
  tryCatch((req, res) =>
    res.oidc.callback({
      returnTo: '/public/app',
      redirectUri: constants.OPENID_REDIRECT,
      authorizationParams: {
        response_type: 'id_token', // This requires you to provide a client secret
        scope: 'openid bapi',
        response_mode: 'form_post'
      }
    })
  )
)

router.post('/protectedRoute', openId.requiresAuth(), async function (req, res) {
  const email = req?.oidc?.user?.email

```

### Exemplu de integrare cu Moodle

![image](https://github.com/roeid-ro/integrare/assets/113096980/abe55371-a17f-41f3-b2aa-3e441b88a3e5)

![image](https://github.com/roeid-ro/integrare/assets/113096980/6d1d63b9-a4ef-4ce8-8132-5a9ed0df8b19)

![image](https://github.com/roeid-ro/integrare/assets/113096980/35163785-5d7a-49f5-960a-9f4fc8717efa)

![image](https://github.com/roeid-ro/integrare/assets/113096980/cb84ca8d-c759-45ff-a2ec-a39975916dfe)

### Exemplu de configurare cu Ory Kratos (self-hosted)

Pentru a configura ROeID ca si Generic Provider intr-un sistem bazat pe solutia Ory Kratos self-hosted, pe langa celalalte campuri obligatorii, va fi necesara adaugarea urmatoarei sectiuni in fisierul de tip "Ory Kratos Configuration File":

```yaml
selfservice:
  methods:
    oidc:
      config:
        providers:
          - id: < UN ID UNIC, roeid >
            provider: generic
            client_id: <CLIENT_ID>
            client_secret: <CLIENT_SECRET>
            issuer_url: <ISSUER_URL>
            mapper_url: <PATH SAU BASE64 JSONNET>
            scope:
              - openid <SCOPE>
      enabled: true
```

Model schema de identitate (se poate modifica dupa nevoile proiectului):

```yaml
{
  "$id": "https://schemas.ory.sh/presets/kratos/quickstart/email-password/identity.schema.json",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Persoana Fizica",
  "type": "object",
  "group": "default",
  "properties":
    {
      "traits":
        {
          "type": "object",
          "group": "default",
          "properties":
            {
              "email":
                {
                  "type": "string",
                  "format": "email",
                  "title": "E-Mail",
                  "ory.sh/kratos":
                    {
                      "verification": { "via": "email" },
                      "recovery": { "via": "email" },
                      "credentials": { "password": { "identifier": true } },
                    },
                },
              "data_nasterii":
                {
                  "type": "string",
                  "title": "Data Nașterii",
                  "format": "date",
                },
              "name":
                {
                  "type": "object",
                  "required": ["last", "first"],
                  "properties":
                    {
                      "first": { "title": "Prenume", "type": "string" },
                      "last": { "title": "Nume", "type": "string" },
                    },
                },
              "cnp": { "type": "string", "title": "CNP", "maxLength": 13 },
              "telefon": { "type": "string", "title": "Telefon" },
              "adresa":
                {
                  "type": "object",
                  "required": ["localitate", "judet"],
                  "properties":
                    {
                      "strada": { "type": "string", "title": "Strada" },
                      "nr": { "type": "string", "title": "Număr" },
                      "bloc": { "type": "string", "title": "Bloc" },
                      "scara": { "type": "string", "title": "Scara" },
                      "etaj": { "type": "string", "title": "Etaj" },
                      "apt": { "type": "string", "title": "Apartament" },
                      "localitate": { "type": "string", "title": "Localitate" },
                      "judet": { "type": "string", "title": "Județ" },
                      "cod_postal":
                        {
                          "type": "string",
                          "title": "Cod Poștal",
                          "maxLength": 6,
                        },
                    },
                },
              "act":
                {
                  "type": "object",
                  "properties":
                    {
                      "tipact": { "type": "string", "title": "Tip Act" },
                      "serie": { "type": "string", "title": "Serie" },
                      "autoritatea_emitenta":
                        { "type": "string", "title": "Eliberat de" },
                      "valabilitate_document":
                        {
                          "type": "string",
                          "title": "Valabil Până La",
                          "format": "date",
                        },
                    },
                },
            },
          "required": ["email"],
          "additionalProperties": false,
        },
    },
}
```

Model jsonnet pentru maparea schemei de identitate de mai sus pe atributele ROeID:

```yaml
local claims = std.extVar('claims');

{
    identity: {
        traits: {
            name: {
                [if "nume" in claims.raw_claims then "first" else null]: claims.raw_claims.nume,
                [if "prenume" in claims.raw_claims then "last" else null]: claims.raw_claims.prenume,
            },
            email: claims.email,
            [if "cnp" in claims.raw_claims then "cnp" else null]: claims.raw_claims.cnp,
            [if "telefon" in claims.raw_claims then "telefon" else null]: claims.raw_claims.telefon,
            verified: claims.verified,
            [if "datanasterii" in claims.raw_claims then "data_nasterii" else null]: claims.raw_claims.datanasterii,
            adresa: {
                [if "strada" in claims.raw_claims then "strada" else null]: claims.raw_claims.strada,
                [if "bloc" in claims.raw_claims then "bloc" else null]: claims.raw_claims.bloc,
                [if "nr" in claims.raw_claims then "nr" else null]: claims.raw_claims.nr,
                [if "etaj" in claims.raw_claims then "etaj" else null]: claims.raw_claims.etaj,
                [if "apartament" in claims.raw_claims then "apt" else null]: claims.raw_claims.apartament,
                [if "localitate" in claims.raw_claims then "localitate" else null]: claims.raw_claims.localitate,
                [if "judet" in claims.raw_claims then "judet" else null]: claims.raw_claims.judet
            },
            act: {
                [if "tipact" in claims.raw_claims then "tipact" else null]: claims.raw_claims.tipact,
                [if "serie" in claims.raw_claims then "serie" else null]: claims.raw_claims.serie,
                [if "autoritateaemitenta" in claims.raw_claims then "autoritatea_emitenta" else null]: claims.raw_claims.autoritateaemitenta,
                [if "valabilitatedocument" in claims.raw_claims then "valabilitate_document" else null]: claims.raw_claims.valabilitatedocument
            }
        }
    }
}
```




