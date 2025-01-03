### RKNN Model Zoo Testing YOLO World (with CLIP Model)
```shell
pip install transformers
```
```shell
cd Projects/rknn_model_zoo/examples/yolo_world/model/
```

```shell
wget -O ./clip_text.onnx https://ftrg.zbox.filez.com/v2/delivery/data/95f00b0fc900458ba134f8b180b3f7a1/examples/clip/clip_text.onnx
wget -O ./yolo_world_v2s.onnx https://ftrg.zbox.filez.com/v2/delivery/data/95f00b0fc900458ba134f8b180b3f7a1/examples/yolo_world/yolo_world_v2s.onnx
```

- The CLIP text model only supports FP and not INT8
```shell
cd ../../python/clip_text/
python convert.py ../../model/clip_text.onnx rk3588 fp
```
- The Yolo World model (default : INT8)
```shell
cd ../../python/yolo_world/
python convert.py ../../model/yolo_world_v2s.onnx rk3588
```

- Run Models with Python
```shell
cd ..
python yolo_world.py --target rk3588
```

- It runs properly
```text
   class        score      xmin, ymin, xmax, ymax
--------------------------------------------------
   person       0.935     [ 473,  371,  545,  580]
traffic light    0.843     [ 136,   36,  187,  202]
traffic light    0.843     [ 218,    0,  318,  104]
traffic light    0.614     [ 193,  108,  326,  239]
traffic light    0.270     [ 143,  230,  183,  269]
```

- result img custom text and custom img
![img](result.jpg)
