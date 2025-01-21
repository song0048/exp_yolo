---
comments: true
description: Learn how to export YOLO11 models to RKNN format for efficient deployment on Rockchip platforms with enhanced performance.
keywords: YOLO11, RKNN, model export, Ultralytics, Rockchip, machine learning, model deployment, computer vision, deep learning
---

# RKNN Export for Ultralytics YOLO11 Models

When deploying computer vision models on embedded devices, especially those powered by Rockchip processors, having a compatible model format is essential. Exporting [Ultralytics YOLO11](https://github.com/ultralytics/ultralytics) models to RKNN format ensures optimized performance and compatibility with Rockchip's hardware. This guide will walk you through converting your YOLO11 models to RKNN format, enabling efficient deployment on Rockchip platforms.

!!! note

    This guide has been tested with [Radxa Rock 5B](https://radxa.com/products/rock5/5b) which is based on Rockchip RK3588 and [Radxa Zero 3W](https://radxa.com/products/zeros/zero3w) which is based on Rockchip RK3566. It is expected to work across other Rockchip-based devices which supports [rknn-toolkit2](https://github.com/airockchip/rknn-toolkit2) such as RK3576, RK3568, RK3562, RV1103, RV1106, RV1103B, RV1106B and RK2118.

<p align="center">
  <img width="100%" src="https://www.rock-chips.com/Images/web/solution/AI/chip_s.png" alt="RKNN">
</p>

## What is Rockchip?

Renowned for delivering versatile and power-efficient solutions, Rockchip designs advanced System-on-Chips (SoCs) that power a wide range of consumer electronics, industrial applications, and AI technologies. With ARM-based architecture, built-in Neural Processing Units (NPUs), and high-resolution multimedia support, Rockchip SoCs enable cutting-edge performance for devices like tablets, smart TVs, IoT systems, and edge AI applications. Companies like Radxa, ASUS, Pine64, Orange Pi, Odroid, Khadas, and Banana Pi offer a variety of products based on Rockchip SoCs, further extending their reach and impact across diverse markets.

## RKNN Toolkit

The [RKNN Toolkit](https://github.com/airockchip/rknn-toolkit2) is a set of tools and libraries provided by Rockchip to facilitate the deployment of deep learning models on their hardware platforms. RKNN, or Rockchip Neural Network, is the proprietary format used by these tools. RKNN models are designed to take full advantage of the hardware acceleration provided by Rockchip's NPU (Neural Processing Unit), ensuring high performance in AI tasks on devices like RK3588, RK3566, RV1103, RV1106, and other Rockchip-powered systems.

## Key Features of RKNN Models

RKNN models offer several advantages for deployment on Rockchip platforms:

- **Optimized for NPU**: RKNN models are specifically optimized to run on Rockchip's NPUs, ensuring maximum performance and efficiency.
- **Low Latency**: The RKNN format minimizes inference latency, which is critical for real-time applications on edge devices.
- **Platform-Specific Customization**: RKNN models can be tailored to specific Rockchip platforms, enabling better utilization of hardware resources.

## Flash OS to Rockchip hardware

The first step after getting your hands on a Rockchip-based device is to flash an OS so that that the hardware can boot into a working environment. In this guide we will point to getting started guides of the two devices that we tested which are Radxa Rock 5B and Radxa Zero 3W.

- [Radxa Rock 5B Getting Started Guide](https://docs.radxa.com/en/rock5/rock5b)
- [Radxa Zero 3W Getting Started Guide](https://docs.radxa.com/en/zero/zero3)

## Export to RKNN: Converting Your YOLO11 Model

Export an Ultralytics YOLO11 model to RKNN format and run inference with the exported model.

!!! note

    Make sure to use an X86-based Linux PC to export the model to RKNN because exporting on Rockchip-based devices (ARM64) are not supported.

### Installation

To install the required packages, run:

!!! Tip "Installation"

    === "CLI"

        ```bash
        # Install the required package for YOLO11
        pip install ultralytics
        ```

For detailed instructions and best practices related to the installation process, check our [Ultralytics Installation guide](../quickstart.md). While installing the required packages for YOLO11, if you encounter any difficulties, consult our [Common Issues guide](../guides/yolo-common-issues.md) for solutions and tips.

### Usage

!!! note

    Export is currently only supported for detection models. More model support will be coming in the future.

!!! Example "Usage"

    === "Python"

        ```python
        from ultralytics import YOLO

        # Load the YOLO11 model
        model = YOLO("yolo11n.pt")

        # Export the model to RKNN format
        # Here name can be one of rk3588, rk3576, rk3566, rk3568, rk3562, rv1103, rv1106, rv1103b, rv1106b, rk2118
        model.export(format="rknn", args={"name": "rk3588"})  # creates '/yolo11n_rknn_model'
        ```

    === "CLI"

        ```bash
        # Export a YOLO11n PyTorch model to RKNN format
        # Here name can be one of rk3588, rk3576, rk3566, rk3568, rk3562, rv1103, rv1106, rv1103b, rv1106b, rk2118
        yolo export model=yolo11n.pt format=rknn name=rk3588  # creates '/yolo11n_rknn_model'
        ```

For more details about the export process, visit the [Ultralytics documentation page on exporting](../modes/export.md).

## Deploying Exported YOLO11 RKNN Models

Once you've successfully exported your Ultralytics YOLO11 models to RKNN format, the next step is deploying these models on Rockchip-based devices.

### Installation

To install the required packages, run:

!!! Tip "Installation"

    === "CLI"

        ```bash
        # Install the required package for YOLO11
        pip install ultralytics
        ```

### Usage

!!! Example "Usage"

    === "Python"

        ```python
        from ultralytics import YOLO

        # Load the exported RKNN model
        rknn_model = YOLO("./yolo11n_rknn_model")

        # Run inference
        results = rknn_model("https://ultralytics.com/images/bus.jpg")
        ```

    === "CLI"

        ```bash
        # Run inference with the exported model
        yolo predict model='./yolo11n_rknn_model' source='https://ultralytics.com/images/bus.jpg'
        ```

## Benchmarks

YOLO11 benchmarks below were run by the Ultralytics team on Radxa Rock 5B based on Rockchip RK3588 with `rknn` model format measuring speed and accuracy.

| Model   | Format | Status | Size (MB) | mAP50-95(B) | Inference time (ms/im) |
| ------- | ------ | ------ | --------- | ----------- | ---------------------- |
| YOLO11n | rknn   | ✅     | 7.4       | 0.61        | 99.5                   |
| YOLO11s | rknn   | ✅     | 20.7      | 0.741       | 122.3                  |
| YOLO11m | rknn   | ✅     | 41.9      | 0.764       | 298.0                  |
| YOLO11l | rknn   | ✅     | 53.3      | 0.72        | 319.6                  |
| YOLO11x | rknn   | ✅     | 114.6     | 0.828       | 632.1                  |

!!! note

    Validation for the above benchmark was done using coco8 dataset

## Summary

In this guide, you've learned how to export Ultralytics YOLO11 models to RKNN format to enhance their deployment on Rockchip platforms. You were also introduced to the RKNN Toolkit and the specific advantages of using RKNN models for edge AI applications.

For further details on usage, visit the [RKNN official documentation](https://github.com/airockchip/rknn-toolkit2).

Also, if you'd like to know more about other Ultralytics YOLO11 integrations, visit our [integration guide page](../integrations/index.md). You'll find plenty of useful resources and insights there.