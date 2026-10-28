# Tasks-
This is code for tasks and my assignment
task1
#include <stdio.h>

int main() {
    long householdNumber;
    char category;
    float previousReading, currentReading, units, energyCharge, fixedCharge, totalBill;

    float RATE_PER_UNIT = 6.50;
    float FIXED_CHARGE = 50.00;

    printf("Enter Household Number: ");
    scanf("%ld", &householdNumber);

    printf("Enter Category (D for Domestic, C for Commercial): ");
    scanf(" %c", &category);

    printf("Enter Previous Reading: ");
    scanf("%f", &previousReading);

    printf("Enter Current Reading: ");
    scanf("%f", &currentReading);

    units = currentReading - previousReading;

    if (units < 0) {
        units = 0;
    }

    energyCharge = units * RATE_PER_UNIT;
    totalBill = energyCharge + FIXED_CHARGE;

    printf("\n--- BILL SUMMARY ---\n");
    printf("Household ID: %ld\n", householdNumber);
    printf("Category: %c\n", category);
    printf("Units Consumed: %.2f\n", units);
    printf("Energy Charge: Rs. %.2f\n", energyCharge);
    printf("Fixed Charge: Rs. %.2f\n", FIXED_CHARGE);
    printf("Total Amount to Pay: Rs. %.2f\n", totalBill);

    return 0;
}
task2
#include <stdio.h>
#include <limits.h>

/* The meter register is a 16-bit counter: after 65535 it rolls over to 0. */
typedef unsigned short meter_t;

unsigned int consumption(meter_t previous, meter_t current)
{
    /* Unsigned 16-bit arithmetic: the result wraps modulo 65536,
       so a negative difference becomes the correct positive units. */
    return (meter_t)(current - previous);
}

int main(void)
{
    printf("sizeof(meter_t) = %zu bytes, maximum reading = %u\n",
           sizeof(meter_t), (unsigned)USHRT_MAX);
    printf("Consumption: %u units\n", consumption(1200, 1530));
    printf("Consumption: %u units\n", consumption(65400, 120)); /* rolled over */
    return 0;
}
task 3
#include <stdio.h>

/* ---------- CONSTANTS (tariff, changed in one place only) ---------- */
#define SLAB1_UNITS          100      /* first 100 units */
#define SLAB2_UNITS          200      /* units 101 to 200 */
#define RATE1_PAISE            0      /* first slab is free */
#define RATE2_PAISE          225      /* Rs 2.25 per unit */
#define RATE3_PAISE          450      /* Rs 4.50 per unit above 200 */
#define FIXED_CHARGE_PAISE  5000L     /* Rs 50 fixed charge */

/* Returns the bill in paise, so that no rounding error can occur. */
long compute_bill(int units)
{
    long paise = FIXED_CHARGE_PAISE;

    /* Units 1 to 100 */
    if (units > 0) {
        int u = (units < SLAB1_UNITS) ? units : SLAB1_UNITS;
        paise += (long)u * RATE1_PAISE;
    }
    /* Units 101 to 200 */
    if (units > SLAB1_UNITS) {
        int u = (units < SLAB2_UNITS) ? units : SLAB2_UNITS;
        paise += (long)(u - SLAB1_UNITS) * RATE2_PAISE;
    }
    /* Units above 200 */
    if (units > SLAB2_UNITS) {
        paise += (long)(units - SLAB2_UNITS) * RATE3_PAISE;
    }
    return paise;
}

int main(void)
{
    int tests[] = {0, 100, 150, 200, 350};
    int n = sizeof tests / sizeof tests[0];
    int i;

    for (i = 0; i < n; i++) {
        long p = compute_bill(tests[i]);
        printf("%3d units -> Rs %ld.%02ld\n", tests[i], p / 100, p % 100);
    }
    return 0;
}
task 4
#include <stdio.h>

/* Fully parenthesised macros */
#define SAFE_SQUARE(x)        ((x) * (x))
#define MAX(a, b)             (((a) > (b)) ? (a) : (b))
#define RUPEES_TO_PAISE(r)    ((long)((r) * 100 + 0.5))

/* Unsafe version, for comparison */
#define UNSAFE_SQUARE(x)      (x * x)

int main(void)
{
    int val = 5;
    int x = 10, y = 20;
    int i = 5, j = 2;
    double rupees = 50.50;

    printf("--- SQUARE ---\n");
    printf("SAFE_SQUARE(val + 1)   = %d\n", SAFE_SQUARE(val + 1));
    printf("UNSAFE_SQUARE(val + 1) = %d\n", UNSAFE_SQUARE(val + 1));

    printf("\n--- MAX ---\n");
    printf("MAX(x, y) = %d\n", MAX(x, y));

    printf("\n--- Side effect (double evaluation) ---\n");
    printf("Before: i=%d, j=%d\n", i, j);
    int result = MAX(i++, j);
    printf("After MAX(i++, j): result=%d, i=%d, j=%d\n", result, i, j);

    printf("\n--- RUPEES_TO_PAISE ---\n");
    printf("%.2f Rupees = %ld Paise\n", rupees, RUPEES_TO_PAISE(rupees));

    return 0;
}
