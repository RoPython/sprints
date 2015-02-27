Sprint RoPython #3 - 28 februarie 2015
======================================

Mai jos sunt o serie de issues ușoare, potrivite pentru cei care nu știu 
code base-ul Pylint prea mult, precum și o listă de funcționalități  dorite. 
	 
Dacă nu găsiți ceva interesant aici, mult mai multe issues și lucruri de făcut se găsesc pe bug
trackerul nostru: https://bitbucket.org/logilab/pylint/issues?status=new&status=open



Issues ușoare
-------------
 
 * https://bitbucket.org/logilab/pylint/issue/422/create-a-ci-gate-for-analyzing-pylint  
 
Dacă  rulăm pylint peste code base-ul pylint, rezultă o mulțime de erori și  mesaje.
Vrem să adăugăm un pas de bootstrapping în tox.ini, care să  ruleze pylint peste pylint.
Pentru că sunt prea multe probleme de  rezolvat, acest lucru trebuie făcut iterativ, astfel:

  - adăugăm un pylintrc în repo, care să aibă toate mesajele dezactivate
  - rezolvăm toate problemele de un anumit tip, de ex. trailing-whitespaces
  - activăm mesajul în pylintrc
  
  
* https://bitbucket.org/logilab/pylint/issue/431/arguments-differ-should-allow-addition-of

Trebuie  adăugat un flag prin care argumentele opționale să nu fie luate în  considerare atunci
când se verifică checkul arguments-differ (dacă o  funcție A are doi parametri a și b, atunci o
funcție dintr-o subclasă B  ar trebui să aibă aceeași parametri ca principiul LSP să fie respectat.  
Dacă a doua funcție are în schimb argumente opționale, în cel mai bun  caz putem ignora acest lucru printr-un flag)  


* https://bitbucket.org/logilab/pylint/issue/96/color-on-windows-cmd-prompt


* https://bitbucket.org/logilab/pylint/issue/253/a-checker-that-verifies-that-__deepcopy__

Issue-ul  este doar pentru __deepcopy__, însă poate fi extins pentru toatele  metodele speciale din Python. 
Ideea este să existe un checker care să  verifice dacă metodele speciale definite într-o clasă sunt 
corespunzătoare (au argumentele corecte pentru moment).


* https://bitbucket.org/logilab/pylint/issue/327/explicit-call-of-magic-method-should-give

Trebuie adăugat un warning care să fie emis când Pylint întâlnește apeluri de genul `` x = obj.__setattr__(...)``

  
* https://bitbucket.org/logilab/pylint/issue/367/tries-to-open-emacs-locks
 
*  https://bitbucket.org/logilab/pylint/issue/388/promote-a-couple-of-warnings

Warning-urile  respective pot fi upgradate la erori. 
Asta înseamnă că din Wxxx devin  Exxx. Pentru că trebuie menținută compatibilitatea cu versiunile  precedente, 
mesajele noi trebuie să aibă intrarea old_names cu numele  vechi.
Un exemplu: https://bitbucket.org/logilab/pylint/src/9e46aa88e650dae51696d3fbccade9914b9b3a3f/checkers/python3.py?at=default#cl-220
Totodată, ar trebui să avem și teste pentru mesajele schimbate.

* https://bitbucket.org/logilab/astroid/issue/68/add-tests-for-the-brain-plugins

Nu  toate pluginurile de astroid brain sunt testate. Ar trebui adăugate  teste care să valideze
că pluginurile merg. A se vedea  astroid.test.unittest_brain pentru exemple de teste.


* https://bitbucket.org/logilab/pylint/issue/412/accessing-instance-members-in-a-mixin

Anumite mesaje nu trebuie emise dacă clasa în care se află metoda sau atributul respectiv este un Mixin.

* https://bitbucket.org/logilab/pylint/issue/449/require-or-allow-providing-justification

Trebuie adăugat suport pentru posibilitatea de a continua un pragma message cu ;.
Următoarele trebuie permise:
    
      # pylint: disable=msg
      # pylint: disable=msg;
      
Practic, găsirea unui pragma message se va opri după primul ; găsit sau la finalul liniei, astfel încât,
utliizatorii care doresc, pot continua linia cu un mesaj de justificare.
   duplicate: https://bitbucket.org/logilab/pylint/issue/216/would-like-to-be-able-to-put-additional
   
* https://bitbucket.org/logilab/pylint/issue/104/add-switch-to-change-column-offset-from-0

* https://bitbucket.org/logilab/pylint/issue/81/pyreverse-detect-propertys-as-class

pyreverse este o unealtă pe care pylint o pune la dispoziție pentru generarea
de diagrame UML dintr-un cod sursă. Trebuie modificat astfel încât proprietățile
să fie detectate ca atribute, nu funcții.

* https://bitbucket.org/logilab/pylint/issue/437/false-positive-e1601-print-statement-used

print-statement este un mesaj ce este emis în cadrul verificatorului de compatibilitate
python3.py. Acesta se activează prin flagul --py3k și toate mesajele vor fi dezactivate,
mai puțin ale lui. Bugul de mai sus este legat de alt mod, error mode, activat prin flagul -E / --errors-only.
Acesta va dezactiva toate mesajele care nu sunt erori, dar nu se uită să vadă dacă o parte
din erorile activate pot fi emise în mod normal.

* https://bitbucket.org/logilab/pylint/issue/420/html-report-unicodeencodeerror

* https://bitbucket.org/logilab/pylint/issue/215/document-msg-template-as-replacement-for

* https://bitbucket.org/logilab/pylint/issue/379/installing-pylint-via-setuptools-as-a

Trebuie investigată folosirea opțiunii data_files pentru instalarea testelor.
   
   
Alte functionalități dorite (și alte issues mai grele)
------------------------------------------------------

- portarea testelor funcționale pe noua platformă
 Avem   două modalități de a face teste funcționale, prima oară în pylint.test.input e varianta veche,
 unde fișierele de input sunt separate de cele de output, aflate în test.messages și a doua oară avem
 pylint.functional, unde fișierele de input sunt laolaltă cu cele de output.
 Ele mai au și alte îmbunătățiri, cum ar fi faptul că poți   specifica într-un fișier ce mesaje trebuie emise.
 Vrem să portăm cât  mai  multe teste din primul format pe ultimul format, primul dispărând  după  asta.
 
 
- de îmbunătățit documentația

  * diferențe între Pylint și Pyflakes sau alte verificatoare
  * documentarea noului format de teste
  * scoaterea părților care vorbesc despre versiuni vechi de Pylint
  * folosirea unei noi teme
  * alte îmbunătățiri (multe secțiuni nu sunt prea bine explicate sau deloc)
  
  
- schimbarea părților ce depind de logilab.common

Vrem să nu mai depindem de logilab.common. În momentul de față sunt câteva locuri unde folosim această bibliotecă:
   - pentru parsarea argumentelor CLI
   - pentru reporturi
   - pentru un cached_property
   - etc
   
Reducerea dependenței de această bibliotecă trebuie făcută iterativ,
pentru că sunt multe lucruri de schimbat și riscăm regressii dacă nu avem teste consistente.

- Pluginuri pentru frameworkuri sau biblioteci cunoscute, pentru care Pylint nu se descurcă atât de bine:
   * nova.objects
   * eventlet.green
   * multiprocessing in Python 3.4+ 
   * orice altceva. :-)
       
- https://bitbucket.org/logilab/astroid/issue/74/classmro-doesnt-understand-dynamic

Implementarea pe care o avem acum pentru aflarea ordinei de rezolvare a metodelor (mro),
nu poate înțelege anumite cazuri, cum ar fi următorul:

def DecoratorMixin():
    class Mixin(object):
        pass
    return Mixin
class A(DecoratorMixin(), DecoratorMixin()):
    pass
    
Aici, DecoratorMixin() va întoarce de fiecare dată altă clasă Mixin și 
astroid.scoped_nodes.Class.mro nu ține cont de acest lucru.
(O idee ar fi să implementăm __eq__ pentru nodurile Class în care să verificăm
dacă  scope-ul nodului este o funcție și dacă această condiție este
adevărată, să nu considerăm nodul curent egal cu celălalt,
dacă și celălalt are aceeași condiție respectată)
            
- pluginuri pylint pentru editoare/ides

* https://bitbucket.org/logilab/pylint/issue/478/zmq-no-member-false-positives-even-with

Trebuie adăugat un astroid brain tip prin care procesarea modului zmq să fie păcălită
de existența atributelor respective.

* https://bitbucket.org/logilab/pylint/issue/376/remaining-work-from-the-issue-20

pylint are un mod prin care poate analiza un cod pentru posibile probleme
de compatibilitate între Python 2 și Python 3. Pentru asta, folosim flagul --py3k.
În cadrul acestui verificator, toate celelalte mesaje vor fi dezactivate și vor fi activate
doar cele din cadrul verificatorului python3.py. Mai sunt câteva mesaje noi ce pot fi implementate
în cadrul acestui verificator:


*   Exceptions escape except statement

         try:
		      1/0
        except Exception as exc:
              pass
        f = exc
		
*   Loop variant escapes listcomp

       [i for i in range(10)]
	   print(i)
	   
*   list.sort(cmp=..)/sorted(cmp=...)

      list.sort(cmp=func)
	    
*  Indexing bytes using type inference

      b"something"[0]


* https://bitbucket.org/logilab/pylint/issue/382/pylint-does-not-detect-attributes-added

   Trebuie scris un astroid brain tip, prin care să se înțeleagă următorul
   cod https://code.google.com/p/googleappengine/source/browse/trunk/python/google/appengine/ext/ndb/model.py#3931.
   și https://code.google.com/p/googleappengine/source/browse/trunk/python/google/appengine/ext/ndb/__init__.py
   
   Astfel, acest nou brain tip trebuie să adauge în modulul appengine.ext.ndb acele elemente locale
   care nu pot fi detectate.
   
* https://bitbucket.org/logilab/pylint/issue/456/parallel-jobs-dont-work-with-custom

  pylint suportă distribuire pe mai multe procese, cu flagul --jobs. Momentan, în cadrul
  acestei distribuiri, nu se ține cont de pluginurile custom pasate la linia de comandă.
  Trebuie investigat dacă putem suport acest lucru și dacă da, să adăugăm
  suport pentru această funcționalitate.
  
* https://bitbucket.org/logilab/pylint/issue/272/anomalous-backslash-in-string-for-raw  

Trebuie verificată folosirea funcției lib2to3.pgen2.tokenize în loc de tokenize.generate_tokens,
din pylint.utils.tokenize_module.

* https://bitbucket.org/logilab/pylint/issue/210/unused-variable-when-binding-as-a-default

Trebuie investigat de ce următorul cod emite în mod greșit un unused-variable.

def function():
    for i in range(10):
        return lambda i=i: i
		
  
* https://bitbucket.org/logilab/pylint/issue/354/cannot-infer-constructor-arguments-with

Trebuie investigat de ce Pylint nu înțelege cazul de mai sus și rezolvat.

* https://bitbucket.org/logilab/pylint/issue/438/mro-method-not-found

În cazul de mai sus, lookupul se va face pe metaclasă, care în cazul ăsta va fi type.
Trebuie adăugat cel mai probabil în astroid, în implementarea pentru getattr, suport pentru
acest lucru. Adică dacă un item nu a fost găsit în clasa curentă, să se facă un lookup
pe metaclasă. Metaclasa poate fi aflată cu Class.metaclass(), dar ea va întoarce doar
metaclasa setată explicit. Însă dacă știm că nodul clasă în cauză este new style class,
atunci putem să ne uităm după valori din type.