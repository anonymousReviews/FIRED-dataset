# The Fully-labeled hIgh-fRequencyElectricity Disaggregation (FIRED) dataset

Python module to load and interact with the FIRED dataset. 
The files contain scripts to generate several statistics and plots from the data.

# Dataset Info

- voltage and current wave-forms of a three-room apartment in Germany 
- 32 days of recording
- 21 individual appliance readings at 2kHz
- aggregated readings from appartment's mains at 8kHz
- data stored in matroska multimedia container as audio stream
- additional sensor readings (temp, hum), lighting states (on/of, dimm, color) and device information available
- 50Hz and 1Hz data summary with derived active, reactive and apparent power readings available 
- 99.98% data availability (missing data filled with zeros to maintain timestamps)


## Download

The server hosting the dataset is omitted for blind review.
For a sample of the dataset, use the following link: [click](https://xfl.jp/aHhWNi)

It provides 4 days 1Hz readings, 1 day 50 Hz readings and 20 min high freq readings + 32 days annotation data.

## Installation

Required python packages: numpy, pandas, scipy, json and pyav.
To install ```pyav```:
```bash
git clone https://github.com/pscholl/PyAV.git
cd PyAv
git checkout origin/phil
python3 setup.py install
```

## How to Use

The helper module makes using the dataset a breeze.
```python
import helper as hp

# load 1Hz power data of the television
startTs,  stopTs = hp.getRecordingRange("2020.07.13", "2020.07.16")
television = hp.getPower("television", 1, startTs, stopTs)

# load 50Hz power data of powermeter09 (Fridge) of day 2020.07.15
startTs,  stopTs = hp.getRecordingRange("2020.07.15", "2020.07.16")
fridge = hp.getMeterPower("powermeter09", 50, startTs=startTs, stopTs=end)
```

Plotting the data is straightforward:
```python
import matplotlib.pyplot as plt
import numpy as np
from datetime import datetime

start = fridge["timestamp"]
end = start+(len(fridge["data"])/fridge["samplingrate"])
timestamps = np.linspace(start, end, len(fridge["data"])
dates = [datetime.fromtimestamp(ts) for ts in timestamps]

fig, ax = plt.subplots()
ax.plot(dates, fridge["data"]["p"], label="active power")
ax.plot(dates, fridge["data"]["q"], label="reactive power")

ax.set(xlabel='Time of day', ylabel='Power (W/var)', title='Fridge')
fig.autofmt_xdate()
plt.show()

```
