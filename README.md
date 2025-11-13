📘 VARI-MU COMP METASOUND PATCH
Vintage Tube Compressor Reconstruction inside Unreal MetaSound

Version: 1.0.0

Author: KANG



★ Overview

This MetaSound pack recreates the behavior of a vintage Fairchild-style vari-mu tube compressor by modeling the functional behavior of its analog components using MetaSound nodes.

The system simulates:

○ Input Transformer coloring & gain staging

○ Variable-Mu Tube Amplification (compression curve + saturation)

○ Program-Dependent Time Constant Network (6-mode release characteristic)

○ Output Transformer tone shaping

○ Detector / Control Voltage Path (envelope behavior)

The goal of this project is to demonstrate how MetaSound can be used not only for DSP-style processing, but also to rebuild analog audio hardware behaviors inside the Unreal Engine audio ecosystem.



★ Features

✔ Reconstructed Analog Behavior

Modeled from the key building blocks of the Fairchild 660 compressor:

○ Input Transformer (T1)
Adds musical pre-coloration & interacts with Input Gain

○ Mu-Variable Tube Stage
Emulates increasing compression ratio as level rises (vari-mu curve)

○ Sidechain Detector
Peak/RMS behavior, smoothing, knee calculation

○ Program-Dependent Time Constant Network
Switchable 1–6 modes with automatic fast/slow release blending

○ Output Transformer Coloring
Subtle harmonic warmth using waveshapers and gain staging

✔ Full User-Controllable Interface

Exposed parameters allow real-time control just like the hardware front panel.

✔ Soft/Hard Knee, Parallel Mix, Makeup Gain

Modern workflow features added while preserving vintage behavior.

✔ 100% MetaSound Native

No external DSP libraries required. Works in UE 5.4.




★ Analog Component → MetaSound Mapping


Analog Component	Function in Hardware	MetaSound Patch Equivalent
○ Input Transformer	: Impedance matching, saturation, pre-emphasis	MP_INPUT / InputGain_dB × DecibelsToLinearGain
○ PAD / Input Gain Network	: Adjust incoming level before tube	INPUT_GAIN_DB parameter controlling gain staging
○ Variable-Mu Tubes	: Level-dependent gain reduction, rising ratio	MP_VARIMU gain computer (vari-mu curve, knee, GR mapping)
○ Sidechain Rectifier	: Converts audio to control voltage	MP_DETECTOR (Peak/RMS smoothing, envelope follower)
○ Time Constant Network (Mode 1–6)	: Program-dependent release (dual time)	MP_TIMECONST + MP_TIMECONST_MODE_TABLE
○ Output Tube + Transformer	: Final tone shaping, harmonic coloration	MP_COLOR (waveshaper + transformer-like soft saturation)




★ Patch Architecture

The full compressor is composed of modular MetaSound patches:

MP_VMC (Main Patch)
│
├── MP_INPUT                // Input transformer, gain staging
├── MP_DETECTOR             // Peak/RMS envelope + smoothing
├── MP_TIMECONST            // Fast/Slow release blending
├── MP_TCMODE               // Mode 1–6 preset values
├── MP_VARIMU               // Vari-mu gain computer
├── MP_MAKEUP               // Output saturation / transformer tone
└── MP_PARALLELMIX          // Parallel compression mix


Each component can be opened and studied individually to understand how analog behavior is reconstructed using MetaSound nodes.




★ User-Adjustable Parameters

● Input Gain (dB)

Controls how hard the signal hits the tube stage.
Acts like the PAD/Pre-gain of the real Fairchild.

● Threshold (dB)

Level at which compression begins.
Lower threshold = more compression.

● Time Constant Mode (1–6)

Recreates the Fairchild’s 6 release profiles.
Each mode blends fast and slow release components.


● Ratio

Controls base compression intensity.
Internally multiplied by the vari-mu curve to mimic tube behavior.

● Knee (dB)

Softens the transition into compression.
0 dB = hard knee, 6–12 dB = soft vintage knee.


● Makeup Gain (dB)

Restores gain lost during compression.

● Mix (Dry/Wet)

Allows parallel compression.




★ Usage Example

Add an Audio Component to your Actor.

Assign a Wave Asset.

Replace the output MetaSound with new patch.

Adjust:

○ Input Gain

○ Threshold

○ Time Constant Mode

○ Ratio / Knee / Mix

Play and listen to vari-mu compression inside Unreal.




★ Technical Notes

Linear gain factors always clamped to avoid positive GR.

Knee and Ratio computations follow variable-mu behavior.

Time Mode derives from lookup table + MapRange interpolation.

Built for UE 5.4 MetaSound architecture.

Zero external dependencies.




★ Troubleshooting

No compression happening

Check input gain level

Ensure detector path connected

Verify threshold < source level

Audio gets louder when knee = 0

Knee minimum is clamped internally

Ensure GR_dB stays ≤ 0

TimeConstant not changing

Mode table must be connected to MP_TIMECONST




★ Support

For bug reports or questions:
rkdvlfrn2@naver.com
