# Finance Module

Utilities for interest, loans, present and future value, and growth calculations.

## Usage

Simply copy the `finance` dir or the `finance.gray` file into your projects structure, Then in the file you plan on using the API write the following at the top of the file:
```gray
import "relative/path/to/finance"
```

Rates are decimals, so 5% is `0.05`. Amounts and years cannot be negative.

Every function returns an `Error` alongside its value. Check the error before using the value; when an error is returned the value is `-1.0e15` and should not be used.

### Interest

```gray
fn main(){
    mut simple, simpleErr = finance.calculate_simple_interest(1000.0, 0.05, 2.0)            // Returns 100.0
    mut compound, compoundErr = finance.calculate_compound_interest(1000.0, 0.05, 2.0, 12)  // Returns 104.941
}
```

### Present and Future Value

```gray
fn main(){
    mut future, futureErr = finance.calculate_future_value(1000.0, 0.05, 10.0)   // Returns 1628.89
    mut present, presentErr = finance.calculate_present_value(1628.89, 0.05, 10.0)   // Returns about 1000.0
    mut npv, npvErr = finance.calculate_net_present_value(0.1, {-1000.0, 500.0, 500.0, 500.0})   // Returns 243.426
}
```

### Growth

```gray
fn main(){
    mut change, changeErr = finance.calculate_percent_change(80.0, 100.0)   // Returns 25.0
    mut growth, growthErr = finance.calculate_compound_annual_growth_rate(100.0, 200.0, 5.0)   // Returns 0.148698
}
```

### Loans

```gray
fn main(){
    mut payment, paymentErr = finance.calculate_loan_payment(200000.0, 0.06, 30.0, 12)   // Returns 1199.10

    mut schedule, scheduleErr = finance.calculate_amortization_schedule(10000.0, 0.06, 1.0, 12)
    if scheduleErr == nil {
        println(schedule[0].interest)   // Returns 50.0
        println(schedule[11].balance)   // Returns 0.0
    }
}
```

### Error Handling

```gray
fn main(){
    mut change, err = finance.calculate_percent_change(0.0, 50.0)
    if err != nil {
        println("Error: ${err}")   // oldValue must not be zero
    }
}
```

## API Reference

| Function | Description |
|----------|-------------|
| `calculate_simple_interest(principal f64, rate f64, years f64) -> (f64, Error)` | Calculates the simple interest earned on the passed principal(principal) at the passed yearly rate(rate) over the passed years(years) |
| `calculate_compound_interest(principal f64, rate f64, years f64, compoundsPerYear i64) -> (f64, Error)` | Calculates the compound interest earned on the passed principal(principal) at the passed yearly rate(rate) over the passed years(years), compounded the passed number of times per year(compoundsPerYear) |
| `calculate_future_value(presentValue f64, rate f64, periods f64) -> (f64, Error)` | Calculates the future value of the passed present value(presentValue) after the passed number of periods(periods) at the passed rate per period(rate) |
| `calculate_present_value(futureValue f64, rate f64, periods f64) -> (f64, Error)` | Calculates the present value of the passed future value(futureValue) received after the passed number of periods(periods) at the passed rate per period(rate) |
| `calculate_net_present_value(rate f64, cashFlows [f64]) -> (f64, Error)` | Calculates the net present value of the passed cash flows(cashFlows) at the passed rate per period(rate). The first cash flow happens now and is not discounted |
| `calculate_percent_change(oldValue f64, newValue f64) -> (f64, Error)` | Calculates the percent change from the passed old value(oldValue) to the passed new value(newValue). Returns 25.0 for a 25% increase. Returns an error if oldValue is zero |
| `calculate_compound_annual_growth_rate(startValue f64, endValue f64, years f64) -> (f64, Error)` | Calculates the compound annual growth rate from the passed start value(startValue) to the passed end value(endValue) over the passed years(years). Returns a decimal, so 0.05 is 5%. Returns an error if startValue or years is zero |
| `calculate_loan_payment(principal f64, rate f64, years f64, paymentsPerYear i64) -> (f64, Error)` | Calculates the payment of a loan of the passed principal(principal) at the passed yearly rate(rate), paid over the passed years(years) in the passed number of payments per year(paymentsPerYear). The total number of payments is rounded to a whole number and must be at least 1 |
| `calculate_amortization_schedule(principal f64, rate f64, years f64, paymentsPerYear i64) -> ([AmortizationPayment], Error)` | Builds the payment schedule of a loan of the passed principal(principal) at the passed yearly rate(rate), paid over the passed years(years) in the passed number of payments per year(paymentsPerYear). The last payment clears the remaining balance |

### AmortizationPayment

One payment of a loan.

| Field | Description |
|-------|-------------|
| `number i64` | The payment number, starting at 1 |
| `payment f64` | The amount paid |
| `interest f64` | How much of the payment went to interest |
| `principalPaid f64` | How much of the payment went to the principal |
| `balance f64` | The balance left after the payment |

### Errors

| Code | Returned when |
|------|---------------|
| `FINANCE_VAL_NEGATIVE` | an amount or number of years is negative |
| `FINANCE_VAL_NOT_A_NUMBER` | a value is NaN |
| `FINANCE_VAL_INFINITE` | a value is infinite |
| `FINANCE_VAL_TOO_LARGE` | a value has a magnitude above `1.0e15`, or years is above 1000 |
| `FINANCE_RATE_OUT_OF_RANGE` | a rate is at or below `-1.0`, or above `1000.0` |
| `FINANCE_INVALID_FREQUENCY` | compoundsPerYear or paymentsPerYear is not from 1 to 366 |
| `FINANCE_INVALID_PERIOD_COUNT` | a loan has fewer than 1 payment |
| `FINANCE_ZERO_VALUE` | oldValue, startValue or years that must not be zero is zero |
| `FINANCE_EMPTY_CASH_FLOWS` | `calculate_net_present_value` is passed no cash flows |
| `FINANCE_RESULT_OUT_OF_RANGE` | the result overflowed |
