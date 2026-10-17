# Geometry Module

Utilities for calculating the volume and surface area of solid shapes, and for working with points, rectangles, circles and line segments on a plane.

## Usage

Simply copy the `geometry` dir or the `geometry.gray` file into your projects structure, Then in the file you plan on using the API write the following at the top of the file:
```gray
import "relative/path/to/geometry"
```

Every function returns an `Error` alongside its value. Check the error before using the value; when an error is returned the value is `-1.0e50`, `false`, or a point with both coordinates `-1.0e50`, and should not be used.

Each struct function takes the shape as its first parameter, so it can be called either as `geometry.Circle.area(circle)` or as `circle.area()`.

### Solid Shapes

```gray
fn main(){
    mut volume, volumeErr = geometry.calculate_sphere_volume(1.0)            // Returns 4.18879
    mut surface, surfaceErr = geometry.calculate_cylinder_surface_area(1.0, 2.0)   // Returns 18.8496
    mut boxVolume, boxVolumeErr = geometry.calculate_box_volume(2.0, 3.0, 4.0)     // Returns 24.0
}
```

### Points

```gray
fn main(){
    mut first geometry.Point2D = geometry.Point2D{x: 0.0, y: 0.0}
    mut second geometry.Point2D = geometry.Point2D{x: 3.0, y: 4.0}

    mut distance, distanceErr = first.distance(second)   // Returns 5.0
    mut middle, middleErr = first.midpoint(second)       // Returns Point2D{x: 1.5, y: 2.0}
}
```

### Rectangles

```gray
fn main(){
    mut room geometry.Rectangle = geometry.Rectangle{x: 0.0, y: 0.0, width: 4.0, height: 3.0}
    mut other geometry.Rectangle = geometry.Rectangle{x: 4.0, y: 3.0, width: 2.0, height: 2.0}
    mut point geometry.Point2D = geometry.Point2D{x: 1.0, y: 1.0}

    mut area, areaErr = room.area()                        // Returns 12.0
    mut perimeter, perimeterErr = room.perimeter()         // Returns 14.0
    mut inside, insideErr = room.contains_point(point)     // Returns true
    mut overlap, overlapErr = room.intersects(other)       // Returns true, they touch at a corner
}
```

### Circles

```gray
fn main(){
    mut circle geometry.Circle = geometry.Circle{x: 0.0, y: 0.0, radius: 5.0}
    mut point geometry.Point2D = geometry.Point2D{x: 3.0, y: 4.0}

    mut area, areaErr = circle.area()                      // Returns 78.5398
    mut circumference, circumferenceErr = circle.circumference()   // Returns 31.4159
    mut inside, insideErr = circle.contains_point(point)   // Returns true, it is on the edge
}
```

### Line Segments

```gray
fn main(){
    mut origin geometry.Point2D = geometry.Point2D{x: 0.0, y: 0.0}
    mut corner geometry.Point2D = geometry.Point2D{x: 4.0, y: 4.0}
    mut top geometry.Point2D = geometry.Point2D{x: 0.0, y: 4.0}
    mut right geometry.Point2D = geometry.Point2D{x: 4.0, y: 0.0}

    mut crosses, crossesErr = geometry.segments_intersect(origin, corner, top, right)   // Returns true
}
```

### Error Handling

```gray
fn main(){
    mut volume, err = geometry.calculate_sphere_volume(-1.0)
    if err != nil {
        println("Error: ${err}")   // radius -1.0 is negative, min is 0.0
    }
}
```

## API Reference

### Solid Shape Functions

| Function | Description |
|----------|-------------|
| `calculate_cube_volume(side f64) -> (f64, Error)` | Calculates the volume of a cube with the passed side(side) length |
| `calculate_cube_surface_area(side f64) -> (f64, Error)` | Calculates the surface area of a cube with the passed side(side) length |
| `calculate_box_volume(length f64, width f64, height f64) -> (f64, Error)` | Calculates the volume of a rectangular box with the passed length(length), width(width) and height(height) |
| `calculate_box_surface_area(length f64, width f64, height f64) -> (f64, Error)` | Calculates the surface area of a rectangular box with the passed length(length), width(width) and height(height) |
| `calculate_sphere_volume(radius f64) -> (f64, Error)` | Calculates the volume of a sphere with the passed radius(radius) |
| `calculate_sphere_surface_area(radius f64) -> (f64, Error)` | Calculates the surface area of a sphere with the passed radius(radius) |
| `calculate_cylinder_volume(radius f64, height f64) -> (f64, Error)` | Calculates the volume of a cylinder with the passed radius(radius) and height(height) |
| `calculate_cylinder_surface_area(radius f64, height f64) -> (f64, Error)` | Calculates the surface area of a cylinder, including both ends, with the passed radius(radius) and height(height) |
| `calculate_cone_volume(radius f64, height f64) -> (f64, Error)` | Calculates the volume of a cone with the passed radius(radius) and height(height) |
| `calculate_cone_surface_area(radius f64, height f64) -> (f64, Error)` | Calculates the surface area of a cone, including its base, with the passed radius(radius) and height(height) |

### Point2D

Has the fields `x` and `y`.

| Function | Description |
|----------|-------------|
| `distance(first Point2D, second Point2D) -> (f64, Error)` | Calculates the distance between the passed points(first, second) |
| `midpoint(first Point2D, second Point2D) -> (Point2D, Error)` | Finds the point halfway between the passed points(first, second) |

### Rectangle

Has the fields `x`, `y`, `width` and `height`. `x` and `y` are the corner with the smallest x and y.

| Function | Description |
|----------|-------------|
| `area(rectangle Rectangle) -> (f64, Error)` | Calculates the area of the passed rectangle(rectangle) |
| `perimeter(rectangle Rectangle) -> (f64, Error)` | Calculates the perimeter of the passed rectangle(rectangle) |
| `contains_point(rectangle Rectangle, point Point2D) -> (bool, Error)` | Checks if the passed point(point) is inside or on the edge of the passed rectangle(rectangle) |
| `intersects(first Rectangle, second Rectangle) -> (bool, Error)` | Checks if the passed rectangles(first, second) overlap. Rectangles that only touch at an edge or corner count as overlapping |

### Circle

Has the fields `x`, `y` and `radius`. `x` and `y` are the center.

| Function | Description |
|----------|-------------|
| `area(circle Circle) -> (f64, Error)` | Calculates the area of the passed circle(circle) |
| `circumference(circle Circle) -> (f64, Error)` | Calculates the circumference of the passed circle(circle) |
| `contains_point(circle Circle, point Point2D) -> (bool, Error)` | Checks if the passed point(point) is inside or on the edge of the passed circle(circle) |
| `intersects(first Circle, second Circle) -> (bool, Error)` | Checks if the passed circles(first, second) overlap. Circles that only touch at one point count as overlapping |

### Line Segment Functions

| Function | Description |
|----------|-------------|
| `segments_intersect(firstStart Point2D, firstEnd Point2D, secondStart Point2D, secondEnd Point2D) -> (bool, Error)` | Checks if the line segment from the passed firstStart to firstEnd crosses or touches the line segment from secondStart to secondEnd |

### Errors

| Code | Returned when |
|------|---------------|
| `GEOMETRY_VAL_NEGATIVE` | a side, length, width, height or radius is negative |
| `GEOMETRY_VAL_NOT_A_NUMBER` | a value is NaN |
| `GEOMETRY_VAL_INFINITE` | a value is infinite |
| `GEOMETRY_VAL_TOO_LARGE` | a value has a magnitude above `1.0e50` |
