# Haptic-
Low pass signal

import plotly.graph_objs as go
import plotly.figure_factory as ff

import numpy as np
import pandas as pd
import scipy

from scipy import signal

import matplotlib.pyplot as plt

import chart_studio.plotly as py

import chart_studio.plotly as py
import plotly.graph_objs as go
import plotly.figure_factory as ff

import numpy as np
import pandas as pd
import scipy

from scipy import signal

data = pd.read_csv('HAPTD12.csv')
df = data[100:2000]

print (df)

#table = ff.create_table(df)
#py.iplot(table, filename='HAPTIC7-data')
#print(HAPTIC7-data)

X = data['2']
A = data['1']
P = data['3']
#print (X)
Y = X[100:2000]
B = A[100:2000]
Q = P[100:2000]
print(Y)
trace1 = go.Scatter(
    x=list(range(len(list(Y)))),
    y=list(Y),
    mode='lines',
    name='Y-Haptic Data'
)
trace2 = go.Scatter(
    x=list(range(len(list(B)))),
    y=list(B),
    mode='lines',
    name='X-Haptic Data'
)
trace3 = go.Scatter(
    x=list(range(len(list(Q)))),
    y=list(Q),
    mode='lines',
    name='Z-Haptic Data'
)

layout = go.Layout(
    showlegend=True
)

trace_data = [trace1, trace2, trace3]
fig = go.Figure(data=trace_data, layout=layout)
fig.show()
#py.iplot(fig, filename='HAPTIC7-plot')
test= Y
#print(test)

fc = 0.001
b = 0.1
N = int(np.ceil((0.5 / b)))
if not N % 2: N += 1
n = np.arange(N)

sinc_func = np.sinc(2 * fc * (n - (N - 1) / 2))
window = 0.42 - 0.5 * np.cos(2 * np.pi * n / (N - 1)) + 0.08 * np.cos(4 * np.pi * n / (N - 1))
sinc_func = sinc_func * window
sinc_func = sinc_func / np.sum(sinc_func)

s = list(Y)
t = list(B)
u = list(Q)
#print(s)
new_signal = np.convolve(list(map(float,s)), sinc_func)
new_signal1 = np.convolve(list(map(float,t)), sinc_func)
new_signal2 = np.convolve(list(map(float,u)), sinc_func)

# Simplifying for calculating "G"
G = 62.33
new_signal = (new_signal * 1000)/G
new_signal1 = (new_signal1 * 1000)/G
new_signal2 = (new_signal2 * 1000)/G
print(new_signal)


# sliced array on X-axis
offset_start = 200
offset_stop = -200
new_signal = new_signal[offset_start:offset_stop]
new_signal1 = new_signal1[offset_start:offset_stop]
new_signal2 = new_signal2[offset_start:offset_stop]

# move data to zero on Y-axis
new_signal = new_signal-new_signal[0]
new_signal1 = new_signal1-new_signal1[0]
new_signal2 = new_signal2-new_signal2[0]


import plotly.graph_objects as go

trace1 = go.Scatter(
    x=list(range(len(new_signal1))),
    y=new_signal1,
    mode='lines',
    name='X-axis',
    marker=dict(
        color='#4cbfc5' #blue
    )
)

trace2 = go.Scatter(
    x=list(range(len(new_signal))),
    y=new_signal,
    mode='lines',
    name='Y-axis',
    marker=dict(
        color='#C54C82'  #pink
    )
)


trace3 = go.Scatter(
    x=list(range(len(new_signal2))),
    y=new_signal2,
    mode='lines',
    name='Z-axis',
    marker=dict(
        color='#abc54c'  #green
    )
)

layout = go.Layout(
    title='Low-Pass Filter',
    showlegend=True
)

trace_data = [trace1, trace2, trace3]
fig = go.Figure(data=trace_data, layout=layout)
fig.show()


# smooth data
new_signal = signal.savgol_filter(new_signal, 53, 3)
new_signal1 = signal.savgol_filter(new_signal1, 53, 3)
new_signal2 = signal.savgol_filter(new_signal2, 53, 3)


trace1 = go.Scatter(
    x=list(range(len(new_signal))),
    y=new_signal,
    mode='lines',
    name='X-axis',
    marker=dict(
        color='#4cbfc5' #blue
    )
)

trace2 = go.Scatter(
    x=list(range(len(new_signal1))),
    y=new_signal1,
    mode='lines',
    name='Y-axis',
    marker=dict(
        color='#C54C82'  #pink
    )
)


trace3 = go.Scatter(
    x=list(range(len(new_signal2))),
    y=new_signal2,
    mode='lines',
    name='Z-axis',
    marker=dict(
        color='#abc54c'  #green
    )
)

layout = go.Layout(
    title='Low-Pass Filter',
    showlegend=True
)

trace_data = [trace1, trace2, trace3]
fig = go.Figure(data=trace_data, layout=layout)
fig.show()

#Calculate the G value
G= np.ptp(new_signal, axis=0)
G1= np.ptp(new_signal1, axis=0)
G2= np.ptp(new_signal2, axis=0)
print(G)
print(G1)
print(G2)





