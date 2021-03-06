==========================
Upgrading a source install
==========================

.. note::

   Before upgrading CKAN you should check the compatibility of any custom
   themes or extensions you're using, check the changelog, and backup your
   database. See :ref:`upgrading`.

The process for upgrading a source install is the same, no matter what type of
CKAN release you're upgrading to:

#. Activate your virtualenv and switch to the ckan source directory, e.g.:

   .. parsed-literal::

    |activate|
    cd |virtualenv|/src/ckan

#. Checkout the new CKAN version from git, for example::

    git fetch
    git checkout release-v2.0

   If you have any CKAN extensions installed from source, you may need to
   checkout newer versions of the extensions at this point as well. Refer to
   the documentation for each extension.

#. Update CKAN's dependencies::

     pip install --upgrade -r requirements.txt

#. If you are upgrading to a new :ref:`major release <releases>` you need to
   update your Solr schema symlink.

   When :ref:`setting up solr` you created a symlink
   ``/etc/solr/conf/schema.xml`` linking to a CKAN Solr schema file such as
   |virtualenv|/src/ckan/ckan/config/solr/schema-2.0.xml. This symlink
   should be updated to point to the latest schema file in
   |virtualenv|/src/ckan/ckan/config/solr/, if it doesn't already.

   For example, to update the symlink:

   .. parsed-literal::

     sudo rm /etc/solr/conf/schema.xml
     sudo ln -s |virtualenv|/src/ckan/ckan/config/solr/schema-2.0.xml /etc/solr/conf/schema.xml

#. If you are upgrading to a new :ref:`major release <releases>` update your
   CKAN database's schema using the ``db upgrade`` command.

   .. warning ::

     To avoid problems during the database upgrade, comment out any plugins
     that you have enabled in your ini file. You can uncomment them again when
     the upgrade finishes.

   For example:

   .. parsed-literal::

    paster db upgrade --config=\ |development.ini|

   See :ref:`paster db` for details of the ``db upgrade``
   command.

#. Rebuild your search index by running the ``ckan search-index rebuild``
   command:

   .. parsed-literal::

    paster search-index rebuild -r --config=\ |development.ini|

   See :ref:`rebuild search index` for details of the
   ``ckan search-index rebuild`` command.

#. Finally, restart your web server. For example if you have deployed CKAN
   using the Apache web server on Ubuntu linux, run this command:

   .. parsed-literal::

    |reload_apache|

#. You're done!

You should now be able to visit your CKAN website in your web browser and see
that it's running the new version of CKAN.

