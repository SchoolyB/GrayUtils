# Stats Module

Utilities for calculating descriptive statistics over arrays of `f64` values.

## Usage

Simply copy the `stats` dir or the `stats.gray` file into your projects structure, Then in the file you plan on using the API write the following at the top of the file:
```gray
import "relative/path/to/stats"
```

Every function returns `(f64, Error)`. Check the error before using the value; when an error is returned the value is `-1.0e100` and should not be used.

### Mean, Median and Mode

```gray
fn main(){
    mut data [f64] = {2.0, 4.0, 4.0, 4.0, 5.0, 5.0, 7.0, 9.0}

    mut average, averageErr = stats.mean(data)     // Returns 5.0
    mut middle, middleErr = stats.median(data)     // Returns 4.5
    mut common, commonErr = stats.mode(data)       // Returns 4.0
}
```

### Variance and Standard Deviation

```gray
fn main(){
    mut data [f64] = {2.0, 4.0, 4.0, 4.0, 5.0, 5.0, 7.0, 9.0}

    mut sample, sampleErr = stats.variance(data)                 // Returns 4.571428571428571
    mut population, populationErr = stats.population_variance(data)   // Returns 4.0
    mut deviation, deviationErr = stats.stddev(data)             // Returns 2.138089935299395
    mut populationDeviation, populationDeviationErr = stats.population_stddev(data)   // Returns 2.0
}
```

### Percentile

```gray
fn main(){
    mut data [f64] = {2.0, 4.0, 4.0, 4.0, 5.0, 5.0, 7.0, 9.0}

    mut ninetieth, ninetiethErr = stats.percentile(data, 90.0)   // Returns 7.6
}
```

### Error Handling

```gray
fn main(){
    mut empty [f64] = {}
    mut average, err = stats.mean(empty)
    if err != nil {
        println("Error: ${err}")   // Data is empty
    }
}
```

## API Reference

| Function | Description |
|----------|-------------|
| `mean(data [f64]) -> (f64, Error)` | Calculates the mean of the passed data(data) |
| `median(data [f64]) -> (f64, Error)` | Calculates the median of the passed data(data). Averages the two middle values when data has an even count |
| `mode(data [f64]) -> (f64, Error)` | Finds the mode of the passed data(data). Returns the smallest value when several values occur equally often |
| `variance(data [f64]) -> (f64, Error)` | Calculates the sample variance of the passed data(data), dividing by one less than the count. Needs at least 2 values |
| `population_variance(data [f64]) -> (f64, Error)` | Calculates the population variance of the passed data(data), dividing by the count |
| `stddev(data [f64]) -> (f64, Error)` | Calculates the sample standard deviation of the passed data(data). Needs at least 2 values |
| `population_stddev(data [f64]) -> (f64, Error)` | Calculates the population standard deviation of the passed data(data) |
| `percentile(data [f64], percent f64) -> (f64, Error)` | Calculates the percentile(percent) of the passed data(data) by linear interpolation. percent must be from 0.0 to 100.0 |

### Errors

| Code | Returned when |
|------|---------------|
| `STATS_EMPTY_DATA` | data is empty |
| `STATS_NOT_ENOUGH_DATA` | data has fewer values than the function needs |
| `STATS_VAL_NOT_A_NUMBER` | data holds a NaN |
| `STATS_VAL_INFINITE` | data holds an infinite value |
| `STATS_VAL_TOO_LARGE` | data holds a value with a magnitude above `1.0e100` |
| `STATS_PERCENT_OUT_OF_RANGE` | percent is NaN or outside 0.0 to 100.0 |
| `STATS_RESULT_OUT_OF_RANGE` | the result overflowed |
