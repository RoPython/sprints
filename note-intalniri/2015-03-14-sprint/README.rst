======
Notițe
======

Notițe discuții de la sprint.

Idee ``tracerex``
=================

Pentru https://bitbucket.org/logilab/pylint/issue/210/unused-variable-when-binding-as-a-default trebuie sa ma
familiarizez cu codul din ``variables.py``. Din pacate codul e relativ voluminos si problema pare sa fie de "state
keeping".

Mi-am facut un tracer miniatura care printeaza tot ce se executa in ``variables.py``. Am modificat ``__main__.py`` sa
aiba::

    def dumbtrace(frame, event, args):
        if "variables.py" in frame.f_code.co_filename:
            sys.stdout.write("%015s:%-3s %06s %s" % (
                os.path.basename(frame.f_code.co_filename),
                frame.f_lineno,
                event,
                linecache.getline(frame.f_code.co_filename, frame.f_lineno)
            ))
        return dumbtrace  # "step in"

Ar fi util sa pot printa variabile de pe stack in mod selectic, incat sa imi pot da seama ce se intampla.

Idee de API pentru libraria tracerex (ca si arbore de decizie):

.. sourcecode:: python

    from tracerex import trace, F

    install(F(
        module="pylint.checkers.variables",
        action="print:self",
    ) | F(
        function="visit_lambda",
        action="print:node"
    ))

Idee de ``F`` ca si predicate syntactic sugar:

.. sourcecode:: python

    F(lambda module, function, locals: function.__name__ == "visit_lambda"])

    # echivalent cu

    F(function="visit_lambda")

Idee de la Claudiu, suport de breakpoints:

.. sourcecode:: python

    from tracerex import break_on

    break_on(F(function="visit_lambda"))

    # sau mai selectiv, cu un predicat

    break_on(F(function="visit_lambda", predicate=lambda module, function, locals: locals["node"] == "Foobar"]))

Idee de "actiuni":

.. sourcecode:: python

    from tracerex import trace, F

    install(F(
        module="pylint.checkers.variables",
        action="print:self"  # ca si alternativa la vars
    ) | F(
        function="visit_lambda",
        action="break"  # ca si alternativa la break_on
        # sau: action=pdb.set_trace ?

    ) | F(
        function="visit_dictcomp",
        action=lambda module, function, locals: print(locals)  # ca si callback, similar cu idea de predicat
    ))

Compozabilitate expressii ``F``:

    "AND":

    .. sourcecode:: python

        F(module="pylint.checkers.variables") & F(function="visit_lambda")

        # e acelasi lucru ca si

        F(module="pylint.checkers.variables", function="visit_lambda")

    "OR":

    .. sourcecode:: python

        F(module="pylint.checkers.variables") | F(function="visit_lambda")

        # e acelasi lucru ca si

        F(lambda mod, func, locals: mod == "pylint.checkers.variables" or func.__name__ == "visit_lambda")


Dorinte pentru API
--------------------

* Ortogonal: idea e ca API-ul sa ofere cateva interfete simple (filtre, actiuni etc) pe care utilizatorul sa le combine
  incat sa obtina ceva. Scopul e sa fie ceva compozabil, nu multe functii specializate pe anumite usecaseuri.
* Pe doua nivele ("layered API"): idea e ca api-ul sa ofere niste primitive care suporta 2 feluri de utilizare:

  * DSL bazat pe text. Ex: ``F(function="visit_lambda")``, ``F(action="print")``.

    Limitarile sunt evidente dar e nevoie de el pentru ca:

    * utilizatorii sa nu fie nevoiti sa scrie cod pentru scenariile simple
    * sa se poata incarca dintr-un fisier/string de configuratie (unde nu poate sa existe cod) - vezi idea de activare.

  * DSL-ul bazat pe text este intermixabil cu primitive low-level (callbacks): predicate si actiuni.

    Efectiv, ``F(function="visit_lambda")`` este doar syntactic sugar la ``F(lambda mod, func, locals: func ==
    "visit_lambda")``

Idei de activare
----------------

Activare prin ``.pth`` + env var::

    pip install tracerex

    PYTHON_TRACEREX="??? ceva config ???" my_buggy_app
