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
        if 'variables.py' in frame.f_code.co_filename:
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
        module='pylint.checkers.variables',
        vars=['self']
    ) | F(
        function='visit_lambda',
        vars=['node']
    ))

Idee de ``F`` ca si predicate syntactic sugar:

.. sourcecode:: python

    F(lambda module, function, locals: function.__name__ == 'visit_lambda'])

    # echivalent cu

    F(function='visit_lambda')

Idee de la Claudiu, suport de breakpoints:

.. sourcecode:: python

    from tracerex import break_on

    break_on(F(function='visit_lambda'))

    # sau mai selectiv, cu un predicat

    break_on(F(function='visit_lambda', predicate=lambda module, function, locals: locals['node'] == 'Foobar']))

Idee de "actiuni":

.. sourcecode:: python

    from tracerex import trace, F

    install(F(
        module='pylint.checkers.variables',
        action='print:self'  # ca si alternativa la vars
    ) | F(
        function='visit_lambda',
        action='break'  # ca si alternativa la break_on
    ) | F(
        function='visit_dictcomp',
        action=lambda module, function, locals: print(locals)  # ca si callback, similar cu idea de predicat
    ))

