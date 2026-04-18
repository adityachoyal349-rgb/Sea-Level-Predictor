# Sea-Level-Predictor

import pandas as pd
import matplotlib.pyplot as plt
from scipy.stats import linregress


def draw_plot():
    # 1. Import data
    df = pd.read_csv('epa-sea-level.csv')

    fig, ax = plt.subplots(figsize=(12, 6))

    # 2. Scatter plot
    ax.scatter(df['Year'], df['CSIRO Adjusted Sea Level'], alpha=0.6, s=20)

    # 3. Line of best fit — all data (1880 → 2050)
    x_all = range(df['Year'].min(), 2051)
    slope1, intercept1, *_ = linregress(df['Year'], df['CSIRO Adjusted Sea Level'])
    ax.plot(x_all, [slope1 * x + intercept1 for x in x_all],
            color='red', label='Best fit (all data)')

    # 4. Line of best fit — from 2000 → 2050
    df_2000 = df[df['Year'] >= 2000]
    x_recent = range(2000, 2051)
    slope2, intercept2, *_ = linregress(df_2000['Year'], df_2000['CSIRO Adjusted Sea Level'])
    ax.plot(x_recent, [slope2 * x + intercept2 for x in x_recent],
            color='green', label='Best fit (2000–present)')

    # 5. Labels, title, legend
    ax.set_xlabel('Year')
    ax.set_ylabel('Sea Level (inches)')
    ax.set_title('Rise in Sea Level')
    ax.legend()

    fig.savefig('sea_level_plot.png')
    return ax
    
