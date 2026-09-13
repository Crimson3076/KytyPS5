# Explicit-LOD gather experiment

This branch includes the buffer fix and an experimental 2D IMAGE_GATHER4_L (opcode 0x44) lowering. LLVM AMDGPU MIMGInstructions.td identifies this opcode. The exact three instruction words from the SpongeBob crash are included in a compiler/SPIR-V regression test.

The portable lowering queries mip dimensions and samples four texel centers with explicit LOD, preserving gather component ordering. It avoids requiring the AMD texture-gather-LOD Vulkan extension on NVIDIA.

This is not ready for an upstream PR. Visual equivalence, sampler min/max LOD clamping, anisotropic filtering, mip rounding at ties, and texel-center precision still require validation. The fallback rounds and clamps LOD to image levels, but host sampler clamps may further alter sampling. Only non-comparison 2D images are supported; other dimensions fail explicitly. Four samples can cost more than one native gather. The compiled build needs in-game testing. Do not describe this as complete instruction emulation.

AI disclosure: Codex authored this experimental implementation, tests, and build changes at the contributor's request. Human review is pending.
