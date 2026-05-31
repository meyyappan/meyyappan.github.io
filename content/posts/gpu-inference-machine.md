---
title: "Building a GPU inference machine from an old Dell Optiplex"
date: 2026-05-30
draft: false
tags: ["hardware", "gpu", "inference", "linux"]
description: "Repurposing a Dell Optiplex 5070 for local GPU inference with an RTX 5060 Ti — two annoying problems and how I solved them."
---

I had an old Dell Optiplex 5070 sitting around and wanted to turn it into a local inference machine. Picked up an RTX 5060 Ti. Two problems came up, both with non-obvious fixes.

## Problem 1: The GPU doesn't fit

The Optiplex 5070 is a small form factor machine. The 5060 Ti does not fit inside the case — it's too long and too tall.

**Fix:** PCIe riser cable. Pull the GPU out of the case entirely, connect it to the motherboard via a riser, and let it sit externally. Looks janky, works fine.

## Problem 2: Ubuntu doesn't detect the GPU

After booting, `nvidia-smi` returned nothing. The GPU was invisible to the OS.

The issue: the motherboard wasn't allocating memory address space to the discrete GPU — the kind of BAR (Base Address Register) allocation that lets the CPU and OS actually talk to the card. This is a known issue with small form factor and business-class machines that weren't designed with discrete GPUs in mind.

I went down a rabbit hole of BIOS settings before finding the actual fix.

**Fix:** plug the monitor into the HDMI port on the RTX 5060 Ti directly, not the onboard graphics port on the motherboard.

When the display is connected to the onboard port, the system initializes the integrated GPU as primary and the discrete card never gets properly initialized. Switching to the GPU's own HDMI output forces the system to bring it up correctly — after which `nvidia-smi` worked immediately and the card was fully accessible for inference.

## End result

Cheap local inference machine from hardware I already had. The riser setup is ugly but the machine lives in a corner so it doesn't matter.

If you're doing the same build, save yourself an hour: plug into the GPU first.
