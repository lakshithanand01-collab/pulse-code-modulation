# pulse-code-modulation
# Aim
Write a simple Python program for the modulation and demodulation of PCM, and DM.
# Tools required

Tools Required
Google Colab
Python
NumPy Library
Matplotlib Library
Internet Connection
Computer / Laptop

# Theory 
Pulse Code Modulation (PCM) is a technique used to convert analog signals into digital form. It involves three steps: sampling, quantization, and encoding.
In sampling, the signal is taken at regular intervals based on the Nyquist Sampling Theorem. Quantization converts sampled values into discrete levels, introducing small error. Encoding represents each quantized value as a binary code. At the receiver, decoding reconstructs the signal approximately.
Delta Modulation (DM) is a simplified method that encodes only the change in signal. It uses 1 for increase and 0 for decrease, making it simple but less accurate than PCM.

# Program

import numpy as np
import matplotlib.pyplot as plt

# -----------------------------------
# Step 1: Generate Analog Input Signal
# -----------------------------------

fm = 2                  # Message signal frequency (Hz)
fs = 20                 # Sampling frequency (Hz)
duration = 2            # Duration in seconds
n_bits = 3              # Number of bits for PCM

t = np.linspace(0, duration, 1000)     # Continuous time axis
ts = np.arange(0, duration, 1/fs)      # Sampled time axis

# Original analog signal (Sine wave)
x = np.sin(2 * np.pi * fm * t)

# Sampled signal
xs = np.sin(2 * np.pi * fm * ts)

# -----------------------------------
# Step 2: Quantization
# -----------------------------------

L = 2 ** n_bits         # Number of quantization levels
x_min = -1
x_max = 1

delta = (x_max - x_min) / L

# Uniform Quantization
xq = np.round((xs - x_min) / delta) * delta + x_min
xq = np.clip(xq, x_min, x_max)

# -----------------------------------
# Step 3: PCM Encoding
# -----------------------------------

indices = ((xq - x_min) / delta).astype(int)
indices = np.clip(indices, 0, L - 1)

binary_codes = [format(i, f'0{n_bits}b') for i in indices]

print("Sampled Value\tQuantized Value\tPCM Code")
print("------------------------------------------------")
for i in range(len(xs)):
    print(f"{xs[i]:.3f}\t\t{xq[i]:.3f}\t\t{binary_codes[i]}")

# -----------------------------------
# Step 4: Create PCM Pulse Waveform
# -----------------------------------

pcm_bits = ""

for code in binary_codes:
    pcm_bits += code

pcm_wave = [int(bit) for bit in pcm_bits]

bit_time = np.arange(len(pcm_wave))

# -----------------------------------
# Step 5: PCM Decoding (Demodulation)
# -----------------------------------

decoded_indices = np.array([int(code, 2) for code in binary_codes])
decoded_signal = decoded_indices * delta + x_min

# -----------------------------------
# Step 6: Plot All Signals
# -----------------------------------

plt.figure(figsize=(12, 14))

# Original Analog Signal
plt.subplot(5, 1, 1)
plt.plot(t, x)
plt.title("Original Analog Signal")
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.grid(True)

# Sampled Signal
plt.subplot(5, 1, 2)
plt.stem(ts, xs, basefmt=" ")
plt.title("Sampled Signal")
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.grid(True)

# Quantized Signal
plt.subplot(5, 1, 3)
plt.stem(ts, xq, basefmt=" ")
plt.title("Quantized Signal")
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.grid(True)

# PCM Binary Pulse Waveform
plt.subplot(5, 1, 4)
plt.step(bit_time, pcm_wave, where='post')
plt.ylim(-0.2, 1.2)
plt.title("PCM Output Waveform (Binary Pulse Train)")
plt.xlabel("Bit Position")
plt.ylabel("Binary Level")
plt.grid(True)

# Demodulated Signal
plt.subplot(5, 1, 5)
plt.step(ts, decoded_signal, where='mid')
plt.title("PCM Demodulated (Reconstructed) Signal")
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.grid(True)

plt.tight_layout()
plt.show()

# Output Waveform
<img width="1150" height="996" alt="Screenshot 2026-04-29 083019" src="https://github.com/user-attachments/assets/fdcc6983-79d6-4b94-bd57-88a6e127ffb3" />
<img width="1608" height="347" alt="Screenshot 2026-04-29 083033" src="https://github.com/user-attachments/assets/23181a8f-88b0-47ba-a352-a82137ad5b88" />

# Results

Thus the simple Python program for the modulation and demodulation of PCM, and DM is simulated and verified.

