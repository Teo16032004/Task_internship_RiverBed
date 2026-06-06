# NOTES — Teodor Suteu


## 1. Bug-urile găsite

Pentru fiecare bug, scrie 2-3 propoziții:

### Bug #1
- **Unde era:** Fiser main.py, linia 32
- **Cum l-am găsit:** primul test care pica: (test_create_event_returns_201) returneaza 200 in loc de 201
- **Cum l-am fixat:** am scris in parametrii la functia de post si status_code = 201

### Bug #2
- **Unde era:** Fisier storage.py, linia 51
- **Cum l-am găsit:** cele doua teste de list_events test_list_events_includes_created_items si test_list_events_paginates_without_overlap care pica din cauza la return ul din storage.py
- **Cum l-am fixat:** practic acea functia de list_events din storage returneaza o lista care evident e indexata de la 0. dar acolo e returnat cu offset=0 adunat cu 1, deci porneste de la al doilea element si la fel si la liminta superioara nu trebuie adunat 1. deci return ul devine return all_events[offset : offset+ limit] si asa se rezolva ambele teste

### Bug #3
- **Unde era:** Fisier storage.py, functiile de list_events si soft_delete_event
- **Cum l-am găsit:** testele de delete events nu functionau si asa mi-am dat seama ca ar trebui sa schimb ceva in logica de stergere din storage
- **Cum l-am fixat:** Initial m-am gandit sa sterg direct din dictionarul de events id-ul la event si gata se rezolva tot. intr-adevar testele treceau in acest fel dar care mai era sensul la cuvantul soft in soft_delete... atunci am zis ca practic trebuie sa verific daca eventul a primit o valoare la campul deleted_at si in cazul acela insemna ca ar fi sters. deci in list_events() nu il mai afisam, iar in soft_delete_event() returnai None daca era deja sters pentru a putea primi codul de eroare 404 ca nu il mai gaseste.

---

## 2. Endpoint-ul nou

- **Decizii de design:** Am adaugat in storage metoda list_storage_events care primeste ca parametri user_id si un string since Optional si returneaza o lista de evenimente, apoi am facut filtrarea sa pun in lista userului doar evenimentele care coincid cu id-ul lui si care nu au fost sterse cumva inainte. daca exista parametrul since se filtreaza si dupa el si verifica daca data de created_at este mai mare decat since_date si apoi returnam lista cu evenimentele user-ului
In main in functia de get a aplicatiei se verifica daca user-ul exista si apoi se returneaza lista
- **Cazuri edge pe care le-ai acoperit:** parametru since absent, soft deleted events, user inexistent
- **Teste adăugate:** 1.Test care verifica ca returneaza toate evenimentele daca parametrul since lipseste, 2.Test care verifica ca evenimente sterse cu soft delete nu apar in lista returnata. 
3.Test care verifica daca returneaza 404 daca userul cu id-ul cerut nu exista
4.Test care verifica daca se returneaza corect si datele cu parametrul since

---

## 3. Folosirea AI-ului

- **Ce ai folosit:** Copilot
- **Prompturi reprezentative folosite:** Prompt: poti sa ma ajuti sa fac 4 teste pentru aceasta noua metoda unul in care sa listezi fara since date, unul in care sa listezi fara since date dar acel event sa fie deleted, unul in care sa nu existe userul si unul in care sa listezi cu since day 
- **Unde te-a ajutat cel mai mult:** La construirea testelor, nu stiu sintaxa de python asa de bine si atunci i-am zis ce teste trebuie facute si l-am rugat sa ma ajute sa le fac
- **Unde te-a încurcat sau ți-a dat un răspuns greșit:** Initial s-a incurcat cu formatul datelor si ultimul test nu trecea
- **Cum ai verificat ce-a generat:** Am rulat testele si ziceau ca au dat pass, am verificat si in /docs cu operatii manuale sa vad daca totul functioneaza cum ar trebui


---

## 4. Ce-ai face cu mai mult timp

1.O baza de date reala orice SQL nu prea conteaza, ceva in care sa stochezi userii si evenimentele pentru persistenta ca atunci cand dai run la aplicatie sa nu mai trebuiasca mereu sa creezi useri si evenimente noi
2.Autentificarea userilor
3.Un Frontend minimal care sa te ajute sa vezi si sa testezi mai usor totul

---

## 5. Întrebări / observații

Mi-a placut mult sa lucrez la aceasta tema/aplicatie. Am vazut cum functioneaza un backend de FastAPI in python si m-am familizat cu sintaxa (sunt obisnuit cu Java SpringBoot)
