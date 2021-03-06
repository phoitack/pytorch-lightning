.. _production-inference:

Inference in Production
=======================
PyTorch Lightning eases the process of deploying models into production.


Exporting to ONNX
-----------------
PyTorch Lightning provides a handy function to quickly export your model to ONNX format, which allows the model to be independent of PyTorch and run on an ONNX Runtime.

To export your model to ONNX format call the `to_onnx` function on your Lightning Module with the filepath and input_sample.

.. code-block:: python

    filepath = 'model.onnx'
    model = SimpleModel()
    input_sample = torch.randn((1, 64))
    model.to_onnx(filepath, input_sample, export_params=True)

You can also skip passing the input sample if the `example_input_array` property is specified in your LightningModule.

Once you have the exported model, you can run it on your ONNX runtime in the following way:

.. code-block:: python

    ort_session = onnxruntime.InferenceSession(filepath)
    input_name = ort_session.get_inputs()[0].name
    ort_inputs = {input_name: np.random.randn(1, 64).astype(np.float32)}
    ort_outs = ort_session.run(None, ort_inputs)
