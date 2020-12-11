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
