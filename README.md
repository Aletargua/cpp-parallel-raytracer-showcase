# cpp-parallel-raytracer-showcase
High-performance 3D rendering engine implemented in C++23. Focuses on hardware-aware optimizations, including Structure of Arrays (SOA) data layouts and massive parallelization using Intel OneAPI TBB. Features a containerized development environment and a comprehensive unit testing suite.

High-Performance Parallel Ray Tracer (C++23 and OneTBB)
This repository serves as a technical showcase for a 3D rendering engine optimized for high-concurrency environments. Developed at Universidad Carlos III de Madrid (UC3M), the project focuses on the intersection of mathematical rigor and low-level system optimization.

Academic Integrity Disclaimer: To comply with university regulations, the full source code is maintained in a private repository. This showcase highlights architectural decisions, performance benchmarks, and specific implementation patterns.

Architecture and Modern C++ Standards
The engine is built using C++23 to ensure a robust and extensible foundation while maintaining high execution speeds.

Static Polymorphism and Type Safety
The shading system avoids traditional Object-Oriented hierarchies and the associated virtual table (vtable) overhead by utilizing static polymorphism.

Variant-Based Materials: The material system (Matte, Metal, Refractive) is implemented using std::variant and std::visit.

Compiler Optimization: This approach allows the compiler to inline shading logic more effectively and eliminates dynamic dispatch during the recursive ray-tracing process.

Flexible Command Dispatching
Input processing is handled by a decoupled Parser designed for scalability.

Dispatch Mapping: A std::map<std::string, ConfigParserFunc> acts as a centralized dispatcher for scene entities.

Decoupled Logic: This design ensures that adding new geometry or material types does not require modifications to the core parsing loop.

High Performance and Hardware-Aware Parallelism
The parallel version of the engine features a complete redesign of the data layout to maximize hardware efficiency and thread utilization.

AOS to SOA Transformation
To optimize memory bandwidth and cache hit rates, the world representation was migrated from an Array of Structures (AOS) to a Structure of Arrays (SOA) layout.

Data Locality: Storing geometric properties in contiguous memory vectors improves the efficiency of the CPU cache and branch predictor.

SIMD Alignment: The SOA layout provides a foundation for future SIMD (Single Instruction, Multiple Data) vectorization of intersection algorithms.

Concurrency with Intel OneTBB
The rendering pipeline leverages Intel Threading Building Blocks (TBB) to scale across high-core-count processors.

Thread-Local Storage (TLS): To eliminate mutex contention and data races, the implementation uses tbb::enumerable_thread_specific<std::mt19937_64> for random number generation.

Dynamic Load Balancing: The system utilizes auto_partitioner and configurable grain_size to maintain an even workload distribution across all available threads.

HPC Benchmarking: Performance was validated on the Cluster Avignon (High-Performance Computing environment), demonstrating near-linear scalability up to 256 threads.

Infrastructure and Quality Assurance
Professional software engineering requires a focus on reliability, reproducibility, and rigorous verification.

Reproducible Development Environments
Containerized Toolchain: A custom Ubuntu 24.04 Docker image provides a consistent development environment across different machines.

Modern Stack: The infrastructure includes GCC 14, Clang 20, and CMake 3.30+ to utilize advanced optimization flags such as -march=native and -ffast-math.

Comprehensive Testing Suite
An extensive suite of unit tests was developed using GoogleTest to ensure the correctness of all core components.

Geometric Verification: Specific tests validate the intersection logic and surface normal calculations for spheres and cylinders.

RNG Determinism: Testing ensures that the anti-aliasing jitter remains deterministic for a given seed, which is critical for regression testing in stochastic systems.

Error Handling: Usage of ASSERT_DEATH verifies that the engine fails gracefully and provides informative feedback when processing malformed input files.
