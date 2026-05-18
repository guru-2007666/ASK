# ASK & FSK
# Aim
Write a simple Python program for the modulation and demodulation of ASK and FSK.
# Tools required
Google colab
# Thoery
Digital modulation is the process of converting digital data into analog signals for transmission through a communication channel. Two commonly used digital modulation techniques are Amplitude Shift Keying (ASK) and Frequency Shift Keying (FSK).

In ASK, the amplitude of the carrier signal changes according to the binary input signal while the frequency remains constant.

- Binary `1` is represented by the presence of the carrier signal.
- Binary `0` is represented by the absence or reduction of the carrier signal.

In FSK, the frequency of the carrier signal changes according to the binary input signal while the amplitude remains constant.

- Binary `1` is transmitted using one frequency.
- Binary `0` is transmitted using another frequency.

The ASK signal is represented as:

$$
s(t)=m(t)A_c\sin(2\pi f_c t)
$$

The FSK signal is represented as:

$$
s(t)=A_c\sin(2\pi f_1 t)\quad \text{for binary 1}
$$

$$
s(t)=A_c\sin(2\pi f_2 t)\quad \text{for binary 0}
$$

At the receiver side, the modulated signals are demodulated to recover the original binary data. In this experiment, Python is used to generate the message signal, carrier signal, ASK/FSK modulated signals, and the recovered output signals.
Where:
$m(t)$ = Message signal

$A_c$​ = Carrier amplitude

$f_c$​ = Carrier frequency


At the receiver side, the ASK signal is demodulated by multiplying it with the carrier signal and passing it through a low-pass filter to recover the original binary data.
In this experiment, Python is used to generate the message signal, carrier signal, ASK modulated signal, and decoded output waveform.
# Program
## ASK
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Low-pass filter
def lpf(x, fc, fs):
    b, a = butter(4, fc/(0.5*fs), 'low')
    return lfilter(b, a, x)

# Parameters
fs, fc, br, T = 1000, 50, 10, 1
t = np.arange(0, T, 1/fs)

# Message signal
bits = np.random.randint(0, 2, br)
msg = np.repeat(bits, fs//br)

# Carrier signal
carrier = np.sin(2*np.pi*fc*t)

# ASK modulation & demodulation
ask = msg * carrier
demod = lpf(ask * carrier, fc, fs)
decoded = (demod[::fs//br] > 0.25).astype(int)

# Plot
plt.figure(figsize=(10,9))
plt.suptitle("NAME :BHUVANESH P\nREG NO : 212224060047",fontsize=12, fontweight='bold')

plt.subplot(4,1,1)
plt.plot(t, msg)
plt.title("Message Signal")

plt.subplot(4,1,2)
plt.plot(t, carrier)
plt.title("Carrier Signal")

plt.subplot(4,1,3)
plt.plot(t, ask)
plt.title("ASK Modulated Signal")

plt.subplot(4,1,4)
plt.step(range(len(decoded)), decoded, where='mid')
plt.title("Decoded Bits")

plt.tight_layout(rect=[0,0,1,0.93])
plt.show()

```
## FSK
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Low-pass filter
def lpf(x, fc, fs):
    b, a = butter(4, fc/(0.5*fs), 'low')
    return lfilter(b, a, x)

# Parameters
fs, f1, f2, br, T = 1000, 30, 70, 10, 1
t = np.arange(0, T, 1/fs)
bd = fs // br

# Message signal
bits = np.random.randint(0, 2, br)
msg = np.repeat(bits, bd)

# Carrier signals
c1 = np.sin(2*np.pi*f1*t)
c2 = np.sin(2*np.pi*f2*t)

# FSK Modulation
fsk = np.zeros_like(t)
for i, b in enumerate(bits):
    fsk[i*bd:(i+1)*bd] = np.sin(2*np.pi*(f2 if b else f1)*t[i*bd:(i+1)*bd])

# Demodulation (correlation)
d1 = lpf(fsk * c1, f1, fs)
d2 = lpf(fsk * c2, f2, fs)

dec = [(np.sum(d2[i*bd:(i+1)*bd]**2) >
        np.sum(d1[i*bd:(i+1)*bd]**2)) for i in range(br)]
demod = np.repeat(dec, bd)

# Plot
plt.figure(figsize=(10,10))
plt.suptitle("NAME :BHUVANESH P\nREG NO : 212224060047",fontsize=12, fontweight='bold')

plt.subplot(5,1,1); plt.plot(t, msg); plt.title("Message Signal")
plt.subplot(5,1,2); plt.plot(t, c1); plt.title("Carrier f1 (bit 0)")
plt.subplot(5,1,3); plt.plot(t, c2); plt.title("Carrier f2 (bit 1)")
plt.subplot(5,1,4); plt.plot(t, fsk); plt.title("FSK Modulated Signal")
plt.subplot(5,1,5); plt.plot(t, demod); plt.title("Demodulated Signal")

plt.tight_layout(rect=[0,0,1,0.93])
plt.show()
```

# Output Waveform
## ASK
<img width="978" height="887" alt="image" src="https://github.com/user-attachments/assets/bdaa2781-7e67-42c3-abf5-6edf53f6463e" />


## FSK
<img width="989" height="985" alt="image" src="https://github.com/user-attachments/assets/ebe3b81a-b8ba-4994-b835-c6c28c7fb161" />


# Results
Thus the ASK AND FSK experiment has been completed and it is verified successfully
