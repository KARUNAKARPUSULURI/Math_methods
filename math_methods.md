# Complete JavaScript Math Methods Guide

## 1. Basic Math Properties & Constants

| Property | Description | Value |
|----------|-------------|--------|
| `Math.PI` | Ratio of circumference to diameter | ~3.14159 |
| `Math.E` | Euler's number | ~2.71828 |
| `Math.SQRT2` | Square root of 2 | ~1.41421 |
| `Math.SQRT1_2` | Square root of 1/2 | ~0.70710 |
| `Math.LN2` | Natural logarithm of 2 | ~0.69314 |
| `Math.LN10` | Natural logarithm of 10 | ~2.30258 |
| `Math.LOG2E` | Base 2 logarithm of E | ~1.44269 |
| `Math.LOG10E` | Base 10 logarithm of E | ~0.43429 |

### Constants Usage Examples:

```javascript
// Example 1: Circle calculations
function circleCalculations(radius) {
    return {
        circumference: 2 * Math.PI * radius,
        area: Math.PI * radius ** 2,
        diameter: 2 * radius
    };
}

console.log("Circle (radius=5):", circleCalculations(5));

// Example 2: Exponential growth
function exponentialGrowth(initial, rate, time) {
    return initial * Math.E ** (rate * time);
}

console.log("Investment Growth:", exponentialGrowth(1000, 0.05, 2));
```

## 2. Rounding Methods Reference

| Method | Description | Example |
|--------|-------------|---------|
| `Math.round()` | Round to nearest integer | `Math.round(4.7)` // 5 |
| `Math.floor()` | Round down | `Math.floor(4.7)` // 4 |
| `Math.ceil()` | Round up | `Math.ceil(4.3)` // 5 |
| `Math.trunc()` | Remove decimal part | `Math.trunc(4.7)` // 4 |

### Rounding Examples:

```javascript
// Example 1: Price rounding
function formatPrice(price, currency = 'USD') {
    const rounded = Math.round(price * 100) / 100;
    return new Intl.NumberFormat('en-US', {
        style: 'currency',
        currency: currency
    }).format(rounded);
}

console.log("Formatted Prices:");
console.log(formatPrice(29.995));  // $30.00
console.log(formatPrice(10.2));    // $10.20

// Example 2: Percentage calculations
function calculatePercentages(values) {
    const total = values.reduce((sum, val) => sum + val, 0);
    
    return values.map(value => ({
        value,
        percentage: Math.round((value / total) * 100),
        exactPercentage: (value / total) * 100,
        floor: Math.floor((value / total) * 100),
        ceil: Math.ceil((value / total) * 100)
    }));
}

console.log("Percentages:", calculatePercentages([33, 66, 1]));

// Example 3: Custom rounding function
function roundTo(number, precision) {
    const factor = 10 ** precision;
    return Math.round(number * factor) / factor;
}

console.log("Custom Rounding:");
console.log(roundTo(3.14159, 2));  // 3.14
console.log(roundTo(10.8675, 3));  // 10.868
```

## 3. Arithmetic & Power Methods

| Method | Description | Example |
|--------|-------------|---------|
| `Math.abs()` | Absolute value | `Math.abs(-5)` // 5 |
| `Math.pow()` | Power | `Math.pow(2, 3)` // 8 |
| `Math.sqrt()` | Square root | `Math.sqrt(16)` // 4 |
| `Math.cbrt()` | Cube root | `Math.cbrt(27)` // 3 |
| `Math.sign()` | Number's sign | `Math.sign(-5)` // -1 |

### Arithmetic Examples:

```javascript
// Example 1: Vector calculations
function vectorOperations(x1, y1, x2, y2) {
    const deltaX = Math.abs(x2 - x1);
    const deltaY = Math.abs(y2 - y1);
    
    return {
        distance: Math.sqrt(Math.pow(deltaX, 2) + Math.pow(deltaY, 2)),
        angle: Math.atan2(deltaY, deltaX) * (180 / Math.PI),
        manhattanDistance: deltaX + deltaY
    };
}

console.log("Vector:", vectorOperations(0, 0, 3, 4));

// Example 2: Financial calculations
function loanCalculator(principal, annualRate, years) {
    const monthlyRate = annualRate / 12 / 100;
    const payments = years * 12;
    
    const monthlyPayment = principal * 
        (monthlyRate * Math.pow(1 + monthlyRate, payments)) /
        (Math.pow(1 + monthlyRate, payments) - 1);
    
    return {
        monthlyPayment: roundTo(monthlyPayment, 2),
        totalPayment: roundTo(monthlyPayment * payments, 2),
        totalInterest: roundTo((monthlyPayment * payments) - principal, 2)
    };
}

console.log("Loan Details:", loanCalculator(200000, 3.5, 30));

// Example 3: Scientific calculations
function scientificCalc(value) {
    return {
        squareRoot: Math.sqrt(value),
        cubeRoot: Math.cbrt(value),
        log: Math.log(value),
        log10: Math.log10(value),
        exp: Math.exp(value),
        sign: Math.sign(value)
    };
}

console.log("Scientific Calc:", scientificCalc(16));
```

## 4. Trigonometric Methods

| Method | Description | Example |
|--------|-------------|---------|
| `Math.sin()` | Sine | `Math.sin(Math.PI/2)` |
| `Math.cos()` | Cosine | `Math.cos(Math.PI)` |
| `Math.tan()` | Tangent | `Math.tan(Math.PI/4)` |
| `Math.asin()` | Arcsine | `Math.asin(1)` |
| `Math.acos()` | Arccosine | `Math.acos(-1)` |
| `Math.atan()` | Arctangent | `Math.atan(1)` |

### Trigonometry Examples:

```javascript
// Example 1: Triangle calculations
function triangleCalculator(a, b, angleC_degrees) {
    const angleC_rad = angleC_degrees * Math.PI / 180;
    
    const c = Math.sqrt(
        Math.pow(a, 2) + 
        Math.pow(b, 2) - 
        2 * a * b * Math.cos(angleC_rad)
    );
    
    const angleA_rad = Math.asin(a * Math.sin(angleC_rad) / c);
    const angleB_rad = Math.PI - angleA_rad - angleC_rad;
    
    return {
        sideC: roundTo(c, 4),
        angleA: roundTo(angleA_rad * 180 / Math.PI, 2),
        angleB: roundTo(angleB_rad * 180 / Math.PI, 2),
        area: roundTo(0.5 * a * b * Math.sin(angleC_rad), 4)
    };
}

console.log("Triangle:", triangleCalculator(5, 7, 60));

// Example 2: Circular motion
function circularMotion(radius, angularSpeed, time) {
    const angle = angularSpeed * time;
    
    return {
        x: roundTo(radius * Math.cos(angle), 4),
        y: roundTo(radius * Math.sin(angle), 4),
        tangentialVelocity: roundTo(radius * angularSpeed, 4)
    };
}

console.log("Motion:", circularMotion(10, Math.PI/4, 2));
```

## 5. Random Numbers & Range Methods

| Method | Description | Example |
|--------|-------------|---------|
| `Math.random()` | Random [0,1) | `Math.random()` |
| `Math.max()` | Maximum value | `Math.max(1,2,3)` // 3 |
| `Math.min()` | Minimum value | `Math.min(1,2,3)` // 1 |

### Random & Range Examples:

```javascript
// Example 1: Random number utilities
const random = {
    // Random integer between min and max (inclusive)
    integer(min, max) {
        return Math.floor(Math.random() * (max - min + 1)) + min;
    },
    
    // Random float between min and max
    float(min, max) {
        return Math.random() * (max - min) + min;
    },
    
    // Random boolean with probability
    boolean(probability = 0.5) {
        return Math.random() < probability;
    },
    
    // Random item from array
    fromArray(array) {
        return array[Math.floor(Math.random() * array.length)];
    },
    
    // Generate array of random numbers
    array(length, min, max) {
        return Array.from({ length }, () => this.integer(min, max));
    }
};

console.log("Random Examples:");
console.log("Integer (1-10):", random.integer(1, 10));
console.log("Float (0-1):", random.float(0, 1));
console.log("Boolean (30%):", random.boolean(0.3));
console.log("Array:", random.array(5, 1, 100));

// Example 2: Statistics utilities
function calculateStats(numbers) {
    const sum = numbers.reduce((a, b) => a + b, 0);
    const mean = sum / numbers.length;
    
    const sortedNumbers = [...numbers].sort((a, b) => a - b);
    const middle = Math.floor(sortedNumbers.length / 2);
    
    return {
        min: Math.min(...numbers),
        max: Math.max(...numbers),
        range: Math.max(...numbers) - Math.min(...numbers),
        mean: roundTo(mean, 2),
        median: sortedNumbers.length % 2 === 0
            ? (sortedNumbers[middle - 1] + sortedNumbers[middle]) / 2
            : sortedNumbers[middle],
        sum: sum
    };
}

const numbers = random.array(10, 1, 100);
console.log("Stats:", calculateStats(numbers));

// Example 3: Probability distributions
function normalDistribution(mean = 0, stdDev = 1) {
    // Box-Muller transform
    const u1 = Math.random();
    const u2 = Math.random();
    
    const z0 = Math.sqrt(-2.0 * Math.log(u1)) * Math.cos(2.0 * Math.PI * u2);
    return z0 * stdDev + mean;
}

function generateDistribution(samples, mean, stdDev) {
    return Array.from({ length: samples }, 
        () => roundTo(normalDistribution(mean, stdDev), 2)
    );
}

console.log("Normal Distribution:", 
    calculateStats(generateDistribution(1000, 100, 15))
);
```

## 6. Practical Math Applications

```javascript
// Example 1: 3D distance calculator
function calculate3DDistance(point1, point2) {
    const deltaX = point2.x - point1.x;
    const deltaY = point2.y - point1.y;
    const deltaZ = point2.z - point1.z;
    
    return {
        distance: Math.sqrt(
            Math.pow(deltaX, 2) + 
            Math.pow(deltaY, 2) + 
            Math.pow(deltaZ, 2)
        ),
        components: {
            x: Math.abs(deltaX),
            y: Math.abs(deltaY),
            z: Math.abs(deltaZ)
        }
    };
}

// Example 2: Physics calculations
function projectileMotion(initialVelocity, angle_degrees, height = 0) {
    const g = 9.81; // Gravity acceleration (m/s²)
    const angle_rad = angle_degrees * Math.PI / 180;
    const v0x = initialVelocity * Math.cos(angle_rad);
    const v0y = initialVelocity * Math.sin(angle_rad);
    
    // Time to reach maximum height
    const timeToMax = v0y / g;
    
    // Maximum height
    const maxHeight = height + (v0y * v0y) / (2 * g);
    
    // Time of flight
    const timeOfFlight = (v0y + Math.sqrt(v0y * v0y + 2 * g * height)) / g;
    
    // Range
    const range = v0x * timeOfFlight;
    
    return {
        maxHeight: roundTo(maxHeight, 2),
        range: roundTo(range, 2),
        timeOfFlight: roundTo(timeOfFlight, 2),
        timeToMax: roundTo(timeToMax, 2)
    };
}

// Example 3: Geometric calculations
const geometry = {
    // Regular polygon area
    regularPolygonArea(sides, length) {
        return (sides * Math.pow(length, 2)) / 
            (4 * Math.tan(Math.PI / sides));
    },
    
    // Sphere surface area and volume
    sphereCalculations(radius) {
        return {
            surfaceArea: 4 * Math.PI * Math.pow(radius, 2),
            volume: (4/3) * Math.PI * Math.pow(radius, 3)
        };
    },
    
    // Cylinder calculations
    cylinderCalculations(radius, height) {
        return {
            lateralArea: 2 * Math.PI * radius * height,
            totalArea: 2 * Math.PI * radius * (radius + height),
            volume: Math.PI * Math.pow(radius, 2) * height
        };
    }
};

console.log("3D Distance:", calculate3DDistance(
    {x: 0, y: 0, z: 0}, 
    {x: 3, y: 4, z: 5}
));

console.log("Projectile:", projectileMotion(50, 45, 0));

console.log("Geometry:");
console.log("Pentagon Area:", geometry.regularPolygonArea(5, 10));
console.log("Sphere:", geometry.sphereCalculations(5));
console.log("Cylinder:", geometry.cylinderCalculations(3, 10));
```

These examples demonstrate advanced mathematical calculations and their practical applications, including:
1. Basic arithmetic and rounding operations
2. Trigonometric calculations
3. Statistical analysis
4. Random number generation
5. Physics calculations
6. Geometric computations

Each example includes practical use cases and can be extended or modified based on specific needs.