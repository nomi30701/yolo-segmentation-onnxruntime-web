# YOLO11 Instance Segmentation Browser onnxruntime-web

<img src="https://github.com/nomi30701/yolo-segmentation-onnxruntime-web/blob/main/preview.png" height=80% width=80%>

This is yolo model Instance Segmentation browser, powered by onnxruntime web.

Support Webgpu acceleration performance and wasm(cpu).

Realtime process webcam, image.

Add custom model for inference.

## Models
### Available Yolo Models
| Model                                                  | Input Size | Param. |
| :----------------------------------------------------- | :--------: | :----: |
| [YOLO11-N](https://github.com/ultralytics/ultralytics) |    640     |  2.6M  |
| [YOLO11-S](https://github.com/ultralytics/ultralytics) |    640     |  9.4M  |

## Setup
```bash
git clone https://github.com/nomi30701/yolo-segmentation-onnxruntime-web.git
cd yolo-segmentation-onnxruntime-web
yarn install # install dependencies
```
## Scripts
```bash
yarn dev # start dev server 
```

## Use other YOLO model
1. Conver YOLO model to onnx format. Read more on [Ultralytics](https://docs.ultralytics.com/).
    ```Python
    from ultralytics import YOLO

    # Load a model
    model = YOLO("yolo11n-seg.pt")

    # Export the model
    model.export(format="onnx", opset=12, dynamic=True)  
    ```
2. Copy your yolo model to `./public/models` folder. (Also can click **`Add model`** button)
3. Add `<option>` HTML element in `App.jsx`,`value="YOUR_FILE_NAME"`. 
    ```HTML
    ...
    <option value="YOUR_FILE_NAME">CUSTOM-MODEL</option>
    <option value="yolo11n-seg-simplify-dynamic">yolo11n-2.6M</option>
    <option value="yolo11s-seg-simplify-dynamic">yolo11s-9.4M</option>
    ...
    ```
4. select your model on page.
5. DONE!👍
> ✨ Support Webgpu
> 
> For onnx format support Webgpu, export model set **`opset=12`**.

> ✨ Dynamic input size
>
> If doesn't need dynamic input, please comment out in **`/utils/inference_pipeline.js`**
> ```Javascript
> const [src_mat_preProcessed, xRatio, yRatio] = await preProcess(
>   src_mat,
>   sessionsConfig.input_shape[2],
>   sessionsConfig.input_shape[3]
> );
> ```
>
> And delete
> ```Javascript
> const [src_mat_preProcessed, div_width, div_height] = preProcess_dynamic(src_mat);
> const xRatio = src_mat.cols / div_width;
> const yRatio = src_mat.rows / div_height;
> ```
> Change Tensor size
> ```Javascript
> const input_tensor = new ort.Tensor("float32", src_mat_preProcessed.data32F, [
>   1,
>   3,
>   sessionsConfig.input_shape[3],
>   sessionsConfig.input_shape[2],
> ]);
