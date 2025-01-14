# dlib-face-recognition

Inspired by [a similar python library](https://github.com/ageitgey/face_recognition), 
`dlib-face-recognition` is a Rust library that binds to certain specific features of the [dlib C++ library](https://github.com/davisking/dlib).

This repository will dedicate itself to improve the library's content.

These include:

* An FHOG-based face detector.
* A CNN-based face detector (slower, but more powerful).
* A face landmark predictor for identifying specific landmarks (eyes, nose, etc) from face rectangles.
* A face encoding neural network for generating 128 dimensional face encodings that can be compared via their euclidean distances.

## Original Working

The original working is [here (unmaintaned; since Aug 2021)](https://github.com/expenses/face_recognition).

## Building

### Supported Platforms

* Linux { aarch64, x86_64 }
    - Ubuntu 20.04
* MacOS { x86_64 }
    - Apple Silicon (`Apple M1`)
* Windows { x86_64 }
    - Windows 10

For better maintaining, please let us know whether the other platforms support it.
Besides, you may claim us whether the specific platform should support it through `Issues` .

### Dependencies

* cmake
* blas
* lapack

For Windows, [`vcpkg`](https://vcpkg.io/en/getting-started.html) may help building both `blas` and `lapack` .
For other platforms such as Linux, package managers should support installing them.

### Building Native library

`dlib-face-recognition` requires dlib to be installed.

The C++ library `dlib` will be automatically installed via `dlib-face-recognition-sys` .

For building, this library uses `cmake` , so please make sure for getting [ `cmake` ](https://cmake.org/install/) .

### Building Rust package

`dlib-face-recognition` includes a `embed-all` feature flag that can be used with `cargo build --features embed-all` .

This will automatically download the face predictor, cnn face detector and face encoding neural network models (the fhog face detector is included in dlib and does not need to be downloaded). Alternatively, these models can be downloaded manually:

* CNN Face Detector: http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2  
* Landmark Predictor: http://dlib.net/files/mmod_human_face_detector.dat.bz2
* Face Recognition Net: http://dlib.net/files/dlib_face_recognition_resnet_model_v1.dat.bz2

If this feature flag is enabled, the matching structs will have `Default::default` implementations provided that allows you to load them without having to worry about file locations.

```bash
# Install cmake on a CUDA-enabled machine ...
cargo build --features embed-all
```

## Testing

There is one included test to recognize, and draw a face's points:

* `cargo run --features embed-all --example draw` -> To run the example.

There is two files to benchmark the code, and test some functions:

* `cargo test --features embed-all --test benchmarks` -> To run the benchmarks.
* `cargo test --features embed-all --test utilities_tests` -> To run the utilities tester.

## Examples

```bash
mkdir outputs
cargo run --features embed-all --example draw assets/obama_1.jpg outputs/obama_1.jpg
```
