# Report: Key Insights from Public Transport Data

This analysis covers daily ridership data from July 2019 to September 2024.

### 1. The Overwhelming Impact of the COVID-19 Pandemic
The single most significant event in the dataset is the COVID-19 pandemic. A dramatic and unprecedented drop in ridership is visible starting in March 2020. Total daily ridership fell from over 80,000 to under 10,000. This is followed by a slow, multi-year recovery. Any forecasting model *must* account for this structural break.

### 2. Clear Service Hierarchy
The transport services are not used equally. The `Rapid Route` is consistently the most-used service, closely followed by the `Local Route`. `Light Rail` is a strong third. In contrast, `School`, `Peak Service`, and `Other` are niche services with much lower passenger volumes.

### 3. Powerful Weekly Seasonality
A distinct 7-day (weekly) pattern dominates the data. All services see a massive decline in usage on Saturdays and Sundays. This is especially true for:
* **`School` Service:** Drops to or near zero on weekends.
* **`Peak Service`:** Also drops to zero, confirming it is a weekday, rush-hour-only service.
This robust pattern is a key predictor for forecasting.

### 4. Specialized Service Behavior
The `School` service demonstrates the most predictable pattern. Its ridership is high on weekdays but disappears entirely on weekends and for extended periods. These periods align perfectly with summer holidays (approx. July-August) and winter breaks (late December), making it highly dependent on the academic calendar.

### 5. Annual Holiday Dips
Beyond the major COVID-19 event, the most consistent drops in ridership occur during major public holidays. There are sharp, clear dips in ridership across *all* services every year around December 25th (Christmas) and January 1st (New Year's Day). This highlights the need to include a holiday calendar as a feature in a predictive model.