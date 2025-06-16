import matplotlib.pyplot as plt
import numpy as np

# Data

categories = ['Natural wetlands', 'Agriculture & waste', 'Fossil fuel use', 'Biomass/biofuel burning', 'Other natural emissions']
bottom_up_values = [185, 195, 121, 30, 199]
top_down_values = [167, 188, 105, 34, 64]

# Uncertainty ranges (min-max percentage)

bottom_up_uncertainty = [0.4*185, 0.15*195, 0.20*121, 0.30*30, 0.9*199]
top_down_uncertainty = [0.80*167, 0.65*188, 0.50*105, 0.55*34, 1.50*64]

x = np.arange(len(categories)) # the label locations
width = 0.35 # the width of the bars

fig, ax = plt.subplots(figsize=(12, 8))
rects1 = ax.bar(x - width/2, bottom_up_values, width, label='Bottom-up', yerr=bottom_up_uncertainty, capsize=5)
rects2 = ax.bar(x + width/2, top_down_values, width, label='Top-down', yerr=top_down_uncertainty, capsize=5)

# Add some text for labels, title and custom x-axis tick labels, etc.

ax.set_xlabel('Sources of Methane Emissions')
ax.set_ylabel('Methane Emissions (TgCH4/yr)')
ax.set_title('Comparison of Bottom-up and Top-down Methane Emission Budgets')
ax.set_xticks(x)
ax.set_xticklabels(categories)
ax.legend()

# Attach a text label above each bar in rects, displaying its height.

def autolabel(rects):
"""Attach a text label above each bar in _rects_, displaying its height."""
for rect in rects:
height = rect.get_height()
ax.annotate('{}'.format(height),
xy=(rect.get_x() + rect.get_width() / 2, height),
xytext=(0, 3), # 3 points vertical offset
textcoords="offset points",
ha='center', va='bottom')

autolabel(rects1)
autolabel(rects2)

fig.tight_layout()

plt.show()
