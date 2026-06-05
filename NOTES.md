# NOTES — [Numele tău]

Copiază acest fișier ca `NOTES.md` și completează-l.

Vrem să fie scurt — maxim 1 pagină. Mai mult contează claritatea decât lungimea.

---

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

- **Decizii de design:** (ce-ai considerat? ce ai ales și de ce?)
- **Cazuri edge pe care le-ai acoperit:**
- **Teste adăugate:** (ce verifică fiecare)

---

## 3. Folosirea AI-ului

Fii cinstit. Nu pierzi puncte dacă spui adevărul, dimpotrivă.

- **Ce ai folosit:** (ChatGPT / Cursor / Copilot / altele)
- **Prompturi reprezentative folosite:** (scrie prompturile pe care le consideri relevante + context scurt: la ce te-au ajutat)
- **Unde te-a ajutat cel mai mult:**
- **Unde te-a încurcat sau ți-a dat un răspuns greșit:** (foarte interesant pentru noi!)
- **Cum ai verificat ce-a generat:**
- **Anexă opțională — export chat:** (dacă vrei, poți adăuga un export de chat relevant)

---

## 4. Ce-ai face cu mai mult timp

(Lista scurtă, 3-5 puncte. Arată-ne că ai văzut limitele actuale.)

---

## 5. Întrebări / observații

(Orice nu a fost clar, orice ai vrea să discuți cu noi.)
