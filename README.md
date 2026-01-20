# Man-Machine-Interface
Full cyberisation of an individual starting from software to control the biomechanical parts of the body with infinite precision.

---------- 2026 ----------

**MechOptimizer**—which sounds like it sits at that intersection of critical infrastructure and machine-man symbiosis - definitely want a clean build.

Given the hardware-interfacing nature of work, the **CMake** route is almost always superior because it handles cross-compilation and library linking (like `libusb` or `GL` headers) much more gracefully than a static Makefile.

### The "Researcher" Build Pattern

To keep my environment clean and ensure the binary is optimized for the machine you're running it on, use this workflow:

```bash
mkdir -p build && cd build

# -DCMAKE_BUILD_TYPE=Release ensures the compiler optimizes for performance
# -march=native tells the compiler to use all instructions available on your CPU
cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_FLAGS="-march=native"

# Use all available cores to speed up the compile
make -j$(nproc)

```

### Why both files exist in "MechOptimizer"

If I’ve inherited or am refactoring this repo, here is likely why both are present:

1. **The Makefile is a "Phony" Wrapper:** Developers often put a `Makefile` in the root that just contains `all: ; mkdir -p build && cd build && cmake .. && make`. It's a convenience for users who just want to type `make`.
2. **Cross-Platform Parity:** `CMakeLists.txt` is there to generate project files for Ninja, Visual Studio, or Xcode, while the `Makefile` is kept for legacy CI/CD pipelines.

---

### A Warning for "Man-Machine" Merging

If **MechOptimizer** is meant to interface with low-level hardware or biometric/cybernetic data, check the `CMakeLists.txt` for these specific flags:

* **`find_package(Threads REQUIRED)`**: Essential if you're doing real-time optimization.
* **`target_link_libraries(...)`**: See if it's linking against `libusb` or `boost`. If those are missing on your system, `cmake ..` will fail early and tell you exactly what headers you need to `apt install`.
