# Units Module

Utilities for converting between metric and imperial units of measurement.

## Usage

Simply copy the `units` dir into your projects structure, Then in the file you plan on using the API write the following at the top of the file:

```gray
import "relative/path/to/units"
```

### Error Handling

Every function returns `(f64, Error)`. Check the error before using the value; when an error is returned the value is 0.0 and should not be used.

```gray
fn main(){
    mut km, err = units.mile_to_kilometer(-5.0)
    if err != nil {
        println("Error: ${err}")   // Value -5.0 is negative, min is 0.0
    }
}
```

Which error occurred is checked on `err.code`:

```gray
when err.code {
    is .LENGTH_VAL_NEGATIVE { println("negative length") }
    default                 { println(err.msg) }
}
```

| Module | Error codes |
|--------|-------------|
| Length | `LENGTH_VAL_NEGATIVE`, `LENGTH_VAL_NOT_A_NUMBER`, `LENGTH_VAL_INFINITE`, `LENGTH_VAL_TOO_LARGE` |
| Area | `AREA_VAL_NEGATIVE`, `AREA_VAL_NOT_A_NUMBER`, `AREA_VAL_INFINITE`, `AREA_VAL_TOO_LARGE` |
| Weight | `WEIGHT_VAL_NEGATIVE`, `WEIGHT_VAL_NOT_A_NUMBER`, `WEIGHT_VAL_INFINITE`, `WEIGHT_VAL_TOO_LARGE` |
| Temperature | `TEMPERATURE_VAL_BELOW_ABSOLUTE_ZERO`, `TEMPERATURE_VAL_NOT_A_NUMBER`, `TEMPERATURE_VAL_INFINITE`, `TEMPERATURE_VAL_TOO_LARGE` |
| Volume | `VOLUME_VAL_NEGATIVE`, `VOLUME_VAL_NOT_A_NUMBER`, `VOLUME_VAL_INFINITE`, `VOLUME_VAL_TOO_LARGE` |
| Speed | `SPEED_VAL_NEGATIVE`, `SPEED_VAL_NOT_A_NUMBER`, `SPEED_VAL_INFINITE`, `SPEED_VAL_TOO_LARGE` |
| Data Storage | `VAL_NEGATIVE`, `VAL_NOT_A_NUMBER`, `VAL_INFINITE`, `VAL_TOO_LARGE_FOR_BASE` |
| Physics | `PHYSICS_VAL_NEGATIVE`, `PHYSICS_VAL_ZERO`, `PHYSICS_VAL_NOT_A_NUMBER`, `PHYSICS_VAL_INFINITE`, `PHYSICS_VAL_TOO_LARGE`, `PHYSICS_VAL_TOO_SMALL`, `PHYSICS_RESULT_OUT_OF_RANGE` |

### Length Conversions

```gray
fn main(){
    mut km, kmErr = units.mile_to_kilometer(5.0)         // Returns 8.04672
    mut feet, feetErr = units.meter_to_feet(10.0)          // Returns 32.8084
    mut inches, inchesErr = units.centimeter_to_inch(2.54)   // Returns 1.0
}
```

### Area Conversions

```gray
fn main(){
    mut sqm, sqmErr = units.sq_foot_to_sq_meter(100.0)    // Returns 9.2903
    mut acres, acresErr = units.hectare_to_acre(1.0)        // Returns 2.47105
    mut area, areaErr = units.calculate_circle_area(5.0)   // Returns 78.5398
}
```

### Weight Conversions

```gray
fn main(){
    mut kg, kgErr = units.convert_pound_to_kilogram(150.0)           // Returns 68.0388
    mut oz, ozErr = units.convert_gram_to_ounce(100.0)               // Returns 3.5274
    mut tons, tonsErr = units.convert_kilogram_to_metric_ton(1000.0)   // Returns 1.0
}
```

### Temperature Conversions

```gray
fn main(){
    mut f, fErr = units.convert_celsius_to_fahrenheit(100.0)   // Returns 212.0
    mut c, cErr = units.convert_fahrenheit_to_celsius(32.0)    // Returns 0.0
    mut k, kErr = units.convert_celsius_to_kelvin(0.0)         // Returns 273.15
}
```

### Volume Conversions

```gray
fn main(){
    mut liters, litersErr = units.convert_gallon_to_liter(1.0)               // Returns 3.78541
    mut cups, cupsErr = units.convert_liter_to_cup(1.0)                    // Returns 4.22675
    mut ml, mlErr = units.convert_fluid_ounce_to_milliliter(8.0)         // Returns 236.588
}
```

### Speed Conversions

```gray
fn main(){
    mut kmh, kmhErr = units.miles_per_hour_to_kilometers_per_hour(60.0)        // Returns 96.5604
    mut knots, knotsErr = units.kilometers_per_hour_to_knot(100.0)               // Returns 53.9957
    mut mps, mpsErr = units.feet_per_second_to_meters_per_second(10.0)         // Returns 3.048
}
```

### Data Storage Conversions

```gray
fn main(){
    mut gb, gbErr = units.megabyte_to_gigabyte(1000.0)    // Returns 1.0 (SI)
    mut gib, gibErr = units.mebibyte_to_gibibyte(1024.0)   // Returns 1.0 (IEC)
    mut kb, kbErr = units.kibibyte_to_kilobyte(1.0)       // Returns 1.024
}
```

### Physics Calculations

```gray
fn main(){
    mut pressure, pressureErr = units.calculate_pressure(100.0, 4.0)   // Returns 25.0
    mut force, forceErr = units.calculate_force(2.0, 3.0)              // Returns 6.0
    mut acceleration, accelerationErr = units.calculate_acceleration_from_rest(2.0, 10.0)   // Returns 5.0
}
```

## API Reference

### Length Functions

#### Metric to Metric

| Function | Description |
|----------|-------------|
| `centimeter_to_meter(val f64) -> (f64, Error)` | Converts the passed centimeters(val) to meters |
| `meter_to_centimeter(val f64) -> (f64, Error)` | Converts the passed meters(val) to centimeters |
| `meter_to_kilometer(val f64) -> (f64, Error)` | Converts the passed meters(val) to kilometers |
| `kilometer_to_meter(val f64) -> (f64, Error)` | Converts the passed kilometers(val) to meters |

#### Imperial to Imperial

| Function | Description |
|----------|-------------|
| `inch_to_feet(val f64) -> (f64, Error)` | Converts the passed inches(val) to feet |
| `foot_to_inch(val f64) -> (f64, Error)` | Converts the passed feet(val) to inches |
| `feet_to_yard(val f64) -> (f64, Error)` | Converts the passed feet(val) to yards |
| `yard_to_feet(val f64) -> (f64, Error)` | Converts the passed yards(val) to feet |
| `yard_to_mile(val f64) -> (f64, Error)` | Converts the passed yards(val) to miles |
| `mile_to_yard(val f64) -> (f64, Error)` | Converts the passed miles(val) to yards |
| `feet_to_mile(val f64) -> (f64, Error)` | Converts the passed feet(val) to miles |
| `mile_to_feet(val f64) -> (f64, Error)` | Converts the passed miles(val) to feet |

#### Imperial to Metric

| Function | Description |
|----------|-------------|
| `inch_to_centimeter(val f64) -> (f64, Error)` | Converts the passed inches(val) to centimeters |
| `inch_to_meter(val f64) -> (f64, Error)` | Converts the passed inches(val) to meters |
| `feet_to_meter(val f64) -> (f64, Error)` | Converts the passed feet(val) to meters |
| `feet_to_centimeter(val f64) -> (f64, Error)` | Converts the passed feet(val) to centimeters |
| `yard_to_meter(val f64) -> (f64, Error)` | Converts the passed yards(val) to meters |
| `mile_to_kilometer(val f64) -> (f64, Error)` | Converts the passed miles(val) to kilometers |
| `mile_to_meter(val f64) -> (f64, Error)` | Converts the passed miles(val) to meters |

#### Metric to Imperial

| Function | Description |
|----------|-------------|
| `centimeter_to_inch(val f64) -> (f64, Error)` | Converts the passed centimeters(val) to inches |
| `meter_to_inch(val f64) -> (f64, Error)` | Converts the passed meters(val) to inches |
| `meter_to_feet(val f64) -> (f64, Error)` | Converts the passed meters(val) to feet |
| `meter_to_yard(val f64) -> (f64, Error)` | Converts the passed meters(val) to yards |
| `kilometer_to_mile(val f64) -> (f64, Error)` | Converts the passed kilometers(val) to miles |
| `kilometer_to_feet(val f64) -> (f64, Error)` | Converts the passed kilometers(val) to feet |

### Area Functions

#### Shape Area Calculations

| Function | Description |
|----------|-------------|
| `calculate_rectangle_area(l, w f64) -> (f64, Error)` | Calculates the area of a rectangle with the passed in length(l) and width(w) |
| `calculate_square_area(s f64) -> (f64, Error)` | Calculates the area of a square with the passed in side's(s) length |
| `calculate_triangle_area(b, h f64) -> (f64, Error)` | Calculates the area of a triangle with the passed base(b) and height(h) |
| `calculate_circle_area(r f64) -> (f64, Error)` | Calculates the area of a circle with the passed radius(r) |
| `calculate_parallelogram_area(b, h f64) -> (f64, Error)` | Calculates the area of a parallelogram with the passed base(b) and height(h) |
| `calculate_trapezoid_area(b1, b2, h f64) -> (f64, Error)` | Calculates the area of a trapezoid with the passed base1(b1), base2(b2) and heigh(h) |
| `calculate_ellipse_area(r1, r2 f64) -> (f64, Error)` | Calculates the area of an ellipse with the passed radiouses(r1, r2) |
| `calculate_rhombus_area(d1, d2 f64) -> (f64, Error)` | Calculates the area of a rhombus with the passed diagonals(d1, d2) |

#### Metric to Metric

| Function | Description |
|----------|-------------|
| `sq_centimeter_to_sq_meter(val f64) -> (f64, Error)` | Converts the passed value(val) as square centimeters to square meters |
| `sq_meter_to_sq_centimeter(val f64) -> (f64, Error)` | Converts the passed value(val) as square meters to square centimeters |
| `sq_meter_to_sq_kilometer(val f64) -> (f64, Error)` | Converts the passed value(val) as square meters to square kilometers |
| `sq_kilometer_to_sq_meter(val f64) -> (f64, Error)` | Converts the passed value(val) as square kilometers to square meters |
| `sq_meter_to_hectare(val f64) -> (f64, Error)` | Converts the passed value(val) as square meters to hectares |
| `hectare_to_sq_meter(val f64) -> (f64, Error)` | Converts the passed value(val) as hectares to square meters |

#### Imperial to Imperial

| Function | Description |
|----------|-------------|
| `sq_inch_to_sq_foot(val f64) -> (f64, Error)` | Converts the passed value(val) as square inches to square feet |
| `sq_foot_to_sq_inch(val f64) -> (f64, Error)` | Converts the passed value(val) as square feet to square inches |
| `sq_foot_to_sq_yard(val f64) -> (f64, Error)` | Converts the passed value(val) as square feet to square yards |
| `sq_yard_to_sq_foot(val f64) -> (f64, Error)` | Converts the passed value(val) as square yards to square feet |
| `sq_foot_to_acre(val f64) -> (f64, Error)` | Converts the passed value(val) as square feet to acres |
| `acre_to_sq_foot(val f64) -> (f64, Error)` | Converts the passed value(val) as acres to square feet |
| `acre_to_sq_mile(val f64) -> (f64, Error)` | Converts the passed value(val) as acres to square miles |
| `sq_mile_to_acre(val f64) -> (f64, Error)` | Converts the passed value(val) as square miles to acres |

#### Imperial to Metric

| Function | Description |
|----------|-------------|
| `sq_inch_to_sq_centimeter(val f64) -> (f64, Error)` | Converts the passed value(val) as square inches to square centimeters |
| `sq_foot_to_sq_meter(val f64) -> (f64, Error)` | Converts the passed value(val) as square feet to square meters |
| `sq_yard_to_sq_meter(val f64) -> (f64, Error)` | Converts the passed value(val) as square yards to square meters |
| `acre_to_sq_meter(val f64) -> (f64, Error)` | Converts the passed value(val) as acres to square meters |
| `acre_to_hectare(val f64) -> (f64, Error)` | Converts the passed value(val) as acres to hectares |
| `sq_mile_to_sq_kilometer(val f64) -> (f64, Error)` | Converts the passed value(val) as square miles to square kilometers |

#### Metric to Imperial

| Function | Description |
|----------|-------------|
| `sq_centimeter_to_sq_inch(val f64) -> (f64, Error)` | Converts the passed value(val) as square centimeters to square inches |
| `sq_meter_to_sq_foot(val f64) -> (f64, Error)` | Converts the passed value(val) as square meters to square feet |
| `sq_meter_to_sq_yard(val f64) -> (f64, Error)` | Converts the passed value(val) as square meters to square yards |
| `sq_meter_to_acre(val f64) -> (f64, Error)` | Converts the passed value(val) as square meters to acres |
| `hectare_to_acre(val f64) -> (f64, Error)` | Converts the passed value(val) as hectares to acres |
| `sq_kilometer_to_sq_mile(val f64) -> (f64, Error)` | Converts the passed value(val) as square kilometers to square miles |

### Weight Functions

#### Metric to Metric

| Function | Description |
|----------|-------------|
| `convert_milligram_to_gram(val f64) -> (f64, Error)` | Converts the passed milligrams(val) to grams |
| `convert_gram_to_milligram(val f64) -> (f64, Error)` | Converts the passed grams(val) to milligrams |
| `convert_gram_to_kilogram(val f64) -> (f64, Error)` | Converts the passed grams(val) to kilograms |
| `convert_kilogram_to_gram(val f64) -> (f64, Error)` | Converts the passed kilograms(val) to grams |
| `convert_kilogram_to_metric_ton(val f64) -> (f64, Error)` | Converts the passed kilograms(val) to metric tons |
| `convert_metric_ton_to_kilogram(val f64) -> (f64, Error)` | Converts the passed metric tons(val) to kilograms |

#### Imperial to Imperial

| Function | Description |
|----------|-------------|
| `convert_ounce_to_pound(val f64) -> (f64, Error)` | Converts the passed ounces(val) to pounds |
| `convert_pound_to_ounce(val f64) -> (f64, Error)` | Converts the passed pounds(val) to ounces |
| `convert_pound_to_stone(val f64) -> (f64, Error)` | Converts the passed pounds(val) to stone |
| `convert_stone_to_pound(val f64) -> (f64, Error)` | Converts the passed stone(val) to pounds |
| `convert_pound_to_short_ton(val f64) -> (f64, Error)` | Converts the passed pounds(val) to short tons |
| `convert_short_ton_to_pound(val f64) -> (f64, Error)` | Converts the passed short tons(val) to pounds |

#### Imperial to Metric

| Function | Description |
|----------|-------------|
| `convert_ounce_to_gram(val f64) -> (f64, Error)` | Converts the passed ounces(val) to grams |
| `convert_ounce_to_kilogram(val f64) -> (f64, Error)` | Converts the passed ounces(val) to kilograms |
| `convert_pound_to_gram(val f64) -> (f64, Error)` | Converts the passed pounds(val) to grams |
| `convert_pound_to_kilogram(val f64) -> (f64, Error)` | Converts the passed pounds(val) to kilograms |
| `convert_stone_to_kilogram(val f64) -> (f64, Error)` | Converts the passed stone(val) to kilograms |
| `convert_short_ton_to_kilogram(val f64) -> (f64, Error)` | Converts the passed short tons(val) to kilograms |
| `convert_short_ton_to_metric_ton(val f64) -> (f64, Error)` | Converts the passed short tons(val) to metric tons |

#### Metric to Imperial

| Function | Description |
|----------|-------------|
| `convert_gram_to_ounce(val f64) -> (f64, Error)` | Converts the passed grams(val) to ounces |
| `convert_gram_to_pound(val f64) -> (f64, Error)` | Converts the passed grams(val) to pounds |
| `convert_kilogram_to_ounce(val f64) -> (f64, Error)` | Converts the passed kilograms(val) to ounces |
| `convert_kilogram_to_pound(val f64) -> (f64, Error)` | Converts the passed kilograms(val) to pounds |
| `convert_kilogram_to_stone(val f64) -> (f64, Error)` | Converts the passed kilograms(val) to stone |
| `convert_kilogram_to_short_ton(val f64) -> (f64, Error)` | Converts the passed kilograms(val) to short tons |
| `convert_metric_ton_to_short_ton(val f64) -> (f64, Error)` | Converts the passed metric tons(val) to short tons |

### Temperature Functions

#### Celsius

| Function | Description |
|----------|-------------|
| `convert_celsius_to_fahrenheit(val f64) -> (f64, Error)` | Converts the passed degrees Celsius(val) to degrees Fahrenheit |
| `convert_celsius_to_kelvin(val f64) -> (f64, Error)` | Converts the passed degrees Celsius(val) to kelvin |

#### Fahrenheit

| Function | Description |
|----------|-------------|
| `convert_fahrenheit_to_celsius(val f64) -> (f64, Error)` | Converts the passed degrees Fahrenheit(val) to degrees Celsius |
| `convert_fahrenheit_to_kelvin(val f64) -> (f64, Error)` | Converts the passed degrees Fahrenheit(val) to kelvin |

#### Kelvin

| Function | Description |
|----------|-------------|
| `convert_kelvin_to_celsius(val f64) -> (f64, Error)` | Converts the passed kelvin(val) to degrees Celsius |
| `convert_kelvin_to_fahrenheit(val f64) -> (f64, Error)` | Converts the passed kelvin(val) to degrees Fahrenheit |

### Volume Functions

#### Metric to Metric

| Function | Description |
|----------|-------------|
| `convert_milliliter_to_liter(val f64) -> (f64, Error)` | Converts the passed milliliters(val) to liters |
| `convert_liter_to_milliliter(val f64) -> (f64, Error)` | Converts the passed liters(val) to milliliters |
| `convert_liter_to_cubic_meter(val f64) -> (f64, Error)` | Converts the passed liters(val) to cubic meters |
| `convert_cubic_meter_to_liter(val f64) -> (f64, Error)` | Converts the passed cubic meters(val) to liters |

#### Imperial to Imperial

| Function | Description |
|----------|-------------|
| `convert_fluid_ounce_to_cup(val f64) -> (f64, Error)` | Converts the passed fluid ounces(val) to cups |
| `convert_cup_to_fluid_ounce(val f64) -> (f64, Error)` | Converts the passed cups(val) to fluid ounces |
| `convert_cup_to_pint(val f64) -> (f64, Error)` | Converts the passed cups(val) to pints |
| `convert_pint_to_cup(val f64) -> (f64, Error)` | Converts the passed pints(val) to cups |
| `convert_pint_to_quart(val f64) -> (f64, Error)` | Converts the passed pints(val) to quarts |
| `convert_quart_to_pint(val f64) -> (f64, Error)` | Converts the passed quarts(val) to pints |
| `convert_quart_to_gallon(val f64) -> (f64, Error)` | Converts the passed quarts(val) to gallons |
| `convert_gallon_to_quart(val f64) -> (f64, Error)` | Converts the passed gallons(val) to quarts |
| `convert_fluid_ounce_to_gallon(val f64) -> (f64, Error)` | Converts the passed fluid ounces(val) to gallons |
| `convert_gallon_to_fluid_ounce(val f64) -> (f64, Error)` | Converts the passed gallons(val) to fluid ounces |

#### Imperial to Metric

| Function | Description |
|----------|-------------|
| `convert_fluid_ounce_to_milliliter(val f64) -> (f64, Error)` | Converts the passed fluid ounces(val) to milliliters |
| `convert_fluid_ounce_to_liter(val f64) -> (f64, Error)` | Converts the passed fluid ounces(val) to liters |
| `convert_cup_to_milliliter(val f64) -> (f64, Error)` | Converts the passed cups(val) to milliliters |
| `convert_cup_to_liter(val f64) -> (f64, Error)` | Converts the passed cups(val) to liters |
| `convert_pint_to_liter(val f64) -> (f64, Error)` | Converts the passed pints(val) to liters |
| `convert_quart_to_liter(val f64) -> (f64, Error)` | Converts the passed quarts(val) to liters |
| `convert_gallon_to_liter(val f64) -> (f64, Error)` | Converts the passed gallons(val) to liters |
| `convert_gallon_to_milliliter(val f64) -> (f64, Error)` | Converts the passed gallons(val) to milliliters |

#### Metric to Imperial

| Function | Description |
|----------|-------------|
| `convert_milliliter_to_fluid_ounce(val f64) -> (f64, Error)` | Converts the passed milliliters(val) to fluid ounces |
| `convert_liter_to_fluid_ounce(val f64) -> (f64, Error)` | Converts the passed liters(val) to fluid ounces |
| `convert_milliliter_to_cup(val f64) -> (f64, Error)` | Converts the passed milliliters(val) to cups |
| `convert_liter_to_cup(val f64) -> (f64, Error)` | Converts the passed liters(val) to cups |
| `convert_liter_to_pint(val f64) -> (f64, Error)` | Converts the passed liters(val) to pints |
| `convert_liter_to_quart(val f64) -> (f64, Error)` | Converts the passed liters(val) to quarts |
| `convert_liter_to_gallon(val f64) -> (f64, Error)` | Converts the passed liters(val) to gallons |
| `convert_milliliter_to_gallon(val f64) -> (f64, Error)` | Converts the passed milliliters(val) to gallons |

### Speed Functions

#### Metric to Metric

| Function | Description |
|----------|-------------|
| `meters_per_second_to_kilometers_per_hour(val f64) -> (f64, Error)` | Converts the passed meters per second(val) to kilometers per hour |
| `kilometers_per_hour_to_meters_per_second(val f64) -> (f64, Error)` | Converts the passed kilometers per hour(val) to meters per second |

#### Imperial to Imperial

| Function | Description |
|----------|-------------|
| `miles_per_hour_to_feet_per_second(val f64) -> (f64, Error)` | Converts the passed miles per hour(val) to feet per second |
| `feet_per_second_to_miles_per_hour(val f64) -> (f64, Error)` | Converts the passed feet per second(val) to miles per hour |

#### Imperial to Metric

| Function | Description |
|----------|-------------|
| `miles_per_hour_to_kilometers_per_hour(val f64) -> (f64, Error)` | Converts the passed miles per hour(val) to kilometers per hour |
| `miles_per_hour_to_meters_per_second(val f64) -> (f64, Error)` | Converts the passed miles per hour(val) to meters per second |
| `feet_per_second_to_meters_per_second(val f64) -> (f64, Error)` | Converts the passed feet per second(val) to meters per second |
| `feet_per_second_to_kilometers_per_hour(val f64) -> (f64, Error)` | Converts the passed feet per second(val) to kilometers per hour |

#### Metric to Imperial

| Function | Description |
|----------|-------------|
| `kilometers_per_hour_to_miles_per_hour(val f64) -> (f64, Error)` | Converts the passed kilometers per hour(val) to miles per hour |
| `meters_per_second_to_miles_per_hour(val f64) -> (f64, Error)` | Converts the passed meters per second(val) to miles per hour |
| `meters_per_second_to_feet_per_second(val f64) -> (f64, Error)` | Converts the passed meters per second(val) to feet per second |
| `kilometers_per_hour_to_feet_per_second(val f64) -> (f64, Error)` | Converts the passed kilometers per hour(val) to feet per second |

#### Knot

| Function | Description |
|----------|-------------|
| `knot_to_kilometers_per_hour(val f64) -> (f64, Error)` | Converts the passed knots(val) to kilometers per hour |
| `kilometers_per_hour_to_knot(val f64) -> (f64, Error)` | Converts the passed kilometers per hour(val) to knots |
| `knot_to_miles_per_hour(val f64) -> (f64, Error)` | Converts the passed knots(val) to miles per hour |
| `miles_per_hour_to_knot(val f64) -> (f64, Error)` | Converts the passed miles per hour(val) to knots |
| `knot_to_meters_per_second(val f64) -> (f64, Error)` | Converts the passed knots(val) to meters per second |
| `meters_per_second_to_knot(val f64) -> (f64, Error)` | Converts the passed meters per second(val) to knots |

### Data Storage Functions

#### Decimal (SI)

(1 KB = 1000 bytes)

| Function | Description |
|----------|-------------|
| `byte_to_kilobyte(val f64) -> (f64, Error)` | Converts the passed bytes(val) to kilobytes |
| `kilobyte_to_byte(val f64) -> (f64, Error)` | Converts the passed kilobytes(val) to bytes |
| `kilobyte_to_megabyte(val f64) -> (f64, Error)` | Converts the passed kilobytes(val) to megabytes |
| `megabyte_to_kilobyte(val f64) -> (f64, Error)` | Converts the passed megabytes(val) to kilobytes |
| `megabyte_to_gigabyte(val f64) -> (f64, Error)` | Converts the passed megabytes(val) to gigabytes |
| `gigabyte_to_megabyte(val f64) -> (f64, Error)` | Converts the passed gigabytes(val) to megabytes |
| `gigabyte_to_terabyte(val f64) -> (f64, Error)` | Converts the passed gigabytes(val) to terabytes |
| `terabyte_to_gigabyte(val f64) -> (f64, Error)` | Converts the passed terabytes(val) to gigabytes |
| `terabyte_to_petabyte(val f64) -> (f64, Error)` | Converts the passed terabytes(val) to petabytes |
| `petabyte_to_terabyte(val f64) -> (f64, Error)` | Converts the passed petabytes(val) to terabytes |

#### Binary (IEC)

(1 KiB = 1024 bytes)

| Function | Description |
|----------|-------------|
| `byte_to_kibibyte(val f64) -> (f64, Error)` | Converts the passed bytes(val) to kibibytes |
| `kibibyte_to_byte(val f64) -> (f64, Error)` | Converts the passed kibibytes(val) to bytes |
| `kibibyte_to_mebibyte(val f64) -> (f64, Error)` | Converts the passed kibibytes(val) to mebibytes |
| `mebibyte_to_kibibyte(val f64) -> (f64, Error)` | Converts the passed mebibytes(val) to kibibytes |
| `mebibyte_to_gibibyte(val f64) -> (f64, Error)` | Converts the passed mebibytes(val) to gibibytes |
| `gibibyte_to_mebibyte(val f64) -> (f64, Error)` | Converts the passed gibibytes(val) to mebibytes |
| `gibibyte_to_tebibyte(val f64) -> (f64, Error)` | Converts the passed gibibytes(val) to tebibytes |
| `tebibyte_to_gibibyte(val f64) -> (f64, Error)` | Converts the passed tebibytes(val) to gibibytes |
| `tebibyte_to_pebibyte(val f64) -> (f64, Error)` | Converts the passed tebibytes(val) to pebibytes |
| `pebibyte_to_tebibyte(val f64) -> (f64, Error)` | Converts the passed pebibytes(val) to tebibytes |

#### Decimal to Binary

| Function | Description |
|----------|-------------|
| `kilobyte_to_kibibyte(val f64) -> (f64, Error)` | Converts the passed kilobytes(val) to kibibytes |
| `megabyte_to_mebibyte(val f64) -> (f64, Error)` | Converts the passed megabytes(val) to mebibytes |
| `gigabyte_to_gibibyte(val f64) -> (f64, Error)` | Converts the passed gigabytes(val) to gibibytes |
| `terabyte_to_tebibyte(val f64) -> (f64, Error)` | Converts the passed terabytes(val) to tebibytes |

#### Binary to Decimal

| Function | Description |
|----------|-------------|
| `kibibyte_to_kilobyte(val f64) -> (f64, Error)` | Converts the passed kibibytes(val) to kilobytes |
| `mebibyte_to_megabyte(val f64) -> (f64, Error)` | Converts the passed mebibytes(val) to megabytes |
| `gibibyte_to_gigabyte(val f64) -> (f64, Error)` | Converts the passed gibibytes(val) to gigabytes |
| `tebibyte_to_terabyte(val f64) -> (f64, Error)` | Converts the passed tebibytes(val) to terabytes |

### Physics Functions

#### Pressure & Force

| Function | Description |
|----------|-------------|
| `calculate_pressure(force f64, area f64) -> (f64, Error)` | Calculates the pressure from the passed force(force) and area(area) |
| `calculate_force(mass f64, acceleration f64) -> (f64, Error)` | Calculates the force from the passed mass(mass) and acceleration(acceleration) |

#### Acceleration

| Function | Description |
|----------|-------------|
| `calculate_acceleration_from_velocity_time(initialVelocity f64, finalVelocity f64, duration f64) -> (f64, Error)` | Calculates acceleration from the passed initial velocity(initialVelocity), final velocity(finalVelocity) and duration(duration) |
| `calculate_acceleration_from_velocity_distance(initialVelocity f64, finalVelocity f64, distance f64) -> (f64, Error)` | Calculates acceleration from the passed initial velocity(initialVelocity), final velocity(finalVelocity) and distance(distance) |
| `calculate_acceleration_from_initial_time_distance(initialVelocity f64, duration f64, distance f64) -> (f64, Error)` | Calculates acceleration from the passed initial velocity(initialVelocity), duration(duration) and distance(distance) |
| `calculate_acceleration_from_final_time_distance(finalVelocity f64, duration f64, distance f64) -> (f64, Error)` | Calculates acceleration from the passed final velocity(finalVelocity), duration(duration) and distance(distance) |
| `calculate_acceleration_from_rest(duration f64, distance f64) -> (f64, Error)` | Calculates acceleration of an object starting from rest from the passed duration(duration) and distance(distance) |
| `calculate_force_to_acceleration(force f64, mass f64) -> (f64, Error)` | Calculates acceleration from the passed force(force) and mass(mass) |

### Constants

**Length:** `CM_PER_METER`, `METERS_PER_KM`, `INCHES_PER_FOOT`, `FEET_PER_YARD`, `FEET_PER_MILE`, `YARDS_PER_MILE`, `CM_PER_INCH`, `METERS_PER_INCH`, `CM_PER_FOOT`, `METERS_PER_FOOT`, `METERS_PER_YARD`, `METERS_PER_MILE`, `KM_PER_MILE`, `INCHES_PER_CM`, `INCHES_PER_METER`, `FEET_PER_METER`, `YARDS_PER_METER`, `MILES_PER_KM`, `FEET_PER_KM`

**Area:** `SQ_CM_PER_SQ_METER`, `SQ_METERS_PER_SQ_KM`, `SQ_INCHES_PER_SQ_FOOT`, `SQ_FEET_PER_SQ_YARD`, `SQ_FEET_PER_ACRE`, `ACRES_PER_SQ_MILE`, `SQ_CM_PER_SQ_INCH`, `SQ_METERS_PER_SQ_FOOT`, `SQ_METERS_PER_SQ_YARD`, `SQ_METERS_PER_ACRE`, `SQ_KM_PER_SQ_MILE`, `HECTARES_PER_ACRE`, `SQ_INCHES_PER_SQ_CM`, `SQ_FEET_PER_SQ_METER`, `SQ_YARDS_PER_SQ_METER`, `ACRES_PER_SQ_METER`, `SQ_MILES_PER_SQ_KM`, `ACRES_PER_HECTARE`

**Weight:** `MG_PER_GRAM`, `GRAMS_PER_KG`, `KG_PER_METRIC_TON`, `OZ_PER_LB`, `LB_PER_STONE`, `LB_PER_SHORT_TON`, `GRAMS_PER_OZ`, `GRAMS_PER_LB`, `KG_PER_LB`, `KG_PER_STONE`, `KG_PER_SHORT_TON`, `OZ_PER_GRAM`, `LB_PER_KG`, `LB_PER_GRAM`, `STONES_PER_KG`, `SHORT_TONS_PER_KG`

**Temperature:** `CELSIUS_TO_FAHRENHEIT_MULTIPLIER`, `FAHRENHEIT_TO_CELSIUS_MULTIPLIER`, `FAHRENHEIT_OFFSET`, `KELVIN_OFFSET`

**Volume:** `ML_PER_LITER`, `LITERS_PER_CUBIC_METER`, `FL_OZ_PER_CUP`, `CUPS_PER_PINT`, `PINTS_PER_QUART`, `QUARTS_PER_GALLON`, `FL_OZ_PER_GALLON`, `ML_PER_FL_OZ`, `ML_PER_CUP`, `LITERS_PER_PINT`, `LITERS_PER_QUART`, `LITERS_PER_GALLON`, `FL_OZ_PER_ML`, `FL_OZ_PER_LITER`, `CUPS_PER_LITER`, `PINTS_PER_LITER`, `QUARTS_PER_LITER`, `GALLONS_PER_LITER`

**Speed:** `KMH_PER_MPS`, `MPS_PER_KMH`, `FPS_PER_MPH`, `MPH_PER_FPS`, `KMH_PER_MPH`, `MPH_PER_KMH`, `MPS_PER_MPH`, `MPH_PER_MPS`, `MPS_PER_FPS`, `FPS_PER_MPS`, `KMH_PER_KNOT`, `KNOTS_PER_KMH`, `MPH_PER_KNOT`, `KNOTS_PER_MPH`, `MPS_PER_KNOT`, `KNOTS_PER_MPS`

**Data Storage:** `BYTES_PER_KB`, `KB_PER_MB`, `MB_PER_GB`, `GB_PER_TB`, `TB_PER_PB`, `BYTES_PER_MB`, `BYTES_PER_GB`, `BYTES_PER_TB`, `BYTES_PER_PB`, `BYTES_PER_KIB`, `KIB_PER_MIB`, `MIB_PER_GIB`, `GIB_PER_TIB`, `TIB_PER_PIB`, `BYTES_PER_MIB`, `BYTES_PER_GIB`, `BYTES_PER_TIB`, `BYTES_PER_PIB`

**Physics:** `STANDARD_GRAVITY_METERS_PER_SECOND_SQUARED`, `STANDARD_GRAVITY_FEET_PER_SECOND_SQUARED`, `SPEED_OF_LIGHT_METERS_PER_SECOND`, `SPEED_OF_SOUND_METERS_PER_SECOND`, `STANDARD_ATMOSPHERE_PASCALS`, `PASCALS_PER_BAR`, `PASCALS_PER_PSI`, `PASCALS_PER_MILLIMETER_OF_MERCURY`
