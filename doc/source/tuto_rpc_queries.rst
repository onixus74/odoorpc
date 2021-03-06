
.. _tuto-execute-queries:

Execute RPC queries
*******************

The basic methods to execute RPC queries related to data models are
:func:`execute <odoorpc.ODOO.execute>` and
:func:`execute_kw <odoorpc.ODOO.execute_kw>`.
They take at least two parameters (the model and the name of the method to
call) following by additional variable parameters according to the method
called::

    >>> order_data = odoo.execute('sale.order', 'read', [1], ['name'])

This instruction will call the ``read`` method of the ``sale.order`` model
for the order ID=1, and will only returns the value of the field ``name``.

However there is a more efficient way to perform methods of a model by getting
a proxy of it with the
:func:`model registry <odoorpc.env.Environment.__getitem__>`, which
provides an API almost syntactically identical to the `Odoo` server side API
(see :class:`odoorpc.models.Model`), and which is able to set the user
context automatically (both for read and write operations)::

    >>> User = odoo.env['res.users']
    >>> User.write([6], {'name': "Dupont D."})
    True
    >>> odoo.env.context
    {'lang': 'fr_FR', 'tz': False}
    >>> Product = odoo.env['product.template']
    >>> Product.name_get([25])
    [[25, '[FURN_8220] Bureau Quatre Personnes']]

To stop sending the user context, use the :attr:`odoorpc.ODOO.config` property::

    >>> odoo.config['auto_context'] = False
    >>> Product.name_get([25])   # Without context, lang 'en_US' by default
    [[25, '[FURN_8220] Four Person Desk']]

.. note::

    The ``auto_context`` option only affect methods called from model proxies.

Here is another example of how to install a module (you have to be logged
as an administrator to perform this task)::

    >>> Module = odoo.env['ir.module.module']
    >>> module_ids = Module.search([('name', '=', 'purchase')])
    >>> Module.button_immediate_install(module_ids)

:ref:`Next step: Browse records <tuto-browse-records>`
