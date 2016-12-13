.. Oppdatere

Jenkins CI
----------

`Jenkins.includeos.org <https://jenkins.includeos.org>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Getting your personal build to build on the jenkins server
----------------------------------------------------------

If you want to get your personal fork of IncludeOS to build with every commit this procedure will show you what steps to go through.

Things to take note off:

- Will by default build on your dev branch. This will be easier to change at a later date.

- Will look for the repo: ``https://github.com/<github-username>/IncludeOS``

- Does not merge with upstream dev automatically as of this date.

Follow these steps to get it to work:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Go to the settings page for your **personal fork**

|Settings menu|

2. Navigate to the *Webhooks & Services* section and press the *Add webhook* button. Then enter the following url into the *Payload URL* section ``https://jenkins.includeos.org/github-webhook/``. Then press *Add webhook*

|Payload URL|

3. To make sure this works, go back to the webhooks page and make sure you see the green checkmark next to the url. This might take a few seconds, so refresh the page.

|Checkmark|

Then when I create the tests results will be available on `Jenkins.includeos.org <https://jenkins.includeos.org>`__

.. |Settings menu| image:: http://i.imgur.com/wfoYcaD.png
.. |Payload URL| image:: http://i.imgur.com/g0gEcBq.png
.. |Checkmark| image:: http://i.imgur.com/yUTIwZ1.png?1
