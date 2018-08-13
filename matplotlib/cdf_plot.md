# Plot CDF of Values

```python
sortedvalues = np.sort(temp)
p = 1. * np.arange(len(temp))/(len(temp) - 1)
plt.plot(sortedvalues, p)
```
