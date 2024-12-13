---
title: Acoustic Localization
date: 2022-09-01 00:00:00 +0800
categories: [Projects, Robotics]
tags: [acoustics, localization]     # TAG names should always be lowercase
subsection: "SAUVC"
author: <author_id>
mermaid: true
pin: false
image: /assets/images/ldr.png
---

Localization is required to complete the target acquisition and re-acquisition task in the challenge. The pingers fitted to the drums act as an acoustic source. A 2/3/4 acoustic sensor setup can be used for localization. There are different localization techniques, such as received signal strength indication (RSSI), angle of arrival (AOA), time difference of arrival (TDOA), and time of arrival (TOA), of which different methods are used in various applications.

The angle of arrival estimates is found by using TDOA, and the far-field approach is followed for simplicity.

**Time difference of arrival estimation:**

![Untitled](/assets/images/SAUVC%20f5d93874f979458ca1e3985d07e7da5b/Untitled%202.png)

- We get a hyperbola with two possible locations with two hydrophones, and with four hydrophones, it is easier to find the location of the source.
- The time difference between the two microphones is computed and used in estimating the position of the acoustic source.

**Finding the time difference:**

The following methods are widely used, with each having its pros and cons:

- **Normalized frequency-based TDOA:** This method can be used when the source is of a single frequency(and the value is known). We convert the obtained signals into the frequency domain and use the phase difference to find the time difference, and this method is prone to noise.
- **GCC-PHAT:** This is based on the GCC algorithm. In order to compute the TDOA between the reference channel and any other channel for any given segment, it is usual to estimate it as the delay that causes the cross-correlation between the two signal segments to be maximum. This method is less prone to noise but is computationally expensive.

**Hilbert transformation:**

This method works well for narrow-band signals and can be simplified using FFT to reduce computational costs and gives good results when compared to cross-correlation methods.