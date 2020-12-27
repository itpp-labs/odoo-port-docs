=======================
 ``13.0-`` → ``14.0+``
=======================

OCA's checklist
===============

https://github.com/OCA/maintainer-tools/wiki/Migration-to-version-14.0#tasks-to-do-in-the-migration

New API
=======

.. code-block:: sh

    # tour.STEPS.SHOW_APPS_MENU_ITEM is replaced to tour.stepUtils.showAppsMenuItem()
    # https://github.com/odoo/odoo/commit/ab2e3fdb7d801f8846b84733a47127e818a49d5b
    find . -type f -name '*.js' | xargs sed -i 's/STEPS.SHOW_APPS_MENU_ITEM/stepUtils.showAppsMenuItem()/g'

Selection
=========

  ValueError: *model.name*: required selection fields must define an ondelete policy that implements the proper cleanup of the corresponding records upon module uninstallation. Please use one or more of the following policies: 'set default' (if the field has a default defined), 'cascade', or a single-argument callable where the argument is the recordset containing the specified option.
  
See https://odoo-development.readthedocs.io/en/latest/dev/py/fields.html#selection

Point of Sale
=============

Point of Sale frontend was refactored to use `OWL <https://odoo.github.io/owl/>`__ framework. So almost everyting in dependent modules should be adapted to use OWL.

New API
-------

.. code-block:: sh

    # https://github.com/odoo/odoo/commit/c6397ab3b3654da336a4269a81d5d91016baf520
    find . -type f -name '*.xml' | xargs sed -i 's/widget.format_currency/env.pos.format_currency/g'


Cashier selecting popup
-----------------------

Instead of calling ``gui.select_employee`` use following code

.. code-block:: javascript

    const useSelectEmployee = require('pos_hr.useSelectEmployee');
    const { selectEmployee } = useSelectEmployee();

    // ...

    const list = this.env.pos.employees
        .filter((employee) => employee.id !== this.env.pos.get_cashier().id)
        .map((employee) => {
            return {
                id: employee.id,
                item: employee,
                label: employee.name,
                isSelected: false,
            };
        });

    const employee = await selectEmployee(list);
    if (employee) {
        this.env.pos.set_cashier(employee);
    }

Based on: https://github.com/odoo/odoo/blob/338f8d843832c339c52c00c7f4c41b5ab2d696ed/addons/pos_hr/static/src/js/CashierName.js#L27-L41

.. include:: DINAR-PORT.rst
