# How To Update ONNX Test Report

Currently, this test report is generated by a script. This script reads `onnx_test_report.rst.tmpl` as a template, then generated the result report by collecting the test result.


## How to use it

Ensure current environment can perform onnx testing. A docker container might be available for this purpose:
```
nnabla\docker\development\Dockerfile.onnx-test
```

Then, perform the following command:
```
nnabla/python/test/utils/conversion>python -m pytest . -s
```

This command will perform testing and generate test result.


## How to maintain

- importer_funcs_opset.yaml
  This file shows a table that describes which op version current implementation supports for each onnx function.
  If a op version is supported, it is set to true. For example:
  ```
  BatchNormalization:
      '1': true
      '6': true
      '7': false
  ```
  op version 7 is not supported in current implementation, hence, we set `'7'` to `false`.

- exporter_funcs_opset.yaml
  This file shows how nnabla functions is mapped to onnx functions, which op version will be involved.
  For example,
  ``` 
  Affine:
    - Reshape@5
    - Flatten@1
    - Gemm@6
  ```
  NNabla's `Affine` function will be mapped to 3 ONNX functions, `Reshape`, `Flatten`, `Gemm`. The number after `@` means op version of this function.

- onnx_test_report.rst.tmpl
  This is template file, if description has changed, we need to modify this part manually.