# Copyright 2019 Google LLC
#
# This source code is licensed under the BSD-style license found in the
# LICENSE file in the root directory of this source tree.
#
# Description:
#   XNNPACK - optimized floating-point neural network operators library

load(":build_defs.bzl", "xnnpack_aggregate_library", "xnnpack_benchmark", "xnnpack_binary", "xnnpack_cc_library", "xnnpack_min_size_copts", "xnnpack_optional_armcl_copts", "xnnpack_optional_armcl_deps", "xnnpack_optional_dnnl_copts", "xnnpack_optional_dnnl_deps", "xnnpack_optional_gemmlowp_copts", "xnnpack_optional_gemmlowp_deps", "xnnpack_optional_ruy_copts", "xnnpack_optional_ruy_deps", "xnnpack_optional_tflite_copts", "xnnpack_optional_tflite_deps", "xnnpack_std_copts", "xnnpack_std_cxxopts", "xnnpack_unit_test", "xnnpack_visibility")

licenses(["notice"])

exports_files(["LICENSE"])

OPERATOR_BENCHMARK_DEPS = [
    ":XNNPACK",
    ":bench_utils",
    "@cpuinfo",
    "@pthreadpool",
]

MICROKERNEL_BENCHMARK_DEPS = [
    ":ukernels",
    ":bench_utils",
    ":enable_assembly",
    "@cpuinfo",
    "@FP16",
    "@pthreadpool",
]

ACCURACY_EVAL_DEPS = [
    ":XNNPACK",
    ":ukernels",
    "@FP16",
    "@pthreadpool",
]

MICROKERNEL_TEST_DEPS = [
    ":ukernels",
    ":enable_assembly",
    "@cpuinfo",
    "@FP16",
    "@pthreadpool",
]

OPERATOR_TEST_DEPS = [
    ":XNNPACK",
    "@pthreadpool",
    "@FP16",
]

OPERATOR_SRCS = [
    "src/add-nc.c",
    "src/argmax-pooling-nhwc.c",
    "src/average-pooling-nhwc.c",
    "src/binary-elementwise-nd.c",
    "src/channel-pad-nc.c",
    "src/channel-shuffle-nc.c",
    "src/clamp-nc.c",
    "src/convolution-nchw.c",
    "src/convolution-nhwc.c",
    "src/deconvolution-nhwc.c",
    "src/fully-connected-nc.c",
    "src/global-average-pooling-ncw.c",
    "src/global-average-pooling-nwc.c",
    "src/hardswish-nc.c",
    "src/leaky-relu-nc.c",
    "src/max-pooling-nhwc.c",
    "src/prelu-nc.c",
    "src/resize-bilinear-nhwc.c",
    "src/sigmoid-nc.c",
    "src/softmax-nc.c",
    "src/unpooling-nhwc.c",
]

TABLE_SRCS = [
    "src/tables/exp2-k-over-64.c",
    "src/tables/exp2-k-over-2048.c",
]

SCALAR_UKERNELS = [
    "src/f32-argmaxpool/4x-scalar-c1.c",
    "src/f32-argmaxpool/9p8x-scalar-c1.c",
    "src/f32-argmaxpool/9x-scalar-c1.c",
    "src/f32-avgpool/9p8x-minmax-scalar-c1.c",
    "src/f32-avgpool/9x-minmax-scalar-c1.c",
    "src/f32-clamp/gen/scalar-x1.c",
    "src/f32-clamp/gen/scalar-x2.c",
    "src/f32-clamp/gen/scalar-x4.c",
    "src/f32-conv-hwc/3x3s2p0p1c3x4-scalar-1x1.c",
    "src/f32-conv-hwc/3x3s2p1c3x4-scalar-1x1.c",
    "src/f32-conv-hwc2spchw/3x3s2p1c3x4-scalar-1x1.c",
    "src/f32-dwconv-spchw/3x3p1-scalar.c",
    "src/f32-dwconv-spchw/3x3s2p1-scalar.c",
    "src/f32-dwconv-spchw/5x5p2-scalar.c",
    "src/f32-dwconv-spchw/5x5s2p2-scalar.c",
    "src/f32-dwconv/gen/up1x4-scalar-acc2.c",
    "src/f32-dwconv/gen/up1x4-scalar.c",
    "src/f32-dwconv/gen/up1x9-scalar-acc2.c",
    "src/f32-dwconv/gen/up1x9-scalar.c",
    "src/f32-dwconv/gen/up1x25-scalar-acc2.c",
    "src/f32-dwconv/gen/up1x25-scalar.c",
    "src/f32-dwconv/gen/up2x4-scalar-acc2.c",
    "src/f32-dwconv/gen/up2x4-scalar.c",
    "src/f32-dwconv/gen/up2x9-scalar-acc2.c",
    "src/f32-dwconv/gen/up2x9-scalar.c",
    "src/f32-dwconv/gen/up2x25-scalar-acc2.c",
    "src/f32-dwconv/gen/up2x25-scalar.c",
    "src/f32-dwconv/gen/up1x4-minmax-scalar-acc2.c",
    "src/f32-dwconv/gen/up1x4-minmax-scalar.c",
    "src/f32-dwconv/gen/up1x9-minmax-scalar-acc2.c",
    "src/f32-dwconv/gen/up1x9-minmax-scalar.c",
    "src/f32-dwconv/gen/up1x25-minmax-scalar-acc2.c",
    "src/f32-dwconv/gen/up1x25-minmax-scalar.c",
    "src/f32-dwconv/gen/up2x4-minmax-scalar-acc2.c",
    "src/f32-dwconv/gen/up2x4-minmax-scalar.c",
    "src/f32-dwconv/gen/up2x9-minmax-scalar-acc2.c",
    "src/f32-dwconv/gen/up2x9-minmax-scalar.c",
    "src/f32-dwconv/gen/up2x25-minmax-scalar-acc2.c",
    "src/f32-dwconv/gen/up2x25-minmax-scalar.c",
    "src/f32-gavgpool-spchw/scalar-x1.c",
    "src/f32-gavgpool/7p7x-minmax-scalar-c1.c",
    "src/f32-gavgpool/7x-minmax-scalar-c1.c",
    "src/f32-gemm/gen-inc/1x4inc-minmax-scalar.c",
    "src/f32-gemm/gen-inc/2x4inc-minmax-scalar.c",
    "src/f32-gemm/gen-inc/4x4inc-minmax-scalar.c",
    "src/f32-gemm/gen/1x4-scalar.c",
    "src/f32-gemm/gen/2x4-scalar.c",
    "src/f32-gemm/gen/4x2-scalar.c",
    "src/f32-gemm/gen/4x4-scalar.c",
    "src/f32-gemm/gen/1x4-minmax-scalar.c",
    "src/f32-gemm/gen/2x4-minmax-scalar.c",
    "src/f32-gemm/gen/4x2-minmax-scalar.c",
    "src/f32-gemm/gen/4x4-minmax-scalar.c",
    "src/f32-hswish/gen/scalar-x1.c",
    "src/f32-hswish/gen/scalar-x2.c",
    "src/f32-hswish/gen/scalar-x4.c",
    "src/f32-ibilinear/gen/scalar-c1.c",
    "src/f32-ibilinear/gen/scalar-c2.c",
    "src/f32-ibilinear/gen/scalar-c4.c",
    "src/f32-igemm/gen/1x4-scalar.c",
    "src/f32-igemm/gen/2x4-scalar.c",
    "src/f32-igemm/gen/4x2-scalar.c",
    "src/f32-igemm/gen/4x4-scalar.c",
    "src/f32-igemm/gen/1x4-minmax-scalar.c",
    "src/f32-igemm/gen/2x4-minmax-scalar.c",
    "src/f32-igemm/gen/4x2-minmax-scalar.c",
    "src/f32-igemm/gen/4x4-minmax-scalar.c",
    "src/f32-maxpool/9p8x-minmax-scalar-c1.c",
    "src/f32-pavgpool/9p8x-minmax-scalar-c1.c",
    "src/f32-pavgpool/9x-minmax-scalar-c1.c",
    "src/f32-ppmm/gen/2x4-minmax-scalar.c",
    "src/f32-ppmm/gen/3x3-minmax-scalar.c",
    "src/f32-ppmm/gen/4x2-minmax-scalar.c",
    "src/f32-ppmm/gen/4x4-minmax-scalar.c",
    "src/f32-prelu/gen/scalar-2x1.c",
    "src/f32-prelu/gen/scalar-2x4.c",
    "src/f32-raddstoreexpminusmax/gen/scalar-lut64-p2-x1.c",
    "src/f32-raddstoreexpminusmax/gen/scalar-lut64-p2-x2.c",
    "src/f32-raddstoreexpminusmax/gen/scalar-lut64-p2-x2-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/scalar-lut64-p2-x4.c",
    "src/f32-raddstoreexpminusmax/gen/scalar-lut64-p2-x4-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/scalar-lut64-p2-x4-acc4.c",
    "src/f32-raddstoreexpminusmax/gen/scalar-p5-x1.c",
    "src/f32-raddstoreexpminusmax/gen/scalar-p5-x2.c",
    "src/f32-raddstoreexpminusmax/gen/scalar-p5-x2-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/scalar-p5-x4.c",
    "src/f32-raddstoreexpminusmax/gen/scalar-p5-x4-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/scalar-p5-x4-acc4.c",
    "src/f32-rmax/scalar.c",
    "src/f32-sigmoid/gen/scalar-lut2048-p1-div-x1.c",
    "src/f32-sigmoid/gen/scalar-lut2048-p1-div-x2.c",
    "src/f32-sigmoid/gen/scalar-lut2048-p1-div-x4.c",
    "src/f32-sigmoid/gen/scalar-lut64-p2-div-x1.c",
    "src/f32-sigmoid/gen/scalar-lut64-p2-div-x2.c",
    "src/f32-sigmoid/gen/scalar-lut64-p2-div-x4.c",
    "src/f32-sigmoid/gen/scalar-p5-div-x1.c",
    "src/f32-sigmoid/gen/scalar-p5-div-x2.c",
    "src/f32-sigmoid/gen/scalar-p5-div-x4.c",
    "src/f32-spmm/gen/1x1-minmax-scalar-pipelined.c",
    "src/f32-spmm/gen/1x1-minmax-scalar.c",
    "src/f32-spmm/gen/2x1-minmax-scalar-pipelined.c",
    "src/f32-spmm/gen/2x1-minmax-scalar.c",
    "src/f32-spmm/gen/4x1-minmax-scalar-pipelined.c",
    "src/f32-spmm/gen/4x1-minmax-scalar.c",
    "src/f32-spmm/gen/8x1-minmax-scalar-pipelined.c",
    "src/f32-spmm/gen/8x1-minmax-scalar.c",
    "src/f32-spmm/gen/8x2-minmax-scalar.c",
    "src/f32-spmm/gen/8x4-minmax-scalar.c",
    "src/f32-vbinary/gen/vadd-minmax-scalar-x1.c",
    "src/f32-vbinary/gen/vadd-minmax-scalar-x2.c",
    "src/f32-vbinary/gen/vadd-minmax-scalar-x4.c",
    "src/f32-vbinary/gen/vaddc-minmax-scalar-x1.c",
    "src/f32-vbinary/gen/vaddc-minmax-scalar-x2.c",
    "src/f32-vbinary/gen/vaddc-minmax-scalar-x4.c",
    "src/f32-vbinary/gen/vdiv-minmax-scalar-x1.c",
    "src/f32-vbinary/gen/vdiv-minmax-scalar-x2.c",
    "src/f32-vbinary/gen/vdiv-minmax-scalar-x4.c",
    "src/f32-vbinary/gen/vdivc-minmax-scalar-x1.c",
    "src/f32-vbinary/gen/vdivc-minmax-scalar-x2.c",
    "src/f32-vbinary/gen/vdivc-minmax-scalar-x4.c",
    "src/f32-vbinary/gen/vmax-scalar-x1.c",
    "src/f32-vbinary/gen/vmax-scalar-x2.c",
    "src/f32-vbinary/gen/vmax-scalar-x4.c",
    "src/f32-vbinary/gen/vmaxc-scalar-x1.c",
    "src/f32-vbinary/gen/vmaxc-scalar-x2.c",
    "src/f32-vbinary/gen/vmaxc-scalar-x4.c",
    "src/f32-vbinary/gen/vmin-scalar-x1.c",
    "src/f32-vbinary/gen/vmin-scalar-x2.c",
    "src/f32-vbinary/gen/vmin-scalar-x4.c",
    "src/f32-vbinary/gen/vminc-scalar-x1.c",
    "src/f32-vbinary/gen/vminc-scalar-x2.c",
    "src/f32-vbinary/gen/vminc-scalar-x4.c",
    "src/f32-vbinary/gen/vmul-minmax-scalar-x1.c",
    "src/f32-vbinary/gen/vmul-minmax-scalar-x2.c",
    "src/f32-vbinary/gen/vmul-minmax-scalar-x4.c",
    "src/f32-vbinary/gen/vmulc-minmax-scalar-x1.c",
    "src/f32-vbinary/gen/vmulc-minmax-scalar-x2.c",
    "src/f32-vbinary/gen/vmulc-minmax-scalar-x4.c",
    "src/f32-vbinary/gen/vrdivc-minmax-scalar-x1.c",
    "src/f32-vbinary/gen/vrdivc-minmax-scalar-x2.c",
    "src/f32-vbinary/gen/vrdivc-minmax-scalar-x4.c",
    "src/f32-vbinary/gen/vrsubc-minmax-scalar-x1.c",
    "src/f32-vbinary/gen/vrsubc-minmax-scalar-x2.c",
    "src/f32-vbinary/gen/vrsubc-minmax-scalar-x4.c",
    "src/f32-vbinary/gen/vsub-minmax-scalar-x1.c",
    "src/f32-vbinary/gen/vsub-minmax-scalar-x2.c",
    "src/f32-vbinary/gen/vsub-minmax-scalar-x4.c",
    "src/f32-vbinary/gen/vsubc-minmax-scalar-x1.c",
    "src/f32-vbinary/gen/vsubc-minmax-scalar-x2.c",
    "src/f32-vbinary/gen/vsubc-minmax-scalar-x4.c",
    "src/f32-vmulcaddc/gen/c1-minmax-scalar-2x.c",
    "src/f32-vmulcaddc/gen/c2-minmax-scalar-2x.c",
    "src/f32-vmulcaddc/gen/c4-minmax-scalar-2x.c",
    "src/math/expminus-scalar-lut2048-p1.c",
    "src/math/expminus-scalar-lut64-p2.c",
    "src/math/expminus-scalar-p5.c",
    "src/math/sigmoid-scalar-lut2048-p1-div.c",
    "src/math/sigmoid-scalar-lut64-p2-div.c",
    "src/math/sigmoid-scalar-p5-div.c",
    "src/q8-avgpool/9p8x-minmax-scalar-c1.c",
    "src/q8-avgpool/9x-minmax-scalar-c1.c",
    "src/q8-dwconv/up1x9-minmax-scalar.c",
    "src/q8-gavgpool/7p7x-minmax-scalar-c1.c",
    "src/q8-gavgpool/7x-minmax-scalar-c1.c",
    "src/q8-gemm/2x2-minmax-scalar.c",
    "src/q8-igemm/2x2-minmax-scalar.c",
    "src/q8-vadd/minmax-scalar.c",
    "src/u8-clamp/scalar-x4.c",
    "src/u8-lut32norm/scalar.c",
    "src/u8-maxpool/9p8x-minmax-scalar-c1.c",
    "src/u8-rmax/scalar.c",
    "src/x32-packx/x2-scalar.c",
    "src/x32-packx/x3-scalar.c",
    "src/x32-packx/x4-scalar.c",
    "src/x32-pad/x2-scalar.c",
    "src/x32-unpool/scalar.c",
    "src/x32-zip/x2-scalar.c",
    "src/x32-zip/x3-scalar.c",
    "src/x32-zip/x4-scalar.c",
    "src/x32-zip/xm-scalar.c",
    "src/x8-lut/scalar.c",
    "src/x8-zip/x2-scalar.c",
    "src/x8-zip/x3-scalar.c",
    "src/x8-zip/x4-scalar.c",
    "src/x8-zip/xm-scalar.c",
    "src/requantization/precise-scalar.c",
    "src/requantization/fp32-scalar.c",
    "src/requantization/q31-scalar.c",
    "src/requantization/gemmlowp-scalar.c",
]

WASM_UKERNELS = [
    "src/f32-avgpool/9p8x-minmax-wasm-c1.c",
    "src/f32-avgpool/9x-minmax-wasm-c1.c",
    "src/f32-clamp/gen/wasm-x1.c",
    "src/f32-clamp/gen/wasm-x2.c",
    "src/f32-clamp/gen/wasm-x4.c",
    "src/f32-dwconv/gen/up1x4-wasm-acc2.c",
    "src/f32-dwconv/gen/up1x4-wasm.c",
    "src/f32-dwconv/gen/up1x9-wasm-acc2.c",
    "src/f32-dwconv/gen/up1x9-wasm.c",
    "src/f32-dwconv/gen/up1x25-wasm-acc2.c",
    "src/f32-dwconv/gen/up1x25-wasm.c",
    "src/f32-dwconv/gen/up2x4-wasm-acc2.c",
    "src/f32-dwconv/gen/up2x4-wasm.c",
    "src/f32-dwconv/gen/up2x9-wasm-acc2.c",
    "src/f32-dwconv/gen/up2x9-wasm.c",
    "src/f32-dwconv/gen/up2x25-wasm-acc2.c",
    "src/f32-dwconv/gen/up2x25-wasm.c",
    "src/f32-dwconv/gen/up1x4-minmax-wasm-acc2.c",
    "src/f32-dwconv/gen/up1x4-minmax-wasm.c",
    "src/f32-dwconv/gen/up1x9-minmax-wasm-acc2.c",
    "src/f32-dwconv/gen/up1x9-minmax-wasm.c",
    "src/f32-dwconv/gen/up1x25-minmax-wasm-acc2.c",
    "src/f32-dwconv/gen/up1x25-minmax-wasm.c",
    "src/f32-dwconv/gen/up2x4-minmax-wasm-acc2.c",
    "src/f32-dwconv/gen/up2x4-minmax-wasm.c",
    "src/f32-dwconv/gen/up2x9-minmax-wasm-acc2.c",
    "src/f32-dwconv/gen/up2x9-minmax-wasm.c",
    "src/f32-dwconv/gen/up2x25-minmax-wasm-acc2.c",
    "src/f32-dwconv/gen/up2x25-minmax-wasm.c",
    "src/f32-gavgpool/7p7x-minmax-wasm-c1.c",
    "src/f32-gavgpool/7x-minmax-wasm-c1.c",
    "src/f32-gemm/gen-inc/1x4inc-minmax-wasm.c",
    "src/f32-gemm/gen-inc/2x4inc-minmax-wasm.c",
    "src/f32-gemm/gen-inc/4x4inc-minmax-wasm.c",
    "src/f32-gemm/gen/1x4-wasm.c",
    "src/f32-gemm/gen/2x4-wasm.c",
    "src/f32-gemm/gen/4x2-wasm.c",
    "src/f32-gemm/gen/4x4-wasm.c",
    "src/f32-gemm/gen/1x4-minmax-wasm.c",
    "src/f32-gemm/gen/2x4-minmax-wasm.c",
    "src/f32-gemm/gen/4x2-minmax-wasm.c",
    "src/f32-gemm/gen/4x4-minmax-wasm.c",
    "src/f32-hswish/gen/wasm-x1.c",
    "src/f32-hswish/gen/wasm-x2.c",
    "src/f32-hswish/gen/wasm-x4.c",
    "src/f32-igemm/gen/1x4-wasm.c",
    "src/f32-igemm/gen/2x4-wasm.c",
    "src/f32-igemm/gen/4x2-wasm.c",
    "src/f32-igemm/gen/4x4-wasm.c",
    "src/f32-igemm/gen/1x4-minmax-wasm.c",
    "src/f32-igemm/gen/2x4-minmax-wasm.c",
    "src/f32-igemm/gen/4x2-minmax-wasm.c",
    "src/f32-igemm/gen/4x4-minmax-wasm.c",
    "src/f32-maxpool/9p8x-minmax-wasm-c1.c",
    "src/f32-pavgpool/9p8x-minmax-wasm-c1.c",
    "src/f32-pavgpool/9x-minmax-wasm-c1.c",
    "src/f32-vbinary/gen/vadd-minmax-wasm-x1.c",
    "src/f32-vbinary/gen/vadd-minmax-wasm-x2.c",
    "src/f32-vbinary/gen/vadd-minmax-wasm-x4.c",
    "src/f32-vbinary/gen/vaddc-minmax-wasm-x1.c",
    "src/f32-vbinary/gen/vaddc-minmax-wasm-x2.c",
    "src/f32-vbinary/gen/vaddc-minmax-wasm-x4.c",
    "src/f32-vbinary/gen/vdiv-minmax-wasm-x1.c",
    "src/f32-vbinary/gen/vdiv-minmax-wasm-x2.c",
    "src/f32-vbinary/gen/vdiv-minmax-wasm-x4.c",
    "src/f32-vbinary/gen/vdivc-minmax-wasm-x1.c",
    "src/f32-vbinary/gen/vdivc-minmax-wasm-x2.c",
    "src/f32-vbinary/gen/vdivc-minmax-wasm-x4.c",
    "src/f32-vbinary/gen/vmax-wasm-x1.c",
    "src/f32-vbinary/gen/vmax-wasm-x2.c",
    "src/f32-vbinary/gen/vmax-wasm-x4.c",
    "src/f32-vbinary/gen/vmaxc-wasm-x1.c",
    "src/f32-vbinary/gen/vmaxc-wasm-x2.c",
    "src/f32-vbinary/gen/vmaxc-wasm-x4.c",
    "src/f32-vbinary/gen/vmin-wasm-x1.c",
    "src/f32-vbinary/gen/vmin-wasm-x2.c",
    "src/f32-vbinary/gen/vmin-wasm-x4.c",
    "src/f32-vbinary/gen/vminc-wasm-x1.c",
    "src/f32-vbinary/gen/vminc-wasm-x2.c",
    "src/f32-vbinary/gen/vminc-wasm-x4.c",
    "src/f32-vbinary/gen/vmul-minmax-wasm-x1.c",
    "src/f32-vbinary/gen/vmul-minmax-wasm-x2.c",
    "src/f32-vbinary/gen/vmul-minmax-wasm-x4.c",
    "src/f32-vbinary/gen/vmulc-minmax-wasm-x1.c",
    "src/f32-vbinary/gen/vmulc-minmax-wasm-x2.c",
    "src/f32-vbinary/gen/vmulc-minmax-wasm-x4.c",
    "src/f32-vbinary/gen/vrdivc-minmax-wasm-x1.c",
    "src/f32-vbinary/gen/vrdivc-minmax-wasm-x2.c",
    "src/f32-vbinary/gen/vrdivc-minmax-wasm-x4.c",
    "src/f32-vbinary/gen/vrsubc-minmax-wasm-x1.c",
    "src/f32-vbinary/gen/vrsubc-minmax-wasm-x2.c",
    "src/f32-vbinary/gen/vrsubc-minmax-wasm-x4.c",
    "src/f32-vbinary/gen/vsub-minmax-wasm-x1.c",
    "src/f32-vbinary/gen/vsub-minmax-wasm-x2.c",
    "src/f32-vbinary/gen/vsub-minmax-wasm-x4.c",
    "src/f32-vbinary/gen/vsubc-minmax-wasm-x1.c",
    "src/f32-vbinary/gen/vsubc-minmax-wasm-x2.c",
    "src/f32-vbinary/gen/vsubc-minmax-wasm-x4.c",
    "src/f32-vmulcaddc/gen/c1-minmax-wasm-2x.c",
    "src/f32-vmulcaddc/gen/c2-minmax-wasm-2x.c",
    "src/f32-vmulcaddc/gen/c4-minmax-wasm-2x.c",
]

PSIMD_FASTMATH_UKERNELS = [
    "src/f32-argmaxpool/4x-psimd-c4.c",
    "src/f32-argmaxpool/9p8x-psimd-c4.c",
    "src/f32-argmaxpool/9x-psimd-c4.c",
    "src/f32-avgpool/9p8x-minmax-psimd-c4.c",
    "src/f32-avgpool/9x-minmax-psimd-c4.c",
    "src/f32-clamp/gen/psimd-x4.c",
    "src/f32-clamp/gen/psimd-x8.c",
    "src/f32-dwconv/gen/up4x25-minmax-psimd-acc2.c",
    "src/f32-dwconv/gen/up4x25-minmax-psimd.c",
    "src/f32-dwconv/gen/up4x4-minmax-psimd-acc2.c",
    "src/f32-dwconv/gen/up4x4-minmax-psimd.c",
    "src/f32-dwconv/gen/up4x9-minmax-psimd-acc2.c",
    "src/f32-dwconv/gen/up4x9-minmax-psimd.c",
    "src/f32-dwconv/gen/up8x25-minmax-psimd-acc2.c",
    "src/f32-dwconv/gen/up8x25-minmax-psimd.c",
    "src/f32-dwconv/gen/up8x4-minmax-psimd-acc2.c",
    "src/f32-dwconv/gen/up8x4-minmax-psimd.c",
    "src/f32-dwconv/gen/up8x9-minmax-psimd-acc2.c",
    "src/f32-dwconv/gen/up8x9-minmax-psimd.c",
    "src/f32-gavgpool/7p7x-minmax-psimd-c4.c",
    "src/f32-gavgpool/7x-minmax-psimd-c4.c",
    "src/f32-gemm/gen/1x8-minmax-psimd-loadsplat.c",
    "src/f32-gemm/gen/1x8-minmax-psimd-splat.c",
    "src/f32-gemm/gen/1x8s4-minmax-psimd.c",
    "src/f32-gemm/gen/4x2c4-minmax-psimd.c",
    "src/f32-gemm/gen/4x8-minmax-psimd-loadsplat.c",
    "src/f32-gemm/gen/4x8-minmax-psimd-splat.c",
    "src/f32-gemm/gen/4x8s4-minmax-psimd.c",
    "src/f32-gemm/gen/6x8-minmax-psimd-loadsplat.c",
    "src/f32-gemm/gen/6x8-minmax-psimd-splat.c",
    "src/f32-gemm/gen/6x8s4-minmax-psimd.c",
    "src/f32-gemm/gen-inc/1x8inc-minmax-psimd-loadsplat.c",
    "src/f32-gemm/gen-inc/1x8inc-minmax-psimd-splat.c",
    "src/f32-gemm/gen-inc/1x8s4inc-minmax-psimd.c",
    "src/f32-gemm/gen-inc/4x8inc-minmax-psimd-loadsplat.c",
    "src/f32-gemm/gen-inc/4x8inc-minmax-psimd-splat.c",
    "src/f32-gemm/gen-inc/4x8s4inc-minmax-psimd.c",
    "src/f32-gemm/gen-inc/6x8inc-minmax-psimd-loadsplat.c",
    "src/f32-gemm/gen-inc/6x8inc-minmax-psimd-splat.c",
    "src/f32-gemm/gen-inc/6x8s4inc-minmax-psimd.c",
    "src/f32-hswish/gen/psimd-x4.c",
    "src/f32-hswish/gen/psimd-x8.c",
    "src/f32-ibilinear/gen/psimd-c4.c",
    "src/f32-ibilinear/gen/psimd-c8.c",
    "src/f32-igemm/gen/1x8-minmax-psimd-loadsplat.c",
    "src/f32-igemm/gen/1x8-minmax-psimd-splat.c",
    "src/f32-igemm/gen/1x8s4-minmax-psimd.c",
    "src/f32-igemm/gen/4x2c4-minmax-psimd.c",
    "src/f32-igemm/gen/4x8-minmax-psimd-loadsplat.c",
    "src/f32-igemm/gen/4x8-minmax-psimd-splat.c",
    "src/f32-igemm/gen/4x8s4-minmax-psimd.c",
    "src/f32-igemm/gen/6x8-minmax-psimd-loadsplat.c",
    "src/f32-igemm/gen/6x8-minmax-psimd-splat.c",
    "src/f32-igemm/gen/6x8s4-minmax-psimd.c",
    "src/f32-maxpool/9p8x-minmax-psimd-c4.c",
    "src/f32-pavgpool/9p8x-minmax-psimd-c4.c",
    "src/f32-pavgpool/9x-minmax-psimd-c4.c",
    "src/f32-ppmm/gen/4x8-minmax-psimd.c",
    "src/f32-prelu/gen/psimd-2x4.c",
    "src/f32-prelu/gen/psimd-2x8.c",
    "src/f32-rmax/psimd.c",
    "src/f32-vbinary/gen/vadd-minmax-psimd-x4.c",
    "src/f32-vbinary/gen/vadd-minmax-psimd-x8.c",
    "src/f32-vbinary/gen/vaddc-minmax-psimd-x4.c",
    "src/f32-vbinary/gen/vaddc-minmax-psimd-x8.c",
    "src/f32-vbinary/gen/vdiv-minmax-psimd-x4.c",
    "src/f32-vbinary/gen/vdiv-minmax-psimd-x8.c",
    "src/f32-vbinary/gen/vdivc-minmax-psimd-x4.c",
    "src/f32-vbinary/gen/vdivc-minmax-psimd-x8.c",
    "src/f32-vbinary/gen/vmax-psimd-x4.c",
    "src/f32-vbinary/gen/vmax-psimd-x8.c",
    "src/f32-vbinary/gen/vmaxc-psimd-x4.c",
    "src/f32-vbinary/gen/vmaxc-psimd-x8.c",
    "src/f32-vbinary/gen/vmin-psimd-x4.c",
    "src/f32-vbinary/gen/vmin-psimd-x8.c",
    "src/f32-vbinary/gen/vminc-psimd-x4.c",
    "src/f32-vbinary/gen/vminc-psimd-x8.c",
    "src/f32-vbinary/gen/vmul-minmax-psimd-x4.c",
    "src/f32-vbinary/gen/vmul-minmax-psimd-x8.c",
    "src/f32-vbinary/gen/vmulc-minmax-psimd-x4.c",
    "src/f32-vbinary/gen/vmulc-minmax-psimd-x8.c",
    "src/f32-vbinary/gen/vrdivc-minmax-psimd-x4.c",
    "src/f32-vbinary/gen/vrdivc-minmax-psimd-x8.c",
    "src/f32-vbinary/gen/vrsubc-minmax-psimd-x4.c",
    "src/f32-vbinary/gen/vrsubc-minmax-psimd-x8.c",
    "src/f32-vbinary/gen/vsub-minmax-psimd-x4.c",
    "src/f32-vbinary/gen/vsub-minmax-psimd-x8.c",
    "src/f32-vbinary/gen/vsubc-minmax-psimd-x4.c",
    "src/f32-vbinary/gen/vsubc-minmax-psimd-x8.c",
    "src/f32-vmulcaddc/gen/c4-minmax-psimd-2x.c",
    "src/f32-vmulcaddc/gen/c8-minmax-psimd-2x.c",
    "src/x32-packx/x4-psimd.c",
    "src/x32-pad/x2-psimd.c",
    "src/x32-unpool/psimd.c",
    "src/x32-zip/x2-psimd.c",
    "src/x32-zip/x3-psimd.c",
    "src/x32-zip/x4-psimd.c",
    "src/x32-zip/xm-psimd.c",
    "src/requantization/precise-psimd.c",
    "src/requantization/fp32-psimd.c",
]

PSIMD_ACCMATH_UKERNELS = [
    "src/f32-raddstoreexpminusmax/gen/psimd-p5-x4.c",
    "src/f32-raddstoreexpminusmax/gen/psimd-p5-x8.c",
    "src/f32-raddstoreexpminusmax/gen/psimd-p5-x8-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/psimd-p5-x12.c",
    "src/f32-raddstoreexpminusmax/gen/psimd-p5-x12-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/psimd-p5-x12-acc3.c",
    "src/f32-raddstoreexpminusmax/gen/psimd-p5-x16.c",
    "src/f32-raddstoreexpminusmax/gen/psimd-p5-x16-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/psimd-p5-x16-acc4.c",
    "src/f32-raddstoreexpminusmax/gen/psimd-p5-x20.c",
    "src/f32-raddstoreexpminusmax/gen/psimd-p5-x20-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/psimd-p5-x20-acc5.c",
    "src/f32-sigmoid/gen/psimd-p5-div-x4.c",
    "src/f32-sigmoid/gen/psimd-p5-div-x8.c",
    "src/f32-sigmoid/gen/psimd-p5-div-x12.c",
    "src/f32-sigmoid/gen/psimd-p5-div-x16.c",
    "src/f32-sigmoid/gen/psimd-p5-div-x20.c",
    "src/f32-sigmoid/gen/psimd-p5-div-x24.c",
    "src/math/sigmoid-psimd-p5-div.c",
]

# ISA-specific micro-kernels
NEON_UKERNELS = [
    "src/f32-avgpool/9p8x-minmax-neon-c4.c",
    "src/f32-avgpool/9x-minmax-neon-c4.c",
    "src/f32-clamp/gen/neon-x4.c",
    "src/f32-clamp/gen/neon-x8.c",
    "src/f32-dwconv/gen/up4x9-minmax-neon.c",
    "src/f32-dwconv/gen/up4x9-minmax-neon-acc2.c",
    "src/f32-dwconv/gen/up8x9-minmax-neon.c",
    "src/f32-dwconv/gen/up8x9-minmax-neon-acc2.c",
    "src/f32-gavgpool-spchw/neon-x4.c",
    "src/f32-gavgpool/7p7x-minmax-neon-c4.c",
    "src/f32-gavgpool/7x-minmax-neon-c4.c",
    "src/f32-gemm/gen/1x8-minmax-neon-lane-ld64.c",
    "src/f32-gemm/gen/4x2-minmax-neon-lane-ld64.c",
    "src/f32-gemm/gen/4x8-minmax-neon-lane-ld128.c",
    "src/f32-gemm/gen/4x8-minmax-neon-lane-ld64.c",
    "src/f32-gemm/gen/5x8-minmax-neon-lane-ld64.c",
    "src/f32-gemm/gen/6x8-minmax-neon-lane-ld64.c",
    "src/f32-gemm/gen/6x8-minmax-neon-lane-ld128.c",
    "src/f32-gemm/gen/1x8-minmax-neon-dup-ld64.c",
    "src/f32-gemm/gen/4x8-minmax-neon-dup-ld128.c",
    "src/f32-gemm/gen/4x8-minmax-neon-dup-ld64.c",
    "src/f32-gemm/gen/6x8-minmax-neon-dup-ld64.c",
    "src/f32-gemm/gen/6x8-minmax-neon-dup-ld128.c",
    "src/f32-gemm/gen/1x8s4-minmax-neon.c",
    "src/f32-gemm/gen/4x8s4-minmax-neon.c",
    "src/f32-gemm/gen/6x8s4-minmax-neon.c",
    "src/f32-gemm/gen/8x8s4-minmax-neon.c",
    "src/f32-gemm/gen-inc/1x8inc-minmax-neon-lane-ld64.c",
    "src/f32-gemm/gen-inc/4x8inc-minmax-neon-lane-ld128.c",
    "src/f32-gemm/gen-inc/4x8inc-minmax-neon-lane-ld64.c",
    "src/f32-gemm/gen-inc/5x8inc-minmax-neon-lane-ld64.c",
    "src/f32-gemm/gen-inc/6x8inc-minmax-neon-lane-ld64.c",
    "src/f32-gemm/gen-inc/6x8inc-minmax-neon-lane-ld128.c",
    "src/f32-gemm/gen-inc/1x8inc-minmax-neon-dup-ld64.c",
    "src/f32-gemm/gen-inc/4x8inc-minmax-neon-dup-ld128.c",
    "src/f32-gemm/gen-inc/4x8inc-minmax-neon-dup-ld64.c",
    "src/f32-gemm/gen-inc/6x8inc-minmax-neon-dup-ld64.c",
    "src/f32-gemm/gen-inc/6x8inc-minmax-neon-dup-ld128.c",
    "src/f32-gemm/gen-inc/1x8s4inc-minmax-neon.c",
    "src/f32-gemm/gen-inc/4x8s4inc-minmax-neon.c",
    "src/f32-gemm/gen-inc/6x8s4inc-minmax-neon.c",
    "src/f32-gemm/gen-inc/8x8s4inc-minmax-neon.c",
    "src/f32-hswish/gen/neon-x4.c",
    "src/f32-hswish/gen/neon-x8.c",
    "src/f32-ibilinear/gen/neon-c4.c",
    "src/f32-ibilinear/gen/neon-c8.c",
    "src/f32-igemm/gen/1x8-minmax-neon-lane-ld64.c",
    "src/f32-igemm/gen/4x2-minmax-neon-lane-ld64.c",
    "src/f32-igemm/gen/4x4-minmax-neon-lane-ld64.c",
    "src/f32-igemm/gen/4x8-minmax-neon-lane-ld128.c",
    "src/f32-igemm/gen/4x8-minmax-neon-lane-ld64.c",
    "src/f32-igemm/gen/6x8-minmax-neon-lane-ld64.c",
    "src/f32-igemm/gen/6x8-minmax-neon-lane-ld128.c",
    "src/f32-igemm/gen/1x8-minmax-neon-dup-ld64.c",
    "src/f32-igemm/gen/4x8-minmax-neon-dup-ld128.c",
    "src/f32-igemm/gen/4x8-minmax-neon-dup-ld64.c",
    "src/f32-igemm/gen/6x8-minmax-neon-dup-ld64.c",
    "src/f32-igemm/gen/6x8-minmax-neon-dup-ld128.c",
    "src/f32-igemm/gen/1x8s4-minmax-neon.c",
    "src/f32-igemm/gen/4x8s4-minmax-neon.c",
    "src/f32-igemm/gen/6x8s4-minmax-neon.c",
    "src/f32-igemm/gen/8x8s4-minmax-neon.c",
    "src/f32-maxpool/9p8x-minmax-neon-c4.c",
    "src/f32-pavgpool/9p8x-minmax-neon-c4.c",
    "src/f32-pavgpool/9x-minmax-neon-c4.c",
    "src/f32-ppmm/gen/4x8-minmax-neon.c",
    "src/f32-ppmm/gen/8x8-minmax-neon.c",
    "src/f32-prelu/gen/neon-2x4.c",
    "src/f32-prelu/gen/neon-2x8.c",
    "src/f32-raddstoreexpminusmax/gen/neon-p5-x4.c",
    "src/f32-raddstoreexpminusmax/gen/neon-p5-x8.c",
    "src/f32-raddstoreexpminusmax/gen/neon-p5-x8-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neon-p5-x12.c",
    "src/f32-raddstoreexpminusmax/gen/neon-p5-x12-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neon-p5-x12-acc3.c",
    "src/f32-raddstoreexpminusmax/gen/neon-p5-x16.c",
    "src/f32-raddstoreexpminusmax/gen/neon-p5-x16-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neon-p5-x16-acc4.c",
    "src/f32-raddstoreexpminusmax/gen/neon-p5-x20.c",
    "src/f32-raddstoreexpminusmax/gen/neon-p5-x20-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neon-p5-x20-acc5.c",
    "src/f32-raddstoreexpminusmax/gen/neon-lut64-p2-x4.c",
    "src/f32-raddstoreexpminusmax/gen/neon-lut64-p2-x8.c",
    "src/f32-raddstoreexpminusmax/gen/neon-lut64-p2-x8-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neon-lut64-p2-x12.c",
    "src/f32-raddstoreexpminusmax/gen/neon-lut64-p2-x12-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neon-lut64-p2-x12-acc3.c",
    "src/f32-raddstoreexpminusmax/gen/neon-lut64-p2-x16.c",
    "src/f32-raddstoreexpminusmax/gen/neon-lut64-p2-x16-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neon-lut64-p2-x16-acc4.c",
    "src/f32-raddstoreexpminusmax/gen/neon-lut64-p2-x20.c",
    "src/f32-raddstoreexpminusmax/gen/neon-lut64-p2-x20-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neon-lut64-p2-x20-acc5.c",
    "src/f32-rmax/neon.c",
    "src/f32-sigmoid/gen/neon-frac-p9-p10-nr1recps-x16.c",
    "src/f32-sigmoid/gen/neon-rr2-p5-nr2recps-x4.c",
    "src/f32-sigmoid/gen/neon-rr2-p5-nr2recps-x8.c",
    "src/f32-sigmoid/gen/neon-rr2-p5-nr2recps-x12.c",
    "src/f32-sigmoid/gen/neon-rr2-p5-nr2recps-x16.c",
    "src/f32-sigmoid/gen/neon-rr2-p5-nr2recps-x20.c",
    "src/f32-sigmoid/gen/neon-rr2-p5-nr2recps-x24.c",
    "src/f32-sigmoid/gen/neon-rr2-lut64-p2-nr2recps-x4.c",
    "src/f32-sigmoid/gen/neon-rr2-lut64-p2-nr2recps-x8.c",
    "src/f32-sigmoid/gen/neon-rr2-lut64-p2-nr2recps-x12.c",
    "src/f32-sigmoid/gen/neon-rr2-lut64-p2-nr2recps-x16.c",
    "src/f32-sigmoid/gen/neon-rr2-lut64-p2-nr2recps-x20.c",
    "src/f32-sigmoid/gen/neon-rr2-lut64-p2-nr2recps-x24.c",
    "src/f32-sigmoid/gen/neon-rr2-lut2048-p1-nr2recps-x4.c",
    "src/f32-sigmoid/gen/neon-rr2-lut2048-p1-nr2recps-x8.c",
    "src/f32-sigmoid/gen/neon-rr2-lut2048-p1-nr2recps-x12.c",
    "src/f32-sigmoid/gen/neon-rr2-lut2048-p1-nr2recps-x16.c",
    "src/f32-sigmoid/gen/neon-rr2-lut2048-p1-nr2recps-x20.c",
    "src/f32-sigmoid/gen/neon-rr2-lut2048-p1-nr2recps-x24.c",
    "src/f32-vbinary/gen/vadd-minmax-neon-x4.c",
    "src/f32-vbinary/gen/vadd-minmax-neon-x8.c",
    "src/f32-vbinary/gen/vaddc-minmax-neon-x4.c",
    "src/f32-vbinary/gen/vaddc-minmax-neon-x8.c",
    "src/f32-vbinary/gen/vmax-neon-x4.c",
    "src/f32-vbinary/gen/vmax-neon-x8.c",
    "src/f32-vbinary/gen/vmaxc-neon-x4.c",
    "src/f32-vbinary/gen/vmaxc-neon-x8.c",
    "src/f32-vbinary/gen/vmin-neon-x4.c",
    "src/f32-vbinary/gen/vmin-neon-x8.c",
    "src/f32-vbinary/gen/vminc-neon-x4.c",
    "src/f32-vbinary/gen/vminc-neon-x8.c",
    "src/f32-vbinary/gen/vmul-minmax-neon-x4.c",
    "src/f32-vbinary/gen/vmul-minmax-neon-x8.c",
    "src/f32-vbinary/gen/vmulc-minmax-neon-x4.c",
    "src/f32-vbinary/gen/vmulc-minmax-neon-x8.c",
    "src/f32-vbinary/gen/vrsubc-minmax-neon-x4.c",
    "src/f32-vbinary/gen/vrsubc-minmax-neon-x8.c",
    "src/f32-vbinary/gen/vsub-minmax-neon-x4.c",
    "src/f32-vbinary/gen/vsub-minmax-neon-x8.c",
    "src/f32-vbinary/gen/vsubc-minmax-neon-x4.c",
    "src/f32-vbinary/gen/vsubc-minmax-neon-x8.c",
    "src/f32-vmulcaddc/gen/c4-minmax-neon-2x.c",
    "src/f32-vmulcaddc/gen/c8-minmax-neon-2x.c",
    "src/q8-avgpool/9p8x-minmax-neon-c8.c",
    "src/q8-avgpool/9x-minmax-neon-c8.c",
    "src/q8-dwconv/up8x9-minmax-neon.c",
    "src/q8-gavgpool/7p7x-minmax-neon-c8.c",
    "src/q8-gavgpool/7x-minmax-neon-c8.c",
    "src/q8-gemm/4x8-minmax-neon.c",
    "src/q8-gemm/8x8-minmax-neon.c",
    "src/q8-igemm/4x8-minmax-neon.c",
    "src/q8-igemm/8x8-minmax-neon.c",
    "src/q8-vadd/minmax-neon.c",
    "src/u8-clamp/neon-x64.c",
    "src/u8-maxpool/9p8x-minmax-neon-c16.c",
    "src/u8-rmax/neon.c",
    "src/x32-packx/x4-neon-st4.c",
    "src/x32-pad/x2-neon.c",
    "src/x32-zip/x2-neon.c",
    "src/x32-zip/x3-neon.c",
    "src/x32-zip/x4-neon.c",
    "src/x32-zip/xm-neon.c",
    "src/x8-zip/x2-neon.c",
    "src/x8-zip/x3-neon.c",
    "src/x8-zip/x4-neon.c",
    "src/x8-zip/xm-neon.c",
    "src/math/sigmoid-neon-frac-p9-p10-nr1recps.c",
    "src/math/sigmoid-neon-rr1-lut2048-p1-nr2recps.c",
    "src/math/sigmoid-neon-rr1-lut64-p2-nr2recps.c",
    "src/math/sigmoid-neon-rr1-p5-nr2recps.c",
    "src/math/sigmoid-neon-rr2-lut2048-p1-nr2recps.c",
    "src/math/sigmoid-neon-rr2-lut64-p2-nr2recps.c",
    "src/math/sigmoid-neon-rr2-p5-nr2recps.c",
    "src/requantization/precise-neon.c",
    "src/requantization/fp32-neon.c",
    "src/requantization/q31-neon.c",
    "src/requantization/gemmlowp-neon.c",
]

NEONFMA_UKERNELS = [
    "src/f32-ibilinear/gen/neonfma-c4.c",
    "src/f32-ibilinear/gen/neonfma-c8.c",
    "src/f32-igemm/gen/1x8-minmax-neonfma-dup-ld64.c",
    "src/f32-igemm/gen/4x8-minmax-neonfma-dup-ld128.c",
    "src/f32-igemm/gen/4x8-minmax-neonfma-dup-ld64.c",
    "src/f32-igemm/gen/6x8-minmax-neonfma-dup-ld64.c",
    "src/f32-igemm/gen/6x8-minmax-neonfma-dup-ld128.c",
    "src/f32-igemm/gen/1x8s4-minmax-neonfma.c",
    "src/f32-igemm/gen/4x8s4-minmax-neonfma.c",
    "src/f32-igemm/gen/6x8s4-minmax-neonfma.c",
    "src/f32-igemm/gen/8x8s4-minmax-neonfma.c",
    "src/f32-dwconv/gen/up4x9-minmax-neonfma.c",
    "src/f32-dwconv/gen/up4x9-minmax-neonfma-acc2.c",
    "src/f32-dwconv/gen/up8x9-minmax-neonfma.c",
    "src/f32-dwconv/gen/up8x9-minmax-neonfma-acc2.c",
    "src/f32-gemm/gen/1x8-minmax-neonfma-dup-ld64.c",
    "src/f32-gemm/gen/4x8-minmax-neonfma-dup-ld128.c",
    "src/f32-gemm/gen/4x8-minmax-neonfma-dup-ld64.c",
    "src/f32-gemm/gen/6x8-minmax-neonfma-dup-ld64.c",
    "src/f32-gemm/gen/6x8-minmax-neonfma-dup-ld128.c",
    "src/f32-gemm/gen/1x8s4-minmax-neonfma.c",
    "src/f32-gemm/gen/4x8s4-minmax-neonfma.c",
    "src/f32-gemm/gen/6x8s4-minmax-neonfma.c",
    "src/f32-gemm/gen/8x8s4-minmax-neonfma.c",
    "src/f32-gemm/gen-inc/1x8inc-minmax-neonfma-dup-ld64.c",
    "src/f32-gemm/gen-inc/4x8inc-minmax-neonfma-dup-ld128.c",
    "src/f32-gemm/gen-inc/4x8inc-minmax-neonfma-dup-ld64.c",
    "src/f32-gemm/gen-inc/6x8inc-minmax-neonfma-dup-ld64.c",
    "src/f32-gemm/gen-inc/6x8inc-minmax-neonfma-dup-ld128.c",
    "src/f32-gemm/gen-inc/1x8s4inc-minmax-neonfma.c",
    "src/f32-gemm/gen-inc/4x8s4inc-minmax-neonfma.c",
    "src/f32-gemm/gen-inc/6x8s4inc-minmax-neonfma.c",
    "src/f32-gemm/gen-inc/8x8s4inc-minmax-neonfma.c",
    "src/f32-hswish/gen/neonfma-x4.c",
    "src/f32-hswish/gen/neonfma-x8.c",
    "src/f32-ppmm/gen/4x8-minmax-neonfma.c",
    "src/f32-ppmm/gen/8x8-minmax-neonfma.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-p5-x4.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-p5-x8.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-p5-x8-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-p5-x12.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-p5-x12-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-p5-x12-acc3.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-p5-x16.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-p5-x16-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-p5-x16-acc4.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-p5-x20.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-p5-x20-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-p5-x20-acc5.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-lut64-p2-x4.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-lut64-p2-x8.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-lut64-p2-x8-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-lut64-p2-x12.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-lut64-p2-x12-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-lut64-p2-x12-acc3.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-lut64-p2-x16.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-lut64-p2-x16-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-lut64-p2-x16-acc4.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-lut64-p2-x20.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-lut64-p2-x20-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/neonfma-lut64-p2-x20-acc5.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr2fma-x4.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr2fma-x8.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr2fma-x12.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr2fma-x16.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr2fma-x20.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr2fma-x24.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr1recps1fma-x4.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr1recps1fma-x8.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr1recps1fma-x12.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr1recps1fma-x16.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr1recps1fma-x20.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr1recps1fma-x24.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr2recps-x4.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr2recps-x8.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr2recps-x12.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr2recps-x16.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr2recps-x20.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-nr2recps-x24.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr2fma-x4.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr2fma-x8.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr2fma-x12.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr2fma-x16.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr2fma-x20.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr2fma-x24.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr1recps1fma-x4.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr1recps1fma-x8.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr1recps1fma-x12.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr1recps1fma-x16.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr1recps1fma-x20.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr1recps1fma-x24.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr2recps-x4.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr2recps-x8.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr2recps-x12.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr2recps-x16.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr2recps-x20.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-nr2recps-x24.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr2fma-x4.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr2fma-x8.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr2fma-x12.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr2fma-x16.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr2fma-x20.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr2fma-x24.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr1recps1fma-x4.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr1recps1fma-x8.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr1recps1fma-x12.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr1recps1fma-x16.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr1recps1fma-x20.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr1recps1fma-x24.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr2recps-x4.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr2recps-x8.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr2recps-x12.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr2recps-x16.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr2recps-x20.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-nr2recps-x24.c",
    "src/f32-vmulcaddc/gen/c4-minmax-neonfma-2x.c",
    "src/f32-vmulcaddc/gen/c8-minmax-neonfma-2x.c",
    "src/math/exp-neonfma-lut64-p2.c",
    "src/math/exp-neonfma-p5.c",
    "src/math/expminus-neonfma-lut2048-p1.c",
    "src/math/expminus-neonfma-lut64-p2.c",
    "src/math/expminus-neonfma-p5.c",
    "src/math/sigmoid-neonfma-rr1-lut2048-p1-nr1recps1fma.c",
    "src/math/sigmoid-neonfma-rr1-lut2048-p1-nr2fma.c",
    "src/math/sigmoid-neonfma-rr1-lut2048-p1-nr2recps.c",
    "src/math/sigmoid-neonfma-rr1-lut64-p2-nr1recps1fma.c",
    "src/math/sigmoid-neonfma-rr1-lut64-p2-nr2fma.c",
    "src/math/sigmoid-neonfma-rr1-lut64-p2-nr2recps.c",
    "src/math/sigmoid-neonfma-rr1-p5-nr1recps1fma.c",
    "src/math/sigmoid-neonfma-rr1-p5-nr2fma.c",
    "src/math/sigmoid-neonfma-rr1-p5-nr2recps.c",
    "src/math/sigmoid-neonfma-rr2-lut2048-p1-nr1recps1fma.c",
    "src/math/sigmoid-neonfma-rr2-lut2048-p1-nr2fma.c",
    "src/math/sigmoid-neonfma-rr2-lut2048-p1-nr2recps.c",
    "src/math/sigmoid-neonfma-rr2-lut64-p2-nr1recps1fma.c",
    "src/math/sigmoid-neonfma-rr2-lut64-p2-nr2fma.c",
    "src/math/sigmoid-neonfma-rr2-lut64-p2-nr2recps.c",
    "src/math/sigmoid-neonfma-rr2-p5-nr1recps1fma.c",
    "src/math/sigmoid-neonfma-rr2-p5-nr2fma.c",
    "src/math/sigmoid-neonfma-rr2-p5-nr2recps.c",
]

AARCH64_NEONFMA_UKERNELS = [
    "src/f32-vbinary/gen/vdiv-minmax-neon-x4.c",
    "src/f32-vbinary/gen/vdiv-minmax-neon-x8.c",
    "src/f32-vbinary/gen/vdivc-minmax-neon-x4.c",
    "src/f32-vbinary/gen/vdivc-minmax-neon-x8.c",
    "src/f32-vbinary/gen/vrdivc-minmax-neon-x4.c",
    "src/f32-vbinary/gen/vrdivc-minmax-neon-x8.c",
    "src/f32-gemm/gen/1x8-minmax-neonfma-lane-ld64.c",
    "src/f32-gemm/gen/4x2-minmax-neonfma-lane-ld64.c",
    "src/f32-gemm/gen/4x8-minmax-neonfma-lane-ld128.c",
    "src/f32-gemm/gen/4x8-minmax-neonfma-lane-ld64.c",
    "src/f32-gemm/gen/5x8-minmax-neonfma-lane-ld64.c",
    "src/f32-gemm/gen/6x8-minmax-neonfma-lane-ld64.c",
    "src/f32-gemm/gen/6x8-minmax-neonfma-lane-ld128.c",
    "src/f32-gemm/gen-inc/1x8inc-minmax-neonfma-lane-ld64.c",
    "src/f32-gemm/gen-inc/4x8inc-minmax-neonfma-lane-ld128.c",
    "src/f32-gemm/gen-inc/4x8inc-minmax-neonfma-lane-ld64.c",
    "src/f32-gemm/gen-inc/5x8inc-minmax-neonfma-lane-ld64.c",
    "src/f32-gemm/gen-inc/6x8inc-minmax-neonfma-lane-ld64.c",
    "src/f32-gemm/gen-inc/6x8inc-minmax-neonfma-lane-ld128.c",
    "src/f32-igemm/gen/1x8-minmax-neonfma-lane-ld64.c",
    "src/f32-igemm/gen/4x2-minmax-neonfma-lane-ld64.c",
    "src/f32-igemm/gen/4x4-minmax-neonfma-lane-ld64.c",
    "src/f32-igemm/gen/4x8-minmax-neonfma-lane-ld128.c",
    "src/f32-igemm/gen/4x8-minmax-neonfma-lane-ld64.c",
    "src/f32-igemm/gen/6x8-minmax-neonfma-lane-ld64.c",
    "src/f32-igemm/gen/6x8-minmax-neonfma-lane-ld128.c",
    "src/f32-conv-hwc/3x3s2p1c3x4-neonfma-2x2.c",
    "src/f32-conv-hwc/3x3s2p1c3x8-neonfma-2x2.c",
    "src/f32-conv-hwc2spchw/3x3s2p1c3x4-neonfma-2x2.c",
    "src/f32-dwconv-spchw/3x3p1-neonfma.c",
    "src/f32-dwconv-spchw/5x5p2-neonfma.c",
    "src/f32-dwconv-spchw/3x3s2p1-neonfma.c",
    "src/f32-dwconv-spchw/5x5s2p2-neonfma.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-div-x4.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-div-x8.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-div-x12.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-div-x16.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-div-x20.c",
    "src/f32-sigmoid/gen/neonfma-rr1-p5-div-x24.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-div-x4.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-div-x8.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-div-x12.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-div-x16.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-div-x20.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut64-p2-div-x24.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-div-x4.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-div-x8.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-div-x12.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-div-x16.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-div-x20.c",
    "src/f32-sigmoid/gen/neonfma-rr1-lut2048-p1-div-x24.c",
    "src/f32-spmm/gen/12x1-minmax-neonfma.c",
    "src/f32-spmm/gen/12x2-minmax-neonfma.c",
    "src/f32-spmm/gen/12x4-minmax-neonfma.c",
    "src/f32-spmm/gen/16x1-minmax-neonfma-pipelined.c",
    "src/f32-spmm/gen/16x1-minmax-neonfma-unroll2.c",
    "src/f32-spmm/gen/16x1-minmax-neonfma.c",
    "src/f32-spmm/gen/16x2-minmax-neonfma.c",
    "src/f32-spmm/gen/16x4-minmax-neonfma.c",
    "src/f32-spmm/gen/4x1-minmax-neonfma-pipelined.c",
    "src/f32-spmm/gen/4x1-minmax-neonfma-unroll2.c",
    "src/f32-spmm/gen/4x1-minmax-neonfma.c",
    "src/f32-spmm/gen/4x2-minmax-neonfma.c",
    "src/f32-spmm/gen/4x4-minmax-neonfma.c",
    "src/f32-spmm/gen/8x1-minmax-neonfma-pipelined.c",
    "src/f32-spmm/gen/8x1-minmax-neonfma-unroll2.c",
    "src/f32-spmm/gen/8x1-minmax-neonfma.c",
    "src/f32-spmm/gen/8x2-minmax-neonfma.c",
    "src/f32-spmm/gen/8x4-minmax-neonfma.c",
    "src/math/sigmoid-neonfma-rr1-lut2048-p1-div.c",
    "src/math/sigmoid-neonfma-rr1-lut64-p2-div.c",
    "src/math/sigmoid-neonfma-rr1-p5-div.c",
    "src/math/sigmoid-neonfma-rr2-lut2048-p1-div.c",
    "src/math/sigmoid-neonfma-rr2-lut64-p2-div.c",
    "src/math/sigmoid-neonfma-rr2-p5-div.c",
]

AARCH64_NEONFP16ARITH_UKERNELS = [
    "src/f16-gemm/gen/4x8-neonfp16arith-ld64.c",
    "src/f16-gemm/gen/6x8-neonfp16arith-ld64.c",
    "src/f16-gemm/gen/8x8-neonfp16arith-ld64.c",
    "src/f16-spmm/gen/8x1-minmax-neonfp16arith.c",
    "src/f16-spmm/gen/8x1-minmax-neonfp16arith-unroll2.c",
    "src/f16-spmm/gen/16x1-minmax-neonfp16arith.c",
    "src/f16-spmm/gen/16x1-minmax-neonfp16arith-unroll2.c",
    "src/f16-spmm/gen/24x1-minmax-neonfp16arith.c",
    "src/f16-spmm/gen/24x1-minmax-neonfp16arith-unroll2.c",
    "src/f16-spmm/gen/32x1-minmax-neonfp16arith.c",
    "src/f16-spmm/gen/32x1-minmax-neonfp16arith-unroll2.c",
]

SSE_UKERNELS = [
    "src/f32-avgpool/9p8x-minmax-sse-c4.c",
    "src/f32-avgpool/9x-minmax-sse-c4.c",
    "src/f32-clamp/gen/sse-x4.c",
    "src/f32-clamp/gen/sse-x8.c",
    "src/f32-dwconv-spchw/3x3p1-sse.c",
    "src/f32-dwconv-spchw/3x3s2p1-sse.c",
    "src/f32-dwconv/gen/up4x25-minmax-sse-acc2.c",
    "src/f32-dwconv/gen/up4x25-minmax-sse.c",
    "src/f32-dwconv/gen/up4x4-minmax-sse-acc2.c",
    "src/f32-dwconv/gen/up4x4-minmax-sse.c",
    "src/f32-dwconv/gen/up4x9-minmax-sse-acc2.c",
    "src/f32-dwconv/gen/up4x9-minmax-sse.c",
    "src/f32-dwconv/gen/up8x25-minmax-sse-acc2.c",
    "src/f32-dwconv/gen/up8x25-minmax-sse.c",
    "src/f32-dwconv/gen/up8x4-minmax-sse-acc2.c",
    "src/f32-dwconv/gen/up8x4-minmax-sse.c",
    "src/f32-dwconv/gen/up8x9-minmax-sse-acc2.c",
    "src/f32-dwconv/gen/up8x9-minmax-sse.c",
    "src/f32-gavgpool-spchw/sse-x4.c",
    "src/f32-gavgpool/7p7x-minmax-sse-c4.c",
    "src/f32-gavgpool/7x-minmax-sse-c4.c",
    "src/f32-gemm/gen/1x8-minmax-sse-dup.c",
    "src/f32-gemm/gen/1x8-minmax-sse-load1.c",
    "src/f32-gemm/gen/1x8s4-minmax-sse.c",
    "src/f32-gemm/gen/4x2c4-minmax-sse.c",
    "src/f32-gemm/gen/4x8-minmax-sse-dup.c",
    "src/f32-gemm/gen/4x8-minmax-sse-load1.c",
    "src/f32-gemm/gen/4x8s4-minmax-sse.c",
    "src/f32-gemm/gen-inc/1x8inc-minmax-sse-dup.c",
    "src/f32-gemm/gen-inc/1x8inc-minmax-sse-load1.c",
    "src/f32-gemm/gen-inc/1x8s4inc-minmax-sse.c",
    "src/f32-gemm/gen-inc/4x8inc-minmax-sse-dup.c",
    "src/f32-gemm/gen-inc/4x8inc-minmax-sse-load1.c",
    "src/f32-gemm/gen-inc/4x8s4inc-minmax-sse.c",
    "src/f32-hswish/gen/sse-x4.c",
    "src/f32-hswish/gen/sse-x8.c",
    "src/f32-ibilinear/gen/sse-c4.c",
    "src/f32-ibilinear/gen/sse-c8.c",
    "src/f32-igemm/gen/1x8-minmax-sse-dup.c",
    "src/f32-igemm/gen/1x8-minmax-sse-load1.c",
    "src/f32-igemm/gen/1x8s4-minmax-sse.c",
    "src/f32-igemm/gen/4x2c4-minmax-sse.c",
    "src/f32-igemm/gen/4x8-minmax-sse-dup.c",
    "src/f32-igemm/gen/4x8-minmax-sse-load1.c",
    "src/f32-igemm/gen/4x8s4-minmax-sse.c",
    "src/f32-maxpool/9p8x-minmax-sse-c4.c",
    "src/f32-pavgpool/9p8x-minmax-sse-c4.c",
    "src/f32-pavgpool/9x-minmax-sse-c4.c",
    "src/f32-ppmm/gen/4x8-minmax-sse.c",
    "src/f32-rmax/sse.c",
    "src/f32-spmm/gen/4x1-minmax-sse.c",
    "src/f32-spmm/gen/8x1-minmax-sse.c",
    "src/f32-vbinary/gen/vadd-minmax-sse-x4.c",
    "src/f32-vbinary/gen/vadd-minmax-sse-x8.c",
    "src/f32-vbinary/gen/vaddc-minmax-sse-x4.c",
    "src/f32-vbinary/gen/vaddc-minmax-sse-x8.c",
    "src/f32-vbinary/gen/vdiv-minmax-sse-x4.c",
    "src/f32-vbinary/gen/vdiv-minmax-sse-x8.c",
    "src/f32-vbinary/gen/vdivc-minmax-sse-x4.c",
    "src/f32-vbinary/gen/vdivc-minmax-sse-x8.c",
    "src/f32-vbinary/gen/vmax-sse-x4.c",
    "src/f32-vbinary/gen/vmax-sse-x8.c",
    "src/f32-vbinary/gen/vmaxc-sse-x4.c",
    "src/f32-vbinary/gen/vmaxc-sse-x8.c",
    "src/f32-vbinary/gen/vmin-sse-x4.c",
    "src/f32-vbinary/gen/vmin-sse-x8.c",
    "src/f32-vbinary/gen/vminc-sse-x4.c",
    "src/f32-vbinary/gen/vminc-sse-x8.c",
    "src/f32-vbinary/gen/vmul-minmax-sse-x4.c",
    "src/f32-vbinary/gen/vmul-minmax-sse-x8.c",
    "src/f32-vbinary/gen/vmulc-minmax-sse-x4.c",
    "src/f32-vbinary/gen/vmulc-minmax-sse-x8.c",
    "src/f32-vbinary/gen/vrdivc-minmax-sse-x4.c",
    "src/f32-vbinary/gen/vrdivc-minmax-sse-x8.c",
    "src/f32-vbinary/gen/vrsubc-minmax-sse-x4.c",
    "src/f32-vbinary/gen/vrsubc-minmax-sse-x8.c",
    "src/f32-vbinary/gen/vsub-minmax-sse-x4.c",
    "src/f32-vbinary/gen/vsub-minmax-sse-x8.c",
    "src/f32-vbinary/gen/vsubc-minmax-sse-x4.c",
    "src/f32-vbinary/gen/vsubc-minmax-sse-x8.c",
    "src/f32-vmulcaddc/gen/c4-minmax-sse-2x.c",
    "src/f32-vmulcaddc/gen/c8-minmax-sse-2x.c",
    "src/x32-packx/x4-sse.c",
]

SSE2_UKERNELS = [
    "src/f32-argmaxpool/9p8x-sse2-c4.c",
    "src/f32-argmaxpool/4x-sse2-c4.c",
    "src/f32-argmaxpool/9x-sse2-c4.c",
    "src/f32-prelu/gen/sse2-2x4.c",
    "src/f32-prelu/gen/sse2-2x8.c",
    "src/f32-raddstoreexpminusmax/gen/sse2-p5-x4.c",
    "src/f32-raddstoreexpminusmax/gen/sse2-p5-x8.c",
    "src/f32-raddstoreexpminusmax/gen/sse2-p5-x8-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/sse2-p5-x12.c",
    "src/f32-raddstoreexpminusmax/gen/sse2-p5-x12-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/sse2-p5-x12-acc3.c",
    "src/f32-raddstoreexpminusmax/gen/sse2-p5-x16.c",
    "src/f32-raddstoreexpminusmax/gen/sse2-p5-x16-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/sse2-p5-x16-acc4.c",
    "src/f32-raddstoreexpminusmax/gen/sse2-p5-x20.c",
    "src/f32-raddstoreexpminusmax/gen/sse2-p5-x20-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/sse2-p5-x20-acc5.c",
    "src/f32-sigmoid/gen/sse2-p5-div-x4.c",
    "src/f32-sigmoid/gen/sse2-p5-div-x8.c",
    "src/f32-sigmoid/gen/sse2-p5-div-x12.c",
    "src/f32-sigmoid/gen/sse2-p5-div-x16.c",
    "src/f32-sigmoid/gen/sse2-p5-div-x20.c",
    "src/f32-sigmoid/gen/sse2-p5-div-x24.c",
    "src/q8-avgpool/9p8x-minmax-sse2-c8.c",
    "src/q8-avgpool/9x-minmax-sse2-c8.c",
    "src/q8-igemm/4x4c2-minmax-sse2.c",
    "src/q8-dwconv/up8x9-minmax-sse2.c",
    "src/q8-gavgpool/7p7x-minmax-sse2-c8.c",
    "src/q8-gavgpool/7x-minmax-sse2-c8.c",
    "src/q8-gemm/2x4c8-minmax-sse2.c",
    "src/q8-gemm/4x4c2-minmax-sse2.c",
    "src/q8-vadd/minmax-sse2.c",
    "src/u8-clamp/sse2-x64.c",
    "src/u8-maxpool/9p8x-minmax-sse2-c16.c",
    "src/u8-rmax/sse2.c",
    "src/x32-pad/x2-sse2.c",
    "src/x32-zip/x2-sse2.c",
    "src/x32-zip/x3-sse2.c",
    "src/x32-zip/x4-sse2.c",
    "src/x32-zip/xm-sse2.c",
    "src/x8-zip/x2-sse2.c",
    "src/x8-zip/x3-sse2.c",
    "src/x8-zip/x4-sse2.c",
    "src/x8-zip/xm-sse2.c",
    "src/math/exp-sse2-p5.c",
    "src/math/expminus-sse2-p5.c",
    "src/math/sigmoid-sse2-p5-div.c",
    "src/requantization/precise-sse2.c",
    "src/requantization/fp32-sse2.c",
    "src/requantization/q31-sse2.c",
    "src/requantization/gemmlowp-sse2.c",
]

SSSE3_UKERNELS = [
    "src/requantization/precise-ssse3.c",
    "src/requantization/q31-ssse3.c",
    "src/requantization/gemmlowp-ssse3.c",
]

SSE41_UKERNELS = [
    "src/f32-prelu/gen/sse41-2x4.c",
    "src/f32-prelu/gen/sse41-2x8.c",
    "src/f32-sigmoid/gen/sse41-p5-div-x4.c",
    "src/f32-sigmoid/gen/sse41-p5-div-x8.c",
    "src/f32-sigmoid/gen/sse41-p5-div-x12.c",
    "src/f32-sigmoid/gen/sse41-p5-div-x16.c",
    "src/f32-sigmoid/gen/sse41-p5-div-x20.c",
    "src/f32-sigmoid/gen/sse41-p5-div-x24.c",
    "src/requantization/precise-sse4.c",
    "src/requantization/q31-sse4.c",
    "src/requantization/gemmlowp-sse4.c",
]

AVX_UKERNELS = [
    "src/f32-clamp/gen/avx-x8.c",
    "src/f32-clamp/gen/avx-x16.c",
    "src/f32-dwconv/gen/up16x4-minmax-avx-acc2.c",
    "src/f32-dwconv/gen/up16x4-minmax-avx.c",
    "src/f32-dwconv/gen/up8x4-minmax-avx-acc2.c",
    "src/f32-dwconv/gen/up8x4-minmax-avx.c",
    "src/f32-dwconv/gen/up16x9-minmax-avx-acc2.c",
    "src/f32-dwconv/gen/up16x9-minmax-avx.c",
    "src/f32-dwconv/gen/up8x9-minmax-avx-acc2.c",
    "src/f32-dwconv/gen/up8x9-minmax-avx.c",
    "src/f32-dwconv/gen/up16x25-minmax-avx-acc2.c",
    "src/f32-dwconv/gen/up16x25-minmax-avx.c",
    "src/f32-dwconv/gen/up8x25-minmax-avx-acc2.c",
    "src/f32-dwconv/gen/up8x25-minmax-avx.c",
    "src/f32-gemm/gen/1x8-minmax-avx-broadcast.c",
    "src/f32-gemm/gen/4x8-minmax-avx-broadcast.c",
    "src/f32-gemm/gen/5x8-minmax-avx-broadcast.c",
    "src/f32-gemm/gen/6x8-minmax-avx-broadcast.c",
    "src/f32-gemm/gen/7x8-minmax-avx-broadcast.c",
    "src/f32-gemm/gen/1x16-minmax-avx-broadcast.c",
    "src/f32-gemm/gen/3x16-minmax-avx-broadcast.c",
    "src/f32-gemm/gen/4x16-minmax-avx-broadcast.c",
    "src/f32-gemm/gen/5x16-minmax-avx-broadcast.c",
    "src/f32-gemm/gen-inc/1x8inc-minmax-avx-broadcast.c",
    "src/f32-gemm/gen-inc/4x8inc-minmax-avx-broadcast.c",
    "src/f32-gemm/gen-inc/5x8inc-minmax-avx-broadcast.c",
    "src/f32-gemm/gen-inc/6x8inc-minmax-avx-broadcast.c",
    "src/f32-gemm/gen-inc/7x8inc-minmax-avx-broadcast.c",
    "src/f32-gemm/gen-inc/1x16inc-minmax-avx-broadcast.c",
    "src/f32-gemm/gen-inc/3x16inc-minmax-avx-broadcast.c",
    "src/f32-gemm/gen-inc/4x16inc-minmax-avx-broadcast.c",
    "src/f32-gemm/gen-inc/5x16inc-minmax-avx-broadcast.c",
    "src/f32-hswish/gen/avx-x8.c",
    "src/f32-hswish/gen/avx-x16.c",
    "src/f32-igemm/gen/1x8-minmax-avx-broadcast.c",
    "src/f32-igemm/gen/4x8-minmax-avx-broadcast.c",
    "src/f32-igemm/gen/5x8-minmax-avx-broadcast.c",
    "src/f32-igemm/gen/6x8-minmax-avx-broadcast.c",
    "src/f32-igemm/gen/7x8-minmax-avx-broadcast.c",
    "src/f32-igemm/gen/1x16-minmax-avx-broadcast.c",
    "src/f32-igemm/gen/3x16-minmax-avx-broadcast.c",
    "src/f32-igemm/gen/4x16-minmax-avx-broadcast.c",
    "src/f32-igemm/gen/5x16-minmax-avx-broadcast.c",
    "src/f32-prelu/gen/avx-2x8.c",
    "src/f32-prelu/gen/avx-2x16.c",
    "src/f32-rmax/avx.c",
    "src/f32-vbinary/gen/vadd-minmax-avx-x8.c",
    "src/f32-vbinary/gen/vadd-minmax-avx-x16.c",
    "src/f32-vbinary/gen/vaddc-minmax-avx-x8.c",
    "src/f32-vbinary/gen/vaddc-minmax-avx-x16.c",
    "src/f32-vbinary/gen/vdiv-minmax-avx-x8.c",
    "src/f32-vbinary/gen/vdiv-minmax-avx-x16.c",
    "src/f32-vbinary/gen/vdivc-minmax-avx-x8.c",
    "src/f32-vbinary/gen/vdivc-minmax-avx-x16.c",
    "src/f32-vbinary/gen/vmax-avx-x8.c",
    "src/f32-vbinary/gen/vmax-avx-x16.c",
    "src/f32-vbinary/gen/vmaxc-avx-x8.c",
    "src/f32-vbinary/gen/vmaxc-avx-x16.c",
    "src/f32-vbinary/gen/vmin-avx-x8.c",
    "src/f32-vbinary/gen/vmin-avx-x16.c",
    "src/f32-vbinary/gen/vminc-avx-x8.c",
    "src/f32-vbinary/gen/vminc-avx-x16.c",
    "src/f32-vbinary/gen/vmul-minmax-avx-x8.c",
    "src/f32-vbinary/gen/vmul-minmax-avx-x16.c",
    "src/f32-vbinary/gen/vmulc-minmax-avx-x8.c",
    "src/f32-vbinary/gen/vmulc-minmax-avx-x16.c",
    "src/f32-vbinary/gen/vrdivc-minmax-avx-x8.c",
    "src/f32-vbinary/gen/vrdivc-minmax-avx-x16.c",
    "src/f32-vbinary/gen/vrsubc-minmax-avx-x8.c",
    "src/f32-vbinary/gen/vrsubc-minmax-avx-x16.c",
    "src/f32-vbinary/gen/vsub-minmax-avx-x8.c",
    "src/f32-vbinary/gen/vsub-minmax-avx-x16.c",
    "src/f32-vbinary/gen/vsubc-minmax-avx-x8.c",
    "src/f32-vbinary/gen/vsubc-minmax-avx-x16.c",
    "src/f32-vscale/avx-unroll32.c",
]

FMA3_UKERNELS = [
    "src/f32-dwconv/gen/up16x4-minmax-fma3-acc2.c",
    "src/f32-dwconv/gen/up16x4-minmax-fma3.c",
    "src/f32-dwconv/gen/up8x4-minmax-fma3-acc2.c",
    "src/f32-dwconv/gen/up8x4-minmax-fma3.c",
    "src/f32-dwconv/gen/up16x9-minmax-fma3-acc2.c",
    "src/f32-dwconv/gen/up16x9-minmax-fma3.c",
    "src/f32-dwconv/gen/up8x9-minmax-fma3-acc2.c",
    "src/f32-dwconv/gen/up8x9-minmax-fma3.c",
    "src/f32-dwconv/gen/up16x25-minmax-fma3-acc2.c",
    "src/f32-dwconv/gen/up16x25-minmax-fma3.c",
    "src/f32-dwconv/gen/up8x25-minmax-fma3-acc2.c",
    "src/f32-dwconv/gen/up8x25-minmax-fma3.c",
    "src/f32-gemm/gen/1x8-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen/4x8-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen/5x8-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen/6x8-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen/7x8-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen/8x8-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen/1x16-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen/3x16-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen/4x16-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen/5x16-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen/1x16s4-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen/3x16s4-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen/4x16s4-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen/5x16s4-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen-inc/1x8inc-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen-inc/4x8inc-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen-inc/5x8inc-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen-inc/6x8inc-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen-inc/7x8inc-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen-inc/8x8inc-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen-inc/1x16inc-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen-inc/3x16inc-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen-inc/4x16inc-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen-inc/5x16inc-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen-inc/1x16s4inc-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen-inc/3x16s4inc-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen-inc/4x16s4inc-minmax-fma3-broadcast.c",
    "src/f32-gemm/gen-inc/5x16s4inc-minmax-fma3-broadcast.c",
    "src/f32-hswish/gen/fma3-x8.c",
    "src/f32-hswish/gen/fma3-x16.c",
    "src/f32-igemm/gen/1x8-minmax-fma3-broadcast.c",
    "src/f32-igemm/gen/4x8-minmax-fma3-broadcast.c",
    "src/f32-igemm/gen/5x8-minmax-fma3-broadcast.c",
    "src/f32-igemm/gen/6x8-minmax-fma3-broadcast.c",
    "src/f32-igemm/gen/7x8-minmax-fma3-broadcast.c",
    "src/f32-igemm/gen/8x8-minmax-fma3-broadcast.c",
    "src/f32-igemm/gen/1x16-minmax-fma3-broadcast.c",
    "src/f32-igemm/gen/3x16-minmax-fma3-broadcast.c",
    "src/f32-igemm/gen/4x16-minmax-fma3-broadcast.c",
    "src/f32-igemm/gen/5x16-minmax-fma3-broadcast.c",
    "src/f32-igemm/gen/1x16s4-minmax-fma3-broadcast.c",
    "src/f32-igemm/gen/3x16s4-minmax-fma3-broadcast.c",
    "src/f32-igemm/gen/4x16s4-minmax-fma3-broadcast.c",
    "src/f32-igemm/gen/5x16s4-minmax-fma3-broadcast.c",
]

AVX2_UKERNELS = [
    "src/f32-raddexpminusmax/gen/avx2-p5-x64.c",
    "src/f32-raddexpminusmax/gen/avx2-p5-x64-acc2.c",
    "src/f32-raddexpminusmax/gen/avx2-p5-x64-acc4.c",
    "src/f32-raddexpminusmax/gen/avx2-p5-x72.c",
    "src/f32-raddexpminusmax/gen/avx2-p5-x72-acc3.c",
    "src/f32-raddexpminusmax/gen/avx2-p5-x80.c",
    "src/f32-raddexpminusmax/gen/avx2-p5-x80-acc2.c",
    "src/f32-raddexpminusmax/gen/avx2-p5-x80-acc5.c",
    "src/f32-raddexpminusmax/gen/avx2-p5-x96.c",
    "src/f32-raddexpminusmax/gen/avx2-p5-x96-acc2.c",
    "src/f32-raddexpminusmax/gen/avx2-p5-x96-acc3.c",
    "src/f32-raddexpminusmax/gen/avx2-p5-x96-acc6.c",
    "src/f32-raddextexp/gen/avx2-p5-x64.c",
    "src/f32-raddextexp/gen/avx2-p5-x64-acc2.c",
    "src/f32-raddextexp/gen/avx2-p5-x64-acc4.c",
    "src/f32-raddextexp/gen/avx2-p5-x72.c",
    "src/f32-raddextexp/gen/avx2-p5-x72-acc3.c",
    "src/f32-raddextexp/gen/avx2-p5-x80.c",
    "src/f32-raddextexp/gen/avx2-p5-x80-acc2.c",
    "src/f32-raddextexp/gen/avx2-p5-x80-acc5.c",
    "src/f32-raddextexp/gen/avx2-p5-x96.c",
    "src/f32-raddextexp/gen/avx2-p5-x96-acc2.c",
    "src/f32-raddextexp/gen/avx2-p5-x96-acc3.c",
    "src/f32-raddextexp/gen/avx2-p5-x96-acc6.c",
    "src/f32-raddstoreexpminusmax/gen/avx2-p5-x64.c",
    "src/f32-raddstoreexpminusmax/gen/avx2-p5-x64-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/avx2-p5-x64-acc4.c",
    "src/f32-raddstoreexpminusmax/gen/avx2-p5-x72.c",
    "src/f32-raddstoreexpminusmax/gen/avx2-p5-x72-acc3.c",
    "src/f32-raddstoreexpminusmax/gen/avx2-p5-x80.c",
    "src/f32-raddstoreexpminusmax/gen/avx2-p5-x80-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/avx2-p5-x80-acc5.c",
    "src/f32-raddstoreexpminusmax/gen/avx2-p5-x96.c",
    "src/f32-raddstoreexpminusmax/gen/avx2-p5-x96-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/avx2-p5-x96-acc3.c",
    "src/f32-raddstoreexpminusmax/gen/avx2-p5-x96-acc6.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-div-x8.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-div-x16.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-div-x24.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-div-x32.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-div-x40.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-div-x48.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-div-x56.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-div-x64.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-div-x72.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-div-x80.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr1fma-x8.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr1fma-x16.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr1fma-x24.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr1fma-x32.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr1fma-x40.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr1fma-x48.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr1fma-x56.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr1fma-x64.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr1fma-x72.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr1fma-x80.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr2fma-x8.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr2fma-x16.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr2fma-x24.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr2fma-x32.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr2fma-x40.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr2fma-x48.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr2fma-x56.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr2fma-x64.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr2fma-x72.c",
    "src/f32-sigmoid/gen/avx2-rr1-p5-nr2fma-x80.c",
    "src/f32-vscaleexpminusmax/gen/avx2-p5-x8.c",
    "src/f32-vscaleexpminusmax/gen/avx2-p5-x16.c",
    "src/f32-vscaleexpminusmax/gen/avx2-p5-x24.c",
    "src/f32-vscaleexpminusmax/gen/avx2-p5-x32.c",
    "src/f32-vscaleexpminusmax/gen/avx2-p5-x40.c",
    "src/f32-vscaleexpminusmax/gen/avx2-p5-x48.c",
    "src/f32-vscaleexpminusmax/gen/avx2-p5-x56.c",
    "src/f32-vscaleexpminusmax/gen/avx2-p5-x64.c",
    "src/f32-vscaleexpminusmax/gen/avx2-p5-x72.c",
    "src/f32-vscaleexpminusmax/gen/avx2-p5-x80.c",
    "src/f32-vscaleexpminusmax/gen/avx2-p5-x88.c",
    "src/f32-vscaleexpminusmax/gen/avx2-p5-x96.c",
    "src/f32-vscaleextexp/gen/avx2-p5-x8.c",
    "src/f32-vscaleextexp/gen/avx2-p5-x16.c",
    "src/f32-vscaleextexp/gen/avx2-p5-x24.c",
    "src/f32-vscaleextexp/gen/avx2-p5-x32.c",
    "src/f32-vscaleextexp/gen/avx2-p5-x40.c",
    "src/f32-vscaleextexp/gen/avx2-p5-x48.c",
    "src/f32-vscaleextexp/gen/avx2-p5-x56.c",
    "src/f32-vscaleextexp/gen/avx2-p5-x64.c",
    "src/f32-vscaleextexp/gen/avx2-p5-x72.c",
    "src/f32-vscaleextexp/gen/avx2-p5-x80.c",
    "src/f32-vscaleextexp/gen/avx2-p5-x88.c",
    "src/f32-vscaleextexp/gen/avx2-p5-x96.c",
    "src/math/exp-avx2-p5.c",
    "src/math/exp-avx2-perm-p3.c",
    "src/math/exp-avx2-perm-p4.c",
    "src/math/expminus-avx2-p5.c",
    "src/math/extexp-avx2-p5.c",
    "src/math/sigmoid-avx2-rr2-p5-div.c",
    "src/math/sigmoid-avx2-rr1-p5-div.c",
    "src/math/sigmoid-avx2-rr2-p5-nr2fma.c",
    "src/math/sigmoid-avx2-rr1-p5-nr2fma.c",
    "src/math/sigmoid-avx2-rr2-p5-nr1fma.c",
    "src/math/sigmoid-avx2-rr1-p5-nr1fma.c",
]

AVX512F_UKERNELS = [
    "src/f32-clamp/gen/avx512f-x16.c",
    "src/f32-clamp/gen/avx512f-x32.c",
    "src/f32-dwconv/gen/up32x4-minmax-avx512f-acc2.c",
    "src/f32-dwconv/gen/up32x4-minmax-avx512f.c",
    "src/f32-dwconv/gen/up16x4-minmax-avx512f-acc2.c",
    "src/f32-dwconv/gen/up16x4-minmax-avx512f.c",
    "src/f32-dwconv/gen/up32x9-minmax-avx512f-acc2.c",
    "src/f32-dwconv/gen/up32x9-minmax-avx512f.c",
    "src/f32-dwconv/gen/up16x9-minmax-avx512f-acc2.c",
    "src/f32-dwconv/gen/up16x9-minmax-avx512f.c",
    "src/f32-dwconv/gen/up32x25-minmax-avx512f-acc2.c",
    "src/f32-dwconv/gen/up32x25-minmax-avx512f.c",
    "src/f32-dwconv/gen/up16x25-minmax-avx512f-acc2.c",
    "src/f32-dwconv/gen/up16x25-minmax-avx512f.c",
    "src/f32-gemm/gen/1x16-minmax-avx512f-broadcast.c",
    "src/f32-gemm/gen/4x16-minmax-avx512f-broadcast.c",
    "src/f32-gemm/gen/5x16-minmax-avx512f-broadcast.c",
    "src/f32-gemm/gen/6x16-minmax-avx512f-broadcast.c",
    "src/f32-gemm/gen/7x16-minmax-avx512f-broadcast.c",
    "src/f32-gemm/gen/8x16-minmax-avx512f-broadcast.c",
    "src/f32-gemm/gen-inc/1x16inc-minmax-avx512f-broadcast.c",
    "src/f32-gemm/gen-inc/4x16inc-minmax-avx512f-broadcast.c",
    "src/f32-gemm/gen-inc/5x16inc-minmax-avx512f-broadcast.c",
    "src/f32-gemm/gen-inc/6x16inc-minmax-avx512f-broadcast.c",
    "src/f32-gemm/gen-inc/7x16inc-minmax-avx512f-broadcast.c",
    "src/f32-gemm/gen-inc/8x16inc-minmax-avx512f-broadcast.c",
    "src/f32-hswish/gen/avx512f-x16.c",
    "src/f32-hswish/gen/avx512f-x32.c",
    "src/f32-igemm/gen/1x16-minmax-avx512f-broadcast.c",
    "src/f32-igemm/gen/4x16-minmax-avx512f-broadcast.c",
    "src/f32-igemm/gen/5x16-minmax-avx512f-broadcast.c",
    "src/f32-igemm/gen/6x16-minmax-avx512f-broadcast.c",
    "src/f32-igemm/gen/7x16-minmax-avx512f-broadcast.c",
    "src/f32-igemm/gen/8x16-minmax-avx512f-broadcast.c",
    "src/f32-prelu/gen/avx512f-2x16.c",
    "src/f32-prelu/gen/avx512f-2x32.c",
    "src/f32-raddexpminusmax/gen/avx512f-p5-scalef-x128.c",
    "src/f32-raddexpminusmax/gen/avx512f-p5-scalef-x128-acc2.c",
    "src/f32-raddexpminusmax/gen/avx512f-p5-scalef-x128-acc4.c",
    "src/f32-raddexpminusmax/gen/avx512f-p5-scalef-x144.c",
    "src/f32-raddexpminusmax/gen/avx512f-p5-scalef-x144-acc3.c",
    "src/f32-raddexpminusmax/gen/avx512f-p5-scalef-x160.c",
    "src/f32-raddexpminusmax/gen/avx512f-p5-scalef-x160-acc2.c",
    "src/f32-raddexpminusmax/gen/avx512f-p5-scalef-x160-acc5.c",
    "src/f32-raddexpminusmax/gen/avx512f-p5-scalef-x192.c",
    "src/f32-raddexpminusmax/gen/avx512f-p5-scalef-x192-acc2.c",
    "src/f32-raddexpminusmax/gen/avx512f-p5-scalef-x192-acc3.c",
    "src/f32-raddexpminusmax/gen/avx512f-p5-scalef-x192-acc6.c",
    "src/f32-raddextexp/gen/avx512f-p5-scalef-x128.c",
    "src/f32-raddextexp/gen/avx512f-p5-scalef-x128-acc2.c",
    "src/f32-raddextexp/gen/avx512f-p5-scalef-x128-acc4.c",
    "src/f32-raddextexp/gen/avx512f-p5-scalef-x144.c",
    "src/f32-raddextexp/gen/avx512f-p5-scalef-x144-acc3.c",
    "src/f32-raddextexp/gen/avx512f-p5-scalef-x160.c",
    "src/f32-raddextexp/gen/avx512f-p5-scalef-x160-acc2.c",
    "src/f32-raddextexp/gen/avx512f-p5-scalef-x160-acc5.c",
    "src/f32-raddextexp/gen/avx512f-p5-scalef-x192.c",
    "src/f32-raddextexp/gen/avx512f-p5-scalef-x192-acc2.c",
    "src/f32-raddextexp/gen/avx512f-p5-scalef-x192-acc3.c",
    "src/f32-raddextexp/gen/avx512f-p5-scalef-x192-acc6.c",
    "src/f32-raddstoreexpminusmax/gen/avx512f-p5-scalef-x128.c",
    "src/f32-raddstoreexpminusmax/gen/avx512f-p5-scalef-x128-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/avx512f-p5-scalef-x128-acc4.c",
    "src/f32-raddstoreexpminusmax/gen/avx512f-p5-scalef-x144.c",
    "src/f32-raddstoreexpminusmax/gen/avx512f-p5-scalef-x144-acc3.c",
    "src/f32-raddstoreexpminusmax/gen/avx512f-p5-scalef-x160.c",
    "src/f32-raddstoreexpminusmax/gen/avx512f-p5-scalef-x160-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/avx512f-p5-scalef-x160-acc5.c",
    "src/f32-raddstoreexpminusmax/gen/avx512f-p5-scalef-x192.c",
    "src/f32-raddstoreexpminusmax/gen/avx512f-p5-scalef-x192-acc2.c",
    "src/f32-raddstoreexpminusmax/gen/avx512f-p5-scalef-x192-acc3.c",
    "src/f32-raddstoreexpminusmax/gen/avx512f-p5-scalef-x192-acc6.c",
    "src/f32-rmax/avx512f.c",
    "src/f32-vbinary/gen/vadd-minmax-avx512f-x16.c",
    "src/f32-vbinary/gen/vadd-minmax-avx512f-x32.c",
    "src/f32-vbinary/gen/vaddc-minmax-avx512f-x16.c",
    "src/f32-vbinary/gen/vaddc-minmax-avx512f-x32.c",
    "src/f32-vbinary/gen/vdiv-minmax-avx512f-x16.c",
    "src/f32-vbinary/gen/vdiv-minmax-avx512f-x32.c",
    "src/f32-vbinary/gen/vdivc-minmax-avx512f-x16.c",
    "src/f32-vbinary/gen/vdivc-minmax-avx512f-x32.c",
    "src/f32-vbinary/gen/vmax-avx512f-x16.c",
    "src/f32-vbinary/gen/vmax-avx512f-x32.c",
    "src/f32-vbinary/gen/vmaxc-avx512f-x16.c",
    "src/f32-vbinary/gen/vmaxc-avx512f-x32.c",
    "src/f32-vbinary/gen/vmin-avx512f-x16.c",
    "src/f32-vbinary/gen/vmin-avx512f-x32.c",
    "src/f32-vbinary/gen/vminc-avx512f-x16.c",
    "src/f32-vbinary/gen/vminc-avx512f-x32.c",
    "src/f32-vbinary/gen/vmul-minmax-avx512f-x16.c",
    "src/f32-vbinary/gen/vmul-minmax-avx512f-x32.c",
    "src/f32-vbinary/gen/vmulc-minmax-avx512f-x16.c",
    "src/f32-vbinary/gen/vmulc-minmax-avx512f-x32.c",
    "src/f32-vbinary/gen/vrdivc-minmax-avx512f-x16.c",
    "src/f32-vbinary/gen/vrdivc-minmax-avx512f-x32.c",
    "src/f32-vbinary/gen/vrsubc-minmax-avx512f-x16.c",
    "src/f32-vbinary/gen/vrsubc-minmax-avx512f-x32.c",
    "src/f32-vbinary/gen/vsub-minmax-avx512f-x16.c",
    "src/f32-vbinary/gen/vsub-minmax-avx512f-x32.c",
    "src/f32-vbinary/gen/vsubc-minmax-avx512f-x16.c",
    "src/f32-vbinary/gen/vsubc-minmax-avx512f-x32.c",
    "src/f32-vscale/avx512f-unroll64.c",
    "src/f32-vscaleexpminusmax/gen/avx512f-p5-scalef-x16.c",
    "src/f32-vscaleexpminusmax/gen/avx512f-p5-scalef-x32.c",
    "src/f32-vscaleexpminusmax/gen/avx512f-p5-scalef-x48.c",
    "src/f32-vscaleexpminusmax/gen/avx512f-p5-scalef-x64.c",
    "src/f32-vscaleexpminusmax/gen/avx512f-p5-scalef-x80.c",
    "src/f32-vscaleexpminusmax/gen/avx512f-p5-scalef-x96.c",
    "src/f32-vscaleexpminusmax/gen/avx512f-p5-scalef-x112.c",
    "src/f32-vscaleexpminusmax/gen/avx512f-p5-scalef-x128.c",
    "src/f32-vscaleexpminusmax/gen/avx512f-p5-scalef-x144.c",
    "src/f32-vscaleexpminusmax/gen/avx512f-p5-scalef-x160.c",
    "src/f32-vscaleexpminusmax/gen/avx512f-p5-scalef-x176.c",
    "src/f32-vscaleexpminusmax/gen/avx512f-p5-scalef-x192.c",
    "src/f32-vscaleextexp/gen/avx512f-p5-scalef-x16.c",
    "src/f32-vscaleextexp/gen/avx512f-p5-scalef-x32.c",
    "src/f32-vscaleextexp/gen/avx512f-p5-scalef-x48.c",
    "src/f32-vscaleextexp/gen/avx512f-p5-scalef-x64.c",
    "src/f32-vscaleextexp/gen/avx512f-p5-scalef-x80.c",
    "src/f32-vscaleextexp/gen/avx512f-p5-scalef-x96.c",
    "src/f32-vscaleextexp/gen/avx512f-p5-scalef-x112.c",
    "src/f32-vscaleextexp/gen/avx512f-p5-scalef-x128.c",
    "src/f32-vscaleextexp/gen/avx512f-p5-scalef-x144.c",
    "src/f32-vscaleextexp/gen/avx512f-p5-scalef-x160.c",
    "src/f32-vscaleextexp/gen/avx512f-p5-scalef-x176.c",
    "src/f32-vscaleextexp/gen/avx512f-p5-scalef-x192.c",
    "src/math/exp-avx512f-p5-scalef.c",
    "src/math/exp-avx512f-p5.c",
    "src/math/exp-avx512f-perm-p3.c",
    "src/math/exp-avx512f-perm2-p2.c",
    "src/math/extexp-avx512f-p5.c",
]

AARCH32_ASM_UKERNELS = [
    "src/q8-dwconv/up8x9-minmax-aarch32-neon.S",
    "src/f32-gemm/4x8-minmax-aarch32-neon-cortex-a53.S",
    "src/f32-gemm/4x8-minmax-aarch32-neon-cortex-a55.S",
    "src/f32-gemm/gen/4x8-minmax-aarch32-neon-cortex-a75.S",
    "src/f32-gemm/gen/4x8-minmax-aarch32-neon-pld-cortex-a75.S",
    "src/f32-gemm/4x8-minmax-aarch32-neon-ld64.S",
    "src/f32-igemm/4x8-minmax-aarch32-neon-ld64.S",
    "src/f32-igemm/gen/4x8-minmax-aarch32-neon-cortex-a75.S",
    "src/f32-igemm/gen/4x8-minmax-aarch32-neon-pld-cortex-a75.S",
    "src/f32-igemm/4x8-minmax-aarch32-neon-cortex-a53.S",
    "src/f32-igemm/4x8-minmax-aarch32-neon-cortex-a55.S",
]

AARCH64_ASM_UKERNELS = [
    "src/f16-gemm/gen/1x16-minmax-aarch64-neonfp16arith-ld32.S",
    "src/f16-gemm/gen/4x16-minmax-aarch64-neonfp16arith-ld32.S",
    "src/f16-gemm/gen/6x16-minmax-aarch64-neonfp16arith-ld32.S",
    "src/f16-gemm/gen-inc/1x16inc-minmax-aarch64-neonfp16arith-ld32.S",
    "src/f16-gemm/gen-inc/4x16inc-minmax-aarch64-neonfp16arith-ld32.S",
    "src/f16-gemm/gen-inc/6x16inc-minmax-aarch64-neonfp16arith-ld32.S",
    "src/f32-dwconv/up4x9-minmax-aarch64-neonfma-cortex-a55.S",
    "src/f32-dwconv/up4x9-minmax-aarch64-neonfma.S",
    "src/f32-gemm/gen/1x8-minmax-aarch64-neonfma-ld64.S",
    "src/f32-gemm/gen/1x12-minmax-aarch64-neonfma-cortex-a53.S",
    "src/f32-gemm/gen/1x8-minmax-aarch64-neonfma-cortex-a53.S",
    "src/f32-gemm/gen/1x8-minmax-aarch64-neonfma-cortex-a57.S",
    "src/f32-gemm/gen/1x8-minmax-aarch64-neonfma-cortex-a75.S",
    "src/f32-gemm/gen/4x12-minmax-aarch64-neonfma-cortex-a53.S",
    "src/f32-gemm/gen/4x8-minmax-aarch64-neonfma-cortex-a53.S",
    "src/f32-gemm/gen/4x8-minmax-aarch64-neonfma-cortex-a55.S",
    "src/f32-gemm/gen/4x8-minmax-aarch64-neonfma-cortex-a57.S",
    "src/f32-gemm/gen/4x8-minmax-aarch64-neonfma-cortex-a75.S",
    "src/f32-gemm/gen/4x8-minmax-aarch64-neonfma-ld128.S",
    "src/f32-gemm/gen/4x8-minmax-aarch64-neonfma-ld64.S",
    "src/f32-gemm/gen/5x8-minmax-aarch64-neonfma-cortex-a57.S",
    "src/f32-gemm/gen/5x8-minmax-aarch64-neonfma-cortex-a75.S",
    "src/f32-gemm/gen/6x8-minmax-aarch64-neonfma-cortex-a53.S",
    "src/f32-gemm/gen/6x8-minmax-aarch64-neonfma-cortex-a55.S",
    "src/f32-gemm/gen/6x8-minmax-aarch64-neonfma-cortex-a73.S",
    "src/f32-gemm/gen/6x8-minmax-aarch64-neonfma-cortex-a57.S",
    "src/f32-gemm/gen/6x8-minmax-aarch64-neonfma-cortex-a75.S",
    "src/f32-gemm/gen/6x8-minmax-aarch64-neonfma-ios.S",
    "src/f32-gemm/gen/6x8-minmax-aarch64-neonfma-ld128.S",
    "src/f32-gemm/gen/6x8-minmax-aarch64-neonfma-ld64.S",
    "src/f32-gemm/gen-inc/1x8inc-minmax-aarch64-neonfma-ld64.S",
    "src/f32-gemm/gen-inc/1x12inc-minmax-aarch64-neonfma-cortex-a53.S",
    "src/f32-gemm/gen-inc/1x8inc-minmax-aarch64-neonfma-cortex-a53.S",
    "src/f32-gemm/gen-inc/1x8inc-minmax-aarch64-neonfma-cortex-a57.S",
    "src/f32-gemm/gen-inc/1x8inc-minmax-aarch64-neonfma-cortex-a75.S",
    "src/f32-gemm/gen-inc/4x12inc-minmax-aarch64-neonfma-cortex-a53.S",
    "src/f32-gemm/gen-inc/4x8inc-minmax-aarch64-neonfma-cortex-a53.S",
    "src/f32-gemm/gen-inc/4x8inc-minmax-aarch64-neonfma-cortex-a55.S",
    "src/f32-gemm/gen-inc/4x8inc-minmax-aarch64-neonfma-cortex-a57.S",
    "src/f32-gemm/gen-inc/4x8inc-minmax-aarch64-neonfma-cortex-a75.S",
    "src/f32-gemm/gen-inc/4x8inc-minmax-aarch64-neonfma-ld128.S",
    "src/f32-gemm/gen-inc/4x8inc-minmax-aarch64-neonfma-ld64.S",
    "src/f32-gemm/gen-inc/5x8inc-minmax-aarch64-neonfma-cortex-a57.S",
    "src/f32-gemm/gen-inc/5x8inc-minmax-aarch64-neonfma-cortex-a75.S",
    "src/f32-gemm/gen-inc/6x8inc-minmax-aarch64-neonfma-cortex-a53.S",
    "src/f32-gemm/gen-inc/6x8inc-minmax-aarch64-neonfma-cortex-a55.S",
    "src/f32-gemm/gen-inc/6x8inc-minmax-aarch64-neonfma-cortex-a73.S",
    "src/f32-gemm/gen-inc/6x8inc-minmax-aarch64-neonfma-cortex-a57.S",
    "src/f32-gemm/gen-inc/6x8inc-minmax-aarch64-neonfma-cortex-a75.S",
    "src/f32-gemm/gen-inc/6x8inc-minmax-aarch64-neonfma-ios.S",
    "src/f32-gemm/gen-inc/6x8inc-minmax-aarch64-neonfma-ld128.S",
    "src/f32-gemm/gen-inc/6x8inc-minmax-aarch64-neonfma-ld64.S",
    "src/f32-igemm/1x12-minmax-aarch64-neonfma-cortex-a53.S",
    "src/f32-igemm/1x8-minmax-aarch64-neonfma-cortex-a53.S",
    "src/f32-igemm/gen/1x8-minmax-aarch64-neonfma-cortex-a57.S",
    "src/f32-igemm/gen/1x8-minmax-aarch64-neonfma-cortex-a75.S",
    "src/f32-igemm/4x12-minmax-aarch64-neonfma-cortex-a53.S",
    "src/f32-igemm/4x8-minmax-aarch64-neonfma-cortex-a53.S",
    "src/f32-igemm/4x8-minmax-aarch64-neonfma-cortex-a55.S",
    "src/f32-igemm/gen/4x8-minmax-aarch64-neonfma-cortex-a57.S",
    "src/f32-igemm/gen/4x8-minmax-aarch64-neonfma-cortex-a75.S",
    "src/f32-igemm/gen/5x8-minmax-aarch64-neonfma-cortex-a57.S",
    "src/f32-igemm/gen/5x8-minmax-aarch64-neonfma-cortex-a75.S",
    "src/f32-igemm/6x8-minmax-aarch64-neonfma-cortex-a53.S",
    "src/f32-igemm/6x8-minmax-aarch64-neonfma-cortex-a55.S",
    "src/f32-igemm/6x8-minmax-aarch64-neonfma-cortex-a73.S",
    "src/f32-igemm/gen/6x8-minmax-aarch64-neonfma-cortex-a57.S",
    "src/f32-igemm/gen/6x8-minmax-aarch64-neonfma-cortex-a75.S",
    "src/f32-igemm/gen/6x8-minmax-aarch64-neonfma-ios.S",
]

INTERNAL_MICROKERNEL_HDRS = [
    "src/requantization/gemmlowp-requantization.h",
    "src/xnnpack/argmaxpool.h",
    "src/xnnpack/avgpool.h",
    "src/xnnpack/clamp.h",
    "src/xnnpack/common.h",
    "src/xnnpack/conv.h",
    "src/xnnpack/dwconv.h",
    "src/xnnpack/gavgpool.h",
    "src/xnnpack/gemm.h",
    "src/xnnpack/hswish.h",
    "src/xnnpack/ibilinear.h",
    "src/xnnpack/igemm.h",
    "src/xnnpack/intrinsics-polyfill.h",
    "src/xnnpack/lut.h",
    "src/xnnpack/math.h",
    "src/xnnpack/maxpool.h",
    "src/xnnpack/memory.h",
    "src/xnnpack/packx.h",
    "src/xnnpack/pad.h",
    "src/xnnpack/params.h",
    "src/xnnpack/pavgpool.h",
    "src/xnnpack/ppmm.h",
    "src/xnnpack/prelu.h",
    "src/xnnpack/raddexpminusmax.h",
    "src/xnnpack/raddextexp.h",
    "src/xnnpack/raddstoreexpminusmax.h",
    "src/xnnpack/rmax.h",
    "src/xnnpack/scalar-utils.h",
    "src/xnnpack/spmm.h",
    "src/xnnpack/unpool.h",
    "src/xnnpack/vadd.h",
    "src/xnnpack/vbinary.h",
    "src/xnnpack/vmulcaddc.h",
    "src/xnnpack/vscale.h",
    "src/xnnpack/vscaleexpminusmax.h",
    "src/xnnpack/vscaleextexp.h",
    "src/xnnpack/vunary.h",
    "src/xnnpack/zip.h",
]

INTERNAL_HDRS = INTERNAL_MICROKERNEL_HDRS + [
    "include/xnnpack.h",
    "src/xnnpack/allocator.h",
    "src/xnnpack/compute.h",
    "src/xnnpack/im2col.h",
    "src/xnnpack/indirection.h",
    "src/xnnpack/math-stubs.h",
    "src/xnnpack/operator.h",
    "src/xnnpack/pack.h",
    "src/xnnpack/params-init.h",
    "src/xnnpack/requantization-stubs.h",
    "src/xnnpack/requantization.h",
    "src/xnnpack/subgraph.h",
]

ACCURACY_EVAL_HDRS = INTERNAL_MICROKERNEL_HDRS + [
    "src/xnnpack/math-stubs.h",
]

MICROKERNEL_BENCHMARK_HDRS = INTERNAL_MICROKERNEL_HDRS + [
    "src/xnnpack/params-init.h",
    "include/xnnpack.h",
]

MICROKERNEL_TEST_HDRS = INTERNAL_MICROKERNEL_HDRS + [
    "src/xnnpack/isa-checks.h",
    "src/xnnpack/params-init.h",
    "src/xnnpack/requantization.h",
    "include/xnnpack.h",
]

OPERATOR_TEST_PARAMS_HDRS = [
    "src/xnnpack/params.h",
    "src/xnnpack/common.h",
]

WEIGHTS_PACK_HDRS = [
    "src/xnnpack/pack.h",
    "src/xnnpack/operator.h",
    "src/xnnpack/compute.h",
]

LOGGING_COPTS = select({
    # No logging in optimized mode
    ":optimized_build": ["-DXNN_LOG_LEVEL=0"],
    # Full logging in debug mode
    ":debug_build": ["-DXNN_LOG_LEVEL=5"],
    # Error-only logging in default (fastbuild) mode
    "//conditions:default": ["-DXNN_LOG_LEVEL=2"],
})

LOGGING_HDRS = [
    "src/xnnpack/log.h",
]

xnnpack_cc_library(
    name = "tables",
    srcs = TABLE_SRCS,
    hdrs = INTERNAL_HDRS,
    copts = xnnpack_std_copts(),
)

xnnpack_cc_library(
    name = "scalar_ukernels",
    srcs = SCALAR_UKERNELS,
    hdrs = INTERNAL_HDRS,
    aarch32_copts = ["-marm"],
    copts = xnnpack_std_copts(),
    deps = [
        ":tables",
        "@FP16",
        "@FXdiv",
        "@pthreadpool",
    ],
)

xnnpack_cc_library(
    name = "wasm_ukernels",
    hdrs = INTERNAL_HDRS,
    copts = xnnpack_std_copts(),
    wasm_srcs = WASM_UKERNELS,
    deps = [
        ":tables",
        "@FP16",
        "@FXdiv",
        "@pthreadpool",
    ],
)

xnnpack_cc_library(
    name = "psimd_fastmath_ukernels",
    srcs = PSIMD_FASTMATH_UKERNELS,
    hdrs = INTERNAL_HDRS,
    aarch32_copts = [
        "-marm",
        "-mfpu=neon",
    ],
    copts = xnnpack_std_copts(),
    optimized_copts = [
        "-O3",
        "-ffast-math",
    ],
    deps = [
        ":tables",
        "@FP16",
        "@psimd",
        "@pthreadpool",
    ],
)

xnnpack_cc_library(
    name = "psimd_accmath_ukernels",
    srcs = PSIMD_ACCMATH_UKERNELS,
    hdrs = INTERNAL_HDRS,
    aarch32_copts = [
        "-marm",
        "-mfpu=neon",
    ],
    copts = xnnpack_std_copts(),
    optimized_copts = [
        "-O3",
    ],
    deps = [
        ":tables",
        "@FP16",
        "@psimd",
        "@pthreadpool",
    ],
)

xnnpack_cc_library(
    name = "neon_ukernels",
    hdrs = INTERNAL_HDRS,
    aarch32_copts = [
        "-marm",
        "-mfpu=neon",
    ],
    aarch32_srcs = NEON_UKERNELS,
    aarch64_srcs = NEON_UKERNELS,
    copts = xnnpack_std_copts(),
    deps = [
        ":tables",
        "@FP16",
        "@pthreadpool",
    ],
)

xnnpack_cc_library(
    name = "neonfma_ukernels",
    hdrs = INTERNAL_HDRS,
    aarch32_copts = [
        "-marm",
        "-mfpu=neon-vfpv4",
    ],
    aarch32_srcs = NEONFMA_UKERNELS,
    aarch64_srcs = NEONFMA_UKERNELS + AARCH64_NEONFMA_UKERNELS,
    copts = xnnpack_std_copts(),
    deps = [
        ":tables",
        "@FP16",
        "@pthreadpool",
    ],
)

xnnpack_cc_library(
    name = "neonfp16arith_ukernels",
    hdrs = INTERNAL_HDRS,
    aarch64_copts = ["-march=armv8.2-a+fp16"],
    aarch64_srcs = AARCH64_NEONFP16ARITH_UKERNELS,
    copts = xnnpack_std_copts(),
    deps = [
        ":tables",
        "@FP16",
        "@pthreadpool",
    ],
)

xnnpack_cc_library(
    name = "sse2_ukernels",
    hdrs = INTERNAL_HDRS,
    copts = xnnpack_std_copts(),
    x86_copts = ["-msse2"],
    x86_srcs = SSE_UKERNELS + SSE2_UKERNELS,
    deps = [
        ":tables",
        "@FP16",
        "@pthreadpool",
    ],
)

xnnpack_cc_library(
    name = "ssse3_ukernels",
    hdrs = INTERNAL_HDRS,
    copts = xnnpack_std_copts(),
    x86_copts = ["-mssse3"],
    x86_srcs = SSSE3_UKERNELS,
    deps = [
        ":tables",
        "@FP16",
        "@pthreadpool",
    ],
)

xnnpack_cc_library(
    name = "sse41_ukernels",
    hdrs = INTERNAL_HDRS,
    copts = xnnpack_std_copts(),
    x86_copts = ["-msse4.1"],
    x86_srcs = SSE41_UKERNELS,
    deps = [
        ":tables",
        "@FP16",
        "@pthreadpool",
    ],
)

xnnpack_cc_library(
    name = "avx_ukernels",
    hdrs = INTERNAL_HDRS,
    copts = xnnpack_std_copts(),
    x86_copts = ["-mavx"],
    x86_srcs = AVX_UKERNELS,
    deps = [
        ":tables",
        "@FP16",
        "@pthreadpool",
    ],
)

xnnpack_cc_library(
    name = "fma3_ukernels",
    hdrs = INTERNAL_HDRS,
    copts = xnnpack_std_copts(),
    x86_copts = [
        "-mfma",
    ],
    x86_srcs = FMA3_UKERNELS,
    deps = [
        ":tables",
        "@FP16",
        "@pthreadpool",
    ],
)

xnnpack_cc_library(
    name = "avx2_ukernels",
    hdrs = INTERNAL_HDRS,
    copts = xnnpack_std_copts(),
    x86_copts = [
        "-mfma",
        "-mavx2",
    ],
    x86_srcs = AVX2_UKERNELS,
    deps = [
        ":tables",
        "@FP16",
        "@pthreadpool",
    ],
)

xnnpack_cc_library(
    name = "avx512f_ukernels",
    hdrs = INTERNAL_HDRS,
    copts = xnnpack_std_copts(),
    x86_copts = ["-mavx512f"],
    x86_srcs = AVX512F_UKERNELS,
    deps = [
        ":tables",
        "@FP16",
        "@pthreadpool",
    ],
)

xnnpack_cc_library(
    name = "asm_ukernels",
    hdrs = ["src/xnnpack/assembly.h"],
    aarch32_srcs = AARCH32_ASM_UKERNELS,
    aarch64_copts = ["-march=armv8.2-a+fp16"],
    aarch64_srcs = AARCH64_ASM_UKERNELS,
)

xnnpack_aggregate_library(
    name = "ukernels",
    aarch32_deps = [
        ":psimd_fastmath_ukernels",
        ":psimd_accmath_ukernels",
        ":neon_ukernels",
        ":neonfma_ukernels",
        ":asm_ukernels",
    ],
    aarch64_deps = [
        ":psimd_fastmath_ukernels",
        ":psimd_accmath_ukernels",
        ":neon_ukernels",
        ":neonfma_ukernels",
        ":neonfp16arith_ukernels",
        ":asm_ukernels",
    ],
    generic_deps = [":scalar_ukernels"],
    wasm_deps = [
        ":wasm_ukernels",
    ],
    wasmsimd_deps = [
        ":wasm_ukernels",
        ":psimd_fastmath_ukernels",
        ":psimd_accmath_ukernels",
    ],
    x86_deps = [
        ":psimd_fastmath_ukernels",
        ":psimd_accmath_ukernels",
        ":sse2_ukernels",
        ":ssse3_ukernels",
        ":sse41_ukernels",
        ":avx_ukernels",
        ":fma3_ukernels",
        ":avx2_ukernels",
        ":avx512f_ukernels",
    ],
)

xnnpack_cc_library(
    name = "im2col",
    srcs = ["src/im2col.c"],
    hdrs = [
        "src/xnnpack/common.h",
        "src/xnnpack/im2col.h",
    ],
    copts = xnnpack_std_copts(),
)

xnnpack_cc_library(
    name = "indirection",
    srcs = ["src/indirection.c"],
    hdrs = INTERNAL_HDRS,
    copts = xnnpack_std_copts(),
    deps = [
        "@FP16",
        "@FXdiv",
        "@pthreadpool",
    ],
)

xnnpack_cc_library(
    name = "operator_run",
    srcs = ["src/operator-run.c"],
    hdrs = INTERNAL_HDRS + LOGGING_HDRS,
    copts = xnnpack_std_copts() + LOGGING_COPTS + [
        # Wrappers for multi-pass microkernels use VLAs for temporary buffers.
        "-Wno-vla",
    ] + select({
        ":xnn_enable_hmp_explicit_false": ["-DXNN_MAX_UARCH_TYPES=1"],
        "//conditions:default": [],
    }),
    deps = [
        "@FP16",
        "@FXdiv",
        "@clog",
        "@pthreadpool",
    ],
)

cc_library(
    name = "enable_assembly",
    defines = select({
        ":xnn_enable_assembly_explicit_true": ["XNN_ENABLE_ASSEMBLY=1"],
        ":xnn_enable_assembly_explicit_false": ["XNN_ENABLE_ASSEMBLY=0"],
        "//conditions:default": ["XNN_ENABLE_ASSEMBLY=1"],
    }),
)

xnnpack_cc_library(
    name = "operators",
    srcs = OPERATOR_SRCS + [
        "src/memory.c",
        "src/operator-delete.c",
    ],
    hdrs = INTERNAL_HDRS + LOGGING_HDRS,
    copts = xnnpack_std_copts() + LOGGING_COPTS + [
        "-Isrc",
        "-Iinclude",
    ] + select({
        ":debug_build": [],
        "//conditions:default": xnnpack_min_size_copts(),
    }) + select({
        ":xnn_enable_hmp_explicit_false": ["-DXNN_MAX_UARCH_TYPES=1"],
        "//conditions:default": [],
    }),
    wasm_srcs = ["src/wasm-stubs.c"],
    wasmsimd_srcs = ["src/wasm-stubs.c"],
    deps = [
        ":indirection",
        "@FP16",
        "@FXdiv",
        "@clog",
        "@pthreadpool",
    ],
)

cc_library(
    name = "XNNPACK",
    srcs = [
        "src/init.c",
        "src/runtime.c",
        "src/subgraph.c",
        "src/tensor.c",
    ],
    copts = xnnpack_std_copts() + LOGGING_COPTS + [
        "-Isrc",
        "-Iinclude",
    ] + select({
        ":debug_build": [],
        "//conditions:default": xnnpack_min_size_copts(),
    }) + select({
        ":xnn_enable_hmp_explicit_false": ["-DXNN_MAX_UARCH_TYPES=1"],
        "//conditions:default": [],
    }),
    includes = ["include"],
    linkstatic = True,
    textual_hdrs = ["include/xnnpack.h"],
    visibility = xnnpack_visibility(),
    deps = [
        ":enable_assembly",
        ":ukernels",
        ":operator_run",
        ":operators",
        "@clog",
        "@pthreadpool",
    ] + select({
        ":emscripten": [],
        "//conditions:default": ["@cpuinfo"],
    }),
)

cc_library(
    name = "xnnpack_operators_nhwc_f32",
    srcs = [
        "src/init.c",
    ],
    copts = xnnpack_std_copts() + LOGGING_COPTS + [
        "-Isrc",
        "-Iinclude",
    ] + select({
        ":debug_build": [],
        "//conditions:default": xnnpack_min_size_copts(),
    }) + select({
        ":xnn_enable_hmp_explicit_false": ["-DXNN_MAX_UARCH_TYPES=1"],
        "//conditions:default": [],
    }),
    defines = [
        "XNN_NO_Q8_OPERATORS",
        "XNN_NO_U8_OPERATORS",
        "XNN_NO_X8_OPERATORS",
        "XNN_NO_NCHW_OPERATORS",
    ],
    includes = ["include"],
    linkstatic = True,
    textual_hdrs = ["include/xnnpack.h"],
    visibility = xnnpack_visibility(),
    deps = [
        ":enable_assembly",
        ":ukernels",
        ":operator_run",
        ":operators",
        "@clog",
        "@pthreadpool",
    ] + select({
        ":emscripten": [],
        "//conditions:default": ["@cpuinfo"],
    }),
)

xnnpack_cc_library(
    name = "bench_utils",
    srcs = ["bench/utils.cc"],
    hdrs = ["bench/utils.h"],
    copts = ["-Wno-unused-result"],
    deps = [
        "@com_google_benchmark//:benchmark",
        "@cpuinfo",
    ],
)

######################### Benchmarks for micro-kernels #########################

xnnpack_benchmark(
    name = "q8_gemm_bench",
    srcs = [
        "bench/gemm.h",
        "bench/q8-gemm.cc",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_BENCHMARK_HDRS,
    copts = ["-Wno-unused-function"] + xnnpack_optional_ruy_copts() + xnnpack_optional_gemmlowp_copts(),
    deps = MICROKERNEL_BENCHMARK_DEPS + xnnpack_optional_ruy_deps() + xnnpack_optional_gemmlowp_deps(),
)

xnnpack_benchmark(
    name = "f16_gemm_bench",
    srcs = [
        "bench/f16-gemm.cc",
        "bench/gemm.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_BENCHMARK_HDRS,
    copts = ["-Wno-unused-function"],
    deps = MICROKERNEL_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "f16_spmm_bench",
    srcs = [
        "bench/f16-spmm.cc",
        "bench/gemm.h",
        "bench/google/gemm.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_BENCHMARK_HDRS,
    copts = ["-Wno-unused-function"],
    deps = MICROKERNEL_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "f32_igemm_bench",
    srcs = [
        "bench/f32-igemm.cc",
        "bench/conv.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_BENCHMARK_HDRS,
    deps = MICROKERNEL_BENCHMARK_DEPS + [":indirection"],
)

xnnpack_benchmark(
    name = "f32_conv_hwc_bench",
    srcs = [
        "bench/f32-conv-hwc.cc",
        "bench/dconv.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_BENCHMARK_HDRS,
    copts = ["-Wno-unused-function"],
    deps = MICROKERNEL_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "f32_conv_hwc2spchw_bench",
    srcs = [
        "bench/f32-conv-hwc2spchw.cc",
        "bench/dconv.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_BENCHMARK_HDRS,
    copts = ["-Wno-unused-function"],
    deps = MICROKERNEL_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "f32_dwconv_bench",
    srcs = [
        "bench/f32-dwconv.cc",
        "bench/dwconv.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_BENCHMARK_HDRS,
    deps = MICROKERNEL_BENCHMARK_DEPS + [":indirection"],
)

xnnpack_benchmark(
    name = "f32_dwconv_spchw_bench",
    srcs = [
        "bench/f32-dwconv-spchw.cc",
        "bench/dwconv.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_BENCHMARK_HDRS,
    deps = MICROKERNEL_BENCHMARK_DEPS + [":indirection"],
)

xnnpack_benchmark(
    name = "f32_gemm_bench",
    srcs = [
        "bench/f32-gemm.cc",
        "bench/gemm.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_BENCHMARK_HDRS,
    copts = ["-Wno-unused-function"] + xnnpack_optional_ruy_copts(),
    deps = MICROKERNEL_BENCHMARK_DEPS + xnnpack_optional_ruy_deps(),
)

xnnpack_benchmark(
    name = "f32_raddexpminusmax_bench",
    srcs = [
        "bench/f32-raddexpminusmax.cc",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_BENCHMARK_HDRS,
    deps = MICROKERNEL_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "f32_raddextexp_bench",
    srcs = [
        "bench/f32-raddextexp.cc",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_BENCHMARK_HDRS,
    deps = MICROKERNEL_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "f32_raddstoreexpminusmax_bench",
    srcs = [
        "bench/f32-raddstoreexpminusmax.cc",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_BENCHMARK_HDRS,
    deps = MICROKERNEL_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "f32_rmax_bench",
    srcs = [
        "bench/f32-rmax.cc",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_BENCHMARK_HDRS,
    deps = MICROKERNEL_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "f32_sigmoid_bench",
    srcs = [
        "bench/f32-sigmoid.cc",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_BENCHMARK_HDRS,
    copts = ["-Wno-unused-function"],
    deps = MICROKERNEL_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "f32_spmm_bench",
    srcs = [
        "bench/f32-spmm.cc",
        "bench/gemm.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_BENCHMARK_HDRS,
    copts = ["-Wno-unused-function"],
    deps = MICROKERNEL_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "f32_softmax_bench",
    srcs = [
        "bench/f32-softmax.cc",
    ] + MICROKERNEL_BENCHMARK_HDRS,
    copts = ["-Wno-unused-function"] + xnnpack_optional_dnnl_copts(),
    deps = MICROKERNEL_BENCHMARK_DEPS + xnnpack_optional_dnnl_deps(),
)

xnnpack_benchmark(
    name = "f32_vscaleexpminusmax_bench",
    srcs = [
        "bench/f32-vscaleexpminusmax.cc",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_BENCHMARK_HDRS,
    deps = MICROKERNEL_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "f32_vscaleextexp_bench",
    srcs = [
        "bench/f32-vscaleextexp.cc",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_BENCHMARK_HDRS,
    deps = MICROKERNEL_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "f32_im2col_gemm_bench",
    srcs = [
        "bench/f32-im2col-gemm.cc",
        "bench/conv.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_BENCHMARK_HDRS,
    deps = MICROKERNEL_BENCHMARK_DEPS + [":im2col"],
)

xnnpack_benchmark(
    name = "requantization_bench",
    srcs = [
        "bench/requantization.cc",
        "src/xnnpack/requantization-stubs.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_BENCHMARK_HDRS,
    deps = MICROKERNEL_BENCHMARK_DEPS,
)

########################### Benchmarks for operators ###########################

xnnpack_benchmark(
    name = "add_bench",
    srcs = ["bench/add.cc"],
    deps = OPERATOR_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "average_pooling_bench",
    srcs = ["bench/average-pooling.cc"],
    copts = xnnpack_optional_tflite_copts(),
    deps = OPERATOR_BENCHMARK_DEPS + xnnpack_optional_tflite_deps(),
)

xnnpack_benchmark(
    name = "channel_shuffle_bench",
    srcs = ["bench/channel-shuffle.cc"],
    deps = OPERATOR_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "convolution_bench",
    srcs = ["bench/convolution.cc"],
    copts = xnnpack_optional_tflite_copts() + xnnpack_optional_armcl_copts(),
    deps = OPERATOR_BENCHMARK_DEPS + xnnpack_optional_tflite_deps() + xnnpack_optional_armcl_deps(),
)

xnnpack_benchmark(
    name = "deconvolution_bench",
    srcs = ["bench/deconvolution.cc"],
    copts = xnnpack_optional_tflite_copts(),
    deps = OPERATOR_BENCHMARK_DEPS + xnnpack_optional_tflite_deps(),
)

xnnpack_benchmark(
    name = "global_average_pooling_bench",
    srcs = ["bench/global-average-pooling.cc"],
    deps = OPERATOR_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "max_pooling_bench",
    srcs = ["bench/max-pooling.cc"],
    deps = OPERATOR_BENCHMARK_DEPS,
)

xnnpack_benchmark(
    name = "sigmoid_bench",
    srcs = ["bench/sigmoid.cc"],
    copts = xnnpack_optional_tflite_copts(),
    deps = OPERATOR_BENCHMARK_DEPS + xnnpack_optional_tflite_deps(),
)

xnnpack_benchmark(
    name = "prelu_bench",
    srcs = ["bench/prelu.cc"],
    copts = xnnpack_optional_tflite_copts(),
    deps = OPERATOR_BENCHMARK_DEPS + xnnpack_optional_tflite_deps(),
)

xnnpack_benchmark(
    name = "softmax_bench",
    srcs = ["bench/softmax.cc"],
    copts = xnnpack_optional_tflite_copts(),
    deps = OPERATOR_BENCHMARK_DEPS + xnnpack_optional_tflite_deps(),
)

############################# End-to-end benchmarks ############################

cc_library(
    name = "mobilenet_v1",
    srcs = ["models/mobilenet-v1.cc"],
    hdrs = ["models/models.h"],
    copts = xnnpack_std_cxxopts(),
    linkstatic = True,
    deps = [
        ":XNNPACK",
        "@pthreadpool",
    ],
)

cc_library(
    name = "mobilenet_v2",
    srcs = ["models/mobilenet-v2.cc"],
    hdrs = ["models/models.h"],
    copts = xnnpack_std_cxxopts(),
    linkstatic = True,
    deps = [
        ":XNNPACK",
        "@pthreadpool",
    ],
)

cc_library(
    name = "mobilenet_v3_large",
    srcs = ["models/mobilenet-v3-large.cc"],
    hdrs = ["models/models.h"],
    copts = xnnpack_std_cxxopts(),
    linkstatic = True,
    deps = [
        ":XNNPACK",
        "@pthreadpool",
    ],
)

cc_library(
    name = "mobilenet_v3_small",
    srcs = ["models/mobilenet-v3-small.cc"],
    hdrs = ["models/models.h"],
    copts = xnnpack_std_cxxopts(),
    linkstatic = True,
    deps = [
        ":XNNPACK",
        "@pthreadpool",
    ],
)

xnnpack_benchmark(
    name = "f32_dwconv_e2e_bench",
    srcs = [
        "bench/f32-dwconv-e2e.cc",
        "bench/end2end.h",
    ] + MICROKERNEL_BENCHMARK_HDRS,
    copts = ["-Wno-unused-function"],
    deps = MICROKERNEL_BENCHMARK_DEPS + [
        ":XNNPACK",
        ":mobilenet_v1",
        ":mobilenet_v2",
        ":mobilenet_v3_large",
        ":mobilenet_v3_small",
    ],
)

xnnpack_benchmark(
    name = "f32_gemm_e2e_bench",
    srcs = [
        "bench/f32-gemm-e2e.cc",
        "bench/end2end.h",
    ] + MICROKERNEL_BENCHMARK_HDRS,
    copts = ["-Wno-unused-function"],
    deps = MICROKERNEL_BENCHMARK_DEPS + [
        ":XNNPACK",
        ":mobilenet_v1",
        ":mobilenet_v2",
        ":mobilenet_v3_large",
        ":mobilenet_v3_small",
    ],
)

xnnpack_benchmark(
    name = "end2end_bench",
    srcs = ["bench/end2end.cc"],
    deps = [
        ":XNNPACK",
        ":bench_utils",
        ":mobilenet_v1",
        ":mobilenet_v2",
        ":mobilenet_v3_large",
        ":mobilenet_v3_small",
        "@pthreadpool",
    ],
)

#################### Accuracy evaluation for math functions ####################

xnnpack_benchmark(
    name = "f32_exp_eval",
    srcs = [
        "eval/f32-exp.cc",
        "src/xnnpack/AlignedAllocator.h",
    ] + ACCURACY_EVAL_HDRS,
    deps = ACCURACY_EVAL_DEPS,
)

xnnpack_benchmark(
    name = "f32_expminus_eval",
    srcs = [
        "eval/f32-expminus.cc",
        "src/xnnpack/AlignedAllocator.h",
    ] + ACCURACY_EVAL_HDRS,
    deps = ACCURACY_EVAL_DEPS,
)

xnnpack_benchmark(
    name = "f32_extexp_eval",
    srcs = [
        "eval/f32-extexp.cc",
        "src/xnnpack/AlignedAllocator.h",
    ] + ACCURACY_EVAL_HDRS,
    deps = ACCURACY_EVAL_DEPS,
)

xnnpack_benchmark(
    name = "f32_sigmoid_eval",
    srcs = [
        "eval/f32-sigmoid.cc",
        "src/xnnpack/AlignedAllocator.h",
    ] + ACCURACY_EVAL_HDRS,
    deps = ACCURACY_EVAL_DEPS,
)

######################### Unit tests for micro-kernels #########################

xnnpack_unit_test(
    name = "f16_gemm_minmax_test",
    srcs = [
        "test/f16-gemm-minmax.cc",
        "test/gemm-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f16_spmm_minmax_test",
    srcs = [
        "test/f16-spmm-minmax.cc",
        "test/spmm-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_argmaxpool_test",
    srcs = [
        "test/f32-argmaxpool.cc",
        "test/argmaxpool-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_avgpool_minmax_test",
    srcs = [
        "test/f32-avgpool-minmax.cc",
        "test/avgpool-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_ibilinear_test",
    srcs = [
        "test/f32-ibilinear.cc",
        "test/ibilinear-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_clamp_test",
    srcs = [
        "test/f32-clamp.cc",
        "test/clamp-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_igemm_test",
    srcs = [
        "test/f32-igemm.cc",
        "test/gemm-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_igemm_minmax_test",
    srcs = [
        "test/f32-igemm-minmax.cc",
        "test/gemm-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_conv_hwc_test",
    srcs = [
        "test/f32-conv-hwc.cc",
        "test/conv-hwc-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_conv_hwc2spchw_test",
    srcs = [
        "test/f32-conv-hwc2spchw.cc",
        "test/conv-hwc2spchw-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_dwconv_test",
    srcs = [
        "test/f32-dwconv.cc",
        "test/dwconv-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_dwconv_minmax_test",
    srcs = [
        "test/f32-dwconv-minmax.cc",
        "test/dwconv-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_dwconv_spchw_test",
    srcs = [
        "test/f32-dwconv-spchw.cc",
        "test/dwconv-spchw-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_gavgpool_minmax_test",
    srcs = [
        "test/f32-gavgpool-minmax.cc",
        "test/gavgpool-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_gavgpool_spchw_test",
    srcs = [
        "test/f32-gavgpool-spchw.cc",
        "test/gavgpool-spchw-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_gemm_test",
    srcs = [
        "test/f32-gemm.cc",
        "test/gemm-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_gemm_minmax_test",
    srcs = [
        "test/f32-gemm-minmax.cc",
        "test/gemm-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_gemminc_minmax_test",
    srcs = [
        "test/f32-gemminc-minmax.cc",
        "test/gemm-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_hswish_test",
    srcs = [
        "test/f32-hswish.cc",
        "test/hswish-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_maxpool_minmax_test",
    srcs = [
        "test/f32-maxpool-minmax.cc",
        "test/maxpool-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_pavgpool_minmax_test",
    srcs = [
        "test/f32-pavgpool-minmax.cc",
        "test/avgpool-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_ppmm_minmax_test",
    srcs = [
        "test/f32-ppmm-minmax.cc",
        "test/gemm-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_prelu_test",
    srcs = [
        "test/f32-prelu.cc",
        "test/prelu-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_raddexpminusmax_test",
    srcs = [
        "test/f32-raddexpminusmax.cc",
        "test/raddexpminusmax-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_raddextexp_test",
    srcs = [
        "test/f32-raddextexp.cc",
        "test/raddextexp-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_raddstoreexpminusmax_test",
    srcs = [
        "test/f32-raddstoreexpminusmax.cc",
        "test/raddstoreexpminusmax-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_rmax_test",
    srcs = [
        "test/f32-rmax.cc",
        "test/rmax-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_sigmoid_test",
    srcs = [
        "test/f32-sigmoid.cc",
        "test/vunary-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_spmm_minmax_test",
    srcs = [
        "test/f32-spmm-minmax.cc",
        "test/spmm-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vadd_minmax_test",
    srcs = [
        "test/f32-vadd-minmax.cc",
        "test/vbinary-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vaddc_minmax_test",
    srcs = [
        "test/f32-vaddc-minmax.cc",
        "test/vbinaryc-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vdiv_minmax_test",
    srcs = [
        "test/f32-vdiv-minmax.cc",
        "test/vbinary-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vdivc_minmax_test",
    srcs = [
        "test/f32-vdivc-minmax.cc",
        "test/vbinaryc-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vrdivc_minmax_test",
    srcs = [
        "test/f32-vrdivc-minmax.cc",
        "test/vbinaryc-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vmax_test",
    srcs = [
        "test/f32-vmax.cc",
        "test/vbinary-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vmaxc_test",
    srcs = [
        "test/f32-vmaxc.cc",
        "test/vbinaryc-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vmin_test",
    srcs = [
        "test/f32-vmin.cc",
        "test/vbinary-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vminc_test",
    srcs = [
        "test/f32-vminc.cc",
        "test/vbinaryc-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vmul_minmax_test",
    srcs = [
        "test/f32-vmul-minmax.cc",
        "test/vbinary-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vmulc_minmax_test",
    srcs = [
        "test/f32-vmulc-minmax.cc",
        "test/vbinaryc-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vmulcaddc_minmax_test",
    srcs = [
        "test/f32-vmulcaddc-minmax.cc",
        "test/vmulcaddc-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vscale_test",
    srcs = [
        "test/f32-vscale.cc",
        "test/vscale-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vscaleexpminusmax_test",
    srcs = [
        "test/f32-vscaleexpminusmax.cc",
        "test/vscaleexpminusmax-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vscaleextexp_test",
    srcs = [
        "test/f32-vscaleextexp.cc",
        "test/vscaleextexp-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vsub_minmax_test",
    srcs = [
        "test/f32-vsub-minmax.cc",
        "test/vbinary-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vsubc_minmax_test",
    srcs = [
        "test/f32-vsubc-minmax.cc",
        "test/vbinaryc-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "f32_vrsubc_minmax_test",
    srcs = [
        "test/f32-vrsubc-minmax.cc",
        "test/vbinaryc-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "q8_avgpool_minmax_test",
    srcs = [
        "test/q8-avgpool-minmax.cc",
        "test/avgpool-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "q8_igemm_minmax_test",
    srcs = [
        "test/q8-igemm-minmax.cc",
        "test/gemm-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "q8_dwconv_minmax_test",
    srcs = [
        "test/q8-dwconv-minmax.cc",
        "test/dwconv-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "q8_gavgpool_minmax_test",
    srcs = [
        "test/q8-gavgpool-minmax.cc",
        "test/gavgpool-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "q8_gemm_minmax_test",
    srcs = [
        "test/q8-gemm-minmax.cc",
        "test/gemm-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + WEIGHTS_PACK_HDRS + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "q8_vadd_minmax_test",
    srcs = [
        "test/q8-vadd-minmax.cc",
        "test/vadd-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "u8_clamp_test",
    srcs = [
        "test/u8-clamp.cc",
        "test/clamp-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "u8_lut32norm_test",
    srcs = [
        "test/u8-lut32norm.cc",
        "test/lut-norm-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "u8_maxpool_minmax_test",
    srcs = [
        "test/u8-maxpool-minmax.cc",
        "test/maxpool-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "u8_rmax_test",
    srcs = [
        "test/u8-rmax.cc",
        "test/rmax-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "x32_packx_test",
    srcs = [
        "test/x32-packx.cc",
        "test/pack-microkernel-tester.h",
        "src/xnnpack/AlignedAllocator.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "x32_pad_test",
    srcs = [
        "test/x32-pad.cc",
        "test/pad-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "x32_unpool_test",
    srcs = [
        "test/x32-unpool.cc",
        "test/unpool-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "x32_zip_test",
    srcs = [
        "test/x32-zip.cc",
        "test/zip-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "x8_lut_test",
    srcs = [
        "test/x8-lut.cc",
        "test/lut-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "x8_zip_test",
    srcs = [
        "test/x8-zip.cc",
        "test/zip-microkernel-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

xnnpack_unit_test(
    name = "requantization_test",
    srcs = [
        "src/xnnpack/requantization-stubs.h",
        "test/requantization.cc",
        "test/requantization-tester.h",
    ] + MICROKERNEL_TEST_HDRS,
    deps = MICROKERNEL_TEST_DEPS,
)

########################## Size tests for the library #########################

xnnpack_binary(
    name = "operator_size_test",
    srcs = ["test/operator-size.c"],
    deps = [":xnnpack_operators_nhwc_f32"],
)

xnnpack_binary(
    name = "subgraph_size_test",
    srcs = ["test/subgraph-size.c"],
    deps = [":XNNPACK"],
)

########################### Unit tests for operators ##########################

xnnpack_unit_test(
    name = "add_nc_test",
    srcs = [
        "test/add-nc.cc",
        "test/add-operator-tester.h",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "add_nd_test",
    srcs = [
        "test/add-nd.cc",
        "test/binary-elementwise-operator-tester.h",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "argmax_pooling_nhwc_test",
    srcs = [
        "test/argmax-pooling-nhwc.cc",
        "test/argmax-pooling-operator-tester.h",
    ] + OPERATOR_TEST_PARAMS_HDRS,
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "average_pooling_nhwc_test",
    srcs = [
        "test/average-pooling-nhwc.cc",
        "test/average-pooling-operator-tester.h",
    ] + OPERATOR_TEST_PARAMS_HDRS,
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "channel_pad_nc_test",
    srcs = [
        "test/channel-pad-nc.cc",
        "test/channel-pad-operator-tester.h",
    ] + OPERATOR_TEST_PARAMS_HDRS,
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "channel_shuffle_nc_test",
    srcs = [
        "test/channel-shuffle-nc.cc",
        "test/channel-shuffle-operator-tester.h",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "clamp_nc_test",
    srcs = [
        "test/clamp-nc.cc",
        "test/clamp-operator-tester.h",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "convolution_nhwc_test",
    srcs = [
        "test/convolution-nhwc.cc",
        "test/convolution-operator-tester.h",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "convolution_nchw_test",
    srcs = [
        "test/convolution-nchw.cc",
        "test/convolution-operator-tester.h",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "deconvolution_nhwc_test",
    srcs = [
        "test/deconvolution-nhwc.cc",
        "test/deconvolution-operator-tester.h",
    ] + OPERATOR_TEST_PARAMS_HDRS,
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "divide_nd_test",
    srcs = [
        "test/binary-elementwise-operator-tester.h",
        "test/divide-nd.cc",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "fully_connected_nc_test",
    srcs = [
        "test/fully-connected-nc.cc",
        "test/fully-connected-operator-tester.h",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "global_average_pooling_nwc_test",
    srcs = [
        "test/global-average-pooling-nwc.cc",
        "test/global-average-pooling-operator-tester.h",
    ] + OPERATOR_TEST_PARAMS_HDRS,
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "global_average_pooling_ncw_test",
    srcs = [
        "test/global-average-pooling-ncw.cc",
        "test/global-average-pooling-operator-tester.h",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "hardswish_nc_test",
    srcs = [
        "test/hardswish-nc.cc",
        "test/hardswish-operator-tester.h",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "leaky_relu_nc_test",
    srcs = [
        "test/leaky-relu-nc.cc",
        "test/leaky-relu-operator-tester.h",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "max_pooling_nhwc_test",
    srcs = [
        "test/max-pooling-nhwc.cc",
        "test/max-pooling-operator-tester.h",
    ] + OPERATOR_TEST_PARAMS_HDRS,
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "maximum_nd_test",
    srcs = [
        "test/binary-elementwise-operator-tester.h",
        "test/maximum-nd.cc",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "minimum_nd_test",
    srcs = [
        "test/binary-elementwise-operator-tester.h",
        "test/minimum-nd.cc",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "multiply_nd_test",
    srcs = [
        "test/binary-elementwise-operator-tester.h",
        "test/multiply-nd.cc",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "prelu_nc_test",
    srcs = [
        "test/prelu-nc.cc",
        "test/prelu-operator-tester.h",
    ] + OPERATOR_TEST_PARAMS_HDRS,
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "resize_bilinear_nhwc_test",
    srcs = [
        "test/resize-bilinear-nhwc.cc",
        "test/resize-bilinear-operator-tester.h",
    ] + OPERATOR_TEST_PARAMS_HDRS,
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "sigmoid_nc_test",
    srcs = [
        "test/sigmoid-nc.cc",
        "test/sigmoid-operator-tester.h",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "softmax_nc_test",
    srcs = [
        "test/softmax-nc.cc",
        "test/softmax-operator-tester.h",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "subtract_nd_test",
    srcs = [
        "test/binary-elementwise-operator-tester.h",
        "test/subtract-nd.cc",
    ],
    deps = OPERATOR_TEST_DEPS,
)

xnnpack_unit_test(
    name = "unpooling_nhwc_test",
    srcs = [
        "test/unpooling-nhwc.cc",
        "test/unpooling-operator-tester.h",
    ],
    deps = OPERATOR_TEST_DEPS,
)

############################# Build configurations #############################

# Enables usage of assembly kernels.
config_setting(
    name = "xnn_enable_assembly_explicit_true",
    define_values = {"xnn_enable_assembly": "true"},
)

# Disables usage of assembly kernels.
config_setting(
    name = "xnn_enable_assembly_explicit_false",
    define_values = {"xnn_enable_assembly": "false"},
)

# Disables usage of HMP-aware optimizations.
config_setting(
    name = "xnn_enable_hmp_explicit_false",
    define_values = {"xnn_enable_hmp": "false"},
)

# Builds with -c dbg
config_setting(
    name = "debug_build",
    values = {
        "compilation_mode": "dbg",
    },
)

# Builds with -c opt
config_setting(
    name = "optimized_build",
    values = {
        "compilation_mode": "opt",
    },
)

config_setting(
    name = "linux_k8",
    values = {"cpu": "k8"},
)

config_setting(
    name = "linux_aarch64",
    values = {"cpu": "aarch64"},
)

config_setting(
    name = "android",
    values = {"crosstool_top": "//external:android/crosstool"},
)

config_setting(
    name = "android_armv7",
    values = {
        "crosstool_top": "//external:android/crosstool",
        "cpu": "armeabi-v7a",
    },
)

config_setting(
    name = "android_arm64",
    values = {
        "crosstool_top": "//external:android/crosstool",
        "cpu": "arm64-v8a",
    },
)

config_setting(
    name = "android_x86",
    values = {
        "crosstool_top": "//external:android/crosstool",
        "cpu": "x86",
    },
)

config_setting(
    name = "android_x86_64",
    values = {
        "crosstool_top": "//external:android/crosstool",
        "cpu": "x86_64",
    },
)

config_setting(
    name = "windows_x86",
    values = {"cpu": "win_x86"},
)

config_setting(
    name = "windows_x86_64",
    values = {"cpu": "win_x64"},
)

config_setting(
    name = "macos_x86_64",
    values = {
        "apple_platform_type": "macos",
        "cpu": "darwin",
    },
)

config_setting(
    name = "emscripten",
    values = {"cpu": "js"},
)

config_setting(
    name = "emscripten_wasm",
    values = {
        "cpu": "js",
    },
)

config_setting(
    name = "emscripten_wasmsimd",
    values = {
        "cpu": "js",
        "features": "wasm_simd",
    },
)

config_setting(
    name = "emscripten_asmjs",
    values = {
        "cpu": "asmjs",
    },
)

config_setting(
    name = "ios_armv7",
    values = {
        "apple_platform_type": "ios",
        "cpu": "ios_armv7",
    },
)

config_setting(
    name = "ios_arm64",
    values = {
        "apple_platform_type": "ios",
        "cpu": "ios_arm64",
    },
)

config_setting(
    name = "ios_arm64e",
    values = {
        "apple_platform_type": "ios",
        "cpu": "ios_arm64e",
    },
)

config_setting(
    name = "ios_x86",
    values = {
        "apple_platform_type": "ios",
        "cpu": "ios_i386",
    },
)

config_setting(
    name = "ios_x86_64",
    values = {
        "apple_platform_type": "ios",
        "cpu": "ios_x86_64",
    },
)

config_setting(
    name = "watchos_armv7k",
    values = {
        "apple_platform_type": "watchos",
        "cpu": "watchos_armv7k",
    },
)

config_setting(
    name = "watchos_arm64_32",
    values = {
        "apple_platform_type": "watchos",
        "cpu": "watchos_arm64_32",
    },
)

config_setting(
    name = "watchos_x86",
    values = {
        "apple_platform_type": "watchos",
        "cpu": "watchos_i386",
    },
)

config_setting(
    name = "watchos_x86_64",
    values = {
        "apple_platform_type": "watchos",
        "cpu": "watchos_x86_64",
    },
)

config_setting(
    name = "tvos_arm64",
    values = {
        "apple_platform_type": "tvos",
        "cpu": "tvos_arm64",
    },
)

config_setting(
    name = "tvos_x86_64",
    values = {
        "apple_platform_type": "tvos",
        "cpu": "tvos_x86_64",
    },
)
