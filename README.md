# **Upperbody Segmentation SDK**

`UpperbodySegmentationSDK` is a high-performance human silhouette extraction and masking library built on **OpenCV** and **DeepCore (Deepixel's proprietary library)**. It leverages TensorFlow Lite models for real-time human detection, precise foreground-background separation, and high-quality segmentation masking. 

It outputs a segmentation mask corresponding to the detected human region, providing pixel-level accuracy. This enables you to 
**seamlessly separate the human upper body from the background** for various applications.


The SDK outputs a single-channel 8-bit grayscale image of the same resolution as the input. Pixel values range from 0 to 255, where 255 indicates the foreground and 0 indicates the background. Intermediate values may appear near foreground-background boundaries to create a soft transition and reduce aliasing artifacts,


Below is an illustration showing the **segmentation coverage**:
<p align="center">
  <img src="https://github.com/user-attachments/assets/cb54bf83-745c-49bd-90ae-1816b20d01bd" width="48%" />
  <img src="https://github.com/user-attachments/assets/0fefcda6-a9b3-455e-8dd4-e655ae8fb91e" width="48%" />
</p>

## **Key Features**
* **Real-time Processing:** Fast and accurate upperbody (portrait) segmentation.
* **Robust Algorithms:** Highly resilient against complex backgrounds, motion blurs, and lighting changes.
* **Multi-person Support:** Supports simultaneous segmentation of multiple people in a single frame.
* **Ultra-lightweight:** Works on CPU only — no GPU required.
* **Cross-Platform:** Supported on iOS, Android, Windows, macOS, and Web.
* **Versatile Use Cases:** Ideal for virtual backgrounds (video conferencing), portrait mode (background blur), AR try-on, live streaming, and photo editing.



## **Performance**

### 1. Speed
> **Note:** GPU Usage is **0%** (Only CPU Inference is used).

| Phone Model | Year | CPU Name | Tier | Target <br> (ms / %) | **Inference** <br> **Time (ms)** | **CPU** <br> **Usage (%)** |
| :--- | :---: | :--- | :--- | :---: | :---: | :---: |
| Samsung Galaxy S9 | 2018 | Exynos 9810 | Android-Low | 20 / 10 | **7-8** (-13) | **2.75** (-7.25) |
| Samsung Galaxy A52s | 2021 | Snapdragon 778G | Android-Mid | 9 / 4 | **8.15** (-0.85) | **3.04** (-0.96) |
| Samsung Galaxy Fold3 | 2021 | Snapdragon 888 | Android-High | 5 / 4 | **4.99** (-0.01) | **1.86** (-2.14) |
| Samsung Galaxy S22 | 2022 | Snapdragon 8 Gen 1 | Android-High | 5 / 4 | **4.25** (-0.75) | **1.58** (-2.42) |
| iPhone 6s | 2015 | A9 | iOS-Low | 15 / 20 | **12-13** (-3) | **19.5-20.5** (-0.5) |
| iPhone XS | 2018 | A12 Bionic | iOS-Mid | 7 / 4 | **5.55** (-1.45) | **3-4** (-1) |
| iPhone 14 Pro | 2022 | A16 Bionic | iOS-High | 5 / 4 | **3.09** (-1.91) | **1.8** (-2.2) |

### 2. Accuracy

| Metric | Value | Description |
| :--- | :---: | :--- |
| Mean IoU <br> Mean Boundary F1 <br> Mean Boundary IoU | **86.01%** <br> **88.77%** <br> **82.74%** | PP-HumanSeg14K<br> validation dataset<br>(2,431 images) |
| Mean IoU <br> Mean Boundary F1 <br> Mean Boundary IoU | **97.32%** <br> **97.63%** <br> **90.31%** | EasyPortrait<br> validation dataset<br> (4,000 images) |
| Mean IoU <br> Mean Boundary F1 <br> Mean Boundary IoU | **96.17%** <br> **95.05%** <br> **84.15%** | EG1800 dataset<br> (1,710 images) |


### 3. SDK Size
| 플랫폼 | 파일 | Uncompressed | Compressed (gzip) |
|---|---|---|---|
| **Android arm64-v8a** | libhumanx_native_android.so | 7.38 MB | 3.16 MB |
| **Android armeabi-v7a** | libhumanx_native_android.so | 4.60 MB | 2.50 MB |
| **iOS arm64** | libhumanx_native_ios.dylib | 11.63 MB | 4.60 MB |
| **macOS arm64** | libhumanx_native_mac.dylib | 14.64 MB | 5.29 MB |
| **WASM** (js + wasm) | humanx_native_wasm.* | 6.69 MB | 2.11 MB |


<!-- ## **API** -->
## **HumanX Native NAVER API Reference**

Created Date: 2026-04-14

## Scope

- Main namespace (C++): `xyz::deepixel` and `xyz::deepixel::coreai`
- Main module (NAVER): `UpperbodySegmentationSDK`

## Header Map

### Entry Headers

- `humanx/humanx_naver.hpp`: C++ umbrella header
- `humanx/humanx_naver_c.h`: C umbrella header
- `humanx/humanx_coreai_naver.hpp`: C++ CoreAI umbrella header
- `humanx/humanx_coreai_naver_c.h`: C CoreAI umbrella header

### CoreAI Module Headers

- `humanx/coreai/coreai_upper_body_segmentation_naver.hpp`
- `humanx/coreai/coreai_upper_body_segmentation_naver_c.h`

### Shared Type Headers

- `humanx/image_type.hpp`
- `humanx/image_type_c.h`
- `humanx/image_buffer.hpp`
- `humanx/image_buffer_c.h`
- `humanx/frame_input.hpp`
- `humanx/frame_input_c.h`
- `humanx/upper_body_segmentation_types.hpp`
- `humanx/upper_body_segmentation_types_c.h`
- `humanx/humanx_export.h`

## C++ API

## Image and Frame Types

### enum DP_IMAGE_TYPE

Supported input image formats.

**Description**: Describes pixel layout of input frames used by processing APIs.

**Values**:

- `BGR_888` - 3-channel BGR, 8 bits per channel. Preferred internal format for algorithms.
- `RGB_888` - 3-channel RGB, 8 bits per channel.
- `YUV_420_888` - YUV 4:2:0 semi-planar format (NV21-compatible conversion path).
- `RGBA_8888` - 4-channel RGBA, 8 bits per channel.
- `BGRA_8888` - 4-channel BGRA, 8 bits per channel. Recommended for iOS readback paths.

**Platform Recommendations**:

- Android: `YUV_420_888` or `RGBA_8888`
- iOS: `BGRA_8888`

**Performance Note**: `BGR_888` is the fastest path because algorithms use BGR natively and typically avoid an additional color conversion step.

### struct ImageBuffer

Lightweight image buffer container with optional ownership.

**Description**: This type stores image shape/stride metadata and a pointer to pixel bytes. It supports two memory modes:
- **View mode**: References external memory (`ownsData=false`)
- **Owned mode**: Stores bytes in `ownedData` (`ownsData=true`)

**Fields**:

- `int32_t width` - Image width in pixels.
- `int32_t height` - Image height in pixels.
- `int32_t step` - Number of bytes per row.
- `int32_t channels` - Number of channels per pixel.
- `const uint8_t* data` - Pointer to first pixel byte (may point to external or owned memory).
- `size_t dataSize` - Total readable bytes at `data`.
- `bool ownsData` - True if this instance owns pixel bytes via `ownedData`.
- `std::vector<uint8_t> ownedData` - Owned storage used when `ownsData=true`.

**Methods**:

- `void setOwnedCopy(const uint8_t* srcData, size_t srcSize)` - Copy external bytes into owned storage.
- `void setOwnedMove(std::vector<uint8_t>&& srcData)` - Move byte vector into owned storage without copying.
- `void setView(const uint8_t* srcData, size_t srcSize)` - Reference external memory without taking ownership.
- `bool empty() const` - Check whether buffer is effectively empty or invalid.
- `void clear()` - Reset metadata and release owned storage.

**Helper Conversions**:

- `ImageBuffer fromCImageBuffer(const DpxlImageBuffer& src, bool copyData = false)` - Convert C to C++.
- `DpxlImageBuffer toCImageBuffer(const ImageBuffer& src)` - Convert C++ to C.

### struct FrameInput

Input frame descriptor for image processing APIs.

**Description**: This structure describes a single image frame and its metadata. The image buffer is supplied by the caller through `frame`.

**Buffer Interpretation**:
- `frame.data` points to the first byte of the image buffer.
- `frame.dataSize` is the total readable byte size.
- `frame.width` and `frame.height` are in pixels.
- `frame.stride` is the byte distance between adjacent rows.
- `imageType` defines pixel format and channel layout.

**Fields**:

- `unsigned int timestamp` - Frame timestamp propagated by the caller (unit defined by caller).
- `ImageBuffer frame` - Input frame image buffer and memory layout information.
- `DP_IMAGE_TYPE imageType` - Pixel format of `frame`. Default: `BGRA_8888` (recommended for GL readback paths).
- `bool isStill` - True for still image input, false for streaming/video frame input.

### struct CoreAIUpperBodySegmentationOutput

`UpperbodySegmentationSDK` inference result.

**Description**: Contains processing status and segmentation output for a single frame. The output mask represents the detected human region with pixel-level foreground confidence.

**Fields**:

- `unsigned int timestamp` - Frame timestamp propagated from input context.
- `bool success` - True when segmentation succeeded and `segmentationMask` is valid.
- `ImageBuffer segmentationMask` - Pixel-level `UpperbodySegmentationSDK` mask image.

**Segmentation Mask Characteristics**:

- Single-channel 8-bit grayscale image (or equivalent binary matrix)
- Same resolution as input image
- Foreground confidence per pixel:
  - 255: confirmed foreground (human region)
  - 1–254: soft edge / transition region
  - 0: background

## CoreAI Class

### class xyz::deepixel::coreai::CoreAIUpperBodySegmentationNaver

High-performance `UpperbodySegmentationSDK` engine.

**Description**: This class performs real-time foreground/background separation. It generates a segmentation mask for the detected human region, enabling clean upper-body extraction from the background.

**Output Mask Characteristics**:
- Single-channel 8-bit grayscale image
- Same resolution as input image
- Each pixel ranges from 0–255, representing foreground probability.
- Pixel-level foreground confidence:
  - `255`: confirmed foreground (human region)
  - `1–254`: soft edge / transition region
  - `0`: background
  - Users may threshold (e.g., >128) to obtain a binary mask.

### Lifecycle and Inference Functions

**Lifecycle**:

- `DpxlCoreAIUpperBodySegmentationNaver* dpxl_coreai_upperbodyseg_naver_create(void)`
  - Create and initialize CoreAIUpperBodySegmentationNaver instance.
  - Returns: Opaque pointer to CoreAIUpperBodySegmentationNaver instance.

- `void dpxl_coreai_upperbodyseg_naver_destroy(DpxlCoreAIUpperBodySegmentationNaver* handle)`
  - Destroy CoreAIUpperBodySegmentationNaver instance.
  - Parameters: `handle` - Pointer to CoreAIUpperBodySegmentationNaver instance.

**Initialization**:

- `bool dpxl_coreai_upperbodyseg_naver_initialize(DpxlCoreAIUpperBodySegmentationNaver* handle)`
  - Initialize `UpperbodySegmentationSDK` with license key.
  - Parameters: `handle` - Pointer to CoreAIUpperBodySegmentationNaver instance.
  - Returns: `true` if initialization succeeds, `false` otherwise.

- `bool dpxl_coreai_upperbodyseg_naver_is_initialized(DpxlCoreAIUpperBodySegmentationNaver* handle)`
  - Check if algorithm is initialized.
  - Parameters: `handle` - Pointer to CoreAIUpperBodySegmentationNaver instance.
  - Returns: `true` if initialized, `false` otherwise.

**Inference**:

- `bool dpxl_coreai_upperbodyseg_naver_process(DpxlCoreAIUpperBodySegmentationNaver* handle, const DpxlFrameInput* data, double blurSigma, DpxlCoreAIUpperBodySegmentationOutput* output)`
  - Process image for `UpperbodySegmentationSDK`.
  - Parameters:
    - `handle` - Pointer to CoreAIUpperBodySegmentationNaver instance.
    - `data` - Processing data containing image information.
    - `blurSigma` - Gaussian blur sigma for segmentation post-processing (default: 0.5).
      - Lower values → sharper edges, potentially more noise
      - Higher values → smoother results, may reduce fine details
    - `output` - Output structure to store the result.
  - Returns: `true` if processing/result generation succeeds, `false` otherwise.

## C API

## Image and Frame Types

### enum DpxlImageType

C equivalent of `DP_IMAGE_TYPE`.

**Description**: Supported input image formats. Describes pixel layout of input frames used by processing APIs.

**Values**:

- `DPXL_IMAGE_TYPE_BGR_888` - 3-channel BGR, 8 bits per channel. Preferred internal format for algorithms.
- `DPXL_IMAGE_TYPE_RGB_888` - 3-channel RGB, 8 bits per channel.
- `DPXL_IMAGE_TYPE_YUV_420_888` - YUV 4:2:0 semi-planar format (NV21-compatible conversion path).
- `DPXL_IMAGE_TYPE_RGBA_8888` - 4-channel RGBA, 8 bits per channel.
- `DPXL_IMAGE_TYPE_BGRA_8888` - 4-channel BGRA, 8 bits per channel. Recommended for iOS readback paths.

**Platform Recommendations**:

- Android: `DPXL_IMAGE_TYPE_YUV_420_888` or `DPXL_IMAGE_TYPE_RGBA_8888`
- iOS: `DPXL_IMAGE_TYPE_BGRA_8888`

**Performance Note**: `DPXL_IMAGE_TYPE_BGR_888` is the fastest path because algorithms use BGR natively and typically avoid an additional color conversion step.

### struct DpxlImageBuffer

C image buffer descriptor with optional ownership.

**Description**: Stores image shape/stride metadata and a pointer to pixel bytes.

**Fields**:

- `int32_t width` - Image width in pixels.
- `int32_t height` - Image height in pixels.
- `int32_t step` - Number of bytes per row.
- `int32_t channels` - Number of channels per pixel.
- `const uint8_t* data` - Pointer to first pixel byte.
- `size_t dataSize` - Total readable bytes.
- `int32_t ownsData` - Ownership flag:
  - `0`: non-owning view (external memory)
  - `1`: owning semantics (owned memory)

### struct DpxlFrameInput

C input frame descriptor for image processing APIs.

**Description**: Describes a single image frame and its metadata in C format.

**Fields**:

- `uint32_t timestamp` - Frame timestamp propagated by the caller.
- `const void* data` - Pointer to image buffer.
- `size_t dataSize` - Total readable bytes.
- `int32_t width` - Image width in pixels.
- `int32_t height` - Image height in pixels.
- `int32_t stride` - Number of bytes per row.
- `DpxlImageType imageType` - Pixel format of the image.
- `bool isStill` - True for still image input, false for streaming/video frame input.

### struct DpxlCoreAIUpperBodySegmentationOutput

C `UpperbodySegmentationSDK` inference result.

**Description**: Contains processing status and segmentation output for a single frame.

**Fields**:

- `uint32_t timestamp` - Frame timestamp propagated from input context.
- `bool success` - True when segmentation succeeded and `segmentationMask` is valid.
- `DpxlImageBuffer segmentationMask` - Pixel-level `UpperbodySegmentationSDK` mask image.

**Segmentation Mask Characteristics**:

- Single-channel 8-bit grayscale image (or equivalent binary matrix)
- Same resolution as input image
- Foreground confidence per pixel:
  - `255`: confirmed foreground (human region)
  - `1–254`: soft edge / transition region
  - `0`: background

## CoreAI Handle and Functions

### Opaque Handle

`typedef struct DpxlCoreAIUpperBodySegmentationNaver DpxlCoreAIUpperBodySegmentationNaver;`

**Description**: Opaque handle for the C++ CoreAIUpperBodySegmentationNaver instance. This handle refers to a high-performance `UpperbodySegmentationSDK` engine that performs real-time foreground/background separation and generates a segmentation mask for the detected human region, enabling clean upper-body extraction from the background.

**Output Mask Characteristics**:
- Single-channel 8-bit grayscale image
- Same resolution as input image
- Each pixel ranges from 0–255, representing foreground probability.
- Pixel-level foreground confidence:
  - `255`: confirmed foreground (human region)
  - `1–254`: soft edge / transition region
  - `0`: background
  - Users may threshold (e.g., >128) to obtain a binary mask.

### Lifecycle and Inference Functions

**Creation and Destruction**:

- `DpxlCoreAIUpperBodySegmentationNaver* dpxl_coreai_upperbodyseg_naver_create(void)`
  - Create and initialize CoreAIUpperBodySegmentationNaver instance.
  - Returns: Opaque pointer to CoreAIUpperBodySegmentationNaver instance.

- `void dpxl_coreai_upperbodyseg_naver_destroy(DpxlCoreAIUpperBodySegmentationNaver* handle)`
  - Destroy CoreAIUpperBodySegmentationNaver instance.
  - Parameters: `handle` - Pointer to CoreAIUpperBodySegmentationNaver instance.

**Initialization**:

- `bool dpxl_coreai_upperbodyseg_naver_initialize(DpxlCoreAIUpperBodySegmentationNaver* handle)`
  - Initialize `UpperbodySegmentationSDK` with license key.
  - Parameters: `handle` - Pointer to CoreAIUpperBodySegmentationNaver instance.
  - Returns: `true` if initialization succeeds, `false` otherwise.

- `bool dpxl_coreai_upperbodyseg_naver_is_initialized(DpxlCoreAIUpperBodySegmentationNaver* handle)`
  - Check if algorithm is initialized.
  - Parameters: `handle` - Pointer to CoreAIUpperBodySegmentationNaver instance.
  - Returns: `true` if initialized, `false` otherwise.

**Inference**:

- `bool dpxl_coreai_upperbodyseg_naver_process(DpxlCoreAIUpperBodySegmentationNaver* handle, const DpxlFrameInput* data, double blurSigma, DpxlCoreAIUpperBodySegmentationOutput* output)`
  - Process image for `UpperbodySegmentationSDK`.
  - Parameters:
    - `handle` - Pointer to CoreAIUpperBodySegmentationNaver instance.
    - `data` - Processing data containing image information.
    - `blurSigma` - Gaussian blur sigma for segmentation post-processing (default: 0.5).
      - Lower values → sharper edges, potentially more noise
      - Higher values → smoother results, may reduce fine details
    - `output` - Output structure to store the result.
  - Returns: `true` if processing/result generation succeeds, `false` otherwise.

## Minimal Include Examples

### C++

```cpp
#include <humanx/humanx_naver.hpp>

using namespace xyz::deepixel;
using namespace xyz::deepixel::coreai;

CoreAIUpperBodySegmentationNaver engine;
if (engine.initialize()) {
    FrameInput in;
    // fill in.frame / in.imageType / in.timestamp / in.isStill
    auto out = engine.process(in, 0.5);
}
```

### C

```c
#include <humanx/humanx_naver_c.h>

DpxlCoreAIUpperBodySegmentationNaver* h = dpxl_coreai_upperbodyseg_naver_create();
if (h && dpxl_coreai_upperbodyseg_naver_initialize(h)) {
    DpxlFrameInput in = {0};
    DpxlCoreAIUpperBodySegmentationOutput out = {0};
    dpxl_coreai_upperbodyseg_naver_process(h, &in, 0.5, &out);
}
dpxl_coreai_upperbodyseg_naver_destroy(h);
```

## JavaScript API (WASM)

### Runtime Module Factory

- `HumanXNative(options?) -> Promise<Module>`
  - Emscripten module factory exported by `humanx_native_wasm.js`.
  - Typical usage:
    - `const wasmModule = await HumanXNative({ locateFile: (path) => path });`

**Common runtime exports**:

- `_malloc(size)` - Allocate bytes in WASM heap.
- `_free(ptr)` - Free previously allocated heap pointer.
- `HEAPU8` - Uint8 view for raw byte copy (`HEAPU8.set(...)`).

### Wrapper Class (Recommended)

In this repository, the recommended JS surface is the wrapper in `samples-naver/wasm-web-sample/humanx-wrapper.js`:

- `class CoreAIUpperBodySegmentation`
  - `constructor(wasmModule)`
  - `async initialize(): Promise<boolean>`
  - `async isInitialized(): Promise<boolean>`
  - `isProcessing(): boolean`
  - `async process(timestamp, dataPtr, dataSize, width, height, stride, imageType, isStill, blurSigma = 0.5): Promise<ProcessResult>`
  - `destroy(): void`

`ProcessResult` shape:

- `success: boolean`
- `timestamp: number`
- `segmentationMaskWidth: number`
- `segmentationMaskHeight: number`
- `segmentationMaskStep: number`
- `segmentationMask: number` (raw pointer in WASM heap)

### JS Input/Output Mapping

`CoreAIUpperBodySegmentation.process(...)` parameters map to C/C++ API concepts as follows:

- `timestamp` -> `DpxlFrameInput.timestamp`
- `dataPtr` + `dataSize` -> `DpxlFrameInput.data` + `DpxlFrameInput.dataSize`
- `width` / `height` / `stride` -> `DpxlFrameInput.width` / `height` / `stride`
- `imageType` -> `DpxlFrameInput.imageType`
- `isStill` -> `DpxlFrameInput.isStill`
- `blurSigma` -> `dpxl_coreai_upperbodyseg_naver_process(..., blurSigma, ...)`

### ImageType in Browser Sample

- Browser sample uses RGBA input from Canvas `ImageData`.
- Example: `wasmModule.ImageType.RGBA_8888` (converted to integer enum value before passing to `process`).

### Minimal JS Example (Wrapper)

```javascript
const wasmModule = await HumanXNative({ locateFile: (path) => path });
const seg = new CoreAIUpperBodySegmentation(wasmModule);

const ok = await seg.initialize();
if (!ok) throw new Error('initialize failed');

const imageSize = width * height * 4; // RGBA
const imagePtr = wasmModule._malloc(imageSize);
wasmModule.HEAPU8.set(rgbaBytes, imagePtr);

const imageType = Number(wasmModule.ImageType.RGBA_8888);
const result = await seg.process(
  Date.now() >>> 0,
  imagePtr,
  imageSize,
  width,
  height,
  width * 4,
  imageType,
  true,
  0.5
);

wasmModule._free(imagePtr);
seg.destroy();
```

### Memory and Lifetime Notes

- Always free input buffers allocated with `_malloc`.
- `result.segmentationMask` is a raw pointer to WASM memory, not a copied JS array.
- If you need persistent mask data, copy bytes out immediately (for example into `Uint8Array`) before subsequent native calls or destroy operations.

### Low-level Embind Objects (Advanced)

Wrapper internally uses embind classes exposed by WASM runtime, including:

- `FrameInput`
- `ImageBuffer`
- `CoreAIUpperBodySegmentationNaver`

Advanced users can call these directly, but wrapper usage is preferred for predictable object lifecycle and normalized result conversion.


