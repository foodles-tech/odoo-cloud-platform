===========================================
Attachments on Microsoft Azure Blob Storage
===========================================

This addon allows to store the attachments (documents and assets) on `Microsoft Azure
Blob Storage <https://docs.microsoft.com/azure/storage/blobs/>`_.

Configuration
-------------

Activate Azure Blob storage:

* Create or set the system parameter with the key ``ir_attachment.location``
  and the value in the form ``azure``.

* Configure accesses with the following environment variables:

.. list-table::
   :header-rows: 1

   * - Name
     - Description
     - Values
   * - ``AZURE_STORAGE_CONNECTION_STRING``
     - Connection string.
     - Required if using the connection string authentication method.
   * - ``AZURE_STORAGE_ACCOUNT_NAME``
     - Name of the storage account.
     - Required if using the storage account authentication method.
   * - ``AZURE_STORAGE_ACCOUNT_URL``
     - URL of the storage account
     -
   * - ``AZURE_STORAGE_ACCOUNT_KEY``
     - Key of the storage account
     -
   * - ``AZURE_STORAGE_NAME``
     - Name of the container within the blob storage. 1 container per database will be
       created.
     - Optional. The strings ``{db}`` and ``{env}`` can be used inside that variable
       and the values will be replaced respectively by the database name and environment
       name.
   * - ``AZURE_DUPLICATE``
     - If set, the blob storage and all its objects will be copied when the database is
       duplicated.
     - True
   * - ``AZURE_DELETE_ON_DBDROP``
     - If set, the container and all its objects will be deleted when the database is
       dropped.
     - True

One container will be created per database using the `RUNNING_ENV` environment variable
and the name of the database. By default, `RUNNING_ENV` is set to `dev`.

The container name will also be stored in the database for each attachment,
and will be used to access the right container in the storage.

This addon must be added in the server wide addons with (``--load`` option):

``--load=web,attachment_azure``

The System Parameter ``ir_attachment.storage.force.database`` can be customized to
force storage of files in the database. See the documentation of the module
``base_attachment_object_storage``.

Limitations
-----------

* You need to call ``env['ir.attachment'].force_storage()`` after
  having changed the ``ir_attachment.location`` configuration in order to
  migrate the existing attachments to Azure Blob Storage.
