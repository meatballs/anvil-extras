Utils
=====
Client and server side utility functions.

Timing
------
There are client and server side decorators which you can use to show that a function has been called and the length of time it took to execute.

Client Code
^^^^^^^^^^^
Import the `timed` decorator and apply it to a function:

.. code-block:: python

   from anvil_extras.utils import timed

   @timed
   def target_function(args, **kwargs):
       print("hello world")

When the decorated function is called, you will see messages in the IDE console showing the arguments that were passed to it and the execution time.

Server Code
^^^^^^^^^^^
Import the `timed` decorator and apply it to a function:

.. code-block:: python

   import anvil.server
   from anvil_extras.server_utils import timed


   @anvil.server.callable
   @timed
   def target_function(args, **kwargs):
       print("hello world")

On the server side, the decorator takes a `logging.Logger` instance as one of its optional arguments. The default instance will log to stdout, so that messages will appear in your app's logs and in the IDE console. You can, however, create your own logger and pass that instead if you need more sophisticated behaviour:

.. code-block:: python

   import logging
   import anvil.server
   from anvil_extras.server_utils import timed

   my_logger = logging.getLogger(__name__)


   @timed(logger=my_logger)
   def target_function(args, **kwargs):
       ...

The decorator also takes an optional `level` argument which must be one of the standard levels from the logging module. When no argument is passed, the default level is `logging.INFO`.

Auto-Refresh
------------
Whenever you set a form's `item` attribute, the form's `refresh_data_bindings` method is called automatically.

The `utils` module includes a decorator you can add to a form's class so that `refresh_data_bindings` is called whenever `item` changes at all.

To use it, import the decorator and apply it to the class for a form:

.. code-block:: python

   from anvil_extras.utils import auto_refreshing
   from ._anvil_designer import MyFormTemplate


   @auto_refreshing
   class MyForm(MyFormTemplate):
       def __init__(self, **properties):
           self.init_components(**properties)

Now, the form has an `item` property which behaves like a dictionary. Whenever a value of that dictionary changes, the form's `refresh_data_bindings` method will be called.
