---
title: Thruster Control of AUV Using LQR
date: 2022-08-12 00:00:00 +0800
categories: [Projects, ML]
tags: [ml, dl, cnn, computer vision, python]     # TAG names should always be lowercase
author: <author_id>
mermaid: true
pin: true
image: /assets/images/ldr.png
---

# **Abstract**

To make two different control system models in Simulink on PID and LQR controllers respectively and to get the results for positional and velocity parameters of the ROV based on the desired reference inputs. To tune parameters related to both the controllers to achieve better results and finally test the model physically underwater.

# **A. PID Controller:**

## **Literature Review:**

The basic mathematical model of an ROV is given by:

[https://lh5.googleusercontent.com/f732jKKqU0sQ-L3hI7nzV9pkejI-N_KJzSlpck1j-8G78UBEz02emNbbNOaa9dHmj1DlouLYxFkupfeiIgAXsxU6bjW4isNogCZY4LUTOR7Z2fJPdPPyS1ckTAaah4BHRlkV2j39uKVRdsknqtUfm1J3s6zIZDc3hHxPZk24H5hvuta9x-K9kpHKfaDhBLp4](https://lh5.googleusercontent.com/f732jKKqU0sQ-L3hI7nzV9pkejI-N_KJzSlpck1j-8G78UBEz02emNbbNOaa9dHmj1DlouLYxFkupfeiIgAXsxU6bjW4isNogCZY4LUTOR7Z2fJPdPPyS1ckTAaah4BHRlkV2j39uKVRdsknqtUfm1J3s6zIZDc3hHxPZk24H5hvuta9x-K9kpHKfaDhBLp4)

- where M = Mass matrix (with added mass effect taken into account)
- C = Coriolis force matrix
- D = Damping forces matrix
- g = Restoring forces matrix
- 𝜏 = Forces & moments matrix

**1. Added Mass Effect:**

The added mass of an object is the effect in which some mass of fluid surrounding the object under observation is accelerated/decelerated along with it.

**2. Coriolis Effect:**

The Coriolis force is a fictitious force that comes into play whenever we are trying to explain the forces on an object with respect to a rotating frame.

**3. Ziegler-Nichols Method for tuning PIDs:**

This method can be used to tune our PID both in the case where we have a working model for our plant or even when we don’t. The first step to this method is measuring two parameters: KU which is the gain at which the system becomes marginally stable and TU which is the period of oscillation at marginal system response. These values are found by taking KI and KD values to be zero for that input and changing KP until marginal stability is achieved.

After these parameters are evaluated controller gains can simply be calculated from the below table:

[https://lh3.googleusercontent.com/x4qDb3pw76i-ixfjEA6FAaInJJF0AxObdt6DT46yW63y-4M-oIByjqmyDsVnDq7_kwAjyKpHOPVwgsDV5KZelNsnQS0hqlqNvhuQmUUmBr7y4lnBwrfhN9dxghBemz6UCcNYra4NyPftVzghL17_PoOG07qVhc6V7SOeMlIrcHuLP22IC0d08tXjrCIjeJeg](https://lh3.googleusercontent.com/x4qDb3pw76i-ixfjEA6FAaInJJF0AxObdt6DT46yW63y-4M-oIByjqmyDsVnDq7_kwAjyKpHOPVwgsDV5KZelNsnQS0hqlqNvhuQmUUmBr7y4lnBwrfhN9dxghBemz6UCcNYra4NyPftVzghL17_PoOG07qVhc6V7SOeMlIrcHuLP22IC0d08tXjrCIjeJeg)

**4. Controller Models:**

The two controller models that we used are:

**A. Linear Model:**

The schematic of the simulink model created using the linear PID control looks like this:

[https://lh6.googleusercontent.com/H7puU2R5FNvzsoKLzcFbx_A9M-OnbAA1LVBKhEbbL1C2IFmU5i0fQxHvcKHzBMsH9-uatMikxOKpF8A0HgZrCEDr_XZ5s7vRPwwcxK0CiRFOic_eu9ClLXsRQNT7t1EayoJ0k9jT07J7rF1B9uRx9AdxJygbBLwHBwg8yVQP_XwYZnq7k4FPBMDhdlsvVX58](https://lh6.googleusercontent.com/H7puU2R5FNvzsoKLzcFbx_A9M-OnbAA1LVBKhEbbL1C2IFmU5i0fQxHvcKHzBMsH9-uatMikxOKpF8A0HgZrCEDr_XZ5s7vRPwwcxK0CiRFOic_eu9ClLXsRQNT7t1EayoJ0k9jT07J7rF1B9uRx9AdxJygbBLwHBwg8yVQP_XwYZnq7k4FPBMDhdlsvVX58)

It consists of different functionalities in each block of the design:

[https://lh6.googleusercontent.com/Uc1l7vAedpjweCnDhSzGxCqId5P7HCWmxceqYBvLJoQ02VikpA3HB9YLUufk67O4AiVFv95kUmuDKL84kNGlepY9b3WCqtxAkWCTQSbNpNB4I5jUYXlk-V0fZ2xepA_qG2lffDkmXTf1vI8nLhEG2xPT3UCr7a8OupKhCLEgQBMUcu1AKOw6a8GCV5Pf0HZH](https://lh6.googleusercontent.com/Uc1l7vAedpjweCnDhSzGxCqId5P7HCWmxceqYBvLJoQ02VikpA3HB9YLUufk67O4AiVFv95kUmuDKL84kNGlepY9b3WCqtxAkWCTQSbNpNB4I5jUYXlk-V0fZ2xepA_qG2lffDkmXTf1vI8nLhEG2xPT3UCr7a8OupKhCLEgQBMUcu1AKOw6a8GCV5Pf0HZH)

The PID controller uses an error signal, called the tracking error, generated from the difference between the desired position and the current position of the rover.

**𝑒 = 𝜂d − 𝜂**

This error signal, in the world frame, is converted to the error signal in body frame 𝑒b using the following equation:

**𝑒*b* = 𝐽*T*(𝜂) 𝑒**

where the transformation matrix from the vehicle body frame to the world reference frame using Euler angle transformation is given by:

[https://lh5.googleusercontent.com/TF7BLYGp8JbT91e50N6_1ZJDjMCuS_ihNXbCblXF8d4RzxitUC7s71-_u5Y63vMhmAAJxq7b3ANmi7RvwCJCdV6F3n2DVGRgVGL-0u5y3SuIAJKlZQdFanYrb7mGIgEjM-VADZqA7doDmzQyDB-tFf0CEUGEf8m0TCPZSET-LtcnqEvtxAS2xOu80SZLpm5x](https://lh5.googleusercontent.com/TF7BLYGp8JbT91e50N6_1ZJDjMCuS_ihNXbCblXF8d4RzxitUC7s71-_u5Y63vMhmAAJxq7b3ANmi7RvwCJCdV6F3n2DVGRgVGL-0u5y3SuIAJKlZQdFanYrb7mGIgEjM-VADZqA7doDmzQyDB-tFf0CEUGEf8m0TCPZSET-LtcnqEvtxAS2xOu80SZLpm5x)

[https://lh6.googleusercontent.com/qmmoOLOF5xdWJ_jhNZgYMDKGyFxDw5-rfMZWUXWbQilT8HDzD8lEAO5eFaxhdTkooF9qDDvnebaibIilkZRiEx68kmfvFuAOqJ0e7y9q5L8DPCxLSecFyH9fxoQDttF2cAXuk9v1JcdTc5onRbuQAeRHkCTDLOgCIdpIICr0r8t2yFCytVwGS-3rNgO0DBzE](https://lh6.googleusercontent.com/qmmoOLOF5xdWJ_jhNZgYMDKGyFxDw5-rfMZWUXWbQilT8HDzD8lEAO5eFaxhdTkooF9qDDvnebaibIilkZRiEx68kmfvFuAOqJ0e7y9q5L8DPCxLSecFyH9fxoQDttF2cAXuk9v1JcdTc5onRbuQAeRHkCTDLOgCIdpIICr0r8t2yFCytVwGS-3rNgO0DBzE)

Using the error signal in the body frame 𝑒b, the torque generated by the PID can be calculated using the equation:

[https://lh3.googleusercontent.com/PrkABt-hrS8uVBHbxF4Y4yvV7vQrrC_NdTJZvS-FsAuNv4WnZI03Bok990hwzOFWioTttU0hlZBAMa-m6pVLGkzZeAEue_7ffd1rOeFdVWLcfcCOJ2S-lvD0cFjv6WZ_Q1VRESrpGhbqBqQyrbc9goxUmwvWsaXV_FG5H4e-smv0VILLmOb0frpzpmvAeeey](https://lh3.googleusercontent.com/PrkABt-hrS8uVBHbxF4Y4yvV7vQrrC_NdTJZvS-FsAuNv4WnZI03Bok990hwzOFWioTttU0hlZBAMa-m6pVLGkzZeAEue_7ffd1rOeFdVWLcfcCOJ2S-lvD0cFjv6WZ_Q1VRESrpGhbqBqQyrbc9goxUmwvWsaXV_FG5H4e-smv0VILLmOb0frpzpmvAeeey)

In order to generalise the required control forces, control allocation calculates the control input signal u to apply to the thrusters. The control forces due to the control inputs applied to the thrusters can be expressed as:

**𝜏 = 𝑇(𝛼)𝐹 = 𝑇(𝛼)𝐾*u***

As a result, the control input vector can be derived as:

[https://lh3.googleusercontent.com/1l9NOLTQYEiVTpCVumQ-e3ecXnvK7Pr1TzCj0EAdsOBDE4Nh5r8ZkcO7hiKFU2yo4wWufT8_Q3ZMAPlOfUImBiBDT_AMBpMVsgHaPj-I4C-_IKhALFiQVLbejnmB101KzI62NdfnvkmtUVpR0n80qHtszGMi6isteyBluLrQGPE3PMEyzya0lE97Zv5n3iR6](https://lh3.googleusercontent.com/1l9NOLTQYEiVTpCVumQ-e3ecXnvK7Pr1TzCj0EAdsOBDE4Nh5r8ZkcO7hiKFU2yo4wWufT8_Q3ZMAPlOfUImBiBDT_AMBpMVsgHaPj-I4C-_IKhALFiQVLbejnmB101KzI62NdfnvkmtUVpR0n80qHtszGMi6isteyBluLrQGPE3PMEyzya0lE97Zv5n3iR6)

For the linear model, the following values of KP, KI, and KD have been obtained using the Ziegler-Nichols method:

| 6-DoF PID | Surge | Sway | Heave | Roll | Pitch | Yaw |
| --- | --- | --- | --- | --- | --- | --- |
| KP | 3 | 3 | 3 | 4 | 4 | 2 |
| KI | 0.2 | 0.2 | 0.2 | 0.3 | 0.3 | 0.1 |
| KD | 2.5 | 2.5 | 0.5 | 0.5 | 1 | 0.5 |

[https://lh4.googleusercontent.com/M4hGVWy-Krdu04qYcd9rZRlg1ZaQ361U7ZFfcq3Ii4RGHl3Zs13KUFXuIfn0uerkVGYUz2r-Sl7ShQcFaw9MqOxOh4k3koOXZ6_isekpryfNqODPEO-R2nAsBzk6uJ6CbdyisJV_UCyyGkt1CS6wnodHaLjF7VZlypfT-lz2briRQZFqTaLqzpNZ2NxBngo_](https://lh4.googleusercontent.com/M4hGVWy-Krdu04qYcd9rZRlg1ZaQ361U7ZFfcq3Ii4RGHl3Zs13KUFXuIfn0uerkVGYUz2r-Sl7ShQcFaw9MqOxOh4k3koOXZ6_isekpryfNqODPEO-R2nAsBzk6uJ6CbdyisJV_UCyyGkt1CS6wnodHaLjF7VZlypfT-lz2briRQZFqTaLqzpNZ2NxBngo_)

Using the control input vector, the thruster system generates the control forces in 6 DoFs with the help of the above mentioned equation:

**𝜏 = 𝑇(𝛼)𝐹 = 𝑇(𝛼)𝐾*u***

Since the 8 thrusters of the ROV produce a maximum thrust of 40N at operating voltage of 16V, the thrust coefficients are approximated to 40. Thus the thrust coefficient matrix **𝐾** is taken as

**𝐾 = 𝑑𝑖𝑎𝑔[40, 40, 40, 40, 40, 40, 40, 40]**

After obtaining the torque vector, the kinetics is used to determine the acceleration in the body frame for the given forces using the state equation:

**𝑀𝜈̇ + 𝐶(𝜈)𝜈 + 𝐷(𝜈)𝜈 + 𝑔(𝜂) = 𝜏**

The kinematics block is then used to define the vehicle velocity in the world frame **𝑣*n***. The position of the vehicle is then determined in the integrator and, using inverse kinematics, is converted to the body frame before being supplied back into the controller.

**B. Non-Linear Model:**

Because of disturbances underwater like current speed, there will be non-linearities introduced into the system. This makes the linear-PID controller model inappropriate to use as there will be a lot of deviations from the desired o/p and noise in the system. So, we need to include the system dynamics to get the control input to the thrusters.

In the nonlinear model-based PID control system design, the dynamic model of the ROV is utilized to produce a 6-DoF predictive force and the model-based PID is used to provide a corrective force in 6 DoFs to adjust the error in the model. This is advantageous in that the model error and nonlinearities tend to be smaller than the dynamics themselves.

In the predictive force generation, a virtual reference trajectory strategy is introduced for the design of trajectory tracking. With the use of a scalar measure of tracking in Fossen (Fossen, 1994), a virtual reference 𝑥𝑟 can be defined that satisfies:

𝑥𝑟_dot = 𝑥𝑑̇ _dot+ 𝜆𝑒𝑏

where 𝜆 > 0 is the control bandwidth that describes the amount of tracking error to the overall tracking performance, and 𝑒𝑏

is the tracking error in the body frame.

Since the velocity 𝑣 is the time derivative of the position (i.e. 𝑣 = 𝜂̇), for a defined virtual reference position 𝜂𝑟, the following is satisfied:

𝑣𝑟 = 𝑣𝑑 + 𝜆𝑒b

So, lambda(𝜆) is used to tune the 6-DoF predictive force.

This is shown in the following block:

[https://lh6.googleusercontent.com/rJA193nCqC7P2pN9uWob8Ta0vDHkXSqoNH1v0G_aUR3yiKvg6XiKMz5dGOx2Ou0D2-uwCk6sD45ZuWHiSTQrjPIblESmDmc7T9Pudycs5IMfnp2k_kEwySthp4DhgTdc022eBHu4Q1aXvvX47VSADWo0GtvAdmjFH1O7HlfuyuswWhXjvy2FlNZNMHVCgcr3](https://lh6.googleusercontent.com/rJA193nCqC7P2pN9uWob8Ta0vDHkXSqoNH1v0G_aUR3yiKvg6XiKMz5dGOx2Ou0D2-uwCk6sD45ZuWHiSTQrjPIblESmDmc7T9Pudycs5IMfnp2k_kEwySthp4DhgTdc022eBHu4Q1aXvvX47VSADWo0GtvAdmjFH1O7HlfuyuswWhXjvy2FlNZNMHVCgcr3)

Where ‘A’ is the new controller output.

The PID controller gains(Kp, Ki and Kd) for the non-linear model are found out by Ziegler Nichols method and were found out to be as follows:

[https://lh5.googleusercontent.com/fSpHEtjRfJABJhHYWseVfxkUHZKrjXnD35mJmltYFoMR1pD_7wPwuMbBGwWkDXVeWKt53fW1mfcsk9Oiasg6bxT9tc9xEFMaxI_DV6EbLx5Ha5-3t1YOH1vetlDTqRBix-aIhP6eIMBSn5x8sui5BQwrWW0kTCLNxtbYVpNOhCv4gn2qUGw80YXkxjBGF2b5](https://lh5.googleusercontent.com/fSpHEtjRfJABJhHYWseVfxkUHZKrjXnD35mJmltYFoMR1pD_7wPwuMbBGwWkDXVeWKt53fW1mfcsk9Oiasg6bxT9tc9xEFMaxI_DV6EbLx5Ha5-3t1YOH1vetlDTqRBix-aIhP6eIMBSn5x8sui5BQwrWW0kTCLNxtbYVpNOhCv4gn2qUGw80YXkxjBGF2b5)

Finally, the control law for the nonlinear model-based PID controller is computed given by:

[https://lh4.googleusercontent.com/OKTgfKfj4_GcszyavbrwxHhyTbaIOhCCmev3rB5DaKdHFpyitq7loZH4c4ST2GqfyFM98oWr5Fj2BaKlqgMjtnlFpWrI3tF12pnAWrbVQ4PctrKKTcYO4vZK0CICn8Pd-lqp89ELeBILLaKuyHYoOOie0iFVIAiaOsVxgQd5c6WDrLgbWg2WS5UPw-90RzYr](https://lh4.googleusercontent.com/OKTgfKfj4_GcszyavbrwxHhyTbaIOhCCmev3rB5DaKdHFpyitq7loZH4c4ST2GqfyFM98oWr5Fj2BaKlqgMjtnlFpWrI3tF12pnAWrbVQ4PctrKKTcYO4vZK0CICn8Pd-lqp89ELeBILLaKuyHYoOOie0iFVIAiaOsVxgQd5c6WDrLgbWg2WS5UPw-90RzYr)

The final model for this system is shown below:

[https://lh6.googleusercontent.com/wn-IiuVZ5R-j15YADtX8WGCb3A4OZSKzbE4ZbQH3JJl22IhvdiZUziwb0yO-Ly7OFiRdN8QlKE5cFMT9TucavaFy6xPy1a1iJioLuPiOV6CIxcqmdueFei7c3EfWWnW8Tf46dqUjT_NGVjY8edXq07OvWMttj84uF5UBN80JFI6wrReFO1zCeFtz7T7i2qNf](https://lh6.googleusercontent.com/wn-IiuVZ5R-j15YADtX8WGCb3A4OZSKzbE4ZbQH3JJl22IhvdiZUziwb0yO-Ly7OFiRdN8QlKE5cFMT9TucavaFy6xPy1a1iJioLuPiOV6CIxcqmdueFei7c3EfWWnW8Tf46dqUjT_NGVjY8edXq07OvWMttj84uF5UBN80JFI6wrReFO1zCeFtz7T7i2qNf)

Here, we took desired position input as [1; 1; 2; 0; 0; 0].

**Implementation:**

We implemented the models for both the controllers above and got the following results when we give a desired positional input:

**1) Linear Controller Model:**

Using the above method, we have obtained the following results for a desired position input: eta_d = [3;4;1;1.57;0;0].

The output of the position and orientation control is obtained as follows:

**Position X :**

*Desired output:* 3m

*Obtained result:*

[https://lh6.googleusercontent.com/K_2sAZy4_KQDNgiRuIlBsSR2cSIJkYM4O4VF3mybkO16wilQv8_szndqZSkvBKgQ8G9HEdD4WSi8fPTYbmJWH9w5gFA0D0SAM4bhI4TLZQnNqmCmxz95qlqoAzGO4IDkA2WHWR7XlPfeQTkHCDX_uefPv_aIer2ZjCaYOGdwSQV66V_NRVvKGkrT2GODS3Z6](https://lh6.googleusercontent.com/K_2sAZy4_KQDNgiRuIlBsSR2cSIJkYM4O4VF3mybkO16wilQv8_szndqZSkvBKgQ8G9HEdD4WSi8fPTYbmJWH9w5gFA0D0SAM4bhI4TLZQnNqmCmxz95qlqoAzGO4IDkA2WHWR7XlPfeQTkHCDX_uefPv_aIer2ZjCaYOGdwSQV66V_NRVvKGkrT2GODS3Z6)

The steady state response final value = 3.

**Position Y :**

*Desired output:* 4m

*Obtained result:*

[https://lh4.googleusercontent.com/X-SzWSpGYQqMe6mGpLf8hfszz2F7VnT4Z1YdcHyvL8by8fEi_VPvm7zwnwXQmskAsniY2_qHFUSupnk9RneU3ac18DPuwqNtTfHNXYyJmFqJJKNf5htlCFAk6d6OXAfDqztODFqJ6jboeKxZ5gLujhE1_z792OO0N2y5q4s1IuSIneDIpGUE869a1edvuzIo](https://lh4.googleusercontent.com/X-SzWSpGYQqMe6mGpLf8hfszz2F7VnT4Z1YdcHyvL8by8fEi_VPvm7zwnwXQmskAsniY2_qHFUSupnk9RneU3ac18DPuwqNtTfHNXYyJmFqJJKNf5htlCFAk6d6OXAfDqztODFqJ6jboeKxZ5gLujhE1_z792OO0N2y5q4s1IuSIneDIpGUE869a1edvuzIo)

The steady state response final value = 4.

**Position Z :**

*Desired output:* 1m

*Obtained result:*

[https://lh5.googleusercontent.com/MDcKSNROGP0uy4SY7AlhrUyU-iFvp7TuLpaGHDyV0Y57NQ8lGsf8t484TJ1qn9VM61jIsGwFVgPVBDpUxGkIB2GnqV34OleLYrto9a_jJVEDhlsvT4expircyicbZvH55lYtjazbtLgHfRPtXYM9EonrFWMDmkYhKOpDxXenrHmzsuJ9hdmXZ5n8HvQBWC2B](https://lh5.googleusercontent.com/MDcKSNROGP0uy4SY7AlhrUyU-iFvp7TuLpaGHDyV0Y57NQ8lGsf8t484TJ1qn9VM61jIsGwFVgPVBDpUxGkIB2GnqV34OleLYrto9a_jJVEDhlsvT4expircyicbZvH55lYtjazbtLgHfRPtXYM9EonrFWMDmkYhKOpDxXenrHmzsuJ9hdmXZ5n8HvQBWC2B)

The steady state response final value = 1.

**Orientation 𝛷 :**

*Desired output:* 1.57 radians

*Obtained result:*

[https://lh3.googleusercontent.com/n8cVoX9WVpBU3ioVd0hLMsE0Mkjcu04VZ2-0KX7E2IOwjgazbrJahLggnkAgzXSYzdVwKij1MaLsNHKbNM3AlB1_vsqbt6P3nbZ7J4aeBKI95eng72jwVHjQ1dfMH9QX3pI3FpoIp1XmoCQZpi_QxV5Z8TlSAsgeEu2A1K8-EqQZJbyTGqEac4suYcJ97i4k](https://lh3.googleusercontent.com/n8cVoX9WVpBU3ioVd0hLMsE0Mkjcu04VZ2-0KX7E2IOwjgazbrJahLggnkAgzXSYzdVwKij1MaLsNHKbNM3AlB1_vsqbt6P3nbZ7J4aeBKI95eng72jwVHjQ1dfMH9QX3pI3FpoIp1XmoCQZpi_QxV5Z8TlSAsgeEu2A1K8-EqQZJbyTGqEac4suYcJ97i4k)

The steady state response final value = 1.57.

**Orientation 𝛳 :**

*Desired output:* 0 radians

*Obtained result:*

[https://lh4.googleusercontent.com/iuVXYLnwryq7SqY2u6opZryQAeTqXMbRUBYxTmZ27ZS70Kr23R0iShKFIWFPoqNg59Z2VneOWKkIvp20qf9KB135XqJ-8Ils3WyQ2UQ0iHWY5WOvKm8S4yPM_si2vSphAdV8QfEx0HVbyrg_gAxsn5_TYktdBBSpeHt5j3m8jEJaEhW-cCd8tqvTZJ85u_yo](https://lh4.googleusercontent.com/iuVXYLnwryq7SqY2u6opZryQAeTqXMbRUBYxTmZ27ZS70Kr23R0iShKFIWFPoqNg59Z2VneOWKkIvp20qf9KB135XqJ-8Ils3WyQ2UQ0iHWY5WOvKm8S4yPM_si2vSphAdV8QfEx0HVbyrg_gAxsn5_TYktdBBSpeHt5j3m8jEJaEhW-cCd8tqvTZJ85u_yo)

The steady state response final value = 0.

**Orientation 𝛹 :**

*Desired output:* 0 radians

*Obtained result:*

[https://lh5.googleusercontent.com/E71QRwlxzuqe42D64AY3qI50LQFFALgOqWjlB0GFFNu0U0BHa5_Mb_K1cxduzVjgrw9CJJQbOhpgz0dLbWwk-2pdL7djbyKhKp3msiV1abon4E9sJ34Ri-k7wfS2bkoV3KcGTUTY2_4SMWST3FHPZv_asPteTOZtwK4kTsPZj2gpM4ttGtgD_sxZP6OmDkg2](https://lh5.googleusercontent.com/E71QRwlxzuqe42D64AY3qI50LQFFALgOqWjlB0GFFNu0U0BHa5_Mb_K1cxduzVjgrw9CJJQbOhpgz0dLbWwk-2pdL7djbyKhKp3msiV1abon4E9sJ34Ri-k7wfS2bkoV3KcGTUTY2_4SMWST3FHPZv_asPteTOZtwK4kTsPZj2gpM4ttGtgD_sxZP6OmDkg2)

The steady state response final value = 3.

The control inputs to the thrusters for the same are represented in the plots below:

[https://lh3.googleusercontent.com/-PVz8d06J48hlhXGkJHQJLQMS-pTatrUAReg2PXrD6cHxoaQpv1897ssICMXSKrsAl4kClRUaZsGyX1MdhTByvUN1vqYtHGj6GGNvU0IGK0oQ1pPoGlcq4xk9pPvJ3iawF3Vp34-iIXNQ9DUMIABIat5BRE8uQTreNdFGRb_GaVXsm_dsi-KQyA-v4WbU8uQ](https://lh3.googleusercontent.com/-PVz8d06J48hlhXGkJHQJLQMS-pTatrUAReg2PXrD6cHxoaQpv1897ssICMXSKrsAl4kClRUaZsGyX1MdhTByvUN1vqYtHGj6GGNvU0IGK0oQ1pPoGlcq4xk9pPvJ3iawF3Vp34-iIXNQ9DUMIABIat5BRE8uQTreNdFGRb_GaVXsm_dsi-KQyA-v4WbU8uQ)

As it is evident from the plots, each thruster is instructed to provide the required thrust every instant for a finite period of time after which the control input eventually settles down to a final value.

The model of the rover under study somewhat looks like this:

[https://lh5.googleusercontent.com/Vz25oUdmq34C2rE4aLSky9EezmM9NcfNCp_9QNXxzjt0wPr2TXVvj9q3yz32fLPn0zd4aKTfQ-17xy3PKKaE_pn68-dI5pRgCSwgyH3hKQTo_bn7j3Qopl_vd1bKVCJ3mjldUbYuEFLEtaaJ8KOmd321NePBeXJ1O5yLgoaGf2JInD8aJOdUwVJNoNvKaidQ](https://lh5.googleusercontent.com/Vz25oUdmq34C2rE4aLSky9EezmM9NcfNCp_9QNXxzjt0wPr2TXVvj9q3yz32fLPn0zd4aKTfQ-17xy3PKKaE_pn68-dI5pRgCSwgyH3hKQTo_bn7j3Qopl_vd1bKVCJ3mjldUbYuEFLEtaaJ8KOmd321NePBeXJ1O5yLgoaGf2JInD8aJOdUwVJNoNvKaidQ)

Here, the thrusters 1,2,3,4 are used in the position control in the X,Y directions while 5,6,7,8 are used for the Z-directional control. If we see the plots for the thruster control inputs for 1,2,3,4:

[https://lh5.googleusercontent.com/oFA3Z2IIM5aA6rJO4Hc6GxKdpKeWJY1ztC2c1-FpavCHFnjLZlQcAOUgRgjv24KhR_xEPgcsZgw-La1Ac8S1l5NwjDT7wqmM_qIt4qBTmpN2zmmTt5OY0bjDlhvuB7A8HSG1VcyZ0tKdqtdcEZw32av9e2oUiFFwtE1iowkphVkoRPIZJe1fesQjOP29se24](https://lh5.googleusercontent.com/oFA3Z2IIM5aA6rJO4Hc6GxKdpKeWJY1ztC2c1-FpavCHFnjLZlQcAOUgRgjv24KhR_xEPgcsZgw-La1Ac8S1l5NwjDT7wqmM_qIt4qBTmpN2zmmTt5OY0bjDlhvuB7A8HSG1VcyZ0tKdqtdcEZw32av9e2oUiFFwtE1iowkphVkoRPIZJe1fesQjOP29se24)

We can observe the steady state final value to be zero for all the thrusters 1,2,3,4.

***Reason:*** The above thrusters are used for controlling the position in X,Y directions and their respective control inputs drive them to work in a synchronised manner so as to reach the respective position. Once the rover has reached the required X,Y, coordinates of the position with the required orientation, there is no need for them to provide any more thrust given the lack of any external force acting in the X,Y directions.

The interesting part comes when we observe the control input plots for the thrusters 5,6,7,8:

[https://lh4.googleusercontent.com/smYUFxTgbZC8Nd0XvDn50eWUH_R1e3mUYB1OTgMxqWScea0s8LfDkkeho6pPwgrC8GMABr9bGN_Y2OHUL489Oo2oYMM8T40q3IWZMypu8mPmSwqJITsic1fylEBJ63BAMijRu5VMbjWsxDm_xyFFnsOxf9YcW0WgHdhNqLw_SBQUxUFm5wTvZUDZfN7yAjEL](https://lh4.googleusercontent.com/smYUFxTgbZC8Nd0XvDn50eWUH_R1e3mUYB1OTgMxqWScea0s8LfDkkeho6pPwgrC8GMABr9bGN_Y2OHUL489Oo2oYMM8T40q3IWZMypu8mPmSwqJITsic1fylEBJ63BAMijRu5VMbjWsxDm_xyFFnsOxf9YcW0WgHdhNqLw_SBQUxUFm5wTvZUDZfN7yAjEL)

As it is observed, each of the control inputs have a non-zero steady state value; positive for thrusters 6,7 and negative for thrusters 5,8.

***Reason:*** Thrusters 5,6,7,8 are used for positional control in the Z direction. When the rover moves to the desired Z coordinate position in the required orientation, the job of the thrusters is not done since the position has to be retained while neutralising an external force. This external force is the buoyancy acting upon the rover since it is designed to be positively buoyant. The values of forcing acting due to weight and buoyancy for the rover under study are:

W = 112.8 N

B = 114.8 N ; this implies that the net external force acting on the rover is 2N upwards.

When we look at thrusters 5,8 ; they provide a thrust vertically upwards when rotated clockwise while thrusters 6,7 produce a thrust vertically downwards for the same. This means that for thrusters 5,8 positive thrust is upwards while negative thrust is downwards. This is opposite in the case of thrusters 6,7 From the plots of control inputs to the above thrusters, the steady state values are as follows:

For thrusters 5,8: -1.25*10-2 (approx.)

For thrusters 6,7: +1.25*10-2 (approx.)

Thrust produced by thrusters 5,8 is negative, which implies that thrust is produced in the  vertically downwards direction.

Thrust produced by thrusters 6,7 is positive, which again implies that thrust is produced in the vertically downwards direction.

Total thrust produced T = K*u where K= 40 for all the thrusters.

Total thrust produced in steady state: 4*1.25*10-2 *40 N = 2 N downwards.

Since the external force in Z direction has been neutralised, the rover is stabilised once it reaches the desired position.

This is how the positional control of the underwater rover has been established using the linear PID control method.

**2) Non-Linear Controller Model:**

For desired positional input as [1; 1; 2; 0; 0; 0], we got the following results for positional coordinates in world frame:

**Position X :**

Desired output: 1 m

[https://lh4.googleusercontent.com/dWTQ3K4m_sP83L9fknlMB8R2lyBh3HZJK3UX3byA6JUuQK-xmsZPSz4j3qn42o3jRxlPTBDxr0kURM51KIMR-7cxTdtbMzZTDPS139FUyEE69z7KSQ4xkA-Y3hdcBJdeJFIXNJI56Kq_UQ4D4SW3-LdWCOWAsWnw0nFHbB-txFmFez02LTdEH1XWCMuH1LrL](https://lh4.googleusercontent.com/dWTQ3K4m_sP83L9fknlMB8R2lyBh3HZJK3UX3byA6JUuQK-xmsZPSz4j3qn42o3jRxlPTBDxr0kURM51KIMR-7cxTdtbMzZTDPS139FUyEE69z7KSQ4xkA-Y3hdcBJdeJFIXNJI56Kq_UQ4D4SW3-LdWCOWAsWnw0nFHbB-txFmFez02LTdEH1XWCMuH1LrL)

**Position Y :**

Desired output: 1 m

[https://lh6.googleusercontent.com/2CEK592u7BFWMN0m82ffWnRhfhs5as5uHEa87mg1vBDVKGfaieyVpsHMmCQ2EK5M0CS2Es8uvsS7Fn6yYNK5RhD9h0ZERawYqF2ml1sVe-5wrVbhJT774q7LaR5BxdDmNlOBa8wU4e9GP9tHRcDGoO77CPnxqF_1ajFnLeCkIsHvF5_WcZtHxeDPyvlVd1yw](https://lh6.googleusercontent.com/2CEK592u7BFWMN0m82ffWnRhfhs5as5uHEa87mg1vBDVKGfaieyVpsHMmCQ2EK5M0CS2Es8uvsS7Fn6yYNK5RhD9h0ZERawYqF2ml1sVe-5wrVbhJT774q7LaR5BxdDmNlOBa8wU4e9GP9tHRcDGoO77CPnxqF_1ajFnLeCkIsHvF5_WcZtHxeDPyvlVd1yw)

**Position Z :**

Desired output: 2 m

[https://lh5.googleusercontent.com/xBm6H6U65t6L8Y7rhX8gOpl-EF9s6u-YVJk94mjq6iRlzeA4WK4zPzOXC_lfl3uzeKbGXgiuRXDsWbJxGwHRWXBzCiAMTsTZ65I1ChoRp_8FWkYrBusruKy9XTQ2hDzHvsYDLs5ZOG0TmA1JQWid6EUsLyr4UFf35RIDrIks1ItfU9MLAnItdhUGTOmzS6wn](https://lh5.googleusercontent.com/xBm6H6U65t6L8Y7rhX8gOpl-EF9s6u-YVJk94mjq6iRlzeA4WK4zPzOXC_lfl3uzeKbGXgiuRXDsWbJxGwHRWXBzCiAMTsTZ65I1ChoRp_8FWkYrBusruKy9XTQ2hDzHvsYDLs5ZOG0TmA1JQWid6EUsLyr4UFf35RIDrIks1ItfU9MLAnItdhUGTOmzS6wn)

**Orientation 𝛷 :**

Desired output: 0 rad

[https://lh5.googleusercontent.com/O1gd6SJFFlwrYg716tFhP8FdFYxBUrokLUCHp1u4aqS4L6Ys-keG1-zI7aL4FiNjW2UHhQKhfAzQcUMvKy5d2Dn8YxWKjHYoaqm23WaJ9CjoYqaaFRfh42r5mUEh_3MOkjUQ47ikjdYYDWDKBJiuUQNG4Pa0usocn2qpQqDcaJy7L-RCKRJlINnSBCZeUD1-](https://lh5.googleusercontent.com/O1gd6SJFFlwrYg716tFhP8FdFYxBUrokLUCHp1u4aqS4L6Ys-keG1-zI7aL4FiNjW2UHhQKhfAzQcUMvKy5d2Dn8YxWKjHYoaqm23WaJ9CjoYqaaFRfh42r5mUEh_3MOkjUQ47ikjdYYDWDKBJiuUQNG4Pa0usocn2qpQqDcaJy7L-RCKRJlINnSBCZeUD1-)

**Orientation 𝛳 :**

Desired output: 0 rad

[https://lh5.googleusercontent.com/pBCEMCBPJwsT8ilKt6BOyJpJ5MX_cjJ-CJjBa5244kzALagniXtmzC3zsGuSnrqK63fYRi3wLc4xaGELA1whfapuwIfBf0ziWUGjCuXd26njXUbouYH8adQcYKY5GhyXcUuSh5NG2hkAD92A98fVL6K9xQSMJGmLYncMLF9kzvfUXF96An1U7m8eV5nEhj4J](https://lh5.googleusercontent.com/pBCEMCBPJwsT8ilKt6BOyJpJ5MX_cjJ-CJjBa5244kzALagniXtmzC3zsGuSnrqK63fYRi3wLc4xaGELA1whfapuwIfBf0ziWUGjCuXd26njXUbouYH8adQcYKY5GhyXcUuSh5NG2hkAD92A98fVL6K9xQSMJGmLYncMLF9kzvfUXF96An1U7m8eV5nEhj4J)

**Orientation 𝛹 :**

Desired output: 0 rad

[https://lh6.googleusercontent.com/Xc9_NBs7vZ0htBwv86JPQIJ319jRTfFo9MGuioe6i8jKOqBiibVJkHMIQIwKNpJvM937giJFP2ldDV9GdRUyhENnrQe01puURvyUkr5Z-e2TOEN1WlagHz4fbD0NuVsxvdu0aBpFjJCszUF7DIFOGR-WGfMAEWzNMMo4UlajU4A90FPafIEyOvZ5eWhvRAQD](https://lh6.googleusercontent.com/Xc9_NBs7vZ0htBwv86JPQIJ319jRTfFo9MGuioe6i8jKOqBiibVJkHMIQIwKNpJvM937giJFP2ldDV9GdRUyhENnrQe01puURvyUkr5Z-e2TOEN1WlagHz4fbD0NuVsxvdu0aBpFjJCszUF7DIFOGR-WGfMAEWzNMMo4UlajU4A90FPafIEyOvZ5eWhvRAQD)

The controller input plots (controller input for each thruster will be as follows):

[https://lh4.googleusercontent.com/vV8lSX2J6f3IQluq-hSSDzDGcr1wpl2SDg1mfP-BXp_znq2UJJEWUkQ1UomlfaThOFXuVrNhraJfnK4pWGQppuMkGXBL4RWVIj1h3qAjw5Gtv9YKHzHinO1PD2_hFgBb2TDFLNty9auFqy2RgP9kdSybqFAGIMhxvnVJs_kwj8kGsQMDmm9tZq125C8y8us2](https://lh4.googleusercontent.com/vV8lSX2J6f3IQluq-hSSDzDGcr1wpl2SDg1mfP-BXp_znq2UJJEWUkQ1UomlfaThOFXuVrNhraJfnK4pWGQppuMkGXBL4RWVIj1h3qAjw5Gtv9YKHzHinO1PD2_hFgBb2TDFLNty9auFqy2RgP9kdSybqFAGIMhxvnVJs_kwj8kGsQMDmm9tZq125C8y8us2)

So, we can see that the thrust control input for thrusters 5,6,7 and 8 will be: -1.375e-02, 1.375e-02, 1.375e-02, -1.375e-02 respectively.

Now, since propeller pair of 5 and 8 will be opposite to that of the pair 6 and 7, we get the total thrust on the ROV in Z-direction as:

1.375e-02*4*40 = 2N (approx.), where 40 is the gain.

So, the force in the vertical direction is balanced.

**B)  LQR Controller:**

**Literature Review:**

The PID controller has been one of the most commonly used controllers for a really long time. There have been numerous PID tuning techniques, such as the Ziegler-Nichols method but are insufficient for high-performance control applications. The Linear Quadratic Regulator (LQR) is an optimal control method based on full state feedback. It aims to minimise the quadratic cost function and is then applied to linear systems, hence the name Linear Quadratic Regulator.

**Why LQR controller?**

The use of LQR  over PID control comes from the higher robustness of the former in terms of tuning the parameters with varying conditions. PID control uses the error in the input parameter of the closed loop system and tunes the parameters to reduce the error to zero. LQR, on the other hand, uses the state space model of the system and takes complete state feedback: -Kx, to calculate the error. LQR uses the method of cost function to calculate the control input cost vs the importance of achieving desired states.

**State-Space Model with Full State Feedback Gain:**

[https://lh5.googleusercontent.com/qxJw4GOGbUSj-mAesJ5HO_WR7YPZ075uS4xOminohLd9GAERiSmRbmfX8jci7MI-oaOA9JLg6JQO5LEy5Vjul02OmrHIAyMpQMmw5Z4YavRELCYa1HzszVZTvw9Dae7x5x2iaOcHWnovOKv1Z25_351AW3XOnmZZ0Jez1qXroUxqbOZI47UK8GmKrtLSH0ij](https://lh5.googleusercontent.com/qxJw4GOGbUSj-mAesJ5HO_WR7YPZ075uS4xOminohLd9GAERiSmRbmfX8jci7MI-oaOA9JLg6JQO5LEy5Vjul02OmrHIAyMpQMmw5Z4YavRELCYa1HzszVZTvw9Dae7x5x2iaOcHWnovOKv1Z25_351AW3XOnmZZ0Jez1qXroUxqbOZI47UK8GmKrtLSH0ij)

Ẋ = AX + BU

y = CX+DU

**Cost Function:**

The cost function defined by the system equations is minimised by the LQR controller using an optimal control algorithm. The cost function involves the system's state parameters and input (control) parameters, along with the Q and R matrices. For the optimal LQR solution, the overall cost function must be as low as possible. The weights given to the state and control parameters are represented by the Q and R matrices, which act as knobs whose values can be varied to adjust the total value of the cost function.

The system must have a linearized state-space model to solve the LQR optimization problem. The cost function to be optimised is given by

𝐽 = ∫ (𝑋*T*𝑄𝑋 + 𝑈*T*𝑅𝑈)𝑑*t*

**Algebraic Riccati Equation and Its Solution(S matrix):**

The Q and R matrices are used to solve the Algebraic Riccati Equation (ARE) to compute the full state feedback matrix.

𝐴*T*𝑆 + 𝑆𝐴 − 𝑆𝐵𝑅*-1*𝐵*T*𝑆 + 𝑄 = 0

On solving the above equation, we obtain the matrix 𝑆.

**Feedback Gain (K) and Eigen Values from S matrix:**

The matrix 𝑆 obtained from the above ARE is used to find the full state feedback gain matrix K using the relation,

𝐾 = 𝑅*-1*𝐵*T*𝑆

The control matrix U is then given by

U = - KX

**Linearisation of a state-space model:**

The linearization of a state-space model is needed while using the LQR technique since it works on linear systems. In our case, the state space model is in the form: ẋ = f(x); where f(x) is a nonlinear function of x. In such cases, to linearise the equation, we use the concept of linearising about a fixed point. The steps for the same are as follows:

1. Find the fixed points x̄ ; where f(x̄) = 0.
2. Linearise about an x̄, by calculating the Jacobian of dynamics at the fixed point x̄; where the latter can be represented as:

[https://lh3.googleusercontent.com/AiPZbHWZDnip4OPovRDsfezjdz0oNaUS4KzwZYiNm4wpZb2P9TuZ_scpVkb0fUp7kdqCrypVfp2J0WKBZZzlSx38bTPk57H6CPXirqFgSuP0AcwwXPsdN6Ha6RaT97PLMuylwtvzj_tsgSGS71TGJBY16HTNKyyhKl_A_XB3lRaBPZKt1SMEqIQTWaJp9XIN](https://lh3.googleusercontent.com/AiPZbHWZDnip4OPovRDsfezjdz0oNaUS4KzwZYiNm4wpZb2P9TuZ_scpVkb0fUp7kdqCrypVfp2J0WKBZZzlSx38bTPk57H6CPXirqFgSuP0AcwwXPsdN6Ha6RaT97PLMuylwtvzj_tsgSGS71TGJBY16HTNKyyhKl_A_XB3lRaBPZKt1SMEqIQTWaJp9XIN)

where each term is a partial derivative of the dynamics with respect to a variable.

This step is executed since the dynamics of a nonlinear system behave linearly at the fixed point, or, in a small neighborhood around the fixed point.

Changing the frame of reference to one with x̄ as the origin:

ẋ -ẋ̄  = f(x)

= f(x̄) + J.(x -x̄) + J2.(x-x̄)2 + . . . . .

where   J  is calculated at x̄.

The higher order terms from the third term  J2.(x-x̄)2 are neglected since they are really small, hence the equation reduces to:

∆ẋ = 0 +J.∆x + 0

⇒ ∆ẋ = J.∆x

This is in the form  ẋ = Ax

*Note:* The above method for linearization works only when the fixed point satisfies the condition for linearising a system given by the Hartman Grobman Theorem, which states that:

“*the behaviour of a dynamical system in a domain near a hyperbolic equilibrium point is qualitatively the same as the behaviour of its [linearisation](https://en.wikipedia.org/wiki/Linearization) near this equilibrium point, where hyperbolicity means that no eigenvalue of the linearisation has real part equal to zero. Therefore, when dealing with such dynamical systems one can use the simpler linearisation of the system to analyse its behaviour around equilibria.[[1]](https://en.wikipedia.org/wiki/Hartman%E2%80%93Grobman_theorem#cite_note-1)*”

Hence, linearization works only when the fixed point is hyperbolic or put simply, has a non-zero real part.

**Implementation:**

The following is the Simulink model that we built:

[https://lh5.googleusercontent.com/evtLQM21i3uN9EyCsCoEZT8tFGHewd8FSvLAOoDFsdt0VhYTuBn6o83IIxgQxDh9KeoZ0kQimeLWcQhZPxsq1QEQpdyO6T8nDISinLtuchq7oH6eO7nPDYCwctmT2cCNqrGRb_t3JhPNOFGwvYaQ_1A7oDNlcvBEGQPpO0i1zXKaATiUUvz8XgU8ZLLLxd3v](https://lh5.googleusercontent.com/evtLQM21i3uN9EyCsCoEZT8tFGHewd8FSvLAOoDFsdt0VhYTuBn6o83IIxgQxDh9KeoZ0kQimeLWcQhZPxsq1QEQpdyO6T8nDISinLtuchq7oH6eO7nPDYCwctmT2cCNqrGRb_t3JhPNOFGwvYaQ_1A7oDNlcvBEGQPpO0i1zXKaATiUUvz8XgU8ZLLLxd3v)

To implement this, we need the following:

1. State-space model:
    1. A and B matrices: These relate the derivative of the states with the current states and the control input. But, since our model is non-linear in nature, we cannot get a direct relation consisting of A and B matrices. Also, the LQR controller works only for Linear Systems. So, we have to linearise our system around an operating point. For our system, we took the origin in the world frame (initial point of the bot) as the operating point. Also, we used the position of the bot in the world frame and velocity of the bot in its own frame as the two states. Meaning:

[https://lh4.googleusercontent.com/usycppy-9oTOk5QIKMUphDGwxyLz5gHZt9EBtRUeoLvjjYeexPx1XGBpWTBZyVKkX3323CrC3NMzWqWkZT5yJIcpjHNV2ggdiVMRahRjUbuA0IWvDM3_AAfHF79r6pSuCnGKwtQlI_3iRQYgGirv7QvpBDAwFUVA2zzp4h4MhcmeS-Zagz3fWucWegRuuZYk](https://lh4.googleusercontent.com/usycppy-9oTOk5QIKMUphDGwxyLz5gHZt9EBtRUeoLvjjYeexPx1XGBpWTBZyVKkX3323CrC3NMzWqWkZT5yJIcpjHNV2ggdiVMRahRjUbuA0IWvDM3_AAfHF79r6pSuCnGKwtQlI_3iRQYgGirv7QvpBDAwFUVA2zzp4h4MhcmeS-Zagz3fWucWegRuuZYk)

(Here x is the state but not the distance in x direction)

[https://lh4.googleusercontent.com/0srxj2jYdlOyK8g3GBsVQapTNd0ej7qRw2BM9nwP92IeTryEE5m5p1QDs3UC-xzGoYA09fRPDPYu60auiQV6-Nmg8KukbgIOXG8rFPbUJ8RwlqLWKtzPIUATS5Yhse13bp7YFpPY5eFhp8z6lBiw8xex9Phk6bj0H6TgBm3CmTbbB8bMKJHM4b0_QxPkA0hI](https://lh4.googleusercontent.com/0srxj2jYdlOyK8g3GBsVQapTNd0ej7qRw2BM9nwP92IeTryEE5m5p1QDs3UC-xzGoYA09fRPDPYu60auiQV6-Nmg8KukbgIOXG8rFPbUJ8RwlqLWKtzPIUATS5Yhse13bp7YFpPY5eFhp8z6lBiw8xex9Phk6bj0H6TgBm3CmTbbB8bMKJHM4b0_QxPkA0hI)

Since,

[https://lh6.googleusercontent.com/TG03dKmaOF98_FVYTbPDzL7cIvMD-wcTcVV_qXg4gB0UNf3Inv3FFY3XGurBnuc89T9xAjzt56wszTSQgrk6eu9NLNqL9WfU8gNlExC5WKgAZ5qK8NJ_ymZr1XfxbblsSoMhzyqGc41xarXY30YYSLDJbZV76JsOMyXgPEf18EqqSBLqlNNtXF-TKObt-MSn](https://lh6.googleusercontent.com/TG03dKmaOF98_FVYTbPDzL7cIvMD-wcTcVV_qXg4gB0UNf3Inv3FFY3XGurBnuc89T9xAjzt56wszTSQgrk6eu9NLNqL9WfU8gNlExC5WKgAZ5qK8NJ_ymZr1XfxbblsSoMhzyqGc41xarXY30YYSLDJbZV76JsOMyXgPEf18EqqSBLqlNNtXF-TKObt-MSn)

The above state space model is clearly non-linear. So, we have to linearise to find A and B as follows:

[https://lh5.googleusercontent.com/a7LCqXM8G8Ue3fst3D43MfY4sRC5eZ8qqWxHofV56OTueeXBoCTgiNNS8ZWjud4sA96eIFQ3s27MHidxcWENHPqATIMJDROSHJeUp_07P2ZR-IOBFQIKdaermOty1GT6zysnZ_MJtD-UNw6GUbTrQBbFRk0EjcQEzgZQPSPciz1L_QdKJR_YxO2DaTJgPw-X](https://lh5.googleusercontent.com/a7LCqXM8G8Ue3fst3D43MfY4sRC5eZ8qqWxHofV56OTueeXBoCTgiNNS8ZWjud4sA96eIFQ3s27MHidxcWENHPqATIMJDROSHJeUp_07P2ZR-IOBFQIKdaermOty1GT6zysnZ_MJtD-UNw6GUbTrQBbFRk0EjcQEzgZQPSPciz1L_QdKJR_YxO2DaTJgPw-X)

[https://lh6.googleusercontent.com/goeHoE_hnLYq5ij98zLaUNgyBXt-pvjMzUp9y0X2OoB_PXGF5JMPbE1WOwMuJeS38iZB6IEZ5WDqeVn9Ueb2mHNJcXT_IPMVWnu4y83Z75bggm3OIeHlSu41SCmdG24wV7FYVPysm1ADqfh0DRrrn7x5rpAl23oQ8vvMSiOUaP82WIdEOU9dWFf1dhj1OMWI](https://lh6.googleusercontent.com/goeHoE_hnLYq5ij98zLaUNgyBXt-pvjMzUp9y0X2OoB_PXGF5JMPbE1WOwMuJeS38iZB6IEZ5WDqeVn9Ueb2mHNJcXT_IPMVWnu4y83Z75bggm3OIeHlSu41SCmdG24wV7FYVPysm1ADqfh0DRrrn7x5rpAl23oQ8vvMSiOUaP82WIdEOU9dWFf1dhj1OMWI)

So, we obtain the A and B as follows:

[https://lh4.googleusercontent.com/7XXtrMKmilLrE6RY_IeQ-0wFgIPT8Z-mT1iMYs-q7PBte6sHyvc9dpJty4P_W5EIc0lzSSCBjIYD39LQTY9wqyNjnMrekLMG-W_7Y8C9hFIzHEkQkSHiQiOgv8LGRpWGCpbpbVFosgJEhdH24-xyG6t4bIYn1iwP0cPIiPa4F_Se1QLGBtQL1RdU641DdOAw](https://lh4.googleusercontent.com/7XXtrMKmilLrE6RY_IeQ-0wFgIPT8Z-mT1iMYs-q7PBte6sHyvc9dpJty4P_W5EIc0lzSSCBjIYD39LQTY9wqyNjnMrekLMG-W_7Y8C9hFIzHEkQkSHiQiOgv8LGRpWGCpbpbVFosgJEhdH24-xyG6t4bIYn1iwP0cPIiPa4F_Se1QLGBtQL1RdU641DdOAw)

[https://lh5.googleusercontent.com/5dokKd-QWWW1U56rpR3PPuR2SBcXFvb-fggXjerdvUe4C8VIf3bgmA_oYniz9ehbeq6ZdmWMbN3KqWI-SWcpGUbPSBFFVnYlNttzF9PLlOh1rjJJAM5zF1RcFzjJkrI4VWZP15Cqoy2ko2OTfrfL6HommWpubp2FLBOvtOUCdSD_Alh60vX1o1hn29E7CORA](https://lh5.googleusercontent.com/5dokKd-QWWW1U56rpR3PPuR2SBcXFvb-fggXjerdvUe4C8VIf3bgmA_oYniz9ehbeq6ZdmWMbN3KqWI-SWcpGUbPSBFFVnYlNttzF9PLlOh1rjJJAM5zF1RcFzjJkrI4VWZP15Cqoy2ko2OTfrfL6HommWpubp2FLBOvtOUCdSD_Alh60vX1o1hn29E7CORA)

But we obtained the above results by taking weight W and buoyancy B as 110 N and 120 N respectively.

1. We take the C matrix in state-space (y=CX+DU) as eye(12) so as to send back all the states as feedback to the summing point.
2. Q and R matrices:
    1. Q stands for the importance of the states to reach their desired values.
    2. R stands for the cost of the control input.
    3. So, if we have an expensive control input compared to that of our needs to reach the states, we take the Q matrix to be dominating compared to R and vice versa.
3. Then, we calculate the full-state feedback matrix, the Algebraic Riccati Equation and the eigenvalues by using the following command:

[K,S,P] = lqr(A,B,Q,R)

1. Then, we put the obtained value of the Gain matrix in the state-space model in Simulink. The following results are obtained for different R matrices (Q matrix being a 12x12 identity matrix) and desired states being [1;2;1;0;0;0;0;0;0;0;0;0]:

[https://lh4.googleusercontent.com/GJLiLKhoIVYQW0x77g3VmfZuCSSFa8gcAdi8m2fgl3Qx57FDh3XsdALLCpcqDtOAZiAvFFcCNQY8wYtBW_K8_FtCPo3CwsPrkSCWtCGXifT75zIsSpLC_UefAH7HbNLZh5M3TQTkurUNrUvl3vVTsUtyesSivAHTFRHJuDX8xlRCq_dSac8ZfcmW3jZB5CEb](https://lh4.googleusercontent.com/GJLiLKhoIVYQW0x77g3VmfZuCSSFa8gcAdi8m2fgl3Qx57FDh3XsdALLCpcqDtOAZiAvFFcCNQY8wYtBW_K8_FtCPo3CwsPrkSCWtCGXifT75zIsSpLC_UefAH7HbNLZh5M3TQTkurUNrUvl3vVTsUtyesSivAHTFRHJuDX8xlRCq_dSac8ZfcmW3jZB5CEb)

[https://lh3.googleusercontent.com/NbCxKuYAEFtXmpmZ-jf09ITaSHdV8_WoqZOu4KfGqFOzZorjN3PGLzk7oeyF-EFqces_0CkIoQhzaqLsiFdynl85KALzbJo34dXrQylSnuwOqsiU19GtmGkgo6yMmZAw66vPDttVd6q_QA-CcxRV66InkNQSsxSBFqG38k6H0PajZff7LFDhMfoxLQbTIz7J](https://lh3.googleusercontent.com/NbCxKuYAEFtXmpmZ-jf09ITaSHdV8_WoqZOu4KfGqFOzZorjN3PGLzk7oeyF-EFqces_0CkIoQhzaqLsiFdynl85KALzbJo34dXrQylSnuwOqsiU19GtmGkgo6yMmZAw66vPDttVd6q_QA-CcxRV66InkNQSsxSBFqG38k6H0PajZff7LFDhMfoxLQbTIz7J)

For the above, the Poles of the closed loop and open loop transfer function are as follows:

[https://lh3.googleusercontent.com/XU48RRm1jZGmBQAn7ImEck6fDJM92joZ1VmZkbUdmVjA43u48CqDi1pCA6P5DodtlZ2m78A30ySeTu2F3fH-1J9iwMr97ePLjGN97DWmnt8Gx9BOQtqVlVWzHbfk7QZ4SxY1Fcmz6WZBfPp66oUqEPjqbAY2xWDuXujlbqAtoMjoLCf3fEACqA5_-5XdCeAn](https://lh3.googleusercontent.com/XU48RRm1jZGmBQAn7ImEck6fDJM92joZ1VmZkbUdmVjA43u48CqDi1pCA6P5DodtlZ2m78A30ySeTu2F3fH-1J9iwMr97ePLjGN97DWmnt8Gx9BOQtqVlVWzHbfk7QZ4SxY1Fcmz6WZBfPp66oUqEPjqbAY2xWDuXujlbqAtoMjoLCf3fEACqA5_-5XdCeAn)

**Poles:**

Closed loop(With the feedback):

[https://lh6.googleusercontent.com/fV3aMJoYwnw094QNVbodVTIyDb_RCiOhQEGG87-0qds16TLFMSgzcyYH6d6QQFEFH-22sCdl12v68yYPKLXXOvF9-mtuPPB4FA-J7nJm69hHbM98RTw-mi-9EdKP7ZfeYKDfkG9r7Yzbot7aooPJGhxRqgYSYzXn74L2Jfi-EuqjfKLv7iEEBTzzrLqfmAyR](https://lh6.googleusercontent.com/fV3aMJoYwnw094QNVbodVTIyDb_RCiOhQEGG87-0qds16TLFMSgzcyYH6d6QQFEFH-22sCdl12v68yYPKLXXOvF9-mtuPPB4FA-J7nJm69hHbM98RTw-mi-9EdKP7ZfeYKDfkG9r7Yzbot7aooPJGhxRqgYSYzXn74L2Jfi-EuqjfKLv7iEEBTzzrLqfmAyR)

Open loop(Without the feedback):

[https://lh6.googleusercontent.com/R2L8AiYHlEIFK-cMlxfX1TdWtBZE6VA6Qj5OfHy6KyIFPmSTSxIE3W35i0V_LQjxRF5p1lyAAevFeHvvpsNSbhfJqjLGgp_DIZC8q5vGL_svjWqNZ72RVTN5HwlGB0JI9v8Rz2tYMjBouGsyJhC22lKwgr_W9Q-D1V-ZB4orLguGO32cFikfqElFRWpSiJGv](https://lh6.googleusercontent.com/R2L8AiYHlEIFK-cMlxfX1TdWtBZE6VA6Qj5OfHy6KyIFPmSTSxIE3W35i0V_LQjxRF5p1lyAAevFeHvvpsNSbhfJqjLGgp_DIZC8q5vGL_svjWqNZ72RVTN5HwlGB0JI9v8Rz2tYMjBouGsyJhC22lKwgr_W9Q-D1V-ZB4orLguGO32cFikfqElFRWpSiJGv)

Making ‘R’ more dominant

[https://lh5.googleusercontent.com/UH2rUQigw9K9h98-yCho5KKvtiwZnqkEHUenJ739dCfpb8fppdZtL3Lx6qKrHudNp9scv-HK6Ml2xZ0XI0kWBTITs4MSOcMTQ99xpf-FB0RNVDs8uhQ9QTrVS-wpPCKn0n-ntX920jLLzbAaKlczEO7Xs19Ihsteo0hIdN-W3kij_7duMrUd3LZebjBBD6BC](https://lh5.googleusercontent.com/UH2rUQigw9K9h98-yCho5KKvtiwZnqkEHUenJ739dCfpb8fppdZtL3Lx6qKrHudNp9scv-HK6Ml2xZ0XI0kWBTITs4MSOcMTQ99xpf-FB0RNVDs8uhQ9QTrVS-wpPCKn0n-ntX920jLLzbAaKlczEO7Xs19Ihsteo0hIdN-W3kij_7duMrUd3LZebjBBD6BC)

[https://lh5.googleusercontent.com/Y9Naj1f-bPqyh0YHmfsBbBSTTJeGaqALajXrOOdJ3fpPJVAiQwCVqUUbZiVy_wl4LGVbp8J9xhL2A_DqTqvQh95_XbIjw3qyMUJgd6MXwVfye5kQ87xH7-PiFA-vE43XObJwqbOGw3qVbgg4jqstg2dzH0wLAIHVwWgnha4Vg3TYMS6_GEsyp1EDsacjxWyp](https://lh5.googleusercontent.com/Y9Naj1f-bPqyh0YHmfsBbBSTTJeGaqALajXrOOdJ3fpPJVAiQwCVqUUbZiVy_wl4LGVbp8J9xhL2A_DqTqvQh95_XbIjw3qyMUJgd6MXwVfye5kQ87xH7-PiFA-vE43XObJwqbOGw3qVbgg4jqstg2dzH0wLAIHVwWgnha4Vg3TYMS6_GEsyp1EDsacjxWyp)

Making ‘R’ less dominant

[https://lh3.googleusercontent.com/09DoR0iYtlNtnM2PmyaBNtdxTIjoL8q_Kdylx9TKW9SNcjX1BpaMQNElp3qk5STGr0NIGkuvg19mwg4RF_F1LNSc8Jwn_lLbGmaK3XRmZ-7o3t9RY_5Dq5GPcl36fj1rfNz94pJeQxiBs4DwV-41C0j8x8N2mv6eGQLkbNMsP7lbZzba3ixScQwg5J7SruJY](https://lh3.googleusercontent.com/09DoR0iYtlNtnM2PmyaBNtdxTIjoL8q_Kdylx9TKW9SNcjX1BpaMQNElp3qk5STGr0NIGkuvg19mwg4RF_F1LNSc8Jwn_lLbGmaK3XRmZ-7o3t9RY_5Dq5GPcl36fj1rfNz94pJeQxiBs4DwV-41C0j8x8N2mv6eGQLkbNMsP7lbZzba3ixScQwg5J7SruJY)

[https://lh3.googleusercontent.com/C9FHha5EE9ErH4Gru42JxlpfjrcBIYl4PWjGryJmVePp_EX6gwXhij3fRe_5Zq8p2XyrNgF3aNNpqsBTQAeCHbvK5hxeyGvz0zptk-rwwX4CgHL8mYas2d8_Wef2orMsS4P0BceiyHuk0jG1SmxFIlwSyOk8tnKCkA_Ei00J4iYveI7F1-BSvlg-R1xTmPHi](https://lh3.googleusercontent.com/C9FHha5EE9ErH4Gru42JxlpfjrcBIYl4PWjGryJmVePp_EX6gwXhij3fRe_5Zq8p2XyrNgF3aNNpqsBTQAeCHbvK5hxeyGvz0zptk-rwwX4CgHL8mYas2d8_Wef2orMsS4P0BceiyHuk0jG1SmxFIlwSyOk8tnKCkA_Ei00J4iYveI7F1-BSvlg-R1xTmPHi)

**Results:**

1. Observed states:

[https://lh4.googleusercontent.com/bs9_vU8fweBPwtUO8CHZRWuxKi24hyvp85h2joFIJyDENZvmbHf8mRMfznYqdoZzqoMZDDVQlhSHL7brxSwQFrYOUbwFX0t-bDWBgmHIzPZE-I_s2eNO5ZECANOUorreL267pNOU4WVIefJqIxQTNavwPfoXZ-BU3h4L0XCsYN6eGytAIMcHxulWWOdZhAwK](https://lh4.googleusercontent.com/bs9_vU8fweBPwtUO8CHZRWuxKi24hyvp85h2joFIJyDENZvmbHf8mRMfznYqdoZzqoMZDDVQlhSHL7brxSwQFrYOUbwFX0t-bDWBgmHIzPZE-I_s2eNO5ZECANOUorreL267pNOU4WVIefJqIxQTNavwPfoXZ-BU3h4L0XCsYN6eGytAIMcHxulWWOdZhAwK)

More dominant ‘R’

[https://lh3.googleusercontent.com/qVQ-8tcW4zs2plx8W9Ti7Is8m1jpXcKhCopWtcc5eAH58eMln7YQctoRsHLtyCV_LKM5tNWZrjTU4htMGKMlU0xXLQe7-iqpGK3rAuUzZHh7FtZOmROm9Yikyl4fbB_2KeNeJXqE0yjDNI7oq25LhBuL9vcjViQhjwi1fYy9ZJuUd3DpmYmVXMOifKrUVdHg](https://lh3.googleusercontent.com/qVQ-8tcW4zs2plx8W9Ti7Is8m1jpXcKhCopWtcc5eAH58eMln7YQctoRsHLtyCV_LKM5tNWZrjTU4htMGKMlU0xXLQe7-iqpGK3rAuUzZHh7FtZOmROm9Yikyl4fbB_2KeNeJXqE0yjDNI7oq25LhBuL9vcjViQhjwi1fYy9ZJuUd3DpmYmVXMOifKrUVdHg)

Less dominant ‘R’

[https://lh3.googleusercontent.com/HdGDHEI6UcOvVBTLyCYe9u59aHXx7ozhpueYkdnSPzxCVj1RweHofjEMkCGFJE6Hbvbow3jpjojiiPYWnUg1T5ZRs41D68uv5DW23uykJBaQ9LXBkgbAW5O-wQWworyUlG7Vb4w5AhWsRLs_QYiqR4cjQ8N9oeB9bpvQldYt1_aQO1aGMQvERpvTx7NPG51y](https://lh3.googleusercontent.com/HdGDHEI6UcOvVBTLyCYe9u59aHXx7ozhpueYkdnSPzxCVj1RweHofjEMkCGFJE6Hbvbow3jpjojiiPYWnUg1T5ZRs41D68uv5DW23uykJBaQ9LXBkgbAW5O-wQWworyUlG7Vb4w5AhWsRLs_QYiqR4cjQ8N9oeB9bpvQldYt1_aQO1aGMQvERpvTx7NPG51y)

1. Thrust i/p to the thrusters:

[https://lh6.googleusercontent.com/t5Y37Ck-n_dfGzHzrS1o42Xh8DmZFa4rQQtqP3cohdUDOBDa-i47gPADO-xS_DNvdTpnH8Fc5XDFFLh4NOx03rs4UpY31AskaQocLCFkAzKnvoF1h_k8dL35uhmmiVd-wIwfBM4VGXDur5M31-T6XNJT-9GCK4gpyJqglkDT93P16UcDQNF6m0CZssr4i5ZL](https://lh6.googleusercontent.com/t5Y37Ck-n_dfGzHzrS1o42Xh8DmZFa4rQQtqP3cohdUDOBDa-i47gPADO-xS_DNvdTpnH8Fc5XDFFLh4NOx03rs4UpY31AskaQocLCFkAzKnvoF1h_k8dL35uhmmiVd-wIwfBM4VGXDur5M31-T6XNJT-9GCK4gpyJqglkDT93P16UcDQNF6m0CZssr4i5ZL)

More dominant ‘R’

[https://lh5.googleusercontent.com/acg89420fcWPf7yMeQW60p-2rH4pH-8-H3h75BERJB_JblHHV72QVJMDTSIeEYxL3cCsUUzFDfrgD4_EEnfx86aBv-mCvbjO6jM_tszcIDh54AAv-W2AUSc6IQGqGuGCj0fVbIcySkmN-ZSjceP6jUEP90embklvzdnYE4PM0JzlILC_4L245aUxBJ6PVhw3](https://lh5.googleusercontent.com/acg89420fcWPf7yMeQW60p-2rH4pH-8-H3h75BERJB_JblHHV72QVJMDTSIeEYxL3cCsUUzFDfrgD4_EEnfx86aBv-mCvbjO6jM_tszcIDh54AAv-W2AUSc6IQGqGuGCj0fVbIcySkmN-ZSjceP6jUEP90embklvzdnYE4PM0JzlILC_4L245aUxBJ6PVhw3)

Less dominant ‘R’

[https://lh5.googleusercontent.com/kXzt9-Yz7d-O7i6-e3xglgVdGblovmXmtyS25xdoGAb1iHY3BowCgLt1g-QYV-jY6TiGloXWMUJ3AhVrFb1c6HfDTDm2hp9faoPl-Yd90_IZjhzHUJ_ltNrhZM20PxmMyv-tW5rlbX1tmtwEGP7FhNXippclyAYg-fRSd9R685p8YgFLNySYfyxIJP3B2F0R](https://lh5.googleusercontent.com/kXzt9-Yz7d-O7i6-e3xglgVdGblovmXmtyS25xdoGAb1iHY3BowCgLt1g-QYV-jY6TiGloXWMUJ3AhVrFb1c6HfDTDm2hp9faoPl-Yd90_IZjhzHUJ_ltNrhZM20PxmMyv-tW5rlbX1tmtwEGP7FhNXippclyAYg-fRSd9R685p8YgFLNySYfyxIJP3B2F0R)

**Band Limited White Noise:**

We planned on modelling a system with White Noise. So, we used the following control system:

[https://lh5.googleusercontent.com/X05Xbh2TelNlXKC5mIhJkUrFaxJ1Nmdv1Jre9DZkfsIBTXPPIvTMn-SOBaXAZsVRvzxpBrbI1rwoFUE6FI7r3302FKBGbDiWQ6KSBuQSTw1YadDzxb7wSw53xxxBrsMSlU_LrITOZPqTjwYJ03qR_Fn-lA6zj0x_jIxsO_8WdVW5SFkTkP3SZf5dapFtncSM](https://lh5.googleusercontent.com/X05Xbh2TelNlXKC5mIhJkUrFaxJ1Nmdv1Jre9DZkfsIBTXPPIvTMn-SOBaXAZsVRvzxpBrbI1rwoFUE6FI7r3302FKBGbDiWQ6KSBuQSTw1YadDzxb7wSw53xxxBrsMSlU_LrITOZPqTjwYJ03qR_Fn-lA6zj0x_jIxsO_8WdVW5SFkTkP3SZf5dapFtncSM)

Where:

[https://lh4.googleusercontent.com/lLPSNCKsts2h8RE_N65gYH8fgZkReZQT28BUTFV9HCzcAnLetbb_VkBv8IKoJxbG6AoaIpLu99m0Hbl0rJMxN7-9iNOaYvSqOgIHUJJR3JGBMZuCn61aEOzIOCubElpwS6rYSI6KUVZm2u_ylo3neZuvyfa88Io12-cWt2gBQgOKx1IeEAbMWP7rLEzAPYC-](https://lh4.googleusercontent.com/lLPSNCKsts2h8RE_N65gYH8fgZkReZQT28BUTFV9HCzcAnLetbb_VkBv8IKoJxbG6AoaIpLu99m0Hbl0rJMxN7-9iNOaYvSqOgIHUJJR3JGBMZuCn61aEOzIOCubElpwS6rYSI6KUVZm2u_ylo3neZuvyfa88Io12-cWt2gBQgOKx1IeEAbMWP7rLEzAPYC-)

We included a low pass filter for removing noise with frequency greater than 1Hz. With low values of ‘Q’ we are getting really unstable results as the importance to the states is reduced. Later, we increased the value of Q to allow the states to reach quickly. This made the response of our system better. Taking the reference input as: [0;0;2;1.57;1;0.8;0;0;0;0;0;0];

1. Q = eye(12):

States:

[https://lh3.googleusercontent.com/oYnl-IYKC1yCXnVY2PDPbAUXhCSmMAVr0bd_nSyoPBBQNLt5U4xhOsUbpOkt3yl8CVmPIPplozrUUkKHLOm6SwAvGjEYNPUHgdeZe3fvG63j_tpMpAjTFQSh1OIE_hYrlkX-5cH4kMD6sqw6Vgo0BFkQsFyKGfHjStR43zt3In20XvqDirNw4HYAEh7TvuYR](https://lh3.googleusercontent.com/oYnl-IYKC1yCXnVY2PDPbAUXhCSmMAVr0bd_nSyoPBBQNLt5U4xhOsUbpOkt3yl8CVmPIPplozrUUkKHLOm6SwAvGjEYNPUHgdeZe3fvG63j_tpMpAjTFQSh1OIE_hYrlkX-5cH4kMD6sqw6Vgo0BFkQsFyKGfHjStR43zt3In20XvqDirNw4HYAEh7TvuYR)

Thrust provided by each thruster:

[https://lh5.googleusercontent.com/mlLdNb235t0vFPf4od0v6eqw2f32G_b2YiR5aesfvi7ATjPpSjtp22idan56Gy5rhh3xeGGXNRCpnN9MVuijNWEIf2CNWc4aDs49bwldJUixPF5DFhz6BQ2NMGRw4-MVXsy6unE3iA9RG_uBPwKr7aY8gujasDqNiCj2sEaDNPTWX_Ry8PhCRXqS-gxE2QxN](https://lh5.googleusercontent.com/mlLdNb235t0vFPf4od0v6eqw2f32G_b2YiR5aesfvi7ATjPpSjtp22idan56Gy5rhh3xeGGXNRCpnN9MVuijNWEIf2CNWc4aDs49bwldJUixPF5DFhz6BQ2NMGRw4-MVXsy6unE3iA9RG_uBPwKr7aY8gujasDqNiCj2sEaDNPTWX_Ry8PhCRXqS-gxE2QxN)

Poles of this system:

[https://lh5.googleusercontent.com/TdT5Sk5MTqh44NUyDz6es7gH34tVu9P2pVjPehhOyqStNIwLJzbrn1-jrB_vSPVFSKHu6YpL18lp3RsaHXSBjbtweZLeBdoylp1XkSQkQxqhuF6Q7Iak0McYD5N_mxKWDDWK6ge37Es0MlMBvEYSbc3lMQj6W5-EudlV7lOqGqDdJNIcRijpsFAbcqGlxlbK](https://lh5.googleusercontent.com/TdT5Sk5MTqh44NUyDz6es7gH34tVu9P2pVjPehhOyqStNIwLJzbrn1-jrB_vSPVFSKHu6YpL18lp3RsaHXSBjbtweZLeBdoylp1XkSQkQxqhuF6Q7Iak0McYD5N_mxKWDDWK6ge37Es0MlMBvEYSbc3lMQj6W5-EudlV7lOqGqDdJNIcRijpsFAbcqGlxlbK)

1. Q = 1000*eye(12):

States:

[https://lh4.googleusercontent.com/uUWSBvAfUi26sl18kLMfc1zdTo-sgw9If8_mbt5Z-pV5z-9L1pMgqve1iOMoQoemWadz62OJX4yxHuJrULGh3B1nOgwiMKIOcyAqBah2_Epj515qHtxTLjLzYxTZo5PjYJ8jiHmnLnh6YVs8YgUSTtFzFgwJxBJBHMoGIVjtPy0oTnpdGomu3arjUIEOFgZd](https://lh4.googleusercontent.com/uUWSBvAfUi26sl18kLMfc1zdTo-sgw9If8_mbt5Z-pV5z-9L1pMgqve1iOMoQoemWadz62OJX4yxHuJrULGh3B1nOgwiMKIOcyAqBah2_Epj515qHtxTLjLzYxTZo5PjYJ8jiHmnLnh6YVs8YgUSTtFzFgwJxBJBHMoGIVjtPy0oTnpdGomu3arjUIEOFgZd)

Thrust provided by each thruster:

[https://lh5.googleusercontent.com/oiYmWaR2GDxcijGLQrJ51kDrIoOOEuEuiQkqh7J7cKHzJ0uu2t7ftY6lFaSfja0JlxrIygpLweRa6pBhiPfcvezyD7FfKhyVVPtKs-cR-6ODawDuL9FDUpVXHm0eboyesgdUrSH5rWAZk9k1GJnA6plG1PnF2BOoNDT-XXDDufqfjJY8GKA0SqlmUOVPvfZq](https://lh5.googleusercontent.com/oiYmWaR2GDxcijGLQrJ51kDrIoOOEuEuiQkqh7J7cKHzJ0uu2t7ftY6lFaSfja0JlxrIygpLweRa6pBhiPfcvezyD7FfKhyVVPtKs-cR-6ODawDuL9FDUpVXHm0eboyesgdUrSH5rWAZk9k1GJnA6plG1PnF2BOoNDT-XXDDufqfjJY8GKA0SqlmUOVPvfZq)

Poles of this system:

[https://lh5.googleusercontent.com/GMZU2QoQFS4bL2XrHCy1weYAAUl-OeP2RPe4FuUYJhE5Ikyxx3Rrhg_rqnxCPhPhLSgfI-sEDvN_9LjtcZzzB04YyobH8dFTESXCQ-xMAARqGQwDZ7A1MgDVYUpJsMTgxfMb_Ez3FLIyZbhumaJx-SavU4n7VZGdWspwjhVtVPmK6TDht4GP_ecADhZTIKqU](https://lh5.googleusercontent.com/GMZU2QoQFS4bL2XrHCy1weYAAUl-OeP2RPe4FuUYJhE5Ikyxx3Rrhg_rqnxCPhPhLSgfI-sEDvN_9LjtcZzzB04YyobH8dFTESXCQ-xMAARqGQwDZ7A1MgDVYUpJsMTgxfMb_Ez3FLIyZbhumaJx-SavU4n7VZGdWspwjhVtVPmK6TDht4GP_ecADhZTIKqU)

Clearly, the response of the system is faster in the second case.

**Note:** In the above two cases, the open loop and closed loop pole plots are drawn without taking the noise into account. The effect of noise is shown only in the ‘Thrust’ and ‘States’ plots.

Also, in the paper that we referred to, they considered noise caused due to the ‘current velocity’ which has a direct effect on the velocity of the rover instead of the torques. So, for that, a Random Number generator block was included in  the model as follows:

[https://lh6.googleusercontent.com/iMqp-__6ciV6B1diC28Mf4W5AP_XdUY1pv8oJC1ufOOjsOkXO4bhXGLm9Nfxb4nFmmcgryAjubXUPhi1ibUSYUsFR2BKNfqHDQkn6-0kQ6ThuVtsihQZ3tZ5cPDS_phi_PidVa_rBSmFCQOWi274uFpU6d07RLGh2UoS9fl8LI_zUWBjmN3UmiC2ZQMOQfrX](https://lh6.googleusercontent.com/iMqp-__6ciV6B1diC28Mf4W5AP_XdUY1pv8oJC1ufOOjsOkXO4bhXGLm9Nfxb4nFmmcgryAjubXUPhi1ibUSYUsFR2BKNfqHDQkn6-0kQ6ThuVtsihQZ3tZ5cPDS_phi_PidVa_rBSmFCQOWi274uFpU6d07RLGh2UoS9fl8LI_zUWBjmN3UmiC2ZQMOQfrX)

The parameters of the noise block are as follows:

[https://lh5.googleusercontent.com/2QBkuP28xLynCgK5RzGSmciCC3Xwr39H5F2kh4w1nnP8mzqdBPkGLsgbnT74OtL_HE5jUVvBcCI_XLl63yjvs6h_6nzyMsxthfUUXz2enk6QSNWujVM_-AlvC3sV8ATifVST1408d6c0Vi-MPfEPnoJhyw_2UuMsA7yPzAm3sHGwhv74-626slkITMoQcH6B](https://lh5.googleusercontent.com/2QBkuP28xLynCgK5RzGSmciCC3Xwr39H5F2kh4w1nnP8mzqdBPkGLsgbnT74OtL_HE5jUVvBcCI_XLl63yjvs6h_6nzyMsxthfUUXz2enk6QSNWujVM_-AlvC3sV8ATifVST1408d6c0Vi-MPfEPnoJhyw_2UuMsA7yPzAm3sHGwhv74-626slkITMoQcH6B)

Here, noise is added considering the velocity of current as: [0.25,0.25,0.25] and taking the variance as 0.01 in all the three directions.

Taking R matrix as: R=1.2*eye(6)+0.8*ones(6)

And Q matrix as: Q=1000*eye(12);

The results that we obtained for 1min stop time  are:

States:

[https://lh5.googleusercontent.com/6gGAurxBPIFLuMGrd_svmt6aklx0r8nFYteBpje-H-9fcdxuWYnGZ7kPlkW3atCcfliHZcBn11opyJ8MKDhMl9KRdC_8U5da6arvHhdkZXqztmF-U0p2Sgh3qIZdK-Zs39S9HQTw-ZYvKnje3zfeKO1C7w6-mr5EVK1jpcDQ7Wv76Xoxxs2hiSxkaKxmqUHX](https://lh5.googleusercontent.com/6gGAurxBPIFLuMGrd_svmt6aklx0r8nFYteBpje-H-9fcdxuWYnGZ7kPlkW3atCcfliHZcBn11opyJ8MKDhMl9KRdC_8U5da6arvHhdkZXqztmF-U0p2Sgh3qIZdK-Zs39S9HQTw-ZYvKnje3zfeKO1C7w6-mr5EVK1jpcDQ7Wv76Xoxxs2hiSxkaKxmqUHX)

Thrust provided by each thruster:

[https://lh4.googleusercontent.com/ywFEacLZoEKQlTAwyJ_ESu6i57i-QdoVsNlIeZJR9boBURsgTT2cpKN-yQPxDYpb2aCqsbAyJ6QzMUTVV3C6P6oa1hUxUdd-O12sB6Qql-l1sIbREB9hTCSiRM0F_YvasuaR4vyLK31awX39XIfz7cHP181-oGOHdd6qtoKDGUj-GWPrWngi0mWr7uxAHeOs](https://lh4.googleusercontent.com/ywFEacLZoEKQlTAwyJ_ESu6i57i-QdoVsNlIeZJR9boBURsgTT2cpKN-yQPxDYpb2aCqsbAyJ6QzMUTVV3C6P6oa1hUxUdd-O12sB6Qql-l1sIbREB9hTCSiRM0F_YvasuaR4vyLK31awX39XIfz7cHP181-oGOHdd6qtoKDGUj-GWPrWngi0mWr7uxAHeOs)

Clearly, the thrust output involves too much vibrations.

**Analysis of Eigenvalues:**

When we analyse the change in eigen values while considering the open loop and LQR controlled closed loop systems, for the same values of Q,R we see a clear change in their position in the pole zero plot.

[https://lh5.googleusercontent.com/TdT5Sk5MTqh44NUyDz6es7gH34tVu9P2pVjPehhOyqStNIwLJzbrn1-jrB_vSPVFSKHu6YpL18lp3RsaHXSBjbtweZLeBdoylp1XkSQkQxqhuF6Q7Iak0McYD5N_mxKWDDWK6ge37Es0MlMBvEYSbc3lMQj6W5-EudlV7lOqGqDdJNIcRijpsFAbcqGlxlbK](https://lh5.googleusercontent.com/TdT5Sk5MTqh44NUyDz6es7gH34tVu9P2pVjPehhOyqStNIwLJzbrn1-jrB_vSPVFSKHu6YpL18lp3RsaHXSBjbtweZLeBdoylp1XkSQkQxqhuF6Q7Iak0McYD5N_mxKWDDWK6ge37Es0MlMBvEYSbc3lMQj6W5-EudlV7lOqGqDdJNIcRijpsFAbcqGlxlbK)

Considering the above plot, we can see a clear shift in the eigen values in the direction of the negative real axis, ensuring more stability of the system. As we write down the eigen values or the poles of the two systems, the difference becomes evident.

For   Q = eye(12) & R = 0.1*ones(12) + 0.9*eye(12)

| Open loop poles: | Closed loop poles: |
| --- | --- |
| 0.0000 + 0.0000i      
   0.0000 + 0.0000i
  -0.2740 + 4.3867i
  -0.2740 - 4.3867i
  -0.2633 + 0.0000i
  -0.3003 + 4.3834i
  -0.3003 - 4.3834i
  -0.4067 + 0.0000i
   0.0000 + 0.0000i
   0.0000 + 0.0000i
  -0.4504 + 0.0000i
  -0.4375 + 0.0000i | -6.3274 + 0.0000i
  -3.2047 + 3.1841i
  -3.2047 - 3.1841i
  -3.4503 + 2.9329i
  -3.4503 - 2.9329i
  -0.9222 + 0.4604i
  -0.9222 - 0.4604i
  -0.8836 + 0.4709i
  -0.8836 - 0.4709i
  -1.0104 + 0.0000i
  -0.2170 + 0.0000i
  -0.4043 + 0.0000i |

As it can be observed, in the case of the open loop system, we have four eigen values or poles located at the origin which leads to the fact that the system is marginally stable. Whereas, for the closed loop system, most of  the poles have shifted towards the left side of the imaginary axis with all the poles lying on the left half of the s-plane. This gives enough proof to say that after implementing LQR control, the system has become more stable as compared to the open loop system and hence has helped in increasing performance in terms of reaching the end states.

**Conclusion:**

Given the results of implementing both PID and LQR control on the ROV system, we can see a better response for an LQR control over the PID when we include disturbances in the environment. Also, the LQR control is far more robust in adapting to the change in buoyancy as compared to the PID control, which broadens the spectrum of usage in the former as compared to the latter. When we have changes in the system, using a PID requires tuning all the three gain values for all the control inputs. Tuning the gain values for multiple parameters is not an easy task and there is a very narrow range of values which give the desired results for a specific situation. On the other hand, when we want to adapt to changes while working on LQR control, our tuning is dependent only on the Q,R cost matrices which can help in changing the  relative importance of the states and control inputs for a given circumstance. Here, our goal is achieved by changing the values of the matrices, so as to select the more important factor among the states and inputs,  where we achieve the desired results for a wide range of values which only differ in terms of speed of transient response and steady state error. Another advantage of LQR over PID control that we have come across was the ability to easily control velocity components along with the position coordinates, which would lead to much more complexity in the case of a PID based controller. For simulating in a noisy environment, we used nonlinear PID control where a water current velocity was added as a disturbance; its value being constant with time and we attained appreciable results. Whereas for the LQR setup, we have used a gaussian white noise as a source of disturbance, which gives a better look at the real world scenario. Upon increasing the Q matrix or simply, the cost of attaining the states, we have observed better performance in attaining the states and less erratic, more uniform thrust outputs produced by the thrusters; which is a more reliable result since it is realisable in a real world environment.

However, one drawback of using the LQR control is its applicability to only linear systems, whereas most of the real world scenarios tend to have non linearity in them. However, this problem can be overcome by linearising the system near the fixed points and applying the control.

Hence, we have come to the conclusion that using an LQR control is **far more reliable** than a PID control for the positional and velocity control of a 6-DOF Autonomous Underwater Vehicle.

**Future Work:**

- The model can be implemented with an Adaptive Controller based system which is expected to give better performance.
- We wish to implement our work on the actual physical system to know how it works in the real world. And by doing that, we will be able to understand how the sensor noise affects the system and how the controller will compensate for it.

**Bibliography:**

1. 6-DoF Modelling and Control of a Remotely Operated Vehicle by Chu-Jou Wu, B.Eng. (Electrical) Academic Supervisor: Assoc Prof. Karl Sammut Engineering Discipline College of Science and Engineering Flinders University July, 2018.
2. Lum, C. (2019). Introduction to Linear Quadratic Regulator (LQR) Control. Retrieved from YouTube: https://www.youtube.com/watch?v=wEevt2a4SKI&t=4689