Nipype2Pydra
============
.. image:: https://github.com/nipype/nipype2pydra/actions/workflows/tests.yml/badge.svg
   :target: https://github.com/nipype/nipype2pydra/actions/workflows/tests.yml
.. image:: https://codecov.io/gh/nipype/nipype2pydra/branch/main/graph/badge.svg?token=UIS0OGPST7
   :target: https://codecov.io/gh/nipype/nipype2pydra
.. image:: https://img.shields.io/pypi/pyversions/nipype2pydra.svg
   :target: https://pypi.python.org/pypi/nipype2pydra/
   :alt: Supported Python versions
.. image:: https://img.shields.io/pypi/v/nipype2pydra.svg
   :target: https://pypi.python.org/pypi/nipype2pydra/
   :alt: Latest Version

Nipype2Pydra is a command-line tool for semi-automatically porting command interfaces
and workflows written from the Nipype_ workflow engine to its successor Pydra_. It has
been designed so that any required manual-specifications are stored in separate YAML
specification files, which is read ahead of the conversion, so once designed the
conversion can be re-run to pick up changes in newer versions of the Nipype_ code without
having to redo the manual steps.


Basic Usage
-----------

To convert a Nipype_ interface to a Pydra_ task run the `nipype2pydra task` command and
pass it the conversion specification YAML file and the root of the package to save the
generated module, e.g.::

    $ nipype2pydra task ants_registration_registration.yaml /path/to/package/root

This will create a module file under the package root directory based on the `output_module`
field in the specification, e.g::
    
    /path/to/package/root/output/module/path

If that is missing and the nipype interface is in the standard `nipype.interfaces`
package, then it will be stored at `pydra.tasks.` with the same path end.

Conversion Specifications
-------------------------

While the conversions aim to as automatic as possible, there are structural
differences between Nipype_ and Pydra_ that require manual specification, and it might
be desirable to rename some of the inputs/outputs to more intuitive names. Such
specifications are stored in YAML_ format, such as the ANTs registration conversion
specification,

.. code-block:: yaml

    task_name: Registration
    nipype_module: nipype.interfaces.ants.registration
    output_requirements:
        output_warped_image: ["fixed_image", "moving_image", "output_transform_prefix"]
    output_templates:
        output_warped_image: "{output_transform_prefix}warped"
    doctest:
        fixed_image: test.nii.gz
        moving_image: test.nii.gz
        cmdline: >- 
            antsRegistration --output [ output_, output_warped_image.nii.gz ]
            --metric Mattes[ test.nii, test.nii, 1, 32, Random, 0.05 ]
        tests_inputs: []
        tests_outputs:
        - AttributeError

*Detailed description of the different options to go here*

Installation
------------

*Nipype2Pydra* can be installed for Python >= 3.7 from PyPI with

.. code-block:: bash

    $ python3 -m pip install nipype2pydra


License
-------

This work is licensed under a
`Creative Commons Attribution 4.0 International License <http://creativecommons.org/licenses/by/4.0/>`_

.. image:: https://i.creativecommons.org/l/by/4.0/88x31.png
  :target: http://creativecommons.org/licenses/by/4.0/
  :alt: Creative Commons Attribution 4.0 International License

.. _Pydra: https://pydra.readthedocs.io
.. _Nipype: https://nipype.readthedocs.io/en/latest/
.. _YAML: https://yaml.org
