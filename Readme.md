# DeepStream ESP32-CAM Integration

## Overview
This repository provides step-by-step instructions for making NVIDIA's DeepStream Test5 application compatible with ESP32-CAM streams. The ESP32-CAM is a low-cost camera module with built-in Wi-Fi, making it ideal for lightweight streaming applications. Integrating it with DeepStream enables efficient processing of live camera feeds in AI-powered video analytics pipelines.

## Background
The code was initially developed with DeepStream SDK 5.1. As of now, DeepStream 7.1 is available, and while the core changes remain relevant, users need to adapt these modifications to the newer version.

## Why ESP32-CAM?
- **Cost-effective**: Affordable compared to other IP cameras.
- **Wireless Connectivity**: Streams over Wi-Fi, reducing cabling.
- **Customizable**: Open to firmware modifications for tailored use.

## Key Changes
The modifications focus on handling ESP32-CAM RTSP streams by updating three primary functions across two files: `deepstream_source_bin.c` and `deepstream_sink_bin.c`.

## Step-by-Step Guide

### 1. Locate the DeepStream Source Code
For DeepStream 7.1, the source code is located at:
```
/opt/nvidia/deepstream/deepstream-7.1/sources/apps/apps-common/src/
```
For DeepStream 5.1 (reference in this repository):
```
/opt/nvidia/deepstream/deepstream-5.1/sources/apps/src/
```

### 2. Modify `cb_rtspsrc_select_stream` Function
- In `deepstream_source_bin.c` (DeepStream 7.1): This function is located from **line 529 to 585**.
- In this repository (DeepStream 5.1): Copy the corresponding code from **line 509 to 559** and replace it in the 7.1 code.

### 3. Modify `create_rtsp_src_bin` Function
- In `deepstream_source_bin.c` (DeepStream 7.1): This function is located from **line 892 to 1108**.
- In this repository (DeepStream 5.1): Copy the corresponding code from **line 759 to 984** and replace it in the 7.1 code.

### 4. Modify Sink Configuration
- In `deepstream_sink_bin.c` (DeepStream 7.1): Modify the code from **line 306 to 691**.
- In this repository (DeepStream 5.1): Copy the corresponding code from **line 304 to 627** and replace it in the 7.1 code.

### 5. Update Header Files
In addition to the C code, you need to update the header files located in the `includes/` directory of this repository:
- **deepstream_config.h**: Add the following lines:
```c
#define NVDS_ELEM_DECODER "nvv4l2decoder"
#define NVDS_ELEM_VIDEO_CONVERT "videoconvert"
#define NVDS_ELEM_ENC_MJPEG_SW "jpegenc"
```
- **deepstream_sources.h**: Under `typedef struct`, add:
```c
GstElement *cap_filter2;
GstElement *capsfilterRate;
GstElement *depay;
GstElement *parser;
GstElement *jitterbuffer;
GstElement *videorate;
GstElement *enc_que;
GstElement *dec_que;
GstElement *decoder;
```
- **deepstream_sinks.h**: Under `typedef enum`, add:
```c
NV_DS_ENCODER_MJPEG
```

## Directory Structure
```
/opt/nvidia/deepstream/deepstream-5.1/sources/apps
├── src
│   ├── deepstream_source_bin.c
│   └── deepstream_sink_bin.c
└── includes
    ├── deepstream_config.h
    ├── deepstream_sources.h
    └── deepstream_sinks.h
```

## Compatibility
- Developed and tested with **DeepStream 5.1**.
- Adaptable for **DeepStream 7.1** with the steps above.

## Future Work
- Verify compatibility with DeepStream 7.1.
- Optimize performance for ESP32-CAM streams.

## Acknowledgments
- NVIDIA for the DeepStream SDK.
- ESP32-CAM community for documentation and insights.

## Contact
For questions or contributions, feel free to reach out.

