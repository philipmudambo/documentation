================
Automation rules
================

Automation rules are used to trigger automatic changes based on user actions (e.g., apply a
modification when a field is set to a specific value), email events, time conditions (e.g., archive
a record 7 days after its last update), or external events.

To create an automation rule with Studio, proceed as follows:

#. Open Studio, then click :guilabel:`Automations`, then :guilabel:`New`.
#. Define the :ref:`studio/automated-actions/trigger` and, depending on the trigger you chose, fill
   in the fields that appear on the screen once you've selected the trigger.
#. Click :guilabel:`Add an action`, then select the :guilabel:`Type` of
   :ref:`action <studio/automated-actions/action>` and fill in the fields that appear on the screen
   based on the action you selected.
#. Click :guilabel:`Save & Close` or :guilabel:`Save & New`.

.. example::

   .. image:: automated_actions/automated-action-ex.png
      :alt: Example of an automated action on the Subscription model

.. tip::
   - To modify the :doc:`model <models_modules_apps>` where the automation rule is applied, switch
     models before clicking :guilabel:`Automations` in Studio, or :ref:`activate the Developer mode
     <developer-mode>`, and select the :guilabel:`Model` in the :guilabel:`Automation Rules` form.
   - You can also create automation rules from any kanban stage by clicking the gear icon
     (:guilabel:`⚙` ) next to the kanban stage name, then selecting :guilabel:`Automations`. By
     default, the :guilabel:`Stage is set to` trigger is defined in this case, but you can change it
     if necessary.

.. _studio/automated-actions/trigger:

Trigger
=======

The :guilabel:`Trigger` is used to define when the automated action should be applied. The available
triggers depend on the :doc:`model <models_modules_apps>`. Five trigger categories are available
overall:

- :ref:`studio/automated-actions/trigger/values-updated`
- :ref:`studio/automated-actions/trigger/email-events`
- :ref:`studio/automated-actions/trigger/values-timing-conditions`
- :ref:`studio/automated-actions/trigger/custom`
- :ref:`studio/automated-actions/trigger/external`

.. tip::
   For some triggers, you can define a :guilabel:`Before Update Domain` to define the conditions
   that must be met *before* the automation rule is triggered. In contrast,
   :ref:`Extra Conditions <studio/automated-actions/trigger/values-timing-conditions>`
   and :ref:`Apply on <studio/automated-actions/trigger/custom>` filters allow to define additional
   conditions for executing the automation rule. Unlike :guilabel:`Before Update domain` filters,
   these conditions are checked *during* the execution of the automation rule.

   To define a :guilabel:`Before Update Domain`, :ref:`activate the Developer mode
   <developer-mode>`, click :guilabel:`Edit Domain`, then :guilabel:`New Rule`.

   For example, if you want the automated action to happen when an email address is set on a
   contact, define the :guilabel:`Before Update Domain` to `Email is not set`, and the
   :guilabel:`Apply on` domain to `Email is set`.

   .. image:: automated_actions/before-update-domain-ex.png
      :alt: Example of a trigger with a Before Update Domain

.. _studio/automated-actions/trigger/values-updated:

Values Updated
--------------

The triggers available in this category depend on the model. Select the trigger, then select a value
if required.

.. image:: automated_actions/values-updated-ex.png
   :alt: Example of a Values Updated trigger

.. _studio/automated-actions/trigger/email-events:

Email Events
------------

Trigger automated actions upon receiving or sending emails.

.. _studio/automated-actions/trigger/values-timing-conditions:

Timing Conditions
-----------------

Trigger automated actions based on a date field. The following triggers are available:

- :guilabel:`Based on date field`: Select the field to be used next to the :guilabel:`Delay` field.
- :guilabel:`After creation`: The action is triggered when a record is created and saved.
- :guilabel:`After last update`: The action is triggered when an existing record is edited and
  saved.

You can then define:

- a :guilabel:`Delay`: Specify the number of minutes, hours, days, or months. To trigger the action
  before the trigger date, specify a negative number instead.
  If you selected the :guilabel:`Based on date field` trigger, you must also select the field to be
  used to define the delay.
- :guilabel:`Extra Conditions`: Click :guilabel:`Add condition`, then define the conditions to be
  met for triggering the automation rule. Click :guilabel:`New rule` to add another condition.

The action is triggered when the delay is reached, and the conditions are met.

.. note::
   By default, the scheduler checks for trigger dates every 4 hours.

.. example::
   If you want to send a reminder email 30 minutes before the start of a calendar event, select the
   :guilabel:`Start (Calendar Event)` under :guilabel:`Trigger Date` and set the :guilabel:`Delay`
   to **-30** :guilabel:`Minutes`.

   .. image:: automated_actions/timing-conditions-trigger.png
      :alt: Example of a Based on date field trigger

.. _studio/automated-actions/trigger/custom:

Custom
------

Trigger automated actions:

- :guilabel:`On save`: When the record is saved;
- :guilabel:`On deletion`: When a record is deleted;
- :guilabel:`On UI change`: When a field's value is changed on the on the :ref:`Form view
  <studio/views/general/form>`, even before saving the record.

.. note::
   The :guilabel:`On UI change` trigger can only be used with the
   :ref:`studio/automated-actions/action/python-code` action, and only works when a modification is
   made manually. The action is not executed if the field is changed through another automation
   rule.

For the :guilabel:`On save` and :guilabel:`On UI change` triggers, you must then define the field(s)
to be used to trigger the automation rule in the :guilabel:`When updating` field. If the field
is left empty, all of the model's fields are taken into account.

Optionally, you can also define additional conditions to be met to trigger the automation rule
in the :guilabel:`Apply on` field.

.. _studio/automated-actions/trigger/external:

External
--------

Trigger automated actions based on an external event using a webhook, e.g., move a project task to
the :guilabel:`Done` stage when a branch has been merged on GitHub. A webhook is a method of
communication between two systems where the source system sends an HTTP(S) request to a destination
system based on a specific event. It usually includes a data payload containing information about
the event that occurred.

To configure the :guilabel:`Webhook` trigger, define the :guilabel:`URL` of the source system
(i.e., the system sending the request), then enter the code to run to determine the record on which
the automation rule should be executed in the :guilabel:`Target Record` field, e.g., `XXX`.

You can also choose to :guilabel:`Log Calls` to record the payloads received and make sure the data
sent by the source system matches the expected format and content. This also helps identify
and diagnose any issues that may arise. To access the logs, click the :guilabel:`Logs` smart
button at the top of the :guilabel:`Automation rules` form.

.. note::
   If you wish to use the webhook's content for a purpose other than to find the record, you can
   only do so using an :ref:`studio/automated-actions/action/python-code` action.

.. _studio/automated-actions/action:

Actions
=======

Once you have defined the automation rule's trigger, click :guilabel:`Add an action` to define the
action to be executed.

.. tip::
   You can define multiple actions for the same trigger/automation rule.

.. _studio/automated-actions/action/update-record:

Update Record
-------------

This action allows to update one of the record's (related) fields. Click the :guilabel:`Update`
field and, in the list that opens, select or search for the field to be updated; click the right
arrow next to the field name to access the list of related fields if needed.

If the selected field is a :ref:`many2many field <studio/fields/relational-fields/many2many>`,
choose whether the field must be updated by :guilabel:`Adding`, :guilabel:`Removing`, or
:guilabel:`Setting it to` the chosen value or by :guilabel:`Clearing it`.

.. example::
   If you want the automated action to remove a tag from the customer record, set the
   :guilabel:`Update` field to :guilabel:`Customer > Tags`, select :guilabel:`By Removing`, then
   select the tag.

   .. image:: automated_actions/update-record-ex.png
      :alt: Example of an Update Record action

.. tip::
   Alternatively, you can also set a record's field dynamically using Python code and external
   references: select :guilabel:`Compute` instead of :guilabel:`Update`, then enter the code to be
   used for computing the field's value.
   XXX should this be in a tip + does it need an example? XXX

Create Activity
---------------

This action is used to schedule a new activity linked to the record. Select an :guilabel:`Activity
Type`, enter a :guilabel:`Title` and description, then specify when you want the activity to be
scheduled in the :guilabel:`Due Date In` field, select a :guilabel:`User type`:

- To always assign the activity to the same user, select :guilabel:`Specific User` and add the user
  in the :guilabel:`Responsible` field;
- To target a user linked to the record dynamically, select :guilabel:`Dynamic User (based on
  record)` and change the :guilabel:`User Field` if necessary.

.. example::
   After a lead is turned into an opportunity, you want your automated action to set up a call for
   the user responsible for the lead. To do so, set the :guilabel:`Activity Type` to
   :guilabel:`Call` and the :guilabel:`Activity User Type` to :guilabel:`Dynamic User (based on
   record)`.

   .. image:: automated_actions/create-activity-ex.png
      :alt: Example of a Create Activity action

Send Email and Send SMS
-----------------------

The action is used to send an email or a text message to a contact linked to a specific record. To
do so, select or create an :guilabel:`Email Template` or an an :guilabel:`SMS Template`, then, in
the :guilabel:`Send Email As` or :guilabel:`Send SMS As` field, choose how you want to send the
email or text message.

.. _studio/automated-actions/action/add-followers:

Add Followers and Remove Followers
----------------------------------

Use this action to (un)subscribe existing contacts to the record.

Create Record
-------------

The action is used to create a new record on any model.

.. note::
   Selecting a :guilabel:`Record to Create` is only required if you want to target a model other
   than the one you are on.

Specify a :guilabel:`Name` for the record, then select a field in the :guilabel:`Link Field` field
to link the record that triggered the creation of the new record. For example, you could create a
contact automatically when a lead is turned into an opportunity.

.. tip::
   You can create another automation rule with :ref:`studio/automated-actions/action/update-record`
   actions to update the fields of the new record if necessary. For example, you can use a
   :guilabel:`Create Record` action to create a new task in a project and then assign it to a
   specific user using an :guilabel:`Update Record` action.

.. _studio/automated-actions/action/python-code:

Execute Code
------------

This action is used to execute Python code. The available variables are described in the
:guilabel:`Python Code` tab, which is also used to write your code, or in the :guilabel:`Help` tab.

Send Webhook Notification
-------------------------

This action allows to send a POST request with the values of the :guilabel:`Fields` to the URL
specified in the :guilabel:`URL` field.

The :guilabel:`Sample Payload` provides a preview of the data included in the request using a random
record's data or dummy data if no record is available.

.. _studio/automated-actions/action/several-actions:

Execute Existing Actions
------------------------

The action is used to trigger multiple actions at the same time. To do so, click on :guilabel:`Add a
line`, then, in the :guilabel:`Add: Child Actions` pop-up, select an existing action or click
:guilabel:`New` to create a new one.
