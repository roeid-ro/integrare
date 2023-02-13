# Detalii despre integrarea cu sistemul ROeiD

## Modelul de incredere pentru SSO

În vederea realizării parteneriatului între Platforma ROeID și Furnizorii de Servicii de eGuvernare este necesara stabilirea unor acorduri / protocoale de colaborare pentru asigurarea încrederii și transferului de identități și credențiale de autentificare între Platforma ROeID și sistemele eGuvernare. Încheierea acestor acorduri / protocoale va permite Furnizorilor de Servicii eGuvernare să ia decizii privind accesul în sistemele proprii pe baza nivelului de încredere definit în cadrul Platformei ROeID![image](https://user-images.githubusercontent.com/113096980/215775261-e123ff17-dd47-4443-a14c-76610c82836a.png)

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

## Date puse la dispozitie de ROeiD si formatul acestora

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

