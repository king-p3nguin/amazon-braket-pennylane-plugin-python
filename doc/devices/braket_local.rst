The local Braket device
=======================

The local qubit device of the PennyLane-Braket plugin runs gate-based quantum computations on the local Braket SDK. This
could be either utilizing the processors of your own PC, or those of a `Braket notebook instance <https://docs.aws.amazon.com/braket/latest/developerguide/braket-get-started-create-notebook.html>`_ hosted on AWS.

This device is useful for small-scale simulations in which the time of sending a job to a remote service would add
an unnecessary overhead. It can also be used for rapid prototyping before running a computation
on a paid-for remote service.

Usage
~~~~~

After the Braket SDK and the plugin are installed you immediately have access to the local Braket device in PennyLane.

To instantiate the local Braket simulator, simply use:

.. code-block:: python

    import pennylane as qml
    device_local = qml.device("braket.local.qubit", wires=2) # local state vector simulator
    # device_local = qml.device("braket.local.qubit", backend="default", wires=2) # local state vector simulator
    # device_local = qml.device("braket.local.qubit", backend="braket_sv", wires=2) # local state vector simulator
    # device_local = qml.device("braket.local.qubit", backend="braket_dm", wires=2) # local state vector simulator

You can define and evaluate quantum nodes with these devices just as you would with any other PennyLane device.

For example:

.. code-block:: python

    @qml.qnode(device_local)
    def circuit(x, y, z):
        qml.RZ(z, wires=[0])
        qml.RY(y, wires=[0])
        qml.RX(x, wires=[0])
        qml.CNOT(wires=[0, 1])
        return qml.expval(qml.PauliZ(0)), var(qml.PauliZ(1))

When executed, the circuit will perform the computation on the local machine.

>>> circuit(0.2, 0.1, 0.3)
array([0.97517033, 0.04904283])

Device options
~~~~~~~~~~~~~~

You can set ``shots`` to ``None`` (default) to get exact results instead of results calculated from samples.

Supported operations
~~~~~~~~~~~~~~~~~~~~

The operations supported by this device vary based on the operations supported by the underlying Braket device. To check
the device's supported operations, run

.. code-block:: python

    dev.operations

In addition to those `provided by PennyLane <https://pennylane.readthedocs.io/en/stable/introduction/operations.html#qubit-operations>`_,
the PennyLane-Braket plugin provides the following framework-specific operations, which can be imported
from :mod:`braket.pennylane_plugin.ops <.ops>`:

.. autosummary::
    braket.pennylane_plugin.CPhaseShift00
    braket.pennylane_plugin.CPhaseShift01
    braket.pennylane_plugin.CPhaseShift10
    braket.pennylane_plugin.PSWAP
    braket.pennylane_plugin.GPi
    braket.pennylane_plugin.GPi2
    braket.pennylane_plugin.MS
    braket.pennylane_plugin.PRx

