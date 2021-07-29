# Arduino-
Measuring freezing point of water at home!
### Here's my code for measuring the freezing/melting point of a solution!

import pyfirmata as pf
import numpy as np
import time
from jupyterplot import ProgressPlot


board = pf.Arduino('/dev/cu.usbmodem14101')
it = pf.util.Iterator(board)
it. start()


analog_0 = board.get_pin('a:0:i')
analog_1 = board.get_pin('a:1:i')


Vk = []
Vb = []
pp = ProgressPlot(x_lim = [0,60], y_lim = [0,2])
for i in range(60):
    time.sleep(1)
    Vk.append(analog_0.read()*5.0)
    Vb.append(analog_1.read()*5.0)
    pp.update(analog_0.read()*5.0)


Vk = np.array(Vk)
Vb = np.array(Vb)
print(Vk)
print(Vb)

Rk = 1000
I = (Vk)/Rk
print(I)



Ru = (Vb-Vk)/I
print (Ru)

T0=0
alpha=0.00392
T=(Ru/1000 - 1)/alpha + T0
print(T)


import matplotlib.pyplot as plt


plt.plot(range(0,len(T)), T, 'b-o')
plt.ylim([-10,10])
plt.xlabel("Time (min)")
plt.ylabel("Temperature (°C)")
plt.show()




