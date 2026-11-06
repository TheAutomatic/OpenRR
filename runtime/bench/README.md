# OpenRR Standalone Steady + DRS Benchmark & Bit-Exact Parity Harness

Single-file D3D12 benchmark and bit-exact FP16 output comparator (`openrr_bench.cpp`) used to measure steady-state GPU time, Dynamic Resolution Scaling (DRS) GPU time, feature/process VRAM delta, and 100% bit-exact output parity (`max_ulp_diff == 0`) between a baseline `OpenNrRrRuntime.dll` and a candidate `OpenNrRrRuntime.dll`.

## Build (MSVC x64)

```bat
cl /nologo /std:c++20 /EHsc /O2 /W4 /utf-8 /I runtime\include runtime\bench\openrr_bench.cpp /Fe:openrr_bench.exe /link d3d12.lib dxgi.lib
```

## Run (1440p Quality `1706x960 -> 2560x1440`, Steady 120 frames + DRS `1450x816 <-> 1706x960` 64 frames)

```bat
:: Legacy mode (--fast-mode 0) with bit-exact parity check against a baseline DLL:
openrr_bench.exe --baseline-runtime <path\to\baseline\OpenNrRrRuntime.dll> --runtime <path\to\candidate\OpenNrRrRuntime.dll> --data-dir <path\to\opennr-rr> --dlssd <path\to\nvngx_dlssd.dll> --out out\bench-legacy --fast-mode 0

:: New / Fast mode (--fast-mode 1):
openrr_bench.exe --baseline-runtime <path\to\baseline\OpenNrRrRuntime.dll> --runtime <path\to\candidate\OpenNrRrRuntime.dll> --data-dir <path\to\opennr-rr> --dlssd <path\to\nvngx_dlssd.dll> --out out\bench-new --fast-mode 1

:: 4K Quality (2560x1440 -> 3840x2160):
openrr_bench.exe --runtime <path\to\candidate\OpenNrRrRuntime.dll> --data-dir <path\to\opennr-rr> --dlssd <path\to\nvngx_dlssd.dll> --out out\bench-4k --rw 2560 --rh 1440 --ow 3840 --oh 2160 --fast-mode 1
```
